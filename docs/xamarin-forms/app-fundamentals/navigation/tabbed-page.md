---
title: Xamarin.Forms 탭된 페이지
description: Xamarin.Forms TabbedPage 세부 정보 영역에 콘텐츠를 로드 하는 각 탭을 사용 하 여 탭 및 더 큰 세부 정보 영역을 목록으로 구성 됩니다. 이 문서에서는 페이지의 컬렉션을 탐색 하는 TabbedPage를 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 3eb978780222da2050fc91dfa41c68ef4bd3b6f4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996297"
---
# <a name="xamarinforms-tabbed-page"></a>Xamarin.Forms 탭된 페이지

_Xamarin.Forms TabbedPage 세부 정보 영역에 콘텐츠를 로드 하는 각 탭을 사용 하 여 탭 및 더 큰 세부 정보 영역을 목록으로 구성 됩니다. 이 문서에서는 페이지의 컬렉션을 탐색 하는 TabbedPage를 사용 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

다음 스크린샷에서 표시 된 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 각 플랫폼에서:

![](tabbed-page-images/tab1.png "TabbedPage 예제")

다음 스크린샷에서 각 플랫폼에서 탭 형식에 집중 합니다.

![](tabbed-page-images/tabbedpage-components.png "TabbedPage 탭 구성 요소")

레이아웃을 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), 및 해당 탭 플랫폼에 따라 달라 집니다.

- IOS에서 탭 목록 화면 맨 아래에 나타나고 세부 영역이 위에 합니다. 각 탭에는 일반적인 확인에 대 한 투명도 사용 하 여 30 PNG 30x, 높은 해상도 대 한 60x60 및 iPhone 6 용 90x90 될 아이콘 이미지도 또한 확인 합니다. 5 개 탭에 있는 경우는 *자세한* 탭이 표시 됩니다, 사용할 수 있는 다른 탭에 액세스 해야 합니다. Xamarin.Forms 응용 프로그램에서 이미지를 로드 하는 방법에 대 한 자세한 내용은 참조 하세요. [이미지를 사용 하 여 작업](~/xamarin-forms/user-interface/images.md)합니다. 아이콘 요구 사항에 대 한 자세한 내용은 참조 하세요. [응용 프로그램 탭 만들기](~/ios/user-interface/controls/creating-tabbed-applications.md)합니다.

    > [!NOTE]
  > 합니다 `TabbedRenderer` iOS에는 재정의 가능한 `GetIcon` 탭 아이콘을 지정된 된 소스에서 로드 하는 메서드. 이 재정의 사용 하면 SVG 이미지 아이콘으로 사용할 수는 `TabbedPage`합니다. 또한 아이콘의 선택 또는 선택 하지 않은 버전을 제공할 수 있습니다.

- Android에서 기본적으로 화면 맨 위에 있는 탭 목록 표시 및 세부 정보 영역에서 아래입니다. 그러나 탭은 플랫폼 특정 화면 아래쪽으로 이동할 수 있습니다. 자세한 내용은 [설정은 TabbedPage 도구 모음 배치 및 색](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-toolbar)합니다.

    > [!NOTE]
  > 참고는 AppCompat에서 Android를 사용 하는 경우 각 탭도 아이콘이 표시 됩니다. 또한 합니다 `TabbedPageRenderer` Android AppCompat에는 재정의 가능한 `SetTabIcon` 사용자 지정에서 탭 아이콘을 로드 하는 메서드 `Drawable`합니다. 이 재정의 사용 하면 SVG 이미지 아이콘으로 사용할 수는 `TabbedPage`합니다.

- Windows 태블릿 폼 팩터를 탭 항상는 표시 되지 않습니다 및 사용자가 스와이프 다운 해야 (또는 마우스 오른쪽 단추 클릭 마우스 연결 되어 있는 경우) 탭을 확인 하는 `TabbedPage` (아래와 같이).

![](tabbed-page-images/windows-tabs.png "Windows에서 TabbedPage 탭")

## <a name="creating-a-tabbedpage"></a>TabbedPage 만들기

두 가지 방법을 만드는 데 사용할 수는 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage):

- [채울](#Populating_a_TabbedPage_with_a_Page_Collection) 는 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 자식 컬렉션을 사용 하 여 [ `Page` ](xref:Xamarin.Forms.Page) 컬렉션과 같은 개체 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 인스턴스.
- [할당](#Populating_a_TabbedPage_with_a_Template) 컬렉션을를 [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 속성 및 할당을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 에 [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 에 대 한 페이지를 반환 하도록 속성 컬렉션의 개체입니다.

두 방법으로는 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 사용자가 각 탭에는 각 페이지를 표시 합니다.

> [!NOTE]
> 하는 것이 좋습니다는 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 채울 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 고 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)인스턴스에만 합니다. 이 모든 플랫폼에서 일관 된 사용자 환경을 보장 하는 데 도움이 됩니다.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>페이지 컬렉션으로 TabbedPage 채우기

다음 XAML 코드 예제는 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 자식 컬렉션을 사용 하 여 채워 생성 [ `Page` ](xref:Xamarin.Forms.Page) 개체:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:TabbedPageWithNavigationPage;assembly=TabbedPageWithNavigationPage"
            x:Class="TabbedPageWithNavigationPage.MainPage">
    <local:TodayPage />
    <NavigationPage Title="Schedule" Icon="schedule.png">
        <x:Arguments>
            <local:SchedulePage />
        </x:Arguments>
    </NavigationPage>
</TabbedPage>
```

다음 코드 예제에서는 해당 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) C#에서 만든:

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    var navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.Icon = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

합니다 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 두 개의 자식 채워집니다 [ `Page` ](xref:Xamarin.Forms.Page) 개체입니다. 첫 번째 자식이 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 인스턴스 및 두 번째 탭은을 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 포함 하는 `ContentPage` 인스턴스.

> [!NOTE]
> 합니다 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) UI 가상화를 지원 하지 않습니다. 경우 성능이 저하 될 수 있습니다 따라서는 `TabbedPage` 너무 많은 자식 요소가 포함 됩니다.

다음 스크린샷에서 표시 된 `TodayPage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 에 표시 되는 인스턴스를 *지금* 탭:

![](tabbed-page-images/today-page.png "ContentPage를 TabbedPage에서")

선택 하는 *일정* 표시 탭를 `SchedulePage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 래핑됩니다 인스턴스입니다를 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 인스턴스 및에 표시 됩니다는 다음 스크린 샷:

![](tabbed-page-images/schedule-page.png "TabbedPage의 NavigationPage")

레이아웃에 대 한 자세한를 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)를 참조 하십시오 [탐색을 수행](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)합니다.

> [!NOTE]
> 배치에 사용할 수 있지만 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 에 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), 적용할 좋지에 `TabbedPage` 에 `NavigationPage`. 이므로 ios의 경우는 `UITabBarController` 항상 역할에 대 한 래퍼를 `UINavigationController`입니다. 자세한 내용은 [결합 뷰 컨트롤러 인터페이스](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) iOS 개발자 라이브러리에서에서.

#### <a name="navigation-inside-a-tab"></a>탭 내에서 탐색

호출 하 여 두 번째 탭에서 탐색을 수행할 수 있습니다는 [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) 메서드를를 [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) 속성을 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 인스턴스를 다음 코드 예제에서 설명한 것 처럼:

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

`UpcomingAppointmentsPage` 인스턴스가 탐색 스택으로 푸시되어 활성 페이지가 됩니다. 다음 스크린샷과에서 같습니다.

![](tabbed-page-images/navigationpage.png "탭 내에서 탐색")

탐색에 사용 하 여 수행 하는 방법에 대 한 자세한 내용은 합니다 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 클래스를 참조 하십시오 [계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)합니다.

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>템플릿 사용 하 여 TabbedPage 채우기

다음 XAML 코드 예제에서는 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 할당 하 여 생성을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 에 [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 에 대 한 페이지를 반환 하도록 속성 컬렉션의 개체:

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
      <ContentPage Title="{Binding Name}" Icon="monkeyicon.png">
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

합니다 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 설정 하 여 데이터를 채운 합니다 [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 코드 숨김 파일에 대 한 생성자에서 속성:

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

다음 코드 예제에서는 해당 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) C#에서 만든:

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
        Icon = "monkeyicon.png",
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

각 탭에 표시 됩니다는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 사용 하는 일련의 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 및 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스 탭에 대 한 데이터를 표시 합니다. 다음 스크린샷에서 표시에 대 한 콘텐츠를 *Tamarin* 탭:

![](tabbed-page-images/tab3.png "템플릿 사용 하 여 TabbedPage 채우기")

그런 다음 다른 탭을 선택 하면 해당 탭에 대 한 콘텐츠를 표시 합니다.

> [!NOTE]
> 합니다 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) UI 가상화를 지원 하지 않습니다. 경우 성능이 저하 될 수 있습니다 따라서는 `TabbedPage` 너무 많은 자식 요소가 포함 됩니다.

에 대 한 자세한 내용은 합니다 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)를 참조 하세요 [25 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold의 Xamarin.Forms 책의 합니다.

## <a name="summary"></a>요약

이 문서에서는 페이지의 컬렉션을 탐색 하는 TabbedPage를 사용 하는 방법을 보여 줍니다. Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 세부 정보 영역에 콘텐츠를 로드 하는 각 탭을 사용 하 여 탭 및 더 큰 세부 정보 영역을 목록으로 구성 합니다.


## <a name="related-links"></a>관련 링크

- [페이지 종류](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](xref:Xamarin.Forms.TabbedPage)
