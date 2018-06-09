---
title: 세 가지 유형의 베 지 어 곡선으로 분할
description: 이 문서는 입방 형 3, 정방형, 및 원추형 베 지 어 곡선 Xamarin.Forms 응용 프로그램에서 렌더링 하기 위해 SkiaSharp를 사용 하는 방법을 설명 하 고 샘플 코드와 함께이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: charlespetzold
ms.author: chape
ms.date: 05/25/2017
ms.openlocfilehash: 4a1b86035f9ce31b6e9fafac06cd0090a516b542
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244008"
---
# <a name="three-types-of-bzier-curves"></a>세 가지 유형의 베 지 어 곡선으로 분할

_탐색 SkiaSharp 렌더링 입방 형 3, 정방형, 및 원추형 베 지 어 곡선을 사용 하는 방법_

베 지 어 곡선 피에르 베 지 어 (1910 – 1999) 자동차 회사 곡선 자동차 본문의 컴퓨터 기반 디자인에 대 한 사용 Renault 프랑스어 엔지니어 후 라고 합니다.

베 지 어 곡선으로 분할은 대화형 디자인에 잘 맞는 것으로 알려져:는 정상적으로 작동 &mdash; 즉, 무한 하거나 다루기 어려운 되도록 곡선을 발생 시키는 singularities 없는 &mdash; 며, 일반적으로 시선을 . 컴퓨터 기반 글꼴의 문자 윤곽선은 일반적으로 베 지 어 곡선으로 정의 됩니다.

![](beziers-images/beziersample.png "예제 3 차원 곡선")

Wikipedia 문서에 [베 지 어 곡선](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) 몇 가지 유용한 정보를 포함 합니다. 용어 *베 지 어 곡선* 실제로 비슷한 곡선의 제품군을 참조 합니다. SkiaSharp는 호출 되는 베 지 어 곡선의 세 가지 유형인은 *입방 형*, *정방형*, 및 *원추형*합니다. 원추형은 라고도 *합리적인 quadratic*합니다.

## <a name="the-cubic-bzier-curve"></a>입방 형 3 차원 곡선

베 지 어 곡선의 베 지 어 곡선의 제목을 표시 되 면 대부분의 개발자가 보고 있는 형식이 메시지를 표시 하는 입체입니다.

입방 형 3 베 지 어 곡선을 추가할 수 있습니다는 `SKPath` 를 사용 하 여 개체는 [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) 메서드 3 개 `SKPoint` 매개 변수 또는 [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/System.Single/) 별도오버로드`x` 및 `y` 매개 변수:

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

곡선은 윤곽선의 현재 위치에서 시작합니다. 전체 입방 형 3 차원 곡선은 점이 4 개에 의해 정의 됩니다.

- 시작점: 경우 윤곽, 또는 (0, 0)에서 현재 지점 `MoveTo` 호출 되지 않았습니다
- 먼저 제어점: `point1` 에 `CubicTo` 호출
- 두 번째 제어점: `point2` 에 `CubicTo` 호출
- 끝점: `point3` 에 `CubicTo` 호출

결과 곡선 시작 지점에서 시작 하 고 끝점에서 끝납니다. 곡선은 일반적으로 통과 하지 않으므로 두 개의 제어점입니다. 대신 like 많은 자석과 자신 들을 위한 곡선을 작동 합니다.

입방 형 3 차원 곡선에 대해 가장 좋은 방법은 실험을 통해 됩니다. 이의 용도 **베 지 어 곡선** 에서 파생 되는 페이지를 `InteractivePage`합니다. [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) 파일 인스턴스화하는 `SKCanvasView` 및 `TouchEffect`합니다. [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) 코드 숨김 파일 네 개를 만듭니다 `TouchPoint` 해당 생성자에 있는 개체입니다. `PaintSurface` 이벤트 처리기를 만듭니다는 `SKPath` 4에 따라 베 지 어 곡선을 렌더링 하 `TouchPoint` 개체, 및도 끝점에는 제어점에서 점선된 접선을 그립니다.

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

여기 세 플랫폼 모두에서 실행 되 고 있습니다.

[![](beziers-images/beziercurve-small.png "베 지 어 곡선 페이지의 삼중 스크린샷")](beziers-images/beziercurve-large.png#lightbox "베 지 어 곡선 페이지의 삼중 스크린샷")

수학적으로 곡선 입방 형 3 다항식입니다. 곡선 최대 세 지점에 직선을 교차 합니다. 시작 지점에서 곡선은 항상 탄젠트,와 동일한 방향으로, 처음부터 직선 가리키도록 첫 번째 제어점입니다. 끝점에서 곡선은 항상 탄젠트,와 동일한 방향으로, 두 번째 컨트롤에서 직선 가리키는 끝점입니다.

입방 형 3 차원 곡선은 항상 네 개의 점을 연결 볼록 사각형에 의해 제한 됩니다. 이 라고는 *볼록 집합*합니다. 제어점 시작 및 끝 지점 사이의 직선 상에 베 지 어 곡선 직선으로 렌더링 합니다. 하지만 곡선 수도 교차 자체를 세 번째 스크린 샷은 설명 된 것 처럼 합니다.

경로 윤곽선에 여러 개의 연결 된 입방 형 3 베 지 어 곡선을 포함 될 수 있지만 두 입방 형 3 베 지 어 곡선으로 분할 간의 연결 다음 세 점 모두가 동일 선상의 하는 경우에 부드러운 됩니다 (즉, 직선 상에 있음):

- 첫 번째 곡선의 둘째 제어점
- 두 번째 곡선의 시작점은 첫 번째 곡선의 끝점
- 두 번째 곡선의 첫째 제어점

다음 문서에서 [ **SVG 경로 데이터** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) 부드러운 연결 된 베 지 어 곡선의 정의 쉽게 수행할 수 있는 기능을 볼 수 있습니다.

입방 형 3 차원 곡선을 렌더링 하는 기본 매개 방정식을 알고 있는 경우에 따라 유용 합니다. 에 대 한 *t* 0에서 1 사이의 파라메트릭은 다음과 같습니다.

x(t) = (1 – t)³x₀ + 3t(1 – t)²x₁ + 3t²(1 – t)x₂ + t³x₃

y(t) = (1-t) ³y₀ + 3t 이상 (1-t) ²y₁ + 3t² (1-t) y₂ + t³y₃

3의 가장 높은 지 수 입방 형 polynomials 이들은 확인 합니다. 확인할 때 쉽게 `t` 0 이면 점이 (x₀ y₀)을 하는 시작 지점 및 시기 `t` 가 1 이면는 지점입니다 (x₃ y₃), 끝점입니다. 시작점 근처 (의 값이 낮은 `t`), (x₁, y₁) 첫 번째 제어점에 적용 하 고 근처 끝점 강력한 (높은 값 ' t ') (x₂, y₂) 두 번째 제어점에 큰 영향을 줍니다.

## <a name="bzier-curve-approximation-to-circular-arcs"></a>원호를 베 지 어 곡선 근사값

경우에 따라 원호를 렌더링 하는 베 지 어 곡선을 사용 하는 것이 유용 합니다. 입방 형 3 차원 곡선 수 대략적인 원호 상태일 때 잘 사분기 원, 최대 4 개의 연결 된 베 지 어 곡선 전체 원을 정의할 수 있도록입니다. 이 근사값을 25 년 전에 게시 하는 두 개의 문서에 설명 되어 있습니다.

> Tor Dokken, et al "곡률 연속 베 지 어 커브에서 원 중 적절 한 근사치" *컴퓨터 보조 기하학적 디자인 7* 33 41 (1990).

> Michael Goldapp, "입방 형 Polynomials 하 여 원호 근사치" *컴퓨터 보조 기하학적 디자인 8* 227-238 (1991).

다음 그림에 레이블이 지정 된 4 개의 점만 `pto`, `pt1`, `pt2`, 및 `pt3` 원호를 대략적으로 보여 주는 베 지 어 곡선 (빨간색으로 표시)을 정의 합니다.

![](beziers-images/bezierarc45.png "원호를 베 지 어 커브의 근사값")

제어점을 시작점 및 끝점에서 줄은 탄젠트 원을 베 지 어 곡선을 하 고 길이가 한 *L*합니다. 위에 언급 된 첫 번째 문서 최상의 베 지 어 곡선 원호를 대략적으로 보여 있는지 나타냅니다 경우 해당 길이 *L* 다음과 같이 계산 됩니다.

L = 4 × tan(α / 4) / 3

그림에서는 45도 각도 1 L 0.265와 같습니다. 코드에서 해당 값은 원의 원하는 반지름이 곱할 합니다.

**베 지 어 원호** 페이지에서는 정의 근사치를 구하려면 최대 180도 범위의 각도 대 한 원호를 베 지 어 곡선을 확인할 수 있습니다. [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) 파일 인스턴스화하는 `SKCanvasView` 및 `Slider` 각도 선택 하기 위한 합니다. `PaintSurface` 의 이벤트 처리기는 [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) 코드 숨김 파일 변환을 사용 하 여 캔버스의 가운데에 점 (0, 0)을 설정 합니다. 비교를 위해 해당 지점에 중심이 원을 그리고 베 지 어 곡선의 두 개의 제어점을 계산 합니다.

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

시작점과 끝점 (`point0` 및 `point3`) 원의 정규 파라메트릭 방정식 유형에 따라 계산 됩니다. 원 중심이 때문에 (0, 0), 이러한 점은 처리 될 수도 방사형 벡터로 원의 중심에서의 원 둘레를 합니다. 제어점이 방사형 벡터 오른쪽 각도에 되기 때문에 해당 하는 원의 접하는 줄에 있습니다. 오른쪽 각도에 벡터 단순히 교체 된 X 및 Y 좌표 및 음수 만든 그 중 하나 사용 하 여 원래 벡터가입니다.

다음은 세 가지 다른 관점을 사용 하 여 세 플랫폼에서 실행 중인 프로그램입니다.

[![](beziers-images/beziercirculararc-small.png "베 지 어 원호 페이지의 삼중 스크린샷")](beziers-images/beziercirculararc-large.png#lightbox "베 지 어 원호 페이지의 삼중 스크린샷")

세 번째 스크린 샷에 치중 하 고 각도 180도 있지만 iOS 화면에는 있다는 것 각도가 90도 제대로 분기 원에 맞게 표시 베 지 어 곡선 특히 반원에서 벗어나는 표시 됩니다.

두 개의 제어점의 좌표를 계산 분기 원 방향 다음과 같은 경우 상당히 쉽습니다.

![](beziers-images/bezierarc90.png "1/4 원형 베 지 어 곡선으로의 근사값")

원의 반지름은 100, *L* 55 이며 기억 하기 쉬운는 번호입니다.

**원 더하거나 제곱** 페이지 원형 및 사각형 사이의 그림 애니메이션 효과 적용 합니다. 원이 있는 좌표에서이 배열 정의의 첫 번째 열에 표시 된 네 개의 베 지 어 커브에서 근접할는 [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) 클래스:

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

두 번째 열 면적 같습니다 약 원의 면적 사각형을 정의 하는 4 개의 베 지 어 곡선의 좌표를 포함 합니다. (있는 사각형 그리기는 *정확한* 주어진 원으로 영역은의 기하학적 문제 해결할 수 없는 클래식 [원 더하거나 제곱](https://en.wikipedia.org/wiki/Squaring_the_circle).) 베 지 어 곡선으로 분할이 있는 사각형을 렌더링 하는 것에 대 한 각 곡선에 대 한 두 개의 제어점 동일 며, 시작 및 끝 지점 동일 선상의 베 지 어 곡선 직선으로 렌더링 됩니다.

배열의 세 번째 열은 애니메이션에 대 한 보간된 값입니다. 16 밀리초에 대 한 타이머를 설정 하는 페이지 및 `PaintSurface` 이러한 비율로 처리기가 호출 됩니다.

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

포인트의 sinusoidally 진동 값에 따라 보간 `t`합니다. 일련의 연결 된 4 개의 베 지 어 곡선을 생성 하는 보간된 포인트 사용 됩니다. 사각형으로 원에서 진행률을 표시 한 세 가지 플랫폼에서 실행 중인 애니메이션 다음과 같습니다.

[![](beziers-images/squaringthecircle-small.png "Squaring의 삼중 스크린 샷 원 페이지")](beziers-images/squaringthecircle-large.png#lightbox "는 Squaring의 삼중 스크린 샷 원 페이지")

이러한 애니메이션이 있는 원호 및 직선으로 렌더링할 수 있을 정도로 유연 알고리즘 방식으로 곡선 없이 수 없습니다.

**베 지 어 무한대** 페이지에는 또한 근사치를 구하려면 원호를 베 지 어 곡선의 기능을 사용 합니다. 다음은 `PaintSurface` 에서 처리기는 [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) 클래스:

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

관련성을 확인 하려면 그래프 용지에 이러한 좌표를 그리는 데 좋은 습관 수도 있습니다. 무한대 기호는 점 (0, 0)을 중심으로 있고 두 루프의 가운데 (–150, 0) 및 (150, 0) 하 고 100의 반지름입니다. 일련의 `CubicTo` 명령을 볼 수 있습니다 –95 및 –205의 값에 대 한 제어 점의 좌표 X (해당 값은 –150 더하기 및 빼기 55), 205 및 95 (150 더하기 및 빼기 55)으로 250, –250 왼쪽과 오른쪽에 대 한 합니다. 유일한 예외는 무한대 기호 자체를 가운데에 교차 하는 경우. 이 경우 제어점 50, –50 아웃 곡선 가운데 맞춤을 조합 하 여 좌표를 갖는 합니다.

다음은 세 플랫폼 모두에서 무한대 기호가입니다.

[![](beziers-images/bezierinfinity-small.png "베 지 어 무한대 페이지의 삼중 스크린샷")](beziers-images/bezierinfinity-large.png#lightbox "베 지 어 무한대 페이지의 삼중 스크린샷")

다소 더 부드럽게 만들기 중심 쪽으로 렌더링 하 여 무한대 기호 보다는 **호 무한대** 에서 페이지는 [ **세 가지 방법으로 호를 그릴 수** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) 문서.

## <a name="the-quadratic-bzier-curve"></a>정방형 3 차원 곡선

2 차 베 지 어 곡선 하나만 제어점 개이고 곡선은 3 개의 점으로 정의: 시작점, 제어 지점 및 끝 지점입니다. 파라메트릭 수식과 매우 비슷합니다 입방 형 3 차원 곡선을 곡선 2 차 다항식 이므로 높은 지 수는 2:

x(t) = (1-t) ²x₀ + 2t (1-t) x₁ + t²x₂

y(t) = (1-t) ²y₀ + 2t (1-t) y₁ + t²y₂

경로에 정방형 베 지 어 곡선을 추가 하려면 사용 된 [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) 메서드 또는 [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/System.Single/System.Single/System.Single/System.Single/) 오버 로드와 별도 `x` 및 `y` 좌표:

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

메서드를 현재 위치에서 곡선 추가 `point2` 와 `point1` 제어 지점으로 합니다.

정방형 베 지 어 곡선을 시험해 볼 수 있습니다는 **정방형 곡선** 매우 비슷하지만 페이지는 **베 지 어 곡선** 페이지 3 개만 터치 포인트를 되었다는 점만 제외 합니다. 다음은 `PaintSurface` 의 처리기는 [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) 코드 숨김 파일:

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

및 세 플랫폼 모두에서 실행 되 고 여기서:

[![](beziers-images/quadraticcurve-small.png "정방형 곡선 페이지의 삼중 스크린샷")](beziers-images/quadraticcurve-large.png#lightbox "정방형 곡선 페이지의 삼중 스크린샷")

점선은 탄젠트 시작점 및 끝점, 곡선 및 제어점 만나고 합니다.

정방형 베 지 어는 일반적인 모양의 곡선을 해야 하지만 두가 아닌 컨트롤 한 개의 지점에서 편리 하 게 하려는 경우에 유용 합니다. 정방형 베 지 어 모든 다른 곡선을 사용 되어 내부적으로 Skia 타원형 호를 렌더링 하는 보다 더 효율적으로 렌더링 합니다.

그러나 정방형 베 지 어 곡선의 모양이 않습니다 합니다 타원형 이기 때문에 여러 정방형 베 지 어 타원형 호를 계산 하는 데 필요한 합니다. 정방형 베 지 어는 포물선의 세그먼트 대신입니다.

## <a name="the-conic-bzier-curve"></a>원추형 베 지 어 곡선

원추형 베 지 어 곡선 &mdash; 라고도 합리적인 정방형 3 차원 곡선 &mdash; 는 베 지 어 곡선의 제품군으로 비교적 최근 추가 합니다. 정방형 베 지 어 곡선 합리적인 정방형 3 차원 곡선 시작점, 끝점 및 제어점 하나 포함 됩니다. 유리 정방형 3 차원 곡선도 필요 하지만 *가중치* 값입니다. 호출 되는 *합리적인* 정방형 파라메트릭 수식 비율을 포함 하기 때문입니다.

매개 방정식 X 및 Y는 동일한 분모를 공유 하는 비율입니다. 다음에 대 한 기준에 대 한 수식은 *t* 0에서 사이의 가중치는 1로 *w*:

d(t) = (1-t) ² + 2wt(1 – t) + t²

이론적으로 합리적인 quadratic 세 조건 각각에 대해 하나씩 세 개의 별도 가중치 값을 포함할 수 있지만 중간 용어에 가중치 값을 하나만 단순화할 수 있습니다.

X 및 Y 좌표에 대 한 매개 방정식은 중간 용어에 가중치 값도 포함 되어 식이 분모로 나눈 있다는 점을 제외 하면에 정방형 3 차원에 대 한 매개 방정식 유사 합니다.

x(t) = ((1 – t) ²x₀ + 2wt (1-t) x₁ t²x₂ +)) ÷ d(t)

y(t) = ((1 – t) ²y₀ + 2wt (1-t) y₁ t²y₂ +)) ÷ d(t)

유리 정방형 베 지 어 커브 라고도 *conics* 원추형 만큼의 세그먼트로 정확 하 게 나타낼 수 있으므로 &mdash; hyperbolas, parabolas, 타원, 및 원입니다.

경로에 유리 정방형 베 지 어 곡선을 추가 하려면 사용 된 [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) 메서드 또는 [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) 오버 로드와 별도 `x` 및 `y` 좌표:

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

최종 확인 `weight` 매개 변수입니다.

**원추형 곡선** 페이지에서는 이러한 곡선을 실험할 수 있습니다. `ConicCurvePage` 클래스는 `InteractivePage`에서 파생됩니다. [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) 파일 인스턴스화하는 `Slider` -2에서 2 사이의 가중치 값을 선택 합니다. [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) 코드 숨김 파일 세 개를 만들며 `TouchPoint` 개체 및 `PaintSurface` 처리기 컨트롤에 탄젠트 줄이 포함 된 결과 곡선 렌더링 사항:

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

여기 세 플랫폼 모두에서 실행 되 고 있습니다.

[![](beziers-images/coniccurve-small.png "원추형 곡선 페이지의 삼중 스크린샷")](beziers-images/coniccurve-large.png#lightbox "원추형 곡선 페이지의 삼중 스크린샷")

볼 수 있듯이 제어점 가중치가 높은 경우 더 눈 곡선 끌어오기 것 처럼 보입니다. 가중치가 0 인 곡선 끝점으로 시작 점에서 직선 됩니다.

이론적으로 음수 가중치 허용 여부와 구부러지지 곡선 인해 *자리를 비울* 제어점에서입니다. 그러나 가중치를-1 또는 그 원인은 아래의 특정 값에 대 한 음수 되도록 매개 방정식의 분모 *t*합니다. 아마도 이러한 이유로 음수 가중치에서는 무시 되 고 `ConicTo` 메서드. **원추형 곡선** 음수 가중치 0을 가중치로 구분해 서와 렌더링할 직선 실험 하 여 볼 수 있듯이 하지만 음수 가중치를 설정할 수 있습니다.

매우 쉽게 파생과 가중치를 사용 하는 `ConicTo` 를 원호를 최대 그리는 메서드 (시간은 포함 되지 않음) 반원입니다. 다음 다이어그램에는 시작점 및 끝점에서 접선 제어점에 충족합니다.

![](beziers-images/conicarc.png "원호의 원추형 호 렌더링")

삼각 함수를 사용 하 여 제어 포인트 원 중심에서의 거리를 결정: 절반 각도 α의 코사인 값을 나눈 원의 반지름을 됩니다. 원호 시작점과 끝점 간의 그리려면 절반 각도의 코사인 해당 동일한 값에 가중치를 설정 합니다. 고 해당 각도 180도 이면 다음 탄젠트 선 충족 하지 가중치는 0이 됩니다. 하지만 각도 180도 보다 작은 경우에 대 한 계산 제대로 작동 합니다.

**원추형 원호** 이 페이지를 보여 줍니다. [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) 파일 인스턴스화합니다는 `Slider` 각도 선택 합니다. `PaintSurface` 의 처리기는 [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) 제어점 및 무게를 계산 하는 코드 숨김 파일:

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

볼 수 있듯이 차이가 없습니다 시각적는 `ConicTo` 경로 빨간색으로 표시 되 고 기본 원이 참조용으로 표시 합니다.

[![](beziers-images/coniccirculararc-small.png "원추형 원호 페이지의 삼중 스크린샷")](beziers-images/coniccirculararc-large.png#lightbox "원추형 원호 페이지의 삼중 스크린샷")

하지만 각도 180도 및 수학 실패로 설정 합니다.

이 경우에 개념이 `ConicTo` 이론적으로 (파라메트릭 수식에 따라), 원을 호출 하 여 완료 수 때문에 음수 가중치를 지원 하지 않습니다 `ConicTo` 선만 있지만 가중치의 음수 값입니다. 그러면 두 개만으로 전체 원 만드는 `ConicTo` 곡선 사이 (포함 되지 않음) 모든 각도에 따라 0도 180도 합니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
