---
title: SkiaSharp 효과
description: 그라데이션, 비트맵 바둑판식 배열, blend 모드, 흐림 효과 및 기타 효과를 사용 하 여 그래픽의 일반 표시를 변경 하는 방법에 대해 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B3E06572-8E2A-49FA-90D1-444C394CD516
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d9fa710f5dfc61c2892b8fc409a39b37cf449018
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136307"
---
# <a name="skiasharp-effects"></a>SkiaSharp 효과

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp [`SKPaint`](xref:SkiaSharp.SKPaint) 클래스는 일반적인 _효과_용어로 분류할 수 있는 6 개의 속성을 정의 합니다. 이러한 속성은 그래픽의 정상적인 표시를 변경 하는 속성입니다. SkiaSharp 효과는 다음과 같은 여섯 가지 범주로 나뉩니다.

## <a name="path-effects"></a>[경로 효과](../curves/effects.md)

[`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect)의 속성을 형식의 개체로 설정 하 여 `SKPaint` [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) 파선을 표시 하거나 경로에서 만든 패턴으로 영역을 스트로크 또는 채웁니다. 경로 효과는이 시리즈의 앞부분에 있는 [**SkiaSharp의 경로 효과**](../curves/effects.md)문서에 설명 되어 있습니다.

## <a name="shaders"></a>[셰이더](shaders/index.md)

[`Shader`](xref:SkiaSharp.SKPaint.Shader)의 속성을 `SKPaint` 형식의 개체로 설정 [`SKShader`](xref:SkiaSharp.SKShader) 하 여 선형 또는 원형 그라데이션, 바둑판식 비트맵 및 Perlin 노이즈 패턴을 표시 합니다.

## <a name="blend-modes"></a>[혼합 모드](blend-modes/index.md)

[`BlendMode`](xref:SkiaSharp.SKPaint.BlendMode)의 속성을 `SKPaint` 열거형의 멤버로 설정 하 여 [`SKBlendMode`](xref:SkiaSharp.SKBlendMode) 원본 그래픽이 대상에 표시 될 때 발생 하는 결과를 제어 합니다. SkiaSharp는 Porter-Duff 모드, 분리 가능 blend 모드 및 분리 되지 않은 혼합 모드를 비롯 하 여 모든 CSS 합성 모드와 혼합 모드를 지원 합니다.

## <a name="mask-filters"></a>[마스크 필터](mask-filters.md)

[`MaskFilter`](xref:SkiaSharp.SKPaint.MaskFilter)의 속성을 `SKPaint` [`SKMaskFilter`](xref:SkiaSharp.SKMaskFilter) 흐린 및 기타 알파 효과에 대 한 형식의 개체로 설정 합니다.

## <a name="image-filters"></a>[이미지 필터](image-filters.md)

[`ImageFilter`](xref:SkiaSharp.SKPaint.ImageFilter)의 속성을 `SKPaint` [`SKImageFilter`](xref:SkiaSharp.SKImageFilter) 흐린 비트맵의 경우 형식의 개체로 설정 하 고 그림자, 볼록 또는 engraving 효과를 만듭니다.

## <a name="color-filters"></a>[색 필터](color-filters.md)

[`ColorFilter`](xref:SkiaSharp.SKPaint.ColorFilter) `SKPaint` [`SKColorFilter`](xref:SkiaSharp.SKColorFilter) 테이블 또는 행렬 변환을 사용 하 여 색을 변경 하려면의 속성을 형식의 개체로 설정 합니다.

이러한 아티클의 모든 샘플 코드는 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)에 있습니다. 홈 페이지에서 **SkiaSharp 효과**를 선택 합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
