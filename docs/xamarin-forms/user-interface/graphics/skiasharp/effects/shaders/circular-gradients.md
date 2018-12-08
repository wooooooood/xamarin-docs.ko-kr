---
title: SkiaSharp 순환 그라데이션
description: 다양 한 유형의 그라데이션 원으로 기반에 대해 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 400AE23A-6A0B-4FA8-BD6B-DE4146B04732
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: a17ddf438856600870c9bb3da60a5f4667128d57
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056047"
---
# <a name="the-skiasharp-circular-gradients"></a>SkiaSharp 순환 그라데이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

합니다 [ `SKShader` ](xref:SkiaSharp.SKShader) 클래스 네 가지 유형의 그라데이션 만드는 정적 메서드를 정의 합니다. 합니다 [ **SkiaSharp 선형 그라데이션** ](linear-gradient.md) 문서에 설명 합니다 [ `CreateLinearGradient` ](xref:SkiaSharp.SKShader.CreateLinearGradient*) 메서드. 이 문서에는 다른 세 가지 유형의 그라데이션 원에 기반한 모두 다룹니다.

합니다 [ `CreateRadialGradient` ](xref:SkiaSharp.SKShader.CreateRadialGradient*) 메서드 원의 중심에서 나오는 그라데이션을 만듭니다.

![방사형 그라데이션 샘플](circular-gradients-images/RadialGradientSample.png)

합니다 [ `CreateSweepGradient` ](xref:SkiaSharp.SKShader.CreateSweepGradient*) 메서드 원의 중심를 비우고 그라데이션을 만듭니다.

![스윕 그라데이션 샘플](circular-gradients-images/SweepGradientSample.png)

세 번째 그라데이션 유형을 매우 일반적인 아닙니다. 두 지점 원뿔 그라데이션 라고 하 고 정의한 합니다 [ `CreateTwoPointConicalGradient` ](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient*) 메서드. 그라데이션에서 다른 하나의 원을에서 확장 됩니다.

![원뿔 그라데이션 샘플](circular-gradients-images/ConicalGradientSample.png)

두 원은 크기가 다른 경우 그라데이션의 원뿔 형태를 사용 합니다.

이 문서에서는 이러한 그라데이션을 자세히 살펴봅니다.

## <a name="the-radial-gradient"></a>방사형 그라데이션

합니다 [ `CreateRadialGradient` ](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode)) 메서드는 다음 구문을 가집니다.

```csharp
public static SKShader CreateRadialGradient (SKPoint center, 
                                             Single radius, 
                                             SKColor[] colors, 
                                             Single[] colorPos, 
                                             SKShaderTileMode mode)
```

A [ `CreateRadialGradient` ](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) 오버 로드도 변환 행렬 매개 변수를 포함 합니다.

처음 두 인수 원과 반지름의 중심을 지정 합니다. 그라데이션의 해당 센터에서 시작 되 고 바깥쪽으로 연장 `radius` 픽셀입니다. 초과 되나요 `radius` 에 따라 달라 집니다 합니다 [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode) 인수입니다. 합니다 `colors` 매개 변수는 두 가지 이상의 색 (마찬가지로, 선형 그라데이션 메서드)의 배열 및 `colorPos` 은 0 ~ 1 범위에 정수 배열입니다. 이 정수는 함께 색의 상대 위치를 나타내는 `radius` 줄. 해당 인수를 설정할 수 있습니다 `null` 색 간격을 동일 하 게 하려면.

사용 하는 경우 `CreateRadialGradient` 원 채우기 그라데이션의 가운데 원의 반지름 그라데이션의 radius는 원의 중심을 설정할 수 있습니다. 이런 경우는 `SKShaderTileMode` 인수가 그라데이션의 렌더링에 영향을 주지 않습니다. 그라데이션의 여 채운 영역 그라데이션, 정의한 원 보다 크면 하지만 `SKShaderTileMode` 인수 원 밖 되나요에 큰 영향을 미칩니다.

미치는 `SKShaderMode` 에 설명 되어 합니다 **방사형 그라데이션** 페이지에 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 샘플. 이 페이지에 대 한 XAML 파일은는 `Picker` 의 세 멤버 중 하나를 선택할 수 있도록 합니다 `SKShaderTileMode` 열거형:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.RadialGradientPage"
             Title="Radial Gradient">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                Grid.Row="0"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="tileModePicker" 
                Grid.Row="1"
                Title="Shader Tile Mode" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKShaderTileMode}">
                    <x:Static Member="skia:SKShaderTileMode.Clamp" />
                    <x:Static Member="skia:SKShaderTileMode.Repeat" />
                    <x:Static Member="skia:SKShaderTileMode.Mirror" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</ContentPage>
```

코드 숨김 파일에 방사형 그라데이션 사용 하 여 전체 캔버스를 색입니다. 그라데이션의 가운데 캔버스의 가운데에 집합과 반지름은 100 픽셀로 설정 합니다. 그라데이션의 두 색을 검정 및 흰색 이루어져 있습니다.

```csharp
public partial class RadialGradientPage : ContentPage
{
    public RadialGradientPage ()
    {
        InitializeComponent ();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKShaderTileMode tileMode =
            (SKShaderTileMode)(tileModePicker.SelectedIndex == -1 ?
                                        0 : tileModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                new SKPoint(info.Rect.MidX, info.Rect.MidY),
                                100,
                                new SKColor[] { SKColors.Black, SKColors.White },
                                null,
                                tileMode);

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

이 코드는 센터에서 100 픽셀을 흰색으로 점차적으로 페이드 효과 가운데에 검은색을 사용 하 여 그라데이션을 만듭니다. 에 따라 달라 집니다 해당 radius 이외의 어떤 일이 생기는 `SKShaderTileMode` 인수:

[![방사형 그라데이션](circular-gradients-images/RadialGradient.png "방사형 그라데이션")](circular-gradients-images/RadialGradient-Large.png#lightbox)

세 가지 경우 모두 그라데이션의 캔버스를 채웁니다. 왼쪽에 있는 iOS 화면에서 반지름 초과 그라데이션의 마지막 색은 흰색는 계속 됩니다. 결과 `SKShaderTileMode.Clamp`합니다. Android 화면 효과 보여 줍니다. `SKShaderTileMode.Repeat`: 센터에서에서 100 픽셀 그라데이션의 시작 다시 첫 번째 색은 검정입니다. 그라데이션의는 반지름의 모든 100 픽셀을 반복합니다. 

유니버설 Windows 플랫폼 화면 오른쪽에 나와 있는에 어떻게 `SKShaderTileMode.Mirror` 대체 directions 그라데이션 하면 합니다. 첫 번째 그라데이션 방법은 검정 가운데에 100 픽셀의 반지름에 흰색입니다. 다음은 200 픽셀 반지름에 검정색으로 100 픽셀 radius에서 흰색 및 다음 그라데이션 다시 반전 됩니다.

방사형 그라데이션에서 두 개 이상의 색을 사용할 수 있습니다. 합니다 **Rainbow 호 그라데이션** 샘플 rainbow 및 빨간색으로 끝나는 색에 해당 하는 8 개의 색 배열과 8 개 위치 값의 배열을 만듭니다.

```csharp
public class RainbowArcGradientPage : ContentPage
{
    public RainbowArcGradientPage ()
    {
        Title = "Rainbow Arc Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            float rainbowWidth = Math.Min(info.Width, info.Height) / 4f;

            // Center of arc and gradient is lower-right corner
            SKPoint center = new SKPoint(info.Width, info.Height);

            // Find outer, inner, and middle radius
            float outerRadius = Math.Min(info.Width, info.Height);
            float innerRadius = outerRadius - rainbowWidth;
            float radius = outerRadius - rainbowWidth / 2;

            // Calculate the colors and positions
            SKColor[] colors = new SKColor[8];
            float[] positions = new float[8];

            for (int i = 0; i < colors.Length; i++)
            {
                colors[i] = SKColor.FromHsl(i * 360f / 7, 100, 50);
                positions[i] = (i + (7f - i) * innerRadius / outerRadius) / 7f;
            }

            // Create sweep gradient based on center and outer radius
            paint.Shader = SKShader.CreateRadialGradient(center, 
                                                         outerRadius, 
                                                         colors, 
                                                         positions, 
                                                         SKShaderTileMode.Clamp);
            // Draw a circle with a wide line
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = rainbowWidth;

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

최소 너비의 가정 하 고 캔버스의 높이 1000입니다. 즉는 `rainbowWidth` 값은 250입니다. 합니다 `outerRadius` 및 `innerRadius` 값 1000으로 설정 됩니다 및 750, 각각. 이러한 값을 계산 하는 데 사용 된 `positions` 배열, 1부터 0.75f 8 값 범위입니다. `radius` 원을 선 그리기에 대 한 값이 사용 됩니다. 875 의미 750 픽셀인 radius 사이의 1000 픽셀의 반지름을 250 픽셀 스트로크 너비를 확장 하는 값:

[![Rainbow 호 그라데이션](circular-gradients-images/RainbowArcGradient.png "Rainbow 호 그라데이션")](circular-gradients-images/RainbowArcGradient-Large.png#lightbox)

이 그라데이션을 사용 하 여 전체 캔버스를 채운 경우 내부 반경 내 빨간색 인지 표시 됩니다. 왜냐하면는 `positions` 배열의 0부터 시작 하지 않습니다. 첫 번째 색은 첫 번째 배열 값을 통해 0 오프셋에 사용 됩니다. 그라데이션의 외부 radius 초과 빨간색 이기도합니다. 결과는 `Clamp` 타일 모드입니다. 두꺼운 선 선 그리기에 대 한 그라데이션을 사용 되므로 이러한 빨간색 영역이 표시 되지 않습니다.

## <a name="radial-gradients-for-masking"></a>마스킹에 대 한 방사형 그라데이션

선형 그라데이션 같은 방사형 그라데이션 투명 하거나 부분적으로 투명 한 색을 통합할 수 있습니다. 호출 프로세스의이 기능은 유용 _마스킹_는 이미지의 다른 부분을 강조 하기 위해 이미지의 일부를 숨깁니다.

합니다 **방사형 그라데이션 마스크** 페이지 예를 보여 줍니다. 프로그램은 리소스 비트맵 중 하나를 로드 합니다. 합니다 `CENTER` 고 `RADIUS` 필드 비트맵 검사에서 결정 된 및 강조 표시 해야 하는 영역을 참조 합니다. `PaintSurface` 처리기 비트맵을 표시 하려면 사각형을 계산 하 여 시작 하 고 해당 사각형에 표시 합니다.

```csharp
public class RadialGradientMaskPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(RadialGradientMaskPage),
        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    static readonly SKPoint CENTER = new SKPoint(180, 300);
    static readonly float RADIUS = 120;

    public RadialGradientMaskPage ()
    {
        Title = "Radial Gradient Mask";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find rectangle to display bitmap
        float scale = Math.Min((float)info.Width / bitmap.Width,
                                (float)info.Height / bitmap.Height);

        SKRect rect = SKRect.Create(scale * bitmap.Width, scale * bitmap.Height);

        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Display bitmap in rectangle
        canvas.DrawBitmap(bitmap, rect);

        // Adjust center and radius for scaled and offset bitmap
        SKPoint center = new SKPoint(scale * CENTER.X + x,
                                     scale * CENTER.Y + y);
        float radius = scale * RADIUS;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                center,
                                radius,
                                new SKColor[] { SKColors.Transparent,
                                                SKColors.White },
                                new float[] { 0.6f, 1 },
                                SKShaderTileMode.Clamp);

            // Display rectangle using that gradient
            canvas.DrawRect(rect, paint);
        }
    }
}
```

비트맵을 그린 후 몇 가지 간단한 코드는 다음과 같이 변환 됩니다. `CENTER` 하 고 `RADIUS` 에 `center` 및 `radius`, 크기가 조정 되 고 표시 하기 위해 이동 된 비트맵에 강조 표시 된 영역을 참조 하는 합니다. 이러한 값은 해당 center 및 radius를 사용 하 여 방사형 그라데이션을 만드는 데 사용 됩니다. 에 두 색으로 시작 센터에서와 반지름에 대 한 첫 번째 60%에 대해 투명 합니다. 그라데이션의 다음 흰색으로 흐려 지:

[![방사형 그라데이션 마스크](circular-gradients-images/RadialGradientMask.png "방사형 그라데이션 마스크")](circular-gradients-images/RadialGradientMask-Large.png#lightbox)

이 방법은 비트맵을 마스크 하는 가장 좋은 방법은 아닙니다. 마스크에 대부분의 흰색을 캔버스의 배경과 일치 하도록 선택 된 색에이 문제가입니다. 백그라운드 다른 색 이면 &mdash; 또는 자체를 그라데이션 아마도 &mdash; 일치 하지 않습니다. 마스킹 더 나은 방법은 문서에 나와 [SkiaSharp Porter 임신 blend 모드](../blend-modes/porter-duff.md)합니다.

## <a name="radial-gradients-for-specular-highlights"></a>방사형 그라데이션 반사 강조 표시

광원은 둥근된 표면 발생 한 경우 여러 방향으로 빛을 반사 하 되지만 보는 사람의 눈으로 직접 바운스 빛의 일부입니다. 호출 화면에서 흰색 영역을 유사 항목의 모양을 종종 이렇게를 _반사 하이라이트_합니다.

3 차원 그래픽에서 반사 하이라이트 밝은 경로 및 음영을 결정 하는 알고리즘에서 일으키는 경우가 많습니다. 2 차원 그래픽에서 모양의 3D 표면에 제안에 반사 하이라이트 추가 되기도 합니다. 반사 하이라이트 round 빨간 공이 플랫 빨간색 원이 변환 수 있습니다.

합니다 **방사형 반사 강조 표시** 페이지 방사형 그라데이션을 사용 하 여 작업을 정확 하 게 수행 하 합니다. 합니다 `PaintSurface` 원 및 2에 대 한 반지름을 계산 하 여 처리기 하다 보면 지루함을 `SKPoint` 값 &mdash; 는 `center` 및 `offCenter` 원의 왼쪽 위 가장자리와 가운데 사이의 중간는:

```csharp
public class RadialSpecularHighlightPage : ContentPage
{
    public RadialSpecularHighlightPage()
    {
        Title = "Radial Specular Highlight";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        float radius = 0.4f * Math.Min(info.Width, info.Height);
        SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
        SKPoint offCenter = center - new SKPoint(radius / 2, radius / 2);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                offCenter,
                                radius / 2,
                                new SKColor[] { SKColors.White, SKColors.Red },
                                null,
                                SKShaderTileMode.Clamp);

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

합니다 `CreateRadialGradient` 호출에는 시작 하는 그라데이션을 만듭니다 `offCenter` 백서와 절반 반지름의 거리에 빨간색으로 끝나는 지점입니다. 다음과 같이 나타납니다.

[![방사형 반사 하이라이트](circular-gradients-images/RadialSpecularHighlight.png "방사형 반사 강조 표시")](circular-gradients-images/RadialSpecularHighlight-Large.png#lightbox)

이 그라데이션을 자세히 살펴보면 결함이 되는 것을 결정할 수 있습니다. 둥근된 화면에 맞게 좀 더 대칭적 것을 원치 않는 그라데이션의 특정 시점을 중심으로 하며 반사 하이라이트 아래 섹션에 표시 된 것을 선호할 수는 경우 [ **반사 하이라이트에 대 한 원뿔 그라데이션을**](#conical-gradients-for-specular-highlights)합니다.

## <a name="the-sweep-gradient"></a>스윕 그라데이션

합니다 [ `CreateSweepGradient` ](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[])) 메서드는 간단한 구문의 모든 그라데이션 만들기 메서드:

```csharp
public static SKShader CreateSweepGradient (SKPoint center, 
                                            SKColor[] colors, 
                                            Single[] colorPos)
```

Center에만, 색, 배열 및 색 위치 이며 그라데이션의는 중심점의 오른쪽에서 시작 하 고 중심 360도 시계 방향으로 추가 합니다. 이 없는 `SKShaderTileMode` 매개 변수입니다.

A [ `CreateSweepGradient` ](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKMatrix)) 행렬 변환 매개 변수를 사용 하 여 오버 로드도 제공 됩니다. 시작점을 변경 하는 그라데이션을 회전 변환을 적용할 수 있습니다. 또한 방향에서 시계 방향으로 시계 반대 방향으로 변경 하려면 크기 조정 변환을 적용할 수 있습니다.

합니다 **스윕 그라데이션** 페이지 스윕 그라데이션 원의 스트로크 너비 50 픽셀의 색을 사용 하 여:

[![그라데이션 스윕](circular-gradients-images/SweepGradient.png "그라데이션 비우기")](circular-gradients-images/SweepGradient-Large.png#lightbox)

`SweepGradientPage` 클래스 다른 색상 값을 사용 하 여 8 개의 색의 배열을 정의 합니다. 배열의 시작 하 고 스크린샷에서 맨 오른쪽에 나타나는 빨간색 (색상 값 0의 360)를 사용 하 여 종료를 확인 합니다.

```csharp
public class SweepGradientPage : ContentPage
{
    bool drawBackground;

    public SweepGradientPage ()
    {
        Title = "Sweep Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            drawBackground ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            // Define an array of rainbow colors
            SKColor[] colors = new SKColor[8];

            for (int i = 0; i < colors.Length; i++)
            {
                colors[i] = SKColor.FromHsl(i * 360f / 7, 100, 50);
            }

            SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);

            // Create sweep gradient based on center of canvas
            paint.Shader = SKShader.CreateSweepGradient(center, colors, null);

            // Draw a circle with a wide line
            const int strokeWidth = 50;
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = strokeWidth;

            float radius = (Math.Min(info.Width, info.Height) - strokeWidth) / 2;
            canvas.DrawCircle(center, radius, paint);

            if (drawBackground)
            {
                // Draw the gradient on the whole canvas
                paint.Style = SKPaintStyle.Fill;
                canvas.DrawRect(info.Rect, paint);
            }
        }
    }
}
```

프로그램 구현를 `TapGestureRecognizer` 수 있도록 하는 끝에 일부 코드를 `PaintSurface` 처리기입니다. 이 코드는 캔버스를 채우도록 동일한 그라데이션을 사용 합니다.

[![그라데이션 전체 스윕](circular-gradients-images/SweepGradientFull.png "그라데이션 전체 스윕")](circular-gradients-images/SweepGradientFull-Large.png#lightbox)

이러한 스크린샷은 그라데이션 채우기 어떤 영역으로 하는 방법을 보여 줍니다. 그라데이션의 시작와 동일한 색으로 종료 하지 않습니다, 경우 중심점의 오른쪽에 불연속성을 됩니다.

## <a name="the-two-point-conical-gradient"></a>두 지점 원뿔 그라데이션

합니다 [ `CreateTwoPointConicalGradient` ](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode)) 메서드는 다음 구문을 가집니다.

```csharp
public static SKShader CreateTwoPointConicalGradient (SKPoint startCenter, 
                                                      Single startRadius, 
                                                      SKPoint endCenter, 
                                                      Single endRadius, 
                                                      SKColor[] colors, 
                                                      Single[] colorPos, 
                                                      SKShaderTileMode mode)
```

중심점 및 라고 하는 두 개의 원의 반지름을 사용 하 여 매개 변수를 시작 합니다 _시작_ 원 및 _끝_ 원입니다. 나머지 세 개의 매개 변수는 동일 `CreateLinearGradient` 고 `CreateRadialGradient`입니다. A [ `CreateTwoPointConicalGradient` ](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) 오버 로드 된 매트릭스 변환을 포함 합니다.

그라데이션의 시작 원에서 시작 하 고 최종 원에서 끝납니다. `SKShaderTileMode` 매개 변수 두 원은 초과 발생 하는 상황을 제어 합니다. 두 지점 원뿔 기울기가 유일한 그라데이션 영역을 채우는 완전히 하지 않습니다. 두 원은 동일한 반지름이 그라데이션의 원의 지름을 동일 너비를 사용 하 여 사각형으로 제한 됩니다. 두 원은 서로 다른 반지름이 그라데이션의 원뿔을 형성 합니다.

두 지점 원뿔 그라데이션을 사용 하 여 실험 하려는 것 이므로 **원뿔 그라데이션** 에서 파생 되는 페이지 `InteractivePage` 두 원 반지름에 대 한 이동 되어야 하는 두 터치 포인트를 허용 하려면:

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
                       xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Effects.ConicalGradientPage"
                       Title="Conical Gradient">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        
        <Grid BackgroundColor="White"
              Grid.Row="0">
            <skiaforms:SKCanvasView x:Name="canvasView"
                                    PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>

        <Picker x:Name="tileModePicker" 
                Grid.Row="1"
                Title="Shader Tile Mode" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKShaderTileMode}">
                    <x:Static Member="skia:SKShaderTileMode.Clamp" />
                    <x:Static Member="skia:SKShaderTileMode.Repeat" />
                    <x:Static Member="skia:SKShaderTileMode.Mirror" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</local:InteractivePage>
```

코드 숨김 파일을 두 개의 정의 `TouchPoint` 50과 100의 고정된 반지름을 사용 하 여 개체:

```csharp
public partial class ConicalGradientPage : InteractivePage
{
    const int RADIUS1 = 50;
    const int RADIUS2 = 100;

    public ConicalGradientPage ()
    {
        touchPoints = new TouchPoint[2];

        touchPoints[0] = new TouchPoint
        {
            Center = new SKPoint(100, 100),
            Radius = RADIUS1
        };

        touchPoints[1] = new TouchPoint
        {
            Center = new SKPoint(300, 300),
            Radius = RADIUS2
        };

        InitializeComponent();
        baseCanvasView = canvasView;
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKColor[] colors = { SKColors.Red, SKColors.Green, SKColors.Blue };
        SKShaderTileMode tileMode = 
            (SKShaderTileMode)(tileModePicker.SelectedIndex == -1 ? 
                                        0 : tileModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateTwoPointConicalGradient(touchPoints[0].Center,
                                                                  RADIUS1,
                                                                  touchPoints[1].Center,
                                                                  RADIUS2,
                                                                  colors,
                                                                  null,
                                                                  tileMode);
            canvas.DrawRect(info.Rect, paint);
        }

        // Display the touch points here rather than by TouchPoint
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 3;

            foreach (TouchPoint touchPoint in touchPoints)
            {
                canvas.DrawCircle(touchPoint.Center, touchPoint.Radius, paint);
            }
        }
    }
}
```

`colors` 배열이 빨강, 녹색 및 파랑입니다. 아래쪽에 코드를 `PaintSurface` 검은색 원을 그라데이션의 방해 하지는 않도록 하는 대로 처리기 두 터치 포인트를 그립니다.

있음을 `DrawRect` 호출 전체 캔버스 색 그라데이션을 사용 합니다. 그러나 일반적인 경우 캔버스 많이 남아 그라데이션의 여 무색 있습니다. 다음은 가능한 세 가지 구성을 보여 주는 프로그램입니다.

[![원뿔 그라데이션](circular-gradients-images/ConicalGradient.png "원뿔 그라데이션")](circular-gradients-images/ConicalGradient-Large.png#lightbox)

IOS 화면 왼쪽에 있는 결과 보여 줍니다.는 `SKShaderTileMode` 설정은 `Clamp`합니다. 그라데이션의 두 번째 원에 가장 가까운 쪽 반대 되는 작은 원 경계 내에서 빨간색으로 시작 합니다. `Clamp` 값 red를 원뿔의 지점에 계속 해도 됩니다. 그라데이션의 해당 원 안에 들어올 이상 blue 사용 하 여 첫 번째 원형에 가장 가까운 되었지만 계속 하는 큰 원 외부 가장자리에 파란색으로 끝납니다.

Android 화면은 유사 하지만 `SKShaderTileMode` 의 `Repeat`합니다. 이제 그라데이션의 첫 번째 한 원 내부에서 시작 하 고 두 번째 원을 외부 종료 명확 하 게 됩니다. `Repeat` 빨간색 원 내부에 큰를 사용 하 여 다시 반복 그라데이션을 설정으로 인해 합니다.

UWP 화면 작은 원을 더 큰 원 안에 완전히 이동할 때 어떻게 되는지 보여 줍니다. 그라데이션의 원뿔 되 고 중지 하 고 대신 전체 영역을 채웁니다. 방사형 그라데이션 효과 비슷합니다 이지만 비대칭 이면 작은 원을 정확히 가운데에 맞춰 더 큰 원 안에 있습니다.

하나의 원을 차례로에 중첩 되어 있지만 반사 하이라이트 적합 그라데이션의 실용적인 유용성을 의구심 수 있습니다.

## <a name="conical-gradients-for-specular-highlights"></a>원뿔 그라데이션 반사 강조 표시

이 문서의 앞부분에서 반사 하이라이트를 만들려면 방사형 그라데이션을 사용 하는 방법을 살펴보았습니다. 이 목적을 위해 두 지점 원뿔 그라데이션 이용할 수 있습니다 하 고 모양을 하는 것이 좋습니다.

[![원뿔 반사 하이라이트](circular-gradients-images/ConicalSpecularHighlight.png "원뿔 반사 강조 표시")](circular-gradients-images/ConicalSpecularHighlight-Large.png#lightbox)

비대칭 모양을 보다 잘 개체는 둥근된 표면을 제안합니다. 

그리기 코드를 **원뿔 반사 강조 표시** 페이지와 동일 합니다 **방사형 반사 강조 표시** 셰이더를 제외 하 고 페이지:

```csharp
public class ConicalSpecularHighlightPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateTwoPointConicalGradient(
                                offCenter,
                                1,
                                center,
                                radius,
                                new SKColor[] { SKColors.White, SKColors.Red },
                                null,
                                SKShaderTileMode.Clamp);

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

두 원은 센터 있을 `offCenter` 고 `center`입니다. 원을 중심이 `center` 중심이 원 하지만 전체 공을 포함 하는 radius와 사용 하 여 연결 된 `offCenter` 하나의 픽셀만 반지름이. 그라데이션의 효과적으로 해당 지점에서 시작 하 고 볼의에 지에서 끝납니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
