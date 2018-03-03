---
title: "Android SDK Tools에 대한 변경 내용"
description: "설치 하는 API 수준 및 AVDs Android SDK가 관리 하는 방법을 변경 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 69e9f08870a01c056951700978d07277af5edfa8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="changes-to-the-android-sdk-tooling"></a>Android SDK Tools에 대한 변경 내용

_설치 하는 API 수준 및 AVDs Android SDK가 관리 하는 방법을 변경 합니다._

## <a name="changes-to--android-sdk-tooling"></a>Android SDK 도구에 대 한 변경

Google Android 용 SDK 도구의 최신 버전에서 새로운 in favour of 기존 AVD 및 SDK 관리자가 제거 _명령줄 인터페이스_ (CLI) 도구입니다. 전자 **android** 프로그램 제거 되었습니다 및 Android SDK 도구 버전을 지 나 GUI (그래픽 사용자 인터페이스) 관리자 Mac 및 이전 버전의 Xamarin for Visual Studio 용 Visual Studio에서 작동 하지 것입니다.


![Visual Studio에서 android IDE 메뉴](sdk-cli-tooling-changes-images/android-ide-menu.png)

사용 하려고 하면는 **android** 프로그램 명령줄을 통해 다음과 같은 오류 메시지가 발생 합니다.

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

따라서 CLI 도구를 사용 하 여 관리 하 고 에뮬레이터 및 Android SDK를 업데이트 해야 합니다.

### <a name="cli-tools"></a>CLI 도구

다음 프로그램은 이제 Android SDK 도구에 대 한 명령줄 인터페이스를 구성합니다.

#### <a name="sdkmanager"></a>sdkmanager

**에 추가 되었습니다:** Android SDK 도구 25.2.3 (2016 년 11 월) 이상.

라는 새 프로그램은 **sdkmanager** 에 **도구/bin** Android SDK의 폴더입니다. 이 도구는 명령줄에서 Android SDK를 유지 하기 위해 사용 됩니다. 이 도구를 사용 하는 방법에 대 한 자세한 내용은 참조 [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)합니다.

#### <a name="avdmanager"></a>avdmanager

**에 추가 되었습니다:** Android SDK 도구 25.3.0 (2017 년 3 월) 이상.

라는 새 프로그램은 **avdmanager** 에 **도구/bin** Android SDK의 폴더입니다. 이 도구는 AVD의 Google Android 에뮬레이터에 대 한 유지 관리 하는 데 사용 됩니다. 이 도구를 사용 하는 방법에 대 한 자세한 내용은 참조 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)합니다.

### <a name="downgrading"></a>다운 그레이드

다운 그레이드 하려면 프로그램 **Android SDK 도구** 에서 Android SDK의 이전 버전을 설치 하 여 버전은 [Android 개발자 웹 사이트](https://developer.android.com/studio/index.html)합니다.

### <a name="using-the-old-gui"></a>이전 GUI를 사용 하 여

실행 하 여 원래 GUI를 계속 사용할 수 있습니다는 **android** 내부의 프로그램 프로그램 **도구** 폴더에 있는 동안 계속 **Android SDK 도구** 버전 **25.2.5**  이하로 합니다.


## <a name="related-links"></a>관련 링크

- [Android SDK 설정](~/android/get-started/installation/android-sdk.md)
- [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)
- [SDK 도구 릴리스 정보 (Google)](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
