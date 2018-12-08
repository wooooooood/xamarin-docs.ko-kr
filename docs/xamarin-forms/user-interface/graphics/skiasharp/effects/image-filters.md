---
title: SkiaSharp 이미지 필터
description: 이미지 필터 흐림 만들고 그림자를 사용 하는 방법에 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 173E7B22-AEC8-4F12-B657-1C0CEE01AD63
author: davidbritch
ms.author: dabritch
ms.date: 08/27/2018
ms.openlocfilehash: 517ebfb529dd26236ba157d40168fa7c75288d27
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53050376"
---
# <a name="skiasharp-image-filters"></a>SkiaSharp 이미지 필터

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

이미지 필터는 픽셀 이미지를 구성 하는 모든 색 비트에서 작동 하는 효과입니다. 이 문서에 설명 된 대로 알파 채널 에서만 작동 하는 마스크 필터 보다 더 많은 [ **SkiaSharp 마스크 필터**](mask-filters.md)합니다. 이미지 필터를 사용 하려면 설정 합니다 [ `ImageFilter` ](xref:SkiaSharp.SKPaint.ImageFilter) 속성을 `SKPaint` 형식의 개체에 [ `SKImageFilter` ](xref:SkiaSharp.SKImageFilter) 클래스의 정적 메서드 중 하나를 호출 하 여 만든 합니다.

마스크 필터에 익숙해지는 가장 좋은 방법은 이러한 정적 메서드를 사용 하 여 실험가 있습니다. 전체 비트맵의 blur 마스크 필터를 사용할 수 있습니다.

![예제에서는 흐림](image-filters-images/ImageFilterExample.png "예제 흐리게 표시")

또한이 문서를 만들고 드롭 그림자, 볼록 및 효과 조각에 대 한 이미지 필터를 사용 하 여 보여 줍니다.

## <a name="blurring-vector-graphics-and-bitmaps"></a>벡터 그래픽 및 비트맵을 흐리게 표시

만든 흐림 효과 [ `SKImageFilter.CreateBlur` ](xref:SkiaSharp.SKImageFilter.CreateBlur*) 정적 메서드가에 흐리게 방법에 대 한 중요 한 장점 합니다 [ `SKMaskFilter` ](xref:SkiaSharp.SKMaskFilter) 클래스: 이미지 필터 수는 전체 비트맵을 흐리게 표시 합니다. 메서드가 다음 구문:

```csharp
public static SkiaSharp.SKImageFilter CreateBlur (float sigmaX, float sigmaY,
                                                  SKImageFilter input = null,
                                                  SKImageFilter.CropRect cropRect = null);
```

이 메서드에 두 시그마 값 &mdash; 가로 방향 및 세로 방향에 대 한 두 번째 흐림 정도 대 한 첫 번째입니다. 세 번째 선택적 인수로 다른 이미지 필터를 지정 하 여 이미지 필터 연계할 수 있습니다. 자르기 사각형을 지정할 수도 있습니다.

**이미지 실험 흐리게 표시** 페이지에 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 포함 하는 두 `Slider` 흐림 효과의 다양 한 수준을 설정 시험해 볼 수 있는 뷰:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.ImageBlurExperimentPage"
             Title="Image Blur Experiment">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="sigmaXSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaXSlider},
                              Path=Value,
                              StringFormat='Sigma X = {0:F1}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="sigmaYSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaYSlider},
                              Path=Value,
                              StringFormat='Sigma Y = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

코드 숨김 파일에서는 두 개의 `Slider` 호출에 값 `SKImageFilter.CreateBlur` 에 대 한는 `SKPaint` 텍스트와 비트맵을 표시 하는 데 사용 되는 개체:


```csharp
public partial class ImageBlurExperimentPage : ContentPage
{
    const string TEXT = "Blur My Text";

    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                            typeof(MaskBlurExperimentPage),
                            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public ImageBlurExperimentPage ()
    {
        InitializeComponent ();
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

        canvas.Clear(SKColors.Pink);

        // Get values from sliders
        float sigmaX = (float)sigmaXSlider.Value;
        float sigmaY = (float)sigmaYSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = (info.Width - 100) / (TEXT.Length / 2);
            paint.ImageFilter = SKImageFilter.CreateBlur(sigmaX, sigmaY);

            // Get text bounds and calculate display rectangle
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);
            SKRect textRect = new SKRect(0, 0, info.Width, textBounds.Height + 50);

            // Center the text in the display rectangle
            float xText = textRect.Width / 2 - textBounds.MidX;
            float yText = textRect.Height / 2 - textBounds.MidY;

            canvas.DrawText(TEXT, xText, yText, paint);

            // Calculate rectangle for bitmap
            SKRect bitmapRect = new SKRect(0, textRect.Bottom, info.Width, info.Height);
            bitmapRect.Inflate(-50, -50);

            canvas.DrawBitmap(bitmap, bitmapRect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

스크린샷 세 개에 대 한 다양 한 설정을 표시 합니다 `sigmaX` 고 `sigmaY` 설정:

[![흐린 실험 이미지](image-filters-images/ImageBlurExperiment.png "흐리게 실험 이미지")](image-filters-images/ImageBlurExperiment-Large.png#lightbox)

흐리게 효과 다른 디스플레이 크기 및 해상도 간에 일관 된 설정 `sigmaX` 고 `sigmaY` 흐리게 효과에 적용 되는 이미지의 렌더링 된 픽셀 크기에 비례 하는 값입니다.

## <a name="drop-shadow"></a>그림자

합니다 [ `SKImageFilter.CreateDropShadow` ](xref:SkiaSharp.SKImageFilter.CreateDropShadow*) 정적 메서드를 만듭니다는 `SKImageFilter` 그림자에 대 한 개체:

```csharp
public static SKImageFilter CreateDropShadow (float dx, float dy,
                                              float sigmaX, float sigmaY,
                                              SKColor color,
                                              SKDropShadowImageFilterShadowMode shadowMode,
                                              SKImageFilter input = null,
                                              SKImageFilter.CropRect cropRect = null);
```

이 개체를 설정 합니다 `ImageFilter` 의 속성을 `SKPaint` 개체 및 해당 개체를 사용 하 여 그리는 것 뒤에 그림자를 기준으로 해야 합니다.

합니다 `dx` 고 `dy` 매개 변수에서 그래픽 개체의 픽셀 단위의 그림자의 가로 및 세로 오프셋을 나타냅니다. 2 차원 그래픽에서 규칙 광원 이러한 두 인수가 모두 아래 및 그래픽 개체의 오른쪽에 그림자를 위치로 양의 되도록 하는 홈페이지에서 오는 것으로 가정 하는 것입니다.

합니다 `sigmaX` 고 `sigmaY` 매개 변수 그림자에 대 한 요소를 흐리게 표시 됩니다.

`color` 매개 변수는 그림자의 색입니다. 이 `SKColor` 값 투명도 포함할 수 있습니다. 한 가지 방법은 색 값은 `SKColors.Black.WithAlpha(0x80)` 색 백그라운드 어두워집니다.

마지막 두 매개 변수는 선택적입니다.

합니다 **섀도 실험 삭제** 프로그램의 값을 시험해 볼 수 있습니다 `dx`, `dy`를 `sigmaX`, 및 `sigmaY` 그림자 효과로 텍스트 문자열을 표시 하 합니다. XAML 파일은 4 `Slider` 뷰 이러한 값을 설정 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.DropShadowExperimentPage"
             Title="Drop Shadow Experiment">
    <ContentPage.Resources>
        <Style TargetType="Slider">
            <Setter Property="Margin" Value="10, 0" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="HorizontalTextAlignment" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="dxSlider"
                Minimum="-20"
                Maximum="20"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference dxSlider},
                              Path=Value,
                              StringFormat='Horizontal offset = {0:F1}'}" />

        <Slider x:Name="dySlider"
                Minimum="-20"
                Maximum="20"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference dySlider},
                              Path=Value,
                              StringFormat='Vertical offset = {0:F1}'}" />

        <Slider x:Name="sigmaXSlider"
                Maximum="10"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaXSlider},
                              Path=Value,
                              StringFormat='Sigma X = {0:F1}'}" />

        <Slider x:Name="sigmaYSlider"
                Maximum="10"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaYSlider},
                              Path=Value,
                              StringFormat='Sigma Y = {0:F1}'}" />
    </StackLayout>
</ContentPage>
```

코드 숨김 파일을 파란색 텍스트 문자열에서 빨간색 그림자를 만들려면 해당 값을 사용 합니다.

```csharp
public partial class DropShadowExperimentPage : ContentPage
{
    const string TEXT = "Drop Shadow";

    public DropShadowExperimentPage ()
    {
        InitializeComponent ();
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

        // Get values from sliders
        float dx = (float)dxSlider.Value;
        float dy = (float)dySlider.Value;
        float sigmaX = (float)sigmaXSlider.Value;
        float sigmaY = (float)sigmaYSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = info.Width / 7;
            paint.Color = SKColors.Blue;
            paint.ImageFilter = SKImageFilter.CreateDropShadow(
                                    dx,
                                    dy,
                                    sigmaX,
                                    sigmaY,
                                    SKColors.Red,
                                    SKDropShadowImageFilterShadowMode.DrawShadowAndForeground);

            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Center the text in the display rectangle
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

실행 중인 프로그램이 다음과 같습니다.

[![섀도 실험을 삭제할](image-filters-images/DropShadowExperiment.png "섀도 실험 삭제")](image-filters-images/DropShadowExperiment-Large.png#lightbox)

맨 오른쪽에 있는 유니버설 Windows 플랫폼 스크린샷에 음수 오프셋된 값 발생할 그림자 위와 텍스트의 왼쪽에 표시 되도록 합니다. 이 컴퓨터 그래픽에 대 한 규칙 아닌 오른쪽 아래에서 조명이 광원을 제안 합니다. 있지만 어떤 방식으로든에서 잘못 아마도 매우 흐린 만들어질 수도 되 그림자 이므로 대부분의 그림자 보다 더 장식용 것 같습니다.

## <a name="lighting-effects"></a>조명 효과

`SKImageFilter` 클래스는 비슷한 이름 및 매개 변수, 복잡성을 증가 하는 순서에 나와 있는 6 개의 메서드를 정의 합니다.

- [`CreateDistantLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateDistantLitDiffuse*)
- [`CreateDistantLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateDistantLitSpecular*)
- [`CreatePointLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreatePointLitDiffuse*)
- [`CreatePointLitSpecular`](xref:SkiaSharp.SKImageFilter.CreatePointLitSpecular*)
- [`CreateSpotLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateSpotLitDiffuse*)
- [`CreateSpotLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateSpotLitSpecular*)

이러한 메서드는 3 차원 화면에서 광원의 다양 한 종류의 효과 모방 하는 이미지 필터를 만듭니다. 결과 이미지 필터 서 이러한 개체를 상승 된 권한 또는 들어간 표시할 발생할 수 있습니다, 3D 공간에서 반사 강조 표시 또는 있었던 것 처럼 2 차원 개체를 나타냅니다.

`Distant` 밝은 메서드 조명이 먼 거리에서를 가정 합니다. 불어넣을 개체를 목적으로 밝은 지구 작은 영역에서 Sun 마찬가지로 3D 공간에서 일관 된 단방향으로 간주 됩니다. `Point` 밝은 메서드를 내보내는 모든 방향으로 조명을 3D 공간에서 전구를 모방 합니다. `Spot` light에 위치 및 방향 손전등 매우 유사 하 게 합니다.

위치 및 방향 3D 공간에서 둘 다의 값을 사용 하 여 지정 된를 [ `SKPoint3` ](xref:SkiaSharp.SKPoint3) 비슷한 구조 `SKPoint` 있지만 라는 세 가지 속성을 사용 하 여 `X`를 `Y`, 및 `Z`합니다.

수와 이러한 메서드는 매개 변수는 복잡성으로 실험을 어려워집니다. 시작 하는 데는 **먼 Light 실험** 매개 변수를 실험해 볼 수 있습니다. 페이지를 `CreateDistantLightDiffuse` 메서드:

```csharp
public static SKImageFilter CreateDistantLitDiffuse (SKPoint3 direction,
                                                     SKColor lightColor,
                                                     float surfaceScale,
                                                     float kd,
                                                     SKImageFilter input = null,
                                                     SKImageFilter.CropRect cropRect = null);
```

페이지에는 마지막 두 개의 선택적 매개 변수를 사용 하지 않습니다.

세 `Slider` 는 XAML에서 뷰 파일 수를 선택 하면를 `Z` 의 좌표를 `SKPoint3` 값을 `surfaceScale` 매개 변수를 및 `kd` "확산 조명 상수"는 API 설명서에 정의 된 매개 변수:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaLightExperiment.MainPage"
             Title="Distant Light Experiment">

    <StackLayout>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           VerticalOptions="FillAndExpand" />

        <Slider x:Name="zSlider"
                Minimum="-10"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zSlider},
                              Path=Value,
                              StringFormat='Z = {0:F0}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="surfaceScaleSlider"
                Minimum="-1"
                Maximum="1"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference surfaceScaleSlider},
                              Path=Value,
                              StringFormat='Surface Scale = {0:F1}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="lightConstantSlider"
                Minimum="-1"
                Maximum="1"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference lightConstantSlider},
                              Path=Value,
                              StringFormat='Light Constant = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

코드 숨김 파일을 이러한 3 개의 값을 가져옵니다 및 사용 하 여 텍스트 문자열을 표시 하는 이미지 필터:

```csharp
public partial class DistantLightExperimentPage : ContentPage
{
    const string TEXT = "Lighting";

    public DistantLightExperimentPage()
    {
        InitializeComponent();
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

        float z = (float)zSlider.Value;
        float surfaceScale = (float)surfaceScaleSlider.Value;
        float lightConstant = (float)lightConstantSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.IsAntialias = true;

            // Size text to 90% of canvas width
            paint.TextSize = 100;
            float textWidth = paint.MeasureText(TEXT);
            paint.TextSize *= 0.9f * info.Width / textWidth;

            // Find coordinates to center text
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            float xText = info.Rect.MidX - textBounds.MidX;
            float yText = info.Rect.MidY - textBounds.MidY;

            // Create distant light image filter
            paint.ImageFilter = SKImageFilter.CreateDistantLitDiffuse(
                                    new SKPoint3(2, 3, z),
                                    SKColors.White,
                                    surfaceScale,
                                    lightConstant);

            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

첫 번째 인수 `SKImageFilter.CreateDistantLitDiffuse` 광원의 방향입니다. 양의 X 및 Y 좌표 광원 오른쪽 및 아래쪽 뾰족한 임을 나타냅니다. 화면에 양수 Z 좌표 지점입니다. XAML 파일을 사용 하면 음수 Z 값을 선택할 수 있습니다 하지만 볼 수 있도록에 작업이 수행 됩니다: 음수 Z 좌표는 화면 가리키도록 광원을 발생 하는 개념적으로 합니다. 항목에 대 한 다른 다음 작은 음수 값에 조명 효과 작동이 중지 됩니다.

`surfaceScale` 인수는-1에서 1까지 수 있습니다. (높거나 낮은 값 있을 영향을 주지 않습니다.) 이들은 그래픽 개체 (이 경우 텍스트 문자열) 캔버스 화면에서 거리를 나타내는 Z 축에 상대 값입니다. 캔버스의 화면 위의 텍스트 문자열을 발생 시키는 음수 및 양수 값을 캔버스에 들어온 사용 합니다.

`lightConstant` 값은 양수 여야 합니다. (수 있는 프로그램을 음수 값 작업을 중지 하는 효과 입히기 볼 수 있도록 합니다.) 값이 높을수록 더 강 할수록 light을 발생합니다.

이러한 요소는 볼록 가져오려고 분산할 수 있습니다 하면 적용 `surfaceScale` (마찬가지로 iOS 및 Android 스크린샷) 음수인 및 음각을 하면 적용 `surfaceScale` 이 양수인 경우으로 오른쪽에서 UWP 스크린 샷 포함:

[![멀리 떨어져 있는 밝은 실험](image-filters-images/DistantLightExperiment.png "먼 밝은 실험")](image-filters-images/DistantLightExperiment-Large.png#lightbox)

Android 스크린 샷에 Z 값은 0으로, 아래쪽 및 오른쪽에 밝은 가리키고 있음을 의미 합니다. 백그라운드 켜지는 되지 않습니다 및 텍스트 문자열의 화면 켜지는 되지 중 하나입니다. 밝은 매우 미묘한 효과 텍스트의에 지만 영향을 줍니다.

다른 방법은 볼록 및 음각 텍스트 문서에서 설명 했습니다 [The 변환 변환](../transforms/translate.md): 텍스트 문자열은 서로 약간 오프셋 되는 다양 한 색으로 두 번 표시 됩니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
