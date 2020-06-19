---
title: SkiaSharp 선형 그라데이션
description: 두 색의 점진적 혼합으로 구성 된 그라데이션을 사용 하 여 선 또는 채우기 영역을 스트로크 하는 방법에 대해 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 20A2A8C4-FEB7-478D-BF57-C92E26117B6A
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 43aa429046c1b0f72a1cbe6a5b921da9b8907a49
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84132225"
---
# <a name="the-skiasharp-linear-gradient"></a>SkiaSharp 선형 그라데이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[`SKPaint`](xref:SkiaSharp.SKPaint)클래스는 [`Color`](xref:SkiaSharp.SKPaint.Color) 단색으로 선 또는 채우기 영역을 스트로크 하는 데 사용 되는 속성을 정의 합니다. 또는 색의 점진적 혼합 인 _그라데이션으로_선 또는 채우기 영역을 채울 수 있습니다.

![선형 그라데이션 샘플](linear-gradient-images/LinearGradientSample.png "선형 그라데이션 샘플")

가장 기본적인 그라데이션 유형은 _선형_ 그라데이션입니다. 색 혼합은 한 점에서 다른 점으로 선 ( _그라데이션 선_이라고 함)에서 발생 합니다. 그라데이션 선에 수직인 선은 동일한 색을 갖습니다. 두 정적 메서드 중 하나를 사용 하 여 선형 그라데이션을 만듭니다 [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*) . 두 오버 로드 간의 차이점은 행렬 변환이 포함 되 고 다른 오버 로드는 그렇지 않은 경우입니다. 

이러한 메서드는 [`SKShader`](xref:SkiaSharp.SKShader) 의 속성으로 설정 된 형식의 개체를 반환 [`Shader`](xref:SkiaSharp.SKPaint.Shader) `SKPaint` 합니다. `Shader`속성이 null이 아닌 경우 속성을 재정의 `Color` 합니다. 스트로크 하는 선 또는이 개체를 사용 하 여 채워진 모든 영역 `SKPaint` 에는 단색이 아닌 그라데이션을 기반으로 합니다.

> [!NOTE]
> `Shader`호출에 개체를 포함 하는 경우 속성이 무시 됩니다 `SKPaint` `DrawBitmap` . `Color` `SKPaint` [SkiaSharp 비트맵 표시](../../bitmaps/displaying.md#displaying-in-pixel-dimensions)문서에 설명 된 대로의 속성을 사용 하 여 비트맵을 표시 하기 위한 투명도 수준을 설정할 수 있지만 그라데이션 투명도를 사용 하 `Shader` 여 비트맵을 표시 하기 위해 속성을 사용할 수는 없습니다. 그라데이션 투명 효과를 사용 하 여 비트맵을 표시 하는 데 사용할 수 있는 다른 기술은 다음과 같습니다. [SkiaSharp 원형 그라데이션과](circular-gradients.md#radial-gradients-for-masking) [SkiaSharp 합성 및 blend 모드](../blend-modes/porter-duff.md#gradient-transparency-and-transitions)문서에 설명 되어 있습니다.

## <a name="corner-to-corner-gradients"></a>모퉁이 간 그라데이션

선형 그라데이션은 사각형의 한쪽 모퉁이에서 다른 모퉁이까지 확장 되는 경우가 많습니다. 시작점이 사각형의 왼쪽 위 모퉁이가 면 그라데이션이 확장 될 수 있습니다.

- 왼쪽 아래 모퉁이에 수직으로
- 가로 오른쪽 위 모퉁이까지
- 오른쪽 아래 모서리에 대각선으로

대각선 선형 그라데이션은 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **SkiaSharp 셰이더 및 기타 효과** 섹션에 있는 첫 번째 페이지에서 보여 줍니다. **모퉁이-모퉁이 그라데이션** 페이지는 `SKCanvasView` 해당 생성자에를 만듭니다. `PaintSurface`처리기는 `SKPaint` 문에 개체를 만든 `using` 다음 캔버스를 중심으로 300 픽셀 사각형 사각형을 정의 합니다.

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

`Shader`의 속성에는 `SKPaint` `SKShader` 정적 메서드의 반환 값이 할당 됩니다 `SKShader.CreateLinearGradient` . 5 개의 인수는 다음과 같습니다.

- 그라데이션의 시작점입니다. 여기에서 사각형의 왼쪽 위 모퉁이로 설정 합니다.
- 그라데이션의 끝점입니다. 여기에서 사각형의 오른쪽 아래 모서리로 설정 합니다.
- 그라데이션에 기여 하는 둘 이상의 색으로 이루어진 배열입니다.
- `float`그라데이션 선 내에서 색의 상대 위치를 나타내는 값의 배열입니다.
- 그라데이션이 [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) 그라데이션 선의 끝을 넘어 동작 하는 방식을 나타내는 열거형의 멤버입니다.

그라데이션 개체를 만든 후이 메서드는 `DrawRect` 셰이더를 포함 하는 개체를 사용 하 여 300 픽셀 사각형을 그립니다 `SKPaint` . IOS, Android 및 UWP (유니버설 Windows 플랫폼)에서 실행 되 고 있습니다.

[![모퉁이 간 그라데이션](linear-gradient-images/CornerToCornerGradient.png "모퉁이 간 그라데이션")](linear-gradient-images/CornerToCornerGradient-Large.png#lightbox)

그라데이션 선은 처음 두 인수로 지정 된 두 개의 점으로 정의 됩니다. 이러한 점은 그라데이션으로 표시 되는 그래픽 개체가 _아니라_ _캔버스_ 를 기준으로 합니다. 그라데이션 선을 따라 색은 왼쪽 위의 빨강에서 오른쪽 아래의 파란색으로 점차 전환 됩니다. 그라데이션 선에 수직인 모든 줄에는 일정 한 색이 있습니다.

`float`네 번째 인수로 지정 된 값 배열에는 색 배열과 일 대 일 대응 관계가 있습니다. 값은 이러한 색이 발생 하는 그라데이션 선을 따라 상대적인 위치를 표시 합니다. 여기서 0은 `Red` 그라데이션 선의 시작 부분에서 발생 함을 의미 하 고 1은 `Blue` 줄의 끝에서 발생 함을 의미 합니다. 숫자는 오름차순 이어야 하며 0에서 1 사이 여야 합니다. 해당 범위에 없으면 해당 범위에 맞게 조정 됩니다.

배열의 두 값은 0과 1이 아닌 값으로 설정할 수 있습니다. 다음을 실행해보세요.

```csharp
new float[] { 0.25f, 0.75f }
```

이제 그라데이션 선의 전체 1/4 분기가 순수한 빨강 이며 마지막 분기가 순수한 파랑입니다. 빨강 및 파랑의 혼합은 그라데이션 선의 중앙 절반으로 제한 됩니다.

일반적으로 이러한 위치 값을 0에서 1로 동일 하 게 지정할 수 있습니다. 이 경우의 `null` 네 번째 인수로를에 제공 하면 됩니다 `CreateLinearGradient` .

이 그라데이션은 300-픽셀 사각형 사각형의 두 모퉁이 사이에 정의 되지만 해당 사각형의 채우기로 제한 되지 않습니다. **모퉁이 간 그라데이션** 페이지에는 페이지에서 탭 하거나 마우스를 클릭 하면 응답 하는 추가 코드가 포함 됩니다. `drawBackground`각 탭에서 필드가 전환 `true` 됩니다 `false` . 값이 이면 `true` `PaintSurface` 처리기는 동일한 개체를 사용 하 여 `SKPaint` 전체 캔버스를 채운 다음 작은 사각형을 나타내는 검정색 사각형을 그립니다. 

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

화면을 탭 하면 다음과 같이 표시 됩니다.

[![모퉁이 간 그라데이션 전체](linear-gradient-images/CornerToCornerGradientFull.png "모퉁이 간 그라데이션 전체")](linear-gradient-images/CornerToCornerGradientFull-Large.png#lightbox)

그라데이션은 그라데이션 선을 정의 하는 점을 벗어나 동일한 패턴으로 반복 됩니다. 이 반복은의 마지막 인수가 이기 때문에 발생 합니다 `CreateLinearGradient` `SKShaderTileMode.Repeat` . 다른 옵션을 곧 볼 수 있습니다.

또한 그라데이션 선을 지정 하는 데 사용 하는 점이 고유 하지 않습니다. 그라데이션 선에 수직인 선은 동일한 색을 가지 므로 동일한 효과에 대해 지정할 수 있는 그라데이션 선 수는 무한 합니다. 예를 들어 가로 그라데이션을 사용 하 여 사각형을 채울 때 왼쪽 위 및 오른쪽 위 모퉁이 또는 왼쪽 아래와 오른쪽 아래 모퉁이를 지정 하거나 해당 줄과 함께 사용 되는 두 개의 요소를 지정할 수 있습니다.

## <a name="interactively-experiment"></a>대화형 실험

**대화형 선형 그라데이션** 페이지를 사용 하 여 대화형으로 선형 그라데이션을 시험해 볼 수 있습니다. 이 페이지에서는 `InteractivePage` [**호를 그리기 위한 세 가지 방법**](../../curves/arcs.md)문서에 소개 된 클래스를 사용 합니다 .는 `InteractivePage` [`TouchEffect`](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md) `TouchPoint` 손가락 또는 마우스를 사용 하 여 이동할 수 있는 개체의 컬렉션을 유지 관리 하는 이벤트를 처리 합니다.

XAML 파일은를의 `TouchEffect` 부모에 연결 `SKCanvasView` 하 고, `Picker` 열거형의 세 멤버 중 하나를 선택할 수 있는도 포함 합니다 [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) .

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

코드 숨김이 있는 파일의 생성자는 `TouchPoint` 선형 그라데이션의 시작점과 끝점에 대 한 두 개의 개체를 만듭니다. `PaintSurface`처리기는 세 가지 색의 배열을 정의 하 고 (그라데이션을 빨강에서 녹색으로 파랑으로)에서 현재를 가져옵니다 `SKShaderTileMode` `Picker` .

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

`PaintSurface`처리기는 `SKShader` 모든 정보에서 개체를 만들고이를 사용 하 여 전체 캔버스를 색으로 만듭니다. 값의 배열은 `float` 로 설정 됩니다 `null` . 그렇지 않고 세 가지 색의 공간을 동일 하 게 하려면 해당 매개 변수를 0, 0.5 및 1 값을 가진 배열로 설정 합니다.

처리기의 대부분은 다음과 같은 `PaintSurface` 여러 개체를 표시 하는 데 유용 합니다. 터치 지점은 윤곽선 원, 그라데이션 선 및 터치 점에서 그라데이션 선에 수직인 선입니다.

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

두 접점를 연결 하는 그라데이션 선은 쉽게 그릴 수 있지만 수직 선에는 약간의 추가 작업이 필요 합니다. 그라데이션 선은 한 단위의 길이로 정규화 된 벡터로 변환 된 다음 90 각도로 회전 됩니다. 그러면 해당 벡터의 길이가 200 픽셀로 지정 됩니다. 이는 터치 점에서 선형 선으로 확장 하는 4 개의 선을 그리는 데 사용 됩니다.

수직인 선은 그라데이션의 시작과 끝과 일치 합니다. 이러한 줄을 벗어나는 결과는 열거의 설정에 따라 달라 집니다 `SKShaderTileMode` .

[![대화형 선형 그라데이션](linear-gradient-images/InteractiveLinearGradient.png "대화형 선형 그라데이션")](linear-gradient-images/InteractiveLinearGradient-Large.png#lightbox)

3 개의 스크린샷은의 세 가지 다른 값에 대 한 결과를 보여 줍니다 [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) . IOS 스크린 샷에서는 `SKShaderTileMode.Clamp` 그라데이션 테두리의 색을 확장 하는을 보여 줍니다. `SKShaderTileMode.Repeat`Android 스크린 샷의 옵션은 그라데이션 패턴이 반복 되는 방법을 보여 줍니다. `SKShaderTileMode.Mirror`UWP 스크린샷의 옵션도 패턴을 반복 하지만 매번 패턴이 반대로 바뀌어 색 불연속성 발생 하지 않습니다.

## <a name="gradients-on-gradients"></a>그라데이션의 그라데이션

`SKShader`클래스는를 제외 하 고 공용 속성 또는 메서드를 정의 하지 않습니다 `Dispose` . `SKShader`따라서 해당 정적 메서드로 만든 개체는 변경할 수 없습니다. 서로 다른 두 개체에 동일한 그라데이션을 사용 하는 경우에도 그라데이션을 약간 다르게 변경할 가능성이 높습니다. 이렇게 하려면 새 개체를 만들어야 `SKShader` 합니다.

**그라데이션 텍스트** 페이지에는 비슷한 그라데이션으로 색이 지정 된 텍스트와 백그라운드 표시 됩니다.

[![그라데이션 텍스트](linear-gradient-images/GradientText.png "그라데이션 텍스트")](linear-gradient-images/GradientText-Large.png#lightbox)

그라데이션의 유일한 차이점은 시작점과 끝점입니다. 텍스트를 표시 하는 데 사용 되는 그라데이션은 텍스트에 대 한 경계 사각형의 모퉁이에 있는 두 요소를 기준으로 합니다. 배경의 경우 두 점은 전체 캔버스를 기반으로 합니다. 코드는 다음과 같습니다.

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

`Shader`개체의 속성은 `SKPaint` 배경을 포함 하는 그라데이션을 표시 하기 위해 먼저 설정 됩니다. 그라데이션 지점은 캔버스의 왼쪽 위와 오른쪽 아래 모퉁이로 설정 됩니다.

코드는 `TextSize` `SKPaint` 텍스트를 캔버스 너비의 90%에 표시 하도록 개체의 속성을 설정 합니다. 텍스트 범위는 `xText` `yText` 텍스트를 `DrawText` 가운데 맞춤 하기 위해 메서드에 전달할 및 값을 계산 하는 데 사용 됩니다.

그러나 두 번째 호출의 그라데이션 지점은 `CreateLinearGradient` 캔버스를 표시할 때 캔버스를 기준으로 텍스트의 왼쪽 위와 오른쪽 아래 모퉁이를 참조 해야 합니다. 이 `textBounds` 작업은 사각형을 같은 및 값으로 이동 하 여 수행 됩니다 `xText` `yText` .

```csharp
textBounds.Offset(xText, yText);
```

이제 사각형의 왼쪽 위와 오른쪽 아래 모퉁이를 사용 하 여 그라데이션의 시작점과 끝점을 설정할 수 있습니다.

## <a name="animating-a-gradient"></a>그라데이션에 애니메이션 적용

여러 가지 방법으로 그라데이션을 애니메이션에 적용할 수 있습니다. 한 가지 방법은 시작점과 끝점에 애니메이션 효과를 주는 것입니다. **그라데이션 애니메이션** 페이지는 캔버스를 중심으로 하는 원에서 두 지점을 이동 합니다. 이 원의 반지름은 캔버스의 너비 또는 높이 중 더 작은 쪽의 절반입니다. 시작점과 끝점은이 원의 서로 반대 이며, 그라데이션은 타일 모드에서 흰색에서 검은색으로 이동 합니다 `Mirror` .

[![그라데이션 애니메이션](linear-gradient-images/GradientAnimation.png "그라데이션 애니메이션")](linear-gradient-images/GradientAnimation-Large.png#lightbox)

생성자는를 만듭니다 `SKCanvasView` . `OnAppearing`및 `OnDisappearing` 메서드는 애니메이션 논리를 처리 합니다.

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

`OnTimerTick`메서드는 `angle` 3 초 마다 0에서 2π까지 애니메이션 효과를 주는 값을 계산 합니다. 

두 개의 그라데이션 지점을 계산 하는 한 가지 방법은 다음과 같습니다. `SKPoint`이라는 값 `vector` 은 캔버스의 중심에서 원 반지름의 점으로 확장 되도록 계산 됩니다. 이 벡터의 방향은 각도의 사인 및 코사인 값을 기준으로 합니다. 그런 다음 두 개의 반대 그라데이션 점이 계산 됩니다. 한 점은 중심점에서 해당 벡터를 빼서 계산 하 고 다른 점은 벡터를 중심점에 추가 하 여 계산 됩니다.

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

약간 다른 방법으로는 더 작은 코드가 필요 합니다. 이 접근 방식을 사용 하면 [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) 행렬 변환이 있는 오버 로드 메서드를 마지막 인수로 사용 합니다. 이 방법은 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 버전입니다.

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

캔버스의 너비가 높이 보다 작은 경우 두 그라데이션 지점은 (0, 0) 및 ( `info.Width` , 0)로 설정 됩니다. 마지막 인수로 전달 되는 회전 변환은 `CreateLinearGradient` 화면 중심을 중심으로 두 개의 요소를 효과적으로 회전 합니다.

각도가 0 인 경우에는 회전이 없으며 두 개의 그라데이션 점이 캔버스의 왼쪽 위와 오른쪽 위 모퉁이에 있습니다. 이러한 요소는 이전 호출에서와 같이 계산 된 것과 동일한 그라데이션 점이 아닙니다 `CreateLinearGradient` . 그러나 이러한 점은 캔버스의 중심을 bisects 하는 가로 그라데이션 선에 _평행이_ 되며 동일한 그라데이션을 생성 합니다.

**무지개 그라데이션**

**무지개 그라데이션** 페이지는 캔버스의 왼쪽 위 모퉁이에서 오른쪽 아래 모퉁이로 무지개를 그립니다. 그러나이 무지개 그라데이션은 실제 무지개와 유사 하지 않습니다. 곡선이 아닌 직선 이지만 0에서 360 사이의 색조 값을 순환 하 여 결정 되는 8 개의 HSL (색조-채도) 색을 기준으로 합니다.

```csharp
SKColor[] colors = new SKColor[8];

for (int i = 0; i < colors.Length; i++)
{
    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
}
```

해당 코드는 아래에 표시 된 처리기의 일부입니다 `PaintSurface` . 처리기는 캔버스의 왼쪽 위 모퉁이에서 오른쪽 아래 모퉁이로 확장 되는 6 면 다각형을 정의 하는 경로를 만듭니다.

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

메서드의 두 그라데이션 지점은 `CreateLinearGradient` 이 경로를 정의 하는 두 개의 점에 기반 합니다. 두 점이 모두 왼쪽 위 모퉁이에 가깝습니다. 첫 번째는 캔버스의 위쪽 가장자리에 있고, 두 번째는 캔버스의 왼쪽 가장자리에 있습니다. 결과는 다음과 같습니다.

[![무지개 오류가 잘못 되었습니다.](linear-gradient-images/RainbowGradientFaulty.png "무지개 오류가 잘못 되었습니다.")](linear-gradient-images/RainbowGradientFaulty-Large.png#lightbox)

이는 흥미로운 이미지 이지만 의도 한 것이 아닙니다. 문제는 선형 그라데이션을 만들 때 상수 색의 줄이 그라데이션 선에 수직인 것입니다. 그라데이션 선은 그림의 위쪽과 왼쪽이 교차 하는 지점을 기반으로 하며, 해당 선은 일반적으로 오른쪽 아래 모퉁이로 확장 되는 그림의 가장자리와 수직이 아닙니다. 이 방법은 캔버스를 정사각형으로 사용 하는 경우에만 작동 합니다.

적절 한 무지개 그라데이션을 만들려면 그라데이션 선이 레인의 가장자리와 수직이 야 합니다. 더 많은 계산을 포함 합니다. 벡터는 그림의 긴 쪽에 평행 하 게 정의 해야 합니다. 벡터는 해당 면에 수직이 되도록 90도 회전 됩니다. 그런 다음를 곱하여 그림의 너비가 되도록 `rainbowWidth` 합니다. 두 그라데이션 지점은 그림의 측면에 있는 점을 기반으로 계산 되며, 해당 점과 벡터를 더한 값입니다. [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **무지개 그라데이션** 페이지에 표시 되는 코드는 다음과 같습니다.

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

이제 무지개 색은 그림에 맞춰 정렬 됩니다.

[![무지개 그라데이션](linear-gradient-images/RainbowGradient.png "무지개 그라데이션")](linear-gradient-images/RainbowGradient-Large.png#lightbox)

**무한대 색**

무지개 그라데이션은 **Infinity 색** 페이지에도 사용 됩니다. 이 페이지에서는 [**세 가지 유형의 베 지 어 곡선**](../../curves/beziers.md#bezier-curve-approximation-to-circular-arcs)에 설명 된 경로 개체를 사용 하 여 infinity 기호를 그립니다. 그런 다음 이미지를 연속 해 서 스윕 하는 애니메이션 레인 그라데이션으로 이미지를 색으로 지정 합니다.

생성자는 `SKPath` 무한대 기호를 설명 하는 개체를 만듭니다. 경로를 만든 후에는 생성자가 경로의 사각형 범위를 가져올 수도 있습니다. 그런 다음 라는 값을 계산 `gradientCycleLength` 합니다. 그라데이션이 사각형의 왼쪽 위와 오른쪽 아래 모퉁이를 기준으로 하는 경우 `pathBounds` 이 `gradientCycleLength` 값은 그라데이션 패턴의 총 가로 너비입니다.

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

또한 생성자는 레인의 `colors` 배열 및 개체를 만듭니다 `SKCanvasView` .

`OnAppearing`및 메서드의 재정의는 `OnDisappearing` 애니메이션에 대 한 오버 헤드를 수행 합니다. `OnTimerTick`메서드는 필드에 `offset` 0부터 2 초 마다 애니메이션 효과를 적용 합니다 `gradientCycleLength` .

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

마지막으로 `PaintSurface` 처리기가 infinity 기호를 렌더링 합니다. 경로에는 (0, 0) 중심점을 둘러싼 음수 및 양의 좌표가 포함 되어 있으므로 캔버스의 `Translate` 변환을 사용 하 여 가운데로 이동 합니다. 변환 변환 뒤에는 `Scale` 캔버스의 너비와 높이 중 95% 내에서 계속 유지 하면서 가능한 한 많은 양의 무한대 기호를 사용 하는 배율 인수를 적용 하는 변환이 적용 됩니다. 

`STROKE_WIDTH`상수는 경로 경계 사각형의 너비와 높이에 추가 됩니다. 이 너비의 줄을 사용 하 여 경로를 스트로크 설정 하므로 렌더링 된 infinity 크기의 크기는 4 면의 절반 너비 만큼 증가 합니다.

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

의 처음 두 인수로 전달 된 요소를 확인 `SKShader.CreateLinearGradient` 합니다. 이러한 점은 원래 경로 경계 사각형을 기반으로 합니다. 첫 번째 지점은 ( &ndash; 250, &ndash; 100)이 고 두 번째 지점은 (250, 100)입니다. SkiaSharp 내부에서는 이러한 점이 현재 캔버스 변환에 적용 되므로 표시 된 infinity 부호와 올바르게 맞춰집니다.

에 대 한 마지막 인수를 사용 하지 않으면 `CreateLinearGradient` infinity 부호의 왼쪽 위에서 오른쪽 아래로 확장 되는 무지개 그라데이션이 표시 됩니다. 실제로 그라데이션은 왼쪽 위 모퉁이에서 경계 사각형의 오른쪽 아래 모퉁이로 확장 됩니다. 렌더링 된 infinity 기호가 모든 변의 값의 절반을 기준으로 경계 사각형 보다 큽니다 `STROKE_WIDTH` . 그라데이션은 시작과 끝 모두에서 빨간색이 고 그라데이션을 사용 하 여 만들어지기 때문에 `SKShaderTileMode.Repeat` 차이가 눈에 띄지 않습니다.

에 대 한 마지막 인수를 사용 하 여 `CreateLinearGradient` 그라데이션 패턴은 이미지를 통해 지속적으로 스윕 됩니다.

[![무한대 색](linear-gradient-images/InfinityColors.png "무한대 색")](linear-gradient-images/InfinityColors-Large.png#lightbox)

## <a name="transparency-and-gradients"></a>투명도 및 그라데이션

그라데이션에 영향을 주는 색은 투명도를 통합할 수 있습니다. 한 색에서 다른 색으로 페이드 인 되는 그라데이션 대신 색에서 투명 하 게 그라데이션 효과를 적용할 수 있습니다. 

몇 가지 흥미로운 효과를 위해이 기법을 사용할 수 있습니다. 일반적인 예제 중 하나는 해당 리플렉션을 사용 하는 그래픽 개체를 보여 줍니다.

[![반사 그라데이션](linear-gradient-images/ReflectionGradient.png "반사 그라데이션")](linear-gradient-images/ReflectionGradient-Large.png#lightbox)

아래쪽에 완전히 투명 하 게 위쪽에서 50% 투명 한 그라데이션으로 색이 지정 됩니다. 이러한 투명도 수준은 0x80 및 0의 알파 값과 연결 됩니다.

`PaintSurface` **반사 그라데이션** 페이지의 처리기는 텍스트 크기를 캔버스 너비의 90%로 조정 합니다. 그런 다음 `xText` 및 `yText` 값을 계산 하 여 텍스트를 가로로 가운데에 맞추고 페이지의 세로 가운데에 해당 하는 기준선에 배치 합니다.

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

이러한 `xText` 및 `yText` 값은 `DrawText` 처리기의 맨 아래에 있는 호출에 반영 된 텍스트를 표시 하는 데 사용 되는 값과 같습니다 `PaintSurface` . 그러나이 코드 바로 앞에는의 메서드에 대 한 호출이 표시 됩니다 `Scale` `SKCanvas` . 이 `Scale` 메서드는 가로 방향으로 1 (아무 것도 수행 하지 않음)으로 크기를 조정 하지만 1을 기준으로 세로 방향으로 조정 &ndash; 됩니다. 회전 중심은 점 (0,)으로 설정 됩니다 `yText` `yText` . 여기서는 캔버스의 세로 중심 이며 원래는 `info.Height` 2로 나눈 값입니다.

버 Ia는 캔버스 변환 전에 그라데이션을 사용 하 여 그래픽 개체를 색으로 변환 합니다. 반사 되지 않은 텍스트가 그려진 후에는 `textBounds` 표시 된 텍스트에 해당 하는 사각형을 이동 합니다.

```csharp
textBounds.Offset(xText, yText);
```

`CreateLinearGradient`이 호출은 사각형 위쪽에서 아래쪽으로 향하는 그라데이션을 정의 합니다. 그라데이션은 완전히 투명 한 파란색 ( `paint.Color.WithAlpha(0)` )에서 50% transparent blue ()로 시작 `paint.Color.WithAlpha(0x80)` 합니다. Canvas 변환은 텍스트를 뒤집어 대칭 이동 하므로 50% 투명 파란색은 기준선에서 시작 하 여 텍스트 위쪽에서 투명 하 게 됩니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
