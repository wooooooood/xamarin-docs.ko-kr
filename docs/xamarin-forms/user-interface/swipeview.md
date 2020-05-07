---
title: Xamarin.ios SwipeView
description: SwipeView는 콘텐츠 항목 주위에 래핑하는 컨테이너 컨트롤이 며 살짝 밀기 제스처로 표시 되는 상황에 맞는 메뉴 항목을 제공 합니다.
ms.prod: xamarin
ms.assetId: 602456B5-701B-4948-B454-B1F31283F1CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/26/2020
ms.openlocfilehash: 992e4dd1a2b2a1d1a4f0b76dadf4704241486415
ms.sourcegitcommit: 520ea9d52266f745d2c09642bac21f64a56f8c31
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82859100"
---
# <a name="xamarinforms-swipeview"></a>Xamarin.ios SwipeView

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)

는 `SwipeView` 콘텐츠 항목 주위에 래핑하는 컨테이너 컨트롤이 며 살짝 밀기 제스처로 표시 되는 상황에 맞는 메뉴 항목을 제공 합니다.

[![IOS 및 Android의 CollectionView SwipeView 항목의 스크린샷](swipeview-images/swipeview-collectionview.png "SwipeView 항목 살짝 밀기")](swipeview-images/swipeview-collectionview-large.png#lightbox "SwipeView 항목 살짝 밀기")

`SwipeView`는 Xamarin. Forms 4.4에서 사용할 수 있습니다. 그러나 현재 실험적 이며 `AppDelegate` ,를 호출 `MainActivity` `App` `Forms.Init`하기 전에 IOS의 클래스, Android의 클래스 또는 UWP의 클래스에 다음 코드 줄을 추가 하 여 사용할 수 있습니다.

```csharp
Forms.SetFlags("SwipeView_Experimental");
```

`SwipeView`는 다음 속성을 정의합니다.

- `LeftItems`는 컨트롤을 `SwipeItems`좌 변에 스와이프 때 호출할 수 있는 살짝 밀기 항목을 나타내는 형식의입니다.
- `RightItems`는 컨트롤이 오른쪽 `SwipeItems`에서 스와이프 때 호출할 수 있는 살짝 밀기 항목을 나타내는 형식의입니다.
- `TopItems`는 컨트롤을 `SwipeItems`위에서 아래로 스와이프 때 호출할 수 있는 살짝 밀기 항목을 나타내는 형식의입니다.
- `BottomItems`는 컨트롤이 아래쪽 `SwipeItems`에서 스와이프 때 호출할 수 있는 살짝 밀기 항목을 나타내는 형식의입니다.

이러한 속성은 개체에 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 의해 지원 됩니다. 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

또한은 `SwipeView` [`Content`](xref:Xamarin.Forms.ContentView.Content) [`ContentView`](xref:Xamarin.Forms.ContentView) 클래스에서 속성을 상속 합니다. `Content` 속성은 `SwipeView` 클래스의 콘텐츠 속성 이므로 명시적으로 설정할 필요가 없습니다.

또한 `SwipeView` 클래스는 다음과 같은 네 개의 이벤트를 정의 합니다.

- `SwipeStarted`는 살짝 밀기가 시작 될 때 발생 합니다. 이 `SwipeStartedEventArgs` 이벤트와 함께 제공 되는 개체 `SwipeDirection` 에는 형식의 `SwipeDirection`속성이 있습니다.
- `SwipeChanging`는 살짝 밀기 이동 시 발생 합니다. 이 `SwipeChangingEventArgs` 이벤트와 함께 제공 되는 개체 `SwipeDirection` 에는 형식의 속성 `SwipeDirection`및 형식의 `Offset` 속성이 있습니다 `double`.
- `SwipeEnded`는 살짝 밀기가 종료 될 때 발생 합니다. 이 `SwipeEndedEventArgs` 이벤트와 함께 제공 되는 개체 `SwipeDirection` 에는 형식의 `SwipeDirection`속성이 있습니다.
- `CloseRequested`는 살짝 밀기 항목이 닫힐 때 발생 합니다.

또한에 `SwipeView` 는 및 `Open` 메서드가 `Close` 포함 되어 있으며,이 메서드는 각각 살짝 밀기 항목을 프로그래밍 방식으로 열고 닫습니다.

> [!NOTE]
> `SwipeView`에는를 `SwipeView`열 때 사용 되는 전환을 제어 하는 IOS 및 Android에 대 한 플랫폼별가 있습니다. 자세한 내용은 [SwipeView 살짝 밀기 전환 모드 In iOS](~/xamarin-forms/platform/ios/swipeview-swipetransitionmode.md) 및 [SwipeView 살짝 밀기 전환 모드 (Android에서](~/xamarin-forms/platform/android/swipeview-swipetransitionmode.md))를 참조 하세요.

## <a name="create-a-swipeview"></a>SwipeView 만들기

는 `SwipeView` 에서 `SwipeView` 래핑하는 콘텐츠와 살짝 밀기 제스처로 표시 되는 살짝 밀기 항목을 정의 해야 합니다. 살짝 밀기 항목은 `SwipeItem` `SwipeView` `LeftItems`, `RightItems` `TopItems`, 또는 `BottomItems`의 네 방향 컬렉션 중 하나에 배치 되는 하나 이상의 개체입니다.

다음 예제에서는 XAML로를 `SwipeView` 인스턴스화하는 방법을 보여 줍니다.

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
    <Grid HeightRequest="60"
          WidthRequest="300"
          BackgroundColor="LightGray">
        <Label Text="Swipe right"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Grid>
</SwipeView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
// SwipeItems
SwipeItem favoriteSwipeItem = new SwipeItem
{
    Text = "Favorite",
    IconImageSource = "favorite.png",
    BackgroundColor = Color.LightGreen
};
favoriteSwipeItem.Invoked += OnFavoriteSwipeItemInvoked;

SwipeItem deleteSwipeItem = new SwipeItem
{
    Text = "Delete",
    IconImageSource = "delete.png",
    BackgroundColor = Color.LightPink
};
deleteSwipeItem.Invoked += OnDeleteSwipeItemInvoked;

List<SwipeItem> swipeItems = new List<SwipeItem>() { favoriteSwipeItem, deleteSwipeItem };

// SwipeView content
Grid grid = new Grid
{
    HeightRequest = 60,
    WidthRequest = 300,
    BackgroundColor = Color.LightGray
};
grid.Children.Add(new Label
{
    Text = "Swipe right",
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
});

SwipeView swipeView = new SwipeView
{
    LeftItems = new SwipeItems(swipeItems),
    Content = grid
};
```

이 예제에서 `SwipeView` 콘텐츠는를 포함 [`Grid`](xref:Xamarin.Forms.Grid) 하는입니다. [`Label`](xref:Xamarin.Forms.Label)

[![IOS 및 Android의 SwipeView 콘텐츠 스크린샷](swipeview-images/swipeview-content.png "SwipeView 콘텐츠")](swipeview-images/swipeview-content-large.png#lightbox "SwipeView 콘텐츠")

살짝 밀기 항목은 `SwipeView` 콘텐츠에 대 한 작업을 수행 하는 데 사용 되며, 컨트롤이 왼쪽에서 스와이프 때 표시 됩니다.

[![IOS 및 Android에서 SwipeView 살짝 밀기 항목의 스크린샷](swipeview-images/swipeview-swipeitems.png "SwipeView 항목 살짝 밀기")](swipeview-images/swipeview-swipeitems-large.png#lightbox "SwipeView 항목 살짝 밀기")

기본적으로 사용자가 탭 할 때 살짝 밀기 항목이 실행 됩니다. 그러나 이 동작을 변경할 수 있습니다. 자세한 내용은 [살짝 밀기 모드](#swipe-mode)를 참조 하세요.

살짝 밀기 항목이 실행 된 후에는 살짝 밀기 항목이 숨겨지고 `SwipeView` 콘텐츠가 다시 표시 됩니다. 그러나 이 동작을 변경할 수 있습니다. 자세한 내용은 [살짝 밀기 동작](#swipe-behavior)을 참조 하세요.

> [!NOTE]
> 살짝 밀기 콘텐츠 및 살짝 밀기 항목은 인라인으로 배치 되거나 리소스로 정의 될 수 있습니다.

## <a name="swipe-items"></a>항목 살짝 밀기

`LeftItems` `RightItems`, `TopItems`, 및 `BottomItems` 컬렉션은 모두 형식 `SwipeItems`입니다. 클래스 `SwipeItems` 는 다음 속성을 정의 합니다.

- `Mode`는 살짝의 `SwipeMode`상호 작용 효과를 나타내는 형식의입니다. 살짝 밀기 모드에 대 한 자세한 내용은 [살짝 밀기 모드](#swipe-mode)를 참조 하세요.
- `SwipeBehaviorOnInvoked`는 `SwipeView` 살짝 밀기 `SwipeBehaviorOnInvoked`항목이 호출 된 후의 동작 방식을 나타내는 형식의입니다. 살짝 밀기 동작에 대 한 자세한 내용은 [살짝 밀기 동작](#swipe-behavior)을 참조 하세요.

이러한 속성은 개체에 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 의해 지원 됩니다. 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

각 살짝 밀기 항목은 4 `SwipeItem` `SwipeItems` 방향 컬렉션 중 하나에 배치 되는 개체로 정의 됩니다. 클래스 `SwipeItem` 는 [`MenuItem`](xref:Xamarin.Forms.MenuItem) 클래스에서 파생 되 고 다음 멤버를 추가 합니다.

- 살짝 `BackgroundColor` 밀기 항목의 배경색 `Color`을 정의 하는 형식의 속성입니다. 이 속성은 바인딩 가능한 속성에 의해 지원 됩니다.
- 살짝 `Invoked` 밀기 항목이 실행 될 때 발생 하는 이벤트입니다.

> [!IMPORTANT]
> 클래스 [`MenuItem`](xref:Xamarin.Forms.MenuItem) 는, `CommandParameter` `IconImageSource`, 및 `Text`를 비롯 `Command`한 여러 속성을 정의 합니다. 이러한 속성은 `SwipeItem` 개체에 대해 설정 하 여 모양을 정의 하 고, 살짝 밀기 항목이 `ICommand` 호출 될 때 실행 되는를 정의할 수 있습니다. 자세한 내용은 [Xamarin.ios MenuItem](~/xamarin-forms/user-interface/menuitem.md)을 참조 하세요.

다음 예제에서는의 `SwipeItem` `LeftItems` 컬렉션에서 두 개의 개체를 보여 `SwipeView`줍니다.

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

각 `SwipeItem` 의 모양은 `Text`, `IconImageSource`및 `BackgroundColor` 속성의 조합에 의해 정의 됩니다.

[![IOS 및 Android에서 SwipeView 살짝 밀기 항목의 스크린샷](swipeview-images/swipeview-swipeitems.png "SwipeView 항목 살짝 밀기")](swipeview-images/swipeview-swipeitems-large.png#lightbox "SwipeView 항목 살짝 밀기")

를 `SwipeItem` 탭 하면 해당 `Invoked` 이벤트가 발생 하 고 등록 된 이벤트 처리기에 의해 처리 됩니다. 또는 `SwipeItem` 이 호출 `Command` 될 때 실행 되는 `ICommand` 구현으로 속성을 설정할 수 있습니다.

> [!NOTE]
> 의 모양이 `Text` 또는 `IconImageSource` 속성을 `SwipeItem` 사용 하 여 정의 되는 경우 콘텐츠는 항상 가운데 맞춤 됩니다.

살짝 밀기 항목을 `SwipeItem` 개체로 정의 하는 것 외에도 사용자 지정 살짝 밀기 항목 뷰를 정의할 수 있습니다. 자세한 내용은 [사용자 지정 살짝 밀기 항목](#custom-swipe-items)을 참조 하세요.

## <a name="swipe-direction"></a>살짝 밀기 방향

`SwipeView`는 `SwipeItems` `SwipeItem` 개체를 추가할 방향 컬렉션에 의해 정의 되는 살짝 밀기 방향을 사용 하 여 네 가지 살짝 밀기 방향을 지원 합니다. 각 살짝 밀기 방향은 자신의 살짝 밀기 항목을 포함할 수 있습니다. 예를 들어 다음 예제에서는 살짝 밀기 `SwipeView` 항목이 살짝 밀기 방향에 종속 된을 보여 줍니다.

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Command="{Binding DeleteCommand}" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <SwipeView.RightItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Command="{Binding FavoriteCommand}" />
            <SwipeItem Text="Share"
                       IconImageSource="share.png"
                       BackgroundColor="LightYellow"
                       Command="{Binding ShareCommand}" />
        </SwipeItems>
    </SwipeView.RightItems>
    <!-- Content -->
</SwipeView>
```

이 예제에서는 `SwipeView` 콘텐츠를 오른쪽 이나 왼쪽으로 스와이프 수 있습니다. 오른쪽으로 살짝 밀어 **삭제** 살짝 밀기 항목이 표시 되 고 왼쪽으로 살짝 밀어 있으면 **즐겨찾기가** 표시 되 고 살짝 밀기 항목이 **공유** 됩니다.

> [!WARNING]
> 에서 한 번에 한 방향 `SwipeItems` 컬렉션의 인스턴스를 하나만 설정할 수 있습니다 `SwipeView`. 따라서에 대 한 두 개의 `LeftItems` 정의를 가질 `SwipeView`수 없습니다.

, `SwipeStarted` `SwipeChanging`및 `SwipeEnded` 이벤트는 이벤트 인수에서 속성을 `SwipeDirection` 통해 살짝 밀기 방향을 보고 합니다. 이 속성은 다음 네 `SwipeDirection`개의 멤버로 구성 된 열거형 인 형식입니다.

- `Right`오른쪽으로 살짝 밀기가 발생 했음을 나타냅니다.
- `Left`왼쪽으로 살짝 밀기가 발생 했음을 나타냅니다.
- `Up`위쪽으로 살짝 밀기가 발생 했음을 나타냅니다.
- `Down`아래쪽으로 살짝 밀기가 발생 했음을 나타냅니다.

## <a name="swipe-mode"></a>살짝 밀기 모드

`SwipeItems` 클래스에는 살짝 `Mode` 밀기 상호 작용의 효과를 나타내는 속성이 있습니다. 이 속성은 `SwipeMode` 열거형 멤버 중 하나로 설정 해야 합니다.

- `Reveal`살짝 밀기 항목이 표시 됨을 나타냅니다. 이 값은 `SwipeItems.Mode` 속성의 기본값입니다.
- `Execute`살짝 밀기 항목이 실행 됨을 나타냅니다.

표시 모드에서 사용자는 하나 이상의 살짝 `SwipeView` 밀기 항목으로 구성 된 메뉴를 열기 위해를 swipes, 살짝 밀기 항목을 명시적으로 눌러 실행 해야 합니다. 살짝 밀기 항목이 실행 된 후에는 살짝 밀기 항목이 닫히고 `SwipeView` 콘텐츠가 다시 표시 됩니다. 실행 모드에서 사용자는를 `SwipeView` swipes 하 여 하나 이상의 살짝 밀기 항목으로 구성 된 메뉴를 열고 자동으로 실행 됩니다. 실행 후에는 살짝 밀기 항목이 닫히고 `SwipeView` 콘텐츠가 다시 표시 됩니다.

다음 예제에서는 실행 모드 `SwipeView` 를 사용 하도록 구성 된를 보여 줍니다.

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems Mode="Execute">
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Command="{Binding DeleteCommand}" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

이 예제에서 콘텐츠는 `SwipeView` 즉시 실행 되는 살짝 밀기 항목을 표시 하기 위해 스와이프 권한이 될 수 있습니다. 실행 후 `SwipeView` 콘텐츠가 다시 표시 됩니다.

## <a name="swipe-behavior"></a>살짝 밀기 동작

클래스 `SwipeItems` 에는 `SwipeBehaviorOnInvoked` `SwipeView` 속성이 있으며이 속성은 살짝 밀기 항목이 호출 된 후이 동작 하는 방식을 나타냅니다. 이 속성은 `SwipeBehaviorOnInvoked` 열거형 멤버 중 하나로 설정 해야 합니다.

- `Auto`표시 모드에서 살짝 밀기 항목이 `SwipeView` 호출 된 후 닫힙니다. 실행 모드에서는 살짝 밀기 항목이 호출 된 `SwipeView` 후에가 계속 열려 있음을 나타냅니다. 이 값은 `SwipeItems.SwipeBehaviorOnInvoked` 속성의 기본값입니다.
- `Close`살짝 밀기 항목이 `SwipeView` 호출 된 후이 닫히는 것을 나타냅니다.
- `RemainOpen`살짝 밀기 항목이 `SwipeView` 호출 된 후가 열린 상태로 유지 됨을 나타냅니다.

다음 예제에서는 살짝 밀기 `SwipeView` 항목이 호출 된 후에도 열려 있는 상태로 유지 되도록 구성 된를 보여 줍니다.

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems SwipeBehaviorOnInvoked="RemainOpen">
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

## <a name="custom-swipe-items"></a>사용자 지정 살짝 밀기 항목

사용자 지정 살짝 밀기 항목은 `SwipeItemView` 유형을 사용 하 여 정의할 수 있습니다. 클래스 `SwipeItemView` 는 [`ContentView`](xref:Xamarin.Forms.ContentView) 클래스에서 파생 되 고 다음 속성을 추가 합니다.

- `Command`는 살짝 밀기 `ICommand`항목을 누를 때 실행 되는 형식의입니다.
- `object` 형식의 `CommandParameter` - `Command`에 전달되는 매개 변수입니다.

이러한 속성은 개체에 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 의해 지원 됩니다. 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

또한 `SwipeItemView` 클래스는 `Command` 가 실행 `Invoked` 된 후 항목을 누를 때 발생 하는 이벤트를 정의 합니다.

다음 예제에서는의 `SwipeItemView` `LeftItems` 컬렉션에 있는 개체를 보여 줍니다 `SwipeView`.

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItemView Command="{Binding CheckAnswerCommand}"
                           CommandParameter="{Binding Source={x:Reference resultEntry}, Path=Text}">
                <StackLayout Margin="10"
                             WidthRequest="300">
                    <Entry x:Name="resultEntry"
                           Placeholder="Enter answer"
                           HorizontalOptions="CenterAndExpand" />
                    <Label Text="Check"
                           FontAttributes="Bold"
                           HorizontalOptions="Center" />
                </StackLayout>
            </SwipeItemView>
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

이 `SwipeItemView` 예제에서는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Entry`](xref:Xamarin.Forms.Entry) 및를 [`Label`](xref:Xamarin.Forms.Label)포함 하는로 구성 됩니다. 사용자 `Entry`가에 입력을 입력 한 후에는 `SwipeViewItem` `ICommand` `SwipeItemView.Command` 속성으로 정의 된을 실행 하는 나머지를 누를 수 있습니다.

## <a name="open-and-close-a-swipeview-programmatically"></a>프로그래밍 방식으로 SwipeView 열기 및 닫기

`SwipeView`에 `Open` 는 `Close` 각각 살짝 밀기 항목을 프로그래밍 방식으로 열고 닫는 및 메서드가 포함 되어 있습니다.

메서드 `Open` 는가 열리는 `OpenSwipeItem` 방향을 `SwipeView` 지정 하기 위해 인수를 필요로 합니다. 열거형 `OpenSwipeItem` 에는 네 개의 멤버가 있습니다.

- `LeftItems`- `LeftItems` 컬렉션에서 살짝 밀기 `SwipeView` 항목을 표시 하기 위해가 왼쪽에서 열리도록 지정 합니다.
- `TopItems`- `TopItems` 컬렉션에서 살짝 밀기 `SwipeView` 항목을 표시 하기 위해가 맨 위에서 열립니다.
- `RightItems`- `RightItems` 컬렉션에서 살짝 밀기 `SwipeView` 항목을 표시 하기 위해가 오른쪽에서 열리도록 지정 합니다.
- `BottomItems`- `BottomItems` 컬렉션의 살짝 밀기 `SwipeView` 항목을 표시 하기 위해가 아래쪽에서 열리도록 지정 합니다.

`SwipeView` 이름이 `swipeView`지정 된 경우 다음 예제에서는를 `SwipeView` 열어 `LeftItems` 컬렉션의 살짝 밀기 항목을 표시 하는 방법을 보여 줍니다.

```csharp
swipeView.Open(OpenSwipeItem.LeftItems);
```

그런 `swipeView` 다음 `Close` 메서드를 사용 하 여를 닫을 수 있습니다.

```csharp
swipeView.Close();
```

> [!NOTE]
> `Close` 메서드가 호출 되 면 `CloseRequested` 이벤트가 발생 합니다.

## <a name="disable-a-swipeview"></a>SwipeView 사용 안 함

응용 프로그램에서 콘텐츠 항목을 살짝 밀어 넣을 수 없는 상태를 입력할 수 있습니다. 이러한 경우에는 `SwipeView` `IsEnabled` 속성을로 `false`설정 하 여를 비활성화할 수 있습니다. 이렇게 하면 사용자가 살짝 밀기 항목을 표시 하기 위해 콘텐츠를 살짝 밀기 할 수 없습니다.

`Command` 또한 `SwipeItem` 또는 `SwipeItemView`의 속성을 정의 하는 경우의 `CanExecute` 대리자를 지정 하 여 `ICommand` 살짝 밀기 항목을 사용 하거나 사용 하지 않도록 설정할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [SwipeView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)
- [Xamarin.Forms MenuItem](~/xamarin-forms/user-interface/menuitem.md)
