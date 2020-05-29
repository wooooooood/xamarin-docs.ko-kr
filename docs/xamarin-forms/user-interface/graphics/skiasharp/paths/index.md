---
title: ''
description: 이 문서에서는 SkiaSharp를 사용 하 여 응용 프로그램에서 선과 그래픽 경로를 그리는 방법을 설명 하 Xamarin.Forms 고 샘플 코드를 사용 하 여이를 보여 줍니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 97c7305e59a023e65535186bbbe39a9c2b7d4c26
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138998"
---
# <a name="skiasharp-lines-and-paths"></a>SkiaSharp 선 및 경로

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp를 사용 하 여 선 및 그래픽 패스 그리기_

[이전 섹션](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) 에서는 SkiaSharp `SKCanvas` 클래스가 원, 타원, 사각형, 둥근 사각형, 텍스트 및 비트맵을 그리는 여러 메서드를 포함 하 고 있음을 보여 주었습니다. 이 섹션 및 이후 섹션에서는 *그래픽 경로*를 만들고 렌더링 하는 데 연결 된 다양 한 클래스를 다룹니다.

그래픽 경로는 SkiaSharp에서 선과 곡선을 그리는 가장 일반화 된 방법입니다. 이 섹션에서는 개체를 사용 하 여 [`SKPath`](xref:SkiaSharp.SKPath) 직선을 그리는 방법 및 작은 직선 ( *다중선*이라고 함)의 컬렉션을 사용 하 여 알고리즘 방식으로 정의할 수 있는 곡선을 그리는 방법을 설명 합니다. [**SkiaSharp 곡선 및 경로**](../curves/index.md) 에 대 한 이후 섹션에서는에서 지 원하는 다양 한 곡선 종류에 대해 설명 `SKPath` 합니다.

이 섹션의 모든 샘플 프로그램은 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 프로그램의 홈 페이지 및 해당 솔루션의 [**paths**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths) 폴더에 있는 제목 **선 및 경로** 아래에 나타납니다.

## <a name="lines-and-stroke-caps"></a>[선 및 스트로크 단면](lines.md)

SkiaSharp를 사용 하 여 다른 스트로크 캡이 있는 선을 그리는 방법에 대해 알아봅니다.

## <a name="path-basics"></a>[경로 기본 사항](paths.md)

`SKPath`선과 곡선을 결합 하는 SkiaSharp 개체를 탐색 합니다.

## <a name="the-path-fill-types"></a>[경로 채우기 유형](fill-types.md)

SkiaSharp 경로 채우기 유형으로 가능한 다른 효과를 검색 합니다.

## <a name="polylines-and-parametric-equations"></a>[폴리라인 및 파라메트릭 수식](polylines.md)

SkiaSharp를 사용 하 여 패라메트릭 방정식으로 정의할 수 있는 선을 렌더링 합니다.

## <a name="dots-and-dashes"></a>[점 및 대시](dots.md)

SkiaSharp에서 점선 및 파선 그리기의 복잡성을 줄입니다.

## <a name="finger-painting"></a>[손가락으로 그리기](finger-paint.md)

손가락을 사용 하 여 캔버스에 그립니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
