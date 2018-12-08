---
title: 3가지 형식의 Bézier 곡선
description: 이 문서에서는 SkiaSharp 사용 하 여 Xamarin.Forms 응용 프로그램에서 입방 형 3, 정방형 및 원추형 베 지 어 곡선을 렌더링 하는 방법에 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: davidbritch
ms.author: dabritch
ms.date: 05/25/2017
ms.openlocfilehash: 58cf11b2a88e0c399ee197e9c8365d7deafd0f39
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055480"
---
# <a name="three-types-of-bzier-curves"></a>3가지 형식의 Bézier 곡선

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_SkiaSharp 사용 하 여 입방 형 3, 정방형 및 원추형 베 지 어 곡선을 렌더링 하는 방법_

베 지 어 곡선 피에르 베 지 어 (1910 – 1999)의 자동차 본문의 컴퓨터 기반 디자인의 곡선을 사용 하는 자동차 회사에서 프랑스어 엔지니어 후 이라고 합니다.

대화형 디자인에 잘 맞는 것으로 알려져는 베 지 어 곡선: 정상적으로 작동 되기 &mdash; 즉, 무한 또는 다루기 될 곡선을 일으키는 singularities 없습니다 &mdash; 시선을 일반적으로 되며 :

![](beziers-images/beziersample.png "샘플 베 지 어 곡선")

컴퓨터 기반 글꼴의 문자 윤곽선은 일반적으로 베 지 어 곡선을 사용 하 여 정의 됩니다.

에 대 한 Wikipedia 문서 [ **베 지 어 곡선** ](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) 몇 가지 유용한 배경 정보를 포함 합니다. 용어 *베 지 어 곡선* 실제로 유사한 곡선의 제품군을 가리킵니다. SkiaSharp 호출 되는 베 지 어 곡선의 세 가지 종류를 지원 합니다 *입방 형 3*의 *정방형*, 및 *원추형*합니다. 원추형이 라고도 합니다 *rational 값*합니다.

## <a name="the-cubic-bzier-curve"></a>3 차원 큐빅 곡선

3 차 방정식에 베 지 어 곡선의 제목을 표시 되 면 대부분의 개발자의 생각 하는 베 지 어 곡선의 형식입니다.

입방 형 3 차원 큐빅 곡선으로 추가할 수 있습니다는 `SKPath` 를 사용 하 여 개체를 [ `CubicTo` ](xref:SkiaSharp.SKPath.CubicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKPoint)) 3 개를 사용 하 여 메서드 `SKPoint` 매개 변수 또는 [ `CubicTo` ](xref:SkiaSharp.SKPath.CubicTo(System.Single,System.Single,System.Single,System.Single,System.Single,System.Single)) 별도의오버로드`x` 고 `y` 매개 변수:

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

곡선은 윤곽선의 현재 위치에서 시작합니다. 전체 입방 형 3 차원 곡선 네 지점에서 정의 됩니다.

- 시작점: 현재 지정 된 윤곽선 또는 점 (0, 0) 경우 `MoveTo` 호출 되지 않았습니다
- 먼저 제어 지점: `point1` 에 `CubicTo` 호출
- 두 번째 제어점: `point2` 에 `CubicTo` 호출
- 끝점: `point3` 에 `CubicTo` 호출

결과 곡선 시작 지점에서 시작 및 끝 지점에서 끝납니다. 곡선을 일반적으로 두 개의 제어점;를 통해 전달 되지 않습니다. 대신 컨트롤 자신 들을 위한 곡선을 가져오려고 자석 마찬가지로 함수를 가리킵니다.

가장 좋은 방법은 입방 형 3 차원 큐빅 곡선에 대해 실험을 통해 됩니다. 용도이 **베 지 어 곡선** 에서 파생 되는 페이지 `InteractivePage`합니다. 합니다 [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) 파일은 합니다 `SKCanvasView` 및 `TouchEffect`합니다. 합니다 [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) 코드 숨김 파일 네 개를 만듭니다 `TouchPoint` 생성자에서는 개체입니다. 합니다 `PaintSurface` 이벤트 처리기를 만듭니다를 `SKPath` 기준으로 4 개의 베 지 어 곡선을 렌더링 하 `TouchPoint` 개체 이며 또한 엔드포인트로 제어 지점에서 점으로 구분 된 접선을 그립니다.

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

여기이 실행 됩니다.

[![](beziers-images/beziercurve-small.png "베 지 어 곡선 페이지 스크린샷 삼중")](beziers-images/beziercurve-large.png#lightbox "삼중 베 지 어 곡선 페이지 스크린샷")

수학적으로 곡선이 입방 형 3 다항식입니다. 곡선 최대 세 지점에 직선을 교차 합니다. 시작 시점에 곡선이 항상 탄젠트와 같은 방향으로에서, 시작까지에서 직선을 가리키는 첫 번째 제어점입니다. 끝점에서 곡선이 항상 탄젠트와 같은 방향으로에서, 두 번째 컨트롤까지에서 직선을 가리키는 끝점입니다.

입방 형 3 차원 큐빅 곡선은 항상 네 개의 점을 연결 하는 볼록 사변형에 의해 제한 됩니다. 이 호출 되는 *(convex hull)* 합니다. 제어점 사이의 출발점 및 도착 점에 직선에 놓여, 베 지 어 곡선 직선으로 렌더링 합니다. 하지만 곡선 수도 교차 자체 세 번째 스크린샷에서 보여 주듯이 합니다.

경로 윤곽선에는 여러 연결 된 3 차원 큐빅 곡선을 포함할 수 있지만 두 3 차원 큐빅 곡선 사이의 연결 다음 세 가지 사항을 동일 선상의 경우에 부드러운 됩니다 (즉, 직선에 놓여):

- 첫 번째 곡선의 둘째 제어점
- 두 번째 곡선의 시작점 이기도 하는 첫 번째 곡선의 끝점
- 두 번째 곡선의 첫째 제어점

다음 문서의 [ **SVG 경로 데이터**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md), 매끄러운 연결 된 베 지 어 곡선의 정의 쉽게 하는 기능을 살펴봅니다.

경우에 따라 입방 형 3 차원 큐빅 곡선을 렌더링 하는 기본 매개 방정식을 알고는 것이 유용 합니다. 에 대 한 *t* 0에서 1 사이의 매개 방정식은 다음과 같습니다.

x(t) = (1 – t)³x₀ + 3t(1 – t)²x₁ + 3t²(1 – t)x₂ + t³x₃

y(t) = (1-t) ³y₀ + 3t (1-t) ²y₁ + 3t² (1-t) y₂ + t³y₃

3의 가장 높은 지 수 입방 형 3 polynomials 이들은 확인 합니다. 때를 확인 하기 쉽습니다 `t` 가 0 이면 요점은 (x₀ y₀) 시작 지점이 있는 시점과 `t` 가 1 이면 요점은 (x₃ y₃) 끝점에는 합니다. 시작점을 거의 (낮은 값에 대 한 `t`), (x₁, y₁) 첫 번째 제어점에 강력한 적용 하 고 근처 끝점 (높은 값 ' t ') (x₂, y₂) 두 번째 제어점 강력한 영향을 미칩니다.

## <a name="bezier-curve-approximation-to-circular-arcs"></a>원호를 베 지 어 곡선 근사치

경우에 따라 원호를 렌더링 하는 베 지 어 곡선을 사용 하는 것이 유용 합니다. 입방 형 3 차원 큐빅 곡선 수를 추정 원호를 잘 사분기 원 최대 하므로 연결 된 4 개의 베 지 어 곡선 전체 원을 정의할 수 있습니다. 이 근사치는 25 년 전에 게시 하는 두 개의 문서에 설명 되어 있습니다.

> Tor Dokken, et al "과 유사한 추정치 곡률 연속 베 지 어 곡선에서 원 중" *컴퓨터 지원 된 기하학적 디자인 7* 33: 41 (1990).

> "입방 형 3 Polynomials 여 원호 근사값", Michael Goldapp *컴퓨터 지원 기하학적 디자인 8* (1991 년) 227 238 합니다.

다음 다이어그램은 레이블이 지정 된 점이 4 개를 보여 줍니다. `pto`, `pt1`를 `pt2`, 및 `pt3` 원호를 대략적으로 보여 주는 (빨간색으로 표시) 베 지 어 곡선을 정의 합니다.

![](beziers-images/bezierarc45.png "원호를 베 지 어 곡선으로의 근사값")

시작 및 끝 지점에서 줄 제어점을 원 하 베 지 어 곡선의 접선 하며 길이가 *L*합니다. 위에 언급 된 첫 번째 문서 나타냅니다 최상의 베 지 어 곡선 원호를 대략적으로 보여 줍니다 경우 해당 길이 *L* 다음과 같이 계산 됩니다.

L = 4 × tan(α / 4) / 3

그림은 45도 각도 이므로 L 0.265와 같습니다. 코드에서 해당 값 원의 반지름을 원하는 곱하여 지정 됩니다.

합니다 **베 지 어 원호** 페이지 정의 근사치를 180도 범위의 각도 대 한 원호를 베 지 어 곡선을 실험할 수 있습니다. [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) 파일은 합니다 `SKCanvasView` 및 `Slider` 각도 선택 합니다. 합니다 `PaintSurface` 이벤트 처리기는 [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) 코드 숨김 파일 변환을 사용 하 여 캔버스의 가운데에 점 (0, 0)을 설정 합니다. 비교를 위해 해당 지점에 중심이 있는 원을 그리는 하 고 베 지 어 곡선에 대 한 두 개의 제어점을 계산 합니다.

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

시작점과 끝점 (`point0` 고 `point3`) 원에 대 한 일반 파라메트릭 수식에 따라 계산 됩니다. 원을 중심으로 하기 때문에 (0, 0) 이러한 지점이 처리 될 수도 방사형 벡터로 원의 중심에서 원주를 합니다. 제어점이 방사형 벡터에 직각 되므로 탄젠트 값은 원형에 있는 줄에 있습니다. 다른 직각을 이루어야 벡터는 X 및 Y 좌표는 교환, 음수 수행 중 하나를 사용 하 여 원래 벡터 하기만 하면 됩니다.

다른 각도 사용 하 여 실행 중인 프로그램이 다음과 같습니다.

[![](beziers-images/beziercirculararc-small.png "베 지 어 원호 페이지 스크린샷 삼중")](beziers-images/beziercirculararc-large.png#lightbox "삼중 베 지 어 원호 페이지 스크린샷")

세 번째 스크린샷에서 자세히 살펴보고 및 각도 180도 하지만 iOS 화면 표시 각도가 90도 때 제대로 1/4 원에 맞게 많다고 베 지 어 곡선 특히 반원에서 벗어납니다 표시 됩니다.

1/4 원 같이 방향인 경우 두 개의 제어점 좌표를 계산 하는 것은 매우 쉽습니다.

![](beziers-images/bezierarc90.png "베 지 어 곡선을 사용 하 여 사분기 원의 근사값")

원의 반지름은 100 *L* 55, 이며 기억 하기 쉬운는 수입니다.

합니다 **원을 더하거나 제곱** 원형 및 사각형 사이의 그림을 페이지에 애니메이션을 적용 합니다. 원은 좌표가이 배열 정의의 첫 번째 열에 표시 됩니다는 4 개의 베 지 어 곡선으로 근사치를 계산 하는 [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) 클래스:

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

두 번째 열 영역 같습니다 약 원의 면적 사각형을 정의 하는 4 개의 베 지 어 곡선의 좌표를 포함 합니다. (사용 하 여 사각형 그리기 합니다 *정확한* 지정 원으로 영역은의 기하학적 문제 해결할 수 없는 클래식 [원을 더하거나 제곱](https://en.wikipedia.org/wiki/Squaring_the_circle).) 베 지 어 곡선을 사용 하 여 사각형을 렌더링 하는 것에 대 한 각 곡선에 대 한 두 개의 제어점 동일 하 고가 시작 및 끝 지점을 사용 하 여 동일 선상의 베 지 어 곡선 직선으로 렌더링 됩니다.

배열의 세 번째 열은 애니메이션의 보간된 값입니다. 16 밀리초 타이머를 설정 하는 페이지 및 `PaintSurface` 속도 처리기가 호출 됩니다.

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

점을 sinusoidally 진동 값에 따라 보간 `t`합니다. 일련의 연결 된 4 개의 베 지 어 곡선을 생성 하는 보간된 지점 사용 됩니다. 애니메이션을 다음과 같습니다.

[![](beziers-images/squaringthecircle-small.png "삼중 스크린샷은 Squaring 원 페이지")](beziers-images/squaringthecircle-large.png#lightbox "삼중 스크린샷은 Squaring 원 페이지")

이러한 애니메이션은 원호와 직선으로 렌더링할 수 있을 만큼 유연 알고리즘 곡선은 없이 수 없습니다.

합니다 **베 지 어 무한대** 페이지도는 근사치 원호를 베 지 어 곡선의 기능을 사용 합니다. 다음은 `PaintSurface` 에서 처리기는 [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) 클래스:

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

그래프 문서에 관련성을 확인 하려면 이러한 좌표를 그릴 좋은 연습을 수 있습니다. 점 (0, 0)을 중심으로 무한대 기호는 있고 두 개의 루프 센터 (–150, 0) 및 (150, 0) 및 100의 반지름입니다. 일련의 `CubicTo` 명령을 볼 수 있습니다 –95 및 –205 값에서 수행 하는 제어점 좌표 X (해당 값은 –150 더하기 및 빼기 55) 205 및 95 (150 더하기 및 빼기 55) 뿐만 아니라 250 –250 왼쪽과 오른쪽에 대 한 합니다. 무한대 기호를 센터에서 자체 교차 하는 경우 유일한 예외가입니다. 에 없는 경우에 제어점 50 및 –50 곡선 가운데 근처에 있는 초과 맞춤을 조합 하 여 좌표입니다.

무한대 기호는 다음과 같습니다.

[![](beziers-images/bezierinfinity-small.png "베 지 어 무한대 페이지 스크린샷 삼중")](beziers-images/bezierinfinity-large.png#lightbox "삼중 베 지 어 무한대 페이지 스크린샷")

것이 다소 원활 하 게 중심에서 렌더링 된 무한대 기호를 **Arc 무한대** 에서 페이지를 [ **호를 그리려면 세 가지 방법으로** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) 문서.

## <a name="the-quadratic-bzier-curve"></a>정방형 베 지 어 곡선

정방형 베 지 어 곡선 하나만 제어점 있고 곡선은 3 개의 점으로 정의 됩니다: 시작점, 제어 지점 및 끝 지점입니다. 파라메트릭 수식 비슷합니다 입방 형 3 차원 큐빅 곡선을 곡선 2 차 다항식 이므로 가장 높은 지 수는 2:

x(t) = (1-t) ²x₀ + 2t (1-t) x₁ + t²x₂

y(t) = (1-t) ²y₀ + 2t (1-t) y₁ + t²y₂

정방형 베 지 어 곡선 경로를 추가 하려면 사용 합니다 [ `QuadTo` ](xref:SkiaSharp.SKPath.QuadTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint)) 메서드 또는 [ `QuadTo` ](xref:SkiaSharp.SKPath.QuadTo(System.Single,System.Single,System.Single,System.Single)) 별도의 오버 로드 `x` 고 `y` 좌표:

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

메서드를 현재 위치에서 곡선을 추가 `point2` 사용 하 여 `point1` 제어 지점입니다.

정방형 베 지 어 곡선을 시험해 볼 수는 **정방형 곡선** 매우 비슷한 페이지는 **베 지 어 곡선** 제외 하면 세 개의 터치 포인트는 해당 페이지. 다음은 `PaintSurface` 처리기에는 [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) 코드 숨김 파일:

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

및 여기 실행 됩니다.

[![](beziers-images/quadraticcurve-small.png "정방형 곡선 페이지 스크린샷 삼중")](beziers-images/quadraticcurve-large.png#lightbox "삼중 정방형 곡선 페이지 스크린샷")

점선은 시작점 및 끝점에서 곡선 탄젠트 되며 제어점에 충족 합니다.

정방형 베 지 어 곡선, 일반 도형 필요 하지만 두 가지가 아닌 하나의 제어 지점에서 편리 하 게 하려는 경우 적합 합니다. 정방형 베 지 어 모든 다른 곡선,이 내부적으로 Skia 타원형 호를 렌더링 하는 것 보다 더 효율적으로 렌더링 합니다.

그러나 정방형 베 지 어 곡선의 모양이 타원형, 여러 정방형 베 지 어 타원형 호를 예상 하는 데 필요한 이유는 합니다. 정방형 베 지 어는을 포물선 세그먼트입니다.

## <a name="the-conic-bzier-curve"></a>원추형 베 지 어 곡선

원추형 베 지 어 곡선 &mdash; 라고도 rational 정방형 베 지 어 곡선 &mdash; 베 지 어 곡선의 제품군에 비교적 최근에 추가 됩니다. 정방형 베 지 어 곡선의 같은 rational 정방형 베 지 어 곡선 시작점, 끝점 및 하나의 제어점 포함 됩니다. Rational 정방형 베 지 어 곡선 해야 하지만 한 *가중치* 값입니다. 라고는 *rational* 정방형 파라메트릭 수식 비율을 포함 하기 때문입니다.

파라메트릭 수식에 대 한 X 및 Y 동일한 분모를 공유 하는 비율입니다. 분모를 계산 하는 식은 다음과 같습니다 *t* 에서 0부터 1 및의 가중치 *w*:

d(t) = (1-t) ² + 2wt(1 – t) + t²

이론적으로 rational 값에는 세 가지 용어가 마다 하나씩 세 개의 별도 가중치 값을 포함 될 수 있습니다 하지만 이러한 중간 용어에 가중치 값이 하나만으로 단순화할 수 있습니다.

X 및 Y 좌표에 대 한 매개 방정식 중간 용어에 가중치 값도 포함 되어 있습니다 식 분모로 나눈가 있다는 점을 제외 하면 정방형 3 차원 곡선에 대 한 매개 방정식와 비슷합니다.

x(t) = ((1 – t) ²x₀ + 2wt (1-t) x₁ + t²x₂)) ÷ d(t)

y(t) = ((1 – t) ²y₀ + 2wt (1-t) y₁ + t²y₂)) ÷ d(t)

Rational 정방형 베 지 어 곡선 이라고 *conics* 원추형 섹션의 세그먼트가 정확히 나타낼 수 있으므로 &mdash; hyperbolas, parabolas, 타원, 및 원입니다.

경로에 유리 정방형 베 지 어 곡선을 추가 하려면 사용 합니다 [ `ConicTo` ](xref:SkiaSharp.SKPath.ConicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) 메서드 또는 [ `ConicTo` ](xref:SkiaSharp.SKPath.ConicTo(System.Single,System.Single,System.Single,System.Single,System.Single)) 별도의 오버 로드 `x` 및 `y` 좌표:

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

마지막 확인 `weight` 매개 변수입니다.

합니다 **원추형 곡선** 페이지에서는 이러한 곡선을 실험할 수 있습니다. `ConicCurvePage` 클래스는 `InteractivePage`에서 파생됩니다. 합니다 [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) 파일은을 `Slider` -2에서 2 사이의 가중치 값을 선택 합니다. 합니다 [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) 코드 숨김 파일 세 개를 만듭니다 `TouchPoint` 개체 및 `PaintSurface` 처리기 컨트롤에 접선을 사용 하 여 결과 곡선 렌더링 사항은 다음과 같습니다.

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

여기이 실행 됩니다.

[![](beziers-images/coniccurve-small.png "삼중 원추형 곡선 페이지 스크린샷")](beziers-images/coniccurve-large.png#lightbox "삼중 원추형 곡선 페이지 스크린샷")

알 수 있듯이 제어점 가중치가 높은 경우 자세한으로 곡선을 끌어오는 것 같습니다. 가중치가 0 인 곡선 직선 시작 점에서 끝점 됩니다.

이론적으로 음수 가중치의 허용 되지 않으며 변형할 곡선 발생할 *떨어진* 제어점에서입니다. 그러나 가중치를-1 또는 원인을 아래 매개 방정식의 특정 값에 대 한 음수 되려면 분모 *t*합니다. 아마도 이러한 이유로 음수 가중치에서는 무시 되는 `ConicTo` 메서드. 합니다 **원추형 곡선** 프로그램을 사용 하면 음수 가중치를 설정 하지만 보면서 보시 음수 가중치의 가중치가 0 인 것과 같습니다 있고 렌더링할 직선을 발생 합니다.

제어점과 가중치를 사용 하 여 파생 쉽게는 `ConicTo` 원호를 그리는 방법 (하지만 포함 하지 않음) 반원입니다. 다음 다이어그램은 시작점과 끝점에서 접선 제어점에 충족 합니다.

![](beziers-images/conicarc.png "원호의 원추 호 렌더링")

삼각 함수를 사용 하 여 제어점 원 중심에서의 거리를 확인 하려면: 절반 각도 α의 코사인 값으로 나눈 원의 반지름을 것입니다. 시작점과 끝점 간의 원호를 그릴 절반 각도의 코사인 해당 동일한 값에 가중치를 설정 합니다. 유의 각도가 180도 인 경우 다음의 접선을 충족 하지 가중치는 0 하십시오. 하지만 180도 보다 작은 각도 대 한 수학 정상적으로 작동 합니다.

합니다 **원추형 원호** 이 페이지를 보여 줍니다. 합니다 [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) 파일은을 `Slider` 각도 선택 합니다. 합니다 `PaintSurface` 처리기에는 [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) 제어점 및 무게를 계산 하는 코드 숨김 파일:

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

알 수 있듯이 차이가 없는 visual는 `ConicTo` 빨간색으로 표시 하는 경로 및 기본 원을 참조용으로 표시 합니다.

[![](beziers-images/coniccirculararc-small.png "삼중 원추형 원호 페이지 스크린샷")](beziers-images/coniccirculararc-large.png#lightbox "삼중 원추형 원호 페이지 스크린샷")

하지만 180도 수학 실패 하는 각도 설정 합니다.

이 경우에 상당히 아쉬운 점입니다 `ConicTo` (파라메트릭 수식에 따라) 이론적으로 원을를 호출 하 여 완료할 수 있습니다 때문에 음수 가중치를 지원 하지 않습니다 `ConicTo` 선만 같지만 가중치 값이 음수입니다. 그러면 두를 사용 하 여 전체 원 만들기 `ConicTo` 곡선 사이 (포함 되지 않음) 모든 각도에 따라 0도 180도 합니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
