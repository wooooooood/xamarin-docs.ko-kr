---
title: 경로 및 텍스트
description: 경로 및 텍스트의 교차점에 알아보고
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 08/01/2017
ms.openlocfilehash: c0b793a495278d91429045d7e396917d02c1412e
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/05/2018
---
# <a name="paths-and-text"></a>경로 및 텍스트

_경로 및 텍스트의 교차점에 알아보고_

최신 그래픽 시스템의 텍스트 글꼴은 일반적으로 정방형 베 지 어 곡선으로 분할 하 여 정의 되는 문자 윤곽선의 컬렉션. 따라서 대부분의 최신 그래픽 시스템 텍스트 문자 그래픽 경로로 변환할 수 있는 기능을 포함 합니다. 

있는지 있습니다 수 텍스트 문자의 윤곽선 스트로크으로 채우는 이미 살펴보았습니다. 에 설명 된 대로 특정 스트로크 너비와 경로 효과 이러한 문자 윤곽선을 표시할 수 있습니다는 [ **경로 효과** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) 문서. 에 문자열을 변환할 수 이기도 하지만 `SKPath` 개체입니다. 에 설명 된 기술로 클리핑에 대 한 텍스트 윤곽선을 사용할 수 있다는 의미이 고 [ **경로 및 영역을 사용 하 여 클리핑** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md) 문서. 

경로 효과 사용 하 여 문자 개요 스트로크를 외에도 효과 경로 기반으로 하는 문자열에서 파생 되는 경로 만들 수도 있습니다 하 고 두 효과 결합할 수도 있습니다.

![](text-paths-images/pathsandtextsample.png "경로 텍스트 효과")

에 [이전 문서](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) 언급 했 듯이 방법을 [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) 방식의 `SKPaint` 선된 패스의 개요를 얻을 수 있습니다. 또한 문자 윤곽선에서 파생 된 경로와이 메서드를 사용할 수 있습니다.

마지막으로,이 문서에서는 경로 텍스트의 다른 교차:는 [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) 방식의 `SKCanvas` 텍스트의 기준선 곡선된 경로 뒤에 오도록 텍스트 문자열을 표시할 수 있습니다.

## <a name="text-to-path-conversion"></a>텍스트 변환 경로를

[ `GetTextPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetTextPath/p/System.String/System.Single/System.Single/) 방식의 `SKPaint` 문자열을 변환는 `SKPath` 개체:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

`x` 및 `y` 인수는 텍스트의 왼쪽 기준의 시작 지점을 나타냅니다. 와 같이 동일한 역할 여기 재생 되는 `DrawText` 방식의 `SKCanvas`합니다. 경로 내에서 텍스트의 왼쪽의 기준선 (x, y) 좌표를 갖습니다. 

`GetTextPath` 메서드는 개념을 세우는 단순히을 채우거 나 결과 경로 스트로크 하려는 경우. 법선의 `DrawText` 메서드를 사용 하면 작업을 수행할 수 있습니다. `GetTextPath` 메서드는 경로 관련 된 다른 작업에 더 유용 합니다.

이러한 태스크 중 하나에 맞추는 됩니다. **클리핑 텍스트** 페이지 "코드입니다." 라는 단어의 문자 윤곽선에 따라 클리핑 패스를 만듭니다. 이 경로를 이미지를 포함 하는 비트맵을 자르는 페이지 크기를 위해 확장 된 **클리핑 텍스트** 소스 코드:

[![](text-paths-images/clippingtext-small.png "클리핑 텍스트 페이지의 삼중 스크린샷")](text-paths-images/clippingtext-large.png#lightbox "클리핑 텍스트 페이지의 삼중 스크린 샷")

[ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs) 에 포함 리소스로 저장 되어 있는 비트맵을 로드 하는 클래스 생성자는 **미디어** 솔루션의 폴더:

```csharp
public class ClippingTextPage : ContentPage
{
    SKBitmap bitmap;

    public ClippingTextPage()
    {
        Title = "Clipping Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.PageOfCode.png";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

`PaintSurface` 처리기를 만들어 시작 프로그램 `SKPaint` 개체 텍스트에 적합 합니다. `Typeface` 속성이 설정 되어으로 `TextSize`, 특정 응용 프로그램에 대해 있지만 `TextSize` 속성은 arbirtrary 순수 하 게 합니다. 또한 것을 볼 수 없는 `Style` 설정 합니다.

`TextSize` 및 `Style` 속성 설정이 필요 하지 않습니다. 때문에이 `SKPaint` 개체에만 사용 되는 `GetTextPath` 텍스트 문자열 "코드"를 사용 하 여 호출 합니다. 처리기는 다음 결과 측정 `SKPath` 개체 하 고 가운데 및 페이지의 크기를 조정 하는 세 가지 변환을 적용 합니다. 경로는 클리핑 패스도 설정할 수 있습니다.

```csharp
public class ClippingTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Blue);

        using (SKPaint paint = new SKPaint())
        {
            paint.Typeface = SKTypeface.FromFamilyName(null, SKTypefaceStyle.Bold);
            paint.TextSize = 10;

            using (SKPath textPath = paint.GetTextPath("CODE", 0, 0))
            {
                // Set transform to center and enlarge clip path to window height
                SKRect bounds;
                textPath.GetTightBounds(out bounds);

                canvas.Translate(info.Width / 2, info.Height / 2);
                canvas.Scale(info.Width / bounds.Width, info.Height / bounds.Height);
                canvas.Translate(-bounds.MidX, -bounds.MidY);

                // Set the clip path
                canvas.ClipPath(textPath);
            }
        }

        // Reset transforms
        canvas.ResetMatrix();

        // Display bitmap to fill window but maintain aspect ratio
        SKRect rect = new SKRect(0, 0, info.Width, info.Height);
        canvas.DrawBitmap(bitmap, 
            rect.AspectFill(new SKSize(bitmap.Width, bitmap.Height)));
    }
}
```

클리핑 패스 설정 되 고 나면 비트맵을 표시 하 고 문자 윤곽선에 클리핑됩니다. 사용 된 [ `AspectFill` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRect.AspectFill/p/SkiaSharp.SKSize/) 방식의 `SKRect` 페이지 가로 세로 비율을 유지 하면서 채우기 위한 사각형을 계산 하 합니다.

**텍스트 경로 효과** 페이지 단일 앰퍼샌드 문자를 1 D 경로 효과 만들기에 대 한 경로를 변환 합니다. 이 경로 영향을 주지 않고 페인트 개체는 같은 문자를 더 큰 버전의 개요를 스트로크 하려면 다음 사용 됩니다.

[![](text-paths-images/textpatheffect-small.png "경로 텍스트 효과 페이지의 삼중 스크린샷")](text-paths-images/textpatheffect-large.png#lightbox "텍스트 경로 효과 페이지의 삼중 스크린샷")

작업의 많은 [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) 클래스 필드와 생성자에 발생 합니다. 두 `SKPaint` 필드는 두 개의 서로 다른 용도로 사용 되 고 정의 된 개체: 첫 번째 (라는 `textPathPaint`)와 앰퍼샌드를 변환 하는 데 사용 되는 `TextSize` 1d 경로 효과 대 한 경로로 50입니다. 두 번째 (`textPaint`)는 해당 경로 영향을 주지 않고 앰퍼샌드의 더 큰 버전을 표시 하는 데 사용 됩니다. 이런 이유로 `Style` 이 두 번째 페인트 개체가으로 설정 되 `Stroke`, 하지만 `StrokeWidth` 1d 경로 효과 사용 하는 경우 해당 속성은 필요 하기 때문에 속성이 설정 되지 않은:

```csharp
public class TextPathEffectPage : ContentPage
{
    const string character = "@";
    const float littleSize = 50;

    SKPathEffect pathEffect;

    SKPaint textPathPaint = new SKPaint
    {
        TextSize = littleSize
    };

    SKPaint textPaint = new SKPaint 
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black
    };

    public TextPathEffectPage()
    {
        Title = "Text Path Effect";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Get the bounds of textPathPaint
        SKRect textPathPaintBounds;
        textPathPaint.MeasureText(character, ref textPathPaintBounds);

        // Create textPath centered around (0, 0)
        SKPath textPath = textPathPaint.GetTextPath(character, 
                                                    -textPathPaintBounds.MidX,
                                                    -textPathPaintBounds.MidY);
        // Create the path effect
        pathEffect = SKPathEffect.Create1DPath(textPath, littleSize, 0,
                                               SKPath1DPathEffectStyle.Translate);
    }
    ...
}
```

생성자는 먼저 사용 하 여는 `textPathPaint` 와 앰퍼샌드를 측정 하는 개체는 `TextSize` 50입니다. 사각형의 중심 좌표 네거티브에 전달 되어는 `GetTextPath` 텍스트를 패스로 변환 하는 메서드. 결과 경로는 (0, 0) 문자를 1 D 경로 효과 대 한 이상적인가의 가운데에 지점입니다.

생각할 수 있습니다는 `SKPathEffect` 생성자의 끝에 개체에 설정할 수는 `PathEffect` 속성 `textPaint` 필드로 저장 하지 않고 합니다. 이 아웃 킨된 not의 결과 왜곡 상태일 때 잘 작동 하지만 `MeasureText` 에서 호출 된 `PaintSurface` 처리기:

```csharp
public class TextPathEffectPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set textPaint TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Do not measure the text with PathEffect set!
        SKRect textBounds;
        textPaint.MeasureText(character, ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Set the PathEffect property and display text
        textPaint.PathEffect = pathEffect;
        canvas.DrawText(character, xText, yText, textPaint);
    }
}
```

`MeasureText` 호출이 센터 페이지에서 문자를 사용 합니다. 문제를 피하기 위해는 `PathEffect` 상태로 표시 되는 텍스트 측정을 후 페인트 개체에 속성이 설정 되어 있습니다.

## <a name="outlines-of-character-outlines"></a>문자 윤곽선의 윤곽선

일반적으로 [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) 방식의 `SKPaint` 페인트 속성을 적용 하 여 하나의 경로를 다른 변환 가장 주목할 만한: 선 너비와 경로 효과입니다. 경로 효과 없이 사용 하면 `GetFillPath` 효율적으로 다른 경로 간략하게 설명 하는 경로 만듭니다. 에 설명 된이 **경로 개요 탭** 페이지에 [ **경로 효과** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) 문서.

호출할 수도 있습니다 `GetFillPath` 에서 반환 된 경로에 `GetTextPath` 되지만 처음에 있습니다 아닐 완전히 있는지 하 모양을 있을 것입니다.

**문자 개요 윤곽선** 페이지에는 기술을 보여 줍니다. 에 모든 관련 코드는 `PaintSurface` 의 처리기는 [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) 클래스.

생성자는 먼저 만듭니다는 `SKPaint` 라는 개체 `textPaint` 와 `TextSize` 속성 페이지의 크기에 따라 합니다. 이 사용 하 여 경로로 변환 되는 `GetTextPath` 메서드. 좌표에 대 한 인수 `GetTextPath` 효과적으로 가운데 맞춤 화면에서 경로:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint())
    {
        // Set Style for the character outlines
        textPaint.Style = SKPaintStyle.Stroke;

        // Set TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Measure the text
        SKRect textBounds;
        textPaint.MeasureText("@", ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Get the path for the character outlines
        using (SKPath textPath = textPaint.GetTextPath("@", xText, yText))
        {
            // Create a new path for the outlines of the path
            using (SKPath outlinePath = new SKPath())
            {
                // Convert the path to the outlines of the stroked path
                textPaint.StrokeWidth = 25;
                textPaint.GetFillPath(textPath, outlinePath);

                // Stroke that new path
                using (SKPaint outlinePaint = new SKPaint())
                {
                    outlinePaint.Style = SKPaintStyle.Stroke;
                    outlinePaint.StrokeWidth = 5;
                    outlinePaint.Color = SKColors.Red;

                    canvas.DrawPath(outlinePath, outlinePaint);
                }
            }
        }
    }
}
```

`PaintSurface` 처리기 라는 새 경로 만듭니다 `outlinePath`합니다. 이 대상 경로에 대 한 호출에서 `GetFillPath`합니다. `StrokeWidth` 속성 25 원인 중 `outlinePath` 텍스트 문자를 따라 그려지는 25 픽셀 너비 경로 간략하게 설명 하기 위해 합니다. 이 경로 다음 빨강 선 너비가 5에에서 표시 됩니다.

[![](text-paths-images/characteroutlineoutlines-small.png "문자 개요 윤곽선 페이지의 삼중 스크린샷")](text-paths-images/characteroutlineoutlines-large.png#lightbox "문자 개요 윤곽선 페이지의 삼중 스크린샷")

치중를 경로 개요 날카로운 모퉁이 사용 하면 여기서 overlaps 나타납니다. 다음은이 프로세스의 일반 아티팩트입니다.

## <a name="text-along-a-path"></a>경로 따라 텍스트

텍스트는 일반적으로 가로 기준선에 표시 됩니다. 세로 또는 대각선 방향으로 실행 하려면 텍스트를 회전할 수 있지만 초기 여전히 직선입니다.

그러나 텍스트는 곡선에 따라 실행 되도록 할 수도 있습니다. 이의 용도 [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) 방식의 `SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

두 번째 인수로 지정 된 경로 함께 실행 하는 첫 번째 인수에 지정 된 텍스트가 이루어집니다. 사용 하 여 경로의 시작 부분부터 오프셋에 텍스트를 시작할 수 있습니다는 `hOffset` 인수입니다. 경로 텍스트의 기준선을 형성 하는 일반적으로: 텍스트 어센더 경로의 한 쪽에 있고 다른 텍스트 디센더와 있는 합니다. 사용 하 여 경로 텍스트 기준선을 오프셋할 수 있지만 `vOffset` 인수입니다.

이 메서드는 설정에 지침을 제공 하는 기능이 없는 `TextSize` 속성의 `SKPaint` 끝에는 경로의 시작 부분에서 실행 되도록 완벽 하 게 크기의 텍스트입니다. 경우에 따라 텍스트 크기에 직접 알아낼 수 있습니다. 다른 시간 해야 경로 측정 함수를 사용 하 여 향후 기사에서 이해 해야 합니다.

**순환 텍스트** 프로그램 원 주위 텍스트를 배치 합니다. 정확 하 게 맞게 텍스트 크기를 조정 쉽게 원의 원주를 결정 하는 것이 쉽습니다. `PaintSurface` 의 처리기는 [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) 클래스는 페이지의 크기에 따라 원의 반지름을 계산 합니다. 해당 원의 되 `circularPath`:

```csharp
public class CircularTextPage : ContentPage
{
    const string text = "xt in a circle that shapes the te";
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circularPath = new SKPath())
        {
            float radius = 0.35f * Math.Min(info.Width, info.Height);
            circularPath.AddCircle(info.Width / 2, info.Height / 2, radius);

            using (SKPaint textPaint = new SKPaint())
            {
                textPaint.TextSize = 100;
                float textWidth = textPaint.MeasureText(text);
                textPaint.TextSize *= 2 * 3.14f * radius / textWidth;

                canvas.DrawTextOnPath(text, circularPath, 0, 0, textPaint);
            }
        }
    }
}
```

`TextSize` 속성 `textPaint` 있는 텍스트의 너비와 원의 원주를 일치 하도록 한 다음 조정 됩니다.

[![](text-paths-images/circulartext-small.png "순환 텍스트 페이지의 삼중 스크린샷")](text-paths-images/circulartext-large.png#lightbox "순환 텍스트 페이지의 삼중 스크린 샷")

텍스트 자체도 다소 순환 되도록 선택 되었습니다: "circle" 단어는 모두 문장에는 제목 및 전치사 구의 개체가 있습니다. 

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
