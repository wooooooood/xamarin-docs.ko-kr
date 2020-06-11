---
제목: "SkiaSharp의 점 및 대시" 설명: "이 문서에서는 SkiaSharp에서 점선 및 파선 그리기의 복잡 한 부분을 마스터 하는 방법을 알아보고 샘플 코드를 사용 하 여이를 보여 줍니다."
assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078: xamarin-skiasharp author: davidbritch: dabritch:: 03/10/2017:: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="dots-and-dashes-in-skiasharp"></a>SkiaSharp의 점 및 대시

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp에서 점선 및 파선 그리기의 복잡성을 줄입니다._

SkiaSharp를 사용 하 여 실선이 아닌 선을 그릴 수 있지만 대신 점과 대시로 구성 됩니다.

![](dots-images/dottedlinesample.png "Dotted line")

의 속성으로 설정 하는 클래스의 인스턴스인 *경로 효과*를 사용 하 여이 작업을 수행 [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) [`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect) `SKPaint` 합니다. 로 정의 된 정적 생성 방법 중 하나를 사용 하 여 경로 효과를 만들거나 경로 효과를 조합할 수 있습니다 `SKPathEffect` . ( `SKPathEffect` 는 SkiaSharp에서 지 원하는 6 가지 효과 중 하나 이며 나머지는 [**SkiaSharp 효과**](../effects/index.md)섹션에 설명 되어 있습니다.)

점선이 나 파선을 그리려면 [`SKPathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) 정적 메서드를 사용 합니다. 인수에는 두 가지가 있습니다. 첫 번째 인수는 `float` 점 및 대시의 길이와 그 사이의 공백 길이를 나타내는 값의 배열입니다. 이 배열의 요소 수는 짝수 여야 하며 두 개 이상의 요소가 있어야 합니다. 배열에는 요소가 없을 수도 있지만이 경우에는 실선이 발생 합니다. 두 개의 요소가 있는 경우 첫 번째 요소는 점 또는 대시의 길이이 고 두 번째는 다음 점 또는 대시 앞에 있는 간격의 길이입니다. 세 개 이상의 요소가 있는 경우 대시 길이, 간격 길이, 대시 길이, 간격 길이 등의 순서로 정렬 됩니다.

일반적으로 대시 및 간격 길이를 스트로크 너비의 배수로 설정 합니다. 예를 들어 스트로크 너비가 10 픽셀인 경우 배열 {10, 10}에서 점과 간격이 스트로크 두께와 동일한 길이의 점선이 그려집니다.

그러나 `StrokeCap` 개체의 설정은 `SKPaint` 이러한 점과 대시에도 영향을 줍니다. 잠시 후에이 배열의 요소에 영향을 주는 것을 알 수 있습니다.

**점 및** 파선 페이지에서 점선 및 파선을 보여 줍니다. [**DotsAndDashesPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml) 파일은 두 개의 `Picker` 뷰를 인스턴스화합니다. 하나는 스트로크 캡을 선택 하 고 두 번째는 대시 배열을 선택 하는 것입니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.DotsAndDashesPage"
             Title="Dots and Dashes">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="0"
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

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>10, 10</x:String>
                    <x:String>30, 10</x:String>
                    <x:String>10, 10, 30, 10</x:String>
                    <x:String>0, 20</x:String>
                    <x:String>20, 20</x:String>
                    <x:String>0, 20, 20, 20</x:String>
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

 에서 처음 세 개의 항목은 `dashArrayPicker` 스트로크 너비가 10 픽셀인 것으로 가정 합니다. {10, 10} 배열은 점선을 위한 것이 고, {30, 10}은 파선을 위한 것 이며, {10, 10, 30, 10}은 점 및 대시 선입니다. 다른 세 가지 방법에 대해서는 곧 설명 합니다.

코드를 포함 하는 파일에는 [`DotsAndDashesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml.cs) `PaintSurface` 이벤트 처리기와 뷰에 액세스 하기 위한 두 가지 도우미 루틴이 포함 되어 있습니다 `Picker` .

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
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = (SKStrokeCap)strokeCapPicker.SelectedItem,
        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint);
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string str = (string)picker.SelectedItem;
    string[] strs = str.Split(new char[] { ' ', ',' }, StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

다음 스크린샷에서 맨 왼쪽에 있는 iOS 화면에는 점선이 표시 됩니다.

[![](dots-images/dotsanddashes-small.png "Triple screenshot of the Dots and Dashes page")](dots-images/dotsanddashes-large.png#lightbox "Triple screenshot of the Dots and Dashes page")

그러나 Android 화면에는 배열 {10, 10}을 사용 하는 점선이 표시 되어야 하지만 대신 실선이 있습니다. 무슨 일이 일어났나요? 문제는 Android 화면에는의 스트로크 캡 설정도 포함 되어 있기 때문입니다 `Square` . 이렇게 하면 스트로크 너비의 반만큼 모든 대시가 확장 되어 간격이 채워집니다.

또는의 스트로크 단면을 사용 하는 경우이 문제를 해결 하려면 `Square` `Round` 스트로크 길이로 배열의 대시 길이 (경우에 따라 대시 길이가 0 인 경우)를 줄이고 스트로크 길이로 간격 길이를 늘려야 합니다. 이는 XAML 파일의에서 마지막 3 개의 대시 배열이 계산 되는 방법입니다 `Picker` .

- 점선으로 {10, 10}이 (가) {0, 20}이 됩니다.
- 파선의 {30, 10}은 {20, 20}이 됩니다.
- 점 및 파선에 대해 {10, 10, 30, 10}은 {0, 20, 20, 20}이 됩니다.

UWP 화면에는의 스트로크 단면에 대 한 점선 및 파선이 표시 `Round` 됩니다. `Round`스트로크 캡은 종종 굵은 선으로 점 및 대시의 가장 좋은 모양을 제공 합니다.

지금까지 메서드에 대 한 두 번째 매개 변수를 언급 하지 않았습니다 `SKPathEffect.CreateDash` . 이 매개 변수의 이름은 `phase` 이며 줄의 시작 부분에 대 한 점 및 대시 패턴 내의 오프셋을 참조 합니다. 예를 들어 대시 배열이 {10, 10}이 고 `phase` 가 10 이면 줄은 점이 아닌 간격으로 시작 합니다.

매개 변수의 흥미로운 응용 프로그램 하나 `phase` 는 애니메이션에 있습니다. **애니메이션이 적용 된 나선형** 페이지는 **Archimedean 나선형** 페이지와 유사 합니다. 단, [`AnimatedSpiralPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/AnimatedSpiralPage.cs) 클래스는 `phase` 메서드를 사용 하 여 매개 변수에 애니메이션 효과를 적용 합니다 Xamarin.Forms `Device.Timer` .

```csharp
public class AnimatedSpiralPage : ContentPage
{
    const double cycleTime = 250;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float dashPhase;

    public AnimatedSpiralPage()
    {
        Title = "Animated Spiral";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
            dashPhase = (float)(10 * t);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }

            return pageIsActive;
        });
    }
    ···  
}
```

물론 실제로 프로그램을 실행 하 여 애니메이션을 확인 해야 합니다.

[![](dots-images/animatedspiral-small.png "Triple screenshot of the Animated Spiral page")](dots-images/animatedspiral-large.png#lightbox "Triple screenshot of the Animated Spiral page")

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
