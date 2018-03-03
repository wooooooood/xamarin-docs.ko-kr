---
title: "멀티 터치 손가락 추적"
description: "이 문서에서는 여러 손가락에서 터치 이벤트를 추적 하는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: c096e211bc29e94dbff0202c50ca69780cb6849e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="multi-touch-finger-tracking"></a>멀티 터치 손가락 추적

_이 문서에서는 여러 손가락에서 터치 이벤트를 추적 하는 방법을 보여 줍니다._

화면에 동시에 이동 하는 동안 개별 손가락을 추적 하는 멀티 터치 응용 프로그램을 필요로 하는 경우 경우가 있습니다. 하나의 일반적인 응용 프로그램은 finger-paint 프로그램입니다. 원하는 사용자를 한 손가락으로 그릴 수 있지만 한 번에 여러 손가락으로 그릴 수 있습니다. 여러 터치 이벤트를 처리 하는 프로그램으로 이러한 손가락을 구분 하기 위해 필요 합니다.

먼저 손가락 화면을 터치, iOS 만듭니다는 [ `UITouch` ](https://developer.xamarin.com/api/type/UIKit.UITouch/) 해당 지문에 대 한 개체입니다. 손가락 화면에서 이동 하 고 다음이 시점에서 개체가 삭제 된 화면에서 들어 올릴 때이 개체 동일 하 게 유지 합니다. 추적 하기 위해 손가락을 프로그램이이 저장 하지 않아야 `UITouch` 개체를 직접 합니다. 대신 사용할 수는 [ `Handle` ](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) 형식의 속성이 `IntPtr` 고유 하 게 식별 이러한 `UITouch` 개체입니다.

거의 항상 개별 손가락을 추적 하는 프로그램에 추적 터치에 대 한 사전 유지 관리 합니다. IOS 프로그램에 대 한 사전 키는는 `Handle` 특정 손가락을 식별 하는 값입니다. 사전 값은 응용 프로그램에 따라 달라 집니다. 에 [핑거 페인트](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) 해당 손가락을 사용 하 여 그리는 선을 렌더링 하는 데 필요한 모든 정보를 포함 하는 개체와 연결 된 프로그램을 해제 하기 위해 터치) (에서 각 손가락 선이 있습니다. 프로그램은 정의 하는 작은 `FingerPaintPolyline` 이 용도 대 한 클래스:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new CGPath();
    }

    public CGColor Color { set; get; }

    public float StrokeWidth { set; get; }

    public CGPath Path { private set; get; }
}
```

각 폴리라인에 색, 스트로크 너비 및 iOS 그래픽 [ `CGPath` ](https://developer.xamarin.com/api/type/CoreGraphics.CGPath/) 를 그릴 때 줄의 여러 요소를 렌더링 하 고 누적 하는 개체입니다.


아래 표시 된 코드의 모든 나머지 부분에 포함 된 한 `UIView` 라는 파생 클래스 `FingerPaintCanvasView`합니다. 클래스 형식 개체의 사전을 유지 관리 하는지 `FingerPaintPolyline` 적극적으로 하나 이상의 손가락으로 그릴 되는 동안:

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

이 사전에 뷰를 신속 하 게 가져올 수 있습니다는 `FingerPaintPolyline` 각 손가락와 관련 된 정보에 따라는 `Handle` 의 속성은 `UITouch` 개체입니다.

`FingerPaintCanvasView` 클래스는 또한 유지 관리는 `List` 완료 된 폴리라인에 대 한 개체:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

이 개체 `List` 그려진는 동일한 순서로 표시 됩니다.

`FingerPaintCanvasView` 에 정의 된 5 개의 메서드를 재정의 `View`:

- [`TouchesBegan`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesBegan/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesMoved`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesMoved/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesEnded`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesEnded/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesCancelled`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesCancelled/p/Foundation.NSSet/UIKit.UIEvent/)
- [`Draw`](https://developer.xamarin.com/api/member/UIKit.UIView.Draw/p/CoreGraphics.CGRect/)

다양 한 `Touches` 재정의 누적 다중선을 구성 하는 지점입니다.

[`Draw`] 완료 된 폴리라인 차례로 진행 중인 폴리라인 재정의 그립니다.

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (CGContext context = UIGraphics.GetCurrentContext())
    {
        // Stroke settings
        context.SetLineCap(CGLineCap.Round);
        context.SetLineJoin(CGLineJoin.Round);

        // Draw the completed polylines
        foreach (FingerPaintPolyline polyline in completedPolylines)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }

        // Draw the in-progress polylines
        foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

각각의 `Touches` 재정의 잠재적으로 하나 이상의 표시 된 여러 손가락, 작업 보고 `UITouch` 에 저장 된 개체는 `touches` 메서드에 대 한 인수입니다. `TouchesBegan` 이러한 개체의 재정의 반복 합니다. 각 `UITouch` 개체 메서드를 만들고 새 초기화 `FingerPaintPolyline` 저장에서 가져온 손가락의 초기 위치를 포함 하는 개체는 `LocationInView` 메서드. 이 `FingerPaintPolyline` 개체에 추가 `InProgressPolylines` 사용 하 여 사전은 `Handle` 의 속성은 `UITouch` 사전 키로 개체:

```csharp
public override void TouchesBegan(NSSet touches, UIEvent evt)
{
    base.TouchesBegan(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Create a FingerPaintPolyline, set the initial point, and store it
        FingerPaintPolyline polyline = new FingerPaintPolyline
        {
            Color = StrokeColor,
            StrokeWidth = StrokeWidth,
        };

        polyline.Path.MoveToPoint(touch.LocationInView(this));
        inProgressPolylines.Add(touch.Handle, polyline);
    }
    SetNeedsDisplay();
}
```

메서드가 호출 하 여 마지막 `SetNeedsDisplay` 에 대 한 호출을 생성 하는 `Draw` 재정의 및 화면을 업데이트할 수 있습니다.

화면에서 손가락 또는 손가락 이동는 `View` 를 여러 번 호출 가져옵니다 해당 `TouchesMoved` 재정의 합니다. 이 재정의 마찬가지로 반복는 `UITouch` 에 저장 된 개체는 `touches` 인수 그래픽 경로에 손가락의 현재 위치를 추가 합니다.

```csharp
public override void TouchesMoved(NSSet touches, UIEvent evt)
{
    base.TouchesMoved(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Add point to path
        inProgressPolylines[touch.Handle].Path.AddLineToPoint(touch.LocationInView(this));
    }
    SetNeedsDisplay();
}
```

`touches` 컬렉션에 포함 된 `UITouch` 손가락에 대 한 마지막 호출 후 이동에 대 한 개체 `TouchesBegan` 또는 `TouchesMoved`합니다. 필요한 경우 `UITouch` 해당 하는 개체 *모든* 화면 접촉 현재 손가락, 해당 정보는를 통해 사용할 수는 `AllTouches` 속성은 `UIEvent` 메서드에 대 한 인수입니다.

`TouchesEnded` 재정의 두 개의 작업 합니다. 그래픽 경로 및 전송에 마지막 점을 추가 해야 합니다는 `FingerPaintPolyline` 에서 개체는 `inProgressPolylines` 사전에는 `completedPolylines` 목록:

```csharp
public override void TouchesEnded(NSSet touches, UIEvent evt)
{
    base.TouchesEnded(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Get polyline from dictionary and remove it from dictionary
        FingerPaintPolyline polyline = inProgressPolylines[touch.Handle];
        inProgressPolylines.Remove(touch.Handle);

        // Add final point to path and save with completed polylines
        polyline.Path.AddLineToPoint(touch.LocationInView(this));
        completedPolylines.Add(polyline);
    }
    SetNeedsDisplay();
}
```

`TouchesCancelled` 재정의 하면 중단 하 고 처리 되는 `FingerPaintPolyline` 사전에 개체:

```csharp
public override void TouchesCancelled(NSSet touches, UIEvent evt)
{
    base.TouchesCancelled(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        inProgressPolylines.Remove(touch.Handle);
    }
    SetNeedsDisplay();
}
```

이 처리를 사용 하면 맵는 [핑거 페인트](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) 프로그램 개별 손가락을 추적 하 고 화면에 결과 그립니다.

[ ![](touch-tracking-images/image01.png "개별 손가락을 추적 하 고 화면에 결과 그리기")](touch-tracking-images/image01.png)

이제 화면에서 손가락을 개별 추적 파일 그룹을 구분할 하는 방법을 살펴보았습니다.



## <a name="related-links"></a>관련 링크

- [해당 하는 Xamarin Android 가이드](~/android/app-fundamentals/touch/touch-tracking.md)
- [핑거 페인트 (샘플)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
