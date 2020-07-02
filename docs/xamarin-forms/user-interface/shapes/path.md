---
title: 'Xamarin.Forms셰이프: 경로'
description: Xamarin.Forms경로 클래스를 사용 하 여 곡선 및 복잡 한 도형을 그릴 수 있습니다.
ms.prod: xamarin
ms.assetid: B29486F4-9A5E-4588-ABDF-7EB1E69B9AE6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c0cd0e1939e443bc2c4a1ead8d0a29462ce04a68
ms.sourcegitcommit: 91b4d2f93687fadec5c3f80aadc8f7298d911624
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85795036"
---
# <a name="xamarinforms-shapes-path"></a>Xamarin.Forms셰이프: 경로

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Path`클래스는 클래스에서 파생 `Shape` 되며 곡선 및 복잡 한 도형을 그리는 데 사용할 수 있습니다. 이러한 곡선과 도형은 종종 개체를 사용 하 여 설명 `Geometry` 합니다. 클래스가 클래스에서 상속 하는 속성에 대 한 자세한 내용은 `Path` `Shape` [ Xamarin.Forms 셰이프](index.md)를 참조 하세요.

`Path`는 다음 속성을 정의합니다.

- `Data``Geometry`그릴 모양을 지정 하는 형식의입니다.
- `RenderTransform`는 `Transform` 그리기 전에 경로의 기 하 도형에 적용 되는 변환을 나타내는 형식의입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

변환에 대 한 자세한 내용은 [ Xamarin.Forms 경로 변환](path-transforms.md)을 참조 하세요.

## <a name="create-a-path"></a>경로 만들기

경로를 그리려면 개체를 만들고 `Path` 해당 속성을 설정 `Data` 합니다. 속성을 설정 하는 방법에는 두 가지가 있습니다 `Data` .

- `Data`경로 태그 구문을 사용 하 여 XAML에서의 문자열 값을 설정할 수 있습니다. 이 방법을 사용 하는 경우 `Path.Data` 값은 그래픽의 serialization 형식을 사용 합니다. 일반적으로이 문자열 값을 만든 후에는 직접 편집 하지 않습니다. 대신 디자인 도구를 사용 하 여 데이터를 조작 하 고 속성에서 사용할 수 있는 문자열 조각으로 내보냅니다 `Data` .
- `Data`개체에 속성을 설정할 수 있습니다 `Geometry` . 이 개체는 특정 `Geometry` 개체 이거나 `GeometryGroup` 여러 geometry 개체를 단일 개체로 결합할 수 있는 컨테이너 역할을 하는입니다.

### <a name="create-a-path-with-path-markup-syntax"></a>경로 태그 구문을 사용 하 여 경로 만들기

다음 XAML 예제에서는 경로 태그 구문을 사용 하 여 삼각형을 그리는 방법을 보여 줍니다.

```xaml
<Path Data="M 10,100 L 100,100 100,50Z"
      Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Start" />
```

`Data`문자열은 `M` 경로에 대 한 절대 시작점을 설정 하는로 표시 되는 이동 명령으로 시작 합니다. `L`는 시작점에서 지정 된 끝점으로 직선을 만드는 line 명령입니다. `Z`는 close 명령이 며 현재 지점을 시작 지점에 연결 하는 줄을 만듭니다. 결과는 삼각형입니다.

![경로 삼각형](path-images/triangle.png "경로 삼각형")

> [!NOTE]
> 경로 태그 구문은 XAML 에서만 사용할 수 있습니다.

경로 태그 구문에 대 한 자세한 내용은 [ Xamarin.Forms 경로 태그 구문](path-markup-syntax.md)을 참조 하세요.

### <a name="create-a-path-with-geometry-objects"></a>Geometry 개체를 사용 하 여 경로 만들기

`Geometry`개체의 속성을 설정 하는 데 사용 되는 개체를 사용 하 여 곡선 및 셰이프를 설명할 수 있습니다 `Path` `Data` . 선택할 수 있는 다양 한 `Geometry` 개체가 있습니다. `EllipseGeometry`, `LineGeometry` 및 클래스는 `RectangleGeometry` 상대적으로 간단한 도형을 설명 합니다. 더 복잡 한 도형을 만들거나 곡선을 만들려면를 사용 `PathGeometry` 합니다.

`PathGeometry`개체는 하나 이상의 개체로 구성 됩니다 `PathFigure` . 각 `PathFigure` 개체는 다른 모양을 나타냅니다. 각 `PathFigure` 개체는 각각 `PathSegment` 모양의 연결 부분을 나타내는 하나 이상의 개체로 구성 됩니다. 세그먼트 형식에는 `LineSegment` , `BezierSegment` 및 `ArcSegment` 클래스가 포함 됩니다.

다음 XAML 예제에서는 개체를 사용 하 여 삼각형을 그리는 방법을 보여 줍니다 `PathGeometry` .

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

이 예제에서 삼각형의 시작점은 (10100)입니다. (10100)에서 (100100)로 그리고 (100100)에서 (100, 50)로 선 세그먼트가 그려집니다. 그런 다음 속성이로 설정 되어 있기 때문에 첫 번째 및 마지막 세그먼트가 연결 됩니다 `PathFigure.IsClosed` `true` . 결과는 삼각형입니다.

![경로 삼각형](path-images/triangle.png "경로 삼각형")

기 하 도형에 대 한 자세한 내용은 geometry [ Xamarin.Forms 를 참조 하세요](geometries.md).

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms셰이프도](index.md)
- [Xamarin.Forms형상의](geometries.md)
- [Xamarin.Forms경로 태그 구문](path-markup-syntax.md)
- [Xamarin.Forms경로 변환](path-transforms.md)
