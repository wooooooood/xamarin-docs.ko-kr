---
title: 픽셀 및 디바이스 독립적 단위
description: 이 문서에서는 Xamarin.Forms 좌표 SkiaSharp 좌표 사이의 차이점에 살펴보고 및 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: davidbritch
ms.author: dabritch
ms.date: 02/09/2017
ms.openlocfilehash: e2bf493a5d8a4197fbc59044edf126761b41cf8d
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649720"
---
# <a name="pixels-and-device-independent-units"></a>픽셀 및 디바이스 독립적 단위

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp 좌표 및 Xamarin.Forms 좌표 간의 차이점을 살펴보기_

이 문서에서는 SkiaSharp 및 Xamarin.Forms를 사용 하는 좌표계의 차이점을 살펴봅니다. 두 개의 좌표 시스템 간에 변환 하 고 특정 영역을 채우는 그래픽을 그릴 수도 정보를 얻을 수 있습니다.

![](pixels-images/screenfillexample.png "화면을 채우는 타원")

잠시 Xamarin.Forms에 프로그래밍 된 했으므로, 경우에 Xamarin.Forms 좌표와 크기에 대 한 느낌을 해야 합니다. 두 가지 이전 문서에서 그린 원에 약간 작은 보일 수 있습니다.

이러한 원 *는* Xamarin.Forms 크기에 비해 작은 합니다. 기본적으로 SkiaSharp Xamarin.Forms 기본 플랫폼에 의해 설정 된 장치 독립적 단위 좌표와 크기를 기반으로 하는 동안 픽셀 단위로 그립니다. (에서 Xamarin.Forms 좌표계에 대 한 자세한 정보를 찾을 수 있습니다 [5 장입니다. 크기를 다루는](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) 책 *Creating Mobile Apps with Xamarin.Forms*.)

페이지의 [ **SkewSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 이라는 제목의 프로그램 **화면 크기** SkiaSharp 텍스트 출력을 사용 하 여 서로 다른 세 원본의 디스플레이 화면의 크기를 표시 하려면:

- 일반 Xamarin.Forms [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) 하 고 [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) 의 속성을 `SKCanvasView` 개체.
- 합니다 [ `CanvasSize` ](xref:SkiaSharp.Views.Forms.SKCanvasView.CanvasSize) 의 속성을 `SKCanvasView` 개체입니다.
- [ `Size` ](xref:SkiaSharp.SKImageInfo.Size) 의 속성을 `SKImageInfo` 와 일치 하는 값입니다는 `Width` 및 `Height` 두 이전 페이지에서 사용 되는 속성입니다.

합니다 [ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) 클래스에는 이러한 값을 표시 하는 방법을 보여 줍니다. 생성자 저장 합니다 `SKCanvasView` 개체에 액세스할 수 있도록 필드로 `PaintSurface` 이벤트 처리기:

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

`SKCanvas` 다른 6 개 포함 되어 있습니다 `DrawText` 메서드에만이 [ `DrawText` ](xref:SkiaSharp.SKCanvas.DrawText(System.String,System.Single,System.Single,SkiaSharp.SKPaint)) 메서드는 가장 간단 합니다.

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

텍스트 문자열을 지정 하면, X 및 Y 좌표를 시작 하려면 텍스트 인 및 `SKPaint` 개체입니다. X 좌표는 텍스트의 왼쪽이 배치 되는 위치를 지정 하지만 감시 합니다. Y 좌표는 텍스트 *기준선* 의 위치를 지정 합니다. 그 어느 때 인라인된 종이에 손으로 작성 했다면, 기준 (예: g, p, q 및 y에 해당)는 디센더와 내림차순 아래는 자 사이트에 선입니다.

`SKPaint` 개체를 사용 하면 텍스트, 글꼴 패밀리 및 텍스트 크기의 색을 지정할 수 있습니다. 기본적으로 [ `TextSize` ](xref:SkiaSharp.SKPaint.TextSize) 속성이 휴대폰과 같은 고해상도 장치에서 작은 텍스트의 결과 12의 값입니다. 가장 간단한 응용 프로그램을 제외 해야 표시 하는 텍스트의 크기에 대 한 정보입니다. `SKPaint` 클래스 정의 [ `FontMetrics` ](xref:SkiaSharp.SKPaint.FontMetrics) 속성과 여러 [ `MeasureText` ](xref:SkiaSharp.SKPaint.MeasureText(System.String)) 메서드를 하지만 덜 멋진 요구 사항에는 [ `FontSpacing` ](xref:SkiaSharp.SKPaint.FontSpacing) 속성은 텍스트의 연속 줄 간격에 대 한 권장 되는 값을 제공합니다.

다음 `PaintSurface` 처리기를 만듭니다를 `SKPaint` 개체에 대 한는 `TextSize` 디센더와 아래쪽 어센더 위쪽에서 텍스트의 세로 높이 40 픽셀입니다. 합니다 `FontSpacing` 값을 `SKPaint` 47 픽셀에 대 한 그 보다 약간 큰 개체를 반환 합니다.

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

메서드 시작 (왼쪽에 있는 작은 여백)에 대 한 20의 X 좌표 및 Y 좌표를 사용 하 여 텍스트의 첫 번째 줄 `fontSpacing`, 화면 맨 위에 있는 텍스트의 첫 번째 줄의 전체 높이 표시 하는 데 필요한 것 보다 좀 더 많은 인 합니다. 호출한 후 `DrawText`, Y 좌표 중 하나 또는 두 개의 씩 증가 `fontSpacing`합니다.

실행 중인 프로그램이 다음과 같습니다.

[![](pixels-images/surfacesize-small.png "화면 크기 페이지 스크린샷 삼중")](pixels-images/surfacesize-large.png#lightbox "삼중 화면 크기 페이지 스크린샷")

알 수 있듯이 `CanvasSize` 의 속성을 `SKCanvasView` 및 `Size` 의 속성을 `SKImageInfo` 값은 픽셀 크기를 보고에 일관 되. `Height` 및 `Width` 의 속성을 `SKCanvasView` Xamarin.Forms 속성 및 플랫폼에 의해 정의 된 장치 독립적 단위에서 보기의 크기를 보고 합니다.

왼쪽에서 시뮬레이터가 시작 iOS 7 장치 독립적 단위 당 두 픽셀에 있고 가운데에 Android Nexus 5 단위당 3 픽셀로 합니다. 이유는 원 앞에서 살펴본 다양 한 플랫폼에 대 한 다양 한 크기입니다.

장치 독립적 단위에서 완전히 작동 하려는 경우 있습니다 이렇게 설정 합니다 `IgnorePixelScaling` 의 속성을 `SKCanvasView` 를 `true`. 그러나 결과 선호 하지 않을 수 있습니다. SkiaSharp 장치 독립적 단위에서 보기의 크기와 같은 픽셀 크기를 사용 하 여 더 작은 장치 화면에 그래픽을 렌더링합니다. (예를 들어, SkiaSharp 사용 360 x 512 픽셀의 화면 Nexus 5.) 그런 다음 확장 눈에 띄는 비트맵 계단의 크기에 해당 이미지를 구성 합니다.

동일한 이미지 해상도 유지 하려면 두 개의 좌표 시스템 간에 변환 하려면 간단한 함수를 직접 작성 하는 것이 좋습니다.

외에 `DrawCircle` 메서드를 `SKCanvas` 두 정의 `DrawOval` 메서드는 타원을 그립니다. 타원 단일 radius를 사용 하지 않고 두 반지름으로 정의 됩니다. 이러한 라고는 *주 radius* 하며 *부 radius*합니다. `DrawOval` 메서드 두 반지름을 사용 하 여 타원을 X 및 Y 축에 병렬 그립니다. (X 및 Y 축에 병렬 없는 축을 사용 하 여 타원을 그릴 경우 문서에 설명 된 대로 회전 변환을 사용할 수 있습니다 [ **The 회전 변환** ](../transforms/rotate.md) 또는에 설명 된 대로 그래픽 경로 문서 [ **호를 그리려면 세 가지 방법으로**](../curves/arcs.md)). 이 오버 로드 된 [ `DrawOval` ](xref:SkiaSharp.SKCanvas.DrawOval(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) 메서드는 두 개의 반지름 매개 변수 이름은 `rx` 및 `ry` X 및 Y 축에 병렬 됨을 나타내려면:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

화면을 채우는 타원을 그릴 수는? 합니다 **타원 채우기** 페이지를 보여 줍니다 하는 방법입니다. `PaintSurface` 이벤트 처리기는 [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) 클래스에서 스트로크 너비를 뺍니다 합니다 `xRadius` 및 `yRadius` 전체 타원에 맞게 값 및 해당 화면 내에서 간략하게 설명 합니다.

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

여기이 실행 됩니다.

[![](pixels-images/ellipsefill-small.png "화면 크기 페이지 스크린샷 삼중")](pixels-images/ellipsefill-large.png#lightbox "삼중 화면 크기 페이지 스크린샷")

다른 [ `DrawOval` ](xref:SkiaSharp.SKCanvas.DrawOval(SkiaSharp.SKRect,SkiaSharp.SKPaint)) 메서드가 [ `SKRect` ](xref:SkiaSharp.SKRect) 사각형의 왼쪽 위 모퉁이 오른쪽 아래 모퉁이의 X 및 Y 좌표를 기준으로 정의 되는 인수입니다. 타원을 사용할 수도 제안 하는 해당 사각형을 채우는 합니다 **타원 채우기** 이와 같은 페이지:

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

그러나는 네 면 타원 윤곽선의 모든 가장자리를 자릅니다. 모든 조정 해야 합니다 `SKRect` 기반으로 하는 생성자 인수를 `strokeWidth` 이 이렇게 하려면 오른쪽:

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
