---
title: SkiaSharp에서 SVG 경로 데이터
description: 이 문서는 확장성이 뛰어난 벡터 그래픽 형식으로 텍스트 문자열을 사용 하 여 SkiaSharp 경로 정의 하는 방법에 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2017
ms.openlocfilehash: 467863dba2f5757e0590ccf64927ae2af292f285
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770568"
---
# <a name="svg-path-data-in-skiasharp"></a>SkiaSharp에서 SVG 경로 데이터

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_텍스트 문자열을 사용 하 여 확장 가능한 벡터 그래픽 형식으로 경로 정의 합니다._

합니다 [ `SKPath` ](xref:SkiaSharp.SKPath) 클래스 형식으로 확장 가능한 SVG (벡터 그래픽) 사양에 설정 된 텍스트 문자열의 전체 경로 개체의 정의 지원 합니다. 이 문서의 뒷부분에 나오는 같은 텍스트 문자열의 전체 경로 나타낼 수는 어떻게 표시 됩니다.

![](path-data-images/pathdatasample.png "SVG 경로 데이터를 사용 하 여 정의 하는 샘플 경로")

SVG는 웹 페이지에 대 한 언어를 프로그래밍 하는 XML 기반 그래픽입니다. SVG 일련의 함수 호출 보다는 태그에 정의 해야 하는 경로 허용 해야 하기 때문에 표준 SVG은 텍스트 문자열로 전체 그래픽 경로 지정 하는 매우 간단 하 게 포함 되어 있습니다.

SkiaSharp, 내에서이 형식으로 라고 "SVG 경로 데이터입니다." 형식은 Windows XAML 기반 프로그래밍 환경의 Windows Presentation Foundation 등으로 알려져 있는 유니버설 Windows 플랫폼 에서도 지원 됩니다 합니다 [경로 태그 구문](/dotnet/framework/wpf/graphics-multimedia/path-markup-syntax) 또는 [이동 그리기 명령 구문 및](/windows/uwp/xaml-platform/move-draw-commands-syntax/)합니다. 또한 XML과 같은 텍스트 기반 파일에서 특히 벡터 그래픽 이미지에 대 한 교환 형식으로 제공할 수 있습니다.

합니다 [ `SKPath` ](xref:SkiaSharp.SKPath) 단어를 사용 하 여 두 개의 메서드를 정의 하는 클래스 `SvgPathData` 이름에:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

정적 [ `ParseSvgPathData` ](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String)) 메서드를 문자열로 변환 합니다는 `SKPath` 개체를 하는 동안 [ `ToSvgPathData` ](xref:SkiaSharp.SKPath.ToSvgPathData) 변환는 `SKPath` 개체를 문자열입니다.

점이 5 별 지점을 중심으로 (0, 0)는 radius 사용 하 여 100의 SVG 문자열을 다음과 같습니다.

```
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

문자는 구축 하는 명령은 `SKPath` 개체: `M` 나타냅니다는 `MoveTo` 를 호출 `L` 는 `LineTo`, 및 `Z` 는 `Close` 윤곽선을 닫습니다. 각 번호 쌍 점의 X 및 Y 좌표를 제공합니다. 다음에 유의 합니다 `L` 명령 뒤에 쉼표로 구분 하 여 여러 지점입니다. 일련의 좌표, 지점, 쉼표 및 공백을 동일 하 게 처리 됩니다. 일부 프로그래머는 X 및 Y 좌표 간에 아니라는 점 사이 쉼표를 추가할 수 있지만 쉼표나 공백을 모호성을 피하기 위해만 필요 합니다. 이 완벽 하 게 유효합니다.

```
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

SVG 경로 데이터의 구문을 공식적으로 문서화 [SVG 사양의 섹션 8.3](http://www.w3.org/TR/SVG11/paths.html#PathData)합니다. 요약 정보는 다음과 같습니다.

## <a name="moveto"></a>**MoveTo**

```
M x y
```

현재 위치를 설정 하 여 경로에 새 윤곽선 시작 됩니다. 경로 데이터는 항상로 시작 되는 `M` 명령입니다.

## <a name="lineto"></a>**LineTo**

```
L x y ...
```

이 명령은 경로에 직선 (또는 줄)를 추가 하 고 마지막 줄의 끝에 새 현재 위치를 설정 합니다. 따르면 합니다 `L` 여러 쌍의 명령과 *x* 및 *y* 좌표입니다.

## <a name="horizontal-lineto"></a>**가로 LineTo**

```
H x ...
```

이 명령은 경로에 가로줄을 추가 하 고 줄의 끝에 새 현재 위치를 설정 합니다. 따를 수 있습니다 합니다 `H` 여러 개의 명령 *x* 좌표 있지만 대부분이 잘 이해 하지 않습니다.

## <a name="vertical-line"></a>**수직선**

```
V y ...
```

이 명령은 경로에 세로 줄을 추가 하 고 줄의 끝에 새 현재 위치를 설정 합니다.

## <a name="close"></a>**닫기**

```
Z
```

`C` 명령은 현재 위치에서 윤곽선의 시작 부분에 직선을 추가 하 여 윤곽선을 닫습니다.

## <a name="arcto"></a>**ArcTo**

타원형 호 윤곽선을 추가 하는 명령을 전체 SVG 경로 데이터 사양에서 가장 복잡 한 명령을 단연입니다. 유일한 명령이 있는 숫자를 나타낼 수 있습니다 좌표 값 이외의 것:

```
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

합니다 *rx* 하 고 *ry* 매개 변수는 타원의 가로 및 세로 반지름입니다. 합니다 *회전 각도* 도 시계 방향으로 됩니다.

설정 된 *큰 호 플래그* 큰 호에는 1 또는 0 작은 호에 대 한 합니다.

설정 합니다 *스윕 플래그* 1에 시계 방향으로 및 0으로 시계 반대 방향으로 합니다.

지점 호가 그려지는 (*x*, *y*)을 새 현재 위치 됩니다.

## <a name="cubicto"></a>**CubicTo**

```
C x1 y1 x2 y2 x3 y3 ...
```

이 명령은 현재 위치에서 입방 형 3 차원 큐빅 곡선을 추가 (*x3*를 *y3*)을 새 현재 위치 됩니다. 요소 (*x1*를 *y1*) 및 (*x2*를 *y2*) 제어점입니다.

여러 베 지 어 곡선으로 단일 지정할 수 있습니다 `C` 명령입니다. 점 개수 3의 배수 여야 합니다.

"부드러운" 베 지 어 곡선 명령이 이기도합니다.

```
S x2 y2 x3 y3 ...
```

이 명령은 (엄밀히 필요한) 하지만 일반 베 지 어 명령을 따라야 합니다. 부드러운 베 지 어 명령을 상호 지점과 관련 된 이전 베 지 어의 두 번째 제어점의 반사 되도록 첫 번째 제어점을 계산 합니다. 이러한 3 개의 점으로 되며 동일 선상의 따라서 두 베 지 어 곡선으로 분할 간의 연결이 부드러운.

## <a name="quadto"></a>**QuadTo**

```
Q x1 y1 x2 y2 ...
```

정방형 베 지 어 곡선에 대 한 지점의 수에 2의 배수 여야 합니다. 시작 하는 (*x1*, *y1*) 끝점 (및 새 현재 위치) 이며 (*x2*하십시오 *y2*)

부드러운 정방형 곡선 명령을 이기도합니다.

```
T x2 y2 ...
```

제어점은 이전 정방형 곡선의 제어점을 기준으로 계산 됩니다.

이러한 모든 명령이 사용할 수 있습니다 "상대" 버전에서 현재 위치를 기준으로 좌표 지점 있는 합니다. 예를 들어 이러한 상대 명령을 소문자를 사용 하 여 시작 `c` 대신 `C` 입방 형 3 차원 곡선 명령의 상대 버전에 대 한 합니다.

SVG 경로 데이터 정의의 범위입니다. 반복 되는 명령 그룹 또는 모든 유형의 계산을 수행 하기 위한 기능이 없습니다. 에 대 한 명령 `ConicTo` 했거나 다른 유형의 호 사양 사용할 수 없습니다.

정적 [ `SKPath.ParseSvgPathData` ](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String)) 메서드에 SVG 명령의 유효한 문자열이 필요 합니다. 메서드가 반환 하는 경우 구문 오류가 감지 되 면 `null`합니다. 오류 표시입니다.

[ `ToSvgPathData` ](xref:SkiaSharp.SKPath.ToSvgPathData) 메서드는 기존 SVG 경로 데이터를 가져오기 위한 편리한 `SKPath` 다른 프로그램에 전송할 또는 XML과 같은 텍스트 기반 파일 형식으로 저장 하는 개체입니다. (의 `ToSvgPathData` 메서드는이 문서의 샘플 코드에서 다루지 않습니다.) 수행할 *되지* 예상 `ToSvgPathData` 경로 생성 하는 메서드 호출을 정확 하 게 해당 문자열을 반환 합니다. 여러 원호는 변환 함을 알게 하는 특히 `QuadTo` 명령에서 반환 되는 경로 데이터에 표시 되는 방식입니다 `ToSvgPathData`합니다.

합니다 **경로 데이터 Hello** 페이지 단어가 마법 "HELLO" SVG 경로 데이터를 사용 하 여 합니다. 모두를 `SKPath` 하 고 `SKPaint` 개체의 필드로 정의 된 합니다 [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) 클래스:

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

텍스트 문자열을 정의 하는 경로 지점 (0, 0)에서 왼쪽 위 모퉁이에서 시작 됩니다. 각 문자는 와이드 단위는 50 및 100 단위에 불과하지만 하 고 전체 경로 350 단위 와이드 문자 다른 25 단위로 구분 됩니다.

'Hello"'H'는 'E'는 두 개의 연결 된 3 차원 큐빅 곡선 세 줄 윤곽 구성 됩니다. 다음에 유의 합니다 `C` 명령 뒤에 6 개의 점 있고 두 제어점 – 10 및 기타 문자 Y 좌표를 범위 밖에 놓입니다 110의 Y 좌표입니다. 'L'은 두 개의 연결 된 선 하는 동안는 ' o '가 사용 하 여 렌더링 되는 타원을 `A` 명령입니다.

`M` 위치 왼쪽의 세로 가운데 인 점 (350, 50)를 설정 하는 마지막 윤곽선을 시작 하는 명령을 측의 ' o '입니다. 첫 번째 숫자 다음에 표시 된 대로 `A` 명령을 타원에 25 가로 반지름 및 50 세로 반지름입니다. 끝점에서 숫자의 마지막 쌍으로 표시 됩니다는 `A` 점 (300, 49.9)를 나타내는 명령입니다. 시작 지점에서 의도적으로 약간 다릅니다. 끝점 시작점 같음으로 설정 된 경우 원호 렌더링 되지 않습니다. 완전 한 타원을 그릴 설정한 끝점에 닫기 (같지 않음) 그러나 시작점을 하거나 두 개 이상 사용 해야 `A` 완전 한 타원의 부분에 대 한 각 명령입니다.

페이지의 생성자에 다음 문을 추가 하 고 결과 문자열을 검사 하려면 중단점을 설정 하는 것이 좋습니다.

```csharp
string str = helloPath.ToSvgPathData();
```

일련의 긴를 사용 하 여 원호를 교체한 알게 `Q` 정방형 베 지 어 곡선을 사용 하 여 원호의 증분 근사값에 대 한 명령입니다.

`PaintSurface` 처리기 'E'에 대 한 제어점을 포함 하지 않는 경로 긴밀 하 게 범위를 가져옵니다 하 고 ' 곡선 o '입니다. 세 가지 변환 경로의 가운데 점 (0, 0), 캔버스 (하지만 또한 고려 스트로크 너비)의 크기에 대 한 경로 확장 이동한 다음 경로의 가운데를 캔버스의 가운데 위로 이동:

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

경로 보이는 가로 모드로 볼 때 적절 한 캔버스를 채웁니다.

[![](path-data-images/pathdatahello-small.png "경로 데이터 Hello 페이지 스크린샷 삼중")](path-data-images/pathdatahello-large.png#lightbox "삼중 경로 데이터 Hello 페이지 스크린샷")

합니다 **경로 데이터 Cat** 페이지는와 유사 합니다. 경로 및 그리기 개체 모두에 필드로 정의 된 [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) 클래스:

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

헤드 고양이 그림이 그려 원 이며 두 개를 사용 하 여 렌더링 되는 여기 `A` 그리는 반원 명령입니다. 둘 다 `A` 100의 가로 및 세로 반지름을 정의 하는 head에 대 한 명령입니다. 첫 번째 호에서 시작 (240, 100)에서 끝납니다 (240, 300) 다시 종료 하는 두 번째 호에 대 한 시작점 됩니다 (240, 100).

두 대는 또한 두 개의 렌더링 `A` 명령 및 고양이 head와 마찬가지로 두 번째 `A` 명령은 첫 번째 시작으로 동일한 지점에서 끝납니다 `A` 명령입니다. 그러나 이러한 한 쌍의 `A` 명령을 타원을 정의 하지 않습니다. 사용 하 여 각 원호의 40 단위 이며 반지름도 40 단위, 즉, 이러한 원호 전체 semicircles 없는 합니다.

합니다 `PaintSurface` 처리기 앞의 예제에서와 비슷한 변환을 수행 하지만 단일 설정 `Scale` 가로 세로 비율을 유지 하 고 고양이 수염에 대 한 화면의 측면을 건드리지 않을 작은 여백을 제공 요소:

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

실행 중인 프로그램이 다음과 같습니다.

[![](path-data-images/pathdatacat-small.png "경로 데이터 Cat 페이지 스크린샷 삼중")](path-data-images/pathdatacat-large.png#lightbox "삼중 경로 데이터 Cat 페이지 스크린샷")

일반적으로,는 `SKPath` 개체가 필드로 정의 된, 경로 윤곽 다른 메서드나 생성자에 정의 되어야 합니다. 그러나 SVG 경로 데이터를 사용할 때 살펴보았습니다 필드 정의에 전체 경로 지정할 수 있습니다.

이전 **까다로운 아날로그 클록** 샘플을 [ **The 회전 변환** ](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) 문서 시계 바늘을 간단한 선으로 표시 합니다. 합니다 **아날로그 클록 상당히** 아래 프로그램을 사용 하 여 해당 줄 대체 `SKPath` 필드로 정의 된 개체를 [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) 클래스와 함께 `SKPaint` 개체:

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

이제 시간 및 분 손을 영역 포함 했습니다. 이러한 실습 서로 구별을 하려면 black 개요 및 사용 하 여 회색 채우기를 사용 하 여 그릴 합니다 `handStrokePaint` 및 `handFillPaint` 개체입니다.

이전 **까다로운 아날로그 클록** 샘플 작은 원을 표시 시간 및 분 루프에 그려진 합니다. 이 **아날로그 클록 상당히** 샘플에 사용 되는 완전히 다른 방법: 시간 및 분 표시는 점선으로 사용 하 여 그린 합니다 `minuteMarkPaint` 및 `hourMarkPaint` 개체:

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

합니다 [ **점 및 대시** ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) 문서 사용 하는 방법을 설명 합니다 [ `SKPathEffect.CreateDash` ](xref:SkiaSharp.SKPathEffect.CreateDash*) 파선을 만드는 방법. 첫 번째 인수는 일반적 `float` 으로 두 개의 요소를 포함 하는 배열입니다. 첫 번째 요소는 대시의 길이 이며 두 번째 요소는 대시 사이의 간격입니다. 경우는 `StrokeCap` 속성이 `SKStrokeCap.Round`, 둥근된 끝 대시 대시의 양쪽에서 스트로크 너비에 따라 대시 길이 효과적으로 연장 합니다. 따라서 첫 번째 배열 요소를 0으로 설정 된 점선을 만듭니다.

이러한 점 사이의 거리는 두 번째 배열 요소에 의해 제어 됩니다. 알 수 있듯이 곧 이러한 두 `SKPaint` 개체 90 단위의 반지름 원을 그리는 데 사용 됩니다. 이 원의 원주 60 분 표시 모든 3 π 단위 표시 되어야 함을 의미 합니다 180π 되므로에서 두 번째 값을 `float` 배열을 `minuteMarkPaint`합니다. 12 시간 표시 나타나야 모든 15π 단위를 두 번째에서 값인 `float` 배열입니다.

합니다 `PrettyAnalogClockPage` 클래스 16 밀리초 마다 화면을 무효화 합니다. 타이머를 설정 합니다 및 `PaintSurface` 속도 처리기가 호출 됩니다. 이전 정의 `SKPath` 고 `SKPaint` 개체 수에 대 한 그리기 코드 정리:

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

그러나 한편으로 두 번째를 사용 하 여 수행 됩니다 특별 한. 시계는 업데이트 되므로 16 밀리초 마다를 `Millisecond` 의 속성을 `DateTime` 값의 개별 점프 이동 하는 대신 스윕을 수동으로 두 번째 애니메이션 효과를 잠재적으로 사용할 수 있습니다 초 초에서. 하지만이 코드 이어갈 수 하는 움직임을 허용 하지 않습니다. 대신를 사용 하 여 Xamarin.Forms [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) 하 고 [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) 애니메이션 감속/가속 함수는 다른 종류의 이동에 대 한 합니다. 감속/가속 함수가 양식 화면 떨림 방식으로 이동할 초침 &mdash; 끌어오기 다시 작은 이동한 다음 약간 과도 하 게 해결 목적지 효과 안타깝게도 재현할 수 없는 정적이 스크린 샷에 전에:

[![](path-data-images/prettyanalogclock-small.png "삼중 아날로그 시계 매우 페이지 스크린샷")](path-data-images/prettyanalogclock-large.png#lightbox "삼중 아날로그 시계 매우 페이지 스크린샷")

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
