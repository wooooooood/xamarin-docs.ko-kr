---
title: 경로 정보 및 열거형
description: 경로 대 한 정보를 가져오고 내용 열거
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/12/2017
ms.openlocfilehash: 82ac4ea49462c7520219e1a621ea3946297b1b45
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/05/2018
---
# <a name="path-information-and-enumeration"></a>경로 정보 및 열거형

_경로 대 한 정보를 가져오고 내용 열거_

[ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) 여러 속성 및 메서드는 경로 대 한 정보를 얻을 수 있도록 하는 클래스를 정의 합니다. [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) 및 [ `TightBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.TightBounds/) 경로의 metrical 치수를 확인할 속성 (및 관련된 메서드). [ `Contains` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Contains/p/System.Single/System.Single/) 메서드를 사용 하면 특정 시점 경로 내 인지 확인 합니다.

경우에 따라 모든 선과 경로 구성 하는 곡선의 총 길이 확인 하는 것이 유용 합니다. 이 아니므로 간단한 알고리즘 방식으로 작업을 전체 클래스 라는 [ `PathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) 에 이루어집니다.

도 유용 경우에 따라 모든 그리기 작업 및 경로 구성 하는 요소를 가져올 수 있습니다. 처음에이 기능은 불필요 한 보이기는: 프로그램 내용을 이미 알고 있는 프로그램이 경로 만든 경우. 그러나를 살펴 보았으며 하 여 경로 만들 수도 있습니다 [경로 효과](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) 및 변환 하 여 [경로로 텍스트 문자열](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)합니다. 모든 그리기 작업 및 이러한 경로 구성 하는 지점을 얻을 수 있습니다. 한 가지 가능한 모든 지점에 알고리즘 변환을 적용 하는 것입니다. 이렇게 하면 반구 주위에 텍스트를 배치 하는 기법:

![](information-images/pathenumerationsample.png "텍스트 반구에 줄 바꿈")

## <a name="getting-the-path-length"></a>경로 길이 가져오기

문서에서 [ **경로 및 텍스트** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) 사용 하는 방법을 알아보았습니다는 [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) 메서드 인 초기 패스의 과정을 따릅니다 텍스트 문자열을 그립니다. 하지만 경로 정확 하 게 맞도록 텍스트 크기를 조정 하면 어떨까요? 원 주위 텍스트를 그리기 위한 쉽게 때문에 이것이 원의 원주를 계산 하기는 합니다. 하지만 되는 타원의 원 둘레 또는 베 지 어 곡선의 길이로 그리 간단 하지 않습니다. 

[ `SKPathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) 클래스 수입니다. [생성자](https://developer.xamarin.com/api/constructor/SkiaSharp.SKPathMeasure.SKPathMeasure/p/SkiaSharp.SKPath/System.Boolean/System.Single/) 허용는 `SKPath` 인수 및 [ `Length` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPathMeasure.Length/) 속성의 길이 보여 줍니다.

이 확인할는 **경로 길이** 예제를 기반으로 하는 **베 지 어 곡선** 페이지. [ **PathLengthPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) 파일에서 파생 `InteractivePage` 터치 인터페이스를 포함 합니다.

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Curves.PathLengthPage"
                       Title="Path Length">
    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</local:InteractivePage>
```

[ **PathLengthPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) 코드 숨김 파일을 사용 하면 끝점을 정의 하 고 있는 입방 형 3 차원 곡선의 제어점을 4 개의 터치 포인트를 이동할 수 있습니다. 세 필드는 텍스트 문자열을 정의 `SKPaint` 개체 및 텍스트의 너비를 계산된 합니다.

```csharp
public partial class PathLengthPage : InteractivePage
{
    const string text = "Compute length of path";

    static SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Black,
        TextSize = 10,
    };

    static readonly float baseTextWidth = textPaint.MeasureText(text);
    ...
}
```

`baseTextWidth` 필드는 기반으로 하는 텍스트의 너비는 `TextSize` 10을 설정 합니다.

`PaintSurface` 처리기 베 지 어 곡선을 그리는 데 및 다음의 전체 길이 따라 맞게 텍스트 크기를 조정 합니다.

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

        // Get path length
        SKPathMeasure pathMeasure = new SKPathMeasure(path, false, 1);

        // Find new text size
        textPaint.TextSize = pathMeasure.Length / baseTextWidth * 10;

        // Draw text on path
        canvas.DrawTextOnPath(text, path, 0, 0, textPaint);
    }
    ...
}
```

`Length` 속성을 새로 만든 `SKPathMeasure` 개체의 경로 길이 가져옵니다. 이 나눈는 `baseTextWidth` (변수인 10의 텍스트 크기에 따라 텍스트의 너비) 값 및 다음 10의 기본 텍스트 크기와 곱하면 합니다. 결과 해당 경로의 텍스트를 표시 하기 위한 새 텍스트 크기:

[![](information-images/pathlength-small.png "경로 길이 페이지의 삼중 스크린샷")](information-images/pathlength-large.png#lightbox "경로 길이 페이지의 삼중 스크린샷")

베 지 어 곡선을 더 길거나 더 짧은 가져옵니다, 텍스트 크기 변경 볼 수 있습니다.

## <a name="traversing-the-path"></a>경로 통과합니다.

`SKPathMeasure` 둘 이상의 측정값의 경로 길이 수행할 수 있습니다. 경로 길이 0 사이의 모든 값에 대 한 프로그램 `SKPathMeasure` 개체는 해당 지점에서 경로 및 경로 곡선 탄젠트에 위치를 가져올 수 있습니다. 탄젠트 형식의 벡터로 제공 됩니다는 `SKPoint` 개체나에 캡슐화 된 회전을으로 프로그램 `SKMatrix` 개체입니다. 메서드를 다음은 `SKPathMeasure` 다양 하 고 융통성 있는 방법으로이 정보를 얻으려면입니다.

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

[ `SKPathMeasureMatrixFlags` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasureMatrixFlags/) 됩니다.

- [`GetPosition`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPosition/)
- [`GetTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)
- [`GetPositionAndTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)

**자전거 절반 파이프** 페이지 사람 모양 아이콘이 있는 입방 형 3 차원 곡선을 따라 전달 자전거로 자전거에 애니메이션 효과 적용 합니다.

[![](information-images/unicyclehalfpipe-small.png "자전거 절반 파이프 페이지의 삼중 스크린샷")](information-images/unicyclehalfpipe-large.png#lightbox "자전거 절반 파이프 페이지의 삼중 스크린샷")

`SKPaint` 절반 파이프와는 자전거 선 그리기에 사용 되는 개체의 필드로 정의 됩니다는 [ `UnicycleHalfPipePage` ]() 클래스입니다. 또한 정의 `SKPath` 는 자전거에 대 한 개체:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };

    SKPath unicyclePath = SKPath.ParseSvgPathData(
        "M 0 0" + 
        "A 25 25 0 0 0 0 -50" +
        "A 25 25 0 0 0 0 0 Z" +
        "M 0 -25 L 0 -100" +
        "A 15 15 0 0 0 0 -130" +
        "A 15 15 0 0 0 0 -100 Z" +
        "M -25 -85 L 25 -85");
    ...
}
```

표준 재정의 포함 하는 클래스는 `OnAppearing` 및 `OnDisappearing` 애니메이션에 대 한 메서드. `PaintSurface` 처리기 절반 파이프에 대 한 경로 만들고 다음에 그립니다. `SKPathMeasure` 개체는 다음이 경로에 따라 만들어집니다.

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath pipePath = new SKPath())
        {
            pipePath.MoveTo(50, 50);
            pipePath.CubicTo(0, 1.25f * info.Height, 
                             info.Width - 0, 1.25f * info.Height,
                             info.Width - 50, 50);

            canvas.DrawPath(pipePath, strokePaint);

            using (SKPathMeasure pathMeasure = new SKPathMeasure(pipePath))
            {
                float length = pathMeasure.Length;

                // Animate t from 0 to 1 every three seconds
                TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
                float t = (float)(timeSpan.TotalSeconds % 5 / 5);

                // t from 0 to 1 to 0 but slower at beginning and end
                t = (float)((1 - Math.Cos(t * 2 * Math.PI)) / 2);

                SKMatrix matrix;
                pathMeasure.GetMatrix(t * length, out matrix, 
                                      SKPathMeasureMatrixFlags.GetPositionAndTangent);

                canvas.SetMatrix(matrix);
                canvas.DrawPath(unicyclePath, strokePaint);
            }
        }
    }
}
```

`PaintSurface` 의 값을 계산 하는 처리기 `t` 하는 라인 0에서 1로 5 초 마다. 다음 사용 하 여는 `Math.Cos` 하는 값을 변환 하려면 함수 `t` 하는 범위는 0에서 1, 0, 다시로 여기서 0은에 해당 왼쪽, 위쪽에서 시작 부분에서 자전거 1은 오른쪽 상단에 자전거에 해당 하는 동안 합니다. 코사인 함수 때문에 파이프의 맨 성과 맨 아래에 가장 빠른 속도입니다.

이 값의 `t` 첫 번째 인수에 대 한 경로 길이 곱한 수 해야 `GetMatrix`합니다. 행렬에 적용 되는 `SKCanvas` 자전거 경로 그리기 위한 개체입니다.

## <a name="enumerating-the-path"></a>경로 열거합니다.

클래스의 두 개의 포함 `SKPath` 경로의 내용을 열거할 수 있습니다. 이러한 클래스는 [ `SKPath.Iterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+Iterator/) 및 [ `SKPath.RawIterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+RawIterator/)합니다. 두 클래스는 매우 유사 하지만 `SKPath.Iterator` 경로 길이 0을, 또는 길이가 0 가까이 요소를 제거할 수 있습니다. `RawIterator` 아래 예에 사용 됩니다.

형식의 개체를 가져올 수 있습니다 `SKPath.RawIterator` 호출 하 여는 [ `CreateRawIterator` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CreateRawIterator()/) 방식의 `SKPath`합니다. 반복적으로 호출 하 여 수행 되는 경로 전체를 열거 하는 [ `Next` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.Next/p/SkiaSharp.SKPoint[]/) 메서드. 개의 전달 `SKPoint` 값:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

`Next` 의 멤버를 반환 하는 메서드는 [ `SKPathVerb` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathVerb/) 열거 합니다. 이러한 값을 경로에서 특정 그리기 명령을 나타냅니다. 배열에 삽입 하는 유효한 점 개수가이 동사에 따라 달라 집니다.

- [`Move`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Move/) 단일 지점과
- [`Line`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Line/) 두 점과 함께
- [`Cubic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Cubic/) 4 개의 점이 있는
- [`Quad`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Quad/) 세 개의 점을 사용
- [`Conic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Conic/) 세 개의 점을 사용 (호출 또한는 [ `ConicWeight` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.ConicWeight/) 가중치 메서드)
- [`Close`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Close/) 하나의 점에서
- [`Done`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Done/)

`Done` 동사 열거형 완료 되었음을 나타냅니다.

있으며 없는 `Arc` 동사입니다. 이 나타냅니다 모든 원호 경로에 추가 될 때 곡선을 베 지 어 곡선으로 변환 됩니다.

에 정보 중 일부는 `SKPoint` 배열 중복 됩니다. 예를 들어 경우는 `Move` 동사 뒤는 `Line` 동사를 다음 두 개의 점을 함께 사용 해야 하는 중 첫 번째는 `Line` 동일는 `Move` 지점입니다. 실제로 이러한 중복 매우 유용합니다. 가져올 때는 `Cubic` 동사, 헤더와 함께 입방 형 3 차원 곡선을 정의 하는 모든 점이 4 개. 이전 동사에서 설정 된 현재 위치를 유지 하지 않아도 됩니다.

문제가 있는 동사 인데 `Close`합니다. 이 명령은 현재 위치에서 의해 이미 설정 된 윤곽선의 시작 부분에 직선을 그립니다는 `Move` 명령입니다. 이상적으로 `Close` 동사 한 지점 보다는이 두 지점을 제공 해야 합니다. 더욱 심각한 지점 함께 제공 되는 `Close` 동사는 항상 (0, 0). 즉, 경로 열거할 때 필요가 있다는 것 아마도 유지 하는 `Move` 지점과 현재 위치입니다.

정적 [ `PathExtensions` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) 클래스를 일련의 작은 직선 곡선을 계산 하는 세 가지 종류의 베 지 어 곡선으로 변환 하는 여러 메서드가 포함 되어 있습니다. (파라메트릭 수식 문서에 제공 된 [ **베 지 어 곡선의 세 가지 형식**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md).) `Interpolate` 메서드 하나만 단위 길이에 있는 많은 짧은 줄에 직선을 나눕니다.

```csharp
static class PathExtensions
{
    ...
    static SKPoint[] Interpolate(SKPoint pt0, SKPoint pt1)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * pt0.X + t * pt1.X;
            float y = (1 - t) * pt0.Y + t * pt1.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenCubic(SKPoint pt0, SKPoint pt1, SKPoint pt2, SKPoint pt3)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2) + Length(pt2, pt3));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * (1 - t) * pt0.X +
                        3 * t * (1 - t) * (1 - t) * pt1.X +
                        3 * t * t * (1 - t) * pt2.X +
                        t * t * t * pt3.X;
            float y = (1 - t) * (1 - t) * (1 - t) * pt0.Y +
                        3 * t * (1 - t) * (1 - t) * pt1.Y +
                        3 * t * t * (1 - t) * pt2.Y +
                        t * t * t * pt3.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenQuadratic(SKPoint pt0, SKPoint pt1, SKPoint pt2)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenConic(SKPoint pt0, SKPoint pt1, SKPoint pt2, float weight)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float denominator = (1 - t) * (1 - t) + 2 * weight * t * (1 - t) + t * t;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * weight * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * weight * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            x /= denominator;
            y /= denominator;
        }

        return points;
    }

    static double Length(SKPoint pt0, SKPoint pt1)
    {
        return Math.Sqrt(Math.Pow(pt1.X - pt0.X, 2) + Math.Pow(pt1.Y - pt0.Y, 2));
    }
}
```

이러한 모든 방법을 확장 메서드에서 참조 `CloneWithTransform` 아래에 표시 합니다. 이 메서드는 경로 명령을 열거 하 고 데이터를 기반으로 새 경로 생성 하 여 경로 복제 합니다. 그러나 새 경로 구성 되어 `MoveTo` 및 `LineTo` 호출 합니다. 모든 곡선과 직선 일련의 작은 선으로 세분화 됩니다.

호출할 때 `CloneWithTransform`, 메서드에 전달할는 `Func<SKPoint, SKPoint>`를 사용 하는 함수는는 `SKPaint` 반환 하는 매개 변수는 `SKPoint` 값입니다. 이 함수는 사용자 지정 알고리즘 변환을 적용 하려면 모든 지점에 대 한 호출 됩니다.

```csharp
static class PathExtensions
{
    public static SKPath CloneWithTransform(this SKPath pathIn, Func<SKPoint, SKPoint> transform)
    {
        SKPath pathOut = new SKPath();

        using (SKPath.RawIterator iterator = pathIn.CreateRawIterator())
        {
            SKPoint[] points = new SKPoint[4];
            SKPathVerb pathVerb = SKPathVerb.Move;
            SKPoint firstPoint = new SKPoint();
            SKPoint lastPoint = new SKPoint();

            while ((pathVerb = iterator.Next(points)) != SKPathVerb.Done)
            {
                switch (pathVerb)
                {
                    case SKPathVerb.Move:
                        pathOut.MoveTo(transform(points[0]));
                        firstPoint = lastPoint = points[0];
                        break;

                    case SKPathVerb.Line:
                        SKPoint[] linePoints = Interpolate(points[0], points[1]);

                        foreach (SKPoint pt in linePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[1];
                        break;

                    case SKPathVerb.Cubic:
                        SKPoint[] cubicPoints = FlattenCubic(points[0], points[1], points[2], points[3]);

                        foreach (SKPoint pt in cubicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[3];
                        break;

                    case SKPathVerb.Quad:
                        SKPoint[] quadPoints = FlattenQuadratic(points[0], points[1], points[2]);

                        foreach (SKPoint pt in quadPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Conic:
                        SKPoint[] conicPoints = FlattenConic(points[0], points[1], points[2], iterator.ConicWeight());

                        foreach (SKPoint pt in conicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Close:
                        SKPoint[] closePoints = Interpolate(lastPoint, firstPoint);

                        foreach (SKPoint pt in closePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        firstPoint = lastPoint = new SKPoint(0, 0);
                        pathOut.Close();
                        break;
                }
            }
        }
        return pathOut;
    }
    ...
}
```

복제 된 경로 아주 작은 일직선으로 감소 하기 때문에 변환 함수에 직선 선을 곡선으로 변환 하는 기능이 있습니다.

메서드 호출 변수에 각 윤곽선에 첫 번째 지점을 유지 하도록 통지 `firstPoint` 변수에 현재 위치 각 그리기 명령 및 `lastPoint`합니다. 다음은 마지막 닫는 생성 하는 데 필요한 경우 줄는 `Close` 동사 발생 합니다.

**GlobularText** 샘플이 확장 메서드를 사용 하 여 것 처럼 보이는 3 차원 효과에 반구 주위의 텍스트를 줄 바꿈 합니다.

[![](information-images/globulartext-small.png "Globular 텍스트 페이지의 삼중 스크린샷")](information-images/globulartext-large.png#lightbox "Globular 텍스트 페이지의 삼중 스크린 샷")

[ `GlobularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) 클래스 생성자에는이 변환을 수행 합니다. 생성 된 `SKPaint` 를 획득 하 고 개체의 텍스트에 대 한는 `SKPath` 에서 개체는 `GetTextPath` 메서드. 이것이에 전달 된 경로 `CloneWithTransform` 변형 함수 이며 함께 확장 메서드: 

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;

    public GlobularTextPage()
    {
        Title = "Globular Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.Typeface = SKTypeface.FromFamilyName("Times New Roman");
            textPaint.TextSize = 100;

            using (SKPath textPath = textPaint.GetTextPath("HELLO", 0, 0))
            {
                SKRect textPathBounds;
                textPath.GetBounds(out textPathBounds);

                globePath = textPath.CloneWithTransform((SKPoint pt) =>
                {
                    double longitude = (Math.PI / textPathBounds.Width) * 
                                            (pt.X - textPathBounds.Left) - Math.PI / 2;
                    double latitude = (Math.PI / textPathBounds.Height) * 
                                            (pt.Y - textPathBounds.Top) - Math.PI / 2;

                    longitude *= 0.75;
                    latitude *= 0.75;

                    float x = (float)(Math.Cos(latitude) * Math.Sin(longitude));
                    float y = (float)Math.Sin(latitude);

                    return new SKPoint(x, y);
                });
            }
        }
    }
    ...
}
```

변환 함수는 먼저 라는 두 개의 값을 계산 `longitude` 및 `latitude` -π/2는 위와 맨 아래에 텍스트의 왼쪽에서 오른쪽 및 텍스트의 맨 아래에 π/2에 이르는 합니다. 이러한 값의 범위는 시각적으로 만족 스러운 하지 않으므로 0.75를 곱하여 감소 됩니다. (이러한 조정 되지 않은 코드를 사용해 보십시오. 텍스트가 북부 및 남 극 지방에서 모호 측면에서 씬.) 이러한 3 차원 구형 좌표를 2 차원 변환할지 `x` 및 `y` 표준 수식에서 좌표입니다.

새 경로 필드로 저장 됩니다. `PaintSurface` 처리기 다음 단순히 해야 가운데 맞추고 화면에 표시 하는 경로 확장 합니다.

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint pathPaint = new SKPaint())
        {
            pathPaint.Style = SKPaintStyle.Fill;
            pathPaint.Color = SKColors.Blue;
            pathPaint.StrokeWidth = 3;
            pathPaint.IsAntialias = true;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(0.45f * Math.Min(info.Width, info.Height));     // radius
            canvas.DrawPath(globePath, pathPaint);
        }
    }
}
```

이 매우 다양 한 방법입니다. 에 설명 된 경로가 효과의 배열 하는 경우는 [ **경로 효과** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) 문서 포함 되어야 할지 더 쉬워집니다 무언가 포괄할 매우 하지 않습니다 채울 하는 방법입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
