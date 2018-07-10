---
title: 요약 16 장입니다. 데이터 바인딩
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 16 장 요약 합니다. 데이터 바인딩'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 92cf7f0163c4f074c718e86b06cf4830ff857c58
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935604"
---
# <a name="summary-of-chapter-16-data-binding"></a>요약 16 장입니다. 데이터 바인딩

프로그래머가 자주 한 개체의 속성을 변경 될 때 검색 하는 이벤트 처리기를 작성 하 고 다른 개체의 속성 값을 변경 하는 데는 있습니다. 기술을 사용 하 여이 프로세스를 자동화할 수 있습니다 *증상*합니다. 데이터 바인딩은 대개 XAML에서 정의 및 사용자 인터페이스 정의의 일부가 됩니다.

많은 경우 이러한 데이터 바인딩은 기본 데이터에 사용자 인터페이스 개체를 연결합니다. 이 기술에 살펴봅니다 더입니다 [ **18 장입니다. MVVM**](chapter18.md)합니다. 그러나 데이터 바인딩을 두 개 이상의 사용자 인터페이스 요소를 연결할 수도 있습니다. 이 챕터의 데이터 바인딩에 대 한 초기 예의 대부분에는이 기술을 보여 줍니다.

## <a name="binding-basics"></a>바인딩 기본 사항

여러 속성, 메서드 및 클래스를 데이터 바인딩에 관련 됩니다.

- 합니다 [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) 클래스에서 파생 됩니다 [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) 다양 한 데이터 바인딩 특성을 캡슐화 하 고
- 합니다 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 속성에서 정의 되는 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) 클래스
- 합니다 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) 메서드는 또한 정의한 합니다 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) 클래스
- 합니다 [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) 클래스를 정의 추가 3 `SetBinding` 메서드

다음 두 클래스는 바인딩에 대 한 XAML 태그 확장을 지원합니다.

- [`BindingExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) 지원 된 `Binding` 태그 확장
- [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) 지원 된 `x:Reference` 태그 확장

두 가지 인터페이스는 데이터 바인딩에서 관련 됩니다.

- [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) 에 `System.ComponentModel` 때 속성 변경 알림 구현에 대 한 네임 스페이스는
- [`IValueConverter`](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) 다른 데이터 바인딩 값의 형식을 변환 하는 작은 클래스를 정의 하는 데 사용 됩니다.

데이터 바인딩을 동일한 개체 또는 두 개의 다른 개체 (자주)의 두 가지 속성을 연결합니다. 이러한 두 속성으로 참조 됩니다 합니다 *소스* 하며 *대상*합니다. 일반적으로 소스 속성의 변경으로 인해 변경 대상 속성에서 발생 하지만 방향을 반대로 경우도 있습니다. 관계 없이:

- 합니다 *대상* 속성으로 [`BindableProperty`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- 합니다 *원본* 속성은 일반적으로 구현 하는 클래스의 멤버 [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/)

구현 하는 클래스 `INotifyPropertyChanged` 발생을 [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/) 속성 값을 변경할 때 이벤트입니다. `BindableObject` 구현 `INotifyPropertyChanged` 자동으로 발생 하 고는 `PropertyChanged` 속성에서 지 원하는 경우 이벤트를 `BindableProperty` 수 있지만 변경 값을 작성할 수 고유한 클래스 구현 하는 `INotifyPropertyChanged` 에서 파생 하지 않고 `BindableObject`합니다.

## <a name="code-and-xaml"></a>코드 및 XAML

합니다 [ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) 샘플 코드에서 데이터 바인딩을 설정 하는 방법에 설명 합니다.

- 원본이 `Value` 의 속성을 `Slider`
- 대상이 `Opacity` 의 속성을 `Label`

설정 하 여 두 개체가 연결 되어는 `BindingContext` 의 `Label` 개체를 `Slider` 개체입니다. 두 속성을 호출 하 여 연결 된를 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) 확장 메서드를를 `Label` 참조를 `OpacityProperty` 바인딩 가능 속성 및 `Value` 속성은 `Slider` 나타낸를 문자열입니다.

조작 합니다 `Slider` 가 `Label` 보기 내부 및 외부 페이드입니다.

합니다 [ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) 는 XAML에서 설정 하는 데이터 바인딩 사용 하 여 동일한 프로그램입니다. `BindingContext` 의 `Label` 로 설정 됩니다는 `x:Reference` 태그 확장 참조를 `Slider`, 및 `Opacity` 속성을 `Label` 로 설정 되어를 `Binding` 태그 확장과 해당 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) 속성을 참조 하는 `Value` 의 속성을 `Slider`.

## <a name="source-and-bindingcontext"></a>원본 및 BindingContext

합니다 [ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) 예제 코드의 대 안으로 보여 줍니다. `Binding` 개체를 설정 하 여 만들 합니다 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) 속성을를 `Slider` 개체 및 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) 속성을 "Value"입니다. 합니다 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) 메서드의 `BindableObject` 에서 호출 됩니다는 `Label` 개체입니다.

합니다 [ `Binding` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) 도 사용 되었을 수를 정의 하는 `Binding` 개체입니다.

합니다 [ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) 예제 XAML의 비슷한 기법을 보여 줍니다. `Opacity` 의 속성을 `Label` 로 설정 되어를 `Binding` 태그 확장과 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) 로 `Value` 속성 및 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) 로 embedded `x:Reference` 태그 확장 합니다.

요약 하자면, 바인딩 소스 개체를 참조 하는 방법은 두 가지 있습니다.

- 통해 여 `BindingContext` 대상의 속성
- 통해 합니다 `Source` 의 속성을 `Binding` 개체 자체

둘 다 지정 하는 경우 두 번째 우선 합니다. 이점은 `BindingContext` 는 시각적 트리를 통해 전파 됩니다. 이것이 *매우* 여러 대상 속성 같은 원본 개체에 바인딩된 경우에 유용 합니다.

합니다 [ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) 프로그램에는이 기술을 사용 하 여 보여 줍니다 합니다 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) 요소입니다. 두 `Button` 탐색 하기 위한 요소 앞 이나 뒤로 상속을 `BindingContext` 를 참조 하는 부모 로부터 `WebView`합니다. 합니다 `IsEnabled` 두 단추의 속성을 갖게 간단한 `Binding` 단추를 대상으로 하는 태그 확장 `IsEnabled` 속성의 설정에 따라 합니다 [ `CanGoBack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoBack/) 및 [ `CanGoForward` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoForward/) 의 읽기 전용 속성을 `WebView`입니다.

## <a name="the-binding-mode"></a>바인딩 모드

설정 된 [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) 속성을 `Binding` 의 멤버에는 [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) 열거형:

- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) source 속성에 변경 내용이 대상에 영향을 줄합니다
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) 대상 속성의 변경 내용이 원본에 영향을 줄합니다
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) 원본 및 대상에 변경 내용을 서로 영향을 줄 수 있도록
- [`Default`](xref:Xamarin.Forms.BindingMode.Default) 사용 하는 [ `DefaultBindingMode` ](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) 시기를 지정 된 대상 `BindableProperty` 만들어진 합니다. 에 지정 된 경우 기본값은 `OneWay` 일반적인 바인딩 가능한 속성에 대 한 및 `OneWayToSource` 읽기 전용으로 바인딩할 수 있는 속성에 대 한 합니다.

일반적으로 MVVM 시나리오의 데이터 바인딩에의 대상이 될 가능성이 있는 속성을 `DefaultBindingMode` 의 `TwoWay`합니다. 이러한 항목은 다음과 같습니다.

- `Value` 속성의 `Slider` 및 `Stepper`
- `IsToggled` 속성 `Switch`
- `Text` 속성을 `Entry`, `Editor`, 및 `SearchBar`
- `Date` 속성 `DatePicker`
- `Time` 속성 `TimePicker`

[ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) 샘플 대상 인 데이터 바인딩을 사용 하 여 네 가지 바인딩 모드를 보여 줍니다.는 `FontSize` 속성을 `Label` 원본이 및는 `Value` 속성을 `Slider`입니다. 이렇게 하면 각 `Slider` 해당 글꼴 크기를 제어 하려면 `Label`합니다. 하지만 `Slider` 요소 때문에 초기화 되지 합니다 `DefaultBindingMode` 의 합니다 `FontSize` 속성이 `OneWay`합니다.

[ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) 에 바인딩을 설정 하는 샘플을 `Value` 의 속성을 `Slider` 참조를 `FontSize` 각 속성 `Label`. 이 이전 버전과 호환성 보이지만 초기화에서 잘 작동 하는 `Slider` 요소 때문에 `Value` 의 속성을 `Slider` 에 `DefaultBindingMode` 의 `TwoWay`.

[![바인딩 역방향 삼중 스크린 샷](images/ch16fg06-small.png "바인딩 역방향")](images/ch16fg06-large.png#lightbox "역방향 바인딩")

MVVM의 바인딩을 정의 하는 방법을 비슷합니다 이며 이러한 유형의 바인딩은 자주 사용 합니다.

## <a name="string-formatting"></a>문자열 서식 지정

형식의 대상 속성의 경우 `string`를 사용할 수는 [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) 정의한 속성 `BindingBase` 소스를 변환 하는 `string`. 설정 된 `StringFormat` 속성을 정적을 사용 하 여 사용할 수 있는 문자열의 형식을 지정 하는.NET [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) 개체를 표시 하는 형식입니다. 태그 확장 내에서이 서식 문자열을 사용 하 여, 묶으세요 작은따옴표 있도록 중괄호 안에 포함 된 태그 확장에 대 한 잘못 된 수 없습니다.

합니다 [ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) 샘플을 사용 하는 방법에 설명 `StringFormat` XAML에서.

[ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) 샘플에 대 한 바인딩을 사용 하 여 페이지의 크기를 표시 하는 방법을 보여 줍니다 합니다 `Width` 및 `Height` 의 속성을 `ContentPage`합니다.

## <a name="why-is-it-called-path"></a>"Path" 이유 호출 것?

합니다 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) 속성의 `Binding` 일련의 속성 및 인덱서 마침표로 구분 될 수 있으므로 따라서 라고 합니다. 합니다 [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) 샘플에서는 몇 가지 예를 보여 줍니다.

## <a name="binding-value-converters"></a>바인딩 값 변환기

바인딩 소스 및 대상 속성을 다른 형식 경우 바인딩 변환기를 사용 하 여 형식 간에 변환할 수 있습니다. 이 클래스는 구현 하는 클래스를 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) 인터페이스 및 두 개의 메서드가: [ `Convert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.Convert/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) 대상에 원본을 변환할 및 [ `ConvertBack` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.ConvertBack/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) 소스에 대상을 변환 합니다.

[ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리는 변환에 대 한 예제는 `int` 에 `bool`. 보여 주는 것을 [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) 만 사용 하도록 설정 하는 샘플을 `Button` 문자를 하나 이상에 입력 된 경우는 `Entry`.

[ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) 변환 클래스는 `bool` 에 `string` 에 대 한 텍스트를 반환할지를 지정 하는 두 속성을 정의 하 고 `false` 및 `true` 값입니다.
합니다 [ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) 비슷합니다. 합니다 [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) 샘플 이러한 두 가지 변환기를 사용 하 여 다른 텍스트 기반으로 하는 서로 다른 색으로 표시 하는 방법을 보여 줍니다는 `Switch` 설정 합니다.

제네릭 [ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) 바꿀 수는 `BoolToStringConverter` 및 `BoolToColorConverter` 일반화 된 역할도 및 `bool`-에-모든 형식의 개체 변환기입니다.

## <a name="bindings-and-custom-views"></a>바인딩 및 사용자 지정 보기

데이터 바인딩을 사용 하 여 사용자 지정 컨트롤을 간소화할 수 있습니다. [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) 코드 파일에 정의 `Text`를 `TextColor`를 `FontSize`, `FontAttributes`, 및 `IsChecked` 속성 않지만 컨트롤의 시각적 개체에 대 한 모든 논리가 없습니다.
대신 합니다 [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) 파일에서 데이터 바인딩을 통해 컨트롤의 시각적 개체에 대 한 모든 태그를 포함 합니다 `Label` 코드 숨김 파일에 정의 된 속성을 기반으로 하는 요소입니다.

합니다 [ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) 샘플과 `NewCheckBox` 사용자 지정 컨트롤입니다.



## <a name="related-links"></a>관련 링크

- [16 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [16 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
