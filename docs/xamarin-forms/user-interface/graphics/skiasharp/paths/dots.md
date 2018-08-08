---
title: 점 및 대시 SkiaSharp에서
description: 이 문서에서 SkiaSharp, 점선과 파선 선 그리기의 복잡성을 마스터 하는 방법에 살펴봅니다 및 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 7c336e6b5224f61ff84eb39652788b23f52b806e
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615420"
---
# <a name="dots-and-dashes-in-skiasharp"></a>점 및 대시 SkiaSharp에서

_SkiaSharp에서 점선과 파선 선 그리기의 복잡성을 마스터_

SkiaSharp solid 되지 않지만 대신 점 및 대시 구성 되는 줄을 그릴 수 있습니다.

![](dots-images/dottedlinesample.png "점선")

이 작업을 수행을 *경로 효과*의 인스턴스인 합니다 [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) 로 설정 하는 클래스를 [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) 속성 `SKPaint`. 경로 만들 수 있습니다 사용 하는 정적 효과 (또는 결합 경로 효과) `Create` 정의한 메서드 `SKPathEffect`합니다.

사용할 점선된 또는 파선된을 그리려면 합니다 [ `SKPathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) 정적 메서드입니다. 두 개의 인수: 배열을 먼저 이것이 `float` 점 및 대시의 길이 사이 공백의 길이 나타내는 값입니다. 이 배열 요소 수는 짝수 있고 둘 이상의 요소가 있어야 합니다. (있을 수는 배열의 요소가 있지만 실선에서 발생 하는.) 두 개의 요소가 있는 경우 첫 번째 점 또는 대시의 길이 이며 두 번째 간격의 길이 다음 점 또는 대시 하기 전에 합니다. 이 순서 있는 경우 두 개 이상의 요소가: 대시 길이 "," 간격 길이 "," 대시 길이 "," 간격 길이 "및" 등입니다.

일반적으로 스트로크 너비의 배수로 대시 및 간격 길이 확인 해야 합니다. 스트로크 너비는 10 픽셀, 예를 들어, 다음 배열은 {10, 10} 그립니다 점선 점과 간격이 있는 스트로크 두께와 같은 길이.

그러나 합니다 `StrokeCap` 설정의 `SKPaint` 개체도 영향을 줍니다 이러한 점 및 대시 합니다. 곧 확인 하겠지만 대로이 배열의 요소에 영향을 주게입니다.

점 및에 파선 설명 되어는 **점 및 대시** 페이지입니다. 합니다 [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml) 파일은 두 `Picker` 스트로크 단면 및 dash 배열을 선택 하 고 두 번째 선택할 수에 대 한 뷰:

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

처음 세 개의 항목을 `dashArrayPicker` 스트로크 너비 10 픽셀 것으로 가정 합니다. {10, 10} 배열이 dotted 줄에 대 한 {30, 10} 점선으로 및 {10, 10, 30, 10}는 점 파선입니다. (다른 3 개는 잠시 후에 설명 합니다.)

[ `DotsAndDashesPage` 코드 숨김 파일](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs) 포함 된 `PaintSurface` 이벤트 처리기와 도우미 루틴에 액세스 하기 위한 몇은 `Picker` 뷰:

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

다음 스크린샷에서 맨 왼쪽에 있는 iOS 화면에는 점선을 표시 됩니다.

[![](dots-images/dotsanddashes-small.png "점 및 대시 페이지 스크린샷 삼중")](dots-images/dotsanddashes-large.png#lightbox "Triple 점 및 대시 페이지 스크린샷")

그러나 Android 화면 표시 {10, 10} 배열을 사용 하 여 점선 할 수도 아니지만 줄 실선입니다. 어떻게 된 것입니까? 문제는 Android 화면에서는 스트로크 cap 설정이 `Square`합니다. 이 간격을 채우도록을 일으킨 절반 스트로크 너비에서 모든 대시를 확장 합니다.

스트로크 단면을 사용 하는 경우이 문제를 해결 하려면 `Square` 또는 `Round`, 대시 길이 배열 (경우에 따라 결과 대시의 길이가 0), 스트로크 길이로 감소 하며 스트로크 길이로 간격 길이 늘립니다. 이것이, 어떻게 최종 세 가지 대시의 배열은 `Picker` 계산 된 XAML 파일에:

- {10, 10} 수 {0, 20} 점선의
- {30, 10} 됩니다 {20, 20} 점선에 대 한
- {10, 10, 30, 10} {0, 20, 20, 20} 점선과 파선 줄은

스트로크에 대 한 줄 점과 점선는 UWP 화면 표시의 상한을 `Round`합니다. `Round` 스트로크 단면 두꺼운 선에서 점 및 대시의 최적 모양을 제공 합니다.

지금까지 언급 되지 된 내용이 두 번째 매개 변수는 `SKPathEffect.CreateDash` 메서드. 이 매개 변수의 이름은 `phase` 줄의 시작 부분에 대 한 점 및 대시 패턴 내의 오프셋을 지칭 합니다. 예를 들어 dash 배열은 {10, 10} 및 `phase` 은 10이 고 다음 줄 점 보다는 간격을 시작 합니다.

한 가지 흥미로운 적용 된 `phase` 매개 변수는 애니메이션입니다. **나선형 애니메이션** 비슷합니다는 **Archimedean 나선형** 는 제외 하 고 페이지를 [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/AnimatedSpiralPage.cs) 클래스가 애니메이션을 적용를 `phase` 매개 변수. 페이지에는 또 다른 방법은 애니메이션을 보여 줍니다. 이전 예제는 [ `PulsatingEllipsePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml.cs) 사용 된 `Task.Delay` 애니메이션을 제어 하는 방법. 이 예제에서는 대신 Xamarin.Forms `Device.Timer` 메서드:


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

[![](dots-images/animatedspiral-small.png "애니메이션 나선형 페이지 스크린샷 삼중")](dots-images/animatedspiral-large.png#lightbox "삼중 애니메이션 나선형 페이지 스크린샷")

이제 선을 그리려면 매개 방정식을 사용 하 여 곡선을 정의 하는 방법을 살펴봤습니다. 나중에 게시할 섹션에서는 다양 한 종류의 곡선을 설명 하는 `SKPath` 지원 합니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
