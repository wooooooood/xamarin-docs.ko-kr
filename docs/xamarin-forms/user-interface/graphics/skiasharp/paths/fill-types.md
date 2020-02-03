---
title: 경로 채우기 유형
description: 이 문서에서는 SkiaSharp 경로 채우기 유형, 사용 가능한 다양 한 효과 검사 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 98081ed1a9aef1260150671d4fd026dd64c20b62
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723641"
---
# <a name="the-path-fill-types"></a>경로 채우기 유형

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp 경로 채우기 유형에 서 가능한 다른 효과를 검색 합니다._

경로에 두 개의 윤곽선 겹칠 수 및 단일 윤곽선을 구성 하는 줄이 겹칠 수 있습니다. 잠재적으로 모든 포함 된 영역을 채울 수 있습니다, 있지만 포함된 된 모든 영역을 입력 하지 않을 수도 있습니다. 예를 들면 다음과 같습니다.

![](fill-types-images/filltypeexample.png "Five-pointed star partially filles")

이 통해 약간 제어할 수 있습니다. 채우기 알고리즘은 [`SKPathFillType`](xref:SkiaSharp.SKPathFillType) 열거의 멤버로 설정 하는 `SKPath`의 [`SKFillType`](xref:SkiaSharp.SKPath.FillType) 속성에 의해 제어 됩니다.

- `Winding`기본값
- `EvenOdd`
- `InverseWinding`
- `InverseEvenOdd`

권선과 홀수 알고리즘을 모두 포함 된 모든 영역 채워진 인지 채워지지 무한대로 해당 영역에서 가져온 가상 줄에 따라 결정 합니다. 해당 줄 경로 구성 하는 하나 이상의 경계 줄과 교차 합니다. 감기 모드를 사용 하 여 경계 줄 방향으로 영역에 그릴 줄 수가 단방향 분산에 그릴 수 없는 경우 채워집니다. 그렇지 않은 경우 영역에 채워집니다. 홀수 알고리즘 경계선의 숫자가 홀수 이면 영역을 채웁니다.

많은 일상적인 경로 사용 하 여 감기 알고리즘 종종 경로의 모든 포함 된 영역을 채웁니다. 일반적으로 홀수 알고리즘 보다 흥미로운 결과 생성합니다.

전형적인 예는 **5 방향 별** 페이지에서 보여 주는 것 처럼 5 방향 별입니다. [**Fivepointedstarpage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FivePointedStarPage.xaml) 파일은 두 개의 `Picker` 뷰를 인스턴스화하여 경로 채우기 유형을 선택 하 고 경로를 스트로크 또는 채우거 나 둘 다에 대해 선택 하 고 순서를 지정 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.FivePointedStarPage"
             Title="Five-Pointed Star">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="fillTypePicker"
                Title="Path Fill Type"
                Grid.Row="0"
                Grid.Column="0"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKPathFillType}">
                    <x:Static Member="skia:SKPathFillType.Winding" />
                    <x:Static Member="skia:SKPathFillType.EvenOdd" />
                    <x:Static Member="skia:SKPathFillType.InverseWinding" />
                    <x:Static Member="skia:SKPathFillType.InverseEvenOdd" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="drawingModePicker"
                Title="Drawing Mode"
                Grid.Row="0"
                Grid.Column="1"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Fill only</x:String>
                    <x:String>Stroke only</x:String>
                    <x:String>Stroke then Fill</x:String>
                    <x:String>Fill then Stroke</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2"
                                PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

코드를 사용 하는 경우에는 `Picker` 값을 사용 하 여 5 개의 뾰족한 별을 그립니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPath path = new SKPath
    {
        FillType = (SKPathFillType)fillTypePicker.SelectedItem
    };
    path.MoveTo(info.Width / 2, info.Height / 2 - radius);

    for (int i = 1; i < 5; i++)
    {
        // angle from vertical
        double angle = i * 4 * Math.PI / 5;
        path.LineTo(center + new SKPoint(radius * (float)Math.Sin(angle),
                                        -radius * (float)Math.Cos(angle)));
    }
    path.Close();

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 50,
        StrokeJoin = SKStrokeJoin.Round
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    switch ((string)drawingModePicker.SelectedItem)
    {
        case "Fill only":
            canvas.DrawPath(path, fillPaint);
            break;

        case "Stroke only":
            canvas.DrawPath(path, strokePaint);
            break;

        case "Stroke then Fill":
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case "Fill then Stroke":
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

일반적으로 경로 채우기 형식은 채우기에만 영향을 미치며 스트로크에는 영향을 주지 않지만 두 개의 `Inverse` 모드는 채우기와 스트로크 모두에 영향을 줍니다. 채우기의 경우 두 `Inverse` 형식은 영역을 채우는 영역을 oppositely 별모양 외부 영역이 채워집니다. 스트로크의 경우 두 `Inverse` 형식은 스트로크를 제외한 모든 색을 색으로 합니다. 이러한 역 채우기 형식을 사용 하 여 iOS 스크린샷에서 보여 주듯이 일부 홀수 효과 만들 수 있습니다.

[![](fill-types-images/fivepointedstar-small.png "Triple screenshot of the Five-Pointed Star page")](fill-types-images/fivepointedstar-large.png#lightbox "Triple screenshot of the Five-Pointed Star page")

Android 스크린 샷에서는 일반적인 짝수-홀수 및 권선 효과를 보여 주지만 스트로크 및 채우기의 순서는 결과에도 영향을 줍니다.

감기 알고리즘은 줄이 그려지는 방향에 따라 달라 집니다. 일반적으로 경로 만들 때, 제어할 수 있습니다 그 방향에서 다른 한 지점에서 줄을 그릴는 지정한. 그러나 `SKPath` 클래스는 `AddRect`와 같은 메서드 및 전체 컨투어를 그리는 `AddCircle`도 정의 합니다. 이러한 개체를 그리는 방법을 제어 하기 위해 메서드에는 두 개의 멤버가 있는 [`SKPathDirection`](xref:SkiaSharp.SKPathDirection)형식의 매개 변수가 포함 됩니다.

- `Clockwise`
- `CounterClockwise`

`SKPathDirection` 매개 변수를 포함 하는 `SKPath` 메서드는 `Clockwise`의 기본값을 제공 합니다.

**겹치는 원** 페이지는 짝수-홀수 경로 채우기 유형을 사용 하 여 네 개의 겹치는 원이 있는 경로를 만듭니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(info.Width, info.Height) / 4;

    SKPath path = new SKPath
    {
        FillType = SKPathFillType.EvenOdd
    };

    path.AddCircle(center.X - radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X - radius / 2, center.Y + radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y + radius / 2, radius);

    SKPaint paint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    canvas.DrawPath(path, paint);

    paint.Style = SKPaintStyle.Stroke;
    paint.StrokeWidth = 10;
    paint.Color = SKColors.Magenta;

    canvas.DrawPath(path, paint);
}
```

이 최소한의 코드만 사용 하 여 만든 흥미로운 이미지:

[![](fill-types-images/overlappingcircles-small.png "Triple screenshot of the Overlapping Circles page")](fill-types-images/overlappingcircles-large.png#lightbox "Triple screenshot of the Overlapping Circles page")

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
