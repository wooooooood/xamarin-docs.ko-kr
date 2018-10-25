---
title: SkiaSharp에서 단순 원 그리기
description: 이 문서는 캔버스 및 그리기 개체를 포함 하 여 Xamarin.Forms 응용 프로그램에서 SkiaSharp 그리기의 기본 사항을 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 32fa126d1990839a98ce03cdbfa245a2df97671d
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615225"
---
# <a name="drawing-a-simple-circle-in-skiasharp"></a>SkiaSharp에서 단순 원 그리기

_개체를 그릴 및 캔버스를 포함 하 여 SkiaSharp 그리기 기본 사항 알아보기_

SkiaSharp, 만드는 등을 사용 하 여 Xamarin.Forms에서 그래픽 그리기의 개념을 소개 하는이 문서는 `SKCanvasView` 처리 하는 그래픽을 호스트 하는 개체를 `PaintSurface` 이벤트를 사용 하 여를 `SKPaint` 색 및 다른 그리기를 지정 하는 개체 특성입니다.

합니다 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 프로그램이 SkiaSharp 문서이 시리즈에 대 한 모든 샘플 코드를 포함 합니다. 첫 번째 페이지 자격이 **단순 원** page 클래스를 호출 하 고 [ `SimpleCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs)합니다. 이 코드는 100 픽셀의 radius 사용 하 여 페이지의 가운데에 원을 그리는 방법을 보여 줍니다. 원 윤곽선은 빨간색이 고 원의 내부는 파란색입니다.

![](circle-images/circleexample.png "빨간색 윤곽선이 있는 파란색 원")

합니다 [ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) page 클래스에서 파생 `ContentPage` 에 두 개의 및 `using` SkiaSharp 네임 스페이스에 대 한 지시문:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

클래스의 다음 생성자를 만듭니다는 [ `SKCanvasView` ](xref:SkiaSharp.Views.Forms.SKCanvasView) 개체를 위한 처리기를 연결 합니다 [ `PaintSurface` ](xref:SkiaSharp.Views.Forms.SKCanvasView.PaintSurface) 이벤트 집합과 `SKCanvasView` 페이지의 내용으로 개체:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvasView` 페이지의 전체 콘텐츠 영역을 차지 합니다. 또는 결합할 수 있습니다는 `SKCanvasView` 다른 Xamarin.Forms를 사용 하 여 `View` 파생형 살펴보겠지만 다른 예제에서입니다.

`PaintSurface` 이벤트 처리기가 모든 드로잉을 수행할 수 있습니다. 이 메서드 여러 번 프로그램이 실행 되는 동안, 그래픽 표시 되므로 다시 만드는 데 필요한 모든 정보를 보관 해야 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

합니다 [ `SKPaintSurfaceEventArgs` ](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs) 이벤트와 함께 제공 되는 개체에 두 개의 속성이 있습니다.

- [`Info`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info) 형식의 [`SKImageInfo`](xref:SkiaSharp.SKImageInfo)
- [`Surface`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface) 형식의 [`SKSurface`](xref:SkiaSharp.SKSurface)

`SKImageInfo` 구조에 대 한 정보가 그리기 화면에 가장 중요 한 점은 너비와 높이 (픽셀)입니다. `SKSurface` 개체 자체 그리기 화면을 나타냅니다. 이 프로그램을 그리기 화면의 비디오 디스플레이 있지만 다른 프로그램에는 `SKSurface` 개체에 그릴 SkiaSharp를 사용 하는 비트맵을 나타낼 수도 있습니다.

가장 중요 한 속성을 `SKSurface` 됩니다 [ `Canvas` ](xref:SkiaSharp.SKSurface.Canvas) 형식의 [ `SKCanvas` ](xref:SkiaSharp.SKCanvas)합니다. 이 클래스는 그리기 컨텍스트에 실제 그리기를 수행 하는 데 사용 하는 그래픽입니다. `SKCanvas` 개체 그래픽 변형과 클리핑 포함 된 그래픽 상태를 캡슐화 합니다.

다음은 일반적인 시작을 `PaintSurface` 이벤트 처리기:

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

합니다 [ `Clear` ](xref:SkiaSharp.SKCanvas.Clear) 메서드 투명 한 색을 사용 하 여 캔버스를 지웁니다. 오버 로드를 사용 하면 캔버스에 대 한 배경색을 지정할 수 있습니다.

여기서 목표는 파란색으로 채워진 빨간색 원을 그리려면입니다. 이 특정 그래픽 이미지의 두 가지 색상 들어 있으므로 작업 두 단계로 수행 해야 합니다. 첫 번째 단계는 원 윤곽선을 그립니다. 색 및 줄의 다른 특성을 지정 하려면 생성 및 초기화 된 [ `SKPaint` ](xref:SkiaSharp.SKPaint) 개체:

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

합니다 [ `Style` ](xref:SkiaSharp.SKPaint.Style) 속성을 원하는 지 나타냅니다 *스트로크* 줄 (이 경우 원 윤곽선) 대신 *채우기* 내부. 세 멤버를 [ `SKPaintStyle` ](xref:SkiaSharp.SKPaintStyle) 열거형은 다음과 같습니다.

- [`Fill`](xref:SkiaSharp.SKPaintStyle.Fill)
- [`Stroke`](xref:SkiaSharp.SKPaintStyle.Stroke)
- [`StrokeAndFill`](xref:SkiaSharp.SKPaintStyle.StrokeAndFill)

기본값은 `Fill`입니다. 선을 스트로크 및 동일한 색을 사용 하 여 내부를 채우는 세 번째 옵션을 사용 합니다.

설정 된 [ `Color` ](xref:SkiaSharp.SKPaint.Color) 속성 형식의 값을 [ `SKColor` ](xref:SkiaSharp.SKColor)합니다. 가져올 수는 `SKColor` 는 Xamarin.Forms를 변환 하 여 값이 `Color` 값을 `SKColor` 확장 메서드를 사용 하 여 값 [ `ToSKColor` ](SkiaSharp.Views.Forms.Extensions.ToSKColor*). 합니다 [ `Extensions` ](xref:SkiaSharp.Views.Forms.Extensions) 클래스는 `SkiaSharp.Views.Forms` 네임 스페이스는 Xamarin.Forms 값과 SkiaSharp 값 간에 변환 하는 다른 메서드를 포함 합니다.

합니다 [ `StrokeWidth` ](xref:SkiaSharp.SKPaint.StrokeWidth) 속성 선의 두께 나타냅니다. 여기에 25 픽셀에 설정 됩니다.

사용할 `SKPaint` 원을 그릴 개체:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

디스플레이 화면의 왼쪽 위 모퉁이 기준으로 좌표를 지정 합니다. X를 오른쪽으로 증가 및 Y 좌표가 증가 다운 조정 합니다. 종종 그래픽에 대 한 자세한 내용은 수학 표기법 (x, y)가 시점을 나타내는 데 사용 됩니다. 점 (0, 0)을 디스플레이 화면의 왼쪽 위 모퉁이 이며 라고 합니다 *원본*합니다.

처음 두 인수 `DrawCircle` 원의 중심의 X 및 Y 좌표를 나타냅니다. 이러한 원의 중심을 디스플레이 화면의 가운데에 삽입할 너비와 높이 디스플레이 화면의 절반에 할당 됩니다. 세 번째 인수는 원의 반지름을 지정 하 고 마지막 인수는는 `SKPaint` 개체입니다.

내부를 채우는 서클의,의 두 가지 속성을 변경할 수 있습니다 합니다 `SKPaint` 개체와 호출 `DrawCircle` 다시 합니다. 이 코드에는 또한 가져오기 하는 대체 방법을 보여 줍니다는 `SKColor` 의 여러 필드 중 하나에서 값을 [ `SKColors` ](xref:SkiaSharp.SKColors) 구조:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
이 이번에는 `DrawCircle` 호출의 새 속성을 사용 하 여 원을 채웁니다는 `SKPaint` 개체입니다.

IOS, Android 및 유니버설 Windows 플랫폼에서 실행 중인 프로그램이 다음과 같습니다.

[![](circle-images/simplecircle-small.png "단순 원 페이지 스크린샷 삼중")](circle-images/simplecircle-large.png#lightbox "삼중 단순 원 페이지 스크린샷")

프로그램을 직접 실행 하는 경우 휴대폰 또는 그래픽을 다시 그리면 하는 방법을 보려면 옆으로 돌려 시뮬레이터를 설정할 수 있습니다. 그래픽을 다시 그려야 할 때마다는 `PaintSurface` 이벤트 처리기가 다시 호출 합니다.

그라데이션 또는 타일 비트맵 그래픽 개체에 색 수 이기도 합니다. 이러한 옵션에 섹션에서 설명 됩니다 [ **SkiaSharp 셰이더**](../effects/shaders/index.md)합니다.

`SKPaint` 개체 그래픽 그리기 속성의 컬렉션 보다 약간 더 합니다. 이러한 개체는 간단 합니다. 다시 사용할 수 있습니다 `SKPaint` 이 프로그램은, 또는 여러 개 만들 수 있습니다 개체 `SKPaint` 속성 그리기의 다양 한 조합에 대 한 개체입니다. 만들고 외부에서 이러한 개체를 초기화할 수는 `PaintSurface` 고 이벤트 처리기에에서 저장할 수 필드로 page 클래스.

> [!NOTE]
> 합니다 `SKPaint` 클래스 정의 [ `IsAntialias` ](xref:SkiaSharp.SKPaint.IsAntialias) 앤티 앨리어싱을 그래픽 렌더링에 사용할 수 있도록 합니다. 앤티 앨리어싱을 시각적으로 부드러운 가장자리에서이 속성을 설정 하 고 싶을 하므로 일반적으로 발생 `true` 대부분의 프로그램 `SKPaint` 개체입니다. 에서는 편의 위해가이 속성은 _되지_ 대부분의 샘플 페이지에서 설정 합니다.

원 윤곽선의 너비를 각각 25 픽셀인으로 지정 하지만 &mdash; 또는 원의 반지름의 1/4 &mdash; 얇 것 이며에 이유가: 파란색 원에서 줄의 너비 절반가 려 지 합니다. 에 대 한 인수는 `DrawCircle` 메서드 원의 추상 지리 좌표를 정의 합니다. 가장 가까운 픽셀에 해당 차원에 파란색 내부 크기가 있지만 25 픽셀 너비 윤곽선 포괄 기하학적 원 &mdash; 내부 및 외부의 절반에 반 합니다.

다음 샘플은 [Xamarin.Forms를 사용 하 여 통합](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) 문서 시각적으로이에서는 합니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
