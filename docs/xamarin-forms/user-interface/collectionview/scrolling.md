---
title: 항목을 스크롤하여
description: 스크롤을 시작 하는 사용자 천공 기와 때 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 또한 CollectionView 프로그래밍 방식으로 스크롤하여 항목 두 ScrollTo 메서드를 정의 합니다.
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/19/2019
ms.openlocfilehash: da7f379076b8e193deddc99e9004f051ba006cbb
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58330042"
---
# <a name="scroll-an-item-into-view"></a>항목을 스크롤하여

![미리 보기](~/media/shared/preview.png)

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView` 는 현재 미리 보기, 및 일부 계획된 기능 부족 합니다. 또한 API 구현을 완료 되 면 변경할 수 있습니다.

`CollectionView` 두 정의 `ScrollTo` 메서드를 스크롤하여 항목입니다. 오버 로드 중 하나는 동안 지정된 된 항목을 뷰로 스크롤합니다 다른 보기에 지정된 된 인덱스에 항목을 스크롤합니다. 두 오버 로드는 스크롤 완료 된 후 항목의 정확한 위치를 나타내기 위해 지정 될 수 있는 추가 인수 및 애니메이션 스크롤 효과를 줄 지 여부를 갖습니다.

`CollectionView` 정의 `ScrollToRequested` 될 때 발생 하는 이벤트 중 하나는 `ScrollTo` 메서드가 호출 됩니다. `ScrollToRequestedEventArgs` 와 함께 제공 되는 개체를 `ScrollToRequested` 이벤트에 여러 속성을 포함 하 여 `IsAnimated`, `Index`를 `Item`, 및 `ScrollToPosition`합니다. 이러한 속성에 지정 된 인수에서 설정 된 `ScrollTo` 메서드 호출 합니다.

스크롤을 시작 하는 사용자 천공 기와 때 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 이 기능은 중지를 스크롤할 때 위치로 항목 맞춤 때문에 맞추기로 알려져 있습니다. 자세한 내용은 [맞춤 지점을](#snap-points)합니다.

## <a name="scroll-an-item-at-an-index-into-view"></a>인덱스에 있는 항목을 스크롤하여

첫 번째 `ScrollTo` 메서드 오버 로드는 지정된 된 인덱스에 항목을 뷰로 스크롤합니다. 지정 된을 `CollectionView` 라는 개체 `collectionView`, 다음 예제에서는 뷰로 12 인덱스의 항목 스크롤 하는 방법을 보여 줍니다.

```csharp
collectionView.ScrollTo(12);
```

## <a name="scroll-an-item-into-view"></a>항목을 스크롤하여

두 번째 `ScrollTo` 메서드 오버 로드는 지정된 된 항목을 뷰로 스크롤합니다. 지정 된을 `CollectionView` 라는 개체 `collectionView`, 다음 예제에서는 지정된 된 항목 보기로 스크롤할 하는 방법을 보여 줍니다.

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

## <a name="control-scroll-position"></a>컨트롤의 스크롤 위치

뷰를 항목 스크롤할 때 사용 하 여 스크롤 완료 된 후 항목의 정확한 위치를 지정할 수 있습니다 합니다 `position` 의 인수는 `ScrollTo` 메서드. 이 인수를 허용 된 [ `ScrollToPosition` ](xref:Xamarin.Forms.ScrollToPosition) 열거형 멤버입니다.

### <a name="makevisible"></a>MakeVisible

합니다 [ `ScrollToPosition.MakeVisible` ](xref:Xamarin.Forms.ScrollToPosition) 멤버 보기에 표시 될 때까지 항목을 스크롤할 수를 나타냅니다.

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

이 예제에서는 코드 항목 뷰로 스크롤 하는 데 필요한 최소 스크롤 발생 합니다.

[![IOS 및 Android에서 뷰로 스크롤할 항목과 CollectionView 세로 목록 스크린샷](scrolling-images/scrolltoposition-makevisible.png "CollectionView 세로 스크롤된 항목 목록")](scrolling-images/scrolltoposition-makevisible-large.png#lightbox "CollectionView 세로 목록 항목 스크롤")

> [!NOTE]
> [ `ScrollToPosition.MakeVisible` ](xref:Xamarin.Forms.ScrollToPosition) 멤버 기본적으로 사용 하는 경우는 `position` 호출할 때 인수를 지정 하지 않으면는 `ScrollTo` 메서드.

### <a name="start"></a>시작

합니다 [ `ScrollToPosition.Start` ](xref:Xamarin.Forms.ScrollToPosition) 항목 보기의 시작 부분으로 스크롤하여는 멤버를 나타냅니다.

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

이 예제에서는 코드 보기의 시작 부분으로 스크롤되는 항목에 발생 합니다.

[![IOS 및 Android에서 뷰로 스크롤할 항목과 CollectionView 세로 목록 스크린샷](scrolling-images/scrolltoposition-start.png "CollectionView 세로 스크롤된 항목 목록")](scrolling-images/scrolltoposition-start-large.png#lightbox "CollectionView 세로 목록 항목 스크롤")

### <a name="center"></a>가운데 맞춤

합니다 [ `ScrollToPosition.Center` ](xref:Xamarin.Forms.ScrollToPosition) 뷰의 가운데에 항목을 스크롤하여는 멤버를 나타냅니다.

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

이 예제에서는 코드 뷰의 가운데로 스크롤 중인 항목에 발생 합니다.

[![IOS 및 Android에서 뷰로 스크롤할 항목과 CollectionView 세로 목록 스크린샷](scrolling-images/scrolltoposition-center.png "CollectionView 세로 스크롤된 항목 목록")](scrolling-images/scrolltoposition-center-large.png#lightbox "CollectionView 세로 목록 항목 스크롤")

### <a name="end"></a>끝

합니다 [ `ScrollToPosition.End` ](xref:Xamarin.Forms.ScrollToPosition) 보기의 끝에 항목을 스크롤하여는 멤버를 나타냅니다.

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

이 예제에서는 코드 보기의 끝에 스크롤되는 항목에 발생 합니다.

[![IOS 및 Android에서 뷰로 스크롤할 항목과 CollectionView 세로 목록 스크린샷](scrolling-images/scrolltoposition-end.png "CollectionView 세로 스크롤된 항목 목록")](scrolling-images/scrolltoposition-end-large.png#lightbox "CollectionView 세로 목록 항목 스크롤")

## <a name="disable-scroll-animation"></a>스크롤 애니메이션을 사용 하지 않도록 설정

스크롤 애니메이션을 뷰로 항목 스크롤할 때 표시 됩니다. 하지만이 애니메이션을 설정 하 여 비활성화할 수 있습니다 합니다 `animate` 인수를 `ScrollTo` 메서드를 `false`:

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="snap-points"></a>맞춤 지점

스크롤을 시작 하는 사용자 천공 기와 때 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 이 기능 이라고 맞추기 중지 되 고 다음 속성을 통해 제어 됩니다 스크롤 하는 경우 위치에 항목 맞춤 때문에 `ItemsLayout` 클래스:

- `SnapPointsType`형식의 `SnapPointsType`를 스크롤할 때 맞춤 지점이의 동작을 지정 합니다.
- `SnapPointsAlignment`형식의 `SnapPointsAlignment`, 끌기 지점 항목을 사용 하 여 정렬 되는 방법을 지정 합니다.

이러한 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 개체 속성을 데이터 바인딩의 대상 수 있음을 의미 합니다.

> [!NOTE]
> 맞추기 발생 하면 해당 발생 방향의 최소한의 동작을 생성 하는 합니다.

### <a name="snap-points-type"></a>맞춤 지점 유형

`SnapPointsType` 열거형은 다음 멤버를 정의 합니다.

- `None` 스크롤 스냅 되지 않습니다 항목을 나타냅니다.
- `Mandatory` 해당 콘텐츠를 나타내는 가장 가까운 맞춤에 대 한 스냅숏 항상 있는 스크롤은 자연스럽 게 중지 관성의 방향을 가리킵니다.
- `MandatorySingle` 와 같은 동작을 나타내는 `Mandatory`, 하지만 한 번에 하나의 항목을 스크롤합니다.

기본적으로 `SnapPointsType` 속성이 `SnapPointsType.None`, 해당 스크롤 하면 스냅 되지 않습니다 항목 다음 스크린샷과에서 같이:

[![IOS 및 Android에서 맞춤 지점이 없이 CollectionView 세로 목록 스크린샷](scrolling-images/snappoints-none.png "끌기 지점 없이 CollectionView 세로 목록")](scrolling-images/snappoints-none-large.png#lightbox "스냅인 없이 CollectionView 세로 목록 지점")

### <a name="snap-points-alignment"></a>맞춤 지점 맞춤

합니다 `SnapPointsAlignment` 열거형 정의 `Start`를 `Center`, 및 `End` 멤버입니다.

> [!IMPORTANT]
> 값을 `SnapPointsAlignment` 속성은만 적용 될 때를 `SnapPointsType` 속성이로 설정 되어 `Mandatory`, 또는 `MandatorySingle`.

#### <a name="start"></a>시작

`SnapPointsAlignment.Start` 끌기 지점 항목의 선행 가장자리에 부합 되는 멤버를 나타냅니다.

기본적으로 `SnapPointsAlignment` 속성은 `SnapPointsAlignment.Start`로 설정됩니다. 그러나 완성도 다음 XAML 예제에서는이 열거형 멤버를 설정 하는 방법을 보여 줍니다.

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

때 스크롤을 시작 하는 사용자 천공 기와 맨 위 항목 보기의 맨 위에 정렬 됩니다.

[![IOS 및 Android에서 시작 스냅인 지점과 CollectionView 세로 목록 스크린샷](scrolling-images/snappoints-start.png "시작점 스냅인을 사용 하 여 세로 목록 CollectionView")](scrolling-images/snappoints-start-large.png#lightbox "시작 CollectionView 세로 목록 맞춤 지점")

#### <a name="center"></a>가운데 맞춤

`SnapPointsAlignment.Center` 끌기 지점 항목의 가운데에 부합 되는 멤버를 나타냅니다. 다음 XAML 예제에서는이 열거형 멤버를 설정 하는 방법을 보여 줍니다.

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

스크롤을 시작 하는 사용자 천공 기와 때 맨 위 항목 센터 보기의 맨 위에 있는 정렬 됩니다.

[![중심점을 사용한 스냅인, iOS 및 Android에서 CollectionView 세로 목록 스크린샷](scrolling-images/snappoints-center.png "중심점 스냅인을 사용 하 여 세로 목록 CollectionView")](scrolling-images/snappoints-center-large.png#lightbox "CollectionView 세로 목록 가운데 맞춤 지점")

#### <a name="end"></a>끝

`SnapPointsAlignment.End` 멤버 항목의 후행 가장자리에 맞춤 지점이 맞추도록 나타냅니다. 다음 XAML 예제에서는이 열거형 멤버를 설정 하는 방법을 보여 줍니다.

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

때 스크롤을 시작 하는 사용자 천공 기와 아래 항목 뷰의 아래쪽에 정렬 됩니다.

[![IOS 및 Android에서 최종 맞춤 지점과 CollectionView 세로 목록 스크린샷](scrolling-images/snappoints-end.png "종단점이 스냅인을 사용 하 여 세로 목록 CollectionView")](scrolling-images/snappoints-end-large.png#lightbox "최종 스냅인을 사용 하 여 CollectionView 세로 목록 지점")

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
