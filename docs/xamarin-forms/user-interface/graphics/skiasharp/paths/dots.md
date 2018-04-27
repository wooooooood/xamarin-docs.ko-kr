---
title: 점, 대시
description: 마스터 SkiaSharp에 점선과 파선 선 그리기의 고급 기능
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 1e295ac424c311472ff175d4627c5fb12641d31f
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="dots-and-dashes"></a>점, 대시

_마스터 SkiaSharp에 점선과 파선 선 그리기의 고급 기능_

SkiaSharp에서는 실선은 아니라 점과 대시의으로 구성 되며, 대신 있는 선을 그릴 수 있습니다.

![](dots-images/dottedlinesample.png "점선")

이 작업을 수행는 *경로 효과*의 인스턴스인는 [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) 로 설정 하는 클래스는 [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) 속성 `SKPaint`합니다. 경로 만들 수 있습니다는 정적을 사용 하 여 효과 (또는 결합 되어 감사가 만들어집니다 경로 효과) `Create` 정의한 메서드 `SKPathEffect`합니다.

점선된 또는 파선된 그리기를 사용 하는 [ `SKPathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) 정적 메서드입니다. 두 인수는: 배열을이 먼저는 `float` 점과 대시의 길이 및 사이 공백의 길이 나타내는 값입니다. 이 배열의 요소 수는 짝수 있고 둘 이상의 요소가 있어야 합니다. (있을 수는 배열의 요소 개수가 0 있지만 실선에서 발생 시키는.) 두 요소가 있는 경우 첫 번째는 점 또는 대시의 길이 이며 두 번째 점 또는 대시 다음 앞에서 간격의 길이 합니다. 두 개 이상의 요소 있으면이 순서로 되어: 대시 길이 "," 간격 길이 "," 대시 길이 "," 간격 길이 "및" 등입니다.

일반적으로 스트로크 너비의 배수 대시와 간격 길이 확인 합니다. 스트로크 너비는 10 픽셀, 예를 들어 다음 배열 {10, 10} 그립니다 점선 점 및 간격이 있는 선 두께와 같은 길이.

그러나는 `StrokeCap` 설정인는 `SKPaint` 개체도 영향을 줍니다 이러한 점과 대시 합니다. 곧, 알게이 배열의 요소에 영향을 미칩니다입니다.

점 표기법에 따른 및 파선에서 보여지는 **점선 및 대시** 페이지. [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml) 파일 두 개를 인스턴스화하고 `Picker` 획 cap 및 dash 배열을 선택 하 고 두 번째 선택할 수에 대 한 뷰:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.DotsAndDashesPage"
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
            <Picker.Items>
                <x:String>Butt</x:String>
                <x:String>Round</x:String>
                <x:String>Square</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>10, 10</x:String>
                <x:String>30, 10</x:String>
                <x:String>10, 10, 30, 10</x:String>
                <x:String>0, 20</x:String>
                <x:String>20, 20</x:String>
                <x:String>0, 20, 20, 20</x:String>
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

처음 세 항목은 `dashArrayPicker` 스트로크 너비 10 픽셀 것으로 가정 합니다. {10, 10} 배열이 점선에 대 한 {30, 10}가 파선 및 {10, 10, 30, 10}에 대 한 점 파선입니다. (다른 3 개가 살펴봅니다 곧.)

[ `DotsAndDashesPage` 코드 숨김 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs) 포함는 `PaintSurface` 이벤트 처리기 및 몇 가지에 액세스 하기 위한 도우미 루틴의 `Picker` 보기:

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
        StrokeCap = (SKStrokeCap)Enum.Parse(typeof(SKStrokeCap),
                        strokeCapPicker.Items[strokeCapPicker.SelectedIndex]),

        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }

    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string[] strs = picker.Items[picker.SelectedIndex].Split(new char[] { ' ', ',' },
                                                             StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

다음 스크린샷에서 왼쪽 끝에 iOS 화면 점선을 보여 줍니다.

[![](dots-images/dotsanddashes-small.png "점, 대시 페이지의 삼중 스크린샷")](dots-images/dotsanddashes-large.png#lightbox "점과 대시 페이지의 삼중 스크린 샷")

Android 화면 점선 {10, 10} 배열을 사용 하 여 표시 되는 또한 하지만 대신 선은 단색입니다. 어떻게 된 것입니까? 문제는 Android 화면에서는 스트로크 caps 설정이 있는지 `Square`합니다. 모든 대시 간격 모두 채울 그 결과, 절반 스트로크 너비를 확장 합니다.

스트로크 cap를 사용 하는 경우이 문제를 해결 하려면 `Square` 또는 `Round`, (경우에 따라 결과 대시의 길이가 0), stroke 길이 의해 배열에서 대시 길이 감소 하며 스트로크 길이 의해 간격 길이 늘립니다. 이것은 어떻게 최종 3 대시 배열에는 `Picker` 계산 된 XAML 파일에:

- {10, 10}은 {0, 20} 점선에 대 한
- {30, 10}은 {20, 20} 파선에 대 한
- {10, 10, 30, 10} {0, 20, 20, 20} 점선과 파선 선 되 면

파선 선의 점 표기법에 따른 UWP 화면 표시 된의 캡 `Round`합니다. `Round` 선 단면 두꺼운 선에서 점, 대시의 최상의 모양을 제공 합니다.

지금까지 언급 되지 이루어졌을 두 번째 매개 변수는 `SKPathEffect.CreateDash` 메서드. 이 매개 변수 이름이 `phase` 하는 줄의 시작 부분에 대 한 점 대시 패턴 내의 오프셋을 지칭 합니다. 예를 들어, 대시 배열이 {10, 10} 및 `phase` 은 10이 고 다음 줄 점 대신 간격으로 시작 합니다.

한 가지 흥미로운 적용은 `phase` 매개 변수는 애니메이션에서 합니다. **애니메이션 나선** 페이지는 비슷합니다는 **Archimedean 나선** 점을 제외 하 고 페이지는 [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/AnimatedSpiralPage.cs) 클래스 애니메이션 효과 적용는 `phase` 매개 변수입니다. 페이지에는 애니메이션에 또 다른 방법을 보여 줍니다. 이전 예는 [ `PulsatingEllipsePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml.cs) 사용 되는 `Task.Delay` 애니메이션을 제어 하는 메서드. 이 예에서는 대신 Xamarin.Forms는 `Device.Timer` 메서드:


```csharp
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
```

물론, 실제로 애니메이션을 확인 하기 위해 프로그램을 실행 해야 합니다.

[![](dots-images/animatedspiral-small.png "애니메이션을 나선 페이지의 삼중 스크린샷")](dots-images/animatedspiral-large.png#lightbox "애니메이션 나선 페이지의 삼중 스크린샷")

이제 선을 그리는 파라메트릭 수식을 사용 하 여 곡선을 정의 하는 방법을 살펴보았습니다. 나중에 게시할는 섹션은 다양 한 종류의 곡선을 설명 하는 `SKPath` 지원 합니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
