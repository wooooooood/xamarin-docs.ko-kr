---
title: Xamarin.ios의 멀티 터치 핑거 추적
description: 이 문서에서는 Xamarin.ios 앱에서 멀티 터치 제스처의 개별 손가락을 추적 하는 방법을 설명 합니다. 손가락 그리기 앱 예제를 중심으로 합니다.
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: eca8a1bdc3c60ae4dbaabf022a9dbe91f7e30402
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767033"
---
# <a name="multi-touch-finger-tracking-in-xamarinios"></a>Xamarin.ios의 멀티 터치 핑거 추적

_이 문서에서는 여러 손가락에서 터치 이벤트를 추적 하는 방법을 보여 줍니다._

다중 터치 응용 프로그램이 화면에서 동시에 이동할 때 개별 손가락을 추적 해야 하는 경우가 있습니다. 일반적인 응용 프로그램 하나는 손가락 그리기 프로그램입니다. 사용자가 단일 손가락으로 그릴 수 있고 한 번에 여러 손가락으로 그릴 수 있도록 하려고 합니다. 프로그램에서 여러 터치 이벤트를 처리할 때 이러한 손가락을 구분 해야 합니다.

손가락이 화면을 처음 터치 하면 iOS는 해당 손가락에 [`UITouch`](xref:UIKit.UITouch) 대 한 개체를 만듭니다. 이 개체는 손가락 화면에서 손가락을 이동한 후 화면에서 리프트 하는 것과 동일 하 게 유지 됩니다. 이때 개체가 삭제 됩니다. 손가락을 추적 하려면 프로그램에서이 `UITouch` 개체를 직접 저장 하지 않아야 합니다. 대신 형식의 [`Handle`](xref:Foundation.NSObject.Handle) `IntPtr` 속성을 사용 하 여 이러한 `UITouch` 개체를 고유 하 게 식별할 수 있습니다.

거의 항상 개별 손가락을 추적 하는 프로그램은 터치 추적을 위한 사전을 유지 관리 합니다. IOS 프로그램의 경우 사전 키는 특정 손가락을 식별 `Handle` 하는 값입니다. 사전 값은 응용 프로그램에 따라 달라 집니다. [FingerPaint](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint) 프로그램에서 터치와 릴리스 사이에 있는 각 손가락 스트로크가 해당 손가락으로 그린 선을 렌더링 하는 데 필요한 모든 정보를 포함 하는 개체와 연결 됩니다. 프로그램은 이러한 용도로 작은 `FingerPaintPolyline` 클래스를 정의 합니다.

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

각 다중선에는 그릴 때 선의 여러 요소를 누적 및 렌더링 [`CGPath`](xref:CoreGraphics.CGPath) 하기 위한 색, 스트로크 너비 및 iOS 그래픽 개체가 있습니다.

아래에 표시 된 나머지 코드는 모두 이라는 `UIView` `FingerPaintCanvasView`파생물에 포함 되어 있습니다. 이 클래스는 하나 이상의 손가락으로 현재 그릴 `FingerPaintPolyline` 때 형식의 개체 사전을 유지 관리 합니다.

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

이 사전을 사용 하면 뷰에서 `FingerPaintPolyline` `UITouch` 개체의 `Handle` 속성을 기반으로 각 손가락에 연결 된 정보를 신속 하 게 가져올 수 있습니다.

또한 `FingerPaintCanvasView` 클래스는 완료 된 `List` 폴리라인의 개체를 유지 관리 합니다.

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

이 `List` 개체는 그려지는 순서와 동일 합니다.

`FingerPaintCanvasView`는로 `View`정의 된 5 개의 메서드를 재정의 합니다.

- [`TouchesBegan`](xref:UIKit.UIResponder.TouchesBegan(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesMoved`](xref:UIKit.UIResponder.TouchesMoved(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesEnded`](xref:UIKit.UIResponder.TouchesEnded(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesCancelled`](xref:UIKit.UIResponder.TouchesCancelled(Foundation.NSSet,UIKit.UIEvent))
- [`Draw`](xref:UIKit.UIView.Draw(CoreGraphics.CGRect))

다양 한 `Touches` 재정의에서 다중선을 구성 하는 점이 누적 됩니다.

[`Draw`] 재정의는 완료 된 폴리라인 및 진행 중인 폴리라인를 그립니다.

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

각 재정의는 `Touches` 메서드에 대 한 `touches` 인수에 저장 된 하나 `UITouch` 이상의 개체로 표시 되는 여러 손가락의 동작을 보고할 수 있습니다. 는 `TouchesBegan` 이러한 개체를 통해 루프를 재정의 합니다. 각 `UITouch` 개체에 대해 메서드는 `LocationInView` 메서드에서 가져온 손가락의 초기 `FingerPaintPolyline` 위치를 저장 하는 등의 새 개체를 만들고 초기화 합니다. 이 `FingerPaintPolyline` 개체는 `UITouch` 개체의 속성 `InProgressPolylines` 을 `Handle` 사전 키로 사용 하 여 사전에 추가 됩니다.

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

메서드는를 호출 `SetNeedsDisplay` 하 여 `Draw` 재정의 및 화면 업데이트에 대 한 호출을 생성 합니다.

화면에서 손가락이 나 손가락을 움직이면는 `View` `TouchesMoved` 재정의에 대 한 여러 호출을 가져옵니다. 이 재정의는 `touches` 인수에 저장 `UITouch` 된 개체를 통해 비슷하게 반복 되며 손가락의 현재 위치를 그래픽 경로에 추가 합니다.

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

컬렉션 `touches` 에는 `TouchesBegan` 또는 를`TouchesMoved`마지막으로 호출한 이후 이동 된 손가락의 `UITouch` 개체만 포함 됩니다. 현재 화면과 접촉 하 `UITouch` 고 있는 *모든* 손가락에 해당 하는 개체가 필요한 경우 해당 정보는 메서드에 대 한 `AllTouches` `UIEvent` 인수의 속성을 통해 사용할 수 있습니다.

재정의 `TouchesEnded` 에는 두 개의 작업이 있습니다. 그래픽 경로에 마지막 지점을 추가 하 고 개체를 `FingerPaintPolyline` `inProgressPolylines` 사전에서 `completedPolylines` 목록으로 전송 해야 합니다.

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

재정의는 단순히 사전에서 개체를 `FingerPaintPolyline` 포기 하 여 처리 됩니다. `TouchesCancelled`

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

이러한 처리를 통해 [FingerPaint](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint) 프로그램은 개별 손가락을 추적 하 고 결과를 화면에 그릴 수 있습니다.

[![](touch-tracking-images/image01.png "개별 손가락 추적 및 화면에 결과 그리기")](touch-tracking-images/image01.png#lightbox)

이제 화면에서 개별 손가락을 추적 하 고 서로 구별할 수 있는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [동급 Xamarin Android 가이드](~/android/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint)
