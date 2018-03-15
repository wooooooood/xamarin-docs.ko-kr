---
title: "선 및 스트로크 단면"
description: "SkiaSharp 다른 스트로크 캡이 포함 된 선을 그리는 데 사용 하는 방법에 알아봅니다"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 341d850709ff27f4dc397cee3bb2fc5f73c0ec3c
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="lines-and-stroke-caps"></a>선 및 스트로크 단면

_SkiaSharp 다른 스트로크 캡이 포함 된 선을 그리는 데 사용 하는 방법에 알아봅니다_

SkiaSharp, 한 줄을 렌더링은 렌더링 하는 일련의 연결 된 직선 매우 다릅니다. 그러나도 한 줄을 그릴 때 특정 스트로크 너비 및 넓어집니다 줄 줄을 제공 하는 경우가 많습니다, 더 중요 한 호출 선의 끝 모양이 되는 *획 cap*:

![](lines-images/strokecapsexample.png "세 가지 획 caps 옵션")

단일 선 그리기에 대 한 `SKCanvas` 간단한 정의 [ `DrawLine` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawLine/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) 방식은 인수 시작 및 끝 줄의 좌표를 나타내는 메서드는 `SKPaint` 개체:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

기본적으로는 `StrokeWidth` 새로 인스턴스화된의 속성 `SKPaint` 개체는 0 값이 1 픽셀의 선 두께의 렌더링에 1과 동일한 결과가입니다. 이 나타나므로 정도로 작은 전화, 예: 고해상도 장치에서 설정 좋을 것은 `StrokeWidth` 큰 값으로. 다른 문제가 발생 하는 큰 두께의 선 그리기를 시작 하면 하지만: 시작 되 고 이러한 두꺼운 줄 끝 렌더링 되는 방식을?

작업이 시작 되 고 줄 끝 모양을 라고는 *선 단면* 또는 Skia는 *획 cap*합니다. 이 컨텍스트에서 "cap" 라는 단어 hat의 종류를 의미 &mdash; 줄의 끝에 있는 것입니다. 설정한는 [ `StrokeCap` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeCap/) 의 속성은 `SKPaint` 개체의 다음 멤버 중 하나에 [ `SKStrokeCap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeCap/) 열거형:

- [`Butt`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Butt/) (기본값)
- [`Square`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)

이러한 샘플 프로그램에 가장 잘 설명 되어 있습니다. 홈 페이지의 두 번째 섹션은 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) 라는 페이지와 프로그램을 시작 **스트로크 단면** 기반는 [ `StrokeCapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/StrokeCapsPage.cs) 클래스입니다. 이 페이지에서는 정의 `PaintSurface` 의 세 멤버를 반복 하는 이벤트 처리기는 `SKStrokeCap` 열거형에서 열거형 멤버의 이름을 표시 하 고 해당 선 단면을 사용 하 여 선 그리기:

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

각 멤버에 대 한는 `SKStrokeCap` 열거형 처리기 그립니다 두 줄 50 픽셀 및 2 픽셀의 선 두께를 맨 위에 배치 하는 다른 줄의 스트로크 두께 사용 합니다. 이 두 번째 줄은 기하학적 시작과 선 두께 스트로크 단면 독립적인 줄의 끝을 보여 주기 위한:

[![](lines-images/strokecaps-small.png "스트로크 단면 페이지의 삼중 스크린샷")](lines-images/strokecaps-large.png#lightbox "스트로크 단면 페이지의 삼중 스크린샷")

볼 수 있듯이 `Square` 및 `Round` 스트로크 단면 스트로크 너비 절반 줄의 시작 및 끝에서 다시 여 줄 길이 효과적으로 확장 합니다. 이 확장은 렌더링 되는 그래픽 개체의 크기를 결정 해야 하는 경우 중요 합니다.

`SKCanvas` 클래스는 조금 특이 한 여러 줄을 그리기 위한 다른 방법을 포함 되어 있습니다.

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points` 매개 변수는 배열로 `SKPoint` 값 및 `mode` 의 멤버인는 [ `SKPointMode` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPointMode/) 에 세 명의 멤버가 있는 열거형:

- [`Points`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Points/) 개별 데이터 요소의 렌더링 하려면
- [`Lines`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Lines/) 각 쌍의 위치를 연결 하려면
- [`Polygon`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Polygon/) 모든 연속 점 연결 하려면

**여러 줄** 페이지에는이 방법을 보여 줍니다. [ `MultipleLinesPage` XAML 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/MultipleLinesPage.xaml) 두 개를 인스턴스화하고 `Picker` 수 있는 보기 선택의 멤버는 `SKPointMode` 열거형 및의 구성원은 `SKStrokeCap` 열거형:

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

`SelectedIndexChanged` 둘 다에 대 한 처리기 `Picker` 뷰 단순히 무효화는 `SKCanvasView` 개체:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

이 처리기가 있는지 여부를 확인 해야 합니다.는 `SKCanvasView` 개체가 첫 번째 이벤트 처리기가 호출 될 때는 `SelectedIndex` 속성의는 `Picker` XAML 파일에서 0으로 설정 하기 전에 발생 하는 `SKCanvasView` 인스턴스화된 합니다.

`PaintSurface` 에서 선택된 된 두 개의 항목을 가져오기 위한 제네릭 메서드를 액세스 하는 처리기는 `Picker` 뷰와 열거형 값에 변환:

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

스크린샷은의 다양 한 `Picker` 세 가지 플랫폼에서 선택 항목:

[![](lines-images/multiplelines-small.png "여러 줄 페이지의 삼중 스크린샷")](lines-images/multiplelines-large.png#lightbox "여러 줄 페이지의 삼중 스크린샷")

IPhone에서 왼쪽된에 표시 된 방법을 `SKPointMode.Points` 열거형 멤버 하면 `DrawPoints` 각 항목에 렌더링 하는 `SKPoint` 선 단면 경우 사각형으로 배열 `Butt` 또는 `Square`합니다. 선 끝 모양 이면 원으로 렌더링 됩니다 `Round`합니다.

대신 사용 하는 경우 `SKPointMode.Lines`Android 화면의 가운데에 표시 된 것 처럼는 `DrawPoints` 메서드의 각 쌍 사이 선을 그립니다 `SKPoint` 값,이 경우 지정 된 선 단면을 사용 하 여 `Round`합니다.

Windows 모바일 장치에서는의 결과 `SKPointMode.Polygon` 값입니다. 배열에 있는 연속 점 사이의 선이 그려집니다 있지만 매우 밀접 하 게을 보면이 줄 연결 되어 있지 않은 확인할 수 있습니다. 각각이 별도 줄의 시작 되며 지정 된 선 단면으로 끝납니다. 선택 하는 경우는 `Round` 캡이 포함 줄은 연결 된 것으로 보일 수도 있지만 실제로 연결 되어 있지 않은 합니다.

줄 연결 되거나 연결 되지 있는지 여부를 그래픽 경로 작업할 때 중요 한 요소입니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
