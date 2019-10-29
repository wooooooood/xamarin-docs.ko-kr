---
title: Android SDK Tools에 대한 변경 내용
description: Android SDK 설치 된 API 수준 및 AVDs를 관리 하는 방법에 대 한 변경 내용입니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 4c559a76d7354fd957d065717ef14d91591d1be0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019504"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Android SDK Tools에 대한 변경 내용

_Android SDK 설치 된 API 수준 및 AVDs를 관리 하는 방법에 대 한 변경 내용입니다._

## <a name="changes-to-android-sdk-tooling"></a>Android SDK 도구 변경

최신 버전의 Android 용 SDK Tools Google은 새 CLI (명령줄 인터페이스) 도구를 선호 하는 기존 AVD 및 SDK 관리자를 제거 했습니다. **Android** 프로그램이 제거 되었으며 Mac용 Visual Studio의 Google GUI (그래픽 사용자 인터페이스) 관리자와 Xamarin 용 Visual Studio Tools의 이전 버전은 더 이상 Android SDK Tools의 25.2.5 이전 버전으로 작동 하지 않습니다. 예를 들어 명령줄을 통해 **android** 프로그램을 사용 하려고 하면 다음과 같은 오류 메시지가 나타납니다.

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

다음 섹션에서는 25.3.0 이상 Android SDK를 사용 하 여 Android SDK 및 Android 가상 장치를 관리 하는 방법을 설명 합니다.

### <a name="ui-tools"></a>UI 도구

이제 Visual Studio 및 Mac용 Visual Studio에서 중단 된 Google GUI 기반 관리자에 대 한 Xamarin 교체를 제공 합니다.

- Xamarin Android 앱을 개발 하는 데 필요한 도구, 플랫폼 및 기타 구성 요소를 Android SDK 다운로드 하려면 레거시 Google SDK Manager 대신 [Xamarin Android SDK 관리자](~/android/get-started/installation/android-sdk.md) 를 사용 하세요.

- Android 가상 장치를 만들고 구성 하려면 레거시 Google 에뮬레이터 관리자 대신 [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md) 를 사용 합니다.

이러한 도구는 대체 하는 Google GUI 기반 관리자와 기능적으로 동일 합니다.

### <a name="cli-tools"></a>CLI 도구

또는 CLI 도구를 사용 하 여 에뮬레이터 및 Android SDK를 관리 하 고 업데이트할 수 있습니다. 다음 프로그램은 Android SDK tools에 대 한 명령줄 인터페이스를 구성 합니다.

#### <a name="sdkmanager"></a>sdkmanager

**추가 된 위치:** Android SDK Tools 25.2.3 (11 월, 2016) 이상입니다.

Android SDK의 **tools/bin** 폴더에 **sdkmanager** 라는 새 프로그램이 있습니다. 이 도구는 명령줄에서 Android SDK를 유지 하는 데 사용 됩니다. 이 도구를 사용 하는 방법에 대 한 자세한 내용은 [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)를 참조 하세요.

#### <a name="avdmanager"></a>avdmanager

**추가 된 위치:** Android SDK Tools 25.3.0 (3 월, 2017) 이상입니다.

Android SDK의 **tools/bin** 폴더에 **avdmanager** 라는 새 프로그램이 있습니다. 이 도구는 Android Emulator에 대 한 AVDs를 유지 관리 하는 데 사용 됩니다. 이 도구를 사용 하는 방법에 대 한 자세한 내용은 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)를 참조 하세요.

### <a name="downgrading"></a>다운

[Android Developer 웹 사이트](https://developer.android.com/studio/index.html)에서 이전 버전의 Android SDK를 설치 하 여 **Android SDK Tools** 버전을 다운 그레이드할 수 있습니다.

### <a name="using-the-old-gui"></a>이전 GUI 사용

**Android SDK Tools** 버전 **25.2.5** 이하인 경우 **도구** 폴더 내에서 **ANDROID** 프로그램을 실행 하 여 원본 GUI를 계속 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Android SDK 설정](~/android/get-started/installation/android-sdk.md)
- [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)
- [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)
- [SDK Tools 릴리스 정보(Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
