---
title: "롤리팝 기능"
description: "이 문서에서는 Android 5.0 (롤리팝)에 도입 된 새로운 기능의 간략 한 개요를 제공 합니다. 이러한 기능에는 애니메이션, 보기 그림자를 그릴 색조 등 새로운 지원 기능 뿐만 아니라 자료 테마 라는 새 사용자 인터페이스 스타일을 포함 합니다. Android 5.0에는 저장소, 네트워킹, 연결 및 멀티미디어 기능을 개선 하기 위해 향상 된 알림, 두 개의 새 UI 위젯, 작업 스케줄러는 새 및 몇 가지 새로운 Api도 포함 되어 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: de6829a0a698133ad9002ead1cd7c534a30b1f6c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="lollipop-features"></a>롤리팝 기능

_이 문서에서는 Android 5.0 (롤리팝)에 도입 된 새로운 기능의 간략 한 개요를 제공 합니다. 이러한 기능에는 애니메이션, 보기 그림자를 그릴 색조 등 새로운 지원 기능 뿐만 아니라 자료 테마 라는 새 사용자 인터페이스 스타일을 포함 합니다. Android 5.0에는 저장소, 네트워킹, 연결 및 멀티미디어 기능을 개선 하기 위해 향상 된 알림, 두 개의 새 UI 위젯, 작업 스케줄러는 새 및 몇 가지 새로운 Api도 포함 되어 있습니다._

## <a name="lollipop-overview"></a>롤리팝 개요

Android 5.0 (롤리팝)에 새로운 디자인 언어 *자료 디자인*, 더욱 쉽고 사용 하도록 앱을 확인 하는 새로운 기능이의 캐스팅을 지 원하는 것으로 합니다. 자료 디자인에서는 Android 5.0 뿐만 아니라 제공 Android 휴대폰 생기; 또한 Android 기반 태블릿, 데스크톱 컴퓨터, 보기, 그리고 스마트 Tv에 대 한 새로운 디자인 규칙 집합이 제공합니다. 디자인에 주안점을 간단 하 고 미니 멀리즘 수행 하는 동안 사용자가 신속 하 게 하 고 직관적으로 이해 인터페이스에 친숙 한 tactile 특성 (예: 현실적인 가장자리와 화면 표시)를 사용 합니다.

*자재 테마* 는 Android에서 UI 디자인 원칙 이러한의 전형입니다. 이 문서는 자료 테마 지원 기능을 포함 하 여 시작 됩니다.

-   **애니메이션** &ndash; *피드백 터치* 애니메이션 *작업 전환* 애니메이션 *상태 전환 보기* 애니메이션과 *효과 표시*합니다.

-   **그림자 및 권한 상승 보기** &ndash; 뷰 생깁니다는 `elevation` 속성; 뷰를 더 높은 `elevation` 값 마스터 페이지에 더 큰 그림자를 캐스팅 합니다.

-   **기능 색** &ndash; *그릴 색조* 해당 색을 변경 하 여 이미지 자산을 재사용할 수 가능 하 게 하 고 *두드러진 색상을 추출* 동적으로 사용 하면 테마 이미지의에서 색상을 기준으로 앱 합니다.

기능에 이미 내장 된 여러 자료 테마 Android 5.0 UI, 동안 될 다른 명시적으로에 추가 해야 앱. 예를 들어 일부 표준 보기 (예: 단추) 앱에는 대부분 보기 그림자 사용 하도록 설정 해야 하는 동안 터치 피드백 애니메이션 이미 포함 되어 있습니다.

자료 테마를 통해 일으킵니다 UI 개선 사항 외에도 Android 5.0이이 문서에 설명 되어 있는 다른 몇 가지 새로운 기능이 포함 합니다.

-   **알림 향상** &ndash; Android 5.0에서 알림을 새로운 디자인, 잠금 화면 알림, 지원 및 새으로 크게 업데이트 되었습니다 *헤드업* 알림 표시 형식입니다.

-   **새 UI 위젯** &ndash; 새 `RecyclerView` 위젯을 사용 하면 큰 데이터 집합 및 복잡 한 정보 및 새 전달 하기 위해 응용 프로그램에 쉽게 `CardView` 위젯 텍스트를 표시 하기 위한 간소화 된 카드 같은 표시 형식을 제공 하 고 이미지입니다.

-   **새 Api** &ndash; Bluetooth 연결과 저장소 관리를 간소화 하기 보다 유연한 제어 멀티미디어 플레이어 및 장치 카메라 Android 5.0 향상 된 다중 네트워크 지원 위한 새 Api가 추가 합니다. 새 작업 일정 기능에 맞게 비동기적으로 작업을 실행할 수 있습니다.은 예약 된 시간입니다. 이 기능은 by 배터리 수명을 개선 하려면 예를 들어 장치 연결 된 경우 위치 및 청구를 수행 하기 위해 작업을 예약 합니다.


## <a name="requirements"></a>요구 사항

다음은 Xamarin 기반 앱에서 새 Android 5.0 기능을 사용 하려면 필요 합니다.

-   **Xamarin.Android** &ndash; Xamarin.Android 4.20 이상을 설치 하 고 Mac.에 대 한 Visual Studio 또는 Visual Studio를 사용 하 여 구성 해야 

-   **Android SDK** &ndash; Android 5.0 (API 21) 나중에 설치 되어 있어야 Android SDK Manager를 통해 합니다.

-   **Java 개발자 키트** &ndash; Xamarin.Android 필요 [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 또는 24 API 수준에 대 한 개발 하는 경우 이후 이상 (JDK 1.8 API에서는 수준을 지원 롤리팝을 포함 하 여 24 보다 이전 버전). JDK 1.8의 64 비트 버전은 사용자 지정 컨트롤 또는 양식 미리 보기를 사용 하는 경우에 필요 합니다.

계속 사용할 수 있습니다 [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API 수준 23에 맞게 개발 이하의 경우.


## <a name="setting-up-an-android-50-project"></a>Android 5.0 프로젝트 설정

Android 5.0 프로젝트를 만들려면 최신 도구와 SDK 패키지를 설치 해야 합니다. 대상으로 Android 5.0 Xamarin.Android 프로젝트를 설정 하려면 다음 단계를 사용 합니다.

1. Xamarin.Android 도구를 설치 하 고 Xamarin 라이선스를 활성화 합니다. 참조 [설정 및 설치](~/android/get-started/installation/index.md) Xamarin.Android 설치에 대 한 자세한 내용은 합니다.

2. Mac 용 Visual Studio를 사용 하는 경우 최신 Android 5.0 업데이트를 설치 합니다.

3. Android SDK Manager를 시작 (Mac 용 Visual Studio에서 사용 하 여 **도구 &gt; 열려 Android SDK Manager&hellip;**) 23.0.5 Android SDK 도구를 설치 및 이상 버전:

    [![Android SDK Manager에서 Android SDK 도구 선택](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   또한 최신 Android 5.0 SDK 패키지 (API 21)를 설치 합니다.

    [![Android SDK Manager에서 Android 5.0 SDK 패키지를 설치합니다.](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   Android SDK Manager를 사용 하는 방법에 대 한 자세한 내용은 참조 [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html)합니다.

4. 새 Xamarin.Android 프로젝트를 만듭니다. Xamarin 사용한 Android 개발을 처음 접하는 경우 참조 [Hello, Android](~/android/get-started/hello-android/index.md) Android 프로젝트 만들기에 대 한 자세한 내용은 합니다. Android 프로젝트를 만들 때 Android 5.0에 대 한 버전 설정을 구성 해야 합니다.
   Mac 용 Visual Studio에서로 이동 **프로젝트 옵션 &gt; 빌드 &gt; 일반** 설정 **대상 프레임 워크** 를 **Android 5.0 (롤리팝)** 또는 나중에:

    ![Android 5.0 롤리팝 대상 Framwework 설정](lollipop-images/target-framework.png)

   아래 **프로젝트 옵션 &gt; 빌드 &gt; Android 응용 프로그램**, 최소 설정 하 고 Android 버전을 대상 **자동으로 사용 하 여 대상 프레임 워크 버전**:

    ![최소 및 대상 Android 버전을 자동으로 설정](lollipop-images/minimum-android-version.png)

5. 응용 프로그램을 테스트 하려면 에뮬레이터 또는 Android 장치를 구성 합니다. 에뮬레이터를 사용 하는 경우 참조 [Android 에뮬레이터 설정](~/android/get-started/installation/android-emulator/index.md) Xamarin Studio 또는 Visual Studio와 함께 사용 하기 위해 Android 에뮬레이터를 구성 하는 방법을 배울 수 있습니다. Android 장치를 사용 하는 경우 참조 [the Preview SDK 설정을](https://developer.android.com/preview/setup-sdk.html) Android 5.0 장치를 업데이트 하는 방법을 배울 수 있습니다. 실행 및 Xamarin.Android 응용 프로그램 디버깅에 대 한 Android 장치를 구성 하려면 참조 [개발에 대 한 설정으로 장치](~/android/get-started/installation/set-up-device-for-development.md)합니다.

참고: Android L 미리 보기를 대상으로 지정 된 기존 Android 프로젝트를 업데이트 하는 경우 업데이트 해야는 **대상 프레임 워크** 및 **Android 버전** 위에서 설명한 값에 있습니다.

## <a name="important-changes"></a>중요 한 변경 내용

이전에 게시 된 Android 앱은 Android 5.0에서 변경 내용에 의해 저하 될 수 있습니다. 특히, Android 5.0에서는 새로운 런타임 및 크게 변경 된 알림 형식을 사용합니다.

### <a name="android-runtime"></a>Android 런타임

Android 5.0 Dalvik 대신 기본 런타임으로 새 Android 런타임 (아트)를 사용합니다. 아트는 몇 가지 주요 새로운 기능을 구현합니다.

-   **미리-(AOT) 컴파일** &ndash; AOT 앱이 처음 시작 되기 전에 응용 프로그램 코드를 컴파일 하 여 앱 성능을 향상 시킬 수 있습니다. 앱을 설치할 때 아트는 컴파일된 응용 프로그램 대상 장치에 대 한 실행 파일을 생성 합니다.

-   **GC (가비지 수집) 향상 된** &ndash; 아트의 GC 향상 앱 성능을 향상 시킬 수도 있습니다. 가비지 컬렉션에는 이제 1 GC 일시 중지를 사용 하 여, 2 개가 아닌 및 더 적절 한 시간 내에 동시 GC 작업을 완료 합니다.

-   **응용 프로그램 디버깅 향상** &ndash; 아트 예외를 분석 하 고 충돌 보고서에 더 많은 진단 정보를 제공 합니다.

아트에서 변경 하지 않고 기존 앱이 작동 해야 &ndash; 이전 Dalvik 런타임을에 고유한 기법을 악용 하는 응용 프로그램을 제외 하 고는 작동 하지 않을 아트에서 합니다. 이러한 변경에 대 한 자세한 내용은 참조 [확인 앱 동작에는 Android 런타임 (아트)](http://developer.android.com/guide/practices/verifying-apps-art.html)합니다.


### <a name="notification-changes"></a>알림 변경

알림 Android 5.0에서 크게 변경 되었습니다.

-   **사운드 및 진동 다르게 처리** &ndash; 알림 소리 및 진동 이제 의해 처리 됩니다 `Notification.Builder` 대신 `Ringtone`, `MediaPlayer`, 및 `Vibrator`합니다.

-   **새로운 색 구성표** &ndash; 자료 테마에 따라 알림을 텍스트로 렌더링 어두운 흰색 또는 매우 적은 배경에 합니다. 또한 시스템 색 구성표와 조정 하기 위해 Android에서 알림 아이콘에서 알파 채널을 수정할 수 있습니다. 

-   **잠금 화면 알림** &ndash; 알림 수 이제 장치 잠금 화면에 나타납니다.

-   **헤드업** &ndash; 우선 순위가 높은 알림을 작은 부동 창 (헤드업 알림)에 표시 됨 경우 장치 잠금 해제 되어 있고 화면 켜져 있습니다.

대부분의 경우에서 Android 5.0에 기존 앱 알림 기능을 포팅 하려면 다음 단계:

1.  사용 하 여 코드 변환 `Notification.Builder` (또는 `NotificationsCompat.Builder`) 알림이 생성에 대 한 합니다. 

2.  기존 알림 자산 새 자료 테마 색 구성표에서 볼 수 있는지 확인 합니다.

3.  잠금 화면에서 제공 하는 경우 알림이 있어야 어떤 표시 여부를 결정 합니다. 알림을 public 인 경우 콘텐츠는 잠금 화면에 표시 해야?

4.  알림 범주를 설정 하 여 새 Android 5.0에서 올바르게 처리 되도록 *방해 하지* 모드입니다.

알림을 전송 컨트롤을 디스플레이 미디어 재생 상태를 표시를 사용 하 여 `RemoteControlClient`, 호출 또는 `ActivityManager.GetRecentTasks`, 참조 [중요 한 동작 변경 내용](http://developer.android.com/preview/api-overview.html#Behaviors) Android 용 알림 프로그램을 업데이트 하는 방법에 대 한 자세한 내용은 5.0입니다.

Android에서 알림이 생성 하는 방법에 대 한 정보를 참조 하십시오. [로컬 알림을](~/android/app-fundamentals/notifications/local-notifications.md)합니다. [호환성](~/android/app-fundamentals/notifications/local-notifications.md#compatibility) 이 문서의 섹션 아래쪽 호환 되는 알림을 만드는 방법에 설명 이전 버전의 Android 합니다.


## <a name="material-theme"></a>자재 테마

새 Android 5.0 자료 테마에서는 Android UI의 모양과 느낌을 대대적으로 변경 합니다. 시각적 요소는 이제 굵게 그래픽과 서체 인쇄 기반 디자인의 밝은 색에 사용 하는 tactile 화면을 사용 합니다. 자료 테마의 예는 다음 스크린샷에 표시 되는

[![자료 테마 홈 화면, 앱 화면 및 화면 설정의 스크린샷](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0는 왼쪽에 표시 된 홈 화면으로 반응 합니다. Center 스크린 샷 앱 목록의 첫 번째 화면 이며 오른쪽에 스크린 샷은 **설정을** 화면입니다. Google의 [자료 디자인](https://material.io/guidelines/material-design/introduction.html) 사양 뒤에 새로운 자료가 테마 개념이 기본 디자인 규칙에 설명 합니다.

자재 테마에 응용 프로그램에서 사용할 수 있는 세 가지 기본 제공 된 버전:는 `Theme.Material` 어두운 테마 (기본값) 이면는 `Theme.Material.Light` 테마 및 `Theme.Material.Light.DarkActionBar` 테마: 

[![어두운 스크린샷, 조명 및 DarkActionBar 테마](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

Xamarin.Android 앱의 자료 테마 기능을 사용 하는 방법에 대 한 자세한 내용은 [자료 테마](~/android/user-interface/material-theme.md)합니다.


## <a name="animations"></a>애니메이션

Android 5.0 터치 피드백 애니메이션, 활동 전환 애니메이션 및 응용 프로그램 인터페이스를 사용 하는 보다 직관적인 있도록 뷰 상태 전환 애니메이션을 제공 합니다. Android 5.0 앱을 사용할 수 또한 *효과 표시* 애니메이션을 숨기 거 나 뷰를 표시 합니다. 사용할 수 있습니다 *동작 곡선* 얼마나 빨리 구성 하는 설정을 또는 느리게 애니메이션 렌더링 됩니다.


### <a name="touch-feedback-animations"></a>터치 피드백 애니메이션

터치 피드백 애니메이션 보기 처리 된 경우 사용자에 게 시각적 피드백을 제공 합니다. 예를 들어 단추 이제 영향을 미치기 있을 때 표시 변함이 &ndash; Android 5.0에서 기본 터치 피드백 애니메이션입니다. Ripple 애니메이션 새에 의해 구현 됩니다 `RippleDrawable` 클래스입니다. 영향 보기의 범위를 종료 하거나 보기의 경계를 벗어나 확장 하도록 구성할 수 있습니다. 예를 들어 스크린 샷의 다음과 같은 단추에 큰 영향 터치 애니메이션 중:

![단추에 애니메이션 ripple의 프레임 스크린 샷 하 여 프레임](lollipop-images/touch-animation.png)

단추와 초기 터치 연락처 (왼쪽에서 오른쪽) 나머지 시퀀스를 단추 가장자리로 영향 확산 하는 방법을 보여 줍니다. 동안 첫 번째 이미지의 왼쪽에서 발생 합니다. Ripple 애니메이션 종료 되 면 원래 모양으로 뷰를 반환 합니다. 기본 ripple 애니메이션 초 소수 부분에서 수행 하지만 더 길거나 더 짧은 시간 길이 대 한 애니메이션의 길이 사용자 지정할 수 있습니다.

Android 5.0에서 피드백 애니메이션을 터치에 대 한 자세한 참조 [터치 피드백을 사용자 지정](http://developer.android.com/training/material/animations.html#Touch)합니다.


### <a name="activity-transition-animations"></a>활동 전환 애니메이션

활동 전환 애니메이션 하나의 활동 간 전환 될 때 사용자 의미 visual 연속성을 제공 합니다. 앱 세 가지 유형의 전환 애니메이션을 지정할 수 있습니다.

-   **전환 입력** &ndash; 활동 들어가면 장면에 대 한 합니다.

-   **전환 종료** &ndash; 활동 장면을 종료 하는 경우에 대 한 합니다.

-   **공유 요소 전환** &ndash; 두 활동에 공통 된 보기를 다음 첫 번째 활동 전환으로 변경 되는 경우에 대 한 합니다.

예를 들어 다음과 같은 순서의 스크린 샷 공유 요소 전환을 보여 줍니다.

[![공유 요소 전환 애니메이션의 프레임 스크린 샷 하 여 프레임](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

공유 요소 (한 애벌레 사진)는 첫 번째 활동;에 여러 가지 뷰 중 하나 것만 보기에는 두 번째 작업에서 두 번째 첫 번째 활동 전환으로 되도록 확대 합니다.

#### <a name="enter-transition-animation-types"></a>전환 애니메이션 유형 입력

Enter 전환에 대 한 Android 5.0에는 세 가지 유형의 애니메이션 제공합니다.

-   **애니메이션을 분해** &ndash; 장면의 가운데에서 보기를 확대 합니다.

-   **슬라이드 애니메이션** &ndash; 에서 장면이의 모서리 중 하나에서 보기를 이동 합니다.

-   **애니메이션에 페이드** &ndash; 장면에 대 한 뷰 사라집니다.

#### <a name="exit-transition-animation-types"></a>종료 전환 애니메이션 유형

종료 전환에 대 한 Android 5.0에는 세 가지 유형의 애니메이션 제공합니다.

-   **애니메이션을 분해** &ndash; 뷰 장면의 중심을 축소 합니다.

-   **슬라이드 애니메이션** &ndash; 보기 장면의 가장자리 중 하나로 이동 합니다.

-   **애니메이션에 페이드** &ndash; 페이드 장면 부족 보기.

#### <a name="shared-element-transition-animation-types"></a>전환 애니메이션 형식 요소를 공유합니다.

공유 요소 전환와 같은 여러 유형의 애니메이션을 지원합니다.

-   보기의 레이아웃 또는 클립 범위를 변경 합니다.

-   크기 조정 및 보기의 회전을 변경 합니다.

-   보기에 대 한 크기 및 규모가 형식을 변경 합니다.

Android 5.0에서 활동 전환 애니메이션에 대 한 자세한 내용은 [사용자 지정 활동 전환](http://developer.android.com/training/material/animations.html#Transitions)합니다.


### <a name="view-state-transition-animations"></a>뷰 상태 전환 애니메이션

Android 5.0에 대 한 애니메이션을 보기의 상태가 변경 될 때 실행 수 있습니다. 다음 방법 중 하나를 사용 하 여 보기 상태 전환 애니메이션을 적용할 수 있습니다.

-   특정 보기와 연결 된 상태 변경의 효과 적용 하는 drawables를 만듭니다. 새 `AnimatedStateListDrawable` 클래스 drawables 뷰 상태 변경 간 애니메이션을 표시 하는 만들 수 있습니다.

-   뷰 상태가 변경 될 때 실행 되는 애니메이션 기능을 정의 합니다. 새 `StateListAnimator` 클래스를 사용 하면 뷰 상태가 변경 될 때 실행 되는 애니메이터 정의할 수 있습니다.

Android 5.0의 보기 상태 전환 애니메이션에 대 한 자세한 내용은 [뷰 상태 변경 내용이 애니메이션 효과 주는](http://developer.android.com/training/material/animations.html#ViewState)합니다.


### <a name="reveal-effect"></a>효과 표시

*효과 표시* 클리핑 원 해당 변경 내용을 반지름을 표시 하거나 숨기는 보기입니다. 초기 및 최종 클리핑 원의 반지름을 설정 하 여이 영향을 제어할 수 있습니다. 스크린 샷의 다음과 같은 화면 중심에서 표시 효과 애니메이션:

[![표시 애니메이션의 프레임 스크린 샷 하 여 프레임](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

다음 화면 왼쪽된 아래 모서리에서 일어나는 표시 효과 애니메이션을 보여 줍니다.

[![프레임 여 클리핑 애니메이션의 프레임 스크린 샷](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

애니메이션을 되돌릴 수를 표시 합니다. 즉, 클리핑 원 보기 숨기려면 축소 하지 않고 수는 보기를 표시 하기 위해 확대.

Android 5.0 표시 효과에 대 한 자세한 내용은 참조 하십시오. [표시 효과 사용 하 여](http://developer.android.com/training/material/animations.html#Reveal)합니다.


### <a name="curved-motion"></a>곡선된 동작

이러한 애니메이션 기능 외에도 Android 5.0 또한 애니메이션의 경우 시간 및 동작 곡선을 지정할 수 있도록 하는 새로운 Api를 제공 합니다. Android 5.0 이러한 곡선을 사용 하 여 시간 및 공간 이동 애니메이션 중 보간합니다. 세 가지 곡선 Android 5.0에서 정의 됩니다.

-   **빠른\_아웃\_선형\_에** &ndash; 신속 하 게 가속화 하 고 가속화 애니메이션의 끝까지 계속 합니다.

-   **빠른\_아웃\_느린\_에** &ndash; 애니메이션의 끝까지 감속 신속 하 고 느린 Accelerates 합니다.

-   **선형\_아웃\_느린\_에** &ndash; 최대 개발 속도 및 느리게 시작 애니메이션의 끝에 감속 합니다.

새 사용할 수 있습니다 `PathInterpolator` 동작 보간을 수행 방법을 지정 하는 클래스입니다. `PathInterpolator` 지정 된 제어점 및 모션 곡선에 따라 애니메이션 경로 통과 하는 보간이입니다. Android 5.0에서 곡선된 동작 설정을 지정 하는 방법에 대 한 자세한 내용은 참조 [곡선 동작을 사용 하 여](http://developer.android.com/training/material/animations.html#CurvedMotion)합니다.


## <a name="view-shadows--elevation"></a>보기 그림자 및 권한 상승

Android 5.0에서 지정할 수 있습니다는 *상승* 새 설정 하 여 뷰의 `Z` 속성입니다. 큼 `Z` 값을 float 배경 위에 더 높은 같지 뷰가 백그라운드에 더 큰 별다른에 뷰를 사용 하면 됩니다. 뷰의 초기 상승의 경우 구성 하 여 설정할 수 있습니다 해당 `elevation` 레이아웃의 특성입니다.

다음 예제에서는 빈에서 캐스팅 그림자 `TextView` 의 승격 속성이로 설정 된 경우 2dp, 4dp, 및 6dp를 각각 제어 합니다.

[![더 큰 progessively의 스크린 샷을 보려면 그림자](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

그림자 설정 보기 (위와 같이) 정적 수 또는 보기의 배경 위에 일시적으로 증가 하 고 표시 하는 보기를 만들 애니메이션에 사용할 수 있습니다. 사용할 수는 `ViewPropertyAnimator` 상승의 경우 보기의 애니메이션 효과를 줄 클래스입니다. 보기의 상승 레이아웃의 합계인 `elevation` 설정 뒤에 `translationZ` 통해 설정할 수 있는 속성은 `ViewPropertyAnimator` 메서드를 호출 합니다.

Android 5.0에서 보기 그림자에 대 한 자세한 내용은 [그림자 정의 및 클리핑 뷰](http://developer.android.com/training/material/shadows-clipping.html)합니다.


## <a name="color-features"></a>색상 기능

Android 5.0에는 앱에서 색을 관리 하기 위한 두 가지 새로운 기능이 제공 합니다.

-   *그릴 색조* 레이아웃 특성을 변경 하 여 이미지 자산의 색을 변경할 수 있습니다.

-   *두드러진 색 추출* 동적으로 표시 된 이미지의 색상표와 조정 하기 위해 응용 프로그램의 색 테마를 사용자 지정할 수 있습니다.


### <a name="drawable-tinting"></a>그릴 색조

새 android 5.0 레이아웃 인식 `tint` 를 여러 버전의 서로 다른 색을 표시 하도록 이러한 자산을 만들 필요 없이 drawables의 색을 설정 하는 데 사용할 수 있는 특성입니다. 이 기능을 사용 하는 비트맵으로 정의한 알파 마스크 및 사용 된 `tint` 자산의 색을 정의 하는 특성입니다. 이렇게 하면 자산을 한 번만 만들고 테마에 맞게 레이아웃에서에 색을 수 있습니다.

다음 예에서 단일 이미지 자산 &ndash; 투명 한 배경의 흰색 로고 &ndash; tint 변형을 만드는 데 사용 됩니다.

![투명 한 배경의 흰색 Xamarin 로고](lollipop-images/xamarin-logo-white.png)

다음 예제에 나와 있는 것 처럼이 로고는 파란색 원형 배경 위에 표시 됩니다. 왼쪽 이미지는 로고 없이 표시 하는 방법을 `tint` 설정 합니다. 가운데 이미지에는 로고의 `tint` 특성이 진한 회색으로 설정 되어 있습니다. 오른쪽 이미지에 `tint` 연한 회색으로 설정 됩니다.

![다른 tint 설정 사용 하 여 위의 로고의 예](lollipop-images/drawable-tinting.png)

Android 5.0에 그릴 색조 하는 방법에 대 한 자세한 내용은 [그릴 색조](http://developer.android.com/training/material/drawables.html#DrawableTint)합니다.


### <a name="prominent-color-extraction"></a>두드러진 색상을 추출

새 Android 5.0 `Palette` 클래스를 사용 하는 사용자 지정 색상표를 동적으로 적용할 수 있도록 이미지에서 색을 추출할 수 있습니다. `Palette` 클래스 이미지에서 6 개 색을 추출 하 고 색 채도 및 밝기 상대 수준에 따라 이러한 색의 레이블을:

-   넘치는

-   넘치는 어둡게

-   넘치는 light

-   음소거

-   음소거 어둡게

-   음소거 light

예를 들어 다음 스크린샷에서 사진 앱 보기 두드러진 색 디스플레이에 이미지에서 추출 및 이미지에 맞게 응용 프로그램의 색 구성표에 맞게 이러한 색을 사용 하 여:

[![녹색으로 및 파랑 테마 색 추출의 스크린샷](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

위의 스크린샷에서 작업 모음 압축 푼된 "넘치는 조명"로 설정 되어 색과 배경 압축 푼된 "넘치는 어둡게"로 설정 된 색입니다. 위의 각 예제에 작은 색 사각형의 행은 이미지에서 추출 된 색상표 색을 설명 하기 위해 포함 됩니다.

Android 5.0의 색상을 추출 하는 방법에 대 한 자세한 내용은 [이미지에서 두드러진 색 추출](http://developer.android.com/training/material/drawables.html#ColorExtract)합니다.


## <a name="new-ui-widgets"></a>새 UI 위젯

Android 5.0에는 두 개의 새 UI 위젯 소개합니다.

-   `RecyclerView` &ndash; 스크롤할 수 있는 항목의 목록을 표시 하는 보기 그룹입니다.

-   `CardView` &ndash; 모퉁이가 둥근 기본 레이아웃입니다.

두 위젯 자료 테마 기능에는 처리 된 지원이 포함 예를 들어 `RecyclerView` 애니메이션을 사용 하 여 뷰를를 추가 및 제거 하 고 `CardView` 사용 하 여 볼 그림자 각 카드의 배경 위에 배치 하는 것 같습니다. 이러한 새로운 위젯의 예는 다음 스크린샷에서에 나와 있습니다.

[![RecyclerView를 사용 하 여 빌드한 앱의 스크린 샷](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

왼쪽 스크린샷에서의 예로 `RecyclerView` 에 스크린샷 및 전자 메일 응용 프로그램에서 사용 되는 오른쪽의 예는 `CardView` 여행 예약 응용 프로그램에서 사용 되는 합니다.


### <a name="recyclerview"></a>RecyclerView

`RecyclerView` 유사한 `ListView,` 있지만 뷰 또는 동적으로 변경 하는 요소가 포함 된 목록을의 큰 집합에 대해 더 잘 맞습니다. 마찬가지로 `ListView,` 기본 데이터 집합에 액세스 하도록 어댑터를 지정 합니다. 그러나 달리 `ListView,` 사용 하면 한 *레이아웃 관리자* 내의 항목 위치를 지정할 `RecyclerView`합니다. 레이아웃 관리자 작업도 수행 보기 재활용; 더 이상 사용자에 게 표시 되지 않는 항목 보기의 재사용을 관리 합니다.

사용 하는 경우는 `RecyclerView` 지정 해야 위젯는 `LayoutManager` 및 어댑터. 이 그림에 나와 있는 것 처럼 `LayoutManager` 는 어댑터 간의 매개자 및 `RecyclerView`:

![LayoutManager, 어댑터 및 데이터 집합을 지 원하는 RecyclerView의 다이어그램](lollipop-images/recyclerview-diagram.png)

다음 스크린샷에서 보여 주기는 `RecyclerView` 100 항목이 포함 된 (각 항목은 구성 되어는 `ImageView` 및 `TextView`):

[![이미지를 통해 스크롤할 RecyclerView 응용 프로그램의 스크린 샷](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView` 이 큰 데이터 집합으로 쉽게 처리 &ndash; 스크롤 종료 목록의 시작 부분에서이 샘플에서 목록의 응용 프로그램은 몇 초 밖에 걸리지 합니다. `RecyclerView` 또한 애니메이션; 지원 사실, 항목 추가 및 제거에 대 한 애니메이션 기본적으로 활성화 됩니다. 에 항목을 추가 하는 경우는 `RecyclerView`, 스크린 샷의이 시퀀스에 나와 있는 것 처럼에 사라지기:

[![프레임으로의 사진 항목 옅은 색의 프레임 스크린 샷](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

에 대 한 자세한 `RecyclerView`, 참조 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)합니다.


### <a name="cardview"></a>CardView

`CardView` 모퉁이가 둥근 부동 카드를 시뮬레이션 하는 간단한 보기가입니다. 때문에 `CardView` 기본 제공 뷰 그림자가, 시각적 깊이 응용 프로그램에 추가할 수는 쉬운 방법을 제공 합니다. 다음 스크린샷에서의 세 가지 텍스트 기반 예제를 보여 줍니다. `CardView`:

[![CardView 기반 항목과 RecyclerView를 사용 하 여 앱 스크린 샷을 예제](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

각 위의 예제에서 카드를 포함 한 `TextView`; 배경색을 통해 설정 됩니다는 `cardBackgroundColor` 특성입니다.

에 대 한 자세한 `CardView`, 참조 [CardView](~/android/user-interface/controls/card-view.md)합니다.


## <a name="enhanced-notifications"></a>향상 된 알림

Android 5.0에서 알림 시스템 크게 새 시각적 형식 및 새로운 기능으로 업데이트 되었습니다. 알림 Android 5.0의 새로운이 있어야 합니다. 예를 들어 Android 5.0에서 알림을 사용 하 여 진한 텍스트 밝은 배경 위에.

![Android 5.0 알림이 되지 않은 확장 된 예제](lollipop-images/expanded-notification-contracted.png)

큰 아이콘이 (위의 예에 표시)으로 알림 표시 되 면 Android 5.0는 작은 아이콘을 큰 아이콘 위에 배지를 나타냅니다. 

Android 5.0 장치 잠금 화면에 알림을 나타날 수도 있습니다.
예를 들어 다음은 예제 스크린샷은 단일 알림 사용 하 여 잠금 화면입니다.

[![잠금 화면에 표시 되는 알림 스크린 샷](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

사용자 수를 두 번 누릅니다 장치 잠금을 해제 하 해당 알림을 발생 시킨 앱으로 이동 하려면 잠금 화면에서 알림 또는 알림을 해제할을 살짝 밉니다. 알림이 새 *가시성* 내용의 양을 잠금 화면에 표시 될 수를 결정 하는 설정입니다. 잠금 화면 알림에 표시할 중요 한 내용이 수 있도록 할지 여부를 선택할 수 있습니다.

Android 5.0 이라고 하는 새 우선 순위가 높은 알림 표시 형식에는 *헤드업*합니다. 화면 알림 몇 초간 화면 맨 위에서부터 내려갈 및 다음 화면 맨 위에 있는 알림 음영으로 다시 후퇴 합니다. 화면 알림 시스템 UI 현재 실행 중인 작업을 방해 하지 않고 중요 한 정보를 넣을 수 있도록 합니다. 다음 예제에서는 간단한 헤드업 알림이 앱 위에 표시 됩니다.

[![Heads-up 알림 예](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

화면 알림 다음과 같은 이벤트에 대 한 일반적으로 사용 됩니다.

-   다음 새 메시지

-   들어오는 전화 통화

-   배터리 부족 표시

-   경보

Android 5.0 표시가 높거나 최대 우선 순위 설정 하는 경우에 알림을 헤드업 형태로 표시 됩니다.

Android 5.0에서 Android를 정렬 및 표시를 지능적으로 더 수 있도록 알림 메타 데이터를 제공할 수 있습니다. Android 5.0에는 우선 순위, 표시 유형 및 범주에 따라 알림을 구성합니다.
알림 범주에 되었을 때 알림을 제공할 수 필터링에 사용 되 *방해 하지* 모드입니다.

만들기 및 Android 5.0, 그러면 최신 기능을 사용 하 여 알림을 시작 하는 방법에 대 한 자세한 내용은 참조 [로컬 알림을](~/android/app-fundamentals/notifications/local-notifications.md)합니다.


## <a name="new-apis"></a>새로운 API

위에서 설명한 새로운 모양 및 기능 외에도 Android 5.0 기존 멀티미디어의 기능, 저장소 및 무선/연결 기능을 확장 하는 새로운 Api를 추가 합니다. 또한 Android 5.0 새 작업 스케줄러 기능에 대 한 지원을 제공 하는 새 Api가 포함 됩니다.

### <a name="camera"></a>카메라

Android 5.0 카메라 향상 된 기능에 대 한 몇 가지 새로운 Api를 제공합니다. 새 `Android.Hardware.Camera2` 네임 스페이스는 Android 장치에 연결 된 개별 카메라 장치 액세스에 대 한 기능을 포함 합니다. 또한 `Android.Hardware.Camera2` 파이프라인으로 각 카메라 장치 모델: 캡처 요청을 수락, 이미지를 캡처 및 결과 출력 합니다. 이 방식으로 만드는 앱 카메라 장치에 대 한 여러 캡처 요청을 큐에 대기 합니다.

다음 Api 가능한 새로운 기능을 확인합니다.

-   `CameraManager.GetCameraIdList` &ndash; 프로그래밍 방식으로 카메라 장치;에 액세스할 수 있습니다. 사용 하면 `CameraManager.OpenCamera` 특정 카메라 장치에 연결 합니다.

-   `CameraCaptureSession` &ndash; 캡처하거나 카메라 장치에서 이미지를 스트리밍합니다. 구현 하는 `CameraCaptureSession.CaptureListener` 새 이미지 캡처 이벤트를 처리 하는 인터페이스입니다.

-   `CaptureRequest` &ndash; 캡처 매개 변수를 정의합니다.

-   `CaptureResult` &ndash; 이미지 캡처 작업의 결과 제공합니다.

새 카메라 Android 5.0에서 Api에 대 한 자세한 내용은 [미디어](http://developer.android.com/about/versions/android-5.0.html#Media)합니다.

### <a name="audio-playback"></a>오디오 재생

Android 5.0 업데이트는 `AudioTrack` 더 나은 오디오 재생에 대 한 클래스:

-   `ENCODING_PCM_FLOAT` &ndash; 구성 `AudioTrack` 오디오 데이터를 더 나은 동적 범위, 큰 헤드룸 및 더 높은 품질 (덕분에 늘어난된 전체 자릿수)에 대 한 부동 소수점 형식에서 허용 하도록 합니다. 또한, 부동 소수점 형식 오디오 잘릴 수 있습니다.

-   `ByteBuffer` &ndash; 오디오 데이터를 제공 하면 이제 `AudioTrack` 바이트 배열입니다.

-   `WRITE_NON_BLOCKING` &ndash; 이 옵션을 간소화 버퍼링 일부 응용 프로그램에 대 한 다중 스레딩 합니다.

에 대 한 자세한 `AudioTrack` Android 5.0의 개선 사항 참조 [미디어](http://developer.android.com/about/versions/android-5.0.html#Media)합니다.

### <a name="media-playback-control"></a>미디어 재생 컨트롤

Android 5.0에는 새 `Android.Media.MediaController` 클래스는 대체 `RemoteControlClient`합니다. `Android.Media.MediaController` 간단한 전송 제어 Api를 제공 하 고 UI 컨텍스트 외부에서 재생 스레드로부터 안전한 제어를 제공 합니다. 다음과 같은 새로운 Api 전송 제어를 처리합니다.

-   `Android.Media.Session.MediaSession` &ndash; 미디어는 컨트롤러가 여러 개를 처리 하는 세션을 제어 합니다. 호출 하면 `MediaSession.GetSessionToken` ऍ प ् 세션과 상호 작용 하는 토큰을 요청할 수 있습니다.

-   `MediaController.TransportControls` &ndash; 핸들 전송와 같은 명령 **재생**, **중지**, 및 **Skip**합니다.

또한 새 사용할 수 `Android.App.Notification.MediaStyle` 미디어 세션 풍부한 알림 콘텐츠 (예: 압축을 풀어 앨범 표시)에 연결할 클래스를 합니다.

Android 5.0의 새로운 미디어 재생 제어 기능에 대 한 자세한 내용은 [미디어](http://developer.android.com/about/versions/android-5.0.html#Media)합니다.

### <a name="storage"></a>저장소

Android 5.0 응용 프로그램 디렉터리 및 문서 작업을 쉽게 수행할 수 있도록 저장소 액세스 프레임 워크를 업데이트 합니다.

-   디렉터리 하위 트리를 선택 하려면 작성 한 보낼 수 있습니다는 `Android.Intent.Action.OPEN_DOCUMENT_TREE` 의도 합니다. 이 의도 하면 하위 트리 선택을; 지 원하는 모든 공급자 인스턴스를 표시 하도록 시스템 그런 다음 사용자는를 탐색 하 고 디렉터리를 선택 합니다.

-   을 만들고 새 문서 또는 하위 트리 아래에서 아무 곳 이나 디렉터리를 관리 하려면 새로운 사용 `CreateDocument`, `RenameDocument`, 및 `DeleteDocument` 방식의 `DocumentsContract`합니다.

-   모든 공유 스토리지 장치에서 미디어 디렉터리에 대 한 경로 얻으려면 새 호출 `Android.Content.Context.GetExternalMediaDirs` 메서드.

새 저장소 Android 5.0에서 Api에 대 한 자세한 내용은 [저장소](http://developer.android.com/preview/api-overview.html#Storage)합니다.

### <a name="wireless--connectivity"></a>무선 및 연결

Android 5.0 무선 및 연결을 위한 다음과 같은 API 기능을 추가합니다.

-   새 *다중 네트워크* 를 찾아 연결 하기 전에 특정 기능을 사용 하는 네트워크를 선택 하는 앱에 대 한 수 있게 하는 Api입니다.

-   낮은 에너지 Bluetooth 주변 장치 역할을 하는 Android 5.0 장치 수 있는 Bluetooth 브로드캐스팅의 기능입니다.

-   NFC 향상 된 기능을 쉽게 해 주는 다른 장치와 데이터를 공유 하는 데 근거리 통신 기능을 사용 합니다.

새 무선 및 Android 5.0의 Api 연결 하는 방법에 대 한 자세한 내용은 [무선 및 연결](http://developer.android.com/preview/api-overview.html#Wireless)합니다.

### <a name="job-scheduling"></a>작업 예약

Android 5.0에는 새 `JobScheduler` 사용자를 지원할 수 있는 API 장치가 연결 되어 있는 경우에 실행 하는 특정 작업을 예약 및 충전 하 여 배터리 소모를 최소화 합니다. 요금제 네트워크 대신 Wi-fi 네트워크를 통해 연결 되어 있는 경우 큰 파일을 다운로드 하는 등의 해당 작업에 적합 한 조건이 될 때 실행 되도록 작업을 예약 하는 데이 작업 스케줄러 기능을 사용할 수도 있습니다.

새 작업 Android 5.0의 Api를 예약 하는 방법에 대 한 자세한 내용은 [작업 예약](http://developer.android.com/preview/api-overview.html#JobScheduler)합니다.

## <a name="summary"></a>요약

이 문서 Xamarin.Android 응용 프로그램 개발자에 대 한 Android 5.0의 중요 한 새로운 기능의 개요를 제공합니다.

-   자재 테마

-   애니메이션

-   보기 그림자 및 권한 상승 문제점

-   색 기능을 그릴 수 있는 색조 같은 및 두드러진 색 추출

-   새 `RecyclerView` 및 `CardView` 위젯

-   알림 향상 된 기능

-   카메라, 오디오 재생, 미디어 제어, 저장소, 무선 연결 및 작업 일정을 위한 새 Api

Xamarin Android 개발을 처음 접하는 경우 읽고 [설정 및 설치](~/android/get-started/installation/index.md) Xamarin.Android를 시작할 수 있도록 합니다.
[Hello, Android](~/android/get-started/hello-android/index.md) 는 Android 프로젝트를 만드는 방법을 학습 하기 위한 훌륭한 소개 합니다.



## <a name="related-links"></a>관련 링크

- [Android L 개발자 미리 보기](http://developer.android.com/preview/index.html)
- [Android SDK 가져오기](https://developer.android.com/sdk/index.html#Other)
- [자재 디자인](http://developer.android.com/preview/material/index.html)
- [자재 디자인 원칙](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
