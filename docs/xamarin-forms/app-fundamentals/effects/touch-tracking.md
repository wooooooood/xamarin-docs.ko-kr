---
title: "효과의 이벤트를 호출합니다."
description: "효과 정의 하 고 기본 네이티브 보기에서 변경 됨을 나타내는 이벤트를 호출 수입니다. 이 문서에는 추적, 낮은 수준의 멀티 터치 손가락을 구현 하는 방법 및 터치 작업을 알리는 이벤트를 생성 하는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 89d3b56a15110d0c106c43ce227f11f86bbdf404
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="invoking-events-from-effects"></a>효과의 이벤트를 호출합니다.

_효과 정의 하 고 기본 네이티브 보기에서 변경 됨을 나타내는 이벤트를 호출 수입니다. 이 문서에는 추적, 낮은 수준의 멀티 터치 손가락을 구현 하는 방법 및 터치 작업을 알리는 이벤트를 생성 하는 방법을 보여 줍니다._

이 문서에서 설명 하는 효과 낮은 수준의 터치 이벤트에 대 한 액세스를 제공 합니다. 이러한 하위 수준 이벤트를 기존를 통해 사용할 수 없는 `GestureRecognizer` 클래스, 있지만 몇 가지 종류의 응용 프로그램에 중요 한 됩니다. 예를 들어 한 finger-paint 응용 프로그램 화면에서 이동 하는 동안 개별 손가락을 추적 해야 합니다. 음악 키보드 탭 검색 하 고는 glissando에서 다른 키가 두 개에서 gliding 손가락 뿐만 아니라 개별 키를 해제 합니다.

효과 멀티 터치 손가락 추적 Xamarin.Forms 요소에 첨부 때문에 적합 합니다.

## <a name="platform-touch-events"></a>플랫폼 터치 이벤트

IOS, Android 및 유니버설 Windows 플랫폼 응용 프로그램을 검색 하는 낮은 수준의 API를 포함 하는 모든 활동을 터치 합니다. 이러한 플랫폼의 세 가지 기본 유형은 구분 모든 터치 이벤트:

- *누른*손가락 화면을 터치 하는 경우
- *이동*는 화면을 터치 손가락을 움직이면,
- *출시*화면에서 손가락 해제 될 때,

여러 손가락 멀티 터치 환경에서 동시에 화면을 접할 수 있습니다. 다양 한 플랫폼 응용 프로그램을 여러 손가락 서로 구분 하는 데 사용할 수 있는 id (ID) 번호를 포함 합니다.

IOS의 경우에 `UIView` 세 가지 재정의 가능한 메서드를 정의 하는 클래스 `TouchesBegan`, `TouchesMoved`, 및 `TouchesEnded` 이러한 세 가지 기본 이벤트에 해당 합니다. 문서 [멀티 터치 손가락 추적](~/ios/app-fundamentals/touch/touch-tracking.md) 이러한 메서드를 사용 하는 방법을 설명 합니다. 그러나에서 파생 된 클래스를 재정의 하는 iOS 프로그램 필요 하지 않습니다 `UIView` 이러한 메서드를 사용 하도록 합니다. IOS `UIGestureRecognizer` 도 정의 같은 세 개의 메서드와 있습니다에서 파생 되는 클래스의 인스턴스를 연결할 수 `UIGestureRecognizer` 모든 `UIView` 개체입니다.

Android에서는 `View` 라는 재정의 가능한 메서드를 정의 하는 클래스 `OnTouchEvent` 모든 터치 작업을 처리할 수 있습니다. 터치 활동의 형식 열거형 멤버에 의해 정의 되 `Down`, `PointerDown`, `Move`, `Up`, 및 `PointerUp` 문서에 설명 된 대로 [멀티 터치 손가락 추적](~/android/app-fundamentals/touch/touch-tracking.md)합니다. Android `View` 라는 이벤트 정의 `Touch` 이벤트 처리기에 연결 될 수 있는 `View` 개체입니다.

에 플랫폼 UWP (유니버설 Windows)는 `UIElement` 명명 된 이벤트를 정의 하는 클래스 `PointerPressed`, `PointerMoved`, 및 `PointerReleased`합니다. 문서에 설명 되어 [MSDN에서 포인터 입력 처리 문서](/windows/uwp/input-and-devices/handle-pointer-input/) 및에 대 한 API 설명서는 [ `UIElement` ](/uwp/api/windows.ui.xaml.uielement/) 클래스입니다.

`Pointer` 유니버설 Windows 플랫폼에서 API 마우스, 터치 및 펜 입력을 통합 하기 위한 용도가 있습니다. 이런 이유로 `PointerMoved` 이벤트는 마우스 단추를 누르지 않은 경우에 요소에서 마우스를 이동할 때 호출 됩니다. `PointerRoutedEventArgs` 이러한 이벤트를 함께 제공 되는 개체에 라는 속성이 `Pointer` 라는 속성이 있는 `IsInContact` 마우스 단추를 누르면 나 손가락 화면 접촉 되어 있는 경우 나타냅니다.

UWP 라는 두 개 더 많은 이벤트를 정의 하는 또한 `PointerEntered` 및 `PointerExited`합니다. 이 경우 마우스 또는 손가락은 요소에서를 다른 위치로 이동 나타냅니다. 예를 들어 A와 B 라는 두 개의 인접 한 요소 두 요소에 대 한 포인터 이벤트 처리기를 설치 해야 합니다. A에서 손가락으로를 누르면는 `PointerPressed` 이벤트 호출 됩니다. 손가락 이동할 때 A 호출 `PointerMoved` 이벤트입니다. 손가락 A에서 B로 이동 하는 경우 A을 호출 하는 `PointerExited` 이벤트와 B를 호출는 `PointerEntered` 이벤트입니다. 다음 손가락 릴리스될 때마다 경우 B을 호출 하는 `PointerReleased` 이벤트입니다.

IOS 및 Android 플랫폼은 UWP 다릅니다: 보기에 대 한 호출을 처음 가져오는 `TouchesBegan` 또는 `OnTouchEvent` 손가락 터치 보기 손가락 다른 보기로 이동 하는 경우 모든 터치 활동 경우에도 해당 가져오려는 계속 합니다. 응용 프로그램 포인터를 캡처하는 경우 UWP 유사 하 게 동작 수:에서 `PointerEntered` 이벤트 처리기, 요소를 호출 하 여 `CapturePointer` 하 고 해당 손가락에서 모든 터치 작업을 가져옵니다.

UWP 접근 방식을 음악 키보드 예를 들어, 응용 프로그램의 일부 형식에 매우 유용 하 게 되려면를 증명 합니다. 각 키를 처리 하 여 그 키에 대 한 터치 이벤트 손가락이를 사용 하 여 다른 한 키에서 이동할 때 감지 된 `PointerEntered` 및 `PointerExited` 이벤트입니다.

따라서이 문서에 설명 된 터치 추적 효과 UWP 접근 방식을 구현 합니다.

## <a name="the-touch-tracking-effect-api"></a>터치 추적 효과 API

[ **추적 효과 데모 터치** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) 샘플 포함 클래스 (열거형) 하 고 하위 수준 터치 추적을 구현 합니다. 이러한 형식은 네임 스페이스에 속한 `TouchTracking` 라는 단어로 시작 하 고 `Touch`합니다. **TouchTrackingEffectDemos** 이식 가능한 클래스 라이브러리 프로젝트에는 `TouchActionType` 터치 이벤트의 형식에 대 한 열거형:

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

모든 플랫폼 터치 이벤트가 취소를 나타내는 이벤트를 포함 합니다.

`TouchEffect` PCL에 클래스에서 파생 `RoutingEffect` 이라는 이벤트를 정의 하 고 `TouchAction` 라는 이름의 메서드 `OnTouchAction` 를 호출 하는 `TouchAction` 이벤트:

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

`TouchActionEventArgs` 각 이벤트를 함께 제공 되는 모든 정보를 포함 하는 PCL에 클래스:

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

응용 프로그램이 사용할 수는 `Id` 개별 손가락을 추적 하기 위한 속성입니다. 공지는 `IsInContact` 속성입니다. 이 속성은 항상 `true` 에 대 한 `Pressed` 이벤트 및 `false` 에 대 한 `Released` 이벤트입니다. 또한 항상 이므로 `true` 에 대 한 `Moved` iOS 및 Android에는 이벤트입니다. `IsInContact` 속성 수 있습니다 `false` 에 대 한 `Moved` 누름 유니버설 Windows 플랫폼의 바탕 화면에서 프로그램 실행 되 고 단추 없이 마우스 포인터를 이동 하는 경우에 이벤트입니다.

사용할 수는 `TouchEffect` 솔루션의 PCL 프로젝트에 파일을 포함 하 여 고 인스턴스를 추가 하 여 응용 프로그램에서 클래스는 `Effects` Xamarin.Forms 요소의 컬렉션입니다. 에 처리기를 연결 하는 `TouchAction` 이벤트 터치 이벤트를 가져올 수 있습니다.

사용 하도록 `TouchEffect` 사용자의 응용 프로그램에도 필요에 포함 된 플랫폼 구현 **TouchTrackingEffectDemos** 솔루션입니다.

## <a name="the-touch-tracking-effect-implementations"></a>터치 추적 효과 구현

IOS, Android 및 UWP 구현의 `TouchEffect` 가장 간단한 구현 (UWP)으로 시작 하 고 다른 조건들 보다 구조적으로 더 복잡 한 있기 때문에 iOS 구현으로 끝나는 아래에 설명 되어 있습니다.

### <a name="the-uwp-implementation"></a>UWP 구현

UWP 구현의 `TouchEffect` 는 가장 간단한 방법입니다. 클래스에서 파생 되는 일반적으로 `PlatformEffect` 두 어셈블리 특성을 포함 합니다.

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

`OnAttached` 재정의 필드와 연결 합니다. 모든 포인터 이벤트 처리기로 몇 가지 정보를 저장 합니다.

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

        // Get access to the TouchEffect class in the PCL
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

`OnPointerPressed` 처리기 효과 이벤트를 호출 하 여 호출 된 `onTouchAction` 필드에 `CommonHandler` 메서드:

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

`OnPointerPressed` 값도 확인는 `Capture` PCL 및 호출의 효과 클래스에 속성 `CapturePointer` 경우 `true`합니다.

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

구현 해야 하므로 Android 및 iOS 구현 하는 복잡 한 반드시는 `Exited` 및 `Entered` 손가락을 움직이면은 요소를 다른 이벤트입니다. 두 구현 모두 구조가 유사 합니다.

Android `TouchEffect` 클래스 설치에 대 한 처리기는 `Touch` 이벤트:

```csharp
view = Control == null ? Container : Control;
...
view.Touch += OnTouch;
```

클래스에 정적 사전 두 개를 정의합니다.

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

`viewDictionary` 될 때마다 새 항목을 가져옵니다는 `OnAttached` 재정의 호출 됩니다.

```csharp
viewDictionary.Add(view, this);
```

항목의 사전에서 제거 됩니다 `OnDetached`합니다. 모든 인스턴스에 `TouchEffect` 효과에 연결 된 특정 보기와 연결 됩니다. 정적 사전 있도록 `TouchEffect` 다른 보기와 그에 해당을 열거 하는 인스턴스 `TouchEffect` 인스턴스. 이것은 하나의 뷰에서 다른 이벤트를 전송 하는 데 허용 하는 데 필요 합니다.

Android 터치 이벤트를 개별 손가락을 추적 하는 응용 프로그램을 허용 하는 ID 코드를 할당 합니다. `idToEffectDictionary` 연결 사용 하 여이 ID 코드는 `TouchEffect` 인스턴스. 이 사전에 항목이 추가 될 때는 `Touch` 손가락 누르기에 대 한 처리기가 호출 됩니다.

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

            capture = pclTouchEffect.Capture;
            break;

```

항목이 제거 되 고 `idToEffectDictionary` 화면에서 손가락 해제 될 때입니다. `FireEvent` 메서드를 호출 하는 데 필요한 모든 정보를 단순히 누적 된 `OnTouchAction` 메서드:

```csharp
void FireEvent(TouchEffect touchEffect, int id, TouchActionType actionType, Point pointerLocation, bool isInContact)
{
    // Get the method to call for firing events
    Action<Element, TouchActionEventArgs> onTouchAction = touchEffect.pclTouchEffect.OnTouchAction;

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

다른 모든 터치 유형에 두 가지 방법으로 처리 됩니다: 경우는 `Capture` 속성은 `true`, 터치 이벤트는 매우 단순한 번역에는 `TouchEffect` 정보입니다. 가져와서 더 복잡 한 경우 `Capture` 은 `false` 터치 이벤트 보기 간을 이동할 수 할 수 있으므로 합니다. 이의 책임은 `CheckForBoundaryHop` 이동 이벤트 중 호출 되는 메서드. 이 메서드 모두 정적 사전을 사용 합니다. 열거 하는 `viewDictionary` 손가락에 현재 연결을 사용 하 여 보기를 확인 하려면 `idToEffectDictionary` 를 현재 저장 `TouchEffect` 인스턴스 (및 따라서 다음 현재 보기) 특정 ID와 연결 된:

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

변경 된 경우는 `idToEffectionDictionary`, 잠재적으로 메서드 `FireEvent` 에 대 한 `Exited` 및 `Entered` 다른 어떤 보기에서 전송 하는 데 있습니다. 그러나 손가락 된 이동 없이 연결 된 보기에서 사용 되는 영역으로 `TouchEffect`, 또는 연결 된 영향을 주지 않고 보기로 영역입니다.

공지는 `try` 및 `catch` 보기에 액세스 하는 경우 차단 합니다. 탐색 한 후 홈 페이지로 다시 이동 하는 페이지에는 `OnDetached` 메서드는 및에 항목이 남아는 `viewDictionary` Android에서는 삭제 하지만 합니다.

### <a name="the-ios-implementation"></a>IOS 구현

IOS 구현 점을 제외 하 고 Android 구현에 비슷합니다. iOS `TouchEffect` 클래스의 파생 항목 인스턴스화해야 `UIGestureRecognizer`합니다. 이 클래스는 명명 된 iOS 프로젝트에 클래스 `TouchRecognizer`합니다. 이 클래스를 저장 하는 두 개의 정적 사전을 유지 `TouchRecognizer` 인스턴스:

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

이 구조의 많은 `TouchRecognizer` 클래스는 Android 비슷합니다 `TouchEffect` 클래스입니다.

## <a name="putting-the-touch-effect-to-work"></a>작업에 터치 효과 배치합니다.

[ **TouchTrackingEffectDemos** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) 프로그램에 일반적인 작업에 대 한 터치 추적 효과 테스트 하는 5 개의 페이지가 포함 되어 있습니다.

**BoxView 끌어** 페이지 추가할 수 있습니다. `BoxView` 요소를 사용 하는 `AbsoluteLayout` 다음 화면으로 끕니다. [XAML 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml) 두 개를 인스턴스화하고 `Button` 추가 대 한 보기 `BoxView` 요소를 사용 하는 `AbsoluteLayout` 선택을 취소 하 고는 `AbsoluteLayout`합니다.

메서드에 [코드 숨김 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml.cs) 새을 추가 하는 `BoxView` 에 `AbsoluteLayout` 도 추가 `TouchEffect` 개체는 `BoxView` 효과에 이벤트 처리기를 연결 하 고:

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

`TouchAction` 모두에 대 한 모든 터치 이벤트를 처리 하는 이벤트 처리기는 `BoxView` 요소 하지만 주의 해야 하는 데 필요한: 단일 두 손가락을 허용할 수 없습니다 `BoxView` 끌기만 프로그램을 구현 하 고 두 손가락 것 때문에 서로 충돌 합니다. 이러한 이유로 페이지 현재 추적 중인 각 손가락에 대 한 포함 된 클래스를 정의 합니다.

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

`dragDictionary` 에 대 한 항목이 포함 된 모든 `BoxView` 현재 끌고 있습니다.

`Pressed` 터치 작업을이 사전에 항목 추가 및 `Released` 작업에 의해 제거 합니다. `Pressed` 논리를 사전에 항목이 이미 있는지 확인 해야 `BoxView`합니다. 이 경우는 `BoxView` 이미 끌고 및 새 이벤트는 두 번째 손가락으로 그에 동일한는 `BoxView`합니다. 에 대 한는 `Moved` 및 `Released` 동작, 이벤트 처리기 해야에 있는지 확인 사전 항목에 대 한 `BoxView` 하 고 터치 `Id` 속성을 끌어 `BoxView` 사전 항목의 일치:

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

`Pressed` 논리 집합은 `Capture` 의 속성은 `TouchEffect` 개체를 `true`합니다. 동일한 이벤트 처리기를 해당 지문에 대 한 모든 후속 이벤트를 전달 하 것과 효과가 있습니다.

`Moved` 논리 이동은 `BoxView` 변경 하 여는 `LayoutBounds` 연결 된 속성입니다. `Location` 기준으로 이벤트 인수의 속성은 항상는 `BoxView` 끌어 오는 경우는 `BoxView` 일정 한 속도로 끌고는 `Location` 연속 된 이벤트의 속성은 약 동일 합니다. 예를 들어, 손가락이는 `BoxView` 의 중심에는 `Pressed` 작업 저장소는 `PressPoint` 의 속성 (50, 50)는 후속 이벤트에 대 한 동일 하 게 유지 합니다. 경우는 `BoxView` 후속 상수 속도로 대각선 방향으로 끌면 `Location` 동안 속성이 `Moved` 동작의 값을 수 있습니다 (55, 55)는 쿼리에서 `Moved` 논리는 의가로및세로위치에5를추가합니다`BoxView`. 이렇게 하면 이동는 `BoxView` 되도록 중심을 관통 다시 손가락 바로 아래 합니다.

여러 이동할 수 있습니다 `BoxView` 동시에 다른 손가락을 사용 하 여 요소입니다.

[![](touch-tracking-images/boxviewdragging-small.png "BoxView 끌어 페이지의 삼중 스크린샷")](touch-tracking-images/boxviewdragging-large.png "BoxView 끌어 페이지의 삼중 스크린 샷")

### <a name="subclassing-the-view"></a>보기를 서브클래싱합니다.

종종 자체 터치 이벤트를 처리 하는 Xamarin.Forms 요소에 대 한 쉽습니다. **Draggable BoxView 끌어** 페이지는 동일 하 게 작동는 **BoxView 끌어** 페이지 하지만 사용자 끌기 형식의 인스턴스인 요소는 [ `DraggableBoxView` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/DraggableBoxView.cs) 파생 된 클래스 `BoxView`:

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

생성자를 만들고 연결는 `TouchEffect`, 설정 및는 `Capture` 를 먼저 해당 개체를 인스턴스화할 때 속성입니다. 사전이 없습니다 클래스 자체에 저장 하기 때문에 반드시 `isBeingDragged`, `pressPoint`, 및 `touchId` 각 손가락와 관련 된 값입니다. `Moved` 처리 변경는 `TranslationX` 및 `TranslationY` 속성 논리가 작동 하도록 경우에의 부모는 `DraggableBoxView` 않습니다는 `AbsoluteLayout`합니다.

### <a name="integrating-with-skiasharp"></a>SkiaSharp와 통합

다음 두 데모 그래픽 필요 하 고 SkiaSharp이이 목적을 위해 사용 합니다. 에 대 한 자세한 내용은 경우가 [xamarin.forms에서를 사용 하 여 SkiaSharp](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) 이러한 예제를 살펴보려면 전에 합니다. 처음 두 문서 ("SkiaSharp 그리기 기본 사항" 및 "SkiaSharp 줄 및 경로")는 여기에 필요한 모든 다룹니다.

**타원 그리기** 페이지 화면에서 손가락을 살짝 밀어 타원을 그릴 수 있습니다. 손가락을 이동 하는 방법을 따라, 아래 오른쪽에 왼쪽 위 또는 반대 모퉁이에 다른 모퉁이에서 타원을 그릴 수 있습니다. 타원 임의의 색상 및 불투명도 사용 하 여 그린 합니다.

[![](touch-tracking-images/ellipsedrawing-small.png "타원 그리기 페이지의 삼중 스크린샷")](touch-tracking-images/ellipsedrawing-large.png "타원 그리기 페이지의 삼중 스크린샷")

타원 중 하나 다음 터치 할 경우 다른 위치로 끌어 놓을 수 있습니다. 이렇게 하려면으로 "적중 테스트," 특정 시점 그래픽 개체에 대 한 검색 해야 합니다. SkiaSharp 줄임표 되지 않으므로 Xamarin.Forms 요소를 직접 실행할 수 없는 `TouchEffect` 처리 합니다. `TouchEffect` 전체에 적용 해야 `SKCanvasView` 개체입니다.

[EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml) 파일 인스턴스화하는 `SKCanvasView` 단일 셀에서 `Grid`합니다. `TouchEffect` 에 있는 개체를 연결 `Grid`:

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

Android 및 유니버설 Windows 플랫폼에는 `TouchEffect` 에 직접 연결할 수는 `SKCanvasView`, iOS 작동 하지 않는 경우에 합니다. 에 `Capture` 속성이 `true`합니다.

형식의 개체로 나타내는 SkiaSharp 렌더링 하는 각 타원 `EllipseDrawingFigure`:

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

`StartPoint` 및 `EndPoint` 프로그램 터치식 입력; 처리할 때 사용 되는 속성의 `Rectangle` 속성 타원 그리기에 사용 됩니다. `LastFingerLocation` 타원을 끄는 경우에 발생 되는 속성 및 `IsInEllipse` 적중 테스트 메서드를 지원 합니다. 메서드가 반환 `true` 포인터가 타원 안에 있는 경우.

[코드 숨김 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml.cs) 세 컬렉션을 유지 합니다.

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

`draggingFigure` 사전의 하위 집합에 포함 되어는 `completedFigures` 컬렉션입니다. SkiaSharp `PaintSurface` 렌더링 개체에서 이러한 이벤트 처리기는 `completedFigures` 및 `inProgressFigures` 컬렉션:

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

터치 처리의 가장 까다로운 부분은 `Pressed` 처리 합니다. 여기에 적중 테스트를 수행 하지만 코드에서 사용자의 손가락 아래에서 줄임표를 발견 하면 해당 타원만 놓을 수 있습니다 다른 손가락으로 끌 되어 있지 않는 경우. 가 아래 없는 타원 이면 코드 새 타원 그리기의 프로세스를 시작 합니다.

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

다른 SkiaSharp 예로 **손가락 페인트** 페이지. 두 개에서 선 색과 선 너비를 선택할 수 있습니다 `Picker` 뷰를 다음 하나 이상의 손가락으로 그립니다.

[![](touch-tracking-images/fingerpaint-small.png "손가락 페인트 페이지의 삼중 스크린샷")](touch-tracking-images/fingerpaint-large.png "손가락 페인트 페이지의 삼중 스크린샷")

이 예제에는 별도 클래스를 화면에 그릴 각 줄을 나타내는 데 필요 합니다.

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

`SKPath` 개체 각 줄을 렌더링 하는 데 사용 됩니다. [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/FingerPaintPage.xaml.cs) 파일 이러한 개체, 그리고 현재 해당 폴리라인에 대 한 및 완료 된 폴리라인 관리를 위한 두 컬렉션을 유지 합니다.

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

`Pressed` 처리에서는 새 `FingerPaintPolyline`, 호출 `MoveTo` 초기 지점을 저장 하는 경로 개체에 해당 개체를 추가 하 고는 `inProgressPolylines` 사전입니다. `Moved` 호출 처리 `LineTo` 새 손가락 위치와 경로 개체에 및 `Released` 처리 전송에서 완료 된 폴리라인 `inProgressPolylines` 를 `completedPolylines`합니다. 다시 한 번 그리기 코드 실제 SkiaSharp는 비교적 간단 합니다.

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

### <a name="tracking-view-to-view-touch"></a>보기-터치를 추적합니다.

모든 이전 예제를 설정는 `Capture` 속성의는 `TouchEffect` 를 `true`때는 `TouchEffect` 만들어진 경우 또는 `Pressed` 이벤트가 발생 합니다. 이렇게 하면 동일한 요소 뷰를 처음 누를 손가락와 관련 된 모든 이벤트를 받습니다. 마지막 예제에서는 *하지* 설정 `Capture` 를 `true`합니다. 손가락을 움직이면 화면 접촉은 요소 간에 서로 다른 동작을 유발 합니다. 손가락에서 이동 하는 요소와 이벤트를 수신는 `Type` 속성이로 설정 `TouchActionType.Exited` 두 번째 요소와 이벤트를 수신 하 고는 `Type` 설정인 `TouchActionType.Entered`합니다.

이러한 유형의 터치 처리 음악 키보드에 대 한 매우 유용합니다. 키를 누를 때, 뿐만 아니라 다른 키가 두 개에서 손가락 슬라이드 때 검색할 수 있어야 합니다.

**자동 키보드** 페이지는 작은 정의 [ `WhiteKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/WhiteKey.cs) 및 [ `BlackKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BlackKey.cs) 에서 파생 된 클래스 [ `Key` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/Key.cs)에서 파생 되는 `BoxView`합니다.

`Key` 클래스는 실제 음악 프로그램에서 사용할 수 있게 합니다. 라는 공용 속성을 정의 `IsPressed` 및 `KeyNumber`, MIDI 표준에 의해 설정 된 키 코드로 설정 하기 위한 용도가입니다. `Key` 클래스에는 또한 라는 이벤트 정의 `StatusChanged`이며 때 호출 되는 `IsPressed` 속성 변경 합니다.

여러 손가락 각 키에서 허용 됩니다. 이러한 이유로 `Key` 클래스를 유지 관리는 `List` 현재 해당 키를 변경 하는 모든 손가락 터치 ID 번호.

```csharp
List<long> ids = new List<long>();
```

`TouchAction` 에 ID를 추가 하는 이벤트 처리기는 `ids` 목록 모두에 대 한는 `Pressed` 이벤트 유형 및 `Entered` 형식이 아니라 경우에만 `IsInContact` 속성은 `true` 에 대 한는 `Entered` 이벤트입니다. ID에서 제거 되 고 `List` 에 대 한는 `Released` 또는 `Exited` 이벤트:

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

`AddToList` 및 `RemoveFromList` 메서드는 모두 확인 하는 경우는 `List` 비어 있음과 비어 간에 변경 된 경우 호출는 `StatusChanged` 이벤트입니다.

다양 한 `WhiteKey` 및 `BlackKey` 페이지의 요소 정렬 되는 [XAML 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/SilentKeyboardPage.xaml), 전화 가로 모드로 유지 되는 경우 최상의 표시:

[![](touch-tracking-images/silentkeyboard-small.png "자동 키보드 페이지의 삼중 스크린샷")](touch-tracking-images/silentkeyboard-large.png "자동 키보드 페이지의 삼중 스크린 샷")

키에서 손가락을 정리 하는 경우 표시 됩니다 컬러로 약간 변경 하 여 다른 한 키에서 터치 이벤트 전송 됩니다.

## <a name="summary"></a>요약

이 문서 작성 및 하위 수준 멀티 터치 처리를 구현 하는 효과 사용 하는 방법과 효과의 이벤트를 호출 하는 방법을 명시 되어 있습니다.


## <a name="related-links"></a>관련 링크

- [IOS에서 멀티 터치 손가락 추적](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Android에서 추적 멀티 터치 손가락](~/android/app-fundamentals/touch/touch-tracking.md)
- [터치 추적 효과 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)
