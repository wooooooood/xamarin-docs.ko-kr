---
title: SkiaSharp의 경로 기본 사항
description: 이 문서에서는 연결 된 선 및 곡선을 결합 하는 데 SkiaSharp SKPath 개체를 탐색 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: c892adf2f75ec00c4a9ee171ded78f79bb8227e9
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725196"
---
# <a name="path-basics-in-skiasharp"></a>SkiaSharp의 경로 기본 사항

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_연결 된 선 및 곡선을 결합 하기 위한 SkiaSharp 된 경로 개체 탐색_

그래픽 경로의 가장 중요 한 기능 중 하나는 경우 이러한 연결 되지 않아야 하 고 여러 줄을 연결 해야 하는 경우를 정의할 수 있습니다. 이러한 두 개의 삼각형의 위쪽 있듯이 차이 중요 수 있습니다.

![](paths-images/connectedlinesexample.png "Two triangles showing the difference between connected and disconnected lines")

그래픽 경로는 [`SKPath`](xref:SkiaSharp.SKPath) 개체에 의해 캡슐화 됩니다. 경로는 하나 이상의 *컨투어에*대 한 컬렉션입니다. 각 컨투어는 *연결* 된 직선 및 곡선의 컬렉션입니다. 윤곽을 서로 연결 되어 있지 않지만 시각적으로 중첩 될 수 있습니다. 경우에 따라 단일 윤곽선 자체적으로 겹쳐질 수 있습니다.

외형선은 일반적으로 다음 `SKPath`메서드를 호출 하 여 시작 합니다.

- 새 컨투어를 시작 [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo*)

이 메서드에 대 한 인수는 단일 점으로, `SKPoint` 값으로 표현 하거나 별도 X 및 Y 좌표로 표현할 수 있습니다. `MoveTo` 호출은 컨투어 시작 지점 및 초기 *현재 점을*설정 합니다. 줄 또는 현재 위치에서 메서드를 새로운 현재 점의 그러면에 지정 된 지점에 곡선을 사용 하 여 윤곽선을 계속 하려면 다음 메서드를 호출할 수 있습니다.

- 경로에 직선을 추가 [`LineTo`](xref:SkiaSharp.SKPath.LineTo*)
- 원 또는 타원의 원주 선 인 호를 추가 하려면 [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*) 합니다.
- 입방 형 3 차원 곡선 스플라인을 추가 하 [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo*)
- 정방형 3 차원 곡선 스플라인을 추가 하 [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo*)
- 원추형 섹션 (줄임표, parabolas 및 hyperbolas)을 정확 하 게 렌더링할 수 있는 유리수 정방형 3 차원 곡선 스플라인을 추가 하려면 [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo*) 합니다.

이러한 5 개의 메서드 선이나 곡선에 설명 하는 데 필요한 모든 정보를 포함 합니다. 이러한 다섯 개의 메서드의 메서드 호출 바로 앞에서 설정한 현재 지점과 함께에서 작동 합니다. 예를 들어 `LineTo` 메서드는 현재 점에 따라 윤곽선에 직선을 추가 하므로 `LineTo`에 대 한 매개 변수는 단일 지점입니다.

`SKPath` 클래스는 이러한 6 개의 메서드와 이름이 같지만 시작 부분에 `R` 있는 메서드를 정의 하기도 합니다.

- [`RMoveTo`](xref:SkiaSharp.SKPath.RMoveTo*)
- [`RLineTo`](xref:SkiaSharp.SKPath.RLineTo*)
- [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*)
- [`RCubicTo`](xref:SkiaSharp.SKPath.RCubicTo*)
- [`RQuadTo`](xref:SkiaSharp.SKPath.RQuadTo*)
- [`RConicTo`](xref:SkiaSharp.SKPath.RConicTo*)

`R`는 *상대*를 나타냅니다. 이러한 메서드는 `R` 없지만 현재 점을 기준으로 하는 해당 메서드와 동일한 구문을 사용 합니다. 이 비슷한 구성 요소가 메서드를 여러 번 호출 하 여 경로 그리는 데 유용 합니다.

컨투어는 새 컨투어를 시작 하는 `MoveTo` 또는 `RMoveTo`에 대 한 다른 호출로 끝나지만 컨투어를 닫는 `Close`를 호출 합니다. `Close` 메서드는 현재 점에서 컨투어의 첫 번째 점에 직선을 자동으로 추가 하 고 경로를 닫힌 것으로 표시 합니다. 즉, 스트로크 캡 없이 렌더링 됩니다.

열려 있는 컨투어와 닫힌 컨투어 간의 차이점은 두 개의 두 개의 2 개를 포함 하는 `SKPath` 개체를 사용 하 여 두 개의 삼각형을 렌더링 하는 **두 개의 삼각형 컨투어** 페이지에 나와 있습니다. 첫 번째 윤곽선 열려 하 고 두 번째 닫힙니다. [`TwoTriangleContoursPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/TwoTriangleContoursPage.cs) 클래스는 다음과 같습니다.

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

첫 번째 컨투어는 `SKPoint` 값이 아닌 X 및 Y 좌표를 사용 하는 [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo(System.Single,System.Single)) 에 대 한 호출로 구성 된 다음 세 개의 [`LineTo`](xref:SkiaSharp.SKPath.LineTo(System.Single,System.Single)) 호출을 통해 삼각형의 세 면을 그립니다. 두 번째 컨투어는 `LineTo`에 대 한 두 개의 호출만 포함 하지만 컨투어를 닫는 [`Close`](xref:SkiaSharp.SKPath.Close)에 대 한 호출로 컨투어를 완료 합니다. 큰 차이:

[![](paths-images/twotrianglecontours-small.png "Triple screenshot of the Two Triangle Contours page")](paths-images/twotrianglecontours-large.png#lightbox "Triple screenshot of the Two Triangle Contours page")

알 수 있듯이 첫 번째 윤곽선은 일련의 세 가지 연결 된 선 있지만 끝 시작 부분을 사용 하 여 연결 하지 않습니다. 두 줄 맨 위에 있는 겹칩니다. 두 번째 컨투어는 명확 하 게 종결 되었으며 `Close` 메서드가 컨투어를 닫기 위해 최종 줄을 자동으로 추가 하기 때문에 하나 이상의 `LineTo` 호출로 수행 되었습니다.

`SKCanvas`는 하나의 [`DrawPath`](xref:SkiaSharp.SKCanvas.DrawPath(SkiaSharp.SKPath,SkiaSharp.SKPaint)) 메서드만 정의 합니다 .이 데모에서는 경로를 채우고 스트로크 하기 위해 두 번 호출 됩니다. 모든 윤곽 가득 차면 닫혀 있지 않은 해당 합니다. 닫히지 않은 경로 채우기 위해, 직선 윤곽선의 시작점과 끝점 간에 존재로 간주 됩니다. 첫 번째 컨투어에서 마지막 `LineTo`을 제거 하거나 두 번째 컨투어에서 `Close` 호출을 제거 하는 경우 각 컨투어는 두 면만 갖지만 삼각형 인 것 처럼 채워집니다.

`SKPath`는 다른 여러 메서드 및 속성을 정의 합니다. 다음 방법 닫히거나 방법에 따라 닫히지 수 있는 경로 전체 윤곽을 추가 합니다.

- [`AddRect`](xref:SkiaSharp.SKPath.AddRect*)
- [`AddRoundedRect`](xref:SkiaSharp.SKPath.AddRoundedRect(SkiaSharp.SKRect,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddCircle`](xref:SkiaSharp.SKPath.AddCircle(System.Single,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddOval`](xref:SkiaSharp.SKPath.AddOval(SkiaSharp.SKRect,SkiaSharp.SKPathDirection))
- 타원의 원주에 곡선을 추가 [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single))
- 현재 경로에 다른 경로를 추가 [`AddPath`](xref:SkiaSharp.SKPath.AddPath*)
- 반대 방향으로 다른 경로를 추가 [`AddPathReverse`](xref:SkiaSharp.SKPath.AddPathReverse(SkiaSharp.SKPath))

`SKPath` 개체는 일련의 점과 연결 &mdash; 기 하 도형만 정의 합니다. `SKPath` `SKPaint` 개체와 결합 된 경우에만 특정 색, 스트로크 너비 등을 사용 하 여 렌더링 된 경로입니다. 또한 `DrawPath` 메서드에 전달 된 `SKPaint` 개체는 전체 경로의 특징을 정의 합니다. 여러 색 필요한 것 그리기 하려는 경우에 각 색에 대 한 별도 경로 사용 해야 합니다.

선의 시작과 끝 모양이 스트로크 단면에 의해 정의 되는 것 처럼 두 줄 사이의 연결 모양은 *스트로크 조인*에 의해 정의 됩니다. `SKPaint`의 [`StrokeJoin`](xref:SkiaSharp.SKPaint.StrokeJoin) 속성을 [`SKStrokeJoin`](xref:SkiaSharp.SKStrokeJoin) 열거의 멤버로 설정 하 여이를 지정 합니다.

- pointy join에 대 한 `Miter`
- 둥근 조인에 대 한 `Round`
- 잘라내야 조인에 대 한 `Bevel`

스트로크 **조인** 페이지에는 스트로크 **단면** 페이지와 비슷한 코드와 세 가지 스트로크 조인이 표시 됩니다. 다음은 [`StrokeJoinsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeJoinsPage.cs) 클래스의 `PaintSurface` 이벤트 처리기입니다.

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

마이터 조인을 줄에 연결 하는 # 지점으로 구성 됩니다. 두 줄에 작은 각도로 가입 마이터 조인을 상당히 길어질 수 있습니다. 과도 하 게 긴 마이터 조인을 방지 하기 위해 사접 조인의 길이는 `SKPaint`의 [`StrokeMiter`](xref:SkiaSharp.SKPaint.StrokeMiter) 속성 값에 의해 제한 됩니다. 이 길이 초과 하는 음 빗면 조인 되도록 잘려 됩니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
