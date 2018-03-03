---
title: "호를 그리려면 다음 세 가지 방법"
description: "SkiaSharp를 사용 하 여 세 가지 방법으로 호를 정의 하는 방법을 알아봅니다"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/10/2017
ms.openlocfilehash: 236f5da78022d6f6482ed66ffd439c4cd15766a3
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="three-ways-to-draw-an-arc"></a>호를 그리려면 다음 세 가지 방법

_SkiaSharp를 사용 하 여 세 가지 방법으로 호를 정의 하는 방법을 알아봅니다_

원호를 곡선이 무한대 등록의 둥근된 부분와 같은 타원의 원 둘레에:

![](arcs-images/arcsample.png "무한대 기호")

해당 정의의 단순성, 불구 하 고 방법이 있으면 모든 요구를 충족 하는 그리기 호 함수를 정의 하 고 따라서 호를 그리려면는 가장 좋은 방법은의 그래픽 시스템 간에 합의 없습니다. 이러한 이유로 `SKPath` 하나만 방법 자체를 클래스는 제한 하지 않습니다.

`SKPath` 정의 `AddArc` 메서드, 서로 다른 5 `ArcTo` 메서드 및 두 명의 상대 `RArcTo` 메서드. 이러한 메서드는 매우 다른 세 가지 방법을 나타내는 호를 지정 하는 세 가지 범주로 분류 됩니다. 사용할 수 있는 호를 그릴 수 있는 그래픽을 사용 하 여이 호 적용 되는 방식 및 정의를 사용할 수 있는 정보에 따라 달라 집니다.

## <a name="the-angle-arc"></a>각도 호로 변환

타원 그리기 각도 호 방법은 되는 타원의 경계를 지정 하는 사각형을 지정 해야 합니다. 이 타원의 호 각도 타원 호 및 해당 길이의 시작 부분을 만드는 중심에서으로 표시 됩니다. 두 가지 방법이 각도 호를 그립니다. 이들은 [ `AddArc` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) 메서드 및 [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKRect/System.Single/System.Single/System.Boolean/) 메서드:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

이러한 메서드는 Android 동일 [ `AddArc` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.AddArc/p/Android.Graphics.RectF/System.Single/System.Single/) 및 [ `ArcTo` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.ArcTo/p/Android.Graphics.RectF/System.Single/System.Single/System.Boolean/) 메서드. IOS [ `AddArc` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArc/p/System.Boolean/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) 메서드 유사 되지만 호로 원의 원주에 제한 되어 보다 타원으로 일반화 합니다.

두 방법 모두로 시작는 `SKRect` 되는 타원의 크기와 위치를 정의 하는 값:

![](arcs-images/anglearcoval.png "각도 호를 시작 하는 타원")

호는이 타원의 원 둘레의 일부입니다.

`startAngle` 인수는 타원의 가운데에서 오른쪽에 그려집니다 가로줄 기준으로 시계 방향으로 회전 각도입니다. `sweepAngle` 기준으로 인수는는 `startAngle`합니다. 다음은 `startAngle` 및 `sweepAngle` 60 및 100 각도 값 각각:

![](arcs-images/anglearcangles.png "각도 호를 정의 하는 각도")

시작 각도에서 호의 시작 됩니다. 길이 스윕 각도 따라 결정 됩니다.

![](arcs-images/anglearchighlight.png "강조 표시 된 각도 호로 변환")

사용 하 여 경로에 추가 된 곡선의 `AddArc` 또는 `ArcTo` 메서드는 빨간색으로 여기에 표시 되는 타원의 둘레의 해당 부분에 단순히:

![](arcs-images/anglearc.png "단독으로 각도 호로 변환")

`startAngle` 또는 `sweepAngle` 인수는 음수가 될 수 있으며:의 양수 값에 대 한 시계 반대 방향으로 호는 `sweepAngle` 및 음수 값에 대 한 시계 반대 방향으로 합니다.

그러나 `AddArc` 않습니다 *하지* 닫힌된 윤곽선을 정의 합니다. 호출 하는 경우 `LineTo` 후 `AddArc`, 시점으로 호의 끝에서 선이 그려집니다는 `LineTo` 메서드와 동일한의 경우도 마찬가지입니다. `ArcTo`합니다.

`AddArc` 자동으로 새 윤곽선을 시작 하 고 한 호출에 기능적 `ArcTo` 의 마지막 인수와 `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

마지막 인수에 호출 되도록 `forceMoveTo`를 효과적으로 수행 하 고는 `MoveTo` 호의 시작 부분에서 호출 합니다. 새 윤곽선을 시작합니다. 즉 하지 에서처럼의 마지막 인수 `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

이 버전의 `ArcTo` 호의 시작 부분에 현재 위치에서 하는 선을 그립니다. 즉, 더 큰 윤곽선 가운데에 호 어딘가에 수 있습니다.

**각도 호** 페이지에서는 두 개의 슬라이더를 사용 하 여 시작이 지정 스윕 각도를 수 있습니다. XAML 파일이 두 개를 인스턴스화하고 `Slider` 요소 및 `SKCanvasView`합니다. `PaintCanvas` 의 처리기는 [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) 파일 타원과 두 개를 사용 하 여 호를 그립니다 `SKPaint` 필드로 정의 된 개체:

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

볼 수 있듯이 시작 각도 및 스윕 각도 둘 다 음수 값에 사용할 수 있습니다.

[![](arcs-images/anglearc-small.png "각도 호 페이지의 삼중 스크린샷")](arcs-images/anglearc-large.png "각도 호 페이지의 삼중 스크린샷")

호를 생성 하려면이 방법은 알고리즘 방식으로 가장 단순한 및 호를 설명 하는 매개 방정식 파생 하기 쉽습니다. 타원 및 시작 및 스윕 각도의 위치와 크기를 알고 있으면 원호의 시작점 및 끝점 수를 계산 간단한 삼각 함수를 사용 하 여.

x = 타원입니다. MidX + (타원형 합니다. 너비 / 2) * cos(angle)

y = 타원입니다. MidY + (타원형 합니다. 높이 / 2) * sin(angle)

`angle` 값 `startAngle` 또는 `startAngle + sweepAngle`합니다.

호를 정의 하는 두 개의 각도의 사용은 그리기, 예를 들어, 원형 차트 만들기를 원하는 호의 각도 길이 알고 있는 경우에 가장 적합 합니다. **쪼개진 원형 차트** 이 페이지를 보여 줍니다. [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) 클래스는 내부 클래스를 사용 하 여 일부 맞추어진된 데이터 및 색을 정의 합니다.

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

`PaintSurface` 처리기를 계산 하는 항목을 먼저 반복을 `totalValues` 번호입니다. 이 통해는 합계의 부분으로 각 항목의 크기를 확인할 수 있고 각도를 변환 하는 것 됩니다.

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

새 `SKPath` 각 원형 조각이 개체가 만들어집니다. 센터에서 줄의 경로 구성 되어 아니라면 `ArcTo` 호를 및 다른 줄에서 센터 결과를 다시 그리는 `Close` 호출 합니다. 이 프로그램을 이동 하 여 주십시오 센터에서 50 픽셀인 "쪼개기" 원형 조각을 표시 합니다. 해당 작업에는 각 조각에 대 한 스윕 각도의 중간점 방향에서 벡터를 필요합니다.

[![](arcs-images/explodedpiechart-small.png "쪼개진 원형 차트 페이지의 삼중 스크린샷")](arcs-images/explodedpiechart-large.png "쪼개진 원형 차트 페이지의 삼중 스크린 샷")

모양 "쪼개기" 없이 보려면 주석으로 `Translate` 호출:

[![](arcs-images/explodedpiechartunexploded-small.png "폭발적 없이 쪼개진 원형 차트 페이지의 삼중 스크린샷")](arcs-images/explodedpiechartunexploded-large.png "분해 없이 쪼개진 원형 차트 페이지의 삼중 스크린 샷")

## <a name="the-tangent-arc"></a>탄젠트 호로 변환

두 번째 유형의에서 지 원하는 호 `SKPath` 는 *탄젠트 호*, 호는 두 개의 연결 된 선 탄젠트 원의 원주 때문에 이렇게 부름 합니다.

탄젠트 호를 호출 하 여 경로에 추가 됩니다는 [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) 두 개의 메서드 `SKPoint` 매개 변수 또는 [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) 오버 로드와 별도 `Single` 에 대 한 매개 변수는 사항:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

이 `ArcTo` 메서드는는 PostScript [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) (PDF 문서 페이지 532) 함수 및 iOS [ `AddArcToPoint` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArcToPoint/p/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) 메서드.

`ArcTo` 방법은 점이 3 개:

- 경우는 곡선 또는 점 (0, 0)의 현재 지점 `MoveTo` 호출 되지 않았습니다
- 첫 번째 지점 인수는 `ArcTo` 라는 메서드는 *지점 코너*
- 두 번째 지점 인수 `ArcTo`라고 하는 *대상 지점*:

![](arcs-images/tangentarcthreepoints.png "탄젠트 호를 시작 하는 점이 3 개")

이러한 세 점을 연결 된 두 줄을 정의합니다.

![](arcs-images/tangentarcconnectinglines.png "탄젠트 호의 점이 3 개를 연결 하는 선")

동일 선상의 & #x 2014;는 세 가지 경우 즉, 동일한 직선 & #x 2014; 상에 있는 경우 원호가 그려집니다.

`ArcTo` 메서드에 포함 됩니다는 `radius` 매개 변수입니다. 이 원의 반지름을 정의합니다.

![](arcs-images/tangentarccircle.png "탄젠트 호 원")

타원에 대 한 탄젠트 호 일반화 되지 않으면 합니다.

두 줄을 임의의 각도로 충족 하는 경우 탄젠트 두 줄으로 구분 되 해당 원의 해당 줄 사이 삽입할 수 있습니다.

![](arcs-images/tangentarctangentcircle.png "두 줄 사이의 탄젠트 호 원")

윤곽선에 추가 되는 곡선에 지정 된 지점 중 하나를 변경 하지 않습니다는 `ArcTo` 메서드. 첫 번째 탄젠트 점 및 두 번째 탄젠트 점에서 끝나는 호를 직선 현재 지점에서 구성 됩니다.

![](arcs-images/tangentarchighlight.png "두 줄 사이의 강조 표시 된 탄젠트 호로 변환")

최종 직선 및 윤곽선에 추가 된 호 다음과 같습니다.

![](arcs-images/tangentarc.png "두 줄 사이의 강조 표시 된 탄젠트 호로 변환")

두 번째 탄젠트 점에서 윤곽을 계속할 수 있습니다.

**탄젠트 호** 페이지에서는 탄젠트 호 실험할 수 있습니다. 파생 되는 여러 페이지의 첫 번째 [ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/InteractivePage.cs), 유용한 몇 가지를 정의 하는 `SKPaint` 개체 하 고 수행 `TouchPoint` 처리:

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

`TangentArcPage` 클래스는 `InteractivePage`에서 파생됩니다. 생성자는 [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) 파일 인스턴스화 및 초기화에 대 한 책임은 `touchPoints` 배열과 설정 `baseCanvasView` (에서 `InteractivePage`)에 `SKCanvasView` 에서 개체를 인스턴스화할는 [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) 파일:

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

`PaintSurface` 처리기 사용 하 여는 `ArcTo` 터치 포인트를 기반으로 호를 그리려면 메서드 및 `Slider`, 알고리즘 방식으로 원 각도에 따라 계산 되지만:

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

다음은 **탄젠트 호** 세 플랫폼 모두에서 실행 되는 페이지:

[![](arcs-images/tangentarc-small.png "탄젠트 호 페이지의 삼중 스크린샷")](arcs-images/tangentarc-large.png "탄젠트 호 페이지의 삼중 스크린샷")

Windows Mobile 장치에서 세 점 모두가 거의 동일 선상의 및 호 매우 작습니다.

탄젠트 호는 둥근된 사각형 같은 둥근된 모서리를 만드는 데 적합 합니다. 때문에 `SKPath` 이미 포함 되어는 `AddRoundedRect` 메서드를는 **반올림 Heptagon** 페이지를 사용 하는 방법을 보여 줍니다 `ArcTo` 7 면 다각형의 모서리를 반올림 합니다. (코드는 모든 일반 다각형에 대 한 범용 화) 합니다.

`PaintSurface` 의 처리기는 [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) 클래스를 포함 하나 `for` heptagon, 및이 항목에서 7 개의 변의 중간점을 계산 하는 두 번째 7 정점을 계산 하는 루프 꼭지점입니다. 이러한 중간점은 다음 경로 생성 하는 데 사용 됩니다.

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

다음은 세 가지 플랫폼에서 실행 중인 프로그램입니다.

[![](arcs-images/roundedheptagon-small.png "반올림 Heptagon 페이지의 삼중 스크린샷")](arcs-images/roundedheptagon-large.png "반올림 Heptagon 페이지의 삼중 스크린샷")

## <a name="the-elliptical-arc"></a>타원형 호의

타원형 호를 호출 하 여 경로에 추가 됩니다는 [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/SkiaSharp.SKPoint/) 메서드 두 개 `SKPoint` 매개 변수 또는 [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/System.Single/System.Single/) 별도 X와 Y 좌표 오버 로드:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

타원형 호는와 일치는 [타원형 호](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) 그래픽 SVG (Scalable Vector) 및 유니버설 Windows 플랫폼에 포함 된 [ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) 클래스입니다.

이러한 `ArcTo` 메서드는 현재 지점 윤곽선의 두 점 사이의 호를 그립니다 마지막 매개 변수를 `ArcTo` 메서드 (에서 `xy` 매개 변수 또는 별도 `x` 및 `y` 매개 변수):

![](arcs-images/ellipticalarcpoints.png "타원형 호를 정의 하는 두 개의 점")

첫 번째 지점 매개 변수는 `ArcTo` 메서드 (`r`, 또는 `rx` 및 `ry`) 점이 아닌 전혀 대신; 되는 타원의 가로 및 세로 반지름을 지정 하지만

![](arcs-images/ellipticalarcellipse.png "타원형 호 정의 타원")

`xAxisRotate` 매개 변수는 시계 방향으로 90이이 타원의 수:

![](arcs-images/ellipticalarctiltedellipse.png "기운된 타원 타원형 호를 정의 합니다.")

이 기운된 타원이 두 요소를 건드리면 되도록 배치, 여 요소를를 갖는 두 개의 서로 다른 연결 됩니다.

![](arcs-images/ellipticalarcellipse1.png "타원형 호의 첫 번째 집합")

두 가지 방법으로 이러한 두 개의 원호를 구별할 수: 최상위 호 아래쪽 호 보다 크면 및 시계 반대 방향 아래쪽 호를 그릴지 동안 시계 방향으로 맨 위 호를 그릴 왼쪽에서 오른쪽으로 호를 그릴 때.

다른 방식으로 두 점 사이의 타원을 맞출 수 이기도 합니다.

![](arcs-images/ellipticalarcellipse2.png "타원형 호의 두 번째 집합")

이제는 시계 방향으로 그려진 위에 작은 호 그려진 아래쪽에 더 큰 호 시계 반대 방향으로.

이러한 두 요소에서 네 가지 방법 중 총에서 기운된 타원에 의해 정의 되는 호 따라서 연결할 수 있습니다.

![](arcs-images/ellipticalarccolors.png "모든 4 개의 타원형 호")

이러한 네 가지 호의 4 가지 조합에 의해 구별 됩니다는 [ `SKPathArcSize` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathArcSize/) 및 [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/) 에 열거형 형식 인수는 `ArcTo` 메서드:

- 빨간색: SKPathArcSize.Large 및 SKPathDirection.Clockwise
- 녹색: SKPathArcSize.Small 및 SKPathDirection.Clockwise
- 파란색: SKPathArcSize.Small 및 SKPathDirection.CounterClockwise
- 자홍: SKPathArcSize.Large 및 SKPathDirection.CounterClockwise

기운된 타원 아닌 경우 두 점 사이 맞게 충분히 큰 충분히 큰 될 때까지 크기가 조정 균일 하 게 합니다. 두 개의 고유 원호는이 경우 두 개의 요소를 연결합니다. 이러한와 구분할 수는 `SKPathDirection` 매개 변수입니다.

호를 정의 합니다.이 방법은 처음에 복잡 한 말 있지만 회전 된 타원으로 호를 정의할 수 있습니다 하는 유일한 방법은 되며 것이 가장 쉬운 방법으로 호 윤곽선의 다른 부분과 통합 해야 하는 경우.

**타원형 호** 페이지를 사용 하면 대화형으로 두 개의 점을 크기, 타원의 회전을 설정할 수 있습니다. `EllipticalArcPage` 클래스에서 파생 됩니다 `InteractivePage`, 및 `PaintSurface` 의 처리기는 [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) 코드 숨김 파일 4 개 호를 그립니다.

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

다음 세 가지 플랫폼에서 실행 중인:

[![](arcs-images/ellipticalarc-small.png "타원형 호 페이지의 삼중 스크린샷")](arcs-images/ellipticalarc-large.png "타원형 호 페이지의 삼중 스크린샷")

**호 무한대** 페이지 무한대 기호를 그리는 데 타원형 호를 사용 합니다. 무한대 기호 반지름 100 단위 100 건으로 구분 된 2 개의 원을 기반으로 합니다.

![](arcs-images/infinitycircles.png "두 개의 원")

서로 교차 하는 두 줄은 두 원형으로 탄젠트:

![](arcs-images/infinitycircleslines.png "탄젠트 줄이 포함 된 두 개의 원")

무한대 기호는 부품 이러한 원과 두 줄의 조합입니다. 타원형 호를 그릴 무한대 기호를 사용 하려면 두 줄 고 원에 탄젠트 있는 좌표를 결정 해야 합니다.

원 중 하나에 올바른 사각형을 생성 합니다.

![](arcs-images/infinitytriangle.png "탄젠트 선 및 포함 된 circle 있는 두 개의 원")

원의 반지름은 100 단위 및 삼각형의 빗변 관계는 150 단위 각도 α 150, 또는 41.8도 나눈 값 100의 아크사인 값 (역 사인). 삼각형의 다른 쪽의 길이 150 번 41.8 각도의 코사인 또는 112 있는 피타고라스의 계산할 수도 있습니다.

이 정보를 사용 하 여 탄젠트의 좌표를 계산한 다음 있습니다.

x = 112·cos(41.8) = 83

y = 112·sin(41.8) = 75

4 개의 탄젠트 사항이 100의 원 반지름을 사용 하 여 지점 (0, 0)에서 가운데 맞춤은 무한대 기호를 그리려면 하기만 합니다.

![](arcs-images/infinitycoordinates.png "탄젠트 선 좌표가와 두 개의 원")

`PaintSurface` 처리기에는 [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) 클래스 무한대 기호를 배치 하는 (0, 0) 지점 페이지의 가운데에 배치 하 고 화면 크기에 대 한 경로 조정:

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

코드를 사용 하 여는 `Bounds` 속성 `SKPath` 캔버스의 크기 조정을 무한대 사인 값의 크기를 확인 하려면:

[![](arcs-images/arcinfinity-small.png "호 무한대 페이지의 삼중 스크린샷")](arcs-images/arcinfinity-large.png "호 무한대 페이지의 삼중 스크린샷")

결과 제안 하는 약간 작은 같습니다는 `Bounds` 속성 `SKPath` 경로 보다 큰 크기를 보고 합니다.

내부적으로 Skia 근사치 정방형 여러 베 지 어 곡선을 사용 하 여 호를 계산 합니다. 이러한 곡선 (다음 섹션에 표시)으로 렌더링 된 곡선의 일부가 아닌 하지만 곡선 그려지는 방법을 제어 하는 제어점 포함 합니다. `Bounds` 속성이 제어점을 포함 합니다.

긴밀 하 게 맞춤을 사용은 `TightBounds` 제어점을 제외 하는 속성입니다. 같습니다. 가로 모드에서 실행 되 고 사용 하 여 프로그램은 `TightBounds` 속성 경로 경계를 가져오려면:

[![](arcs-images/arcinfinitytightbounds-small.png "긴밀 하 게 한을 가진 호 무한대 페이지의 삼중 스크린샷")](arcs-images/arcinfinitytightbounds-large.png "긴밀 하 게 한을 가진 호 무한대 페이지의 삼중 스크린 샷")

원호 및 직선 간의 연결 옵션은 수학적으로 부드러운에 호 직선 하 여 변경을 약간 상황이 뜻하지 않게 보일 수 있습니다. 더 나은 무한대 기호를 다음 페이지에 표시 됩니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
