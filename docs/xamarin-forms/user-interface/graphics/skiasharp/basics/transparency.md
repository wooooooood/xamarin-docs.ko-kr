---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 735aae1b9d94865bd34450861bd6c57b08c420c2
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84134721"
---
# <a name="skiasharp-transparency"></a>SkiaSharp 투명도

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

앞서 살펴본 것 처럼 클래스에는 [`SKPaint`](xref:SkiaSharp.SKPaint) [`Color`](xref:SkiaSharp.SKPaint.Color) 형식의 속성이 포함 됩니다 [`SKColor`](xref:SkiaSharp.SKColor) . `SKColor`알파 채널을 포함 하므로 값을 사용 하 여 색을 지정할 수 있는 모든 항목을 `SKColor` 부분적으로 투명 하 게 지정할 수 있습니다. 

[**SkiaSharp 문서의 기본 애니메이션**](animation.md) 에서 일부 투명도를 보여 주었습니다. 이 문서에서는 혼합을 통해 여러 개체를 단일 장면으로 결합 하 여 _혼합_으로 알려진 기법을 결합 하는 방법에 대해 자세히 설명 합니다. 고급 혼합 기술에 대해서는 [**SkiaSharp 셰이더**](../effects/shaders/index.md) 섹션의 문서에서 설명 합니다.

네 개의 매개 변수 생성자를 사용 하 여 처음으로 색을 만들 때 투명도 수준을 설정할 수 있습니다 [`SKColor`](xref:SkiaSharp.SKColor.%23ctor(System.Byte,System.Byte,System.Byte,System.Byte)) .

```csharp
SKColor (byte red, byte green, byte blue, byte alpha);
```

알파 값 0은 완전히 투명 하 고 알파 값 0xFF는 완전히 불투명 합니다. 이러한 두 극단 사이의 값은 부분적으로 투명 한 색을 만듭니다.

또한는 `SKColor` [`WithAlpha`](xref:SkiaSharp.SKColor.WithAlpha*) 기존 색에서 지정 된 알파 수준으로 새 색을 만드는 편리한 메서드를 정의 합니다.

```csharp
SKColor halfTransparentBlue = SKColors.Blue.WithAlpha(0x80);
```

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **코드 추가 코드** 페이지에서 부분적으로 투명 한 텍스트를 사용 하는 방법을 보여 줍니다. 이 페이지는 값에 투명도를 통합 하 여의 두 텍스트 문자열을 페이드 아웃 합니다 `SKColor` .

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

`transparency`사인 곡선 리듬에서 1과 1 사이의 차이가 있는 필드에 애니메이션 효과가 적용 됩니다. 첫 번째 텍스트 문자열은 1에서 값을 빼서 계산 된 알파 값이 표시 됩니다 `transparency` .

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * (1 - transparency)));
```

[`WithAlpha`](xref:SkiaSharp.SKColor.WithAlpha*)메서드는 기존 색에 알파 구성 요소를 설정 합니다. 여기서는 `SKColors.Blue` 입니다. 두 번째 텍스트 문자열은 값 자체에서 계산 된 알파 값을 사용 합니다 `transparency` .

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * transparency));
```

애니메이션은 두 단어 사이를 대체 하 여 사용자를 "코드 urging" (또는 "추가 코드" 요청) 합니다.

[![코드 추가 코드](transparency-images/CodeMoreCode.png "코드 추가 코드")](transparency-images/CodeMoreCode-Large.png#lightbox)

이전 문서 [**SkiaSharp의 비트맵 기본 사항**](bitmaps.md)에서는의 메서드 중 하나를 사용 하 여 비트맵을 표시 하는 방법을 살펴보았습니다 [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap*) `SKCanvas` . 모든 `DrawBitmap` 메서드에는 개체를 `SKPaint` 마지막 매개 변수로 포함 합니다. 기본적으로이 매개 변수는로 설정 되며 무시 해도 됩니다 `null` . 

또는 `Color` 이 개체의 속성을 설정 하 여 `SKPaint` 특정 수준의 투명도를 사용 하는 비트맵을 표시할 수 있습니다. 의 속성에서 투명도 수준을 설정 하면 `Color` 비트맵을 `SKPaint` 페이드 인 하거나 페이드 아웃할 수 있습니다. 

비트맵 투명도는 **비트맵 흩어 뿌리기** 페이지에서 보여 줍니다. XAML 파일은 `SKCanvasView` 및를 인스턴스화합니다 `Slider` .

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

코드 숨김이 파일은 두 개의 비트맵 리소스를 로드 합니다. 이러한 비트맵의 크기는 동일 하지 않지만 가로 세로 비율은 다음과 같습니다.

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

`Color`개체의 속성은 `SKPaint` 두 비트맵에 대해 두 개의 보조 알파 수준으로 설정 됩니다. 비트맵과 함께를 사용 하는 경우 `SKPaint` 값의 나머지가 무엇 인지는 중요 하지 않습니다 `Color` . 알파 채널은 모두 중요 합니다. 여기에서 코드는 단순히 `WithAlpha` 속성의 기본값에 대해 메서드를 호출 합니다 `Color` .

`Slider`한 비트맵과 다른 비트맵 간에 dissolves 이동:

[![비트맵 흩어 뿌리기](transparency-images/BitmapDissolve.png "비트맵 흩어 뿌리기")](transparency-images/BitmapDissolve-Large.png#lightbox)

지난 몇 가지 문서에서는 SkiaSharp를 사용 하 여 텍스트, 원, 타원, 둥근 사각형 및 비트맵을 그리는 방법에 대해 살펴보았습니다. 다음 단계는 그래픽 경로에서 연결 된 선을 그리는 방법을 배울 [SkiaSharp 선 및 경로](../paths/index.md) 입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
