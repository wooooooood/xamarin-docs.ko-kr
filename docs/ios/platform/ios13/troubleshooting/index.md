---
title: Xamarin 및 13 iOS 문제 해결
description: 이 섹션에서는 iOS 13 관련 된 Xamarin 기능에 대 한 문제 해결 팁을 포함 합니다.
ms.prod: xamarin
ms.assetid: 00DE8C33-1407-45C0-A0C7-32AF1E490034
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/12/2019
ms.openlocfilehash: 99fd7e41e1521c6a9254d286bf989281658ccf24
ms.sourcegitcommit: 85c45dc28ab3625321c271804768d8e4fce62faf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67039790"
---
# <a name="troubleshooting-tips-for-ios-13-and-xamarinios"></a>IOS 13 및 Xamarin.iOS 한 문제 해결 팁

![미리 보기 기능](~/media/shared/preview.png)

## <a name="updating-to-xcode-11-stops-the-simulator-from-launching"></a>시뮬레이터에서 시작을 중지 Xcode 11로 업데이트 하는 중

업데이트 한 후 **Xcode 11 beta 1** 시뮬레이터 시작 될 때마다 다음 예외가 throw 되 고 시뮬레이터를 시작 하지 않습니다. 이 모든 시뮬레이터를 사용 하 여 발생합니다.

### <a name="exception"></a>예외

`Foundation.ObjCException: NSInvalidArgumentException: -[SimDevice registerNotificationHandler:]: unrecognized selector sent to instance 0x7ffbf5d1e110`

### <a name="workaround"></a>해결 방법

될 때까지 [해결](https://github.com/xamarin/xamarin-macios/issues/6216), 개발자 작업을 계속할 수 있도록 이전 시뮬레이터 프레임 워크를 다시 설치 하려면 다음 단계를 따라 수 있습니다.

> [!NOTE]
> 이러한 단계는 두 개의 Xcode 응용 프로그램을 있다고 가정 합니다.
> - **Xcode11 beta1.app** – mac 용 Visual Studio 및 시뮬레이터를 사용 하 여 작동 하지 않는 베타 버전
> - **Xcode102.app** -안정적인 버전의 Xcode 10입니다. 호출 될 수도 여러분 **Xcode.app**합니다.
>
> 구성에 따라 아래 명령줄 예제를 변경 합니다.

1. Xcode 11 xcode-select-를 통해 선택 했는지 확인 합니다.

   `sudo xcode-select -s /Applications/Xcode11-beta1.app/Contents/Developer/`

2. 설치 실행에 필요한 경우, 처음에 대 한 도구입니다.

    `/Applications/Xcode11-beta1.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

3. 다음 프레임 워크를 제거 합니다.

    `sudo rm -Rf  /Library/Developer/PrivateFrameworks/CoreSimulator.framework/Versions/*`

4. 이전 Xcode 버전으로 전환

   `sudo xcode-select -s /Applications/Xcode102.app/Contents/Developer/`

5. 에 대 한 첫 번째 시작 도구를 다시 실행 합니다 _이전_ 방금 선택한 Xcode 버전

   `/Applications/Xcode102.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

다음이 단계를 따라 iOS 시뮬레이터를 다시 사용 하려면 해야 합니다.