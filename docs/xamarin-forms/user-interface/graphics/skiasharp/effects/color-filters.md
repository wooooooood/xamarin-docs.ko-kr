---
title: SkiaSharp 색 필터
description: 색 필터를 사용 하 여 색을 변환 또는 테이블로 변환할 수 있습니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 774E7B55-AEC8-4F12-B657-1C0CEE01AD63
author: davidbritch
ms.author: dabritch
ms.date: 08/28/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b9c89d4d426884d678e77687ffa226cced97be58
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136385"
---
# <a name="skiasharp-color-filters"></a>SkiaSharp 색 필터

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

색 필터는 비트맵 또는 다른 이미지의 색을 posterization 등의 효과에 대 한 다른 색으로 변환할 수 있습니다.

![색 필터 예](color-filters-images/ColorFiltersExample.png "색 필터 예")

색 필터를 사용 하려면 [`ColorFilter`](xref:SkiaSharp.SKPaint.ColorFilter) 의 속성을 `SKPaint` [`SKColorFilter`](xref:SkiaSharp.SKColorFilter) 해당 클래스의 정적 메서드 중 하나에서 만든 형식의 개체로 설정 합니다. 이 문서에서는 다음을 보여 줍니다. 

- 메서드를 사용 하 여 만든 색 변환 [`CreateColorMatrix`](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*) 입니다.
- 메서드를 사용 하 여 만든 색 테이블 [`CreateTable`](xref:SkiaSharp.SKColorFilter.CreateTable*) 입니다.

## <a name="the-color-transform"></a>색 변환입니다.

색 변환은 행렬을 사용 하 여 색을 수정 합니다. 대부분의 2D 그래픽 시스템과 마찬가지로 SkiaSharp는 대개 [**SkiaSharp의 매트릭스 변환**](../transforms/matrix.md)문서에서 좌표 요소를 iscussed로 변환 하는 데 행렬을 사용 합니다. 는 [`SKColorFilter`](xref:SkiaSharp.SKColorFilter) 행렬 변환도 지원 하지만 행렬에서는 RGB 색을 변환 합니다. 이러한 색 변환을 이해 하려면 행렬 개념에 대 한 지식이 필요 합니다. 

색 변환 매트릭스에는 4 개의 행과 5 개의 열로 이루어진 차원이 있습니다.

<pre>
| M11 M12 M13 M14 M15 |
| M21 M22 M23 M24 M25 |
| M31 M32 M33 M34 M35 |
| M41 M42 M43 M44 M45 |
</pre>

RGB 원본 색 (R, G, B, A)을 대상 색 (R ', G ', B ', A ')으로 변환 합니다. 

행렬 곱셈을 준비 하는 동안 원본 색은 5 × 1 행렬로 변환 됩니다.

<pre>
| R |
| G |
| B |
| A |
| 1 |
</pre>

이러한 R, G, B 및 값은 0에서 255 사이의 원래 바이트입니다. 0에서 1 사이의 부동 소수점 값으로 정규화 _되지 않습니다_ .

추가 셀은 변환 인수에 필요 합니다. 이는 좌표 점수를 변환 하기 위해 행렬을 사용 하는 문서에서 3 [**x 3 행렬의 이유**](../transforms/matrix.md#the-reason-for-the-3-by-3-matrix) 섹션에 설명 된 대로 3 × 3 매트릭스를 사용 하 여 2 차원 좌표 요소를 변환 하는 것과 유사 합니다.

4 × 5 행렬에는 5 × 1 매트릭스가 곱하고, 제품은 변환 된 색이 있는 4 × 1 행렬입니다.

<pre>
| M11 M12 M13 M14 M15 |    | R |   | R' |
| M21 M22 M23 M24 M25 |    | G |   | G' |
| M31 M32 M33 M34 M35 |  × | B | = | B' |
| M41 M42 M43 M44 M45 |    | A |   | A' |
                           | 1 |
</pre>

R ', G ', B ' 및 A '에 대 한 별도의 수식은 다음과 같습니다.

`R' = M11·R + M12·G + M13·B + M14·A + M15` 

`G' = M21·R + M22·G + M23·B + M24·A + M25` 

`B' = M31·R + M32·G + M33·B + M34·A + M35` 

`A' = M41·R + M42·G + M43·B + M44·A + M45` 

대부분의 행렬은 일반적으로 0에서 2 사이에 있는 곱하기 요소로 구성 됩니다. 그러나 마지막 열 (M15 ~ M45)에는 수식에 추가 된 값이 포함 되어 있습니다. 이러한 값의 범위는 일반적으로 0에서 255입니다. 결과는 0에서 255 사이의 값으로 고정 됩니다.

Id 매트릭스는 다음과 같습니다.

<pre>
| 1 0 0 0 0 |
| 0 1 0 0 0 |
| 0 0 1 0 0 |
| 0 0 0 1 0 |
</pre>

이로 인해 색이 변경 되지 않습니다. 변환 수식은 다음과 같습니다.

`R' = R` 

`G' = G`

`B' = B`

`A' = A`

M44 셀은 불투명도를 유지 하므로 매우 중요 합니다. 일반적으로 M41, M42 및 M43가 모두 0 인 경우에는 빨강, 녹색 및 파랑 값을 기준으로 불투명도를 사용 하지 않는 것이 좋습니다. 그러나 M44가 0 이면는 0이 되 고 아무 것도 표시 되지 않습니다.

색 매트릭스를 사용 하는 가장 일반적인 방법 중 하나는 색 비트맵을 회색 눈금 비트맵으로 변환 하는 것입니다. 여기에는 빨강, 녹색 및 파랑 값의 가중치가 적용 된 평균에 대 한 수식이 포함 됩니다. SRGB ("표준 빨강 녹색 파랑") 색 공간을 사용 하 여 비디오를 표시 하는 경우이 수식은 다음과 같습니다.

회색 음영 = 0.2126 · R + 0.7152 · G + 0.0722 · B

색 비트맵을 회색 눈금 비트맵으로 변환 하려면 R ', G ' 및 B의 결과가 모두 같은 값 이어야 합니다. 행렬은 다음과 같습니다.

<pre>
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0    0    0    1 0 |
</pre>

이 행렬에 해당 하는 SkiaSharp 데이터 형식이 없습니다. 대신 행렬을 `float` 행 순서에 있는 20 개의 값, 즉 첫 번째 행, 두 번째 행 등의 배열로 나타내야 합니다.

정적 [`SKColorFilter.CreateColorMatrix`](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*) 메서드의 구문은 다음과 같습니다.

```csharp
public static SKColorFilter CreateColorMatrix (float[] matrix);
```

여기서 `matrix` 는 20 값의 배열입니다 `float` . C #에서 배열을 만들 때 4 × 5 매트릭스와 유사 하 게 숫자의 형식을 지정 하는 것이 쉽습니다. 이는 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플의 **회색 크기 행렬** 페이지에서 보여 줍니다.

```csharp
public class GrayScaleMatrixPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(CenteredTilesPage),
                        "SkiaSharpFormsDemos.Media.Banana.jpg");

    public GrayScaleMatrixPage()
    {
        Title = "Gray-Scale Matrix";

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

        using (SKPaint paint = new SKPaint())
        {
            paint.ColorFilter =
                SKColorFilter.CreateColorMatrix(new float[]
                {
                    0.21f, 0.72f, 0.07f, 0, 0,
                    0.21f, 0.72f, 0.07f, 0, 0,
                    0.21f, 0.72f, 0.07f, 0, 0,
                    0,     0,     0,     1, 0
                });

            canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

`DrawBitmap`이 코드에 사용 되는 메서드는 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플에 포함 된 **BitmapExtension.cs** 파일에서 가져온 것입니다. 

IOS, Android 및 유니버설 Windows 플랫폼에서 실행 되는 결과는 다음과 같습니다.

[![회색 크기 행렬](color-filters-images/GrayScaleMatrix.png "회색 크기 행렬")](color-filters-images/GrayScaleMatrix-Large.png#lightbox)

네 번째 행과 네 번째 열에 있는 값을 확인 합니다. 변환 된 색의 A ' 값에 대 한 원래 색의 값을 곱하여 하는 중요 한 요소입니다. 해당 셀이 0 이면 아무것도 표시 되지 않으며 문제를 찾기 어려울 수 있습니다.

색 매트릭스를 사용 하 여 실험 하는 경우 원본 또는 대상의 큐브 뷰에서 변환을 처리할 수 있습니다. 원본의 빨간색 픽셀이 대상의 빨간색, 녹색 및 파란색 픽셀에 어떻게 기여 하나요? 행렬의 첫 번째 _열_ 에 있는 값에 따라 결정 됩니다. 또는 대상 빨강 픽셀의 영향을 받는 빨간색, 녹색 및 파란색 픽셀의 영향을 받습니까? 이는 행렬의 첫 번째 _행_ 에 의해 결정 됩니다.

색 변환을 사용 하는 방법에 대 한 몇 가지 아이디어는 [**이미지 다시 칠하기**](https://docs.microsoft.com/dotnet/framework/winforms/advanced/recoloring-images) 페이지를 참조 하세요. 논의에는 Windows Forms, 행렬은 다른 형식 이지만 개념은 동일 합니다.

**파스텔 행렬** 은 원본 빨강 픽셀을 attenuating 빨강 및 녹색 픽셀을 약간 강조 하 여 대상 빨강 픽셀을 계산 합니다. 이 프로세스는 다음과 유사 하 게 녹색 및 파란색 픽셀에 대해 발생 합니다.

```csharp
public class PastelMatrixPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(PastelMatrixPage),
                        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    public PastelMatrixPage()
    {
        Title = "Pastel Matrix";

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

        using (SKPaint paint = new SKPaint())
        {
            paint.ColorFilter =
                SKColorFilter.CreateColorMatrix(new float[]
                {
                    0.75f, 0.25f, 0.25f, 0, 0,
                    0.25f, 0.75f, 0.25f, 0, 0,
                    0.25f, 0.25f, 0.75f, 0, 0,
                    0, 0, 0, 1, 0
                });

            canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

결과는 여기에서 볼 수 있듯이 색의 강도를 음소거 하는 것입니다.

[![파스텔 행렬](color-filters-images/PastelMatrix.png "파스텔 행렬")](color-filters-images/PastelMatrix-Large.png#lightbox)

## <a name="color-tables"></a>색 테이블

정적 [`SKColorFilter.CreateTable`](xref:SkiaSharp.SKColorFilter.CreateTable*) 메서드는 두 가지 버전으로 제공 됩니다.

```csharp
public static SKColorFilter CreateTable (byte[] table);

public static SKColorFilter CreateTable (byte[] tableA, byte[] tableR, byte[] tableG, byte[] tableB);
```

배열에는 항상 256 항목이 포함 되어 있습니다. `CreateTable`단일 테이블이 있는 메서드에서는 빨강, 녹색 및 파랑 구성 요소에 대해 동일한 테이블이 사용 됩니다. 간단한 조회 테이블입니다. 원본 색이 (R, G, B)이 고 대상 색이 (R ', B ', G ') 인 경우 `table` 원본 구성 요소를 사용 하 여 인덱싱을 통해 대상 구성 요소를 가져옵니다.

`R' = table[R]`

`G' = table[G]`

`B' = table[B]`

두 번째 방법에서는 네 가지 색 구성 요소 각각에 별도의 색 테이블이 있거나 둘 이상의 구성 요소 간에 동일한 색 테이블이 공유 될 수 있습니다.

두 번째 메서드에 대 한 인수 중 하나를 `CreateTable` 시퀀스의 0 ~ 255 값을 포함 하는 색 테이블로 설정 하려면 대신를 사용 하면 `null` 됩니다. 일반적 `CreateTable` 으로 호출에는 `null` 알파 채널에 대 한 첫 번째 인수가 있습니다.

[SkiaSharp 비트맵 픽셀 비트에 액세스](../bitmaps/pixel-bits.md#posterization)하는 문서의 **Posterization** 에 대 한 섹션에서 색 해상도를 줄이기 위해 비트맵의 개별 픽셀 비트를 수정 하는 방법을 살펴보았습니다. 이는 _posterization_이라는 기술입니다. 

색 표를 사용 하 여 비트맵을 포스터화 할 수도 있습니다. **테이블 포스터화** 페이지의 생성자는 아래쪽 6 비트가 0으로 설정 된 바이트에 인덱스를 매핑하는 색 테이블을 만듭니다.

```csharp
public class PosterizeTablePage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(PosterizeTablePage),
                        "SkiaSharpFormsDemos.Media.MonkeyFace.png");

    byte[] colorTable = new byte[256];

    public PosterizeTablePage()
    {
        Title = "Posterize Table";

        // Create color table
        for (int i = 0; i < 256; i++)
        {
            colorTable[i] = (byte)(0xC0 & i);
        }

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

        using (SKPaint paint = new SKPaint())
        {
            paint.ColorFilter =
                SKColorFilter.CreateTable(null, null, colorTable, colorTable);

            canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

이 프로그램은 녹색 채널과 파란색 채널에만이 색 표를 사용 하도록 선택 합니다. 빨간색 채널은 전체 해상도를 계속 유지 합니다.

[![테이블 포스터화](color-filters-images/PosterizeTable.png "테이블 포스터화")](color-filters-images/PosterizeTable-Large.png#lightbox)

다양 한 효과에 대 한 다양 한 색 채널에 다양 한 색 테이블을 사용할 수 있습니다. 

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
