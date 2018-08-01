---
title: SkiaSharp 시에서 경로 작업
description: 이 문서는 샘플 코드와 함께 이러한 부하 분산 방식이 고 선 그리기 및 채우기, 사용할 경로 사용할 수 있는 다양 한 SkiaSharp 경로 효과 설명 합니다.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: charlespetzold
ms.author: chape
ms.date: 07/29/2017
ms.openlocfilehash: 2071a2fb140d0e9c78d4c86d6aa70d3606dc1f98
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244112"
---
# <a name="path-effects-in-skiasharp"></a>SkiaSharp 시에서 경로 작업

_경로 따라 그려지는 고 채움으로써에 사용할 수 있는 다양 한 경로 효과 검색_

A *경로 효과* 의 인스턴스가 [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) 8 개의 정적 중 하나를 사용 하 여 만든 클래스 `Create` 메서드. `SKPathEffect` 개체가 다음으로 설정 되 고 [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) 속성의는 `SKPaint` 작은 복제 경로 있는 줄을 선 그리기 다양 한 예를 들어 흥미로운 효과 대 한 개체:

![](effects-images/patheffectsample.png "연결 된 체인 샘플")

경로 효과 수행할 수 있습니다.

- 점, 대시 있는 줄을 선
- 모든 채워진된 경로 있는 줄을 선
- 해치 줄이 포함 된 영역 채우기
- 바둑판식으로 배열 된 경로와 영역 채우기
- 날카로운 모퉁이가 둥근 확인
- 임의 "지터" 선 및 곡선을 추가 합니다.

또한 두 개 이상의 경로 효과 결합할 수 있습니다.

또한이 문서에 사용 하는 방법을 보여 줍니다는 `GetFillPath` 방식의 `SKPaint` 하나의 경로 속성을 적용 하 여 다른 경로로 변환할 수 `SKPaint`를 포함 하 여 `StrokeWidth` 및 `PathEffect`합니다. 따라서 경로 다른 패스의 개요를 가져오는 등의 몇 가지 흥미로운 기술 됩니다. `GetFillPath` 경로 효과 관련 하 여 도움이 됩니다.

## <a name="dots-and-dashes"></a>점, 대시

사용은 [ `PathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) 메서드는 문서에 설명 된 [ **점선 및 대시**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)합니다. 메서드의 첫 번째 인수가 짝수 대시의 길이 및 대시 사이 간격의 길이 교대로 두 개 이상의 값의 수를 포함 하는 배열:

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

이러한 값은 *하지* 스트로크 너비를 기준으로 합니다. 스트로크 너비는 10, 하려는 줄 정사각형 대시와 사각형으로 구성 하는 경우 설정 하는 예를 들어는 `intervals` 배열을 {10, 10}. `phase` 인수 대시 패턴의 줄 시작 되는 위치를 나타냅니다. 이 예제에서는 정사각형 간격으로 시작 하는 줄을 하려는 경우 설정 `phase` 10입니다.

대시의 끝의 영향을 받는 `StrokeCap` 속성 `SKPaint`합니다. 넓은 선 두께 대 한 것은 매우 일반적으로이 속성을 설정 하려면 `SKStrokeCap.Round` 대시의 끝을 반올림 합니다. 이 경우의 값에는 `intervals` 배열 do *하지* 은 순환 점 0의 너비를 지정 해야 함을 의미 하는 반올림에서 발생 하는 추가 길이 포함 합니다. 10의 스트로크 너비에 대 한 선을 원형 점와 같은 지름의 점 사이 간격이 발생을 만들려면 사용는 `intervals` 배열을 {0, 20}.

**점으로 구분 된 텍스트에 애니메이션을** 페이지는 비슷합니다는 **설명 텍스트** 문서에서 설명 하는 페이지 [ **통합 텍스트와 그래픽** ](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md) 에 설정 하 여 텍스트 문자를 설명 표시 되는 것은 `Style` 속성은 `SKPaint` 개체를 `SKPaintStyle.Stroke`합니다. 또한 **점으로 구분 된 텍스트에 애니메이션을** 사용 하 여 `SKPathEffect.CreateDash` 점선된 모양을 설명이 제공 하 고 프로그램 또한 애니메이션의 `phase` 의 인수는 `SKPathEffect.CreateDash` 텍스트 주위의 이동 하는 것 처럼 보일 점 만드는 메서드와 알림이 문자 수입니다. 가로 모드에서 페이지는 다음과 같습니다.

[![](effects-images/animateddottedtext-small.png "애니메이션을 점으로 구분 된 텍스트 페이지의 삼중 스크린샷")](effects-images/animateddottedtext-large.png#lightbox "점으로 구분 된 텍스트에 애니메이션을 페이지의 삼중 스크린 샷")

[ `AnimatedDottedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) 클래스 일부 상수를 정의 하 여 시작 하 고 재정의 `OnAppearing` 및 `OnDisappearing` 애니메이션에 대 한 메서드:

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    const string text = "DOTTED";
    const float strokeWidth = 10;
    static readonly float[] dashArray = { 0, 2 * strokeWidth };

    SKCanvasView canvasView;
    bool pageIsActive;

    public AnimatedDottedTextPage()
    {
        Title = "Animated Dotted Text";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;

        Device.StartTimer(TimeSpan.FromSeconds(1f / 60), () =>
        {
            canvasView.InvalidateSurface();
            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`PaintSurface` 처리기 먼저 만듭니다는 `SKPaint` 텍스트를 표시 하는 개체입니다. `TextSize` 속성 화면 너비에 따라 조정 됩니다.

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create an SKPaint object to display the text
        using (SKPaint textPaint = new SKPaint
            {
                Style = SKPaintStyle.Stroke,
                StrokeWidth = strokeWidth,
                StrokeCap = SKStrokeCap.Round,
                Color = SKColors.Blue,
            })
        {
            // Adjust TextSize property so text is 95% of screen width
            float textWidth = textPaint.MeasureText(text);
            textPaint.TextSize *= 0.95f * info.Width / textWidth;

            // Find the text bounds
            SKRect textBounds = new SKRect();
            textPaint.MeasureText(text, ref textBounds);

            // Calculate offsets to center the text on the screen
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            // Animate the phase; t is 0 to 1 every second
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 1 / 1);
            float phase = -t * 2 * strokeWidth;

            // Create dotted line effect based on dash array and phase
            using (SKPathEffect dashEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                // Set it to the paint object
                textPaint.PathEffect = dashEffect;

                // And draw the text
                canvas.DrawText(text, xText, yText, textPaint);
            }
        }
    }
}
```

메서드의 끝까지는 `SKPathEffect.CreateDash` 메서드를 사용 하 여 호출 됩니다는 `dashArray` 는 필드와의 애니메이션으로 정의 된 `phase` 값입니다. `SKPathEffect` 인스턴스로 설정 됩니다는 `PathEffect` 의 속성은 `SKPaint` 텍스트를 표시 하는 개체입니다.

설정할 수 있습니다는 `SKPathEffect` 개체는 `SKPaint` 텍스트를 측정 하 고 페이지 가운데 맞추기 전의 개체입니다. 그러나이 경우 애니메이션 점과 대시 약간 달라질 렌더링된 텍스트의 크기에 시키며, 텍스트를 약간 진동 경향이. (시도해 보십시오!)

텍스트 문자 주위의 애니메이션된 점 원으로 한지 각 닫힌된 곡선의 특정 지점 도트 존재 내부 및 외부로 팝 할 것 처럼 보일 수도 확인할 수 있습니다. 이것이 문자 윤곽선을 정의 하는 경로 시작 되 고 끝나는 위치입니다. 경로 길이 대시 패턴 (이 경우 20 픽셀) 길이의 배수가 없으면 해당 패턴의 일부만 경로의 끝에 맞출 수 있습니다.

경로 길이 맞게 대시 패턴의 길이를 조정할 수도 있지만 되어야 하는 기술 경로 길이 결정 해야 하는 이후 문서에 설명 합니다.

**점 / 대시 Morph** 대시 울 양식 대시를 다시 결합 하는 점, 나눌 수 있도록 프로그램 애니메이션 자체 대시 패턴을 적용 합니다.

[![](effects-images/dotdashmorph-small.png "점 대시 Morph 페이지의 삼중 스크린샷")](effects-images/dotdashmorph-large.png#lightbox "점 대시 Morph 페이지의 삼중 스크린샷")

[ `DotDashMorphPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) 재정의 `OnAppearing` 및 `OnDisappearing` 이전 프로그램 하지만 클래스를 정의 하는 같은 방법으로 메서드는 `SKPaint` 필드로 개체:

```csharp
public class DotDashMorphPage : ContentPage
{
    const float strokeWidth = 30;
    static readonly float[] dashArray = new float[4];

    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint ellipsePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = strokeWidth,
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create elliptical path
        using (SKPath ellipsePath = new SKPath())
        {
            ellipsePath.AddOval(new SKRect(50, 50, info.Width - 50, info.Height - 50));

            // Create animated path effect
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 3 / 3);
            float phase = 0;

            if (t < 0.25f)  // 1, 0, 1, 2 --> 0, 2, 0, 2
            {
                float tsub = 4 * t;
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2 * tsub;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2;
            }
            else if (t < 0.5f)  // 0, 2, 0, 2 --> 1, 2, 1, 0
            {
                float tsub = 4 * (t - 0.25f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2 * (1 - tsub);
                phase = strokeWidth * tsub;
            }
            else if (t < 0.75f) // 1, 2, 1, 0 --> 0, 2, 0, 2
            {
                float tsub = 4 * (t - 0.5f);
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2 * tsub;
                phase = strokeWidth * (1 - tsub);
            }
            else               // 0, 2, 0, 2 --> 1, 0, 1, 2
            {
                float tsub = 4 * (t - 0.75f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2 * (1 - tsub);
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2;
            }

            using (SKPathEffect pathEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                ellipsePaint.PathEffect = pathEffect;
                canvas.DrawPath(ellipsePath, ellipsePaint);
            }
        }
    }
}
```

`PaintSurface` 처리기는 페이지의 크기에 따라 타원형 경로 만들고 설정 하는 코드는 긴 작업 실행의 `dashArray` 및 `phase` 변수입니다. 애니메이션 효과 준 변수로 `t` 범위는 0에서 1는 `if` 시간에 4 개 분기 및 이러한 분기의 각 블록 어셈블할 `tsub` 도 범위는 0에서 1입니다. 맨 끝에서 프로그램을 만듭니다는 `SKPathEffect` 로 설정 하 고는 `SKPaint` 그리기에 대 한 개체입니다.

## <a name="from-path-to-path"></a>경로에 대 한 경로에서

[ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) 방식의 `SKPaint` 의 설정에 따라 다른 파티션으로 하나의 경로 설정의 `SKPaint` 개체입니다. 이 과정을 보려면 대체는 `canvas.DrawPath` 다음 코드를 사용 하 여 이전 프로그램에서 호출:

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

이 새 코드는 `GetFillPath` 변환 호출는 `ellipsePath` (변수인 방금 타원)에 `newPath`, 함께 표시 됩니다는 `newPaint`합니다. `newPaint` 개체가 모든 기본 속성 설정이 만들어집니다 점을 제외 하 고는 `Style` 속성을 기반으로 설정 되는 부울 값을 반환할 `GetFillPath`합니다.

시각적 개체에 설정 된 색을 제외 하 고 동일 `ellipsePaint` 아닌 `newPaint`합니다. 에 정의 된 단순한 타원 대신 `ellipsePath`, `newPath` 일련의 점과 대시를 정의 하는 다양 한 경로 윤곽선을 포함 합니다. 이 다양 한 속성을 적용 한 결과 `ellipsePaint` - `StrokeWidth`, `StrokeCap`, 및 `PathEffect` -를 `ellipsePath` 고 그 결과 경로에 넣는 `newPath`합니다. `GetFillPath` 메서드는 반환 나타내는 부울 값을 대상 경로 입력할 수 있는지 여부 않으면 반환 값은이 예에서 `true` 경로 채우기 위한 합니다.

바꾸려고는 `Style` 에서 설정 `newPaint` 를 `SKPaintStyle.Stroke` 1 픽셀 너비 선으로 설명 된 개별 경로 윤곽선을 볼 수 있습니다.

## <a name="stroking-with-a-path"></a>경로와 선 그리기

[ `SKPathEffect.Create1DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create1DPath/p/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPath1DPathEffectStyle/) 메서드는 개념적으로 비슷합니다 `SKPathEffect.CreateDash` 제외 하 고 대시와 간격의 패턴 아닌 경로 지정 합니다. 이 경로 스트로크 줄 또는 곡선을 여러 번 복제 됩니다.

사용되는 구문은 다음과 같습니다.

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

> [!IMPORTANT]
> 주의: 오버 로드는 `Create1DPath` 형식의 열거형 인수로 정의 된 `SkPath1DPathEffect` 소문자 'k'으로 이 이름은 오류 및 있는지 열거형 및 메서드는 사용 되지 않지만 사용자 코드의 일부로 사용 되지 않는 메서드에 대 한 매우 쉽습니다 하며 정확 하 게 확인할 수 어렵습니다 잘못 된 결과적으로 합니다.

일반적으로 전달 하는 경로 `Create1DPath` 작고 (0, 0) 점을 가운데 맞춤 됩니다. `advance` 줄에 복제 되는 경로 매개 변수 경로 센터에서의 거리를 나타냅니다. 일반적으로이 인수는 경로의 대략적인 너비를 설정 합니다. `phase` 것와 동일한 역할 여기에서 수행 합니다. 인수는 `CreateDash` 메서드.

[ `SKPath1DPathEffectStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath1DPathEffectStyle/) 에 세 명의 멤버가 있습니다.

- [`Translate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Translate/)
- [`Rotate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Rotate/)
- [`Morph`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Morph/)

`Translate` 멤버의 경로를 선 또는 곡선을 따라 복제 되는 동일한 방향에 유지 하면 됩니다. 에 대 한 `Rotate`, 경로 접선 곡선을 기준으로 회전 합니다. 경로 가로선에 대 한 일반 방향이 있습니다. `Morph` 유사한 `Rotate` 제외 하 고 경로 자체 스트로크 되 고 선의 곡률에 맞게 곡선도 합니다.

**1 D 경로 효과** 페이지에서는 이러한 세 가지 옵션을 보여 줍니다. [ **OneDimensionalPathEffectPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml) 파일에 해당 하는 열거형의 세 멤버 3 개 항목을 포함 하는 선택을 정의:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.OneDimensionalPathEffectPage"
             Title="1D Path Effect">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="effectStylePicker"
                Title="Effect Style"
                Grid.Row="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Translate</x:String>
                <x:String>Rotate</x:String>
                <x:String>Morph</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1" />
    </Grid>
</ContentPage>
```

[ **OneDimensionalPathEffectPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs) 코드 숨김 파일 3 개 정의 `SKPathEffect` 필드로 개체입니다. 이러한 모두 사용 하 여 만들어지며 `SKPathEffect.Create1DPath` 와 `SKPath` 개체를 사용 하 여 만든 `SKPath.ParseSvgPathData`합니다. 첫 번째는 간단한 상자, 두 번째는 다이아몬드 모양 및 세 번째 사각형입니다. 이러한 세 가지 효과 스타일을 보여 주기 위해 사용 됩니다.

```csharp
public partial class OneDimensionalPathEffectPage : ContentPage
{
    SKPathEffect translatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 -10 L 10 -10, 10 10, -10 10 Z"),
                                  24, 0, SKPath1DPathEffectStyle.Translate);

    SKPathEffect rotatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 0 L 0 -10, 10 0, 0 10 Z"),
                                  20, 0, SKPath1DPathEffectStyle.Rotate);

    SKPathEffect morphPathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -25 -10 L 25 -10, 25 10, -25 10 Z"),
                                  55, 0, SKPath1DPathEffectStyle.Morph);

    SKPaint pathPaint = new SKPaint
    {
        Color = SKColors.Blue
    };

    public OneDimensionalPathEffectPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath path = new SKPath())
        {
            path.MoveTo(new SKPoint(0, 0));
            path.CubicTo(new SKPoint(2 * info.Width, info.Height),
                         new SKPoint(-info.Width, info.Height),
                         new SKPoint(info.Width, 0));

            switch (effectStylePicker.Items[effectStylePicker.SelectedIndex])
            {
                case "Translate":
                    pathPaint.PathEffect = translatePathEffect;
                    break;

                case "Rotate":
                    pathPaint.PathEffect = rotatePathEffect;
                    break;

                case "Morph":
                    pathPaint.PathEffect = morphPathEffect;
                    break;
            }

            canvas.DrawPath(path, pathPaint);
        }
    }
}
```

`PaintSurface` 처리기 자체 반복 여부를 확인 하 고 선택기에 액세스 하는 베 지 어 곡선을 만듭니다 `PathEffect` 선을를 사용 해야 합니다. 세 가지 옵션- `Translate`, `Rotate`, 및 `Morph` -왼쪽에서 오른쪽으로 표시 됩니다.

[![](effects-images/1dpatheffect-small.png "1d 경로 효과 페이지의 삼중 스크린샷")](effects-images/1dpatheffect-large.png#lightbox "1d 경로 효과 페이지의 삼중 스크린 샷")

에 지정 된 경로 `SKPathEffect.Create1DPath` 메서드는 항상 채워집니다. 에 지정 된 경로 `DrawPath` 경우 메서드를 그릴지 항상는 `SKPaint` 개체에 해당 `PathEffect` 속성이 1 D 경로 효과로 설정 합니다. 다음에 유의 `pathPaint` 개체에 `Style` 기본값은 일반적으로 설정 `Fill`, 경로 관계 없이 스트로크 하지만 합니다.

사용 하는 상자는 `Translate` 예제는 정사각형, 20 픽셀이 및 `advance` 인수를 24로 설정 합니다. 이러한 차이 수평 또는 수직, 줄 대략 되 었 상자의 대각선은 28.3 픽셀 때문에 줄 대각선 경우 상자 약간 겹칠 경우 상자 사이의 간격이 발생 합니다.

다이아몬드 모양에는 `Rotate` 예제에서는 너비가 20 픽셀 이기도 합니다. `advance` 는 지점이 다이아몬드 선의 곡률 함께 회전 되는지 그린 계속 되도록 20으로 설정 합니다.

에 있는 사각형 셰이프에 `Morph` 예제는 50 픽셀 너비는 `advance` 베 지 어 곡선 주위 구부러진 작은 사각형 간의 간격이 조화 55 설정 합니다.

경우는 `advance` 경로 ()의 크기 보다 작으면 인수는 다음 복제 된 경로 겹칠 수 있습니다. 이 인해 몇 가지 흥미로운 효과 발생할 수 있습니다. **연결 된 체인** 중단는 catenary 독특한 모양에 연결 된 체인, 비슷한 것 원 중첩 페이지에 표시 됩니다.

[![](effects-images/linkedchain-small.png "연결 된 체인 페이지의 삼중 스크린샷")](effects-images/linkedchain-large.png#lightbox "연결 된 체인 페이지의 삼중 스크린샷")

매우 유사 하 고 실제로 원 아닙니다 있는지 표시 합니다. 체인의 각 링크에는 두 개의 원호, 크기 및 인접 한 링크를 사용 하 여 연결할 처럼 보이는 하므로 위치입니다.

체인 또는 가중치 균일 한 분포의 케이블은 catenary의 형태로 중단 됩니다. 반전 된 catenary의 형태로 작성 하는 아키텍처는 아키텍처의 가중치에서 압력 똑같이 배분에서 유리 합니다. catenary에 단순 수학 설명:

y는 · = cosh(x / a)

*cosh* 쌍 곡 코사인 함수입니다. 에 대 한 *x* 0 *cosh* 0이 고 *y* 같음 *는*합니다. catenary의 중심입니다. 와 같은 *코사인* 함수 *cosh* 라고 *도*, 즉 *cosh(–x)* equals *cosh(x)*, 양수 또는 음수 인수 증가 대 한 값이 증가 하 고 있습니다. 이러한 값은 catenary의 양쪽을 형성 하는 곡선에 설명 합니다.

적절 한 값을 찾는 *는* catenary 휴대폰의 페이지의 크기에 맞게 않습니다 직접 계산 합니다. 경우 *w* 및 *h* 의 최적 값의 사각형의 높이 너비는 *는* 은 다음 수식을 충족 합니다.

cosh (w/2/a) = 1 + h / a

다음 메서드는 [ `LinkedChainPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) 클래스는 두 식 왼쪽 및 오른쪽으로 등호를 참조 하 여 해당 같음 통합 `left` 및 `right`합니다. 값이 작으면 *는*, `left` 보다 크면 `right`;의 값이 크면 *는*, `left` 는 보다 작은 `right`합니다. `while` 루프에서의 최적의 값에 좁힙니다 *는*:

```csharp
float FindOptimumA(float width, float height)
{
    Func<float, float> left = (float a) => (float)Math.Cosh(width / 2 / a);
    Func<float, float> right = (float a) => 1 + height / a;

    float gtA = 1;         // starting value for left > right
    float ltA = 10000;     // starting value for left < right

    while (Math.Abs(gtA - ltA) > 0.1f)
    {
        float avgA = (gtA + ltA) / 2;

        if (left(avgA) < right(avgA))
        {
            ltA = avgA;
        }
        else
        {
            gtA = avgA;
        }
    }

    return (gtA + ltA) / 2;
}
```

`SKPath` 클래스의 생성자 및 결과에서 만든 링크에 대 한 개체 `SKPathEffect` 개체가 다음으로 설정 되는 `PathEffect` 속성은 `SKPaint` 필드로 저장 된 개체:

```csharp
public class LinkedChainPage : ContentPage
{
    const float linkRadius = 30;
    const float linkThickness = 5;

    Func<float, float, float> catenary = (float a, float x) => (float)(a * Math.Cosh(x / a));

    SKPaint linksPaint = new SKPaint
    {
        Color = SKColors.Silver
    };

    public LinkedChainPage()
    {
        Title = "Linked Chain";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the individual links
        SKRect outer = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
        SKRect inner = outer;
        inner.Inflate(-linkThickness, -linkThickness);

        using (SKPath linkPath = new SKPath())
        {
            linkPath.AddArc(outer, 55, 160);
            linkPath.ArcTo(inner, 215, -160, false);
            linkPath.Close();

            linkPath.AddArc(outer, 235, 160);
            linkPath.ArcTo(inner, 395, -160, false);
            linkPath.Close();

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(linkPath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);
        }
    }
    ...
}
```

주요 작업은 `PaintSurface` 처리기 catenary 자체에 대 한 경로 만드는 것입니다. 최적의 확인 한 후 *는* 저장 하는 방식에 `optA` 변수에 해야 창의 위쪽에서 오프셋을 계산 합니다. 컬렉션에 누적 될 수 있는 다음 `SKPoint` catenary에 대 한 값을 한 경로로 설정 하 고 이전에 만든 사용 하 여 경로 그리는 `SKPaint` 개체:

```csharp
public class LinkedChainPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        // Width and height of catenary
        int width = info.Width;
        float height = info.Height - linkRadius;

        // Find the optimum 'a' for this width and height
        float optA = FindOptimumA(width, height);

        // Calculate the vertical offset for that value of 'a'
        float yOffset = catenary(optA, -width / 2);

        // Create a path for the catenary
        SKPoint[] points = new SKPoint[width];

        for (int x = 0; x < width; x++)
        {
            points[x] = new SKPoint(x, yOffset - catenary(optA, x - width / 2));
        }

        using (SKPath path = new SKPath())
        {
            path.AddPoly(points, false);

            // And render that path with the linksPaint object
            canvas.DrawPath(path, linksPaint);
        }
    }
    ...
}
```

이 프로그램에 사용 되는 경로 정의 `Create1DPath` 있어야 해당 (0, 0) 중심을 지정 합니다. 이 방법은 적절 한 때문에 (0, 0) 패스의 줄 또는 표시 되는 곡선에 맞게 정렬 됩니다. 그러나 사용할 수 있습니다 심화 되지 않은 (0, 0) 일부 특수 효과 대 한 지점입니다.

**컨베이어 벨트** 페이지 곡선된 위쪽 및 아래쪽 창의 크기를이 크기는 장방형 컨베이어 벨트와 비슷한 경로 만듭니다. 간단한을 사용 하 여 해당 경로 스트로크 `SKPaint` 너비가 20 픽셀이 고 색이 지정 된 회색 개체를 다른 다시 스트로크 `SKPaint` 개체는 `SKPathEffect` 거의 버킷와 비슷한 경로 참조 하는 개체:

[![](effects-images/conveyorbelt-small.png "컨베이어 벨트 페이지의 삼중 스크린샷")](effects-images/conveyorbelt-large.png#lightbox "컨베이어 벨트 페이지의 삼중 스크린샷")

(0, 0)의 버킷 경로 이므로 핸들, 그럴 경우에는 `phase` 인수 애니메이션 효과가 적용 되어, 버킷의 아마도 scooping 물 아래쪽에서 위쪽 및 맨 위에 있는 덤프 아웃 컨베이어 벨트를 중심으로 하는 것 같습니다.

[ `ConveyorBeltPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs) 클래스 구현 재정의 애니메이션은 `OnAppearing` 및 `OnDisappearing` 메서드. 집합에 대 한 경로 페이지의 생성자에 정의 됩니다.

```csharp
public class ConveyorBeltPage : ContentPage
{
    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint conveyerPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 20,
        Color = SKColors.DarkGray
    };

    SKPath bucketPath = new SKPath();

    SKPaint bucketsPaint = new SKPaint
    {
        Color = SKColors.BurlyWood,
    };

    public ConveyorBeltPage()
    {
        Title = "Conveyor Belt";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the bucket starting with the handle
        bucketPath.AddRect(new SKRect(-5, -3, 25, 3));

        // Sides
        bucketPath.AddRoundedRect(new SKRect(25, -19, 27, 18), 10, 10,
                                  SKPathDirection.CounterClockwise);
        bucketPath.AddRoundedRect(new SKRect(63, -19, 65, 18), 10, 10,
                                  SKPathDirection.CounterClockwise);

        // Five slats
        for (int i = 0; i < 5; i++)
        {
            bucketPath.MoveTo(25, -19 + 8 * i);
            bucketPath.LineTo(25, -13 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                             SKPathDirection.CounterClockwise, 65, -13 + 8 * i);
            bucketPath.LineTo(65, -19 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                             SKPathDirection.Clockwise, 25, -19 + 8 * i);
            bucketPath.Close();
        }

        // Arc to suggest the hidden side
        bucketPath.MoveTo(25, -17);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                         SKPathDirection.Clockwise, 65, -17);
        bucketPath.LineTo(65, -19);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                         SKPathDirection.CounterClockwise, 25, -19);
        bucketPath.Close();

        // Make it a little bigger and correct the orientation
        bucketPath.Transform(SKMatrix.MakeScale(-2, 2));
        bucketPath.Transform(SKMatrix.MakeRotationDegrees(90));
    }
    ...
```

버킷 생성 코드를 약간 크게 버킷을 확인 하 고 옆으로 설정 하는 두 개의 변환을 완료 합니다. 이러한 변환 적용 앞의 코드에 있는 모든 좌표를 조정 하는 보다 쉽게 했습니다.

`PaintSurface` 처리기 자체가 컨베이어 벨트에 대 한 경로 정의 하 여 시작 합니다. 이 단순히 한 쌍의 선 및 20 픽셀 너비 진한 회색 선으로 그려진 세미콜론 원의 쌍:

```csharp
public class ConveyorBeltPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        float width = info.Width / 3;
        float verticalMargin = width / 2 + 150;

        using (SKPath conveyerPath = new SKPath())
        {
            // Straight verticals capped by semicircles on top and bottom
            conveyerPath.MoveTo(width, verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large,
                               SKPathDirection.Clockwise, 2 * width, verticalMargin);
            conveyerPath.LineTo(2 * width, info.Height - verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large,
                               SKPathDirection.Clockwise, width, info.Height - verticalMargin);
            conveyerPath.Close();

            // Draw the conveyor belt itself
            canvas.DrawPath(conveyerPath, conveyerPaint);

            // Calculate spacing based on length of conveyer path
            float length = 2 * (info.Height - 2 * verticalMargin) +
                           2 * ((float)Math.PI * width / 2);

            // Value will be somewhere around 200
            float spacing = length / (float)Math.Round(length / 200);

            // Now animate the phase; t is 0 to 1 every 2 seconds
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 2 / 2);
            float phase = -t * spacing;

            // Create the buckets PathEffect
            using (SKPathEffect bucketsPathEffect =
                        SKPathEffect.Create1DPath(bucketPath, spacing, phase,
                                                  SKPath1DPathEffectStyle.Rotate))
            {
                // Set it to the Paint object and draw the path again
                bucketsPaint.PathEffect = bucketsPathEffect;
                canvas.DrawPath(conveyerPath, bucketsPaint);
            }
        }
    }
}
```

그리기 컨베이어 벨트에 대 한 논리 가로 모드에서 작동 하지 않습니다.

버킷의는 컨베이어 벨트에 200 픽셀에 대 한 배치 해야 합니다. 그러나 컨베이어 벨트가 되지 않았을 수로 한 long, 200 픽셀의 배수가 `phase` 의 인수 `SKPathEffect.Create1DPath` 은 애니메이션 효과가 적용 버킷 팝업 내부 / 외부로 존재 합니다.

이러한 이유로 프로그램 먼저 계산 라는 값을 `length` 컨베이어 벨트의 길이입니다. 컨베이어 벨트로 구성 된 직선 및 세미콜론 원 이므로 간단한 계산 합니다. 그런 다음 버킷 수가 나눈 `length` 200 여 합니다. 이것은 가장 가까운 정수로 반올림 하 고 나뉩니다 번호가 다음 인지 `length`합니다. 결과 버킷 수는 정수 계열에 대 한 간격입니다. `phase` 인수는 단순히 소수 부분입니다.

## <a name="from-path-to-path-again"></a>다시 경로에 대 한 경로

맨 아래에 `DrawSurface` 처리기에 **컨베이어 벨트**, 주석으로 처리는 `canvas.DrawPath` 호출 하 고 다음 코드로 바꿉니다.

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

이전 예제와 마찬가지로 `GetFillPath`, 결과 색 제외 하 고 동일한 지 확인할 수 있습니다. 실행 한 후 `GetFillPath`, `newPath` 버킷 경로의 여러 복사본을 포함 하는 개체, 각각에 배치 하는 동일한 스폿 애니메이션에 메서드를 호출할 때 해당 배치 된다고 합니다.

## <a name="hatching-an-area"></a>영역 해칭

[ `SKPathEffect.Create2DLines` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DLine/p/System.Single/SkiaSharp.SKMatrix/) 채워지는 라고도 하는 병렬 줄 영역 *해칭 선을*합니다. 메서드는 다음 구문을 가집니다.

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

`width` 인수 해치 줄의 스트로크 너비를 지정 합니다. `matrix` 매개 변수는 크기 조정 및 선택적 회전의 조합입니다. 배율 인수 Skia 해치 줄 간격을 사용 하는 픽셀 단위로를 나타냅니다. 줄 구분은 뺀 배율 인수는 `width` 인수입니다. 배율 인수 보다 작거나 같은 경우는 `width` 값 해치 줄 사이 공백이 없어야 됩니다 및 채울 영역 표시 됩니다. 가로 및 세로 크기 조정에 대 한 동일한 값을 지정 합니다.

기본적으로 해치 선은 가로입니다. 경우는 `matrix` 매개 변수에 포함 회전, 해치 선은 시계 방향으로 회전 합니다.

**채우기 해치** 페이지에서는이 경로 효과 보여 줍니다. [ `HatchFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs) 더 나타내는 배율 요소와 함께 3 픽셀 너비와 가로 평행선에 대 한 첫 번째 간격을 더 많이 떨어져 있는 6 픽셀, 클래스 필드로 3 경로 효과 정의 합니다. 따라서 3 픽셀은이 줄 구분이 됩니다. 두 번째 경로 효과 너비가 6 픽셀인 세로 평행선 간격을 더 많이 떨어져 있는 (따라서 분리는 18 픽셀은) 24 픽셀에 대 한 및 빗금 줄 12 넓은 공백이 포함 된 36 픽셀 더 많이 떨어져 있는 세 번째입니다.

```csharp
public class HatchFillPage : ContentPage
{
    SKPaint fillPaint = new SKPaint();

    SKPathEffect horzLinesPath = SKPathEffect.Create2DLine(3, SKMatrix.MakeScale(6, 6));

    SKPathEffect vertLinesPath = SKPathEffect.Create2DLine(6,
        Multiply(SKMatrix.MakeRotationDegrees(90), SKMatrix.MakeScale(24, 24)));

    SKPathEffect diagLinesPath = SKPathEffect.Create2DLine(12,
        Multiply(SKMatrix.MakeScale(36, 36), SKMatrix.MakeRotationDegrees(45)));

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

행렬 확인 `Multiply` 메서드. 가로 및 세로 배율 동일 하기 때문에 크기 조정 및 회전 매트릭스 곱할 순서는 중요 하지 않습니다.

`PaintSurface` 처리기는 이러한 세 경로 효과와 함께에서 세 가지 다른 색상으로 사용 하 여 `fillPaint` 페이지에 맞게 크기 조정 된 모퉁이가 둥근된 사각형을 채웁니다. `Style` 에서 속성을 설정 `fillPaint` 설정은 무시 됩니다; 때는 `SKPaint` 개체에서 만든 경로 효과 포함 `SKPathEffect.Create2DLine`, 영역에 관계 없이 채워집니다.

```csharp
public class HatchFillPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath roundRectPath = new SKPath())
        {
            // Create a path
            roundRectPath.AddRoundedRect(
                new SKRect(50, 50, info.Width - 50, info.Height - 50), 100, 100);

            // Horizontal hatch marks
            fillPaint.PathEffect = horzLinesPath;
            fillPaint.Color = SKColors.Red;
            canvas.DrawPath(roundRectPath, fillPaint);

            // Vertical hatch marks
            fillPaint.PathEffect = vertLinesPath;
            fillPaint.Color = SKColors.Blue;
            canvas.DrawPath(roundRectPath, fillPaint);

            // Diagonal hatch marks -- use clipping
            fillPaint.PathEffect = diagLinesPath;
            fillPaint.Color = SKColors.Green;

            canvas.Save();
            canvas.ClipPath(roundRectPath);
            canvas.DrawRect(new SKRect(0, 0, info.Width, info.Height), fillPaint);
            canvas.Restore();

            // Outline the path
            canvas.DrawPath(roundRectPath, strokePaint);
        }
    }
    ...
}
```

신중 하 게 결과 보면 빨강 및 파랑 평행선 모퉁이가 둥근된 사각형을 정확 하 게 제한 하지 되도록 표시 됩니다. (이 분명히 기본 Skia 코드의 특성입니다.) 이 작업이 충분 하지 않으면 녹색으로 빗금 줄에 대 한 다른 접근 방식은 표시 됩니다: 모퉁이가 둥근된 사각형 오려낸 경로로 사용 되 고 전체 페이지 해치 선이 그려집니다.

`PaintSurface` 처리기가 단순히 빨강 및 파랑 해치 줄의 불일치를 확인할 수 있도록 모퉁이가 둥근된 사각형을 그리는을 호출 하 여 종료 합니다.

[![](effects-images/hatchfill-small.png "채우기 해치 페이지의 삼중 스크린샷")](effects-images/hatchfill-large.png#lightbox "해치 채우기 페이지의 삼중 스크린 샷")

Android 화면 매우 ै 그렇게: 씬 빨강 선 및 넓어짐 겉보기 빨강 선으로 통합 하기 위해 씬 공간 및 넓은 공간 스크린샷에서의 크기 조정을 발생 했습니다.

## <a name="filling-with-a-path"></a>경로으로 채우기

[ `SKPathEffect.Create2DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DPath/p/SkiaSharp.SKMatrix/SkiaSharp.SKPath/) 영역을 바둑판식 적용 가로 세로로 복제 되는 경로와 영역을 채울 수 있습니다.

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

`SKMatrix` 배율 인수는 복제 된 경로의 가로 및 세로 간격을 나타냅니다. 이 사용 하 여 경로 회전할 수 있지만 `matrix` 인수; 회전 경로 사용할 경우 경로 자체 회전를 사용 하 여는 `Transform` 정의한 메서드 `SKPath`합니다.

일반적으로 복제 된 경로 채워질 영역 대신 화면의 왼쪽 및 위쪽 가장자리에 맞춥니다. 0에서 왼쪽 및 위쪽 측면에서 가로 및 세로 오프셋을 지정 하는 배율 인수 사이의 변환 요소를 제공 하 여이 동작을 재정의할 수 있습니다.

**경로 타일 채우기** 페이지에서는이 경로 효과 보여 줍니다. 영역을 바둑판식으로 배열에 사용 된 경로에 필드로 정의 됩니다는 [ `PathFileFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) 클래스입니다. 가로 및 세로 좌표-40부터 범위 40, 즉,이 경로 80 픽셀로:

```csharp
public class PathTileFillPage : ContentPage
{
    SKPath tilePath = SKPath.ParseSvgPathData(
        "M -20 -20 L 2 -20, 2 -40, 18 -40, 18 -20, 40 -20, " +
        "40 -12, 20 -12, 20 12, 40 12, 40 40, 22 40, 22 20, " +
        "-2 20, -2 40, -20 40, -20 8, -40 8, -40 -8, -20 -8 Z");
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Red;

            using (SKPathEffect pathEffect =
                   SKPathEffect.Create2DPath(SKMatrix.MakeScale(64, 64), tilePath))
            {
                paint.PathEffect = pathEffect;

                canvas.DrawRoundRect(
                    new SKRect(50, 50, info.Width - 50, info.Height - 50),
                    100, 100, paint);
            }
        }
    }
}
```

에 `PaintSurface` 처리기는 `SKPathEffect.Create2DPath` 겹쳐 80 픽셀 정사각형 타일을 64 개로 가로 및 세로 간격을 설정 하는 호출 합니다. 다행히 경로 타일에 인접으로 충분히 meshing 퍼즐 조각, 유사 합니다.

[![](effects-images/pathtilefill-small.png "경로 타일 채우기 페이지의 삼중 스크린샷")](effects-images/pathtilefill-large.png#lightbox "경로 타일 채우기 페이지의 삼중 스크린 샷")

Android 화면에 특히 일부 왜곡을 하면 원래 스크린 샷에서 확장 됩니다.

이러한 타일 항상 표시 전체 내용과 알 자를 수 없습니다. 처음 두 가지 스크린샷입니다에서 확인할 수는 없지만 채워질 영역 모퉁이가 둥근된 사각형 임을 합니다. 이 타일을 특정 영역을 자를 하려면 클리핑 패스를 사용 합니다.

설정 해 보세요는 `Style` 의 속성은 `SKPaint` 개체를 `Stroke`, 입력 하지 않고 설명 된 각 타일을 볼 수 있습니다.

## <a name="rounding-sharp-corners"></a>날카로운 모퉁이 반올림합니다.

**반올림 Heptagon** 프로그램에 표시 되는 [ **세 가지 방법으로 호를 그릴 수** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) 문서는 탄젠트 호 7 면 그림의 지점에 있는 곡선을 하는 데 사용 합니다. **다른 반올림 Heptagon** 페이지 표시는 편이 훨씬 더 간단에서 만든 경로 효과 사용 하 여 [ `SKPathEffect.CreateCorner` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCorner/p/System.Single/) 메서드:

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

단일 인수 라는 있지만 `radius` 절반 원하는 모서리 반지름으로 설정 해야 합니다. (기본 Skia 코드의 특성입니다.)

다음은 `PaintSurface` 의 처리기는 [ `AnotherRoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs) 클래스:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);
    SKPoint[] vertices = new SKPoint[numVertices];
    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    float cornerRadius = 100;

    // Create the path
    using (SKPath path = new SKPath())
    {
        path.AddPoly(vertices, true);

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            // Set argument to half the desired corner radius!
            paint.PathEffect = SKPathEffect.CreateCorner(cornerRadius / 2);

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);

            // Uncomment DrawCircle call to verify corner radius
            float offset = cornerRadius / (float)Math.Sin(Math.PI * (numVertices - 2) / numVertices / 2);
            paint.Color = SKColors.Green;
            // canvas.DrawCircle(vertices[0].X, vertices[0].Y + offset, cornerRadius, paint);
        }
    }
}
```

이 효과 사용 하 여 선 그리기 또는 기반 채우기는 `Style` 의 속성은 `SKPaint` 개체입니다. 다음 세 플랫폼 모두에입니다.

[![](effects-images/anotherroundedheptagon-small.png "다른 반올림 Heptagon 페이지의 삼중 스크린샷")](effects-images/anotherroundedheptagon-large.png#lightbox "다른 반올림 Heptagon 페이지의 삼중 스크린샷")

이 둥근된 heptagon는 이전 프로그램에 동일 볼 수 있습니다. 더 많은 유도 해야 할 경우 모서리 반지름은 100 진정으로 대신에 지정 된 50는 `SKPathEffect.CreateCorner` 호출 수 주석 마지막 문에서 프로그램 및 참조 100 radius 원이 겹쳐 모퉁이에 있습니다.

## <a name="random-jitter"></a>임의 지터

경우에 따라 컴퓨터 그래픽의 완벽 한 직선 하지 매우 원하는 않으며 약간 임의성이 필요 합니다. 시도 해야 하는 경우에 [ `SKPathEffect.CreateDiscrete` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDiscrete/p/System.Single/System.Single/System.UInt32/) 메서드:

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

선 그리기 또는 작성에 대 한이 경로 효과 사용할 수 있습니다. 줄은 연결 된 단위로 구분-하 여 지정 된 길이 대략적인 `segLength` -하 고 다양 한 방향으로 확장 합니다. 원래 줄과에서의 편차 범위 지정 된 `deviation`합니다.

마지막 인수에는 효과 위해 사용 되는 의사 난수 시퀀스를 생성 하는 데 사용 하는 초기값입니다. 지터 효과 다른 초기값에 대 한 약간 다르게 표시 됩니다. 기본값은 0 이며이 효과 동일 프로그램을 실행할 때마다 인수가 있습니다. 화면 다시 표시 될 때마다 다른 지터를 원하는 경우는 초기값을 설정할 수 있습니다는 `Millisecond` 속성은 `DataTime.Now` 값 (예:).


**실험 지터** 페이지 사각형 선 그리기에 값을 확인할 수 있습니다.

[![](effects-images/jitterexperiment-small.png "삼중 지터 실험 페이지의 스크린샷")](effects-images/jitterexperiment-large.png#lightbox "Triple screenshot of the JitterExperiment page")

프로그램이 straightfoward입니다. [ **JitterExperimentPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) 파일 두 개를 인스턴스화하고 `Slider` 요소 및 `SKCanvasView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.JitterExperimentPage"
             Title="Jitter Experiment">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Label">
                    <Setter Property="HorizontalTextAlignment" Value="Center" />
                </Style>

                <Style TargetType="Slider">
                    <Setter Property="Margin" Value="20, 0" />
                    <Setter Property="Minimum" Value="0" />
                    <Setter Property="Maximum" Value="100" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="segLengthSlider"
                Grid.Row="0"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference segLengthSlider},
                              Path=Value,
                              StringFormat='Segment Length = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="deviationSlider"
                Grid.Row="2"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference deviationSlider},
                              Path=Value,
                              StringFormat='Deviation = {0:F0}'}"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

`PaintSurface` 의 처리기는 [ **JitterExperimentPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs) 코드 숨김 파일을 호출 될 때마다는 `Slider` 값이 변경 합니다. 호출 `SKPathEffect.CreateDiscrete` 두를 사용 하 여 `Slider` 값 및이 사용 하 여 사각형을 스트로크 하려면:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float segLength = (float)segLengthSlider.Value;
    float deviation = (float)deviationSlider.Value;

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.StrokeWidth = 5;
        paint.Color = SKColors.Blue;

        using (SKPathEffect pathEffect = SKPathEffect.CreateDiscrete(segLength, deviation))
        {
            paint.PathEffect = pathEffect;

            SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

임의 이러한 차이 따라 채워진 영역의 윤곽선 인 경우에을 채우기 위한이 영향을 사용할 수 있습니다. **지터 텍스트** 페이지가 경로 효과 사용 하 여 텍스트를 표시 하는 방법을 보여 줍니다. 대부분의 코드는 `PaintSurface` 의 처리기는 [ `JitterTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) 클래스 되는 텍스트를 가운데 맞춤 및 크기 조정:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "FUZZY";

    using (SKPaint textPaint = new SKPaint())
    {
        textPaint.Color = SKColors.Purple;
        textPaint.PathEffect = SKPathEffect.CreateDiscrete(3f, 10f);

        // Adjust TextSize property so text is 95% of screen width
        float textWidth = textPaint.MeasureText(text);
        textPaint.TextSize *= 0.95f * info.Width / textWidth;

        // Find the text bounds
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        // Calculate offsets to center the text on the screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

여기 세 플랫폼 모두에서 가로 모드에서 실행 중인:

[![](effects-images/jittertext-small.png "삼중 지터 텍스트 페이지의 스크린샷")](effects-images/jittertext-large.png#lightbox "Triple screenshot of the JitterText page")

## <a name="path-outlining"></a>경로 개요

이미 본 거의 두 가지 예는 [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) 방식의 `SKPaint`에 존재 하는 [오버 로드](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/):

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale)
```

만 처음 두 개의 인수가 필요 합니다. 참조 하는 경로 액세스 하는 메서드는 `src` 의 스트로크 속성에 따라 경로 데이터를 수정 하는 인수를는 `SKPaint` 개체 (포함 하 여는 `PathEffect` 속성), 다음은 결과를 기록 하 고는 `dst` 경로. `resScale` 매개 변수로 더 작은 대상 경로 만드는 전체 자릿수를 줄일 수 있습니다 및 `cullRect` 인수 외부 사각형 윤곽선을 제거할 수 있습니다.

이 메서드는 하나의 기본 사용 경로 효과 전혀 포함 되지 않습니다. 경우는 `SKPaint` 개체에 해당 `Style` 속성이로 설정 `SKPaintStyle.Stroke`, 수행 *하지* 있어야 해당 `PathEffect` 설정한 후 `GetFillPath` 를 나타내는 경로 만듭니다는 *개요*소스 경로의 페인트 속성에 의해 스트로크 하는 경우.

예를 들어 경우는 `src` 경로 간단한 원의 반지름 500, 및 `SKPaint` 100의 스트로크 너비를 지정 하는 개체는 다음 `dst` 경로 두 개의 동심원, 550 반지름이 인 450 및 다른 radius를 사용 됩니다. 메서드는 `GetFillPath` 이 채우기 때문에 `dst` 경로 따라 그려지는와 같은지는 `src` 경로입니다. 또한 스트로크 수 있지만 `dst` 경로 윤곽선 참조에 대 한 경로입니다.

**경로 개요 탭** 이 보여 줍니다. `SKCanvasView` 및 `TapGestureRecognizer` 인스턴스화된는 [ **TapToOutlineThePathPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml) 파일입니다. [ **TapToOutlineThePathPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs) 코드 숨김 파일 3 개 정의 `SKPaint` 개체와 선 그리기에 대 한 두 개의 필드로 100 및 20 및 채우기에 대 한 제 3의 너비를 스트로크:

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    bool outlineThePath = false;

    SKPaint redThickStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 100
    };

    SKPaint redThinStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 20
    };

    SKPaint blueFill = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    public TapToOutlineThePathPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewTapped(object sender, EventArgs args)
    {
        outlineThePath ^= true;
        (sender as SKCanvasView).InvalidateSurface();
    }
    ...
}
```

화면 유출 된 경우는 `PaintSurface` 처리기 사용 하 여는 `blueFill` 및 `redThickStroke` 원형 경로 렌더링 하는 개체를 그리는:

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circlePath = new SKPath())
        {
            circlePath.AddCircle(info.Width / 2, info.Height / 2,
                                 Math.Min(info.Width / 2, info.Height / 2) -
                                 redThickStroke.StrokeWidth);

            if (!outlineThePath)
            {
                canvas.DrawPath(circlePath, blueFill);
                canvas.DrawPath(circlePath, redThickStroke);
            }
            else
            {
                using (SKPath outlinePath = new SKPath())
                {
                    redThickStroke.GetFillPath(circlePath, outlinePath);

                    canvas.DrawPath(outlinePath, blueFill);
                    canvas.DrawPath(outlinePath, redThinStroke);
                }
            }
        }
    }
}
```

원의 채우기 및 예상 대로 스트로크:

[![](effects-images/taptooutlinethepathnormal-small.png "일반 탭에 개요 Path 페이지의 삼중 스크린샷")](effects-images/taptooutlinethepathnormal-large.png#lightbox "일반 탭에 개요 Path 페이지의 삼중 스크린 샷")

화면을 누를 때 `outlineThePath` 로 설정 되어 `true`, 및 `PaintSurface` 처리기 만듭니다 새 `SKPath` 개체에 대 한 호출에서 대상 경로으로 사용 합니다 `GetFillPath` 에 `redThickStroke` 페인트 개체입니다. 대상 경로 입력 하 고 선이 서로 `redThinStroke`, 다음에서 결과:

[![](effects-images/taptooutlinethepathoutlined-small.png "윤곽선이 있는 탭에 개요 Path 페이지의 삼중 스크린샷")](effects-images/taptooutlinethepathoutlined-large.png#lightbox "윤곽선이 있는 탭에 개요 Path 페이지의 삼중 스크린샷")

명확 하 게 두 개의 빨간색 원이 표시는 원래 원형 경로 두 개의 순환 윤곽선으로 변환 되어 있는지 나타냅니다.

이 메서드를 사용 하는 경로 개발 하는 데 매우 유용할 수 있습니다는 `SKPathEffect.Create1DPath` 메서드. 경로 복제 하는 경우 이러한 메서드에 지정한 경로 항상 채워집니다. 채울 전체 경로 사용 하지 않으려는 경우 윤곽선 신중 하 게 정의 해야 합니다.

예를 들어는 **연결 된 체인** 샘플에 일련의 네 개의 원호, 각 쌍 기반 채울 경로 영역에 간략하게 설명 하는 두 개의 반지름으로 정의 된 링크. 코드 수는 `LinkedChainPage` 약간 다르게 수행 하는 클래스입니다.

첫째, 다시 정의 해야 합니다는 `linkRadius` 상수:

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

`linkPath` 원하는 단일 해당 radius에 따라 두 개의 원호 시작 각도 및 스윕 각도 되었습니다:

```csharp
using (SKPath linkPath = new SKPath())
{
    SKRect rect = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
    linkPath.AddArc(rect, 55, 160);
    linkPath.AddArc(rect, 235, 160);

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = linkThickness;

        using (SKPath outlinePath = new SKPath())
        {
            strokePaint.GetFillPath(linkPath, outlinePath);

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(outlinePath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);

        }

    }
}
```

`outlinePath` 개체는 다음의 윤곽선 팜과 `linkPath` 에 지정 된 속성을 사용 하 여 스트로크 때 `strokePaint`합니다.

이 방법을 사용 하는 또 다른 예에 사용 되는 경로 대 한 다음에 나오는 `SKPathEffect.Create2DPath` 메서드.

## <a name="combining-path-effects"></a>경로 효과 결합합니다.

두 개의 최종 정적 생성 메서드 `SKPathEffect` 는 [ `SKPathEffect.CreateSum` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateSum/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/) 및 [ `SKPathEffect.CreateCompose` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCompose/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/):

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

두이 방법 모두 복합 경로 효과를 만들려면 두 개의 경로 효과 결합 합니다. `CreateSum` 메서드를 별도로 적용 되는 두 개의 경로 효과 비슷한 경로 효과 만듭니다 동안 `CreateCompose` 하나의 경로 효과 적용 합니다 (의 `inner`) 적용 하는 `outer` 합니다.

이미 본 방법을 `GetFillPath` 방식의 `SKPaint` 하나의 경로에 따라 다른 경로로 변환할 수 `SKPaint` 속성 (포함 하 여 `PathEffect`) 되지 않아야 하므로 *너무* 방법는 알수없는`SKPaint`개체가 해당 작업에 지정 된 두 개의 경로 효과로 두 번 수행할 수는 `CreateSum` 또는 `CreateCompose` 메서드.

한 가지 확실 한 용도 `CreateSum` 정의 하는 것는 `SKPaint` 개체 경로 하나 영향을 주지 않고 경로 칠 및 다른 경로 영향을 주지 않고 경로 선입니다. 이 확인할는 **프레임에 Cats** 수직 물결 막대 가장자리가 프레임에 속하고 cats 배열을 표시 하는 샘플:

[![](effects-images/catsinframe-small.png "프레임의 Cats 페이지의 삼중 스크린샷")](effects-images/catsinframe-large.png#lightbox "프레임의 Cats 페이지의 삼중 스크린샷")

[ `CatsInFramePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs) 클래스 여러 필드를 정의 하 여 시작 합니다. 첫 번째 필드를 인식할 수는 [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) 에서 클래스는 [ **SVG 경로 데이터** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) 문서. 두 번째 경로 선과 호 프레임의 조개 패턴에 대 한 기반으로 합니다.

```csharp
public class CatsInFramePage : ContentPage
{
    // From PathDataCatPage.cs
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint catStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5
    };

    SKPath scallopPath =
        SKPath.ParseSvgPathData("M 0 0 L 50 0 A 60 60 0 0 1 -50 0 Z");

    SKPaint framePaint = new SKPaint
    {
        Color = SKColors.Black
    };
    ...
}
```

`catPath` 에 사용 될 수는 `SKPathEffect.Create2DPath` 메서드 경우는 `SKPaint` 개체 `Style` 속성이 `Stroke`합니다. 그러나 경우는 `catPath` 직접이 프로그램에서는 다음 cat의 전체 헤드 입력, 하며 수염에 대도 표시 되지 않습니다는 데 사용 됩니다. (시도해 보십시오!) 해당 경로의 개요를 다운로드 하 고 그에 따라 개요를 사용 해야 하는 고 `SKPathEffect.Create2DPath` 메서드.

생성자는이 작업을 수행합니다. 먼저 적용 하는 두 개의 변환을 `catPath` 이동 하는 (0, 0) 크기가 축소 및 중심을 가리키도록 합니다. `GetFillPath` 윤곽선의 모든 개요 가져옵니다 `outlinedCatPath`, 해당 개체에서 사용 되는 `SKPathEffect.Create2DPath` 호출 합니다. 크기 조정을 영향을 미치는 `SKMatrix` 값은 가로 보다 조금 더 큰 및 번역 요소 상태인 동안 타일 사이 거의 버퍼 cat의 세로 크기 보다 약간 파생 실험적으로 전체 cat 볼 수 있도록에 프레임의 왼쪽 위 모서리:

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    public CatsInFramePage()
    {
        Title = "Cats in Frame";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Move (0, 0) point to center of cat path
        catPath.Transform(SKMatrix.MakeTranslation(-240, -175));

        // Now catPath is 400 by 250
        // Scale it down to 160 by 100
        catPath.Transform(SKMatrix.MakeScale(0.40f, 0.40f));

        // Get the outlines of the contours of the cat path
        SKPath outlinedCatPath = new SKPath();
        catStroke.GetFillPath(catPath, outlinedCatPath);

        // Create a 2D path effect from those outlines
        SKPathEffect fillEffect = SKPathEffect.Create2DPath(
            new SKMatrix { ScaleX = 170, ScaleY = 110,
                           TransX = 75, TransY = 80,
                           Persp2 = 1 },
            outlinedCatPath);

        // Create a 1D path effect from the scallop path
        SKPathEffect strokeEffect =
            SKPathEffect.Create1DPath(scallopPath, 75, 0, SKPath1DPathEffectStyle.Rotate);

        // Set the sum the effects to frame paint
        framePaint.PathEffect = SKPathEffect.CreateSum(fillEffect, strokeEffect);
    }
    ...
}
```

생성자를 호출 `SKPathEffect.Create1DPath` 수직 물결 막대 프레임에 대 한 합니다. 경로 너비는 100 픽셀 이지만 실제 너비는 75 픽셀 프레임 주위 중첩 되는 복제 된 경로 확인 합니다. 생성자 호출의 마지막 문을 `SKPathEffect.CreateSum` 두 경로 효과 결합 하 고 결과를 설정 하 고 `SKPaint` 개체입니다.

이 작업을 사용 하면는 `PaintSurface` 처리기 매우 간단할 것입니다. 하기만 하 사각형을 정의 하 고 사용 하 여 그리는 `framePaint`:

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect rect = new SKRect(50, 50, info.Width - 50, info.Height - 50);
        canvas.ClipRect(rect);
        canvas.DrawRect(rect, framePaint);
    }
}
```

경로 효과 뒤 알고리즘에는 항상 전체 경로 따라 그려지는 또는 채움에 대 한 표시 될 사각형 밖에 표시 일부 시각적 개체를 일으킬 수 있는 원인이 됩니다. `ClipRect` 이전에 호출 된 `DrawRect` 호출 상당히 클리너 되도록 시각적 개체를 허용 합니다. (실습 클리핑 없이!)

일반적으로 사용 하는 `SKPathEffect.CreateCompose` 일부 지터 다른 경로 효과를 추가 합니다. 직접 확실히 테스트할 수 있습니다 하지만 약간 다른 예제는 다음과 같습니다.

**해치 파선** 파선 해치 줄이 포함 된 타원을 채웁니다. 작업의 대부분의 [ `DashedHatchLinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) 클래스를 수행 하는 필드 정의에서 바로 합니다. 이러한 필드는 대시 효과 해치 효과 정의합니다. 로 정의 된 `static` 후에서 참조 하기 때문에 `SKPathEffect.CreateCompose` 호출는 `SKPaint` 정의:

```csharp
public class DashedHatchLinesPage : ContentPage
{
    static SKPathEffect dashEffect =
        SKPathEffect.CreateDash(new float[] { 30, 30 }, 0);

    static SKPathEffect hatchEffect = SKPathEffect.Create2DLine(20,
        Multiply(SKMatrix.MakeScale(60, 60),
                 SKMatrix.MakeRotationDegrees(45)));

    SKPaint paint = new SKPaint()
    {
        PathEffect = SKPathEffect.CreateCompose(dashEffect, hatchEffect),
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

`PaintSurface` 표준 오버 헤드와를 한 번 호출에 포함 해야 하는 처리기 `DrawOval`:

```csharp
public class DashedHatchLinesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawOval(info.Width / 2, info.Height / 2,
                        0.45f * info.Width, 0.45f * info.Height,
                        paint);
    }
    ...
}
```

이미 검색 된 대로 해치 선 정확 하 게 영역의 내부로 제한 되지 않습니다 되며이 예제에서는 항상 시작 전체 대시로 왼쪽에:

[![](effects-images/dashedhatchlines-small.png "파선 해치 페이지의 삼중 스크린샷")](effects-images/dashedhatchlines-large.png#lightbox "해치 파선 페이지의 삼중 스크린샷")

이제에서 간단한 범위의 경로 효과 살펴 보았으며 점과 이상한 조합에 대시가 구상에 사용 하 여을 만들 수 있습니다를 참조 합니다.



## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
