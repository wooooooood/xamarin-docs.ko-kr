---
title: Xamarin.Forms 마스터-세부 페이지
description: Xamarin.Forms MasterDetailPage에 두 관련된 페이지의 정보 항목을 표시 하는 마스터 페이지 및 마스터 페이지의 항목에 대 한 세부 정보를 제공 하는 세부 정보 페이지를 관리 하는 페이지입니다. 이 문서는 MasterDetailPage 사용 정보는 페이지 사이 이동 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: a3d0edbd933339ee8b8a0a277a4f2493cc8dc70e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997467"
---
# <a name="xamarinforms-master-detail-page"></a>Xamarin.Forms 마스터-세부 페이지

_Xamarin.Forms MasterDetailPage에 두 관련된 페이지의 정보 항목을 표시 하는 마스터 페이지 및 마스터 페이지의 항목에 대 한 세부 정보를 제공 하는 세부 정보 페이지를 관리 하는 페이지입니다. 이 문서는 MasterDetailPage 사용 정보는 페이지 사이 이동 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

마스터 페이지는 다음 스크린샷과에서 같이 항목의 목록을 일반적으로 표시 됩니다.

[![](master-detail-page-images/masterpage-components.png "마스터 페이지 구성 요소")](master-detail-page-images/masterpage-components-large.png#lightbox "마스터 페이지 구성 요소")

목록 항목의 위치는 각 플랫폼에서 동일 하 고 해당 세부 정보 페이지로 이동 됩니다 항목 중 하나를 선택 합니다. 또한 마스터 페이지에는 또한 active 세부 정보 페이지로 이동 하는 단추가 있는 탐색 모음 기능:

- IOS에서 탐색 모음 페이지의 맨 위에 있는 있고 세부 정보 페이지로 이동 하는 단추가 있습니다. 또한 마스터 페이지 왼쪽으로 살짝 밀어 active 세부 정보 페이지를 탐색할 수 있습니다.
- Android에서 탐색 모음 페이지의 맨 위에 있는 하 고 세부 정보 페이지는 제목, 아이콘 및 탐색 단추를 표시 합니다. 아이콘에 정의 되어는 `[Activity]` 를 데코레이팅하는 특성을 `MainActivity` Android 플랫폼 특정 프로젝트에서 클래스입니다. 또한 현재 세부 정보 페이지를 탐색할 수도 있습니다 마스터 페이지 왼쪽으로 살짝 밀어, 화면의 맨 오른쪽 세부 정보 페이지를 탭 하 여 및 탭 하 여 합니다 *다시* 화면의 아래쪽 단추입니다.
- Windows 플랫폼 (UWP (유니버설), 탐색 모음 페이지의 맨 위에 있는 있고 세부 정보 페이지로 이동 하는 단추가 있습니다.

항목에 해당 하는 세부 정보 페이지 표시 데이터 마스터 페이지에서 선택한 세부 정보 페이지의 주요 구성 요소는 다음 스크린샷에 표시 됩니다.

![](master-detail-page-images/detailpage-components.png "세부 정보 페이지 구성 요소")

세부 정보 페이지에 내용이 플랫폼에 종속 된 탐색 모음

- IOS에서 탐색 모음 페이지의 맨 위에 있는 및 제목 표시 있고 단추가 마스터 페이지를 반환 하는 세부 정보 페이지 인스턴스 래핑됩니다 합니다 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 인스턴스. 또한 마스터 페이지 세부 정보 페이지를 오른쪽을 살짝 밀어를 반환할 수 있습니다.
- Android에서 탐색 모음 페이지의 맨 위에 있는 하 고 제목, 아이콘 및 마스터 페이지를 반환 하는 단추를 표시 합니다. 아이콘에 정의 되어는 `[Activity]` 를 데코레이팅하는 특성을 `MainActivity` Android 플랫폼 특정 프로젝트에서 클래스입니다.
- UWP에서 탐색 모음 제목을 표시 및 마스터 페이지를 반환 하는 단추가 있는 페이지의 맨 위에 있는 합니다.

### <a name="navigation-behavior"></a>탐색 동작

마스터 및 세부 정보 페이지 간에 탐색 환경의 동작은 플랫폼에 따라 다릅니다.

- Ios의 경우 세부 정보 페이지 *슬라이드* 페이지 왼쪽에서 왼쪽된 부분 세부 정보에서 마스터 페이지 슬라이드도 오른쪽에 계속 표시 됩니다.
- 세부 정보 및 마스터 페이지에 Android *겹쳐진* 서로 합니다.
- 세부 정보 및 마스터 페이지에 UWP *교환*합니다.

IOS 및 Android에서 마스터 페이지에 비슷한 폭 세로 모드에서 마스터 페이지와 세부 정보 페이지 자세히 볼 수 있도록 한다는 가로 모드로 비슷한 동작이 관찰 됩니다.

탐색 동작을 제어 하는 방법에 대 한 내용은 [세부 정보 페이지 표시 동작을 제어](#Controlling_the_Detail_Page_Display_Behavior)입니다.

## <a name="creating-a-masterdetailpage"></a>MasterDetailPage 만들기

A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 포함 [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) 고 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) 형식 둘 다를 수 있는 속성 [ `Page` ](xref:Xamarin.Forms.Page)를 가져오고 마스터 및 세부 정보 페이지를 각각 설정 하는 데 사용 되는 합니다.

> [!IMPORTANT]
> A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 루트 페이지 되도록 설계 되었습니다 및으로 상태에서 다른 페이지 형식에서 자식 페이지를 예기치 않은 없고 일관 되지 않은 동작이 발생할 수 있습니다. 뿐만 아니라는 것이 좋습니다는의 마스터 페이지는 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 은 항상를 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 인스턴스 및 세부 정보 페이지를 사용 하 여 채워진만 해야 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)하십시오 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), 및 `ContentPage` 인스턴스. 이 모든 플랫폼에서 일관 된 사용자 환경을 보장 하는 데 도움이 됩니다.

다음 XAML 코드 예제와 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 로 설정 하는 [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) 하 고 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) 속성:

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

다음 코드 예제에서는 해당 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) C#에서 만든:

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

합니다 [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) 속성을 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 인스턴스. [ `MasterDetailPage.Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) 속성을 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 포함 하는 `ContentPage` 인스턴스.

### <a name="creating-the-master-page"></a>마스터 페이지 만들기

다음 XAML 코드 예제에서는의 선언을 보여 줍니다.는 `MasterPage` 를 통해 참조 되는 개체를 [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) 속성:

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

구성 페이지는 [ `ListView` ](xref:Xamarin.Forms.ListView) 설정 하 여 XAML에서 데이터도 채워진 해당 [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 배열의 속성 `MasterPageItem` 인스턴스. 각 `MasterPageItem` 정의 `Title`하십시오 `IconSource`, 및 `TargetType` 속성입니다.

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 에 할당 되는 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 속성을 표시 하는 각 `MasterPageItem`. `DataTemplate` 포함을 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) 으로 구성 된는 [ `Image` ](xref:Xamarin.Forms.Image) 와 [ `Label` ](xref:Xamarin.Forms.Label)합니다. [ `Image` ](xref:Xamarin.Forms.Image) 표시를 `IconSource` 속성 값 및 [ `Label` ](xref:Xamarin.Forms.Label) 표시 합니다 `Title` 각각에 대 한 속성 값을 `MasterPageItem`합니다.

페이지에 해당 [ `Title` ](xref:Xamarin.Forms.Page.Title) 하 고 [ `Icon` ](xref:Xamarin.Forms.Page.Icon) 속성 집합입니다. 세부 정보 페이지는 제목 표시줄에 아이콘 세부 정보 페이지에 표시 됩니다. 이에 세부 정보 페이지 인스턴스를 배치 하 여 iOS에서 활성화 되어야 합니다는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 인스턴스.

> [!NOTE]
> 합니다 [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) 페이지에 있어야 해당 [ `Title` ](xref:Xamarin.Forms.Page.Title) 속성을 설정 또는 예외가 발생 합니다.

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

다음 스크린샷에서 각 플랫폼에서 마스터 페이지를 표시합니다.

![](master-detail-page-images/masterpage.png "마스터 페이지 예제")

### <a name="creating-and-displaying-the-detail-page"></a>만들기 및 세부 정보 페이지 표시

`MasterPage` 인스턴스가 포함을 `ListView` 노출 하는 속성 해당 [ `ListView` ](xref:Xamarin.Forms.ListView) 인스턴스 있도록를 `MainPage` [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 인스턴스를 등록할 수는 이벤트 처리기를 처리 하는 [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트입니다. 이 통해는 `MainPage` 인스턴스를 설정 합니다 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) 속성을 나타내는 선택한 페이지를 `ListView` 항목. 다음 코드 예제에서는 이벤트 처리기를 보여 줍니다.

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

- 검색 된 [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) 에서 [ `ListView` ](xref:Xamarin.Forms.ListView) 인스턴스를 선택한 것을 제공 `null`, 세부 정보 페이지를 에저장된종류의새인스턴스를설정합니다`TargetType`의 속성을 `MasterPageItem`입니다. 페이지 형식에 래핑됩니다를 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 아이콘을 통해 참조 하도록 인스턴스를 [ `Icon` ](xref:Xamarin.Forms.Page.Icon) 속성에는 `MasterPage` iOS에서 세부 정보 페이지에 표시 됩니다.
- 선택한 항목을 [ `ListView` ](xref:Xamarin.Forms.ListView) 로 설정 되어 `null` 하나도 되도록를 `ListView` 항목을 선택할 수는 다음 시간을 `MasterPage` 표시 됩니다.
- 세부 정보 페이지를 설정 하 여 사용자에 게 표시 되는 [ `MasterDetailPage.IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) 속성을 `false`입니다. 이 속성의 마스터 / 세부 정보 페이지 표시 되 고 있는지 여부를 제어 합니다. 로 설정 해야 `true` 마스터 페이지를 표시 하려면 및 `false` 세부 정보 페이지를 표시 합니다.

다음 스크린샷에서 표시 된 `ContactPage` 마스터 페이지에서 선택 된 된 후 표시 되는 세부 정보 페이지에서:

![](master-detail-page-images/detailpage.png "세부 정보 페이지 예제")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>세부 정보 페이지 표시 동작 제어

하는 방법을 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 마스터 및 세부 정보 페이지를 관리 합니다. 휴대폰 또는 태블릿, 장치, 방향 및 값에 응용 프로그램의 실행 여부에 따라 달라 집니다 합니다 [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) 속성입니다. 이 속성 세부 정보 페이지는 표시 하는 방법을 결정 합니다. 가능한 값은

- **기본** – 플랫폼 기본값을 사용 하는 페이지가 표시 됩니다.
- **팝 오버** – 세부 정보 페이지에서 또는 마스터 페이지를 부분적으로 검사 합니다.
- **분할** – 마스터 페이지 왼쪽에 표시 되 고 세부 정보 페이지 오른쪽에 표시 됩니다.
- **SplitOnLandscape** – 분할 화면 가로 방향으로 장치가 있을 때 사용 됩니다.
- **SplitOnPortrait** – 장치가 세로 방향으로 때 화면 분할이 사용 됩니다.

다음 XAML 코드 예제에서는 설정 하는 방법에 설명 합니다 [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) 속성을 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

다음 코드 예제에서는 해당 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) C#에서 만든:

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

그러나 값을 [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) 속성 태블릿 또는 데스크톱에서 실행 중인 응용 프로그램에만 영향을 줍니다. 항상 휴대폰에서 실행 되는 응용 프로그램에는 *팝 오버* 동작 합니다.

## <a name="summary"></a>요약

이 문서에 사용 하는 방법을 보여 줍니다는 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 정보의 페이지 사이 이동 합니다. Xamarin.Forms `MasterDetailPage` 항목을 표시 하는 마스터 페이지 및 마스터 페이지의 항목에 대 한 세부 정보를 제공 하는 세부 정보 페이지 두 페이지의 관련된 정보를 관리 하는 페이지입니다.


## <a name="related-links"></a>관련 링크

- [페이지 종류](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](xref:Xamarin.Forms.MasterDetailPage)
