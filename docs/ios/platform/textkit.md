---
title: Xamarin.iOS에 TextKit
description: 이 문서에서는 TextKit Xamarin.iOS에서 사용 하는 방법을 설명 합니다. TextKit 강력한 텍스트 레이아웃 및 렌더링 기능을 제공합니다.
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: ac80d1d07f5649d377dd6fdefcb4911ba9ec2dcb
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788337"
---
# <a name="textkit-in-xamarinios"></a>Xamarin.iOS에 TextKit

TextKit 강력한 텍스트 레이아웃 및 렌더링 기능을 제공 하는 새로운 API입니다. 하위 수준 코어 텍스트 프레임 워크 위에 빌드됩니다. 따라서 하 있지만 핵심 텍스트 보다 사용 하기가 훨씬 쉽습니다.

TextKit의 기능을 표준 컨트롤에 사용할 수 있도록 하려면 여러 iOS 텍스트 컨트롤 TextKit, 사용 하도록 다시 구현 되었을 포함 하 여:

-  UITextView
-  UITextField
-  UILabel

## <a name="architecture"></a>아키텍처

TextKit 레이아웃과 다음 클래스를 포함 하 여 디스플레이에서 텍스트 저장소를 구분 하는 계층화 된 아키텍처를 제공 합니다.

-  `NSTextContainer` – 좌표계 및 레이아웃 텍스트에 사용 되는 기 하 도형을 제공 합니다.
-  `NSLayoutManager` – 문자 모양에 텍스트를 설정 하 여 텍스트 배치 합니다. 
-  `NSTextStorage` – 일괄 처리 텍스트의 속성 업데이트 처리 뿐만 아니라 텍스트 데이터를 보유 합니다. 모든 일괄 처리 업데이트는 레이아웃을 다시 계산 하 고 텍스트를 다시 그리기 같은 변경 내용의 실제 처리에 대 한 레이아웃 관리자에 게 전달 됩니다.


이러한 세 클래스는 텍스트를 렌더링 하는 보기에 적용 됩니다. 기본 제공 텍스트 보기와 같은 처리 `UITextView`, `UITextField`, 및 `UILabel` 설정에 이미 있지만 만들고에 적용할 수 있습니다 `UIView` 인스턴스.

다음 그림에서는이 아키텍처를 보여 줍니다.

 ![](textkit-images/textkitarch.png "이 그림 TextKit 아키텍처를 보여 줍니다.")

## <a name="text-storage-and-attributes"></a>텍스트를 저장 및 특성

`NSTextStorage` 뷰로 표시 되는 텍스트를 포함 하는 클래스입니다. 또한 변경 내용을 표시 하기 위한 레이아웃 관리자--문자 변경 하는 텍스트 또는 해당 특성을 통신합니다. `NSTextStorage` 상속 `MSMutableAttributed` 텍스트 특성을 일괄 처리 사이에서 지정할 수를 변경할 수 있도록 문자열 `BeginEditing` 및 `EndEditing` 호출 합니다.

예를 들어 다음 코드 조각 지정은 전경으로 변경 하 고 배경 색, 각각 하 고 특정 범위를 대상으로 지정:

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

후 `EndEditing` 은 호출 변경 다시 모든 필요한 레이아웃 및 보기에 표시할 텍스트에 대 한 렌더링 계산을 수행 하는 레이아웃 관리자에 게 전송 됩니다.

## <a name="layout-with-exclusion-path"></a>제외 경로 있는 레이아웃

레이아웃을 지원 하며 라는 다중 열 텍스트와 지정 된 경로 주위의 이동을 텍스트와 같은 복잡 한 시나리오에 대 한 허용 TextKit *제외 경로*합니다. 제외 경로 지정된 된 경로 주위에 텍스트가 텍스트 레이아웃의 기 하 도형을 수정 하는 텍스트 컨테이너에 적용 됩니다.

제외 경로는 추가 하려면 설정 해야는 `ExclusionPaths` 레이아웃 관리자에는 속성입니다. 이 속성을 설정 하면 레이아웃 관리자 하 텍스트 레이아웃을 무효화 하 고 제외 경로 앞뒤 텍스트를 전달 합니다.

### <a name="exclusion-based-on-a-cgpath"></a>CGPath 기준으로 제외

다음 사항을 고려 `UITextView` 하위 클래스 구현:

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

이 코드는 코어 그래픽을 사용 하 여 텍스트 보기에 그리기에 대 한 지원을 추가 합니다. 이후는 `UITextView` 클래스 이제 TextKit 텍스트 렌더링 및 레이아웃에 대해 사용 하도록 기본 제공, TextKit 제외 경로 설정 하는 등의 모든 기능을 사용할 수 있습니다.

> [!IMPORTANT]
> 이 예에서는 하위 클래스 `UITextView` 그리기 지원 터치를 추가 합니다. 서브클래싱 `UITextView` TextKit의 기능을 가져올 필요가 없습니다.



사용자는 그려지는 텍스트 보기에 그릴 후 `CGPath` 에 적용 되는 `UIBezierPath` 설정 하 여 인스턴스는 `UIBezierPath.CGPath` 속성:

```csharp
bezierPath.CGPath = exclusionPath;
```

텍스트 레이아웃 경로 업데이트에서는 코드의 다음 줄을 업데이트 합니다.

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

다음 스크린 샷에서 텍스트 레이아웃 그려지는 경로 주위에 어떻게 변경 되는지 보여 줍니다.

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")--> 
![](textkit-images/exclusionpath2.png "이 스크린샷은 텍스트 레이아웃 그려지는 경로 주위에 어떻게 변경 되는지 보여 줍니다.")

다음에 유의 레이아웃 관리자 `AllowsNonContiguousLayout` 이 경우 속성이 false로 설정 됩니다. 이렇게 하면 텍스트가 변경 된 모든 사례에 대해 다시 계산 해야 레이아웃 됩니다. 이 속성을 true로 설정 큰 문서의 경우 특히 전체 레이아웃 새로 고침을 방지 하 여 성능 이점을 얻을 수 있습니다. 그러나 설정 `AllowsNonContiguousLayout` 를 true 없게 제외 경로 예를 들어 일부 경우-레이아웃 업데이트의 경로 설정 하기 전에 후행 캐리지 리턴 없이 런타임 시 텍스트를 입력 하면 됩니다.


## <a name="related-links"></a>관련 링크

- [IOS 7 (샘플) 소개](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 사용자 인터페이스 개요](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
