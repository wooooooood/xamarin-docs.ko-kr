---
title: ''
description: 이 문서에서는 SkiaSharp를 사용 하 여 응용 프로그램에서 다른 스트로크 캡이 있는 선을 그리는 방법을 설명 하 Xamarin.Forms 고 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 87b97ad913e08c42d16bbf055f168c07b9bd60e8
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137210"
---
# <a name="lines-and-stroke-caps"></a>선 및 스트로크 단면

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp를 사용 하 여 다른 스트로크 캡이 있는 선을 그리는 방법에 대해 알아봅니다._

SkiaSharp에서 한 줄을 렌더링 하는 것은 일련의 연결 된 직선을 렌더링 하는 것과 매우 다릅니다. 그러나 단일 줄을 그리는 경우에도 줄에 특정 스트로크 너비를 지정 해야 하는 경우가 종종 있습니다. 이러한 줄이 더 넓게 표시 되 면 줄의 끝 모양이 중요할 수도 있습니다. 선의 끝 모양을 *스트로크 단면*이라고 합니다.

![](lines-images/strokecapsexample.png "The three stroke caps options")

단일 줄을 그리기 위해은 `SKCanvas` [`DrawLine`](xref:SkiaSharp.SKCanvas.DrawLine(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) 개체가 있는 줄의 시작 및 끝 좌표를 나타내는 인수가 포함 된 간단한 메서드를 정의 합니다 `SKPaint` .

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

기본적으로 [`StrokeWidth`](xref:SkiaSharp.SKPaint.StrokeWidth) 새로 인스턴스화된 개체의 속성은 `SKPaint` 0 이며, 1 픽셀의 선 렌더링 시 1의 값과 동일한 효과가 있습니다. 휴대폰 등의 고해상도 장치에서 매우 씬로 나타나므로를 큰 값으로 설정 하는 것이 좋습니다 `StrokeWidth` . 그러나 조정 가능한 두께의 선 그리기를 시작 하면 다른 문제가 발생 합니다. 이러한 굵은 선의 시작과 끝을 렌더링 하는 방법은 무엇입니까?

줄의 시작 및 끝 모양을 *줄 캡* 이라고 하며, 선 끝 *에 선 끝*을 표시 합니다. 이 컨텍스트에서 "캡" 이라는 단어는 &mdash; 줄의 끝에 있는 종류의 hat 항목을 나타냅니다. [`StrokeCap`](xref:SkiaSharp.SKPaint.StrokeCap)개체의 속성을 `SKPaint` 열거형의 다음 멤버 중 하나로 설정 합니다 [`SKStrokeCap`](xref:SkiaSharp.SKStrokeCap) .

- `Butt`(기본값)
- `Square`
- `Round`

이는 샘플 프로그램에서 가장 잘 보여 줍니다. [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 프로그램의 **SkiaSharp Lines 및 Paths** 섹션은 클래스에 따라 **스트로크 캡** 이라는 페이지로 시작 합니다 [`StrokeCapsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeCapsPage.cs) . 이 페이지에서는 열거형 `PaintSurface` 의 세 가지 멤버를 반복 하 여 `SKStrokeCap` 열거형 멤버의 이름을 표시 하 고 해당 스트로크 캡을 사용 하 여 선을 그리는 이벤트 처리기를 정의 합니다.

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

열거형의 각 멤버에 대해 `SKStrokeCap` 처리기는 스트로크 두께가 50 픽셀이 고 다른 줄이 2 픽셀의 스트로크 두께로 배치 된 두 줄을 그립니다. 이 두 번째 선은 선 두께와 스트로크 캡에 관계 없이 선의 기하학적 시작과 끝을 보여 주기 위한 것입니다.

[![](lines-images/strokecaps-small.png "Triple screenshot of the Stroke Caps page")](lines-images/strokecaps-large.png#lightbox "Triple screenshot of the Stroke Caps page")

여기에서 볼 수 있듯이 및 stroke 캡은 줄의 시작 부분에 `Square` `Round` 있는 스트로크 너비의 절반을 기준으로 줄의 길이를 효과적으로 확장 하 고 끝에 다시 선 길이를 확장 합니다. 이 확장은 렌더링 된 그래픽 개체의 크기를 확인 해야 하는 경우에 중요 합니다.

클래스에는 `SKCanvas` 특이 한 하는 여러 줄을 그리는 다른 메서드도 포함 되어 있습니다.

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points`매개 변수는 값의 배열이 며 `SKPoint` , 다음 `mode` [`SKPointMode`](xref:SkiaSharp.SKPointMode) 세 개의 멤버를 포함 하는 열거형의 멤버입니다.

- `Points`개별 요소를 렌더링 하려면
- `Lines`각 점의 쌍을 연결 하려면
- `Polygon`연속 되는 모든 요소를 연결 하려면

**여러 줄** 페이지에서이 메서드를 보여 줍니다. [**MultipleLinesPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/MultipleLinesPage.xaml) 파일은 열거형 `Picker` 의 멤버 `SKPointMode` 와 열거형의 멤버를 선택할 수 있는 두 개의 뷰를 인스턴스화합니다 `SKStrokeCap` .

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

네임 스페이스는 `SkiaSharp` 및 열거형의 멤버를 참조 하는 데 필요 하기 때문에 SkiaSharp 네임 스페이스 선언은 약간 `SKPointMode` 다릅니다 `SKStrokeCap` . `SelectedIndexChanged`두 뷰에 대 한 처리기는 `Picker` 단순히 개체를 무효화 합니다 `SKCanvasView` .

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

이 처리기는의 `SKCanvasView` `SelectedIndex` 속성이 `Picker` XAML 파일에서 0으로 설정 되 고가 인스턴스화되기 전에 발생 하는 경우 이벤트 처리기가 먼저 호출 되기 때문에 개체의 존재 여부를 확인 해야 합니다 `SKCanvasView` .

`PaintSurface`처리기는 뷰에서 두 개의 열거형 값을 가져옵니다 `Picker` .

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

스크린샷에는 다양 한 `Picker` 선택 항목이 표시 됩니다.

[![](lines-images/multiplelines-small.png "Triple screenshot of the Multiple Lines page")](lines-images/multiplelines-large.png#lightbox "Triple screenshot of the Multiple Lines page")

왼쪽에 있는 iPhone은 `SKPointMode.Points` `DrawPoints` `SKPoint` 줄 끝이 또는 인 경우 열거형 멤버가 배열의 각 요소를 사각형으로 렌더링 하는 방법을 보여 줍니다 `Butt` `Square` . 줄 끝이 이면 원이 렌더링 됩니다 `Round` .

Android 스크린샷에는의 결과가 표시 `SKPointMode.Lines` 됩니다. `DrawPoints`메서드는 `SKPoint` 지정 된 줄 끝 (이 경우)을 사용 하 여 각 값 쌍 사이에 선을 그립니다 `Round` .

를 사용 하는 경우 `SKPointMode.Polygon` 배열에서 연속 되는 요소 사이에 선이 그려지고 매우 긴밀 하 게 표시 되는 경우에는 이러한 줄이 연결 되지 않습니다. 이러한 개별 줄은 각각 지정 된 선 끝 모양으로 시작 하 고 끝납니다. 대문자를 선택 하면 `Round` 선이 연결 된 것으로 표시 될 수 있지만 연결 되지 않습니다.

선이 연결 되어 있거나 연결 되지 않은 경우에는 그래픽 경로를 사용 하는 것이 중요 한 부분입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
