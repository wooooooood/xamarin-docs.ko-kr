---
제목: " Xamarin.Forms CollectionView 스크롤" 설명: "사용자가 스크롤을 시작 하려면 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 또한 CollectionView는 프로그래밍 방식으로 항목을 뷰로 스크롤 하는 두 개의 ScrollTo 메서드를 정의 합니다. "
assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5: xamarin-forms author: davidbritch: dabritch:: 09/17/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-collectionview-scrolling"></a>Xamarin.FormsCollectionView 스크롤

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)항목을 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 뷰로 스크롤 하는 두 가지 메서드를 정의 합니다. 오버 로드 중 하나는 지정 된 인덱스에 있는 항목을 뷰로 스크롤하고 다른 하나는 지정 된 항목을 뷰로 스크롤합니다. 두 오버 로드 모두에 항목이 속한 그룹을 나타내는 추가 인수, 스크롤이 완료 된 후 항목의 정확한 위치 및 스크롤에 애니메이션 효과를 적용할지 여부를 지정 하는 추가 인수가 있습니다.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)메서드 중 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) 하나가 호출 될 때 발생 하는 이벤트를 정의 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 합니다. 이벤트와 함께 제공 되는 개체에는,,, 등의 [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs) `ScrollToRequested` 많은 속성이 있습니다 `IsAnimated` `Index` `Item` `ScrollToPosition` . 이러한 속성은 메서드 호출에 지정 된 인수에서 설정 됩니다 `ScrollTo` .

또한은 [`CollectionView`](xref:Xamarin.Forms.CollectionView) `Scrolled` 스크롤이 발생 했음을 나타내기 위해 발생 하는 이벤트를 정의 합니다. `ItemsViewScrolledEventArgs`이벤트와 함께 제공 되는 개체에는 `Scrolled` 많은 속성이 있습니다. 자세한 내용은 [스크롤 검색](#detect-scrolling)을 참조 하세요.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)또한에 `ItemsUpdatingScrollMode` `CollectionView` 새 항목이 추가 될 때의 스크롤 동작을 나타내는 속성도 정의 합니다. 이 속성에 대 한 자세한 내용은 [새 항목이 추가 될 때 스크롤 위치 제어](#control-scroll-position-when-new-items-are-added)를 참조 하세요.

사용자가 스크롤을 시작 swipes 면 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 이 기능은 스크롤이 중지 될 때 항목이 위치에 스냅 되기 때문에 맞추기 라고 합니다. 자세한 내용은 [맞추기 요소](#snap-points)를 참조 하세요.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)사용자가 스크롤하면 데이터를 증분 로드할 수도 있습니다. 자세한 내용은 [데이터를 증분 로드](populate-data.md#load-data-incrementally)를 참조 하세요.

## <a name="detect-scrolling"></a>스크롤 검색

[`CollectionView`](xref:Xamarin.Forms.CollectionView)`Scrolled`스크롤이 발생 했음을 나타내기 위해 발생 하는 이벤트를 정의 합니다. 다음 XAML 예제에서는 `CollectionView` 이벤트에 대 한 이벤트 처리기를 설정 하는을 보여 줍니다 `Scrolled` .

```xaml
<CollectionView Scrolled="OnCollectionViewScrolled">
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView();
collectionView.Scrolled += OnCollectionViewScrolled;
```

이 코드 예제에서 이벤트 `OnCollectionViewScrolled` 처리기는 이벤트가 발생할 때 실행 됩니다 `Scrolled` .

```csharp
void OnCollectionViewScrolled(object sender, ItemsViewScrolledEventArgs e)
{
    Debug.WriteLine("HorizontalDelta: " + e.HorizontalDelta);
    Debug.WriteLine("VerticalDelta: " + e.VerticalDelta);
    Debug.WriteLine("HorizontalOffset: " + e.HorizontalOffset);
    Debug.WriteLine("VerticalOffset: " + e.VerticalOffset);
    Debug.WriteLine("FirstVisibleItemIndex: " + e.FirstVisibleItemIndex);
    Debug.WriteLine("CenterItemIndex: " + e.CenterItemIndex);
    Debug.WriteLine("LastVisibleItemIndex: " + e.LastVisibleItemIndex);
}
```

이 예제에서 `OnCollectionViewScrolled` 이벤트 처리기는 `ItemsViewScrolledEventArgs` 이벤트와 함께 제공 되는 개체의 값을 출력 합니다.

> [!IMPORTANT]
> `Scrolled`이벤트는 사용자가 시작한 스크롤에 대해 발생 하 고 프로그래밍 방식으로 스크롤됩니다.

## <a name="scroll-an-item-at-an-index-into-view"></a>인덱스에 있는 항목을 뷰로 스크롤합니다.

첫 번째 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드 오버 로드는 지정 된 인덱스에 있는 항목을 뷰로 스크롤합니다. [`CollectionView`](xref:Xamarin.Forms.CollectionView)라는 개체가 지정 된 `collectionView` 경우 다음 예제에서는 인덱스 12의 항목을 뷰로 스크롤 하는 방법을 보여 줍니다.

```csharp
collectionView.ScrollTo(12);
```

또는 항목 및 그룹 인덱스를 지정 하 여 그룹화 된 데이터의 항목을 뷰로 스크롤할 수 있습니다. 다음 예에서는 두 번째 그룹의 세 번째 항목을 뷰로 스크롤 하는 방법을 보여 줍니다.

```csharp
// Items and groups are indexed from zero.
collectionView.ScrollTo(2, 1);
```

> [!NOTE]
> 이 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) 이벤트는 메서드를 호출할 때 발생 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 합니다.

## <a name="scroll-an-item-into-view"></a>항목을 뷰로 스크롤

두 번째 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드 오버 로드는 지정 된 항목을 뷰로 스크롤합니다. [`CollectionView`](xref:Xamarin.Forms.CollectionView)라는 개체가 지정 된 `collectionView` 경우 다음 예제에서는 Proboscis 원숭이 항목을 뷰로 스크롤 하는 방법을 보여 줍니다.

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

또는 항목과 그룹을 지정 하 여 그룹화 된 데이터의 항목을 뷰로 스크롤할 수 있습니다. 다음 예제에서는 Monkeys 그룹의 Proboscis 원숭이 항목을 뷰로 스크롤 하는 방법을 보여 줍니다.

```csharp
GroupedAnimalsViewModel viewModel = BindingContext as GroupedAnimalsViewModel;
AnimalGroup group = viewModel.Animals.FirstOrDefault(a => a.Name == "Monkeys");
Animal monkey = group.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey, group);
```

> [!NOTE]
> 이 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) 이벤트는 메서드를 호출할 때 발생 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 합니다.

## <a name="disable-scroll-animation"></a>스크롤 애니메이션 사용 안 함

항목을 뷰로 스크롤하면 스크롤 애니메이션이 표시 됩니다. 그러나 메서드의 인수를로 설정 하 여이 애니메이션을 사용 하지 않도록 설정할 수 있습니다 `animate` `ScrollTo` `false` .

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position"></a>컨트롤 스크롤 위치

항목을 뷰로 스크롤할 때 스크롤 후의 정확한 항목 위치는 메서드의 인수를 사용 하 여 지정할 수 있습니다 `position` [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) . 이 인수는 [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) 열거형 멤버를 허용 합니다.

### <a name="makevisible"></a>MakeVisible

[`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition)멤버는 보기에 표시 될 때까지 항목을 스크롤 함을 나타냅니다.

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

이 예제 코드는 항목을 뷰로 스크롤 하는 데 필요한 최소한의 스크롤이 발생 합니다.

[![IOS 및 Android에서 항목이 뷰로 스크롤 된 CollectionView 세로 목록 스크린샷](scrolling-images/scrolltoposition-makevisible.png "항목이 스크롤 된 CollectionView 세로 목록")](scrolling-images/scrolltoposition-makevisible-large.png#lightbox "항목이 스크롤 된 CollectionView 세로 목록")

> [!NOTE]
> [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) `position` 메서드를 호출할 때 인수를 지정 하지 않으면 기본적으로 멤버가 사용 됩니다 `ScrollTo` .

### <a name="start"></a>시작

[`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition)멤버는 항목을 뷰의 시작 부분으로 스크롤 해야 함을 나타냅니다.

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

이 예제 코드를 실행 하면 항목이 뷰의 시작 부분으로 스크롤됩니다.

[![IOS 및 Android에서 항목이 뷰로 스크롤 된 CollectionView 세로 목록 스크린샷](scrolling-images/scrolltoposition-start.png "항목이 스크롤 된 CollectionView 세로 목록")](scrolling-images/scrolltoposition-start-large.png#lightbox "항목이 스크롤 된 CollectionView 세로 목록")

### <a name="center"></a>중심

[`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition)멤버는 항목을 뷰의 가운데로 스크롤 해야 함을 나타냅니다.

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

이 예제 코드는 항목을 뷰의 가운데로 스크롤합니다.

[![IOS 및 Android에서 항목이 뷰로 스크롤 된 CollectionView 세로 목록 스크린샷](scrolling-images/scrolltoposition-center.png "항목이 스크롤 된 CollectionView 세로 목록")](scrolling-images/scrolltoposition-center-large.png#lightbox "항목이 스크롤 된 CollectionView 세로 목록")

### <a name="end"></a>끝

[`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition)멤버는 항목을 뷰의 끝으로 스크롤 해야 함을 나타냅니다.

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

이 예제 코드는 항목을 뷰의 끝으로 스크롤합니다.

[![IOS 및 Android에서 항목이 뷰로 스크롤 된 CollectionView 세로 목록 스크린샷](scrolling-images/scrolltoposition-end.png "항목이 스크롤 된 CollectionView 세로 목록")](scrolling-images/scrolltoposition-end-large.png#lightbox "항목이 스크롤 된 CollectionView 세로 목록")

## <a name="control-scroll-position-when-new-items-are-added"></a>새 항목이 추가 되는 경우 스크롤 위치 제어

[`CollectionView`](xref:Xamarin.Forms.CollectionView)바인딩 가능한 `ItemsUpdatingScrollMode` 속성에 의해 지원 되는 속성을 정의 합니다. 이 속성은 `ItemsUpdatingScrollMode` `CollectionView` 새 항목이 추가 될 때의 스크롤 동작을 나타내는 열거형 값을 가져오거나 설정 합니다. `ItemsUpdatingScrollMode` 열거형은 다음 멤버를 정의합니다.

- `KeepItemsInView`새 항목이 추가 될 때 표시 되는 첫 번째 항목을 유지 하도록 스크롤 오프셋을 조정 합니다.
- `KeepScrollOffset`새 항목이 추가 될 때 목록의 시작 부분을 기준으로 스크롤 오프셋을 유지 합니다.
- `KeepLastItemInView`새 항목이 추가 될 때 마지막 항목이 표시 되도록 스크롤 오프셋을 조정 합니다.

기본값은 `ItemsUpdatingScrollMode` 속성은 `KeepItemsInView`합니다. 따라서 목록에서 처음 표시 되는 항목에 새 항목이 추가 되 면 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 목록에 표시 되는 항목은 계속 표시 됩니다. 새로 추가 된 항목이 목록의 맨 아래에 항상 표시 되도록 하려면 `ItemsUpdatingScrollMode` 속성을로 설정 해야 합니다 `KeepLastItemInView` .

```xaml
<CollectionView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## <a name="scroll-bar-visibility"></a>스크롤 막대 표시 여부

[`CollectionView`](xref:Xamarin.Forms.CollectionView)`HorizontalScrollBarVisibility`바인딩 가능한 `VerticalScrollBarVisibility` 속성에 의해 지원 되는 및 속성을 정의 합니다. 이러한 속성은 [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) 가로 또는 세로 스크롤 막대가 표시 되는 경우를 나타내는 열거형 값을 가져오거나 설정 합니다. `ScrollBarVisibility` 열거형은 다음 멤버를 정의합니다.

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)플랫폼의 기본 스크롤 막대 동작을 나타내며 및 속성의 기본값입니다 `HorizontalScrollBarVisibility` `VerticalScrollBarVisibility` .
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)뷰가 뷰에 맞는 경우에도 스크롤 막대가 표시 됨을 나타냅니다.
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)콘텐츠가 뷰에 맞지 않는 경우에도 스크롤 막대가 표시 되지 않음을 나타냅니다.

## <a name="snap-points"></a>끌기 지점

사용자가 스크롤을 시작 swipes 면 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 이 기능은 스크롤이 중지 될 때 항목이 위치에 스냅 되 고 클래스의 다음 속성에 의해 제어 되기 때문에 맞추기 라고 합니다 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) .

- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)형식의는 [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) 스크롤할 때 중심점의 동작을 지정 합니다.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)형식의는 [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) 맞춤 지점이 항목에 정렬 되는 방법을 지정 합니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

> [!NOTE]
> 맞추기를 수행 하는 경우 최소한의 동작을 생성 하는 방향으로 수행 됩니다.

### <a name="snap-points-type"></a>맞춤 지점의 유형

[`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)열거형은 다음 멤버를 정의 합니다.

- `None`스크롤이 항목에 맞추지 않음을 나타냅니다.
- `Mandatory`는 항상 내용이 관성의 방향에 따라 스크롤할 수 있는 가장 가까운 맞춤 지점에 맞춰집니다.
- `MandatorySingle`와 동일한 동작을 나타내지만 한 `Mandatory` 번에 한 항목만 스크롤합니다.

기본적으로 속성은 [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) `SnapPointsType.None` 다음 스크린샷에 표시 된 것 처럼 스크롤이 항목을 맞추지 않도록 하는로 설정 됩니다.

[![IOS 및 Android에서 스냅 지점이 없는 CollectionView 세로 목록의 스크린샷](scrolling-images/snappoints-none.png "CollectionView 세로 목록 (맞춤 지점이 없음)")](scrolling-images/snappoints-none-large.png#lightbox "CollectionView 세로 목록 (맞춤 지점이 없음)")

### <a name="snap-points-alignment"></a>맞춤 지점의 맞춤

[`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)열거형은 `Start` , 및 멤버를 정의 `Center` `End` 합니다.

> [!IMPORTANT]
> 속성의 값은 [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) 속성이 또는로 설정 된 경우에만 적용 됩니다 `Mandatory` `MandatorySingle` .

#### <a name="start"></a>시작

`SnapPointsAlignment.Start`멤버는 맞춤 지점이 항목의 선행 가장자리에 맞춰지도록 지정 합니다.

기본적으로 속성은 [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) 로 설정 됩니다 `SnapPointsAlignment.Start` . 그러나 완전성을 위해 다음 XAML 예제에서는이 열거형 멤버를 설정 하는 방법을 보여 줍니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Start" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    // ...
};
```

사용자가 스크롤을 시작 하는 데 swipes 위쪽 항목은 뷰의 위쪽에 정렬 됩니다.

[![IOS 및 Android의 시작 CollectionView 세로 목록 스크린샷](scrolling-images/snappoints-start.png "시작 스냅 지점이 있는 CollectionView 세로 목록")](scrolling-images/snappoints-start-large.png#lightbox "시작 스냅 지점이 있는 CollectionView 세로 목록")

#### <a name="center"></a>중심

`SnapPointsAlignment.Center`멤버는 맞춤 지점이 항목의 가운데에 정렬 됨을 나타냅니다. 다음 XAML 예제에서는이 열거형 멤버를 설정 하는 방법을 보여 줍니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Center" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    // ...
};
```

사용자가 스크롤을 시작 하는 데 swipes 위쪽 항목은 보기의 위쪽에 정렬 됩니다.

[![IOS 및 Android에서 가운데 맞춤 지점이 있는 CollectionView 세로 목록의 스크린샷](scrolling-images/snappoints-center.png "가운데 맞춤 지점이 있는 CollectionView 세로 목록")](scrolling-images/snappoints-center-large.png#lightbox "가운데 맞춤 지점이 있는 CollectionView 세로 목록")

#### <a name="end"></a>끝

`SnapPointsAlignment.End`멤버는 맞춤 지점이 항목의 끝 가장자리에 맞춰지도록 지정 합니다. 다음 XAML 예제에서는이 열거형 멤버를 설정 하는 방법을 보여 줍니다.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="End" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    // ...
};
```

사용자가 스크롤 시작을 swipes 하는 경우 아래쪽 항목이 보기의 아래쪽에 정렬 됩니다.

[![IOS 및 Android의 CollectionView 세로 목록에 끝 맞춤 지점이 있는 스크린샷](scrolling-images/snappoints-end.png "CollectionView 세로 목록에 끝 맞추기 지점이 있습니다.")](scrolling-images/snappoints-end-large.png#lightbox "CollectionView 세로 목록에 끝 맞추기 지점이 있습니다.")

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
