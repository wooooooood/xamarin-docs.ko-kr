---
title: 시간차 변환
description: 로 인해 기울이기 변환을 SkiaSharp의 기운된 그래픽 개체를 만들 수는 방법을 참조 하세요.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: charlespetzold
ms.author: chape
ms.date: 03/20/2017
ms.openlocfilehash: c9f5f9f20296b1c2443a8addeebd4d12ccaa1ab4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="the-skew-transform"></a>시간차 변환

_로 인해 기울이기 변환을 SkiaSharp의 기운된 그래픽 개체를 만들 수는 방법을 참조 하세요._

SkiaSharp, 기울이기 변환을이 이미지에 그림자 같은 그래픽 개체를 기울이는:

![](skew-images/skewexample.png "텍스트 그림자를 왜곡 시킬 프로그램에서 기울이기의 예")

기울이기를 parallelograms, 사각형 변환 하지만 왜곡 된 타원은 타원 여전히 합니다.

Xamarin.Forms는 변환, 크기 조정 및 회전에 대 한 속성을 정의 하지만에 없는 경우 해당 속성이 Xamarin.Forms 오차에 대 한

[ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/System.Single/System.Single/) 방식의 `SKCanvas` 수직 및 수평 기울이기에 대 한 두 개의 인수를 왜곡 시킬 허용:

```csharp
public void Skew (Single xSkew, Single ySkew)
```

두 번째 [ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/SkiaSharp.SKPoint/) 메서드는 해당 인수가 단일에서 결합 `SKPoint` 값:

```csharp
public void Skew (SKPoint skew)
```

그러나 그럴 가능성은 있는지 사용할 것이 두 가지 방법 중 하나 분리에서 합니다.

**실험 기울이기** – 10에서 10 사이의 범위에 값이 왜곡을 시도할 때 페이지 수 있습니다. 텍스트 문자열을 배치 하는 페이지의 왼쪽 위 모서리에 두 개에서 얻은 값을 시간차 `Slider` 요소입니다. 다음은 `PaintSurface` 의 처리기는 [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) 클래스:

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

값은 `xSkew` 인수 값은 양수 값에 대 한 오른쪽 텍스트 또는 음수 값에 대 한 왼쪽 맨 이동 합니다. 값 `ySkew` 양수 또는 음수 값에 대해 아래쪽 텍스트를 오른쪽으로 이동 합니다.

[![](skew-images/skewexperiment-small.png "기울이기 실험 페이지의 삼중 스크린샷")](skew-images/skewexperiment-large.png#lightbox "기울이기 실험 페이지의 삼중 스크린샷")

경우 `xSkew` 의 음수는 `ySkew`, 결과 회전 이지만 다소 표시 창으로 조절 합니다.

변형 수식은 다음과 같습니다.

x' = x + xSkew · y

y' ySkew · = x + y

양수에 대 한 예를 들어 `xSkew` 값을 변환 된 `x'` 값 늘어나면서 `y` 증가 합니다. 기울기 발생 원인입니다.

삼각형 200 픽셀 너비와 100 픽셀 지점 (0, 0)에서 왼쪽 위 모퉁이 배치 되 고 사용 하 여 렌더링 하는 경우는 `xSkew` 1.5 다음 평행 사변형 결과의 값:

![](skew-images/skeweffect.png "시간차 변환의 사각형에 미치는 영향")

좌표의 아래쪽 가장자리의 `y` 값 되기 때문에 100, 150 픽셀 위치를 오른쪽으로 이동 합니다.

값이 0이 아닌 `xSkew` 또는 `ySkew`동일 하 게 유지 지점 (0, 0)만 필요 합니다. 해당 시점 기울이기의 중심을 간주할 수 있습니다. 다른 값인지 되도록 기울이기의 가운데 (이 일반적으로 경우)를 해야 하는 경우 차이가 없습니다 `Skew` 있는 제공 하는 메서드. 명시적으로 결합 해야 `Translate` 으로 호출 하는 `Skew` 호출 합니다. 가운데에서 기울이기 하려면 `px` 및 `py`, 다음 호출:

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

합성 변형 수식에는

x' = x + xSkew · (y-py)

y' ySkew · = (x-px) + y

경우 `ySkew` 는 0이 고 0이 아닌 값만 지정 하는 `xSkew`, 다음 `px` 값은 사용 되지 않습니다. 값이 관련와 마찬가지로 `ySkew` 및 `py`합니다.

기울기 각도 α가이 다이어그램에서 같은 각도으로 시간차 지정 더 잘 알고 수 있습니다.:

![](skew-images/skewangleeffect.png "지정 된 기울이기 각도 사각형에 시간차 변환의 영향")

이 예에서 해당 각도의 탄젠트 100 픽셀 세로 150 픽셀 작업의 비율을은 56.3도입니다.

XAML 파일은 **각도 실험 기울이기** 페이지는 비슷합니다는 **각도 왜곡 시킬** 점을 제외 하 고 페이지는 `Slider` 요소 –90에서 90도 사이입니다. [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) 코드 숨김 파일 페이지에서 텍스트를 가운데 정렬 하 고 사용 하 여 `Translate` 페이지의 가운데에 기울이기의 중심을 설정할 수 있습니다. 짧은 `SkewDegrees` 메서드 코드 맨 아래에 각도를 기울입니다 값으로 변환 합니다.

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

양수 또는 음수 90도 각도로 가까워지면 탄젠트 무한대에 도달 하지만 약 80도 정도까지 각도 사용할 수 있습니다:

[![](skew-images/skewangleexperiment-small.png "각도 실험 기울이기 페이지의 삼중 스크린샷")](skew-images/skewangleexperiment-large.png#lightbox "각도 실험 기울이기 페이지의 삼중 스크린샷")

작은 음수 수평 기울이기도 기울임꼴 또는 오블리크 텍스트를 모방 하면는 **오블리크 텍스트** 페이지를 보여 줍니다. [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) 클래스가 수행 하는 방법을 보여 줍니다.

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

`TextAlign` 속성 `SKPaint` 로 설정 된 `Center`합니다. 변환, 없이 `DrawText` 의 좌표를 사용 하 여 호출 (0, 0) 왼쪽 위 모서리에 있는 기준의 가로 중심 텍스트의 위치는입니다. `SkewDegrees` 20도 기준선을 기준으로 텍스트를 가로로 기울입니다. `Translate` 호출 캔버스의 가운데에는 텍스트의 기준의 가로 가운데 이동:

[![](skew-images/obliquetext-small.png "오블리크 텍스트 페이지의 삼중 스크린샷")](skew-images/obliquetext-large.png#lightbox "오블리크 텍스트 페이지의 삼중 스크린 샷")

**텍스트 그림자를 왜곡 시킬** 페이지에 텍스트 외부로 기울이는 텍스트 그림자 45도 기울이기 및 세로 크기 조정의 조합을 사용 하는 방법을 보여 줍니다. 여기의 관련 일부인는 `PaintSurface` 처리기:

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

그림자 표시 한 다음 텍스트를:

[![](skew-images/skewshadowtext1-small.png "기울이기 텍스트 그림자 페이지의 삼중 스크린샷")](skew-images/skewshadowtext1-large.png#lightbox "기울이기 텍스트 그림자 페이지의 삼중 스크린샷")

에 전달 된 세로 좌표는 `DrawText` 메서드 기준선을 기준으로 텍스트의 위치를 나타냅니다. 기울이기의 가운데에 사용 되는 동일한 세로 좌표입니다. 텍스트 문자열 하강 문자를 포함 하는 경우에이 기술을 작동 하지 않습니다. 예를 들어 subsitute "그림자" 및 여기에 대 한 "사소" 라는 단어의 결과:

[![](skew-images/skewshadowtext2-small.png "디센더와 대체 단어를 왜곡 시킬 텍스트 그림자 페이지의 삼중 스크린샷")](skew-images/skewshadowtext2-large.png#lightbox "디센더와 대체 단어를 왜곡 시킬 텍스트 그림자 페이지의 삼중 스크린 샷")

그림자 및 텍스트는 초기에 정렬 됩니다 되지만 효과 보기 잘못 합니다. 해결 하는 텍스트 범위를 가져올 해야 합니다.

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

이제 이러한 디센더와의 맨 아래에서 그림자를 확장합니다.

[![](skew-images/skewshadowtext3-small.png "하강 조정 기울이기 텍스트 그림자 페이지의 삼중 스크린샷")](skew-images/skewshadowtext3-large.png#lightbox "하강 조정 기울이기 텍스트 그림자 페이지의 삼중 스크린 샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
