---
title: 멀티 터치 손가락 Xamarin.Android에서 추적
description: 이 항목에서는 여러 손가락에서 터치 이벤트를 추적 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 34a9d2d9b8acb05a1b978a70e85038507032faaa
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104689"
---
# <a name="multi-touch-finger-tracking"></a>손가락 멀티 터치 추적

_이 항목에서는 여러 손가락에서 터치 이벤트를 추적 하는 방법을 보여 줍니다._

멀티 터치 응용 프로그램 화면에서 동시에 이동 하는 대로 각 손가락을 추적 해야 할 경우 경우가 있습니다. 일반적인 응용 프로그램을 하나의 finger-paint 프로그램입니다. 사용자를 한 손가락으로 그릴 수 있지만 한 번에 여러 손가락으로 그릴 수 있게 되기를 원하는 합니다. 프로그램이 여러 터치 이벤트를 처리 하는 대로 각 손가락에 해당 하는 이벤트를 구별 해야 합니다. Android는이 목적을 위해 ID 코드를 제공 하지만 가져오기 및 처리는 코드를 약간 까다로울 수 있습니다.

특정 손가락을 사용 하 여 연결 된 모든 이벤트에 대 한 ID 코드는 동일 합니다. ID 코드는 먼저 손가락으로 화면을 터치 및 화면에서가 손가락을 뗄 후 유효 하지 않게 하는 경우에 할당 됩니다.
이러한 ID 코드는 일반적으로 매우 작은 정수 및 Android 이상 터치 이벤트에 대 한 여를 다시 사용 합니다.

거의 항상 각 손가락을 추적 하는 프로그램에는 터치 추적에 대 한 사전 유지 관리 합니다. 사전 키에는 특정 손가락을 식별 하는 ID 코드입니다. 사전 값은 응용 프로그램에 따라 달라 집니다. 에 [핑거 페인트](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) (릴리스 touch)에서 각 손가락 스트로크는 손가락을 사용 하 여 그리는 선을 렌더링 하는 데 필요한 모든 정보를 포함 하는 개체와 연결 된 프로그램입니다. 프로그램은 정의 하는 작은 `FingerPaintPolyline` 이 목적을 위해 클래스:

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

각 폴리라인에 색, 스트로크 너비 및 Android는 그래픽 [ `Path` ](https://developer.xamarin.com/api/type/Android.Graphics.Path/) 누적를 그릴 때 줄의 여러 요소를 렌더링 하는 개체입니다.

아래 표시 된 코드의 나머지 부분에 포함 되어는 `View` 라는 파생 `FingerPaintCanvasView`합니다. 클래스 형식의 개체를 사전 유지 관리는 `FingerPaintPolyline` 적극적으로 하나 이상의 손가락으로 그릴 되는 동안:

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

이 사전에 뷰를 빠르게 가져올 수 있습니다는 `FingerPaintPolyline` 특정 손가락 연관 된 정보입니다.

합니다 `FingerPaintCanvasView` 클래스도 유지 관리는 `List` 완료 된 폴리라인에 개체:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

이 개체 `List` 그려진는 동일한 순서로 표시 됩니다.

`FingerPaintCanvasView` 정의한 두 가지 메서드를 재정의 `View`: [`OnDraw`](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/p/Android.Graphics.Canvas/)
및 [ `OnTouchEvent` ](https://developer.xamarin.com/api/member/Android.Views.View.OnTouchEvent/p/Android.Views.MotionEvent/)합니다.
해당 `OnDraw` 재정의 보기 완료 된 폴리라인으로 그리고 진행 중인 다중선을 그립니다.

재정의는 `OnTouchEvent` 메서드를 가져와서 시작을 `pointerIndex` 에서 값을 `ActionIndex` 속성입니다. 이 `ActionIndex` 값 여러 손가락을 구분 하지만 일관 되지 않은 여러 이벤트에서. 이런 이유로 사용 하는 `pointerIndex` 포인터를 가져오려면 `id` 에서 값는 `GetPointerId` 메서드. 이 ID *는* 여러 이벤트 간에 일치 합니다.

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

재정의 사용 하는 합니다 `ActionMasked` 속성에는 `switch` 문 대신 `Action` 속성. 이유는 다음과 같습니다.

멀티 터치를 사용 하 여 처리 하는 경우는 `Action` 속성의 값이 `MotionEventsAction.Down` 첫 번째 손가락 터치 화면 및 다음의 값에 대 한 `Pointer2Down` 고 `Pointer3Down` 두 번째와 세 번째 손가락도 화면을 터치 합니다. 네 번째와 다섯 번째 손가락 확인 연락처 합니다 `Action` 속성이 없는의 멤버에 해당 하는 숫자 값을 `MotionEventsAction` 열거형! 의미를 해석 하는 값의 비트 플래그의 값을 검사 해야 합니다.

마찬가지로, 손가락 화면을 사용 하 여 연락처를 유지 하는 대로 합니다 `Action` 속성에 값 `Pointer2Up` 및 `Pointer3Up` 두 번째와 세 번째 손가락에 대 한 및 `Up` 첫 번째 손가락에 대 한 합니다.

합니다 `ActionMasked` 속성은 더 적은 되므로와 함께에서 사용할 값의 숫자는 `ActionIndex` 여러 손가락을 구분 하는 속성입니다. 손가락 터치 화면, 속성만 같을 수 있습니다 `MotionEventActions.Down` 에 대 한 첫 번째 손가락 및 `PointerDown` 후속 손가락에 대 한 합니다. 손가락 화면을 유지 하는 대로 `ActionMasked` 의 값이 `Pointer1Up` 후속 손가락에 대 한 및 `Up` 첫 번째 손가락에 대 한 합니다.

사용 하는 경우 `ActionMasked`서 `ActionIndex` 일반적으로 있지만 화면에서 다른 메서드에 인수로 제외 하 고 해당 값을 사용할 필요가 없는 두고 터치를 후속 손가락을 구별할 수는 `MotionEvent` 개체입니다. 가장 중요 한 중 멀티 터치에 대 한 이러한 메서드는 `GetPointerId` 위의 코드에서 호출 합니다. 메서드에서 반환 되는 값을 특정 손가락 이벤트에 연결할 사전 키에 대 한 사용할 수 있습니다.

`OnTouchEvent` 에서 재정의 된 [핑거 페인트](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) 프로그램 프로세스를 `MotionEventActions.Down` 및 `PointerDown` 동일 하 게 만들어 새 이벤트 `FingerPaintPolyline` 개체와 사전에 추가:

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

에 `pointerIndex` 를 뷰 내에서 손가락의 위치를 가져올 때도 사용 됩니다. 와 연결 된 모든 터치 정보는 `pointerIndex` 값입니다. `id` 사전 항목을 만드는 데의 있도록 손가락 여러 메시지를 고유 하 게 식별 합니다.

마찬가지로,는 `OnTouchEvent` 핸들도 재정의 `MotionEventActions.Up` 및 `Pointer1Up` 완료 다중선을 전송 하 여 동일 하 게는 `completedPolylines` 컬렉션 중 그릴 수 있도록는 `OnDraw` 재정의 합니다. 코드는 또한 제거는 `id` 사전에서 항목:

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

까다로운 부분 신청입니다.

다운 사이 및 위로 이벤트 일반적으로 많이 있으며 `MotionEventActions.Move` 이벤트입니다. 이러한 단일 호출에서 묶이는 `OnTouchEvent`에서 다르게 처리 되어야 합니다는 `Down` 및 `Up` 이벤트. 합니다 `pointerIndex` 에서 이전에 가져온 값을 `ActionIndex` 속성을 무시 합니다. 대신 메서드가 여러 가져와야 `pointerIndex` 0 사이 반복 하 여 값 및 `PointerCount` 속성을 매핑한 다음는 `id` 각 `pointerIndex` 값:

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

이 형식으로 처리 하면를 [핑거 페인트](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) 프로그램 각 손가락을 추적 하 고 화면에 결과 그립니다.

[![핑거 페인트 예의 예제 스크린샷](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

이제 살펴보았습니다 화면의 각 손가락을 추적 및 파일 그룹을 구분할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [해당 하는 Xamarin iOS 가이드](~/ios/app-fundamentals/touch/touch-tracking.md)
- [핑거 페인트 (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
