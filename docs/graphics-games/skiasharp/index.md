---
title: SkiaSharp 사용 하 여 2D 그리기
description: 이 문서는 SkiaSharp 사용 하 여 그리기 플랫폼 간 2D의 개요를 제공 합니다. SkiaSharp를 설명 하는 다양 한 가이드 및 다양 한 Api에 연결 합니다.
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: 0c8cbc14308c8c4131e5aaa2bcc0ddfa798af610
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130922"
---
# <a name="2d-drawing-with-skiasharp"></a>SkiaSharp 사용 하 여 2D 그리기

SkiaSharp 2D 그래픽 작업을 수행 하는 강력한 C# API를 제공 합니다. 전원을 [Google의 Skia 라이브러리](http://skia.org), Google Chrome, Firefox 및 Android의 그래픽 스택을 구동 하는 라이브러리입니다.

[![](images/ide-sml.png "SkiaSharp 2D 그래픽을 수행 하기 위한 강력한 C# API를 제공")](images/ide.png#lightbox)

SkiaSharp 이식 가능한 라이브러리 이며으로 편리 하 게 제공 되는 [플랫폼 간 NuGet 패키지](https://www.nuget.org/packages/SkiaSharp), 즉시 다음 플랫폼을 지원 하 고: macOS, Xamarin.Android, Xamarin.iOS 및 Windows 바탕 화면.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[SkiaSharp 소개](~/graphics-games/skiasharp/introduction.md)

SkiaSharp 및 샘플의 핵심 개념 개요 그래픽, 텍스트, 비트맵을 렌더링 하는 코드와 이미지 필터를 사용 합니다.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[Xamarin.Forms에 대 한 SkiaSharp 자습서](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Xamarin.Forms에서 렌더링 되는 플랫폼 그래픽 간 사용 하는 방법에 알아봅니다.

- [그리기 기본 사항](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [단순 원 그리기](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Xamarin.Forms와 통합](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [픽셀 및 장치 독립적 단위](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [기본 애니메이션](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [텍스트와 그래픽 통합](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [비트맵 기본 사항](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [선 및 경로](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [선 및 스트로크 단면](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [경로 기본 사항](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [경로 채우기 유형](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [폴리라인 및 파라메트릭 수식](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [점 및 대시](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [손가락으로 그리기](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [좌표 이동 변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [배율 변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [회전 변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [기울이기 변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [매트릭스 변환](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [터치 조작](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [비 affine 변환만](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [3D 회전](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [곡선 및 경로](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [원호를 그리는 3가지 방법](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [3가지 형식의 Bézier 곡선](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [SVG 경로 데이터](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [경로 및 지역 클리핑](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [경로 효과](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [경로 및 텍스트](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [경로 정보 및 열거형](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)
- [비트맵](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/index.md)
  * [비트맵을 표시합니다.](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/displaying.md)
  * [만들고 비트맵에 그리기](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/drawing.md)
  * [비트맵을 자르기](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/cropping.md)
  * [비트맵의 분할된 된 표시](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/segmented.md)
  * [비트맵 파일에 저장](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/saving.md)
  * [비트맵 픽셀 비트에 액세스](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/pixel-bits.md)
  * [비트맵에 애니메이션 적용](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/animating.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[플랫폼별 설명](~/graphics-games/skiasharp/platform.md)

이 페이지에는 iOS, Android, macOS 및 Windows를 비롯 한 다양 한 플랫폼에서 SkiaSharp에 대 한 설치 지침을 설명 합니다.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[API 설명서](https://developer.xamarin.com/api/namespace/SkiaSharp/)

찾아볼 수 있습니다 합니다 [API 설명서](https://developer.xamarin.com/api/namespace/SkiaSharp/) SkiaSharp 웹 사이트에 대 한 합니다.

## <a name="work-in-progress"></a>진행 중

SkiaSharp는 진행 중인 작업 커뮤니티와 공유 하는 것입니다. Skia API의 중요 한 부분을 바인딩한 우리가 하는 동안 수행 남은 작업량입니다. Skia에 의해 표시 되는 안정적인 C API를 사용 하는 것 이며 온갖 작업의 전체 검사 Api를 제공 하는 Skia C 바인딩에 영향을 주는 계속 합니다.

바인딩 노력 가이드를 남겨 주세요 의견이 나 제안 문제 GitHub 리포지토리에서 도움이 [ http://github.com/mono/SkiaSharp ](http://github.com/mono/SkiaSharp)합니다.
