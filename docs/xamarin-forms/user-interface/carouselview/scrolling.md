---
title: Xamarin.ios CarouselView 스크롤
description: 사용자가 스크롤을 시작 swipes 면 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 또한 CarouselView는 프로그래밍 방식으로 항목을 뷰로 스크롤 하는 두 개의 ScrollTo 메서드를 정의 합니다.
ms.prod: xamarin
ms.assetid: 92D7B618-07FA-4343-9D0F-212525E92C39
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/28/2020
ms.openlocfilehash: 735a572f4aadfc224e545e371525b96f29c9552e
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305686"
---
# <a name="xamarinforms-carouselview-scrolling"></a>Xamarin.ios CarouselView 스크롤

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 다음과 같은 스크롤 관련 속성을 정의 합니다.

- 가로 스크롤 막대가 표시 되는 시점을 지정 하는 `ScrollBarVisibility`형식의 `HorizontalScrollBarVisibility`입니다.
- `IsDragging``CarouselView` 스크롤 여부를 나타내는 `bool`형식입니다. 이는 읽기 전용 속성 이며 기본값은 `false`입니다.
- `IsScrollAnimated``CarouselView`를 스크롤할 때 애니메이션이 발생 하는지 여부를 지정 하는 `bool`형식입니다. 기본값은 `true`입니다.
- 새 항목이 추가 될 때 `CarouselView`의 스크롤 동작을 나타내는 `ItemsUpdatingScrollMode`형식의 `ItemsUpdatingScrollMode`입니다.
- 세로 스크롤 막대가 표시 되는 시점을 지정 하는 `ScrollBarVisibility`형식의 `VerticalScrollBarVisibility`입니다.

이러한 속성은 모두 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, 데이터 바인딩의 대상이 될 수 있습니다.

또한 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 는 항목을 뷰로 스크롤 하는 두 개의 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드를 정의 합니다. 오버 로드 중 하나는 지정 된 인덱스에 있는 항목을 뷰로 스크롤하고 다른 하나는 지정 된 항목을 뷰로 스크롤합니다. 두 오버 로드 모두 스크롤이 완료 된 후 항목의 정확한 위치를 나타내기 위해 지정할 수 있는 추가 인수와 스크롤에 애니메이션 효과를 적용할지 여부를 지정 합니다.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드 중 하나가 호출 될 때 발생 하는 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) 이벤트를 정의 합니다. `ScrollToRequested` 이벤트와 함께 제공 되는 [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs) 개체에는 `IsAnimated`, `Index`, `Item`, `ScrollToPosition`등의 많은 속성이 있습니다. 이러한 속성은 `ScrollTo` 메서드 호출에 지정 된 인수에서 설정 됩니다.

또한 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 은 스크롤이 발생 했음을 나타내기 위해 발생 하는 `Scrolled` 이벤트를 정의 합니다. `Scrolled` 이벤트와 함께 제공 되는 `ItemsViewScrolledEventArgs` 개체에는 많은 속성이 있습니다. 자세한 내용은 [스크롤 검색](#detect-scrolling)을 참조 하세요.

사용자가 스크롤을 시작 swipes 면 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 이 기능은 스크롤이 중지 될 때 항목이 위치에 스냅 되기 때문에 맞추기 라고 합니다. 자세한 내용은 [맞추기 요소](#snap-points)를 참조 하세요.

사용자가 스크롤하면 데이터를 증분 로드할 수도 [`CarouselView`](xref:Xamarin.Forms.CarouselView) . 자세한 내용은 [데이터를 증분 로드](populate-data.md#load-data-incrementally)를 참조 하세요.

## <a name="detect-scrolling"></a>스크롤 검색

`IsDragging` 속성을 검사 하 여 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 현재 항목을 스크롤 하 고 있는지 여부를 확인할 수 있습니다.

또한 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 은 스크롤이 발생 했음을 나타내기 위해 발생 하는 `Scrolled` 이벤트를 정의 합니다. 이 이벤트는 스크롤에 대 한 데이터가 필요한 경우에 사용 해야 합니다.

다음 XAML 예제에서는 `Scrolled` 이벤트에 대 한 이벤트 처리기를 설정 하는 `CarouselView` 보여 줍니다.

```xaml
<CarouselView Scrolled="OnCollectionViewScrolled">
    ...
</CarouselView>
```

해당 하는 C# 코드가입니다.

```csharp
CarouselView carouselView = new CarouselView();
carouselView.Scrolled += OnCarouselViewScrolled;
```

이 코드 예제에서는 `Scrolled` 이벤트가 발생 하면 `OnCarouselViewScrolled` 이벤트 처리기가 실행 됩니다.

```csharp
void OnCarouselViewScrolled(object sender, ItemsViewScrolledEventArgs e)
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

이 예제에서 `OnCarouselViewScrolled` 이벤트 처리기는 이벤트와 함께 제공 되는 `ItemsViewScrolledEventArgs` 개체의 값을 출력 합니다.

> [!IMPORTANT]
> `Scrolled` 이벤트는 사용자가 시작한 스크롤에 대해 발생 하 고 프로그래밍 방식으로 스크롤됩니다.

## <a name="scroll-an-item-at-an-index-into-view"></a>인덱스에 있는 항목을 뷰로 스크롤합니다.

첫 번째 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드 오버 로드는 지정 된 인덱스에 있는 항목을 뷰로 스크롤합니다. `carouselView`라는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 개체가 지정 된 경우 다음 예제에서는 인덱스 6의 항목을 뷰로 스크롤 하는 방법을 보여 줍니다.

```csharp
carouselView.ScrollTo(6);
```

> [!NOTE]
> [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) 이벤트는 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드가 호출 될 때 발생 합니다.

## <a name="scroll-an-item-into-view"></a>항목을 뷰로 스크롤

두 번째 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드 오버 로드는 지정 된 항목을 뷰로 스크롤합니다. `carouselView`이라는 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 개체가 지정 된 다음 예제에서는 Proboscis 원숭이 항목을 뷰로 스크롤 하는 방법을 보여 줍니다.

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
carouselView.ScrollTo(monkey);
```

> [!NOTE]
> [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) 이벤트는 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드가 호출 될 때 발생 합니다.

## <a name="disable-scroll-animation"></a>스크롤 애니메이션 사용 안 함

[`CarouselView`](xref:Xamarin.Forms.CarouselView)의 항목 간을 이동할 때 스크롤 애니메이션이 표시 됩니다. 이 애니메이션은 사용자가 시작한 스크롤 및 프로그래밍 방식의 스크롤 모두에서 발생 합니다. `IsScrollAnimated` 속성을 `false`로 설정 하면 스크롤 범주에 대 한 애니메이션을 사용 하지 않도록 설정 됩니다.

또는 프로그래밍 방식으로 스크롤 하는 경우 스크롤 애니메이션을 사용 하지 않도록 설정 하려면 `ScrollTo` 메서드의 `animate` 인수를 `false`로 설정할 수 있습니다.

```csharp
carouselView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position"></a>컨트롤 스크롤 위치

항목을 뷰로 스크롤할 때 스크롤이 완료 된 후 항목의 정확한 위치는 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 메서드의 `position` 인수를 사용 하 여 지정할 수 있습니다. 이 인수는 [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) 열거형 멤버를 허용 합니다.

### <a name="makevisible"></a>MakeVisible

[`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) 멤버는 보기에 표시 될 때까지 항목을 스크롤 해야 함을 나타냅니다.

```csharp
carouselView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

이 예제 코드는 항목을 뷰로 스크롤 하는 데 필요한 최소 스크롤을 생성 합니다.

> [!NOTE]
> `ScrollTo` 메서드를 호출할 때 `position` 인수를 지정 하지 않으면 기본적으로 [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) 멤버가 사용 됩니다.

### <a name="start"></a>시작

[`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition) 멤버는 항목을 뷰의 시작 부분으로 스크롤 해야 함을 나타냅니다.

```csharp
carouselView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

이 예제 코드는 항목을 뷰의 시작 부분으로 스크롤합니다.

### <a name="center"></a>중심

[`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition) 멤버는 항목을 뷰의 가운데로 스크롤 해야 함을 나타냅니다.

```csharp
carouselViewView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

이 예제 코드는 항목을 뷰의 가운데로 스크롤합니다.

### <a name="end"></a>끝

[`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition) 멤버는 항목을 뷰의 끝으로 스크롤 해야 함을 나타냅니다.

```csharp
carouselViewView.ScrollTo(monkey, position: ScrollToPosition.End);
```

이 예제 코드는 항목을 뷰의 끝으로 스크롤합니다.

## <a name="control-scroll-position-when-new-items-are-added"></a>새 항목이 추가 되는 경우 스크롤 위치 제어

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 은 바인딩 가능한 속성에 의해 지원 되는 `ItemsUpdatingScrollMode` 속성을 정의 합니다. 이 속성은 새 항목이 추가 될 때 `CarouselView`의 스크롤 동작을 나타내는 `ItemsUpdatingScrollMode` 열거형 값을 가져오거나 설정 합니다. `ItemsUpdatingScrollMode` 열거형은 다음 멤버를 정의합니다.

- `KeepItemsInView` 새 항목이 추가 될 때 표시 되는 첫 번째 항목을 유지 하기 위해 스크롤 오프셋을 조정 합니다.
- `KeepScrollOffset`는 새 항목이 추가 될 때 목록의 시작 부분을 기준으로 스크롤 오프셋을 유지 합니다.
- 새 항목이 추가 될 때 마지막 항목이 표시 되도록 스크롤 오프셋을 조정 `KeepLastItemInView` 합니다.

기본값은 `ItemsUpdatingScrollMode` 속성은 `KeepItemsInView`합니다. 따라서 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 에 새 항목이 추가 되 면 목록에서 첫 번째로 표시 되는 항목이 계속 표시 됩니다. 새로 추가 된 항목이 목록의 맨 아래에 항상 표시 되도록 하려면 `ItemsUpdatingScrollMode` 속성을 `KeepLastItemInView`으로 설정 해야 합니다.

```xaml
<CarouselView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CarouselView>
```

해당 하는 C# 코드가입니다.

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## <a name="scroll-bar-visibility"></a>스크롤 막대 표시 여부

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 은 바인딩 가능한 속성에 의해 지원 되는 `HorizontalScrollBarVisibility` 및 `VerticalScrollBarVisibility` 속성을 정의 합니다. 이러한 속성은 가로 또는 세로 스크롤 막대가 표시 되는 경우를 나타내는 [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) 열거형 값을 가져오거나 설정 합니다. `ScrollBarVisibility` 열거형은 다음 멤버를 정의합니다.

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility) 는 플랫폼에 대 한 기본 스크롤 막대 동작을 나타내며 `HorizontalScrollBarVisibility` 및 `VerticalScrollBarVisibility` 속성의 기본값입니다.
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility) 콘텐츠가 보기에 맞는 경우에도 스크롤 막대가 표시 됨을 나타냅니다.
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility) 은 콘텐츠가 뷰에 맞지 않는 경우에도 스크롤 막대가 표시 되지 않음을 나타냅니다.

## <a name="snap-points"></a>맞춤 요소

사용자가 스크롤을 시작 swipes 면 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 이 기능은 스크롤이 중지 될 때 항목이 위치에 스냅 되 고 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) 클래스의 다음 속성에 의해 제어 되기 때문에 맞추기 라고 합니다.

- [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)형식의 [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)은 스크롤할 때 스냅 지점의 동작을 지정 합니다.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)형식의 [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)는 맞춤 지점이 항목에 정렬 되는 방법을 지정 합니다.

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

> [!NOTE]
> 맞추기를 수행 하는 경우 최소한의 동작을 생성 하는 방향으로 수행 됩니다.

### <a name="snap-points-type"></a>맞춤 지점의 유형

[`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) 열거형은 다음 멤버를 정의 합니다.

- `None` 스크롤이 항목에 맞추지 않음을 나타냅니다.
- `Mandatory`은 항상 내용이 관성의 방향에 따라 스크롤할 수 있는 가장 가까운 맞춤 지점에 맞춰집니다.
- `MandatorySingle`은 `Mandatory`와 동일한 동작을 나타내지만 한 번에 한 항목만 스크롤합니다.

기본적으로 [`CarouselView`](xref:Xamarin.Forms.CarouselView)에서 [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) 속성은 `SnapPointsType.MandatorySingle`로 설정 되므로 스크롤은 한 번에 한 항목만 스크롤합니다.

다음 스크린샷에는 맞추기 기능이 해제 된 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 표시 됩니다.

[![IOS 및 Android에서 스냅 지점이 없는 CarouselView의 스크린샷](scrolling-images/snappoints-none.png "CarouselView (snap points 불포함)")](scrolling-images/snappoints-none-large.png#lightbox "CarouselView (snap points 불포함)")

### <a name="snap-points-alignment"></a>맞춤 지점의 맞춤

[`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) 열거형은 `Start`, `Center`및 `End` 멤버를 정의 합니다.

> [!IMPORTANT]
> [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) 속성의 값은 [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) 속성이 `Mandatory`또는 `MandatorySingle`로 설정 된 경우에만 적용 됩니다.

#### <a name="start"></a>시작

`SnapPointsAlignment.Start` 멤버는 맞춤 지점이 항목의 선행 가장자리에 맞춰지도록 지정 합니다. 다음 XAML 예제에서는이 열거형 멤버를 설정 하는 방법을 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Start" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

해당 하는 C# 코드가입니다.

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    // ...
};
```

사용자가 [`CarouselView`](xref:Xamarin.Forms.CarouselView)가로 스크롤으로 스크롤을 시작 하면 왼쪽 항목이 뷰의 왼쪽에 정렬 됩니다.

[![IOS 및 Android에서 시작 하는 CarouselView의 스크린샷](scrolling-images/snappoints-start.png "시작 스냅 지점이 있는 CarouselView")](scrolling-images/snappoints-start-large.png#lightbox "시작 스냅 지점이 있는 CarouselView")

#### <a name="center"></a>중심

`SnapPointsAlignment.Center` 멤버는 맞춤 지점이 항목의 가운데에 정렬 됨을 나타냅니다.

기본적으로 [`CarouselView`](xref:Xamarin.Forms.CarouselView) [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) 속성은 `Center`로 설정 됩니다. 그러나 완전성을 위해 다음 XAML 예제에서는이 열거형 멤버를 설정 하는 방법을 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Center" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

해당 하는 C# 코드가입니다.

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    // ...
};
```

사용자가 [`CarouselView`](xref:Xamarin.Forms.CarouselView)가로 스크롤으로 스크롤을 시작 하는 경우 가운데 항목은 뷰의 가운데에 정렬 됩니다 (swipes).

[![IOS 및 Android에서 가운데 맞춤 지점이 있는 CarouselView의 스크린샷](scrolling-images/snappoints-center.png "가운데 맞춤 지점이 있는 CarouselView")](scrolling-images/snappoints-center-large.png#lightbox "가운데 맞춤 지점이 있는 CarouselView")

#### <a name="end"></a>끝

`SnapPointsAlignment.End` 멤버는 맞춤 지점이 항목의 후행 가장자리에 맞춰지도록 지정 합니다. 다음 XAML 예제에서는이 열거형 멤버를 설정 하는 방법을 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="End" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

해당 하는 C# 코드가입니다.

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    // ...
};
```

사용자가 [`CarouselView`](xref:Xamarin.Forms.CarouselView)가로 스크롤으로 스크롤을 시작 하면 오른쪽 항목이 뷰의 오른쪽에 맞춰집니다.

[![IOS 및 Android에서 최종 맞춤 지점이 있는 CarouselView의 스크린샷](scrolling-images/snappoints-end.png "끝 맞춤 지점이 있는 CarouselView")](scrolling-images/snappoints-end-large.png#lightbox "끝 맞춤 지점이 있는 CarouselView")

## <a name="related-links"></a>관련 링크

- [CarouselView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
