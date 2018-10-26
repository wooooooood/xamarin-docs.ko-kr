---
title: 플랫폼 간 MonoGame 만들기 – 1 부
description: 이 연습에서는 iOS 및 Android MonoGame을 사용 하 여 새 프로젝트를 만드는 방법을 보여 줍니다. 결과 각 플랫폼에 대해 하나의 프로젝트 뿐만 아니라 플랫폼 간 공유 코드 프로젝트를 솔루션 Mac 용 Visual Studio. 이 프로젝트에는 실행 하는 경우 빈 블루 스크린이 표시 됩니다.
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 82b1408cafedf98a8619e8e039ba00b332f74516
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "33921991"
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>플랫폼 간 MonoGame 만들기 – 1 부

_이 연습에서는 iOS 및 Android MonoGame을 사용 하 여 새 프로젝트를 만드는 방법을 보여 줍니다. 결과 각 플랫폼에 대해 하나의 프로젝트 뿐만 아니라 플랫폼 간 공유 코드 프로젝트를 솔루션 Mac 용 Visual Studio. 이 프로젝트에는 실행 하는 경우 빈 블루 스크린이 표시 됩니다._

MonoGame 코드 재사용의 많은 부분을 사용 하 여 플랫폼 간 게임 개발을 수 있습니다. 이 연습에서는 중점적으로 플랫폼 간 코드 공유 코드 프로젝트 뿐만 아니라 iOS 및 Android 용 프로젝트를 포함 하는 솔루션을 설정 합니다.

작업을 완료 했습니다에서는 게임 업데이트 논리를 수행 하기 위한 적절 한 구조가 포함 된 프로젝트를 하 고 초당 30 프레임이에 논리를 그리기 게임입니다. MonoGame 프로젝트에 대 한 기본 프로젝트로 사용 수 있습니다. 프로젝트는 실행 하는 경우이 같이 표시 됩니다.

![빈 블루 스크린](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio-for-mac"></a>MonoGame을 Mac 용 Visual Studio에 추가

MonoGame에 추가할 수 있습니다 추가 기능으로 Visual Studio for mac Mac에서 선택 **Mac 용 Visual Studio** > **추가 기능 관리자...**  . Windows에 도구 선택 * *에서 * * > **추가 기능 관리자...**  . 선택 합니다 **갤러리** 탭을 확장 합니다 **게임 개발** 범주를 선택 **MonoGame 추가 기능**, 클릭 **설치**:

![MonoGame을 선택 하면 Mac 확장 갤러리에 대 한 visual Studio](part1-images/image2.png)

> [!IMPORTANT]
> **참고**: 경우 합니다 **게임 개발** 섹션은 추가 기능 관리자에 나타나지 않으면, 수동으로 다운로드 하 고 여기에서 최신 버전을 설치할 수 있습니다: http://www.monogame.net/downloads/합니다. 표시할 템플릿에 대 한 Mac 용 Visual Studio를 다시 시작 해야 합니다.

설치 되 면 메시지는 다음 섹션에서 보듯이 Mac 용 Visual Studio에서 MonoGame 템플릿 표시 됩니다.

## <a name="creating-a-new-solution"></a>새 솔루션 만들기

Mac 선택에 대 한 Visual Studio에서 **파일 > 새 솔루션**합니다. 에 **새 프로젝트** 대화 상자에서 클릭 **기타**, 스크롤하여를 **일반** 섹션에서를 * * 유니버설 MonoGame 모바일 응용 프로그램 * * 옵션을 선택한 다음 클릭 합니다.

![MonoGame 응용 프로그램을 만드는 새 프로젝트 대화 상자](part1-images/image3.png)

WalkingGame 프로젝트 이름을 지정 하 고 만들기를 클릭 합니다.

![이름 및 위치를 선택 하는 새 프로젝트 대화 상자](part1-images/image4.png)

이제 프로젝트는 iOS 또는 Android 프로젝트와 마찬가지로 실행 됩니다. 프로젝트 수레 국화 파란색 배경을 표시를 실행 해야 합니다.

![비어 있는 파란색 앱 배경](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>Android 컴파일 오류를 수정합니다.

MonoGame의 템플릿의 현재 버전과 Android에 대 한 몇 가지 구문 오류 `Activity1.cs` 파일입니다. 이러한 문제를 해결 하려면 대체는 `OnCreate` 다음을 사용 하 여 함수:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```

## <a name="summary"></a>요약

이 연습에서는 mac 용 Visual Studio를 사용 하 여 플랫폼 간 MonoGame 프로젝트를 만드는 방법 설명 이 결과 빈 블루 스크린이 경우 이 프로젝트는 모든 iOS 및 Android 게임에 대 한 시작 점으로 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API 설명서](http://www.monogame.net/documentation/?page=main)
