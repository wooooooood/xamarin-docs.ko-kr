---
title: Xamarin.ios의 모양 API
description: iOS를 사용 하면 응용 프로그램에서 해당 컨트롤의 모든 인스턴스에 변경 내용을 적용할 수 있도록 개별 개체가 아닌 정적 클래스 수준에서 시각적 속성 설정을 적용할 수 있습니다.
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 11/15/2018
ms.openlocfilehash: 7a7f0fe9d0dc07d892686e6596f3cc09a2587513
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73003375"
---
# <a name="appearance-api-in-xamarinios"></a>Xamarin.ios의 모양 API

_iOS를 사용 하면 응용 프로그램에서 해당 컨트롤의 모든 인스턴스에 변경 내용을 적용할 수 있도록 개별 개체가 아닌 정적 클래스 수준에서 시각적 속성 설정을 적용할 수 있습니다._

이 기능은 해당 기능을 지 원하는 모든 UIKit 컨트롤의 정적 `Appearance` 속성을 통해 Xamarin.ios에서 노출 됩니다. 따라서 시각적 모양 (색조 색 및 배경 이미지와 같은 속성)을 쉽게 사용자 지정 하 여 응용 프로그램에 일관 된 모양을 제공할 수 있습니다. 모양 API는 iOS 5에서 도입 되었지만이 중 일부는 iOS 9에서 더 이상 사용 되지 않지만 Xamarin.ios 앱에서 일부 스타일 지정 및 테마 효과를 달성할 수 있는 좋은 방법입니다.

## <a name="overview"></a>개요

iOS를 사용 하면 다양 한 UIKit 컨트롤의 모양을 사용자 지정 하 여 표준 컨트롤이 응용 프로그램에 적용 하려는 브랜딩을 따르도록 할 수 있습니다.

사용자 지정 모양을 적용 하는 두 가지 다른 방법이 있습니다.

- **컨트롤 인스턴스에 직접** -도구 모음, 탐색 모음, 단추 및 슬라이더를 비롯 한 다양 한 컨트롤에 대 한 색조 색, 배경 이미지 및 제목 위치 및 기타 특성을 설정할 수 있습니다.

- **모양 정적 속성에 기본값 설정** – 각 컨트롤에 대 한 사용자 지정 가능한 특성은 `Appearance` 정적 속성을 통해 노출 됩니다. 이러한 속성에 적용 하는 모든 사용자 지정은 속성이 설정 된 후 생성 된 해당 형식의 모든 컨트롤에 대 한 기본값으로 사용 됩니다.

모양새 샘플 응용 프로그램은 다음 스크린샷에 표시 된 것 처럼 세 가지 메서드를 모두 보여 줍니다.

[![](introduction-to-the-appearance-api-images/appearance01-sml.png "The Appearance sample application demonstrates all three methods")](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

IOS 8에서 모양 프록시는 TraitCollections로 확장 되었습니다.
 `AppearanceForTraitCollection`를 사용 하 여 특정 특성 컬렉션의 기본 모양을 설정할 수 있습니다. [스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 가이드에서이에 대해 자세히 알아볼 수 있습니다.

## <a name="setting-appearance-properties"></a>모양 속성 설정

첫 번째 화면에서 다음과 같이 정적 모양 클래스를 사용 하 여 단추와 노랑/주황 요소에 스타일을 적용할 수 있습니다.

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

녹색 요소 스타일은 기본값을 재정의 하는 `ViewDidLoad` 메서드에서 다음과 같이 설정 됩니다.

```csharp
slider2.ThumbTintColor = UIColor.FromRGB (0,127,70); // dark green
slider2.MinimumTrackTintColor = UIColor.FromRGB (66,255,63);
slider2.MaximumTrackTintColor = UIColor.FromRGB (197,255,132);
```

```csharp
progress2.ProgressTintColor = UIColor.FromRGB (66,255,63);
progress2.TrackTintColor = UIColor.FromRGB (197,255,132);
```

## <a name="using-uiappearance-in-xamarinforms"></a>Xamarin.ios에서 UIAppearance 사용

모양 API는 Xamarin.ios 솔루션에서 [iOS 앱의 스타일을](~/xamarin-forms/platform/ios/formatting.md#uiappearance) 지정할 때 유용할 수 있습니다. `AppDelegate` 클래스의 몇 줄은 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)를 만들지 않고도 특정 색 구성표를 구현 하는 데 도움이 될 수 있습니다.

### <a name="custom-themes-and-uiappearance"></a>사용자 지정 테마 및 UIAppearance

iOS에서는 *uiappearance* api를 사용 하 여 사용자 인터페이스 컨트롤의 많은 시각적 특성을 "테마가 적용" 하 여 특정 컨트롤의 모든 인스턴스가 동일한 모양을 갖도록 할 수 있습니다. 이는 컨트롤의 개별 인스턴스가 아닌 많은 사용자 인터페이스 컨트롤 클래스에서 모양 속성으로 노출 됩니다. 정적 `Appearance` 속성에 표시 속성을 설정 하면 응용 프로그램에서 해당 형식의 모든 컨트롤에 영향을 줍니다.

개념을 보다 잘 이해 하려면 예제를 참조 하십시오.

자홍 색조로 특정 `UISegmentedControl`를 변경 하려면 `ViewDidLoad`에서 다음과 같이 화면에서 특정 컨트롤을 참조 합니다.

```csharp
sg1.TintColor = UIColor.Magenta;
```

또는 디자이너의 속성 패드에서 값을 설정 합니다.

[![](introduction-to-the-appearance-api-images/propertiespadtint.png "Properties Pad Tint")](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

아래 이미지는 ' sg1 ' 이라는 컨트롤에만 색조를 설정 하는 것을 보여 줍니다.

[![](introduction-to-the-appearance-api-images/image53.png "Setting the individual control tint")](introduction-to-the-appearance-api-images/image53.png#lightbox)

이러한 방식으로 많은 컨트롤을 설정 하려면 완전히 비효율적 이기 때문에 클래스 자체에서 정적 `Appearance` 속성을 대신 설정할 수 있습니다. 이는 아래 코드에 나와 있습니다.

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

이제 아래 이미지에서는 모양이 자홍으로 설정 된 분할 된 컨트롤을 모두 보여 줍니다.

[![](introduction-to-the-appearance-api-images/image54.png "Setting the Appearance control tint")](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` 속성은 AppDelegate의 `FinishedLaunching` 이벤트와 같은 응용 프로그램 수명 주기 초기에 설정 하거나 영향을 받는 컨트롤이 표시 되기 전에 ViewController에서 설정 해야 합니다.

자세한 내용은 [모양 API 소개](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) 를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [모양 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/appearance)
- [UIAppearance 프로토콜 참조](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Xamarin.ios의 모양](~/xamarin-forms/platform/ios/formatting.md#uiappearance)
