---
title: Xamarin.ios의 넓은 색
description: 이 문서에서는 광범위 한 색과 Xamarin.ios 또는 Xamarin.ios 앱에서이를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: e7240a271de1f0199c2c9fc045f5c95745eb98c5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031255"
---
# <a name="wide-color-in-xamarinios"></a>Xamarin.ios의 넓은 색

_이 문서에서는 광범위 한 색 및 Xamarin.ios 또는 Xamarin.ios 앱에서이를 사용 하는 방법을 설명 합니다._

iOS 10 및 macOS Sierra는 핵심 그래픽, 핵심 이미지, 금속 및 AVFoundation과 같은 프레임 워크를 포함 하 여 시스템 전체에서 확장 범위 픽셀 형식 및 넓은 색 영역 색 공간에 대 한 지원을 향상 시킵니다. 넓은 색 표시를 사용 하는 장치에 대 한 지원은 전체 그래픽 스택에이 동작을 제공 하 여 추가로 줄어들 됩니다.

## <a name="asset-catalogs"></a>자산 카탈로그

### <a name="supporting-wide-color-with-asset-catalogs"></a>자산 카탈로그를 사용 하 여 넓은 색 지원

IOS 10 및 macOS Sierra에서 Apple은 광범위 한 색을 지원 하기 위해 앱 번들의 고정 이미지 콘텐츠를 포함 하 고 분류 하는 데 사용 되는 자산 카탈로그를 확장 했습니다.

자산 카탈로그를 사용 하면 앱에 다음과 같은 이점이 제공 됩니다.

- 정적 이미지 자산에 가장 적합 한 배포 옵션을 제공 합니다.
- 자동 색 보정을 지원 합니다.
- 자동 픽셀 형식 최적화를 지원 합니다.
- 앱 조각화 및 앱 Thinning를 지원 하 여 관련 된 콘텐츠만 최종 사용자의 장치에 배달 되도록 합니다.

Apple에서는 광범위 한 색 지원을 위해 다음과 같은 자산 카탈로그를 개선 했습니다.

- 16 비트 (색 채널 별) 소스 콘텐츠를 지원 합니다.
- 영역 표시 별 콘텐츠 카탈로그를 지원 합니다. SRGB 또는 디스플레이 P3 gamuts에 대해 콘텐츠에 태그를 지정할 수 있습니다.

개발자에 게는 앱에서 넓은 색 콘텐츠를 지원 하기 위한 세 가지 옵션이 있습니다.

1. **아무 작업도 수행 하지 않음** -앱이 선명한 색을 표시 해야 하는 경우 (사용자 환경을 개선 하는 경우)에만 넓은 색 콘텐츠를 사용 해야 하므로이 요구 사항 외부의 콘텐츠는 그대로 유지 해야 합니다. 모든 하드웨어 환경에서 계속 해 서 올바르게 렌더링 됩니다.
2. **P3 표시를 위해 기존 콘텐츠 업그레이드** -이렇게 하려면 개발자가 자산 카탈로그의 기존 이미지 콘텐츠를 p3 형식의 새로운 업그레이드 된 파일로 바꾸고 자산 편집기에서 이와 같이 태그를 지정 해야 합니다. 빌드 시 이러한 자산에서 sRGB 파생 이미지가 생성 됩니다.
3. **최적화 된 자산 콘텐츠 제공** -이 경우 개발자는 자산 카탈로그의 각 이미지에 대 한 8 비트 sRGB 및 16 비트 표시 P3 비전을 제공 합니다 (자산 편집기에 올바르게 태그가 지정 됨).

### <a name="asset-catalog-deployment"></a>자산 카탈로그 배포

다음은 개발자가 넓은 색 이미지 콘텐츠를 포함 하는 자산 카탈로그를 사용 하 여 앱을 앱 스토어에 제출할 때 발생 합니다.

- 앱이 최종 사용자에 게 배포 될 때 앱 조각화는 적절 한 콘텐츠 변종이 사용자의 장치에만 배달 되도록 합니다.
- 넓은 색을 지원 하지 않는 장치에서는 넓은 색 콘텐츠를 포함 하기 위한 페이로드 비용이 없습니다. 장치에는 제공 되지 않습니다.
- macOS Sierra 이상에서 `NSImage` 하면 하드웨어 표시에 대 한 최상의 콘텐츠 표현이 자동으로 선택 됩니다.
- 표시 된 콘텐츠는 장치 하드웨어의 특징 변경에 의해 자동으로 새로 고쳐집니다.

### <a name="asset-catalog-storage"></a>Asset Catalog 저장소

Asset Catalog 저장소에는 앱에 대 한 다음과 같은 속성 및 의미가 있습니다.

- 빌드 시 Apple은 고화질 이미지 변환을 통해 이미지 콘텐츠의 저장소를 최적화 하려고 시도 합니다.
- 16 비트는 넓은 색 콘텐츠에 대해 색 채널당 사용 됩니다.
- 콘텐츠 종속 이미지 압축은 인도품 콘텐츠 크기를 낮추는 데 사용 됩니다. 콘텐츠 크기를 최적화 하기 위해 새로운 "손실" 압축 추가 되었습니다.

## <a name="rendering-off-screen-images-in-app"></a>앱에서 화면 이미지 렌더링

생성 되는 앱의 유형에 따라 사용자가 인터넷에서 수집한 이미지 콘텐츠를 포함 하거나, 응용 프로그램 내에서 직접 이미지 콘텐츠를 만들 수 있습니다 (예: 벡터 그리기 앱).

이러한 두 경우 모두 앱은 iOS 및 macOS에 추가 된 향상 된 기능을 사용 하 여 필수 이미지를 넓은 색으로 렌더링할 수 있습니다.

### <a name="drawing-wide-color-in-ios"></a>IOS에서 넓은 색 그리기

IOS 10에서 넓은 색 이미지를 올바르게 그리는 방법에 대해 설명 하기 전에 다음과 같은 일반적인 iOS 그리기 코드를 살펴보세요.

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    UIGraphics.BeginImageContext (size);

    ...

    UIGraphics.EndImageContext ();
    return image = UIGraphics.GetImageFromCurrentImageContext ();
    }
```

와이드 색 이미지를 그리는 데 사용 _하기 전에_ 해결 해야 하는 표준 코드에 문제가 있습니다. IOS 이미지 그리기를 시작 하는 데 사용 되는 `UIGraphics.BeginImageContext (size)` 방법에는 다음과 같은 제한 사항이 있습니다.

- 색 채널 당 8 비트 이상으로 이미지 컨텍스트를 만들 수 없습니다.
- 확장 범위 sRGB 색 공간의 색을 나타낼 수 없습니다.
- 백그라운드에서 하위 수준 C 루틴이 호출 되기 때문에 비 sRGB 색 공간에 컨텍스트를 만드는 인터페이스를 제공할 수 없습니다.

이러한 제한 사항을 처리 하 고 iOS 10에서 넓은 색 이미지를 그리려면 다음 코드를 대신 사용 합니다.

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    var render = new UIGraphicsImageRenderer (size);

    var image = render.CreateImage ((UIGraphicsImageRendererContext context) => {
        var bounds = context.Format.Bounds;
        CGRect slice;
        CGRect remainder;
        bounds.Divide (bounds.Width / 2, CGRectEdge.MinXEdge, out slice, out remainder);

        var redP3 = UIColor.FromDisplayP3 (1.0F, 0.0F, 0.0F, 1.0F);
        redP3.SetColor ();
        context.FillRect (slice);

        var redRGB = UIColor.Red;
        redRGB.SetColor ();
        context.FillRect (remainder);
    });

    // Return results
    return image;
}
```

새 `UIGraphicsImageRenderer` 클래스는 확장 범위 sRGB 색 공간을 처리할 수 있는 새 이미지 컨텍스트를 만들며 다음과 같은 기능을 제공 합니다.

- 모든 색은 기본적으로 관리 됩니다.
- 기본적으로 확장 범위 sRGB 색 공간을 지원 합니다.
- 앱이 실행 되 고 있는 iOS 장치의 기능에 따라 sRGB 또는 확장 범위 sRGB 색 공간에서 렌더링 해야 하는지 지능적으로 결정 합니다.
- 개발자가 begin 및 end 컨텍스트 명령 호출에 대해 걱정할 필요가 없도록`CGContext`(이미지 컨텍스트) 수명을 완전히 및 자동으로 관리 합니다.
- `UIGraphics.GetCurrentContext()` 메서드와 호환 됩니다.

`UIGraphicsImageRenderer` 클래스의 `CreateImage` 메서드를 호출 하 여 넓은 색 이미지를 만들고, 그릴 이미지 컨텍스트와 함께 완료 처리기를 전달 합니다. 모든 그리기는 다음과 같이이 완료 처리기 내에서 수행 됩니다.

- `UIColor.FromDisplayP3` 메서드는 넓은 색 영역 디스플레이 P3 색 공간에 완전히 포화 된 새 빨강 색을 만들며 사각형의 처음 절반을 그리는 데 사용 됩니다. 
- 사각형의 두 번째 절반은 비교를 위해 일반 sRGB 완전 채도 빨강 색으로 그려집니다.

### <a name="drawing-wide-color-in-macos"></a>MacOS에서 넓은 색 그리기

`NSImage` 클래스는 넓은 색 이미지 그리기를 지원 하기 위해 macOS Sierra 확장 되었습니다. 예를 들면,

```csharp
var size = CGSize(250,250);
var wideColorImage = new NSImage(size, false, (drawRect) =>{
    var rects = drawRect.Divide(drawRect.Size.Width/2,CGRect.MinXEdge);

    var color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    var path = new NSBezierPath(rects.Slice).Fill();

    color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    path = new NSBezierPath(rects.Remainder).Fill();

    // Return modified
    return true;
});
```

## <a name="rendering-on-screen-images-in-app"></a>앱에서 화면 이미지 렌더링

와이드 컬러 이미지를 화면에 렌더링 하기 위해이 프로세스는 위에 제공 된 macOS 및 iOS에 대 한 화면에 있는 넓은 색 이미지를 그리는 것과 유사 하 게 작동 합니다.

### <a name="rendering-on-screen-in-ios"></a>IOS에서 화면에 렌더링

앱이 iOS에서 화면에 와이드 컬러 이미지를 렌더링 해야 하는 경우 일반적인 방법으로 `UIView`의 `Draw` 메서드를 재정의 합니다. 예를 들면,

```csharp
using System;
using UIKit;
using CoreGraphics;

namespace MonkeyTalk
{
    public class MonkeyView : UIView
    {
        public MonkeyView ()
        {
        }

        public override void Draw (CGRect rect)
        {
            base.Draw (rect);

            // Draw the image to screen
        ...
        }
    }
}
```

IOS 10은 위에 표시 된 `UIGraphicsImageRenderer` 클래스를 사용 하는 경우 `Draw` 메서드가 호출 될 때 앱이 실행 되는 iOS 장치의 기능에 따라 sRGB 또는 확장 범위 sRGB 색 공간에 렌더링 해야 하는지 여부를 지능적으로 결정 합니다. 또한 `UIImageView`는 iOS 9.3 이후 색으로도 관리 되었습니다.

앱이 `UIView` 또는 `UIViewController`에서 렌더링을 수행 하는 방법을 알고 있어야 하는 경우 `UITraitCollection` 클래스의 새 `DisplayGamut` 속성을 확인할 수 있습니다. 이 값은 다음의 `UIDisplayGamut` 열거형입니다.

- P3
- Srgb
- 지정 되지 않은

응용 프로그램이 이미지를 그리는 데 사용 되는 색 공간을 제어 하려는 경우에는 `CALayer`의 새 `ContentsFormat` 속성을 사용 하 여 원하는 색 공간을 지정할 수 있습니다. 이 값은 다음과 같은 `CAContentsFormat` 열거형 일 수 있습니다.

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>MacOS에서 화면에 렌더링

응용 프로그램에서 macOS의 화면을 컬러로 이미지를 렌더링 해야 하는 경우 일반적인 방법으로 해당 `NSView`의 `DrawRect` 메서드를 재정의 합니다. 예를 들면,

```csharp
using System;
using AppKit;
using CoreGraphics;
using CoreAnimation;

namespace MonkeyTalkMac
{
    public class MonkeyView : NSView
    {
        public MonkeyView ()
        {
        }

        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Draw graphics into the view
            ...
        }
    }
}
```

다시 말하지만, `DrawRect` 메서드가 호출 될 때 앱이 실행 되는 Mac 하드웨어의 기능을 기반으로 sRGB 또는 확장 범위 sRGB 색 공간에 렌더링 해야 하는지 여부를 지능적으로 결정 합니다.

응용 프로그램이 이미지를 그리는 데 사용 되는 색 공간을 제어 하려는 경우 `NSWindow` 클래스의 새 `DepthLimit` 속성을 사용 하 여 원하는 색 공간을 지정할 수 있습니다. 이 값은 다음과 같은 `NSWindowDepth` 열거형 일 수 있습니다.

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb
