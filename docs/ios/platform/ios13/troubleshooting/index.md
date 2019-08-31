---
title: Xamarin 및 iOS 13 문제 해결
description: 이 섹션에는 iOS 13과 관련 된 Xamarin 기능에 대 한 문제 해결 팁이 포함 되어 있습니다.
ms.prod: xamarin
ms.assetid: 00DE8C33-1407-45C0-A0C7-32AF1E490034
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/12/2019
ms.openlocfilehash: d2a2146a0b7345475e2eb93d52fb02387c833224
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200040"
---
# <a name="troubleshooting-tips-for-ios-13-and-xamarinios"></a>IOS 13 및 Xamarin.ios에 대 한 문제 해결 팁

![미리 보기 기능](~/media/shared/preview.png)

## <a name="updating-to-xcode-11-stops-the-simulator-from-launching"></a>Xcode 11로 업데이트 하면 시뮬레이터 시작이 중지 됩니다.

시뮬레이터를 시작할 때마다 **Xcode 11 beta 1** 로 업데이트 한 후에는 다음 예외가 throw 되 고 시뮬레이터가 시작 되지 않습니다. 이는 모든 시뮬레이터에서 발생 합니다.

### <a name="exception"></a>예외

`Foundation.ObjCException: NSInvalidArgumentException: -[SimDevice registerNotificationHandler:]: unrecognized selector sent to instance 0x7ffbf5d1e110`

### <a name="workaround"></a>해결 방법

[수정](https://github.com/xamarin/xamarin-macios/issues/6216)이 있을 때까지 다음 단계를 수행 하 여 개발자가 계속 작업할 수 있도록 이전 시뮬레이터 프레임 워크를 다시 설치할 수 있습니다.

> [!NOTE]
> 이러한 단계에서는 두 개의 Xcode 응용 프로그램이 있다고 가정 합니다.
> - **Xcode11-beta1** – 시뮬레이터 및 Mac용 Visual Studio에서 작동 하지 않는 베타 버전입니다.
> - **Xcode102** – 안정적인 버전의 Xcode 10입니다. **Xcode**을 호출할 수도 있습니다.
>
> 아래 명령줄 예제를 구성에 적합 하 게 변경 합니다.

1. Xcode을 통해 Xcode 11을 선택 했는지 확인 합니다.

   `sudo xcode-select -s /Applications/Xcode11-beta1.app/Contents/Developer/`

2. 필요한 경우 설치 도구를 처음으로 실행 합니다.

    `/Applications/Xcode11-beta1.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

3. 다음 프레임 워크를 제거 합니다.

    `sudo rm -Rf  /Library/Developer/PrivateFrameworks/CoreSimulator.framework/Versions/*`

4. 이전 Xcode 버전으로 다시 전환 합니다.

   `sudo xcode-select -s /Applications/Xcode102.app/Contents/Developer/`

5. 방금 선택한 _이전_ Xcode 버전에 대 한 첫 번째 시작 도구를 다시 실행 합니다.

   `/Applications/Xcode102.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

이러한 단계를 수행 하면 iOS 시뮬레이터를 다시 사용할 수 있습니다.
