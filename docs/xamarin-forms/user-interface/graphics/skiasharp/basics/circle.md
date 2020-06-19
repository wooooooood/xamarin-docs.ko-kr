---
title: SkiaSharp에서 간단한 원 그리기
description: 이 문서에서는 응용 프로그램에서 캔버스 및 그리기 개체를 비롯 한 SkiaSharp drawing의 기본 사항을 설명 Xamarin.Forms 하 고 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fb873102bfb8568b8298a39ea2429fb6c27af175
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137724"
---
# <a name="drawing-a-simple-circle-in-skiasharp"></a>SkiaSharp에서 간단한 원 그리기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Canvas 및 paint 개체를 포함 하 여 SkiaSharp drawing의 기본 사항을 알아봅니다._

이 문서에서는 Xamarin.Forms `SKCanvasView` 그래픽을 호스트 하 고, `PaintSurface` 이벤트를 처리 하 고, 개체를 사용 하 여 `SKPaint` 색 및 기타 그리기 특성을 지정 하는 개체를 만드는 등 SkiaSharp를 사용 하 여 그래픽을 그리는 개념을 소개 합니다.

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 프로그램에는이 일련의 SkiaSharp 아티클에 대 한 모든 샘플 코드가 포함 되어 있습니다. 첫 번째 페이지에는 **간단한 원이** 부여 되 고 page 클래스가 호출 됩니다 [`SimpleCirclePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) . 이 코드에서는 페이지의 중앙에서 반지름이 100 픽셀인 원을 그리는 방법을 보여 줍니다. 원의 윤곽선은 빨간색이 고 원의 내부는 파란색입니다.

![](circle-images/circleexample.png "A blue circle outlined in red")

[`SimpleCircle`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs)페이지 클래스는에서 파생 `ContentPage` 되며 `using` SkiaSharp 네임 스페이스에 대 한 두 개의 지시문을 포함 합니다.

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

클래스의 다음 생성자는 [`SKCanvasView`](xref:SkiaSharp.Views.Forms.SKCanvasView) 개체를 만들고, 이벤트에 대 한 처리기를 연결 하 [`PaintSurface`](xref:SkiaSharp.Views.Forms.SKCanvasView.PaintSurface) 고, 개체를 `SKCanvasView` 페이지의 콘텐츠로 설정 합니다.

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

는 `SKCanvasView` 페이지의 전체 콘텐츠 영역을 차지 합니다. `SKCanvasView` Xamarin.Forms `View` 다른 예제에서 볼 수 있는 것 처럼 다른 파생 항목을와 결합할 수 있습니다.

`PaintSurface`이벤트 처리기는 모든 그리기를 수행 하는 위치입니다. 프로그램이 실행 되는 동안이 메서드를 여러 번 호출할 수 있으므로 그래픽 표시를 다시 만드는 데 필요한 모든 정보를 유지 관리 해야 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[`SKPaintSurfaceEventArgs`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs)이벤트와 함께 제공 되는 개체에는 두 가지 속성이 있습니다.

- [`Info`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info)([`SKImageInfo`](xref:SkiaSharp.SKImageInfo) 형식)
- [`Surface`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface)([`SKSurface`](xref:SkiaSharp.SKSurface) 형식)

구조에는 `SKImageInfo` 그리기 화면에 대 한 정보 (가장 중요 한 것은 너비 및 높이 픽셀)가 포함 됩니다. `SKSurface`개체는 그리기 화면 자체를 나타냅니다. 이 프로그램에서 그리기 화면은 비디오 표시 이지만 다른 프로그램에서 `SKSurface` 개체는 SkiaSharp을 사용 하 여 그릴 때 사용 하는 비트맵을 나타낼 수도 있습니다.

의 가장 중요 한 속성 `SKSurface` 은 [`Canvas`](xref:SkiaSharp.SKSurface.Canvas) 형식입니다 [`SKCanvas`](xref:SkiaSharp.SKCanvas) . 이 클래스는 실제 그리기를 수행 하는 데 사용 하는 그래픽 그리기 컨텍스트입니다. 개체는 그래픽 `SKCanvas` 변환과 클리핑을 포함 하는 그래픽 상태를 캡슐화 합니다.

이벤트 처리기의 일반적인 시작은 `PaintSurface` 다음과 같습니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();
    ...
}

```

[`Clear`](xref:SkiaSharp.SKCanvas.Clear)메서드는 투명 한 색을 사용 하 여 캔버스를 지웁니다. 오버 로드를 사용 하면 캔버스의 배경색을 지정할 수 있습니다.

여기서의 목표는 파란색으로 채워진 빨간색 원을 그리는 것입니다. 이 특정 그래픽 이미지에는 두 가지 색이 포함 되어 있으므로 작업을 두 단계로 수행 해야 합니다. 첫 번째 단계는 원의 윤곽선을 그리는 것입니다. 선의 색 및 기타 특성을 지정 하려면 개체를 만들고 초기화 합니다 [`SKPaint`](xref:SkiaSharp.SKPaint) .

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 25
    };
    ...
}
```

[`Style`](xref:SkiaSharp.SKPaint.Style)속성은 내부를 *채우지* 않고 선 (이 경우 원의 윤곽선)을 *스트로크* 하 려 함을 나타냅니다. 열거형의 세 가지 멤버는 다음과 같습니다 [`SKPaintStyle`](xref:SkiaSharp.SKPaintStyle) .

- [`Fill`](xref:SkiaSharp.SKPaintStyle.Fill)
- [`Stroke`](xref:SkiaSharp.SKPaintStyle.Stroke)
- [`StrokeAndFill`](xref:SkiaSharp.SKPaintStyle.StrokeAndFill)

기본값은 `Fill`입니다. 세 번째 옵션을 사용 하 여 선을 스트로크 하 고 동일한 색을 사용 하 여 내부를 채웁니다.

속성을 [`Color`](xref:SkiaSharp.SKPaint.Color) 형식의 값으로 설정 합니다 [`SKColor`](xref:SkiaSharp.SKColor) . 값을 가져오는 한 가지 방법은 `SKColor` Xamarin.Forms `Color` `SKColor` 확장 메서드를 사용 하 여 값을 값으로 변환 하는 것입니다 [`ToSKColor`](xref:SkiaSharp.Views.Forms.Extensions.ToSKColor*) . [`Extensions`](xref:SkiaSharp.Views.Forms.Extensions)네임 스페이스의 클래스는 `SkiaSharp.Views.Forms` Xamarin.Forms 값과 SkiaSharp 값 사이를 변환 하는 다른 메서드를 포함 합니다.

[`StrokeWidth`](xref:SkiaSharp.SKPaint.StrokeWidth)속성은 선의 두께를 나타냅니다. 여기서는 25 픽셀로 설정 됩니다.

해당 개체를 사용 하 여 `SKPaint` 원을 그립니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

좌표는 표시 표면의 왼쪽 위 모퉁이를 기준으로 지정 됩니다. X 좌표가 오른쪽으로 증가 하 고 Y 좌표가 증가 합니다. 그래픽에 대해 설명 하는 것 처럼 종종 수학적 표기법 (x, y)을 사용 하 여 점을 나타냅니다. 점 (0, 0)은 표시 표면의 왼쪽 위 모퉁이가 며 일반적으로 *원점*이라고 합니다.

의 처음 두 인수는 `DrawCircle` 원 중심의 X 및 Y 좌표를 표시 합니다. 이는 표시 표면의 너비와 높이에 할당 되어 표시 표면의 중심에 원 중심을 배치 합니다. 세 번째 인수는 원의 반경을 지정 하 고 마지막 인수는 `SKPaint` 개체입니다.

원의 내부를 채우도록 개체의 두 속성을 변경 하 `SKPaint` 고을 다시 호출할 수 있습니다 `DrawCircle` . 또한이 코드는 `SKColor` 구조체의 여러 필드 중 하나에서 값을 가져오는 다른 방법을 보여 줍니다 [`SKColors`](xref:SkiaSharp.SKColors) .

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```

이번에는 `DrawCircle` 호출에서 개체의 새 속성을 사용 하 여 원을 채웁니다 `SKPaint` .

IOS 및 Android에서 실행 되는 프로그램은 다음과 같습니다.

[![](circle-images/simplecircle-small.png "Triple screenshot of the Simple Circle page")](circle-images/simplecircle-large.png#lightbox "Triple screenshot of the Simple Circle page")

프로그램을 직접 실행 하는 경우 휴대폰 또는 시뮬레이터를 사용 하 여 그래픽을 다시 그리는 방법을 볼 수 있습니다. 그래픽을 다시 그려야 할 때마다 `PaintSurface` 이벤트 처리기가 다시 호출 됩니다.

또한 그라데이션 또는 비트맵 타일이 포함 된 그래픽 개체를 색으로 지정할 수 있습니다. 이러한 옵션은 [**SkiaSharp 셰이더**](../effects/shaders/index.md)의 섹션에서 설명 합니다.

`SKPaint`개체는 그래픽 그리기 속성의 컬렉션 보다 약간 더 적습니다. 이러한 개체는 간단 합니다. `SKPaint`이 프로그램에서 사용 하는 개체를 다시 사용 하거나 `SKPaint` 다양 한 그리기 속성 조합에 대 한 여러 개체를 만들 수 있습니다. 이벤트 처리기 외부에서 이러한 개체를 만들고 초기화할 수 있으며,이 개체를 `PaintSurface` 페이지 클래스에 필드로 저장할 수 있습니다.

> [!NOTE]
> `SKPaint`클래스는 [`IsAntialias`](xref:SkiaSharp.SKPaint.IsAntialias) 그래픽의 렌더링에서 앤티앨리어싱을 사용할 수 있도록를 정의 합니다. 앤티앨리어싱을 통해 일반적으로 시각적으로 더 부드러운 가장자리가 생성 되므로 대부분의 개체에서이 속성을로 설정할 수 있습니다 `true` `SKPaint` . 간단한 설명을 위해이 속성은 대부분의 샘플 페이지에서 설정 _되지 않습니다_ .

원 윤곽선의 너비가 25 픽셀 또는 원 반지름의 1/4로 지정 된 경우에도 더 &mdash; &mdash; 가늘게 보이는 것 처럼 보입니다. 선의 너비는 파란색 원으로 가려져 있는 것이 좋습니다. 메서드에 대 한 인수는 `DrawCircle` 원의 추상 기하학적 좌표를 정의 합니다. 파란색 내부는 해당 차원에 가장 가까운 픽셀까지 크기가 지정 되지만 25 픽셀의 윤곽선은 &mdash; 바깥쪽의 안쪽 및 절반에 있는 기하학적 원을 cmio 합니다.

문서 [와 통합 Xamarin.Forms ](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) 하는 다음 샘플에서는이를 시각적으로 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
