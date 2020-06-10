---
제목: " Xamarin.Forms CarouselView data" description: "system.windows.controls.itemscontrol.itemssource 속성을 IEnumerable을 구현 하는 컬렉션으로 설정 하 여 CarouselView를 데이터로 채웁니다."
assetid: 20DB2C57-CE3A-4D91-80DC-73AE361A3CB0: xamarin-forms author: davidbritch: dabritch:: 04/29/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-carouselview-data"></a>Xamarin.FormsCarouselView 데이터

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)에는 표시할 데이터 및 모양을 정의 하는 다음 속성이 포함 되어 있습니다.

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)형식의는 `IEnumerable` 표시 될 항목의 컬렉션을 지정 하 고 기본값은 `null` 입니다.
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)형식의는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 표시할 항목 컬렉션의 각 항목에 적용할 템플릿을 지정 합니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)`ItemsUpdatingScrollMode`새 항목이 추가 될 때의 스크롤 동작을 나타내는 속성을 정의 `CarouselView` 합니다. 이 속성에 대 한 자세한 내용은 [새 항목이 추가 될 때 스크롤 위치 제어](scrolling.md#control-scroll-position-when-new-items-are-added)를 참조 하세요.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)사용자가 스크롤하면 증분 데이터 가상화를 지원 합니다. 자세한 내용은 [데이터를 증분 로드](#load-data-incrementally)를 참조 하세요.

## <a name="populate-a-carouselview-with-data"></a>데이터를 사용 하 여 CarouselView 채우기

는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 속성을를 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 구현 하는 컬렉션으로 설정 하 여 데이터로 채워집니다 `IEnumerable` . 문자열 배열에서 속성을 초기화 하 여 XAML에 항목을 추가할 수 있습니다 `ItemsSource` .

```xaml
<CarouselView>
    <CarouselView.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Baboon</x:String>
            <x:String>Capuchin Monkey</x:String>
            <x:String>Blue Monkey</x:String>
            <x:String>Squirrel Monkey</x:String>
            <x:String>Golden Lion Tamarin</x:String>
            <x:String>Howler Monkey</x:String>
            <x:String>Japanese Macaque</x:String>
        </x:Array>
    </CarouselView.ItemsSource>
</CarouselView>
```

> [!NOTE]
> `x:Array` 요소는 배열의 항목 유형을 나타내는 `Type` 특성이 필요합니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.ItemsSource = new string[]
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

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)기본 컬렉션에서 항목이 추가, 제거 또는 변경 될 때를 새로 고쳐야 하는 경우 기본 컬렉션은 `IEnumerable` 와 같은 속성 변경 알림을 보내는 컬렉션 이어야 합니다 `ObservableCollection` .

기본적으로는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 항목을 가로로 표시 합니다. 다음 스크린샷에는 `CarouselView` iOS 및 Android에서 다른 문자열 항목을 표시 하는 방법을 보여 줍니다.

[![IOS 및 Android에서 텍스트 항목을 포함 하는 CarouselView의 스크린샷](populate-data-images/text.png "CarouselView의 텍스트 항목")](populate-data-images/text-large.png#lightbox "CarouselView의 텍스트 항목")

방향을 변경 하는 방법에 대 한 자세한 내용은 [`CarouselView`](xref:Xamarin.Forms.CarouselView) [ Xamarin.Forms CarouselView Layout](layout.md)을 참조 하세요. 에서 각 항목의 모양을 정의 하는 방법에 대 한 자세한 내용은 `CarouselView` [항목 모양 정의](#define-item-appearance)를 참조 하세요.

### <a name="data-binding"></a>데이터 바인딩

[`CarouselView`](xref:Xamarin.Forms.CarouselView)데이터 바인딩을 사용 하 여 속성을 컬렉션에 바인딩하면 데이터를 데이터로 채울 수 있습니다 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `IEnumerable` . XAML에서이 작업은 태그 확장을 사용 하 여 구현 됩니다 `Binding` .

```xaml
<CarouselView ItemsSource="{Binding Monkeys}" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

이 예제에서 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성 데이터는 `Monkeys` 연결 된 viewmodel의 속성에 바인딩됩니다.

> [!NOTE]
> 컴파일된 바인딩을 사용 하면 응용 프로그램에서 데이터 바인딩 성능을 향상 시킬 수 있습니다 Xamarin.Forms . 자세한 내용은 [컴파일된 바인딩](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)을 참조하세요.

데이터 바인딩에 대한 자세한 내용은 [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)을 참조하세요.

## <a name="define-item-appearance"></a>항목 모양 정의

에서 각 항목의 모양은 속성을 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 로 설정 하 여 정의할 수 있습니다 [`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

carouselView.ItemTemplate = new DataTemplate(() =>
{
    Label nameLabel = new Label { ... };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Image image = new Image { ... };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label locationLabel = new Label { ... };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Label detailsLabel = new Label { ... };
    detailsLabel.SetBinding(Label.TextProperty, "Details");

    StackLayout stackLayout = new StackLayout
    {
        Children = { nameLabel, image, locationLabel, detailsLabel }
    };

    Frame frame = new Frame { ... };
    StackLayout rootStackLayout = new StackLayout
    {
        Children = { frame }
    };

    return rootStackLayout;
});
```

에 지정 된 요소는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 에 있는 각 항목의 모양을 정의 합니다 `CarouselView` . 이 예제에서 내의 레이아웃은 `DataTemplate` 에 의해 관리 되 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 고 데이터는 개체와 세 개의 개체를 사용 하 여 표시 됩니다 .이 개체는 [`Image`](xref:Xamarin.Forms.Image) [`Label`](xref:Xamarin.Forms.Label) 모두 클래스의 속성에 바인딩됩니다 `Monkey` .

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

다음 스크린샷은 각 항목의 템플릿 결과를 보여 줍니다.

[![IOS 및 Android에서 각 항목이 템플릿 인 CarouselView의 스크린샷](populate-data-images/datatemplate.png "CarouselView의 템플릿 항목")](populate-data-images/datatemplate-large.png#lightbox "CarouselView의 템플릿 항목")

데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

## <a name="choose-item-appearance-at-runtime"></a>런타임에 항목 모양 선택

에서 각 항목의 모양은 속성을 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 개체로 설정 하 여 항목 값에 따라 런타임에 선택할 수 있습니다 [`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) .

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CarouselViewDemos.Controls"
             x:Class="CarouselViewDemos.Views.HorizontalLayoutDataTemplateSelectorPage">
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

    <CarouselView ItemsSource="{Binding Monkeys}"
                  ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

[`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)속성은 개체로 설정 됩니다 `MonkeyDataTemplateSelector` . 다음 예제에서는 클래스를 보여 줍니다 `MonkeyDataTemplateSelector` .

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

`MonkeyDataTemplateSelector`클래스는 `AmericanMonkey` `OtherMonkey` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 다른 데이터 템플릿으로 설정 된 및 속성을 정의 합니다. 이 `OnSelectTemplate` 재정의는 `AmericanMonkey` 원숭이 이름에 "아메리카"가 포함 된 경우 템플릿을 반환 합니다. 원숭이 이름에 "아메리카"가 포함 되지 않은 경우 `OnSelectTemplate` 재정의는 해당 데이터를 회색으로 표시 하는 템플릿을 반환 합니다 `OtherMonkey` .

[![IOS 및 Android에서 CarouselView 런타임 항목 템플릿 선택의 스크린샷](populate-data-images/datatemplateselector.png "CarouselView의 런타임 항목 템플릿 선택")](populate-data-images/datatemplateselector-large.png#lightbox "CarouselView의 런타임 항목 템플릿 선택")

데이터 템플릿 선택기에 대 한 자세한 내용은 [Create a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)을 참조 하세요.

> [!IMPORTANT]
> 를 사용 하는 경우 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 개체의 루트 요소를으로 설정 하지 마십시오 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `ViewCell` . 이로 인해에는 셀 개념이 없기 때문에 예외가 throw 됩니다 `CarouselView` .

## <a name="display-indicators"></a>표시 지표

의 항목 수와 현재 위치를 나타내는 표시기가 `CarouselView` 옆에 표시 될 수 있습니다 `CarouselView` . 이 `IndicatorView` 작업은 다음과 같은 컨트롤을 사용 하 여 수행할 수 있습니다.

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

이 예제에서는의 `IndicatorView` `CarouselView` 각 항목에 대 한 표시기를 사용 하 여 아래에 렌더링 됩니다 `CarouselView` . 는 `IndicatorView` 속성을 개체로 설정 하 여 데이터로 채워집니다 `CarouselView.IndicatorView` `IndicatorView` . 각 표시기는 연한 회색 원 이지만의 현재 항목을 나타내는 표시기는 `CarouselView` 진한 회색입니다.

[![IOS 및 Android에서 CarouselView 및 IndicatorView의 스크린샷](populate-data-images/indicators.png "IndicatorView 원")](populate-data-images/indicators-large.png#lightbox "IndicatorView 원")

> [!IMPORTANT]
> 속성을 설정 하면 속성이 속성에 바인딩되어 속성에 속성 `CarouselView.IndicatorView` `IndicatorView.Position` `CarouselView.Position` 바인딩이 생성 `IndicatorView.ItemsSource` `CarouselView.ItemsSource` 됩니다.

표시기에 대 한 자세한 내용은 [ Xamarin.Forms IndicatorView](~/xamarin-forms/user-interface/indicatorview.md)를 참조 하세요.

## <a name="context-menus"></a>상황에 맞는 메뉴

[`CarouselView`](xref:Xamarin.Forms.CarouselView)는 `SwipeView` 살짝 밀기 제스처를 사용 하 여 상황에 맞는 메뉴를 표시 하는를 통해 데이터 항목의 상황에 맞는 메뉴를 지원 합니다. 는 `SwipeView` 콘텐츠 항목 주위에 래핑하는 컨테이너 컨트롤이 며 해당 콘텐츠 항목에 대 한 상황에 맞는 메뉴 항목을 제공 합니다. 따라서가 `CarouselView` `SwipeView` 래핑하는 콘텐츠를 정의 하는 `SwipeView` 및 살짝 밀기 제스처로 표시 되는 상황에 맞는 메뉴 항목을 정의 하는을 만들어에 대해 상황에 맞는 메뉴를 구현할 수 있습니다. 이렇게 `SwipeView` 하려면 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 에서 데이터의 각 항목의 모양을 정의 하는에를 추가 합니다 `CarouselView` .

```xaml
<CarouselView x:Name="carouselView"
              ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                    <Frame HasShadow="True"
                           BorderColor="DarkGray"
                           CornerRadius="5"
                           Margin="20"
                           HeightRequest="300"
                           HorizontalOptions="Center"
                           VerticalOptions="CenterAndExpand">
                        <SwipeView>
                            <SwipeView.TopItems>
                                <SwipeItems>
                                    <SwipeItem Text="Favorite"
                                               IconImageSource="favorite.png"
                                               BackgroundColor="LightGreen"
                                               Command="{Binding Source={x:Reference carouselView}, Path=BindingContext.FavoriteCommand}"
                                               CommandParameter="{Binding}" />
                                </SwipeItems>
                            </SwipeView.TopItems>
                            <SwipeView.BottomItems>
                                <SwipeItems>
                                    <SwipeItem Text="Delete"
                                               IconImageSource="delete.png"
                                               BackgroundColor="LightPink"
                                               Command="{Binding Source={x:Reference carouselView}, Path=BindingContext.DeleteCommand}"
                                               CommandParameter="{Binding}" />
                                </SwipeItems>
                            </SwipeView.BottomItems>
                            <StackLayout>
                                <!-- Define item appearance -->
                            </StackLayout>
                        </SwipeView>
                    </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

carouselView.ItemTemplate = new DataTemplate(() =>
{
    StackLayout stackLayout = new StackLayout();
    Frame frame = new Frame { ... };

    SwipeView swipeView = new SwipeView();
    SwipeItem favoriteSwipeItem = new SwipeItem
    {
        Text = "Favorite",
        IconImageSource = "favorite.png",
        BackgroundColor = Color.LightGreen
    };
    favoriteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.FavoriteCommand", source: carouselView));
    favoriteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    SwipeItem deleteSwipeItem = new SwipeItem
    {
        Text = "Delete",
        IconImageSource = "delete.png",
        BackgroundColor = Color.LightPink
    };
    deleteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.DeleteCommand", source: carouselView));
    deleteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    swipeView.TopItems = new SwipeItems { favoriteSwipeItem };
    swipeView.BottomItems = new SwipeItems { deleteSwipeItem };

    StackLayout swipeViewStackLayout = new StackLayout { ... };
    swipeView.Content = swipeViewStackLayout;
    frame.Content = swipeView;
    stackLayout.Children.Add(frame);

    return stackLayout;
});
```

이 예제에서 콘텐츠는 `SwipeView` [`StackLayout`](xref:Xamarin.Forms.StackLayout) 에서로 둘러싸인 각 항목의 모양을 정의 하는입니다 [`Frame`](xref:Xamarin.Forms.Frame) [`CarouselView`](xref:Xamarin.Forms.CarouselView) . 살짝 밀기 항목은 콘텐츠에 대 한 작업을 수행 하는 데 사용 되며 `SwipeView` 컨트롤이 위쪽에서 아래쪽으로 스와이프 때 표시 됩니다.

[![IOS 및 Android에서 CarouselView 하위 상황에 맞는 메뉴 항목의 스크린샷](populate-data-images/swipeview-bottom.png "CarouselView가 bottom SwipeView 상황에 맞는 메뉴 항목")](populate-data-images/swipeview-bottom-large.png#lightbox "CarouselView가 bottom SwipeView 상황에 맞는 메뉴 항목") 
 [ ![IOS 및 Android의 CarouselView 최상위 메뉴 항목 스크린샷](populate-data-images/swipeview-top.png "Top SwipeView 상황에 맞는 메뉴 항목을 사용 하는 CarouselView")](populate-data-images/swipeview-top-large.png#lightbox "Top SwipeView 상황에 맞는 메뉴 항목을 사용 하는 CarouselView")

`SwipeView`는 개체를 추가할 방향 컬렉션에 의해 정의 되는 살짝 밀기 방향을 사용 하 여 네 가지 살짝 밀기 방향을 지원 `SwipeItems` `SwipeItems` 합니다. 기본적으로 사용자가 탭 할 때 살짝 밀기 항목이 실행 됩니다. 또한 살짝 밀기 항목이 실행 된 후에는 살짝 밀기 항목이 숨겨지고 `SwipeView` 콘텐츠가 다시 표시 됩니다. 그러나 이러한 동작은 변경할 수 있습니다.

컨트롤에 대 한 자세한 내용은 `SwipeView` [ Xamarin.Forms SwipeView](~/xamarin-forms/user-interface/swipeview.md)를 참조 하세요.

## <a name="pull-to-refresh"></a>당겨서 새로 고침

[`CarouselView`](xref:Xamarin.Forms.CarouselView)는를 통해 기능을 새로 고칠 수 있도록 지원 합니다 .이를 통해 `RefreshView` 항목을 축소 하 여 새로 고칠 수 있도록 데이터를 표시 합니다. 는 `RefreshView` 자식에서 스크롤 가능한 콘텐츠를 지 원하는 경우 해당 자식에 대 한 기능을 새로 고치는 끌어오기를 제공 하는 컨테이너 컨트롤입니다. 따라서를 `CarouselView` 의 자식으로 설정 하 여에 대 한 끌어오기를 새로 고쳐 구현 합니다 `RefreshView` .

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <CarouselView ItemsSource="{Binding Animals}">
        ...
    </CarouselView>
</RefreshView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
refreshView.Content = carouselView;
// ...
```

사용자가 새로 고침을 시작 하면 `ICommand` 속성으로 정의 된가 `Command` 실행 되어 표시 되는 항목을 새로 고쳐야 합니다. 새로 고침이 발생 하는 동안 애니메이션 처리 원으로 구성 된 새로 고침 시각화가 표시 됩니다.

[![IOS 및 Android에서 CarouselView 끌어오기-새로 고침의 스크린샷](populate-data-images/pull-to-refresh.png "CarouselView 가져오기-새로 고침")](populate-data-images/pull-to-refresh-large.png#lightbox "CarouselView 가져오기-새로 고침")

속성의 값은 `RefreshView.IsRefreshing` 의 현재 상태를 나타냅니다 `RefreshView` . 사용자가 새로 고침을 트리거하는 경우이 속성은 자동으로로 전환 됩니다 `true` . 새로 고침이 완료 되 면 속성을로 다시 설정 해야 합니다 `false` .

에 대 한 자세한 내용은 `RefreshView` [ Xamarin.Forms refreshview](~/xamarin-forms/user-interface/refreshview.md)를 참조 하세요.

## <a name="load-data-incrementally"></a>증분 방식으로 데이터 로드

[`CarouselView`](xref:Xamarin.Forms.CarouselView)사용자가 스크롤하면 증분 데이터 가상화를 지원 합니다. 이렇게 하면 사용자가 스크롤할 때 웹 서비스에서 데이터 페이지를 비동기적으로 로드 하는 등의 시나리오를 사용할 수 있습니다. 또한 더 많은 데이터를 로드 하는 지점은 사용자가 빈 공간을 보거나 스크롤에서 중지 되도록 구성할 수 있습니다.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)데이터의 증분 로드를 제어 하는 다음 속성을 정의 합니다.

- `RemainingItemsThreshold`형식의, `int` 이벤트가 발생 되는 목록에 아직 표시 되지 않는 항목의 임계값입니다 `RemainingItemsThresholdReached` .
- `RemainingItemsThresholdReachedCommand`에 `ICommand` 도달할 때 실행 되는 형식의입니다 `RemainingItemsThreshold` .
- `object` 형식의 `RemainingItemsThresholdReachedCommandParameter` - `RemainingItemsThresholdReachedCommand`에 전달되는 매개 변수입니다.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)또한 `RemainingItemsThresholdReached` `CarouselView` `RemainingItemsThreshold` 항목이 표시 되지 않을 정도로가 충분히 스크롤 될 때 발생 하는 이벤트를 정의 합니다. 이 이벤트를 처리 하 여 더 많은 항목을 로드할 수 있습니다. 또한 `RemainingItemsThresholdReached` 이벤트가 발생 하면 `RemainingItemsThresholdReachedCommand` 이 실행 되어 viewmodel에서 증분 데이터 로드가 발생 하도록 할 수 있습니다.

속성의 기본값은 `RemainingItemsThreshold` -1 이며이는 `RemainingItemsThresholdReached` 이벤트가 발생 하지 않음을 나타냅니다. 속성 값이 0 이면의 `RemainingItemsThresholdReached` 마지막 항목이 표시 될 때 이벤트가 발생 합니다 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) . 0 보다 큰 값의 경우에 `RemainingItemsThresholdReached` `ItemsSource` 아직 스크롤되지 않는 항목 수가에 포함 되어 있으면 이벤트가 발생 합니다.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)속성의 `RemainingItemsThreshold` 값이 항상-1 보다 크거나 같도록 속성의 유효성을 검사 합니다.

다음 XAML 예제에서는 데이터를 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 증분 로드 하는을 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Animals}"
              RemainingItemsThreshold="2"
              RemainingItemsThresholdReached="OnCarouselViewRemainingItemsThresholdReached"
              RemainingItemsThresholdReachedCommand="{Binding LoadMoreDataCommand}">
    ...
</CarouselView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView
{
    RemainingItemsThreshold = 2
};
carouselView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

이 코드 예제에서 이벤트는 `RemainingItemsThresholdReached` 두 개의 항목이 아직 스크롤되지 않고 이벤트 처리기를 실행 하는 경우에 발생 합니다 `OnCollectionViewRemainingItemsThresholdReached` .

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> 를 `RemainingItemsThresholdReachedCommand` viewmodel의 구현에 바인딩하여 데이터를 증분 로드할 수도 있습니다 `ICommand` .

## <a name="related-links"></a>관련 링크

- [CarouselView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Xamarin.FormsIndicatorView](~/xamarin-forms/user-interface/indicatorview.md)
- [Xamarin.FormsRefreshView](~/xamarin-forms/user-interface/refreshview.md)
- [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [DataTemplateSelector 만들기 Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
