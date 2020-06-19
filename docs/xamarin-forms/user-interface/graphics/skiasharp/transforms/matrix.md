---
title: SkiaSharp의 매트릭스 변환
description: 이 문서에서는 다양 한 변환 매트릭스를 사용 하 여 SkiaSharp 변환을 심층적으로 다이브 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: davidbritch
ms.author: dabritch
ms.date: 04/12/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e8d11add988828fa4e26d3f6728dd0b4319b3630
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84133304"
---
# <a name="matrix-transforms-in-skiasharp"></a>SkiaSharp의 매트릭스 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_다양 한 변환 매트릭스를 사용 하 여 SkiaSharp 변환에 대해 자세히 알아보기_

개체에 적용 되는 모든 변환은 `SKCanvas` 구조체의 단일 인스턴스로 통합 됩니다 [`SKMatrix`](xref:SkiaSharp.SKMatrix) . 이는 모든 최신 2D 그래픽 시스템의 표준 3 x 3 변형 행렬입니다.

앞서 살펴본 바와 같이 transform 행렬에 대해 몰라도 SkiaSharp에서 변환을 사용할 수 있지만 변형 매트릭스가 이론적 관점에서 중요 합니다. 변환을 사용 하 여 경로를 수정 하거나 복잡 한 터치 입력을 처리할 경우에는이 문서에 나와 있습니다.

![](matrix-images/matrixtransformexample.png "A bitmap subjected to an affine transform")

에 적용 되는 현재 변환 매트릭스는 `SKCanvas` 읽기 전용 속성에 액세스 하 여 언제 든 지 사용할 수 있습니다 [`TotalMatrix`](xref:SkiaSharp.SKCanvas.TotalMatrix) . 메서드를 사용 하 여 새 변환 매트릭스를 설정 하 [`SetMatrix`](xref:SkiaSharp.SKCanvas.SetMatrix(SkiaSharp.SKMatrix)) 고를 호출 하 여 해당 변환 행렬을 기본값으로 복원할 수 있습니다 [`ResetMatrix`](xref:SkiaSharp.SKCanvas.ResetMatrix) .

`SKCanvas`캔버스의 matrix 변환과 직접 작동 하는 다른 멤버는 [`Concat`](xref:SkiaSharp.SKCanvas.Concat(SkiaSharp.SKMatrix@)) 두 매트릭스를 곱하여 두 매트릭스를 연결 하는 것입니다.

기본 transform 행렬은 항등 매트릭스 이며 대각선 셀에는 1이 고 다른 모든 위치에는 0이 있습니다.

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

정적 메서드를 사용 하 여 항등 행렬을 만들 수 있습니다 [`SKMatrix.MakeIdentity`](xref:SkiaSharp.SKMatrix.MakeIdentity) .

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

`SKMatrix`기본 생성자는 항등 매트릭스를 반환 *하지* 않습니다. 모든 셀이 0으로 설정 된 행렬을 반환 합니다. `SKMatrix`이러한 셀을 수동으로 설정 하려는 경우가 아니면 생성자를 사용 하지 마세요.

SkiaSharp가 그래픽 개체를 렌더링 하는 경우 각 요소 (x, y)는 세 번째 열에 1이 있는 1 x 3 행렬로 효과적으로 변환 됩니다.

<pre>
| x  y  1 |
</pre>

이 1-3 행렬은 Z 좌표가 1로 설정 된 3 차원 점을 나타냅니다. 수학적 이유 (뒷부분에서 설명)는 2 차원 행렬 변환에서 3 차원으로 작업 해야 하는 이유입니다. 이 1-3 행렬은 3D 좌표계에서 점을 나타내는 것으로 간주할 수 있지만 항상 2D 평면에서 1과 같습니다.

이 1-3 행렬에는 transform 매트릭스가 곱하고 그 결과는 캔버스에서 렌더링 되는 지점입니다.

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

표준 행렬 곱셈을 사용 하 여 변환 된 지점은 다음과 같습니다.

`x' = x`

`y' = y`

`z' = 1`

이것이 기본 변환입니다.

`Translate`개체에 대해 메서드가 호출 되 면이 `SKCanvas` `tx` `ty` 메서드에 대 한 및 인수는 `Translate` transform 행렬의 세 번째 행에서 처음 두 셀이 됩니다.

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

이제 곱하기는 다음과 같습니다.

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

다음은 변환 수식입니다.

`x' = x + tx`

`y' = y + ty`

크기 조정 요인의 기본값은 1입니다. `Scale`새 개체에서 메서드를 호출 하 `SKCanvas` 는 경우 결과 변환 행렬에는 `sx` `sy` 대각선 셀의 및 인수가 포함 됩니다.

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

변환 수식은 다음과 같습니다.

`x' = sx · x`

`y' = sy · y`

를 호출한 후의 변환 행렬에는 배율 인수 옆에 있는 `Skew` 행렬 셀의 두 인수가 포함 됩니다.

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

변환 수식은 다음과 같습니다.

`x' = x + xSkew · y`

`y' = ySkew · x + y`

`RotateDegrees` `RotateRadians` Α 또는 각도에 대 한 호출의 경우 transform 행렬은 다음과 같습니다.

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

다음은 변환 수식입니다.

`x' = cos(α) · x - sin(α) · y`

`y' = sin(α) · x - cos(α) · y`

Α가 0 도이면 항등 매트릭스입니다. Α가 180 도이면 transform 행렬은 다음과 같습니다.

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

180 각도 회전은 개체를 가로 및 세로로 대칭 이동 하는 것과 같습니다 .이는 배율 인수를-1로 설정 하 여 수행 됩니다.

이러한 모든 유형의 변환은 *상관* 변환으로 분류 됩니다. 상관 변환은 행렬의 세 번째 열을 포함 하지 않습니다 .이 열은 기본값 0, 0, 1로 유지 됩니다. [**비 관계 변환**](non-affine.md) 문서에서는 비 상관 변환에 대해 설명 합니다.

## <a name="matrix-multiplication"></a>행렬 곱하기

Transform 행렬을 사용 하는 경우의 한 가지 중요 한 장점은 SkiaSharp 설명서에서 *연결*로 참조 되는 행렬 곱셈에서 복합 변환을 얻을 수 있다는 것입니다. 의 많은 변환 관련 메서드는 `SKCanvas` "사전 연결" 또는 "사전 연결"을 참조 합니다. 이는 행렬 곱셈이 교환 되지 않으므로 중요 한 곱하기의 순서를 나타냅니다.

예를 들어, 메서드에 대 한 설명서에서는 [`Translate`](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single)) "지정 된 번역을 사용 하 여 현재 매트릭스를 미리 concats" 하는 반면 메서드에 대 한 설명서에서는 [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) "지정 된 소수 자릿수를 사용 하 여 현재 매트릭스를 미리 concats" 라고 표시 합니다.

이는 메서드 호출로 지정 된 변환이 승수 (왼쪽 피연산자)이 고 현재 transform 매트릭스가 multiplicand (오른쪽 피연산자) 임을 의미 합니다.

가를 호출 하 고 다음을 호출 한다고 가정 합니다 `Translate` `Scale` .

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

`Scale`변형에 `Translate` 복합 변형 행렬의 변환이 곱해집니다.

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale`다음과 같이 호출할 수 있습니다 `Translate` .

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

이 경우 곱셈의 순서는 반대로 조정 되며 배율 인수는 다음과 같이 변환 요소에 효과적으로 적용 됩니다.

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

`Scale`피벗 점을 사용 하는 메서드는 다음과 같습니다.

```csharp
canvas.Scale(sx, sy, px, py);
```

이는 다음과 같은 변환 및 배율 호출에 해당 합니다.

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

세 가지 변환 매트릭스는 메서드를 코드에 표시 하는 방법에서 반대 순서로 곱합니다.

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

## <a name="the-skmatrix-structure"></a>서는 행렬 구조

`SKMatrix`구조는 `float` transform 행렬의 9 개 셀에 해당 하는 형식의 읽기/쓰기 속성 9 개를 정의 합니다.

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix`또한 형식 이라는 속성을 정의 [`Values`](xref:SkiaSharp.SKMatrix.Values) `float[]` 합니다. 이 속성을 사용 하 여,,,,,,, 및 순서로 한 샷의 9 개의 값을 설정 하거나 가져올 수 있습니다 `ScaleX` `SkewX` `TransX` `SkewY` `ScaleY` `TransY` `Persp0` `Persp1` `Persp2` .

`Persp0`, `Persp1` 및 셀은 `Persp2` [**비 관계 변환**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)문서에서 설명 합니다. 이러한 셀의 기본값은 0, 0, 1 이면 다음과 같이 변환에 좌표 점이 곱해집니다.

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

`x' = ScaleX · x + SkewX · y + TransX`

`y' = SkewX · x + ScaleY · y + TransY`

`z' = 1`

이는 완전 한 2 차원 상관 변환입니다. 상관 변환은 병렬 선을 유지 합니다. 즉, 사각형이 평행 사변형 이외의 값으로 변환 되지 않습니다.

`SKMatrix`구조체는 값을 만드는 몇 가지 정적 메서드를 정의 합니다 `SKMatrix` . 모든 반환 `SKMatrix` 값은 다음과 같습니다.

- [`MakeTranslation`](xref:SkiaSharp.SKMatrix.MakeTranslation(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single,System.Single,System.Single))피벗 점을 사용 하 여
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single))라디안의 각도
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single,System.Single,System.Single))피벗 점이 있는 라디안의 각도
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single))
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single,System.Single,System.Single))피벗 점을 사용 하 여
- [`MakeSkew`](xref:SkiaSharp.SKMatrix.MakeSkew(System.Single,System.Single))

`SKMatrix`는 두 매트릭스를 연결 하는 여러 정적 메서드를 정의 합니다 .이는 곱하기를 의미 합니다. 이러한 메서드의 이름은 [`Concat`](xref:SkiaSharp.SKMatrix.Concat*) , [`PostConcat`](xref:SkiaSharp.SKMatrix.PostConcat*) 및 이며 [`PreConcat`](xref:SkiaSharp.SKMatrix.PreConcat*) , 각각에는 두 가지 버전이 있습니다. 이러한 메서드에는 반환 값이 없습니다. 대신 인수를 통해 기존 `SKMatrix` 값을 참조 `ref` 합니다. 다음 예제에서, `A` `B` 및 `R` ("result"의 경우)는 모두 `SKMatrix` 값입니다.

두 `Concat` 메서드는 다음과 같이 호출 됩니다.

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

다음 곱셈을 수행 합니다.

`R = B × A`

다른 메서드에는 두 개의 매개 변수만 있습니다. 첫 번째 매개 변수는 수정 되 고 메서드 호출에서 반환 될 때 두 행렬의 곱을 포함 합니다. 두 `PostConcat` 메서드는 다음과 같이 호출 됩니다.

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

이러한 호출은 다음 작업을 수행 합니다.

`A = A × B`

두 가지 `PreConcat` 방법은 유사 합니다.

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

이러한 호출은 다음 작업을 수행 합니다.

`A = B × A`

모든 인수를 포함 하는 이러한 메서드 버전은 `ref` 기본 구현을 호출 하는 데 약간 더 효율적 이지만, 코드를 읽는 사람이 코드를 읽는 경우와 인수를 사용 하는 모든 항목이 메서드에 의해 수정 된다고 가정할 수 있습니다 `ref` . 또한 메서드 중 하나의 결과인 인수를 전달 하는 것이 편리한 경우가 `Make` 있습니다. 예를 들면 다음과 같습니다.

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

그러면 다음 행렬이 생성 됩니다.

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

이는 배율 변환에 변환 변환을 곱한 값입니다. 이 경우 `SKMatrix` 구조체는 라는 메서드를 포함 하는 바로 가기를 제공 합니다 [`SetScaleTranslate`](xref:SkiaSharp.SKMatrix.SetScaleTranslate(System.Single,System.Single,System.Single,System.Single)) .

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

이는 생성자를 사용 하는 것이 안전한 몇 가지 방법 중 하나입니다 `SKMatrix` . `SetScaleTranslate`메서드는 행렬의 9 개 셀을 모두 설정 합니다. `SKMatrix`정적 및 메서드에서 생성자를 사용 하는 것도 안전 `Rotate` 합니다 `RotateDegrees` .

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

이러한 메서드는 회전 변환을 기존 변환에 연결 *하지 않습니다* . 메서드는 행렬의 모든 셀을 설정 합니다. `MakeRotation`값을 인스턴스화하지 않는다는 점만 제외 하 고 및 메서드와 기능적으로 동일 `MakeRotationDegrees` `SKMatrix` 합니다.

표시 하려는 개체가 있다고 가정 `SKPath` 하지만 약간 다른 방향이 나 다른 중심점을 사용 하는 것이 좋습니다. [`Transform`](xref:SkiaSharp.SKPath.Transform(SkiaSharp.SKMatrix))인수를 사용 하 여의 메서드를 호출 하 여 해당 경로의 모든 좌표를 수정할 수 있습니다 `SKPath` `SKMatrix` . **경로 변환** 페이지에서는이 작업을 수행 하는 방법을 보여 줍니다. [`PathTransform`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs)클래스는 `HendecagramPath` 필드의 개체를 참조 하지만 해당 생성자를 사용 하 여 해당 경로에 변환을 적용 합니다.

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

`HendecagramPath`개체의 중심은 (0, 0)이 고, star의 11 개 점은 모든 방향으로 100 단위로 해당 센터에서 바깥쪽으로 확장 됩니다. 이는 경로에 양수 및 음수 좌표가 모두 있음을 의미 합니다. **경로 변환** 페이지에서는 모든 양의 좌표를 사용 하 여 별 세 번 더 큰 별표로 작업할 수 있습니다. 또한 별모양의 한 지점을 수직으로 가리키지 않으려는 경우 대신 별모양의 한 지점이 바로 아래를 가리키도록 하려고 합니다. (별에는 11 점이 있으므로 둘 다 가질 수 없습니다.) 이렇게 하려면 별 360도를 22로 나눈 값을 회전 해야 합니다.

생성자는 `SKMatrix` 다음 패턴을 사용 하 여 메서드를 사용 하 여 세 개의 개별 변환에서 개체를 빌드합니다 `PostConcat` . 여기서 A, B 및 C는의 인스턴스입니다 `SKMatrix` .

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

이는 일련의 연속 multiplications 이므로 결과는 다음과 같습니다.

`A × B × C`

연속 multiplications는 각 변환의 기능을 이해 하는 데 도움이 됩니다. 크기 조정 변환은 경로 좌표 크기를 3의 비율로 늘려 하므로 좌표는-300에서 300 사이입니다. 회전 변환은 원점을 중심으로 별을 회전 시킵니다. 그런 다음 변환 변환은이를 300 픽셀 오른쪽 아래로 이동 하므로 모든 좌표가 양수가 됩니다.

동일한 행렬을 생성 하는 다른 시퀀스가 있습니다. 다른 항목은 다음과 같습니다.

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

이렇게 하면 중심 주위의 경로를 먼저 회전 한 다음 100 픽셀을 오른쪽으로, 아래로 변환 하 여 모든 좌표가 양수입니다. 그러면 별 (0, 0) 인 새 왼쪽 위 모퉁이를 기준으로 별 크기가 증가 합니다.

`PaintSurface`처리기는이 경로를 간단 하 게 렌더링할 수 있습니다.

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

캔버스의 왼쪽 위 모퉁이에 표시 됩니다.

[![](matrix-images/pathtransform-small.png "Triple screenshot of the Path Transform page")](matrix-images/pathtransform-large.png#lightbox "Triple screenshot of the Path Transform page")

이 프로그램의 생성자는 다음 호출을 사용 하 여 경로에 행렬을 적용 합니다.

```csharp
transformedPath.Transform(matrix);
```

경로는이 매트릭스를 속성으로 유지 *하지* 않습니다. 대신 경로의 모든 좌표에 변환을 적용 합니다. `Transform`을 다시 호출 하면 변환이 다시 적용 되 고 다시 이동할 수 있는 유일한 방법은 변환을 취소 하는 다른 행렬을 적용 하는 것입니다. 다행히 구조체는 `SKMatrix` [`TryInvert`](xref:SkiaSharp.SKMatrix.TryInvert*) 지정 된 행렬을 반대로 하는 행렬을 가져오는 메서드를 정의 합니다.

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

`TryInverse`모든 매트릭스가 반전할 수는 없지만 그래픽 변환에는 반전할 수 없는 매트릭스가 사용 될 가능성이 없기 때문에 메서드가 호출 됩니다.

`SKPoint`값, 요소 배열, `SKRect` 또는 프로그램 내의 단일 숫자에도 행렬 변환을 적용할 수 있습니다. `SKMatrix`구조는 다음과 같이 단어로 시작 하는 메서드 컬렉션을 사용 하 여 이러한 작업을 지원 합니다 `Map` .

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

마지막 메서드를 사용 하는 경우 `SKRect` 구조체에서 회전 된 사각형을 나타낼 수 없다는 점에 유의 하세요. 메서드는 `SKMatrix` 변환 및 크기 조정을 나타내는 값에만 의미가 있습니다.

## <a name="interactive-experimentation"></a>대화형 실험

상관 변환에 대 한 느낌을 얻는 한 가지 방법은 화면 주위에서 비트맵의 세 모퉁이를 대화형으로 이동 하 고 변환 결과를 확인 하는 것입니다. 이는 **관계 행렬 표시** 페이지의 개념입니다. 이 페이지에는 다른 데모 에서도 사용 되는 두 개의 다른 클래스가 필요 합니다.

[`TouchPoint`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchPoint.cs)클래스는 화면 주위에서 끌 수 있는 반투명 원을 표시 합니다. `TouchPoint`에는 `SKCanvasView` 또는의 부모인 요소에 연결 된가 있어야 합니다 `SKCanvasView` [`TouchEffect`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchEffect.cs) . `Capture` 속성을 `true`으로 설정합니다. `TouchAction`이벤트 처리기에서 프로그램은 `ProcessTouchEvent` `TouchPoint` 각 인스턴스에 대해의 메서드를 호출 해야 합니다 `TouchPoint` . `true`터치 이벤트로 인해 터치 지점이 이동 하는 경우 메서드는를 반환 합니다. 또한 처리기는 `PaintSurface` 각 인스턴스에서 메서드를 호출 하 여 개체에 전달 해야 합니다 `Paint` `TouchPoint` `SKCanvas` .

`TouchPoint`SkiaSharp 시각적 개체를 별도의 클래스에 캡슐화 하는 일반적인 방법을 보여 줍니다. 클래스는 시각적 개체의 특성을 지정 하는 속성을 정의할 수 있으며, `Paint` 인수를 사용 하 여 명명 된 메서드는 `SKCanvas` 이를 렌더링할 수 있습니다.

`Center`의 속성은 `TouchPoint` 개체의 위치를 나타냅니다. 이 속성을 설정 하 여 위치를 초기화할 수 있습니다. 사용자가 캔버스 주위에서 원을 끌면 속성이 변경 됩니다.

**상관 행렬 표시 페이지** 에도 클래스가 필요 합니다 [`MatrixDisplay`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/MatrixDisplay.cs) . 이 클래스는 개체의 셀을 표시 합니다 `SKMatrix` . 이 클래스에는 렌더링 된 `Measure` 행렬의 차원을 가져오고 표시 하는 두 개의 공용 메서드가 있습니다. `Paint` 클래스에는 `MatrixPaint` `SKPaint` 다른 글꼴 크기나 색으로 바꿀 수 있는 형식의 속성이 포함 되어 있습니다.

[**ShowAffineMatrixPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) 파일은를 인스턴스화하고 `SKCanvasView` 를 연결 합니다 `TouchEffect` . [**ShowAffineMatrixPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) 코드를 만든 다음 세 개의 개체를 만든 `TouchPoint` 다음 포함 된 리소스에서 로드 하는 비트맵의 세 모퉁이에 해당 하는 위치로 설정 합니다.

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

상관 행렬은 3 개의 점으로 고유 하 게 정의 됩니다. 세 `TouchPoint` 개체는 비트맵의 왼쪽 위, 오른쪽 위 및 왼쪽 아래 모퉁이에 해당 합니다. 상관 행렬은 사각형을 평행 사변형으로 변환할 수 있기 때문에 네 번째 지점은 나머지 세 번째 요소에 포함 됩니다. 생성자는에 대 한 호출로 종료 `ComputeMatrix` 되며, `SKMatrix` 이 세 요소에서 개체의 셀을 계산 합니다.

`TouchAction`처리기는 `ProcessTouchEvent` 각의 메서드를 호출 합니다 `TouchPoint` . `scale`값은 Xamarin.Forms 좌표를 픽셀로 변환 합니다.

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

`TouchPoint`가 이동 했으면 메서드는를 `ComputeMatrix` 다시 호출 하 고 화면을 무효화 합니다.

`ComputeMatrix`메서드는 이러한 3 개의 점이 암시 하는 행렬을 결정 합니다. 이라는 행렬은 `A` 1 픽셀 사각형을 세 개의 점에 따라 평행 사변형으로 변환 하는 반면, 배율 변환은 `S` 비트맵을 1 픽셀 사각형 사각형으로 확장 합니다. 복합 행렬은 `S` × `A` :

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

마지막으로, `PaintSurface` 이 메서드는 해당 행렬을 기준으로 비트맵을 렌더링 하 고, 화면 아래쪽에 매트릭스를 표시 하 고, 비트맵의 세 모퉁이에 터치 요소를 렌더링 합니다.

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

아래 iOS 화면에서는 페이지가 처음 로드 될 때 비트맵을 보여 주지만 다른 두 개의 화면에는 일부 조작 후에 표시 됩니다.

[![](matrix-images/showaffinematrix-small.png "Triple screenshot of the Show Affine Matrix page")](matrix-images/showaffinematrix-large.png#lightbox "Triple screenshot of the Show Affine Matrix page")

터치 점이 비트맵의 모퉁이를 끄는 것 처럼 보이지만이는 효과를 주는 것입니다. 터치 점에서 계산 된 행렬은 모퉁이가 터치 점과 일치 하도록 비트맵을 변환 합니다.

사용자가 모퉁이를 끄는 것이 아니라, 개체에서 직접 하나 또는 두 손가락을 사용 하 여 끌어서 놓고 회전 하는 것이 아니라 사용자가 비트맵을 이동 하 고 크기를 조정 하 고 회전 하는 것이 보다 자연스럽 게 좋습니다. 이에 대해서는 다음 문서 [**터치 조작을**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)다룹니다.

## <a name="the-reason-for-the-3-by-3-matrix"></a>3 x 3 행렬의 원인

2 차원 그래픽 시스템에는 2 x 2-2 개의 transform 행렬만 필요할 수 있습니다.

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

이는 크기 조정, 회전 및 기울이기에는 적용 되지만 변환의 가장 기본적인 변환은 지원 되지 않습니다.

문제는 2-2 매트릭스가 두 차원의 *선형* 변형을 나타내는 것입니다. 선형 변환은 일부 기본 산술 연산을 유지 하지만 선형 변환은 점 (0, 0)을 변경 하지 않는다는 의미 중 하나입니다. 선형 변환을 사용 하면 변환이 불가능 합니다.

3 차원에서 선형 변환 매트릭스는 다음과 같습니다.

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

레이블이 지정 된 셀은 `SkewXY` 값이 Y의 값을 기준으로 x 좌표를 기울이는 것을 의미 합니다. 셀은 `SkewXZ` 값이 Z 값을 기준으로 x 좌표를 기울일 것을 의미 하 고 다른 셀에 대해서도 유사한 값을 나타냅니다 `Skew` .

`SkewZX`및를 `SkewZY` 0으로 설정 하 고을 1로 설정 하 여이 3d 변환 매트릭스를 2 차원 평면으로 제한할 수 있습니다 `ScaleZ` .

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

2 차원 그래픽을 3D 공간의 평면에 완전히 그리면 (Z가 1 인 경우) 변환 곱셈은 다음과 같습니다.

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

모든 항목은 Z가 1과 같은 2 차원 평면에 남아 있지만 `SkewXZ` 및 `SkewYZ` 셀은 2 차원 변환 요인이 됩니다.

3 차원 선형 변환이 2 차원 비선형 변환의 역할을 하는 방법입니다. 즉, 3D 그래픽의 변환은 4 x 4 매트릭스를 기반으로 합니다.

`SKMatrix`SkiaSharp의 구조는 해당 세 번째 행에 대 한 속성을 정의 합니다.

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

및의 값이 0이 아니면 `Persp0` `Persp1` Z가 1 인 2 차원 평면에서 개체를 이동 하는 변환이 발생 합니다. 이러한 개체를 다시 해당 평면으로 이동할 때 발생 하는 작업은 [**비 상관 변환**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)에 대 한 문서에서 설명 합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
