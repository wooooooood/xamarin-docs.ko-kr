---
title: SkiaSharp 변환
description: 이 문서에서는 응용 프로그램에 SkiaSharp 그래픽을 표시 하기 위한 변환을 알아보고 Xamarin.Forms 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e20ea5d1d3f813b04a927601fbe1180ff39ed176
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140194"
---
# <a name="skiasharp-transforms"></a>SkiaSharp 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp 그래픽을 표시 하기 위한 변환에 대해 알아보기_

SkiaSharp는 개체의 메서드로 구현 되는 일반적인 그래픽 변환을 지원 [`SKCanvas`](xref:SkiaSharp.SKCanvas) 합니다. 수학적으로 변환은 `SKCanvas` 그래픽 개체가 렌더링 될 때 그리기 함수에서 지정 하는 좌표 및 크기를 변경 합니다. 변환은 반복적인 그래픽 또는 애니메이션을 그리는 데 편리한 경우가 많습니다. &mdash;비트맵 또는 텍스트 회전과 같은 일부 기술은 &mdash; 변환을 사용 하지 않고도 수행할 수 있습니다.

SkiaSharp 변환은 다음과 같은 작업을 지원 합니다.

- 한 위치에서 다른 *위치로 좌표 이동*
- *크기를 조정* 하 여 좌표 및 크기를 늘리거나 줄입니다.
- *회전* 하 여 점을 중심으로 좌표를 회전 합니다.
- 사각형이 평행 사변형이 되도록 가로 또는 세로로 좌표를 이동 하는 *오차*

이러한 방식을 *상관* 변환 이라고 합니다. 상관 변환은 항상 병렬 선을 유지 하 고 좌표나 크기가 무한 하지 않도록 합니다. 사각형은 평행 사변형 이외의 값으로 변환 되지 않으며 원은 타원 이외의 값으로 변환 되지 않습니다.

SkiaSharp는 표준 3 x 3 변환 매트릭스를 기반으로 하는 비 상관 변환 ( *projective* 또는 *큐브 뷰* 라고도 함)도 지원 합니다. 비 상관 변환을 통해 사각형을 모든 볼록 사변형 변환할 수 있습니다 .이는 모든 내부 각도가 180도 미만인 4 면의 그림입니다. 비 관계 변환으로 인해 좌표나 크기가 무한 해질 수 있지만 3D 효과에는 중요 합니다.

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>SkiaSharp와 변환 간의 차이점 Xamarin.Forms

Xamarin.Forms는 SkiaSharp의 변환과 유사한 변환도 지원 합니다. Xamarin.Forms [`VisualElement`](xref:Xamarin.Forms.VisualElement) 클래스는 다음과 같은 변환 속성을 정의 합니다.

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) 및 [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation), [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) 및 [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

`RotationX`및 `RotationY` 속성은 준 3d 효과를 만드는 원근 변환입니다.

SkiaSharp 변환과 변환 사이에는 몇 가지 중요 한 차이점이 있습니다 Xamarin.Forms .

첫 번째 차이점은 `SKCanvas` Xamarin.Forms 변형이 개별 파생에 적용 되는 동안 SkiaSharp 변환이 전체 개체에 적용 된다는 것입니다 `VisualElement` . Xamarin.Forms `SKCanvasView` `SKCanvasView` 는에서 파생 되 고, `VisualElement` 그 안에서는 SkiaSkarp 변환이 적용 되기 때문에 개체 자체에 변환을 적용할 수 있습니다 `SKCanvasView` .

SkiaSharp 변환은의 왼쪽 위 모퉁이를 기준으로 하며, `SKCanvas` Xamarin.Forms 변환은 적용 되는의 왼쪽 위 모퉁이를 기준으로 합니다 `VisualElement` . 이러한 차이는 크기 조정 및 회전 변환을 적용 하는 경우에 중요 합니다. 이러한 변환은 항상 특정 점을 기준으로 하기 때문입니다.

중요 한 차이점은 변환은 SKiaSharp 변환에 대 *methods* 한 것 이지만 Xamarin.Forms 변환은 *속성*입니다. 이는 구문상 차이를 제외한 의미 체계 차이입니다. SkiaSharp 변환은 Xamarin.Forms 변환 상태를 설정 하는 동안 작업을 수행 합니다. SkiaSharp 변환은 이후에 그려진 그래픽 개체에 적용 되지만 변환이 적용 되기 전에 그려진 그래픽 개체에는 적용 되지 않습니다. 반면, Xamarin.Forms 변환은 속성이 설정 되는 즉시 이전에 렌더링 된 요소에 적용 됩니다. SkiaSharp 변환은 메서드가 호출 될 때 누적 됩니다. Xamarin.Forms속성이 다른 값으로 설정 된 경우 변환이 바뀝니다.

이 단원의 모든 샘플 프로그램은 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 프로그램의 **SkiaSharp 변환** 섹션에 표시 됩니다. 솔루션의 [**변환**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms) 폴더에서 소스 코드를 찾을 수 있습니다.

## <a name="the-translate-transform"></a>[좌표 이동 변환](translate.md)

변환 변환을 사용 하 여 SkiaSharp 그래픽을 이동 하는 방법을 알아봅니다.

## <a name="the-scale-transform"></a>[배율 변환](scale.md)

개체를 다양 한 크기로 크기 조정 하기 위한 SkiaSharp 배율 변환을 검색 합니다.

## <a name="the-rotate-transform"></a>[회전 변환](rotate.md)

SkiaSharp 회전 변환으로 가능한 효과 및 애니메이션을 탐색 합니다.

## <a name="the-skew-transform"></a>[기울이기 변환](skew.md)

기울이기 변환이 기울임 그래픽 개체를 만드는 방법을 참조 하세요.

## <a name="matrix-transforms"></a>[매트릭스 변환](matrix.md)

다양 한 변환 매트릭스를 사용 하 여 SkiaSharp 변환에 대해 자세히 알아봅니다.

## <a name="touch-manipulations"></a>[터치 조작](touch.md)

행렬 변환을 사용 하 여 끌기, 크기 조정 및 회전에 대 한 터치 조작을 구현할 수 있습니다.

## <a name="non-affine-transforms"></a>[비아핀(Non-Affine) 변환](non-affine.md)

비 상관 변환 효과를 사용 하 여 oridinary 이상으로 이동 합니다.

## <a name="3d-rotation"></a>[3D 회전](3d-rotation.md)

비 상관 변환을 사용 하 여 3D 공간에서 2D 개체를 회전 합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
