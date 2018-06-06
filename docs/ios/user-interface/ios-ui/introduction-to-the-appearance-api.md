---
title: Xamarin.iOS 모양 API
description: iOS 응용 프로그램에서 해당 컨트롤의 모든 인스턴스에 적용 됩니다 되도록 시각적 속성 설정이 아니라 개별 개체에 대 한 정적 클래스 수준에서 적용할 수 있습니다.
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 02b33550451506ef4756f0f7d4400b4f98cef368
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790255"
---
# <a name="appearance-api-in-xamarinios"></a>Xamarin.iOS 모양 API

_iOS 응용 프로그램에서 해당 컨트롤의 모든 인스턴스에 적용 됩니다 되도록 시각적 속성 설정이 아니라 개별 개체에 대 한 정적 클래스 수준에서 적용할 수 있습니다._

이 기능을 통해 정적 Xamarin.iOS에 노출 됩니다 `Appearance` 속성을 지 원하는 모든 UIKit 컨트롤입니다. 시각적 모양 (tint 색과 배경 이미지와 같은 속성) 따라서 쉽게 사용자 지정할 수 있습니다 응용 프로그램에 일관 된 모양을 제공 합니다. 모양 API iOS 5에에서 도입 된 아니며 iOS 9에에서 일부 부분은 사용이 중단 된 동안 여전히 일부 스타일 및 테마 설정 효과 Xamarin.iOS 앱에서 수행 하는 데 유용 합니다.

## <a name="overview"></a>개요

iOS 응용 프로그램에 적용 하려는 브랜딩 맞게 표준 컨트롤 많은 UIKit 컨트롤의 모양을 사용자 지정할 수 있습니다.

사용자 지정 모양을 적용 하려면 두 가지 방법으로

- **컨트롤 인스턴스에 직접** – 도구 모음, 탐색 모음, 단추 및 슬라이더를 포함 하 여 많은 컨트롤에 색조 색상, 배경 이미지 및 제목 위치 (으로 다른 특성)를 설정할 수 있습니다.

- **모양 정적 속성에 기본값을 설정** – 각 컨트롤에 대 한 사용자 지정 가능한 특성을 통해 노출 되는 `Appearance` 정적 속성입니다. 이러한 속성에 적용 한 모든 사용자 지정 속성이 설정 된 후에 만들어지는 해당 형식의 모든 컨트롤에 대 한 기본값으로 사용 됩니다.

모양 샘플 응용 프로그램에서는이 스크린 샷에 표시 된 것 처럼 세 가지 방법을 모두를 보여 줍니다.

 [![](introduction-to-the-appearance-api-images/appearance01.png "모양 샘플 응용 프로그램에는 세 가지 방법을 모두 보여 줍니다.")](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

IOS 8 기준으로 모양 프록시 TraitCollections 하도록 확장 되었습니다.
 `AppearanceForTraitCollection` 데 사용할 수는 특정 특성 컬렉션에 기본 모양을 설정 합니다. 자세한 내용은이에 대 한는 [스토리 보드에 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 가이드 합니다.


## <a name="setting-appearance-properties"></a>모양 속성을 설정합니다.

첫 번째 화면에서 단추 및 다음과 같은 노랑/주황색 요소 스타일을 지정 하 정적 모양 클래스를 사용 합니다.

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

녹색 요소 스타일에, 다음과 같이 설정 됩니다는 `ViewDidLoad` 기본값을 재정의 하는 메서드 및 *모양* 정적 클래스:

```csharp
slider2.ThumbTintColor = UIColor.FromRGB (0,127,70); // dark green
slider2.MinimumTrackTintColor = UIColor.FromRGB (66,255,63);
slider2.MaximumTrackTintColor = UIColor.FromRGB (197,255,132);
```

```csharp
progress2.ProgressTintColor = UIColor.FromRGB (66,255,63);
progress2.TrackTintColor = UIColor.FromRGB (197,255,132);
```

## <a name="using-uiappearance-in-xamarinforms"></a>Xamarin.forms에서 UIAppearance를 사용 하 여

모양 API 때 유용할 수 있습니다 [iOS 응용 프로그램 스타일 지정](~/xamarin-forms/platform/ios/theme.md#uiappearance) Xamarin.Forms 솔루션에서 합니다. 에 몇 줄만 작성 된 `AppDelegate` 클래스를 만들 필요 없이 특정 색 구성표를 구현 하는 데 도움이 될 수는 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)합니다.


### <a name="custom-themes-and-uiappearance"></a>사용자 지정 테마 및 UIAppearance

iOS 허용 하는 사용자의 시각적 특성을 여러 인터페이스 컨트롤을 사용 하 여 "테마"는 *UIAppearance* 동일한 모양을 갖도록 하는 특정 컨트롤의 모든 인스턴스를 강제로 Api입니다. 이 컨트롤의 개별 인스턴스가 아니라 많은 사용자 인터페이스 컨트롤 클래스를 모양 속성으로 노출 됩니다. 정적에 디스플레이 속성을 설정 `Appearance` 속성에는 응용 프로그램에서 해당 형식의 모든 컨트롤에 적용 합니다.

개념을 더 잘 이해 하는 예제를 참조 하세요.

특정 변경 하려면 `UISegmentedControl` 자홍 tint가 특정 컨트롤에서 다음과 같이 화면에서 참조 하려고 `ViewDidLoad`:

```csharp
sg1.TintColor = UIColor.Magenta;
```

또는 디자이너의 속성 패드에 값을 설정 합니다. 

[![](introduction-to-the-appearance-api-images/propertiespadtint.png "패드 Tint 속성")](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

아래 이미지에서는이 농도 's g 1' 이라는 컨트롤에만 설정 하는 보여 줍니다.

 [![](introduction-to-the-appearance-api-images/image53.png "개별 컨트롤 tint 설정")](introduction-to-the-appearance-api-images/image53.png#lightbox)

이러한 방식으로 여러 컨트롤을 설정 하는 완전히 비효율적일 수 대신 정적 설정할 수 있습니다 `Appearance` 클래스 자체에 속성입니다. 이 아래 코드에 나와 있습니다.

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

이제 아래 이미지는 자홍으로 설정 하는 모양을 사용 하 여 세그먼트화 된 컨트롤이 모두 보여 줍니다.

 [![](introduction-to-the-appearance-api-images/image54.png "모양 컨트롤 tint 설정")](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` 속성 설정 해야 응용 프로그램 수명의 초기에 같은 AppDelegate의 `FinishedLaunching` 이벤트 또는 영향을 받는 컨트롤을 표시 하기 전에 ViewController 합니다.


참조는 [모양 API 소개](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) 대 한 자세한 내용은 합니다.


## <a name="related-links"></a>관련 링크

- [모양 (샘플)](https://developer.xamarin.com/samples/monotouch/IntroToAppearance/)
- [UIAppearance 프로토콜 참조](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Xamarin.Forms에는 모양](~/xamarin-forms/platform/ios/theme.md#uiappearance)
