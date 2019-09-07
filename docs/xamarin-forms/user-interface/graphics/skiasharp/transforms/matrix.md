---
title: SkiaSharp의 행렬 변환
description: 이 문서에서는 다양 한 변형 매트릭스를 사용 하 여 SkiaSharp 변환 자세히 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: davidbritch
ms.author: dabritch
ms.date: 04/12/2017
ms.openlocfilehash: 6e78e3930ec731bc970ef39ddb7fe7051d62f63a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770436"
---
# <a name="matrix-transforms-in-skiasharp"></a>SkiaSharp의 행렬 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp 변환 다양 한 변환 매트릭스를 사용 하 여 본격적으로 활용 하기_

에 적용 되는 모든 변환 합니다 `SKCanvas` 개체의 단일 인스턴스에서 통합 됩니다 합니다 [ `SKMatrix` ](xref:SkiaSharp.SKMatrix) 구조입니다. 모든 최신 2D 그래픽 시스템에서와 비슷한 표준 3-3에서 변환 행렬입니다.

지금까지 살펴본 대로에서 사용할 수 있습니다 변환을 SkiaSharp 행렬 있지만 변환 매트릭스 이론적인 측면에서 중요 한 이며 중요 경로 수정 변환을 사용 하는 경우 변환에 대 한 알 필요 없이 또는 둘 다의 복잡 한 터치식 입력을 처리 하기 위한 이 문서에서는 다음에 설명 되어 있습니다.

![](matrix-images/matrixtransformexample.png "비트맵 유사 변환 적용")

현재 변환 매트릭스에 적용 된 `SKCanvas` 읽기 전용 액세스 하 여 언제 든 지 사용할 수는 [ `TotalMatrix` ](xref:SkiaSharp.SKCanvas.TotalMatrix) 속성입니다. 사용 하 여 새 변환 행렬을 설정할 수 있습니다 합니다 [ `SetMatrix` ](xref:SkiaSharp.SKCanvas.SetMatrix(SkiaSharp.SKMatrix)) 메서드를 복원할 수는 변환 행렬을 기본값으로 호출 하 여 [ `ResetMatrix` ](xref:SkiaSharp.SKCanvas.ResetMatrix)합니다.

경우에 다른 `SKCanvas` 캔버스의 매트릭스 변환을 직접 작동 하는 멤버가 [ `Concat` ](xref:SkiaSharp.SKCanvas.Concat(SkiaSharp.SKMatrix@)) 곱하여 해당 함께 두 매트릭스를 연결 하는 합니다.

기본 변환 매트릭스를 항등 행렬 및 대각선 셀과 0의 다른 곳에서 1 구성 됩니다.

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

사용 하는 정적 id 행렬을 만들 수 있습니다 [ `SKMatrix.MakeIdentity` ](xref:SkiaSharp.SKMatrix.MakeIdentity) 메서드:

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

합니다 `SKMatrix` 기본 생성자가 *하지* 항등 행렬을 반환 합니다. 0으로 설정 하는 셀을 모두 사용 하 여 행렬을 반환 합니다. 사용 하지 마십시오는 `SKMatrix` 생성자 해당 셀을 수동으로 설정 하려는 경우가 아니면 합니다.

SkiaSharp 그래픽 개체를 렌더링 하는 경우 각 점 (x, y)는 효과적으로 세 번째 열에서 1 사용 하 여 1 ~ 3 행렬으로 변환 됩니다.

<pre>
| x  y  1 |
</pre>

이 1 ~ 3 행렬을 1로 설정 하는 Z 좌표를 사용 하 여 3 차원 요소를 나타냅니다. 2 차원 행렬 변환 3 차원에서 작업 해야 하는 이유 (뒷부분에서 설명) 하는 수학 이유가 있습니다. 이 1 ~ 3 행렬 3D 좌표 시스템을 하지만 항상 2D 평면에 시점을 나타내는 Z = 1 생각할 수 있습니다.

이 1 ~ 3 매트릭스 변환 매트릭스를 곱합니다 다음 하 고 결과 캔버스에서 렌더링 하는 지점:

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

표준 행렬 곱셈을 사용 하는 변환 된 요소는 다음과 같습니다.

`x' = x`

`y' = y`

`z' = 1`

이것이 기본 변환입니다.

경우는 `Translate` 메서드를 호출 합니다 `SKCanvas` 개체를 `tx` 및 `ty` 인수를를 `Translate` 메서드 변환 행렬의 셋째 행의 처음 두 셀 수:

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

곱셈은 이제 다음과 같습니다.

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

변환 하는 수식을 다음과 같습니다.

`x' = x + tx`

`y' = y + ty`

크기 조정 요인을 기본값이 1입니다. 호출 하는 경우는 `Scale` 새 메서드 `SKCanvas` 개체를 결과 변환 매트릭스를 포함 합니다 `sx` 및 `sy` 대각선 셀에는 인수:

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

변환 수식에는 다음과 같습니다.

`x' = sx · x`

`y' = sy · y`

호출한 후 변형 행렬 `Skew` 행렬 셀 크기 조정 요인을에 인접 한 두 개의 인수를 포함 합니다.

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

변환 하는 수식을 다음과 같습니다.

`x' = x + xSkew · y`

`y' = ySkew · x + y`

에 대 한 호출에 대 한 `RotateDegrees` 또는 `RotateRadians` α의 각도 변환 매트릭스는 다음과 같습니다.

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

변환 하는 수식을 다음과 같습니다.

`x' = cos(α) · x - sin(α) · y`

`y' = sin(α) · x - cos(α) · y`

Α 0도 인 경우 항등 매트릭스입니다. Α 180도 인 경우 변환 행렬은 다음과 같습니다.

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

180도 회전은 가로 또는 세로로 대칭 이동은 개체는 또한-1의 배율 인수를 설정 하 여 수행 됩니다.

이러한 모든 유형의 변환으로 분류 됩니다 *affine* 변환 합니다. 되지 affine 변환만는 0, 0 및 1의 기본값을 유지 하는 행렬의 셋째 열에 포함 됩니다. 이 문서 [ **비 관계 변환** ](non-affine.md) 비 관계 변환에 설명 합니다.

## <a name="matrix-multiplication"></a>행렬 곱

변환 매트릭스를 사용 하 여 중요 한 장점 중 하나는 복합 변환으로 SkiaSharp 설명서에서 자주 참조 되는 매트릭스 곱하기를 통해 가져올 수 있음을 *연결*합니다. 변환 관련 메서드 중 많은 `SKCanvas` "사전 연결" 또는 "pre-concat."를 참조 하세요. 이 곱하기 행렬 곱셈 가환 적 이므로 반드시 순서 나타냅니다.

예를 들어, 설명서는 [ `Translate` ](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single)) 방법에 대 한 설명서는 동안 해당 it "Pre-concats 지정된 된 이동 사용 하 여 현재 행렬" 라는 합니다 [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) 메서드는 it "Pre-concats 지정한 소수 자릿수를 사용 하 여 현재 행렬입니다." 라는

즉, 메서드 호출에서 지정 된 변환을 승수 (왼쪽 피연산자)는 현재 변환 매트릭스는 피 승수 (오른쪽 피연산자).

가정 `Translate` 뒤에 라고 `Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

합니다 `Scale` 변환 곱하고 `Translate` 복합 변환 행렬의 변환:

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` 전에 라고 `Translate` 같이:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

경우 곱하기의 순서를 반대로 및 크기 조정 요인을 번역 요소에 효과적으로 적용 됩니다.

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

다음은 `Scale` 피벗 점 사용 하 여 메서드:

```csharp
canvas.Scale(sx, sy, px, py);
```

다음 변환 및 확장 호출을 하는 것과 같습니다.

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

세 가지 변환 행렬 메서드가 코드에 표시 하는 방법에서 역순으로 곱할:

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

## <a name="the-skmatrix-structure"></a>SKMatrix 구조

`SKMatrix` 형식의 9 읽기/쓰기 속성을 정의 하는 구조 `float` 변환 행렬의 셀 9에 해당 합니다.

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` 또한 라는 속성을 정의 [ `Values` ](xref:SkiaSharp.SKMatrix.Values) 형식의 `float[]`합니다. 순서 대로 한 번에 9 개의 값을 가져오거나 설정 하려면이 속성을 사용할 수 있습니다 `ScaleX`, `SkewX`, `TransX`, `SkewY`, `ScaleY`를 `TransY`, `Persp0`, `Persp1`, 및 `Persp2`합니다.

합니다 `Persp0`, `Persp1`, 및 `Persp2` 셀 문서에서 설명 됩니다 [ **비 관계 변환**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)합니다. 이러한 셀 0, 0 및 1의 기본값으로 변환의 같이 좌표 지점으로 곱합니다.

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

`x' = ScaleX · x + SkewX · y + TransX`

`y' = SkewX · x + ScaleY · y + TransY`

`z' = 1`

전체 2 차원 유사 변환입니다. 유사 변환에는 사각형 되지 변환 되는 평행 사변형 이외의 즉 평행선을 유지 합니다.

합니다 `SKMatrix` 구조를 만드는 몇 가지 정적 메서드를 정의 `SKMatrix` 값입니다. 이러한 모든 반환 `SKMatrix` 값:

- [`MakeTranslation`](xref:SkiaSharp.SKMatrix.MakeTranslation(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single,System.Single,System.Single)) 피벗 점을 사용 하 여
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single)) 라디안에서 각도
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single,System.Single,System.Single)) 피벗 점 사용 하 여 라디안 각도
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single))
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single,System.Single,System.Single)) 피벗 점을 사용 하 여
- [`MakeSkew`](xref:SkiaSharp.SKMatrix.MakeSkew(System.Single,System.Single))

`SKMatrix` 또한 정의 두 매트릭스를 연결 하는 몇 가지 정적 메서드 곱한 결과를 의미 합니다. 이러한 메서드 이름은 [ `Concat` ](xref:SkiaSharp.SKMatrix.Concat*), [ `PostConcat` ](xref:SkiaSharp.SKMatrix.PostConcat*), 및 [ `PreConcat` ](xref:SkiaSharp.SKMatrix.PreConcat*), 두 가지 버전의 각 및 합니다. 이러한 메서드는 반환 값이 없습니다. 기존 참조는 대신 `SKMatrix` 값을 통해 `ref` 인수입니다. 다음 예에서 `A`, `B`, 및 `R` ("결과")은 모든 `SKMatrix` 값입니다.

두 `Concat` 다음과 같은 메서드를 호출 합니다.

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

이러한 다음 곱하기를 수행합니다.

`R = B × A`

다른 방법만 두 개의 매개 변수입니다. 첫 번째 매개 변수를 수정 하 고 메서드 호출에서 반환 시를 두 매트릭스를 포함 합니다. 두 `PostConcat` 다음과 같은 메서드를 호출 합니다.

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

이러한 호출은 다음 작업을 수행합니다.

`A = A × B`

두 `PreConcat` 메서드는 유사 합니다.

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

이러한 호출은 다음 작업을 수행합니다.

`A = B × A`

모두를 사용 하 여 이러한 메서드의 버전 `ref` 인수는 기본 구현을 호출에서 약간 더 효율적 이지만 코드를 읽고 사용 하 여 아무 것도 가정 하 고 다른 사용자에 게 혼동을 줄 수 있습니다는 `ref` 인수를 수정 하 여 메서드입니다. 또한 것 중 하나를 결과로 생성 되는 인수를 전달 하면 편리 합니다 `Make` 메서드와 같은:

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

이 다음 행렬을 만듭니다.

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

좌표 이동 변환 곱한 배율 변환입니다. 이 경우에는 `SKMatrix` 구조는 라는 메서드를 사용 하 여 바로 가기를 제공 [ `SetScaleTranslate` ](xref:SkiaSharp.SKMatrix.SetScaleTranslate(System.Single,System.Single,System.Single,System.Single)):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

이 안전 하 게 하는 몇 번 중 하나인는 `SKMatrix` 생성자입니다. `SetScaleTranslate` 메서드는 행렬의 모든 9 개의 셀을 설정 합니다. 사용 하기에 안전한 이기도 합니다 `SKMatrix` 정적 생성자 `Rotate` 및 `RotateDegrees` 메서드:

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

이 메서드에서 수행 *되지* 기존 변환에 회전 변환을 연결 합니다. 메서드는 행렬의 모든 셀을 설정합니다. 기능적으로 동일 합니다 `MakeRotation` 및 `MakeRotationDegrees` 메서드 제외 하 고 인스턴스화할 하지는 `SKMatrix` 값입니다.

이 있다고 가정해 봅시다는 `SKPath` 개체를 표시 하려고 하지만 약간 다른 방향으로 또는 다른 중심점을 포함할 것을 선호 합니다. 호출 하 여 해당 경로의 모든 좌표를 수정할 수 있습니다 합니다 [ `Transform` ](xref:SkiaSharp.SKPath.Transform(SkiaSharp.SKMatrix)) 메서드의 `SKPath` 사용 하 여는 `SKMatrix` 인수입니다. 합니다 **경로 변환** 페이지가 작업을 수행 하는 방법에 설명 합니다. [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs) 참조 클래스는 `HendecagramPath` 개체 필드에 해당 경로에 변환을 적용 하려면 하지만 해당 생성자를 사용 합니다.

```csharp
public class PathTransformPage : ContentPage
{
    SKPath transformedPath = HendecagramArrayPage.HendecagramPath;

    public PathTransformPage()
    {
        Title = "Path Transform";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        SKMatrix matrix = SKMatrix.MakeScale(3, 3);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(360f / 22));
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(300, 300));

        transformedPath.Transform(matrix);
    }
    ...
}
```

`HendecagramPath` 개체에는 중심이 (0, 0) 11 별 요소 모든 방향으로 100 단위로 해당 중심에서 바깥쪽으로 확장 하 고 있습니다. 이 경로 긍정 및 부정 좌표는 것을 의미 합니다. 합니다 **경로 변환** 페이지 3 배 정도 클 별이 있는 모든 양의 좌표를 사용 하 여 작업을 선호 합니다. 또한 하나의 수직으로 가리키도록 별 점에서 하지. 하려고 대신 별표의 한 위치에 대 한 바로 아래로 가리키도록 합니다. (하므로 별 11 포인트에 해당 없습니다 둘 다.) 이 22로 나눈 값으로 360도 별 회전 필요 합니다.

생성자 작성을 `SKMatrix` 를 사용 하 여 세 가지 별도 변환에서 개체를 `PostConcat` 여기서 A, B 및 C의 인스턴스는 다음 패턴을 사용 하면 메서드 `SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

이므로 일련의 연속 곱셈 결과 다음과 같습니다.

`A × B × C`

연속 늘린 후 곱하기 각 변환의 용도 이해 하는 데 도움이 됩니다. 배율 변환 좌표 –300에서 300 사이 있으므로 3 배 경로 좌표의 크기를 늘립니다. 회전 변환 별 원점 주위를 회전합니다. 좌표 이동 변환 후 이동 300 픽셀 오른쪽에서 양수 될 모든 좌표를 종료 합니다.

동일한 행렬을 생성 하는 다른 시퀀스가 있습니다. 다른 하나는 다음과 같습니다.

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

이 중심의 경로 먼저 회전 100 픽셀 오른쪽으로 및 따라서 모든 좌표는 양수입니다. 별 지점 (0, 0)의 새 왼쪽 위 모퉁이 기준으로 크기가 증가 됩니다.

`PaintSurface` 처리기가이 경로 단순히 렌더링할 수 있습니다.

```csharp
public class PathTransformPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Magenta;
            paint.StrokeWidth = 5;

            canvas.DrawPath(transformedPath, paint);
        }
    }
}

```

캔버스의 왼쪽 위 모퉁이에 표시 합니다.

[![](matrix-images/pathtransform-small.png "경로 변환 페이지의 3 배가 스크린샷")](matrix-images/pathtransform-large.png#lightbox "삼중 경로 변환 페이지 스크린샷")

이 프로그램의 생성자에는 다음 호출을 사용 하 여 경로에 행렬 적용 됩니다.

```csharp
transformedPath.Transform(matrix);
```

경로 *되지* 속성으로이 매트릭스를 유지 합니다. 대신 모든 경로의 좌표에 변환을 적용 됩니다. 경우 `Transform` 라고 다시 변환이 다시 적용 됩니다 및 변환을 실행 취소 하는 다른 매트릭스를 적용 하 여 뒤로 이동할 수는 유일한 방법은 합니다. 다행 스럽게도 합니다 `SKMatrix` 구조 정의 [ `TryInvert` ](xref:SkiaSharp.SKMatrix.TryInvert*) 메서드는 행렬을 가져옵니다는 지정된 된 매트릭스를 반대로 바꿉니다.

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

메서드는 `TryInverse` 일부 매트릭스를 반전할 수 있지만 행렬을 되돌릴 수 없는 그래픽 변환에 사용할 가능성이 없습니다.

행렬 변환 적용할 수도 있습니다는 `SKPoint` 값, 점의 배열을 `SKRect`, 또는 만으로도 프로그램 내에서 단일 숫자입니다. 합니다 `SKMatrix` 구조가 라는 단어로 시작 하는 메서드의 컬렉션을 사용 하 여 이러한 작업을 지 원하는 `Map`, 같은:

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

마지막으로 메서드를 사용 하는 경우에 유의 합니다 `SKRect` 구조체가 회전된의 사각형을 나타내는 수. 만 적합 메서드는 `SKMatrix` 번역을 나타내는 및 크기 조정 값입니다.

## <a name="interactive-experimentation"></a>대화형 실험

대화형으로 화면에서 이리저리 비트맵의 세 모퉁이 이동 하 고 어떤 변환 결과 표시를 하는 유사 변환에 대해 한 가지 방법입니다. 이 개념을 **Affine 행렬 표시** 페이지입니다. 이 페이지에도 다른 데모에서 사용 되는 다른 두 클래스를 필요 합니다.

합니다 [ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchPoint.cs) 클래스 화면으로 끌 수 있습니다는 반투명 원을 표시 합니다. `TouchPoint` `SKCanvasView` 또는의 부모에 있는 요소는 `SKCanvasView` 가 합니다 [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchEffect.cs) 연결 합니다. `Capture` 속성을 `true`으로 설정합니다. 에 `TouchAction` 프로그램에서 호출 해야 이벤트 처리기는 `ProcessTouchEvent` 에서 메서드 `TouchPoint` 각각에 대 한 `TouchPoint` 인스턴스. 메서드는 반환 `true` 터치 지점 이동에서 터치 이벤트를 발생 시킨 경우. 또한 합니다 `PaintSurface` 처리기를 호출 해야 합니다는 `Paint` 각 메서드 `TouchPoint` 인스턴스를 전달 합니다 `SKCanvas` 개체.

`TouchPoint` 일반적인 방법을 보여 줍니다 방식으로 별도 클래스에서 SkiaSharp 시각적 개체를 캡슐화 할 수 있습니다. 클래스는 시각적 개체의 특성을 지정 하기 위한 속성을 정의할 수 있으며 메서드 `Paint` 사용 하 여는 `SKCanvas` 인수에서 렌더링할 수 있습니다.

합니다 `Center` 속성의 `TouchPoint` 개체의 위치를 나타냅니다. 위치를 초기화할이 속성을 설정할 수 있습니다. 속성에는 원 캔버스 주위를 끌 때 변경 합니다.

합니다 **Affine 행렬 페이지 표시** 도 필요 합니다 [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/MatrixDisplay.cs) 클래스입니다. 셀을 표시 하는이 클래스는 `SKMatrix` 개체입니다. 두 개의 공용 메서드를가지고: `Measure` 렌더링 된 행렬의 크기를 가져오려면 및 `Paint` 표시 합니다. 클래스는 `MatrixPaint` 형식의 속성 `SKPaint` 다른 글꼴 크기 또는 색에 바꿀 수 있습니다.

합니다 [ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) 파일은 합니다 `SKCanvasView` 연결을 `TouchEffect`합니다. 합니다 [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) 코드 숨김 파일을 만드는 세 가지 `TouchPoint` 개체 및 포함 된에서 로드 하는 비트맵의 세 모퉁이에 해당 하는 위치로 설정 리소스:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    SKMatrix matrix;
    SKBitmap bitmap;
    SKSize bitmapSize;

    TouchPoint[] touchPoints = new TouchPoint[3];

    MatrixDisplay matrixDisplay = new MatrixDisplay();

    public ShowAffineMatrixPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }

        touchPoints[0] = new TouchPoint(100, 100);                  // upper-left corner
        touchPoints[1] = new TouchPoint(bitmap.Width + 100, 100);   // upper-right corner
        touchPoints[2] = new TouchPoint(100, bitmap.Height + 100);  // lower-left corner

        bitmapSize = new SKSize(bitmap.Width, bitmap.Height);
        matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                           touchPoints[1].Center,
                                           touchPoints[2].Center);
    }
    ...
}
```

affine 행렬 3 개의 점으로 정의 고유 하 게 됩니다. 세 가지 `TouchPoint` 비트맵의 왼쪽 위, 오른쪽 위 및 왼쪽 아래 모퉁이에 해당 하는 개체입니다. 관계 매트릭스는 평행 사변형으로 변환 하는 사각형의 수만 이기 때문에 네 번째 포인터는 다른 3 개 포함 됩니다. 생성자를 호출 하 여 마지막 `ComputeMatrix`의 셀을 계산 하는 `SKMatrix` 이러한 세 지점에서 개체입니다.

합니다 `TouchAction` 처리기 호출을 `ProcessTouchEvent` 메서드의 각 `TouchPoint`합니다. `scale` 값 Xamarin.Forms 좌표에서 픽셀로 변환 합니다.

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = canvasView.CanvasSize.Width / (float)canvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                               touchPoints[1].Center,
                                               touchPoints[2].Center);
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

있는 경우 `TouchPoint` 메서드를 호출 합니다 움직인 `ComputeMatrix` 다시 화면을 무효화 하 고 합니다.

`ComputeMatrix` 메서드는 이러한 3 개의 점으로 사용 권한에 포함 된 행렬을 결정 합니다. 행렬 호출 `A` 평행 사변형으로 1 픽셀 사각형 사각형 세 지점에 따라 변환 호출 배율 변환 하는 동안 `S` 비트맵 1 픽셀 사각형 사각형을 확장 합니다. 복합 매트릭스가 `S` × `A`:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL)
    {
        // Scale transform
        SKMatrix S = SKMatrix.MakeScale(1 / size.Width, 1 / size.Height);

        // Affine transform
        SKMatrix A = new SKMatrix
        {
            ScaleX = ptUR.X - ptUL.X,
            SkewY = ptUR.Y - ptUL.Y,
            SkewX = ptLL.X - ptUL.X,
            ScaleY = ptLL.Y - ptUL.Y,
            TransX = ptUL.X,
            TransY = ptUL.Y,
            Persp2 = 1
        };

        SKMatrix result = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

마지막으로 `PaintSurface` 메서드는 매트릭스에 따라 비트맵을 렌더링 화면 맨 아래에 행렬을 표시 하 고 비트맵의 세 모퉁이에 터치 포인트를 렌더링 합니다.

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap using the matrix
        canvas.Save();
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(matrix);

        matrixDisplay.Paint(canvas, matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));

        // Display the touchpoints
        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }
  }
```

다른 두 화면 몇 가지 조작 후 표시 하는 동안 페이지를 처음 로드 될 때 iOS 화면 아래 비트맵을 보여 줍니다.

[![](matrix-images/showaffinematrix-small.png "삼중 Affine 행렬 표시 페이지 스크린샷")](matrix-images/showaffinematrix-large.png#lightbox "삼중 Affine 행렬 표시 페이지 스크린샷")

이 처럼 터치 포인트 비트맵의 모퉁이 끌어 처럼 보이지만, 감을입니다. 터치 포인트에서 계산 된 행렬 모서리 터치 포인트를 맞춰 있도록 비트맵을 변환 합니다.

사용자가 이동, 크기 조정 및 모서리를 끌어가 아니라 비트맵 회전에 대 한 더 자연 스러운 있지만 사용 하 여 하나 또는 두 손가락 직접을 개체에 손가락 모으기, 회전 합니다. 다음 문서에서 다룹니다 [ **터치 조작**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)합니다.

## <a name="the-reason-for-the-3-by-3-matrix"></a>3-3 하 여 행렬에 대 한 이유

2 차원 그래픽 시스템만-2-2에서 변환 행렬을 필요는 정상일 수 있습니다.

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

크기 조정, 회전 및 심지어 기울이기이 기능이 작동 하지만 지원 되지 가장 변환 하는 기본 변환 됩니다.

2-2 하 여 행렬을 나타냄을 문제가 되는 *선형* 2 차원에서 변환 합니다. 선형 변환에는 일부 기본 산술 연산을 유지 이지만 영향 중 선형 변환에 점 (0, 0)을 변경 하지 않습니다. 선형 변환 하는 번역은 불가능 합니다.

3 차원에서 선형 변환 매트릭스를 다음과 같이 표시 됩니다.

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

레이블이 지정 된 셀 `SkewXY` 값의 Y 값을 기반으로 한 X 좌표를 기울입니다 즉; 셀 `SkewXZ` 있음을 나타내고 값 오차 Z;의 값을 기반으로 한 X 좌표 값이 다른 마찬가지로 기울이기 `Skew` 셀입니다.

이 3D 변환 매트릭스를 설정 하 여 2 차원 평면을 제한할 수 있기 `SkewZX` 하 고 `SkewZY` 0으로 및 `ScaleZ` 1:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

2 차원 그래픽 Z = 1 3D 공간에서 평면에 전적으로 그려지는 경우 변환 곱셈은 다음과 같습니다.

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

모든 항목이 변하지 2 차원 평면에서 Z = 1, 하지만 `SkewXZ` 및 `SkewYZ` 셀 효과적으로 2 차원 번역 요소입니다.

3 차원 선형 변환 2 차원 비선형 변환을 역할도 하는 방법입니다. (비유 하자면 3D 그래픽의 변환에 기반한-4x4 행렬입니다.)

`SKMatrix` SkiaSharp의 구조는 세 번째 행에 대 한 속성을 정의 합니다.

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

0이 아닌 값 `Persp0` 고 `Persp1` Z = 1 2 차원 평면 해제 하는 개체를 이동 하는 변환에서 발생 합니다. 문서에 대해서는 해당 개체를 평면 다시 이동할 때 어떻게 될까요 [ **비 관계 변환**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
