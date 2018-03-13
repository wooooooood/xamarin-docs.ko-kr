---
title: "탭된 페이지"
description: "Xamarin.Forms TabbedPage 탭 목록과 큰 세부 정보 영역에서 세부 정보 영역에 콘텐츠를 로드 하는 각 탭으로 구성 됩니다. 이 문서를 사용 하는 TabbedPage 페이지의 컬렉션을 탐색 하는 방법을 보여줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 2ec783b6963fc4ae14166ebf1e56bf8a802ba8b4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="tabbed-page"></a>탭된 페이지

_Xamarin.Forms TabbedPage 탭 목록과 큰 세부 정보 영역에서 세부 정보 영역에 콘텐츠를 로드 하는 각 탭으로 구성 됩니다. 이 문서를 사용 하는 TabbedPage 페이지의 컬렉션을 탐색 하는 방법을 보여줍니다._

## <a name="overview"></a>개요

다음 스크린 샷 표시는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 각 플랫폼에서:

![](tabbed-page-images/tab1.png "TabbedPage 예제")

다음 스크린샷에서 각 플랫폼에서 탭 형식에 집중 합니다.

![](tabbed-page-images/tabbedpage-components.png "TabbedPage 탭 구성 요소")

레이아웃은 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), 및 해당 탭 플랫폼에 따라 달라 집니다.

- Ios, 화면 맨 아래에 표시 탭의 목록 및 세부 정보 영역을 초과 합니다. 각 탭은 또한 일반적인 확인에 대 한 투명 PNG 30 x 30, 높은 확인용 60x60 및 iPhone 6에 대 한 90 x 90 있어야 하 고 아이콘 이미지는 더하기 해결 합니다. 6 개 이상의 탭 개가 *더 많은* 탭이 표시 됩니다, 사용할 수 있는 추가 탭 액세스할 수 있습니다. Xamarin.Forms 응용 프로그램에서 이미지를 로드 하는 방법에 대 한 자세한 내용은 참조 [이미지 작업](~/xamarin-forms/user-interface/images.md)합니다. 아이콘 요구 사항에 대 한 자세한 내용은 참조 [탭 응용 프로그램 만들기](~/ios/user-interface/controls/creating-tabbed-applications.md)합니다.

    > [!NOTE]
  > `TabbedRenderer` for iOS에는 재정의 가능한 `GetIcon` 탭 아이콘을 지정 된 소스에서 로드를 사용할 수 있는 메서드. 이 재정의 사용 하면에서 SVG 이미지 아이콘으로 사용할 수는 `TabbedPage`합니다. 또한 아이콘의 또는 선택 하지 않은 버전을 제공할 수 있습니다.

- Android에서는 화면 위쪽에 표시 된 탭의 목록 및 세부 정보 영역 미만인 합니다. 탭 이름이 자동으로 대/소문자를 하 고 있는 한 화면에 맞게 너무 많은 경우에 탭의 컬렉션을 스크롤할 수 있습니다.

    > [!NOTE]
  > 참고는 AppCompat을 Android에서 사용할 경우 각 탭도 아이콘이 표시 됩니다. 또한는 `TabbedPageRenderer` for Android AppCompat에는 재정의 가능한 `SetTabIcon` 사용자 지정에서 탭 아이콘을 로드 하는 데 사용 될 방법을 `Drawable`합니다. 이 재정의 사용 하면에서 SVG 이미지 아이콘으로 사용할 수는 `TabbedPage`합니다.

- Windows Phone 화면 위쪽에 표시 된 탭의 목록 및 세부 정보 영역 미만인 합니다. 한 화면에 맞게 너무 많이 있는 경우 이름은 자동으로 사용자를 소문자로 변환 탭 탭의 컬렉션을 스크롤할 수 있습니다.
- Windows 태블릿 양식-요인에는 탭이 나타나지 항상 및 사용자가 통과 다운 해야 (또는 마우스 오른쪽 단추 클릭 마우스 연결 되어 있는 경우) 탭을 확인 하는 `TabbedPage` (아래와 같이).

![](tabbed-page-images/windows-tabs.png "Windows에서 TabbedPage 탭")

## <a name="creating-a-tabbedpage"></a>TabbedPage 만들기

두 가지 접근 방법을 만드는 데 사용할 수는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/):

- [채울](#Populating_a_TabbedPage_with_a_Page_Collection) 는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 자식 요소의 컬렉션으로 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 컬렉션과 같은 개체 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 인스턴스.
- [할당](#Populating_a_TabbedPage_with_a_Template) 컬렉션에는 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) 속성 및 할당 한 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 에 [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) 에 대 한 페이지를 반환 하는 속성 컬렉션의 개체입니다.

두 방법으로는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 사용자가 각 탭을 선택 합니다. 각 페이지를 표시 합니다.

> [!NOTE]
> 하는 것이 좋습니다는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 으로 채워져야 할지 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 및 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)인스턴스만 있습니다. 이 모든 플랫폼에서 일관 된 사용자 환경을 위해 도움이 됩니다.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>페이지 컬렉션으로 TabbedPage 채우기

다음 XAML 코드 예제는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 자식 요소의 컬렉션으로 채워 생성 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 개체:

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

다음 코드 예제에 해당 하는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) C#에서 만들어진:

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

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 두 명의 자식 채워집니다 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 개체입니다. 첫 번째 자식이 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 인스턴스 및 두 번째 탭은는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 포함 하는 `ContentPage` 인스턴스.

> [!NOTE]
> [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) UI 가상화를 지원 하지 않습니다. 따라서 성능 영향을 받을 수는 경우는 `TabbedPage` 자식 요소가 너무 많습니다.

다음 스크린 샷 표시는 `TodayPage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 에 표시 되는 인스턴스는 *오늘* 탭:

![](tabbed-page-images/today-page.png "에 TabbedPage ContentPage")

선택 하는 *일정* 표시 탭는 `SchedulePage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 에 겹쳐서 표시 된 인스턴스는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 인스턴스를 마우스에 표시 되는 다음 스크린 샷:

![](tabbed-page-images/schedule-page.png "에 TabbedPage NavigationPage")

레이아웃에 대 한 내용은 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), 참조 [수행 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)합니다.

> [!NOTE]
> 배치 하도록 허용 하는 동안는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 에 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)를 배치 하는 것이 좋습니다 하지는 `TabbedPage` 에 `NavigationPage`합니다. ¿¡´, Ios,는 `UITabBarController` 항상 역할에 대 한 래퍼를는 `UINavigationController`합니다. 자세한 내용은 참조 [보기 컨트롤러 인터페이스가 결합](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) iOS 개발자 라이브러리에서에서.

#### <a name="navigation-inside-a-tab"></a>탭 내 탐색

호출 하 여 두 번째 탭에서 탐색을 수행할 수 있습니다는 [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) 에서 메서드는 [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 속성의는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 인스턴스를 다음 코드 예제에서와 같이:

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

`UpcomingAppointmentsPage` 인스턴스가 탐색 스택으로 푸시되어 활성 페이지가 됩니다. 이 다음 스크린샷에 나와 있습니다.

![](tabbed-page-images/navigationpage.png "탭 내 탐색")

탐색에 사용 하 여 수행 하는 방법에 대 한 자세한 내용은 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 클래스를 참조 하십시오. [계층 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)합니다.

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>템플릿 사용 하 여 TabbedPage 채우기

다음 XAML 코드 예제에서는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 할당 하 여 생성 한 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 에 [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) 에 대 한 페이지를 반환 하는 속성 컬렉션의 개체:

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

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 설정 하 여 데이터를 채운는 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) 코드 숨김 파일에 대 한 생성자에서 속성:

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

다음 코드 예제에 해당 하는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) C#에서 만들어진:

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

각 탭에 표시 됩니다는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 를 사용 하는 일련의 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 및 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 인스턴스 탭에 대 한 데이터를 표시 합니다. 에 대 한 콘텐츠를 표시 하는 다음 스크린샷에서 *Tamarin* 탭:

![](tabbed-page-images/tab3.png "템플릿 사용 하 여 TabbedPage 채우기")

해당 탭에 대 한 콘텐츠를 표시 한 다음 다른 탭을 선택 합니다.

> [!NOTE]
> [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) UI 가상화를 지원 하지 않습니다. 따라서 성능 영향을 받을 수는 경우는 `TabbedPage` 자식 요소가 너무 많습니다.

에 대 한 자세한 내용은 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), 참조 [25 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold Xamarin.Forms 책의 합니다.

## <a name="summary"></a>요약

이 문서에는 페이지의 컬렉션을 탐색 하는 TabbedPage를 사용 하는 방법을 보여 줍니다. Xamarin.Forms는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 탭 목록과 큰 세부 정보 영역에서 세부 정보 영역에 콘텐츠를 로드 하는 각 탭으로 구성 됩니다.


## <a name="related-links"></a>관련 링크

- [페이지 변형](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (sample)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)
