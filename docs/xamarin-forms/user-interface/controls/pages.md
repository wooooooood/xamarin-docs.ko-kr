---
title: Xamarin.Forms Pages
description: "Xamarin.Forms 페이지 플랫폼 간 모바일 응용 프로그램 화면을 나타냅니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 5f979d2dbb894107d8d606ec1f41de44c294cdd3
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2018
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms Pages

_Xamarin.Forms 페이지 플랫폼 간 모바일 응용 프로그램 화면을 나타냅니다._

Xamarin.Forms는에서 파생 되는 아래 설명 된 모든 페이지 유형 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 클래스입니다. 이러한 시각적 요소 화면의 전체 또는 대부분을 차지합니다. A `Page` 개체가 나타내는 `ViewController` iOS에서 및 `Page` 유니버설 Windows 플랫폼에 있습니다. Android에서는 각 페이지와 같은 화면을 차지는 `Activity`, 있지만 Xamarin.Forms 페이지는 *하지* `Activity` 개체입니다.

[ ![](pages-images/pages-sml.png "Xamarin.Forms 페이지 형식")](pages-images/pages.png#lightbox "Xamarin.Forms 페이지 유형")

## <a name="pages"></a>인쇄할 페이지

Xamarin.Forms에는 다음 페이지 유형을 지원합니다.

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     | 
| --- | --- | 
| [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 페이지의 단순 하 고 가장 일반적인 형식이입니다. 설정의 [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentPage.Content/) 단일 속성 [ `View` ](views.md) 가장 자주 변경 되는 개체는 [ `Layout` ](layouts.md) 같은 [ `StackLayout` ](layouts.md#stackLayout), [ `Grid` ](layouts.md#grid), 또는 [ `ScrollView` ](layouts.md#scrollView)합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) | [![ContentPage 예제](pages-images/ContentPage.png "ContentPage 예제")](pages-images/ContentPage-Large.png#lightbox "ContentPage 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     | 
| --- | --- | 
| A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 두 정보 창을 관리 합니다. 설정의 [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) 목록이 나 메뉴를 보여 주는 일반적으로 페이지에는 속성입니다. 설정의 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) 속성 페이지의 마스터 페이지에서 선택한 항목을 표시 합니다. [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) 속성은 마스터 또는 세부 정보 페이지 표시 되는지 여부를 제어 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) / [가이드](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![MasterDetailPage 예제](pages-images/MasterDetailPage.png "MasterDetailPage 예제")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) 와 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     | 
| --- | --- | 
| [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 스택 기반 아키텍처를 사용 하 여 다른 페이지 간의 탐색을 관리 합니다. 인스턴스 홈 페이지의 응용 프로그램의 페이지 탐색을 사용할 경우의 생성자에 전달 해야는 `NavigationPage` 개체입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) / [가이드](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [예제 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/), [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/), 및 [3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![NavigationPage 예제](pages-images/NavigationPage.png "NavigationPage 예제")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) 와 [코드 뒤에 있는 =](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     | 
| --- | --- | 
| [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 에서 파생 [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) 클래스 및 자식 간 탐색 탭을 사용 하 여 페이지를 허용 합니다. 설정의 [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) 속성 페이지 또는 집합의 컬렉션을는 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) 속성 데이터 개체의 컬렉션을 고 [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) 속성을 한 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 각 개체를 시각적으로 나타내야 하는 방법을 설명 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) / [가이드](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [예제 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/) 및 [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![TabbedPage 예제](pages-images/TabbedPage.png "TabbedPage 예제")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     | 
| --- | --- | 
| [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) 에서 파생 [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) 클래스 및 손가락 넘기기가 읽는 자식 간의 탐색을 허용 합니다. 설정의 [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) 속성의 컬렉션을 [ `ContentPage` ](#contentPage) 개체 또는 집합은 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) 속성 데이터 개체의 컬렉션을 및 [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) 속성을 한 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 각 개체를 시각적으로 나타내야 하는 방법을 설명 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) / [가이드](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [예제 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/) 및 [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![CarouselPage 예제](pages-images/CarouselPage.png "CarouselPage 예제")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     | 
| --- | --- | 
| [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) 컨트롤 템플릿 사용 하 여 전체 화면 콘텐츠가 표시 되 고 기본 클래스에 대 한 [ `ContentPage` ](#contentPage)합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) / [가이드](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![TemplatedPage 예제](pages-images/TemplatedPage.png "TemplatedPage 예제")](pages-images/TemplatedPage.png "TemplatedPage 예제") |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery 샘플](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API 설명서](https://developer.xamarin.com/api/root/Xamarin.Forms/)
