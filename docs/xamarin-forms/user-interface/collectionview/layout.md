---
제목: " Xamarin.Forms CollectionView Layout" description: "기본적으로 CollectionView는 해당 항목을 세로 목록에 표시 합니다. 그러나 세로 및 가로 목록과 그리드를 지정할 수 있습니다. "
assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9: xamarin-forms author: davidbritch: dabritch:: 08/22/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-collectionview-layout"></a>Xamarin.FormsCollectionView 레이아웃

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)는 레이아웃을 제어 하는 다음 속성을 정의 합니다.

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout), 형식의, [`IItemsLayout`](xref:Xamarin.Forms.IItemsLayout) 사용할 레이아웃을 지정 합니다.
- [`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy), 형식의, [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy) 사용할 항목 측정 전략을 지정 합니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

기본적으로는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 해당 항목을 세로 목록에 표시 합니다. 그러나 다음 레이아웃 중 하나를 사용할 수 있습니다.

- 세로 목록 – 새 항목이 추가 될 때 세로로 확장 되는 단일 열 목록입니다.
- 가로 목록 – 새 항목이 추가 될 때 가로로 증가 하는 단일 행 목록입니다.
- 세로 그리드 – 새 항목이 추가 될 때 세로로 증가 하는 여러 열 표입니다.
- 가로 그리드 – 새 항목이 추가 될 때 가로로 증가 하는 다중 행 표입니다.

이러한 레이아웃은 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) 클래스에서 파생 되는 클래스에 속성을 설정 하 여 지정할 수 있습니다 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) . 이 클래스는 다음 속성을 정의 합니다.

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)형식의는 [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) [`CollectionView`](xref:Xamarin.Forms.CollectionView) 항목이 추가 될 때가 확장 되는 방향을 지정 합니다.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)형식의는 [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) 맞춤 지점이 항목에 정렬 되는 방법을 지정 합니다.
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)형식의는 [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) 스크롤할 때 중심점의 동작을 지정 합니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다. 맞추기 요소에 대 한 자세한 내용은 [ Xamarin.Forms CollectionView 스크롤](scrolling.md) 가이드의 [snap points](scrolling.md#snap-points) 을 참조 하세요.

[`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)열거형은 다음 멤버를 정의 합니다.

- `Vertical`[`CollectionView`](xref:Xamarin.Forms.CollectionView)항목이 추가 될 때이 세로로 확장 됨을 나타냅니다.
- `Horizontal`[`CollectionView`](xref:Xamarin.Forms.CollectionView)항목이 추가 될 때가 가로로 확장 됨을 나타냅니다.

`LinearItemsLayout`클래스는 클래스에서 상속 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) 되며 `ItemSpacing` `double` 각 항목 주위의 빈 공간을 나타내는 형식의 속성을 정의 합니다. 이 속성의 기본값은 0이 고 해당 값은 항상 0 보다 크거나 같아야 합니다. `LinearItemsLayout`또한 클래스는 정적 `Vertical` 및 멤버를 정의 `Horizontal` 합니다. 이러한 멤버를 사용 하 여 각각 세로 또는 가로 목록을 만들 수 있습니다. 또는 `LinearItemsLayout` 열거형 멤버를 인수로 지정 하 여 개체를 만들 수 있습니다 [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) .

클래스는 [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) 클래스에서 상속 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) 되며 다음 속성을 정의 합니다.

- `VerticalItemSpacing``double`각 항목 주위의 세로 빈 공간을 나타내는 형식의입니다. 이 속성의 기본값은 0이 고 해당 값은 항상 0 보다 크거나 같아야 합니다.
- `HorizontalItemSpacing``double`각 항목 주위에 있는 가로 빈 공간을 나타내는 형식의입니다. 이 속성의 기본값은 0이 고 해당 값은 항상 0 보다 크거나 같아야 합니다.
- `Span``int`표에 표시 되는 열 또는 행 수를 나타내는 형식의입니다. 이 속성의 기본값은 1이 고 해당 값은 항상 1 보다 크거나 같아야 합니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)에서는 네이티브 레이아웃 엔진을 사용 하 여 레이아웃을 수행 합니다.

## <a name="vertical-list"></a>세로 목록

기본적으로는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 세로 목록 레이아웃으로 해당 항목을 표시 합니다. 따라서 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) 이 레이아웃을 사용 하도록 속성을 설정할 필요는 없습니다.

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
</CollectionView>
```

그러나 완전성을 위해의 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 속성을로 설정 하 여 해당 항목을 세로 목록에 표시 하도록를 설정할 수 있습니다 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) `VerticalList` .

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="VerticalList">
    ...
</CollectionView>
```

또는 속성을 개체로 설정 하 여 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) `LinearItemsLayout` `Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) 열거형 멤버를 `Orientation` 속성 값으로 지정 하 여이 작업을 수행할 수도 있습니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = LinearItemsLayout.Vertical
};
```

그러면 새 항목이 추가 될 때 세로로 확장 되는 단일 열 목록이 생성 됩니다.

[![IOS 및 Android에서 CollectionView 세로 목록 레이아웃의 스크린샷](layout-images/vertical-list.png "CollectionView 세로 목록 레이아웃")](layout-images/vertical-list-large.png#lightbox "CollectionView 세로 목록 레이아웃")

## <a name="horizontal-list"></a>가로 목록

[`CollectionView`](xref:Xamarin.Forms.CollectionView)에서는 속성을로 설정 하 여 해당 항목을 가로 목록에 표시할 수 있습니다 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) `HorizontalList` .

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="HorizontalList">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

또는 속성을 개체로 설정 하 여 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) `LinearItemsLayout` `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) 열거형 멤버를 `Orientation` 속성 값으로 지정 하 여이 레이아웃을 수행할 수도 있습니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = LinearItemsLayout.Horizontal
};
```

그러면 새 항목이 추가 될 때 가로 크기가 증가 하는 단일 행 목록이 생성 됩니다.

[![IOS 및 Android에서 CollectionView 가로 목록 레이아웃의 스크린샷](layout-images/horizontal-list.png "CollectionView 가로 목록 레이아웃")](layout-images/horizontal-list-large.png#lightbox "CollectionView 가로 목록 레이아웃")

## <a name="vertical-grid"></a>세로 눈금

[`CollectionView`](xref:Xamarin.Forms.CollectionView)는 속성이 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) 로 설정 된 개체에 속성을 설정 하 여 해당 항목을 세로 모눈에 표시할 수 있습니다 [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) `Vertical` .

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="80" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

기본적으로 세로은 [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) 단일 열에 항목을 표시 합니다. 그러나이 예제에서는 속성을 `GridItemsLayout.Span` 2로 설정 합니다. 그러면 새 항목이 추가 될 때 세로로 증가 하는 2 열 눈금이 생성 됩니다.

[![IOS 및 Android에서 CollectionView 세로 격자 레이아웃의 스크린샷](layout-images/vertical-grid.png "세로 모눈 레이아웃 CollectionView")](layout-images/vertical-grid-large.png#lightbox "세로 모눈 레이아웃 CollectionView")

## <a name="horizontal-grid"></a>가로 눈금

[`CollectionView`](xref:Xamarin.Forms.CollectionView)는 속성이 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) 로 설정 된 개체로 속성을 설정 하 여 가로 모눈에 항목을 표시할 수 있습니다 [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) `Horizontal` .

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Horizontal"
                        Span="4" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

기본적으로 가로는 [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) 단일 행에 항목을 표시 합니다. 그러나이 예제에서는 속성을 `GridItemsLayout.Span` 4로 설정 합니다. 그러면 새 항목이 추가 될 때 가로 크기가 증가 하는 4 행 표가 생성 됩니다.

[![IOS 및 Android에서 CollectionView 가로 그리드 레이아웃의 스크린샷](layout-images/horizontal-grid.png "CollectionView 가로 그리드 레이아웃")](layout-images/horizontal-grid-large.png#lightbox "CollectionView 가로 그리드 레이아웃")

## <a name="headers-and-footers"></a>머리글 및 바닥글

[`CollectionView`](xref:Xamarin.Forms.CollectionView)는 목록에 있는 항목으로 스크롤 하는 머리글 및 바닥글을 표시할 수 있습니다. 머리글과 바닥글은 문자열, 뷰 또는 개체 일 수 있습니다 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

[`CollectionView`](xref:Xamarin.Forms.CollectionView)머리글 및 바닥글을 지정 하기 위한 다음 속성을 정의 합니다.

- `Header`형식의는 `object` 목록 시작 부분에 표시 되는 문자열, 바인딩 또는 뷰를 지정 합니다.
- `HeaderTemplate`형식의 형식은를 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 지정 `DataTemplate` 하는 데 사용할를 지정 합니다 `Header` .
- `Footer`형식의는 `object` 목록 끝에 표시 되는 문자열, 바인딩 또는 뷰를 지정 합니다.
- `FooterTemplate`형식의 형식은를 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 지정 `DataTemplate` 하는 데 사용할를 지정 합니다 `Footer` .

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

왼쪽에서 오른쪽으로 수평으로 확장 되는 레이아웃에 머리글을 추가 하면 머리글은 목록 왼쪽에 표시 됩니다. 마찬가지로 왼쪽에서 오른쪽으로 수평으로 증가 하는 레이아웃에 바닥글을 추가 하면 목록 오른쪽에 바닥글이 표시 됩니다.

### <a name="display-strings-in-the-header-and-footer"></a>머리글 및 바닥글에 문자열 표시

`Header`및 `Footer` 속성은 `string` 다음 예제와 같이 값으로 설정할 수 있습니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                Header="Monkeys"
                Footer="2019">
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    Header = "Monkeys",
    Footer = "2019"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

이 코드는 iOS 스크린샷에 표시 된 헤더와 Android 스크린샷에 표시 된 바닥글을 포함 하는 다음과 같은 스크린샷을 생성 합니다.

[![IOS 및 Android에서 CollectionView 문자열 헤더 및 바닥글의 스크린샷](layout-images/header-footer-string.png "CollectionView string 헤더 및 꼬리말")](layout-images/header-footer-string-large.png#lightbox "CollectionView string 헤더 및 꼬리말")

### <a name="display-views-in-the-header-and-footer"></a>머리글 및 바닥글에 보기 표시

`Header`및 `Footer` 속성은 각각 뷰로 설정할 수 있습니다. 단일 보기 이거나 여러 자식 뷰를 포함 하는 뷰입니다. 다음 예제에서는 개체를 `Header` `Footer` [`StackLayout`](xref:Xamarin.Forms.StackLayout) 포함 하는 개체로 설정 된 및 속성을 보여 줍니다 [`Label`](xref:Xamarin.Forms.Label) .

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.Header>
        <StackLayout BackgroundColor="LightGray">
            <Label Margin="10,0,0,0"
                   Text="Monkeys"
                   FontSize="Small"
                   FontAttributes="Bold" />
        </StackLayout>
    </CollectionView.Header>
    <CollectionView.Footer>
        <StackLayout BackgroundColor="LightGray">
            <Label Margin="10,0,0,0"
                   Text="Friends of Xamarin Monkey"
                   FontSize="Small"
                   FontAttributes="Bold" />
        </StackLayout>
    </CollectionView.Footer>
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    Header = new StackLayout
    {
        Children =
        {
            new Label { Text = "Monkeys", ... }
        }
    },
    Footer = new StackLayout
    {
        Children =
        {
            new Label { Text = "Friends of Xamarin Monkey", ... }
        }
    }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

이 코드는 iOS 스크린샷에 표시 된 헤더와 Android 스크린샷에 표시 된 바닥글을 포함 하는 다음과 같은 스크린샷을 생성 합니다.

[![IOS 및 Android에서 보기를 사용 하 여 CollectionView 머리글 및 바닥글의 스크린샷](layout-images/header-footer-view.png "CollectionView 뷰 머리글 및 바닥글")](layout-images/header-footer-view-large.png#lightbox "CollectionView 뷰 머리글 및 바닥글")

### <a name="display-a-templated-header-and-footer"></a>템플릿 기반 머리글 및 바닥글 표시

`HeaderTemplate`및 `FooterTemplate` 속성은 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 머리글 및 바닥글의 서식을 지정 하는 데 사용 되는 개체로 설정할 수 있습니다. 이 시나리오에서 `Header` 및 속성은 `Footer` 다음 예제와 같이 적용 될 템플릿이 현재 소스에 바인딩되어야 합니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                Header="{Binding .}"
                Footer="{Binding .}">
    <CollectionView.HeaderTemplate>
        <DataTemplate>
            <StackLayout BackgroundColor="LightGray">
                <Label Margin="10,0,0,0"
                       Text="Monkeys"
                       FontSize="Small"
                       FontAttributes="Bold" />
            </StackLayout>
        </DataTemplate>
    </CollectionView.HeaderTemplate>
    <CollectionView.FooterTemplate>
        <DataTemplate>
            <StackLayout BackgroundColor="LightGray">
                <Label Margin="10,0,0,0"
                       Text="Friends of Xamarin Monkey"
                       FontSize="Small"
                       FontAttributes="Bold" />
            </StackLayout>
        </DataTemplate>
    </CollectionView.FooterTemplate>
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    HeaderTemplate = new DataTemplate(() =>
    {
        return new StackLayout { };
    }),
    FooterTemplate = new DataTemplate(() =>
    {
        return new StackLayout { };
    })
};
collectionView.SetBinding(ItemsView.HeaderProperty, ".");
collectionView.SetBinding(ItemsView.FooterProperty, ".");
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

이 코드는 iOS 스크린샷에 표시 된 헤더와 Android 스크린샷에 표시 된 바닥글을 포함 하는 다음과 같은 스크린샷을 생성 합니다.

[![IOS 및 Android에서 템플릿을 사용 하 여 CollectionView 헤더 및 바닥글의 스크린샷](layout-images/header-footer-template.png "CollectionView 템플릿 머리글 및 바닥글")](layout-images/header-footer-template-large.png#lightbox "CollectionView 템플릿 머리글 및 바닥글")

## <a name="item-spacing"></a>항목 간격

기본적으로의 각 항목 사이에는 공백이 없습니다 [`CollectionView`](xref:Xamarin.Forms.CollectionView) . 에서 사용 하는 항목 레이아웃에 대 한 속성을 설정 하 여이 동작을 변경할 수 있습니다 `CollectionView` .

에서 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 해당 속성을 개체로 설정 하는 경우 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) 속성을 `LinearItemsLayout` `LinearItemsLayout.ItemSpacing` `double` 항목 사이의 간격을 나타내는 값으로 설정할 수 있습니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           ItemSpacing="20" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> 속성에는 `LinearItemsLayout.ItemSpacing` 속성 값이 항상 0 보다 크거나 같도록 확인 하는 유효성 검사 콜백 집합이 있습니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

이 코드는 세로 단일 열 목록을 생성 하며, 항목 사이에는 20 개의 간격이 있습니다.

[![IOS 및 Android에서 항목 간격이 있는 CollectionView의 스크린샷](layout-images/vertical-list-spacing.png "CollectionView 항목 간격")](layout-images/vertical-list-spacing-large.png#lightbox "CollectionView 항목 간격")

에서 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 해당 속성을 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) 개체로 설정 하면 [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) `GridItemsLayout.VerticalItemSpacing` 및 `GridItemsLayout.HorizontalItemSpacing` 속성을 `double` 항목 사이에서 가로 및 세로 방향으로 빈 공간을 나타내는 값으로 설정할 수 있습니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2"
                        VerticalItemSpacing="20"
                        HorizontalItemSpacing="30" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> `GridItemsLayout.VerticalItemSpacing`및 속성에는 `GridItemsLayout.HorizontalItemSpacing` 속성 값이 항상 0 보다 크거나 같은지 확인 하는 유효성 검사 콜백이 설정 되어 있습니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
    {
        VerticalItemSpacing = 20,
        HorizontalItemSpacing = 30
    }
};
```

이 코드는 세로 2 열 그리드로, 항목 사이에 20 이라는 세로 간격이 있고 항목 사이에 30 이라는 가로 간격이 있습니다.

[![Android에서 항목 간격이 있는 CollectionView의 스크린샷](layout-images/vertical-grid-spacing.png "CollectionView 항목 간격")](layout-images/vertical-grid-spacing-large.png#lightbox "CollectionView 항목 간격")

## <a name="item-sizing"></a>항목 크기 조정

의 UI 요소가 고정 크기를 지정 하지 않는 경우 기본적으로의 각 항목 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 은 개별적으로 측정 되 고 크기가 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 지정 됩니다. 변경할 수 있는이 동작은 속성 값으로 지정 됩니다 [`CollectionView.ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy) . 이 속성 값은 열거형 멤버 중 하나로 설정할 수 있습니다 [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy) .

- `MeasureAllItems`– 각 항목은 개별적으로 측정 됩니다. 이것은 기본값입니다.
- `MeasureFirstItem`– 첫 번째 항목만 측정 되며 이후의 모든 항목은 첫 번째 항목의 크기와 동일 하 게 지정 됩니다.

> [!IMPORTANT]
> `MeasureFirstItem`크기 조정 전략을 사용 하면 항목 크기를 모든 항목에서 균일 하 게 사용할 수 있는 경우 성능이 향상 됩니다.

다음 코드 예제에서는 속성을 설정 하는 방법을 보여 줍니다 [`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy) .

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

## <a name="dynamic-resizing-of-items"></a>항목의 동적 크기 조정

[`CollectionView`](xref:Xamarin.Forms.CollectionView)내에서 요소의 레이아웃 관련 속성을 변경 하 여의 항목을 런타임에 동적으로 조정할 수 있습니다 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . 예를 들어 다음 코드 예제에서는 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 개체의 및 속성을 변경 합니다 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) [`Image`](xref:Xamarin.Forms.Image) .

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(60) ? 100 : 60;
}
```

`OnImageTapped`이벤트 처리기는 탭 하는 개체에 대 한 응답으로 실행 되며 [`Image`](xref:Xamarin.Forms.Image) 더 쉽게 볼 수 있도록 이미지의 크기를 변경 합니다.

[![IOS 및 Android에서 동적 항목 크기 조정을 사용 하는 CollectionView의 스크린샷](layout-images/runtime-resizing.png "CollectionView 동적 항목 크기 조정")](layout-images/runtime-resizing-large.png#lightbox "CollectionView 동적 항목 크기 조정")

## <a name="right-to-left-layout"></a>오른쪽에서 왼쪽 레이아웃

[`CollectionView`](xref:Xamarin.Forms.CollectionView)속성을로 설정 하 여 오른쪽에서 왼쪽 흐름 방향으로 해당 콘텐츠를 레이아웃 할 수 있습니다 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft) . 그러나 페이지 `FlowDirection` 또는 루트 레이아웃에 속성을 설정 하는 것이 가장 좋습니다. 이렇게 하면 페이지 또는 루트 레이아웃 내의 모든 요소가 흐름 방향에 응답 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.VerticalListFlowDirectionPage"
             Title="Vertical list (RTL FlowDirection)"
             FlowDirection="RightToLeft">
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}">
            ...
        </CollectionView>
    </StackLayout>
</ContentPage>
```

[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)부모가 있는 요소의 기본값은 [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent) 입니다. 따라서는에서 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 속성 값을 상속 `FlowDirection` 하며,이 값은에서 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 속성 값을 상속 합니다 `FlowDirection` [`ContentPage`](xref:Xamarin.Forms.ContentPage) . 이로 인해 다음 스크린샷에 오른쪽에서 왼쪽 레이아웃이 표시 됩니다.

[![IOS 및 Android에 대 한 CollectionView 오른쪽에서 왼쪽 세로 목록 레이아웃의 스크린샷](layout-images/vertical-list-rtl.png "CollectionView 오른쪽에서 왼쪽 세로 목록 레이아웃")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView 오른쪽에서 왼쪽 세로 목록 레이아웃")

흐름 방향에 대 한 자세한 내용은 [오른쪽에서 왼쪽 지역화](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [오른쪽에서 왼쪽으로 쓰는 언어 지역화](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin.FormsCollectionView 스크롤](scrolling.md)
