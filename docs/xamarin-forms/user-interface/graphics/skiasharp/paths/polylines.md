---
title: 폴리라인 및 파라메트릭 수식
description: 이 문서는 모든 줄을 렌더링을 사용 하 여 SkiaSharp를 정의 하는 방법을 파라메트릭 수식이 포함을 설명 하 고 샘플 코드와 함께이 보여 줍니다.
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 9539a21b7dbc91da63795639610886233ed705be
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245311"
---
# <a name="polylines-and-parametric-equations"></a>폴리라인 및 파라메트릭 수식

_SkiaSharp 매개 방정식을 정의할 수 있는 줄을 사용 하 여_

이 가이드의 뒷부분에 나오는 부분에서는 다양 한 메서드를 볼 수 있습니다는 `SKPath` 렌더링 특정 종류의 곡선을 정의 합니다. 그러나는 유형의에서 직접 지원 하지 않는 곡선을 그리는 데 필요한 때이 `SKPath`합니다. 이러한 경우를 수학적으로 정의할 수 있는 모든 곡선을 그리는 폴리라인 (연결 된 선 컬렉션)를 사용할 수 있습니다. 줄 만큼 작지 않아서 만들고 다양 한 충분히 결과 버리면 곡선입니다. 이 나선은 3, 600 작은 선이 실제로입니다.

![](polylines-images/spiralexample.png "나선")

일반적으로 파라메트릭 수식 쌍 측면에서 곡선을 정의 하는 것이 좋습니다. X 및 Y 좌표는 대 한 수식을 이들은 라고도 하는 세 번째 변수가 의존 `t` 시간에 대 한 합니다. 다음 파라메트릭 수식에 대 한은 1 (0, 0) 하는 지점에 중심이 있는 원을 정의 하는 예를 들어 *t* 0에서 1:

 x = y cos(2πt) sin(2πt) =

 radius를 지정 하려면 1 보다 큰 단순히 해당 radius 사인과 코사인 값을 곱해 추가 하는 중앙의 다른 위치로 이동 해야 하는 경우 해당 값:

 x = xCenter + radius·cos(2πt) y = yCenter + radius·sin(2πt)

가로 및 세로 축 병렬을 타원에 대 한 두 개의 반지름은 다음과 같습니다.

x = xCenter + xRadius·cos(2πt) y = yCenter + yRadius·sin(2πt)

그런 다음 다양 한 포인트를 계산 하 고 경로 확인란을 추가 하는 루프에 해당 하는 SkiaSharp 코드를 넣을 수 있습니다. 다음 SkiaSharp 코드 만듭니다는 `SKPath` 표시 화면을 채우는 타원에 대 한 개체입니다. 루프는 직접 360도을 순환합니다. 중심은 절반 너비와 높이 디스플레이 화면의 함께 두 개의 반지름:

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

물론, 때문에 다중선을 사용 하 여 타원 만들 필요가 없습니다 `SKPath` 포함는 `AddOval` 을 수행 하는 메서드입니다. 그러나에서 제공 하지 않는 시각적 개체를 그리는 데 경우가 `SKPath`합니다.

**Archimedean 나선** 페이지와 유사한 코드의 타원 코드 하지만 중요 한 차이점입니다. 반복 360도 원의 중심 10 번 반지름을 지속적으로 조정:

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

결과 라고도 *산술 나선* 각 루프 간의 오프셋도 상수 이므로:

[![](polylines-images/archimedeanspiral-small.png "Archimedean 나선 페이지의 삼중 스크린샷")](polylines-images/archimedeanspiral-large.png#lightbox "Archimedean 나선 페이지의 삼중 스크린샷")

에 `SKPath` 에서 만든는 `using` 블록입니다. 이 `SKPath` 보다 더 많은 메모리를 소비는 `SKPath` 를 제안 하는 이전 프로그램의 개체는 `using` 블록은 모든 관리 되지 않는 리소스를 삭제 하기 위해 더 적합 합니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
