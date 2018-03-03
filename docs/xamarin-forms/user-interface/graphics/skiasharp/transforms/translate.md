---
title: "이동 변환"
description: "SkiaSharp 그래픽을 이동 하려면 이동 변환을 사용 하는 방법을 알아봅니다"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 491c82406dafceb876ddbb4a0a7204447b95f57d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="the-translate-transform"></a>이동 변환

_SkiaSharp 그래픽을 이동 하려면 이동 변환을 사용 하는 방법을 알아봅니다_

가장 간단한 SkiaSharp에서 변형을 형식이 *번역* 또는 *번역* 변환 합니다. 이 변환에는 가로 및 세로 방향으로 그래픽 개체 프로비저닝으로 전환 됩니다. 관점에서 번역은 단순히 그리기 기능에서 사용 하는 좌표를 변경 하 여 일반적으로 동일한 효과 얻을 수 있으므로 가장 불필요 한 변환 합니다. 그러나 경로 렌더링할 때 모든 좌표가에 캡슐화 됩니다 경로 훨씬 쉽게 전체 경로 이동 하려면 이동 변환을 적용 되기 때문입니다.

번역은 애니메이션 하 고 간단한 텍스트 효과 대 한 유용 이기도합니다.

![](translate-images/translateexample.png "텍스트 그림자, 조각, 볼록 변환 사용")

[ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) 메서드에서 `SKCanvas` 이후에 그려지는 그래픽 개체가 이동 하 여 가로 및 세로로 두 개의 매개 변수가:

```csharp
public void Translate (Single dx, Single dy)
```

이 인수는 음수가 될 수 있습니다. 두 번째 [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/SkiaSharp.SKPoint/) 단일에서 두 변환 값을 결합 하는 메서드 `SKPoint` 값:

```csharp
public void Translate (SKPoint point)
```

**번역 누적** 의 페이지는 [ **SkiaSharpForms** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) 프로그램 예제를 여러 번 호출 하는 방법을 보여 줍니다는 `Translate` 메서드 누적 되어 증가 합니다. [ `AccumulatedTranslate` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) 같은 사각형의 20 버전을 표시 하는 클래스, 각 오프셋 이전 사각형에서 데 필요한 만큼만 대각선 따라 스트레치은 하도록 합니다. 다음은 `PaintSurface` 이벤트 처리기.

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

연속 된 사각형은 페이지 아래로 trickle:

[![](translate-images/accumulatedtranslate-small.png "누적 된 변환 페이지의 삼중 스크린샷")](translate-images/accumulatedtranslate-large.png "누적 변환 페이지의 삼중 스크린 샷")

누적 된 변환 요소는 경우 `dx` 및 `dy`, 그리기 함수에 지정한 점은 및 (`x`, `y`), 그래픽 개체는 지점에는 렌더링 한 다음 (`x'`, `y'`), 여기서:

x' = x + dx

y' y + dy =

이 라고는 *수식 변환* 번역에 대 한 합니다. 기본값 `dx` 및 `dy` 새 `SKCanvas` 는 0입니다.

일반적으로 기호로 이동 변환을 그림자 효과 유사한 방법을 사용 하는 **텍스트 효과 번역** 페이지를 보여 줍니다. 다음의 관련 부분을은 `PaintSurface` 의 처리기는 [ `TranslateTextEffectsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) 클래스:

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

각 3 개의 예제 `Translate` 로 제공 된 위치에서 오프셋을 텍스트 표시에 대해 호출 됩니다는 `x` 및 `y` 변수입니다. 다음 텍스트는 영향을 주지 번역 다른 색에 다시 표시 됩니다.

[![](translate-images/translatetexteffects-small.png "텍스트 효과 변환 페이지의 삼중 스크린샷")](translate-images/translatetexteffects-large.png "텍스트 효과 변환 페이지의 삼중 스크린샷")

부정 하는 다른 방법을 보여 줍니다 각각의 세 가지 예제는 `Translate` 호출:

첫 번째 예에서는 단순히 호출 `Translate` 다시 있지만 음수 값입니다. 때문에 `Translate` 호출은 누적,이 호출의이 시퀀스는 단순히 총 번역 0의 기본값을 복원 합니다.

두 번째 예제에서는 [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix()/)합니다. 이렇게 하면 모든 변환 기본 상태로 돌아갑니다.

상태를 저장 하는 세 번째 예제는의 `SKCanvas` 개체를 호출 하 여 [ `Save` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Save()/) 다음을 호출 하 여 상태를 복원 하 고 [ `Restore` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Restore/)합니다. 이 방법은 일련의 그리기 작업에 대 한 변환을 조작 하는 가장 용도가 넓은 함수로입니다. 이러한 `Save` 및 `Restore` 스택 처럼 함수를 호출: 호출할 수 있습니다 `Save` 여러 시간 및 호출 `Restore` 에서는 역방향 시퀀스를 이전 상태로 돌아갑니다. `Save` 메서드는 정수를 반환 하 고 해당 정수를 전달할 수 있습니다 [ `RestoreToCount` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RestoreToCount/) 효과적으로 호출 하려면 `Restore` 여러 번입니다. [ `SaveCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.SaveCount/) 속성의 현재 스택에 저장 된 상태 수를 반환 합니다.

그러나 한 번의 호출의에서 통해 수행 하는 변환에 대 한 중요 하지 않은 `PaintSurface` 다음 처리기입니다. 호출할 때마다 새 `PaintSurface` 새 배달 `SKCanvas` 기본 변환 사용 하 여 개체입니다.

또 다른 일반적인 용도 `Translate` 시각적 개체를 렌더링 하는 원래 만든는 그리기에 편리 하 게 하는 좌표를 사용 하 여 변환입니다. 예를 들어 다음 지점 (0, 0)에 센터와 아날로그 시계의 좌표를 지정 하는 것이 좋습니다. 사용할 수 있습니다 다음 변환 하 여 표시할 원하는 위치. 이 확인할는 [**Hendecagram 배열**] 페이지. [ `HendecagramArrayPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/HendecagramPage.cs) 클래스를 만들어 시작 프로그램 `SKPath` 11 점이 별에 대 한 개체입니다. `HendecagramPath` 개체 이루어집니다 public, static, 및 읽기 전용으로 다른 데모 프로그램에서 액세스할 수 있도록 합니다. 정적 생성자에는 것이 만들어집니다.

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

별모양의 중심은 점 (0, 0), 하는 경우 핫스폿에 모든 지점 원을 둘러싼 해당 지점에 있습니다. 각 지점에 5/11ths 360 씩 증가 하 여 각도의 사인 값 및 코사인 값의 조합입니다. (2 각도 늘려 11 점이 별을 만들 수 이기도, 11/3/11ths 또는 4 원의 11 /.) 해당 원의 반지름은 100으로 설정 됩니다.

이 경로 변환 하지 않고 렌더링 중심의 왼쪽 위 모퉁이에 배치 됩니다는 `SKCanvas`, 및의 1/4만 표시 됩니다. `PaintSurface` 처리기 `HendecagramPage` 대신 사용 하 여 `Translate` 별의 여러 복사본을 사용 하 여 캔버스를 바둑판식으로 배열 하려면 각각 임의로 색이 지정 됩니다.

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

다음은 결과가입니다.

[![](translate-images/hendecagramarray-small.png "Hendecagram 배열 페이지의 삼중 스크린샷")](translate-images/hendecagramarray-large.png "Hendecagram 배열 페이지의 삼중 스크린샷")

애니메이션에 변환을 포함 되는 경우도 있습니다. **Hendecagram 애니메이션** 페이지 원 안에 11 점이 개인 별을 이동 합니다. [ `HendecagramAnimationPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) 클래스 일부 필드부터 시작 하 고의 재정의 `OnAppearing` 및 `OnDisappearing` 시작 하 고 Xamarin.Forms 타이머를 중지 하는 메서드:

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

`angle` 필드 애니메이션 효과가 적용 되어 0에서 360도 5 초 마다. `PaintSurface` 처리기 사용 하 여는 `angle` 두 가지 방법으로 속성:에서 색의 색상을 지정 하는 `SKColor.FromHsl` 메서드를 및에 대 한 인수로 `Math.Sin` 및 `Math.Cos` 별의 위치를 제어 하는 메서드:

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

`PaintSurface` 처리기 호출의 `Translate` 캔버스의 가운데에 번역을 먼저 메서드를 두 번 차례로 중심으로 원의 원주를 변환할 (0, 0). 페이지의 범위 내에서 별표를 그대로 유지 하면서을 가능한 한 크게 되도록를 원의 반지름에 대 한 설정입니다.

[![](translate-images/hendecagramanimation-small.png "Hendecagram 애니메이션 페이지의 삼중 스크린샷")](translate-images/hendecagramanimation-large.png "Hendecagram 애니메이션 페이지의 삼중 스크린샷")

페이지의 가운데에서 흥미로운 것 처럼 별 동일한 방향을 유지 함을 확인 합니다. 전혀 회전 하지 않습니다. 회전 변형에 대 한 작업입니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
