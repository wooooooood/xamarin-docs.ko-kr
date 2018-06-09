---
title: 7 장의 요약입니다. XAML 코드와 비교
description: 'Xamarin.Forms를 사용 하 여 모바일 응용 프로그램 만들기: 7 장 요약 합니다. XAML 코드와 비교'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: efa3a22c12983ef742bb46f91ab6096294cdc533
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241450"
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>7 장의 요약입니다. XAML 코드와 비교

Xamarin.Forms는 Extensible Application Markup Language 이라는 XML 기반 태그 언어 또는 XAML (발음이 "zammel")를 지원 합니다. XAML C# 대신 Xamarin.Forms는 응용 프로그램의 사용자 인터페이스의 레이아웃을 정의 및 사용자 인터페이스 요소 간의 바인딩을 정의 하 고 내부 데이터를 제공 합니다.

## <a name="properties-and-attributes"></a>속성 및 특성

Xamarin.Forms 클래스와 구조체는 XAML의 XML 요소 해지고 이러한 클래스와 구조체의 속성이 XML 특성을 됩니다. XAML에서 인스턴스화하기에 클래스는 공용 매개 변수 없는 생성자를 일반적으로 있어야 합니다. XAML에서 설정 된 속성을 공용 있어야 `set` 접근자입니다.

기본 데이터 형식의 속성에 대 한 (`string`, `double`, `bool`, 등), 표준을 사용 하 여 XAML 파서에 `TryParse` 특성 설정을 이러한 형식에 변환 하는 메서드. XAML 파서가 열거형 형식을도 쉽게 처리할 수 및 열거형 플래그가 지정 된 경우 열거형 멤버를 결합할 수는 `Flags` 특성입니다.

을 지원 하기 위해 XAML 파서에 더 복잡 한 유형 (또는 이러한 형식의 속성) 포함할 수는 [ `TypeConverterAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/) 에서 파생 되는 클래스를 식별 하는 [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) 지 원하는에서 변환 문자열 값을 해당 형식입니다. 예를 들어는 [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/) 변환에 이름 및 문자열 "#rrggbb" 등을 색 `Color` 값입니다.

## <a name="property-element-syntax"></a>속성 요소 구문

XAML에서 클래스와에서 만든 개체는 XML 요소로 표현 됩니다. 이 라고 *개체 요소*합니다. 이러한 개체의 속성을 대부분 XML 특성으로 표시 됩니다. 이 라고 *속성 특성*합니다.

경우에 따라 속성을 간단한 문자열로 표현할 수 없는 개체로 설정 되어야 합니다. 이 경우 XAML 지원 라고 하는 태그는 *속성 요소* 클래스 이름과 마침표로 구분 하는 속성 이름을 구성 하는 합니다. Object 요소는 한 쌍의 속성 요소 태그 내에서 다음 나타날 수 있습니다.

## <a name="adding-a-xaml-page-to-your-project"></a>프로젝트에 XAML 페이지를 추가합니다.

Xamarin.Forms 이식 가능한 클래스 라이브러리를 처음 만들 하거나 기존 프로젝트에 XAML 페이지를 추가할 수 있습니다 때 XAML 페이지를 포함할 수 있습니다. 새 항목 추가 대화 상자에서 XAML 페이지를 참조 하는 항목을 선택 또는 `ContentPage` 및 XAML 합니다. (하지는 `ContentView`.)

두 개의 파일이 생성 됩니다: 파일 이름 확장명.xaml와 XAML 파일 및 C# 파일 확장명이. xaml.cs 합니다. C# 파일은 라고도 *코드 숨김* XAML 파일의 합니다. 코드 숨김 파일에서 파생 되는 partial 클래스 정의 `ContentPage`합니다. 빌드 시 XAML 구문 분석 하 고 다른 partial 클래스 정의 동일한 클래스에 대 한 생성 됩니다. 라는 메서드를 포함 하는이 생성 된 클래스 `InitializeComponent` 하는 코드 숨김 파일의 생성자에서 호출 됩니다.

이 끝날 때 런타임에 `InitializeComponent` 인스턴스화되고 경우 C# 코드에서 만들어진 것 처럼 초기화 XAML 파일의 요소는 모두 호출 합니다.

XAML 파일에 루트 요소가 `ContentPage`합니다. 루트 태그 Xamarin.Forms 요소와는 다른 정의 대 한 두 개 이상의 XML 네임 스페이스 선언을 포함 한 `x` 요소와 모든 XAML 구현에 기본 특성에 대 한 접두사입니다. 루트 태그도 포함 되어 있는 `x:Class` 에서 파생 된 클래스의 이름과 네임 스페이스를 나타내는 특성 `ContentPage`합니다. 코드 숨김 파일에서 네임 스페이스 및 클래스 이름을 일치합니다.

XAML 및 코드의 조합 하 여 표시 됩니다는 [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) 샘플.

## <a name="the-xaml-compiler"></a>XAML 컴파일러

Xamarin.Forms는 XAML 컴파일러 이지만 사용은 선택 사항으로의 사용에 따라 한 [ `XamlCompilationAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)합니다. XAML 컴파일되지 않은 경우에 XAML 빌드 시 구문 분석 되 고 여기서도 구문 분석 되어야 런타임에 PCL에 포함 된 XAML 파일입니다. XAML를 컴파일할 경우 빌드 프로세스 이진 형식으로 XAML을 변환 하 고 보다 효율적으로 런타임 처리 됩니다.

## <a name="platform-specificity-in-the-xaml-file"></a>XAML 파일에서 특정 플랫폼

Xaml에서는 [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) 플랫폼별 태그를 선택 하 여 클래스를 사용할 수 있습니다. 제네릭 클래스 이므로으로 인스턴스화해야는 `x:TypeArguments` 대상 형식과 일치 하는 특성입니다. [ `OnIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnIdiom%3CT%3E/) 많지 클래스는 유사 하지만 사용 합니다.

사용 하는 `OnPlatform` 책 게시 된 후 변경 되었습니다. 원래 사용한 함께에서 라는 속성이 `iOS`, `Android`, 및 `WinPhone`합니다. 이제 자식 함께 사용 될 [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) 개체입니다. 설정의 [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) 속성의 public와 일치 하는 문자열을 `const` 의 필드는 [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) 클래스입니다. 설정의 [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Value/) 속성 값과 일치는 `x:TypeArguments` 특성에는 `OnPlatform` 태그입니다.

`OnPlatform` 에 설명 된는 [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) 거의 동일한 XAML의 블록을 포함 하기 때문에 이렇게 부름 샘플입니다. 반복적인이 태그의 존재 여부 기술을 줄이는 사용할 수 있어야 한다는 것을 제안 합니다.

## <a name="the-content-property-attributes"></a>콘텐츠 속성 특성

일부 속성 요소가 매우 자주 나타나는 같은 `<ContentPage.Content>` 태그의 루트 요소에는 `ContentPage`, 또는 `<StackLayout.Children>` 의 자식을 포함 하는 태그 `StackLayout`합니다.

모든 클래스와 하나의 속성을 식별 하도록 허용 된 한 [ `ContentPropertyAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPropertyAttribute/) 클래스에 있습니다. 이 속성에 속성 요소 태그 필요 하지 않습니다. `ContentPage` content 속성으로 정의 `Content`, 및 `Layout<T>` (클래스 `StackLayout` 파생)의 content 속성으로 정의 `Children`합니다. 이러한 속성 요소 태그 필요 하지 않습니다.

속성 요소 `Label` 은 `Text`합니다.

## <a name="formatted-text"></a>서식 있는 텍스트

[ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) 샘플 설정의 몇 가지 예제에는 `Text` 및 `FormattedText` 의 속성 `Label`합니다. Xaml에서는 `Span` 개체의 자식으로 나타나는 `FormattedString` 개체입니다.

 여러 줄 문자열으로 설정 된 경우는 `Text` 속성, 줄 끝 문자는 공백 문자 변환 되지만 여러 줄 문자열의 내용으로 표시 될 때 줄 끝 문자를 유지는 `Label` 또는 `Label.Text` 태그:

 [![공유 하는 텍스트 변형의 삼중 스크린 샷](images/ch07fg03-small.png "서식이 지정 된 텍스트 변형")](images/ch07fg03-large.png#lightbox "서식이 지정 된 텍스트 변형")



## <a name="related-links"></a>관련 링크

- [7 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [7 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [7 장 F # 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
