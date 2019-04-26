---
title: SkiaSharp 선형 그라데이션
description: 선을 스트로크 또는 두 가지 색을 단계적으로 혼합 구성 된 그라데이션으로 영역을 채우는 방법을 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 20A2A8C4-FEB7-478D-BF57-C92E26117B6A
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 9551a3b8e093dbb49a55a3761543602c40e81023
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61158543"
---
# <a name="the-skiasharp-linear-gradient"></a>SkiaSharp 선형 그라데이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

합니다 [ `SKPaint` ](xref:SkiaSharp.SKPaint) 클래스 정의 [ `Color` ](xref:SkiaSharp.SKPaint.Color) 줄 스트로크 또는 단색으로 영역을 채우는 데 사용 되는 속성입니다. 또는 줄 스트로크 또는 영역을 채울 수 있습니다 _그라데이션을_, 색의 점진적으로 혼합 된:

![선형 그라데이션 샘플](linear-gradient-images/LinearGradientSample.png "선형 그라데이션 샘플")

그라데이션의 가장 기본적인 형식은 _선형_ 그라데이션 합니다. Blend 색의 줄에 발생 (호출을 _그라데이션 선_) 다른 한 지점에서. 수직 그라데이션 선으로 줄에는 동일한 색을 가집니다. 선형 그라데이션 두 정적 중 하나를 사용 하 여 만든 [ `SKShader.CreateLinearGradient` ](xref:SkiaSharp.SKShader.CreateLinearGradient*) 메서드. 두 오버 로드 간의 차이점은 매트릭스 변환을 포함 한 다른 그렇지 않습니다. 

이러한 메서드는 형식의 개체를 반환 [ `SKShader` ](xref:SkiaSharp.SKShader) 로 설정 하는 [ `Shader` ](xref:SkiaSharp.SKPaint.Shader) 속성 `SKPaint`. 경우는 `Shader` 속성이 null이 아닌, 재정의 `Color` 속성입니다. 스트로크 하는 모든 줄 또는이 사용 하 여 채워진 `SKPaint` 단색을 나타내는 보다는 그라데이션에 개체를 기반으로 합니다.

> [!NOTE]
> 합니다 `Shader` 포함 하는 경우 속성은 무시 됩니다는 `SKPaint` 개체를 `DrawBitmap` 호출 합니다. 사용할 수는 `Color` 속성을 `SKPaint` 비트맵을 표시 하는 것에 대 한 투명도 수준을 설정 하려면 (문서에 설명 된 대로 [SkiaSharp 표시 비트맵](../../bitmaps/displaying.md#displaying-in-pixel-dimensions)), 하지만 사용할 수 없습니다는 `Shader` 표시에 대 한 속성 그라데이션 투명도 사용 하 여 비트맵입니다. 다른 기술을 그라데이션 투명도 사용 하 여 비트맵을 표시 하기 위해 사용할 수 있습니다. 문서에 설명 된 이러한 [SkiaSharp 순환 그라데이션](circular-gradients.md#radial-gradients-for-masking) 하 고 [SkiaSharp 합치기 및 혼합 모드](../blend-modes/porter-duff.md#gradient-transparency-and-transitions)합니다.

## <a name="corner-to-corner-gradients"></a>모퉁이가-그라데이션

종종 선형 그라데이션 다른 영역의 한쪽 모퉁이에서 확장합니다. 시작점을 사각형의 왼쪽 위 모퉁이 그라데이션 확장할 수 있습니다.

- 왼쪽 아래 모서리에 수직으로
- 오른쪽 위 모퉁이에 수평으로
- 오른쪽 아래 모퉁이에 대각선 방향으로

대각선 선형 그라데이션의의 첫 번째 페이지에 설명 되어는 **SkiaSharp 셰이더 및 기타 효과** 섹션을 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 샘플입니다. **모퉁이가-그라데이션** 를 만들고이 `SKCanvasView` 생성자에서. 합니다 `PaintSurface` 처리기를 만듭니다를 `SKPaint` 개체는 `using` 문을 다음 캔버스의 가운데 300 픽셀 사각형 사각형을 정의:

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

`Shader` 속성을 `SKPaint` 할당 되는 `SKShader` 정적에서 값을 반환 `SKShader.CreateLinearGradient` 메서드. 다섯 개의 인수는 다음과 같습니다.

- 그라데이션의 시작점 사각형의 왼쪽 위 모퉁이 여기로.
- 그라데이션의 끝점 사각형의 오른쪽 아래 모서리를 여기로.
- 두 개 이상의 색 그라데이션에 기여 하는 배열
- 배열을 `float` 색 그라데이션 선 내에서 상대 위치를 나타내는 값
- 멤버는 [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode) 그라데이션의 그라데이션 선 끝 외 동작 하는 방법을 나타내는 열거형

그라데이션 개체를 만든 후 합니다 `DrawRect` 메서드를 사용 하 여 300 픽셀 사각형 사각형을 그립니다는 `SKPaint` 셰이더를 포함 하는 개체입니다. 여기 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에서 실행 합니다.

[![모퉁이가-그라데이션](linear-gradient-images/CornerToCornerGradient.png "모퉁이가-그라데이션")](linear-gradient-images/CornerToCornerGradient-Large.png#lightbox)

그라데이션 선은 처음 두 인수로 지정 된 두 개의 점으로 정의 됩니다. 이러한 점을 기준으로 하는 _캔버스_ 및 _하지_ 그라데이션을 사용 하 여 표시 된 그래픽 개체를 합니다. 그라데이션 선을 따라 색으로 점차 오른쪽 아래에 파란색으로 왼쪽 위에 빨간색에서 전환. 그라데이션 선을에 수직인 모든 줄에 불변 색을 있습니다.

배열을 `float` 색의 배열 사용 하 여 일대일 대응이 네 번째 인수로 지정 된 값입니다. 값은 해당 색 발생 그라데이션 선 따라 상대 위치를 나타냅니다. 여기서 0은 즉 `Red` 그라데이션 줄의 시작 시 발생 즉 1 및 `Blue` 줄의 끝에 발생 합니다. 숫자 오름차순 해야 및 0 ~ 1 범위에 있어야 합니다. 해당 범위의 그렇지 않은 경우 해당 범위에 포함 되도록 조정 됩니다.

배열의 두 값 0과 1 이외의 값으로 설정할 수 있습니다. 다음과 같이 해보세요.

```csharp
new float[] { 0.25f, 0.75f }
```

이제 그라데이션 줄의 첫 번째 전체 분기는 순수한 빨간색 이며 마지막 분기 순수 파란색입니다. 빨강 및 파랑 조합 그라데이션 선 중앙 절반으로 제한 됩니다.

일반적으로 이러한 위치 값이 동일 하 게 0에서에서 1 공간을 해야 합니다. 하는 경우 제공할 수 있습니다 단순히 `null` 네 번째 인수로 `CreateLinearGradient`합니다.

이 그라데이션 300 픽셀 사각형 영역의 두 모퉁이 사이 정의 된, 하지만 해당 사각형을 채우는 제한 하지 않습니다. 합니다 **모퉁이가-그라데이션** 탭에 응답 하는 몇 가지 추가 코드를 포함 하는 페이지 또는 페이지의 마우스 클릭 합니다. 합니다 `drawBackground` 간의 필드 설정/해제 `true` 및 `false` 각 탭을 사용 하 여 합니다. 값이 `true`, 해당 `PaintSurface` 처리기에서는 동일한 `SKPaint` 전체 캔버스를 채우도록 개체 및 다음 작은 사각형을 나타내는 검은 사각형을 그립니다. 

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

[![모퉁이가-그라데이션 전체](linear-gradient-images/CornerToCornerGradientFull.png "모퉁이가-그라데이션 전체")](linear-gradient-images/CornerToCornerGradientFull-Large.png#lightbox)

그라데이션의 그라데이션 선을 정의 하는 점 이외의 동일한 패턴에 자체 반복 있는지 확인 합니다. 이 반복이 발생 하기 때문에 마지막 인수 `CreateLinearGradient` 는 `SKShaderTileMode.Repeat`합니다. (있습니다 곧 확인 하겠지만 다른 옵션입니다.)

또한 그라데이션 줄을 지정 하는 데 사용 하는 지점 고유 하지는 알 수 있습니다. 수직 그라데이션 선으로 줄 무한 같은 효과를 지정할 수 있는 그라데이션 선의 되므로 동일한 색을 가집니다. 예를 들어 가로 그라데이션을 사용 하 여 사각형을 채우는, 왼쪽 및 오른쪽 위 모서리 또는 왼쪽 및 오른쪽 아래 모서리를 사용한 경우라도 및 병렬 해당 줄으로 된 두 점을 지정할 수 있습니다.

## <a name="interactively-experiment"></a>대화형으로 실험

선형 그라데이션을 사용 하 여 대화형으로 테스트할 수 있습니다 합니다 **대화형 선형 그라데이션** 페이지입니다. 이 페이지를 사용 합니다 `InteractivePage` 클래스의 기사에서 소개한 [ **호를 그리려면 세 가지 방법으로**](../../curves/arcs.md)합니다. `InteractivePage` 핸들 [ `TouchEffect` ](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md) 이벤트의 컬렉션을 유지 하기 위해 `TouchPoint` 손가락이 나 마우스를 사용 하 여 이동할 수 있는 개체입니다.

XAML 파일을 연결 합니다 `TouchEffect` 의 부모에는 `SKCanvasView` 도 포함을 `Picker` 의 세 멤버 중 하나를 선택할 수 있도록를 [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode) 열거형:

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

코드 숨김 파일에서 생성자 두 개 만듭니다 `TouchPoint` 선형 그라데이션의 시작점과 끝점에 대 한 개체입니다. 합니다 `PaintSurface` 처리기 (그라데이션 빨간색에서 녹색에서 파란색)에 대 한 세 가지 색의 배열을 정의 하 고 현재 가져옵니다 `SKShaderTileMode` 에서 `Picker`:

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

합니다 `PaintSurface` 처리기를 만듭니다를 `SKShader` 개체는 모든 정보에서 색 전체 캔버스를 사용 하 여 합니다. 배열을 `float` 값으로 설정 됩니다 `null`합니다. 이 고, 그렇지 동일 하 게 세 가지 색 공간을 해당 매개 변수는 배열의 값이 0, 0.5, 1을 설정 합니다.

대량의 `PaintSurface` 처리기 개체를 여러 개 표시에 할애 되어: 윤곽 원, 그라데이션 줄 및 줄 수직 터치 포인트의 그라데이션 선으로 터치 지점:

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

수직 줄 그라데이션의 시작과 끝과 일치합니다. 설정에 따라 선을 초과 어떤 일이 생기는 `SKShaderTileMode` 열거형:

[![대화형 선형 그라데이션](linear-gradient-images/InteractiveLinearGradient.png "대화형 선형 그라데이션")](linear-gradient-images/InteractiveLinearGradient-Large.png#lightbox)

세 가지 값의 결과 표시 하는 세 개의 스크린 샷을 [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode)합니다. IOS 스크린샷은 `SKShaderTileMode.Clamp`, 방금 그라데이션의 테두리의 색을 확장 합니다. `SKShaderTileMode.Repeat` Android 스크린샷에 옵션 그라데이션 패턴 반복 되는 방법을 보여 줍니다. `SKShaderTileMode.Mirror` UWP 스크린샷에 옵션 패턴을도 반복 되지만 패턴 반대로 매번 없는 색 불연속성 발생 합니다.

## <a name="gradients-on-gradients"></a>그라데이션에서 그라데이션

합니다 `SKShader` 없는 공용 속성 또는 메서드를 제외 하 고 클래스 정의 `Dispose`합니다. `SKShader` 개체의 정적 메서드를 통해 생성 되므로 변경할 수 없습니다. 두 개의 다른 개체에 대 한 동일한 그라데이션을 사용 하는 경우에 가능성이 그라데이션의 약간 변경 하는 것이 좋습니다. 이렇게 하려면 새 해야 `SKShader` 개체입니다.

합니다 **그라데이션 텍스트** 텍스트와 유사한 그라데이션으로 색은 백그라운드 페이지에 표시 됩니다.

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

합니다 `Shader` 의 속성을 `SKPaint` 개체 배경을 설명 하는 그라데이션을 표시할 가장 먼저 설정 됩니다. 그라데이션 지점 캔버스의 왼쪽 및 오른쪽 아래 모서리에 설정 됩니다.

코드 집합을 `TextSize` 의 속성을 `SKPaint` 캔버스 너비의 90% 텍스트가 표시 되도록 개체입니다. 텍스트 범위를 사용 하 여을 계산 `xText` 하 고 `yText` 에 전달할 값을 `DrawText` 텍스트를 가운데 메서드.

그러나 그라데이션의 두 번째 지점 `CreateLinearGradient` 호출이 표시 되 면 캔버스를 기준으로 텍스트의 왼쪽 및 오른쪽 아래 모퉁이를 참조 해야 합니다. 이동 하 여 이렇게 합니다 `textBounds` 동일한 사각형 `xText` 고 `yText` 값:

```csharp
textBounds.Offset(xText, yText);
```

사각형의 왼쪽 및 오른쪽 아래 모퉁이 그라데이션의 시작점과 끝점을 설정 하는 데 사용 될 수 있습니다.

## <a name="animating-a-gradient"></a>그라데이션에 애니메이션 적용

그라데이션에 애니메이션을 적용 하는 방법은 여러 가지가 있습니다. 한 가지 방법은 시작점과 끝점을 애니메이션 효과를 주는 하는 것입니다. 합니다 **그라데이션 애니메이션** 페이지 이동 두 점 중점을 두는 원 안에 캔버스에서. 이 원의 반지름을 절반 너비 또는 캔버스의 높이 더 작은 값입니다. 시작 점과 끝이이 원에 서로 반대 되는 및 그라데이션을 사용 하 여 검정색 흰색에서 이동 된 `Mirror` 타일 모드:

[![그라데이션 애니메이션](linear-gradient-images/GradientAnimation.png "그라데이션 애니메이션")](linear-gradient-images/GradientAnimation-Large.png#lightbox)

생성자는 만듭니다는 `SKCanvasView`합니다. 합니다 `OnAppearing` 고 `OnDisappearing` 애니메이션 논리를 처리 하는 메서드:

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

합니다 `OnTimerTick` 메서드를 계산는 `angle` 애니메이션 효과가 적용 됩니다 0에서 2 π 매 3 초입니다. 

두 개의 그라데이션 점을 계산 하는 한 가지 방법은 다음과 같습니다. `SKPoint` 라는 값 `vector` 원의 반지름의 지점에 캔버스의 가운데에서 확장으로 계산 됩니다. 이 벡터의 방향 각도의 사인 및 코사인 값을 기반으로 합니다. 두 개의 반대 그라데이션 지점 계산 됩니다. 1 포인트는 center 지점에서 해당 벡터를 빼서 계산 하 고 다른 중심점에 벡터를 추가 하 여 계산 됩니다.

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

다소 다른 접근 방법이 더 적은 코드에 필요합니다. 이 접근 방식을 이용 합니다 [ `SKShader.CreateLinearGradient` ](xref:SkiaSharp.SKShader.CreateLinearGradient(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) 마지막 인수로 매트릭스 변환 사용 하 여 메서드를 오버 로드 합니다. 이 방법은 버전을 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 샘플:

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

캔버스의 너비가 높이 보다 작은 경우 두 개의 그라데이션 지점으로 설정 됩니다 (0, 0) 및 (`info.Width`, 0). 마지막 인수로 전달 된 회전 변환을 `CreateLinearGradient` 화면의 가운데를 기준으로 두 지점을 효과적으로 회전 합니다.

각도 0, 회전 안 되며 두 그라데이션 점 캔버스의 왼쪽 위 모퉁이 오른쪽 위 모서리는 경우는 note 합니다. 해당 지점에 없는 이전에 표시 된 대로 계산 그라데이션 선만 `CreateLinearGradient` 호출 합니다. 아니지만 이러한 지점이 _병렬_ 그라데이션 가로줄을 캔버스의 가운데 bisects는 및를 동일한 그라데이션 발생 합니다.

**Rainbow 그라데이션**

합니다 **Rainbow 그라데이션** 페이지 캔버스의 왼쪽 위 모퉁이에서 오른쪽 아래 모퉁이에 rainbow을 그립니다. 하지만이 레인 보우 그라데이션 실제 rainbow 같은 하지 않습니다. 곡선을 보다는 직선 있지만 기반으로 360 색상 값 0에서 순환 의해 결정 되는 8 개의 HSL (색상-채도-명도) 색:

```csharp
SKColor[] colors = new SKColor[8];

for (int i = 0; i < colors.Length; i++)
{
    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
}
```

코드의 일부임을 `PaintSurface` 아래에 표시 된 처리기입니다. 처리기는 오른쪽 아래 모서리를 캔버스의 왼쪽 위 모퉁이에서 확장 하는 6 면 다각형을 정의 하는 경로 만들어 시작 합니다.

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

두 그라데이션 점은 `CreateLinearGradient` 메서드 두이 경로 정의 하는 지점을 기반으로 합니다. 두 지점 왼쪽 위 모퉁이 가까워질 합니다. 첫 번째는 캔버스의 위쪽 가장자리에서 이며 두 번째 캔버스의 왼쪽된 가장자리입니다. 결과 다음과 같습니다.

[![잘못 된 rainbow 그라데이션](linear-gradient-images/RainbowGradientFaulty.png "결함이 있는 레인 보우 그라데이션")](linear-gradient-images/RainbowGradientFaulty-Large.png#lightbox)

이 흥미로운 이미지로 이지만 의도 상당히 아닙니다. 문제는 선형 그라데이션을 만들 때 고정 색상의 줄을 수직 그라데이션 선으로. 그라데이션 선은 그림 위쪽 및 왼쪽 면에 닿으면 해당 줄이 그림의 오른쪽 아래 모서리를 확장 하는 가장자리에 수직인 일반적으로 요소를 기반으로 합니다. 이 방법은 캔버스 제곱 된 경우에 작동 합니다.

적절 한 레인 보우 그라데이션을 만들려면 그라데이션 선을 무지개의 가장자리에 수직 이어야 합니다. 더 복잡된 한 계산입니다. 벡터 그림의 옆에 병렬 되는 정의 되어야 합니다. 벡터는 쪽에 수직인 있도록 90도 회전된 합니다. 다음 그림의 너비와 곱하여 수를 늘리는 `rainbowWidth`합니다. 벡터와 해당 시점 및 그림에서 어느 지점에서 두 그라데이션 점 기반 계산 됩니다. 여기에 표시 되는 코드를 **Rainbow 그라데이션** 페이지에 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 샘플:

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

[![Rainbow 그라데이션](linear-gradient-images/RainbowGradient.png "Rainbow 그라데이션")](linear-gradient-images/RainbowGradient-Large.png#lightbox)

**무한대 색**

Rainbow 그라데이션에도 사용 합니다 **무한대 색** 페이지입니다. 이 페이지는 문서에 설명 된 경로 개체를 사용 하는 무한대 기호를 그립니다 [ **베 지 어 곡선의 세 가지 형식**](../../curves/beziers.md#bezier-curve-approximation-to-circular-arcs)합니다. 다음 이미지는 이미지에서 지속적으로 추가 하는 애니메이션된 rainbow 그라데이션 색이 지정 됩니다.

생성자는 만듭니다는 `SKPath` 무한대 기호를 설명 하는 개체입니다. 경로 만든 후 생성자 경로의 사각형 범위를 가져올 수도 있습니다. 그런 다음 이라고 하는 값을 계산 `gradientCycleLength`합니다. 그라데이션이 왼쪽 및 오른쪽 아래 모퉁이 기반으로 하는 경우는 `pathBounds` 사각형이 `gradientCycleLength` 값이 그라데이션 패턴의 전체 가로 너비:

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

생성자도 만듭니다는 `colors` 무지개, 배열 및 `SKCanvasView` 개체입니다.

재정의 된 `OnAppearing` 및 `OnDisappearing` 메서드 애니메이션에 대 한 오버 헤드를 수행 합니다. 합니다 `OnTimerTick` 메서드 애니메이션을 적용 합니다 `offset` 0에서 필드 `gradientCycleLength` 2 초 마다:

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

마지막으로 `PaintSurface` 처리기의 무한대 기호를 렌더링 합니다. 경로의 중심점을 둘러싼 음수 및 양수 좌표를 포함 하기 때문에 (0, 0)을 `Translate` 캔버스에서 변환 센터로 이동 됩니다. 좌표 이동 변환 뒤를 `Scale` 무한대 기호를 캔버스의 높이 및 너비의 95% 내에서 계속 하면서 최대한 늘릴 수 있습니다 하는 배율 인수를 적용 하는 변환입니다. 

다음에 유의 합니다 `STROKE_WIDTH` 상수 경로 경계 사각형의 높이 너비에 추가 됩니다. 네 면 모두 해당 너비의 절반으로 렌더링 된 무한대 크기의 크기가 증가 하므로이 너비의 줄을 사용 하 여 경로 스트로크는:

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

처음 두 인수로 전달 된 점을 살펴볼 `SKShader.CreateLinearGradient`합니다. 해당 지점에 경계 사각형의 원래 경로 기반으로 합니다. 첫 번째 점은 (&ndash;250, &ndash;100)이 고 두 번째 (250, 100). SkiaSharp 내부 해당 지점에 종속 됩니다 현재 캔버스 변환을 표시 무한대 기호를 사용 하 여 올바르게 정렬 되도록 합니다.

마지막 인수 없이 `CreateLinearGradient`, 왼쪽 위 무한대 기호 아래로 순서 대로 오른쪽에서 확장 된 rainbow 그라데이션이 표시 됩니다. (실제로 그라데이션의 확장 왼쪽 위 모서리에서 경계 사각형의 오른쪽 아래 모퉁이에 있습니다. 렌더링 된 무한대 기호를 절반으로 경계 사각형을 보다는 `STROKE_WIDTH` 모든 양쪽 값입니다. 그라데이션의 시작과 끝에 빨간색 그라데이션을 사용 하 여 만들어집니다. 때문에 `SKShaderTileMode.Repeat`, 차이 눈에 띄는 되지 않습니다.)

해당 마지막 인수를 사용 하 여 `CreateLinearGradient`, 이미지에서 그라데이션 패턴을 지속적으로 추가 합니다.

[![무한대 색상](linear-gradient-images/InfinityColors.png "무한대 색")](linear-gradient-images/InfinityColors-Large.png#lightbox)

## <a name="transparency-and-gradients"></a>투명도 및 그라데이션과

그라데이션에 영향을 주는 색 투명도 통합할 수 있습니다. 다른 색에서 간의 페이드 그라데이션을 대신 그라데이션의 투명 색에서 페이드 수 있습니다. 

몇 가지 흥미로운 효과 대 한이 기법을 사용할 수 있습니다. 클래식 예제 중 하나는 리플렉션 사용 하 여 그래픽 개체를 보여 줍니다.

[![리플렉션 그라데이션](linear-gradient-images/ReflectionGradient.png "리플렉션 그라데이션")](linear-gradient-images/ReflectionGradient-Large.png#lightbox)

거꾸로 된 텍스트를 50% 맨 아래에 완전히 투명 하 게 맨 위에 있는 투명 한 그라데이션 색입니다. 이러한 수준의 투명도 0x80의 알파 값 및 0와 연결 됩니다.

합니다 `PaintSurface` 처리기에는 **리플렉션 그라데이션** 페이지 캔버스 너비의 90% 텍스트의 크기를 조정 합니다. 그런 다음 계산 `xText` 고 `yText` 가로 가운데 맞춤 될 텍스트를 배치 하는 값 이지만 페이지의 세로 가운데에 해당 하는 기준에서:

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

이러한 `xText` 및 `yText` 값은 동일한 값에 반영 된 텍스트를 표시 하는 데 사용 합니다 `DrawText` 맨 아래에 호출를 `PaintSurface` 처리기. 그러나 해당 코드를 직전 표시에 대 한 호출을 `Scale` 메서드의 `SKCanvas`합니다. 이 `Scale` 메서드는 수평적으로 확장 (아무 작업도 수행 하지)는 1에서 세로로 하지만 여 &ndash;1 효과적으로 모든 거꾸로 대칭이 되도록 합니다. 회전 중심의 지점으로 설정 됩니다 (0 `yText`), 여기서 `yText` 는 원래 계산 캔버스의 세로 가운데 `info.Height` 2로 나눈 결과입니다.

Skia 그라데이션을 사용 하 여 캔버스 변환 하기 전에 그래픽 개체에 색을 염두에 두어야 합니다. Unreflected 텍스트를 가져온 후의 `textBounds` 사각형에 표시 된 텍스트에 해당 하므로 전부가 이동 됩니다.

```csharp
textBounds.Offset(xText, yText);
```

`CreateLinearGradient` 호출에는 사각형의 위쪽에서 아래쪽으로 향하는 그라데이션을 정의 합니다. 기울기가 완전히 투명 한 파랑 (`paint.Color.WithAlpha(0)`)을 50% 투명 한 파랑 (`paint.Color.WithAlpha(0x80)`). 캔버스 변환을 50% 투명 한 파랑 기준선에서 시작 하 고 텍스트의 맨 위에 있는 투명 하 게 되므로 거꾸로 텍스트를 대칭 이동 합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
