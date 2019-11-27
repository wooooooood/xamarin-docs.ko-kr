---
title: Android 10 기능
description: Xamarin.ios를 사용 하 여 Android 10 용 앱 개발을 시작 하는 방법입니다.
ms.assetid: B3342772-FB88-4B7F-BC15-8BC78EED749E
author: JonDouglas
ms.author: jodou
ms.date: 09/17/2019
ms.openlocfilehash: c19c9e5bd279824ea2d3e4e9f88857388f786a2c
ms.sourcegitcommit: b11dc46a9ba23483195e923de88cbef173730087
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73612273"
---
# <a name="android-10-with-xamarin"></a>Android 10 및 Xamarin

_Xamarin.ios를 사용 하 여 Android 10 용 앱 개발을 시작 하는 방법입니다._

이제 Google에서 Android 10을 사용할 수 있습니다. 이 릴리스에서는 여러 가지 새로운 기능과 Api를 사용할 수 있으며,이 중 상당수는 최신 Android 장치에서 새로운 하드웨어 기능을 활용 하는 데 필요 합니다.

![Android 10 로고](~/android/platform/android-10-images/android10_black.png)

이 문서는 Android 10 용 Xamarin Android 앱 개발을 시작 하는 데 도움이 되는 구조화 된 문서입니다. 필요한 업데이트를 설치 하 고, SDK를 구성 하 고, 테스트할 에뮬레이터 또는 장치를 준비 하는 방법을 설명 합니다. 또한 Android 10의 새로운 기능에 대 한 개요를 제공 하 고 몇 가지 주요 Android 10 기능을 사용 하는 방법을 보여 주는 예제 소스 코드를 제공 합니다.

Xamarin Android 10.0은 Android 10에 대 한 지원을 제공 합니다. Android 10에 대 한 Xamarin Android 지원에 대 한 자세한 내용은 [Xamarin android 10.0 릴리스 정보](https://docs.microsoft.com/xamarin/android/release-notes/10/10.0)를 참조 하세요.

## <a name="requirements"></a>요구 사항

다음 목록은 Xamarin 기반 앱에서 Android 10 기능을 사용 하는 데 필요 합니다.

- **Visual studio** -visual studio 2019을 권장 합니다. Windows에서 Visual Studio 2019 버전 16.3 이상으로 업데이트 합니다. MacOS에서 Mac 용 Visual Studio 2019 버전 8.3 이상으로 업데이트 합니다.
- **Xamarin android** -xamarin. android 10.0 이상이 Visual Studio와 함께 설치 되어야 합니다. Xamarin은 Windows에서 .net 작업을 **사용 하 여 모바일 개발** 의 일부로 자동으로 설치 되 고 **Mac용 Visual Studio 설치 관리자**의 일부로 설치 됩니다.
- **Java 개발자 키트** -Xamarin Android 10.0 개발에는 JDK 8이 필요 합니다. Microsoft의 OpenJDK 배포는 자동으로 Visual Studio의 일부로 설치 됩니다.
- **Android SDK** -Android SDK API 29는 Android SDK Manager를 통해 설치 해야 합니다.

## <a name="get-started"></a>시작

Xamarin.ios를 사용 하 여 Android 10 앱 개발을 시작 하려면 첫 번째 Android 10 프로젝트를 만들기 전에 최신 도구 및 SDK 패키지를 다운로드 하 여 설치 해야 합니다.

1. **Visual Studio 2019을 권장**합니다. Visual Studio 2019 버전 16.3 이상으로 업데이트 합니다. Mac용 Visual Studio 2019를 사용 하는 경우 Mac 용 Visual Studio 2019 버전 8.3 이상을 업데이트 합니다.
2. SDK Manager를 통해 **Android 10 (API 29)** 패키지 및 도구를 설치 합니다.
    - Android 10 (API 29) SDK 플랫폼
    - Android 10 (API 29) 시스템 이미지
    - Android SDK 빌드-도구 29.0.0 +
    - Android SDK 플랫폼-도구 29.0.0 +
    - Android Emulator 29.0.0 +
3. Android 10.0를 대상으로 하는 새 Xamarin Android 프로젝트를 만듭니다.
4. Android 10 앱을 테스트 하기 위한 에뮬레이터 또는 장치를 구성 합니다.

이러한 각 단계는 아래에 설명 되어 있습니다.

### <a name="update-visual-studio"></a>Visual Studio 업데이트

Xamarin을 사용 하 여 Android 10 앱을 빌드하려면 Visual Studio 2019을 사용 하는 것이 좋습니다.

Visual Studio 2019을 사용 하는 경우 Visual studio 2019 버전 16.3 이상으로 업데이트 합니다 (자세한 내용은 [Visual studio 2019을 최신 릴리스로 업데이트](https://docs.microsoft.com/visualstudio/install/update-visual-studio)). MacOS에서 Mac 8.3 이상에 대해 Visual Studio 2019로 업데이트 합니다 (자세한 내용은 [mac 용 Visual studio 2019를 최신 릴리스로 업데이트](https://docs.microsoft.com/visualstudio/mac/update)).

### <a name="install-the-android-sdk"></a>Android SDK 설치

Xamarin. Android 10.0를 사용 하 여 프로젝트를 만들려면 먼저 Android SDK Manager를 사용 하 여 Android 10 용 SDK 플랫폼 **(API 레벨 29)** 을 설치 해야 합니다.

1. SDK Manager를 시작 합니다. Visual Studio에서 **도구 > Android > Android SDK Manager를 클릭 합니다.** Mac용 Visual Studio에서 **도구 > SDK Manager를 클릭 합니다.**
2. 오른쪽 아래 모서리에서 기어 아이콘을 클릭 하 고 **저장소 > Google (지원 되지 않음)** 을 선택 합니다.

    ![Android SDK Manager 리포지토리 선택](~/android/platform/android-10-images/sdkrepository.png)

3. **플랫폼 탭에** **Android SDK Platform 29** 로 나열 된 **Android 10 SDK 플랫폼** 패키지를 설치 합니다 (SDK Manager를 사용 하는 방법에 대 한 자세한 내용은 [Android SDK 설치](https://docs.microsoft.com/xamarin/android/get-started/installation/android-sdk)참조).

    ![Android SDK Manager 플랫폼 탭](~/android/platform/android-10-images/sdkplatforms.png)

### <a name="create-a-xamarinandroid-project"></a>Xamarin Android 프로젝트 만들기

새 Xamarin Android 프로젝트를 만듭니다. Xamarin을 사용 하 여 Android 개발을 처음 접하는 경우에는 [Hello, android](https://docs.microsoft.com/xamarin/android/get-started/hello-android/index) 를 참조 하 여 xamarin.ios 프로젝트를 만드는 방법에 대해 알아보세요.

Android 프로젝트를 만들 때 Android 10.0 이상 버전을 대상으로 하도록 버전 설정을 구성 해야 합니다. 예를 들어 Android 10 용 프로젝트를 대상으로 하려면 프로젝트의 대상 Android API 수준을 **android 10.0 (API 29)** 로 구성 해야 합니다. 여기에는 **대상 프레임 워크 버전과** **대상 ANDROID SDK 버전이** 모두 API 29 이상으로 포함 됩니다. Android API 수준을 구성 하는 방법에 대 한 자세한 내용은 [ANDROID Api 수준 이해](https://docs.microsoft.com/xamarin/android/app-fundamentals/android-api-levels) 를 참조 하세요.

![Xamarin Android 대상 프레임 워크](~/android/platform/android-10-images/targetframework.png)

### <a name="configure-a-device-or-emulator"></a>장치 또는 에뮬레이터 구성

픽셀과 같은 물리적 장치를 사용 하는 경우 휴대폰 설정에서 `System` > `System update` > `Check for update`으로 이동 하 여 Android 10 업데이트를 다운로드할 수 있습니다. 장치의 플래시를 선호 하는 경우 장치에 대 한 [공장 이미지](https://developers.google.com/android/images) 또는 [OTA 이미지](https://developers.google.com/android/ota) 깜박임의 지침을 참조 하세요.

에뮬레이터를 사용 하는 경우 API 레벨 29 용 가상 장치를 만들고 x86 기반 이미지를 선택 합니다. Android Device Manager를 사용 하 여 가상 장치를 만들고 관리 하는 방법에 대 한 자세한 내용은 Android Device Manager를 사용 하 여 [가상 장치 관리](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager) 를 참조 하세요. 테스트 및 디버깅을 위해 Android Emulator를 사용 하는 방법에 대 한 자세한 내용은 [Android Emulator 디버깅](https://docs.microsoft.com/xamarin/android/deploy-test/debugging/debug-on-emulator) 을 참조 하십시오.

## <a name="new-features"></a>새 기능

Android 10에는 다양 한 새로운 기능이 도입 되었습니다. 이러한 새로운 기능 중 일부는 최신 Android 장치에서 제공 하는 새로운 하드웨어 기능을 활용 하기 위한 것이 고, 다른 일부는 Android 사용자 환경을 더욱 향상 시키기 위해 설계 되었습니다.

## <a name="enhance-your-app-with-android-10-features-and-apis"></a>Android 10 기능 및 Api를 사용 하 여 앱 향상

다음으로, 준비가 되 면 Android 10에 대해 알아보고 사용할 수 있는 [새 기능 및 api](https://developer.android.com/preview/api-overview.html) 에 대해 알아보세요. 다음은를 시작 하기 위한 몇 가지 주요 기능입니다.

이러한 기능은 모든 앱에 권장 됩니다.

- **진한 테마:** 진한 [테마](https://developer.android.com/preview/features/darktheme) 추가 하거나 [강제 어둡게](https://developer.android.com/preview/features/darktheme#force_dark)를 사용 하도록 설정 하 여 시스템 수준의 진한 테마를 사용 하는 사용자에 게 일관 된 환경을 합니다.

![어두운 테마](~/android/platform/android-10-images/darktheme.png)

- Edge에서 가장자리로 이동하고 사용자 지정 제스처가 시스템 탐색 제스처** [로 보완되도록 하여 앱에서 gestural 탐색을](https://developer.android.com/preview/features/gesturalnav)** 지원합니다.

![제스처 탐색](~/android/platform/android-10-images/gesturenavigation.png)

- **Foldables에 최적화 :**  [foldables을 최적화](https://developer.android.com/preview/features/foldables)하 여 오늘날의 혁신적인 장치에서 원활한 최첨단 환경을 제공 합니다.

![폴딩 가능](~/android/platform/android-10-images/foldable.png)

앱과 관련 된 경우 다음 기능을 권장 합니다.

- **추가 대화형 알림:** 알림이 메시지를 포함 하는 경우 알림  [에서 제안 된 회신 및 동작](https://developer.android.com/preview/features#smart-suggestions) 을 사용 하도록 설정 하 여 사용자를 참여 시키고 즉시 조치를 취할 수 있도록 합니다.
- **향상 된 생체 인식:** 생체 인식 인증을 사용 하는 경우 최신 장치에서 지문 인증을 지 원하는 기본 방법인 [BiometricPrompt](https://developer.android.com/reference/androidx/biometric/BiometricPrompt)로 이동 합니다.
- **보강 녹음:** 캡션 또는 게임 기록을 지원 하기 위해  [오디오 재생 캡처](https://developer.android.com/preview/features/playback-capture)를 사용 하도록 설정 합니다. 더 많은 사용자를 연결 하 고 앱을 더 쉽게 액세스할 수 있도록 하는 좋은 방법입니다.
- **더 나은 코덱:** 미디어 앱에 대 한 비디오 스트리밍에 대해 [AV1](https://en.wikipedia.org/wiki/AV1) 를 시도 하 고, 높은 동적 범위 비디오에는 [HDR10 +](https://en.wikipedia.org/wiki/High-dynamic-range_video#HDR10+) 를 시도 합니다. 음성 및 음악 스트리밍의 경우 [Opus](http://opus-codec.org/) encoding을 사용 하 고 musicians의 경우 [네이티브 MIDI API](https://developer.android.com/preview/features/midi) 를 사용할 수 있습니다.
- **향상 된 네트워킹 api:** 앱에서 wi-fi를 통해 IoT 장치를 관리 하는 경우 구성, 다운로드 또는 인쇄와 같은 기능을 위해 새 [네트워크 연결 api](https://developer.android.com/preview/features#peer2peer) 를 사용해 보세요.

Android 10에는 몇 가지 새로운 기능과 Api가 있습니다. 모두 보려면 [개발자를 위한 Android 10 사이트](https://developer.android.com/about/versions/10/highlights)를 방문 하세요.

## <a name="behavior-changes"></a>동작 변경

대상 Android 버전이 API 수준 29로 설정 된 경우 위에 설명 된 새로운 기능을 구현 하지 않는 경우에도 앱의 동작에 영향을 주는 몇 가지 플랫폼 변경 내용이 있습니다. 다음 목록은 이러한 변경 내용에 대 한 간략 한 요약입니다.

- [앱의 안정성과 호환성을 보장 하기 위해 android 플랫폼은 이제 앱이 android 10에서 사용할 수 있는 비 SDK 인터페이스를 제한 합니다](https://developer.android.com/about/versions/10/behavior-changes-10#non-sdk-restrictions).
- [공유 메모리가 변경 되었습니다](https://developer.android.com/about/versions/10/behavior-changes-10#shared-memory).
- [Android runtime &AMP; AOT 정확성](https://developer.android.com/about/versions/10/behavior-changes-10#system-only-oat)입니다.
- [전체 화면 의도에 대 한 사용 권한은 `USE_FULL_SCREEN_INTENT`를 요청 해야 합니다 ](https://developer.android.com/about/versions/10/behavior-changes-10#full-screen-intents).
- [Foldables에 대 한 지원](https://developer.android.com/about/versions/10/behavior-changes-10#foldables).

## <a name="summary"></a>요약

이 문서에서는 Android 10을 소개 하 고 Android 10을 사용 하 여 Xamarin Android 개발용 최신 도구 및 패키지를 설치 및 구성 하는 방법을 설명 했습니다. Android 10에서 사용할 수 있는 주요 기능에 대 한 개요를 제공 했습니다. Android 10 용 앱을 만드는 과정을 시작 하는 데 도움이 되는 API 설명서 및 Android 개발자 항목에 대 한 링크가 포함 되어 있습니다. 또한 기존 앱에 영향을 줄 수 있는 가장 중요 한 Android 10 동작 변경 내용도 강조 표시 되어 있습니다.

## <a name="related-links"></a>관련 링크

- [Android 10](https://developer.android.com/about/versions/10)
