---
title: SkiaSharp의 기본적인 애니메이션
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 SkiaSharp 그래픽에 애니메이션을 적용 하는 방법에 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 0ba3d86f52d2e6907f32450d87f30280ade95d3f
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615641"
---
# <a name="basic-animation-in-skiasharp"></a>SkiaSharp의 기본적인 애니메이션

_SkiaSharp 그래픽에 애니메이션을 적용 하는 방법 알아보기_

시켜 Xamarin.Forms에서 SkiaSharp 그래픽 애니메이션을 적용할 수는 `PaintSurface` 약간 다르게 사용 되는 그래픽 그리기 시간 매우 빈번 하 게 호출 될 메서드를 각각. 센터에서 확장 된 것 처럼 보이는 동심원을 사용 하 여이 문서의 뒷부분에 표시 된 애니메이션은 다음과 같습니다.

![](animation-images/animationexample.png "여러 동심원 센터에서 보이는 확장")

합니다 **Pulsating 타원** 페이지에 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 프로그램 애니메이션 효과 적용 되는 타원의 두 가지 축을 pulsating 수에 표시 되도록 하 고 제어할 수 있습니다는 이 pulsation의 속도:


합니다 [ **PulsatingEllipsePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) 파일은는 Xamarin.Forms `Slider` 및 `Label` 슬라이더의 현재 값을 표시 합니다. 이 일반적으로 통합 하는 `SKCanvasView` 다른 Xamarin.Forms 뷰를 사용 하 여:

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

코드 숨김 파일을 인스턴스화하는 `Stopwatch` 고정밀 형식으로 처리 하는 개체입니다. `OnAppearing` 집합을 재정의 합니다 `pageIsActive` 필드를 `true` 라는 메서드를 호출 하 고 `AnimationLoop`입니다. 합니다 `OnDisappearing` 재정의 설정 하는 `pageIsActive` 필드를 `false`:

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

`AnimationLoop` 메서드가 시작 합니다 `Stopwatch` 고 하는 동안 다음 루프 `pageIsActive` 는 `true`. 이 기본적으로 "무한 루프" 페이지에 활성 상태 이지만 프로그램 루프에 대 한 호출 끝나는 때문에 중단을 수행 해도 해당 하는 동안 `Task.Delay` 사용 하 여는 `await` 프로그램 함수의 다른 부분을 수 있는 연산자입니다. 인수 `Task.Delay` 1/30 초 후 완료 하도록 할 수 있습니다. 이 애니메이션의 프레임 속도 정의합니다.

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

합니다 `while` 루프에서 주기 시간을 확보 하 여 시작 된 `Slider`합니다. 예를 들어 5 (초)에서 시간입니다. 값을 계산 하는 두 번째 문은 `t` 에 대 한 *시간*합니다. 에 `cycleTime` 5 `t` 5 초 마다 1 0에서 증가 합니다. 인수는 `Math.Sin` 함수에는 두 번째 문은 범위가 0 ~ 5 초 마다 2 π입니다. `Math.Sin` 함수는 0에서 1 다시 0으로 차례로 까지의 값을 반환 &ndash;1과 0 5 초 마다 값이 1 또는-1 값이 더 느리게 변경 합니다. 값 1은 값이 양수인 경우 항상 다음 것은 2로 나눈 값을 1을 0으로 약 1과 0 값이 저하 되지만 ½ ½로 ½에서 범위 있도록 하므로 추가 됩니다. 에 저장 되어이 `scale` 필드 및 `SKCanvasView` 무효화 됩니다.

합니다 `PaintSurface` 메서드는이 사용 하 여 `scale` 타원의 두 가지 축을 계산 하는 값:

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

메서드는 표시 영역의 크기를 기준으로 최대 반지름 및 최대 radius 기반 최소 반지름을 계산 합니다. `scale` 값 애니메이션이 적용 되어 0과 1 사이의 및 0으로 다시 메서드를 사용 하 여 하는 계산 되므로 `xRadius` 및 `yRadius` 사이 범위입니다 `minRadius` 및 `maxRadius`합니다. 이러한 값은 그리고 타원을 채우는 데 사용 됩니다.

[![](animation-images/pulsatingellipse-small.png "가득 찬 타원 페이지 스크린샷 삼중")](animation-images/pulsatingellipse-large.png#lightbox "삼중 가득 찬 타원 페이지 스크린샷")

에 `SKPaint` 에서 개체를 만들는 `using` 블록입니다. 많은 SkiaSharp 클래스 처럼 `SKPaint` 에서 파생 되 `SKObject`에서 파생 되는 `SKNativeObject`를 구현 하는 합니다 [ `IDisposable` ](xref:System.IDisposable) 인터페이스입니다. `SKPaint` 재정의 `Dispose` 관리 되지 않는 리소스를 해제 하는 방법입니다.

 배치 `SKPaint` 에 `using` 되도록 블록 `Dispose` 이러한 관리 되지 않는 리소스를 해제 하는 블록의 끝에 호출 됩니다. 이런 그래도 메모리에서 사용 하는 경우는 `SKPaint` 개체가 해제 되며 더 순차적 방식으로 메모리를 해제 좀 대응력 유용.NET 가비지 수집기에 의해 하지만 애니메이션 코드에서입니다.

 이 경우 더 나은 솔루션을 두 개를 만드는 것 `SKPaint` 필드로 저장할 개체입니다.

즉 합니다 **원 확장** 애니메이션 않습니다. 합니다 [ `ExpandingCirclesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) 클래스를 포함 하 여 여러 필드를 정의 하 여 시작을 `SKPaint` 개체:

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

이 프로그램에는 Xamarin.Forms에 따라 애니메이션 하는 다른 방법을 사용 하 여 `Device.StartTimer`입니다. 합니다 `t` 필드의 애니메이션이 적용 되어 0에서 1 모든 `cycleTime` 시간 (밀리초):

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

`PaintSurface` 처리기 애니메이션이 적용 된 반지름을 사용 하 여 5 동심원을 그립니다. 경우는 `baseRadius` 으로 다음 100으로 변수는 계산 `t` 0에서 1, 100, 100-200, 200에서 300, 300 ~ 400 및 500 400 0에서 5 원 증가의 반지름에 애니메이션 효과가 적용 됩니다. 원 중 대부분은 `strokeWidth` 50 하지만 첫 번째 원형는 `strokeWidth` 0에서 50으로 애니메이션 효과 줍니다. 원 중 대부분의 경우 색은 파란색이 고 하지만 마지막 원의 색 애니메이션 효과가 적용 됩니다 파란색에서 투명 한:

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

결과 동일한 경우에는 이미지에 표시 되는지 `t` 이 0 인 경우와 `t` 1 이면 및 원 계속 영원히 확장 것 같습니다.

[![](animation-images/expandingcircles-small.png "삼중 원 확장 페이지 스크린샷")](animation-images/expandingcircles-large.png#lightbox "삼중 원 확장 페이지 스크린샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
