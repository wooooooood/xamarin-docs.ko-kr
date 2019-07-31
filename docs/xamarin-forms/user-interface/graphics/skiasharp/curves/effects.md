---
title: SkiaSharp에서 경로 효과
description: 이 문서에서는 샘플 코드를 사용 하 여이 보여 줍니다 및 경로에 선 그리기 및 입력을 사용할 수 있는 다양 한 SkiaSharp 경로 효과 설명 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: davidbritch
ms.author: dabritch
ms.date: 07/29/2017
ms.openlocfilehash: e0af5188dd34e76b419b4cd5bf8d604fb059b7d3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68642763"
---
# <a name="path-effects-in-skiasharp"></a>SkiaSharp에서 경로 효과

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_선 그리기 및 입력에 사용할 경로 허용 하는 다양 한 경로 효과 검색 합니다._

A *경로 효과* 의 인스턴스를 [ `SKPathEffect` ](xref:SkiaSharp.SKPathEffect) 클래스에 의해 정의 되는 8 개의 정적 생성 방법 중 하나를 사용 하 여 만든 클래스입니다. `SKPathEffect` 개체 설정 됩니다는 [ `PathEffect` ](xref:SkiaSharp.SKPaint.PathEffect) 속성을 [ `SKPaint` ](xref:SkiaSharp.SKPaint) 작은 복제 경로 사용 하 여 줄을 따라 다양 한 예를 들어, 흥미로운 효과 대 한 개체 :

![](effects-images/patheffectsample.png "연결 된 체인 샘플")

경로 효과 수행할 수 있습니다.

- 점 및 대시를 사용 하 여 선을 스트로크합니다
- 스트로크 채워진된 모든 경로 사용 하 여 줄
- 빗살 무늬 선을 사용 하 여 영역 채우기
- 바둑판식으로 배열 된 경로 사용 하 여 영역 채우기
- 날카로운 모퉁이가 둥근 확인
- 임의 "지터" 선과 곡선을 추가 합니다.

또한 둘 이상의 경로 효과 결합할 수 있습니다.

이 문서에 사용 하는 방법도 보여 줍니다 합니다 [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath*) 메서드의 `SKPaint` 의 속성을 적용 하 여 다른 경로를 하나의 경로 변환할 `SKPaint`등 `StrokeWidth` 및 `PathEffect`합니다. 이 인해 다른 경로 대 한 개요는 경로 가져오는 것과 같은 몇 가지 흥미로운 기술입니다. `GetFillPath` 경로 효과 관련 하 여 도움이 됩니다.

## <a name="dots-and-dashes"></a>점 및 대시

사용 된 [ `PathEffect.CreateDash` ](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) 문서에 설명 된 메서드 [ **점 및 대시**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)합니다. 메서드의 첫 번째 인수가 두 개 이상의 값을 교대로 반복 되는 대시의 길이 대시 사이 간격의 길이 사이의 짝수를 포함 하는 배열:

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

이러한 값은 *되지* 스트로크 너비를 기준으로 합니다. 스트로크 너비는 10을 줄 사각형 대시 및 간격을 사각형으로 구성 하는 경우 예를 들어 설정 된 `intervals` 배열 {10, 10}. `phase` 대시 패턴 내에서 줄 시작 되는 인수를 나타냅니다. 이 예제에서는 사각형 격차를 사용 하 여 시작 하는 줄을 하려는 경우 설정 `phase` 10입니다.

대시의 끝은 영향을 받지 합니다 `StrokeCap` 속성의 `SKPaint`합니다. 넓은 선 너비에 대 한이 속성을 설정 하는 매우 일반적 이기 `SKStrokeCap.Round` 대시의 끝을 반올림 합니다. 이 경우 값을 `intervals` 배열을 *하지* 길어지는 반올림에서 결과 포함 합니다. 이 팩트 순환 점 0의 너비를 지정 해야 한다는 것을 의미 합니다. 10의 스트로크 너비의 사용에 순환 점과 같은 지름의 점 간의 차이 사용 하 여 줄을 만들려면는 `intervals` 배열 {0, 20}.

**점으로 구분 된 텍스트 애니메이션** 비슷합니다는 **윤곽선이 있는 텍스트** 문서에서 설명 하는 페이지 [ **통합 텍스트와 그래픽** ](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md) 에서 텍스트 문자를 설정 하 여 설명 표시는 `Style` 의 속성을 `SKPaint` 개체를 `SKPaintStyle.Stroke`입니다. 또한 **점으로 구분 된 텍스트 애니메이션** 사용 하 여 `SKPathEffect.CreateDash` 이 개요는 점으로 구분 된 형태를 제공 하 고 프로그램 또한 애니메이션를 `phase` 인수를 `SKPathEffect.CreateDash` 텍스트를 둘러싸는 여행을 점을 확인 하는 방법 문자입니다. 가로 모드에서 페이지가 같습니다.

[![](effects-images/animateddottedtext-small.png "애니메이션을 점으로 구분 된 텍스트 페이지의 삼중 스크린샷")](effects-images/animateddottedtext-large.png#lightbox "삼중 애니메이션 점으로 구분 된 텍스트 페이지 스크린샷")

[ `AnimatedDottedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) 클래스는 몇 가지 상수를 정의 하 여 시작 하 고 재정의 `OnAppearing` 고 `OnDisappearing` 애니메이션에 대 한 메서드:

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

합니다 `PaintSurface` 처리기를 만들어 시작을 `SKPaint` 텍스트를 표시 하는 개체입니다. `TextSize` 속성 화면 너비에 따라 조정 됩니다.

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

메서드의 끝 합니다 `SKPathEffect.CreateDash` 메서드를 사용 하 여 호출 됩니다 합니다 `dashArray` 는 필드와 애니메이션이 적용 된로 정의 된 `phase` 값입니다. `SKPathEffect` 인스턴스로 설정 됩니다는 `PathEffect` 의 속성을 `SKPaint` 텍스트를 표시 하는 개체입니다.

설정할 수 있습니다 합니다 `SKPathEffect` 개체는 `SKPaint` 텍스트를 측정 하 고 페이지의 가운데 하기 전에 개체입니다. 그러나이 경우 렌더링된 된 텍스트의 크기에 애니메이션된 점 및 대시 인해 약간 달라질 및 잠시를 진동 할 텍스트는 경향이 있습니다. (지금 사용해 보세요!)

텍스트 문자 주위의 애니메이션된 점 원으로 있는지 각 닫힌된 곡선의 특정 지점 존재 내부 및 외부로 팝 할 점이 보일 있는 경우도 있습니다. 이것이 문자 윤곽선을 정의 하는 경로 시작 하 고 끝나는 위치입니다. 경로 길이 대시 패턴 (이 경우 20 픽셀) 길이 대 한 배수가 없으면 해당 패턴의 일부만 경로 끝에 맞출 수 있습니다.

경로의 길이 맞게 대시 패턴의 길이를 조정할 수 있지만 문서에서 설명 하는 기술 경로의 길이 결정 해야 하는 [ **경로 정보 및 열거형** ](information.md).

**Dot / Dash Morph** 대시 같습니다 다시 폼 대시를 결합 하는 점, 나눌 수 있도록 프로그램 애니메이션 자체는 대시 패턴을 적용 합니다.

[![](effects-images/dotdashmorph-small.png "Triple 점 Dash Morph 페이지 스크린샷")](effects-images/dotdashmorph-large.png#lightbox "Triple 점 Dash Morph 페이지 스크린샷")

합니다 [ `DotDashMorphPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) 재정의 클래스를 `OnAppearing` 및 `OnDisappearing` 이전 프로그램 했지만 클래스 정의와 같이 메서드를 `SKPaint` 필드로 개체:

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

합니다 `PaintSurface` 처리기가 페이지의 크기를 기준으로 타원형 경로 만들고 설정 하는 코드는 긴 작업을 실행 합니다 `dashArray` 및 `phase` 변수입니다. 애니메이션 변수 `t` 범위는 0에서 1로는 `if` 블록 4 개 분기에 해당 분기의 각 시간 중단 `tsub` 도 범위는 0에서 1 ~입니다. 맨 끝에서 프로그램을 만듭니다는 `SKPathEffect` 설정 하는 `SKPaint` 그리기에 대 한 개체입니다.

## <a name="from-path-to-path"></a>경로에 대 한 경로에서

합니다 [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,System.Single)) 메서드의 `SKPaint` 의 설정에 따라 다른 하나의 경로 설정 합니다 `SKPaint` 개체입니다. 이 과정을 보려면 대체는 `canvas.DrawPath` 다음 코드를 사용 하 여 이전 프로그램에서 호출 합니다.

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

이 새 코드를 `GetFillPath` 변환 호출를 `ellipsePath` (되는 타원에만)에 `newPath`를 사용 하 여 표시 되는 `newPaint`합니다. `newPaint` 개체가 모든 기본 속성 설정을 사용 하 여 만들어집니다 점을 제외 하 고는 `Style` 속성 기반은 부울 값을 반환 `GetFillPath`합니다.

시각적 개체에 설정 된 색을 제외 하 고 동일 `ellipsePaint` 있지만 `newPaint`합니다. 대신에 정의 된 단순한 타원 `ellipsePath`, `newPath` 일련의 점 및 대시를 정의 하는 다양 한 경로 윤곽을 포함 합니다. 이 다양 한 속성을 적용 한 결과인 `ellipsePaint` (특히 `StrokeWidth`, `StrokeCap`, 및 `PathEffect`)를 `ellipsePath` 결과 경로에 배치 하 고 `newPath`. 합니다 `GetFillPath` 메서드가 나타내는 부울 값을 대상 경로 입력할 수 있는지 여부 반환; 반환 값은이 예제에서는 `true` 경로 입력 하는 것에 대 한 합니다.

변경해 보세요 합니다 `Style` 에서 설정 `newPaint` 에 `SKPaintStyle.Stroke` 1 픽셀 너비 선으로 설명 된 개별 경로 윤곽을 확인할 수 있습니다.

## <a name="stroking-with-a-path"></a>경로 사용 하 여 선 그리기

합니다 [ `SKPathEffect.Create1DPath` ](xref:SkiaSharp.SKPathEffect.Create1DPath(SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPath1DPathEffectStyle)) 메서드는 개념적으로 비슷합니다 `SKPathEffect.CreateDash` 제외 하 고 대시 및 간격 패턴 보다는 경로 지정 합니다. 이 경로 여러 번 줄 선이나 곡선에 복제 됩니다.

사용되는 구문은 다음과 같습니다.

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

일반적으로 전달 하는 경로 `Create1DPath` 작고 (0, 0) 점을 가운데 맞춤 됩니다. `advance` 줄에 복제 되는 경로 매개 변수 경로 중심 사이의 거리를 나타냅니다. 일반적으로이 인수 경로의 대략적인 너비를 설정 합니다. `phase` 인수 수행 하므로 동일한 역할이 여기에서 수행 된 `CreateDash` 메서드.

합니다 [ `SKPath1DPathEffectStyle` ](xref:SkiaSharp.SKPath1DPathEffectStyle) 에 세 가지 멤버가 있습니다.

- `Translate`
- `Rotate`
- `Morph`

`Translate` 멤버의 경로 직선이 나 곡선을 따라 복제 되는 동일한 방향으로 유지 하면 됩니다. 에 대 한 `Rotate`, 경로 접선 곡선을 따라 회전 합니다. 경로 해당 일반 방향을 가로 선입니다. `Morph` 비슷합니다 `Rotate` 제외 하 고 경로 자체의 스트로크 되는 줄의 곡률에 맞게 곡선인 수도 있습니다.

합니다 **1 D 경로 효과** 페이지에서는 이러한 세 가지 옵션을 보여 줍니다. 합니다 [ **OneDimensionalPathEffectPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml) 파일 열거형의 세 멤버에 해당 하는 세 가지 항목을 포함 하는 선택을 정의 합니다.

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
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Translate</x:String>
                    <x:String>Rotate</x:String>
                    <x:String>Morph</x:String>
                </x:Array>
            </Picker.ItemsSource>
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

합니다 [ **OneDimensionalPathEffectPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs) 코드 숨김 파일 정의 3 `SKPathEffect` 필드로 개체입니다. 이러한 모든 사용 하 여 만들어집니다 `SKPathEffect.Create1DPath` 사용 하 여 `SKPath` 개체를 사용 하 여 만든 `SKPath.ParseSvgPathData`합니다. 첫 번째는 간단한 상자, 두 번째는 다이아몬드 모양 및 세 번째 사각형입니다. 이러한 세 가지 효과 스타일을 보여 주기 위해 사용 됩니다.

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

            switch ((string)effectStylePicker.SelectedItem))
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

합니다 `PaintSurface` 처리기 자체, 루프 및 결정 선택기에 액세스 하는 베 지 어 곡선을 만듭니다 `PathEffect` 해당 스트로크를 사용 해야 합니다. 세 가지 옵션 — `Translate`, `Rotate`, 및 `Morph` -왼쪽에서 오른쪽으로 표시 됩니다.

[![](effects-images/1dpatheffect-small.png "삼중 1d 경로 효과 페이지 스크린샷")](effects-images/1dpatheffect-large.png#lightbox "삼중 1d 경로 효과 페이지 스크린샷")

에 지정 된 경로 `SKPathEffect.Create1DPath` 메서드 항상 채워집니다. 에 지정 된 경로 `DrawPath` 메서드는 경우에 항상 스트로크 되는 `SKPaint` 개체에 해당 `PathEffect` 1d 경로 효과를 설정 하는 속성입니다. 있음을 합니다 `pathPaint` 개체에 없습니다 `Style` 기본값은 일반적으로 설정 `Fill`, 경로 상관 없이 스트로크는 있지만 합니다.

사용 하는 상자는 `Translate` 예로 20 픽셀 정사각형 및 `advance` 인수를 24로 설정 합니다. 이 차이로 인해 경우 줄 약 수평 또는 수직 상자 겹치는 상자의 대각선 28.3 픽셀 이므로 줄 대각선 되는 경우 잠시 상자 사이의 간격을 합니다.

다이아몬드 모양에는 `Rotate` 예제에서는 너비가 20 픽셀 이기도 합니다. `advance` 다이아몬드 줄의 곡률 함께 회전 되는지으로 터치 지점을 계속 되도록 20으로 설정 되었습니다.

사각형 셰이프를 `Morph` 예로 사용 하 여 50 픽셀을 `advance` 55 베 지 어 곡선 주위 구부러진 것 처럼 사각형 사이의 간격을 작은 수 있도록 설정 합니다.

경우는 `advance` 인수가 경로의 크기 보다 작은 경우 복제 된 경로 겹칠 수 있습니다. 이 작업은 몇 가지 흥미로운 효과에서 발생할 수 있습니다. 합니다 **연결 된 체인** 일련의 중단을 catenary 눈에 띄는 모양에 연결 된 체인을 유사 하 게 보이는 원 겹치는 페이지에 표시 됩니다.

[![](effects-images/linkedchain-small.png "연결 된 체인 페이지 스크린샷 삼중")](effects-images/linkedchain-large.png#lightbox "삼중 연결 된 체인 페이지 스크린샷")

근접 찾고 실제로 원 아닙니다는 표시 됩니다. 체인의 각 링크는 두 개의 원호, 크기 및 배치 되므로 인접 링크를 사용 하 여 연결 하는 것 같습니다.

체인 또는 uniform 가중치 분산의 케이블을 catenary의 형태로 중단 됩니다. 반전 된 catenary의 형태로 작성 하는 아키텍처는 아키텍처의 가중치에서 압력을 균등 하 게 요약 하는에서 혜택. catenary 간단해 보이는 수학 설명에 있습니다.

`y = a · cosh(x / a)`

합니다 *cosh* 은 쌍 곡 코사인 함수입니다. 에 대 한 *x* 0과 같지 *cosh* 0 및 *y* equals *는*합니다. catenary의 중심입니다. 같은 *코사인* 함수 *cosh* 이라고 *도*, 즉 *cosh(–x)* equals *cosh(x)* , 양수 또는 음수 인수 증가 대 한 값을 높이고 있습니다. 이러한 값은 catenary 측면을 형성 하는 곡선을 설명 합니다.

적절 한 값 찾기 *는* 휴대폰의 페이지 크기 catenary에 맞게 아닙니다 직접 계산 합니다. 경우 *w* 하 고 *h* 최적의 값 사각형의 높이 너비는 *를* 은 다음 수식을 충족 합니다.

`cosh(w / 2 / a) = 1 + h / a`

에 다음 메서드를 [ `LinkedChainPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) 클래스는 해당 같음으로 등호의 오른쪽 및 왼쪽에 두 식이 참조 하 여 통합 `left` 고 `right`입니다. 값이 작으면 *를*, `left` 보다 크면 `right`;의 값이 크면 *를*, `left` 는 보다 작은 `right`. 합니다 `while` 루프의 최적 값에에서 좁힙니다 *는*:

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

`SKPath` 링크 만들어지고 결과 클래스의 생성자에서 개체 `SKPathEffect` 개체 설정 됩니다는 `PathEffect` 의 속성을 `SKPaint` 필드로 저장 되어 있는 개체:

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

주 작업은 `PaintSurface` 처리기가 자체 catenary에 대 한 경로 만들어야 합니다. 최적의 확인 한 후 *는* 에 저장 하는 `optA` 변수에 해야 창의 위쪽에서 오프셋을 계산 합니다. 컬렉션에 누적 될 수 있습니다 차례로 `SKPoint` catenary에 대 한 값을 한 경로로 설정 하는 하 고 이전에 만든를 사용 하 여 경로 그리는 `SKPaint` 개체:

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

이 프로그램에 사용 된 경로가 정의 `Create1DPath` 있어야 해당 (0, 0) 센터를 지정 합니다. 이 것이 좋습니다 때문에 (0, 0) 패스의 줄 또는 표시 되는 곡선에 맞춥니다. 그러나 사용할 수는 비 중심 (0, 0) 일부 특수 효과 대 한 지점입니다.

합니다 **컨베이어 벨트** 페이지 곡선된 위쪽 및 아래쪽 창의 크기를 크기가 지정 된는 장방형 컨베이어 벨트와 비슷한 경로 만듭니다. 간단한을 사용 하 여 해당 경로 스트로크 `SKPaint` 20 픽셀 색이 지정 된 회색 개체 및 다른 한 다음 다시 스트로크 `SKPaint` 개체는 `SKPathEffect` 거의 버킷와 비슷한 경로 참조 하는 개체:

[![](effects-images/conveyorbelt-small.png "삼중 컨베이어 벨트 페이지 스크린샷")](effects-images/conveyorbelt-large.png#lightbox "삼중 컨베이어 벨트 페이지 스크린샷")

(0, 0) 버킷 경로의 점은 핸들, 있으므로 `phase` 인수 애니메이션이 적용 되어, 버킷의 아마도 scooping 물 맨 아래에 등록 하 고 맨 위에 있는 덤프 하는 컨베이어 벨트를 중심으로 것 같습니다.

합니다 [ `ConveyorBeltPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs) 클래스의 재정의 사용 하 여 애니메이션을 구현 합니다 `OnAppearing` 및 `OnDisappearing` 메서드. 버킷에 대 한 경로 페이지의 생성자에서 정의 됩니다.

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

버킷 만들기 코드를 약간 크게 버킷 하 고 옆으로 설정 하는 두 가지 변환을 사용 하 여 완료 합니다. 이러한 변환 적용 된 이전 코드에 있는 모든 좌표를 조정 하는 것 보다 쉽습니다.

`PaintSurface` 처리기 자체가 컨베이어 벨트에 대 한 경로 정의 하 여 시작 합니다. 다음은 단순히 한 쌍의 선 및 20 픽셀 너비 진한 회색 선으로 그려진 원 세미콜론의 쌍입니다.

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

컨베이어 벨트 그리기 논리를 가로 모드로 작동 하지 않습니다.

버킷의 200 픽셀 떨어진 컨베이어 벨트에 대 한 배치 해야 합니다. 그러나 컨베이어 벨트 않을의 배수가으로 즉 long, 200 픽셀을 `phase` 인수의 `SKPathEffect.Create1DPath` 은 애니메이션 효과가 적용 버킷 나타납니다 내부 및 외부로 존재 합니다.

이러한 이유로 프로그램 먼저 값을 계산 하 라는 `length` 컨베이어 벨트의 길이입니다. 컨베이어 벨트로 구성 된 직선과 세미콜론 원 이므로 계산은 간단 합니다. 버킷 수가 나누어 계산 됩니다 어 `length` 200 여 합니다. 가장 가까운 정수로 반올림이 고 나눌 숫자 다음는 `length`합니다. 결과 대 한 버킷 정수에 대 한 간격입니다. `phase` 인수가의 분수 하기만 하면 됩니다.

## <a name="from-path-to-path-again"></a>경로가 다시에서

맨 아래에 `DrawSurface` 처리기에서 **컨베이어 벨트**, 주석 처리를 `canvas.DrawPath` 호출 하 고 다음 코드로 바꿉니다:

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

이전 예제와 마찬가지로 `GetFillPath`, 결과 색 제외 하 고 동일한 지 표시 됩니다. 실행 한 후 `GetFillPath`, `newPath` 버킷 경로의 여러 복사본을 포함 하는 개체를 애니메이션 호출 시 해당 배치는 동일한 스폿 배치 각각.

## <a name="hatching-an-area"></a>영역 빗살 무늬

합니다 [ `SKPathEffect.Create2DLines` ](xref:SkiaSharp.SKPathEffect.Create2DLine(System.Single,SkiaSharp.SKMatrix)) 메서드 라고도 평행선이 있는 영역을 채웁니다 *줄 해치*합니다. 메서드가 다음 구문:

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

`width` 인수 빗살 무늬 선 스트로크 너비를 지정 합니다. `matrix` 매개 변수 크기 조정 및 선택적 회전의 조합입니다. 배율은 Skia 빗살 무늬 선 공간을 사용 하는 픽셀 증가분을 나타냅니다. 줄 구분은 빼기 배율 인수는 `width` 인수입니다. 배율 인수 보다 작거나 같은 경우는 `width` 값 빗살 무늬 선 사이의 공백이 됩니다 및 채울 영역에 표시 됩니다. 수평 및 수직적 크기 조정에 대 한 동일한 값을 지정 합니다.

기본적으로 해치 선은 가로입니다. 경우는 `matrix` 회전을 포함 하는 매개 변수, 빗살 무늬 선 시계 방향으로 회전 합니다.

합니다 **채우기 해치** 페이지에이 경로 효과 보여 줍니다. 합니다 [ `HatchFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs) 는 크기 조정 비율을 나타내기 위해 사용 하 여 3 픽셀 너비를 사용 하 여 가로 빗살 무늬 선에 대 한 첫 번째 간격이 6 픽셀 만큼 떨어지게, 클래스 필드로 3 경로 효과 정의 합니다. 줄 구분 3 픽셀로 되므로 합니다. 두 번째 경로 효과 6 개의 픽셀 너비와 수직 빗살 무늬 선 간격이 24 픽셀 떨어진 (따라서 분리는 18 픽셀은)에 대 한 및 빗금 줄 12 픽셀 넓은 공백이 포함 된 36 픽셀 떨어진 세 번째입니다.

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

행렬 확인 `Multiply` 메서드. 가로 및 세로 배율 같기 때문에 크기 조정 및 회전 매트릭스를 곱합니다 순서는 중요 하지 않습니다.

합니다 `PaintSurface` 처리기와 함께에서 서로 다른 세 색을 사용 하 여 이러한 세 경로 효과 사용 하 여 `fillPaint` 페이지에 맞게 크기를 조정할 모서리가 둥근된 사각형을 채웁니다. `Style` 에서 속성을 설정 `fillPaint` 무시 되 때를 `SKPaint` 개체에서 만든 경로 효과 포함 `SKPathEffect.Create2DLine`, 영역에 관계 없이 채워집니다.

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

결과 신중 하 게 보면 빨강 및 파랑 빗살 무늬 선 모퉁이가 둥근된 사각형을 정확 하 게 제한 하지는 표시 됩니다. (이 분명히 특성 기본 Skia 코드입니다.) 이 방법이 만족 스 럽 지 않으면 대각선으로 대각선 모양의 빗살 무늬를 표시 합니다. 모퉁이가 둥근 사각형은 클리핑 경로로 사용 되 고 빗살 무늬 선은 전체 페이지에 그려집니다.

`PaintSurface` 처리기가 단순히 빨강 및 파랑 빗살 무늬 선을 사용 하 여 불일치를 볼 수 있도록 모퉁이가 둥근된 사각형을 스트로크를 호출 하 여 종료 합니다.

[![](effects-images/hatchfill-small.png "채우기 해치 페이지 스크린샷 삼중")](effects-images/hatchfill-large.png#lightbox "삼중 채우기 해치 페이지 스크린샷")

Android 화면은 다음과 같이 표시 되지 않습니다. 스크린 샷 크기를 조정 하면 작은 빨강 선과 얇은 공간이 더 넓은 빨간색 선과 더 넓은 공간으로 통합 됩니다.

## <a name="filling-with-a-path"></a>경로 사용 하 여 채우기

합니다 [ `SKPathEffect.Create2DPath` ](xref:SkiaSharp.SKPathEffect.Create2DPath(SkiaSharp.SKMatrix,SkiaSharp.SKPath)) 영역을 바둑판식 적용 영역을 가로 방향과 세로 방향으로 복제 되는 경로 채울 수 있습니다.

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

`SKMatrix` 배율 복제 경로의 가로 및 세로 간격을 나타냅니다. 하지만이 사용 하 여 경로 회전할 수 없습니다 `matrix` 인수; 회전 경로 사용할 경우 경로 자체의 회전를 사용 하는 `Transform` 정의한 메서드 `SKPath`.

일반적으로 복제 된 경로 채워질 영역 보다는 화면의 왼쪽 및 위쪽 가장자리에 맞춥니다. 0에서 왼쪽 및 위쪽 양쪽에서 가로 및 세로 오프셋을 지정 하려면 크기 조정 요인을 사이의 변환 요소를 제공 하 여이 동작을 재정의할 수 있습니다.

합니다 **경로 타일 채우기** 페이지에이 경로 효과 보여 줍니다. 영역을 바둑판식으로 배열에 사용 되는 경로에서 필드로 정의 되는 [ `PathTileFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) 클래스입니다. 가로 및 세로 좌표 범위에서-40 40, 즉이 경로 80 픽셀 사각형:

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

에 `PaintSurface` 처리기는 `SKPathEffect.Create2DPath` 발생할 겹치는 80 픽셀 정사각형 타일에는 64를 가로 및 세로 간격을 설정 하는 호출 합니다. 다행 스럽게도 경로 원활 하 게 사용 하 여 타일에 인접 meshing 퍼즐 조각에서와 유사 합니다.

[![](effects-images/pathtilefill-small.png "삼중 경로 타일 채우기 페이지 스크린샷")](effects-images/pathtilefill-large.png#lightbox "삼중 경로 타일 채우기 페이지 스크린샷")

Android 화면에서 특히 일부 왜곡을 사용 하면 원래 스크린샷에서 크기 조정 합니다.

이러한 타일 항상 전체를 표시 하는 절대로 잘리지 않습니다 확인 합니다. 처음 두 개의 스크린샷을에서 확인할 수는 없지만 채워질 영역 모퉁이가 둥근된 사각형을 인지 합니다. 이러한 타일 특정 영역을 자를 않으려면 클리핑 패스를 사용 합니다.

설정 해 보세요 합니다 `Style` 의 속성을 `SKPaint` 개체를 `Stroke`를 입력 하는 것이 아니라 설명 된 개별 타일을 확인할 수 있습니다.

것도 가능 영역을 채우는 바둑판식으로 배열 된 비트맵을 사용 하 여 문서에 나와 있는 것 처럼 [ **SkiaSharp 비트맵 바둑판식**](../effects/shaders/bitmap-tiling.md)합니다.

## <a name="rounding-sharp-corners"></a>날카로운 모퉁이 반올림합니다.

**반올림 Heptagon** 프로그램에 표시 되는 [ **호를 그리려면 세 가지 방법으로** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) 7 양면 그림의 지점 곡선을 탄젠트 호를 사용 하는 문서. **다른 반올림 Heptagon** 페이지에서 만든 경로 효과 사용 하는 훨씬 쉽게 방법을 표시 합니다 [ `SKPathEffect.CreateCorner` ](xref:SkiaSharp.SKPathEffect.CreateCorner(System.Single)) 메서드:

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

단일 인수 이름이 있지만 `radius`, 절반 원하는 모퉁이 반경을를 설정 해야 합니다. (기본 Skia 코드의 특징입니다.)

다음은 `PaintSurface` 처리기에는 [ `AnotherRoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs) 클래스:

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

이 효과 사용 하 여 선 그리기 또는 기반 채우기 합니다 `Style` 의 속성을 `SKPaint` 개체입니다. 여기이 실행 됩니다.

[![](effects-images/anotherroundedheptagon-small.png "다른 반올림 Heptagon 페이지 스크린샷 삼중")](effects-images/anotherroundedheptagon-large.png#lightbox "삼중 다른 반올림 Heptagon 페이지 스크린샷")

이 둥근된 heptagon 이전 프로그램 같다는 것을 표시 합니다. 자세한 유도 해야 하는 경우 모퉁이 반경을 실제로 100 보다는 50에 지정 된 된 `SKPathEffect.CreateCorner` 호출 모서리에 있는 프로그램 및 참조 100 radius 원 마지막 문에서 겹쳐을 주석 수 있습니다.

## <a name="random-jitter"></a>임의 지터

경우에 따라 컴퓨터 그래픽의 완벽 한 직선 매우 원하는 않으며 원하는 것이 약간의 임의성. 시도 하려는 경우에 [ `SKPathEffect.CreateDiscrete` ](xref:SkiaSharp.SKPathEffect.CreateDiscrete(System.Single,System.Single,System.UInt32)) 메서드:

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

선 그리기 또는 입력에 대 한이 경로 효과 사용할 수 있습니다. 줄 연결 세그먼트로 나뉩니다-대략적인 길이가 된 `segLength` -하 고 다른 방향으로 확장 합니다. 원래 줄에서 편차 범위 지정 된 `deviation`합니다.

마지막 인수는 효과에 사용 하는 의사 (pseudo) 난수 시퀀스를 생성 하는 데 초기값입니다. 지터 효과 다른 초기값에 대 한 약간 다르게 표시 됩니다. 인수가 0 효과 동일 프로그램을 실행할 때마다 이며, 기본값은입니다. 화면을 다시 때마다 다른 지터를 하려는 경우 초기값을 설정할 수 있습니다는 `Millisecond` 의 속성을 `DataTime.Now` 예를 들어 값입니다.


합니다 **실험 지터** 페이지 사각형 선 그리기는 서로 다른 값을 실험할 수 있습니다.

[![](effects-images/jitterexperiment-small.png "Triple 지터 실험 페이지의 스크린샷")](effects-images/jitterexperiment-large.png#lightbox "Triple screenshot of the JitterExperiment page")

프로그램은 간단 합니다. 합니다 [ **JitterExperimentPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) 파일 두 개를 인스턴스화하고 `Slider` 요소와 `SKCanvasView`:

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

합니다 `PaintSurface` 처리기에는 [ **JitterExperimentPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs) 코드 숨김 파일 이라고 때마다는 `Slider` 값이 변경 합니다. 호출한 `SKPathEffect.CreateDiscrete` 두 가지를 사용 하 여 `Slider` 값 및 사각형을 스트로크 하는:

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

채워진 영역의 윤곽선 이러한 임의 편차 될 수 있는 경우에 입력에 대 한이 영향을 사용할 수 있습니다. 합니다 **지터 텍스트** 페이지가 경로 효과 사용 하 여 텍스트를 표시 하는 방법을 보여 줍니다. 대부분의 코드를 `PaintSurface` 처리기는 [ `JitterTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) 클래스는 텍스트를 가운데 맞춤 및 크기 조정에 할당 되:

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

여기 가로 모드에서 실행 중인:

[![](effects-images/jittertext-small.png "Triple 지터 텍스트 페이지의 스크린샷")](effects-images/jittertext-large.png#lightbox "Triple screenshot of the JitterText page")

## <a name="path-outlining"></a>경로 개요

두 가지 간단한 예를 이미 살펴봤습니다 합니다 [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath*) 메서드의 `SKPaint`, 존재 하는 두 가지 버전:

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale = 1)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale = 1)
```

처음 두 인수에만 필수 이며 참조 경로 액세스 하는 메서드를 `src` 의 스트로크 속성을 기반으로 경로 데이터를 수정 하는 인수를를 `SKPaint` 개체 (포함 하 여는 `PathEffect` 속성), 다음 결과를 씁니다는 `dst` 경로입니다. 합니다 `resScale` 매개 변수를 더 작은 대상 경로 만드는 전체 자릿수를 줄일 수 있습니다 및 `cullRect` 인수 사각형 외부 윤곽을 제거할 수 있습니다.

이 메서드의 기본 사용에는 항상 경로 효과가 포함 되지 않습니다. `SKPaint` 개체 `PathEffect` 의 속성이로 설정되어`GetFillPath` 있고 해당 집합이 없는 경우는 소스 경로에 대 한 개요를 나타내는 경로를 만듭니다. `SKPaintStyle.Stroke` `Style` paint 속성입니다.

예를 들어 경우는 `src` 경로가 500 반지름의 단순 원 및 `SKPaint` 100 스트로크 너비를 지정 하는 개체는 `dst` 경로 두 동심원 반지름은 550 450 및 다른 반지름 하나 됩니다. 메서드를 호출 `GetFillPath` 이 채우기 때문에 `dst` 경로 선 그리기와 동일 합니다 `src` 경로입니다. 스트로크도 수 있지만 `dst` 경로를 경로 윤곽선을 참조 하세요.

합니다 **경로 윤곽 탭** 이 보여 줍니다. `SKCanvasView` 하 고 `TapGestureRecognizer` 에서 인스턴스화되는 [ **TapToOutlineThePathPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml) 파일입니다. 합니다 [ **TapToOutlineThePathPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs) 3을 정의 하는 코드 숨김 파일 `SKPaint` 개체를 사용 하 여 선 그리기에 대 한 두 개의 필드와 스트로크 너비 채우기에 대 한 세 번째 100 및 20:

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

화면 유출 된 경우는 `PaintSurface` 처리기에서 사용 합니다 `blueFill` 및 `redThickStroke` 원형 경로 렌더링 하는 개체:

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

원 채우기 및 예상한 대로 스트로크:

[![](effects-images/taptooutlinethepathnormal-small.png "일반 탭에 윤곽선은 패스 페이지 스크린샷 삼중")](effects-images/taptooutlinethepathnormal-large.png#lightbox "삼중 일반 탭에 윤곽선은 패스 페이지 스크린샷")

화면을 누르면 `outlineThePath` 로 설정 된 `true`, 및 `PaintSurface` 처리기를 새로 만듭니다 `SKPath` 개체 및 사용에 대 한 호출의 대상 경로로 `GetFillPath` 에 `redThickStroke` 그리기 개체입니다. 대상 경로 다음 채워지고 사용 하 여 스트로크 `redThinStroke`, 다음에서 결과:

[![](effects-images/taptooutlinethepathoutlined-small.png "윤곽선이 있는 탭에 윤곽선은 패스 페이지 스크린샷 삼중")](effects-images/taptooutlinethepathoutlined-large.png#lightbox "삼중 윤곽선이 있는 탭에 윤곽선은 패스 페이지 스크린샷")

두 개의 빨간색 원을 원래 원형 경로 두 순환 윤곽 변환 된는 명확히 표시 합니다.

이 메서드를 사용 하는 경로 개발 하는 데 매우 유용할 수는 `SKPathEffect.Create1DPath` 메서드. 이러한 메서드에서 지정한 경로의 경로 복제 되는 경우에 항상 채워져 있습니다. 채울 전체 경로 사용 하지 않으려는 경우 윤곽선 신중 하 게 정의 해야 합니다.

예를 들어 합니다 **연결 된 체인** 링크는 각 쌍의 경로를 채울 영역을 간략하게 설명 두 반지름에 기반한 4 원호, 연속 하 여 정의 된 샘플입니다. 코드를 대체할 수는 `LinkedChainPage` 약간 다른 작업을 수행 하는 클래스입니다.

재정의 하려는 먼저를 `linkRadius` 상수:

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

`linkPath` 원하는 사용 하 여 해당 단일 반지름에 따라 두 개의 원호 시작 각도 및 스윕 각도 되었습니다.

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

합니다 `outlinePath` 개체는 다음의 윤곽선의 받는 사람 `linkPath` 에 지정 된 속성을 사용 하 여 스트로크 할 때 `strokePaint`합니다.

메서드에서 사용 되는 경로 대 한 다음이 기술을 사용 하 여 또 다른 예로 제공 됩니다.

## <a name="combining-path-effects"></a>경로 효과 결합합니다.

두 가지 최종 정적 생성 방법을 `SKPathEffect` 됩니다 [ `SKPathEffect.CreateSum` ](xref:SkiaSharp.SKPathEffect.CreateSum(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect)) 하 고 [ `SKPathEffect.CreateCompose` ](xref:SkiaSharp.SKPathEffect.CreateCompose(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect)):

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

이 두 방법 모두 복합 경로 효과 만드는 두 가지 경로 효과 결합 합니다. `CreateSum` 메서드를 별도로 적용 되는 두 개의 경로 효과 비슷한 경로 효과 만듭니다 하는 동안 `CreateCompose` 하나의 경로 효과 적용 합니다 (를 `inner`) 적용 하는 `outer` 를 합니다.

이미 살펴본 하는 방법을 `GetFillPath` 메서드의 `SKPaint` 하나의 경로에 따라 다른 경로로 변환할 수 `SKPaint` 속성 (포함 `PathEffect`) 수 있도록 *너무* 방법를 알수없는`SKPaint`개체에 지정 된 두 개의 경로 효과 사용 하 여 두 번 해당 작업을 수행할 수 있습니다는 `CreateSum` 또는 `CreateCompose` 메서드.

한 가지 확실 한 용도 `CreateSum` 정의 하는 것을 `SKPaint` 개체 하나 경로 효과 사용 하 여 경로 채우고 다른 경로 효과 사용 하 여 경로 선입니다. 에 설명 되어이 **프레임에서 고양이** 수직 물결 막대 가장자리가 프레임 내 고양이 배열을 표시 하는 샘플:

[![](effects-images/catsinframe-small.png "삼중 고양이의 프레임 페이지 스크린샷")](effects-images/catsinframe-large.png#lightbox "삼중 고양이의 프레임 페이지 스크린샷")

합니다 [ `CatsInFramePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs) 클래스는 여러 필드를 정의 하 여 시작 합니다. 첫 번째 필드를 인식할 수 있습니다 합니다 [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) 에서 클래스를 [ **SVG 경로 데이터** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) 문서. 두 번째 경로 선과 프레임의 조개 패턴에 대 한 호를 기반으로 합니다.

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

`catPath` 에서 사용할 수 있습니다 합니다 `SKPathEffect.Create2DPath` 메서드 경우를 `SKPaint` 개체 `Style` 속성이 `Stroke`. 그러나 경우는 `catPath` 이 프로그램에서는 다음 전체 헤드 고양이의 입력을 하며 수염에 대 한도 표시 되지 않습니다 직접 사용 됩니다. (지금 사용해 보세요!) 해당 경로의 개요에서 해당 윤곽선을 사용 하는 것이 반드시 합니다 `SKPathEffect.Create2DPath` 메서드.

생성자는이 작업을 수행합니다. 먼저 두 변환을 적용 `catPath` 이동 하는 (0, 0) 센터로 가리키고 크기가 축소 합니다. `GetFillPath` 윤곽선 모든 개요를 가져옵니다 `outlinedCatPath`, 해당 개체에서 사용 되는 `SKPathEffect.Create2DPath` 호출 합니다. 크기 조정 고려를 `SKMatrix` 값은 가로 보다 약간 더 크게 및 번역 요소를 사용 하는 동안 타일 간에 거의 버퍼 제공 하기 위해 고양이의 세로 크기를 파생 다소 알려지고 실험적으로 전체 고양이 볼 수 있도록에 프레임의 왼쪽 위 모퉁이.

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

생성자 호출 `SKPathEffect.Create1DPath` 수직 물결 막대 프레임에 대 한 합니다. 경로 너비를 100 픽셀에 있지만 복제 경로 프레임 내에 중첩 되는 실제 너비는 75 픽셀은 알 수 있습니다. 생성자 호출의 마지막 문을 `SKPathEffect.CreateSum` 를 두 개의 경로 효과 결합 하 여 결과를 설정 합니다 `SKPaint` 개체입니다.

이 모든 작업 수를 `PaintSurface` 매우 단순하게 하는 처리기. 만 할 사각형을 정의 하 고 사용 하 여 그리는 `framePaint`:

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

경로 효과 뒤 알고리즘 항상 하면 전체 표시할 선 그리기 또는 채우기 사용 되는 일부 시각적 개체는 사각형 밖에 표시 될 수 있습니다. `ClipRect` 이전에 호출을 `DrawRect` 호출 훨씬 깔끔한 되도록 시각적 개체를 허용 합니다. (사용해 클리핑 없이!)

일반적으로 사용 하는 것 `SKPathEffect.CreateCompose` 일부 지터가 다른 경로 효과를 추가 합니다. 확실히 직접 실험할 수 있습니다 하지만 약간 다른 예는 같습니다.

합니다 **해치 파선** 파선은 평행선을 사용 하 여 타원을 채웁니다. 대부분의 작업을 [ `DashedHatchLinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) 클래스 수행 됩니다 필드 정의의 오른쪽입니다. 이러한 필드는 dash 효과 해치 효과 정의합니다. 로 정의 됩니다 `static` 에 다음 참조 하므로 `SKPathEffect.CreateCompose` 에서 호출 된 `SKPaint` 정의:

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

합니다 `PaintSurface` 표준 오버 헤드를 더한를 한 번 호출을 포함 해야 하는 처리기 `DrawOval`:

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

이미 알게 빗살 무늬 선 영역의 내부를 정확 하 게 제한 되지 않습니다 및이 예제에서는 항상 시작 전체 대시를 사용 하 여 왼쪽에서:

[![](effects-images/dashedhatchlines-small.png "삼중 파선 빗살 무늬 선 페이지 스크린샷")](effects-images/dashedhatchlines-large.png#lightbox "삼중 파선 빗살 무늬 선 페이지 스크린샷")

이제 지금까지 살펴본 경로 효과 이상한 조합에 해당 범위에서 간단한 점 및 대시 상상력을 사용 하 여을 만들 수 있습니다를 참조 하세요.



## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
