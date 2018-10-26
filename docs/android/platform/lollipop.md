---
title: 롤리팝 기능
description: 이 문서에서는 Android 5.0 (Lollipop)에 도입 된 새로운 기능의 간략 한 개요를 제공 합니다. 이러한 기능에는 애니메이션와 보기 그림자를 그릴 수 있는 색조 새로운 지원 기능 뿐만 아니라 자료 테마 라는 새 사용자 인터페이스 스타일을 포함 합니다. Android 5.0에는 저장소, 네트워킹, 연결 및 멀티미디어 기능을 개선 하기 위해 향상 된 알림, 두 개의 새 UI 위젯, 새 작업 스케줄러를 및 몇 가지 새로운 Api도 포함 됩니다.
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: d79c0563d1dc9a2cfe75b702300982bb4d38553b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117865"
---
# <a name="lollipop-features"></a>롤리팝 기능

_이 문서에서는 Android 5.0 (Lollipop)에 도입 된 새로운 기능의 간략 한 개요를 제공 합니다. 이러한 기능에는 애니메이션와 보기 그림자를 그릴 수 있는 색조 새로운 지원 기능 뿐만 아니라 자료 테마 라는 새 사용자 인터페이스 스타일을 포함 합니다. Android 5.0에는 저장소, 네트워킹, 연결 및 멀티미디어 기능을 개선 하기 위해 향상 된 알림, 두 개의 새 UI 위젯, 새 작업 스케줄러를 및 몇 가지 새로운 Api도 포함 됩니다._

## <a name="lollipop-overview"></a>롤리팝 개요

Android 5.0 (Lollipop)는 새로운 디자인 언어에는 *재료 디자인*,이 사용 하 여 앱을 쉽게 하 고 보다 직관적으로 사용할 새 기능의 캐스팅을 지원 합니다. 재질 디자인을 사용 하 여 Android 5.0 뿐만 아니라 제공 Android 휴대폰 생기; 또한 Android 기반 태블릿, 데스크톱 컴퓨터, 조사식 및 스마트 Tv에 대 한 디자인 규칙의 새 집합을 제공합니다. 디자인에 주안점을 간단 하 고 미니 멀리즘 하면서 사용 하 여 친숙 한 tactile 특성 (예: 실제 화면 가장자리와 큐) 사용자를 신속 하 고 직관적으로 인터페이스를 이해 합니다.

*재질 테마* 는 Android에서 이러한 UI 디자인 원칙의 전형입니다. 이 문서는 재질 테마 지원 기능을 포함 하 여 시작 합니다.

-   **애니메이션** &ndash; *피드백 Touch* 애니메이션 *활동 전환* 애니메이션 *상태 전환 보기* 애니메이션과 *효과 표시*합니다.

-   **그림자 및 권한 상승 확인** &ndash; 뷰는 이제는 `elevation` 속성; 보기를 사용 하 여 더 높은 `elevation` 값 백그라운드에서 더 큰 그림자를 캐스팅 합니다.

-   **기능 색** &ndash; *Drawable 색조* 해당 색을 변경 하 여 이미지 자산을 다시 사용할 수 있도록 하 고 *두드러진 색 추출* 동적으로 도움이 테마 이미지의에서 색상을 기반으로 앱 합니다.

많은 자료 테마 기능에 이미 작성 되어 Android 5.0 UI, 동안 될 다른 사용자가 명시적으로 추가 해야 앱. 예를 들어, 일부 표준 뷰 (예: 단추) 앱에는 대부분의 뷰 그림자 사용 하도록 설정 해야 하는 동안 이미 터치 피드백 애니메이션을 포함 합니다.

재질 테마를 통해 일으킵니다 UI 개선 사항 외에도 Android 5.0이이 문서에서 다루는 다른 몇 가지 새로운 기능이 포함 합니다.

-   **알림 향상** &ndash; 알림이 Android 5.0에서 새롭게, 잠금 화면 알림을 지원 하 고 새 크게 업데이트 되었습니다 *헤 즈 업* 알림 프레젠테이션 형식입니다.

-   **새 UI 위젯** &ndash; 새 `RecyclerView` 위젯을 쉽게 큰 데이터 집합 및 복잡 한 정보와 새 전달 하기 위해 앱 `CardView` 위젯은 텍스트를 표시 하기 위한 간소화 된 카드와 같은 프레젠테이션 형식을 제공 하 고 이미지입니다.

-   **새 Api** &ndash; Android 5.0 Bluetooth 연결, 저장소 관리를 간소화 및 멀티미디어 플레이어 및 카메라 장치 보다 유연 하 게 제어를 향상 된 여러 네트워크 지원에 대 한 새로운 Api를 추가 합니다. 새 작업 예약 기능 이란에서 비동기적으로 작업을 실행할 수 있습니다. 예약 된 시간입니다. 이 기능을 사용 하면에서 배터리 수명을 개선 하려면 예를 들어 장치가 있는 경우 위치 및 청구를 수행 하기 위해 작업을 예약 합니다.


## <a name="requirements"></a>요구 사항

다음은 Xamarin 기반 앱에서 새 Android 5.0 기능을 사용 하려면 필요 합니다.

-   **Xamarin.Android** &ndash; 4.20 이상 Xamarin.Android를 설치 하 고 mac 용 Visual Studio 또는 Visual Studio를 사용 하 여 구성 해야 

-   **Android SDK** &ndash; Android 5.0 (API 21) 나중에 설치 해야 Android SDK Manager를 통해 또는 합니다.

-   **Java Developer Kit** &ndash; Xamarin.Android 필요 [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) API 수준 24 개발 하는 경우 이후 또는 큰 (JDK 1.8에서는 API 수준 24 롤리팝을 비롯 한 이전). 64 비트 JDK 1.8 버전이 필요한 사용자 지정 컨트롤 또는 폼 미리 보기를 사용 하는 경우입니다.

계속 사용할 수 있습니다 [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API 레벨 23에 맞게 개발 이하의 경우.


## <a name="setting-up-an-android-50-project"></a>Android 5.0 프로젝트 설정

Android 5.0 프로젝트를 만들려면 최신 도구 및 SDK 패키지를 설치 해야 합니다. Android 5.0를 대상으로 하는 Xamarin.Android 프로젝트를 설정 하려면 다음 단계를 사용 합니다.

1. Xamarin.Android 도구를 설치 하 고 Xamarin 라이선스를 활성화 합니다. 참조 [설정 및 설치](~/android/get-started/installation/index.md) Xamarin.Android를 설치 하는 방법에 대 한 자세한 내용은 합니다.

2. Visual Studio for Mac를 사용 하는 경우에 최신 Android 5.0 업데이트를 설치 합니다.

3. Android SDK Manager 시작 (Mac 용 Visual Studio에서 사용 하 여 **도구 &gt; 열린 Android SDK Manager&hellip;**) 및 Android SDK Tools 23.0.5 설치 이상:

    [![Android SDK Manager에서 Android SDK 도구 선택](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   또한 최신 Android 5.0 SDK 패키지 (API 21 이상)를 설치 합니다.

    [![Android SDK Manager에서 Android 5.0 SDK 패키지를 설치합니다.](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   Android SDK Manager를 사용 하는 방법에 대 한 자세한 내용은 참조 하세요. [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html)합니다.

4. 새 Xamarin.Android 프로젝트를 만듭니다. Xamarin 사용한 Android 개발을 처음 접하는 경우 참조 [Hello, Android](~/android/get-started/hello-android/index.md) Android 프로젝트 만들기에 대해 알아보려면 합니다. Android 프로젝트를 만들면 Android 5.0 버전 설정을 구성 해야 합니다.
   Mac 용 Visual Studio로 이동 **프로젝트 옵션 &gt; 빌드 &gt; 일반** 설정 하 고 **대상 프레임 워크** 하 **Android 5.0 (Lollipop)** 또는 나중에:

    ![Android 5.0 Lollipop에 대상 Framwework 설정](lollipop-images/target-framework.png)

   아래 **프로젝트 옵션 &gt; 빌드 &gt; Android 응용 프로그램**대상 Android 버전을 최소 설정한 **자동으로 사용 하 여 대상 프레임 워크 버전**:

    ![최소 및 대상 Android 버전은 자동으로 설정](lollipop-images/minimum-android-version.png)

5. 앱을 테스트 하려면 에뮬레이터 또는 Android 장치를 구성 합니다. 에뮬레이터를 사용 하는 경우 참조 [Android Emulator 설정](~/android/get-started/installation/android-emulator/index.md) 에 Xamarin Studio 또는 Visual Studio를 사용 하 여 사용 하기 위해 Android 에뮬레이터를 구성 하는 방법을 알아봅니다. Android 장치를 사용 하는 경우 참조 [the Preview SDK 설정](https://developer.android.com/preview/setup-sdk.html) Android 5.0 장치를 업데이트 하는 방법을 알아보려면. 실행 하 고 Xamarin.Android 응용 프로그램 디버깅에 대 한 Android 장치를 구성 하려면 참조 [장치 개발을 위한 설정](~/android/get-started/installation/set-up-device-for-development.md)합니다.

참고: 대상 Android L 미리 보기는 기존 Android 프로젝트를 업데이트 하는 경우 업데이트 해야 합니다 **대상 프레임 워크** 및 **Android 버전** 위에서 설명한 값으로.

## <a name="important-changes"></a>중요 한 변경 내용

이전에 게시 된 Android 앱은 Android 5.0에서 변경 하 여 저하 될 수 있습니다. 특히, Android 5.0에서는 새로운 런타임 및 크게 변경 된 알림 형식에 사용합니다.

### <a name="android-runtime"></a>Android 런타임

Android 5.0 Dalvik 대신 기본 런타임으로 새 Android 런타임 (그림)을 사용합니다. 아트는 몇 가지 주요 새 기능을 구현합니다.

-   **미리-(AOT) 컴파일** &ndash; AOT 앱이 처음으로 시작 하기 전에 앱 코드를 컴파일하여 앱 성능을 향상 시킬 수 있습니다. 앱을 설치 하면 아트 컴파일된 앱 대상 장치에 대 한 실행 파일을 생성 합니다.

-   **GC (가비지 수집) 향상** &ndash; 아트의 GC 향상 앱 성능을 향상할 수도 있습니다. 가비지 컬렉션에는 이제 2 대신 1 GC 일시 중지를 사용 하 고 동시 GC 작업을 더 적시에 완료 합니다.

-   **응용 프로그램 디버깅 개선** &ndash; 아트 예외를 분석 하 고 충돌 보고서에 자세한 진단 정보를 제공 합니다.

기존 앱 아트에서 변경 없이 작동 해야 &ndash; 이전 Dalvik 런타임에 고유한 기법을 악용 하는 앱을 제외 하 고는 작동 하지 않을 수 아트에서 합니다. 이러한 변경에 대 한 자세한 내용은 참조 하세요. [앱 확인 동작에는 Android 런타임 (최신)](http://developer.android.com/guide/practices/verifying-apps-art.html)합니다.


### <a name="notification-changes"></a>알림 변경

알림 Android 5.0에서 크게 변경 되었습니다.

-   **소리 및 진동 다르게 처리 됩니다** &ndash; 알림 사운드 및 진동 이제 의해 처리 됩니다 `Notification.Builder` 대신 `Ringtone`합니다 `MediaPlayer`, 및 `Vibrator`합니다.

-   **새로운 색 구성표** &ndash; 재질 테마에 따라 알림을 텍스트로 렌더링 어두운 배경에 흰색이 나 매우 가볍습니다. 또한 시스템 색 구성표를 사용 하 여 조정 하는 Android에서 알림 아이콘에 알파 채널을 수정할 수 있습니다. 

-   **잠금 화면 알림을** &ndash; 알림 장치 잠금 화면에 이제 나타날 수 있습니다.

-   **헤 즈 업** &ndash; 우선 순위가 높은 알림 (헤드업 알림)와 같은 작은 부동 창에 나타납니다 경우 장치 잠금 해제 되어 화면 켜져 있습니다.

대부분의 경우에는 다음 단계가 필요 Android 5.0에 기존 앱 알림 기능을 이식 합니다.

1.  사용 하도록 코드를 변환할 `Notification.Builder` (또는 `NotificationsCompat.Builder`) 알림 만들기에 대 한 합니다. 

2.  기존 알림 자산 새로운 재질 테마 색 구성표에서 볼 수 있는지 확인 합니다.

3.  잠금 화면에 표시 될 때 알림이 있어야 어떤 표시 여부를 결정 합니다. 알림을 public 인 경우 콘텐츠는 잠금 화면에 표시 해야?

4.  새 Android 5.0에서 올바르게 처리 되도록 알림의 범주를 설정 *방해 금지* 모드입니다.

알림을 전송 컨트롤을 디스플레이 미디어 재생 상태를 표시를 사용 하 여 `RemoteControlClient`를 호출 하거나 `ActivityManager.GetRecentTasks`를 참조 [중요 한 동작 변경 내용](http://developer.android.com/preview/api-overview.html#Behaviors) Android에 대 한 알림을 업데이트 하는 방법에 대 한 자세한 내용은 5.0입니다.

Android에서 알림을 만드는 방법에 대 한 자세한 내용은 [로컬 알림을](~/android/app-fundamentals/notifications/local-notifications.md)합니다. 합니다 [호환성](~/android/app-fundamentals/notifications/local-notifications.md#compatibility) 이 문서의 섹션 아래쪽 호환 되는 알림을 만드는 방법에 설명 이전 버전의 Android 사용 하 여 합니다.


## <a name="material-theme"></a>재질 테마

새 Android 5.0 재질 테마 Android UI의 모양과 느낌을 전면적으로 변화를 제공합니다. 이제 시각적 요소에 굵게 표시 된 그래픽, 입력 체계, 및 인쇄 기반 디자인의 밝은 색을 사용 하는 tactile 화면을 사용 합니다. 재질 테마의 예는 다음 스크린샷에 표시 되는

[![재질 테마 홈 화면, 응용 프로그램 화면 및 설정 화면 스크린샷](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0 왼쪽에 표시 된 홈 화면을 사용 하 여 인사말입니다. Center 스크린 샷 앱 목록의 첫 번째 화면 이며 오른쪽 스크린샷에 합니다 **설정을** 화면. Google [재질 디자인](https://material.io/guidelines/material-design/introduction.html) 사양 기본 디자인 규칙에 새 재질 테마 개념을 설명 합니다.

재질 테마에는 앱에서 사용할 수 있는 세 가지 기본 제공 버전 포함: 합니다 `Theme.Material` 어두운 테마 (기본값)를 `Theme.Material.Light` 테마 및 `Theme.Material.Light.DarkActionBar` 테마: 

[![어두운 스크린샷, 조명 및 DarkActionBar 테마](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

재질 테마 기능을 사용 하 여 Xamarin.Android 앱에 대 한 자세한 내용은 참조 하세요 [재질 테마](~/android/user-interface/material-theme.md)합니다.


## <a name="animations"></a>애니메이션

Android 5.0 터치 피드백 애니메이션, 작업 전환 애니메이션 및 앱 인터페이스를 사용 하는 것이 직관적 있도록 뷰 상태 전환 애니메이션을 제공 합니다. 또한 Android 5.0 앱을 사용할 수 *효과 표시* 애니메이션을 숨기 거 나 뷰를 표시 합니다. 사용할 수 있습니다 *동작 곡선* 얼마나 신속 하 게 구성 하는 설정 또는 느린 애니메이션 렌더링 됩니다.


### <a name="touch-feedback-animations"></a>터치 피드백이 애니메이션

터치 피드백이 애니메이션 뷰일 수 된 경우 사용자에 게 시각적 피드백을 제공 합니다. 예를 들어, 단추 이제 파급 효과가 있을 때 표시 작업 &ndash; Android 5.0에서 기본 터치 피드백 애니메이션입니다. Ripple 애니메이션 새 구현 됩니다 `RippleDrawable` 클래스입니다. 보기의 범위에서 종료 또는 보기의 범위를 벗어나 확장 파급 효과가 구성할 수 있습니다. 예를 들어, 다음 일련의 스크린샷은 터치 애니메이션 하는 동안 단추의 파급 효과가 보여 줍니다.

![단추에서 ripple 애니메이션 프레임 스크린샷에서 프레임](lollipop-images/touch-animation.png)

단추를 사용 하 여 초기 터치 연락처 (왼쪽에서 오른쪽) 나머지 시퀀스를 단추 가장자리로 파급 효과가 분산 하는 방법을 보여 줍니다. 동안 왼쪽의 첫 번째 이미지에 발생 합니다. Ripple 애니메이션이 종료 될 때 원래 모양으로 뷰를 반환 합니다. 기본 ripple 애니메이션 초 미만의 시간에 수행 하지만 더 길거나 더 짧은 시간 길이 대 한 애니메이션의 길이 사용자 지정할 수 있습니다.

Android 5.0에서 피드백 애니메이션, 터치 대 한 자세한 내용은 참조 [Touch 피드백을 사용자 지정](http://developer.android.com/training/material/animations.html#Touch)합니다.


### <a name="activity-transition-animations"></a>작업 전환 애니메이션

작업 전환 애니메이션 하나의 활동 간에 전환 될 때 사용자의 연속성을 제공 합니다. 앱 세 가지 유형의 전환 애니메이션을 지정할 수 있습니다.

-   **전환 입력** &ndash; 활동 들어가면 장면에 대 한 합니다.

-   **전환 종료** &ndash; 활동 장면을 종료 하는 경우에 대 한 합니다.

-   **공유 요소 전환** &ndash; 다음은 첫 번째 작업 전환으로 두 작업에 공통적으로 적용 되는 뷰를 변경 하는 경우에 대 한 합니다.

예를 들어, 다음 일련의 스크린샷은 공유 요소 전환을 보여 줍니다.

[![공유 요소 전환 애니메이션의 프레임 스크린샷에서 프레임](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

공유 요소 (한 애벌레 사진)을 사용 하면 첫 번째 작업;의 여러 뷰 중 하나인 이 두 번째 활동은 첫 번째 전환으로 두 번째 작업에만 뷰를 확대 합니다.

#### <a name="enter-transition-animation-types"></a>전환 애니메이션 형식 입력

Enter 전환에 대 한 Android 5.0에는 애니메이션의 세 가지 유형을 제공합니다.

-   **애니메이션 explode** &ndash; 장면의 가운데에서 뷰를 확대 합니다.

-   **슬라이드 애니메이션** &ndash; 장면의 가장자리 중 보기에서 이동 합니다.

-   **애니메이션 페이드** &ndash; 장면에 대 한 보기 사라집니다.

#### <a name="exit-transition-animation-types"></a>종료 전환 애니메이션 형식

종료 전환에 대 한 Android 5.0에는 애니메이션의 세 가지 유형을 제공합니다.

-   **애니메이션 explode** &ndash; 중심 장면에 뷰를 축소 합니다.

-   **슬라이드 애니메이션** &ndash; 뷰일 장면의 가장자리 중 하나로 이동 합니다.

-   **애니메이션 페이드** &ndash; 장면은 뷰가 사라집니다.

#### <a name="shared-element-transition-animation-types"></a>공유 요소 전환 애니메이션 형식

공유 요소 전환와 같은 여러 유형의 애니메이션을 지원합니다.

-   뷰 레이아웃 또는 클립 범위를 변경 합니다.

-   크기 조정 및 회전 뷰의 변경 합니다.

-   보기에 대 한 크기 조정 및 규모 형식을 변경 합니다.

Android 5.0에서 전환 애니메이션 작업에 대 한 자세한 내용은 참조 하세요 [사용자 지정 활동 전환](http://developer.android.com/training/material/animations.html#Transitions)합니다.


### <a name="view-state-transition-animations"></a>뷰 상태 전환 애니메이션

Android 5.0 애니메이션 뷰 상태가 변경 될 때 실행할 수 있습니다. 다음 방법 중 하나를 사용 하 여 보기 상태 전환 애니메이션을 적용할 수 있습니다.

-   특정 뷰를 사용 하 여 연결 된 상태 변경에 애니메이션 효과 주기는 드로어 블을 만듭니다. 새 `AnimatedStateListDrawable` 클래스 뷰 상태 변경 간 애니메이션을 표시 하는 드로어 블을 만들 수 있습니다.

-   뷰 상태가 변경 될 때 실행 되는 애니메이션 기능을 정의 합니다. 새 `StateListAnimator` 클래스 뷰 상태가 변경 될 때 실행 되는 애니메이터 정의할 수 있습니다.

Android 5.0에서 뷰 상태 전환 애니메이션에 대 한 자세한 내용은 참조 하세요 [뷰 상태 변경에 애니메이션 효과 주기](http://developer.android.com/training/material/animations.html#ViewState)합니다.


### <a name="reveal-effect"></a>효과 표시

합니다 *효과 표시* 클리핑 원 해당 변경 내용을 반지름을 표시 하거나 숨기는 보기입니다. 초기 및 최종 클리핑 원의 반지름을 설정 하 여이 효과 제어할 수 있습니다. 다음 일련의 스크린샷은 화면의 가운데에서 표시 효과 애니메이션을 보여 줍니다.

[![프레임 스크린샷에서 표시 애니메이션 프레임](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

다음 시퀀스에서는 화면의 왼쪽된 아래 모서리에서 발생 하는 표시 효과 애니메이션을 보여 줍니다.

[![클리핑 애니메이션 프레임 스크린샷에서 프레임](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

표시할 애니메이션을 되돌릴 수 있습니다. 즉, 클리핑 원 보기 숨기려면 축소 것이 아니라 수 보기를 표시 하려면 확대 합니다.

Android 5.0 표시 영향에 대 한 자세한 내용은 참조 하세요. [표시 효과 사용 하 여](http://developer.android.com/training/material/animations.html#Reveal)입니다.


### <a name="curved-motion"></a>곡선된 동작

이러한 애니메이션 기능 외에도 Android 5.0 또한 애니메이션의 시간과 동작 곡선을 지정할 수 있도록 새로운 Api를 제공 합니다. Android 5.0 이러한 곡선을 사용 하 여 애니메이션 하는 동안 임시 및 공간 이동 보간합니다. 세 가지 곡선 Android 5.0에서 정의 됩니다.

-   **빠른\_아웃\_선형\_에서** &ndash; 신속 하 게 가속화 하 고 가속화 애니메이션의 끝까지 계속 됩니다.

-   **빠른\_아웃\_느린\_에서** &ndash; 애니메이션의 끝 쪽 감속 빠르고 느린 Accelerates 합니다.

-   **선형\_아웃\_느린\_에서** &ndash; 최고 속도 느리게 시작 애니메이션의 끝에 감속 합니다.

새 따르면 `PathInterpolator` 동작 보간을 수행 하는 방법을 지정 하는 클래스입니다. `PathInterpolator` 지정 된 제어점과 동작 곡선에 따라 애니메이션 경로 통과 하는 보간이입니다. Android 5.0에서 곡선된 동작 설정을 지정 하는 방법에 대 한 자세한 내용은 참조 하세요. [사용 하 여 곡선 동작](http://developer.android.com/training/material/animations.html#CurvedMotion)합니다.


## <a name="view-shadows--elevation"></a>보기 그림자 & 권한 상승

Android 5.0에서 지정할 수 있습니다는 *권한 상승이* 새 설정 하 여 뷰의 `Z` 속성입니다. 큼 `Z` 값을 float 배경 위에 더 높은 표시 보기를 만드는 백그라운드에서 더 큰 그림자에 뷰를 사용 하면 됩니다. 뷰의 초기 권한 상승을 구성 하 여 설정할 수 있습니다 해당 `elevation` 레이아웃의 특성입니다.

다음 예제에서는 빈에서 캐스팅 그림자 `TextView` 해당 권한 상승 특성으로 설정 되 면 2dp, 4dp, 및 6dp를 각각 제어 합니다.

[![Progessively 큰 보기 그림자의 스크린샷](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

섀도 설정 보기 (위와 같이) 정적 수 또는 보기의 배경 위에 증가 일시적으로 표시 하는 보기를 만들 애니메이션에서 사용할 수 있습니다. 사용할 수는 `ViewPropertyAnimator` 뷰의 권한 상승에 애니메이션 효과를 클래스입니다. 뷰의 레이아웃의 합계인 `elevation` 설정 및 `translationZ` 속성을 통해 설정할 수 있는 `ViewPropertyAnimator` 메서드를 호출 합니다.

Android 5.0에서 보기 그림자에 대 한 자세한 내용은 참조 하세요 [그림자 정의 및 클리핑 뷰](http://developer.android.com/training/material/shadows-clipping.html)합니다.


## <a name="color-features"></a>색 기능

Android 5.0는 앱에서 색을 관리 하기 위한 두 가지 새로운 기능을 제공 합니다.

-   *Drawable 색조* 레이아웃 특성을 변경 하 여 이미지 자산의 색을 변경할 수 있습니다.

-   *주요 색상을 추출* 동적으로 표시 된 이미지의 색상표를 사용 하 여 조정 하 여 앱의 색 테마를 사용자 지정할 수 있도록 합니다.


### <a name="drawable-tinting"></a>Drawable 색조

Android 5.0 레이아웃 인식 새 `tint` 다른 색을 표시 하려면 이러한 자산의 여러 버전을 만들지 않고도 드로어 블의 색을 설정 하는 데 사용할 수 있는 특성입니다. 이 기능을 사용 하려면 알파 마스크를 사용 하 여 비트맵 정의 `tint` 자산의 색을 정의 하는 특성입니다. 이렇게 하면 자산을 한 번 만들고 테마에 맞게 레이아웃에서 색상을 수 있습니다.

다음 예에서 단일 이미지 자산 &ndash; 투명 한 배경의 흰색 로고 &ndash; tint 변형을 만드는 데 사용 됩니다.

![투명 한 배경의 흰색 Xamarin 로고](lollipop-images/xamarin-logo-white.png)

이 로고는 다음 예와에서 같이 파란색 순환 배경 위에 표시 됩니다. 왼쪽 이미지는 로고 없이 표시 되는 방식을 `tint` 설정 합니다. 가운데 이미지에서 로고의 `tint` 특성을 진한 회색으로 설정 됩니다. 오른쪽의 그림과에서 `tint` 연한 회색으로 설정 됩니다.

![다른 tint 설정 사용 하 여 위의 로고의 예](lollipop-images/drawable-tinting.png)

Drawable 색조 Android 5.0에 대 한 자세한 내용은 참조 하세요 [Drawable 색조](http://developer.android.com/training/material/drawables.html#DrawableTint)합니다.


### <a name="prominent-color-extraction"></a>주요 색상을 추출

새 Android 5.0 `Palette` 클래스를 사용 하면 사용자 지정 색 색상표를 동적으로 적용할 수 있도록 이미지에서 색을 추출 합니다. `Palette` 클래스 이미지에서 6 개 색을 추출 하 고 레이블 색 채도 및 밝기 상대 수준에 따라 이러한 색:

-   활발 한

-   활발 한 어둡게

-   활발 한 조명

-   음소거

-   음소거 어둡게

-   Light 음소거

예를 들어, 다음 스크린샷과 사진 보기 앱을 표시에서 이미지에서 두드러진 색을 추출 및 이미지와 일치 하는 앱의 색 구성표에 맞게 이러한 색을 사용 하 여:

[![Pink, 녹색과 파란색 테마의 스크린 샷을 색 추출](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

위의 스크린샷에서 작업 모음 추출된 "활발 한 light"로 설정 되어 색 및 배경 추출된 "활발 한 어둡게"로 설정 되어 색입니다. 위의 예제에서는 작은 색 사각형 행 이미지에서 추출 된 색상표를 보여 주기 위해 포함 됩니다.

Android 5.0에서 색 추출에 대 한 자세한 내용은 참조 하세요 [이미지에서 추출 두드러진 색](http://developer.android.com/training/material/drawables.html#ColorExtract)합니다.


## <a name="new-ui-widgets"></a>새 UI 위젯

Android 5.0에는 두 개의 새 UI 위젯:

-   `RecyclerView` &ndash; 스크롤할 수 있는 항목의 목록을 표시 하는 보기 그룹입니다.

-   `CardView` &ndash; 모퉁이가 둥근 기본 레이아웃입니다.

두 위젯 재질 테마 기능 내재 지원이 포함 됩니다. 예를 들어 `RecyclerView` 애니메이션을 사용 하 여 추가 하 고 뷰를 제거 및 `CardView` 사용 하 여 배경 위에 float로 표시 하는 각 카드는 그림자를 확인 합니다. 이러한 새 위젯의 예는 다음 스크린샷과에서 같습니다.

[![RecyclerView로 빌드된 앱의 스크린샷](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

왼쪽 스크린샷에서 예가 `RecyclerView` 에 전자 메일 앱에서 스크린샷을 사용의 예로 `CardView` 여행 예약 응용 프로그램에서 사용 합니다.


### <a name="recyclerview"></a>RecyclerView

`RecyclerView` 비슷합니다 `ListView,` 있지만 대량의 뷰나 동적으로 변경 되는 요소를 사용 하 여 목록에 더 적합 합니다. 같은 `ListView,` 기본 데이터 집합에 액세스 하려면 어댑터를 지정 합니다. 그러나와 달리 `ListView,` 사용을 *레이아웃 관리자* 위치로 내의 항목 `RecyclerView`. 레이아웃 관리자도 담당 보기 재활용; 항목 보기 더 이상 사용자에 게 표시 되지 않는의 재사용을 관리 합니다.

사용 하는 경우는 `RecyclerView` 를 지정 해야 위젯에서 `LayoutManager` 및 어댑터. 이 그림에 표시 된 대로 `LayoutManager` 어댑터 간의 매개자는 및 `RecyclerView`:

![RecyclerView LayoutManager, 어댑터 및 데이터 집합을 지 원하는 다이어그램](lollipop-images/recyclerview-diagram.png)

다음 스크린샷에서 설명 된 `RecyclerView` 항목 100 개를 포함 하는 (각 항목은는 `ImageView` 및 `TextView`):

[![이미지를 통해 스크롤할 RecyclerView 앱의 스크린샷](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView` 이 큰 데이터 집합을 쉽게 처리 &ndash; 스크롤 간 목록의 시작 부분에서이 샘플에서 목록의 앱만 몇 초만에 합니다. `RecyclerView` 또한 애니메이션; 지원 사실, 항목 추가 및 제거에 대 한 애니메이션은 기본적으로 활성화 됩니다. 항목에 추가 될 때를 `RecyclerView`,이 일련의 스크린샷은 에서처럼에 페이드 아웃 합니다.

[![프레임의 사진 항목 페이드의 프레임 스크린샷](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

에 대 한 자세한 `RecyclerView`를 참조 하세요 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)합니다.


### <a name="cardview"></a>CardView

`CardView` 모퉁이가 둥근 부동 카드를 시뮬레이션 하는 간단한 보기가입니다. 때문에 `CardView` 기본 제공 뷰 그림자가 앱에 시각적 깊이 추가할 수는 쉬운 방법을 제공 합니다. 다음 스크린샷은의 세 가지 텍스트 기반 예제를 보여 줍니다. `CardView`:

[![RecyclerView CardView 기반 항목을 사용 하 여 사용 하 여 앱의 예제 스크린샷](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

위의 예제에서 카드의 각각 포함 하는 `TextView`; 배경색을 통해 설정 됩니다는 `cardBackgroundColor` 특성입니다.

에 대 한 자세한 `CardView`를 참조 하세요 [CardView](~/android/user-interface/controls/card-view.md)합니다.


## <a name="enhanced-notifications"></a>향상 된 알림

새 시각적 형식 및 새로운 기능을 사용 하 여 Android 5.0에서 알림 시스템 크게 업데이트 되었습니다. Android 5.0에서 알림을 새롭게 개편을 했습니다. 예를 들어, Android 5.0에서 알림을 이제는 밝은 배경의 통해 어두운 텍스트를 사용합니다.

![확장된 되지 않은 Android 5.0 알림 발생의 예](lollipop-images/expanded-notification-contracted.png)

큰 아이콘 (처럼 위의 예제) 알림이 표시 되 면 Android 5.0 작은 아이콘 큰 아이콘 배지를 제공 합니다. 

Android 5.0에서 알림 장치 잠금 화면에 나타날 수도 있습니다.
예를 들어, 다음은 단일 알림이 잠금 화면의 스크린 샷 예가입니다.

[![잠금 화면에 나타나는 알림이 스크린샷](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

사용자 수 두 번 눌러서 장치의 잠금을 해제 하 고 해당 알림을 발생 하는 앱으로 이동 하려면 잠금 화면에서 알림 또는 안쪽으로 살짝 밀어 알림을 해제 합니다. 알림이 있는 새 *가시성* 얼 만큼의 콘텐츠가 잠금 화면에 표시할 수 있습니다를 결정 하는 설정입니다. 잠금 화면 알림을 표시할 중요 한 콘텐츠를 허용할 것인지를 선택할 수 있습니다.

Android 5.0 라는 새로운 우선 순위가 높은 알림 프레젠테이션 형식을 제공 *헤 즈 업*합니다. 헤 즈 업 알림 몇 초간 화면 맨 위에서 아래로 슬라이드 및 다음 화면 맨 위에 있는 알림 음영으로 후퇴 합니다. 헤 즈 업 알림 수 있도록 시스템 현재 실행 중인 작업을 방해 하지 않고 사용자 앞에 중요 한 정보를 배치 하는 UI에 대 한 합니다. 다음 예제에서는 앱을 기반으로 표시 하는 간단한 헤드업 알림을 보여 줍니다.

[![Heads-up 알림의 예제](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

헤 즈 업 알림은 다음 이벤트에 대 한 일반적으로 사용 됩니다.

-   다음 새 메시지

-   들어오는 전화 통화

-   배터리 표시

-   경보

Android 5.0가 높거나 최대 우선 순위 설정 하는 경우에 헤 즈 업 형식에는 알림이 표시 됩니다.

Android 5.0에서 Android 정렬 하 고 더 지능적으로 알림을 표시 하는 데 알림 메타 데이터를 제공할 수 있습니다. Android 5.0에는 우선 순위, 표시 유형 및 범주에 따라 알림을 구성합니다.
알림 범주는 필터링 하는 데 장치 상태인 경우 알림을 표시 될 수 있습니다 *방해 금지* 모드입니다.

만들기 및 최신 Android 5.0 기능을 사용 하 여 알림을 시작 하는 방법에 대 한 자세한 내용은 참조 하세요. [로컬 알림을](~/android/app-fundamentals/notifications/local-notifications.md)합니다.


## <a name="new-apis"></a>새로운 API

Android 5.0 위에 설명 된 새 느낌 및 기능 외에도 기존 멀티미디어의 기능, 저장소 및 무선/연결 기능을 확장 하는 새로운 Api를 추가 합니다. 또한 Android 5.0 새로운 작업 스케줄러 기능에 대 한 지원을 제공 하는 새 Api를 포함 합니다.

### <a name="camera"></a>카메라

Android 5.0 카메라 향상 된 기능에 대 한 몇 가지 새로운 Api를 제공합니다. 새 `Android.Hardware.Camera2` 네임 스페이스는 Android 장치에 연결 된 개별 웹캠 장치 액세스에 대 한 기능이 포함 되어 있습니다. 또한 `Android.Hardware.Camera2` 파이프라인으로 각 카메라 장치 모델: 캡처 요청을 수락, 이미지를 캡처하고 다음 결과 출력 합니다. 이 방법은 앱이 카메라 장치로 여러 캡처 요청을 큐에 대 한 수 있습니다.

다음 Api 가능 이러한 새로운 기능:

-   `CameraManager.GetCameraIdList` &ndash; 장치 카메라를 프로그래밍 방식으로 액세스 하는 데 도움이 사용할 `CameraManager.OpenCamera` 특정 카메라 장치에 연결 합니다.

-   `CameraCaptureSession` &ndash; 캡처 또는 장치 카메라에서에서 이미지를 스트리밍합니다. 구현 하는 `CameraCaptureSession.CaptureListener` 새 이미지 캡처 이벤트를 처리 하는 인터페이스입니다.

-   `CaptureRequest` &ndash; 캡처 매개 변수를 정의합니다.

-   `CaptureResult` &ndash; 이미지 캡처 작업의 결과 제공합니다.

새 카메라 Api Android 5.0에 대 한 자세한 내용은 참조 하세요 [미디어](http://developer.android.com/about/versions/android-5.0.html#Media)합니다.

### <a name="audio-playback"></a>오디오 재생

Android 5.0 업데이트는 `AudioTrack` 더 나은 오디오 재생에 대 한 클래스:

-   `ENCODING_PCM_FLOAT` &ndash; 구성 `AudioTrack` 더 나은 동적 범위, 큰 헤드룸 및 더 높은 품질로 (늘어난된 전체 자릿수) 덕분에 대 한 부동 소수점 형식에서 오디오 데이터를 허용 하도록 합니다. 또한 부동 소수점 형식을 오디오 클립을 방지할 수 있습니다.

-   `ByteBuffer` &ndash; 이제 오디오 데이터를 제공할 수 있습니다 `AudioTrack` 바이트 배열입니다.

-   `WRITE_NON_BLOCKING` &ndash; 이 옵션을 버퍼링 간소화 및 일부 앱에 대 한 다중 스레딩 합니다.

에 대 한 자세한 `AudioTrack` Android 5.0에서 향상 된 기능 참조 [미디어](http://developer.android.com/about/versions/android-5.0.html#Media)합니다.

### <a name="media-playback-control"></a>미디어 재생 제어

Android 5.0에는 새 `Android.Media.MediaController` 클래스를 대체 `RemoteControlClient`합니다. `Android.Media.MediaController` 간소화 된 전송 제어 Api를 제공 하 고 스레드로부터 안전한 재생 UI 컨텍스트 외부에서 제어를 제공 합니다. 전송 컨트롤을 처리 하는 다음과 같은 새로운 Api:

-   `Android.Media.Session.MediaSession` &ndash; 미디어는 컨트롤러가 여러 개를 처리 하는 세션을 제어 합니다. 호출 `MediaSession.GetSessionToken` 세션과 상호 작용 하는 앱 토큰을 요청 합니다.

-   `MediaController.TransportControls` &ndash; 처리와 같은 명령 전송 **재생**를 **중지**, 및 **건너뛰기**합니다.

또한 사용할 수 있습니다 새 `Android.App.Notification.MediaStyle` 풍부한 알림 콘텐츠 (예: 압축을 풀어 앨범 표지 표시)를 사용 하 여 미디어 세션을 연결 하는 클래스입니다.

Android 5.0의 새로운 미디어 재생 제어 기능에 대 한 자세한 내용은 참조 하세요 [미디어](http://developer.android.com/about/versions/android-5.0.html#Media)합니다.

### <a name="storage"></a>저장소

Android 5.0 응용 프로그램 디렉터리 및 문서 작업을 쉽게 수행할 수 있도록 저장소 액세스 프레임 워크를 업데이트 합니다.

-   디렉터리 하위 트리를 선택 하려면 빌드을 보낼 수 있습니다는 `Android.Intent.Action.OPEN_DOCUMENT_TREE` 의도 합니다. 이 인해 시스템 하위 트리 선택;를 지 원하는 모든 공급자 인스턴스를 표시 하려면 그런 다음 사용자를 탐색 하 고 디렉터리를 선택 합니다.

-   를 만들고 새 문서 또는 하위 트리의 아무 곳 이나 디렉터리를 관리 하려면 새 사용할 `CreateDocument`, `RenameDocument`, 및 `DeleteDocument` 메서드의 `DocumentsContract`합니다.

-   새 호출에 모든 공유 저장소 장치의 미디어 디렉터리 경로 얻으려면 `Android.Content.Context.GetExternalMediaDirs` 메서드.

새 저장소 Api Android 5.0에 대 한 자세한 내용은 참조 하세요 [저장소](http://developer.android.com/preview/api-overview.html#Storage)합니다.

### <a name="wireless--connectivity"></a>무선 및 연결

Android 5.0 무선 및 연결에 대 한 다음 API 향상 기능을 추가합니다.

-   새 *다중 네트워크* Api 앱을 찾아 연결 하기 전에 특정 기능을 사용 하 여 네트워크를 선택할 수 있도록 합니다.

-   역할을 Bluetooth 저 에너지 주변 장치를 Android 5.0 장치를 사용할 수 있는 Bluetooth 브로드캐스팅 기능이 있습니다.

-   쉽게 근거리 통신 기능을 사용 하 여 다른 장치를 사용 하 여 데이터를 공유 하는 nfc 지원 향상.

새 무선 및 Android 5.0에서 Api 연결에 대 한 자세한 내용은 참조 하세요 [무선 및 연결](http://developer.android.com/preview/api-overview.html#Wireless)합니다.

### <a name="job-scheduling"></a>작업 예약

Android 5.0에는 새 `JobScheduler` 사용자를 도울 수 있는 API 장치가 연결 되어 있는 경우에 실행 하려면 특정 작업을 예약 하 고 청구 하 여 배터리가 최소화 합니다. 또한이 작업 스케줄러 기능 조건이 요금제 네트워크 대신 Wi-fi 네트워크를 통해 연결 되어 있는 경우 큰 파일을 다운로드 하는 등의 해당 작업에 더 적합 한 경우를 실행 하는 작업을 예약 하는 데 사용할 수 있습니다.

새 작업 예약 Android 5.0에서 Api에 대 한 자세한 내용은 참조 하세요 [예약 작업](http://developer.android.com/preview/api-overview.html#JobScheduler)합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Android 앱 개발자를 위한 Android 5.0의 중요 한 새로운 기능 개요를 제공 합니다.

-   재질 테마

-   애니메이션

-   보기 그림자 및 권한 상승

-   Drawable 색조 등 주요 색상 기능 추출 색

-   새 `RecyclerView` 고 `CardView` 위젯

-   알림 향상

-   카메라, 오디오 재생, 미디어 제어, 저장소, 무선 연결 및 작업 예약에 대 한 새로운 Api

Xamarin Android 개발을 처음 접하는 경우 읽을 [설정 및 설치](~/android/get-started/installation/index.md) Xamarin.Android를 사용 하 여 시작할 수 있도록 합니다.
[Hello, Android](~/android/get-started/hello-android/index.md) 는 Android 프로젝트를 만드는 방법에 대 한 훌륭한 소개 합니다.



## <a name="related-links"></a>관련 링크

- [Android L 개발자 미리 보기](http://developer.android.com/preview/index.html)
- [Android SDK 가져오기](https://developer.android.com/sdk/index.html#Other)
- [재질 디자인](http://developer.android.com/preview/material/index.html)
- [재질 디자인 원칙](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
