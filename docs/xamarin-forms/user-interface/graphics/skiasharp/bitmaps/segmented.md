---
title: SkiaSharp 비트맵의 분할된 된 표시
description: 일부 영역 확장 하 고 일부 영역은 없습니다 있도록 SkiaSharp 비트맵을 표시 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 79AE2033-C41C-4447-95A6-76D22E913D19
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: 71997acde4545fec801dfdc8147ab1a9ace7ab24
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119230"
---
# <a name="segmented-display-of-skiasharp-bitmaps"></a>SkiaSharp 비트맵의 분할된 된 표시

SkiaSharp `SKCanvas` 이라는 메서드를 정의 하는 개체 `DrawBitmapNinePatch` 및 이라는 두 가지 방법 `DrawBitmapLattice` 는 매우 비슷합니다. 둘 다 이러한 메서드는 대상 사각형의 크기를 비트맵을 렌더링 하지만 비트맵을 균일 하 게 확장 하는 대신 해당 픽셀 크기의 비트맵의 일부를 표시 하며 사각형 맞도록 비트맵의 다른 부분 확장:

![샘플 분할](segmented-images/SegmentedSample.png "샘플 분할")

이러한 메서드는 일반적으로 단추와 같은 사용자 인터페이스 개체의 일부가 되 비트맵을 렌더링 하는 데 사용 됩니다. 단추를 디자인할 때 일반적으로 단추의 콘텐츠를 기반으로 하는 단추의 크기 싶지만 단추의 테두리 단추의 내용에 관계 없이 동일한 너비를 할 수 있습니다. 이상적인 응용 프로그램에 `DrawBitmapNinePatch`합니다.

`DrawBitmapNinePatch` 특수 한 경우 `DrawBitmapLattice` 이지만 더 쉽게 이해 하 고 사용할 두 가지 방법 중입니다.

## <a name="the-nine-patch-display"></a>9-패치 표시 

개념상 [ `DrawBitmapNinePatch` ](xref:SkiaSharp.SKCanvas.DrawBitmapNinePatch(SkiaSharp.SKBitmap,SkiaSharp.SKRectI,SkiaSharp.SKRect,SkiaSharp.SKPaint)) 9 사각형으로 비트맵을 분할 합니다.

![9 명의 패치](segmented-images/NinePatch.png "9 패치")

4 개의 모퉁이에 사각형의 픽셀 크기로 표시 됩니다. 화살표를 나타내는 비트맵의 가장자리에서 다른 영역의 대상 사각형의 영역을 가로 또는 세로로 확장 됩니다. 가로 및 세로로 가운데에 사각형이 늘어납니다. 

대상 사각형의 픽셀 크기의 네 모퉁이 표시할에 충분 한 공간이 없는 경우 사용 가능한 크기를 nothing을 축소 하지만 네 모퉁이 표시 하는 것입니다.

이러한 9 개의 사각형으로 비트맵을 나누는 데은 가운데에 사각형을 지정 하는 데 필요한만 있습니다. 구문은 `DrawBitmapNinePatch` 메서드:

```csharp
canvas.DrawBitmapNinePatch(bitmap, centerRectangle, destRectangle, paint);
```

가운데 사각형 비트맵에 상대적입니다. 것을 `SKRectI` 값 (정수 버전 `SKRect`) 모든 좌표 및 크기는 픽셀 단위의입니다. 대상 사각형을 화면에 상대적입니다. `paint` 인수는 선택적 요소입니다.

합니다 **9 패치 표시** 페이지에 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 샘플 형식의 공용 정적 속성을 만들려면 먼저 정적 생성자를 사용 `SKBitmap`:

```csharp
public partial class NinePatchDisplayPage : ContentPage
{
    static NinePatchDisplayPage()
    {
        using (SKCanvas canvas = new SKCanvas(FiveByFiveBitmap))
        using (SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 10
        })
        {
            for (int x = 50; x < 500; x += 100)
                for (int y = 50; y < 500; y += 100)
                {
                    canvas.DrawCircle(x, y, 40, paint);
                }
        }
    }

    public static SKBitmap FiveByFiveBitmap { get; } = new SKBitmap(500, 500);
    ···
}
```

이 문서의 다른 두 페이지는 같은 비트맵을 사용합니다. 비트맵 500 픽셀 정사각형 하며 구성 되어 25 원의 배열 모두 동일한 크기, 각 100 픽셀 사각형 영역을 차지 합니다.

![표 원](segmented-images/CircleGrid.png "표 원")

프로그램의 인스턴스 생성자를 만듭니다는 `SKCanvasView` 사용 하 여는 `PaintSurface` 사용 하는 처리기 `DrawBitmapNinePatch` 해당 전체 화면을 채우도록 확장 비트맵을 표시 하려면:

```csharp
public class NinePatchDisplayPage : ContentPage
{
    ···
    public NinePatchDisplayPage()
    {
        Title = "Nine-Patch Display";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRectI centerRect = new SKRectI(100, 100, 400, 400);
        canvas.DrawBitmapNinePatch(FiveByFiveBitmap, centerRect, info.Rect);
    }
}
```

`centerRect` 사각형 16 원의 중앙 배열을 포함 합니다. 모퉁이에 있는 원을 해당 픽셀 크기, 표시 되 고 그에 따라 늘어나는 나머지:

[![9-패치 디스플레이](segmented-images/NinePatchDisplay.png "9 패치 표시")](segmented-images/NinePatchDisplay-Large.png#lightbox)

UWP 페이지 500 픽셀로 발생 하며 따라서 일련의 동일한 크기의 원 위쪽과 아래쪽 행이 표시 됩니다. 이 고, 그렇지 모퉁이에 있지 않은 모든 분야는 폼 줄임표를 채우도록 확장 합니다.

이상 하 게 표시 되는 원과 타원의 조합으로 구성 된 개체의 행과 열을 원 겹치도록 가운데 사각형을 정의 하세요.

```csharp
SKRectI centerRect = new SKRectI(150, 150, 350, 350);
```

## <a name="the-lattice-display"></a>격자 표시

두 `DrawBitmapLattice` 메서드는 비슷합니다 `DrawBitmapNinePatch`, 하지만 모든 가로 또는 세로 구역 수에 대 한 일반화 되어 있습니다. 이러한 부서 픽셀에 해당 하는 정수 배열에서 정의 됩니다. 

합니다 [ `DrawBitmapLattice` ](xref:SkiaSharp.SKCanvas.DrawBitmapLattice(SkiaSharp.SKBitmap,System.Int32[],System.Int32[],SkiaSharp.SKRect,SkiaSharp.SKPaint)) 정수의 이러한 배열에 대 한 메서드 매개 변수를 사용 하 여 작동 하지 않는 것입니다. 합니다 [ `DrawBitmapLattice` ](xref:SkiaSharp.SKCanvas.DrawBitmapLattice(SkiaSharp.SKBitmap,SkiaSharp.SKLattice,SkiaSharp.SKRect,SkiaSharp.SKPaint)) 형식의 매개 변수를 사용 하 여 메서드 `SKLattice` 작동, 아래 샘플에서 사용 하는 것입니다.

합니다 [ `SKLattice` ](xref:SkiaSharp.SKLattice) 구조 네 가지 속성을 정의 합니다.

- [`XDivs`](xref:SkiaSharp.SKLattice.XDivs)정수 배열
- [`YDivs`](xref:SkiaSharp.SKLattice.YDivs)정수 배열
- [`Flags`](xref:SkiaSharp.SKLattice.Flags)에서 배열을 `SKLatticeFlags`, 열거형 형식
- [`Bounds`](xref:SkiaSharp.SKLattice.Bounds) 형식의 `Nullable<SKRectI>` 비트맵 내 선택적 원본 영역을 지정 하려면

`XDivs` 배열 세로 줄무늬 비트맵의 너비를 나눕니다. 첫 번째 줄무늬를 왼쪽에 0 픽셀에서 확장 `XDivs[0]`합니다. 이 줄은 픽셀 너비에 렌더링 됩니다. 두 번째 줄에서 확장 `XDivs[0]` 에 `XDivs[1]`를 채우도록 확장 되 고 합니다. 세 번째 줄에서 확장 `XDivs[1]` 에 `XDivs[2]` 픽셀 너비에서 렌더링 됩니다. 마지막 줄 비트맵의 오른쪽 가장자리에서 배열의 마지막 요소까지 확장 됩니다. 배열의 요소 수는 짝수가 있으면 해당 픽셀 너비에서 표시 됩니다. 그렇지 않으면 해당 연장 됩니다. 세로 줄무늬의 총 수는 하나는 배열의 요소 수를 초과 합니다.

`YDivs` 배열 비슷합니다. 가로 줄무늬를 배열의 높이 나눕니다.

함께 합니다 `XDivs` 및 `YDivs` 배열 사각형으로 비트맵을 나눕니다. 사각형의 수 세로 줄무늬의 수와 가로 스트립의 수를 곱한 하는 것과 같습니다.

Skia 설명서에 따르면는 `Flags` 배열 요소를 포함 한 각 사각형에 대 한 처음 사각형의 맨 위 행을 두 번째 행 등입니다. 합니다 `Flags` 형식의 배열이 [ `SKLatticeFlags` ](xref:SkiaSharp.SKLatticeFlags)를 다음 멤버로 구성 된 열거형:

- `Default` 값이 0 인
- `Transparent` 값이 1 인

그러나 이러한 플래그 것 같지 않습니다. 작업을 올바른와 무시 하는 것이 좋습니다. 설정 하지 않으면 합니다 `Flags` 속성을 `null`입니다. 배열에 설정 `SKLatticeFlags` 값 사각형의 총 수를 포함 하기에 충분 합니다.

합니다 **격자 9 패치** 사용 하 여 페이지 `DrawBitmapLattice` 모방 하기 위해 `DrawBitmapNinePatch`합니다. 만든 동일한 비트맵을 사용 하 여 `NinePatchDisplayPage`:

```csharp
public class LatticeNinePatchPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeNinePatchPage ()
    {
        Title = "Lattice Nine-Patch";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    `
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 400 };
        lattice.YDivs = new int[] { 100, 400 };
        lattice.Flags = new SKLatticeFlags[9]; 

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

모두를 `XDivs` 및 `YDivs` 속성이 가로 세로 방향으로 비트맵 세 줄무늬를 나누는 두 정수의 배열을로 설정 됩니다: 픽셀에서 0 픽셀 (렌더링 픽셀 크기), 100에서 400 픽셀 100 픽셀 (확장) 및 400 픽셀을 선택 하는 픽셀 500 (픽셀 크기). 함께 `XDivs` 하 고 `YDivs` 9 사각형의 크기는 총 정의의 `Flags` 배열입니다. 배열을 만드는 데 충분는 단순히 배열을 만드는 `SKLatticeFlags.Default` 값입니다.

디스플레이 이전 프로그램으로 동일 합니다.

[![9-패치 lattice](segmented-images/LatticeNinePatch.png "격자 9-패치")](segmented-images/LatticeNinePatch-Large.png#lightbox)

합니다 **격자 표시** 페이지 16 사각형으로 비트맵을 분할 합니다.

```csharp
public class LatticeDisplayPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeDisplayPage()
    {
        Title = "Lattice Display";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 200, 400 };
        lattice.YDivs = new int[] { 100, 300, 400 };

        int count = (lattice.XDivs.Length + 1) * (lattice.YDivs.Length + 1);
        lattice.Flags = new SKLatticeFlags[count];

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

합니다 `XDivs` 및 `YDivs` 배열은 다르게 표시 하도록 매우 이전 예제와 대칭을 유발 합니다.

[![표시 격자](segmented-images/LatticeDisplay.png "격자 표시")](segmented-images/LatticeDisplay-Large.png#lightbox)

IOS 및 Android 이미지 왼쪽에 있는 작은 원만 픽셀 크기로 렌더링 됩니다. 기타 등등 연장 됩니다.

**격자 표시** 페이지를 만드는 일반화를 `Flags` 배열에 실험할 수 있습니다 `XDivs` 및 `YDivs` 쉽게 합니다. 첫 번째 요소를 설정 하면 어떻게 되는지 확인 하려는 특히 합니다 `XDivs` 또는 `YDivs` 0으로 배열 합니다. 

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
