---
제목: "SkiaSharp 원형 그라데이션" 설명: "원을 기반으로 하는 다양 한 유형의 그라데이션에 대해 알아봅니다."
ms. prod: xamarin. 기술: xamarin-skiasharp assetid: 400AE23A-6A0B-4FA8-BD6B-DE4146B04732 author: davidbritch: dabritch: 08/23/2018:-loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="the-skiasharp-circular-gradients"></a>SkiaSharp 원형 그라데이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[`SKShader`](xref:SkiaSharp.SKShader)클래스는 네 가지 유형의 그라데이션을 만드는 정적 메서드를 정의 합니다. [**SkiaSharp 선형 그라데이션**](linear-gradient.md) 문서에서는 메서드에 대해 설명 합니다 [`CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*) . 이 문서에서는 원을 기반으로 하는 다른 세 가지 유형의 그라데이션을 다룹니다.

[`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient*)메서드는 원의 중심에서 emanates 하는 그라데이션을 만듭니다.

![방사형 그라데이션 샘플](circular-gradients-images/RadialGradientSample.png)

[`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient*)메서드는 원의 중심을 중심으로 스윕 되는 그라데이션을 만듭니다.

![스윕 그라데이션 샘플](circular-gradients-images/SweepGradientSample.png)

세 번째 그라데이션 유형은 매우 드뭅니다. 이를 2 포인트 원추형 그라데이션으로 불리 며 메서드에 의해 정의 됩니다 [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient*) . 그라데이션은 한 원에서 다른 원으로 확장 됩니다.

![원추형 그라데이션 샘플](circular-gradients-images/ConicalGradientSample.png)

두 원의 크기가 다른 경우 그라데이션은 원뿔 형태를 사용 합니다.

이 문서에서는 이러한 그라데이션을 자세히 살펴봅니다.

## <a name="the-radial-gradient"></a>방사형 그라데이션

[`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode))메서드의 구문은 다음과 같습니다.

```csharp
public static SKShader CreateRadialGradient (SKPoint center, 
                                             Single radius, 
                                             SKColor[] colors, 
                                             Single[] colorPos, 
                                             SKShaderTileMode mode)
```

[`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix))오버 로드에는 transform matrix 매개 변수도 포함 됩니다.

처음 두 인수는 원의 중심 및 반지름을 지정 합니다. 그라데이션은 해당 중심에서 시작 하 여 픽셀에 대해 바깥쪽으로 확장 `radius` 됩니다. 이후에 발생 `radius` 하는 작업은 인수에 따라 달라 집니다 [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) . `colors`매개 변수는 선형 그라데이션 메서드에서와 같이 둘 이상의 색으로 이루어진 배열이 고 `colorPos` 는 0에서 1 사이의 범위에 있는 정수의 배열입니다. 이러한 정수는 해당 선을 따라 색의 상대 위치를 표시 합니다 `radius` . 해당 인수를로 설정 하 여 색을 동일 하 게 지정할 수 있습니다 `null` .

을 사용 하 여 `CreateRadialGradient` 원을 채울 경우 그라데이션의 중심을 원 중심으로 설정 하 고 그라데이션의 반경을 원의 반경으로 설정할 수 있습니다. 이 경우 `SKShaderTileMode` 인수는 그라데이션 렌더링에 영향을 주지 않습니다. 하지만 그라데이션으로 채워진 영역이 그라데이션으로 정의 된 원 보다 큰 경우이 인수는 원 외부에서 발생 하는 `SKShaderTileMode` 상황에 매우 큰 영향을 미칠 수 있습니다.

의 효과는 `SKShaderMode` [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **방사형 그라데이션** 페이지에서 보여 줍니다. 이 페이지의 XAML 파일은 `Picker` 열거형의 세 가지 멤버 중 하나를 선택할 수 있는를 인스턴스화합니다 `SKShaderTileMode` .

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

코드를 사용 하는 파일은 방사형 그라데이션으로 전체 캔버스를 색으로 바꿉니다. 그라데이션의 중심은 캔버스의 중심으로 설정 되 고 반지름은 100 픽셀로 설정 됩니다. 그라데이션은 검정 및 흰색의 두 색으로 구성 됩니다.

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

이 코드는 중앙에서 검정색의 검정색을 사용 하 여 중앙에서 흰색 100 픽셀까지 점차 페이드를 만듭니다. 해당 radius를 벗어나는 결과는 인수에 따라 달라 집니다 `SKShaderTileMode` .

[![방사형 그라데이션](circular-gradients-images/RadialGradient.png "방사형 그라데이션")](circular-gradients-images/RadialGradient-Large.png#lightbox)

세 가지 경우 모두 그라데이션은 캔버스를 채웁니다. 왼쪽의 iOS 화면에서 반지름을 벗어나는 그라데이션은 흰색으로 마지막 색을 계속 합니다. 의 결과입니다 `SKShaderTileMode.Clamp` . Android 화면에는의 효과가 표시 `SKShaderTileMode.Repeat` 됩니다. 중심의 100 픽셀에서 그라데이션은 검정 인 첫 번째 색으로 다시 시작 합니다. 그라데이션은 100 픽셀의 반경 마다 반복 됩니다. 

오른쪽의 유니버설 Windows 플랫폼 화면에는가 `SKShaderTileMode.Mirror` 그라데이션을 대체 방향으로 만드는 방법이 나와 있습니다. 첫 번째 그라데이션은 100 픽셀의 반지름에서 가운데에서 흰색으로의 검정입니다. 다음은 200 픽셀 반경의 100-픽셀 반경에서 검정으로, 다음 그라데이션은 다시 반대로 합니다.

방사형 그라데이션에서 세 개 이상의 색을 사용할 수 있습니다. **무지개 호 그라데이션** 샘플은 레인의 색에 해당 하는 8 개의 배열을 만들고 빨강으로 끝나며 8 자리 값의 배열입니다.

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

캔버스의 너비와 높이 중 최소값은 1000입니다. 즉, 값이 250이 됩니다 `rainbowWidth` . `outerRadius`및 `innerRadius` 값은 각각 1000 및 750로 설정 됩니다. 이러한 값은 배열을 계산 하는 데 사용 되 `positions` 고 8 개의 값은 0.75 f에서 1 사이입니다. `radius`값은 원을 스트로크 하는 데 사용 됩니다. 875 값은 250 픽셀의 스트로크 너비가 750 픽셀의 반지름과 1000 픽셀의 반지름 사이에서 확장 됨을 의미 합니다.

[![무지개 호 그라데이션](circular-gradients-images/RainbowArcGradient.png "무지개 호 그라데이션")](circular-gradients-images/RainbowArcGradient-Large.png#lightbox)

이 그라데이션으로 전체 캔버스를 채우면 내부 반경 내에서 빨간색으로 표시 됩니다. `positions`배열이 0으로 시작 되지 않기 때문입니다. 첫 번째 색은 0에서 첫 번째 배열 값 까지의 오프셋에 사용 됩니다. 그라데이션은 외부 반경을 벗어나 빨간색 이기도 합니다. 이는 타일 모드의 결과입니다 `Clamp` . 그라데이션은 굵은 선을 스트로크 하는 데 사용 되므로 이러한 빨강 영역은 표시 되지 않습니다.

## <a name="radial-gradients-for-masking"></a>마스킹에 대 한 방사형 그라데이션

선형 그라데이션과 마찬가지로 방사형 그라데이션은 투명 하거나 부분적으로 투명 한 색을 포함할 수 있습니다. 이 기능은 이미지의 일부를 숨겨 이미지의 다른 부분을 강조할 하는 _마스킹_이라는 프로세스에 유용 합니다.

**방사형 그라데이션 마스크** 페이지에 예제가 나와 있습니다. 프로그램은 리소스 비트맵 중 하나를 로드 합니다. `CENTER`및 `RADIUS` 필드는 비트맵 검사에서 결정 되었으며 강조 표시 되어야 하는 영역을 참조 합니다. `PaintSurface`처리기는 사각형을 계산 하 여 비트맵을 표시 한 다음 해당 사각형에 표시 하기 시작 합니다.

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

비트맵을 그린 후 몇 가지 간단한 코드 `CENTER` 는 및을로 변환 합니다 `RADIUS` . 그러면 `center` `radius` 비트맵에서 강조 표시 된 영역을 참조 하 여 표시를 위해 크기가 조정 되 고 이동 됩니다. 이러한 값은 해당 중심 및 반지름이 있는 방사형 그라데이션을 만드는 데 사용 됩니다. 두 색은 중앙에서 그리고 반지름의 처음 60%에 대해 투명 하 게 시작 됩니다. 그러면 그라데이션이 흰색으로 페이드 인 됩니다.

[![방사형 그라데이션 마스크](circular-gradients-images/RadialGradientMask.png "방사형 그라데이션 마스크")](circular-gradients-images/RadialGradientMask-Large.png#lightbox)

이 방법은 비트맵을 마스크 하는 가장 좋은 방법이 아닙니다. 이러한 문제는 마스크의 색이 대부분 캔버스의 배경과 일치 하도록 선택 된 흰색 임을 나타내는 것입니다. 배경이 다른 색 &mdash; 이거나 그라데이션 자체가 &mdash; 일치 하지 않는 경우입니다. 마스킹에 대 한 더 나은 접근 방식은 [SkiaSharp Porter-Duff blend 모드](../blend-modes/porter-duff.md)문서에 나와 있습니다.

## <a name="radial-gradients-for-specular-highlights"></a>반사 하이라이트에 대 한 방사형 그라데이션

조명이 둥근 표면에 노출 되 면 많은 방향의 빛을 반영 하지만 일부 빛은 뷰어의 눈에 직접 바운스 합니다. 이를 통해 _반사 하이라이트_라는 화면에서 유사 항목의 모양을 만들 수도 있습니다.

3 차원 그래픽에서 반사 하이라이트는 광원 경로 및 음영을 결정 하는 데 사용 되는 알고리즘에서 발생 하는 경우가 많습니다. 2 차원 그래픽에서 3D 표면의 모양을 제안 하기 위해 반사 하이라이트가 추가 되기도 합니다. 반사 하이라이트는 플랫 빨강 원을 원 빨강 공로 변환할 수 있습니다.

**방사형 반사 강조 표시** 페이지는 방사형 그라데이션을 사용 하 여 정확 하 게 수행 합니다. `PaintSurface`이 처리기는 원의 반지름을 계산 하 여 생물 하 고, 두 `SKPoint` 값 &mdash; a `center` 와 `offCenter` 원의 왼쪽 위 가장자리의 중간에 있는를 만듭니다.

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

`CreateRadialGradient`이 호출은 흰색을 사용 하 여 해당 지점에서 시작 하 `offCenter` 고 반경 절반 거리 만큼 빨간색으로 끝나는 그라데이션을 만듭니다. 다음과 같이 나타납니다.

[![방사형 반사 강조 표시](circular-gradients-images/RadialSpecularHighlight.png "방사형 반사 강조 표시")](circular-gradients-images/RadialSpecularHighlight-Large.png#lightbox)

이 그라데이션을 자세히 살펴보면 결함이 있음을 결정할 수 있습니다. 그라데이션은 특정 지점을 중심으로 중심 이며 모퉁이가 둥근 표면을 반영 하는 것이 약간 더 작은 경우도 있습니다. 이 경우 아래에 표시 된 반사 하이라이트를 [**반사 하이라이트에 대 한 원추 그라데이션**](#conical-gradients-for-specular-highlights)섹션에서 사용 하는 것이 좋습니다.

## <a name="the-sweep-gradient"></a>스윕 그라데이션

[`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[]))메서드에는 모든 그라데이션 생성 메서드의 가장 간단한 구문이 있습니다.

```csharp
public static SKShader CreateSweepGradient (SKPoint center, 
                                            SKColor[] colors, 
                                            Single[] colorPos)
```

단지 가운데, 색 배열 및 색 위치입니다. 그라데이션은 중심점의 오른쪽에서 시작 하 여 가운데를 중심으로 360도 시계 방향으로 스윕 합니다. `SKShaderTileMode`매개 변수는 없습니다.

[`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKMatrix))행렬 transform 매개 변수가 있는 오버 로드도 사용할 수 있습니다. 회전 변환을 그라데이션에 적용 하 여 시작점을 변경할 수 있습니다. 배율 변환을 적용 하 여 시계 방향에서 시계 반대 방향으로 방향을 변경할 수도 있습니다.

**스윕 그라데이션** 페이지는 스윕 그라데이션을 사용 하 여 획 너비가 50 픽셀인 원을 색으로 바꿉니다.

[![스윕 그라데이션](circular-gradients-images/SweepGradient.png "스윕 그라데이션")](circular-gradients-images/SweepGradient-Large.png#lightbox)

`SweepGradientPage`클래스는 색상 값이 다른 8 색의 배열을 정의 합니다. 배열의 맨 오른쪽에 표시 되는 빨강 (색상 값 0 또는 360)으로 시작 하 고 끝나는 것을 볼 수 있습니다.

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

프로그램은 `TapGestureRecognizer` 처리기의 끝에서 일부 코드를 활성화 하는도 구현 합니다 `PaintSurface` . 이 코드는 동일한 그라데이션을 사용 하 여 캔버스를 채웁니다.

[![스윕 그라데이션 전체](circular-gradients-images/SweepGradientFull.png "스윕 그라데이션 전체")](circular-gradients-images/SweepGradientFull-Large.png#lightbox)

이러한 스크린샷은 그라데이션에 따라 색이 지정 된 영역을 채우는 것을 보여 줍니다. 그라데이션이 동일한 색으로 시작 하 고 끝나지 않는 경우 중심점 오른쪽에 불연속성 됩니다.

## <a name="the-two-point-conical-gradient"></a>2 포인트 원추형 그라데이션

[`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode))메서드의 구문은 다음과 같습니다.

```csharp
public static SKShader CreateTwoPointConicalGradient (SKPoint startCenter, 
                                                      Single startRadius, 
                                                      SKPoint endCenter, 
                                                      Single endRadius, 
                                                      SKColor[] colors, 
                                                      Single[] colorPos, 
                                                      SKShaderTileMode mode)
```

매개 변수는 _시작_ 원 및 _끝_ 원 이라고 하는 두 원의 중심 지점과 반지름으로 시작 합니다. 나머지 세 매개 변수는 및의 경우와 동일 합니다 `CreateLinearGradient` `CreateRadialGradient` . [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix))오버 로드에는 행렬 변환이 포함 됩니다.

그라데이션은 시작 원에서 시작 하 여 끝 원에서 끝납니다. `SKShaderTileMode`매개 변수는 두 원을 벗어난 결과를 제어 합니다. 2 포인트 원추형 그라데이션은 영역을 완전히 채우지 않는 유일한 그라데이션입니다. 두 원의 반지름이 동일한 경우 그라데이션은 원의 직경과 동일한 너비를 가진 사각형으로 제한 됩니다. 두 원의 반지름이 다른 경우 그라데이션은 원뿔을 형성 합니다.

두 점 원추 그라데이션을 시험해 볼 수 있으므로 **원추형 그라데이션** 페이지가에서 파생 되어 `InteractivePage` 두 원 반지름에 대해 두 개의 터치 포인트를 이동할 수 있습니다.

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

코드 숨김이 파일은 `TouchPoint` 고정 반지름이 50 및 100 인 두 개의 개체를 정의 합니다.

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

`colors`배열은 빨강, 녹색 및 파랑입니다. 처리기의 아래쪽에 있는 코드는 `PaintSurface` 두 개의 터치 지점을 검정 원으로 그려 그라데이션을 액세스가 방해 하지 않도록 합니다.

`DrawRect`호출은 그라데이션을 사용 하 여 전체 캔버스를 색으로 표시 합니다. 그러나 일반적인 경우 캔버스의 대부분은 그라데이션으로 uncolored 유지 됩니다. 가능한 세 가지 구성을 보여 주는 프로그램은 다음과 같습니다.

[![원추형 그라데이션](circular-gradients-images/ConicalGradient.png "원추형 그라데이션")](circular-gradients-images/ConicalGradient-Large.png#lightbox)

왼쪽의 iOS 화면에는 `SKShaderTileMode` 의 설정 효과가 표시 `Clamp` 됩니다. 그라데이션은 두 번째 원과 가장 가까이 있는 작은 원의 가장자리 안에 빨강으로 시작 합니다. 이 `Clamp` 값은 또한 빨강을 계속 원뿔의 점으로 설정 합니다. 그라데이션은 첫 번째 원에 가장 가까운 큰 원의 외부 가장자리에 파란색으로 끝나지만 해당 원 내에서 파란색으로 계속 됩니다.

Android 화면은 유사 하지만 `SKShaderTileMode` 의가 `Repeat` 있습니다. 이제 그라데이션이 첫 번째 원 내에서 시작 하 고 두 번째 원 밖으로 끝나도록 하는 것이 더 명확 합니다. `Repeat`설정으로 인해 큰 원 안에 빨간색으로 그라데이션이 다시 반복 됩니다.

UWP 화면에는 작은 원이 큰 원 안에 완전히 이동 하면 어떤 일이 발생 하는지 보여 줍니다. 그라데이션 중지점은 원뿔이 아니라 전체 영역을 채웁니다. 효과는 방사형 그라데이션과 유사 하지만 작은 원이 큰 원 내에서 정확히 가운데에 배치 되지 않은 경우에는 비대칭입니다.

한 원이 다른 원으로 중첩 된 경우 그라데이션의 실제적인 유용성을 확인할 수 있지만 반사 하이라이트에는 적합 합니다.

## <a name="conical-gradients-for-specular-highlights"></a>반사 하이라이트의 원추형 그라데이션

이 문서의 앞부분에서는 방사형 그라데이션을 사용 하 여 반사 하이라이트를 만드는 방법을 살펴보았습니다. 이 목적을 위해 2 점 원추형 그라데이션을 사용할 수도 있으며, 표시 되는 모양을 선호 하는 경우도 있습니다.

[![원추형 반사 강조 표시](circular-gradients-images/ConicalSpecularHighlight.png "원추형 반사 강조 표시")](circular-gradients-images/ConicalSpecularHighlight-Large.png#lightbox)

비대칭 모양은 개체의 둥근 표면을 제안 합니다. 

**원추형 반사 강조 표시** 페이지의 그리기 코드는 셰이더를 제외 하 고 **방사형 반사 강조 표시** 페이지와 동일 합니다.

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

두 원에는 및의 센터가 있습니다 `offCenter` `center` . 를 중심으로 하는 원은 `center` 전체 해골을 포함 하는 반지름에 연결 되어 있지만에서 가운데에 있는 원은 `offCenter` 한 픽셀씩의 반지름을 갖습니다. 그라데이션은 실제로 해당 지점에서 시작 하 여 구슬 가장자리에서 끝납니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
