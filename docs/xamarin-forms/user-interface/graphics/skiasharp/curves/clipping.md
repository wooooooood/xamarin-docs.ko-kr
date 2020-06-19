---
title: 경로 및 지역 클리핑
description: 이 문서에서는 SkiaSharp 경로를 사용 하 여 그래픽을 특정 영역에 클리핑 하 고 영역을 만드는 방법 및 샘플 코드를 사용 하 여이를 보여 주는 방법을 설명 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a4bb6c30ada13691146d00d2094df8f13ca453b9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140259"
---
# <a name="clipping-with-paths-and-regions"></a>경로 및 지역 클리핑

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_경로를 사용 하 여 특정 영역에 그래픽 클리핑 및 영역 만들기_

그래픽의 렌더링을 특정 영역으로 제한 해야 하는 경우도 있습니다. 이를 *클리핑*이라고 합니다. 키 구멍을 통해 표시 되는 원숭이의 이미지와 같은 특수 효과에 대해 클리핑을 사용할 수 있습니다.

![Keyhole를 통한 원숭이](clipping-images/clippingsample.png)

*클리핑 영역은* 그래픽이 렌더링 되는 화면 영역입니다. 클리핑 영역 외부에 표시 되는 모든 항목은 렌더링 되지 않습니다. 클리핑 영역은 일반적으로 사각형 또는 [`SKPath`](xref:SkiaSharp.SKPath) 개체로 정의 되지만 개체를 사용 하 여 클리핑 영역을 정의할 수도 있습니다 [`SKRegion`](xref:SkiaSharp.SKRegion) . 경로에서 영역을 만들 수 있기 때문에 처음에는 이러한 두 가지 유형의 개체가 관련 된 것으로 보입니다. 그러나 지역에서 경로를 만들 수는 없으며 내부적으로는 서로 다른 위치에 있습니다. 경로는 일련의 선과 곡선으로 구성 되 고, 영역은 일련의 수평 스캔 선으로 정의 됩니다.

위의 이미지는 Keyhole 페이지를 **통해 원숭이** 에서 만들었습니다. [`MonkeyThroughKeyholePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs)클래스는 SVG 데이터를 사용 하 여 경로를 정의 하 고 생성자를 사용 하 여 프로그램 리소스에서 비트맵을 로드 합니다.

```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    SKBitmap bitmap;
    SKPath keyholePath = SKPath.ParseSvgPathData(
        "M 300 130 L 250 350 L 450 350 L 400 130 A 70 70 0 1 0 300 130 Z");

    public MonkeyThroughKeyholePage()
    {
        Title = "Monkey through Keyhole";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

개체는 `keyholePath` keyhole의 개요를 설명 하지만, 좌표는 완전히 임의로 제공 되며 경로 데이터를 고안 했을 때 편리한 것을 반영 합니다. 이러한 이유로 `PaintSurface` 처리기는이 경로의 범위를 가져오고를 호출 `Translate` 하 여 경로를 `Scale` 화면 가운데로 이동 하 고 화면 높이를 거의 같게 만듭니다.

```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set transform to center and enlarge clip path to window height
        SKRect bounds;
        keyholePath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.98f * info.Height / bounds.Height);
        canvas.Translate(-bounds.MidX, -bounds.MidY);

        // Set the clip path
        canvas.ClipPath(keyholePath);

        // Reset transforms
        canvas.ResetMatrix();

        // Display monkey to fill height of window but maintain aspect ratio
        canvas.DrawBitmap(bitmap,
            new SKRect((info.Width - info.Height) / 2, 0,
                       (info.Width + info.Height) / 2, info.Height));
    }
}
```

그러나 경로는 렌더링 되지 않습니다. 대신, 변환을 수행 하는 동안 경로를 사용 하 여이 문으로 클리핑 영역을 설정 합니다.

```csharp
canvas.ClipPath(keyholePath);
```

`PaintSurface`그런 다음 처리기는에 대 한 호출을 사용 하 여 변환을 다시 설정 하 `ResetMatrix` 고 화면 전체 높이로 확장할 비트맵을 그립니다. 이 코드에서는 비트맵이 사각형 이며이 특정 비트맵이 인 것으로 가정 합니다. 비트맵은 클리핑 경로에서 정의한 영역 내 에서만 렌더링 됩니다.

[![Keyhole 페이지를 통한 원숭이의 세 번째 스크린샷](clipping-images/monkeythroughkeyhole-small.png)](clipping-images/monkeythroughkeyhole-large.png#lightbox)

클리핑 경로는 `ClipPath` 그래픽 개체 (예: 비트맵)가 표시 되는 경우에 적용 되는 변환이 아니라 메서드를 호출할 때 적용 되는 변환에 적용 됩니다. 클리핑 경로는 메서드와 함께 저장 되 고 메서드로 복원 되는 캔버스 상태의 일부입니다 `Save` `Restore` .

## <a name="combining-clipping-paths"></a>클리핑 패스 결합

정확 하 게 말하면 클리핑 영역은 메서드로 "설정" 되지 않습니다 `ClipPath` . 대신 캔버스의 크기와 같은 사각형으로 시작 하는 기존 클리핑 경로와 결합 됩니다. 속성 또는 속성을 사용 하 여 클리핑 영역의 사각형 범위를 가져올 수 있습니다 [`ClipBounds`](xref:SkiaSharp.SKCanvas.ClipBounds) [`ClipDeviceBounds`](xref:SkiaSharp.SKCanvas.ClipDeviceBounds) . `ClipBounds`속성은 `SKRect` 적용 될 수 있는 변환을 반영 하는 값을 반환 합니다. `ClipDeviceBounds`속성은 값을 반환 합니다 `RectI` . 이는 정수 차원이 있는 사각형 이며 실제 픽셀 차원의 클리핑 영역을 설명 합니다.

를 호출 `ClipPath` 하면 클리핑 영역을 새 영역과 결합 하 여 클리핑 영역을 줄일 수 있습니다. 메서드의 전체 구문은 [`ClipPath`](xref:SkiaSharp.SKCanvas.ClipPath(SkiaSharp.SKPath,SkiaSharp.SKClipOperation,System.Boolean)) 다음과 같습니다.

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

또한 [`ClipRect`](xref:SkiaSharp.SKCanvas.ClipRect(SkiaSharp.SKRect,SkiaSharp.SKClipOperation,System.Boolean)) 클리핑 영역을 사각형으로 결합 하는 메서드도 있습니다.

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

기본적으로 결과 클리핑 영역은 기존 클리핑 영역과 `SKPath` `SKRect` 또는 메서드에 지정 된 또는의 교집합입니다 `ClipPath` `ClipRect` . 이는 **네 개의 원형 교차 클립** 페이지에서 보여 줍니다. `PaintSurface`클래스의 처리기는 [`FourCircleInteresectClipPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) 동일한 개체를 재사용 `SKPath` 하 여 네 개의 겹치는 원을 만듭니다. 각각은에 대 한 연속 호출을 통해 클리핑 영역을 줄입니다 `ClipPath` .

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float size = Math.Min(info.Width, info.Height);
    float radius = 0.4f * size;
    float offset = size / 2 - radius;

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    using (SKPath path = new SKPath())
    {
        path.AddCircle(-offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(-offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColors.Blue;
            canvas.DrawPaint(paint);
        }
    }
}
```

왼쪽은 다음과 같은 네 원의 교집합입니다.

[![네 개의 원형 교차 클립 페이지의 삼중 스크린샷](clipping-images//fourcircleintersectclip-small.png)](clipping-images/fourcircleintersectclip-large.png#lightbox)

열거형에는 [`SKClipOperation`](xref:SkiaSharp.SKClipOperation) 두 개의 멤버만 있습니다.

- `Difference`기존 클리핑 영역에서 지정 된 경로 또는 사각형을 제거 합니다.

- `Intersect`지정 된 경로 또는 사각형과 기존 클리핑 영역을 교차 시킵니다.

`SKClipOperation.Intersect`클래스의 네 인수를 `FourCircleIntersectClipPage` 로 바꾸면 `SKClipOperation.Difference` 다음과 같이 표시 됩니다.

[![차이 작업을 포함 하는 네 개의 원 교차 클립 페이지의 3 차 스크린샷](clipping-images//fourcircledifferenceclip-small.png)](clipping-images/fourcircledifferenceclip-large.png#lightbox)

네 개의 겹치는 원이 클리핑 영역에서 제거 되었습니다.

**클립 작업** 페이지는 이러한 두 작업 간의 차이점을 보여 줍니다. 왼쪽의 첫 번째 원은의 기본 클립 작업을 사용 하 여 클리핑 영역에 추가 되 고 `Intersect` 오른쪽에 있는 두 번째 원은 텍스트 레이블로 표시 된 클립 작업을 사용 하 여 클리핑 영역에 추가 됩니다.

[![클립 작업 페이지의 세 번째 스크린샷](clipping-images//clipoperations-small.png)](clipping-images/clipoperations-large.png#lightbox)

[`ClipOperationsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs)클래스는 두 개의 `SKPaint` 개체를 필드로 정의한 다음 화면을 두 개의 사각형 영역으로 나눕니다. 이러한 영역은 휴대폰의 세로 모드 또는 가로 모드 여부에 따라 달라 집니다. `DisplayClipOp`그런 다음 클래스는 `ClipPath` 두 개의 원 경로를 사용 하 여 각 클리핑 작업을 보여 주는 텍스트와 호출을 표시 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;

    foreach (SKClipOperation clipOp in Enum.GetValues(typeof(SKClipOperation)))
    {
        // Portrait mode
        if (info.Height > info.Width)
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width, y + info.Height / 2), clipOp);
            y += info.Height / 2;
        }
        // Landscape mode
        else
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width / 2, y + info.Height), clipOp);
            x += info.Width / 2;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKClipOperation clipOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(clipOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    canvas.Save();

    using (SKPath path1 = new SKPath())
    {
        path1.AddCircle(xCenter - radius / 2, yCenter, radius);
        canvas.ClipPath(path1);

        using (SKPath path2 = new SKPath())
        {
            path2.AddCircle(xCenter + radius / 2, yCenter, radius);
            canvas.ClipPath(path2, clipOp);

            canvas.DrawPaint(fillPaint);
        }
    }

    canvas.Restore();
}
```

`DrawPaint`일반적으로를 호출 하면 전체 canvas가 해당 개체와 채워집니다 `SKPaint` . 그러나이 경우 메서드는 클리핑 영역 내 에서만 그립니다.

## <a name="exploring-regions"></a>영역 탐색

개체를 기준으로 클리핑 영역을 정의할 수도 있습니다 [`SKRegion`](xref:SkiaSharp.SKRegion) .

새로 만든 `SKRegion` 개체는 빈 영역을 설명 합니다. 일반적으로 개체에 대 한 첫 번째 호출은 [`SetRect`](xref:SkiaSharp.SKRegion.SetRect(SkiaSharp.SKRectI)) 지역이 사각형 영역을 설명 하기 위한 것입니다. 에 대 한 매개 변수는 `SetRect` `SKRectI` &mdash; 사각형을 픽셀 단위로 지정 하므로 정수 좌표를 포함 하는 사각형의 값입니다. 그런 다음 [`SetPath`](xref:SkiaSharp.SKRegion.SetPath(SkiaSharp.SKPath,SkiaSharp.SKRegion)) 개체를 사용 하 여를 호출할 수 있습니다 `SKPath` . 그러면 경로의 내부와 동일 하지만 초기 사각형 영역으로 잘린 지역이 생성 됩니다.

메서드 오버 로드 중 하나 (예:)를 호출 하 여 지역을 수정할 수도 있습니다 [`Op`](xref:SkiaSharp.SKRegion.Op*) .

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

[`SKRegionOperation`](xref:SkiaSharp.SKRegionOperation)열거형은와 비슷하지만 `SKClipOperation` 더 많은 멤버가 있습니다.

- `Difference`

- `Intersect`

- `Union`

- `XOR`

- `ReverseDifference`

- `Replace`

호출을 수행 하는 지역은 `Op` 멤버를 기반으로 하는 매개 변수로 지정 된 지역과 결합 됩니다 `SKRegionOperation` . 마지막으로 클리핑에 적합 한 영역을 가져오는 경우의 메서드를 사용 하 여 캔버스의 클리핑 영역으로 설정할 수 있습니다 [`ClipRegion`](xref:SkiaSharp.SKCanvas.ClipRegion(SkiaSharp.SKRegion,SkiaSharp.SKClipOperation)) `SKCanvas` .

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

다음 스크린샷은 6 개 지역 작업을 기반으로 하는 클리핑 영역을 보여 줍니다. 왼쪽 원은 메서드가 호출 되는 지역 이며 `Op` , 오른쪽 원은 메서드로 전달 되는 영역입니다 `Op` .

[![영역 작업 페이지의 세 번째 스크린샷](clipping-images//regionoperations-small.png)](clipping-images/regionoperations-large.png#lightbox)

이러한 두 원을 결합할 수 있는 가능성은 모두 있나요? 결과 이미지를 세 가지 구성 요소의 조합으로 간주 합니다. 이러한 구성 요소는 `Difference` , 및 작업에 표시 됩니다 `Intersect` `ReverseDifference` . 총 조합 수는 세 번째 거듭제곱 또는 8입니다. 누락 된 두 가지는 원래 지역 (를 호출 하지 않는 결과 `Op` ) 및 완전히 비어 있는 영역입니다.

먼저 경로를 만든 다음 해당 경로에서 영역을 만든 다음 여러 영역을 결합 해야 하므로 클리핑에 영역을 사용 하는 것이 더 어렵습니다. **영역 작업** 페이지의 전체 구조는 **클립 작업과** 매우 유사 하지만 클래스는 화면을 [`RegionOperationsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) 6 개 영역으로 나누고이 작업에 대 한 영역을 사용 하는 데 필요한 추가 작업을 보여 줍니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;
    float width = info.Height > info.Width ? info.Width / 2 : info.Width / 3;
    float height = info.Height > info.Width ? info.Height / 3 : info.Height / 2;

    foreach (SKRegionOperation regionOp in Enum.GetValues(typeof(SKRegionOperation)))
    {
        DisplayClipOp(canvas, new SKRect(x, y, x + width, y + height), regionOp);

        if ((x += width) >= info.Width)
        {
            x = 0;
            y += height;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKRegionOperation regionOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(regionOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    SKRectI recti = new SKRectI((int)rect.Left, (int)rect.Top,
                                (int)rect.Right, (int)rect.Bottom);

    using (SKRegion wholeRectRegion = new SKRegion())
    {
        wholeRectRegion.SetRect(recti);

        using (SKRegion region1 = new SKRegion(wholeRectRegion))
        using (SKRegion region2 = new SKRegion(wholeRectRegion))
        {
            using (SKPath path1 = new SKPath())
            {
                path1.AddCircle(xCenter - radius / 2, yCenter, radius);
                region1.SetPath(path1);
            }

            using (SKPath path2 = new SKPath())
            {
                path2.AddCircle(xCenter + radius / 2, yCenter, radius);
                region2.SetPath(path2);
            }

            region1.Op(region2, regionOp);

            canvas.Save();
            canvas.ClipRegion(region1);
            canvas.DrawPaint(fillPaint);
            canvas.Restore();
        }
    }
}
```

메서드와 메서드 간의 큰 차이는 `ClipPath` `ClipRegion` 다음과 같습니다.

> [!IMPORTANT]
> 메서드와 달리 `ClipPath` `ClipRegion` 메서드는 변환의 영향을 받지 않습니다.

이러한 차이에 대 한 근거를 이해 하려면 지역이 무엇 인지 이해 하는 것이 좋습니다. 클리핑 작업 또는 지역 작업을 내부적으로 구현 하는 방법에 대해 살펴본 경우 매우 복잡할 수 있습니다. 잠재적으로 매우 복잡 한 여러 경로를 결합 하 고 결과 경로의 개요는 알고리즘에 대 한 개요입니다.

이 작업은 각 경로를 이전에 만든 것과 같은 일련의 수평 스캔 선으로 축소 하는 경우 상당히 간단해 집니다. 각 검사 줄은 단순히 시작점과 끝점이 있는 가로 선입니다. 예를 들어 반지름이 10 픽셀인 원은 각각 원의 왼쪽 부분에서 시작 하 여 오른쪽 부분에서 끝나는 20 개의 가로 스캔 선으로 분해할 수 있습니다. 두 원을 모든 지역 작업과 결합 하는 것은 해당 하는 스캐닝선의 각 쌍에 대 한 시작 및 끝 좌표를 검토 하는 것이 간단 하기 때문에 매우 간단 합니다.

지역이 며 영역을 정의 하는 일련의 수평 스캔 선입니다.

그러나 영역이 일련의 스캐닝선으로 줄어들면 이러한 검사 줄은 특정 픽셀 차원을 기반으로 합니다. 엄격 하 게 말하면 지역은 벡터 그래픽 개체가 아닙니다. 압축 된 단색 비트맵과 경로에 대 한 특성의 성격은 더 가깝습니다. 따라서 충돌을 잃지 않고 영역을 크기 조정 하거나 회전할 수 없으므로 클리핑 영역에 사용할 경우에는 변환 되지 않습니다.

그러나 그리기를 위해 지역에 변환을 적용할 수 있습니다. **지역 페인트** 프로그램 생생하게 알려주고 자 지역의 내부 특성을 보여 줍니다. [`RegionPaintPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs)클래스는 `SKRegion` `SKPath` 10 단위 반지름 원의를 기준으로 개체를 만듭니다. 그런 다음 변환은 해당 원을 확장 하 여 페이지를 채웁니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int radius = 10;

    // Create circular path
    using (SKPath circlePath = new SKPath())
    {
        circlePath.AddCircle(0, 0, radius);

        // Create circular region
        using (SKRegion circleRegion = new SKRegion())
        {
            circleRegion.SetRect(new SKRectI(-radius, -radius, radius, radius));
            circleRegion.SetPath(circlePath);

            // Set transform to move it to center and scale up
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(Math.Min(info.Width / 2, info.Height / 2) / radius);

            // Fill region
            using (SKPaint fillPaint = new SKPaint())
            {
                fillPaint.Style = SKPaintStyle.Fill;
                fillPaint.Color = SKColors.Orange;

                canvas.DrawRegion(circleRegion, fillPaint);
            }

            // Stroke path for comparison
            using (SKPaint strokePaint = new SKPaint())
            {
                strokePaint.Style = SKPaintStyle.Stroke;
                strokePaint.Color = SKColors.Blue;
                strokePaint.StrokeWidth = 0.1f;

                canvas.DrawPath(circlePath, strokePaint);
            }
        }
    }
}
```

`DrawRegion`호출은 영역을 주황색으로 채우고 `DrawPath` 통화는 비교를 위해 파란색의 원래 경로를 입력 합니다.

[![영역 페인트 페이지의 삼중 스크린샷](clipping-images//regionpaint-small.png)](clipping-images/regionpaint-large.png#lightbox)

영역은 명확 하 게 일련의 불연속 좌표입니다.

클리핑 영역에 대 한 연결에서 변환을 사용할 필요가 없는 경우 **네 리프 클로버** 페이지가 보여 주는 것 처럼 클리핑에 영역을 사용할 수 있습니다. [`FourLeafCloverPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs)클래스는 네 개의 원형 영역에서 복합 영역을 생성 하 고, 해당 복합 지역을 클리핑 영역으로 설정한 다음, 페이지의 중앙에서 emanating 일련의 360 직선을 그립니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float xCenter = info.Width / 2;
    float yCenter = info.Height / 2;
    float radius = 0.24f * Math.Min(info.Width, info.Height);

    using (SKRegion wholeScreenRegion = new SKRegion())
    {
        wholeScreenRegion.SetRect(new SKRectI(0, 0, info.Width, info.Height));

        using (SKRegion leftRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion rightRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion topRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion bottomRegion = new SKRegion(wholeScreenRegion))
        {
            using (SKPath circlePath = new SKPath())
            {
                // Make basic circle path
                circlePath.AddCircle(xCenter, yCenter, radius);

                // Left leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, 0));
                leftRegion.SetPath(circlePath);

                // Right leaf
                circlePath.Transform(SKMatrix.MakeTranslation(2 * radius, 0));
                rightRegion.SetPath(circlePath);

                // Make union of right with left
                leftRegion.Op(rightRegion, SKRegionOperation.Union);

                // Top leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, -radius));
                topRegion.SetPath(circlePath);

                // Combine with bottom leaf
                circlePath.Transform(SKMatrix.MakeTranslation(0, 2 * radius));
                bottomRegion.SetPath(circlePath);

                // Make union of top with bottom
                bottomRegion.Op(topRegion, SKRegionOperation.Union);

                // Exclusive-OR left and right with top and bottom
                leftRegion.Op(bottomRegion, SKRegionOperation.XOR);

                // Set that as clip region
                canvas.ClipRegion(leftRegion);

                // Set transform for drawing lines from center
                canvas.Translate(xCenter, yCenter);

                // Draw 360 lines
                for (double angle = 0; angle < 360; angle++)
                {
                    float x = 2 * radius * (float)Math.Cos(Math.PI * angle / 180);
                    float y = 2 * radius * (float)Math.Sin(Math.PI * angle / 180);

                    using (SKPaint strokePaint = new SKPaint())
                    {
                        strokePaint.Color = SKColors.Green;
                        strokePaint.StrokeWidth = 2;

                        canvas.DrawLine(0, 0, x, y, strokePaint);
                    }
                }
            }
        }
    }
}
```

네 리프 클로버 표시 되지 않지만 클리핑 없이 렌더링 하기 어려울 수 있는 이미지입니다.

[![네 리프 클로버 페이지의 삼중 스크린샷](clipping-images//fourleafclover-small.png)](clipping-images/fourleafclover-large.png#lightbox)

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
