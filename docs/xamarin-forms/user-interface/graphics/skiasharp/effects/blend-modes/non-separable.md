---
제목: "분리 되지 않은 혼합 모드" 설명: "색상, 채도 또는 광도를 변경 하는 데 분리 되지 않은 혼합 모드를 사용 합니다."
ms. prod: xamarin. 기술: xamarin-skiasharp assetid: 97FA2730-87C0-4914-8C9F-C64A02CF9EEF author: davidbritch: dabritch: 08/23/2018:-loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="the-non-separable-blend-modes"></a>분리 되지 않은 혼합 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp 분리 가능 blend [**모드**](separable.md)문서에서 볼 수 있듯이 분리 가능 blend 모드는 빨강, 녹색 및 파랑 채널에 대해 별도로 작업을 수행 합니다. 분리 되지 않은 혼합 모드는 그렇지 않습니다. 색, 채도 및 광도 수준에서 작동 하는 경우 분리 되지 않은 혼합 모드는 다음과 같은 다양 한 방식으로 색을 변경할 수 있습니다.

![분리 가능 하지 않은 샘플](non-separable-images/NonSeparableSample.png "분리 가능 하지 않은 샘플")

## <a name="the-hue-saturation-luminosity-model"></a>색상-채도-광도 모델

분리 되지 않은 혼합 모드를 이해 하려면 대상 및 원본 픽셀을 색-채도-광도 모델에서 색으로 처리 해야 합니다. (명도를 밝기 라고도 합니다.)

HSL 색 모델에 대해서는이 문서의 [**통합 Xamarin.Forms **](../../basics/integration.md) 및 샘플 프로그램에서 hsl 색을 사용 하 여 실험을 수행할 수 있습니다. 정적 메서드를 사용 `SKColor` 하 여 색상, 채도 및 명도 값을 사용 하 여 값을 만들 수 있습니다 [`SKColor.FromHsl`](xref:SkiaSharp.SKColor.FromHsl*) .

색상은 색의 기준 wavelength 나타냅니다. 색상 값의 범위는 0에서 360 사이이 고 덧셈 및 subtractive 주를 순환 합니다. 빨강은 값 0, 노랑, 60, 녹색은 120, 사이안은 180, blue is 240, 자홍은 300, 주기가 다시 빨간색으로 바뀝니다.

기준 색이 없는 경우 &mdash; 색은 흰색 또는 검은색 이거나 회색 음영 인 경우 색상은 정의 되지 &mdash; 않으며 일반적으로 0으로 설정 됩니다. 

채도 값의 범위는 0에서 100이 고 색의 순도를 나타낼 수 있습니다. 채도 값 100은 purest 색 이지만 100 보다 작은 값으로 인해 색이 더 grayish 됩니다. 채도 값이 0 이면 회색 음영이 생성 됩니다.

명도 (또는 명도) 값은 색의 밝기를 나타냅니다. 광도 값 0은 다른 설정에 관계 없이 검정입니다. 마찬가지로 광도 값 100은 흰색입니다. 

HSL 값 (0, 100, 50)은 순수 빨강의 RGB 값 (FF, 00, 00)입니다. HSL 값 (180, 100, 50)은 RGB 값 (00, FF, FF), 순수한 녹청입니다. 채도가 감소 함에 따라 기준 색 구성 요소가 감소 하 고 다른 구성 요소가 증가 합니다. 채도 수준 0에서 모든 구성 요소는 동일 하 고 색은 회색 음영입니다. 검정으로 이동 하려면 밝기 줄이기 흰색으로 이동 하려면 밝기를 늘립니다.

## <a name="the-blend-modes-in-detail"></a>혼합 모드 세부 정보

다른 blend 모드와 마찬가지로 네 가지 분리 안 혼합 모드에는 대상 (종종 비트맵 이미지)과 원본 (종종 단일 색 또는 그라데이션)이 포함 됩니다. Blend 모드는 대상 및 원본에서 색상, 채도 및 명도 값을 결합 합니다.

| Blend 모드   | 원본에서 구성 요소 | 대상의 구성 요소 |
| ------------ | ---------------------- | --------------------------- |
| `Hue`        | 색상                    | 채도 및 명도   |
| `Saturation` | 채도             | 색상 및 광도          |
| `Color`      | 색상 및 채도     | 밝기                  | 
| `Luminosity` | 밝기             | 색상 및 채도          | 

알고리즘에 대 한 W3C [**합성 및 혼합 수준 1**](https://www.w3.org/TR/compositing-1/) 지정을 참조 하세요.

분리 **되지 않은 혼합 모드** 페이지에는 `Picker` 다음 blend 모드 중 하나를 선택 하 고 세 개의 보기를 선택 하 여 `Slider` HSL 색을 선택할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaviews="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.NonSeparableBlendModesPage"
             Title="Non-Separable Blend Modes">

    <StackLayout>
        <skiaviews:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="blendModePicker"
                Title="Blend Mode"
                Margin="10, 0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKBlendMode}">
                    <x:Static Member="skia:SKBlendMode.Hue" />
                    <x:Static Member="skia:SKBlendMode.Saturation" />
                    <x:Static Member="skia:SKBlendMode.Color" />
                    <x:Static Member="skia:SKBlendMode.Luminosity" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Slider x:Name="hueSlider"
                Maximum="360"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="satSlider"
                Maximum="100"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="lumSlider"
                Maximum="100"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <StackLayout Orientation="Horizontal">
            <Label x:Name="hslLabel"
                   HorizontalOptions="CenterAndExpand" />

            <Label x:Name="rgbLabel"
                   HorizontalOptions="CenterAndExpand" />

        </StackLayout>
    </StackLayout>
</ContentPage>
```

공간을 절약 하기 위해 세 가지 `Slider` 보기는 프로그램의 사용자 인터페이스에서 식별 되지 않습니다. 순서는 색상, 채도 및 광도 임을 명심 해야 합니다. `Label`페이지 맨 아래에 있는 두 개의 보기에는 HSL 및 RGB 색 값이 표시 됩니다.

코드를 사용 하는 파일은 비트맵 리소스 중 하나를 로드 하 고 캔버스에서 최대한 크게 표시 한 다음 캔버스에 캔버스를 포함 합니다. 사각형 색은 세 가지 보기를 기반으로 `Slider` 하며 blend 모드는에서 선택한 것입니다 `Picker` .

```csharp
public partial class NonSeparableBlendModesPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(NonSeparableBlendModesPage),
                        "SkiaSharpFormsDemos.Media.Banana.jpg");
    SKColor color;

    public NonSeparableBlendModesPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs e)
    {
        // Calculate new color based on sliders
        color = SKColor.FromHsl((float)hueSlider.Value,
                                (float)satSlider.Value,
                                (float)lumSlider.Value);

        // Use labels to display HSL and RGB color values
        color.ToHsl(out float hue, out float sat, out float lum);

        hslLabel.Text = String.Format("HSL = {0:F0} {1:F0} {2:F0}",
                                      hue, sat, lum);

        rgbLabel.Text = String.Format("RGB = {0:X2} {1:X2} {2:X2}",
                                      color.Red, color.Green, color.Blue);

        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);

        // Get blend mode from Picker
        SKBlendMode blendMode =
            (SKBlendMode)(blendModePicker.SelectedIndex == -1 ?
                                        0 : blendModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Color = color;
            paint.BlendMode = blendMode;
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

프로그램에는 세 개의 슬라이더에서 선택한 대로 HSL 색 값이 표시 되지 않습니다. 대신 이러한 슬라이더에서 색 값을 만든 다음 메서드를 사용 하 여 색상 [`ToHsl`](xref:SkiaSharp.SKColor.ToHsl*) , 채도 및 명도 값을 가져옵니다. 이는 `FromHsl` 메서드가 HSL 색을 구조에 내부적으로 저장 된 RGB 색으로 변환 하기 때문입니다 `SKColor` . `ToHsl`메서드는 RGB에서 HSL로 변환 하지만 결과는 항상 원래 값이 아닙니다. 

예를 들어 `FromHsl` 는 0 이므로 HSL 값 (180, 50, 0)을 RGB 색 (0, 0, 0)으로 변환 합니다 `Luminosity` . `ToHsl`메서드는 RGB 색 (0, 0, 0)을 HSL 색 (0, 0, 0)으로 변환 합니다 .이 경우 색상 및 채도 값은 관련이 없기 때문입니다. 이 프로그램을 사용 하는 경우 슬라이더를 사용 하 여 지정 하는 것이 아니라 프로그램에서 사용 하는 HSL 색의 표현을 확인 하는 것이 좋습니다.

`SKBlendModes.Hue`Blend 모드는 대상의 채도 및 광도 수준을 유지 하면서 원본의 색상 수준을 사용 합니다. 이 blend 모드를 테스트할 때 채도 및 명도 슬라이더는 0 또는 100이 아닌 다른 것으로 설정 해야 합니다. 이러한 경우에는 해당 색상이 고유 하 게 정의 되지 않습니다.

[![분리 되지 않은 혼합 모드-색상](non-separable-images/NonSeparableBlendModes-Hue.png "분리 되지 않은 혼합 모드-색상")](non-separable-images/NonSeparableBlendModes-Hue-Large.png#lightbox)

을 사용 하는 경우 왼쪽에 있는 iOS 스크린샷 처럼 슬라이더를 0으로 설정 하면 모든 것이 reddish로 설정 됩니다. 그러나 이미지에 녹색 및 파란색이 모두 없음을 의미 하는 것은 아닙니다. 여전히 결과에 회색 음영이 있습니다. 예를 들어 RGB 색 (40, 40, C0)은 HSL 색 (240, 50, 50)과 동일 합니다. 색상은 파란색 이지만 채도 값 50는 빨강 및 녹색 구성 요소도 있음을 나타냅니다. 색조가를 사용 하 여 0으로 설정 된 경우 `SKBlendModes.Hue` HSL 색은 (0, 50, 50) RGB 색 (c0, 40, 40)입니다. 여전히 파란색 및 녹색 구성 요소가 있지만 이제 주요 구성 요소는 빨강입니다.

`SKBlendModes.Saturation`Blend 모드는 원본의 채도 수준을 대상의 색상 및 광도와 결합 합니다. 색조로 색과 마찬가지로, 명도가 0 또는 100 인 경우에는 채도가 잘 정의 되지 않습니다. 이론적으로는 두 극단 사이의 광도 설정이 작동 해야 합니다. 그러나 광도 설정은 결과에 더 많은 영향을 주는 것 처럼 보입니다. 밝기를 50로 설정 하 고 사진의 채도 수준을 설정 하는 방법을 확인할 수 있습니다.

[![분리 되지 않은 혼합 모드-채도](non-separable-images/NonSeparableBlendModes-Saturation.png "분리 되지 않은 혼합 모드-채도")](non-separable-images/NonSeparableBlendModes-Saturation-Large.png#lightbox)

이 blend 모드를 사용 하 여 흐릿한 이미지의 색 채도를 높이 거 나 회색 음영 으로만 구성 된 결과 이미지에 대해 채도를 0 (왼쪽의 iOS 스크린샷)에서 0으로 낮출 수 있습니다.

`SKBlendModes.Color`Blend 모드는 대상의 광도를 유지 하지만 원본의 색상 및 채도를 사용 합니다. 즉, 극단 사이에 있는 광도 슬라이더의 모든 설정이 작동 하는 것을 의미 합니다. 

[![분리 되지 않은 혼합 모드-색](non-separable-images/NonSeparableBlendModes-Color.png "분리 되지 않은 혼합 모드-색")](non-separable-images/NonSeparableBlendModes-Color-Large.png#lightbox)

이 blend 모드의 응용 프로그램은 곧 표시 됩니다.

마지막으로 `SKBlendModes.Luminosity` blend 모드는와 반대입니다 `SKBlendModes.Color` . 대상의 색조와 채도를 유지 하지만 소스의 광도를 사용 합니다. `Luminosity`Blend 모드는 배치의 가장 중요 하지 않습니다. 색상 및 채도 슬라이더는 이미지에 영향을 주지만 중간 광도 에서도 이미지는 고유 하지 않습니다.

[![분리 되지 않은 혼합 모드-광도](non-separable-images/NonSeparableBlendModes-Luminosity.png "분리 되지 않은 혼합 모드-광도")](non-separable-images/NonSeparableBlendModes-Luminosity-Large.png#lightbox)

이론적으로 이미지의 광도를 늘리거나 줄이는 것은 더 어둡거나 더 어두워집니다. 흥미로운 것은, c # [ **참조** 의 명도 예제](https://skia.org/user/api/SkBlendMode_Reference#Luminosity) 는 매우 유사 합니다.

일반적으로 전체 대상 이미지에 적용 되는 단일 색으로 구성 된 소스가 있는 분리 되지 않은 혼합 모드 중 하나를 사용 하는 경우가 아닙니다. 효과가 너무 많습니다. 이미지의 한 부분으로 효과를 제한 하려고 합니다. 이 경우 소스는 transparancy을 통합 하거나 소스를 더 작은 그래픽으로 제한할 수 있습니다.

## <a name="a-matte-for-a-separable-mode"></a>분리 가능 모드의 무광택

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 리소스로 포함 된 비트맵 중 하나는 다음과 같습니다. 파일 이름은 **Banana.jpg**입니다.

![바나나 원숭이](non-separable-images/Banana.jpg "바나나 원숭이")

바나나만 포함 하는 매트를 만들 수 있습니다. [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 리소스 이기도 합니다. 파일 이름은 **BananaMatte.png**입니다.

![바나나 무광택](non-separable-images/BananaMatte.png "바나나 무광택")

Black 바나나 셰이프를 제외 하 고 나머지 비트맵은 투명 합니다.

**파란색 바나나** 페이지는 해당 매트를 사용 하 여 원숭이가 보유 하 고 있는 바나나의 색조와 채도를 변경 하지만 이미지에서 다른 아무 것도 변경 하지 않습니다. 

다음 클래스에서는 `BlueBananaPage` **Banana.jpg** 비트맵이 필드로 로드 됩니다. 생성자는 **BananaMatte.png** 비트맵을 `matteBitmap` 개체로 로드 하지만 해당 개체를 생성자 밖으로 유지 하지는 않습니다. 대신 라는 세 번째 비트맵이 `blueBananaBitmap` 생성 됩니다. 는로 `matteBitmap` 설정 되 `blueBananaBitmap` `SKPaint` 고가 `Color` blue로 설정 되 고가 `BlendMode` 로 설정 된 `SKBlendMode.SrcIn` 이 그려집니다. 는 `blueBananaBitmap` 주로 투명 하 게 유지 되지만 바나나의 진한 순수한 파랑 이미지를 사용 합니다.

```csharp
public class BlueBananaPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BlueBananaPage),
        "SkiaSharpFormsDemos.Media.Banana.jpg");

    SKBitmap blueBananaBitmap;

    public BlueBananaPage()
    {
        Title = "Blue Banana";

        // Load banana matte bitmap (black on transparent)
        SKBitmap matteBitmap = BitmapExtensions.LoadBitmapResource(
            typeof(BlueBananaPage),
            "SkiaSharpFormsDemos.Media.BananaMatte.png");

        // Create a bitmap with a solid blue banana and transparent otherwise
        blueBananaBitmap = new SKBitmap(matteBitmap.Width, matteBitmap.Height);

        using (SKCanvas canvas = new SKCanvas(blueBananaBitmap))
        {
            canvas.Clear();
            canvas.DrawBitmap(matteBitmap, new SKPoint(0, 0));

            using (SKPaint paint = new SKPaint())
            {
                paint.Color = SKColors.Blue;
                paint.BlendMode = SKBlendMode.SrcIn;
                canvas.DrawPaint(paint);
            }
        }

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

        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);

        using (SKPaint paint = new SKPaint())
        {
            paint.BlendMode = SKBlendMode.Color;
            canvas.DrawBitmap(blueBananaBitmap, 
                              info.Rect, 
                              BitmapStretch.Uniform, 
                              paint: paint);
        }
    }
}
```

`PaintSurface`처리기는 바나나을 보유 하는 원숭이를 사용 하 여 비트맵을 그립니다. 이 코드 뒤에는가 표시 됩니다 `blueBananaBitmap` `SKBlendMode.Color` . 바나나의 표면에서 각 픽셀의 색상 및 채도는 색상 240에 해당 하 고 채도 값은 100 인 단색 파란색으로 바뀝니다. 그러나 광도는 동일 하 게 유지 됩니다. 즉, 바나나는 새 색에도 불구 하 고 실제 질감이 계속 유지 됩니다.

[![파란색 바나나](non-separable-images/BlueBanana.png "파란색 바나나")](non-separable-images/BlueBanana-Large.png#lightbox)

Blend 모드를로 변경해 보세요 `SKBlendMode.Saturation` . 바나나 노란색으로 유지 되지만 더 강한 노랑입니다. 실제 응용 프로그램에서는 바나나 blue를 설정 하는 것 보다 더 적합할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
