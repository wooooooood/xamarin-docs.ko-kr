---
title: SkiaSharp 변환
description: SkiaSharp 그래픽을 표시 하기 위한 변환에 대 한 자세한 내용은
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 668488ab7efae66f1777e9ae6ded1f725833fe16
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2018
---
# <a name="skiasharp-transforms"></a>SkiaSharp 변환

_SkiaSharp 그래픽을 표시 하기 위한 변환에 대 한 자세한 내용은_

SkiaSharp 지원의 메서드로 구현 되는 전통적인 그래픽 변형을 [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/) 개체입니다. 변환 좌표 및에서 지정 하는 크기를 변경 하는 수학적으로 `SKCanvas` 그리기 기능 그래픽 개체 렌더링 됩니다. 변환은 애니메이션 또는 반복적인 그래픽 그리기에 대 한 편리한 경우가 많습니다. 일부 기법은 &mdash; 비트맵 또는 텍스트를 회전 등 &mdash; 변환 사용 하지 않고 가능 하지 않습니다.

SkiaSharp 변환 작업을 지원 합니다.

- *번역* 다른 한 위치에서 shift 좌표로
- *배율* 좌표 및 크기를 늘리거나
- *회전* 점 기준 좌표가 회전 하려면
- *기울이기* 이동할 가로 또는 세로로 조정 직사각형 평행 사변형 되도록

이 라고 *유사* 변환 합니다. 3x3 유사 변환 평행선 항상 유지 및 무한 되도록 좌표 또는 크기를 일으키지 않습니다. 사각형이 평행 사변형 이외의 값으로 변환 되지 않습니다 및 원이 되는 타원 이외의 값으로 변환 되지 않습니다.

SkiaSharp에서는 유사 형식이 아닌 변형 (호출 또한 *프로젝션* 또는 *관점* 변형) 표준 3-3 변환 매트릭스에 따라 합니다. 유사 형식이 아닌 변형 사각형을 모든 볼록 사각형 (네 면 도형 모든 내부 각도가 보다 작은 180도)로 변환 해야 할 수 있습니다. 3D 효과 대 한 중요 한 되어도 없지만 좌표 또는 크기를 infinite, 있을 비 3x3 유사 변환 될 수 있습니다.

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>SkiaSharp 및 Xamarin.Forms 변환 사이의 차이점

Xamarin.Forms에는 또한 SkiaSharp와 유사한 변환을 지원 합니다. Xamarin.Forms는 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) 다음 변환 속성을 정의 하는 클래스:

- `TranslationX` 및 `TranslationY`
- `Scale`
- `Rotation`, `RotationX` 및 `RotationY`

`RotationX` 및 `RotationY` 속성은 하는 의사 3D 효과 만들 관점 변환 합니다.

몇 가지 중요 한 차이점 SkiaSharp 변환 및 Xamarin.Forms 변환과 있습니다.

첫 번째 점은 SkiaSharp 변환은 전체에 적용 `SKCanvas` 작업자에 게 Xamarin.Forms 변환을 적용 하는 동안 개체 `VisualElement` 파생 항목입니다. (Xamarin.Forms 변환을 적용할 수는 `SKCanvasView` 때문에 개체 자체 `SKCanvasView` 에서 파생 `VisualElement`, 내에 있지만 `SKCanvasView`, SkiaSkarp 변환을 적용 합니다.)

SkiaSharp 변환은의 왼쪽 위 모퉁이 기준으로 `SKCanvas` Xamarin.Forms 변환은의 왼쪽 위 모퉁이 기준으로 하는 동안는 `VisualElement` 적용 됩니다. 크기 조정을 적용할 때이 차이점은 중요 하 고 특정 시점을 기준으로 해당이 변환이 항상 되므로 회전을 변환 합니다.

매우 큰 차이점은 SKiaSharp 변환 않는다는 *메서드* Xamarin.Forms 변환은 동안 *속성*합니다. 이 구문상의 차이 외는 의미 체계 차이가: SkiaSharp 변환 Xamarin.Forms 변환 상태를 설정 하는 동안 작업을 수행 합니다. SkiaSharp 변환 그래픽 개체에는 변환이 적용 되기 전에 그려진 아닌 이후에 그려지는 그래픽 개체에 적용 합니다. 반면, 속성을 설정 하는 즉시 Xamarin.Forms 변환 이전에 렌더링 된 요소에 적용 합니다. 이라고 하는 메서드는; SkiaSharp 변환 누적 됩니다. Xamarin.Forms 변환 속성을 다른 값으로 대체 됩니다.

이 섹션의 모든 샘플 프로그램 머리글 아래에 나타나고 **변환** 의 홈 페이지에는 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 프로그램 및는 [ **변환** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms) 솔루션의 폴더입니다.

## <a name="the-translate-transformtranslatemd"></a>[좌표 이동 변환](translate.md)

이동할 SkiaSharp 그래픽 이동 변환을 사용 하는 방법에 알아봅니다.

## <a name="the-scale-transformscalemd"></a>[배율 변환](scale.md)

개체를 다양 한 크기 조절용 SkiaSharp 배율 변환을 검색 합니다.

## <a name="the-rotate-transformrotatemd"></a>[회전 변환](rotate.md)

및 SkiaSharp 회전 변환을 사용 하 여 가능한 애니메이션 효과 탐색 합니다.

## <a name="the-skew-transformskewmd"></a>[기울이기 변환](skew.md)

로 인해 기울이기 변환을 SkiaSharp의 기운된 그래픽 개체를 만들 수는 방법을 참조 하십시오.

## <a name="matrix-transformsmatrixmd"></a>[매트릭스 변환](matrix.md)

심층적으로 알아보기 다양 한 변환 매트릭스와 SkiaSharp 변환 합니다.

## <a name="touch-manipulationstouchmd"></a>[터치 조작](touch.md)

사용 하 여 행렬 끌어, 크기 조정 및 회전에 대 한 터치 조작을 구현 하는 변형 합니다.

## <a name="non-affine-transformsnon-affinemd"></a>[비아핀(Non-Affine) 변환](non-affine.md)

유사 형식이 아닌 변형 효과로 인해 oridinary 넘지.

## <a name="3d-rotation3d-rotationmd"></a>[3D 회전](3d-rotation.md)

사용 하 여 유사 형식이 아닌 3D 공간에서 개체를 2D 회전을 변형 합니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
