---
title: SkiaSharp의 3D 회전
description: 이 문서에서는 비 관계 변환을 사용 하 여 3D 공간에서 2D 개체를 회전 하는 방법에 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2017
ms.openlocfilehash: 7ac9ec458f16357ef50e23c459a9b0e1f79bdd97
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61349033"
---
# <a name="3d-rotations-in-skiasharp"></a>SkiaSharp의 3D 회전

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_비 관계 변환을 사용 하 여 3D 공간에서 2D 개체를 회전 합니다._

비 관계 변환에 대 한 일반적인 응용 프로그램 3D 공간에서 2D 개체의 회전을 시뮬레이션 합니다.

![](3d-rotation-images/3drotationsexample.png "3D 공간에서 텍스트 문자열 회전")

이 작업에서는 3 차원 회전을 사용 하 여 작업 및 비 관계을 파생 `SKMatrix` 이러한 3D 회전을 수행 하는 변환입니다.

이 개발 하는 것이 어렵습니다 `SKMatrix` 변형 2 차원 내 에서만 작동 합니다. 이 3으로 3 행렬 3D 그래픽에서 사용 되는 4x4 매트릭스에서 파생 되는 경우 작업이 훨씬 쉬워집니다. SkiaSharp 포함 된 [ `SKMatrix44` ](xref:SkiaSharp.SKMatrix44) 이 있지만 약간의 경험이 3D 그래픽에 대 한 클래스는 3D 회전 및 4-4 변환 매트릭스를 이해 하는 데 필요 합니다.

개념적으로 Z. 라는 세 번째 축을 추가 하는 3 차원 좌표계는 Z 축을 화면 오른쪽 각도에. 3D 공간의 점 좌표는 세 자리 숫자를 사용 하 여 표시 됩니다: (x, y, z). 3d에서 X의 값을 늘리면이 문서에서는 사용 하는 좌표계 오른쪽 되며 증가 Y 값 두 개의 차원에서와 마찬가지로 아래로 이동 합니다. 양수 Z 값 증가 화면에서 제공 됩니다. 원점은 2D 그래픽 마찬가지로 왼쪽 위 모퉁이 있습니다. 이 평면에 오른쪽 각도 Z 축은 XY 평면으로 화면의 생각할 수 있습니다.

이 왼쪽 좌표계 라고 합니다. 에 왼쪽의 X 좌표 (오른쪽)를 양의 방향에서에 대 한 엄지와 가리키고 손가락은 Y 증가 하는 방향 (아래쪽) 조정 하는 경우 다음에 thumb의에서 지점 Z 좌표가 늘어나는 방향을-에서 초과 확장 화면입니다.

3D 그래픽에서 변환-4x4 행렬을 기반으로 합니다. 4-4 항등 매트릭스는 다음과 같습니다.

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

4x4 행렬을 사용 하 여 작업을 해당 행 및 열 번호를 사용 하 여 셀을 식별 하기에 편리한으로:

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

그러나는 SkiaSharp `Matrix44` 클래스는 약간 다릅니다. 설정 하거나 개별 셀 값을 가져올 유일한 방법은 `SKMatrix44` 사용 하는 것은 [ `Item` ](xref:SkiaSharp.SKMatrix44.Item(System.Int32,System.Int32)) 인덱서 합니다. 행 및 열 인덱스는 0부터 시작 하지 않고 1부터 시작 하 고 행과 열 교환 됩니다. 위의 다이어그램에 있는 셀 M14 인덱서를 사용 하 여 액세스할 `[3, 0]` 에 `SKMatrix44` 개체입니다.

3D 그래픽 시스템에서는 3D 지점 (x, y, z)는 4x4 변환 매트릭스를 곱하는 대 한 1-4 매트릭스도 변환 됩니다.

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

2D 유사 변환 발생 하는 3 차원에서 3D 변환 4 차원에서으로 간주 됩니다. 네 번째 차원의 ' W ' 라고 하 고 3D 공간 W 좌표 1에 일치 하는 4 차원 공간 내에 존재로 간주 됩니다. 변환 수식에는 다음과 같습니다.

`x' = M11·x + M21·y + M31·z + M41`

`y' = M12·x + M22·y + M32·z + M42`

`z' = M13·x + M23·y + M33·z + M43`

`w' = M14·x + M24·y + M34·z + M44`

변환 수식에서 명확 하는 셀 `M11`, `M22`, `M33` X, Y 및 Z 방향의 배율 인수는 및 `M41`를 `M42`, 및 `M43` X, Y 및 Z의 번역 요인이 방법을 설명 합니다.

이러한 좌표는 W가 1 x 3D 공간에 다시 변환할 ', y'를 z '좌표는 모든 나눈 w' 및:

`x" = x' / w'`

`y" = y' / w'`

`z" = z' / w'`

`w" = w' / w' = 1`

나누기 w' 3D 공간에서 큐브 뷰를 제공 합니다. 경우 w'가 1 다음 없습니다 관점에서 발생 합니다.

3D 공간에서 회전은 매우 복잡할 수 있지만 가장 간단한 회전은 X, Y 및 Z 축을 기준입니다. X 축 중심으로 α 각도의 회전은이 매트릭스입니다.

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

X의 값이이 변환에 적용 하는 경우 동일 하 게 유지 합니다. Y 축 중심으로 회전 변경 하지 않고 Y의 값을 유지:

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Z 축 중심으로 회전 2D 그래픽와 동일 합니다.

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

회전의 방향은 좌표계의 선호도 포함 됩니다. 왼쪽 시스템입니다 증가 값 특정 축 방향으로 왼쪽의 엄지 단추를 가리킬 경우-X 축 중심으로 회전 오른쪽 회전 Y 축 및 사용자 쪽 Z 축 중심으로 회전에 대 한 아래쪽-다음의 곡선 yo 다른 손가락의 회전 양의 각도 방향을 나타냅니다.

`SKMatrix44` 정적 일반화 [ `CreateRotation` ](xref:SkiaSharp.SKMatrix44.CreateRotation(System.Single,System.Single,System.Single,System.Single)) 하 고 [ `CreateRotationDegrees` ](xref:SkiaSharp.SKMatrix44.CreateRotationDegrees(System.Single,System.Single,System.Single,System.Single)) 축 회전 중심점입니다 발생을 지정할 수 있도록 하는 메서드:

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

X 축 중심으로 회전에서 처음 세 개의 인수 1, 0, 0로 설정 합니다. Y 축 중심으로 회전에서 0, 1, 0으로 설정 하 고 Z 축 중심으로 회전, 0, 0, 1로 설정 합니다.

네 번째 열은 4-4의 관점입니다. `SKMatrix44` 원근 변환을 만드는 메서드가 없는 있지만 직접 다음 코드를 사용 하 여 만들 수 있습니다.

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

인수 이름에 대 한 이유 `depth` 곧 분명 하 게 됩니다. 해당 코드는 행렬을 만듭니다.

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

W 다음 계산의 결과 변환 하는 수식을 ':

`w' = –z / depth + 1`

이 z 값 0 보다 작은 경우 (개념적 뒤 XY 평면) 하는 경우 X 및 Y 좌표를 줄이기 위해 및 Z의 양수 값에 대 한 X 및 Y 좌표를 높이기 위해 사용 됩니다. Z 좌표 인 경우 `depth`, 다음 w'는 0이 고 좌표 될 무한 합니다. 3 차원 그래픽 시스템은 카메라 비유를 기반으로 구축 및 `depth` 값 좌표계의 원점을에서 카메라의 거리를 나타냅니다. 그래픽 개체는 Z 좌표를 즉 있는지 `depth` 단위 원점에서 카메라의 렌즈 닿는 개념적 고 무한정 커지면 합니다.

아마도 사용할 것이 점을 염두 `perspectiveMatrix` 값과 함께 회전 행렬입니다. 회전할 그래픽 개체에 보다 큰 X 또는 Y 좌표 `depth`, 3D 공간에서이 개체의 회전은 보다 큰 Z 좌표를 포함 하는 일을 할 `depth`합니다. 이 방지할 수 있어야 합니다. 만들면 `perspectiveMatrix` 설정할 `depth` 그래픽 개체를 회전 하는 방법에 관계 없이 모든 좌표에 충분히 큰 값으로. 이 0으로 나누기 하지는 되도록 합니다.

3D 회전 및 큐브 뷰를 결합 하 여 4x4 행렬을 함께 곱한 필요 합니다. 이 작업을 위해 `SKMatrix44` 연결 메서드를 정의 합니다. 하는 경우 `A` 하 고 `B` 는 `SKMatrix44` 다음 다음 코드는 × b:에는 등호를 설정 하는 개체

```csharp
A.PostConcat(B);
```

4-4 변환 매트릭스를 2D 그래픽 시스템에서 사용할 2D 개체에 적용 됩니다. 이러한 개체는 평면 및 0의 Z 좌표를 할 것으로 간주 됩니다. 변환 곱셈은 앞에 표시 된 변환 보다 좀 더 간단 합니다.

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

행렬의 셋째 행의 모든 셀을 포함 하지 않는 변환 수식에서 z 결과 0 값:

x' = M11·x + M21·y + M41

y' = M12·x + M22·y + M42

z' = M13·x + M23·y + M43

w' = M14·x + M24·y + M44

또한 z' 좌표 관련이 없는 여기도 합니다. 3D 개체는 2 차원 그래픽 시스템에서 표시 되 면 Z 좌표 값을 무시 하 여 2 차원 개체를 축소 됩니다. 변환 하는 수식을 실제로 이러한 두 같습니다.

`x" = x' / w'`

`y" = y' / w'`

즉, 세 번째 행 *고* -4x4 행렬의 세 번째 열을 무시할 수 있습니다.

하는 경우, 이유 있지만 필요한-4x4 행렬을 처음부터 인가요?

세 번째 행과 세 번째 열은 4-4의 2 차원 변환, 세 번째 행 및 열에 대 한 관련 되지 않지만 *수행* 때 하기 전에 역할을 다양 한 `SKMatrix44` 값 곱합니다. 예를 들어 곱하는 원근 변환 사용 하 여 Y 축 중심으로 회전 하는 것으로 가정 합니다.

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

제품에서 셀 `M14` 이제 관점을 포함 합니다. 2D 개체에는 행렬을 적용 하려는 경우 세 번째 행과 열에 3-3 행렬으로 변환할 제거 됩니다.

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

이제는 2 차원 점의 변환을 사용할 수 있습니다.

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

변환 하는 수식을 다음과 같습니다.

`x' = cos(α)·x`

`y' = y`

`z' = (sin(α)/depth)·x + 1`

이제 z 나눌 모든 ':

`x" = cos(α)·x / ((sin(α)/depth)·x + 1)`

`y" = y / ((sin(α)/depth)·x + 1)`

Y 축 중심을 양의 양의 각도 사용 하 여 2D 개체는 회전 하는 경우 X 값 오목 하 게 표시 부정 하는 동안 배경에 전경으로 제공 되는 X 값입니다. X 값을 더 작은 또는 뷰어를 멀리 이동할 때 더 큰 가깝게 됩니다 좌표로 Y 축에서 가장 먼 곳 (코사인 값을 적용은)는 Y 축에 가깝게 이동할 것 같습니다.

사용 하는 경우 `SKMatrix44`, 다양 한 곱하여 모든 3D 회전 및 큐브 뷰 작업을 수행할 `SKMatrix44` 값입니다. 4에서 4에서 2 차원-3x3 매트릭스를 추출할 수 있습니다 다음 매트릭스를 사용 하 여는 [ `Matrix` ](xref:SkiaSharp.SKMatrix44.Matrix) 의 속성을 `SKMatrix44` 클래스입니다. 이 속성은 익숙한 반환 `SKMatrix` 값입니다.

합니다 **회전 3D** 페이지 3D 회전을 시험해 볼 수 있습니다. 합니다 [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) 파일은 4 개의 슬라이더 X, Y 및 Z 축 기준으로 회전을 설정 하 고 깊이 값을 설정 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.Rotation3DPage"
             Title="Rotation 3D">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
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
                    <Setter Property="Margin" Value="20, 0" />
                    <Setter Property="Maximum" Value="360" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="xRotateSlider"
                Grid.Row="0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference xRotateSlider},
                              Path=Value,
                              StringFormat='X-Axis Rotation = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="yRotateSlider"
                Grid.Row="2"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference yRotateSlider},
                              Path=Value,
                              StringFormat='Y-Axis Rotation = {0:F0}'}"
               Grid.Row="3" />

        <Slider x:Name="zRotateSlider"
                Grid.Row="4"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zRotateSlider},
                              Path=Value,
                              StringFormat='Z-Axis Rotation = {0:F0}'}"
               Grid.Row="5" />

        <Slider x:Name="depthSlider"
                Grid.Row="6"
                Maximum="2500"
                Minimum="250"
                ValueChanged="OnSliderValueChanged" />

        <Label Grid.Row="7"
               Text="{Binding Source={x:Reference depthSlider},
                              Path=Value,
                              StringFormat='Depth = {0:F0}'}" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="8"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

`depthSlider` 초기화 되는 `Minimum` 250 값입니다. 이 여기에서 순환할 2D 개체의 X 및 Y 좌표 원점을 250 픽셀 반지름으로 정의 된 원형 제한을 의미 합니다. 3D 공간에서이 개체의 모든 회전 250 보다 작은 좌표 값 항상 발생 합니다.

합니다 [ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) 코드 숨김 파일 로드 하는 비트맵은 300 픽셀 사각형입니다.

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

이 비트맵에 3D 변환을 가운데 표시 되 고 하는 경우 다음 X 및 Y 좌표 –150 150 사이의 모퉁이 센터에서 212 픽셀 250 픽셀이 반경 내 하다는 것입니다.

합니다 `PaintSurface` 처리기를 만듭니다 `SKMatrix44` 개체 슬라이더를 기반으로 하며 함께 사용 하 여 곱합니다 `PostConcat`합니다. 합니다 `SKMatrix` 마지막에서 추출 된 값 `SKMatrix44` 개체 주위에서 가운데 화면 가운데에 회전 변환을 변환:

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Use 3D matrix for 3D rotations and perspective
        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, (float)xRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, (float)yRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, (float)zRotateSlider.Value));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / (float)depthSlider.Value;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the bitmap
        canvas.SetMatrix(matrix);
        float xBitmap = xCenter - bitmap.Width / 2;
        float yBitmap = yCenter - bitmap.Height / 2;
        canvas.DrawBitmap(bitmap, xBitmap, yBitmap);
    }
}
```

네 번째 슬라이더를 사용 하 여 실험 하는 경우에 다른 깊이 설정을 뷰어의에서 추가 개체를 이동 하지 않습니다 하지만 대신 큐브 뷰 결과의 범위를 alter는 보면:

[![](3d-rotation-images/rotation3d-small.png "삼중 회전 3D 페이지 스크린샷")](3d-rotation-images/rotation3d-large.png#lightbox "삼중 회전 3D 페이지 스크린샷")

합니다 **애니메이션 회전 3D** 도 사용 하 여 `SKMatrix44` 3D 공간에 있는 텍스트 문자열에 애니메이션 효과를 합니다. `textPaint` 개체 필드 생성자가 사용 되는 텍스트의 경계를 결정 하는 설정 합니다.

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    SKCanvasView canvasView;
    float xRotationDegrees, yRotationDegrees, zRotationDegrees;
    string text = "SkiaSharp";
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        TextSize = 100,
        StrokeWidth = 3,
    };
    SKRect textBounds;

    public AnimatedRotation3DPage()
    {
        Title = "Animated Rotation 3D";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Measure the text
        textPaint.MeasureText(text, ref textBounds);
    }
    ...
}
```

`OnAppearing` 재정의 세 Xamarin.Forms 정의 `Animation` 애니메이션 효과를 줄 개체를 `xRotationDegrees`, `yRotationDegrees`, 및 `zRotationDegrees` 서로 다른 속도로 필드. 이러한 애니메이션 기간 소수로 설정 된 전체 조합 마다 385 초 또는 10 분 이상만 반복 하므로 (5 초, 7, 11 초 및) 번호:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();

        new Animation((value) => xRotationDegrees = 360 * (float)value).
            Commit(this, "xRotationAnimation", length: 5000, repeat: () => true);

        new Animation((value) => yRotationDegrees = 360 * (float)value).
            Commit(this, "yRotationAnimation", length: 7000, repeat: () => true);

        new Animation((value) =>
        {
            zRotationDegrees = 360 * (float)value;
            canvasView.InvalidateSurface();
        }).Commit(this, "zRotationAnimation", length: 11000, repeat: () => true);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        this.AbortAnimation("xRotationAnimation");
        this.AbortAnimation("yRotationAnimation");
        this.AbortAnimation("zRotationAnimation");
    }
    ...
}
```

이전 프로그램 처럼 합니다 `PaintCanvas` 처리기를 만듭니다 `SKMatrix44` 회전 및 측면에 대 한 값을 가지 며 함께:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Scale so text fits
        float scale = Math.Min(info.Width / textBounds.Width,
                               info.Height / textBounds.Height);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(scale, scale));

        // Calculate composite 3D transforms
        float depth = 0.75f * scale * textBounds.Width;

        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, xRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, yRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, zRotationDegrees));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / depth;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the text
        canvas.SetMatrix(matrix);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;
        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

회전 중심을 화면의 센터로 이동 하 고 화면와 동일한 너비가 되도록 텍스트 문자열의 크기를 조정할 몇 가지 2D 변환을 사용 하 여이 3D 회전 묶 였는:

[![](3d-rotation-images/animatedrotation3d-small.png "삼중 애니메이션 회전 3D 페이지 스크린샷")](3d-rotation-images/animatedrotation3d-large.png#lightbox "삼중 애니메이션 회전 3D 페이지 스크린샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
