---
title: Xamarin.Forms를 사용한 통합
description: 터치 및 Xamarin.Forms 요소에 응답 하는 SkiaSharp 그래픽 작성
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 67c4330d8e446a407dec7792fe5f40cdd9d23c22
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="integrating-with-xamarinforms"></a>Xamarin.Forms를 사용한 통합

_터치 및 Xamarin.Forms 요소에 응답 하는 SkiaSharp 그래픽 작성_

SkiaSharp 그래픽 여러 가지 방법으로 Xamarin.Forms의 나머지와 통합할 수 있습니다. SkiaSharp 캔버스 및 Xamarin.Forms 요소가 동일한 페이지에 SkiaSharp 캔버스 맨 위에 위치에도 Xamarin.Forms 요소를 결합할 수 있습니다.

![](integration-images/integrationexample.png "슬라이더와 색을 선택 하면")

Xamarin.Forms에 대화형 SkiaSharp 그래픽 만들기를 다른 하나는 터치 합니다.
두 번째 페이지에는 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 프로그램은 조직이 권한을 부여 받은 **토글 채우기 탭**합니다. 단순 원을 두 가지 방법으로 그릴 &mdash; 채우기 없이 및 채우기가 적용 된 &mdash; 탭으로 전환 합니다. [ `TapToggleFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) 클래스는 사용자 입력에 대 한 응답으로 SkiaSharp 그래픽을 변경 하는 방법을 보여 줍니다.

이 페이지에 대 한는 `SKCanvasView` 클래스 인스턴스화되고는 [TapToggleFill.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) 또한 Xamarin.Forms는 설정 된 파일을 [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) 보기:

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

공지는 `skia` XML 네임 스페이스 선언 합니다.

`Tapped` 에 대 한 처리기는 `TapGestureRecognizer` 개체 호출 하는 부울 필드의 값을 단순히 설정/해제는 [ `InvalidateSurface` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface()/) 방식의 `SKCanvasView`:

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

에 대 한 호출 `InvalidateSurface` 한 호출을 효과적으로 생성 된 `PaintSurface` 처리기를 사용 하는 `showFill` 을 채우거 나 원 채우지 필드:

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

`StrokeWidth` 속성이 차이 강조 하기 위해 50으로 설정 되어 있습니다. 또한 내부 만들려면 먼저 및 윤곽선 전체 선 두께 볼 수 있습니다. 기본적으로 그래픽 그림의 뒷부분에 나오는 그려지는 `PaintSurface` 이벤트 처리기를가 려 서는 처리기의 앞부분에 나오는 그린 합니다.

**색 탐색** 페이지 통합 하는 방법을 또한 SkiaSharp 그래픽 다른 Xamarin.Forms 요소와 보여 주고도 SkiaSharp에 색을 정의 하기 위한 두 가지 대체 방법 간의 차이점을 보여 줍니다. 정적 [ `SKColor.FromHsl` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsl/p/System.Single/System.Single/System.Single/System.Byte/) 메서드 만듭니다는 `SKColor` 색상-채도-밝기 모델을 기반으로 하는 값:

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

정적 [ `SKColor.FromHsv` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsv/p/System.Single/System.Single/System.Single/System.Byte/) 메서드 만듭니다는 `SKColor` 비슷한 색상-채도 값 모델을 기반으로 하는 값:

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

두 경우 모두는 `h` 인수 범위는 0에서 360 사이입니다. `s`, `l`, 및 `v` 인수 0에서 100 사이입니다. `a` (알파 또는 불투명도) 인수 범위는 0에서 255입니다.

[ **ColorExplorePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) 파일에서는 두 개의 `SKCanvasView` 개체에 `StackLayout` 나란히 있는 `Slider` 및 `Label` HSL을 선택 하는 데 사용할 수 있는 보기 및 HSV 색상 값:

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

두 `SKCanvasView` 단일 셀에 있는 요소가 `Grid` 와 `Label` 결과 RGB 색상 값을 표시 하기 위한 맨 위에 있는 것입니다.

[ **ColorExplorePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) 코드 숨김 파일은 상대적으로 간단 합니다. 공유 `ValueChanged` 3 개에 대 한 처리기 `Slider` 요소 단순히 모두 무효화 `SKCanvasView` 요소입니다. `PaintSurface` 처리기도 표시 된 색으로 캔버스의 선택을 취소는 `Slider` 요소도 설정 하 고는 `Label` 위쪽에 있는 것은 `SKCanvasView` 요소:

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

HSL와 HSV 색 모델에서는 색상 값 범위는 0에서 360 하 고 색의 색상을 나타내는 주요를 나타냅니다. 무지개 색의 기존 색입니다: 빨강, 주황, 노랑, 녹색, 파란색, indigo, 보라, 및을 빨간색 원 안에 백 합니다.

HSL 모델의 밝기는 0 값은 검은색으로 항상 및 100이 고 값은 항상 흰색입니다. 채도 값이 0 이면 밝기 값 0과 100 사이의 회색 음영 인 됩니다. 채도 늘리는 더 많은 색을 추가 합니다. 순수 색 (임 255, 0이 고는 세 번째 0에서 255 까지의 같은 다른 같은 하나의 구성 요소를 사용 하 여 RGB 값)은 100 채도 및 밝기는 50 때 발생 합니다.

HSV 모델에서 순수 색 채도 값은 100 때 발생 합니다. 값이 0 다른 설정에 관계 없이 색은 검정입니다. 0과 값의 범위는 0에서 100 채도 회색조 발생 합니다.

하지만 두 모델에 대해 이해할 수 있도록 하는 가장 좋은 방법은 이러한 실험:

[![](integration-images/colorexplore-large.png "색 탐색 페이지의 삼중 스크린샷")](integration-images/colorexplore-small.png#lightbox "색 탐색 페이지의 삼중 스크린 샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
