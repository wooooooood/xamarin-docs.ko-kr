---
title: Xamarin.iOS에서 모양 API
description: iOS를 사용 하면 응용 프로그램에서 해당 컨트롤의 모든 인스턴스에 변경 내용을 적용 되도록 visual 속성 설정 대신 개별 개체를 정적 클래스 수준에서 적용할 수 있습니다.
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/15/2018
ms.openlocfilehash: bfbc902b0912527fea6aaa58c6706ef5a0ccbf8e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61178340"
---
# <a name="appearance-api-in-xamarinios"></a>Xamarin.iOS에서 모양 API

_iOS를 사용 하면 응용 프로그램에서 해당 컨트롤의 모든 인스턴스에 변경 내용을 적용 되도록 visual 속성 설정 대신 개별 개체를 정적 클래스 수준에서 적용할 수 있습니다._

이 기능은 정적 통해 Xamarin.iOS에서 노출 됩니다 `Appearance` 지 원하는 모든 UIKit 컨트롤의 속성입니다. 시각적 모양은 (색조 색 및 배경 이미지와 같은 속성) 이므로 쉽게 사용자 지정할 수 있습니다 응용 프로그램에 일관 된 모양을 제공 합니다. 모양 API iOS 5에서에서 도입 되었으며 동안 iOS 9에서에서 되지의 일부에는 여전히 일부 스타일 지정 및 Xamarin.iOS 앱에서 테마 효과 달성 하는 좋은 방법입니다.

## <a name="overview"></a>개요

iOS 응용 프로그램에 적용 하려는 브랜딩 준수 표준 컨트롤 많은 UIKit 컨트롤의 모양을 사용자 지정할 수 있습니다.

사용자 지정 모양을 적용할 두 가지가 있습니다.

- **컨트롤 인스턴스에서 직접** – 도구 모음, 탐색 모음에서 단추 및 슬라이더를 비롯 한 많은 컨트롤에 색조 색, 배경 이미지 및 제목 위치 (으로 몇 가지 다른 특성)를 설정할 수 있습니다.

- **모양 정적 속성에 기본값을 설정할** – 각 컨트롤에 대 한 사용자 지정 가능한 특성을 통해 노출 되는 `Appearance` 정적 속성입니다. 이러한 속성에 적용 하는 모든 사용자 지정 속성이 설정 된 다음 생성 되는 해당 형식의 모든 컨트롤에 대 한 기본값으로 사용 됩니다.

이 스크린 샷에 표시 된 것과 같이 모양을 샘플 응용 프로그램은 세 가지 방법을 모두를 보여 줍니다.

[![](introduction-to-the-appearance-api-images/appearance01-sml.png "모양을 샘플 응용 프로그램에는 세 가지 방법을 모두 보여 줍니다.")](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

IOS 8 기준으로 모양을 프록시 TraitCollections 확장 되었습니다.
 `AppearanceForTraitCollection` 특정 특성 (trait) 컬렉션의 기본 모양을 설정에 사용할 수 있습니다. 자세한 내용은이에 대 한는 [스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 가이드.

## <a name="setting-appearance-properties"></a>모양 속성을 설정합니다.

첫 번째 화면에서 정적 모양 클래스에 있는 단추와 같이 노랑/주황색 요소 스타일을 지정 하는 합니다.

```csharp
// Set the default appearance values
UIButton.Appearance.TintColor = UIColor.LightGray;
UIButton.Appearance.SetTitleColor(UIColor.FromRGB(0,127,14), UIControlState.Normal);

UISlider.Appearance.ThumbTintColor = UIColor.Red;
UISlider.Appearance.MinimumTrackTintColor = UIColor.Orange;
UISlider.Appearance.MaximumTrackTintColor = UIColor.Yellow;

UIProgressView.Appearance.ProgressTintColor = UIColor.Yellow;
UIProgressView.Appearance.TrackTintColor = UIColor.Orange;
```

녹색 요소 스타일 같이 설정 합니다 `ViewDidLoad` 기본값을 재정의 하는 메서드 및 *모양을* 정적 클래스:

```csharp
slider2.ThumbTintColor = UIColor.FromRGB (0,127,70); // dark green
slider2.MinimumTrackTintColor = UIColor.FromRGB (66,255,63);
slider2.MaximumTrackTintColor = UIColor.FromRGB (197,255,132);
```

```csharp
progress2.ProgressTintColor = UIColor.FromRGB (66,255,63);
progress2.TrackTintColor = UIColor.FromRGB (197,255,132);
```

## <a name="using-uiappearance-in-xamarinforms"></a>Xamarin.Forms에서 UIAppearance 사용

모양 API 때 유용할 수 있습니다 [iOS 앱 스타일 지정](~/xamarin-forms/platform/ios/formatting.md#uiappearance) Xamarin.Forms 솔루션에. 몇 줄을 `AppDelegate` 클래스를 만들 필요 없이 특정 색 구성표를 구현 하는 데 도움이 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)합니다.

### <a name="custom-themes-and-uiappearance"></a>사용자 지정 테마 및 UIAppearance

iOS 인터페이스 컨트롤을 사용 하 여 "테마" 사용자의 많은 시각적 특성을 허용 합니다 *UIAppearance* 동일한 모양을 갖도록 하는 특정 컨트롤의 모든 인스턴스를 강제로 Api. 이 컨트롤의 개별 인스턴스가 아니라 여러 사용자 인터페이스 컨트롤 클래스에는 모양 속성으로 노출 됩니다. 정적에 표시 속성을 설정 `Appearance` 속성 응용 프로그램에서 해당 형식의 모든 컨트롤에 영향을 줍니다.

개념을 더 잘 이해 하는 예제를 살펴보겠습니다.

특정 변경 하려면 `UISegmentedControl` 자홍 tint 할 특정 컨트롤에 다음과 같은 화면에서를 참조 하려고 `ViewDidLoad`:

```csharp
sg1.TintColor = UIColor.Magenta;
```

또는 디자이너의 Properties pad에서 값을 설정 합니다.

[![](introduction-to-the-appearance-api-images/propertiespadtint.png "속성 패드 색조")](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

아래 이미지는 농도 'sg1' 라는 컨트롤이 집합이 보여 줍니다.

[![](introduction-to-the-appearance-api-images/image53.png "개별 컨트롤 색조를 설정합니다.")](introduction-to-the-appearance-api-images/image53.png#lightbox)

이러한 방식으로 많은 컨트롤을 설정 하는 완전히 비효율적일 수 정적 대신 설정할 수 있습니다 `Appearance` 클래스 자체의 속성입니다. 이 작업은 아래 코드에 표시 됩니다.

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

이제이 아래 이미지에서는 두 분할 된 컨트롤을 자홍으로 모양을 사용 하 여 보여 줍니다.

[![](introduction-to-the-appearance-api-images/image54.png "모양 컨트롤 색조를 설정합니다.")](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` 속성 설정 해야 응용 프로그램 수명 주기 초기에 같은 AppDelegate의 `FinishedLaunching` 이벤트 또는 영향을 받는 컨트롤을 표시 하기 전에 ViewController입니다.

참조를 [모양을 API 소개](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) 좀 더 자세한 내용은 합니다.

## <a name="related-links"></a>관련 링크

- [모양 (샘플)](https://developer.xamarin.com/samples/monotouch/Appearance/)
- [UIAppearance 프로토콜 참조](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Xamarin.Forms의 모양](~/xamarin-forms/platform/ios/formatting.md#uiappearance)
