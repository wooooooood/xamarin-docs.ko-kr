---
title: MonoGame와 게임 개발 소개
description: 이 다중 파트 연습 MonoGame를 사용 하 여 간단한 2D 응용 프로그램을 만드는 방법을 보여 줍니다.  일반적인 게임에서는 엔터티 및 물리학 게임을 입력 하 고, 그래픽 등의 프로그래밍 개념입니다.
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 46cc3a7e3bb6c58e04626c9d2cc9437c16ba19f5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="introduction-to-game-development-with-monogame"></a>MonoGame와 게임 개발 소개

_이 다중 파트 연습 MonoGame를 사용 하 여 간단한 2D 응용 프로그램을 만드는 방법을 보여 줍니다.  일반적인 게임에서는 엔터티 및 물리학 게임을 입력 하 고, 그래픽 등의 프로그래밍 개념입니다._

이 문서에서는 플랫폼 간 게임을 만들기 위한 MonoGame API 기술을 설명 합니다. 플랫폼 목록은 전체 참조는 [MonoGame 웹 사이트](http://www.monogame.net/)합니다. 이 자습서는 사용 C# 코드 샘플도 MonoGame는 F #을 사용한 완벽 하 게 작동 합니다.

MonoGame는 플랫폼 간, 하드웨어 가속 자산 가져오기에 대 한 그래픽, 오디오, 게임 상태 관리, 입력 및 콘텐츠 파이프라인을 제공 하는 API입니다. 대부분의 게임 엔진 달리 MonoGame 제공 하거나 모든 패턴 또는 프로젝트 구조를 적용지 않습니다.  즉 개발자는 원하는 코드를 구성 하는 동안에 새 프로젝트를 처음 시작할 때 약간의 설치 프로그램 코드가 필요 함을 의미 합니다.

이 연습의 첫 번째 섹션 빈 프로젝트 설정에 중점을 둡니다. 마지막 섹션에서는 모든 우리의 게임 논리 및 콘텐츠-가장는 됩니다 플랫폼 크로스 작성을 설명 합니다.

이 연습에서는 끝날 때까지 됩니다 만들어져 간단한 게임 플레이어 터치식 입력 애니메이션된 캐릭터를 제어할 수 있습니다.  기술적으로 전체 게임 (있어 win 잊어버리거나 조건 없음) 하지 않더라도 수많은 게임 개발 개념을 보여 줍니다 및 다양 한 유형의 게임에 대 한 기반으로 사용할 수 있습니다. 

다음은이 연습의 결과입니다.

![마우스를 다음 샘플 게임 문자의 애니메이션](images/image1.gif)

## <a name="monogame-and-xna"></a>Monogame 및 XNA

MonoGame 라이브러리 구문 및 기능이 모두에서 Microsoft XNA 라이브러리를 모방 하는 데 사용 됩니다.  대부분 XNA 코드 수정 하지 않을 MonoGame에 사용할 수 있게 – Microsoft.Xna 네임 스페이스 아래의 모든 MonoGame 개체가 존재 합니다. 

XNA에 익숙한 개발자 이미 익숙할 것 MonoGame의 구문 및 MonoGame 사용에 대 한 추가 정보를 찾고 개발자는 기존 온라인 XNA 연습, API 설명서 및 토론을 참조할 수 있습니다.


## <a name="walkthrough-parts"></a>연습 부분

- [크로스 플랫폼 MonoGame 프로젝트 만들기-1 부](~/graphics-games/monogame/introduction/part1.md)
- [Part 2-는 WalkingGame 구현](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>관련 링크

- [WalkingGame MonoGame 프로젝트 (샘플)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
- [XNB 글꼴 iOS](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Content/fonts)
- [XNB 글꼴 Android](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Assets/Content/fonts)
- [NuGet에 MonoGame Android](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [NuGet에 MonoGame iOS](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API 설명서](http://www.monogame.net/documentation/?page=main)
