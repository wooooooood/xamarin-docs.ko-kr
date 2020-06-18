---
title: 'title: “Xamarin.Forms TabbedPage” description: “Xamarin.Forms TabbedPage는 탭 목록과 더 큰 세부 정보 영역으로 구성되며 각 탭은 세부 정보 영역으로 콘텐츠를 로드합니다.'
description: '이 문서에서는 페이지의 컬렉션을 검색하려면 TabbedPage를 사용하는 방법을 설명합니다.” ms.prod: xamarin ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 11/07/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 38389867ba52e63d8310e3b59d7838f58e8cf488
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84137516"
---
# <a name="xamarinforms-tabbedpage"></a>Xamarin.Forms TabbedPage

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage)

Xamarin.Forms [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 탭 목록과 더 큰 세부 정보 영역으로 구성되며 각 탭은 세부 정보 영역으로 콘텐츠를 로드합니다. 다음 스크린샷은 iOS 및 Android의 `TabbedPage`를 보여줍니다.

[![3개 탭이 포함된 TabbedPage의 스크린샷(iOS 및 Android)](tabbed-page-images/tabbedpage-today.png "3개의 탭이 있는 TabbedPage")](tabbed-page-images/tabbedpage-today-large.png#lightbox "3개의 탭이 있는 TabbedPage")

iOS에서 탭 목록은 화면 맨 아래에 나타나고 세부 내용 영역이 위에 위치합니다. 각 탭은 제목과 아이콘으로 구성되며 알파 채널이 있는 PNG 파일이어야 합니다. 세로 방향에서는 탭 제목 위에 탭 모음 아이콘이 표시됩니다. 가로 방향에서는 아이콘 및 제목이 나란히 표시됩니다. 또한 디바이스 및 방향에 따라 일반 또는 작은 탭 표시줄이 표시될 수도 있습니다. 6개 이상의 탭이 있는 경우 다른 탭에 액세스하는 데 사용할 수 있는 **자세히** 탭이 표시됩니다. 아이콘 요구 사항에 대한 자세한 내용은 developer.apple.com의 [탭 표시줄 아이콘 크기](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/custom-icons#tab-bar-icon-size)를 참조하세요.

> [!TIP]
> iOS의 경우 `TabbedRenderer`에는 지정된 원본에서 탭 아이콘을 로드하는 데 사용할 수 있는 재정의 가능한 `GetIcon` 메서드가 포함됩니다. 이 재정의를 통해 `TabbedPage`에서 SVG 이미지를 아이콘으로 사용할 수 있습니다. 또한 선택하거나 선택하지 않은 버전의 아이콘을 제공할 수 있습니다.

Android에서는 탭 목록이 화면 맨 위에 표시되고 세부 정보 영역은 아래에 위치합니다. 각 탭은 제목과 아이콘으로 구성되며 알파 채널이 있는 PNG 파일이어야 합니다. 그러나 탭은 특정 플랫폼에서 화면 아래쪽으로 이동할 수 있습니다. 6개 이상의 탭이 있고 탭 목록이 화면 아래쪽에 있는 경우 추가 탭에 액세스하는 데 사용할 수 있는 *자세히* 탭이 나타납니다. 아이콘 요구 사항에 대한 자세한 내용은 material.io의 [Tabs](https://material.io/components/tabs/#) 및 developer.android.com의 [다양한 픽셀 밀도 지원](https://developer.android.com/training/multiscreen/screendensities)을 참조하세요. 화면 아래쪽으로 탭을 이동하는 방법에 대한 자세한 내용은 [TabbedPage 도구 모음 배치 및 색 설정](~/xamarin-forms/platform/android/tabbedpage-toolbar-placement-color.md)을 참조하세요.

> [!TIP]
> Android AppCompat의 경우 `TabbedPageRenderer`에는 사용자 지정된 `Drawable`에서 탭 아이콘을 로드하는 데 사용할 수 있는 재정의 가능한 `GetIconDrawable` 메서드가 포함됩니다. 이 재정의를 통해 `TabbedPage`에서 SVG 이미지를 아이콘으로 사용할 수 있고 위아래 탭 표시줄 모두에서 작업할 수 있습니다. 또는 위쪽 탭 표시줄의 경우 재정의 가능한 `SetTabIcon` 메서드를 사용하여 사용자 지정 `Drawable`에서 탭 아이콘을 로드할 수 있습니다.

UWP(유니버설 Windows 플랫폼)에서는 탭 목록이 화면 맨 위에 표시되고 세부 정보 영역은 아래에 표시됩니다. 각 탭은 제목으로 구성됩니다. 그러나 플랫폼별로 각 탭에 아이콘을 추가할 수 있습니다. 자세한 내용은 [Windows의 TabbedPage 아이콘](~/xamarin-forms/platform/windows/tabbedpage-icons.md)을 참조하세요.

## <a name="create-a-tabbedpage"></a>TabbedPage 만들기

두 방법을 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)를 만드는 데 사용할 수 있습니다.

- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)를 자식 [`Page`](xref:Xamarin.Forms.Page) 개체 컬렉션(예: [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체의 컬렉션)으로 채웁니다. 자세한 내용은 [페이지 컬렉션을 사용하여 TabbedPage 채우기](#populate-a-tabbedpage-with-a-page-collection)를 참조하세요.
- 컬렉션을 [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 속성에 할당하고 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)을 [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 속성에 할당하여 컬렉션의 개체에 대한 페이지를 반환합니다. 자세한 내용은 [템플릿을 사용하여 TabbedPage 채우기](#populate-a-tabbedpage-with-a-template)를 참조하세요.

사용자가 각 탭을 선택하면 두 가지 방법으로 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 각 페이지를 표시합니다.

> [!IMPORTANT]
> [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)를 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 및 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스만으로 채우는 것이 좋습니다. 이렇게 하면 모든 플랫폼에서 일관된 사용자 환경을 보장하는 데 도움이 됩니다.

또한 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 다음 속성을 정의합니다.

- 탭 표시줄의 배경색인 [`Color`](xref:Xamarin.Forms.Color) 형식의 [`BarBackgroundColor`](xref:Xamarin.Forms.TabbedPage.BarBackgroundColor).
- 탭 표시줄의 텍스트 색인 [`Color`](xref:Xamarin.Forms.Color) 형식의 [`BarTextColor`](xref:Xamarin.Forms.TabbedPage.BarTextColor).
- 선택된 탭의 색인 [`Color`](xref:Xamarin.Forms.Color) 형식의 [`SelectedTabColor`](xref:Xamarin.Forms.TabbedPage.SelectedTabColor).
- 선택 취소된 탭의 색인 [`Color`](xref:Xamarin.Forms.Color) 형식의 [`UnselectedTabColor`](xref:Xamarin.Forms.TabbedPage.UnselectedTabColor).

이 모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되며, 이는 속성에 스타일을 지정할 수 있으며 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다.

> [!WARNING]
> [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)에서 각 [`Page`](xref:Xamarin.Forms.Page) 개체는 `TabbedPage`가 생성될 때 만들어집니다. 이로 인해 특히 `TabbedPage`가 애플리케이션의 루트 페이지인 경우 사용자 환경이 저하될 수 있습니다. 그러나 Xamarin.Forms Shell을 사용하면 탐색에 대한 응답으로 탭 표시줄을 통해 액세스되는 페이지를 요청에 따라 만들 수 있습니다. 자세한 내용은 [Xamarin.Forms Shell](~/xamarin-forms/app-fundamentals/shell/index.md)을 참조하세요.

## <a name="populate-a-tabbedpage-with-a-page-collection"></a>페이지 컬렉션으로 TabbedPage 채우기

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)를 자식 [`Page`](xref:Xamarin.Forms.Page) 개체 컬렉션(예: [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체의 컬렉션)으로 채울 수 있습니다. 이렇게 하려면 `Page` 개체를 [`TabbedPage.Children`](xref:Xamarin.Forms.MultiPage`1.Children*) 컬렉션에 추가합니다. 이 작업은 다음과 같이 XAML에서 수행합니다.

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

> [!NOTE]
> [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)가 파생되는 [`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1) 클래스의 [`Children`](xref:Xamarin.Forms.MultiPage`1.Children*) 속성은 `MultiPage<T>`의 `ContentProperty`입니다. 따라서 XAML에서 `Children` 속성에 [`Page`](xref:Xamarin.Forms.Page) 개체를 명시적으로 할당할 필요가 없습니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    NavigationPage navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.IconImageSource = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

이 예제에서 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)는 두 개의 [`Page`](xref:Xamarin.Forms.ContentPage) 개체로 채워집니다. 첫 번째 자식은 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체이고 두 번째 자식은 `ContentPage` 개체를 포함하는 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)입니다.

다음 스크린샷에서는 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)의 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체를 보여줍니다.

[![3개 탭이 포함된 TabbedPage의 스크린샷(iOS 및 Android)](tabbed-page-images/tabbedpage-today.png "3개의 탭이 있는 TabbedPage")](tabbed-page-images/tabbedpage-today-large.png#lightbox "3개의 탭이 있는 TabbedPage")

다른 탭을 선택하면 해당 탭을 나타내는 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체가 표시됩니다.

[![탭이 포함된 TabbedPage의 스크린샷(iOS 및 Android)](tabbed-page-images/tabbedpage-week.png "탭이 있는 TabbedPage")](tabbed-page-images/tabbedpage-week-large.png#lightbox "탭이 있는 TabbedPage")

**일정** 탭에서 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체가 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 개체에 래핑됩니다.

> [!WARNING]
> [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)를 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)에 배치할 수 있지만 `TabbedPage`를 `NavigationPage`에 배치하지 않는 것이 좋습니다. iOS의 경우 `UITabBarController`가 항상 `UINavigationController`에 대한 래퍼의 역할을 하기 때문입니다. 자세한 내용은 iOS 개발자 라이브러리에서 [결합된 보기 컨트롤러 인터페이스](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html)를 참조하세요.

## <a name="navigate-within-a-tab"></a>탭 내 탐색

[`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체가 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 개체에 래핑된 경우 탭 내 탐색을 수행할 수 있습니다. 이렇게 하려면 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체의 [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성에서 [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) 메서드를 호출합니다.

```csharp
await Navigation.PushAsync (new UpcomingAppointmentsPage ());
```

탐색하는 페이지는 [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) 메서드에 대한 인수로 지정됩니다. 이 예제에서는 `UpcomingAppointmentsPage` 페이지가 탐색 스택으로 푸시되어 활성 페이지가 됩니다.

[![iOS 및 Android에서 탭 내 탐색을 보여 주는 스크린샷](tabbed-page-images/tabbedpage-upcoming.png "탭에서 TabbedPage 탐색")](tabbed-page-images/tabbedpage-upcoming-large.png#lightbox "탭에서 TabbedPage 탐색")

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 클래스를 사용하는 검색을 수행하는 방법에 대한 자세한 내용은 [계층적 검색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)을 참조하세요.

## <a name="populate-a-tabbedpage-with-a-template"></a>템플릿으로 TabbedPage 채우기

[`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 속성에 데이터 컬렉션을 할당하고 데이터를 [`Page`](xref:Xamarin.Forms.Page) 개체로 템플릿화하는 [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 속성에 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)을 할당하여 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)를 페이지로 채울 수 있습니다. 이 작업은 다음과 같이 XAML에서 수행합니다.

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="clr-namespace:TabbedPageDemo;assembly=TabbedPageDemo"
            x:Class="TabbedPageDemo.TabbedPageDemoPage"
            ItemsSource="{x:Static local:MonkeyDataModel.All}">            
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

해당하는 C# 코드는 다음과 같습니다.

```csharp
public class TabbedPageDemoPageCS : TabbedPage
{
  public TabbedPageDemoPageCS ()
  {
    var booleanConverter = new NonNullToBooleanConverter ();

    ItemTemplate = new DataTemplate (() =>
    {
      var nameLabel = new Label
      {
        FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label)),
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center
      };
      nameLabel.SetBinding (Label.TextProperty, "Name");

      var image = new Image { WidthRequest = 200, HeightRequest = 200 };
      image.SetBinding (Image.SourceProperty, "PhotoUrl");

      var familyLabel = new Label
      {
        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
        FontAttributes = FontAttributes.Bold
      };
      familyLabel.SetBinding (Label.TextProperty, "Family");
      ...

      var contentPage = new ContentPage
      {
        IconImageSource = "monkeyicon.png",
        Content = new StackLayout {
          Padding = new Thickness (5, 25),
          Children =
          {
            nameLabel,
            image,
            new StackLayout
            {
              Padding = new Thickness (50, 10),
              Children =
              {
                new StackLayout
                {
                  Orientation = StackOrientation.Horizontal,
                  Children =
                  {
                    new Label { Text = "Family:", HorizontalOptions = LayoutOptions.FillAndExpand },
                    familyLabel
                  }
                },
                // ...
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

이 예제에서 각 탭은 [`Image`](xref:Xamarin.Forms.Image) 및 [`Label`](xref:Xamarin.Forms.Label) 개체를 사용하여 탭의 데이터를 표시하는 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체로 구성됩니다.

[![템플릿 기반 TabbedPage의 스크린샷(iOS 및 Android)](tabbed-page-images/tabbedpage-template.png "템플릿 기반 TabbedPage")](tabbed-page-images/tabbedpage-template-large.png#lightbox "템플릿 기반 TabbedPage")

다른 탭을 선택하면 해당 탭을 나타내는 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체가 표시됩니다.

## <a name="related-links"></a>관련 링크

- [TabbedPageWithNavigationPage(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage)
- [TabbedPage(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage)
- [계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [페이지 종류](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPage API](xref:Xamarin.Forms.TabbedPage)
