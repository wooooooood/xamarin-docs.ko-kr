---
title: 'Xamarin.Forms도형: 사각형'
description: Xamarin.Forms사각형 클래스를 사용 하 여 사각형을 그릴 수 있습니다.
ms.prod: xamarin
ms.assetid: 2DD663D3-DAEC-495C-AB6D-8A143FC97637
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1fd985aa2997be2b35fe3b22606b891aa0b66cf3
ms.sourcegitcommit: ef3d4a70e70927c4f231b763842c5355f1571d15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243739"
---
# <a name="xamarinforms-shapes-rectangle"></a>Xamarin.Forms도형: 사각형

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Rectangle`클래스는 클래스에서 파생 `Shape` 되며 사각형 및 사각형을 그리는 데 사용할 수 있습니다. 클래스가 클래스에서 상속 하는 속성에 대 한 자세한 내용은 `Rectangle` `Shape` [ Xamarin.Forms 셰이프](index.md)를 참조 하세요.

`Rectangle`는 다음 속성을 정의합니다.

- `RadiusX`, 형식의, `double` 사각형의 모퉁이를 둥글게 하는 데 사용 되는 x 축 반지름입니다. 이 속성의 기본값은 0.0입니다.
- `RadiusY`, 형식의, `double` 사각형의 모퉁이를 둥글게 하는 데 사용 되는 y-축 반경입니다. 이 속성의 기본값은 0.0입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

`Rectangle`클래스는 `Aspect` 클래스에서 상속 된 속성을로 설정 합니다 `Shape` `Stretch.Fill` . 속성에 대 한 자세한 내용은 `Aspect` [늘이기 셰이프](index.md#stretch-shapes)를 참조 하세요.

## <a name="create-a-rectangle"></a>사각형 만들기

사각형을 그리려면 개체를 만들고 `Rectangle` 해당 및 속성을 설정 `WidthRequest` `HeightRequest` 합니다. 사각형 안에를 그리려면 해당 속성을로 설정 `Fill` [`Color`](xref:Xamarin.Forms.Color) 합니다. 사각형에 윤곽선을 지정 하려면 해당 속성을로 설정 `Stroke` [`Color`](xref:Xamarin.Forms.Color) 합니다. `StrokeThickness`속성은 사각형 윤곽선의 두께를 지정 합니다.

사각형의 모퉁이를 둥글게 지정 하려면 해당 `RadiusX` 및 속성을 설정 `RadiusY` 합니다. 이러한 속성은 사각형의 모퉁이를 둥글게 하는 데 사용 되는 x 축과 y 축 반경을 설정 합니다.

사각형을 그리려면 `WidthRequest` 개체의 및 속성을 동일 하 게 설정 `HeightRequest` `Rectangle` 합니다.

다음 XAML 예제에서는 채워진 사각형을 그리는 방법을 보여 줍니다.

```xaml
<Rectangle Fill="Red"
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

이 예제에서는 크기가 150x50 (장치 독립적 단위) 인 빨강 채워진 사각형을 그립니다.

![채워진 사각형](rectangle-images/filled.png "채워진 사각형")

다음 XAML 예제에서는 모퉁이가 둥근 채워진 사각형을 그리는 방법을 보여 줍니다.

```xaml
<Rectangle Fill="Blue"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10"
           WidthRequest="200"
           HeightRequest="100"
           HorizontalOptions="Start" />
```

이 예제에서는 모퉁이가 둥근 모퉁이가 칠해진 사각형을 그립니다.

![모퉁이가 둥근 사각형](rectangle-images/rounded.png "모퉁이가 둥근 사각형")

파선 사각형을 그리는 방법에 대 한 자세한 내용은 [파선 도형 그리기](index.md#draw-dashed-shapes)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms셰이프도](index.md)
