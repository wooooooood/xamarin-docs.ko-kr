---
title: 경로 채우기 유형
description: 이 문서에서는 SkiaSharp 경로 채우기 유형, 사용 가능한 다양 한 효과 검사 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 17043054c920a69570f38b227d05980494e29139
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615472"
---
# <a name="the-path-fill-types"></a>경로 채우기 유형

_SkiaSharp 경로 채우기 유형으로 가능한 다른 결과 검색 합니다._

경로에 두 개의 윤곽선 겹칠 수 및 단일 윤곽선을 구성 하는 줄이 겹칠 수 있습니다. 잠재적으로 모든 포함 된 영역을 채울 수 있습니다, 있지만 포함된 된 모든 영역을 입력 하지 않을 수도 있습니다. 예를 들면 다음과 같습니다.

![](fill-types-images/filltypeexample.png "점이 5 filles 부분적으로 별")

이 통해 약간 제어할 수 있습니다. 채우기 알고리즘에 의해 제어 됩니다는 [ `SKFillType` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.FillType/) 속성을 `SKPath`의 멤버로 설정 합니다 [ `SKPathFillType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathFillType/) 열거형:

- [`Winding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.Winding/)기본값
- [`EvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.EvenOdd/)
- [`InverseWinding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseWinding/)
- [`InverseEvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseEvenOdd/)

권선과 홀수 알고리즘을 모두 포함 된 모든 영역 채워진 인지 채워지지 무한대로 해당 영역에서 가져온 가상 줄에 따라 결정 합니다. 해당 줄 경로 구성 하는 하나 이상의 경계 줄과 교차 합니다. 감기 모드를 사용 하 여 경계 줄 방향으로 영역에 그릴 줄 수가 단방향 분산에 그릴 수 없는 경우 채워집니다. 그렇지 않은 경우 영역에 채워집니다. 홀수 알고리즘 경계선의 숫자가 홀수 이면 영역을 채웁니다.

많은 일상적인 경로 사용 하 여 감기 알고리즘 종종 경로의 모든 포함 된 영역을 채웁니다. 일반적으로 홀수 알고리즘 보다 흥미로운 결과 생성합니다.

에 설명 된 대로 전형적인 예로 점이 5 별 합니다 **Five-Pointed 별** 페이지입니다. 합니다 [FivePointedStarPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FivePointedStarPage.xaml) 파일은 두 `Picker` 뷰 경로 선택 하려면 입력 형식 및 경로 스트로크 아니면 채워진 여부 또는 둘 다를 어떤 순서로:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.FivePointedStarPage"
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
            <Picker.Items>
                <x:String>Winding</x:String>
                <x:String>EvenOdd</x:String>
                <x:String>InverseWinding</x:String>
                <x:String>InverseEvenOdd</x:String>
            </Picker.Items>
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
            <Picker.Items>
                <x:String>Fill only</x:String>
                <x:String>Stroke only</x:String>
                <x:String>Stroke then Fill</x:String>
                <x:String>Fill then Stroke</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

코드 숨김 파일을 모두 사용 하 여 `Picker` 점이 5 개 값:

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
        FillType = (SKPathFillType)Enum.Parse(typeof(SKPathFillType),
                        fillTypePicker.Items[fillTypePicker.SelectedIndex])
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

    switch (drawingModePicker.SelectedIndex)
    {
        case 0:
            canvas.DrawPath(path, fillPaint);
            break;

        case 1:
            canvas.DrawPath(path, strokePaint);
            break;

        case 2:
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case 3:
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

일반적으로 경로 채우기 유형에 영향을 주지 채우기와 스트로크 되지 않습니다 하지만 두 `Inverse` 모드 채우기와 스트로크에 영향을 줍니다. 두 채우기에 `Inverse` 형식 입력 영역 oppositely는 별 외부 영역을 채웁니다. 선의 경우 두 `Inverse` 스트로크를 제외한 모든 항목 형식 색입니다. 이러한 역 채우기 형식을 사용 하 여 iOS 스크린샷에서 보여 주듯이 일부 홀수 효과 만들 수 있습니다.

[![](fill-types-images/fivepointedstar-small.png "삼중 Five-Pointed 별 페이지 스크린샷")](fill-types-images/fivepointedstar-large.png#lightbox "삼중 Five-Pointed 별 페이지 스크린샷")

스크린샷에서 Android 및 UWP 일반적인 홀수 및 감기 효과 보여주지만 스트로크 및 채우기의 순서는 결과도 영향을 줍니다.

감기 알고리즘은 줄이 그려지는 방향에 따라 달라 집니다. 일반적으로 경로 만들 때, 제어할 수 있습니다 그 방향에서 다른 한 지점에서 줄을 그릴는 지정한. 그러나 합니다 `SKPath` 클래스와 같은 메서드 정의 `AddRect` 및 `AddCircle` 전체 윤곽을 그리는입니다. 메서드를 이러한 개체는 그리는 방법을 제어 하려면 형식 매개 변수를 포함 [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/), 두 명의 멤버가 있는 합니다.

- [`Clockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.Clockwise/)
- [`CounterClockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.CounterClockwise/)

메서드 `SKPath` 포함 하는 `SKPathDirection` 매개 변수 기본값은 지정 `Clockwise`합니다.

합니다 **겹치는 원** 페이지 홀수 경로 채우기 유형으로 4 개의 겹치는 원을 사용 하 여 경로 만듭니다.

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

[![](fill-types-images/overlappingcircles-small.png "겹치는 원 페이지 스크린샷 삼중")](fill-types-images/overlappingcircles-large.png#lightbox "삼중 겹치는 원 페이지 스크린샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
