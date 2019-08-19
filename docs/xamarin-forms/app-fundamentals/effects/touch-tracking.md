---
title: 효과로부터 이벤트 호출
description: 효과는 기본 네이티브 뷰의 변경을 신호로 알리는 이벤트를 정의하고 호출할 수 있습니다. 이 문서에서는 하위 수준 멀티 터치 손가락 추적을 구현하는 방법과 터치 동작을 신호로 알려주는 이벤트를 생성하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2018
ms.openlocfilehash: 94a10213f8ae42d6e8f3407b18051021d92be5bc
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68978557"
---
# <a name="invoking-events-from-effects"></a>효과로부터 이벤트 호출

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)

_효과는 기본 네이티브 뷰의 변경을 신호로 알리는 이벤트를 정의하고 호출할 수 있습니다. 이 문서에서는 하위 수준 멀티 터치 손가락 추적을 구현하는 방법과 터치 동작을 신호로 알려주는 이벤트를 생성하는 방법을 보여줍니다._

이 문서에 설명된 효과는 하위 수준 터치 이벤트에 대한 액세스를 제공합니다. 이러한 하위 수준 이벤트는 기존 `GestureRecognizer` 클래스를 통해 사용할 수는 없지만 일부 유형의 애플리케이션에는 핵심적인 요소입니다. 예를 들어 손가락 페인팅 애플리케이션은 화면에서 각각의 손가락이 움직이는 것을 추적해야 합니다. 음악 키보드는 각각의 키를 눌렀다 떼는 것뿐만 아니라 글리산도 주법으로 한 키에서 다른 키로 손가락이 미끄러지는 것도 감지해야 합니다.

효과는 원하는 Xamarin.Forms 요소에 연결할 수 있기 때문에 멀티 터치 손가락 추적에 이상적입니다.

## <a name="platform-touch-events"></a>플랫폼 터치 이벤트

iOS, Android 및 유니버설 Windows 플랫폼에는 애플리케이션으로 터치 동작을 감지할 수 있는 하위 수준 API가 모두 포함되어 있습니다. 이러한 모든 플랫폼에서는 세 가지 기본적인 유형의 터치 이벤트가 구별됩니다.

- *Pressed* - 손가락이 화면을 터치하는 경우
- *Moved* - 화면을 터치하는 손가락이 이동하는 경우
- *Released* - 손가락을 화면에서 떼는 경우

멀티 터치 환경에서는 여러 개의 손가락으로 동시에 화면을 터치할 수 있습니다. 애플리케이션에서 여러 손가락을 구별하는 데 사용할 수 있는 ID(식별) 번호가 다양한 플랫폼에 포함되어 있습니다.

iOS의 경우 `UIView` 클래스가 이 세 가지 기본적인 이벤트에 해당하는 재정의가 가능한 세 가지 메서드인 `TouchesBegan`, `TouchesMoved` 및 `TouchesEnded`를 정의합니다. [ 멀티 터치 손가락 추적](~/ios/app-fundamentals/touch/touch-tracking.md) 문서에 이러한 메서드를 사용하는 방법이 설명되어 있습니다. 단 iOS 프로그램에서는 이러한 메서드를 사용하기 위해 `UIView`에서 파생되는 클래스를 재정의할 필요가 없습니다. 또한 iOS `UIGestureRecognizer`에서는 동일한 세 가지 메서드가 정의되기 때문에 `UIGestureRecognizer`에서 파생되는 클래스의 인스턴스를 원하는 `UIView` 개체에 연결할 수 있습니다.

Android의 경우 `View` 클래스에서 재정의가 가능한 메서드인 `OnTouchEvent`가 정의되어 모든 터치 동작이 처리됩니다. 터치 동작의 유형은 열거형 멤버인 `Down`, `PointerDown`, `Move`, `Up` 및 `PointerUp`으로 정의되며 [멀티 터치 손가락 추적](~/android/app-fundamentals/touch/touch-tracking.md) 문서에 설명되어 있습니다. Android `View` 역시 원하는 `View` 개체에 이벤트 처리기를 연결할 수 있는 `Touch` 이벤트를 정의합니다.

UWP(유니버설 Windows 플랫폼)에서는 `UIElement` 클래스가 `PointerPressed`, `PointerMoved` 및 `PointerReleased` 이벤트를 정의합니다. 이 내용은 [MSDN의 포인터 입력 처리](/windows/uwp/input-and-devices/handle-pointer-input/) 문서와 [`UIElement`](/uwp/api/windows.ui.xaml.uielement/) 클래스에 대한 API 설명서에 설명되어 있습니다.

유니버설 Windows 플랫폼의 `Pointer` API는 마우스, 터치 및 펜 입력을 통합하기 위한 API입니다. 따라서 `PointerMoved` 이벤트는 마우스 단추를 누르지 않더라도 마우스가 요소 사이를 이동하면 호출됩니다. 이러한 이벤트를 동반하는 `PointerRoutedEventArgs` 개체에는 마우스 단추를 눌렀는지 또는 손가락이 화면을 접촉했는지 알려주는 `IsInContact` 속성이 포함된 `Pointer` 속성이 있습니다.

또한 UWP는 `PointerEntered` 및 `PointerExited`라는 두 가지 이벤트를 더 정의합니다. 이 이벤트는 마우스나 손가락이 한 가지 요소에서 다른 요소로 이동하는 것을 알려줍니다. 예를 들어 A와 B라는 두 개의 인접한 요소가 있고, 두 요소 모두에 포인터 이벤트에 대한 처리기가 설치되어 있습니다. 손가락이 A를 누르면 `PointerPressed` 이벤트가 호출됩니다. 손가락을 이동하면 A가 `PointerMoved` 이벤트를 호출합니다. 손가락이 A에서 B로 이동하면 A는 `PointerExited` 이벤트를 호출하고 B는 `PointerEntered` 이벤트를 호출합니다. 그런 다음, 손가락을 떼면 B가 `PointerReleased` 이벤트를 호출합니다.

iOS 및 Android 플랫폼은 UWP와 다릅니다. 손가락이 보기를 터치할 때 `TouchesBegan`이나 `OnTouchEvent`에 대한 호출을 먼저 받는 보기는 손가락이 다른 보기로 이동하더라도 계속해서 모든 터치 동작을 받습니다. 애플리케이션이 포인터를 캡처하면 UWP도 유사하게 작동할 수 있습니다. `PointerEntered` 이벤트 처리기에서 요소는 `CapturePointer`를 호출한 다음, 해당 손가락의 모든 터치 동작을 받습니다.

UWP 방식은 음악 키보드와 같은 유형의 일부 애플리케이션에 매우 유용한 것으로 입증되었습니다. 각 키는 이러한 키에 대한 터치 이벤트를 처리할 수 있으며 `PointerEntered`와 `PointerExited` 이벤트를 사용하여 손가락이 한 키에서 다른 키로 미끄러지는 경우를 감지합니다.

이런 이유 때문에 이 문서에 설명된 터치 추적 효과를 통해 UWP 방식이 구현됩니다.

## <a name="the-touch-tracking-effect-api"></a>터치 추적 효과 API

[**터치 추적 효과 데모**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/) 샘플에는 하위 수준 터치 추적을 구현하는 클래스(및 열거형)가 포함됩니다. 이러한 형식은 `TouchTracking` 네임스페이스에 속하며 `Touch`라는 단어로 시작됩니다. **TouchTrackingEffectDemos** .NET Standard 라이브러리 프로젝트에는 터치 이벤트 유형에 대한 `TouchActionType` 열거형이 포함됩니다.

```csharp
public enum TouchActionType
{
    Entered,
    Pressed,
    Moved,
    Released,
    Exited,
    Cancelled
}
```

모든 플랫폼에는 터치 이벤트가 취소된 것을 나타내는 이벤트도 포함됩니다.

.NET Standard 라이브러리의 `TouchEffect` 클래스는 `RoutingEffect`에서 파생되고 `TouchAction` 이벤트를 호출하는 `TouchAction`이라는 이벤트와 `OnTouchAction`이라는 메서드를 정의합니다.

```csharp
public class TouchEffect : RoutingEffect
{
    public event TouchActionEventHandler TouchAction;

    public TouchEffect() : base("XamarinDocs.TouchEffect")
    {
    }

    public bool Capture { set; get; }

    public void OnTouchAction(Element element, TouchActionEventArgs args)
    {
        TouchAction?.Invoke(element, args);
    }
}
```

`Capture` 속성도 확인하십시오. 터치 이벤트를 캡처하려면 애플리케이션에서 `Pressed` 이벤트 이전에 이 속성을 `true`로 설정해야 합니다. 그렇지 않으면 터치 이벤트가 유니버설 Windows 플랫폼에 있는 이벤트와 동일하게 작동합니다.

.NET Standard 라이브러리의 `TouchActionEventArgs` 클래스에는 각 이벤트와 함께 제공되는 모든 정보가 포함됩니다.

```csharp
public class TouchActionEventArgs : EventArgs
{
    public TouchActionEventArgs(long id, TouchActionType type, Point location, bool isInContact)
    {
        Id = id;
        Type = type;
        Location = location;
        IsInContact = isInContact;
    }

    public long Id { private set; get; }

    public TouchActionType Type { private set; get; }

    public Point Location { private set; get; }

    public bool IsInContact { private set; get; }
}
```

애플리케이션에서 `Id` 속성을 사용하여 개별 손가락을 추적할 수 있습니다. `IsInContact` 속성을 확인하십시오. 이 속성은 `Pressed` 이벤트에는 항상 `true`이고 `Released` 이벤트에는 `false`입니다. iOS 및 Android의 `Moved` 이벤트에 대해서도 항상 `true`입니다. `IsInContact` 속성은 프로그램이 데스크톱에서 실행되고 마우스 단추를 누르지 않고 포인터를 이동하는 경우 유니버설 Windows 플랫폼의 `Moved` 이벤트에 대해 `false`일 수도 있습니다.

솔루션의 .NET Standard 라이브러리 프로젝트에 파일을 포함시키고 원하는 Xamarin.Forms 요소의 `Effects` 컬렉션에 인스턴스를 추가하면 자체 애플리케이션에 `TouchEffect` 클래스를 사용할 수 있습니다. `TouchAction` 이벤트에 처리기를 연결하여 터치 이벤트를 가져옵니다.

자체 애플리케이션에 `TouchEffect`를 사용하려면 **TouchTrackingEffectDemos** 솔루션에 포함된 플랫폼 구현도 필요합니다.

## <a name="the-touch-tracking-effect-implementations"></a>터치 추적 효과 구현

`TouchEffect`의 iOS, Android 및 UWP 구현은 가장 간단한 구현(UWP)부터 다른 구현보다 구조적으로 복잡한 iOS 구현까지 아래에 설명되어 있습니다.

### <a name="the-uwp-implementation"></a>UWP 구현

`TouchEffect`의 UWP 구현이 가장 간단합니다. 일반적으로 클래스는 `PlatformEffect`에서 파생되고 두 가지 어셈블리을 포함합니다.

```csharp
[assembly: ResolutionGroupName("XamarinDocs")]
[assembly: ExportEffect(typeof(TouchTracking.UWP.TouchEffect), "TouchEffect")]

namespace TouchTracking.UWP
{
    public class TouchEffect : PlatformEffect
    {
        ...
    }
}
```

`OnAttached` 재정의는 일부 정보를 필드로 저장하고 처리기를 모든 포인터 이벤트에 연결합니다.

```csharp
public class TouchEffect : PlatformEffect
{
    FrameworkElement frameworkElement;
    TouchTracking.TouchEffect effect;
    Action<Element, TouchActionEventArgs> onTouchAction;

    protected override void OnAttached()
    {
        // Get the Windows FrameworkElement corresponding to the Element that the effect is attached to
        frameworkElement = Control == null ? Container : Control;

        // Get access to the TouchEffect class in the .NET Standard library
        effect = (TouchTracking.TouchEffect)Element.Effects.
                    FirstOrDefault(e => e is TouchTracking.TouchEffect);

        if (effect != null && frameworkElement != null)
        {
            // Save the method to call on touch events
            onTouchAction = effect.OnTouchAction;

            // Set event handlers on FrameworkElement
            frameworkElement.PointerEntered += OnPointerEntered;
            frameworkElement.PointerPressed += OnPointerPressed;
            frameworkElement.PointerMoved += OnPointerMoved;
            frameworkElement.PointerReleased += OnPointerReleased;
            frameworkElement.PointerExited += OnPointerExited;
            frameworkElement.PointerCanceled += OnPointerCancelled;
        }
    }
    ...
}    
```

`OnPointerPressed` 처리기는 `CommonHandler` 메서드에서 `onTouchAction` 필드를 호출하여 효과 이벤트를 호출합니다.

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerPressed(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Pressed, args);

        // Check setting of Capture property
        if (effect.Capture)
        {
            (sender as FrameworkElement).CapturePointer(args.Pointer);
        }
    }
    ...
    void CommonHandler(object sender, TouchActionType touchActionType, PointerRoutedEventArgs args)
    {
        PointerPoint pointerPoint = args.GetCurrentPoint(sender as UIElement);
        Windows.Foundation.Point windowsPoint = pointerPoint.Position;  

        onTouchAction(Element, new TouchActionEventArgs(args.Pointer.PointerId,
                                                        touchActionType,
                                                        new Point(windowsPoint.X, windowsPoint.Y),
                                                        args.Pointer.IsInContact));
    }
}
```

`OnPointerPressed`는 .NET Standard 라이브러리의 효과 클래스에 있는 `Capture` 속성 값을 확인하고 `true`이면 `CapturePointer`를 호출합니다.

 다른 UWP 이벤트 처리기는 훨씬 더 간단합니다.

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerEntered(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Entered, args);
    }
    ...
}
```

### <a name="the-android-implementation"></a>Android 구현

Android와 iOS 구현은 손가락이 한 요소에서 다른 요소로 이동할 때 `Exited` 및 `Entered` 이벤트를 구현해야 하므로 당연히 더 복잡합니다. 두 가지 구현 모두 구조가 비슷합니다.

Android `TouchEffect` 클래스는 `Touch` 이벤트에 대한 처리기를 설치합니다.

```csharp
view = Control == null ? Container : Control;
...
view.Touch += OnTouch;
```

이 클래스는 두 가지 고정적인 사전도 정의합니다.

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    static Dictionary<Android.Views.View, TouchEffect> viewDictionary =
        new Dictionary<Android.Views.View, TouchEffect>();

    static Dictionary<int, TouchEffect> idToEffectDictionary =
        new Dictionary<int, TouchEffect>();
    ...
```

`viewDictionary`는 `OnAttached` 재정의가 호출될 때마다 새 항목을 가져옵니다.

```csharp
viewDictionary.Add(view, this);
```

항목이 `OnDetached`의 사전에서 제거됩니다. `TouchEffect`의 모든 인스턴스는 효과가 연결되어 있는 특정 보기와 연결됩니다. 정적 사전을 사용하면 모든 `TouchEffect` 인스턴스가 다른 모든 보기 및 해당 `TouchEffect` 인스턴스를 통해 열거될 수 있습니다. 이 기능은 한 보기에서 다른 보기로 이벤트를 전송하는 데 필요합니다.

Android는 애플리케이션이 각 손가락을 추적할 수 있도록 터치 이벤트에 ID 코드를 할당합니다. `idToEffectDictionary`는 이 ID 코드를 `TouchEffect` 인스턴스와 연결합니다. 손가락 누름에 대해 `Touch` 처리기가 호출되면 이 사전에 항목이 추가됩니다.

```csharp
void OnTouch(object sender, Android.Views.View.TouchEventArgs args)
{
    ...
    switch (args.Event.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:
            FireEvent(this, id, TouchActionType.Pressed, screenPointerCoords, true);

            idToEffectDictionary.Add(id, this);

            capture = libTouchEffect.Capture;
            break;

```

화면에서 손가락을 떼면 `idToEffectDictionary`에서 항목이 제거됩니다. `FireEvent` 메서드는 `OnTouchAction` 메서드를 호출하는 데 필요한 모든 정보를 단지 누적시킵니다.

```csharp
void FireEvent(TouchEffect touchEffect, int id, TouchActionType actionType, Point pointerLocation, bool isInContact)
{
    // Get the method to call for firing events
    Action<Element, TouchActionEventArgs> onTouchAction = touchEffect.libTouchEffect.OnTouchAction;

    // Get the location of the pointer within the view
    touchEffect.view.GetLocationOnScreen(twoIntArray);
    double x = pointerLocation.X - twoIntArray[0];
    double y = pointerLocation.Y - twoIntArray[1];
    Point point = new Point(fromPixels(x), fromPixels(y));

    // Call the method
    onTouchAction(touchEffect.formsElement,
        new TouchActionEventArgs(id, actionType, point, isInContact));
}
```

다른 모든 터치 유형은 두 가지 다른 방식으로 처리됩니다. `Capture` 속성이 `true`이면 터치 이벤트는 `TouchEffect` 정보로 매우 간단한 전환입니다. `Capture`가 `false`이면 더 복잡해집니다. 터치 이벤트가 한 보기에서 다른 보기로 이동해야 할 수도 있기 때문입니다. 이것은 이동 이벤트 중에 호출되는 `CheckForBoundaryHop` 메서드가 담당합니다. 이 메서드는 고정적인 사전을 활용합니다. `viewDictionary`를 통해 열거되고 손가락이 현재 터치하고 있는 보기를 확인하고 `idToEffectDictionary`를 사용하여 특정 ID와 연결된 현재 `TouchEffect` 인스턴스(및 현재 보기)를 저장합니다.

```csharp
void CheckForBoundaryHop(int id, Point pointerLocation)
{
    TouchEffect touchEffectHit = null;

    foreach (Android.Views.View view in viewDictionary.Keys)
    {
        // Get the view rectangle
        try
        {
            view.GetLocationOnScreen(twoIntArray);
        }
        catch // System.ObjectDisposedException: Cannot access a disposed object.
        {
            continue;
        }
        Rectangle viewRect = new Rectangle(twoIntArray[0], twoIntArray[1], view.Width, view.Height);

        if (viewRect.Contains(pointerLocation))
        {
            touchEffectHit = viewDictionary[view];
        }
    }

    if (touchEffectHit != idToEffectDictionary[id])
    {
        if (idToEffectDictionary[id] != null)
        {
            FireEvent(idToEffectDictionary[id], id, TouchActionType.Exited, pointerLocation, true);
        }
        if (touchEffectHit != null)
        {
            FireEvent(touchEffectHit, id, TouchActionType.Entered, pointerLocation, true);
        }
        idToEffectDictionary[id] = touchEffectHit;
    }
}
```

`idToEffectDictionary`가 변경되면 메서드는 `Exited` 및 `Entered`에 대해 잠재적으로 `FireEvent`를 호출하여 한 보기에서 다른 보기로 전송합니다. 하지만 손가락이 `TouchEffect`가 연결되지 않은 보기가 차지하는 영역으로 이동했거나 이 영역에서 효과가 연결되어 있는 보기로 이동했을 수도 있습니다.

에 액세스 할 때 `try`와 `catch` 블록을 확인합니다. 이동된 페이지에서 홈 페이지로 다시 돌아가면 `OnDetached` 메서드가 호출되지 않고 항목에 `viewDictionary`에 유지되지만 Android에서는 처리된 것으로 간주됩니다.

### <a name="the-ios-implementation"></a>iOS 구현

iOS 구현은 Android 구현과 유사하지만 iOS `TouchEffect` 클래스가 `UIGestureRecognizer`의 파생 클래스를 인스턴스화해야 한다는 점만 다릅니다. 이 클래스는 iOS 프로젝트에 있는 `TouchRecognizer`라는 클래스입니다. 이 클래스는 `TouchRecognizer` 인스턴스를 저장하는 두 개의 고정적인 사전을 유지합니다.

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

이 `TouchRecognizer` 클래스 구조의 많은 부분이 Android `TouchEffect` 클래스와 유사합니다.

> [!IMPORTANT]
> `UIKit`의 많은 보기에는 기본적으로 터치가 사용되지 않습니다. iOS 프로젝트에서 `TouchEffect` 클래스의 `OnAttached` 재정의에 `view.UserInteractionEnabled = true;`를 추가하여 터치를 사용하도록 설정할 수 있습니다. 이 작업은 효과가 연결된 요소에 해당하는 `UIView`를 가져온 후에 수행해야 합니다.

## <a name="putting-the-touch-effect-to-work"></a>터치 효과 작업

[**TouchTrackingEffectDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/) 프로그램에는 일반적인 작업에 대한 터치 추적 효과를 테스트하는 페이지가 5개 있습니다.

**BoxView Dragging**(BoxView 드래그) 페이지에서는 `AbsoluteLayout`에 `BoxView` 요소를 추가한 다음, 화면에서 드래그할 수 있습니다. [XAML 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml)은 `BoxView` 요소를 `AbsoluteLayout`에 추가하고 `AbsoluteLayout`을 지우는 두 개의 `Button` 보기를 인스턴스화합니다.

새 `BoxView`를 `AbsoluteLayout`에 추가하는 [코드 숨김 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml.cs)에 있는 메서드도 `BoxView`에 `TouchEffect` 개체를 추가하고 효과에 이벤트 처리기를 연결합니다.

```csharp
void AddBoxViewToLayout()
{
    BoxView boxView = new BoxView
    {
        WidthRequest = 100,
        HeightRequest = 100,
        Color = new Color(random.NextDouble(),
                          random.NextDouble(),
                          random.NextDouble())
    };

    TouchEffect touchEffect = new TouchEffect();
    touchEffect.TouchAction += OnTouchEffectAction;
    boxView.Effects.Add(touchEffect);
    absoluteLayout.Children.Add(boxView);
}
```

`TouchAction` 이벤트 처리기는 모든 `BoxView` 요소의 모든 터치 이벤트를 처리하지만 몇 가지 주의할 점이 있습니다. 프로그램이 끌기만 구현하기 때문에 `BoxView` 하나에 손가락 두 개가 허용되지 않고 두 손가락이 서로 방해가 됩니다. 따라서 페이지는 현재 추적 중인 각 손가락에 대해 포함된 클래스를 정의합니다.

```csharp
class DragInfo
{
    public DragInfo(long id, Point pressPoint)
    {
        Id = id;
        PressPoint = pressPoint;
    }

    public long Id { private set; get; }

    public Point PressPoint { private set; get; }
}

Dictionary<BoxView, DragInfo> dragDictionary = new Dictionary<BoxView, DragInfo>();
```

`dragDictionary`에는 현재 드래그하고 있는 모든 `BoxView`에 대한 항목이 포함됩니다.

`Pressed` 터치 동작은 이 사전에 항목을 추가하고 `Released` 동작은 이것을 제거합니다. `Pressed` 논리는 해당 `BoxView`에 대한 사전에 항목이 이미 있는지 확인해야 합니다. 그렇다면 `BoxView`를 이미 드래그 중이고 새 이벤트는 동일한 `BoxView`의 두 번째 손가락입니다. `Moved` 및 `Released` 동작의 경우, 사전에 해당 `BoxView`에 대한 항목이 있는지 여부와 드래그한 `BoxView`에 대한 터치 `Id` 속성이 사전 항목에 있는 속성과 일치하는지를 이벤트 처리기가 확인해야 합니다.

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    BoxView boxView = sender as BoxView;

    switch (args.Type)
    {
        case TouchActionType.Pressed:
            // Don't allow a second touch on an already touched BoxView
            if (!dragDictionary.ContainsKey(boxView))
            {
                dragDictionary.Add(boxView, new DragInfo(args.Id, args.Location));

                // Set Capture property to true
                TouchEffect touchEffect = (TouchEffect)boxView.Effects.FirstOrDefault(e => e is TouchEffect);
                touchEffect.Capture = true;
            }
            break;

        case TouchActionType.Moved:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                Rectangle rect = AbsoluteLayout.GetLayoutBounds(boxView);
                Point initialLocation = dragDictionary[boxView].PressPoint;
                rect.X += args.Location.X - initialLocation.X;
                rect.Y += args.Location.Y - initialLocation.Y;
                AbsoluteLayout.SetLayoutBounds(boxView, rect);
            }
            break;

        case TouchActionType.Released:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                dragDictionary.Remove(boxView);
            }
            break;
    }
}
```

`Pressed` 논리는 `TouchEffect` 개체의 `Capture` 속성을 `true`로 설정합니다. 이렇게 하면 이 손가락에 대한 모든 후속 이벤트가 동일한 이벤트 처리기로 제공되는 효과가 있습니다.

`Moved` 논리는 `LayoutBounds`이 연결된 속성을 변경하여 `BoxView`를 이동합니다. 이벤트 인수의 `Location` 속성은 항상 드래그되는 `BoxView`를 기준으로 하며 `BoxView`가 일정한 속도로 드래그되는 경우에는 연속 이벤트의 `Location` 속성은 거의 동일합니다. 예를 들어 손가락이 `BoxView`의 가운데를 누르면 `Pressed` 동작이 `PressPoint` 속성을 (50, 50)으로 저장하고 후속 이벤트에서 동일하게 유지됩니다. `BoxView`를 대각선 방향으로 일정한 속도로 드래그하면 `Moved` 동작을 수행하는 동안 후속 `Location` 속성 값이 (55, 55)가 되고 이런 경우 `Moved` 논리가 `BoxView`의 가로 및 세로 위치에 5를 추가합니다. 그러면 가운데가 손가락 바로 아래에 다시 오도록 `BoxView`가 이동됩니다.

다른 손가락을 사용하여 여러 `BoxView` 요소를 동시에 옮길 수 있습니다.

[![](touch-tracking-images/boxviewdragging-small.png "BoxView 드래그 페이지의 삼중 스크린샷")](touch-tracking-images/boxviewdragging-large.png#lightbox "BoxView 드래그 페이지의 삼중 스크린샷")

### <a name="subclassing-the-view"></a>보기 서브클래스 지정

Xamarin.Forms 요소가 자체 터치 이벤트를 처리하는 것이 더 쉬운 경우가 많습니다. **Draggable BoxView Dragging**(드래그 가능 BoxView 드래그) 페이지는 **BoxView Dragging**(BoxView 드래그) 페이지와 동일하게 작동하지만 사용자가 드래그하는 요소는 `BoxView`에서 파생된 [`DraggableBoxView`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/DraggableBoxView.cs) 클래스의 인스턴스입니다.

```csharp
class DraggableBoxView : BoxView
{
    bool isBeingDragged;
    long touchId;
    Point pressPoint;

    public DraggableBoxView()
    {
        TouchEffect touchEffect = new TouchEffect
        {
            Capture = true
        };
        touchEffect.TouchAction += OnTouchEffectAction;
        Effects.Add(touchEffect);
    }

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!isBeingDragged)
                {
                    isBeingDragged = true;
                    touchId = args.Id;
                    pressPoint = args.Location;
                }
                break;

            case TouchActionType.Moved:
                if (isBeingDragged && touchId == args.Id)
                {
                    TranslationX += args.Location.X - pressPoint.X;
                    TranslationY += args.Location.Y - pressPoint.Y;
                }
                break;

            case TouchActionType.Released:
                if (isBeingDragged && touchId == args.Id)
                {
                    isBeingDragged = false;
                }
                break;
        }
    }
}
```

생성자는 `TouchEffect`를 만들어서 연결하고 이 개체가 처음 인스턴스화될 때 `Capture` 속성을 설정합니다. 클래스 자체가 각 손가락과 관련된 `isBeingDragged`, `pressPoint` 및 `touchId` 값을 저장하기 때문에 사전이 필요하지 않습니다. `Moved` 처리는 `TranslationX` 및 `TranslationY` 속성을 변경하기 때문에 `DraggableBoxView`의 부모가 `AbsoluteLayout`이 아니어도 논리는 작동합니다.

### <a name="integrating-with-skiasharp"></a>SkiaSharp와 통합

다음 두 가지 데모에는 그래픽이 필요하며 이 용도를 위해 SkiaSharp가 사용됩니다. 이 예제를 학습하기 전에 [Xamarin.Forms에서 SkiaSharp 사용](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)에 대해 알아보는 것이 좋습니다. 여기에서 필요한 모든 내용이 처음 두 문서("SkiaSharp 그리기 기본 사항" 및 "SkiaSharp 선 및 경로")에 포함되어 있습니다.

**Ellipse Drawing**(타원 그리기) 페이지에서는 화면에서 손가락을 밀어서 타원을 그릴 수 있습니다. 손가락을 움직이는 방식에 따라 왼쪽 위에서 오른쪽 아래로 타원을 그리거나 다른 쪽 모서리에서 반대편 모서리로 타원을 그릴 수 있습니다. 타원은 임의 색상과 불투명도로 그려집니다.

[![](touch-tracking-images/ellipsedrawing-small.png "타원 그리기 페이지의 삼중 스크린샷")](touch-tracking-images/ellipsedrawing-large.png#lightbox "타원 그리기 페이지의 삼중 스크린샷")

타원 중 하나를 터치한 다음, 다른 위치로 끌 수 있습니다. 이렇게 하려면 특정 지점에서 그래픽 개체를 검색하는 기능과 관련된 "hit-testing"(적중 테스트)이라는 기술이 필요합니다. SkiaSharp 타원은 Xamarin.Forms 요소가 아니기 때문에 자체적인 `TouchEffect` 프로세싱을 수행할 수 없습니다. `TouchEffect`가 전체 `SKCanvasView` 개체에 적용되어야 합니다.

[EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml) 파일은 단일 셀 `Grid`에서 `SKCanvasView`를 인스턴스화합니다. `TouchEffect` 개체는 이 `Grid`에 연결되어 있습니다.

```xaml
<Grid x:Name="canvasViewGrid"
        Grid.Row="1"
        BackgroundColor="White">

    <skia:SKCanvasView x:Name="canvasView"
                        PaintSurface="OnCanvasViewPaintSurface" />
    <Grid.Effects>
        <tt:TouchEffect Capture="True"
                        TouchAction="OnTouchEffectAction" />
    </Grid.Effects>
</Grid>
```

Android와 유니버설 Windows 플랫폼에서는 `TouchEffect`를 `SKCanvasView`에 직접 연결할 수 있지만 iOS에서는 이렇게 할 수 없습니다. `Capture` 속성이 `true`로 설정되는 것에 유의합니다.

SkiaSharp에서 렌더링되는 각 타원은 `EllipseDrawingFigure` 유형의 개체로 표현됩니다.

```csharp
class EllipseDrawingFigure
{
    SKPoint pt1, pt2;

    public EllipseDrawingFigure()
    {
    }

    public SKColor Color { set; get; }

    public SKPoint StartPoint
    {
        set
        {
            pt1 = value;
            MakeRectangle();
        }
    }

    public SKPoint EndPoint
    {
        set
        {
            pt2 = value;
            MakeRectangle();
        }
    }

    void MakeRectangle()
    {
        Rectangle = new SKRect(pt1.X, pt1.Y, pt2.X, pt2.Y).Standardized;
    }

    public SKRect Rectangle { set; get; }

    // For dragging operations
    public Point LastFingerLocation { set; get; }

    // For the dragging hit-test
    public bool IsInEllipse(SKPoint pt)
    {
        SKRect rect = Rectangle;

        return (Math.Pow(pt.X - rect.MidX, 2) / Math.Pow(rect.Width / 2, 2) +
                Math.Pow(pt.Y - rect.MidY, 2) / Math.Pow(rect.Height / 2, 2)) < 1;
    }
}
```

`StartPoint` 및 `EndPoint` 속성은 프로그램이 터치 입력을 처리할 때 사용되며 `Rectangle` 속성은 타원을 그리는 데 사용됩니다. `LastFingerLocation` 속성은 타원이 드래그될 때 작동하고 `IsInEllipse` 메서드는 hit-testing(적중 테스트)을 지원합니다. 포인트가 타원 안에 있으면 메서드는 `true`를 반환합니다.

[코드 숨김 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml.cs)에는 세 가지 컬렉션이 유지됩니다.

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

`draggingFigure` 사전은 `completedFigures` 컬렉션 하위 집합을 포함합니다. SkiaSharp `PaintSurface` 이벤트 처리기는 `completedFigures` 및 `inProgressFigures` 컬렉션에 있는 개체를 렌더링합니다.

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Fill
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (EllipseDrawingFigure figure in completedFigures)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
    foreach (EllipseDrawingFigure figure in inProgressFigures.Values)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
}
```

터치 프로세싱의 가장 까다로운 부분은 `Pressed` 처리입니다. 여기에서 hit-testing(적중 테스트)이 수행되지만 코드가 사용자의 손가락 아래에서 타원을 감지하면 타원이 현재 다른 손가락으로 드래그되지 않는 경우에만 드래그가 가능합니다. 사용자의 손가락 아래에 타원이 없으면 코드는 새 타원을 그리는 프로세스를 시작합니다.

```csharp
case TouchActionType.Pressed:
    bool isDragOperation = false;

    // Loop through the completed figures
    foreach (EllipseDrawingFigure fig in completedFigures.Reverse<EllipseDrawingFigure>())
    {
        // Check if the finger is touching one of the ellipses
        if (fig.IsInEllipse(ConvertToPixel(args.Location)))
        {
            // Tentatively assume this is a dragging operation
            isDragOperation = true;

            // Loop through all the figures currently being dragged
            foreach (EllipseDrawingFigure draggedFigure in draggingFigures.Values)
            {
                // If there's a match, we'll need to dig deeper
                if (fig == draggedFigure)
                {
                    isDragOperation = false;
                    break;
                }
            }

            if (isDragOperation)
            {
                fig.LastFingerLocation = args.Location;
                draggingFigures.Add(args.Id, fig);
                break;
            }
        }
    }

    if (isDragOperation)
    {
        // Move the dragged ellipse to the end of completedFigures so it's drawn on top
        EllipseDrawingFigure fig = draggingFigures[args.Id];
        completedFigures.Remove(fig);
        completedFigures.Add(fig);
    }
    else // start making a new ellipse
    {
        // Random bytes for random color
        byte[] buffer = new byte[4];
        random.NextBytes(buffer);

        EllipseDrawingFigure figure = new EllipseDrawingFigure
        {
            Color = new SKColor(buffer[0], buffer[1], buffer[2], buffer[3]),
            StartPoint = ConvertToPixel(args.Location),
            EndPoint = ConvertToPixel(args.Location)
        };
        inProgressFigures.Add(args.Id, figure);
    }
    canvasView.InvalidateSurface();
    break;
```

다른 SkiaSharp 예제는 **Finger Paint**(손가락 페인팅) 페이지입니다. 두 가지 `Picker` 보기에서 스트로크 색상과 스트로크 너비를 선택한 다음, 하나 이상의 손가락으로 그릴 수 있습니다.

[![](touch-tracking-images/fingerpaint-small.png "손가락 페인팅 페이지의 삼중 스크린샷")](touch-tracking-images/fingerpaint-large.png#lightbox "손가락 페인팅 페이지의 삼중 스크린샷")

이 예제에는 화면에 그려진 각 선을 나타낼 별도의 클래스도 필요합니다.

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new SKPath();
    }

    public SKPath Path { set; get; }

    public Color StrokeColor { set; get; }

    public float StrokeWidth { set; get; }
}
```

`SKPath` 개체는 각 줄을 렌더링하는 데 사용됩니다. [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/FingerPaintPage.xaml.cs) 파일은 이러한 개체의 두 가지 컬렉션을 유지하며, 한 가지는 현재 그려지는 폴리라인, 나머지는 완료된 폴리라인용입니다.

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

`Pressed` 처리는 새 `FingerPaintPolyline`을 만들고 초기 지점을 저장하기 위해 개체 경로에서 `MoveTo`를 호출하고 이 개체를 `inProgressPolylines` 사전에 추가합니다. `Moved` 처리는 새 손가락 위치로 경로 개체에서 `LineTo`를 호출하고 `Released` 처리는 `inProgressPolylines`에서 `completedPolylines`로 완료된 폴리라인을 전송합니다. 이번에도 SkiaSharp 그리기 코드는 비교적 간단합니다.

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    StrokeCap = SKStrokeCap.Round,
    StrokeJoin = SKStrokeJoin.Round
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (FingerPaintPolyline polyline in completedPolylines)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }

    foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }
}
```

### <a name="tracking-view-to-view-touch"></a>보기 간 터치 추적

앞의 모든 예제에서 `TouchEffect`가 만들어지거나 `Pressed` 이벤트가 발생하는 경우 `TouchEffect`의 `Capture` 속성을 `true`로 설정했습니다. 이렇게 하면 보기를 처음 누르는 손가락과 연결된 모든 이벤트를 동일한 요소가 수신할 수 있습니다. 최종 샘플은 `Capture`를 `true`로 설정하지 않습니다.  이로 인해 화면에 접촉하는 손가락이 한 요소에서 다른 요소로 이동할 때 다른 동작이 발생합니다. 손가락을 뗀 요소는 `Type` 속성이 `TouchActionType.Exited`로 설정된 이벤트를 수신하고 두 번째 요소는 `Type`이 `TouchActionType.Entered`로 설정된 이벤트를 수신합니다.

이런 유형의 터치 처리는 음악 키보드에 매우 유용합니다. 키는 눌렸을 때는 물론 손가락이 키보드 사이를 미끄러질 때도 이를 감지할 수 있어야 합니다.

**Silent Keyboard**(무음 키보드) 페이지는 `BoxView`에서 파생된 [`Key`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/Key.cs)에서 파생된 작은 [`WhiteKey`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/WhiteKey.cs) 및 [`BlackKey`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BlackKey.cs) 클래스를 정의합니다.

`Key` 클래스는 실제 음악 프로그램에 사용할 준비가 되었습니다. 이것은 `IsPressed` 및 `KeyNumber`라는 공용 속성을 정의하며, 이 속성은 MIDI 표준에 의해 설정된 키 코드로 설정됩니다. `Key` 클래스는 `StatusChanged`라는 이벤트도 정의합니다. 이것은 `IsPressed` 속성이 변경되면 호출됩니다.

각 키에는 여러 손가락이 허용됩니다. 이런 이유 때문에 `Key` 클래스에는 현재 키를 터치하고 있는 모든 손가락의 터치 ID 번호 `List`가 유지됩니다.

```csharp
List<long> ids = new List<long>();
```

`TouchAction` 이벤트 처리기는 `Pressed` 이벤트 유형과 `Entered` 유형 모두에 대한 `ids` 목록에 ID를 추가합니다. 단, `Entered` 이벤트에 대한 `IsInContact` 속성이 `true`인 경우에만 추가됩니다. ID는 `Released` 또는 `Exited` 이벤트의 `List`에서는 ID가 제거됩니다.

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    switch (args.Type)
    {
      case TouchActionType.Pressed:
          AddToList(args.Id);
          break;

        case TouchActionType.Entered:
            if (args.IsInContact)
            {
                AddToList(args.Id);
            }
            break;

        case TouchActionType.Moved:
            break;

        case TouchActionType.Released:
        case TouchActionType.Exited:
            RemoveFromList(args.Id);
            break;
    }
}

```

`AddToList`와 `RemoveFromList` 메서드 모두 `List`가 비어 있거나 비어 있지 않은 상태로 변경되었는지를 확인하고 변경된 경우에는 `StatusChanged` 이벤트를 호출합니다.

다양한 `WhiteKey`와 `BlackKey` 요소가 페이지의 [XAML 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/SilentKeyboardPage.xaml)에서 정렬되며, 이 상태는 폰을 가로 모드로 들었을 때 가장 잘 나타납니다.

[![](touch-tracking-images/silentkeyboard-small.png "무음 키보드 페이지의 삼중 스크린샷")](touch-tracking-images/silentkeyboard-large.png#lightbox "무음 키보드 페이지의 삼중 스크린샷")

키 사이로 손가락이 지나가면 터치 이벤트가 한 키에서 다른 키로 전송되는 약간의 변화가 색상으로 나타납니다.

## <a name="summary"></a>요약

이 문서에서는 효과에서 이벤트를 호출하는 방법과 하위 수준 멀티 터치 처리를 구현하는 효과를 작성하고 사용하는 방법을 살펴보았습니다.


## <a name="related-links"></a>관련 링크

- [iOS에서 멀티 터치 손가락 추적](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Android에서 멀티 터치 손가락 추적](~/android/app-fundamentals/touch/touch-tracking.md)
- [터치 추적 효과(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)
