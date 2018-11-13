---
title: 효과의 이벤트를 호출합니다.
description: 효과 정의 하 고 기본 네이티브 보기에 대 한 변경 내용을 신호 이벤트를 호출할 수 있습니다. 이 문서를 추적 하는 하위 수준 멀티 터치 손가락을 구현 하는 방법 및 터치 작업을 보이는 이벤트를 생성 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 0a5e2c1a7a7807da91fd98e617467ea251a25bc0
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/11/2018
ms.locfileid: "51527406"
---
# <a name="invoking-events-from-effects"></a>효과의 이벤트를 호출합니다.

_효과 정의 하 고 기본 네이티브 보기에 대 한 변경 내용을 신호 이벤트를 호출할 수 있습니다. 이 문서를 추적 하는 하위 수준 멀티 터치 손가락을 구현 하는 방법 및 터치 작업을 보이는 이벤트를 생성 하는 방법을 보여 줍니다._

이 문서에 설명 된 효과 낮은 수준의 터치 이벤트에 대 한 액세스를 제공 합니다. 이러한 하위 수준 이벤트를 기존를 통해 사용할 수 없는 `GestureRecognizer` 있지만 클래스는 일부 유형의 응용 프로그램에 중요 합니다. 예를 들어를 finger-paint 응용 프로그램 화면에서 이동할 때 각 손가락을 추적 해야 합니다. 음악 키보드 탭 검색 해야 하 고 하나의 키에서 다른 한 glissando gliding 손가락 뿐만 아니라 개별 키를 해제 합니다.

효과는 손가락 멀티 터치 추적 Xamarin.Forms 요소에 연결할 수 있으므로 적합 합니다.

## <a name="platform-touch-events"></a>플랫폼 터치 이벤트

IOS, Android 및 유니버설 Windows 플랫폼 응용 프로그램을 검색 하는 하위 수준 API를 포함 하는 모든 활동을 터치 합니다. 이러한 플랫폼 모두 세 가지 기본 유형의 구분 터치 이벤트:

- *누른*손가락 화면을 터치 하는 경우
- *이동*여 화면을 터치 손가락을 움직이면,
- *출시*손가락을 화면에서 해제 될 때,

다중 터치 환경에서 여러 손가락 동시에 화면을 터치 수 있습니다. 다양 한 플랫폼 응용 프로그램은 여러 손가락을 구분 하는 데 사용할 수 있는 id (ID) 번호를 포함 합니다.

IOS에는 `UIView` 세 개의 재정의 가능한 메서드를 정의 하는 클래스 `TouchesBegan`, `TouchesMoved`, 및 `TouchesEnded` 이러한 세 가지 기본 이벤트에 해당 합니다. 이 문서 [멀티 터치 손가락 추적](~/ios/app-fundamentals/touch/touch-tracking.md) 이러한 메서드를 사용 하는 방법에 설명 합니다. 그러나 iOS 프로그램 않아도에서 파생 된 클래스 재정의 `UIView` 이러한 메서드를 사용 하도록 합니다. IOS `UIGestureRecognizer` 도 정의 같은 세 가지 메서드 및 있습니다에서 파생 된 클래스의 인스턴스를 연결할 수 있습니다 `UIGestureRecognizer` 에 `UIView` 개체입니다.

Android에서 합니다 `View` 클래스 라는 재정의 가능한 메서드를 정의 합니다. `OnTouchEvent` 모든 터치 작업을 처리 합니다. 터치 활동의 type을가 열거형 멤버를 정의한 `Down`, `PointerDown`를 `Move`, `Up`, 및 `PointerUp` 문서에 설명 된 대로 [멀티 터치 손가락 추적](~/android/app-fundamentals/touch/touch-tracking.md)합니다. Android `View` 라는 이벤트가 정의 `Touch` 이벤트 처리기에 연결할 수 있도록 `View` 개체입니다.

Windows 플랫폼 (UWP (유니버설), 합니다 `UIElement` 명명 된 이벤트를 정의 하는 클래스 `PointerPressed`, `PointerMoved`, 및 `PointerReleased`합니다. 문서에 설명 된 이러한 [MSDN에 대 한 포인터 입력 처리 문서](/windows/uwp/input-and-devices/handle-pointer-input/) 및에 대 한 API 설명서를 [ `UIElement` ](/uwp/api/windows.ui.xaml.uielement/) 클래스.

`Pointer` 유니버설 Windows 플랫폼에서 API 마우스, 터치, 펜 입력 통합 됩니다. 이런 이유로 `PointerMoved` 이벤트는 마우스 단추를 누르지 않은 경우에 요소에서 마우스를 이동할 때 호출 됩니다. `PointerRoutedEventArgs` 이러한 이벤트와 함께 제공 되는 개체에 라는 속성이 `Pointer` 라는 속성이 있는 `IsInContact` 마우스 단추가 눌러져 나 손가락 화면 접촉 되어 있는 경우 나타냅니다.

UWP 라는 두 개의 자세한 이벤트를 정의 하는 또한 `PointerEntered` 고 `PointerExited`입니다. 이러한 손가락이 나 마우스 이동 하면 요소에서를 다른 것을 나타냅니다. 예를 들어 A와 B 라는 두 개의 인접 한 요소 두 요소 모두 포인터 이벤트에 대 한 처리기를 설치 했습니다. 손가락 a를 누르면는 `PointerPressed` 이벤트가 호출 됩니다. 호출 하는 손가락 이동 `PointerMoved` 이벤트입니다. 손가락 A에서 B로 이동 하는 경우는 호출을 `PointerExited` 이벤트와 B를 호출 하는 `PointerEntered` 이벤트입니다. B를 호출 하는 손가락이 출시 후에 `PointerReleased` 이벤트입니다.

IOS 및 Android 플랫폼은 UWP 다릅니다: 뷰를 먼저 호출을 가져옵니다 `TouchesBegan` 또는 `OnTouchEvent` 손가락 터치 보기 손가락 다른 뷰로 이동 하는 경우에 모든 터치 작업을 자동으로 가져오려고 계속 합니다. UWP 응용 프로그램에 포인터를 캡처하는 경우 마찬가지로 동작할 수 있습니다:에 `PointerEntered` 이벤트 처리기를 호출 하 여 요소 `CapturePointer` 한 다음 해당 손가락에서 모든 터치 작업을 가져옵니다.

UWP 접근 방식은 일부 유형의 응용 프로그램, 예를 들어, 음악 키보드에 대 한 매우 유용 하 게 증명 합니다. 각 키에서 해당 키에 대 한 터치 이벤트를 처리 하 고 손가락을 사용 하 여 다른 하나의 키에서 슬라이드에 하는 경우를 검색할 수는 `PointerEntered` 고 `PointerExited` 이벤트입니다.

따라서가이 문서에 설명 된 터치 추적 효과 UWP 접근 방식을 구현 합니다.

## <a name="the-touch-tracking-effect-api"></a>터치 추적 효과 API

합니다 [ **추적 효과 데모 Touch** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) 클래스 (및 열거형) 낮은 수준의 터치 추적을 구현 하는 샘플에 포함 되어 있습니다. 이러한 형식은 네임 스페이스에 속하는 `TouchTracking` 단어로 시작 하 고 `Touch`입니다. **TouchTrackingEffectDemos** .NET Standard 라이브러리 프로젝트에 포함 된 `TouchActionType` 터치 이벤트 유형에 대 한 열거형:

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

모든 플랫폼에는 터치 이벤트가 취소 되었는지 여부를 나타내는 이벤트가 포함 됩니다.

합니다 `TouchEffect` .NET 표준 라이브러리의 클래스에서 파생 됩니다 `RoutingEffect` 이라는 이벤트를 정의 하 고 `TouchAction` 라는 이름의 메서드 `OnTouchAction` 를 호출 하는 `TouchAction` 이벤트:

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

또한는 `Capture` 속성입니다. 터치 이벤트를 캡처하려면 응용 프로그램이이 속성을 설정 해야 `true` 이전에 `Pressed` 이벤트입니다. 그렇지 않으면 터치 이벤트 유니버설 Windows 플랫폼에서와 같이 동작합니다.

`TouchActionEventArgs` .NET 표준 라이브러리의 클래스는 각 이벤트와 함께 제공 되는 모든 정보를 포함 합니다.

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

응용 프로그램이 사용할 수는 `Id` 각 손가락을 추적 하기 위한 속성입니다. 통지를 `IsInContact` 속성입니다. 이 속성은 항상 `true` 에 대 한 `Pressed` 이벤트와 `false` 에 대 한 `Released` 이벤트입니다. 것도 항상 `true` 에 대 한 `Moved` iOS 및 Android에는 이벤트입니다. 합니다 `IsInContact` 속성 수 있습니다 `false` 에 대 한 `Moved` 바탕 화면에서 프로그램이 실행 되 고 단추 없이 마우스 포인터를 이동 하는 경우 유니버설 Windows 플랫폼에서 이벤트 누를 합니다.

사용할 수는 `TouchEffect` 클래스 인스턴스를 추가 하 고 솔루션의.NET Standard 라이브러리 프로젝트에 파일을 포함 하 여 사용자 고유의 응용 프로그램에는 `Effects` Xamarin.Forms 요소의 컬렉션입니다. 처리기를 연결 합니다 `TouchAction` 이벤트 터치 이벤트를 가져오려고 합니다.

사용 하도록 `TouchEffect` 응용 프로그램에 필요에 포함 된 플랫폼 구현 **TouchTrackingEffectDemos** 솔루션입니다.

## <a name="the-touch-tracking-effect-implementations"></a>터치 추적 효과 구현

IOS, Android 및 UWP 구현의 `TouchEffect` 가장 간단한 구현 (UWP)로 시작 하 고 다른 구조적으로 더 복잡 한 이기 때문에 iOS 구현 끝나는 다음과 같습니다.

### <a name="the-uwp-implementation"></a>UWP 구현

UWP 구현의 `TouchEffect` 간단 합니다. 클래스에서 파생 되는 일반적으로 `PlatformEffect` 두 어셈블리 특성을 포함 합니다.

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

`OnAttached` 재정의 모든 포인터 이벤트 필드와 연결 처리기로 몇 가지 정보를 저장 합니다.

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

합니다 `OnPointerPressed` 처리기를 호출 하 여 결과 이벤트를 호출 하는 `onTouchAction` 필드에 `CommonHandler` 메서드:

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

`OnPointerPressed` 값도 확인 합니다 `Capture` .NET Standard 라이브러리를 호출 효과 클래스의 속성에에서 `CapturePointer` 있으면 `true`합니다.

 다른 UWP 이벤트 처리기는 훨씬 더 간단 합니다.

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

Android 및 iOS 구현을 구현 해야 하므로 반드시 더 복잡 합니다 `Exited` 및 `Entered` 손가락을 움직이면 요소에서 간 이벤트입니다. 두 구현 모두 마찬가지로 구조화 되어 있습니다.

Android `TouchEffect` 클래스에 대 한 처리기를 설치 합니다 `Touch` 이벤트:

```csharp
view = Control == null ? Container : Control;
...
view.Touch += OnTouch;
```

클래스에는 두 개의 정적 사전을 정의합니다.

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

`viewDictionary` 될 때마다 새 항목을 가져옵니다는 `OnAttached` 재정의 라고 합니다.

```csharp
viewDictionary.Add(view, this);
```

항목이 사전에서 제거 됩니다 `OnDetached`합니다. 모든 인스턴스의 `TouchEffect` 효과에 연결 된 특정 뷰를 사용 하 여 연결 합니다. 모든 정적 사전 허용 `TouchEffect` 인스턴스를 다른 모든 보기 및 해당 열거 `TouchEffect` 인스턴스. 이 다른 어떤 보기에서 이벤트를 전송 하는 데 필요 합니다.

Android 터치 이벤트를 각 손가락을 추적 하는 응용 프로그램을 허용 하는 ID 코드를 할당 합니다. 합니다 `idToEffectDictionary` 사용 하 여이 ID 코드는 연결을 `TouchEffect` 인스턴스. 이 사전에 항목이 추가 될 때를 `Touch` 손가락 press 처리기가 호출:

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

항목이 제거 되는 `idToEffectDictionary` 화면에서 손가락을 해제할 때. 합니다 `FireEvent` 메서드를 호출 하는 데 필요한 모든 정보를 간단히 누적 된 `OnTouchAction` 메서드:

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

다른 모든 터치 형식의 두 가지 방법으로 처리 됩니다: 경우 합니다 `Capture` 속성은 `true`, 터치 이벤트를 매우 간단한 변환이 `TouchEffect` 정보입니다. 가져와서 더 복잡 한 경우 `Capture` 는 `false` 터치 이벤트를 다른 보기로 이동할 할 수 있으므로 합니다. 책임입니다는 `CheckForBoundaryHop` 메서드를 이동 이벤트 동안 호출 됩니다. 이 메서드는 모두 정적 사전을 사용 합니다. 열거 하는 `viewDictionary` 손가락이 닿는 현재를 사용 하는 뷰를 확인 하려면 `idToEffectDictionary` 현재 스토어로 `TouchEffect` 인스턴스 (및 따라서 현재 보기) 특정 ID와 연결 된:

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

변경 된 경우는 `idToEffectDictionary`, 잠재적으로 호출 `FireEvent` 에 대 한 `Exited` 및 `Entered` 다른 하나의 뷰에서 전송할 합니다. 그러나 손가락 수로 옮겨지면 연결 하지 않고 뷰를 차지 하는 영역 `TouchEffect`, 또는 연결 된 효과 사용 하 여 뷰에 해당 영역에서 합니다.

표시 된 `try` 및 `catch` 보기에 액세스 하는 경우 차단 합니다. 탐색 하는 다음에 홈 페이지로 다시 이동 하는 페이지에는 `OnDetached` 메서드가 호출 되지 않습니다 및 항목 유지는 `viewDictionary` Android에서는 삭제 하지만 합니다.

### <a name="the-ios-implementation"></a>IOS 구현

IOS 구현은 제외 하 고 Android 구현에 비슷합니다. iOS `TouchEffect` 클래스의 파생 클래스를 인스턴스화해야 합니다. `UIGestureRecognizer`합니다. 이 iOS 프로젝트에서 클래스 `TouchRecognizer`합니다. 이 클래스를 저장 하는 두 개의 정적 사전을 유지 `TouchRecognizer` 인스턴스:

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

이 구조의 많이 `TouchRecognizer` 클래스는 Android 비슷합니다 `TouchEffect` 클래스입니다.

## <a name="putting-the-touch-effect-to-work"></a>터치 효과 작업에 투입

합니다 [ **TouchTrackingEffectDemos** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) 프로그램에는 일반적인 작업에 대 한 터치 추적 결과 테스트 하는 5 개 페이지가 있습니다.

합니다 **BoxView 끌어** 페이지에 추가할 수 있습니다 `BoxView` 요소를 사용 하는 `AbsoluteLayout` 다음 화면에서 이리저리 드래그 합니다. [XAML 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml) 두 개를 인스턴스화하고 `Button` 뷰 추가 대 한 `BoxView` 요소를 사용 하는 `AbsoluteLayout` 선택을 취소 하 고는 `AbsoluteLayout`합니다.

메서드는 [코드 숨김 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml.cs) 더하는 새 `BoxView` 에 `AbsoluteLayout` 추가 `TouchEffect` 개체를 `BoxView` 효과 대 한 이벤트 처리기를 연결 하 고:

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

합니다 `TouchAction` 모두에 대 한 모든 터치 이벤트를 처리 하는 이벤트 처리기는 `BoxView` 요소인 하지만 몇 가지 주의 해야: 단일 두 손가락으로 허용 될 수 없습니다 `BoxView` 프로그램만 끌어, 구현 및 두 손가락으로 하기 때문에 서로 방해 합니다. 따라서 페이지 현재 추적 중인 각 손가락 마다 포함 된 클래스를 정의 합니다.

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

합니다 `dragDictionary` 에 대 한 항목이 모든 `BoxView` 현재 끌고 있는 합니다.

합니다 `Pressed` 터치가 사전에 항목 추가 및 `Released` 작업 제거 합니다. 합니다 `Pressed` 논리에 대 한 사전에 항목이 이미 인지 확인 해야 `BoxView`합니다. 그렇다면 합니다 `BoxView` 끄는 이미이 고 새 이벤트는 두 번째 손가락으로에 동일한 `BoxView`. 에 대 한 합니다 `Moved` 하 고 `Released` 작업, 이벤트 처리기를 확인 해야 합니다 사전에 대 한 항목이 있는 경우 `BoxView` 하 고 터치 `Id` 끈 속성 `BoxView` 사전 항목의 일치:

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

`Pressed` 논리 집합을 `Capture` 의 속성을 `TouchEffect` 개체를 `true`. 이 손가락에 대 한 후속 이벤트를 모두 동일한 이벤트 처리기를 제공 하는 효과가 있습니다.

합니다 `Moved` 논리 이동 합니다 `BoxView` 변경 하 여는 `LayoutBounds` 연결 된 속성입니다. `Location` 기준으로 이벤트 인수의 속성은 항상를 `BoxView` 끌고 있는 경우에 `BoxView` 일정 한 속도로 끌고를 `Location` 연속 이벤트의 속성은 동일 약 합니다. 예를 들어, 손가락이 키를 누를 경우는 `BoxView` 의 중심에는 `Pressed` 작업 저장소를 `PressPoint` 속성 (50, 50)는 후속 이벤트에 대 한 동일 하 게 유지 합니다. 경우는 `BoxView` 후속 상수 속도로 대각선 방향으로 끌 `Location` 속성 중 합니다 `Moved` 작업의 값일 수 있습니다 (55, 55) 경우는 `Moved` 합니다 의가로및세로위치에5를추가하는논리`BoxView`. 이동이 `BoxView` 중심 다시 되도록 손가락 바로 아래입니다.

여러 이동할 수 있습니다 `BoxView` 동시에 다른 손가락을 사용 하 여 요소입니다.

[![](touch-tracking-images/boxviewdragging-small.png "삼중 BoxView 끌어 페이지 스크린샷")](touch-tracking-images/boxviewdragging-large.png#lightbox "삼중 BoxView 끌어 페이지 스크린샷")

### <a name="subclassing-the-view"></a>뷰를 서브클래싱합니다.

종종 자체 터치 이벤트를 처리 하는 Xamarin.Forms 요소에 대 한 쉽습니다. **Draggable BoxView 끌어** 페이지에 동일 하 게 작동 합니다 **BoxView 끌어** 페이지 있지만 사용자 끌기의 인스턴스인 요소를 [ `DraggableBoxView` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/DraggableBoxView.cs) 파생 된 클래스 `BoxView`:

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

생성자를 만들고 연결 합니다 `TouchEffect`, 설정 및는 `Capture` 해당 개체를 처음으로 인스턴스화될 때 속성입니다. 클래스 자체를 저장 하므로 사전이 없습니다 필수임 `isBeingDragged`, `pressPoint`, 및 `touchId` 각 손가락 연관 된 값입니다. `Moved` 처리 변경 합니다 `TranslationX` 및 `TranslationY` 논리 작동 속성 경우에의 부모를 `DraggableBoxView` 아닙니다는 `AbsoluteLayout`.

### <a name="integrating-with-skiasharp"></a>SkiaSharp 통합

다음 두 데모 그래픽 하며 SkiaSharp이이 목적을 위해 사용 합니다. 에 대해 자세히 알아보려면는 것이 좋습니다 [Xamarin.Forms에서 SkiaSharp 사용 하 여](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) 전에 이러한 예제를 연구 합니다. 처음 두 문서 ("SkiaSharp 그리기 기본 사항" 및 "SkiaSharp 선 및 경로")를 가린다는 여기 해야 합니다.

합니다 **타원 그리기** 페이지를 사용 하면 화면에서 손가락을 밀어서 타원을 그릴 수 있습니다. 손가락을 이동 하는 방법에 따라, 반대쪽 모퉁이 있는 모든 다른 모서리 또는 왼쪽-오른쪽, 아래에서에서 타원을 그릴 수 있습니다. 타원 임의 색상과 불투명도 사용 하 여 그려집니다.

[![](touch-tracking-images/ellipsedrawing-small.png "타원 그리기 페이지 스크린샷 삼중")](touch-tracking-images/ellipsedrawing-large.png#lightbox "삼중 타원 그리기 페이지 스크린샷")

줄임표 중 다음 터치 할 경우 다른 위치로 끌 수 있습니다. "적중 테스트," 특정 지점에서 그래픽 개체에 대 한 검색을 포함 하는 기술이 필요 합니다. SkiaSharp 줄임표 없는 Xamarin.Forms 요소 자체를 수행할 수 없습니다 있도록 `TouchEffect` 처리 합니다. 합니다 `TouchEffect` 전체에 적용 해야 `SKCanvasView` 개체입니다.

합니다 [EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml) 파일은 합니다 `SKCanvasView` 단일 셀에서 `Grid`합니다. 합니다 `TouchEffect` 개체는 연결 된 `Grid`:

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

Android 및 유니버설 Windows 플랫폼에는 `TouchEffect` 에 직접 연결할 수 있습니다는 `SKCanvasView`, 있지만 작동 하지 않는 경우 iOS에서 합니다. 다음에 유의 합니다 `Capture` 속성이 `true`.

SkiaSharp 렌더링 하는 각 타원은 형식의 개체를 표현 `EllipseDrawingFigure`:

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

`StartPoint` 및 `EndPoint` 프로그램은 터치 입력을 처리할 때 사용 되는 속성을 `Rectangle` 속성 타원 그리기에 사용 됩니다. `LastFingerLocation` 타원을 끌어 놓으면 속성 고려해 야 하며 `IsInEllipse` 적중 테스트 메서드를 지원 합니다. 메서드는 반환 `true` ellipse 안쪽 지점이 있으면 됩니다.

합니다 [코드 숨김 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml.cs) 세 컬렉션을 유지 합니다.

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

합니다 `draggingFigure` 의 하위 집합을 포함 하는 사전은 `completedFigures` 컬렉션입니다. SkiaSharp `PaintSurface` 이벤트 처리기에서 이러한 개체를 렌더링 합니다 `completedFigures` 고 `inProgressFigures` 컬렉션:

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

터치 처리의 가장 까다로운 부분은 `Pressed` 처리 합니다. 이 경우 적중 테스트를 수행 하지만 코드에서 사용자의 손가락 아래 타원을 발견 하면 해당 타원만 끌어 놓을 수 있습니다 다른 손가락으로 현재 끌고 있는 경우. 사용자의 손가락 아래 없습니다 타원의 경우 코드를 새 타원 그리기 프로세스를 시작 합니다.

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

다른 SkiaSharp 예제는 **손가락으로 그리기** 페이지입니다. 두 스트로크 색과 스트로크 너비를 선택할 수 있습니다 `Picker` 뷰 및 다음 하나 이상의 손가락으로 그립니다.

[![](touch-tracking-images/fingerpaint-small.png "손가락으로 그리기 페이지 스크린샷 삼중")](touch-tracking-images/fingerpaint-large.png#lightbox "삼중 손가락으로 그리기 페이지 스크린샷")

이 예제에는 화면에 그릴 각 줄을 나타내는 별도 클래스를 필요 합니다.

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

`SKPath` 개체는 각 줄을 렌더링에 사용 됩니다. 합니다 [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/FingerPaintPage.xaml.cs) 파일에 이러한 개체, 그리고 현재 해당 폴리라인에 하나 및 완료 된 폴리라인으로 대 한 다른 두 컬렉션은 유지 관리 합니다.

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

`Pressed` 처리를 만듭니다 `FingerPaintPolyline`를 호출 `MoveTo` 초기 지점에 저장 하는 경로 개체에 해당 개체를 추가 하 고는 `inProgressPolylines` 사전입니다. `Moved` 호출을 처리 `LineTo` 새 손가락 위치를 사용 하 여 경로 개체에 및 `Released` 처리 전송에서 완료 된 다중선 `inProgressPolylines` 에 `completedPolylines`입니다. 이번에 실제 SkiaSharp 그리기 코드는 비교적 간단 합니다.

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

### <a name="tracking-view-to-view-touch"></a>뷰를 터치 추적

앞의 모든 예제 설정를 `Capture` 의 속성을 `TouchEffect` 를 `true`때를 `TouchEffect` 만든 때나는 `Pressed` 이벤트가 발생 한 합니다. 동일한 요소 먼저 뷰를 누르는 손가락을 사용 하 여 연결 된 모든 이벤트를 수신 하는 것이 이렇게 합니다. 최종 샘플 *되지* 설정 `Capture` 하려면 `true`합니다. 그러면 손가락을 움직이면 화면 닿는 요소에서 간 다르게 동작 합니다. 사용 하 여 이벤트를 수신 하는 요소에서 손가락 이동 하는 `Type` 속성이로 설정 `TouchActionType.Exited` 사용 하 여 이벤트를 수신 하는 두 번째 요소와는 `Type` 설정 `TouchActionType.Entered`합니다.

이 유형의 터치 처리 음악 키보드에 대 한 매우 유용합니다. 키를 누를 때에 다른 하나의 키에서 손가락을 움직일 슬라이드 하는 경우를 검색할 수 있어야 합니다.

**자동 키보드** 페이지에서는 소규모 [ `WhiteKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/WhiteKey.cs) 하 고 [ `BlackKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BlackKey.cs) 에서 파생 된 클래스 [ `Key` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/Key.cs)에서 파생 되는 `BoxView`합니다.

`Key` 클래스는 실제 음악 프로그램에서 사용할 수 있습니다. 라는 공용 속성 정의 `IsPressed` 및 `KeyNumber`, MIDI 표준에 따라 설정 키 코드에 설정 되었습니다. `Key` 클래스도 이라는 이벤트를 정의 `StatusChanged`를 호출할 때를 `IsPressed` 속성 변경 합니다.

여러 손가락 각 키에서 허용 됩니다. 이러한 이유로 합니다 `Key` 클래스를 유지 관리를 `List` 현재 해당 키를 변경 하는 모든 손가락 터치 ID 번호:

```csharp
List<long> ids = new List<long>();
```

`TouchAction` 에 ID를 추가 하는 이벤트 처리기는 `ids` 둘 다에 대 한 목록을 `Pressed` 이벤트 유형 및 `Entered` 형식이 아니라 경우에만 `IsInContact` 속성은 `true` 에 대 한는 `Entered` 이벤트. ID에서 제거 되는 `List` 에 대 한는 `Released` 또는 `Exited` 이벤트:

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

`AddToList` 하 고 `RemoveFromList` 메서드는 모두 확인 하는 경우를 `List` 비어 있음과 비어 간에 변경 된를 호출 하 고는 `StatusChanged` 이벤트.

다양 한 `WhiteKey` 하 고 `BlackKey` 페이지에서 요소 정렬 [XAML 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/SilentKeyboardPage.xaml), 전화를 가로 모드로 유지 되는 경우 좋은:

[![](touch-tracking-images/silentkeyboard-small.png "자동 키보드 페이지 스크린샷 삼중")](touch-tracking-images/silentkeyboard-large.png#lightbox "삼중 자동 키보드 페이지 스크린샷")

키에서 손가락을 스윕, 보면 색에서 약간 변경 하 여 다른 하나의 키에서 터치 이벤트 전송 됩니다.

## <a name="summary"></a>요약

이 문서를 적용에서 하는 이벤트를 호출 하는 방법 및 쓰기 하위 수준 멀티 터치 처리를 구현 하는 효과 사용 하는 방법을 살펴보았습니다.


## <a name="related-links"></a>관련 링크

- [IOS에서 다중 터치 손가락 추적](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Android에서 추적 하는 멀티 터치 손가락](~/android/app-fundamentals/touch/touch-tracking.md)
- [터치 추적 효과 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)
