---
title: Xamarin.Forms 페이지
description: Xamarin.Forms 페이지는 플랫폼 간 모바일 응용 프로그램 화면을 나타냅니다. 이 문서에서는 Xamarin.Forms에 포함 된 페이지를 나열 합니다.
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: fea1db6c65e533692601439f4712d15371740644
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995900"
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms 페이지

_Xamarin.Forms 페이지는 플랫폼 간 모바일 응용 프로그램 화면을 나타냅니다._

Xamarin.Forms에서 아래에 나와 있는 모든 페이지 유형 파생 [ `Page` ](xref:Xamarin.Forms.Page) 클래스입니다. 이러한 시각적 요소는 모든 또는 대부분의 화면을 차지합니다. A `Page` 개체가 나타내는 `ViewController` ios에서 및 `Page` 유니버설 Windows 플랫폼에서 합니다. Android에서 각 페이지와 같은 화면을 차지를 `Activity`, Xamarin.Forms 페이지 되지만 *없습니다* `Activity` 개체입니다.

[ ![](pages-images/pages-sml.png "Xamarin.Forms 페이지 형식")](pages-images/pages.png#lightbox "Xamarin.Forms 페이지 형식")

## <a name="pages"></a>인쇄할 페이지

Xamarin.Forms는 페이지 형식을 지원 합니다.

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage) 페이지의 간단 하 고 가장 일반적인 형식이입니다. 설정 합니다 [ `Content` ](xref:Xamarin.Forms.ContentPage.Content) 속성을 단일 [ `View` ](views.md) 대부분 인 개체를 [ `Layout` ](layouts.md) 와 같은 [ `StackLayout` ](layouts.md#stackLayout)하십시오 [ `Grid` ](layouts.md#grid), 또는 [ `ScrollView` ](layouts.md#scrollView)합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.ContentPage) | [![ContentPage 예제](pages-images/ContentPage.png "ContentPage 예제")](pages-images/ContentPage-Large.png#lightbox "ContentPage 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 정보의 두 창을 관리 합니다. 설정 된 [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) 일반적으로 목록 또는 메뉴를 보여 주는 페이지로 속성입니다. 설정 된 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) 마스터 페이지에서 선택한 항목을 보여 주는 페이지로 속성입니다. 합니다 [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) 속성의 마스터 / 세부 정보 페이지 표시 되는지 여부를 제어 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.MasterDetailPage) / [가이드](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![MasterDetailPage 예제](pages-images/MasterDetailPage.png "MasterDetailPage 예제")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) 사용 하 여 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| 합니다 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 스택 기반 아키텍처를 사용 하 여 다른 페이지 간 탐색을 관리 합니다. 생성자에 홈 페이지의 인스턴스를 전달 해야 응용 프로그램에서 페이지 탐색 사용 하는 경우는 `NavigationPage` 개체입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.NavigationPage) / [가이드](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [1 샘플](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)하십시오 [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/), 및 [3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![NavigationPage 예제](pages-images/NavigationPage.png "NavigationPage 예제")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) 사용 하 여 [코드 숨김 =](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 에서 파생 [ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1) 클래스 및 자식 간 탐색 탭을 사용 하 여 페이지를 허용 합니다. 설정 된 [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children) 속성 페이지 또는 집합의 컬렉션을를 [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 속성을 데이터 개체의 컬렉션 및 [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 속성을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 각 개체가 시각적으로 나타낼 수 하는 방법을 설명 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.TabbedPage) / [가이드](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [1 샘플](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/) 고 [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![TabbedPage 예제](pages-images/TabbedPage.png "TabbedPage 예제")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) 에서 파생 [ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1) 클래스 및 자식 간 탐색 손가락 살짝를 통해 페이지를 허용 합니다. 설정 된 [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children) 속성의 컬렉션을 [ `ContentPage` ](#contentPage) 개체나 집합 합니다 [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 속성 데이터 개체의 컬렉션을 및 [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 속성을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 각 개체가 시각적으로 나타낼 수 하는 방법을 설명 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.CarouselPage) / [가이드](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [1 샘플](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/) 고 [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![CarouselPage 예제](pages-images/CarouselPage.png "CarouselPage 예제")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) 컨트롤 템플릿 사용 하 여 전체 화면 콘텐츠를 표시 하 고에 대 한 기본 클래스인 [ `ContentPage` ](#contentPage)합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.TemplatedPage) / [가이드](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![TemplatedPage 예제](pages-images/TemplatedPage.png "TemplatedPage 예제")](pages-images/TemplatedPage.png "TemplatedPage 예제") |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery 샘플](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API 설명서](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
