---
title: CocosSharp
description: 이 문서는 CocosSharp와 게임 개발에 대 한 다양 한 문서 링크 합니다.
ms.prod: xamarin
ms.assetid: 5E72869D-3541-408B-AB64-D34C777AFB79
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2018
ms.openlocfilehash: 2772af61dfc60b7ecd0fd5f7ecdfe5701d2504f0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="cocossharp"></a>CocosSharp

_CocosSharp는 C# 및 F #을 사용 하 여 2D 게임을 작성 하기 위한 라이브러리입니다. 그는 인기 있는 Cocos2D 엔진의.NET 포트입니다._

## <a name="introduction-to-cocossharp"></a>CocosSharp 소개

CocosSharp 2D 게임 엔진 플랫폼 간 게임을 만들기 위한 기술을 제공 합니다. 전체 목록은 지원 되는 플랫폼에 대 한 참조는 [GitHub에서 CocosSharp wiki](https://github.com/mono/CocosSharp/wiki)합니다.
이러한 가이드 C#을 사용에 대 한 코드 예제도 CocosSharp는 F #을 사용한 완벽 하 게 작동 합니다.

CocosSharp의 핵심에서 제공 되는 [MonoGame 프레임 워크](http://www.monogame.net/)을 만드는데 자체 플랫폼 간, 하드웨어 가속 자산 가져오기에 대 한 그래픽, 오디오, 게임 상태 관리, 입력 및 콘텐츠 파이프라인을 제공 하는 API입니다.
CocosSharp 적합 2D 게임에 대 한 효율적인 추상화 계층입니다.
또한 더 큰 게임 복잡성에서 게임까지 확장 되면서 자신의 핵심 라이브러리 외부에서 자신의 최적화를 수행할 수 있습니다. 즉, CocosSharp는 개발자가 게임 크기나 복잡성에 국한 되지 않고 빠르게 시작할 수 있도록 사용 및 성능, 쉬운을 혼합 하 여 제공 합니다.

이 실습 비디오 게임을 만드는 간단한 플랫폼 간 CocosSharp 방법을 보여 줍니다.

> [!Video https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Developing-Cross-platform-2D-Games-in-C-and-CocosSharp/player]

## <a name="bouncinggamegraphics-gamescocossharpbouncing-gamemd"></a>[BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)

![BouncingGame](images/bouncing-game.png "BouncingGame")

이 가이드에서는 BouncingGame, 게임 콘텐츠, 게임을 빌드, 게임 논리 추가 하는 데 사용 하 고 다양 한 시각적 요소를 사용 하는 방법을 비롯 하 여 설명 합니다.

## <a name="fruity-falls-gamegraphics-gamescocossharpfruity-fallsmd"></a>[게임 Fruity 위치 합니다.](~/graphics-games/cocossharp/fruity-falls.md)

![Fruity 까지의 게임 스크린 샷](images/fruity-falls.png "Fruity 까지의 게임 스크린 샷")

이 가이드에 일반적인 CocosSharp와 게임 디자인 물리학, 콘텐츠 관리, 게임 상태 등 게임 개발 개념을 다루는 Fruity 까지의 게임을 설명 합니다.  

## <a name="coin-time-gamegraphics-gamescocossharpcointimemd"></a>[동전 시간 게임](~/graphics-games/cocossharp/cointime.md)

![시간 게임 스크린 샷 동전](images/cointime.png "동전 시간 게임 스크린 샷")

동전 시간은 iOS 및 Android 용 게임는 전체 platformer입니다. 게임의 목표는 모든 수준의 동전 수집를 다음 적 및 장애를 방지 하면서 종료 문 도달 하는 것입니다.

## <a name="drawing-geometry-with-ccdrawnodegraphics-gamescocossharpccdrawnodemd"></a>[CCDrawNode로 기 하 도형 그리기](~/graphics-games/cocossharp/ccdrawnode.md)

![셰이프 CCDrawNode를 사용 하 여 그린](images/ccdrawnode.png "CCDrawNode를 사용 하 여 그릴 모양")

CCDrawNode 선, 원, 삼각형 등 기본 개체를 그리기 위한 메서드를 제공 합니다.

## <a name="animating-with-ccactiongraphics-gamescocossharpccactionmd"></a>[CCAction을 사용한 애니메이션](~/graphics-games/cocossharp/ccaction.md)

![CCAction 애니메이션](images/ccaction.png "A CCAction 애니메이션")

`CCAction` CocosSharp 개체에 애니메이션 효과를 사용할 수 있는 기본 클래스가입니다. 이 가이드에서는 기본 제공 `CCAction` 위치, 크기 조정 및 회전 등의 일반적인 작업에 대 한 구현 합니다. 상속 하 여 사용자 지정 구현을 만드는 방법에 대해서도 설명 `CCAction`합니다.

## <a name="using-tiled-with-cocossharpgraphics-gamescocossharptiledmd"></a>[CocosSharp로 바둑판식 화면 사용](~/graphics-games/cocossharp/tiled.md)

![게임에서 수준을](images/tiled.png "게임의 수준")

바둑판식으로 배열은 강력한 유연 하 고, 하며 직교 및 아이소메트릭 타일을 만들기 위한 완성 된 응용 프로그램 게임에 대 한 매핑 CocosSharp 타일의 기본 파일 형식에 대 한 기본 제공 통합을 제공합니다.

## <a name="entities-in-cocossharpgraphics-gamescocossharpentitiesmd"></a>[CocosSharp의 엔터티](~/graphics-games/cocossharp/entities.md)

![게임에서 우주선](images/entities.png "게임에서 우주선")

엔터티 방법은 게임 코드를 구성 하는 강력한 방법입니다. 가독성을 높일 수, 코드를 보다 쉽게 유지 관리, 기본 제공 부모/자식 기능을 활용 하 고 있습니다.

## <a name="handling-multiple-resolutions-in-cocossharpgraphics-gamescocossharpresolutionsmd"></a>[CocosSharp에 여러 해상도 처리합니다.](~/graphics-games/cocossharp/resolutions.md)

![화면 해상도 나타내는 표](images/resolutions.png "화면 해상도 나타내는 표")

이 가이드에는 다양 한 해상도의 장치에서 제대로 표시 하는 게임을 개발 하는 CocosSharp를 사용 하는 방법을 보여 줍니다.

## <a name="cocossharp-content-pipelinegraphics-gamescocossharpcontent-pipelineindexmd"></a>[CocosSharp 콘텐츠 파이프라인](~/graphics-games/cocossharp/content-pipeline/index.md)

![XNB](images/content-pipeline.png "XNB")

콘텐츠 파이프라인 콘텐츠를 최적화 하 고 특정 하드웨어에 또는 특정 게임 개발 프레임 워크와 함께 로드할 수 있도록 서식을 지정 하려면 게임 개발에 종종 사용 됩니다.

## <a name="improving-frame-rate-with-ccspritesheetgraphics-gamescocossharpccspritesheetmd"></a>[프레임 속도와 CCSpriteSheet 향상](~/graphics-games/cocossharp/ccspritesheet.md)

![CCSpriteSheet에서 트리](images/ccspritesheet.png "는 CCSpriteSheet에서 트리")

`CCSpriteSheet` 결합 하 고 한 텍스처에서 여러 이미지 파일을 사용 하 여에 대 한 기능을 제공 합니다. 질감 수가 감소 게임의 로드 시간 및 프레임 속도 향상 시킬 수 있습니다.

## <a name="texture-caching-using-cctexturecachegraphics-gamescocossharptexture-cachemd"></a>[질감 CCTextureCache를 사용 하 여 캐싱](~/graphics-games/cocossharp/texture-cache.md)

![CocosSharp 이미지를 캐시 하는 방법에 대 한 표현을](images/texture-cache.png "CocosSharp 이미지를 캐시 하는 방식을의 표현")

CocosSharp의 `CCTextureCache` 클래스를 구성, 캐시, 및 콘텐츠를 언로드할 표준 방법을 제공 합니다. 

## <a name="2d-math-with-cocossharpgraphics-gamescocossharpmathmd"></a>[2D 수학 CocosSharp와](~/graphics-games/cocossharp/math.md)

![이미지 회전 되 고](images/math.png "회전 되 고 이미지")

이 가이드에서는 게임 개발에 대 한 2D 수학을 다룹니다. CocosSharp를 사용 하 여 일반적인 게임 개발 작업을 수행 하는 방법을 보여 주는 하 고 수학 뒤에 이러한 작업에 설명 합니다.

## <a name="performance-and-visual-effects-with-ccrendertexturegraphics-gamescocossharpccrendertexturemd"></a>[성능 및 CCRenderTexture로 시각 효과](~/graphics-games/cocossharp/ccrendertexture.md)

![게임에서 스프라이트](images/ccrendertexture.png "게임에서 스프라이트")

`CCRenderTexture` 클래스 여러 CocosSharp 개체 단일 텍스처를 렌더링 하기 위한 기능을 제공 합니다. 일단 만들어지면 `CCRenderTexture` 그래픽을 효율적으로 렌더링 하 고 시각 효과 구현 하려면 인스턴스를 사용할 수 있습니다.
