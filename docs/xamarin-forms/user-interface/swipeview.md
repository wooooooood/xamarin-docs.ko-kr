---
title: Xamarin.ios SwipeView
description: SwipeView는 콘텐츠 항목 주위에 래핑하는 컨테이너 컨트롤이 며 살짝 밀기 제스처로 표시 되는 상황에 맞는 메뉴 항목을 제공 합니다.
ms.prod: xamarin
ms.assetId: 602456B5-701B-4948-B454-B1F31283F1CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/11/2020
ms.openlocfilehash: 6131287b200846a033e0c476d7039dfd774cab68
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635591"
---
# <a name="xamarinforms-swipeview"></a>Xamarin.ios SwipeView

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)

`SwipeView`은 콘텐츠 항목 주위에 래핑하는 컨테이너 컨트롤이 며 살짝 밀기 제스처로 표시 되는 상황에 맞는 메뉴 항목을 제공 합니다.

[![IOS 및 Android의 CollectionView SwipeView 항목의 스크린샷](swipeview-images/swipeview-collectionview.png "SwipeView 항목 살짝 밀기")](swipeview-images/swipeview-collectionview-large.png#lightbox "SwipeView 항목 살짝 밀기")

`SwipeView`는 Xamarin. Forms 4.4에서 사용할 수 있습니다. 그러나 현재 실험적 이며, `Forms.Init`를 호출 하기 전에 iOS의 `AppDelegate` 클래스, Android의 `MainActivity` 클래스 또는 UWP의 `App` 클래스에 다음 코드 줄을 추가 하 여 사용할 수 있습니다.

```csharp
Forms.SetFlags("SwipeView_Experimental");
```

`SwipeView`는 다음 속성을 정의 합니다.

- `LeftItems`은 컨트롤이 왼쪽에서 스와이프 때 호출할 수 있는 살짝 밀기 항목을 나타내는 `SwipeItems`형식의입니다.
- `RightItems`는 컨트롤이 우변에서 스와이프 때 호출할 수 있는 살짝 밀기 항목을 나타내는 `SwipeItems`형식의입니다.
- `TopItems`는 컨트롤이 위에서 아래로 스와이프 때 호출 될 수 있는 살짝 밀기 항목을 나타내는 `SwipeItems`형식의입니다.
- `BottomItems`는 컨트롤이 아래쪽에서 위쪽으로 스와이프 때 호출할 수 있는 살짝 밀기 항목을 나타내는 `SwipeItems`형식의입니다.

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, 데이터 바인딩의 대상이 될 수 있고 스타일을 지정할 수 있습니다.

또한 `SwipeView`는 [`ContentView`](xref:Xamarin.Forms.ContentView) 클래스에서 [`Content`](xref:Xamarin.Forms.ContentView.Content) 속성을 상속 합니다. `Content` 속성은 `SwipeView` 클래스의 콘텐츠 속성 이므로 명시적으로 설정할 필요가 없습니다.

`SwipeView` 클래스는 또한 다음 네 가지 이벤트를 정의 합니다.

- `SwipeStarted`는 살짝 밀기가 시작 될 때 발생 합니다. 이 이벤트와 함께 제공 되는 `SwipeStartedEventArgs` 개체에는 `SwipeDirection`형식의 `SwipeDirection` 속성이 있습니다.
- `SwipeChanging`는 살짝 밀기 이동 시 발생 합니다. 이 이벤트와 함께 제공 되는 `SwipeChangingEventArgs` 개체에는 `SwipeDirection`형식의 `SwipeDirection` 속성과 `double`형식의 `Offset` 속성이 있습니다.
- `SwipeEnded`는 살짝 밀기가 종료 될 때 발생 합니다. 이 이벤트와 함께 제공 되는 `SwipeEndedEventArgs` 개체에는 `SwipeDirection`형식의 `SwipeDirection` 속성이 있습니다.
- `CloseRequested`는 살짝 밀기 항목이 닫힐 때 발생 합니다.

또한 `SwipeView`는 살짝 밀기 항목을 닫는 `Close` 메서드를 정의 합니다.

> [!NOTE]
> `SwipeView`에는 `SwipeView`를 열 때 사용 되는 전환을 제어 하는 iOS 및 Android에 대 한 플랫폼별가 있습니다. 자세한 내용은 [SwipeView 살짝 밀기 전환 모드 In iOS](~/xamarin-forms/platform/ios/swipeview-swipetransitionmode.md) 및 [SwipeView 살짝 밀기 전환 모드 (Android에서](~/xamarin-forms/platform/android/swipeview-swipetransitionmode.md))를 참조 하세요.

## <a name="create-a-swipeview"></a>SwipeView 만들기

`SwipeView`은 `SwipeView` 래핑하는 콘텐츠 및 살짝 밀기 제스처로 표시 되는 살짝 밀기 항목을 정의 해야 합니다. 살짝 밀기 항목은 네 개의 `SwipeView` 방향 컬렉션 `LeftItems`, `RightItems`, `TopItems`또는 `BottomItems`중 하나에 배치 되는 하나 이상의 `SwipeItem` 개체입니다.

다음 예제에서는 XAML로 `SwipeView`를 인스턴스화하는 방법을 보여 줍니다.

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

이 예제에서 `SwipeView` 내용은 [`Label`](xref:Xamarin.Forms.Label)를 포함 하는 [`Grid`](xref:Xamarin.Forms.Grid) 입니다.

[![IOS 및 Android의 SwipeView 콘텐츠 스크린샷](swipeview-images/swipeview-content.png "SwipeView 콘텐츠")](swipeview-images/swipeview-content-large.png#lightbox "SwipeView 콘텐츠")

살짝 밀기 항목은 `SwipeView` 콘텐츠에 대 한 작업을 수행 하는 데 사용 되며, 왼쪽에서 컨트롤이 스와이프 때 표시 됩니다.

[![IOS 및 Android에서 SwipeView 살짝 밀기 항목의 스크린샷](swipeview-images/swipeview-swipeitems.png "SwipeView 항목 살짝 밀기")](swipeview-images/swipeview-swipeitems-large.png#lightbox "SwipeView 항목 살짝 밀기")

기본적으로 사용자가 탭 할 때 살짝 밀기 항목이 실행 됩니다. 그러나 이 동작을 변경할 수 있습니다. 자세한 내용은 [살짝 밀기 모드](#swipe-mode)를 참조 하세요.

살짝 밀기 항목이 실행 된 후에는 살짝 밀기 항목이 숨겨지고 `SwipeView` 콘텐츠가 다시 표시 됩니다. 그러나 이 동작을 변경할 수 있습니다. 자세한 내용은 [살짝 밀기 동작](#swipe-behavior)을 참조 하세요.

> [!NOTE]
> 살짝 밀기 콘텐츠 및 살짝 밀기 항목은 인라인으로 배치 되거나 리소스로 정의 될 수 있습니다.

## <a name="swipe-items"></a>항목 살짝 밀기

`LeftItems`, `RightItems`, `TopItems`및 `BottomItems` 컬렉션은 모두 `SwipeItems`형식입니다. `SwipeItems` 클래스는 다음 속성을 정의 합니다.

- `Mode`는 살짝의 상호 작용 효과를 나타내는 `SwipeMode`형식입니다. 살짝 밀기 모드에 대 한 자세한 내용은 [살짝 밀기 모드](#swipe-mode)를 참조 하세요.
- `SwipeBehaviorOnInvoked`형식의 `SwipeBehaviorOnInvoked`는 살짝 밀기 항목이 호출 된 후 `SwipeView` 동작 하는 방식을 나타냅니다. 살짝 밀기 동작에 대 한 자세한 내용은 [살짝 밀기 동작](#swipe-behavior)을 참조 하세요.

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, 데이터 바인딩의 대상이 될 수 있고 스타일을 지정할 수 있습니다.

각 살짝 밀기 항목은 네 개의 `SwipeItems` 방향 컬렉션 중 하나에 배치 되는 `SwipeItem` 개체로 정의 됩니다. `SwipeItem` 클래스는 [`MenuItem`](xref:Xamarin.Forms.MenuItem) 클래스에서 파생 되 고 다음 멤버를 추가 합니다.

- 살짝 밀기 항목의 배경색을 정의 하는 `Color`형식의 `BackgroundColor` 속성입니다. 이 속성은 바인딩 가능한 속성에 의해 지원 됩니다.
- 살짝 밀기 항목이 실행 될 때 발생 하는 `Invoked` 이벤트입니다.

> [!IMPORTANT]
> [`MenuItem`](xref:Xamarin.Forms.MenuItem) 클래스는 `Command`, `CommandParameter`, `IconImageSource`및 `Text`를 비롯 한 여러 속성을 정의 합니다. 이러한 속성은 `SwipeItem` 개체에 설정 하 여 모양을 정의 하 고, 살짝 밀기 항목이 호출 될 때 실행 되는 `ICommand`를 정의할 수 있습니다. 자세한 내용은 [Xamarin.ios MenuItem](~/xamarin-forms/user-interface/menuitem.md)을 참조 하세요.

다음 예제에서는 `SwipeView`의 `LeftItems` 컬렉션에 `SwipeItem` 개체 두 개를 보여 줍니다.

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

각 `SwipeItem`의 모양은 `Text`, `IconImageSource`및 `BackgroundColor` 속성을 조합 하 여 정의 됩니다.

[![IOS 및 Android에서 SwipeView 살짝 밀기 항목의 스크린샷](swipeview-images/swipeview-swipeitems.png "SwipeView 항목 살짝 밀기")](swipeview-images/swipeview-swipeitems-large.png#lightbox "SwipeView 항목 살짝 밀기")

`SwipeItem` 탭 하면 해당 `Invoked` 이벤트가 발생 하 고 등록 된 이벤트 처리기에 의해 처리 됩니다. 또는 `SwipeItem`를 호출할 때 실행 되는 `ICommand` 구현으로 `Command` 속성을 설정할 수 있습니다.

> [!NOTE]
> `SwipeItem` 모양이 `Text` 또는 `IconImageSource` 속성을 사용 하 여 정의 되는 경우 콘텐츠는 항상 중앙에 맞춰집니다.

`SwipeItem` 개체로 살짝 밀기 항목을 정의 하는 것 외에도 사용자 지정 살짝 밀기 항목 뷰를 정의할 수 있습니다. 자세한 내용은 [사용자 지정 살짝 밀기 항목](#custom-swipe-items)을 참조 하세요.

## <a name="swipe-direction"></a>살짝 밀기 방향

`SwipeView`는 `SwipeItem` 개체가 추가 되는 방향 `SwipeItems` 컬렉션에 의해 정의 되는 살짝 밀기 방향을 사용 하는 네 가지 살짝 밀기 방향을 지원 합니다. 각 살짝 밀기 방향은 자신의 살짝 밀기 항목을 포함할 수 있습니다. 예를 들어 다음 예제에서는 살짝 밀기 항목이 살짝 밀기 방향에 종속 된 `SwipeView`를 보여 줍니다.

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
> `SwipeView`에서 한 번에 한 방향 `SwipeItems` 컬렉션의 인스턴스 하나만 설정할 수 있습니다. 따라서 `SwipeView`에는 두 개의 `LeftItems` 정의를 사용할 수 없습니다.

`SwipeStarted`, `SwipeChanging`및 `SwipeEnded` 이벤트는 이벤트 인수의 `SwipeDirection` 속성을 통해 살짝 밀기 방향을 보고 합니다. 이 속성은 다음의 네 가지 멤버로 구성 된 열거형 인 `SwipeDirection`형식입니다.

- `Right` 오른쪽으로 살짝 밀기가 발생 했음을 나타냅니다.
- `Left` 왼쪽으로 살짝 밀기가 발생 했음을 나타냅니다.
- `Up` 위쪽으로 살짝 밀기가 발생 했음을 나타냅니다.
- `Down` 아래쪽으로 살짝 밀기가 발생 했음을 나타냅니다.

## <a name="swipe-mode"></a>살짝 밀기 모드

`SwipeItems` 클래스에는 살짝의 상호 작용 효과를 나타내는 `Mode` 속성이 있습니다. 이 속성은 `SwipeMode` 열거형 멤버 중 하나로 설정 해야 합니다.

- `Reveal` 살짝 밀기 항목이 표시 되는 것을 나타냅니다. 이 값은 `SwipeItems.Mode` 속성의 기본값입니다.
- `Execute` 살짝 밀기 항목이 실행 됨을 나타냅니다.

표시 모드에서 사용자는 하나 이상의 살짝 밀기 항목으로 구성 된 메뉴를 여는 `SwipeView`을 swipes 하 고,이를 실행 하려면 살짝 밀기 항목을 명시적으로 눌러야 합니다. 살짝 밀기 항목이 실행 된 후에는 살짝 밀기 항목이 닫히고 `SwipeView` 콘텐츠가 다시 표시 됩니다. 실행 모드에서 사용자는 하나 이상의 살짝 밀기 항목으로 구성 된 메뉴를 열고 자동으로 실행 되는 `SwipeView`을 swipes 합니다. 실행 후에는 살짝 밀기 항목이 닫히고 `SwipeView` 콘텐츠가 다시 표시 됩니다.

다음 예에서는 실행 모드를 사용 하도록 구성 된 `SwipeView`를 보여 줍니다.

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

이 예제에서 `SwipeView` 콘텐츠는 즉시 실행 되는 살짝 밀기 항목을 나타내기 위해 스와이프 권한이 될 수 있습니다. 실행 후 `SwipeView` 콘텐츠가 다시 표시 됩니다.

## <a name="swipe-behavior"></a>살짝 밀기 동작

`SwipeItems` 클래스에는 살짝 밀기 항목이 호출 된 후 `SwipeView` 동작 하는 방식을 나타내는 `SwipeBehaviorOnInvoked` 속성이 있습니다. 이 속성은 `SwipeBehaviorOnInvoked` 열거형 멤버 중 하나로 설정 해야 합니다.

- `Auto`은 살짝 밀기 항목이 호출 된 후에 `SwipeView`를 닫고 실행 모드에서는 살짝 밀기 항목이 호출 된 후에 `SwipeView` 계속 열려 있음을 나타냅니다. 이 값은 `SwipeItems.SwipeBehaviorOnInvoked` 속성의 기본값입니다.
- `Close`는 살짝 밀기 항목이 호출 된 후 `SwipeView`를 닫도록 지정 합니다.
- `RemainOpen`는 살짝 밀기 항목이 호출 된 후 `SwipeView` 열린 상태로 유지 됨을 나타냅니다.

다음 예제에서는 살짝 밀기 항목이 호출 된 후에도 열려 있는 상태로 유지 되도록 구성 된 `SwipeView`를 보여 줍니다.

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

사용자 지정 살짝 밀기 항목은 `SwipeItemView` 유형을 사용 하 여 정의할 수 있습니다. `SwipeItemView` 클래스는 [`ContentView`](xref:Xamarin.Forms.ContentView) 클래스에서 파생 되 고 다음 속성을 추가 합니다.

- `Command`는 살짝 밀기 항목을 누를 때 실행 되는 `ICommand`형식입니다.
- `CommandParameter` 형식의 `object` - `Command`에 전달되는 매개 변수입니다.

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, 데이터 바인딩의 대상이 될 수 있고 스타일을 지정할 수 있습니다.

또한 `SwipeItemView` 클래스는 `Command` 실행 된 후 항목을 탭 할 때 발생 하는 `Invoked` 이벤트를 정의 합니다.

다음 예제에서는 `SwipeView`의 `LeftItems` 컬렉션에 `SwipeItemView` 개체를 보여 줍니다.

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

이 예제에서 `SwipeItemView`은 [`Entry`](xref:Xamarin.Forms.Entry) 및 [`Label`](xref:Xamarin.Forms.Label)를 포함 하는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 구성 됩니다. 사용자가 `Entry`에 입력을 입력 한 후 `SwipeItemView.Command` 속성으로 정의 된 `ICommand`을 실행 하는 나머지 `SwipeViewItem`를 탭 할 수 있습니다.

## <a name="disable-a-swipeview"></a>SwipeView 사용 안 함

응용 프로그램에서 콘텐츠 항목을 살짝 밀어 넣을 수 없는 상태를 입력할 수 있습니다. 이러한 경우 `IsEnabled` 속성을 `false`로 설정 하 여 `SwipeView`를 사용 하지 않도록 설정할 수 있습니다. 이렇게 하면 사용자가 살짝 밀기 항목을 표시 하기 위해 콘텐츠를 살짝 밀기 할 수 없습니다.

또한 `SwipeItem` 또는 `SwipeItemView`의 `Command` 속성을 정의 하는 경우에는 `ICommand`의 `CanExecute` 대리자를 지정 하 여 살짝 밀기 항목을 사용 하거나 사용 하지 않도록 설정할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [SwipeView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)
- [Xamarin.Forms MenuItem](~/xamarin-forms/user-interface/menuitem.md)
