---
title: 기울이기 변환
description: 이 문서에서는 기울이기 변환에서 SkiaSharp, 기운된 그래픽 개체를 만들 수 있습니다 하는 방법에 대해 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: davidbritch
ms.author: dabritch
ms.date: 03/20/2017
ms.openlocfilehash: 0592f80b7d7352463ba22d7d371cc1b18ac3e0be
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655574"
---
# <a name="the-skew-transform"></a>기울이기 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_기울이기 변환에서 SkiaSharp 기운된 그래픽 개체를 만들 수 있습니다 하는 방법을 참조 하세요._

SkiaSharp, 기울이기 변환 그림자가 그림과에서 같은 그래픽 개체를 기울이는:

![](skew-images/skewexample.png "그림자 텍스트 기울이기 프로그램에서 기울이기의 예")

오차는 평행 사변형에 사각형을 설정 하지만 불균형된 타원은 타원 계속 합니다.

Xamarin.Forms 변환, 배율 및 회전에 대 한 속성을 정의 하지만 해당 속성이 없는 Xamarin.Forms의 오차에 대 한 합니다.

합니다 [ `Skew` ](xref:SkiaSharp.SKCanvas.Skew(System.Single,System.Single)) 메서드의 `SKCanvas` 기울이기 세로 및 가로 기울이기를 대 한 두 개의 인수를 허용 합니다.

```csharp
public void Skew (Single xSkew, Single ySkew)
```

두 번째 [ `Skew` ](xref:SkiaSharp.SKCanvas.Skew(SkiaSharp.SKPoint)) 메서드는 이러한 인수는 단일에서 결합 `SKPoint` 값:

```csharp
public void Skew (SKPoint skew)
```

그러나 그럴 가능성은 사용할 두 가지 방법 중 하나에서 격리 합니다.

합니다 **실험 기울이기** 오차를 사용 하 여 실험 페이지 있습니다 – 10에서 10 사이의 사이의 값입니다. 텍스트 문자열에서 두 가져온 오차 값을 사용 하 여 페이지의 왼쪽 위 모퉁이에 배치 됩니다 `Slider` 요소입니다. 다음은 `PaintSurface` 처리기에는 [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) 클래스:

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

값을 `xSkew` 양수 값에 대 한 오른쪽 텍스트 또는 음수 값에 대 한 왼쪽의 아래쪽을 이동 하는 인수입니다. 값 `ySkew` 양수 또는 음수 값에 대해 텍스트의 오른쪽 이동 합니다.

[![](skew-images/skewexperiment-small.png "기울이기 실험 페이지의 3 배가 스크린 샷")](skew-images/skewexperiment-large.png#lightbox "삼중 기울이기 실험 페이지 스크린샷")

경우는 `xSkew` 값은 음수를 `ySkew` 값 결과 회전 이지만으로 다소 비율도 UWP 표시를 나타냅니다.

변환 수식에는 다음과 같습니다.

x' = x + xSkew? y

y' ySkew? = x + y

양수에 대 한 예를 들어 `xSkew` 값을 변환 된 `x'` 값으로 증가 `y` 증가 합니다. 기울기 원인입니다.

왼쪽 위 모퉁이가 점 (0, 0)를 배치 하는 사용 하 여 렌더링 되는 삼각형 200 픽셀 너비 고 100 픽셀 짜리는 `xSkew` 값이 1.5, 다음 평행 사변형 결과:

![](skew-images/skeweffect.png "기울이기 변환 사각형에 끼치는 영향")

아래쪽 가장자리의 좌표의 `y` 값이 100 인 이므로 150 픽셀을 오른쪽으로 옮겨집니다.

값이 0이 아닌 `xSkew` 또는 `ySkew`를 점 (0, 0) 동일 하 게 유지 합니다. 이때 기울이기 원의 간주할 수 있습니다. 그 밖의 내용은 되도록 기울이기의 가운데 (이 일반적으로 대/소문자)을 해야 하는 경우 없습니다 `Skew` 메서드를 제공 하는 합니다. 명시적으로 결합 해야 `Translate` 사용 하 여 호출 된 `Skew` 호출 합니다. 가운데에서 기울이기 `px` 및 `py`, 다음 호출을 수행 합니다.

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

복합 변환 수식은 다음과 같습니다.

x' = x + xSkew? (y-py)

y' ySkew? = (x-px) + y

하는 경우 `ySkew` 가 0 이면 해당 `px` 값은 사용 되지 않습니다. 값을 관련 되지 않습니다 및 마찬가지로 `ySkew` 고 `py`입니다.

수 편하다면 기울기 각도 α가 다이어그램과에서 같은 기울기 각도를 지정 합니다.

![](skew-images/skewangleeffect.png "기울이기 변환 기울이기 각도 사용 하 여 사각형에 미치는 표시")

100 픽셀 세로 150 픽셀 shift의 비율은이 예에서 해당 각도의 탄젠트 56.3 (도).

XAML 파일을를 **각도 실험 기울이기** 비슷합니다는 **기울이기 각도** 는 제외 하 고 페이지를 `Slider` 요소 인에서 90도 사이입니다. [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) 코드 숨김 파일 페이지에서 텍스트를 가운데 정렬 하 고 사용 하 여 `Translate` 기울이기 페이지의 가운데를 중심을 설정 합니다. 짧은 `SkewDegrees` 메서드 코드의 맨 아래에서 각도 값 기울이기 변환 합니다.

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

각도 양수 또는 음수 90도 다가오면 탄젠트 가까워지면 무한대 있지만 약 80도 정도까지 각도 사용할 수 없습니다.

[![](skew-images/skewangleexperiment-small.png "기울이기 각도 실험 페이지의 3 배가 스크린 샷")](skew-images/skewangleexperiment-large.png#lightbox "삼중 기울이기 각도 실험 페이지 스크린샷")

작은 음수 가로 기울이기를으로 오블리크 또는 기울임꼴 텍스트를 모방 합니다 **오블리크 텍스트** 페이지를 보여 줍니다. 합니다 [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) 클래스가 어떻게 수행 되는지 보여 줍니다.

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

합니다 `TextAlign` 속성을 `SKPaint` 로 설정 된 `Center`합니다. 모든 변환 없이 `DrawText` 의 좌표를 사용 하 여 호출 (0, 0) 왼쪽 위 모퉁이에서 기준의 가로 가운데를 사용 하 여 텍스트의 위치는입니다. `SkewDegrees` 기준선을 기준으로 20도 텍스트를 가로로 기울어집니다. `Translate` 호출 캔버스의 가운데에 텍스트의 기준선의 가로 가운데를 이동 합니다.

[![](skew-images/obliquetext-small.png "오블리크 텍스트 페이지의 3 배가 스크린샷")](skew-images/obliquetext-large.png#lightbox "삼중 오블리크 텍스트 페이지 스크린샷")

합니다 **그림자 텍스트 기울이기** 페이지 텍스트에서 기울이는 텍스트 그림자 45도 기울이기 및 세로 눈금을 조합해 서 사용 하는 방법에 설명 합니다. 관련 부분은 여기는 `PaintSurface` 처리기:

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

그림자가 표시 된 첫 번째 및 다음 텍스트:

[![](skew-images/skewshadowtext1-small.png "기울이기 섀도 텍스트 페이지의 3 배가 스크린샷")](skew-images/skewshadowtext1-large.png#lightbox "삼중 기울이기 섀도 텍스트 페이지 스크린샷")

에 전달 하는 세로 좌표는 `DrawText` 메서드 기준선을 기준으로 텍스트의 위치를 나타냅니다. 기울이기의 중심에 사용 되는 동일한 세로 좌표입니다. 텍스트 문자열 디센더를 포함 하는 경우에이 기술은 작동 하지 않습니다. 예를 들어, "섀도"에 대 한 "마구" 라는 단어를 대체 하 고 결과 다음과 같습니다.

[![](skew-images/skewshadowtext2-small.png "삼중 디센더를 사용 하 여 대체 단어를 사용 하 여 섀도 텍스트 기울이기 페이지 스크린샷")](skew-images/skewshadowtext2-large.png#lightbox "삼중 디센더를 사용 하 여 대체 단어를 사용 하 여 섀도 텍스트 기울이기 페이지 스크린샷")

섀도 및 텍스트는 기준선에 맞춘 여전히 있지만 효과 깔 잘못 된 합니다. 이 문제를 해결 하려면 텍스트 범위를 가져오려고 합니다.

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

`Translate` 호출 디센더의 높이로 조정 해야 합니다.

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

이제 이러한 디센더와 맨 아래에서 그림자를 확장합니다.

[![](skew-images/skewshadowtext3-small.png "삼중 하강 조정 기울이기 섀도 텍스트 페이지 스크린샷")](skew-images/skewshadowtext3-large.png#lightbox "삼중 하강 조정 기울이기 섀도 텍스트 페이지 스크린샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
