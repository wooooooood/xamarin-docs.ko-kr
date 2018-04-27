---
title: 색
description: Xamarin.Forms는 유연한 플랫폼 간 색 클래스를 제공합니다.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 71c10e1de8b94b8d9799d144fb603c82c40ca9eb
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="colors"></a>색

_Xamarin.Forms는 유연한 플랫폼 간 색 클래스를 제공합니다._

이 문서에서는 다양 한 방법으로 `Color` Xamarin.Forms에 클래스를 사용할 수 있습니다.

`Color` 클래스 다양 한 색 인스턴스 작성 하는 메서드를 제공 합니다.

-  **명명 된 색** -의 일반적인 명명 된 색을 포함 하 여 컬렉션 `Red`, `Green`, 및 `Blue`합니다.
-  **FromHex** -문자열 값을 HTML, 예: "00FF00"에서 사용 하는 구문과 유사 합니다. Alpha는 필요에 따라 첫 번째 쌍은 문자 ("CC00FF00")으로 지정할 수 있습니다.
-  **FromHsla** -색상, 채도 및 명도 `double` 값이 선택적 알파 값 (0.0-1.0).
-  **FromRgb** -빨간색, 녹색 및 파란색 `int` 값 (0-255).
-  **FromRgba** -빨간색, 녹색, 파랑 및 알파 `int` 값 (0-255).
-  **FromUint** -단일 설정 `double` 값을 나타내는 **argb**합니다.

여기에 할당 된 일부 예제에서는 색을가 `BackgroundColor` 허용 되는 구문의 여러 변형도 사용 하 여 일부 레이블:

```csharp
var red    = new Label { Text = "Red",   BackgroundColor = Color.Red };
var orange = new Label { Text = "Orange",BackgroundColor = Color.FromHex("FF6A00") };
var yellow = new Label { Text = "Yellow",BackgroundColor = Color.FromHsla(0.167, 1.0, 0.5, 1.0) };
var green  = new Label { Text = "Green", BackgroundColor = Color.FromRgb (38, 127, 0) };
var blue   = new Label { Text = "Blue",  BackgroundColor = Color.FromRgba(0, 38, 255, 255) };
var indigo = new Label { Text = "Indigo",BackgroundColor = Color.FromRgb (0, 72, 255) };
var violet = new Label { Text = "Violet",BackgroundColor = Color.FromHsla(0.82, 1, 0.25, 1) };

var transparent = new Label { Text = "Transparent",BackgroundColor = Color.Transparent };
var @default = new Label    { Text = "Default",    BackgroundColor = Color.Default };
var accent = new Label      { Text = "Accent",     BackgroundColor = Color.Accent };
```

이러한 색은 아래 각 플랫폼에 표시 됩니다. 최종 색-확인 `Accent` -은 iOS 및 Android; blue-ish 색 Xamarin.Forms로이 값이 정의 합니다.

 [![색 데모](colors-images/colors-sml.png "색 데모")](colors-images/colors.png#lightbox "색 데모")

## <a name="colordefault"></a>Color.Default

사용 된 `Default` 다시 기본값으로 플랫폼 (이 각 속성에 대 한 각 플랫폼에서 다른 기본 색 이해) 색 값을 설정 (또는 다시 설정).

개발자를 설정 하려면이 값을 사용할 수 있습니다는 `Color` 해야 하지만 **하지** 의 구성 요소 RGB 값 (있습니다 하는 모두-1로 설정)에 대 한이 인스턴스를 쿼리 합니다.

## <a name="colortransparent"></a>Color.Transparent

색 선택을 설정 합니다.

## <a name="coloraccent"></a>Color.Accent

IOS 및 Android에서이 인스턴스는 기본 마스터 페이지에 표시 되지만 기본 텍스트와 같지 않습니다 대비 색으로 설정 됩니다.

## <a name="additional-methods"></a>추가 방법

`Color` 새 색을 만드는 데 사용할 수 있는 추가 메서드를 포함 하는 인스턴스:

-  **AddLuminosity** -제공 된 델타 명도 수정 하 여 색을 반환 합니다.
-  **WithHue** -색상을 나타내는 제공 된 값으로 대체 하는 새 색을 반환 합니다.
-  **WithLuminosity** -명도 제공 된 값으로 대체 하는 새 색을 반환 합니다.
-  **WithSaturation** -채도 제공 된 값으로 대체 하는 새 색을 반환 합니다.
-  **MultiplyAlpha** -제공 된 알파 값으로 곱하여 알파, 수정 하 여 색을 반환 합니다.

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

이 코드 조각을 사용 하 여는 `Device.RuntimePlatform` 속성을 선택적으로의 색을 설정 하려면 프로그램 `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>XAML에서 사용 하 여

색 정의 된 색 이름 또는 여기에 표시 된 16 진수 표현을 사용 하 여 XAML에서 쉽게 참조할 수도 있습니다.

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

## <a name="summary"></a>요약

Xamarin.Forms는 `Color` 클래스 플랫폼 인식 색 참조를 생성 하는 데 사용 됩니다. 공유 코드 및 XAML에서 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [ColorsSample](https://developer.xamarin.com/samples/WorkingWithColors)
- [바인딩 가능한 선택 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
