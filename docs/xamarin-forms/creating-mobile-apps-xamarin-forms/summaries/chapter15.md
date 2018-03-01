---
title: "15 장의 요약입니다. 대화형 인터페이스"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: e6c61f9a6ba66db2b9a5c7b217c7da952607e709
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>15 장의 요약입니다. 대화형 인터페이스

이 장에서 설명 8 `View` 사용자와 상호 작용을 허용 하는 파생 항목입니다.

## <a name="view-overview"></a>개요 보기

파생 되는 20 인스턴스화할 수 있는 클래스를 포함 하는 Xamarin.Forms `View` 아닌 `Layout`합니다. 이 중 6 개 이전 장에서 설명 되었을:

- `Label`: [ **Chapter 2입니다. 응용 프로그램 분석**](chapter02.md)
- `BoxView`: [ **Chapter 3입니다. 스택 스크롤**](chapter03.md)
- `Button`: [ **6 장. 단추 클릭**](chapter06.md)
- `Image`: [ **13 장 합니다. Bitmaps**](chapter13.md)
- `ActivityIndicator`: [ **13 장 합니다. Bitmaps**](chapter13.md)
- `ProgressBar`: [ **장 14입니다. AbsoluteLayout**](chapter14.md)

이 장에서 8 개의 보기 기본.NET 데이터 형식을와 상호 작용 하는 사용자를 효과적으로 허용:

<table>
  <tr>
    <th>데이터 형식</th>
    <th>보기</th>
  </tr>
  <tr>
    <td>`Double`</td>
    <td
      <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">슬라이더</a></code>,
      <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Stepper</a></code>
    </td>
  </tr>
  <tr>
    <td>`Boolean`</td>
    <td>
      <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">Switch</a></code>
    </td>
  </tr>
  <tr>
    <td>`String`</td>
    <td>
      <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a></code>, <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Editor</a></code>, <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a></code>
    </td>
  </tr>
  <tr>
    <td>`DateTime`</td>
    <td>
      <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a></code>, <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a></code>
    </td>
  </tr>
</table>

이러한 뷰는 기본 데이터 형식은의 대화형 시각적 표시로 생각할 수 있습니다. 이 개념은 다음 장에서 더 대해서는 [ **장 16입니다. 데이터 바인딩**](chapter16.md)합니다.

나머지 6 개의 뷰는 다음 단원에 포함 됩니다.

- `WebView`: [ **장 16입니다. 데이터 바인딩**](chapter16.md)
- `Picker`: [ **19 장 합니다. 컬렉션 뷰**](chapter19.md)
- `ListView`: [ **19 장 합니다. 컬렉션 뷰**](chapter19.md)
- `TableView`: [ **19 장 합니다. 컬렉션 뷰**](chapter19.md)
- `Map`: [ **장 28입니다. 위치 및 지도**](chapter28.md)
- `OpenGLView`:이 가이드 (및 Windows 플랫폼에 대 한 지원 되지 않습니다)에서 다루는 not

## <a name="slider-and-stepper"></a>슬라이더와 스텝 퍼

둘 다 [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) 및 [ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) 사용자 범위에서 숫자 값을 선택할 수 있게 합니다. `Slider` 동안 연속 범위는 `Stepper` 불연속 값을 포함 합니다.

### <a name="slider-basics"></a>슬라이더 기본 사항

[ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) 가로줄로 나눠진 왼쪽에는 최소에서 오른쪽에 대 한 최대값으로 값 범위를 나타내는 됩니다. 세 가지 공용 속성을 정의합니다.

- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) 형식의 `double`, 기본 0 값
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) 형식의 `double`, 기본 0 값
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) 형식의 `double`, 1의 기본값

이러한 속성을 지 원하는 바인딩 가능 속성 일치 하는지 확인 합니다.

- 모든 세 가지 속성에는 [ `coerceValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) 되도록 바인딩할 수 있는 속성에 대해 지정 된 메서드 `Value` 사이의 `Minimum` 및 `Maximum`합니다.
- [ `validateValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+ValidateValueDelegate/) 메서드를 `MinimumProperty` 반환 `false` 경우 `Minimum` 보다 크거나 같은 값으로 설정 되어 `Maximum`, 유사 `MaximumProperty`합니다. 반환 `false` 에서 `validateValue` 메서드를 사용 하면은 `ArgumentException` 발생 합니다.

`Slider` 발생은 [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) 인 이벤트는 [ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) 인수 때는 `Value` 속성 변경 내용을 프로그래밍 방식으로 또는 때 사용자 조작는 `Slider`합니다.

[ **SliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) 샘플의 간단한 사용법을 보여줍니다는 `Slider`합니다.

### <a name="common-pitfalls"></a>일반적인 문제

코드와 XAML에는 `Minimum` 및 `Maximum` 속성 지정 하는 순서로 설정 합니다. 이러한 속성을 초기화 하십시오 있도록 `Maximum` 보다 항상 큽니다 `Minimum`합니다. 그렇지 않으면 예외가 발생합니다.

초기화는 `Slider` 속성으로 지정 하면는 `Value` 속성을 변경 및 `ValueChanged` 이벤트를 발생 합니다. 인지 확인 해야는 `Slider` 이벤트 처리기를 페이지 초기화 하는 동안 만든 아직 하지 않은 보기에 액세스 하지 않습니다.

`ValueChanged` 하는 동안 이벤트가 발생 하지 않습니다 `Slider` 초기화 하지 않는 한는 `Value` 속성 변경 합니다. 호출할 수 있습니다는 `ValueChanged` 코드에서 직접 처리기입니다.

### <a name="slider-color-selection"></a>슬라이더 색 선택

[ **RgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) 프로그램에 세 개의 `Slider` 을 대화형으로 색의 RGB 값을 지정 하 여 선택할 수 있는 요소:

[![R G B 슬라이더의 삼중 스크린 샷](images/ch15fg03-small.png "RGB 슬라이더")](images/ch15fg03-large.png "RGB 슬라이더")

[ **TextFade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) 샘플에서는 두 개의 `Slider` 요소를 이동 하는 두 개의 `Label` 에서 요소는 `AbsoluteLayout` 하나에 다른 페이드 및 합니다.

### <a name="the-stepper-difference"></a>스텝 퍼 차이

[ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) 같은 속성 및 이벤트로 정의 `Slider` 있지만 `Maximum` 100 속성은 초기화 및 `Stepper` 네 번째 속성을 정의:

- [`Increment`](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) 형식의 `double`1로 초기화 된,

시각적으로 `Stepper` 레이블이 지정 된 두 개의 단추 이루어져 **& #x 2013;** 및  **+** 합니다. 키를 누르면 **& #x 2013;** 감소 `Value` 여 `Increment` 최소 `Minimum`합니다. 키를 누르면  **+**  증가 `Value` 여 `Increment` 최대 `Maximum`합니다.

이 확인할는 [ **StepperDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) 샘플.

## <a name="switch-and-checkbox"></a>스위치 및 확인란

[ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) 부울 값을 지정할 수 있습니다.

### <a name="switch-basics"></a>스위치 기본 사항

시각적으로 `Switch` 오프 했다가 다시 로그온 설정 수 있는 토글 구성 됩니다. 클래스는 하나의 속성을 정의합니다.

- [`IsToggled`](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) 형식의 `bool`

`Switch` 하나의 이벤트를 정의합니다.

- [`Toggled`](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) 와 함께 사용 된 [ `ToggledEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToggledEventArgs/) 때 발생 합니다. 개체는 `IsToggled` 속성 변경 합니다.

[ **SwitchDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) 프로그램을 보여 줍니다.는 `Switch`합니다.

### <a name="a-traditional-checkbox"></a>기존의 확인란

일부 개발자 들이 더 많은 기존 수도 `CheckBox` 에 `Switch`합니다. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에 포함 되어는 `CheckBox` 에서 파생 된 클래스 `ContentView`합니다. `CheckBox` 에 의해 구현 된 [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) 및 [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) 파일입니다. `CheckBox` 세 가지 속성을 정의 합니다 (`Text`, `FontSize`, 및 `IsChecked`) 및 `CheckedChanged` 이벤트입니다.

[ **CheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) 예제는이 `CheckBox`합니다.

## <a name="typing-text"></a>텍스트를 입력합니다.

Xamarin.Forms는 사용자가 입력 하 고 텍스트를 편집할 수 있는 세 가지 뷰를 정의 합니다.

- [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 한 줄 텍스트에 대 한
- [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) 여러 줄의 텍스트에 대 한
- [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) 검색을 위해 텍스트 한 줄로 합니다.

`Entry` 및 `Editor` 에서 파생 [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/)에서 파생 되는 `View`합니다. `SearchBar` 직접 파생 `View`합니다.

### <a name="keyboard-and-focus"></a>키보드 및 포커스

전화 및 태블릿 물리적 키보드 없이는 `Entry`, `Editor`, 및 `SearchBar` 모든 요소를 팝업 하 가상 바로 발생 합니다. 화면에이 키보드의 존재는 입력 포커스가 관련이 있습니다. 뷰 모두 있어야 해당 [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) 및 [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) 속성으로 설정 `true` 입력된 포커스를 얻으려고 합니다.

입력된 포커스가 있는 두 가지 방법, 읽기 전용 속성 및 두 개의 이벤트가 참여 합니다. 이러한 작업은 모두 정의 하 여 `VisualElement`:

- [ `Focus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Focus()/) 메서드는 요소에 입력된 포커스를 설정 하려고 시도 하 고 반환 `true` 성공 하는 경우
- [ `Unfocus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Unfocus()/) 메서드 요소에서 입력된 포커스를 제거 합니다.
- [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) 읽기 전용 속성 요소에 입력 포커스가 경우를 나타냅니다.
- [ `Focused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Focused/) 이벤트 요소 입력된 포커스를 가져오는지 하는 시기를 나타냅니다
- [ `Unfocused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Unfocused/) 이벤트 요소에서 입력된 포커스를 잃을 때 나타냅니다.

### <a name="choosing-the-keyboard"></a>키보드를 선택합니다.

[ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) 클래스를 `Entry` 및 `Editor` 파생 하나의 속성을 정의 합니다.

- [`Keyboard`](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) 형식의 [`Keyboard`](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/)

표시 되는 키보드의 종류를 나타냅니다. 일부 키보드 Uri 또는 숫자 최적화 됩니다.

`Keyboard` 클래스에 정적 키보드를 정의할 수 있습니다. [ `Keyboard.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Keyboard.Create/p/Xamarin.Forms.KeyboardFlags/) 형식의 인수를 사용 하 여 메서드 [ `KeyboardFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardFlags/), 다음 비트 플래그를 사용 하 여 열거형:

- `None` 0으로 설정
- [`CapitalizeSentence`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.CapitalizeSentence/) 1로 설정
- [`Spellcheck`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Spellcheck/) 2로 설정
- [`Suggestions`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Suggestions/) 4로 설정
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.All/) \xFFFFFFFF로 설정

여러 줄을 사용 하는 경우 [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) 단락 또는 그 이상의 텍스트 예상 되는 경우 호출 `Keyboard.Create` 키보드를 선택할 수 있는 좋은 방법입니다. 한 줄에 대 한 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), 다음 읽기 전용의 정적 속성 `Keyboard` 유용 합니다.

- [`Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Default/)
- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Text/)
- [`Chat`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Chat/)
- [`Url`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Url/)
- [`Email`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Email/)
- [`Telephone`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Telephone/)
- [`Numeric`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Numeric/) 소수점 유무에 양수입니다.

[ `KeyboardTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardTypeConverter/) 에 나타난 것 처럼 XAML에서 이러한 속성을 지정할 수는 [ **EntryKeyboards** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) 프로그램.

### <a name="entry-properties-and-events"></a>항목 속성 및 이벤트

한 줄 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 다음 속성을 정의 합니다.

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) 형식의 `string`에 나타나는 텍스트는 `Entry`
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) 형식의 `Color`
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontFamily/) 형식의 `string`
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontSize/) 형식의 `double`
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontAttributes/) 형식의 `FontAttributes`
- [`IsPassword`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.IsPassword/) 형식의 `bool`을 마스킹할 문자에 이르게
- [`Placeholder`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) 형식의 `string`, 흐린 색이 지정 된 텍스트에 표시 되는 `Entry` 아무 것도 입력 하기 전에
- [`PlaceholderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.PlaceholderColor/) 형식의 `Color`

`Entry` 도 두 개의 이벤트를 정의 합니다.

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) 와 [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) 될 때마다 발생 합니다. 개체는 `Text` 속성 변경
- [`Completed`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/)키보드 해제 되 고 사용자가 완료 될 때 발생 합니다. 사용자는 플랫폼 특정 방식으로 완료를 나타냅니다.

[ **QuadraticEquations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) 샘플에서는 이러한 두 이벤트를 보여 줍니다.

### <a name="the-editor-difference"></a>편집기 차이

여러 줄 [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) 동일한 정의 `Text` 및 `Font` 속성으로 `Entry` 하지만 다른 속성이 아니라 합니다. `Editor` 또한 동일한 두 개의 속성을 정의 `Entry`합니다.

[**JustNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) 저장 하 고의 내용을 복원 하는 자유 형식 노트 기록 프로그램는 `Editor`합니다.

### <a name="the-searchbar"></a>SearchBar

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) 에서 파생 되지 않습니다 `InputView`없기 때문에 `Keyboard` 속성입니다. 하지만 모든 것은 `Text`, `Font`, 및 `Placeholder` 속성은 `Entry` 를 정의 합니다. 또한 `SearchBar` 세 개의 추가 속성을 정의 합니다.

- [`CancelButtonColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.CancelButtonColor/) 형식의 `Color`
- [`SearchCommand`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) 형식의 [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) 사용 된 데이터 바인딩 및 MVVM
- [`SearchCommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) 형식의 `Object`와 함께 사용할 `SearchCommand`

플랫폼 특정 텍스트 단추 지우기로 적용을 취소 합니다. `SearchBar` 에 플랫폼 관련 검색 단추도 있습니다. 두 이벤트 중 하나를 이러한 단추 중 하나를 누르면 발생 하는 `SearchBar` 정의:

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.TextChanged/) 와 함께 사용 된 [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) 개체
- [`SearchButtonPressed`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/)

[ **SearchBarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) 샘플는 `SearchBar`합니다.

## <a name="date-and-time-selection"></a>날짜 및 시간 선택

[ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) 및 [ `TimePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) 뷰 사용자 지정 날짜 또는 시간을 허용 하는 플랫폼 특정 컨트롤을 구현 합니다.

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) 네 가지 속성을 정의합니다.

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) 형식의 `DateTime`1900 년 1 월 1 일 초기화
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) 형식의 `DateTime`2100 년 12 월 31, 초기화
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) 형식의 `DateTime`초기화 `DateTime.Today`
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) 형식의 `string`, "d", 간단한 날짜 패턴으로 초기화 하는 문자열의 서식을 지정 하는.NET "7/20/1969"은 미국 영어에서와 같은 날짜 표시를 생성 합니다.

설정할 수 있습니다는 `DateTime` 속성 요소로 속성을 표현 하 고 고정 문화권 간단한 날짜를 사용 하 여 XAML에서 속성 형식 ("7/20/1969").   

[ **DaysBetweenDates** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) 샘플 사용자가 선택한 두 날짜 사이의 일 수를 계산 합니다.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker 이동해 왔거나는 TimeSpanPicker 입니까?

[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) 두 개의 속성 및 없는 이벤트를 정의합니다.

- [`Time`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) 형식의 `TimeSpan` 대신 `DateTime`, 자정 이후 경과 된 시간을 나타내는
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Format/) 형식의 `string`,.NET 미국에서 "t", "오후 1시 45분"와 같은 시간 표시에는 짧은 시간 패턴으로 초기화 하는 문자열의 서식을 지정 합니다.

[ **SetTimer** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) 프로그램을 사용 하는 방법을 보여 줍니다는 `TimePicker` 타이머에 대 한 시간을 지정할 수 있습니다. 프로그램이 전경에 유지 하는 경우에 작동 합니다.

**SetTimer** 를 사용 하 여 보여 줍니다는 [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) 방식의 `Page` 경고 대화 상자를 표시 합니다.



## <a name="related-links"></a>관련 링크

- [15 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [15 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Entry](~/xamarin-forms/user-interface/text/entry.md)
- [편집기](~/xamarin-forms/user-interface/text/editor.md)
