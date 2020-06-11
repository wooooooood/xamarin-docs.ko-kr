---
제목: "SkiaSharp의 SVG 경로 데이터" 설명: "이 문서에서는 확장 가능한 벡터 그래픽 형식의 텍스트 문자열을 사용 하 여 SkiaSharp 경로를 정의 하는 방법을 설명 하 고 샘플 코드를 사용 하 여이를 보여 줍니다."
ms. prod: xamarin. 기술: xamarin-skiasharp assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2 author: davidbritch: dabritch: 05/24/2017:-loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="svg-path-data-in-skiasharp"></a>SkiaSharp의 SVG 경로 데이터

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_확장 가능한 벡터 그래픽 형식의 텍스트 문자열을 사용 하 여 경로 정의_

[`SKPath`](xref:SkiaSharp.SKPath)클래스는 SVG (스케일러블 벡터 그래픽) 사양에 의해 설정 된 형식으로 텍스트 문자열에서 전체 경로 개체의 정의를 지원 합니다. 이 문서의 뒷부분에서 텍스트 문자열에 있는 경로와 같은 전체 경로를 표시 하는 방법을 확인할 수 있습니다.

![](path-data-images/pathdatasample.png "A sample path defined with SVG path data")

SVG는 웹 페이지에 대 한 XML 기반 그래픽 프로그래밍 언어입니다. SVG는 일련의 함수 호출이 아닌 태그에서 경로를 정의할 수 있도록 허용 해야 하므로, SVG 표준에는 전체 그래픽 경로를 텍스트 문자열로 지정 하는 매우 간결한 방법이 포함 되어 있습니다.

SkiaSharp 내에서이 형식을 "SVG 경로-데이터" 라고 합니다. 이 형식은 Windows Presentation Foundation 및 유니버설 Windows 플랫폼를 비롯 하 여 Windows XAML 기반 프로그래밍 환경 에서도 지원 됩니다. 여기서 [경로 태그 구문](/dotnet/framework/wpf/graphics-multimedia/path-markup-syntax) 또는 [이동 및 그리기 명령 구문](/windows/uwp/xaml-platform/move-draw-commands-syntax/)이라고 합니다. 특히 XML과 같은 텍스트 기반 파일에서 벡터 그래픽 이미지에 대 한 교환 형식으로 사용할 수도 있습니다.

[`SKPath`](xref:SkiaSharp.SKPath)클래스는 이름에 단어가 포함 된 두 개의 메서드를 정의 합니다 `SvgPathData` .

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

정적 [`ParseSvgPathData`](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String)) 메서드는 문자열을 `SKPath` 개체로 변환 하 고,는 [`ToSvgPathData`](xref:SkiaSharp.SKPath.ToSvgPathData) 개체를 `SKPath` 문자열로 변환 합니다.

다음은 반지름이 100 인 점 (0, 0)을 중심으로 하는 5 개의 뾰족한 별에 대 한 SVG 문자열입니다.

```
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

문자는 개체를 빌드하는 명령입니다 `SKPath` .는 `M` 호출을 나타내고 `MoveTo` `L` 는 이며 `LineTo` `Z` `Close` 컨투어를 닫는 것입니다. 각 숫자 쌍은 점의 X 및 Y 좌표를 제공 합니다. `L`명령 다음에 쉼표로 구분 된 여러 점이 있습니다. 일련의 좌표와 점에서 쉼표와 공백은 동일 하 게 처리 됩니다. 일부 프로그래머는 요소가 아니라 X 좌표와 Y 좌표 사이에 쉼표를 추가 하는 것이 좋습니다. 단, 쉼표 또는 공백은 모호성을 방지 하는 데 필요 합니다. 이는 완벽 하 게 사용할 수 있습니다.

```
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

SVG 경로 데이터의 구문은 [svg 사양의 섹션 8.3](https://www.w3.org/TR/SVG11/paths.html#PathData)에 공식적으로 문서화 되어 있습니다. 요약 정보는 다음과 같습니다.

## <a name="moveto"></a>**이면**

```
M x y
```

이렇게 하면 현재 위치를 설정 하 여 경로에서 새 컨투어를 시작 합니다. 경로 데이터는 항상 명령으로 시작 해야 합니다 `M` .

## <a name="lineto"></a>**LineTo**

```
L x y ...
```

이 명령은 경로에 직선 또는 줄을 추가 하 고 새 현재 위치를 마지막 줄의 끝으로 설정 합니다. `L`여러 쌍의 *x* 및 *y* 좌표를 사용 하 여 명령을 수행할 수 있습니다.

## <a name="horizontal-lineto"></a>**수평 LineTo**

```
H x ...
```

이 명령은 경로에 가로선을 추가 하 고 새 현재 위치를 줄의 끝으로 설정 합니다. `H`다중 *x* 좌표를 사용 하 여 명령을 실행할 수 있지만이는 그다지 적합 하지 않습니다.

## <a name="vertical-line"></a>**세로줄**

```
V y ...
```

이 명령은 경로에 세로줄을 추가 하 고 새 현재 위치를 줄의 끝으로 설정 합니다.

## <a name="close"></a>**닫기**

```
Z
```

이 `C` 명령은 현재 위치에서 컨투어 시작 부분에 직선을 추가 하 여 컨투어를 닫습니다.

## <a name="arcto"></a>**ArcTo**

윤곽선에 타원형 호를 추가 하는 명령은 전체 SVG 경로-데이터 사양에서 가장 복잡 한 명령입니다. 숫자가 좌표 값이 아닌 다른 항목을 나타낼 수 있는 유일한 명령입니다.

```
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

*Rx* 및 *r)* 매개 변수는 타원의 가로 및 세로 반지름입니다. *회전 각도* 는 시계 방향 (도)입니다.

클 호에 대해 *큼 플래그* 를 1로 설정 하거나, 작은 호의 경우 0으로 설정 합니다.

*스윕 플래그* 를 시계 방향으로 1로 설정 하 고, 시계 반대 방향으로 0으로 설정 합니다.

호가 새 현재 위치가 되는 점 (*x*, *y*)에 그려집니다.

## <a name="cubicto"></a>**CubicTo**

```
C x1 y1 x2 y2 x3 y3 ...
```

이 명령은 현재 위치에서 (*x3*, *y3*)로 입방 형 3 차원 곡선을 추가 합니다 .이는 새로운 현재 위치가 됩니다. 요소 (*x1*, *y1*) 및 (*x2*, *y2*)는 제어점입니다.

단일 명령으로 여러 개의 베 지 어 곡선을 지정할 수 있습니다 `C` . 점의 수는 3의 배수 여야 합니다.

또한 "부드러운" 베 지 어 곡선 명령도 있습니다.

```
S x2 y2 x3 y3 ...
```

이 명령은 일반적인 베 지 어 명령 (반드시 필요한 것은 아니지만)을 따라야 합니다. 부드러운 베 지 어 명령은 첫 번째 제어점을 계산 하 여 해당 상호 지점을 중심으로 이전 Bezier의 두 번째 제어점을 반영 합니다. 따라서 이러한 세 가지 점이 colinear 두 베 지 어 곡선 간의 연결은 부드러운 것입니다.

## <a name="quadto"></a>**QuadTo**

```
Q x1 y1 x2 y2 ...
```

정방형 3 차원 곡선의 경우 점 수는 2의 배수 여야 합니다. 제어점은 (*x1*, *y1*)이 고 끝점 (및 새 현재 위치)은 (*x2*, *y2*)입니다.

또한 부드러운 정방형 곡선 명령도 있습니다.

```
T x2 y2 ...
```

제어 지점은 이전 정방형 곡선의 제어점을 기반으로 계산 됩니다.

이러한 모든 명령은 "상대" 버전 에서도 사용할 수 있습니다. 여기서 좌표 점은 현재 위치를 기준으로 합니다. 이러한 상대 명령은 알파벳 3 `c` `C` 차원 명령의 상대 버전이 아니라 소문자로 시작 합니다.

이는 SVG 경로 데이터 정의의 범위입니다. 명령 그룹을 반복 하거나 모든 유형의 계산을 수행 하는 기능은 없습니다. `ConicTo`또는 다른 형식의 호 사양에 대 한 명령은 사용할 수 없습니다.

정적 [`SKPath.ParseSvgPathData`](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String)) 메서드에는 유효한 SVG 명령 문자열이 필요 합니다. 구문 오류가 발견 되 면이 메서드는를 반환 `null` 합니다. 유일한 오류 표시입니다.

[`ToSvgPathData`](xref:SkiaSharp.SKPath.ToSvgPathData)메서드는 기존 개체의 SVG 경로 데이터를 가져와 `SKPath` 다른 프로그램으로 전송 하거나 XML과 같은 텍스트 기반 파일 형식으로 저장 하는 데 유용 합니다. `ToSvgPathData`이 문서의 샘플 코드에서는이 방법을 보여 줍니다. *not* `ToSvgPathData` 경로를 만든 메서드 호출에 해당 하는 문자열을 반환 하지 않을 것입니다. 특히 원호를 여러 명령으로 변환 하는 것을 확인할 수 `QuadTo` 있으며,이는에서 반환 된 경로 데이터에 표시 되는 방식입니다 `ToSvgPathData` .

**경로 데이터 Hello** 페이지는 SVG 경로 데이터를 사용 하 여 "Hello" 라는 단어를 출력 합니다. `SKPath`및 개체는 모두 `SKPaint` 클래스의 필드로 정의 됩니다 [`PathDataHelloPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) .

```csharp
public class PathDataHelloPage : ContentPage
{
    SKPath helloPath = SKPath.ParseSvgPathData(
        "M 0 0 L 0 100 M 0 50 L 50 50 M 50 0 L 50 100" +                // H
        "M 125 0 C 60 -10, 60 60, 125 50, 60 40, 60 110, 125 100" +     // E
        "M 150 0 L 150 100, 200 100" +                                  // L
        "M 225 0 L 225 100, 275 100" +                                  // L
        "M 300 50 A 25 50 0 1 0 300 49.9 Z");                           // O

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ...
}
```

텍스트 문자열을 정의 하는 경로는 지점 (0, 0)의 왼쪽 위 모퉁이에서 시작 합니다. 각 문자는 50 단위이 고 100 단위는 세로로 구분 되며 문자는 다른 25 단위로 구분 됩니다. 즉, 전체 경로는 350 단위 너비입니다.

"Hello"의 ' H '는 3 1 선 윤곽선으로 구성 되 고 ' E '는 두 개의 연결 된 입방 형 3 차원 곡선입니다. `C`명령 다음에는 6 개의 점이 있고, 두 제어점의 y 좌표는-10 및 110 이므로 다른 문자의 y 좌표 범위 밖에 배치 됩니다. ' L '은 두 개의 연결 된 줄 이지만 ' O '는 명령으로 렌더링 되는 타원입니다 `A` .

`M`마지막 컨투어를 시작 하는 명령은 위치를 포인트 (350, 50)로 설정 합니다 .이 위치는 ' O ' 왼쪽의 세로 가운데입니다. 명령 뒤의 첫 번째 숫자로 표시 된 것 처럼 `A` 타원의 가로 반경이 25이 고 세로 반지름이 50입니다. 끝점은 `A` (300, 49.9) 지점을 나타내는 명령의 마지막 숫자 쌍으로 표시 됩니다. 이것은 단순히 시작 지점과 약간 다릅니다. 끝점이 시작 지점과 동일 하 게 설정 된 경우 호가 렌더링 되지 않습니다. 전체 타원을 그리려면 끝점을 시작점 (또는 같지 않음)으로 설정 해야 합니다. 그렇지 않으면 `A` 전체 타원의 일부에 대해 둘 이상의 명령을 사용 해야 합니다.

다음 문을 페이지의 생성자에 추가 하 고 결과 문자열을 검사 하는 중단점을 설정 하는 것이 좋습니다.

```csharp
string str = helloPath.ToSvgPathData();
```

`Q`근 베 지 어 곡선을 사용 하 여 호의 증분 근사값에 대 한 긴 일련의 명령으로 호가 대체 되었음을 알 수 있습니다.

`PaintSurface`처리기는 ' E ' 및 ' O ' 곡선에 대 한 제어점을 포함 하지 않는 경로의 긴밀 한 경계를 가져옵니다. 세 가지 변환은 경로의 중심을 점 (0, 0)으로 이동 하 고, 경로를 캔버스의 크기로 조정 하 고, 패스의 중심을 캔버스의 가운데로 이동 합니다.

```csharp
public class PathDataHelloPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect bounds;
        helloPath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(info.Width / (bounds.Width + paint.StrokeWidth),
                     info.Height / (bounds.Height + paint.StrokeWidth));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(helloPath, paint);
    }
}
```

이 경로는 가로 모드로 볼 때 더 합리적인 모양의 캔버스를 채웁니다.

[![](path-data-images/pathdatahello-small.png "Triple screenshot of the Path Data Hello page")](path-data-images/pathdatahello-large.png#lightbox "Triple screenshot of the Path Data Hello page")

**데이터 Cat 경로** 페이지도 유사 합니다. 경로 및 그리기 개체는 모두 클래스의 필드로 정의 됩니다 [`PathDataCatPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) .

```csharp
public class PathDataCatPage : ContentPage
{
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

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 5
    };
    ...
}
```

고양이 헤드는 원 이며, 여기서는 `A` 각각 반원를 그리는 두 개의 명령으로 렌더링 됩니다. `A`헤드에 대 한 두 명령 모두 100의 가로 및 세로 반지름을 정의 합니다. 첫 번째 호는 (240, 100)에서 시작 하 고 (240, 300)에서 종료 됩니다 .이는 (240, 100)에서 종료 되는 두 번째 원호의 시작점입니다.

두 개의 눈동자는 두 개의 명령으로 렌더링 되 `A` 고, cat의 head와 마찬가지로 두 번째 `A` 명령은 첫 번째 명령의 시작과 동일한 지점에서 끝납니다 `A` . 그러나 이러한 `A` 명령 쌍은 타원을 정의 하지 않습니다. 각 호의를 포함 하는은 40 단위 이며 반지름은 40 단위 이기도 합니다. 즉, 이러한 원호는 전체 semicircles 되지 않습니다.

`PaintSurface`처리기는 이전 샘플과 유사한 변환을 수행 하지만, `Scale` 가로 세로 비율을 유지 하 고 작은 여백을 제공 하도록 단일 요소를 설정 하 여 cat의 수염 화면 측면을 건드리지 않도록 합니다.

```csharp
public class PathDataCatPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        SKRect bounds;
        catPath.GetBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(0.9f * Math.Min(info.Width / bounds.Width,
                                     info.Height / bounds.Height));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(catPath, paint);
    }
}
```

실행 중인 프로그램은 다음과 같습니다.

[![](path-data-images/pathdatacat-small.png "Triple screenshot of the Path Data Cat page")](path-data-images/pathdatacat-large.png#lightbox "Triple screenshot of the Path Data Cat page")

일반적으로 `SKPath` 개체가 필드로 정의 된 경우 경로의 컨투어는 생성자 또는 다른 메서드에서 정의 해야 합니다. 그러나 SVG 경로 데이터를 사용 하는 경우 필드 정의에서 경로를 완전히 지정할 수 있습니다.

[**회전 변환**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) 문서의 이전에는 **못 된 아날로그 클록** 샘플에서 Clock의 손을 간단한 선으로 표시 했습니다. 아래 **아날로그 클록** 프로그램은 이러한 줄을 `SKPath` 클래스의 필드로 정의 된 개체와 [`PrettyAnalogClockPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) 함께 개체와 바꿉니다 `SKPaint` .

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    // Clock hands pointing straight up
    SKPath hourHandPath = SKPath.ParseSvgPathData(
        "M 0 -60 C   0 -30 20 -30  5 -20 L  5   0" +
                "C   5 7.5 -5 7.5 -5   0 L -5 -20" +
                "C -20 -30  0 -30  0 -60 Z");

    SKPath minuteHandPath = SKPath.ParseSvgPathData(
        "M 0 -80 C   0 -75  0 -70  2.5 -60 L  2.5   0" +
                "C   2.5 5 -2.5 5 -2.5   0 L -2.5 -60" +
                "C 0 -70  0 -75  0 -80 Z");

    SKPath secondHandPath = SKPath.ParseSvgPathData(
        "M 0 10 L 0 -80");

    // SKPaint objects
    SKPaint handStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2,
        StrokeCap = SKStrokeCap.Round
    };

    SKPaint handFillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Gray
    };
    ...
}
```

이제 시간 및 분 바늘에는 포함 된 영역이 있습니다. 이러한 손을 서로 구분할 수 있도록 및 개체를 사용 하 여 검정 윤곽선과 회색 채우기를 모두 사용 하 여 그립니다 `handStrokePaint` `handFillPaint` .

이전에는 **못 된 아날로그 클록** 샘플에서 시간 및 분으로 표시 된 작은 원이 루프에서 그려집니다. 이 처럼 **아날로그 클록** 샘플에서는 완전히 다른 방법이 사용 됩니다. 시간 및 분 표시는 `minuteMarkPaint` 및 개체를 사용 하 여 그려진 점선입니다 `hourMarkPaint` .

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    SKPaint minuteMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 3 * 3.14159f }, 0)
    };

    SKPaint hourMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 6,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 15 * 3.14159f }, 0)
    };
    ...
}
```

[**점 및 대시**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) 문서에서는 메서드를 사용 하 여 파선을 만드는 방법에 대해 설명 [`SKPathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash*) 했습니다. 첫 번째 인수는 `float` 일반적으로 두 개의 요소가 있는 배열입니다. 첫 번째 요소는 대시의 길이이 고 두 번째 요소는 대시 사이의 간격입니다. `StrokeCap`속성이로 설정 되 면 대시의 `SKStrokeCap.Round` 둥근 끝에서 대시의 양쪽에 있는 스트로크 너비로 대시 길이를 효과적으로 늘립니다. 따라서 첫 번째 배열 요소를 0으로 설정 하면 점선이 만들어집니다.

이러한 점 사이의 거리는 두 번째 배열 요소에 의해 제어 됩니다. 잠시 후에 이러한 두 `SKPaint` 개체를 사용 하 여 반지름이 90 단위인 원을 그립니다. 따라서이 원의 원주는 180 π입니다. 즉, 60 분 표시는에서 배열의 두 번째 값인 3π 단위 마다 표시 되어야 합니다 `float` `minuteMarkPaint` . 12 시간 표시는 두 번째 배열의 값인 15π 단위 마다 표시 되어야 합니다 `float` .

`PrettyAnalogClockPage`클래스는 16 밀리초 마다 표면을 무효화 하는 타이머를 설정 하 고, `PaintSurface` 해당 속도에서 처리기를 호출 합니다. 이전 버전의 `SKPath` 및 개체 정의를 `SKPaint` 통해 매우 깔끔한 그리기 코드를 사용할 수 있습니다.

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Transform for 100-radius circle in center
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / 200, info.Height / 200));

        // Draw circles for hour and minute marks
        SKRect rect = new SKRect(-90, -90, 90, 90);
        canvas.DrawOval(rect, minuteMarkPaint);
        canvas.DrawOval(rect, hourMarkPaint);

        // Get time
        DateTime dateTime = DateTime.Now;

        // Draw hour hand
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawPath(hourHandPath, handStrokePaint);
        canvas.DrawPath(hourHandPath, handFillPaint);
        canvas.Restore();

        // Draw minute hand
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawPath(minuteHandPath, handStrokePaint);
        canvas.DrawPath(minuteHandPath, handFillPaint);
        canvas.Restore();

        // Draw second hand
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        canvas.Save();
        canvas.RotateDegrees(6 * (dateTime.Second + (float)t));
        canvas.DrawPath(secondHandPath, handStrokePaint);
        canvas.Restore();
    }
}
```

그러나 특별 한 작업은 두 번째 손으로 수행 됩니다. 클록은 16 밀리초 마다 업데이트 되므로 `Millisecond` 값의 속성을 사용 하 여 `DateTime` 불연속 점프에서 second로 이동 하는 것이 아니라 스윕 초에 애니메이션 효과를 줄 수 있습니다. 그러나이 코드는 이동이 원활 하 게 진행 되는 것을 허용 하지 않습니다. 대신, Xamarin.Forms [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) 및 [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) 애니메이션 감속/가속 함수를 사용 하 여 다른 종류의 움직임을 사용 합니다. 이러한 감속/가속 함수는 두 번째 손을 jerkier 방식으로 이동 하 여 이동 하기 전에 약간의 작업을 수행 하는 방식으로 이동 하 &mdash; 고, 해당 대상을 약간 과도 하 게 전환 하 여 이러한 정적 스크린샷에서 재현할 수 없는 효과를 발생 시킵니다.

[![](path-data-images/prettyanalogclock-small.png "Triple screenshot of the Pretty Analog Clock page")](path-data-images/prettyanalogclock-large.png#lightbox "Triple screenshot of the Pretty Analog Clock page")

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
