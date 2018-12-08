---
title: 폴리라인 및 파라메트릭 수식
description: 이 문서에서는 어떻게 렌더링을 사용 하 여 SkiaSharp에 모든 줄 매개 방정식을 사용 하 여 정의할 수 있습니다 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: c6328135e0310c7b10b89bf2e32ce62869b15cfb
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53059988"
---
# <a name="polylines-and-parametric-equations"></a>폴리라인 및 파라메트릭 수식

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_SkiaSharp 매개 방정식을 사용 하 여 정의할 수 있는 모든 줄을 렌더링 하는 데_

에 [ **SkiaSharp 곡선 및 경로** ](../curves/index.md) 섹션이이 가이드의 다양 한 방법을 볼 수는 [ `SKPath` ](xref:SkiaSharp.SKPath) 렌더링 특정 종류의 곡선을 정의 합니다. 그러나 경우가 유형의에서 직접 지원 하지 않는 곡선을 그리는 데 필요한 `SKPath`합니다. 이런 경우, 수학적으로 정의할 수 있는 모든 곡선을 그릴 다중선 (연결 된 선 컬렉션)를 사용할 수 있습니다. 줄을 충분히 작게를 다양 한 충분히 결과 모양은 곡선입니다. 이 나선형 3,600 거의 줄 실제로 같습니다.

![](polylines-images/spiralexample.png "나선형")

일반적으로 매개 방정식의 쌍을 기준으로 곡선을 정의 하는 것이 좋습니다. X 및 Y 좌표는 수식 됩니다 라고도, 세 번째 변수가 종속 `t` 시간에 대 한 합니다. 다음 파라메트릭 수식에 대 한 원 중심 점 (0, 0)에 1의 반지름을 정의 하는 예를 들어 *t* 0에서 1:

x = cos(2πt)

y sin(2πt) =

 반지름을 1 보다 큰 하려는 경우 단순히 해당 radius 사인 및 코사인 값을 곱한를 중앙의 다른 위치로 이동 해야 하는 경우 해당 값을 추가:

x = xCenter + radius·cos(2πt)

y = yCenter + radius·sin(2πt)

가로 및 세로 축 병렬을 사용 하 여 타원에 대 한 두 개의 반지름 관련 됩니다.

x = xCenter + xRadius·cos(2πt)

y = yCenter + yRadius·sin(2πt)

그런 다음 다양 한 지점을 계산한 다음 경로에 추가 하는 루프에 해당 하는 SkiaSharp 코드를 넣을 수 있습니다. 다음 SkiaSharp 코드는 `SKPath` 화면을 채우는 타원에 대 한 개체입니다. 루프는 직접 360도 통해 주기입니다. 가운데 너비와 높이 디스플레이 화면의 절반 이며 되므로 두 반지름.

```csharp
SKPath path = new SKPath();

for (float angle = 0; angle < 360; angle += 1)
{
    double radians = Math.PI * angle / 180;
    float x = info.Width / 2 + (info.Width / 2) * (float)Math.Cos(radians);
    float y = info.Height / 2 + (info.Height / 2) * (float)Math.Sin(radians);

    if (angle == 0)
    {
        path.MoveTo(x, y);
    }
    else
    {
        path.LineTo(x, y);
    }
}
path.Close();
```

이 인해 360 거의 선으로 정의 되는 타원입니다. 렌더링 될 때 부드러운 나타납니다.

물론 있으므로 다중선을 사용 하 여 타원을 만들 필요가 없습니다 `SKPath` 포함는 `AddOval` 를 수행 하는 메서드가 있습니다. 제공 하지 않는 시각적 개체를 그릴 수도 있습니다 하지만 `SKPath`합니다.

합니다 **Archimedean 나선형** 페이지에는 다음과 같은 코드 타원 코드 하지만 중요 한 차이가 있습니다. 이 루프 원의 360도 10 번 반지름을 지속적으로 조정:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(center.X, center.Y);

    using (SKPath path = new SKPath())
    {
        for (float angle = 0; angle < 3600; angle += 1)
        {
            float scaledRadius = radius * angle / 3600;
            double radians = Math.PI * angle / 180;
            float x = center.X + scaledRadius * (float)Math.Cos(radians);
            float y = center.Y + scaledRadius * (float)Math.Sin(radians);
            SKPoint point = new SKPoint(x, y);

            if (angle == 0)
            {
                path.MoveTo(point);
            }
            else
            {
                path.LineTo(point);
            }
        }

        SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 5
        };

        canvas.DrawPath(path, paint);
    }
}
```

결과 라고는 *산술 나선형* each 사이의 오프셋 상수 이므로:

[![](polylines-images/archimedeanspiral-small.png "삼중 Archimedean 나선형 페이지 스크린샷")](polylines-images/archimedeanspiral-large.png#lightbox "삼중 Archimedean 나선형 페이지 스크린샷")

있음을 합니다 `SKPath` 만들어집니다는 `using` 블록입니다. 이 `SKPath` 보다 더 많은 메모리를 사용 합니다 `SKPath` 제안 하는 이전 프로그램에서 개체를 `using` 블록은 모든 관리 되지 않는 리소스를 삭제 하는 것이 적합 합니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
