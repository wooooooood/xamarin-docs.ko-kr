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
ms.openlocfilehash: de1848f7439e62e84daea56defa3f4583f5758ea
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84947323"
---
# <a name="xamarinforms-shapes-polygon"></a>Xamarin.Forms도형: 다각형

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polygon`클래스는 클래스에서 파생 `Shape` 되며 닫힌 셰이프를 구성 하는 일련의 선으로 연결 된 다각형을 그리는 데 사용할 수 있습니다. 클래스가 클래스에서 상속 하는 속성에 대 한 자세한 내용은 `Polygon` `Shape` [ Xamarin.Forms 셰이프](index.md)를 참조 하세요.

`Polygon`는 다음 속성을 정의합니다.

- `Points`는 `PointCollection` `Point` 다각형의 꼭 짓 점을 설명 하는 구조체 컬렉션인 형식의입니다.
- `FillRule`는 `FillRule` 도형의 내부 채우기가 결정 되는 방법을 지정 하는 형식의입니다. 이 속성의 기본값은 `FillRule.EvenOdd`입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

## <a name="create-a-polygon"></a>다각형 만들기

다음 XAML 예제에서는 다각형을 그리는 방법을 보여 줍니다.

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5"
         HeightRequest="100"
         WidthRequest="200" />
```

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
- [Xamarin.Forms셰이프도](index.md)
