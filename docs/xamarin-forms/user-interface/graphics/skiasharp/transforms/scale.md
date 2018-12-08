---
title: 크기 조정 변환
description: Thhis 문서 탐색 SkiaSharp 배율 변환 개체, 다양 한 규모를 확장 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
ms.openlocfilehash: 9bc320273df192f9daf2520f451601335731e7b0
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061354"
---
# <a name="the-scale-transform"></a>크기 조정 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_SkiaSharp 배율 변환 다양 한 크기는 개체 크기 조정에 대 한 검색_

살펴본 것 처럼 [ **The 변환 변환** ](translate.md) 문서 좌표 이동 변환 한 위치에서 다른에 그래픽 개체를 이동할 수 있습니다. 이와 대조적으로 배율 변환 그래픽 개체의 크기를 변경 합니다.

![](scale-images/scaleexample.png "긴 단어 크기 조정")

배율 변환에는 종종 더 큰 내용이 이동할 그래픽 좌표 하면 됩니다.

앞의 번역 요인의 영향을 설명 하는 두 개의 변환 수식이 `dx` 고 `dy`:

x' = x + dx

y' = y + dy

배율 `sx` 및 `sy` 가산적 대신 곱셈 됩니다.

x' = sx? x

y' = sy? y

Translate 요인의 기본값은 0; 배율 인수는 기본 값은 1입니다.

합니다 `SKCanvas` 클래스 정의 4 `Scale` 메서드. 첫 번째 [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single)) 는 동일한 가로 및 세로 크기 조정 하려는 경우 단계에 대 한 메서드는:

```csharp
public void Scale (Single s)
```

이 이라고 *등방성* 크기 조정 &mdash; 크기 조정 된 동일한 두 방향 모두에서. 개체의 가로 세로 비율 유지 등방성 크기 조정 합니다.

두 번째 [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) 메서드를 사용 하면 가로 및 세로 크기 조정에 대 한 다른 값을 지정 합니다.

```csharp
public void Scale (Single sx, Single sy)
```

이 인해 *이방성* 크기 조정 합니다.
세 번째 [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(SkiaSharp.SKPoint)) 메서드는 단일에서 두 배율 결합 `SKPoint` 값:

```csharp
public void Scale (SKPoint size)
```

네 번째 `Scale` 메서드 곧 설명 합니다.

합니다 **기본 확장** 페이지를 보여 줍니다는 `Scale` 메서드. 합니다 [ **BasicScalePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) 파일에는 두 개의 `Slider` 수 있도록 하는 요소 0과 10 사이의 가로 및 세로 배율 인수를 선택 합니다. 합니다 [ **BasicScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) 호출 하려면 해당 값을 사용 하는 코드 숨김 파일 `Scale` 스트로크 파선으로 하 고 일부 텍스트 왼쪽 위에에서 맞게 크기를 조정할 모퉁이가 둥근된 사각형을 표시 하기 전에 캔버스의 모퉁이.

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

궁금할: 크기 조정 요인을 미치는 영향은 무엇입니까에서 반환 되는 값을 `MeasureText` 메서드의 `SKPaint`? 대답: 전혀 그렇지 않습니다. `Scale` 메서드는 `SKCanvas`합니다. 사용 하 여 수행 하는 아무 것도 변경 되지 않습니다는 `SKPaint` 캔버스의 항목을 렌더링 하는 개체를 사용 하기 전에 개체입니다.

알 수 있듯이 그린 후 모든 것을 `Scale` 증가 비례적으로 호출:

[![](scale-images/basicscale-small.png "기본 확장 페이지 스크린샷 삼중")](scale-images/basicscale-large.png#lightbox "삼중 기본 확장 페이지 스크린샷")

텍스트를 모퉁이가 둥근된 사각형 캔버스의 왼쪽 및 위쪽 가장자리 사이의 여백이 10 픽셀인을 반올림 하는 줄에서 대시의 길이 파선 선의 두께 사항이 모두 동일한 배율입니다.

> [!IMPORTANT]
> 유니버설 Windows 플랫폼 anisotropicly 배율 조정 된 텍스트를 올바르게 렌더링 하지 않습니다.

확장 하면 이방성 줄 마다 되려면 스트로크 너비 가로 및 세로 축에 맞춥니다. (도이 페이지의 첫 번째 이미지에서 알 수 있습니다.) 스트로크 너비를 크기 조정 요인을 영향을 받지 않으려면 0으로 설정 하 고 항상 관계 없이 1 픽셀로 설정 됩니다는 `Scale` 설정 합니다.

조정은 캔버스의 왼쪽 위 모퉁이 기준으로 합니다. 이 수 정확 하 게 원하는 있지만 되지 않을 수 있습니다. 텍스트와 사각형을 다른 곳을 캔버스에 배치 하 고 중심을 기준으로 크기를 조정 하려는 경우 네 번째 버전을 사용할 수는 경우에 [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) 크기 조정의 중심을 지정 하려면 두 개의 추가 매개 변수를 포함 하는 메서드:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px` 및 `py` 매개 변수 라고도 하는 지점을 정의 합니다 *센터 크기 조정* 는 SkiaSharp 설명서는 라고 하지만 *피벗 점*합니다. 이 확장 하 여 영향을 받지 않는 캔버스의 왼쪽 위 모퉁이 기준으로 지점입니다. 모든 확장에 가운데를 기준으로 발생합니다.

합니다 [ **가운데 맞춤 크기 조정** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) 페이지 작동 방식을 보여 줍니다. `PaintSurface` 처리기는 비슷합니다는 **기본 확장** 점을 제외 하면 프로그램는 `margin` 텍스트를 가운데에 가로, 세로 모드에서 프로그램을 가장 잘 작동 하는지 즉 하는 값은 계산:

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

모퉁이가 둥근된 사각형의 왼쪽 위 모퉁이 배치 됩니다 `margin` 캔버스의 왼쪽에서 픽셀 및 `margin` 위에서 픽셀입니다. 마지막 두 인수는 `Scale` 메서드는 해당 값 및 너비와 높이의 모퉁이가 둥근된 사각형의 너비와 높이 텍스트의로 설정 됩니다. 즉, 모든 크기 조정 해당 영역의 가운데를 기준으로 합니다.

[![](scale-images/centeredscale-small.png "가운데 맞춤 크기 조정 페이지의 3 배가 스크린 샷")](scale-images/centeredscale-large.png#lightbox "삼중 가운데 맞춤 크기 조정 페이지 스크린샷")

합니다 `Slider` 이 프로그램의 요소 범위를 갖고 &ndash;10 ~ 10입니다. 알 수 있듯이 수직적 크기 조정 (예: android 화면 가운데에서)의 음수 값 개체 크기 조정의 중심을 통과 하는 가로 축을 대칭으로 배열할지 발생 합니다. 수평적 크기 조정은 (UWP 화면 오른쪽의 예)의 음수 값을 선택 하면 크기 조정의 중심을 통과 하는 세로 축을 대칭으로 배열할지 개체가 합니다.

버전을 [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) 피벗 지점 메서드 3 개에 대 한 바로 가기입니다 `Translate` 및 `Scale` 호출 합니다. 이 대체 하 여 작동 방식을 확인 하려는 합니다 `Scale` 에서 메서드를 **가운데 확장** 다음 페이지:

```csharp
canvas.Translate(-px, -py);
```

이들은 피벗 점 좌표의 부정입니다.

이제 프로그램을 다시 실행합니다. 캔버스의 왼쪽 위 모서리에 있는 가운데 되도록 사각형 및 텍스트 이동 되며 볼 수 있습니다. 이 단계에 대해 간단히 확인할 수 있습니다. 이제 프로그램이 전혀 확장 되지 않습니다 때문에 슬라이더는 물론 작동 하지 않습니다.

이제 기본을 추가할 `Scale` (하지 않고 크기 조정 센터)를 호출할 *하기 전에* 는 `Translate` 호출:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

익숙한 경우이 연습에서 그래픽 시스템 프로그래밍에서 생각 하는 무엇이 다른 하지만 하지 않습니다. Skia 연속 변환 호출 약간 다르게 처리에서 어떤 익숙하게 사용 하 여 합니다.

에서는 연속을 사용 하 여 `Scale` 고 `Translate` 호출 모퉁이가 둥근된 사각형의 중심 왼쪽 위 모서리에서 여전히 이지만 이제 모퉁이가 둥근된 사각형의 중심 이기도 캔버스의 왼쪽 위 모퉁이 기준으로 확장할 수 있습니다.

이제 이전에 `Scale` 호출, 다른 추가 `Translate` 가운데 맞춤 값을 사용 하 여 호출 합니다.

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

에 유의 기본값인 `sx` 및 `sy` 은 1입니다. 피벗 점 (px, py) 이러한 수식으로 변환 하지 않습니다 직접 확인 하기 쉽습니다. 캔버스를 기준으로 동일한 위치에 유지 됩니다.

결합 하면 `Translate` 고 `Scale` 호출 순서가 중요 합니다. 경우는 `Translate` 뒤에 오는 `Scale`, 번역 요소 크기 조정 요인을 기준으로 효과적으로 조정 됩니다. 경우는 `Translate` 앞에 오는 `Scale`, 번역 요소는 배율 조정 되지 않습니다. 이 프로세스 다소 분명해 집니다 (비록 더 수학) 경우 변환 행렬의 주제는 도입 되었습니다.

합니다 `SKPath` 클래스 정의 읽기 전용 [ `Bounds` ](xref:SkiaSharp.SKPath.Bounds) 반환 하는 속성을 `SKRect` 경로에 좌표가 범위를 정의 합니다. 예를 들어 경우는 `Bounds` 앞에서 만든 hendecagram 경로에서 속성을 가져오는 합니다 `Left` 및 `Top` 사각형의 속성은 약-100는 `Right` 및 `Bottom` 속성은 약 100 하며 `Width` 고 `Height` 속성은 약 200 개. (대부분의 실제 값은 거의 없는 별표 지점의 반지름은 100 원으로 정의 되어 있지만 상위 지점에만 가로 또는 세로 축이 있는 병렬 때문에.)

이 정보의 가용성 수 확장 파생 캔버스의 크기에 대 한 경로 크기 조정에 대 한 적절 한 요소를 변환 해야 것을 의미 합니다. 합니다 [ **이방성 크기 조정** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) 페이지 11 가리킨 별표를 사용 하 여이 보여 줍니다. *이방성* 확장 한 것 같은 가로 및 세로 방향으로 즉, 별 원래의 가로 세로 비율을 유지 하지는 의미입니다. 여기 관련 코드는는 `PaintSurface` 처리기:

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

`pathBounds` 이 코드의 맨 위쪽 사각형을 가져와 다음 캔버스 높이 너비를 사용 하 여 나중에 사용 된 `Scale` 호출 합니다. 렌더링 될 때 호출 자체로 경로의 좌표를 크기 조정 됩니다는 `DrawPath` 호출 하지만 별 캔버스의 오른쪽 위 모서리에 있는 가운데에 맞춰야 됩니다. 아래로 및 왼쪽으로 이동 해야 합니다. 이 작업을 `Translate` 호출 합니다. 이러한 두 속성의 `pathBounds` -약 100 되므로 번역 요인은 약 100입니다. 때문에 `Translate` 후 호출 되는 `Scale` 호출 별모양의 중심 캔버스의 가운데에 이동 하도록 크기 조정 요인을 기준으로 효과적으로 해당 값이 확장:

[![](scale-images/anisotropicscaling-small.png "3 중 이방성 크기 조정 페이지 스크린샷")](scale-images/anisotropicscaling-large.png#lightbox "3 중 이방성 크기 조정 페이지 스크린샷")

다른 방식으로 생각할 수 있습니다 합니다 `Scale` 및 `Translate` 호출 역방향 시퀀스의 효과 결정 하는 것:는 `Translate` 캔버스의 왼쪽 위 모서리에 기반 하지만 완벽 하 게 표시 되도록 호출 경로 이동 합니다. `Scale` 메서드 그러면 해당 별 큰 왼쪽 위 모퉁이 기준으로 합니다.

사실 별 캔버스 보다 약간 더 큰 인지 표시 됩니다. 문제는 스트로크 너비입니다. `Bounds` 의 속성 `SKPath` 좌표 차원의 인코딩된 경로 나타내고 크기를 조정 하는 프로그램이 사용 하는 것이입니다. 특정 스트로크 너비를 사용 하 여 경로 렌더링할 때 렌더링 되는 경로 캔버스 보다 큽니다.

이 문제를 해결 하는 보완 해야 합니다. 이 프로그램에 한 가지 쉬운 방법은 다음 문 바로 앞에 추가 하는 것은 `Scale` 호출 합니다.

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

이 인해 증가 `pathBounds` 1.5 단위 네 면 모두에서 사각형입니다. 선 조인 반올림 하는 경우에 적절 한 솔루션입니다. 마이터 조인을 길어질 수 있습니다 하 고 계산 하기가 어렵습니다.

텍스트와 비슷한 방법으로 사용할 수도 있습니다는 **이방성 텍스트** 페이지를 보여 줍니다. 여기의 관련 부분은 합니다 `PaintSurface` 에서 처리기는 [ `AnisotropicTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) 클래스:

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

유사한 논리에 이며 텍스트에서 반환 하는 사각형에 텍스트 범위에 따라 페이지 크기 확장 `MeasureText` (되 실제 텍스트를 약간 초과):

[![](scale-images/anisotropictext-small.png "3 중 이방성 테스트 페이지 스크린샷")](scale-images/anisotropictext-large.png#lightbox "3 중 이방성 테스트 페이지 스크린샷")

그래픽 개체의 가로 세로 비율을 유지 해야 할 경우 등방성 크기 조정을 사용 하는 것이 좋습니다. 합니다 **등방성 확장** 페이지 11 가리킨 스타이 보여 줍니다. 개념적으로 등방성 크기 조정을 사용 하 여 페이지의 가운데에 그래픽 개체를 표시 하기 위한 단계를 다음과 같습니다.

- 왼쪽 위 모퉁이에 그래픽 개체의 중심을 변환 합니다.
- 그래픽 개체 크기를 나눈 가로 및 세로 페이지 크기의 최소값을 기준으로 개체를 확장 합니다.
- 페이지의 가운데에 확장 된 개체의 중심을 변환 합니다.

합니다 [ `IsotropicScalingPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) 별을 표시 하기 전에 다음이 단계를 반대 순서로 수행 합니다.

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

[![](scale-images/isotropicscaling-small.png "삼중 등방성 크기 조정 페이지 스크린샷")](scale-images/isotropicscaling-large.png#lightbox "삼중 등방성 크기 조정 페이지 스크린샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
