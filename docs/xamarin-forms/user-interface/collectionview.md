---
title: Xamarin.Forms CollectionView
description: CollectionView은 다른 레이아웃 사양을 사용 하 여 데이터의 목록을 제공 하기 위한 유연 하 고 성능이 뛰어난 뷰입니다.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2018
ms.openlocfilehash: 46ab8169b31624e3862cf14f477bcd6cf8c8f3a3
ms.sourcegitcommit: 408b78dd6eded4696469e316af7922a5991f2211
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53246310"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

![미리 보기](~/media/shared/preview.png)

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` 데이터의 목록을 표시 하는 것에 대 한 뷰를 다른 레이아웃 사양을 사용 합니다. 보다 유연한 제공 하려고 하 고 효율적인 대안을 `ListView`입니다. 동안 합니다 `CollectionView` 및 `ListView` Api는 유사한, 몇 가지 주목할 만한 차이점이 있습니다.

- `CollectionView` 셀의 개념이 있습니다. 대신, 데이터 템플릿 목록에서 데이터의 각 항목의 모양을 정의할 수 사용 됩니다.
- `CollectionView` API 화면을 줄어듭니다 `ListView`합니다. 많은 속성 및 이벤트 `ListView` 에 없는 `CollectionView`합니다.
- `CollectionView` 목록 또는 표로에 세로 또는 가로로 표시 되는 데이터를 허용 하는 유연한 레이아웃 모델을 있습니다.

`CollectionView` 되는 설정 하 여 해당 `ItemsSource` 속성을 구현 하는 컬렉션 `IEnumerable`, 및 해당 `ItemTemplate` 속성을는 `DataTemplate` 각 목록 항목 모양을 정의 하는 합니다. 또한 해당 `ItemsLayout` 속성 설정 해야 합니다는 `ItemsLayout` 클래스 목록에 대 한 레이아웃 사양이 정의 합니다.

> [!IMPORTANT]
> `CollectionView` 현재는 초기 미리 보기 이며 대부분의 계획 된 기능이 부족 합니다. 또한 API 구현을 완료 되 면 변경 됩니다.

`CollectionView` Xamarin.Forms 4.0-pre1 제공 됩니다. 그러나 현재 실험적 이므로 코드를 다음 줄을 추가 하 여 에서만 사용할 수 있습니다 하 `AppDelegate` 또는 ios의 경우 클래스에 `MainActivity` 호출 하기 전에 android에서 클래스 `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

## <a name="populating-a-collectionview-with-data"></a>데이터 CollectionView 채우기

A `CollectionView` 설정 하 여 데이터를 채운 해당 `ItemSource` 속성을 구현 하는 컬렉션 `IEnumerable`합니다. 초기화 하 여 XAML에서 항목을 추가할 수는 `ItemsSource` 속성 항목의 배열:

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
> 합니다 `x:Array` 요소에는 `Type` 배열에 있는 항목의 유형을 나타내는 특성입니다.

또한를 `CollectionView` 바인딩할 데이터 바인딩을 사용 하 여 데이터로 채울 수 있습니다 해당 `ItemsSource` 속성을 `IEnumerable` 컬렉션입니다. XAML에서이 통해 여 `Binding` 태그 확장:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

이 예제에서는 `ItemsSource` 속성 데이터를 바인딩하는 `Monkeys` 반환 하는 연결 된 뷰 모델의 속성을 `IList<Monkey>` 컬렉션. 다음 코드 예제는 `Monkey` 네 가지 속성을 포함 하는 클래스:

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

## <a name="providing-a-view-for-the-empty-state"></a>비어 있는 상태에 대 한 보기를 제공합니다.

경우는 `CollectionView` 표시할 데이터가 없습니다 사용자에 게 적절 한 메시지를 표시할 수 있습니다. 설정 하 여 이렇게 합니다 `CollectionView.EmptyView` 속성을는 `object` 는 표시 될 때 지정 된 컬렉션을 `ItemSource` 속성이 비어:

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}">
    <CollectionView.EmptyView>
        <Label Text="No items to display" />
    </CollectionView.EmptyView>
</CollectionView>
```

합니다 `EmptyView` 속성 설정할 수 있습니다는 `string`, 바인딩 또는 보기 이므로 필요한 경우에 대화형 콘텐츠를 포함할 수 있습니다.

또한 합니다 `EmptyViewTemplate` 속성 설정할 수 있습니다를 `DataTemplate` 서식을 지정 하는 데 수는 `EmptyView`.

## <a name="defining-list-item-appearance"></a>목록 항목 모양을 정의

각 항목에 대해 데이터의 모양을 합니다 `CollectionView` 설정 하 여 정의할 수 있습니다 합니다 `CollectionView.ItemTemplate` 속성을는 `DataTemplate`:

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

이 예제에서는 합니다 `DataTemplate` 포함를 `Grid` 사용 하 여는 `Image`, 두 개의 `Label` 의 속성을 바인딩하는 모든 인스턴스에 `Monkey` 개체.

데이터 템플릿에 대 한 자세한 내용은 참조 하세요. [데이터 템플릿을](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)합니다.

## <a name="specifying-a-layout"></a>레이아웃을 지정합니다.

기본적으로 `CollectionView` 세로 목록에 항목을 표시 합니다. 그러나 다음 레이아웃 사양을 사용할 수 있습니다.

- 세로 목록-새 항목 추가 됨에 따라 세로로 늘어남에 따라 하는 단일 열 목록입니다.
- 가로 목록-새 항목 추가 됨에 따라 가로로 증가 하는 단일 행 목록입니다.
- 세로 격자 눈금-새 항목 추가 됨에 따라 세로로 늘어남에 따라 하는 여러 열 표입니다.
- 가로 격자 눈금-새 항목 추가 됨에 따라 가로로 증가 하는 다중 행 표입니다.

이러한 레이아웃을 설정 하 여 지정할 수 있습니다는 `CollectionView.ItemsLayout` 속성을를 `ItemsLayout` 클래스를 사용 하 여는 `ListItemsLayout` 목록 레이아웃을 제공 하는 클래스 및 `GridItemsLayout` 표 형태 레이아웃을 제공 하는 클래스입니다.

합니다 `ItemsLayout` 클래스를 정의 합니다 `Orientation` 형식의 속성 `ItemsLayoutOrientation`합니다. 합니다 `ItemsLayoutOrientation` 열거형 정의 `Vertical` 고 `Horizontal` 멤버 값입니다.

합니다 `ListItemsLayout` 클래스에서 상속 합니다 `ItemsLayout` 클래스 및 정적 정의 `VerticalList` 및 `HorizontalList` 멤버입니다. 이러한 멤버는 각각 세로 또는 가로 목록를 만드는 데 사용할 수 있습니다. 또는 `ListItemsLayout` 인스턴스를 만들 수를 지정 하는 `ItemsLayoutOrientation` 인수로 열거형 멤버입니다.

`GridItemsLayout` 클래스에서 상속 합니다 `ItemsLayout` 클래스를 정의 `Span` 형식의 속성 `int`, 열 또는 모눈에 표시할 행의 수를 나타내는입니다. 기본값은 `Span` 속성은 1이 고 1 보다 크거나 같은 경우에 해당 값은 항상 이어야 합니다.

### <a name="vertical-list"></a>세로 목록

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

또는 이렇게 설정 하 여는 `ItemsLayout` 의 인스턴스에 대 한 속성을 `ListItemsLayout` 클래스를 지정 하는 `Vertical` `ItemsLayoutOrientation` 인수로 열거형 멤버:

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

이 인해 새 항목 추가 됨에 따라 세로로 늘어남에 따라 하는 단일 열 목록에서:

[![CollectionView 세로 목록 레이아웃](collectionview-images/vertical-list.png "CollectionView 세로 목록 레이아웃")](collectionview-images/vertical-list-large.png#lightbox)

### <a name="horizontal-list"></a>가로 목록

A `CollectionView` 설정 하 여 가로 목록에서 해당 항목을 표시할 수 해당 `ItemsLayout` 속성을 정적 `ListItemsLayout.HorizontalList` 멤버:

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

또는 이렇게 설정 하 여는 `ItemsLayout` 의 인스턴스에 대 한 속성을 `ListItemsLayout` 클래스를 지정 하는 `Horizontal` `ItemsLayoutOrientation` 인수로 열거형 멤버:

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

이 인해 새 항목 추가 됨에 따라 가로로 확장 되는 단일 행 목록:

[![가로 목록 레이아웃 CollectionView](collectionview-images/horizontal-list.png "CollectionView 가로 목록 레이아웃")](collectionview-images/horizontal-list-large.png#lightbox)

### <a name="vertical-grid"></a>세로 격자 눈금

A `CollectionView` 설정 하 여을 세로 격자 눈금에서 해당 항목을 표시할 수 있습니다 해당 `ItemsLayout` 속성을 `GridItemsLayout` 인스턴스입니다 `Orientation` 속성 `Vertical`:

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

기본적으로 세로 `GridItemsLayout` 단일 열에서 항목을 표시 합니다. 그러나 설정 하는이 예제는 `GridItemsLayout.Span` 속성을 2로 합니다. 이 인해 새 항목 추가 됨에 따라 세로로 확장 되는 2 열 그리드에:

[![CollectionView 세로 격자 레이아웃](collectionview-images/vertical-grid.png "CollectionView 세로 모눈 레이아웃")](collectionview-images/vertical-grid-large.png#lightbox)

### <a name="horizontal-grid"></a>가로 눈금

A `CollectionView` 설정 하 여 가로 격자 눈금에 항목을 표시할 수 있습니다 해당 `ItemsLayout` 속성을 `GridItemsLayout` 인스턴스입니다 `Orientation` 속성 `Horizontal`:

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

기본적으로 가로 `GridItemsLayout` 단일 행으로 항목을 표시 합니다. 그러나 설정 하는이 예제는 `GridItemsLayout.Span` 4 속성입니다. 이 인해 증가 함에 따라 가로로 새 항목이 추가 되는 행 4 표:

[![CollectionView 가로 격자 레이아웃](collectionview-images/horizontal-grid.png "CollectionView 가로 격자 레이아웃")](collectionview-images/horizontal-grid-large.png#lightbox)

## <a name="scrolling"></a>스크롤

`CollectionView` 요청 된 항목을 사용 하 여 뷰로 스크롤할 수는 `ScrollTo` 메서드. 이 메서드는 지정된 된 인덱스에 항목을 뷰로 스크롤합니다.

```csharp
_collectionView.ScrollTo(12);
```

또는이 메서드의 오버 로드는 지정된 된 항목 보기로 스크롤할 사용할 수 있습니다.

```csharp
var viewModel = BindingContext as MonkeysViewModel;
var monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
_collectionView.ScrollTo(monkey);
```

두 오버 로드에 항목을 그룹, 스크롤 애니메이션 효과를 주는 여부와 스크롤에 완료 된 (start, center, 종료)를 만든 후 항목의 정확한 위치를 지정 하는 지정 해야 추가 인수를 허용 합니다.

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
