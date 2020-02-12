---
title: 요약 15 장입니다. 대화형 인터페이스
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 15 장 요약 합니다. 대화형 인터페이스'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 5f96d2f4b619bbb10bb58e9b1b5dc7007c1ce888
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77131098"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>요약 15 장입니다. 대화형 인터페이스

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)

이 장에서는 사용자와의 상호 작용을 허용 하는 8 개의 `View` 파생물을 살펴봅니다.

## <a name="view-overview"></a>개요 보기

Xamarin.ios는 `View`에서 파생 되지만 `Layout`되지 않는 20 개의 인스턴스화할 수 있는 클래스를 포함 합니다. 이러한 6 이전 챕터에서 설명 되었을:

- `Label`: [ **2 장. 앱의 분석**](chapter02.md)
- `BoxView`: [ **3 장. 스택 스크롤**](chapter03.md)
- `Button`: [ **6 장. 단추 클릭**](chapter06.md)
- `Image`: [ **13 장. 비트맵**](chapter13.md)
- `ActivityIndicator`: [ **13 장. 비트맵**](chapter13.md)
- `ProgressBar`: [ **14 장. AbsoluteLayout**](chapter14.md)

이 챕터에 8 뷰는 기본.NET 데이터 유형과 상호 작용할 수를 효과적으로 허용:

|데이터 형식|뷰|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider), [`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor), [`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker), [`TimePicker`](xref:Xamarin.Forms.TimePicker)|

이러한 뷰는 기본 데이터 형식의 대화형 시각적 표시로 생각할 수 있습니다. 이 개념은 다음 장 16 장에서 자세히 설명 [**합니다. 데이터 바인딩**](chapter16.md).

나머지 6 개의 뷰가 다음 챕터에서 적용 됩니다.

- `WebView`: [ **16 장. 데이터 바인딩**](chapter16.md)
- `Picker`: [ **19 장. 컬렉션 뷰**](chapter19.md)
- `ListView`: [ **19 장. 컬렉션 뷰**](chapter19.md)
- `TableView`: [ **19 장. 컬렉션 뷰**](chapter19.md)
- `Map`: [ **28 장. 위치 및 지도**](chapter28.md)
- `OpenGLView`:이 책에서 다루지 않음 (Windows 플랫폼에 대 한 지원 없음)

## <a name="slider-and-stepper"></a>슬라이더 및 스텝 퍼

사용자는 [`Slider`](xref:Xamarin.Forms.Slider) 와 [`Stepper`](xref:Xamarin.Forms.Stepper) 모두 범위에서 숫자 값을 선택할 수 있습니다. `Stepper`는 불연속 값을 포함 하는 동안 `Slider` 연속 범위입니다.

### <a name="slider-basics"></a>슬라이더 기본 사항

[`Slider`](xref:Xamarin.Forms.Slider) 은 왼쪽의 최소값부터 오른쪽의 최대값까지 값의 범위를 나타내는 가로 막대입니다. 세 가지 공용 속성을 정의 합니다.

- `double`형식의 [`Value`](xref:Xamarin.Forms.Slider.Value) 이며 기본값은 0입니다.
- `double`형식의 [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) 이며 기본값은 0입니다.
- `double`형식의 [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) 기본값 1

이러한 속성을 백업 하는 바인딩 가능한 속성 일치 하는지 확인 합니다.

- 세 가지 속성 모두에 대해 바인딩 가능 속성에 대해 지정 된 [`coerceValue`](xref:Xamarin.Forms.BindableProperty.CoerceValueDelegate) 메서드는 `Value` `Minimum`와 `Maximum`사이에 있는지 확인 합니다.
- `MinimumProperty`에 대 한 [`validateValue`](xref:Xamarin.Forms.BindableProperty.ValidateValueDelegate) 메서드는 `Minimum` `Maximum`보다 크거나 같은 값으로 설정 되는 경우 `false`을 반환 하 고 `MaximumProperty`에 대해서도 유사 하 게 반환 합니다. `validateValue` 메서드에서 `false` 반환 하면 `ArgumentException` 발생 합니다.

`Slider`는 `Value` 속성이 변경 될 때 또는 사용자가 `Slider`을 조작할 때 [`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs) 인수를 사용 하 여 [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) 이벤트를 발생 시킵니다.

[**SliderDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) 샘플은 `Slider`를 간단 하 게 사용 하는 방법을 보여 줍니다.

### <a name="common-pitfalls"></a>공통 문제

코드와 XAML 모두에서 `Minimum` 및 `Maximum` 속성은 지정한 순서 대로 설정 됩니다. `Maximum`이 `Minimum`보다 항상 큰지 이러한 속성을 초기화 해야 합니다. 그렇지 않으면 예외가 발생합니다.

`Slider` 속성을 초기화 하면 `Value` 속성이 변경 되 고 `ValueChanged` 이벤트가 발생 합니다. `Slider` 이벤트 처리기가 페이지 초기화 중 아직 생성 되지 않은 뷰에 액세스 하지 않는지 확인 해야 합니다.

`Value` 속성이 변경 되는 경우를 제외 하 고는 `Slider` 초기화 중에 `ValueChanged` 이벤트가 발생 하지 않습니다. 코드에서 직접 `ValueChanged` 처리기를 호출할 수 있습니다.

### <a name="slider-color-selection"></a>슬라이더 색 선택

[**RgbSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) 프로그램에는 RGB 값을 지정 하 여 색을 대화형으로 선택할 수 있는 세 가지 `Slider` 요소가 포함 되어 있습니다.

[![R G B 슬라이더의 삼중 스크린샷](images/ch15fg03-small.png "RGB 슬라이더")](images/ch15fg03-large.png#lightbox "RGB 슬라이더")

[**Textfade**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) 샘플은 두 개의 `Slider` 요소를 사용 하 여 `AbsoluteLayout`에서 두 개의 `Label` 요소를 이동 하 고 다른 요소에 대해 페이드를 합니다.

### <a name="the-stepper-difference"></a>스텝 퍼 차이

[`Stepper`](xref:Xamarin.Forms.Stepper) 는 `Slider`와 동일한 속성 및 이벤트를 정의 하지만 `Maximum` 속성은 100로 초기화 되 고 `Stepper`는 네 번째 속성을 정의 합니다.

- `double`형식의 [`Increment`](xref:Xamarin.Forms.Stepper.Increment) 1로 초기화 됨

시각적으로 `Stepper`는 **&ndash;** 및 **+** 레이블이 지정 된 두 개의 단추로 구성 됩니다. **&ndash;** 를 누르면 최소한의 `Minimum``Increment` 하 여 `Value` 줄어듭니다. **+** 를 누르면 최대 `Maximum``Increment` 여 `Value` 늘어납니다.

이는 [**Stepperdemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) 샘플에서 보여 줍니다.

## <a name="switch-and-checkbox"></a>스위치 및 확인란

사용자는 [`Switch`](xref:Xamarin.Forms.Switch) 를 사용 하 여 부울 값을 지정할 수 있습니다.

### <a name="switch-basics"></a>기본 전환

시각적으로 `Switch`는 설정/해제할 수 있는 토글으로 구성 됩니다. 클래스는 하나의 속성을 정의합니다.

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)(`bool` 형식)

`Switch`는 하나의 이벤트를 정의 합니다.

- `IsToggled` 속성이 변경 될 때 발생 하는 [`ToggledEventArgs`](xref:Xamarin.Forms.ToggledEventArgs) 개체와 함께 [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) .

[**Switchdemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) 프로그램은 `Switch`를 보여 줍니다.

### <a name="a-traditional-checkbox"></a>기존의 확인란

일부 개발자는 `Switch`보다 기존 `CheckBox`를 선호 하는 경우가 있습니다. [**Xamarin.ios**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리는 `ContentView`에서 파생 되는 `CheckBox` 클래스를 포함 합니다. `CheckBox`은 [CheckBox.xaml.cs 및](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) 파일에 의해 구현 [됩니다.](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) `CheckBox`는 세 가지 속성 (`Text`, `FontSize`및 `IsChecked`)과 `CheckedChanged` 이벤트를 정의 합니다.

[**CheckBoxDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) 샘플에서는이 `CheckBox`를 보여 줍니다.

## <a name="typing-text"></a>텍스트를 입력합니다.

Xamarin.Forms는 사용자가 입력 하 고 텍스트를 편집할 수 있는 세 가지 뷰를 정의 합니다.

- 텍스트 한 줄에 대 한 [`Entry`](xref:Xamarin.Forms.Entry)
- 여러 줄의 텍스트에 대 한 [`Editor`](xref:Xamarin.Forms.Editor)
- 검색을 위해 한 줄의 텍스트를 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 합니다.

`Entry` 및 `Editor`는 `View`에서 파생 되는 [`InputView`](xref:Xamarin.Forms.InputView)에서 파생 됩니다. `SearchBar`은 `View`에서 직접 파생 됩니다.

### <a name="keyboard-and-focus"></a>키보드 및 포커스

실제 키보드가 없는 휴대폰 및 태블릿에서 `Entry`, `Editor`및 `SearchBar` 요소가 있으면 모두 가상 키보드가 팝업 됩니다. 화면에이 키보드의 현재 상태는 입력 포커스가 관련이 있습니다. 뷰의 [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) 와 [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) 속성은 입력 포커스를 얻기 위해 `true`로 설정 되어야 합니다.

입력된 포커스가 있는 두 가지 방법, 읽기 전용 속성 및 두 개의 이벤트가 포함 됩니다. 이러한 작업은 모두 `VisualElement`에 의해 정의 됩니다.

- [`Focus`](xref:Xamarin.Forms.VisualElement.Focus) 메서드는 입력 포커스를 요소에 설정 하려고 시도 하 고 성공 하면 `true`을 반환 합니다.
- [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus) 메서드는 요소에서 입력 포커스를 제거 합니다.
- [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) 읽기 전용 속성은 요소에 입력 포커스가 있는지 여부를 나타냅니다.
- [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) 이벤트는 요소가 입력 포커스를 가져오는 시기를 나타냅니다.
- [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) 이벤트는 요소가 입력 포커스를 잃을 때를 나타냅니다.

### <a name="choosing-the-keyboard"></a>키보드를 선택합니다.

`Entry` 및 `Editor` 파생 된 [`InputView`](xref:Xamarin.Forms.InputView) 클래스는 하나의 속성만 정의 합니다.

- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard)([`Keyboard`](xref:Xamarin.Forms.Keyboard) 형식)

표시 되는 키보드의 종류를 나타냅니다. 일부 키보드 Uri 또는 숫자 최적화 됩니다.

`Keyboard` 클래스를 사용 하면 다음 비트 플래그를 사용 하는 열거형 인 [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags)형식의 인수를 사용 하 여 정적 [`Keyboard.Create`](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags)) 메서드를 사용 하 여 키보드를 정의할 수 있습니다.

- 0으로 설정 `None`
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) 1로 설정
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) 2로 설정
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) 4로 설정
- \xFFFFFFFF로 설정 [`All`](xref:Xamarin.Forms.KeyboardFlags.All)

여러 줄 [`Editor`](xref:Xamarin.Forms.Editor) 사용 하 여 단락이 나 텍스트가 필요한 경우에는 `Keyboard.Create`를 호출 하 여 키보드를 선택 하는 것이 좋은 방법입니다. 한 줄 [`Entry`](xref:Xamarin.Forms.Entry)의 경우 `Keyboard`의 다음과 같은 정적 읽기 전용 속성이 유용 합니다.

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- 소수점이 있거나 없는 양수의 경우 [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) 합니다.

[`KeyboardTypeConverter`](xref:Xamarin.Forms.KeyboardTypeConverter) 는 [**entrykeyboards**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) 프로그램에서 설명한 대로 XAML에서 이러한 속성을 지정할 수 있습니다.

### <a name="entry-properties-and-events"></a>항목 속성 및 이벤트

한 줄 [`Entry`](xref:Xamarin.Forms.Entry) 는 다음 속성을 정의 합니다.

- `string`형식의 [`Text`](xref:Xamarin.Forms.InputView.Text) `Entry`에 표시 되는 텍스트입니다.
- [`TextColor`](xref:Xamarin.Forms.InputView.TextColor)(`Color` 형식)
- [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily)(`string` 형식)
- [`FontSize`](xref:Xamarin.Forms.Entry.FontSize)(`double` 형식)
- [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes)(`FontAttributes` 형식)
- 문자를 마스킹할 수 있는 `bool`형식의 [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword)
- 지정 된 항목을 입력 하기 전에 `Entry`에 표시 되는 dimly 색 텍스트에 대 한 `string`형식의 [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder)
- [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor)(`Color` 형식)

또한 `Entry`는 두 개의 이벤트를 정의 합니다.

- `Text` 속성이 변경 될 때마다 발생 하는 [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) 개체를 사용 하 여 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)
- [`Completed`](xref:Xamarin.Forms.Entry.Completed)사용자가 완료 되 고 키보드가 해제 될 때 발생 합니다. 사용자는 플랫폼 특정 방식으로 완료를 나타냅니다.

[**QuadraticEquations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) 샘플에서는 이러한 두 이벤트를 보여 줍니다.

### <a name="the-editor-difference"></a>편집기 차이

여러 줄 [`Editor`](xref:Xamarin.Forms.Editor) 는 동일한 `Text` 및 `Font` 속성을 `Entry` 하지만 다른 속성은 정의 하지 않습니다. 또한 `Editor` `Entry`와 동일한 두 개의 속성을 정의 합니다.

[**JustNotes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) 는 `Editor`의 내용을 저장 하 고 복원 하는 자유 형식의 노트 작성 프로그램입니다.

### <a name="the-searchbar"></a>SearchBar

[`SearchBar`](xref:Xamarin.Forms.SearchBar) 은 `InputView`에서 파생 되지 않으므로 `Keyboard` 속성이 없습니다. 하지만 `Entry`에서 정의 하는 모든 `Text`, `Font`및 `Placeholder` 속성이 있습니다. 또한 `SearchBar`는 다음과 같은 세 가지 추가 속성을 정의 합니다.

- [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor)(`Color` 형식)
- 데이터 바인딩 및 MVVM와 함께 사용할 형식의 [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) [`ICommand`](xref:System.Windows.Input.ICommand)
- `SearchCommand`에 사용할 `Object`형식의 [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter)

플랫폼 특정 텍스트 지우기 단추를 취소 합니다. 또한 `SearchBar`에는 플랫폼별 검색 단추도 있습니다. 이러한 단추 중 하나를 누르면 `SearchBar` 정의 하는 두 이벤트 중 하나가 발생 합니다.

- [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) 개체 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 수반
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

[**Search바 데모**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) 샘플은 `SearchBar`를 보여 줍니다.

## <a name="date-and-time-selection"></a>날짜 및 시간 선택

[`DatePicker`](xref:Xamarin.Forms.DatePicker) 및 [`TimePicker`](xref:Xamarin.Forms.TimePicker) 뷰는 사용자가 날짜 또는 시간을 지정할 수 있도록 하는 플랫폼별 컨트롤을 구현 합니다.

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](xref:Xamarin.Forms.DatePicker) 는 네 가지 속성을 정의 합니다.

- 1900 년 1 월 1 일로 초기화 된 `DateTime`형식의 [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate)
- `DateTime`형식의 [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) 2100 년 12 월 31 일까 지 초기화 됨
- `DateTime`형식의 [`Date`](xref:Xamarin.Forms.DatePicker.Date) `DateTime.Today`으로 초기화 됩니다.
- "d"로 초기화 되는 .NET 형식 지정 문자열인 "d"로 초기화 되는 형식 `string`의 [`Format`](xref:Xamarin.Forms.DatePicker.Format) 입니다 .이는 미국의 "7/20/1969"과 같은 날짜를 표시 합니다.

속성을 속성 요소로 표현 하 고 문화권 고정 간단한 날짜 형식 ("7/20/1969")을 사용 하 여 XAML에서 `DateTime` 속성을 설정할 수 있습니다.   

[**DaysBetweenDates**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) 샘플은 사용자가 선택한 두 날짜 사이의 일 수를 계산 합니다.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (또는 TimeSpanPicker 것)

[`TimePicker`](xref:Xamarin.Forms.TimePicker) 는 두 가지 속성 및 이벤트를 정의 하지 않습니다.

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) 자정 이후 경과 된 시간을 나타내는 `DateTime`이 아니라 `TimeSpan` 유형입니다.
- `string`형식의 [`Format`](xref:Xamarin.Forms.TimePicker.Format) "t"로 초기화 되는 .net 형식 지정 문자열 (간단한 시간 패턴)으로, 미국의 "1:45 PM"과 같이 시간이 표시 됩니다.

[**Settimer**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) 프로그램은 `TimePicker`를 사용 하 여 타이머에 대 한 시간을 지정 하는 방법을 보여 줍니다. 프로그램을 전경에 유지 하는 경우에 작동 합니다.

또한 **Settimer** 는 `Page`의 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) 메서드를 사용 하 여 경고 상자를 표시 하는 방법을 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [15 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [15 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [슬라이더](~/xamarin-forms/user-interface/slider.md)
- [항목](~/xamarin-forms/user-interface/text/entry.md)
- [편집기](~/xamarin-forms/user-interface/text/editor.md)
- [DatePicker](~/xamarin-forms/user-interface/datepicker.md)
