---
title: '운영 체제를 설치 하 고 설치 하는 중입니다. Android '
description: 이 문서에서는 Android를 개발 하기 위해 컴퓨터 및 장치를 준비 하는 데 필요한 설치 단계 및 구성 세부 정보를 안내 합니다. 이 문서의 끝부분에는 Mac용 Visual Studio 및/또는 Microsoft Visual Studio에 통합 된 Xamarin Android가 설치 되어 있고, 첫 번째 Xamarin.ios 인 응용 프로그램 빌드를 시작할 준비가 되었습니다.
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 83ec214ae1838959355e99322ce5a809ead004fa
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028737"
---
# <a name="setup-and-installation"></a>설정 및 설치

_이 문서에서는 Android를 개발 하기 위해 컴퓨터 및 장치를 준비 하는 데 필요한 설치 단계 및 구성 세부 정보를 안내 합니다. 이 문서의 끝부분에는 Mac용 Visual Studio 및/또는 Microsoft Visual Studio에 통합 된 Xamarin Android가 설치 되어 있고, 첫 번째 Xamarin.ios 인 응용 프로그램 빌드를 시작할 준비가 되었습니다._

## <a name="requirements"></a>요구 사항

Xamarin 기반 Android 마모 앱을 만들려면 다음이 필요 합니다.

- Visual **studio 또는 Mac용 Visual Studio** &ndash; visual Studio 2017 Community 이상이 필요 합니다.

- Visual Studio 또는 Mac용 Visual Studio를 사용 하 여 **xamarin.ios &ndash; xamarin. android 4.17** 이상 버전을 설치 하 고 구성 해야 합니다.

- Android SDK Manager를 통해 **Android SDK** Android SDK 5.0.1 용 (API 21) 이상을 설치 해야 합니다.

- API 레벨 24 1.8 이상에 대해 개발 중인 경우에는 jdk 1.8를 사용 하 여 **Java 개발자 키트** &ndash; Xamarin Android를 개발 하는 경우 [jdk](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 는 24 이전 api 수준도 지원 합니다.

API 레벨 23 또는 이전 버전을 개발 하는 경우에는 [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) 을 계속 사용할 수 있습니다.

> [!IMPORTANT]
> Xamarin.Android는 JDK 9를 지원하지 않습니다.

## <a name="installation"></a>설치

Xamarin.ios를 설치한 후 다음 단계를 수행 하 여 Android 앱을 빌드하고 테스트할 준비가 완료 되도록 합니다. 

1. 필요한 Android SDK 및 도구를 설치 합니다.
2. 테스트 장치를 구성 합니다.
3. 첫 번째 Android 앱을 만듭니다.

이러한 단계는 다음 섹션에 설명 되어 있습니다.

### <a name="install-android-sdk-and-tools"></a>Android SDK 및 도구 설치 

**Android SDK 관리자**를 시작 합니다. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Visual Studio에서 Android SDK 관리자를 시작 하는 방법](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Mac용 Visual Studio에서 Android SDK 관리자를 시작 하는 방법](installation-images/xs/sdk-menu.png)

-----

다음 Android SDK 및 도구가 설치 되어 있는지 확인 합니다.

- Android SDK Tools v 24.0.0 이상
- Android 4.4 W (API20) 또는
- Android 5.0.1 용 (API21) 이상.

최신 SDK 및 도구를 설치 하지 않은 경우 필요한 SDK 도구 *및* api 비트를 다운로드 합니다 (아래에 api 선택이 표시 &ndash; 하려면 약간 스크롤해야 할 수 있음). 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Android 5.0.1 용 구성 요소를 사용 하도록 설정 하는 예제 SDK Manager 스크린샷](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Android 4.4 및 5.0.1 용 구성 요소를 사용 하도록 설정 하는 예제 SDK Manager 스크린샷](installation-images/xs/sdk-select.png)

-----

## <a name="configuration"></a>Configuration

앱 테스트를 사용 하려면 먼저 Android 마모 에뮬레이터 또는 실제 Android 장치를 구성 해야 합니다. 

### <a name="android-wear-emulator"></a>Android 마모 에뮬레이터

Android 마모 에뮬레이터를 사용 하려면 먼저 **Google Emulator Manager**를 사용 하 여 android 용 avd (Android 가상 장치)를 구성 해야 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Visual Studio에서 Android Emulator 관리자를 시작 하는 방법](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Mac용 Visual Studio에서 Android Emulator 관리자를 시작 하는 방법](installation-images/xs/emulator-menu.png)

-----

Android 마모 에뮬레이터를 설정 하는 방법에 대 한 자세한 내용은 [에뮬레이터에서 Android 마모 디버그](~/android/wear/deploy-test/debug-on-emulator.md)를 참조 하세요.

### <a name="android-wear-device"></a>Android 장치

Android 마모 Smartwatch 같은 Android 장치를 사용 하는 경우 에뮬레이터를 사용 하는 대신이 장치에서 앱을 디버그할 수 있습니다. 마모 된 장치를 사용 하 여 개발 하는 방법에 대 한 자세한 내용은 [마모 된 장치에서 디버그](~/android/wear/deploy-test/debug-on-device.md)를 참조 하세요.

## <a name="create-your-first-android-wear-app"></a>첫 번째 Android 앱 만들기

[Hello, 마모](~/android/wear/get-started/hello-wear.md) 된 지침에 따라 첫 번째 시청 앱을 빌드 하세요.

## <a name="packaging-your-app"></a>앱 패키징

Android 마모 응용 프로그램은 항상 자매 Android 휴대폰 앱과 함께 배포 됩니다. 

Android 마모 응용 프로그램을 주 Android 응용 프로그램에 대 한 참조로 추가 하면 자동으로 Android 마모 프로젝트로 간주 되 고 필요한 모든 XML 및 메타 데이터가 생성 됩니다. 또한 패키지와 버전 번호가 일치 하는지 확인 하 여 앱을 쉽게 Google Play 수 있습니다. 

패키지 사용 앱에 대 한 자세한 내용은 [패키징 작업](~/android/wear/deploy-test/packaging.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [SkeletonWear (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-skeletonwear)
