---
title: MonoGame를 사용한 게임 개발 소개
description: 이 다중 파트 연습에서는 MonoGame를 사용 하 여 간단한 2D 응용 프로그램을 만드는 방법을 보여 줍니다.  그래픽, 입력, 게임 엔터티 및 물리학 같은 일반적인 게임 프로그래밍 개념을 다룹니다.
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 8ffb62c8715ff60e9b0ea3b2bc536f3441fb8765
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724676"
---
# <a name="introduction-to-game-development-with-monogame"></a>MonoGame를 사용한 게임 개발 소개

_이 다중 파트 연습에서는 MonoGame를 사용 하 여 간단한 2D 응용 프로그램을 만드는 방법을 보여 줍니다.  그래픽, 입력, 게임 엔터티 및 물리학 같은 일반적인 게임 프로그래밍 개념을 다룹니다._

이 문서에서는 플랫폼 간 게임을 만들기 위한 MonoGame API 기술에 대해 설명 합니다. 플랫폼의 전체 목록은 [MonoGame 웹 사이트](http://www.monogame.net/)를 참조 하세요. 이 자습서에서는 MonoGame C# 완벽 하 게 작동 F# 하는 경우에도 코드 샘플에 대해를 사용 합니다.

MonoGame는 그래픽, 오디오, 게임 상태 관리, 입력 및 자산 가져오기를 위한 콘텐츠 파이프라인을 제공 하는 플랫폼 간 하드웨어 가속 API입니다. 대부분의 게임 엔진과 달리 MonoGame는 패턴 또는 프로젝트 구조를 제공 하거나 적용 하지 않습니다.  이는 개발자가 코드를 원하는 대로 구성할 수 있다는 것을 의미 하는 반면, 새 프로젝트를 처음 시작할 때 약간의 설정 코드가 필요 함을 의미 하기도 합니다.

이 연습의 첫 번째 섹션에서는 빈 프로젝트를 설정 하는 방법을 집중적으로 설명 합니다. 마지막 섹션에서는 모든 게임 논리 및 콘텐츠를 작성 하는 방법을 다룹니다. 대부분은 플랫폼 간입니다.

이 연습을 마치면 플레이어가 터치 입력으로 애니메이션 문자를 제어할 수 있는 간단한 게임을 만들었습니다.  이는 기술적으로 전체 게임을 하지는 않지만 (win 또는 분실 조건이 있으므로) 많은 게임 개발 개념을 보여 주며 많은 유형의 게임을 위한 기반으로 사용할 수 있습니다.

다음은이 연습의 결과를 보여 줍니다.

![마우스 다음에 나오는 샘플 게임 문자 애니메이션](images/image1.gif)

## <a name="monogame-and-xna"></a>MonoGame 및 XNA

MonoGame 라이브러리는 구문과 기능 모두에서 Microsoft의 XNA 라이브러리를 모방 하기 위한 것입니다.  모든 MonoGame 개체는 MonoGame에서 수정 없이 대부분의 XNA 코드를 사용할 수 있도록 하는 Microsoft Xna 네임 스페이스에 있습니다.

XNA에 익숙한 개발자는 이미 MonoGame의 구문에 익숙하고 MonoGame 사용에 대 한 추가 정보를 찾는 개발자는 기존 온라인 XNA 연습, API 설명서 및 토론을 참조할 수 있습니다.

## <a name="walkthrough-parts"></a>연습 파트

- [1 부-플랫폼 간 MonoGame 프로젝트 만들기](~/graphics-games/monogame/introduction/part1.md)
- [2 부-WalkingGame 구현](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>관련 링크

- [WalkingGame MonoGame 프로젝트 (샘플)](https://docs.microsoft.com/samples/xamarin/mobile-samples/walkinggamemg/)
- [NuGet의 MonoGame Android](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [NuGet의 MonoGame iOS](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API 설명서](http://www.monogame.net/documentation/?page=main)
