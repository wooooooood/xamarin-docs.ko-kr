---
title: Xamarin 양식 페이지
description: Xamarin Forms 페이지는 플랫폼 간 모바일 응용 프로그램 화면을 나타냅니다. 이 문서에서는 Xamarin.ios에 포함 된 페이지를 나열 합니다.
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 278256f75d94fe47510ae4d15f12a3ff3a6a2b19
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "69976440"
---
# <a name="xamarinforms-pages"></a>Xamarin 양식 페이지

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin Forms 페이지는 플랫폼 간 모바일 응용 프로그램 화면을 나타냅니다._

아래에서 설명 하는 모든 페이지 형식은 Xamarin.ios [`Page`](xref:Xamarin.Forms.Page) 클래스에서 파생 됩니다. 이러한 시각적 요소는 대부분의 화면을 차지 합니다. @No__t_0 개체는 iOS의 `ViewController` 및 유니버설 Windows 플랫폼의 `Page`를 나타냅니다. Android에서 각 페이지는 `Activity`와 같은 화면을 차지 하지만 Xamarin.ios 페이지는 `Activity` *되지 않습니다* .

[![](pages-images/pages-sml.png "Xamarin.Forms Page Types")](pages-images/pages.png#lightbox "Xamarin.Forms Page Types")

## <a name="pages"></a>Pages

Xamarin.ios는 다음과 같은 페이지 유형을 지원 합니다.

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage) 가장 간단 하 고 가장 일반적인 페이지 유형입니다. [@No__t_1](xref:Xamarin.Forms.ContentPage.Content) 속성을 단일 [`View`](views.md) 개체로 설정 합니다 .이 개체는 일반적으로 [`StackLayout`](layouts.md#stackLayout), [`Grid`](layouts.md#grid)또는 [`Content`1](layouts.md#scrollView)같은 [`Layout`](layouts.md) 입니다.<br /><br />[API 문서](xref:Xamarin.Forms.ContentPage) | [![ContentPage 예제](pages-images/ContentPage.png "ContentPage 예제")](pages-images/ContentPage-Large.png#lightbox "ContentPage 예제")<br />이 페이지  / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| [@No__t_1](xref:Xamarin.Forms.MasterDetailPage) 는 두 가지 정보 창을 관리 합니다. [@No__t_1](xref:Xamarin.Forms.MasterDetailPage.Master) 속성을 일반적으로 목록이 나 메뉴를 표시 하는 페이지로 설정 합니다. [@No__t_1](xref:Xamarin.Forms.MasterDetailPage.Detail) 속성을 마스터 페이지에서 선택한 항목을 표시 하는 페이지로 설정 합니다. [@No__t_1](xref:Xamarin.Forms.MasterDetailPage.IsPresented) 속성은 마스터 또는 세부 페이지를 표시할지 여부를 제어 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.MasterDetailPage)  / [가이드](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)  / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage) | [![MasterDetailPage 예제](pages-images/MasterDetailPage.png "MasterDetailPage 예제")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage 예제")<br />[코드 숨김으로](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml)  /  [이 페이지에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| [@No__t_1](xref:Xamarin.Forms.NavigationPage) 는 스택 기반 아키텍처를 사용 하 여 다른 페이지 간의 탐색을 관리 합니다. 응용 프로그램에서 페이지 탐색을 사용 하는 경우 홈 페이지의 인스턴스를 `NavigationPage` 개체의 생성자에 전달 해야 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.NavigationPage)  / [가이드](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)  / [Sample 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata), [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)  | [![NavigationPage 예제](pages-images/NavigationPage.png "NavigationPage 예제")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage 예제")<br />[코드 = 숨김으로](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml)  /  [이 페이지에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 는 abstract [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) 클래스에서 파생 되며 탭을 사용 하 여 자식 페이지 간을 탐색할 수 있습니다. [@No__t_1](xref:Xamarin.Forms.MultiPage`1.Children) 속성을 페이지 컬렉션으로 설정 하거나 [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 속성을 데이터 개체 컬렉션으로 설정 하 고 [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 속성을 각 개체가 시각적으로 표시 되는 방법을 설명 하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 으로 설정 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.TabbedPage)  / [가이드](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)  / [샘플 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage) 및 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage) | [![TabbedPage 예제](pages-images/TabbedPage.png "TabbedPage 예제")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage 예제")<br />이 페이지  / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) 는 abstract [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) 클래스에서 파생 되며 손가락으로 살짝 밀기를 통해 자식 페이지를 탐색할 수 있습니다. [@No__t_1](xref:Xamarin.Forms.MultiPage`1.Children) 속성을 [`ContentPage`](#contentPage) 개체의 컬렉션으로 설정 하거나 [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 속성을 데이터 개체의 컬렉션으로 설정 하 고 [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 속성을 각 개체를 시각적으로 표현 하는 방법을 설명 하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 로 설정 합니다. 나타내는.<br /><br />[API 설명서](xref:Xamarin.Forms.CarouselPage)  / [가이드](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md)  / [샘플 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage) 및 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate) | [![CarouselPage 예제](pages-images/CarouselPage.png "CarouselPage 예제")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage 예제")<br />이 페이지  / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) 는 컨트롤 템플릿을 사용 하 여 전체 화면 콘텐츠를 표시 하 고 [`ContentPage`](#contentPage)의 기본 클래스입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.TemplatedPage)  / [가이드](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![TemplatedPage 예제](pages-images/TemplatedPage.png "TemplatedPage 예제")](pages-images/TemplatedPage.png "TemplatedPage 예제") |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.ios 양식 갤러리 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API 설명서](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
