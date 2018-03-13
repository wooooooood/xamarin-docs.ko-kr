---
title: "25 장의 요약입니다. 페이지 변형"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 30642709519fc809d30da9a437728112f56a64d6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-25-page-varieties"></a>25 장의 요약입니다. 페이지 변형

두 개의 클래스에서 파생 되는 지금까지 살펴본 `Page`: `ContentPage` 및 `NavigationPage`합니다. 이 장에서 다른 두 소개:

- [`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 두 페이지, 마스터 및 세부 관리
- [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 탭을 통해 액세스 되는 여러 자식 페이지를 관리 합니다.

이러한 페이지 형식 보다 더 정교한 탐색 옵션을 제공할는 `NavagationPage` 에 설명 된 [장 24입니다. 페이지 탐색](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md)합니다.

## <a name="master-and-detail"></a>마스터 및 세부 정보

[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 형식의 두 가지 속성을 정의 `Page`: [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) 및 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/)합니다. 이러한 각 속성을 설정 하는 일반적으로 `ContentPage`합니다. `MasterDetailPage` 표시 하 고 이러한 두 페이지 간에 전환 합니다.

이러한 두 페이지 사이 전환 하려면 기본 두 가지가 있습니다.

- *분할* 마스터 및 세부 있는 병렬
- *popover* 있는 세부 정보 페이지 또는 담당 마스터에 일부 설명 페이지

여러 변형이 *popover* 접근 방식 (*슬라이드*, *겹칠*, 및 *스왑*),이 일반적으로 플랫폼 하지만 종속입니다. 설정할 수 있습니다는 [ `MasterDetailBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) 속성 `MasterDetailPage` 의 멤버에는 [ `MasterBehavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterBehavior/) 열거형:

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Default/)
- [`Split`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Split/)
- [`SplitOnLandscape`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnLandscape/)
- [`SplitOnPortrait`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnPortrait/)
- [`Popover`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Popover/)

그러나이 속성에 휴대폰에 영향을 주지 않습니다. 전화는 항상 popover 동작을 내포 합니다. 태블릿 및 바탕 화면 창 분리 동작이 있을 수 있습니다.

### <a name="exploring-the-behaviors"></a>탐색 동작

[ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) 샘플을 사용 하면 서로 다른 장치에서 기본 동작을 확인할 수 있습니다. 프로그램에 두 개의 별도 `ContentPage` 마스터 및 세부 정보에 대 한 파생 항목 (와 `Title` 둘 다에서 속성을 설정), 다른 클래스에서 파생 되 고 `MasterDetailPage` 결합 하 합니다. 세부 정보 페이지에 포함 되어는 `NavigationPage` 하므로 UWP 프로그램 없이 작동 하지 않습니다.

Windows 8.1 및 Windows Phone 8.1 플랫폼으로 설정 하는 비트맵 필요는 `Icon` 마스터 페이지의 속성입니다.

### <a name="back-to-school"></a>풍부한 참조 리소스

[ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) 샘플에서 학생을 표시 하도록 프로그램을 생성 하는 약간 다른 방식을 사용는 [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) 라이브러리입니다.

`Master` 및 `Detail` 속성은의 시각적 트리 함께 정의 [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) 파일에서 파생 된 `MasterDetailPage`합니다. 이 정렬을 마스터 및 세부 정보 페이지 사이 설정할 수에 대 한 데이터 바인딩을 사용 하면 됩니다.

XAML 파일을 설정 하는 [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) 속성 `MasterDetailPage` 를 `True`합니다. 이 인해 시작; 시 표시 될 마스터 페이지 기본적으로 세부 정보 페이지 표시 됩니다. [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) 파일 집합 `IsPresented` 를 `false` 에서 항목을 선택 하는 경우는 `ListView` 마스터 페이지에 있습니다. 세부 정보 페이지 표시 됩니다.

[![학교 및 세부 삼중 스크린 샷](images/ch25fg09-small.png "는 MasterDetailPage에서 세부 정보 페이지")](images/ch25fg09-large.png#lightbox "는 MasterDetailPage에서 세부 정보 페이지")

### <a name="your-own-user-interface"></a>사용자 고유의 사용자 인터페이스

Xamarin.Forms를 마스터 및 세부 정보 뷰 사이 전환 하기 위한 사용자 인터페이스를 제공 하지만 있습니다를 직접 제공할 수 있습니다. 이렇게 하려면

- 설정의 [ `IsGestureEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsGestureEnabled/) 속성을 `false` 넘기기가 사용 하지 않으려면
- 재정의 [ `ShouldShowToolbarButton` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton()/) 메서드 및 반환 `false` Windows 8.1 및 Windows Phone 8.1에서 도구 모음 단추를 숨기려면 합니다.

와 같은 마스터 및 세부 정보 페이지 사이 전환할 수 있는 방법을 제공 해야는 [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) 샘플.

[ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) 샘플 사용 하 여 다른 접근 방식을 보여 줍니다.는 `TapGestureRecognizer` 마스터 및 세부 정보 페이지에 있습니다.

## <a name="tabbedpage"></a>TabbedPage

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 는 탭을 사용 하 여 사이에서 전환할 수 있는 페이지의 컬렉션입니다. 파생 `MultiPage<Page>` 없는 공용 속성 또는 자체의 메서드를 정의 합니다. [`MultiPage<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/)그러나 속성 정의지 않습니다.

- [`Children`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) 형식의 속성 `IList<T>`

이 정보를 입력 `Children` 페이지 개체를 사용 하 여 컬렉션입니다.

또 다른 방법은 정의할 수 있습니다는 `TabbedPage` 자식과 거의 동일한는 `ListView` 탭된 페이지를 자동으로 생성 하는 이러한 두 속성을 사용 하 여:

- [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) 형식의 `IEnumerable`
- [`ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) 형식의 `DataTemplate`

그러나이 방법은 컬렉션에 여러 항목이 포함 된 경우 iOS에서 제대로 작동 하지 않습니다.

`MultiPage<T>` 추적할 수 있는 페이지를 현재 보고 하는 데 사용할 수 있는 두 개 더 많은 속성을 정의 합니다.

- [`CurrentPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.CurrentPage/) 형식의 `T`참조 하는 페이지
- [`SelectedItem`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.SelectedItem/) 형식의 `Object`의 개체 참조는 `ItemsSource` 컬렉션

`MultiPage<T>` 또한 두 개의 이벤트를 정의합니다.

- [`PagesChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.PagesChanged/) 경우는 `ItemsSource` 컬렉션 변경
- [`CurrentPageChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.CurrentPageChanged/) 표시 된 페이지가 변경 될 때

### <a name="discrete-tab-pages"></a>개별 탭 페이지

[ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) 샘플은 세 가지 방법으로 색을 표시 하는 세 개의 탭된 페이지의 구성 됩니다. 각 탭은는 `ContentPage` 파생 개체를 선택한 다음은 `TabbedPage` 파생 클래스 [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) 세 페이지를 결합 합니다.

에 나타나는 각 페이지에 대 한 `TabbedPage`, `Title` 속성 탭에서 텍스트를 지정 하는 데 필요 하 고 Apple 스토어 아이콘도 사용 해야 하므로 `Icon` iOS에 대 한 속성이 설정 되어:

[![개별 탭 색의 삼중 스크린 샷](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

[ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) 샘플 모든 학생을 나열 하는 홈 페이지에 있습니다. 학생의 탭이 수행 되는 경우이를 탐색 한 `TabbedPage` 파생 개체를 [ `StudentNotesDataPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), 3 개를 통합 하는 `ContentPage` 해당 학생에 대 한 몇 가지 정보를 입력할 수 중 하나는 해당 시각적 트리의 개체입니다.

### <a name="using-an-itemtemplate"></a>에 ItemTemplate을 사용 하 여

[ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) 샘플에서는 [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다. [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) 파일 집합의 `DataTemplate` 속성의 `TabbedPage` 문자로 시작 하는 시각적 트리를 `ContentPage` 의 속성에 대 한 바인딩을 포함 하 `NamedColor` ( 는에대한바인딩을포함합니다.`Title` 속성).

그러나 이것은 iOS에 문제가 있습니다. 몇 가지 항목을 표시할 수 있는 되며 좋은 아이콘을 부여할 수 없습니다.



## <a name="related-links"></a>관련 링크

- [25 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [25 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [마스터-세부 페이지](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [탭된 페이지](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
