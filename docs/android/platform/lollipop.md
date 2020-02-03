---
title: 롤리팝 기능
description: 이 문서에서는 Android 5.0 (롤리팝)에 도입 된 새로운 기능에 대 한 개략적인 개요를 제공 합니다. 이러한 기능에는 재질 테마 라는 새로운 사용자 인터페이스 스타일 뿐만 아니라 애니메이션, 그림자, 그릴 만한 색조 등의 새로운 지원 기능도 포함 됩니다. Android 5.0에는 저장소, 네트워킹, 연결 및 멀티미디어 기능을 개선 하기 위해 향상 된 알림, 두 가지 새로운 UI 위젯, 새 작업 스케줄러 및 몇 가지 새로운 Api가 포함 되어 있습니다.
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 297c7806ce8a880d65c38ef0e4672e41fee5acfe
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724444"
---
# <a name="lollipop-features"></a>롤리팝 기능

_이 문서에서는 Android 5.0 (롤리팝)에 도입 된 새로운 기능에 대 한 개략적인 개요를 제공 합니다. 이러한 기능에는 재질 테마 라는 새로운 사용자 인터페이스 스타일 뿐만 아니라 애니메이션, 그림자, 그릴 만한 색조 등의 새로운 지원 기능도 포함 됩니다. Android 5.0에는 저장소, 네트워킹, 연결 및 멀티미디어 기능을 개선 하기 위해 향상 된 알림, 두 가지 새로운 UI 위젯, 새 작업 스케줄러 및 몇 가지 새로운 Api가 포함 되어 있습니다._

## <a name="lollipop-overview"></a>롤리팝 개요

Android 5.0 (롤리팝)에는 새로운 디자인 언어, *자재 디자인*및 새로운 기능의 지원 캐스트를 도입 하 여 앱을 더 쉽고 직관적으로 사용할 수 있도록 하는 새로운 기능이 도입 되었습니다. 재질 디자인을 사용 하는 경우 Android 5.0는 Android 휴대폰에 facelift 수 있습니다. 또한 Android 기반 태블릿, 데스크톱 컴퓨터, 감시 및 스마트 Tv에 대 한 새로운 디자인 규칙 집합을 제공 합니다. 이러한 디자인 규칙은 사용자가 인터페이스를 빠르고 직관적으로 이해할 수 있도록 친숙 한 tactile 특성 (예: 사실적인 표면 및에 지 큐)을 활용 하는 동시에 간단 하 고 minimalism 강조 합니다.

*재질 테마* 는 Android에서 이러한 UI 디자인 원칙을 전형는 것입니다. 이 문서에서는 먼저 재질 테마의 지원 기능을 다룹니다.

- *터치 피드백* 애니메이션, *작업 전환* 애니메이션, *뷰 상태 전환* 애니메이션 및 표시 *효과*를 &ndash; **애니메이션** 입니다.

- **그림자 및 권한 상승** &ndash; 보기에는 이제 `elevation` 속성이 있습니다.   `elevation` 값이 높은 뷰는 배경에서 더 큰 그림자를 캐스팅 합니다.

- 그릴 수 있는 *색조* &ndash; **색 기능** 을 사용 하면 색을 변경 하 여 이미지 자산을 다시 사용할 수 있으며, *강조 된 색 추출을* 사용 하면 이미지의 색을 기반으로 하 여 앱에 동적으로 테마를 지정할 수 있습니다.

대부분의 재질 테마 기능은 Android 5.0 UI 환경에 이미 내장 되어 있지만 앱에 명시적으로 추가 해야 하는 경우도 있습니다. 예를 들어 일부 표준 보기 (예: 단추)에는 터치 피드백 애니메이션이 이미 포함 되어 있지만 앱에서 대부분의 보기 그림자를 사용 하도록 설정 해야 합니다.

Android 5.0에는 재질 테마를 통해 제공 되는 UI 개선 사항 외에도이 문서에서 설명 하는 여러 가지 새로운 기능이 포함 되어 있습니다.

- **향상 된 알림** &ndash; Android 5.0의 알림은 새로운 디자인, 잠금 화면 알림 지원 및 새로운 *준비* 알림 프레젠테이션 형식으로 크게 업데이트 되었습니다.

- 새 `RecyclerView` 위젯을 &ndash; **새 UI 위젯을** 사용 하면 앱이 쉽게 많은 데이터 집합 및 복잡 한 정보를 전달할 수 있으며, 새로운 `CardView` 위젯은 텍스트 및 이미지를 표시 하기 위한 간단한 카드 모양의 프레젠테이션 형식을 제공 합니다.

- **새 api** &ndash; Android 5.0은 여러 네트워크 지원, 향상 된 Bluetooth 연결, 더 쉬운 저장소 관리 및 멀티미디어 플레이어와 카메라 장치를 유연 하 게 제어 하는 새로운 api를 추가 합니다. 새 작업 예약 기능을 사용 하 여 예약 된 시간에 작업을 비동기적으로 실행할 수 있습니다. 이 기능을 사용 하면 장치가 연결 되 고 충전 될 때 작업을 예약 하는 등의 방법으로 배터리 수명을 향상할 수 있습니다.

## <a name="requirements"></a>요구 사항

Xamarin 기반 앱에서 새로운 Android 5.0 기능을 사용 하려면 다음이 필요 합니다.

- Visual Studio 또는 Mac용 Visual Studio를 사용 하 여 **xamarin.ios &ndash; xamarin. android 4.20** 이상 버전을 설치 하 고 구성 해야 합니다.

- Android SDK Manager를 통해 **Android SDK** &ndash; Android 5.0 (API 21) 이상을 설치 해야 합니다.

- Xamarin.ios &ndash; **Java 개발자 키트** 를 사용 하려면 api 수준 24 이상에 대해 개발 하는 경우 jdk [1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 이상이 필요 합니다 (jdk 1.8은 롤리팝을 비롯 하 여 24 이전 API 수준도 지원 함). 사용자 지정 컨트롤 또는 폼 미리 보기를 사용 하는 경우 64 비트 버전의 JDK 1.8이 필요 합니다.

API 레벨 23 또는 이전 버전을 개발 하는 경우에는 [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) 을 계속 사용할 수 있습니다.

## <a name="setting-up-an-android-50-project"></a>Android 5.0 프로젝트 설정

Android 5.0 프로젝트를 만들려면 최신 도구 및 SDK 패키지를 설치 해야 합니다. Android 5.0를 대상으로 하는 Xamarin Android 프로젝트를 설정 하려면 다음 단계를 수행 합니다.

1. Xamarin Android 도구를 설치 하 고 Xamarin 라이선스를 활성화 합니다. Xamarin을 설치 하는 방법에 대 한 자세한 내용은 [설정 및 설치](~/android/get-started/installation/index.md) 를 참조 하세요.

2. Mac용 Visual Studio를 사용 하는 경우 최신 Android 5.0 업데이트를 설치 합니다.

3. Mac용 Visual Studio에서 Android SDK 관리자를 시작 하 고, 도구를 사용 하 여 **Android SDK manager&hellip;) &gt; 열고** , Android SDK Tools 23.0.5 이상을 설치 합니다.

    [Android SDK Manager에서 Android SDK 도구를 선택 ![](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   또한 최신 Android 5.0 SDK 패키지 (API 21 이상)를 설치 합니다.

    [Android SDK Manager에 Android 5.0 SDK 패키지를 설치 ![](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   Android SDK Manager를 사용 하는 방법에 대 한 자세한 내용은 [SDK Manager](https://developer.android.com/tools/help/sdk-manager.html)를 참조 하세요.

4. 새 Xamarin Android 프로젝트를 만듭니다. Xamarin을 사용 하 여 Android를 처음 사용 하는 경우 android 프로젝트를 만드는 방법에 대해 알아보려면 [Hello, android](~/android/get-started/hello-android/index.md) 를 참조 하세요. Android 프로젝트를 만들 때 Android 5.0에 대 한 버전 설정을 구성 해야 합니다.
   Mac용 Visual Studio에서 프로젝트 옵션으로 이동 하 **&gt; &gt; 일반 빌드** 를 선택 하 고 **대상 프레임 워크** 를 **Android 5.0 (롤리팝)** 이상으로 설정 합니다.

    ![대상 Framwework를 Android 5.0 롤리팝으로 설정](lollipop-images/target-framework.png)

   **프로젝트 옵션 &gt; 빌드 &gt; Android 응용 프로그램**에서 최소 및 대상 Android 버전을 **자동 사용 대상 프레임 워크 버전**으로 설정 합니다.

    ![최소 및 대상 Android 버전을 자동으로 설정](lollipop-images/minimum-android-version.png)

5. 에뮬레이터 또는 Android 장치를 구성 하 여 앱을 테스트 합니다. 에뮬레이터를 사용 하는 경우 Xamarin Studio 또는 Visual Studio에서 사용할 Android 에뮬레이터를 구성 하는 방법을 알아보려면 [설치 Android Emulator](~/android/get-started/installation/android-emulator/index.md) 를 참조 하세요. Android 장치를 사용 하는 경우 Android 5.0에 대 한 장치를 업데이트 하는 방법을 알아보려면 [PREVIEW SDK 설정](https://developer.android.com/preview/setup-sdk.html) 을 참조 하세요. Xamarin Android 응용 프로그램을 실행 하 고 디버깅 하기 위해 Android 장치를 구성 하려면 [개발용 장치 설정](~/android/get-started/installation/set-up-device-for-development.md)을 참조 하세요.

참고: Android L Preview를 대상으로 하는 기존 Android 프로젝트를 업데이트 하는 경우 **대상 프레임 워크** 및 **android 버전** 을 위에서 설명한 값으로 업데이트 해야 합니다.

## <a name="important-changes"></a>중요 한 변경 내용

이전에 게시 된 Android 앱은 Android 5.0 변경의 영향을 받을 수 있습니다. 특히 Android 5.0는 새로운 런타임과 상당한 변경 된 알림 형식을 사용 합니다.

### <a name="android-runtime"></a>Android 런타임

Android 5.0에서는 새 Android Runtime (ART)을 기본 런타임으로 사용 합니다. 아트는 다음과 같은 몇 가지 주요 새로운 기능을 구현 합니다.

- **AOT (사전) 컴파일** &ndash; AOT는 앱이 처음 시작 되기 전에 앱 코드를 컴파일하여 앱 성능을 향상 시킬 수 있습니다. 앱이 설치 되 면 ART는 대상 장치에 대 한 컴파일된 앱 실행 파일을 생성 합니다.

- **향상 된 gc (가비지 수집)** &ndash;의 gc 개선으로 앱 성능을 향상 시킬 수도 있습니다. 이제 가비지 컬렉션은 두 가지 대신 하나의 GC 일시 중지를 사용 하 고 동시 GC 작업을 보다 시기 적절 한 방식으로 완료 합니다.

- **향상 된 앱 디버깅** &ndash; 아트에서는 예외 및 충돌 보고서를 분석 하는 데 도움이 되는 진단 세부 정보를 제공 합니다.

기존 앱은 art에서 작동 하지 않을 수 있는 이전 Dalvik 런타임에 고유한 기술을 활용 하는 앱을 제외 하 고는 &ndash;에서 변경 하지 않고 작동 해야 합니다. 이러한 변경에 대 한 자세한 내용은 [Android Runtime에서 앱 동작 확인 (ART)](https://developer.android.com/guide/practices/verifying-apps-art.html)을 참조 하세요.

### <a name="notification-changes"></a>알림 변경

Android 5.0에서 알림이 크게 변경 되었습니다.

- 경고음 및 **진동은 &ndash; 서로 다르게 처리** 되며 vibrations 이제 `Ringtone`, `MediaPlayer`, `Vibrator`대신 `Notification.Builder`에서 처리 됩니다.

- **새 색 구성표** &ndash; 재질 테마에 따라 흰색 또는 매우 밝은 배경의 어두운 텍스트를 사용 하 여 알림이 렌더링 됩니다. 또한 알림 아이콘의 알파 채널은 시스템 색 구성표를 사용 하 여 조정 하기 위해 Android에서 수정할 수 있습니다.

- 이제 **잠금 화면 알림** &ndash; 알림이 장치 잠금 화면에 표시 될 수 있습니다.

- 장치를 잠금 해제 하 고 화면이 켜 졌으 면 이제 높은 우선 순위 알림이 작은 부동 창 (준비 알림)에 표시 됩니다 **. &ndash;**

대부분의 경우 기존 앱 알림 기능을 Android 5.0로 이식 하려면 다음 단계를 수행 해야 합니다.

1. 알림을 만들기 위해 `Notification.Builder` 또는 `NotificationsCompat.Builder`를 사용 하도록 코드를 변환 합니다.

2. 기존 알림 자산이 새 재질 테마 색 구성표에 표시 되는지 확인 합니다.

3. 알림이 잠금 화면에 표시 될 때 표시 되는 표시 유형을 결정 합니다. 알림이 공용이 아니면 잠금 화면에 표시 되어야 하는 콘텐츠는 무엇 인가요?

4. 새 Android 5.0 *안 함* 모드에서 올바르게 처리 되도록 알림의 범주를 설정 합니다.

알림이 전송 제어를 제공 하거나, 미디어 재생 상태를 표시 하거나, `RemoteControlClient`를 사용 하거나, `ActivityManager.GetRecentTasks`를 호출 하는 경우 Android 5.0에 대 한 알림 업데이트에 대 한 자세한 내용은 [중요 한 동작 변경 내용](https://developer.android.com/preview/api-overview.html#Behaviors) 을 참조 하세요.

Android에서 알림을 만드는 방법에 대 한 자세한 내용은 [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)을 참조 하세요.

## <a name="material-theme"></a>재질 테마

새 Android 5.0 재질 테마는 Android UI의 모양과 느낌을 변경 합니다. 시각적 요소는 이제 인쇄 기반 디자인의 굵은 그래픽, 입력 체계 및 밝은 색을 사용 하는 tactile 표면을 사용 합니다. 재질 테마의 예는 다음 스크린샷에 나와 있습니다.

[재질 테마 홈 화면, 앱 화면 및 설정 화면 ![스크린샷](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0는 왼쪽에 표시 되는 홈 화면을 인사말 합니다. 센터 스크린샷은 앱 목록의 첫 번째 화면 이며 오른쪽의 스크린샷은 **설정** 화면입니다. Google의 [재질 디자인](https://material.io/guidelines/material-design/introduction.html) 사양은 새 재질 테마 개념의 기본 디자인 규칙을 설명 합니다.

재질 테마에는 앱에서 사용할 수 있는 기본 제공 기능으로는 `Theme.Material` 진한 테마 (기본값), `Theme.Material.Light` 테마 및 `Theme.Material.Light.DarkActionBar` 테마가 포함 되어 있습니다.

[진한, Light 및 DarkActionBar 테마의 ![스크린샷](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

Xamarin Android 앱에서 재질 테마 기능을 사용 하는 방법에 대 한 자세한 내용은 [재질 테마](~/android/user-interface/material-theme.md)를 참조 하세요.

## <a name="animations"></a>애니메이션

Android 5.0는 터치 피드백 애니메이션, 작업 전환 애니메이션 및 뷰 상태 전환 애니메이션을 제공 하 여 앱 인터페이스를 보다 직관적으로 사용할 수 있도록 합니다. 또한 Android 5.0 앱은 표시 *효과* 애니메이션을 사용 하 여 보기를 숨기 거 나 표시할 수 있습니다. *곡선 동작* 설정을 사용 하 여 속도가 빠른 애니메이션이 렌더링 되는 방식을 구성할 수 있습니다.

### <a name="touch-feedback-animations"></a>터치 피드백 애니메이션

터치 피드백 애니메이션은 뷰가 작업 될 때 사용자에 게 시각적 피드백을 제공 합니다. 예를 들어 이제 단추를 클릭할 때 ripple 효과가 표시 &ndash;이는 Android 5.0의 기본 터치 피드백 애니메이션입니다. Ripple 애니메이션은 새 `RippleDrawable` 클래스에 의해 구현 됩니다. Ripple 효과를 뷰의 범위에서 종료 하거나 보기 범위를 벗어나 확장 하도록 구성할 수 있습니다. 예를 들어 다음 스크린샷 시퀀스는 터치 애니메이션 중에 단추의 ripple 효과를 보여 줍니다.

![단추에 대 한 ripple 애니메이션의 프레임별 스크린샷](lollipop-images/touch-animation.png)

왼쪽의 첫 번째 이미지에서 단추가 있는 초기 터치 접촉이 발생 하는 반면, 나머지 시퀀스 (왼쪽에서 오른쪽)는 ripple 효과가 단추 가장자리까지 어떻게 확산 되어 있음을 보여 줍니다. Ripple 애니메이션이 종료 되 면 뷰가 원래 모양으로 돌아갑니다. 기본 ripple 애니메이션은 초 단위로 발생 하지만 애니메이션의 길이는 더 길거나 짧은 시간 길이에 맞게 사용자 지정할 수 있습니다.

Android 5.0의 터치 피드백 애니메이션에 대 한 자세한 내용은 [터치 피드백 사용자 지정](https://developer.android.com/training/material/animations.html#Touch)을 참조 하세요.

### <a name="activity-transition-animations"></a>작업 전환 애니메이션

활동 전환 애니메이션은 한 활동이 다른 활동으로 전환 될 때 사용자에 게 시각적 연속성을 제공 합니다. 앱은 다음과 같은 세 가지 유형의 전환 애니메이션을 지정할 수 있습니다.

- 작업이 장면에 들어가면 전환 &ndash;를 **입력** 합니다.

- **종료 전환은** 작업이 장면을 종료 하는 경우에 대 한 &ndash;입니다.

- 첫 번째 활동이 다음으로 전환 될 때 두 활동에 공통적인 뷰가 변경 되는 경우에 대 한 **공유 요소 전환은** &ndash; 합니다.

예를 들어 다음 스크린샷 시퀀스는 공유 요소 전환을 보여줍니다.

[공유 요소 전환 애니메이션의 프레임별 스크린샷 ![프레임](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

공유 요소 (핵심 er기둥의 사진)는 첫 번째 활동의 여러 뷰 중 하나입니다. 첫 번째 활동이 두 번째 활동으로 전환 될 때 두 번째 활동에서 유일한 뷰가 되도록 확대 합니다.

#### <a name="enter-transition-animation-types"></a>전환 애니메이션 유형 입력

Enter 전환의 경우 Android 5.0은 세 가지 유형의 애니메이션을 제공 합니다.

- **애니메이션을 폭발** &ndash; 장면 중심에서 보기를 확대 합니다.

- **슬라이드 애니메이션** &ndash; 장면의 가장자리 중 하나에서 뷰를 이동 합니다.

- **페이드 애니메이션** &ndash; 보기를 장면으로 페이드 인 합니다.

#### <a name="exit-transition-animation-types"></a>종료 전환 애니메이션 유형

종료 전환의 경우 Android 5.0은 세 가지 유형의 애니메이션을 제공 합니다.

- **애니메이션 &ndash; 분해** 하 여 뷰를 장면의 가운데로 축소 합니다.

- **슬라이드 애니메이션** &ndash; 뷰를 장면의 가장자리 중 하나로 이동 합니다.

- **페이드 애니메이션** &ndash; 장면에서 보기를 페이드 아웃 합니다.

#### <a name="shared-element-transition-animation-types"></a>공유 요소 전환 애니메이션 형식

공유 요소 전환은 다음과 같은 여러 유형의 애니메이션을 지원 합니다.

- 뷰의 레이아웃 또는 클립 범위 변경

- 뷰의 배율 및 회전 변경

- 뷰의 크기 및 크기 조정 유형 변경

Android 5.0의 작업 전환 애니메이션에 대 한 자세한 내용은 [작업 전환 사용자 지정](https://developer.android.com/training/material/animations.html#Transitions)을 참조 하세요.

### <a name="view-state-transition-animations"></a>뷰 상태 전환 애니메이션

Android 5.0을 사용 하면 뷰 상태가 변경 될 때 애니메이션을 실행할 수 있습니다. 다음 방법 중 하나를 사용 하 여 뷰 상태 전환에 애니메이션 효과를 적용할 수 있습니다.

- 특정 뷰와 연결 된 상태 변경에 애니메이션을 적용 하는 drawables을 만듭니다. 새 `AnimatedStateListDrawable` 클래스를 사용 하면 뷰 상태 변경 사이에 애니메이션을 표시 하는 drawables을 만들 수 있습니다.

- 뷰의 상태가 변경 될 때 실행 되는 애니메이션 기능을 정의 합니다. 새 `StateListAnimator` 클래스를 사용 하면 뷰의 상태가 변경 될 때 실행 되는 애니메이터를 정의할 수 있습니다.

Android 5.0의 뷰 상태 전환 애니메이션에 대 한 자세한 내용은 [뷰 상태 변경에 애니메이션 효과 주기](https://developer.android.com/training/material/animations.html#ViewState)를 참조 하세요.

### <a name="reveal-effect"></a>효과 표시

표시 *효과* 는 보기를 표시 하거나 숨기도록 반경을 변경 하는 클리핑 원입니다. 클리핑 원의 초기 및 최종 반지름을 설정 하 여이 효과를 제어할 수 있습니다. 다음 스크린샷 시퀀스는 화면 중심에서 표시 효과 애니메이션을 보여 줍니다.

[애니메이션 나타내기의 프레임별 스크린샷 ![프레임](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

다음 시퀀스는 화면의 왼쪽 아래 모퉁이에서 발생 하는 표시 효과 애니메이션을 보여 줍니다.

[클리핑 애니메이션의 프레임별 스크린샷 ![](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

표시 애니메이션은 되돌릴 수 있습니다. 즉, 보기를 표시 하기 위해 확대 하는 대신 클리핑 원을 축소 하 여 보기를 숨길 수 있습니다.

에서 Android 5.0 표시 효과에 대 한 자세한 내용은 표시 [효과 사용](https://developer.android.com/training/material/animations.html#Reveal)을 참조 하세요.

### <a name="curved-motion"></a>곡선 동작

이러한 애니메이션 기능 외에도 Android 5.0는 애니메이션의 시간 및 동작 곡선을 지정할 수 있는 새로운 Api를 제공 합니다. Android 5.0은 이러한 곡선을 사용 하 여 애니메이션 중에 임시 및 공간 이동을 보간합니다. Android 5.0에는 세 가지 곡선이 정의 되어 있습니다.

- **빠른\_확장\_선형 &ndash;\_** 빠르게 가속 되며 애니메이션이 끝날 때까지 계속 가속화 됩니다.

- **빠른\_\_&ndash;\_속도가 빠른** 속도를 빠르게 가속화 하 고 애니메이션의 끝에 빠르게 감속 합니다.

- **선형\_out\_느린 &ndash;\_** 는 최고 속도로 시작 하 고 애니메이션의 끝까지 천천히 감속 합니다.

새 `PathInterpolator` 클래스를 사용 하 여 동작 보간을 수행 하는 방법을 지정할 수 있습니다. `PathInterpolator`는 지정 된 제어 지점과 동작 곡선에 따라 애니메이션 경로를 트래버스하는 보간 기입니다. Android 5.0에서 곡선 동작 설정을 지정 하는 방법에 대 한 자세한 내용은 [곡선 동작 사용](https://developer.android.com/training/material/animations.html#CurvedMotion)을 참조 하세요.

## <a name="view-shadows--elevation"></a>그림자 & 권한 상승 보기

Android 5.0에서는 새 `Z` 속성을 설정 하 여 뷰의 *권한 상승을* 지정할 수 있습니다. `Z` 값이 클수록 뷰가 배경에서 더 큰 그림자를 캐스팅 하 여 뷰가 배경 위에 부동 상태로 표시 되도록 합니다. 레이아웃에서 해당 `elevation` 특성을 구성 하 여 뷰의 초기 권한 상승을 설정할 수 있습니다.

다음 예제에서는 권한 상승 특성이 2dp, 4dp 및 6dp로 각각 설정 된 경우 빈 `TextView` 컨트롤로 캐스팅 되는 그림자를 보여 줍니다.

[progessively 큰 뷰 그림자의 ![스크린샷](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

보기 그림자 설정은 위에 표시 된 것과 같이 정적일 수도 있고 애니메이션에서 사용 하 여 뷰가 보기의 배경 위에 일시적으로 표시 되도록 할 수도 있습니다. `ViewPropertyAnimator` 클래스를 사용 하 여 뷰의 상승에 애니메이션 효과를 적용할 수 있습니다. 뷰의 상승은 레이아웃 `elevation` 설정의 합계 이며 `ViewPropertyAnimator` 메서드 호출을 통해 설정할 수 있는 `translationZ` 속성을 더한 값입니다.

Android 5.0의 그림자 보기에 대 한 자세한 내용은 [그림자 및 클리핑 뷰 정의](https://developer.android.com/training/material/shadows-clipping.html)를 참조 하세요.

## <a name="color-features"></a>색 기능

Android 5.0는 앱에서 색을 관리 하기 위한 두 가지 새로운 기능을 제공 합니다.

- *그릴* 수 있는 색조를 사용 하면 레이아웃 특성을 변경 하 여 이미지 자산의 색을 변경할 수 있습니다.

- *두드러진 색 추출을* 사용 하면 표시 된 이미지의 색상표와 어울리도록 앱 색 테마를 동적으로 사용자 지정할 수 있습니다.

### <a name="drawable-tinting"></a>그릴 때 색조

Android 5.0 레이아웃은 다양 한 색을 표시 하기 위해 여러 버전의 자산을 만들지 않고도 사용할 수 있는 새 `tint` 특성을 인식 합니다. 이 기능을 사용 하려면 비트맵을 알파 마스크로 정의 하 고 `tint` 특성을 사용 하 여 자산의 색을 정의 합니다. 이렇게 하면 자산을 한 번 만들고 레이아웃에 색을 지정할 수 있습니다.

다음 예제에서는 단색 변형을 만드는 데 사용 되는 투명 한 배경 &ndash; 있는 흰색 로고 &ndash; 단일 이미지 자산입니다.

![투명 한 배경을 사용 하는 흰색 Xamarin 로고](lollipop-images/xamarin-logo-white.png)

이 로고는 다음 예와 같이 파란색 원형 배경 위에 표시 됩니다. 왼쪽 이미지는 `tint` 설정 없이 로고가 표시 되는 방법입니다. 가운데 이미지에서 로고의 `tint` 특성은 진한 회색으로 설정 됩니다. 오른쪽 이미지에서 `tint`은 연한 회색으로 설정 됩니다.

![다른 색조 설정이 있는 위의 로고의 예](lollipop-images/drawable-tinting.png)

Android 5.0의 그릴 때 색조에 대 한 자세한 내용은 [그릴 때 색조](https://developer.android.com/training/material/drawables.html#DrawableTint)를 참조 하세요.

### <a name="prominent-color-extraction"></a>두드러진 색 추출

새 Android 5.0 `Palette` 클래스를 사용 하면 이미지에서 색을 추출 하 여 사용자 지정 색상표에 동적으로 적용할 수 있습니다. `Palette` 클래스는 이미지에서 6 개의 색을 추출 하 고 색 채도 및 밝기의 상대적 수준에 따라 이러한 색에 레이블을 합니다.

- 활발

- 생생한 어둡게

- 생생한 조명

- 오디오가

- 음소거 됨

- 음소거 됨 라이트

예를 들어 다음 스크린샷에서 사진 보기 앱은 표시 되는 이미지에서 중요 한 색을 추출 하 고 이러한 색을 사용 하 여 이미지와 일치 하도록 앱의 색 구성표를 조정 합니다.

[녹색, 분홍 및 파랑 테마 색 추출의 ![스크린샷](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

위의 스크린샷에서 작업 모음은 추출 된 "생생한 광원" 색으로 설정 되 고 배경은 추출 된 "생생한 짙은" 색으로 설정 됩니다. 위의 각 예에서는 이미지에서 추출 된 색상표 색을 보여 주기 위해 작은 색 사각형의 행이 포함 되어 있습니다.

Android 5.0에서 색 추출에 대 한 자세한 내용은 [이미지에서 두드러진 색 추출](https://developer.android.com/training/material/drawables.html#ColorExtract)을 참조 하세요.

## <a name="new-ui-widgets"></a>새 UI 위젯

Android 5.0에는 두 가지 새로운 UI 위젯이 도입 되었습니다.

- 스크롤 가능한 항목의 목록을 표시 하는 보기 그룹을 `RecyclerView` &ndash; 합니다.

- 모퉁이가 둥근 기본 레이아웃을 &ndash; `CardView`.

두 위젯은 모두 재질 테마 기능을 지 원하는 구운을 포함 합니다. 예를 들어 뷰를 추가 및 제거 하는 데 애니메이션을 사용 하 고, `RecyclerView` 각 카드가 배경 위에 부동 상태로 표시 되도록 그림자 보기를 사용 `CardView` 합니다. 이러한 새 위젯의 예는 다음 스크린샷에 나와 있습니다.

[RecyclerView로 빌드된 앱 ![스크린샷](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

왼쪽의 스크린샷에는 전자 메일 앱에서 사용 되는 `RecyclerView`의 예가 있습니다. 오른쪽의 스크린샷은 여행 예약 앱에서 사용 되는 `CardView`의 예입니다.

### <a name="recyclerview"></a>RecyclerView

`RecyclerView`은 `ListView,`와 유사 하지만 동적으로 변경 되는 요소가 포함 된 많은 뷰 또는 목록 집합에 보다 적합 합니다. `ListView,`와 마찬가지로 어댑터를 지정 하 여 기본 데이터 집합에 액세스 합니다. 그러나 `ListView,`와 달리 *레이아웃 관리자* 를 사용 하 여 `RecyclerView`내에 항목을 배치할 수 있습니다. 레이아웃 관리자는 뷰 재활용도 처리 합니다. 사용자에 게 더 이상 표시 되지 않는 항목 보기의 재사용을 관리 합니다.

`RecyclerView` 위젯을 사용 하는 경우 `LayoutManager` 및 어댑터를 지정 해야 합니다. 이 그림에 표시 된 것 처럼 `LayoutManager`는 어댑터와 `RecyclerView`간의 매개체입니다.

![LayoutManager, 어댑터 및 데이터 집합을 지 원하는 RecyclerView 다이어그램](lollipop-images/recyclerview-diagram.png)

다음 스크린샷은 100 항목을 포함 하는 `RecyclerView`를 보여 줍니다 (각 항목은 `ImageView` 및 `TextView`로 구성 됨).

[이미지를 통해 스크롤 하는 RecyclerView 앱의 ![스크린샷](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

이 샘플 응용 프로그램에서 목록의 처음부터 목록의 끝까지 &ndash; 쉽게 스크롤할 수 있는이 대량 데이터 집합을 처리 `RecyclerView` 몇 초 밖에 걸리지 않습니다. `RecyclerView`는 애니메이션도 지원 합니다. 실제로 항목 추가 및 제거에 대 한 애니메이션은 기본적으로 사용 하도록 설정 됩니다. 항목이 `RecyclerView`에 추가 되 면이 스크린샷 시퀀스와 같이 페이드 인 됩니다.

[사진 항목의 프레임별 스크린샷 ![](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

`RecyclerView`에 대 한 자세한 내용은 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)를 참조 하세요.

### <a name="cardview"></a>CardView

`CardView`은 모퉁이가 둥근 부동 카드를 시뮬레이션 하는 간단한 뷰입니다. `CardView`에는 기본 제공 보기 섀도우가 있으므로 앱에 시각적 깊이를 추가 하는 쉬운 방법을 제공 합니다. 다음 스크린샷에는 `CardView`의 세 가지 텍스트 지향 예제가 나와 있습니다.

[![CardView 기반 항목과 함께 RecyclerView를 사용 하는 앱의 예제 스크린샷](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

위의 예제에서 각 카드에는 `TextView`포함 되어 있습니다. 배경색은 `cardBackgroundColor` 특성을 통해 설정 됩니다.

`CardView`에 대 한 자세한 내용은 [CardView](~/android/user-interface/controls/card-view.md)를 참조 하세요.

## <a name="enhanced-notifications"></a>향상 된 알림

Android 5.0의 알림 시스템은 새 시각적 형식 및 새로운 기능을 사용 하 여 크게 업데이트 되었습니다. 알림은 Android 5.0에서 새롭게 보입니다. 예를 들어 Android 5.0의 알림은 이제 밝은 배경에 진한 텍스트를 사용 합니다.

![확장 취소 된 Android 5.0 알림의 예](lollipop-images/expanded-notification-contracted.png)

위의 예제와 같이 알림에 커다란 아이콘이 표시 되 면 Android 5.0은 작은 아이콘을 크게 아이콘 위에 배지 표시 합니다.

Android 5.0에서는 알림이 장치 잠금 화면에도 표시 될 수 있습니다.
예를 들어 다음은 단일 알림이 포함 된 잠금 화면 예제 스크린샷입니다.

[잠금 화면에 나타나는 알림의 ![스크린샷](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

사용자는 잠금 화면에서 알림을 두 번 탭 하 여 장치의 잠금을 해제 하 고 해당 알림을 시작한 앱으로 이동 하거나, 살짝 밀어 알림을 해제할 수 있습니다. 알림에는 잠금 화면에 표시할 수 있는 콘텐츠의 양을 결정 하는 새로운 *표시 유형* 설정이 있습니다. 사용자는 중요 한 콘텐츠가 잠금 화면 알림에 표시 되도록 허용할지 여부를 선택할 수 있습니다.

Android 5.0에는 *준비*라는 새로운 우선 순위가 높은 알림 프레젠테이션 형식이 도입 되었습니다. 준비 알림은 몇 초 동안 화면 맨 위에서 아래로 이동 하 고 화면 맨 위에 있는 알림 음영으로 되돌아갑니다. 알림 메시지를 통해 시스템 UI는 현재 실행 중인 작업을 중단 하지 않고 사용자 앞에 중요 한 정보를 넣을 수 있습니다.
다음 예제에서는 앱 위에 표시 되는 간단한 준비 알림을 보여 줍니다.

[![준비 알림의 예](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

일반적으로 준비 알림은 다음 이벤트에 사용 됩니다.

- 새 다음 메시지

- 수신 전화 통화

- 배터리 부족 표시

- 경보

Android 5.0는 높은 또는 최대 우선 순위 설정이 있는 경우에만 준비 형식으로 알림을 표시 합니다.

Android 5.0에서 알림 메타 데이터를 제공 하 여 Android에서 알림을 보다 지능적으로 정렬 하 고 표시할 수 있습니다. Android 5.0는 우선 순위, 표시 유형 및 범주에 따라 알림을 구성 합니다.
알림 범주는 장치가 *방해* 금지 모드에 있을 때 표시할 수 있는 알림을 필터링 하는 데 사용 됩니다.

최신 Android 5.0 기능을 사용 하 여 알림을 만들고 시작 하는 방법에 대 한 자세한 내용은 [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)을 참조 하세요.

## <a name="new-apis"></a>새로운 API

Android 5.0은 위에 설명 된 새로운 모양 및 느낌 기능 외에도 기존 멀티미디어, 저장소 및 무선/연결 기능의 기능을 확장 하는 새로운 Api를 추가 합니다. 또한 Android 5.0에는 새 작업 스케줄러 기능을 지 원하는 새로운 Api가 포함 되어 있습니다.

### <a name="camera"></a>카메라

Android 5.0는 향상 된 카메라 기능을 위한 몇 가지 새로운 Api를 제공 합니다. 새 `Android.Hardware.Camera2` 네임 스페이스에는 Android 장치에 연결 된 개별 카메라 장치에 액세스 하기 위한 기능이 포함 되어 있습니다. 또한 `Android.Hardware.Camera2`는 각 카메라 장치를 파이프라인으로 모델링 합니다. 캡처 요청을 수락 하 고 이미지를 캡처한 다음 결과를 출력 합니다. 이 접근 방식을 사용 하면 앱이 카메라 장치에 여러 캡처 요청을 큐에 대기 시킬 수 있습니다.

다음 Api는 이러한 새로운 기능을 가능 하 게 합니다.

- `CameraManager.GetCameraIdList` &ndash;를 통해 카메라 장치에 프로그래밍 방식으로 액세스할 수 있습니다. `CameraManager.OpenCamera`를 사용 하 여 특정 카메라 장치에 연결 합니다.

- 카메라 장치에서 이미지를 캡처하거나 스트리밍하는 `CameraCaptureSession` &ndash; 합니다. 새 이미지 캡처 이벤트를 처리 하는 `CameraCaptureSession.CaptureListener` 인터페이스를 구현 합니다.

- `CaptureRequest` &ndash;는 캡처 매개 변수를 정의 합니다.

- `CaptureResult` &ndash;는 이미지 캡처 작업의 결과를 제공 합니다.

Android 5.0의 새로운 카메라 Api에 대 한 자세한 내용은 [미디어](https://developer.android.com/about/versions/android-5.0.html#Media)를 참조 하세요.

### <a name="audio-playback"></a>오디오 재생

Android 5.0은 더 나은 오디오 재생을 위해 `AudioTrack` 클래스를 업데이트 합니다.

- `ENCODING_PCM_FLOAT` &ndash;는 더 나은 동적 범위, 더 많은 공간 및 높은 품질을 위해 부동 소수점 형식의 오디오 데이터를 허용 하도록 `AudioTrack`를 구성 합니다 (전체 자릿수가 증가 함). 또한 부동 소수점 형식은 오디오 클리핑을 방지 하는 데 도움이 됩니다.

- `ByteBuffer` &ndash; 이제 `AudioTrack`에 오디오 데이터를 바이트 배열로 제공할 수 있습니다.

- `WRITE_NON_BLOCKING` &ndash;이 옵션은 일부 앱에 대 한 버퍼링 및 다중 스레딩을 간소화 합니다.

Android 5.0의 향상 된 기능에 대 한 자세한 `AudioTrack` [미디어](https://developer.android.com/about/versions/android-5.0.html#Media)를 참조 하세요.

### <a name="media-playback-control"></a>미디어 재생 제어

Android 5.0에는 `RemoteControlClient`를 대체 하는 새로운 `Android.Media.MediaController` 클래스가 도입 되었습니다. `Android.Media.MediaController`은 간소화 된 전송 제어 Api를 제공 하며 UI 컨텍스트 외부에서 스레드로부터 안전한 재생 제어를 제공 합니다. 전송 제어를 처리 하는 새 Api는 다음과 같습니다.

- `Android.Media.Session.MediaSession`는 여러 컨트롤러를 처리 하는 미디어 제어 세션 &ndash; 합니다. `MediaSession.GetSessionToken`를 호출 하 여 앱이 세션과 상호 작용 하는 데 사용 하는 토큰을 요청 합니다.

- `MediaController.TransportControls` &ndash;는 **재생**, **중지**, **Skip**등의 전송 명령을 처리 합니다.

또한 새 `Android.App.Notification.MediaStyle` 클래스를 사용 하 여 미디어 세션을 다양 한 알림 콘텐츠 (예: 앨범 아트 추출 및 표시)와 연결할 수 있습니다.

Android 5.0의 새로운 미디어 재생 제어 기능에 대 한 자세한 내용은 [미디어](https://developer.android.com/about/versions/android-5.0.html#Media)를 참조 하세요.

### <a name="storage"></a>스토리지

Android 5.0는 응용 프로그램이 디렉터리와 문서를 쉽게 사용할 수 있도록 저장소 액세스 프레임 워크를 업데이트 합니다.

- 디렉터리 하위 트리를 선택 하기 위해 `Android.Intent.Action.OPEN_DOCUMENT_TREE` 의도를 빌드하고 보낼 수 있습니다. 이로 인해 시스템은 하위 트리 선택을 지 원하는 모든 공급자 인스턴스를 표시 합니다. 그런 다음 사용자는 디렉터리를 찾아서 선택 합니다.

- 하위 트리 아래의 어디에서 나 새 문서 또는 디렉터리를 만들고 관리 하려면 `DocumentsContract`의 새로운 `CreateDocument`, `RenameDocument`및 `DeleteDocument` 메서드를 사용 합니다.

- 모든 공유 저장 장치에서 미디어 디렉터리에 대 한 경로를 가져오려면 새 `Android.Content.Context.GetExternalMediaDirs` 메서드를 호출 합니다.

Android 5.0의 새로운 저장소 Api에 대 한 자세한 내용은 [저장소](https://developer.android.com/preview/api-overview.html#Storage)를 참조 하세요.

### <a name="wireless--connectivity"></a>무선 & 연결

Android 5.0는 무선 및 연결에 대 한 다음과 같은 향상 된 API를 추가 합니다.

- 응용 프로그램에서 연결을 설정 하기 전에 특정 기능을 가진 네트워크를 찾아 선택할 수 있게 해 주는 새로운 *다중 네트워크* api입니다.

- Android 5.0 장치가 저 에너지 Bluetooth 주변 장치로 작동할 수 있도록 하는 Bluetooth 브로드캐스팅 기능.

- NFC 고급 기능을 통해 다른 장치와 데이터를 공유 하는 근거리 통신 기능을 보다 쉽게 사용할 수 있습니다.

Android 5.0의 새로운 무선 및 연결 Api에 대 한 자세한 내용은 [무선 및 연결](https://developer.android.com/preview/api-overview.html#Wireless)을 참조 하세요.

### <a name="job-scheduling"></a>작업 일정 조정

Android 5.0에는 장치를 연결 하 고 충전 하는 경우에만 특정 작업을 실행 하도록 예약 하 여 배터리 소모를 최소화 하는 데 도움이 되는 새로운 `JobScheduler` API가 도입 되었습니다. 이 작업 스케줄러 기능을 사용 하 여 장치가 계량 된 네트워크 대신 Wi-fi 네트워크를 통해 연결 되어 있을 때 용량이 많은 파일을 다운로드 하는 등의 작업에 더 적합 한 경우 실행할 작업을 예약할 수도 있습니다.

Android 5.0의 새로운 작업 예약 Api에 대 한 자세한 내용은 [작업 예약](https://developer.android.com/preview/api-overview.html#JobScheduler)을 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 Xamarin android 앱 개발자를 위한 Android 5.0의 중요 한 새로운 기능에 대 한 개요를 제공 했습니다.

- 재질 테마

- 애니메이션

- 그림자 및 권한 상승 보기

- 그릴 때 색조 및 강조 된 색 추출을 비롯 한 색 기능

- 새 `RecyclerView` 및 `CardView` 위젯

- 알림 기능 향상

- 카메라, 오디오 재생, 미디어 컨트롤, 저장소, 무선/연결 및 작업 예약을 위한 새 Api

Xamarin Android 개발을 처음 접하는 경우에는 Xamarin android를 시작 하는 데 도움이 되는 [설정 및 설치](~/android/get-started/installation/index.md) 를 참조 하세요.
[Hello, android](~/android/get-started/hello-android/index.md) 는 android 프로젝트를 만드는 방법을 배우는 데 유용한 소개입니다.

## <a name="related-links"></a>관련 링크

- [Android L Developer Preview](https://developer.android.com/preview/index.html)
- [Android SDK 가져오기](https://developer.android.com/sdk/index.html#Other)
- [재질 디자인](https://developer.android.com/preview/material/index.html)
