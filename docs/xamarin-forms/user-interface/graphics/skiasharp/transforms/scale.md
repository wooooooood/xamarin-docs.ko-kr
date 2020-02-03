---
title: 크기 조정 변환
description: Thhis 문서 탐색 SkiaSharp 배율 변환 개체, 다양 한 규모를 확장 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
ms.openlocfilehash: 7dc7a2faabef86aa63b52739edda696efcb54aba
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724248"
---
# <a name="the-scale-transform"></a>크기 조정 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_다양 한 크기로 개체 크기를 조정 하기 위한 SkiaSharp 배율 변환 검색_

[**변환 변환 문서에서**](translate.md) 볼 수 있듯이 변환 변환은 그래픽 개체를 한 위치에서 다른 위치로 이동할 수 있습니다. 이와 대조적으로 배율 변환 그래픽 개체의 크기를 변경 합니다.

![](scale-images/scaleexample.png "A tall word scaled in size")

배율 변환에는 종종 더 큰 내용이 이동할 그래픽 좌표 하면 됩니다.

이전에는 `dx` 및 `dy`의 변환 요소가 미치는 영향을 설명 하는 두 가지 변환 수식을 살펴보았습니다.

x' = x + dx

y' = y + dy

`sx` 및 `sy`의 크기 조정 요인은 덧셈이 아닌 곱하기입니다.

x' = sx? x

y' = sy? y

Translate 요인의 기본값은 0; 배율 인수는 기본 값은 1입니다.

`SKCanvas` 클래스는 네 개의 `Scale` 메서드를 정의 합니다. 첫 번째 [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single)) 메서드는 동일한 수평 및 수직 배율 인수를 원하는 경우에 대 한 것입니다.

```csharp
public void Scale (Single s)
```

이를 양방향 *isotropic* 크기 조정 &mdash; 크기 조정 이라고 합니다. 개체의 가로 세로 비율 유지 등방성 크기 조정 합니다.

두 번째 [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) 메서드를 사용 하 여 가로 및 세로 크기 조정에 대해 다른 값을 지정할 수 있습니다.

```csharp
public void Scale (Single sx, Single sy)
```

이로 인해 *이방성* 크기가 조정 됩니다.
세 번째 [`Scale`](xref:SkiaSharp.SKCanvas.Scale(SkiaSharp.SKPoint)) 메서드는 두 개의 배율 인수를 단일 `SKPoint` 값으로 결합 합니다.

```csharp
public void Scale (SKPoint size)
```

네 번째 `Scale` 방법은 잠시 설명 됩니다.

**기본 크기 조정** 페이지에서는 `Scale` 방법을 보여 줍니다. [**BasicScalePage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) 파일에는 0과 10 사이의 가로 및 세로 배율 인수를 선택할 수 있는 두 개의 `Slider` 요소가 포함 되어 있습니다. [**BasicScalePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) 코드에서 해당 값 `Scale`을 사용 하 여 파선으로 그린 모퉁이가 둥근 사각형을 표시 하 고 크기를 조정 하 여 캔버스의 왼쪽 위 모퉁이에 텍스트를 맞춥니다.

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

크기 조정 요인이 `SKPaint`의 `MeasureText` 메서드에서 반환 된 값에 어떻게 영향을 주는지 궁금할 수 있습니다. 대답: 전혀 그렇지 않습니다. `Scale`은 `SKCanvas`메서드입니다. 개체를 사용 하 여 캔버스에서 항목을 렌더링할 때까지 `SKPaint` 개체에서 수행 하는 작업에는 영향을 주지 않습니다.

여기에서 볼 수 있듯이 `Scale` 호출 후에 그려진 모든 항목은 비례적으로 늘어납니다.

[![](scale-images/basicscale-small.png "Triple screenshot of the Basic Scale page")](scale-images/basicscale-large.png#lightbox "Triple screenshot of the Basic Scale page")

텍스트를 모퉁이가 둥근된 사각형 캔버스의 왼쪽 및 위쪽 가장자리 사이의 여백이 10 픽셀인을 반올림 하는 줄에서 대시의 길이 파선 선의 두께 사항이 모두 동일한 배율입니다.

> [!IMPORTANT]
> 유니버설 Windows 플랫폼 anisotropicly 배율 조정 된 텍스트를 올바르게 렌더링 하지 않습니다.

확장 하면 이방성 줄 마다 되려면 스트로크 너비 가로 및 세로 축에 맞춥니다. 이 페이지의 첫 번째 이미지 에서도 확인할 수 있습니다. 스트로크 너비를 배율 인수에 영향을 주지 않도록 하려면 0으로 설정 하 고, `Scale` 설정에 관계 없이 항상 1 픽셀 너비를 설정 합니다.

조정은 캔버스의 왼쪽 위 모퉁이 기준으로 합니다. 이 수 정확 하 게 원하는 있지만 되지 않을 수 있습니다. 텍스트와 사각형을 다른 곳을 캔버스에 배치 하 고 중심을 기준으로 크기를 조정 하려는 경우 이 경우 두 개의 추가 매개 변수를 포함 하 여 크기 조정 중심을 지정 하는 [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) 메서드의 네 번째 버전을 사용할 수 있습니다.

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px` 및 `py` 매개 변수는 *크기 조정 중심* 이 라고도 하는 점을 정의 하지만 SkiaSharp 설명서에서 *피벗 점*이라고 합니다. 이 확장 하 여 영향을 받지 않는 캔버스의 왼쪽 위 모퉁이 기준으로 지점입니다. 모든 확장에 가운데를 기준으로 발생합니다.

[**중앙 크기 조정**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) 페이지에는이 기능이 어떻게 작동 하는지 표시 됩니다. `PaintSurface` 처리기는 텍스트를 가로로 가운데 맞춤 하기 위해 `margin` 값이 계산 된다는 점을 제외 하 고 **기본 크기 조정** 프로그램과 유사 합니다. 즉, 프로그램이 세로 모드에서 가장 잘 작동 한다는 것을 의미 합니다.

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

모퉁이가 둥근 사각형의 왼쪽 위 모퉁이는 캔버스의 왼쪽부터 픽셀 `margin` 배치 되며 위쪽의 픽셀을 `margin` 합니다. `Scale` 메서드에 대 한 마지막 두 개의 인수는 텍스트의 너비와 높이를 더한 값으로 설정 되며,이는 모퉁이가 둥근 사각형의 너비와 높이 이기도 합니다. 즉, 모든 크기 조정 해당 영역의 가운데를 기준으로 합니다.

[![](scale-images/centeredscale-small.png "Triple screenshot of the Centered Scale page")](scale-images/centeredscale-large.png#lightbox "Triple screenshot of the Centered Scale page")

이 프로그램의 `Slider` 요소 범위는 10에서 10 &ndash;사이입니다. 알 수 있듯이 수직적 크기 조정 (예: android 화면 가운데에서)의 음수 값 개체 크기 조정의 중심을 통과 하는 가로 축을 대칭으로 배열할지 발생 합니다. 수평적 크기 조정은 (UWP 화면 오른쪽의 예)의 음수 값을 선택 하면 크기 조정의 중심을 통과 하는 세로 축을 대칭으로 배열할지 개체가 합니다.

피벗 점이 있는 [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) 메서드의 버전은 세 가지 `Translate` 및 `Scale` 호출에 대 한 바로 가기입니다. 이 작업을 수행 하는 방법은 **가운데 맞춤** 페이지의 `Scale` 메서드를 다음으로 대체 하 여 확인 하는 것이 좋습니다.

```csharp
canvas.Translate(-px, -py);
```

이들은 피벗 점 좌표의 부정입니다.

이제 프로그램을 다시 실행합니다. 캔버스의 왼쪽 위 모서리에 있는 가운데 되도록 사각형 및 텍스트 이동 되며 볼 수 있습니다. 이 단계에 대해 간단히 확인할 수 있습니다. 이제 프로그램이 전혀 확장 되지 않습니다 때문에 슬라이더는 물론 작동 하지 않습니다.

이제 `Translate`를 호출 *하기 전에* 기본 `Scale` 호출 (크기 조정 센터 제외)을 추가 합니다.

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

익숙한 경우이 연습에서 그래픽 시스템 프로그래밍에서 생각 하는 무엇이 다른 하지만 하지 않습니다. Skia 연속 변환 호출 약간 다르게 처리에서 어떤 익숙하게 사용 하 여 합니다.

연속 `Scale` 및 `Translate` 호출을 사용 하 여 모퉁이가 둥근 사각형의 중심은 여전히 왼쪽 위 모퉁이에 있지만, 이제 캔버스의 왼쪽 위 모퉁이를 기준으로 크기를 조정할 수 있습니다 .이는 모퉁이가 둥근 사각형의 가운데 이기도 합니다.

이제 `Scale`를 호출 하기 전에 중심 값을 사용 하 여 다른 `Translate` 호출을 추가 합니다.

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

이 확장된 하면 다시를 원래 위치로 이동합니다. 이러한 세 번 호출 하는 것과 동일합니다.

```csharp
canvas.Scale(sx, sy, px, py);
```

개별 변환 총 변환 수식은 되도록 중요:

 x' = sx? (x-px) + px

 y' = sy? (y-py) + py

`sx` 및 `sy`의 기본값은 1입니다. 피벗 점 (px, py) 이러한 수식으로 변환 하지 않습니다 직접 확인 하기 쉽습니다. 캔버스를 기준으로 동일한 위치에 유지 됩니다.

`Translate` 및 `Scale` 호출을 결합 하는 경우 순서가 중요 합니다. `Translate` `Scale`뒤에 오는 경우 변환 요소는 크기 조정 요인에 의해 효과적으로 조정 됩니다. `Translate` `Scale`앞에 오는 경우에는 변환 요인이 조정 되지 않습니다. 이 프로세스 다소 분명해 집니다 (비록 더 수학) 경우 변환 행렬의 주제는 도입 되었습니다.

`SKPath` 클래스는 경로에서 좌표의 범위를 정의 하는 `SKRect`을 반환 하는 읽기 전용 [`Bounds`](xref:SkiaSharp.SKPath.Bounds) 속성을 정의 합니다. 예를 들어 앞에서 만든 hendecagram 경로에서 `Bounds` 속성을 가져오는 경우 사각형의 `Left` 및 `Top` 속성은 약 100이 고 `Right` 및 `Bottom` 속성은 약 100 이며 `Width` 및 `Height` 속성은 약 200입니다. (대부분의 실제 값은 거의 없는 별표 지점의 반지름은 100 원으로 정의 되어 있지만 상위 지점에만 가로 또는 세로 축이 있는 병렬 때문에.)

이 정보의 가용성 수 확장 파생 캔버스의 크기에 대 한 경로 크기 조정에 대 한 적절 한 요소를 변환 해야 것을 의미 합니다. [**이방성 크기 조정**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) 페이지에서는 11 방향 별을 사용 하 여이를 보여 줍니다. *이방성* 규모는 가로 및 세로 방향이 같지 않음을 의미 합니다. 즉, 별이 원래의 가로 세로 비율을 유지 하지 않습니다. `PaintSurface` 처리기의 관련 코드는 다음과 같습니다.

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

`pathBounds` 사각형은이 코드의 맨 위 근처에서 가져온 다음 나중에 `Scale` 호출에서 캔버스의 너비와 높이와 함께 사용 됩니다. 자체를 호출 하면 `DrawPath` 호출에서 렌더링 될 때 경로 좌표가 조정 되지만 별이 캔버스의 오른쪽 위 모퉁이에 가운데 맞춤 됩니다. 아래로 및 왼쪽으로 이동 해야 합니다. `Translate` 호출의 작업입니다. `pathBounds`의 이러한 두 속성은 약 100 이므로 변환 요인은 100입니다. `Translate` 호출은 `Scale` 호출 후에 수행 되기 때문에 이러한 값은 크기 조정 요소에 의해 효과적으로 조정 되므로 별의 중심을 캔버스의 중심으로 이동 합니다.

[![](scale-images/anisotropicscaling-small.png "Triple screenshot of the Anisotropic Scaling page")](scale-images/anisotropicscaling-large.png#lightbox "Triple screenshot of the Anisotropic Scaling page")

`Scale` 및 `Translate` 호출에 대해 고려할 수 있는 다른 방법은 역방향 시퀀스의 효과를 결정 하는 것입니다. `Translate` 호출은 완전히 표시 되지만 캔버스의 왼쪽 위 모퉁이에 표시 되도록 경로를 이동 합니다. 그런 다음 `Scale` 메서드는 왼쪽 위 모퉁이를 기준으로 더 큰 별을 만듭니다.

사실 별 캔버스 보다 약간 더 큰 인지 표시 됩니다. 문제는 스트로크 너비입니다. `SKPath`의 `Bounds` 속성은 경로에서 인코딩된 좌표의 크기를 나타내며 프로그램에서 크기를 조정 하는 데 사용 합니다. 특정 스트로크 너비를 사용 하 여 경로 렌더링할 때 렌더링 되는 경로 캔버스 보다 큽니다.

이 문제를 해결 하는 보완 해야 합니다. 이 프로그램의 한 가지 간단한 방법은 `Scale` 호출 바로 앞에 다음 문을 추가 하는 것입니다.

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

그러면 4 면에서 1.5 단위로 `pathBounds` 사각형이 늘어납니다. 선 조인 반올림 하는 경우에 적절 한 솔루션입니다. 마이터 조인을 길어질 수 있습니다 하 고 계산 하기가 어렵습니다.

또한 **이방성 텍스트** 페이지에서 보여 주는 것 처럼 텍스트와 유사한 기술을 사용할 수 있습니다. 다음은 [`AnisotropicTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) 클래스에서 `PaintSurface` 처리기의 관련 부분입니다.

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

비슷한 논리 이며 텍스트는 `MeasureText`에서 반환 되는 텍스트 범위 사각형 (실제 텍스트 보다 조금 큼)에 따라 페이지 크기로 확장 됩니다.

[![](scale-images/anisotropictext-small.png "Triple screenshot of the Anisotropic Test page")](scale-images/anisotropictext-large.png#lightbox "Triple screenshot of the Anisotropic Test page")

그래픽 개체의 가로 세로 비율을 유지 해야 할 경우 등방성 크기 조정을 사용 하는 것이 좋습니다. **Isotropic 크기 조정** 페이지에서는 11 방향 별에 대해이를 보여 줍니다. 개념적으로 등방성 크기 조정을 사용 하 여 페이지의 가운데에 그래픽 개체를 표시 하기 위한 단계를 다음과 같습니다.

- 왼쪽 위 모퉁이에 그래픽 개체의 중심을 변환 합니다.
- 그래픽 개체 크기를 나눈 가로 및 세로 페이지 크기의 최소값을 기준으로 개체를 확장 합니다.
- 페이지의 가운데에 확장 된 개체의 중심을 변환 합니다.

[`IsotropicScalingPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) 는 별표를 표시 하기 전에 역순으로 이러한 단계를 수행 합니다.

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

코드 표시 별 10 번 이상, 10% 점진적으로 빨간색에서 파란색 색을 변경 하 여 요소 배율을 감소 될 때마다:

[![](scale-images/isotropicscaling-small.png "Triple screenshot of the Isotropic Scaling page")](scale-images/isotropicscaling-large.png#lightbox "Triple screenshot of the Isotropic Scaling page")

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
