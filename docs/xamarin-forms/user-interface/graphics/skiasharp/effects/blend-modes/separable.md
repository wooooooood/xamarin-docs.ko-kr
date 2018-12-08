---
title: 분리 가능한 blend 모드
description: 빨강, 녹색 및 파랑 색을 변경 하려면 분리 가능한 혼합 모드를 사용 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 66D1A537-A247-484E-B5B9-FBCB7838FBE9
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 594e98230d4f4bd8aca27f92f4544f8c59b5f0a2
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061457"
---
# <a name="the-separable-blend-modes"></a>분리 가능한 blend 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

이 문서에서 볼 수 있듯이 [ **SkiaSharp Porter 임신 blend 모드**](porter-duff.md), Porter 임신 blend 모드는 일반적으로 클리핑 작업을 수행 합니다. 분리 가능한 blend 모드는 다릅니다. 분리 가능한 모드는 이미지의 개별 빨강, 녹색 및 파랑 색 구성 요소를 변경합니다. 분리 가능한 blend 모드 색의 빨강, 녹색 및 파랑 조합 흰색인 실제로 보여 주기 위해 혼합할 수 있습니다.

![기본 색](separable-images/SeparableSample.png "기본 색")

## <a name="lighten-and-darken-two-ways"></a>두 가지 방법으로 어둡게 및 밝게 

일반적으로 어느 정도 비트맵이 너무 어둡게 또는 밝게 너무 합니다. imabe 어둡게 또는 밝게 분리 가능한 blend 모드를 사용할 수 있습니다.  분리 가능 혼합 모드의 두 실제로 [ `SKBlendMode` ](xref:SkiaSharp.SKBlendMode) 열거형 라고 `Lighten` 및 `Darken`합니다. 

이러한 두 가지 모드에서 보여 합니다 **어둡게 및 밝게** 페이지입니다. XAML 파일을 두 개를 인스턴스화하고 `SKCanvasView` 개체와 두 개의 `Slider` 뷰:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.LightenAndDarkenPage"
             Title="Lighten and Darken">
    <StackLayout>
        <skia:SKCanvasView x:Name="lightenCanvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="lightenSlider"
                Margin="10"
                ValueChanged="OnSliderValueChanged" />

        <skia:SKCanvasView x:Name="darkenCanvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="darkenSlider"
                Margin="10"
                ValueChanged="OnSliderValueChanged" />
    </StackLayout>
</ContentPage>
```

첫 번째 `SKCanvasView` 하 고 `Slider` 시연 `SKBlendMode.Lighten` 하 고 두 번째 쌍은 보여 줍니다 `SKBlendMode.Darken`합니다. 두 개의 `Slider` 뷰는 동일한 공유 `ValueChanged` 처리기 및 두 개의 `SKCanvasView` 동일한 공유 `PaintSurface` 처리기입니다. 개체는 모두 이벤트 처리기 검사 이벤트를 발생 합니다.

```csharp
public partial class LightenAndDarkenPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                typeof(SeparableBlendModesPage),
                "SkiaSharpFormsDemos.Media.Banana.jpg");

    public LightenAndDarkenPage ()
    {
        InitializeComponent ();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if ((Slider)sender == lightenSlider)
        {
            lightenCanvasView.InvalidateSurface();
        }
        else
        {
            darkenCanvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find largest size rectangle in canvas
        float scale = Math.Min((float)info.Width / bitmap.Width,
                               (float)info.Height / bitmap.Height);
        SKRect rect = SKRect.Create(scale * bitmap.Width, scale * bitmap.Height);
        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Display bitmap
        canvas.DrawBitmap(bitmap, rect);

        // Display gray rectangle with blend mode
        using (SKPaint paint = new SKPaint())
        {
            if ((SKCanvasView)sender == lightenCanvasView)
            {
                byte value = (byte)(255 * lightenSlider.Value);
                paint.Color = new SKColor(value, value, value);
                paint.BlendMode = SKBlendMode.Lighten;
            }
            else
            {
                byte value = (byte)(255 * (1 - darkenSlider.Value));
                paint.Color = new SKColor(value, value, value);
                paint.BlendMode = SKBlendMode.Darken;
            }

            canvas.DrawRect(rect, paint);
        }
    }
}
```

`PaintSurface` 처리기 비트맵에 대 한 적합 한 사각형을 계산 합니다. 해당 비트맵을 표시 하 고 다음 사용 하 여 비트맵 사각형을 표시 하는 처리기를 `SKPaint` 개체를 해당 `BlendMode` 속성으로 설정 `SKBlendMode.Lighten` 또는 `SKBlendMode.Darken`합니다. 합니다 `Color` 속성을 기반으로 하는 회색 음영을 `Slider`합니다. 에 대 한 합니다 `Lighten` 모드, 색 범위가 검정에서을 흰색으로 대해서는 `Darken` 흰색에서 검은색 범위의 모드입니다.

왼쪽에서 오른쪽 스크린샷을 표시 점점 더 큰 `Slider` 위쪽 이미지 밝은 가져오고 아래쪽 이미지 가져옵니다 음영이 짙을 수록 값:

[![어둡게 및 밝게](separable-images/LightenAndDarken.png "어둡게 및 밝게")](separable-images/LightenAndDarken-Large.png#lightbox)

이 프로그램 방식 분리 가능한 blend 모드 사용 되는 일반적인 방법을 보여 줍니다: 대상이 비트맵 자주 일종의 이미지입니다. 원본이 사용 하 여 표시 사각형을 `SKPaint` 개체를 해당 `BlendMode` 분리 가능한 blend 모드를 설정 하는 속성입니다. 사각형 (이므로 여기) 단색 수 또는 그라데이션 합니다. 투명도 _되지_ 분리 가능한 blend 모드를 사용 하 여 일반적으로 사용 됩니다.

이 프로그램을 사용 하 여 실험 시에 이러한 두 혼합 모드 밝게 및 이미지를 균일 하 게 어둡게 하지 않는 알 수 있습니다. 대신는 `Slider` 일종의 임계값을 설정 하는 것 같습니다. 예를 들어을 늘리면 합니다 `Slider` 에 대 한는 `Lighten` 모드 이미지의 어두운 영역 가져오기 light 먼저 밝은 영역을 동일 하 게 유지 하는 동안.

에 대 한는 `Lighten` 모드 대상 픽셀 (Dr Dg, Db)의 RGB 색 값 이며 원본 픽셀 색 (Sr Sg, Sb), 출력은 다음 (또는 Og, Ob) 다음과 같이 계산 합니다.

 = Max (Dr, Sr) 또는 Og (Dg, Sg) 최대 Ob = = max (Db, Sb)

이와 별도로, 빨강, 녹색 및 파랑에 대 한 결과 원본과 대상의 큽니다. 이 대상의 어두운 영역을 먼저 밝게의 효과 생성 합니다.

`Darken` 모드는 비슷한 점을 제외 하 고 결과 원본과 대상의 가장 작은 수입니다.

 Min (Dr, Sr) Og = 또는 = min (Dg, Sg) Ob (Db, Sb) min =

빨강, 녹색 및 파랑 구성 요소는 각각 별도로 처리는 이러한 모드를 혼합 하는 이유는 라고 합니다 _분리 가능한_ 혼합 모드입니다. 이러한 이유로 약어 **Dc** 하 고 **Sc** 대상 및 소스 색을 사용할 수 있으며 계산 각 빨강, 녹색 및 파랑 구성 요소를 개별적으로 적용할 것으로 간주 됩니다.

다음 표에서 수행할 작업의 간략 한 설명 사용 하 여 모든 분리 가능한 blend 모드를 보여 줍니다. 두 번째 열 변하지를 생성 하는 소스 색을 표시 합니다.

| 혼합 모드   | 변경 안 함 | 작업 |
| ------------ | --------- | --------- |
| `Plus`       | 검정     | 색을 추가 하 여 밝게: Sc + Dc |
| `Modulate`   | 하얀     | 어둡게 색을 곱하여: Sc· Dc | 
| `Screen`     | 검정     | 보완 제품을 보완 합니다: Sc + Dc &ndash; Sc· Dc |
| `Overlay`    | 회색      | 역 `HardLight` |
| `Darken`     | 하얀     | 최소 색: min (Sc, Dc) |
| `Lighten`    | 검정     | 색의 최대: max (Sc, Dc) |
| `ColorDodge` | 검정     | 원본에 따라 대상 어둡게 |
| `ColorBurn`  | 하얀     | 원본에 따라 대상 어둡게 | 
| `HardLight`  | 회색      | 강한 집중 미치는 비슷합니다 |
| `SoftLight`  | 회색      | 소프트 추천의 결과 비슷하게 | 
| `Difference` | 검정     | 음영이 짙을 수록 더 밝은에서 뺍니다: Abs (Dc &ndash; Sc) | 
| `Exclusion`  | 검정     | 유사한 `Difference` 하지만 낮은 대비 |
| `Multiply`   | 하얀     | 어둡게 색을 곱하여: Sc· Dc |

W3C에서 자세한 알고리즘을 찾을 수 있습니다 [ **합성 하 고 수준 1 혼합** ](https://www.w3.org/TR/compositing-1/) 사양과 Skia [ **SkBlendMode 참조** ](https://skia.org/user/api/SkBlendMode_Reference)이지만 이러한 두 원본의 표기법 같지 않습니다. 에 유의 `Plus` Porter 임신 blend 모드로, 일반적으로 간주 됩니다 및 `Modulate` W3C 사양과의 일부가 아닙니다.

원본 투명 인 경우 다음 모든 분리에 대 한 혼합 모드 제외 하 고 `Modulate`, 혼합 모드에 영향을 주지 않습니다. 앞에서 살펴본 것 처럼는 `Modulate` blend 모드 곱하기의 알파 채널을 통합 합니다. 그렇지 않으면 `Modulate` 것과 동일한 효과가 `Multiply`합니다. 

라는 두 가지 모드를 알 수 있습니다 `ColorDodge` 고 `ColorBurn`입니다. 단어 _닷지_ 하 고 _굽기_ photographic 암실 사례에서 발생 합니다. 확대기 빛을 비추는 것과 음수를 통해 사진 인쇄를 수 있습니다. 없는 light를 사용 하 여 인쇄는 흰색입니다. 인쇄는 음영이 짙을 수록 더 긴 기간에 대 한 인쇄에 자세한 light 떨어지면 가져옵니다. 인쇄 결정권자는 늦는 해당 영역 밝은 print의 특정 부분에서 빛의 일부를 차단 하는 직접 또는 작은 개체 경우가 많습니다. 이 이라고 _닷지_합니다. 직접 어두워집니다 라는 특정 위치에 자세한 조명 하 고 (또는 광원의 대부분을 차단 하는 실습)에 구멍이 있는 불투명 자료를 사용할 수 있습니다 반대로 _굽기_합니다.

합니다 **닷지 및 Burn** 프로그램은 매우 비슷합니다 **어둡게 및 밝게**합니다. XAML 파일이 동일 하지만 다른 요소 이름의 구조적 및 마찬가지로 코드 숨김 파일이 상당히 유사 하지만 이러한 두 혼합 모드의 효과 상당히 다릅니다.

[![Burn 및 닷지](separable-images/DodgeAndBurn.png "닷지 및 Burn")](separable-images/DodgeAndBurn-Large.png#lightbox)

소규모 `Slider` 값을 `Lighten` 모드의 어두운 영역 하는 동안 먼저 `ColorDodge` 더 균일 하 게 밝게.

이미지 처리 응용 프로그램에는 종종 닷지 및 마찬가지로 한 암실 특정 영역으로 제한 됩니다 수 있습니다. 그라데이션 또는 다양 한 회색 음영을 사용 하 여 비트맵에서 수행할 수 있습니다.

## <a name="exploring-the-separable-blend-modes"></a>분리 가능한 blend 모드를 탐색합니다.

합니다 **분리 가능한 Blend 모드** 페이지를 사용 하면 모든 분리 가능한 blend 모드를 검사할 수 있습니다. 비트맵 대상과 blend 모드 중 하나를 사용 하 여 색이 칠해진된 사각형 원본을 표시 합니다. 

XAML 파일은 정의 `Picker` (blend 모드를 선택)를와 네 개의 슬라이더입니다. 처음 세 개의 슬라이더 원본의 빨강, 녹색 및 파랑 구성 요소를 설정할 수 있습니다. 네 번째 슬라이더는 회색 음영을 설정 하 여 해당 값을 재정의 하는 데 사용 됩니다. 개별 슬라이더 식별 되지 않습니다 하지만 색 해당 함수를 나타냅니다.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaviews="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.SeparableBlendModesPage"
             Title="Separable Blend Modes">

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
                    <x:Static Member="skia:SKBlendMode.Plus" />
                    <x:Static Member="skia:SKBlendMode.Modulate" />
                    <x:Static Member="skia:SKBlendMode.Screen" />
                    <x:Static Member="skia:SKBlendMode.Overlay" />
                    <x:Static Member="skia:SKBlendMode.Darken" />
                    <x:Static Member="skia:SKBlendMode.Lighten" />
                    <x:Static Member="skia:SKBlendMode.ColorDodge" />
                    <x:Static Member="skia:SKBlendMode.ColorBurn" />
                    <x:Static Member="skia:SKBlendMode.HardLight" />
                    <x:Static Member="skia:SKBlendMode.SoftLight" />
                    <x:Static Member="skia:SKBlendMode.Difference" />
                    <x:Static Member="skia:SKBlendMode.Exclusion" />
                    <x:Static Member="skia:SKBlendMode.Multiply" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Slider x:Name="redSlider"
                MinimumTrackColor="Red"
                MaximumTrackColor="Red"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="greenSlider"
                MinimumTrackColor="Green"
                MaximumTrackColor="Green"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="blueSlider"
                MinimumTrackColor="Blue"
                MaximumTrackColor="Blue"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="graySlider"
                MinimumTrackColor="Gray"
                MaximumTrackColor="Gray"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="colorLabel"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

코드 숨김 파일 비트맵 리소스 중 하나를 로드 하 고 캔버스의 위쪽 절반에서 한 번에 다시 아래쪽을 두 번 그립니다 캔버스의 절반.

```csharp
public partial class SeparableBlendModesPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(SeparableBlendModesPage),
                        "SkiaSharpFormsDemos.Media.Banana.jpg"); 

    public SeparableBlendModesPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs e)
    {
        if (sender == graySlider)
        {
            redSlider.Value = greenSlider.Value = blueSlider.Value = graySlider.Value;
        }

        colorLabel.Text = String.Format("Color = {0:X2} {1:X2} {2:X2}",
                                        (byte)(255 * redSlider.Value),
                                        (byte)(255 * greenSlider.Value),
                                        (byte)(255 * blueSlider.Value));

        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw bitmap in top half
        SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
        canvas.DrawBitmap(bitmap, rect, BitmapStretch.Uniform);

        // Draw bitmap in bottom halr
        rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
        canvas.DrawBitmap(bitmap, rect, BitmapStretch.Uniform);

        // Get values from XAML controls
        SKBlendMode blendMode =
            (SKBlendMode)(blendModePicker.SelectedIndex == -1 ?
                                        0 : blendModePicker.SelectedItem);

        SKColor color = new SKColor((byte)(255 * redSlider.Value),
                                    (byte)(255 * greenSlider.Value),
                                    (byte)(255 * blueSlider.Value));

        // Draw rectangle with blend mode in bottom half
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = color;
            paint.BlendMode = blendMode;
            canvas.DrawRect(rect, paint);
        }
    }
}
```

아래쪽에는 `PaintSurface` 처리기를 사각형 선택한 blend 모드 및 선택한 색을 사용 하 여 두 번째 비트맵 위에 그려집니다. 맨 위에 있는 원래 비트맵을 사용 하 여 맨 아래에서 수정 된 비트맵을 비교할 수 있습니다.

[![분리 가능한 Blend 모드](separable-images/SeparableBlendModes.png "분리 가능한 Blend 모드")](separable-images/SeparableBlendModes-Large.png#lightbox)

## <a name="additive-and-subtractive-primary-colors"></a>가산 및 무언가 감 기본 색

합니다 **기본 색** 페이지는 빨강, 녹색 및 파랑의 세 가지 겹치는 원을 그립니다.

[![기본 색 가산적](separable-images/PrimaryColors-Additive.png "가산적 기본 색")](separable-images/PrimaryColors-Additive.png#lightbox)

가산적 기본 색입니다. 녹청, 자홍, 노랑, 두 조합을 생성 하 고 세 가지의 조합은 흰색입니다.

사용 하 여 이러한 세 개의 원으로 그려집니다 합니다 `SKBlendMode.Plus` 있지만 모드를 사용할 수도 있습니다 `Screen`를 `Lighten`, 또는 `Difference` 같은 효과 대 한 합니다. 프로그램은 다음과 같습니다.

```csharp
public class PrimaryColorsPage : ContentPage
{
    bool isSubtractive;

    public PrimaryColorsPage ()
    {
        Title = "Primary Colors";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;

        // Switch between additive and subtractive primaries at tap
        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            isSubtractive ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);

        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
        float radius = Math.Min(info.Width, info.Height) / 4;
        float distance = 0.8f * radius;     // from canvas center to circle center
        SKPoint center1 = center + 
            new SKPoint(distance * (float)Math.Cos(9 * Math.PI / 6),
                        distance * (float)Math.Sin(9 * Math.PI / 6));
        SKPoint center2 = center +
            new SKPoint(distance * (float)Math.Cos(1 * Math.PI / 6),
                        distance * (float)Math.Sin(1 * Math.PI / 6));
        SKPoint center3 = center +
            new SKPoint(distance * (float)Math.Cos(5 * Math.PI / 6),
                        distance * (float)Math.Sin(5 * Math.PI / 6));

        using (SKPaint paint = new SKPaint())
        {
            if (!isSubtractive)
            {
                paint.BlendMode = SKBlendMode.Plus; 
                System.Diagnostics.Debug.WriteLine(paint.BlendMode);

                paint.Color = SKColors.Red;
                canvas.DrawCircle(center1, radius, paint);

                paint.Color = SKColors.Lime;    // == (00, FF, 00)
                canvas.DrawCircle(center2, radius, paint);

                paint.Color = SKColors.Blue;
                canvas.DrawCircle(center3, radius, paint);
            }
            else
            {
                paint.BlendMode = SKBlendMode.Multiply
                System.Diagnostics.Debug.WriteLine(paint.BlendMode);

                paint.Color = SKColors.Cyan;
                canvas.DrawCircle(center1, radius, paint);

                paint.Color = SKColors.Magenta;
                canvas.DrawCircle(center2, radius, paint);

                paint.Color = SKColors.Yellow;
                canvas.DrawCircle(center3, radius, paint);
            }
        }
    }
}
```

포함 하는 프로그램을 `TabGestureRecognizer`입니다. 프로그램에 사용 하 여 화면을 누르거나 탭 하는 경우 `SKBlendMode.Multiply` 무언가 감 세 원색을 표시 하려면:

[![기본 색 무언가 감](separable-images/PrimaryColors-Subtractive.png "무언가 감 기본 색")](separable-images/PrimaryColors-Subtractive-Large.png#lightbox)

`Darken` 모드 이와 같은 효과 대해서도 작동 합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
