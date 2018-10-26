---
title: 살짝 밀기 제스처 인식기를 추가합니다.
description: 이 문서에는 뷰에서 발생 살짝 밀기 제스처를 인식 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 164976C2-1429-49FB-9EB6-621E2681C19B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/14/2018
ms.openlocfilehash: 95e95d8849824cd2dc31c2019627cc5adbbefeec
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131926"
---
# <a name="adding-a-swipe-gesture-recognizer"></a>살짝 밀기 제스처 인식기를 추가합니다.

_살짝 밀기 제스처 손가락을 가로 또는 세로 방향으로 화면에서 이동 되 고은 콘텐츠 탐색을 시작 하는 데 자주 사용 하는 경우 발생 합니다. 이 문서의 코드 예제에서 수행 되는 [살짝 밀기 제스처](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/SwipeGesture/) 샘플입니다._

있도록를 [ `View` ](xref:Xamarin.Forms.View) 살짝 밀기 제스처를 인식, 만들기를 [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) 인스턴스를 설정 합니다 [ `Direction` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Direction) 속성을는 [ `SwipeDirection` ](xref:Xamarin.Forms.SwipeDirection) 열거형 값 (`Left`, `Right`, `Up`, 또는 `Down`), 필요에 따라 설정 합니다 [ `Threshold` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold) 속성, 핸들을 [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) 이벤트를 새 제스처 인식기를 추가 하 고는 [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) 뷰의 컬렉션입니다. 다음 코드 예제는 `SwipeGestureRecognizer` 에 연결을 [ `BoxView` ](xref:Xamarin.Forms.BoxView):

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

에 해당 하는 다음과 같습니다 C# 코드:

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
```

합니다 [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) 클래스도 포함 되어 있습니다를 [ `Threshold` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold) 속성을 선택적으로 설정할 수 있는 `uint` 해야 달성할 수 있는 최소 안쪽으로 살짝 밀어 거리를 나타내는 값을 장치 독립적 단위에서 인식할 수 있도록 살짝 밉니다. 이 속성의 기본값은 100, 100 대 미만의 장치 독립적 단위를 무시할 수 있는 모든 천공 기와 한다는 의미입니다.

## <a name="recognizing-the-swipe-direction"></a>살짝 밀기 방향을 인식

위의 예에서 합니다 [ `Direction` ](xref:Xamarin.Forms.SwipedEventArgs.Direction) 속성의 값을 단일 합니다 [ `SwipeDirection` ](xref:Xamarin.Forms.SwipeDirection) 열거형입니다. 그러나 것도 가능에서 여러 값으로이 속성을 설정 하는 `SwipeDirection` 열거형 있도록를 [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) 둘 이상의 방향으로 한 번에 대 한 응답에서 이벤트가 발생 합니다. 그러나 제약 조건을 단일 [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) 동일한 축에 나타나는 천공 기와 인식할 수 있습니다. 설정 하 여 가로 축에 나타나는 천공 기와 인식 될 수 있으므로 합니다 `Direction` 속성을 `Left` 고 `Right`:

```xaml
<SwipeGestureRecognizer Direction="Left,Right" Swiped="OnSwiped"/>
```

마찬가지로, 설정 하 여 세로 축에 나타나는 천공 기와 인식 될 수는 [ `Direction` ](xref:Xamarin.Forms.SwipedEventArgs.Direction) 속성을 `Up` 고 `Down`:

```csharp
var swipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up | SwipeDirection.Down };
```

또는 [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) 각 살짝 밀기 방향을 천공 기와 모든 방향으로 인식 하도록 만들 수 있습니다.

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Right" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Up" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Down" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

에 해당 하는 다음과 같습니다 C# 코드:

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;
var rightSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Right };
rightSwipeGesture.Swiped += OnSwiped;
var upSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up };
upSwipeGesture.Swiped += OnSwiped;
var downSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Down };
downSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
boxView.GestureRecognizers.Add(rightSwipeGesture);
boxView.GestureRecognizers.Add(upSwipeGesture);
boxView.GestureRecognizers.Add(downSwipeGesture);
```

> [!NOTE]
> 위의 예제에서 동일한 이벤트 처리기에 응답 합니다 [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) 이벤트 발생 합니다. 그러나 각 [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) 인스턴스는 필요한 경우 다른 이벤트 처리기를 사용할 수 있습니다.

## <a name="responding-to-the-swipe"></a>스와이프에 응답

에 대 한 이벤트 처리기를 [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) 이벤트는 다음 예제에 표시 됩니다.

```csharp
void OnSwiped(object sender, SwipedEventArgs e)
{
    switch (e.Direction)
    {
        case SwipeDirection.Left:
            // Handle the swipe
            break;
        case SwipeDirection.Right:
            // Handle the swipe
            break;
        case SwipeDirection.Up:
            // Handle the swipe
            break;
        case SwipeDirection.Down:
            // Handle the swipe
            break;
    }
}
```

합니다 [ `SwipedEventArgs` ](xref:Xamarin.Forms.SwipedEventArgs) 필요에 따라 안쪽으로 살짝 밀어에 응답 하는 사용자 지정 논리를 사용 하 여를 살짝 밀기 방향을 확인 하기 위해 검사할 수 있습니다. 살짝 밀기 방향을에서 가져올 수 있습니다는 [ `Direction` ](xref:Xamarin.Forms.SwipedEventArgs.Direction) 의 값 중 하나로 설정할 수 있는 이벤트 인수의 속성을 [ `SwipeDirection` ](xref:Xamarin.Forms.SwipeDirection) 열거형입니다. 또한 이벤트 인수는 또한을 [ `Parameter` ](xref:Xamarin.Forms.SwipedEventArgs.Parameter) 속성의 값으로 설정 될 합니다 [ `CommandParameter` ](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) 속성을 정의 하는 경우.

## <a name="using-commands"></a>명령을 사용 하 여

합니다 [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) 클래스도 포함 되어 있습니다 [ `Command` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Command) 고 [ `CommandParameter` ](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) 속성입니다. 이러한 속성은 일반적으로 모델-뷰-ViewModel (MVVM) 패턴을 사용 하는 응용 프로그램에서 사용 됩니다. `Command` 속성 정의 `ICommand` 살짝 밀기 제스처를 인식 되 면 호출 될 사용 하 여는 `CommandParameter` 전달할 개체를 정의 하는 속성을 `ICommand.` 다음 코드 예제에 바인딩하는 방법을 보여 줍니다는 `Command` 속성을 `ICommand` 페이지와 해당 인스턴스를 설정한 보기 모델에 정의 된 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext):

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left, CommandParameter = "Left" };
leftSwipeGesture.SetBinding(SwipeGestureRecognizer.CommandProperty, "SwipeCommand");
boxView.GestureRecognizers.Add(leftSwipeGesture);
```

해당 하는 XAML 코드는 다음과 같습니다.

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Command="{Binding SwipeCommand}" CommandParameter="Left" />
    </BoxView.GestureRecognizers>
</BoxView>
```

`SwipeCommand` 형식의 속성인 `ICommand` 페이지로 설정 된 모델 인스턴스 보기에에서 정의 된 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)합니다. 살짝 밀기 제스처를 인식 하는 경우는 `Execute` 메서드는 `SwipeCommand` 개체 실행 됩니다. 인수는 `Execute` 메서드는 값을 [ `CommandParameter` ](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) 속성입니다. 명령에 대 한 자세한 내용은 참조 하세요. [The 명령 인터페이스](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)합니다.

## <a name="creating-a-swipe-container"></a>안쪽으로 살짝 밀어 컨테이너를 만듭니다.

합니다 `SwipeContainer` 다음 코드 예제에 나와 있는 클래스는 되어 일반화 된 안쪽으로 살짝 밀어 인식 클래스 래핑하는 [ `View` ](xref:Xamarin.Forms.View) 살짝 밀기 제스처를 인식 하는 데:

```csharp
public class SwipeContainer : ContentView
{
    public event EventHandler<SwipedEventArgs> Swipe;

    public SwipeContainer()
    {
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Left));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Right));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Up));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Down));
    }

    SwipeGestureRecognizer GetSwipeGestureRecognizer(SwipeDirection direction)
    {
        var swipe = new SwipeGestureRecognizer { Direction = direction };
        swipe.Swiped += (sender, e) => Swipe?.Invoke(this, e);
        return swipe;
    }
}
```

합니다 `SwipeContainer` 클래스를 만듭니다 [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) 모든 4 개의 안쪽으로 살짝 밀어 방향에 대 한 개체 및 연결 `Swipe` 이벤트 처리기입니다. 이러한 이벤트 처리기를 호출 합니다 `Swipe` 에 정의 된 이벤트는 `SwipeContainer`합니다.

다음 XAML 코드 예제는 `SwipeContainer` 래핑 클래스를 [ `BoxView` ](xref:Xamarin.Forms.BoxView):

```xaml
<ContentPage ...>
    <StackLayout>
        <local:SwipeContainer Swipe="OnSwiped" ...>
            <BoxView Color="Teal" ... />
        </local:SwipeContainer>
    </StackLayout>
</ContentPage>
```

다음 코드 예제에서는 하는 방법을 `SwipeContainer` 래핑하는 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 에 C# 페이지:

```csharp
public class SwipeContainerPageCS : ContentPage
{
    public SwipeContainerPageCS()
    {
        var boxView = new BoxView { Color = Color.Teal, ... };
        var swipeContainer = new SwipeContainer { Content = boxView, ... };
        swipeContainer.Swipe += (sender, e) =>
        {
          // Handle the swipe
        };

        Content = new StackLayout
        {
            Children = { swipeContainer }
        };
    }
}
```

때를 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 살짝 밀기 제스처를 수신 합니다 [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) 이벤트에는 [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) 발생 합니다. 이 작업에서 처리 되는 `SwipeContainer` 자체 발생 하는 클래스 `Swipe` 이벤트입니다. 이 `Swipe` 페이지의 이벤트를 처리 합니다. 합니다 [ `SwipedEventArgs` ](xref:Xamarin.Forms.SwipedEventArgs) 를 살짝 밀기 방향을 필요에 따라 안쪽으로 살짝 밀어에 응답 하는 사용자 지정 논리를 사용 하 여 확인을 검사할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [살짝 밀기 제스처 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/SwipeGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [SwipeGestureRecognizer](xref:Xamarin.Forms.SwipeGestureRecognizer)
