---
title: 분할 된 SkiaSharp 비트맵 표시
description: 일부 영역이 늘어나고 일부 영역은 표시 되지 않도록 SkiaSharp 비트맵을 표시 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 79AE2033-C41C-4447-95A6-76D22E913D19
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5c3909271580d0568d7c603de0d434ff5b3f3bc4
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138673"
---
# <a name="segmented-display-of-skiasharp-bitmaps"></a>분할 된 SkiaSharp 비트맵 표시

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp `SKCanvas` 개체는 라는 메서드와 `DrawBitmapNinePatch` 매우 유사한 라는 두 개의 메서드를 정의 합니다 `DrawBitmapLattice` . 이러한 두 메서드는 모두 비트맵을 대상 사각형의 크기로 렌더링 하지만 비트맵을 균일 하 게 스트레치 하는 대신 비트맵의 일부를 픽셀 치수로 표시 하 고 비트맵의 다른 부분을 확장 하 여 사각형에 맞게 합니다.

![분할 샘플](segmented-images/SegmentedSample.png "분할 샘플")

이러한 메서드는 일반적으로 단추와 같은 사용자 인터페이스 개체의 일부를 구성 하는 비트맵을 렌더링 하는 데 사용 됩니다. 단추를 디자인할 때 일반적으로 단추의 내용에 따라 단추의 크기를 조정 하는 것이 좋습니다. 단추 내용에 관계 없이 단추의 테두리 너비가 동일한 것이 좋습니다. 이는의 이상적인 응용 프로그램입니다 `DrawBitmapNinePatch` .

`DrawBitmapNinePatch`는의 특수 한 경우 `DrawBitmapLattice` 이지만를 사용 하 고 이해 하는 두 가지 방법이 더 쉽습니다.

## <a name="the-nine-patch-display"></a>9 개의 패치 표시 

개념적으로 [`DrawBitmapNinePatch`](xref:SkiaSharp.SKCanvas.DrawBitmapNinePatch(SkiaSharp.SKBitmap,SkiaSharp.SKRectI,SkiaSharp.SKRect,SkiaSharp.SKPaint)) 는 비트맵을 9 개의 사각형으로 나눕니다.

![9 개 패치](segmented-images/NinePatch.png "9 개 패치")

네 모퉁이의 사각형은 픽셀 크기로 표시 됩니다. 화살표가 나타내는 것 처럼 비트맵의 가장자리에 있는 다른 영역은 가로 또는 세로 방향으로 대상 사각형의 영역으로 확대 됩니다. 가운데에 있는 사각형은 가로와 세로로 모두 늘어납니다. 

대상 사각형에 해당 픽셀 크기의 네 개 모퉁이를 표시 하는 데 충분 한 공간이 없으면 사용 가능한 크기로 축소 되 고 네 개의 모퉁이가 표시 됩니다.

비트맵을 이러한 9 개의 사각형으로 나누려면 가운데에 사각형을 지정 해야 합니다. 다음은 메서드의 구문입니다 `DrawBitmapNinePatch` .

```csharp
canvas.DrawBitmapNinePatch(bitmap, centerRectangle, destRectangle, paint);
```

가운데 사각형은 비트맵을 기준으로 합니다. `SKRectI`값 (의 정수 버전 `SKRect` )이 고 모든 좌표와 크기는 픽셀 단위입니다. 대상 사각형은 표시 화면을 기준으로 합니다. `paint` 인수는 선택적 요소입니다.

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **9 개 패치 표시** 페이지는 먼저 정적 생성자를 사용 하 여 형식의 공용 정적 속성을 만듭니다 `SKBitmap` .

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

이 문서의 다른 두 페이지는 동일한 비트맵을 사용 합니다. 비트맵은 500 픽셀 사각형 이며, 각각 100 픽셀의 제곱 영역을 차지 하는 25 개의 원 배열로 구성 됩니다.

![원형 그리드](segmented-images/CircleGrid.png "원형 그리드")

프로그램의 인스턴스 생성자는를 `SKCanvasView` 사용 하 여 `PaintSurface` 비트맵을 `DrawBitmapNinePatch` 전체 디스플레이 화면으로 확장 하 여 표시 하는를 만듭니다.

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

`centerRect`사각형은 중심 16 원의 중앙 배열을 포함 합니다. 모서리에 표시 되는 원은 픽셀 차원에 표시 되 고 그에 따라 다른 모든 항목이 늘어납니다.

[![9-패치 표시](segmented-images/NinePatchDisplay.png "9-패치 표시")](segmented-images/NinePatchDisplay-Large.png#lightbox)

UWP 페이지의 너비는 500 픽셀 이므로 상위 및 하위 행을 동일한 크기의 일련의 원으로 표시 합니다. 그렇지 않으면 모퉁이에 있지 않은 모든 원이 타원을 형성 하도록 늘어납니다.

원과 타원의 조합으로 구성 된 개체를 이상한 방식으로 표시 하려면 중심 사각형을 정의 하 여 원의 행과 열이 겹치지 않도록 하십시오.

```csharp
SKRectI centerRect = new SKRectI(150, 150, 350, 350);
```

## <a name="the-lattice-display"></a>격자 표시

두 `DrawBitmapLattice` 메서드는와 유사 `DrawBitmapNinePatch` 하지만 임의의 수의 수평 또는 수직 분할선에 대해 일반화 됩니다. 이러한 분할선은 픽셀에 해당 하는 정수 배열로 정의 됩니다. 

[`DrawBitmapLattice`](xref:SkiaSharp.SKCanvas.DrawBitmapLattice(SkiaSharp.SKBitmap,System.Int32[],System.Int32[],SkiaSharp.SKRect,SkiaSharp.SKPaint))이러한 정수 배열에 대 한 매개 변수가 있는 메서드는 작동 하지 않는 것 같습니다. [`DrawBitmapLattice`](xref:SkiaSharp.SKCanvas.DrawBitmapLattice(SkiaSharp.SKBitmap,SkiaSharp.SKLattice,SkiaSharp.SKRect,SkiaSharp.SKPaint))형식의 매개 변수를 사용 하는 메서드는 `SKLattice` 작동 하며 아래에 표시 된 샘플에서 사용 됩니다.

[`SKLattice`](xref:SkiaSharp.SKLattice)구조는 네 가지 속성을 정의 합니다.

- [`XDivs`](xref:SkiaSharp.SKLattice.XDivs), 정수 배열
- [`YDivs`](xref:SkiaSharp.SKLattice.YDivs), 정수 배열
- [`Flags`](xref:SkiaSharp.SKLattice.Flags), 배열 `SKLatticeFlags` , 열거형 형식
- [`Bounds`](xref:SkiaSharp.SKLattice.Bounds)`Nullable<SKRectI>`비트맵 내에서 선택적 소스 사각형을 지정 하는 형식입니다.

`XDivs`배열은 비트맵의 너비를 세로 구획으로 나눕니다. 첫 번째 스트립은 왼쪽의 픽셀 0에서로 확장 `XDivs[0]` 됩니다. 이 스트립은 픽셀 너비로 렌더링 됩니다. 두 번째 스트립은에서 `XDivs[0]` 로 확장 `XDivs[1]` 되 고가 확장 됩니다. 세 번째 스트립은에서 `XDivs[1]` 로 확장 `XDivs[2]` 되 고 픽셀 너비로 렌더링 됩니다. 마지막 스트립은 배열의 마지막 요소부터 비트맵의 오른쪽 가장자리로 확장 됩니다. 배열에 짝수 개수의 요소가 있으면 픽셀 너비로 표시 됩니다. 그렇지 않으면 확장 됩니다. 총 세로 스트립 수가 배열의 요소 수보다 하나 더 많은 경우

`YDivs`배열이 비슷합니다. 배열의 높이를 가로 줄무늬로 나눕니다.

`XDivs`및 `YDivs` 배열은 모두 비트맵을 사각형으로 나눕니다. 사각형 수는 가로 줄무늬 수와 세로 줄무늬의 수를 곱한 값과 같습니다.

가 중 Ia 설명서에 따라 배열에는 `Flags` 각 사각형에 대 한 요소, 먼저 사각형의 맨 위 행, 두 번째 행 등이 포함 됩니다. `Flags`배열은 형식이 며 [`SKLatticeFlags`](xref:SkiaSharp.SKLatticeFlags) , 열거형은 다음 멤버를 포함 합니다.

- `Default`값 0 인
- `Transparent`값 1

그러나 이러한 플래그는 예상 대로 작동 하지 않으며 무시 하는 것이 가장 좋습니다. 하지만 속성은로 설정 하지 않습니다 `Flags` `null` . `SKLatticeFlags`총 사각형 수를 포함할 수 있는 크기의 배열로 설정 합니다.

**격자 9 패치** 페이지는 `DrawBitmapLattice` 를 모방 하는 데 사용 `DrawBitmapNinePatch` 됩니다. 에서 만든 것과 동일한 비트맵을 사용 합니다 `NinePatchDisplayPage` .

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

`XDivs`및 속성은 모두 `YDivs` 두 개의 정수의 배열로 설정 됩니다. 즉, 픽셀 0부터 픽셀 100 (픽셀 크기로 렌더링 됨), 픽셀 100에서 픽셀 400 (늘이기), 픽셀 500 400에서 픽셀 (픽셀 크기)로 구분 합니다. 및는 `XDivs` 모두 `YDivs` 배열 크기의 총 9 개의 사각형을 정의 `Flags` 합니다. 단지 배열을 만드는 것 만으로 값 배열을 만들 수 `SKLatticeFlags.Default` 있습니다.

표시는 이전 프로그램과 동일 합니다.

[![격자 9-패치](segmented-images/LatticeNinePatch.png "격자 9-패치")](segmented-images/LatticeNinePatch-Large.png#lightbox)

**격자 표시** 페이지는 비트맵을 16 개의 사각형으로 나눕니다.

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

`XDivs`및 `YDivs` 배열은 약간 다르므로 앞의 예제와 같이 표시 되지 않습니다.

[![격자 표시](segmented-images/LatticeDisplay.png "격자 표시")](segmented-images/LatticeDisplay-Large.png#lightbox)

왼쪽의 iOS 및 Android 이미지에서 작은 원으로 픽셀 크기로 렌더링 됩니다. 다른 모든 항목은 늘어납니다.

**격자 표시** 페이지를 사용 하 여 일반화를 `Flags` 쉽게 시험해 볼 수 있습니다 `XDivs` `YDivs` . 특히 또는 배열의 첫 번째 요소를 0으로 설정 하면 어떤 일이 발생 하는지 확인 하는 것이 좋습니다 `XDivs` `YDivs` . 

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
