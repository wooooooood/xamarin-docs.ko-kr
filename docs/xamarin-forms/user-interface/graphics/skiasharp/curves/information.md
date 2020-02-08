---
title: 경로 정보 및 열거형
description: 이 문서에서는 콘텐츠를 열거 및 SkiaSharp 경로 대 한 정보를 제공 하는 방법을 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2017
ms.openlocfilehash: 6f4f4e6253c14d86e2057f13d6232a07a83b4d26
ms.sourcegitcommit: ae5557c5024d4b7bd52b2f33cb96114ce2b8e086
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77045077"
---
# <a name="path-information-and-enumeration"></a>경로 정보 및 열거형

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_경로에 대 한 정보를 가져오고 콘텐츠를 열거 합니다._

[`SKPath`](xref:SkiaSharp.SKPath) 클래스는 경로에 대 한 정보를 가져올 수 있는 몇 가지 속성 및 메서드를 정의 합니다. [`Bounds`](xref:SkiaSharp.SKPath.Bounds) 및 [`TightBounds`](xref:SkiaSharp.SKPath.TightBounds) 속성 및 관련 메서드는 경로의 metrical 크기를 가져옵니다. [`Contains`](xref:SkiaSharp.SKPath.Contains(System.Single,System.Single)) 메서드를 사용 하 여 특정 점이 경로 내에 있는지 여부를 확인할 수 있습니다.

경우에 따라 모든 줄 및 경로 구성 하는 곡선의 총 길이 확인 하는 것이 유용 합니다. 이 길이를 계산 하는 작업은 알고리즘 방식으로 단순 작업이 아니므로 [`PathMeasure`](xref:SkiaSharp.SKPathMeasure) 이라는 전체 클래스가이 작업에 할당 됩니다.

또한 유용한 경우가 모든 그리기 작업 및 경로 구성 하는 요소를 가져오려고 합니다. 처음에이 기능은 불필요 한 보일 수 있습니다: 프로그램 내용을 이미 알고 있는 프로그램에서 경로 만든 경우. 그러나 [경로 효과](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) 와 [텍스트 문자열을 패스로](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)변환 하 여 경로를 만들 수도 있습니다. 모든 그리기 작업 및 이러한 경로 구성 하는 지점을 얻을 수 있습니다. 한 가지 방법은 다음과 같습니다. 반구 주위의 텍스트 줄 바꿈, 예를 들어 모든 지점에 알고리즘 변환 적용

![](information-images/pathenumerationsample.png "Text wrapped on a hemisphere")

## <a name="getting-the-path-length"></a>경로 길이 가져오기

문서 [**경로 및 텍스트**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) 에서 [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) 메서드를 사용 하 여 기준이 경로를 따르는 텍스트 문자열을 그리는 방법에 대해 알아보았습니다. 그러나 경로 정확 하 게 맞도록 텍스트의 크기를 원하는 경우? 텍스트를 원 주위 그릴 원의 원주를 계산 하는 간단한 이므로 쉽습니다. 하지만 타원의 일부 또는 베 지 어 곡선의 길이 그리 간단 하지 않습니다.

[`SKPathMeasure`](xref:SkiaSharp.SKPathMeasure) 클래스는 도움이 될 수 있습니다. [생성자](xref:SkiaSharp.SKPathMeasure.%23ctor(SkiaSharp.SKPath,System.Boolean,System.Single)) 는 `SKPath` 인수를 수락 하 고 [`Length`](xref:SkiaSharp.SKPathMeasure.Length) 속성은 해당 길이를 표시 합니다.

이 클래스는 **베 지 어 곡선** 페이지를 기반으로 하는 **경로 길이** 샘플에서 보여 줍니다. [**PathLengthPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) 파일은 `InteractivePage`에서 파생 되 고 터치 인터페이스를 포함 합니다.

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

[**PathLengthPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) 코드를 사용 하면 4 개의 터치 점을 이동 하 여 입방 형 3 차원 곡선의 끝점 및 제어점을 정의할 수 있습니다. 텍스트 문자열, `SKPaint` 개체 및 텍스트의 계산 된 너비를 정의 하는 세 개의 필드가 있습니다.

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

`baseTextWidth` 필드는 `TextSize` 설정 10을 기준으로 하는 텍스트의 너비입니다.

`PaintSurface` 처리기는 베 지 어 곡선을 그린 다음 전체 길이에 맞게 텍스트의 크기를 조정 합니다.

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

새로 만든 `SKPathMeasure` 개체의 `Length` 속성은 경로의 길이를 가져옵니다. 경로 길이는 `baseTextWidth` 값 (텍스트 크기 10을 기준으로 하는 텍스트의 너비)으로 나눈 다음 기본 텍스트 크기인 10을 곱합니다. 결과 해당 경로 상에 텍스트를 표시 하기 위한 새 텍스트 크기:

[![](information-images/pathlength-small.png "Triple screenshot of the Path Length page")](information-images/pathlength-large.png#lightbox "Triple screenshot of the Path Length page")

베 지 어 곡선을 더 길거나 더 짧은 가져옵니다를 변경 하는 텍스트 크기를 볼 수 있습니다.

## <a name="traversing-the-path"></a>경로 트래버스합니다.

`SKPathMeasure`는 경로의 길이를 측정 하는 것 이상의 작업을 수행할 수 있습니다. 0과 경로 길이 사이의 값에 대해 `SKPathMeasure` 개체는 경로의 위치와 해당 지점에 있는 경로 곡선에 대 한 탄젠트를 가져올 수 있습니다. 접선은 `SKPoint` 개체의 형태로 벡터 나 `SKMatrix` 개체에 캡슐화 된 회전으로 사용할 수 있습니다. 다양 하 고 유연한 방식으로이 정보를 가져오는 `SKPathMeasure` 방법은 다음과 같습니다.

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

[`SKPathMeasureMatrixFlags`](xref:SkiaSharp.SKPathMeasureMatrixFlags) 열거형의 멤버는 다음과 같습니다.

- `GetPosition`
- `GetTangent`
- `GetPositionAndTangent`

**Unicycle 반쪽 파이프** 페이지는 입방 형 3 차원 곡선을 따라 앞뒤로 이동 하는 Unicycle에 대 한 스틱 그림에 애니메이션을 적용 합니다.

[![](information-images/unicyclehalfpipe-small.png "Triple screenshot of the Unicycle Half-Pipe page")](information-images/unicyclehalfpipe-large.png#lightbox "Triple screenshot of the Unicycle Half-Pipe page")

절반 파이프와 unicycle을 스트로크 하는 데 사용 되는 `SKPaint` 개체는 `UnicycleHalfPipePage` 클래스의 필드로 정의 됩니다. Unicycle에 대 한 `SKPath` 개체도 정의 됩니다.

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

클래스는 애니메이션에 대 한 `OnAppearing` 및 `OnDisappearing` 메서드의 표준 재정의를 포함 합니다. `PaintSurface` 처리기는 반쪽 파이프의 경로를 만든 다음 그립니다. 그런 다음이 경로를 기반으로 `SKPathMeasure` 개체가 만들어집니다.

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

`PaintSurface` 처리기는 5 초 마다 0에서 1로 이동 하는 `t` 값을 계산 합니다. 그런 다음 `Math.Cos` 함수를 사용 하 여이 값을 0에서 1 사이의 `t` 값으로 변환 하 고 다시 0으로 변환 합니다. 여기서 0은 왼쪽 위에 있는 unicycle에 해당 하 고, 1은 오른쪽 위에 있는 unicycle에 해당 합니다. 코사인 함수는 파이프의 맨 위에 있는 가장 느린 고 맨 아래에서 가장 빠른 속도 하면 됩니다.

이 `t` 값은 `GetMatrix`첫 번째 인수에 대 한 경로 길이를 곱하여 합니다. 그런 다음 unicycle 경로를 그리기 위해 매트릭스가 `SKCanvas` 개체에 적용 됩니다.

## <a name="enumerating-the-path"></a>경로 열거합니다.

`SKPath`의 두 포함 클래스를 사용 하 여 경로의 내용을 열거할 수 있습니다. 이러한 클래스는 [`SKPath.Iterator`](xref:SkiaSharp.SKPath.Iterator) [`SKPath.RawIterator`](xref:SkiaSharp.SKPath.RawIterator)됩니다. 두 클래스는 매우 유사 하지만 경로에서 길이가 0 이거나 길이가 0 인 요소를 제거할 수 `SKPath.Iterator`. `RawIterator`은 아래 예제에서 사용 됩니다.

`SKPath`의 [`CreateRawIterator`](xref:SkiaSharp.SKPath.CreateRawIterator) 메서드를 호출 하 여 `SKPath.RawIterator` 형식의 개체를 가져올 수 있습니다. 경로를 통해 열거 하는 것은 [`Next`](xref:SkiaSharp.SKPath.RawIterator.Next*) 메서드를 반복적으로 호출 하 여 수행 됩니다. 4 개의 `SKPoint` 값 배열로 전달 합니다.

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

`Next` 메서드는 [`SKPathVerb`](xref:SkiaSharp.SKPathVerb) 열거형 형식의 멤버를 반환 합니다. 이러한 값을 경로에 특정 그리기 명령을 나타냅니다. 배열에 삽입 하는 유효한 점 개수가이 동사에 따라 달라 집니다.

- 단일 지점으로 `Move`
- 두 점이 있는 `Line`
- 4 개의 점이 있는 `Cubic`
- 3 개 점이 있는 `Quad`
- 3 개 요소를 사용 하 여 `Conic` 하 고 가중치에 대 한 [`ConicWeight`](xref:SkiaSharp.SKPath.RawIterator.ConicWeight*) 메서드도 호출 합니다.
- 한 점을 사용 하 여 `Close`
- `Done`

`Done` 동사는 경로 열거가 완료 되었음을 나타냅니다.

`Arc` 동사가 없습니다. 이 모든 원호 경로에 추가 하는 경우에 베 지 어 곡선으로 변환 됩니다 것을 나타냅니다.

`SKPoint` 배열의 정보 중 일부는 중복 됩니다. 예를 들어 `Move` 동사가 뒤에 `Line` 동사가 오는 경우 `Line`와 함께 제공 되는 두 점 중 첫 번째 점이 `Move` 지점과 동일 합니다. 실제로이 중복 매우 유용합니다. `Cubic` 동사가 있으면 입방 형 3 차원 곡선을 정의 하는 네 개의 점이 모두 수반 됩니다. 이전 동사 설정한 현재 위치를 유지할 필요가 없습니다.

그러나 문제가 있는 동사는 `Close`됩니다. 이 명령은 현재 위치에서 `Move` 명령으로 이전에 설정 된 윤곽선의 시작 부분으로 직선을 그립니다. 이상적으로 `Close` 동사는 단 하나의 점이 아닌 이러한 두 점을 제공 해야 합니다. `Close` 동사와 함께 제공 되는 점은 항상 (0, 0)입니다. 경로를 통해 열거 하는 경우 `Move` 지점과 현재 위치를 유지 해야 합니다.

## <a name="enumerating-flattening-and-malforming"></a>열거, 병합, 및 Malforming

알고리즘을 적용 하는 것이 바람직 경우가 변환 malform에 대 한 경로에 어떤 방식으로든에서:

![](information-images/pathenumerationsample.png "Text wrapped on a hemisphere")

이러한 문자는 대부분 직선 이루어진 아직 곡선에 이러한 직선을 twisted 분명히 있어야 합니다. 어떻게 이것이 가능 한가요?

키가 원래 직선은 일련의 작은 직선으로 분류 됩니다. 그런 다음 곡선을 형성 하는 다양 한 방법의 더 작은 직선 이러한 개별을 조작할 수 있습니다.

이 프로세스를 지원 하기 위해 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플에는 길이가 한 단위 인 여러 짧은 줄로 직선을 분할 하는 `Interpolate` 메서드를 사용 하는 정적 [`PathExtensions`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) 클래스가 포함 되어 있습니다. 또한 클래스에는 일련의 작은 직선 곡선 근사치는 세 가지 유형의 베 지 어 곡선으로 변환 하는 여러 메서드가 들어 있습니다. 패라메트릭 수식은 [**3 가지 유형의 베 지 어 곡선**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)문서에 나와 있습니다. 이 프로세스를 곡선 _평면화_ 라고 합니다.

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
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static double Length(SKPoint pt0, SKPoint pt1)
    {
        return Math.Sqrt(Math.Pow(pt1.X - pt0.X, 2) + Math.Pow(pt1.Y - pt0.Y, 2));
    }
}
```

이러한 모든 메서드는이 클래스에 포함 된 `CloneWithTransform` 확장 메서드에서 참조 되며 아래에 나와 있습니다. 이 메서드는 경로 명령을 열거 하 고 데이터를 기반으로 새 경로 생성 하 여 경로 복제 합니다. 그러나 새 경로는 `MoveTo` 및 `LineTo` 호출로만 구성 됩니다. 모든 곡선 및 직선 일련의 작은 줄으로 세분화 됩니다.

`CloneWithTransform`를 호출할 때 `SKPoint` 값을 반환 하는 `SKPaint` 매개 변수를 사용 하는 함수인 `Func<SKPoint, SKPoint>`를 메서드에 전달 합니다. 이 함수는 사용자 지정 알고리즘 변환을 적용 하려면 모든 지점에 대 한 호출 됩니다.

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

복제 경로 작은 직선에 감소 하기 때문에 변환 함수에 직선 곡선으로 변환 하는 기능이 있습니다.

메서드는 `firstPoint` 변수에서 각 컨투어의 첫 번째 점을 유지 하 고 `lastPoint`변수에서 각 그리기 명령 다음의 현재 위치를 유지 합니다. 이러한 변수는 `Close` 동사가 발견 될 때 최종 닫는 줄을 생성 하는 데 필요 합니다.

**GlobularText** 샘플에서는이 확장 메서드를 사용 하 여 3d 효과의 반구 주위에 텍스트를 래핑합니다.

[![](information-images/globulartext-small.png "Triple screenshot of the Globular Text page")](information-images/globulartext-large.png#lightbox "Triple screenshot of the Globular Text page")

[`GlobularTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) 클래스 생성자는이 변환을 수행 합니다. 텍스트에 대 한 `SKPaint` 개체를 만든 다음 `GetTextPath` 메서드에서 `SKPath` 개체를 가져옵니다. 다음은 변환 함수와 함께 `CloneWithTransform` 확장 메서드에 전달 되는 경로입니다.

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

Transform 함수는 먼저 `longitude` 이라는 두 값을 계산 하 고 텍스트의 위쪽과 왼쪽에 있는 – π/2부터 텍스트의 오른쪽 및 아래쪽에 π/2 까지의 범위를 `latitude` 합니다. 이러한 값의 범위는 시각적으로 만족 하지 않으므로 0.75를 곱하여 인하 됩니다. (이러한 조정 하지 않고 코드를 사용해 보십시오. 북부 및 남부 극 지방 텍스트는 너무 복잡해 집니다 .이 경우에는 너무 작은 쪽입니다. 이러한 3 차원 구면 좌표는 표준 수식에 의해 2 차원 `x` 및 `y` 좌표로 변환 됩니다.

새 경로 필드로 저장 됩니다. 그러면 `PaintSurface` 처리기가 화면에 표시할 경로의 크기를 조정 하 고 크기를 조정 하기만 하면 됩니다.

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

이 매우 다양 한 기술입니다. [**경로 효과**](effects.md) 문서에 설명 된 경로 효과 배열에 포함 되어야 하는 내용이 확실 하지 않은 경우이를 통해 간격을 채울 수 있습니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
