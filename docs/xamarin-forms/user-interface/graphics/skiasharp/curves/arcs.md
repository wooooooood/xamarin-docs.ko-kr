---
title: 원호를 그리는 3가지 방법
description: 이 문서에서는 SkiaSharp를 사용 하 여 세 가지 방법으로 타원을 정의 하는 방법에 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2017
ms.openlocfilehash: cfa96273b6c23d755925b08c9daec22c94627be7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61088929"
---
# <a name="three-ways-to-draw-an-arc"></a>원호를 그리는 3가지 방법

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_SkiaSharp를 사용 하 여 세 가지 방법으로 타원을 정의 하는 방법 알아보기_

호 곡선이이 무한대 기호 둥근된 파트와 같은 타원의 일부:

![](arcs-images/arcsample.png "무한대 기호")

해당 정의의 단순성에도 불구 하 고는 모든 요구를 충족 하는 호 그리기 함수를 정의할 수 없습니다 및 따라서는 가장 좋은 방법은 호를 그릴 그래픽 시스템 간에 합의 되지 않습니다. 이러한 이유로 `SKPath` 하나만 방법 자체를 클래스에 제한 하지 않습니다.

`SKPath` 정의 [ `AddArc` ](xref:SkiaSharp.SKPath.AddArc*) 메서드를 서로 다른 5 [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo*) 메서드 및 두 명의 상대 [ `RArcTo` ](xref:SkiaSharp.SKPath.RArcTo*) 메서드. 이러한 메서드는 호를 지정 하 세 가지 매우 다양 한 방법을 나타내는 세 가지 범주로 나뉩니다. 것을 사용할지는 호의 및이 호 다른 그래픽을 그리는 연동 하는 방법을 정의 하는 제공 되는 정보에 따라 달라 집니다.

## <a name="the-angle-arc"></a>각도 호

타원 그리기 각도 호 접근 방식은 타원을 제한 하는 사각형을 지정 하는 필요 합니다. 이 타원의 호 시작 호 및 해당 길이 나타내는 타원의 가운데에서 각도으로 표시 됩니다. 두 가지 방법이 각도 호를 그립니다. 이들은 합니다 [ `AddArc` ](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single)) 메서드 및 [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKRect,System.Single,System.Single,System.Boolean)) 메서드:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

이러한 메서드는 Android 동일 [ `AddArc` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.AddArc/p/Android.Graphics.RectF/System.Single/System.Single/) 하 고 [ `ArcTo` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.ArcTo/p/Android.Graphics.RectF/System.Single/System.Single/System.Boolean/) 메서드. IOS [ `AddArc` ](xref:CoreGraphics.CGPath.AddArc(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.Boolean)) 메서드 비슷합니다 되지만 원의 원주에 원호 제한 보다 타원을 일반화 합니다.

두 방법 모두 시작을 `SKRect` 타원의 크기와 위치를 정의 하는 값:

![](arcs-images/anglearcoval.png "각도 호를 시작 하는 타원")

이 타원의 원 둘레의 일부인 호입니다.

`startAngle` 인수는 오른쪽에는 타원의 중심에서 가져온 가로줄을 기준으로 시계 방향의 각도입니다. 합니다 `sweepAngle` 인수는 기준으로 `startAngle`합니다. 다음과 같습니다 `startAngle` 고 `sweepAngle` 60도의 및 100도 각각 값:

![](arcs-images/anglearcangles.png "각도 호를 정의 하는 각도")

호의 시작 각도 시작 합니다. 스윕 각도 길이가 적용 됩니다. 호는 빨간색으로 다음과 같습니다.

![](arcs-images/anglearchighlight.png "강조 표시 된 각도 호")

곡선 경로 추가 합니다 `AddArc` 또는 `ArcTo` 메서드는 단순히 원주 타원의 부분:

![](arcs-images/anglearc.png "자체적으로 각도 호")

합니다 `startAngle` 또는 `sweepAngle` 인수는 음수일 수 있습니다. 양수 값에 대해 시계 방향으로 호는 `sweepAngle` 및 음수 값에 대 한 시계 반대 방향으로 합니다.

그러나 `AddArc` 않습니다 *하지* 닫힌된 윤곽선을 정의 합니다. 호출 하는 경우 `LineTo` 후 `AddArc`에서 호의 끝에서 줄이 그려집니다 합니다 `LineTo` 메서드와 동일한의 경우도 마찬가지 `ArcTo`합니다.

`AddArc` 자동으로 새 윤곽선을 시작 하 고 호출으로 기능적 `ArcTo` 의 마지막 인수를 사용 하 여 `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

마지막 인수 라고 `forceMoveTo`를 효과적으로 수행 하 고는 `MoveTo` 부채꼴의 시작 부분에서 호출 합니다. 새 윤곽선을 시작합니다. 즉 하지 못할 경우의 마지막 인수를 사용 하 여 `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

이 버전의 `ArcTo` 부채꼴의 시작 부분에 현재 위치에서 줄을 그립니다. 즉, 호 큰 윤곽선 중간 어딘가에 있을 수 있습니다.

합니다 **각도 호** 페이지에서는 두 개의 슬라이더를 사용 하 여 시작을 지정 하 여 스윕 각도를 수 있습니다. XAML 파일을 두 개를 인스턴스화하고 `Slider` 요소 및 `SKCanvasView`합니다. `PaintCanvas` 처리기에는 [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) 타원 및 원호 2를 사용 하 여 파일 그립니다 `SKPaint` 필드로 정의 된 개체:

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

알 수 있듯이 시작 각도 및 스윕 각도 둘 다 음수 값에 사용할 수 있습니다.

[![](arcs-images/anglearc-small.png "삼중 각도 호 페이지 스크린샷")](arcs-images/anglearc-large.png#lightbox "삼중 각도 호 페이지 스크린샷")

알고리즘 방식으로 가장 간단 하 고 호를 생성 하는이 방법 이며 호를 설명 하는 매개 방정식을 파생 하기가 쉽습니다. 타원 및 시작 및 스윕 각도의 위치와 크기를 알고 있으면 호의 시작점과 끝점 수를 계산 간단한 삼각 함수를 사용 하 여.

`x = oval.MidX + (oval.Width / 2) * cos(angle)`

`y = oval.MidY + (oval.Height / 2) * sin(angle)`

합니다 `angle` 값은 `startAngle` 또는 `startAngle + sweepAngle`합니다.

호를 정의 하는 두 개의 각도 사용 그리려면, 예를 들어 원형 차트 만들기를 원하는 호의 angular 길이 알고 있는 경우에 적합 합니다. 합니다 **쪼개진 원형 차트** 이 페이지를 보여 줍니다. 합니다 [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) 클래스는 내부 클래스를 사용 하 여 일부 작성 된 데이터 및 색을 정의 합니다.

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

합니다 `PaintSurface` 처리기는 먼저 계산에 항목을 반복을 `totalValues` 수입니다. 이 통해 수는 총 부분으로 각 항목의 크기를 결정 하 고 각도를 변환한:

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

새 `SKPath` 각 원형 조각이 개체가 만들어집니다. 센터에서 한 줄으로 구성 된 경로 `ArcTo` center 결과에 원호, 및 다른 줄을 다시 그리기를 `Close` 호출 합니다. 이 프로그램을 이동 하 여 모든 센터에서 50 픽셀 "쪼개기" 원형 조각을 표시 합니다. 해당 작업에는 각 조각에 대 한 스윕 각도의 중간점의 방향에서 벡터를 필요합니다.

[![](arcs-images/explodedpiechart-small.png "쪼개진 원형 차트 페이지의 3 배가 스크린샷")](arcs-images/explodedpiechart-large.png#lightbox "삼중 쪼개진 원형 차트 페이지 스크린샷")

그렇 군요 "급증" 하지 않고를 확인 하려면 단순히 주석으로 처리 된 `Translate` 호출:

[![](arcs-images/explodedpiechartunexploded-small.png "급증 없이 쪼개진 원형 차트 페이지의 3 배가 스크린샷")](arcs-images/explodedpiechartunexploded-large.png#lightbox "삼중 급증 없이 쪼개진 원형 차트 페이지 스크린샷")

## <a name="the-tangent-arc"></a>탄젠트 호

두 번째 호에서 지 원하는 유형의 `SKPath` 은 *탄젠트 호*, 소위 다음 호는 두 개의 연결 된 선 탄젠트 원의 원주 때문에.

탄젠트 호를 호출 하 여 경로에 추가 됩니다는 [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) 두 개를 사용 하 여 메서드 `SKPoint` 매개 변수 또는 [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,System.Single,System.Single)) 별도의 오버 로드 `Single` 에 대 한 매개 변수는 사항은 다음과 같습니다.

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

이렇게 `ArcTo` 메서드는 포스트 스크립트는 비슷합니다 [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) (페이지 532) 함수 및 iOS [ `AddArcToPoint` ](xref:CoreGraphics.CGPath.AddArcToPoint(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat)) 메서드.

`ArcTo` 방법은 세 지점:

- 경우는 윤곽선 또는 점 (0, 0)의 현재 지점 `MoveTo` 호출 되지 않았습니다
- 첫 번째 지점 인수는 `ArcTo` 메서드를 호출 합니다 *지점 모서리*
- 두 번째 지점 인수 `ArcTo`라는 합니다 *대상 지점*:

![](arcs-images/tangentarcthreepoints.png "탄젠트 호를 시작 하는 3 개의 점으로")

세 가지 사항은 연결 된 두 줄을 정의합니다.

![](arcs-images/tangentarcconnectinglines.png "탄젠트 원호의 세 개의 점을 연결 하는 줄")

3 개의 점으로 동일 선상의 경우 &mdash; 즉, 동일한 직선에 놓여 있습니다 경우 &mdash; 원호가 가져오게 됩니다.

합니다 `ArcTo` 메서드에 포함 됩니다는 `radius` 매개 변수입니다. 이 원의 반지름을 정의 합니다.

![](arcs-images/tangentarccircle.png "탄젠트 원호의 원")

탄젠트 호 타원에 대 한 일반화 되지 됩니다.

모든 각도로 두 줄을 충족 하는 경우 해당 원 수 사이 삽입 선을 두 줄으로 탄젠트 되도록:

![](arcs-images/tangentarctangentcircle.png "두 줄 사이의 탄젠트 호 원")

윤곽선에 추가 되는 곡선을 건드리지 않습니다에 지정 된 지점 중 하나는 `ArcTo` 메서드. 첫 번째 탄젠트 지점, 빨간색으로 여기 표시 된 두 번째 탄젠트 점에서 끝나는 호를 현재 위치에서 직선의 구성 됩니다.

![](arcs-images/tangentarchighlight.png "두 줄 사이의 강조 표시 된 탄젠트 호")

최종 직선 및 호 윤곽선에 추가 되는 다음과 같습니다.

![](arcs-images/tangentarc.png "두 줄 사이의 강조 표시 된 탄젠트 호")

두 번째 탄젠트 점에서 윤곽선을 계속할 수 있습니다.

합니다 **탄젠트 호** 페이지 탄젠트 호의 실험할 수 있습니다. 파생 되는 여러 페이지의 첫 번째 [ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/InteractivePage.cs)를 정의 하는 몇 가지 유용한 `SKPaint` 개체 및 수행 `TouchPoint` 처리:

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

`TangentArcPage` 클래스는 `InteractivePage`에서 파생됩니다. 생성자에는 [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) 파일이 인스턴스화 및 초기화 하는 일을 담당 합니다 `touchPoints` 배열 및 설정 `baseCanvasView` (에서 `InteractivePage`)에 `SKCanvasView` 개체에서 인스턴스화되는 [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) 파일:

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

`PaintSurface` 처리기에서 사용 합니다 `ArcTo` 터치 포인트를 기반으로 한 호를 그리려면 메서드 및 `Slider`, 알고리즘 방식으로 원의 각도에 따라 계산 되지만:

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

다음은 **탄젠트 호** 실행 페이지:

[![](arcs-images/tangentarc-small.png "Arc Tangent 페이지 스크린샷 삼중")](arcs-images/tangentarc-large.png#lightbox "삼중 탄젠트 호 페이지 스크린샷")

탄젠트 호는 모서리가 둥근된 사각형을 같은 둥근된 모서리를 만드는 데 적합 합니다. 때문에 `SKPath` 이미 포함 되어 있습니다를 `AddRoundedRect` 메서드는 **Heptagon 반올림** 페이지를 사용 하는 방법에 설명 `ArcTo` 7 양면 다각형의 모퉁이 둥글게 하는 것에 대 한 합니다. (코드는 일반 모든 다각형에 대 한 범용 화) 합니다.

합니다 `PaintSurface` 처리기는 [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) 클래스 하나를 포함 합니다. `for` heptagon, 및에서 이러한 7 측면의 중간점을 계산 하는 7 개의 꼭 짓 점 좌표를 계산 하는 루프 꼭 짓 점입니다. 이러한 중간점 경로 생성에 사용 됩니다.

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

실행 중인 프로그램이 다음과 같습니다.

[![](arcs-images/roundedheptagon-small.png "삼중 반올림 Heptagon 페이지 스크린샷")](arcs-images/roundedheptagon-large.png#lightbox "삼중 반올림 Heptagon 페이지 스크린샷")

## <a name="the-elliptical-arc"></a>타원형 원호

타원형 호를 호출 하 여 경로에 추가 됩니다는 [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,SkiaSharp.SKPoint)) 두 변수가 있는 메서드에 `SKPoint` 매개 변수 또는 [ `ArcTo` ](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,System.Single,System.Single)) 별도 X와 Y 좌표 오버 로드:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

타원형 호는 일치 하는 [타원형 호](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) 확장성 SVG (벡터 그래픽) 및 유니버설 Windows 플랫폼에 포함 된 [ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) 클래스입니다.

이러한 `ArcTo` 메서드는 윤곽선의 현재 지점 두 점 사이의 호를 그립니다 및 마지막 매개 변수를 `ArcTo` 메서드 (합니다 `xy` 매개 변수 또는 별도 `x` 및 `y` 매개 변수):

![](arcs-images/ellipticalarcpoints.png "타원형 호를 정의 하는 두 지점")

첫 번째 지점 매개 변수를 `ArcTo` 메서드 (`r`, 또는 `rx` 및 `ry`) 점이 아닌 전혀 아니라 대신; 되는 타원의 가로 및 세로 반지름

![](arcs-images/ellipticalarcellipse.png "타원 타원형 호를 정의 합니다.")

`xAxisRotate` 매개 변수는이 타원의 회전을 시계 방향 각도입니다.

![](arcs-images/ellipticalarctiltedellipse.png "타원형 호를 정의한 기운된 타원")

이 기운된 타원 두 개의 점을 이어지도록 합니다이 배치를 점은 두 개의 다른 호로 연결 됩니다.

![](arcs-images/ellipticalarcellipse1.png "첫 번째 집합이 타원형 원호")

이러한 두 개의 원호 다음과 같이 두 가지 방법으로 구별할 수 수 있습니다. 상위 호 아래쪽 호 보다 큰 이며 호가 왼쪽에서 오른쪽을 따라 상위 호가 시계 방향으로 아래쪽 호가 시계 반대 방향으로 하는 동안.

다른 방식으로 두 점 사이의 타원을 맞출 수 이기도 합니다.

![](arcs-images/ellipticalarcellipse2.png "두 번째 집합이 타원형 원호")

이제는 시계 방향으로 그려지는 맨 위에 작은 호 그려지는 맨 아래에서 더 큰 호 시계 반대 방향으로.

따라서 네 가지 방법 중 총에서 기운된 타원에 의해 정의 되는 호에서 이러한 두 지점을 연결할 수 있습니다.

![](arcs-images/ellipticalarccolors.png "모든 4 개의 타원형 원호")

이러한 4 개의 타원의 4 가지 조합으로 구분 됩니다는 [ `SKPathArcSize` ](xref:SkiaSharp.SKPathArcSize) 하 고 [ `SKPathDirection` ](xref:SkiaSharp.SKPathDirection) 열거형 형식 인수가 `ArcTo` 메서드:

- 빨강: SKPathArcSize.Large 및 SKPathDirection.Clockwise
- 녹색: SKPathArcSize.Small 및 SKPathDirection.Clockwise
- 파란색: SKPathArcSize.Small 및 SKPathDirection.CounterClockwise
- 자홍: SKPathArcSize.Large 및 SKPathDirection.CounterClockwise

기운된 타원 두 점 사이 맞게 충분히 큰 경우 다음은 균일 하 게 규모가 충분히 될 때까지 합니다. 두 개의 고유 원호 경우 두 요소를 연결합니다. 사용 하 여 구분할 수 있습니다 이러한 합니다 `SKPathDirection` 매개 변수입니다.

호를 정의 하는 데이 방법은 첫 번째 발생의 복잡 한 말 이지만 회전된 타원을 사용 하 여 원호를 정의할 수 있습니다 하는 유일한 방법은 이므로 것이 가장 쉬운 방법은 타원 윤곽선의 다른 부분과 통합 해야 하는 경우.

합니다 **타원형 호** 페이지를 사용 하면 대화형으로 두 점이 크기 및 타원의 회전을 설정할 수 있습니다. `EllipticalArcPage` 클래스에서 파생 되며 `InteractivePage`, 및 `PaintSurface` 처리기에는 [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) 코드 숨김 파일 네 개의 타원을 그립니다:

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

여기이 실행 됩니다.

[![](arcs-images/ellipticalarc-small.png "타원형 호 페이지 스크린샷 삼중")](arcs-images/ellipticalarc-large.png#lightbox "삼중 타원형 호 페이지 스크린샷")

합니다 **Arc 무한대** 페이지 타원형 호를 사용 하 여은 무한대 기호를 그립니다. 무한대 기호를 구분 하 여 100 단위 100 단위의 반지름을 사용 하 여 두 개의 원을 기반으로 합니다.

![](arcs-images/infinitycircles.png "두 개의 원")

서로 교차 하는 두 줄 모두 원형으로 탄젠트 같습니다.

![](arcs-images/infinitycircleslines.png "접선을 사용 하 여 두 개의 원")

무한대 기호에는 이러한 원과 두 줄의 일부 조합입니다. 무한대 기호를 그릴 타원형 호를 사용 하려면 두 줄을 탄젠트 원에 있는 좌표를 결정 해야 합니다.

원 중 하나에 올바른 사각형을 생성 합니다.

![](arcs-images/infinitytriangle.png "두 개의 원을 접선와 포함 된 원")

원의 반지름은 100 단위 및 삼각형의 빗변 이므로 150 단위 각도 α 150 여 또는 41.8도 구분 100의 아크사인 (역 사인). 삼각형의 다른 쪽의 길이가 150 번 41.8 각도의 코사인 또는 112는 피타고라스 정리 하 여 계산할 수도 있습니다.

이 정보를 사용 하 여 탄젠트 지점의 좌표를 계산한 다음 있습니다.

`x = 112·cos(41.8) = 83`

`y = 112·sin(41.8) = 75`

4 개의 탄젠트 사항은 100 원 반지름을 사용 하 여 점 (0, 0)에 가운데 맞춤은 무한대 기호를 그릴 필요한 모든 것 같습니다.

![](arcs-images/infinitycoordinates.png "접선 및 좌표를 사용 하 여 두 개의 원")

`PaintSurface` 처리기에는 [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) 클래스의 무한대 기호를 배치 있도록는 (0, 0) 지점 페이지의 가운데에 배치 되 고 화면 크기에 대 한 경로 확장:

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

코드를 사용 하는 `Bounds` 속성의 `SKPath` 를 캔버스의 크기를 조정할 무한대 사인 값의 크기를 확인 하려면:

[![](arcs-images/arcinfinity-small.png "삼중 호 무한대 페이지 스크린샷")](arcs-images/arcinfinity-large.png#lightbox "삼중 호 무한대 페이지 스크린샷")

결과 것을 제안 하는 약간 작은 합니다 `Bounds` 의 속성 `SKPath` 경로 보다 큰 크기를 보고 합니다.

내부적으로 Skia 여러 정방형 베 지 어 곡선을 사용 하 여 호의 대략적으로 보여 줍니다. 이러한 곡선 (다음 섹션에서 확인할 수) 포함 제어점 곡선을 그리는 방법을 관리 하지만 렌더링된 곡선의 일부가 아닙니다. `Bounds` 속성에는 해당 컨트롤 지점이 포함 됩니다.

긴밀 하 게 맞춤을 사용 합니다 `TightBounds` 제어점을 제외 하는 속성입니다. 같습니다. 가로 모드에서 실행 되 고 사용 하 여 프로그램을 `TightBounds` 속성 경로 경계를 가져오려면:

[![](arcs-images/arcinfinitytightbounds-small.png "긴밀 하 게 범위를 사용 하 여 원호 무한대 페이지 스크린샷 삼중")](arcs-images/arcinfinitytightbounds-large.png#lightbox "삼중 긴밀 하 게 범위를 사용 하 여 원호 무한대 페이지 스크린샷")

직선 원호 사이의 연결을 수학적으로 부드러운 이지만, 직선 호에서 변경 잠시 갑작스러운 보일 수 있습니다. 더 나은 무한대 기호를 다음 문서에 표시 됩니다 [ **베 지 어 곡선의 세 가지 형식**](beziers.md)합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
