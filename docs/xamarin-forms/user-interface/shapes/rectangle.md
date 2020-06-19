---
title: 'Xamarin.Forms도형: 사각형'
description: Xamarin.Forms사각형 클래스를 사용 하 여 사각형을 그릴 수 있습니다.
ms.prod: xamarin
ms.assetid: 2DD663D3-DAEC-495C-AB6D-8A143FC97637
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a0d528948ae8a57fc7c702d8a4ce3d3ec2eda15a
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990824"
---
# <a name="xamarinforms-shapes-rectangle"></a>Xamarin.Forms도형: 사각형

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Rectangle`클래스는 클래스에서 파생 `Shape` 되며 사각형을 그리는 데 사용할 수 있습니다. 클래스가 클래스에서 상속 하는 속성에 대 한 자세한 내용은 `Rectangle` `Shape` [ Xamarin.Forms 셰이프](index.md)를 참조 하세요.

`Rectangle`는 다음 속성을 정의합니다.

- `RadiusX`, 형식의, `double` 사각형의 모퉁이를 둥글게 하는 데 사용 되는 x 축 반지름입니다. 이 속성의 기본값은 0.0입니다.
- `RadiusY`, 형식의, `double` 사각형의 모퉁이를 둥글게 하는 데 사용 되는 y-축 반경입니다. 이 속성의 기본값은 0.0입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

`Rectangle`클래스는 `Aspect` 클래스에서 상속 된 속성을로 설정 합니다 `Shape` `Stretch.Fill` .

## <a name="create-a-rectangle"></a>사각형 만들기

다음 XAML 예제에서는 모퉁이가 둥근 채워진 사각형을 그리는 방법을 보여 줍니다.

```xaml
<Rectangle Fill="DarkBlue"
           Stroke="Red"
           StrokeThickness="4"
           RadiusX="12"
           RadiusY="24"           
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms셰이프도](index.md)
