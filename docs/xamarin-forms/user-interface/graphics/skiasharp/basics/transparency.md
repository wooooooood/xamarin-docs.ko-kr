---
title: SkiaSharp 투명도
description: 투명도 사용 하 여 단일 장면에서 여러 개체를 결합 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B62F9487-C30E-4C63-BAB1-4C091FF50378
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 74335de66e74f6adc7c9488a1b78c31d36d03f14
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759414"
---
# <a name="skiasharp-transparency"></a>SkiaSharp 투명도

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

지금까지 살펴본 대로 [ `SKPaint` ](xref:SkiaSharp.SKPaint) 클래스를 포함 한 [ `Color` ](xref:SkiaSharp.SKPaint.Color) 형식의 속성 [ `SKColor` ](xref:SkiaSharp.SKColor). `SKColor` 알파 채널을 모두 사용 하 여 색을 포함 한 `SKColor` 값 부분적으로 투명 하 게 될 수 있습니다. 

몇 가지 투명도에 설명 된 합니다 [ **SkiaSharp의 기본적인 애니메이션** ](animation.md) 문서. 이 문서에서는 단일 장면 라고도 하는 기술에서에서 여러 개체를 결합 하는 투명도 다소 더 깊은 됩니다 _혼합_합니다. 혼합 기술 고급의 문서에 설명 되어는 [ **SkiaSharp 셰이더** ](../effects/shaders/index.md) 섹션입니다.

4-매개 변수를 사용 하 여 색을 처음 만들 때 투명도 수준을 설정할 수 있습니다 [ `SKColor` ](xref:SkiaSharp.SKColor.%23ctor(System.Byte,System.Byte,System.Byte,System.Byte)) 생성자:

```csharp
SKColor (byte red, byte green, byte blue, byte alpha);
```

알파 값이 0은 완전히 투명 이며 0xFF의 알파 값을 완전히 불투명 합니다. 이러한 두 가지 극단적인 사이의 값 부분적으로 투명 한 색을 만듭니다.

또한 `SKColor` 정의 유용한 [ `WithAlpha` ](xref:SkiaSharp.SKColor.WithAlpha*) 알파 수준이 지정된 하지만 기존 색에서 색을 만드는 메서드:

```csharp
SKColor halfTransparentBlue = SKColors.Blue.WithAlpha(0x80);
```

부분적으로 투명 한 텍스트의 사용에 설명 되어는 **코드 보다 코드** 페이지에 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플입니다. 이 페이지 페이드 두 개의 텍스트 문자열의 시작 및 종료에서 투명도 통합 하 여는 `SKColor` 값:

```csharp
public class CodeMoreCodePage : ContentPage
{
    SKCanvasView canvasView;
    bool isAnimating;
    Stopwatch stopwatch = new Stopwatch();
    double transparency;

    public CodeMoreCodePage ()
    {
        Title = "Code More Code";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 5;     // seconds
        double progress = stopwatch.Elapsed.TotalSeconds % duration / duration;
        transparency = 0.5 * (1 + Math.Sin(progress * 2 * Math.PI));
        canvasView.InvalidateSurface();

        return isAnimating;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        const string TEXT1 = "CODE";
        const string TEXT2 = "MORE";

        using (SKPaint paint = new SKPaint())
        {
            // Set text width to fit in width of canvas
            paint.TextSize = 100;
            float textWidth = paint.MeasureText(TEXT1);
            paint.TextSize *= 0.9f * info.Width / textWidth;

            // Center first text string
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT1, ref textBounds);

            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * (1 - transparency)));
            canvas.DrawText(TEXT1, xText, yText, paint);

            // Center second text string
            textBounds = new SKRect();
            paint.MeasureText(TEXT2, ref textBounds);

            xText = info.Width / 2 - textBounds.MidX;
            yText = info.Height / 2 - textBounds.MidY;

            paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * transparency));
            canvas.DrawText(TEXT2, xText, yText, paint);
        }
    }
}
```

`transparency` 필드는 0에서 1까지 다양할 진폭이 리듬에서 다시을 애니메이션 효과가 적용 됩니다. 첫 번째 텍스트 문자열을 빼서 계산 합니다. 알파 값을 사용 하 여 표시 됩니다는 `transparency` 1의 값:

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * (1 - transparency)));
```

합니다 [ `WithAlpha` ](xref:SkiaSharp.SKColor.WithAlpha*) 같습니다. 기존 색의 알파 구성 요소를 설정 하는 메서드 `SKColors.Blue`합니다. 두 번째 텍스트 문자열에서 계산 된 알파 값을 사용 하 여 `transparency` 값 자체:

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * transparency));
```

애니메이션 두 단어, 긴급 "자세히 code" 사용자 (또는 "더 많은 코드"를 요청 하는 것)이 번갈아:

[![더 많은 코드를 코드](transparency-images/CodeMoreCode.png "더 많은 코드를 코드")](transparency-images/CodeMoreCode-Large.png#lightbox)

에 대 한 이전 문서에서 [ **SkiaSharp의 비트맵 기본 사항**](bitmaps.md), 중 하나를 사용 하 여 비트맵을 표시 하는 방법을 살펴보았습니다 합니다 [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap*) 의 메서드 `SKCanvas`합니다. 모든는 `DrawBitmap` 메서드로 `SKPaint` 마지막 매개 변수로 개체입니다. 기본적으로이 매개 변수 설정 `null` 및 무시할 수 있습니다. 

설정할 수 있습니다 합니다 `Color` 이 `SKPaint` 일정 수준의 투명도 사용 하 여 비트맵을 표시 하는 개체입니다. 에 대 한 투명도 수준을 설정 합니다 `Color` 의 속성 `SKPaint` 비트맵을 페이드 인 / 체크 아웃 하거나 다른 하나의 비트맵 주만에 수 있습니다. 

비트맵 투명도에 설명 되어는 **비트맵 주만에** 페이지입니다. XAML 파일은는 `SKCanvasView` 및 `Slider`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.BitmapDissolvePage"
             Title="Bitmap Dissolve">
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="progressSlider"
                Margin="10"
                ValueChanged="OnSliderValueChanged" />
    </StackLayout>
</ContentPage>
```

코드 숨김 파일을 두 비트맵 리소스를 로드합니다. 이러한 비트맵은 크기가 동일 하지 않습니다 하지만 동일한 가로 세로 비율:

```csharp
public partial class BitmapDissolvePage : ContentPage
{
    SKBitmap bitmap1;
    SKBitmap bitmap2;

    public BitmapDissolvePage()
    {
        InitializeComponent();

        // Load two bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(
                                "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg"))
        {
            bitmap1 = SKBitmap.Decode(stream);
        }
        using (Stream stream = assembly.GetManifestResourceStream(
                                "SkiaSharpFormsDemos.Media.FacePalm.jpg"))
        {
            bitmap2 = SKBitmap.Decode(stream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find rectangle to fit bitmap
        float scale = Math.Min((float)info.Width / bitmap1.Width,
                                (float)info.Height / bitmap1.Height);
        SKRect rect = SKRect.Create(scale * bitmap1.Width,
                                    scale * bitmap1.Height);
        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Get progress value from Slider
        float progress = (float)progressSlider.Value;

        // Display two bitmaps with transparency
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = paint.Color.WithAlpha((byte)(0xFF * (1 - progress)));
            canvas.DrawBitmap(bitmap1, rect, paint);

            paint.Color = paint.Color.WithAlpha((byte)(0xFF * progress));
            canvas.DrawBitmap(bitmap2, rect, paint);
        }
    }
}
```

`Color` 속성의는 `SKPaint` 개체 두 비트맵에 대 한 두 개의 보완 알파 수준으로 설정 됩니다. 사용 하는 경우 `SKPaint` 비트맵을 사용 하 여 어떤 나머지는 중요 하지 않습니다는 `Color` 값이 있습니다. 가장 중요는 알파 채널입니다. 여기에서 코드를 호출 합니다 `WithAlpha` 기본값인 메서드는 `Color` 속성입니다.

이동 된 `Slider` 하나의 비트맵 즉 사이 디졸브:

[![디졸브 비트맵](transparency-images/BitmapDissolve.png "디졸브 비트맵")](transparency-images/BitmapDissolve-Large.png#lightbox)

지난 몇 가지 문서에서는 텍스트 "," 원 "," 줄임표 "," 둥근된 사각형 "및" 비트맵을 그릴 SkiaSharp 사용 하는 방법을 살펴보았습니다. 다음 단계 [SkiaSharp 선 및 경로](../paths/index.md) 그래픽 경로에 연결 된 선을 그리는 방법을 learn 됩니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
