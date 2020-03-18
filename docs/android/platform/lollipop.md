---
title: Lollipop 기능
description: 이 문서에서는 Android 5.0(Lollipop)에 도입된 새로운 기능을 개략적으로 설명합니다. 이러한 기능에는 재질 테마라는 새로운 사용자 인터페이스 스타일뿐만 아니라 애니메이션, 그림자, 드로어블 틴팅 등 새로운 지원 기능도 포함됩니다. Android 5.0에는 향상된 알림 기능, 두 가지 새로운 UI 위젯, 새로운 작업 스케줄러, 그리고 스토리지, 네트워킹, 연결 및 멀티미디어 기능을 개선하기 위한 새로운 API가 다수 포함되어 있습니다.
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 297c7806ce8a880d65c38ef0e4672e41fee5acfe
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "76724444"
---
# <a name="lollipop-features"></a>Lollipop 기능

_이 문서에서는 Android 5.0(Lollipop)에 도입된 새로운 기능을 개략적으로 설명합니다. 이러한 기능에는 재질 테마라는 새로운 사용자 인터페이스 스타일뿐만 아니라 애니메이션, 그림자, 드로어블 틴팅 등 새로운 지원 기능도 포함됩니다. Android 5.0에는 향상된 알림 기능, 두 가지 새로운 UI 위젯, 새로운 작업 스케줄러, 그리고 스토리지, 네트워킹, 연결 및 멀티미디어 기능을 개선하기 위한 새로운 API가 다수 포함되어 있습니다._

## <a name="lollipop-overview"></a>Lollipop 개요

Android 5.0(Lollipop)에서는 새로운 디자인 언어인 *재질 디자인*을 도입하고, 이와 함께 앱을 더 쉽고 직관적으로 사용할 수 있도록 새로운 기능을 지원합니다. 재질 디자인을 통해 Android 5.0는 Android 휴대폰을 페이스리프트할 뿐 아니라 Android 기반 태블릿, 데스크톱 컴퓨터, 워치 및 스마트 TV에 대한 새로운 디자인 규칙 세트도 제공합니다. 이러한 디자인 규칙은 사용자가 인터페이스를 빠르고 직관적으로 이해할 수 있도록 친숙한 촉감 특성(예: 사실적인 표면 및 가장자리 표현)을 활용하는 동시에 단순성과 미니멀리즘을 강조합니다.

*재질 테마*는 Android에서 이러한 UI 디자인 원칙이 구현된 것입니다. 이 문서에서는 재질 테마의 지원 기능부터 살펴봅니다.

- **애니메이션** &ndash; *터치 피드백* 애니메이션, *활동 전환* 애니메이션, *보기 상태 전환* 애니메이션 및 *표시 효과*가 있습니다.

- **보기 그림자 및 높이(elevation)** &ndash; 이제 보기에 `elevation` 속성이 있습니다. 보기는 `elevation` 값이 높을수록 배경에 더 큰 그림자를 드리웁니다.

- **색 기능** &ndash; *드로어블 틴팅*을 사용하면 이미지 자산을 색을 바꿔 다시 사용할 수 있고, *테마 색 추출*을 사용하면 이미지의 색을 기반으로 동적으로 앱에 테마를 적용할 수 있습니다.

많은 재질 테마 기능이 Android 5.0 UI 환경에 이미 내장되어 있지만 앱에 명시적으로 추가해야 하는 경우도 있습니다. 예를 들어 일부 표준 보기(예: 단추)에는 터치 피드백 애니메이션이 이미 포함되어 있지만 앱에서 대부분 보기 그림자를 사용하도록 설정해야 합니다.

Android 5.0에는 재질 테마를 통해 제공되는 UI 개선 사항 외에도 이 문서에서 설명하는 여러 가지 새로운 기능이 포함되어 있습니다.

- **향상된 알림 기능** &ndash; Android 5.0의 알림은 새로운 디자인, 잠금 화면 알림 지원 및 새로운 *헤드업* 알림 프레젠테이션 형식으로 크게 업데이트되었습니다.

- **새로운 UI 위젯** &ndash; 새로운 `RecyclerView` 위젯을 사용하면 앱이 대량 데이터 세트와 복잡한 정보를 보다 쉽게 전달할 수 있으며, 새로운 `CardView` 위젯은 텍스트 및 이미지를 표시하기 위한 간단한 카드 모양의 프레젠테이션 형식을 제공합니다.

- **새로운 API** &ndash; Android 5.0에는 다중 네트워크를 지원하고, Bluetooth 연결을 개선하고, 스토리지 관리를 간소화하고, 멀티미디어 플레이어 및 카메라 디바이스를 보다 유연하게 제어할 수 있는 새로운 API가 추가되었습니다. 새로운 작업 예약 기능을 사용하여 예약된 시간에 작업을 비동기적으로 실행할 수 있습니다. 이 기능을 사용하면 디바이스가 연결되고 충전될 때 작업을 예약하는 등의 방법으로 배터리 사용 시간을 향상할 수 있습니다.

## <a name="requirements"></a>요구 사항

Xamarin 기반 앱에서 Android 5.0 기능을 사용하려면 다음이 필요합니다.

- **Xamarin.Android** &ndash; Visual Studio 또는 Mac용 Visual Studio에서 Xamarin.Android 4.20 이상을 설치하고 구성해야 합니다.

- **Android SDK** &ndash; Android SDK 관리자를 통해 Android SDK 5.0(API 21) 이상을 설치해야 합니다.

- **Java Developer Kit** &ndash; Xamarin.Android는 API 레벨 24 이상에서 개발하는 경우 [JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 이상이 필요합니다(JDK 1.8은 Lollipop을 포함하여 24 이하의 API 레벨도 지원). 사용자 지정 컨트롤 또는 Forms Previewer를 사용하는 경우 64비트 버전의 JDK 1.8이 필요합니다.

API 레벨 23 및 이하를 대상으로 개발하는 경우 [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)을 계속 사용할 수 있습니다.

## <a name="setting-up-an-android-50-project"></a>Android 5.0 프로젝트 설정

Android 5.0 프로젝트를 만들려면 최신 도구 및 SDK 패키지를 설치해야 합니다. Android 5.0을 대상으로 하는 Xamarin.Android 프로젝트를 설정하려면 다음 단계를 수행합니다.

1. Xamarin.Android 도구를 설치하고 Xamarin 라이선스를 활성화합니다. Xamarin.Android를 설치하는 방법에 대한 자세한 내용은 [설정 및 설치](~/android/get-started/installation/index.md)를 참조하세요.

2. Mac용 Visual Studio를 사용하는 경우 최신 Android 5.0 업데이트를 설치합니다.

3. Android SDK 관리자를 시작하고(Mac용 Visual Studio에서는 **도구 &gt; Android SDK 관리자 열기&hellip;** 사용) Android SDK Tools 23.0.5 이상을 설치합니다.

    [![Android SDK 관리자에서 Android SDK Tools 선택](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   또한 최신 Android 5.0 SDK 패키지(API 21 이상)를 설치합니다.

    [![Android SDK 관리자에서 Android 5.0 SDK 패키지 설치](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   Android SDK 관리자를 사용하는 방법에 대한 자세한 내용은 [SDK 관리자](https://developer.android.com/tools/help/sdk-manager.html)를 참조하세요.

4. 새 Xamarin.Android 프로젝트를 만듭니다. Xamarin을 사용한 Android 개발이 처음이라면 [Hello, Android](~/android/get-started/hello-android/index.md)를 참조하여 Android 프로젝트를 만드는 방법에 대해 알아보세요. Android 프로젝트를 만들 때 Android 5.0에 대한 버전 설정을 구성해야 합니다.
   Mac용 Visual Studio에서 **프로젝트 옵션 &gt; 빌드 &gt; 일반l**으로 이동하여 **대상 네트워크**를 **Android 5.0(Lollipop)** 이상으로 설정합니다.

    ![대상 프레임워크를 Android 5.0 Lollipop으로 설정](lollipop-images/target-framework.png)

   **프로젝트 옵션 &gt; 빌드 &gt; Android 애플리케이션**에서 최소 및 대상 Android 버전을 **자동 - 대상 프레임워크 버전 사용**으로 설정합니다.

    ![최소 및 대상 Android 버전을 자동으로 설정](lollipop-images/minimum-android-version.png)

5. 앱을 테스트하기 위해 에뮬레이터 또는 Android 디바이스를 구성합니다. 에뮬레이터를 사용하는 경우 Xamarin Studio 또는 Visual Studio에서 사용할 Android Emulator를 구성하는 방법을 알아보려면 [Android Emulator 설정](~/android/get-started/installation/android-emulator/index.md)을 참조하세요. Android 디바이스를 사용하는 경우 디바이스를 Android 5.0용으로 업데이트하는 방법을 알아보려면 [미리 보기 SDK 설정](https://developer.android.com/preview/setup-sdk.html)을 참조하세요. Xamarin.Android 애플리케이션을 실행하고 디버그하기 위해 Android 디바이스를 구성하려면 [개발용 디바이스 설정](~/android/get-started/installation/set-up-device-for-development.md)을 참조하세요.

참고: Android L 미리 보기를 대상으로 하는 기존 Android 프로젝트를 업데이트하는 경우 **대상 프레임워크** 및 **Android 버전**을 위에 설명된 값으로 업데이트해야 합니다.

## <a name="important-changes"></a>중요한 변경 내용

이전에 게시된 Android 앱은 Android 5.0 변경 사항의 영향을 받을 수 있습니다. 특히 Android 5.0은 새로운 런타임과 상당히 변경된 알림 형식을 사용합니다.

### <a name="android-runtime"></a>Android 런타임

Android 5.0에서는 기본 런타임으로 Dalvik 대신 새로운 Android Runtime(ART)을 사용합니다. ART는 다음과 같은 몇 가지 주요 새로운 기능을 구현합니다.

- **AOT(Ahead-Of-Time) 컴파일** &ndash; AOT는 앱이 처음 시작되기 전에 앱 코드를 컴파일하여 앱 성능을 향상시킬 수 있습니다. 앱이 설치되면 ART가 대상 디바이스에 대한 컴파일된 앱 실행 파일을 생성합니다.

- **향상된 GC(가비지 수집)** &ndash; ART의 향상된 GC 기능은 앱 성능도 향상시킬 수 있습니다. 가비지 수집은 이제 GC 일시 중지를 두 개가 아니라 한 개를 사용하며 동시 GC 작업이 보다 시기 적절하게 완료됩니다.

- **향상된 앱 디버깅** &ndash; ART는 예외 및 충돌 보고서를 분석하는 데 도움이 되는 진단 세부 정보를 제공합니다.

이전 Dalvik 런타임에 고유한 기술을 활용하는 앱을 제외하면(ART에서 작동하지 않을 수 있음) 기존 앱은 변경 없이 ART에서 작동합니다. 이러한 변경 내용에 대한 자세한 내용은 [Android Runtime(ART)에서 앱 동작 확인](https://developer.android.com/guide/practices/verifying-apps-art.html)을 참조하세요.

### <a name="notification-changes"></a>알림 변경 사항

Android 5.0에서는 알림 기능이 크게 변경되었습니다.

- **소리 및 진동의 처리 방법 변경** &ndash; 이제 알림 소리 및 진동은 `Ringtone`, `MediaPlayer` 및 `Vibrator` 대신 `Notification.Builder`가 처리합니다.

- **새로운 색 구성표** &ndash; 재질 테마에 따라 알림이 흰색 또는 매우 밝은 배경의 어두운 텍스트로 렌더링됩니다. 또한 알림 아이콘의 알파 채널은 Android가 시스템 색 구성표와 어울리도록 수정할 수 있습니다.

- **잠금 화면 알림** &ndash; 이제 알림이 디바이스 잠금 화면에 표시될 수 있습니다.

- **헤드업** &ndash; 이제 디바이스가 잠금 해제되고 화면이 켜져 있으면 우선 순위가 높은 알림이 작은 부동 창(헤드업 알림)에 표시됩니다.

대부분의 경우 기존 앱 알림 기능을 Android 5.0으로 이식하려면 다음 단계가 필요합니다.

1. 알림을 만들기 위해 `Notification.Builder` 또는 `NotificationsCompat.Builder`를 사용하도록 코드를 변환합니다.

2. 기존 알림 자산이 새로운 재질 테마 색 구성표에 표시되는지 확인합니다.

3. 알림이 잠금 화면에 표시될 때 알림 표시 유형을 결정합니다. 공개 알림이 아닌 경우 잠금 화면에 표시되는 내용을 결정합니다.

4. 새로운 Android 5.0 *방해 금지* 모드에서 올바르게 처리되도록 알림의 범주를 설정합니다.

알림이 전송 컨트롤을 제공하거나, 미디어 재생 상태를 표시하거나, `RemoteControlClient`를 사용하거나, `ActivityManager.GetRecentTasks`를 호출하는 경우 Android 5.0에 대한 알림을 업데이트하는 방법에 대한 자세한 내용은 [중요한 동작 변경 사항](https://developer.android.com/preview/api-overview.html#Behaviors)을 참조하세요.

Android에서 알림을 만드는 방법에 대한 자세한 내용은 [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)을 참조하세요.

## <a name="material-theme"></a>재질 테마

새로운 Android 5.0 재질 테마는 Android UI의 모양과 느낌을 변경합니다. 시각적 요소는 이제 인쇄 기반 디자인의 굵은 그래픽, 입력 체계 및 밝은 색이 특징인 촉감 표면을 사용합니다. 재질 테마의 예는 다음 스크린샷에 나와 있습니다.

[![재질 테마 홈 화면, 앱 화면 및 설정 화면 스크린샷](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0은 왼쪽에 표시되는 홈 화면으로 인사합니다. 가운데 스크린샷은 앱 목록의 첫 번째 화면이며 오른쪽의 스크린샷은 **설정** 화면입니다. Google의 [재질 디자인](https://material.io/guidelines/material-design/introduction.html) 사양은 새로운 재질 테마 개념의 기본 디자인 규칙을 설명합니다.

재질 테마에는 앱에서 사용할 수 있는 세 가지 기본 제공 테마인 `Theme.Material` 어두운 테마(기본값), `Theme.Material.Light` 테마 및 `Theme.Material.Light.DarkActionBar` 테마가 포함되어 있습니다.

[![어두운 테마, 밝은 테마 및 DarkActionBar 테마 스크린샷](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

Xamarin.Android 앱에서 재질 테마 기능을 사용하는 방법에 대한 자세한 내용은 [재질 테마](~/android/user-interface/material-theme.md)를 참조하세요.

## <a name="animations"></a>애니메이션

Android 5.0은 터치 피드백 애니메이션, 활동 전환 애니메이션, 보기 상태 전환 애니메이션을 제공하여 앱 인터페이스를 보다 직관적으로 사용할 수 있습니다. 또한 Android 5.0 앱은 *표시 효과* 애니메이션을 사용하여 보기를 숨기거나 표시할 수 있습니다. *곡선 동작* 설정을 사용하여 애니메이션이 빠르게 또는 느리게 렌더링되도록 구성할 수 있습니다.

### <a name="touch-feedback-animations"></a>터치 피드백 애니메이션

터치 피드백 애니메이션은 보기를 터치할 때 사용자에게 시각적 피드백을 제공합니다. 예를 들어 단추를 터치하면 이제 잔물결 효과가 표시됩니다. 이 효과는 Android 5.0의 기본 터치 피드백 애니메이션입니다. 잔물결 애니메이션은 새로운 `RippleDrawable` 클래스에 의해 구현됩니다. 잔물결 효과를 보기의 경계에서 종료하거나 보기 경계를 넘어 확장하도록 구성할 수 있습니다. 예를 들어 다음 스크린샷 시퀀스는 터치 애니메이션 도중 단추의 잔물결 효과를 보여 줍니다.

![단추에 대한 잔물결 애니메이션의 프레임별 스크린샷](lollipop-images/touch-animation.png)

왼쪽의 첫 번째 이미지는 단추를 처음 터치한 상태이고, 나머지 시퀀스에서는(왼쪽에서 오른쪽으로) 잔물결 효과가 어떻게 단추 가장자리까지 확산되는지 보여 줍니다. 잔물결 애니메이션이 종료되면 보기가 원래 모양으로 돌아갑니다. 기본 잔물결 애니메이션은 몇 분의 1초 동안 발생하지만 애니메이션 길이는 더 길거나 더 짧게 사용자 지정할 수 있습니다.

Android 5.0의 터치 피드백 애니메이션에 대한 자세한 내용은 [터치 피드백 사용자 지정](https://developer.android.com/training/material/animations.html#Touch)을 참조하세요.

### <a name="activity-transition-animations"></a>활동 전환 애니메이션

활동 전환 애니메이션은 한 활동에서 다른 활동으로 전환될 때 사용자에게 시각적 연속감을 제공합니다. 앱은 다음 세 유형의 전환 애니메이션을 지정할 수 있습니다.

- **들어가기 전환** &ndash; 활동이 장면으로 들어갈 때의 애니메이션입니다.

- **나가기 전환** &ndash; 활동이 장면에서 나갈 때의 애니메이션입니다.

- **공유 요소 전환** &ndash; 두 활동에 공통된 보기가 첫 번째 활동에서 두 번째 활동으로 전환되면서 바뀔 때의 애니메이션입니다.

예를 들어 다음 스크린샷 시퀀스는 공유 요소 전환을 보여 줍니다.

[![공유 요소 전환 애니메이션의 프레임별 스크린샷](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

공유 요소(애벌레 사진)는 첫 번째 활동의 여러 보기 중 하나입니다. 이 공유 요소는 첫 번째 활동이 두 번째 활동으로 전환될 때 두 번째 활동에서 유일한 보기가 되도록 확대됩니다.

#### <a name="enter-transition-animation-types"></a>들어가기 전환 애니메이션 유형

들어가기 전환의 경우 Android 5.0은 세 가지 유형의 애니메이션을 제공합니다.

- **사방 확대 애니메이션** &ndash; 장면의 가운데서 보기를 확대합니다.

- **슬라이드 애니메이션** &ndash; 보기가 장면의 한 모서리에서 안쪽으로 들어옵니다.

- **페이드 애니메이션** &ndash; 보기를 장면으로 페이드 인합니다.

#### <a name="exit-transition-animation-types"></a>나가기 전환 애니메이션 유형

나가기 전환의 경우 Android 5.0은 세 가지 유형의 애니메이션을 제공합니다.

- **사방 확대 애니메이션** &ndash; 보기를 장면의 가운데로 축소합니다.

- **슬라이드 애니메이션** &ndash; 보기가 장면의 한 모서리로 이동하여 사라집니다.

- **페이드 애니메이션** &ndash; 보기를 장면에서 페이드 아웃합니다.

#### <a name="shared-element-transition-animation-types"></a>공유 요소 전환 애니메이션 유형

공유 요소 전환은 다음과 같은 여러 유형의 애니메이션을 지원합니다.

- 보기의 레이아웃 또는 잘림 경계를 변경

- 보기의 배율 및 회전을 변경

- 보기의 크기 및 배율 유형을 변경

Android 5.0의 활동 전환 애니메이션에 대한 자세한 내용은 [활동 전환 사용자 지정](https://developer.android.com/training/material/animations.html#Transitions)을 참조하세요.

### <a name="view-state-transition-animations"></a>보기 상태 전환 애니메이션

Android 5.0을 사용하면 보기 상태가 변경될 때 애니메이션을 실행할 수 있습니다. 다음 방법 중 하나를 사용하여 보기 상태 전환에 애니메이션 효과를 적용할 수 있습니다.

- 특정 보기와 연결된 상태 변화를 애니메이션화하는 드로어블을 만듭니다. 새로운 `AnimatedStateListDrawable` 클래스를 사용하면 보기 상태가 바뀌는 사이에 애니메이션을 표시하는 드로어블을 만들 수 있습니다.

- 보기 상태가 바뀔 때 실행되는 애니메이션 기능을 정의합니다. 새로운 `StateListAnimator` 클래스를 사용하면 보기 상태가 바뀔 때 실행되는 애니메이터를 정의할 수 있습니다.

Android 5.0의 보기 상태 전환 애니메이션에 대한 자세한 내용은 [보기 상태 변화 애니메이션화](https://developer.android.com/training/material/animations.html#ViewState)를 참조하세요.

### <a name="reveal-effect"></a>표시 효과

*표시 효과*는 반지름의 크기가 바뀌면서 보기를 표시하거나 숨기는 잘린 원형입니다. 잘린 원형의 초기 또는 최종 반원을 설정하여 이 효과를 제어할 수 있습니다. 다음 스크린샷 시퀀스는 화면 가운데서 발생하는 표시 효과 애니메이션을 보여 줍니다.

[![표시 효과 애니메이션의 프레임별 스크린샷](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

다음 시퀀스는 화면의 왼쪽 아래에서 발생하는 표시 효과 애니메이션을 보여 줍니다.

[![잘린 원형 애니메이션의 프레임별 스크린샷](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

표시 효과 애니메이션은 역방향으로 실행할 수 있습니다. 즉, 잘린 원형이 확대되면서 보기를 표시하는 것이 아니라 축소되면서 보기를 숨길 수 있습니다.

Android 5.0 표시 효과에 대한 자세한 내용은 [표시 효과 사용](https://developer.android.com/training/material/animations.html#Reveal)을 참조하세요.

### <a name="curved-motion"></a>곡선 동작

이러한 애니메이션 기능 외에도 Android 5.0은 애니메이션의 시간 및 동작 곡선을 지정할 수 있는 새로운 API를 제공합니다. Android 5.0은 이러한 곡선을 사용하여 애니메이션 도중 시간 및 공간 이동을 보간합니다. Android 5.0에는 세 가지 곡선이 정의되어 있습니다.

- **Fast\_out\_linear\_in** &ndash; 빠르게 가속하고 애니메이션이 끝날 때까지 계속 가속합니다.

- **Fast\_out\_slow\_in** &ndash; 빠르게 가속하고 애니메이션 끝으로 갈수록 서서히 감속합니다.

- **Linear\_out\_slow\_in** &ndash; 피크 속도로 시작하여 애니메이션 끝으로 갈수록 서서히 감속합니다.

새로운 `PathInterpolator` 클래스를 사용하여 동작 보간 방식을 지정할 수 있습니다. `PathInterpolator`는 지정된 컨트롤 지점 및 동작 곡선에 따라 애니메이션 경로를 트래버스하는 보간기입니다. Android 5.0에서 곡선 동작 설정을 지정하는 방법에 대한 자세한 내용은 [곡선 동작 사용](https://developer.android.com/training/material/animations.html#CurvedMotion)을 참조하세요.

## <a name="view-shadows--elevation"></a>보기 그림자 및 높이(elevation)

Android 5.0에서는 새로운 `Z` 속성을 설정하여 보기의 *높이*를 지정할 수 있습니다. `Z` 값이 클수록 보기가 배경에 더 큰 그림자를 드리워 보기가 배경 위로 더 높이 떠 있는 것으로 보입니다. 레이아웃에서 `elevation` 특성을 구성하여 보기의 초기 높이를 설정할 수 있습니다.

다음 예제에서는 높이 특성이 2dp, 4dp, 6dp로 각각 설정된 경우 빈 `TextView` 컨트롤에 드리워지는 그림자를 보여 줍니다.

[![점차 커지는 보기 그림자의 스크린샷](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

보기 그림자 설정은 위에 표시된 것과 같이 정적일 수도 있고 애니메이션에서 사용하여 보기가 배경 위에 일시적으로 표시되도록 할 수도 있습니다. `ViewPropertyAnimator` 클래스를 사용하여 보기의 높이에 애니메이션 효과를 적용할 수 있습니다. 보기의 높이는 레이아웃 `elevation` 설정과 `ViewPropertyAnimator` 메서드 호출을 통해 설정할 수 있는 `translationZ` 속성을 더한 값입니다.

Android 5.0의 보기 그림자에 대한 자세한 내용은 [그림자 및 잘린 보기 정의](https://developer.android.com/training/material/shadows-clipping.html)를 참조하세요.

## <a name="color-features"></a>색 기능

Android 5.0은 앱에서 색을 관리하기 위한 두 가지 새로운 기능을 제공합니다.

- *드로어블 틴팅*을 사용하면 레이아웃 특성을 변경하여 이미지 자산의 색을 바꿀 수 있습니다.

- *테마 색 추출*을 사용하면 표시된 이미지의 색상표와 어울리도록 앱 색 테마를 동적으로 사용자 지정할 수 있습니다.

### <a name="drawable-tinting"></a>드로어블 틴팅

Android 5.0 레이아웃은 다양한 색을 표시하기 위해 여러 버전의 자산을 만들지 않고도 드로어블의 색을 설정하는 데 사용할 수 있는 새로운 `tint` 특성을 인식합니다. 이 기능을 사용하려면 비트맵을 알파 마스크로 정의하고 `tint` 특성을 사용하여 자산의 색을 정의합니다. 이렇게 하면 자산을 한 번 만들고 레이아웃에서 테마에 맞게 색을 지정할 수 있습니다.

다음 예제에서는 단일 이미지 자산(투명한 배경의 흰색 로고)을 사용하여 색조 변형을 만듭니다.

![투명한 배경의 흰색 Xamarin 로고](lollipop-images/xamarin-logo-white.png)

이 로고는 다음 예와 같이 파란색 원형 배경 위에 표시됩니다. 왼쪽 이미지는 `tint` 설정 없이 로고가 표시되는 방법입니다. 가운데 이미지에서는 로고의 `tint` 특성이 진한 회색으로 설정됩니다. 오른쪽 이미지에서는 `tint` 특성이 연한 회색으로 설정됩니다.

![위 로고의 다른 색조 설정 예](lollipop-images/drawable-tinting.png)

Android 5.0의 드로어블 틴팅에 대한 자세한 내용은 [드로어블 틴팅](https://developer.android.com/training/material/drawables.html#DrawableTint)을 참조하세요.

### <a name="prominent-color-extraction"></a>테마 색 추출

새로운 Android 5.0 `Palette` 클래스를 사용하면 이미지에서 색을 추출하여 사용자 지정 색상표에 동적으로 적용할 수 있습니다. `Palette` 클래스는 이미지에서 6개 색을 추출하고 색 채도 및 밝기의 상대적 수준에 따라 이러한 색에 레이블을 지정합니다.

- 생생함

- 생생하게 진함

- 생생하게 연함

- 침침함

- 침침하게 진함

- 침침하게 연함

예를 들어 다음 스크린샷에서 사진 보기 앱은 표시되는 이미지에서 중요한 색을 추출하고 이러한 색을 사용하여 이미지와 일치하도록 앱의 색 구성표를 조정합니다.

[![녹색, 분홍색 및 파란색 테마 색 추출의 스크린샷](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

위의 스크린샷에서 작업 모음은 추출된 "생생하게 연한" 색으로 설정되고 배경은 추출된 "생생하게 진한" 색으로 설정됩니다. 위의 각 예에서는 이미지에서 추출된 색상표 색을 보여 주기 위해 작은 색 사각형의 행이 포함되어 있습니다.

Android 5.0에서 색 추출에 대한 자세한 내용은 [이미지에서 테마 색 추출](https://developer.android.com/training/material/drawables.html#ColorExtract)을 참조하세요.

## <a name="new-ui-widgets"></a>새로운 UI 위젯

Android 5.0에는 두 가지 새로운 UI 위젯이 도입되었습니다.

- `RecyclerView` &ndash; 스크롤 가능한 항목의 목록을 표시하는 보기 그룹입니다.

- `CardView` &ndash; 모서리가 둥근 기본 레이아웃입니다.

두 위젯은 모두 재질 테마 기능을 기본적으로 지원합니다. 예를 들어 `RecyclerView`는 보기를 추가 및 제거하는 데 애니메이션을 사용하고, `CardView`는 각 카드가 배경 위에 부동 상태로 보이도록 보기 그림자를 사용합니다. 이러한 새로운 위젯의 예는 다음 스크린샷에 나와 있습니다.

[![RecyclerView로 빌드된 앱의 스크린샷](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

왼쪽의 스크린샷에는 이메일 앱에서 사용되는 `RecyclerView`의 예입니다. 오른쪽의 스크린샷은 여행 예약 앱에서 사용되는 `CardView`의 예입니다.

### <a name="recyclerview"></a>RecyclerView

`RecyclerView`는 `ListView,`와 비슷하지만 동적으로 바뀌는 요소가 포함된 대규모 보기 또는 목록 집합에 보다 적합합니다. `ListView,`와 마찬가지로 어댑터를 지정하여 기본 데이터 세트에 액세스합니다. 그러나 `ListView,`와 달리 *레이아웃 관리자*를 사용하여 `RecyclerView` 안에 항목을 배치할 수 있습니다. 또한 레이아웃 관리자는 보기 재활용을 관리합니다. 즉, 사용자에게 더 이상 표시되지 않는 항목 보기의 재사용을 관리합니다.

`RecyclerView` 위젯을 사용하는 경우 `LayoutManager`와 어댑터를 지정해야 합니다. 이 그림에 표시된 것처럼 `LayoutManager`는 어댑터와 `RecyclerView` 간의 중개자입니다.

![RecyclerView 및 지원하는 LayoutManager, 어댑터 및 데이터 세트의 다이어그램](lollipop-images/recyclerview-diagram.png)

다음 스크린샷에서는 100개 항목을 포함하는 `RecyclerView`를 보여 줍니다(각 항목은 `ImageView` 및 `TextView`로 구성됨).

[![RecyclerView 앱이 이미지를 스크롤하는 스크린샷](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView`는 이 샘플 앱에서 목록의 처음부터 목록의 끝까지 스크롤하는 데 몇 초 밖에 걸리지 않을 정도로 이 대규모 데이터 세트를 쉽게 처리합니다. `RecyclerView`는 애니메이션도 지원합니다. 실제로 항목 추가 및 제거에 대한 애니메이션은 기본적으로 사용하도록 설정됩니다. 항목이 `RecyclerView`에 추가되면 이 스크린샷 시퀀스와 같이 페이드 인됩니다.

[![사진 항목 페이드 인의 프레임별 스크린샷](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

`RecyclerView`에 대한 자세한 내용은 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)를 참조하세요.

### <a name="cardview"></a>CardView

`CardView`는 모서리가 둥근 부동 카드를 시뮬레이션하는 간단한 보기입니다. `CardView`에는 기본 제공 보기 그림자가 있으므로 앱에 시각적 깊이를 추가하는 쉬운 방법을 제공합니다. 다음 스크린샷에는 `CardView`의 세 가지 텍스트 관련 예제가 나와 있습니다.

[![RecyclerView를 CardView 기반 항목과 함께 사용하는 앱 예제 스크린샷](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

위의 예제에서 각 카드에는 `TextView`가 포함되어 있습니다. 배경색은 `cardBackgroundColor` 특성을 통해 설정됩니다.

`CardView`에 대한 자세한 내용은 [CardView](~/android/user-interface/controls/card-view.md)를 참조하세요.

## <a name="enhanced-notifications"></a>향상된 알림 기능

Android 5.0의 알림 시스템은 새로운 시각적 형식과 새로운 기능을 사용하여 크게 업데이트되었습니다. Android 5.0에서 알림은 모양이 새로워졌습니다. 예를 들어 Android 5.0의 알림은 이제 밝은 배경에 진한 텍스트를 사용합니다.

![확장 취소된 Android 5.0 알림 예](lollipop-images/expanded-notification-contracted.png)

위의 예와 같이 알림에 큰 아이콘이 표시되면 Android 5.0은 작은 아이콘을 큰 아이콘 위에 배지로 표시합니다.

Android 5.0에서는 알림이 디바이스 잠금 화면에 표시될 수도 있습니다.
예를 들어 다음은 단일 알림이 포함된 잠금 화면의 스크린샷입니다.

[![잠금 화면에 표시된 알림 스크린샷](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

사용자는 잠금 화면에서 알림을 두 번 탭하여 디바이스의 잠금을 해제하고 해당 알림을 시작한 앱으로 이동하거나 살짝 밀어 알림을 해제할 수 있습니다. 알림에는 잠금 화면에 표시할 수 있는 콘텐츠 양을 결정하는 새로운 *표시 유형* 설정이 있습니다. 사용자는 중요한 콘텐츠가 잠금 화면 알림에 표시되도록 허용할지 여부를 선택할 수 있습니다.

Android 5.0에는 *헤드업*이라는 새로운 우선 순위가 높은 알림 표시 형식이 도입되었습니다. 헤드업 알림은 몇 초 동안 화면 맨 위에서 아래로 이동했다가 다시 화면 맨 위에 있는 알림 음영으로 되돌아갑니다. 헤드업 알림을 통해 시스템 UI는 현재 실행 중인 작업을 중단하지 않고 사용자에게 중요한 정보를 표시할 수 있습니다.
다음 예에서는 앱 위에 표시되는 간단한 헤드업 알림을 보여 줍니다.

[![헤드업 알림 예](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

일반적으로 헤드업 알림은 다음 이벤트에 사용됩니다.

- 새로운 다음 메시지

- 수신 전화

- 배터리 부족 표시

- 경보

Android 5.0은 높은 또는 최대 우선 순위 설정이 있는 경우에만 헤드업 형식으로 알림을 표시합니다.

Android 5.0에서는 Android가 알림을 보다 지능적으로 정렬하고 표시하도록 알림 메타데이터를 제공할 수 있습니다. Android 5.0은 우선 순위, 표시 유형 및 범주에 따라 알림을 구성합니다.
알림 범주는 디바이스가 *방해 금지* 모드일 때 표시할 수 있는 알림을 필터링하는 데 사용됩니다.

최신 Android 5.0 기능을 사용하여 알림을 만들고 시작하는 방법에 대한 자세한 내용은 [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)을 참조하세요.

## <a name="new-apis"></a>새로운 API

Android 5.0은 위에 설명된 새로운 모양과 느낌 기능 외에도 기존 멀티미디어, 스토리지 및 무선/연결 기능을 확장하는 새로운 API를 추가했습니다. 또한 Android 5.0에는 새로운 작업 스케줄러 기능을 지원하는 새로운 API가 포함되어 있습니다.

### <a name="camera"></a>카메라

Android 5.0은 향상된 카메라 기능을 위한 몇 가지 새로운 API를 제공합니다. 새로운 `Android.Hardware.Camera2` 네임스페이스에는 Android 디바이스에 연결된 개별 카메라 디바이스에 액세스하기 위한 기능이 포함되어 있습니다. 또한 `Android.Hardware.Camera2`는 각 카메라 디바이스를 파이프라인으로 모델링합니다. 즉, 캡처 요청을 수락하고 이미지를 캡처한 다음 결과를 출력합니다. 이 접근 방식을 사용하면 앱이 카메라 디바이스에 대한 여러 캡처 요청을 큐에 대기시킬 수 있습니다.

다음 API는 이러한 새로운 기능을 구현합니다.

- `CameraManager.GetCameraIdList` &ndash; 카메라 디바이스에 프로그래밍 방식으로 액세스할 수 있습니다. `CameraManager.OpenCamera`를 사용하여 특정 카메라 디바이스에 연결합니다.

- `CameraCaptureSession` &ndash; 카메라 디바이스에서 이미지를 캡처하거나 스트리밍합니다. 새 이미지 캡처 이벤트를 처리하는 `CameraCaptureSession.CaptureListener` 인터페이스를 구현합니다.

- `CaptureRequest` &ndash; 캡처 매개 변수를 정의합니다.

- `CaptureResult` &ndash; 이미지 캡처 작업의 결과를 제공합니다.

Android 5.0의 새로운 카메라 API에 대한 자세한 내용은 [미디어](https://developer.android.com/about/versions/android-5.0.html#Media)를 참조하세요.

### <a name="audio-playback"></a>오디오 재생

Android 5.0은 향상된 오디오 재생을 위해 `AudioTrack` 클래스를 업데이트했습니다.

- `ENCODING_PCM_FLOAT` &ndash; 다이내믹 레인지, 헤드룸 및 음질을 향상하기 위해(전체 자릿수 증가 덕분) 부동 소수점 형식의 오디오 데이터를 허용하도록 `AudioTrack`을 구성합니다. 또한 부동 소수점 형식은 오디오 클리핑을 방지하는 데 도움이 됩니다.

- `ByteBuffer` &ndash; 이제 `AudioTrack`에 오디오 데이터를 바이트 배열로 제공할 수 있습니다.

- `WRITE_NON_BLOCKING` &ndash; 이 옵션은 일부 앱에 대해 버퍼링 및 다중 스레딩을 간소화합니다.

Android 5.0의 향상된 `AudioTrack` 기능에 대한 자세한 내용은 [미디어](https://developer.android.com/about/versions/android-5.0.html#Media)를 참조하세요.

### <a name="media-playback-control"></a>미디어 재생 컨트롤

Android 5.0에는 `RemoteControlClient`를 대체하는 새로운 `Android.Media.MediaController` 클래스가 도입되었습니다. `Android.Media.MediaController` 간소화된 전송 컨트롤 API를 제공하며 UI 컨텍스트 외부에서 스레드로부터 안전한 재생 컨트롤을 제공합니다. 새로운 전송 컨트롤을 처리하는 새 API는 다음과 같습니다.

- `Android.Media.Session.MediaSession` &ndash; 여러 컨트롤러를 처리하는 미디어 컨트롤 세션입니다. `MediaSession.GetSessionToken`을 호출하여 앱이 세션과 상호 작용하는 데 사용하는 토큰을 요청합니다.

- `MediaController.TransportControls` &ndash; **재생**, **정지**, **건너뛰기** 등의 전송 명령을 처리합니다.

또한 새로운 `Android.App.Notification.MediaStyle` 클래스를 사용하여 미디어 세션을 풍부한 알림 콘텐츠(예: 앨범 아트 추출 및 표시)와 연결할 수 있습니다.

Android 5.0의 새로운 미디어 재생 컨트롤 기능에 대한 자세한 내용은 [미디어](https://developer.android.com/about/versions/android-5.0.html#Media)를 참조하세요.

### <a name="storage"></a>스토리지

Android 5.0에서는 애플리케이션이 디렉터리와 문서를 쉽게 사용할 수 있도록 스토리지 액세스 프레임워크가 업데이트되었습니다.

- 디렉터리 하위 트리를 선택하기 위해 `Android.Intent.Action.OPEN_DOCUMENT_TREE` 의도를 빌드하고 보낼 수 있습니다. 이 의도는 시스템에서 하위 트리 선택을 지원하는 모든 공급자 인스턴스를 표시합니다. 그런 다음 사용자가 디렉터리를 찾아서 선택합니다.

- 하위 트리 아래 어디서나 새 문서 또는 디렉터리를 만들고 관리하려면 `DocumentsContract`의 새로운 `CreateDocument`, `RenameDocument` 및 `DeleteDocument` 메서드를 사용합니다.

- 모든 공유 스토리지 디바이스에서 미디어 디렉터리에 대한 경로를 가져오려면 새로운 `Android.Content.Context.GetExternalMediaDirs` 메서드를 호출합니다.

Android 5.0의 새로운 스토리지 API에 대한 자세한 내용은 [스토리지](https://developer.android.com/preview/api-overview.html#Storage)를 참조하세요.

### <a name="wireless--connectivity"></a>무선 및 연결

Android 5.0에는 무선 및 연결에 대한 다음과 같은 향상된 API가 추가되었습니다.

- 새로운 *다중 네트워크* API - 앱이 연결하기 전에 특정 기능을 가진 네트워크를 찾아 선택할 수 있습니다.

- Bluetooth 브로드캐스팅 기능 - Android 5.0 디바이스가 저전력 Bluetooth 주변 장치로 작동할 수 있습니다.

- NFC 기능 향상 - 다른 디바이스와 데이터를 공유하는 근거리 통신 기능을 보다 쉽게 사용할 수 있습니다.

Android 5.0의 새로운 무선 및 연결 API에 대한 자세한 내용은 [무선 및 연결](https://developer.android.com/preview/api-overview.html#Wireless)을 참조하세요.

### <a name="job-scheduling"></a>작업 예약

Android 5.0에는 디바이스를 연결하고 충전하는 경우에만 특정 작업을 실행하도록 예약하여 배터리 소모를 최소화하는 데 도움이 되는 새로운 `JobScheduler` API가 도입되었습니다. 이 작업 스케줄러 기능을 사용하여 디바이스가 유료 네트워크가 아니라 Wi-Fi 네트워크를 통해 연결되었을 때 대용량 파일을 다운로드하는 등 더 적합한 조건에서 작업을 실행하도록 예약할 수도 있습니다.

Android 5.0의 새로운 작업 예약 API에 대한 자세한 내용은 [작업 예약](https://developer.android.com/preview/api-overview.html#JobScheduler)을 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Android 앱 개발자에게 중요한 Android 5.0의 새로운 기능을 개략적으로 설명했습니다.

- 재질 테마

- 애니메이션

- 보기 그림자 및 높이(elevation)

- 드로어블 틴팅, 테마 색 추출과 같은 색 기능

- 새로운 `RecyclerView` 및 `CardView` 위젯

- 알림 기능 향상

- 카메라, 오디오 재생, 미디어 컨트롤, 스토리지, 무선/연결 및 작업 예약을 위한 새로운 API

Xamarin.Android 개발이 처음인 경우에는 Xamarin.Android를 시작하는 데 도움이 되는 [설정 및 설치](~/android/get-started/installation/index.md)를 참조하세요.
[Hello, Android](~/android/get-started/hello-android/index.md)는 Android 프로젝트를 만드는 방법을 배우는 데 유용한 소개입니다.

## <a name="related-links"></a>관련 링크

- [Android L 개발자 미리 보기](https://developer.android.com/preview/index.html)
- [Android SDK 가져오기](https://developer.android.com/sdk/index.html#Other)
- [재질 디자인](https://developer.android.com/preview/material/index.html)
