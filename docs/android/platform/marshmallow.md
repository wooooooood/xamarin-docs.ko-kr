---
title: "Marshmallow 기능"
description: "이 문서에서는 Android 6.0 Marshmallow에 대 한 응용 프로그램을 개발 하려면 Xamarin.Android를 사용 하 여 사용 하 여 시작 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: b28ca68701394a8b7b0b543a5ae646910e7c8361
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="marshmallow-features"></a>Marshmallow 기능

_이 문서에서는 Android 6.0 Marshmallow에 대 한 응용 프로그램을 개발 하려면 Xamarin.Android를 사용 하 여 사용 하 여 시작 합니다._

이 문서 Android 6.0 Marshmallow에 대 한 새로운 기능의 개요를 제공 Xamarin.Android Android Marshmallow 개발을 위해 준비 하는 방법에 설명 하 고 새 Android Marshmallow를 사용 하는 방법을 보여 주는 예제 응용 프로그램에 대 한 링크를 제공 합니다. Xamarin.Android 앱의 기능입니다. 

<a name="overview" />

## <a name="overview"></a>개요

[Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html), 다음 주요 Android는 Android 롤리팝 후 릴리스 합니다.
Xamarin.Android Android Marshmallow 한 지원 합니다.

-   **API 23/Android 6.0 바인딩** &ndash; 아래에 설명 된 새로운 기능에 대 한 많은 새로운 Api를 추가 하는 Android 6.0; API 수준 23 대상으로 지정할 경우 이러한 Api는 Xamarin.Android 앱에 사용할 수 있습니다. Android 6.0 Api에 대 한 자세한 내용은 참조 [Android 6.0 Api](http://developer.android.com/preview/api-overview.html)합니다. 

[![태블릿 및 휴대폰 Marshmallow 실행의 이미지 Hero](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png)

Marshmallow 릴리스는 주로 중심으로 "폴란드어 및 품질" 있지만 Xamarin.Android 개발자에 게 관심 있는 여러 가지 새로운 기능도 제공 합니다. 이러한 기능에는 다음이 포함됩니다. 

-   **런타임 권한** &ndash; 이 향상 된이 기능을 사용 하면 런타임 시 상황별으로에 대 한 보안 권한을 승인 하는 사용자에 대 한 수입니다. 

-   **인증 개선** &ndash; Android Marshmallow 부터는 앱 이제 지문 센서를 사용해 인증할 수 사용자 및 새 *자격 증명 확인* 기능에는 입력에 대 한 필요성이 최소화 암호입니다. 

-   **응용 프로그램 연결** &ndash; 있을 필요가 제거 하기 위해이 기능을 통해는 **앱 선택** 팝업 창이 자동으로 응용 프로그램 웹 도메인에 연결 합니다. 

-   **공유를 직접** &ndash; 정의할 수 있습니다 *공유 대상으로 직접* 하는 공유 신속 하 고 직관적인 사용자에 대 한;이 기능을 사용 하면 uers 다른 앱와 콘텐츠를 공유 합니다. 

-   **상호 작용 음성** &ndash; 이 새로운 API를 사용 하면 앱에 대화형 음성 기능을 작성할 수 있습니다. 

-   **4 K 디스플레이 모드** &ndash; 에서 Android Marshmallow, 응용 프로그램을 요청할 수 디스플레이 해상도 지 원하는 하드웨어에 대 한 4k입니다. 

-   **새로운 오디오 기능** &ndash; 부터는 Marshmallow Android 이제 지원 MIDI 프로토콜입니다. 또한 디지털 오디오 캡처 및 재생 개체를 만드는 새 클래스를 제공 하 고 입력 및 오디오 장치를 연결 하기 위한 새 API 후크를 제공 합니다. 

-   **새로운 비디오 기능** &ndash; Marshmallow 앱 도움이 되는 새 클래스 렌더링 오디오 및 비디오 스트림 동기화를 제공 합니다;이 클래스 동적 재생 속도 대 한 지원도 제공 합니다. 

-   **작업에 대 한 android** &ndash; Marshmallow, 단일 사용자를 회사 소유 장치에 대 한 향상 된 컨트롤을 포함 합니다. 자동 설치를 지원 하며 장치 소유자에 의해 응용 프로그램, 시스템 업데이트, 개선 된 인증서 관리, 데이터 사용량 추적, 사용 권한 관리 및 작업 상태 알림을 대 한 자동 동의 제거 합니다. 

-   **자재 디자인 지원 라이브러리** &ndash; 새 *디자인 지원 라이브러리* 디자인 구성 요소와 쉽게 자료 디자인 디자인 응용 프로그램에 빌드할 수 있는 패턴을 제공 합니다. 

또한 많은 코어 Android 라이브러리 업데이트는 Android M에서와 함께 출시 된 및 이러한 업데이트는 Android M와 이전 버전의 Android에 대 한 새로운 기능을 제공 합니다.

또한 많은 코어 Android 라이브러리 업데이트 Android Marshmallow와 함께 출시 된 및 이러한 업데이트는 이전 버전의 Android와 Android Marshmallow에 대 한 새로운 기능을 제공 합니다. 이 문서에서는 Android Marshmallow를 사용 하 여 앱을 구축 하는 방법에 설명 하 고 Android 6.0으로 강조 표시는 새로운 기능의 개요를 제공 합니다. 


<a name="requirements" />

## <a name="requirements"></a>요구 사항

다음은 Xamarin 기반 앱에서 새 Android Marshmallow 기능을 사용 하려면 필요 합니다. 

-   **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 또는 나중에 설치 하 고 해야 Visual Studio 또는 Xamarin Studio를 사용 하 여 구성 합니다.

-   **Mac 용 visual Studio** 또는 **Visual Studio** &ndash; 5.9.7.22 버전, Mac 용 Visual Studio를 사용 하는 하거나 이상 버전이 필요 합니다. Visual Studio, 3.11.1537 버전을 사용 하는 경우 또는 나중의 Visual Studio 용 Xamarin 도구가 필요 합니다. 

-   **Android SDK** &ndash; Android SDK (API 23) 6.0 나중에 설치 되어 있어야 Android SDK Manager를 통해 합니다.

-   **Java 개발자 키트** &ndash; Xamarin.Android 필요 [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 또는 24 API 수준에 대 한 개발 하는 경우 이후 이상 (JDK 1.8 API에서는 수준을 지원 Marshmallow를 포함 하 여 24 보다 이전 버전). JDK 1.8의 64 비트 버전은 사용자 지정 컨트롤 또는 양식 미리 보기를 사용 하는 경우에 필요 합니다.

계속 사용할 수 있습니다 [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API 수준 23에 맞게 개발 이하의 경우. 

<a name="gettingstarted" />

## <a name="getting-started"></a>시작

Android Marshmallow Xamarin.Android를 사용 하 여 시작 하려면 다운로드 하 고 Android Marshmallow 프로젝트를 만들려면 먼저 최신 도구와 SDK 패키지를 설치 해야 합니다. 

1.  최신 Xamarin 업데이트를 설치는 **안정적인** 채널입니다. 

2.  Android 6.0 Marshmallow SDK 패키지 및 도구를 설치 합니다.

3.  Android 6.0 Marshmallow (API 수준 23)를 대상으로 하는 새 Xamarin.Android 프로젝트를 만듭니다. 

4.  Android Marshmallow에 대 한 에뮬레이터 또는 장치를 구성 합니다.

이러한 각 단계는 다음 섹션에서 설명:

<a name="updates" />

### <a name="install-xamarin-updates"></a>Xamarin 업데이트를 설치 합니다.

Xamarin Android 6.0 Marshmallow에 대 한 지원을 포함 하도록를 업데이트 하려면 업데이트 채널을 변경 **안정적인** 하 고 모든 업데이트를 설치 합니다. 업데이트 채널에서 업데이트를 설치 하는 방법에 대 한 자세한 내용은 참조 [변경 업데이트 채널](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/)합니다. 

<a name="sdkpreview" />

### <a name="install-the-android-60-sdk"></a>Android 6.0 SDK 설치

Android Marshmallow에 대 한 Xamarin.Android 프로젝트를 만들려면 먼저 Android 6.0 SDK를 설치 하려면 Android SDK Manager를 사용 해야 합니다.

-   Android SDK Manager를 시작 (Mac 용 Visual Studio에서 사용 하 여 **도구 > SDK Manager**; Visual Studio를 사용 하 여 **도구 > Android > Android SDK Manager**) 하 고 최신 Android SDK 도구 설치:

    [![Android SDK Manager에서 Android SDK 도구 선택](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png)

-   또한 최신 설치 **Android 6.0** SDK 패키지:

    [![Android SDK Manager에서 Android 6.0 SDK 패키지를 선택 하면](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png)

설치한 다음 Android SDK 도구 개정 24.3.4 이상.
Android SDK Manager를 사용 하 여 Android 6.0 SDK를 설치 하는 방법에 대 한 자세한 내용은 참조 [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html)합니다.


<a name="xaproject" />

### <a name="start-a-xamarinandroid-project"></a>Start a Xamarin.Android Project

새 Xamarin.Android 프로젝트를 만듭니다. Xamarin 사용한 Android 개발을 처음 접하는 경우 참조 [Hello, Android](~/android/get-started/hello-android/index.md) Android 프로젝트 만들기에 대 한 자세한 내용은 합니다. 

Android 프로젝트를 만들 때 대상 Android 6.0 MarshMallow에 버전 설정을 구성 해야 합니다. 에 대 한 프로젝트를 구성 해야 Marshmallow에 대 한 프로젝트를 대상으로 **API 수준 23 (Xamarin.Android v6.0 지원)**합니다. Android API 수준 수준을 구성 하는 방법에 대 한 자세한 내용은 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다.


<a name="emudev" />

### <a name="configure-an-emulator-or-device"></a>에뮬레이터 또는 장치를 구성 합니다.

에뮬레이터를 사용 하는 경우 Android AVD Manager를 시작 하 고 다음 설정을 사용 하 여 새 장치를 만듭니다.

-   장치: Nexus 5, 6, 또는 9입니다.
-   대상: Android 6.0 API 수준 23
-   ABI: x86

예를 들어이 가상 장치를 Nexus 5 에뮬레이션 하기 위해 구성 합니다.

[![AVD Nexus 5 장치, Android 6.0 대상 및 Intel Atom (x86)를 사용 하 여 구성](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png)

Nexus 5와 같은 물리적 장치를 사용 하는 경우 또는 9, 6, 설치할 수 있습니다 Android Marshmallow의 미리 보기 이미지입니다. Android Marshmallow에 장치를 업데이트 하는 방법에 대 한 자세한 내용은 참조 [하드웨어 시스템 이미지](http://developer.android.com/preview/download.html#images)합니다.


<a name="newfeatures" />

## <a name="new-features"></a>새 기능

Android 사용자 환경을 개선 하 고, 성능, 증가, 버그 수정에 포커스가 있는 많은 Android Marshmallow에 도입 된 변경 합니다. 그러나 Marshmallow는 Android 플랫폼의 기본 사항에 광범위 한 일부 변경도 도입 했습니다. 다음 섹션에서는 이러한 향상 된이 기능을 강조 표시 하 고 새로운 Android Marshmallow 기능을 사용 하 여 응용 프로그램에서 시작 하는 데 유용한 링크를 제공 합니다. 


<a name="permissions" />

### <a name="runtime-permissions"></a>런타임 사용 권한

Android 권한 시스템 크게 액세스에 최적화 된 및 Android 롤리팝 이후 간소화 되었습니다. Android Marshmallow 사용자 설치 시간을 런타임 시 아닌 경우-별로 사용 권한을 부여 합니다. Android Marshmallow 이상이 기능을 지원 하려면 응용 프로그램 (의 컨텍스트에서 사용 권한을 가져야) 런타임 시 사용 권한에 대 한 사용자에 게 프롬프트를 디자인 합니다. 이러한 변경을 통해 응용 프로그램을 사용 하려면 사용자가 쉽게 설치 하 고 응용 프로그램을 업그레이드 하는 프로세스를 간소화 하기 때문에 즉시 합니다. 

참조 [Android Marshmallow에 런타임 권한 요청](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) Xamarin.Android 앱에서 런타임 권한을 구현 하는 방법에 대 한 자세한 내용은 (코드 예제가 포함).
Xamarin Android Marshmallow (이상) 런타임 권한이 작동 하는 방법을 보여 주는 샘플 응용 프로그램 제공: [RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)합니다.

이 샘플 응용 프로그램에는 다음 방법을 보여 줍니다.

-   확인 하는 방법 및 런타임에 사용 권한을 요청 합니다.
-   Android M 장치에 대 한 사용 권한을 선언 하는 방법입니다.

사용 하려면이 샘플 응용 프로그램:

1.  탭의 **카메라** 또는 **연락처** 대화를 요청 하는 단추는 사용 권한을 표시 합니다.
2.  카메라 또는 연락처 조각을 볼 수 있는 권한을 부여 합니다.

Android Marshmallow에 새로운 런타임 권한 기능에 대 한 자세한 내용은 참조 [시스템 권한으로 작업](https://developer.android.com/preview/features/runtime-permissions.html)합니다.


<a name="authentication" />

### <a name="authentication-enhancements"></a>인증의 향상 된 기능

Android Marshmallow 포함 암호에 대 한 필요성을 제거 하는 데 도움이 되는 두 가지 인증 기능이 향상이 됩니다.

-   **인증 지문** &ndash; 지문 검색을 사용 하 여 사용자를 인증 합니다.

-   **자격 증명 확인** &ndash; 기간 장치 ॉ क에 따라 사용자를 인증 합니다.

링크와 다음에 설명 하는 샘플 응용 프로그램 수에 익숙해질 수 이러한 새로운 기능을 사용 합니다.


<a name="fingerprint" />

#### <a name="fingerprint-authentication"></a>지문 인증

지문 검색 하는 하드웨어를 지 원하는 장치에서 사용할 수 있습니다 새 `FingerPrintManager` 사용자를 인증 하는 클래스입니다.
Android Marshmallow에는 지문 인증 기능에 대 한 자세한 내용은 참조 [지문 인증](https://developer.android.com/preview/api-overview.html#fingerprint-authentication)합니다.

Xamarin에서는 등록 된 지문을 사용 하 여 응용 프로그램에서 사용자를 인증 하는 방법을 보여 주는 샘플 응용 프로그램: [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)합니다.

사용 하려면이 샘플 응용 프로그램:

1.  터치는 **구매** 단추 지문 인증 대화 상자를 엽니다.
2.  인증 하 여 등록 된 지문을 검색 합니다.

이 샘플 응용 프로그램 지문 판독기를 사용 하 여 장치 필요 하다는 참고 합니다.
이 앱에는 지문 (또는 암호) 저장 하지 않습니다.


<a name="voice" />

#### <a name="voice-interactions"></a>음성 상호 작용

Android Marshmallow에 도입 된 새 음성의 상호 작용 기능은 사용자가 응용 프로그램의 동작을 확인 하 고 옵션의 목록에서 선택할 음성을 사용 하 합니다. 음성 상호 작용 하는 방법에 대 한 자세한 내용은 참조 [음성 상호 작용 API 개요](https://developers.google.com/voice-actions/interaction/)합니다. 

참조 [대화 음성 상호 작용 사용 하 여 Android 앱에 추가](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) Xamarin.Android 앱에서 음성 상호 작용을 구현 하는 방법에 대 한 자세한 내용은 (코드 예제가 포함).
샘플 앱은 사용할 수 있는 음성 상호 작용 API Xamarin.Android 응용 프로그램에서 사용 하는 방법을 보여 주는: [음성 상호 작용](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions)합니다.


<a name="confirmcred" />

#### <a name="confirm-credential"></a>자격 증명 확인

사용 하 여 새 *자격 증명 확인* 기능 Android Marshmallow의 기억 하 고 시간 장치 ॉ क에 따라 인증 하 여 앱 별 암호를 입력 하지 않아도 사용자가 확보할 수 있습니다.
이 작업을 수행 하려면 새로운 사용 `SetUserAuthenticationValidityDurationSeconds` 의 메서드는 `KeyGenerator`합니다. 사용 하 여는 `KeyGuardManager`의 `CreateConfirmDeviceCredentialIntent` 메서드를 다시 응용 프로그램 내에서 사용자를 인증 합니다. Android Marshmallow의이 새로운 기능에 대 한 자세한 내용은 참조 [자격 증명 확인](https://developer.android.com/preview/api-overview.html#confirm-credential)합니다.

Xamarin 앱에서 장치 자격 증명 (예: PIN, 패턴 또는 암호)를 사용 하는 방법을 보여 주는 샘플 응용 프로그램에서는: [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

사용 하려면이 샘플 응용 프로그램:

1.  장치에서 보안 잠금 화면 설정 (**Secure > 보안 > Screenlock**).
2.  탭의 **구매** 단추 및 보안 잠금 화면 자격 증명을 확인 합니다.


<a name="chrometabs" />

### <a name="chrome-custom-tabs"></a>크롬 사용자 지정 탭

사용자가 URL을 누를 때 선택 하는 응용 프로그램 개발자에 맞서게: 응용 프로그램 브라우저를 시작 하거나 기반으로 하는 앱에서 브라우저를 사용 하 여 한 `WebView`합니다. 두 옵션 모두 해결 과제가 &ndash; 브라우저를 실행 하는 것은 하는 동안 사용자 지정할 수 없는 많은 컨텍스트 스위치 `WebView`의브라우저와 상태를 공유 하지 않습니다. 또한 사용 하 여의 `WebView`s 추가 유지 관리 오버 헤드를 추가할 수 있습니다. 

*사용자 지정 탭 크롬* 사용 하면 쉽고 원활 하 게 강력한 크롬을 통해 웹 사이트 사용자가 앱을 유지 하지 않고 표시할 수 있습니다. 이 기능을 더 많이 제어할 사용자의 웹 경험; 앱 사용 네이티브 간의 전환 만들므로에 의존 하지 않고 웹 콘텐츠 보다 원활한 및는 `WebView`합니다. 앱 크롬 모양과 다음 사용자 지정 하 여 느낌에 영향을 줄 수 있습니다. 

-   도구 모음 색

-   입력 하 고 애니메이션 종료

-   크롬 도구 모음 및 오버플로 메뉴에서 사용자 지정 작업

-   크롬 (로드 속도 빠르게)에 대 한 시작 전 및 콘텐츠 프리페치

를 사용 하려면이 기능의 Xamarin.Android 앱에서 다운로드 및 설치는 [Android 지원 사용자 지정 탭 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/)합니다.
이 기능에 대 한 자세한 내용은 참조 [크롬 사용자 지정 탭](https://developer.chrome.com/multidevice/android/customtabs)합니다.


<a name="designlib" />

### <a name="material-design-support-library"></a>자재 디자인 지원 라이브러리

Android 롤리팝 도입 [자료 디자인](http://www.google.com/design/spec/material-design/introduction.html) Android 환경을 새로 고치려면 새로운 디자인 언어로 (참조 [자료 테마](~/android/user-interface/material-theme.md) 자재 디자인 Xamarin.Android 앱에서 사용 하는 방법에 대 한 정보에 대 한). Android Marshmallow Google 도입 했는데이 *Android 디자인 지원 라이브러리* 자료를 채택 하는 개발자는 쉽게 앱에 대 한 모양 및 느낌을 설계 합니다. 이 라이브러리에는 다음 구성 요소가 포함 됩니다.

-   **CoordinatorLayout** &ndash; 새 `CoordinatorLayout` 위젯은 유사 하지만 보다 더 강력 하는 한 `FrameLayout`합니다. 사용할 수 있습니다 `CoordinatorLayout` 자식 뷰에 대 한 컨테이너 또는 최상위 레이아웃으로 제공 된 `layout_anchor` 다른 보기를 기준으로 앵커 보기에 사용할 수 있는 특성입니다.

-   **도구 모음을 축소** &ndash; 새 `CollapsingToolbarLayout` 모음인 축소 응용 프로그램에 대 한 래퍼인 `Toolbar`합니다. (유의 *앱 바* 된 이전으로 지칭 되는 *작업 모음*.)

-   **작업 단추 부동** &ndash; 응용 프로그램의 인터페이스에 대 한 기본 동작을 나타내는 라운드 단추입니다.

-   **텍스트 편집에 대 한 레이블을 부동** &ndash; 사용 하 여 새 `TextInputLayout` 위젯 (를 래핑하고 `EditText`) 사용자는 텍스트를 입력 하는 경우 힌트는 숨겨져 부동 레이블을 표시 하려면.

-   **탐색 뷰의** &ndash; 새 `NavigationView` 위젯을 사용 하면 사용자가 탐색 하기 더 쉬운 방식으로 탐색 서랍을 사용 합니다.

-   **Snackbar** &ndash; 새 `SnackBar` 위젯은 다른 요소를 화면에 나타나는 모든 화면 맨 아래에 대 한 간단한 메시지를 표시 하는 간단한 피드백 메커니즘 (알림 메시지와 유사).

-   **자재 탭** &ndash; 새 `TabLayout` 위젯 응용 프로그램에서 최상위 탐색을 구현 하는 방법으로 탭을 표시 하는 것에 대 한 가로 레이아웃을 제공 합니다.

활용 하기 위해는 [디자인 지원 라이브러리](http://developer.android.com/tools/support-library/features.html#design) Xamarin.Android 앱에서 다운로드 하 고 Xamarin 설치 [Xamarin 지원 라이브러리 디자인](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) NuGet 패키지 합니다.

참조 [Android 지원 디자인 라이브러리와 아름 다운 자료 디자인](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) Xamarin.Android 앱에서 자료 디자인 지원 라이브러리를 사용 하는 방법에 대 한 자세한 내용은 (코드 예제가 포함).
Xamarin에서는 새 Android 디자인 라이브러리 Xamarin.Android에서 데모 샘플 응용 프로그램 &ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare)합니다.
이 샘플에서는 디자인 라이브러리의 다음 기능을 보여 줍니다.


-   도구 모음을 축소
-   부동 작업 단추
-   보기 고정
-   NavigationView
-   Snackbar

디자인 라이브러리에 대 한 자세한 내용은 참조 [Android 디자인 지원 라이브러리](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) Android 개발자 블로그에 있습니다.


<a name="libraries" />

### <a name="additional-library-updates"></a>추가 라이브러리 업데이트

Google Android Marshmallow 외에도 여러 가지 코어 Android 라이브러리에 대 한 관련된 업데이트를 발표 했습니다. Xamarin에는 몇 가지 미리 보기 릴리스 NuGet 패키지를 통해 이러한 업데이트에 대 한 Xamarin.Android 지원을 제공합니다. 

-   [Google Play 서비스](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; 새 Google Play 서비스의 최신 버전에 포함 되어 *앱 초대* 기능 앱 친구와 공유할 수 있습니다. 이 기능에 대 한 자세한 내용은 참조 [Google의 App 초대를 확장 하 고 응용 프로그램의 도달 범위](http://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/)합니다. 

-   [Android 지원 라이브러리](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; 이러한 NuGets Android 프레임 워크 Api의 이전 버전과 호환 버전을 제공 하는 동안만 라이브러리 Api에 사용할 수 있는 기능을 제공 합니다. 

-   [Android 착용 식 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; 이 NuGet Google Play 서비스 바인딩이 포함 되어 있습니다. 착용 식 라이브러리의 최신 버전을 쓰는 유형 Android 플랫폼 (사용자 지정 앱에 대 한 쉽게 탐색할 수 있도록 포함)는 새로운 기능을 제공 합니다. 


## <a name="summary"></a>요약

이 문서는 Android Marshmallow 도입 하 고 설치 하 고 Marshmallow에 있는 최신 도구와 Xamarin.Android 개발에 대 한 패키지를 구성 하는 방법을 설명 합니다. 또한 Xamarin.Android 개발을 위해 가장 흥미로운 새 Android Marshmallow 기능의 개요를 제공 합니다.


## <a name="related-links"></a>관련 링크

- [Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html)
- [Android SDK 가져오기](https://developer.android.com/sdk/index.html#Other)
- [기능 개요](https://developer.android.com/preview/api-overview.html)
- [릴리스 정보](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (샘플)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (샘플)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (샘플)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
