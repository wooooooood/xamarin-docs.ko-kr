---
제목: "경로 채우기 유형" 설명: "이 문서에서는 SkiaSharp 경로 채우기 유형과 관련 된 다양 한 효과를 살펴보고 샘플 코드를 사용 하 여이를 보여 줍니다."
assetid: 57103A7A-49A2-46AE-894C-7C2664682644: xamarin-skiasharp author: davidbritch: dabritch:: 03/10/2017:: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="the-path-fill-types"></a>경로 채우기 유형

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp 경로 채우기 유형에 서 가능한 다른 효과를 검색 합니다._

패스의 두 컨투어는 겹칠 수 있으며, 단일 컨투어를 구성 하는 선은 겹칠 수 있습니다. 모든 포함 된 영역을 채울 수 있지만 포함 된 영역을 모두 채우지는 않을 수 있습니다. 예를 들면 다음과 같습니다.

![](fill-types-images/filltypeexample.png "Five-pointed star partially filles")

이에 대 한 제어가 거의 없습니다. 채우기 알고리즘은 [`SKFillType`](xref:SkiaSharp.SKPath.FillType) `SKPath` 열거형의 멤버로 설정 하는의 속성에 의해 제어 됩니다 [`SKPathFillType`](xref:SkiaSharp.SKPathFillType) .

- `Winding`, 기본값
- `EvenOdd`
- `InverseWinding`
- `InverseEvenOdd`

굴곡 및 짝수-홀수 알고리즘은 모두 해당 영역에서 무한대로 그려진 가상 줄을 기반으로 채워져 있는지 여부를 결정 합니다. 이 줄은 경로를 구성 하는 하나 이상의 경계 선을 교차 합니다. 굴곡 모드를 사용 하면 한 방향으로 그려진 경계 선 수가 다른 방향으로 그려진 선 수를 분산 하는 경우 영역이 채워지지 않습니다. 그렇지 않으면 영역이 채워집니다. 짝수-홀수 알고리즘은 경계 선 수가 홀수 인 경우 영역을 채웁니다.

많은 루틴 경로를 사용 하 여 굴곡 알고리즘은 경로에 포함 된 모든 영역을 채우는 경우가 많습니다. 짝수-홀수 알고리즘은 일반적으로 더 흥미로운 결과를 생성 합니다.

전형적인 예는 **5 방향 별** 페이지에서 보여 주는 것 처럼 5 방향 별입니다. [**Fivepointedstarpage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FivePointedStarPage.xaml) 파일은 두 개의 뷰를 인스턴스화하여 `Picker` 경로 채우기 유형을 선택 하 고 경로를 스트로크 또는 채우거 나 둘 다를 선택 하 고 순서를 지정 합니다.

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

코드를 사용 하는 파일은 두 값을 모두 사용 하 여 `Picker` 5 개의 뾰족한 별을 그립니다.

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

일반적으로 경로 채우기 형식은 채우기에만 영향을 미치며 스트로크에는 영향을 주지 않지만 두 `Inverse` 모드는 채우기와 스트로크 모두에 영향을 줍니다. 채우기의 경우 두 가지 `Inverse` 형식이 oppositely 영역을 채워서 별모양 외부 영역이 채워집니다. 스트로크의 경우 두 `Inverse` 형식 모두 스트로크를 제외한 모든 색을 색으로 합니다. 이러한 역 채우기 유형을 사용 하면 iOS 스크린샷에서 보여 주는 것 처럼 일부 이상한 효과가 발생할 수 있습니다.

[![](fill-types-images/fivepointedstar-small.png "Triple screenshot of the Five-Pointed Star page")](fill-types-images/fivepointedstar-large.png#lightbox "Triple screenshot of the Five-Pointed Star page")

Android 스크린 샷에서는 일반적인 짝수-홀수 및 권선 효과를 보여 주지만 스트로크 및 채우기의 순서는 결과에도 영향을 줍니다.

굴곡 알고리즘은 선이 그려지는 방향에 따라 결정 됩니다. 일반적으로 경로를 만들 때 한 점에서 다른 점으로 선을 그리도록 지정 하는 것과 같은 방향을 제어할 수 있습니다. 그러나 클래스는 `SKPath` 전체 컨투어를 그리는 등의 메서드를 정의 합니다 `AddRect` `AddCircle` . 이러한 개체를 그리는 방법을 제어 하기 위해 메서드에는 두 개의 멤버가 있는 형식의 매개 변수가 포함 됩니다 [`SKPathDirection`](xref:SkiaSharp.SKPathDirection) .

- `Clockwise`
- `CounterClockwise`

`SKPath`매개 변수를 포함 하는의 메서드는 `SKPathDirection` 기본값을 제공 `Clockwise` 합니다.

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

최소한의 코드로 만든 흥미로운 이미지입니다.

[![](fill-types-images/overlappingcircles-small.png "Triple screenshot of the Overlapping Circles page")](fill-types-images/overlappingcircles-large.png#lightbox "Triple screenshot of the Overlapping Circles page")

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
