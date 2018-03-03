---
title: "경로 및 영역을 사용 하 여 클리핑"
description: "클립 그래픽 경로 사용 하 여 특정 영역을 영역을 만드는 데"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/16/2017
ms.openlocfilehash: b1c5b64725a163e15f07d2aecaea4e56b7ecec2e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="clipping-with-paths-and-regions"></a>경로 및 영역을 사용 하 여 클리핑

_클립 그래픽 경로 사용 하 여 특정 영역을 영역을 만드는 데_

그래픽 렌더링 특정 영역으로 제한 하는 경우가 가끔 있습니다. 로 알려져 *클리핑*합니다. 이 이미지는 구멍 차례로 살펴 보았을 원숭이의 등의 특수 효과 대 한 클리핑을 사용할 수 있습니다.

![](clipping-images/clippingsample.png "구멍을 통해 원숭이")

*클리핑 영역* 그래픽 렌더링 되는 화면 영역입니다. 클리핑 영역 외부에 표시 되는 아무 것도 렌더링 되지 않습니다. 클리핑 영역이 정의한 일반적으로 [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) 있지만 개체 수 또는 정의할 사용 하 여 오려낸 영역은 [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) 개체입니다. 이러한 두 가지 유형의 개체에 우선 하므로 것 처럼 보일 관련 경로에서 영역을 만들 수 있습니다. 그러나는 지역에서 경로 만들 수 없습니다 및 내부적으로 매우 다른 지: 경로 일련의 가로 스캐닝선 정의 되는 영역 동안 일련의 선 및 곡선을 구성 합니다.

위 이미지에서 만들어진는 **구멍을 통해 원숭이** 페이지. [ `MonkeyThroughKeyholePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs) 클래스 SVG 데이터를 사용 하 여 경로 정의 하 고 프로그램 리소스에서 비트맵을 로드 하는 생성자를 사용 하 여:

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

하지만 `keyholePath` 는 구멍 개요를 설명 하는 개체, 좌표 완전히 임의적 이며 경로 데이터를 수립 하는 경우 무엇 이었습니까 편리 하 게 반영 합니다. 이러한 이유로 `PaintSurface` 처리기가 경로 호출의 범위를 가져오면 `Translate` 및 `Scale` 화면 중앙에 경로 이동 하 고 거의 화면 길게 설정:


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

하지만 경로 렌더링 되지 않습니다. 대신, 변환, 다음 경로이 문 사용 하 여 오려낸 영역을 설정 하려면 사용 됩니다.

```csharp
canvas.ClipPath(keyholePath);
```

`PaintSurface` 처리기에 다음을 호출 하 여 변환을 다시 설정 `ResetMatrix` 화면의 전체 높이를 확장 하는 비트맵을 그립니다. 이 코드는 비트맵 이라고 가정 정사각형을이 비트맵 인 합니다. 비트맵에서 클리핑 패스에 정의 된 영역 내에 렌더링 됩니다.

[![](clipping-images/monkeythroughkeyhole-small.png "구멍 페이지를 통해 원숭이의 삼중 스크린 샷")](clipping-images/monkeythroughkeyhole-large.png "구멍 페이지를 통해 원숭이의 삼중 스크린 샷")

클리핑 패스 변환에 적용 되 면는 `ClipPath` 메서드를 호출 하 고에 변환을 적용 때 그래픽 개체 (예: 비트맵)이 표시 되지 않습니다. 클리핑 패스는 캔버스 상태와 함께 저장 하는 부분에서 `Save` 메서드 되 고 있는 복원 된는 `Restore` 메서드.

## <a name="combining-clipping-paths"></a>클리핑 패스 결합

엄격히 말해서, 클리핑 영역 설정 되지 않습니다""는 `ClipPath` 메서드. 대신, 결합 된 화면 크기를 동일 사각형으로 시작 하는 기존 클리핑 경로 있습니다. 사용 하 여 오려낸 영역의 사각형 범위를 가져올 수 있습니다는 [ `ClipBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipBounds/) 속성 또는 [ `ClipDeviceBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipDeviceBounds/) 속성입니다. `ClipBounds` 속성에서 반환 된 `SKRect` 반영 하는 변환 된 값이 적용 되는 합니다. `ClipDeviceBounds` 속성에서 반환 된 `RectI` 값입니다. 정수 차원 사각형 이며 실제 픽셀 치수의 클립 영역을 설명 합니다.

호출에 `ClipPath` 새 영역을 사용 하 여 오려낸 영역을 결합 하 여 오려낸 영역을 축소 합니다. 전체 구문을 [ `ClipPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipPath/p/SkiaSharp.SKPath/SkiaSharp.SKClipOperation/System.Boolean/) 방법은:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

또한 한 [ `ClipRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRect/p/SkiaSharp.SKRect/SkiaSharp.SKClipOperation/System.Boolean/) 사각형 오려낸 영역 조합 된 메서드:

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

기본적으로 결과 클리핑 영역은 기존 클리핑 영역의 교집합 및 `SKPath` 또는 `SKRect` 에 지정 된 된 `ClipPath` 또는 `ClipRect` 메서드. 이 확인할는 **4 개의 원 교차 클립** 페이지. `PaintSurface` 의 처리기는 [ `FourCircleInteresectClipPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) 클래스를 다시 사용 동일한 `SKPath` 연속 호출을 통해 클리핑 영역 축소는 4 개의 겹치는 원 만들 개체 `ClipPath`:

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

[![](clipping-images//fourcircleintersectclip-small.png "4 개의 원 교차 클립 페이지의 삼중 스크린샷")](clipping-images/fourcircleintersectclip-large.png "4 개의 원 교차 클립 페이지의 삼중 스크린샷")

[ `SKClipOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKClipOperation/) 열거형에는 두 명의 멤버:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Difference/) 기존 클리핑 영역에서 지정 된 경로 또는 사각형을 제거합니다.

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Intersect/) 지정 된 경로 또는 기존 클리핑 영역 직사각형을 교차합니다.

4 개의 대체 하는 경우 `SKClipOperation.Intersect` 인수에는 `FourCircleIntersectClipPage` 클래스와 `SKClipOperation.Difference`, 다음에 표시 됩니다.

[![](clipping-images//fourcircledifferenceclip-small.png "차이점 작업을 4 개의 원 교차 클립 페이지의 삼중 스크린샷")](clipping-images/fourcircledifferenceclip-large.png "차이 연산 사용 하 여 4 개의 원 교차 클립 페이지의 삼중 스크린 샷")

4 개의 겹치는 원 클리핑 영역에서 제거 되었습니다.

**클립 작업** 페이지 원 쌍만과 두 가지 동작 차이점을 보여 줍니다. 왼쪽의 첫 번째 원의 기본 클립 작업과 클리핑 영역에 추가 될 `Intersect`, 텍스트 레이블으로 표시 된 클립 작업 함께 오른쪽에 두 번째 원이 클리핑 영역에 추가 됩니다.

[![](clipping-images//clipoperations-small.png "클립 작업 페이지의 삼중 스크린 샷")](clipping-images/clipoperations-large.png "클립 작업 페이지의 삼중 스크린 샷")

[ `ClipOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs) 두 클래스 정의 `SKPaint` 필드로 개체를 다음 두 개의 사각형 영역으로 화면 나눕니다. 이러한 영역 전화가 세로 또는 가로 모드 인지에 따라 다릅니다. `DisplayClipOp` 클래스에는 다음 텍스트와 호출 표시 `ClipPath` 각 잘라내기 작업을 설명 하기 위해 두 개의 원 경로:

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

호출 `DrawPaint` 를 채울 전체 캔버스를 정상적으로 인해 `SKPaint` 개체 하지만 경우 메서드가 방금 그리는 클리핑 영역 내에서.

## <a name="exploring-regions"></a>영역을 탐색합니다.

에 대 한 API 설명서 탐색 했습니다 경우 `SKCanvas`, 오버 로드를 발견할 수는 `ClipPath` 및 `ClipRect` 메서드는 위에 설명 된 방법과 유사 하지만, 대신 명명 된 매개 변수는 [ `SKRegionOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegionOperation/) 대신 `SKClipOperation`합니다. `SKRegionOperation` 양식 클리핑 영역에 대 한 경로 결합 하 여 다소 더 많은 유연성을 제공 하는 6 개 멤버가 포함 됩니다.

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Difference/)

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Intersect/)

- [`Union`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Union/)

- [`XOR`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.XOR/)

- [`ReverseDifference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.ReverseDifference/)

- [`Replace`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Replace/)

그러나 오버 로드 `ClipPath` 및 `ClipRect` 와 `SKRegionOperation` 매개 변수가 사용 되지 않으며 사용할 수 없습니다.

계속 사용할 수 있습니다는 `SKRegionOperation` 의 측면에서 클리핑 영역을 정의 하는 열거형 되지만 필요는 [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) 개체입니다.

새로 만든 `SKRegion` 빈 영역을 설명 하는 개체입니다. 일반적으로 개체에 첫 번째 호출 [ `SetRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetRect/p/SkiaSharp.SKRectI/) 를 지역 사각형 영역에 설명 합니다. 매개 변수를 `SetRect` 는는 `SKRectI` 값 & #x 2014; 사각형 값과 정수 속성입니다. 호출할 수 있습니다 [ `SetPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetPath/p/SkiaSharp.SKPath/SkiaSharp.SKRegion/) 와 `SKPath` 개체입니다. 이 메서드는 경로, 내부 동일 하지만 초기 사각형 영역에 클리핑 하는 영역을 만듭니다.

`SKRegionOperation` 열거형에 중 하나를 호출할 때만 발생는 [ `Op` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.Op/p/SkiaSharp.SKRegion/SkiaSharp.SKRegionOperation/) 와 같은 메서드 오버 로드 합니다.

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

수행 하면 해당 영역에서 `Op` 대 한 호출에 따라 매개 변수로 지정 된 영역와 결합 하는 `SKRegionOperation` 멤버입니다. 마지막으로 가져오는 경우 영역 클리핑에 적합 한,으로 설정할 수 있습니다를 사용 하 여 캔버스의 클립 영역에서 [ `ClipRegion` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRegion/p/SkiaSharp.SKRegion/SkiaSharp.SKClipOperation/) 방식의 `SKCanvas`:

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

다음 스크린 샷에서 6 개의 영역 작업에 따라 클리핑 영역을 보여 줍니다. 왼쪽된 circle은 지역 하는 `Op` 메서드를 호출 하 고 오른쪽 원은 지역에 전달 되는 `Op` 메서드:

[![](clipping-images//regionoperations-small.png "영역 작업 페이지의 삼중 스크린샷")](clipping-images/regionoperations-large.png "영역 작업 페이지의 삼중 스크린샷")

이러한 모든 가능성을 결합 하 여 이러한 두 개의 원을?합니다 결과 이미지 자체에 표시 되는 세 가지 구성의 조합으로 고려는 `Difference`, `Intersect`, 및 `ReverseDifference` 작업 합니다. 조합 총 수를 3 제곱 2 개 또는 8은 합니다. 누락 된 두 가지 원본 영역 (에서 호출 하지 줄어들고 결과적 `Op` 전혀) 및 완전히 빈 지역입니다.

먼저 해당 경로에서 경로 및 영역을 만드는 한 후 여러 영역을 결합 해야 하기 때문에 캡처에 대 한 영역을 사용 하기가 더 어렵습니다. 전체 구조는 **영역 작업** 페이지는 매우 비슷합니다 **클립 작업** 있지만 [ `RegionOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) 클래스 6 개의 영역으로 화면을 분할 하 고 이 작업에 대 한 영역을 사용 하는 데 필요한 추가 작업을 보여 줍니다.

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

다음은 가장 큰 차이 `ClipPath` 메서드 및 `ClipRegion` 메서드:

> [!IMPORTANT]
> 와 달리는 `ClipPath` 메서드는 `ClipRegion` 메서드 변환의 영향을 받지 않습니다.

이러한 차이 대 한 이유를 이해 하려면 이해 하면 도움이 됩니다 어떤 영역은입니다. 어떻게 클립 작업 또는 작업 영역 구현 하 고 내부적으로 생각 한, 경우 아마도 것이 매우 복잡 한 합니다. 잠재적으로 매우 복잡 한 여러 개의 패스를 결합 하 고 결과 경로가 개요 알고리즘 밋 될 수입니다.

하지만이 작업은 각 경로 일련의 구식 항목이 튜브 Tv에에서 있는 수평 스캔 선 줄이면 크게 간소화 됩니다. 각 검색 줄은 단순히 하는 가로줄이 시작점 및 끝점입니다. 예를 들어 원형 10의 반지름으로 원의 왼쪽된 부분에서 시작 하 고 오른쪽 부분에서 끝납니다는 각각 20 수평 스캔 행으로 분해할 수 있습니다. 두 개의 원을 하며 지역 작업이 결합 됩니다 해당 스캐닝선 각 쌍의 시작 및 끝 좌표 검사 하기만 되었기 때문에 매우 간단한.

값은 영역은: 영역을 정의 하는 일련의 가로 스캐닝선 합니다.

그러나 일련의 검사 영역 축소 된 경우 정렬, 특정 픽셀 치수를 기반으로 하는 줄이이 검색 됩니다. 엄격히 말해서, 지역 벡터 그래픽 개체가 아닙니다. 것과 경로 보다 압축된 단색 비트맵에에서 가깝습니다. 따라서 영역 크기가 조정 하거나 충실도 유지 하면서 및 클리핑 영역에 사용 되는 경우 변환 되지 않습니다는 이러한 이유로 회전할 수 없습니다.

그러나 그리기 위해 지역에 변환을 적용할 수 있습니다. **지역 페인트** 프로그램 생생하게 영역의 내부 특성을 보여 줍니다. [ `RegionPaintPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs) 클래스를 만듭니다는 `SKRegion` 기반 개체는 `SKPath` 10 단위 radius 원의 합니다. 변환은 다음 페이지를 채우기에 해당 원의 확장 합니다.

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

`DrawRegion` 호출 주황색으로 영역을 채우는 동안는 `DrawPath` 호출 파란색 비교에 대 한 원래 경로 획:

[![](clipping-images//regionpaint-small.png "그리기 영역 페이지의 삼중 스크린샷")](clipping-images/regionpaint-large.png "영역 페인트 페이지의 삼중 스크린샷")

지역은 일련의 개별 좌표입니다.

오려낸 영역을 관련 하 여 변환을 사용 하 여 필요 하지 않으면,으로 사용할 수 있습니다 영역 클리핑에는 **네-리프 Clover** 페이지를 보여 줍니다. [ `FourLeafCloverPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs) 클래스 순환 4 개 지역에서 복합 영역을 생성, 클리핑 영역으로 복합 해당 영역을 설정 하 고 다음 일련의 시작 페이지의 가운데에서 360 직선을 그립니다.

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

स ो द 실제로 ि 네-리프 clover 처럼 이지만 클리핑 없이 렌더링 하기 어려운 될 수 있는 이미지:

[![](clipping-images//fourleafclover-small.png "4 개의-리프 Clover 페이지의 삼중 스크린샷")](clipping-images/fourleafclover-large.png "네-리프 Clover 페이지의 삼중 스크린샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
