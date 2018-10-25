---
title: 경로 정보 및 열거형
description: 이 문서에서는 콘텐츠를 열거 및 SkiaSharp 경로 대 한 정보를 제공 하는 방법을 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2017
ms.openlocfilehash: 6efefe11b31428f41bfa945aff93aa70aa764870
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615277"
---
# <a name="path-information-and-enumeration"></a>경로 정보 및 열거형

_경로 대 한 정보를 가져오고 내용을 열거 합니다._

합니다 [ `SKPath` ](xref:SkiaSharp.SKPath) 여러 속성 및 메서드는 경로 대 한 정보를 얻을 수 있도록 하는 클래스를 정의 합니다. 합니다 [ `Bounds` ](xref:SkiaSharp.SKPath.Bounds) 하 고 [ `TightBounds` ](xref:SkiaSharp.SKPath.TightBounds) 속성 (및 관련된 메서드) 경로의 metrical 크기를 가져옵니다. 합니다 [ `Contains` ](xref:SkiaSharp.SKPath.Contains(System.Single,System.Single)) 메서드를 사용 하면 경로 내에서 특정 지점 인지 확인 합니다.

경우에 따라 모든 줄 및 경로 구성 하는 곡선의 총 길이 확인 하는 것이 유용 합니다. 이 길이 계산 하지 않으므로 간단한 알고리즘 방식으로 작업을 전체 클래스 라는 [ `PathMeasure` ](xref:SkiaSharp.SKPathMeasure) 를 전용으로 지정 됩니다.

또한 유용한 경우가 모든 그리기 작업 및 경로 구성 하는 요소를 가져오려고 합니다. 처음에이 기능은 불필요 한 보일 수 있습니다: 프로그램 내용을 이미 알고 있는 프로그램에서 경로 만든 경우. 그러나 지금까지 살펴본 하 여 경로 만들 수도 있습니다 [경로 효과](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) 변환 하 [경로에 텍스트 문자열](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)합니다. 모든 그리기 작업 및 이러한 경로 구성 하는 지점을 얻을 수 있습니다. 한 가지 방법은 다음과 같습니다. 반구 주위의 텍스트 줄 바꿈, 예를 들어 모든 지점에 알고리즘 변환 적용

![](information-images/pathenumerationsample.png "반구에서 텍스트")

## <a name="getting-the-path-length"></a>경로 길이 가져오기

문서의 [ **경로 및 텍스트** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) 사용 하는 방법을 살펴보았습니다 합니다 [ `DrawTextOnPath` ](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) 해당 기준을 따릅니다 경로의 과정 텍스트 문자열을 그리는 방법입니다. 그러나 경로 정확 하 게 맞도록 텍스트의 크기를 원하는 경우? 텍스트를 원 주위 그릴 원의 원주를 계산 하는 간단한 이므로 쉽습니다. 하지만 타원의 일부 또는 베 지 어 곡선의 길이 그리 간단 하지 않습니다.

합니다 [ `SKPathMeasure` ](xref:SkiaSharp.SKPathMeasure) 클래스는 데 도움이 됩니다. 합니다 [생성자](xref:SkiaSharp.SKPathMeasure.%23ctor(SkiaSharp.SKPath,System.Boolean,System.Single)) 허용는 `SKPath` 인수를 및 [ `Length` ](xref:SkiaSharp.SKPathMeasure.Length) 속성 길이 표시 합니다.

이 클래스에 설명 되어는 **경로 길이** 샘플을 기반으로 하는 합니다 **베 지 어 곡선** 페이지입니다. 합니다 [ **PathLengthPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) 에서 파생 되는 파일 `InteractivePage` 터치 인터페이스를 포함 합니다.

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

합니다 [ **PathLengthPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) 코드 숨김 파일을 사용 하면 끝점을 정의 하 고 입방 형 3 차원 큐빅 곡선의 제어점을 4 개의 터치 포인트를 이동할 수 있습니다. 세 개의 필드가 정의 텍스트 문자열을 `SKPaint` 개체 및 텍스트의 너비를 계산된 합니다.

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

합니다 `baseTextWidth` 필드를 기준으로 텍스트의 너비를 `TextSize` 10의 설정 합니다.

`PaintSurface` 처리기 베 지 어 곡선을 그리고 다음 전체 길이 따라 맞게 텍스트 크기:

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

합니다 `Length` 속성을 새로 만든 `SKPathMeasure` 경로의 길이 가져옵니다. 경로 길이 나눈는 `baseTextWidth` (인 10의 텍스트 크기에 따라 텍스트의 너비) 값 및 10의 기본 텍스트 크기를 곱한 후 합니다. 결과 해당 경로 상에 텍스트를 표시 하기 위한 새 텍스트 크기:

[![](information-images/pathlength-small.png "경로 길이 페이지 스크린샷 삼중")](information-images/pathlength-large.png#lightbox "삼중 경로 길이 페이지 스크린샷")

베 지 어 곡선을 더 길거나 더 짧은 가져옵니다를 변경 하는 텍스트 크기를 볼 수 있습니다.

## <a name="traversing-the-path"></a>경로 트래버스합니다.

`SKPathMeasure` 이상의 측정값 경로의 길이 수행할 수 있습니다. 0과 경로 길이 사이의 모든 값에 대 한는 `SKPathMeasure` 개체는 해당 지점에서 경로 및 경로 곡선 탄젠트의 위치를 가져올 수 있습니다. 탄젠트 벡터의 형태로 제공 됩니다는 `SKPoint` 개체 또는 회전으로 캡슐화를 `SKMatrix` 개체입니다. 여기의 가지가 `SKPathMeasure` 다양 하 고 유연한 방식으로이 정보를 가져오고:

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

멤버는 [ `SKPathMeasureMatrixFlags` ](xref:SkiaSharp.SKPathMeasureMatrixFlags) 열거:

- `GetPosition`
- `GetTangent`
- `GetPositionAndTangent`

합니다 **자전거 절반 파이프** 페이지 사람 모양 아이콘이 입방 형 3 차원 큐빅 곡선을 따라 앞뒤로를 보이는 자전거에 애니메이션 효과 줍니다.

[![](information-images/unicyclehalfpipe-small.png "삼중 자전거 절반 파이프 페이지 스크린샷")](information-images/unicyclehalfpipe-large.png#lightbox "삼중 자전거 절반 파이프 페이지 스크린샷")

합니다 `SKPaint` 절반-파이프와는 자전거 선 그리기에 사용 되는 개체의 필드로 정의 됩니다 합니다 [ `UnicycleHalfPipePage` ]() 클래스입니다. 또한 정의 `SKPath` 는 자전거에 대 한 개체:

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

클래스의 표준 재정의 포함 합니다 `OnAppearing` 고 `OnDisappearing` 애니메이션에 대 한 메서드. `PaintSurface` 처리기 절반 파이프에 대 한 경로 만들고 다음을 그립니다. `SKPathMeasure` 개체는 다음이 경로에 따라 만들어집니다.

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

합니다 `PaintSurface` 의 값을 계산 하는 처리기 `t` 는 이동 0에서 1 5 초 마다. 사용 하 여는 `Math.Cos` 함수는 값으로 변환할 `t` 는 범위는 0에서 1을 0으로 다시 1은 오른쪽 맨 위에 있는 자전거에 해당 하는 동안 0에서 왼쪽, 위쪽에서 시작 하는 자전거를 해당 위치 합니다. 코사인 함수는 파이프의 맨 위에 있는 가장 느린 고 맨 아래에서 가장 빠른 속도 하면 됩니다.

이 값 `t` 첫 번째 인수에 대 한 경로 길이 곱한 수 해야 `GetMatrix`합니다. 행렬에 적용 되는 `SKCanvas` 자전거 패스 그리기에 대 한 개체입니다.

## <a name="enumerating-the-path"></a>경로 열거합니다.

클래스를 포함 하는 두 개의 `SKPath` 경로의 내용을 열거할 수 있습니다. 이러한 클래스는 [ `SKPath.Iterator` ](xref:SkiaSharp.SKPath.Iterator) 하 고 [ `SKPath.RawIterator` ](xref:SkiaSharp.SKPath.RawIterator)합니다. 두 클래스는 매우 유사 하지만 `SKPath.Iterator` 경로 길이가 0 인, 또는 길이가 0에 가까운 요소를 제거할 수 있습니다. `RawIterator` 아래 예제에 사용 됩니다.

형식의 개체를 가져올 수 있습니다 `SKPath.RawIterator` 를 호출 하 여 합니다 [ `CreateRawIterator` ](xref:SkiaSharp.SKPath.CreateRawIterator) 메서드의 `SKPath`합니다. 반복적으로 호출 하 여 수행 됩니다 경로 통해 열거 합니다 [ `Next` ](xref:SkiaSharp.SKPath.RawIterator.Next*) 메서드. 4 개의 배열을 전달 `SKPoint` 값:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

합니다 `Next` 의 멤버를 반환 하는 메서드를 [ `SKPathVerb` ](xref:SkiaSharp.SKPathVerb) 열거형 형식입니다. 이러한 값을 경로에 특정 그리기 명령을 나타냅니다. 배열에 삽입 하는 유효한 점 개수가이 동사에 따라 달라 집니다.

- `Move` 단일 지점
- `Line` 두 점과 함께
- `Cubic` 점이 4 개를 사용 하 여
- `Quad` 세 개의 점을 사용 하 여
- `Conic` 세 개의 점을 사용 하 여 (또한 호출을 [ `ConicWeight` ](xref:SkiaSharp.SKPath.RawIterator.ConicWeight*) 가중치에 대 한 메서드)
- `Close` 하나의 점에서
- `Done`

`Done` 동사 경로 열거형 완료 되었음을 나타냅니다.

있으며 없습니다 `Arc` 동사입니다. 이 모든 원호 경로에 추가 하는 경우에 베 지 어 곡선으로 변환 됩니다 것을 나타냅니다.

에 있는 정보의 일부를 `SKPoint` 배열 중복 됩니다. 예를 들어 경우는 `Move` 뒤에 동사를 `Line` 동사를 수반 되는 두 요소의 첫 번째는 `Line` 와 같습니다는 `Move` 지점. 실제로이 중복 매우 유용합니다. 표시 되는 경우는 `Cubic` 동사 입방 형 3 차원 큐빅 곡선을 정의 하는 모든 네 가지 요소와 함께 합니다. 이전 동사 설정한 현재 위치를 유지할 필요가 없습니다.

문제가 있는 동사에 것 인데, `Close`합니다. 이 명령은 현재 위치에서에서 이전에 설정 된 윤곽선의 시작 부분에 직선을 그립니다는 `Move` 명령입니다. 이상적으로 `Close` 동사를 하나의 지점 보다는이 두 지점을 제공 해야 합니다. 더욱 심각한 함께 제공 되는 지점에는 `Close` 동사는 항상 (0, 0). 경로 통해 열거 하는 경우에 유지 해야 아마도 `Move` 지점과 현재 위치입니다.

## <a name="enumerating-flattening-and-malforming"></a>열거, 병합, 및 Malforming

알고리즘을 적용 하는 것이 바람직 경우가 변환 malform에 대 한 경로에 어떤 방식으로든에서:

![](information-images/pathenumerationsample.png "반구에서 텍스트")

이러한 문자는 대부분 직선 이루어진 아직 곡선에 이러한 직선을 twisted 분명히 있어야 합니다. 어떻게 이것이 가능 한가요?

키가 원래 직선은 일련의 작은 직선으로 분류 됩니다. 그런 다음 곡선을 형성 하는 다양 한 방법의 더 작은 직선 이러한 개별을 조작할 수 있습니다. 

이 프로세스를 사용 하는 데는 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 샘플에는 정적 [ `PathExtensions` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) 클래스를 `Interpolate` 구분 하는 메서드를 하나의 단위 길이에 다양 한 짧은 줄에는 직선입니다. 또한 클래스에는 일련의 작은 직선 곡선 근사치는 세 가지 유형의 베 지 어 곡선으로 변환 하는 여러 메서드가 들어 있습니다. (문서의 파라메트릭 수식에 표시 된 [ **베 지 어 곡선의 세 가지 형식**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md).) 이 프로세스를 호출할 _평면화_ 곡선:

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

이러한 모든 메서드는 확장 메서드에서 참조 `CloneWithTransform` 도이 클래스에 포함 되어 있으며 아래에 표시 합니다. 이 메서드는 경로 명령을 열거 하 고 데이터를 기반으로 새 경로 생성 하 여 경로 복제 합니다. 그러나 새 경로 으로만 이루어진 `MoveTo` 고 `LineTo` 호출 합니다. 모든 곡선 및 직선 일련의 작은 줄으로 세분화 됩니다.

호출할 때 `CloneWithTransform`, 메서드에 전달할를 `Func<SKPoint, SKPoint>`를 사용 하는 함수는는 `SKPaint` 반환 하는 매개 변수는 `SKPoint` 값. 이 함수는 사용자 지정 알고리즘 변환을 적용 하려면 모든 지점에 대 한 호출 됩니다.

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

메서드를 각 윤곽선 이라는 변수에 있는 첫 번째 지점에서 유지 하는 공지 `firstPoint` 변수에 현재 위치 각 그리기 명령 및 `lastPoint`합니다. 이러한 변수는 마지막 닫는 생성 하는 데 필요한 경우 줄을 `Close` 동사 발생 합니다.

합니다 **GlobularText** 샘플이 확장 메서드를 사용 하 여 보이는 텍스트 주위에 배치할 반구 3D 효과 냅니다.

[![](information-images/globulartext-small.png "삼중 Globular 텍스트 페이지 스크린샷")](information-images/globulartext-large.png#lightbox "삼중 Globular 텍스트 페이지 스크린샷")

합니다 [ `GlobularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) 클래스 생성자는이 변환을 수행 합니다. 만듭니다는 `SKPaint` 텍스트에 대해 개체를 가져오는 다음을 `SKPath` 에서 개체를 `GetTextPath` 메서드. 이 전달 된 경로 `CloneWithTransform` 변환 함수를 함께 확장 메서드:

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

변환 함수는 먼저 라는 두 개의 값을 계산 `longitude` 고 `latitude` – π/2 맨 위 및 텍스트의 왼쪽에서에서 오른쪽 텍스트의 맨 아래에서 π/2 사이의 합니다. 이러한 값의 범위는 시각적으로 만족 하지 않으므로 0.75를 곱하여 인하 됩니다. (이러한 조정 하지 않고 코드를 사용해 보십시오. 텍스트가 너무 북부 및 중남부 극 지방에서 모호한 측면에서 씬.) 이러한 3 차원 구형 좌표에 2 차원 변환 됩니다 `x` 고 `y` 표준 수식 좌표입니다.

새 경로 필드로 저장 됩니다. `PaintSurface` 처리기 다음 단순히 해야 중심 및 화면에 표시 하는 경로 확장 합니다.

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

이 매우 다양 한 기술입니다. 경로 효과의 배열에서 설명 하는 경우는 [ **경로 효과** ](effects.md) 문서 항목을 포함 시켜야 느낌 둘러싸지 매우 않습니다 채울 방법이이 있습니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
