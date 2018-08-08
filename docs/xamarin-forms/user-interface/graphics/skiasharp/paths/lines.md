---
title: 선 및 스트로크 단면
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 다른 스트로크 단면 있는 선을 그리려면 SkiaSharp 사용 방법에 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 000bf24c1b06baab892f0b165c8b9eeebebce49d
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615264"
---
# <a name="lines-and-stroke-caps"></a>선 및 스트로크 단면

_다른 스트로크 단면 있는 선을 그리려면 SkiaSharp 사용 방법 알아보기_

SkiaSharp, 전혀 렌더링에서는 일련의 연결 된 직선 렌더링에서 다릅니다. 도 한 줄을 그릴 때 특정 스트로크 너비 및 줄 고 더 광범위 한 줄을 제공 하는 데 필요한 경우가 그러나 더 중요 한가 호출 된 줄의 끝 모양의 합니다 *스트로크 단면*:

![](lines-images/strokecapsexample.png "세 선 caps 옵션")

단일 선 그리기 `SKCanvas` 간단한 정의 [ `DrawLine` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawLine/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) 인수 시작 및 끝 좌표로 사용 하 여 선의 표시 하는 메서드는 `SKPaint` 개체:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

기본적으로 `StrokeWidth` 속성을 새로 인스턴스화된 `SKPaint` 개체가 0으로, 1 픽셀의 선 두께 렌더링에 1의 값으로 동일한 효과가 있습니다. 이 나타나므로 매우 얇은 휴대폰와 같은 고해상도 장치에서 설정 하 고 싶을 `StrokeWidth` 큰 값으로. 또 다른 문제를 발생 시키는 많은 두께의 줄 그리기를 시작 하면 되지만: 시작 되 고 이러한 두꺼운 선 끝 렌더링 되는 방식을?

작업이 시작 되 고 줄 끝 모양을 라고는 *선 끝 모양을* 또는 Skia는 *스트로크 단면*합니다. "이 컨텍스트에서" cap 라는 단어는 hat의 종류를 의미 &mdash; 줄의 끝에 위치 하는 것입니다. 설정한 합니다 [ `StrokeCap` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeCap/) 의 속성을 `SKPaint` 개체의 다음 멤버 중 하나를 [ `SKStrokeCap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeCap/) 열거형:

- [`Butt`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Butt/) (기본값)
- [`Square`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)

이러한 샘플 프로그램을 통해 잘 설명 됩니다. 홈 페이지의 두 번째 섹션을 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 라는 제목의 페이지를 사용 하 여 프로그램 시작 **스트로크 단면** 기반으로 합니다 [ `StrokeCapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeCapsPage.cs) 클래스입니다. 이 페이지에서는 정의 `PaintSurface` 의 세 멤버를 반복 하는 이벤트 처리기는 `SKStrokeCap` 열거형 멤버의 이름을 둘 다를 표시 하 고 해당 스트로크 단면을 사용 하는 선을 그리기 열거형:

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

각 멤버에 대해는 `SKStrokeCap` 열거형 처리기 50 픽셀 및 2 픽셀로 스트로크 두께 사용 하 여 맨 위에 배치 하는 다른 줄 스트로크 두께 사용 하 여 하나를 두 줄을 그립니다. 이 두 번째 줄은 기하학적 시작과 선 두께 및 스트로크 단면 독립적인 줄의 끝을 설명 하기 위한 것:

[![](lines-images/strokecaps-small.png "삼중 스트로크 단면 페이지 스크린샷")](lines-images/strokecaps-large.png#lightbox "삼중 스트로크 단면 페이지 스크린샷")

알 수 있듯이 합니다 `Square` 고 `Round` 스트로크 단면 줄의 시작 부분 및 끝 다시 절반 스트로크 너비에 의해 줄의 길이 효과적으로 확장 합니다. 이 확장이 렌더링 되는 그래픽 개체의 크기를 결정 하는 데 필요한 경우에 중요 합니다.

`SKCanvas` 클래스에는 다소 특이 한 여러 줄을 그리기 위한 다른 방법을 포함 되어 있습니다.

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points` 매개 변수는 배열이 `SKPoint` 값 및 `mode` 멤버인 합니다 [ `SKPointMode` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPointMode/) 세 명의 멤버가 있는 열거형:

- [`Points`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Points/) 개별 요소를 렌더링 하려면
- [`Lines`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Lines/) 각 쌍의 위치를 연결 하려면
- [`Polygon`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Polygon/) 모든 연속 요소를 연결 하려면

합니다 **여러 줄** 페이지에는이 방법을 보여 줍니다. [ `MultipleLinesPage` XAML 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/MultipleLinesPage.xaml) 두 개를 인스턴스화하고 `Picker` 수 있도록 하는 보기의 멤버를 선택 합니다 `SKPointMode` 열거형 및 멤버인은 `SKStrokeCap` 열거형:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.MultipleLinesPage"
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
            <Picker.Items>
                <x:String>Points</x:String>
                <x:String>Lines</x:String>
                <x:String>Polygon</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Butt</x:String>
                <x:String>Round</x:String>
                <x:String>Square</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

합니다 `SelectedIndexChanged` 둘 다에 대 한 처리기 `Picker` 뷰는 단순히 무효화는 `SKCanvasView` 개체:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

이 처리기가 있는지 확인 해야 합니다 `SKCanvasView` 개체 때문에 첫 번째 이벤트 처리기가 호출 될 때를 `SelectedIndex` 의 속성을 `Picker` XAML 파일에서 0으로 설정 됩니다 하기 전에 발생 하는 `SKCanvasView` 인스턴스화된.

`PaintSurface` 에서 선택된 된 두 항목을 가져오기 위한 제네릭 메서드를 액세스 하는 처리기는 `Picker` 뷰와 열거형 값을 변환 하 합니다.

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
        StrokeCap = GetPickerItem<SKStrokeCap>(strokeCapPicker)
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = GetPickerItem<SKPointMode>(pointModePicker);
    canvas.DrawPoints(pointMode, points, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }
    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}
```

스크린샷은 다양 한 `Picker` 세 가지 플랫폼에서 선택 합니다.

[![](lines-images/multiplelines-small.png "여러 줄 페이지의 3 배가 스크린샷")](lines-images/multiplelines-large.png#lightbox "삼중 여러 줄 페이지 스크린샷")

왼쪽된 박람회에서 iPhone 하는 방법을 `SKPointMode.Points` 열거형 멤버 발생 `DrawPoints` 각 항목에 렌더링 하는 `SKPoint` 선 끝 모양을 경우 사각형으로 배열 `Butt` 또는 `Square`합니다. 선 끝 모양을 경우 원 렌더링 됩니다 `Round`합니다.

대신 사용 하는 경우 `SKPointMode.Lines`센터에서 Android 화면에 표시 된 것 처럼 합니다 `DrawPoints` 메서드 각 쌍 사이 선을 그립니다 `SKPoint` 값,이 경우 지정된 선 끝 모양를 사용 하 여 `Round`입니다.

결과 표시 하는 UWP 스크린샷에서 `SKPointMode.Polygon` 값입니다. 배열에서 연속 점 사이의 줄이 그려집니다 있지만 매우 자세히 살펴보면 이러한 줄 연결 되어 있지 않은 확인할 수 있습니다. 각 이러한 별도 줄 시작 되며 지정된 선 끝 모양을로 끝납니다. 선택 하는 경우는 `Round` 캡이 포함 된 줄 연결 되어 나타날 수 있지만 실제로 연결 되지 않은 합니다.

줄 연결 되거나 연결 되지 있는지 여부를 그래픽 경로 사용 하는 중요 한 측면 이며


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
