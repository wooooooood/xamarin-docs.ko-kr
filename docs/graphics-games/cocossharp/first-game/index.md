---
title: "CocosSharp와 게임 개발 소개"
description: "이 다중 파트 연습 CocosSharp를 사용 하 여 간단한 2D 게임을 만드는 방법을 보여 줍니다. 그래픽, 입력 및 물리학 같은 일반적인 게임 프로그래밍 개념을 설명합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: BCA99A61-A48D-4207-9A35-4C62F10C9AA5
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 3b1bb45ab87c85dff42b4f7ea5297eb3596b81a5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-game-development-with-cocossharp"></a>CocosSharp와 게임 개발 소개

_이 다중 파트 연습 CocosSharp를 사용 하 여 간단한 2D 게임을 만드는 방법을 보여 줍니다. 그래픽, 입력 및 물리학 같은 일반적인 게임 프로그래밍 개념을 설명합니다._

CocosSharp 2D 게임 엔진 플랫폼 간 게임을 만들기 위한 기술을 제공 합니다. 전체 목록은 지원 되는 플랫폼에 대 한 참조는 [GitHub에서 CocosSharp wiki](https://github.com/mono/CocosSharp/wiki)합니다. 이 자습서는 사용 C# 코드 샘플도 CocosSharp는 F #을 사용한 완벽 하 게 작동 합니다.

CocosSharp의 핵심에서 제공 되는 [MonoGame 프레임 워크](http://www.monogame.net/)을 만드는데 자체 플랫폼 간, 하드웨어 가속 자산 가져오기에 대 한 그래픽, 오디오, 게임 상태 관리, 입력 및 콘텐츠 파이프라인을 제공 하는 API입니다. CocosSharp 적합 2D 게임에 대 한 효율적인 추상화 계층입니다. 또한 더 큰 게임 복잡성에서 게임까지 확장 되면서 자신의 핵심 라이브러리 외부에서 자신의 최적화를 수행할 수 있습니다. 즉, CocosSharp는 개발자가 게임 크기나 복잡성에 국한 되지 않고 빠르게 시작할 수 있도록 사용 및 성능, 쉬운을 혼합 하 여 제공 합니다.

첫 번째 섹션에서는이 연습에서는 기준 빈 프로젝트를 설정 합니다.  두 번째 부분은 모든 우리의 게임 논리 작성을 설명 합니다. 

이 연습의 끝으로는 만들어져 플레이어의 목표를 볼 화면을 벗어날 떨어지는에서 유지에 수평으로 패 않도록 여기서는 간단한 게임. 각 바운스 시점에서 플레이어의 점수를 증가 합니다.

![](images/image1.png "각 바운스 시점에서 플레이어의 점수를 증가 합니다.")

# <a name="walkthrough-parts"></a>연습 부분

* [1 – CocosSharp 프로젝트를 만드는 단계](~/graphics-games/cocossharp/first-game/part1.md)
* [Part 2-는 BouncingGame 구현](~/graphics-games/cocossharp/first-game/part2.md)

## <a name="related-links"></a>관련 링크

- [게임 콘텐츠 (샘플)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
- [완료 된 프로젝트 (샘플)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [NuGet에 CocosSharp PCL](http://www.nuget.org/packages/CocosSharp.PCL.Shared/)
- [CocosSharp API 설명서](http://developer.xamarin.comhttps://developer.xamarin.com/api/namespace/CocosSharp/)
