---
title: SkiaSharp 노이즈 및 작성
description: Perlin 노이즈 셰이더를 생성 하 고 다른 셰이더를 사용 하 여 결합 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 90C2D00A-2876-43EA-A836-538C3318CF93
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: dea7f5e51a864922d56f7b65d19b21a889cbc650
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656161"
---
# <a name="skiasharp-noise-and-composing"></a>SkiaSharp 노이즈 및 작성

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

간단한 벡터 그래픽 자연스럽 경향이 있습니다. 직선, 부드러운 곡선 및 단색 실제 개체의 결함 유사 하지 않습니다. 1982 영화에 대 한 컴퓨터에서 생성 된 그래픽 작업 하는 동안 _트 론에_, 컴퓨터 과학자 박 단은 Perlin 임의의 프로세스 보다 현실적인 질감 이러한 이미지를 제공 하는 데는 알고리즘을 개발을 시작 합니다. 1997 년 박 단은 Perlin Academy 상을 기술 도전 과제에 대 한이 했습니다. 자신의 업무에 Perlin 노이즈 이라고 하 고 SkiaSharp 지원 됩니다. 예를 들면 다음과 같습니다.

![Perlin 노이즈 샘플](noise-images/NoiseSample.png "Perlin 노이즈 샘플")

알 수 있듯이 각 픽셀을 임의의 색 값이 아닙니다. 임의 셰이프 픽셀 연속성 픽셀에서 발생합니다.

Perlin 노이즈 Skia 지원은 CSS 및 SVG에 대 한 W3C 사양을 기반으로 합니다. 섹션 8.20 [ **필터 효과 모듈 수준 1** ](http://www.w3.org/TR/filter-effects-1/#feTurbulenceElement) C 코드에서 기본 Perlin 노이즈 알고리즘을 포함 합니다.

## <a name="exploring-perlin-noise"></a>Perlin 노이즈를 탐색합니다.

합니다 [ `SKShader` ](xref:SkiaSharp.SKShader) Perlin 노이즈를 생성 하기 위한 두 개의 다른 정적 메서드를 정의 하는 클래스: [ `CreatePerlinNoiseFractalNoise` ](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise*) 고 [ `CreatePerlinNoiseTurbulence` ](xref:SkiaSharp.SKShader.CreatePerlinNoiseTurbulence*)합니다. 매개 변수는 동일 합니다.

```csharp
public static SkiaSharp CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);

public static SkiaSharp.SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);
```

두 메서드 모두 추가 사용 하 여 오버 로드 된 버전에도 존재 `SKPointI` 매개 변수입니다. 섹션 [ **바둑판식 배열 Perlin 노이즈** ](#tiling-perlin-noise) 이러한 오버 로드에 설명 합니다.

두 `baseFrequency` 인수는 양수 값으로 0에서 1 사이의 SkiaSharp 설명서에 정의 되어 있지만 더 높은 값으로 설정할 수 있습니다. 값이 높을수록, 가로 및 세로 방향으로 임의 이미지에는 큰 변경 합니다.

`numOctaves` 값은 정수 1 이상입니다. 알고리즘의 반복 요소를 관련이 있습니다. 각 추가 octave 이므로 이전 octave의 절반 octave 값이 높은 영향 감소 효과 제공 합니다.

`seed` 매개 변수는 난수 생성기의 시작 지점입니다. 부동 소수점 값으로 지정 하지만 비율을 사용 하 고 0은 1과 동일 전에 잘립니다.

**Perlin 노이즈** 페이지에 [ **SkiaSharpFormsDemos**)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플에서는 다양 한 값을 사용 하 여 실험을 `baseFrequency` 및 `numOctaves` 인수. XAML 파일을 다음과 같습니다.

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

에서는 두 `Slider` 두 뷰 `baseFrequency` 인수입니다. 낮은 값의 범위를 확장 하려면이 슬라이더는 로그입니다. 코드 숨김 파일에 대 한 인수를 계산 합니다 `SKShader`의 거듭제곱의 메서드를 `Slider` 값입니다. `Label` 뷰 계산 된 값을 표시 합니다.

```csharp
float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);
```

`Slider` 0.001에 해당 하는 값이 1 `Slider` 0.01에 해당 하는 os 2 값을 `Slider` 0.1에 해당 하는 3의 값 및 `Slider` 1에 해당 하는 값이 4입니다.

해당 코드를 포함 하는 코드 숨김 파일을 다음과 같습니다.

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

장치 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에서 실행 중인 프로그램이 다음과 같습니다. 프랙탈 노이즈 캔버스의 위쪽 절반에 표시 됩니다. 분야의 난 기류 노이즈 다음과 같습니다. 아래쪽 절반

[![Perlin 노이즈](noise-images/PerlinNoise.png "Perlin 노이즈")](noise-images/PerlinNoise-Large.png#lightbox)

동일한 인수는 항상 왼쪽 위 모퉁이에서 시작 하는 동일한 패턴을 생성 합니다. 이 일관성은 UWP 창의 높이 너비를 조정 하면 분명 합니다. Windows 10 화면을 다시 그리면, 처럼 캔버스의 위쪽 절반에 있는 패턴과 동일 합니다.

의미 없는 패턴을 다양 한 수준의 투명도 통합합니다. 투명도 색을 설정 하는 경우 알 수는 `canvas.Clear()` 호출 합니다. 해당 색이 패턴에서 중요 합니다. 이 효과 섹션에 대해서도 배웁니다 [ **결합 여러 셰이더**](#combining-multiple-shaders)합니다.

이러한 Perlin 노이즈 패턴은 거의 단독으로 사용 됩니다. 종종 혼합 모드 및 이후 기사에서 설명 하는 색 필터에 종속 됩니다.

## <a name="tiling-perlin-noise"></a>Perlin 노이즈를 바둑판식으로 배열

두 정적 `SKShader` Perlin 노이즈를 만드는 메서드를 오버 로드 버전에도 존재 합니다. 합니다 [ `CreatePerlinNoiseFractalNoise` ](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI)) 하 고 [ `CreatePerlinNoiseTurbulence` ](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI)) 오버 로드에는 추가 `SKPointI` 매개 변수:

```csharp
public static SKShader CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);

public static SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);
```

합니다 [ `SKPointI` ](xref:SkiaSharp.SKPointI) 구조는 친숙 한 정수 버전 [ `SKPoint` ](xref:SkiaSharp.SKPoint) 구조입니다. `SKPointI` 정의 `X` 하 고 `Y` 형식의 속성 `int` 대신 `float`합니다.

이러한 메서드는 지정된 된 크기의 반복 패턴을 만듭니다. 각 타일의 오른쪽 가장자리를 왼쪽된 가장자리와 동일 하 고 위쪽 가장자리 아래쪽 가장자리와 같습니다. 이 특성에 설명 되어는 **Perlin 노이즈 바둑판식으로 배열** 페이지입니다. XAML 파일은 이전 예제와 유사 하지만 복사본만 `Stepper` 변경에 대 한 보기를 `seed` 인수:

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

코드 숨김 파일 타일 크기에 대 한 상수를 정의합니다. 합니다 `PaintSurface` 처리기는 해당 크기의 비트맵을 만듭니다 및 `SKCanvas` 해당 비트맵에 드로잉에 대 한 합니다. `SKShader.CreatePerlinNoiseTurbulence` 메서드 타일 크기를 사용 하 여 셰이더를 만듭니다. 이 셰이더는 비트맵에 그려집니다.

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

비트맵 만든 후에, 다른 `SKPaint` 개체는 호출 하 여 비트맵을 바둑판식으로 배열 된 패턴을 만드는 데 `SKShader.CreateBitmap`합니다. 두 인수를 확인할 수 있습니다 `SKShaderTileMode.Repeat`:

```csharp
paint.Shader = SKShader.CreateBitmap(bitmap,
                                     SKShaderTileMode.Repeat,
                                     SKShaderTileMode.Repeat);
```

이 셰이더는 캔버스를 포함 하는 데 사용 됩니다. 마지막으로, 다른 `SKPaint` 개체 원래 비트맵의 크기를 표시 하는 사각형을 그리는 데 사용 됩니다.

만 `seed` 매개 변수는 사용자 인터페이스에서 선택할 수 있습니다. 하는 경우 동일한 `seed` 패턴은 각 플랫폼에서 사용 됩니다, 동일한 패턴이 표시 됩니다. 다른 `seed` 값으로 인해 다른 패턴이 있습니다.

[![Perlin 노이즈를 바둑판식으로 배열](noise-images/TiledPerlinNoise.png "Perlin 노이즈를 바둑판식으로 배열")](noise-images/TiledPerlinNoise-Large.png#lightbox)

왼쪽 위 모퉁이에 있는 200 픽셀 사각형 패턴과 다른 타일로 원활 하 게 이동합니다.

## <a name="combining-multiple-shaders"></a>여러 셰이더를 결합합니다.

합니다 `SKShader` 클래스에 포함을 [ `CreateColor` ](xref:SkiaSharp.SKShader.CreateColor*) 지정 단색 셰이더를 만드는 메서드를 합니다. 해당 색 설정 하기만 하면이 셰이더 자체로 유용 이므로 `Color` 의 속성을 `SKPaint` 개체 및 설정는 `Shader` 속성을 null.

이 `CreateColor` 메서드가 다른 메서드를 유용 하 게 하는 `SKShader` 정의 합니다. 이 메서드는 [ `CreateCompose` ](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader)), 두 셰이더를 결합 합니다. 구문은 다음과 같습니다.

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader);
```

합니다 `srcShader` 위쪽에 효과적으로 그려집니다 (원본 셰이더)는 `dstShader` (대상 셰이더). 원본 셰이더 단색 또는 그라데이션 투명성이 적용 되지 않은 경우 대상 셰이더 완전히 가릴 수 있습니다.

Perlin 노이즈 셰이더 투명도 포함합니다. 해당 셰이더 소스 이면 대상 셰이더 투명 영역을 통해 표시 됩니다.

합니다 **구성 Perlin 노이즈** 페이지에는 첫 번째 거의 동일한 XAML 파일이 **Perlin 노이즈** 페이지입니다. 코드 숨김 파일은 유사 합니다. 하지만 원래 **Perlin 노이즈** 집합 페이지는 `Shader` 의 속성 `SKPaint` 정적에서 반환 된 셰이더에 `CreatePerlinNoiseFractalNoise` 및 `CreatePerlinNoiseTurbulence` 메서드. 이렇게 **Perlin 노이즈 구성** 호출 페이지 `CreateCompose` 조합 셰이더에 대 한 합니다. 대상이 사용 하 여 만든 파란색 단색 셰이더 `CreateColor`합니다. 원본이 Perlin 노이즈 셰이더:

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

프랙탈 노이즈 셰이더가 위쪽; 분야의 난 기류 셰이더가 맨 아래에서:

[![Perlin 노이즈를 구성](noise-images/ComposedPerlinNoise.png "Perlin 노이즈를 구성 합니다.")](noise-images/ComposedPerlinNoise-Large.png#lightbox)

얼마나 파랑 이러한 셰이더 개가으로 표시 하는 것은 **Perlin 노이즈** 페이지입니다. 차이점은 노이즈 셰이더에서 투명도를 보여 줍니다.

오버 로드 이기도 합니다 [ `CreateCompose` ](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader,SkiaSharp.SKBlendMode)) 메서드:

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader, SKBlendMode blendMode);
```

가 멤버인 마지막 매개 변수를 `SKBlendMode` 열거형에서 다음 일련의 문서에서 설명 하는 29 멤버로 구성 된 열거형 [ **SkiaSharp 합치기 및 혼합 모드**](../blend-modes/index.md)합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
