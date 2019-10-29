---
title: SkiaSharp 노이즈 및 작성
description: Perlin 노이즈 셰이더를 생성 하 고 다른 셰이더와 결합 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 90C2D00A-2876-43EA-A836-538C3318CF93
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: c1e500936b89f2ec8dc17279a7ed878dc7f5cbb3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029433"
---
# <a name="skiasharp-noise-and-composing"></a>SkiaSharp 노이즈 및 작성

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

간단한 벡터 그래픽은 자연스럽 게 보입니다. 직선, 부드러운 곡선 및 단색은 실제 개체의 결함와 유사 하지 않습니다. 1982 영화 _Tron_컴퓨터에서 생성 된 그래픽에서 작업 하는 동안 Computer 과학자 켄은 Perlin는 임의 프로세스를 사용 하 여 이러한 이미지를 보다 현실적인 질감으로 제공 하는 알고리즘 개발을 시작 했습니다. 1997에서 켄은 Perlin는 기술 성과를 달성 하기 위해 보병의 경력을가지고 있습니다. 해당 작업은 Perlin 소음 이라고 하며 SkiaSharp에서 지원 됩니다. 예를 들면 다음과 같습니다.

![Perlin 노이즈 샘플](noise-images/NoiseSample.png "Perlin 노이즈 샘플")

여기에서 볼 수 있듯이 각 픽셀은 임의의 색 값이 아닙니다. 픽셀에서 픽셀로의 연속성으로 임의의 모양이 생성 됩니다.

Perlin Ia에서의 노이즈 지원은 CSS 및 SVG 용 W3C 사양을 기반으로 합니다. [**필터 효과 모듈 수준 1**](https://www.w3.org/TR/filter-effects-1/#feTurbulenceElement) 의 섹션 8.20에는 C 코드의 기본 Perlin 노이즈 알고리즘이 포함 되어 있습니다.

## <a name="exploring-perlin-noise"></a>Perlin 노이즈 탐색

[`SKShader`](xref:SkiaSharp.SKShader) 클래스는 Perlin 노이즈를 생성 하는 두 가지 다른 정적 메서드인 [`CreatePerlinNoiseFractalNoise`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise*) 및 [`CreatePerlinNoiseTurbulence`](xref:SkiaSharp.SKShader.CreatePerlinNoiseTurbulence*)를 정의 합니다. 매개 변수는 동일 합니다.

```csharp
public static SkiaSharp CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);

public static SkiaSharp.SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);
```

두 메서드는 모두 추가 `SKPointI` 매개 변수를 사용 하 여 오버 로드 된 버전에도 있습니다. [**Perlin 노이즈**](#tiling-perlin-noise) 섹션에서는 이러한 오버 로드에 대해 설명 합니다.

두 개의 `baseFrequency` 인수는 0에서 1 사이의 SkiaSharp 설명서에 정의 된 양의 값 이지만 더 높은 값으로도 설정할 수 있습니다. 값이 높을수록 가로 및 세로 방향으로 임의 이미지의 변화가 커집니다.

`numOctaves` 값은 1 이상의 정수입니다. 알고리즘의 반복 요소와 관련이 있습니다. 각 추가 되며는 이전 되며의 절반에 해당 하는 효과를 적용 하므로 더 높은 되며 값으로 효과가 줄어듭니다.

`seed` 매개 변수는 난수 생성기의 시작 지점입니다. 는 부동 소수점 값으로 지정 되기는 하지만 사용 하기 전에 소수 자릿수가 잘리고 0은 1과 같습니다.

[ **SkiaSharpFormsDemos**)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **Perlin 노이즈** 페이지를 사용 하면 다양 한 `baseFrequency` 값과 `numOctaves` 인수를 시험해 볼 수 있습니다. XAML 파일은 다음과 같습니다.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.PerlinNoisePage"
             Title="Perlin Noise">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="baseFrequencyXSlider"
                Maximum="4"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="baseFrequencyXText"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="baseFrequencyYSlider"
                Maximum="4"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="baseFrequencyYText"
               HorizontalTextAlignment="Center" />

        <StackLayout Orientation="Horizontal"
                     HorizontalOptions="Center"
                     Margin="10">

            <Label Text="{Binding Source={x:Reference octavesStepper},
                                  Path=Value,
                                  StringFormat='Number of Octaves: {0:F0}'}"
                   VerticalOptions="Center" />

            <Stepper x:Name="octavesStepper"
                     Minimum="1"
                     ValueChanged="OnStepperValueChanged" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

두 개의 `baseFrequency` 인수에 대해 두 개의 `Slider` 뷰를 사용 합니다. 낮은 값의 범위를 확장 하기 위해 슬라이더는 로그입니다. 코드 숨김이 파일은 `Slider` 값의 거듭제곱에서 `SKShader`메서드에 대 한 인수를 계산 합니다. `Label` 뷰는 계산 된 값을 표시 합니다.

```csharp
float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);
```

`Slider` 값 1은 0.001에 해당 하 고, `Slider` 값 os 2는 0.01에 해당 하 고, 3의 `Slider` 값은 0.1에 해당 하며, 4의 `Slider` 값은 1에 해당 합니다.

다음은 해당 코드를 포함 하는 코드를 포함 하는 파일입니다.

```csharp
public partial class PerlinNoisePage : ContentPage
{
    public PerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get values from sliders and stepper
        float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
        baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

        float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
        baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);

        int numOctaves = (int)octavesStepper.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader =
                SKShader.CreatePerlinNoiseFractalNoise(baseFreqX,
                                                       baseFreqY,
                                                       numOctaves,
                                                       0);

            SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
            canvas.DrawRect(rect, paint);

            paint.Shader =
                SKShader.CreatePerlinNoiseTurbulence(baseFreqX,
                                                     baseFreqY,
                                                     numOctaves,
                                                     0);

            rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

다음은 iOS, Android 및 유니버설 Windows 플랫폼 (UWP) 장치에서 실행 되는 프로그램입니다. 프랙탈 소음은 캔버스의 위쪽 절반에 표시 됩니다. Turbulence 소음은 아래쪽 절반에 있습니다.

[![Perlin 노이즈](noise-images/PerlinNoise.png "Perlin 노이즈")](noise-images/PerlinNoise-Large.png#lightbox)

동일한 인수는 항상 왼쪽 위 모퉁이에서 시작 하는 동일한 패턴을 생성 합니다. 이 일관성은 UWP 창의 너비와 높이를 조정할 때 명확 합니다. Windows 10에서 화면을 다시 그리면 캔버스의 위쪽 절반에 있는 패턴이 동일 하 게 유지 됩니다.

노이즈 패턴은 다양 한 수준의 투명도를 통합 합니다. `canvas.Clear()` 호출에서 색을 설정 하면 투명도가 명확 하 게 표시 됩니다. 이 색은 패턴에서 강조 됩니다. [**여러 셰이더 결합**](#combining-multiple-shaders)단원에도이 효과가 표시 됩니다.

이러한 Perlin 노이즈 패턴은 자체적으로 거의 사용 되지 않습니다. 일반적으로이는 나중에 설명 하는 문서에서 설명 하는 혼합 모드 및 색 필터입니다.

## <a name="tiling-perlin-noise"></a>바둑판식 배열 Perlin 노이즈

Perlin 노이즈를 만들기 위한 두 개의 정적 `SKShader` 메서드는 오버 로드 버전에도 있습니다. [`CreatePerlinNoiseFractalNoise`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI)) 및 [`CreatePerlinNoiseTurbulence`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI)) 오버 로드에는 추가 `SKPointI` 매개 변수가 있습니다.

```csharp
public static SKShader CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);

public static SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);
```

[`SKPointI`](xref:SkiaSharp.SKPointI) 구조체는 친숙 한 [`SKPoint`](xref:SkiaSharp.SKPoint) 구조의 정수 버전입니다. `SKPointI`는 `float`대신 `int` 형식의 `X` 및 `Y` 속성을 정의 합니다.

이러한 메서드는 지정 된 크기의 반복 패턴을 만듭니다. 각 타일에서 오른쪽 가장자리는 왼쪽 가장자리와 같으며 위쪽 가장자리는 아래쪽 가장자리와 동일 합니다. 이 특성은 **바둑판식 Perlin 노이즈** 페이지에서 보여 줍니다. XAML 파일은 이전 샘플과 유사 하지만 `seed` 인수를 변경 하는 `Stepper` 보기만 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.TiledPerlinNoisePage"
             Title="Tiled Perlin Noise">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <StackLayout Orientation="Horizontal"
                     HorizontalOptions="Center"
                     Margin="10">

            <Label Text="{Binding Source={x:Reference seedStepper},
                                  Path=Value,
                                  StringFormat='Seed: {0:F0}'}"
                   VerticalOptions="Center" />

            <Stepper x:Name="seedStepper"
                     Minimum="1"
                     ValueChanged="OnStepperValueChanged" />

        </StackLayout>
    </StackLayout>
</ContentPage>
```

코드 숨김이 파일은 타일 크기에 대 한 상수를 정의 합니다. `PaintSurface` 처리기는 해당 크기의 비트맵을 만들고 해당 비트맵으로 그리기 위한 `SKCanvas` 합니다. `SKShader.CreatePerlinNoiseTurbulence` 메서드는 해당 타일 크기를 사용 하 여 셰이더를 만듭니다. 이 셰이더는 비트맵에 그려집니다.

```csharp
public partial class TiledPerlinNoisePage : ContentPage
{
    const int TILE_SIZE = 200;

    public TiledPerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get seed value from stepper
        float seed = (float)seedStepper.Value;

        SKRect tileRect = new SKRect(0, 0, TILE_SIZE, TILE_SIZE);

        using (SKBitmap bitmap = new SKBitmap(TILE_SIZE, TILE_SIZE))
        {
            using (SKCanvas bitmapCanvas = new SKCanvas(bitmap))
            {
                bitmapCanvas.Clear();

                // Draw tiled turbulence noise on bitmap
                using (SKPaint paint = new SKPaint())
                {
                    paint.Shader = SKShader.CreatePerlinNoiseTurbulence(
                                        0.02f, 0.02f, 1, seed,
                                        new SKPointI(TILE_SIZE, TILE_SIZE));

                    bitmapCanvas.DrawRect(tileRect, paint);
                }
            }

            // Draw tiled bitmap shader on canvas
            using (SKPaint paint = new SKPaint())
            {
                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat);
                canvas.DrawRect(info.Rect, paint);
            }

            // Draw rectangle showing tile
            using (SKPaint paint = new SKPaint())
            {
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                paint.StrokeWidth = 2;

                canvas.DrawRect(tileRect, paint);
            }
        }
    }
}
```

비트맵을 만든 후에는 `SKShader.CreateBitmap`를 호출 하 여 바둑판식 비트맵 패턴을 만드는 데 다른 `SKPaint` 개체를 사용 합니다. `SKShaderTileMode.Repeat`의 두 인수를 확인 합니다.

```csharp
paint.Shader = SKShader.CreateBitmap(bitmap,
                                     SKShaderTileMode.Repeat,
                                     SKShaderTileMode.Repeat);
```

이 셰이더는 캔버스를 포함 하는 데 사용 됩니다. 마지막으로, 다른 `SKPaint` 개체를 사용 하 여 원래 비트맵의 크기를 표시 하는 사각형을 스트로크 합니다.

`seed` 매개 변수만 사용자 인터페이스에서 선택할 수 있습니다. 각 플랫폼에서 동일한 `seed` 패턴이 사용 되는 경우 동일한 패턴을 표시 합니다. 다른 `seed` 값으로 인해 다음과 같은 패턴이 있습니다.

[![바둑판식으로 Perlin 노이즈](noise-images/TiledPerlinNoise.png "바둑판식으로 Perlin 노이즈")](noise-images/TiledPerlinNoise-Large.png#lightbox)

왼쪽 위 모퉁이에 있는 200 픽셀 사각형 패턴은 다른 타일로 원활 하 게 흐릅니다.

## <a name="combining-multiple-shaders"></a>여러 셰이더 결합

`SKShader` 클래스에는 지정 된 단색으로 셰이더를 만드는 [`CreateColor`](xref:SkiaSharp.SKShader.CreateColor*) 메서드가 포함 되어 있습니다. 이 셰이더는 `SKPaint` 개체의 `Color` 속성으로 간단히 설정 하 고 `Shader` 속성을 null로 설정할 수 있기 때문에 자체에서 그다지 유용 하지 않습니다.

이 `CreateColor` 메서드는 `SKShader` 정의 하는 다른 메서드에서 유용 합니다. 이 메서드는 두 셰이더를 결합 하는 [`CreateCompose`](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader))됩니다. 구문은 다음과 같습니다.

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader);
```

`srcShader` (원본 셰이더)는 실제로 `dstShader` (대상 셰이더) 위에 그려집니다. 원본 셰이더가 투명 한 색 이거나 투명 하지 않은 그라데이션 인 경우 대상 셰이더가 완전히 가려집니다.

Perlin 노이즈 셰이더는 투명도를 포함 합니다. 해당 셰이더가 원본인 경우 대상 셰이더는 투명 영역을 통해 표시 됩니다.

**구성 된 Perlin 노이즈** 페이지에는 첫 번째 **Perlin 노이즈** 페이지와 거의 동일한 XAML 파일이 있습니다. 코드 숨김이 비슷한 파일도 있습니다. 그러나 원래 **Perlin 노이즈** 페이지는 `SKPaint`의 `Shader` 속성을 정적 `CreatePerlinNoiseFractalNoise` 및 `CreatePerlinNoiseTurbulence` 메서드에서 반환 된 셰이더에 설정 합니다. 구성 된이 **Perlin 노이즈** 페이지는 조합 셰이더에 대 한 `CreateCompose`를 호출 합니다. 대상은 `CreateColor`를 사용 하 여 만든 단색 파란색 셰이더입니다. 원본은 Perlin 노이즈 셰이더입니다.

```csharp
public partial class ComposedPerlinNoisePage : ContentPage
{
    public ComposedPerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get values from sliders and stepper
        float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
        baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

        float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
        baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);

        int numOctaves = (int)octavesStepper.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateCompose(
                SKShader.CreateColor(SKColors.Blue),
                SKShader.CreatePerlinNoiseFractalNoise(baseFreqX,
                                                       baseFreqY,
                                                       numOctaves,
                                                       0));

            SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
            canvas.DrawRect(rect, paint);

            paint.Shader = SKShader.CreateCompose(
                SKShader.CreateColor(SKColors.Blue),
                SKShader.CreatePerlinNoiseTurbulence(baseFreqX,
                                                     baseFreqY,
                                                     numOctaves,
                                                     0));

            rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

프랙탈 노이즈 셰이더가 위쪽에 있습니다. turbulence 셰이더는 아래쪽에 있습니다.

[![구성 된 Perlin 노이즈](noise-images/ComposedPerlinNoise.png "구성 된 Perlin 노이즈")](noise-images/ComposedPerlinNoise-Large.png#lightbox)

이러한 셰이더가 **Perlin 노이즈** 페이지에 표시 되는 것 보다 bluer 정도를 확인 합니다. 차이는 노이즈 셰이더의 투명도의 양을 보여 줍니다.

[`CreateCompose`](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader,SkiaSharp.SKBlendMode)) 메서드의 오버 로드도 있습니다.

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader, SKBlendMode blendMode);
```

최종 매개 변수는 `SKBlendMode` 열거형의 멤버 이며, [**SkiaSharp 합성 모드와 blend 모드**](../blend-modes/index.md)의 다음 문서 시리즈에서 설명 하는 29 개 멤버가 포함 된 열거형입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
