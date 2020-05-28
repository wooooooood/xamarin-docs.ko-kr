---
title: ''
description: 이 문서에서는 응용 프로그램에서 SkiaSharp 그래픽에 애니메이션 효과를 주는 방법을 설명 하 Xamarin.Forms 고 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9a59f65655772768860ce29128f14a48641abc26
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84134279"
---
# <a name="basic-animation-in-skiasharp"></a>SkiaSharp의 기본 애니메이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp 그래픽에 애니메이션을 적용 하는 방법을 알아봅니다._

메서드를 주기적으로 호출 하 여 SkiaSharp 그래픽에 애니메이션 효과를 적용할 수 있습니다 Xamarin.Forms `PaintSurface` . 각 시간 마다 그래픽을 약간 다르게 그릴 수 있습니다. 다음은 가운데에서 확장 되는 동심 원이 있는이 문서의 뒷부분에 표시 되는 애니메이션입니다.

![](animation-images/animationexample.png "Several concentric circles seemingly expanding from the center")

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 프로그램의 **Pulsating ellipse** 페이지는 타원의 두 축에 애니메이션 효과를 주기 위해 타원의 두 축에 애니메이션 효과를 주기 때문에이 pulsation의 요율도 제어할 수 있습니다. [**PulsatingEllipsePage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) 파일은 Xamarin.Forms `Slider` `Label` 슬라이더의 현재 값을 표시 하는 및를 인스턴스화합니다. 이는를 다른 뷰와 통합 하는 일반적인 방법입니다 `SKCanvasView` Xamarin.Forms .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.PulsatingEllipsePage"
             Title="Pulsating Ellipse">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Slider x:Name="slider"
                Grid.Row="0"
                Maximum="10"
                Minimum="0.1"
                Value="5"
                Margin="20, 0" />

        <Label Grid.Row="1"
               Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Cycle time = {0:F1} seconds'}"
               HorizontalTextAlignment="Center" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

코드를 사용 하는 파일은 `Stopwatch` 높은 정밀도 클록으로 사용할 개체를 인스턴스화합니다. `OnAppearing`재정의는 필드를로 설정 하 `pageIsActive` `true` 고 라는 메서드를 호출 합니다 `AnimationLoop` . `OnDisappearing`재정의는 `pageIsActive` 다음 필드를로 설정 합니다 `false` .

```csharp
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float scale;            // ranges from 0 to 1 to 0

public PulsatingEllipsePage()
{
    InitializeComponent();
}

protected override void OnAppearing()
{
    base.OnAppearing();
    pageIsActive = true;
    AnimationLoop();
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    pageIsActive = false;
}
```

메서드는를 `AnimationLoop` 시작한 `Stopwatch` 다음가 인 동안 루프를 실행 합니다 `pageIsActive` `true` . 이는 기본적으로 페이지가 활성화 되어 있는 동안 "무한 루프" 이지만 루프가 종료 되지 않습니다. 연산자를 사용 하 여에 대 한 호출로 인해 루프가 종료 되므로 `Task.Delay` `await` 프로그램의 다른 부분을 사용할 수 있습니다. 인수는 `Task.Delay` 1/30 초 후에 완료 되도록 합니다. 애니메이션의 프레임 주기를 정의 합니다.

```csharp
async Task AnimationLoop()
{
    stopwatch.Start();

    while (pageIsActive)
    {
        double cycleTime = slider.Value;
        double t = stopwatch.Elapsed.TotalSeconds % cycleTime / cycleTime;
        scale = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;
        canvasView.InvalidateSurface();
        await Task.Delay(TimeSpan.FromSeconds(1.0 / 30));
    }

    stopwatch.Stop();
}

```

`while`루프는에서 주기 시간을 가져와 시작 합니다 `Slider` . 이 시간은 5와 같은 시간 (초)입니다. 두 번째 문은 `t` *time*에 대 한 값을 계산 합니다. 의 경우 `cycleTime` `t` 5 초 마다 0에서 1로 늘립니다. `Math.Sin`두 번째 문의 함수에 대 한 인수 범위는 5 초 마다 0에서 2π 사이입니다. `Math.Sin`이 함수는 0에서 1 사이의 값을 반환 하 고 &ndash; 5 초 마다 1과 0으로 값을 반환 하지만 값이 1 또는-1에 가까워지면 더 느리게 변경 되는 값을 반환 합니다. 값 1이 추가 되어 값이 항상 양수 이며 2로 나뉘어 있으므로 값의 범위는 1/2에서 1에서 1/2 1/2 사이이 고 값이 1과 0 이면 속도가 느립니다. 이는 필드에 저장 되 `scale` 고는 `SKCanvasView` 무효화 됩니다.

`PaintSurface`메서드는이 값을 사용 하 여 `scale` 타원의 두 축을 계산 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float maxRadius = 0.75f * Math.Min(info.Width, info.Height) / 2;
    float minRadius = 0.25f * maxRadius;

    float xRadius = minRadius * scale + maxRadius * (1 - scale);
    float yRadius = maxRadius * scale + minRadius * (1 - scale);

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.Color = SKColors.Blue;
        paint.StrokeWidth = 50;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);

        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.SkyBlue;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
    }
}
```

이 메서드는 표시 영역의 크기와 최대 반지름을 기준으로 하는 최소 반경을 기반으로 최대 반지름을 계산 합니다. `scale`값은 0에서 1 사이에 애니메이션을 적용 하 여 0으로 다시 계산 하므로 메서드는이를 사용 하 여와 `xRadius` 사이의 범위를 계산 합니다 `yRadius` `minRadius` `maxRadius` . 이러한 값은 타원을 그리거나 채우는 데 사용 됩니다.

[![](animation-images/pulsatingellipse-small.png "Triple screenshot of the Pulsating Ellipse page")](animation-images/pulsatingellipse-large.png#lightbox "Triple screenshot of the Pulsating Ellipse page")

`SKPaint`개체는 블록에 생성 됩니다 `using` . 많은 SkiaSharp 클래스와 마찬가지로 인터페이스를 구현 하는에서 `SKPaint` `SKObject` 파생 되는에서 파생 `SKNativeObject` [`IDisposable`](xref:System.IDisposable) 됩니다. `SKPaint`메서드를 재정의 `Dispose` 하 여 관리 되지 않는 리소스를 해제 합니다.

 블록을 배치 하면이 `SKPaint` `using` `Dispose` 관리 되지 않는 리소스를 해제 하기 위해 블록의 끝에서가 호출 됩니다. 이는 개체에서 사용 하는 메모리가 `SKPaint` .net 가비지 수집기에 의해 해제 되는 경우에도 마찬가지입니다. 하지만 애니메이션 코드에서는 메모리를 보다 순차적인 방법으로 해제 하는 것이 좋습니다.

 이 특정 사례에서 더 나은 솔루션은 두 `SKPaint` 개체를 한 번 만들어 필드로 저장 하는 것입니다.

이는 **확장 원** 애니메이션의 기능입니다. [`ExpandingCirclesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs)클래스는 개체를 포함 하 여 여러 필드를 정의 하는 것부터 시작 합니다 `SKPaint` .

```csharp
public class ExpandingCirclesPage : ContentPage
{
    const double cycleTime = 1000;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float t;
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke
    };

    public ExpandingCirclesPage()
    {
        Title = "Expanding Circles";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ...
}
```

이 프로그램은 메서드를 기반으로 하는 애니메이션에 다른 방법을 사용 Xamarin.Forms `Device.StartTimer` 합니다. `t`필드는 밀리초 마다 0에서 1 사이에 애니메이션을 적용 합니다 `cycleTime` .

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            t = (float)(stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime);
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

`PaintSurface`처리기는 애니메이션 반지름이 있는 5 개의 동심 원을 그립니다. `baseRadius`변수가 100로 계산 된 경우 `t` 0에서 1 사이에 애니메이션 효과를 주는 것 처럼 5 원의 반지름은 0에서 100, 100에서 200, 200에서 300, 300에서 400, 400에서 500로 증가 합니다. 대부분의 원에서는 `strokeWidth` 50 이지만 첫 번째 원의 경우 `strokeWidth` 0에서 50으로 애니메이션 효과를 적용 합니다. 대부분의 원에서 색은 파란색 이지만 마지막 원의 경우 색은 파란색에서 투명으로 애니메이션 효과를 적용 합니다. `SKColor`불투명도를 지정 하는 생성자의 네 번째 인수를 확인 합니다.

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
        float baseRadius = Math.Min(info.Width, info.Height) / 12;

        for (int circle = 0; circle < 5; circle++)
        {
            float radius = baseRadius * (circle + t);

            paint.StrokeWidth = baseRadius / 2 * (circle == 0 ? t : 1);
            paint.Color = new SKColor(0, 0, 255,
                (byte)(255 * (circle == 4 ? (1 - t) : 1)));

            canvas.DrawCircle(center.X, center.Y, radius, paint);
        }
    }
}
```

그 결과,가 1과 같은 경우에는 이미지가 동일 하 게 표시 되 `t` `t` 고 원은 계속 해 서 계속 확장 되는 것 처럼 보입니다.

[![](animation-images/expandingcircles-small.png "Triple screenshot of the Expanding Circles page")](animation-images/expandingcircles-large.png#lightbox "Triple screenshot of the Expanding Circles page")

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
