---
title: 'Xamarin.Forms도형: 기 하 도형'
description: Xamarin.Formsgeometry 클래스를 사용 하 여 2D 도형의 기 하 도형을 설명할 수 있습니다.
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 00c11c07e796ffa24c3ed1f23ca3ea29885a3d8b
ms.sourcegitcommit: 91b4d2f93687fadec5c3f80aadc8f7298d911624
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85795171"
---
# <a name="xamarinforms-shapes-geometries"></a>Xamarin.Forms도형: 기 하 도형

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Geometry`클래스 및이 클래스에서 파생 되는 클래스를 사용 하 여 2d 셰이프의 기 하 도형에 대해 설명할 수 있습니다. `Geometry`개체는 사각형 및 원과 같은 단순 또는 두 개 이상의 geometry 개체에서 만든 복합 일 수 있습니다. 또한 원호 및 곡선을 포함 하는 더 복잡 한 기 하 도형을 만들 수 있습니다.

`Geometry`클래스는 다양 한 종류의 기 하 도형을 정의 하는 여러 클래스의 부모 클래스입니다.

- `EllipseGeometry`는 타원 또는 원의 기 하 도형을 나타냅니다.
- `GeometryGroup`는 여러 geometry 개체를 단일 개체로 결합할 수 있는 컨테이너를 나타냅니다.
- `LineGeometry`는 선의 기 하 도형을 나타냅니다.
- `PathGeometry`는 원호, 곡선, 타원, 선, 사각형으로 구성 될 수 있는 복잡 한 모양의 기 하 도형을 나타냅니다.
- `RectangleGeometry`는 사각형 또는 사각형의 기 하 도형을 나타냅니다.

`Geometry`및 `Shape` 클래스는 모두 2d 셰이프를 설명 하는 것 처럼 보이지만 중요 한 차이점이 있습니다. 클래스는 클래스에서 `Geometry` 파생 [`BindableObject`](xref:Xamarin.Forms.BindableObject) 되는 반면 클래스는 클래스 `Shape` 에서 파생 [`View`](xref:Xamarin.Forms.View) 됩니다. 따라서 개체 `Shape` 는 자신을 렌더링 하 고 레이아웃 시스템에 참여할 수 있지만 `Geometry` 개체는 그렇지 않습니다. 개체는 개체 `Shape` 보다 더 쉽게 사용할 수 있지만 `Geometry` 개체는 `Geometry` 더 다양 합니다. 개체를 사용 하 여 2D 그래픽을 렌더링 하는 동안 `Shape` `Geometry` 개체를 사용 하 여 2d 그래픽의 기하학적 영역을 정의 하 고 클리핑 영역을 정의할 수 있습니다.

다음 클래스에는 개체에 대해 설정할 수 있는 속성이 있습니다 `Geometry` .

- `Path`클래스는를 사용 하 여 `Geometry` 해당 내용을 설명 합니다. `Geometry`속성을 개체로 설정 하 `Path.Data` `Geometry` 고 `Path` 개체의 `Fill` 및 속성을 설정 `Stroke` 하 여를 렌더링할 수 있습니다.
- 클래스에는 [`VisualElement`](xref:Xamarin.Forms.VisualElement) `Clip` `Geometry` 요소의 콘텐츠의 윤곽선을 정의 하는 형식의 속성이 있습니다. `Clip`속성이 개체로 설정 된 경우 `Geometry` 의 영역 내에 있는 영역만 `Geometry` 표시 됩니다. 자세한 내용은 [Geometry를 사용 하 여 클리핑](#clip-with-a-geometry)을 참조 하세요.

클래스에서 파생 되는 클래스는 `Geometry` 간단한 기 하 도형, 경로 기 하 도형 및 복합 기 하 도형 등의 세 가지 범주로 그룹화 할 수 있습니다.

## <a name="simple-geometries"></a>간단한 기 하 도형

단순 geometry 클래스는 `EllipseGeometry` , `LineGeometry` 및 `RectangleGeometry` 입니다. 원형, 선, 사각형 등의 기본 기하학적 모양을 만드는 데 사용 됩니다. 이러한 동일한 모양과 더 복잡 한 셰이프는를 사용 하 여 만들거나 `PathGeometry` geometry 개체를 함께 결합 하 여 만들 수 있지만 이러한 클래스는 이러한 기본 기하학적 셰이프를 생성 하는 간단한 방법을 제공 합니다.

### <a name="ellipsegeometry"></a>EllipseGeometry

타원 기 하 도형은 기 하 도형 또는 타원 또는 원을 나타내고, 중심점, x 반지름 및 y 반지름에 의해 정의 됩니다.

`EllipseGeometry` 클래스는 다음과 같은 속성을 정의합니다.

- `Center`는 [`Point`](xref:Xamarin.Forms.Point) 도형의 중심점을 나타내는 형식의입니다.
- `RadiusX``double`기 하 도형의 x 반경 값을 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.
- `RadiusY``double`기 하 도형의 y 반지름 값을 나타내는 형식의입니다. 이 속성의 기본값은 0.0입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

다음 예제에서는 개체에서를 만들고 렌더링 하는 방법을 보여 줍니다 `EllipseGeometry` `Path` .

```xaml
<Path Fill="Blue"
      Stroke="Red">
  <Path.Data>
    <EllipseGeometry Center="50,50"
                     RadiusX="50"
                     RadiusY="50" />
  </Path.Data>
</Path>
```

이 예제에서의 중심은 `EllipseGeometry` (50, 50)로 설정 되 고, x 반지름 및 y 반지름은 모두 50로 설정 됩니다. 이렇게 하면 내부가 파란색으로 칠해진 100 장치 독립적 단위의 빨간색 원이 생성 됩니다.

![EllipseGeometry](geometry-images/ellipse.png "EllipseGeometry")

### <a name="linegeometry"></a>LineGeometry

선 기 하 도형은 선의 기 하 도형을 나타내고 선 및 끝점의 시작점을 지정 하 여 정의 됩니다.

`LineGeometry` 클래스는 다음과 같은 속성을 정의합니다.

- `StartPoint`는 [`Point`](xref:Xamarin.Forms.Point) 선의 시작점을 나타내는 형식의입니다.
- `EndPoint`[`Point`](xref:Xamarin.Forms.Point)선의 끝점을 나타내는 형식의입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

다음 예제에서는 개체에서를 만들고 렌더링 하는 방법을 보여 줍니다 `LineGeometry` `Path` .

```xaml
<Path Stroke="Black">
  <Path.Data>
    <LineGeometry StartPoint="10,20"
                  EndPoint="100,130" />
  </Path.Data>
</Path>
```

이 예제에서는 `LineGeometry` (10, 20)에서 (100130)로 그려집니다.

![LineGeometry](geometry-images/line.png "LineGeometry")

> [!NOTE]
> 줄에 `Fill` 내부가 없기 때문에를 렌더링 하는의 속성을 설정 해도 `Path` `LineGeometry` 아무 효과가 없습니다.

### <a name="rectanglegeometry"></a>RectangleGeometry

사각형 기 하 도형은 사각형 또는 사각형의 기 하 도형을 나타내며 `Rect` 상대 위치와 높이 및 너비를 지정 하는 구조체를 사용 하 여 정의 됩니다.

`RectangleGeometry`클래스는 `Rect` 사각형의 크기를 나타내는 형식의 속성을 정의 합니다 [`Rectangle`](xref:Xamarin.Forms.Rectangle) . 이 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상이 될 수 있고 스타일을 지정할 수 있습니다.

다음 예제에서는 개체에서를 만들고 렌더링 하는 방법을 보여 줍니다 `RectangleGeometry` `Path` .

```xaml
<Path Fill="Blue"
      Stroke="Red">
  <Path.Data>
    <RectangleGeometry Rect="10,10,150,100" />
  </Path.Data>
</Path>
```

사각형의 위치와 크기는 구조체에 의해 정의 됩니다 `Rect` . 이 예제에서 위치는 (10, 10)이 고 너비는 150 이며 높이는 100 장치 독립적 단위입니다.

![RectangleGeometry](geometry-images/rectangle.png "RectangleGeometry")

## <a name="path-geometries"></a>경로 기 하 도형

경로 기 하 도형은 원호, 곡선, 타원, 선, 사각형으로 구성 될 수 있는 복잡 한 도형을 설명 합니다.

`PathGeometry` 클래스는 다음과 같은 속성을 정의합니다.

- `Figures`는 `PathFigureCollection` `PathFigure` 경로의 콘텐츠를 설명 하는 개체의 컬렉션을 나타내는 형식의입니다.
- `FillRule``FillRule`기 하 도형에 포함 된 교차 영역이 결합 되는 방법을 결정 하는 형식의입니다. 이 속성의 기본값은 `FillRule.EvenOdd`입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

열거형에 대 한 자세한 내용은 `FillRule` [ Xamarin.Forms 셰이프: 채우기 규칙](fillrules.md)을 참조 하세요.

> [!NOTE]
> `Figures`속성은 `ContentProperty` 클래스의입니다 `PathGeometry` . 따라서 XAML에서 명시적으로 설정할 필요가 없습니다.

는 `PathGeometry` 개체의 컬렉션으로 구성 되며 `PathFigure` 각 개체는 기 하 `PathFigure` 도형의 셰이프를 설명 합니다. 각 `PathFigure` 은 각각 `PathSegment` 모양의 세그먼트를 설명 하는 하나 이상의 개체로 구성 됩니다. 세그먼트에는 여러 가지 유형이 있습니다.

- `ArcSegment`-두 요소 사이에 타원형 호를 만듭니다.
- `BezierSegment`-두 요소 사이에 입방 형 3 차원 곡선을 만듭니다.
- `LineSegment`-두 요소 사이에 선을 만듭니다.
- `PolyBezierSegment`는 일련의 입방 형 3 차원 곡선을 만듭니다.
- `PolyLineSegment`-일련의 줄을 만듭니다.
- `PolyQuadraticBezierSegment`는 일련의 정방형 3 차원 곡선을 만듭니다.
- `QuadraticBezierSegment`는 정방형 3 차원 곡선을 만듭니다.

위의 모든 클래스는 추상 클래스에서 파생 `PathSegment` 됩니다.

내의 세그먼트는 `PathFigure` 각 세그먼트의 끝점이 다음 세그먼트의 시작점 인 단일 기 하 도형으로 결합 됩니다. `StartPoint`의 속성은 `PathFigure` 첫 번째 세그먼트를 그릴 지점을 지정 합니다. 각 후속 세그먼트는 이전 세그먼트의 끝점에서 시작합니다. 예를 들어 `10,50` `10,150` 속성을로 설정 하 `StartPoint` `10,50` 고 `LineSegment` `Point` 속성 설정을 사용 `10,150` 하 여를 만들면에서로의 세로선을 정의할 수 있습니다.

```xaml
<Path Stroke="Black">
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

### <a name="create-an-arcsegment"></a>ArcSegment 만들기

는 `ArcSegment` 두 요소 사이에 타원형 원호를 만듭니다. 타원형 호를 해당 시작 및 끝 지점 x 및 y 반지름을 x 축 회전 요소인 원호가 180도 및 호를 그릴 방향을 설명 하는 값 보다 커야 하는지 여부를 나타내는 값으로 정의 됩니다.

`ArcSegment` 클래스는 다음과 같은 속성을 정의합니다.

- `Point`는 [`Point`](xref:Xamarin.Forms.Point) 타원형 호의 끝점을 나타내는 형식의입니다. 이 속성의 기본값은 (0, 0)입니다.
- `Size`[`Size`](xref:Xamarin.Forms.Size)-원호의 x 및 y 반지름을 나타내는 형식의입니다. 이 속성의 기본값은 (0, 0)입니다.
- `RotationAngle``double`x 축을 중심으로 타원이 회전 하는 각도 (도)를 나타내는 형식의입니다. 이 속성의 기본값은 0입니다.
- `SweepDirection``SweepDirection`호를 그릴 방향을 지정 하는 형식의입니다. 이 속성의 기본값은 `SweepDirection.CounterClockwise`입니다.
- `IsLargeArc`는 원호의이 `bool` 180도 보다 큰지 여부를 나타내는 형식의입니다. 이 속성의 기본값은 `false`입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

> [!NOTE]
> `ArcSegment`클래스에 원호의 시작점에 대 한 속성이 포함 되어 있지 않습니다. 나타내는 원호의 끝점만 정의 합니다. 원호의 시작점은 `PathFigure` 가 추가 되는의 현재 지점입니다 `ArcSegment` .

`SweepDirection` 열거형은 다음 멤버를 정의합니다.

- `CounterClockwise`-호가 시계 방향으로 그려지도록 지정 합니다.
- `Clockwise`-호가 시계 방향으로 카운터에 그려지도록 지정 합니다.

다음 예제에서는 개체에서를 만들고 렌더링 하는 방법을 보여 줍니다 `ArcSegment` `Path` .

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,100">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <ArcSegment Size="100,50"
                                            RotationAngle="45"
                                            IsLargeArc="True"
                                            SweepDirection="CounterClockwise"
                                            Point="200,100" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

이 예제에서 타원형 원호는 (10100)에서 (200100)로 그려집니다.

### <a name="create-a-beziersegment"></a>System.windows.media.beziersegment> 만들기

는 `BezierSegment` 두 요소 사이에 입방 형 3 차원 곡선을 만듭니다. 입방 형 3 차원 곡선은 시작점, 끝점 및 두 개의 제어점으로 정의 됩니다.

`BezierSegment` 클래스는 다음과 같은 속성을 정의합니다.

- `Point1`는 [`Point`](xref:Xamarin.Forms.Point) 곡선의 첫 번째 제어점을 나타내는 형식의입니다. 이 속성의 기본값은 (0, 0)입니다.
- `Point2`는 [`Point`](xref:Xamarin.Forms.Point) 곡선의 두 번째 제어점을 나타내는 형식의입니다. 이 속성의 기본값은 (0, 0)입니다.
- `Point3`[`Point`](xref:Xamarin.Forms.Point)-곡선의 끝점을 나타내는 형식의입니다. 이 속성의 기본값은 (0, 0)입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

> [!NOTE]
> `BezierSegment`클래스에 곡선의 시작점에 대 한 속성이 포함 되어 있지 않습니다. 곡선의 시작점은 `PathFigure` 가 추가 되는의 현재 지점입니다 `BezierSegment` .

입방 형 3 차원 곡선의 두 개의 제어점 자석, 자체적으로 직선 수의 일부를 끌어들이고 곡선을 만듭니다 처럼 동작 합니다. 첫 번째 제어점은 곡선의 시작 부분에 영향을 줍니다. 두 번째 제어점은 곡선의 끝 부분에 영향을 줍니다. 곡선이 반드시 제어 요소 중 하나를 통과 하는 것은 아닙니다. 대신 각 제어점은 줄의 해당 부분을 자체적으로 이동 하지만 자체적으로는 이동 하지 않습니다.

다음 예제에서는 개체에서를 만들고 렌더링 하는 방법을 보여 줍니다 `BezierSegment` `Path` .

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <BezierSegment Point1="100,0"
                                               Point2="200,200"
                                               Point3="300,10" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

이 예에서는 (10, 10)에서 (300, 10)까지 입방 형 3 차원 곡선을 그립니다. 곡선에는 (100, 0) 및 (200200)에 두 개의 제어점이 있습니다.

![System.windows.media.beziersegment>](geometry-images/beziersegment.png "System.windows.media.beziersegment>")

### <a name="create-a-linesegment"></a>LineSegment 만들기

는 `LineSegment` 두 요소 사이에 선을 만듭니다.

`LineSegment`클래스는 `Point` [`Point`](xref:Xamarin.Forms.Point) 선 세그먼트의 끝점을 나타내는 형식의 속성을 정의 합니다. 이 속성의 기본값은 (0, 0) 이며 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

> [!NOTE]
> `LineSegment` 클래스 선의 시작점에 대 한 속성을 포함 하지 않습니다. 종료 지점만 정의 합니다. 줄의 시작점은 `PathFigure` 가 추가 되는의 현재 지점입니다 `LineSegment` .

다음 예제에서는 개체에서 개체를 만들고 렌더링 하는 방법을 보여 줍니다 `LineSegment` `Path` .

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Start">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure IsClosed="True"
                                StartPoint="10,100">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <LineSegment Point="100,100" />
                                <LineSegment Point="100,50" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

이 예제에서 줄 세그먼트는 (10100)에서 (100100)로 그리고 (100100)에서 (100, 50)로 그려집니다. 또한의 `PathFigure` 속성이로 설정 되어 있으므로이 닫혀 `IsClosed` `true` 있습니다. 이로 인해 삼각형이 그려집니다.

![LineSegments](geometry-images/linesegments.png "LineSegments")

### <a name="create-a-polybeziersegment"></a>PolyBezierSegment 만들기

는 `PolyBezierSegment` 하나 이상의 입방 형 3 차원 곡선을 만듭니다.

클래스는을 `PolyBezierSegment` `Points` 정의 하는 요소를 나타내는 형식의 속성을 정의 합니다 `PointCollection` `PolyBezierSegment` . 는 `PointCollection` `ObservableCollection` 개체의입니다 [`Point`](xref:Xamarin.Forms.Point) . 이 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상이 될 수 있고 스타일을 지정할 수 있습니다.

> [!NOTE]
> `PolyBezierSegment`클래스에 곡선의 시작점에 대 한 속성이 포함 되어 있지 않습니다. 곡선의 시작점은 `PathFigure` 가 추가 되는의 현재 지점입니다 `PolyBezierSegment` .

다음 예제에서는 개체에서를 만들고 렌더링 하는 방법을 보여 줍니다 `PolyBezierSegment` `Path` .

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <PolyBezierSegment Points="0,0 100,0 150,100 150,0 200,0 300,10" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

이 예제에서은 `PolyBezierSegment` 두 개의 입방 형 3 차원 곡선을 지정 합니다. 첫 번째 곡선은 (10, 10)에서 (150100)의 제어점 (0, 0)과 다른 제어점 (100, 0)으로 시작 합니다. 두 번째 곡선은 (150100)에서 (300, 10), 제어점이 (150, 0)이 고, 다른 제어점은 (200, 0)입니다.

![PolyBezierSegment](geometry-images/polybeziersegment.png "PolyBezierSegment")

### <a name="create-a-polylinesegment"></a>PolyLineSegment 만들기

는 `PolyLineSegment` 하나 이상의 선 세그먼트를 만듭니다.

클래스는을 `PolyLineSegment` `Points` 정의 하는 요소를 나타내는 형식의 속성을 정의 합니다 `PointCollection` `PolyLineSegment` . 는 `PointCollection` `ObservableCollection` 개체의입니다 [`Point`](xref:Xamarin.Forms.Point) . 이 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상이 될 수 있고 스타일을 지정할 수 있습니다.

> [!NOTE]
> `PolyLineSegment` 클래스 선의 시작점에 대 한 속성을 포함 하지 않습니다. 줄의 시작점은 `PathFigure` 가 추가 되는의 현재 지점입니다 `PolyLineSegment` .

다음 예제에서는 개체에서를 만들고 렌더링 하는 방법을 보여 줍니다 `PolyLineSegment` `Path` .

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigure StartPoint="10,10">
                    <PathFigure.Segments>
                        <PolyLineSegment Points="50,10 50,50" />
                    </PathFigure.Segments>
                </PathFigure>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

이 예제에서은 `PolyLineSegment` 두 줄을 지정 합니다. 첫 번째 줄은 (10, 10)에서 (50, 10) 사이이 고 두 번째 줄은 (50, 10)에서 (50, 50) 사이입니다.

![PolyLineSegment](geometry-images/polylinesegment.png "PolyLineSegment")

### <a name="create-a-polyquadraticbeziersegment"></a>PolyQuadraticBezierSegment 만들기

는 `PolyQuadraticBezierSegment` 하나 이상의 정방형 3 차원 곡선을 만듭니다.

클래스는을 `PolyQuadraticBezierSegment` `Points` 정의 하는 요소를 나타내는 형식의 속성을 정의 합니다 `PointCollection` `PolyQuadraticBezierSegment` . 는 `PointCollection` `ObservableCollection` 개체의입니다 [`Point`](xref:Xamarin.Forms.Point) . 이 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상이 될 수 있고 스타일을 지정할 수 있습니다.

> [!NOTE]
> `PolyQuadraticBezierSegment`클래스에 곡선의 시작점에 대 한 속성이 포함 되어 있지 않습니다. 곡선의 시작점은 `PathFigure` 가 추가 되는의 현재 지점입니다 `PolyQuadraticBezierSegment` .

다음 예제에서는 개체에서를 만들고 렌더링 하는 방법을 보여 줍니다 `PolyQuadraticBezierSegment` `Path` .

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <PolyQuadraticBezierSegment Points="100,100 150,50 0,100 15,200" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

이 예제에서은 `PolyQuadraticBezierSegment` 두 개의 베 지 어 곡선을 지정 합니다. 첫 번째 곡선은 (10, 10)에서 (150, 50)부터 제어 지점이 (100100)에 있습니다. 두 번째 곡선은 (0100)에서 제어 지점이 있는 (100100)에서 (15200) 사이입니다.

![PolyQuadraticBezierSegment](geometry-images/polyquadraticbeziersegment.png "PolyQuadraticBezierSegment")

### <a name="create-a-quadraticbeziersegment"></a>QuadraticBezierSegment 만들기

는 `QuadraticBezierSegment` 두 요소 사이에 정방형 3 차원 곡선을 만듭니다.

`QuadraticBezierSegment` 클래스는 다음과 같은 속성을 정의합니다.

- `Point1`는 [`Point`](xref:Xamarin.Forms.Point) 곡선의 제어점을 나타내는 형식의입니다. 이 속성의 기본값은 (0, 0)입니다.
- `Point2`[`Point`](xref:Xamarin.Forms.Point)-곡선의 끝점을 나타내는 형식의입니다. 이 속성의 기본값은 (0, 0)입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

> [!NOTE]
> `QuadraticBezierSegment`클래스에 곡선의 시작점에 대 한 속성이 포함 되어 있지 않습니다. 곡선의 시작점은 `PathFigure` 가 추가 되는의 현재 지점입니다 `QuadraticBezierSegment` .

다음 예제에서는 개체에서를 만들고 렌더링 하는 방법을 보여 줍니다 `QuadraticBezierSegment` `Path` .

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <QuadraticBezierSegment Point1="200,200"
                                                        Point2="300,10" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

이 예에서는 (10, 10)에서 (300, 10)까지 정방형 3 차원 곡선을 그립니다. 곡선의 제어점은 (200200)입니다.

![QuadraticBezierSegment](geometry-images/quadraticbeziersegment.png "QuadraticBezierSegment")

### <a name="create-complex-geometries"></a>복잡 한 기 하 도형 만들기

개체의 조합을 사용 하 여 더 복잡 한 기 하 도형을 만들 수 있습니다 `PathSegment` . 다음 예제에서는, 및를 사용 하 여 셰이프를 만듭니다 `BezierSegment` `LineSegment` `ArcSegment` .

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigure StartPoint="10,50">
                    <PathFigure.Segments>
                        <BezierSegment Point1="100,0"
                                       Point2="200,200"
                                       Point3="300,100"/>
                        <LineSegment Point="400,100" />
                        <ArcSegment Size="50,50"
                                    RotationAngle="45"
                                    IsLargeArc="True"
                                    SweepDirection="Clockwise"
                                    Point="200,100"/>
                    </PathFigure.Segments>
                </PathFigure>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

이 예에서는 `BezierSegment` 4 개의 요소를 사용 하 여를 먼저 정의 합니다. 그런 다음이 예제에서는의 끝점 사이에서로 지정 된 지점 사이에 그려지는를 추가 합니다 `LineSegment` `BezierSegment` `LineSegment` . 마지막으로는의 `ArcSegment` 끝점에서로 지정 된 지점까지 그려집니다 `LineSegment` `ArcSegment` .

내에서 여러 개체를 사용 하 여 더 복잡 한 기 하 도형을 만들 수 있습니다 `PathFigure` `PathGeometry` . 다음 예제에서는 `PathGeometry` `PathFigure` 여러 개체를 포함 하는 7 개 개체에서을 만듭니다 `PathSegment` .

```xaml
<Path Stroke="Red"
      StrokeThickness="12"
      StrokeLineJoin="Round">
    <Path.Data>
        <PathGeometry>
            <!-- H -->
            <PathFigure StartPoint="0,0">
                <LineSegment Point="0,100" />
            </PathFigure>
            <PathFigure StartPoint="0,50">
                <LineSegment Point="50,50" />
            </PathFigure>
            <PathFigure StartPoint="50,0">
                <LineSegment Point="50,100" />
            </PathFigure>

            <!-- E -->
            <PathFigure StartPoint="125, 0">
                <BezierSegment Point1="60, -10"
                               Point2="60, 60"
                               Point3="125, 50" />
                <BezierSegment Point1="60, 40"
                               Point2="60, 110"
                               Point3="125, 100" />
            </PathFigure>

            <!-- L -->
            <PathFigure StartPoint="150, 0">
                <LineSegment Point="150, 100" />
                <LineSegment Point="200, 100" />
            </PathFigure>

            <!-- L -->
            <PathFigure StartPoint="225, 0">
                <LineSegment Point="225, 100" />
                <LineSegment Point="275, 100" />
            </PathFigure>

            <!-- O -->
            <PathFigure StartPoint="300, 50">
                <ArcSegment Size="25, 50"
                            Point="300, 49.9"
                            IsLargeArc="True" />
            </PathFigure>
        </PathGeometry>
    </Path.Data>
</Path>
```

이 예제에서 "Hello" 라는 단어는 `LineSegment` `BezierSegment` 단일 개체와 함께 및 개체의 조합을 사용 하 여 그려집니다 `ArcSegment` .

![여러 PathFigure 개체](geometry-images/multiple-pathfigures.png "여러 PathFigure 개체")

## <a name="composite-geometries"></a>복합 기 하 도형

복합 geometry 개체는를 사용 하 여 만들 수 있습니다 `GeometryGroup` . `GeometryGroup`클래스는 하나 이상의 개체에서 복합 기 하 도형을 만듭니다 `Geometry` . 에는 원하는 수의 `Geometry` 개체를 추가할 수 있습니다 `GeometryGroup` .

`GeometryGroup` 클래스는 다음과 같은 속성을 정의합니다.

- `Children`는를 정의 하는 개체를 지정 하는 형식의입니다 `GeometryCollection` `GeomtryGroup` . 는 `GeometryCollection` `ObservableCollection` 개체의입니다 `Geometry` .
- `FillRule``FillRule`는의 교차 영역을 결합 하는 방법을 지정 하는 형식의입니다 `GeometryGroup` . 이 속성의 기본값은 `FillRule.EvenOdd`입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

> [!NOTE]
> `Children`속성은 `ContentProperty` 클래스의입니다 `GeometryGroup` . 따라서 XAML에서 명시적으로 설정할 필요가 없습니다.

열거형에 대 한 자세한 내용은 `FillRule` [ Xamarin.Forms 셰이프: 채우기 규칙](fillrules.md)을 참조 하세요.

복합 기 하 도형을 그리려면 필수 `Geometry` 개체를의 자식으로 설정 하 `GeometryGroup` 고 개체를 사용 하 여 표시 합니다 `Path` . 다음 XAML은 이러한 예를 보여 줍니다.

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

이 예제에서는 `EllipseGeometry` 서로 다른 중심 좌표를 사용 하 여 동일한 x 반지름 및 y 반지름 좌표를 사용 하는 네 개의 개체를 결합 합니다. 이렇게 하면 기본 채우기 규칙으로 인해 내부가 주황색으로 채워진 네 개의 겹치는 원이 생성 됩니다 `EvenOdd` .

![GeometryGroup](geometry-images/geometrygroup.png "GeometryGroup")

## <a name="clip-with-a-geometry"></a>기 하 도형으로 클리핑

클래스에는 [`VisualElement`](xref:Xamarin.Forms.VisualElement) `Clip` `Geometry` 요소의 콘텐츠의 윤곽선을 정의 하는 형식의 속성이 있습니다. `Clip`속성이 개체로 설정 된 경우 `Geometry` 의 영역 내에 있는 영역만 `Geometry` 표시 됩니다.

다음 예제에서는 `Geometry` 개체를의 클립 영역으로 사용 하는 방법을 보여 줍니다 [`Image`](xref:Xamarin.Forms.Image) .

```xaml
<Image Source="monkeyface.png">
    <Image.Clip>
        <EllipseGeometry RadiusX="100"
                         RadiusY="100"
                         Center="180,180" />
    </Image.Clip>
</Image>
```

이 예제에서는 `EllipseGeometry` `RadiusX` 및 `RadiusY` 값이 100이 고 값이 `Center` (180180) 인이의 속성으로 설정 됩니다 `Clip` [`Image`](xref:Xamarin.Forms.Image) . 타원의 영역 내에 있는 이미지 부분만 표시 됩니다.

![EllipseGeometry를 사용 하 여 이미지 잘라내기](geometries-images/clip-ellipsegeometry.png "EllipseGeometry를 사용 하 여 이미지 잘라내기")

> [!NOTE]
> 간단한 기 하 도형, 경로 기 하 도형 및 복합 기 하 도형을 사용 하 여 개체를 클리핑할 수 있습니다 [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

## <a name="other-features"></a>기타 기능

`GeometryHelper`클래스는 다음과 같은 도우미 메서드를 제공 합니다.

- `FlattenGeometry`는를로 평면화 `Geometry` `PathGeometry` 합니다.
- `FlattenCubicBezier`-입방 형 3 차원 곡선을 컬렉션으로 평면화 `List<Point>` 합니다.
- `FlattenQuadraticBezier`는 정방형 3 차원 곡선을 컬렉션으로 평면화 `List<Point>` 합니다.
- `FlattenArc`-타원형 원호를 컬렉션으로 평면화 `List<Point>` 합니다.

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms셰이프도](index.md)
- [Xamarin.Forms셰이프: 채우기 규칙](fillrules.md)
