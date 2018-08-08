---
title: 텍스트와 그래픽 통합
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에 SkiaSharp 그래픽을 사용 하 여 텍스트를 통합 하는 렌더링 된 텍스트 문자열의 크기를 결정 하는 방법에 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: be1524029ada79896f83517c3b439f2ad0e2c6d9
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615163"
---
# <a name="integrating-text-and-graphics"></a>텍스트와 그래픽 통합

_SkiaSharp 그래픽을 사용 하 여 텍스트를 통합 하는 렌더링 된 텍스트 문자열의 크기를 확인 하는 방법을 참조 하세요._

이 문서에 텍스트를 측정, 가능한 경우 텍스트는 특정 크기를 확장 및 기타 그래픽을 사용 하 여 텍스트를 통합 하는 방법을 보여 줍니다.

![](text-images/textandgraphicsexample.png "사각형으로 묶은 텍스트")

SkiaSharp `Canvas` 클래스는 사각형을 그리는 방법에도 포함 되어 있습니다 ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) 및 모퉁이가 둥근 사각형 ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/)). 이러한 메서드에 필요 사각형으로 정의할 수는 `SKRect` 값입니다.

합니다 **묶을 텍스트** 페이지 센터 페이지에 항목을 그릴 모서리가 둥근된 사각형의 쌍으로 이루어진 프레임을 사용 하 여 짧은 텍스트 문자열입니다. 합니다 [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) 클래스가 어떻게 수행 되는지 보여 줍니다.

SkiaSharp 사용 하 여는 `SKPaint` 텍스트 및 글꼴 특성 설정 해도 클래스도 사용할 수 텍스트의 렌더링 된 크기를 가져올 수 있습니다. 다음 부분 `PaintSurface` 이벤트 처리기 호출 두 개의 다른 `MeasureText` 메서드. 첫 번째 [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) 호출에는 간단한 `string` 인수와 현재 글꼴 특성을 기반으로 하는 텍스트의 픽셀 너비를 반환 합니다. 프로그램 후 새 계산 `TextSize` 의 속성을 `SKPaint` 렌더링 된 너비, 현재를 기준으로 개체 `TextSize` 속성 및 표시 영역 너비입니다. 설정 하는 것이 `TextSize` 텍스트 문자열 화면의 너비의 90%로 렌더링 될 수 있도록 합니다.

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
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

두 번째 [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/) 호출에는 `SKRect` 인수를 사용할 수 있도록 렌더링된 된 텍스트의 높이 및 너비를 가져옵니다. 합니다 `Height` 속성을이 `SKRect` 값 대문자로, 어센더 및 텍스트 문자열에서 디센더와의 유무에 따라 달라 집니다. 다른 `Height` mom"텍스트 문자열", "고양이" 및 "dog", 예를 들어 값을 보고 합니다.

`Left` 및 `Top` 의 속성을 `SKRect` 구조에서 텍스트 표시 되 면 렌더링된 된 텍스트의 왼쪽 위 모퉁이의 좌표를 나타냅니다는 `DrawText` 사용 하 여 호출 0 X 및 Y 위치. 이 프로그램이 7 iPhone 시뮬레이터에서 실행 되는 경우에 예를 들어 `TextSize` 90.6254 첫 번째 호출은 다음 계산의 결과로 값이 할당 됩니다 `MeasureText`합니다. 합니다 `SKRect` 두 번째 호출에서 얻은 값 `MeasureText` 에 다음 속성 값:

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88;

X 및 Y 좌표 하는 것에 유의 전달할를 `DrawText` 메서드 기준선에서 텍스트의 왼쪽을 지정 합니다. `Top` 값 텍스트 (68 it 88에서 뺀) 및 기준선 위에 68 픽셀은 확장을 나타냅니다 기준선 아래 20 픽셀입니다. 합니다 `Left` 값이 6 이면 6 픽셀 X 값의 오른쪽을 시작 하는 텍스트를 `DrawText` 호출 합니다. 따라서 일반적인 문자 간 간격입니다. 화면의 왼쪽 위 모서리에 있는 단단히 고정 텍스트를 표시 하려는 경우 이러한 부정 전달 `Left` 하 고 `Top` X 및 Y 좌표 값 `DrawText`,이 예제에서는 &ndash;6 및 68 합니다.

합니다 `SKRect` 여러 유용한 속성 및 메서드를 나머지에 사용 되는 일부 구조 정의 `PaintSurface` 처리기입니다. 합니다 `MidX` 고 `MidY` 값 중심 사각형의 좌표를 나타냅니다. (7 iPhone 예제에서는 이러한 값은 338.4107 및 &ndash;24.) 다음 코드를 사용 하면 좌표에 대 한 가장 간단한 계산에 대 한 이러한 값을 사용 하는 화면의 텍스트를 가운데.

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

`PaintSurface` 처리기에 대 한 두 호출 끝나는 `DrawRoundRect`, 인수 둘 다 필요 `SKRect`합니다. 이 `SKRect` 값은 물론 비슷합니다는 `SKRect` 에서 가져온 값을 `MeasureText` 메서드 하지만 같을 수 없습니다. 먼저 모퉁이가 둥근된 사각형 하지 텍스트의 가장자리를 통해 그리고 둘째, 공간에서 이동 해야 할 수 있도록 약간 클 해야 되도록 합니다 `Left` 및 `Top` 사각형 되도록 여기서는 왼쪽 위 모퉁이에 해당 하는 값 에 위치합니다. 이러한 두 작업으로 이루어집니다 `Offset` 하 고 `Inflate` 정의한 메서드 `SKRect`:

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

그런 다음, 나머지 메서드는 간단 합니다. 다른 만들면 `SKPaint` 테두리 및 호출에 대 한 개체 `DrawRoundRect` 두 번입니다. 두 번째 호출은 다른 10 픽셀 x 팽창 사각형을 사용 합니다. 첫 번째 호출에 20 픽셀; 모퉁이 반경을 지정합니다 두 번째 30 픽셀의 모퉁이 반경이 있으므로 평행 하는 것 같습니다.

 [![](text-images/framedtext-small.png "묶을 텍스트 페이지의 3 배가 스크린샷")](text-images/framedtext-large.png#lightbox "삼중 묶을 텍스트 페이지 스크린샷")

텍스트와 크기가 프레임을 확인 하려면 휴대폰 이나 옆으로 돌려 시뮬레이터를 설정할 수 있습니다.

화면에 일부 텍스트를 가운데에 필요한 경우 설정 하 여 텍스트를 측정 하는 약 하지 않고 수행할 수 있습니다 합니다 `TextAlign` 속성을 `SKPaint` 에 `SKTextAlign.Center`입니다. 지정한 X 좌표를 `DrawText` 메서드 후 텍스트의 가로 가운데 위치를 나타냅니다. 화면의 중간점을 전달 하는 경우는 `DrawText` 메서드를 텍스트를 가로 방향으로 가운데에 표시 됩니다 및 *거의* 기준선 세로로 가운데 때문에 세로로 가운데에 있습니다.

그래픽 옵션 같은 훨씬 텍스트 자체를 처리할 수 있습니다. 한 가지 간단한 옵션의 일반적인 채워진된 표시 하지 않고 텍스트 문자 윤곽선을 표시 하는 것:

[![](text-images/outlinedtext-small.png "윤곽선이 있는 텍스트 페이지의 스크린 샷을 3 배나 늘리고")](text-images/outlinedtext-large.png#lightbox "삼중 윤곽선이 있는 텍스트 페이지의 스크린 샷")

법선을 변경 하 여 이렇게 `Style` 의 속성을 `SKPaint` 개체의 기본 설정인 `SKPaintStyle.Fill` 를 `SKPaintStyle.Stroke` 스트로크 너비를 지정 하 여. `PaintSurface` 처리기는 **윤곽선이 있는 텍스트** 페이지 어떻게 수행 되는지 보여 줍니다.

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
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

 지난 몇 가지 문서에서는 해야 하면 지금 텍스트를 그릴 SkiaSharp 사용 하는 방법을 살펴보았으며, 그릴 모서리가 둥근된 사각형 및 타원을 원 다음 단계 [SkiaSharp 선 및 경로](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md) 그래픽 경로에 연결 된 선을 그리는 방법을 learn 됩니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
