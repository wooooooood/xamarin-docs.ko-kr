---
title: 명령줄 에뮬레이터
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 513b06749d1616e9fd10f04d22259810c0b4d265
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120725"
---
# <a name="command-line-emulator"></a>명령줄 에뮬레이터


## <a name="running-the-android-emulator-from-the-command-line"></a>명령줄에서 Android Emulator 실행

명령줄에서 Android Emulator 실행을 활성화하려면 Android SDK에서 제공하는 "에뮬레이터" 도구를 사용하면 됩니다. 이 도구는 OS X의 터미널 또는 Windows 컴퓨터의 명령 프롬프트에서 에뮬레이터를 실행하는 데 사용할 수 있습니다.

특정 Android Emulator를 시작하려면 Android SDK 위치의 tools 디렉터리(예: C:\android-sdk-windows\tools)에서 다음 명령을 실행합니다.

Windows

```cmd
emulator.exe -avd NameOfYourEmulator -partition-size 512
```

macOS

```bash
./emulator -avd NameOfYourEmulator -partition-size 512
```

이 파티션 크기가 필요한 이유는 기본적으로 에뮬레이터의 크기가 작으므로 에뮬레이터에 Xamarin.Android 플랫폼을 설치할 충분한 공간을 확보하도록 하기 위해서입니다.

추가 매개 변수에 대한 자세한 내용은 Android 사이트에서 확인할 수 있습니다. [http://developer.android.com/guide/developing/tools/emulator.html](http://developer.android.com/guide/developing/tools/emulator.html)
