---
title: "유사 형식이 아닌 변형"
description: "변환 행렬의 세 번째 열이 있는 테이퍼 효과 및 큐브 만들기"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 3fda8524b824042aa4aba07853da2801baf47027
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="non-affine-transforms"></a>유사 형식이 아닌 변형

_변환 행렬의 세 번째 열이 있는 테이퍼 효과 및 큐브 만들기_

변환, 크기 조정, 회전 및 기울이기 모두으로 분류 됩니다 *유사* 변환 합니다. 3x3 유사 변환 평행선을 보존합니다. 두 줄을 변환 하기 전에 병렬 그대로 계속 병렬 변환 후 합니다. 사각형은 항상 parallelograms 바뀝니다.

그러나 SkiaSharp 유사 형식이 아닌 변환, 그리고 모든 볼록이 사각형으로 사각형을 변환 하는 기능이 수 이기도 합니다.

![](non-affine-images/nonaffinetransformexample.png "볼록 사각형으로 변환 하는 비트맵")

볼록 사각형 각도가 내부 서로 교차 하지 않는 왼쪽과 180도 보다 항상 작아야 네 면 그림입니다.

비 3x3 유사 변형 변환 행렬의 세 번째 행은 0, 0 및 1 이외의 값으로 설정 된 경우 결과 합니다. 전체 `SKMatrix` 곱셈은:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

결과 변환 수식에는

x' = ScaleX·x + SkewX·y + TransX

y' = SkewY·x + ScaleY·y + TransY

z` = Persp0·x + Persp1·y + Persp2

2 차원 변환에는 3 x 3 매트릭스를 사용 하 여의 기본 규칙은 모든 하 게 유지 되도록 평면에 Z = 1입니다. 하지 않는 한 `Persp0` 및 `Persp1` 는 0으로, 및 `Persp2` 가 1 이면 변환을 Z 좌표 평면 오프 이동 되었습니다.

이를으로 복원 하려면 2 차원 변환은 평면에 좌표를 다시 이동 해야 합니다. 다른 단계는 필요 합니다. X', y', z' 값 해야 수로 나눈 z':

x" = x' / z'

y" = y' / z'

z" = z' / z' = 1

이 라고 *동종 좌표* Möbius 스트립 그의 토폴로지 문제에 대 한 훨씬 더 나은 알려진 년 8 월 페르디난드 Möbius 수학적으로 개발 된 하 고 있습니다.

하는 경우 z'가 0 이면 무한 좌표로 나누기 결과입니다. 실제로 동종 좌표를 개발 하기 위한 Möbius의 동기 중 하나는 무한 숫자를 사용 하 여 무한 값을 나타낼 수 있는 기능 했습니다.

그러나 그래픽을 표시할 때 렌더링을 무한 값으로 변환 하는 좌표를 통해 가능한 하지 않으려는 합니다. 이러한 좌표는 렌더링 되지 않습니다. 이러한 좌표 주변의 모든 매우 광범위 하 고 시각적으로 일관 되지 않이 됩니다.

이 수식에 원하지 않는 z 값 ' 0이 되 고 있습니다.

z` = Persp0·x + Persp1·y + Persp2

`Persp2` 0 또는 0이 아니면 셀 일 수 있습니다. 경우 `Persp2` 가 0 이면 z' 지점 (0, 0)에 대 한 0이 고 없는 일반적으로 바람직하지 해당 지점 2 차원 그래픽에서 매우 흔히 이므로 합니다. 경우 `Persp2` 경우 generality 손실 없이 0과 같지 않은 `Persp2` 1로 고정 됩니다. 예를 들어 판단 하 `Persp2` 수 단순히 행렬의 모든 셀으로 나누면 5, 낮추는 다음 5, 해야 `Persp2` 1로 동일 하며 결과 동일 합니다.

이러한 이유로, `Persp2` 항등 매트릭스에서 동일한 값인 1에 고정 종종 됩니다.

일반적으로 `Persp0` 및 `Persp1` 는 작은 숫자입니다. 예를 들어 항등 매트릭스 하지만 집합에서 시작 하 여 `Persp0` 0.01로:

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

변형 수식에는

x' = x / (0.01·x + 1)

y' = y / (0.01·x + 1)

이제이 변환을 사용 하 여 원점에 100 픽셀의 사각형 상자를 렌더링 합니다. 네 모퉁이 변환 하는 방법을 다음과 같습니다.

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

때 x가 100, z' 분모 이므로 2, x 및 y 좌표는 반으로 축소 합니다. 오른쪽 상자의 왼쪽 보다 짧은 됩니다.

![](non-affine-images/nonaffinetransform.png "유사 형식이 아닌 변형으로 발전 하 여 상자")

`Persp` 뷰어 먼 오른쪽의 상자 기울어진 이제는 제안 된 단축법 때문에 이러한 셀 이름의 부분은 "관점"를 의미 합니다.

**테스트 큐브 뷰** 페이지의 값을 실험할 수 있습니다 `Persp0` 및 `Pers1` 작동 방식을 이해할 수 있도록 합니다. 이러한 행렬 셀의 적절 한 값은 매우 작으므로 하는 `Slider` 유니버설 Windows 플랫폼에서 없습니다 올바르게 처리 합니다. 두 UWP 문제에 맞게 `Slider` 요소에는 [ **TestPerspective.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) – 1을 1로 범위를 초기화할 필요:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.TestPerspectivePage"
             Title="Test Perpsective">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Label">
                    <Setter Property="HorizontalTextAlignment" Value="Center" />
                </Style>

                <Style TargetType="Slider">
                    <Setter Property="Minimum" Value="-1" />
                    <Setter Property="Maximum" Value="1" />
                    <Setter Property="Margin" Value="20, 0" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="persp0Slider"
                Grid.Row="0"
                ValueChanged="OnPersp0SliderValueChanged" />

        <Label x:Name="persp0Label"
               Text="Persp0 = 0.0000"
               Grid.Row="1" />

        <Slider x:Name="persp1Slider"
                Grid.Row="2"
                ValueChanged="OnPersp1SliderValueChanged" />

        <Label x:Name="persp1Label"
               Text="Persp1 = 0.0000"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

슬라이더에 대 한 이벤트 처리기는 [ `TestPerspectivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) –0.01 및 0.01 사이 있도록 코드 숨김 파일 100으로 값을 나눕니다. 또한 생성자는 비트맵에 로드합니다.

```csharp
public partial class TestPerspectivePage : ContentPage
{
    SKBitmap bitmap;

    public TestPerspectivePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }

    void OnPersp0SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp0Label.Text = String.Format("Persp0 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }

    void OnPersp1SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp1Label.Text = String.Format("Persp1 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }
    ...
}
```

`PaintSurface` 처리기에서 계산 된 `SKMatrix` 라는 값 `perspectiveMatrix` 100로 나눈 값이 두 개의 슬라이더의 값을 기반으로 합니다. 이 결합 된 두이 변환의 중심 비트맵의 가운데에 배치 하는 변환으로 변환 합니다.

```csharp
public partial class TestPerspectivePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Calculate perspective matrix
        SKMatrix perspectiveMatrix = SKMatrix.MakeIdentity();
        perspectiveMatrix.Persp0 = (float)persp0Slider.Value / 100;
        perspectiveMatrix.Persp1 = (float)persp1Slider.Value / 100;

        // Center of screen
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);
        SKMatrix.PostConcat(ref matrix, perspectiveMatrix);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(xCenter, yCenter));

        // Coordinates to center bitmap on canvas
        float x = xCenter - bitmap.Width / 2;
        float y = yCenter - bitmap.Height / 2;

        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

다음은 몇 가지 예제 이미지입니다.

[![](non-affine-images/testperspective-small.png "테스트 큐브 뷰 페이지의 삼중 스크린샷")](non-affine-images/testperspective-large.png "테스트 큐브 뷰 페이지의 삼중 스크린샷")

슬라이더를 시도할 때 –0.0066 이하일 0.0066 이외의 값은 이미지의 갑자기 분열 된 및 일관 되지 않도록 찾을 수 있습니다. 변환할 비트맵 300 픽셀 사각형입니다. 비트맵의 좌표에서에서 까지의 –150 150 하므로 중심을 기준으로 변환 됩니다. 이전에 설명한 대로 z 값 ' 됩니다.

z` = Persp0·x + Persp1·y + 1

경우 `Persp0` 또는 `Persp1` 0.0066 보다 크면 –0.0066, 아래의 다음은 언제나 있기 마련 a에서 z 발생 하는 비트맵의 일부 좌표 또는 ' 값이 0입니다. 0으로 나누기 일으키는 되 고 렌더링 복잡해 됩니다. 유사 형식이 아닌 변형을 사용할 때 렌더링 하는 0으로 나누기 발생 하는 좌표를 사용 하 여 작업을 방지 하려고 합니다.

설정할 수 없습니다 일반적으로 `Persp0` 및 `Persp1` 격리에서 합니다. 이기도 종종 특정 유형의 유사 형식이 아닌 변형을 달성 하기 위해 행렬의 다른 셀을 설정 해야 합니다.

이러한 유사 형식이 아닌 하나의 변환 되는 *테이퍼 변환*합니다. 이 유형의 유사 형식이 아닌 변형 사각형의 전체 크기를 유지 하지만 한쪽 점점 가늘어지거나:

![](non-affine-images/tapertransform.png "테이퍼 변환의 대상 상자")

[ `TaperTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs) 클래스는 이러한 매개 변수에 따라 유사 형식이 아닌 변환의 일반화 된 계산을 수행 합니다.

- 변환 중인 이미지의 사각형 크기
- 점점 가늘어지거나, 하는 사각형의 면을 지정 하는 열거형
- 어떻게 점점 가늘어지거나 것을 나타내는 다른 열거 하 고
- 테이퍼의 범위입니다.

코드는 다음과 같습니다.

```csharp
enum TaperSide { Left, Top, Right, Bottom }

enum TaperCorner { LeftOrTop, RightOrBottom, Both }

static class TaperTransform
{
    public static SKMatrix Make(SKSize size, TaperSide taperSide, TaperCorner taperCorner, float taperFraction)
    {
        SKMatrix matrix = SKMatrix.MakeIdentity();

        switch (taperSide)
        {
            case TaperSide.Left:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp0 = (taperFraction - 1) / size.Width;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Top:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp1 = (taperFraction - 1) / size.Height;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Right:
                matrix.ScaleX = 1 / taperFraction;
                matrix.Persp0 = (1 - taperFraction) / (size.Width * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        break;
                }
                break;

            case TaperSide.Bottom:
                matrix.ScaleY = 1 / taperFraction;
                matrix.Persp1 = (1 - taperFraction) / (size.Height * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        break;
                }
                break;
        }
        return matrix;
    }
}
```

이 클래스에서 사용 되는 **테이퍼 변환** 페이지. XAML 파일이 두 개를 인스턴스화하고 `Picker` 요소를 사용 하는 열거형 값을 선택 및 `Slider` 테이퍼 분수를 선택 합니다. [ `PaintSurface` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) 처리기 결합 하 여 두 개의 테이퍼 변환 번역 확인 비트맵의 왼쪽 위 모퉁이 기준으로 변환 하는 변환을:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    TaperSide taperSide = (TaperSide)taperSidePicker.SelectedIndex;
    TaperCorner taperCorner = (TaperCorner)taperCornerPicker.SelectedIndex;
    float taperFraction = (float)taperFractionSlider.Value;

    SKMatrix taperMatrix =
        TaperTransform.Make(new SKSize(bitmap.Width, bitmap.Height),
                            taperSide, taperCorner, taperFraction);

    // Display the matrix in the lower-right corner
    SKSize matrixSize = matrixDisplay.Measure(taperMatrix);

    matrixDisplay.Paint(canvas, taperMatrix,
        new SKPoint(info.Width - matrixSize.Width,
                    info.Height - matrixSize.Height));

    // Center bitmap on canvas
    float x = (info.Width - bitmap.Width) / 2;
    float y = (info.Height - bitmap.Height) / 2;

    SKMatrix matrix = SKMatrix.MakeTranslation(-x, -y);
    SKMatrix.PostConcat(ref matrix, taperMatrix);
    SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(x, y));

    canvas.SetMatrix(matrix);
    canvas.DrawBitmap(bitmap, x, y);
}
```

다음은 몇 가지 예입니다.

[![](non-affine-images/tapertransform-small.png "테이퍼 변환 페이지의 삼중 스크린샷")](non-affine-images/tapertransform-large.png "테이퍼 변환 페이지의 삼중 스크린 샷")

다른 유형의 일반화 된 유사 형식이 아닌 변형은 다음 문서에서 설명 하는 3D 회전 [3D 회전](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)합니다.

유사 형식이 아닌 변형 모든 볼록 사각형에 사각형을 변형할 수 있습니다. 이 확인할는 **비 유사 매트릭스 표시** 페이지. 매우 비슷합니다는 **Affine 행렬 표시** 에서 페이지는 [매트릭스를 변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md) 네 번째 있다는 점을 제외 하 고 문서 `TouchPoint` 비트맵의 네 번째 모퉁이 조작 하는 개체:

[![](non-affine-images/shownonaffinematrix-small.png "비 Affine 행렬 표시 페이지의 삼중 스크린샷")](non-affine-images/shownonaffinematrix-large.png "비 Affine 행렬 표시 페이지의 삼중 스크린샷")

프로그램에서이 메서드를 사용 하 여 변환을 성공적으로 계산으로 비트맵의 모서리 중 하나의 내부가 각도 180도 보다 큰지 확인 하거나 양쪽이 서로 교차 하지 마세요는 [ `ShowNonAffineMatrixPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) 클래스:

```csharp
static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL, SKPoint ptLR)
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

    // Non-Affine transform
    SKMatrix inverseA;
    A.TryInvert(out inverseA);
    SKPoint abPoint = inverseA.MapPoint(ptLR);
    float a = abPoint.X;
    float b = abPoint.Y;

    float scaleX = a / (a + b - 1);
    float scaleY = b / (a + b - 1);

    SKMatrix N = new SKMatrix
    {
        ScaleX = scaleX,
        ScaleY = scaleY,
        Persp0 = scaleX - 1,
        Persp1 = scaleY - 1,
        Persp2 = 1
    };

    // Multiply S * N * A
    SKMatrix result = SKMatrix.MakeIdentity();
    SKMatrix.PostConcat(ref result, S);
    SKMatrix.PostConcat(ref result, N);
    SKMatrix.PostConcat(ref result, A);

    return result;
}
```

이 메서드는 계산의 용이성을 위해 이러한 변환으로 비트맵의 네 모퉁이 수정 하는 방법을 보여 주는 화살표가 있는 여기 symbolized는 다음 세 가지 별도 변환의 제품으로 총 변환을 가져옵니다.

(0, 0) → (0, 0) (0, 0) → → (x, 0, y0) (왼쪽 위)

(0, H) → (0, 1) (0, 1) → → (x1 y1) (왼쪽 아래)

(W, 0) (1, 0) → → (1, 0) → (x2 y2) (오른쪽 위)

(W, H) → (1, 1) (a, b) → → (x3 y3) (오른쪽)

오른쪽에 있는 마지막 좌표는 4 개의 터치 포인트와 관련 된 네 개의 점입니다. 이들은 비트맵의 모서리 중 마지막 좌표입니다.

W 및 H 비트맵의 높이 너비를 나타냅니다. 첫 번째 변환 (`S`) 단순히 1 픽셀 사각형으로 비트맵 크기를 조정 합니다. 두 번째 변환이 유사 형식이 아닌 변형 `N`, 세 번째는 3x3 유사 변환 및 `A`합니다. 이전 관계 처럼가 되도록 해당 유사 변환 점이 3 개 기반의 [ `ComputeMatrix` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) 메서드 및와 네 번째 행이 포함 되어 있지는 (a, b) 지점입니다.

`a` 및 `b` 세 번째 변환이 유사 하 값이 계산 됩니다. 코드 유사 변환의 역 수를 가져오고이 사용 하 여 오른쪽 아래 모서리에 매핑할 합니다. (A, b) 지점입니다.

3 차원 그래픽을 모방 하기 위해 유사 형식이 아닌 변형의 또 다른 용도 합니다. 다음 문서에서 [3D 회전](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md) 3D 공간에서 2 차원 그래픽 회전 하는 방법을 참조 하십시오.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
