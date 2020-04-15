---
title: Marshmallow 기능
description: 이 문서는 Xamarin.Android를 사용하여 Android 6.0 Marshmallow용 앱 개발을 시작하는 데 도움이 됩니다.
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: fb1ba92be9527d490b3d34bd4c0e454b0a750837
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019981"
---
# <a name="marshmallow-features"></a>Marshmallow 기능

_이 문서는 Xamarin.Android를 사용하여 Android 6.0 Marshmallow용 앱 개발을 시작하는 데 도움이 됩니다._

이 문서에서는 Android 6.0 Marshmallow의 새로운 기능을 개략적으로 설명하고, Android Marshmallow 개발용 Xamarin.Android를 준비하는 방법을 설명하고, Xamarin.Android 앱에서 새로운 Android Marshmallow 기능을 사용하는 방법을 설명하는 샘플 애플리케이션에 대한 링크를 제공합니다. 

## <a name="overview"></a>개요

[Android 6.0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html)는 Android Lollipop 다음 번의 주요 Android 릴리스입니다.
Xamarin.Android는 Android Marshmallow를 지원하며 다음을 포함합니다.

- **API 23/Android 6.0 바인딩** &ndash; Android 6.0에는 아래 설명된 새 기능을 위한 새로운 API가 다수 추가되었습니다. API 레벨 23을 대상으로 하는 경우 Xamarin.Android 앱에서 이러한 API를 사용할 수 있습니다. Android 6.0 API에 대한 자세한 내용은 [Android 6.0 API](https://developer.android.com/preview/api-overview.html)를 참조하세요. 

[![Marshmallow를 실행하는 태블릿 및 휴대폰의 히어로 이미지](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Marshmallow 릴리스가 주로 "세련 및 품질"에 중점을 두지만 Xamarin.Android 개발자에게 많은 새로운 기능을 제공합니다. 이러한 기능에는 다음이 포함됩니다. 

- **런타임 권한** &ndash; 이 기능 향상을 통해 사용자가 런타임에 사례별로 보안 권한을 승인할 수 있습니다. 

- **인증 향상** &ndash; Android Marshmallow부터 이제 앱이 지문 센서를 사용하여 사용자를 인증할 수 있으며, 새로운 *자격 증명 확인* 기능으로 암호 입력 필요성이 최소화합니다. 

- **앱 링크** &ndash; 이 기능을 사용하면 앱을 웹 도메인과 자동으로 연결하여 **앱 선택기** 팝업을 사용하지 않아도 됩니다. 

- **직접 공유** &ndash; 사용자가 빠르고 직관적으로 공유할 수 있는 *직접 공유* 대상을 정의할 수 있습니다. 이 기능을 사용하면 다른 앱과 콘텐츠를 공유할 수 있습니다. 

- **음성 상호 작용** &ndash; 이 새로운 API를 통해 앱에 대화형 음성 기능을 빌드할 수 있습니다. 

- **4K 디스플레이 모드** &ndash; Android Marshmallow에서 앱은 지원하는 하드웨어에 대해 4K 디스플레이 해상도를 요청할 수 있습니다. 

- **새로운 오디오 기능** &ndash; Marshmallow부터 이제 Android에서 MIDI 프로토콜을 지원합니다. 또한 디지털 오디오 캡처 및 재생 개체를 만들기 위한 새로운 클래스를 제공하고 오디오와 입력 디바이스를 연결하기 위한 새로운 API 후크를 제공합니다. 

- **새로운 비디오 기능** &ndash; Marshmallow는 앱이 오디오 및 비디오 스트림을 동기 방식으로 렌더링하는 데 도움이 되는 새로운 클래스를 제공합니다. 이 클래스는 동적 재생 속도도 지원합니다. 

- **Android for Work** &ndash; Marshmallow에는 회사 소유의 단일 사용자 디바이스에 대한 향상된 컨트롤이 포함되어 있습니다. 디바이스 소유자에 의한 자동 앱 설치 및 제거, 시스템 업데이트 자동 수락, 향상된 인증서 관리, 데이터 사용 추적, 권한 관리 및 작업 상태 알림을 지원합니다. 

- **재질 디자인 지원 라이브러리** &ndash; 새 *디자인 지원 라이브러리*는 애플리케이션에 재질 디자인 모양과 느낌을 보다 쉽게 빌드할 수 있는 디자인 구성 요소 및 패턴을 제공합니다. 

또한 Android M과 함께 많은 핵심 Android 라이브러리 업데이트가 출시되었으며 이러한 업데이트는 Android M 및 이전 버전의 Android 모두에 새로운 기능을 제공합니다.

또한 Android Marshmallow와 함께 많은 핵심 Android 라이브러리 업데이트가 출시되었으며 이러한 업데이트는 Android Marshmallow 및 이전 버전의 Android 모두에 새로운 기능을 제공합니다. 이 문서에서는 Android Marshmallow를 사용하여 앱 빌드를 시작하는 방법을 설명하고 Android 6.0의 중요한 새로운 기능을 간략하게 설명합니다. 

## <a name="requirements"></a>요구 사항

Xamarin 기반 앱에서 새로운 Android Marshmallow 기능을 사용하려면 다음이 필요합니다. 

- **Xamarin.Android** &ndash; Visual Studio 또는 Xamarin Studio에서 xamarin.Android 5.1.7.12 이상을 설치하고 구성해야 합니다.

- **Mac용 Visual Studio** 또는 **Visual Studio** &ndash; Mac용 Visual Studio를 사용하는 경우 버전 5.9.7.22 이상이 필요합니다. Visual Studio를 사용하는 경우 Visual Studio용 Xamarin 도구 버전 3.11.1537 이상이 필요합니다. 

- **Android SDK** &ndash; Android SDK 관리자를 통해 Android SDK 6.0(API 23) 이상을 설치해야 합니다.

- **Java Developer Kit** &ndash; Xamarin.Android는 API 레벨 24 이상에서 개발하는 경우 [JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 이상이 필요합니다(JDK 1.8은 Marshmallow를 포함하여 24 이하의 API 레벨도 지원). 사용자 지정 컨트롤 또는 Forms Previewer를 사용하는 경우 64비트 버전의 JDK 1.8이 필요합니다.

API 레벨 23 및 이하를 대상으로 개발하는 경우 [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)을 계속 사용할 수 있습니다. 

## <a name="getting-started"></a>시작

Xamarin.Android에서 Android Marshmallow를 사용하기 시작하려면 Android Marshmallow 프로젝트를 만들기 전에 최신 도구 및 SDK 패키지를 다운로드하여 설치해야 합니다. 

1. **안정** 채널에서 최신 Xamarin 업데이트를 설치합니다. 

2. Android 6.0 Marshmallow SDK 패키지 및 도구를 설치합니다.

3. Android 6.0 Marshmallow(API 레벨 23)을 대상으로 하는 새 Xamarin.Android 프로젝트를 만듭니다. 

4. Android Marshmallow용 에뮬레이터 또는 디바이스를 구성합니다.

각 단계는 다음 섹션에서 설명합니다.

### <a name="install-xamarin-updates"></a>Xamarin 업데이트 설치

Android 6.0 Marshmallow 지원이 포함되도록 Xamarin을 업데이트하려면 업데이트 채널을 **안정**으로 변경하고 모든 업데이트를 설치합니다. 업데이트 채널에서 업데이트를 설치하는 방법에 대한 자세한 내용은 [업데이트 채널 변경](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel)을 참조하세요. 

### <a name="install-the-android-60-sdk"></a>Android 6.0 SDK 설치

Android Marshmallow용 Xamarin.Android 프로젝트를 만들려면 먼저 Android SDK 관리자를 사용하여 Android 6.0 SDK를 설치해야 합니다.

- Android SDK 관리자를 시작하고(Mac용 Visual Studio에서는 **도구 > SDK 관리자**를 사용, Visual Studio에서는 **도구 > Android > Android SDK 관리자**를 사용) 최신 Android SDK Tools를 설치합니다.

    [![Android SDK 관리자에서 Android SDK Tools 선택](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

- 또한 최신 **Android 6.0** SDK 패키지를 설치합니다.

    [![Android SDK 관리자에서 Android 6.0 SDK 패키지 선택](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Android SDK Tools 수정 버전 24.3.4 이상을 설치해야 합니다.
Android SDK 관리자를 사용하여 Android 6.0 SDK를 설치하는 방법에 대한 자세한 내용은 [SDK 관리자](https://developer.android.com/tools/help/sdk-manager.html)를 참조하세요.

### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android 프로젝트 시작

새 Xamarin.Android 프로젝트를 만듭니다. Xamarin을 사용한 Android 개발이 처음이라면 [Hello, Android](~/android/get-started/hello-android/index.md)를 참조하여 Android 프로젝트를 만드는 방법에 대해 알아보세요. 

Android 프로젝트를 만들 때 Android 6.0 MarshMallow를 대상으로 버전 설정을 구성해야 합니다. 프로젝트가 Marshmallow를 대상으로 하려면 프로젝트를 **API 레벨 23(Xamarin.Android v6.0 지원)** 에 대해 구성해야 합니다. Android API 레벨을 구성하는 방법에 대한 자세한 내용은 [Android API 레벨 이해](~/android/app-fundamentals/android-api-levels.md)를 참조하세요.

### <a name="configure-an-emulator-or-device"></a>에뮬레이터 또는 디바이스 구성

에뮬레이터를 사용하는 경우 Android AVD 관리자를 시작하고 다음 설정을 사용하여 새 디바이스를 만듭니다.

- 디바이스: Nexus 5, 6 또는 9.
- 대상: Android 6.0 - API 레벨 23
- ABI: x86

예를 들어 이 가상 디바이스는 Nexus 5를 에뮬레이트하도록 구성되었습니다.

[![Nexus 5 디바이스, Android 6.0 대상 및 Intel Atom(x86)을 사용하여 AVD 구성](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Nexus 5, 6 또는 9와 같은 물리적 디바이스를 사용하는 경우 Android Marshmallow의 미리 보기 이미지를 설치할 수 있습니다. Android Marshmallow 디바이스를 업데이트하는 방법에 대한 자세한 내용은 [하드웨어 시스템 이미지](https://developer.android.com/preview/download.html#images)를 참조하세요.

## <a name="new-features"></a>새 기능

Android Marshmallow에 도입된 변경 내용 중 상당수는 Android 사용자 환경을 개선하고, 성능을 향상시키고, 버그를 수정하는 데 집중되었습니다. 그러나 Marshmallow에는 Android 플랫폼의 기본 사항에 대한 몇 가지 광범위한 변경 사항도 도입되었습니다. 다음 섹션에서는 이러한 향상된 기능을 중점적으로 설명하고 앱에서 새로운 Android Marshmallow 기능을 사용하기 시작하는 데 도움이 되는 링크를 제공합니다. 

### <a name="runtime-permissions"></a>런타임 권한

Android 권한 시스템은 Android Lollipop 이후 크게 최적화되고 간소화되었습니다. Android Marshmallow에서 사용자는 설치 시가 아닌 런타임에 사례별로 권한을 부여합니다. Android Marshmallow 이상에서 이 기능을 지원하려면 런타임에 사용자에게 권한을 부여하라는 메시지를 표시하도록 앱을 디자인합니다(권한이 필요한 컨텍스트에서). 이렇게 변경하면 앱을 설치하고 업그레이드하는 프로세스가 간소화되므로 사용자가 보다 쉽게 즉시 앱을 사용하기 시작할 수 있습니다. 

Xamarin.Android 앱에서 런타임 권한을 구현하는 방법에 대한 자세한 내용(코드 예제 포함)은 [Android Marshmallow에서 런타임 권한 요청](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/)을 참조하세요.
또한 Xamarin은 Android Marshmallow(이상)에서 런타임 권한이 작동하는 방식을 보여 주는 샘플 앱을 제공합니다. [RuntimePermissions](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-runtimepermissions).

이 샘플 앱은 다음을 보여 줍니다.

- 런타임에 권한을 확인하고 요청하는 방법.
- Android M 디바이스에서 권한을 선언하는 방법.

이 샘플 앱을 사용하려면

1. **카메라** 또는 **연락처** 단추를 탭하여 권한 요청 대화 상자를 표시합니다.
2. 카메라 또는 연락처 조각을 볼 수 있는 권한을 부여합니다.

Android Marshmallow의 새로운 런타임 권한 기능에 대한 자세한 내용은 [시스템 권한으로 작업](https://developer.android.com/preview/features/runtime-permissions.html)을 참조하세요.

### <a name="authentication-enhancements"></a>인증 기능 향상

Android Marshmallow는 암호의 필요성을 제거하는 데 도움이 되는 두 가지 인증 기능 향상을 포함합니다.

- **지문 인증** &ndash; 지문 스캔을 사용하여 사용자를 인증합니다.

- **자격 증명 확인** &ndash; 디바이스가 잠금 해제된 기간을 기준으로 사용자를 인증합니다.

다음에 설명된 링크 및 샘플 앱을 통해 이러한 새로운 기능을 익힐 수 있습니다.

#### <a name="fingerprint-authentication"></a>지문 인증

지문 스캔 하드웨어를 지원하는 디바이스에서는 새로운 `FingerPrintManager` 클래스를 사용하여 사용자를 인증할 수 있습니다.
Android Marshmallow의 지문 인증 기능에 대한 자세한 내용은 [지문 인증](https://developer.android.com/preview/api-overview.html#fingerprint-authentication)을 참조하세요.

Xamarin은 등록된 지문을 사용하여 앱에서 사용자를 인증하는 방법을 보여 주는 샘플 앱을 제공합니다. [FingerprintDialog](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog).

이 샘플 앱을 사용하려면

1. **구매** 단추를 터치하여 지문 인증 대화 상자를 엽니다.
2. 등록된 지문을 스캔하여 인증합니다.

이 샘플 앱에는 지문 판독기가 포함된 디바이스가 필요합니다.
이 앱은 지문(또는 암호)을 저장하지 않습니다.

#### <a name="voice-interactions"></a>음성 상호 작용

Android Marshmallow에 도입된 새로운 음성 상호 작용 기능을 사용하면 앱 사용자가 음성을 사용하여 작업을 확인하고 옵션 목록에서 선택할 수 있습니다. 음성 상호 작용에 대한 자세한 내용은 [음성 상호 작용 API 개요](https://developers.google.com/voice-actions/interaction/)를 참조하세요. 

Xamarin.Android 앱에서 음성 상호 작용을 구현하는 방법에 대한 자세한 내용(코드 예제 포함)은 [음성 상호 작용을 사용하여 Android 앱에 대화 추가](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/)를 참조하세요.
Xamarin.Android 앱에서 음성 상호 작용 API를 사용하는 방법을 보여 주는 샘플 앱을 사용할 수 있습니다. [음성 상호 작용](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).

#### <a name="confirm-credential"></a>자격 증명 확인

Android Marshmallow의 새로운 *자격 증명 확인* 기능을 사용하면 디바이스가 잠금 해제된 기간에 따라 사용자를 인증할 수 있어 사용자가 앱별 암호를 기억하고 입력할 필요가 없습니다.
이렇게 하려면 `KeyGenerator`의 새로운 `SetUserAuthenticationValidityDurationSeconds` 메서드를 사용합니다. `KeyGuardManager`의 `CreateConfirmDeviceCredentialIntent` 메서드를 사용하여 앱 내에서 사용자를 다시 인증합니다. Android Marshmallow의 이 새로운 기능에 대한 자세한 내용은 [자격 증명 확인](https://developer.android.com/preview/api-overview.html#confirm-credential)을 참조하세요.

Xamarin은 앱에서 디바이스 자격 증명(예: PIN, 패턴 또는 암호)을 사용하는 방법을 보여 주는 샘플 앱을 제공합니다. [ConfirmCredential](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-confirmcredential)

이 샘플 앱을 사용하려면

1. 디바이스에서 보안 잠금 화면을 설정합니다(**보안 > 보안 > 화면 잠금**).
2. **구매** 단추를 탭하고 보안 잠금 화면 자격 증명을 확인합니다.

### <a name="chrome-custom-tabs"></a>Chrome 사용자 지정 탭

앱 개발자는 사용자가 URL을 탭할 때 선택해야 합니다. 앱은 브라우저를 시작할 수도 있고 `WebView`를 기반으로 하는 앱 내 브라우저를 사용할 수도 있습니다. 두 옵션 모두 어려움이 있습니다. 브라우저 시작은 사용자 지정할 수 없는 무거운 컨텍스트 전환이고, `WebView`는 브라우저와 상태를 공유하지 않습니다. 또한 `WebView`를 사용하면 유지 관리 오버 헤드가 추가될 수 있습니다. 

*Chrome 사용자 지정 탭*을 사용하면 사용자가 앱을 떠나지 않게 하면서 Chrome의 기능을 사용하여 웹 사이트를 간편하고 매끄럽게 표시할 수 있습니다. 이 기능을 통해 앱이 사용자의 웹 환경을 더욱 효과적으로 제어할 수 있습니다. `WebView`를 사용하지 않아도 네이티브 콘텐츠와 웹 콘텐츠 간에 더 원활하게 전환할 수 있습니다. 또한 다음을 사용자 지정하여 앱이 Chrome의 모양과 느낌에도 영향을 줄 수 있습니다. 

- 도구 모음 색

- 시작 및 종료 애니메이션

- Chrome 도구 모음 및 오버플로 메뉴의 사용자 지정 작업

- Chrome 사전 시작 및 콘텐츠 프리페치(더 빠른 로드)

Xamarin.Android 앱에서 이 기능을 활용하려면 [Android 지원 사용자 지정 탭 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/)를 다운로드하여 설치합니다.
이 기능에 대한 자세한 내용은 [Chrome 사용자 지정 탭](https://developer.chrome.com/multidevice/android/customtabs)을 참조하세요.

### <a name="material-design-support-library"></a>재질 디자인 지원 라이브러리

Android Lollipop에서 Android 환경을 새로 고칠 수 있는 새로운 디자인 언어로 [재질 디자인](https://www.google.com/design/spec/material-design/introduction.html)이 도입되었습니다(Xamarin.Android 앱에서 재질 디자인 사용에 대한 자세한 내용은 [재질 테마](~/android/user-interface/material-theme.md) 참조). Android Marshmallow에서 Google은 앱 개발자가 재질 디자인 모양과 느낌을 보다 쉽게 채택할 수 있도록 *Android 디자인 지원 라이브러리*를 도입했습니다. 이 라이브러리에는 다음과 같은 구성 요소가 포함되어 있습니다.

- **CoordinatorLayout** &ndash; 새로운 `CoordinatorLayout` 위젯은 `FrameLayout`과 비슷하지만 보다 강력합니다. `CoordinatorLayout`을 자식 보기용 컨테이너 또는 최상위 레이아웃으로 사용할 수 있으며, 다른 보기를 기준으로 보기를 고정하는 데 사용할 수 있는 `layout_anchor` 특성을 제공합니다.

- **축소 도구 모음** &ndash; 새로운 `CollapsingToolbarLayout`은 축소 앱 바로, `Toolbar`에 대한 래퍼입니다. (*앱 바*는 이전에 *작업 모음*이라고 불렀습니다.)

- **부동 작업 단추** &ndash; 앱의 인터페이스에 대한 기본 작업을 표시하는 둥근 단추입니다.

- **텍스트 편집용 부동 레이블** &ndash; 새로운 `TextInputLayout` 위젯(`EditText`를 래핑)을 사용하여 사용자가 텍스트를 입력할 때 힌트가 숨겨질 경우 부동 레이블을 표시합니다.

- **탐색 보기** &ndash; 새로운 `NavigationView` 위젯을 통해 사용자가 보다 쉽게 탐색할 수 있도록 탐색 서랍을 사용할 수 있습니다.

- **스낵바** &ndash; 새로운 `SnackBar` 위젯은 화면 아래쪽에 간단한 메시지를 표시하는 간단한 피드백 메커니즘으로(알림과 비슷함) 화면의 다른 모든 요소 위에 나타납니다.

- **재질 탭** &ndash; 새로운 `TabLayout` 위젯은 앱에서 최상위 탐색을 구현하는 방법으로 탭을 표시하기 위한 가로 레이아웃을 제공합니다.

Xamarin.Android 앱에서 [디자인 지원 라이브러리](https://developer.android.com/tools/support-library/features.html#design)를 활용하려면 Xamarin [Xamarin Support Library Design](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) NuGet 패키지를 다운로드하여 설치합니다.

Xamarin.Android 앱에서 재질 디자인 지원 라이브러리를 사용하는 방법에 대한 자세한 내용은 [Android 지원 디자인 라이브러리를 사용하여 멋진 재질 디자인](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/)을 참조하세요.
Xamarin은 Xamarin.Android의 새로운 Android 디자인 라이브러리를 보여 주는 샘플 앱 [Cheesesquare](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-cheesesquare)를 제공합니다.
이 샘플에서는 다음과 같은 디자인 라이브러리의 기능을 보여 줍니다.

- 축소 도구 모음
- 부동 작업 단추
- 보기 고정
- 탐색 보기
- 스낵바

디자인 라이브러리에 대한 자세한 내용은 Android 개발자 블로그의 [Android 디자인 지원 라이브러리](https://android-developers.googleblog.com/2015/05/android-design-support-library.html)를 참조하세요.

### <a name="additional-library-updates"></a>추가 라이브러리 업데이트

Android Marshmallow 이외에 Google은 여러 핵심 Android 라이브러리와 관련된 업데이트를 발표했습니다. Xamarin은 여러 미리 보기 릴리스 NuGet 패키지를 통해 이러한 업데이트에 대한 Xamarin.Android 지원을 제공합니다. 

- [Google Play 서비스](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; 최신 버전의 Google Play 서비스에는 사용자가 친구와 앱을 공유할 수 있는 새로운 *App Invites* 기능이 포함되어 있습니다. 이 기능에 대한 자세한 내용은 [Google의 App Invites를 사용하여 앱 도달 범위 확장](https://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/)을 참조하세요. 

- [Android 지원 라이브러리](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; 이러한 Nuget은 라이브러리 API 전용 기능을 제공하면서 이전 버전과 호환되는 Android 프레임워크 API 버전도 제공합니다. 

- [Android 웨어러블 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; 이 NuGet에는 Google Play 서비스 바인딩이 포함됩니다. 최신 버전의 웨어러블 라이브러리는 Android Wear 플랫폼에 대한 새로운 기능(사용자 지정 앱의 보다 간편한 탐색 포함)을 제공합니다. 

## <a name="summary"></a>요약

이 문서에서는 Android Marshmallow를 소개하고 Marshmallow에서 Xamarin.Android 개발을 위한 최신 도구 및 패키지를 설치 및 구성하는 방법을 설명했습니다. 또한 Xamarin.Android 개발을 위해 가장 흥미로운 새로운 Android Marshmallow 기능을 간략하게 설명했습니다.

## <a name="related-links"></a>관련 링크

- [Android 6.0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html)
- [Android SDK 가져오기](https://developer.android.com/sdk/index.html#Other)
- [기능 개요](https://developer.android.com/preview/api-overview.html)
- [릴리스 정보](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1.99/index.md)
- [RuntimePermissions(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-runtimepermissions)
- [ConfirmCredential(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-confirmcredential)
- [FingerprintDialog(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
