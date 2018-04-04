---
title: 크기 조정 변환
description: 개체를 다양 한 크기 조절용 SkiaSharp 배율 변환을 검색 합니다.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: 4c2650d4586f210b121c4c72b79e92ce72d135fe
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="the-scale-transform"></a>크기 조정 변환

_개체를 다양 한 크기 조절용 SkiaSharp 배율 변환을 검색 합니다._

에 나와 있는 것 처럼 [The 번역 변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md) 문서, 이동 변환을 한 위치에서 다른에 그래픽 개체를 이동할 수 있습니다. 반면, 배율 변환을 그래픽 개체의 크기를 변경합니다.

![](scale-images/scaleexample.png "배율 크기에 따라 높이가 단어")

배율 변환을 역시 그래픽 좌표 더 큰 내용이 이동 하면 됩니다.

앞의 번역 요인의 영향을 설명 하는 두 개의 변환 수식이 `dx` 및 `dy`:

x' = x + dx

y' y + dy =

배율 `sx` 및 `sy` 덧셈 대신 곱셈 됩니다:

x' sx · = x

y' sy · = y

번역 요인의 기본 값은 0; 배율 인수의 기본값은 1입니다.

`SKCanvas` 클래스 정의 4 `Scale` 메서드. 첫 번째 [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/) 메서드는 팩터링 하는 경우 동일한 가로 및 세로 크기 조정을 사용할 때의:

```csharp
public void Scale (Single s)
```

로 알려져 *등방성* 배율 &mdash; 배율 즉 동일한 방향으로 모두 합니다. 개체의 가로 세로 비율 유지 등방성 크기를 조정 합니다.

두 번째 [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) 메서드를 사용 하면 가로 및 세로 크기 조정에 대 한 서로 다른 값을 지정할 수 있습니다.

```csharp
public void Scale (Single sx, Single sy)
```

이 인해 *이방성* 크기 조정 합니다.
세 번째 [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/SkiaSharp.SKPoint/) 메서드 결합 단일에서 두 배율 `SKPoint` 값:

```csharp
public void Scale (SKPoint size)
```

네 번째 `Scale` 메서드를 간략하게 설명 합니다.

**기본 배율** 페이지에서는 `Scale` 메서드. [ **BasicScalePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) XAML 파일에는 두 개의 `Slider` 수 있는 요소는 0과 10 사이의 가로 및 세로 배율 인수를 선택 합니다. [ **BasicScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) 호출 하려면 해당 값을 사용 하는 코드 숨김 파일 `Scale` 선이 서로 파선 및 일부 텍스트를 왼쪽 상단에에서 맞게 크기 조정 된 모퉁이가 둥근된 사각형을 표시 하기 전에 캔버스의 모서리:

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

궁금할 수 있습니다: 배율 인수 미치는 영향은 무엇입니까에서 반환 된 값은 `MeasureText` 방식의 `SKPaint`? 대답: 전혀 나타나지 않도록 합니다. `Scale` 방법이 `SKCanvas`합니다. 와 아무것도 변경 되지 않습니다는 `SKPaint` 해당 개체를 사용 하 여 캔버스에 있는 항목을 렌더링 될 때까지 개체입니다.

볼 수 있듯이, 그리는 모든 항목이 `Scale` 비례적으로 증가 호출 합니다.

[![](scale-images/basicscale-small.png "기본 크기 조정 페이지의 삼중 스크린 샷")](scale-images/basicscale-large.png#lightbox "기본 크기 조정 페이지의 삼중 스크린 샷")

텍스트를 파선 모퉁이가 및 캔버스의 왼쪽 및 위쪽 가장자리와 모퉁이가 둥근된 사각형 사이의 10 픽셀 여백을 반올림 해당 줄에는 대시의 길이의 너비는 동일한 배율 인수 모두 받습니다.

> [!IMPORTANT]
> 유니버설 Windows 플랫폼 anisotropicly 배율이 조정 된 텍스트를 제대로 렌더링 하지 않습니다.

원인을 배율 이방성 줄에 대 한 다른 되도록 스트로크 너비 가로 및 세로 축에 맞춥니다. (이기도 함이 페이지에서 첫 번째 이미지 명확 합니다.) 배율 인수에 영향을 받을 스트로크 너비를 사용 하지 않으려면 0으로 설정 하 고에 관계 없이 너비가 1 픽셀이 항상 됩니다는 `Scale` 설정 합니다.

확장은 캔버스의 왼쪽 위 모퉁이 기준으로 합니다. 이 정확 하 게 원하는 있지만 아닐 수 있습니다. 캔버스에 텍스트와 사각형을 다른 위치를 배치 하 고 중심을 관통 개체인를 가정 합니다. 네 번째 버전을 사용할 수는 경우에 [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/System.Single/System.Single/) 크기 조정의 중심을 지정 하는 두 개의 추가 매개 변수를 포함 하는 메서드:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px` 및 `py` 매개 변수는이 라고도 하는 지점을 정의 *센터 배율* 는 SkiaSharp에 설명서 라고 하지만 *피벗 지점*합니다. 이 크기 조정에 영향을 받지 않는 캔버스의 왼쪽 위 모퉁이 기준으로 점입니다. 모든 확장에 해당 가운데를 기준으로 발생합니다.

[ **가운데 배율** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) 이 페이지에 표시 합니다. `PaintSurface` 처리기는 비슷합니다는 **기본 배율** 점을 제외 하 고 프로그램의 `margin` 텍스트를 가로로 가운데에 프로그램에에서 가장 적합 세로 모드를 의미 하는 값은 계산:

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

모퉁이가 둥근된 사각형의 왼쪽 위 모서리 위치 `margin` 캔버스의 왼쪽에서 픽셀 및 `margin` 위쪽에서 픽셀입니다. 에 대 한 마지막 두 개의 인수는 `Scale` 방법은 해당 값 및 너비와 높이의 모퉁이가 둥근된 사각형의 너비와 높이 이기도 텍스트의로 설정 됩니다. 즉, 모든 크기 조정 사각형의 가운데를 기준으로 합니다.

[![](scale-images/centeredscale-small.png "가운데에 크기 조정 페이지의 삼중 스크린 샷")](scale-images/centeredscale-large.png#lightbox "가운데에 크기 조정 페이지의 삼중 스크린 샷")

`Slider` 이 프로그램의 요소는 범위는 &ndash;10 ~ 10입니다. 볼 수 있듯이 세로 크기 조정 (예:는 Android에서 가운데에 화면)의 음수 값을 선택 하면 크기 조정의 중심을 통과 하는 가로 축을 대칭 개체가 합니다. 가로 크기 조정 (예: Windows 화면 오른쪽)의 음수 값을 선택 하면 개체가 크기 조정의 중심을 통과 하는 세로 축을 대칭 이동 합니다.

이 네 번째 버전의는 `Scale` 메서드는 실제로 바로 가기입니다. 대체 하 여이 과정을 확인 해야 할 수도 있습니다는 `Scale` 다음이 코드에서 메서드:

```csharp
canvas.Translate(-px, -py);
```

피벗 점 좌표의 부정입니다.

이제 프로그램을 다시 실행합니다. 가운데 캔버스의 왼쪽 위 모서리는 사각형 및 텍스트 이동 되며 볼 수 있습니다. 해당 거의 볼 수 있습니다. 슬라이더 물론 지금 프로그램에 적절 하지 않습니다 때문에 작동 하지 않습니다.

이제 기본 추가 `Scale` (없음, 크기 조정 센터)를 호출 *전에* 하 `Translate` 호출:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

익숙한 그래픽 프로그래밍 시스템 이라고 생각할 수도 있습니다는 잘못 된 다른 하지만에서이 실행 되지 않습니다. Skia 연속 변환 호출 약간 다르게 처리에서 어떤 수 잘 알고 있어야 합니다.

에서는 연속 된 `Scale` 및 `Translate` 왼쪽 위 모서리에 여전히 호출, 모서리가 둥근된 직사각형의 중심은 있지만 이제 모퉁이가 둥근된 사각형의 중심 이기도 캔버스의 왼쪽 위 모퉁이 기준으로 확장할 수 있습니다.

그 전에 이제 `Scale` 다른 호출 추가 `Translate` 가운데 맞춤 값으로 호출 합니다.

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

이 크기 조정 된 결과 다시를 원래 위치로 이동 합니다. 이러한 세 번 호출 하는 것과 동일합니다.

```csharp
canvas.Scale(sx, sy, px, py);
```

총 변환 공식은 되도록 개별 변환은 중요 합니다.

 x' sx · = (x-px) + px

 y' sy · = (y-py) + py

기본 값 `sx` 및 `sy` 은 1입니다. 피벗 점 (px, py) 이러한 수식으로 변환 하지 않습니다 직접 유도 하는 것이 쉽습니다. 캔버스를 기준으로 동일한 위치에 그대로 유지 됩니다.

결합 하면 `Translate` 및 `Scale` 호출, 순서가 중요 합니다. 경우는 `Translate` 뒤에 오는 `Scale`, 번역 요소 크기 조정 요인에 의해 효과적으로 조정 됩니다. 경우는 `Translate` 앞에 오는 `Scale`, 번역 요소는 크기가 조정 되지 않습니다. 이 프로세스 다소 명백해 집니다 (하지만 더 수학) 변환 행렬의 주제는 도입 되었습니다.

`SKPath` 클래스 정의 읽기 전용 [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) 속성을 반환 하는 `SKRect` 경로에 좌표가 범위를 정의 합니다. 예를 들어는 `Bounds` 앞에서 만든 hendecagram 경로에서 속성을 가져오는 `Left` 및 `Top` 사각형의 속성은 약-100는 `Right` 및 `Bottom` 속성은 약 100 및 `Width` 및 `Height` 속성은 약 200 개. (대부분의 실제 값은 거의 없는 별표 포인트 100의 반지름 원과 정의 되어 있지만 상위 지점에만 가로 또는 세로 축이 있는 병렬 때문에.)

이 정보의 가용성 눈금 파생 해 서 캔버스의 크기에 대 한 경로 크기 조정에 대 한 적합 한 요소를 변환 해야 것을 의미 합니다. [ **이방성 배율** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) 페이지는 11 점이 별이 보여 줍니다. *이방성* 배율 것 하지 않음을 가로 및 세로 방향 즉, 별은 원래 가로 세로 비율을 유지 되지 않습니다. 여기는 관련 코드는 `PaintSurface` 처리기:

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

`pathBounds` 위쪽이 코드의 사각형은 구하여 너비와 높이 캔버스 나중에 사용 된 `Scale` 호출 합니다. 렌더링 될 때 호출 자체로 경로의 좌표 크기 조정 됩니다는 `DrawPath` 호출 했지만 별 캔버스의 오른쪽 위 모서리에서 가운데에 맞춰야 합니다. 아래쪽 및 왼쪽으로 이동 해야 합니다. 이 작업의는 `Translate` 호출 합니다. 이러한 두 속성의 `pathBounds` 약-100 되므로 변환 요소는 약 100입니다. 때문에 `Translate` 후 호출 되는 `Scale` 호출, 캔버스의 가운데에는 별모양의 중심 이동 하도록 배율 요소에 의해 효과적으로 해당 값이 확장:

[![](scale-images/anisotropicscaling-small.png "이방성 크기 조정 페이지의 삼중 스크린샷")](scale-images/anisotropicscaling-large.png#lightbox "이방성 크기 조정 페이지의 삼중 스크린 샷")

또 다른 방법은 생각해볼 수 있겠습니다는 `Scale` 및 `Translate` 역방향 시퀀스의 효과 확인 하는 호출:는 `Translate` 캔버스의 왼쪽 위 모서리에서 지향 하지만 완벽 하 게 표시 되는 것이 호출 경로 이동 합니다. `Scale` 메서드 설정 하면 해당 별 큰 왼쪽 위 모퉁이 기준으로 합니다.

실제로 별은 캔버스 보다 약간 큰로 표시 합니다. 문제는 스트로크 너비입니다. `Bounds` 속성의 `SKPath` 좌표의 크기는 경로에 인코딩되고 크기를 조정 프로그램은 사용 된 나타냅니다. 경로 특정 선 너비가 렌더링 되 면 렌더링 되는 경로 캔버스 보다 큽니다.

이 문제를 해결 하려면 해당 보정 해야 합니다. 이 프로그램에는 한 가지 쉬운 방법은 다음 문 바로 전 까지의 추가 하는 것은 `Scale` 호출:

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

이렇게 하면 증가 `pathBounds` 네 면에서 모두 1.5 단위에서 사각형입니다. 선 조인을 반올림 하는 경우에 이것이 적절 한 솔루션입니다. 이 음 더 길 수 및 계산 하기가 어렵습니다.

으로 텍스트와 비슷한 기법을 사용할 수도 있습니다는 **이방성 텍스트** 페이지를 보여 줍니다. 다음의 관련 부분을은 `PaintSurface` 에서 처리기는 [ `AnisotropicTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) 클래스:

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

도 비슷한 논리가 이며에서 반환 된 텍스트 경계 사각형에 따라 페이지의 크기에 텍스트가 확장 `MeasureText` (실제 텍스트 보다 약간 큰는 있는):

[![](scale-images/anisotropictext-small.png "3 중 이방성 테스트 페이지 스크린샷")](scale-images/anisotropictext-large.png#lightbox "3 중 이방성 테스트 페이지 스크린샷")

그래픽 개체의 가로 세로 비율을 유지 해야 할 경우 등방성 크기 조정을 사용 합니다. **등방성 배율** 페이지 11 점이 개인 별에 대 한이 보여 줍니다. 개념적으로, 그래픽 개체 등방성 크기 조정을 사용 하 여 페이지의 가운데에 표시 하기 위한 단계는.

- 그래픽 개체를 왼쪽 위 모퉁이의 중심을 변환 합니다.
- 그래픽 개체 크기를 나눈 가로 및 세로 페이지 치수의 최소에 따라 개체의 크기를 조정 합니다.
- 페이지의 가운데에 크기 조정 된 개체의 중심을 변환 합니다.

[ `IsotropicScalingPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) 반대 순서로 별을 표시 하기 전에 다음이 단계를 수행 합니다.

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

코드도 표시 별 10 번 이상, 10%-점진적으로 빨강에서 파랑 색을 변경 하 여 크기 조정의 감소 될 때마다:

[![](scale-images/isotropicscaling-small.png "등방성 크기 조정 페이지의 삼중 스크린샷")](scale-images/isotropicscaling-large.png#lightbox "등방성 크기 조정 페이지의 삼중 스크린 샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
