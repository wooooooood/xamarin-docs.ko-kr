---
제목: "다음에 통합" Xamarin.Forms 설명: "이 문서에서는 터치 및 요소에 응답 하는 SkiaSharp 그래픽을 만드는 방법을 설명 하 Xamarin.Forms 고 샘플 코드를 사용 하 여이를 보여 줍니다."
skiasharp assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A author: davidbritch: dabritch: ms. date: 02/09/2017 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="integrating-with-xamarinforms"></a>통합Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_터치 및 요소에 응답 하는 SkiaSharp 그래픽 만들기 Xamarin.Forms_

SkiaSharp 그래픽은 여러 가지 방법으로의 나머지와 통합할 수 있습니다 Xamarin.Forms . 동일한 페이지에서 SkiaSharp canvas와 요소를 결합 Xamarin.Forms 하 고 Xamarin.Forms SkiaSharp 캔버스 위에 요소를 배치할 수도 있습니다.

![](integration-images/integrationexample.png "Selecting a color with sliders")

에서 대화형 SkiaSharp 그래픽을 만드는 또 다른 방법은 Xamarin.Forms 터치를 통하는 것입니다.
[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 프로그램의 두 번째 페이지에는 **채우기 토글 탭**이 있습니다. 채우기가 없고 &mdash; 탭으로 채우기가 전환 된 간단한 원을 그립니다 &mdash; . [`TapToggleFillPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs)클래스는 사용자 입력에 응답 하 여 SkiaSharp 그래픽을 변경 하는 방법을 보여 줍니다.

이 페이지에 대 한 `SKCanvasView` 클래스는 [TapToggleFill](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) 파일에서 인스턴스화되어 뷰에서도 설정 합니다 Xamarin.Forms [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.TapToggleFillPage"
             Title="Tap Toggle Fill">

    <skia:SKCanvasView PaintSurface="OnCanvasViewPaintSurface">
        <skia:SKCanvasView.GestureRecognizers>
            <TapGestureRecognizer Tapped="OnCanvasViewTapped" />
        </skia:SKCanvasView.GestureRecognizers>
    </skia:SKCanvasView>
</ContentPage>
```

`skia`XML 네임 스페이스 선언을 확인 합니다.

`Tapped`개체의 처리기는 `TapGestureRecognizer` 단순히 부울 필드의 값을 전환 하 고 [`InvalidateSurface`](xref:SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface) 의 메서드를 호출 합니다 `SKCanvasView` .

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

에 대 한 호출은 `InvalidateSurface` `PaintSurface` 필드를 사용 하 여 원을 채우거 나 채우지 않도록 처리기에 대 한 호출을 효과적으로 생성 합니다 `showFill` .

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 50
    };
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);

    if (showFill)
    {
        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.Blue;
        canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    }
}
```

`StrokeWidth`속성이 50로 설정 되어 차이를 강조할. 내부를 먼저 그린 다음 윤곽선을 그려 전체 선 두께를 볼 수도 있습니다. 기본적으로 이벤트 처리기에서 나중에 그려진 그래픽 그림은 `PaintSurface` 처리기에서 앞에 그려진 그림을 숨깁니다.

**색 탐색** 페이지에서는 SkiaSharp 그래픽을 다른 요소와 통합할 수 있는 방법을 보여 Xamarin.Forms 주고, SkiaSharp에서 색을 정의 하는 두 가지 대체 방법 간의 차이점도 보여 줍니다. 정적 [`SKColor.FromHsl`](xref:SkiaSharp.SKColor.FromHsl(System.Single,System.Single,System.Single,System.Byte)) 메서드는 `SKColor` 색상-채도-밝기 모델을 기반으로 값을 만듭니다.

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

정적 [`SKColor.FromHsv`](xref:SkiaSharp.SKColor.FromHsv(System.Single,System.Single,System.Single,System.Byte)) 메서드는 `SKColor` 유사한 색상-채도-값 모델을 기반으로 값을 만듭니다.

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

두 경우 모두 `h` 인수 범위는 0에서 360 까지입니다. `s`, `l` 및 `v` 인수의 범위는 0에서 100 까지입니다. `a`(알파 또는 불투명도) 인수 범위는 0에서 255 까지입니다.

[**ColorExplorePage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) 파일은 `SKCanvasView` `StackLayout` `Slider` `Label` 사용자가 HSL 및 HSV color 값을 선택할 수 있는 및 보기와 나란히 두 개의 개체를 만듭니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Basics.ColorExplorePage"
             Title="Color Explore">
    <StackLayout>
        <!-- Hue slider -->
        <Slider x:Name="hueSlider"
                Maximum="360"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference hueSlider},
                              Path=Value,
                              StringFormat='Hue = {0:F0}'}" />

        <!-- Saturation slider -->
        <Slider x:Name="saturationSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference saturationSlider},
                              Path=Value,
                              StringFormat='Saturation = {0:F0}'}" />

        <!-- Lightness slider -->
        <Slider x:Name="lightnessSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference lightnessSlider},
                              Path=Value,
                              StringFormat='Lightness = {0:F0}'}" />

        <!-- HSL canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hslCanvasView"
                               PaintSurface="OnHslCanvasViewPaintSurface" />

            <Label x:Name="hslLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>

        <!-- Value slider -->
        <Slider x:Name="valueSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference valueSlider},
                              Path=Value,
                              StringFormat='Value = {0:F0}'}" />

        <!-- HSV canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hsvCanvasView"
                               PaintSurface="OnHsvCanvasViewPaintSurface" />

            <Label x:Name="hsvLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>
    </StackLayout>
</ContentPage>
```

두 `SKCanvasView` 요소는 `Grid` `Label` 결과 RGB 색 값을 표시 하기 위해 위에 앉아 있는 단일 셀에 있습니다.

[**ColorExplorePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) 코드 숨김이 비교적 단순 합니다. `ValueChanged`세 요소에 대 한 공유 처리기는 `Slider` 두 요소를 모두 무효화 합니다 `SKCanvasView` . `PaintSurface`처리기는 요소로 표시 된 색을 사용 하 여 캔버스를 지우고 `Slider` `Label` 요소 위에 앉아 있는를 설정 합니다 `SKCanvasView` .

```csharp
public partial class ColorExplorePage : ContentPage
{
    public ColorExplorePage()
    {
        InitializeComponent();

        hueSlider.Value = 0;
        saturationSlider.Value = 100;
        lightnessSlider.Value = 50;
        valueSlider.Value = 100;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        hslCanvasView.InvalidateSurface();
        hsvCanvasView.InvalidateSurface();
    }

    void OnHslCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsl((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)lightnessSlider.Value);
        args.Surface.Canvas.Clear(color);

        hslLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }

    void OnHsvCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsv((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)valueSlider.Value);
        args.Surface.Canvas.Clear(color);

        hsvLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }
}
```

HSL 및 HSV 색 모델에서 색상 값의 범위는 0에서 360이 고 색의 기준 색상을 나타냅니다. 이러한 색은 무지개의 전통적인 색 (빨강, 주황, 노랑, 녹색, 파랑, indigo, 보라)으로, 원 안의 빨간색으로 돌아옵니다.

HSL 모델에서 밝기의 0 값은 항상 검은색 이며 100 값은 항상 흰색입니다. 채도 값이 0 이면 0에서 100 사이의 밝기 값은 회색 음영입니다. 채도를 높이면 색이 더 늘어납니다. 순수한 색 (하나의 구성 요소가 255이 고, 다른 하나는 0이 고, 세 번째는 0 ~ 255) 인 RGB 값인 경우에는 채도가 100이 고 밝기는 50 인 경우 발생 합니다.

HSV 모델에서 순수 색은 채도와 값이 모두 100 인 경우에 발생 합니다. 값이 0 이면 다른 설정에 관계 없이 색은 검정입니다. 채도가 0이 고 값 범위가 0에서 100 인 경우 회색 음영이 발생 합니다.

그러나 두 모델에 대 한 느낌을 얻는 가장 좋은 방법은 직접 시험해 보는 것입니다.

[![](integration-images/colorexplore-large.png "Triple screenshot of the Color Explore page")](integration-images/colorexplore-small.png#lightbox "Triple screenshot of the Color Explore page")

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
