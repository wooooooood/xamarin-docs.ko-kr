---
title: Android SDK Tools에 대한 변경 내용
description: Android SDK가 설치된 API 수준 및 AVD를 관리하는 방법에 대한 변경 내용입니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 4c559a76d7354fd957d065717ef14d91591d1be0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019504"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Android SDK Tools에 대한 변경 내용

_Android SDK가 설치된 API 수준 및 AVD를 관리하는 방법에 대한 변경 내용입니다._

## <a name="changes-to-android-sdk-tooling"></a>Android SDK 도구에 대한 변경 내용

Android용 SDK Tools의 최신 버전에서 Google은 새로운 CLI(명령줄 인터페이스) 도구를 위해 기존의 AVD 및 SDK 관리자를 제거했습니다. **android** 프로그램이 제거되었고 Mac용 Visual Studio 및 Xamarin용 Visual Studio Tools의 이전 버전 Google GUI(그래픽 사용자 인터페이스) 관리자는 더 이상 Android SDK Tools의 이전 버전 25.2.5에서 작동하지 않습니다. 예를 들어 명령줄을 통해 **android** 프로그램을 사용하려고 하면 다음과 같은 오류 메시지가 표시됩니다.

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

다음 섹션에서는 Android SDK 25.3.0 이상을 사용하여 Android SDK 및 Android 가상 디바이스를 관리하는 방법에 대해 설명합니다.

### <a name="ui-tools"></a>UI 도구

이제 Visual Studio 및 Mac용 Visual Studio에서 중단된 Google GUI 기반 관리자를 위해 Xamarin 교체를 제공합니다.

- Xamarin.Android 앱 개발에 필요한 Android SDK 도구, 플랫폼 및 기타 구성 요소를 다운로드하려면 레거시 Android SDK Manager 대신 [Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md)를 사용합니다.

- Android 가상 디바이스를 만들고 구성하려면 레거시 Google Emulator Manager 대신 [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)를 사용합니다.

이러한 도구는 대체하는 Google GUI 기반 관리자와 기능적으로 동일합니다.

### <a name="cli-tools"></a>CLI 도구

또는 CLI 도구를 사용하여 에뮬레이터 및 Android SDK를 관리하고 업데이트할 수 있습니다. 이제 다음 프로그램은 Android SDK 도구에 대한 명령줄 인터페이스를 구성합니다.

#### <a name="sdkmanager"></a>sdkmanager

**추가된 위치:** Android SDK Tools 25.2.3(2016년 11월) 이상.

Android SDK의 **tools/bin** 폴더에 **sdkmanager**라는 새 프로그램이 있습니다. 이 도구는 명령줄에서 Android SDK를 유지 관리하는 데 사용됩니다. 이 도구의 사용 방법에 대한 자세한 내용은 [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)를 참조하세요.

#### <a name="avdmanager"></a>avdmanager

**추가된 위치:** Android SDK Tools 25.3.0(2017년 3월) 이상.

Android SDK의 **tools/bin** 폴더에 **avdmanager**라는 새 프로그램이 있습니다. 이 도구는 Android Emulator의 AVD를 유지 관리하는 데 사용됩니다. 이 도구의 사용 방법에 대한 자세한 내용은 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)를 참조하세요.

### <a name="downgrading"></a>다운그레이드

[Android Developer 웹 사이트](https://developer.android.com/studio/index.html)에서 이전 버전의 Android SDK를 설치하여 **Android SDK Tools** 버전을 다운그레이드할 수 있습니다.

### <a name="using-the-old-gui"></a>이전 GUI 사용

**Android SDK Tools** 버전 **25.2.5** 이하에 있는 한 **도구** 폴더 내에서 **android** 프로그램을 실행하여 원본 GUI를 계속 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Android SDK 설정](~/android/get-started/installation/android-sdk.md)
- [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)
- [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)
- [SDK Tools 릴리스 정보(Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
