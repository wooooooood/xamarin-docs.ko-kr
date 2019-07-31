---
title: SkiaSharp 색 필터
description: 색 필터를 사용 하 여 변환 또는 테이블을 사용 하 여 색을 변환 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 774E7B55-AEC8-4F12-B657-1C0CEE01AD63
author: davidbritch
ms.author: dabritch
ms.date: 08/28/2018
ms.openlocfilehash: 5aa8b2e85d5a7d547af5333dcaf350025b86cc26
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647692"
---
# <a name="skiasharp-color-filters"></a>SkiaSharp 색 필터

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

색 필터 포스터화와 같은 효과 다른 색 비트맵 (또는 다른 이미지)의 색을 변환할 수 있습니다.

![색 필터 예제](color-filters-images/ColorFiltersExample.png "색 필터 예제")

색 필터를 사용 하려면 설정 합니다 [ `ColorFilter` ](xref:SkiaSharp.SKPaint.ColorFilter) 속성을 `SKPaint` 형식의 개체에 [ `SKColorFilter` ](xref:SkiaSharp.SKColorFilter) 해당 클래스의 정적 메서드 중 하나에서 만든 합니다. 이 문서에는 다음 방법을 보여 줍니다. 

- 색 변환을 사용 하 여 만든 합니다 [ `CreateColorMatrix` ](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*) 메서드.
- 색상표를 사용 하 여 만든 합니다 [ `CreateTable` ](xref:SkiaSharp.SKColorFilter.CreateTable*) 메서드.

## <a name="the-color-transform"></a>색 변환

색 변환에서는 색을 수정 하는 행렬을 사용 합니다. 마찬가지로 대부분의 2D 그래픽 시스템에서 SkiaSharp 사용 하 여 행렬 문서의 iscussed로 좌표로 변환에 대 한 대부분 [ **SkiaSharp 변환 행렬**](../transforms/matrix.md)합니다. 합니다 [ `SKColorFilter` ](xref:SkiaSharp.SKColorFilter) 매트릭스 변환 하지만 RGB 색 매트릭스 변환도 지원 합니다. 어느 정도 익숙하다고 행렬 개념은 이러한 색 변환 이해 하는 데 필요한 합니다. 

색 변환 매트릭스는 4 개의 행과 다섯 개의 열을 차원에 있습니다.

<pre>
| M11 M12 M13 M14 M15 |
| M21 M22 M23 M24 M25 |
| M31 M32 M33 M34 M35 |
| M41 M42 M43 M44 M45 |
</pre>

대상 색 RGB 소스 색 (R, G, B, A) 변환 (R'를 G', B ','). 

행렬 곱에 대 한 준비, 소스 색 5 × 1 행렬으로 변환 됩니다.

<pre>
| R |
| G |
| B |
| A |
| 1 |
</pre>

이러한 R, G, B 및 A 값은 0에서 255 까지의 원래 바이트. 이들은 _되지_ 0 ~ 1 범위에 부동 소수점 값을 정규화 합니다.

추가 셀 번역 요소에 필요합니다. 이 섹션에 설명 된 대로 2 차원 좌표로 변환 하는 3 × 3 행렬을 사용 하는 것과 유사 [ **3-3 하 여 행렬의 이유** ](../transforms/matrix.md#the-reason-for-the-3-by-3-matrix) 행렬을 사용 하 여 변환에 대 한 문서에서 요소를 조정 합니다.

5 × 1 행렬으로 4 × 5 행렬 곱을 제품 4 × 1 매트릭스 변환 된 색으로 사용 됩니다.

<pre>
| M11 M12 M13 M14 M15 |    | R |   | R' |
| M21 M22 M23 M24 M25 |    | G |   | G' |
| M31 M32 M33 M34 M35 |  × | B | = | B' |
| M41 M42 M43 M44 M45 |    | A |   | A' |
                           | 1 |
</pre>

R에 대 한 별도 수식을 같습니다 ', G'를 A 및 B',':

`R' = M11·R + M12·G + M13·B + M14·A + M15` 

`G' = M21·R + M22·G + M23·B + M24·A + M25` 

`B' = M31·R + M32·G + M33·B + M34·A + M35` 

`A' = M41·R + M42·G + M43·B + M44·A + M45` 

행렬의 대부분 일반적으로 0 ~ 2 범위에 있는 곱하기 요소 이루어져 있습니다. 그러나 마지막 열 (M45 통해 M15) 수식에 추가 되는 값을 포함 합니다. 이러한 값 일반적으로 범위는 0에서 255입니다. 결과 0에서 255 사이의 범위로 제한 합니다.

항등 매트릭스 다음과 같습니다.

<pre>
| 1 0 0 0 0 |
| 0 1 0 0 0 |
| 0 0 1 0 0 |
| 0 0 0 1 0 |
</pre>

이렇게 하면 색 변경 되지 않았습니다. 변환 하는 수식을 다음과 같습니다.

`R' = R` 

`G' = G`

`B' = B`

`A' = A`

불투명도 보존 되므로 M44 셀이 매우 중요 합니다. 불투명 빨강, 녹색 및 파랑 값을 기반으로 하는 원치 않을 것 때문 M41, M42, 및 M43 모든 0는 경우 일반적으로입니다. 그러나 M44이 0 이면는 ' 0이 되 고 아무 것도 표시 됩니다.

색 매트릭스를 사용 하는 가장 일반적인 하나 회색조 비트맵을 색 비트맵으로 변환 하는 것입니다. 여기에 빨간색, 녹색 및 파란색 값의 가중치가 적용 된 평균에 대 한 수식을 포함 됩니다. ("표준 빨강 녹색 파란색") sRGB 색 공간을 사용 하는 비디오 디스플레이이 수식은 다음과 같습니다.

회색 음영 0.2126· = R + 0.7152· G + 0.0722· B

R 회색조 비트맵을 색 비트맵으로 변환 하려면 ', G', B' 결과 모두 같아야 동일한 값 및 합니다. 행렬은:

<pre>
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0    0    0    1 0 |
</pre>

이 행렬에 해당 하는 SkiaSharp 데이터 형식은 있습니다. 20 배열로 행렬을 나타내야 합니다 대신 `float` 행 순서로 값: 첫 번째 행을 두 번째 행 및 등입니다.

정적 [ `SKColorFilter.CreateColorMatrix` ](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*) 메서드는 다음 구문을 가집니다.

```csharp
public static SKColorFilter CreateColorMatrix (float[] matrix);
```

여기서 `matrix` 20의 배열이 `float` 값입니다. 배열을 만들 때 C#을 4 × 5 행렬 비슷합니다 있으므로 숫자 형식 지정 하기가 쉽습니다. 에 설명 되어이 **회색조 행렬** 페이지에 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플:

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

`DrawBitmap` 에서이 코드에 사용 하는 메서드는 합니다 **BitmapExtension.cs** 에 포함 된 파일을 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플. 

IOS, Android 및 유니버설 Windows 플랫폼에서 실행 되는 결과 다음과 같습니다.

[![회색조 행렬](color-filters-images/GrayScaleMatrix.png "회색조 행렬")](color-filters-images/GrayScaleMatrix-Large.png#lightbox)

네 번째 행과 네 번째 열 값에 주의 합니다. A에 대 한 원래 색의 값을 곱하고 중요 한 요소는 ' 변환 된 색의 값입니다. 해당 셀이 0 이면 아무 것도 표시 되 고 문제를 찾기 어려울 수 있습니다.

색 매트릭스를 사용 하 여 시험해 보려는 경우에 원본 큐브 또는 대상의 관점에서 변환을 처리할 수 있습니다. 원본의 빨간색 픽셀 대상의 빨강, 녹색 및 파랑 픽셀에 어떻게 기여 해야? 첫 번째 값을 기준으로 결정 됩니다 _열_ 행렬입니다. 또는 해야 빨간색 대상 픽셀 영향 원본의 빨간색, 녹색 및 파란색 픽셀로? 첫 번째 결정 됩니다 _행_ 행렬입니다.

아이디어 색 변환을 사용 하는 방법에 대해서는 [ **이미지 다시 칠하기** ](https://docs.microsoft.com/dotnet/framework/winforms/advanced/recoloring-images) 페이지입니다. 초점은 Windows Forms 및 행렬은 다른 형식으로 하지만 개념은 동일 합니다.

합니다 **파스텔 행렬** 빨간색 대상 픽셀 attenuating 빨간색 원본 픽셀 및 약간 빨간색 및 녹색 픽셀을 강조 하 여 계산 합니다. 이 프로세스는 녹색, 파란색 픽셀 마찬가지로 발생합니다.

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

여기서 볼 수 있듯이 색의 강도 음소거 하려면, 결과:

[![파스텔 행렬](color-filters-images/PastelMatrix.png "파스텔 행렬")](color-filters-images/PastelMatrix-Large.png#lightbox)

## <a name="color-tables"></a>색 테이블

정적 [ `SKColorFilter.CreateTable` ](xref:SkiaSharp.SKColorFilter.CreateTable*) 메서드는 두 가지 버전으로 제공 됩니다.

```csharp
public static SKColorFilter CreateTable (byte[] table);

public static SKColorFilter CreateTable (byte[] tableA, byte[] tableR, byte[] tableG, byte[] tableB);
```

배열에는 항상 256 항목 포함. 에 `CreateTable` 빨간색, 녹색 및 파랑 구성 요소에 대 한 테이블을 동일한 테이블을 사용 하 여 메서드를 사용 합니다. 간단한 조회 테이블입니다. 원본 색이 (r, G, b)이 고 대상 색이 (r ', B ', G ') 인 경우 원본 구성 요소를 사용 하 여 인덱싱을 `table` 통해 대상 구성 요소를 가져옵니다.

`R' = table[R]`

`G' = table[G]`

`B' = table[B]`

두 번째 방법에서는 각각 네 가지 색 구성 요소는 별도 색상표를 가질 수 또는 동일한 색 테이블은 둘 이상의 구성 요소 간에 공유 될 수 있습니다.

두 번째 인수 중 하나를 설정 하려는 경우 `CreateTable` 순서 대로 0에서 255 사이의 값이 들어 있는 색상표를 메서드를 사용할 수 `null` 대신 합니다. 매우 자주 합니다 `CreateTable` 호출에는 `null` 알파 채널에 대 한 첫 번째 인수입니다.

섹션에서 **포스터화** 에서 문서의 [SkiaSharp 액세스 비트맵 픽셀 비트](../bitmaps/pixel-bits.md#posterization), 색 해상도 낮추면에 비트맵의 각 픽셀 비트를 수정 하는 방법을 알아보았습니다. 이것이 기법 _포스터화_합니다. 

색상표를 사용 하 여 비트맵을 포스터 수도 있습니다. 생성자는 **포스터 테이블** 페이지에 매핑되는 해당 항목이 있는 인덱스 아래쪽 바이트 0으로 설정 하는 6 비트 색 테이블을 만듭니다.

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

프로그램의 녹색 및 파란색 채널에만이 색상표를 사용 하려면 선택 합니다. 빨간색 채널 전체 해상도 계속 합니다.

[![테이블 포스터](color-filters-images/PosterizeTable.png "테이블 포스터")](color-filters-images/PosterizeTable-Large.png#lightbox)

다양 한 효과 대 한 다른 색 채널에 대 한 다양 한 색 테이블을 사용할 수 있습니다. 

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
