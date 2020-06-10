---
제목: "SkiaSharp의 경로 효과" 설명: "이 문서에서는 패스를 스트로크 및 채우기에 사용할 수 있도록 하는 다양 한 SkiaSharp Path 효과를 설명 하 고 샘플 코드를 사용 하 여이를 보여 줍니다."
ms. prod: xamarin. 기술: xamarin-skiasharp assetid: 95167D1F-A718-405A-AFCC-90E596D422F3 author: davidbritch: dabritch: 07/29/2017:-loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="path-effects-in-skiasharp"></a>SkiaSharp의 경로 효과

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_스트로크 그리기 및 채우기에 경로를 사용할 수 있도록 하는 다양 한 경로 효과를 검색 합니다._

*경로 효과* 는 [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) 클래스에 의해 정의 된 8 개의 정적 생성 메서드 중 하나를 사용 하 여 만든 클래스의 인스턴스입니다. `SKPathEffect`그런 다음 개체를 개체의 속성으로 설정 하 여 [`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect) [`SKPaint`](xref:SkiaSharp.SKPaint) 여러 개의 흥미로운 효과를 적용 합니다. 예를 들어, 작은 복제 된 경로를 사용 하 여 줄을 스트로크 합니다.

![연결 된 체인 샘플](effects-images/patheffectsample.png)

경로 효과를 사용 하 여 다음을 수행할 수 있습니다.

- 점과 대시가 있는 선 스트로크
- 채워진 경로를 사용 하 여 선 스트로크
- 빗살 무늬 선으로 영역 채우기
- 타일 경로를 사용 하 여 영역 채우기
- 날카로운 모퉁이가 둥근 모양 만들기
- 선 및 곡선에 임의의 "지터" 추가

또한 두 개 이상의 경로 효과를 결합할 수 있습니다.

또한이 문서에서는의 메서드를 사용 하 여 [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath*) `SKPaint` `SKPaint` 및를 비롯 한의 속성을 적용 하 여 한 경로를 다른 경로로 변환 하 `StrokeWidth` 는 방법을 보여 줍니다 `PathEffect` . 이로 인해 다른 경로에 대 한 개요 인 경로를 얻는 것과 같은 몇 가지 흥미로운 기술이 있습니다. `GetFillPath`는 경로 효과와 연결 하는 데도 유용 합니다.

## <a name="dots-and-dashes"></a>점 및 대시

메서드 사용은 [`PathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) [**점 및 대시**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)문서에 설명 되어 있습니다. 메서드의 첫 번째 인수는 대시의 길이와 대시 간 간격의 길이 사이에서 교대로 반복 되는 두 개 이상의 값을 포함 하는 배열입니다.

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

이러한 값은 스트로크 너비를 기준으로 *하지 않습니다* . 예를 들어 스트로크 너비가 10이 고 정사각형 대시와 정사각형으로 구성 된 줄을 원하는 경우 `intervals` 배열을 {10, 10}으로 설정 합니다. `phase`인수는 줄이 대시 패턴 내에서 시작 되는 위치를 나타냅니다. 이 예제에서 줄을 사각형 간격으로 시작 하려면를 `phase` 10으로 설정 합니다.

대시의 끝은의 속성에 의해 영향을 받습니다 `StrokeCap` `SKPaint` . 넓은 스트로크 너비의 경우이 속성을로 설정 하 여 대시의 끝을 둥글게 하는 것이 일반적입니다 `SKStrokeCap.Round` . 이 경우 배열의 값은 `intervals` 반올림으로 인 한 추가 길이를 포함 *하지 않습니다* . 이 사실은 원형 점에 0 너비를 지정 해야 함을 의미 합니다. 스트로크 너비 (10)의 경우 같은 지름의 점 사이에 원형 점과 간격이 있는 선을 만들려면 `intervals` {0, 20}의 배열을 사용 합니다.

**애니메이션 점선 텍스트** 페이지는 개체의 속성을로 설정 하 여 [**텍스트와 그래픽 통합**](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md) 문서에 설명 된 텍스트와 그래픽 통합 문서에서 설명 하는 **텍스트 페이지와** 비슷합니다 `Style` `SKPaint` `SKPaintStyle.Stroke` . 또한 **애니메이션 점선 텍스트** 는를 사용 `SKPathEffect.CreateDash` 하 여이 개요를 점선으로 표시 하 고, 프로그램은 `phase` 문자를 `SKPathEffect.CreateDash` 텍스트 문자 주위에 이동 하는 것 처럼 보이게 하는 메서드의 인수에도 애니메이션 효과를 줍니다. 가로 모드의 페이지는 다음과 같습니다.

[![애니메이션 점선 텍스트 페이지의 삼중 스크린 샷](effects-images/animateddottedtext-small.png)](effects-images/animateddottedtext-large.png#lightbox)

[`AnimatedDottedTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs)클래스는 일부 상수를 정의 하 여 시작 하 고 `OnAppearing` `OnDisappearing` 애니메이션에 대 한 및 메서드를 재정의 합니다.

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

`PaintSurface`처리기는 `SKPaint` 텍스트를 표시 하는 개체를 만들어 시작 합니다. `TextSize`속성은 화면의 너비를 기준으로 조정 됩니다.

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

메서드가 끝날 때 `SKPathEffect.CreateDash` 메서드는 `dashArray` 필드로 정의 된 및 애니메이션 된 값을 사용 하 여 호출 됩니다 `phase` . `SKPathEffect`인스턴스는 `PathEffect` 텍스트를 표시 하는 개체의 속성으로 설정 됩니다 `SKPaint` .

또는 `SKPathEffect` `SKPaint` 텍스트를 측정 하 고 페이지에 가운데 맞춤 하기 전에 개체를 개체에 설정할 수 있습니다. 그러나이 경우 애니메이션 된 점과 대시는 렌더링 된 텍스트의 크기에 일부 변형을 발생 시킵니다. 텍스트는 약간 진동. (사용해 보세요!)

또한 텍스트 문자 주위에 애니메이션을 적용 하는 점으로 원을 표시 하는 것을 알 수 있습니다. 각 닫힌 곡선에는 점이 pop에 표시 되 고 존재 하지 않는 것으로 보입니다. 여기서는 문자 윤곽선을 정의 하는 경로가 시작 되 고 끝나는 위치입니다. 경로 길이가 대시 패턴의 정수 배수가 아닌 경우 (이 경우 20 픽셀) 해당 패턴의 일부만 경로의 끝에 맞출 수 있습니다.

경로 길이에 맞게 대시 패턴의 길이를 조정할 수 있지만 경로를 결정 해야 합니다. 경로 [**정보 및 열거형**](information.md)문서에서 설명 하는 기술입니다.

**점/대시 모핑** 프로그램은 대시 패턴 자체에 애니메이션을 적용 하 여 대시를 점으로 나누어 대시를 다시 폼으로 결합 하는 점으로 구분 합니다.

[![점 대시 모핑 페이지의 삼중 스크린샷](effects-images/dotdashmorph-small.png)](effects-images/dotdashmorph-large.png#lightbox)

[`DotDashMorphPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs)클래스는 `OnAppearing` 이전 프로그램에서와 마찬가지로 및 메서드를 재정의 `OnDisappearing` 하지만 클래스는 개체를 필드로 정의 합니다 `SKPaint` .

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

`PaintSurface`처리기는 페이지 크기를 기준으로 타원형 경로를 만들고 및 변수를 설정 하는 긴 코드 섹션을 실행 `dashArray` `phase` 합니다. 애니메이션을 적용 하는 변수의 `t` 범위는 0에서 1 사이 `if` 이며, 블록은 해당 시간을 4 분기로 나누고 각 분기의 범위는 `tsub` 0에서 1 사이입니다. 이 프로그램은 가장 끝에서를 만들어 `SKPathEffect` `SKPaint` 그리기를 위해 개체로 설정 합니다.

## <a name="from-path-to-path"></a>경로에서 경로로

[`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,System.Single))의 메서드는 `SKPaint` 개체의 설정에 따라 한 경로를 다른 경로로 설정 `SKPaint` 합니다. 이 기능이 어떻게 작동 하는지 확인 하려면 `canvas.DrawPath` 이전 프로그램의 호출을 다음 코드로 바꿉니다.

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

이 새 코드에서 `GetFillPath` 호출은 `ellipsePath` (단지 oval)로 변환 되 고 `newPath` 로 표시 됩니다 `newPaint` . `newPaint` `Style` 속성이의 부울 반환 값을 기반으로 설정 된다는 점을 제외 하 고 모든 기본 속성 설정을 사용 하 여 개체가 만들어집니다 `GetFillPath` .

시각적 개체는에 설정 된 색을 제외 하 고는 동일 `ellipsePaint` `newPaint` 합니다. 에 정의 된 단순한 타원이 아닌는 `ellipsePath` `newPath` 일련의 점과 대시를 정의 하는 다양 한 경로 컨투어를 포함 합니다. 이는에 대 한 다양 한 속성 `ellipsePaint` (특히,, `StrokeWidth` 및)을에 적용 하 `StrokeCap` 고 결과 `PathEffect` `ellipsePath` 경로를에 배치 `newPath` 하는 것입니다. `GetFillPath`메서드는 대상 경로를 채워야 하는지 여부를 나타내는 부울 값을 반환 합니다 .이 예제에서 반환 값은 `true` 경로를 채우는 데 사용할 수 있습니다.

`Style`에서로 설정을 변경 하 `newPaint` 여 `SKPaintStyle.Stroke` 개별 경로 컨투어가 1 픽셀 너비의 선으로 표시 되는 것을 볼 수 있습니다.

## <a name="stroking-with-a-path"></a>경로를 사용 하 여 그리기

[`SKPathEffect.Create1DPath`](xref:SkiaSharp.SKPathEffect.Create1DPath(SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPath1DPathEffectStyle))메서드는 `SKPathEffect.CreateDash` 대시 및 간격 패턴 대신 경로를 지정 한다는 점을 제외 하 고는 개념적으로 유사 합니다. 이 경로는 선 또는 곡선을 스트로크 하기 위해 여러 번 복제 됩니다.

사용되는 구문은 다음과 같습니다.

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

일반적으로에 전달 하는 경로는 `Create1DPath` 작고 점 (0, 0)을 중심으로 가운데 맞춤 됩니다. `advance`매개 변수는 패스가 줄에서 복제 될 때 경로 가운데 사이의 거리를 나타냅니다. 일반적으로이 인수를 경로의 대략적인 너비로 설정 합니다. `phase`인수는 메서드에서와 동일한 역할을 합니다 `CreateDash` .

에는 [`SKPath1DPathEffectStyle`](xref:SkiaSharp.SKPath1DPathEffectStyle) 세 가지 멤버가 있습니다.

- `Translate`
- `Rotate`
- `Morph`

`Translate`멤버는 회선이 나 곡선을 따라 복제 되는 것과 동일한 방향으로 경로를 유지 합니다. 의 경우 곡선에 대 한 `Rotate` 탄젠트를 기준으로 경로가 회전 됩니다. 경로의 법선 방향은 가로 선입니다. `Morph`는와 유사 합니다 `Rotate` . 단,는 경로 자체가 스트로크 되는 선의 곡률과 일치 하는 곡선입니다.

**1D 경로 효과** 페이지는 이러한 세 가지 옵션을 보여 줍니다. [**OneDimensionalPathEffectPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml) 파일은 열거형의 세 멤버에 해당 하는 세 개의 항목이 포함 된 선택기를 정의 합니다.

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

[**OneDimensionalPathEffectPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs) 코드를 정의 하는 파일은 세 가지 개체를 필드로 정의 합니다 `SKPathEffect` . 이러한 개체는를 사용 하 여 `SKPathEffect.Create1DPath` 만든 개체와 함께를 사용 하 여 만듭니다 `SKPath` `SKPath.ParseSvgPathData` . 첫 번째 상자는 단순 상자이 고 두 번째는 다이아몬드 모양이 며 세 번째는 사각형입니다. 이는 세 가지 효과 스타일을 보여 주기 위해 사용 됩니다.

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

`PaintSurface`처리기는 자체를 반복 하는 베 지 어 곡선을 만들고 선택기에 액세스 하 여 `PathEffect` 스트로크를 스트로크 하는 데 사용 해야 하는 점을 결정 합니다. , 및의 세 가지 옵션은 `Translate` `Rotate` `Morph` 왼쪽에서 오른쪽으로 표시 됩니다.

[![1D 경로 효과 페이지의 세 번째 스크린샷](effects-images/1dpatheffect-small.png)](effects-images/1dpatheffect-large.png#lightbox)

메서드에 지정 된 경로는 `SKPathEffect.Create1DPath` 항상 채워집니다. `DrawPath` `SKPaint` 개체의 `PathEffect` 속성이 1d 경로 효과로 설정 되어 있으면 메서드에 지정 된 경로가 항상 스트로크 됩니다. 개체에는 기본적으로 `pathPaint` `Style` 로 설정 된 설정이 `Fill` 없지만 경로는에 관계 없이 스트로크 됩니다.

예제에 사용 되는 상자는 `Translate` 20 픽셀 사각형 이며 인수는 `advance` 24로 설정 됩니다. 이러한 차이로 인해 선 간격이 가로 또는 세로 일 때 상자 사이에 간격이 발생 하지만 상자의 대각선이 28.3 픽셀 이기 때문에 줄이 대각선 인 경우 상자는 약간 겹칩니다.

예제의 다이아몬드 도형은 `Rotate` 20 픽셀 너비 이기도 합니다. 은 (는) `advance` 20으로 설정 되므로 다이아몬드가 선의 곡률과 함께 회전 될 때 요소가 계속 해 서 접촉 됩니다.

예제의 사각형 도형은 `Morph` 50 픽셀 너비 이며, `advance` 베 지 어 곡선 주위에서 구부러진 사각형 사이에 약간 55의 간격이 있습니다.

`advance`인수가 경로의 크기 보다 작은 경우 복제 된 경로가 겹칠 수 있습니다. 이로 인해 몇 가지 흥미로운 효과가 발생할 수 있습니다. **연결 된 체인** 페이지에는 연결 된 체인과 유사 하 게 표시 되는 일련의 겹치는 원이 표시 되며,이는 다양 한 모양의 독특한 모양에서 중단 됩니다.

[![연결 된 체인 페이지의 삼중 스크린샷](effects-images/linkedchain-small.png)](effects-images/linkedchain-large.png#lightbox)

매우 가까이 보이지만 실제로 원이 표시 되지 않는 것을 알 수 있습니다. 체인의 각 링크는 크기가 조정 되 고 위치가 지정 된 두 개의 원호 이며 인접 링크와 연결 하는 것 처럼 보입니다.

다양 한 방식으로 정지 된 균일 한 무게 분포의 체인 또는 케이블입니다. 반전 된 식의 형태로 기본 제공 되는 아치는 아치의 무게와 동일한 압력 분포를 활용 합니다. 다음과 같이 간단한 수학적 설명을 포함 하 고 있습니다.

`y = a · cosh(x / a)`

*Cosh* 는 쌍곡선 코사인 함수입니다. *X* 가 0 인 경우 *cosh* 는 0이 고 *y* 는 *a*와 같습니다. 이는 다양 한 기능을 중심으로 합니다. *코사인* 함수와 마찬가지로 *cosh* 는 *짝수*여야 합니다. 즉, cosh *(– x)* 는 *cosh (x)* 가 되 고 양수 또는 음수 인수를 늘리면 값이 증가 하는 것을 의미 합니다. 이러한 값은 다양 한 측면을 형성 하는 곡선을 설명 합니다.

전화 페이지의 차원에 대 한 적절 *한* 값을 찾는 것은 직접 계산이 아닙니다. *W* 및 *h* 가 사각형의 너비와 높이 이면의 최적 *값은 다음 수식을 만족 합니다* .

`cosh(w / 2 / a) = 1 + h / a`

클래스의 다음 메서드는 [`LinkedChainPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) 등호의 왼쪽과 오른쪽에 있는 두 식을 및로 참조 하 여 해당 일치를 통합 합니다 `left` `right` . *의 작은 값에 대해* `left` 가 보다 큽니다. 큰 값의 경우는 `right` *a* `left` 보다 작습니다 `right` . 루프는의 `while` 최적 값으로 축소 됩니다. *a*

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

`SKPath`링크에 대 한 개체는 클래스의 생성자에서 만들어지고 결과 `SKPathEffect` 개체는 `PathEffect` 필드로 저장 된 개체의 속성으로 설정 됩니다 `SKPaint` .

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

처리기의 주요 작업은 다양 `PaintSurface` 한 경로를 만드는 것입니다. 최적를 결정 한 *후* 변수에 저장 하면 `optA` 창의 위쪽에서 오프셋을 계산 해야 합니다. 그런 다음,이를 `SKPoint` 경로에 설정 하 고 이전에 만든 개체를 사용 하 여 경로를 그릴 수 있습니다 `SKPaint` .

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

이 프로그램은에서 `Create1DPath` (0, 0) 점을 중심에 배치 하는 데 사용 되는 경로를 정의 합니다. 경로의 점 (0, 0)이 표시할 된 선 또는 곡선에 맞춰 정렬 되기 때문에이는 타당 한 것 같습니다. 그러나 일부 특수 효과의 경우 가운데 맞춤 (0, 0) 지점을 사용할 수 있습니다.

**컨베이어 벨트** 페이지는 창의 크기에 맞게 크기가 지정 된 위쪽 및 아래쪽 곡선을 사용 하 여 oblong 컨베이어 벨트와 유사한 경로를 만듭니다. 이 경로는 간단한 `SKPaint` 개체 20 픽셀 너비와 색이 지정 된 회색으로 스트로크 된 다음, `SKPaint` `SKPathEffect` 작은 버킷으로 비슷한 경로를 참조 하는 개체를 사용 하 여 다른 개체와 함께 다시 표시 됩니다.

[![컨베이어 벨트 페이지의 세 번째 스크린샷](effects-images/conveyorbelt-small.png)](effects-images/conveyorbelt-large.png#lightbox)

버킷 경로의 (0, 0) 지점은 핸들 이므로 `phase` 인수에 애니메이션을 적용할 때 버킷은 컨베이어 벨트를 중심으로 보이는 것 처럼 보일 수 있습니다. 즉, 아래쪽에 물을 scooping 위쪽에서이를 덤프할 수 있습니다.

[`ConveyorBeltPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs)클래스는 및 메서드를 재정의 하 여 애니메이션을 구현 `OnAppearing` `OnDisappearing` 합니다. 버킷의 경로는 페이지의 생성자에 정의 되어 있습니다.

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

버킷 만들기 코드는 버킷 크기를 늘려 조금씩 전환 하는 두 가지 변환으로 완료 됩니다. 이러한 변환을 적용 하는 것은 이전 코드의 모든 좌표를 조정 하는 것 보다 쉽습니다.

`PaintSurface`처리기는 컨베이어 벨트 자체의 경로를 정의 하 여 시작 합니다. 이는 단순히 줄 쌍 이며, 20 픽셀의 진한 회색 선을 사용 하 여 그린 쌍 쌍입니다.

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

컨베이어 벨트를 그리기 위한 논리는 가로 모드에서 작동 하지 않습니다.

버킷은 컨베이어 벨트에서 200 픽셀 간격으로 배치 되어야 합니다. 그러나 컨베이어 벨트는 200 픽셀의 배수가 아닐 수 있습니다. 즉,의 인수가 애니메이션으로 표시 되 `phase` `SKPathEffect.Create1DPath` 면 버킷이 존재 하지 않습니다.

따라서 프로그램은 먼저 `length` 컨베이어 벨트의 길이인 이라는 값을 계산 합니다. 컨베이어 벨트는 직선 및 반 원으로 구성 되기 때문에 간단한 계산입니다. 그런 다음, 버킷 수는 200으로 나눠 계산 됩니다 `length` . 가장 가까운 정수로 반올림 되 고이 숫자는로 나뉩니다 `length` . 결과는 정수 버킷 수의 간격입니다. `phase`인수는 단지 해당의 비율입니다.

## <a name="from-path-to-path-again"></a>경로에서 경로로 다시

`DrawSurface` **컨베이어 벨트**의 처리기 아래에서 호출을 주석으로 처리 `canvas.DrawPath` 하 고 다음 코드로 바꿉니다.

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

이전 예제와 마찬가지로 색을 `GetFillPath` 제외 하 고 결과가 동일 하다는 것을 알 수 있습니다. 을 실행 한 후 `GetFillPath` 개체에는 `newPath` 버킷 경로에 대 한 여러 복사본이 포함 되어 있으며, 각 복사본은 애니메이션이 호출 시 배치 되는 동일한 지점에 배치 됩니다.

## <a name="hatching-an-area"></a>해칭 영역

[`SKPathEffect.Create2DLines`](xref:SkiaSharp.SKPathEffect.Create2DLine(System.Single,SkiaSharp.SKMatrix))메서드는 평행선 ( *해치 선*이라고 함)을 사용 하 여 영역을 채웁니다. 메서드의 구문은 다음과 같습니다.

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

`width`인수는 해치 선의 스트로크 너비를 지정 합니다. `matrix`매개 변수는 크기 조정 및 선택적 회전의 조합입니다. 배율 인수는 고 지 수를 사용 하 여 해치 선 공간을 지정 하는 픽셀 증가값을 나타냅니다. 줄을 구분 하는 것은 인수를 뺀 배율 인수입니다 `width` . 배율 인수가 값 보다 작거나 같으면 `width` 해치 선 사이에 공백이 표시 되지 않으며 영역이 채워져 있는 것으로 나타납니다. 가로 및 세로 크기 조정에 동일한 값을 지정 합니다.

기본적으로 빗살 무늬 선은 가로입니다. `matrix`매개 변수에 회전이 포함 되어 있으면 해치 선이 시계 방향으로 회전 합니다.

**빗살 무늬 채우기** 페이지에서이 경로 효과를 보여 줍니다. [`HatchFillPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs)이 클래스는 세 개의 path 효과를 필드로 정의 합니다. 첫 번째는 너비가 3 픽셀이 고 너비가 6 픽셀 떨어져 있음을 나타내는 크기 조정 요소를 사용 합니다. 따라서 선 간의 분리는 3 픽셀입니다. 두 번째 경로는 너비가 24 픽셀 떨어져 있는 세로 해치 선 (구분은 18 픽셀)에 대 한 것 이며 세 번째는 36 픽셀 떨어져 있는 대각선 해치 선 12 픽셀에 대 한 것입니다.

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

행렬 메서드를 확인 `Multiply` 합니다. 가로 및 세로 배율 인수가 동일 하기 때문에 크기 조정 및 회전 매트릭스를 곱하는 순서는 중요 하지 않습니다.

처리기는와 함께 세 가지 색 효과를 사용 하 여 `PaintSurface` `fillPaint` 페이지에 맞게 모퉁이가 둥근 사각형 크기를 채웁니다. `Style`에 설정 된 속성은 `fillPaint` 무시 되 고, `SKPaint` 개체에에서 만든 경로 효과가 포함 된 경우 `SKPathEffect.Create2DLine` 다음에 관계 없이 영역이 채워집니다.

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

결과를 신중히 살펴보면 빨강 및 파랑 해치 선이 둥근 사각형으로 정확 하 게 한정 되지 않는 것을 알 수 있습니다. (이것은 기본가 중 Ia 코드의 특징입니다.) 이 방법이 만족 스 럽 지 않으면 녹색으로 대각선 해치 선에 대 한 대체 방법이 표시 됩니다. 모퉁이가 둥근 사각형이 클리핑 경로로 사용 되 고 빗살 무늬 선이 전체 페이지에 그려집니다.

`PaintSurface`처리기는 단순히 모퉁이가 둥근 사각형을 스트로크 하는 호출로 마무리 되므로 빨강 및 파랑 해치 선과의 불일치를 확인할 수 있습니다.

[![빗살 무늬 채우기 페이지의 세 번째 스크린샷](effects-images/hatchfill-small.png)](effects-images/hatchfill-large.png#lightbox)

Android 화면은 다음과 같이 표시 되지 않습니다. 스크린 샷 크기를 조정 하면 얇은 빨강 선과 얇은 공간이 더 넓은 빨간색 선과 더 넓은 공간으로 통합 됩니다.

## <a name="filling-with-a-path"></a>경로를 사용 하 여 채우기

에서는 [`SKPathEffect.Create2DPath`](xref:SkiaSharp.SKPathEffect.Create2DPath(SkiaSharp.SKMatrix,SkiaSharp.SKPath)) 가로 및 세로로 복제 된 경로를 사용 하 여 영역을 채운 다음 영역을 바둑판식으로 채울 수 있습니다.

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

`SKMatrix`배율 인수는 복제 된 경로의 가로 및 세로 간격을 표시 합니다. 그러나이 인수를 사용 하 여 경로를 회전할 수 없습니다 `matrix` . 경로를 회전 하려면에 의해 정의 된 메서드를 사용 하 여 경로 자체를 회전 합니다 `Transform` `SKPath` .

복제 된 경로는 일반적으로 채워지는 영역이 아니라 화면의 왼쪽 및 위쪽 가장자리와 맞춥니다. 0과 배율 인수 사이의 변환 요소를 제공 하 여 왼쪽과 위쪽에서 가로 및 세로 오프셋을 지정 하면이 동작을 재정의할 수 있습니다.

**경로 타일 채우기** 페이지에서이 경로 효과를 보여 줍니다. 영역을 바둑판식으로 배열 하는 데 사용 되는 경로는 클래스의 필드로 정의 됩니다 [`PathTileFillPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) . 가로 및 세로 좌표는-40에서 40 사이 이며,이는이 경로가 80 픽셀 정사각형 임을 의미 합니다.

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

처리기에서 `PaintSurface` `SKPathEffect.Create2DPath` 호출은 가로 및 세로 간격을 64로 설정 하 여 80 픽셀 정사각형 타일이 겹칩니다. 다행히이 경로는 인접 한 타일과 비슷하게 meshing.

[![경로 타일 채우기 페이지의 세 번째 스크린샷](effects-images/pathtilefill-small.png)](effects-images/pathtilefill-large.png#lightbox)

원본 스크린샷에서 크기를 조정 하면 특히 Android 화면에서 약간의 왜곡이 발생 합니다.

이러한 타일은 항상 전체로 표시 되며 잘리지 않습니다. 처음 두 스크린샷에서는 채워지는 영역이 둥근 사각형 임을 명확 하 게 알 수는 없습니다. 이러한 타일을 특정 영역으로 자르려면 클리핑 패스를 사용 합니다.

`Style`개체의 속성을로 설정 해 보면 `SKPaint` `Stroke` 개별 타일이 채워져 있지 않고 윤곽선이 표시 됩니다.

[**SkiaSharp 비트맵 바둑판식**](../effects/shaders/bitmap-tiling.md)배열 문서에 표시 된 것 처럼 바둑판식 비트맵으로 영역을 채울 수도 있습니다.

## <a name="rounding-sharp-corners"></a>뾰족한 모퉁이 둥근 모양

[**원호를 그리기 위한 세 가지 방법**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) 으로 제공 되는 **둥근 Heptagon** 프로그램은 접선 호를 사용 하 여 일곱 개의 측면 그림을 곡선으로 만듭니다. **또 다른 라운드 된 Heptagon** 페이지에는 메서드에서 만든 경로 효과를 사용 하는 훨씬 손쉬운 방법이 나와 [`SKPathEffect.CreateCorner`](xref:SkiaSharp.SKPathEffect.CreateCorner(System.Single)) 있습니다.

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

단일 인수의 이름은 이지만 `radius` 원하는 모퉁이 반경의 절반으로 설정 해야 합니다. (이는 기본 서 부 Ia 코드의 특징입니다.)

`PaintSurface`클래스의 처리기는 [`AnotherRoundedHeptagonPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs) 다음과 같습니다.

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

개체의 속성을 기반으로 하는 선 그리기 또는 채우기에 이러한 효과를 사용할 수 있습니다 `Style` `SKPaint` . 실행 되 고 있습니다.

[![둥근 다른 Heptagon 페이지의 세 번째 스크린샷](effects-images/anotherroundedheptagon-small.png)](effects-images/anotherroundedheptagon-large.png#lightbox)

이 둥근 heptagon 이전 프로그램과 동일 하다는 것을 알 수 있습니다. 호출에 지정 된 50이 아닌 진정한 100이 아니라 모퉁이 반지름이 진정한 이라고 하는 경우 `SKPathEffect.CreateCorner` 프로그램에서 최종 문의 주석 처리를 제거 하 여 모퉁이에 있는 100-radius 원을 볼 수 있습니다.

## <a name="random-jitter"></a>임의 지터

경우에 따라 컴퓨터 그래픽의 지 수 직선 줄이 원하는 대로 표시 되지 않고 약간의 임의성이 필요 합니다. 이 경우 메서드를 사용해 볼 수 있습니다 [`SKPathEffect.CreateDiscrete`](xref:SkiaSharp.SKPathEffect.CreateDiscrete(System.Single,System.Single,System.UInt32)) .

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

스트로크 효과 또는 채우기에이 경로 효과를 사용할 수 있습니다. 줄은 연결 된 세그먼트로 분리 되며,의 대략적인 길이는로 지정 되 `segLength` 고 다른 방향으로 확장 됩니다. 원본 줄에서의 편차 범위는에 의해 지정 됩니다 `deviation` .

마지막 인수는 효과에 사용 되는 의사 (pseudo) 난수 시퀀스를 생성 하는 데 사용 되는 초기값입니다. 지터 효과는 초기값 마다 약간 다르게 보입니다. 인수의 기본값은 0입니다 .이는 프로그램을 실행할 때마다 효과가 동일 함을 의미 합니다. 화면을 다시 그릴 때마다 다른 지터를 원하는 경우 값의 속성으로 초기값을 설정할 수 있습니다 `Millisecond` `DataTime.Now` (예:).

**지터 실험** 페이지를 사용 하 여 사각형을 스트로크 하는 데 다른 값을 시험해 볼 수 있습니다.

[![JitterExperiment 페이지의 세 번째 스크린샷](effects-images/jitterexperiment-small.png)](effects-images/jitterexperiment-large.png#lightbox)

프로그램은 간단 합니다. [**JitterExperimentPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) 파일은 두 개의 `Slider` 요소 및를 인스턴스화합니다 `SKCanvasView` .

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

`PaintSurface`값이 변경 될 때마다 [**JitterExperimentPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs) 코드 숨겨진 파일의 처리기가 호출 됩니다 `Slider` . `SKPathEffect.CreateDiscrete`두 값을 사용 하 여를 호출 하 `Slider` 고 사각형을 스트로크 하는 데 사용 합니다.

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

이 효과를 채우는 데에도 사용할 수 있습니다 .이 경우 채워진 영역의 윤곽선은 이러한 임의 편차를 따릅니다. **지터 텍스트** 페이지는이 경로 효과를 사용 하 여 텍스트를 표시 하는 방법을 보여 줍니다. 클래스 처리기에서 대부분의 코드는 `PaintSurface` 텍스트의 [`JitterTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) 크기를 조정 하 고 가운데에 정렬 합니다.

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

여기서는 가로 모드로 실행 되 고 있습니다.

[![JitterText 페이지의 세 번째 스크린샷](effects-images/jittertext-small.png)](effects-images/jittertext-large.png#lightbox)

## <a name="path-outlining"></a>경로 개요

[`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath*)두 가지 버전의 메서드에 대 한 두 가지 작은 예제 `SKPaint` 를 이미 살펴보았습니다.

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale = 1)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale = 1)
```

처음 두 인수만 필요 합니다. 메서드는 인수에서 참조 하는 경로에 액세스 `src` 하 고, 개체의 스트로크 속성 (속성 포함)을 기반으로 경로 데이터를 수정한 `SKPaint` 다음, `PathEffect` 결과를 경로에 씁니다 `dst` . `resScale`매개 변수를 사용 하면 정밀도를 줄여서 더 작은 대상 경로를 만들 수 있으며, `cullRect` 인수는 사각형 외부의 컨투어를 제거할 수 있습니다.

이 메서드의 기본 사용에는 항상 경로 효과가 포함 되지 않습니다. `SKPaint` 개체의 `Style` 속성이로 설정 되어 `SKPaintStyle.Stroke` 있고 해당 집합이 *없는 경우* 는 `PathEffect` `GetFillPath` 페인트 속성으로 스트로크 된 것 처럼 소스 경로의 *윤곽선* 을 나타내는 경로를 만듭니다.

예를 들어 `src` 경로가 반지름 500의 단순 원이 고 `SKPaint` 개체가 100의 스트로크 너비를 지정 하는 경우 `dst` 경로는 두 개의 동심 원이 됩니다. 하나는 반지름이 450이 고 다른 하나는 반지름이 550입니다. `GetFillPath`이 경로를 채우는 것 `dst` 은 경로를 선 그리기와 동일 하기 때문에 메서드가 호출 됩니다 `src` . 그러나 경로를 스트로크 하 여 `dst` 경로 윤곽선을 볼 수도 있습니다.

**경로를 간략하게 설명 하는 탭** 은이를 보여 줍니다. `SKCanvasView`및는 `TapGestureRecognizer` [**TapToOutlineThePathPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml) 파일에서 인스턴스화됩니다. [**TapToOutlineThePathPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs) 코드를 사용 하 여 세 개의 `SKPaint` 개체를 필드로 정의 합니다. 두 개의 개체는 두 개의 스트로크 너비를 100 및 20으로 채우고 세 번째는 채우기에 사용 됩니다.

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

화면을 누르지 않은 경우 `PaintSurface` 처리기는 및 그리기 개체를 사용 하 여 `blueFill` `redThickStroke` 순환 경로를 렌더링 합니다.

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

원는 예상한 대로 채워지고 스트로크 됩니다.

[![경로 페이지에 대 한 개요를 설명 하는 일반 탭의 세 번째 스크린샷](effects-images/taptooutlinethepathnormal-small.png)](effects-images/taptooutlinethepathnormal-large.png#lightbox)

화면을 누르면 `outlineThePath` 가로 설정 되 고, `true` `PaintSurface` 처리기가 새 개체를 만들어 `SKPath` `GetFillPath` paint 개체에 대 한 호출에서 대상 경로로 사용 합니다 `redThickStroke` . 그런 다음 해당 대상 경로를 채우고 그에 `redThinStroke` 따라 다음을 수행 합니다.

[![경로 페이지를 윤곽선으로 표시 하는 탭의 세 번째 스크린샷](effects-images/taptooutlinethepathoutlined-small.png)](effects-images/taptooutlinethepathoutlined-large.png#lightbox)

두 빨간색 원은 원래 원형 경로가 두 개의 원형 윤곽선으로 변환 되었음을 명확 하 게 표시 합니다.

이 메서드는 메서드에 사용할 경로를 개발 하는 데 매우 유용할 수 있습니다 `SKPathEffect.Create1DPath` . 이러한 메서드에서 지정 하는 경로는 항상 경로가 복제 될 때 채워집니다. 전체 경로를 채우지 않으려면 개요를 신중 하 게 정의 해야 합니다.

예를 들어 연결 된 **체인** 샘플에서 링크는 네 개의 호로 이루어진 일련의 원호를 사용 하 여 정의 되었습니다. 각 쌍은 채워질 경로 영역을 간략하게 설명 하기 위해 두 반지름을 기반으로 합니다. 클래스의 코드를 대체 하 여 `LinkedChainPage` 약간 다르게 할 수 있습니다.

먼저 상수를 다시 정의 하려고 합니다 `linkRadius` .

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

`linkPath`이제는 원하는 시작 각도 및 스윕 각도를 사용 하 여 해당 단일 반지름을 기반으로 하는 두 개의 원호입니다.

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

`outlinePath`그런 다음 개체는 `linkPath` 에 지정 된 속성을 사용 하 여 스트로크 될 때의 개요를 받는 사람입니다 `strokePaint` .

이 기법을 사용 하는 또 다른 예는 메서드에 사용 되는 경로에 대 한 다음입니다.

## <a name="combining-path-effects"></a>조합 경로 효과

의 두 가지 최종 정적 생성 메서드 `SKPathEffect` 는 [`SKPathEffect.CreateSum`](xref:SkiaSharp.SKPathEffect.CreateSum(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect)) 및입니다 [`SKPathEffect.CreateCompose`](xref:SkiaSharp.SKPathEffect.CreateCompose(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect)) .

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

이러한 두 메서드는 두 경로 효과를 결합 하 여 복합 경로 효과를 만듭니다. 메서드는 별도의 경로 효과 ()를 적용 하는 반면는 경로 효과를 하나 적용 한 다음에 적용 하는 경로 효과를 `CreateSum` 만듭니다 `CreateCompose` `inner` `outer` .

`GetFillPath`메서드가 `SKPaint` `SKPaint` `PathEffect` *too* `SKPaint` 또는 메서드에 지정 된 두 개의 경로 효과를 사용 하 여 해당 작업을 두 번 수행 하는 방법에 대 한 자세한 정보를 제외 하 고는 속성을 기반으로 한 경로를 다른 경로로 변환할 수 `CreateSum` 있는 방법을 이미 살펴보았습니다 `CreateCompose` .

를 사용 하는 한 가지 방법은 경로를 하나 사용 하 여 경로를 채우고 다른 경로 효과를 사용 하 여 패스를 `CreateSum` 채우는 개체를 정의 하는 것입니다 `SKPaint` . 이는 scalloped edge를 사용 하는 프레임 내에서 고양이의 배열을 표시 하는 **frame의 고양이** 샘플에 설명 되어 있습니다.

[![프레임 페이지의 고양이 세 번째 스크린샷](effects-images/catsinframe-small.png)](effects-images/catsinframe-large.png#lightbox)

[`CatsInFramePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs)클래스는 여러 필드를 정의 하 여 시작 합니다. [`PathDataCatPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) [**SVG 경로 데이터**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) 아티클의 클래스에서 첫 번째 필드를 인식할 수 있습니다. 두 번째 경로는 프레임의 scallop 패턴에 대 한 선 및 원호를 기반으로 합니다.

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

`catPath` `SKPathEffect.Create2DPath` `SKPaint` 개체 `Style` 속성이로 설정 된 경우 메서드에서를 사용할 수 있습니다 `Stroke` . 그러나 `catPath` 이 프로그램에서 직접 사용 되는 경우에는 cat의 전체 헤드를 채우고 수염 표시 되지 않습니다. (사용해 보세요!) 해당 경로의 개요를 가져와서 메서드에서 해당 개요를 사용 해야 `SKPathEffect.Create2DPath` 합니다.

생성자는이 작업을 수행 합니다. 먼저 두 개의 변환을 적용 `catPath` 하 여 (0, 0) 점을 가운데로 이동 하 고 크기를 축소 합니다. `GetFillPath`에서 컨투어의 모든 윤곽선을 가져오고 `outlinedCatPath` 해당 개체가 호출에 사용 됩니다 `SKPathEffect.Create2DPath` . `SKMatrix`타일 사이에 약간의 버퍼를 제공 하기 위해이 값의 배율 인수는 cat의 가로 및 세로 크기 보다 약간 더 크지만, 변환 요소는 약간 알려지고 실험적으로 되어 프레임의 왼쪽 위 모퉁이에 전체 cat이 표시 됩니다.

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

그런 다음 생성자는 `SKPathEffect.Create1DPath` scalloped 프레임에 대해를 호출 합니다. 경로의 너비는 100 픽셀 이지만 앞으로는 복제 된 경로를 프레임 주위에 겹칠 수 있도록 75 픽셀을 미리 볼 수 있습니다. 생성자의 마지막 문은 `SKPathEffect.CreateSum` 를 호출 하 여 두 경로 효과를 결합 하 고 결과를 개체로 설정 합니다 `SKPaint` .

이러한 모든 작업을 통해 `PaintSurface` 처리기가 매우 간단할 수 있습니다. 사각형을 정의 하 고 다음을 사용 하 여 그려야 합니다 `framePaint` .

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

경로 효과를 사용 하는 알고리즘은 항상 그리기 또는 채우기에 사용 되는 전체 경로를 사용 하 여 일부 시각적 개체를 사각형 외부에 표시 하도록 합니다. `ClipRect`호출 전에 호출을 `DrawRect` 통해 시각적 개체를 매우 깔끔하게 표시할 수 있습니다. (클리핑 없이 사용해 보세요.)

일반적으로를 사용 하 여 `SKPathEffect.CreateCompose` 다른 경로 효과에 일부 지터를 추가 합니다. 직접 시험해 볼 수 있지만 약간 다른 예제는 다음과 같습니다.

**파선 빗살 무늬 선은** 타원을 파선 인 해치 선으로 채웁니다. 클래스에 있는 대부분의 작업 [`DashedHatchLinesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) 은 필드 정의에서 바로 수행 됩니다. 이러한 필드는 대시 효과와 빗살 무늬 효과를 정의 합니다. 이러한 `static` `SKPathEffect.CreateCompose` 작업은 정의의 호출에서 참조 되기 때문에로 정의 됩니다 `SKPaint` .

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

`PaintSurface`처리기는 표준 오버 헤드와에 대 한 호출을 포함 해야 합니다 `DrawOval` .

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

이미 검색 한 것 처럼 해치 선은 영역 내부를 정확 하 게 제한 하지 않으며,이 예제에서는 항상 전체 대시를 사용 하 여 왼쪽에서 시작 합니다.

[![파선 해치 선 페이지의 삼중 스크린샷](effects-images/dashedhatchlines-small.png)](effects-images/dashedhatchlines-large.png#lightbox)

이제 간단한 점과 대시에서 이상한 조합 까지의 경로 효과를 보았을 때 상상력를 사용 하 여 만들 수 있는 항목을 확인 합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
