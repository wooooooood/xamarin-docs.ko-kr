---
title: "다중 플랫폼 CocosSharp 프로젝트 만들기"
description: "이 연습에는 다중 플랫폼 CocosSharp 솔루션을 새로 만드는 방법을 보여 줍니다. 이 연습의 결과 세 개의 프로젝트를 포함 하는 Mac 솔루션에 대 한 Visual Studio: 한 이식 가능한 클래스 라이브러리 프로젝트, Android 관련 프로젝트 하나 및 iOS 관련 프로젝트 하 나와 있습니다. 결과 프로젝트에는 실행 때 빈 검은색 화면이 표시 됩니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 37C97693-B0A8-4064-97B6-A6FAB5BA4FB7
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 2906035ce9bd44d111b89ccfe7443896775315b7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-multi-platform-cocossharp-project"></a>다중 플랫폼 CocosSharp 프로젝트 만들기

_이 연습에는 다중 플랫폼 CocosSharp 솔루션을 새로 만드는 방법을 보여 줍니다. 이 연습의 결과 세 개의 프로젝트를 포함 하는 Mac 솔루션에 대 한 Visual Studio: 한 이식 가능한 클래스 라이브러리 프로젝트, Android 관련 프로젝트 하나 및 iOS 관련 프로젝트 하 나와 있습니다. 결과 프로젝트에는 실행 때 빈 검은색 화면이 표시 됩니다._

CocosSharp 2D 게임 엔진 코드와 콘텐츠를 여러 플랫폼 간에 공유할 수 있습니다. 이 연습에는 iOS 및 Android 개발을 지원할 수 있는 프로젝트를 만드는 방법을 보여 줍니다. 특히,이 연습에서는 다음 항목을 설명 합니다.

 - CocosSharp 설치
 - 새 솔루션 만들기
 - `LoadGame` 메서드

# <a name="installing-cocossharp"></a>CocosSharp 설치

첫째, 추가 CocosSharp Visual Studio로 Mac.에 대 한 Mac에서 실행 하는 경우 선택 **Mac 용 Visual Studio** > **추가 기능 관리자...**  . Windows에서 실행 하는 경우 선택 **도구** > **추가 기능 관리자...**  . 클릭는 **갤러리** 탭을 확장 하 고는 **CocosSharp 항목**선택, **CocosSharp 프로젝트 템플릿**, 마지막으로 클릭 하 고 **설치 중...**  .

![CocosSharp 추가 기능](part1-images/xamarinstudioaddinsmac.png "")

# <a name="creating-a-new-solution"></a>새 솔루션 만들기

CocosSharp를 설치 하는 솔루션을 만들겠습니다. Mac 용 Visual Studio에서 선택 **파일** > **새로** > **솔루션 중...** . 선택 된 **앱** 옵션에서 **플랫폼 간** 섹션에서 **CocosSharp 빈 프로젝트**, 클릭 하 고 **다음**:

![](part1-images/image1.png "크로스 플랫폼 섹션에서 응용 프로그램 옵션 선택 CocosSharp 빈 프로젝트를 선택 하 고 클릭")

이름을 입력 **BouncingGame** 에 대 한는 **프로젝트 이름**, 클릭 **만들기**:

![](part1-images/image2.png "프로젝트 이름에 대 한 BouncingGame 이름을 입력 한 다음 만들기를 클릭 합니다.")

프로젝트를 만든 하 고 Mac 용 Visual Studio에서는 컴파일 및 실행할 수는 회색 배경이 표시: 

![](part1-images/image3.png "후 프로젝트를 만든 후 Visual Studio, Mac 용 컴파일 및이 회색 배경이 표시 되도록 실행 하는")


# <a name="loadgame-method"></a>LoadGame 메서드

기본 CocosSharp 프로젝트를 설정 하기 위한 iOS 및 Android에만 클래스를 포함 한 `CCGameView`, CocosSharp를 시작 하는 데 사용 되는 합니다. `CCGameView` 인스턴스가 플랫폼 특정 방식으로 생성 되: iOS 프로젝트를 만듭니다는 `CCGameView` 에 `Main.storyboard` 파일을 Android에서 만들어지는 동안는 `CCGameView` 에 `Main.axml` 파일입니다. 각 플랫폼에서에서 CCGameView 인스턴스를 사용 하는 `LoadGame` 메서드로 몇 가지 기본 설치를 수행 합니다. 이 코드를 수정 하지 않습니다 것, 경우에 몇 가지 중요 한 세부 정보를 살펴보면을 보겠습니다.

 - 코드 집합은 `gameView.DesignResolution` 768 하 여 1024 자가 하입니다. 이 표준화 현재 장치 가로 세로 비율, 실제 해상도 또는 방향에 관계 없이 장치에서 위치를 지정 합니다. 
 - 코드 몇 가지 검색 경로 추가합니다. 검색 경로는 콘텐츠 디렉터리 접두사 없이 로드 되도록 할 수도 있습니다. 예를 들어 이후는 `"Sounds"` 경로 검색 경로 다음 디렉터리에 파일이 추가 됩니다 `"Content/Sounds/mysound.xnb"` 로 로드할 수 단순히 `"mysound.xnb"`합니다. 검색 경로 비슷합니다 `using` 문을 – 코드에서 코드를 줄일 수 있습니다 하지만 모호성을 초래할 수 있습니다.
 - 코드를 생성 한 `GameLayer` 인스턴스. `GameLayer` 이 `CCLayer`-위치를 추가할 예정이 게임 논리의 모든 클래스를 상속 합니다. 더 큰 게임 여러 필요할 수 있습니다 `CCLayer` 인스턴스 또는 짝수 배수가 `CCScene` 인스턴스 (여러 개 포함할 수 있는 `CCLayer` 인스턴스)를 하는 단일 하지만 `CCLayer` 이 게임에 대 한 합니다.

#  <a name="summary"></a>요약

이 연습에서는 대상 Mac.에 대 한 Visual Studio를 사용 하 여 플랫폼 간 CocosSharp 프로젝트를 만드는 방법 결과는 모든 게임 프로젝트에 대 한 시작 점으로 사용할 수 있는 빈 화면입니다.