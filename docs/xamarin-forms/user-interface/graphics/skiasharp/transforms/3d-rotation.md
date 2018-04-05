---
title: 3D 회전
description: 사용 하 여 유사 형식이 아닌 3D 공간에서 개체를 2D 회전을 변형 합니다.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 2f5562475db17b7451fe7cb2ee8bbf4ccb782a87
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/05/2018
---
# <a name="3d-rotations"></a>3D 회전

_사용 하 여 유사 형식이 아닌 3D 공간에서 개체를 2D 회전을 변형 합니다._

일반적인 용도로 유사 형식이 아닌 변형 3D 공간에서 개체를 2D 회전을 시뮬레이션 하 고:

![](3d-rotation-images/3drotationsexample.png "3D 공간에서 회전 하는 텍스트 문자열")

이 작업에서는 3 차원 회전을 사용 하 고 비 관계을 파생 `SKMatrix` 이러한 3D 회전을 수행 하는 변환입니다.

이 개발 하는 것이 어렵습니다 `SKMatrix` 변형 2 차원 내에서 전적으로 작동 합니다. 작업이이 3 x 3 매트릭스는 3D 그래픽에 사용 되는 4-4 매트릭스에서 파생 되는 경우 훨씬 쉬워집니다. SkiaSharp 포함는 [ `SKMatrix44` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.PreConcat/p/SkiaSharp.SKMatrix44/) 이 목적 이지만 3D 그래픽에 몇 가지 배경에 대 한 클래스는 3D 회전 및 4-4 변환 매트릭스를 이해 하는 데 필요 합니다.

개념적으로 문자로 라는 세 번째 축을 추가 하는 3 차원 좌표계, 화면 오른쪽 각도에 Z 축이 있습니다. 3D 공간에서 좌표 점 기반으로 세 숫자가으로 표시 됩니다: (x, y, z). 3d에서 오른쪽 증가 X의 값이이 문서에서 사용 하는 좌표계 되며 Y의 증가 값이 두 차원에서와 마찬가지로 아래로 이동 합니다. 화면 나오는 양의 Z 값이 증가 합니다. 원점은 왼쪽 위 모퉁이 있는 2 차원 그래픽에서와 마찬가지로 있습니다. 이 평면의 오른쪽 각도에 Z 축이 있는 XY 평면으로 화면 생각할 수 있습니다.

이 왼쪽 좌표 시스템 이라고 합니다. 프로그램 왼쪽의 X 좌표 (오른쪽)으로 양의 방향에서에 대 한 집게 가리키고 Y 늘어나는 방향 중간 손가락 (다운)를 조정 하는 경우 다음 프로그램 thumb의에서 점은 Z 좌표 늘어나는 방향을-에서으로 확장 화면입니다.

3D 그래픽에는 변환은 4-4 행렬을 기반으로 합니다. 4-4 항등 매트릭스는 다음과 같습니다.

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

4-4 행렬을 사용할 경우 하는 것의 행 및 열 번호를 가진 셀을 식별 합니다.

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

그러나는 SkiaSharp `Matrix44` 클래스는 약간 다릅니다. 개별 셀 값을 가져오거나 설정 하는 유일한 방법은 `SKMatrix44` 사용 하는 것은 [ `Item` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Item/p/System.Int32/System.Int32/) 인덱서 합니다. 행 및 열 인덱스는 0부터 시작 하지 않고 1부터 시작 하 고 행과 열이 서로 바뀌어서 합니다. 인덱서를 사용 하는 위의 다이어그램에 M14 셀에 액세스 하는 `[3, 0]` 에 `SKMatrix44` 개체입니다.

3D 그래픽 시스템에서는 (x, y, z) 3D 지점 4-4 변환 매트릭스를 곱하는 1-4 매트릭스를 변환 됩니다.

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

3 차원에서 발생 하는 2D 유사 변환, 4 차원에서 3D 변환 간주 됩니다. 네 번째 차원의 W, 라고 하 고 3D 공간 W 좌표는 1로 4 차원 공간 내에 존재로 간주 됩니다. 변형 수식은 다음과 같습니다.

x' = M11·x + M21·y + M31·z + M41

y' = M12·x + M22·y + M32·z + M42

z' = M13·x + M23·y + M33·z + M43

w' = M14·x + M24·y + M34·z + M44

변형 수식에서 분명히 하는 셀 `M11`, `M22`, `M33` X, Y 및 Z 방향의 배율 계수는 및 `M41`, `M42`, 및 `M43` 는 X, Y 및 Z 번역 요소 방향입니다.

이러한 좌표 W가 1, x 3D 공간으로 다시 변환 하려면 ', y', z' 좌표는 모두로 나눈 w':

x" = x' / w'

y" = y' / w'

z" = z' / w'

w" = w' / w' = 1

이 나누기 w로 ' 3D 공간에서 큐브 뷰를 제공 합니다. 경우 w'가 1 다음 없는 발생 합니다.

회전 3D 공간에서 매우 복잡 해질 수는 있지만 X, Y 및 Z 축을 중심은 가장 간단한 회전 합니다. X 축을 α 각도의 회전은이 행렬입니다.

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

X의 값이이 변환에 적용 하는 경우 동일 하 게 유지 합니다. Y 축 중심으로 회전 유지 Y 변경 되지 않은 값:

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Z 축 중심으로 회전 2 차원 그래픽에서와 같습니다.

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

좌표 시스템의 선호도 회전 방향을 포함 됩니다. 왼쪽 시스템입니다 증가 값 특정 축 방향으로 왼쪽 손 엄지 단추를 가리킬 경우-회전을 X 축에 대 한 오른쪽으로 회전 Y 축을 중심으로 회전을 Z 축에 대 한 아래쪽-다음의 곡선 yo 다른 손가락의 회전 양의 각도 방향을 나타냅니다.

`SKMatrix44` 정적 일반화 [ `CreateRotation` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotation/p/System.Single/System.Single/System.Single/System.Single/) 및 [ `CreateRotationDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotationDegrees/p/System.Single/System.Single/System.Single/System.Single/) 회전 중심점입니다 발생 축을 지정 하려면 사용할 수 있는 메서드가 있습니다.

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

X 축 중심으로 회전에 대 한 1, 0, 0에 처음 세 개의 인수를 설정 합니다. Y 축 중심으로 회전에 대 한 0, 1, 0으로 설정 하 고 Z 축 중심으로 회전에 대 한 0, 0, 1로 설정 합니다.

4-4의 네 번째 열은 관점입니다. `SKMatrix44` 관점 변환, 작성용 메서드가 없으므로 있지만 직접 다음 코드를 사용 하 여 만들 수 있습니다.

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

인수 이름에 대 한 이유 `depth` 곧 확인할 수 있습니다. 해당 코드는 행렬을 만듭니다.

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

W 다음 계산에서는 변형 수식 결과 ':

w' – z = / 깊이 + 1

Z 값 (개념적으로 XY 평면) 뒤에 0 보다 작은 경우 X 및 Y 좌표를 줄이기 위해 고 Z의 양수 값에 대 한 X 및 Y 좌표를 증가 시키기 위해 사용 됩니다. Z 좌표 인 경우 `depth`, 다음 w'는 0이 고 좌표 될 무한 합니다. 3 차원 그래픽 시스템은 카메라 비유를 기반으로 구축 및 `depth` 여기에서 값 좌표계의 원점부터 카메라의 거리를 나타냅니다. 그래픽 개체에는 Z 좌표를 즉 경우 `depth` 단위 출처에서 것 카메라 렌즈 닿는 개념적으로 매핑되며 크기에 제한이 있습니다.

아마도 사용할 것이 염두에서에 둬야 `perspectiveMatrix` 값 조합 하 여 회전 행렬입니다. 회전 되 고 graphics 개체는 X 또는 Y 좌표 보다 큰 경우 `depth`, 3D 공간에서이 개체의 회전은 보다 큰 Z 좌표를 포함 될 `depth`합니다. 이러한 경우가 발생 해야 합니다! 만들 때 `perspectiveMatrix` 설정 하려는 `depth` 회전 하는 방법에 관계 없이 graphics 개체에서 모든 좌표에 대 한 충분히 큰 값으로. 이 0으로 나누기 되지 되는지 확인 합니다.

3D 회전 및 큐브 뷰를 결합 하 여 4-4 행렬을 곱한 필요 합니다. 이 위해 `SKMatrix44` 연결 메서드를 정의 합니다. 경우 `A` 및 `B` 는 `SKMatrix44` 개체 × b:에 다음 코드는 설정

```csharp
A.PostConcat(B);
```

4-4 변환 매트릭스를 2 차원 그래픽 시스템에서 사용할 경우 2D 개체에 적용 됩니다. 이러한 개체는 평면 및 0의 Z 좌표를 갖는으로 간주 됩니다. 변환 곱셈은 앞에 표시 된 변환 보다 좀 더 간단 합니다.

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

행렬의 세 번째 행에 있는 셀을 포함 하지 않는 변환 수식에서 z 결과 대 한 해당 0 값:

x' = M11·x + M21·y + M41

y' = M12·x + M22·y + M42

z' = M13·x + M23·y + M43

w' = M14·x + M24·y + M44

또한 z' 좌표는 관련 여기 뿐입니다. 3D 개체는 2 차원 그래픽 시스템에 표시 되 면 Z 좌표 값을 무시 하 여 2 차원 개체에 축소 됩니다. 변형 수식 실제로이 두 가지

x" = x' / w'

y" = y' / w'

즉, 세 번째 행 *및* 4-4 행렬의 세 번째 열을 무시할 수 있습니다.

하지만 해당 되는 경우, 이유 인지 확인 하지 않아도 4-4 매트릭스 처음부터

세 번째 행 및 세 번째 열에는 4-4 2 차원 변환은, 세 번째 행 및 열에 대 한 관련가 *않습니다* 역할을 하며 때 하기 전에 다양 한 `SKMatrix44` 값 곱합니다. 예를 들어 원근감 변환이 Y 축 중심으로 회전 곱하기를 가정 합니다.

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

셀 제품에서 `M14` 관점 값이 들어 있습니다. 해당 행렬 2D 개체에 적용 하려는 경우 세 번째 행 및 열 3-3 행렬으로 변환할 제거 됩니다.

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

이제 2D 점의 변환을 사용할 수 있습니다.

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

변형 수식에는

x' = cos(α)·x

y' = y

z' = (sin(α)/depth)·x + 1

이제 모든 것에 z 나눌 ':

"x = cos (α) ·x / (((α) sin / 깊이) ·x + 1)

y" = y / ((sin(α)/depth)·x + 1)

2D 개체 양각 양의 다음 Y 축을 중심으로 회전 하는 경우 X 값 오목 하 게 표시 부정 하는 동안 백그라운드에 X 값은 전경으로와 야 합니다. X 값 좌표로 Y 축에서 가장 먼 곳 (코사인 값에 의해 제어 되)이 표시는 Y 축에 가깝게 이동 하는 작은 또는 뷰어 먼 이동 하는 동안 큰 나 가깝게 것 같습니다.

사용 하는 경우 `SKMatrix44`, 다양 한 곱하여 모든 3D 회전 및 큐브 뷰 작업을 수행할 `SKMatrix44` 값입니다. 4-4에서는 2 차원 3 x 3 매트릭스를 추출할 수 있습니다 다음 매트릭스를 사용 하 여는 [ `Matrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Matrix/) 의 속성은 `SKMatrix44` 클래스. 이 속성은 익숙한 반환 `SKMatrix` 값입니다.

**회전 3D** 페이지 3D 회전을 실험할 수 있습니다. [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) 파일 X, Y 및 Z 축 중심으로 회전을 설정 하 고 깊이 값을 설정 하는 4 개의 슬라이더를 인스턴스화합니다.

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

에 `depthSlider` 사용 하 여 초기화는 `Minimum` 250의 값입니다. 이 여기 회전 되 고 2 차원 개체에는 원점 250 픽셀 radius에 정의 된 circle에 제한 된 X 및 Y 좌표를 의미 합니다. 3D 공간에서이 개체의 모든 회전 좌표 값 250 보다 작은 항상 발생 합니다.

[ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) 비트맵 너비가 300 픽셀이 고 정사각형을 코드 숨김 파일을 로드 합니다.

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
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

이 비트맵에 3D 변환 가운데 표시 되는 경우 다음 X 및 Y 좌표 –150 사이의 150, 250 픽셀 반경 내 하다는 것 212 픽셀 중심에서 모서리에 있는 동안 합니다.

`PaintSurface` 처리기 만듭니다 `SKMatrix44` 개체 슬라이더를 기반으로 하며 함께 사용 하 여 곱합니다 `PostConcat`합니다. `SKMatrix` 최종에서 추출한 값 `SKMatrix44` 개체 주위에서 가운데 맞춤 화면 중앙에 회전 하는 변환을 번역:

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
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

네 번째 슬라이더를 시험해 볼 때 다른 깊이 설정을 뷰어에서 개체를 더 이상 이동 하지 않는 하지만 대신 원근 효과의 범위를 변경할 알 수 있습니다.

[![](3d-rotation-images/rotation3d-small.png "회전 3D 페이지의 삼중 스크린샷")](3d-rotation-images/rotation3d-large.png#lightbox "회전의 3D 페이지의 삼중 스크린 샷")

**애니메이션 회전 3D** 또한 사용 하 여 `SKMatrix44` 3D 공간에서 텍스트 문자열에 애니메이션 효과를 주는 합니다. `textPaint` 텍스트의 범위를 결정 하는 필드는 생성자에서 사용으로 설정 하는 개체:

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

`OnAppearing` 재정의 정의 세 Xamarin.Forms `Animation` 애니메이션 효과를 줄 개체는 `xRotationDegrees`, `yRotationDegrees`, 및 `zRotationDegrees` 서로 다른 속도로 필드입니다. 이러한 애니메이션 기간 설정 되어 있는 단추가 숫자를-5 초를 7 초 11 초-모든 385 초 또는 10 분 이상에 전체 조합 반복 되므로:

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

이전 프로그램에서와 같이 `PaintCanvas` 처리기 만듭니다 `SKMatrix44` 회전 및 관점에 대 한 값을 가지 며 함께:

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

2D 변환을 여러 화면 가운데에 회전의 중심을 이동 하 고 동일한 너비로 화면 구분 되는 텍스트 문자열의 크기를 조정할 수로이 3D 회전 묶은:

[![](3d-rotation-images/animatedrotation3d-small.png "애니메이션을 회전 3D 페이지의 삼중 스크린샷")](3d-rotation-images/animatedrotation3d-large.png#lightbox "애니메이션 회전의 3D 페이지의 삼중 스크린 샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
