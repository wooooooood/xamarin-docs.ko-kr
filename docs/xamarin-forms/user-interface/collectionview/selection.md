---
title: Xamarin.Forms CollectionView 선택
description: CollectionView 선택은 기본적으로 비활성화 됩니다. 그러나 단일 및 다중 선택을 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 1ffed60253889491636fa105dd444ced9c2bedf5
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65048227"
---
# <a name="xamarinforms-collectionview-selection"></a>Xamarin.Forms CollectionView 선택

![](~/media/shared/preview.png "이 API는 현재 시험판")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` 항목 선택을 제어 하는 다음 속성을 정의 합니다.

- `SelectionMode`형식의 `SelectionMode`, 선택 모드입니다.
- `SelectedItem`형식의 `object`, 목록에서 선택한 항목입니다. 이 속성에는 `null` 선택 된 항목이 값입니다.
- `SelectedItems`형식의 `IList<object>`, 목록에서 항목을 선택 합니다. 이 속성은 읽기 전용, 있고는 `null` 항목이 선택 되 면 값입니다.
- `SelectionChangedCommand`형식의 `ICommand`, 선택한 항목이 변경 될 때 실행 되는 합니다.
- `SelectionChangedCommandParameter`를 형식 `object`에 전달 되는 매개 변수는 `SelectionChangedCommand`합니다.

이러한 모든 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 개체 속성을 데이터 바인딩의 대상 수 있음을 의미 합니다.

기본적으로 `CollectionView` 선택할 수 없게 됩니다. 그러나 설정 하 여이 동작을 변경할 수 있습니다 합니다 `SelectionMode` 속성 값 중 하나는 `SelectionMode` 열거형 멤버:

- `None` – 항목을 선택할 수를 나타냅니다. 기본값입니다.
- `Single` – 단일 항목을 선택할 수 있는 있는, 선택한 항목 강조 표시를 사용 하 여 나타냅니다.
- `Multiple` –는 여러 항목을 선택할 수, 선택한 항목 강조 표시를 사용 하 여 나타냅니다.

`CollectionView` 정의 `SelectionChanged` 될 때 발생 하는 이벤트를 `SelectedItem` 속성이 변경 되거나 응용 프로그램 속성을 설정 하는 경우 또는 목록에서 항목을 선택 하는 사용자입니다. 또한이 이벤트는 또한 발생 시기를 `SelectedItems` 속성 변경 합니다. 합니다 `SelectionChangedEventArgs` 와 함께 제공 되는 개체를 `SelectionChanged` 이벤트에 두 개의 속성의 형식은 모두 `IReadOnlyList<object>`:

- `PreviousSelection` -선택을 변경 하기 전에 선택 된 항목의 목록입니다.
- `CurrentSelection` – 선택 변경 후 선택 된 항목의 목록입니다.

## <a name="single-selection"></a>단일 선택

경우는 `SelectionMode` 속성 `Single`, 단일 항목을 `CollectionView` 선택할 수 있습니다. 항목을 선택 하면는 `SelectedItem` 속성이 선택한 항목의 값으로 설정 됩니다. 이 속성이 변경 되 면를 `SelectionChangedCommand` 실행 됩니다 (값을 사용 하 여는 `SelectionChangedCommandParameter` 에 전달 되는 `ICommand`), 및 `SelectionChanged` 이벤트가 발생 합니다.

다음 XAML 예제에서는 `CollectionView` 단일 선택 항목에 응답할 수입니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

이 코드 예제에서는 합니다 `OnCollectionViewSelectionChanged` 이벤트 처리기가 실행 때는 `SelectionChanged` 현재 선택한 항목 이전에 선택한 항목을 검색 하는 이벤트 처리기를 사용 하 여 이벤트 발생:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> 합니다 `SelectionChanged` 변경의 결과로 발생 하는 변경 하 여 이벤트를 발생 시킬 수는 `SelectionMode` 속성입니다.

다음 스크린샷은의 단일 항목 선택 표시는 `CollectionView`:

[![IOS 및 Android에서의 단일 선택 CollectionView 세로 목록 스크린샷](selection-images/single-selection.png "단일 선택 하 여 세로 목록 CollectionView")](selection-images/single-selection-large.png#lightbox "단일 CollectionView 세로 목록 선택")

## <a name="multiple-selection"></a>다중 선택

경우는 `SelectionMode` 속성이로 설정 된 `Multiple`에서 여러 항목을 `CollectionView` 선택할 수 있습니다. 항목이 선택 되는 `SelectedItems` 속성이 선택된 된 항목에 설정 됩니다. 이 속성이 변경 되 면를 `SelectionChangedCommand` 실행 됩니다 (값을 사용 하 여는 `SelectionChangedCommandParameter` 에 전달 되는 `ICommand`), 및 `SelectionChanged` 이벤트가 발생 합니다.

다음 XAML 예제에서는 `CollectionView` 는 여러 항목 선택에 응답할 수 있습니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

이 코드 예제에서는 합니다 `OnCollectionViewSelectionChanged` 이벤트 처리기가 실행 때는 `SelectionChanged` 이전에 선택한 항목을 현재 선택된 된 항목을 검색 하는 이벤트 처리기를 사용 하 여 이벤트 발생:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> 합니다 `SelectionChanged` 변경의 결과로 발생 하는 변경 하 여 이벤트를 발생 시킬 수는 `SelectionMode` 속성입니다.

다음 스크린샷에서 표시에서 여러 항목을 선택할을 `CollectionView`:

[![IOS 및 Android에서 다중 선택 사용 하 여 CollectionView 세로 목록 스크린샷](selection-images/multiple-selection.png "CollectionView 세로 다중 선택 목록")](selection-images/multiple-selection-large.png#lightbox "CollectionView 세로 목록 다중 선택")

## <a name="single-pre-selection"></a>단일 사전 선택

경우는 `SelectionMode` 속성이로 설정 되어 `Single`의 단일 항목을는 `CollectionView` 설정 하 여 미리 선택할 수 있습니다는 `SelectedItem` 속성 항목을 합니다. 다음 XAML 예제에서는 `CollectionView` 미리 선택 하는 단일 항목:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey, Mode=TwoWay}">
    ...
</CollectionView>
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey", BindingMode.TwoWay);
```

합니다 `SelectedItem` 속성 데이터를 바인딩하는 `SelectedMonkey` 형식인 연결 된 뷰 모델의 속성 `Monkey`합니다. A `TwoWay` 경우 사용자는 선택한 항목의 값을 변경 하도록 바인딩을 사용 합니다 `SelectedMonkey` 속성이 설정 됩니다 하 여 선택한 `Monkey` 개체입니다. 합니다 `SelectedMonkey` 속성에 정의 된 합니다 `MonkeysViewModel` 클래스와의 네 번째 항목으로 설정 됩니다는 `Monkeys` 컬렉션:

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    ...
    public ObservableCollection<Monkey> Monkeys { get; private set; }

    Monkey selectedMonkey;
    public Monkey SelectedMonkey
    {
        get
        {
            return selectedMonkey;
        }
        set
        {
            if (selectedMonkey != value)
            {
                selectedMonkey = value;
            }
        }
    }

    public MonkeysViewModel()
    {
        ...
        selectedMonkey = Monkeys.Skip(3).FirstOrDefault();
    }
    ...
}
```

따라서,는 `CollectionView` 나타나면 목록에서 네 번째 항목은 미리 선택:

[![IOS 및 Android에서 번의 사전 선택 CollectionView 세로 목록 스크린샷](selection-images/single-pre-selection.png "번의 사전 선택 CollectionView 세로 목록")](selection-images/single-pre-selection-large.png#lightbox "CollectionView 세로 목록 번의 사전 선택")

## <a name="multiple-pre-selection"></a>여러 사전 선택

경우는 `SelectionMode` 속성이 `Multiple`에서 여러 항목을 `CollectionView` 미리 선택할 수 있습니다. 다음 XAML 예제에서는 `CollectionView` 여러 항목의 사전 선택이 가능 합니다.

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple">
    ...
</CollectionView>
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

여러 항목을 `CollectionView` 추가 하 여 미리 선택할 수는 `SelectedItems` 속성:

```csharp
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(1).FirstOrDefault());
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(3).FirstOrDefault());
collectionView.SelectedItems.Add(viewModel.Monkeys.Skip(4).FirstOrDefault());
```

> [!NOTE]
> `SelectedItems` 속성은 읽기 전용 이므로 하므로 양방향 데이터 항목을 미리 선택할 바인딩을 사용할 수 있습니다.

따라서,는 `CollectionView` 나타납니다, 두 번째, 네 번째, 다섯 번째 항목 목록에서 미리 선택 되며:

[![IOS 및 Android에서 다중 사전 항목 선택 CollectionView 세로 목록 스크린샷](selection-images/multiple-pre-selection.png "사전 여러 선택 영역을 사용 하 여 세로 목록 CollectionView")](selection-images/multiple-pre-selection-large.png#lightbox "CollectionView 세로 여러 사전 선택 목록")

## <a name="change-selected-item-color"></a>선택한 항목 색 변경

`CollectionView` 에 `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState) 에서 선택한 항목을 시각적으로 변화를 시작 하려면 사용할 수 있는 여 `CollectionView`입니다. 이 대 한 일반적인 사용 사례 `VisualState` 다음 XAML 예제에 나와 있는 선택한 항목의 배경색을 변경 합니다.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="Grid">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="Normal" />
                        <VisualState x:Name="Selected">
                            <VisualState.Setters>
                                <Setter Property="BackgroundColor"
                                        Value="LightSkyBlue" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}"
                        SelectionMode="Single">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        ...
                    </Grid>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

> [!IMPORTANT]
> [ `Style` ](xref:Xamarin.Forms.Style) 포함 하는 `Selected` `VisualState` 있어야를 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) 의 루트 요소 형식인 속성 값을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate),으로 설정 된는 `ItemTemplate` 속성 값입니다.

이 예제에서는 합니다 [ `Style.TargetType` ](xref:Xamarin.Forms.Style.TargetType) 속성 값으로 설정 됩니다 `Grid` 때문에의 루트 요소를 `ItemTemplate` 는 [ `Grid` ](xref:Xamarin.Forms.Grid)합니다. `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState) 항목이 지정 합니다 `CollectionView` 확인란이 선택 되는 [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) 항목으로 설정 됩니다 `LightSkyBlue`:

[![IOS 및 Android에서 단일 선택 사용자 지정 색을 사용 하 여 CollectionView 세로 목록 스크린샷](selection-images/single-selection-color.png "단일 선택 사용자 지정 색을 사용 하 여 세로 목록 CollectionView") ] (selection-images/single-selection-color-large.png#lightbox " 단일 선택 사용자 지정 색을 사용 하 여 CollectionView 세로 목록")

시각적 상태에 대 한 자세한 내용은 참조 하세요. [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)합니다.

## <a name="disable-selection"></a>선택 영역을 사용 하지 않도록 설정

`CollectionView` 선택은 기본적으로 비활성화 됩니다. 그러나 경우는 `CollectionView` 에 사용 하도록 설정 하 게 선택 설정 하 여 사용할 수는 `SelectionMode` 속성을 `None`:

```xaml
<CollectionView ...
                SelectionMode="None" />
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    SelectionMode = SelectionMode.None
};
```

경우는 `SelectionMode` 속성이로 설정 된 `None`, 항목는 `CollectionView` 선택할 수 없습니다는 `SelectedItem` 속성 상태로 유지 됩니다 `null`, 및 `SelectionChanged` 이벤트 발생 하지 것입니다.

> [!NOTE]
> 항목이 선택 된 경우 및 `SelectionMode` 에서 속성이 변경 될 `Single` 하 `None`의 `SelectedItem` 속성으로 설정 됩니다 `null` 및 `SelectionChanged` 빈 이벤트가 발생 `CurrentSelection` 속성입니다.

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
