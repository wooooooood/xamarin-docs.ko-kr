---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6ceac2d866e67af5cf3496fcf8c072ae83ecfe38
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140246"
---
# <a name="path-basics-in-skiasharp"></a>SkiaSharp의 경로 기본 사항

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_연결 된 선 및 곡선을 결합 하기 위한 SkiaSharp 된 경로 개체 탐색_

그래픽 경로의 가장 중요 한 기능 중 하나는 여러 줄이 연결 되어야 하는 시기와 연결 되지 않아야 하는 경우를 정의 하는 기능입니다. 이러한 두 삼각형의 위쪽에서 보여 주는 것 처럼 차이점은 중요할 수 있습니다.

![](paths-images/connectedlinesexample.png "Two triangles showing the difference between connected and disconnected lines")

그래픽 경로는 개체에 의해 캡슐화 됩니다 [`SKPath`](xref:SkiaSharp.SKPath) . 경로는 하나 이상의 *컨투어에*대 한 컬렉션입니다. 각 컨투어는 *연결* 된 직선 및 곡선의 컬렉션입니다. 외형선은 서로 연결 되지 않지만 시각적으로 겹칠 수 있습니다. 경우에 따라 단일 컨투어가 겹칠 수 있습니다.

외형선은 일반적으로 다음 메서드를 호출 하 여 시작 합니다 `SKPath` .

- [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo*)새 컨투어를 시작 하려면

이 메서드에 대 한 인수는 단일 점으로, 값으로 표현 `SKPoint` 하거나 별도 X 및 Y 좌표로 표현할 수 있습니다. 이 `MoveTo` 호출은 컨투어 시작 지점 및 초기 *현재 점을*설정 합니다. 다음 메서드를 호출 하 여 현재 점에서 메서드에 지정 된 지점까지 선 또는 곡선이 있는 컨투어를 계속 진행 하 고 새 현재 점이 됩니다.

- [`LineTo`](xref:SkiaSharp.SKPath.LineTo*)경로에 직선을 추가 하려면
- [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*)원 또는 타원의 원주 선 인 호를 추가 하려면
- [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo*)입방 형 3 차원 곡선 스플라인을 추가 하려면
- [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo*)정방형 3 차원 곡선 스플라인을 추가 하려면
- [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo*)원추형 섹션 (줄임표, parabolas 및 hyperbolas)을 정확 하 게 렌더링할 수 있는 유리수 정방형 3 차원 곡선 스플라인을 추가 하려면

이러한 다섯 가지 방법에는 선 또는 곡선을 설명 하는 데 필요한 모든 정보가 포함 되어 있지 않습니다. 이러한 다섯 개의 메서드는 바로 앞의 메서드 호출로 설정 된 현재 지점과 함께 작동 합니다. 예를 들어, `LineTo` 메서드는 현재 점에 따라 윤곽선에 직선을 추가 하므로에 대 한 매개 변수는 `LineTo` 단일 지점만 됩니다.

또한 클래스는 다음과 같은 `SKPath` 6 개의 메서드와 동일한 이름을 가지는 메서드를 정의 합니다 `R` .

- [`RMoveTo`](xref:SkiaSharp.SKPath.RMoveTo*)
- [`RLineTo`](xref:SkiaSharp.SKPath.RLineTo*)
- [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*)
- [`RCubicTo`](xref:SkiaSharp.SKPath.RCubicTo*)
- [`RQuadTo`](xref:SkiaSharp.SKPath.RQuadTo*)
- [`RConicTo`](xref:SkiaSharp.SKPath.RConicTo*)

는 `R` *상대*를 나타냅니다. 이러한 메서드는가 없는 해당 메서드와 동일한 구문을 사용 `R` 하지만 현재 점에 상대적입니다. 이는 여러 번 호출 하는 메서드에서 경로의 비슷한 부분을 그리는 데 유용 합니다.

컨투어는 또는에 대 한 다른 호출로 끝나지만 `MoveTo` `RMoveTo` 새 컨투어를 시작 하거나에 대 한 호출을 사용 하 여 `Close` 컨투어를 닫습니다. `Close`메서드는 현재 점에서 컨투어의 첫 번째 점에 직선을 자동으로 추가 하 고 경로를 닫힌 것으로 표시 합니다. 즉, 스트로크 캡 없이 렌더링 됩니다.

두 개의 윤곽선이 있는 개체를 사용 하 여 두 개의 삼각형을 렌더링 하는 **두 개의 삼각형 컨투어** 페이지에서 여는 컨투어와 닫힌 컨투어 간의 차이를 보여 줍니다 `SKPath` . 첫 번째 컨투어가 열리고 두 번째 컨투어가 닫혀 있습니다. [`TwoTriangleContoursPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/TwoTriangleContoursPage.cs)클래스는 다음과 같습니다.

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

첫 번째 컨투어는 [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo(System.Single,System.Single)) 값이 아닌 X 및 Y 좌표를 사용 하는 호출로 구성 된 `SKPoint` 다음 세 개의 호출을 통해 [`LineTo`](xref:SkiaSharp.SKPath.LineTo(System.Single,System.Single)) 삼각형의 세 면을 그립니다. 두 번째 외형선에는에 대 한 두 개의 호출만 `LineTo` 있지만 컨투어를 닫는를 호출 하 여 컨투어를 마무리 합니다 [`Close`](xref:SkiaSharp.SKPath.Close) . 차이점은 중요 합니다.

[![](paths-images/twotrianglecontours-small.png "Triple screenshot of the Two Triangle Contours page")](paths-images/twotrianglecontours-large.png#lightbox "Triple screenshot of the Two Triangle Contours page")

여기서 볼 수 있듯이 첫 번째 컨투어는 세 개의 연결 된 선 이지만 끝에 연결 되지 않습니다. 두 줄이 위쪽에 겹칩니다. 두 번째 컨투어는 명확 하 게 종결 되며, `LineTo` `Close` 메서드가 컨투어를 닫기 위해 최종 줄을 자동으로 추가 하기 때문에 한 번의 호출로 수행 되었습니다.

`SKCanvas`하나의 메서드만 정의 합니다 [`DrawPath`](xref:SkiaSharp.SKCanvas.DrawPath(SkiaSharp.SKPath,SkiaSharp.SKPaint)) .이 데모에서는 경로를 채우고 스트로크 하기 위해를 두 번 호출 합니다. 닫히지 않은 경우에도 모든 컨투어가 채워집니다. 닫히지 않은 경로를 채우기 위해 직선은 윤곽선의 시작점과 끝점 사이에 존재 하는 것으로 간주 됩니다. `LineTo`첫 번째 컨투어에서 마지막을 제거 하거나 `Close` 두 번째 컨투어에서 호출을 제거 하는 경우 각 컨투어는 두 면만 갖지만 삼각형 인 것 처럼 채워집니다.

`SKPath`다른 많은 메서드와 속성을 정의 합니다. 다음 메서드는 경로에 전체 컨투어를 추가 합니다 .이는 메서드에 따라 닫히거나 닫히지 않았을 수 있습니다.

- [`AddRect`](xref:SkiaSharp.SKPath.AddRect*)
- [`AddRoundedRect`](xref:SkiaSharp.SKPath.AddRoundedRect(SkiaSharp.SKRect,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddCircle`](xref:SkiaSharp.SKPath.AddCircle(System.Single,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddOval`](xref:SkiaSharp.SKPath.AddOval(SkiaSharp.SKRect,SkiaSharp.SKPathDirection))
- [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single))타원의 원주에 곡선을 추가 하려면
- [`AddPath`](xref:SkiaSharp.SKPath.AddPath*)현재 경로에 다른 경로를 추가 하려면
- [`AddPathReverse`](xref:SkiaSharp.SKPath.AddPathReverse(SkiaSharp.SKPath))반대 방향으로 다른 경로를 추가 하려면

`SKPath`개체는 &mdash; 일련의 점과 연결의 기 하 도형만 정의 한다는 점에 유의 하세요. `SKPath`가 개체와 결합 된 경우에만 `SKPaint` 특정 색, 스트로크 너비 등을 사용 하 여 렌더링 된 경로입니다. 또한 메서드로 전달 되는 개체는 `SKPaint` `DrawPath` 전체 경로의 특징을 정의 합니다. 여러 색이 필요한 항목을 그리려면 각 색에 별도의 경로를 사용 해야 합니다.

선의 시작과 끝 모양이 스트로크 단면에 의해 정의 되는 것 처럼 두 줄 사이의 연결 모양은 *스트로크 조인*에 의해 정의 됩니다. [`StrokeJoin`](xref:SkiaSharp.SKPaint.StrokeJoin)의 속성을 `SKPaint` 열거형의 멤버로 설정 하 여이를 지정 합니다 [`SKStrokeJoin`](xref:SkiaSharp.SKStrokeJoin) .

- `Miter`pointy join의 경우
- `Round`둥근 조인의 경우
- `Bevel`잘라내야 조인

스트로크 **조인** 페이지에는 스트로크 **단면** 페이지와 비슷한 코드와 세 가지 스트로크 조인이 표시 됩니다. `PaintSurface`다음은 클래스의 이벤트 처리기입니다 [`StrokeJoinsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeJoinsPage.cs) .

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

실행 중인 프로그램은 다음과 같습니다.

[![](paths-images/strokejoins-small.png "Triple screenshot of the Stroke Joins page")](paths-images/strokejoins-large.png#lightbox "Triple screenshot of the Stroke Joins page")

사접 조인은 선이 연결 되는 뾰족한 점으로 구성 됩니다. 두 줄이 작은 각도로 조인 되 면 사접 조인이 매우 길어질 수 있습니다. 너무 긴 마이터 조인을 방지 하기 위해 사접 조인의 길이는의 속성 값에 의해 제한 됩니다 [`StrokeMiter`](xref:SkiaSharp.SKPaint.StrokeMiter) `SKPaint` . 이 길이를 초과 하는 마이터 조인은 빗면 조인이 될 잘라내야 off입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
