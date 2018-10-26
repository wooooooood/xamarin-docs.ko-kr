---
title: 분리 되지 않은 혼합 모드
description: 색상, 채도 또는 명도 변경 하려면 분리 되지 않은 혼합 모드를 사용 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97FA2730-87C0-4914-8C9F-C64A02CF9EEF
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 07df66e69186803e3322bd71a4b3b37710655de4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131891"
---
# <a name="the-non-separable-blend-modes"></a>분리 되지 않은 혼합 모드

문서에서 볼 수 있듯이 [ **SkiaSharp 분리 가능한 blend 모드**](separable.md), 분리 가능한 blend 모드를 개별적으로 빨강, 녹색 및 파란색 채널에서 작업을 수행 합니다. 분리 가능한 비 blend 모드 변환 되지 않습니다. 색의 색상, 채도 및 명도 수준으로 운영 하 여 분리 되지 않은 혼합 모드 흥미로운 방식으로 색을 변경할 수 있습니다.

![분리 가능한 비 샘플](non-separable-images/NonSeparableSample.png "분리 가능한 비 샘플")

## <a name="the-hue-saturation-luminosity-model"></a>색상-채도-명도 모델

분리 가능한 비 blend 모드를 이해 하려면 색상-채도-명도 모델에 대 한 색으로 대상 및 원본 픽셀을 처리 하는 데 필요한 됩니다. (명도 라고도 밝기입니다.)

HSL 색 모델 문서에서 설명한 [ **Xamarin.Forms를 사용 하 여 통합** ](../../basics/integration.md) 하 고 해당 문서의 예제 프로그램 HSL 색을 사용 하 여 실험을 허용 합니다. 만들 수 있습니다는 `SKColor` 색상, 채도 및 명도 값을 사용 하 여 정적을 사용 하 여 값 [ `SKColor.FromHsl` ](xref:SkiaSharp.SKColor.FromHsl*) 메서드.

색조 색의 기준 파장을 나타냅니다. 색상 값 범위는 0에서 360 및 가산적 및 무언가 감 주 순환: 빨간색은 값 0, 노란색 60, 녹색은 120, 녹청 180, 파란색은 240, 자홍 300 이며 주기 360에 빨간색으로 돌아갑니다.

주요 색상 없는 경우 &mdash; 예를 들어, 색은 흰색, 검은색 또는 회색 음영을 &mdash; 색상은 정의 되지 않은 하 고 일반적으로 0으로 설정 합니다. 

채도 값 범위는 0에서 100 하 고 색의 무결성을 나타낼 수 있습니다. 채도 값을 100 값 100 보다 grayish 되려면 색 보다 낮은 하는 동안 가장 고귀한 색이 됩니다. 회색 음영의 채도 값이 0이 발생합니다.

명도 (또는 밝기) 값을 어떻게 밝은 색이를 나타냅니다. 명도 값 0은 다른 설정과 관계 없이 검정입니다. 마찬가지로, 명도 값 100은 흰색입니다. 

HSL 값 (0, 100, 50)에 순수한 빨간색 상태인 RGB 값 (FF, 00, 00)입니다. HSL 값 (180, 100, 50)에 RGB 값 (00에서 FF, FF), 순수 녹청입니다. 채도 줄이면으로 주요 색상 구성 요소는 감소 하 고 다른 구성 요소는 증가 합니다. 0의 채도 수준에서 구성 요소를 모두 동일 하 고 색이 회색조입니다. 검정; 이동할 명도 감소 흰색으로 이동할 명도 늘립니다.

## <a name="the-blend-modes-in-detail"></a>세부 정보에서 혼합 모드

다른 혼합 모드와 같은 4 가지 분리 되지 않은 혼합 모드 (이 종종 비트맵 이미지)는 대상 및 소스, 자주 변경 되는 단색 또는 그라데이션 포함 됩니다. 원본과 대상에서 색상, 채도 및 명도 값을 결합 하는 혼합 모드:

| 혼합 모드   | 원본에서 구성 요소 | 대상에서 구성 요소 |
| ------------ | ---------------------- | --------------------------- |
| `Hue`        | 색상                    | 채도 명도   |
| `Saturation` | 채도             | 색상 및 명도          |
| `Color`      | 색상 및 채도     | 명도                  | 
| `Luminosity` | 명도             | 색상 및 채도          | 

W3C를 참조 하세요 [ **합성 하 고 수준 1 혼합** ](https://www.w3.org/TR/compositing-1/) 알고리즘에 대 한 사양입니다.

**분리 되지 않은 혼합 모드** 페이지에는 `Picker` 다음 중 하나를 선택 하려면 blend 모드 및 3 `Slider` HSL 색을 선택 하는 뷰:

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

공간 절약, 3 개를 `Slider` 뷰 프로그램의 사용자 인터페이스에서 식별 되지 않습니다. 순서는 색상, 채도 및 명도 해야 합니다. 두 `Label` 뷰 페이지의 맨 아래에 HSL 및 RGB 색 값을 표시 합니다.

코드 숨김 파일 비트맵 리소스 중 하나를 로드, 표시 하는 캔버스에서 가장 크게 다음 사각형을 사용 하 여 캔버스에 다룹니다. 사각형 색이 세 가지에 따라 `Slider` 뷰와 혼합 모드는 선택한은 `Picker`:

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

확인 프로그램 HSL 색 값을 3 개의 슬라이더에 의해 선택 된 것으로 표시 되지 않습니다. 대신, 색 값을 해당 슬라이더에서 만들고 사용 하 여는 [ `ToHsl` ](xref:SkiaSharp.SKColor.ToHsl*) 색상, 채도 및 명도 값을 수집 하는 방법입니다. 때문에 이것이 합니다 `FromHsl` 메서드는 HSL 색에서 내부적으로 저장 되는 RGB 색 변환는 `SKColor` 구조입니다. `ToHsl` 메서드 HSL RGB에서 변환 되지만 결과가 원래 값을 항상 수 없습니다. 

예를 들어 `FromHsl` 때문에 (0, 0, 0)의 RGB 색 (180, 50, 0) HSL 값을 변환 합니다 `Luminosity` 0입니다. `ToHsl` 메서드 색상 및 채도 값 관련이 없기 때문에 (0, 0, 0)의 RGB 색 HSL 색 (0, 0, 0)으로 변환 합니다. 더 나은 것이 프로그램을 사용 하는 경우 대신 슬라이더를 사용 하 여 지정한 프로그램을 사용 하는 HSL 색의 표현을 표시 합니다.

`SKBlendModes.Hue` blend 모드 대상의 채도 명도 수준을 유지 하면서 원본의 Hue 수준을 사용 합니다. 이 혼합 모드 테스트를 할 때 채도 명도 슬라이더 설정 해야 합니다를 0 이나 100이 아닌 이러한 경우 색상 정의 되어 있지 않으므로 고유 하 게 합니다.

[![분리 가능한 비 Blend 모드-Hue](non-separable-images/NonSeparableBlendModes-Hue.png "Hue 분리 되지 않은 혼합 모드")](non-separable-images/NonSeparableBlendModes-Hue-Large.png#lightbox)

경우 하 모든 항목 기능 빨강을 사용 하 여 (마찬가지로 왼쪽에 있는 iOS 스크린샷) 슬라이더를 0으로 설정 합니다. 이미지 없는 완전히 그렇다고 하지만 녹색 및 파랑의 합니다. 물론 여전히 회색조 결과 있습니다. 예를 들어, RGB 색을 나타내는 (40, 40, C0) HSL 색 (240, 50, 50)과 같습니다. 색상은 파란색 하지만 50의 채도 값 빨간색 및 녹색 구성 요소도 개 있음을 나타냅니다. 색상을 0으로 설정 된 경우 `SKBlendModes.Hue`, HSL 색은 (0, 50, 50), RGB 색 (C0, 40, 40). 여전히 파랑 및 녹색 구성 요소 있지만 이제 주요 구성 요소는 빨간색입니다.

`SKBlendModes.Saturation` blend 모드 색상 및 대상의 광도 사용 하 여 원본의 채도 수준을 결합 합니다. 같은 색상, 채도 제대로 정의 되지 않은 명도 0 이나 100입니다. 이론적으로 이러한 두 가지 극단적인 간의 명도 설정 작동 해야 합니다. 그러나 명도 설정 보다 더 많은 결과 영향을 줄 것 같습니다. 명도 50으로 설정 하 고 그림 채도 수준을 설정 하는 방법을 볼 수 있습니다.

[![분리 가능한 비 Blend 모드-채도](non-separable-images/NonSeparableBlendModes-Saturation.png "채도 분리 되지 않은 혼합 모드")](non-separable-images/NonSeparableBlendModes-Saturation-Large.png#lightbox)

이 혼합 모드를 사용 하 여 색 두기 이미지의 채도 높이기 위해 또는 회색조만 이루어진 결과 이미지 (예: 왼쪽에 있는 iOS 스크린샷) 0 채도 줄일 수 있습니다.

`SKBlendModes.Color` blend 모드 대상의 광도 유지 되지만 색상 및 채도 소스를 사용 합니다. 마찬가지로 즉, 기계의 사이 명도 슬라이더의 모든 설정이 작동 해야 합니다. 

[![분리 가능한 비 Blend 모드-색](non-separable-images/NonSeparableBlendModes-Color.png "분리 되지 않은 혼합 모드-색")](non-separable-images/NonSeparableBlendModes-Color-Large.png#lightbox)

이 혼합 모드 응용 프로그램을 곧 표시 됩니다.

마지막으로, 합니다 `SKBlendModes.Luminosity` 혼합 모드의 반대는 `SKBlendModes.Color`합니다. 해당 색상 및 대상의 채도 유지 하지만 원본의 명도 사용 합니다. `Luminosity` blend 모드는 일괄 처리의 가장 알 수 없는: The 색상 및 채도 슬라이더 이미지에 영향을 줍니다 하지만 보통 명도에 이미지는 고유 합니다.

[![분리 가능한 비 Blend 모드-명도](non-separable-images/NonSeparableBlendModes-Luminosity.png "명도 분리 되지 않은 혼합 모드")](non-separable-images/NonSeparableBlendModes-Luminosity-Large.png#lightbox)

이론적으로 이미지의 광도 늘리거나 해야 더 밝게 또는 음영이 짙을 수록 해당 합니다. 흥미롭게도 [명도 예제에는 Skia **SkBlendMode 참조** ](https://skia.org/user/api/SkBlendMode_Reference#Luminosity) 와 매우 유사 합니다.

것 일반적으로 전체 대상 이미지에 적용 한 색을 구성 하는 소스를 사용 하 여 분리 되지 않은 혼합 모드 중 하나를 사용 해야 하는 경우입니다. 효과가 너무 유용합니다. 이미지의 한 부분에 영향을 제한 해야 합니다. 이 경우 원본 transparancy, 통합 아마도 하는 또는 아마도 원본 작은 그래픽으로 제한 됩니다.

## <a name="a-matte-for-a-separable-mode"></a>분리 가능한 모드에 대 한 매트

여기에 리소스로 포함 된 비트맵 중 하나인 합니다 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 샘플입니다. 파일 이름이 **Banana.jpg**:

![바나나 Monkey](non-separable-images/Banana.jpg "Banana Monkey")

바나나만 포함 하는 매트를 두는 것이 가능 합니다. 리소스에도를 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 샘플입니다. 파일 이름이 **BananaMatte.png**:

![바나나 매트](non-separable-images/BananaMatte.png "Banana 매트")

검은색 banana 셰이프 외에도 비트맵의 나머지는 투명 합니다.

합니다 **파란색 Banana** 색상 및 채도 monkey를 누른 채로 바나나 변경할 수 있지만 아무 이미지에서를 변경 하려면 페이지에서 해당 매트를 사용 합니다. 

다음과에서 `BlueBananaPage` 클래스를 **Banana.jpg** 필드로 비트맵 로드 됩니다. 생성자 로드를 **BananaMatte.png** 으로 비트맵의 `matteBitmap` 개체가 아니라 해당 생성자에 관계 없이 해당 개체를 유지 하지 않습니다. 세 번째 비트맵의 이름이 아니라 `blueBananaBitmap` 만들어집니다. 합니다 `matteBitmap` 에 그려집니다 `blueBananaBitmap` 뒤에 `SKPaint` 사용 하 여 해당 `Color` 파랑으로 설정 및 해당 `BlendMode` 로 `SKBlendMode.SrcIn`합니다. `blueBananaBitmap` 대부분 투명 하지만 바나나 solid 순수 파란색 이미지를 사용 하 여 유지 됩니다.

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

`PaintSurface` 처리기는 banana 보유 monkey를 사용 하 여 비트맵을 그립니다. 이 코드 뒤에 표시 `blueBananaBitmap` 사용 하 여 `SKBlendMode.Color`입니다. 바나나 화면을 통해 각 픽셀의 색상 및 채도 240 색상 값 100의 채도 값을 해당 하는 파란색으로 바뀝니다. 그러나 광도, 동일 하 게 유지, 즉, 새 색에도 불구 하 고 현실적인 텍스처는 banana 계속:

[![바나나 파란색](non-separable-images/BlueBanana.png "Banana 파란색")](non-separable-images/BlueBanana-Large.png#lightbox)

Blend 모드를 변경해 보세요 `SKBlendMode.Saturation`합니다. banana 노란색 남아 있지만 더 강 할수록 노란색입니다. 실제 응용 프로그램에서는이 banana 파란색 설정 보다 더 바람직합니다 효과 수 있습니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)