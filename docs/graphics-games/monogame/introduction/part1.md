---
title: 1 부-플랫폼 간 MonoGame 만들기
description: 이 연습에서는 MonoGame를 사용 하 여 iOS 및 Android에 대 한 새 프로젝트를 만드는 방법을 보여 줍니다. 그 결과 플랫폼 간 공유 코드 프로젝트와 각 플랫폼에 대 한 하나의 프로젝트가 포함 된 Mac용 Visual Studio 솔루션이 있습니다. 이 프로젝트는 실행 시 빈 파란색 화면을 표시 합니다.
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: d72c428bb4b8c88365180c5c3c50b107eed2b21d
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68978452"
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>1 부-플랫폼 간 MonoGame 만들기

_이 연습에서는 MonoGame를 사용 하 여 iOS 및 Android에 대 한 새 프로젝트를 만드는 방법을 보여 줍니다. 그 결과 플랫폼 간 공유 코드 프로젝트와 각 플랫폼에 대 한 하나의 프로젝트가 포함 된 Mac용 Visual Studio 솔루션이 있습니다. 이 프로젝트는 실행 시 빈 파란색 화면을 표시 합니다._

MonoGame를 사용 하면 코드 재사용 부분이 많은 플랫폼 간 게임을 개발할 수 있습니다. 이 연습에서는 플랫폼 간 코드에 대 한 공유 코드 프로젝트 뿐만 아니라 iOS 및 Android에 대 한 프로젝트를 포함 하는 솔루션을 설정 하는 방법을 집중적으로 다룹니다.

완료 되 면 프로젝트는 초당 30 개 프레임에서 게임 업데이트 논리 및 게임 그리기 논리를 수행 하기 위한 적절 한 구조를 가집니다. MonoGame 프로젝트에 대 한 기본 프로젝트로 사용할 수 있습니다. 실행 시 프로젝트는 다음과 같이 표시 됩니다.

![빈 파란색 화면](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio"></a>Visual Studio에 MonoGame 추가

> [!IMPORTANT]
> MonoGame는 Visual Studio 2019 또는 Mac용 Visual Studio에서 기본적으로 설치 되지 않습니다.
>
> 에서 http://www.monogame.net/downloads/ 최신 버전을 수동으로 다운로드 하 여 설치한 후 설치 관리자를 실행 해야 합니다. 템플릿을 표시 하려면 Visual Studio를 다시 시작 해야 할 수 있습니다.
>
> 그러면 **게임 개발** 섹션이 **추가 기능 관리자**에 표시 됩니다.

Mac용 Visual Studio에 대 한 MonoGame 추가 기능을 사용 하도록 설정 하려면 **Mac용 Visual Studio** > **추가 기능 관리자 ...** 를 선택 합니다. Windows의 Visual Studio 2019의 경우 **도구** > **추가 기능 관리자 ...** 를 선택 합니다. **갤러리** 탭을 선택 하 고, **Game Development** 범주를 확장 하 고, **MonoGame Addin**을 선택 하 고, **설치 ...** 를 클릭 합니다.

![Mac용 Visual Studio 확장 갤러리 MonoGame 선택](part1-images/image2.png)

MonoGame 템플릿이 설치 되 면 다음 섹션에서 볼 수 있듯이 Mac용 Visual Studio에 표시 됩니다.

## <a name="creating-a-new-solution"></a>새 솔루션 만들기

Mac용 Visual Studio에서 **파일 > 새 솔루션**을 선택 합니다. **새 프로젝트** 대화 상자에서 **기타**를 클릭 하 고 **일반** 섹션으로 스크롤한 다음 **유니버설 MonoGame 모바일 응용 프로그램** 옵션을 선택 하 고 다음을 클릭 합니다.

![새 프로젝트 대화 상자 MonoGame 응용 프로그램 만들기](part1-images/image3.png)

프로젝트 이름을 WalkingGame로 선택 하 고 만들기를 클릭 합니다.

![이름 및 위치를 선택 하는 새 프로젝트 대화 상자](part1-images/image4.png)

이제 프로젝트는 다른 iOS 또는 Android 프로젝트와 마찬가지로 실행 됩니다. 수레 국화 청색 blue 배경을 표시 하는 프로젝트를 실행 해야 합니다.

![빈 파란색 앱 배경](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>Android 컴파일 오류 수정

MonoGame의 현재 버전에는 Android `Activity1.cs` 파일에 몇 가지 구문 오류가 포함 되어 있습니다. 이러한 문제를 해결 하려면 `OnCreate` 함수를 다음으로 바꿉니다.

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

이 연습에서는 Mac용 Visual Studio를 사용 하 여 플랫폼 간 MonoGame 프로젝트를 만드는 방법을 설명 했습니다. 이 결과는 빈 파란색 화면입니다. 이 프로젝트는 iOS 및 Android 게임의 시작 지점으로 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API 설명서](http://www.monogame.net/documentation/?page=main)
