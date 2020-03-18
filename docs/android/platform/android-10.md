---
title: Android 10 기능
description: Xamarin.Android를 사용하여 Android 10용 앱 개발을 시작하는 방법입니다.
ms.assetid: B3342772-FB88-4B7F-BC15-8BC78EED749E
author: JonDouglas
ms.author: jodou
ms.date: 09/17/2019
ms.openlocfilehash: c19c9e5bd279824ea2d3e4e9f88857388f786a2c
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73612273"
---
# <a name="android-10-with-xamarin"></a>Xamarin이 포함된 Android 10

_Xamarin.Android를 사용하여 Android 10용 앱 개발을 시작하는 방법입니다._

이제 Google에서 Android 10을 사용할 수 있습니다. 이 릴리스에서는 여러 가지 새로운 기능과 API를 사용할 수 있으며 이 중 상당수는 최신 Android 디바이스에서 새로운 하드웨어 기능을 활용하는 데 필요합니다.

![Android 10 로고](~/android/platform/android-10-images/android10_black.png)

이 문서는 Android 10용 Xamarin.Android 앱 개발을 시작하는 데 도움이 되도록 구성되었습니다. 필요한 업데이트를 설치하고, SDK를 구성하고, 테스트할 에뮬레이터 또는 디바이스를 준비하는 방법을 설명합니다. 또한 Android 10의 새로운 기능에 대한 개요를 제공하고 주요 Android 10 기능 중 일부를 사용하는 방법을 보여주는 예제 소스 코드를 제공합니다.

Xamarin.Android 10.0은 Android 10을 지원합니다. Android 10에 대한 Xamarin.Android 지원에 대한 자세한 내용은 [Xamarin.Android 10.0 릴리스 정보](https://docs.microsoft.com/xamarin/android/release-notes/10/10.0)를 참조하세요.

## <a name="requirements"></a>요구 사항

Xamarin 기반 앱에서 Android 10 기능을 사용하려면 다음 목록이 필요합니다.

- **Visual Studio** - Visual Studio 2019를 사용하는 것이 좋습니다. Windows에서 Visual Studio 2019 버전 16.3 이상으로 업데이트합니다. macOS에서 Mac용 Visual Studio 2019 버전 8.3 이상으로 업데이트합니다.
- **Xamarin.Android** - Xamarin.Android 10.0 이상은 Visual Studio와 함께 설치되어야 합니다(Xamarin.Android는 Windows에서 **.NET을 이용한 모바일 개발** 워크로드의 일부로 자동 설치되고 **Mac용 Visual Studio 설치 관리자**의 일부로 설치됨).
- **Java Developer Kit** - Xamarin.Android 10.0 개발에는 JDK 8이 필요합니다. Microsoft의 OpenJDK 배포는 Visual Studio의 일부로 자동 설치됩니다.
- **Android SDK** - Android SDK API 29는 Android SDK Manager를 통해 설치해야 합니다.

## <a name="get-started"></a>시작

Xamarin.Android를 사용하여 Android 10 앱 개발을 시작하려면 첫 번째 Android 10 프로젝트를 만들기 전에 최신 도구 및 SDK 패키지를 다운로드하여 설치해야 합니다.

1. **Visual Studio 2019를 사용하는 것이 좋습니다**. Visual Studio 2019 버전 16.3 이상으로 업데이트합니다. Mac용 Visual Studio 2019를 사용하는 경우 Mac용 Visual Studio 2019 버전 8.3 이상으로 업데이트합니다.
2. SDK 관리자를 통해 **Android 10(API 29)** 패키지 및 도구를 설치합니다.
    - Android 10(API 29) SDK 플랫폼
    - Android 10(API 29) 시스템 이미지
    - Android SDK 빌드-도구 29.0.0+
    - Android SDK 플랫폼-도구 29.0.0+
    - Android Emulator 29.0.0+
3. Android 10.0을 대상으로 하는 새 Xamarin.Android 프로젝트를 만듭니다.
4. Android 10 앱을 테스트하기 위한 에뮬레이터 또는 디바이스를 구성합니다.

이러한 각 단계는 아래에 설명되어 있습니다.

### <a name="update-visual-studio"></a>Visual Studio 업데이트

Xamarin을 사용하여 Android 10 앱을 빌드하려면 Visual Studio 2019를 사용하는 것이 좋습니다.

Visual Studio 2019를 사용하는 경우 Visual Studio 2019 버전 16.3 이상으로 업데이트합니다. 자세한 내용은 [Visual Studio 2019를 최신 릴리스로 업데이트](https://docs.microsoft.com/visualstudio/install/update-visual-studio)를 참조하세요. macOS에서 Mac용 Visual Studio 2019 8.3 이상으로 업데이트합니다. 자세한 내용은 [Mac용 Visual Studio 2019를 최신 릴리스로 업데이트](https://docs.microsoft.com/visualstudio/mac/update)를 참조하세요.

### <a name="install-the-android-sdk"></a>Android SDK 설치

Xamarin.Android 10.0을 통해 프로젝트를 만들려면 먼저 Android SDK 관리자를 사용하여 **Android 10(API 수준 29)** 용 SDK 플랫폼을 설치해야 합니다.

1. SDK 관리자를 시작합니다. Visual Studio에서 **도구 > Android > Android SDK 관리자**를 클릭합니다. Mac용 Visual Studio에서 **도구 > SDK 관리자**를 클릭합니다.
2. 오른쪽 아래 모서리에서 기어 아이콘을 클릭하고 **리포지토리 > Google(지원되지 않음)** 을 선택합니다.

    ![Android SDK 관리자 리포지토리 선택](~/android/platform/android-10-images/sdkrepository.png)

3. **플랫폼** 탭에서 **Android SDK 플랫폼 29**로 나열된 **Android 10 SDK 플랫폼** 패키지를 설치합니다(SDK 관리자 사용에 대한 자세한 내용은 [Android SDK 설치](https://docs.microsoft.com/xamarin/android/get-started/installation/android-sdk) 참조).

    ![Android SDK 관리자 플랫폼 탭](~/android/platform/android-10-images/sdkplatforms.png)

### <a name="create-a-xamarinandroid-project"></a>Xamarin.Android 프로젝트 만들기

새 Xamarin.Android 프로젝트를 만듭니다. Xamarin을 사용한 Android 개발을 처음 접하는 경우 [Hello, Android](https://docs.microsoft.com/xamarin/android/get-started/hello-android/index)를 참조하여 Xamarin.Android 프로젝트를 만드는 방법에 대해 알아보세요.

Android 프로젝트를 만들 때는 Android 10.0 이상을 대상으로 버전 설정을 구성해야 합니다. 예를 들어 Android 10용 프로젝트를 대상으로 하려면 프로젝트의 대상 Android API 수준을 **Android 10.0(API 29)** 으로 구성해야 합니다. 여기에는 **대상 프레임워크 버전** 및 **대상 Android SDK 버전**이 API 29 이상에 모두 포함됩니다. Android API 수준을 구성하는 방법에 대한 자세한 내용은 [Android API 수준 이해](https://docs.microsoft.com/xamarin/android/app-fundamentals/android-api-levels)를 참조하세요.

![Xamarin.Android 대상 프레임워크](~/android/platform/android-10-images/targetframework.png)

### <a name="configure-a-device-or-emulator"></a>디바이스 또는 에뮬레이터 구성

픽셀과 같은 물리적 디바이스를 사용하는 경우 휴대폰 설정에서 `System` > `System update` > `Check for update`로 이동하여 Android 10 업데이트를 다운로드할 수 있습니다. 디바이스를 플래시하려면 디바이스에 대한 [팩터리 이미지](https://developers.google.com/android/images) 또는 [OTA 이미지](https://developers.google.com/android/ota)를 플래시하는 방법에 대한 지침을 참조하세요.

에뮬레이터를 사용하는 경우 API 수준 29용 가상 디바이스를 만들고 x86 기반 이미지를 선택합니다. Android 디바이스 관리자를 사용하여 가상 디바이스를 만들고 관리하는 방법에 대한 자세한 내용은 [Android 디바이스 관리자를 사용하여 가상 디바이스 관리](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager)를 참조하세요. 테스트 및 디버깅을 위해 Android Emulator를 사용하는 방법에 대한 자세한 내용은 [Android Emulator에서 디버깅](https://docs.microsoft.com/xamarin/android/deploy-test/debugging/debug-on-emulator)을 참조하세요.

## <a name="new-features"></a>새 기능

Android 10에는 다양한 새로운 기능이 도입되었습니다. 이러한 새로운 기능 중 일부는 최신 Android 디바이스에서 제공하는 새로운 하드웨어 기능을 활용하기 위한 것이며 다른 일부는 Android 사용자 환경을 더욱 향상시키기 위해 설계되었습니다.

## <a name="enhance-your-app-with-android-10-features-and-apis"></a>Android 10 기능 및 API를 사용하여 앱 향상

그런 다음, 준비가 되었으면 Android 10에 대해 알아보고 사용할 수 있는  [새 기능과 API](https://developer.android.com/preview/api-overview.html) 에 대해 알아봅니다. 다음은 시작하기 위한 몇 가지 주요 기능입니다.

이러한 기능은 모든 앱에 권장됩니다.

- **어두운 테마:**   [어두운 테마](https://developer.android.com/preview/features/darktheme) 를 추가하거나  [Force Dark](https://developer.android.com/preview/features/darktheme#force_dark)를 활성화하여 시스템 전체의 어두운 테마를 사용하는 사용자에게 일관된 환경을 사용하도록 설정합니다.

![어두운 테마](~/android/platform/android-10-images/darktheme.png)

- 에지 간 이동하고 사용자 지정 제스처가 시스템 탐색 제스처를 보완하도록 하여 앱에서 **제스처 탐색 [을 지원](https://developer.android.com/preview/features/gesturalnav)**  합니다.

![제스처 탐색](~/android/platform/android-10-images/gesturenavigation.png)

- **foldables에 최적화:**   [foldables를 최적화](https://developer.android.com/preview/features/foldables)하여 오늘날의 혁신적인 디바이스에서 완벽한 에지 간 환경을 제공합니다.

![폴딩 가능](~/android/platform/android-10-images/foldable.png)

앱과 관련된 경우 다음 기능을 권장합니다.

- **추가 대화형 알림:**  알림이 메시지가 포함된 경우  [알림에서 제안된 회신 및 작업](https://developer.android.com/preview/features#smart-suggestions) 을 활성화하여 사용자를 참여시키고 즉시 조치를 취할 수 있습니다.
- **향상된 생체 인식:**  생체 인식 인증을 사용하는 경우 최신 디바이스에서 지문 인증을 지원하는 기본 방법인  [BiometricPrompt](https://developer.android.com/reference/androidx/biometric/BiometricPrompt)로 이동합니다.
- **보강 기록:**  캡션 또는 게임 재생 기록을 지원하려면  [오디오 재생 캡처](https://developer.android.com/preview/features/playback-capture)를 사용하도록 설정합니다. 더 많은 사용자를 연결하고 앱을 더 쉽게 액세스할 수 있도록 하는 좋은 방법입니다.
- **향상된 코덱:**  미디어 앱의 경우 비디오 스트리밍용  [AV1](https://en.wikipedia.org/wiki/AV1)  및 높은 동적 범위 비디오용  [HDR10+](https://en.wikipedia.org/wiki/High-dynamic-range_video#HDR10+) 를 사용해 보세요. 음성 및 음악 스트리밍의 경우  [Opus](http://opus-codec.org/) 인코딩을 사용할 수 있으며 음악가의 경우  [네이티브 MIDI API](https://developer.android.com/preview/features/midi) 를 사용할 수 있습니다.
- **향상된 네트워킹 API:**  앱이 Wi-Fi를 통해 IoT 디바이스를 관리하는 경우 구성, 다운로드 또는 인쇄와 같은 기능을 위해 새로운  [네트워크 연결 API](https://developer.android.com/preview/features#peer2peer) 를 사용해 보세요.

Android 10에는 몇 가지 새로운 기능과 API가 있습니다. 이러한 항목을 모두 보려면  [개발자용 Android 10 사이트](https://developer.android.com/about/versions/10/highlights)를 방문하세요.

## <a name="behavior-changes"></a>동작 변경

대상 Android 버전이 API 수준 29로 설정되면 위에서 설명된 새로운 기능을 구현하지 않더라도 앱의 동작에 영향을 주는 몇 가지 플랫폼 변경 내용이 있습니다. 다음 목록은 이러한 변경 내용을 간략하게 요약한 것입니다.

- [앱의 안정성과 호환성을 보장하기 위해 Android 플랫폼은 이제 앱이 Android 10에서 사용할 수 있는 비 SDK 인터페이스를 제한합니다](https://developer.android.com/about/versions/10/behavior-changes-10#non-sdk-restrictions).
- [공유 메모리가 변경되었습니다](https://developer.android.com/about/versions/10/behavior-changes-10#shared-memory).
- [Android 런타임 및 AOT 정확성](https://developer.android.com/about/versions/10/behavior-changes-10#system-only-oat).
- [전체 화면 의도에 대한 사용 권한은 `USE_FULL_SCREEN_INTENT`를 요청해야 합니다](https://developer.android.com/about/versions/10/behavior-changes-10#full-screen-intents).
- [Foldables에 대해 지원합니다](https://developer.android.com/about/versions/10/behavior-changes-10#foldables).

## <a name="summary"></a>요약

이 문서에서는 Android 10을 소개하고 Android 10을 사용하여 Xamarin.Android 개발을 위한 최신 도구 및 패키지를 설치 및 구성하는 방법을 설명했습니다. Android 10에서 사용할 수 있는 주요 기능에 대한 개요를 제공했습니다. 여기에는 Android 10용 앱을 만드는 과정을 시작하는 데 도움이 되는 API 설명서 및 Android 개발자 항목에 대한 링크가 포함되어 있습니다. 기존 앱에 영향을 줄 수 있는 가장 중요한 Android 10 동작 변경 내용도 강조 표시되어 있습니다.

## <a name="related-links"></a>관련 링크

- [Android 10](https://developer.android.com/about/versions/10)
