---
title: 경로 기본 사항
description: 연결 된 선 및 곡선을 결합 하는 데 SkiaSharp SKPath 개체를 탐색 합니다.
ms.topic: article
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: e083a9d6a97181b4e726d4553b14671f06157f80
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2018
---
# <a name="path-basics"></a>경로 기본 사항

_연결 된 선 및 곡선을 결합 하는 데 SkiaSharp SKPath 개체를 탐색 합니다._

그래픽 경로의 가장 중요 한 기능 중 하나 때 이러한 연결 되지 않아야 하 고 여러 줄을 연결 하는 경우를 정의 하는 기능입니다. 이러한 두 개의 삼각형의 위쪽에서 설명한 것 처럼 차이 매우 표시 될 수 있습니다.

![](paths-images/connectedlinesexample.png "연결 되거나 연결이 끊긴 줄 간의 차이 보여 주는 두 개의 삼각형")

그래픽 경로 의해 캡슐화 됩니다는 [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) 개체입니다. 경로 하나 이상의 컬렉션 *윤곽선*합니다. 각 윤곽선의 컬렉션인 *연결* 직선 및 곡선입니다. 윤곽선 서로에 연결 되어 있지 않지만 시각적으로 겹칠 수 있습니다. 경우에 따라 단일 윤곽선 자체를 겹칠 수 있습니다.

윤곽선의 다음 메서드를 호출 하 여 일반적으로 시작 `SKPath`:

- `MoveTo` 새 윤곽선을 시작 하려면

해당 메서드에 인수가 단일 지점으로 표현할 수 있습니다는 `SKPoint` 값 또는 별도 X 및 Y 좌표입니다. `MoveTo` 윤곽선과 초기 시작 부분에 지점을 설정 하는 호출 *현재 지점*합니다. 선 또는 면 연결이 새 현재 지점 메서드에 지정 된 지점에 현재 위치에서 곡선 윤곽을 계속 하려면 다음 메서드를 호출할 수 있습니다.

- `LineTo` 경로에 직선을 추가 하려면
- `ArcTo` 선이 원 또는 타원의 호를 추가 하려면
- `CubicTo` 입방 형 3 차원 곡선 스플라인을 추가 하려면
- `QuadTo` 정방형 3 차원 곡선 스플라인을 추가 하려면
- `ConicTo` 유리 정방형 베 지 어 스플라인을, 원추형 섹션 (타원, parabolas, 및 hyperbolas)를 정확 하 게 렌더링할 수 있는 추가 하려면

이러한 5 개 방법을 선 또는 곡선에 설명 하는 데 필요한 모든 정보를 포함 합니다. 이러한 5 개 메서드의 메서드 호출 바로 앞에서 설정한 현재 지점과 함께에서 작동 합니다. 예를 들어는 `LineTo` 직선 윤곽을 기반으로 현재 지점 이므로 매개 변수를 추가 하는 메서드 `LineTo` 단일 지점에서 합니다.

`SKPath` 클래스에는 또한 하지만 이러한 6 개의 메서드와 동일한 이름을 갖는 메서드를 정의 `R` 시작 부분에서:

- `RMoveTo`
- `RLineTo`
- `RArcTo`
- `RCubicTo`
- `RQuadTo`
- `RConicTo`

`R` 에 *상대*합니다. 하지 않고도 해당 메서드와 같은 구문을 갖습니다는 `R` 하지만 현재 시점을 기준으로 합니다. 이 여러 번 호출 하는 메서드의에서 패스의 유사한 부분을 그리는 데 유용 합니다.

윤곽선 다른 호출으로 끝나는 `MoveTo` 또는 `RMoveTo`, 호출이 나 새 윤곽선 시작 `Close`, 윤곽 닫습니다는 합니다. `Close` 메서드 자동으로 추가 하 고 첫 번째는 곡선의 현재 위치에서 직선 하 고 닫힌 것으로 경로 표시 즉, 모든 스트로크 단면 없이 렌더링 되도록 합니다.

열려 있는 트랜잭션과 닫혀 윤곽선 간의 차이를 보여 줍니다는 **두 개의 삼각형 윤곽선** 페이지를 사용 하는 `SKPath` 두 개의 삼각형을 렌더링 하는 두 개의 윤곽선 인 개체입니다. 첫 번째 윤곽선 열려 있고 두 번째 닫힙니다. 다음은 [ `TwoTriangleContours` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs) 클래스:

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

첫 번째 윤곽선에 대 한 호출 이루어져 [ `MoveTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.MoveTo/p/System.Single/System.Single/) X 및 Y 좌표를 사용 하 여 아닌 `SKPoint` 를 세 번 호출 하 여 뒤에 오는 값 [ `LineTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.LineTo/p/System.Single/System.Single/) 의 세 가지 면을 그리는 데는 삼각형입니다. 두 번째 윤곽 한 두 개의 호출이 `LineTo` 윤곽을 호출 하 여 완료 하지만 [ `Close` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Close()/), 윤곽을 닫습니다. 큰 차이:

[![](paths-images/twotrianglecontours-small.png "두 개의 삼각형 윤곽선 페이지의 삼중 스크린샷")](paths-images/twotrianglecontours-large.png#lightbox "두 개의 삼각형 윤곽선 페이지의 삼중 스크린샷")

볼 수 있듯이 첫 번째 윤곽선은 일련의 세 가지 연결 된 선 분명히 있지만 시작 부분과 끝 연결 되지는 않습니다. 위쪽에서 두 줄 겹칩니다. 두 번째 윤곽 분명히 닫혀 있고 더 적은 하나를 사용 하 여 수행 된 `LineTo` 없어 해당 메서드가 호출 된 `Close` 메서드 윤곽을 마지막 줄을 자동으로 추가 합니다.

`SKCanvas` 하나만 정의 [ `DrawPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawPath/p/SkiaSharp.SKPath/SkiaSharp.SKPaint/) 메서드를이 데모를 두 번 호출을 채우고 패스 선입니다. 모든 윤곽선 가득 차면 닫혀 있지 않은 하는 합니다. 닫히지 않은 경로 채우기 위해 직선 가정 윤곽선의 시작점 및 끝점 사이 존재 합니다. 마지막을 제거 하면 `LineTo` 첫 번째 윤곽선 또는 제거는 `Close` 호출에서 두 번째 윤곽 각 윤곽선을 마치는 삼각형을 채울 수 있지만 두 개의 측면을 갖습니다.

`SKPath` 여러 가지 메서드와 속성을 정의합니다. 다음 방법 경로 닫히거나 닫히지 방법에 따라 수를 전체 윤곽선을 추가 합니다.

- `AddRect`
- [`AddRoundedRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddRoundedRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddCircle`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddCircle/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddOval`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddOval/p/SkiaSharp.SKRect/SkiaSharp.SKPathDirection/)
- [`AddArc`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) 추가 되는 타원의 원 둘레에 곡선을 추가 하려면
- `AddPath` 현재 경로에 다른 경로 추가 하려면
- [`AddPathReverse`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddPathReverse/p/SkiaSharp.SKPath/) 반대 방향으로 다른 경로 추가 하려면

한 `SKPath` 개체 정의 geometry &mdash; 일련의 포인트와 연결 합니다. 경우에만 `SKPath` 와 결합 하는 `SKPaint` 개체는 특정 색, 스트로크 너비 등을 사용 하 여 렌더링 경로입니다. 또한 한다는 점에 유의 하는 `SKPaint` 에 전달 된 개체는 `DrawPath` 메서드는 전체 경로 특성을 정의 합니다. 여러 색 요구 무언가 그려야 하려는 경우에 각 색에 대 한 별도 경로 사용 해야 합니다.

두 줄 사이의 연결의 모양을 정의한 범위의 시작 줄의 끝 모양을 선 단면에 정의 된 처럼는 *획 조인*합니다. 이 설정 하 여 지정 된 [ `StrokeJoin` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeJoin/) 의 속성 `SKPaint` 의 멤버에는 [ `SKStrokeJoin` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeJoin/) 열거형:

- [`Miter`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Miter/) 뾰족한 조인에 대 한
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Round/) 둥근된 조인에 대 한
- [`Bevel`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Bevel/) 되었다고 해 오프 조인에 대 한

**획 조인** 페이지와 유사한 코드를 있는 조인은 이러한 세 개의 선을 표시는 **스트로크 단면** 페이지. 이는 `PaintSurface` 의 이벤트 처리기는 [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs) 클래스:

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

다음은 세 가지 플랫폼에서 실행 중인 프로그램입니다.

[![](paths-images/strokejoins-small.png "선 조인 페이지의 삼중 스크린샷")](paths-images/strokejoins-large.png#lightbox "획 조인 페이지의 삼중 스크린 샷")

이 음 줄 연결할 수 있는 날카로운 지점으로 구성 됩니다. 두 줄을 작은 각도로 조인 마이터 연결 상당히 길어질 수 있습니다. 지나치게 긴 이음이 조인을 방지 하려면 마이터 연결의 길이 값에 의해 제한 됩니다는 [ `StrokeMiter` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeMiter/) 속성 `SKPaint`합니다. 이 길이 초과 하는 음 경사 연결 되도록 잘려 됩니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
