---
title: '설치 및 설정 Wear OS onXamarin.Android '
description: 이 문서에서는 컴퓨터와 Android Wear 개발용 장치를 준비 하는 데 필요한 구성 세부 정보 및 설치 단계를 안내 합니다. 이 문서 끝으로 작동 하는 Mac 및/또는 Microsoft Visual Studio, Visual Studio에 통합 된 Xamarin.Android Wear 설치 해야 하 고 첫 번째 Xamarin.Android Wear 응용 프로그램을 빌드할 준비가 됩니다.
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 96fd6d32f37dd90422f05caf33cfda9a65683fd2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61308073"
---
# <a name="setup-and-installation"></a>설정 및 설치

_이 문서에서는 컴퓨터와 Android Wear 개발용 장치를 준비 하는 데 필요한 구성 세부 정보 및 설치 단계를 안내 합니다. 이 문서 끝으로 작동 하는 Mac 및/또는 Microsoft Visual Studio, Visual Studio에 통합 된 Xamarin.Android Wear 설치 해야 하 고 첫 번째 Xamarin.Android Wear 응용 프로그램을 빌드할 준비가 됩니다._

## <a name="requirements"></a>요구 사항

다음은 Xamarin 기반 Android Wear 앱을 만드는 데 필요 합니다.

-   **Visual Studio 또는 Mac 용 Visual Studio** &ndash; Visual Studio 2017 Community 이상 버전이 필요 합니다.

-   **Xamarin.Android** &ndash; 4.17 이상 Xamarin.Android를 설치 하 고 mac 용 Visual Studio 또는 Visual Studio를 사용 하 여 구성 해야

-   **Android SDK** -Android SDK (API 21) 5.0.1 나중에 설치 해야 Android SDK Manager를 통해 또는 합니다.

-   **Java Developer Kit** &ndash; Xamarin Android 개발 장치가 필요한 [JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 개발 API 수준 24 이상을 있다면 (JDK 1.8에서는 API 수준 24 미만도).

계속 사용할 수 있습니다 [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API 레벨 23에 맞게 개발 이하의 경우.

> [!IMPORTANT]
> Xamarin.Android는 JDK 9를 지원하지 않습니다.

## <a name="installation"></a>설치

Xamarin.Android를 설치한 후 Android Wear 앱 빌드 및 테스트 준비가 되도록 다음 단계를 수행 합니다. 

1.  필요한 Android SDK 및 도구를 설치 합니다.
2.  테스트 장치를 구성 합니다.
3.  첫 번째 Android Wear 앱을 만듭니다.

이러한 단계는 다음 섹션에 설명 되어 있습니다.


### <a name="install-android-sdk-and-tools"></a>Android SDK 및 도구 설치 

시작 합니다 **Android SDK Manager**: 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Visual Studio에서 Android SDK Manager를 시작 하는 방법](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Mac 용 Visual Studio에서 Android SDK Manager를 시작 하는 방법](installation-images/xs/sdk-menu.png)

-----


다음 Android SDK 및 도구를 설치 했는지 확인 합니다.

* Android SDK Tools v 24.0.0 이상 및
* Android 4.4W (API20) 또는
* Android 5.0.1(api (API21) 이상.

최신 SDK 및 도구가 설치 되어 있지 않은, 경우에 필요한 SDK 도구를 다운로드 *하 고* API 비트 (찾기 비트 스크롤해야 할 수도 있습니다 &ndash; API 선택 영역 아래에 표시 됩니다): 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Android 5.0.1(api 설정 스크린샷 예제에서는 SDK Manager 구성 요소](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Android 4.4 및 5.0.1 스크린샷 예제에서는 SDK Manager 구성 요소](installation-images/xs/sdk-select.png)

-----


## <a name="configuration"></a>구성

앱 테스트를 사용 하려면 먼저 Android Wear 에뮬레이터 또는 실제 Android Wear 장치를 구성 해야 합니다. 


### <a name="android-wear-emulator"></a>Android Wear 에뮬레이터

Android Wear 에뮬레이터를 사용 하려면 먼저 사용 하 여 Android Wear 가상 장치 AVD (Android)를 구성 해야 합니다 **Google 에뮬레이터 관리자**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Visual Studio에서 Android Emulator 관리자를 시작 하는 방법](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Mac 용 Visual Studio에서 Android Emulator 관리자를 시작 하는 방법](installation-images/xs/emulator-menu.png)

-----

Android Wear 에뮬레이터 설정에 대 한 자세한 내용은 참조 하세요. [에뮬레이터에서 Android Wear 디버그](~/android/wear/deploy-test/debug-on-emulator.md)합니다.


### <a name="android-wear-device"></a>Android Wear 장치

Android Wear Smartwatch 같은 Android Wear 장치를 사용 하는 경우에 에뮬레이터를 사용 하는 대신이 장치에서 앱을 디버그할 수 있습니다. Wear 장치를 사용 하 여 개발 하는 방법에 대 한 내용은 [Wear 장치에서 디버그](~/android/wear/deploy-test/debug-on-device.md)합니다.


## <a name="create-your-first-android-wear-app"></a>첫 번째 Android Wear 앱 만들기

수행 합니다 [안녕하세요, Wear](~/android/wear/get-started/hello-wear.md) 첫 번째 watch 앱을 작성 하기 위한 지침입니다.


## <a name="packaging-your-app"></a>앱 패키징

Android wear 응용 프로그램은 항상 도우미 Android 휴대폰 앱을 사용 하 여 배포 됩니다. 

Android Wear 응용 프로그램 기본 Android 응용 프로그램에 대 한 참조를 추가 하면 자동으로 Android Wear 프로젝트를 간주 됩니다 및 및 생성 하는 모든 필요한 XML 메타 데이터를 합니다. 또한 Google Play에 앱을 쉽게 제공할 수 있도록 패키지 및 버전 번호 일치 하는지 확인 합니다. 

Wear 앱 패키징 하는 방법에 대 한 자세한 내용은 참조 하세요 [패키징 작업](~/android/wear/deploy-test/packaging.md)합니다.


## <a name="related-links"></a>관련 링크

- [SkeletonWear (샘플)](https://developer.xamarin.com/samples/SkeletonWear/)
