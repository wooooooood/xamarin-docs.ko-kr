---
title: 3가지 형식의 Bézier 곡선
description: 이 문서에서는 SkiaSharp를 사용 하 여 응용 프로그램에서 입방, 정방형 및 원추형 베 지 어 곡선을 렌더링 하는 방법을 설명 하 Xamarin.Forms 고 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: davidbritch
ms.author: dabritch
ms.date: 05/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1ad548846500ccbacc2a3d117919bfb4df1a1d79
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138686"
---
# <a name="three-types-of-bzier-curves"></a>3가지 형식의 Bézier 곡선

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp를 사용 하 여 입방, 정방형 및 원추형 베 지 어 곡선을 렌더링 하는 방법을 알아봅니다._

베 지 어 곡선은 자동차에 대 한 컴퓨터 기반 디자인의 곡선을 사용 하는 자동차 회사 Renault의 프랑스어 엔지니어 (1910 – 1999)로 이름이 지정 됩니다.

베 지 어 곡선은 대화형 디자인에 적합 하다는 것으로 알려져 있습니다. 즉 &mdash; , 곡선이 무한 하거나 singularities 하 &mdash; 고 일반적으로 보기 편 멋지고 하는 것은 아닙니다.

![샘플 베 지 어 곡선](beziers-images/beziersample.png)

컴퓨터 기반 글꼴의 문자 윤곽선은 일반적으로 베 지 어 곡선으로 정의 됩니다.

[**베 지 어 곡선**](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) 의 위키백과 문서에는 몇 가지 유용한 배경 정보가 포함 되어 있습니다. *베 지 어 곡선* 이라는 용어는 실제로 비슷한 곡선의 패밀리를 나타냅니다. SkiaSharp는 *입방*, *정방형*및 *원추형*이라는 세 가지 유형의 베 지 어 곡선을 지원 합니다. 원추형은 *유리수*로도 알려져 있습니다.

## <a name="the-cubic-bzier-curve"></a>입방 형 3 차원 곡선입니다.

입방는 베 지 어 곡선의 주체가 발생할 때 대부분의 개발자가 생각 하는 베 지 어 곡선의 유형입니다.

`SKPath` [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKPoint)) 세 개의 매개 변수를 사용 하는 메서드 `SKPoint` 또는 [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo(System.Single,System.Single,System.Single,System.Single,System.Single,System.Single)) 개별 `x` 및 매개 변수가 있는 오버 로드 `y` 를 사용 하 여 개체에 입방 형 3 차원 곡선을 추가할 수 있습니다.

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

곡선이 컨투어의 현재 지점에서 시작 됩니다. 전체 입방 형 3 차원 곡선은 4 개 요소로 정의 됩니다.

- 시작점: 윤곽선의 현재 지점 이거나, `MoveTo` 가 호출 되지 않은 경우 (0, 0)입니다.
- 첫 번째 제어 지점: `point1` `CubicTo` 호출에서
- 두 번째 제어점: `point2` 호출에서 `CubicTo`
- 끝점: `point3` `CubicTo` 호출에서

결과 곡선은 시작점에서 시작 하 여 끝 지점에서 끝납니다. 곡선은 일반적으로 두 개의 제어 요소를 통과 하지 않습니다. 대신 제어 지점은 곡선을 가까운 자석 하는 것과 매우 유사 합니다.

입방 형 3 차원 곡선에 대 한 느낌을 얻는 가장 좋은 방법은 실험입니다. 이는에서 파생 되는 **베 지 어 곡선** 페이지의 목적입니다 `InteractivePage` . [**BezierCurvePage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) 파일은 `SKCanvasView` 및를 인스턴스화합니다 `TouchEffect` . [**BezierCurvePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) 코드 숨김으로 만든 파일은 생성자에 4 개의 개체를 만듭니다 `TouchPoint` . `PaintSurface`이벤트 처리기는 `SKPath` 네 개의 개체를 기반으로 하 여 베 지 어 곡선을 렌더링 하는를 만들고 `TouchPoint` 제어점에서 끝점으로의 점선 접선을 그립니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with cubic Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.CubicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     touchPoints[3].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[2].Center.X,
                    touchPoints[2].Center.Y,
                    touchPoints[3].Center.X,
                    touchPoints[3].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
       touchPoint.Paint(canvas);
    }
}
```

실행 되 고 있습니다.

[![베 지 어 곡선 페이지의 세 번째 스크린샷](beziers-images/beziercurve-small.png)](beziers-images/beziercurve-large.png#lightbox)

수학적으로 곡선은 큐빅 다항식입니다. 곡선이 가장 많이 세 점에서 직선을 교차 합니다. 시작점에서 곡선은 항상와 동일한 방향으로 시작점과 시작점에서 첫 번째 제어점 사이의 직선으로 탄젠트 하 게 됩니다. 끝점에서 곡선은 항상와 동일한 방향으로 두 번째 제어점에서 끝점으로의 직선으로 탄젠트 하 게 됩니다.

입방 형 3 차원 곡선은 항상 4 개의 점을 연결 하는 볼록 사변형에 의해 제한 됩니다. 이를 *볼록 선체*라고 합니다. 시작점과 끝점 사이의 직선에 제어점이 있으면 베 지 어 곡선이 직선으로 렌더링 됩니다. 하지만 세 번째 스크린샷에서 보여 주는 것 처럼 곡선이 서로 교차 될 수도 있습니다.

경로 컨투어에는 연결 된 입방 형 3 차원 곡선을 여러 개 포함할 수 있지만, 다음 세 점이 colinear (즉, 직선으로 배치 됨) 경우에만 두 번째 입방 형 3 차원 곡선 사이의 연결이 부드럽게 됩니다.

- 첫 번째 곡선의 두 번째 제어점입니다.
- 첫 번째 곡선의 끝점으로, 두 번째 곡선의 시작점 이기도 합니다.
- 두 번째 곡선의 첫 번째 제어점입니다.

[**SVG 경로 데이터**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)에 대 한 다음 문서에서는 부드러운 연결 된 베 지 어 곡선을 쉽게 정의할 수 있는 기능을 검색 합니다.

경우에 따라 입방 형 3 차원 곡선을 렌더링 하는 기본 패라메트릭 수식을 알고 있으면 유용 합니다. 0에서 *1 사이의 경우* 패라메트릭 방정식은 다음과 같습니다.

x (t) = (1 – t) ³ x ₀ + 3t (1 – t) ² x ₁ + 3t ² (1 – t) x ₂ + t ³ x ₃

y (t) = (1 – t) ³ y ₀ + 3t (1 – t) ² y ₁ + 3t ² (1 – t) y ₂ + t ³ y ₃

3의 가장 높은 지 수는 이러한가 입방 polynomials 확인 합니다. 가 0이 고 `t` , 점이 (x ₀, y ₀)이 고, 시작점 이며,가 `t` 1 이면 지점은 (x ₃, y ₃) 끝점이 되는 것을 쉽게 확인할 수 있습니다. 시작점 근처 (낮은 값의 경우)에서 `t` 첫 번째 제어점 (x ₁, y ₁)은 강력한 효과를 가지 며 끝점 근처 (' t '의 높은 값)는 두 번째 제어점 (x ₂, y ₂)이 강력한 효과를 가집니다.

## <a name="bezier-curve-approximation-to-circular-arcs"></a>원형 원호와 베 지 어 곡선 근사값

때로는 베 지 어 곡선을 사용 하 여 원호를 렌더링 하면 편리 합니다. 입방 형 3 차원 곡선은 1/4 원으로 매우 잘 된 원호를 대략적으로 사용할 수 있으므로 4 개의 연결 된 3 차원 곡선은 전체 원을 정의할 수 있습니다. 이러한 근사값은 25 년 전에 게시 된 두 문서에서 설명 합니다.

> Dokken, et al, "곡률 연속 베 지 어 곡선," *컴퓨터와 기하학적 디자인 7* (1990), 33-41의 뛰어난 원

> Michael Goldapp, "원호를 입방 Polynomials로 근사 하 게 하는 경우", 컴퓨터에 지 *디자인 8* (1991), 227-238.

다음 다이어그램에서는, 및 라는 네 개의 점을 보여 주고 원호를 대략적으로 보여 `pto` `pt1` `pt2` `pt3` 주는 베 지 어 곡선 (빨간색으로 표시)을 정의 합니다.

![베 지 어 곡선의 원호 근사값](beziers-images/bezierarc45.png)

제어점에 대 한 시작점과 끝점의 선은 원과 베 지 어 곡선에 탄젠트 하 고 길이는 *L*입니다. 위에서 언급 한 첫 번째 문서는 해당 길이 *L* 이 다음과 같이 계산 되 면 베 지 어 곡선이 원호와 가장 잘 근접 함을 나타냅니다.

L = 4 × tan (α/4)/3

이 그림에서는 45 각도를 표시 하므로 L은 0.265과 같습니다. 코드에서 해당 값에 원하는 원의 반지름을 곱합니다.

**베 지 어 원형 호** 페이지를 사용 하 여 베 지 어 곡선을 정의 하 고 최대 180도 범위의 각도에 대해 원호를 대략적으로 테스트할 수 있습니다. [**BezierCircularArcPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) 파일은 `SKCanvasView` `Slider` 각도를 선택 하기 위해 및를 인스턴스화합니다. `PaintSurface` [**BezierCircularArgPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) 코드에서 이벤트 처리기는 변환을 사용 하 여 점 (0, 0)을 캔버스의 중심으로 설정 합니다. 비교를 위해 해당 지점을 중심으로 원을 그린 다음 베 지 어 곡선의 두 제어점을 계산 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 3;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate length of control point line
    float length = radius * 4 * (float)Math.Tan(Math.PI * angle / 180 / 4) / 3;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the end points
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point3 = new SKPoint(radius * sin, radius * cos);

    // Find the control points
    SKPoint point0Normalized = Normalize(point0);
    SKPoint point1 = point0 + new SKPoint(length * point0Normalized.Y,
                                          -length * point0Normalized.X);

    SKPoint point3Normalized = Normalize(point3);
    SKPoint point2 = point3 + new SKPoint(-length * point3Normalized.Y,
                                          length * point3Normalized.X);

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);
    canvas.DrawCircle(point3.X, point3.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point3.X, point3.Y, point2.X, point2.Y, dottedStroke);

    // Draw the Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.CubicTo(point1, point2, point3);
        canvas.DrawPath(path, redStroke);
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

```

시작점과 끝점 ( `point0` 및 `point3` )은 원의 일반 패라메트릭 방정식을 기반으로 계산 됩니다. 원이 (0, 0) 중심에 있기 때문에 이러한 지점은 원의 중심에서 원주로 방사형 벡터로 취급할 수도 있습니다. 제어점은 원에 탄젠트 한 선에 있으므로 이러한 방사형 벡터의 오른쪽 각도에 있습니다. 오른쪽 각도의 벡터는 X 및 Y 좌표가 교환 되 고 그 중 하나가 음수를 만든 원래 벡터입니다.

다른 각도로 실행 되는 프로그램은 다음과 같습니다.

[![베 지 어 원형 호 페이지의 세 번째 스크린샷](beziers-images/beziercirculararc-small.png)](beziers-images/beziercirculararc-large.png#lightbox)

세 번째 스크린샷에서 자세히 살펴보면, 각도가 180도 인 경우 베 지 어 곡선이 반원에서 어떻게 다른 지 확인 하는 것을 알 수 있지만,이 경우에는 해당 각도가 90도 인 경우에만 1/4 원을 만족 하는 것으로 보입니다.

분기 원이 다음과 같은 경우 두 제어 지점의 좌표를 계산 하기가 매우 쉽습니다.

![베 지 어 곡선을 사용 하는 사분기 근사 원](beziers-images/bezierarc90.png)

원의 반지름이 100 이면 *L* 은 55이 고 기억할 수 있는 쉬운 수입니다.

원 페이지를 **제곱** 하면 원과 사각형 사이에 그림에 애니메이션 효과가 적용 됩니다. 원은 클래스에서이 배열 정의의 첫 번째 열에 해당 좌표가 표시 되는 4 개의 베 지 어 곡선으로 표시 됩니다 [`SquaringTheCirclePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) .

```csharp
public class SquaringTheCirclePage : ContentPage
{
    SKPoint[,] points =
    {
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() },
        { new SKPoint(  55,  100), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,   55), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,    0), new SKPoint(   125,      0), new SKPoint() },
        { new SKPoint( 100,  -55), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(  55, -100), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(   0, -100), new SKPoint(     0,   -125), new SKPoint() },
        { new SKPoint( -55, -100), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,  -55), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,    0), new SKPoint(  -125,      0), new SKPoint() },
        { new SKPoint(-100,   55), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint( -55,  100), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() }
    };
    ...
}
```

두 번째 열에는 해당 영역이 원의 영역과 거의 같은 사각형을 정의 하는 4 개의 베 지 어 곡선 좌표가 포함 됩니다. 지정 된 원과 *정확한* 영역을 사용 하 여 사각형을 그리는 것은 [원을 제곱](https://en.wikipedia.org/wiki/Squaring_the_circle)하는 클래식 해결할 수 없는 기하학적 문제입니다. 베 지 어 곡선을 사용 하는 사각형을 렌더링 하는 경우 각 곡선에 대 한 두 제어점은 동일 하 고, 시작점과 끝점을 사용 하 여 colinear 베 지 어 곡선이 직선으로 렌더링 됩니다.

배열의 세 번째 열은 애니메이션에 대 한 보간된 값에 대 한 것입니다. 페이지는 타이머를 16 밀리초로 설정 하 고, `PaintSurface` 해당 속도에서 처리기를 호출 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    canvas.Translate(info.Width / 2, info.Height / 2);
    canvas.Scale(Math.Min(info.Width / 300, info.Height / 300));

    // Interpolate
    TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
    float t = (float)(timeSpan.TotalSeconds % 3 / 3);   // 0 to 1 every 3 seconds
    t = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;     // 0 to 1 to 0 sinusoidally

    for (int i = 0; i < 13; i++)
    {
        points[i, 2] = new SKPoint(
            (1 - t) * points[i, 0].X + t * points[i, 1].X,
            (1 - t) * points[i, 0].Y + t * points[i, 1].Y);
    }

    // Create the path and draw it
    using (SKPath path = new SKPath())
    {
        path.MoveTo(points[0, 2]);

        for (int i = 1; i < 13; i += 3)
        {
            path.CubicTo(points[i, 2], points[i + 1, 2], points[i + 2, 2]);
        }
        path.Close();

        canvas.DrawPath(path, cyanFill);
        canvas.DrawPath(path, blueStroke);
    }
}
```

지점은의 sinusoidally 진동 값에 따라 보간됩니다 `t` . 보간된 지점은 일련의 연결 된 3 차원 곡선을 생성 하는 데 사용 됩니다. 실행 되는 애니메이션은 다음과 같습니다.

[![원 페이지를 제곱 하는 세 번째 스크린샷](beziers-images/squaringthecircle-small.png)](beziers-images/squaringthecircle-large.png#lightbox)

이러한 애니메이션은 원호 및 직선으로 렌더링 될 수 있을 만큼 유연 하 게 알고리즘 방식으로 수 있는 곡선이 없으면 불가능 합니다.

**베 지 어 Infinity** 페이지는 또한 베 지 어 곡선을 사용 하 여 원호를 대략적으로 만듭니다. `PaintSurface`클래스의 처리기는 [`BezierInfinityPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) 다음과 같습니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.MoveTo(0, 0);                                // Center
        path.CubicTo(  50,  -50,   95, -100,  150, -100); // To top of right loop
        path.CubicTo( 205, -100,  250,  -55,  250,    0); // To far right of right loop
        path.CubicTo( 250,   55,  205,  100,  150,  100); // To bottom of right loop
        path.CubicTo(  95,  100,   50,   50,    0,    0); // Back to center  
        path.CubicTo( -50,  -50,  -95, -100, -150, -100); // To top of left loop
        path.CubicTo(-205, -100, -250,  -55, -250,    0); // To far left of left loop
        path.CubicTo(-250,   55, -205,  100, -150,  100); // To bottom of left loop
        path.CubicTo( -95,  100,  -50,   50,    0,    0); // Back to center
        path.Close();

        SKRect pathBounds = path.Bounds;
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.9f * Math.Min(info.Width / pathBounds.Width,
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

그래프 용지에 이러한 좌표를 그려 어떻게 관련 되어 있는지 확인 하는 것이 좋습니다. 무한대 기호는 점 (0, 0)을 중심으로 하 고, 두 루프는 (-150, 0) 및 (150, 0)의 중심과 100의 반지름을 갖습니다. 일련의 명령에서 `CubicTo` -95 및 – 205 (해당 값은 – 150 plus 55 및-), 205 및 95 (150 plus 및 빼기 55) 및 오른쪽 및 왼쪽에 대 한 250 및 – 250 값을 취하는 제어 지점의 X 좌표를 볼 수 있습니다. 유일한 예외는 무한대 기호가 중앙에서 자기 자신을 교차 하는 경우입니다. 이 경우 제어점은 가운데 근처의 곡선을 그리는 데 50 및 – 50 조합을 사용 하는 좌표를 포함 합니다.

Infinity 기호는 다음과 같습니다.

[![베 지 어 Infinity 페이지의 삼중 스크린샷](beziers-images/bezierinfinity-small.png)](beziers-images/bezierinfinity-large.png#lightbox)

[**호를 그리기 위한 세 가지 방법**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) 으로 **arc infinity** 페이지에서 렌더링 되는 무한대 부호 보다 중앙에서 약간 더 부드럽게 됩니다.

## <a name="the-quadratic-bzier-curve"></a>정방형 3 차원 곡선

정방형 3 차원 곡선에는 제어점이 하나 뿐 이며, 곡선은 시작점, 제어점 및 끝점의 세 가지 요소로 정의 됩니다. 패라메트릭 방정식은 최고 지수가 2 이므로 곡선이 정방형 3 차원 곡선과 매우 유사 하므로 곡선은 정방형 다항식입니다.

x (t) = (1 – t) ² x ₀ + 2t (1 – t) x ₁ + t ² x ₂

y (t) = (1 – t) ² y ₀ + 2t (1 – t) y ₁ + t ² y ₂

정방형 3 차원 곡선을 경로에 추가 하려면 [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint)) 메서드 또는 [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo(System.Single,System.Single,System.Single,System.Single)) 오버 로드를 별도의 `x` 및 좌표로 사용 합니다 `y` .

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

메서드는 현재 위치에서 `point2` 제어 지점으로 사용 하는 곡선을에 추가 합니다 `point1` .

3 차원 **곡선 페이지를** 사용 하 여 정방형 3 차원 곡선을 시험해 볼 수 있습니다 .이 페이지에는 3 개의 터치 지점만 있다는 점만 제외 하 고 **베 지 어 곡선** 페이지와 매우 비슷합니다. 다음은 `PaintSurface` [**QuadraticCurve.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) 코드 파일의 처리기입니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with quadratic Bezier
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.QuadTo(touchPoints[1].Center,
                    touchPoints[2].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

그리고 다음을 실행 합니다.

[![정방형 곡선 페이지의 세 번째 스크린샷](beziers-images/quadraticcurve-small.png)](beziers-images/quadraticcurve-large.png#lightbox)

점선은 시작점과 끝점에서 곡선에 탄젠트 하 고 제어 지점에서 충족 합니다.

정방형 베 지 어는 일반적인 모양의 곡선이 필요한 경우에 유용 하지만 두 개의 제어점을 사용 하지 않는 것이 좋습니다. 정방형 3 차원 곡선은 다른 곡선 보다 더 효율적으로 렌더링 됩니다. 즉, 타원 원호를 렌더링 하기 위해 서 기 Ia에서 내부적으로 사용 됩니다.

그러나 정방형 3 차원 곡선의 모양은 타원이 아니므로 타원 원호를 대략적으로 Béziers 하는 데 여러 정방형이 필요 합니다. 정방형 3 차원은 parabola의 세그먼트입니다.

## <a name="the-conic-bzier-curve"></a>원추형 베 지 어 곡선

원추형 정방형 3 차원 곡선으로 알려진 원추형 베 지 어 곡선은 &mdash; &mdash; 베 지 어 곡선 패밀리에 상대적으로 최근 추가 된 항목입니다. 정방형 3 차원 곡선 처럼, 유리 정방형 3 차원 곡선에는 시작점, 끝점 및 한 개의 제어 점이 포함 됩니다. 하지만, 유리수 정방형 3 차원 곡선에는 *가중치* 값도 필요 합니다. 패라메트릭 수식이 비율을 포함 하기 때문에이를 *유리수* 근 부릅니다.

X 및 Y의 패라메트릭 방정식은 같은 분모를 공유 하는 비율입니다. 0에서 1 사이의 분모와 가중치 값 *w*에 *대 한 분모* 의 수식은 다음과 같습니다.

d (t) = (1 – t) ² + 2wt (1 – t) + t ²

이론적으로, 유리수는 세 개의 용어 각각에 대해 하나씩 3 개의 개별 가중치 값을 포함할 수 있지만 이러한 값은 가운데 용어로 한 가중치 값으로만 간소화할 수 있습니다.

X 및 Y 좌표에 대 한 패라메트릭 방정식은 가운데 용어에 가중치 값도 포함 되 고 식이 분모로 나뉘는 점을 제외 하 고 정방형 3 차원에 대 한 패라메트릭 방정식과 비슷합니다.

x (t) = ((1 – t) ² x ₀ + 2wt (1 – t) x ₁ + t ² x ₂)) ÷ d (t)

y (t) = ((1 – t) ² y ₀ + 2wt (1 – t) y ₁ + t ² y ₂)) ÷ d (t)

유리 정방형 3 차원 곡선은 hyperbolas, parabolas, 줄임표 및 원의 모든 원추형 섹션의 세그먼트를 정확 하 게 나타낼 수 있으므로 *conics* 라고도 &mdash; 합니다.

경로에 유리수 정방형 3 차원 곡선을 추가 하려면 [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) 메서드 또는 [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo(System.Single,System.Single,System.Single,System.Single,System.Single)) 오버 로드를 별도의 `x` 및 좌표로 사용 합니다 `y` .

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

최종 `weight` 매개 변수를 확인 합니다.

**원추형 곡선** 페이지를 사용 하 여 이러한 곡선을 시험해 볼 수 있습니다. `ConicCurvePage` 클래스는 `InteractivePage`에서 파생됩니다. [**ConicCurvePage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) 파일은를 인스턴스화하여 `Slider` -2와 2 사이의 가중치 값을 선택 합니다. [**ConicCurvePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) 코드 숨김이 3 인 `TouchPoint` 개체를 만들고 `PaintSurface` 처리기는 단순히 접선을 사용 하 여 결과 곡선을 제어 점에 렌더링 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with conic curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.ConicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     (float)weightSlider.Value);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

실행 되 고 있습니다.

[![원추형 곡선 페이지의 세 번째 스크린샷](beziers-images/coniccurve-small.png)](beziers-images/coniccurve-large.png#lightbox)

여기에서 볼 수 있듯이 제어점은 가중치가 높을수록 곡선을 더 많이 끌어 옵니다. 가중치가 0 인 경우 곡선은 시작점에서 끝점으로 직선이 됩니다.

이론적으로는 음수 가중치를 사용할 수 있으며이로 인해 곡선이 제어 *지점에서 구부러지지* 않습니다. 그러나-1 이하의 가중치를 통해 패라메트릭 수식의 분모는 *t*의 특정 값에 대해 음수가 됩니다. 따라서 메서드에서 음수 가중치는 무시 됩니다 `ConicTo` . **원추형 곡선** 프로그램을 사용 하면 음수 가중치를 설정할 수 있지만 실험에서 볼 수 있듯이 음수 가중치는 가중치 0과 동일한 효과를 가지 며 직선의 렌더링을 발생 시킵니다.

메서드를 사용 하 여 `ConicTo` 반원 (포함 하지 않음)까지 원호를 그리기 위해 제어 지점과 가중치를 매우 쉽게 파생 시킬 수 있습니다. 다음 다이어그램에서는 시작점과 끝점의 접선 선이 제어 지점에서 충족 합니다.

![원호의 원추형 호 렌더링](beziers-images/conicarc.png)

삼각법를 사용 하 여 원의 중심에서 제어점의 거리를 결정할 수 있습니다. 원의 반지름은 α 각도의 절반을 코사인으로 나눈 것입니다. 시작점과 끝점 사이에서 원호를 그리려면 해당 각도의 절반을 동일한 코사인으로 가중치를 설정 합니다. 각도가 180 도이면 탄젠트 선은 충족 되지 않으며 가중치가 0입니다. 그러나 180도 보다 작은 각도의 경우 수학은 제대로 작동 합니다.

**원추형** 원호 페이지에서이를 보여 줍니다. [**ConicCircularArc**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) 파일은 `Slider` 각도를 선택 하기 위해를 인스턴스화합니다. `PaintSurface` [**ConicCircularArc.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) 코드 숨김이 지정 된 파일의 처리기는 제어 지점과 가중치를 계산 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 4;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the points and weight
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point1 = new SKPoint(0, radius / cos);
    SKPoint point2 = new SKPoint(radius * sin, radius * cos);
    float weight = cos;

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point2.X, point2.Y, point1.X, point1.Y, dottedStroke);

    // Draw the conic
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.ConicTo(point1, point2, weight);
        canvas.DrawPath(path, redStroke);
    }
}
```

여기에서 볼 수 있듯이 `ConicTo` 빨간색으로 표시 된 경로와 참조에 대해 표시 되는 기본 원 사이에는 시각적 차이점이 없습니다.

[![원추형 원호 페이지의 세 번째 스크린샷](beziers-images/coniccirculararc-small.png)](beziers-images/coniccirculararc-large.png#lightbox)

그러나 각도를 180도로 설정 하면 수학이 실패 합니다.

이론적으로는 `ConicTo` 패라메트릭 방정식을 기반으로 하므로 음수 가중치를 지원 하지 않는 것이 좋습니다 .이 경우에는 동일한 점이 있는에 대 한 다른 호출로 원을 완료할 수 `ConicTo` 있지만 가중치의 음수 값을 사용할 수 없습니다. 이렇게 하면 `ConicTo` 0도와 180도 사이의 각도를 기준으로 두 개의 곡선이 있는 전체 원을 만들 수 있습니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
