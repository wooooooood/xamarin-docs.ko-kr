---
title: Xamarin.Forms의 색
description: Xamarin.Forms는 유연한 플랫폼 간 색 클래스를 제공 합니다. 이 문서는 Color class를 사용 하는 방법을 제공 하는 기능을 설명 합니다.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: f68e192db0b7acceb325ad44f40dce9cb229a26a
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528979"
---
# <a name="colors-in-xamarinforms"></a>Xamarin.Forms의 색

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin.Forms는 유연한 플랫폼 간 색 클래스를 제공 합니다._

이 문서에서는 다양 한 방법을 소개 합니다 `Color` Xamarin.Forms에서 클래스를 사용할 수 있습니다.

`Color` 다양 한 색 인스턴스를 생성 하는 메서드를 제공 하는 클래스

- **명명 된 색** -일반적인 명명 된 색을 포함 하 여 컬렉션인 `Red`를 `Green`, 및 `Blue`합니다.
- **Fromhex** -문자열 값 (예: "00FF00")을 HTML에서 사용 하는 구문과 유사 합니다. 선택적으로 Alpha는 첫 번째 문자 쌍 ("CC00FF00")으로 지정할 수 있습니다.
- **FromHsla** -색상, 채도 및 명도 `double` 선택적 알파 값 (0.0-1.0) 값입니다.
- **FromRgb** -빨강, 녹색 및 파랑 `int` 값 (0-255).
- **FromRgba** -빨간색, 녹색, 파랑 및 알파 `int` 값 (0-255).
- **FromUint** -단일 설정 `double` 값을 나타내는 **argb**합니다.

여기에 할당 된 일부 예제에서는 색을가 `BackgroundColor` 다양 한 허용 되는 구문 사용 하 여 일부 레이블의:

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

이러한 색은 아래 각 플랫폼에 표시 됩니다. 최종 색-확인 `Accent` -blue-ish 색 iOS 및 Android 용 Xamarin.Forms에서이 값은 정의 합니다.

 [![색 데모](colors-images/colors-sml.png "색 데모")](colors-images/colors.png#lightbox "색 데모")

## <a name="colordefault"></a>Color.Default

사용 된 `Default` 기본값으로 다시 플랫폼 (각 속성에 대 한 각 플랫폼에서 다른 기본 색을 나타내는 이해) 색 값을 설정 (또는 다시 설정).

개발자 설정 하려면이 값을 사용할 수는 `Color` 해야 하지만 **하지** (해당 하는 모든-1로 설정) 구성 요소 RGB 값에 대해이 인스턴스를 쿼리 합니다.

## <a name="colortransparent"></a>Color.Transparent

제거할 색을 설정 합니다.

## <a name="coloraccent"></a>Color.Accent

IOS 및 Android에서이 인스턴스는 기본 배경으로 표시 되었지만 기본 텍스트 색 같지는 않습니다 대비 색으로 설정 됩니다.

## <a name="additional-methods"></a>추가 메서드

`Color` 새 색을 만드는 데 사용할 수 있는 추가 메서드를 포함 하는 인스턴스:

- **AddLuminosity** -제공 된 델타 여 명도 수정 하 여 색을 반환 합니다.
- **WithHue** -색상을 나타내는 제공 된 값 대체 색을 반환 합니다.
- **WithLuminosity** -제공 된 값을 사용 하 여 명도 대체 색을 반환 합니다.
- **WithSaturation** -제공 된 값을 사용 하 여 채도 대체 색을 반환 합니다.
- **MultiplyAlpha** -제공된 된 알파 값으로 곱하여 알파, 수정 하 여 색을 반환 합니다.

## <a name="implicit-conversions"></a>암시적 변환

간에 암시적 변환이 합니다 `Xamarin.Forms.Color` 및 `System.Drawing.Color` 형식을 수행할 수 있습니다.

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

이 코드 조각을 사용 하는 `Device.RuntimePlatform` 선택적으로 색을 설정 하는 속성을 `ActivityIndicator`:

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

> [!NOTE]
> XAML 컴파일 사용 하는 경우 색 이름은 대/소문자 구분 되며 소문자로 작성할 수 있습니다. XAML 컴파일에 대 한 자세한 내용은 참조 하세요. [XAML 컴파일](~/xamarin-forms/xaml/xamlc.md)합니다.

## <a name="summary"></a>요약

Xamarin.Forms `Color` 클래스 플랫폼 인식 색 참조를 만드는 데 사용 됩니다. 공유 코드 및 XAML에서 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [ColorsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [바인딩할 수 있는 선택 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
