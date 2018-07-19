---
title: Android SDK Tools에 대한 변경 내용
description: 설치 하는 API 수준 및 AVDs Android SDK가 관리 하는 방법을 변경 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2018
ms.openlocfilehash: 4e808736fd92fa40ecbf0c24938c0fedd7afcff9
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935453"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Android SDK Tools에 대한 변경 내용

_설치 하는 API 수준 및 AVDs Android SDK가 관리 하는 방법을 변경 합니다._

## <a name="changes-to-android-sdk-tooling"></a>Android SDK 도구에 대 한 변경

Android 용 SDK 도구의 최신 버전의 Google 새 CLI (명령줄 인터페이스) 도구를 위해 기존 AVD 및 SDK 관리자를 제거 했습니다. **android** 프로그램을 제거 하 고 Google GUI (그래픽 사용자 인터페이스) 관리자 Mac 및 이전 버전의 Xamarin 용 도구 Visual Studio 용 Visual Studio에서 더 이상 작동 이전 버전과 25.2.5 Android SDK 도구입니다. 예를 들어 사용 하려고는 **android** 프로그램 명령줄을 통해 다음과 같은 오류 메시지가 발생 합니다.

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

다음 섹션에서는 Android SDK 및 Android SDK 25.3.0를 사용 하 여 Android 가상 장치를 관리 하는 방법 및 이후 버전입니다.

### <a name="ui-tools"></a>UI 도구

Visual Studio 및 Mac 용 Visual Studio 이제 Xamarin 대체 함수는 지원 되지 않는 Google GUI 기반 관리자를 제공합니다.

-   Android SDK 도구, 플랫폼 및 Xamarin.Android 앱 개발을 위한 해야 하는 다른 구성 요소를 다운로드 하려면 사용 된 [Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md) 레거시 Google SDK Manager 대신 합니다.

-   사용 하 여 만들고 Android 가상 장치를 구성 하는 [Android 장치 관리자](~/android/get-started/installation/android-emulator/device-manager.md) 레거시 Google 에뮬레이터 관리자 대신 합니다.

이러한 도구는 기능적으로 Google GUI 기반 관리자를 대체 합니다.

### <a name="cli-tools"></a>CLI 도구

또는, 관리 및 에뮬레이터 및 Android SDK를 업데이트 하려면 CLI 도구를 사용할 수 있습니다. 다음 프로그램은 이제 Android SDK 도구에 대 한 명령줄 인터페이스를 구성합니다.

#### <a name="sdkmanager"></a>sdkmanager

**에 추가 되었습니다:** Android SDK 도구 25.2.3 (2016 년 11 월) 이상.

라는 새 프로그램은 **sdkmanager** 에 **도구/bin** Android SDK의 폴더입니다. 이 도구는 명령줄에서 Android SDK를 유지 하기 위해 사용 됩니다. 이 도구를 사용 하는 방법에 대 한 자세한 내용은 참조 [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)합니다.

#### <a name="avdmanager"></a>avdmanager

**에 추가 되었습니다:** Android SDK 도구 25.3.0 (2017 년 3 월) 이상.

라는 새 프로그램은 **avdmanager** 에 **도구/bin** Android SDK의 폴더입니다. 이 도구는 AVDs Android 에뮬레이터에 대 한 유지 하기 위해 사용 됩니다. 이 도구를 사용 하는 방법에 대 한 자세한 내용은 참조 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)합니다.

### <a name="downgrading"></a>다운 그레이드

다운 그레이드 하려면 프로그램 **Android SDK 도구** 에서 Android SDK의 이전 버전을 설치 하 여 버전은 [Android 개발자 웹 사이트](https://developer.android.com/studio/index.html)합니다.

### <a name="using-the-old-gui"></a>이전 GUI를 사용 하 여

실행 하 여 원래 GUI를 계속 사용할 수 있습니다는 **android** 내부의 프로그램 프로그램 **도구** 폴더에 있는 동안 계속 **Android SDK 도구** 버전 **25.2.5**  이하로 합니다.


## <a name="related-links"></a>관련 링크

- [Android SDK 설정](~/android/get-started/installation/android-sdk.md)
- [Android 장치 관리자](~/android/get-started/installation/android-emulator/device-manager.md)
- [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)
- [SDK Tools 릴리스 정보(Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
