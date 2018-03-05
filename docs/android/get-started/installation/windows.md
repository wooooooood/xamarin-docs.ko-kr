---
title: "Windows 설치"
description: "이 가이드에서는 Windows에서 Visual Studio용 Xamarin.Android를 설치하는 단계를 설명하고, 첫 번째 Xamarin.Android 응용 프로그램을 빌드하는 Xamarin.Android를 구성하는 방법을 설명합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2BE4D5AD-D468-B177-8F96-837D084E7DE1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 10cfb790fca30b67faaa8f7720a358ec34c25663
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="windows-installation"></a>Windows 설치

_이 가이드에서는 Windows에서 Visual Studio용 Xamarin.Android를 설치하는 단계를 설명하고, 첫 번째 Xamarin.Android 응용 프로그램을 빌드하는 Xamarin.Android를 구성하는 방법을 설명합니다._

<a name="overview" />

## <a name="overview"></a>개요

Xamarin은 이제 모든 버전의 Visual Studio에 무료로 포함되며 별도의 라이선스를 요구하지 않으므로 Visual Studio 설치 관리자를 사용하여 Xamarin.Android 도구를 다운로드하고 설치할 수 있습니다.
(이전 버전의 Xamarin.Android에 필요한 수동 설치 및 라이선스 단계는 더 이상 필요하지 않습니다.) 이 가이드에서는 다음 항목에 대해 알아봅니다.

-   Java 개발 키트, Android SDK 및 Android NDK의 사용자 지정 위치를 구성하는 방법

-   Android SDK Manager를 시작하여 추가 Android SDK 구성 요소를 다운로드하고 설치하는 방법

-   디버깅 및 테스트할 Android 장치 또는 에뮬레이터를 준비하는 방법

-   첫 번째 Xamarin.Android 앱 프로젝트를 만드는 방법

이 가이드의 뒷부분에서는 Visual Studio에 통합된 Xamarin.Android 설치가 작동하고 첫 번째 Xamarin.Android 응용 프로그램을 빌드할 준비가 됩니다.

## <a name="installation"></a>설치

Windows에서 Visual Studio와 함께 사용할 Xamarin을 설치하는 방법에 자세한 내용은 [Windows 설치](~/cross-platform/get-started/installation/windows.md) 가이드를 참조하세요.

<a name="configuration" />

## <a name="configuration"></a>구성

Xamarin.Android는 JDK(Java Development Kit) 및 Android SDK를 사용하여 앱을 빌드합니다. 설치하는 동안 Visual Studio 설치 프로그램은 기본 위치에 이러한 도구를 배치하고 적절한 경로 구성을 사용하여 개발 환경을 구성합니다. **도구 > 옵션 > Xamarin > Android 설정**을 클릭하여 다음 위치를 보고 변경할 수 있습니다.

![Xamarin Android 설정 대화 상자의 스크린샷](windows-images/07-settings.png)

대부분의 사용자의 경우 이러한 기본 위치는 추가 변경 없이 사용할 수 있습니다. 그러나 이러한 도구에 사용자 지정 위치를 포함하는 Visual Studio를 구성할 수도 있습니다(예: 다른 위치에서 Java JDK, Android SDK 또는 NDK를 설치한 경우). 변경하려는 경로 옆에 있는 **변경**을 클릭한 다음, 새 위치로 이동합니다.

Xamarin.Android는 API 수준 24 이상을 대상으로 개발하는 경우에 필요한 [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)을 사용합니다(JDK 8은 API 24 미만도 지원함). API 수준 23 이하를 대상으로 개발하는 경우 [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)을 계속 사용할 수 있습니다.

> [!IMPORTANT]
> **참고:** Xamarin.Android는 JDK 9를 지원하지 않습니다.

<a name="sdk_manager" />

### <a name="android-sdk-manager"></a>Android SDK Manager

Android는 여러 Android API 수준 설정을 사용하여 다양한 버전의 Android에 걸쳐 앱의 호환성을 확인합니다(Android API 수준에 대한 자세한 내용은 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md) 참조).
대상으로 지정하려는 Android API 수준에 따라 추가 Android SDK 구성 요소를 다운로드하고 설치해야 합니다. 또한 Android SDK에서 제공하는 선택적 도구 및 에뮬레이터 이미지를 설치해야 할 수 있습니다. 이를 위해 **Android SDK Manager**를 사용합니다. **도구 > Android > Android SDK Manager**를 클릭하여 **Android SDK Manager**를 시작할 수 있습니다.

[![Android SDK Manager를 시작하는 방법](windows-images/08-sdk-manager-sml.png)](windows-images/08-sdk-manager.png)

기본적으로 Visual Studio는 Google Android SDK Manager를 설치합니다.

![Google Android SDK Manager의 스크린샷 예제](windows-images/09-google-sdk-manager.png)

Google Android SDK Manager를 사용하여 최대 Android SDK 도구 패키지의 25.2.3 버전을 설치할 수 있습니다. 그러나 나중에 최신 버전의 Android SDK Tools 패키지를 사용해야 하는 경우 Visual Studio용 Xamarin Android SDK Manager 플러그 인을 설치해야 합니다(Visual Studio Marketplace에서 사용 가능). Android SDK Tools 패키지의 25.2.3 버전에서 Google의 독립 실행형 SDK Manager를 사용하지 않기 때문에 이 기능이 필요합니다. 

Xamarin Android SDK Manager에 대한 자세한 내용은 [Android SDK 설정](~/android/get-started/installation/android-sdk.md)을 참조하세요.

<a name="emulator" />

### <a name="android-emulator"></a>Android 에뮬레이터

테스트에 사용할 물리적 Android 장치가 없는 경우 Android 에뮬레이터를 사용하여 앱을 테스트할 수 있습니다. Google Android 에뮬레이터에 대한 자세한 내용은 [Android SDK 에뮬레이터](~/android/deploy-test/debugging/android-sdk-emulator/index.md)를 참조하세요.

Google Android 에뮬레이터는 Intel의 HAXM(Hardware Accelerated Execution Manager)를 사용합니다. 그러면 다른 에뮬레이터에서 사용하는 가상화 기술과 충돌할 수 있습니다. 세 가지 주요 가상화 기술은 다음과 같습니다.

-   **Hyper-V**(Android용 Visual Studio 에뮬레이터 및 Windows Phone 에뮬레이터에서 사용함) 

-   **Virtual Box**(Genymotion에서 사용함)

-   **Intel HAXM**(Google Android SDK 에뮬레이터에서 사용함) 

개발 컴퓨터의 CPU가 한 번에 하나의 가상화 기술만 지원할 수 있기 때문에 개발 컴퓨터에서 하나의 기술만 사용하는 것이 좋습니다.

<a name="device" />

### <a name="android-device"></a>Android 장치

테스트에 사용할 물리적 Android 장치가 있는 경우 개발에 사용하도록 설정하는 것이 좋습니다. [개발용 장치 설정](~/android/get-started/installation/set-up-device-for-development.md)을 참조하여 개발할 Android 장치를 구성한 다음, Xamarin.Android 응용 프로그램을 실행하고 디버깅하는 컴퓨터에 연결합니다.


<a name="create_app" />

## <a name="create-an-application"></a>응용 프로그램 만들기

Xamarin.Android를 설치했으므로 Visual Studio를 시작하여 새 프로젝트를 만들 수 있습니다. **파일 > 새로 만들기 > 프로젝트**를 클릭하여 앱을 만들기 시작합니다.

![새 프로젝트를 만드는 방법](windows-images/10-new-project.png)

**새 프로젝트** 대화 상자의 **템플릿** 아래에서 **Android**를 선택하고, 오른쪽 창에서 **비어 있는 앱(Android)**을 클릭합니다. 앱의 이름을 입력한 다음(아래 스크린샷에서 앱을 **MyApp**이라고 함), **확인**을 클릭합니다.

[![새 프로젝트 대화 상자의 스크린샷, 비어 있는 Android 앱 만들기](windows-images/11-first-app-sml.png)](windows-images/11-first-app.png)

정말 간단하죠. 이제 Xamarin.Android를 사용하여 Android 응용 프로그램을 만들 준비가 되었습니다.


<a name="summary" />

## <a name="summary"></a>요약

이 문서에서는 Windows에서 Xamarin.Android 플랫폼을 설정하고 설치하는 방법, (선택 사항)사용자 지정 JDK Java 및 Android SDK 설치 위치를 사용하여 Visual Studio를 구성하는 방법, SDK Manager를 시작하여 추가 Android SDK를 설치하는 방법, Android 장치 또는 에뮬레이터를 설정하는 방법 및 첫 번째 응용 프로그램을 빌드하기 시작하는 방법을 알아보았습니다.

다음 단계에서는 [Hello, Android](~/android/get-started/hello-android/index.md) 자습서를 살펴보고 작동하는 Xamarin.Android 앱을 만드는 방법을 알아봅니다.


## <a name="related-links"></a>관련 링크

- [Visual Studio 다운로드](https://www.visualstudio.com/vs/)
- [Visual Studio Tools for Xamarin 설치](~/cross-platform/get-started/installation/windows.md)
- [시스템 요구 사항](~/cross-platform/get-started/requirements.md)
- [Android SDK 설정](~/android/get-started/installation/android-sdk.md)
- [Android SDK 에뮬레이터](~/android/get-started/installation/android-emulator/index.md)
- [개발용 장치 설정](~/android/get-started/installation/set-up-device-for-development.md)
