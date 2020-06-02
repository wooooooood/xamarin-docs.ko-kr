---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 25. Page varieties''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e66fb50b8d537ee0267457d5b0ab0f417813e676
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136619"
---
# <a name="summary-of-chapter-25-page-varieties"></a>요약 - 25장. 페이지 종류

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)

지금까지 `Page`에서 파생되는 두 가지 클래스인 `ContentPage` 및 `NavigationPage`에 대해 살펴봤습니다. 이 장에서는 다른 두 항목에 대해 설명합니다.

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)는 마스터 및 세부의 2개 페이지를 관리합니다.
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 탭을 통해 액세스되는 여러 자식 페이지를 관리합니다.

이러한 페이지 형식은 `NavagationPage`([24장. 페이지 탐색](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md) 설명 참조)보다 정교한 탐색 옵션을 제공합니다.

## <a name="master-and-detail"></a>마스터 및 세부

[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)는 `Page` 형식의 두 속성인 [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) 및 [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail)을 정의합니다. 일반적으로 이러한 각 속성을 `ContentPage`로 설정합니다. `MasterDetailPage`는 이러한 두 페이지를 표시하고 전환합니다.

이러한 두 페이지 간에 전환하는 기본 방법은 두 가지가 있습니다.

- *분할* - 마스터 및 세부가 나란히 배치됩니다.
- *팝오버* - 세부 페이지가 마스터 페이지의 일부 또는 전체를 덮습니다.

*팝오버* 접근 방식은 여러 변형(*슬라이드*, *겹침* 및 *교체*)이 있지만 이러한 접근 방식은 일반적으로 플랫폼에 따라 달라집니다. `MasterDetailPage`의 [`MasterDetailBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) 속성을 [`MasterBehavior`](xref:Xamarin.Forms.MasterBehavior) 열거형의 멤버로 설정할 수 있습니다.

- [`Default`](xref:Xamarin.Forms.MasterBehavior.Default)
- [`Split`](xref:Xamarin.Forms.MasterBehavior.Split)
- [`SplitOnLandscape`](xref:Xamarin.Forms.MasterBehavior.SplitOnLandscape)
- [`SplitOnPortrait`](xref:Xamarin.Forms.MasterBehavior.SplitOnPortrait)
- [`Popover`](xref:Xamarin.Forms.MasterBehavior.Popover)

하지만 이 속성은 휴대폰에 영향을 주지 않습니다. 휴대폰은 항상 팝오버 동작을 갖습니다. 태블릿 및 데스크톱 창만 분할 동작을 가질 수 있습니다.

### <a name="exploring-the-behaviors"></a>동작 탐색

[**MasterDetailBehaviors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) 샘플을 사용하면 여러 디바이스의 기본 동작을 실험해볼 수 있습니다. 이 프로그램에는 마스터 및 세부를 위한 두 가지 개별적인 `ContentPage` 파생 항목(두 항목 모두 `Title` 속성 설정)이 포함되며, 이를 결합하는 `MasterDetailPage`에서 파생되는 다른 클래스가 있습니다. 세부 페이지가 없으면 UWP 프로그램이 작동하지 않기 때문에 세부 페이지가 `NavigationPage`에 포함됩니다.

Windows 8.1 및 Windows Phone 8.1 플랫폼에서는 비트맵이 마스터 페이지의 `Icon` 속성으로 설정되어야 합니다.

### <a name="back-to-school"></a>학교로 돌아가기

[**SchoolAndDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) 샘플은 [**SchoolOfFineArt**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) 라이브러리에서 학생을 표시하기 위해 프로그램을 생성하는 다소 다른 접근 방법을 사용합니다.

`Master` 및 `Detail` 속성은 `MasterDetailPage`에서 파생되는 [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) 파일의 시각적 트리로 정의됩니다. 이 배열에서는 마스터 및 디테일 페이지 사이에 데이터 바인딩을 설정할 수 있습니다.

이 XAML 파일은 또한 `MasterDetailPage`의 [`IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented) 속성을 `True`로 설정합니다. 그러면 시작 시에 마스터 페이지가 표시됩니다. 기본적으로는 세부 페이지가 표시됩니다. [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) 파일은 마스터 페이지의 `ListView`에서 항목을 선택할 때 `IsPresented`를 `false`로 설정합니다. 그런 다음, 세부 페이지가 표시됩니다.

[![학교 및 세부의 세 가지 스크린샷](images/ch25fg09-small.png "MasterDetailPage의 세부 페이지")](images/ch25fg09-large.png#lightbox "MasterDetailPage의 세부 페이지")

### <a name="your-own-user-interface"></a>고유 사용자 인터페이스

Xamarin.Forms에서 마스터 및 세부 뷰 사이의 전환을 위한 사용자 인터페이스가 제공되더라도 사용자가 자신의 고유 인터페이스를 제공할 수 있습니다. 이를 수행하려면:

- [`IsGestureEnabled`](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabled) 속성을 `false`로 설정하여 살짝 밀기를 사용하지 않도록 설정합니다.
- [`ShouldShowToolbarButton`](xref:Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton) 메서드를 재정의하고 `false`를 반환하여 Windows 8.1 및 Windows Phone 8.1에서 도구 모음 단추를 숨깁니다.

그런 다음, [**ColorsDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) 샘플에 설명된 것처럼 마스터 및 세부 페이지 간의 전환 방법을 제공합니다.

[**MasterDetailTaps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) 샘플은 마스터 및 세부 페이지에서 `TapGestureRecognizer`를 사용한 또 다른 접근 방법을 보여줍니다.

## <a name="tabbedpage"></a>TabbedPage

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 탭 사용 간에 전환할 수 있는 페이지의 컬렉션입니다. 이 컬렉션은 `MultiPage<Page>`에서 파생되며 공용 속성 또는 자체 메서드를 정의하지 않습니다. 하지만 [`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1)는 속성을 정의합니다.

- `IList<T>` 형식의 [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) 속성

이 `Children` 컬렉션에 페이지 개체를 채웁니다.

다른 접근 방법을 사용하면 탭 페이지를 자동으로 생성하는 두 속성을 사용하는 `ListView`와 상당히 비슷하게 `TabbedPage` 자식 요소를 정의할 수 있습니다.

- [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource)(`IEnumerable` 형식)
- [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)(`DataTemplate` 형식)

하지만 이 접근 방법은 컬렉션에 항목이 여러 개 포함되었을 때 iOS에서 제대로 작동하지 않습니다.

`MultiPage<T>`는 현재 표시된 페이지를 추적할 수 있게 해주는 두 가지 속성을 더 정의합니다.

- 페이지를 참조하는 `T` 형식의 [`CurrentPage`](xref:Xamarin.Forms.MultiPage`1.CurrentPage)
- `ItemsSource` 컬렉션의 개체를 참조하는 `Object` 형식의 [`SelectedItem`](xref:Xamarin.Forms.MultiPage`1.SelectedItem)

`MultiPage<T>`는 또한 두 가지 이벤트를 정의합니다.

- [`PagesChanged`](xref:Xamarin.Forms.MultiPage`1.PagesChanged) - `ItemsSource` 컬렉션이 변경될 때의 이벤트
- [`CurrentPageChanged`](xref:Xamarin.Forms.MultiPage`1.CurrentPageChanged) - 표시된 페이지가 변경될 때의 이벤트

### <a name="discrete-tab-pages"></a>개별 탭 페이지

[**DiscreteTabbedColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) 샘플은 색을 세 가지 서로 다른 방식으로 표시하는 세 개의 탭 페이지로 구성됩니다. 각 탭은 `ContentPage` 파생 항목이며, `TabbedPage` 파생 [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml)은 3개 페이지를 결합합니다.

`TabbedPage`에 표시되는 각 페이지에 대해 `Title` 속성은 탭에 텍스트를 지정해야 하고, Apple Store에서는 `Icon` 속성이 iOS에 대해 설정되도록 아이콘도 사용해야 합니다.

[![개별 탭 색상의 세 가지 스크린샷](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

[**StudentNotes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) 샘플에는 모든 학생들을 나열하는 홈 페이지가 있습니다. 학생을 누르면 `TabbedPage` 파생 항목 [`StudentNotesDataPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml)로 이동되고, 여기에는 해당 시각적 트리에 3개의 `ContentPage` 개체가 표시됩니다. 이 중 하나는 해당 학생에 대한 일부 메모를 입력하도록 허용합니다.

### <a name="using-an-itemtemplate"></a>ItemTemplate 사용

[**MultiTabbedColor**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) 샘플은 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에서 [`NamedColor`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) 클래스를 사용합니다. [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) 파일은 `TabbedPage`의 `DataTemplate` 속성을 `ContentPage`로 시작하는 시각적 트리로 설정합니다. 여기에는 `NamedColor`의 속성에 대한 바인딩이 포함됩니다(`Title` 속성에 대한 바인딩 포함).

하지만 이것은 iOS에서 문제가 될 수 있습니다. 항목을 몇 개만 표시할 수 있으며, 아이콘을 제공하기 위한 좋은 방법이 없습니다.

## <a name="related-links"></a>관련 링크

- [25장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [25장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [마스터-세부 페이지](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [탭 페이지](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
