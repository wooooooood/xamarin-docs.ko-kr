---
title: Xamarin.iOS에서 광범위 한 색
description: 이 문서는 광범위 한 색 및 어떻게 Xamarin.iOS 또는 Xamarin.Mac 앱에서 사용할 수를 설명 합니다.
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 523aa1a97e1c536b5afbdb66c52f605382fa338d
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67268798"
---
# <a name="wide-color-in-xamarinios"></a>Xamarin.iOS에서 광범위 한 색

_이 문서에서는 광범위 한 색 및 어떻게 Xamarin.iOS 또는 Xamarin.Mac 앱에서 사용할 수 있습니다를 다룹니다._

iOS 10과 macOS Sierra 확장 범위 픽셀 형식 및 AVFoundation 핵심 그래픽, Core 이미지, 금속 등 프레임 워크를 비롯 한 시스템 전체에서 광범위 한 색상 공간에 대 한 지원을 향상 시킵니다. 와이드 컬러 디스플레이 사용 하 여 장치에 대 한 지원은 전체 그래픽 스택 전체에서이 동작을 제공 하 여 용이 추가 됩니다.

## <a name="asset-catalogs"></a>자산 카탈로그

### <a name="supporting-wide-color-with-asset-catalogs"></a>자산 카탈로그를 사용 하 여 와이드 컬러 지원

IOS 10과 macOS Sierra에서 Apple 와이드 컬러를 지원 하기 위해 자산 카탈로그를 포함 하 고 앱의 번들에 정적 이미지 콘텐츠를 분류 하는 데 확장 되었습니다.

사용 하 여 자산 카탈로그 앱에 다음과 같은 이점을 제공 합니다.

- 정적 이미지 자산에 대 한 가장 좋은 배포 옵션을 제공합니다.
- 자동 색 보정을 지원합니다.
- 이러한 자동 픽셀 형식 최적화를 지원 합니다.
- 앱 및 앱 관련 get 된 콘텐츠만 최종 사용자의 장치에 배달 되도록 보장 하는 축소 지원 합니다.

Apple 와이드 컬러 지원에 대 한 자산 카탈로그에 다음과 같은 향상 기능이 했습니다.

- 16 비트 색 채널당 원본 콘텐츠를 지원합니다.
- 카탈로그 콘텐츠가 표시 영역으로 지원합니다. 콘텐츠는 sRGB 또는 디스플레이 P3 색상에 대 한 태그를 지정할 수 있습니다.

개발자가 자신의 앱에서 지원 되는 광범위 한 색 내용에 대 한 세 가지 옵션:

1. **아무 작업도 수행** -이 요구 사항은 외부에서 내용으로 두어야 와이드 컬러 콘텐츠 앱 선명한 색 (여기서는 사용자의 환경을 개선)를 표시 해야 하는 경우에만 사용 해야, 하므로-됩니다. 모든 하드웨어 상황에서 올바르게 렌더링 되어야 하는 것이 계속 됩니다.
2. **디스플레이 P3를 업그레이드 하는 기존 콘텐츠** -개발자가 자산 카탈로그의 기존 이미지 콘텐츠를 P3 형식으로 업그레이드 된 새 파일을 바꾸고 그러한 자산 편집기에서 태그가 필요 합니다. 빌드 시 sRGB 파생 이미지를 이러한 자산에서 생성 됩니다.
3. **자산 콘텐츠 액세스에 최적화 된 제공** -이 경우 개발자는 8 비트 sRGB 및 16 비트 디스플레이 P3 비전 (제대로 자산 편집기에서 태그 지정)은 자산 카탈로그의 각 이미지의 제공 됩니다.

### <a name="asset-catalog-deployment"></a>자산 카탈로그 배포

다음은 개발자가 와이드 컬러 이미지 콘텐츠를 포함 하는 자산 카탈로그를 사용 하 여 앱 스토어에 앱을 제출 하는 경우 발생 합니다.

- 최종 사용자에 게 앱을 배포 하면 앱 조각화 하면 사용자의 장치에 적절 한 콘텐츠 variant를 배달 합니다.
- 와이드 컬러를 지원 하지 않는 장치에서 비용은 없습니다 페이로드 와이드 컬러 콘텐츠 포함에 대 한 대로 장치에 제공 되지 않습니다.
- `NSImage` macos Sierra (이상) 하드웨어의 표시에 대 한 최상의 콘텐츠 표현을 자동으로 선택 됩니다.
- 표시 된 장치 하드웨어 디스플레이 특성 변경 하는 경우 자동으로 새로 고칠 수 됩니다.

### <a name="asset-catalog-storage"></a>자산 카탈로그 저장소

자산 카탈로그 저장소에는 다음 속성 및 앱에 미치는 영향에 있습니다.

- 빌드 시 Apple를 통해 고품질 이미지 변환은 이미지 콘텐츠 저장소를 최적화 하려고 합니다.
- 16 비트 와이드 컬러 콘텐츠에 대 한 색 채널당 사용 됩니다.
- 콘텐츠를 배달할 콘텐츠 크기를 줄이는 종속 이미지 압축을 사용 합니다. 새 "손실" 압축 콘텐츠 크기를 최적화 하기 위해 추가 되었습니다.

## <a name="rendering-off-screen-images-in-app"></a>앱에서 화면 외부 이미지를 렌더링합니다.

만들려는 앱의 유형에 따라 해당 이미지 콘텐츠는 인터넷에서 수집한 만들거나 (예: 벡터 앱을 예를 들어 드로잉) 앱 내에서 직접 이미지 콘텐츠를 포함 하도록 사용자를 수 있습니다.

두이 경우 모두 앱 렌더링할 수는 필요한 이미지 iOS 및 macOS에 추가 된 향상 된 기능을 사용 하 여 광범위 한 색에서 화면 밖에.

### <a name="drawing-wide-color-in-ios"></a>IOS에서 광범위 한 색을 그리기

IOS 10에서에서 와이드 컬러 이미지를 올바르게 그리는 방법에 설명 하기 전에 다음 일반적인 iOS 그리기 코드를 살펴보십시오.

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

해결 해야 하는 표준 코드를 사용 하 여 문제가 _하기 전에_ 와이드 컬러 이미지를 그릴 수 있습니다. `UIGraphics.BeginImageContext (size)` iOS 이미지 그리기를 시작 하는 데 사용 되는 메서드는 다음과 같은 제한이 있습니다.

- 이미지 컨텍스트가 색 채널당 8 비트 이상을 사용 하 여 만들 수 없습니다.
- 확장 범위 sRGB 색 공간에서에서 색을 나타낼 수 없습니다.
- 백그라운드에서 호출 되는 하위 수준 C 루틴으로 인해 비 sRGB 색 공간에서 컨텍스트를 만들기에 대 한 인터페이스를 제공할 수가 없습니다.

이러한 한계를 처리 하 고 iOS 10에서에서 와이드 컬러 이미지를 그리기를 하려면 다음 코드를 대신 사용 합니다.

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

새 `UIGraphicsImageRenderer` 클래스 확장 범위 sRGB 색 공간을 처리할 수 있는 새 이미지 컨텍스트를 만들고 다음 기능이 포함 되어 있습니다.

- 기본적으로 관리 되는 색을 완벽 하 게 합니다.
- 기본적으로 확장 범위 sRGB 색 공간을 지원합니다.
- 지능적으로 sRGB 또는 확장 범위 sRGB 색 공간을 앱에서 실행 되는 iOS 장치 기능에 따라 렌더링 해야 하는 경우를 결정 합니다.
- 완벽 하 게 하 고 자동으로 관리 이미지 컨텍스트 (`CGContext`) 수명 개발자 호출에 걱정 하지 않아도 되므로 시작 및 상황에 맞는 명령을 종료 합니다.
- 호환 되는 `UIGraphics.GetCurrentContext()` 메서드.

합니다 `CreateImage` 메서드는 `UIGraphicsImageRenderer` 클래스 와이드 컬러 이미지를 만드는 호출 되 고 완료 처리기에 그릴 이미지 컨텍스트를 사용 하 여 전달 합니다. 다음과 같이이 완료 처리기 내에서 수행 됩니다 모든 그리기.

- `UIColor.FromDisplayP3` 메서드 새 채도가 빨간색 멋지고 디스플레이 P3 색 공간에서 만들고 사각형의 처음 절반을 그리는 데 사용 됩니다. 
- 두 번째 사각형의 절반을 그리기 일반 sRGB 완벽 하 게 포화 비교에 대 한 빨간색으로 표시 합니다.

### <a name="drawing-wide-color-in-macos"></a>MacOS의 광범위 한 색을 그리기

`NSImage` 클래스에서 macOS Sierra 와이드 컬러 이미지의 그리기를 지원 하도록 확장 되었습니다. 예를 들어:

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

## <a name="rendering-on-screen-images-in-app"></a>앱에서 이미지 렌더링 화면

와이드 컬러 이미지를 화면의 렌더링 하려면 프로세스 위에 표시 되는 iOS 및 macOS에 대 한 오프 스크린 와이드 컬러 이미지를 그릴 비슷하게 작동 합니다.

### <a name="rendering-on-screen-in-ios"></a>IOS에서 화면의 렌더링

앱을 iOS에서 화면의 와이드 컬러에서 이미지를 렌더링 해야 하는 경우 재정의 `Draw` 메서드는 `UIView` 평소 대로 해당 합니다. 예를 들어:

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

IOS 10와 마찬가지로 합니다 `UIGraphicsImageRenderer` 지능적으로 sRGB 또는 확장 범위 sRGB 색 공간 앱이 실행 하는 경우에 iOS 장치 기능에 따라 렌더링 해야 하는 경우 결정 위에 표시 된 클래스는 `Draw` 메서드가 호출 됩니다. 또한는 `UIImageView` 색도 iOS 9.3 이후 관리 되었습니다.

앱에서 렌더링이 수행 되는 방법을 알아야 할 경우는 `UIView` 또는 `UIViewController`, 새 확인할 수 있습니다 `DisplayGamut` 의 속성을 `UITraitCollection` 클래스입니다. 이 값은 한 `UIDisplayGamut` 열거형 중:

- P3
- Srgb
- 지정되지 않음

앱에서 색 공간 이미지를 그리는 데 사용 되는 제어 하려는 경우 새 사용할 수 있습니다 `ContentsFormat` 의 속성을 `CALayer` 원하는 색 공간을 지정 합니다. 이 값은 한 `CAContentsFormat` 열거형 중:

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>MacOS에서 화면 렌더링

앱을 macOS에서 화면의 광범위 한 색에서 이미지를 렌더링 해야 하는 경우 재정의 `DrawRect` 메서드는 `NSView` 평소 대로 해당 합니다. 예를 들어:

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

마찬가지로 지능적으로 결정 sRGB 또는 확장 범위 sRGB 색 공간 앱이 실행 하는 경우에 Mac 하드웨어의 기능에 따라 렌더링 해야 하는 경우는 `DrawRect` 메서드가 호출 됩니다.

앱에서 색 공간 이미지를 그리는 데 사용 되는 제어 하려는 경우 새 사용할 수 있습니다 `DepthLimit` 의 속성을 `NSWindow` 원하는 색 공간을 지정 하는 클래스입니다. 이 값은 한 `NSWindowDepth` 열거형 중:

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb
