---
title: Marshmallow 기능
description: 이 문서는에서 Xamarin.ios를 사용 하 여 Android 6.0 Marshmallow 앱 개발을 시작 하는 데 도움이 됩니다.
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 1048d954656152f47509887ed6acf21962a787b2
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510469"
---
# <a name="marshmallow-features"></a>Marshmallow 기능

_이 문서는에서 Xamarin.ios를 사용 하 여 Android 6.0 Marshmallow 앱 개발을 시작 하는 데 도움이 됩니다._

이 문서에서는 android 6.0 Marshmallow의 새로운 기능에 대 한 개요를 제공 하 고 Android Marshmallow 개발용 Xamarin을 준비 하는 방법을 설명 하며, 새 Android Marshmallow를 활용 하는 방법을 설명 하는 샘플 응용 프로그램에 대 한 링크를 제공 합니다. Xamarin Android 앱의 기능. 


## <a name="overview"></a>개요

Android [6.0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html)는 android 롤리팝 후의 다음 주요 android 릴리스입니다.
Xamarin Android는 Android Marshmallow을 지원 하며 다음을 포함 합니다.

-   **API 23/Android 6.0 바인딩** &ndash; Android 6.0는 아래에 설명 된 새 기능에 대 한 많은 새 api를 추가 합니다. api 레벨 23을 대상으로 하는 경우 Xamarin Android 앱에서 이러한 api를 사용할 수 있습니다. Android 6.0 Api에 대 한 자세한 내용은 [android 6.0 api](https://developer.android.com/preview/api-overview.html)를 참조 하세요. 

[![Marshmallow를 실행 하는 태블릿 및 휴대폰의 주인공 이미지](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Marshmallow 릴리스가 주로 "폴란드어 및 품질"에 중점을 두지만 Xamarin Android 개발자에 게 많은 새로운 기능을 제공 합니다. 이러한 기능은 다음과 같습니다. 

-   **런타임 권한** &ndash; 이러한 향상 된 기능을 통해 사용자는 런타임에 사례별로 보안 권한을 승인할 수 있습니다. 

-   **인증 기능 향상** Android Marshmallow부터 앱은 이제 지문 센서를 사용 하 여 사용자를 인증할 수 있으며 새 *자격 증명 확인* 기능은 암호 입력의 필요성을 최소화 합니다. &ndash; 

-   **앱 링크** 이 기능을 사용 하면 앱을 웹 도메인과 자동으로 연결 하 여 **앱 선택** 팝업을 제거할 필요가 없습니다. &ndash; 

-   **직접 공유** 사용자가 빠르고 직관적으로 공유할 수 있도록 *직접 공유 대상을* 정의할 수 있습니다 .이 기능을 사용 하면 사용자에서 다른 앱과 콘텐츠를 공유할 수 있습니다. &ndash; 

-   **음성 상호 작용** &ndash; 이 새 API를 사용 하 여 앱에 대화형 음성 기능을 빌드할 수 있습니다. 

-   **4K 디스플레이 모드** &ndash; Android Marshmallow에서 앱은이를 지 원하는 하드웨어에 대해 4k 디스플레이 해상도를 요청할 수 있습니다. 

-   **새로운 오디오 기능** &ndash; Marshmallow부터 Android는 이제 MIDI 프로토콜을 지원 합니다. 또한 디지털 오디오 캡처 및 재생 개체를 만들기 위한 새로운 클래스를 제공 하 고 오디오와 입력 장치를 연결 하기 위한 새 API 후크를 제공 합니다. 

-   **새 비디오 기능** &ndash; Marshmallow는 앱에서 오디오 및 비디오 스트림을 동기화 하는 데 도움이 되는 새로운 클래스를 제공 합니다 .이 클래스는 동적 재생 속도로도 지원 합니다. 

-   **Android For Work** &ndash; Marshmallow에는 회사 소유의 단일 사용자 장치에 대 한 향상 된 컨트롤이 포함 되어 있습니다. 장치 소유자가 자동으로 앱을 설치 및 제거 하 고, 시스템 업데이트를 자동으로 승인 하 고, 향상 된 인증서 관리, 데이터 사용 추적, 사용 권한 관리 및 작업 상태 알림을 지원 합니다. 

-   **재질 디자인 지원 라이브러리** 새 디자인 *지원 라이브러리* 는 응용 프로그램에 재질 디자인 모양과 느낌을 보다 쉽게 빌드할 수 있게 해 주는 디자인 구성 요소 및 패턴을 제공 합니다. &ndash; 

또한 Android M을 사용 하 여 많은 핵심 Android 라이브러리 업데이트가 출시 되었으며 이러한 업데이트는 android M과 이전 버전의 Android 모두에 새로운 기능을 제공 합니다.

또한 Android Marshmallow를 사용 하 여 많은 핵심 Android 라이브러리 업데이트가 출시 되었으며 이러한 업데이트는 android Marshmallow 및 이전 버전의 Android 모두에 새로운 기능을 제공 합니다. 이 문서에서는 Android Marshmallow를 사용 하 여 앱 빌드를 시작 하는 방법을 설명 하 고 Android 6.0의 새로운 기능에 대 한 개요를 제공 합니다. 

## <a name="requirements"></a>요구 사항

Xamarin 기반 앱에서 새로운 Android Marshmallow 기능을 사용 하려면 다음이 필요 합니다. 

-   Visual Studio 또는 Xamarin Studio를 사용 하 여 **xamarin android** &ndash; xamarin.ios 5.1.7.12 이상을 설치 하 고 구성 해야 합니다.

-   **Mac용 Visual Studio** 또는 **Visual Studio** &ndash; Mac용 Visual Studio를 사용 하는 경우 버전 5.9.7.22 이상이 필요 합니다. Visual Studio를 사용 하는 경우 Visual Studio 용 Xamarin tools 버전이 3.11.1537 이상 필요 합니다. 

-   **Android SDK** &ndash; Android SDK Manager를 통해 Android SDK 6.0 (API 23) 이상을 설치 해야 합니다.

-   **Java 개발자 키트** API 수준 24 이상에 대해 개발 중인 경우 xamarin.ios는 [jdk 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 이상이 필요 합니다 (jdk 1.8은 Marshmallow를 포함 하 여 24 이전 API 수준도 지원). &ndash; 사용자 지정 컨트롤 또는 폼 미리 보기를 사용 하는 경우 64 비트 버전의 JDK 1.8이 필요 합니다.

API 레벨 23 또는 이전 버전을 개발 하는 경우에는 [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) 을 계속 사용할 수 있습니다. 


## <a name="getting-started"></a>시작하기

Android Marshmallow에서 android 사용을 시작 하려면 Android Marshmallow 프로젝트를 만들기 전에 최신 도구 및 SDK 패키지를 다운로드 하 여 설치 해야 합니다. 

1.  **안정적인** 채널에서 최신 Xamarin 업데이트를 설치 합니다. 

2.  Android 6.0 Marshmallow SDK 패키지 및 도구를 설치 합니다.

3.  Android 6.0 Marshmallow (API 레벨 23)를 대상으로 하는 새 Xamarin Android 프로젝트를 만듭니다. 

4.  Android Marshmallow 에뮬레이터 또는 장치를 구성 합니다.

이러한 각 단계는 다음 섹션에 설명 되어 있습니다.


### <a name="install-xamarin-updates"></a>Xamarin 업데이트 설치

Android 6.0 Marshmallow에 대 한 지원이 포함 되도록 Xamarin을 업데이트 하려면 업데이트 채널을 **안정적** 으로 변경 하 고 모든 업데이트를 설치 합니다. 업데이트 채널에서 업데이트를 설치 하는 방법에 대 한 자세한 내용은 [업데이트 채널 변경](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel)을 참조 하세요. 


### <a name="install-the-android-60-sdk"></a>Android 6.0 SDK 설치

Android 용 Xamarin Android Marshmallow 프로젝트를 만들려면 먼저 Android SDK Manager를 사용 하 여 Android 6.0 SDK를 설치 해야 합니다.

-   Mac용 Visual Studio에서 Android SDK Manager를 시작 합니다. (Visual Studio에서 **도구 > SDK manager**를 사용 합니다. Visual Studio에서 **도구 > Android > Android SDK Manager**)를 사용 하 고 최신 Android SDK Tools을 설치 합니다.

    [![Android SDK 관리자에서 Android SDK 도구 선택](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

-   또한 최신 **Android 6.0** SDK 패키지를 설치 합니다.

    [![Android SDK Manager에서 Android 6.0 SDK 패키지 선택](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Android SDK Tools 수정 버전 24.3.4 이상을 설치 해야 합니다.
Android SDK Manager를 사용 하 여 Android 6.0 SDK를 설치 하는 방법에 대 한 자세한 내용은 [SDK Manager](https://developer.android.com/tools/help/sdk-manager.html)를 참조 하세요.



### <a name="start-a-xamarinandroid-project"></a>Xamarin Android 프로젝트 시작

새 Xamarin Android 프로젝트를 만듭니다. Xamarin을 사용 하 여 Android를 처음 사용 하는 경우 android 프로젝트를 만드는 방법에 대해 알아보려면 [Hello, android](~/android/get-started/hello-android/index.md) 를 참조 하세요. 

Android 프로젝트를 만들 때 Android 6.0 MarshMallow를 대상으로 하는 버전 설정을 구성 해야 합니다. Marshmallow에 대 한 프로젝트를 대상으로 하려면 **API 레벨 23 (Xamarin Android v 6.0 지원)** 에 대 한 프로젝트를 구성 해야 합니다. Android API 수준 수준을 구성 하는 방법에 대 한 자세한 내용은 [ANDROID Api 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조 하세요.



### <a name="configure-an-emulator-or-device"></a>에뮬레이터 또는 장치 구성

에뮬레이터를 사용 하는 경우 Android AVD 관리자를 시작 하 고 다음 설정을 사용 하 여 새 장치를 만듭니다.

-   장치: Nexus 5, 6 또는 9.
-   대상: Android 6.0-API 레벨 23
-   ABI: x86

예를 들어이 가상 장치는 Nexus 5를 에뮬레이트하기 위해 구성 됩니다.

[![Nexus 5 장치, Android 6.0 대상 및 Intel Atom (x86)을 사용 하 여 AVD 구성](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Nexus 5, 6 또는 9와 같은 물리적 장치를 사용 하는 경우 Android Marshmallow의 미리 보기 이미지를 설치할 수 있습니다. Android Marshmallow 장치를 업데이트 하는 방법에 대 한 자세한 내용은 [하드웨어 시스템 이미지](https://developer.android.com/preview/download.html#images)를 참조 하세요.



## <a name="new-features"></a>새 기능

Android Marshmallow에 도입 된 변경 내용 중 상당수는 Android 사용자 환경을 개선 하 고, 성능을 향상 시키고, 버그를 수정 하는 데 중점을 두었습니다. 그러나 Marshmallow에는 Android 플랫폼의 기본 사항에 대 한 몇 가지 광범위 한 변경 사항이 도입 되었습니다. 다음 섹션에서는 이러한 향상 된 기능을 중점적으로 설명 하 고 앱에서 새로운 Android Marshmallow 기능을 사용 하 여 시작 하는 데 도움이 되는 링크를 제공 합니다. 



### <a name="runtime-permissions"></a>런타임 권한

Android 사용 권한 시스템은 Android 롤리팝 이후 크게 최적화 되 고 간소화 되었습니다. Android Marshmallow에서 사용자는 설치 시가 아닌 런타임에 사례별로 사용 권한을 부여 합니다. Android Marshmallow 이상에서이 기능을 지원 하려면 런타임에 사용자에 게 권한을 부여 하 라는 메시지를 표시 하도록 앱을 디자인 합니다 (권한이 필요한 컨텍스트). 이렇게 변경 하면 앱을 설치 하 고 업그레이드 하는 프로세스를 간소화 하므로 사용자가 즉시 앱 사용을 보다 쉽게 시작할 수 있습니다. 

Xamarin Android 앱에서 런타임 권한을 구현 하는 방법에 대 한 자세한 내용은 [Android Marshmallow에서 런타임 권한 요청](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) 을 참조 하세요.
또한 Xamarin은 Android Marshmallow (이상)에서 런타임 권한이 작동 하는 방식을 보여 주는 샘플 앱을 제공 합니다. [Runtimepermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions).

이 샘플 앱은 다음을 보여 줍니다.

-   런타임에 권한을 확인 하 고 요청 하는 방법입니다.
-   Android M 장치에 대 한 사용 권한을 선언 하는 방법입니다.

이 샘플 앱을 사용 하려면 다음을 수행 합니다.

1.  **카메라** 또는 **연락처** 단추를 탭 하 여 권한 요청 대화 상자를 표시 합니다.
2.  카메라 또는 연락처 조각을 볼 수 있는 권한을 부여 합니다.

Android Marshmallow의 새로운 런타임 권한 기능에 대 한 자세한 내용은 [시스템 권한으로 작업](https://developer.android.com/preview/features/runtime-permissions.html)을 참조 하세요.



### <a name="authentication-enhancements"></a>향상 된 인증

Android Marshmallow는 암호의 필요성을 제거 하는 데 도움이 되는 두 가지 인증 향상 기능을 포함 합니다.

-   **지문 인증** &ndash; 는 지문 스캔을 사용 하 여 사용자를 인증 합니다.

-   **자격 증명 확인** &ndash; 장치의 잠금이 해제 된 기간을 기준으로 사용자를 인증 합니다.

다음에 설명 된 링크 및 샘플 앱을 통해 이러한 새로운 기능을 익힐 수 있습니다.


#### <a name="fingerprint-authentication"></a>지문 인증

지문 검사 하드웨어를 지 원하는 장치에서는 새 `FingerPrintManager` 클래스를 사용 하 여 사용자를 인증할 수 있습니다.
Android Marshmallow의 지문 인증 기능에 대 한 자세한 내용은 [지문 인증](https://developer.android.com/preview/api-overview.html#fingerprint-authentication)을 참조 하세요.

Xamarin은 등록 된 지문을 사용 하 여 앱에서 사용자를 인증 하는 방법을 보여 주는 샘플 앱을 제공 합니다. [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog).

이 샘플 앱을 사용 하려면 다음을 수행 합니다.

1.  **구입** 단추를 눌러 지문 인증 대화 상자를 엽니다.
2.  등록 된 지문을 검색 하 여 인증 합니다.

이 샘플 앱에는 지문 판독기가 포함 된 장치가 필요 합니다.
이 앱은 지문을 저장 하지 않습니다 (또는 암호).



#### <a name="voice-interactions"></a>음성 상호 작용

Android Marshmallow에 도입 된 새로운 음성 조작 기능을 사용 하면 앱의 사용자가 음성을 사용 하 여 작업을 확인 하 고 옵션 목록에서 선택할 수 있습니다. 음성 상호 작용에 대 한 자세한 내용은 [음성 상호 작용 API 개요](https://developers.google.com/voice-actions/interaction/)를 참조 하세요. 

Xamarin Android 앱에서 음성 상호 작용을 구현 하는 방법에 대 한 자세한 내용은 [음성 상호 작용을 사용 하 여 Android 앱에 대화 추가](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) (코드 예제 포함)를 참조 하세요.
Xamarin Android 앱에서 음성 상호 작용 API를 사용 하는 방법을 보여 주는 샘플 앱을 사용할 수 있습니다. [음성 상호 작용](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).



#### <a name="confirm-credential"></a>자격 증명 확인

Android Marshmallow의 새 *자격 증명 확인* 기능을 사용 하면 사용자가 장치를 잠금 해제 한 기간에 따라 인증 하 여 앱 별 암호를 기억할 필요 없이 비울 수 있습니다.
이렇게 하려면 `SetUserAuthenticationValidityDurationSeconds` `KeyGenerator`의 새 메서드를 사용 합니다. `KeyGuardManager` 의`CreateConfirmDeviceCredentialIntent` 메서드를 사용 하 여 앱 내에서 사용자를 다시 인증 합니다. Android Marshmallow의이 새로운 기능에 대 한 자세한 내용은 [자격 증명 확인](https://developer.android.com/preview/api-overview.html#confirm-credential)을 참조 하세요.

Xamarin은 앱에서 장치 자격 증명 (예: PIN, 패턴 또는 암호)을 사용 하는 방법을 보여 주는 샘플 앱을 제공 합니다. [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

이 샘플 앱을 사용 하려면 다음을 수행 합니다.

1.  장치에서 보안 잠금 화면을 설정 합니다 (보안 **> 보안 > screenlock**).
2.  **구매** 단추를 탭 하 고 보안 잠금 화면 자격 증명을 확인 합니다.



### <a name="chrome-custom-tabs"></a>Chrome 사용자 지정 탭

앱 개발자는 사용자가 URL을 탭 할 때 선택할 수 있습니다. 앱은 브라우저를 시작 하거나을 기반으로 하 `WebView`는 앱 내 브라우저를 사용할 수 있습니다. 두 옵션 모두 브라우저 &ndash; 를 시작 하는 데 문제가 있다는 것은 사용자 지정할 수 없는 `WebView`높은 컨텍스트 전환 이며 s는 브라우저와 상태를 공유 하지 않습니다. 또한 `WebView`s를 사용 하 여 추가 유지 관리 오버 헤드를 추가할 수 있습니다. 

*Chrome 사용자 지정 탭* 을 사용 하면 사용자가 앱을 떠나지 않고도 chrome의 기능을 사용 하 여 웹 사이트를 쉽고 원활 표시할 수 있습니다. 이 기능을 사용 하면 앱이 사용자의 웹 환경을 더욱 효과적으로 제어할 수 있습니다. 를 사용 하지 않고도 `WebView`네이티브 콘텐츠와 웹 콘텐츠를 더 원활 하 게 전환할 수 있습니다. 앱은 다음을 사용자 지정 하 여 Chrome의 모양과 느낌에도 영향을 줄 수 있습니다. 

-   도구 모음 색

-   Enter 및 exit 애니메이션

-   Chrome 도구 모음 및 오버플로 메뉴의 사용자 지정 작업

-   Chrome 사전 시작 및 콘텐츠 사전 인출 (더 빠른 로드)

Xamarin Android 앱에서이 기능을 활용 하려면 [Android Support 사용자 지정 탭 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/)를 다운로드 하 여 설치 합니다.
이 기능에 대 한 자세한 내용은 [Chrome 사용자 지정 탭](https://developer.chrome.com/multidevice/android/customtabs)을 참조 하세요.



### <a name="material-design-support-library"></a>재질 디자인 지원 라이브러리

Android 롤리팝은 Android 환경을 새로 고치기 위한 새로운 디자인 언어로 [자료 디자인](http://www.google.com/design/spec/material-design/introduction.html) 을 도입 했습니다 (Xamarin android 앱에서 재질 디자인 사용에 대 한 자세한 내용은 [재질 테마](~/android/user-interface/material-theme.md) 참조). Android Marshmallow를 사용 하 여 Google은 앱 개발자가 재질 디자인 모양과 느낌을 보다 쉽게 채택할 수 있도록 *Android 디자인 지원 라이브러리* 를 도입 했습니다. 이 라이브러리에는 다음과 같은 구성 요소가 포함 되어 있습니다.

-   **CoordinatorLayout** 새 위젯은와 유사`FrameLayout`하지만 보다 강력 합니다. &ndash; `CoordinatorLayout` 를 자식 보기 `CoordinatorLayout` 의 컨테이너로 사용 하거나 최상위 레이아웃으로 사용할 수 있으며, 다른 뷰를 기준으로 보기를 `layout_anchor` 고정 하는 데 사용할 수 있는 특성을 제공 합니다.

-   **도구 모음 축소** 새은에 대 한 `Toolbar`래퍼입니다. `CollapsingToolbarLayout` &ndash; ( *앱 바* 는 이전에 *작업 모음*이라고 함)

-   **부동 동작 단추** &ndash; 앱의 인터페이스에 대 한 기본 작업을 나타내는 둥근 단추입니다.

-   **텍스트 편집용 부동 레이블** 사용자가 텍스트 `TextInputLayout` 를 입력할 때 힌트가 `EditText`숨겨지는 경우 새 위젯 (래핑)를 사용 하 여 부동 레이블을 표시 합니다. &ndash;

-   **탐색 보기** &ndash; 새`NavigationView` 위젯을 사용 하면 사용자가 쉽게 탐색할 수 있는 방법으로 탐색 서랍을 사용할 수 있습니다.

-   **Snackbar** &ndash; 새`SnackBar` 위젯은 화면 아래쪽에 간단한 메시지를 표시 하는 간단한 피드백 메커니즘 (알림과 유사)으로 화면의 다른 모든 요소 위에 나타납니다.

-   **재질 탭** &ndash; 새`TabLayout` 위젯은 앱에서 최상위 탐색을 구현 하는 방법으로 탭을 표시 하기 위한 가로 레이아웃을 제공 합니다.

Xamarin Android 앱에서 [디자인 지원 라이브러리](https://developer.android.com/tools/support-library/features.html#design) 를 활용 하려면 Xamarin [Xamarin 지원 라이브러리 디자인](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) NuGet 패키지를 다운로드 하 여 설치 합니다.

Xamarin Android 앱에서 재질 디자인 지원 라이브러리를 사용 하는 방법에 대 한 자세한 내용은 [Android 지원 디자인 라이브러리를 사용 하 여 멋진 재질 디자인](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) 을 참조 하세요.
Xamarin은 xamarin.ios &ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare)에서 새 android 디자인 라이브러리를 데모 하는 샘플 앱을 제공 합니다.
이 샘플은 디자인 라이브러리의 다음 기능을 보여 줍니다.


-   축소 도구 모음
-   부동 동작 단추
-   앵커 보기
-   NavigationView
-   Snackbar

디자인 라이브러리에 대 한 자세한 내용은 Android 개발자 블로그의 [Android Design Support library](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) 를 참조 하세요.


### <a name="additional-library-updates"></a>추가 라이브러리 업데이트

Google은 Android Marshmallow 외에도 몇 가지 핵심 Android 라이브러리에 관련 업데이트를 발표 했습니다. Xamarin은 몇 가지 미리 보기 릴리스 NuGet 패키지를 통해 이러한 업데이트에 대 한 Xamarin Android 지원을 제공 합니다. 

-   [Google Play 서비스](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) 최신 버전의 Google Play 서비스에는 사용자가 동료와 앱을 공유할 수 있도록 하는 새로운 *앱 초대* 기능이 포함 되어 있습니다. &ndash; 이 기능에 대 한 자세한 내용은 [Google의 앱 초대를 사용 하 여 앱의 연결 확장](https://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/)을 참조 하세요. 

-   [Android 지원 라이브러리](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; 이러한 nuget는 이전 버전과 호환 되는 버전의 Android framework api를 제공 하는 동안 라이브러리 api에만 사용할 수 있는 기능을 제공 합니다. 

-   [Android Wearable 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; 이 NuGet에는 Google Play 서비스 바인딩이 포함 됩니다. 최신 버전의 wearable 라이브러리는 Android 도입 플랫폼에 대 한 새로운 기능 (사용자 지정 앱에 대 한 간편한 탐색 포함)을 제공 합니다. 


## <a name="summary"></a>요약

이 문서에서는 Android Marshmallow을 소개 하 고 Marshmallow에서 Xamarin.ios 개발용 최신 도구 및 패키지를 설치 하 고 구성 하는 방법을 설명 했습니다. 또한 Xamarin.ios 개발에 대해 가장 흥미로운 새로운 Android Marshmallow 기능에 대 한 개요를 제공 했습니다.


## <a name="related-links"></a>관련 링크

- [Android 6.0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html)
- [Android SDK 가져오기](https://developer.android.com/sdk/index.html#Other)
- [기능 개요](https://developer.android.com/preview/api-overview.html)
- [릴리스 정보](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1.99/index.md)
- [RuntimePermissions (샘플)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [가 나 자격 증명 (샘플)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (샘플)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
