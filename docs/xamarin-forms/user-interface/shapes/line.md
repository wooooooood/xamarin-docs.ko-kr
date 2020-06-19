---
title: 'Xamarin.Forms도형: 선'
description: Xamarin.Forms줄 클래스를 사용 하 여 선을 그릴 수 있습니다.
ms.prod: xamarin
ms.assetid: 384F1A72-6D3B-4FD3-BC40-E00A73A463EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c2255db884fbe51e253aaf0f01909e732df94850
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84947370"
---
# <a name="xamarinforms-shapes-line"></a>Xamarin.Forms도형: 선

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Line`클래스는 클래스에서 파생 `Shape` 되며 선을 그리는 데 사용할 수 있습니다. 클래스가 클래스에서 상속 하는 속성에 대 한 자세한 내용은 `Line` `Shape` [ Xamarin.Forms 셰이프](index.md)를 참조 하세요.

`Line`는 다음 속성을 정의합니다.

- `X1`double 형식의은 선 시작점의 x 좌표를 나타냅니다. 이 속성의 기본값은 0.0입니다.
- `X2`double 형식의는 선 끝점의 x 좌표를 나타냅니다. 이 속성의 기본값은 0.0입니다.
- `Y1`double 형식의는 선 시작점의 y 좌표를 나타냅니다. 이 속성의 기본값은 0.0입니다.
- `Y2`double 형식의는 선 끝점의 y 좌표를 나타냅니다. 이 속성의 기본값은 0.0입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

## <a name="create-a-line"></a>선 만들기

다음 XAML 예제에서는 선을 그리는 방법을 보여 줍니다.

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="DarkBlue"
      StrokeThickness="4"
      HeightRequest="120"
      WidthRequest="120" />
```

> [!NOTE]
> `Fill`의 속성을 설정 `Line` 해도 아무 효과가 없습니다.

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
- [Xamarin.Forms셰이프도](index.md)
