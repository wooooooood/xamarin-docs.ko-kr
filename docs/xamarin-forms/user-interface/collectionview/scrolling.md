---
title: Xamarin.ios CollectionView 스크롤
description: 사용자가 스크롤을 시작 swipes 면 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 또한 CollectionView는 프로그래밍 방식으로 항목을 뷰로 스크롤 하는 두 개의 ScrollTo 메서드를 정의 합니다.
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 89bbe402f056b875a7dadd96527364847ad470e8
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68738929"
---
# <a name="xamarinforms-collectionview-scrolling"></a>Xamarin.ios CollectionView 스크롤

![](~/media/shared/preview.png "이 API는 현재 시험판")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)항목을 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 뷰로 스크롤 하는 두 가지 메서드를 정의 합니다. 오버 로드 중 하나는 지정 된 인덱스에 있는 항목을 뷰로 스크롤하고 다른 하나는 지정 된 항목을 뷰로 스크롤합니다. 두 오버 로드 모두 스크롤이 완료 된 후 항목의 정확한 위치를 나타내기 위해 지정할 수 있는 추가 인수와 스크롤에 애니메이션 효과를 적용할지 여부를 지정 합니다.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드 중 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) 하나가 호출 될 때 발생 하는 이벤트를 정의 합니다. `IsAnimated` `Index` `Item` `ScrollToPosition`이벤트와 함께 제공 되는 [개체에는,,,등의많은속성이있습니다.`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs) `ScrollToRequested` 이러한 속성은 `ScrollTo` 메서드 호출에 지정 된 인수에서 설정 됩니다.

사용자가 스크롤을 시작 swipes 면 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 이 기능은 스크롤이 중지 될 때 항목이 위치에 스냅 되기 때문에 맞추기 라고 합니다. 자세한 내용은 [맞추기 요소](#snap-points)를 참조 하세요.

## <a name="scroll-an-item-at-an-index-into-view"></a>인덱스에 있는 항목을 뷰로 스크롤합니다.

첫 번째 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드 오버 로드는 지정 된 인덱스에 있는 항목을 뷰로 스크롤합니다. [`CollectionView`](xref:Xamarin.Forms.CollectionView) 라는`collectionView`개체가 지정 된 경우 다음 예제에서는 인덱스 12의 항목을 뷰로 스크롤 하는 방법을 보여 줍니다.

```csharp
collectionView.ScrollTo(12);
```

## <a name="scroll-an-item-into-view"></a>항목을 뷰로 스크롤

두 번째 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드 오버 로드는 지정 된 항목을 뷰로 스크롤합니다. [`CollectionView`](xref:Xamarin.Forms.CollectionView) 라는`collectionView`개체가 지정 된 경우 다음 예제에서는 지정 된 항목을 뷰로 스크롤 하는 방법을 보여 줍니다.

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

## <a name="control-scroll-position"></a>컨트롤 스크롤 위치

항목을 뷰로 스크롤할 때 스크롤 후의 정확한 항목 위치는 `position` [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드의 인수를 사용 하 여 지정할 수 있습니다. 이 인수는 열거형 [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) 멤버를 허용 합니다.

### <a name="makevisible"></a>MakeVisible

멤버 [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) 는 보기에 표시 될 때까지 항목을 스크롤 함을 나타냅니다.

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

이 예제 코드는 항목을 뷰로 스크롤 하는 데 필요한 최소한의 스크롤이 발생 합니다.

항목이 [ ![표시 된 CollectionView 세로 목록의 스크린샷, IOS 및 Android](scrolling-images/scrolltoposition-makevisible.png "CollectionView 세로 목록에서 항목 스크롤") ] (scrolling-images/scrolltoposition-makevisible-large.png#lightbox "항목이 스크롤 된 CollectionView 세로 목록")

> [!NOTE]
> 메서드를 호출할 때 인수를 `position` 지정 [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) 하지 않으면 기본적으로 멤버가 `ScrollTo` 사용 됩니다.

### <a name="start"></a>시작

멤버 [`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition) 는 항목을 뷰의 시작 부분으로 스크롤 해야 함을 나타냅니다.

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

이 예제 코드를 실행 하면 항목이 뷰의 시작 부분으로 스크롤됩니다.

항목이 [ ![표시 된 CollectionView 세로 목록의 스크린샷, IOS 및 Android](scrolling-images/scrolltoposition-start.png "CollectionView 세로 목록에서 항목 스크롤") ] (scrolling-images/scrolltoposition-start-large.png#lightbox "항목이 스크롤 된 CollectionView 세로 목록")

### <a name="center"></a>가운데 맞춤

멤버 [`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition) 는 항목을 뷰의 가운데로 스크롤 해야 함을 나타냅니다.

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

이 예제 코드는 항목을 뷰의 가운데로 스크롤합니다.

항목이 [ ![표시 된 CollectionView 세로 목록의 스크린샷, IOS 및 Android](scrolling-images/scrolltoposition-center.png "CollectionView 세로 목록에서 항목 스크롤") ] (scrolling-images/scrolltoposition-center-large.png#lightbox "항목이 스크롤 된 CollectionView 세로 목록")

### <a name="end"></a>종료

멤버 [`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition) 는 항목을 뷰의 끝으로 스크롤 해야 함을 나타냅니다.

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

이 예제 코드는 항목을 뷰의 끝으로 스크롤합니다.

항목이 [ ![표시 된 CollectionView 세로 목록의 스크린샷, IOS 및 Android](scrolling-images/scrolltoposition-end.png "CollectionView 세로 목록에서 항목 스크롤") ] (scrolling-images/scrolltoposition-end-large.png#lightbox "항목이 스크롤 된 CollectionView 세로 목록")

## <a name="disable-scroll-animation"></a>스크롤 애니메이션 사용 안 함

항목을 뷰로 스크롤하면 스크롤 애니메이션이 표시 됩니다. 그러나 `animate` `ScrollTo` 메서드의 인수를로 `false`설정 하 여이 애니메이션을 사용 하지 않도록 설정할 수 있습니다.

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="snap-points"></a>맞춤 요소

사용자가 스크롤을 시작 swipes 면 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 이 기능은 스크롤이 중지 될 때 항목이 위치에 스냅 되 고 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) 클래스의 다음 속성에 의해 제어 되기 때문에 맞추기 라고 합니다.

- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)형식의 [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)는 스크롤할 때 중심점의 동작을 지정 합니다.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)형식의 [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)는 맞춤 지점이 항목에 정렬 되는 방법을 지정 합니다.

이러한 속성은 개체에 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 의해 지원 됩니다. 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

> [!NOTE]
> 맞추기를 수행 하는 경우 최소한의 동작을 생성 하는 방향으로 수행 됩니다.

### <a name="snap-points-type"></a>맞춤 지점의 유형

열거형 [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) 은 다음 멤버를 정의 합니다.

- `None`스크롤이 항목에 맞추지 않음을 나타냅니다.
- `Mandatory`는 항상 내용이 관성의 방향에 따라 스크롤할 수 있는 가장 가까운 맞춤 지점에 맞춰집니다.
- `MandatorySingle`와 `Mandatory`동일한 동작을 나타내지만 한 번에 한 항목만 스크롤합니다.

기본적 [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) 으로 속성은 다음 스크린샷에 표시 된 `SnapPointsType.None`것 처럼 스크롤이 항목을 맞추지 않도록 하는로 설정 됩니다.

[ ![CollectionView 세로 목록 (snap points 불포함), IOS 및 Android](scrolling-images/snappoints-none.png "CollectionView 세로 목록 (snap Points 불포함") )의 스크린샷] (scrolling-images/snappoints-none-large.png#lightbox "CollectionView 세로 목록 (맞춤 지점이 없음") )

### <a name="snap-points-alignment"></a>맞춤 지점의 맞춤

열거형 [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) 은, `Start`및 `Center`멤버를정의 합니다. `End`

> [!IMPORTANT]
> 속성의 [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) 값은 [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) 속성이 `Mandatory` 또는`MandatorySingle`로 설정 된 경우에만 적용 됩니다.

#### <a name="start"></a>시작

멤버 `SnapPointsAlignment.Start` 는 맞춤 지점이 항목의 선행 가장자리에 맞춰지도록 지정 합니다.

기본적 [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) 으로 속성은로 `SnapPointsAlignment.Start`설정 됩니다. 그러나 완전성을 위해 다음 XAML 예제에서는이 열거형 멤버를 설정 하는 방법을 보여 줍니다.

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="Start">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

사용자가 스크롤을 시작 하는 데 swipes 위쪽 항목은 뷰의 위쪽에 정렬 됩니다.

시작 snap 지점이 [있는 ![CollectionView 세로 목록의 스크린샷, IOS 및 Android](scrolling-images/snappoints-start.png "CollectionView 세로 목록에 시작 맞춤 지점이") ] 있습니다. (scrolling-images/snappoints-start-large.png#lightbox "시작 스냅 지점이 있는 CollectionView 세로 목록")

#### <a name="center"></a>가운데 맞춤

멤버 `SnapPointsAlignment.Center` 는 맞춤 지점이 항목의 가운데에 정렬 됨을 나타냅니다. 다음 XAML 예제에서는이 열거형 멤버를 설정 하는 방법을 보여 줍니다.

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="Center">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

사용자가 스크롤을 시작 하는 데 swipes 위쪽 항목은 보기의 위쪽에 정렬 됩니다.

중앙 맞춤 지점이 있는 [ ![CollectionView 세로 목록의 스크린샷, IOS 및 Android](scrolling-images/snappoints-center.png "CollectionView 세로 목록 (가운데 맞춤 지점의") 경우] ) (scrolling-images/snappoints-center-large.png#lightbox "가운데 맞춤 지점이 있는 CollectionView 세로 목록")

#### <a name="end"></a>종료

멤버 `SnapPointsAlignment.End` 는 맞춤 지점이 항목의 끝 가장자리에 맞춰지도록 지정 합니다. 다음 XAML 예제에서는이 열거형 멤버를 설정 하는 방법을 보여 줍니다.

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="End">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

사용자가 스크롤 시작을 swipes 하는 경우 아래쪽 항목이 보기의 아래쪽에 정렬 됩니다.

[최종 맞춤 지점이 있는 ![CollectionView 세로 목록의 스크린샷, IOS 및 Android](scrolling-images/snappoints-end.png "CollectionView 세로 목록") ] (scrolling-images/snappoints-end-large.png#lightbox "CollectionView 세로 목록에 끝 맞추기 지점이") 있습니다.

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
