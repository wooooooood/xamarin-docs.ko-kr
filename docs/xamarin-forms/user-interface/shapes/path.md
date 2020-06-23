---
title: 'Xamarin.Forms셰이프: 경로'
description: Xamarin.Forms경로 클래스를 사용 하 여 곡선 및 복잡 한 도형을 그릴 수 있습니다.
ms.prod: xamarin
ms.assetid: B29486F4-9A5E-4588-ABDF-7EB1E69B9AE6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 384dcef3c2b480166f17e91d547f8cda39c83dc0
ms.sourcegitcommit: 7fc658bbdcb8130cd9d611e55e79a1830fc5d5a2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85132972"
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

다음 XAML 예제에서는 경로 태그 구문 이라고 하는 특수 한 축약 구문을 사용 하 여 다각형을 그리는 방법을 보여 줍니다.

```xaml
<Path Data="M 10,50 L 200,70"
      Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100" />
```

`Data`문자열은 `M` 경로에 대 한 시작점을 설정 하는로 표시 되는 "moveto" 명령으로 시작 합니다. `L`는 시작점에서 지정 된 끝점으로 직선을 만드는 line 명령입니다.

> [!NOTE]
> 경로 태그 구문은 XAML 에서만 사용할 수 있습니다.

경로 태그 구문에 대 한 자세한 내용은 [ Xamarin.Forms 경로 태그 구문](path-markup-syntax.md)을 참조 하세요.

## <a name="path-geometry"></a>경로 기 하 도형

`Geometry`개체의 속성을 설정 하는 데 사용 되는 개체를 사용 하 여 곡선 및 셰이프를 설명할 수 있습니다 `Path` `Data` .

선택할 수 있는 다양 한 `Geometry` 개체가 있습니다. `EllipseGeometry`, `LineGeometry` 및 클래스는 `RectangleGeometry` 상대적으로 간단한 도형을 설명 합니다. 더 복잡 한 도형을 만들거나 곡선을 만들려면를 사용 `PathGeometry` 합니다.

`PathGeometry`개체는 하나 이상의 개체로 구성 됩니다 `PathFigure` . 각 `PathFigure` 개체는 다른 모양을 나타냅니다. 각 `PathFigure` 개체는 각각 `PathSegment` 모양의 연결 부분을 나타내는 하나 이상의 개체로 구성 됩니다. 세그먼트 형식에는 `LineSegment` , 및가 `BezierSegment` `ArcSegment` 포함 됩니다.

다음 예제에서는를 `Path` 사용 하 여 삼각형을 그립니다.

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100">
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

기 하 도형에 대 한 자세한 내용은 geometry [ Xamarin.Forms 를 참조 하세요](geometries.md).

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms셰이프도](index.md)
- [Xamarin.Forms형상의](geometries.md)
- [Xamarin.Forms경로 태그 구문](path-markup-syntax.md)
- [Xamarin.Forms경로 변환](path-transforms.md)
