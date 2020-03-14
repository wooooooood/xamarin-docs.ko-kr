---
title: Xamarin.ios CollectionView 데이터
description: CollectionView는 System.windows.controls.itemscontrol.itemssource 속성을 IEnumerable을 구현 하는 컬렉션으로 설정 하 여 데이터로 채워집니다.
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: 9442f7878d9290946fabb7bfc5dee77a828228c7
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305932"
---
# <a name="xamarinforms-collectionview-data"></a>Xamarin.ios CollectionView 데이터

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

표시 되는 데이터와 표시 되는 데이터를 정의 하는 다음 속성을 포함 하 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 입니다.

- `IEnumerable`형식의 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)는 표시할 항목의 컬렉션을 지정 하 고 기본값은 `null`입니다.
- [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)형식의 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)는 표시할 항목 컬렉션의 각 항목에 적용할 템플릿을 지정 합니다.

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) 새 항목이 추가 될 때 `CollectionView`의 스크롤 동작을 나타내는 `ItemsUpdatingScrollMode` 속성을 정의 합니다. 이 속성에 대 한 자세한 내용은 [새 항목이 추가 될 때 스크롤 위치 제어](scrolling.md#control-scroll-position-when-new-items-are-added)를 참조 하세요.

사용자가 스크롤하면 데이터를 증분 로드할 수도 [`CollectionView`](xref:Xamarin.Forms.CollectionView) . 자세한 내용은 [데이터를 증분 로드](#load-data-incrementally)를 참조 하세요.

## <a name="populate-a-collectionview-with-data"></a>데이터를 사용 하 여 CollectionView 채우기

[`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성을 `IEnumerable`를 구현 하는 컬렉션으로 설정 하 여 데이터로 채워집니다. 문자열 배열에서 `ItemsSource` 속성을 초기화 하 여 XAML에 항목을 추가할 수 있습니다.

```xaml
<CollectionView>
  <CollectionView.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </CollectionView.ItemsSource>
</CollectionView>
```

> [!NOTE]
> `x:Array` 요소는 배열의 항목 유형을 나타내는 `Type` 특성이 필요합니다.

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView();
collectionView.ItemsSource = new string[]
{
    "Baboon",
    "Capuchin Monkey",
    "Blue Monkey",
    "Squirrel Monkey",
    "Golden Lion Tamarin",
    "Howler Monkey",
    "Japanese Macaque"
};
```

> [!WARNING]
> [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) UI 스레드 외부에서 업데이트 되는 경우 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 예외를 throw 합니다.

기본적으로 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 는 다음 스크린샷에 표시 된 것 처럼 세로 목록에 항목을 표시 합니다.

[![IOS 및 Android에서 텍스트 항목을 포함 하는 CollectionView의 스크린샷](populate-data-images/text.png "CollectionView의 텍스트 항목")](populate-data-images/text-large.png#lightbox "CollectionView의 텍스트 항목")

> [!IMPORTANT]
> 기본 컬렉션에서 항목이 추가, 제거 또는 변경 될 때 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 를 새로 고쳐야 하는 경우 기본 컬렉션은 `ObservableCollection`와 같은 속성 변경 알림을 보내는 `IEnumerable` 컬렉션 이어야 합니다.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 레이아웃을 변경 하는 방법에 대 한 자세한 내용은 [xamarin.ios CollectionView layout](layout.md)을 참조 하세요. `CollectionView`에서 각 항목의 모양을 정의 하는 방법에 대 한 자세한 내용은 [항목 모양 정의](#define-item-appearance)를 참조 하세요.

### <a name="data-binding"></a>데이터 바인딩

데이터 바인딩을 사용 하 여 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성을 `IEnumerable` 컬렉션에 바인딩하면 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 데이터를 채울 수 있습니다. XAML에서이 작업은 `Binding` 태그 확장을 사용 하 여 구현 됩니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

이 예제에서 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성 데이터는 연결 된 viewmodel의 `Monkeys` 속성에 바인딩됩니다.

> [!NOTE]
> 컴파일된 바인딩을 사용하면 Xamarin.Forms 응용 프로그램에서 데이터 바인딩 성능을 향상시킬 수 있습니다. 자세한 내용은 [컴파일된 바인딩](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)을 참조하세요.

데이터 바인딩에 대한 자세한 내용은 [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)을 참조하세요.

## <a name="define-item-appearance"></a>항목 모양 정의

[`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)로 설정 하 여 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 에 있는 각 항목의 모양을 정의할 수 있습니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
    ...
</CollectionView>
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

collectionView.ItemTemplate = new DataTemplate(() =>
{
    Grid grid = new Grid { Padding = 10 };
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });

    Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 60, WidthRequest = 60 };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Label locationLabel = new Label { FontAttributes = FontAttributes.Italic, VerticalOptions = LayoutOptions.End };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Grid.SetRowSpan(image, 2);

    grid.Children.Add(image);
    grid.Children.Add(nameLabel, 1, 0);
    grid.Children.Add(locationLabel, 1, 1);

    return grid;
});
```

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 에 지정 된 요소는 목록에 있는 각 항목의 모양을 정의 합니다. 이 예제에서 `DataTemplate` 내의 레이아웃은 [`Grid`](xref:Xamarin.Forms.Grid)를 통해 관리 됩니다. `Grid`에는 [`Image`](xref:Xamarin.Forms.Image) 개체와 두 개의 [`Label`](xref:Xamarin.Forms.Label) 개체가 포함 되어 있으며,이는 모두 `Monkey` 클래스의 속성에 바인딩됩니다.

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

다음 스크린샷에는 목록의 각 항목에 대 한 템플릿 결과가 나와 있습니다.

[![IOS 및 Android에서 각 항목이 템플릿 인 CollectionView의 스크린샷](populate-data-images/datatemplate.png "CollectionView의 템플릿 항목")](populate-data-images/datatemplate-large.png#lightbox "CollectionView의 템플릿 항목")

데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

## <a name="choose-item-appearance-at-runtime"></a>런타임에 항목 모양 선택

[`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 속성을 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 개체로 설정 하 여 항목 값을 기준으로 런타임에 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 에 있는 각 항목의 모양을 선택할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CollectionViewDemos.Controls">
    <ContentPage.Resources>
        <DataTemplate x:Key="AmericanMonkeyTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="OtherMonkeyTemplate">
            ...
        </DataTemplate>

        <controls:MonkeyDataTemplateSelector x:Key="MonkeySelector"
                                             AmericanMonkey="{StaticResource AmericanMonkeyTemplate}"
                                             OtherMonkey="{StaticResource OtherMonkeyTemplate}" />
    </ContentPage.Resources>

    <CollectionView ItemsSource="{Binding Monkeys}"
                    ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

[`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 속성은 `MonkeyDataTemplateSelector` 개체로 설정 됩니다. 다음 예제에서는 `MonkeyDataTemplateSelector` 클래스를 보여 줍니다.

```csharp
public class MonkeyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate AmericanMonkey { get; set; }
    public DataTemplate OtherMonkey { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Monkey)item).Location.Contains("America") ? AmericanMonkey : OtherMonkey;
    }
}
```

`MonkeyDataTemplateSelector` 클래스는 다른 데이터 템플릿으로 설정 된 `AmericanMonkey` 및 `OtherMonkey` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 속성을 정의 합니다. `OnSelectTemplate` 재정의는 원숭이 이름에 "아메리카"가 포함 된 경우 원숭이의 이름과 위치를 청록색으로 표시 하는 `AmericanMonkey` 템플릿을 반환 합니다. 원숭이 이름에 "아메리카"가 포함 되지 않은 경우 `OnSelectTemplate` 재정의는 원숭이의 원숭이 이름과 위치를 표시 하는 `OtherMonkey` 템플릿을 반환 합니다.

[![IOS 및 Android에서 CollectionView 런타임 항목 템플릿 선택의 스크린샷](populate-data-images/datatemplateselector.png "CollectionView의 런타임 항목 템플릿 선택")](populate-data-images/datatemplateselector-large.png#lightbox "CollectionView의 런타임 항목 템플릿 선택")

데이터 템플릿 선택기에 대 한 자세한 내용은 [DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)를 참조 하세요.

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)사용 하는 경우 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 개체의 루트 요소를 `ViewCell`로 설정 하지 마십시오. 이로 인해 `CollectionView`에는 셀 개념이 없으므로 예외가 throw 됩니다.

## <a name="context-menus"></a>상황에 맞는 메뉴

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 는 `SwipeView`를 통해 데이터 항목에 대 한 상황에 맞는 메뉴를 지원 하며,이 메뉴는 살짝 밀기 제스처로 상황에 맞는 메뉴를 표시 합니다. `SwipeView`는 콘텐츠 항목 주위에 래핑하는 컨테이너 컨트롤이 며 해당 콘텐츠 항목에 대 한 상황에 맞는 메뉴 항목을 제공 합니다. 따라서 상황에 맞는 메뉴는 `SwipeView`에서 래핑하는 콘텐츠를 정의 하는 `SwipeView`을 만들고 살짝 밀기 제스처로 표시 되는 상황에 맞는 메뉴 항목을 만들어 `CollectionView`에 대해 구현 됩니다. 이렇게 하려면 `SwipeView`를 `CollectionView`의 각 데이터 항목의 모양을 정의 하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 의 루트 뷰로 설정 합니다.

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <SwipeView>
                <SwipeView.LeftItems>
                    <SwipeItems>
                        <SwipeItem Text="Favorite"
                                   IconImageSource="favorite.png"
                                   BackgroundColor="LightGreen"
                                   Command="{Binding Source={x:Reference collectionView}, Path=BindingContext.FavoriteCommand}"
                                   CommandParameter="{Binding}" />
                        <SwipeItem Text="Delete"
                                   IconImageSource="delete.png"
                                   BackgroundColor="LightPink"
                                   Command="{Binding Source={x:Reference collectionView}, Path=BindingContext.DeleteCommand}"
                                   CommandParameter="{Binding}" />
                    </SwipeItems>
                </SwipeView.LeftItems>
                <Grid BackgroundColor="White"
                      Padding="10">
                    <!-- Define item appearance -->
                </Grid>
            </SwipeView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

collectionView.ItemTemplate = new DataTemplate(() =>
{
    // Define item appearance
    Grid grid = new Grid { Padding = 10, BackgroundColor = Color.White };
    // ...

    SwipeView swipeView = new SwipeView();
    SwipeItem favoriteSwipeItem = new SwipeItem
    {
        Text = "Favorite",
        IconImageSource = "favorite.png",
        BackgroundColor = Color.LightGreen
    };
    favoriteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.FavoriteCommand", source: collectionView));
    favoriteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    SwipeItem deleteSwipeItem = new SwipeItem
    {
        Text = "Delete",
        IconImageSource = "delete.png",
        BackgroundColor = Color.LightPink
    };
    deleteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.DeleteCommand", source: collectionView));
    deleteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    swipeView.LeftItems = new SwipeItems { favoriteSwipeItem, deleteSwipeItem };
    swipeView.Content = grid;    
    return swipeView;
});
```

이 예제에서 `SwipeView` 내용은 [`CollectionView`](xref:Xamarin.Forms.CollectionView)에서 각 항목의 모양을 정의 하는 [`Grid`](xref:Xamarin.Forms.Grid) 입니다. 살짝 밀기 항목은 `SwipeView` 콘텐츠에 대 한 작업을 수행 하는 데 사용 되며, 왼쪽에서 컨트롤이 스와이프 때 표시 됩니다.

[![IOS 및 Android에서 CollectionView 상황에 맞는 메뉴 항목의 스크린샷](populate-data-images/swipeview.png "SwipeView 상황에 맞는 메뉴 항목을 사용 하는 CollectionView")](populate-data-images/swipeview-large.png#lightbox "SwipeView 상황에 맞는 메뉴 항목을 사용 하는 CollectionView")

`SwipeView`는 `SwipeItems` 개체가 추가 되는 방향 `SwipeItems` 컬렉션에 의해 정의 되는 살짝 밀기 방향을 사용 하는 네 가지 살짝 밀기 방향을 지원 합니다. 기본적으로 사용자가 탭 할 때 살짝 밀기 항목이 실행 됩니다. 또한 살짝 밀기 항목이 실행 된 후에는 살짝 밀기 항목이 숨겨지고 `SwipeView` 콘텐츠가 다시 표시 됩니다. 그러나 이러한 동작은 변경할 수 있습니다.

`SwipeView` 컨트롤에 대 한 자세한 내용은 [Xamarin.ios SwipeView](~/xamarin-forms/user-interface/swipeview.md)를 참조 하세요.

## <a name="pull-to-refresh"></a>새로 고치려면 끌어오기

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 는 `RefreshView`를 통해 기능을 새로 고치는 기능을 지원 합니다 .이를 통해 항목 목록에서 아래로 당겨 데이터를 새로 고칠 수 있습니다. `RefreshView`은 자식에서 스크롤 가능한 콘텐츠를 지 원하는 경우 해당 자식에 대 한 기능을 새로 고치는 가져오기를 제공 하는 컨테이너 컨트롤입니다. 따라서 `RefreshView`의 자식으로 설정 하 여 `CollectionView`에 대 한 끌어오기를 새로 고칩니다.

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <CollectionView ItemsSource="{Binding Animals}">
        ...
    </CollectionView>
</RefreshView>
```

해당 하는 C# 코드가입니다.

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
refreshView.Content = collectionView;
// ...
```

사용자가 새로 고침을 시작 하면 `Command` 속성으로 정의 된 `ICommand` 실행 되어 표시 되는 항목을 새로 고쳐야 합니다. 새로 고침이 발생 하는 동안 애니메이션 처리 원으로 구성 된 새로 고침 시각화가 표시 됩니다.

[![IOS 및 Android에서 CollectionView 끌어오기-새로 고침의 스크린샷](populate-data-images/pull-to-refresh.png "CollectionView 가져오기-새로 고침")](populate-data-images/pull-to-refresh-large.png#lightbox "CollectionView 가져오기-새로 고침")

`RefreshView.IsRefreshing` 속성의 값은 `RefreshView`의 현재 상태를 나타냅니다. 사용자가 새로 고침을 트리거하는 경우이 속성은 자동으로 `true`로 전환 됩니다. 새로 고침이 완료 되 면 속성을 `false`으로 다시 설정 해야 합니다.

`RefreshView`에 대 한 자세한 내용은 [Xamarin.ios RefreshView](~/xamarin-forms/user-interface/refreshview.md)를 참조 하세요.

## <a name="load-data-incrementally"></a>증분 방식으로 데이터 로드

사용자가 항목을 스크롤하면 데이터를 증분 로드 하는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 지원 됩니다. 이렇게 하면 사용자가 스크롤할 때 웹 서비스에서 데이터 페이지를 비동기적으로 로드 하는 등의 시나리오를 사용할 수 있습니다. 또한 더 많은 데이터를 로드 하는 지점은 사용자가 빈 공간을 보거나 스크롤에서 중지 되도록 구성할 수 있습니다.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 는 다음 속성을 정의 하 여 데이터의 증분 로드를 제어 합니다.

- `int`형식의 `RemainingItemsThreshold``RemainingItemsThresholdReached` 이벤트가 발생 하는 목록에 아직 표시 되지 않는 항목의 임계값입니다.
- `RemainingItemsThresholdReachedCommand``RemainingItemsThreshold`에 도달할 때 실행 되는 `ICommand`형식입니다.
- `RemainingItemsThresholdReachedCommandParameter` 형식의 `object` - `RemainingItemsThresholdReachedCommand`에 전달되는 매개 변수입니다.

또한 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 는 `RemainingItemsThreshold` 항목이 표시 되지 않을 만큼 `CollectionView` 스크롤 될 때 발생 하는 `RemainingItemsThresholdReached` 이벤트를 정의 합니다. 이 이벤트를 처리 하 여 더 많은 항목을 로드할 수 있습니다. 또한 `RemainingItemsThresholdReached` 이벤트가 발생 하면 `RemainingItemsThresholdReachedCommand` 실행 되어 viewmodel에서 증분 데이터 로드가 발생 하도록 할 수 있습니다.

`RemainingItemsThreshold` 속성의 기본값은-1입니다 .이 값은 `RemainingItemsThresholdReached` 이벤트가 발생 되지 않음을 나타냅니다. 속성 값이 0 이면 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 의 최종 항목이 표시 될 때 `RemainingItemsThresholdReached` 이벤트가 발생 합니다. 값이 0 보다 큰 경우에는 `ItemsSource`에 아직 스크롤되지 않는 항목 수가 포함 되어 있으면 `RemainingItemsThresholdReached` 이벤트가 발생 합니다.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) 은 `RemainingItemsThreshold` 속성의 유효성을 검사 하 여 해당 값이 항상-1 보다 크거나 같도록 합니다.

다음 XAML 예제에서는 데이터를 증분 로드 하는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 보여 줍니다.

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                RemainingItemsThreshold="5"
                RemainingItemsThresholdReached="OnCollectionViewRemainingItemsThresholdReached">
    ...
</CollectionView>
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    RemainingItemsThreshold = 5
};
collectionView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

이 코드 예제에서는 5 개의 항목이 아직 스크롤되지 않는 경우 `RemainingItemsThresholdReached` 이벤트가 발생 하 고, 응답으로 `OnCollectionViewRemainingItemsThresholdReached` 이벤트 처리기를 실행 합니다.

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> `RemainingItemsThresholdReachedCommand`를 viewmodel의 `ICommand` 구현에 바인딩하여 증분 방식으로 데이터를 로드할 수도 있습니다.

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin.ios RefreshView](~/xamarin-forms/user-interface/refreshview.md)
- [Xamarin.ios SwipeView](~/xamarin-forms/user-interface/swipeview.md)
- [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin. Forms DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
