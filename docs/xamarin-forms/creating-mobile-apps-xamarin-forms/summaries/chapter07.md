---
title: 요약 - 7장. XAML과 코드 비교
description: 'Xamarin.Forms로 모바일 앱 만들기: 요약 - 7장. XAML과 코드 비교'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0b92988e1e838072fca0d8a284455a62db05e757
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136863"
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>요약 - 7장. XAML과 코드 비교

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)

> [!NOTE]
> 이 페이지의 정보는 Xamarin.Forms가 이 책에 제공된 자료와는 다르게 사용되는 경우를 설명합니다.

Xamarin.Forms는 Extensible Application Markup Language, 즉 XAML(“자멜”로 읽음)이라고 하는 XML 기반 생성 언어를 지원합니다. XAML은 Xamarin.Forms 애플리케이션의 사용자 인터페이스 레이아웃을 정의하고, 사용자 인터페이스 요소와 기본 데이터 사이의 바인딩을 정의할 때 C#에 대한 대안을 제공합니다.

## <a name="properties-and-attributes"></a>속성 및 특성

Xamarin.Forms 클래스 및 구조체는 XAML에서 XML 요소가 되고, 이러한 클래스 및 구조체의 속성은 XML 특성이 됩니다. 클래스가 XAML로 인스턴스화되기 위해서는 일반적으로 공용 매개 변수 없는 생성자를 포함해야 합니다. XAML에 설정되는 모든 속성은 공용 `set` 접근자를 포함해야 합니다.

기본 데이터 형식(`string`, `double`, `bool` 등)의 속성에 대해 XAML 파서는 표준 `TryParse` 메서드를 사용해서 특성 설정을 해당 형식으로 변환합니다. XAML 파서는 또한 열거형 형식을 쉽게 처리할 수 있으며, 열거형 형식이 `Flags` 특성으로 플래그 지정되어 있으면 열거형 멤버를 결합할 수 있습니다.

XAML 파서를 돕기 위해 더 복잡한 형식(또는 이러한 형식의 속성)에는 문자열 값에서 해당 형식으로의 변환을 지원하는 [`TypeConverter`](xref:Xamarin.Forms.TypeConverter)에서 파생되는 클래스를 식별하는 [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute)가 포함될 수 있습니다. 예를 들어 [`ColorTypeConverter`](xref:Xamarin.Forms.ColorTypeConverter)는 "#rrggbb"와 같은 색 이름 및 문자열을 `Color` 값으로 변환합니다.

## <a name="property-element-syntax"></a>속성-요소 구문

XAML에서 클래스 및 클래스로부터 생성되는 개체는 XML 요소로 표시됩니다. 이러한 요소를 *개체 요소*라고 부릅니다. 이러한 개체의 속성 대부분은 XML 특성으로 표시됩니다. 이를 *속성 특성*이라고 부릅니다.

일부 경우에는 단순 문자열로 표시될 수 없는 개체로 속성을 설정해야 합니다. 이 경우에 XAML은 클래스 이름과 속성 이름이 마침표로 구분된 *속성 요소*라는 태그를 지원합니다. 그런 다음에는 속성-요소 태그 쌍 내에 개체 요소가 표시될 수 있습니다.

## <a name="adding-a-xaml-page-to-your-project"></a>프로젝트에 XAML 페이지 추가

Xamarin.Forms 이식 가능한 클래스 라이브러리는 처음 생성될 때 XAML 페이지를 포함할 수 있습니다. 또는 사용자가 기존 프로젝트에 XAML 페이지를 추가할 수 있습니다. 대화 상자에서 새 항목을 추가하려면 XML 페이지 또는 `ContentPage` 및 XAML을 참조하는 항목을 선택합니다. (`ContentView`가 아님).

> [!NOTE]
> 이 장이 작성된 이후 Visual Studio 옵션이 변경되었습니다.

파일 확장명이 .xaml인 XAML 파일과 확장명이 .xaml.cs인 C# 파일을 포함한 두 파일이 생성됩니다. C# 파일은 종종 XAML 파일의 *코드 숨김*이라고 부릅니다. 코드 숨김 파일은 `ContentPage`에서 파생되는 partial 클래스 정의입니다. 빌드 시간에 XAML이 구문 분석되고 다른 partial 클래스 정의가 동일 클래스에 대해 생성됩니다. 이 생성된 클래스에는 코드 숨김 파일의 생성자로부터 호출되는 `InitializeComponent`라는 메서드가 포함됩니다.

런타임 중에는 `InitializeComponent` 호출이 완료될 때, XAML 파일의 모든 요소가 C# 코드에서 생성되었을 때와 같이 인스턴스화되고 초기화됩니다.

XAML 파일의 루트 요소는 `ContentPage`입니다. 루트 태그에는 최소한 2개 이상의 XML 네임스페이스 선언이 포함됩니다. 하나는 Xamarin.Forms 요소를 위한 선언이고, `x` 접두사를 정의하는 다른 하나는 모든 XAML 구현에 내장된 요소 및 특성을 위한 선언입니다. 루트 태그에는 또한 `ContentPage`에서 파생되는 클래스의 이름 및 네임스페이스를 나타내는 `x:Class` 특성이 포함됩니다. 이 태그는 코드 숨김 파일의 네임스페이스 및 클래스 이름과 일치합니다.

XAML 및 코드 조합은 [**CodePlusXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) 샘플로 표시됩니다.

## <a name="the-xaml-compiler"></a>XAML 컴파일러

Xamarin.Forms에 XAML 컴파일러가 있지만, 이를 사용할지 여부는 [`XamlCompilationAttribute`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 사용에 따라 선택적입니다. XAML이 컴파일되지 않으면 XAML이 빌드 시간에 구문 분석되고 XAML 파일이 PCL에 포함됩니다. 이것은 또한 런타임에 구문 분석됩니다. XAML이 컴파일되면 빌드 프로세스가 XAML을 이진 형식으로 변환하며, 런타임 처리가 더 효율적입니다.

## <a name="platform-specificity-in-the-xaml-file"></a>XAML 파일의 플랫폼 특이성

XAML에서는 [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) 클래스를 사용하여 플랫폼에 종속된 태그를 선택할 수 있습니다. 이 클래스는 제네릭 클래스이며 대상 형식과 일치하는 `x:TypeArguments` 특성으로 인스턴스화되어야 합니다. [`OnIdiom`](xref:Xamarin.Forms.OnIdiom`1) 클래스도 비슷하지만 훨씬 드물게 사용됩니다.

이 책이 게시된 후 `OnPlatform` 사용이 변경되었습니다. 원래는 이름이 `iOS`, `Android` 및 `WinPhone`인 속성들과 함께 사용되었지만, 이제는 자식 [`On`](xref:Xamarin.Forms.On) 개체와 함께 사용됩니다. [`Platform`](xref:Xamarin.Forms.On.Platform) 속성을 [`Device`](xref:Xamarin.Forms.Device) 클래스의 공용 `const` 필드와 일치하는 문자열로 설정합니다. [`Value`](xref:Xamarin.Forms.On.Value) 속성을 `OnPlatform` 태그의 `x:TypeArguments` 특성과 일치하는 값으로 설정합니다.

[**ScaryColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) 샘플에서 보여주는 `OnPlatform`은 거의 동일한 XAML 블록을 포함하기 때문에 호출됩니다. 이러한 반복 태그가 있으면 이를 줄이기 위한 기술을 사용할 수 있음을 나타냅니다.

## <a name="the-content-property-attributes"></a>콘텐츠 속성 특성

`ContentPage` 태그의 루트 요소에 있는 `<ContentPage.Content>` 태그 또는 `StackLayout`의 자식을 포함하는 `<StackLayout.Children>` 태그와 같이 일부 속성 요소는 상당히 자주 사용됩니다.

모든 클래스가 해당 클래스에서 [`ContentPropertyAttribute`](xref:Xamarin.Forms.ContentPropertyAttribute)를 포함하는 하나의 속성을 식별할 수 있습니다. 이 속성의 경우에는 속성-요소 태그가 필요하지 않습니다. `ContentPage`는 해당 콘텐츠 속성을 `Content`로 정의하고, `Layout<T>`(`StackLayout`이 파생하는 클래스)는 해당 콘텐츠 속성을 `Children`으로 정의합니다. 이러한 속성 요소 태그는 필요하지 않습니다.

`Label`의 속성 요소는 `Text`입니다.

## <a name="formatted-text"></a>서식 있는 텍스트

[**TextVariations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) 샘플에는 `Label`의 `Text` 및 `FormattedText` 속성을 설정하는 여러 예가 포함되어 있습니다. XAML에서 `Span` 개체는 `FormattedString` 개체의 자식으로 표시됩니다.

 여러 줄 문자열이 `Text` 속성으로 설정된 경우에는 줄 끝 문자가 공백 문자로 변환되지만, 여러 줄 문자열이 `Label` 또는 `Label.Text` 태그로 표시된 경우 줄 끝 문자가 보존됩니다.

 [![텍스트 변형 공유의 세 가지 스크린샷](images/ch07fg03-small.png "서식이 지정된 텍스트 변형")](images/ch07fg03-large.png#lightbox "서식이 지정된 텍스트 변형")

## <a name="related-links"></a>관련 링크

- [7장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [7장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [7장 F# 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
- [XAML 기본 사항](~/xamarin-forms/xaml/xaml-basics/index.md)
