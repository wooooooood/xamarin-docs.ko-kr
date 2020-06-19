---
title: SkiaSharp의 3D 회전
description: 이 문서에서는 비 상관 변환을 사용 하 여 3D 공간에서 2D 개체를 회전 하는 방법을 설명 하 고 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3706139a2c15d01af67203c2bd09b281de80ed52
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140207"
---
# <a name="3d-rotations-in-skiasharp"></a>SkiaSharp의 3D 회전

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_비 상관 변환을 사용 하 여 3D 공간에서 2D 개체를 회전 합니다._

비 상관 변환의 한 가지 일반적인 응용 프로그램은 3D 공간에서 2D 개체의 회전을 시뮬레이션 하는 것입니다.

![](3d-rotation-images/3drotationsexample.png "A text string rotated in 3D space")

이 작업에는 3 차원 회전을 사용한 다음 `SKMatrix` 이러한 3d 회전을 수행 하는 비 상관 변환의 파생이 포함 됩니다.

이러한 변환은 두 차원 내 에서만 작동 하는 것이 좋습니다 `SKMatrix` . 3D 그래픽에서 사용 되는 4 x 4 매트릭스에서 3 x 3 매트릭스를 파생 하는 경우 작업이 훨씬 쉬워졌습니다. SkiaSharp에는 [`SKMatrix44`](xref:SkiaSharp.SKMatrix44) 이 목적을 위한 클래스가 포함 되어 있지만 3d 회전 및 4 x 4 변환 매트릭스를 이해 하려면 3d 그래픽의 일부 배경이 필요 합니다.

3 차원 좌표계는 Z 라는 세 번째 축을 추가 합니다. 개념적으로 Z 축은 화면에 직각을 갖습니다. 3D 공간의 좌표 점이 3 개의 숫자 (x, y, z)로 표시 됩니다. 이 문서에서 사용 되는 3D 좌표계에서 X 값을 늘리려면 두 차원에서와 마찬가지로 Y의 값이 확장 됩니다. 양의 Z 값을 늘려도 화면에서 나옵니다. 원점은 2D 그래픽과 마찬가지로 왼쪽 위 모퉁이입니다. 이 평면에 대 한 오른쪽 각도에서 Z 축을 사용 하 여 화면을 XY 평면으로 간주할 수 있습니다.

이를 왼쪽 좌표 시스템 이라고 합니다. 양수 X 좌표 (오른쪽)의 방향으로 왼쪽의 엄지와 집게 손가락을 가리키고 가운데 손가락을 Y 좌표 (아래쪽)의 방향으로 가리키면 화면에 표시 되는 Z 좌표 방향의 화살표가 화면에서 확장 됩니다.

3D 그래픽에서 변환은 4 x 4 매트릭스를 기반으로 합니다. 4-4 항등 매트릭스는 다음과 같습니다.

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

4 x 4 매트릭스를 사용 하는 경우 행 및 열 번호를 사용 하 여 셀을 식별 하는 것이 편리 합니다.

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

그러나 SkiaSharp 클래스는 `Matrix44` 약간 다릅니다. 에서 개별 셀 값을 설정 하거나 가져오는 유일한 방법은 `SKMatrix44` 인덱서를 사용 하는 것입니다 [`Item`](xref:SkiaSharp.SKMatrix44.Item(System.Int32,System.Int32)) . 행 및 열 인덱스는 1부터 시작 하는 것이 아니라 0부터 시작 하 여 행과 열이 교환 됩니다. 위의 다이어그램에서 M14 셀은 개체의 인덱서를 사용 하 여 액세스할 수 `[3, 0]` `SKMatrix44` 있습니다.

3D 그래픽 시스템에서 3D 점 (x, y, z)은 4 x 4 x 4 x 4 x 4로 변환 됩니다.

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

3 차원에서 수행 되는 2D 변환과 유사 하 게 3D 변환은 4 차원에서 수행 되는 것으로 간주 됩니다. 네 번째 차원을 W 라고 하며, 3D 공간은 4D 공간 내에 존재 하는 것으로 간주 됩니다. 여기서 W 좌표는 1과 같습니다. 변환 수식은 다음과 같습니다.

`x' = M11·x + M21·y + M31·z + M41`

`y' = M12·x + M22·y + M32·z + M42`

`z' = M13·x + M23·y + M33·z + M43`

`w' = M14·x + M24·y + M34·z + M44`

변환 수식에서 알 수 있는 것은 셀이 `M11` `M22` `M33` x, y 및 z 방향의 배율 인수이 고, `M41` `M42` 및 `M43` 는 x, y 및 z 방향의 변환 요소입니다.

이러한 좌표를 3D 공간으로 다시 변환 하려면 (W가 1 인 경우) x ', y ' 및 z ' 좌표는 모두 w '로 나뉩니다.

`x" = x' / w'`

`y" = y' / w'`

`z" = z' / w'`

`w" = w' / w' = 1`

W '로 나누기는 3D 공간에서 큐브 뷰를 제공 합니다. W '가 1 이면 큐브 뷰가 발생 하지 않습니다.

3D 공간에서의 회전은 매우 복잡할 수 있지만 가장 간단한 회전은 X, Y 및 Z 축을 중심으로 하는 회전입니다. X 축을 중심으로 하는 각도 α 회전은 다음 행렬입니다.

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

이 변환이 적용 될 때 X의 값은 동일 하 게 유지 됩니다. Y 축을 중심으로 회전 하면 값이 변경 되지 않은 상태로 유지 됩니다.

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Z 축을 중심으로 회전 하는 것은 2D 그래픽과 동일 합니다.

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

회전 방향은 좌표계를 활용 하 여 암시 됩니다. 이는 왼손 시스템 이므로 특정 축에 대 한 값을 높이는 왼쪽의 엄지 단추를 마우스 오른쪽 단추로 클릭 하 여 X 축을 중심으로 회전 하 고 Y 축을 중심으로 회전 하 여 Z 축을 중심으로 회전 하는 경우, 다른 손가락의 곡선은 긍정적인 각도의 회전 방향을 나타냅니다.

`SKMatrix44`에는 [`CreateRotation`](xref:SkiaSharp.SKMatrix44.CreateRotation(System.Single,System.Single,System.Single,System.Single)) [`CreateRotationDegrees`](xref:SkiaSharp.SKMatrix44.CreateRotationDegrees(System.Single,System.Single,System.Single,System.Single)) 회전이 발생 하는 축을 지정할 수 있는 일반화 된 정적 및 메서드가 있습니다.

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

X 축을 중심으로 회전 하려면 처음 세 인수를 1, 0, 0으로 설정 합니다. Y 축을 중심으로 회전 하려면이를 0, 1, 0으로 설정 하 고 Z 축을 중심으로 회전 하려면 0, 0, 1로 설정 합니다.

4-4의 네 번째 열은 큐브 뷰에 대 한 것입니다. 에는 `SKMatrix44` 큐브 뷰 변환을 만드는 방법이 없지만 다음 코드를 사용 하 여 사용자를 직접 만들 수 있습니다.

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

인수 이름의 이유가 `depth` 곧 명백 하 게 됩니다. 이 코드는 행렬을 만듭니다.

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

변환 수식은 다음과 같이 w '를 계산 합니다.

`w' = –z / depth + 1`

Z 값이 0 보다 작은 경우 (개념적으로 XY 평면 뒤에) X 및 Y 좌표를 줄이는 데 사용 되며 Z 값에 대해 X 및 Y 좌표를 늘립니다. Z 좌표가와 같으면 `depth` w '는 0이 고 좌표는 무한대가 됩니다. 3 차원 그래픽 시스템은 카메라 비유를 중심으로 구축 되며 여기에서 `depth` 값은 좌표계의 원점 으로부터의 카메라 거리를 나타냅니다. 그래픽 개체의 축이 원점의 단위인 Z 좌표 이면이는 `depth` 개념적으로 카메라의 렌즈를 터치 하 고 무한히 크게 커집니다.

회전 매트릭스와 함께이 값을 사용 하는 것이 좋습니다 `perspectiveMatrix` . 회전 중인 그래픽 개체의 X 축과 Y 좌표가 보다 큰 경우 `depth` 3d 공간에서이 개체의 회전은 Z 좌표가 보다 클 수 있습니다 `depth` . 이는 피해 야 합니다. 를 만들 때 `perspectiveMatrix` `depth` 회전 방식에 관계 없이 graphics 개체의 모든 좌표에 대 한 값을 충분히 크게 설정 하려고 합니다. 이렇게 하면 0으로 나누기가 수행 되지 않습니다.

3D 회전 및 큐브 뷰를 결합 하려면 4 x 4 매트릭스를 함께 곱합니다. 이 목적을 위해에서는 `SKMatrix44` 연결 메서드를 정의 합니다. `A`및가 개체인 경우에는 `B` `SKMatrix44` 다음 코드에서 a × B와 같은 값을 설정 합니다.

```csharp
A.PostConcat(B);
```

2D 그래픽 시스템에서 4 x 4로 변형 매트릭스가 사용 되는 경우 2D 개체에 적용 됩니다. 이러한 개체는 플랫 이며 Z 좌표가 0 인 것으로 간주 됩니다. 변환 곱셈은 앞에서 설명한 변환 보다 약간 더 간단 합니다.

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Z의 값이 0 이면 행렬의 세 번째 행에 있는 셀이 포함 되지 않는 변형 수식이 발생 합니다.

x ' = M11 · x + M21 · y + M41

y ' = M12 · x + M22 · y + M42

z ' = M13 · x + M23 · y + M43

w ' = M14 · x + M24 · y + M44

또한 z 좌표는 여기 에서도 관련이 없습니다. 3D 개체가 2D 그래픽 시스템에 표시 되 면 Z 좌표 값을 무시 하 여 2 차원 개체로 축소 됩니다. 변환 수식은 정말 이러한 두 가지 방법입니다.

`x" = x' / w'`

`y" = y' / w'`

즉, 4 x 4 행렬의 세 번째 행 *과* 세 번째 열을 무시할 수 있습니다.

그렇다면 4-4 매트릭스가 첫 번째 경우에도 필요한 이유는 무엇 인가요?

4-4의 세 번째 행과 세 번째 열은 2 차원 변환과 관련이 없지만 세 번째 행과 열은 다양 한 값이 함께 연결 될 때 이전에 역할을 *수행* 합니다 `SKMatrix44` . 예를 들어 원근감 변환과 함께 Y 축을 중심으로 회전 하는 경우를 가정해 보겠습니다.

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

제품에서 셀에는 `M14` 큐브 뷰 값이 포함 됩니다. 이 행렬을 2D 개체에 적용 하려는 경우 세 번째 행과 열은 3 x 3 행렬로 변환 하기 위해 제거 됩니다.

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

이제 2D 포인트를 변환 하는 데 사용할 수 있습니다.

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

변환 수식은 다음과 같습니다.

`x' = cos(α)·x`

`y' = y`

`z' = (sin(α)/depth)·x + 1`

이제 모든 항목을 z로 나눕니다.

`x" = cos(α)·x / ((sin(α)/depth)·x + 1)`

`y" = y / ((sin(α)/depth)·x + 1)`

2D 개체가 Y 축 주위에서 양의 각도로 회전 하는 경우 양의 X 값은 recede 하는 반면 음수 X 값은 포그라운드로 제공 됩니다. X 값은 y 축에서의 좌표 (코사인 값으로 제어 됨)와 가깝게 이동 하는 것 처럼 보일 수 있습니다. Y 축에서의 좌표는 가장 quantity 더 작거나 뷰어에 더 가깝게 이동 하기 때문입니다.

를 사용 하는 경우 `SKMatrix44` 다양 한 값을 곱하여 3d 회전 및 큐브 뷰 작업을 모두 수행 합니다 `SKMatrix44` . 그런 다음 클래스의 속성을 사용 하 여 4-6 행렬에서 2 차원 3 x 3 행렬을 추출할 수 있습니다 [`Matrix`](xref:SkiaSharp.SKMatrix44.Matrix) `SKMatrix44` . 이 속성은 친숙 한 `SKMatrix` 값을 반환 합니다.

**회전 3d** 페이지에서는 3d 회전을 사용 하 여 실험해 볼 수 있습니다. [**Rotation3DPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) 파일은 4 개의 슬라이더를 인스턴스화하여 X, Y 및 Z 축을 중심으로 회전을 설정 하 고 깊이 값을 설정 합니다.

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

는 `depthSlider` 250 값으로 초기화 됩니다 `Minimum` . 이는 여기서 회전 중인 2D 개체의 X 및 Y 좌표가 원본 주위에 있는 250 픽셀 반경으로 정의 된 원으로 제한 됨을 의미 합니다. 3D 공간에서이 개체의 회전은 항상 250 보다 작은 좌표 값을 반환 합니다.

[**Rotation3DPage.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) 코드 숨김이 300 픽셀 사각형의 비트맵에서 로드 됩니다.

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

3D 변환이이 비트맵을 중심으로 하는 경우 X 및 Y 좌표는-150 ~ 150 사이이 고, 모퉁이는 중앙에서 212 픽셀 이므로 모든 항목은 250 픽셀 반지름 내에 있습니다.

`PaintSurface`처리기는 `SKMatrix44` 슬라이더를 기준으로 개체를 만들고를 사용 하 여이를 곱합니다 `PostConcat` . `SKMatrix`최종 개체에서 추출 된 값은 `SKMatrix44` 화면 중심의 회전을 가운데 맞춤 하기 위해 변환 변환으로 둘러싸여 있습니다.

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

네 번째 슬라이더를 사용 하 여 실험을 수행 하는 경우 다른 깊이 설정에서 개체를 뷰어에서 멀리 이동 하지 않고 큐브 뷰 효과의 범위를 변경 하는 것을 알 수 있습니다.

[![](3d-rotation-images/rotation3d-small.png "Triple screenshot of the Rotation 3D page")](3d-rotation-images/rotation3d-large.png#lightbox "Triple screenshot of the Rotation 3D page")

**애니메이션 회전 3d** 는를 사용 `SKMatrix44` 하 여 3d 공간에서 텍스트 문자열에 애니메이션 효과를 주는 데에도 사용 됩니다. `textPaint`필드로 설정 된 개체는 생성자에서 텍스트의 범위를 결정 하는 데 사용 됩니다.

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

`OnAppearing`재정의는 세 가지 개체를 정의 Xamarin.Forms `Animation` 하 여 `xRotationDegrees` , `yRotationDegrees` 및 `zRotationDegrees` 필드에 서로 다른 비율을 적용 합니다. 이러한 애니메이션의 기간은 소수 (5 초, 7 초, 11 초)로 설정 되므로 전체 조합은 385 초 또는 10 분 넘게 반복 됩니다.

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

이전 프로그램에서와 같이 처리기는 `PaintCanvas` `SKMatrix44` 회전 및 큐브 뷰에 대 한 값을 만들고 함께 곱합니다.

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

이 3D 회전은 회전 중심을 화면 중심으로 이동 하 고, 텍스트 문자열의 크기를 조정 하 여 화면과 동일한 너비를 갖도록 하는 여러 2D 변환으로 둘러싸여 있습니다.

[![](3d-rotation-images/animatedrotation3d-small.png "Triple screenshot of the Animated Rotation 3D page")](3d-rotation-images/animatedrotation3d-large.png#lightbox "Triple screenshot of the Animated Rotation 3D page")

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
