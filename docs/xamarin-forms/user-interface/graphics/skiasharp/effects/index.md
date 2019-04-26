---
title: SkiaSharp 효과
description: 그라데이션으로 그래픽의 기본 표시 크기를 변경, 바둑판식 배열 비트맵, 혼합 모드, 흐리게 표시 하는 방법 및 기타 효과 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B3E06572-8E2A-49FA-90D1-444C394CD516
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2018
ms.openlocfilehash: 121d505d578aa20e86977c0da5d69626bbad1f53
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61289307"
---
# <a name="skiasharp-effects"></a>SkiaSharp 효과

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

SkiaSharp [ `SKPaint` ](xref:SkiaSharp.SKPaint) 클래스의 일반 용어에서 분류할 수 있는 6 개의 속성을 정의 합니다. _효과_합니다. 이러한 값은 어떤 방식으로든에서 그래픽의 기본 표시 크기를 변경 하는 속성입니다. SkiaSharp 효과 6 개의 범주로 구분 됩니다.

## <a name="path-effectscurveseffectsmd"></a>[경로 효과](../curves/effects.md)

설정 합니다 [ `PathEffect` ](xref:SkiaSharp.SKPaint.PathEffect) 의 속성 `SKPaint` 형식의 개체로 [ `SKPathEffect` ](xref:SkiaSharp.SKPathEffect) 파선을 표시 영역 경로에서 만든 패턴으로 채우기 또는 스트로크 하려면. 문서의이 시리즈의 앞부분에 나오는 경로 효과 다루었습니다 [ **SkiaSharp에서 경로 효과**](../curves/effects.md)합니다.

## <a name="shadersshadersindexmd"></a>[셰이더](shaders/index.md)

설정 합니다 [ `Shader` ](xref:SkiaSharp.SKPaint.Shader) 속성을 `SKPaint` 형식의 개체에 [ `SKShader` ](xref:SkiaSharp.SKShader) 선형 또는 순환 그라데이션, 바둑판식으로 배열 된 비트맵 및 Perlin 노이즈 패턴을 표시 하려면.

## <a name="blend-modesblend-modesindexmd"></a>[혼합 모드](blend-modes/index.md)

설정 합니다 [ `BlendMode` ](xref:SkiaSharp.SKPaint.BlendMode) 의 속성 `SKPaint` 의 멤버에는 [ `SKBlendMode` ](xref:SkiaSharp.SKBlendMode) 대상에 소스 그래픽이 표시 되는 경우를 제어 하는 열거형입니다. SkiaSharp 모든 CSS 합치기와 blend 모드 Porter 임신 모드, 분리 가능한 혼합 모드 및 분리 되지 않은 혼합 모드를 포함 하 여 지원 합니다.

## <a name="mask-filtersmask-filtersmd"></a>[마스크 필터](mask-filters.md)

설정 합니다 [ `MaskFilter` ](xref:SkiaSharp.SKPaint.MaskFilter) 속성을 `SKPaint` 형식의 개체로 [ `SKMaskFilter` ](xref:SkiaSharp.SKMaskFilter) 흐림 효과 기타 알파 효과 대 한 합니다.

## <a name="image-filtersimage-filtersmd"></a>[이미지 필터](image-filters.md)

설정 합니다 [ `ImageFilter` ](xref:SkiaSharp.SKPaint.ImageFilter) 속성을 `SKPaint` 형식의 개체에 [ `SKImageFilter` ](xref:SkiaSharp.SKImageFilter) 흐림 비트맵 효과 적용 하 고 만드는 그림자에 대 한 볼록, 또는 효과 조각입니다.

## <a name="color-filterscolor-filtersmd"></a>[색 필터](color-filters.md)

설정 합니다 [ `ColorFilter` ](xref:SkiaSharp.SKPaint.ColorFilter) 속성을 `SKPaint` 형식의 개체에 [ `SKColorFilter` ](xref:SkiaSharp.SKColorFilter) 행렬 변환 또는 테이블을 사용 하 여 색을 변경 하려면.

이러한 문서에 대 한 모든 샘플 코드는 [ **SkiaSharpFormsDemos**](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)합니다. 홈 페이지에서 선택 **SkiaSharp 효과**합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
