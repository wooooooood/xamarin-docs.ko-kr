---
title: "SVG 경로 데이터입니다."
description: "텍스트 문자열을 사용 하 여 확장 가능한 벡터 그래픽 형식으로 경로 정의 합니다."
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/24/2017
ms.openlocfilehash: feb4c5f4c7e7ad3fc5f762786001be9aa57ae718
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="svg-path-data"></a>SVG 경로 데이터입니다.

_텍스트 문자열을 사용 하 여 확장 가능한 벡터 그래픽 형식으로 경로 정의 합니다._

`SKPath` 클래스 그래픽 SVG (Scalable Vector) 사양에서 설정 된 형식이 텍스트 문자열에서 전체 경로 개체의 정의 지원 합니다. 이 문서의 뒷부분에 나오는 텍스트 문자열에 같은이 전체 경로 표시 하는 방법 표시 됩니다.

![](path-data-images/pathdatasample.png "SVG 경로 데이터를 사용 하 여 정의 하는 샘플 경로")

SVG는 웹 페이지에 대 한 언어를 프로그래밍 하는 XML 기반 그래픽입니다. SVG 일련의 함수 호출 보다는 태그에서 정의 해야 하는 경로 허용 해야 하기 때문에 표준 SVG은 텍스트 문자열로 전체 그래픽 경로 지정 하는 매우 간단 하 게 포함 되어 있습니다.

SkiaSharp, 내에서이 형식을 라고 SVG 경로-"데이터"입니다. 형식은 Windows XAML 기반 프로그래밍 환경의 Windows Presentation Foundation 등으로 알려져 있는 유니버설 Windows 플랫폼 에서도 지원 됩니다는 [경로 태그 구문](https://msdn.microsoft.com/library/ms752293%28v=vs.110%29.aspx) 또는 [이동 그릴 명령 구문 및](/windows/uwp/xaml-platform/move-draw-commands-syntax/)합니다. 특히 XML과 같은 텍스트 기반 파일에서에서의 벡터 그래픽 이미지는 교환 형식으로는 사용할 수 있습니다.

SkiaSharp 단어가 있는 두 개의 메서드를 정의 합니다. `SvgPathData` 이름에:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

정적 [ `ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) 메서드를 문자열로 변환 된 `SKPath` 개체를 동안 [ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) 변환는 `SKPath` 개체를 문자열로 합니다.

다음은 점이 5 별 지점을 중심으로 (0, 0) radius와 100 대 한 SVG 문자열이입니다.

```csharp
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

문자는 명령을 생성 하는 `SKPath` 개체입니다. `M` 나타냅니다는 `MoveTo` 호출, `L` 은 `LineTo`, 및 `Z` 은 `Close` 를 윤곽선을 닫습니다. 각 번호 쌍을 점의 X 및 Y 좌표를 제공합니다. 다음에 유의 `L` 명령 쉼표로 구분 하 여 여러 지점 나옵니다. 일련의 좌표와 점, 쉼표 및 공백은 동일 하 게 처리 됩니다. 일부 프로그래머 지점, 사이 있지 않고 X 및 Y 좌표 사이 쉼표를 배치 하는 것을 선호 하지만 쉼표나 공백과 모호성을 피하기 위해만 필요 합니다. 이 완벽 하 게 올바릅니다.

```csharp
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

SVG 경로 데이터의 구문을 공식적으로 문서화 [SVG 사양의 섹션 8.3](http://www.w3.org/TR/SVG11/paths.html#PathData)합니다. 다음은 요약이입니다.

## <a name="moveto"></a>**MoveTo**

```csharp
M x y
```

현재 위치를 설정 하 여 경로에 새 윤곽선을 시작 합니다. 경로 데이터는 항상으로 시작 해야는 `M` 명령입니다.

## <a name="lineto"></a>**LineTo**

```csharp
L x y ...
```

이 명령은 경로에 직선 (또는 줄)를 추가 하 고 마지막 줄의 끝에 새 현재 위치를 설정 합니다. 참고할 수는 `L` 여러 쌍의 명령을 *x* 및 *y* 좌표입니다.

## <a name="horizontal-lineto"></a>**가로 LineTo**

```csharp
H x ...
```

이 명령은 경로에 가로 줄을 추가 하 고 줄의 끝에 새 현재 위치를 설정 합니다. 참고할 수는 `H` 여러 명령 *x* 좌표 하지만 고정할 수 없습니다.

## <a name="vertical-line"></a>**세로 라인**

```csharp
V y ...
```

이 명령은 경로에 세로 줄을 추가 하 고 줄의 끝에 새 현재 위치를 설정 합니다.

## <a name="close"></a>**닫기**

```csharp
Z
```

`C` 명령은 현재 위치에서 윤곽선의 시작 부분에 직선을 추가 하 여 윤곽을 닫습니다.

## <a name="arcto"></a>**ArcTo**

타원형 호 윤곽선에 추가할 명령을 전체 SVG path-지정 된 데이터에서에서 가장 복잡 한 명령을 지금까지 경우 것은는 숫자 나타낼 수 좌표 값 이외의 하는 유일한 명령입니다.

```csharp
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

*rx* 및 *시도* 매개 변수는 타원의 가로 및 세로 반지름입니다. *회전 각도* 도 시계 방향으로 됩니다.

설정의 *호 플래그 큰* 큰 호에 대 한 1 또는 0 작은 호입니다.

설정의 *스윕 플래그* 1에 시계 방향으로 회전 하 고 0으로 시계 반대 방향으로 합니다.

지점에 호를 그릴 (*x*, *y*)을 새 현재 위치 됩니다.

## <a name="cubicto"></a>**CubicTo**

```csharp
C x1 y1 x2 y2 x3 y3 ...
```

이 명령은를 현재 위치에서 있는 입방 형 3 차원 곡선을 추가 (*x3*, *y3*)을 새 현재 위치 됩니다. 점 (*x1*, *y1*) 및 (*x2*, *y2*) 제어점입니다.

단일 여러 베 지 어 곡선으로 분할을 지정할 수 있습니다 `C` 명령입니다. 점 개수는 3의 배수 여야 합니다.

"부드러운" 베 지 어 곡선 명령이 이기도합니다.

```csharp
S x2 y2 x3 y3 ...
```

이 명령은 (하지만 반드시 필요한가 있는) 일반 베 지 어 명령을 따라야 합니다. 부드러운 베 지 어 명령이 해당 상호 점을 이전 베 지 어의 두 번째 제어점 반영 되도록 첫 번째 제어점을 계산 합니다. 이러한 세 점 모두가 동일 선상의 따라서 및는 두 베 지 어 곡선으로 분할 간의 연결이 부드러운 합니다.

## <a name="quadto"></a>**QuadTo**

```csharp
Q x1 y1 x2 y2 ...
```

정방형 베 지 어 곡선에 대 한 점 개수는 2의 배수 여야 합니다. 시작 하는 (*x1*, *y1*) 끝점 (및 새 현재 위치)는 (*x2*, *y2*)

정방형 곡선을 부드럽게 명령 이기도합니다.

```csharp
T x2 y2 ...
```

이전 정방형 곡선의 제어점 제어 지점은 기반으로 계산 됩니다.

이러한 모든 명령을 수 또한 "상대" 버전에서 사용할 수 있는 좌표 점 현재 위치를 기준으로 합니다. 예를 들어 상대 이러한 명령은 소문자 문자로 시작 `c` 대신 `C` 입방 형 3 차원 곡선 명령의 상대적 버전에 대 한 합니다.

SVG 경로 데이터 정의의 범위입니다. 반복 되는 명령 그룹 또는 모든 유형의 계산을 수행 하기 위한 기능이 있습니다. 에 대 한 명령 `ConicTo` 했거나 다른 유형의 호 사양 사용할 수 없습니다.

정적 [ `SKPath.ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) 메서드에 유효한 문자열이 SVG 명령의 필요 합니다. 메서드가 반환 하는 경우 구문 오류가 검색 되 면 `null`합니다. 유일한 오류 표시입니다.

[ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) 메서드는 기존에서 SVG 경로 데이터를 가져오기 하는 데 편리 `SKPath` 개체를 다른 프로그램으로 전송 또는 XML과 같은 텍스트 기반 파일 형식으로 저장 합니다. (의 `ToSvgPathData` 이 문서에 대 한 샘플 코드에서 메서드를 설명 하지 않음.) 수행 *하지* 예상 `ToSvgPathData` 경로 생성 하는 메서드 호출에 정확 하 게 해당 문자열을 반환 합니다. 특히, 배워 호 배수 값으로 변환 됩니다 `QuadTo` 명령에서 반환 된 경로 데이터에 표시 되는 방식 즉 `ToSvgPathData`합니다.

**경로 데이터 Hello** 페이지 마법 해당 단어 "HELLO" SVG 경로 데이터를 사용 하 여 합니다. 둘 다는 `SKPath` 및 `SKPaint` 개체의 필드로 정의 되는 [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) 클래스:

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

텍스트 문자열을 정의 하는 경로 지점 (0, 0)에서 왼쪽 위 모퉁이에서 시작 됩니다. 각 문자가 50 명의 단위 너비 및 높이 100 단위 이며 전체 경로 350 단위 와이드 문자 다른 25 단위로 구분 됩니다.

'Hello"의 'H'는 'E'는 두 개의 연결 된 입방 형 3 베 지 어 곡선 세 줄 윤곽선의 구성 됩니다. 에 `C` 명령 뒤에 6 개의 점으로 붙고 – 10과 다른 문자의 Y 좌표의 범위 밖에 있는 놓입니다 110의 Y 좌표를 갖는 두 개의 제어점입니다. 'L'은 두 개의 연결 된 선 동안는 ' o '와 함께 렌더링 되는 타원을가 `A` 명령입니다.

에 `M` 위치 왼쪽의 세로 가운데 인 점 (350, 50)를 설정 하는 마지막 윤곽을 시작 하는 명령을 쪽의 ' o '. 첫 번째 숫자 다음에 표시 된 대로 `A` 명령, 타원에 25는 가로 반지름이 고 세로 반경 50입니다. 끝점에 숫자 들의 마지막 쌍으로 표시 됩니다는 `A` 점 (300, 49.9)를 나타내는 명령입니다. 하는 시작 지점에서 방금 의도적으로 약간 다릅니다. 끝점은 시작 지점으로 설정, 호 렌더링 되지 않습니다. 완전 한 타원을 그릴를 설정 해야 끝점에 가깝게 같지 않음) (않음 두 개 이상의 시작 지점 또는 있습니다 사용 해야 `A` 완전 한 타원의 부분에 대 한 각 명령입니다.

페이지의 생성자에 다음 문을 추가 하 고 다음 결과 문자열을 검사 하려면 중단점을 설정 하 고 설정할 수 있습니다.

```csharp
string str = helloPath.ToSvgPathData();
```

호를 일련의 긴으로 바뀌었음을 배워 `Q` 정방형 베 지 어 곡선을 사용 하 여 호의 증분 근사값에 대 한 명령입니다.

`PaintSurface` 처리기 제어점 'E'에 대 한 포함 하지 않는 경로의 긴밀 하 게 범위를 가져오면 및 ' 곡선 o '. 다음 세 가지 변환 경로의 중심 지점 (0, 0)으로, 경로 캔버스 (하지만 또한 고려 스트로크 너비)의 크기를 조정할 고 이동한 다음 이동 경로의 중심 캔버스의 가운데에:

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

경로 가로 모드로 볼 때 더 적절 하 게 표시 되는 캔버스를 채웁니다.

[![](path-data-images/pathdatahello-small.png "경로 데이터 Hello 페이지의 삼중 스크린샷")](path-data-images/pathdatahello-large.png "경로 데이터 Hello 페이지의 삼중 스크린 샷")

**경로 데이터 Cat** 페이지는와 유사 합니다. 경로 및 그리기 개체 모두에 필드로 정의 [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) 클래스:

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

Cat의 머리 원, 이며 두 개의로 렌더링 될 여기 `A` 그리는 반원 명령입니다. 둘 다 `A` 100의 가로 및 세로 반지름을 정의 하는 헤드에 대 한 명령입니다. 시작 하는 첫 번째 호 (240, 100)에서 끝납니다 (240, 300) 다시 끝나는 두 번째 호에 대 한 시작 지점이 됩니다 (240, 100).

두 개의 눈 두 개의로 렌더링 됩니다 `A` 명령 및 cat의 헤드와 마찬가지로 두 번째 `A` 명령 첫 번째의 시작으로 동일한 지점에서 끝날 `A` 명령입니다. 그러나 이러한 쌍의 `A` 명령 되는 타원을 정의 하지 않습니다. 각 호의 40 단위가 고 반지름은 또한 40 단위, 즉, 이러한 호 전체 semicircles 없는 합니다.

`PaintSurface` 처리기 앞의 예제에서와 비슷한 변환을 수행 하지만 single `Scale` 가로 세로 비율을 유지 관리 하 고 거의 여백을 제공 하 여 cat의 수염에 대는 화면의 측면을 터치 하지 않으므로 요소:

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

다음은 세 플랫폼 모두에서 실행 중인 프로그램입니다.

[![](path-data-images/pathdatacat-small.png "경로 데이터 Cat 페이지의 삼중 스크린샷")](path-data-images/pathdatacat-large.png "경로 데이터 Cat 페이지의 삼중 스크린샷")

일반적으로, 한 `SKPath` 개체 필드로 정의 되 면 경로의 윤곽선 생성자 나 다른 방법에 정의 되어야 합니다. 그러나 SVG 경로 데이터를 사용할 때 살펴보았습니다 필드 정의에 전체 경로 지정할 수 있습니다.

이전 **까다로운 아날로그 클록** 샘플는 [ **The 회전 변환** ](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) 문서 단순 선으로 시계 바늘을 표시 합니다. **아날로그 클록 꽤** 아래 프로그램 대체 된 이러한 줄 `SKPath` 에 필드로 정의 된 개체는 [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) 클래스와 함께 `SKPaint` 개체:

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

시간 및 분 바늘은 영역, 따라서 해당 손 서로 고유 이제 묶인, black 개요와 모두 회색 채우기를 사용 하 여 사용 된 `handStrokePaint` 및 `handFillPaint` 개체입니다.

이전 **까다로운 아날로그 클록** 샘플은 거의 원을 표시 시간 및 분 루프에 그려진 합니다. 이 **아날로그 클록 꽤** 샘플에 사용 되는 완전히 다른 방법: 시간 및 분 부호를 사용 하 여은 점선으로 사용 하 여 그린는 `minuteMarkPaint` 및 `hourMarkPaint` 개체:

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

[ **점선 및 대시** ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) 가이드 사용 하는 방법을 설명는 `SKPathEffect.CreateDash` 파선 만드는 메서드를 합니다. 첫 번째 인수는 `float` 일반적으로 두 개의 요소가 있는 배열: 첫 번째 요소는 대시의 길이 이며 두 번째 요소는 대시 사이의 간격입니다. 경우는 `StrokeCap` 속성이 `SKStrokeCap.Round`, 둥근된는 대시의 끝 대시의 양쪽 모두에 스트로크 너비에 따라 대시 길이 효과적으로 연장 합니다. 따라서 첫 번째 배열 요소를 0으로 설정 점선 만듭니다.

이러한 점 사이의 거리는 두 번째 배열 요소에 의해 제어 됩니다. 하겠지만 하겠지만, 이러한 두 `SKPaint` 개체는 90의 반지름 있는 원을 그리는 데 사용 됩니다. 이 원의 원주 60 분 부호를 사용 하 여 모든 3 π 단위를 표시 해야 이므로, 180π 되므로에 두 번째 값에 `float` 에 배열 `minuteMarkPaint`합니다. 12 시간 표시와 야 모든 15π 단위는 두 번째에서 값인 `float` 배열입니다.

`PrettyAnalogClockPage` 클래스 16 밀리초 마다는 화면을 무효화 하는 타이머를 설정 및 `PaintSurface` 이러한 비율로 처리기가 호출 됩니다. 이전 정의 `SKPath` 및 `SKPaint` 개체에 대 한 허용 아주 간결 그리기 코드:

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

그러나 두 번째 손으로 로드할은 특수 한 합니다. 시계는 업데이트 되므로 16 밀리초 마다는 `Millisecond` 속성의는 `DateTime` 값 하나 개별 점프에 이동 하는 대신 직접 두 번째 스윕을 애니메이션 효과를 잠재적으로 사용할 수 있습니다 초에 두 번째에서입니다. 하지만이 코드를 매끄럽게 이동을 허용 하지 않습니다. 대신,는 Xamarin.Forms를 사용 [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) 및 [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) 감속/가속 함수는 다른 종류의 이동에 대 한 애니메이션 합니다. 이러한 감속/가속 함수 인해 두 번째 포인터는 화면 떨림 방식으로 & #x 2014;으로 이동 하려면 끌어오기 다시 약간를 이동한 다음 약간 과도 하 게 해결 목적지 효과 아쉽게도 재현할 수 없는 정적이 스크린 샷에 전에:

[![](path-data-images/prettyanalogclock-small.png "예쁜 아날로그 클록 페이지의 삼중 스크린샷")](path-data-images/prettyanalogclock-large.png "아날로그 클록 꽤 페이지의 삼중 스크린샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
