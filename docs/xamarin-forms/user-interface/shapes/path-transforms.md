---
title: 'Xamarin.Forms셰이프: 경로 변환'
description: Xamarin.Forms변환은 한 좌표 공간에서 다른 좌표 공간으로 경로 개체를 변환 하는 방법을 정의 합니다.
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/02/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 41de95c452212dce77d6365265e4813170c9b9b9
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853054"
---
# <a name="xamarinforms-shapes-path-transforms"></a>Xamarin.Forms셰이프: 경로 변환

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

는 `Transform` `Path` 한 좌표 공간에서 다른 좌표 공간으로 개체를 변환 하는 방법을 정의 합니다. 개체에 변환을 적용 하면 `Path` 개체가 UI에서 렌더링 되는 방식을 변경 합니다.

변환은 회전, 크기 조정, 기울이기 및 변환의 네 가지 일반적인 분류로 분류할 수 있습니다. Xamarin.Forms이러한 각 변환 분류에 대해 클래스를 정의 합니다.

- `RotateTransform`지정 된로를 회전 하는입니다 `Path` `Angle` .
- `ScaleTransform`는 `Path` 지정 된 및 양만큼 개체 크기를 조정 `ScaleX` `ScaleY` 합니다.
- `SkewTransform`는 `Path` 지정 된 및 양만큼 개체를 `AngleX` 기울입니다 `AngleY` .
- `TranslateTransform`는 `Path` 지정 된 및 양만큼 개체를 `X` 이동 `Y` 합니다.

Xamarin.Forms에서는 더 복잡 한 변환을 만들기 위한 다음 클래스도 제공 합니다.

- `TransformGroup`는 여러 transform 개체로 구성 된 복합 변환을 나타냅니다.
- `CompositeTransform`-개체에 여러 변환 작업을 적용 `Path` 합니다.
- `MatrixTransform`는 다른 변환 클래스에서 제공 하지 않는 사용자 지정 변환을 만듭니다.

이러한 모든 클래스는 `Transform` 형식의 속성을 정의 하는 클래스에서 파생 `Value` `Matrix` 됩니다. 이 속성은 현재 변환을 `Matrix` 개체로 나타냅니다. 구조체에 대 한 자세한 내용은 `Matrix` [Transform 행렬](#transform-matrix)을 참조 하세요.

에 변환을 적용 하려면 `Path` transform 클래스를 만들고이를 속성의 값으로 설정 `Path.RenderTransform` 합니다.

## <a name="rotation-transform"></a>회전 변환

회전 변환은 `Path` 2d x-y 좌표계에서 지정 된 지점에 대 한 개체를 시계 방향으로 회전 합니다.

클래스 `RotateTransform` 에서 파생 되는 클래스는 `Transform` 다음 속성을 정의 합니다.

- `Angle`형식의는 `double` 시계 방향 회전의 각도 (도)를 나타냅니다. 이 속성의 기본값은 0.0입니다.
- `CenterX`형식의는 `double` 회전 중심점의 x 좌표를 나타냅니다. 이 속성의 기본값은 0.0입니다.
- `CenterY`형식의는 `double` 회전 중심점의 y 좌표를 나타냅니다. 이 속성의 기본값은 0.0입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

`CenterX`및 `CenterY` 속성은 `Path` 개체가 회전 되는 지점을 지정 합니다. 이 중심점은 변환 된 개체의 좌표 공간으로 표현 됩니다. 기본적으로 회전은 개체의 왼쪽 위 모퉁이에 해당 하는 (0, 0)에 적용 됩니다 `Path` .

다음 예제에서는 개체를 회전 하는 방법을 보여 줍니다 `Path` .

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <RotateTransform CenterX="0"
                         CenterY="0"
                         Angle="45" />
    </Path.RenderTransform>
</Path>
```

이 예제에서는 개체를 `Path` 왼쪽 위 모퉁이에 대해 45도 회전 합니다.

## <a name="scale-transform"></a>크기 조정 변환

배율 변환은 `Path` 2d x-y 좌표계에서 개체의 크기를 조정 합니다.

클래스 `ScaleTransform` 에서 파생 되는 클래스는 `Transform` 다음 속성을 정의 합니다.

- `ScaleX``double`x-축 배율 인수를 나타내는 형식의입니다. 이 속성의 기본값은 1.0입니다.
- `ScaleY``double`y-축 배율 인수를 나타내는 형식의입니다. 이 속성의 기본값은 1.0입니다.
- `CenterX`-형식의 이며, `double` 이 변환의 중심점의 x 좌표를 나타냅니다. 이 속성의 기본값은 0.0입니다.
- `CenterY`-형식의 이며, `double` 이 변환의 중심점의 y 좌표를 나타냅니다. 이 속성의 기본값은 0.0입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

및의 값 `ScaleX` 은 `ScaleY` 결과 크기 조정에 큰 영향을 미칩니다.

- 0에서 1 사이의 값은 배율이 조정 된 개체의 너비와 높이를 줄입니다.
- 1 보다 큰 값은 배율이 조정 된 개체의 너비와 높이를 늘립니다.
- 값 1은 개체가 크기 조정 되지 않았음을 의미 합니다.
- 음수 값은 눈금 개체를 가로 및 세로로 대칭 이동 합니다.
- 0에서-1 사이의 값은 눈금 개체를 대칭 이동 하 고 너비와 높이를 줄입니다.
- -1 보다 작은 값은 개체를 대칭 이동 하 고 너비와 높이를 늘립니다.
- 값-1은 크기 조정 된 개체를 대칭 이동 하지만 가로 또는 세로 크기는 변경 하지 않습니다.

`CenterX`및 `CenterY` 속성은 개체의 크기를 조정 하는 지점을 지정 합니다 `Path` . 이 중심점은 변환 된 개체의 좌표 공간으로 표현 됩니다. 기본적으로 크기 조정은 개체의 왼쪽 위 모퉁이에 해당 하는 (0, 0)에 적용 됩니다 `Path` . 이렇게 하면 개체를 이동 하 여 `Path` 더 크게 표시 하는 효과가 있습니다. 변환을 적용 하면 개체가 있는 좌표 공간이 변경 되기 때문입니다 `Path` .

다음 예제에서는 개체의 크기를 조정 하는 방법을 보여 줍니다 `Path` .

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <ScaleTransform CenterX="0"
                        CenterY="0"
                        ScaleX="1.5"
                        ScaleY="1.5" />
    </Path.RenderTransform>
</Path>
```

이 예제에서 개체는 `Path` 크기의 1.5 배 크기를 조정 합니다.

## <a name="skew-transform"></a>변형 기울이기

기울이기 변환은 `Path` 2d x-y 좌표계에서 개체를 기울입니다 .이는 2d 개체에서 3d 깊이의 환상 효과를 만드는 데 유용 합니다.

클래스 `SkewTransform` 에서 파생 되는 클래스는 `Transform` 다음 속성을 정의 합니다.

- `AngleX``double`y-축에서 시계 반대 방향으로 측정 된 x-축 기울기 각도 (도)를 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.
- `AngleY``double`x-축에서 시계 반대 방향으로 측정 된 y-축 기울기 각도 (도)를 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.
- `CenterX``double`변환 중심의 x 좌표를 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.
- `CenterY``double`변환 중심의 y 좌표를 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

기울이기 변환의 효과를 예측 하려면가 `AngleX` 원래 좌표계를 기준으로 x 축 값을 기울이는 것이 좋습니다. 따라서 `AngleX` 30의 경우 y 축은 원본을 통해 30도 회전 하 고 x의 값을 해당 원점에서 30도 기울입니다. 마찬가지로, `AngleY` 30의는 개체의 y 값을 `Path` 원점 으로부터 30도 기울입니다.

> [!NOTE]
> `Path`현재 위치에서 개체를 기울이려면 `CenterX` 및 `CenterY` 속성을 개체의 중심점으로 설정 합니다.

다음 예제에서는 개체를 기울이는 방법을 보여 줍니다 `Path` .

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <SkewTransform CenterX="0"
                       CenterY="0"
                       AngleX="45"
                       AngleY="0" />
    </Path.RenderTransform>
</Path>
```

이 예제에서는 `Path` 중심점 (0, 0)에서 45도의 가로 기울기를 개체에 적용 합니다.

## <a name="translate-transform"></a>변환 변환

변환 변환은 2D x-y 좌표계에서 개체를 이동 합니다.

클래스 `TranslateTransform` 에서 파생 되는 클래스는 `Transform` 다음 속성을 정의 합니다.

- `X``double`x-축을 따라 이동할 거리를 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.
- `Y``double`y 축을 따라 이동할 거리를 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

음수 `X` 값은 개체를 왼쪽으로 이동 하 고, 양수 값은 개체를 오른쪽으로 이동 합니다. 음수 `Y` 값은 개체를 위로 이동 하 고, 양수 값은 개체를 아래로 이동 합니다.

다음 예제에서는 개체를 변환 하는 방법을 보여 줍니다 `Path` .

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <TranslateTransform X="50"
                            Y="50" />
    </Path.RenderTransform>
</Path>
```

이 예제에서 개체는 `Path` 50 장치 독립적 단위로 오른쪽으로 이동 하 고, 50 장치 독립적 단위는 아래로 이동 합니다.

## <a name="multiple-transforms"></a>여러 변환

Xamarin.Forms에는 개체에 다중 변환 적용을 지 원하는 두 개의 클래스가 있습니다 `Path` . 이는 `TransformGroup` , 및 `CompositeTransform` 입니다. 는 `TransformGroup` 원하는 순서로 변환을 수행 하는 반면는 `CompositeTransform` 특정 순서로 변환을 수행 합니다.

### <a name="transform-groups"></a>변환 그룹

변환 그룹은 여러 개체로 구성 된 복합 변환을 나타냅니다 `Transform` .

클래스 `TransformGroup` 에서 파생 되는 클래스는 `Transform` `Children` `TransformCollection` 개체의 컬렉션을 나타내는 형식의 속성을 정의 `Transform` 합니다. 이 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상이 될 수 있고 스타일을 지정할 수 있습니다.

변환 순서는 클래스를 사용 하는 복합 변환에서 중요 합니다 `TransformGroup` . 예를 들어 경우 먼저 회전 다음 크기 조정, 변환, 결과 얻게 다른 보다 먼저 변환 하는 경우 다음 회전 하 고 확장 합니다. 한 가지 이유는 회전 및 크기 조정과 같은 변환이 좌표계의 원점을 기준으로 수행 된다는 것입니다. 원본에서 가운데에 있는 개체의 크기를 조정 하면 원점에서 벗어나 이동 된 개체의 크기를 조정 하는 다른 결과가 생성 됩니다. 마찬가지로, 개체를 회전 하면 원점에 중점을 두는 원본에서 이동 된 개체를 회전 다른 결과 생성 합니다.

다음 예제에서는 클래스를 사용 하 여 복합 변환을 수행 하는 방법을 보여 줍니다 `TransformGroup` .

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <TransformGroup>
            <ScaleTransform ScaleX="1.5"
                            ScaleY="1.5" />
            <RotateTransform Angle="45" />
        </TransformGroup>
    </Path.RenderTransform>
</Path>
```

이 예제에서 개체는 `Path` 크기를 1.5 배까지 확장 한 다음 45 도씩 회전 합니다.

## <a name="composite-transforms"></a>복합 변환

복합 변환은 개체에 여러 변환을 적용 합니다.

클래스 `CompositeTransform` 에서 파생 되는 클래스는 `Transform` 다음 속성을 정의 합니다.

- `CenterX`-형식의 이며, `double` 이 변환의 중심점의 x 좌표를 나타냅니다. 이 속성의 기본값은 0.0입니다.
- `CenterY`-형식의 이며, `double` 이 변환의 중심점의 y 좌표를 나타냅니다. 이 속성의 기본값은 0.0입니다.
- `ScaleX``double`x-축 배율 인수를 나타내는 형식의입니다. 이 속성의 기본값은 1.0입니다.
- `ScaleY``double`y-축 배율 인수를 나타내는 형식의입니다. 이 속성의 기본값은 1.0입니다.
- `SkewX``double`y-축에서 시계 반대 방향으로 측정 된 x-축 기울기 각도 (도)를 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.
- `SkewY``double`x-축에서 시계 반대 방향으로 측정 된 y-축 기울기 각도 (도)를 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.
- `Rotation`형식의는 `double` 시계 방향 회전의 각도 (도)를 나타냅니다. 이 속성의 기본값은 0.0입니다.
- `TranslateX``double`x-축을 따라 이동할 거리를 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.
- `TranslateY``double`y 축을 따라 이동할 거리를 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

는 `CompositeTransform` 다음 순서로 변환을 적용 합니다.

1. 크기 조정 ( `ScaleX` 및 `ScaleY` ).
1. 기울이기 ( `SkewX` 및 `SkewY` ).
1. 회전 ( `Rotation` ).
1. ( `TranslateX` ,)를 변환 `TranslateY` 합니다.

다른 순서로 개체에 여러 변환을 적용 하려는 경우를 만들고 원하는 `TransformGroup` 순서로 변환을 삽입 해야 합니다.

> [!IMPORTANT]
> 는 `CompositeTransform` `CenterX` `CenterY` 모든 변환에 대해 동일한 중심점, 및를 사용 합니다. 변환 당 다른 중심점을 지정 하려면를 사용 합니다. `TransformGroup`

다음 예제에서는 클래스를 사용 하 여 복합 변환을 수행 하는 방법을 보여 줍니다 `CompositeTransform` .

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <CompositeTransform ScaleX="1.5"
                            ScaleY="1.5"
                            Rotation="45"
                            TranslateX="50"
                            TranslateY="50" />
    </Path.RenderTransform>
</Path>
```

이 예제에서 개체는 `Path` 크기를 1.5 배까지 확장 한 다음 45도로 회전 한 다음 50 장치 독립적 단위로 변환 됩니다.

## <a name="transform-matrix"></a>행렬 변환

변환은 2D 공간에서 변환을 수행 하는 3x3 유사 변환 매트릭스와 관련 하 여 설명할 수 있습니다. 이 3x3 행렬은 `Matrix` 세 개의 행과 세 개의 값 열로 이루어진 컬렉션인 구조체로 표현 됩니다 `double` .

`Matrix`구조체는 다음 속성을 정의 합니다.

- `Determinant`는 `double` 행렬의 행렬식을 가져오는 형식의입니다.
- `HasInverse``bool`행렬을 반전할 수 있는지 여부를 나타내는 형식의입니다.
- `Identity`는 `Matrix` 항등 매트릭스를 가져오는 형식의입니다.
- `HasIdentity`는 `bool` 매트릭스가 항등 매트릭스 인지 여부를 나타내는 형식의입니다.
- `M11`는 `double` 행렬의 첫 번째 행과 첫 번째 열 값을 나타내는 형식입니다.
- `M12`는 `double` 행렬의 첫 번째 행과 두 번째 열 값을 나타내는 형식입니다.
- `M21`는 `double` 행렬의 두 번째 행과 첫 번째 열 값을 나타내는 형식입니다.
- `M22`는 `double` 행렬의 두 번째 행과 두 번째 열 값을 나타내는 형식입니다.
- `OffsetX``double`행렬의 세 번째 행과 첫 번째 열 값을 나타내는 형식의입니다.
- `OffsetY``double`행렬의 세 번째 행과 두 번째 열 값을 나타내는 형식의입니다.

`OffsetX`및 `OffsetY` 속성은 각각 x 축과 y 축을 따라 좌표 공간을 변환할 크기를 지정 하기 때문에 이름이 지정 됩니다.

또한 구조체는,, 등을 포함 하 여 `Matrix` 행렬 값을 조작 하는 데 사용할 수 있는 일련의 메서드를 노출 `Append` `Invert` `Multiply` `Prepend` 합니다.

다음 표에서는 행렬의 구조를 보여 줍니다 Xamarin.Forms .

| | | |
|---------|---------|-----|
| M11     | M12     | 0.0 |
| M21     | M22     | 0.0 |
| OffsetX | OffsetY | 1.0 |

> [!NOTE]
> 상관 변환 행렬의 최종 열은 (0, 0, 1)과 같으므로 처음 두 열의 멤버만 지정 해야 합니다.

행렬 값을 조작 하 여 개체를 회전, 크기 조정, 기울이기 및 변환할 수 있습니다 `Path` . 예를 들어 값을 100로 변경 하는 경우 `OffsetX` `Path` x 축을 따라 개체 100 장치 독립적 단위를 이동 하 여 사용할 수 있습니다. 값을 3으로 변경 하는 경우 `M22` 이 값을 사용 하 여 `Path` 개체를 현재 높이의 3 배로 늘일 수 있습니다. 두 값을 모두 변경 하는 경우 `Path` x 축을 따라 개체 100 장치 독립적 단위를 이동 하 고 높이를 3의 비율로 늘립니다. 또한 관계 변환 매트릭스를 곱하여 회전 및 기울이기와 같은 모든 수의 선형 변환을 형성 하 고 그 다음에 변환을 수행할 수 있습니다.

## <a name="custom-transforms"></a>사용자 지정 변환

클래스 `MatrixTransform` 에서 파생 되는 클래스는 `Transform` `Matrix` `Matrix` 변환을 정의 하는 행렬을 나타내는 형식의 속성을 정의 합니다. 이 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상이 될 수 있고 스타일을 지정할 수 있습니다.

,, 또는 개체를 사용 하 여 설명할 수 있는 변환은에서 `TranslateTransform` `ScaleTransform` 동일 하 게 설명할 `RotateTransform` `SkewTransform` 수 있습니다 `MatrixTransform` . 그러나,, `TranslateTransform` `ScaleTransform` `RotateTransform` 및 `SkewTransform` 클래스는에서 벡터 구성 요소를 설정 하는 것 보다 더 쉽게 사용할 `Matrix` 수 있습니다. 따라서 클래스는 `MatrixTransform` 일반적으로 `RotateTransform` ,, `ScaleTransform` `SkewTransform` 또는 클래스에서 제공 하지 않는 사용자 지정 변환을 만드는 데 사용 됩니다 `TranslateTransform` .

다음 예제에서는 `Path` 를 사용 하 여 개체를 변환 하는 방법을 보여 줍니다 `MatrixTransform` .

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <MatrixTransform>
            <MatrixTransform.Matrix>
                <!-- M11 stretches, M12 skews -->
                <Matrix OffsetX="10"
                        OffsetY="100"
                        M11="1.5"
                        M12="1" />
            </MatrixTransform.Matrix>
        </MatrixTransform>
    </Path.RenderTransform>
</Path>
```

이 예제에서 개체는 `Path` X 및 Y 차원에서 늘어나고, 기울어짐 및 offset입니다.

또는에 기본 제공 되는 형식 변환기를 사용 하는 간단한 형식으로 작성할 수 있습니다 Xamarin.Forms .

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <MatrixTransform Matrix="1.5,1,0,1,10,100" />
    </Path.RenderTransform>
</Path>
```

이 예제에서 속성은 `Matrix` `M11` ,,,, `M12` `M21` `M22` `OffsetX` , `OffsetY` 의 6 개 멤버로 구성 된 쉼표로 구분 된 문자열로 지정 됩니다. 이 예제에서 구성원은 쉼표로 구분 되지만 하나 이상의 공백으로 구분 될 수도 있습니다.

또한 이전 예제는 속성의 값과 같은 6 개의 멤버를 지정 하 여 훨씬 더 간소화할 수 있습니다 `RenderTransform` .

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      RenderTransform="1.5 1 0 1 10 100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z" />
```

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms셰이프도](index.md)
