---
title: 원호를 그리는 3가지 방법
description: 이 문서에서는 SkiaSharp를 사용 하 여 세 가지 방법으로 원호를 정의 하는 방법을 설명 하 고 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4eea7d500876793357113453493fa2fe2ede6cc4
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140020"
---
# <a name="three-ways-to-draw-an-arc"></a>원호를 그리는 3가지 방법

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp를 사용 하 여 세 가지 다른 방법으로 원호를 정의 하는 방법을 알아봅니다._

호는이 무한대 부호의 둥근 부분과 같이 타원의 원주 곡선입니다.

![무한대 기호](arcs-images/arcsample.png)

이러한 정의의 단순성에도 불구 하 고 모든 요구를 충족 하는 원호 그리기 함수를 정의할 수 있는 방법은 없습니다. 따라서 원호를 그리는 가장 좋은 방법의 그래픽 시스템 간에는 아무런 차이가 없습니다. 따라서 `SKPath` 클래스는 한 가지 방법 으로만 제한 하지 않습니다.

`SKPath`[`AddArc`](xref:SkiaSharp.SKPath.AddArc*)메서드, 5 개의 다른 [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*) 메서드 및 두 개의 상대 메서드를 정의 [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*) 합니다. 이러한 메서드는 세 가지 범주로 구분 되며 호를 지정 하는 세 가지 다른 방법을 나타냅니다. 사용할 수 있는 정보는 원호를 정의 하는 데 사용할 수 있는 정보에 따라 달라 지 며,이 호가 그리는 다른 그래픽과 어떻게 적합 한지에 따라 달라 집니다.

## <a name="the-angle-arc"></a>앵글 원호

원호를 그리기 위한 각도 원호를 사용 하려면 타원의 범위를 지정 하는 사각형을 지정 해야 합니다. 이 타원의 원주 원호는 원호의 시작과 길이를 나타내는 타원의 중심에서 각도로 표시 됩니다. 두 가지 방법으로 꺾쇠 원호를 그립니다. [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single))메서드 및 [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKRect,System.Single,System.Single,System.Boolean)) 메서드는 다음과 같습니다.

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

이러한 메서드는 Android [`AddArc`](xref:Android.Graphics.Path.AddArc*) 및 [ `ArcTo` ] f: ArcTo *) 메서드와 동일 합니다. IOS [`AddArc`](xref:CoreGraphics.CGPath.AddArc(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.Boolean)) 메서드는 유사 하지만 타원으로 일반화 되는 것이 아니라 원의 원주로 제한 됩니다.

두 메서드는 모두 `SKRect` 타원의 위치 및 크기를 정의 하는 값으로 시작 합니다.

![원호를 시작 하는 타원입니다.](arcs-images/anglearcoval.png)

호는이 타원의 원주 부분입니다.

`startAngle`인수는 타원의 중심에서 오른쪽으로 그린 가로선을 기준으로 하는 시계 방향 각도 (도)입니다. 인수는에 `sweepAngle` 대 한 상대 경로입니다 `startAngle` . 다음은 `startAngle` `sweepAngle` 각각 60 및 100도의 및 값입니다.

![원호를 정의 하는 각도입니다.](arcs-images/anglearcangles.png)

호가 시작 각도에서 시작 합니다. 해당 길이는 스윕 각도로 제어 됩니다. 호는 빨간색으로 표시 됩니다.

![강조 표시 된 원호입니다.](arcs-images/anglearchighlight.png)

또는 메서드를 사용 하 여 경로에 추가 되는 곡선은 `AddArc` `ArcTo` 단순히 타원의 원주 부분입니다.

![자체의 각도 원호](arcs-images/anglearc.png)

`startAngle`또는 `sweepAngle` 인수는 음수가 될 수 있습니다. 양수 값의 경우 호는 시계 방향으로 `sweepAngle` , 음수 값의 경우에는 시계 반대입니다.

그러나는 `AddArc` 닫힌 컨투어를 정의 *하지* 않습니다. 이후에를 호출 하는 경우에는 `LineTo` `AddArc` 원호의 끝에서 메서드의 지점으로 줄이 그려지며의 경우에도 `LineTo` 마찬가지입니다 `ArcTo` .

`AddArc`는 새 컨투어를 자동으로 시작 하 고의 마지막 인수를 사용 하 여에 대 한 호출과 기능적으로 동일 합니다 `ArcTo` `true` .

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

마지막 인수를 호출 하면 `forceMoveTo` `MoveTo` 원호의 시작 부분에서 호출이 효과적으로 발생 합니다. 새 컨투어를 시작 하는입니다. 이는의 마지막 인수를 사용 하지 않는 경우입니다 `false` .

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

이 버전의에서는 `ArcTo` 현재 위치에서 원호의 시작 부분으로 줄을 그립니다. 즉, 호가 큰 컨투어 가운데 어딘가에 있을 수 있습니다.

**각도 호** 페이지에서는 두 개의 슬라이더를 사용 하 여 시작 및 스윕 각도를 지정할 수 있습니다. XAML 파일은 두 개의 `Slider` 요소 및를 인스턴스화합니다 `SKCanvasView` . `PaintCanvas` [**AngleArcPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) 파일의 처리기는 필드로 정의 된 두 개의 개체를 사용 하 여 타원과 호를 모두 그립니다 `SKPaint` .

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
    float startAngle = (float)startAngleSlider.Value;
    float sweepAngle = (float)sweepAngleSlider.Value;

    canvas.DrawOval(rect, outlinePaint);

    using (SKPath path = new SKPath())
    {
        path.AddArc(rect, startAngle, sweepAngle);
        canvas.DrawPath(path, arcPaint);
    }
}
```

여기에서 볼 수 있듯이 시작 각도와 스윕 각도 모두 음수 값을 사용할 수 있습니다.

[![앵글 Arc 페이지의 세 번째 스크린샷](arcs-images/anglearc-small.png)](arcs-images/anglearc-large.png#lightbox)

호를 생성 하는이 방법은 가장 간단한 알고리즘 방식으로 호를 설명 하는 패라메트릭 방정식을 쉽게 파생 시킬 수 있습니다. 타원의 크기와 위치 및 시작 및 스윕 각도를 알고 있으면 간단한 삼각 함수를 사용 하 여 원호의 시작점과 끝점을 계산할 수 있습니다.

`x = oval.MidX + (oval.Width / 2) * cos(angle)`

`y = oval.MidY + (oval.Height / 2) * sin(angle)`

`angle`값은 `startAngle` 또는 `startAngle + sweepAngle` 입니다.

원호를 정의 하기 위해 두 각도를 사용 하는 것은 예를 들어 원형 차트를 만들기 위해 그리려는 호의 각도 길이를 알고 있는 경우에 가장 적합 합니다. **쪼개진 원형 차트** 페이지에서이를 보여 줍니다. [`ExplodedPieChartPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs)클래스는 내부 클래스를 사용 하 여 일부 구성한 데이터와 색을 정의 합니다.

```csharp
class ChartData
{
    public ChartData(int value, SKColor color)
    {
        Value = value;
        Color = color;
    }

    public int Value { private set; get; }

    public SKColor Color { private set; get; }
}

ChartData[] chartData =
{
    new ChartData(45, SKColors.Red),
    new ChartData(13, SKColors.Green),
    new ChartData(27, SKColors.Blue),
    new ChartData(19, SKColors.Magenta),
    new ChartData(40, SKColors.Cyan),
    new ChartData(22, SKColors.Brown),
    new ChartData(29, SKColors.Gray)
};

```

`PaintSurface`처리기는 먼저 항목을 반복 하 여 숫자를 계산 `totalValues` 합니다. 이를 통해 각 항목의 크기를 합계의 분수로 결정 하 고이를 각도로 변환할 수 있습니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int totalValues = 0;

    foreach (ChartData item in chartData)
    {
        totalValues += item.Value;
    }

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float explodeOffset = 50;
    float radius = Math.Min(info.Width / 2, info.Height / 2) - 2 * explodeOffset;
    SKRect rect = new SKRect(center.X - radius, center.Y - radius,
                             center.X + radius, center.Y + radius);

    float startAngle = 0;

    foreach (ChartData item in chartData)
    {
        float sweepAngle = 360f * item.Value / totalValues;

        using (SKPath path = new SKPath())
        using (SKPaint fillPaint = new SKPaint())
        using (SKPaint outlinePaint = new SKPaint())
        {
            path.MoveTo(center);
            path.ArcTo(rect, startAngle, sweepAngle, false);
            path.Close();

            fillPaint.Style = SKPaintStyle.Fill;
            fillPaint.Color = item.Color;

            outlinePaint.Style = SKPaintStyle.Stroke;
            outlinePaint.StrokeWidth = 5;
            outlinePaint.Color = SKColors.Black;

            // Calculate "explode" transform
            float angle = startAngle + 0.5f * sweepAngle;
            float x = explodeOffset * (float)Math.Cos(Math.PI * angle / 180);
            float y = explodeOffset * (float)Math.Sin(Math.PI * angle / 180);

            canvas.Save();
            canvas.Translate(x, y);

            // Fill and stroke the path
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, outlinePaint);
            canvas.Restore();
        }

        startAngle += sweepAngle;
    }
}
```

`SKPath`각 원형 조각에 대해 새 개체가 만들어집니다. 경로는 중심의 선으로 구성 되 고, 호를 `ArcTo` 그리기 위한 이며, 호출의 중심 결과로 또 다른 줄이 반환 `Close` 됩니다. 이 프로그램은 모든 항목을 가운데에서 50 픽셀로 이동 하 여 "쪼개진" 원형 조각을 표시 합니다. 이 작업을 수행 하려면 각 조각에 대 한 스윕 각도의 중간점 방향에 벡터를 사용 해야 합니다.

[![쪼개진 원형 차트 페이지의 세 번째 스크린샷](arcs-images/explodedpiechart-small.png)](arcs-images/explodedpiechart-large.png#lightbox)

"폭발" 없이 표시 되는 모양을 보려면 단순히 호출을 주석으로 처리 합니다 `Translate` .

[![폭발 하지 않고 쪼개진 원형 차트 페이지의 세 번째 스크린샷](arcs-images/explodedpiechartunexploded-small.png)](arcs-images/explodedpiechartunexploded-large.png#lightbox)

## <a name="the-tangent-arc"></a>탄젠트 호

에서 지 원하는 두 번째 형식의 호는 `SKPath` *탄젠트 호*이므로, arc가 연결 된 두 줄에 탄젠트 한 원의 원주 때문에 호출 됩니다.

탄젠트 원호는 [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) 두 개의 매개 변수를 사용 하 여 메서드를 호출 하거나 해당 `SKPoint` [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,System.Single,System.Single)) 점에 대해 별도의 매개 변수가 있는 오버 로드를 사용 하 여 경로에 추가 됩니다 `Single` .

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

이 `ArcTo` 메서드는 포스트 스크립트 [`arct`](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) (페이지 532) 함수와 iOS [`AddArcToPoint`](xref:CoreGraphics.CGPath.AddArcToPoint(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat)) 메서드와 유사 합니다.

이 `ArcTo` 메서드에는 세 가지 점이 포함 됩니다.

- 컨투어의 현재 점 이거나, `MoveTo` 가 호출 되지 않은 경우에는 점 (0, 0)입니다.
- 메서드에 대 한 첫 번째 요소 인수입니다 `ArcTo` ( *모퉁이점* 이라고 함).
- `ArcTo` *대상 지점*이라고 하는에 대 한 두 번째 요소 인수입니다.

![탄젠트 호를 시작 하는 세 가지 요소](arcs-images/tangentarcthreepoints.png)

이 세 가지 요소는 연결 된 두 줄을 정의 합니다.

![접선의 세 요소를 연결 하는 선](arcs-images/tangentarcconnectinglines.png)

3 개의 점이 colinear 인 경우 &mdash; , 같은 직선에 있으면 &mdash; 호가 그려지지 않습니다.

`ArcTo`또한이 메서드는 `radius` 매개 변수를 포함 합니다. 원의 반경을 정의 합니다.

![탄젠트 원호의 원입니다.](arcs-images/tangentarccircle.png)

탄젠트 원호는 타원에 대해 일반화 되지 않습니다.

두 줄이 임의의 각도에서 만나는 경우 해당 원은 두 줄에 탄젠트 하도록 해당 줄 사이에 삽입 될 수 있습니다.

![두 줄 사이의 탄젠트 원호 원입니다.](arcs-images/tangentarctangentcircle.png)

윤곽선에 추가 된 곡선이 메서드에 지정 된 점에 닿지 않습니다 `ArcTo` . 현재 점에서 첫 번째 탄젠트 점으로 직선을, 두 번째 탄젠트 지점에서 끝나는 호를 빨간색으로 표시 하 여 구성 됩니다.

![두 줄 사이의 강조 표시 된 접선 호](arcs-images/tangentarchighlight.png)

다음은 윤곽선에 추가 되는 최종 직선 및 원호입니다.

![두 줄 사이의 강조 표시 된 접선 호](arcs-images/tangentarc.png)

두 번째 탄젠트 점에서 컨투어를 계속 사용할 수 있습니다.

**탄젠트 호** 페이지에서 탄젠트 호를 시험해 볼 수 있습니다. 이는에서 파생 되는 여러 페이지의 첫 번째 이며 [`InteractivePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/InteractivePage.cs) , 몇 가지 유용한 `SKPaint` 개체를 정의 하 고 처리를 수행 합니다 `TouchPoint` .

```csharp
public class InteractivePage : ContentPage
{
    protected SKCanvasView baseCanvasView;
    protected TouchPoint[] touchPoints;

    protected SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3
    };

    protected SKPaint redStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 15
    };

    protected SKPaint dottedStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    };

    protected void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = baseCanvasView.CanvasSize.Width / (float)baseCanvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            baseCanvasView.InvalidateSurface();
        }
    }
}
```

`TangentArcPage` 클래스는 `InteractivePage`에서 파생됩니다. [**TangentArcPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) 파일의 생성자는 배열을 인스턴스화하고 초기화 하 `touchPoints` 고, `baseCanvasView` (의 경우 `InteractivePage` )를 `SKCanvasView` [**TangentArcPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) 파일에서 인스턴스화된 개체로 설정 합니다.

```csharp
public partial class TangentArcPage : InteractivePage
{
    public TangentArcPage()
    {
        touchPoints = new TouchPoint[3];

        for (int i = 0; i < 3; i++)
        {
            TouchPoint touchPoint = new TouchPoint
            {
                Center = new SKPoint(i == 0 ? 100 : 500,
                                     i != 2 ? 100 : 500)
            };
            touchPoints[i] = touchPoint;
        }

        InitializeComponent();

        baseCanvasView = canvasView;
        radiusSlider.Value = 100;
    }

    void sliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

`PaintSurface`이 처리기는 메서드를 사용 하 여 `ArcTo` 터치 점과 a를 기준으로 원호를 그릴 뿐만 `Slider` 아니라 알고리즘 방식으로 각도의 기반이 되는 원을 계산 합니다.

```csharp
public partial class TangentArcPage : InteractivePage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw the two lines that meet at an angle
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.LineTo(touchPoints[1].Center);
            path.LineTo(touchPoints[2].Center);
            canvas.DrawPath(path, dottedStrokePaint);
        }

        // Draw the circle that the arc wraps around
        float radius = (float)radiusSlider.Value;

        SKPoint v1 = Normalize(touchPoints[0].Center - touchPoints[1].Center);
        SKPoint v2 = Normalize(touchPoints[2].Center - touchPoints[1].Center);

        double dotProduct = v1.X * v2.X + v1.Y * v2.Y;
        double angleBetween = Math.Acos(dotProduct);
        float hypotenuse = radius / (float)Math.Sin(angleBetween / 2);
        SKPoint vMid = Normalize(new SKPoint((v1.X + v2.X) / 2, (v1.Y + v2.Y) / 2));
        SKPoint center = new SKPoint(touchPoints[1].Center.X + vMid.X * hypotenuse,
                                     touchPoints[1].Center.Y + vMid.Y * hypotenuse);

        canvas.DrawCircle(center.X, center.Y, radius, this.strokePaint);

        // Draw the tangent arc
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.ArcTo(touchPoints[1].Center, touchPoints[2].Center, radius);
            canvas.DrawPath(path, redStrokePaint);
        }

        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }

    // Vector methods
    SKPoint Normalize(SKPoint v)
    {
        float magnitude = Magnitude(v);
        return new SKPoint(v.X / magnitude, v.Y / magnitude);
    }

    float Magnitude(SKPoint v)
    {
        return (float)Math.Sqrt(v.X * v.X + v.Y * v.Y);
    }
}
```

실행 되는 **탄젠트 호** 페이지는 다음과 같습니다.

[![탄젠트 호 페이지의 삼중 스크린샷](arcs-images/tangentarc-small.png)](arcs-images/tangentarc-large.png#lightbox)

탄젠트 호는 모퉁이가 둥근 사각형과 같이 모퉁이가 둥근 모퉁이를 만드는 데 적합 합니다. 에는 이미 메서드가 포함 되어 있기 때문에 `SKPath` `AddRoundedRect` **모퉁이가 둥근 Heptagon** 페이지에서는를 사용 하 여 `ArcTo` 일곱 개의 다각형의 모퉁이를 반올림 하는 방법을 보여 줍니다. 코드는 일반 다각형에 대해 일반화 됩니다.

`PaintSurface`클래스의 처리기는 [`RoundedHeptagonPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) `for` heptagon의 7 개 꼭 짓 점의 좌표를 계산 하는 루프를 포함 하 고, 두 번째는이 꼭 짓 점에서 7 변의 중간점을 계산 하는 두 번째 루프를 포함 합니다. 이러한 중간점은 경로를 생성 하는 데 사용 됩니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float cornerRadius = 100;
    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPoint[] vertices = new SKPoint[numVertices];
    SKPoint[] midPoints = new SKPoint[numVertices];

    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    // Coordinates of the midpoints of the sides connecting the vertices
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        int prevVertex = (vertex + numVertices - 1) % numVertices;
        midPoints[vertex] = new SKPoint((vertices[prevVertex].X + vertices[vertex].X) / 2,
                                        (vertices[prevVertex].Y + vertices[vertex].Y) / 2);
    }

    // Create the path
    using (SKPath path = new SKPath())
    {
        // Begin at the first midpoint
        path.MoveTo(midPoints[0]);

        for (int vertex = 0; vertex < numVertices; vertex++)
        {
            SKPoint nextMidPoint = midPoints[(vertex + 1) % numVertices];

            // Draws a line from the current point, and then the arc
            path.ArcTo(vertices[vertex], nextMidPoint, cornerRadius);

            // Connect the arc with the next midpoint
            path.LineTo(nextMidPoint);
        }
        path.Close();

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);
        }
    }
}

```

실행 중인 프로그램은 다음과 같습니다.

[![둥근 Heptagon 페이지의 삼중 스크린샷](arcs-images/roundedheptagon-small.png)](arcs-images/roundedheptagon-large.png#lightbox)

## <a name="the-elliptical-arc"></a>타원형 원호

타원형 원호는 [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,SkiaSharp.SKPoint)) 두 개의 매개 변수가 있는 메서드를 호출 `SKPoint` 하거나 [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,System.Single,System.Single)) 별도 X 및 Y 좌표가 있는 오버 로드를 사용 하 여 경로에 추가 됩니다.

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

타원형 원호는 SVG (스케일러블 벡터 그래픽) 및 유니버설 Windows 플랫폼 클래스에 포함 된 [타원형 호와](https://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) 일치 합니다 [`ArcSegment`](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) .

이러한 `ArcTo` 메서드는 컨투어의 현재 지점인 두 점 사이에 원호를 그리고 메서드의 마지막 매개 변수 `ArcTo` ( `xy` 매개 변수 또는 개별 `x` 및 매개 변수)를 그립니다 `y` .

![타원형 원호를 정의한 두 요소](arcs-images/ellipticalarcpoints.png)

메서드 (, 또는 및)에 대 한 첫 번째 요소 매개 변수는 `ArcTo` `r` 점이 아니지 `rx` `ry` 만 대신 타원의 가로 및 세로 반지름을 지정 합니다.

![타원형 원호를 정의한 타원입니다.](arcs-images/ellipticalarcellipse.png)

`xAxisRotate`매개 변수는이 타원을 회전 하는 시계 방향 수입니다.

![타원형 원호를 정의한 기울어진 타원입니다.](arcs-images/ellipticalarctiltedellipse.png)

이 기울어진 타원이 두 점에 닿을 수 있도록 배치 되 면 점이 두 개의 다른 호로 연결 됩니다.

![타원형 호의 첫 번째 집합입니다.](arcs-images/ellipticalarcellipse1.png)

이러한 두 원호는 두 가지 방법으로 구분할 수 있습니다. 즉, 위쪽 원호는 아래쪽 호 보다 크고 원호를 왼쪽에서 오른쪽으로 그리면 위쪽 원호는 시계 방향으로 그려지고 아래쪽 호는 시계 반대 방향으로 그려집니다.

두 요소 사이에 있는 타원을 다른 방식으로 맞출 수도 있습니다.

![타원형 원호의 두 번째 집합입니다.](arcs-images/ellipticalarcellipse2.png)

이제 시계 방향으로 그려진 위쪽에 작은 호가 있으며 시계 반대 방향으로 그려진 아래쪽에 더 큰 호가 있습니다.

따라서 이러한 두 요소는 다음과 같은 네 가지 방법으로 기울어진 타원으로 정의 된 호로 연결할 수 있습니다.

![네 개의 원형 원호 모두](arcs-images/ellipticalarccolors.png)

이러한 네 원호는 [`SKPathArcSize`](xref:SkiaSharp.SKPathArcSize) 및 [`SKPathDirection`](xref:SkiaSharp.SKPathDirection) 열거형 형식 인수를 메서드에 대 한 네 가지 조합으로 구분 합니다 `ArcTo` .

- 빨강: 고가 나가는 방향입니다. 시계 방향
- 녹색: 고가 나가는 방향입니다. 시계 방향
- 파랑: 고가 나 역 크기입니다. 반시계 방향입니다.
- 자홍: 고가 나 역 크기입니다. 반시계 방향으로

기울어진 타원이 두 요소 사이에 맞지 않는 경우 충분히 커질 때까지 균일 하 게 확장 됩니다. 이 경우 두 개의 고유 원호는 두 개의 점에 연결 됩니다. 이러한 매개 변수는 매개 변수를 사용 하 여 구분할 수 있습니다 `SKPathDirection` .

호를 처음 접하는 경우에는이 방법을 사용 하는 것이 가장 복잡 하지만 회전 된 타원이 있는 원호를 정의할 수 있는 유일한 방법은 원호를 윤곽선의 다른 부분과 통합 해야 할 때 가장 쉬운 방법입니다.

**타원형 원호** 페이지를 사용 하 여 두 개의 점과 타원의 크기와 회전을 대화형으로 설정할 수 있습니다. `EllipticalArcPage`클래스는에서 파생 `InteractivePage` 되 고 `PaintSurface` [**EllipticalArcPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) 코드를 가져온 파일의 처리기는 네 개의 원호를 그립니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        int colorIndex = 0;
        SKPoint ellipseSize = new SKPoint((float)xRadiusSlider.Value,
                                          (float)yRadiusSlider.Value);
        float rotation = (float)rotationSlider.Value;

        foreach (SKPathArcSize arcSize in Enum.GetValues(typeof(SKPathArcSize)))
            foreach (SKPathDirection direction in Enum.GetValues(typeof(SKPathDirection)))
            {
                path.MoveTo(touchPoints[0].Center);
                path.ArcTo(ellipseSize, rotation,
                           arcSize, direction,
                           touchPoints[1].Center);

                strokePaint.Color = colors[colorIndex++];
                canvas.DrawPath(path, strokePaint);
                path.Reset();
            }
    }

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}

```

실행 되 고 있습니다.

[![원형 호 페이지의 삼중 스크린 샷](arcs-images/ellipticalarc-small.png)](arcs-images/ellipticalarc-large.png#lightbox)

**Arc infinity** 페이지는 원형 호를 사용 하 여 infinity 기호를 그립니다. Infinity 부호는 100 단위의 반지름이 100 단위로 구분 된 두 개의 원을 기반으로 합니다.

![두 개의 원](arcs-images/infinitycircles.png)

서로 교차 하는 두 줄은 두 원에 모두 탄젠트 합니다.

![접선을 사용 하는 두 개의 원](arcs-images/infinitycircleslines.png)

Infinity 기호는 이러한 원의 부분과 두 줄의 조합입니다. 원형 호를 사용 하 여 무한대 기호를 그리려면 두 줄을 원으로 접하는 좌표가 결정 되어야 합니다.

원 중 하나에서 오른쪽 사각형을 생성 합니다.

![접선 및 포함 된 원이 있는 두 개의 원](arcs-images/infinitytriangle.png)

원의 반지름은 100 단위 이며, 삼각형의 빗변은 150 단위 이므로 각도 α는 100의 아크사인 (역 사인)을 150 또는 41.8 각도로 나눕니다. 삼각형의 반대쪽의 길이는 41.8도 코사인의 150 배 이거나 숫자가 피타고라스 정리도 계산할 수 있는 112입니다.

그런 다음이 정보를 사용 하 여 탄젠트 지점의 좌표를 계산할 수 있습니다.

`x = 112·cos(41.8) = 83`

`y = 112·sin(41.8) = 75`

4 개의 탄젠트 지점은 원 반지름이 100 인 점 (0, 0)을 중심으로 하는 무한대 기호를 그리는 데 필요 합니다.

![접선 및 좌표가 있는 두 개의 원](arcs-images/infinitycoordinates.png)

`PaintSurface`클래스의 처리기는 [`ArcInfinityPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) (0, 0) 점이 페이지의 가운데에 배치 되도록 infinity 기호를 배치 하 고 경로를 화면 크기에 맞게 조정 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.LineTo(83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.CounterClockwise, 83, -75);
        path.LineTo(-83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.Clockwise, -83, -75);
        path.Close();

        // Use path.TightBounds for coordinates without control points
        SKRect pathBounds = path.Bounds;

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / pathBounds.Width,
                              info.Height / pathBounds.Height));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 5;

            canvas.DrawPath(path, paint);
        }
    }
}
```

이 코드에서는의 속성을 사용 하 여 infinity의 크기에 맞게 크기를 `Bounds` `SKPath` 조정 하기 위해 infinity 사인의 크기를 결정 합니다.

[![호 Infinity 페이지의 삼중 스크린샷](arcs-images/arcinfinity-small.png)](arcs-images/arcinfinity-large.png#lightbox)

결과는 작은 작은 값으로 보입니다 `Bounds` .이는의 속성이 `SKPath` 경로 보다 큰 크기를 보고 하는 것을 의미 합니다.

내부적으로는 여러 정방형 3 차원 곡선을 사용 하는 원호를 대략적으로 계산 합니다. 다음 섹션에서 볼 수 있듯이 이러한 곡선에는 곡선을 그리는 방법을 제어 하는 제어 지점과 렌더링 된 곡선의 일부가 아닌 곡선이 포함 됩니다. 속성에는 `Bounds` 이러한 제어점이 포함 됩니다.

더 밀접 하 게 맞추려면 `TightBounds` 제어 지점이 제외 되는 속성을 사용 합니다. 가로 모드에서 실행 되는 프로그램 및 속성을 사용 하 여 `TightBounds` 경로 범위를 가져오는 프로그램은 다음과 같습니다.

[![범위가 엄격한 Arc Infinity 페이지의 삼중 스크린샷](arcs-images/arcinfinitytightbounds-small.png)](arcs-images/arcinfinitytightbounds-large.png#lightbox)

호와 직선 사이의 연결은 수학적으로 부드럽게 보이지만 원호에서 직선으로의 변경 내용이 약간 급격 하 게 보일 수 있습니다. [**3 가지 유형의 베 지 어 곡선**](beziers.md)에 대 한 다음 문서에서는 더 나은 infinity 기호가 제공 됩니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
