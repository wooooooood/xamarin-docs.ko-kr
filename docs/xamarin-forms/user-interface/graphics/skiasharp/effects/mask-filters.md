---
제목: "SkiaSharp 마스크 필터" 설명: "마스크 필터를 사용 하 여 흐린 및 기타 알파 효과를 만드는 방법을 알아봅니다."
ms. prod: xamarin. 기술: xamarin-skiasharp assetid: 940422A1-8BC0-4039-8AD7-26C61320F858 author: davidbritch: dabritch: 08/27/2018:-loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="skiasharp-mask-filters"></a>SkiaSharp mask 필터

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

마스크 필터는 그래픽 개체의 기 하 도형 및 알파 채널을 조작 하는 효과입니다. 마스크 필터를 사용 하려면 [`MaskFilter`](xref:SkiaSharp.SKPaint.MaskFilter) 의 속성을 `SKPaint` [`SKMaskFilter`](xref:SkiaSharp.SKMaskFilter) 정적 메서드 중 하나를 호출 하 여 만든 형식의 개체로 설정 합니다 `SKMaskFilter` .

마스크 필터에 익숙해지는 가장 좋은 방법은 이러한 정적 메서드를 실험 하는 것입니다. 가장 유용한 마스크 필터는 흐림 효과를 만듭니다.

![흐림 효과 예제](mask-filters-images/MaskFilterExample.png "흐림 효과 예제")

이 문서에서 설명 하는 유일한 마스크 필터 기능입니다. [**SkiaSharp 이미지 필터**](image-filters.md) 에 대 한 다음 문서에서는이를 선호 하는 흐리게 효과에 대해서도 설명 합니다. 

정적 [`SKMaskFilter.CreateBlur`](xref:SkiaSharp.SKMaskFilter.CreateBlur(SkiaSharp.SKBlurStyle,System.Single)) 메서드의 구문은 다음과 같습니다.

```csharp
public static SKMaskFilter CreateBlur (SKBlurStyle blurStyle, float sigma);
```

오버 로드를 사용 하면 흐림 효과를 만드는 데 사용 되는 알고리즘에 대 한 플래그를 지정 하 고 다른 그래픽 개체에서 다룰 영역에서 흐리게를 방지 하는 사각형을 지정할 수 있습니다.

[`SKBlurStyle`](xref:SkiaSharp.SKBlurStyle)는 다음 멤버를 포함 하는 열거형입니다.

- `Normal`
- `Solid`
- `Outer`
- `Inner`

이러한 스타일의 효과는 아래 예제에 나와 있습니다. `sigma`매개 변수는 흐림 효과의 범위를 지정 합니다. 이전 버전의 버전에는 흐림의 범위가 반경 값으로 표시 되었습니다. 응용 프로그램에 대 한 radius 값이 더 적합 한 경우에는 해당 값 [`SKMaskFilter.ConvertRadiusToSigma`](xref:SkiaSharp.SKMaskFilter.ConvertRadiusToSigma*) 을 다른 값으로 변환할 수 있는 정적 메서드가 있습니다. 메서드는 반지름을 0.57735에 곱하고 0.5를 추가 합니다.

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **마스크 흐림 실험** 페이지를 사용 하 여 흐림 스타일 및 시그마 값을 시험해 볼 수 있습니다. XAML 파일은 `Picker` 4 개의 `SKBlurStyle` 열거형 멤버와 `Slider` 시그마 값을 지정 하는을 사용 하 여를 인스턴스화합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.MaskBlurExperimentPage"
             Title="Mask Blur Experiment">
    
    <StackLayout>
        <skiaforms:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="blurStylePicker" 
                Title="Filter Blur Style" 
                Margin="10, 0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKBlurStyle}">
                    <x:Static Member="skia:SKBlurStyle.Normal" />
                    <x:Static Member="skia:SKBlurStyle.Solid" />
                    <x:Static Member="skia:SKBlurStyle.Outer" />
                    <x:Static Member="skia:SKBlurStyle.Inner" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Slider x:Name="sigmaSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaSlider},
                              Path=Value,
                              StringFormat='Sigma = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

코드 숨김이 이러한 값을 사용 하 여 개체를 만들고 `SKMaskFilter` 개체의 속성으로 설정 합니다 `MaskFilter` `SKPaint` . 이 `SKPaint` 개체는 텍스트 문자열과 비트맵을 모두 그리는 데 사용 됩니다.

```csharp
public partial class MaskBlurExperimentPage : ContentPage
{
    const string TEXT = "Blur My Text";

    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                            typeof(MaskBlurExperimentPage), 
                            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public MaskBlurExperimentPage ()
    {
        InitializeComponent ();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
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

        // Get values from XAML controls
        SKBlurStyle blurStyle =
            (SKBlurStyle)(blurStylePicker.SelectedIndex == -1 ?
                                        0 : blurStylePicker.SelectedItem);

        float sigma = (float)sigmaSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = (info.Width - 100) / (TEXT.Length / 2);
            paint.MaskFilter = SKMaskFilter.CreateBlur(blurStyle, sigma);

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

다음은 `Normal` 흐리게 스타일 및 늘어난 수준으로 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에서 실행 되는 프로그램입니다 `sigma` .

[![마스크 흐림 실험-보통](mask-filters-images/MaskBlurExperiment-Normal.png "마스크 흐림 실험-보통")](mask-filters-images/MaskBlurExperiment-Normal-Large.png#lightbox)

비트맵의 가장자리에만 흐림 효과가 적용 되는 것을 알 수 있습니다. `SKMaskFilter`이 클래스는 전체 비트맵 이미지를 흐리게 표시 하려는 경우 사용 하는 데 올바른 효과가 없습니다. 이를 위해 [`SKImageFilter`](xref:SkiaSharp.SKImageFilter) [**SkiaSharp 이미지 필터**](image-filters.md)에 대 한 다음 문서에 설명 된 대로 클래스를 사용 합니다.

인수의 값이 늘어나면 텍스트가 더 흐리게 됩니다 `sigma` . 이 프로그램을 실험 하는 동안 특정 값에 대해 `sigma` Windows 10 desktop에서 흐리게 표시 되는 것을 알 수 있습니다. 이러한 차이는 모바일 장치 보다는 데스크톱 모니터의 픽셀 밀도가 낮으므로 픽셀 단위의 텍스트 높이가 더 낮기 때문에 발생 합니다. `sigma`값은 픽셀 단위의 흐림 범위에 비례 하므로 지정 된 값에 대 한 `sigma` 효과는 해상도가 낮은 경우 더 복잡 합니다. 프로덕션 응용 프로그램에서는 그래픽 크기에 비례 하는 값을 계산 하는 것 `sigma` 이 좋습니다. 

응용 프로그램에 가장 적합 한 흐림 수준을 정착 전에 몇 가지 값을 사용해 보세요. 예를 들어 **마스크 흐림 실험** 페이지에서 다음과 같이 설정 해 봅니다 `sigma` .

```csharp
sigma = paint.TextSize / 18;
paint.MaskFilter = SKMaskFilter.CreateBlur(blurStyle, sigma);
```

이제는 아무런 `Slider` 영향을 주지 않지만 흐림 효과의 정도는 플랫폼 간에 일관 됩니다.

[![마스크 흐림 실험-일관성](mask-filters-images/MaskBlurExperiment-Consistent.png "마스크 흐림 실험-일관성")](mask-filters-images/MaskBlurExperiment-Consistent-Large.png#lightbox)

지금 까지의 모든 스크린샷은 열거형 멤버를 사용 하 여 만든 흐림을 표시 했습니다 `SKBlurStyle.Normal` . 다음 스크린샷에는 `Solid` , `Outer` 및 흐리게 스타일의 효과를 보여 줍니다 `Inner` .

[![마스크 흐림 실험](mask-filters-images/MaskBlurExperiment.png "마스크 흐림 실험")](mask-filters-images/MaskBlurExperiment-Large.png#lightbox)

IOS 스크린샷에서는 스타일을 보여 줍니다 `Solid` . 텍스트 문자는 여전히 검정 실선으로 표시 되 고 이러한 텍스트 문자 외부에는 흐림 효과가 추가 됩니다. 

가운데에 있는 Android 스크린 샷에서는 스타일을 보여 줍니다 `Outer` . 문자 스트로크 자체는 비트맵 처럼 제거 되 고 텍스트 문자가 한 번 나타나는 빈 공간을 둘러쌉니다. 

오른쪽의 UWP 스크린샷에서는 스타일을 보여 줍니다 `Inner` . 흐림 효과는 일반적으로 텍스트 문자가 차지 하는 영역으로 제한 됩니다.

[**SkiaSharp 선형 그라데이션**](shaders/linear-gradient.md#transparency-and-gradients) 문서에서는 선형 그라데이션 및 변환을 사용 하 여 텍스트 문자열의 반사를 모방 하는 **리플렉션 그라데이션** 프로그램에 대해 설명 했습니다.

[![반사 그라데이션](shaders/linear-gradient-images/ReflectionGradient.png "반사 그라데이션")](shaders/linear-gradient-images/ReflectionGradient-Large.png#lightbox)

**흐린 반사** 페이지는 해당 코드에 단일 문을 추가 합니다.

```csharp
public class BlurryReflectionPage : ContentPage
{
    const string TEXT = "Reflection";

    public BlurryReflectionPage()
    {
        Title = "Blurry Reflection";

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
            // Set text color to blue
            paint.Color = SKColors.Blue;

            // Set text size to fill 90% of width
            paint.TextSize = 100;
            float width = paint.MeasureText(TEXT);
            float scale = 0.9f * info.Width / width;
            paint.TextSize *= scale;

            // Get text bounds
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Calculate offsets to position text above center
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2;

            // Draw unreflected text
            canvas.DrawText(TEXT, xText, yText, paint);

            // Shift textBounds to match displayed text
            textBounds.Offset(xText, yText);

            // Use those offsets to create a gradient for the reflected text
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, textBounds.Top),
                                new SKPoint(0, textBounds.Bottom),
                                new SKColor[] { paint.Color.WithAlpha(0),
                                                paint.Color.WithAlpha(0x80) },
                                null,
                                SKShaderTileMode.Clamp);

            // Create a blur mask filter
            paint.MaskFilter = SKMaskFilter.CreateBlur(SKBlurStyle.Normal, paint.TextSize / 36);

            // Scale the canvas to flip upside-down around the vertical center
            canvas.Scale(1, -1, 0, yText);

            // Draw reflected text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

새 문은 텍스트 크기를 기준으로 반사 된 텍스트에 대 한 흐림 효과 필터를 추가 합니다.

```csharp
paint.MaskFilter = SKMaskFilter.CreateBlur(SKBlurStyle.Normal, paint.TextSize / 36);
```

이러한 흐림 효과 필터는 반사를 훨씬 더 사실적으로 보이게 합니다.

[![흐린 반사](mask-filters-images/BlurryReflection.png "흐린 반사")](mask-filters-images/BlurryReflection-Large.png#lightbox)

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
