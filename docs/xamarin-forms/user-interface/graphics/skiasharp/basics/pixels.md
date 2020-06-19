---
title: 픽셀 및 디바이스 독립적 단위
description: 이 문서에서는 SkiaSharp 좌표와 좌표 간의 차이점을 알아보고 Xamarin.Forms 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: davidbritch
ms.author: dabritch
ms.date: 02/09/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6d01018f4393ac5562220fa1f9524bc0d9872c67
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137672"
---
# <a name="pixels-and-device-independent-units"></a>픽셀 및 디바이스 독립적 단위

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp 좌표와 좌표 간의 차이점 탐색 Xamarin.Forms_

이 문서에서는 SkiaSharp 및에서 사용 되는 좌표계의 차이점을 살펴봅니다 Xamarin.Forms . 두 좌표계 간에 변환 하는 정보를 얻을 수 있으며 특정 영역을 채우는 그래픽도 그릴 수 있습니다.

![](pixels-images/screenfillexample.png "An oval that fills the screen")

잠시 동안 프로그래밍 했다면 Xamarin.Forms 좌표와 크기에 대 한 느낌이 있을 수 있습니다 Xamarin.Forms . 위의 두 문서에 그려진 원은 약간 작은 것 처럼 보일 수 있습니다.

이러한 원은 크기와 *비교할 때* 작습니다 Xamarin.Forms . 기본적으로 SkiaSharp는 기본 Xamarin.Forms 플랫폼에 의해 설정 된 장치 독립적 단위에서 좌표와 크기를 기준으로 픽셀 단위로 그립니다. 좌표계에 대 한 자세한 내용은 Xamarin.Forms 5 장에서 찾을 수 있습니다 [. ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) *을 사용 하 여 Xamarin.Forms Mobile Apps를 만드는 *책의 크기를 처리 합니다.)

[**SkewSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 프로그램의 **Surface 크기** 에 해당 하는 페이지에서는 SkiaSharp 텍스트 출력을 사용 하 여 세 가지 원본에서 표시 표면의 크기를 표시 합니다.

- Xamarin.Forms [`Width`](xref:Xamarin.Forms.VisualElement.Width) [`Height`](xref:Xamarin.Forms.VisualElement.Height) 개체의 일반 및 속성 `SKCanvasView` 입니다.
- [`CanvasSize`](xref:SkiaSharp.Views.Forms.SKCanvasView.CanvasSize)개체의 속성입니다 `SKCanvasView` .
- [`Size`](xref:SkiaSharp.SKImageInfo.Size) `SKImageInfo` `Width` `Height` 이전 두 페이지에서 사용 된 및 속성과 일치 하는 값의 속성입니다.

[`SurfaceSizePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs)클래스는 이러한 값을 표시 하는 방법을 보여 줍니다. 생성자는 개체를 `SKCanvasView` 필드로 저장 하므로 이벤트 처리기에서 액세스할 수 있습니다 `PaintSurface` .

```csharp
SKCanvasView canvasView;

public SurfaceSizePage()
{
    Title = "Surface Size";

    canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvas`에는 6 가지 방법이 포함 되어 `DrawText` 있지만이 [`DrawText`](xref:SkiaSharp.SKCanvas.DrawText(System.String,System.Single,System.Single,SkiaSharp.SKPaint)) 메서드는 가장 간단 합니다.

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

텍스트 문자열, 텍스트를 시작할 X 및 Y 좌표와 개체를 지정 합니다 `SKPaint` . X 좌표는 텍스트의 왼쪽이 배치 되는 위치를 지정 하지만 조사식: Y 좌표는 텍스트 *기준선* 의 위치를 지정 합니다. 꺽은선형 용지를 기준으로 작성 한 경우 기준선은 문자가 앉아 있는 줄이 고, 아래에서 (예: g, p, q, y)의 하강 문자를 기준으로 합니다.

개체를 사용 하 여 `SKPaint` 텍스트의 색, 글꼴 패밀리 및 텍스트 크기를 지정할 수 있습니다. 기본적으로 속성의 [`TextSize`](xref:SkiaSharp.SKPaint.TextSize) 값은 12입니다 .이 값은 휴대폰 같은 고해상도 장치에서 작은 텍스트를 생성 합니다. 가장 간단한 응용 프로그램에서는 표시 되는 텍스트의 크기에 대 한 정보도 필요 합니다. `SKPaint`클래스는 [`FontMetrics`](xref:SkiaSharp.SKPaint.FontMetrics) 속성과 몇 가지 메서드를 정의 [`MeasureText`](xref:SkiaSharp.SKPaint.MeasureText(System.String)) 하지만, 더 작은 수준의 요구에 대해 [`FontSpacing`](xref:SkiaSharp.SKPaint.FontSpacing) 속성은 연속 된 텍스트 줄 간격을 나타내는 권장 값을 제공 합니다.

다음 `PaintSurface` 처리기는 `SKPaint` 40 픽셀의에 대 한 개체를 만듭니다 `TextSize` .이는 어센더의 위쪽부터 하강의 위쪽에 있는 텍스트의 원하는 세로 높이입니다. `FontSpacing`개체가 반환 하는 값은 `SKPaint` 약 47 픽셀 보다 약간 더 큽니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 40
    };

    float fontSpacing = paint.FontSpacing;
    float x = 20;               // left margin
    float y = fontSpacing;      // first baseline
    float indent = 100;

    canvas.DrawText("SKCanvasView Height and Width:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(String.Format("{0:F2} x {1:F2}",
                                  canvasView.Width, canvasView.Height),
                    x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKCanvasView CanvasSize:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(canvasView.CanvasSize.ToString(), x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKImageInfo Size:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(info.Size.ToString(), x + indent, y, paint);
}
```

메서드는 X 좌표 20 (왼쪽의 작은 여백) 및의 Y 좌표를 사용 하 여 첫 번째 텍스트 줄을 시작 합니다 `fontSpacing` .이는 표시 화면 위쪽에 있는 첫 번째 텍스트 줄의 전체 높이를 표시 하는 데 필요한 것 보다 약간 더 적습니다. 를 호출할 때마다 `DrawText` Y 좌표가 하나 또는 두 단위씩 증가 `fontSpacing` 합니다.

실행 중인 프로그램은 다음과 같습니다.

[![](pixels-images/surfacesize-small.png "Triple screenshot of the Surface Size  page")](pixels-images/surfacesize-large.png#lightbox "Triple screenshot of the Surface Size  page")

여기에서 볼 수 있듯이의 `CanvasSize` 속성과 `SKCanvasView` `Size` 값의 속성은 `SKImageInfo` 픽셀 차원을 보고 하는 것과 일치 합니다. `Height`의 및 `Width` 속성은 `SKCanvasView` 속성 이며 Xamarin.Forms 플랫폼에서 정의한 장치 독립적 단위에서 뷰의 크기를 보고 합니다.

왼쪽에 있는 iOS 7 시뮬레이터는 장치 독립적 단위 당 2 픽셀, 그리고 중앙의 Android Nexus 5에는 단위당 3 픽셀이 있습니다. 이전에 표시 된 단순한 원에는 플랫폼 마다 크기가 서로 다릅니다.

장치 독립적 단위로 완전히 작업 하려면의 속성을로 설정 하 여이 작업을 수행할 수 있습니다 `IgnorePixelScaling` `SKCanvasView` `true` . 그러나 결과와 다를 수 있습니다. SkiaSharp는 작은 장치 화면에 그래픽을 렌더링 하 고, 픽셀 크기는 장치 독립적 단위의 보기 크기와 동일 합니다. 예를 들어 SkiaSharp는 Nexus 5에서 360 x 512 픽셀의 디스플레이 화면을 사용 합니다. 그런 다음 해당 이미지 크기를 조정 하 여 눈에 띄는 비트맵 jaggies을 생성 합니다.

동일한 이미지 해상도를 유지 하기 위해 두 좌표계 간에 변환 하는 간단한 함수를 작성 하는 것이 더 나은 방법입니다.

는 메서드 외에 `DrawCircle` `SKCanvas` 도 타원을 그리는 두 개의 메서드를 정의 합니다 `DrawOval` . 타원은 단일 반지름이 아닌 두 반지름을 기준으로 정의 됩니다. 이를 *주 반지름* 및 *부 반경*이라고 합니다. `DrawOval`메서드는 X 축과 Y 축에 평행한 두 반지름을 사용 하 여 타원을 그립니다. X 축과 Y 축에 평행 하지 않은 축으로 타원을 그려야 하는 경우 [**호를 그리는 세 가지 방법**](../curves/arcs.md)문서에 설명 된 [**대로 회전 변환 또는 그래픽**](../transforms/rotate.md) 경로 문서에 설명 된 대로 회전 변환을 사용할 수 있습니다. 메서드의이 오버 로드는 [`DrawOval`](xref:SkiaSharp.SKCanvas.DrawOval(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) 두 반지름 매개 변수를 `rx` `ry` 지정 하 고 X 축과 Y 축에 평행이 되도록 지정 합니다.

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

표시 표면을 채우는 타원을 그릴 수 있나요? **타원 채우기** 페이지에서는 방법을 보여 줍니다. `PaintSurface` [**EllipseFillPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) 클래스의 이벤트 처리기는 `xRadius` `yRadius` 표시 화면 내에서 전체 타원과 해당 윤곽선에 맞게 및 값에서 스트로크 너비의 절반을 뺍니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float strokeWidth = 50;
    float xRadius = (info.Width - strokeWidth) / 2;
    float yRadius = (info.Height - strokeWidth) / 2;

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = strokeWidth
    };
    canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
}
```

실행 되 고 있습니다.

[![](pixels-images/ellipsefill-small.png "Triple screenshot of the Surface Size  page")](pixels-images/ellipsefill-large.png#lightbox "Triple screenshot of the Surface Size  page")

다른 [`DrawOval`](xref:SkiaSharp.SKCanvas.DrawOval(SkiaSharp.SKRect,SkiaSharp.SKPaint)) 메서드에는 [`SKRect`](xref:SkiaSharp.SKRect) 인수 (왼쪽 위 모퉁이와 오른쪽 아래 모퉁이의 X 및 Y 좌표를 기준으로 정의 된 사각형)가 있습니다. Oval은 사각형을 채워서 다음과 같이 **타원 채우기** 페이지에서 사용할 수 있습니다.

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

그러나 네 면에서 타원의 윤곽선에 대 한 모든 가장자리를 자릅니다. `SKRect` `strokeWidth` 이 작업을 수행 하려면에 따라 모든 생성자 인수를 조정 해야 합니다.

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
