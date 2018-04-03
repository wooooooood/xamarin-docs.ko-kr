---
title: 텍스트와 그래픽 통합
description: 렌더링된 텍스트 SkiaSharp 그래픽 텍스트를 통합 하는 문자열의 크기를 결정 하는 방법을 참조 하십시오.
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 1607fe31785b6793175dfb61e1e12e34429aa089
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2018
---
# <a name="integrating-text-and-graphics"></a>텍스트와 그래픽 통합

_렌더링된 텍스트 SkiaSharp 그래픽 텍스트를 통합 하는 문자열의 크기를 결정 하는 방법을 참조 하십시오._

이 문서에 텍스트를 측정, 가능한 텍스트 특정 크기를 확장 및 다른 그래픽와 텍스트를 통합 하는 방법을 보여 줍니다.

![](text-images/textandgraphicsexample.png "사각형으로 묶은 텍스트")

SkiaSharp `Canvas` 클래스 사각형을 그릴 수 있는 메서드가 포함 되어 ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) 모퉁이가 둥근 사각형 ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/)). 이러한 방법으로 정의할 수 사각형 필요는 `SKRect` 값입니다.

**묶을 텍스트** 페이지는 짧은 텍스트 문자열은 페이지 프레임으로 모퉁이가 둥근된 사각형의 쌍으로 구성 항목에서 가운데에 맞춥니다. [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) 클래스가 수행 하는 방법을 보여 줍니다.

SkiaSharp 사용 하 여는 `SKPaint` 글꼴 및 텍스트 특성 설정 하는 클래스도 사용할 수 텍스트의 렌더링 된 크기를 구합니다. 다음의 시작 부분 `PaintSurface` 이벤트 처리기 호출 두 개의 다른 `MeasureText` 메서드. 첫 번째 [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) 호출에는 간단한 `string` 인수와 현재 글꼴 특성을 기준으로 하는 텍스트의 픽셀 너비 반환 합니다. 프로그램이 다음 새 계산 `TextSize` 속성은 `SKPaint` 해당 렌더링된 너비 현재를 기준으로 개체 `TextSize` 속성과 표시 영역의 너비로 합니다. 설정 하기 위한 용도가이 `TextSize` 화면의 너비의 90%로 렌더링 해야 하는 텍스트 문자열 있도록:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string str = "Hello SkiaSharp!";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Chocolate
    };

    // Adjust TextSize property so text is 90% of screen width
    float textWidth = textPaint.MeasureText(str);
    textPaint.TextSize = 0.9f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds;
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

두 번째 [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/) 호출에는 `SKRect` 인수를 사용할 수 있으므로 렌더링된 된 텍스트의 높이 너비를 가져옵니다. `Height` 속성 `SKRect` 값 대문자로, 어센더 및 텍스트 문자열에서 디센더와의 유무에 따라 달라 집니다. 다른 `Height` 값 텍스트 문자열 "mom", "cat" 및 "dog"에 대 한 예를 들어 보고 됩니다.

`Left` 및 `Top` 의 속성에서 `SKRect` 구조는 텍스트는 표시 하는 경우 렌더링된 된 텍스트의 왼쪽 위 모퉁이의 좌표를 나타내는 `DrawText` 하 여 호출 0 X 및 Y 위치입니다. 이 프로그램 7 iPhone 시뮬레이터에서 실행 중인 경우에 예를 들어 `TextSize` 호출 다음에 첫 번째 계산의 결과로 90.6254 값이 할당 `MeasureText`합니다. `SKRect` 두 번째 호출에서 얻은 값 `MeasureText` 속성 값은 다음과 같습니다.

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88;

X 및 Y 좌표 수는 명심에 전달할는 `DrawText` 메서드 기준선에 텍스트의 왼쪽을 지정 합니다. `Top` 값 텍스트 확장 68 픽셀 기준선 및 (68 it에서 88 빼기) 위에 있는지 여부를 나타냅니다 기준선 아래로 20 픽셀입니다. `Left` 6 픽셀에 X 값의 오른쪽을 시작 하는 텍스트 값이 6 이면는 `DrawText` 호출 합니다. 일반 문자 간 간격 수 있습니다. 디스플레이의 왼쪽 위 모서리에 단단히 고정 텍스트를 표시 하려는 경우 이러한 부정 전달 `Left` 및 `Top` 의 X 및 Y 좌표에 따라 값이 `DrawText`,이 예제에서는 &ndash;6 및 68 합니다.

`SKRect` 몇 가지 유용한 속성 및 메서드의의 나머지 부분에 사용 되는 다음과 같은 구조 정의 `PaintSurface` 처리기입니다. `MidX` 및 `MidY` 중심 사각형의 좌표를 나타내는 값입니다. (7 iPhone 예제에서는 이러한 값은 338.4107 및 &ndash;24.) 다음 코드를 사용 하면 좌표 쉬운 계산에 대 한 이러한 값을 사용 하는 디스플레이에서 텍스트를 가운데에:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(str, xText, yText, textPaint);
    ...
}
```

`PaintSurface` 처리기를 두 번 호출 끝나는 `DrawRoundRect`의 인수는 둘 다 있어야 `SKRect`합니다. 이 `SKRect` 값은 확실히 비슷합니다는 `SKRect` 에서 가져온 값의 `MeasureText` 메서드, 하지만 같을 수 없습니다. 모퉁이가 둥근된 사각형은 텍스트의 모서리 위에 그려지지 하 고 이동 하 여 공간에서 하는 데 필요한 두 번째로, 있도록 약간 더 큰 하는 데 필요한 먼저 있도록는 `Left` 및 `Top` 사각형 되도록 여기서는 왼쪽 위 모서리에 해당 하는 값 에 위치합니다. 이러한 두 작업을 수행 하 `Offset` 및 `Inflate` 정의한 메서드 `SKRect`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Create a new SKRect object for the frame around the text
    SKRect frameRect = textBounds;
    frameRect.Offset(xText, yText);
    frameRect.Inflate(10, 10);

    // Create an SKPaint object to display the frame
    SKPaint framePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5,
        Color = SKColors.Blue
    };

    // Draw one frame
    canvas.DrawRoundRect(frameRect, 20, 20, framePaint);

    // Inflate the frameRect and draw another
    frameRect.Inflate(10, 10);
    framePaint.Color = SKColors.DarkBlue;
    canvas.DrawRoundRect(frameRect, 30, 30, framePaint);
}
```

그런 다음, 메서드의 나머지 부분에서는 간단한 것입니다. 다른 만듭니다 `SKPaint` 테두리 및 호출에 대 한 개체 `DrawRoundRect` 두 번입니다. 두 번째 호출은 다른 10 픽셀 x 팽창 사각형을 사용 합니다. 첫 번째 호출의 20 픽셀; 모서리 반지름을 지정합니다. 두 번째는 평행 하 게 되므로 30 픽셀의 모서리 반지름을 있습니다.

 [![](text-images/framedtext-small.png "묶을 텍스트 페이지의 삼중 스크린샷")](text-images/framedtext-large.png#lightbox "묶을 텍스트 페이지의 삼중 스크린 샷")

전화 또는 옆으로 시뮬레이터 텍스트와 크기가 증가 하는 프레임을 설정할 수 있습니다.

화면에 일부 텍스트를 가운데에 해야 하는 경우 텍스트를 설정 하 여 측정 않고 약 수행할 수 있습니다는 `TextAlign` 속성 `SKPaint` 를 `SKTextAlign.Center`합니다. 지정한 X 좌표는 `DrawText` 메서드 다음 텍스트의 가로 가운데 배치 되는 위치를 나타냅니다. 화면 중간점을 전달 하는 경우는 `DrawText` 메서드를 텍스트는 가로로 가운데 및 *거의* 세로로 가운데에 세로로 가운데 기준선 때문에 있습니다.

그래픽 옵션 처럼 훨씬 텍스트 자체를 처리할 수 있습니다. 한 가지 간단한 옵션 표준 채워진된 디스플레이 아닌 텍스트 문자의 개요를 표시 하는 것:

[![](text-images/outlinedtext-small.png "설명 된 텍스트 페이지의 스크린 샷을 삼중")](text-images/outlinedtext-large.png#lightbox "삼중 설명 된 텍스트 페이지의 스크린 샷")

일반 변경 하 여 이렇게 `Style` 속성은 `SKPaint` 개체에서 기본 설정인 `SKPaintStyle.Fill` 를 `SKPaintStyle.Stroke` 스트로크 너비를 지정 하 여 합니다. `PaintSurface` 의 처리기는 **설명 텍스트** 페이지 수행 하는 방법을 보여 줍니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "OUTLINE";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 1,
        FakeBoldText = true,
        Color = SKColors.Blue
    };

    // Adjust TextSize property so text is 95% of screen width
    float textWidth = textPaint.MeasureText(text);
    textPaint.TextSize = 0.95f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds;
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

 지난 몇 가지 문서에서 있는 있습니다 SkiaSharp 텍스트를 그리는 데 사용 하는 방법을 표시 하는 이제 원, 타원과 모퉁이가 둥근된 사각형입니다. 다음 단계는 [SkiaSharp 선과 경로](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md) 의 그래픽 경로에 연결 된 선을 그리는 방법을 알아보려면 됩니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
