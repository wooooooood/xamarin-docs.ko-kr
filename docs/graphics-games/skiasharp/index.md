---
title: 2D 그리기
description: 크로스 플랫폼 2D 드로잉 SkiaSharp와
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: a6249525e8a5f85284c462888a7698312321642f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="2d-drawing"></a>2D 그리기

SkiaSharp 2 차원 그래픽을 수행 하기 위한 강력한 C# API를 제공 합니다. 으로 작동 하는 [Google의 Skia 라이브러리](http://skia.org), Google Chrome, Firefox 및 Android의 그래픽 스택을 구동 하는 라이브러리입니다.

[![](images/ide-sml.png "2 차원 그래픽을 수행 하기 위한 강력한 C# API를 제공 하는 SkiaSharp")](images/ide.png#lightbox)

SkiaSharp는 휴대용 라이브러리 하며으로 편리 하 게 포함 한 [플랫폼 간 NuGet 패키지](https://www.nuget.org/packages/SkiaSharp), 즉시 다음 플랫폼을 지원 하 고: macOS 등 Xamarin.Android, Xamarin.iOS, 및의 Windows 바탕 화면입니다.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[SkiaSharp 소개](~/graphics-games/skiasharp/introduction.md)

SkiaSharp 및 샘플의 핵심 개념에 대 한 개요 그래픽, 텍스트, 비트맵, 렌더링 하 고 이미지 필터:를 사용 합니다.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[Xamarin.Forms에 대 한 SkiaSharp 자습서](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Xamarin.Forms에 렌더링 하는 플랫폼 그래픽 크로스 작업할 방법에 알아봅니다.

- [그리기 기본 사항](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [단순 원을 그리기](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Xamarin.Forms와 통합](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [픽셀과 장치 독립적 단위](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [기본 애니메이션](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [텍스트와 그래픽 통합](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [비트맵 기본 사항](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [선 및 경로](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [선 및 스트로크 단면](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [경로 기본 사항](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [경로 채우기 유형](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [폴리라인 및 파라메트릭 수식](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [점, 대시](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [손가락으로 그리기](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [이동 변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [크기 조정 변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [회전 변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [시간차 변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [매트릭스 변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [터치 조작](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [비 3x3 유사 변형](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [3D 회전](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [곡선 및 경로](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [원호를 그리는 3가지 방법](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [3가지 형식의 Bézier 곡선](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [SVG 경로 데이터](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [경로 및 지역 클리핑](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [경로 효과](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [경로 및 텍스트](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [경로 정보 및 열거형](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[플랫폼별 설명](~/graphics-games/skiasharp/platform.md)

이 페이지에서는 iOS, Android, macOS 등 및 Windows를 포함 하 여 서로 다른 플랫폼에서 SkiaSharp에 대 한 설치 지침을 설명 합니다.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[API 설명서](https://developer.xamarin.com/api/namespace/SkiaSharp/)

찾아볼 수는 [API 설명서](https://developer.xamarin.com/api/namespace/SkiaSharp/) SkiaSharp 웹 사이트에 대 한 합니다.

## <a name="work-in-progress"></a>진행 중

SkiaSharp는 진행 중인 작업 커뮤니티와 공유 하는 것입니다. Skia API의 중요 한 부분을 바인딩한 우리 하는 동안 많은 작업을 수행 해야 하는 남아 있습니다. Skia를 노출 하는 안정적인 C API를 사용 하 고 현재 계획으로 만드는 작업의 세부적인 Api를 제공 하는 Skia C 바인딩에 참여를 계속 하려면는 합니다.

하기 위한 우리의 바인딩 노력 안내선, 남겨 주세요 의견이 나 제안 문제 GitHub 리포지토리에서 [ http://github.com/mono/SkiaSharp ](http://github.com/mono/SkiaSharp)합니다.
