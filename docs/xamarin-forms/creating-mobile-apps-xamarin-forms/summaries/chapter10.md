---
title: 요약 장 10입니다. XAML 태그 확장
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 3223bc20448bd8354e84dc67ee64a8dc435f7667
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>요약 장 10입니다. XAML 태그 확장

XAML 파서에서 기본.NET 데이터 형식에 대 한 표준 변환에 따라 속성의 형식으로 특성 값으로 설정 하는 모든 문자열을 변환 하는 일반적으로 또는 [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) 속성 또는와 해당 형식에 연결 된 파생 클래스는 [`TypeConverterAttribute`](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/).

하지만 정적 속성 또는 필드의 값 또는 사전에서 항목 예를 들어 다른 원본의 또는 특정 유형의 계산에서 특성을 설정 하는 편리한 경우가 쉽습니다.

이 작업의 한 *XAML 태그 확장*합니다. XAML 태그 확장은 이름이 *하지* XML에 대 한 확장입니다. XAML은 항상 xml이 유효한 XML입니다.

## <a name="the-code-infrastructure"></a>코드 인프라

XAML 태그 확장은 구현 하는 클래스는 [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) 인터페이스입니다. 이러한 클래스에는 종종 이라는 단어가 포함 `Extension` 해당 이름의 끝에 해당 접미사 없이 하지만 일반적으로 XAML에 나타납니다.

다음 XAML 태그 확장은 XAML 모든 구현에서 지원 됩니다.

- `x:Static` 지원 [`StaticExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/)
- `x:Reference` 지원 [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/)
- `x:Type` 지원 [`TypeExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/)
- `x:Null` 지원 [`NullExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/)
- `x:Array` 지원 [`ArrayExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/)

Xamarin.Forms를 포함 하 여 XAML의 여러 구현 된 이러한 4 개의 XAML 태그 확장 지원 됩니다.

- `StaticResource` 지원 [`StaticResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/)
- `DynamicResource` 지원 [`DynamicResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/)
- `Binding` 지 원하는 [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) &mdash;에 설명 된 [장 16입니다. 데이터 바인딩](#chapter16)
- `TemplateBinding` 지 원하는 [ `TemplateBindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) &mdash;책에서 설명 하지 않는

추가로 XAML 태그 확장에 연결 하 여 Xamarin.Forms에 포함 된 [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/):

- [`ConstraintExpression`](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/)&mdash;이 책에서 설명 하지 않는

## <a name="accessing-static-members"></a>정적 멤버에 액세스

사용 하 여는 [ `x:Static` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) 공용 정적 속성, 필드 또는 열거형 멤버의 값으로 특성을 설정 하는 요소입니다. 설정의 [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) 정적 멤버 속성입니다. 일반적으로 쉽게 지정할 수는 `x:Static` 고 중괄호로 멤버 이름입니다. 이름에서 `Member` 속성이 포함 될 필요는 없습니다 멤버 자체입니다. 이 일반적인 구문에 표시 되는 [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) 샘플. 정적 필드 자체에 정의 된는 [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) 클래스입니다. 이 기술을 사용 하면 프로그램을 통해 사용 되는 상수를 설정할 수 있습니다.

추가 XML 네임 스페이스 선언으로 참조할 수 있습니다 공용 정적 속성, 필드 또는.NET framework에 정의 된 열거형 멤버와 같이 [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) 샘플 .

## <a name="resource-dictionaries"></a>리소스 사전

`VisualElement` 라는 속성을 정의 하는 클래스 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) 형식의 개체를 설정할 수 있는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)합니다. XAML에서는 내에서이 사전에 항목을 저장 하 고 확인할 수를 사용 하 여는 `x:Key` 특성입니다. 리소스 사전에 저장 된 항목은 항목에 대 한 모든 참조 간에 공유 됩니다.

### <a name="staticresource-for-most-purposes"></a>대부분의 용도 대 한 StaticResource

대부분의 경우를 사용 하 여는 [ `StaticResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/) 태그 확장에 나타난 것 처럼 리소스 사전에서 항목을 참조 하는 [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) 샘플 . 사용할 수는 `StaticResourceExtension` 요소 또는 `StaticResource` 중괄호 안에:

[![리소스는 공유의 삼중 스크린 샷](images/ch10fg03-small.png "리소스 공유")](images/ch10fg03-large.png#lightbox "리소스 공유")

혼동 하지 마십시오는 `x:Static` 태그 확장 및 `StaticResource` 태그 확장 합니다.

### <a name="a-tree-of-dictionaries"></a>사전의 트리

XAML 파서가 발생 했을 때는 `StaticResource`, 일치 하는 키에 대 한 시각적 트리를 검색을 시작 하 고 다음 찾으므로 `ResourceDictionary` 응용 프로그램의 `App` 클래스입니다. 이 시각적 트리에서 상위 수준에 리소스 사전이 재정의 하는 시각적 트리에 깊은 리소스 사전에 항목을 허용 합니다. 이 확인할는 [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) 샘플.

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource 특별 한 용도로

`StaticResource` 태그 확장 하는 동안 시각적 트리를 빌드할 때 사전에서 검색할 항목으로 인해는 `InitializeComponent` 호출 합니다. 에 대 한 대안 `StaticResource` 은 [ `DynamicResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/), 사전 키에 대 한 링크를 유지 관리 하 고 항목 주요 변경 내용에 의해 참조 될 때 대상을 업데이트 합니다.

차이 `StaticResource` 및 `DynamicResource` 에 설명 된는 [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) 샘플.

속성을 설정 `DynamicResource` 에 설명 된 대로 바인딩 가능한 속성에 의해 지원 되어야 합니다 [11 장 바인딩 가능한 구조](chapter11.md)합니다.

## <a name="lesser-used-markup-extensions"></a>덜 사용 태그 확장

사용 하 여는 [ `x:Null` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) 태그 확장 속성 설정 하려면 `null`합니다.

사용 하 여는 [ `x:Type` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) .net 속성을 설정 하는 태그 확장 `Type` 개체입니다.

사용 하 여 [ `x:Array` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) 배열을 정의할 수 있습니다. 배열 멤버의 유형을 설정 하 여 지정 된 [`Type`] 속성을는 `x:Type` 태그 확장 합니다.

## <a name="a-custom-markup-extension"></a>사용자 지정 태그 확장

구현 하는 클래스를 작성 하 여 사용자 고유의 XAML 태그 확장을 만들 수 있습니다는 [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) 와 상호 작용할는 [ `ProvideValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue/p/System.IServiceProvider/) 메서드.

[ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) 클래스에는 이러한 요구 사항을 만족 합니다. 형식의 값을 만들고 `Color` 명명 된 속성의 값을 기반으로 `H`, `S`, `L`, 및 `A`합니다. 이 클래스는 명명 된 Xamarin.Forms 라이브러리의 첫 번째 항목 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 쌓이는 것 이며이 책의 과정 동안 사용 되는입니다.

[ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) 샘플에는이 라이브러리를 참조 하 고 사용자 지정 태그 확장을 사용 하는 방법을 보여 줍니다.



## <a name="related-links"></a>관련 링크

- [10 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [10 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
