---
title: ''
description: 이 문서에서는 SkiaSharp 그래픽과 텍스트를 응용 프로그램에 통합 하기 위해 렌더링 된 텍스트 문자열의 크기를 결정 하는 방법을 설명 하 Xamarin.Forms 고 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ee97ee2aae11e4e54a0d25e80ffd7bce301fa2f3
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137685"
---
# <a name="integrating-text-and-graphics"></a>텍스트와 그래픽 통합

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp 그래픽과 텍스트를 통합 하기 위해 렌더링 된 텍스트 문자열의 크기를 결정 하는 방법을 참조 하세요._

이 문서에서는 텍스트를 측정 하 고 텍스트를 특정 크기로 확장 하 고 텍스트를 다른 그래픽과 통합 하는 방법을 보여 줍니다.

![](text-images/textandgraphicsexample.png "Text surrounded by rectangles")

이 이미지에는 모퉁이가 둥근 사각형도 포함 됩니다. SkiaSharp 클래스에는 사각형과 모퉁이가 둥근 사각형을 그리기 위한 메서드를 `Canvas` 그리는 메서드가 포함 되어 있습니다 [`DrawRect`](xref:SkiaSharp.SKCanvas.DrawRect*) [`DrawRoundRect`](xref:SkiaSharp.SKCanvas.DrawRoundRect*) . 이러한 메서드를 사용 하 여 사각형을 값으로 정의 `SKRect` 하거나 다른 방식으로 정의할 수 있습니다.

**프레임 텍스트** 페이지는 페이지에 짧은 텍스트 문자열을 가운데에 배치 하 여 모퉁이가 둥근 사각형으로 구성 된 프레임으로 둘러쌉니다. [`FramedTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs)클래스는 수행 방법을 보여 줍니다.

SkiaSharp에서는 클래스를 사용 하 여 `SKPaint` 텍스트 및 글꼴 특성을 설정 하지만이를 사용 하 여 렌더링 된 텍스트 크기를 가져올 수도 있습니다. 다음 이벤트 처리기의 시작 부분에서는 `PaintSurface` 두 가지 메서드를 호출 `MeasureText` 합니다. 첫 번째 [`MeasureText`](xref:SkiaSharp.SKPaint.MeasureText(System.String)) 호출에는 간단한 `string` 인수가 있으며 현재 글꼴 특성에 따라 텍스트의 픽셀 너비를 반환 합니다. 그러면 프로그램에서 `TextSize` `SKPaint` 렌더링 된 너비, 현재 `TextSize` 속성 및 표시 영역의 너비를 기준으로 개체의 새 속성을 계산 합니다. 이 계산은 `TextSize` 텍스트 문자열이 화면 너비의 90%에서 렌더링 되도록 설정 하기 위한 것입니다.

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

두 번째 [`MeasureText`](xref:SkiaSharp.SKPaint.MeasureText(System.String,SkiaSharp.SKRect@)) 호출에는 `SKRect` 인수가 있으므로 렌더링 된 텍스트의 너비와 높이를 모두 가져옵니다. `Height`이 값의 속성은 `SKRect` 텍스트 문자열에 대문자 (어센더)와 하강 문자가 있는지 여부에 따라 달라 집니다. `Height`예를 들어 텍스트 문자열 "mom", "cat" 및 "dog"에 대해 서로 다른 값이 보고 됩니다.

`Left` `Top` 구조체의 및 속성은 `SKRect` `DrawText` X 및 Y 위치가 0 인 호출에 의해 텍스트가 표시 되는 경우 렌더링 된 텍스트의 왼쪽 위 모퉁이의 좌표를 표시 합니다. 예를 들어이 프로그램이 iPhone 7 시뮬레이터에서 실행 되는 경우 `TextSize` 에 대 한 첫 번째 호출 후 계산 결과에는 90.6254 값이 할당 됩니다 `MeasureText` . `SKRect`에 대 한 두 번째 호출에서 가져온 값에는 `MeasureText` 다음과 같은 속성 값이 있습니다.

- `Left`= 6
- `Top` = &ndash;68
- `Width`= 664.8214
- `Height`= 88;

메서드에 전달 하는 X 및 Y 좌표는 `DrawText` 기준선에서 텍스트의 왼쪽을 지정 합니다. `Top`이 값은 텍스트가 기준선 보다 68 픽셀을 확장 하 고 기준선 아래에서 20 픽셀 (88에서 68)을 빼는 것을 나타냅니다. `Left`6 값은 텍스트가 호출에서 X 값의 오른쪽에 6 픽셀 시작 됨을 나타냅니다 `DrawText` . 이렇게 하면 일반적인 문자 간 간격을 사용할 수 있습니다. 표시의 왼쪽 위 모퉁이에 snugly 텍스트를 표시 하려면 이러한 및 값의 음수를 `Left` `Top` X 및 Y 좌표로 전달 합니다 `DrawText` .이 예제에서는 &ndash; 6과 68입니다.

`SKRect`구조체는 몇 가지 유용한 속성 및 메서드를 정의 합니다 .이 중 일부는 처리기의 나머지 부분에서 사용 됩니다 `PaintSurface` . `MidX`및 `MidY` 값은 사각형의 중심 좌표를 표시 합니다. (IPhone 7 예제에서 이러한 값은 338.4107 및 &ndash; 24입니다.) 다음 코드에서는 좌표를 가장 쉽게 계산 하기 위해 이러한 값을 사용 하 여 텍스트를 표시 합니다.

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

`SKImageInfo`또한 info 구조는 [`Rect`](xref:SkiaSharp.SKImageInfo.Rect) 형식의 속성을 정의 `SKRect` 하므로 `xText` `yText` 다음과 같이 계산할 수도 있습니다.

```csharp
float xText = info.Rect.MidX - textBounds.MidX;
float yText = info.Rect.MidY - textBounds.MidY;
```

`PaintSurface`처리기는의 인수를 필요로 하는에 대 한 두 호출로 끝납니다 `DrawRoundRect` `SKRect` . 이 `SKRect` 값은 `SKRect` 메서드에서 가져온 값을 기반으로 `MeasureText` 하지만 동일 하지는 않습니다. 첫째, 모퉁이가 둥근 사각형에서 텍스트 가장자리에 그려지지 않도록 작은 값이 필요 합니다. 두 번째로, `Left` 및 `Top` 값이 사각형을 배치할 왼쪽 위 모퉁이에 해당 하는 공간으로 이동 해야 합니다. 이러한 두 작업은에 의해 [`Offset`](xref:SkiaSharp.SKRect.Offset*) 정의 된 및 메서드로 수행 됩니다 [`Inflate`](xref:SkiaSharp.SKRect.Inflate*) `SKRect` .

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

그 다음에는 메서드의 나머지가 바로 앞에 있습니다. `SKPaint`테두리 및 호출에 대 한 다른 개체를 `DrawRoundRect` 두 번 만듭니다. 두 번째 호출에서는 팽창 사각형을 사용 합니다. 첫 번째 호출에서는 20 픽셀의 모퉁이 반경을 지정 합니다. 두 번째는 모퉁이 반지름이 30 픽셀인 데, 다음과 같이 평행 하 게 보입니다.

 [![](text-images/framedtext-small.png "Triple screenshot of the Framed Text page")](text-images/framedtext-large.png#lightbox "Triple screenshot of the Framed Text page")

휴대폰 또는 시뮬레이터를 옆으로 설정 하 여 텍스트와 프레임 크기가 증가 하는 것을 볼 수 있습니다.

화면에 일부 텍스트를 가운데로 맞추려면 텍스트를 측정 하지 않고 약간의 텍스트를 입력할 수 있습니다. 대신 [`TextAlign`](xref:SkiaSharp.SKPaint.TextAlign) 의 속성을 `SKPaint` 열거형 멤버로 설정 합니다 [`SKTextAlign.Center`](xref:SkiaSharp.SKTextAlign) . 메서드에서 지정 하는 X 좌표는 `DrawText` 텍스트의 가로 가운데가 배치 되는 위치를 나타냅니다. 화면 중간점을 메서드에 전달 하는 경우 기준선이 `DrawText` 세로로 가운데 맞춤 되기 때문에 텍스트는 가로 *방향으로* 가운데에 맞춰지고 세로로 가운데 맞춤 됩니다.

텍스트는 다른 그래픽 개체와 비슷하게 처리 될 수 있습니다. 한 가지 간단한 옵션은 텍스트 문자의 개요를 표시 하는 것입니다.

[![](text-images/outlinedtext-small.png "Triple screen shot of the Outlined Text page")](text-images/outlinedtext-large.png#lightbox "Triple screenshot of the Outlined Text page")

이는 단순히 `Style` 개체의 normal 속성을 `SKPaint` 의 기본 설정인에서로 변경 하 `SKPaintStyle.Fill` `SKPaintStyle.Stroke` 고 스트로크 너비를 지정 하 여 수행 합니다. `PaintSurface` **윤곽선이 있는 텍스트** 페이지의 처리기는 다음과 같은 작업을 수행 하는 방법을 보여 줍니다.

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

또 다른 일반적인 그래픽 개체는 비트맵입니다. [**SkiaSharp 비트맵**](../bitmaps/index.md)섹션에서 자세히 설명 하는 많은 주제 이지만 다음 문서 [**SkiaSharp의 비트맵 기본 사항**](bitmaps.md)에서 보다 간단 소개를 제공 합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
