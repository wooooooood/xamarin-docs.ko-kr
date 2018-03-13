---
title: "설정 및 설치"
description: "이 문서는 설치 단계 및 구성 세부 정보를 쓰는 유형 Android 개발에 대 한 장치 및 컴퓨터를 준비 하는 데 필요한 안내 합니다. 이 문서의 뒷부분에서 작동 중인 Xamarin.Android 마모 설치 Mac 및/또는 Microsoft Visual Studio에 대 한 Visual Studio에 통합 해야 합니다. 첫 번째 Xamarin.Android 마모 응용 프로그램 작성을 시작할 준비가 수 고 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 012f563dcdaa70e33d641a4d8fb52df1622c260a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="setup-and-installation"></a>설정 및 설치

_이 문서는 설치 단계 및 구성 세부 정보를 쓰는 유형 Android 개발에 대 한 장치 및 컴퓨터를 준비 하는 데 필요한 안내 합니다. 이 문서의 뒷부분에서 작동 중인 Xamarin.Android 마모 설치 Mac 및/또는 Microsoft Visual Studio에 대 한 Visual Studio에 통합 해야 합니다. 첫 번째 Xamarin.Android 마모 응용 프로그램 작성을 시작할 준비가 수 고 합니다._

## <a name="requirements"></a>요구 사항

다음은 Android 착용 Xamarin 기반 앱을 만들 필요 합니다.

-   **Visual Studio 또는 Mac 용 Visual Studio** &ndash; 하는 경우 Visual Studio, Visual Studio 2015 Professional을 사용 하거나 이상 버전이 필요 합니다.

-   **Xamarin.Android** &ndash; Xamarin.Android 4.17 이상을 설치 하 고 Mac.에 대 한 Visual Studio 또는 Visual Studio를 사용 하 여 구성 해야

-   **Android SDK** -Android SDK (API 21) 5.0.1 나중에 설치 되어 있어야 Android SDK Manager를 통해 합니다.

-   **Java 개발자 키트** &ndash; Xamarin Android 개발 시 필요한 [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 24 API 수준에 대 한 개발 또는 큰 경우 (JDK 1.8 API에서는 수준을 지원 24 보다 이전 버전).

계속 사용할 수 있습니다 [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API 수준 23에 맞게 개발 이하의 경우.

> [!IMPORTANT]
> Xamarin.Android는 JDK 9를 지원 하지 않습니다.

## <a name="installation"></a>설치

Xamarin.Android를 설치한 후 빌드 및 쓰는 유형 Android 앱을 테스트 준비가 되도록 다음 단계를 수행: 

1.  필요한 Android SDK 및 도구를 설치 합니다.
2.  테스트 장치를 구성 합니다.
3.  첫 Android 착용 응용 프로그램을 만듭니다.

이러한 단계는 다음 섹션에 설명 되어 있습니다.


### <a name="install-android-sdk-and-tools"></a>Android SDK 및 도구 설치 

시작 된 **Android SDK Manager**: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Visual Studio에서 Android SDK Manager를 시작 하는 방법](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Mac 용 Visual Studio에서 Android SDK Manager를 시작 하는 방법](installation-images/xs/sdk-menu.png)

-----


다음 Android SDK 및 도구가 설치 되어 있는지 확인 합니다.

* Android SDK 도구 v 24.0.0 또는 그 이상으로 및
* Android 4.4W (API20) 또는
* Android 5.0.1 (API21) 이상.

최신 SDK 및 도구가 설치 된 없는 경우 필요한 SDK 도구를 다운로드 *및* API 비트 (을 비트에 스크롤해야 할 수도 &ndash; API 영역이 아래 표시): 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android 5.0.1 사용의 예에서는 SDK Manager 스크린 샷 구성 요소](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Android 4.4 및 5.0.1 사용의 예에서는 SDK Manager 스크린 샷 구성 요소](installation-images/xs/sdk-select.png)

-----


## <a name="configuration"></a>구성

사용 하려면 먼저 응용 프로그램을 테스트, 쓰는 유형 Android 에뮬레이터 또는 실제 쓰는 유형 Android 장치를 구성 해야 합니다. 


### <a name="android-wear-emulator"></a>Android 마모 에뮬레이터

사용 하 여 쓰는 유형 Android 가상 장치 AVD (Android)를 구성 해야 쓰는 유형 Android 에뮬레이터를 사용 하려면 먼저는 **Google 에뮬레이터 관리자**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Visual Studio에서 Android 에뮬레이터 관리자를 시작 하는 방법](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Mac 용 Visual Studio에서 Android 에뮬레이터 관리자를 시작 하는 방법](installation-images/xs/emulator-menu.png)

-----

쓰는 유형 Android 에뮬레이터를 설정 하는 방법에 대 한 자세한 내용은 참조 [에뮬레이터 Android 착용 디버그](~/android/wear/deploy-test/debug-on-emulator.md)합니다.


### <a name="android-wear-device"></a>Android 마모 장치

Android 착용 Smartwatch 같은 쓰는 유형 Android 장치를 설정한 경우 에뮬레이터를 사용 하는 대신이 장치에서 앱을 디버깅할 수 있습니다. 마모 장치를 사용 하 여 개발 하는 방법에 대 한 정보를 참조 하십시오. [착용 장치에서 디버깅](~/android/wear/deploy-test/debug-on-device.md)합니다.


## <a name="create-your-first-android-wear-app"></a>첫 번째 Android 마모 앱 만들기

에 따라는 [안녕하세요, 마모](~/android/wear/get-started/hello-wear.md) 첫 조사식 응용 프로그램을 작성 하기 위한 지침입니다.


## <a name="packaging-your-app"></a>앱 패키징

Android 마모 응용 프로그램은 항상 도우미 Android 휴대폰 앱으로 배포 됩니다. 

기본 Android 응용 프로그램에 대 한 참조로 쓰는 유형 Android 응용 프로그램을 추가 하면 자동 Android 착용 프로젝트도 간주 됩니다를 사용자에 대 한 XML 및 메타 데이터에 대 한 모든 필요한을 발생 합니다. 또한, Google Play에 앱을 쉽게 제공할 수 있도록 패키지 및 버전 번호와 일치 하는지 확인 합니다. 

마모 앱 패키징 하는 방법에 대 한 자세한 참조 [패키징 작업](~/android/wear/deploy-test/packaging.md)합니다.


## <a name="related-links"></a>관련 링크

- [SkeletonWear (샘플)](https://developer.xamarin.com/samples/SkeletonWear/)
