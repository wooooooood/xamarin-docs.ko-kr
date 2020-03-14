---
title: Xamarin.Forms의 색
description: Xamarin.Forms는 유연한 플랫폼 간 색 클래스를 제공 합니다. 이 문서는 Color class를 사용 하는 방법을 제공 하는 기능을 설명 합니다.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2019
ms.openlocfilehash: 2a17b037803d1ca6e54000ea7ba3f05c8ce6034f
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305758"
---
# <a name="colors-in-xamarinforms"></a>Xamarin.Forms의 색

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin.ios는 유연한 플랫폼 간 색 클래스를 제공 합니다._

이 문서에서는 Xamarin.ios에서 [`Color`](xref:Xamarin.Forms.Color) 클래스를 사용 하는 다양 한 방법을 소개 합니다.

[`Color`](xref:Xamarin.Forms.Color) 클래스는 색 인스턴스를 작성 하는 여러 메서드를 제공 합니다.

- **명명 된 색** -`Red`, `Green`및 `Blue`를 포함 하는 일반적인 명명 된 색의 컬렉션입니다.
- **Fromhex** -문자열 값 (예: "00FF00")을 HTML에서 사용 하는 구문과 유사 합니다. 선택적으로 Alpha는 첫 번째 문자 쌍 ("CC00FF00")으로 지정할 수 있습니다.
- **Fromhsla** -색상, 채도 및 명도 `double` 값 이며 선택적 알파 값 (0.0-1.0)이 있습니다.
- **Fromrgb** -Red, green 및 blue `int` 값 (0-255)입니다.
- **Fromrgba** -빨강, 녹색, 파랑 및 알파 `int` 값 (0-255)입니다.
- **FromUint** - **argb**를 나타내는 단일 `double` 값을 설정 합니다.

다음은 허용 되는 구문의 다른 변형을 사용 하 여 일부 레이블의 `BackgroundColor`에 할당 된 몇 가지 예제 색입니다.

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

이러한 색은 아래 각 플랫폼에 표시 됩니다. 최종 색 `Accent`는 iOS 및 Android의 파란색 길쭉한 색입니다. 이 값은 Xamarin.ios에 의해 정의 됩니다.

 [![색 데모](colors-images/colors-sml.png "색 데모")](colors-images/colors.png#lightbox "색 데모")

## <a name="colordefault"></a>Color.Default

`Default`를 사용 하 여 색 값을 플랫폼 기본값으로 다시 설정 (또는 다시 설정) 할 수 있습니다 .이는 각 속성에 대 한 각 플랫폼에서 서로 다른 기본 색을 나타내는 것을 이해 합니다.

개발자는이 값을 사용 하 여 `Color` 속성을 설정할 수 있지만 구성 요소 RGB 값에 대해이 인스턴스 **를 쿼리하지 말아야 합니다.** 모두-1로 설정 됩니다.

## <a name="colortransparent"></a>Color.Transparent

제거할 색을 설정 합니다.

## <a name="coloraccent"></a>Color.Accent

IOS 및 Android에서이 인스턴스는 기본 배경으로 표시 되었지만 기본 텍스트 색 같지는 않습니다 대비 색으로 설정 됩니다.

## <a name="additional-methods"></a>추가 방법

[`Color`](xref:Xamarin.Forms.Color) 인스턴스에는 다음과 같은 추가 메서드가 포함 됩니다.

- **Addluminosity** -제공 된 델타를 기준으로 광도를 수정 하 여 `Color`을 반환 합니다.
- **MultiplyAlpha** -알파를 수정 하 여 제공 된 알파 값을 곱하여 `Color`을 반환 합니다.
- **Tohex** -`Color`의 16 진수 `string` 표현을 반환 합니다.
- **Withhue** -색상을 제공 된 값으로 대체 하는 `Color`를 반환 합니다.
- **Withluminosity** -광도를 제공 된 값으로 대체 하는 `Color`를 반환 합니다.
- **Withsaturation** -`Color`을 반환 하 고 채도를 제공 된 값으로 바꿉니다.

## <a name="implicit-conversions"></a>암시적 변환

`Xamarin.Forms.Color`와 `System.Drawing.Color` 형식 간의 암시적 변환을 수행할 수 있습니다.

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

이 코드 조각은 `Device.RuntimePlatform` 속성을 사용 하 여 `ActivityIndicator`색을 선택적으로 설정 합니다.

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>XAML에서 사용 하 여

다음과 같이 정의 된 색 이름 또는 16 진수 표현을 사용 하 여 XAML에서 색을 참조할 수도 있습니다.

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> XAML 컴파일 사용 하는 경우 색 이름은 대/소문자 구분 되며 소문자로 작성할 수 있습니다. XAML 컴파일에 대한 자세한 내용은 [XAML 컴파일](~/xamarin-forms/xaml/xamlc.md)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [ColorsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [바인딩 가능한 선택기 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
