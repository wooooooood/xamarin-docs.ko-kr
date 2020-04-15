---
title: 살짝 밀기 제스처 인식기 추가
description: 이 문서에서는 뷰에서 발생한 살짝 밀기 제스처를 인식하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 164976C2-1429-49FB-9EB6-621E2681C19B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/14/2018
ms.openlocfilehash: ae9b5eb5b768b50ddcbc199040074de855f220de
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "68649451"
---
# <a name="adding-a-swipe-gesture-recognizer"></a>살짝 밀기 제스처 인식기 추가

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture)

_살짝 밀기 제스처는 화면에서 손가락이 가로나 세로 방향으로 이동하는 경우 발생하며 콘텐츠 탐색을 시작하는 데 종종 사용됩니다. 이 문서의 코드 예제는 [살짝 밀기 제스처](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture) 샘플에서 사용됩니다._

[`View`](xref:Xamarin.Forms.View)가 살짝 밀기 제스처를 인식하게 하려면 [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) 인스턴스를 만들고, [`Direction`](xref:Xamarin.Forms.SwipeGestureRecognizer.Direction) 속성을 [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection) 열거형 값(`Left`, `Right`, `Up` 또는 `Down`)으로 설정하고, 필요에 따라 [`Threshold`](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold) 속성을 설정하고, [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) 이벤트를 처리하고, 뷰의 [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) 컬렉션에 제스처 인식기를 새로 추가합니다. 다음 코드 예제에서는 [`BoxView`](xref:Xamarin.Forms.BoxView)에 연결된 `SwipeGestureRecognizer`를 보여줍니다.

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
```

[`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) 클래스에는 살짝 밀기가 인식되려면 디바이스 독립적 단위로 달성되어야 하는 살짝 밀기 최소 거리를 나타내는 `uint` 값으로 필요에 따라 설정될 수 있는 [`Threshold`](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold) 속성도 포함됩니다. 이 속성의 기본값은 100이며, 이는 디바이스 독립적 단위가 100개 미만인 모든 살짝 밀기는 무시된다는 의미입니다.

## <a name="recognizing-the-swipe-direction"></a>살짝 밀기 방향 인식

위의 예에서 [`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction) 속성은 [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection) 열거형의 단일 값으로 설정됩니다. 그러나 이 속성을 `SwipeDirection` 열거형의 여러 값으로 설정할 수도 있으므로 [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) 이벤트는 살짝 밀기에 대한 응답에서 하나를 초과하는 방향으로 발생됩니다. 그러나 제약 조건에 따르면 단일 [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer)는 동일한 축에서 발생하는 살짝 밀기만 인식할 수 있습니다. 따라서 가로 축에서 발행하는 살짝 밀기는 `Direction` 속성을 `Left` 및 `Right`로 설정하여 인식할 수 있습니다.

```xaml
<SwipeGestureRecognizer Direction="Left,Right" Swiped="OnSwiped"/>
```

마찬가지로 세로 축에서 발행하는 살짝 밀기는 [`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction) 속성을 `Up` 및 `Down`으로 설정하여 인식할 수 있습니다.

```csharp
var swipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up | SwipeDirection.Down };
```

또는 각 살짝 밀기 방향에 대한 [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer)를 만들어 모든 방향의 살짝 밀기를 인식할 수 있습니다.

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

해당하는 C# 코드는 다음과 같습니다.

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
> 위의 예제에서 동일한 이벤트 처리기는 [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) 이벤트 발생에 대해 응답합니다. 그러나 각 [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) 인스턴스는 필요한 경우 다른 이벤트 처리기를 사용할 수 있습니다.

## <a name="responding-to-the-swipe"></a>살짝 밀기에 대한 응답

[`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) 이벤트에 대한 이벤트 처리기는 다음 예제에 표시됩니다.

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

필요에 따라 살짝 밀기에 응답하는 사용자 지정 논리를 사용하여 살짝 밀기 방향을 확인하려면 [`SwipedEventArgs`](xref:Xamarin.Forms.SwipedEventArgs)를 검토할 수 있습니다. 살짝 밀기 방향은 [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection) 열거형의 값 중 하나로 설정될 이벤트 인수의 [`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction) 속성에서 가져올 수 있습니다. 또한 이벤트 인수에는 정의되는 경우 [`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) 속성의 값으로 설정될 [`Parameter`](xref:Xamarin.Forms.SwipedEventArgs.Parameter) 속성도 있습니다.

## <a name="using-commands"></a>명령 사용

[`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) 클래스에도 [`Command`](xref:Xamarin.Forms.SwipeGestureRecognizer.Command) 및 [`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) 속성이 포함됩니다. 이러한 속성은 일반적으로 MVVM(Model-View-ViewModel) 패턴을 사용하는 애플리케이션에서 사용됩니다. `Command` 속성은 개체가 `ICommand.`에 전달되도록 정의하는 `CommandParameter` 속성을 사용하여 살짝 밀기 제스처를 인식하는 경우 `ICommand`가 호출되도록 정의합니다. 다음 코드 예제에서는 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 페이지로 설정된 인스턴스의 뷰 모델에서 정의된 `ICommand`에 `Command` 속성을 바인딩하는 방법을 보여줍니다.

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left, CommandParameter = "Left" };
leftSwipeGesture.SetBinding(SwipeGestureRecognizer.CommandProperty, "SwipeCommand");
boxView.GestureRecognizers.Add(leftSwipeGesture);
```

해당 XAML 코드는 다음과 같습니다.

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Command="{Binding SwipeCommand}" CommandParameter="Left" />
    </BoxView.GestureRecognizers>
</BoxView>
```

`SwipeCommand`는 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 페이지로 설정된 뷰 모델 인스턴스에서 정의된 `ICommand` 형식의 속성입니다. 살짝 밀기 제스처를 인식하는 경우 `SwipeCommand` 개체의 `Execute` 메서드가 실행됩니다. `Execute` 메서드에 대한 인수는 [`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) 속성 값입니다. 명령에 대한 자세한 내용은 [명령 인터페이스](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)를 참조하세요.

## <a name="creating-a-swipe-container"></a>살짝 밀기 컨테이너 만들기

다음 코드 예제에 표시된 `SwipeContainer` 클래스는 생성된 살짝 밀기 인식 클래스로서 살짝 밀기 제스처를 인식하려면 [`View`](xref:Xamarin.Forms.View)를 래핑합니다.

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

`SwipeContainer` 클래스는 살짝 밀기 방향 4개 모두에 대해 [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) 개체를 만들고 `Swipe` 이벤트 처리기를 연결합니다. 이러한 이벤트 처리기는 `SwipeContainer`에서 정의된 `Swipe` 이벤트를 호출합니다.

다음 XAML 코드 예제에서는 [`BoxView`](xref:Xamarin.Forms.BoxView)를 래핑하는 `SwipeContainer` 클래스를 보여줍니다.

```xaml
<ContentPage ...>
    <StackLayout>
        <local:SwipeContainer Swipe="OnSwiped" ...>
            <BoxView Color="Teal" ... />
        </local:SwipeContainer>
    </StackLayout>
</ContentPage>
```

다음 코드 예제에서는 `SwipeContainer`가 C# 페이지에서 [`BoxView`](xref:Xamarin.Forms.BoxView)를 래핑하는 방법을 보여줍니다.

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

[`BoxView`](xref:Xamarin.Forms.BoxView)가 살짝 밀기 제스처를 수신하는 경우 [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) 이벤트가 [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer)에서 발생합니다. 이 작업은 자체 `Swipe` 이벤트를 발생시키는 `SwipeContainer` 클래스에서 처리됩니다. 이 `Swipe` 이벤트는 페이지에서 처리됩니다. 그런 다음, 필요에 따라 살짝 밀기에 응답하는 사용자 지정 논리를 사용하여 살짝 밀기 방향을 확인하려면 [`SwipedEventArgs`](xref:Xamarin.Forms.SwipedEventArgs)를 검토할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [살짝 밀기 제스처(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [SwipeGestureRecognizer](xref:Xamarin.Forms.SwipeGestureRecognizer)
