---
title: 경로 및 SkiaSharp 텍스트
description: 이 문서에서는 SkiaSharp 경로 및 텍스트의 교차 부분을 탐색 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 08/01/2017
ms.openlocfilehash: 3576af56d48eec58f3fe5fee42aef143e2edea70
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615459"
---
# <a name="paths-and-text-in-skiasharp"></a>경로 및 SkiaSharp 텍스트

_경로 및 텍스트의 교차점에 알아보고_

최신 그래픽 시스템 텍스트 글꼴은 정방형 베 지 어 곡선에서 일반적으로 정의 되는 문자 윤곽선의 컬렉션입니다. 따라서 많은 최신 그래픽 시스템을 그래픽 경로로 텍스트 문자를 변환 하는 기능을 포함 합니다.

수의 텍스트 문자 윤곽선을 그리는 뿐만 있습니다 채우려는 이미 살펴보았습니다. 이렇게 하면에 설명 된 대로 특정 스트로크 너비와도 경로 효과 사용 하 여 이러한 문자 윤곽선을 표시 하는 [ **경로 효과** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) 문서. 에 문자열을 변환할 수 이기도 하지만 `SKPath` 개체입니다. 에 설명 된 기법을 사용 하 여 클리핑에 대 한 텍스트 윤곽선을 사용할 수 있다는 것을 의미 합니다 [ **경로 및 지역 클리핑** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md) 문서.

문자 윤곽선을 그리지 경로 효과 사용 하는 것 외에도 경로 문자열을 파생 되는 경로 기반으로 하는 효과 만들 수도 있습니다 하 고 두 결과 통합할 수도 있습니다.

![](text-paths-images/pathsandtextsample.png "텍스트 경로 효과")

에 [이전 문서](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) 살펴보았습니다 방법을 [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) 메서드의 `SKPaint` 그리면된 경로 대 한 개요를 얻을 수 있습니다. 또한 문자 윤곽선에서 파생 된 경로 사용 하 여이 메서드를 사용할 수 있습니다.

이 문서에서는 경로 및 텍스트의 다른 교차를 설명 하는 마지막으로: 합니다 [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) 메서드의 `SKCanvas` 텍스트의 기준선 단추가 곡선된 경로 뒤에 오도록 텍스트 문자열을 표시할 수 있습니다.

## <a name="text-to-path-conversion"></a>경로 변환 하는 텍스트

합니다 [ `GetTextPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetTextPath/p/System.String/System.Single/System.Single/) 메서드의 `SKPaint` 문자열을 변환는 `SKPath` 개체:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

합니다 `x` 및 `y` 인수는 텍스트의 왼쪽 기준의 시작점을 나타내는 것입니다. 같은 역할 여기 에서처럼 합니다 `DrawText` 메서드의 `SKCanvas`합니다. 경로 내에서 텍스트의 왼쪽의 기준선 (x, y) 좌표를 갖게 됩니다.

`GetTextPath` 단순히 하려면 채우기 또는 스트로크 결과 경로 메서드는 과잉 처리입니다. 일반적인 `DrawText` 메서드를 사용 하면 이렇게 수 있습니다. `GetTextPath` 메서드는 경로 관련 된 기타 작업에 더 적합 합니다.

이러한 태스크 중 하나에 맞추는 됩니다. 합니다 **클리핑 텍스트** 페이지 "코드입니다." 라는 단어의 문자 윤곽선을 따라 클리핑 패스를 만듭니다. 이 경로 클립의 이미지가 포함 된 비트맵 페이지의 크기를 채우도록 확장 합니다 **클리핑 텍스트** 소스 코드:

[![](text-paths-images/clippingtext-small.png "클리핑 텍스트 페이지의 3 배가 스크린샷")](text-paths-images/clippingtext-large.png#lightbox "삼중 클리핑 텍스트 페이지 스크린샷")

합니다 [ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs) 클래스 생성자에 포함 된 리소스로 저장 되는 비트맵을 로드 합니다 **미디어** 솔루션의 폴더:

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
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

`PaintSurface` 처리기를 만들어 시작을 `SKPaint` 개체 텍스트에 적합 합니다. `Typeface` 속성이 설정 되어 뿐만 `TextSize`에 있지만이 특정 응용 프로그램을 `TextSize` 속성이 arbirtrary 순수 하 게 합니다. 또한 없습니다 `Style` 설정 합니다.

합니다 `TextSize` 및 `Style` 속성 설정이 필요 하지 않습니다. 때문에이 `SKPaint` 개체에만 사용 되는 `GetTextPath` 텍스트 문자열 "코드"를 사용 하 여 호출 합니다. 처리기는 다음 결과 측정 `SKPath` 개체 및 중심 및 페이지 크기 확장 하는 세 가지 변환을 적용 합니다. 경로는 클리핑 경로로 설정할 수 있습니다.

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

클리핑 패스 설정 되 면 비트맵을 표시 하 고 문자 윤곽선에 클리핑됩니다. 사용 합니다 [ `AspectFill` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRect.AspectFill/p/SkiaSharp.SKSize/) 메서드의 `SKRect` 가로 세로 비율을 유지 하는 동안 페이지를 채우는 사각형을 계산 하는 합니다.

합니다 **텍스트 경로 효과** 페이지 단일 앰퍼샌드 문자 1d 경로 효과 만들 경로를 변환 합니다. 그런 다음이 경로 효과 사용 하 여 그리기 개체를는 같은 문자를 더 큰 버전의 윤곽선을 그리는 데:

[![](text-paths-images/textpatheffect-small.png "텍스트 경로 효과 페이지 스크린샷 삼중")](text-paths-images/textpatheffect-large.png#lightbox "삼중 텍스트 경로 효과 페이지 스크린샷")

작업의 많은 합니다 [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) 클래스 필드와 생성자에 발생 합니다. 두 `SKPaint` 필드는 두 가지 다른 용도로 사용 되 고 정의 된 개체: 첫 번째 (라는 `textPathPaint`) 사용 하 여 앰퍼샌드를 변환 하는 `TextSize` 1d 경로 효과 대 한 경로에 50입니다. 두 번째 (`textPaint`) 해당 경로 효과 사용 하 여 앰퍼샌드의 더 큰 버전을 표시 하는 데 사용 됩니다. 이런 이유로 합니다 `Style` 로 설정 되어이 두 번째 그리기 개체 `Stroke`, 하지만 `StrokeWidth` 1d 경로 효과 사용 하는 경우 해당 속성이 필요 하지 않기 때문에 속성이 설정 되지 않은:

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
        SKRect textPathPaintBounds = new SKRect();
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

생성자를 사용 하 여 먼저 합니다 `textPathPaint` 사용 하 여 앰퍼샌드를 측정 하는 개체를 `TextSize` 50입니다. 해당 영역의 center 좌표의 부정 전달 되는 `GetTextPath` 텍스트를 패스로 변환 하는 방법입니다. 결과 경로는 (0, 0) center 1d 경로 효과에 적합 한 문자를 지정 합니다.

생각 합니다 `SKPathEffect` 개체 생성자의 끝에 설정 될 수는 `PathEffect` 의 속성 `textPaint` 필드로 저장 하지 않고 합니다. 만이 초과 되어 결과 왜곡 하기 때문에 매우 잘 작동 합니다 `MeasureText` 에서 호출 된 `PaintSurface` 처리기:

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
        SKRect textBounds = new SKRect();
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

`MeasureText` 호출이 사용 되는 페이지의 문자를 가운데. 문제를 방지 하려면는 `PathEffect` 속성을 표시 하기 전에 있지만 텍스트 측정 된 후 그리기 개체로 설정 합니다.

## <a name="outlines-of-character-outlines"></a>문자 윤곽선의 윤곽선

일반적으로 [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) 메서드의 `SKPaint` 그리기 속성을 적용 하 여 다른 한 경로 변환 특히: 스트로크 너비와 경로 적용 합니다. 경로 효과 없이 사용할 경우 `GetFillPath` 효율적으로 다른 경로 설명 하는 경로 만듭니다. 에 설명 된이 **경로 윤곽 탭** 페이지에 [ **경로 효과** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) 문서.

호출할 수도 있습니다 `GetFillPath` 에서 반환 되는 경로의 `GetTextPath` 하지만 처음 있습니다 완전히 확신할 수는 다음과 같은 모습입니다.

합니다 **문자 윤곽선 윤곽선** 페이지 기술을 보여 줍니다. 모든 관련 코드에는 `PaintSurface` 처리기는 [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) 클래스입니다.

생성자를 만들어 시작를 `SKPaint` 개체인 `textPaint` 사용 하 여를 `TextSize` 속성 페이지의 크기에 따라 합니다. 사용 하 여 경로를 변환 합니다 `GetTextPath` 메서드. 에 대 한 좌표 인수 `GetTextPath` 화면에서 경로 효과적으로 센터:

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
        SKRect textBounds = new SKRect();
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

합니다 `PaintSurface` 처리기는 다음 명명 된 새 경로 만듭니다 `outlinePath`합니다. 대상 경로에 대 한 호출에서 이것이 `GetFillPath`합니다. `StrokeWidth` 25 원인의 속성 `outlinePath` 25 픽셀 너비 경로 따라 텍스트 문자를 간략하게 설명 합니다. 이 경로 빨간색 선 너비가 5 사용 하 여 다음 표시 됩니다.

[![](text-paths-images/characteroutlineoutlines-small.png "문자 윤곽선 개요 페이지 스크린샷 삼중")](text-paths-images/characteroutlineoutlines-large.png#lightbox "삼중 문자 윤곽선 개요 페이지 스크린샷")

밀접 하 게 찾아서 경로 윤곽선 날카로운 모퉁이 사용 하면 여기서 겹침 여부가 표시 됩니다. 다음은이 프로세스의 일반 아티팩트입니다.

## <a name="text-along-a-path"></a>경로 상에 텍스트

텍스트는 일반적으로 가로 기준선에 표시 됩니다. 세로 또는 대각선으로 실행 하는 텍스트를 회전할 수 있습니다 하지만 기준선은 직선을 계속 합니다.

그러나 텍스트 곡선을 따라 실행 되도록 할 수도 있습니다. 용도이 [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) 메서드의 `SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

첫 번째 인수에 지정 된 텍스트를 두 번째 인수로 지정 된 경로 함께 실행 하도록 합니다. 사용 하 여 경로의 시작 부분부터 오프셋에 텍스트를 시작할 수 있습니다는 `hOffset` 인수입니다. 경로 텍스트의 기준선을 형성 하는 일반적으로: 텍스트 어센더는 경로의 한 쪽에 있고 다른 텍스트 디센더와 있습니다. 사용 하 여 경로에서 텍스트 기준선 오프셋 수 있지만 `vOffset` 인수입니다.

이 메서드는 설정에 지침을 제공 하는 `TextSize` 속성의 `SKPaint` 경로의 처음부터 끝까지 실행을 완벽 하 게 크기의 텍스트를 합니다. 경우에 따라 직접 텍스트 크기를 확인할 수 있습니다. 다른 경로 측정 함수는 이후 기사에서 설명할 수를 사용 해야 합니다.

합니다 **순환 텍스트** 프로그램 텍스트를 원 주위에 배치 합니다. 정확히 맞게 텍스트 크기를 쉽게 원의 원주를 결정 하는 것이 쉽습니다. 합니다 `PaintSurface` 처리기는 [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) 클래스는 페이지의 크기를 기준으로 원의 반지름을 계산 합니다. 해당 원 되 `circularPath`:

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

합니다 `TextSize` 속성의 `textPaint` 텍스트 너비 원의 원주와 일치 하도록 조정 합니다.

[![](text-paths-images/circulartext-small.png "순환 텍스트 페이지의 3 배가 스크린샷")](text-paths-images/circulartext-large.png#lightbox "삼중 순환 텍스트 페이지 스크린샷")

텍스트 자체도 어느 정도 순환 되도록 선택 되었습니다: "circle" 이라는 단어는 모두 문장의 주체 및 개체 전치사 구입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
