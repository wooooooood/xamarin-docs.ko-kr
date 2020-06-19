---
title: Xamarin.Forms셰이프도
description: Xamarin.Forms도형은 화면에 모양을 그릴 수 있는 뷰 형식입니다.
ms.prod: xamarin
ms.assetid: 4E749FE8-852C-46DA-BB1E-652936106357
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a2ee7e6afc46c9d7b6e2f3d7710bdf758bafe1b9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84947381"
---
# <a name="xamarinforms-shapes"></a>Xamarin.Forms셰이프도

![](~/media/shared/preview.png "This API is currently pre-release")

는 `Shape` [`View`](xref:Xamarin.Forms.View) 화면에 모양을 그릴 수 있는의 형식입니다. `Shape``Shape`클래스는 클래스에서 파생 되므로 레이아웃 클래스 및 대부분의 컨트롤 내에서 개체를 사용할 수 있습니다 `View` .

Xamarin.Forms셰이프는 `Xamarin.Forms.Shapes` iOS, Android, macOS, 유니버설 Windows 플랫폼 (UWP) 및 Windows Presentation Foundation (WPF)의 네임 스페이스에서 사용할 수 있습니다.

> [!IMPORTANT]
> Xamarin.Forms도형은 현재 실험적 이며 플래그를 설정 하는 방법 으로만 사용할 수 있습니다 `Shapes_Experimental` . 자세한 내용은 [실험적 플래그](~/xamarin-forms/internals/experimental-flags.md)를 참조 하세요.

`Shape`는 다음 속성을 정의합니다.

- `Aspect`형식의은 `Stretch` 셰이프가 할당 된 공간을 채우는 방법을 설명 합니다. 이 속성의 기본값은 `Stretch.None`입니다.
- `Fill`형식의는 `Color` 도형의 내부를 그리는 데 사용 되는 색을 나타냅니다.
- `Stroke`형식의는 `Color` 도형의 윤곽선을 그리는 데 사용 되는 색을 나타냅니다.
- `StrokeDashArray`는 `DoubleCollection` `double` 도형의 윤곽선을 지정 하는 데 사용 되는 대시와 간격의 패턴을 나타내는 값의 컬렉션을 나타내는 형식의입니다.
- `StrokeDashOffset`형식의는 대시 `double` 패턴 내에서 대시가 시작 되는 거리를 지정 합니다. 이 속성의 기본값은 0.0입니다.
- `StrokeLineCap`형식의는 `PenLineCap` 선 또는 세그먼트의 시작과 끝에 있는 셰이프를 설명 합니다. 이 속성의 기본값은 `PenLineCap.Flat`입니다.
- `StrokeLineJoin`형식의는 `PenLineJoin` 도형의 꼭 짓 점에 사용 되는 조인 유형을 지정 합니다. 이 속성의 기본값은 `PenLineJoin.Miter`입니다.
- `StrokeThickness`형식의는 `double` 셰이프 윤곽선의 너비를 나타냅니다. 이 속성의 기본값은 1.0입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

Xamarin.Forms클래스에서 파생 되는 개체의 수를 정의 `Shape` 합니다. `Ellipse`, `Line`, `Path`, `Polygon`, `Polyline`, `Rectangle`을 포함합니다.

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
