---
title: 선 및 스트로크 단면
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 다른 스트로크 단면 있는 선을 그리려면 SkiaSharp 사용 방법에 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 9aaecb8c63ff28111097dce81954f523b4c7731b
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725214"
---
# <a name="lines-and-stroke-caps"></a>선 및 스트로크 단면

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp를 사용 하 여 다른 스트로크 캡이 있는 선을 그리는 방법에 대해 알아봅니다._

SkiaSharp, 전혀 렌더링에서는 일련의 연결 된 직선 렌더링에서 다릅니다. 단일 줄을 그릴 때에 있지만 경우가 줄 특정 스트로크 너비를 지정 하는 데 필요한 합니다. 이러한 줄 광범위 한 있을 경우 줄의 끝 모양의이 중요 합니다. 선의 끝 모양을 *스트로크 단면*이라고 합니다.

![](lines-images/strokecapsexample.png "The three stroke caps options")

단일 줄을 그리기 위해 `SKCanvas`는 인수가 `SKPaint` 개체를 사용 하 여 선의 시작 좌표와 끝 좌표를 나타내는 간단한 [`DrawLine`](xref:SkiaSharp.SKCanvas.DrawLine(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) 메서드를 정의 합니다.

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

기본적으로 새로 인스턴스화된 `SKPaint` 개체의 [`StrokeWidth`](xref:SkiaSharp.SKPaint.StrokeWidth) 속성은 0 이며, 1 픽셀의 선 렌더링 시 1의 값과 동일한 효과가 있습니다. 휴대폰과 같은 고해상도 장치에서 매우 씬로 나타나므로 `StrokeWidth`를 큰 값으로 설정 하는 것이 좋습니다. 또 다른 문제를 발생 시키는 많은 두께의 줄 그리기를 시작 하면 되지만: 시작 되 고 이러한 두꺼운 선 끝 렌더링 되는 방식을?

줄의 시작 및 끝 모양을 *줄 캡* 이라고 하며, 선 끝 *에 선 끝*을 표시 합니다. 이 컨텍스트의 "캡" 단어는 줄의 끝에 있는 항목을 &mdash; 하는 종류의 hat를 나타냅니다. `SKPaint` 개체의 [`StrokeCap`](xref:SkiaSharp.SKPaint.StrokeCap) 속성을 [`SKStrokeCap`](xref:SkiaSharp.SKStrokeCap) 열거의 다음 멤버 중 하나로 설정 합니다.

- `Butt` (기본값)
- `Square`
- `Round`

이러한 샘플 프로그램을 통해 잘 설명 됩니다. [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 프로그램의 **SkiaSharp Lines 및 Paths** 섹션은 [`StrokeCapsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeCapsPage.cs) 클래스에 따라 **스트로크 캡** 이라는 페이지로 시작 합니다. 이 페이지는 `SKStrokeCap` 열거형의 세 멤버를 반복 하 여 열거형 멤버의 이름을 표시 하 고 해당 스트로크 캡을 사용 하 여 선을 그리는 `PaintSurface` 이벤트 처리기를 정의 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Center
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width / 2;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = textPaint.FontSpacing;

    foreach (SKStrokeCap strokeCap in Enum.GetValues(typeof(SKStrokeCap)))
    {
        // Display text
        canvas.DrawText(strokeCap.ToString(), xText, y, textPaint);
        y += textPaint.FontSpacing;

        // Display thick line
        thickLinePaint.StrokeCap = strokeCap;
        canvas.DrawLine(xLine1, y, xLine2, y, thickLinePaint);

        // Display thin line
        canvas.DrawLine(xLine1, y, xLine2, y, thinLinePaint);
        y += 2 * textPaint.FontSpacing;
    }
}
```

`SKStrokeCap` 열거형의 각 멤버에 대해 처리기는 스트로크 두께가 50 픽셀인 두 줄을 그립니다. 한 줄에는 두 픽셀의 스트로크 두께가 있습니다. 이 두 번째 줄은 기하학적 시작과 선 두께 및 스트로크 단면 독립적인 줄의 끝을 설명 하기 위한 것:

[![](lines-images/strokecaps-small.png "Triple screenshot of the Stroke Caps page")](lines-images/strokecaps-large.png#lightbox "Triple screenshot of the Stroke Caps page")

여기에서 볼 수 있듯이 `Square` 및 `Round` 스트로크 캡은 줄의 시작 부분에 있는 스트로크 너비의 절반을 기준으로 줄의 길이를 효과적으로 확장 하 고 끝에서 다시 줄의 길이를 확장 합니다. 이 확장이 렌더링 되는 그래픽 개체의 크기를 결정 하는 데 필요한 경우에 중요 합니다.

`SKCanvas` 클래스에는 약간 특이 한 여러 줄을 그리는 다른 메서드도 포함 되어 있습니다.

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points` 매개 변수는 `SKPoint` 값의 배열이 며 `mode`는 세 개의 멤버를 포함 하는 [`SKPointMode`](xref:SkiaSharp.SKPointMode) 열거형의 멤버입니다.

- 개별 요소를 렌더링 하는 `Points`
- 각 점의 쌍을 연결 하는 `Lines`
- 연속 되는 모든 요소를 연결 `Polygon`

**여러 줄** 페이지에서이 메서드를 보여 줍니다. [**MultipleLinesPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/MultipleLinesPage.xaml) 파일은 `SKPointMode` 열거형의 멤버와 `SKStrokeCap` 열거형의 멤버를 선택할 수 있는 두 개의 `Picker` 뷰를 인스턴스화합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.MultipleLinesPage"
             Title="Multiple Lines">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="pointModePicker"
                Title="Point Mode"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKPointMode}">
                    <x:Static Member="skia:SKPointMode.Points" />
                    <x:Static Member="skia:SKPointMode.Lines" />
                    <x:Static Member="skia:SKPointMode.Polygon" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKStrokeCap}">
                    <x:Static Member="skia:SKStrokeCap.Butt" />
                    <x:Static Member="skia:SKStrokeCap.Round" />
                    <x:Static Member="skia:SKStrokeCap.Square" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                PaintSurface="OnCanvasViewPaintSurface"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

`SkiaSharp` 네임 스페이스는 `SKPointMode` 및 `SKStrokeCap` 열거형의 멤버를 참조 하는 데 필요 하기 때문에 SkiaSharp 네임 스페이스 선언은 약간 다릅니다. `Picker` 뷰의 `SelectedIndexChanged` 처리기는 단순히 `SKCanvasView` 개체를 무효화 합니다.

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

이 처리기는 XAML 파일에서 `Picker`의 `SelectedIndex` 속성이 0으로 설정 되 고 `SKCanvasView` 인스턴스화되기 전에 발생 하는 경우 이벤트 처리기가 먼저 호출 되기 때문에 `SKCanvasView` 개체가 있는지 확인 해야 합니다.

`PaintSurface` 처리기는 `Picker` 뷰에서 두 개의 열거형 값을 가져옵니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create an array of points scattered through the page
    SKPoint[] points = new SKPoint[10];

    for (int i = 0; i < 2; i++)
    {
        float x = (0.1f + 0.8f * i) * info.Width;

        for (int j = 0; j < 5; j++)
        {
            float y = (0.1f + 0.2f * j) * info.Height;
            points[2 * j + i] = new SKPoint(x, y);
        }
    }

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.DarkOrchid,
        StrokeWidth = 50,
        StrokeCap = (SKStrokeCap)strokeCapPicker.SelectedItem
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = (SKPointMode)pointModePicker.SelectedItem;
    canvas.DrawPoints(pointMode, points, paint);
}
```

스크린샷에는 다양 한 `Picker` 선택이 나와 있습니다.

[![](lines-images/multiplelines-small.png "Triple screenshot of the Multiple Lines page")](lines-images/multiplelines-large.png#lightbox "Triple screenshot of the Multiple Lines page")

왼쪽의 iPhone은 `SKPointMode.Points` 열거형 멤버가 `DrawPoints` 하 여 `SKPoint` 배열의 각 요소를 선 끝이 `Butt` 또는 `Square`경우 사각형으로 렌더링 하는 방법을 보여 줍니다. 줄 끝이 `Round`되 면 원이 렌더링 됩니다.

Android 스크린샷은 `SKPointMode.Lines`의 결과를 보여 줍니다. `DrawPoints` 메서드는 지정 된 줄 끝 (이 경우 `Round`)을 사용 하 여 `SKPoint` 값의 각 쌍 사이에 선을 그립니다.

대신 `SKPointMode.Polygon`를 사용 하는 경우 배열에서 연속 되는 요소 사이에 선이 그려지고 매우 긴밀 하 게 표시 되는 경우이 줄이 연결 되지 않은 것을 볼 수 있습니다. 각 이러한 별도 줄 시작 되며 지정된 선 끝 모양을로 끝납니다. `Round` 대문자를 선택 하면 선이 연결 된 것으로 표시 될 수 있지만 연결 되지 않습니다.

줄 연결 되거나 연결 되지 있는지 여부를 그래픽 경로 사용 하는 중요 한 측면 이며

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
