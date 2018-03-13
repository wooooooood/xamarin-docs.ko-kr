---
title: "단순 원 그리기"
description: "SkiaSharp 그리기를 캔버스 및 그리기를 포함 하 여 기본 사항 알아보기"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 5b09621f1d3a24f8061e5cd6551dd85ce93e36e3
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="drawing-a-simple-circle"></a>단순 원 그리기

_SkiaSharp 그리기를 캔버스 및 그리기를 포함 하 여 기본 사항 알아보기_

그래픽 SkiaSharp를 만드는 등을 사용 하 여 Xamarin.Forms에 그리기의 개념을 소개 하는이 문서는 `SKCanvasView` 개체를 처리 하는 그래픽을 호스팅하는 `PaintSurface` 이벤트를 사용 하는 `SKPaint` 색 및 기타 그리기를 지정 하는 개체 특성입니다.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) 프로그램 이러한 일련의 SkiaSharp 문서에 대 한 모든 샘플 코드를 포함 합니다. 첫 번째 페이지의 이름이 **단순 원을** page 클래스를 호출 하 고 [ `SimpleCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs)합니다. 이 코드는 100 픽셀의 radius와 페이지의 가운데에 원을 그리려면 하는 방법을 보여 줍니다. 원의 개요는 빨간색 이며 원의 내부는 파란색입니다.

![](circle-images/circleexample.png "빨간색으로 윤곽선 처리 파란색 원")

[ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) page 클래스에서 파생 `ContentPage` 두 개가 포함 및 `using` SkiaSharp 네임 스페이스에 대 한 지시문:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

클래스의 다음 생성자를 만듭니다는 [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/) 개체에 대 한 처리기를 연결 하는 [ `PaintSurface` ](https://developer.xamarin.com/api/event/SkiaSharp.Views.Forms.SKCanvasView.PaintSurface/) 이벤트와 설정 된 `SKCanvasView` 페이지의 내용으로 개체:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvasView` 페이지의 전체 콘텐츠 영역을 차지 합니다. 또는 결합할 수 있습니다는 `SKCanvasView` 다른 xamarin.forms `View` 파생 항목, 살펴보겠지만 다른 예제에서입니다.

`PaintSurface` 이벤트 처리기는 모든 드로잉을 수행할 수 있습니다. 이 일반적으로 메서드는 여러 번 프로그램이 실행 되는 동안, 그래픽 표시 되므로 다시 만드는 데 필요한 모든 정보를 보관 해야 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[ `SKPaintSurfaceEventArgs` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs/) 이벤트를 함께 제공 되는 개체에 다음 두 가지 속성이 있습니다.

- [`Info`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info/) 형식의 [`SKImageInfo`](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/)
- [`Surface`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface/) 형식의 [`SKSurface`](https://developer.xamarin.com/api/type/SkiaSharp.SKSurface/)

`SKImageInfo` 구조에 대 한 정보가 그리기 화면 가장 중요 한 점은,는 너비와 높이 (픽셀)에서입니다. `SKSurface` 개체 자체 그리기 화면을 나타냅니다. 이 프로그램에서 그리기 화면은 비디오 디스플레이 있지만 다른 프로그램에는 `SKSurface` 개체에 그리는 SkiaSharp를 사용 하는 비트맵을 나타낼 수도 있습니다.

가장 중요 한 속성 `SKSurface` 은 [ `Canvas` ](https://developer.xamarin.com/api/property/SkiaSharp.SKSurface.Canvas/) 형식의 [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/)합니다. 이 클래스는 그리기 컨텍스트에 실제 그리기를 수행 하는 데 사용 하는 그래픽입니다. `SKCanvas` 개체에서 클리핑 및 그래픽 변형을 포함 된 그래픽 상태를 캡슐화 합니다.

다음의 일반적인 시작은는 `PaintSurface` 이벤트 처리기.

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

[ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/) 메서드는 투명 한 색을 사용 하 여 캔버스를 지웁니다. 오버 로드를 사용 하면 캔버스에 대 한 배경색을 지정할 수 있습니다.

목적은 파란색으로 채워진 빨간색 원을 그리려면 합니다. 특정 그래픽 이미지가 두 가지 색에 포함 되어 있으므로 작업 두 단계에서 작업을 수행 해야 합니다. 첫 번째 단계는 원의 윤곽선을 그립니다. 및 기타 특성 줄의 색을 지정 하려면 만들고 초기화는 [ `SKPaint` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaint/) 개체:

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

[ `Style` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Style/) 속성을 나타냅니다 *획* 에 (이 경우 원 개요) 줄 대신 *채우기* 내부 합니다. 세 멤버는 [ `SKPaintStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaintStyle/) 열거형은 다음과 같습니다.

- [`Fill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Fill/)
- [`Stroke`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Stroke/)
- [`StrokeAndFill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.StrokeAndFill/)

기본값은 `Fill`입니다. 세 번째 옵션을 사용 하 여 선을 스트로크을 동일한 색의 내부를 채웁니다.

설정의 [ `Color` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Color/) 속성 형식의 값을 [ `SKColor` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColor/)합니다. 가져오려는 한 가지 방법은 `SKColor` 는 Xamarin.Forms를 변환 하 여 값이 `Color` 값을 한 `SKColor` 확장 메서드를 사용 하 여 값 [ `ToSKColor` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.Extensions.ToSKColor/p/Xamarin.Forms.Color/)합니다. [ `Extensions` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.Extensions/) 클래스에 `SkiaSharp.Views.Forms` 네임 스페이스는 SkiaSharp 값과 Xamarin.Forms 값 사이의 변환 하는 다른 메서드를 포함 합니다.

[ `StrokeWidth` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeWidth/) 속성 선의 두께 나타냅니다. 여기를 25 픽셀로 설정 됩니다.

사용 하면 `SKPaint` 원 그릴 개체:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

좌표는 디스플레이 화면의 왼쪽 위 모퉁이 기준으로 지정 됩니다. X 좌표는 오른쪽 증가 내려갈 Y 좌표가 증가. 종종 그래픽에 대 한 설명에서는 수학 표기법 (x, y)가 점을 나타내는 데 사용 됩니다. 점 (0, 0)을 디스플레이 화면의 왼쪽 위 모서리 이며 일반적으로 라고는 *원점*합니다.

처음 두 개의 인수 `DrawCircle` 원의 중심의 X 및 Y 좌표를 나타냅니다. 이러한 절반 너비와 높이 원의 중심 표시 화면 중앙에 적용할 표시 화면의 할당 됩니다. 세 번째 인수는 원의 반지름을 지정 하 고 마지막 인수는는 `SKPaint` 개체입니다.

내부를 채우는 원,의 두 속성을 변경할 수 있습니다는 `SKPaint` 개체와 호출 `DrawCircle` 다시 합니다. 이 코드를 가져올 대체 방법을 보여 줍니다는 `SKColor` 의 많은 필드 중 하나에서 값의 [ `SKColors` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColors/) 구조:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
이 이번에는 `DrawCircle` 호출의 새 속성을 사용 하 여 circle 채웁니다는 `SKPaint` 개체입니다.

다음은 iOS, Android 및 유니버설 Windows 플랫폼에서 실행 중인 프로그램입니다.

[![](circle-images/simplecircle-small.png "단순 원을 페이지의 삼중 스크린샷")](circle-images/simplecircle-large.png#lightbox "단순 원을 페이지의 삼중 스크린샷")

프로그램을 직접 실행 하는 경우 전화 또는 시뮬레이터 옆으로 그래픽을 다시 그리면는 참조를 설정할 수 있습니다. 다시 그려야 하는 데 필요한 그래픽 때마다는 `PaintSurface` 이벤트 처리기가 다시 호출 합니다.

`SKPaint` 개체는 그래픽 속성 그리기의 컬렉션 보다 약간 더 합니다. 이러한 개체는 매우 간단 합니다. 다시 사용할 수 있습니다 `SKPaint` 이 프로그램이 수행 하거나 여러 개 만들 수 있습니다 개체 `SKPaint` 속성 그리기의 다양 한 조합에 대 한 개체입니다. 만들고 외부에서 이러한 개체를 초기화할 수는 `PaintSurface` 이벤트 처리기 및 있습니다 수 필드로 저장 page 클래스에 있습니다.

원의 윤곽선의 너비는 25 픽셀 & #x 2014;으로 지정 되어 있지만 또는 1 / 4 원형 & #x 2014;의 반지름에 을 도출할 수를 표시 되며 해당 하는 이유: 파란색 원에서 줄의 너비 절반 크기로 려 지 합니다. 에 대 한 인수는 `DrawCircle` 메서드 원의 추상 지리 좌표를 정의 합니다. 파란색 내부의 해당 차원에 가장 가까운 픽셀의 경우 25 픽셀 너비 개요 기하학적 원 & #x 2014; 분명 하지만 내부 및 외부 절반에 반 합니다.

다음 샘플에는 [xamarin.forms 통합](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) 문서 이러한 부하 분산 방식이 시각적으로 합니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
