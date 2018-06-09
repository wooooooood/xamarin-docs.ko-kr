---
title: 픽셀과 장치 독립적 단위
description: 이 문서는 SkiaSharp 좌표 및 Xamarin.Forms 좌표 간의 차이점을 탐색 하 고 샘플 코드와 함께이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 2c7521fbedf1759c32e18d9c7f2a8fc6f0903587
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244141"
---
# <a name="pixels-and-device-independent-units"></a>픽셀과 장치 독립적 단위

_탐색 SkiaSharp 좌표 및 좌표 Xamarin.Forms 간의 차이점_

이 문서는 SkiaSharp 및 Xamarin.Forms를 사용 하는 좌표계의 차이 탐색 합니다. 두 개의 좌표 시스템 간에 변환 하 고 특정 영역을 작성 하는 그래픽을 그릴에 정보를 얻을 수 있습니다.

![](pixels-images/screenfillexample.png "화면을 채우는 타원")

잠시 동안 Xamarin.Forms에 프로그래밍 된 했으므로, 경우에 Xamarin.Forms 좌표 및 크기에 대 한 느낌을 할 수 있습니다. 두 이전 문서에 그려진 원 약간 작아 것 처럼 보일 수 있습니다.

이러한 원 *는* Xamarin.Forms 크기에 비해 작은 합니다. 기본적으로 SkiaSharp Xamarin.Forms 좌표 및 크기를 기본 플랫폼에 의해 설정 된 장치 독립적 단위에 기반 하는 동안 픽셀 단위로 그립니다. (에 Xamarin.Forms 좌표 시스템에 대 한 자세한 내용은 있습니다 [5 장 합니다. 크기를 다루는](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) 책의 *Xamarin.Forms 사용 하 여 모바일 앱 만들기*.)

페이지에는 [ **SkewSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 이라는 제목의 프로그램 **화면 크기** SkiaSharp 텍스트 출력을 사용 하 여 서로 다른 세 원본의 디스플레이 화면의 크기를 표시 하려면:

- 일반 Xamarin.Forms [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) 및 [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) 의 속성은 `SKCanvasView` 개체입니다.
- [ `CanvasSize` ](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKCanvasView.CanvasSize/) 의 속성은 `SKCanvasView` 개체입니다.
- [ `Size` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Size/) 속성은 `SKImageInfo` 와 일치 하는 값은 `Width` 및 `Height` 두 이전 페이지에 사용 되는 속성입니다.

[ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) 클래스에는 이러한 값을 표시 하는 방법을 보여 줍니다. 생성자 저장은 `SKCanvasView` 개체에 액세스할 수 있도록를 필드로 `PaintSurface` 이벤트 처리기.

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

`SKCanvas` 6 개의 다른 포함 `DrawText` 메서드, 하지만이 [ `DrawText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawText/p/System.String/System.Single/System.Single/SkiaSharp.SKPaint/) 메서드는 가장 간단한 방법입니다.

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

텍스트 문자열을 지정 하면, X 및 Y 좌표를 시작 하려면 텍스트는 여기서 및는 `SKPaint` 개체입니다. X 좌표 주의 있지만 왼쪽 텍스트의 배치 위치 지정:의 위치를 지정 하는 Y 좌표는 *초기* 텍스트의 합니다. 초기 적이 줄이 그어진된 용지에 직접 작성 한, 인지 한 행을 어떤 문자 sit 아래 있는 하강 문자 (u g, p, q, 및 y)에서 상속 합니다.

`SKPaint` 개체 텍스트의 글꼴 패밀리, 텍스트 크기의 색을 지정할 수 있습니다. 기본적으로는 [ `TextSize` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.TextSize/) 속성 짐 고해상도 장치에서 매우 작은 텍스트를 12, 값이 있습니다. 가장 간단한 응용 프로그램을 제외 해야 표시 하는 텍스트의 크기에 대 한 정보입니다. `SKPaint` 클래스 정의 [ `FontMetrics` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontMetrics/) 속성과 여러 [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) 메서드 하지만 덜 고급 요구에 따라는 [ `FontSpacing` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontSpacing/) 속성은 간격 연속 되는 줄의 텍스트에 대 한 권장된 값을 제공 합니다.

다음 `PaintSurface` 처리기 만듭니다는 `SKPaint` 개체에 대 한는 `TextSize` 어센더 맨 위에서 맨 아래 디센더 텍스트의 세로 높이 40 픽셀입니다. `FontSpacing` 값은 `SKPaint` 47 픽셀에 대 한 것 보다 약간 큰 개체를 반환 합니다.

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

메서드가 시작 (왼쪽에 거의 여백)에 대 한 20의 X 좌표와 Y 좌표는 텍스트의 첫 번째 줄 `fontSpacing`, 되는 화면 맨 위에 있는 텍스트의 첫 번째 줄의 전체 높이 표시 하는 데 필요한 작업 보다 약간 더 합니다. 호출할 때마다 `DrawText`, Y 좌표 하나 또는 두 개의 씩 만큼 증가 `fontSpacing`합니다.

다음은 세 플랫폼 모두에서 실행 중인 프로그램입니다.

[![](pixels-images/surfacesize-small.png "화면 크기 페이지의 삼중 스크린샷")](pixels-images/surfacesize-large.png#lightbox "화면 크기 페이지의 삼중 스크린 샷")

볼 수 있듯이 `CanvasSize` 속성의는 `SKCanvasView` 및 `Size` 속성의는 `SKImageInfo` 값 픽셀 크기를 보고에서 일관성이 있습니다. `Height` 및 `Width` 의 속성은 `SKCanvasView` Xamarin.Forms 속성 및 플랫폼에 의해 정의 된 장치 독립적 단위에서 뷰의 크기를 보고 합니다.

왼쪽에서 시뮬레이터가 iOS 7 장치 독립적 단위 당 2 픽셀 많고 센터에서 Android Nexus 5 단위 당 3 픽셀입니다. 바로 이러한 이유로 원 앞에 표시 된 여러 플랫폼에 대 한 다양 한 크기입니다.

장치 독립적 단위에서 모든 작업 하려는 경우 후 그렇게 설정 하 여는 `IgnorePixelScaling` 의 속성은 `SKCanvasView` 를 `true`합니다. 그러나 결과 선호 하지 수도 있습니다. SkiaSharp 장치 독립적 단위에서 보기의 크기와 같은 픽셀 크기가 더 작은 장치 화면에서 그래픽 렌더링합니다. (예를 들어 SkiaSharp가 사용 하 여 360 x 512 픽셀의 디스플레이 화면 Nexus 5.) 다음 확장 이미지 크기를 크게 비트맵 계단에 있습니다.

동일한 이미지 해상도 유지 하려면 두 개의 좌표 시스템 사이 변환할 간단한 함수를 직접 작성 하는 것이 좋습니다.

이외에 `DrawCircle` 메서드를 `SKCanvas` 도 두 개의 정의 `DrawOval` 타원을 그릴 메서드. 타원 단일 radius 보다는 두 개의 반지름으로 정의 됩니다. 이 라고는 *주요 radius* 및 *부 radius*합니다. `DrawOval` 메서드는 두 개의 반지름으로 평행 X 및 Y 축 하 합니다. 변환 또는 (나중으로 포함 됨)에 대 한 그래픽 경로 사용 하는이 제한이 문제를 해결할 수 있습니다 하지만 [이 `DrawOval` 메서드](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) 두 개의 반지름 인수 이름을 `rx` 및 `ry` 평행 하는 것을 나타내려면 X 및 Y 축:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

표시 화면을 채우는 타원을 그릴 수는? **타원 채우기** 페이지에서는 방법을 합니다. `PaintSurface` 의 이벤트 처리기는 [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) 클래스 뺍니다의 스트로크 너비는 `xRadius` 및 `yRadius` 전체 타원에 맞게 값 및 해당 디스플레이 화면 내에서 간략히 설명 합니다.

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

다음 세 가지 플랫폼에서 실행 중인:

[![](pixels-images/ellipsefill-small.png "화면 크기 페이지의 삼중 스크린샷")](pixels-images/ellipsefill-large.png#lightbox "화면 크기 페이지의 삼중 스크린 샷")

[다른 `DrawOval` 메서드](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/) 에 [ `SGRect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRect/) 사각형의 왼쪽 및 오른쪽 아래 모서리의 X 및 Y 좌표를 기준으로 정의 된 인수입니다. 타원에 사용할 수도 있는지 제안 하는 해당 사각형에 채웁니다는 **타원 채우기** 다음과 같이 페이지:

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

그러나이 하 네 면 타원의 윤곽선의 모든 가장자리를 잘립니다. 모든 조정 하면는 `SKRect` 생성자 인수에 따라는 `strokeWidth` 오른쪽이 이렇게 하려면:

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
