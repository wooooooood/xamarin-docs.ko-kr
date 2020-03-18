---
title: 15장 요약 대화형 인터페이스
description: 'Xamarin.Forms로 Mobile Apps 앱 만들기: 15장 요약 대화형 인터페이스'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 5f96d2f4b619bbb10bb58e9b1b5dc7007c1ce888
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "77131098"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>15장 요약 대화형 인터페이스

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)

이 장에서는 사용자와 상호 작용할 수 있는 8개의 `View` 파생 항목을 살펴봅니다.

## <a name="view-overview"></a>개요 보기

Xamarin.Forms에는 `Layout`이 아닌 `View`에서 파생되는 20개의 인스턴스화할 수 있는 클래스가 포함되어 있습니다. 이 중 6개는 이전 장에서 다루었습니다.

- `Label`: [**2장 앱 분석**](chapter02.md)
- `BoxView`: [**3장 스택 스크롤**](chapter03.md)
- `Button`: [**6장 단추 클릭**](chapter06.md)
- `Image`: [**13장 비트맵**](chapter13.md)
- `ActivityIndicator`: [**13장 비트맵**](chapter13.md)
- `ProgressBar`: [**14장 AbsoluteLayout**](chapter14.md)

이 장의 8가지 보기를 통해 사용자가 기본 .NET 데이터 형식과 효과적으로 상호 작용할 수 있습니다.

|데이터 형식|뷰|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider), [`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor), [`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker), [`TimePicker`](xref:Xamarin.Forms.TimePicker)|

이러한 보기를 기본 데이터 형식의 시각적 대화형 표현으로 간주할 수 있습니다. 이 개념은 다음 장인 [**16장 데이터 바인딩**](chapter16.md)에서 자세히 살펴봅니다.

나머지 6개 보기는 다음 장에서 다룹니다.

- `WebView`: [**16장 데이터 바인딩**](chapter16.md)
- `Picker`: [**19장 컬렉션 뷰**](chapter19.md)
- `ListView`: [**19장 컬렉션 뷰**](chapter19.md)
- `TableView`: [**19장 컬렉션 뷰**](chapter19.md)
- `Map`: [**28장 위치 및 지도**](chapter28.md)
- `OpenGLView`: 이 책에서 다루지 않음(Windows 플랫폼에 대한 지원 없음)

## <a name="slider-and-stepper"></a>슬라이더 및 스텝퍼

사용자는 [`Slider`](xref:Xamarin.Forms.Slider) 및 [`Stepper`](xref:Xamarin.Forms.Stepper) 모두 범위에서 숫자 값을 선택할 수 있습니다. `Slider`는 연속 범위인 반면 `Stepper`는 불연속 값을 포함합니다.

### <a name="slider-basics"></a>슬라이더 기본 사항

[`Slider`](xref:Xamarin.Forms.Slider)는 왼쪽의 최솟값부터 오른쪽의 최댓값까지 값 범위를 나타내는 가로 막대입니다. 다음 세 가지 public 속성을 정의합니다.

- 기본 값이 0인 `double` 형식의 [`Value`](xref:Xamarin.Forms.Slider.Value)
- 기본 값이 0인 `double` 형식의 [`Minimum`](xref:Xamarin.Forms.Slider.Minimum)
- 기본 값이 1인 `double` 형식의 [`Maximum`](xref:Xamarin.Forms.Slider.Maximum)

이러한 속성을 지원하는 바인딩 가능한 속성은 일관성을 유지합니다.

- 세 가지 속성 모두에 대해 바인딩 가능한 속성에 지정된 [`coerceValue`](xref:Xamarin.Forms.BindableProperty.CoerceValueDelegate) 메서드는 `Value`가 `Minimum`과 `Maximum` 사이에 있도록 합니다.
- `Minimum`이 `Maximum`보다 크거나 같은 값으로 설정되어 있고 `MaximumProperty`와 같은 값으로 설정되어 있는 경우 `MinimumProperty`의 [`validateValue`](xref:Xamarin.Forms.BindableProperty.ValidateValueDelegate) 메서드가 `false`를 반환합니다. `validateValue` 메서드에서 `false`를 반환하면 `ArgumentException`이 발생합니다.

`Value` 속성이 프로그래밍 방식으로 변경되거나 사용자가 `Slider`를 조작할 때 변경되면 `Slider`는 [`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs) 인수를 사용하여 [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) 이벤트를 발생시킵니다.

[**SliderDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) 샘플은 `Slider`의 간단히 사용 방법을 보여줍니다.

### <a name="common-pitfalls"></a>공통 문제

코드와 XAML 모두에서 `Minimum` 및 `Maximum` 속성은 지정한 순서대로 설정됩니다. `Maximum`이 `Minimum`보다 항상 크도록 이러한 속성을 초기화해야 합니다. 그렇지 않으면 예외가 발생합니다.

`Slider` 속성을 초기화하면 `Value` 속성이 변경되고 `ValueChanged` 이벤트가 발생합니다. 페이지 초기화 중에 아직 생성되지 않은 보기에 `Slider` 이벤트 처리기가 액세스하지 않도록 합니다.

`Value` 속성이 변경되는 않는 한 `Slider` 초기화 중에 `ValueChanged` 이벤트가 발생하지 않습니다. 코드에서 직접 `ValueChanged` 처리기를 호출할 수 있습니다.

### <a name="slider-color-selection"></a>슬라이더 색 선택

[**RgbSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) 프로그램에는 RGB 값을 지정하여 색을 대화형으로 선택할 수 있는 세 가지 `Slider` 요소가 포함되어 있습니다.

[![R G B 슬라이더의 삼중 스크린샷](images/ch15fg03-small.png "RGB 슬라이더")](images/ch15fg03-large.png#lightbox "RGB 슬라이더")

[**TextFade**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) 샘플은 두 개의 `Slider` 요소를 사용하여 두 개의 `Label` 요소를 `AbsoluteLayout`에서 이동하고 다른 요소로 페이드 처리합니다.

### <a name="the-stepper-difference"></a>스텝퍼 차이점

[`Stepper`](xref:Xamarin.Forms.Stepper)는 `Slider`와 동일한 속성 및 이벤트를 정의하지만 `Maximum` 속성은 100으로 초기화되고 `Stepper`는 네 번째 속성을 정의합니다.

- 1로 초기화되는 `double` 형식의 [`Increment`](xref:Xamarin.Forms.Stepper.Increment)

시각적으로 `Stepper`는 **&ndash;** 및 **+** 라는 레이블이 지정된 두 개의 단추로 구성되어 있습니다. **&ndash;** 를 누르면 `Value`가 `Increment`에 따라 최소 `Minimum`까지 줄어듭니다. **+** 를 누르면 `Value`가 `Increment`에 따라 최대 `Maximum`까지 늘어납니다.

이는 [**StepperDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) 샘플에서 설명합니다.

## <a name="switch-and-checkbox"></a>스위치 및 확인란

사용자는 [`Switch`](xref:Xamarin.Forms.Switch)를 사용하여 부울 값을 지정할 수 있습니다.

### <a name="switch-basics"></a>스위치 기본 사항

시각적으로 `Switch`는 설정/해제할 수 있는 토글로 구성됩니다. 클래스는 하나의 속성을 정의합니다.

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)(`bool` 형식)

`Switch`는 하나의 이벤트를 정의합니다.

- `IsToggled` 속성이 변경될 때 발생하는 [`ToggledEventArgs`](xref:Xamarin.Forms.ToggledEventArgs) 개체와 함께 [`Toggled`](xref:Xamarin.Forms.Switch.Toggled)를 제공합니다.

[**SwitchDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) 프로그램은 `Switch`를 보여줍니다.

### <a name="a-traditional-checkbox"></a>기존 확인란

일부 개발자는 `Switch`보다 기존 `CheckBox`를 선호하는 경우가 있습니다. [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 `ContentView`에서 파생된 `CheckBox` 클래스가 포함되어 있습니다. `CheckBox`는 [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) 및 [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) 파일을 통해 구현됩니다. `CheckBox`는 세 가지 속성(`Text`, `FontSize` 및 `IsChecked`) 및 `CheckedChanged` 이벤트를 정의합니다.

[**CheckBoxDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) 샘플은 이 `CheckBox`를 보여줍니다.

## <a name="typing-text"></a>텍스트 입력

Xamarin.Forms는 사용자가 텍스트를 입력하고 편집할 수 있도록 하는 세 가지 보기를 정의합니다.

- 한 줄의 텍스트에 대한 [`Entry`](xref:Xamarin.Forms.Entry)
- 여러 줄 텍스트에 대한 [`Editor`](xref:Xamarin.Forms.Editor)
- 검색을 위한 한 줄의 텍스트에 대한 [`SearchBar`](xref:Xamarin.Forms.SearchBar).

`Entry` 및 `Editor`는 `View`에서 파생된 [`InputView`](xref:Xamarin.Forms.InputView)에서 파생됩니다. `SearchBar`는 `View`에서 직접 파생됩니다.

### <a name="keyboard-and-focus"></a>키보드 및 포커스

실제 키보드가 없는 휴대폰 및 태블릿에서는 `Entry`, `Editor` 및 `SearchBar` 요소가 모두 가상 키보드가 팝업되는 원인이 됩니다. 화면에 이 키보드의 현재 상태는 입력 포커스와 관련이 있습니다. 보기에 입력 포커스를 가져오려면 [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) 및 [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) 속성이 모두 `true`로 설정되어 있어야 합니다.

두 메서드, 하나의 읽기 전용 속성 및 두 개의 이벤트가 입력 포커스와 관련되어 있습니다. 이들은 모두 `VisualElement`에 의해 정의됩니다.

- [`Focus`](xref:Xamarin.Forms.VisualElement.Focus) 메서드는 입력 포커스를 요소로 설정하려고 시도하고 성공한 경우 `true`를 반환합니다.
- [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus) 메서드는 요소에서 입력 포커스를 제거합니다.
- [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) 읽기 전용 속성은 요소에 입력 포커스가 있는지 여부를 나타냅니다.
- [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) 이벤트는 요소가 입력 포커스를 가져오는 시기를 나타냅니다.
- [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) 이벤트는 요소가 입력 포커스를 잃을 때를 나타냅니다.

### <a name="choosing-the-keyboard"></a>키보드 선택

`Entry` 및 `Editor`가 파생되는 [`InputView`](xref:Xamarin.Forms.InputView) 클래스는 하나의 속성만 정의합니다.

- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard)([`Keyboard`](xref:Xamarin.Forms.Keyboard) 형식)

이는 표시되는 키보드의 유형을 나타냅니다. 일부 키보드는 URI 또는 숫자에 최적화되어 있습니다.

`Keyboard` 클래스를 통해 다음 비트 플래그가 있는 열거형인 [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) 형식의 인수를 사용하여 정적 [`Keyboard.Create`](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags)) 메서드로 키보드를 정의할 수 있습니다.

- `None`을 0으로 설정
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence)를 1로 설정
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck)를 2로 설정
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions)를 4로 설정
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All)을 \xFFFFFFFF로 설정

한 단락 이상의 텍스트가 필요할 때 여러 줄 [`Editor`](xref:Xamarin.Forms.Editor)를 사용하는 경우 `Keyboard.Create`를 호출하여 키보드를 선택하는 것이 좋은 방법입니다. 한 줄 [`Entry`](xref:Xamarin.Forms.Entry)의 경우 다음과 같은 정적 읽기 전용 속성 `Keyboard`가 유용합니다.

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric): 소수점이 있거나 없는 양수의 경우입니다.

[`KeyboardTypeConverter`](xref:Xamarin.Forms.KeyboardTypeConverter)를 사용하면 [**EntryKeyboards**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) 프로그램에서 설명한 대로 XAML에서 이러한 속성을 지정할 수 있습니다.

### <a name="entry-properties-and-events"></a>항목 속성 및 이벤트

한 줄 [`Entry`](xref:Xamarin.Forms.Entry)는 다음 속성을 정의합니다.

- `Entry`에 표시되는 텍스트인 `string` 형식의 [`Text`](xref:Xamarin.Forms.InputView.Text)
- [`TextColor`](xref:Xamarin.Forms.InputView.TextColor)(`Color` 형식)
- [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily)(`string` 형식)
- [`FontSize`](xref:Xamarin.Forms.Entry.FontSize)(`double` 형식)
- [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes)(`FontAttributes` 형식)
- 문자를 마스킹할 수 있는 `bool` 형식의 [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword)
- 입력하기 전에 `Entry`에 나타나는 희미한 색의 텍스트에 대한 `string` 형식의 [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder)
- [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor)(`Color` 형식)

`Entry`는 두 개의 이벤트도 정의합니다.

- [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) 개체가 있는 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged): `Text` 속성이 변경될 때마 발생합니다.
- [`Completed`](xref:Xamarin.Forms.Entry.Completed): 사용자가 완료되고 키보드가 해제될 때 발생합니다. 사용자가 플랫폼별 방식으로 완료되었음을 나타냅니다.

[**QuadraticEquations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) 샘플은 이러한 두 개의 이벤트를 보여줍니다.

### <a name="the-editor-difference"></a>편집기 차이점

여러 줄 [`Editor`](xref:Xamarin.Forms.Editor)는 `Entry`와 동일한 `Text` 및 `Font` 속성을 정의하지만 다른 속성은 정의하지 않습니다. `Editor`는 `Entry`와 동일한 두 개의 속성도 정의합니다.

[**JustNotes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes)는 `Editor`의 콘텐츠를 저장하고 복원하는 자유 형식의 노트 작성 프로그램입니다.

### <a name="the-searchbar"></a>SearchBar

[`SearchBar`](xref:Xamarin.Forms.SearchBar)는 `InputView`에서 파생되지 않으므로 `Keyboard` 속성이 없습니다. 하지만 `Entry`가 정의하는 모든 `Text`, `Font` 및 `Placeholder` 속성이 있습니다. 또한 `SearchBar`는 다음과 같은 세 가지 추가 속성을 정의합니다.

- [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor)(`Color` 형식)
- 데이터 바인딩과 MVVM을 함께 사용하기 위한 [`ICommand`](xref:System.Windows.Input.ICommand) 형식의 [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand)
- `SearchCommand`와 함께 사용하기 위한 `Object` 형식의 [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter)

플랫폼별 취소 단추를 클릭하면 텍스트가 지워집니다. `SearchBar`에는 플랫폼별 검색 단추도 있습니다. 이러한 단추 중 하나를 누르면 `SearchBar`가 정의하는 두 이벤트 중 하나가 발생합니다.

- [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) 개체가 수반되는 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

[**SearchBarDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) 샘플은 `SearchBar`를 보여줍니다.

## <a name="date-and-time-selection"></a>날짜 및 시간 선택

[`DatePicker`](xref:Xamarin.Forms.DatePicker) 및 [`TimePicker`](xref:Xamarin.Forms.TimePicker) 보기에서는 사용자가 날짜 또는 시간을 지정할 수 있도록 하는 플랫폼별 컨트롤을 구현합니다.

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](xref:Xamarin.Forms.DatePicker)는 다음과 같은 네 가지 속성을 정의합니다.

- `DateTime` 형식의 [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate)(1900년 1월 1일로 초기화됨)
- `DateTime` 형식의 [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate)(2100년 12월 31일로 초기화됨)
- `DateTime` 형식의 [`Date`](xref:Xamarin.Forms.DatePicker.Date)(`DateTime.Today`로 초기화됨)
- `string` 형식의 [`Format`](xref:Xamarin.Forms.DatePicker.Format): .NET 형식 지정 문자열이 간단한 날짜 패턴인 "d"로 초기화되었으며, 그 결과 미국의 "1969/7/20"과 같은 날짜가 표시됩니다.

속성을 속성 요소로 표현하고 문화권 고정 간단한 날짜 형식("1969/7/20")을 사용하여 XAML에서 `DateTime` 속성을 설정할 수 있습니다.   

[**DaysBetweenDates**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) 샘플은 사용자가 선택한 두 날짜 사이의 일 수를 계산합니다.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker(또는 TimeSpanPicker 인가요?)

[`TimePicker`](xref:Xamarin.Forms.TimePicker)는 두 가지 속성을 정의하고 이벤트는 정의하지 않습니다.

- [`Time`](xref:Xamarin.Forms.TimePicker.Time)는 자정 이후 경과된 시간을 나타내는 `DateTime`이 아닌 `TimeSpan` 형식입니다.
- `string` 형식의 [`Format`](xref:Xamarin.Forms.TimePicker.Format): .NET 형식 지정 문자열이 간단한 시간 패턴인 "t"로 초기화되었으며, 그 결과 미국의 "1:45 PM"과 같은 시간이 표시됩니다.

[**SetTimer**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) 프로그램은 `TimePicker`를 사용하여 타이머의 시간을 지정하는 방법을 보여줍니다. 이 프로그램은 전경에 보관하는 경우에만 작동합니다.

**SetTimer**는 `Page`의 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) 메서드를 사용하여 경고 상자를 표시하는 방법도 보여줍니다.

## <a name="related-links"></a>관련 링크

- [15장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [15장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [슬라이더](~/xamarin-forms/user-interface/slider.md)
- [항목](~/xamarin-forms/user-interface/text/entry.md)
- [편집기](~/xamarin-forms/user-interface/text/editor.md)
- [DatePicker](~/xamarin-forms/user-interface/datepicker.md)
