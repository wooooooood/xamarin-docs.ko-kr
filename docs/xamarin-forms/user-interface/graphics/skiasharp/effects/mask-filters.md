---
title: SkiaSharp 마스크 필터
description: 마스크 필터를 사용 하 여 흐림 효과 기타 알파 효과 만드는 방법에 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 940422A1-8BC0-4039-8AD7-26C61320F858
author: davidbritch
ms.author: dabritch
ms.date: 08/27/2018
ms.openlocfilehash: 36a8b5c32261d4f508c82feea1e6127574db6a20
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70198252"
---
# <a name="skiasharp-mask-filters"></a>SkiaSharp 마스크 필터

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

마스크 필터는 기 하 도형 및 그래픽 개체의 알파 채널을 조작 하는 효과입니다. 마스크 필터를 사용 하려면 설정 합니다 [ `MaskFilter` ](xref:SkiaSharp.SKPaint.MaskFilter) 속성을 `SKPaint` 형식의 개체에 [ `SKMaskFilter` ](xref:SkiaSharp.SKMaskFilter) 중 하나를 호출 하 여 만든를 `SKMaskFilter` 정적 메서드.

마스크 필터에 익숙해지는 가장 좋은 방법은 이러한 정적 메서드를 사용 하 여 실험가 있습니다. 가장 유용한 마스크 필터는 흐림 효과 만듭니다.

![예제에서는 흐림](mask-filters-images/MaskFilterExample.png "예제 흐리게 표시")

이 문서에 설명 된 유일한 마스크 필터 기능입니다. 에 대 한 다음 문서 [ **SkiaSharp 이미지 필터** ](image-filters.md) 하는 것이 좋습니다는 흐리게 효과 대해서도 설명 합니다. 

정적 [ `SKMaskFilter.CreateBlur` ](xref:SkiaSharp.SKMaskFilter.CreateBlur(SkiaSharp.SKBlurStyle,System.Single)) 메서드는 다음 구문을 가집니다.

```csharp
public static SKMaskFilter CreateBlur (SKBlurStyle blurStyle, float sigma);
```

오버 로드 흐리게 효과 만들고 기타 그래픽 개체를 사용 하 여 설명 하는 영역에서 흐리게 표시 하지 않으려면 사각형을 사용 하는 알고리즘에 대 한 플래그를 지정할 수 있습니다.

[`SKBlurStyle`](xref:SkiaSharp.SKBlurStyle) 다음 멤버로 구성 된 열거형:

- `Normal`
- `Solid`
- `Outer`
- `Inner`

이러한 스타일의 효과 아래 예제에 표시 됩니다. `sigma` 흐림 효과의 범위를 지정 하는 매개 변수입니다. Skia의 이전 버전에서는 흐림 효과의 범위는 반지름 값을 사용 하 여 표시 되었습니다. 정적 반지름 값 이면 응용 프로그램에 대 한 것이 좋습니다 [ `SKMaskFilter.ConvertRadiusToSigma` ](xref:SkiaSharp.SKMaskFilter.ConvertRadiusToSigma*) 을 서로 변환할 수 있는 메서드. 메서드는 radius 0.57735 곱하고 0.5를 추가 합니다.

**마스크 실험 흐리게** 페이지에 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플을 사용 하면 흐리게 스타일 및 시그마 값을 실험할 수 있습니다. XAML 파일은는 `Picker` 4 개를 사용 하 여 `SKBlurStyle` 열거형 멤버와 `Slider` 시그마 값을 지정 하 합니다.

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

코드 숨김 파일을 만들려면 해당 값을 사용는 `SKMaskFilter` 개체 및로 설정 합니다 `MaskFilter` 의 속성은 `SKPaint` 개체입니다. 이 `SKPaint` 개체는 텍스트 문자열과 비트맵을 그리는 데 사용 합니다.

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

여기는 iOS, Android 및 유니버설 Windows 플랫폼 (UWP) 사용 하 여를 실행 하는 프로그램을 `Normal` 스타일 및 증가 흐리게 `sigma` 수준:

[![흐린 실험-보통 마스크](mask-filters-images/MaskBlurExperiment-Normal.png "마스크 흐리게 보통 실험")](mask-filters-images/MaskBlurExperiment-Normal-Large.png#lightbox)

비트맵의 가장자리에만 영향을 받는 흐리게 효과 확인할 수 있습니다. `SKMaskFilter` 클래스는 전체 비트맵 이미지를 흐리게 표시 하려는 경우 사용할 올바른 적용 하지 않습니다. 사용 하려는 합니다 [ `SKImageFilter` ](xref:SkiaSharp.SKImageFilter) 클래스에 다음 문서에 설명 된 대로 [ **SkiaSharp 이미지 필터**](image-filters.md)합니다.

텍스트의 값을 늘리면 사용 하 여 더 모호 합니다 `sigma` 인수입니다. 이 프로그램을 사용 하 여 실험을 보면는 특정 `sigma` 값 흐리게 효과 Windows 10 데스크톱에서 더 극단적인 경우입니다. 이러한 차이 픽셀 밀도 모바일 장치에서 보다 데스크톱 모니터에 낮은 이며 따라서 텍스트 높이 픽셀 단위로 낮은 때문에 발생 합니다. `sigma` 값은 픽셀에서 흐림 정도 비례 따라서는 지정 된 `sigma` 값 효과 더 낮은 해상도 디스플레이에 좀 더 극단적인 합니다. 프로덕션 응용 프로그램을 원할 것을 계산 하는 `sigma` 그래픽의 크기에 비례 하는 값입니다. 

응용 프로그램에 잘 표시 되는 흐림 효과 수준에서 결정 하기 전까지 여러 값을 시도 합니다. 예를 들어, 합니다 **마스크 흐리게 실험** 페이지에서 설정 `sigma` 같이:

```csharp
sigma = paint.TextSize / 18;
paint.MaskFilter = SKMaskFilter.CreateBlur(blurStyle, sigma);
```

이제는 `Slider` 아무 효과가 있지만 흐림 효과의 수준이 플랫폼 간에 일치 합니다.

[![일관 된 흐림 효과 실험 마스크](mask-filters-images/MaskBlurExperiment-Consistent.png "일관성 있는 실험 흐림 효과 마스크 합니다.")](mask-filters-images/MaskBlurExperiment-Consistent-Large.png#lightbox)

모든 스크린샷에서 흐림 효과 사용 하 여 만든 지금 표시는 `SKBlurStyle.Normal` 열거형 멤버입니다. 다음 스크린샷은의 효과 보여 합니다 `Solid`, `Outer`, 및 `Inner` 스타일 흐리게 표시:

[![흐린 실험 마스크](mask-filters-images/MaskBlurExperiment.png "흐리게 실험 마스크")](mask-filters-images/MaskBlurExperiment-Large.png#lightbox)

IOS 스크린샷은 스타일을 `Solid` 보여 줍니다. 텍스트 문자는 여전히 검정 실선으로 표시 되 고 이러한 텍스트 문자 외부에 흐림 효과를 추가 합니다. 

가운데의 Android 스크린 샷에서는 다음과 같은 `Outer` 스타일을 보여 줍니다. 문자 스트로크 자체는 비트맵 처럼 제거 되며 텍스트 문자가 한 번 나타나는 빈 공간을 둘러쌉니다. 

오른쪽은 UWP 스크린샷에 `Inner` 스타일입니다. 흐리게 효과 일반적으로 텍스트 문자를 차지한 영역이 제한 됩니다.

합니다 [ **SkiaSharp 선형 그라데이션** ](shaders/linear-gradient.md#transparency-and-gradients) 설명 하는 문서를 **리플렉션 그라데이션** 텍스트 문자열의 반사를 모방 하기 위해 선형 그라데이션 및 변환을 사용 하는 프로그램:

[![리플렉션 그라데이션](shaders/linear-gradient-images/ReflectionGradient.png "리플렉션 그라데이션")](shaders/linear-gradient-images/ReflectionGradient-Large.png#lightbox)

합니다 **흐리게 리플렉션** 페이지 코드를 단일 문으로 추가:

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

리플 렉 트 된 텍스트의 텍스트 크기를 기반으로 하는 흐린 필터를 추가 하는 새 문은:

```csharp
paint.MaskFilter = SKMaskFilter.CreateBlur(SKBlurStyle.Normal, paint.TextSize / 36);
```

이 흐리게 필터를 사용 하면 리플렉션 보다 현실적인 보일 수 있습니다:

[![흐린 리플렉션](mask-filters-images/BlurryReflection.png "흐리게 리플렉션")](mask-filters-images/BlurryReflection-Large.png#lightbox)

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
