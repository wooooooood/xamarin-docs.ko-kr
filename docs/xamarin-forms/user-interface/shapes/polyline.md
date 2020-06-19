---
title: 'Xamarin.Forms도형: 폴리라인'
description: Xamarin.Forms다중선 클래스를 사용 하 여 일련의 연결 된 직선을 그릴 수 있습니다.
ms.prod: xamarin
ms.assetid: 15D02690-AC03-457E-8815-8E4C17E4D642
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a5c7dc7af657dbb988ca51eb82cf60ca8b033cbd
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990842"
---
# <a name="xamarinforms-shapes-polyline"></a>Xamarin.Forms도형: 폴리라인

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polyline`클래스는 클래스에서 파생 `Shape` 되며 일련의 연결 된 직선을 그리는 데 사용할 수 있습니다. 클래스가 클래스에서 상속 하는 속성에 대 한 자세한 내용은 `Polyline` `Shape` [ Xamarin.Forms 셰이프](index.md)를 참조 하세요.

`Polyline`는 다음 속성을 정의합니다.

- `Points`는 `PointCollection` `Point` 다중선의 꼭 짓 점을 설명 하는 구조체 컬렉션인 형식의입니다.
- `FillRule`는 다중선의 `FillRule` 교차 영역을 결합 하는 방법을 지정 하는 형식의입니다. 이 속성의 기본값은 `FillRule.EvenOdd`입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

## <a name="create-a-polyline"></a>다중선 만들기

다음 XAML 예제에서는 다각형을 그리는 방법을 보여 줍니다.

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red"
          HeightRequest="100"
          WidthRequest="500" />
```

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms셰이프도](index.md)
