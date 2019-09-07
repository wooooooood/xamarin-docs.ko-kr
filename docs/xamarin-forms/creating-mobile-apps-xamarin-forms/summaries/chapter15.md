---
title: 요약 15 장입니다. 대화형 인터페이스
description: 'Xamarin.ios를 사용 하 여 Mobile Apps 만들기: 요약 15 장입니다. 대화형 인터페이스'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 1c30f87b9173d2ca4de0b2d91ad13145031e9b0a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760766"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>요약 15 장입니다. 대화형 인터페이스

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)

이 챕터에서는 8 `View` 파생형을 사용자와 상호 작용을 허용 합니다.

## <a name="view-overview"></a>개요 보기

파생 되는 20 인스턴스화할 수 있는 클래스를 포함 하는 Xamarin.Forms `View` 있지만 `Layout`합니다. 이러한 6 이전 챕터에서 설명 되었을:

- `Label`: [**2 장. 앱 분석**](chapter02.md)
- `BoxView`: [**3 장. 스택 스크롤**](chapter03.md)
- `Button`: [**6 장. 단추 클릭**](chapter06.md)
- `Image`: [**13 장. Bitmaps**](chapter13.md)
- `ActivityIndicator`: [**13 장. Bitmaps**](chapter13.md)
- `ProgressBar`: [**14 장. AbsoluteLayout**](chapter14.md)

이 챕터에 8 뷰는 기본.NET 데이터 유형과 상호 작용할 수를 효과적으로 허용:

|데이터 형식|보기|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider), [`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor), [`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker), [`TimePicker`](xref:Xamarin.Forms.TimePicker)|

이러한 뷰는 기본 데이터 형식의 대화형 시각적 표시로 생각할 수 있습니다. 이 개념에에서 살펴봅니다 더 다음 장에서 [ **16 장입니다. 데이터 바인딩**](chapter16.md)합니다.

나머지 6 개의 뷰가 다음 챕터에서 적용 됩니다.

- `WebView`: [**16 장. 데이터 바인딩**](chapter16.md)
- `Picker`: [**19 장. 컬렉션 뷰**](chapter19.md)
- `ListView`: [**19 장. 컬렉션 뷰**](chapter19.md)
- `TableView`: [**19 장. 컬렉션 뷰**](chapter19.md)
- `Map`: [**28 장. 위치 및 지도**](chapter28.md)
- `OpenGLView`: 이 설명서에서 다루지 않음 (Windows 플랫폼에 대 한 지원 없음)

## <a name="slider-and-stepper"></a>슬라이더 및 스텝 퍼

둘 다 [ `Slider` ](xref:Xamarin.Forms.Slider) 하 고 [ `Stepper` ](xref:Xamarin.Forms.Stepper) 사용자 범위에서 숫자 값을 선택할 수 있도록 합니다. `Slider` 하면서 연속 범위는 `Stepper` 불연속 값을 포함 합니다.

### <a name="slider-basics"></a>슬라이더 기본 사항

합니다 [ `Slider` ](xref:Xamarin.Forms.Slider) 가로 왼쪽에 최소에서 오른쪽에 대 한 최대값으로 값의 범위를 나타내는 막대 됩니다. 세 가지 공용 속성을 정의 합니다.

- [`Value`](xref:Xamarin.Forms.Slider.Value) 형식의 `double`, 0의 기본값
- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) 형식의 `double`, 0의 기본값
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) 형식의 `double`, 1의 기본값

이러한 속성을 백업 하는 바인딩 가능한 속성 일치 하는지 확인 합니다.

- 모든 세 가지 속성에는 [ `coerceValue` ](xref:Xamarin.Forms.BindableProperty.CoerceValueDelegate) 되도록 바인딩 가능한 속성에 대해 지정 된 메서드 `Value` 사이 `Minimum` 및 `Maximum`합니다.
- [ `validateValue` ](xref:Xamarin.Forms.BindableProperty.ValidateValueDelegate) 메서드를 `MinimumProperty` 반환 `false` 경우 `Minimum` 보다 크거나 같은 값으로 설정 되 `Maximum`, 및에 대 한 유사한 `MaximumProperty`합니다. 반환 `false` 에서 합니다 `validateValue` 메서드를 사용 하면은 `ArgumentException` 발생 합니다.

`Slider` 발생 합니다 [ `ValueChanged` ](xref:Xamarin.Forms.Slider.ValueChanged) 이벤트를 [ `ValueChangedEventArgs` ](xref:Xamarin.Forms.ValueChangedEventArgs) 인수 때를 `Value` 속성 변경 내용을 프로그래밍 방식으로 또는 경우에 사용자 조작를 `Slider`.

합니다 [ **SliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) 의 간단한 사용법을 보여 줍니다는 `Slider`합니다.

### <a name="common-pitfalls"></a>일반적인 문제

코드 및 XAML을 모두 합니다 `Minimum` 및 `Maximum` 속성이 지정 된 순서 대로 설정 됩니다. 이러한 속성을 초기화 해야 되도록 `Maximum` 보다 항상 큽니다 `Minimum`합니다. 그렇지 않으면 예외가 발생합니다.

초기화를 `Slider` 속성 발생할 수 있습니다 합니다 `Value` 속성을 변경 및 `ValueChanged` 이벤트를 발생 합니다. 인지 확인 해야 합니다 `Slider` 이벤트 처리기는 페이지를 초기화 하는 동안 아직 만들지 않은 보기에 액세스 하지 않습니다.

합니다 `ValueChanged` 하는 동안 이벤트가 발생 하지 않습니다 `Slider` 초기화 하지 않는 한는 `Value` 속성 변경 합니다. 호출할 수 있습니다는 `ValueChanged` 코드에서 직접 처리기입니다.

### <a name="slider-color-selection"></a>슬라이더 색 선택

[ **RgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) 프로그램을 포함 된 세 가지 `Slider` RGB 값을 지정 하 여 색을 대화식으로 선택할 수 있는 요소:

[![G B R 슬라이더의 삼중 스크린 샷](images/ch15fg03-small.png "RGB 슬라이더")](images/ch15fg03-large.png#lightbox "RGB 슬라이더")

[ **TextFade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) 샘플을 사용 하 여 두 `Slider` 이동할 두 요소 `Label` 에서 요소는 `AbsoluteLayout` 다른 주머니에 하나의 페이드할 및 합니다.

### <a name="the-stepper-difference"></a>스텝 퍼 차이

[ `Stepper` ](xref:Xamarin.Forms.Stepper) 동일한 속성 및 이벤트를 정의 `Slider` 하지만 `Maximum` 속성은 100으로 초기화 및 `Stepper` 네 번째 속성을 정의 합니다.

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) 형식의 `double`1 초기화

시각적으로 `Stepper` 레이블이 지정 된 두 개의 단추 이루어져 **&ndash;** 하 고 **+** 합니다. 키를 눌러 **&ndash;** 감소 `Value` 의해 `Increment` 를 최소 값인 `Minimum`합니다. 키를 눌러 **+** 증가 `Value` 하 여 `Increment` 최대에 `Maximum`입니다.

이 확인할 합니다 [ **StepperDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) 샘플입니다.

## <a name="switch-and-checkbox"></a>스위치 및 확인란

합니다 [ `Switch` ](xref:Xamarin.Forms.Switch) 부울 값을 지정할 수 있습니다.

### <a name="switch-basics"></a>기본 전환

시각적으로 `Switch` 켜고 설정 수 있는 토글을 이루어져 있습니다. 클래스는 하나의 속성을 정의합니다.

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) 형식의 `bool`

`Switch` 하나의 이벤트를 정의합니다.

- [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) 동반을 [ `ToggledEventArgs` ](xref:Xamarin.Forms.ToggledEventArgs) 개체, 경우에 발생 합니다 `IsToggled` 속성 변경.

합니다 [ **SwitchDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) 프로그램을 보여 줍니다는 `Switch`합니다.

### <a name="a-traditional-checkbox"></a>기존의 확인란

일부 개발자는 보다 일반적인 선호할 `CheckBox` 에 `Switch`합니다. 합니다 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 `CheckBox` 에서 파생 된 클래스 `ContentView`합니다. `CheckBox` 에 의해 구현 되는 [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) 하 고 [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) 파일. `CheckBox` 세 가지 속성을 정의 합니다 (`Text`, `FontSize`, 및 `IsChecked`) 및 `CheckedChanged` 이벤트입니다.

합니다 [ **CheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) 예제는이 `CheckBox`합니다.

## <a name="typing-text"></a>텍스트를 입력합니다.

Xamarin.Forms는 사용자가 입력 하 고 텍스트를 편집할 수 있는 세 가지 뷰를 정의 합니다.

- [`Entry`](xref:Xamarin.Forms.Entry) 한 줄 텍스트
- [`Editor`](xref:Xamarin.Forms.Editor) 여러 줄의 텍스트에 대 한
- [`SearchBar`](xref:Xamarin.Forms.SearchBar) 검색을 위해 텍스트 한 줄로 합니다.

`Entry` 및 `Editor` 에서 파생 [ `InputView` ](xref:Xamarin.Forms.InputView)에서 파생 되는 `View`합니다. `SearchBar` 직접 파생 `View`합니다.

### <a name="keyboard-and-focus"></a>키보드 및 포커스

휴대폰 및 태블릿에서 없이 실제 키보드를 `Entry`, `Editor`, 및 `SearchBar` 모든 요소를 팝업 하 가상 키보드를 발생 합니다. 화면에이 키보드의 현재 상태는 입력 포커스가 관련이 있습니다. 뷰를 모두가지고 있어야 해당 [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) 하 고 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) 속성으로 설정 `true` 입력된 포커스를 가져오려고 합니다.

입력된 포커스가 있는 두 가지 방법, 읽기 전용 속성 및 두 개의 이벤트가 포함 됩니다. 이 모든 정의한 `VisualElement`:

- 합니다 [ `Focus` ](xref:Xamarin.Forms.VisualElement.Focus) 메서드는 요소에 입력된 포커스를 설정 하려고 시도 하 고 반환 `true` 성공 하는 경우
- 합니다 [ `Unfocus` ](xref:Xamarin.Forms.VisualElement.Unfocus) 메서드는 요소에서 입력된 포커스를 제거 합니다.
- 합니다 [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) 요소에 입력 포커스가 있으면 나타내는 읽기 전용 속성
- 합니다 [ `Focused` ](xref:Xamarin.Forms.VisualElement.Focused) 이벤트 때 요소가 포커스 입력 했음을 나타냅니다.
- 합니다 [ `Unfocused` ](xref:Xamarin.Forms.VisualElement.Unfocused) 이벤트 나타냅니다 요소가 입력된 포커스를 잃을 때

### <a name="choosing-the-keyboard"></a>키보드를 선택합니다.

[ `InputView` ](xref:Xamarin.Forms.InputView) 클래스 `Entry` 및 `Editor` 파생 하나의 속성을 정의 합니다.

- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) 형식의 [`Keyboard`](xref:Xamarin.Forms.Keyboard)

표시 되는 키보드의 종류를 나타냅니다. 일부 키보드 Uri 또는 숫자 최적화 됩니다.

합니다 `Keyboard` 클래스에 정적 키보드를 정의할 수 있습니다 [ `Keyboard.Create` ](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags)) 형식의 인수를 사용 하 여 메서드 [ `KeyboardFlags` ](xref:Xamarin.Forms.KeyboardFlags), 다음 비트 플래그를 사용 하 여 열거형:

- `None` 0으로 설정
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) 1로 설정
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) 2로 설정
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) 4로 설정
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) \xFFFFFFFF로 설정

여러 줄을 사용 하는 경우 [ `Editor` ](xref:Xamarin.Forms.Editor) 단락 또는 두 텍스트 개 예상 되는 경우 호출 `Keyboard.Create` 은 키보드를 선택 하는 것이 좋습니다. 한 줄에 대 한 [ `Entry` ](xref:Xamarin.Forms.Entry), 다음 읽기 전용의 정적 속성 `Keyboard` 유용 합니다.

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) 소수점 유무에 양수입니다.

합니다 [ `KeyboardTypeConverter` ](xref:Xamarin.Forms.KeyboardTypeConverter) XAML 나타난 것 처럼 이러한 속성을 지정할 수 있습니다 합니다 [ **EntryKeyboards** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) 프로그램입니다.

### <a name="entry-properties-and-events"></a>항목 속성 및 이벤트

한 줄 [ `Entry` ](xref:Xamarin.Forms.Entry) 다음 속성을 정의 합니다.

- [`Text`](xref:Xamarin.Forms.Entry.Text) 형식의 `string`에 나타나는 텍스트는 `Entry`
- [`TextColor`](xref:Xamarin.Forms.Entry.TextColor) 형식의 `Color`
- [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily) 형식의 `string`
- [`FontSize`](xref:Xamarin.Forms.Entry.FontSize) 형식의 `double`
- [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes) 형식의 `FontAttributes`
- [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) 형식의 `bool`를 마스킹 하도록 문자 하면
- [`Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) 형식의 `string`에 표시 되는 흐린 색이 지정 된 텍스트는 `Entry` 전에 아무 것도 입력
- [`PlaceholderColor`](xref:Xamarin.Forms.Entry.PlaceholderColor) 형식의 `Color`

`Entry` 도 두 개의 이벤트를 정의 합니다.

- [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) 사용 하 여는 [ `TextChangedEventArgs` ](xref:Xamarin.Forms.TextChangedEventArgs) 될 때마다 발생 하는 개체는 `Text` 속성 변경
- [`Completed`](xref:Xamarin.Forms.Entry.Completed)를 사용자가 완료 하 고 키보드 해제 될 때 발생 합니다. 사용자는 플랫폼 특정 방식으로 완료를 나타냅니다.

합니다 [ **QuadraticEquations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) 샘플에서는 이러한 두 이벤트를 보여 줍니다.

### <a name="the-editor-difference"></a>편집기 차이

여러 줄 [ `Editor` ](xref:Xamarin.Forms.Editor) 동일한 정의 `Text` 하 고 `Font` 속성으로 `Entry` 하지만 다른 속성이 아니라 합니다. `Editor` 또한 같은 두 속성을 정의 `Entry`합니다.

[**JustNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) 는 저장 하 고 내용을 복원 하는 자유 형식 정보 기록 프로그램은 `Editor`합니다.

### <a name="the-searchbar"></a>SearchBar

합니다 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 에서 파생 되지 않습니다 `InputView`이므로 없기는 `Keyboard` 속성입니다. 하지만 모든 것은 `Text`, `Font`, 및 `Placeholder` 속성은 `Entry` 정의 합니다. 또한 `SearchBar` 세 가지 추가 속성을 정의 합니다.

- [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) 형식의 `Color`
- [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) 형식의 [ `ICommand` ](xref:System.Windows.Input.ICommand) 사용 데이터 바인딩 및 MVVM
- [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) 형식의 `Object`, 사용 `SearchCommand`

플랫폼 특정 텍스트 지우기 단추를 취소 합니다. `SearchBar` 플랫폼별 검색 단추도 있습니다. 두 이벤트 중 하나를 발생 시킵니다 해당 단추를 눌러는 `SearchBar` 정의:

- [`TextChanged`](xref:Xamarin.Forms.SearchBar.TextChanged) 와 함께 제공 된 [ `TextChangedEventArgs` ](xref:Xamarin.Forms.TextChangedEventArgs) 개체
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

합니다 [ **SearchBarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) 샘플과 `SearchBar`합니다.

## <a name="date-and-time-selection"></a>날짜 및 시간 선택

합니다 [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) 하 고 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) 뷰 사용자 지정 날짜 또는 시간을 허용 하는 플랫폼 특정 컨트롤을 구현 합니다.

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](xref:Xamarin.Forms.DatePicker) 네 가지 속성을 정의합니다.

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) 형식의 `DateTime`1900 년 1 월 1 일에 초기화
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) 형식의 `DateTime`2100 년 12 월 31, 초기화
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) 형식의 `DateTime`으로 초기화 `DateTime.Today`
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) 형식의 `string`, "7/20/1969" 미국의 같은 날짜 표시 결과.NET 형식 문자열 "d", 간단한 날짜 패턴으로 초기화 합니다.

설정할 수 있습니다는 `DateTime` 속성 요소 속성을 표현 하 고 고정 문화권 간단한 날짜를 사용 하 여 XAML에서 속성 형식 ("7/20/1969").   

합니다 [ **DaysBetweenDates** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) 샘플 사용자가 선택한 두 날짜 사이의 일 수를 계산 합니다.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (또는 TimeSpanPicker 것)

[`TimePicker`](xref:Xamarin.Forms.TimePicker) 두 속성 및 이벤트를 정의합니다.

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) 유형의 `TimeSpan` 대신 `DateTime`, 자정 이후 경과 된 시간을 나타내는
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) 형식의 `string`,.NET 미국의 "t", "1:45 PM"와 같은 시간 표시를 생성 하는 짧은 시간 패턴으로 초기화 하는 문자열의 형식을 지정 합니다.

[ **SetTimer** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) 프로그램을 사용 하는 방법에 설명 합니다 `TimePicker` 타이머에 대 한 시간을 지정 하 합니다. 프로그램을 전경에 유지 하는 경우에 작동 합니다.

**SetTimer** 를 사용 하 여 보여 줍니다는 [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) 메서드의 `Page` 경고 상자를 표시 합니다.

## <a name="related-links"></a>관련 링크

- [15 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [15 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [슬라이더](~/xamarin-forms/user-interface/slider.md)
- [항목](~/xamarin-forms/user-interface/text/entry.md)
- [편집기](~/xamarin-forms/user-interface/text/editor.md)
- [DatePicker](~/xamarin-forms/user-interface/datepicker.md)
