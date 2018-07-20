---
title: 요약 10 장입니다. XAML 태그 확장
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 10 장 요약 합니다. XAML 태그 확장'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 74f7e2846a9e8d8390a8322c57db0845718bbba7
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39157005"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>요약 10 장입니다. XAML 태그 확장

XAML 파서는 특성 값으로 설정 되어 기본.NET 데이터 형식에 대 한 표준 변환에 따라 속성의 형식 문자열을 변환 하는 일반적으로 또는 [ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter) 속성 또는 해당 형식과 연결 된 파생 클래스는 [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute).

하지만 예를 들어, 사전 또는 정적 속성 또는 필드의 값에 항목을 다른 원본 또는 일종의 계산에서 특성을 설정 하기에 편리한 경우가 있습니다.

이 작업을 *XAML 태그 확장*합니다. XAML 태그 확장은 이름이 *되지* XML에 대 한 확장입니다. XAML은 항상 xml이 유효한 XML입니다.

## <a name="the-code-infrastructure"></a>코드 인프라

XAML 태그 확장은 구현 하는 클래스를 [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) 인터페이스입니다. 이러한 클래스에는 종종 라는 단어가 포함 `Extension` 해당 이름의 끝에 해당 접미사 없이 하지만 일반적으로 XAML에 나타납니다.

다음 XAML 태그 확장은 XAML의 모든 구현에서 지원 됩니다.

- `x:Static` 지 원하는 [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension)
- `x:Reference` 지 원하는 [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension)
- `x:Type` 지 원하는 [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension)
- `x:Null` 지 원하는 [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension)
- `x:Array` 지 원하는 [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension)

이러한 네 가지 XAML 태그 확장은 XAML, Xamarin.Forms를 비롯 한 많은 구현에서 지원 됩니다.

- `StaticResource` 지 원하는 [`StaticResourceExtension`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)
- `DynamicResource` 지 원하는 [`DynamicResourceExtension`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)
- `Binding` 지 원하는 [ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension) &mdash;에 설명 된 [16 장입니다. 데이터 바인딩](#chapter16)
- `TemplateBinding` 지 원하는 [ `TemplateBindingExtension` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) &mdash;책에서 다루지 않는

추가 XAML 태그 확장에 연결 하 여 Xamarin.Forms 있기 [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout):

- [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)&mdash;이 책에서 다루지 않는

## <a name="accessing-static-members"></a>정적 멤버에 액세스

사용 된 [ `x:Static` ](xref:Xamarin.Forms.Xaml.StaticExtension) 공용 정적 속성, 필드 또는 열거형 멤버의 값으로 특성을 설정할 요소입니다. 설정 된 [ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member) 정적 멤버는 속성입니다. 지정 하려면 더 쉽게 `x:Static` 고 중괄호로 멤버 이름입니다. 이름을 합니다 `Member` 속성을 포함 하지 않아도 멤버 자체입니다. 이 일반 구문이 나와 합니다 [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) 샘플입니다. 자체 정적 필드에 정의 되어는 [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) 클래스입니다. 이 기법을 사용 하면 프로그램을 통해 사용 되는 상수를 설정할 수 있습니다.

XML 네임 스페이스 선언 추가 사용 하 여 참조할 수 있습니다 공용 정적 속성, 필드 또는.NET framework에 정의 된 열거형 멤버에 설명 된 대로 합니다 [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) 샘플 .

## <a name="resource-dictionaries"></a>리소스 사전

합니다 `VisualElement` 라는 속성을 정의 하는 클래스 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) 형식의 개체에 설정할 수 있는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)합니다. XAML을 내에서이 사전에 항목을 저장 하 고 확인할 수를 사용 하 여는 `x:Key` 특성입니다. 리소스 사전에 저장 되어 있는 항목은 항목에 대 한 모든 참조에서 공유 됩니다.

### <a name="staticresource-for-most-purposes"></a>대부분의 용도 StaticResource

대부분의 경우에서 사용 합니다 [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) 태그 확장으로 볼 수 있듯이, 리소스 사전에서 항목을 참조 하는 [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) 샘플 . 사용할 수는 `StaticResourceExtension` 요소 또는 `StaticResource` 중괄호 내에서:

[![리소스를 공유 하는 triple 스크린샷](images/ch10fg03-small.png "리소스 공유")](images/ch10fg03-large.png#lightbox "리소스 공유")

혼동 하지 마십시오 합니다 `x:Static` 태그 확장 및 `StaticResource` 태그 확장 합니다.

### <a name="a-tree-of-dictionaries"></a>사전의 트리

XAML 파서를 발견할 경우는 `StaticResource`, 일치 하는 키에 대 한 시각적 트리 검색을 시작 하 고 다음에서 검색 합니다 `ResourceDictionary` 응용 프로그램에서 `App` 클래스입니다. 이 시각적 트리의 더 높은 리소스 사전 재정의 하려면 시각적 트리의 하위 수준 리소스 사전에 항목을 허용 합니다. 에 설명 되어이 [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) 샘플입니다.

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource 특수 목적을 위해

합니다 `StaticResource` 태그 확장 하는 동안 시각적 트리를 빌드하면 사전에서 검색할 항목으로 인해는 `InitializeComponent` 호출 합니다. 대 안으로 `StaticResource` 은 [ `DynamicResource` ](xref:Xamarin.Forms.Xaml.DynamicResourceExtension), 사전 키에 대 한 링크를 유지 관리 하며 주요 변경 사항에서 항목을 참조 하는 경우 대상 업데이트 합니다.

차이점 `StaticResource` 하 고 `DynamicResource` 에 설명 되어는 [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) 샘플입니다.

속성을 설정 `DynamicResource` 에 설명 된 대로 바인딩 가능한 속성으로 백업 해야 [11 장 바인딩할 수 있는 인프라](chapter11.md)합니다.

## <a name="lesser-used-markup-extensions"></a>덜 사용 하는 태그 확장

사용 된 [ `x:Null` ](xref:Xamarin.Forms.Xaml.NullExtension) 속성을 설정 하는 태그 확장 `null`합니다.

사용 된 [ `x:Type` ](xref:Xamarin.Forms.Xaml.TypeExtension) .net 속성을 설정 하려면 태그 확장 `Type` 개체입니다.

사용 하 여 [ `x:Array` ](xref:Xamarin.Forms.Xaml.ArrayExtension) 배열을 정의 합니다. 배열 멤버의 유형을 설정 하 여 지정 된 [`Type`] 속성을는 `x:Type` 태그 확장 합니다.

## <a name="a-custom-markup-extension"></a>사용자 지정 태그 확장

구현 하는 클래스를 작성 하 여 사용자 고유의 XAML 태그 확장을 만들 수 있습니다 합니다 [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) 사용 하 여 인터페이스를 [ `ProvideValue` ](xref:Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue(System.IServiceProvider)) 메서드.

합니다 [ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) 클래스에는 이러한 요구 사항을 충족 합니다. 형식의 값을 만들면 `Color` 명명 된 속성의 값에 따라 `H`를 `S`, `L`, 및 `A`합니다. 이 클래스는 명명 된 Xamarin.Forms 라이브러리의 첫 번째 항목 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 구축을이 가이드의 과정을 통해 사용 합니다.

합니다 [ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) 샘플에서는이 라이브러리를 참조 하 고 사용자 지정 태그 확장을 사용 하는 방법을 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [10 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [10 장에 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
- [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md)
