---
title: Xamarin.Forms 마스터-세부 페이지
description: Xamarin.Forms MasterDetailPage는 두 개의 관련된 페이지의 정보-항목 수를 표시 하는 마스터 페이지 및 마스터 페이지에서 항목에 대 한 세부 정보를 표시 하는 세부 정보 페이지를 관리 하는 페이지입니다. 이 문서는 MasterDetailPage를 사용 하 고 해당 페이지의 정보 사이 이동 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 46fa32fc8203b32378f4a4fbe07cb8c9f8dbb854
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209208"
---
# <a name="xamarinforms-master-detail-page"></a>Xamarin.Forms 마스터-세부 페이지

_Xamarin.Forms MasterDetailPage는 두 개의 관련된 페이지의 정보-항목 수를 표시 하는 마스터 페이지 및 마스터 페이지에서 항목에 대 한 세부 정보를 표시 하는 세부 정보 페이지를 관리 하는 페이지입니다. 이 문서는 MasterDetailPage를 사용 하 고 해당 페이지의 정보 사이 이동 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

마스터 페이지는 일반적으로 다음 스크린샷에서 같이 항목의 목록을 표시 합니다.

[![](master-detail-page-images/masterpage-components.png "마스터 페이지 구성 요소")](master-detail-page-images/masterpage-components-large.png#lightbox "마스터 페이지 구성 요소")

목록 항목의 위치는 각 플랫폼에서 동일 하 고 항목 중 하나를 선택 하는 페이지로 이동 하 여 해당 세부 정보. 또한 마스터 페이지에는 또한 페이지로 이동 하 여 현재 세부 정보를 사용할 수 있는 단추가 포함 된 탐색 모음을 기능은 다음과 같습니다.

- Ios에서 탐색 모음 페이지 맨 앞에 있어야 이며 세부 정보 페이지를 탐색 하는 단추가 있습니다. 또한 마스터 페이지 왼쪽으로 살짝 밀면 활성 세부 정보 페이지를 탐색할 수도 있습니다.
- Android에서는 탐색 모음이 페이지 맨 앞에 있어야 하 고 세부 정보 페이지에는 제목, 아이콘 및 탐색 단추를 표시 합니다. 아이콘에 정의 된는 `[Activity]` 를 데코레이팅하는 특성의 `MainActivity` Android 플랫폼 관련 프로젝트의 클래스. 또한 현재 세부 정보 페이지를 탐색할 수도 있습니다 마스터 페이지 왼쪽으로 살짝 밀면, 화면 맨 오른쪽 세부 정보 페이지를 탭 하 여 및 탭 하 여는 *다시* 화면 맨 아래에 단추입니다.
- 에 플랫폼 UWP (유니버설 Windows), 탐색 모음 페이지 맨 앞에 있어야 이며 세부 정보 페이지를 탐색 하는 단추가 있습니다.

페이지, 마스터에서 선택한 항목에 해당 하는 세부 정보 페이지 표시 데이터 및 세부 정보 페이지의 주요 구성 요소는 다음 스크린샷에 표시 됩니다.

![](master-detail-page-images/detailpage-components.png "세부 정보 페이지 구성 요소")

세부 정보 페이지 내용이 플랫폼별 탐색 모음을 포함 되어 있습니다.

- Ios, 탐색 모음 페이지 맨 위에 있는 및 제목을 표시 됩니다. 그리고에 마스터 페이지를 반환 하는 단추 세부 정보 페이지 인스턴스가 요소에 래핑되는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 인스턴스. 또한 마스터 페이지 오른쪽 세부 정보 페이지를 살짝 밀어 반환 될 수 있습니다.
- Android에서는 탐색 모음 페이지의 위쪽에 있고 제목, 아이콘 및 마스터 페이지를 반환 하는 단추가 표시 됩니다. 아이콘에 정의 된는 `[Activity]` 를 데코레이팅하는 특성의 `MainActivity` Android 플랫폼 관련 프로젝트의 클래스.
- UWP에서 탐색 모음이 페이지 맨 앞에 있어야 하 고 제목, 하며 표시 마스터 페이지를 반환 하는 단추 합니다.

### <a name="navigation-behavior"></a>탐색 동작

마스터 및 세부 정보 페이지 간에 탐색 환경을 동작은 플랫폼에 따라 다릅니다.

- Ios, 세부 정보 페이지 *슬라이드* 페이지 세부 정보의 왼쪽된 부분을 왼쪽에서 오른쪽 마스터 페이지 슬라이드도 계속 표시 됩니다.
- Android에서는 세부 데이터와 마스터 페이지는 *오버레이할* 서로 합니다.
- UWP, 세부 정보 및 마스터 페이지는 *교환*합니다.

IOS 및 Android에서 마스터 페이지에 비슷한 너비 세로 모드의 마스터 페이지로 있으므로 더 세부 정보 페이지 표시 됩니다는 점을 제외 하 고 가로 모드로 비슷한 동작 관찰 됩니다.

탐색 동작을 제어 하는 방법에 대 한 정보를 참조 하십시오. [세부 정보 페이지 표시 동작 제어](#Controlling_the_Detail_Page_Display_Behavior)합니다.

## <a name="creating-a-masterdetailpage"></a>MasterDetailPage 만들기

A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 포함 [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) 및 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) 속성 형식의 둘 다에 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)를 가져오고 마스터 및 세부 정보 페이지를 각각 설정 하는 데 사용 되는 합니다.

> [!IMPORTANT]
> A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 는 루트 페이지 되도록 설계 하 고 예기치 않은 일치 하지 않는 동작이 발생할 수 다른 페이지 종류에서 자식 페이지를 사용 합니다. 또한는 것이 좋습니다 하의 마스터 페이지는 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 은 항상 한 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 인스턴스 및 세부 정보 페이지를 채울 수만 해야 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), 및 `ContentPage` 인스턴스. 이 모든 플랫폼에서 일관 된 사용자 환경을 위해 도움이 됩니다.

다음 XAML 코드 예제는 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 로 설정 하는 [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) 및 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) 속성:

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  xmlns:local="clr-namespace:MasterDetailPageNavigation;assembly=MasterDetailPageNavigation"
                  x:Class="MasterDetailPageNavigation.MainPage">
    <MasterDetailPage.Master>
        <local:MasterPage x:Name="masterPage" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage>
            <x:Arguments>
                <local:ContactsPage />
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

다음 코드 예제에 해당 하는 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) C#에서 만들어진:

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        masterPage = new MasterPageCS ();
        Master = masterPage;
        Detail = new NavigationPage (new ContactsPageCS ());
        ...
    }
    ...
}
```

[ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) 속성이로 설정 되는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 인스턴스. [ `MasterDetailPage.Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) 속성이로 설정 되는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 포함 하는 `ContentPage` 인스턴스.

### <a name="creating-the-master-page"></a>마스터 페이지 만들기

다음 XAML 코드 예제에서는의 선언을 보여 줍니다.는 `MasterPage` 통해 참조 되는 개체는 [ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) 속성:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             Icon="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView" x:FieldModifier="public">
           <ListView.ItemsSource>
                <x:Array Type="{x:Type local:MasterPageItem}">
                    <local:MasterPageItem Title="Contacts" IconSource="contacts.png" TargetType="{x:Type local:ContactsPage}" />
                    <local:MasterPageItem Title="TodoList" IconSource="todo.png" TargetType="{x:Type local:TodoListPage}" />
                    <local:MasterPageItem Title="Reminders" IconSource="reminders.png" TargetType="{x:Type local:ReminderPage}" />
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid Padding="5,10">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="30"/>
                                <ColumnDefinition Width="*" />
                            </Grid.ColumnDefinitions>
                            <Image Source="{Binding IconSource}" />
                            <Label Grid.Column="1" Text="{Binding Title}" />
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

페이지는 한 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 있는 데이터로 채워지는 XAML에서 설정 하 여 해당 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) 속성의 배열에 `MasterPageItem` 인스턴스. 각 `MasterPageItem` 정의 `Title`, `IconSource`, 및 `TargetType` 속성입니다.

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 에 할당 되는 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) 속성 각 표시을 `MasterPageItem`합니다. `DataTemplate` 포함 한 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 을 구성 하는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 및 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)합니다. [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 표시는 `IconSource` 속성 값 및 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 표시는 `Title` 각각에 대해 속성 값을 `MasterPageItem`합니다.

페이지에 해당 [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) 및 [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) 속성 집합입니다. 세부 정보 페이지에 제목 표시줄 세부 정보 페이지에는 아이콘이 표시 됩니다. 세부 정보 페이지에서 인스턴스를 배치 하 여 iOS에서 활성화 해야이 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 인스턴스.

> [!NOTE]
> [ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) 페이지 있어야 해당 [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) 속성을 설정 또는 예외가 발생 합니다.

다음 코드 예제에서는 C#에서 만들어진 해당 페이지를 보여 줍니다.

```csharp
public class MasterPageCS : ContentPage
{
  public ListView ListView { get { return listView; } }

  ListView listView;

  public MasterPageCS ()
  {
    var masterPageItems = new List<MasterPageItem> ();
    masterPageItems.Add (new MasterPageItem {
      Title = "Contacts",
      IconSource = "contacts.png",
      TargetType = typeof(ContactsPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "TodoList",
      IconSource = "todo.png",
      TargetType = typeof(TodoListPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "Reminders",
      IconSource = "reminders.png",
      TargetType = typeof(ReminderPageCS)
    });

    listView = new ListView {
      ItemsSource = masterPageItems,
      ItemTemplate = new DataTemplate (() => {
        var grid = new Grid { Padding = new Thickness(5, 10) };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(30) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });

        var image = new Image();
        image.SetBinding(Image.SourceProperty, "IconSource");
        var label = new Label { VerticalOptions = LayoutOptions.FillAndExpand };
        label.SetBinding(Label.TextProperty, "Title");

        grid.Children.Add(image);
        grid.Children.Add(label, 1, 0);

        return new ViewCell { View = grid };
      }),
      SeparatorVisibility = SeparatorVisibility.None
    };

    Icon = "hamburger.png";
    Title = "Personal Organiser";
    Content = new StackLayout
    {
      Children = { listView }
    };
  }
}
```

다음 스크린샷에서 각 플랫폼에서 마스터 페이지를 보여 줍니다.

![](master-detail-page-images/masterpage.png "마스터 페이지 예제")

### <a name="creating-and-displaying-the-detail-page"></a>만들기 및 세부 정보 페이지 표시

`MasterPage` 인스턴스에 포함 되어는 `ListView` 속성을 노출 하는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 인스턴스 있도록는 `MainPage` [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 인스턴스를 등록할 수는 처리할 이벤트 처리기는 [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) 이벤트입니다. 이 통해는 `MainPage` 설정 하는 인스턴스는 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) 속성을 나타내는 선택한 페이지를 `ListView` 항목입니다. 다음 코드 예제에서는 이벤트 처리기를 보여 줍니다.

```csharp
public partial class MainPage : MasterDetailPage
{
    public MainPage ()
    {
        ...
        masterPage.listView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as MasterPageItem;
        if (item != null) {
            Detail = new NavigationPage ((Page)Activator.CreateInstance (item.TargetType));
            masterPage.listView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

`OnItemSelected` 메서드는 다음 작업을 수행 합니다.

- 검색은 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) 에서 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 인스턴스를 선택한 아닌지 제공 `null`, 세부 정보 페이지는 에저장된종류의새인스턴스를설정하는`TargetType`의 속성은 `MasterPageItem`합니다. 페이지 형식은에 래핑됩니다는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 통해 참조 아이콘 되도록 인스턴스는 [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) 속성에는 `MasterPage` iOS에서 세부 정보 페이지에 표시 됩니다.
- 선택한 항목의 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 로 설정 된 `null` 중 되도록는 `ListView` 항목 다음에 선택 됩니다는 `MasterPage` 표시 됩니다.
- 설정 하 여 세부 정보 페이지는 사용자에 게 표시 되는 [ `MasterDetailPage.IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) 속성을 `false`합니다. 이 속성의 마스터 / 세부 정보 페이지 표시 됩니다 있는지 여부를 제어 합니다. 로 설정 해야 `true` 마스터 페이지를 표시 하 고 `false` 세부 정보 페이지를 표시 합니다.

다음 스크린 샷 표시는 `ContactPage` 마스터 페이지에서 선택 된 후 표시 된 세부 정보 페이지:

![](master-detail-page-images/detailpage.png "세부 정보 페이지 예제")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>세부 정보 페이지 표시 동작 제어

방법을 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 마스터 및 세부 정보 페이지 관리 휴대폰 또는 태블릿, 장치, 방향 및의 값에 응용 프로그램의 실행 여부에 따라 달라 집니다는 [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) 속성입니다. 이 속성 세부 정보 페이지는 표시 하는 방법을 결정 합니다. 가능한 값은 같습니다.

- **기본** – 플랫폼 기본값을 사용 하 여 페이지가 표시 됩니다.
- **Popover** -세부 정보 페이지를 포함 또는 마스터 페이지에 일부 설명 합니다.
- **분할** – 마스터 페이지 왼쪽에 표시 되 고 세부 정보 페이지 오른쪽에 표시 됩니다.
- **SplitOnLandscape** – 분할 된 화면 가로 방향으로 되었을 때 사용 됩니다.
- **SplitOnPortrait** -화면 분할 장치가 세로 방향으로 때 사용 됩니다.

다음 XAML 코드 예제에서는 설정 하는 방법을 보여 줍니다.는 [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) 속성에는 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

다음 코드 예제에 해당 하는 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) C#에서 만들어진:

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        MasterBehavior = MasterBehavior.Popover;
        ...
    }
}
```

그러나의 값은 [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) 속성 태블릿 또는 바탕 화면에서 실행 되는 응용 프로그램에만 영향을 줍니다. 항상 휴대폰에서 실행 되는 응용 프로그램에는 *Popover* 동작 합니다.

## <a name="summary"></a>요약

이 문서에 사용 하는 방법을 보여 주는 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 정보의 페이지 사이 이동 합니다. Xamarin.Forms는 `MasterDetailPage` 항목 수를 표시 하는 마스터 페이지 및 마스터 페이지에서 항목에 대 한 세부 정보를 표시 하는 세부 정보 페이지 관련된 정보 –의 두 페이지를 관리 하는 페이지입니다.


## <a name="related-links"></a>관련 링크

- [페이지 변형](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)
