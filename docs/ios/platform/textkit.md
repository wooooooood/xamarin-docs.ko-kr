---
title: Xamarin.ios의 TextKit
description: 이 문서에서는 Xamarin.ios에서 TextKit를 사용 하는 방법을 설명 합니다. TextKit은 강력한 텍스트 레이아웃 및 렌더링 기능을 제공 합니다.
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 6c1dee464de1f7ba708b1f7d60affc1616e71ee9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031415"
---
# <a name="textkit-in-xamarinios"></a>Xamarin.ios의 TextKit

TextKit은 강력한 텍스트 레이아웃 및 렌더링 기능을 제공 하는 새로운 API입니다. 하위 수준 핵심 텍스트 프레임 워크를 기반으로 구축 되었지만 핵심 텍스트 보다 훨씬 더 쉽게 사용할 수 있습니다.

TextKit의 기능을 표준 컨트롤에서 사용할 수 있도록 하기 위해 다음을 비롯 하 여 TextKit를 사용 하도록 몇 가지 iOS 텍스트 컨트롤이 다시 구현 되었습니다.

- UITextView
- UITextField
- UILabel

## <a name="architecture"></a>아키텍처

TextKit는 다음 클래스를 비롯 하 여 레이아웃과 표시에서 텍스트 저장소를 구분 하는 계층화 된 아키텍처를 제공 합니다.

- `NSTextContainer` – 텍스트 레이아웃에 사용 되는 좌표계 및 기 하 도형을 제공 합니다.
- `NSLayoutManager` – 텍스트를 문자 모양으로 전환 하 여 텍스트를 배치 합니다.
- `NSTextStorage` – 텍스트 데이터를 포함 하 고 일괄 처리 텍스트 속성 업데이트를 처리 합니다. 일괄 처리 업데이트는 레이아웃을 다시 계산 하 고 텍스트를 다시 그리는 등의 실제 변경 내용 처리를 위해 레이아웃 관리자에 전달 됩니다.

이러한 세 클래스는 텍스트를 렌더링 하는 뷰에 적용 됩니다. `UITextView`, `UITextField`및 `UILabel`와 같은 기본 제공 텍스트 처리 뷰는 이미 설정 되어 있지만 `UIView` 인스턴스에도 만들고 적용할 수 있습니다.

다음 그림은이 아키텍처를 보여 줍니다.

 ![](textkit-images/textkitarch.png "This figure illustrates the TextKit architecture")

## <a name="text-storage-and-attributes"></a>텍스트 저장소 및 특성

`NSTextStorage` 클래스에는 뷰에 표시 되는 텍스트가 포함 됩니다. 또한 텍스트에 대 한 변경 내용 (예: 문자 또는 특성의 변경)을 레이아웃 관리자에 게 표시 하기 위해 전달 합니다. `NSTextStorage`는 `MSMutableAttributed` 문자열에서 상속 되므로 `BeginEditing`와 `EndEditing` 호출 사이에 일괄 처리로 텍스트 특성을 지정할 수 있습니다.

예를 들어, 다음 코드 조각에서는 전경색과 배경색을 각각 변경 하 고 특정 범위를 대상으로 지정 합니다.

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

`EndEditing`가 호출 되 면 변경 내용이 레이아웃 관리자에 게 전송 되 고,이는 뷰에 표시 되는 텍스트에 필요한 레이아웃과 렌더링 계산을 수행 합니다.

## <a name="layout-with-exclusion-path"></a>제외 경로를 포함 하는 레이아웃

TextKit는 레이아웃도 지원 하며, 여러 열 텍스트와 같은 복잡 한 시나리오와 *제외 경로*라는 지정 된 경로 주위에 텍스트 흐름이 가능 합니다. 텍스트 컨테이너에 제외 경로를 적용 하 여 텍스트 레이아웃의 기 하 도형을 수정 하면 지정 된 경로 주위에 텍스트가 흐르게 됩니다.

제외 경로를 추가 하려면 레이아웃 관리자에서 `ExclusionPaths` 속성을 설정 해야 합니다. 이 속성을 설정 하면 레이아웃 관리자가 텍스트 레이아웃을 무효화 하 고 제외 경로 주위에서 텍스트를 흐르게 됩니다.

### <a name="exclusion-based-on-a-cgpath"></a>CGPath 기반 제외

다음 `UITextView` 하위 클래스 구현을 고려 합니다.

```csharp
public class ExclusionPathView : UITextView
{
    CGPath exclusionPath;
    CGPoint initialPoint;
    CGPoint latestPoint;
    UIBezierPath bezierPath;

    public ExclusionPathView (string text)
    {
        Text = text;
        ContentInset = new UIEdgeInsets (20, 0, 0, 0);
        BackgroundColor = UIColor.White;
        exclusionPath = new CGPath ();
        bezierPath = UIBezierPath.Create ();

        LayoutManager.AllowsNonContiguousLayout = false;
    }

    public override void TouchesBegan (NSSet touches, UIEvent evt)
    {
        base.TouchesBegan (touches, evt);

        var touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt)
    {
        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }

    public override void TouchesEnded (NSSet touches, UIEvent evt)
    {
        base.TouchesEnded (touches, evt);

        bezierPath.CGPath = exclusionPath;
        TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
    }

    public override void Draw (CGRect rect)
    {
        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            using (var g = UIGraphics.GetCurrentContext ()) {

                g.SetLineWidth (4);
                UIColor.Blue.SetStroke ();

                if (exclusionPath.IsEmpty) {
                    exclusionPath.AddLines (new CGPoint[] { initialPoint, latestPoint });
                } else {
                    exclusionPath.AddLineToPoint (latestPoint);
                }

                g.AddPath (exclusionPath);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
}
```

이 코드는 핵심 그래픽을 사용 하 여 텍스트 뷰에 그리기 위한 지원을 추가 합니다. `UITextView` 클래스는 이제 텍스트 렌더링 및 레이아웃에 대해 TextKit를 사용 하도록 빌드 되었으므로 제외 경로 설정과 같은 TextKit의 모든 기능을 사용할 수 있습니다.

> [!IMPORTANT]
> 이 예제 서브 클래스 `UITextView` 터치 그리기 지원을 추가 합니다. TextKit의 기능을 얻기 위해 `UITextView` 서브클래싱은 필요 하지 않습니다.

사용자가 텍스트 뷰를 그린 후에는 `UIBezierPath.CGPath` 속성을 설정 하 여 그린 `CGPath` `UIBezierPath` 인스턴스에 적용 됩니다.

```csharp
bezierPath.CGPath = exclusionPath;
```

다음 코드 줄을 업데이트 하면 경로 주위에 텍스트 레이아웃 업데이트가 수행 됩니다.

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

다음 스크린 샷에서는 그려지는 경로를 중심으로 텍스트 레이아웃을 변경 하는 방법을 보여 줍니다.

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")-->
![](textkit-images/exclusionpath2.png "This screenshot illustrates how the text layout changes to flow around the drawn path")

이 경우 레이아웃 관리자의 `AllowsNonContiguousLayout` 속성이 false로 설정 되어 있습니다. 이렇게 하면 텍스트가 변경 되는 모든 경우에 레이아웃을 다시 계산 합니다. 이를 true로 설정 하면 전체 레이아웃 새로 고침을 방지 하 여 성능이 향상 될 수 있습니다. 특히 문서가 큰 경우에는 더욱 그렇습니다. 그러나 `AllowsNonContiguousLayout`를 true로 설정 하면 일부 환경에서 제외 경로에서 레이아웃을 업데이트할 수 없습니다. 예를 들어, 경로를 설정 하기 전에 후행 캐리지 리턴 없이 런타임에 텍스트를 입력 하는 경우입니다.

## <a name="related-links"></a>관련 링크

- [IOS 7 소개 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [iOS 7 사용자 인터페이스 개요](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
