---
title: 'Xamarin.Forms도형: 폴리라인'
description: Xamarin.Forms다중선 클래스를 사용 하 여 일련의 연결 된 직선을 그릴 수 있습니다.
ms.prod: xamarin
ms.assetid: 15D02690-AC03-457E-8815-8E4C17E4D642
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5ffbf452816eb9e60d70175a995419a124bcc457
ms.sourcegitcommit: 8f6cc5208f675c8cfb645bd9ffb0fc1f8ea71411
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85326165"
---
# <a name="xamarinforms-shapes-polyline"></a>Xamarin.Forms도형: 폴리라인

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polyline`클래스는 클래스에서 파생 `Shape` 되며 일련의 연결 된 직선을 그리는 데 사용할 수 있습니다. 다중선은 다각형의 마지막 점이 첫 번째 점에 연결 되지 않는다는 점을 제외 하 고 다각형과 비슷합니다. 클래스가 클래스에서 상속 하는 속성에 대 한 자세한 내용은 `Polyline` `Shape` [ Xamarin.Forms 셰이프](index.md)를 참조 하세요.

`Polyline`는 다음 속성을 정의합니다.

- `Points`는 `PointCollection` `Point` 다중선의 꼭 짓 점을 설명 하는 구조체 컬렉션인 형식의입니다.
- `FillRule`는 다중선의 `FillRule` 교차 영역을 결합 하는 방법을 지정 하는 형식의입니다. 이 속성의 기본값은 `FillRule.EvenOdd`입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

`PointsCollection`형식은 `ObservableCollection` 개체의입니다 [`Point`](xref:Xamarin.Forms.Point) . `Point`구조체는 `X` `Y` `double` 2d 공간에서 x 및 y 좌표 쌍을 나타내는 형식의 및 속성을 정의 합니다. 따라서 속성은 `Points` 단일 쉼표 및/또는 하나 이상의 공백으로 구분 되는 다중선 꼭 짓 점 점을 설명 하는 x 좌표 및 y 좌표 쌍의 목록으로 설정 되어야 합니다. 예를 들어 "40, 10 70, 80" 및 "40 10, 70 80"는 모두 유효 합니다.

열거형에 대 한 자세한 내용은 `FillRule` [ Xamarin.Forms 셰이프: 채우기 규칙](fillrules.md)을 참조 하세요.

## <a name="create-a-polyline"></a>다중선 만들기

다중선을 그리려면 개체를 만들고 `Polyline` 해당 `Points` 속성을 도형의 꼭 짓 점으로 설정 합니다. 다중선에 윤곽선을 지정 하려면 해당 속성을로 설정 `Stroke` [`Color`](xref:Xamarin.Forms.Color) 합니다. `StrokeThickness`속성은 다중선 윤곽선의 두께를 지정 합니다.

> [!IMPORTANT]
> `Fill`의 속성을로 설정 하면 `Polyline` [`Color`](xref:Xamarin.Forms.Color) 시작점과 끝점이 교차 하지 않는 경우에도 다중선의 내부 공간이 그려집니다.

다음 XAML 예제에서는 다중선을 그리는 방법을 보여 줍니다.

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red" />
```

이 예제에서는 빨간색 다중선이 그려집니다.

![폴리라인](polyline-images/stroke.png "폴리라인")

다음 XAML 예제에서는 파선 폴리라인를 그리는 방법을 보여 줍니다.

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red"
          StrokeThickness="2"
          StrokeDashArray="1,1"
          StrokeDashOffset="6" />
```

이 예제에서 다중선은 파선입니다.

![파선 다중선](polyline-images/dashed.png "파선 다중선")

파선 다중선을 그리는 방법에 대 한 자세한 내용은 [파선 모양 그리기](index.md#draw-dashed-shapes)를 참조 하세요.

다음 XAML 예제에서는 기본 채우기 규칙을 사용 하는 다중선을 보여 줍니다.

```xaml
<Polyline Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
          Fill="Blue"
          Stroke="Red"
          StrokeThickness="3" />
```

이 예에서 폴리라인의 채우기 동작은 채우기 규칙을 사용 하 여 결정 됩니다 `EvenOdd` .

![EvenOdd 폴리라인](polyline-images/evenodd.png "EvenOdd polyine")

다음 XAML 예제에서는 채우기 규칙을 사용 하는 다중선을 보여 줍니다 `Nonzero` .

```xaml
<Polyline Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
          Fill="Black"
          FillRule="Nonzero"
          Stroke="Yellow"
          StrokeThickness="3" />
```

![0이 아닌 폴리라인](polyline-images/nonzero.png "0이 아닌 폴리라인")

이 예에서 폴리라인의 채우기 동작은 채우기 규칙을 사용 하 여 결정 됩니다 `Nonzero` .

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms셰이프도](index.md)
- [Xamarin.Forms셰이프: 채우기 규칙](fillrules.md)
