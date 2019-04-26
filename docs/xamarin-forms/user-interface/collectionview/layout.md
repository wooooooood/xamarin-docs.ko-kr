---
title: Xamarin.Forms CollectionView 레이아웃 지정
description: 기본적으로 CollectionView 세로 목록에 해당 항목을 표시 됩니다. 그러나 가로 및 세로 목록 및 표를 지정할 수 있습니다.
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
ms.openlocfilehash: 8ed365ed41ac31c66d41f1a32a7a16929cdc6770
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61367692"
---
# <a name="specify-xamarinforms-collectionview-layout"></a>Xamarin.Forms CollectionView 레이아웃 지정

![미리 보기](~/media/shared/preview.png)

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView` 는 현재 미리 보기, 및 일부 계획된 기능 부족 합니다. 또한 API 구현을 완료 되 면 변경할 수 있습니다.

`CollectionView` 레이아웃을 제어 하는 다음 속성을 정의 합니다.

- `ItemsLayout`형식의 `IItemsLayout`, 사용할 레이아웃을 지정 합니다.
- `ItemSizingStrategy`형식의 `ItemSizingStrategy`, 사용할 항목 측정값 전략을 지정 합니다.

이러한 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 개체 속성을 데이터 바인딩의 대상 수 있음을 의미 합니다.

기본적으로 `CollectionView` 세로 목록에 항목을 표시 합니다. 그러나 다음 레이아웃의 사용할 수 있습니다.

- 세로 목록-새 항목 추가 됨에 따라 세로로 늘어남에 따라 하는 단일 열 목록입니다.
- 가로 목록-새 항목 추가 됨에 따라 가로로 증가 하는 단일 행 목록입니다.
- 세로 격자 눈금-새 항목 추가 됨에 따라 세로로 늘어남에 따라 하는 여러 열 표입니다.
- 가로 격자 눈금-새 항목 추가 됨에 따라 가로로 증가 하는 다중 행 표입니다.

이러한 레이아웃을 설정 하 여 지정할 수는 `ItemsLayout` 속성을 클래스에서 파생 되는 `ItemsLayout` 클래스입니다. 이 클래스에 다음 속성을 정의합니다.

- `Orientation`형식의 `ItemsLayoutOrientation`는 방향을 지정 합니다 `CollectionView` 항목 추가 됨에 따라 확장 합니다.
- `SnapPointsAlignment`형식의 `SnapPointsAlignment`, 끌기 지점 항목을 사용 하 여 정렬 되는 방법을 지정 합니다.
- `SnapPointsType`형식의 `SnapPointsType`를 스크롤할 때 맞춤 지점이의 동작을 지정 합니다.

이러한 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 개체 속성을 데이터 바인딩의 대상 수 있음을 의미 합니다. 끌기 지점에 대 한 자세한 내용은 참조 하세요. [맞춤 지점을](scrolling.md#snap-points) 에 [항목을 스크롤하여](scrolling.md) 가이드입니다.

`ItemsLayoutOrientation` 열거형은 다음 멤버를 정의 합니다.

- `Vertical` 나타내는 `CollectionView` 항목 추가 됨에 따라 세로로 확장 됩니다.
- `Horizontal` 나타내는 `CollectionView` 항목 추가 됨에 따라 가로로 확장 됩니다.

합니다 `ListItemsLayout` 클래스에서 상속 합니다 `ItemsLayout` 클래스 및 정적 정의 `VerticalList` 및 `HorizontalList` 멤버입니다. 이러한 멤버는 각각 세로 또는 가로 목록를 만드는 데 사용할 수 있습니다. 또는 `ListItemsLayout` 개체를 만들 수를 지정 하는 `ItemsLayoutOrientation` 인수로 열거형 멤버입니다.

`GridItemsLayout` 클래스에서 상속 합니다 `ItemsLayout` 클래스를 정의 `Span` 형식의 속성 `int`, 열 또는 모눈에 표시할 행의 수를 나타내는입니다. 기본값은 `Span` 속성은 1이 고 1 보다 크거나 같은 경우에 해당 값은 항상 이어야 합니다.

> [!NOTE]
> `CollectionView` 기본 레이아웃 엔진을 레이아웃 하는 데 사용 합니다.

## <a name="vertical-list"></a>세로 목록

기본적으로 `CollectionView` 세로 목록 레이아웃에서 해당 항목을 표시 합니다. 따라서 필요한 경우가 아니라면 설정 하는 `ItemsLayout` 이 레이아웃을 사용 하는 속성:

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

완성도 위해을 `CollectionView` 세로 목록에 항목을 설정 하 여 표시 설정할 수 있는 해당 `ItemsLayout` 정적 `ListItemsLayout.VerticalList` 멤버:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.VerticalList}">
    ...
</CollectionView>
```

또는 이렇게 설정 하 여는 `ItemsLayout` 개체 속성을 `ListItemsLayout` 클래스를 지정 하는 `Vertical` `ItemsLayoutOrientation` 인수로 열거형 멤버:

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

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.VerticalList
};
```

이 인해 새 항목 추가 됨에 따라 세로로 늘어남에 따라 하는 단일 열 목록에서:

[![IOS 및 Android에서 CollectionView 세로 목록 레이아웃의 스크린 샷](layout-images/vertical-list.png "CollectionView 세로 목록 레이아웃")](layout-images/vertical-list-large.png#lightbox "CollectionView 세로 목록 레이아웃")

## <a name="horizontal-list"></a>가로 목록

`CollectionView` 설정 하 여 가로 목록에서 해당 항목을 표시할 수 해당 `ItemsLayout` 속성을 정적 `ListItemsLayout.HorizontalList` 멤버:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.HorizontalList}">
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

또는 이렇게 설정 하 여 합니다 `ItemsLayout` 속성을를 `ListItemsLayout` 개체를 지정 하는 `Horizontal` `ItemsLayoutOrientation` 인수로 열거형 멤버:

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

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.HorizontalList
};
```

이 인해 새 항목 추가 됨에 따라 가로로 확장 되는 단일 행 목록:

[![IOS 및 Android에서 CollectionView 가로 목록 레이아웃의 스크린 샷](layout-images/horizontal-list.png "CollectionView 가로 목록 레이아웃")](layout-images/horizontal-list-large.png#lightbox "CollectionView 가로 목록 레이아웃")

## <a name="vertical-grid"></a>세로 격자 눈금

`CollectionView` 설정 하 여을 세로 격자 눈금에서 해당 항목을 표시할 수 있습니다 해당 `ItemsLayout` 속성을 `GridItemsLayout` 개체입니다 `Orientation` 속성이 `Vertical`:

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

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

기본적으로 세로 `GridItemsLayout` 단일 열에서 항목을 표시 합니다. 그러나 설정 하는이 예제는 `GridItemsLayout.Span` 속성을 2로 합니다. 이 인해 새 항목 추가 됨에 따라 세로로 확장 되는 2 열 그리드에:

[![IOS 및 Android에서 CollectionView 세로 격자 레이아웃의 스크린 샷](layout-images/vertical-grid.png "CollectionView 세로 격자 레이아웃")](layout-images/vertical-grid-large.png#lightbox "CollectionView 세로 모눈 레이아웃")

## <a name="horizontal-grid"></a>가로 눈금

`CollectionView` 설정 하 여 가로 격자 눈금에 항목을 표시할 수 있습니다 해당 `ItemsLayout` 속성을 `GridItemsLayout` 개체입니다 `Orientation` 속성이 `Horizontal`:

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

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

기본적으로 가로 `GridItemsLayout` 단일 행으로 항목을 표시 합니다. 그러나 설정 하는이 예제는 `GridItemsLayout.Span` 4 속성입니다. 이 인해 증가 함에 따라 가로로 새 항목이 추가 되는 행 4 표:

[![IOS 및 Android에서 CollectionView 가로 격자 레이아웃의 스크린 샷](layout-images/horizontal-grid.png "CollectionView 가로 격자 레이아웃")](layout-images/horizontal-grid-large.png#lightbox "CollectionView 가로 격자 레이아웃")

## <a name="right-to-left-layout"></a>오른쪽에서 왼쪽 레이아웃

`CollectionView` 레이아웃을 설정 하 여 오른쪽에서 왼쪽 흐름 방향으로 내용을 수 해당 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성을 [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft)합니다. 그러나는 `FlowDirection` 속성 이상적으로로 설정 해야 그러면 페이지 또는 루트 레이아웃 내에서 모든 요소는 페이지 또는 루트 레이아웃에서 흐름 방향을에 응답 합니다.

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

기본값 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 된 부모 요소에 대 한 [ `MatchParent` ](xref:Xamarin.Forms.FlowDirection.MatchParent)합니다. 따라서를 `CollectionView` 상속 합니다 `FlowDirection` 속성 값을는 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)는 상속를 `FlowDirection` 속성 값을를 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) . 이 인해 다음 스크린샷에 표시 된 오른쪽에서 왼쪽 레이아웃:

[![IOS 및 Android에서 CollectionView 세로 목록 오른쪽에서 왼쪽 레이아웃을 스크린샷](layout-images/vertical-list-rtl.png "CollectionView 세로 목록 오른쪽에서 왼쪽 레이아웃")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView 오른쪽에서 왼쪽 세로 레이아웃 목록")

흐름 방향에 대 한 자세한 내용은 참조 하세요. [오른쪽에서 왼쪽 지역화](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)합니다.

## <a name="item-sizing"></a>항목 크기 조정

기본적으로 각 항목에 `CollectionView` 는 개별적으로 측정 및 크기에 UI 요소를 제공 합니다 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 고정된 크기를 지정 하지. 변경할 수는이 동작으로 지정 된 된 `CollectionView.ItemSizingStrategy` 속성 값입니다. 이 속성 값 중 하나로 설정할 수 있습니다는 `ItemSizingStrategy` 열거형 멤버:

- `MeasureAllItems` – 각 항목은 개별적으로 측정 됩니다. 기본값입니다.
- `MeasureFirstItem` – 첫 번째 항목으로 같은 크기로 지정 되는 모든 후속 항목을 사용 하 여 첫 번째 항목만 측정 됩니다.

> [!IMPORTANT]
> `MeasureFirstItem` 전략을 크기 조정 항목 크기의 모든 항목에서 균일 할 것 및 성능이 향상된 됩니다 있는 상황에서 사용 해야 합니다.

다음 코드 예제에서는 설정 된 `ItemSizingStrategy` 속성:

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [오른쪽에서 왼쪽 지역화](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [항목을 스크롤하여](scrolling.md)
