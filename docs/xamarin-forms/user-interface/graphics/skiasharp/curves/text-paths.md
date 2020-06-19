---
title: SkiaSharp의 경로 및 텍스트
description: 이 문서에서는 SkiaSharp 경로와 텍스트의 교집합을 알아보고 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 08/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b0cbb7d26a2aea02a3255fc75947c20a3d803b86
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84131900"
---
# <a name="paths-and-text-in-skiasharp"></a>SkiaSharp의 경로 및 텍스트

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_패스와 텍스트의 교집합 살펴보기_

최신 그래픽 시스템에서 텍스트 글꼴은 일반적으로 정방형 3 차원 곡선으로 정의 되는 문자 윤곽선 모음입니다. 따라서 대부분의 최신 그래픽 시스템에는 텍스트 문자를 그래픽 패스로 변환 하는 기능이 포함 되어 있습니다.

텍스트 문자의 윤곽선을 스트로크 하 고 채울 수 있는 것을 이미 살펴보았습니다. 이렇게 하면 [**경로 효과**](effects.md) 문서에 설명 된 대로 특정 스트로크 너비로 이러한 문자 윤곽선을 표시 하 고 경로 효과를 표시할 수 있습니다. 하지만 문자열을 개체로 변환할 수도 있습니다 `SKPath` . 즉, [**경로 및 지역으로 클리핑**](clipping.md) 문서에 설명 된 기술을 사용 하 여 텍스트 윤곽선을 클리핑에 사용할 수 있습니다.

문자 윤곽선을 스트로크 하는 데 경로 효과를 사용 하는 것 외에도 문자열에서 파생 된 경로를 기반으로 하는 경로 효과를 만들 수 있으며, 두 가지 효과를 결합할 수도 있습니다.

![](text-paths-images/pathsandtextsample.png "Text Path Effect")

[**경로 효과**](effects.md)에 대 한 이전 문서에서는 [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single)) 의 메서드가 `SKPaint` 스트로크 된 패스의 개요를 가져오는 방법을 살펴보았습니다. 문자 윤곽선에서 파생 된 경로를 사용 하 여이 메서드를 사용할 수도 있습니다.

마지막으로,이 문서에서는 경로와 텍스트의 또 다른 교집합을 보여 줍니다. [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) 의 메서드를 사용 하면 텍스트 `SKCanvas` 의 기준선이 곡선 경로를 따라가는 텍스트 문자열을 표시할 수 있습니다.

## <a name="text-to-path-conversion"></a>텍스트를 패스로 변환

[`GetTextPath`](xref:SkiaSharp.SKPaint.GetTextPath(System.String,System.Single,System.Single))의 메서드는 `SKPaint` 문자열을 개체로 변환 합니다 `SKPath` .

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

`x`및 `y` 인수는 텍스트 왼쪽의 기준선 시작 지점을 표시 합니다. 이러한 메서드는의 메서드에서와 동일한 역할을 수행 `DrawText` `SKCanvas` 합니다. 경로 내에서 텍스트 왼쪽의 기준선은 좌표 (x, y)를 갖습니다.

`GetTextPath`결과 경로를 채우거 나 스트로크 하려는 경우에는이 메서드를 사용할 필요가 없습니다. 일반적인 `DrawText` 방법으로이 작업을 수행할 수 있습니다. 메서드는 경로를 포함 하는 `GetTextPath` 다른 작업에 더 유용 합니다.

이러한 작업 중 하나를 클리핑 합니다. **텍스트 클리핑** 페이지는 "코드" 라는 단어의 문자 윤곽선을 기반으로 하는 클리핑 경로를 만듭니다. 이 경로는 **클리핑 텍스트** 소스 코드의 이미지를 포함 하는 비트맵을 자르는 페이지 크기에 맞게 늘어납니다.

[![](text-paths-images/clippingtext-small.png "Triple screenshot of the Clipping Text page")](text-paths-images/clippingtext-large.png#lightbox "Triple screenshot of the Clipping Text page")

[`ClippingTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs)클래스 생성자는 솔루션의 **미디어** 폴더에 포함 리소스로 저장 된 비트맵을 로드 합니다.

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

`PaintSurface`처리기는 텍스트에 적합 한 개체를 만들기 시작 합니다 `SKPaint` . `Typeface` `TextSize` 이 특정 응용 프로그램의 경우 속성은 전적으로 속성 이지만 속성이로 설정 됩니다 `TextSize` . 또한 `Style` 설정이 없습니다.

`TextSize` `Style` 이 `SKPaint` 개체는 `GetTextPath` 텍스트 문자열 "CODE"를 사용 하는 호출에만 사용 되므로 및 속성 설정은 필요 하지 않습니다. 그런 다음 처리기는 결과 개체를 측정 하 `SKPath` 고 세 가지 변환을 적용 하 여 가운데 맞춤 하 고 페이지 크기에 맞게 조정 합니다. 그런 다음 경로를 클리핑 경로로 설정할 수 있습니다.

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

클리핑 경로를 설정 하면 비트맵을 표시할 수 있으며 문자 윤곽선으로 잘립니다. 의 메서드를 사용 하 여 [`AspectFill`](xref:SkiaSharp.SKRect.AspectFill(SkiaSharp.SKSize)) `SKRect` 가로 세로 비율을 유지 하면서 페이지를 채우도록 사각형을 계산 합니다.

**텍스트 경로 효과** 페이지는 단일 앰퍼샌드 문자를 패스로 변환 하 여 1d 경로 효과를 만듭니다. 그런 다음이 경로 효과가 있는 그리기 개체를 사용 하 여 같은 문자를 포함 하는 더 큰 버전의 윤곽선을 그립니다.

[![](text-paths-images/textpatheffect-small.png "Triple screenshot of the Text Path Effect page")](text-paths-images/textpatheffect-large.png#lightbox "Triple screenshot of the Text Path Effect page")

클래스에서 대부분의 작업은 [`TextPathEffectPath`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) 필드 및 생성자에서 발생 합니다. `SKPaint`필드로 정의 된 두 개체는 두 가지 용도로 사용 됩니다. 첫 번째 (명명 `textPathPaint` 됨)는 앰퍼샌드를 50의로 변환 하는 데 사용 됩니다 `TextSize` . 두 번째 ( `textPaint` )는 해당 경로 효과를 사용 하 여 더 큰 버전의 앰퍼샌드를 표시 하는 데 사용 됩니다. 이러한 이유 때문에 `Style` 이 두 번째 paint 개체의는로 설정 `Stroke` 되지만 속성은 `StrokeWidth` 1d 경로 효과를 사용 하는 경우에는 필요 하지 않기 때문에 설정 되지 않습니다.

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

생성자는 먼저 개체를 사용 하 여 `textPathPaint` 가 50 인 앰퍼샌드를 측정 합니다 `TextSize` . 그런 다음 해당 사각형의 중심 좌표를 메서드에 전달 하 여 `GetTextPath` 텍스트를 패스로 변환 합니다. 결과 경로의 문자 가운데에는 (0, 0) 점이 있습니다 .이 점이 1D 경로 효과에 적합 합니다.

`SKPathEffect`생성자의 끝에서 만든 개체를 `PathEffect` 필드로 저장 하는 대신의 속성으로 설정할 수 있습니다 `textPaint` . 그러나이는 처리기에서 호출 결과를 왜곡 하기 때문에 매우 잘 작동 하지 않습니다 `MeasureText` `PaintSurface` .

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

이 `MeasureText` 호출을 사용 하 여 페이지의 문자를 가운데에 맞춥니다. 문제를 방지 하기 위해 `PathEffect` 텍스트가 측정 된 후 표시 되기 전에 속성은 paint 개체로 설정 됩니다.

## <a name="outlines-of-character-outlines"></a>문자 윤곽선의 윤곽선

일반적으로 [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single)) 의 메서드는 `SKPaint` 페인트 속성, 특히 스트로크 너비 및 경로 효과를 적용 하 여 한 경로를 다른 경로로 변환 합니다. 경로 효과 없이 사용 하는 경우는 `GetFillPath` 다른 경로를 윤곽선으로 만드는 경로를 효과적으로 만듭니다. 이는 [**경로 효과**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) 문서의 **경로를 간략하게 설명 하는 탭** 에 설명 되어 있습니다.

`GetFillPath`에서 반환 되는 경로에 대해를 호출할 수도 `GetTextPath` 있지만, 처음에는 어떤 것이 든 완전히 확실 하지 않을 수 있습니다.

**문자 개요 개요** 페이지에서는이 기술을 보여 줍니다. 모든 관련 코드는 `PaintSurface` 클래스의 처리기에 [`CharacterOutlineOutlinesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) 있습니다.

생성자는 `SKPaint` `textPaint` 페이지의 크기를 기반으로 속성을 사용 하 여 라는 개체를 만들기 시작 `TextSize` 합니다. 이는 메서드를 사용 하 여 경로로 변환 됩니다 `GetTextPath` . `GetTextPath`화면에서 경로를 효과적으로 가운데 맞춤 하기 위한 좌표 인수:

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

`PaintSurface`그런 다음 처리기는 이라는 새 경로를 만듭니다 `outlinePath` . 이는에 대 한 호출에서 대상 경로가 됩니다 `GetFillPath` . `StrokeWidth`25의 속성은에서 `outlinePath` 텍스트 문자를 스트로크 하는 25 픽셀의 경로에 대 한 개요를 설명 합니다. 그런 다음이 경로는 빨간색으로 표시 되 고 스트로크 너비는 5로 표시 됩니다.

[![](text-paths-images/characteroutlineoutlines-small.png "Triple screenshot of the Character Outline Outlines page")](text-paths-images/characteroutlineoutlines-large.png#lightbox "Triple screenshot of the Character Outline Outlines page")

자세히 살펴보면 경로 개요에서 뾰족한 모퉁이를 만드는 겹치는 부분을 볼 수 있습니다. 이러한 작업은이 프로세스의 일반적인 아티팩트입니다.

## <a name="text-along-a-path"></a>경로를 따라 텍스트

일반적으로 텍스트는 가로 기준선에 표시 됩니다. 세로 또는 대각선 방향으로 실행 되도록 텍스트를 회전할 수 있지만 기준선은 여전히 직선입니다.

그러나 곡선을 따라 텍스트를 실행 하려는 경우가 있습니다. 이는 메서드의 목적입니다 [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) `SKCanvas` .

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

첫 번째 인수에 지정 된 텍스트는 두 번째 인수로 지정 된 경로를 따라 실행 되도록 생성 됩니다. 인수를 사용 하 여 경로의 시작 부분부터 오프셋에 있는 텍스트를 시작할 수 있습니다 `hOffset` . 일반적으로 경로는 텍스트의 기준선을 구성 합니다. 텍스트 어센더는 경로의 한 쪽에 있고 텍스트 디센더는 다른 위치에 있습니다. 그러나 경로에서 인수를 사용 하 여 텍스트 기준선을 오프셋할 수 있습니다 `vOffset` .

이 메서드에는의 속성을 설정 하는 방법에 대 한 지침을 제공 하 여 `TextSize` 경로 시작부터 끝까지 텍스트 크기를 완벽 하 게 지정할 수 있는 기능이 없습니다 `SKPaint` . 경우에 따라 해당 텍스트 크기를 확인할 수 있습니다. 경로 [**정보 및 열거형**](information.md)에 대 한 다음 문서에 설명 된 경로 측정 함수를 사용 해야 하는 경우도 있습니다.

**원형 텍스트** 프로그램은 원 주위에 텍스트를 래핑합니다. 원의 원주를 결정 하는 것은 간단 하므로 정확히 맞도록 텍스트의 크기를 조정 하는 것이 쉽습니다. `PaintSurface`클래스의 처리기는 [`CircularTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) 페이지 크기에 따라 원의 반경을 계산 합니다. 해당 원은 `circularPath` 다음과 같이 됩니다.

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

`TextSize` `textPaint` 그런 다음 텍스트 너비가 원의 원주와 일치 하도록의 속성을 조정 합니다.

[![](text-paths-images/circulartext-small.png "Triple screenshot of the Circular Text page")](text-paths-images/circulartext-large.png#lightbox "Triple screenshot of the Circular Text page")

텍스트 자체는 약간 원형으로도 선택 되었습니다. "circle" 이라는 단어는 모두 문장의 제목과 prepositional 구의 개체입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
