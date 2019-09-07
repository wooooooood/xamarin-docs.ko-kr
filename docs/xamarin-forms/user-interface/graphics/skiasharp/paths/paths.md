---
title: SkiaSharp의 경로 기본 사항
description: 이 문서에서는 연결 된 선 및 곡선을 결합 하는 데 SkiaSharp SKPath 개체를 탐색 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: eee338461593ad131f679d32cadf63fe3b1a4c40
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759339"
---
# <a name="path-basics-in-skiasharp"></a>SkiaSharp의 경로 기본 사항

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_연결 된 선 및 곡선을 결합 하는 데 SkiaSharp SKPath 개체 탐색_

그래픽 경로의 가장 중요 한 기능 중 하나는 경우 이러한 연결 되지 않아야 하 고 여러 줄을 연결 해야 하는 경우를 정의할 수 있습니다. 이러한 두 개의 삼각형의 위쪽 있듯이 차이 중요 수 있습니다.

![](paths-images/connectedlinesexample.png "연결 및 연결이 끊긴 줄 간의 차이 보여 주는 두 개의 삼각형")

그래픽 경로 의해 캡슐화 되는 [ `SKPath` ](xref:SkiaSharp.SKPath) 개체입니다. 하나 이상의 컬렉션의 경로가 *윤곽*합니다. 각 윤곽선의 컬렉션인 *연결* 직선 및 곡선입니다. 윤곽을 서로 연결 되어 있지 않지만 시각적으로 중첩 될 수 있습니다. 경우에 따라 단일 윤곽선 자체적으로 겹쳐질 수 있습니다.

윤곽선의 다음 메서드를 호출 하 여 일반적으로 시작 `SKPath`:

- [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo*) 새 윤곽선을 시작 하려면

해당 메서드에 인수가 표현할 수 있는 단일 지점을 `SKPoint` 값 또는 별도 X 및 Y 좌표입니다. 합니다 `MoveTo` 윤곽선 및 초기 시작 시점을 설정 하는 호출 *현재 지점*합니다. 줄 또는 현재 위치에서 메서드를 새로운 현재 점의 그러면에 지정 된 지점에 곡선을 사용 하 여 윤곽선을 계속 하려면 다음 메서드를 호출할 수 있습니다.

- [`LineTo`](xref:SkiaSharp.SKPath.LineTo*) 경로에 직선을 추가 하려면
- [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*) 원 또는 타원의 일부 줄을 나타내는 호를 추가 하려면
- [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo*) 입방 형 3 차원 곡선 스플라인을 추가 하려면
- [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo*) 정방형 베 지 어 스플라인을 추가 하려면
- [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo*) rational 정방형 베 지 어 스플라인 원추형 섹션 (타원, parabolas, 및 hyperbolas)를 정확 하 게 렌더링할 수 있는 추가 하려면

이러한 5 개의 메서드 선이나 곡선에 설명 하는 데 필요한 모든 정보를 포함 합니다. 이러한 다섯 개의 메서드의 메서드 호출 바로 앞에서 설정한 현재 지점과 함께에서 작동 합니다. 예를 들어 합니다 `LineTo` 윤곽선 직선 현재 시점에 기반 매개 변수를 추가 하는 메서드 `LineTo` 만 단일 지점입니다.

합니다 `SKPath` 클래스에는 또한 사용 하 여 이러한 6 가지 방법으로 동일한 이름을 갖는 메서드를 정의 `R` 부분:

- [`RMoveTo`](xref:SkiaSharp.SKPath.RMoveTo*)
- [`RLineTo`](xref:SkiaSharp.SKPath.RLineTo*)
- [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*)
- [`RCubicTo`](xref:SkiaSharp.SKPath.RCubicTo*)
- [`RQuadTo`](xref:SkiaSharp.SKPath.RQuadTo*)
- [`RConicTo`](xref:SkiaSharp.SKPath.RConicTo*)

합니다 `R` 의미 *상대*합니다. 이러한 메서드 없이 해당 메서드와 동일한 구문을 사용 해야는 `R` 있지만 현재 점을 기준으로 합니다. 이 비슷한 구성 요소가 메서드를 여러 번 호출 하 여 경로 그리는 데 유용 합니다.

윤곽선에 대 한 다른 호출으로 종료 `MoveTo` 또는 `RMoveTo`, 호출이 나 새 윤곽선을 시작 `Close`는 윤곽선을 닫습니다. `Close` 메서드 자동으로 추가 된 윤곽선의 첫 번째 지점과 현재 지점에서 직선 하 고 닫힌 것으로 경로 표시 즉, 모든 스트로크 단면 없이 렌더링 됩니다 것입니다.

열고 닫을 윤곽 간의 차이에 설명 되어 있습니다 합니다 **두 개의 삼각형 윤곽선** 페이지를 사용 하는 `SKPath` 두 삼각형을 렌더링 하는 두 윤곽을 사용 하 여 개체입니다. 첫 번째 윤곽선 열려 하 고 두 번째 닫힙니다. 다음은 [ `TwoTriangleContoursPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs) 클래스:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create the path
    SKPath path = new SKPath();

    // Define the first contour
    path.MoveTo(0.5f * info.Width, 0.1f * info.Height);
    path.LineTo(0.2f * info.Width, 0.4f * info.Height);
    path.LineTo(0.8f * info.Width, 0.4f * info.Height);
    path.LineTo(0.5f * info.Width, 0.1f * info.Height);

    // Define the second contour
    path.MoveTo(0.5f * info.Width, 0.6f * info.Height);
    path.LineTo(0.2f * info.Width, 0.9f * info.Height);
    path.LineTo(0.8f * info.Width, 0.9f * info.Height);
    path.Close();

    // Create two SKPaint objects
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Magenta,
        StrokeWidth = 50
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    // Fill and stroke the path
    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

첫 번째 윤곽선에 대 한 호출 이루어져 [ `MoveTo` ](xref:SkiaSharp.SKPath.MoveTo(System.Single,System.Single)) X 및 Y 좌표를 사용 하 여 아닌 `SKPoint` 를 세 번 호출 뒤에 값 [ `LineTo` ](xref:SkiaSharp.SKPath.LineTo(System.Single,System.Single)) 세 가지 측면을 그릴는 삼각형입니다. 두 번째 윤곽선에 대 한 두 호출에 `LineTo` 윤곽선에 대 한 호출을 사용 하 여 완료 하지만 [ `Close` ](xref:SkiaSharp.SKPath.Close)는 윤곽선을 닫습니다. 큰 차이:

[![](paths-images/twotrianglecontours-small.png "두 개의 삼각형 윤곽선 페이지 스크린샷 삼중")](paths-images/twotrianglecontours-large.png#lightbox "삼중 두 개의 삼각형 윤곽선 페이지 스크린샷")

알 수 있듯이 첫 번째 윤곽선은 일련의 세 가지 연결 된 선 있지만 끝 시작 부분을 사용 하 여 연결 하지 않습니다. 두 줄 맨 위에 있는 겹칩니다. 두 번째 윤곽선 분명히 닫혀 있고 더 적은 하나를 사용 하 여 작업을 수행 했습니다 `LineTo` 하기 때문에 호출 된 `Close` 메서드는 자동으로 윤곽선을 닫으려면 마지막 줄을 추가 합니다.

`SKCanvas` 에서는 하나만 정의 [ `DrawPath` ](xref:SkiaSharp.SKCanvas.DrawPath(SkiaSharp.SKPath,SkiaSharp.SKPaint)) 메서드를 두 번 호출 되는이 데모를 채우고 스트로크 경로입니다. 모든 윤곽 가득 차면 닫혀 있지 않은 해당 합니다. 닫히지 않은 경로 채우기 위해, 직선 윤곽선의 시작점과 끝점 간에 존재로 간주 됩니다. 마지막 제거 하는 경우 `LineTo` 첫 번째 윤곽선 또는 제거를 `Close` 호출에서 두 번째 윤곽선을 각 윤곽선 삼각형 것 처럼 채워질지 않습니다만 두 변 해야 합니다.

`SKPath` 다른 많은 메서드와 속성을 정의합니다. 다음 방법 닫히거나 방법에 따라 닫히지 수 있는 경로 전체 윤곽을 추가 합니다.

- [`AddRect`](xref:SkiaSharp.SKPath.AddRect*)
- [`AddRoundedRect`](xref:SkiaSharp.SKPath.AddRoundedRect(SkiaSharp.SKRect,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddCircle`](xref:SkiaSharp.SKPath.AddCircle(System.Single,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddOval`](xref:SkiaSharp.SKPath.AddOval(SkiaSharp.SKRect,SkiaSharp.SKPathDirection))
- [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single)) 타원의 곡선을 추가 하려면
- [`AddPath`](xref:SkiaSharp.SKPath.AddPath*) 현재 경로에 다른 경로 추가 하려면
- [`AddPathReverse`](xref:SkiaSharp.SKPath.AddPathReverse(SkiaSharp.SKPath)) 반대로 다른 경로 추가 하려면

한다는 점에 주의 `SKPath` 개체는 기 하 도형을 정의 &mdash; 일련의 점과 점을 연결 합니다. 경우에만 `SKPath` 와 결합 되는 `SKPaint` 개체는 특정 색, 스트로크 너비 등을 사용 하 여 렌더링 경로입니다. 또한에 유의 합니다 `SKPaint` 에 전달 된 개체는 `DrawPath` 메서드는 전체 경로 특성을 정의 합니다. 여러 색 필요한 것 그리기 하려는 경우에 각 색에 대 한 별도 경로 사용 해야 합니다.

두 줄 사이의 연결 모양의 시작과 선 끝 모양을 스트로크 단면을 정의한 것 처럼 정의한를 *스트로크 조인*합니다. 설정 하 여이 지정 합니다 [ `StrokeJoin` ](xref:SkiaSharp.SKPaint.StrokeJoin) 속성을 `SKPaint` 의 멤버에는 [ `SKStrokeJoin` ](xref:SkiaSharp.SKStrokeJoin) 열거형:

- `Miter` 가 항상 조인에 대 한
- `Round` 반올림 된 조인에 대 한
- `Bevel` 조인의 여러 레이어로 백오프

합니다 **스트로크 조인** 에서는 다음과 같은 코드를 사용 하 여 조인 스트로크 이러한 세 개의 페이지는 **스트로크 단면** 페이지. 이 `PaintSurface` 이벤트 처리기는 [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs) 클래스:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Right
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width - 100;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = 2 * textPaint.FontSpacing;
    string[] strStrokeJoins = { "Miter", "Round", "Bevel" };

    foreach (string strStrokeJoin in strStrokeJoins)
    {
        // Display text
        canvas.DrawText(strStrokeJoin, xText, y, textPaint);

        // Get stroke-join value
        SKStrokeJoin strokeJoin;
        Enum.TryParse(strStrokeJoin, out strokeJoin);

        // Create path
        SKPath path = new SKPath();
        path.MoveTo(xLine1, y - 80);
        path.LineTo(xLine1, y + 80);
        path.LineTo(xLine2, y + 80);

        // Display thick line
        thickLinePaint.StrokeJoin = strokeJoin;
        canvas.DrawPath(path, thickLinePaint);

        // Display thin line
        canvas.DrawPath(path, thinLinePaint);
        y += 3 * textPaint.FontSpacing;
    }
}
```

실행 중인 프로그램이 다음과 같습니다.

[![](paths-images/strokejoins-small.png "선 조인 페이지 스크린샷 삼중")](paths-images/strokejoins-large.png#lightbox "삼중 스트로크 조인 페이지 스크린샷")

마이터 조인을 줄에 연결 하는 # 지점으로 구성 됩니다. 두 줄에 작은 각도로 가입 마이터 조인을 상당히 길어질 수 있습니다. 지나치게 긴 마이터 조인을 방지 하려면 마이터 조인 길이의 값을 기준으로 제한 됩니다 합니다 [ `StrokeMiter` ](xref:SkiaSharp.SKPaint.StrokeMiter) 속성의 `SKPaint`합니다. 이 길이 초과 하는 음 빗면 조인 되도록 잘려 됩니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
