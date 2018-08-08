---
title: Xamarin.Forms와 통합
description: 이 문서는 터치에 응답 하는 SkiaSharp 그래픽을 만드는 방법 및 Xamarin.Forms 요소에 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 23dcc6f11f40283a220aba47b33717e7e5740dbe
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615849"
---
# <a name="integrating-with-xamarinforms"></a>Xamarin.Forms와 통합

_터치 및 Xamarin.Forms 요소에 응답 하는 SkiaSharp 그래픽 만들기_

SkiaSharp 그래픽은 여러 가지 방법으로 Xamarin.Forms의 나머지 부분에 통합할 수 있습니다. SkiaSharp 캔버스 및 Xamarin.Forms 요소가 동일한 페이지에서 SkiaSharp 캔버스 맨 위에 위치 Xamarin.Forms 요소를 결합할 수 있습니다.

![](integration-images/integrationexample.png "슬라이더를 사용 하 여 색을 선택합니다.")

Xamarin.Forms에서 SkiaSharp 그래픽 대화형 만드는 다른 방법은 터치를 통해 것입니다.
두 번째 페이지에는 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 프로그램 자격이 **토글 채우기 탭**합니다. 두 가지 방법으로 단순 원을 그릴 &mdash; 채우기를 사용 하 여 채우기 없는 &mdash; 탭으로 전환 합니다. 합니다 [ `TapToggleFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) 클래스가 SkiaSharp 그래픽 사용자 입력에 대 한 응답에서을 변경할 수 있습니다 하는 방법을 보여 줍니다.

이 페이지에 대 한 합니다 `SKCanvasView` 클래스에서 인스턴스화됩니다 합니다 [TapToggleFill.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) 도 Xamarin.Forms를 설정 하는 파일 [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) 보기에서:

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

통지를 `skia` XML 네임 스페이스 선언 합니다.

합니다 `Tapped` 에 대 한 처리기를 `TapGestureRecognizer` 개체를 토글합니다 호출 하는 부울 필드의 값을 [ `InvalidateSurface` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface()/) 메서드의 `SKCanvasView`:

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

에 대 한 호출 `InvalidateSurface` 효과적으로에 대 한 호출을 생성 합니다 `PaintSurface` 처리기를 사용 하는 `showFill` 채우거 나 원 채우지 필드:

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

`StrokeWidth` 속성이 차이 강조 하기 위해 50으로 설정 되어 있습니다. 또한 먼저 내부 그리기 및 윤곽선으로 전체 줄 너비를 볼 수 있습니다. 기본적으로 그래픽 그림의 뒷부분에 나오는 그려지는 `PaintSurface` 이벤트 처리기를가 려 서 이전 처리기에서에서 그린 것입니다.

합니다 **Color 탐색** 페이지 어떻게 SkiaSharp 그래픽 다른 Xamarin.Forms 요소와 통합할 수도 있습니다 보여 주고도 SkiaSharp에서 색을 정의 하기 위한 두 가지 대체 방법 간의 차이 보여 줍니다. 정적 [ `SKColor.FromHsl` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsl/p/System.Single/System.Single/System.Single/System.Byte/) 메서드를 만듭니다는 `SKColor` 색상-채도-명도 모델을 기반으로 하는 값:

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

정적 [ `SKColor.FromHsv` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsv/p/System.Single/System.Single/System.Single/System.Byte/) 메서드를 만듭니다는 `SKColor` 유사한 색상-채도 값 모델을 기반으로 하는 값:

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

두 경우 모두는 `h` 인수 범위는 0에서 360 사이입니다. 합니다 `s`, `l`, 및 `v` 인수 0에서 100 사이입니다. `a` (알파 또는 불투명도) 인수 범위는 0에서 255입니다.

[ **ColorExplorePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) 파일 두 개를 만듭니다 `SKCanvasView` 개체를 `StackLayout` 나란히 사용 하 여 `Slider` 고 `Label` HSL을 선택할 수 있는 뷰 및 HSV 색 값:

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

두 `SKCanvasView` 단일 셀에는 요소가 `Grid` 사용 하 여를 `Label` 결과 RGB 색 값을 표시 하기 위한 상위 하나 있는데요.

합니다 [ **ColorExplorePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) 코드 숨김 파일은 비교적 간단 합니다. 공유 `ValueChanged` 세 가지에 대 한 처리기 `Slider` 요소를 간단히 모두 무효화 `SKCanvasView` 요소입니다. `PaintSurface` 처리기에서 표시 된 색을 사용 하 여 캔버스 지우기는 `Slider` 요소를 설정 합니다 `Label` 위쪽에 앉아 있는 `SKCanvasView` 요소:

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

HSL 및 HSV 색 모델의 색상 값 범위는 0에서 360 하 고 색의 주요 색상을 나타냅니다. 무지개의 기존 색입니다: 빨강, 주황, 노랑, 녹색, 파란색, indigo, 보라, 및 빨간색 원 안에 백 합니다.

HSL 모델에서 명도 0 값은 항상 검은색으로 며 100 값을 항상 흰색입니다. 채도 값이 0 이면 명도 값 0과 100 사이의 회색 음영 됩니다. 채도 늘리면 더 많은 색을 추가 합니다. 순수 색 (임, 하나의 구성 요소가 0과는 세 번째 0에서 255 까지의 같은 다른 255를 사용 하 여 RGB 값)은 100 채도 및 밝기는 50 때 발생 합니다.

HSV 모델에서 순수 색 채도 값은 100 때 발생 합니다. 값이 다른 설정에 관계 없이 0 인 경우 색은 검정입니다. 회색조 채도 0과 값 범위는 0에서 100 때 발생 합니다.

하지만 두 모델에 대 한 이해를 위한 가장 좋은 방법은 상호 실험:

[![](integration-images/colorexplore-large.png "삼중 색 탐색 페이지 스크린샷")](integration-images/colorexplore-small.png#lightbox "삼중 색 탐색 페이지 스크린샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
