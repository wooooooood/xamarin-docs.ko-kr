---
title: 좌표 이동 변환
description: 이 문서에서는 transform transform을 사용 하 여 응용 프로그램에서 SkiaSharp 그래픽을 이동 하는 방법을 살펴보고 Xamarin.Forms 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0eb3b4a6b37d59363984c9248cc39de91a6819e0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138257"
---
# <a name="the-translate-transform"></a>좌표 이동 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_변환 변환을 사용 하 여 SkiaSharp 그래픽을 이동 하는 방법을 알아봅니다._

SkiaSharp에서 가장 *간단한 변환 형식은 변환 또는 변환* 입니다. *translation* 이 변환은 그래픽 개체를 가로 및 세로 방향으로 이동 합니다. 즉, 일반적으로 그리기 함수에서 사용 하는 좌표를 변경 하 여 동일한 효과를 얻을 수 있기 때문에 변환이 가장 불필요 한 변환입니다. 그러나 경로를 렌더링할 때 모든 좌표가 경로에 캡슐화 되므로 변환 변환을 적용 하 여 전체 경로를 이동 하는 것이 훨씬 쉽습니다.

변환은 애니메이션 및 간단한 텍스트 효과에도 유용 합니다.

![](translate-images/translateexample.png "Text shadow, engraving, and embossing with translation")

[`Translate`](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single))의 메서드에는 `SKCanvas` 다음과 같은 두 개의 매개 변수가 있습니다 .이 매개 변수를 통해 다음에 그려진 그래픽 개체를 가로 및 세로로 이동 합니다.

```csharp
public void Translate (Single dx, Single dy)
```

이러한 인수는 음수일 수 있습니다. 두 번째 [`Translate`](xref:SkiaSharp.SKCanvas.Translate(SkiaSharp.SKPoint)) 메서드는 두 개의 변환 값을 단일 값으로 결합 합니다 `SKPoint` .

```csharp
public void Translate (SKPoint point)
```

[**SkiaSharpForms**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플 프로그램의 **누적 번역** 페이지에서는 메서드의 여러 호출이 누적 됨을 보여 줍니다 `Translate` . [`AccumulatedTranslatePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs)클래스는 동일한 사각형의 20 개 버전을 표시 합니다. 각각은 이전 사각형에서 한 번만 오프셋 하 여 대각선을 따라 확장 합니다. `PaintSurface`이벤트 처리기는 다음과 같습니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Color = SKColors.Black;
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = 3;

        int rectangleCount = 20;
        SKRect rect = new SKRect(0, 0, 250, 250);
        float xTranslate = (info.Width - rect.Width) / (rectangleCount - 1);
        float yTranslate = (info.Height - rect.Height) / (rectangleCount - 1);

        for (int i = 0; i < rectangleCount; i++)
        {
            canvas.DrawRect(rect, strokePaint);
            canvas.Translate(xTranslate, yTranslate);
        }
    }
}
```

연속 되는 사각형은 페이지 아래로 trickle.

[![](translate-images/accumulatedtranslate-small.png "Triple screenshot of the Accumulated Translate page")](translate-images/accumulatedtranslate-large.png#lightbox "Triple screenshot of the Accumulated Translate page")

누적 변환 요소가 `dx` 및이 `dy` 고 그리기 함수에서 지정 하는 점이 ( `x` ,) 인 경우 `y` 그래픽 개체는 (,) 지점에서 렌더링 됩니다 `x'` `y'` .

x ' = x + dx

y ' = y + dy

이러한 *변환을 변환 수식* 이라고 합니다. `dx`새에 대 한 및의 기본값은 `dy` `SKCanvas` 0입니다.

**텍스트 변환 효과** 페이지가 보여 주는 것 처럼 그림자 효과에 대 한 변환 변환 및 유사한 기술을 사용 하는 것이 일반적입니다. `PaintSurface`클래스에 있는 처리기의 관련 부분은 [`TranslateTextEffectsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) 다음과 같습니다.

```csharp
float textSize = 150;

using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = textSize;
    textPaint.FakeBoldText = true;

    float x = 10;
    float y = textSize;

    // Shadow
    canvas.Translate(10, 10);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("SHADOW", x, y, textPaint);
    canvas.Translate(-10, -10);
    textPaint.Color = SKColors.Pink;
    canvas.DrawText("SHADOW", x, y, textPaint);

    y += 2 * textSize;

    // Engrave
    canvas.Translate(-5, -5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("ENGRAVE", x, y, textPaint);
    canvas.ResetMatrix();
    textPaint.Color = SKColors.White;
    canvas.DrawText("ENGRAVE", x, y, textPaint);

    y += 2 * textSize;

    // Emboss
    canvas.Save();
    canvas.Translate(5, 5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("EMBOSS", x, y, textPaint);
    canvas.Restore();
    textPaint.Color = SKColors.White;
    canvas.DrawText("EMBOSS", x, y, textPaint);
}
```

세 가지 예제에서는 각각 `Translate` 및 변수가 제공 하는 위치에서 오프셋할 텍스트를 표시 하기 위해가 호출 됩니다 `x` `y` . 그러면 텍스트는 변환 효과 없이 다른 색으로 다시 표시 됩니다.

[![](translate-images/translatetexteffects-small.png "Triple screenshot of the Translate Text Effects page")](translate-images/translatetexteffects-large.png#lightbox "Triple screenshot of the Translate Text Effects page")

세 가지 예제는 각각 호출을 부정 하는 다른 방법을 보여 줍니다 `Translate` .

첫 번째 예에서는만 `Translate` 을 다시 호출 하지만 음수 값을 사용 합니다. `Translate`호출이 누적 되므로이 호출 시퀀스는 전체 번역을 기본값인 0으로 복원 합니다.

두 번째 예제에서는를 호출 [`ResetMatrix`](xref:SkiaSharp.SKCanvas.ResetMatrix) 합니다. 이렇게 하면 모든 변환이 해당 기본 상태로 돌아갑니다.

세 번째 예제에서는를 호출 하 여 개체의 상태를 저장 한 `SKCanvas` [`Save`](xref:SkiaSharp.SKCanvas.Save) 다음를 호출 하 여 상태를 복원 [`Restore`](xref:SkiaSharp.SKCanvas.Restore) 합니다. 이는 일련의 그리기 작업에 대해 변환을 조작 하는 가장 다양 한 방법입니다. 이러한 `Save` `Restore` 함수는 스택 처럼 함수를 호출 합니다 .는 `Save` 여러 번 호출할 수 있으며, `Restore` 역방향 시퀀스에서를 호출 하 여 이전 상태로 돌아갈 수 있습니다. `Save`메서드는 정수를 반환 하 고,이 정수를에 전달 하 여를 [`RestoreToCount`](xref:SkiaSharp.SKCanvas.RestoreToCount*) 효과적으로 여러 번 호출할 수 있습니다 `Restore` . [`SaveCount`](xref:SkiaSharp.SKCanvas.SaveCount)속성은 스택에 현재 저장 된 상태 수를 반환 합니다.

[`SKAutoCanvasRestore`](xref:SkiaSharp.SKAutoCanvasRestore)Canvas 상태를 복원 하는 데 클래스를 사용할 수도 있습니다. 이 클래스의 생성자는 문에서 호출 되기 위한 것입니다. `using` canvas 상태는 블록의 끝 부분에서 자동으로 복원 됩니다 `using` .

그러나 처리기의 한 호출에서 다음으로 전달 되는 변환에 대해 걱정할 필요가 없습니다 `PaintSurface` . 를 새로 호출할 때마다 `PaintSurface` `SKCanvas` 기본 변환이 포함 된 새 개체가 제공 됩니다.

변환의 또 다른 일반적인 용도는 `Translate` 그리기에 편리한 좌표를 사용 하 여 원래 만든 시각적 개체를 렌더링 하는 것입니다. 예를 들어 지점 (0, 0)의 중심을 사용 하 여 아날로그 시계의 좌표를 지정 하려고 할 수 있습니다. 그런 다음 변환을 사용 하 여 원하는 위치에 시계를 표시할 수 있습니다. 이 기술은 [**Hendecagram 배열**] 페이지에 설명 되어 있습니다. [`HendecagramArrayPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramArrayPage.cs)클래스는 `SKPath` 11 포인터가 가리키는 별모양에 대 한 개체를 만들기 시작 합니다. `HendecagramPath`개체는 다른 데모 프로그램에서 액세스할 수 있도록 공용, 정적 및 읽기 전용으로 정의 됩니다. 정적 생성자에 생성 됩니다.

```csharp
public class HendecagramArrayPage : ContentPage
{
    ...
    public static readonly SKPath HendecagramPath;

    static HendecagramArrayPage()
    {
        // Create 11-pointed star
        HendecagramPath = new SKPath();
        for (int i = 0; i < 11; i++)
        {
            double angle = 5 * i * 2 * Math.PI / 11;
            SKPoint pt = new SKPoint(100 * (float)Math.Sin(angle),
                                    -100 * (float)Math.Cos(angle));
            if (i == 0)
            {
                HendecagramPath.MoveTo(pt);
            }
            else
            {
                HendecagramPath.LineTo(pt);
            }
        }
        HendecagramPath.Close();
    }
}
```

별이 점 (0, 0) 인 경우에는 별의 모든 점이 해당 점을 둘러싼 원에 있습니다. 각 요소는 360도로 늘어나는 각도의 사인 및 코사인 값의 조합입니다. (각도를 2/11, 3/11ths 또는 원의 4/11로 높이는 경우에도 11 방향 별을 만들 수 있습니다.) 이 원의 반지름은 100로 설정 됩니다.

이 경로가 변형 없이 렌더링 되는 경우 중심은의 왼쪽 위 모퉁이에 배치 되 `SKCanvas` 고 그의 사분기만 표시 됩니다. `PaintSurface`대신의 처리기는 `HendecagramPage` 를 사용 하 여 `Translate` 각각 무작위로 색이 지정 된 별 여러 복사본을 사용 하 여 캔버스를 바둑판식으로 배열 합니다.

```csharp
public class HendecagramArrayPage : ContentPage
{
    Random random = new Random();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            for (int x = 100; x < info.Width + 100; x += 200)
                for (int y = 100; y < info.Height + 100; y += 200)
                {
                    // Set random color
                    byte[] bytes = new byte[3];
                    random.NextBytes(bytes);
                    paint.Color = new SKColor(bytes[0], bytes[1], bytes[2]);

                    // Display the hendecagram
                    canvas.Save();
                    canvas.Translate(x, y);
                    canvas.DrawPath(HendecagramPath, paint);
                    canvas.Restore();
                }
        }
    }
}

```

결과는 다음과 같습니다.

[![](translate-images/hendecagramarray-small.png "Triple screenshot of the Hendecagram Array page")](translate-images/hendecagramarray-large.png#lightbox "Triple screenshot of the Hendecagram Array page")

애니메이션은 종종 변환과 관련 됩니다. **Hendecagram 애니메이션** 페이지는 원에서 11 포인트가 가리키는 별을 이동 합니다. [`HendecagramAnimationPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs)클래스는 `OnAppearing` `OnDisappearing` 타이머를 시작 하 고 중지 하기 위해 및 메서드의 몇 가지 필드 및 재정의로 시작 합니다 Xamarin.Forms .

```csharp
public class HendecagramAnimationPage : ContentPage
{
    const double cycleTime = 5000;      // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float angle;

    public HendecagramAnimationPage()
    {
        Title = "Hedecagram Animation";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
            angle = (float)(360 * t);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }

            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`angle`이 필드는 5 초 마다 0도에서 360도까지 애니메이션 효과를 가집니다. `PaintSurface`처리기는 `angle` 두 가지 방법으로 속성을 사용 합니다. 즉, 메서드에서 색의 색상을 지정 하 고 및 메서드에 대 한 인수를 사용 하 여 `SKColor.FromHsl` `Math.Sin` `Math.Cos` 별 위치를 제어 합니다.

```csharp
public class HendecagramAnimationPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.Translate(info.Width / 2, info.Height / 2);
        float radius = (float)Math.Min(info.Width, info.Height) / 2 - 100;

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColor.FromHsl(angle, 100, 50);

            float x = radius * (float)Math.Sin(Math.PI * angle / 180);
            float y = -radius * (float)Math.Cos(Math.PI * angle / 180);
            canvas.Translate(x, y);
            canvas.DrawPath(HendecagramPage.HendecagramPath, paint);
        }
    }
}
```

`PaintSurface`처리기는 메서드를 `Translate` 두 번 호출 하 고, 먼저 캔버스의 중심으로 이동한 다음 (0, 0) 중심 원의 원주로 변환 합니다. 페이지의 범위 내에서 별표를 계속 유지 하면서 원의 반지름은 최대한 크게 설정 됩니다.

[![](translate-images/hendecagramanimation-small.png "Triple screenshot of the Hendecagram Animation page")](translate-images/hendecagramanimation-large.png#lightbox "Triple screenshot of the Hendecagram Animation page")

별모양은 페이지의 중심을 중심으로 하는 것과 동일한 방향으로 유지 됩니다. 회전 하지 않습니다. 회전 변환에 대 한 작업입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
