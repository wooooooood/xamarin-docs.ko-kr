---
title: 16장 요약 데이터 바인딩
description: 'Xamarin.Forms로 모바일 앱 만들기: 16장 요약 데이터 바인딩'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 2d61413fb1d8c28a3957da53601d0ad682f35518
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "70771100"
---
# <a name="summary-of-chapter-16-data-binding"></a>16장 요약 데이터 바인딩

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)

> [!NOTE] 
> 이 페이지의 정보는 Xamarin.Forms가 책에 제공된 자료와 다른 영역을 표시합니다.

프로그래머는 한 개체의 속성이 변경된 경우를 감지하고 이를 사용하여 다른 개체의 속성 값을 변경하는 이벤트 처리기를 작성하는 경우가 많습니다. 이 프로세스는 *데이터 바인딩* 기술을 사용하여 자동화할 수 있습니다. 데이터 바인딩은 일반적으로 XAML에 정의되며 사용자 인터페이스 정의의 일부가 됩니다.

이러한 데이터 바인딩은 사용자 인터페이스 개체를 기본 데이터에 연결하는 경우가 많습니다. 이 기술은 [**18장 MVVM**](chapter18.md)에서 더 자세히 설명합니다. 그러나 데이터 바인딩은 둘 이상의 사용자 인터페이스 요소를 연결할 수도 있습니다. 이 장의 데이터 바인딩의 초기 예제는 대부분 이 기술을 보여줍니다.

## <a name="binding-basics"></a>바인딩 기본 사항

데이터 바인딩에는 몇 가지 속성, 메서드 및 클래스가 포함되어 있습니다.

- [`Binding`](xref:Xamarin.Forms.Binding) 클래스는 [`BindingBase`](xref:Xamarin.Forms.BindingBase)에서 파생되었으며 데이터 바인딩의 여러 특성을 캡슐화합니다.
- [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 속성은 [`BindableObject`](xref:Xamarin.Forms.BindableObject) 클래스에 의해 정의됩니다.
- [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 메서드도 [`BindableObject`](xref:Xamarin.Forms.BindableObject) 클래스에 의해 정의됩니다.
- [`BindableObjectExtensions`](xref:Xamarin.Forms.BindableObjectExtensions) 클래스는 세 가지 추가 `SetBinding` 메서드를 정의합니다.

다음 두 클래스는 바인딩에 대한 XAML 태그 확장을 지원합니다.

- [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension)은 `Binding` 태그 확장을 지원합니다.
- [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension)은 `x:Reference` 태그 확장을 지원합니다.

데이터 바인딩에는 두 가지 인터페이스가 포함됩니다.

- `System.ComponentModel` 네임스페이스의 [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged)는 속성 변경 시 알림을 구현하기 위한 것입니다.
- [`IValueConverter`](xref:Xamarin.Forms.IValueConverter)는 데이터 바인딩의 한 형식에서 다른 형식으로 값을 변환하는 작은 클래스를 정의하는 데 사용됩니다.

데이터 바인딩은 동일한 개체의 두 속성 또는 (보다 일반적으로) 두 개의 다른 개체를 연결합니다. 이러한 두 속성을 *source* 및 *target*이라고 합니다. 일반적으로 소스 속성을 변경하면 대상 속성에서 변경 내용이 발생하지만 방향이 반대로 바뀝니다. 관련 없음:

- *target* 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)에 의해 지원되어야 합니다.
- *source* 속성은 일반적으로 [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged)를 구현하는 클래스의 멤버입니다.

`INotifyPropertyChanged`를 구현하는 클래스는 속성 값이 변경될 때 [`PropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged) 이벤트를 발생시킵니다. `BindableProperty`에서 지원하는 속성이 값을 변경하면 `BindableObject`가 `INotifyPropertyChanged`를 구현하고 `PropertyChanged` 이벤트를 자동으로 실행하지만 `BindableObject`에서 파생하지 않고 `INotifyPropertyChanged`를 구현하는 고유한 클래스를 작성할 수 있습니다.

## <a name="code-and-xaml"></a>코드 및 XAML

[**OpacityBindingCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) 샘플은 코드에서 데이터 바인딩을 설정하는 방법을 보여줍니다.

- 원본은 `Slider`의 `Value` 속성입니다.
- 대상은 `Label`의 `Opacity` 속성입니다.

`Label` 개체의 `BindingContext`를 `Slider` 개체로 설정하여 두 개체를 연결합니다. 두 속성은 `OpacityProperty` 바인딩 가능 속성과 문자열로 표현된 `Slider`의 `Value` 속성을 참조하여 `Label`에서 [`SetBinding`](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) 확장 메서드를 호출하여 연결됩니다.

그런 다음, `Slider`를 조작하면 `Label` 페이드 인 및 페이드 아웃됩니다.

[**OpacityBindingXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml)은 XAML에서 데이터 바인딩 집합을 사용하는 동일한 프로그램입니다. `Label`의 `BindingContext`이 `Slider`를 참조하는 `x:Reference` 태그 확장으로 설정되고 `Opacity`의 `Label` 속성이 `Slider`의 `Value` 속성을 참조하는 [`Path`](xref:Xamarin.Forms.Binding.Path) 속성이 있는 `Binding` 태그 확장으로 설정됩니다.

## <a name="source-and-bindingcontext"></a>원본 및 BindingContext

[**BindingSourceCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) 샘플은 코드의 다른 방법을 보여줍니다. [`Source`](xref:Xamarin.Forms.Binding.Source) 속성을 `Slider` 개체로 설정하고 [`Path`](xref:Xamarin.Forms.Binding.Path) 속성을 "Value"로 설정하여 `Binding` 개체를 만듭니다. 그러면 `BindableObject`의 [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 메서드가 `Label` 개체에서 호출됩니다.

[`Binding`생성자](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object))를 사용하여 `Binding` 개체를 정의할 수도 있습니다.

[**BindingSourceXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) 샘플은 XAML의 비교 가능한 기술을 보여줍니다. `Label`의 `Opacity` 속성이 [`Path`](xref:Xamarin.Forms.Binding.Path)를 `Value` 속성으로 설정하고 [`Source`](xref:Xamarin.Forms.Binding.Source)는 포함된 `x:Reference` 태그 확장으로 설정되어 `Binding` 태그 확장이 설정됩니다.

요약하면 바인딩 소스 개체를 참조하는 두 가지 방법이 있습니다.

- 대상의 `BindingContext` 속성을 통해
- `Binding` 개체 자체의 `Source` 속성을 통해

둘 다 지정하는 경우 두 번째가 우선적으로 적용됩니다. `BindingContext`의 이점은 시각적 트리를 통해 전파된다는 것입니다. 여러 대상 속성이 동일한 원본 개체에 바인딩되는 경우 *매우* 유용합니다.

[**WebViewDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) 프로그램은 [`WebView`](xref:Xamarin.Forms.WebView) 요소를 사용하여 이 기술을 보여줍니다. 앞뒤로 이동하기 위한 두 개의 `Button` 요소는 `WebView`를 참조하는 부모로부터 `BindingContext`를 상속합니다. 두 단추의 `IsEnabled` 속성에는 `WebView`의 [`CanGoBack`](xref:Xamarin.Forms.WebView.CanGoBack) 및 [`CanGoForward`](xref:Xamarin.Forms.WebView.CanGoForward) 읽기 전용 속성 설정에 따라 단추 `IsEnabled` 속성을 대상으로 하는 간단한 `Binding` 태그 확장이 있습니다.

## <a name="the-binding-mode"></a>바인딩 모드

`Binding`의 [`Mode`](xref:Xamarin.Forms.BindingBase.Mode) 속성을 [`BindingMode`](xref:Xamarin.Forms.BindingMode) 열거형의 멤버로 설정합니다.

- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay): 원본 속성의 변경 내용이 대상의 영향을 줍니다.
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource): 대상 속성의 변경 내용이 원본에 영향을 줍니다.
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay): 원본 및 대상의 변경 내용이 서로 영향을 줍니다.
- [`Default`](xref:Xamarin.Forms.BindingMode.Default): 대상 `BindableProperty`가 생성될 때 지정된 [`DefaultBindingMode`](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode)를 사용합니다. 지정되지 않으면 기본값은 기본 바인딩 가능한 속성의 경우 `OneWay`이고, 읽기 전용 바인딩 가능 속성의 경우 `OneWayToSource`입니다.

> [!NOTE]
> 이제 `BindingMode` 열거형에는 바인딩 컨텍스트가 변경되고 원본 속성이 변경되지 않는 경우에만 바인딩 적용을 위한 `OnTime`도 포함됩니다.

MVVM 시나리오에서 데이터 바인딩의 대상이 될 가능성이 있는 속성은 일반적으로 `TwoWay`의 `DefaultBindingMode`가 있습니다. 이러한 항목은 다음과 같습니다.

- `Slider`, `Stepper`의 `Value` 속성
- `Switch`의 `IsToggled` 속성
- `Entry`, `Editor` 및 `SearchBar`의 `Text` 속성
- `DatePicker`의 `Date` 속성
- `TimePicker`의 `Time` 속성

[**BindingModes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) 샘플은 데이터 바인딩이 있는 네 가지 바인딩 모드를 보여줍니다. 여기서 대상은 `Label`의 `FontSize` 속성이고 원본은 `Slider`의 `Value` 속성입니다. 이를 통해 각 `Slider`는 해당 `Label`의 글꼴 크기를 제어할 수 있습니다. 그러나 `FontSize` 속성의 `DefaultBindingMode`가 `OneWay`이므로 `Slider` 요소가 초기화되지 않습니다.

[**ReverseBinding**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) 샘플은 각 `Label`의 `FontSize` 속성을 참조하는 `Slider`의 `Value` 속성에 대한 바인딩을 설정합니다. 이는 거꾸로 표시되지만 `Slider`의 `Value` 속성에 `TwoWay`의 `DefaultBindingMode`가 있기 때문에 `Slider` 요소를 초기화하는 데 더 효율적입니다.

[![역방향 바인딩의 세 가지 스크린샷](images/ch16fg06-small.png "역방향 바인딩")](images/ch16fg06-large.png#lightbox "역방향 바인딩")

이는 MVVM에서 바인딩이 정의되는 방법과 유사하며 이러한 형식의 바인딩을 자주 사용합니다.

## <a name="string-formatting"></a>문자열 서식 지정

대상 속성이 `string` 형식이면 `BindingBase`에 의해 정의된 [`StringFormat`](xref:Xamarin.Forms.BindingBase.StringFormat) 속성을 사용하여 원본을 `string`으로 변환할 수 있습니다. `StringFormat` 속성을 정적 [`String.Format`](xref:System.String.Format(System.String,System.Object)) 형식과 함께 사용하여 개체를 표시하는 .NET 서식 지정 문자열로 설정합니다. 태그 확장 내에서 이 서식 지정 문자열을 사용하는 경우 포함된 태그 확장에 중괄호를 사용하지 않도록 작은따옴표로 묶습니다.

[**ShowViewValues**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) 샘플은 XAML에서 `StringFormat`을 사용하는 방법을 보여줍니다.

[**WhatSizeBindings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) 샘플은 `ContentPage`의 `Width` 및 `Height` 속성에 대한 바인딩이 포함된 페이지 크기를 표시하는 방법을 보여줍니다.

## <a name="why-is-it-called-path"></a>"Path"라고 불리는 이유는?

`Binding`의 [`Path`](xref:Xamarin.Forms.Binding.Path) 속성은 마침표로 구분된 일련의 속성과 인덱서일 수 있으므로 그렇게 불립니다. [**BindingPathDemos**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) 샘플은 몇 가지 예제를 보여줍니다.

## <a name="binding-value-converters"></a>바인딩 값 변환기

바인딩의 원본 속성과 대상 속성이 다른 형식인 경우 바인딩 변환기를 사용하여 형식 간에 변환할 수 있습니다. 이는 [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) 인터페이스를 구현하는 클래스로서 원본을 대상으로 변환하는 [`Convert`](xref:Xamarin.Forms.IValueConverter.Convert(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) 및 대상을 소스로 변환하는 [`ConvertBack`](xref:Xamarin.Forms.IValueConverter.ConvertBack(System.Object,System.Type,System.Object,System.Globalization.CultureInfo))의 두 가지 메서드를 포함하는 클래스입니다.

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`IntToBoolConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) 클래스는 `int`를 `bool`로 변환하는 예제입니다. [**ButtonEnabler**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) 샘플에서 설명합니다. `Entry`에 하나 이상의 문자가 입력된 경우에만 `Button`을 사용할 수 있습니다.

[`BoolToStringConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) 클래스는 `bool`을 `string`으로 변환하고 두 개의 속성을 정의하여 `false` 및 `true` 값에 대해 반환되는 텍스트를 지정합니다.
[`BoolToColorConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs)는 유사합니다. [**SwitchText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) 샘플은 이러한 두 변환기를 사용하여 `Switch` 설정에 따라 다른 색으로 다른 텍스트를 표시하는 방법을 보여줍니다.

제네릭 [`BoolToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs)는 `BoolToStringConverter` 및 `BoolToColorConverter`를 대체하고 모든 형식의 일반화된 `bool` 개체 변환기로 사용할 수 있습니다.

## <a name="bindings-and-custom-views"></a>바인딩 및 사용자 지정 뷰

데이터 바인딩을 사용하여 사용자 지정 컨트롤을 단순화할 수 있습니다. [`NewCheckBox.cs`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) 코드 파일은 `Text`, `TextColor`, `FontSize`, `FontAttributes` 및 `IsChecked` 속성을 정의하지만 컨트롤의 시각적 개체에 대한 논리는 전혀 없습니다.
대신 [`NewCheckBox.cs.xaml`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) 파일에는 코드 숨김 파일에 정의된 속성을 기반으로 하는 `Label` 요소의 데이터 바인딩을 통해 컨트롤의 시각적 개체에 대한 모든 태그가 포함됩니다.

[**NewCheckBoxDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) 샘플은 `NewCheckBox` 사용자 지정 컨트롤을 보여줍니다.

## <a name="related-links"></a>관련 링크

- [16장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [16장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
- [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
