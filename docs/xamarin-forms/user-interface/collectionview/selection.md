---
title: Xamarin.ios CollectionView 선택
description: 기본적으로 CollectionView 선택은 사용 하지 않도록 설정 되어 있습니다. 그러나 단일 및 다중 선택을 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: f1a3e8bb8959588e64339f70268370440f356be9
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "68738967"
---
# <a name="xamarinforms-collectionview-selection"></a>Xamarin.ios CollectionView 선택

![](~/media/shared/preview.png "이 API는 현재 시험판임")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)항목 선택을 제어 하는 다음 속성을 정의 합니다.

- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode), 형식의 [`SelectionMode`](xref:Xamarin.Forms.SelectionMode), 선택 모드입니다.
- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)목록에서 선택 `object`된 항목인 형식의입니다. 이 속성의 `TwoWay`기본 바인딩 모드는 이며 `null` , 선택 된 항목이 없는 경우에는 값이 있습니다.
- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)목록에서 선택한 `IList<object>`항목의 형식입니다. 이 속성의 `OneWay`기본 바인딩 모드는 이며 `null` 선택 된 항목이 없는 경우에는 값이 있습니다.
- [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)-선택한 항목이 `ICommand`변경 될 때 실행 되는 형식의입니다.
- [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)에 전달 `object` `SelectionChangedCommand`되는 매개 변수인 형식의입니다.

이 모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되며, 이는 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다.

기본적 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 으로 선택은 사용 하지 않도록 설정 되어 있습니다. 그러나 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) 속성 값을 [`SelectionMode`](xref:Xamarin.Forms.SelectionMode) 열거형 멤버 중 하나로 설정 하 여이 동작을 변경할 수 있습니다.

- `None`– 항목을 선택할 수 없음을 나타냅니다. 기본값입니다.
- `Single`– 선택한 항목이 강조 표시 된 상태로 단일 항목을 선택할 수 있음을 나타냅니다.
- `Multiple`– 선택한 항목을 강조 표시 하 여 여러 항목을 선택할 수 있음을 나타냅니다.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)사용자가 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 목록에서 항목을 선택 하거나 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) 응용 프로그램에서 속성을 설정 하 여 속성이 변경 될 때 발생 하는 이벤트를 정의 합니다. 또한이 이벤트는 [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) 속성이 변경 될 때에도 발생 합니다. 이벤트와 함께 제공 되는 `IReadOnlyList<object>` [개체에는두가지속성이있습니다.`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs) `SelectionChanged`

- `PreviousSelection`– 선택 항목을 변경 하기 전에 선택 된 항목의 목록입니다.
- `CurrentSelection`– 선택 항목을 변경한 후 선택 된 항목의 목록입니다.

## <a name="single-selection"></a>단일 선택

속성이로 `Single`설정 되 면의 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 단일 항목을 선택할 수 있습니다. [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) 항목을 선택 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) 하면 속성이 선택 된 항목의 값으로 설정 됩니다. 이 속성이 변경 [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) 되 면이 실행 되 고 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) (값 [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) 이로 전달 `ICommand`됨) 이벤트가 발생 합니다.

다음 XAML 예제에서는 단일 항목 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 선택에 응답할 수 있는을 보여 줍니다.

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

이 코드 예제 `OnCollectionViewSelectionChanged` 에서 이벤트 처리기는 이벤트가 발생할 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 때 실행 되 고, 이벤트 처리기는 이전에 선택한 항목을 검색 하 고 현재 선택한 항목을 검색 합니다.

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> 이 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 이벤트는 속성을 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) 변경한 결과로 발생 하는 변경 내용으로 인해 발생 될 수 있습니다.

다음 스크린샷은의 단일 항목 선택 항목을 [`CollectionView`](xref:Xamarin.Forms.CollectionView)보여 줍니다.

[![IOS 및 Android에서 단일 항목이 선택 된 CollectionView 세로 목록의 스크린샷](selection-images/single-selection.png "단일 선택 영역을 포함 하는 CollectionView 세로 목록")](selection-images/single-selection-large.png#lightbox "단일 선택 영역을 포함 하는 CollectionView 세로 목록")

## <a name="multiple-selection"></a>다중 선택

속성이로 `Multiple`설정 되 면의 여러 항목 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 을 선택할 수 있습니다. [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) 항목을 선택 [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) 하면 속성이 선택 된 항목으로 설정 됩니다. 이 속성이 변경 [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) 되 면이 실행 되 고 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) (값 [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) 이로 전달 `ICommand`됨) 이벤트가 발생 합니다.

다음 XAML 예제에서는 여러 항목 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 선택에 응답할 수 있는을 보여 줍니다.

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

이 코드 예제 `OnCollectionViewSelectionChanged` 에서 이벤트 처리기는 이벤트가 발생할 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 때 실행 되 고, 이벤트 처리기는 이전에 선택한 항목을 검색 하 고 현재 선택한 항목을 검색 합니다.

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> 이 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 이벤트는 속성을 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) 변경한 결과로 발생 하는 변경 내용으로 인해 발생 될 수 있습니다.

다음 스크린샷은에서 여러 항목을 [`CollectionView`](xref:Xamarin.Forms.CollectionView)선택 하 여 보여 줍니다.

[![IOS 및 Android에서 여러 항목이 선택 된 CollectionView 세로 목록 스크린샷](selection-images/multiple-selection.png "여러 선택 영역을 포함 하는 CollectionView 세로 목록")](selection-images/multiple-selection-large.png#lightbox "여러 선택 영역을 포함 하는 CollectionView 세로 목록")

## <a name="single-pre-selection"></a>단일 사전 선택

속성이로 `Single`설정 된 경우 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) 속성을 항목으로 설정 하 여 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 의 단일 항목을 미리 선택할 수 있습니다. [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) 다음 XAML 예제에서는 단일 항목 `CollectionView` 을 미리 선택 하는을 보여 줍니다.

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
> 속성의 기본 바인딩 `TwoWay`모드는입니다. [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)

합니다 [ `SelectedItem` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) 속성 데이터를 바인딩하는 `SelectedMonkey` 형식인 연결 된 뷰 모델의 속성 `Monkey`합니다. 기본적 `TwoWay` 으로 바인딩이 사용 되므로 사용자가 선택한 항목을 변경 하면 `SelectedMonkey` 속성의 값이 선택한 `Monkey` 개체로 설정 됩니다. 속성은 `MonkeysViewModel` 클래스에 정의 되 고,는 `Monkeys` 컬렉션의 네 번째 항목으로 설정 됩니다. `SelectedMonkey`

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

따라서가 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 표시 되 면 목록의 네 번째 항목이 미리 선택 됩니다.

[![IOS 및 Android에서 단일 사전 선택이 포함 된 CollectionView 세로 목록 스크린샷](selection-images/single-pre-selection.png "단일 사전 선택 영역을 포함 하는 CollectionView 세로 목록")](selection-images/single-pre-selection-large.png#lightbox "단일 사전 선택 영역을 포함 하는 CollectionView 세로 목록")

## <a name="multiple-pre-selection"></a>다중 미리 선택

속성이로 `Multiple`설정 되 면의 여러 항목 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 을 미리 선택할 수 있습니다. [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) 다음 XAML 예제에서는 여러 항목 `CollectionView` 을 미리 선택할 수 있도록 하는을 보여 줍니다.

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
> 속성의 기본 바인딩 `OneWay`모드는입니다. [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)

합니다 [ `SelectedItems` ](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) 속성 데이터를 바인딩하는 `SelectedMonkeys` 형식인 연결 된 뷰 모델의 속성 `ObservableCollection<object>`합니다. 속성은 `MonkeysViewModel` 클래스에 정의 되 고,는 `Monkeys` 컬렉션의 두 번째, 네 번째 및 다섯 번째 항목으로 설정 됩니다. `SelectedMonkeys`

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

## <a name="clearing-selections"></a>선택 영역 지우기

`null`및 [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) [속성은로설정하거나바인딩할개체를로설정하여지울수있습니다.`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)

## <a name="change-selected-item-color"></a>선택한 항목 색 변경

[`CollectionView`](xref:Xamarin.Forms.CollectionView)에는 에서`CollectionView`선택한 항목에 대 한 시각적 변경을 시작 하는 데 사용할 수 [있는가있습니다.`VisualState`](xref:Xamarin.Forms.VisualState) `Selected` 이 `VisualState` 에 대 한 일반적인 사용 사례는 다음 XAML 예제에 표시 된 대로 선택한 항목의 배경색을 변경 하는 것입니다.

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
> 을 [`Style`](xref:Xamarin.Forms.Style) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)포함 `Selected` 하는에`ItemTemplate` 는 속성 값으로 설정 되는의 루트 요소 형식인 [속성값이있어야합니다.`TargetType`](xref:Xamarin.Forms.Style.TargetType) `VisualState`

이 예제 [`Style.TargetType`](xref:Xamarin.Forms.Style.TargetType) 에서는의 `Grid` [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 루트 [요소가이므로속성값이로설정됩니다.`Grid`](xref:Xamarin.Forms.Grid) `Selected` [는의`CollectionView`](xref:Xamarin.Forms.CollectionView) 항목이 선택 될때`LightSkyBlue`항목 [의이로설정되도록지정합니다.`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) [`VisualState`](xref:Xamarin.Forms.VisualState)

[![IOS 및 Android에서 사용자 지정 단일 선택 색이 있는 CollectionView 세로 목록의 스크린샷](selection-images/single-selection-color.png "사용자 지정 단일 선택 색이 있는 CollectionView 세로 목록")](selection-images/single-selection-color-large.png#lightbox "사용자 지정 단일 선택 색이 있는 CollectionView 세로 목록")

시각적 상태에 대 한 자세한 내용은 [Xamarin. Forms 시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)를 참조 하세요.

## <a name="disable-selection"></a>선택 사용 안 함

[`CollectionView`](xref:Xamarin.Forms.CollectionView)선택은 기본적으로 사용 하지 않도록 설정 되어 있습니다. 그러나에서 선택 `CollectionView` 이 활성화 되어 있으면 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) 속성을로 `None`설정 하 여 사용 하지 않도록 설정할 수 있습니다.

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

[`CollectionView`](xref:Xamarin.Forms.CollectionView) [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) `null` [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) 속성이로 `None`설정 된 경우의 항목을 선택할 수 없으며,이 속성은 유지 되 고 이벤트는 발생 하지 않습니다. [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode)

> [!NOTE]
> 항목을 선택 [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) 하 고 속성을에서 `Single` 로 [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) `None`변경 하면 속성이로 `null` 설정 되 고 빈 `CurrentSelection` 속성을 사용 하 여 이벤트가 발생 합니다. .

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin.ios 시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)
