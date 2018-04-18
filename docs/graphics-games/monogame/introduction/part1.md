---
title: 크로스 플랫폼 MonoGame 만들기-1 부
description: 이 연습에서는 MonoGame를 사용 하 여 Android 및 iOS에 대 한 새 프로젝트를 만드는 방법을 보여 줍니다. 결과 각 플랫폼에 대 한 프로젝트와 플랫폼 간 공유 코드 프로젝트를 사용 하 여 Mac 솔루션에 대 한 Visual Studio입니다. 이 프로젝트에는 실행 때 빈 파란색 화면을 표시 됩니다.
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 1c859c5a8d8c5d8b0539d4158895e816d47d3d5e
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2018
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>크로스 플랫폼 MonoGame 만들기-1 부

_이 연습에서는 MonoGame를 사용 하 여 Android 및 iOS에 대 한 새 프로젝트를 만드는 방법을 보여 줍니다. 결과 각 플랫폼에 대 한 프로젝트와 플랫폼 간 공유 코드 프로젝트를 사용 하 여 Mac 솔루션에 대 한 Visual Studio입니다. 이 프로젝트에는 실행 때 빈 파란색 화면을 표시 됩니다._

MonoGame에는 코드를 재사용할 수의 큰 부분으로 플랫폼 간 게임을 개발할 수 있게 합니다. 이 연습에서는 중점적으로 플랫폼 간 코드에 대 한 공유 코드 프로젝트와 iOS 및 Android 용 프로젝트를 포함 하는 솔루션을 설정 합니다.

작업이 끝났을 때, 게임 업데이트 논리를 수행 하기 위한 적절 한 구조를 가진 프로젝트를 알아보고 게임을 초당 30 개 프레임에 논리를 그리기 하겠습니다. 모든 MonoGame 프로젝트에 대 한 기본 프로젝트로 사용할 수 있습니다. 프로젝트 진행 실행 하면 다음과 같이 표시 됩니다.

![빈 파란색 화면](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio-for-mac"></a>Mac 용 MonoGame Visual Studio에 추가

MonoGame에 추가할 수는 추가 기능으로 Visual Studio Mac.에 대 한 Mac에서 선택 **Mac 용 Visual Studio** > **추가 기능 관리자...**  . Windows에서는 도구 선택 * * * * > **추가 기능 관리자...**  . 선택 된 **갤러리** 탭을 확장 하 고는 **게임 개발** 범주를 선택 **MonoGame Addin**, 클릭 **설치**:

![Mac 확장 갤러리 MonoGame 선택에 대 한 visual Studio](part1-images/image2.png)

> [!IMPORTANT]
> **참고**: 경우는 **게임 개발** 섹션이 추가 기능 관리자에 표시 되지 않으면, 수동으로 다운로드 하 고 여기에서 최신 버전을 설치할 수 있습니다: http://www.monogame.net/downloads/합니다. 표시할 템플릿에 대 한 Mac 용 Visual Studio를 다시 시작 해야 합니다.

설치 되 면 메시지는 다음 섹션에 나와 있듯이, Mac 용 Visual Studio에서 MonoGame 템플릿이 표시 됩니다.

## <a name="creating-a-new-solution"></a>새 솔루션 만들기

Mac에 대 한 Visual Studio에서 **파일 > 새 솔루션**합니다. 에 **새 프로젝트** 대화 상자에서를 클릭 **기타**, 스크롤하여는 **일반** 섹션에서는 * * 유니버설 MonoGame 모바일 응용 프로그램 * * 옵션을 선택한 다음을 클릭 합니다.

![새 프로젝트 대화 상자 MonoGame 응용 프로그램 만들기](part1-images/image3.png)

WalkingGame 프로젝트 이름을 지정 하 고 만들기를 클릭 합니다.

![이름 및 위치를 선택 하는 새 프로젝트 대화 상자](part1-images/image4.png)

이제 iOS 또는 Android 프로젝트와 마찬가지로 프로젝트가 실행 됩니다. 프로젝트 수레 국화 파란색 배경을 표시를 실행 해야 합니다.

![비어 있는 파란색 앱 배경](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>Android 컴파일 오류 수정

MonoGame의 서식 파일의 현재 버전은 Android에서 몇 가지 구문 오류가 포함 됩니다. `Activity1.cs` 파일입니다. 이러한 문제를 해결 하려면 대체는 `OnCreate` 함수를 다음:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```

## <a name="summary"></a>요약

이 연습에서는 대상 Mac.에 대 한 Visual Studio를 사용 하 여 플랫폼 간 MonoGame 프로젝트를 만드는 방법 이 결과 빈 파란색 화면을입니다. 이 프로젝트는 모든 iOS 및 Android 게임에 대 한 시작 점으로 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [NuGet MonoGame iOS](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API 설명서](http://www.monogame.net/documentation/?page=main)
