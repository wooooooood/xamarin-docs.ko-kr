---
title: SkiaSharp 선형 그라데이션
description: 선을 스트로크 또는 두 가지 색을 단계적으로 혼합 구성 된 그라데이션으로 영역을 채우는 방법을 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 20A2A8C4-FEB7-478D-BF57-C92E26117B6A
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 290e533e54b2ee150b94d9fb6b0f5119324f9cf0
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306502"
---
# <a name="the-skiasharp-linear-gradient"></a>SkiaSharp 선형 그라데이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[`SKPaint`](xref:SkiaSharp.SKPaint) 클래스는 단색으로 선 또는 채우기 영역을 스트로크 하는 데 사용 되는 [`Color`](xref:SkiaSharp.SKPaint.Color) 속성을 정의 합니다. 또는 색의 점진적 혼합 인 _그라데이션으로_선 또는 채우기 영역을 채울 수 있습니다.

![선형 그라데이션 샘플](linear-gradient-images/LinearGradientSample.png "선형 그라데이션 샘플")

가장 기본적인 그라데이션 유형은 _선형_ 그라데이션입니다. 색 혼합은 한 점에서 다른 점으로 선 ( _그라데이션 선_이라고 함)에서 발생 합니다. 수직 그라데이션 선으로 줄에는 동일한 색을 가집니다. 두 정적 [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*) 방법 중 하나를 사용 하 여 선형 그라데이션을 만듭니다. 두 오버 로드 간의 차이점은 매트릭스 변환을 포함 한 다른 그렇지 않습니다. 

이러한 메서드는 `SKPaint`의 [`Shader`](xref:SkiaSharp.SKPaint.Shader) 속성으로 설정 된 [`SKShader`](xref:SkiaSharp.SKShader) 형식의 개체를 반환 합니다. `Shader` 속성이 null이 아닌 경우 `Color` 속성을 재정의 합니다. 스트로크 하는 선 또는이 `SKPaint` 개체를 사용 하 여 채워진 모든 영역은 단색이 아닌 그라데이션을 기반으로 합니다.

> [!NOTE]
> `DrawBitmap` 호출에 `SKPaint` 개체를 포함 하면 `Shader` 속성이 무시 됩니다. [SkiaSharp 비트맵 표시](../../bitmaps/displaying.md#displaying-in-pixel-dimensions)문서에 설명 된 대로 `SKPaint`의 `Color` 속성을 사용 하 여 비트맵을 표시할 투명도 수준을 설정할 수 있지만 그라데이션 투명도를 사용 하 여 비트맵을 표시 하는 데 `Shader` 속성을 사용할 수 없습니다. 그라데이션 투명 효과를 사용 하 여 비트맵을 표시 하는 데 사용할 수 있는 다른 기술은 다음과 같습니다. [SkiaSharp 원형 그라데이션과](circular-gradients.md#radial-gradients-for-masking) [SkiaSharp 합성 및 blend 모드](../blend-modes/porter-duff.md#gradient-transparency-and-transitions)문서에 설명 되어 있습니다.

## <a name="corner-to-corner-gradients"></a>모퉁이가-그라데이션

종종 선형 그라데이션 다른 영역의 한쪽 모퉁이에서 확장합니다. 시작점을 사각형의 왼쪽 위 모퉁이 그라데이션 확장할 수 있습니다.

- 왼쪽 아래 모서리에 수직으로
- 오른쪽 위 모퉁이에 수평으로
- 오른쪽 아래 모퉁이에 대각선 방향으로

대각선 선형 그라데이션은 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **SkiaSharp 셰이더 및 기타 효과** 섹션에 있는 첫 번째 페이지에서 보여 줍니다. **모퉁이-모퉁이 그라데이션** 페이지는 해당 생성자에 `SKCanvasView`를 만듭니다. `PaintSurface` 처리기는 `using` 문에 `SKPaint` 개체를 만든 다음 캔버스에서 300 픽셀 사각형 사각형을 정의 합니다.

```csharp
public class CornerToCornerGradientPage : ContentPage
{
    ···
    public CornerToCornerGradientPage ()
    {
        Title = "Corner-to-Corner Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ···
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            // Create 300-pixel square centered rectangle
            float x = (info.Width - 300) / 2;
            float y = (info.Height - 300) / 2;
            SKRect rect = new SKRect(x, y, x + 300, y + 300);

            // Create linear gradient from upper-left to lower-right
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Left, rect.Top),
                                new SKPoint(rect.Right, rect.Bottom),
                                new SKColor[] { SKColors.Red, SKColors.Blue },
                                new float[] { 0, 1 },
                                SKShaderTileMode.Repeat);

            // Draw the gradient on the rectangle
            canvas.DrawRect(rect, paint);
            ···
        }
    }
}
```

`SKPaint`의 `Shader` 속성에는 정적 `SKShader.CreateLinearGradient` 메서드에서 `SKShader` 반환 값이 할당 됩니다. 다섯 개의 인수는 다음과 같습니다.

- 그라데이션의 시작점 사각형의 왼쪽 위 모퉁이 여기로.
- 그라데이션의 끝점 사각형의 오른쪽 아래 모서리를 여기로.
- 두 개 이상의 색 그라데이션에 기여 하는 배열
- 그라데이션 선 내에서 색의 상대 위치를 나타내는 `float` 값의 배열입니다.
- 그라데이션이 그라데이션 선의 끝을 넘어 동작 하는 방식을 나타내는 [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) 열거형의 멤버입니다.

그라데이션 개체를 만든 후 `DrawRect` 메서드는 셰이더를 포함 하는 `SKPaint` 개체를 사용 하 여 300 픽셀 사각형을 그립니다. 여기 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에서 실행 합니다.

[![모퉁이 간 그라데이션](linear-gradient-images/CornerToCornerGradient.png "모퉁이 간 그라데이션")](linear-gradient-images/CornerToCornerGradient-Large.png#lightbox)

그라데이션 선은 처음 두 인수로 지정 된 두 개의 점으로 정의 됩니다. 이러한 점은 그라데이션으로 표시 되는 그래픽 개체가 _아니라_ _캔버스_ 를 기준으로 합니다. 그라데이션 선을 따라 색으로 점차 오른쪽 아래에 파란색으로 왼쪽 위에 빨간색에서 전환. 그라데이션 선을에 수직인 모든 줄에 불변 색을 있습니다.

네 번째 인수로 지정 된 `float` 값의 배열은 색 배열과 일 대 일 대응 합니다. 값은 해당 색 발생 그라데이션 선 따라 상대 위치를 나타냅니다. 여기서 0은 그라데이션 선의 시작 부분에서 `Red` 발생 함을 의미 하 고, 1은 `Blue`가 줄의 끝에서 발생 함을 의미 합니다. 숫자 오름차순 해야 및 0 ~ 1 범위에 있어야 합니다. 해당 범위의 그렇지 않은 경우 해당 범위에 포함 되도록 조정 됩니다.

배열의 두 값 0과 1 이외의 값으로 설정할 수 있습니다. 다음을 실행해보세요.

```csharp
new float[] { 0.25f, 0.75f }
```

이제 그라데이션 줄의 첫 번째 전체 분기는 순수한 빨간색 이며 마지막 분기 순수 파란색입니다. 빨강 및 파랑 조합 그라데이션 선 중앙 절반으로 제한 됩니다.

일반적으로 이러한 위치 값이 동일 하 게 0에서에서 1 공간을 해야 합니다. 이 경우 `CreateLinearGradient`에 `null` 네 번째 인수로 제공 하기만 하면 됩니다.

이 그라데이션 300 픽셀 사각형 영역의 두 모퉁이 사이 정의 된, 하지만 해당 사각형을 채우는 제한 하지 않습니다. **모퉁이 간 그라데이션** 페이지에는 페이지에서 탭 하거나 마우스를 클릭 하면 응답 하는 추가 코드가 포함 됩니다. `drawBackground` 필드는 `true` 사이에서 전환 되며 각 탭에서 `false` 됩니다. 값이 `true`이면 `PaintSurface` 처리기는 동일한 `SKPaint` 개체를 사용 하 여 전체 캔버스를 채운 다음 작은 사각형을 나타내는 검정색 사각형을 그립니다. 

```csharp
public class CornerToCornerGradientPage : ContentPage
{
    bool drawBackground;

    public CornerToCornerGradientPage ()
    {
        ···
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
        ···
        using (SKPaint paint = new SKPaint())
        {
            ···
            if (drawBackground)
            {
                // Draw the gradient on the whole canvas
                canvas.DrawRect(info.Rect, paint);

                // Outline the smaller rectangle
                paint.Shader = null;
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                canvas.DrawRect(rect, paint);
            }
        }
    }
}
```

화면을 누른 후 볼 수 있는 내용 다음과 같습니다.

[![모퉁이 간 그라데이션 전체](linear-gradient-images/CornerToCornerGradientFull.png "모퉁이 간 그라데이션 전체")](linear-gradient-images/CornerToCornerGradientFull-Large.png#lightbox)

그라데이션의 그라데이션 선을 정의 하는 점 이외의 동일한 패턴에 자체 반복 있는지 확인 합니다. 이 반복은 `CreateLinearGradient`의 마지막 인수가 `SKShaderTileMode.Repeat`되기 때문에 발생 합니다. (있습니다 곧 확인 하겠지만 다른 옵션입니다.)

또한 그라데이션 줄을 지정 하는 데 사용 하는 지점 고유 하지는 알 수 있습니다. 수직 그라데이션 선으로 줄 무한 같은 효과를 지정할 수 있는 그라데이션 선의 되므로 동일한 색을 가집니다. 예를 들어 가로 그라데이션을 사용 하 여 사각형을 채우는, 왼쪽 및 오른쪽 위 모서리 또는 왼쪽 및 오른쪽 아래 모서리를 사용한 경우라도 및 병렬 해당 줄으로 된 두 점을 지정할 수 있습니다.

## <a name="interactively-experiment"></a>대화형으로 실험

**대화형 선형 그라데이션** 페이지를 사용 하 여 대화형으로 선형 그라데이션을 시험해 볼 수 있습니다. 이 페이지에서는 [**호를 그리기 위한 세 가지 방법**](../../curves/arcs.md)문서에 도입 된 `InteractivePage` 클래스를 사용 합니다. `InteractivePage` [`TouchEffect`](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md) 이벤트를 처리 하 여 손가락이 나 마우스로 이동할 수 있는 `TouchPoint` 개체의 컬렉션을 유지 합니다.

XAML 파일은 `TouchEffect`를 `SKCanvasView`의 부모에 연결 하 고 [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) 열거의 세 멤버 중 하나를 선택할 수 있는 `Picker`를 포함 합니다.

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
                       xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Effects.InteractiveLinearGradientPage"
                       Title="Interactive Linear Gradient">
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

코드 숨김이 있는 파일의 생성자는 선형 그라데이션의 시작점과 끝점에 대 한 두 개의 `TouchPoint` 개체를 만듭니다. `PaintSurface` 처리기는 세 가지 색의 배열을 정의 하 고 (그라데이션을 빨강에서 녹색으로 파랑으로), `Picker`에서 현재 `SKShaderTileMode`를 가져옵니다.

```csharp
public partial class InteractiveLinearGradientPage : InteractivePage
{
    public InteractiveLinearGradientPage ()
    {
        InitializeComponent ();

        touchPoints = new TouchPoint[2];

        for (int i = 0; i < 2; i++)
        { 
            touchPoints[i] = new TouchPoint
            {
                Center = new SKPoint(100 + i * 200, 100 + i * 200)
            };
        }

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
            paint.Shader = SKShader.CreateLinearGradient(touchPoints[0].Center,
                                                         touchPoints[1].Center,
                                                         colors,
                                                         null,
                                                         tileMode);
            canvas.DrawRect(info.Rect, paint);
        }
        ···
    }
}
```

`PaintSurface` 처리기는 모든 정보에서 `SKShader` 개체를 만들고이 개체를 사용 하 여 전체 캔버스를 색으로 만듭니다. `float` 값의 배열은 `null`로 설정 됩니다. 이 고, 그렇지 동일 하 게 세 가지 색 공간을 해당 매개 변수는 배열의 값이 0, 0.5, 1을 설정 합니다.

`PaintSurface` 처리기의 대부분은 다음과 같은 여러 개체를 표시 하는 데 활용 됩니다. 즉, 터치 지점은 윤곽선 원, 그라데이션 선, 터치 점에서 그라데이션 선에 수직인 선입니다.

```csharp
public partial class InteractiveLinearGradientPage : InteractivePage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
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

            // Draw gradient line connecting touchpoints
            canvas.DrawLine(touchPoints[0].Center, touchPoints[1].Center, paint);

            // Draw lines perpendicular to the gradient line
            SKPoint vector = touchPoints[1].Center - touchPoints[0].Center;
            float length = (float)Math.Sqrt(Math.Pow(vector.X, 2) +
                                            Math.Pow(vector.Y, 2));
            vector.X /= length;
            vector.Y /= length;
            SKPoint rotate90 = new SKPoint(-vector.Y, vector.X);
            rotate90.X *= 200;
            rotate90.Y *= 200;

            canvas.DrawLine(touchPoints[0].Center, 
                            touchPoints[0].Center + rotate90, 
                            paint);

            canvas.DrawLine(touchPoints[0].Center,
                            touchPoints[0].Center - rotate90,
                            paint);

            canvas.DrawLine(touchPoints[1].Center,
                            touchPoints[1].Center + rotate90,
                            paint);

            canvas.DrawLine(touchPoints[1].Center,
                            touchPoints[1].Center - rotate90,
                            paint);
        }
    }
}
```

두 접점을 연결 하는 그라데이션 선을 그리려면, 간단 하지만 수직 줄은 몇 가지 더 많은 작업이 필요 합니다. 그라데이션 선 벡터를 변환할 하 고 단일 단위 길이 표준화 하 고 방향으로 90도 회전 됩니다. 벡터 200 픽셀의 길이 지정 합니다 됩니다. 수직 그라데이션 선으로 되도록 터치 포인트에서을 확장 하는 4 개의 선을 그리는 데 사용 됩니다.

수직 줄 그라데이션의 시작과 끝과 일치합니다. 이러한 줄을 벗어나는 결과는 `SKShaderTileMode` 열거의 설정에 따라 달라 집니다.

[![대화형 선형 그라데이션](linear-gradient-images/InteractiveLinearGradient.png "대화형 선형 그라데이션")](linear-gradient-images/InteractiveLinearGradient-Large.png#lightbox)

3 개의 스크린샷은 [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode)의 세 가지 다른 값에 대 한 결과를 보여 줍니다. IOS 스크린샷은 그라데이션 테두리의 색을 확장 하는 `SKShaderTileMode.Clamp`표시 합니다. Android 스크린 샷의 `SKShaderTileMode.Repeat` 옵션은 그라데이션 패턴이 반복 되는 방법을 보여 줍니다. UWP 스크린샷의 `SKShaderTileMode.Mirror` 옵션은 또한 패턴을 반복 하지만 매번 패턴이 반전 되어 색 불연속성이 발생 하지 않습니다.

## <a name="gradients-on-gradients"></a>그라데이션에서 그라데이션

`SKShader` 클래스는 `Dispose`를 제외 하 고 공용 속성 또는 메서드를 정의 하지 않습니다. 따라서 해당 정적 메서드로 만든 `SKShader` 개체는 변경할 수 없습니다. 두 개의 다른 개체에 대 한 동일한 그라데이션을 사용 하는 경우에 가능성이 그라데이션의 약간 변경 하는 것이 좋습니다. 이렇게 하려면 새 `SKShader` 개체를 만들어야 합니다.

**그라데이션 텍스트** 페이지에는 비슷한 그라데이션으로 색이 지정 된 텍스트와 백그라운드 표시 됩니다.

[![그라데이션 텍스트](linear-gradient-images/GradientText.png "그라데이션 텍스트")](linear-gradient-images/GradientText-Large.png#lightbox)

그라데이션에서 유일한 차이점은 시작 및 끝점. 텍스트 표시에 사용 되는 그라데이션의 텍스트의 경계 사각형의 모퉁이에 있는 두 점 기반으로 합니다. 백그라운드에 대 한 두 가지 시점은 전체 캔버스를 기반으로 합니다. 코드는 다음과 같습니다.

```csharp
public class GradientTextPage : ContentPage
{
    const string TEXT = "GRADIENT";

    public GradientTextPage ()
    {
        Title = "Gradient Text";

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
            // Create gradient for background
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, 0),
                                new SKPoint(info.Width, info.Height),
                                new SKColor[] { new SKColor(0x40, 0x40, 0x40),
                                                new SKColor(0xC0, 0xC0, 0xC0) },
                                null,
                                SKShaderTileMode.Clamp);

            // Draw background
            canvas.DrawRect(info.Rect, paint);

            // Set TextSize to fill 90% of width
            paint.TextSize = 100;
            float width = paint.MeasureText(TEXT);
            float scale = 0.9f * info.Width / width;
            paint.TextSize *= scale;

            // Get text bounds
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Calculate offsets to center the text on the screen
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            // Shift textBounds by that amount
            textBounds.Offset(xText, yText);

            // Create gradient for text
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(textBounds.Left, textBounds.Top),
                                new SKPoint(textBounds.Right, textBounds.Bottom),
                                new SKColor[] { new SKColor(0x40, 0x40, 0x40),
                                                new SKColor(0xC0, 0xC0, 0xC0) },
                                null,
                                SKShaderTileMode.Clamp);

            // Draw text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

`SKPaint` 개체의 `Shader` 속성이 먼저 설정 되어 배경을 포함 하는 그라데이션이 표시 됩니다. 그라데이션 지점 캔버스의 왼쪽 및 오른쪽 아래 모서리에 설정 됩니다.

이 코드는 텍스트가 캔버스 너비의 90%에 표시 되도록 `SKPaint` 개체의 `TextSize` 속성을 설정 합니다. 텍스트 범위는 `xText` 및 `yText` 값을 계산 하는 데 사용 되며 `DrawText` 메서드에 전달 하 여 텍스트를 가운데에 맞춥니다.

그러나 두 번째 `CreateLinearGradient` 호출의 그라데이션 지점은 캔버스를 표시할 때 캔버스를 기준으로 텍스트의 왼쪽 위와 오른쪽 아래 모퉁이를 참조 해야 합니다. 이 작업을 수행 하려면 `textBounds` 사각형을 동일한 `xText` 및 `yText` 값으로 이동 합니다.

```csharp
textBounds.Offset(xText, yText);
```

사각형의 왼쪽 및 오른쪽 아래 모퉁이 그라데이션의 시작점과 끝점을 설정 하는 데 사용 될 수 있습니다.

## <a name="animating-a-gradient"></a>그라데이션에 애니메이션 적용

그라데이션에 애니메이션을 적용 하는 방법은 여러 가지가 있습니다. 한 가지 방법은 시작점과 끝점을 애니메이션 효과를 주는 하는 것입니다. **그라데이션 애니메이션** 페이지는 캔버스를 중심으로 하는 원에서 두 지점을 이동 합니다. 이 원의 반지름을 절반 너비 또는 캔버스의 높이 더 작은 값입니다. 시작점과 끝점은이 원에서 서로 반대 이며, 그라데이션은 `Mirror` 타일 모드를 사용 하 여 흰색에서 검정으로 이동 합니다.

[![그라데이션 애니메이션](linear-gradient-images/GradientAnimation.png "그라데이션 애니메이션")](linear-gradient-images/GradientAnimation-Large.png#lightbox)

생성자는 `SKCanvasView`을 만듭니다. `OnAppearing` 및 `OnDisappearing` 메서드는 애니메이션 논리를 처리 합니다.

```csharp
public class GradientAnimationPage : ContentPage
{
    SKCanvasView canvasView;
    bool isAnimating;
    double angle;
    Stopwatch stopwatch = new Stopwatch();

    public GradientAnimationPage()
    {
        Title = "Gradient Animation";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 3000;
        angle = 2 * Math.PI * (stopwatch.ElapsedMilliseconds % duration) / duration;
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

`OnTimerTick` 메서드는 3 초 마다 0에서 2π 사이의 애니메이션을 적용 하는 `angle` 값을 계산 합니다. 

두 개의 그라데이션 점을 계산 하는 한 가지 방법은 다음과 같습니다. `vector` 이라는 `SKPoint` 값은 캔버스의 중심에서 원의 반지름에 있는 점으로 확장 되도록 계산 됩니다. 이 벡터의 방향 각도의 사인 및 코사인 값을 기반으로 합니다. 두 반대 그라데이션 점 계산 됩니다: 하나의 지점 center 지점에서 해당 벡터를 뺀 값으로 계산 되 고 다른 중심점에 벡터를 추가 하 여 계산 됩니다.

```csharp
public class GradientAnimationPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
            int radius = Math.Min(info.Width, info.Height) / 2;
            SKPoint vector = new SKPoint((float)(radius * Math.Cos(angle)),
                                         (float)(radius * Math.Sin(angle)));

            paint.Shader = SKShader.CreateLinearGradient(
                                center - vector,
                                center + vector,
                                new SKColor[] { SKColors.White, SKColors.Black },
                                null,
                                SKShaderTileMode.Mirror);

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

다소 다른 접근 방법이 더 적은 코드에 필요합니다. 이 방법을 사용 하면 행렬 변환이 있는 [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) 오버 로드 메서드를 마지막 인수로 사용 합니다. 이 방법은 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 버전입니다.

```csharp
public class GradientAnimationPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, 0),
                                info.Width < info.Height ? new SKPoint(info.Width, 0) : 
                                                           new SKPoint(0, info.Height),
                                new SKColor[] { SKColors.White, SKColors.Black },
                                new float[] { 0, 1 },
                                SKShaderTileMode.Mirror,
                                SKMatrix.MakeRotation((float)angle, info.Rect.MidX, info.Rect.MidY));

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

캔버스의 너비가 높이 보다 작은 경우 두 그라데이션 지점은 (0, 0) 및 (`info.Width`, 0)로 설정 됩니다. `CreateLinearGradient`의 마지막 인수로 전달 되는 회전 변환은 화면 중심을 중심으로 두 개의 요소를 효과적으로 회전 합니다.

각도 0, 회전 안 되며 두 그라데이션 점 캔버스의 왼쪽 위 모퉁이 오른쪽 위 모서리는 경우는 note 합니다. 이러한 점은 이전 `CreateLinearGradient` 호출에 표시 된 것과 동일한 그라데이션 점이 아닙니다. 그러나 이러한 점은 캔버스의 중심을 bisects 하는 가로 그라데이션 선에 _평행이_ 되며 동일한 그라데이션을 생성 합니다.

**무지개 그라데이션**

**무지개 그라데이션** 페이지는 캔버스의 왼쪽 위 모퉁이에서 오른쪽 아래 모퉁이로 무지개를 그립니다. 하지만이 레인 보우 그라데이션 실제 rainbow 같은 하지 않습니다. 곡선을 보다는 직선 있지만 기반으로 360 색상 값 0에서 순환 의해 결정 되는 8 개의 HSL (색상-채도-명도) 색:

```csharp
SKColor[] colors = new SKColor[8];

for (int i = 0; i < colors.Length; i++)
{
    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
}
```

해당 코드는 아래에 표시 된 `PaintSurface` 처리기의 일부입니다. 처리기는 오른쪽 아래 모서리를 캔버스의 왼쪽 위 모퉁이에서 확장 하는 6 면 다각형을 정의 하는 경로 만들어 시작 합니다.

```csharp
public class RainbowGradientPage : ContentPage
{
    public RainbowGradientPage ()
    {
        Title = "Rainbow Gradient";

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

        using (SKPath path = new SKPath())
        {
            float rainbowWidth = Math.Min(info.Width, info.Height) / 2f;

            // Create path from upper-left to lower-right corner
            path.MoveTo(0, 0);
            path.LineTo(rainbowWidth / 2, 0);
            path.LineTo(info.Width, info.Height - rainbowWidth / 2);
            path.LineTo(info.Width, info.Height);
            path.LineTo(info.Width - rainbowWidth / 2, info.Height);
            path.LineTo(0, rainbowWidth / 2);
            path.Close();

            using (SKPaint paint = new SKPaint())
            {
                SKColor[] colors = new SKColor[8];

                for (int i = 0; i < colors.Length; i++)
                {
                    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
                }

                paint.Shader = SKShader.CreateLinearGradient(
                                    new SKPoint(0, rainbowWidth / 2), 
                                    new SKPoint(rainbowWidth / 2, 0),
                                    colors,
                                    null,
                                    SKShaderTileMode.Repeat);

                canvas.DrawPath(path, paint);
            }
        }
    }
}
```

`CreateLinearGradient` 메서드의 두 그라데이션 지점은이 경로를 정의 하는 두 요소를 기준으로 합니다. 두 점이 모두 왼쪽 위 모퉁이에 가깝습니다. 첫 번째는 캔버스의 위쪽 가장자리에서 이며 두 번째 캔버스의 왼쪽된 가장자리입니다. 결과:

[![무지개 오류가 잘못 되었습니다.](linear-gradient-images/RainbowGradientFaulty.png "무지개 오류가 잘못 되었습니다.")](linear-gradient-images/RainbowGradientFaulty-Large.png#lightbox)

이 흥미로운 이미지로 이지만 의도 상당히 아닙니다. 문제는 선형 그라데이션을 만들 때 고정 색상의 줄을 수직 그라데이션 선으로. 그라데이션 선은 그림 위쪽 및 왼쪽 면에 닿으면 해당 줄이 그림의 오른쪽 아래 모서리를 확장 하는 가장자리에 수직인 일반적으로 요소를 기반으로 합니다. 이 방법은 캔버스 제곱 된 경우에 작동 합니다.

적절 한 레인 보우 그라데이션을 만들려면 그라데이션 선을 무지개의 가장자리에 수직 이어야 합니다. 더 복잡된 한 계산입니다. 벡터 그림의 옆에 병렬 되는 정의 되어야 합니다. 벡터는 쪽에 수직인 있도록 90도 회전된 합니다. 그런 다음 `rainbowWidth`를 곱하여 그림의 너비가 되도록 합니다. 벡터와 해당 시점 및 그림에서 어느 지점에서 두 그라데이션 점 기반 계산 됩니다. [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **무지개 그라데이션** 페이지에 표시 되는 코드는 다음과 같습니다.

```csharp
public class RainbowGradientPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        using (SKPath path = new SKPath())
        {
            ···
            using (SKPaint paint = new SKPaint())
            {
                ···
                // Vector on lower-left edge, from top to bottom 
                SKPoint edgeVector = new SKPoint(info.Width - rainbowWidth / 2, info.Height) - 
                                     new SKPoint(0, rainbowWidth / 2);

                // Rotate 90 degrees counter-clockwise:
                SKPoint gradientVector = new SKPoint(edgeVector.Y, -edgeVector.X);

                // Normalize
                float length = (float)Math.Sqrt(Math.Pow(gradientVector.X, 2) +
                                                Math.Pow(gradientVector.Y, 2));
                gradientVector.X /= length;
                gradientVector.Y /= length;

                // Make it the width of the rainbow
                gradientVector.X *= rainbowWidth;
                gradientVector.Y *= rainbowWidth;

                // Calculate the two points
                SKPoint point1 = new SKPoint(0, rainbowWidth / 2);
                SKPoint point2 = point1 + gradientVector;

                paint.Shader = SKShader.CreateLinearGradient(point1,
                                                             point2,
                                                             colors,
                                                             null,
                                                             SKShaderTileMode.Repeat);

                canvas.DrawPath(path, paint);
            }
        }
    }
}
```

이제 무지개 색 그림에 맞춰집니다.

[![무지개 그라데이션](linear-gradient-images/RainbowGradient.png "무지개 그라데이션")](linear-gradient-images/RainbowGradient-Large.png#lightbox)

**무한대 색**

무지개 그라데이션은 **Infinity 색** 페이지에도 사용 됩니다. 이 페이지에서는 [**세 가지 유형의 베 지 어 곡선**](../../curves/beziers.md#bezier-curve-approximation-to-circular-arcs)에 설명 된 경로 개체를 사용 하 여 infinity 기호를 그립니다. 다음 이미지는 이미지에서 지속적으로 추가 하는 애니메이션된 rainbow 그라데이션 색이 지정 됩니다.

생성자는 infinity 기호를 설명 하는 `SKPath` 개체를 만듭니다. 경로 만든 후 생성자 경로의 사각형 범위를 가져올 수도 있습니다. 그런 다음 `gradientCycleLength`라는 값을 계산 합니다. 그라데이션이 `pathBounds` 사각형의 왼쪽 위와 오른쪽 아래 모퉁이를 기준으로 하는 경우이 `gradientCycleLength` 값은 그라데이션 패턴의 총 가로 너비입니다.

```csharp
public class InfinityColorsPage : ContentPage
{
    ···
    SKCanvasView canvasView;

    // Path information 
    SKPath infinityPath;
    SKRect pathBounds;
    float gradientCycleLength;

    // Gradient information
    SKColor[] colors = new SKColor[8];
    ···

    public InfinityColorsPage ()
    {
        Title = "Infinity Colors";

        // Create path for infinity sign
        infinityPath = new SKPath();
        infinityPath.MoveTo(0, 0);                                  // Center
        infinityPath.CubicTo(  50,  -50,   95, -100,  150, -100);   // To top of right loop
        infinityPath.CubicTo( 205, -100,  250,  -55,  250,    0);   // To far right of right loop
        infinityPath.CubicTo( 250,   55,  205,  100,  150,  100);   // To bottom of right loop
        infinityPath.CubicTo(  95,  100,   50,   50,    0,    0);   // Back to center  
        infinityPath.CubicTo( -50,  -50,  -95, -100, -150, -100);   // To top of left loop
        infinityPath.CubicTo(-205, -100, -250,  -55, -250,    0);   // To far left of left loop
        infinityPath.CubicTo(-250,   55, -205,  100, -150,  100);   // To bottom of left loop
        infinityPath.CubicTo( -95,  100, - 50,   50,    0,    0);   // Back to center
        infinityPath.Close();

        // Calculate path information 
        pathBounds = infinityPath.Bounds;
        gradientCycleLength = pathBounds.Width +
            pathBounds.Height * pathBounds.Height / pathBounds.Width;

        // Create SKColor array for gradient
        for (int i = 0; i < colors.Length; i++)
        {
            colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
        }

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
}
```

또한 생성자는 레인 및 `SKCanvasView` 개체에 대 한 `colors` 배열을 만듭니다.

`OnAppearing` 및 `OnDisappearing` 메서드의 재정의는 애니메이션에 대 한 오버 헤드를 수행 합니다. `OnTimerTick` 메서드는 `offset` 필드에 0에서 2 초 마다 `gradientCycleLength` 애니메이션 효과를 적용 합니다.

```csharp
public class InfinityColorsPage : ContentPage
{
    ···
    // For animation
    bool isAnimating;
    float offset;
    Stopwatch stopwatch = new Stopwatch();
    ···

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 2;     // seconds
        double progress = stopwatch.Elapsed.TotalSeconds % duration / duration;
        offset = (float)(gradientCycleLength * progress);
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

마지막으로, `PaintSurface` 처리기가 infinity 기호를 렌더링 합니다. 경로에는 (0, 0) 중심점을 둘러싼 음수 및 양의 좌표가 포함 되어 있으므로 캔버스의 `Translate` 변환을 사용 하 여 가운데로 이동 합니다. 변환 변환 뒤에는 캔버스의 너비와 높이 중 95% 내에서 계속 유지 하면서 가능한 한 많은 양의 무한대 기호를 사용 하는 배율 인수를 적용 하는 `Scale` 변환이 있습니다. 

`STROKE_WIDTH` 상수는 경로 경계 사각형의 너비와 높이에 추가 됩니다. 네 면 모두 해당 너비의 절반으로 렌더링 된 무한대 크기의 크기가 증가 하므로이 너비의 줄을 사용 하 여 경로 스트로크는:

```csharp
public class InfinityColorsPage : ContentPage
{
    const int STROKE_WIDTH = 50;
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set transforms to shift path to center and scale to canvas size
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.95f * 
            Math.Min(info.Width / (pathBounds.Width + STROKE_WIDTH),
                     info.Height / (pathBounds.Height + STROKE_WIDTH)));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = STROKE_WIDTH;
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(pathBounds.Left, pathBounds.Top),
                                new SKPoint(pathBounds.Right, pathBounds.Bottom),
                                colors,
                                null,
                                SKShaderTileMode.Repeat,
                                SKMatrix.MakeTranslation(offset, 0));

            canvas.DrawPath(infinityPath, paint);
        }
    }
}
```

`SKShader.CreateLinearGradient`의 처음 두 인수로 전달 된 요소를 확인 합니다. 해당 지점에 경계 사각형의 원래 경로 기반으로 합니다. 첫 번째 지점은 (&ndash;250, &ndash;100)이 고 두 번째 지점은 (250, 100)입니다. SkiaSharp 내부 해당 지점에 종속 됩니다 현재 캔버스 변환을 표시 무한대 기호를 사용 하 여 올바르게 정렬 되도록 합니다.

`CreateLinearGradient`에 대 한 마지막 인수를 사용 하지 않으면 infinity 부호의 왼쪽 위에서 오른쪽 아래로 확장 되는 무지개 그라데이션이 표시 됩니다. (실제로 그라데이션의 확장 왼쪽 위 모서리에서 경계 사각형의 오른쪽 아래 모퉁이에 있습니다. 렌더링 된 infinity 기호는 모든 측면에서 `STROKE_WIDTH` 값의 절반을 기준으로 경계 사각형 보다 큽니다. 그라데이션은 시작과 끝 모두에서 빨간색이 고 `SKShaderTileMode.Repeat`를 사용 하 여 그라데이션을 생성 하므로 차이가 눈에 띄는 것은 아닙니다.)

`CreateLinearGradient`에 대 한 마지막 인수를 사용 하 여 그라데이션 패턴은 이미지 전체에 걸쳐 계속 스윕 됩니다.

[![무한대 색](linear-gradient-images/InfinityColors.png "무한대 색")](linear-gradient-images/InfinityColors-Large.png#lightbox)

## <a name="transparency-and-gradients"></a>투명도 및 그라데이션과

그라데이션에 영향을 주는 색 투명도 통합할 수 있습니다. 다른 색에서 간의 페이드 그라데이션을 대신 그라데이션의 투명 색에서 페이드 수 있습니다. 

몇 가지 흥미로운 효과 대 한이 기법을 사용할 수 있습니다. 클래식 예제 중 하나는 리플렉션 사용 하 여 그래픽 개체를 보여 줍니다.

[![반사 그라데이션](linear-gradient-images/ReflectionGradient.png "반사 그라데이션")](linear-gradient-images/ReflectionGradient-Large.png#lightbox)

거꾸로 된 텍스트를 50% 맨 아래에 완전히 투명 하 게 맨 위에 있는 투명 한 그라데이션 색입니다. 이러한 수준의 투명도 0x80의 알파 값 및 0와 연결 됩니다.

**반사 그라데이션** 페이지의 `PaintSurface` 처리기는 텍스트 크기를 캔버스 너비의 90%로 조정 합니다. 그런 다음 `xText` 및 `yText` 값을 계산 하 여 텍스트를 가로로 가운데에 맞추고 페이지의 세로 가운데에 해당 하는 기준선에 배치 합니다.

```csharp
public class ReflectionGradientPage : ContentPage
{
    const string TEXT = "Reflection";

    public ReflectionGradientPage ()
    {
        Title = "Reflection Gradient";

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

            // Scale the canvas to flip upside-down around the vertical center
            canvas.Scale(1, -1, 0, yText);

            // Draw reflected text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

이러한 `xText` 및 `yText` 값은 `PaintSurface` 처리기 아래쪽의 `DrawText` 호출에 반영 된 텍스트를 표시 하는 데 사용 되는 값과 같습니다. 그러나이 코드 바로 앞에서 `SKCanvas`의 `Scale` 메서드에 대 한 호출을 볼 수 있습니다. 이 `Scale` 메서드는 행을 1 (아무 것도 수행 하지 않음)으로 수직 확장 하지만, 1을 기준으로 수직 확장 하 &ndash;여 모든 항목을 효과적으로 대칭 이동 합니다. 회전 중심은 점 (0, `yText`)으로 설정 됩니다. 여기서 `yText`는 캔버스의 세로 중심 이며 원래는 `info.Height` 2로 나눈 값입니다.

Skia 그라데이션을 사용 하 여 캔버스 변환 하기 전에 그래픽 개체에 색을 염두에 두어야 합니다. 반사 되지 않은 텍스트가 그려진 후에는 표시 된 텍스트에 해당 하는 `textBounds` 사각형이 이동 됩니다.

```csharp
textBounds.Offset(xText, yText);
```

`CreateLinearGradient` 호출은 사각형 위쪽부터 아래쪽 까지의 그라데이션을 정의 합니다. 그라데이션은 완전히 투명 한 파란색 (`paint.Color.WithAlpha(0)`)에서 50% 투명 한 파랑 (`paint.Color.WithAlpha(0x80)`) 사이입니다. 캔버스 변환을 50% 투명 한 파랑 기준선에서 시작 하 고 텍스트의 맨 위에 있는 투명 하 게 되므로 거꾸로 텍스트를 대칭 이동 합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
