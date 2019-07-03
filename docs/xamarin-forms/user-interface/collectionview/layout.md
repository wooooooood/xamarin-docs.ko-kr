---
title: Xamarin.Forms CollectionView 레이아웃
description: 기본적으로 CollectionView 세로 목록에 해당 항목을 표시 됩니다. 그러나 가로 및 세로 목록 및 표를 지정할 수 있습니다.
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2019
ms.openlocfilehash: 786cea04718022847bba2ecffed8f377dd49bd8b
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67512823"
---
# <a name="xamarinforms-collectionview-layout"></a>Xamarin.Forms CollectionView 레이아웃

![](~/media/shared/preview.png "이 API는 현재 시험판임")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 레이아웃을 제어 하는 다음 속성을 정의 합니다.

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)를 형식 [ `IItemsLayout` ](xref:Xamarin.Forms.IItemsLayout), 사용할 레이아웃을 지정 합니다.
- [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy)를 형식 [ `ItemSizingStrategy` ](xref:Xamarin.Forms.ItemSizingStrategy), 사용할 항목 측정값 전략을 지정 합니다.

이러한 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 개체 속성을 데이터 바인딩의 대상 수 있음을 의미 합니다.

기본적으로 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 세로 목록에 항목을 표시 합니다. 그러나 다음 레이아웃의 사용할 수 있습니다.

- 세로 목록-새 항목 추가 됨에 따라 세로로 늘어남에 따라 하는 단일 열 목록입니다.
- 가로 목록-새 항목 추가 됨에 따라 가로로 증가 하는 단일 행 목록입니다.
- 세로 격자 눈금-새 항목 추가 됨에 따라 세로로 늘어남에 따라 하는 여러 열 표입니다.
- 가로 격자 눈금-새 항목 추가 됨에 따라 가로로 증가 하는 다중 행 표입니다.

이러한 레이아웃을 설정 하 여 지정할 수는 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) 속성을 클래스에서 파생 되는 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsLayout) 클래스입니다. 이 클래스에 다음 속성을 정의합니다.

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)형식의 [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation)는 방향을 지정 합니다 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 항목 추가 됨에 따라 확장 합니다.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)를 형식 [ `SnapPointsAlignment` ](xref:Xamarin.Forms.SnapPointsAlignment), 끌기 지점 항목을 사용 하 여 정렬 되는 방법을 지정 합니다.
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)를 형식 [ `SnapPointsType` ](xref:Xamarin.Forms.SnapPointsType)를 스크롤할 때 맞춤 지점이의 동작을 지정 합니다.

이러한 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 개체 속성을 데이터 바인딩의 대상 수 있음을 의미 합니다. 끌기 지점에 대 한 자세한 내용은 참조 하세요. [맞춤 지점을](scrolling.md#snap-points) 에 [Xamarin.Forms CollectionView 스크롤](scrolling.md) 가이드입니다.

합니다 [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation) 열거형은 다음 멤버를 정의 합니다.

- `Vertical` 나타내는 합니다 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 항목 추가 됨에 따라 세로로 확장 됩니다.
- `Horizontal` 나타내는 합니다 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 항목 추가 됨에 따라 가로로 확장 됩니다.

[ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout) 클래스에서 상속 합니다 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsLayout) 클래스를 정의 `ItemSpacing` 형식의 속성 `double`, 각 항목 둘레 빈 공간을 나타내는입니다. 이 속성의 기본값은 0 및 0 보다 크거나 같은 경우에 해당 값은 항상 이어야 합니다. 합니다 `ListItemsLayout` 클래스에는 또한 정적 정의 `Vertical` 및 `Horizontal` 멤버입니다. 이러한 멤버는 각각 세로 또는 가로 목록를 만드는 데 사용할 수 있습니다. 또는 `ListItemsLayout` 개체를 만들 수를 지정 하는 [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation) 인수로 열거형 멤버입니다.

합니다 [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) 클래스에서 상속 합니다 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsLayout) 클래스 및 다음 속성을 정의 합니다.

- `VerticalItemSpacing`형식의 `double`, 각 항목 둘레 세로 빈 공간을 나타내는입니다. 이 속성의 기본값은 0 및 0 보다 크거나 같은 경우에 해당 값은 항상 이어야 합니다.
- `HorizontalItemSpacing`형식의 `double`, 각 항목 둘레 가로 빈 공간을 나타내는입니다. 이 속성의 기본값은 0 및 0 보다 크거나 같은 경우에 해당 값은 항상 이어야 합니다.
- `Span`형식의 `int`, 열 또는 모눈에 표시할 행의 수를 나타내는입니다. 이 속성의 기본값은 1이 하 고 1 보다 크거나 같은 경우에 해당 값은 항상 이어야 합니다.

이러한 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 개체 속성을 데이터 바인딩의 대상 수 있음을 의미 합니다.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) 기본 레이아웃 엔진을 레이아웃 하는 데 사용 합니다.

## <a name="vertical-list"></a>세로 목록

기본적으로 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 세로 목록 레이아웃에서 해당 항목을 표시 합니다. 따라서 필요한 경우가 아니라면 설정 하는 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) 이 레이아웃을 사용 하는 속성:

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

완성도 위해을 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 설정 하 여 세로 목록에 항목을 표시 설정할 수 있는 해당 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) 속성을 `VerticalList`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="VerticalList">
    ...
</CollectionView>
```

또는 이렇게 설정 하 여는 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) 개체의 속성을 [ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout) 클래스를 지정 하는 `Vertical` [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation) 인수로 열거형 멤버:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout>
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.Vertical
};
```

이 인해 새 항목 추가 됨에 따라 세로로 늘어남에 따라 하는 단일 열 목록에서:

[![IOS 및 Android에서 CollectionView 세로 목록 레이아웃의 스크린 샷](layout-images/vertical-list.png "CollectionView 세로 목록 레이아웃")](layout-images/vertical-list-large.png#lightbox "CollectionView 세로 목록 레이아웃")

## <a name="horizontal-list"></a>가로 목록

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 설정 하 여 가로 목록에서 해당 항목을 표시할 수 해당 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) 속성을 `HorizontalList`:

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

또는 이렇게 설정 하 여 합니다 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) 속성을을 [ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout) 개체를 지정 하는 `Horizontal` [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation) 인수로 서 열거형 멤버:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout>
            <x:Arguments>
                <ItemsLayoutOrientation>Horizontal</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.Horizontal
};
```

이 인해 새 항목 추가 됨에 따라 가로로 확장 되는 단일 행 목록:

[![IOS 및 Android에서 CollectionView 가로 목록 레이아웃의 스크린 샷](layout-images/horizontal-list.png "CollectionView 가로 목록 레이아웃")](layout-images/horizontal-list-large.png#lightbox "CollectionView 가로 목록 레이아웃")

## <a name="vertical-grid"></a>세로 격자 눈금

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 설정 하 여을 세로 격자 눈금에서 해당 항목을 표시할 수 있습니다 해당 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) 속성을 [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) 개체 [ `Orientation` ](xref:Xamarin.Forms.ItemsLayout.Orientation) 속성`Vertical`:

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

기본적으로 세로 [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) 단일 열에서 항목을 표시 합니다. 그러나 설정 하는이 예제는 `GridItemsLayout.Span` 속성을 2로 합니다. 이 인해 새 항목 추가 됨에 따라 세로로 확장 되는 2 열 그리드에:

[![IOS 및 Android에서 CollectionView 세로 격자 레이아웃의 스크린 샷](layout-images/vertical-grid.png "CollectionView 세로 격자 레이아웃")](layout-images/vertical-grid-large.png#lightbox "CollectionView 세로 모눈 레이아웃")

## <a name="horizontal-grid"></a>가로 눈금

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 설정 하 여 가로 격자 눈금에 항목을 표시할 수 있습니다 해당 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) 속성을 [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) 개체[ `Orientation` ](xref:Xamarin.Forms.ItemsLayout.Orientation) 속성 `Horizontal`:

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

기본적으로 가로 [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) 단일 행으로 항목을 표시 합니다. 그러나 설정 하는이 예제는 `GridItemsLayout.Span` 4 속성입니다. 이 인해 증가 함에 따라 가로로 새 항목이 추가 되는 행 4 표:

[![IOS 및 Android에서 CollectionView 가로 격자 레이아웃의 스크린 샷](layout-images/horizontal-grid.png "CollectionView 가로 격자 레이아웃")](layout-images/horizontal-grid-large.png#lightbox "CollectionView 가로 격자 레이아웃")

## <a name="item-spacing"></a>항목 간격

기본적으로 각 항목에 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 묶어 빈 공간에 없습니다. 사용 하는 항목 레이아웃의 속성을 설정 하 여이 동작을 변경할 수 있습니다는 `CollectionView`합니다.

경우는 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 설정 해당 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) 속성을을 [ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout) 개체를 `ListItemsLayout.ItemSpacing` 속성 설정할 수 있습니다는 `double` 각 항목 둘레 빈 공간을 나타내는 값:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout ItemSpacing="20">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> `ListItemsLayout.ItemSpacing` 속성이 속성의 값은 0 보다 크거나 같은 경우 항상 보장 하는 유효성 검사 콜백 집합입니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

이 코드는 각 항목 둘레 20의 간격이 있는 세로 단일 열 목록에 발생 합니다.

[![IOS 및 Android에서 항목 간격을 사용 하 여 CollectionView 스크린샷](layout-images/vertical-list-spacing.png "CollectionView 항목 간격")](layout-images/vertical-list-spacing-large.png#lightbox "CollectionView 항목 간격")

경우는 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 설정 해당 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout) 속성을을 [ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) 개체를 `GridItemsLayout.VerticalItemSpacing` 및 `GridItemsLayout.HorizontalItemSpacing` 속성 수 로 `double` 값 각 항목 둘레 가로 및 세로로 빈 공간을 나타냅니다.

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
> 합니다 `GridItemsLayout.VerticalItemSpacing` 및 `GridItemsLayout.HorizontalItemSpacing` 속성에는 유효성 검사 콜백은 속성의 값은 0 보다 크거나 같은 경우 항상 확인 하는 설정입니다.

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

이 코드는 세로 2 열 그리드에 있는 각 항목 둘레 20 세로 간격 및 각 항목 둘레 30 가로 간격에서 발생 합니다.

[![IOS 및 Android에서 항목 간격을 사용 하 여 CollectionView 스크린샷](layout-images/vertical-grid-spacing.png "CollectionView 항목 간격")](layout-images/vertical-grid-spacing-large.png#lightbox "CollectionView 항목 간격")

## <a name="item-sizing"></a>항목 크기 조정

기본적으로 각 항목에 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 는 개별적으로 측정 및 크기에 UI 요소를 제공 합니다 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 고정된 크기를 지정 하지. 변경 될 수 있는이 동작을 지정 합니다 [ `CollectionView.ItemSizingStrategy` ](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy) 속성 값입니다. 이 속성 값 중 하나로 설정할 수 있습니다 합니다 [ `ItemSizingStrategy` ](xref:Xamarin.Forms.ItemSizingStrategy) 열거형 멤버:

- `MeasureAllItems` – 각 항목은 개별적으로 측정 됩니다. 기본값입니다.
- `MeasureFirstItem` – 첫 번째 항목으로 같은 크기로 지정 되는 모든 후속 항목을 사용 하 여 첫 번째 항목만 측정 됩니다.

> [!IMPORTANT]
> `MeasureFirstItem` 여기서 항목 크기에서 모든 항목 동일 하 게 되는 상황에서 사용할 때 성능이 향상된 하면 전략의 크기를 조정 합니다.

다음 코드 예제에서는 설정 된 [ `ItemSizingStrategy` ](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy) 속성:

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

항목을 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 변경 하 여 런타임에 동적으로 조정할 수 있는 레이아웃 관련 속성 내에서 요소를 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)합니다. 예를 들어, 다음 코드 예제에서는 변경 된 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) 및 [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) 의 속성을 [ `Image` ](xref:Xamarin.Forms.Image) 개체:

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(60) ? 100 : 60;
}
```

합니다 `OnImageTapped` 이벤트 처리기에 대 한 응답으로 실행 되는 [ `Image` ](xref:Xamarin.Forms.Image) 탭 중인 개체를 보다 쉽게 볼 수 있도록 이미지의 크기를 변경:

[![IOS 및 Android에서의 동적 항목 크기를 사용 하 여 CollectionView 스크린샷](layout-images/runtime-resizing.png "CollectionView 동적 항목 크기 조정")](layout-images/runtime-resizing-large.png#lightbox "CollectionView 동적 항목 크기 조정")

## <a name="right-to-left-layout"></a>오른쪽에서 왼쪽 레이아웃

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 레이아웃을 설정 하 여 오른쪽에서 왼쪽 흐름 방향으로 내용을 수 해당 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성을 [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft)합니다. 그러나는 `FlowDirection` 속성 이상적으로로 설정 해야 그러면 페이지 또는 루트 레이아웃 내에서 모든 요소는 페이지 또는 루트 레이아웃에서 흐름 방향을에 응답 합니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
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

기본값 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 된 부모 요소에 대 한 [ `MatchParent` ](xref:Xamarin.Forms.FlowDirection.MatchParent)합니다. 따라서 합니다 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 상속를 `FlowDirection` 속성 값을는 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)는 상속를 `FlowDirection` 속성 값을는 [ `ContentPage`](xref:Xamarin.Forms.ContentPage). 이 인해 다음 스크린샷에 표시 된 오른쪽에서 왼쪽 레이아웃:

[![IOS 및 Android에서 CollectionView 세로 목록 오른쪽에서 왼쪽 레이아웃을 스크린샷](layout-images/vertical-list-rtl.png "CollectionView 세로 목록 오른쪽에서 왼쪽 레이아웃")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView 오른쪽에서 왼쪽 세로 레이아웃 목록")

흐름 방향에 대 한 자세한 내용은 참조 하세요. [오른쪽에서 왼쪽 지역화](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)합니다.

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
- [오른쪽에서 왼쪽 지역화](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin.Forms CollectionView 스크롤](scrolling.md)
