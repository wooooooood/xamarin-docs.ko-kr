---
title: 요약 3 장입니다. 텍스트 더 자세히
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 3 장 요약 합니다. 텍스트 더 자세히'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 609b0066e033b48be55056d459e818a9acc9625c
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563331"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>요약 3 장입니다. 텍스트 더 자세히

이 장에서 설명 합니다 [ `Label` ](xref:Xamarin.Forms.Label) 글꼴, 색 등 서식을 더 자세히 보기입니다.

## <a name="wrapping-paragraphs"></a>단락 줄 바꿈

경우는 [ `Text` ](xref:Xamarin.Forms.Label.Text) 속성을 `Label` 긴 텍스트를 포함 `Label` 자동으로 나타난 것 처럼 여러 줄으로 래핑하는 [ **Baskervilles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) 샘플입니다. Em dash에 대 한 '\u2014'와 같은 유니코드 코드를 포함할 수 있습니다 또는 C# 새 줄을 중단 하려면 '\r'와 같은 문자입니다.

경우는 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) 및 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) 의 속성을 `Label` 로 설정 됩니다 `LayoutOptions.Fill`의 전체 크기는 `Label` 공간에 의해 제어 됩니다는 해당 컨테이너 사용할 수 있도록 설정 합니다. 합니다 `Label` 이라고 *제한*합니다. 크기는 `Label` 해당 컨테이너의 크기입니다.

경우는 `HorizontalOptions` 및 `VerticalOptions` 속성 이외의 값으로 설정 됩니다 `LayoutOptions.Fill`의 크기를 `Label` 최대 크기가 해당 컨테이너를 사용할 수 있도록 하는 텍스트를 렌더링 하는 데 필요한 공간에 의해 관리 됩니다는 `Label`. 합니다 `Label` 이라고 *비제한* 와 자체 크기를 결정 합니다.

(참고: 용어 *제한* 하 고 *비제한* 때문일, 직관적이 지는 무제한 뷰는 일반적으로 제약 된 뷰를 보다 작습니다. 또한 이러한 용어는 사용 되지 않음 일관 되 게 책의 초기 챕터에서.)

같은 보기를 `Label` 하나 이상의 차원을 제한 되며 다른 비제한 수 있습니다. `Label` 가로 방향으로 제한 되는 텍스트 여러 줄에 배치만 됩니다.

경우는 `Label` 는 제한 된 텍스트에 필요한 것 보다 더 많은 공간 차지할 수 것입니다. 텍스트의 전체 영역 내 배치 될 수 있습니다는 `Label`합니다. 설정 된 [ `HorizontalTextAlignment` ](xref:Xamarin.Forms.Label.HorizontalTextAlignment) 속성의 멤버에는 [ `TextAlignment` ](xref:Xamarin.Forms.TextAlignment) 열거형 ([`Start`](xref:Xamarin.Forms.TextAlignment.Start)를 [ `Center` ](xref:Xamarin.Forms.TextAlignment.Center), 또는 [ `End` ](xref:Xamarin.Forms.TextAlignment.Center)) 단락 모든 줄의 맞춤을 제어할 수 있습니다. 기본값은 `Start` 텍스트 왼쪽 맞춤입니다.

설정 합니다 [ `VerticalTextAlignment` ](xref:Xamarin.Forms.Label.VerticalTextAlignment) 속성의 멤버에는 `TextAlignment` 위쪽, 가운데 또는 차지한 영역이 아래쪽에 있는 텍스트를 배치 하는 열거형을 `Label`.

설정 된 [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) 속성의 멤버에는 [ `LineBreakMode` ](xref:Xamarin.Forms.LineBreakMode) 열거형 ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap)를 [ `CharacterWrap` ](xref:Xamarin.Forms.LineBreakMode.CharacterWrap), [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode.NoWrap)합니다 [ `HeadTruncation` ](xref:Xamarin.Forms.LineBreakMode.HeadTruncation)를 [ `MiddleTruncation` ](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation), 또는 [ `TailTruncation` ](xref:Xamarin.Forms.LineBreakMode.TailTruncation))를 컨트롤은 여러 단락 나누기에서 줄 또는 잘립니다 하는 방법입니다.

## <a name="text-and-background-colors"></a>텍스트 및 배경색

설정 합니다 [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) 및 [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) 의 속성 `Label` 하 [ `Color` ](xref:Xamarin.Forms.Color) 텍스트와 배경 색을 제어 하는 값입니다.

합니다 `BackgroundColor` 차지 하는 전체 영역의 배경을에 적용 됩니다는 `Label`합니다. 에 따라 합니다 `HorizontalOptions` 고 `VerticalOptions` 속성, 크기를 텍스트를 표시 하는 데 필요한 영역 보다 훨씬 클 수 있습니다. 색을 사용 하 여 다양 한 값을 사용 하 여 실험 `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`, 및 `VerticalTextAlignment` 어떻게 바뀌는지 확인할 크기와 위치에는 `Label`, 내에서 텍스트의 위치와 크기를 `Label`.

## <a name="the-color-structure"></a>색 구조체

합니다 [ `Color` ](xref:Xamarin.Forms.Color) 구조를 사용 하면 색 이름 또는 빨간색-녹색-파란색 (RGB) 값 또는 색상-채도-명도 (HSL) 값으로 색을 지정 합니다. 알파 채널을 투명도 나타내기 위해 사용할 수도 있습니다.

사용 하 여를 `Color` 생성자를 지정 합니다.

- [회색 음영](xref:Xamarin.Forms.Color.%23ctor(System.Double))
- [RGB 값](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))
- [투명도 사용 하 여 RGB 값](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))

인수는 `double` 0에서 1 사이의 값입니다.

만드는 몇 가지 정적 메서드를 사용할 수도 있습니다 `Color` 값:

- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) 에 대 한 `double` 0에서 1 사이의 RGB 값
- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32)) 0에서 255 정수 RGB 값
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double)) 에 대 한 `double` 투명도 사용 하 여 RGB 값
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) 투명도 사용 하 여 정수 RGB 값
- [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) 에 대 한 `double` 투명도 사용 하 여 HSL 값
- [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32)) 에 대 한는 `uint` 값으로 계산 (B + 256 * (G + 256 * (256 + R *는)))
- [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) 에 대 한는 `string` 형태로 16 진수 형식의 "#AARRGGBB" 또는 "#RRGGBB" 또는 "#ARGB" 또는 "#RGB"에 있는 각 문자에 해당 하는 알파, 빨강에 대 한 16 진수 녹색 및 파란색 채널입니다. 이 메서드는에 설명 된 대로 XAML 색 변환에 사용 되는 기본 [7 장, 코드 및 XAML](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md)합니다.

일단 만들어지면는 `Color` 값은 변경할 수 없습니다. 색의 특징은 다음 속성에서 가져올 수 있습니다.

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

이 모든 항목은 `double` 0에서 1 사이의 값입니다.

`Color` 또한 240 공용 정적 읽기 전용 필드에 대 한 일반적인 색을 정의합니다. 책 기록 된 시간에만 17 일반적인 색 사용할 수 있었습니다.

다른 공용 정적 읽기 전용 필드를 0으로 설정 하는 모든 색 채널을 사용 하 여 색을 정의 합니다.

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

여러 인스턴스 메서드는 새 색을 만들려면 기존 색 수정 허용:

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

마지막으로 두 개의 정적 읽기 전용 속성 특수 색 값을 정의합니다.

- [`Color.Default`](xref:Xamarin.Forms.Color.Default)을로 설정 된 모든 채널 &ndash;1
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default` 플랫폼의 색 구성표를 적용 하기 위한 것 및 결과적으로 서로 다른 플랫폼에서 다양 한 상황에서 다른 의미를 갖습니다. 기본적으로 플랫폼 색 구성표는 다음과 같습니다.

- iOS: 밝은 배경에 어두운 텍스트
- Android: Light (책)에 있는 어두운 배경에 텍스트 또는 어두운는 밝은 배경의 텍스트 (재료 디자인에서 AppCompat 통해는 **마스터** 샘플 코드 리포지토리의 분기)
- 밝은 배경에 UWP: 어두운 텍스트

`Color.Accent` 결과 어둡게 또는 밝게 배경에 표시 되는 플랫폼 특정 (및 경우에 따라 사용자가 선택할 수 있는) 색 값입니다.

## <a name="changing-the-application-color-scheme"></a>응용 프로그램 색 구성표 변경

위 목록에 표시 된 대로 기본 색 구성표를가 하는 다양 한 플랫폼.

광원에서 어둡게 체계를 Android.Manifest.xml 파일에서 또는에서 밝은 테마를 지정 하 여 전환할 수 있기 Android를 대상으로 하는 경우 [추가 AppCompat 및 재질 디자인](~/xamarin-forms/platform/android/appcompat.md)합니다.

Windows 플랫폼에서 색 테마 일반적으로 사용자가 선택 되어 있지만 추가할 수 있습니다는 `RequestedTheme` 특성 중 하나로 설정할 `Light` 또는 `Dark` 플랫폼의 App.xaml 파일에 있습니다. 기본적으로 UWP 프로젝트에서 App.xaml 파일에 포함 된 `RequestedTheme` 특성이로 설정 `Light`합니다.

## <a name="font-sizes-and-attributes"></a>글꼴 크기 및 특성

설정 된 [ `FontFamily` ](xref:Xamarin.Forms.Label.FontFamily) 속성의 `Label` "Times Roman" 글꼴 패밀리 선택과 같은 문자열을 합니다. 그러나 특정 플랫폼에서 지원 되는 글꼴 패밀리를 지정 해야 하 고 이런 점에서 플랫폼 일치 하지 않습니다.

설정 합니다 [ `FontSize` ](xref:Xamarin.Forms.Label.FontSize) 의 속성 `Label` 에 `double` 글꼴의 대략적인 높이 지정 하는 데 합니다. 참조 [5 장의 크기를 사용 하 여 처리](chapter05.md), 자세한 지능적으로 글꼴 크기를 선택 합니다.

또는 미리 설정 된 플랫폼에 종속 된 글꼴을 여러 가지 크기 중 하나를 가져올 수 있습니다. 정적 [ `Device.GetNamedSize` ](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) 메서드 및 [오버 로드](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element)) 둘 다 반환을 `double` 의 멤버에 따라 플랫폼에 적합 한 글꼴 크기 값을 [ `NamedSize` ](xref:Xamarin.Forms.NamedSize)열거형 ([`Default`](xref:Xamarin.Forms.NamedSize.Default)하십시오 [ `Micro` ](xref:Xamarin.Forms.NamedSize.Micro)를 [ `Small` ](xref:Xamarin.Forms.NamedSize.Small), [ `Medium` ](xref:Xamarin.Forms.NamedSize.Medium),  및 [ `Large` ](xref:Xamarin.Forms.NamedSize.Large)). 반환 되는 값을 `Medium` 멤버가 아닐 동일 `Default`합니다. 합니다 [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) 샘플 이러한 명명 된 크기를 사용 하 여 텍스트를 표시 합니다.

설정 된 [ `FontAttributes` ](xref:Xamarin.Forms.Label.FontAttributes) 속성을 `Label` 이러한 멤버에 [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes) 열거형 [ `Bold` ](xref:Xamarin.Forms.FontAttributes.Bold), [ `Italic` ](xref:Xamarin.Forms.FontAttributes.Italic), 또는 [ `None` ](xref:Xamarin.Forms.FontAttributes.None)합니다. 결합할 수 있습니다는 `Bold` 하 고 `Italic` 멤버는 C# 비트 OR 연산자입니다.

## <a name="formatted-text"></a>서식 있는 텍스트

모든 지금 까지의 예제에서 전체 텍스트에서 표시를 `Label` 균일 하 게 형식이 지정 되어 있습니다. 에 텍스트 문자열 내에서 서식을 변경 하려면 설정 하지 않은 합니다 `Text` 속성의 `Label`합니다. 대신 설정 합니다 [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) 형식의 개체에 속성 [ `FormattedString` ](xref:Xamarin.Forms.FormattedString)합니다.

`FormattedString` 에 [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) 속성의 컬렉션을 나타내는 [ `Span` ](xref:Xamarin.Forms.Span) 개체입니다. 각 `Span` 개체에는 자체 [ `Text` ](xref:Xamarin.Forms.Span.Text), [ `FontFamily` ](xref:Xamarin.Forms.Span.FontFamily)를 [ `FontSize` ](xref:Xamarin.Forms.Span.FontSize)하십시오 [ `FontAttributes` ](xref:Xamarin.Forms.Span.FontAttributes)하십시오 [ `ForegroundColor` ](xref:Xamarin.Forms.Span.ForegroundColor), 및 [ `BackgroundColor` ](xref:Xamarin.Forms.Span.BackgroundColor) 속성입니다.

합니다 [ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) 샘플에서는 합니다 `FormattedText` 텍스트 한 줄에 대 한 속성 및 [ **VariableFormattedParagraph** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) 다음과 같이 전체 단락에 대 한 기술을 보여 줍니다.

[![변수의 세 번 스크린 샷 단락 서식이 지정 된](images/ch03fg06-small.png "변수 형식이 지정 된 레이블 텍스트")](images/ch03fg06-large.png#lightbox "변수 형식이 지정 된 레이블 텍스트")

합니다 [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) 프로그램에서는 단일 `Label` 및 `FormattedString` 모든 각 플랫폼에 대해 명명 된 글꼴 크기를 표시 하는 개체입니다.



## <a name="related-links"></a>관련 링크

- [3 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [3 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [3 장 F# 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [레이블](~/xamarin-forms/user-interface/text/label.md)
- [색 작업](~/xamarin-forms/user-interface/colors.md)
