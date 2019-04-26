---
title: CocosSharp 2D 게임 엔진
description: 이 문서는 CocosSharp 사용한 게임 개발에 대 한 다양 한 문서에 연결 합니다. 연결 된 콘텐츠는 샘플 앱, 그리기, 애니메이션 등을 설명합니다.
ms.prod: xamarin
ms.assetid: 5E72869D-3541-408B-AB64-D34C777AFB79
author: conceptdev
ms.author: crdun
ms.date: 03/29/2018
ms.openlocfilehash: add73360ea98d8c516e413f0cc0264f68c58d79d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61024452"
---
# <a name="cocossharp-2d-game-engine"></a>CocosSharp 2D 게임 엔진

_CocosSharp를 사용 하 여 2D 게임 빌드에 대 한 라이브러리는 C# 및 F#합니다. .NET 이식 판 인기 있는 Cocos2D 엔진의 경우_

## <a name="introduction-to-cocossharp"></a>CocosSharp 소개

CocosSharp 2D 게임 엔진 플랫폼 간 게임을 만들기 위한 기술을 제공 합니다. 지원 되는 플랫폼의 전체 목록을 참조 하세요. 합니다 [github wiki CocosSharp](https://github.com/mono/CocosSharp/wiki)합니다.
이러한 가이드를 사용 하 여 C# 코드 샘플에 대 한 CocosSharp는 완벽 하 게 작동 하지만 F# 도 있습니다.

CocosSharp의 핵심에서 제공 하는 합니다 [MonoGame framework](http://www.monogame.net/)는 자체는 플랫폼 간, 하드웨어 가속 자산 가져오기에 대 한 그래픽, 오디오, 게임 상태 관리, 입력 및 콘텐츠 파이프라인을 제공 하는 API입니다.
CocosSharp는 2D 게임에 적합 하는 효율적인 추상화 계층입니다.
또한 더 큰 게임 게임 복잡성 증가 함에 자신의 핵심 라이브러리 외부에서 자체 최적화를 수행할 수 있습니다. 즉, CocosSharp는 개발자가 게임 크기나 복잡성을 제한 하지 않고 빠르게 시작할 수 있도록 다양 한 성능 및 사용 편의성을 제공 합니다.

이 실습 비디오 게임을 간단한 플랫폼 간 CocosSharp를 만드는 방법을 보여 줍니다.

> [!Video https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Developing-Cross-platform-2D-Games-in-C-and-CocosSharp/player]

## <a name="bouncinggamegraphics-gamescocossharpbouncing-gamemd"></a>[BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)

![BouncingGame](images/bouncing-game.png "BouncingGame")

이 가이드에서는 BouncingGame, 게임 콘텐츠, 게임, 게임 논리 추가 등을 빌드하는 데 다양 한 시각적 요소를 사용 하는 방법을 포함 하 여 설명 합니다.

## <a name="fruity-falls-gamegraphics-gamescocossharpfruity-fallsmd"></a>[Fruity Falls 게임](~/graphics-games/cocossharp/fruity-falls.md)

![Fruity Falls 게임 스크린 샷](images/fruity-falls.png "Fruity Falls 게임 스크린 샷")

이 가이드는 일반적인 CocosSharp와 물리학, 콘텐츠 관리, 게임 상태 게임 디자인 등 게임 개발 개념을 다루는 Fruity 됩니다 게임을 설명 합니다.  

## <a name="coin-time-gamegraphics-gamescocossharpcointimemd"></a>[Coin Time 게임](~/graphics-games/cocossharp/cointime.md)

![Coin Time 게임 스크린샷](images/cointime.png "동전 시간 게임 스크린 샷")

주요 고려 사항인 시간은 iOS 및 Android 용 게임을 전체 platformer입니다. 게임의 목표는 모든 수준의 동전을 수집 하 고 다음 종료 도어 적 및 장애를 방지 하는 동안 도달 하는 것입니다.

## <a name="drawing-geometry-with-ccdrawnodegraphics-gamescocossharpccdrawnodemd"></a>[CCDrawNode 사용한 기 하 도형 그리기](~/graphics-games/cocossharp/ccdrawnode.md)

![CCDrawNode를 사용 하 여 그릴 도형을](images/ccdrawnode.png "CCDrawNode를 사용 하 여 그릴 모양")

CCDrawNode 선, 원 및 삼각형 같은 기본 개체를 그리기 위한 메서드를 제공합니다.

## <a name="animating-with-ccactiongraphics-gamescocossharpccactionmd"></a>[CCAction을 사용한 애니메이션](~/graphics-games/cocossharp/ccaction.md)

![CCAction 애니메이션](images/ccaction.png "는 CCAction 애니메이션")

`CCAction` CocosSharp 개체에 애니메이션 효과를 사용할 수 있는 기본 클래스입니다. 이 가이드에서는 기본 제공 `CCAction` 위치, 크기 조정 및 회전 같은 일반적인 태스크의 구현입니다. 상속 하 여 사용자 지정 구현을 만드는 방법에 대해서도 살펴봅니다 `CCAction`합니다.

## <a name="using-tiled-with-cocossharpgraphics-gamescocossharptiledmd"></a>[CocosSharp로 바둑판식 화면 사용](~/graphics-games/cocossharp/tiled.md)

![게임에서 수준](images/tiled.png "게임의 수준")

바둑판식으로 배열은 강력 하 고 유연 하 고 완성 된 응용 프로그램인 부분도 있고 등각 타일을 만들기 위한 매핑합니다 게임에 대 한 키를 누릅니다. CocosSharp 타일의 네이티브 파일 형식에 대 한 기본 제공 통합을 제공합니다.

## <a name="entities-in-cocossharpgraphics-gamescocossharpentitiesmd"></a>[CocosSharp의 엔터티](~/graphics-games/cocossharp/entities.md)

![게임에서 우주선](images/entities.png "게임에서 우주선")

엔터티 패턴은 게임 코드를 구성 하는 강력한 방법입니다. 가독성 향상, 코드를 보다 쉽게 유지 관리 및 부모/자식 기본 제공 기능을 활용 합니다.

## <a name="handling-multiple-resolutions-in-cocossharpgraphics-gamescocossharpresolutionsmd"></a>[CocosSharp에서 여러 해결 방법 처리](~/graphics-games/cocossharp/resolutions.md)

![화면 해상도 나타내는 표가](images/resolutions.png "화면 해상도 나타내는 표")

이 가이드에는 다양 한 해상도의 장치에서 제대로 표시 하는 게임을 개발 하는 CocosSharp를 사용 하는 방법을 보여 줍니다.

## <a name="cocossharp-content-pipelinegraphics-gamescocossharpcontent-pipelineindexmd"></a>[CocosSharp 콘텐츠 파이프라인](~/graphics-games/cocossharp/content-pipeline/index.md)

![XNB](images/content-pipeline.png "XNB")

콘텐츠 파이프라인은 콘텐츠를 최적화 하 고 특정 하드웨어 또는 특정 게임 개발 프레임 워크를 사용 하 여 로드할 수 있도록 형식 게임 개발에 자주 사용 됩니다.

## <a name="improving-frame-rate-with-ccspritesheetgraphics-gamescocossharpccspritesheetmd"></a>[CCSpriteSheet 사용 하 여 프레임 속도 향상합니다.](~/graphics-games/cocossharp/ccspritesheet.md)

![CCSpriteSheet에서 트리](images/ccspritesheet.png "를 CCSpriteSheet에서 트리")

`CCSpriteSheet` 결합 및 하나의 질감의 여러 이미지 파일을 사용 하 여 기능을 제공 합니다. 텍스처 수를 줄이면 게임의 로드 시간 및 프레임 속도 개선할 수 있습니다.

## <a name="texture-caching-using-cctexturecachegraphics-gamescocossharptexture-cachemd"></a>[텍스처 캐싱 CCTextureCache를 사용 하 여](~/graphics-games/cocossharp/texture-cache.md)

![CocosSharp 이미지를 캐시 하는 방법의 표현](images/texture-cache.png "CocosSharp 이미지를 캐시 하는 방법의 표현")

CocosSharp의 `CCTextureCache` 클래스를 구성, 캐시, 및 콘텐츠 언로드 표준 방법을 제공 합니다. 

## <a name="2d-math-with-cocossharpgraphics-gamescocossharpmathmd"></a>[CocosSharp 사용한 2D 수치 연산](~/graphics-games/cocossharp/math.md)

![회전할 이미지로](images/math.png "회전 되는 이미지")

이 가이드에서는 게임 개발에 대 한 2D 수학을 다룹니다. CocosSharp를 사용 하 여 게임 개발의 일반적인 작업을 수행 하는 방법을 하 고 이러한 작업 뒤 수학에 설명 합니다.

## <a name="performance-and-visual-effects-with-ccrendertexturegraphics-gamescocossharpccrendertexturemd"></a>[성능 및 CCRenderTexture 사용 하 여 시각 효과](~/graphics-games/cocossharp/ccrendertexture.md)

![게임에서 스프라이트](images/ccrendertexture.png "게임에서 스프라이트")

`CCRenderTexture` 클래스 여러 CocosSharp 개체는 단일 텍스처를 렌더링 하기 위한 기능을 제공 합니다. 일단 만들어지면 `CCRenderTexture` 효율적으로 그래픽을 렌더링 하 고 시각 효과 구현 하려면 인스턴스를 사용할 수 있습니다.
