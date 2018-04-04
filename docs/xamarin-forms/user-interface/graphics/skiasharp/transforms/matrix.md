---
title: 매트릭스 변환
description: 자세하게 다양 한 변환 매트릭스와 SkiaSharp 변환
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: 87bddc8d541167cef350658ac69f8aaac6d6a2ee
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="matrix-transforms"></a>매트릭스 변환

_자세하게 다양 한 변환 매트릭스와 SkiaSharp 변환_

에 적용 되는 모든 변환을 `SKCanvas` 개체의 단일 인스턴스에서 통합 됩니다는 [ `SKMatrix` ](https://developer.xamarin.com/api/type/SkiaSharp.SKMatrix/) 구조입니다. 모든 최신 2 차원 그래픽 시스템에서와 비슷한 표준 3-3 변환 행렬입니다.

앞서 살펴본 것 처럼에서 사용할 수 있습니다는 변환을 SkiaSharp 행렬 있지만 변환 매트릭스는 이론적 관점에서 볼 때 중요 한 지정 되며 중요 한 경로 수정 하는 변환을 사용 하는 경우 변환에 대 한 알 필요 없이 또는 둘 다의 복잡 한 터치식 입력을 처리 하기 위한 이 문서와 다음에는 보여 줍니다.

![](matrix-images/matrixtransformexample.png "유사 변환의 대상 비트맵")

현재 변환 매트릭스에 적용 된 `SKCanvas` 읽기 전용 액세스 하 여 언제 든 지 사용할 수는 [ `TotalMatrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.TotalMatrix/) 속성입니다. 사용 하 여 새 변환 매트릭스를 설정할 수 있습니다는 [ `SetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.SetMatrix/p/SkiaSharp.SKMatrix/) 메서드를 복원할 수는 변환 행렬을 기본값으로 호출 하 여 [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix/)합니다.

경우에 다른 `SKCanvas` 직접 캔버스의 매트릭스 변환을 사용 하는 멤버는 [ `Concat` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Concat/p/SkiaSharp.SKMatrix@/) 두 행렬을 곱한 하 여 연결입니다.

기본 변환 매트릭스를 항등 매트릭스 이며 대각선 셀과 0의 다른 곳에 있는 1으로 구성 됩니다.

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

정적을 사용 하 여 id 행렬을 만들 수 있습니다 [ `SKMatrix.MakeIdentity` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeIdentity()/) 메서드:

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

`SKMatrix` 기본 생성자가 *하지* 항등 매트릭스를 반환 합니다. 모든 셀 0으로 설정 된 행렬을 반환 합니다. 사용 하지 않는 `SKMatrix` 생성자 셀을 수동으로 설정 하지 않는 경우.

SkiaSharp 그래픽 개체를 렌더링할 때 각 지점 (x, y) 세 번째 열에 1 1-3 행렬에 효과적으로 변환 됩니다.

<pre>
| x  y  1 |
</pre>

이 1-3 행렬 Z 좌표를 1로 설정 된 3 차원 시각을 나타냅니다. 가지 수학 (나중에 설명)는 2 차원 행렬 변형을 3 차원에서 작업 해야 하는 이유입니다. Z = 1 3D 좌표계 하지만 항상 2D 평면에 시점을 나타내는로이 1-3 행렬의 생각할 수 있습니다.

이 1-3 매트릭스 곱하면 변환 매트릭스 및 결과 캔버스에 렌더링 하는 지점:

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

표준 매트릭스 곱을 사용 하 여 변환 된 요소는 다음과 같습니다.

x' = x

y' = y

z' = 1

이것이 기본 변환입니다.

때는 `Translate` 메서드가 호출 되는 `SKCanvas` 개체는 `tx` 및 `ty` 에 대 한 인수는 `Translate` 메서드 변환 매트릭스의 세 번째 행의 처음 두 셀 수:

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

다음은 변형 수식입니다.

x' = x + tx

y' y + ty =

배율 인수는 기본값이 1입니다. 호출 하는 경우는 `Scale` 에서 새로운 메서드 `SKCanvas` 결과 변환 매트릭스에 포함 된 개체는 `sx` 및 `sy` 대각선 셀에는 인수:

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

변형 수식은 다음과 같습니다.

x' sx · = x

y' sy · = y

변환 매트릭스는 호출한 후 `Skew` 배율 인수에 인접 한 행렬 셀에 두 개의 인수를 포함 합니다.

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

변형 수식에는

x' = x + xSkew · y

y' ySkew · = x + y

에 대 한 호출에 대 한 `RotateDegrees` 또는 `RotateRadians` α 각도로 변환 매트릭스는 다음과 같습니다.

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

다음은 변형 수식입니다.

x' = cos(α) · x - sin(α) · y

y' = sin(α) · x - cos(α) · y

Α 0도 이면 id 행렬 인지 합니다. Α 180도 이면 변환 매트릭스는 다음과 같습니다.

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

180도 회전은 개체를 가로로 대칭 이동 또는 세로로-1의 배율 인수를 설정 하 여 수행 됩니다입니다.

이러한 모든 유형의 변환으로 분류 되어 *유사* 변환 합니다. 3x3 유사 변환 하지 0, 0 및 1의 기본값 상태로 남아 있는 행렬의 세 번째 열에 적용 합니다. 문서 [비 3x3 유사 변형](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md) 유사 형식이 아닌 변형에 설명 합니다.

## <a name="matrix-multiplication"></a>매트릭스 곱

합성 변환 매트릭스 곱으로 SkiaSharp 설명서에서 자주 참조 되 여 가져올 수 있습니다는 변환 매트릭스를 사용 하 여 큰 이점이 *연결*합니다. 여러 관련 변환 메서드에 `SKCanvas` "사전 연결" 또는 "이전 concat"를 참조 하십시오 이 매트릭스 곱 가환 적 없기 때문에 중요 한 사항입니다 곱하기 순서와 참조 합니다.

예를 들어에 대 한 설명서는 [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) 메서드 표시에 대 한 설명서는 동안 해당 it "에 지정 된 이동 사용 된 Pre-concats"는 [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) 메서드는 한다는 "사전-concats 현재 지정 된 크기 조정 행렬입니다." 라는

즉, 메서드 호출에서 지정 된 변환이 승수 (왼쪽 피연산자) 현재 변환 매트릭스는 피 승수 (오른쪽 피연산자).

되었다고 가정 `Translate` 뒤 라고 `Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

`Scale` 변환 곱한는 `Translate` 복합 변환 매트릭스에 대 한 변형:

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` 호출 하기 전에 `Translate` 다음과 같이 합니다.

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

이 경우 곱하기의 순서는 반대 및 배율 인수는 번역 요인에 효과적으로 적용 됩니다.

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

다음은 `Scale` 피벗 지점과 메서드:

```csharp
canvas.Scale(sx, sy, px, py);
```

이 다음과 같습니다. 다음 번역 및 소수 호출

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

세 개의 변환 매트릭스 메서드가 코드에 표시 되는 방식에서 반대 순서로 곱할:

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

### <a name="the-skmatrix-structure"></a>SKMatrix 구조

`SKMatrix` 구조 형식의 9 개의 읽기/쓰기 속성을 정의 합니다. `float` 에 해당 하는 변환 행렬의 셀 9:

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` 또한 라는 속성을 정의 [ `Values` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix.Values/) 형식의 `float[]`합니다. 순서 대로 한 번에 9 개의 값을 가져오거나 설정 하려면이 속성을 사용할 수 `ScaleX`, `SkewX`, `TransX`, `SkewY`, `ScaleY`, `TransY`, `Persp0`, `Persp1`, 및 `Persp2`합니다.

`Persp0`, `Persp1`, 및 `Persp2` 셀은 문서에서 설명한 [비 3x3 유사 변형](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)합니다. 이러한 셀의 기본 값이 0, 0 및 1 있으면 변환을 다음과 같이 좌표 지점으로 곱합니다.

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

x' ScaleX · = x + SkewX · y + TransX

y' = SkewX · x + ScaleY · y + TransY

z' = 1

이 전체 2 차원 유사 변환입니다. 3x3 유사 변형 사각형 평행 사변형 이외의 값으로 변환 되지 않습니다 됩니다 의미 하는 병렬 줄을 유지 합니다.

`SKMatrix` 구조를 만드는 몇 가지 정적 메서드를 정의 `SKMatrix` 값입니다. 이러한 모든 반환 `SKMatrix` 값:

- [`MakeTranslation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeTranslation/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/System.Single/System.Single/) 피벗 지점으로
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/) 라디안에서 단위의 각도 대 한
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/System.Single/System.Single/) 피벗 지점과 라디안에서 단위의 각도 대 한
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/)
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/System.Single/System.Single/) 피벗 지점으로
- [`MakeSkew`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeSkew/p/System.Single/System.Single/)

`SKMatrix` 또한 정의 두 행렬을 연결 하는 몇 가지 정적 메서드를 곱한 결과를 의미 합니다. 이러한 메서드 이름은 `Concat`, `PostConcat`, 및 `PreConcat`, 각각의 두 가지 버전 이며 합니다. 이러한 메서드는 반환 값이 없습니다. 기존 참조는 대신, `SKMatrix` 값을 통해 `ref` 인수입니다. 다음 예에서 `A`, `B`, 및 `R` ("result")은 모든 `SKMatrix` 값입니다.

두 `Concat` 다음과 같은 메서드가 호출 됩니다.

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

이러한 다음 곱하기를 수행합니다.

R = B × A

두 개의 매개 변수가 포함 하는 다른 방법. 첫 번째 매개 변수를 수정 하 고 메서드 호출에서 반환 되, 두 행렬의 곱을 포함 합니다. 두 `PostConcat` 다음과 같은 메서드가 호출 됩니다.

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

이러한 호출은 다음 작업을 수행 합니다.

A = A × B

두 `PreConcat` 메서드는 유사 합니다.

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

이러한 호출은 다음 작업을 수행 합니다.

A = B × A

모든 이들 메서드 호출의 버전 `ref` 인수는 기본 구현을 호출에 약간 더 효율적 하지만 코드를 읽고와 어느 것에 가정 하 고 다른 사용자에 게 혼란을 줄 수 있습니다는 `ref` 인수는 메서드에 의해 수정 합니다. 또한 것이 편리할 인수 중 하나의 결과 전달 하는 `Make` 메서드와 같은:

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

다음 표를 만듭니다.

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

이동 변환을 곱한 크기 조정 변환입니다. 이 특별 한 경우에는 `SKMatrix` 구조 라는 메서드가 있는 바로 가기를 제공 [ `SetScaleTranslate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.SetScaleTranslate/p/System.Single/System.Single/System.Single/System.Single/):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

이 안전 하 게 사용할 경우 몇 번 중 하나는 `SKMatrix` 생성자입니다. `SetScaleTranslate` 행렬의 셀 9 모든 메서드를 설정 합니다. 안전 하 게도 `SKMatrix` 정적 생성자 `Rotate` 및 `RotateDegrees` 메서드:

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

이 메서드에서 수행 *하지* 기존 변환을를 회전 변환을 연결 합니다. 메서드는 행렬의 모든 셀을 설정합니다. 기능적으로 동일는 `MakeRotation` 및 `MakeRotationDegrees` 메서드 제외 하 고 인스턴스화할 하지는 `SKMatrix` 값입니다.

있다고 가정 하면는 `SKPath` 개체를 표시 하려고 하지만 약간 다른 방향 또는 다른 중심점 하는 방법을입니다. 호출 하 여 해당 경로의 모든 좌표를 수정할 수 있습니다는 [ `Transform` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Transform/p/SkiaSharp.SKMatrix/) 방식의 `SKPath` 와 `SKMatrix` 인수입니다. **경로 변환** 페이지에는이 작업을 수행 하는 방법을 보여 줍니다. [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs) 참조 클래스는 `HendecagramPath` 개체 필드에 해당 경로에 변환을 적용 하려면 하지만 해당 생성자를 사용 합니다.

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

`HendecagramPath` 개체에 센터의 (0, 0) 및 11 개 점 핫스폿에 모든 방향으로 100 단위 해당 센터에서 바깥쪽으로 확장 합니다. 이 경로는 긍정 및 부정 좌표 것을 의미 합니다. **경로 변환** 페이지와 모든 양의 좌표 3 배 정도 클 별이 있는 작업 수행을 배치 하려고 합니다. 또한 위로 가리키도록 별의 한 위치를 싶어하지 합니다. 원하는 대신 별의 한 위치에 대 한 바로 아래로 가리키도록 합니다. (별 11 개 점 있어서 것 모두 포함할 수 없습니다.) 그러려면 22로 나눈 값으로 360도 별 회전 합니다.

생성자를 작성 한 `SKMatrix` 를 사용 하 여 다음 세 가지 별도 변환에서 개체는 `PostConcat` 여기서 A, B 및 C의 인스턴스는 다음 패턴을 사용 하 여 메서드 `SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

이므로 일련 연속 곱하기의 결과 다음과 같습니다.

A × B × C

연속 된 곱하기 각 변환을 수행 하는 작업을 이해 하는 데 도움이 됩니다. 배율 변환을 좌표 –300에서 300 사이 있으므로 3 배 경로 좌표의 크기를 늘립니다. 회전 변환을 별 원본 주위를 회전합니다. 이동 변환 후 이동 및 양의 될 모든 좌표의 아래쪽 300 픽셀 오른쪽 여 합니다.

동일한 매트릭스를 생성 하는 다른 시퀀스가. 다른 하나는 다음과 같습니다.

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

먼저 경로 중심을 기준으로 회전 하 고 오른쪽으로 100 픽셀 변환이 및 모든 아래쪽 좌표는 양수입니다. 별 지점 (0, 0) 새 왼쪽 위 모퉁이 기준으로 크기를 늘린 다음 됩니다.

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

캔버스의 왼쪽 위 모서리에 표시 합니다.

[![](matrix-images/pathtransform-small.png "경로 변환 페이지의 삼중 스크린샷")](matrix-images/pathtransform-large.png#lightbox "경로 변환 페이지의 삼중 스크린 샷")

이 프로그램의 생성자 호출으로 경로에 행렬을 적용 됩니다.

```csharp
transformedPath.Transform(matrix);
```

경로 *하지* 속성으로이 행렬을 유지 합니다. 대신, 모든 경로 좌표에 변환을 적용 합니다. 경우 `Transform` 라고 다시 변환이 다시 적용 되 및 변환 실행 취소 하는 다른 매트릭스에 적용 하 여 돌아갈 수 있습니다 하는 유일한 방법은 것입니다. 다행히는 `SKMatrix` 구조 정의 [ `TryInverse` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.TryInvert/p/SkiaSharp.SKMatrix/) 행렬을가 가져오는 메서드는 지정된 된 매트릭스를 되돌립니다.

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

메서드는 `TryInverse` 일부만 행렬은 반전할 수만 행렬을 반전할 수 비 그래픽 변환에 대 한 사용 되지 않습니다.

행렬 변환을 적용할 수 있습니다는 `SKPoint` 값, 배열 포인트는 `SKRect`, 또는 심지어 프로그램 내에서 단일 숫자입니다. `SKMatrix` 구조 단어로 시작 하는 메서드의 컬렉션을 사용 하 여 이러한 작업을 지원 `Map`, 이러한:

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

마지막 해당 메서드를 사용 한다는 점에 유의 하는 `SKRect` 구조체가 회전 된 사각형을 표시할 수 있습니다. 만 의미가 대 한 메서드는 `SKMatrix` 번역을 나타내는 및 크기 조정 값입니다.

### <a name="interactive-experimentation"></a>대화형 실험

하나의 3x3 유사 변형에 대해 방법은 대화형으로 화면 비트맵의 세 모퉁이 이동 하 고 어떤 변환 결과 표시 합니다. 이 기본 개념은 **유사 매트릭스를 표시** 페이지. 이 페이지에도 다른 데모에 사용 되는 다른 두 클래스 필요 합니다.

[ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchPoint.cs) 클래스는 화면으로 끌어 올 수 있는 반투명 원을 표시 합니다. `TouchPoint` 사용 하려면는 `SKCanvasView` 또는의 부모 요소는 `SKCanvasView` 가 [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchEffect.cs) 연결 합니다. `Capture` 속성을 `true`으로 설정합니다. 에 `TouchAction` 이벤트 처리기는 프로그램 호출 해야 합니다는 `ProcessTouchEvent` 에서 메서드 `TouchPoint` 각 `TouchPoint` 인스턴스. 메서드가 반환 `true` 터치 이벤트 결과 터치 포인트를 이동 합니다. 또한는 `PaintSurface` 처리기를 호출 해야 합니다는 `Paint` 각 메서드 `TouchPoint` 를 전달 인스턴스는 `SKCanvas` 개체입니다.

`TouchPoint` 공통을 보여 줍니다. 방식으로 SkiaSharp 시각적 개체는 별도 클래스에 캡슐화 할 수 있습니다. 클래스는 시각적 개체의 특성을 지정 하기 위한 속성을 정의 하 고 라는 메서드가 `Paint` 와 `SKCanvas` 인수에서 렌더링할 수 있습니다.

`Center` 속성 `TouchPoint` 개체의 위치를 나타냅니다. 위치; 초기화할이 속성을 설정할 수 있습니다. 속성의 원을 캔버스 주위를 끌 때 변경 됩니다.

**Affine 행렬 페이지 표시** 도 필요는 [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/MatrixDisplay.cs) 클래스입니다. 셀을 표시 하는이 클래스는 `SKMatrix` 개체입니다. 이 클래스에 두 개의 공용 메서드가: `Measure` 렌더링 된 행렬의 크기를 가져올 수 및 `Paint` 표시 합니다. 클래스에 포함 되어는 `MatrixPaint` 형식의 속성이 `SKPaint` 다른 글꼴 크기 또는 색에 대 한 대체 될 수 있습니다.

[ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) 파일 인스턴스화하는 `SKCanvasView` 첨부 한 `TouchEffect`합니다. [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) 코드 숨김 파일 세 개를 만들며 `TouchPoint` 개체를 다음 위치에 해당 하는 포함 된에서 로드 하는 비트맵의 세 모퉁이로 설정 합니다 리소스:

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
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

Affine 행렬 3 개의 점으로 정의 고유 하 게 됩니다. 세 개의 `TouchPoint` 비트맵의 왼쪽 위, 오른쪽 위 및 왼쪽 아래 모퉁이에 해당 하는 개체입니다. Affine 행렬만 평행 사변형 사각형 변형 수 있어서 네 번째 지점은 다른 3 개 포함 됩니다. 생성자에 대 한 호출 끝나는 `ComputeMatrix`의 셀을 계산 하는 `SKMatrix` 이러한 세 가지 지 점으로부터 개체입니다.

`TouchAction` 처리기 호출의 `ProcessTouchEvent` 각 메서드 `TouchPoint`합니다. `scale` 값 Xamarin.Forms 좌표에서 픽셀로 변환:

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

있는 경우 `TouchPoint` 메서드를 호출 합니다 옮겼습니다 `ComputeMatrix` 다시 화면을 무효화 하 고 있습니다.

`ComputeMatrix` 메서드는 이러한 3 개의 점으로 권한에 포함 된 행렬을 확인 합니다. 행렬 호출 `A` 평행 사변형에 1 픽셀 정사각형 사각형 점이 3 개에 따라 변환 하는 동안 호출 배율 변환을 `S` 1 픽셀의 사각형 사각형으로 비트맵의 크기를 조정 합니다. 복합 행렬은 `S` × `A`:

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

        SKMatrix result;
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

마지막으로 `PaintSurface` 메서드 해당 행렬에 따라 비트맵 렌더링 화면 맨 아래에 행렬을 표시 하 고 비트맵의 세 모퉁이에서 터치 포인트를 렌더링 합니다.

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

다른 두 화면 조작 후 표시 하는 동안 페이지를 처음 로드 될 때 아래의 iOS 화면 비트맵을 보여 줍니다.

[![](matrix-images/showaffinematrix-small.png "Affine 행렬 표시 페이지의 삼중 스크린샷")](matrix-images/showaffinematrix-large.png#lightbox "Affine 행렬 표시 페이지의 삼중 스크린샷")

것 처럼 터치 포인트 비트맵의 모서리를 끌어 처럼 보이지만, 가정을입니다. 터치 포인트에서 계산 되는 행렬 모서리 터치 포인트와 일치 하는 비트맵을 변환 합니다.

사용자가 이동, 크기 조정 및 모서리를 끌어가 아니라 비트맵을 회전 하려면 더 자연 스러운 있지만 사용 하 여 하나 또는 두 손가락 직접 개체를 끌어 오면에 손가락 모으기, 회전 합니다. 이 내용은 다음 문서에서 설명 [터치 조작](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)합니다.

### <a name="the-reason-for-the-3-by-3-matrix"></a>3-3 행렬에 대 한 이유

2 차원 그래픽 시스템에 2-2 변환 매트릭스 필요는 것으로 예상 될 수 있습니다.

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

크기 조정, 회전 및에 기울이기이 기능이 작동 하지만 수 없는 경우 중에서 가장 기본적인 변환는 변환입니다.

이 문제는 2-2 매트릭스를 나타낸다고는 *선형* 2 차원에서 변형 합니다. 선형 변환에는 몇 가지 기본 산술 연산을 유지 되어 있지만 중 하나에 영향을 준다는 선형 변환 점 (0, 0)을 변경 하지 않습니다. 선형 변환 하는 번역 불가능 합니다.

3 차원에서 선형 변환 매트릭스를 다음과 같이 보입니다.

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

레이블이 지정 된 셀 `SkewXY` 값 Y의 값을 기반으로 한 X 좌표를 기울입니다 의미; 셀 `SkewXZ` 있음을 나타내고 값 Z;의 값을 기반으로 한 X 좌표를 기울입니다 값 다른 마찬가지로 기울이기 `Skew` 셀입니다.

이 3D 변환 매트릭스를 설정 하 여 2 차원 평면을 제한할 수는 `SkewZX` 및 `SkewZY` 0으로 및 `ScaleZ` 1로:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

2 차원 그래픽 Z = 1 3D 공간에서 평면에 완전히 그리거나 변환 곱하기의 모양은 다음과 같습니다.

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Z = 1, 2 차원 평면에 머물러 모든 것 이지만 `SkewXZ` 및 `SkewYZ` 셀 효과적으로 2 차원 번역 요소입니다.

3 차원 선형 변환 2 차원 비선형 변환으로 사용 하는 방법입니다. (비유, 3D 그래픽에는 변환은 기반으로 4-4 매트릭스 합니다.)

`SKMatrix` SkiaSharp의 구조는 세 번째 행에 대 한 속성을 정의 합니다.

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

0이 아닌 값의 `Persp0` 및 `Persp1` Z = 1 2 차원 평면 벗어난 개체를 이동 하는 변환에서 발생 합니다. 문서에 대해서는 평면에 해당 개체가 다시 이동 하면 어떤 일이 생기 [비 3x3 유사 변형](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)합니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
