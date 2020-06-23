---
title: 'Xamarin.Forms도형: 타원'
description: Xamarin.Forms타원 클래스를 사용 하 여 타원과 원을 그릴 수 있습니다.
ms.prod: xamarin
ms.assetid: 5BF81E25-12E5-49F0-A40C-0CF4C5D63B9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 725f892a667b4e89d55266abcf69e5394b09b6a5
ms.sourcegitcommit: ef3d4a70e70927c4f231b763842c5355f1571d15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243835"
---
# <a name="xamarinforms-shapes-ellipse"></a>Xamarin.Forms도형: 타원

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Ellipse`클래스는 클래스에서 파생 `Shape` 되며 타원과 원을 그리는 데 사용할 수 있습니다. 클래스가 클래스에서 상속 하는 속성에 대 한 자세한 내용은 `Ellipse` `Shape` [ Xamarin.Forms 셰이프](index.md)를 참조 하세요.

`Ellipse`클래스는 `Aspect` 클래스에서 상속 된 속성을로 설정 합니다 `Shape` `Stretch.Fill` . 속성에 대 한 자세한 내용은 `Aspect` [늘이기 셰이프](index.md#stretch-shapes)를 참조 하세요.

## <a name="create-an-ellipse"></a>타원 만들기

타원을 그리려면 개체를 만들고 `Ellipse` 해당 및 속성을 설정 `WidthRequest` `HeightRequest` 합니다. 타원 안쪽에를 그리려면 해당 속성을로 설정 `Fill` [`Color`](xref:Xamarin.Forms.Color) 합니다. 타원에 윤곽선을 지정 하려면 해당 속성을로 설정 `Stroke` [`Color`](xref:Xamarin.Forms.Color) 합니다. `StrokeThickness`속성은 타원 윤곽선의 두께를 지정 합니다.

원을 그리려면 `WidthRequest` `HeightRequest` 개체의 및 속성을 동일 하 게 설정 `Ellipse` 합니다.

다음 XAML 예제에서는 채워진 타원을 그리는 방법을 보여 줍니다.

```xaml
<Ellipse Fill="Red"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

이 예제에서는 크기가 150x50 (장치 독립적 단위) 인 빨강으로 채워진 타원이 그려집니다.

![채워진 타원](ellipse-images/filled.png "채워진 타원")

다음 XAML 예제에서는 원을 그리는 방법을 보여 줍니다.

```xaml
<Ellipse Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="150"
         HorizontalOptions="Start" />
```

이 예제에서는 크기가 150x150 (장치 독립적 단위) 인 빨간색 원이 그려집니다.

![원으로](ellipse-images/circle.png "Circle")

파선 타원을 그리는 방법에 대 한 자세한 내용은 [파선 도형 그리기](index.md#draw-dashed-shapes)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms셰이프도](index.md)
