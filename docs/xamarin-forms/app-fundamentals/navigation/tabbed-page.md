---
title: Xamarin.Forms TabbedPage
description: Xamarin.Forms TabbedPage는 탭 목록과 더 큰 세부 정보 영역으로 구성되며 각 탭은 세부 정보 영역으로 콘텐츠를 로드합니다. 이 문서에서는 페이지의 컬렉션을 검색하려면 TabbedPage를 사용하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 8926813e8efae72efa9af2221318d6f1ff1e344f
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970932"
---
# <a name="xamarinforms-tabbed-page"></a>Xamarin.Forms TabbedPage

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)

_Xamarin.Forms TabbedPage는 탭 목록과 더 큰 세부 정보 영역으로 구성되며 각 탭은 세부 정보 영역으로 콘텐츠를 로드합니다. 이 문서에서는 페이지의 컬렉션을 검색하려면 TabbedPage를 사용하는 방법을 설명합니다._

## <a name="overview"></a>개요

다음 스크린샷은 각 플랫폼에서 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)를 보여줍니다.

![](tabbed-page-images/tab1.png "TabbedPage 예제")

다음 스크린샷은 각 플랫폼의 탭 서식에 초점을 둡니다.

![](tabbed-page-images/tabbedpage-components.png "TabbedPage 탭 구성 요소")

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)의 레이아웃 및 탭은 플랫폼에 따라 달라집니다.

- iOS에서 탭 목록은 화면 맨 아래에 나타나고 세부 내용 영역이 위에 위치합니다. 각 탭에는 일반 해상도의 경우 30x30 PNG, 고해상도의 경우 60x60, iPhone 6 Plus의 경우 90x90라는 투명도인 아이콘 이미지도 포함됩니다. 6개 이상의 탭이 있는 경우 다른 탭에 액세스하는 데 사용할 수 있는 *자세히* 탭이 표시됩니다. Xamarin.Forms 애플리케이션에서 이미지를 로드하는 방법에 대한 자세한 내용은 [이미지 작업](~/xamarin-forms/user-interface/images.md)을 참조하세요. 아이콘 요구 사항에 대한 자세한 내용은 [탭된 애플리케이션 만들기](~/ios/user-interface/controls/creating-tabbed-applications.md)를 참조하세요.

  > [!NOTE]
  > iOS의 경우 `TabbedRenderer`에는 지정된 원본에서 탭 아이콘을 로드하는 데 사용할 수 있는 재정의 가능한 `GetIcon` 메서드가 포함됩니다. 이 재정의를 통해 `TabbedPage`에서 SVG 이미지를 아이콘으로 사용할 수 있습니다. 또한 선택하거나 선택하지 않은 버전의 아이콘을 제공할 수 있습니다.

- Android에서 탭 목록은 기본적으로 화면 맨 위에 표시되고 세부 정보 영역은 아래에 위치합니다. 그러나 탭 목록은 특정 플랫폼에서 화면 아래쪽으로 이동할 수 있습니다. 자세한 내용은 [TabbedPage 도구 모음 배치 및 색 설정](~/xamarin-forms/platform/android/tabbedpage-toolbar-placement-color.md)을 참조하세요.

  > [!NOTE]
  > Android에서 AppCompat을 사용하는 경우 각 탭에도 아이콘이 표시됩니다. 또한 Android AppCompat의 경우 `TabbedPageRenderer`에는 사용자 지정된 `Drawable`에서 탭 아이콘을 로드하는 데 사용할 수 있는 재정의 가능한 `GetIconDrawable` 메서드가 포함됩니다. 이 재정의를 통해 `TabbedPage`에서 SVG 이미지를 아이콘으로 사용할 수 있고 위아래 탭 표시줄 모두에서 작업할 수 있습니다. 또는 위쪽 탭 표시줄의 경우 재정의 가능한 `SetTabIcon` 메서드를 사용하여 사용자 지정 `Drawable`에서 탭 아이콘을 로드할 수 있습니다.

- Windows 태블릿 양식 요소에서 탭은 표시되지 않을 수 있습니다. 또한 사용자는 `TabbedPage` 탭을 보기 위해 (다음과 같이) 아래로 스와이프(또는 마우스가 연결된 경우 마우스 오른쪽 단추 클릭)해야 합니다.

    ![](tabbed-page-images/windows-tabs.png "Windows의 TabbedPage 탭")

## <a name="creating-a-tabbedpage"></a>TabbedPage 만들기

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 다음 속성을 정의합니다.

- 탭 표시줄의 배경색인 [`Color`](xref:Xamarin.Forms.Color) 형식의 [`BarBackgroundColor`](xref:Xamarin.Forms.TabbedPage.BarBackgroundColor).
- 탭 표시줄의 텍스트 색인 [`Color`](xref:Xamarin.Forms.Color) 형식의 [`BarTextColor`](xref:Xamarin.Forms.TabbedPage.BarTextColor).
- 선택된 탭의 색인 [`Color`](xref:Xamarin.Forms.Color) 형식의 [`SelectedTabColor`](xref:Xamarin.Forms.TabbedPage.SelectedTabColor).
- 선택 취소된 탭의 색인 [`Color`](xref:Xamarin.Forms.Color) 형식의 [`UnselectedTabColor`](xref:Xamarin.Forms.TabbedPage.UnselectedTabColor).

이 모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되며, 이는 속성에 스타일을 지정할 수 있으며 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다.

두 방법을 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)를 만드는 데 사용할 수 있습니다.

- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)를 자식 [`Page`](xref:Xamarin.Forms.Page) 개체 컬렉션(예: [`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스의 컬렉션)으로 [채웁니다](#Populating_a_TabbedPage_with_a_Page_Collection).
- 컬렉션을 [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 속성에 [할당](#Populating_a_TabbedPage_with_a_Template)하고 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)을 [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 속성에 할당하여 컬렉션의 개체에 대한 페이지를 반환합니다.

사용자가 각 탭을 선택하면 두 가지 방법으로 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 각 페이지를 표시합니다.

> [!NOTE]
> [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)를 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 및 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스만으로 채우는 것이 좋습니다. 이렇게 하면 모든 플랫폼에서 일관된 사용자 환경을 보장하는 데 도움이 됩니다.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>페이지 컬렉션으로 TabbedPage 채우기

다음 XAML 코드 예제에서는 자식 [`Page`](xref:Xamarin.Forms.Page) 개체의 컬렉션으로 채워서 생성된 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)를 보여줍니다.

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:TabbedPageWithNavigationPage;assembly=TabbedPageWithNavigationPage"
            x:Class="TabbedPageWithNavigationPage.MainPage">
    <local:TodayPage />
    <NavigationPage Title="Schedule" IconImageSource="schedule.png">
        <x:Arguments>
            <local:SchedulePage />
        </x:Arguments>
    </NavigationPage>
</TabbedPage>
```

다음 코드 예제에서는 C#에서 만든 해당 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)를 보여줍니다.

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    var navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.IconImageSource = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 두 개의 자식 [`Page`](xref:Xamarin.Forms.Page) 개체로 채워집니다. 첫 번째 자식은 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스이고 두 번째 탭은 `ContentPage` 인스턴스를 포함하는 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)입니다.

> [!NOTE]
> [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 UI 가상화를 지원하지 않습니다. 따라서 `TabbedPage`에 너무 많은 자식 요소가 포함된 경우 성능에 영향을 미칠 수 있습니다.

다음 스크린샷에서는 *오늘* 탭에 표시된 `TodayPage`[`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스를 보여줍니다.

![](tabbed-page-images/today-page.png "TabbedPage의 ContentPage")

*일정* 탭을 선택하면 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 인스턴스에서 래핑된 `SchedulePage`[`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스를 표시하고 다음 스크린샷처럼 표시됩니다.

![](tabbed-page-images/schedule-page.png "TabbedPage의 NavigationPage")

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)의 레이아웃에 대한 정보는 [검색 수행](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)을 참조하세요.

> [!NOTE]
> [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)를 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)에 배치할 수 있지만 `TabbedPage`를 `NavigationPage`에 배치하지 않는 것이 좋습니다. iOS의 경우 `UITabBarController`가 항상 `UINavigationController`에 대한 래퍼의 역할을 하기 때문입니다. 자세한 내용은 iOS 개발자 라이브러리에서 [결합된 보기 컨트롤러 인터페이스](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html)를 참조하세요.

#### <a name="navigation-inside-a-tab"></a>탭 내에서 검색

다음 코드 예제에서 설명한 대로 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스의 [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성에서 [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) 메서드를 호출하여 두 번째 탭에서 검색을 수행할 수 있습니다.

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

`UpcomingAppointmentsPage` 인스턴스가 탐색 스택으로 푸시되어 활성 페이지가 됩니다. 이 과정은 다음 스크린샷에 나와 있습니다.

![](tabbed-page-images/navigationpage.png "탭 내에서 검색")

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 클래스를 사용하는 검색을 수행하는 방법에 대한 자세한 내용은 [계층적 검색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)을 참조하세요.

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>템플릿으로 TabbedPage 채우기

다음 XAML 코드 예제에서는 컬렉션의 개체에 대해 페이지를 반환하도록 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)을 [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 속성에 할당하여 생성된 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)를 보여줍니다.

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="clr-namespace:TabbedPageDemo;assembly=TabbedPageDemo"
            x:Class="TabbedPageDemo.TabbedPageDemoPage">
  <TabbedPage.Resources>
    <ResourceDictionary>
      <local:NonNullToBooleanConverter x:Key="booleanConverter" />
    </ResourceDictionary>
  </TabbedPage.Resources>
  <TabbedPage.ItemTemplate>
    <DataTemplate>
      <ContentPage Title="{Binding Name}" IconImageSource="monkeyicon.png">
        <StackLayout Padding="5, 25">
          <Label Text="{Binding Name}" Font="Bold,Large" HorizontalOptions="Center" />
          <Image Source="{Binding PhotoUrl}" WidthRequest="200" HeightRequest="200" />
          <StackLayout Padding="50, 10">
            <StackLayout Orientation="Horizontal">
              <Label Text="Family:" HorizontalOptions="FillAndExpand" />
              <Label Text="{Binding Family}" Font="Bold,Medium" />
            </StackLayout>
            ...
          </StackLayout>
        </StackLayout>
      </ContentPage>
    </DataTemplate>
  </TabbedPage.ItemTemplate>
</TabbedPage>
```

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 코드 숨김 파일에 대한 생성자에서 [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 속성을 설정하여 데이터로 채웁니다.

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

다음 코드 예제에서는 C#에서 만든 해당 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)를 보여줍니다.

```csharp
public class TabbedPageDemoPageCS : TabbedPage
{
  public TabbedPageDemoPageCS ()
  {
    var booleanConverter = new NonNullToBooleanConverter ();

    ItemTemplate = new DataTemplate (() => {
      var nameLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label)),
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center
      };
      nameLabel.SetBinding (Label.TextProperty, "Name");

      var image = new Image { WidthRequest = 200, HeightRequest = 200 };
      image.SetBinding (Image.SourceProperty, "PhotoUrl");

      var familyLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
        FontAttributes = FontAttributes.Bold
      };
      familyLabel.SetBinding (Label.TextProperty, "Family");
      ...

      var contentPage = new ContentPage {
        IconImageSource = "monkeyicon.png",
        Content = new StackLayout {
          Padding = new Thickness (5, 25),
          Children = {
            nameLabel,
            image,
            new StackLayout {
              Padding = new Thickness (50, 10),
              Children = {
                new StackLayout {
                  Orientation = StackOrientation.Horizontal,
                  Children = {
                    new Label { Text = "Family:", HorizontalOptions = LayoutOptions.FillAndExpand },
                    familyLabel
                  }
                },
                ...
              }
            }
          }
        }
      };
      contentPage.SetBinding (TitleProperty, "Name");
      return contentPage;
    });
    ItemsSource = MonkeyDataModel.All;
  }
}
```

각 탭은 탭에 대한 데이터를 표시하기 위해 일련의 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 및 [`Label`](xref:Xamarin.Forms.Label) 인스턴스를 사용하는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)를 표시합니다. 다음 스크린샷에서는 *Tamarin* 탭에 대한 콘텐츠를 보여줍니다.

![](tabbed-page-images/tab3.png "템플릿으로 TabbedPage 채우기")

그런 다음, 다른 탭을 선택하면 해당 탭에 대한 콘텐츠를 표시합니다.

> [!NOTE]
> [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 UI 가상화를 지원하지 않습니다. 따라서 `TabbedPage`에 너무 많은 자식 요소가 포함된 경우 성능에 영향을 미칠 수 있습니다.

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)에 대한 자세한 내용은 Charles Petzold의 Xamarin.Forms 책의 [챕터 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)을 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 페이지의 컬렉션을 검색하는 데 TabbedPage를 사용하는 방법을 설명합니다. Xamarin.Forms [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 탭 목록과 더 큰 세부 정보 영역으로 구성되며 각 탭은 세부 정보 영역으로 콘텐츠를 로드합니다.


## <a name="related-links"></a>관련 링크

- [페이지 종류](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage(샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage(샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](xref:Xamarin.Forms.TabbedPage)
