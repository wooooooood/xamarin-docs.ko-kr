---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 207b16f062a5c2137ac5fc3c21775d2486fda57d
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135865"
---
# <a name="the-skew-transform"></a>기울이기 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_기울이기 변환으로 SkiaSharp에서 기울어진 그래픽 개체를 만드는 방법을 참조 하세요._

SkiaSharp에서 기울이기 변환은이 이미지의 그림자와 같은 그래픽 개체를 tilts 합니다.

![](skew-images/skewexample.png "An example of skewing from the Skew Shadow Text program")

기울이기는 사각형이 평행 사변형으로 바뀌고 기울어진 타원이 여전히 타원입니다.

Xamarin.Forms는 변환, 배율 조정 및 회전에 대 한 속성을 정의 하지만에는 기울이기에 해당 하는 속성이 없습니다 Xamarin.Forms .

[`Skew`](xref:SkiaSharp.SKCanvas.Skew(System.Single,System.Single))의 메서드는 `SKCanvas` 가로 기울이기 및 세로 기울이기에 대해 두 개의 인수를 허용 합니다.

```csharp
public void Skew (Single xSkew, Single ySkew)
```

두 번째 [`Skew`](xref:SkiaSharp.SKCanvas.Skew(SkiaSharp.SKPoint)) 메서드는 이러한 인수를 단일 값으로 결합 합니다 `SKPoint` .

```csharp
public void Skew (SKPoint skew)
```

그러나 이러한 두 가지 방법 중 하나를 격리에 사용 하는 것은 거의 없습니다.

**실험 기울이기** 페이지에서-10에서 10 사이의 오차를 기울일 수 있습니다. 텍스트 문자열은 페이지의 왼쪽 위 모퉁이에 배치 되며, 두 요소에서 얻은 기울이기 값이 `Slider` 있습니다. `PaintSurface`클래스의 처리기는 다음과 같습니다 [`SkewExperimentPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) .

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        canvas.Skew((float)xSkewSlider.Value, (float)ySkewSlider.Value);
        canvas.DrawText(text, 0, -textBounds.Top, textPaint);
    }
}
```

인수의 값은 `xSkew` 양수 값에 대해 텍스트 오른쪽의 아래쪽을 이동 하거나 음수 값을 왼쪽으로 이동 합니다. 값 `ySkew` 은 양수 값 또는 음수 값의 경우 텍스트 오른쪽을 아래로 이동 합니다.

[![](skew-images/skewexperiment-small.png "Triple screenshot of the Skew Experiment page")](skew-images/skewexperiment-large.png#lightbox "Triple screenshot of the Skew Experiment page")

값 `xSkew` 이 음수 이면 `ySkew` 결과는 회전이 며 약간 크기를 조정 하기도 합니다.

변환 수식은 다음과 같습니다.

x ' = x + xSkew · x.y

y ' = ySkew · x + y

예를 들어 양수 값의 경우 `xSkew` 변환 된 `x'` 값이 `y` 늘어나면 늘어납니다. 이로 인해 기울기가 발생 합니다.

삼각형 200 픽셀 너비와 100 픽셀의 위쪽이 지점 (0, 0)에서 왼쪽 위 모퉁이에 배치 되 고 1.5 값으로 렌더링 되는 경우 `xSkew` 다음 평행 사변형 결과가 반환 됩니다.

![](skew-images/skeweffect.png "The effect of the skew transform on a rectangle")

아래쪽 가장자리의 좌표는 `y` 100 값을 가지 므로 150 픽셀을 오른쪽으로 이동 합니다.

또는의 0이 아닌 값의 경우 `xSkew` `ySkew` 점 (0, 0)만 동일 하 게 유지 됩니다. 이 지점은 기울이기 중심으로 간주할 수 있습니다. 다른 항목 (일반적으로 경우)으로 기울이기의 중심이 필요한 경우 `Skew` 에는이를 제공 하는 메서드가 없습니다. 호출을 사용 하 여 호출을 명시적으로 결합 해야 `Translate` `Skew` 합니다. 및에서 기울이기를 가운데로 맞추려면 `px` `py` 다음을 호출 합니다.

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

복합 변환 수식은 다음과 같습니다.

x ' = x + xSkew · (y – py)

y ' = ySkew · (x – px) + y

`ySkew`이 0 이면 `px` 값이 사용 되지 않습니다. 값은 관련이 없으며 및의 경우에도 마찬가지입니다 `ySkew` `py` .

이 다이어그램의 각도 α 같이 기울기 각도로 기울이기를 지정 하는 것이 더 편리할 수 있습니다.

![](skew-images/skewangleeffect.png "The effect of the skew transform on a rectangle with a skewing angle indicated")

100 픽셀 세로 까지의 150 픽셀 시프트 비율은 해당 각도의 탄젠트 (이 예에서는 56.3도)입니다.

**기울기 각도 실험** 페이지의 XAML 파일은 요소가-90도에서 90도까지 범위를 제외 하 고는 **기울기 각도** 페이지와 비슷합니다 `Slider` . 코드를 사용 하는 [`SkewAngleExperiment`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) 파일은 페이지의 텍스트를 가운데에 맞춥니다 .를 사용 하 여 `Translate` 페이지의 중심으로 기울이기 중심을 설정 합니다. `SkewDegrees`코드 아래쪽의 short 메서드는 각도를 기울기 값으로 변환 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;

        canvas.Translate(xCenter, yCenter);
        SkewDegrees(canvas, xSkewSlider.Value, ySkewSlider.Value);
        canvas.Translate(-xCenter, -yCenter);
        canvas.DrawText(text, xText, yText, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

각도는 양수 또는 음수 90도에 도달 하는 반면, 탄젠트는 무한대로 반올림 되지만 최대 80도까지 각도를 사용할 수 있습니다.

[![](skew-images/skewangleexperiment-small.png "Triple screenshot of the Skew Angle Experiment page")](skew-images/skewangleexperiment-large.png#lightbox "Triple screenshot of the Skew Angle Experiment page")

작은 음수 가로 기울이기는 오블리크 **텍스트** 페이지에서 보여 주는 것 처럼 오블리크 또는 기울임꼴 텍스트를 모방 합니다. [`ObliqueTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs)클래스는 수행 방법을 보여 줍니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Maroon,
        TextAlign = SKTextAlign.Center,
        TextSize = info.Width / 8       // empirically determined
    })
    {
        canvas.Translate(info.Width / 2, info.Height / 2);
        SkewDegrees(canvas, -20, 0);
        canvas.DrawText(Title, 0, 0, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

`TextAlign`의 속성 `SKPaint` 은로 설정 됩니다 `Center` . 변환이 없는 경우 `DrawText` (0, 0) 좌표가 있는 호출은 왼쪽 위 모퉁이에 있는 기준선의 가로 가운데를 사용 하 여 텍스트를 배치 합니다. 는 `SkewDegrees` 기준선을 기준으로 텍스트를 가로 20도로 기울입니다. 이 `Translate` 호출은 텍스트 기준선의 가로 가운데를 캔버스의 가운데로 이동 합니다.

[![](skew-images/obliquetext-small.png "Triple screenshot of the Oblique Text page")](skew-images/obliquetext-large.png#lightbox "Triple screenshot of the Oblique Text page")

**그림자 텍스트 기울이기** 페이지에서는 45 수준 기울이기와 세로 비율의 조합을 사용 하 여 텍스트에서 tilts 하는 텍스트 그림자를 만드는 방법을 보여 줍니다. 처리기의 관련 부분은 `PaintSurface` 다음과 같습니다.

```csharp
using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = info.Width / 6;   // empirically determined

    // Common to shadow and text
    string text = "Shadow";
    float xText = 20;
    float yText = info.Height / 2;

    // Shadow
    textPaint.Color = SKColors.LightGray;
    canvas.Save();
    canvas.Translate(xText, yText);
    canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
    canvas.Scale(1, 3);
    canvas.Translate(-xText, -yText);
    canvas.DrawText(text, xText, yText, textPaint);
    canvas.Restore();

    // Text
    textPaint.Color = SKColors.Blue;
    canvas.DrawText(text, xText, yText, textPaint);
}
```

그림자가 먼저 표시 된 다음 텍스트는 다음과 같이 표시 됩니다.

[![](skew-images/skewshadowtext1-small.png "Triple screenshot of the Skew Shadow Text page")](skew-images/skewshadowtext1-large.png#lightbox "Triple screenshot of the Skew Shadow Text page")

메서드에 전달 된 세로 좌표는 `DrawText` 기준선을 기준으로 하는 텍스트의 위치를 나타냅니다. 기울이기 중심에 사용 되는 것과 동일한 세로 좌표입니다. 텍스트 문자열에 디센더가 포함 된 경우에는이 방법이 작동 하지 않습니다. 예를 들어 "quirky" 라는 단어를 "Shadow"로 대체 하 고 결과를 확인 합니다.

[![](skew-images/skewshadowtext2-small.png "Triple screenshot of the Skew Shadow Text page with an alternative word with descenders")](skew-images/skewshadowtext2-large.png#lightbox "Triple screenshot of the Skew Shadow Text page with an alternative word with descenders")

그림자와 텍스트는 여전히 기준선에서 정렬 되지만 효과가 잘못 된 것으로 보입니다. 이 문제를 해결 하려면 텍스트 범위를 가져와야 합니다.

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

`Translate`호출을 하강 높이로 조정 해야 합니다.

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

이제 그림자가 해당 디센더의 아래쪽부터 확장 됩니다.

[![](skew-images/skewshadowtext3-small.png "Triple screenshot of the Skew Shadow Text page with adjustments for descenders")](skew-images/skewshadowtext3-large.png#lightbox "Triple screenshot of the Skew Shadow Text page with adjustments for descenders")

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
