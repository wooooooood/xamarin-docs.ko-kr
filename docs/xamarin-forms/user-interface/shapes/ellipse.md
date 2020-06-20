---
title: 'Xamarin.Forms도형: 타원'
description: Xamarin.Forms타원 클래스를 사용 하 여 타원과 원을 그릴 수 있습니다.
ms.prod: xamarin
ms.assetid: 5BF81E25-12E5-49F0-A40C-0CF4C5D63B9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 142deccbcd29548e2d72b7144a01322f9909d98f
ms.sourcegitcommit: d86b7a18cf8b1ef28cd0fe1d311f1c58a65101a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85101285"
---
# <a name="xamarinforms-shapes-ellipse"></a>Xamarin.Forms도형: 타원

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

`Ellipse`클래스는 클래스에서 파생 `Shape` 되며 타원과 원을 그리는 데 사용할 수 있습니다. 클래스가 클래스에서 상속 하는 속성에 대 한 자세한 내용은 `Ellipse` `Shape` [ Xamarin.Forms 셰이프](index.md)를 참조 하세요.

`Ellipse`클래스는 `Aspect` 클래스에서 상속 된 속성을로 설정 합니다 `Shape` `Stretch.Fill` .

## <a name="create-an-ellipse"></a>타원 만들기

다음 XAML 예제에서는 채워진 타원을 그리는 방법을 보여 줍니다.

```xaml
<Ellipse Fill="Red"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

다음 XAML 예제에서는 원을 그리는 방법을 보여 줍니다.

```xaml
<Ellipse Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="150"
         HorizontalOptions="Start" />
```

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.Forms셰이프도](index.md)
