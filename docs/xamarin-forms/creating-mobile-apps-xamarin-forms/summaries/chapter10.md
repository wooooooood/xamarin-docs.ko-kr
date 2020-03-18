---
title: 요약 - 10장. XAML 태그 확장
description: 'Xamarin.Forms로 모바일 앱 만들기: 요약 - 10장. XAML 태그 확장'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 076e9f5155492e5a69d906c587b24495fe39d3f1
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "61334326"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>요약 - 10장. XAML 태그 확장

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)

일반적으로 XAML 파서는 특성 값으로 설정된 모든 문자열을 기본 .NET 데이터 형식에 대한 표준 변환에 따른 속성의 형식으로 또는 [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute)를 사용하여 속성 또는 해당 형식에 연결된 [`TypeConverter`](xref:Xamarin.Forms.TypeConverter) 파생 항목으로 변환합니다.

그러나 경우에 따라 사전에 있는 항목, 정적 속성 또는 필드의 값과 같은 다른 원본에서 또는 일종의 계산을 통해 특성을 설정하는 것이 편리합니다.

이것은 *XAML 태그 확장*의 작업입니다. 이름에도 불구하고 XAML 태그 확장은 XML에 대한 확장이 *아닙니다*. XAML은 항상 유효한 XML입니다.

## <a name="the-code-infrastructure"></a>코드 인프라

XAML 태그 확장은 [`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension) 인터페이스를 구현하는 클래스입니다. 이러한 클래스는 일반적으로 이름 끝에 `Extension`이 붙지만, XAML에는 일반적으로 해당 접미사 없이 표시됩니다.

다음 XAML 태그 확장은 모든 XAML 구현에서 지원됩니다.

- `x:Static`: [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension)에서 지원
- `x:Reference`: [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension)에서 지원
- `x:Type`: [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension)에서 지원
- `x:Null`: [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension)에서 지원
- `x:Array`: [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension)에서 지원

이러한 네 가지 XAML 태그 확장은 Xamarin.Forms를 비롯한 여러 XAML 구현에서 지원됩니다.

- `StaticResource`: [`StaticResourceExtension`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)에서 지원
- `DynamicResource`: [`DynamicResourceExtension`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)에서 지원
- `Binding`: [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension)에서 지원&mdash;참조: [16장. 데이터 바인딩](chapter16.md)
- `TemplateBinding`: [`TemplateBindingExtension`](xref:Xamarin.Forms.Xaml.TemplateBindingExtension)에서 지원&mdash;이 책에서는 다루지 않음

추가 XAML 태그 확장은 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)과 관련하여 Xamarin.Forms에 포함되어 있습니다.

- [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)&mdash;이 책에서 다루지 않음

## <a name="accessing-static-members"></a>정적 멤버 액세스

[`x:Static`](xref:Xamarin.Forms.Xaml.StaticExtension) 요소를 사용하여 특성을 공용 정적 속성, 필드 또는 열거형 멤버의 값으로 설정합니다. [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member) 속성을 정적 멤버로 설정합니다. 일반적으로 `x:Static` 및 멤버 이름을 중괄호로 묶어 지정하는 것이 더 쉽습니다. `Member` 속성의 이름은 포함하지 않아도 되며, 멤버 자체만 포함합니다. 이 일반적인 구문은 [**SharedStatics**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) 샘플에 나와 있습니다. 정적 필드 자체는 [`AppConstants`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) 클래스에서 정의됩니다. 이 방법을 사용하면 프로그램을 통해 사용되는 상수를 설정할 수 있습니다.

[**SystemStatics**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) 샘플에서 보여주듯이, 추가 XML 네임스페이스 선언을 사용하여 .NET 프레임워크에 정의된 공용 정적 속성, 필드 또는 열거형 멤버를 참조할 수 있습니다.

## <a name="resource-dictionaries"></a>리소스 사전

`VisualElement` 클래스는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)형식의 개체로 설정할 수 있는 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources)라는 속성을 정의합니다. XAML 내에서 이 사전에 항목을 저장하고 `x:Key` 특성으로 해당 항목을 식별할 수 있습니다. 리소스 사전에 저장된 항목은 항목에 대한 모든 참조에서 공유됩니다.

### <a name="staticresource-for-most-purposes"></a>범용적인 StaticResource

대부분의 경우 [**ResourceSharing**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) 샘플에서 보여주듯이 [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) 태그 확장을 사용하여 리소스 사전의 항목을 참조합니다. `StaticResourceExtension` 요소 또는 `StaticResource`를 중괄호 안에 사용할 수 있습니다.

[![리소스 공유의 삼중 스크린샷](images/ch10fg03-small.png "리소스 공유")](images/ch10fg03-large.png#lightbox "리소스 공유")

`x:Static` 태그 확장과 `StaticResource` 태그 확장을 혼동해서는 안 됩니다.

### <a name="a-tree-of-dictionaries"></a>사전 트리

XAML 파서는 `StaticResource`가 발견되면 일치하는 키에 대한 시각적 트리 검색을 시작한 다음, 애플리케이션의 `App` 클래스에서 `ResourceDictionary`을 찾습니다. 이렇게 하면 시각적 트리에서 더 하위에 있는 리소스 사전의 항목이 시각적 트리에서 더 상위에 있는 리소스 사전을 재정의할 수 있습니다. 이는 [**ResourceTrees**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) 샘플에서 보여 줍니다.

### <a name="dynamicresource-for-special-purposes"></a>특수 목적의 DynamicResource

`StaticResource` 태그 확장은 `InitializeComponent` 호출에서 시각적 트리가 빌드될 때 사전에서 항목을 검색합니다. `StaticResource`에 대한 대안은 사전 키에 대한 링크를 유지 관리하고 키에서 참조하는 항목이 변경될 때 대상을 업데이트하는 [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)입니다.

`StaticResource`와 `DynamicResource`의 차이점은 [**DynamicVsStatic**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) 샘플에서 설명합니다.

`DynamicResource`에 의해 설정된 속성은 [11장, 바인딩 가능한 인프라](chapter11.md)에서 설명된 대로 바인딩 가능한 속성에 의해 지원되어야 합니다.

## <a name="lesser-used-markup-extensions"></a>덜 사용되는 태그 확장

[`x:Null`](xref:Xamarin.Forms.Xaml.NullExtension) 태그 확장을 사용하여 속성을 `null`로 설정합니다.

[`x:Type`](xref:Xamarin.Forms.Xaml.TypeExtension) 태그 확장을 사용하여 속성을 .NET `Type` 개체로 설정합니다.

[`x:Array`](xref:Xamarin.Forms.Xaml.ArrayExtension)를 사용하여 배열을 정의합니다. [`Type`] 속성을 `x:Type` 태그 확장으로 설정하여 배열 멤버의 형식을 지정합니다.

## <a name="a-custom-markup-extension"></a>사용자 지정 태그 확장

[`ProvideValue`](xref:Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue(System.IServiceProvider)) 메서드를 사용하여 [`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension) 인터페이스를 구현하는 클래스를 작성하여 자체 XAML 태그 확장을 만들 수 있습니다.

[`HslColorExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) 클래스는 이러한 요구 사항을 충족합니다. `H`, `S`, `L` 및 `A`라는 속성의 값을 기반으로 `Color` 형식의 값을 만듭니다. 이 클래스는 이 책에서 작성하고 사용하는 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)라는 Xamarin.Forms 라이브러리의 첫 번째 항목입니다.

[**CustomExtensionDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) 샘플에서는 이 라이브러리를 참조하고 사용자 지정 태그 확장을 사용하는 방법을 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [10장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [10장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
- [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md)
