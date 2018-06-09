---
title: 요약 장 16입니다. 데이터 바인딩
description: 'Xamarin.Forms를 사용 하 여 모바일 응용 프로그램 만들기: 장 16 요약 합니다. 데이터 바인딩'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 520da1518c7b795bd1ad17cc3cfaa8d37815de53
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241512"
---
# <a name="summary-of-chapter-16-data-binding"></a>요약 장 16입니다. 데이터 바인딩

프로그래머가 자주 때 한 개체의 속성이 변경 되었음을 감지 하는 이벤트 처리기를 작성 하 고는 사용 하 여 다른 개체에서 속성의 값을 변경할 수 있습니다. 이 프로세스를 자동화 하는 기술은와 *데이터 바인딩*합니다. 데이터 바인딩은 대개 XAML에서 정의 및 사용자 인터페이스의 정의의 일부가 됩니다.

대개 이러한 데이터 바인딩은 기본 데이터에 사용자 인터페이스 개체를 연결합니다. 이 더에 대해서는 기술 [ **18 장 합니다. MVVM**](chapter18.md)합니다. 그러나 데이터 바인딩을 두 개 이상의 사용자 인터페이스 요소를 연결할 수도 있습니다. 대부분의 데이터 바인딩이 장의 초기 예제는이 기술을 보여 줍니다.

## <a name="binding-basics"></a>바인딩 기본 사항

여러 속성, 메서드 및 클래스는 데이터 바인딩과에서 관련 된:

- [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) 클래스에서 파생 [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) 데이터 바인딩의 여러 특징을 캡슐화 하 고
- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 속성 정의한는 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) 클래스
- [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) 메서드에 의해 정의 됩니다는 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) 클래스
- [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) 클래스 정의 세 개의 추가 `SetBinding` 메서드

다음 두 클래스는 바인딩에 대 한 XAML 태그 확장을 지원합니다.

- [`BindingExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) 지원 된 `Binding` 태그 확장
- [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) 지원 된 `x:Reference` 태그 확장

두 개의 인터페이스 데이터 바인딩과에서 관련 된:

- [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) 에 `System.ComponentModel` 속성이 변경 될 때 알림을 구현 하기 위한 네임 스페이스는
- [`IValueConverter`](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) 데이터 바인딩에 다른 값의 형식을 변환 하는 작은 클래스를 정의 하는 데 사용

데이터 바인딩 동일한 개체 또는 두 개의 다른 개체 (일반적으로)의 두 속성을 연결합니다. 이러한 두 속성은 라고 하는 *소스* 및 *대상*합니다. 일반적으로 source 속성에서 변경 대상 속성에서 발생에 대 한 변경으로 인해 발생 하지만 방향을 반대로 경우도 있습니다. 이와 상관 없이:

- *대상* 속성으로 [`BindableProperty`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- *소스* 속성에는 일반적으로 구현 하는 클래스의 구성원은 [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/)

구현 하는 클래스 `INotifyPropertyChanged` 발생 한 [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/) 속성 값이 변경 될 때 이벤트입니다. `BindableObject` 구현 `INotifyPropertyChanged` 자동으로 발생 하 고는 `PropertyChanged` 속성에서 지 원하는 경우 이벤트는 `BindableProperty` 수 있지만 변경 값을 작성할 수 사용자 고유의 클래스 구현 하는 `INotifyPropertyChanged` 에서 파생 없이 `BindableObject`합니다.

## <a name="code-and-xaml"></a>코드 및 XAML

[ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) 샘플 코드에서 데이터 바인딩을 설정 하는 방법을 보여 줍니다.

- 원본이 `Value` 의 속성을 `Slider`
- 대상이 `Opacity` 의 속성을 `Label`

설정 하 여 연결 된 두 개체는 `BindingContext` 의 `Label` 개체를 `Slider` 개체입니다. 두 속성을 호출 하 여 연결 된는 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) 에 확장 메서드는 `Label` 참조 하는 `OpacityProperty` 바인딩 가능한 속성 및 `Value` 속성의는 `Slider` 으로 표시 되는 문자열입니다.

조작의 `Slider` 나면는 `Label` 보기 내부 및 외부 페이드됩니다.

[ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) 는 XAML에서 설정할 데이터 바인딩 사용 하 여 동일한 프로그램. `BindingContext` 의 `Label` 로 설정 되어는 `x:Reference` 태그 확장을 참조 하는 `Slider`, 및 `Opacity` 속성의는 `Label` 로 설정 되는 `Binding` 태그 확장으로 해당 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) 속성을 참조 하는 `Value` 의 속성은 `Slider`합니다.

## <a name="source-and-bindingcontext"></a>원본 및 BindingContext

[ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) 예제 코드의 다른 접근 방식은 보여 줍니다. A `Binding` 설정 하 여 개체가 생성 되는 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) 속성을는 `Slider` 개체 및 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) 속성을 "Value"입니다. [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) 방식의 `BindableObject` 그런 다음에 호출 됩니다는 `Label` 개체입니다.

[ `Binding` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) 도 사용 되었을 수를 정의 하는 `Binding` 개체입니다.

[ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) 샘플에서는 XAML 비교 가능한 기술을 보여 줍니다. `Opacity` 속성은 `Label` 로 설정 되는 `Binding` 태그 확장으로 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) 로 설정는 `Value` 속성 및 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) 로 설정는 포함 된 `x:Reference` 태그 확장 합니다.

요약 하자면, 바인딩 소스 개체를 참조 하는 방법은 두 가지 있습니다.

- 통해는 `BindingContext` 대상의 속성
- 통해는 `Source` 의 속성은 `Binding` 개체 자체

둘 다 지정 하는 경우 두 번째 우선 합니다. 이점은 `BindingContext` 시각적 트리를 통해 전파 됩니다. 이 *매우* 여러 대상 속성 동일한 원본 개체에 바인딩된 경우에 유용 합니다.

[ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) 프로그램이이 기술을 사용을 보여 줍니다.는 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) 요소입니다. 두 개의 `Button` 탐색을 위한 요소 앞 이나 뒤로 상속는 `BindingContext` 참조 하는 부모 로부터 `WebView`합니다. `IsEnabled` 두 단추의 속성에 다음 간단한는 `Binding` 단추를 대상으로 하는 태그 확장 `IsEnabled` 속성의 설정에 따라는 [ `CanGoBack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoBack/) 및 [ `CanGoForward` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoForward/) 의 읽기 전용 속성은 `WebView`합니다.

## <a name="the-binding-mode"></a>바인딩 모드

설정의 [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) 속성 `Binding` 의 멤버에는 [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) 열거형:

- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) source 속성에 대 한 변경 내용이 대상에 영향을
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) 대상 속성에 대 한 변경 내용이 원본에 영향을
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) 원본 및 대상에 변경 내용이 서로 영향을 줄 수 있도록
- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) 사용 하는 [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) 때 지정 된 대상 `BindableProperty` 만들었습니다. 기본값은 지정 하지 않으면 `OneWay` 기본 바인딩 가능 속성에 대 한 및 `OneWayToSource` 바인딩 가능한 읽기 전용 속성에 대 한 합니다.

MVVM 시나리오의 데이터 바인딩에 대상을 일반적으로 사용할 수 있는 속성에는 한 `DefaultBindingMode` 의 `TwoWay`합니다. 이러한 항목은 다음과 같습니다.

- `Value` 속성의 `Slider` 및 `Stepper`
- `IsToggled` 속성 `Switch`
- `Text` 속성의 `Entry`, `Editor`, 및 `SearchBar`
- `Date` 속성 `DatePicker`
- `Time` 속성 `TimePicker`

[ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) 샘플 되는 데이터 바인딩으로 4 개의 바인딩 모드를 보여 줍니다.는 `FontSize` 속성은 `Label` 고 원본이 `Value` 속성의는 `Slider`합니다. 이렇게 하면 각 `Slider` 해당의 글꼴 크기를 제어할 `Label`합니다. 하지만 `Slider` 요소 때문에 초기화 되지 않은 `DefaultBindingMode` 의 `FontSize` 속성은 `OneWay`합니다.

[ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) 에 바인딩을 설정 하는 예제는 `Value` 속성은 `Slider` 참조 하는 `FontSize` 각 속성 `Label`합니다. 초기화 하에서 더 잘 작동 하지만 이전 버전과 호환성 것 처럼는 `Slider` 요소 때문에 `Value` 의 속성은 `Slider` 에 `DefaultBindingMode` 의 `TwoWay`합니다.

[![바인딩 역방향 삼중 스크린 샷](images/ch16fg06-small.png "바인딩 역방향")](images/ch16fg06-large.png#lightbox "역방향 바인딩")

이것은 바인딩이 MVVM에 정의 된 방법을 유사 하 고 자주 바인딩이 형식을 사용 합니다.

## <a name="string-formatting"></a>문자열 서식 지정

대상 속성 형식의 경우 `string`를 사용할 수 있습니다는 [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) 속성에 정의 된 `BindingBase` 소스를 변환 하는 `string`합니다. 설정의 `StringFormat` 속성을 정적으로 사용 하는 문자열의 서식을 지정 하는.NET [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) 개체를 표시 하는 형식입니다. 태그 확장 내에서이 서식 문자열을 사용할 경우로 묶어 단일 따옴표 하므로 포함 된 태그 확장에 대 한 중괄호를 혼동할 수 없습니다.

[ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) 샘플에서는 사용 하는 방법을 보여 줍니다. `StringFormat` XAML에서 합니다.

[ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) 샘플에 대 한 바인딩을 사용 하 여 페이지의 크기를 표시 하는 방법을 보여 줍니다는 `Width` 및 `Height` 의 속성은 `ContentPage`합니다.

## <a name="why-is-it-called-path"></a>"Path" 호출 이유는?

[ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) 속성 `Binding` 일련의 속성 및 인덱서 마침표로 구분 하 여 수 있기 때문에 따라서 라고 합니다. [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) 샘플 몇 가지 예를 보여 줍니다.

## <a name="binding-value-converters"></a>바인딩 값 변환기

바인딩 소스 및 대상 속성을 다양 한 종류 바인딩 변환기를 사용 하 여 형식을 서로 변환할 수 있습니다. 이 클래스는 구현 하는 클래스는 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) 인터페이스 및 메서드도 포함: [ `Convert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.Convert/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) 원본을 대상으로 변환할 및 [ `ConvertBack` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.ConvertBack/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) 대상 소스에 변환 합니다.

[ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리는 변환에 대 한 예제는 `int` 에 `bool`합니다. 하 여 표시 됩니다는 [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) 수 있는 샘플의 `Button` 에 하나 이상의 문자를 입력 하는 경우는 `Entry`합니다.

[ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) 클래스는 `bool` 에 `string` 되는 텍스트에 대 한 반환할지 지정 하는 두 개의 속성을 정의 하 고 `false` 및 `true` 값입니다.
[ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) 비슷합니다. [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) 샘플 이러한 두 변환기를 사용 하 여 다른 텍스트에 따라 서로 다른 색으로 표시 하는 `Switch` 설정 합니다.

제네릭 [ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) 바꿀 수는 `BoolToStringConverter` 및 `BoolToColorConverter` 역할을 일반화 한 `bool`-에-모든 형식의 개체 변환기입니다.

## <a name="bindings-and-custom-views"></a>바인딩 및 사용자 지정 보기

데이터 바인딩을 사용 하 여 사용자 지정 컨트롤을 간소화할 수 있습니다. [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) 코드 파일 정의 `Text`, `TextColor`, `FontSize`, `FontAttributes`, 및 `IsChecked` 속성, 않지만 전혀 컨트롤의 시각적 개체에 대 한 논리가 없습니다.
대신는 [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) 파일에서 데이터 바인딩을 통해 해당 컨트롤의 시각적 개체에 대 한 모든 태그를 포함 된 `Label` 코드 숨김 파일에 정의 된 속성을 기반으로 하는 요소입니다.

[ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) 샘플는 `NewCheckBox` 사용자 지정 컨트롤입니다.



## <a name="related-links"></a>관련 링크

- [16 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [16 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
