---
title: "요약 Chapter 3입니다. 텍스트를 자세하게"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a3ef515feabfc142f30e7e00a8fed710e733f4dc
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>요약 Chapter 3입니다. 텍스트를 자세하게

이 장에서 설명는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 서식 지정 및 색, 글꼴을 포함 하 여 더 자세히 보기.

## <a name="wrapping-paragraphs"></a>단락 줄 바꿈

경우는 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) 속성 `Label` 긴 텍스트를 포함 `Label` 자동으로에 나타난 것 처럼 여러 줄으로 래핑하는 [ **Baskervilles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) 샘플. Em 대시 또는 C#을 새 줄을 중단 하려면 '\r' 등의 문자에 대 한 '\u2014'와 같은 유니코드 코드를 포함할 수 있습니다.

경우는 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 및 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 의 속성은 `Label` 로 설정 `LayoutOptions.Fill`의 전체 크기는 `Label` 공간에 의해 제어 됩니다는 해당 컨테이너 사용할 수 있게 합니다. `Label` 라고 *제한*합니다. 크기는 `Label` 해당 컨테이너의 크기입니다.

때는 `HorizontalOptions` 및 `VerticalOptions` 속성이 아닌 다른 값으로 설정 된 `LayoutOptions.Fill`, 크기는 `Label` 해당 컨테이너에 대해 사용할 수 있도록 크기까지 텍스트를 렌더링 하는 데 필요한 공간에 의해 관리는 `Label`합니다. `Label` 라고 *무제한* 자체 크기를 결정 합니다.

(참고: 용어 *제한* 및 *무제한* 때문일 수, 비 직관적인 제한 되지 않은 보기는 일반적으로 제한 된 보기 보다 작습니다. 또한 이러한 용어 사용 되지 않습니다 지속적으로 책의 초기 장에서.)

와 같은 보기는 `Label` 에서 하나 이상의 차원을 제한 하 고 다른 제약 받지 않을 수 있습니다. A `Label` 가로 방향으로 제한 되는 텍스트 여러 줄에 배치만 됩니다.

경우는 `Label` 은 제한, 텍스트에 필요한 것 보다 더 많은 공간 차지할 수 것입니다. 텍스트의 전체 영역 내에 배치 될 수 있습니다는 `Label`합니다. 설정의 [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) 속성의 멤버에는 [ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) 열거형 ([`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/), 또는 [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)) 단락의 모든 줄 맞춤을 제어 하 합니다. 기본값은 `Start` 되 고 텍스트를 왼쪽 정렬 합니다.

설정는 [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) 속성의 멤버에는 `TextAlignment` 위쪽, 가운데 또는 차지한 영역이 아래쪽에 있는 텍스트를 배치 하는 열거형은 `Label`합니다.

설정의 [ `LineBreakMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.LineBreakMode/) 속성의 멤버에는 [ `LineBreakMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) 열거형 ([`WordWrap`](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.WordWrap/), [ `CharacterWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.CharacterWrap/), [ `NoWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.NoWrap/), [ `HeadTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.HeadTruncation/), [ `MiddleTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.MiddleTruncation/), 또는 [ `TailTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.TailTruncation/))를 배수 단락 나누기에서 줄 또는 잘립니다 방법을 제어 합니다.

## <a name="text-and-background-colors"></a>텍스트 색과 배경색

설정의 [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) 및 [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) 의 속성 `Label` 를 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 텍스트 및 배경 색을 제어 하는 값입니다.

`BackgroundColor` 를 사용 하는 전체 영역 배경 적용 되는 `Label`합니다. 에 따라는 `HorizontalOptions` 및 `VerticalOptions` 속성, 크기 텍스트를 표시 하는 데 필요한 영역 보다 훨씬 큰 수 있습니다. 색을 사용 하 여의 다양 한 값으로 시험 `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`, 및 `VerticalTextAlignment` 어떻게 바뀌는지 크기 및 위치에는 `Label`에서 텍스트의 위치와 크기는 `Label`합니다.

## <a name="the-color-structure"></a>색 구조체

[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 구조 또는 색 이름 빨간색-녹색-파란색 (RGB) 값 또는 색상-채도-명도 (HSL) 값으로 색을 지정할 수 있습니다. 알파 채널 투명도 나타내는 데 사용할 수도 있습니다.

사용 하 여 한 `Color` 생성자를 지정 합니다.

- [회색 음영으로 표시](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/)
- [RGB 값](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/)
- [투명도 사용 하 여 RGB 값](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/)

인수는 `double` 0에서 1 사이의 값입니다.

만드는 몇 가지 정적 메서드를 사용할 수도 있습니다 `Color` 값:

- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/) 에 대 한 `double` 0에서 1 사이의 RGB 값
- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Int32/System.Int32/System.Int32/) 0에서 255 정수 RGB 값에 대 한
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Double/System.Double/System.Double/System.Double/) 에 대 한 `double` 투명도 사용 하 여 RGB 값
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) 투명도 사용 하 여 정수 RGB 값
- [`Color.FromHsla`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) 에 대 한 `double` 투명도 사용 하 여 HSL 값
- [`Color.FromUint`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromUint/p/System.UInt32/) 에 대 한는 `uint` 값으로 계산 됩니다 (B + 256 * (256 + G * (256 + R * A)))
- [`Color.FromHex`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) 에 대 한는 `string` 형태로 16 진수 숫자의 형식을 "#AARRGGBB" 또는 "#RRGGBB" 또는 "#ARGB" 또는 "#RGB", 여기서 각 문자는 16 진수에 해당에 알파, 빨간색, 녹색 및 파란색 채널입니다. 이 메서드는에 설명 된 대로 XAML 색 변환에 사용 되는 기본 [7 장, XAML 코드와 비교](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md)합니다.

일단 만들어지면는 `Color` 값은 변경할 수 있습니다. 색의 특징은 다음 속성에서 가져올 수 있습니다.

- [`R`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/)
- [`G`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/)
- [`B`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/)
- [`A`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/)
- [`Hue`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Hue/)
- [`Saturation`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Saturation/)
- [`Luminosity`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Luminosity/)

이 모든 항목은 `double` 0에서 1 사이의 값입니다.

`Color` 또한 240 공용 정적 읽기 전용 필드에 대 한 일반적인 색을 정의합니다. 일반적인 색만 17 책 작성 된 시간에 사용할 수 없었습니다.

다른 공용 정적 읽기 전용 필드를 0으로 설정 하는 모든 색 채널 색을 정의 합니다.

- [`Color.Transparent`](https://developer.xamarin.com/api/field/Xamarin.Forms.Color.Transparent/)

여러 인스턴스 메서드와 새 색을 만들려면 기존 색 수정 허용:

- [`AddLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.AddLuminosity/p/System.Double/)
- [`MultiplyAlpha`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.MultiplyAlpha/p/System.Double/)
- [`WithHue`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithHue/p/System.Double/)
- [`WithLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithLuminosity/p/System.Double/)
- [`WithSaturation`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithSaturation/p/System.Double/)

마지막으로, 두 개의 정적 읽기 전용 속성 특별 한 색상 값을 정의합니다.

- [`Color.Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/)을로 설정 된 모든 채널 &ndash;1
- [`Color.Accent`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Accent/)

`Color.Default` 플랫폼의 색 구성표를 적용 되며 결과적으로 서로 다른 플랫폼에서 다양 한 상황에서 다른 의미를 합니다. 기본적으로 플랫폼 색 구성표 있습니다.

- iOS: 연한 배경에 어두운 텍스트
- Android: 연한 책) (에 어두운 화면에 텍스트 또는 연한 배경에 어두운 텍스트 (에서 AppCompat 통해 자료 디자인에 대 한는 **마스터** 의 샘플 코드 리포지토리 분기)
- 연한 배경에 UWP: 어두운 텍스트
- Windows 8.1: 연한 텍스트에 어두운 화면에서
- 에 어두운 화면에서 Windows Phone 8.1: 연한 텍스트

`Color.Accent` 어둡게 또는 밝게 배경에 표시 되는 플랫폼 특정 (및 경우에 따라 사용자가 선택할 수 있는) 색의 결과 값입니다.

## <a name="changing-the-application-color-scheme"></a>응용 프로그램 색 구성표 변경

다양 한 플랫폼에 기본 색 구성표 위 목록에 표시 된 것 처럼 존재 합니다.

Android.Manifest.xml 파일 또는 여 밝은 테마를 지정 하 여 light에 어두운 구성표로 전환 수는 Android을 대상으로 지정 하는 경우 [AppCompat 추가 및 자료 디자인](~/xamarin-forms/platform/android/appcompat.md)합니다.

Windows 플랫폼에서 색 테마 일반적으로 사용자가 선택 되어 있지만 추가할 수는 `RequestedTheme` 특성 중 하나로 설정 `Light` 또는 `Dark` 플랫폼의 App.xaml 파일에 있습니다. UWP 프로젝트에서 App.xaml 파일은 기본적으로 포함 된 `RequestedTheme` 특성이로 설정 `Light`합니다.

## <a name="font-sizes-and-attributes"></a>글꼴 크기 및 특성

설정의 [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontFamily/) 속성 `Label` "Times Roman" 글꼴 패밀리를 선택 하려면 같은 문자열로 합니다. 그러나 특정 플랫폼에서 지원 되는 글꼴 패밀리를 지정 해야 하 고 이런 점에서 플랫폼 일치 하지 않습니다.

설정의 [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontSize/) 속성 `Label` 에 `double` 글꼴의 대략적인 높이 지정 하기 위한 합니다. 참조 [5 장 크기를 다루는](chapter05.md)에 대 한 자세한 내용은 지능적으로 글꼴 크기를 선택 합니다.

또는 여러 미리 설정 된 플랫폼에 종속 된 글꼴 크기 중 하나를 가져올 수 있습니다. 정적 [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) 메서드 및 [오버 로드](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/Xamarin.Forms.Element/) 둘 다 반환는 `double` 플랫폼에 적합 한 글꼴 크기 값의 멤버에 따라는 [ `NamedSize` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NamedSize/)열거형 ([`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Default/), [ `Micro` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Micro/), [ `Small` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Small/), [ `Medium` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Medium/),  및 [ `Large` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Large/)). 반환 된 값은 `Medium` 멤버가 아닐 동일 `Default`합니다. [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) 샘플 이러한 명명 된 크기와 텍스트를 표시 합니다.

설정의 [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontAttributes/) 속성 `Label` 이러한 멤버에 [ `FontAttributes` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FontAttributes/) 열거형 [ `Bold` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Bold/), [ `Italic` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Italic/), 또는 [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.None/)합니다. 결합할 수는 `Bold` 및 `Italic` C# 비트 OR 연산자를 가진 멤버입니다.

## <a name="formatted-text"></a>서식 있는 텍스트

모든 예제에서는 지금까지,으로 표시 하는 전체 텍스트는 `Label` 균일 하 게 형식이 지정 되어 있습니다. 설정 하지 않으면 텍스트 문자열에서 서식 지정을 변경 하는 `Text` 속성 `Label`합니다. 대신, 설정는 [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) 속성 형식의 개체로 [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/)합니다.

`FormattedString` 에 [ `Spans` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FormattedString.Spans/) 의 컬렉션인 속성 [ `Span` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Span/) 개체입니다. 각 `Span` 개체에는 자체 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.Text/), [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontFamily/), [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontSize/), [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontAttributes/), [ `ForegroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.ForegroundColor/), 및 [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.BackgroundColor/) 속성입니다.

[ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) 샘플 사용을 보여 줍니다.는 `FormattedText` 의 텍스트를 한 줄에 대 한 속성 및 [ **VariableFormattedParagraph** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) 다음과 같이 단락 전체 기법을 보여 줍니다.

[![변수의 세 스크린 샷 단락 서식이 지정 된](images/ch03fg06-small.png "변수 형식의 레이블 텍스트")](images/ch03fg06-large.png#lightbox "변수 형식의 레이블 텍스트")

[ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) 프로그램 하나를 사용 하 여 `Label` 및 `FormattedString` 모든 각 플랫폼에 대 한 명명 된 글꼴 크기를 표시 하는 개체입니다.



## <a name="related-links"></a>관련 링크

- [3 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [3 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Chapter 3 F # 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [레이블](~/xamarin-forms/user-interface/text/label.md)
- [색 작업](~/xamarin-forms/user-interface/colors.md)
