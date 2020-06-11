---
제목: "폴리라인 및 패라메트릭 방정식" 설명: "이 문서에서는 SkiaSharp를 사용 하 여 패라메트릭 방정식으로 정의할 수 있는 선을 렌더링 하는 방법을 설명 하 고 샘플 코드를 사용 하 여이를 보여 줍니다."
assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE: xamarin-skiasharp author: davidbritch: dabritch:: 03/10/2017:: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="polylines-and-parametric-equations"></a>폴리라인 및 파라메트릭 수식

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp를 사용 하 여 패라메트릭 방정식으로 정의할 수 있는 선을 렌더링 합니다._

이 가이드의 [**SkiaSharp 곡선 및 경로**](../curves/index.md) 섹션에는 [`SKPath`](xref:SkiaSharp.SKPath) 특정 유형의 곡선을 렌더링 하기 위해 정의 하는 다양 한 메서드가 표시 됩니다. 그러나에서 직접 지원 하지 않는 곡선의 형식을 그려야 하는 경우도 있습니다 `SKPath` . 이러한 경우에는 폴리라인 (연결 된 선의 컬렉션)를 사용 하 여 수학적으로 정의할 수 있는 곡선을 그릴 수 있습니다. 줄 수를 충분히 작게 설정 하면 결과는 곡선 처럼 보입니다. 이 나선형은 실제로 3600 작은 줄입니다.

![](polylines-images/spiralexample.png "A spiral")

일반적으로 패라메트릭 방정식 쌍의 측면에서 곡선을 정의 하는 것이 가장 좋습니다. 이는 세 번째 변수에 종속 된 X 및 Y 좌표의 방정식으로, 때때로 `t` 시간에 대해 호출 됩니다. 예를 들어 다음 패라메트릭 방정식은 0에서 1 사이의 지점 (0, 0 *)에 중점* 을 둘 수 있는 원을 정의 합니다.

`x = cos(2πt)`

`y = sin(2πt)`

 반지름이 1 보다 큰 경우에는 해당 반지름을 기준으로 사인 및 코사인 값을 곱할 수 있으며, 중심을 다른 위치로 이동 해야 하는 경우에는 해당 값을 추가 합니다.

`x = xCenter + radius·cos(2πt)`

`y = yCenter + radius·sin(2πt)`

축이 가로 및 세로 방향으로 평행한 타원의 경우 두 개의 반지름이 사용 됩니다.

`x = xCenter + xRadius·cos(2πt)`

`y = yCenter + yRadius·sin(2πt)`

그런 다음 다양 한 요소를 계산 하 고이를 경로에 추가 하는 루프에 동일한 SkiaSharp 코드를 넣을 수 있습니다. 다음 SkiaSharp 코드는 `SKPath` 표시 표면을 채우는 타원에 대 한 개체를 만듭니다. 루프는 360도를 직접 순환 합니다. 중심은 표시 표면의 너비와 높이의 절반 이며, 그에 해당 하는 두 반지름입니다.

```csharp
SKPath path = new SKPath();

for (float angle = 0; angle < 360; angle += 1)
{
    double radians = Math.PI * angle / 180;
    float x = info.Width / 2 + (info.Width / 2) * (float)Math.Cos(radians);
    float y = info.Height / 2 + (info.Height / 2) * (float)Math.Sin(radians);

    if (angle == 0)
    {
        path.MoveTo(x, y);
    }
    else
    {
        path.LineTo(x, y);
    }
}
path.Close();
```

이렇게 하면 360 줄에 의해 정의 된 타원이 생성 됩니다. 렌더링 되 면 부드러운 것으로 나타납니다.

물론를 사용 하는 메서드를 포함 하므로 다중선을 사용 하 여 타원을 만들 필요가 없습니다 `SKPath` `AddOval` . 그러나에서 제공 하지 않는 시각적 개체를 그릴 수 있습니다 `SKPath` .

**Archimedean 나선형** 페이지에는 타원 코드와 비슷하지만 중요 한 차이점이 있는 코드가 있습니다. 원을 지속적으로 조정 하 여 원의 360도를 10 번 반복 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(center.X, center.Y);

    using (SKPath path = new SKPath())
    {
        for (float angle = 0; angle < 3600; angle += 1)
        {
            float scaledRadius = radius * angle / 3600;
            double radians = Math.PI * angle / 180;
            float x = center.X + scaledRadius * (float)Math.Cos(radians);
            float y = center.Y + scaledRadius * (float)Math.Sin(radians);
            SKPoint point = new SKPoint(x, y);

            if (angle == 0)
            {
                path.MoveTo(point);
            }
            else
            {
                path.LineTo(point);
            }
        }

        SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 5
        };

        canvas.DrawPath(path, paint);
    }
}
```

각 루프 간의 오프셋이 일정 하기 때문에 결과를 *산술 나선형* 이라고 합니다.

[![](polylines-images/archimedeanspiral-small.png "Triple screenshot of the Archimedean Spiral page")](polylines-images/archimedeanspiral-large.png#lightbox "Triple screenshot of the Archimedean Spiral page")

는 `SKPath` 블록에 생성 됩니다 `using` . 이렇게 하면 `SKPath` 이전 프로그램의 개체 보다 더 많은 메모리가 사용 됩니다 .이는 `SKPath` `using` 블록이 관리 되지 않는 리소스를 삭제 하는 데 더 적합 함을 나타냅니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
