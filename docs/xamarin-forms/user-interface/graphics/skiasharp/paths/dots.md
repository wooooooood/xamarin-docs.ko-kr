---
title: 점 및 대시 SkiaSharp에서
description: 이 문서에서 SkiaSharp, 점선과 파선 선 그리기의 복잡성을 마스터 하는 방법에 살펴봅니다 및 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 229f60cbb96058454a1c634e53a7bb00ec725bcf
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723756"
---
# <a name="dots-and-dashes-in-skiasharp"></a>점 및 대시 SkiaSharp에서

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp에서 점선 및 파선 그리기의 복잡성을 줄입니다._

SkiaSharp solid 되지 않지만 대신 점 및 대시 구성 되는 줄을 그릴 수 있습니다.

![](dots-images/dottedlinesample.png "Dotted line")

*경로 효과*를 사용 하 여이 작업을 수행 합니다 .이는 `SKPaint`의 [`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect) 속성으로 설정 하는 [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) 클래스의 인스턴스입니다. `SKPathEffect`에서 정의한 정적 생성 방법 중 하나를 사용 하 여 경로 효과를 만들거나 경로 효과를 조합할 수 있습니다. (`SKPathEffect`는 SkiaSharp에서 지 원하는 6 가지 효과 중 하나 이며 나머지는 [**SkiaSharp 효과**](../effects/index.md)섹션에 설명 되어 있습니다.)

점선이 나 파선을 그리려면 [`SKPathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) 정적 메서드를 사용 합니다. 인수에는 두 가지가 있습니다. 첫 번째 인수는 점 및 대시의 길이와 그 사이의 공백 길이를 나타내는 `float` 값의 배열입니다. 이 배열 요소 수는 짝수 있고 둘 이상의 요소가 있어야 합니다. 배열에는 요소가 없을 수도 있지만이 경우에는 실선이 발생 합니다. 두 개의 요소가 있는 경우 첫 번째 요소는 점 또는 대시의 길이이 고 두 번째는 다음 점 또는 대시 앞에 있는 간격의 길이입니다. 이 순서 있는 경우 두 개 이상의 요소가: 대시 길이 "," 간격 길이 "," 대시 길이 "," 간격 길이 "및" 등입니다.

일반적으로 스트로크 너비의 배수로 대시 및 간격 길이 확인 해야 합니다. 스트로크 너비는 10 픽셀, 예를 들어, 다음 배열은 {10, 10} 그립니다 점선 점과 간격이 있는 스트로크 두께와 같은 길이.

그러나 `SKPaint` 개체의 `StrokeCap` 설정은 이러한 점과 대시에도 영향을 줍니다. 곧 확인 하겠지만 대로이 배열의 요소에 영향을 주게입니다.

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

 `dashArrayPicker`의 처음 3 개 항목은 스트로크 너비가 10 픽셀인 것으로 가정 합니다. {10, 10} 배열이 dotted 줄에 대 한 {30, 10} 점선으로 및 {10, 10, 30, 10}는 점 파선입니다. (다른 3 개는 잠시 후에 설명 합니다.)

[`DotsAndDashesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml.cs) 코드를 포함 하는 파일에는 `PaintSurface` 이벤트 처리기와 `Picker` 뷰에 액세스 하기 위한 두 가지 도우미 루틴이 포함 되어 있습니다.

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

다음 스크린샷에서 맨 왼쪽에 있는 iOS 화면에는 점선을 표시 됩니다.

[![](dots-images/dotsanddashes-small.png "Triple screenshot of the Dots and Dashes page")](dots-images/dotsanddashes-large.png#lightbox "Triple screenshot of the Dots and Dashes page")

그러나 Android 화면 표시 {10, 10} 배열을 사용 하 여 점선 할 수도 아니지만 줄 실선입니다. 경우 문제는 Android 화면에 `Square`의 스트로크 캡 설정도 있다는 것입니다. 이 간격을 채우도록을 일으킨 절반 스트로크 너비에서 모든 대시를 확장 합니다.

`Square` 또는 `Round`의 스트로크 캡을 사용 하는 경우이 문제를 해결 하려면 스트로크 길이로 배열의 대시 길이 (경우에 따라 대시 길이가 0 인 경우)를 줄이고 스트로크 길이로 간격 길이를 늘려야 합니다. 이는 XAML 파일의 `Picker`에 있는 마지막 3 개의 대시 배열이 계산 되는 방법입니다.

- {10, 10} 수 {0, 20} 점선의
- {30, 10} 됩니다 {20, 20} 점선에 대 한
- {10, 10, 30, 10} {0, 20, 20, 20} 점선과 파선 줄은

UWP 화면에는 `Round`의 스트로크 캡에 대 한 점선 및 파선이 표시 됩니다. `Round` 스트로크 캡은 종종 굵은 선으로 점과 대시를 가장 잘 보여 줍니다.

지금까지 `SKPathEffect.CreateDash` 메서드에 대 한 두 번째 매개 변수를 언급 하지 않았습니다. 이 매개 변수는 `phase` 이름이 지정 되며 줄의 시작에 대 한 점 및 대시 패턴 내의 오프셋을 참조 합니다. 예를 들어 대시 배열이 {10, 10}이 고 `phase` 10 인 경우 줄은 점이 아닌 간격으로 시작 합니다.

`phase` 매개 변수의 한 가지 흥미로운 응용 프로그램은 애니메이션입니다. [`AnimatedSpiralPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/AnimatedSpiralPage.cs) 클래스가 xamarin.ios `Device.Timer` 메서드를 사용 하 여 `phase` 매개 변수에 애니메이션을 적용 하는 경우를 제외 하 고 **애니메이션 된 나선형** 페이지는 **Archimedean 나선형** 페이지와 비슷합니다.

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

물론, 실제로 애니메이션을 확인 하기 위해 프로그램을 실행 해야 합니다.

[![](dots-images/animatedspiral-small.png "Triple screenshot of the Animated Spiral page")](dots-images/animatedspiral-large.png#lightbox "Triple screenshot of the Animated Spiral page")

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
