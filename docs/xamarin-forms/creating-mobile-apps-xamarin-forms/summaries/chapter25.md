---
title: 요약 25 장입니다. 페이지 종류
description: Xamarin.Forms를 사용 하 여 모바일 앱을 만듭니다. 요약 25 장입니다. 페이지 종류
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: db6c329c029f52180fe508f277a1cf4834ab493a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61331825"
---
# <a name="summary-of-chapter-25-page-varieties"></a>요약 25 장입니다. 페이지 종류

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)

두 개의 클래스에서 파생 되는 지금까지 살펴본 `Page`: `ContentPage` 고 `NavigationPage`입니다. 이 단원은 다른 두:

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 두 페이지, 마스터 및 세부 정보를 관리합니다.
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 탭을 통해 액세스 되는 여러 자식 페이지를 관리 합니다.

이러한 페이지 형식 보다 더 복잡 한 탐색 옵션을 제공 합니다 `NavagationPage` 에 설명 된 [24 장입니다. 탐색 페이지](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md)합니다.

## <a name="master-and-detail"></a>마스터 및 세부 정보

합니다 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 형식의 두 가지 속성을 정의 `Page`: [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) 하 고 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail)합니다. 이러한 각 속성을 설정 하는 일반적으로 `ContentPage`합니다. `MasterDetailPage` 표시 하 고 이러한 두 페이지 사이 전환 합니다.

이러한 두 페이지 사이 전환 하는 기본적인 방법은 두 가지

- *분할* 마스터 및 세부 있는 나란히
- *팝 오버* 세부 정보 페이지의 설명 하거나 마스터를 부분적으로 검사 위치 페이지

여러 변형이 있습니다 합니다 *팝 오버* 접근 방식 (*슬라이드*, *겹치는*, 및 *스왑*), 하지만이 일반적으로 플랫폼 종속입니다. 설정할 수 있습니다 합니다 [ `MasterDetailBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) 속성을 `MasterDetailPage` 의 멤버에는 [ `MasterBehavior` ](xref:Xamarin.Forms.MasterBehavior) 열거형:

- [`Default`](xref:Xamarin.Forms.MasterBehavior.Default)
- [`Split`](xref:Xamarin.Forms.MasterBehavior.Split)
- [`SplitOnLandscape`](xref:Xamarin.Forms.MasterBehavior.SplitOnLandscape)
- [`SplitOnPortrait`](xref:Xamarin.Forms.MasterBehavior.SplitOnPortrait)
- [`Popover`](xref:Xamarin.Forms.MasterBehavior.Popover)

그러나이 속성은 휴대폰에 영향을 주지 않습니다. 휴대폰에는 항상 팝 오버 동작을 가집니다. 태블릿 및 데스크톱 windows 분할 동작을 가질 수 있습니다.

### <a name="exploring-the-behaviors"></a>탐색 동작

합니다 [ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) 샘플을 사용 하면 다양 한 장치에 대 한 기본 동작으로 실험해 보세요. 두 개의 별도 프로그램에 포함 되어 `ContentPage` 마스터 및 세부 정보에 대 한 파생형 (사용 하 여는 `Title` 둘 다에서 속성을 설정), 및 다른 클래스에서 파생 되는 `MasterDetailPage` 결합 하는 합니다. 세부 정보 페이지에 포함 되어는 `NavigationPage` UWP 프로그램 없으면 작동 하지 않습니다.

Windows 8.1 및 Windows Phone 8.1 플랫폼을 비트맵으로 설정 하는 요구는 `Icon` 마스터 페이지의 속성입니다.

### <a name="back-to-school"></a>풍부한 참조 리소스

합니다 [ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) 샘플의 학생 들을 표시 하도록 프로그램을 생성 하는 약간 다른 방식을 사용 합니다 [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) 라이브러리입니다.

`Master` 및 `Detail` 의 시각적 트리를 사용 하 여 정의 된 속성을 [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) 에서 파생 되는 파일 `MasterDetailPage`. 이 배열에는 마스터 및 세부 정보 페이지 사이 설정할 수에 대 한 데이터 바인딩을 허용 합니다.

XAML 파일 설정 하는 합니다 [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) 속성을 `MasterDetailPage` 에 `True`입니다. 이 인해 시작; 표시할 마스터 페이지 기본적으로 세부 정보 페이지 표시 됩니다. 합니다 [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) 파일 집합 `IsPresented` 하 `false` 에서 항목을 선택 하는 경우는 `ListView` 마스터 페이지에서. 세부 정보 페이지가 표시 됩니다.

[![학교 및 정보의 세 번 스크린 샷](images/ch25fg09-small.png "는 MasterDetailPage에서 세부 정보 페이지")](images/ch25fg09-large.png#lightbox "를 MasterDetailPage에서 세부 정보 페이지")

### <a name="your-own-user-interface"></a>고유한 사용자 인터페이스

Xamarin.Forms는 마스터 / 세부 뷰 간의 전환에 대 한 사용자 인터페이스를 제공 하지만 제공할 수 있습니다 자체. 이를 수행하려면:

- 설정 된 [ `IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabled) 속성을 `false` 살짝 사용 하지 않도록 설정
- 재정의 된 [ `ShouldShowToolbarButton` ](xref:Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton) 메서드와 반환 `false` Windows 8.1 및 Windows Phone 8.1에서 도구 모음 단추를 숨기려면 합니다.

나타난 것 처럼 같은 마스터 및 세부 정보 페이지 사이 전환할 수 있는 방법을 제공 해야 합니다 [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) 샘플입니다.

[ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) 샘플 다른 접근 방식을 사용 하는 방법을 보여 줍니다는 `TapGestureRecognizer` 마스터 및 세부 정보 페이지에 있습니다.

## <a name="tabbedpage"></a>TabbedPage

합니다 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 컬렉션인 페이지 탭을 사용 하 여 전환할 수 있습니다. 파생 되므로 `MultiPage<Page>` 고유한 메서드가 없거나 공용 속성을 정의 합니다. [`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1)에 속성을 정의 하는 반면:

- [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) 형식의 속성 `IList<T>`

이 입력 `Children` 페이지 개체를 사용 하 여 컬렉션입니다.

또 다른 방법은 정의할 수 있습니다 합니다 `TabbedPage` 자식을 마찬가지로 `ListView` 탭된 페이지를 자동으로 생성 하는 이러한 두 속성을 사용 하 여:

- [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 형식의 `IEnumerable`
- [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 형식의 `DataTemplate`

그러나이 방법은 컬렉션에 몇 개의 항목을 포함 하는 경우 iOS에서 제대로 작동 하지 않습니다.

`MultiPage<T>` 추적 하는 페이지를 현재 볼 수 있는 두 개 더 많은 속성을 정의 합니다.

- [`CurrentPage`](xref:Xamarin.Forms.MultiPage`1.CurrentPage) 형식의 `T`페이지 참조
- [`SelectedItem`](xref:Xamarin.Forms.MultiPage`1.SelectedItem) 형식의 `Object`에 있는 개체 참조는 `ItemsSource` 컬렉션

`MultiPage<T>` 또한 두 개의 이벤트를 정의합니다.

- [`PagesChanged`](xref:Xamarin.Forms.MultiPage`1.PagesChanged) 경우는 `ItemsSource` 컬렉션 변경
- [`CurrentPageChanged`](xref:Xamarin.Forms.MultiPage`1.CurrentPageChanged) 표시 된 페이지를 변경 하는 경우

### <a name="discrete-tab-pages"></a>개별 탭 페이지

합니다 [ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) 샘플은 세 가지 방법으로 색을 표시 하는 세 탭 페이지로 구성 됩니다. 각 탭은는 `ContentPage` 파생 개체를 차례로 합니다 `TabbedPage` 파생 [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) 세 페이지를 결합 합니다.

에 나타나는 각 페이지에 대 한는 `TabbedPage`는 `Title` 속성 탭에서 텍스트를 지정 하는 데 필요 하 고 Apple 스토어 아이콘도 사용 해야 하므로 `Icon` iOS에 대 한 속성:

[![개별 탭 색의 3 배가 스크린 샷](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

합니다 [ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) 샘플에는 모든 학생 들을 나열 하는 홈 페이지입니다. 로 이동 학생 변하는데를 `TabbedPage` 파생 개체를 [ `StudentNotesDataPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), 세 가지를 통합 하는 `ContentPage` 해당 시각적에서 개체는 학생에 대 한 일부 정보를 입력할 수 있습니다 하는 중입니다.

### <a name="using-an-itemtemplate"></a>ItemTemplate을 사용 하 여

합니다 [ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) 샘플에서는 합니다 [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) 클래스의 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다. [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) 파일 집합을 `DataTemplate` 속성을 `TabbedPage` 로 시작 하는 시각적 트리를 `ContentPage` 의 속성에 대 한 바인딩을 포함 하는 `NamedColor` (합니다 에대한바인딩을포함합니다.`Title` 속성).

그러나 iOS의 경우 문제가 됩니다. 항목 중 일부만 표시할 수 있습니다 이며 아이콘 제공 방법이 없습니다.



## <a name="related-links"></a>관련 링크

- [25 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [25 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [마스터-세부 페이지](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [탭된 페이지](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
