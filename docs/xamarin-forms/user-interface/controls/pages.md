---
제목: " Xamarin.Forms 페이지" 설명: " Xamarin.Forms 페이지는 플랫폼 간 모바일 응용 프로그램 화면을 나타냅니다. 이 문서에서는에 포함 된 페이지를 나열 Xamarin.Forms 합니다.
assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC: xamarin-forms author: davidbritch: dabritch: ms. date: 01/12/2016 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-pages"></a>Xamarin.Forms Pages

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin Forms 페이지는 플랫폼 간 모바일 응용 프로그램 화면을 나타냅니다._

아래에서 설명 하는 모든 페이지 형식은 클래스에서 파생 됩니다 Xamarin.Forms [`Page`](xref:Xamarin.Forms.Page) . 이러한 시각적 요소는 대부분의 화면을 차지 합니다. `Page`개체는 `ViewController` iOS의와 `Page` 유니버설 Windows 플랫폼의를 나타냅니다. Android에서 각 페이지는와 같은 화면을 차지 `Activity` 하지만 Xamarin.Forms 페이지는 개체가 *아닙니다* `Activity` .

[![](pages-images/pages-sml.png "Xamarin.Forms Page Types")](pages-images/pages.png#lightbox "Xamarin.Forms Page Types")

## <a name="pages"></a>페이지

Xamarin.Forms에서는 다음 페이지 형식을 지원 합니다.

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage)는 가장 간단 하 고 가장 일반적인 페이지 유형입니다. 속성을 [`Content`](xref:Xamarin.Forms.ContentPage.Content) 단일 개체로 설정 합니다 [`View`](views.md) .이 개체는 일반적으로 [`Layout`](layouts.md) , 또는와 같은입니다 [`StackLayout`](layouts.md#stacklayout) [`Grid`](layouts.md#grid) [`ScrollView`](layouts.md#scrollview) .<br /><br />[API 문서](xref:Xamarin.Forms.ContentPage) | [![ContentPage 예제](pages-images/ContentPage.png "ContentPage 예제")](pages-images/ContentPage-Large.png#lightbox "ContentPage 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| 는 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 두 가지 정보 창을 관리 합니다. 속성을 [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) 일반적으로 목록이 나 메뉴를 표시 하는 페이지로 설정 합니다. 속성을 [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) 마스터 페이지에서 선택한 항목을 표시 하는 페이지로 설정 합니다. [`IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented)속성은 마스터 또는 세부 페이지가 표시 되는지 여부를 제어 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.MasterDetailPage)  /  [가이드](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage) | [![MasterDetailPage 예제](pages-images/MasterDetailPage.png "MasterDetailPage 예제")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs)  /  에 대 한 c # 코드 [코드 숨김이](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) 포함 된 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| 는 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 스택 기반 아키텍처를 사용 하 여 다른 페이지 간의 탐색을 관리 합니다. 응용 프로그램에서 페이지 탐색을 사용 하는 경우 홈 페이지의 인스턴스를 개체의 생성자에 전달 해야 합니다 `NavigationPage` .<br /><br />[API 설명서](xref:Xamarin.Forms.NavigationPage)  /  [가이드](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)  /  [예제 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata), [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)  | [![NavigationPage 예제](pages-images/NavigationPage.png "NavigationPage 예제")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs)  /  에 대 한 c # 코드 [코드 = 숨김으로](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)추상 클래스에서 파생 되 [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) 고 탭을 사용 하 여 자식 페이지 간을 탐색할 수 있도록 합니다. 속성을 [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) 페이지 컬렉션으로 설정 하거나, 속성을 데이터 개체 컬렉션으로 설정 하 고, 속성을로 설정 하 여 [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 각 개체를 시각적으로 표시 하는 방법을 설명 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.TabbedPage)  /  [가이드](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)  /  [샘플 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage) 및 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage) | [![TabbedPage 예제](pages-images/TabbedPage.png "TabbedPage 예제")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)추상 클래스에서 파생 [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) 되며 손가락 살짝 밀기를 통해 자식 페이지 간을 탐색할 수 있습니다. 속성을 [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) 개체의 컬렉션으로 설정 [`ContentPage`](#contentpage) 하거나, 속성을 데이터 개체 컬렉션으로 설정 하 고, 속성을로 설정 하 여 [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 각 개체를 시각적으로 표시 하는 방법을 설명 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.CarouselPage)  /  [가이드](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md)  /  [샘플 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage) 및 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate) | [![CarouselPage 예제](pages-images/CarouselPage.png "CarouselPage 예제")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)컨트롤 템플릿이 있는 전체 화면 콘텐츠를 표시 하 고,의 기본 클래스입니다 [`ContentPage`](#contentpage) .<br /><br />[API 설명서](xref:Xamarin.Forms.TemplatedPage)  /  [가이드](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![TemplatedPage 예제](pages-images/TemplatedPage.png "TemplatedPage 예제")](pages-images/TemplatedPage.png "TemplatedPage 예제") |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms양식 갤러리 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI 설명서](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
