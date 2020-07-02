---
title: 'Xamarin.Forms셰이프: 채우기 규칙'
description: Xamarin.Forms셰이프 채우기 규칙은 셰이프의 채우기 영역에 점이 있는지 여부를 결정 합니다.
ms.prod: xamarin
ms.assetid: 5CABB22B-C6BE-43D1-91D9-6E90A4BD5622
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 92fcb86f9acac159cc79cae8e71b180fe229b7a6
ms.sourcegitcommit: 91b4d2f93687fadec5c3f80aadc8f7298d911624
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85794992"
---
# <a name="xamarinforms-shapes-fill-rules"></a>Xamarin.Forms셰이프: 채우기 규칙

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

여러 Xamarin.Forms 도형 클래스에는 `FillRule` 형식의 속성이 있습니다 `FillRule` . 여기에 포함 됩니다 `Polygon`하십시오 `Polyline`, 및 `GeometryGroup`합니다.

`FillRule`열거형은 및 멤버를 정의 합니다 `EvenOdd` `Nonzero` . 각 멤버는 셰이프의 채우기 영역에 점이 있는지 여부를 결정 하는 다른 규칙을 나타냅니다.

> [!IMPORTANT]
> 모든 도형은 채우기 규칙의 용도로 닫혀 있는 것으로 간주 됩니다.

## <a name="evenodd"></a>EvenOdd

`EvenOdd`채우기 규칙은 점에서 모든 방향으로 무한대에서 무한대로 광선을 그리고 광선이 교차 하는 모양 내 세그먼트의 수를 계산 합니다. 이 숫자가 홀수 이면 점이 내부에 있습니다. 이 숫자가 짝수 이면 지점은 바깥쪽입니다.

다음 XAML 예제에서는 기본적으로를 사용 하 여 복합 도형을 만들고 렌더링 합니다 `FillRule` `EvenOdd` .

```xaml
<Path Stroke="Black"
      Fill="#CCCCFF"
      Aspect="Uniform"
      HorizontalOptions="Start">
    <Path.Data>
        <!-- FillRule doesn't need to be set, because EvenOdd is the default. -->
        <GeometryGroup>
            <EllipseGeometry RadiusX="50"
                             RadiusY="50"
                             Center="75,75" />
            <EllipseGeometry RadiusX="70"
                             RadiusY="70"
                             Center="75,75" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="75,75" />
            <EllipseGeometry RadiusX="120"
                             RadiusY="120"
                             Center="75,75" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

이 예제에서는 일련의 동심 링으로 구성 된 복합 모양이 표시 됩니다.

![EvenOdd 채우기 규칙을 사용 하는 복합 도형](fillrule-images/evenodd.png "EvenOdd 채우기 규칙을 사용 하는 복합 도형")

복합 모양에서 중심과 세 번째 링이 채워지지 않습니다. 이러한 두 링의 모든 지점에서 그린 광선이 짝수 개수의 세그먼트를 통과 하기 때문입니다.

![EvenOdd 채우기 규칙을 사용 하 여 주석이 달린 복합 모양](fillrule-images/evenodd-annotated.png "EvenOdd 채우기 규칙을 사용 하 여 주석이 달린 복합 모양")

위의 그림에서 빨간색 원은 점수를 나타내고 선은 임의의 광선을 나타냅니다. 상한의 경우 각각의 임의 광선은 짝수 개의 선 세그먼트를 통과 합니다. 따라서 지점의 링은 채워지지 않습니다. 낮은 지점의 경우 각각의 임의 광선은 홀수 개의 선 세그먼트를 통과 합니다. 따라서 지점에 있는 링이 채워집니다.

## <a name="nonzero"></a>0이 아닌

`Nonzero`채우기 규칙은 점에서 모든 방향으로 무한대에서 무한대로 광선을 그린 다음 도형의 세그먼트가 광선과 교차 하는 위치를 검사 합니다. 0부터 시작 하 여 세그먼트가 왼쪽에서 오른쪽으로 광선과 교차할 때마다 카운트가 증가 하 고 세그먼트가 오른쪽에서 왼쪽으로 광선과 교차할 때마다 감소 합니다. 교차를 계산한 후 결과가 0 이면 점이 다각형 외부에 있습니다. 그렇지 않으면 내부에 있습니다.

다음 XAML 예제에서는를로 설정 하 여 복합 도형을 만들고 렌더링 합니다 `FillRule` `Nonzero` .

```xaml
<Path Stroke="Black"
      Fill="#CCCCFF"
      Aspect="Uniform"
      HorizontalOptions="Start">
    <Path.Data>
        <GeometryGroup FillRule="Nonzero">
            <EllipseGeometry RadiusX="50"
                             RadiusY="50"
                             Center="75,75" />
            <EllipseGeometry RadiusX="70"
                             RadiusY="70"
                             Center="75,75" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="75,75" />
            <EllipseGeometry RadiusX="120"
                             RadiusY="120"
                             Center="75,75" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

이 예제에서는 일련의 동심 링으로 구성 된 복합 모양이 표시 됩니다.

![0이 아닌 채우기 규칙을 사용 하는 복합 도형](fillrule-images/nonzero.png "0이 아닌 채우기 규칙을 사용 하는 복합 도형")

복합 모양에서 모든 링이 채워져 있는지 확인 합니다. 모든 세그먼트가 동일한 방향으로 실행 되므로 모든 점에서 그린 광선이 하나 이상의 세그먼트를 교차 하 고 교차의 합계가 0이 되지 않기 때문입니다.

![0이 아닌 채우기 규칙을 사용 하 여 주석이 달린 복합 모양](fillrule-images/nonzero-annotated.png "0이 아닌 채우기 규칙을 사용 하 여 주석이 달린 복합 모양")

빨간색 화살표 위의 이미지에서는 세그먼트가 그려지는 방향을 나타내고, 검은색 화살표는 가장 안쪽 링의 지점에서 실행 되는 임의의 광선을 나타냅니다. 광선이 교차하는 각 세그먼트에 대해 0 값부터 시작할 경우 세그먼트가 왼쪽에서 오른쪽으로 광선과 교차하므로 1이 추가됩니다.

채우기 규칙의 동작을 더 잘 보여 주기 위해 세그먼트가 다른 방향으로 실행 되는 더 복잡 한 모양이 필요 `Nonzero` 합니다. 다음 XAML 예제에서는가 아닌를 사용 하 여 만든 것을 제외 하 고 앞의 예제와 비슷한 셰이프를 만듭니다 `PathGeometry` `EllipseGeometry` .

```xaml
<Path Stroke="Black"
      Fill="#CCCCFF">
     <Path.Data>
         <GeometryGroup FillRule="Nonzero">
             <PathGeometry>
                 <PathGeometry.Figures>
                     <!-- Inner ring -->
                     <PathFigure StartPoint="120,120">
                         <PathFigure.Segments>
                             <PathSegmentCollection>
                                 <ArcSegment Size="50,50"
                                             IsLargeArc="True"
                                             SweepDirection="CounterClockwise"
                                             Point="140,120" />
                             </PathSegmentCollection>
                         </PathFigure.Segments>
                     </PathFigure>

                     <!-- Second ring -->
                     <PathFigure StartPoint="120,100">
                         <PathFigure.Segments>
                             <PathSegmentCollection>
                                 <ArcSegment Size="70,70"
                                             IsLargeArc="True"
                                             SweepDirection="CounterClockwise"
                                             Point="140,100" />
                             </PathSegmentCollection>
                         </PathFigure.Segments>
                     </PathFigure>

                     <!-- Third ring  -->
                         <PathFigure StartPoint="120,70">
                         <PathFigure.Segments>
                             <PathSegmentCollection>
                                 <ArcSegment Size="100,100"
                                             IsLargeArc="True"
                                             SweepDirection="CounterClockwise"
                                             Point="140,70" />
                             </PathSegmentCollection>
                         </PathFigure.Segments>
                     </PathFigure>

                     <!-- Outer ring -->
                     <PathFigure StartPoint="120,300">
                         <PathFigure.Segments>
                             <ArcSegment Size="130,130"
                                         IsLargeArc="True"
                                         SweepDirection="Clockwise"
                                         Point="140,300" />
                         </PathFigure.Segments>
                     </PathFigure>
                 </PathGeometry.Figures>
             </PathGeometry>
         </GeometryGroup>
     </Path.Data>
 </Path>
```

이 예제에서는 닫혀 있지 않은 일련의 원호 세그먼트가 그려집니다.

![0이 아닌 채우기 규칙을 사용 하는 복합 도형](fillrule-images/nonzero-gaps.png "0이 아닌 채우기 규칙을 사용 하는 복합 도형")

위의 이미지에서 가운데의 세 번째 호는 채워지지 않습니다. 이는 지정 된 광선에서 해당 경로에 있는 세그먼트를 교차 하는 값의 합계가 0 이기 때문입니다.

![0이 아닌 채우기 규칙을 사용 하 여 주석이 달린 복합 모양](fillrule-images/nonzero-gaps-annotated.png "0이 아닌 채우기 규칙을 사용 하 여 주석이 달린 복합 모양")

위의 그림에서 빨간색 원은 포인트를 나타내고, 검정 선은 채워지지 않은 영역에 있는 지점에서 밖으로 이동 하는 임의의 광선을 나타내고, 빨간색 화살표는 세그먼트가 그려지는 방향을 나타냅니다. 볼 수 있듯이 세그먼트를 교차 하는 광선의 값 합계는 0입니다.

- 대각선 방향으로 이동 하는 임의의 광선은 다른 방향으로 실행 되는 두 세그먼트를 교차 합니다. 따라서 세그먼트가 서로를 취소 하 여 0 값을 제공 합니다.
- 대각선 왼쪽으로 이동 하는 임의의 광선은 총 6 개의 세그먼트를 교차 합니다. 그러나 교차는 0이 최종 합계를 갖도록 서로를 취소 합니다.

0의 합계는 링이 채워지지 않습니다.

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms셰이프도](index.md)
