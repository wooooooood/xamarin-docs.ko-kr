---
title: 3 장 요약 텍스트 자세히
description: 'Xamarin.ios를 사용 하 여 Mobile Apps 만들기: 3 장 요약 텍스트 자세히'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 69415b59bbd376330454302981e3216c236a16bb
ms.sourcegitcommit: 93697a20e6fc7da547a8714ac109d7953b61d63f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/28/2019
ms.locfileid: "72980932"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>3 장 요약 텍스트 자세히

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)

이 장에서는 색, 글꼴 및 서식을 포함 하 여 [`Label`](xref:Xamarin.Forms.Label) 보기를 자세히 살펴봅니다.

## <a name="wrapping-paragraphs"></a>단락 줄 바꿈

`Label`의 [`Text`](xref:Xamarin.Forms.Label.Text) 속성이 긴 텍스트를 포함 하는 경우 `Label` [**Baskervilles**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) 샘플에 표시 된 대로 자동으로 여러 줄로 줄 바꿈됩니다. Em에 ' \u2014 ' 같은 유니코드 코드를 포함 하거나 C# ' \r ' 같은 문자를 포함 하 여 새 줄로 나눌 수 있습니다.

`Label`의 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 `LayoutOptions.Fill`로 설정 하면 전체 `Label` 크기는 해당 컨테이너에서 사용할 수 있도록 설정 된 공간에 의해 제어 됩니다. `Label`은 *제한*된 것으로 간주 됩니다. `Label` 크기는 컨테이너의 크기입니다.

`HorizontalOptions` 및 `VerticalOptions` 속성이 `LayoutOptions.Fill`이외의 값으로 설정 된 경우 `Label`의 크기는 해당 컨테이너가 `Label`에 사용할 수 있도록 하는 크기까지 텍스트를 렌더링 하는 데 필요한 공간에 의해 결정 됩니다. `Label`는 *제한* 되지 않은 것으로 간주 되며 자체 크기를 결정 합니다.

제한 및 제한 되지 않은 뷰는 일반적으로 제한 된 보기 보다 더 작기 때문에 *제한* 된 용어와 제한 되지 않은 용어는 명확 하 게 *사용할 수 있습니다* . 또한 이러한 용어는 책의 초기 장에서 일관 되 게 사용 되지 않습니다.

`Label`와 같은 뷰는 한 차원에서 제한 될 수 있으며 다른 차원에서는 제한 되지 않습니다. `Label`는 가로로 제한 된 경우에만 텍스트를 여러 줄로 줄 바꿈 합니다.

`Label` 제한 되 면 텍스트에 필요한 것 보다 훨씬 많은 공간을 차지할 수 있습니다. 텍스트는 `Label`의 전체 영역에 배치할 수 있습니다. [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) 속성을 [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) 열거형의 멤버 ([`Start`](xref:Xamarin.Forms.TextAlignment.Start), [`Center`](xref:Xamarin.Forms.TextAlignment.Center)또는 [`End`](xref:Xamarin.Forms.TextAlignment.Center))로 설정 하 여 단락의 모든 줄 맞춤을 제어 합니다. 기본값은 `Start` 이며 텍스트를 왼쪽에 맞춥니다.

[`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) 속성을 `TextAlignment` 열거형의 멤버로 설정 하 여 `Label`가 차지 하는 영역의 위쪽, 가운데 또는 아래쪽에 텍스트를 배치 합니다.

[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 속성을 [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) 열거형의 멤버 ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap), [`CharacterWrap`](xref:Xamarin.Forms.LineBreakMode.CharacterWrap), [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap), [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode.HeadTruncation), [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation)또는 [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode.TailTruncation))로 설정 하 여 여러 단락 나누기 또는의 줄이 잘립니다.

## <a name="text-and-background-colors"></a>텍스트 및 배경색

`Label`의 [`TextColor`](xref:Xamarin.Forms.Label.TextColor) 및 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 속성을 [`Color`](xref:Xamarin.Forms.Color) 값으로 설정 하 여 텍스트 및 배경의 색을 제어 합니다.

`BackgroundColor`은 `Label`가 차지 하는 전체 영역의 배경에 적용 됩니다. `HorizontalOptions` 및 `VerticalOptions` 속성에 따라 해당 크기가 텍스트를 표시 하는 데 필요한 영역 보다 훨씬 클 수 있습니다. 색을 사용 하 여 `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`및 `VerticalTextAlignment`의 다양 한 값을 실험 하 여 `Label`의 크기와 위치 및 `Label`내의 텍스트 크기와 위치에 미치는 영향을 확인할 수 있습니다.

## <a name="the-color-structure"></a>색 구조

[`Color`](xref:Xamarin.Forms.Color) 구조를 사용 하면 색을 Rgb (빨강-녹색-파랑) 값 또는 색-채도-명도 (HSL) 값으로 지정 하거나 색 이름을 지정할 수 있습니다. 알파 채널은 투명성을 나타내는 데에도 사용할 수 있습니다.

`Color` 생성자를 사용 하 여 다음을 지정 합니다.

- [회색 음영](xref:Xamarin.Forms.Color.%23ctor(System.Double))
- [RGB 값](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))
- [투명도가 있는 RGB 값](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))

인수는 0에서 1 사이의 `double` 값입니다.

여러 정적 메서드를 사용 하 여 `Color` 값을 만들 수도 있습니다.

- 0에서 1 사이의 `double` RGB 값에 대 한 [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double))
- 0에서 255 사이의 정수 RGB 값에 대 한 [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32))
- 투명도가 있는 `double` RGB 값에 대 한 [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double))
- 투명도가 있는 정수 RGB 값에 대 한 [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32))
- 투명도가 있는 `double` HSL 값에 대 한 [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))
- 로 계산 된 `uint` 값 [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32)) (B + 256 \* (G + 256 \* (R + 256 \* a)))
- "#AARRGGBB" 또는 "#RRGGBB" 또는 "#ARGB" 형식으로 된 16 진수의 `string` 형식 [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) , 각 문자는 알파, 빨강, 녹색 및 파랑 채널의 16 진수에 해당 합니다. 이 메서드는 [7, xaml 및 코드 장에서 설명 하](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md)는 것 처럼 xaml 색 변환에 주로 사용 됩니다.

만든 후에는 `Color` 값을 변경할 수 없습니다. 다음 속성에서 색의 특성을 가져올 수 있습니다.

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

0에서 1 사이의 모든 `double` 값입니다.

또한 `Color`는 공용 색에 대해 240 공용 정적 읽기 전용 필드를 정의 합니다. 책이 작성 될 때 17 개의 공통 색만 사용할 수 있습니다.

다른 공용 정적 읽기 전용 필드는 모든 색 채널이 0으로 설정 된 색을 정의 합니다.

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

여러 인스턴스 메서드를 사용 하 여 기존 색을 수정 하 여 새 색을 만들 수 있습니다.

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

마지막으로, 두 정적 읽기 전용 속성은 특수 색 값을 정의 합니다.

- [`Color.Default`](xref:Xamarin.Forms.Color.Default)모든 채널은 &ndash;1로 설정 됩니다.
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default`는 플랫폼의 색 구성표를 적용 하기 위한 것 이므로 다른 플랫폼의 다른 컨텍스트에서는 다른 의미를 가집니다. 기본적으로 플랫폼 색 구성표는 다음과 같습니다.

- iOS: 밝은 배경의 짙은 텍스트
- Android: 어두운 배경의 밝은 텍스트 또는 밝은 배경의 어두운 텍스트 (샘플 코드 리포지토리의 **마스터** 분기에 있는 AppCompat을 통한 재질 디자인의 경우)
- UWP: 밝은 배경의 짙은 텍스트

`Color.Accent` 값은 어둡거나 밝은 배경에 표시 되는 플랫폼별 및 사용자가 선택할 수 있는 색을 생성 합니다.

## <a name="changing-the-application-color-scheme"></a>응용 프로그램 색 구성표 변경

위의 목록에 표시 된 것 처럼 다양 한 플랫폼에는 기본 색 구성표가 있습니다.

Android를 대상으로 하는 경우에는 Android .Manifest 파일에서 밝은 테마를 지정 하거나 [AppCompat과 재질 디자인을 추가](~/xamarin-forms/platform/android/appcompat-material-design.md)하 여 짙은 색으로 전환할 수 있습니다.

Windows 플랫폼의 경우 색 테마는 일반적으로 사용자가 선택 하지만 플랫폼의 app.xaml 파일에서 `Light` 또는 `Dark`에 `RequestedTheme` 특성 집합을 추가할 수 있습니다. 기본적으로 UWP 프로젝트의 app.xaml 파일에는 `Light`으로 설정 된 `RequestedTheme` 특성이 포함 되어 있습니다.

## <a name="font-sizes-and-attributes"></a>글꼴 크기 및 특성

`Label`의 [`FontFamily`](xref:Xamarin.Forms.Label.FontFamily) 속성을 "Times Roman" 등의 문자열로 설정 하 여 글꼴 패밀리를 선택 합니다. 그러나 특정 플랫폼에서 지원 되는 글꼴 패밀리를 지정 해야 하며 이러한 측면에서 플랫폼이 일치 하지 않습니다.

`Label`의 [`FontSize`](xref:Xamarin.Forms.Label.FontSize) 속성을 글꼴의 대략적인 높이를 지정 하는 `double`으로 설정 합니다. 글꼴 크기를 지능적으로 선택 하는 방법에 대 한 자세한 내용은 [5 장, 크기 처리](chapter05.md)를 참조 하세요.

또는 미리 설정 된 여러 플랫폼 종속 글꼴 크기 중 하나를 가져올 수 있습니다. 정적 [`Device.GetNamedSize`](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) 메서드와 [오버 로드](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element)) 는 모두 [`NamedSize`](xref:Xamarin.Forms.NamedSize) 열거형의 멤버 ([`Default`](xref:Xamarin.Forms.NamedSize.Default), [`Micro`](xref:Xamarin.Forms.NamedSize.Micro), [`Small`](xref:Xamarin.Forms.NamedSize.Small)을 [기반으로 하는 플랫폼에 적절 한 `double` 글꼴 크기 값을 반환 합니다.`Medium`](xref:Xamarin.Forms.NamedSize.Medium)및 [`Large`](xref:Xamarin.Forms.NamedSize.Large)). `Medium` 멤버에서 반환 된 값은 `Default`와 동일할 필요는 없습니다. [**Named글꼴 크기**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) 샘플은 이러한 명명 된 크기의 텍스트를 표시 합니다.

`Label`의 [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) 속성을 이러한 [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) 열거, [`Bold`](xref:Xamarin.Forms.FontAttributes.Bold), [`Italic`](xref:Xamarin.Forms.FontAttributes.Italic)또는 [`None`](xref:Xamarin.Forms.FontAttributes.None)의 멤버로 설정 합니다. C# 비트 or 연산자를 사용 하 여 `Bold` 및 `Italic` 멤버를 결합할 수 있습니다.

## <a name="formatted-text"></a>서식 있는 텍스트

지금 까지의 모든 예제에서 `Label` 표시 되는 전체 텍스트의 형식이 균일 하 게 지정 되었습니다. 텍스트 문자열 내에서 서식을 변경 하려면 `Label`의 `Text` 속성을 설정 하지 마세요. 대신 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) 속성을 [`FormattedString`](xref:Xamarin.Forms.FormattedString)형식의 개체로 설정 합니다.

`FormattedString`에는 [`Span`](xref:Xamarin.Forms.Span) 개체 컬렉션인 [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) 속성이 있습니다. 각 `Span` 개체에는 고유한 [`Text`](xref:Xamarin.Forms.Span.Text), [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily), [`FontSize`](xref:Xamarin.Forms.Span.FontSize), [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes), [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)및 [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) 속성이 있습니다.

[**VariableFormattedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) 샘플에서는 텍스트 한 줄에 `FormattedText` 속성을 사용 하는 방법을 보여 줍니다. [**VariableFormattedParagraph**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) 은 다음과 같이 전체 단락에 대 한 기술을 보여 줍니다.

[![가변 형식 단락의 세 가지 스크린샷](images/ch03fg06-small.png "변수 형식의 레이블 텍스트")](images/ch03fg06-large.png#lightbox "변수 형식의 레이블 텍스트")

[**Named글꼴 크기**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) 프로그램은 단일 `Label`와 `FormattedString` 개체를 사용 하 여 각 플랫폼에 대 한 명명 된 글꼴 크기를 모두 표시 합니다.

## <a name="related-links"></a>관련 링크

- [3 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [3 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [3 F# 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [레이블](~/xamarin-forms/user-interface/text/label.md)
- [색 작업](~/xamarin-forms/user-interface/colors.md)
