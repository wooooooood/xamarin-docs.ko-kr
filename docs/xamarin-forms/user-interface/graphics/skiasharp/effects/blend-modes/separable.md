---
제목: "분리 가능한 혼합 모드" 설명: "빨강, 녹색 및 파랑 색을 변경 하려면 분리 가능한 blend 모드를 사용 합니다."
ms. prod: xamarin. 기술: xamarin-skiasharp assetid: 66D1A537-A247-484E-B5B9-FBCB7838FBE9 author: davidbritch: dabritch: 08/23/2018:-loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="the-separable-blend-modes"></a>분리 가능 블렌드 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[**SkiaSharp Porter-Duff**](porter-duff.md)blend 모드 문서에서 살펴본 것 처럼 Porter-duff blend 모드는 일반적으로 클리핑 작업을 수행 합니다. 분리 가능한 혼합 모드는 다릅니다. 분리 가능 모드는 이미지의 개별 빨강, 녹색 및 파랑 색 구성 요소를 변경 합니다. 분리 가능 블렌드 모드는 색을 혼합 하 여 빨간색, 녹색 및 파란색의 조합이 실제로 흰색 임을 보여 줍니다.

![기본 색](separable-images/SeparableSample.png "기본 색")

## <a name="lighten-and-darken-two-ways"></a>두 가지 방법으로 밝게 및 어둡게 

일반적으로 너무 어둡거나 너무 밝은 비트맵이 있습니다. 분리 되지 않은 혼합 모드를 사용 하 여 imabe를 밝게 또는 어둡게 할 수 있습니다.  실제로 열거형에서 분리 가능한 blend 모드의 두 가지 [`SKBlendMode`](xref:SkiaSharp.SKBlendMode) 이름은 `Lighten` 및 `Darken` 입니다. 

이러한 두 가지 모드는 **밝게 및 어둡게** 페이지에서 보여 줍니다. XAML 파일은 두 개의 `SKCanvasView` 개체와 두 개의 뷰를 인스턴스화합니다 `Slider` .

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

첫 번째 `SKCanvasView` 및를 `Slider` 시연 `SKBlendMode.Lighten` 하 고 두 번째 쌍은을 보여 줍니다 `SKBlendMode.Darken` . 두 `Slider` 뷰는 동일한 `ValueChanged` 처리기를 공유 하 고 두 뷰는 `SKCanvasView` 동일한 처리기를 공유 합니다 `PaintSurface` . 두 이벤트 처리기는 모두 이벤트를 발생 시키는 개체를 확인 합니다.

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

`PaintSurface`처리기는 비트맵에 적합 한 사각형을 계산 합니다. 처리기는 해당 비트맵을 표시 한 다음 `SKPaint` `BlendMode` 속성이 또는로 설정 된 개체를 사용 하 여 비트맵 위에 사각형을 표시 합니다 `SKBlendMode.Lighten` `SKBlendMode.Darken` . 속성은을 `Color` 기반으로 하는 회색 음영 `Slider` 입니다. 모드의 경우 `Lighten` 색의 범위는 검정에서 흰색 사이 이지만 모드의 경우에는 흰색에서 검정으로 범위가 범위를 가집니다 `Darken` .

왼쪽에서 오른쪽으로 더 큰 값이 표시 됩니다 `Slider` . 위쪽 이미지는 더 밝아지고 아래쪽 이미지는 더 어두워집니다.

[![밝게 및 어둡게](separable-images/LightenAndDarken.png "밝게 및 어둡게")](separable-images/LightenAndDarken-Large.png#lightbox)

이 프로그램은 분리 가능한 혼합 모드를 사용 하는 일반적인 방법을 보여 줍니다. 대상은 일종의 이미지 이며 매우 자주 비트맵입니다. 소스는 `SKPaint` 해당 속성이 분리 가능 blend 모드로 설정 된 개체를 사용 하 여 표시 되는 사각형입니다 `BlendMode` . 사각형은 단색 (있는 경우) 또는 그라데이션 일 수 있습니다. 투명성은 일반적으로 분리 가능 블렌드 모드에서 사용 _되지 않습니다_ .

이 프로그램을 사용 하 여 시험해 보면 이러한 두 blend 모드가 이미지를 균일 하 게 밝게 하 고 어둡게 만들지 않는 것을 알 수 있습니다. 대신는 `Slider` 일종의 임계값을 설정 하는 것으로 보입니다. 예를 들어 `Slider` 모드에 대 한를 늘리면 `Lighten` 이미지의 어두운 영역이 먼저 표시 되 고 밝은 영역은 동일 하 게 유지 됩니다.

모드의 `Lighten` 경우 대상 픽셀이 RGB 색 값 (Dr, Dg, Db)이 고 원본 픽셀이 색 (Sr, Sg, Sb) 인 경우 출력은 다음과 같이 계산 됩니다 (또는, Og, Ob).

 `Or = max(Dr, Sr)` `Og = max(Dg, Sg)`
 `Ob = max(Db, Sb)`

빨간색, 녹색 및 파란색의 경우 결과는 대상 및 원본의 큽니다. 이렇게 하면 대상의 어두운 영역이 먼저 밝게 효과를 생성 합니다.

`Darken`이 모드는 결과가 대상 및 소스 중 작은 것을 제외 하 고는 유사 합니다.

 `Or = min(Dr, Sr)` `Og = min(Dg, Sg)`
 `Ob = min(Db, Sb)`

빨간색, 녹색 및 파랑 구성 요소는 개별적으로 처리 되므로 이러한 blend 모드를 분리 _가능 블렌드 모드 라고 합니다._ 이러한 이유로 대상 및 원본 색에 약어 **Dc** 와 **Sc** 를 사용할 수 있으며, 각각의 빨간색, 녹색 및 파랑 구성 요소에 대 한 계산이 별도로 적용 된다는 것을 이해 하 고 있습니다.

다음 표에서는 모든 분리 가능한 blend 모드를 보여 줍니다. 두 번째 열은 변경 내용을 생성 하지 않는 원본 색을 표시 합니다.

| Blend 모드   | 변경 내용 없음 | 연산 |
| ------------ | --------- | --------- |
| `Plus`       | 검정     | 색 추가: Sc + Dc |
| `Modulate`   | 흰색     | 색을 곱하여 어둡게: Sc · Dc | 
| `Screen`     | 검정     | 보색의 제품 보완: Sc + Dc &ndash; Sc · Dc |
| `Overlay`    | 회색      | 역`HardLight` |
| `Darken`     | 흰색     | 최소 색: min (Sc, Dc) |
| `Lighten`    | 검정     | 최대 색: 최대 (Sc, Dc) |
| `ColorDodge` | 검정     | 소스를 기반으로 하는 대상 밝게 설정 |
| `ColorBurn`  | 흰색     | 소스를 기반으로 하는 어둡게 대상 | 
| `HardLight`  | 회색      | 강한 스포트라이트의 효과와 비슷합니다. |
| `SoftLight`  | 회색      | 소프트 스포트라이트의 효과와 비슷합니다. | 
| `Difference` | 검정     | 더 가늘게: Abs (Dc Sc)에서 더 가늘게 빼기 &ndash; | 
| `Exclusion`  | 검정     | 와 비슷하지만 `Difference` 대비가 낮습니다. |
| `Multiply`   | 흰색     | 색을 곱하여 어둡게: Sc · Dc |

이러한 두 소스의 표기법이 동일 하지는 않지만 W3C [**합성 및 혼합 수준 1**](https://www.w3.org/TR/compositing-1/) 사양과 지 수 ia [**Skblendmode 참조**](https://skia.org/user/api/SkBlendMode_Reference)에서 더 자세한 알고리즘을 찾을 수 있습니다. `Plus`일반적으로 Porter-Duff blend 모드로 간주 되며 `Modulate` W3C 사양의 일부가 아닙니다.

원본이 투명 한 경우를 제외 하 고 모든 분리 가능 blend 모드의 경우 `Modulate` blend 모드가 효과가 없습니다. 앞서 살펴본 것 처럼 `Modulate` blend 모드는 곱하기에 알파 채널을 통합 합니다. 그렇지 않으면 `Modulate` 와 동일한 결과가 발생 합니다 `Multiply` . 

및 라는 두 가지 모드를 확인 합니다 `ColorDodge` `ColorBurn` . _닷지_ 및 _굽기_ 단어는 사진 darkroom 연습에서 발생 했습니다. Enlarger은 네거티브를 통해 조명을 비추는 사진을 인쇄 합니다. 밝은 인쇄는 흰색입니다. 인쇄는 오랜 시간 동안 인쇄에 더 진하게 수록 됩니다. 인쇄 결정자는 종종 손 또는 작은 개체를 사용 하 여 일부 빛이 인쇄의 특정 부분에서 떨어지는 것을 차단 하 여 해당 영역을 더 밝게 만듭니다. 이를 _dodging_이라고 합니다. 반대로, 구멍이 있는 불투명 재질 (또는 대부분의 빛 차단)을 사용 하 여 특정 지점에서 더 많은 빛을 유도 하 여 _굽기를_이라고 합니다.

**닷지 및 굽기** 프로그램은 **밝게 및 어둡게**와 매우 비슷합니다. XAML 파일은 동일 하지만 요소 이름이 서로 다르지만 코드 숨김이 매우 유사 하지만 이러한 두 blend 모드의 효과는 매우 다릅니다.

[![닷지 및 굽기](separable-images/DodgeAndBurn.png "닷지 및 굽기")](separable-images/DodgeAndBurn-Large.png#lightbox)

작은 값의 경우에는 `Slider` `Lighten` 모드가 더 균일 하 게 어두운 영역을 먼저 밝게 `ColorDodge` 합니다.

이미지 처리 응용 프로그램을 사용 하면 darkroom와 마찬가지로 dodging 및 굽기를 특정 영역으로 제한 하는 경우가 많습니다. 이렇게 하려면 그라데이션을 사용 하거나 음영을 다르게 하는 비트맵을 사용 하 여 수행할 수 있습니다.

## <a name="exploring-the-separable-blend-modes"></a>분리 가능 블렌드 모드 탐색

분리 가능 **블렌드 모드** 페이지에서는 모든 분리 가능 블렌드 모드를 검사할 수 있습니다. 혼합 모드 중 하나를 사용 하 여 비트맵 대상과 색이 지정 된 사각형 원본을 표시 합니다. 

XAML 파일은 `Picker` (blend 모드를 선택 하기 위해) 및 네 개의 슬라이더를 정의 합니다. 처음 세 개의 슬라이더를 사용 하 여 원본의 빨간색, 녹색 및 파란색 구성 요소를 설정할 수 있습니다. 네 번째 슬라이더는 회색 음영을 설정 하 여 해당 값을 재정의 하는 것입니다. 개별 슬라이더는 식별 되지 않지만 색은 함수를 의미 합니다.

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

코드를 로드 하는 파일은 비트맵 리소스 중 하나를 로드 하 고 캔버스의 위쪽 절반에서 한 번 그리고 캔버스의 아래쪽 절반에 있는 다시 두 번 그립니다.

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

처리기의 아래쪽을 향해 `PaintSurface` 선택한 blend 모드와 선택한 색을 사용 하 여 두 번째 비트맵 위에 사각형이 그려집니다. 맨 아래에 있는 수정 된 비트맵을 위쪽의 원본 비트맵과 비교할 수 있습니다.

[![분리 가능한 혼합 모드](separable-images/SeparableBlendModes.png "분리 가능한 혼합 모드")](separable-images/SeparableBlendModes-Large.png#lightbox)

## <a name="additive-and-subtractive-primary-colors"></a>덧셈 및 subtractive 기본 색

**기본 색** 페이지는 빨간색, 녹색 및 파란색의 세 가지 겹치는 원을 그립니다.

[![기본 색 추가](separable-images/PrimaryColors-Additive.png "기본 색 추가")](separable-images/PrimaryColors-Additive.png#lightbox)

이러한 색은 가감 기본 색입니다. 사이안, 자홍 및 노랑을 생성 하는 두 가지 조합 및 모두 흰색의 조합입니다.

이러한 세 원이 모드를 사용 하 여 그려지지 `SKBlendMode.Plus` 만 `Screen` 동일한 효과를 위해, 또는를 사용할 수도 있습니다 `Lighten` `Difference` . 프로그램은 다음과 같습니다.

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

프로그램에는가 포함 됩니다 `TabGestureRecognizer` . 화면을 탭 하거나 클릭 하면 프로그램에서 `SKBlendMode.Multiply` 을 사용 하 여 세 가지 subtractive 주를 표시 합니다.

[![Subtractive 기본 색](separable-images/PrimaryColors-Subtractive.png "Subtractive 기본 색")](separable-images/PrimaryColors-Subtractive-Large.png#lightbox)

`Darken`이 모드는 동일한 효과에도 적용 됩니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
