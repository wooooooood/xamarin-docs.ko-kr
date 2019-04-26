---
title: 경로 및 지역 클리핑
description: 이 문서는 특정 영역에 SkiaSharp 클립 그래픽 경로 사용 하 고 영역을 만드는 방법에 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 4f8b6b7ea0db8d46886c3391f1aef3ba20a5be44
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61086052"
---
# <a name="clipping-with-paths-and-regions"></a>경로 및 지역 클리핑

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_클립 그래픽에 대 한 경로 사용 하 여 특정 영역을 영역을 만드는 데_

특정 영역에 대 한 그래픽 렌더링을 제한 하는 데 필요한 경우가 있습니다. 이 이라고 *클리핑*합니다. 클리핑 구멍을 통해 표시 되는 monkey의이 이미지와 같은 특수 한 효과 대해 사용할 수 있습니다.

![](clipping-images/clippingsample.png "Monkey 구멍을 통해")

합니다 *클리핑 영역* 그래픽 렌더링 되는 화면 영역입니다. 클리핑 영역 외부에 표시 되는 아무 것도 렌더링 되지 않습니다. 클리핑 영역을 사각형에 의해 일반적으로 정의 됩니다 요소나 [ `SKPath` ](xref:SkiaSharp.SKPath) 수 있지만 개체를 정의할 수 있습니다 또는 사용 하 여 클리핑 영역을 [ `SKRegion` ](xref:SkiaSharp.SKRegion) 개체. 이러한 두 가지 유형의 개체에서 먼저 하므로 것 처럼 보일 관련 경로에서 영역을 만들 수 있습니다. 그러나 지역에서 경로 만들 수 없습니다 하 고 내부적으로 매우 다른 지: 경로 일련의 가로 스캐닝선 지역 정의 되는 동안 일련의 선과 곡선을 구성 합니다.

위의 이미지에서 만든 합니다 **구멍 통해 Monkey** 페이지입니다. 합니다 [ `MonkeyThroughKeyholePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs) 클래스 SVG 데이터를 사용 하 여 경로 정의 하 고 프로그램 리소스에서 비트맵을 로드 하는 생성자를 사용 합니다.

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

하지만 `keyholePath` 는 구멍의 개요를 설명 하는 개체, 좌표를 완전히 임의적 이며 경로 데이터 고안 된 경우 무엇 이었습니까 편리 하 게 반영 합니다. 이러한 이유로 합니다 `PaintSurface` 처리기는이 경로 호출의 범위를 가져옵니다 `Translate` 및 `Scale` 화면 가운데에 경로 이동 하 고 거의 화면으로 길게 설정:


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

하지만 경로 렌더링 되지 않습니다. 대신, 변환, 다음 경로이 문 사용 하 여 클리핑 영역을 설정 하려면:

```csharp
canvas.ClipPath(keyholePath);
```

합니다 `PaintSurface` 처리기에 다시 호출 하 여 변환 `ResetMatrix` 화면의 전체 높이를 확장 하도록 비트맵을 그립니다. 이 코드 가정 비트맵 정사각형이 비트맵 인 합니다. 클리핑 경로 의해 정의 되는 영역 내 에서만 비트맵 렌더링 됩니다.

[![](clipping-images/monkeythroughkeyhole-small.png "삼중 스크린샷 구멍 페이지를 통해 Monkey")](clipping-images/monkeythroughkeyhole-large.png#lightbox "구멍 페이지를 통해 Monkey의 삼중 스크린 샷")

클리핑 패스가 적용 변환을 적용 시기를 `ClipPath` 메서드를 호출 하 고 변환에 적용 하면 그래픽 개체 (예: 비트맵)이 표시 되지 않습니다. 클리핑 패스를 사용 하 여 저장 되는 캔버스 상태의 일부인 합니다 `Save` 메서드 사용 하 여 복원는 `Restore` 메서드.

## <a name="combining-clipping-paths"></a>클리핑 패스 결합

엄격히 말해, 클리핑 영역 설정 되지 않습니다""는 `ClipPath` 메서드. 대신, 캔버스 크기가 같은 사각형으로 시작 하는 기존 클리핑 패스를 함께 사용 됩니다. 사용 하 여 클리핑 영역의 사각형 범위를 가져올 수 있습니다 합니다 [ `ClipBounds` ](xref:SkiaSharp.SKCanvas.ClipBounds) 속성 또는 [ `ClipDeviceBounds` ](xref:SkiaSharp.SKCanvas.ClipDeviceBounds) 속성입니다. 합니다 `ClipBounds` 속성이 반환을 `SKRect` 반영 하는 모든 변환 하는 값이 적용 되는 합니다. `ClipDeviceBounds` 속성에서 반환 된 `RectI` 값입니다. 정수 치수를 사용 하 여 사각형 이며 실제 픽셀 크기의 클리핑 영역을 설명 합니다.

호출 하 여 `ClipPath` 새 영역을 사용 하 여 클리핑 영역을 결합 하 여 클리핑 영역을 축소 합니다. 전체 구문은 합니다 [ `ClipPath` ](xref:SkiaSharp.SKCanvas.ClipPath(SkiaSharp.SKPath,SkiaSharp.SKClipOperation,System.Boolean)) 메서드는:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

이기도 한 [ `ClipRect` ](xref:SkiaSharp.SKCanvas.ClipRect(SkiaSharp.SKRect,SkiaSharp.SKClipOperation,System.Boolean)) 사각형을 사용 하 여 클리핑 영역을 결합 하는 메서드:

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

기본적으로 결과 클리핑 영역은 기존 클리핑 영역의 교집합 및 `SKPath` 또는 `SKRect` 에 지정 된 된 `ClipPath` 또는 `ClipRect` 메서드. 에 설명 되어이 **원 네 개의 교차 클립** 페이지입니다. `PaintSurface` 처리기에는 [ `FourCircleInteresectClipPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) 클래스를 다시 사용 된 동일한 `SKPath` 개체는 각 연속 호출을 통해 클리핑 영역을 축소 하는 4 개의 겹치는 원 만들려면 `ClipPath`:

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

남은 이러한 4 개의 원의 교집합입니다.

[![](clipping-images//fourcircleintersectclip-small.png "삼중 원 네 개의 교차 클립 페이지 스크린샷")](clipping-images/fourcircleintersectclip-large.png#lightbox "삼중 원 네 개의 교차 클립 페이지 스크린샷")

합니다 [ `SKClipOperation` ](xref:SkiaSharp.SKClipOperation) 열거형에는 두 명의 멤버:

- `Difference` 기존 클리핑 영역에서 지정 된 경로 또는 사각형을 제거합니다.

- `Intersect` 지정 된 경로 또는 기존 클리핑 영역을 사용 하 여 사각형을 교차

4를 대체 하는 경우 `SKClipOperation.Intersect` 인수에는 `FourCircleIntersectClipPage` 클래스 `SKClipOperation.Difference`, 다음과 같이 표시 됩니다.

[![](clipping-images//fourcircledifferenceclip-small.png "차이점 작업을 사용 하 여 4 개의 원 교차 클립 페이지 스크린샷 삼중")](clipping-images/fourcircledifferenceclip-large.png#lightbox "삼중 차이점 작업을 사용 하 여 4 개의 원 교차 클립 페이지 스크린샷")

4 개의 겹치는 원 클리핑 영역에서 제거 되었습니다.

합니다 **클립 작업** 페이지 원 쌍만을 사용 하 여 이러한 두 작업 간의 차이점을 보여 줍니다. 왼쪽의 첫 번째 원 기본 클립 작업을 사용 하 여 클리핑 영역에 추가 됩니다 `Intersect`반면 두 번째 원의 오른쪽 텍스트 레이블에 의해 표시 된 클립 작업을 사용 하 여 클리핑 영역에 추가 됩니다.

[![](clipping-images//clipoperations-small.png "클립 작업 페이지의 3 배가 스크린 샷")](clipping-images/clipoperations-large.png#lightbox "클립 작업 페이지의 3 배가 스크린 샷")

[ `ClipOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs) 클래스를 정의 두 `SKPaint` 필드로, 개체 및 다음 두 개의 사각형 영역으로 화면을 나눕니다. 이러한 영역은 가로 또는 세로 모드로 휴대폰 인지에 따라 다릅니다. 합니다 `DisplayClipOp` 클래스에는 다음 텍스트와 호출 표시 `ClipPath` 각 클립 작업을 설명 하기 위해 두 개의 원 경로 사용 하 여:

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

호출 `DrawPaint` 일반적으로 채울 전체 캔버스를 하면 `SKPaint` 개체 하지만 경우 메서드가 방금 그리는 클리핑 영역 내에서.

## <a name="exploring-regions"></a>지역 살펴보기

클리핑 영역을 정의할 수도 있습니다는 [ `SKRegion` ](xref:SkiaSharp.SKRegion) 개체입니다.

새로 만든 `SKRegion` 빈 영역을 설명 하는 개체입니다. 일반적으로 개체의 첫 번째 호출은 [ `SetRect` ](xref:SkiaSharp.SKRegion.SetRect(SkiaSharp.SKRectI)) 는 지역에 있는 사각형 영역에 설명 합니다. 매개 변수를 `SetRect` 은 `SKRectI` 값 &mdash; 픽셀 측면에서 사각형을 지정 하기 때문에 정수를 사용 하 여 사각형을 조정 합니다. 호출할 수 있습니다 [ `SetPath` ](xref:SkiaSharp.SKRegion.SetPath(SkiaSharp.SKPath,SkiaSharp.SKRegion)) 사용 하 여는 `SKPath` 개체입니다. 이 경로의 내부와 동일 하지만 잘린 초기 사각형 영역의 영역을 만듭니다.

지역 중 하나를 호출 하 여 수정할 수도 있습니다는 [ `Op` ](xref:SkiaSharp.SKRegion.Op*) 이 0.09716216215와 같은 메서드 오버 로드 합니다.

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

합니다 [ `SKRegionOperation` ](xref:SkiaSharp.SKRegionOperation) 열거형은 유사한 `SKClipOperation` 더 많은 멤버가 있지만:

- `Difference`

- `Intersect`

- `Union`

- `XOR`

- `ReverseDifference`

- `Replace`

수행 하면 해당 지역 합니다 `Op` 에 따라 매개 변수로 지정 된 지역에 대 한 호출이 함께 `SKRegionOperation` 멤버입니다. 마지막으로 가져올 때 지역 클리핑에 대 한 적합 한, 사용 하 여 캔버스 클리핑 영역으로 설정할 수 있습니다 합니다 [ `ClipRegion` ](xref:SkiaSharp.SKCanvas.ClipRegion(SkiaSharp.SKRegion,SkiaSharp.SKClipOperation)) 메서드의 `SKCanvas`:

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

다음 스크린샷은 6 개 지역 작업을 기반으로 하는 클리핑 영역을 보여 줍니다. 왼쪽된 원을 지역은 합니다 `Op` 메서드가 호출 되 고 오른쪽 원이 전달할 지역은 `Op` 메서드:

[![](clipping-images//regionoperations-small.png "영역 작업 페이지의 3 배가 스크린샷")](clipping-images/regionoperations-large.png#lightbox "삼중 영역 작업 페이지 스크린샷")

이러한 모든 가능성 결합 하는 이러한 두 개의 원을?합니다 결과 이미지 자체에 표시 되는 세 가지 구성 요소 조합으로 고려해 야 합니다 `Difference`, `Intersect`, 및 `ReverseDifference` 작업. 조합 총 번호가을 세 제곱 2 개 또는 8입니다. 누락 된 두 가지를 원래 지역 (에서 호출 하지 않으면 결과 `Op` 전혀) 및 완전히 빈 영역을 합니다.

하기가 어렵다는 문제가 먼저 경로 및 지역 해당 경로에서 만들고 그런 다음 여러 영역을 결합 해야 하기 때문에 클리핑에 대 한 영역을 사용 합니다. 전체 구조를 **영역 작업** 페이지는 매우 비슷합니다 **클립 Operations** 되지만 [ `RegionOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) 클래스 6 개의 영역으로 화면을 분할 및 이 작업에 대 한 영역을 사용 하는 데 필요한 추가 작업을 보여 줍니다.

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

다음 사이 큰 차이가는 `ClipPath` 메서드 및 `ClipRegion` 메서드:

> [!IMPORTANT]
> 달리 합니다 `ClipPath` 메서드는 `ClipRegion` 메서드 변환의 영향을 받지 않습니다.

이러한 차이 대 한 근거를 이해 하려면 이해 하면 도움이 됩니다 어떤 지역이 있습니다. 클립 작업 또는 지역 작업 수 구현 방법을 내부적으로 하는 방법에 대 한 고려 하는 경우 아마도 것이 매우 복잡 합니다. 여러 잠재적으로 매우 복잡 한 경로 결합 및 결과 경로의 윤곽선을 알고리즘 악몽 될 수입니다.

이 작업은 각 경로 가로 검색, 여러 줄 구식 진공 tube Tv의에서 함수와 같이 축소 된 경우 상당히 간소화 됩니다. 각 스캐닝선은 시작점 및 끝점을 사용 하 여 가로줄 하기만 하면 됩니다. 예를 들어 10 픽셀의 radius 사용 하 여 원은 원의 왼쪽된 부분에서 시작 하 고 오른쪽 부분에서 끝납니다 각각 20 가로 검색 선으로 분해할 수 있습니다. 영역 작업을 사용 하 여 두 개의 원을 결합 됩니다 해당 스캐닝선의 각 쌍의 시작 및 끝 좌표를 검사 하는 단순히 이기 때문에 매우 간단 합니다.

이것이 영역은입니다. 영역을 정의 하는 가로 스캐닝선의 일련입니다.

그러나 일련의 검색 영역 축소 된 경우 줄 줄 특정 픽셀 크기를 기반으로 이러한 검색 합니다. 엄격히 말해서, 지역 벡터 그래픽 개체가 아닙니다. 경로 보다 압축 된 흑백 비트맵에 본질적으로 더 가까운 것입니다. 따라서 지역 크기를 조정 하거나 회전 클리핑 영역에 사용 되는 경우 변환 되지 않습니다이 따라서 한 충실도 유지 하면서 수 없습니다.

그러나 그리기 위해 지역에 변환을 적용할 수 있습니다. 합니다 **지역 그리기** 프로그램 뛰어난 영역의 내부 특성을 보여 줍니다. [ `RegionPaintPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs) 클래스를 만듭니다는 `SKRegion` 기준으로 개체를 `SKPath` 10 단위 radius 원입니다. 변환에는 다음 페이지를 채우기에 원을 확장 합니다.

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

합니다 `DrawRegion` 호출 주황색 영역을 채우는 동안는 `DrawPath` 호출 원래 경로 비교에 대 한 파란색 선:

[![](clipping-images//regionpaint-small.png "그리기 영역 페이지의 3 배가 스크린샷")](clipping-images/regionpaint-large.png#lightbox "삼중 영역 그리기 페이지 스크린샷")

지역은 일련의 개별 좌표 명확 하 게 합니다.

변형을 사용 하 여 클리핑 영역와 관련 하 여 필요 하지 않으면,으로 사용할 수 있습니다 지역 클리핑에 대 한 합니다 **네-잎 클로버** 페이지를 보여 줍니다. 합니다 [ `FourLeafCloverPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs) 클래스 순환 네 개 지역에서 복합 영역 생성 복합 지역 클리핑 영역으로 설정 하 고 다음 일련의 페이지는 페이지의 가운데에서 360 직선을 그립니다.

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

네-잎 클로버 처럼 실제로 표시 되지 않습니다 이지만 하드 클리핑 없이 렌더링 될 수 있는 이미지:

[![](clipping-images//fourleafclover-small.png "삼중 네-잎 클로버 페이지 스크린샷")](clipping-images/fourleafclover-large.png#lightbox "삼중 네-잎 클로버 페이지 스크린샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
