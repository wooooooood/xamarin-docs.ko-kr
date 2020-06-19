---
title: 크기 조정 변환
description: 이 문서에서는 개체를 다양 한 크기로 크기 조정 하기 위한 SkiaSharp 크기 조정 변환을 알아보고 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bdf33f499bf43d99436cef815c03d35b27866b80
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140181"
---
# <a name="the-scale-transform"></a>크기 조정 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_다양 한 크기로 개체 크기를 조정 하기 위한 SkiaSharp 배율 변환 검색_

[**변환 변환 문서에서**](translate.md) 볼 수 있듯이 변환 변환은 그래픽 개체를 한 위치에서 다른 위치로 이동할 수 있습니다. 반면, 배율 변환은 그래픽 개체의 크기를 변경 합니다.

![](scale-images/scaleexample.png "A tall word scaled in size")

크기 조정 변환은 또한 그래픽 좌표가 크게 이동 하는 경우가 많습니다.

이전에는 및의 변환 요소가 미치는 영향을 설명 하는 두 가지 변환 수식을 살펴보았습니다 `dx` `dy` .

x ' = x + dx

y ' = y + dy

및의 배율 `sx` 인수 `sy` 는 덧셈이 아니라 곱하기입니다.

x ' = sx · .x

y ' = sy · x.y

변환 요소의 기본값은 0입니다. 배율 인수의 기본값은 1입니다.

`SKCanvas`클래스는 네 가지 `Scale` 메서드를 정의 합니다. 첫 번째 [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single)) 방법은 같은 가로 및 세로 배율 인수를 원하는 경우입니다.

```csharp
public void Scale (Single s)
```

이를 *isotropic* &mdash; 양방향에서 동일 하 게 하는 isotropic 배율 조정 이라고 합니다. Isotropic 크기 조정은 개체의 가로 세로 비율을 유지 합니다.

두 번째 [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) 방법을 사용 하 여 가로 및 세로 크기 조정에 대해 다른 값을 지정할 수 있습니다.

```csharp
public void Scale (Single sx, Single sy)
```

이로 인해 *이방성* 크기가 조정 됩니다.
세 번째 [`Scale`](xref:SkiaSharp.SKCanvas.Scale(SkiaSharp.SKPoint)) 메서드는 두 개의 배율 인수를 단일 값으로 결합 합니다 `SKPoint` .

```csharp
public void Scale (SKPoint size)
```

네 번째 `Scale` 방법은 잠시 설명 됩니다.

**기본 크기 조정** 페이지에서는 메서드를 보여 줍니다 `Scale` . [**BasicScalePage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) 파일에 `Slider` 는 0과 10 사이의 가로 및 세로 배율 인수를 선택할 수 있는 두 개의 요소가 포함 되어 있습니다. [**BasicScalePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) 코드를 사용 하는 경우 이러한 값을 사용 하 여 `Scale` 파선으로 그린 모퉁이가 둥근 사각형을 표시 하 고 크기를 조정 하 여 캔버스의 왼쪽 위 모퉁이에 텍스트를 맞춥니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] {  7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        canvas.Scale((float)xScaleSlider.Value,
                     (float)yScaleSlider.Value);

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);

        float margin = 10;
        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

크기 조정 요소가의 메서드에서 반환 된 값에 어떻게 영향을 주는지 궁금할 수 있습니다. `MeasureText` `SKPaint` 대답은 그렇지 않습니다. `Scale`는의 메서드입니다 `SKCanvas` . 개체를 사용 하 여 `SKPaint` 캔버스에서 항목을 렌더링할 때까지 개체를 사용 하는 모든 작업에는 영향을 주지 않습니다.

여기에서 볼 수 있듯이, 호출 후에 그려진 모든 항목은 `Scale` 비례적으로 늘어납니다.

[![](scale-images/basicscale-small.png "Triple screenshot of the Basic Scale page")](scale-images/basicscale-large.png#lightbox "Triple screenshot of the Basic Scale page")

텍스트, 파선의 너비, 해당 선의 대시 길이, 모퉁이의 둥근 모양, 그리고 캔버스의 왼쪽과 위쪽 가장자리 사이의 10 픽셀 여백 및 모퉁이가 둥근 사각형은 모두 동일한 크기 조정 요소에 적용 됩니다.

> [!IMPORTANT]
> 유니버설 Windows 플랫폼 anisotropicly 배율 텍스트를 올바르게 렌더링 하지 않습니다.

이방성 크기 조정을 사용 하면 가로 축과 세로 축에 맞춰진 선에서 획 너비가 달라 집니다. 이 페이지의 첫 번째 이미지 에서도 확인할 수 있습니다. 스트로크 너비를 배율 인수에 영향을 주지 않으려면 0으로 설정 하 고 설정에 관계 없이 항상 1 픽셀 너비를 설정 `Scale` 합니다.

크기 조정은 캔버스의 왼쪽 위 모퉁이를 기준으로 합니다. 이는 사용자가 원하는 것과 정확 하 게 일치 하지 않을 수 있습니다. 텍스트와 사각형을 캔버스의 다른 위치에 배치 하 고 중심을 기준으로 크기를 조정 하려는 경우를 가정 합니다. 이 경우 [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) 두 개의 추가 매개 변수를 포함 하 여 크기 조정 중심을 지정 하는 메서드의 네 번째 버전을 사용할 수 있습니다.

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px`및 `py` 매개 변수는 때때로 *크기 조정 중심* 이 라고도 하는 점을 정의 하지만 SkiaSharp 설명서에서 *피벗 점*이라고 합니다. 크기 조정의 영향을 받지 않는 캔버스의 왼쪽 위 모퉁이를 기준으로 하는 점입니다. 모든 크기 조정은 해당 중심을 기준으로 수행 됩니다.

[**중앙 크기 조정**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) 페이지에는이 기능이 어떻게 작동 하는지 표시 됩니다. `PaintSurface`처리기는 **기본 크기 조정** 프로그램과 유사 합니다 `margin` . 단, 텍스트를 가로로 가운데 맞춤 하기 위해 값을 계산 하는 것을 의미 합니다. 즉, 프로그램이 세로 모드에서 가장 잘 작동 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);
        float margin = (info.Width - textBounds.Width) / 2;

        float sx = (float)xScaleSlider.Value;
        float sy = (float)yScaleSlider.Value;
        float px = margin + textBounds.Width / 2;
        float py = margin + textBounds.Height / 2;

        canvas.Scale(sx, sy, px, py);

        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

모퉁이가 둥근 사각형의 왼쪽 위 모퉁이는 `margin` 캔버스의 왼쪽부터 픽셀 까지의 픽셀 위치에 배치 됩니다 `margin` . 메서드에 대 한 마지막 두 인수는 `Scale` 텍스트의 너비와 높이를 더한 값으로 설정 되며,이는 모퉁이가 둥근 사각형의 너비와 높이 이기도 합니다. 즉, 모든 크기 조정은 해당 사각형의 중심을 기준으로 합니다.

[![](scale-images/centeredscale-small.png "Triple screenshot of the Centered Scale page")](scale-images/centeredscale-large.png#lightbox "Triple screenshot of the Centered Scale page")

`Slider`이 프로그램의 요소는 &ndash; 10에서 10 사이입니다. 여기에서 볼 수 있듯이 세로 크기 조정의 음수 값 (예: 가운데 Android 화면)은 개체가 크기 조정 중심을 통과 하는 가로 축을 중심으로 대칭 이동 되도록 합니다. 오른쪽의 UWP 화면에서와 같이 가로 배율 값이 음수 이면 개체는 크기 조정 중심을 통과 하는 세로 축을 중심으로 대칭 이동 합니다.

[`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single))피벗 점이 있는 메서드의 버전은 일련의 3 및 호출에 대 한 바로 가기입니다 `Translate` `Scale` . `Scale` **중앙 크기 조정** 페이지에서 메서드를 다음으로 대체 하 여이 동작을 확인할 수 있습니다.

```csharp
canvas.Translate(-px, -py);
```

이는 피벗 점 좌표의 부정입니다.

이제 프로그램을 다시 실행합니다. 사각형이 캔버스의 왼쪽 위 모퉁이에 오도록 사각형과 텍스트가 이동 하는 것을 볼 수 있습니다. 이를 거의 볼 수 없습니다. 이제는 프로그램이 전혀 확장 되지 않으므로 슬라이더가 작동 하지 않습니다.

이제 `Scale` 해당 호출 *앞* 에 크기 조정 센터 없이 기본 호출을 추가 합니다 `Translate` .

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

다른 그래픽 프로그래밍 시스템에서이 연습에 대해 잘 알고 있는 경우에는 문제가 없는 것으로 생각할 수 있습니다. 기능을 사용 하는 ia는 자주 사용 하는 변환과 약간 다르게 호출을 처리 합니다.

연속 된 및 호출을 사용 하 여 `Scale` `Translate` 모퉁이가 둥근 사각형의 중심은 여전히 왼쪽 위 모퉁이에 있지만, 이제 캔버스의 왼쪽 위 모퉁이를 기준으로 크기를 조정할 수 있습니다 (모퉁이가 둥근 사각형의 중심 이기도 함).

이제이 호출 전에 `Scale` `Translate` 중심 값을 사용 하 여 다른 호출을 추가 합니다.

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

그러면 배율이 조정 된 결과가 원래 위치로 다시 이동 합니다. 이러한 세 호출은 다음과 같습니다.

```csharp
canvas.Scale(sx, sy, px, py);
```

개별 변환은 복잡해 서 전체 변형 수식이 됩니다.

 x ' = sx · (x – px) + px

 y ' = sy · (y – py) + py

및의 기본값은 `sx` `sy` 1입니다. 이러한 수식에 의해 피벗 지점 (px, py)이 변환 되지 않는 것을 쉽게 유도할 수 있습니다. 캔버스와 상대적으로 동일한 위치에 유지 됩니다.

`Translate`및 `Scale` 를 호출 하는 경우 순서가 중요 합니다. 가 `Translate` 뒤에 오는 경우 `Scale` 변환 요소는 크기 조정 요소에 의해 효과적으로 조정 됩니다. 가 `Translate` 앞에 오는 경우 `Scale` 변환 요소는 크기가 조정 되지 않습니다. 이 프로세스는 변환 매트릭스의 주체가 도입 될 때 약간 더 명확 하 게 됩니다.

`SKPath`클래스는 [`Bounds`](xref:SkiaSharp.SKPath.Bounds) `SKRect` 경로에 있는 좌표의 범위를 정의 하는을 반환 하는 읽기 전용 속성을 정의 합니다. 예를 들어 앞에서 `Bounds` 만든 hendecagram 경로에서 속성을 가져오는 경우 사각형의 및 속성은 약 100이 고, 및 속성은 약 100 이며 및 `Left` `Top` 속성은 `Right` `Bottom` `Width` `Height` 약 200입니다. (별 점이 100의 반지름을 가진 원으로 정의 되지만 위쪽 요소만 가로 또는 세로 축을 사용 하 여 병렬 됨)에는 대부분의 실제 값이 조금 줄어듭니다.

이 정보를 사용할 수 있다는 것은 캔버스 크기에 대 한 경로의 크기를 조정 하는 데 적합 한 크기 조정 및 변환 요인을 파생 시킬 수 있다는 것을 의미 합니다. [**이방성 크기 조정**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) 페이지에서는 11 방향 별을 사용 하 여이를 보여 줍니다. *이방성* 규모는 가로 및 세로 방향이 같지 않음을 의미 합니다. 즉, 별이 원래의 가로 세로 비율을 유지 하지 않습니다. 처리기의 관련 코드는 `PaintSurface` 다음과 같습니다.

```csharp
SKPath path = HendecagramPage.HendecagramPath;
SKRect pathBounds = path.Bounds;

using (SKPaint fillPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Pink
})
using (SKPaint strokePaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 3,
    StrokeJoin = SKStrokeJoin.Round
})
{
    canvas.Scale(info.Width / pathBounds.Width,
                 info.Height / pathBounds.Height);
    canvas.Translate(-pathBounds.Left, -pathBounds.Top);

    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

`pathBounds`사각형은이 코드의 위쪽 근처에서 가져온 다음 나중에 호출에서 캔버스의 너비 및 높이와 함께 사용 됩니다 `Scale` . 호출에서 렌더링 될 때 해당 호출에서 경로를 조정 `DrawPath` 하지만 별이 캔버스의 오른쪽 위 모퉁이에 가운데 맞춤 됩니다. 왼쪽 아래로 이동 해야 합니다. 호출 작업입니다 `Translate` . 의 두 속성 `pathBounds` 은 약-100 이므로 변환 요소는 약 100입니다. 호출 후에 호출을 수행 하기 때문에 `Translate` `Scale` 이러한 값은 크기 조정 요소에 의해 효과적으로 조정 되므로 별의 중심을 캔버스의 중심으로 이동 합니다.

[![](scale-images/anisotropicscaling-small.png "Triple screenshot of the Anisotropic Scaling page")](scale-images/anisotropicscaling-large.png#lightbox "Triple screenshot of the Anisotropic Scaling page")

및 호출에 대해 고려할 수 있는 다른 방법은 `Scale` `Translate` 역방향 시퀀스의 효과를 결정 하는 것입니다. `Translate` 호출은 완전히 표시 되지만 캔버스의 왼쪽 위 모퉁이에 표시 되는 경로를 이동 합니다. `Scale`그런 다음 메서드는 왼쪽 위 모퉁이를 기준으로 더 큰 별을 만듭니다.

실제로 별이 캔버스 보다 약간 더 크다는 것을 보입니다. 이 문제는 스트로크 너비입니다. `Bounds`의 속성은 `SKPath` 경로에서 인코딩된 좌표의 크기를 나타내며 프로그램에서 크기를 조정 하는 데 사용 하는 것입니다. 특정 스트로크 너비로 경로를 렌더링 하면 렌더링 된 경로가 캔버스 보다 커집니다.

이 문제를 해결 하려면 해당 문제를 보완 해야 합니다. 이 프로그램의 한 가지 쉬운 방법은 호출 바로 앞에 다음 문을 추가 하는 것입니다 `Scale` .

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

그러면 `pathBounds` 4 면 모두 1.5 단위로 사각형이 늘어납니다. 이는 스트로크 조인이 반올림 된 경우에만 적절 한 솔루션입니다. 마이터 조인은 더 길고 계산 하기 어려울 수 있습니다.

또한 **이방성 텍스트** 페이지에서 보여 주는 것 처럼 텍스트와 유사한 기술을 사용할 수 있습니다. `PaintSurface`클래스에서 처리기의 관련 부분은 [`AnisotropicTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) 다음과 같습니다.

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 0.1f,
    StrokeJoin = SKStrokeJoin.Round
})
{
    SKRect textBounds = new SKRect();
    textPaint.MeasureText("HELLO", ref textBounds);

    // Inflate bounds by the stroke width
    textBounds.Inflate(textPaint.StrokeWidth / 2,
                       textPaint.StrokeWidth / 2);

    canvas.Scale(info.Width / textBounds.Width,
                 info.Height / textBounds.Height);
    canvas.Translate(-textBounds.Left, -textBounds.Top);

    canvas.DrawText("HELLO", 0, 0, textPaint);
}
```

비슷한 논리 이며 텍스트는에서 반환 된 텍스트 범위 사각형 `MeasureText` (실제 텍스트 보다 조금 큼)에 따라 페이지 크기로 확장 됩니다.

[![](scale-images/anisotropictext-small.png "Triple screenshot of the Anisotropic Test page")](scale-images/anisotropictext-large.png#lightbox "Triple screenshot of the Anisotropic Test page")

그래픽 개체의 가로 세로 비율을 유지 해야 하는 경우에는 isotropic 크기 조정을 사용 하는 것이 좋습니다. **Isotropic 크기 조정** 페이지에서는 11 방향 별에 대해이를 보여 줍니다. 개념적으로 isotropic 크기 조정으로 페이지의 가운데에 그래픽 개체를 표시 하는 단계는 다음과 같습니다.

- 그래픽 개체의 중심을 왼쪽 위 모서리로 변환 합니다.
- 가로 및 세로 페이지의 최소 크기를 그래픽 개체 차원으로 나눈 값을 기준으로 개체의 크기를 조정 합니다.
- 크기 조정 된 개체의 중심을 페이지의 가운데로 변환 합니다.

는 [`IsotropicScalingPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) 별표를 표시 하기 전에 역순으로 이러한 단계를 수행 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPath path = HendecagramArrayPage.HendecagramPath;
    SKRect pathBounds = path.Bounds;

    using (SKPaint fillPaint = new SKPaint())
    {
        fillPaint.Style = SKPaintStyle.Fill;

        float scale = Math.Min(info.Width / pathBounds.Width,
                               info.Height / pathBounds.Height);

        for (int i = 0; i <= 10; i++)
        {
            fillPaint.Color = new SKColor((byte)(255 * (10 - i) / 10),
                                          0,
                                          (byte)(255 * i / 10));
            canvas.Save();
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(scale);
            canvas.Translate(-pathBounds.MidX, -pathBounds.MidY);
            canvas.DrawPath(path, fillPaint);
            canvas.Restore();

            scale *= 0.9f;
        }
    }
}
```

또한이 코드는 각각 배율 인수를 10% 감소 하 고 색을 빨강에서 파랑으로 변경 하는 시간을 10 번 더 표시 합니다.

[![](scale-images/isotropicscaling-small.png "Triple screenshot of the Isotropic Scaling page")](scale-images/isotropicscaling-large.png#lightbox "Triple screenshot of the Isotropic Scaling page")

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
