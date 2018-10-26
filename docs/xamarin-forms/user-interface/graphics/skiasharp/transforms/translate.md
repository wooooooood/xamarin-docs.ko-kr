---
title: 좌표 이동 변환
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 SkiaSharp 그래픽 이동할 좌표 이동 변환을 사용 하는 방법을 검사 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 2171c8f0b2926a645fb98df52bae2391449bf89c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120933"
---
# <a name="the-translate-transform"></a>좌표 이동 변환

_SkiaSharp 그래픽 이동할 좌표 이동 변환 사용 방법 알아보기_

SkiaSharp에서 변환의 가장 간단한 유형이 합니다 *변환* 또는 *번역* 변환 합니다. 이 변환은 가로 및 세로 방향으로 그래픽 개체를 이동합니다. 어떤 의미에서 변환 이므로 가장 불필요 한 변환을 간단히 그리기 함수에서 사용 하는 좌표를 변경 하 여 동일한 효과 일반적으로 수행할 수 있습니다. 하지만 경로 렌더링 하는 경우 모든 좌표에에서 캡슐화 됩니다 경로 훨씬 쉽게 전체 경로 이동 하려면 이동 변환을 적용 되므로.

번역에 대 한 간단한 텍스트 효과 및 애니메이션에 대 한 유용한 이기도합니다.

![](translate-images/translateexample.png "조각, 및 번역을 사용 하 여 볼록 텍스트 그림자")

합니다 [ `Translate` ](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single)) 메서드에서 `SKCanvas` 가로 및 세로로 이동할 이후에 그려지는 그래픽 개체는 두 매개 변수가:

```csharp
public void Translate (Single dx, Single dy)
```

이러한 인수는 음수일 수 있습니다. 두 번째 [ `Translate` ](xref:SkiaSharp.SKCanvas.Translate(SkiaSharp.SKPoint)) 메서드는 단일에서 두 변환 값을 결합 `SKPoint` 값:

```csharp
public void Translate (SKPoint point)
```

**변환 누적** 페이지를 [ **SkiaSharpForms** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 샘플 프로그램의 여러 호출 하는 방법을 보여 줍니다는 `Translate` 메서드 누적 됩니다. 합니다 [ `AccumulatedTranslatePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) 동일한 영역의 20 버전을 표시 하는 클래스, 각각 오프셋 이전 사각형에서 충분 대각선을 따라 stretch 있습니다 있도록 합니다. 다음은 `PaintSurface` 이벤트 처리기:

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

[![](translate-images/accumulatedtranslate-small.png "누적 변환 페이지의 3 배가 스크린샷")](translate-images/accumulatedtranslate-large.png#lightbox "삼중 누적 변환 페이지 스크린샷")

누적된 변환 요소를 사용 하는 경우 `dx` 하 고 `dy`, 그리기 함수에서 지정 하는 점은 이며 (`x`, `y`), 그래픽 개체는 지점에는 렌더링 한 다음 (`x'`, `y'`) 여기서:

x' = x + dx

y' = y + dy

이러한 라고 합니다 *수식 변환* 번역에 대 한 합니다. 값을 기본값으로 `dx` 하 고 `dy` 새 `SKCanvas` 은 0입니다.

일반적으로 좌표 이동 변환 그림자 효과 및 유사한 기술을 사용 하는 **번역할 텍스트 효과** 페이지를 보여 줍니다. 여기의 관련 부분은 합니다 `PaintSurface` 처리기에는 [ `TranslateTextEffectsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) 클래스:

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

세 가지 예의 각 `Translate` 텍스트를 제공한 위치에서 오프셋을 표시 하기 위해 호출 됩니다 합니다 `x` 및 `y` 변수입니다. 다음 텍스트는 변환 효과가 다른 색에 다시 표시 됩니다.

[![](translate-images/translatetexteffects-small.png "텍스트 효과 변환 페이지의 3 배가 스크린샷")](translate-images/translatetexteffects-large.png#lightbox "삼중 텍스트 효과 변환 페이지 스크린샷")

부정 하는 다양 한 방법을 보여 줍니다 각 세 가지 예제는 `Translate` 호출 합니다.

첫 번째 예제에서는 호출 `Translate` 다시 하지만 음의 값을 사용 합니다. 때문에 `Translate` 호출은 누적, 단순히이 호출 시퀀스 0의 기본값으로 총 번역을 복원 합니다.

두 번째 예제에서는 호출 [ `ResetMatrix` ](xref:SkiaSharp.SKCanvas.ResetMatrix)합니다. 이렇게 하면 모든 변환 기본 상태로 돌아갑니다.

상태를 저장 하는 세 번째 예제는 `SKCanvas` 개체에 대 한 호출을 사용 하 여 [ `Save` ](xref:SkiaSharp.SKCanvas.Save) 다음에 대 한 호출을 사용 하 여 상태를 복원 [ `Restore` ](xref:SkiaSharp.SKCanvas.Restore)합니다. 이 그리기 작업에 대 한 변환 조작에 대 한 가장 다양 한 방법입니다. 이러한 `Save` 하 고 `Restore` 스택 처럼 함수를 호출: 호출할 수 있습니다 `Save` 여러 시간 및 호출 `Restore` 반대로 이전 상태를 반환할 시퀀스입니다. 합니다 `Save` 메서드는 정수를 반환 하 고 해당 정수를 전달할 수 있습니다 [ `RestoreToCount` ](xref:SkiaSharp.SKCanvas.RestoreToCount*) 효과적으로 호출 하려면 `Restore` 여러 번입니다. 합니다 [ `SaveCount` ](xref:SkiaSharp.SKCanvas.SaveCount) 속성 스택의 현재 저장 된 상태의 수를 반환 합니다.

사용할 수도 있습니다는 [ `SKAutoCanvasRestore` ](xref:SkiaSharp.SKAutoCanvasRestore) 캔버스 상태를 복원 하기 위한 클래스입니다. 이 클래스의 생성자에서 호출 될 것을 `using` 문; 캔버스 상태가 끝날 때 자동으로 복원 되는 `using` 블록. 

그러나 한 호출에서 가져올 변환에 걱정할 필요가 없습니다를 `PaintSurface` 다음 처리기입니다. 호출할 때마다 새 `PaintSurface` 에 새로 제공 `SKCanvas` 기본 변환 사용 하 여 개체입니다.

또 다른 일반적인 용도 `Translate` 시각적 개체는 렌더링 원래 만들어진 것 그리기에 대 한 편리한는 좌표를 사용 하 여 변환입니다. 예를 들어, 다음 지점 (0, 0)에 center를 사용 하 여 아날로그 시계의 좌표를 지정 하는 것이 좋습니다. 사용할 수 있습니다 다음 변환을 시계를 표시 하려면 원하는 위치. 이 기술은에 설명 되어는 [**Hendecagram 배열**] 페이지입니다. [ `HendecagramArrayPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramPage.cs) 클래스를 만들어 시작을 `SKPath` 11 가리킨 스타는 개체입니다. `HendecagramPath` 다른 데모 프로그램에서 액세스할 수 있도록 개체는 public, 정적 및 읽기 전용으로 정의 됩니다. 정적 생성자에 생성 됩니다.

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

별모양의 중심 점 (0, 0) 인 경우 별표의 모든 점을 해당 지점 주변의 원에. 각 요소에는 5/11ths 360 씩 증가 하는 각도의 사인 및 코사인 값의 조합입니다. (2 방향의 각도 증가 시켜는 11 점이 개인 별을 만들 수 이기도 11, / / 11ths, 3 또는 4/원의 11.) 해당 원의 반지름은 100으로 설정 됩니다.

이 경로 변환 하지 않고 렌더링 중심의 왼쪽 위 모퉁이에 배치 됩니다는 `SKCanvas`, 이며 해당 사분기만 표시 됩니다. 합니다 `PaintSurface` 처리기 `HendecagramPage` 대신 사용 하 여 `Translate` 타일 별의 여러 복사본을 사용 하 여 캔버스에 각각 임의로 색이 지정:

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

결과 다음과 같습니다.

[![](translate-images/hendecagramarray-small.png "삼중 Hendecagram 배열 페이지 스크린샷")](translate-images/hendecagramarray-large.png#lightbox "삼중 Hendecagram 배열 페이지 스크린샷")

애니메이션 변환을 경우가 많습니다. 합니다 **Hendecagram 애니메이션** 페이지 이동 11 가리킨 별 원 안에 있습니다. 합니다 [ `HendecagramAnimationPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) 클래스는 일부 필드를 사용 하 여 시작 하 고의 재정의 `OnAppearing` 및 `OnDisappearing` Xamarin.Forms 타이머를 중지 및 시작 방법:

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

`angle` 필드 애니메이션이 적용 되어 0도에서 360도 5 초 마다. `PaintSurface` 처리기에서 사용 합니다 `angle` 두 가지 방법으로 속성: 색의 색상을 지정 하는 `SKColor.FromHsl` 메서드를 및 인수로 `Math.Sin` 및 `Math.Cos` 별의 위치를 제어 하는 방법:

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

`PaintSurface` 처리기 호출을 `Translate` 캔버스의 가운데에 변환 하려면 먼저 메서드를 두 번 차례로 중심으로 원의 원주 변환할 (0, 0). 원의 반지름은 페이지의 범위 내에서 별표는 그대로 유지 하면서 가장 크게 되도록 설정 됩니다.

[![](translate-images/hendecagramanimation-small.png "삼중 Hendecagram 애니메이션 페이지 스크린샷")](translate-images/hendecagramanimation-large.png#lightbox "삼중 Hendecagram 애니메이션 페이지 스크린샷")

이 페이지의 가운데를 중심으로 하는 대로 별 동일한 방향을 유지 함을 알 수 있습니다. 이 작업은 전혀 회전 하지 않습니다. 회전 변환에 대 한 작업입니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
