---
title: 'Xamarin.Forms도형: 다각형'
description: Xamarin.FormsPolygon 클래스를 사용 하 여 닫힌 셰이프를 형성 하는 일련의 선으로 연결 된 다각형을 그릴 수 있습니다.
ms.prod: xamarin
ms.assetid: D6539F60-A5AC-46EF-86EB-E9F508EB1FA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e6f8ad3afdcdb9137869dc57078ac94895f4183c
ms.sourcegitcommit: ef3d4a70e70927c4f231b763842c5355f1571d15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243818"
---
# <a name="xamarinforms-shapes-polygon"></a>Xamarin.Forms도형: 다각형

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polygon`클래스는 클래스에서 파생 `Shape` 되며 닫힌 셰이프를 구성 하는 일련의 선으로 연결 된 다각형을 그리는 데 사용할 수 있습니다. 클래스가 클래스에서 상속 하는 속성에 대 한 자세한 내용은 `Polygon` `Shape` [ Xamarin.Forms 셰이프](index.md)를 참조 하세요.

`Polygon`는 다음 속성을 정의합니다.

- `Points`는 `PointCollection` [`Point`](xref:Xamarin.Forms.Point) 다각형의 꼭 짓 점을 설명 하는 구조체 컬렉션인 형식의입니다.
- `FillRule`는 `FillRule` 도형의 내부 채우기가 결정 되는 방법을 지정 하는 형식의입니다. 이 속성의 기본값은 `FillRule.EvenOdd`입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

`PointsCollection`형식은 `ObservableCollection` 개체의입니다 [`Point`](xref:Xamarin.Forms.Point) . `Point`구조체는 `X` `Y` `double` 2d 공간에서 x 및 y 좌표 쌍을 나타내는 형식의 및 속성을 정의 합니다. 따라서 `Points` 단일 쉼표 및/또는 하나 이상의 공백으로 구분 된 다각형 꼭 짓 점 점을 설명 하는 x 좌표 및 y 좌표 쌍의 목록으로 속성을 설정 해야 합니다. 예를 들어 "40, 10 70, 80" 및 "40 10, 70 80"는 모두 유효 합니다.

`FillRule` 열거형은 다음 멤버를 정의합니다.

- `EvenOdd`점이 다각형의 채우기 영역에 있는지 여부를 결정 하는 규칙을 나타냅니다. 지점에서 무한대로 임의의 방향으로 광선을 그리고 광선이 교차 하는 모양 내 세그먼트의 수를 계산 합니다. 이 숫자가 홀수 이면 점이 내부에 있습니다. 이 숫자가 짝수 이면 지점은 바깥쪽입니다.
- `Nonzero`점이 다각형의 채우기 영역에 있는지 여부를 결정 하는 규칙을 나타냅니다. 지점에서 점에서 모든 방향으로 무한대로 광선을 그려 하 고 도형의 세그먼트가 광선과 교차 하는 위치를 확인 합니다. 0부터 시작 하 여 세그먼트가 왼쪽에서 오른쪽으로 광선과 교차할 때마다 카운트가 증가 하 고 세그먼트가 오른쪽에서 왼쪽으로 광선과 교차할 때마다 감소 합니다. 교차를 계산한 후 결과가 0 이면 점이 다각형 외부에 있습니다. 그렇지 않으면 내부에 있습니다.

## <a name="create-a-polygon"></a>다각형 만들기

다각형을 그리려면 개체를 만들고 `Polygon` 해당 `Points` 속성을 도형의 꼭 짓 점으로 설정 합니다. 첫 번째 및 마지막 요소를 연결 하는 선이 자동으로 그려집니다. 다각형 내부를 그리려면 해당 속성을로 설정 `Fill` [`Color`](xref:Xamarin.Forms.Color) 합니다. 다각형에 윤곽선을 지정 하려면 해당 속성을로 설정 `Stroke` [`Color`](xref:Xamarin.Forms.Color) 합니다. `StrokeThickness`속성은 다각형 윤곽선의 두께를 지정 합니다.

다음 XAML 예제에서는 채워진 다각형을 그리는 방법을 보여 줍니다.

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5" />
```

이 예제에서는 삼각형을 나타내는 채워진 다각형이 그려집니다.

![채워진 다각형](polygon-images/filled.png "채워진 다각형")

다음 XAML 예제에서는 파선 다각형을 그리는 방법을 보여 줍니다.

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5"
         StrokeDashArray="1,1"
         StrokeDashOffset="6" />
```

이 예제에서 polygon 윤곽선은 파선입니다.

![파선 다각형](polygon-images/dashed.png "파선 다각형")

파선 다각형을 그리는 방법에 대 한 자세한 내용은 [파선 도형 그리기](index.md#draw-dashed-shapes)를 참조 하세요.

다음 XAML 예제에서는 기본 채우기 규칙을 사용 하는 다각형을 보여 줍니다.

```xaml
<Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
         Fill="Blue"
         Stroke="Red"
         StrokeThickness="3" />
```

이 예제에서 각 polygon의 채우기 동작은 채우기 규칙을 사용 하 여 결정 됩니다 `EvenOdd` .

![EvenOdd polygon](polygon-images/evenodd.png "EvenOdd polygon")

다음 XAML 예제에서는 채우기 규칙을 사용 하는 다각형을 보여 줍니다 `Nonzero` .

```xaml
<Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
         Fill="Black"
         FillRule="Nonzero"
         Stroke="Yellow"
         StrokeThickness="3" />
```

![0이 아닌 다각형](polygon-images/nonzero.png "0이 아닌 다각형")

이 예제에서 각 polygon의 채우기 동작은 채우기 규칙을 사용 하 여 결정 됩니다 `Nonzero` .

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms셰이프도](index.md)
