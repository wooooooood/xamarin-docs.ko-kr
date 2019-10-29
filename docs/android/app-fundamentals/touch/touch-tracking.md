---
title: Xamarin Android에서 멀티 터치 핑거 추적
description: 이 항목에서는 여러 손가락에서 터치 이벤트를 추적 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: f960c3cec90bd331f5a1433a869c7720b40c9680
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024279"
---
# <a name="multi-touch-finger-tracking"></a>멀티 터치 핑거 추적

_이 항목에서는 여러 손가락에서 터치 이벤트를 추적 하는 방법을 보여 줍니다._

다중 터치 응용 프로그램이 화면에서 동시에 이동할 때 개별 손가락을 추적 해야 하는 경우가 있습니다. 일반적인 응용 프로그램 하나는 손가락 그리기 프로그램입니다. 사용자가 단일 손가락으로 그릴 수 있고 한 번에 여러 손가락으로 그릴 수 있도록 하려고 합니다. 프로그램에서 여러 터치 이벤트를 처리할 때 각 손가락에 해당 하는 이벤트를 구분 해야 합니다. Android는이 목적을 위해 ID 코드를 제공 하지만 해당 코드를 가져오고 처리 하는 것은 약간 복잡할 수 있습니다.

특정 손가락에 연결 된 모든 이벤트에 대해 ID 코드는 동일 하 게 유지 됩니다. 이 ID 코드는 손가락이 먼저 화면에 닿을 때 할당 되며 손가락을 화면에서 리프트 한 후에는 유효 하지 않게 됩니다.
이러한 ID 코드는 일반적으로 매우 작은 정수 이며, Android는 이후 터치 이벤트에 대해이를 다시 사용할 수 있습니다.

거의 항상 개별 손가락을 추적 하는 프로그램은 터치 추적을 위한 사전을 유지 관리 합니다. 사전 키는 특정 손가락을 식별 하는 ID 코드입니다. 사전 값은 응용 프로그램에 따라 달라 집니다. [FingerPaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) 프로그램에서 터치와 릴리스 사이에 있는 각 손가락 스트로크가 해당 손가락으로 그린 선을 렌더링 하는 데 필요한 모든 정보를 포함 하는 개체와 연결 됩니다. 프로그램은 이러한 용도로 작은 `FingerPaintPolyline` 클래스를 정의 합니다.

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new Path();
    }

    public Color Color { set; get; }

    public float StrokeWidth { set; get; }

    public Path Path { private set; get; }
}
```

각 다중선에는 그릴 때 선의 여러 요소를 누적 및 렌더링 하기 위한 색, 스트로크 너비 및 Android 그래픽 [`Path`](xref:Android.Graphics.Path) 개체가 있습니다.

아래에 표시 된 코드의 나머지 부분은 `FingerPaintCanvasView`이라는 `View` 파생물에 포함 되어 있습니다. 이 클래스는 하나 이상의 손가락으로 현재 그려지는 시간 동안 `FingerPaintPolyline` 형식의 개체 사전을 유지 관리 합니다.

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

이 사전을 사용 하면 보기가 특정 손가락에 연결 된 `FingerPaintPolyline` 정보를 신속 하 게 가져올 수 있습니다.

`FingerPaintCanvasView` 클래스는 완료 된 폴리라인의 `List` 개체도 유지 관리 합니다.

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

이 `List` 개체는 그려지는 순서와 동일 합니다.

`FingerPaintCanvasView` `View`에서 정의한 두 가지 메서드를 재정의 [`OnDraw`](xref:Android.Views.View.OnDraw*)
및를 [`OnTouchEvent`](xref:Android.Views.View.OnTouchEvent*)합니다.
`OnDraw` 재정의에서 뷰는 완성 된 폴리라인를 그린 다음 진행 중인 폴리라인를 그립니다.

`OnTouchEvent` 메서드의 재정의는 `ActionIndex` 속성에서 `pointerIndex` 값을 가져와 시작 합니다. 이 `ActionIndex` 값은 여러 손가락을 구별 하지만 여러 이벤트에서 일치 하지 않습니다. 이러한 이유로 `pointerIndex`를 사용 하 여 `GetPointerId` 메서드에서 포인터 `id` 값을 가져옵니다. 이 ID *는* 여러 이벤트에서 일치 합니다.

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        // ...
    }

    // Invalidate to update the view
    Invalidate();

    // Request continued touch input
    return true;
}
```

재정의는 `Action` 속성이 아닌 `switch` 문의 `ActionMasked` 속성을 사용 합니다. 이유는 다음과 같습니다.

다중 터치를 처리 하는 경우 `Action` 속성에는 화면을 터치 하는 첫 번째 손가락의 `MotionEventsAction.Down` 값이 포함 되 고, 두 번째 및 세 번째 손가락으로도 `Pointer2Down` 및 `Pointer3Down` 값이 화면을 터치 합니다. 네 번째 및 다섯 번째 손가락으로 연락처를 만들 때 `Action` 속성에는 `MotionEventsAction` 열거형의 멤버에도 해당 하지 않는 숫자 값이 있습니다. 값의 비트 플래그 값을 검사 하 여 의미를 해석 해야 합니다.

마찬가지로 손가락으로 화면에 접촉 하는 경우 `Action` 속성은 두 번째 및 세 손가락에 대 한 `Pointer2Up` 및 `Pointer3Up` 값을 포함 하 고 첫 번째 손가락의 `Up` 합니다.

`ActionMasked` 속성은 여러 손가락을 구분 하기 위해 `ActionIndex` 속성과 함께 사용 되기 때문에 더 작은 수의 값을 사용 합니다. 손가락이 화면을 터치 하는 경우 속성은 첫 번째 손가락에 대해서만 `MotionEventActions.Down` 하 고 다음 손가락에 대해서는 `PointerDown` 수 있습니다. 손가락이 화면에서 나갈 때 `ActionMasked`는 후속 손가락의 `Pointer1Up` 값을 가지 며 첫 번째 손가락에 `Up` 합니다.

`ActionMasked`를 사용 하는 경우 `ActionIndex`는 이후 손가락을 서로 구분 하 여 화면에 표시 하지만, 일반적으로 `MotionEvent` 개체의 다른 메서드에 대 한 인수를 제외 하 고는 해당 값을 사용할 필요가 없습니다. 멀티 터치를 위해 이러한 메서드 중 가장 중요 한 하나는 위의 코드에서 호출 `GetPointerId`입니다. 이 메서드는 사전 키로 특정 이벤트를 손가락에 연결 하는 데 사용할 수 있는 값을 반환 합니다.

[FingerPaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) 프로그램의 `OnTouchEvent` 재정의는 새 `FingerPaintPolyline` 개체를 만들어 사전에 추가 하 여 `MotionEventActions.Down` 및 `PointerDown` 이벤트를 동일 하 게 처리 합니다.

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:

            // Create a Polyline, set the initial point, and store it
            FingerPaintPolyline polyline = new FingerPaintPolyline
            {
                Color = StrokeColor,
                StrokeWidth = StrokeWidth
            };

            polyline.Path.MoveTo(args.GetX(pointerIndex),
                                 args.GetY(pointerIndex));

            inProgressPolylines.Add(id, polyline);
            break;
        // ...
    }
    // ...        
}
```

뷰 내에서 손가락의 위치를 가져오는 데에도 `pointerIndex` 사용 됩니다. 모든 터치 정보는 `pointerIndex` 값과 연결 됩니다. `id`는 여러 메시지에서 손가락을 고유 하 게 식별 하므로 사전 항목을 만드는 데 사용 됩니다.

마찬가지로 `OnTouchEvent` 재정의도 `MotionEventActions.Up`를 처리 하 고 `OnDraw` 재정의 중에 그릴 수 있도록 완료 된 다중선을 `completedPolylines` 컬렉션으로 전송 하 여 동일 하 게 `Pointer1Up` 합니다. 또한이 코드는 사전에서 `id` 항목을 제거 합니다.

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Up:
        case MotionEventActions.Pointer1Up:

            inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                args.GetY(pointerIndex));

            // Transfer the in-progress polyline to a completed polyline
            completedPolylines.Add(inProgressPolylines[id]);
            inProgressPolylines.Remove(id);
            break;

        case MotionEventActions.Cancel:
            inProgressPolylines.Remove(id);
            break;
    }
    // ...        
}
```

이제 까다로운 부분입니다.

일반적으로 다운 이벤트와 위로 이벤트 간에는 많은 `MotionEventActions.Move` 이벤트가 있습니다. 이러한 이벤트는 `OnTouchEvent`에 대 한 단일 호출에 번들로 제공 되며 `Down` 및 `Up` 이벤트와 다르게 처리 되어야 합니다. `ActionIndex` 속성에서 이전에 가져온 `pointerIndex` 값은 무시 해야 합니다. 대신, 메서드는 0과 `PointerCount` 속성 사이를 반복 하 여 여러 개의 `pointerIndex` 값을 가져온 다음 각 `pointerIndex` 값에 대 한 `id`을 가져와야 합니다.

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Move:

            // Multiple Move events are bundled, so handle them differently
            for (pointerIndex = 0; pointerIndex < args.PointerCount; pointerIndex++)
            {
                id = args.GetPointerId(pointerIndex);

                inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                    args.GetY(pointerIndex));
            }
            break;
        // ...
    }
    // ...        
}
```

이 유형의 처리를 통해 [FingerPaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) 프로그램은 개별 손가락을 추적 하 고 결과를 화면에 그릴 수 있습니다.

[FingerPaint 예제에서![예제 스크린샷](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

이제 화면에서 개별 손가락을 추적 하 고 서로 구별할 수 있는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [동급 Xamarin iOS 가이드](~/ios/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)
