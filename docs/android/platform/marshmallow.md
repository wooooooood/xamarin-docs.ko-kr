---
title: Marshmallow 기능
description: 이 문서에서는 Xamarin.Android를 사용 하 여 Android 6.0 Marshmallow에 대 한 앱 개발을 사용 하 여 시작 합니다.
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 0393b9a994c1fd62f51cff01a88aa73f71019d53
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113458"
---
# <a name="marshmallow-features"></a>Marshmallow 기능

_이 문서에서는 Xamarin.Android를 사용 하 여 Android 6.0 Marshmallow에 대 한 앱 개발을 사용 하 여 시작 합니다._

이 문서에서는 Android 6.0 Marshmallow의 새로운 기능에 간략하게 Xamarin.Android Android Marshmallow 개발, 준비 하는 방법을 설명 하 고 새 Android Marshmallow를 활용 하는 방법을 보여 주는 샘플 응용 프로그램에 대 한 링크를 제공 합니다. Xamarin.Android 앱의 기능입니다. 


## <a name="overview"></a>개요

[Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html), 다음 주요 Android는 Android Lollipop 후 릴리스 합니다.
Xamarin.Android에는 포함 한 Android Marshmallow를 지원 합니다.

-   **API 23/Android 6.0 바인딩을** &ndash; 아래에 설명 된 새 기능에 대 한 많은 새로운 Api를 추가 하는 Android 6.0; API 레벨 23을 대상 하는 경우 이러한 Api는 Xamarin.Android 앱에 사용할 수 있습니다. Android 6.0 Api에 대 한 자세한 내용은 참조 하세요. [Android 6.0 Api](http://developer.android.com/preview/api-overview.html)합니다. 

[![태블릿 및 휴대폰 Marshmallow 실행의 대표 이미지](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

하지만 Marshmallow 릴리스는 "폴란드 및 품질" 주로 중점을 Xamarin.Android 개발자에 게 관심의 여러 새로운 기능은 제공 합니다. 이러한 기능에는 다음이 포함됩니다. 

-   **런타임 권한에** &ndash; 이 향상 된이 기능을 사용 하면 사용자가 런타임에 상황별으로의 보안 권한을 승인할 수 있습니다. 

-   **인증 기능 향상** &ndash; Android Marshmallow 부터는 앱 이제 지문 센서 하 여 사용자 및 새 인증 *자격 증명 확인* 입력의 필요성을 최소화 하는 기능 암호입니다. 

-   **앱 연결** &ndash; 있을 필요가 제거 하려면이 기능을 통해 합니다 **앱 선택** 웹 도메인을 사용 하 여 앱을 자동으로 연결 하 여 팝업 합니다. 

-   **공유를 직접** &ndash; 정의할 수 있습니다 *공유 대상 직접* 하는 공유 빠르고 직관적인 사용자에 대 한;이 기능은 사용자가 다른 앱과 콘텐츠를 공유 하도록 허용 합니다. 

-   **음성 상호 작용** &ndash; 이 새 API 앱에 대화형 음성 기능을 내장할 수 있습니다. 

-   **4 K 디스플레이 모드** &ndash; 에서 Android Marshmallow, 앱 수 요청 디스플레이 해상도 지 원하는 하드웨어에는 4k입니다. 

-   **새 오디오 기능** &ndash; Marshmallow부터 Android 이제 MIDI 프로토콜 지원. 또한 디지털 오디오 캡처 및 재생 개체를 만들려면 새 클래스를 제공 하 고 오디오 및 입력 장치를 연결 하기 위한 새 API 연결을 제공 합니다. 

-   **비디오 기능과** &ndash; Marshmallow 앱 클래스를 새로 렌더링 오디오 및 비디오 스트림 동기화를 제공 하며이 클래스 동적 재생 속도 대 한 지원도 제공 합니다. 

-   **Android for Work** &ndash; Marshmallow 회사 소유의 단일 사용자 장치에 대 한 향상 된 컨트롤을 포함 합니다. 자동 설치를 지원 하 고 장치 소유자가 앱의 자동 승인 시스템 업데이트, 향상 된 인증서 관리, 데이터 사용량 추적, 사용 권한 관리 및 작업 상태 알림을 제거 합니다. 

-   **재질 디자인 지원 라이브러리** &ndash; 새 *디자인 지원 라이브러리* 디자인 구성 요소와 쉽게 앱에 재질 디자인 모양과 느낌을 빌드할 수 있도록 하는 패턴을 제공 합니다. 

또한 많은 핵심 Android 라이브러리 업데이트 Android M과 함께 릴리스된 및 이러한 업데이트는 Android M와 이전 버전의 Android에 대 한 새로운 기능을 제공 합니다.

또한 많은 핵심 Android 라이브러리 업데이트 Android Marshmallow과 함께 릴리스된 및 이러한 업데이트는 이전 버전의 Android 및 Android Marshmallow에 대 한 새로운 기능을 제공 합니다. 이 문서에서는 Android Marshmallow를 사용 하 여 앱 빌드를 시작 하는 방법에 설명 하며 Android 6.0의 새로운 기능의 개요를 강조 표시 합니다. 

## <a name="requirements"></a>요구 사항

다음은 Xamarin 기반 앱에서 새 Android Marshmallow 기능을 사용 하려면 필요 합니다. 

-   **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 또는 나중에 설치 하 고 해야 Visual Studio 또는 Xamarin Studio를 사용 하 여 구성 합니다.

-   **Mac 용 visual Studio** 나 **Visual Studio** &ndash; 버전 5.9.7.22 Mac 용 Visual Studio를 사용 하거나 이상 버전이 필요 합니다. Visual Studio, 3.11.1537 버전을 사용 하는 경우 또는 나중에 Visual Studio 용 Xamarin 도구 필요 합니다. 

-   **Android SDK** &ndash; Android SDK (API 23) 6.0 이상이 있어야 설치 되어 Android SDK Manager를 통해.

-   **Java Developer Kit** &ndash; Xamarin.Android 필요 [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) API 수준 24 개발 하는 경우 이후 또는 큰 (JDK 1.8에서는 API 수준 24 Marshmallow를 포함 하 여 이전). 64 비트 JDK 1.8 버전이 필요한 사용자 지정 컨트롤 또는 폼 미리 보기를 사용 하는 경우입니다.

계속 사용할 수 있습니다 [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API 레벨 23에 맞게 개발 이하의 경우. 


## <a name="getting-started"></a>시작

Android Marshmallow Xamarin.Android와 함께 사용 하 여 시작 하려면 다운로드 하며 Android Marshmallow 프로젝트를 만들려면 먼저 최신 도구 및 SDK 패키지를 설치 합니다. 

1.  최신 Xamarin 업데이트를 설치 합니다 **안정적인** 채널입니다. 

2.  Android 6.0 Marshmallow SDK 패키지 및 도구를 설치 합니다.

3.  Android 6.0 Marshmallow (API 레벨 23)를 대상으로 하는 새 Xamarin.Android 프로젝트를 만듭니다. 

4.  Android Marshmallow에 대 한 에뮬레이터 또는 장치를 구성 합니다.

이러한 각 단계는 다음 섹션에서 설명 됩니다.


### <a name="install-xamarin-updates"></a>Xamarin 업데이트를 설치 합니다.

Xamarin Android 6.0 Marshmallow에 대 한 지원을 포함 하도록 업데이트에 대 한 업데이트 채널 변경 **안정적인** 모든 업데이트를 설치 합니다. 업데이트 채널에서 업데이트를 설치 하는 방법에 대 한 자세한 내용은 참조 하세요. [업데이트 채널 변경](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel)합니다. 


### <a name="install-the-android-60-sdk"></a>Android 6.0 SDK 설치

Android Marshmallow에 대 한 Xamarin.Android 프로젝트를 만들려면 먼저 Android 6.0 SDK를 설치 하려면 Android SDK Manager를 사용 해야 합니다.

-   Android SDK Manager 시작 (Mac 용 Visual Studio에서 사용 하 여 **도구 > SDK Manager**; Visual Studio를 사용 하 여 **도구 > Android > Android SDK Manager**) 및 최신 Android SDK Tools 설치 합니다.

    [![Android SDK Manager에서 Android SDK 도구 선택](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

-   또한 최신 설치 **Android 6.0** SDK 패키지:

    [![Android SDK Manager에서 Android 6.0 SDK 패키지 선택](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Android SDK Tools 버전 24.3.4를 설치 해야 이상.
Android 6.0 SDK를 설치 하려면 Android SDK Manager를 사용 하는 방법에 대 한 자세한 내용은 참조 하세요. [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html)합니다.



### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android 프로젝트를 시작 합니다.

새 Xamarin.Android 프로젝트를 만듭니다. Xamarin 사용한 Android 개발을 처음 접하는 경우 참조 [Hello, Android](~/android/get-started/hello-android/index.md) Android 프로젝트 만들기에 대해 알아보려면 합니다. 

Android 프로젝트를 만들면 대상 Android 6.0 MarshMallow 버전 설정을 구성 해야 합니다. Marshmallow에 대 한 프로젝트를 대상으로 프로젝트를 구성 해야 합니다 **API 레벨 23 (Xamarin.Android v6.0 지원)** 합니다. Android API 수준 수준을 구성 하는 방법에 대 한 자세한 내용은 참조 하세요 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다.



### <a name="configure-an-emulator-or-device"></a>에뮬레이터 또는 장치를 구성 합니다.

에뮬레이터를 사용 하는 경우 Android AVD Manager를 시작 하 고 다음 설정을 사용 하 여 새 장치를 만듭니다.

-   장치: Nexus 5, 6 또는 9입니다.
-   대상: Android 6.0-API 레벨 23
-   ABI: x86

예를 들어,이 가상 장치를 에뮬레이트할 Nexus 5 구성 합니다.

[![Nexus 5 장치, Android 6.0 대상 및 Intel Atom (x86)를 사용 하 여 AVD를 구성 합니다.](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Nexus 5와 같은 물리적 장치를 사용 하는 경우 6 또는 9를 설치할 수 있습니다 Android Marshmallow의 미리 보기 이미지를 합니다. Android Marshmallow에 장치를 업데이트 하는 방법에 대 한 자세한 내용은 참조 하세요. [하드웨어 시스템 이미지](http://developer.android.com/preview/download.html#images)합니다.



## <a name="new-features"></a>새 기능

Android 사용자 환경 개선, 성능, 향상 및 버그 수정에 초점을 두는 다양 한 Android Marshmallow에 도입 된 변경 내용입니다. 그러나 Marshmallow는 Android 플랫폼의 몇 가지 광범위 한 변경도 도입 했습니다. 다음 섹션에서는 이러한 향상 된이 기능을 강조 표시 하 고 새로운 Android Marshmallow 기능을 사용 하 여 앱에서 시작 하는 데 대 한 링크를 제공 합니다. 



### <a name="runtime-permissions"></a>런타임 사용 권한

Android 권한을 시스템 크게 최적화 되었으며 Android Lollipop 이후 간소화 합니다. Android Marshmallow 사용자 설치 시간 런타임에 아닌 경우 사례 별로 사용 권한을 부여 합니다. Android Marshmallow 이상이 기능을 지원 하려면 앱 (에 필요한 사용 권한 있는 컨텍스트)는 런타임 시 사용 권한에 대 한 사용자에 게 프롬프트 디자인할 수 있습니다. 이러한 변경을 통해 앱을 사용 하려면 사용자가 더 쉽게 설치 하 고 응용 프로그램 업그레이드 프로세스를 간소화 하는 것 때문에 즉시 합니다. 

참조 [Android Marshmallow에서 런타임 권한 요청](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) Xamarin.Android 앱에 런타임 권한을 구현 하는 방법에 대 한 자세한 정보 (코드 예제 포함)에 대 한 합니다.
Xamarin Android Marshmallow (이상) 런타임 권한이 작동 하는 방법을 보여 주는 샘플 앱을 제공 합니다. [RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)합니다.

이 샘플 앱에는 다음 방법을 보여 줍니다.

-   확인 하는 방법 및 런타임에 대 한 권한을 요청 합니다.
-   Android M 장치에 대 한 사용 권한을 선언 하는 방법입니다.

사용 하려면이 샘플 앱:

1.  탭의 **카메라** 또는 **연락처** 대화를 요청 하는 단추는 사용 권한을 표시 합니다.
2.  카메라 또는 연락처 조각을 볼 수 있는 권한을 부여 합니다.

Android Marshmallow에 새로운 런타임 권한 기능에 대 한 자세한 내용은 참조 하세요. [시스템 권한으로 작업](https://developer.android.com/preview/features/runtime-permissions.html)합니다.



### <a name="authentication-enhancements"></a>향상 된 인증

Android Marshmallow에 암호에 대 한 필요성을 제거 하는 데 도움이 되는 두 가지 인증 향상 된 기능에 포함 됩니다.

-   **지문 인증** &ndash; 지문 검색을 사용 하 여 사용자를 인증 합니다.

-   **자격 증명 확인** &ndash; 기간 장치의 잠금 해제 되었습니다 기반 사용자를 인증 합니다.

링크와 다음에 설명 된 샘플 앱 수 친숙 한 새 기능이 도움이 됩니다.


#### <a name="fingerprint-authentication"></a>지문 인증

지문 하드웨어 검색을 지 원하는 장치를 사용할 수 있습니다 새 `FingerPrintManager` 사용자를 인증 하는 클래스입니다.
Android Marshmallow에서 지문 인증 기능에 대 한 자세한 내용은 참조 하세요. [지문 인증](https://developer.android.com/preview/api-overview.html#fingerprint-authentication)합니다.

Xamarin은 등록 된 지문을 사용 하 여 앱에서 사용자를 인증 하는 방법을 보여 주는 샘플 앱을 제공 합니다. [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)합니다.

사용 하려면이 샘플 앱:

1.  터치 합니다 **구매** 단추는 지문 인증 대화 상자를 엽니다.
2.  등록 된 지문 인증에서 검색 합니다.

이 샘플 앱 필요 지문 판독기를 사용 하 여 장치를 참고 합니다.
이 앱 지문을 (또는 암호)를 저장 하지 않습니다.



#### <a name="voice-interactions"></a>음성 상호 작용

Android Marshmallow에 도입 된 새 음성 상호 작용 기능은 음성 인식을 옵션 목록에서 선택한 작업을 확인 하는 데 앱의 사용자입니다. 음성 상호 작용에 대 한 자세한 내용은 참조 하세요. [음성 상호 작용 API 개요](https://developers.google.com/voice-actions/interaction/)합니다. 

참조 [음성 상호 작용을 사용 하 여 Android 앱에 대화 추가](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) Xamarin.Android 앱에 음성 상호 작용을 구현 하는 방법에 대 한 자세한 정보 (코드 예제 포함)에 대 한 합니다.
샘플 앱을 사용할 수는 Xamarin.Android 앱에 음성 상호 작용 API를 사용 하는 방법을 보여 주는: [음성 상호 작용](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions)합니다.



#### <a name="confirm-credential"></a>자격 증명 확인

사용 하 여 새 *자격 증명 확인* 기능 Android Marshmallow의 기억 하 고 인증 기간 장치의 잠금 해제 되었습니다 기준으로 하 여 앱 특정 암호를 입력할 필요가 없도록 사용자를 확보할 수 있습니다.
이렇게 하려면 새 사용할 `SetUserAuthenticationValidityDurationSeconds` 메서드는 `KeyGenerator`합니다. 사용 된 `KeyGuardManager`의 `CreateConfirmDeviceCredentialIntent` 다시 앱 내에서 사용자를 인증 하는 방법입니다. Android Marshmallow에서이 새 기능에 대 한 자세한 내용은 참조 하세요. [자격 증명 확인](https://developer.android.com/preview/api-overview.html#confirm-credential)합니다.

Xamarin 앱에서 장치 자격 증명 (예: PIN, 패턴 또는 암호)를 사용 하는 방법을 보여 주는 샘플 앱을 제공 합니다. [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

사용 하려면이 샘플 앱:

1.  장치의 보안 잠금 화면 설정 (**보안 > 보안 > Screenlock**).
2.  탭의 **구매** 단추 및 보안 잠금 화면 자격 증명을 확인 합니다.



### <a name="chrome-custom-tabs"></a>Chrome 사용자 지정 탭

사용자가 URL을 탭 하면 앱 개발자 들을 선택 합니다: 앱 브라우저를 시작 하거나 기반 앱에서 브라우저를 사용 하 여는 `WebView`합니다. 두 옵션 모두 과제가 &ndash; 브라우저를 실행 하는 것은 하는 동안 사용자 지정할 수 없는 과도 한 컨텍스트 스위치 `WebView`의브라우저를 사용 하 여 상태를 공유 하지 않습니다. 또한 사용 `WebView`s 추가 유지 관리 오버 헤드를 추가할 수 있습니다. 

*사용자 지정 탭 chrome* 쉽고 원활 하 게 사용자가 앱을 유지 하지 않고도 Chrome의 기능을 사용 하 여 websites를 표시할 수 있도록 합니다. 이 기능은 앱 보다 잘 제어할 사용자의 웹 환경을; 네이티브 전환을 것에 의존 하지 않고도 더욱 원활 하 게 콘텐츠를 웹 및는 `WebView`합니다. Chrome의 모양과 느낌 다음 사용자 지정 하 여 앱 달라질 수 있습니다. 

-   도구 모음 색

-   입력 하 고 애니메이션 종료

-   Chrome 도구 모음 및 오버플로 메뉴의 사용자 지정 작업

-   Chrome (더 빨리 로드)에 대 한 시작 전 및 콘텐츠 프리페치

Xamarin android 앱에서이 기능을 사용 하려면 다운로드 및 설치 합니다 [Android 지원 사용자 지정 탭 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/)합니다.
이 기능에 대 한 자세한 내용은 참조 하세요. [Chrome 사용자 지정 탭](https://developer.chrome.com/multidevice/android/customtabs)합니다.



### <a name="material-design-support-library"></a>재질 디자인 지원 라이브러리

도입 된 android Lollipop [재료 디자인](http://www.google.com/design/spec/material-design/introduction.html) 언어로 새 디자인 Android 환경 새로 고침 (참조 [재질 테마](~/android/user-interface/material-theme.md) 재질 디자인을 사용 하 여 Xamarin.Android 앱에 대 한 정보에 대 한). Android Marshmallow Google 도입 된 *Android 지원 라이브러리 디자인* 쉽게 앱에 대 한 자료를 채택 하는 개발자는 모양과 느낌을 설계 합니다. 이 라이브러리는 다음 구성 요소를 포함 합니다.

-   **CoordinatorLayout** &ndash; 새 `CoordinatorLayout` 위젯은 비슷하지만 보다 더 강력을 `FrameLayout`입니다. 사용할 수 있습니다 `CoordinatorLayout` 최상위 레이아웃을 하며 또는 자식 뷰에 대 한 컨테이너로 제공을 `layout_anchor` 다른 보기를 기준으로 앵커 보기에 사용할 수 있는 특성입니다.

-   **도구 모음 축소** &ndash; 새 `CollapsingToolbarLayout` 모음인 축소 앱에 대 한 래퍼인 `Toolbar`합니다. (유의 *앱 바* 은 어떤 이전의 라고 했습니다를 *작업 모음*.)

-   **부동 작업 단추** &ndash; 앱의 인터페이스의 기본 동작을 나타내는 원형 단추입니다.

-   **레이블 텍스트 편집에 대 한 부동** &ndash; 새 `TextInputLayout` 위젯 (를 래핑하고 `EditText`) 사용자는 텍스트를 입력 하는 경우 힌트 숨겨지면 부동 레이블을 표시 합니다.

-   **탐색 뷰의** &ndash; 새 `NavigationView` 위젯을 사용 하면 사용자가 탐색 하기 쉬운 방식으로 탐색 서랍을 사용 합니다.

-   **Snackbar** &ndash; 새 `SnackBar` 위젯은 다른 요소가 화면에 나타나는 모든 화면 맨 아래에 간단한 메시지를 표시 하는 간단한 피드백 메커니즘 (알림 유사).

-   **재질 탭** &ndash; 새 `TabLayout` 위젯 앱에서 최상위 탐색을 구현 하는 방법으로 탭을 표시 하기 위한 가로 레이아웃을 제공 합니다.

활용 하는 [디자인 지원 라이브러리](http://developer.android.com/tools/support-library/features.html#design) Xamarin.Android 앱에서 다운로드 하 고 Xamarin 설치 [Xamarin 지원 라이브러리 디자인](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) NuGet 패키지.

참조 [Android 지원 라이브러리 디자인을 사용 하 여 멋진 자료 디자인](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) Xamarin.Android 앱에 재질 디자인 지원 라이브러리를 사용 하는 방법에 대 한 자세한 정보 (코드 예제 포함)에 대 한 합니다.
Xamarin에서는 Xamarin.Android에서 새 Android 디자인 라이브러리를 데모 하는 샘플 앱 &ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare)합니다.
이 샘플에서는 디자인 라이브러리의 다음 기능을 보여 줍니다.


-   도구 모음 축소
-   부동 작업 단추
-   보기 고정
-   NavigationView
-   Snackbar

디자인 라이브러리에 대 한 자세한 내용은 참조 하세요. [Android 지원 라이브러리 디자인](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) Android 개발자의 블로그입니다.


### <a name="additional-library-updates"></a>추가 라이브러리 업데이트

Google Android Marshmallow 외에도 몇 가지 핵심 Android 라이브러리에 관련된 업데이트를 발표 했습니다. Xamarin에서는 몇 가지 미리 보기 릴리스에서 NuGet 패키지를 통해 이러한 업데이트에 대 한 Xamarin.Android 지원을 합니다. 

-   [Google Play Services](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; Google Play Services의 최신 버전을 새 포함 *앱 초대* 기능 앱 친구 들과 공유할 수 있습니다. 이 기능에 대 한 자세한 내용은 참조 하세요. [Google 앱 초대 사용 하 여 확장 하 고 앱의 도달률](http://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/)합니다. 

-   [Android 지원 라이브러리](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; 이러한 Nuget Android 프레임 워크 Api의 이전 버전과 호환 버전을 제공 하는 동안 라이브러리 Api에 사용할 수만 있는 기능을 제공 합니다. 

-   [Android 착용 식 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; 이 NuGet이 Google Play 서비스 바인딩이 포함 되어 있습니다. 착용 식 라이브러리의 최신 Android Wear 플랫폼에 사용자 지정 앱에 대 한 더 쉽게 탐색 등 새로운 기능을 제공 합니다. 


## <a name="summary"></a>요약

이 문서에서는 Android Marshmallow 도입 하 고 설치 및 Marshmallow에서 최신 도구 및 Xamarin.Android 개발에 대 한 패키지를 구성 하는 방법을 설명 합니다. 또한 Xamarin.Android 개발에 대 한 가장 흥미로운 새로운 Android Marshmallow 기능의 개요를 제공 합니다.


## <a name="related-links"></a>관련 링크

- [Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html)
- [Android SDK 가져오기](https://developer.android.com/sdk/index.html#Other)
- [기능 개요](https://developer.android.com/preview/api-overview.html)
- [릴리스 정보](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (샘플)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (샘플)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (샘플)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
