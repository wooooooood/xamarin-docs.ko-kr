---
title: 비아핀(Non-Affine) 변환
description: 이 문서는 변환 행렬의 세 번째 열을 사용 하 여 큐브 뷰 및 테이퍼 효과 만드는 방법에 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2017
ms.openlocfilehash: da820b0c48eaec52da76504b1aed8e9793c1e74d
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057274"
---
# <a name="non-affine-transforms"></a>비아핀(Non-Affine) 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_변환 행렬의 세 번째 열을 사용 하 여 큐브 뷰 및 테이퍼 효과 만들기_

변환, 크기 조정, 회전 및 기울이기 모두으로 분류 됩니다 *affine* 변환 합니다. Affine 변환만 평행선을 유지합니다. 두 줄을 변환 하기 전에 병렬 경우 이후에 유지 병렬 변환 합니다. 사각형은 항상 parallelograms로 변환 됩니다.

그러나 SkiaSharp 사각형 모든 볼록 사변형으로 변환 하는 기능을 포함 하는 비 관계 변환 수 이기도 합니다.

![](non-affine-images/nonaffinetransformexample.png "볼록 사변형으로 변환 하는 비트맵")

볼록 사변형 서로 교차 하지 변과 180도 보다 항상 작거나 내부 각도 사용 하 여 네 방향 그림입니다.

비 관계 변환 결과 변환 행렬의 셋째 행 0, 0 및 1 이외의 값으로 설정 된 경우. 전체 `SKMatrix` 곱셈은:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

결과 변환 수식은 다음과 같습니다.

x' ScaleX·x + SkewX·y TransX =

y' SkewY·x + ScaleY·y TransY =

z' Persp0·x + Persp1·y Persp2 =

3x3 매트릭스를 사용 하 여 2 차원 변환에 대 한 기본 규칙은 모든 하 게 유지 되도록 평면의 Z = 1입니다. 경우가 아니면 `Persp0` 하 고 `Persp1` 는 0으로, 및 `Persp2` 가 1 이면 변환 평면 해제 Z 좌표 이동 되었습니다.

이 2 차원 변환에 복원 하는 평면에 좌표를 다시 이동 해야 합니다. 다른 단계는 필요 합니다. X', y'를 z '값으로 나누어야 z' 및:

x" = x' / z'

y" = y' / z'

z" = z' / z' = 1

이러한 라고 *동차 좌표* 하므로 개발 된 토폴로지 자신의 문제에 대 한 훨씬 잘 알려진 년 8 월 페르디난드 Möbius 수학적으로 Möbius 줄무늬 및 합니다.

하는 경우 z'가 0 이면 무한 좌표로 나누기 결과입니다. 사실, 동차 좌표를 개발 하기 위한 Möbius의 동기 중 하나 였습니다 유한 숫자를 사용 하 여 무한 한 값을 나타내는 수 있습니다.

그러나 그래픽을 표시 하는 경우에 무한 값으로 변환 하는 좌표를 사용 하 여 항목을 렌더링 하지 않으려는 합니다. 이러한 좌표는 렌더링 되지 않습니다. 좌표 주변의 모든 될 수 있으며 매우 큰 시각적으로 일관 되지 않을 수 있습니다.

이 수식에서 원하지 않는 z 값 ' 0이 되 고 있습니다.

z' Persp0·x + Persp1·y Persp2 =

따라서 이러한 값에는 몇 가지 실질적인 제한 사항이 있습니다. 

`Persp2` 0 또는 0이 아니면 셀 일 수 있습니다. 경우 `Persp2` 이 0 이면 z'는 일반적으로 필요 없는 해당 지점 2 차원 그래픽에서 매우 일반적 이므로 및 점 (0, 0)에 대해서는 0입니다. 하는 경우 `Persp2` 하는 경우 일반 성의 손실 없이 0과 같지 않은 `Persp2` 1 고정 됩니다. 예를 들어 확인 된 경우 `Persp2` 나눌 수 있습니다 단순히 행렬의 모든 셀 5는 다음 5 해야 `Persp2` 1 및 결과 동일 합니다.

이러한 이유로, `Persp2` 고정 됩니다. 대개 1은 항등 매트릭스에서 동일한 값이 있습니다.

일반적으로 `Persp0` 고 `Persp1` 는 작은 숫자입니다. 예를 들어를 항등 매트릭스 있지만 집합을 사용 하 여 먼저 `Persp0` 0.01로:

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

변환 하는 수식을 다음과 같습니다.

x' = x / (0.01·x + 1)

y' = y / (0.01·x + 1)

이제이 변환을 사용 하 여 원점에 배치 100 픽셀 사각형 상자를 렌더링 합니다. 네 모퉁이 변환 하는 방법을 다음과 같습니다.

(0, 0) (0, 0) 하는 경우 →

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

100을 z x 표시 되는 경우 ' 분모 이므로 2 x 및 y 좌표 효과적으로 줄었고 됩니다. 오른쪽 상자에 있는 왼쪽 보다 짧은 됩니다.

![](non-affine-images/nonaffinetransform.png "비 관계 변환에 적용 하는 상자")

`Persp` 상자 오른쪽에 있는 뷰어를 멀리를 사용 하 여 이제 기울어진는 제안 된 단축법 때문에 이러한 셀 이름 부분은 "관점"를 의미 합니다.

합니다 **테스트 큐브 뷰** 페이지의 값을 실험할 수 있습니다 `Persp0` 고 `Pers1` 작동 방식을 이해할 수 있도록 합니다. 이러한 행렬 셀의 적절 한 값은 작기는 `Slider` 유니버설 Windows 플랫폼에서 없습니다 올바르게 처리 합니다. 두 UWP 문제에 맞게 `Slider` 의 요소를 [ **TestPerspective.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) 1로-1에서 범위를 초기화 해야:

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

슬라이더에 대 한 이벤트 처리기를 [ `TestPerspectivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) –0.01 및 0.01 사이 있도록 코드 숨김 파일 100으로 값을 나눕니다. 또한 생성자는 비트맵에 로드 됩니다.

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
        {
            bitmap = SKBitmap.Decode(stream);
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

합니다 `PaintSurface` 처리기에서 계산을 `SKMatrix` 라는 값 `perspectiveMatrix` 100으로 나눈 다음 두 개의 슬라이더의 값을 기반으로 합니다. 이 함께 두 translate 변환을 비트맵의 센터에서이 변환의 중심을 둔:

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

몇 가지 샘플 이미지는 다음과 같습니다.

[![](non-affine-images/testperspective-small.png "테스트 큐브 뷰 페이지의 3 배가 스크린샷")](non-affine-images/testperspective-large.png#lightbox "삼중 테스트 큐브 뷰 페이지 스크린샷")

슬라이더를 사용 하 여 실험 시 0.0066 초과 또는 –0.0066 아래 값 때문에 이미지 갑자기 해지고 분열 된 경도의 있는지 확인할 수 있습니다. 변환할 비트맵은 300 픽셀 사각형이 됩니다. 비트맵의 좌표를 150 –150에서 범위 있도록의 중심을 기준으로 변환 됩니다. 이전에 설명한 대로 z 값 ' 됩니다.

z' Persp0·x Persp1·y + 1 =

하는 경우 `Persp0` 또는 `Persp1` 0.0066 보다 크면 없거나 –0.0066, 아래 다음 항상 z를 초래 하는 비트맵의 일부 좌표 ' 값이 0입니다. 0으로 나누기 되도록 되며 렌더링 교통이 복잡 합니다. 비 관계 변환을 사용 하는 경우 0으로 나누기는 좌표를 사용 하 여 아무 것도 렌더링 하지 않도록 해야 합니다.

설정할 수 없습니다 일반적으로 `Persp0` 고 `Persp1` 격리에서 합니다. 것도 종종 특정 유형의 비 관계 변환 달성 하기 위해 행렬의 다른 셀을 설정 해야 합니다.

이러한 하나의 비 관계 변환 되는 *테이퍼 변환*합니다. 이 유형의 비 관계 변환 사각형의 전체적인 치수는 유지 되지만 한쪽 점점 가늘어지거나:

![](non-affine-images/tapertransform.png "테이퍼 변환을 적용 하는 상자")

합니다 [ `TaperTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs) 클래스는 이러한 매개 변수에 따라 비 관계 변환의 일반화 된 계산을 수행 합니다.

- 변환 중인 이미지의 사각형 크기
- 점점 가늘어지거나에 사각형의 면을 나타내는 열거형
- 어떻게 점점 가늘어지거나 것을 나타내는 다른 열거형 및
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

이 클래스에서 사용 되는 **테이퍼 변환** 페이지입니다. XAML 파일을 두 개를 인스턴스화하고 `Picker` 열거형 값을 선택 하는 요소 및 `Slider` 테이퍼 비율을 선택 합니다. 합니다 [ `PaintSurface` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) 처리기 결합 테이퍼 변환을 두 개의 변환 비트맵의 왼쪽 위 모퉁이 기준으로 변환 되도록 변환 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    TaperSide taperSide = (TaperSide)taperSidePicker.SelectedItem;
    TaperCorner taperCorner = (TaperCorner)taperCornerPicker.SelectedItem;
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

[![](non-affine-images/tapertransform-small.png "삼중 테이퍼 변환 페이지 스크린샷")](non-affine-images/tapertransform-large.png#lightbox "삼중 테이퍼 변환 페이지 스크린샷")

다른 유형의 일반화 된 비 관계 변환은 다음 기사에서 설명 하는 3D 회전 [ **3D 회전**](3d-rotation.md)합니다.

비 관계 변환 모든 볼록 사변형에 사각형을 변환할 수 있습니다. 이 확인할 합니다 **비 Affine 행렬 표시** 페이지입니다. 매우 유사 합니다 **Affine 행렬 표시** 에서 페이지를 [ **매트릭스 변환** ](matrix.md) 네 번째 있는 점을 제외 하 고 문서 `TouchPoint` 네 번째를 조작 하는 개체 비트맵의 모퉁이.

[![](non-affine-images/shownonaffinematrix-small.png "비 Affine 행렬 표시 페이지 스크린샷 삼중")](non-affine-images/shownonaffinematrix-large.png#lightbox "삼중 비 Affine 행렬 표시 페이지 스크린샷")

프로그램에서이 메서드를 사용 하 여 변환을 성공적으로 계산 하지을 하려고 하면 비트맵의 모서리 중 하나를 내부 각도가 180도 보다 큰지 확인 하거나 서로 교차 하는 두 변으로는 [ `ShowNonAffineMatrixPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) 클래스.

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

계산 편의성을 위해이 메서드는 이러한 변환 된 비트맵의 네 모퉁이 수정 하는 방법을 보여 주는 화살표가 여기 기호로 나타낼는 세 가지 별도 변환의 결과로 총 변환을 가져옵니다.

(0, 0) (0, 0) → → (0, 0) → (0, y0) x (왼쪽 위)

(0, H) → (0, 1) (0, 1) → → (x1 y1) (왼쪽)

(W, 0) (1, 0) → → (1, 0) → (x2 y2) (오른쪽 위)

(H, W) → (1, 1) (a, b) → → (x3 y3) (오른쪽)

오른쪽에 있는 마지막 좌표 지점은 4 4 개의 터치 포인트를 사용 하 여 연결 합니다. 이들은 비트맵의 모서리 중 마지막 좌표입니다.

W 및 H 비트맵의 높이 너비를 나타냅니다. 첫 번째 변환을 `S` 1 픽셀 사각형으로 비트맵 크기를 조정 하기만 하면 됩니다. 두 번째 변환이 비 관계 변환 `N`, 세 번째 유사 변환 이며 `A`합니다. 이전 affine 마찬가지로 있으므로 해당 유사 변환 세 가지 요소에 기반 [ `ComputeMatrix` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) 메서드 및 네 번째 행이 포함 되어 있지는 (a, b) 지점입니다.

합니다 `a` 고 `b` 세 번째 변환이 유사 변환이 있도록 값이 계산 됩니다. 코드는 유사 변환의 역함수 값을 가져옵니다 하 고 오른쪽 아래 모서리를 매핑하는. (A, b) 지점입니다.

3 차원 그래픽을 모방 하는 데 비 관계 변환의 다른 사용이 됩니다. 다음 문서에서는 [ **3D 회전** ](3d-rotation.md) 3D 공간에서 2 차원 그래픽을 회전 하는 방법을 참조 하세요.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
