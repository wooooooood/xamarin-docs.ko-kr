---
title: MonoGame 통한 게임 개발 소개
description: 이 다중 파트 연습 MonoGame을 사용 하 여 간단한 2D 응용 프로그램을 만드는 방법을 보여 줍니다.  일반적인 게임 설명 입력 하 고, 그래픽 등의 프로그래밍 개념, 엔터티 및 물리학 게임입니다.
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 4ab98d59bc74672f9531f4dbd3c33a6270582612
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "33920808"
---
# <a name="introduction-to-game-development-with-monogame"></a>MonoGame 통한 게임 개발 소개

_이 다중 파트 연습 MonoGame을 사용 하 여 간단한 2D 응용 프로그램을 만드는 방법을 보여 줍니다.  일반적인 게임 설명 입력 하 고, 그래픽 등의 프로그래밍 개념, 엔터티 및 물리학 게임입니다._

이 문서에서는 플랫폼 간 게임을 만들기 위한 MonoGame API 기술을 설명 합니다. 플랫폼의 전체 목록을 참조 하세요. 합니다 [MonoGame 웹 사이트](http://www.monogame.net/)합니다. 이 자습서를 사용 하 여 C# 코드 샘플에 대 한 MonoGame는 완벽 하 게 작동 하지만 F# 도 있습니다.

MonoGame는 플랫폼 간, 하드웨어 가속 API 자산 가져오기에 대 한 그래픽, 오디오, 게임 상태 관리, 입력 및 콘텐츠 파이프라인을 제공 합니다. 대부분의 게임 엔진과 달리 MonoGame는 제공 하거나 모든 패턴 또는 프로젝트 구조를 적용 하지 않습니다.  즉, 개발자는 원하는 대로 해당 코드를 구성 하는, 하는 동안에 먼저 새 프로젝트를 시작할 때 약간의 설치 코드를 필요 함을 의미 합니다.

첫 번째 섹션에서는이 연습에서는 빈 프로젝트 설정에 중점을 둡니다. 마지막 섹션에서는 게임 논리 및 콘텐츠 – 가장는 될 크로스 플랫폼의 모든 쓰기를 설명 합니다.

이 연습의 끝부분으로는 만들었습니다 간단한 게임 플레이어가 터치 입력을 사용 하 여 애니메이션된 된 문자를 제어할 수 있는 곳입니다.  기술적으로 전체 게임 (win 또는 조건 손실 없기 때문) 되지는 않지만 다양 한 게임 개발 개념을 보여 줍니다 및 다양 한 유형의 게임에 대 한 기초로 사용할 수 있습니다. 

다음은이 연습의 결과입니다.

![샘플 게임 문자 다음 마우스의 애니메이션](images/image1.gif)

## <a name="monogame-and-xna"></a>Monogame 및 XNA

MonoGame 라이브러리 구문과 기능 모두 Microsoft XNA 라이브러리를 모방 하는 데 사용 됩니다.  MonoGame 개체를 모두 수정 하지 않을 MonoGame에 사용할 대부분의 XNA 코드 – Microsoft.Xna 네임 스페이스 아래에 존재 합니다. 

XNA를 사용 하 여 친숙 한 개발자는 이미 익숙할 MonoGame의 구문 및 MonoGame을 통한 작업에 대 한 자세한 개발자는 기존 온라인 XNA 연습, API 설명서 및 토론을 참조할 수 있습니다.


## <a name="walkthrough-parts"></a>연습 부분

- [파트 1-플랫폼 간 MonoGame 프로젝트 만들기](~/graphics-games/monogame/introduction/part1.md)
- [Part 2-Walkinggame 구현](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>관련 링크

- [WalkingGame MonoGame 프로젝트 (샘플)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
- [XNB 글꼴 iOS](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Content/fonts)
- [XNB 글꼴 Android](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Assets/Content/fonts)
- [NuGet의 MonoGame Android](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [NuGet에서 MonoGame iOS](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API 설명서](http://www.monogame.net/documentation/?page=main)
