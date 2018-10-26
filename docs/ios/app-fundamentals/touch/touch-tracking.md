---
title: Xamarin.iOS에서 추적 하는 멀티 터치 손가락
description: 이 문서에서는 Xamarin.iOS 앱에서 다중 터치 제스처에 각 손가락을 추적 하는 방법을 설명 합니다. 손가락 앱 예제를 중심입니다.
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: a1ddcda84d51b5a8a9220558ddaf9476a2321ee8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105053"
---
# <a name="multi-touch-finger-tracking-in-xamarinios"></a>Xamarin.iOS에서 추적 하는 멀티 터치 손가락

_이 문서에서는 여러 손가락에서 터치 이벤트를 추적 하는 방법을 보여 줍니다._

멀티 터치 응용 프로그램 화면에서 동시에 이동 하는 대로 각 손가락을 추적 해야 할 경우 경우가 있습니다. 일반적인 응용 프로그램을 하나의 finger-paint 프로그램입니다. 사용자를 한 손가락으로 그릴 수 있지만 한 번에 여러 손가락으로 그릴 수 있게 되기를 원하는 합니다. 프로그램이 여러 터치 이벤트를 처리할 때 이러한 두 손가락 사이의 구분 해야 합니다.

손가락 먼저 화면을 터치, iOS 만듭니다는 [ `UITouch` ](https://developer.xamarin.com/api/type/UIKit.UITouch/) 해당 지문에 대 한 개체입니다. 이 개체 손가락 화면에서 이동 하 고이 시점에서 개체가 삭제 되는 화면에서 다음가 뗄 동일 합니다. 추적 하기 위해 손가락을 프로그램이이 저장 하지 않아야 `UITouch` 직접 개체입니다. 대신 사용할 수는 [ `Handle` ](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) 형식의 속성 `IntPtr` 고유 하 게 식별 이러한 `UITouch` 개체입니다.

거의 항상 각 손가락을 추적 하는 프로그램에는 터치 추적에 대 한 사전 유지 관리 합니다. IOS 프로그램에 대 한 사전 키는 `Handle` 특정 손가락을 식별 하는 값입니다. 사전 값은 응용 프로그램에 따라 달라 집니다. 에 [핑거 페인트](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) (릴리스 touch)에서 각 손가락 스트로크는 손가락을 사용 하 여 그리는 선을 렌더링 하는 데 필요한 모든 정보를 포함 하는 개체와 연결 된 프로그램입니다. 프로그램은 정의 하는 작은 `FingerPaintPolyline` 이 목적을 위해 클래스:

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

각 폴리라인에 있고 색, 스트로크 너비는 iOS 그래픽 [ `CGPath` ](https://developer.xamarin.com/api/type/CoreGraphics.CGPath/) 누적를 그릴 때 줄의 여러 요소를 렌더링 하는 개체입니다.


나머지 모든 아래에 표시 된 코드에 포함 되어는 `UIView` 라는 파생 `FingerPaintCanvasView`합니다. 클래스 형식의 개체를 사전 유지 관리는 `FingerPaintPolyline` 적극적으로 하나 이상의 손가락으로 그릴 되는 동안:

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

이 사전에 뷰를 빠르게 가져올 수 있습니다는 `FingerPaintPolyline` 각 손가락 연관 된 정보에 따라는 `Handle` 의 속성을 `UITouch` 개체입니다.

합니다 `FingerPaintCanvasView` 클래스도 유지 관리는 `List` 완료 된 폴리라인에 개체:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

이 개체 `List` 그려진는 동일한 순서로 표시 됩니다.

`FingerPaintCanvasView` 정의한 5 개의 메서드를 재정의 `View`:

- [`TouchesBegan`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesBegan/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesMoved`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesMoved/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesEnded`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesEnded/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesCancelled`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesCancelled/p/Foundation.NSSet/UIKit.UIEvent/)
- [`Draw`](https://developer.xamarin.com/api/member/UIKit.UIView.Draw/p/CoreGraphics.CGRect/)

다양 한 `Touches` 재정의 누적 다중선을 구성 하는 지점입니다.

[`Draw`] 완료 된 폴리라인으로 차례로 진행 다중선 재정의 그립니다.

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

각 합니다 `Touches` 재정의는 잠재적으로 하나 이상의 표시 하는 여러 손가락의 작업을 보고 `UITouch` 에 저장 된 개체는 `touches` 메서드의 인수입니다. `TouchesBegan` 이러한 개체의 재정의 반복 합니다. 각 `UITouch` 메서드를 만들고 초기화 된 새 개체 `FingerPaintPolyline` 에서 가져온 손가락의 초기 위치에 저장을 비롯 하 여 개체를 `LocationInView` 메서드. 이 `FingerPaintPolyline` 개체에 추가 됩니다는 `InProgressPolylines` 사용 하 여 사전을 `Handle` 의 속성을 `UITouch` 사전 키로 개체:

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

메서드를 호출 하 여 마지막 `SetNeedsDisplay` 호출을 생성 하는 `Draw` 재정의 및 화면을 업데이트 합니다.

화면에 손가락이 나 손가락 이동 합니다 `View` 에 대 한 여러 호출을 가져옵니다 해당 `TouchesMoved` 재정의 합니다. 이 재정의 마찬가지로 반복 합니다 `UITouch` 에 저장 된 개체는 `touches` 인수 그래픽 경로에 손가락의 현재 위치를 추가 하 고:

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

합니다 `touches` 컬렉션에만 `UITouch` 개체를 마지막으로 호출한 이후 이동 하는 손가락 `TouchesBegan` 또는 `TouchesMoved`합니다. 해야 하는 경우 `UITouch` 해당 하는 개체 *모든* 화면 닿는 현재 손가락을 정보를 통해 제공 됩니다.는 `AllTouches` 속성을 `UIEvent` 메서드의 인수.

`TouchesEnded` 재정의 두 가지 작업이 있습니다. 마지막 지점 전송 그래픽 경로를 추가 해야 합니다 `FingerPaintPolyline` 에서 개체를 `inProgressPolylines` 사전을 `completedPolylines` 목록:

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

합니다 `TouchesCancelled` 재정의 하면 중단에 의해 처리 됩니다는 `FingerPaintPolyline` 사전에 있는 개체:

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

이 처리를 사용 하면 모두를 [핑거 페인트](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) 프로그램 각 손가락을 추적 하 고 화면에 결과 그립니다.

[![](touch-tracking-images/image01.png "각 손가락을 추적 하 고 결과 화면에서 그리기")](touch-tracking-images/image01.png#lightbox)

이제 살펴보았습니다 화면의 각 손가락을 추적 및 파일 그룹을 구분할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [해당 하는 Xamarin Android 가이드](~/android/app-fundamentals/touch/touch-tracking.md)
- [핑거 페인트 (샘플)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
