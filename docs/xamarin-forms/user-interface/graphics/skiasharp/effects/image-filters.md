---
제목: "SkiaSharp 이미지 필터" 설명: "이미지 필터를 사용 하 여 흐리게 및 그림자를 만드는 방법을 알아봅니다."
ms. prod: xamarin. 기술: xamarin-skiasharp assetid: 173E7B22-AEC8-4F12-B657-1C0CEE01AD63 author: davidbritch: dabritch: 08/27/2018:-loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="skiasharp-image-filters"></a>SkiaSharp 이미지 필터

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

이미지 필터는 이미지를 구성 하는 모든 색 비트 픽셀에서 작동 하는 효과입니다. [**SkiaSharp 마스크 필터**](mask-filters.md)문서에 설명 된 대로 알파 채널 에서만 작동 하는 마스크 필터 보다 더 다양 합니다. 이미지 필터를 사용 하려면 [`ImageFilter`](xref:SkiaSharp.SKPaint.ImageFilter) 의 속성을 `SKPaint` [`SKImageFilter`](xref:SkiaSharp.SKImageFilter) 클래스의 정적 메서드 중 하나를 호출 하 여 만든 형식의 개체로 설정 합니다.

마스크 필터에 익숙해지는 가장 좋은 방법은 이러한 정적 메서드를 실험 하는 것입니다. 마스크 필터를 사용 하 여 전체 비트맵을 흐리게 만들 수 있습니다.

![흐림 효과 예제](image-filters-images/ImageFilterExample.png "흐림 효과 예제")

또한이 문서에서는 이미지 필터를 사용 하 여 그림자를 만드는 방법과 볼록 효과 및 engraving 효과를 보여 줍니다.

## <a name="blurring-vector-graphics-and-bitmaps"></a>벡터 그래픽 및 비트맵 흐리게

정적 메서드에서 만든 흐림 효과는 [`SKImageFilter.CreateBlur`](xref:SkiaSharp.SKImageFilter.CreateBlur*) 클래스의 흐림 메서드 보다 상당한 이점이 [`SKMaskFilter`](xref:SkiaSharp.SKMaskFilter) 있습니다. 이미지 필터는 전체 비트맵을 흐리게 만들 수 있습니다. 메서드의 구문은 다음과 같습니다.

```csharp
public static SkiaSharp.SKImageFilter CreateBlur (float sigmaX, float sigmaY,
                                                  SKImageFilter input = null,
                                                  SKImageFilter.CropRect cropRect = null);
```

이 메서드에는 &mdash; 가로 방향의 흐림 범위에 대해 첫 번째 시그마 값과 세로 방향에 대 한 두 번째 시그마 값이 있습니다. 다른 이미지 필터를 선택적인 세 번째 인수로 지정 하 여 이미지 필터를 계단식으로 지정할 수 있습니다. 자르기 사각형을 지정할 수도 있습니다.

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 의 **이미지 흐림 실험** 페이지에 `Slider` 는 다양 한 수준의 흐림 효과를 설정 하 여 시험해 볼 수 있는 두 가지 보기가 포함 되어 있습니다.

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

코드에서 두 값을 사용 하 여 `Slider` `SKImageFilter.CreateBlur` `SKPaint` 텍스트와 비트맵을 표시 하는 데 사용 되는 개체에 대해를 호출 합니다.

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

3 개의 스크린샷은 및 설정에 대 한 다양 한 설정을 보여 줍니다 `sigmaX` `sigmaY` .

[![이미지 흐림 실험](image-filters-images/ImageBlurExperiment.png "이미지 흐림 실험")](image-filters-images/ImageBlurExperiment-Large.png#lightbox)

여러 표시 크기와 해상도 간에 흐림 효과를 유지 하려면 `sigmaX` 및 `sigmaY` 를 흐리게 적용 되는 이미지의 렌더링 된 픽셀 크기에 비례 하는 값으로 설정 합니다.

## <a name="drop-shadow"></a>그림자

[`SKImageFilter.CreateDropShadow`](xref:SkiaSharp.SKImageFilter.CreateDropShadow*)정적 메서드는 그림자 `SKImageFilter` 에 대 한 개체를 만듭니다.

```csharp
public static SKImageFilter CreateDropShadow (float dx, float dy,
                                              float sigmaX, float sigmaY,
                                              SKColor color,
                                              SKDropShadowImageFilterShadowMode shadowMode,
                                              SKImageFilter input = null,
                                              SKImageFilter.CropRect cropRect = null);
```

이 개체를 개체의 속성으로 설정 합니다 .이 개체를 `ImageFilter` `SKPaint` 사용 하 여 그리는 모든 항목에는 그림자가 표시 됩니다.

`dx`및 `dy` 매개 변수는 그래픽 개체에서 그림자의 가로 및 세로 오프셋 (픽셀)을 표시 합니다. 2D 그래픽의 규칙은 왼쪽 위에서 광원의 광원을 가정 하는 것입니다. 즉,이 두 인수 모두 양수 여야 합니다. 즉, 그래픽 개체의 오른쪽에 그림자를 배치 해야 합니다.

`sigmaX`및 `sigmaY` 매개 변수는 그림자에 대 한 흐리게 계수입니다.

`color`매개 변수는 그림자의 색입니다. 이 `SKColor` 값은 투명성을 포함할 수 있습니다. 한 가지 가능성은 색 `SKColors.Black.WithAlpha(0x80)` 배경을 어둡게 하기 위한 색 값입니다.

마지막 두 매개 변수는 선택 사항입니다.

**Drop shadow 실험** 프로그램을 사용 하 여,, 및 값을 실험 하 여 그림자가 `dx` `dy` `sigmaX` `sigmaY` 있는 텍스트 문자열을 표시할 수 있습니다. XAML 파일은 `Slider` 다음 값을 설정 하는 네 가지 뷰를 인스턴스화합니다.

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

코드 숨김 파일은 이러한 값을 사용 하 여 파란색 텍스트 문자열에 빨강 그림자를 만듭니다.

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

실행 중인 프로그램은 다음과 같습니다.

[![섀도 실험 삭제](image-filters-images/DropShadowExperiment.png "섀도 실험 삭제")](image-filters-images/DropShadowExperiment-Large.png#lightbox)

맨 오른쪽에 있는 유니버설 Windows 플랫폼 스크린 샷의 음수 오프셋 값은 그림자가 텍스트의 위쪽과 왼쪽에 표시 되도록 합니다. 이는 컴퓨터 그래픽의 규칙이 아닌 오른쪽 아래의 광원을 제안 합니다. 하지만 그림자가 매우 희미 하 여 대부분의 그림자 보다 많은 장식용으로 보입니다.

## <a name="lighting-effects"></a>조명 효과

클래스는 보다 `SKImageFilter` 복잡 한 이름 및 매개 변수를 포함 하는 6 개의 메서드를 정의 합니다.

- [`CreateDistantLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateDistantLitDiffuse*)
- [`CreateDistantLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateDistantLitSpecular*)
- [`CreatePointLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreatePointLitDiffuse*)
- [`CreatePointLitSpecular`](xref:SkiaSharp.SKImageFilter.CreatePointLitSpecular*)
- [`CreateSpotLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateSpotLitDiffuse*)
- [`CreateSpotLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateSpotLitSpecular*)

이러한 메서드는 3 차원 표면에서 다양 한 종류의 빛 효과를 모방 하는 이미지 필터를 만듭니다. 결과 이미지 필터는 2 차원 개체를 3D 공간에 있었던 것 처럼 비춥니다. 그러면 이러한 개체가 상승 또는 반전 된 상태로 표시 되거나 반사 강조 표시로 표시 될 수 있습니다.

`Distant`광원 메서드는 거리가 먼 거리에서 오는 것으로 가정 합니다. 개체를 비추는 목적을 위해 광원은 3D 공간에서 한 가지 일관 된 방향을 가리키는 것으로 가정 합니다 .이는 지구의 작은 영역에 있는 Sun과 매우 비슷합니다. `Point`광원 메서드는 모든 방향으로 조명을 내보내는 3d 공간에 위치한 전구를 모방 합니다. `Spot`광원은 손전등와 매우 비슷한 위치와 방향을 모두 갖습니다.

3D 공간의 위치와 방향은 모두 구조체의 값으로 지정 됩니다 .이 값은와 [`SKPoint3`](xref:SkiaSharp.SKPoint3) 비슷하지만 `SKPoint` , 및 라는 세 가지 속성이 `X` 있습니다 `Y` `Z` .

이러한 메서드에 대 한 매개 변수의 수와 복잡성은 실험을 어렵게 만듭니다. 시작 하기 위해 **원거리 조명 실험** 페이지에서 메서드에 대 한 매개 변수를 시험해 볼 수 있습니다 `CreateDistantLightDiffuse` .

```csharp
public static SKImageFilter CreateDistantLitDiffuse (SKPoint3 direction,
                                                     SKColor lightColor,
                                                     float surfaceScale,
                                                     float kd,
                                                     SKImageFilter input = null,
                                                     SKImageFilter.CropRect cropRect = null);
```

페이지에서 마지막 두 개의 선택적 매개 변수를 사용 하지 않습니다.

`Slider`XAML 파일의 세 가지 뷰를 사용 하 여 `Z` `SKPoint3` 값, `surfaceScale` 매개 변수 및 매개 변수의 좌표를 선택할 수 있습니다 `kd` .이는 API 설명서에서 "확산 조명 상수"로 정의 됩니다.

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

코드를 가져오는 파일은 이러한 세 가지 값을 가져오고이를 사용 하 여 텍스트 문자열을 표시 하는 이미지 필터를 만듭니다.

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

의 첫 번째 인수는 `SKImageFilter.CreateDistantLitDiffuse` 조명의 방향입니다. 양수 X 및 Y 좌표는 광원이 오른쪽 아래를 가리키고 아래쪽을 가리키고 있음을 의미 합니다. 양의 Z 좌표가 화면을 가리킵니다. XAML 파일을 사용 하면 음수 Z 값을 선택할 수 있지만,이는 개념적으로 음수 Z 좌표를 사용 하면 조명이 화면에서 제외 되는 상황을 볼 수 있습니다. 그 밖의 작은 음수 값에 대해서는 조명 효과의 작동이 중지 됩니다.

`surfaceScale`인수는-1에서 1 사이에 있을 수 있습니다. (더 높거나 낮은 값은 더 이상 영향을 주지 않습니다.) 이는 캔버스 표면의 그래픽 개체 (이 경우 텍스트 문자열)의 치환을 나타내는 Z 축의 상대 값입니다. 음수 값을 사용 하 여 캔버스 표면 위에 텍스트 문자열을, 양수 값을 사용 하 여 캔버스에 놓습니다.

값은 양수 여야 합니다 `lightConstant` . 프로그램에서 음수 값을 허용 하므로 작업을 중지 하는 데 영향을 주는 것을 알 수 있습니다. 값이 높을수록 더 강한 빛이 발생 합니다.

이 `surfaceScale` 음수 (iOS 및 Android 스크린샷을 사용 하는 경우와 마찬가지로)가 음수 이면 `surfaceScale` 오른쪽에 UWP 스크린샷 처럼가 양수인 경우에는 볼록 효과를 얻기 위해 이러한 요소를 균형 있게 조정할 수 있습니다.

[![원거리 조명 실험](image-filters-images/DistantLightExperiment.png "원거리 조명 실험")](image-filters-images/DistantLightExperiment-Large.png#lightbox)

Android 스크린 샷에는 Z 값 0이 있습니다. 즉, 조명이 오른쪽 아래로만을 가리키고 있음을 의미 합니다. 배경은 불이 아니며 텍스트 문자열의 표면에 불이 나타나지 않습니다. 광원은 매우 미묘한 효과를 위해 텍스트 가장자리에만 영향을 주는 효과입니다.

볼록 및 오목 텍스트에 대 한 대체 방법은 [변환 변환](../transforms/translate.md)문서에 설명 되어 있습니다. 텍스트 문자열은 서로 약간 오프셋 되는 다른 색을 사용 하 여 두 번 표시 됩니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
