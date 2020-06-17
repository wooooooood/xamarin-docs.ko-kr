---
제목: "비 상관 변환" 설명: "이 문서에서는 transform 행렬의 세 번째 열을 사용 하 여 원근 및 테이퍼 효과를 만드는 방법에 대해 설명 하 고 샘플 코드를 사용 하 여이를 보여 줍니다."
ms. prod: xamarin. 기술: xamarin-skiasharp assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1 author: davidbritch: dabritch: 04/14/2017:-loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="non-affine-transforms"></a>비아핀(Non-Affine) 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Transform 행렬의 세 번째 열을 사용 하 여 큐브 뷰 및 테이퍼 효과 만들기_

변환, 크기 조정, 회전 및 기울이기는 모두 *관계* 변환으로 분류 됩니다. 상관 변환은 병렬 선을 유지 합니다. 변환 전에 두 줄이 평행이 면 변환 후에도 병렬 상태로 유지 됩니다. 사각형은 항상 parallelograms로 변환 됩니다.

그러나 SkiaSharp는 사각형을 볼록 사변형 변환 하는 기능을 포함 하는 비 상관 변환도 가능 합니다.

![](non-affine-images/nonaffinetransformexample.png "A bitmap transformed into a convex quadrilateral")

볼록 사변형는 서로 교차 하지 않는 항상 180도 보다 작은 내부 각도를 가진 네 면의 그림입니다.

Transform 행렬의 세 번째 행이 0, 0, 1 이외의 값으로 설정 된 경우 비 상관 변환 결과가 발생 합니다. 전체 `SKMatrix` 곱하기는 다음과 같습니다.

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

결과 변환 수식은 다음과 같습니다.

x ' = ScaleX · x + SkewX · y + TransX

y ' = SkewY · x + ScaleY · y + TransY

z ' = Persp0 · x + Persp1 · y + Persp2

2 차원 변환에 3 x 3 행렬을 사용 하는 기본 규칙은 Z가 1 인 평면에 모든 항목을 유지 한다는 것입니다. `Persp0`및 `Persp1` 가 0이 고가 `Persp2` 1 이면 변환에서 Z 좌표를 해당 평면 밖으로 이동 했습니다.

이를 2 차원 변환으로 복원 하려면 좌표를 다시 해당 평면으로 이동 해야 합니다. 다른 단계가 필요 합니다. X ', y ' 및 z ' 값은 z '로 구분 해야 합니다.

x "= x '/z '

y "= y '/z '

z "= z '/z ' = 1

이는 유형이 같은 *좌표* 이며 Mathematician 8 월 Ferdinand Möbius에 의해 개발 되었으며, 해당 토폴로지 Oddity, Möbius 스트립에 대해 훨씬 더 잘 알려져 있습니다.

Z '가 0 이면 나누기가 무한 좌표를 반환 합니다. 실제로 유형이 같은 좌표 개발에 대 한 Möbius's 동기 중 하나는 유한 숫자로 무한 값을 나타내는 기능 이었습니다.

그러나 그래픽을 표시 하는 경우 무한 값으로 변환 되는 좌표를 사용 하 여 무언가를 렌더링 하지 않으려는 경우 이러한 좌표는 렌더링 되지 않습니다. 이러한 좌표 근처의 모든 항목은 매우 크고 시각적으로 일관 되지 않을 수 있습니다.

이 수식에서는 z '의 값이 0이 되지 않도록 합니다.

z ' = Persp0 · x + Persp1 · y + Persp2

따라서 이러한 값에는 몇 가지 실제적인 제한이 있습니다. 

`Persp2`셀은 0 또는 0이 될 수 없습니다. `Persp2`가 0 이면 z '는 점 (0, 0)에 대해 0이 고,이는 2 차원 그래픽에서 매우 일반적 이기 때문에 일반적으로 바람직하지 않습니다. `Persp2`가 0과 같지 않으면 `Persp2` 가 1에서 고정 된 경우 일반 성을가 손실 되지 않습니다. 예를 들어를 5로 설정 하는 경우 `Persp2` 행렬의 모든 셀을 5로 나눌 수 있습니다. 그러면이 값이 1로 설정 되 `Persp2` 고 결과가 동일 하 게 됩니다.

이러한 이유로 `Persp2` 는 id 매트릭스의 동일한 값인 1에서 수정 되는 경우가 많습니다.

일반적으로 `Persp0` 및 `Persp1` 는 작은 숫자입니다. 예를 들어 id 행렬로 시작 하지만를 0.01로 설정 했다고 가정 합니다 `Persp0` .

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

변환 수식은 다음과 같습니다.

x ' = x/(0.01 · x + 1)

y ' = y/(0.01 · x + 1)

이제이 변환을 사용 하 여 원래 위치에 100 픽셀 정사각형 상자를 렌더링 합니다. 네 개의 모퉁이가 어떻게 변환 되는지는 다음과 같습니다.

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

X가 100 인 경우 z ' 분모는 2 이므로 x 및 y 좌표가 효과적으로 반. 상자의 오른쪽이 왼쪽 보다 짧습니다.

![](non-affine-images/nonaffinetransform.png "A box subjected to a non-affine transform")

`Persp`이러한 셀 이름의 부분은 "큐브 뷰"를 참조 합니다. 그러면 단축법은 상자를 뷰어에서 더 오른쪽으로 기울어져 있음을 나타냅니다.

**테스트 큐브 뷰** 페이지를 사용 하 여 및 값을 시험해 보고 `Persp0` `Pers1` 작동 방식에 대 한 느낌을 얻을 수 있습니다. 이러한 행렬 셀의 적절 한 값은 유니버설 Windows 플랫폼의에서 `Slider` 적절 하 게 처리할 수 없는 크기입니다. UWP 문제를 수용 하려면 `Slider` [**testperspective. xaml**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) 의 두 요소를-1에서 1로 초기화 해야 합니다.

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

코드를 만든 파일의 슬라이더에 대 한 이벤트 처리기는 [`TestPerspectivePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) -0.01과 0.01 사이에 있는 값을 100로 나눕니다. 또한 생성자는 비트맵에서 로드 됩니다.

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

`PaintSurface`처리기는 `SKMatrix` `perspectiveMatrix` 이러한 두 슬라이더의 값을 100로 나눈 값을 기반으로 하는 라는 값을 계산 합니다. 이 변환의 중심을 비트맵의 중심에 배치 하는 두 개의 변환 변환과 결합 됩니다.

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

다음은 몇 가지 샘플 이미지입니다.

[![](non-affine-images/testperspective-small.png "Triple screenshot of the Test Perspective page")](non-affine-images/testperspective-large.png#lightbox "Triple screenshot of the Test Perspective page")

슬라이더를 사용 하 여 시험해 보면 0.0066 이하의 값이 보다 0.0066 작은 값을 찾으면 이미지가 갑자기 fractured 및 incoherent 됩니다. 변환 중인 비트맵이 300 픽셀 제곱입니다. 이는 중심을 기준으로 변형 되므로 비트맵의 좌표는-150에서 150 까지입니다. Z 값은 다음과 같습니다.

z ' = Persp0 · x + Persp1 · y + 1

`Persp0`또는 `Persp1` 가 0.0066 보다 크거나 0.0066 보다 큰 경우에는 항상 z ' 값이 0 인 비트맵이 있습니다. 이렇게 하면 0으로 나누기가 발생 하 고 렌더링은 복잡해 집니다. 비 관계 변환을 사용할 때 0으로 나누기를 발생 시키는 좌표를 사용 하 여 아무것도 렌더링 하지 않으려는 경우

일반적으로 및는 격리에서 설정 하지 않습니다 `Persp0` `Persp1` . 행렬의 다른 셀을 설정 하 여 특정 유형의 비 관계 변환을 수행 해야 하는 경우가 종종 있습니다.

이러한 비 상관 변환 중 하나는 *테이퍼 변형*입니다. 이러한 비 상관 변환 형식은 사각형의 전체 크기를 유지 하지만 한쪽은 tapers 합니다.

![](non-affine-images/tapertransform.png "A box subjected to a taper transform")

[`TaperTransform`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs)클래스는 다음 매개 변수를 기반으로 비 상관 변환의 일반화 된 계산을 수행 합니다.

- 변환 되는 이미지의 사각형 크기입니다.
- tapers 하는 사각형의 측면을 나타내는 열거형입니다.
- tapers 방법을 나타내는 또 다른 열거형
- tapering의 범위입니다.

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

이 클래스는 **테이퍼 변형** 페이지에서 사용 됩니다. XAML 파일은 두 개의 `Picker` 요소를 인스턴스화하여 열거형 값을 선택 하 고, `Slider` 테이퍼 분수를 선택 합니다. 처리기는 변환 변환을 [`PaintSurface`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) 두 개의 변환으로 결합 하 여 비트맵의 왼쪽 위 모퉁이를 기준으로 변환을 수행 합니다.

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

예를 들어 다음과 같은 노래를 선택할 수 있다.

[![](non-affine-images/tapertransform-small.png "Triple screenshot of the Taper Transform page")](non-affine-images/tapertransform-large.png#lightbox "Triple screenshot of the Taper Transform page")

다른 형식의 일반화 된 비 관계 변환은 3d 회전으로, 다음 문서인 [**3d**](3d-rotation.md)회전입니다.

비 상관 변환은 사각형을 볼록 사변형 변환할 수 있습니다. 이는 **비 상관 행렬 표시** 페이지에서 보여 줍니다. 비트맵의 네 번째 모퉁이를 조작 하는 네 번째 개체를 포함 한다는 점을 제외 하 고는 [**행렬 변환**](matrix.md) 문서의 **관계 행렬 표시** 페이지와 매우 비슷합니다 `TouchPoint` .

[![](non-affine-images/shownonaffinematrix-small.png "Triple screenshot of the Show Non-Affine Matrix page")](non-affine-images/shownonaffinematrix-large.png#lightbox "Triple screenshot of the Show Non-Affine Matrix page")

비트맵의 모퉁이 중 하나에 대 한 내부 각도를 180도 보다 크게 설정 하거나 두 면을 서로 교차 하도록 설정 하지 않으면 프로그램은 클래스에서이 메서드를 사용 하 여 변환을 성공적으로 계산 합니다 [`ShowNonAffineMatrixPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) .

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

계산의 용이성을 위해이 메서드는 세 개의 개별 변환의 곱으로 전체 변환을 가져옵니다. 여기서는 이러한 변환에서 비트맵의 네 모퉁이를 수정 하는 방법을 보여 주는 화살표가 표시 됩니다.

(0, 0) → (0, 0) → (0, 0) → (x0, y0) (왼쪽 위)

(0, H) → (0, 1) → (0, 1) → (x1, y1) (왼쪽 아래)

(W, 0) → (1, 0) → (1, 0) → (x2, y2) (오른쪽 위)

(W, H) → (1, 1) → (a, b) → (x3, y3) (오른쪽 아래)

오른쪽에 있는 최종 좌표는 네 개의 터치 지점과 연결 된 네 개의 점에 해당 합니다. 이는 비트맵 모퉁이의 최종 좌표입니다.

W 및 H는 비트맵의 너비와 높이를 나타냅니다. 첫 번째 변환은 `S` 단순히 비트맵을 1 픽셀 사각형으로 크기 조정 합니다. 두 번째 변환은 비 상관 변환 이며 `N` 세 번째 변환은 상관 변환입니다 `A` . 상관 변환은 세 개의 점을 기반으로 하므로 이전 관계 [`ComputeMatrix`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) 메서드와 동일 하며, (a, b) 점이 있는 네 번째 행을 포함 하지 않습니다.

`a` `b` 세 번째 변환이 상관 관계를 갖도록 및 값이 계산 됩니다. 이 코드는 관계 변환의 역함수를 가져온 다음이를 사용 하 여 오른쪽 아래 모퉁이를 매핑합니다. 이것은 point (a, b)입니다.

비 상관 변환의 또 다른 용도는 3 차원 그래픽을 모방 하는 것입니다. 다음 문서에서 [**3D 회전**](3d-rotation.md) 은 3d 공간에서 2 차원 그래픽을 회전 하는 방법을 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
