---
title: 요약 - 8장. 코드와 XAML의 조화
description: 'Xamarin.Forms로 모바일 앱 만들기: 요약 - 8장. 코드와 XAML의 조화'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 75f153499edb6d979f9a0269a1439eaf8ca53878
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "61334244"
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>요약 - 8장. 코드와 XAML의 조화

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)

이 장에서는 XAML을 보다 깊이 살펴보고 코드와 XAML이 상호 작용하는 방식을 중점적으로 설명합니다.

## <a name="passing-arguments"></a>인수 전달

일반적인 경우 XAML에서 인스턴스화된 클래스에는 매개 변수가 없는 퍼블릭 생성자가 있어야 합니다. 결과 개체는 속성 설정을 통해 초기화됩니다. 그러나 두 가지 다른 방법으로 개체를 인스턴스화하고 초기화할 수 있습니다.

이러한 방법은 범용 기술이지만 일반적으로 MVVM View Model에 연결하는 데 사용됩니다.

### <a name="constructors-with-arguments"></a>인수가 포함된 생성자

[**ParameteredConstructorDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) 샘플에서는 `x:Arguments` 태그를 사용하여 생성자 인수를 지정하는 방법을 보여줍니다. 이러한 인수는 인수의 형식을 나타내는 요소 태그로 구분되어야 합니다. 기본 .NET 데이터 형식의 경우 다음 태그를 사용할 수 있습니다.

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

### <a name="can-i-call-methods-from-xaml"></a>XAML에서 메서드를 호출할 수 있나요?

[**FactoryMethodDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) 샘플에서는 `x:FactoryMethod` 요소를 사용하여 개체를 만들 때 호출되는 팩터리 메서드를 지정하는 방법을 보여줍니다. 이러한 팩터리 메서드는 공개적이고 고정적이어야 하며 정의된 형식의 개체를 만들어야 합니다. 예를 들어 [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) 메서드는 공개적이고 고정적이며 `Color` 형식의 값을 반환하므로 팩터리 메서드가 될 자격이 있습니다. 팩터리 메서드에 대한 인수는 `x:Arguments` 태그 내에 지정됩니다.

## <a name="the-xname-attribute"></a>x:Name 특성

`x:Name` 특성을 사용하면 XAML에서 인스턴스화된 개체에 이름을 지정할 수 있습니다. 이러한 이름에 대한 규칙은 C# 변수 이름과 동일합니다. 생성자에서 `InitializeComponent` 호출을 반환하고 나면 코드 숨김 파일이 이러한 이름을 참조하여 해당 XAML 요소에 액세스할 수 있습니다. 이름은 실제로 XAML 파서를 통해 생성된 partial 클래스의 프라이빗 필드로 변환됩니다.

[**XamlClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) 샘플에서는 `x:Name`을 사용하여 코드 숨김 파일이 XAML에서 정의된 두 개의 `Label` 요소를 현재 날짜 및 시간으로 업데이트된 상태로 유지할 수 있도록 하는 방법을 보여줍니다.

같은 페이지의 여러 요소에 동일한 이름을 사용할 수 없습니다. 특히 `OnPlatform`을 사용하여 각 플랫폼에 동일하게 명명된 개체를 만드는 경우가 문제입니다. [**PlatformSpecificLabele**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) 샘플에서는 이와 같은 작업을 수행하는 더 좋은 방법을 보여줍니다.

## <a name="custom-xaml-based-views"></a>사용자 지정 XAML 기반 뷰

XAML에서 태그의 반복을 방지하는 여러 가지 방법이 있습니다. 일반적인 기법 중 하나는 [`ContentView`](xref:Xamarin.Forms.ContentView)에서 파생되는 새로운 XAML 기반 클래스를 만드는 것입니다. 이 기법은 [**ColorViewList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) 샘플에서 설명합니다. `ColorView` 클래스는 `ContentView`에서 파생되어 특정 색 및 해당 이름을 표시하는 반면 `ColorViewListPage` 클래스는 일반적인 방법으로 `ContentPage`에서 파생되고 명시적으로 17개의 `ColorView` 인스턴스를 만듭니다.

XAML에서 `ColorView` 클래스에 액세스하려면 동일한 어셈블리의 클래스에 대한 다른 XML 네임스페이스 선언(일반적으로 `local`로 명명됨)이 필요합니다.

## <a name="events-and-handlers"></a>이벤트 및 처리기

이벤트는 XAML에서 이벤트 처리기에 할당될 수 있지만 이벤트 처리기 자체는 코드 숨김 파일에 구현되어야 합니다. [**XamlKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad)는 XAML에서 키패드 사용자 인터페이스를 빌드하는 방법과 코드 숨김 파일에서 `Clicked` 처리기를 구현하는 방법을 보여줍니다.

## <a name="tap-gestures"></a>탭 제스처

모든 `View` 개체는 터치 입력을 가져오고 해당 입력에서 이벤트를 생성할 수 있습니다. `View` 클래스는 [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer)에서 파생되는 클래스의 인스턴스를 하나 이상 포함할 수 있는 [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) 컬렉션 속성을 정의합니다.

[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)는 [`Tapped`](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) 이벤트를 생성합니다. [**MonkeyTap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) 프로그램은 `TapGestureRecognizer` 개체를 4개의 `BoxView` 요소에 연결하여 모조 게임을 만드는 방법을 보여줍니다.

[![MonkeyTap의 삼중 스크린샷](images/ch08fg07-small.png "모조 게임")](images/ch08fg07-large.png#lightbox "모조 게임")

그러나 **MonkeyTap** 프로그램에는 소리가 필요합니다. ([다음 장](chapter09.md)을 참조하세요.)

## <a name="related-links"></a>관련 링크

- [8장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [8장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [8장 F# 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
- [XAML에서 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)
