---
title: 요약 7 장입니다. 코드 및 XAML
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 7 장 요약 합니다. 코드 및 XAML'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 1dc4fe12d3ca23a9ca87c3be7819c970683db469
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563500"
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>요약 7 장입니다. 코드 및 XAML

> [!NOTE] 
> 이 페이지에 대 한 참고 사항 Xamarin.Forms 책의 내용을에서 달라졌는지를 위치 하는 영역을 나타냅니다.

Xamarin.Forms는 XML 기반 태그 언어인 Extensible Application Markup Language 또는 XAML (자 멜 "")를 지원 합니다. XAML 대신 C# Xamarin.Forms 응용 프로그램의 사용자 인터페이스의 레이아웃을 정의 및 사용자 인터페이스 요소 간의 바인딩을 정의 하 고 기본 데이터를 제공 합니다.

## <a name="properties-and-attributes"></a>속성 및 특성

Xamarin.Forms 클래스와 구조체는 XAML에서의 XML 요소가 됩니다 및 이러한 클래스 및 구조체의 속성에는 XML 특성 있게 됩니다. XAML에서 인스턴스화할 클래스 매개 변수가 없는 public 생성자를 일반적으로 있어야 합니다. XAML에서 설정 된 속성은 공용 있어야 합니다. `set` 접근자입니다.

기본 데이터 형식의 속성에 대 한 (`string`, `double`를 `bool`등), XAML 파서를 사용 하 여 표준 `TryParse` 특성 설정을 이러한 형식으로 변환 하는 방법입니다. XAML 구문 분석기에는 열거형 형식도 쉽게 처리할 수 있습니다 및 열거형 형식으로 플래그가 지정 되어 있으면 열거형 멤버 결합할 수 있습니다는 `Flags` 특성입니다.

XAML 파서를 지원 하기 위해 보다 복잡 한 형식 (또는 이러한 형식의 속성) 포함할 수는 [ `TypeConverterAttribute` ](xref:Xamarin.Forms.TypeConverterAttribute) 에서 파생 된 클래스를 식별 하는 [ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter) 변환할 지 해당 형식에 문자열 값입니다. 예를 들어 합니다 [ `ColorTypeConverter` ](xref:Xamarin.Forms.ColorTypeConverter) 변환에 이름 및 "#rrggbb"와 같은 문자열 색 `Color` 값입니다.

## <a name="property-element-syntax"></a>속성 요소 구문

XAML, 클래스 및에서 생성 되는 개체는 XML 요소로 표현 됩니다. 이러한 라고 *요소를 개체*합니다. 이러한 개체의 속성 대부분은 XML 특성으로 표현 됩니다. 이러한 이라고 *속성 특성*합니다.

경우에 따라 속성을 간단한 문자열로 표현할 수 없는 개체에 설정 되어야 합니다. 이 경우 XAML 지원 이라는 태그를 *property 요소* 클래스 이름 및 속성 이름의 마침표로 구분 하 여 구성 된 합니다. 개체 요소나 속성 요소 태그 쌍 내에서 다음 나타날 수 있습니다.

## <a name="adding-a-xaml-page-to-your-project"></a>프로젝트에 XAML 페이지 추가

Xamarin.Forms 이식 가능한 클래스 라이브러리를 기존 프로젝트에 XAML 페이지를 추가할 수 있습니다 또는 처음 만들어질 때 XAML 페이지를 포함할 수 있습니다. 새 항목을 추가 하려면 대화 상자에서 XAML 페이지를 가리키는 항목을 선택 또는 `ContentPage` 및 XAML입니다. (하지는 `ContentView`.)

> [!NOTE] 
> Visual Studio 옵션은이 챕터에서 작성 된 이후 변경 되었습니다.

두 개의 파일이 만들어집니다: XAML 파일을 파일 이름 확장명이.xaml 및 C# 파일 확장명이. xaml.cs. C# 파일은 라고도 합니다 *코드 숨김* XAML 파일의 합니다. 코드 숨김 파일에서 파생 되는 partial 클래스 정의 `ContentPage`합니다. 빌드 시에는 XAML 구문 분석 하 고 다른 partial 클래스 정의 동일한 클래스에 대 한 생성 됩니다. 이렇게 생성 된 클래스 라는 메서드가 포함 되어 `InitializeComponent` 코드 숨김 파일의 생성자에서 호출 되는 합니다.

끝날 때 런타임에 `InitializeComponent` 호출, 모든 XAML 파일의 요소는 인스턴스화되고 것 처럼 C# 코드에서 만들어진 것을 초기화 합니다.

XAML 파일의 루트 요소는 `ContentPage`합니다. 루트 태그에는 Xamarin.Forms 요소 및 다른 정의 대 한 두 개 이상의 XML 네임 스페이스 선언이 포함 되어 있습니다.는 `x` 요소 및 특성의 모든 XAML 구현에 내장 함수에 대 한 접두사입니다. 루트 태그 포함 되어는 `x:Class` 에서 파생 된 클래스의 이름과 네임 스페이스를 나타내는 특성 `ContentPage`합니다. 이 코드 숨김 파일에서 네임 스페이스 및 클래스 이름과 일치 합니다.

XAML 및 코드의 조합에서 보여 주는 것은 [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) 샘플입니다.

## <a name="the-xaml-compiler"></a>XAML 컴파일러

Xamarin.Forms XAML 컴파일러를 갖지만 사용에 따라 해당 사용은 선택 사항를 [ `XamlCompilationAttribute` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)합니다. XAML을 컴파일되지 않은 경우 빌드 시에는 XAML 구문 분석 및 PCL을 여기서도 구문을 분석 하는 런타임 시에 XAML 파일을 포함 됩니다. XAML, 컴파일된 경우 빌드 프로세스 이진 형식으로 변환 된 XAML 및 런타임 처리 프로세스를 더 효율적입니다.

## <a name="platform-specificity-in-the-xaml-file"></a>XAML 파일의 플랫폼 특이성

XAML에 [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) 플랫폼에 종속 된 태그를 선택 하는 클래스를 사용할 수 있습니다. 제네릭 클래스 이며 인스턴스화할 해야는 `x:TypeArguments` 대상 형식과 일치 하는 특성입니다. 합니다 [ `OnIdiom` ](xref:Xamarin.Forms.OnIdiom`1) 클래스와 유사 하지만 사용 되는 많지 않습니다.

사용 `OnPlatform` 출판 된 이후 변경 되었습니다. 명명 된 속성을 사용 하 여 함께에서 원래 사용 된 것 `iOS`, `Android`, 및 `WinPhone`합니다. 자식 사용 하는 이제 [ `On` ](xref:Xamarin.Forms.On) 개체입니다. 설정 합니다 [ `Platform` ](xref:Xamarin.Forms.On.Platform) 공용와 일치 하는 문자열 속성 `const` 의 필드를 [ `Device` ](xref:Xamarin.Forms.Device) 클래스입니다. 설정 합니다 [ `Value` ](xref:Xamarin.Forms.On.Value) 속성 값과 일치 합니다 `x:TypeArguments` 특성은 `OnPlatform` 태그.

`OnPlatform` 에 설명 되어는 [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) 샘플에서는 소위 거의 동일한 XAML의 요소를 포함 하기 때문에 있습니다. 이 반복적으로 태그의 존재 여부 기술을 줄이는 것을 사용할 수 있어야 한다는 것을 제안 합니다.

## <a name="the-content-property-attributes"></a>콘텐츠 속성 특성

일부 속성 요소를 매우 빈번 하 게 발생 같은 합니다 `<ContentPage.Content>` 태그의 루트 요소에는 `ContentPage`, 또는 `<StackLayout.Children>` 태그의 자식을 포함 하는 `StackLayout`합니다.

모든 클래스는 하나의 속성을 식별 하려면 사용할 수는 [ `ContentPropertyAttribute` ](xref:Xamarin.Forms.ContentPropertyAttribute) 클래스에 있습니다. 이 속성에 대 한 속성 요소 태그 필요 하지 않습니다. `ContentPage` 해당 콘텐츠 속성으로 정의 `Content`, 및 `Layout<T>` (클래스 `StackLayout` 파생)의 content 속성으로 정의 `Children`합니다. 이러한 속성 요소 태그 필요 하지 않습니다.

속성 요소의 `Label` 는 `Text`합니다.

## <a name="formatted-text"></a>서식 있는 텍스트

[ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) 설정의 몇 가지 예제를 포함 하는 샘플 합니다 `Text` 및 `FormattedText` 의 속성 `Label`합니다. XAML을 `Span` 개체의 자식으로 표시 된 `FormattedString` 개체입니다.

 여러 줄 문자열 설정 된 경우는 `Text` 속성인 줄 끝 문자는 공백 문자 변환 되지만 줄 끝 문자를 여러 줄 문자열의 내용으로 표시 될 때 유지 됩니다 합니다 `Label` 또는 `Label.Text` 태그:

 [![공유 텍스트 변형의 삼중 스크린 샷](images/ch07fg03-small.png "서식이 지정 된 텍스트 변형")](images/ch07fg03-large.png#lightbox "텍스트 변형 형식")

## <a name="related-links"></a>관련 링크

- [7 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [7 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [7 장 F# 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
- [XAML 기본 사항](~/xamarin-forms/xaml/xaml-basics/index.md)