---
title: Xamarin.Forms 마스터-세부 정보 페이지
description: Xamarin.Forms MasterDetailPage는 두 개의 관련 정보 페이지를 관리하는 페이지입니다. 이러한 페이지는 항목을 표시하는 마스터 페이지와 이 페이지의 항목에 대한 세부 정보를 표시하는 세부 정보 페이지입니다. 이 문서에서는 MasterDetailPage를 사용하는 방법과 정보 페이지 간에 이동하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 3bfb8a10eab1a8a75a3f2048de1ce219df9bde66
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057667"
---
# <a name="xamarinforms-master-detail-page"></a>Xamarin.Forms 마스터-세부 정보 페이지

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)

_Xamarin.Forms MasterDetailPage는 두 개의 관련 정보 페이지를 관리하는 페이지입니다. 이러한 페이지는 항목을 표시하는 마스터 페이지와 이 페이지의 항목에 대한 세부 정보를 표시하는 세부 정보 페이지입니다. 이 문서에서는 MasterDetailPage를 사용하는 방법과 정보 페이지 간에 이동하는 방법을 설명합니다._

## <a name="overview"></a>개요

마스터 페이지는 일반적으로 다음 스크린샷과 같이 항목 목록을 표시합니다.

[![](master-detail-page-images/masterpage-components.png "마스터 페이지 구성 요소")](master-detail-page-images/masterpage-components-large.png#lightbox "마스터 페이지 구성 요소")

항목 목록의 위치는 각 플랫폼에서 동일하며, 항목 중 하나를 선택하면 해당 세부 정보 페이지로 이동합니다. 또한 마스터 페이지에는 활성 세부 정보 페이지로 이동하는 데 사용할 수 있는 단추가 포함된 탐색 모음이 있습니다.

- iOS의 경우 탐색 모음이 페이지의 위쪽에 있으며, 여기에는 세부 정보 페이지로 이동하는 단추가 있습니다. 또한 마스터 페이지를 왼쪽으로 살짝 밀어 활성 세부 정보 페이지로 이동할 수 있습니다.
- Android의 경우 탐색 모음이 페이지의 위쪽에 있으며, 여기에는 제목, 아이콘 및 세부 정보 페이지로 이동하는 단추가 표시됩니다. 아이콘은 Android 플랫폼별 프로젝트에서 `MainActivity` 클래스를 데코레이팅하는 `[Activity]` 특성에 정의됩니다. 또한 마스터 페이지를 왼쪽으로 살짝 밀거나, 화면의 오른쪽 끝에 있는 세부 정보 페이지를 탭하거나, 화면의 아래쪽에 있는 *뒤로* 단추를 탭하여 활성 세부 정보 페이지로 이동할 수 있습니다.
- UWP(유니버설 Windows 플랫폼)의 경우 탐색 모음이 페이지의 위쪽에 있으며, 여기에는 세부 정보 페이지로 이동하는 단추가 있습니다.

세부 정보 페이지에는 마스터 페이지에서 선택한 항목에 해당하는 데이터가 표시되고, 세부 정보 페이지의 주요 구성 요소는 다음 스크린샷에 표시됩니다.

![](master-detail-page-images/detailpage-components.png "세부 정보 페이지 구성 요소")

세부 정보 페이지에는 콘텐츠가 플랫폼에 따라 달라지는 탐색 모음이 있습니다.

- iOS의 경우 탐색 모음이 페이지의 위쪽에 있으며, 여기에는 제목이 표시되고, 세부 정보 페이지 인스턴스를 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 인스턴스에 래핑하는 경우 마스터 페이지를 반환하는 단추가 있습니다. 또한 세부 정보 페이지를 오른쪽으로 살짝 밀어 마스터 페이지를 반환할 수 있습니다.
- Android의 경우 탐색 모음이 페이지의 위쪽에 있으며, 여기에는 제목, 아이콘 및 마스터 페이지를 반환하는 단추가 표시됩니다. 아이콘은 Android 플랫폼별 프로젝트에서 `MainActivity` 클래스를 데코레이팅하는 `[Activity]` 특성에 정의됩니다.
- UWP의 경우 탐색 모음이 페이지의 위쪽에 있으며, 여기에는 제목이 표시되고, 마스터 페이지를 반환하는 단추가 있습니다.

### <a name="navigation-behavior"></a>탐색 동작

마스터 페이지와 세부 정보 페이지 간의 탐색 환경 동작은 플랫폼에 따라 다릅니다.

- iOS의 경우 마스터 페이지를 왼쪽에서 밀면 세부 정보 페이지가 오른쪽으로 *밀리고* 세부 정보 페이지의 왼쪽 부분이 계속 표시됩니다.
- Android의 경우 세부 정보 페이지와 마스터 페이지가 서로 *겹쳐집니다*.
- UWP의 경우 세부 정보 페이지와 마스터 페이지가 *교환*됩니다.

iOS와 Android의 마스터 페이지가 세로 모드의 마스터 페이지와 비슷한 너비를 갖고 있으므로 더 많은 세부 페이지가 표시된다는 점을 제외하고는 가로 모드에서도 이와 비슷한 동작이 관찰됩니다.

탐색 동작을 제어하는 방법에 대한 자세한 내용은 [세부 정보 페이지의 표시 동작 제어](#Controlling_the_Detail_Page_Display_Behavior)를 참조하세요.

## <a name="creating-a-masterdetailpage"></a>MasterDetailPage 만들기

[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)에는 각각 마스터 페이지와 세부 정보 페이지를 가져오고 설정하는 데 사용되는 [`Page`](xref:Xamarin.Forms.Page) 형식의 [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) 및 [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) 속성이 모두 포함됩니다.

> [!IMPORTANT]
> [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)는 루트 페이지로 설계되었으며, 이 페이지를 다른 페이지 형식의 자식 페이지로 사용하면 예기치 않고 일관되지 않은 동작이 발생할 수 있습니다. 또한 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)의 마스터 페이지는 항상 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스여야 하며 세부 정보 페이지는 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage), [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 및 `ContentPage` 인스턴스로 채워야 하는 것이 좋습니다. 이렇게 하면 모든 플랫폼에서 일관된 사용자 환경을 보장하는 데 도움이 됩니다.

다음 XAML 코드 예제에서는 [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) 및 [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) 속성을 설정하는 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)를 보여 줍니다.

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

다음 코드 예제에서는 C#에서 만든 해당 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)를 보여 줍니다.

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

[`MasterDetailPage.Master`](xref:Xamarin.Forms.MasterDetailPage.Master) 속성이 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스로 설정됩니다. [`MasterDetailPage.Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) 속성이 `ContentPage` 인스턴스가 포함된 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)로 설정됩니다.

### <a name="creating-the-master-page"></a>마스터 페이지 만들기

다음 XAML 코드 예제에서는 [`MasterDetailPage.Master`](xref:Xamarin.Forms.MasterDetailPage.Master) 속성을 통해 참조되는 `MasterPage` 개체의 선언을 보여 줍니다.

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

이 페이지는 해당 [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 속성을 `MasterPageItem` 인스턴스의 배열로 설정하여 XAML의 데이터로 채워지는 [`ListView`](xref:Xamarin.Forms.ListView)로 구성됩니다. 각 `MasterPageItem`은 `Title`, `IconSource` 및 `TargetType` 속성을 정의합니다.

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)은 각각의 `MasterPageItem`을 표시하기 위해 [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 속성에 할당됩니다. `DataTemplate`에는 [`Image`](xref:Xamarin.Forms.Image) 및 [`Label`](xref:Xamarin.Forms.Label)로 구성된 [`ViewCell`](xref:Xamarin.Forms.ViewCell)이 포함됩니다. [`Image`](xref:Xamarin.Forms.Image)에는 `IconSource` 속성 값이 표시되고, [`Label`](xref:Xamarin.Forms.Label)에는 각 `MasterPageItem`에 대한 `Title` 속성 값이 표시됩니다.

이 페이지에는 해당 [`Title`](xref:Xamarin.Forms.Page.Title) 및 [`Icon`](xref:Xamarin.Forms.Page.Icon) 속성이 설정되어 있습니다. 세부 정보 페이지에 제목 표시줄이 있으면 아이콘이 세부 정보 페이지에 표시됩니다. 세부 정보 페이지 인스턴스를 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 인스턴스에 래핑하여 iOS에서 이를 활성화해야 합니다.

> [!NOTE]
> [`MasterDetailPage.Master`](xref:Xamarin.Forms.MasterDetailPage.Master) 페이지에는 해당 [`Title`](xref:Xamarin.Forms.Page.Title) 속성이 설정되어 있어야 하며, 그렇지 않으면 예외가 발생합니다.

다음 코드 예제에서는 C#에서 만든 해당 페이지를 보여 줍니다.

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

다음 스크린샷에서는 각 플랫폼의 마스터 페이지를 보여 줍니다.

![](master-detail-page-images/masterpage.png "마스터 페이지 예제")

### <a name="creating-and-displaying-the-detail-page"></a>세부 정보 페이지 만들기 및 표시

`MainPage` [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 인스턴스에서 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트를 처리할 이벤트 처리기를 등록할 수 있도록 `MasterPage` 인스턴스에는 해당 [`ListView`](xref:Xamarin.Forms.ListView) 인스턴스를 공개하는 `ListView` 속성이 포함됩니다. 이렇게 하면 `MainPage` 인스턴스에서 [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) 속성을 선택한 `ListView` 항목을 나타내는 페이지로 설정할 수 있습니다. 다음 코드 예제에서는 이벤트 처리기를 보여 줍니다.

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

`OnItemSelected` 메서드에서 수행하는 작업은 다음과 같습니다.

- [`ListView`](xref:Xamarin.Forms.ListView) 인스턴스에서 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem)을 검색하여 `null`이 아닌 경우 세부 정보 페이지를 `MasterPageItem`의 `TargetType` 속성에 저장된 페이지 형식의 새 인스턴스로 설정합니다. `MasterPage`의 [`Icon`](xref:Xamarin.Forms.Page.Icon) 속성을 통해 참조된 아이콘이 iOS의 세부 정보 페이지에 표시되도록 페이지 형식이 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 인스턴스에 래핑됩니다.
- 다음에 `MasterPage`가 표시되면 `ListView` 항목이 선택되지 않도록 [`ListView`](xref:Xamarin.Forms.ListView)에서 선택한 항목이 `null`로 설정됩니다.
- 세부 정보 페이지는 [`MasterDetailPage.IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented) 속성을 `false`로 설정하여 사용자에게 표시됩니다. 이 속성은 마스터 페이지 또는 세부 정보 페이지의 표시 여부를 제어합니다. 마스터 페이지를 표시하려면 `true`로 설정하고, 세부 정보 페이지를 표시하려면 `false`로 설정해야 합니다.

다음 스크린샷에서는 마스터 페이지에서 선택한 후에 표시되는 `ContactPage` 세부 정보 페이지를 보여 줍니다.

![](master-detail-page-images/detailpage.png "세부 정보 페이지 예제")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>세부 정보 페이지의 표시 동작 제어

[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)에서 마스터 페이지 및 세부 정보 페이지를 관리하는 방법은 애플리케이션이 휴대폰 또는 태블릿에서 실행 중인지 여부, 디바이스의 방향 및 [`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) 속성의 값에 따라 달라진다. 이 속성은 세부 정보 페이지를 표시하는 방법을 결정합니다. 가능한 값은 다음과 같습니다.

- **Default** – 페이지가 플랫폼 기본값을 사용하여 표시됩니다.
- **Popover** – 세부 정보 페이지에서 마스터 페이지의 전체 또는 일부를 덮습니다.
- **Split** – 마스터 페이지가 왼쪽에 표시되고, 세부 정보 페이지는 오른쪽에 표시됩니다.
- **SplitOnLandscape** – 디바이스가 가로 방향일 때 분할 화면이 사용됩니다.
- **SplitOnPortrait** – 디바이스가 세로 방향일 때 분할 화면이 사용됩니다.

다음 XAML 코드 예제에서는 [`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) 속성을 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)에 설정하는 방법을 보여 줍니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

다음 코드 예제에서는 C#에서 만든 해당 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)를 보여 줍니다.

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

그러나 [`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) 속성의 값은 태블릿 또는 데스크톱에서 실행되는 애플리케이션에만 영향을 줍니다. 휴대폰에서 실행되는 애플리케이션에는 항상 *Popover* 동작이 있습니다.

## <a name="summary"></a>요약

이 문서에서는 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)를 사용하고 해당 정보 페이지 간에 이동하는 방법을 보여 주었습니다. Xamarin.Forms `MasterDetailPage`는 두 개의 관련 정보 페이지를 관리하는 페이지입니다. 이러한 페이지는 항목을 표시하는 마스터 페이지와 이 페이지의 항목에 대한 세부 정보를 표시하는 세부 정보 페이지입니다.


## <a name="related-links"></a>관련 링크

- [페이지 종류](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage(샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](xref:Xamarin.Forms.MasterDetailPage)
