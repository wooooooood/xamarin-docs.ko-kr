---
title: 색의Xamarin.Forms
description: Xamarin.Forms유연한 플랫폼 간 색 클래스를 제공 합니다. 이 문서에서는 Color 클래스에서 제공 하는 기능과 사용 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a02fe7451702367d85d322b756df4a547a009454
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137347"
---
# <a name="colors-in-xamarinforms"></a>색의Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin.ios는 유연한 플랫폼 간 색 클래스를 제공 합니다._

이 문서에서는에서 클래스를 사용 하는 다양 한 방법을 소개 [`Color`](xref:Xamarin.Forms.Color) Xamarin.Forms 합니다.

[`Color`](xref:Xamarin.Forms.Color)클래스는 인스턴스를 작성 하는 여러 메서드를 제공 합니다 `Color` .

- **명명 된 색** -, 및를 포함 한 일반적인 명명 된 색의 컬렉션입니다 `Red` `Green` `Blue` .
- `FromHex`-HTML에서 사용 된 구문과 유사한 문자열 값 (예: "00FF00")입니다. 선택적으로 Alpha는 첫 번째 문자 쌍 ("CC00FF00")으로 지정할 수 있습니다.
- `FromHsla`-색조, 채도 및 명도 `double` 값 (선택적 알파 값 (0.0-1.0)).
- `FromHsv`-색상, 채도 및 값 `int` 또는 값 `double` 입니다.
- `FromHsva`-색상, 채도 및 값 `int` 또는 값 `double` 입니다.
- `FromRgb`-빨강, 녹색 및 파랑 `int` 값 (0-255)입니다.
- `FromRgba`-빨강, 녹색, 파랑 및 알파 `int` 값 (0-255)입니다.
- `FromUint`- `double` **argb**를 나타내는 단일 값을 설정 합니다.

다음은 `BackgroundColor` 허용 되는 구문의 다른 변형을 사용 하 여 일부 레이블의에 할당 된 몇 가지 예제 색입니다.

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

이러한 색은 아래 각 플랫폼에 표시 됩니다. 최종 색은 `Accent` iOS 및 Android의 파란색 길쭉한 색입니다 .이 값은에 의해 정의 됩니다 Xamarin.Forms .

 [![색 데모](colors-images/colors-sml.png "색 데모")](colors-images/colors.png#lightbox "색 데모")

## <a name="colordefault"></a>Color. 기본값

을 사용 `Default` 하 여 색 값을 플랫폼 기본값으로 설정 (또는 다시 설정) 합니다 .이는 각 속성에 대 한 각 플랫폼의 서로 다른 기본 색을 나타냅니다.

개발자는이 값을 사용 하 여 `Color` 속성을 설정할 **not** 수 있지만, 해당 구성 요소 RGB 값에 대해이 인스턴스를 쿼리하지 말아야 합니다. 모두-1로 설정 됩니다.

## <a name="colortransparent"></a>Color. 투명

색을 지우도록 설정 합니다.

## <a name="coloraccent"></a>Color. 악센트

IOS 및 Android에서이 인스턴스는 기본 배경에 표시 되는 대비 색으로 설정 되지만 기본 텍스트 색과는 다릅니다.

## <a name="additional-methods"></a>추가 방법

[`Color`](xref:Xamarin.Forms.Color)인스턴스에는 다음과 같은 추가 메서드가 포함 됩니다.

- `AddLuminosity`-제공 된 `Color` 델타를 기준으로 광도를 수정 하 여를 반환 합니다.
- `MultiplyAlpha`-알파를 `Color` 수정 하 고 제공 된 알파 값을 곱하여를 반환 합니다.
- `ToHex`-의 16 진수 `string` 표현을 반환 `Color` 합니다.
- `WithHue`-를 반환 `Color` 하 고, 지정 된 값으로 색상을 바꿉니다.
- `WithLuminosity`-광도를 `Color` 제공 된 값으로 바꿔를 반환 합니다.
- `WithSaturation`-채도를 `Color` 제공 된 값으로 바꿔를 반환 합니다.

## <a name="implicit-conversions"></a>암시적 변환

및 형식 간의 암시적 `Xamarin.Forms.Color` 변환을 `System.Drawing.Color` 수행할 수 있습니다.

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>장치. RuntimePlatform

이 코드 조각에서는 속성을 사용 하 여 `Device.RuntimePlatform` 의 색을 선택적으로 설정 합니다 `ActivityIndicator` .

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="use-from-xaml"></a>XAML에서 사용

다음과 같이 정의 된 색 이름 또는 16 진수 표현을 사용 하 여 XAML에서 색을 참조할 수도 있습니다.

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> XAML 컴파일을 사용 하는 경우 색 이름은 대/소문자를 구분 하지 않으므로 소문자로 작성할 수 있습니다. XAML 컴파일에 대한 자세한 내용은 [XAML 컴파일](~/xamarin-forms/xaml/xamlc.md)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [ColorsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [바인딩 가능한 선택기 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
