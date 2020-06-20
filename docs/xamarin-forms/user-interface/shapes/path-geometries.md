---
title: 'Xamarin.Forms도형: 경로 기 하 도형'
description: Xamarin.Forms경로 기 하 도형 클래스를 사용 하 여 2D 셰이프의 기 하 도형에 대해 설명할 수 있습니다.
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5718b0594581928e6f00e11a15163d176615378f
ms.sourcegitcommit: d86b7a18cf8b1ef28cd0fe1d311f1c58a65101a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85101881"
---
# <a name="xamarinforms-shapes-path-geometries"></a>Xamarin.Forms도형: 경로 기 하 도형

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

`Geometry`클래스 및이 클래스에서 파생 되는 클래스를 사용 하 여 2d 셰이프의 기 하 도형에 대해 설명할 수 있습니다. `Geometry`개체는 사각형 및 원과 같은 단순 또는 두 개 이상의 geometry 개체에서 만든 복합 일 수 있습니다. `PathGeometry`원호 및 곡선을 설명할 수 있는 클래스를 사용 하 여 더 복잡 한 기 하 도형을 만들 수 있습니다.

`Geometry`및 `Shape` 클래스는 모두 2d 셰이프를 설명 하는 것 처럼 보이지만 중요 한 차이점이 있습니다. 클래스는 클래스에서 `Geometry` 파생 `BindableObject` 되는 반면 클래스는 클래스 `Shape` 에서 파생 `View` 됩니다. 따라서 개체 `Shape` 는 자신을 렌더링 하 고 레이아웃 시스템에 참여할 수 있지만 `Geometry` 개체는 그렇지 않습니다. 개체는 개체 `Shape` 보다 더 쉽게 사용할 수 있지만 `Geometry` 개체는 `Geometry` 더 다양 합니다. 개체를 사용 하 여 2D 그래픽을 렌더링 하는 동안 `Shape` `Geometry` 개체를 사용 하 여 2d 그래픽의 기하학적 영역을 정의 하 고 클리핑 영역을 정의할 수 있습니다.

모든 기 하 도형에 대 한 기본 클래스는 추상 클래스 `Geometry` 입니다. 이 클래스에서 파생 되는 클래스는 간단한 기 하 도형, 경로 기 하 도형 및 복합 기 하 도형 등의 세 가지 범주로 그룹화 할 수 있습니다.

## <a name="simple-geometries"></a>간단한 기 하 도형

단순 geometry 클래스는 `EllipseGeometry` , `LineGeometry` 및 `RectangleGeometry` 입니다. 원형, 선, 사각형 등의 기본 기하학적 모양을 만드는 데 사용 됩니다. 이러한 동일한 모양과 보다 복잡 한 셰이프는를 사용 하 여 만들거나 `PathGeometry` geometry 개체를 함께 결합 하 여 만들 수 있지만 이러한 클래스를 사용 하면 이러한 기본 기하학적 셰이프를 보다 간단 하 게 만들 수 있습니다.

### <a name="ellipsegeometry"></a>EllipseGeometry

타원 기 하 도형은 기 하 도형 또는 타원 또는 원을 나타내고, 중심점, x 반지름 및 y 반지름에 의해 정의 됩니다.

`EllipseGeometry`클래스는 다음 속성을 정의 합니다.

- `Center`는 `Point` 도형의 중심점을 나타내는 형식의입니다.
- `RadiusX``double`기 하 도형의 x 반경 값을 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.
- `RadiusY``double`기 하 도형의 y 반지름 값을 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

다음 예제에서는를 만들고 렌더링 하는 방법을 보여 줍니다 `EllipseGeometry` .

```xaml
<Path Fill="Blue"
      Stroke="Red"
      StrokeThickness="1">
  <Path.Data>
    <EllipseGeometry Center="50,50"
                     RadiusX="50"
                     RadiusY="50" />
  </Path.Data>
</Path>
```

이 예제에서의 중심은 `EllipseGeometry` (50, 50)로 설정 되 고, x 반지름 및 y 반지름은 모두 50로 설정 됩니다. 이렇게 하면 100 지름이 파란색으로 칠해진 원이 생성 됩니다.

### <a name="linegeometry"></a>LineGeometry

선 기 하 도형은 선의 기 하 도형을 나타내고 선 및 끝점의 시작점을 지정 하 여 정의 됩니다.

`LimeGeometry`클래스는 다음 속성을 정의 합니다.

- `StartPoint`는 `Point` 선의 시작점을 나타내는 형식의입니다.
- `EndPoint``Point`선의 끝점을 나타내는 형식의입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

다음 예제에서는를 만들고 렌더링 하는 방법을 보여 줍니다 `LineGeometry` .

```xaml
<Path Stroke="Black"
      StrokeThickness="1">
  <Path.Data>
    <LineGeometry StartPoint="10,20"
                  EndPoint="100,130" />
  </Path.Data>
</Path>
```

이 예제에서는 `LineGeometry` (10, 20)에서 (100130)로 그려집니다.

### <a name="rectanglegeometry"></a>RectangleGeometry

사각형 기 하 도형은 사각형을 나타내며 `Rect` 상대 위치와 높이 및 너비를 지정 하는 구조체를 사용 하 여 정의 됩니다.

`RectangleGeometry`클래스는 다음 속성을 정의 합니다.

- `Rect``FormsRect`사각형의 크기를 나타내는 형식의입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

다음 예제에서는를 만들고 렌더링 하는 방법을 보여 줍니다 `RectangleGeometry` .

```xaml
<Path Fill="Blue"
      Stroke="Black"
      StrokeThickness="1">
  <Path.Data>
    <RectangleGeometry Rect="50,50,25,25" />
  </Path.Data>
</Path>
```

사각형의 위치와 크기는 구조체에 의해 정의 됩니다 `Rect` . 이 예제에서 위치는 (50, 50)이 고 높이와 너비는 모두 25 이며 사각형을 만듭니다.

## <a name="path-geometries"></a>경로 기 하 도형

경로 기 하 도형은 원호, 곡선, 타원, 선, 사각형으로 구성 될 수 있는 복잡 한 도형을 설명 합니다.

`PathGeometry`클래스는 다음 속성을 정의 합니다.

- `Figures`는 `PathFigureCollection` `PathFigure` 경로의 콘텐츠를 설명 하는 개체의 컬렉션을 나타내는 형식의입니다.
- `FillRule``FillRule`기 하 도형에 포함 된 교차 영역이 결합 되는 방법을 결정 하는 형식의입니다. 이 속성의 기본값은 `FillRule.EvenOdd`입니다.

> [!NOTE]
> `Figures`속성은 `ContentProperty` 클래스의입니다 `PathGeometry` . 따라서 XAML에서 명시적으로 설정할 필요가 없습니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

는 `PathGeometry` 개체의 컬렉션으로 구성 되며 `PathFigure` 각 개체는 기 하 `PathFigure` 도형의 셰이프를 설명 합니다. 각 `PathFigure` 은 각각 `PathSegment` 모양의 세그먼트를 설명 하는 하나 이상의 개체로 구성 됩니다. 세그먼트에는 여러 가지 유형이 있습니다.

- `ArcSegment`-두 요소 사이에 타원형 호를 만듭니다.
- `BezierSegment`-두 요소 사이에 입방 형 3 차원 곡선을 만듭니다.
- `LineSegment`-두 요소 사이에 선을 만듭니다.
- `PolyBezierSegment`는 일련의 입방 형 3 차원 곡선을 만듭니다.
- `PolyLineSegment`-일련의 줄을 만듭니다.
- `PolyQuadraticBezierSegment`는 일련의 정방형 3 차원 곡선을 만듭니다.
- `QuadraticBezierSegment`는 정방형 3 차원 곡선을 만듭니다.

내의 세그먼트는 `PathFigure` 각 세그먼트의 끝점이 다음 세그먼트의 시작점 인 단일 기 하 도형으로 결합 됩니다. `StartPoint`의 속성은 `PathFigure` 첫 번째 세그먼트를 그릴 지점을 지정 합니다. 각 후속 세그먼트는 이전 세그먼트의 끝점에서 시작합니다. 예를 들어 `10,50` `10,150` 속성을로 설정 하 `StartPoint` `10,50` 고 `LineSegment` `Point` 속성 설정을 사용 `10,150` 하 여를 만들면에서로의 세로선을 정의할 수 있습니다.

```xaml
<Path Stroke="Black"
      StrokeThickness="1">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,50">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <LineSegment Point="10,150" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

개체의 조합을 사용 하 `PathSegment` 고 내의 여러 개체를 사용 하 여 더 복잡 한 기 하 도형을 만들 수 있습니다 `PathFigure` `PathGeometry` .

## <a name="composite-geometries"></a>복합 기 하 도형

복합 geometry 개체는를 사용 하 여 만들 수 있습니다 `GeometryGroup` . `GeometryGroup`클래스는 하나 이상의 개체에서 복합 기 하 도형을 만듭니다 `Geometry` . 에는 원하는 수의 `Geometry` 개체를 추가할 수 있습니다 `GeometryGroup` .

다음 예제에서는에서 기 하 도형을 결합 하는 방법을 보여 줍니다 `GeometryGroup` .

```xaml
<Path Stroke="Green"
      StrokeThickness="2"
      Fill="Orange">
    <Path.Data>
        <GeometryGroup>
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="150,150" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="250,150" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="150,250" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="250,250" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

이 예제에서는 `EllipseGeometry` 서로 다른 중심 좌표를 사용 하 여 동일한 x 반지름 및 y 반지름 좌표를 사용 하는 네 개의 개체를 결합 합니다. 그러면 겹치는 네 개의 원이 생성 되 고,이는 해당 내부가 채우기 규칙을 사용 하 여 주황색으로 채워집니다 `EvenOdd` .

## <a name="other-features"></a>기타 기능

`GeometryHelper`클래스는 다음과 같은 도우미 메서드를 제공 합니다.

- `FlattenGeometry`
- `FlattenCubicBezier`
- `FlattenQuadraticBezier`
- `FlattenArc`

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.Forms셰이프도](index.md)
