---
title: 좌표 이동 변환
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 SkiaSharp 그래픽 이동할 좌표 이동 변환을 사용 하는 방법을 검사 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: f1efd7610b32e6a3903d34fc2f8b5a6e20c9da8a
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723605"
---
# <a name="the-translate-transform"></a>좌표 이동 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_변환 변환을 사용 하 여 SkiaSharp 그래픽을 이동 하는 방법을 알아봅니다._

SkiaSharp에서 가장 *간단한 변환 형식은 변환 또는 변환* 입니다. 이 변환은 가로 및 세로 방향으로 그래픽 개체를 이동합니다. 어떤 의미에서 변환 이므로 가장 불필요 한 변환을 간단히 그리기 함수에서 사용 하는 좌표를 변경 하 여 동일한 효과 일반적으로 수행할 수 있습니다. 하지만 경로 렌더링 하는 경우 모든 좌표에에서 캡슐화 됩니다 경로 훨씬 쉽게 전체 경로 이동 하려면 이동 변환을 적용 되므로.

번역에 대 한 간단한 텍스트 효과 및 애니메이션에 대 한 유용한 이기도합니다.

![](translate-images/translateexample.png "Text shadow, engraving, and embossing with translation")

`SKCanvas`의 [`Translate`](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single)) 메서드는 다음의 두 매개 변수를 포함 하 여 이후에 그려진 그래픽 개체를 가로 및 세로로 이동 시킵니다.

```csharp
public void Translate (Single dx, Single dy)
```

이러한 인수는 음수일 수 있습니다. 두 번째 [`Translate`](xref:SkiaSharp.SKCanvas.Translate(SkiaSharp.SKPoint)) 메서드는 두 개의 변환 값을 단일 `SKPoint` 값으로 결합 합니다.

```csharp
public void Translate (SKPoint point)
```

[**SkiaSharpForms**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플 프로그램의 **누적 번역** 페이지는 `Translate` 메서드의 여러 호출이 누적 됨을 보여 줍니다. [`AccumulatedTranslatePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) 클래스는 동일한 사각형의 20 개 버전을 표시 합니다 .이는 각각 이전 사각형에서 한 번만 오프셋 하 여 대각선을 따라 확장 합니다. `PaintSurface` 이벤트 처리기는 다음과 같습니다.

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

페이지 아래쪽에 있는 연속 사각형 trickle:

[![](translate-images/accumulatedtranslate-small.png "Triple screenshot of the Accumulated Translate page")](translate-images/accumulatedtranslate-large.png#lightbox "Triple screenshot of the Accumulated Translate page")

누적 된 변환 요소가 `dx` 및 `dy`이 고 그리기 함수에서 지정 하는 점이 (`x`, `y`) 인 경우 그래픽 개체는 지점 (`x'`, `y'`)에서 렌더링 됩니다.

x' = x + dx

y' = y + dy

이러한 *변환을 변환 수식* 이라고 합니다. 새 `SKCanvas`에 대 한 `dx` 및 `dy`의 기본값은 0입니다.

**텍스트 변환 효과** 페이지가 보여 주는 것 처럼 그림자 효과에 대 한 변환 변환 및 유사한 기술을 사용 하는 것이 일반적입니다. 다음은 [`TranslateTextEffectsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) 클래스에서 `PaintSurface` 처리기의 관련 부분입니다.

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

세 가지 예제 각각에서 `x` 및 `y` 변수가 제공 하는 위치에서 오프셋할 텍스트를 표시 하기 위해 `Translate`가 호출 됩니다. 다음 텍스트는 변환 효과가 다른 색에 다시 표시 됩니다.

[![](translate-images/translatetexteffects-small.png "Triple screenshot of the Translate Text Effects page")](translate-images/translatetexteffects-large.png#lightbox "Triple screenshot of the Translate Text Effects page")

세 가지 예제는 각각 `Translate` 호출을 부정 하는 다른 방법을 보여 줍니다.

첫 번째 예제에서는 `Translate`을 다시 호출 하지만 음수 값을 사용 합니다. `Translate` 호출은 누적 이므로이 호출 시퀀스는 전체 번역을 기본값인 0으로 복원 합니다.

두 번째 예제에서는 [`ResetMatrix`](xref:SkiaSharp.SKCanvas.ResetMatrix)를 호출 합니다. 이렇게 하면 모든 변환 기본 상태로 돌아갑니다.

세 번째 예제에서는 [`Save`](xref:SkiaSharp.SKCanvas.Save) 에 대 한 호출을 사용 하 여 `SKCanvas` 개체의 상태를 저장 한 다음 [`Restore`](xref:SkiaSharp.SKCanvas.Restore)를 호출 하 여 상태를 복원 합니다. 이 그리기 작업에 대 한 변환 조작에 대 한 가장 다양 한 방법입니다. 이러한 `Save` 및 `Restore`는 스택 처럼 함수를 호출 합니다. `Save`를 여러 번 호출 하 고 역방향으로 `Restore`를 호출 하 여 이전 상태로 되돌릴 수 있습니다. `Save` 메서드는 정수를 반환 하 고 [`RestoreToCount`](xref:SkiaSharp.SKCanvas.RestoreToCount*) 에 해당 정수를 전달 하 여 `Restore`를 여러 번 호출할 수 있습니다. [`SaveCount`](xref:SkiaSharp.SKCanvas.SaveCount) 속성은 스택에 현재 저장 된 상태 수를 반환 합니다.

Canvas 상태를 복원 하는 데 [`SKAutoCanvasRestore`](xref:SkiaSharp.SKAutoCanvasRestore) 클래스를 사용할 수도 있습니다. 이 클래스의 생성자는 `using` 문에서 호출 되기 위한 것입니다. canvas 상태는 `using` 블록의 끝 부분에서 자동으로 복원 됩니다.

그러나 `PaintSurface` 처리기의 한 호출에서 다음으로 전달 되는 변환에 대해 걱정할 필요가 없습니다. `PaintSurface`에 대 한 새 호출은 모두 기본 변환과 함께 새로운 `SKCanvas` 개체를 제공 합니다.

`Translate` 변환은 일반적으로 그리기에 편리한 좌표를 사용 하 여 만든 시각적 개체를 렌더링 하는 데 사용 됩니다. 예를 들어, 다음 지점 (0, 0)에 center를 사용 하 여 아날로그 시계의 좌표를 지정 하는 것이 좋습니다. 사용할 수 있습니다 다음 변환을 시계를 표시 하려면 원하는 위치. 이 기술은 [**Hendecagram 배열**] 페이지에 설명 되어 있습니다. [`HendecagramArrayPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramArrayPage.cs) 클래스는 11 번째 별모양에 대 한 `SKPath` 개체를 만들어 시작 합니다. `HendecagramPath` 개체는 다른 데모 프로그램에서 액세스할 수 있도록 공용, 정적 및 읽기 전용으로 정의 됩니다. 정적 생성자에 생성 됩니다.

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

별모양의 중심 점 (0, 0) 인 경우 별표의 모든 점을 해당 지점 주변의 원에. 각 요소에는 5/11ths 360 씩 증가 하는 각도의 사인 및 코사인 값의 조합입니다. (각도를 2/11, 3/11ths 또는 원의 4/11로 높이는 경우에도 11 방향 별을 만들 수 있습니다.) 이 원의 반지름은 100로 설정 됩니다.

이 경로가 변형 없이 렌더링 되는 경우 중심은 `SKCanvas`의 왼쪽 위 모퉁이에 배치 되 고 그에 해당 하는 1/4만 표시 됩니다. 대신 `HendecagramPage`의 `PaintSurface` 처리기는 `Translate`를 사용 하 여 각각 무작위로 색이 지정 된 별 여러 복사본으로 캔버스를 바둑판식으로 배열 합니다.

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

결과:

[![](translate-images/hendecagramarray-small.png "Triple screenshot of the Hendecagram Array page")](translate-images/hendecagramarray-large.png#lightbox "Triple screenshot of the Hendecagram Array page")

애니메이션 변환을 경우가 많습니다. **Hendecagram 애니메이션** 페이지는 원에서 11 포인트가 가리키는 별을 이동 합니다. [`HendecagramAnimationPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) 클래스는 일부 필드 및 재정의로 시작 하 고 Xamarin. Forms 타이머를 시작 및 중지 하는 `OnDisappearing` 메서드를 `OnAppearing` 합니다.

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

`angle` 필드는 5 초 마다 0도에서 360까지 애니메이션 효과를 적용 합니다. `PaintSurface` 처리기는 다음과 같은 두 가지 방법으로 `angle` 속성을 사용 합니다. `SKColor.FromHsl` 메서드에서 색의 색상을 지정 하 고 `Math.Sin`에 대 한 인수로 별모양의 위치를 제어 하는 `Math.Cos` 메서드를 사용 합니다.

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

`PaintSurface` 처리기는 `Translate` 메서드를 두 번 호출 하 고, 먼저 캔버스의 중심으로 이동한 다음 중심으로 하는 원의 원주 (0, 0)로 변환 합니다. 원의 반지름은 페이지의 범위 내에서 별표는 그대로 유지 하면서 가장 크게 되도록 설정 됩니다.

[![](translate-images/hendecagramanimation-small.png "Triple screenshot of the Hendecagram Animation page")](translate-images/hendecagramanimation-large.png#lightbox "Triple screenshot of the Hendecagram Animation page")

이 페이지의 가운데를 중심으로 하는 대로 별 동일한 방향을 유지 함을 알 수 있습니다. 이 작업은 전혀 회전 하지 않습니다. 회전 변환에 대 한 작업입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
