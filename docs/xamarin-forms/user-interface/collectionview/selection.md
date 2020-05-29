---
title: Xamarin.FormsCollectionView 선택
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 39f118d7073fc551923f891681c8c6cf6a4c5ddd
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137386"
---
# <a name="xamarinforms-collectionview-selection"></a>Xamarin.FormsCollectionView 선택

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)항목 선택을 제어 하는 다음 속성을 정의 합니다.

- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode), 형식의, [`SelectionMode`](xref:Xamarin.Forms.SelectionMode) 선택 모드입니다.
- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)`object`목록에서 선택 된 항목인 형식의입니다. 이 속성의 기본 바인딩 모드는 `TwoWay` 이며, `null` 선택 된 항목이 없는 경우에는 값이 있습니다.
- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)`IList<object>`목록에서 선택한 항목의 형식입니다. 이 속성의 기본 바인딩 모드는 `OneWay` 이며 `null` 선택 된 항목이 없는 경우에는 값이 있습니다.
- [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)`ICommand`-선택한 항목이 변경 될 때 실행 되는 형식의입니다.
- [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)에 `object` 전달 되는 매개 변수인 형식의입니다 `SelectionChangedCommand` .

이 모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되며, 이는 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다.

기본적으로 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 선택은 사용 하지 않도록 설정 되어 있습니다. 그러나 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) 속성 값을 열거형 멤버 중 하나로 설정 하 여이 동작을 변경할 수 있습니다 [`SelectionMode`](xref:Xamarin.Forms.SelectionMode) .

- `None`– 항목을 선택할 수 없음을 나타냅니다. 이것은 기본값입니다.
- `Single`– 선택한 항목이 강조 표시 된 상태로 단일 항목을 선택할 수 있음을 나타냅니다.
- `Multiple`– 선택한 항목을 강조 표시 하 여 여러 항목을 선택할 수 있음을 나타냅니다.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)[`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) 사용자가 목록에서 항목을 선택 하거나 응용 프로그램에서 속성을 설정 하 여 속성이 변경 될 때 발생 하는 이벤트를 정의 합니다. 또한이 이벤트는 속성이 변경 될 때에도 발생 [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) 합니다. 이벤트와 함께 제공 되는 개체에는 [`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs) `SelectionChanged` 두 가지 속성이 있습니다 `IReadOnlyList<object>` .

- `PreviousSelection`– 선택 항목을 변경 하기 전에 선택 된 항목의 목록입니다.
- `CurrentSelection`– 선택 항목을 변경한 후 선택 된 항목의 목록입니다.

또한에는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) `UpdateSelectedItems` [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) 선택한 항목의 목록으로 속성을 업데이트 하는 메서드와 단일 변경 알림만 발생 합니다.

## <a name="single-selection"></a>단일 선택

[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)속성이로 설정 되 면 `Single` 의 단일 항목을 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 선택할 수 있습니다. 항목을 선택 하면 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) 속성이 선택 된 항목의 값으로 설정 됩니다. 이 속성이 변경 되 면이 [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) 실행 되 고 (값이로 [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) 전달 됨 `ICommand` ) [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 이벤트가 발생 합니다.

다음 XAML 예제에서는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 단일 항목 선택에 응답할 수 있는을 보여 줍니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

이 코드 예제에서 이벤트 처리기는 이벤트가 발생할 `OnCollectionViewSelectionChanged` 때 실행 되 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 고, 이벤트 처리기는 이전에 선택한 항목을 검색 하 고 현재 선택한 항목을 검색 합니다.

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)이 이벤트는 속성을 변경한 결과로 발생 하는 변경 내용으로 인해 발생 될 수 있습니다 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) .

다음 스크린샷은의 단일 항목 선택 항목을 보여 줍니다 [`CollectionView`](xref:Xamarin.Forms.CollectionView) .

[![IOS 및 Android에서 단일 항목이 선택 된 CollectionView 세로 목록의 스크린샷](selection-images/single-selection.png "단일 선택 영역을 포함 하는 CollectionView 세로 목록")](selection-images/single-selection-large.png#lightbox "단일 선택 영역을 포함 하는 CollectionView 세로 목록")

## <a name="multiple-selection"></a>다중 선택

[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)속성이로 설정 되 면 `Multiple` 의 여러 항목을 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 선택할 수 있습니다. 항목을 선택 하면 [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) 속성이 선택 된 항목으로 설정 됩니다. 이 속성이 변경 되 면이 [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) 실행 되 고 (값이로 [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) 전달 됨 `ICommand` ) [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 이벤트가 발생 합니다.

다음 XAML 예제에서는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 여러 항목 선택에 응답할 수 있는을 보여 줍니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

이 코드 예제에서 이벤트 처리기는 이벤트가 발생할 `OnCollectionViewSelectionChanged` 때 실행 되 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 고, 이벤트 처리기는 이전에 선택한 항목을 검색 하 고 현재 선택한 항목을 검색 합니다.

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)이 이벤트는 속성을 변경한 결과로 발생 하는 변경 내용으로 인해 발생 될 수 있습니다 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) .

다음 스크린샷은에서 여러 항목을 선택 하 여 보여 줍니다 [`CollectionView`](xref:Xamarin.Forms.CollectionView) .

[![IOS 및 Android에서 여러 항목이 선택 된 CollectionView 세로 목록 스크린샷](selection-images/multiple-selection.png "여러 선택 영역을 포함 하는 CollectionView 세로 목록")](selection-images/multiple-selection-large.png#lightbox "여러 선택 영역을 포함 하는 CollectionView 세로 목록")

## <a name="single-pre-selection"></a>단일 사전 선택

[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)속성이로 설정 된 경우 `Single` 속성을 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 항목으로 설정 하 여의 단일 항목을 미리 선택할 수 있습니다 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) . 다음 XAML 예제에서는 `CollectionView` 단일 항목을 미리 선택 하는을 보여 줍니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey}">
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey");
```

> [!NOTE]
> [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)속성의 기본 바인딩 모드는 `TwoWay` 입니다.

[`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)속성 데이터는 `SelectedMonkey` 형식의 연결 된 뷰 모델의 속성에 바인딩됩니다 `Monkey` . 기본적으로 `TwoWay` 바인딩이 사용 되므로 사용자가 선택한 항목을 변경 하면 속성의 값 `SelectedMonkey` 이 선택한 개체로 설정 됩니다 `Monkey` . `SelectedMonkey`속성은 클래스에 정의 되 `MonkeysViewModel` 고,는 컬렉션의 네 번째 항목으로 설정 됩니다 `Monkeys` .

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

따라서가 표시 되 면 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 목록의 네 번째 항목이 미리 선택 됩니다.

[![IOS 및 Android에서 단일 사전 선택이 포함 된 CollectionView 세로 목록 스크린샷](selection-images/single-pre-selection.png "단일 사전 선택 영역을 포함 하는 CollectionView 세로 목록")](selection-images/single-pre-selection-large.png#lightbox "단일 사전 선택 영역을 포함 하는 CollectionView 세로 목록")

## <a name="multiple-pre-selection"></a>다중 미리 선택

[`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)속성이로 설정 되 면 `Multiple` 의 여러 항목을 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 미리 선택할 수 있습니다. 다음 XAML 예제에서는 `CollectionView` 여러 항목을 미리 선택할 수 있도록 하는을 보여 줍니다.

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectedItems="{Binding SelectedMonkeys}">
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemsProperty, "SelectedMonkeys");
```

> [!NOTE]
> [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)속성의 기본 바인딩 모드는 `OneWay` 입니다.

[`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)속성 데이터는 `SelectedMonkeys` 형식의 연결 된 뷰 모델의 속성에 바인딩됩니다 `ObservableCollection<object>` . `SelectedMonkeys`속성은 클래스에 정의 되 `MonkeysViewModel` 고,는 컬렉션의 두 번째, 네 번째 및 다섯 번째 항목으로 설정 됩니다 `Monkeys` .

```csharp
namespace CollectionViewDemos.ViewModels
{
    public class MonkeysViewModel : INotifyPropertyChanged
    {
        ...
        ObservableCollection<object> selectedMonkeys;
        public ObservableCollection<object> SelectedMonkeys
        {
            get
            {
                return selectedMonkeys;
            }
            set
            {
                if (selectedMonkeys != value)
                {
                    selectedMonkeys = value;
                }
            }
        }

        public MonkeysViewModel()
        {
            ...
            SelectedMonkeys = new ObservableCollection<object>()
            {
                Monkeys[1], Monkeys[3], Monkeys[4]
            };
        }
        ...
    }
}
```

따라서 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 가 표시 되 면 목록에서 두 번째, 네 번째 및 다섯 번째 항목이 미리 선택 됩니다.

[![IOS 및 Android에서 여러 미리 선택 된 CollectionView 세로 목록 스크린샷](selection-images/multiple-pre-selection.png "여러 사전 선택 영역을 포함 하는 CollectionView 세로 목록")](selection-images/multiple-pre-selection-large.png#lightbox "여러 사전 선택 영역을 포함 하는 CollectionView 세로 목록")

## <a name="clear-selections"></a>선택 영역 지우기

[`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)및 속성은로 [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) 설정 하거나 바인딩할 개체를로 설정 하 여 지울 수 있습니다 `null` .

## <a name="change-selected-item-color"></a>선택한 항목 색 변경

[`CollectionView`](xref:Xamarin.Forms.CollectionView)에는 `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) 에서 선택한 항목에 대 한 시각적 변경을 시작 하는 데 사용할 수 있는가 있습니다 `CollectionView` . 이에 대 한 일반적인 사용 사례는 `VisualState` 다음 XAML 예제에 표시 된 대로 선택한 항목의 배경색을 변경 하는 것입니다.

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
> 을 포함 하는에는 속성 [`Style`](xref:Xamarin.Forms.Style) `Selected` `VisualState` [`TargetType`](xref:Xamarin.Forms.Style.TargetType) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 값으로 설정 되는의 루트 요소 형식인 속성 값이 `ItemTemplate` 있어야 합니다.

이 예제에서는 [`Style.TargetType`](xref:Xamarin.Forms.Style.TargetType) 의 루트 요소가 이므로 속성 값이로 설정 됩니다 `Grid` [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`Grid`](xref:Xamarin.Forms.Grid) . 는의 `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) 항목이 선택 될 때 [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 항목의이로 설정 되도록 지정 합니다 `LightSkyBlue` .

[![IOS 및 Android에서 사용자 지정 단일 선택 색이 있는 CollectionView 세로 목록의 스크린샷](selection-images/single-selection-color.png "사용자 지정 단일 선택 색이 있는 CollectionView 세로 목록")](selection-images/single-selection-color-large.png#lightbox "사용자 지정 단일 선택 색이 있는 CollectionView 세로 목록")

시각적 상태에 대 한 자세한 내용은 [ Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)를 참조 하세요.

## <a name="disable-selection"></a>선택 사용 안 함

[`CollectionView`](xref:Xamarin.Forms.CollectionView)선택은 기본적으로 사용 하지 않도록 설정 되어 있습니다. 그러나에서 `CollectionView` 선택이 활성화 되어 있으면 속성을로 설정 하 여 사용 하지 않도록 설정할 수 있습니다 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) `None` .

```xaml
<CollectionView ...
                SelectionMode="None" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    SelectionMode = SelectionMode.None
};
```

속성이로 설정 된 경우의 항목을 선택할 수 없으며,이 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) `None` 속성은 [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) 유지 되 `null` 고 이벤트는 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 발생 하지 않습니다.

> [!NOTE]
> 항목을 선택 하 고 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) 속성을에서로 변경 하면 `Single` `None` [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) 속성이로 설정 되 `null` 고 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 빈 속성을 사용 하 여 이벤트가 발생 합니다 `CurrentSelection` .

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin.Forms시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)
