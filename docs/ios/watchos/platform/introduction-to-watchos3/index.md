---
title: WatchOS 3 소개
description: 이 문서에서는 Xamarin 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 watchOS 3에서에서 사용할 수 있는 기능 소개 합니다.
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/07/2017
ms.openlocfilehash: 506e3795538ceffc77301a608c504fc6ec2045a7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-watchos-3"></a>WatchOS 3 소개

_이 문서에서는 Xamarin 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 watchOS 3에서에서 사용할 수 있는 기능 소개 합니다._

이 문서에서는 다음 항목을 설명 합니다.

- [WatchOS 3의 새로운 이란](#Whats-New-in-watchOS-3)
    - [향상 된 비용을 지불 Apple](#Apple-Pay-Enhancements) 에서 Apple Watch 앱에서 결제에 대 한 지원을 추가 합니다.
    - [백그라운드 작업](#Background-Tasks) 앱 되므로 사용자에서 필요한 경우 백그라운드에서 해당 정보를 업데이트 하는 기능을 지정 합니다.
    - [복잡성 향상 된 기능](#Complications-Enhancements) 앱에 대 한 새로운 기능을 제공 하는 watchOS 3에 대해 지정 되었습니다.
    - [새로 사용 가능한 프레임 워크](#Newly-Available-Frameworks) watchOS 앱에 대 한 노출 했습니다.
    - [사전 제안](#Proactive-Suggestions) 앱 사전 사용자에 게 정보를 표시 하도록 할 수 있습니다.
    * 여러 [보안 및 개인 정보 보호 향상](#Security-and-Privacy-Enhancements) watchOS 3 적용 했습니다.
    - [스냅숏 및 도킹](#Snapshots-and-Dock) 사용자에 게 앱 watchOS 앱에 대 한 빠른 액세스를 제공 합니다.
    - [사용자 알림](#User-Notifications) 로컬 및 원격 알림을 사용자에 게 제공 합니다.
    * 여러 [조사식 연결 프레임 워크 기능 향상](#Watch-Connectivity-Framework-Enhancements) watchOS 3에서에서 지정 되었습니다.
    * 여러 [WatchKit 프레임 워크의 향상 된 기능](#WatchKit-Framework-Enhancements) watchOS 3에서에서 지정 되었습니다.
    - [워크 아웃 응용 프로그램의 향상 된 기능](#Workout-App-Enhancements) 제공 하는 워크 아웃 새로운 능력 관련 Apple Watch 앱.
- [추가 프레임 워크 변경](#Additional-Framework-Changes) watchOS 3 전체에서 지정 되었습니다.
- [사용 되지 않는 Api](#Deprecated-APIs) watchOS 3에에서 있습니다.

<a name="Whats-New-in-watchOS-3" />

## <a name="whats-new-in-watchos-3"></a>WatchOS 3의 새로운 이란

Apple이 몇 가지 새로운 Api 및 서비스를 비롯 한 기존 기능에는 많은 향상 된 구성 요소가 watchOS 3에에서 추가:

<a name="Apple-Pay-Enhancements" />

## <a name="apple-pay-enhancements"></a>Apple Pay 향상 된 기능

3 watchOS PassKit 프레임 워크 Apple Watch 실행 되는 앱에 대 한 실제 상품 및 서비스) (의 안전 하 고, 응용 프로그램에서 결제에 대 한 지원을 수 있도록 확장 되었습니다.

새로운 [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) 및 [PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate) 시키며 사용자 지불 요청 권한을 부여할 수 있는 인터페이스에 응답 하는 클래스입니다.

자세한 내용은 참조 하세요 우리의 [Apple 지불 향상 된 기능](~/ios/watchos/platform/apple-pay.md) 가이드입니다.

<a name="Background-Tasks" />

## <a name="background-tasks"></a>백그라운드 작업

watchOS 3 응용 프로그램 콘텐츠가가지고 있는지 확인 해야 하는 열기 전에 해당 정보를 업데이트 하는 데 사용할 수 있는 몇 가지 백그라운드 작업을 소개 합니다.

다음 새 백그라운드 작업을 사용할 수 있습니다:

- **응용 프로그램 새로 고침 백그라운드** - [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) 태스크 백그라운드에서 해당 상태를 업데이트 하려면 앱을 허용 합니다. 일반적으로 사용 하 여 인터넷에서 새 콘텐츠를 다운로드 하는 등의 다른 작업을이 포함 됩니다는 [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession)합니다.
- **스냅숏 새로 고침 백그라운드** - [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) 태스크 시스템이 도킹 스테이션을 채우는 데 사용 하는 스냅숏을 만들기 전에 해당 콘텐츠 및 UI를 업데이트 하려면 앱을 허용 합니다.
- **조사식 연결 백그라운드** - [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) 쌍을 이루는 iPhone에서 배경 데이터를 받을 때 응용 프로그램에 대 한 작업을 시작 합니다.
- **URL 세션 백그라운드** - [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) 백그라운드 전송 권한 부여가 필요한 되거나 (성공적으로 또는 오류)를 완료 되 면 응용 프로그램에 대 한 작업을 시작 합니다.

자세한 내용은 참조 하세요 우리의 [백그라운드 작업](~/ios/watchos/platform/background-tasks.md) 가이드입니다.

<a name="Complications-Enhancements" />

## <a name="complications-enhancements"></a>복잡성 향상 된 기능

Complication은 작은 시각적 요소를 한 눈에 유용한 정보를 제공 합니다. 선택한 시계 화면의 따라 사용자는 하나 이상의 복잡 함 중 하나를 사용 하 여 조사식 얼굴을 사용자 지정할 수를 있습니다.

watchOS 3을 사용 하는 응용 프로그램 사용자 watch 화면에서 해당 정보를 한 눈에 액세스할 수 있도록 watch 앱에 대 한 하나 이상의 Complication를 만들 수 있습니다.

또한 문제는 다음과 같은 이점을 제공합니다.

- 사용자는 watch 화면에서 직접 complication 탭 하 여 앱을 시작 신속 하 게 수 있습니다.
- 응용 프로그램의 문제 중 하나에 조사식 얼굴 원인을 대 보유 위치 하 게 백그라운드에서 응용 프로그램을 실행 하려고 준비 실행 상태에 앱을 유지 하려면 시스템에 보관 메모리 및 업데이트 하는 추가 시간으로 제공 합니다.
- 복잡성 하루 최소 50 푸시 업데이트 보장 됩니다.
- 추천 Apple Watch 얼굴 갤러리에는 응용 프로그램 복잡성을 포함 하는 경우 (Apple의 참조 [갤러리에 추가 복잡성](https://developer.apple.com/documentation/clockkit/adding_complications_to_the_gallery) 에 대 한 자세한 내용은 설명서).

WatchOS 3에서에서 ClockKit 프레임 워크 이제 초대형 복잡성에 대 한 몇 가지 새 템플릿을 같은 포함 [CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext) 및 [CLKComplicationTemplateExtraLargeRingImage ](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). 또한 지역화 가능한 텍스트를 만들려면 새 메서드를 사용는 [CLKTextProvider](https://developer.apple.com/reference/clockkit/clktextprovider) 클래스입니다.

자세한 내용은 참조 하세요 우리의 [watchOS 3에 대 한 빠른 상호 작용 기술을](~/ios/watchos/platform/quick-interaction-techniques.md) 가이드입니다.

<a name="Newly-Available-Frameworks" />

## <a name="newly-available-frameworks"></a>새로 사용 가능한 프레임 워크

watchOS 3과 같은 이전에 제공 되지 않은 여러 개의 기존 Apple 프레임 워크가 포함 되어 있습니다.

- **SceneKit** -음영, 애니메이션, 조명 물리학와 같은 다른 플랫폼에서 사용 가능한 기능 및 입자 시스템의 대부분을 포함 하 여 조사식 응용 프로그램의 UI를 사용 하 여 SceneKit 3D를 포함 하도록 모델링 합니다. 3D 공간 오디오, 사용자 지정 금속 또는 OpenGL 셰이더, 이미지 필터: 코어 및 물리적으로 기반 자료 지원 되지 않습니다.
- **SpriteKit** -사용 하 여 SpriteKit 렌더링 하 고 대부분의 동작, 물리학, 조명 및 입자 시스템 등의 다른 플랫폼에서 사용할 수 있는 기능을 포함 하 여 앱 watch 앱 UI에 스프라이트 애니메이션을 적용 합니다. 3D 공간 오디오, 비디오 재생 및 Core 이미지 필터: 지원 되지 않습니다.
- **AVFoundation** -관리 하 고 오디오를 재생 합니다.
- **CloudKit** -조사식 응용 프로그램 및 iCloud 컨테이너 간에 데이터를 이동 합니다.
- **오디오 핵심** -오디오 스트림, 복잡 한 버퍼 및 시간 값 표시에 대 한 데이터 형식을 관리 합니다.
- **GameKit** -소셜 게임을 작성 합니다.

<a name="Proactive-Suggestions" />

## <a name="proactive-suggestions"></a>자동 관리 제안

3 watchOS 사전 내 사용자에 게 정보를 표시 하려면 응용 프로그램 허용 컨텍스트를 제공 합니다. 이 기능을 지원 하기 위해는 [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) 이제는 `MapItem` 앱에 다른 앱에서 나중에 사용할 위치 정보를 제공 하는 속성입니다.

자세한 내용은 참조 하세요 우리의 [사전 제안 소개](~/ios/watchos/platform/proactive-suggestions.md) 가이드입니다.

<a name="Security-and-Privacy-Enhancements" />

## <a name="security-and-privacy-enhancements"></a>보안 및 개인 정보 보호 향상 된 기능

Apple은 보안 및 응용 프로그램의 보안을 개선 하 고 최종 사용자의 개인 정보 보호 개발자 watchOS 3의에서 개인 정보를 모두에 몇 가지 향상 되었습니다.

결과적으로, watchOS 3 (또는 이후 버전)에서 실행 되는 앱 해야 정적으로 선언할에 하나 이상의 개인 정보 보호 특정 키를 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하는 `Info.plist` 사용자 응용 프로그램에 액세스 하려는 이유를 설명 하는 파일입니다.

Ios 10 이러한 변경 내용을 공유 하는 3 watchOS, 하므로 iOS 10을 참조 하십시오 [보안 및 개인 정보 보호 향상 된 기능](~/ios/app-fundamentals/security-privacy.md) 자세한 정보에 대 한 가이드입니다.

<a name="Snapshots-and-Dock" />

## <a name="snapshots-and-dock"></a>스냅숏 및 도킹

3 watchOS Apple 도킹 스테이션 사용자가 즐겨 찾는 앱 고정 고 저장해 두고 빠르게 액세스할 수를 추가 했습니다. Apple Watch 측면 단추를 누를 때 스냅숏 고정 된 응용 프로그램 갤러리 표시 됩니다. 사용자 수 왼쪽 이나 오른쪽으로 원하는 앱을 찾은 다음 유틸리티를 실행 중인 응용 프로그램 인터페이스 스냅숏을 바꿉니다 실행 하려면 앱을 누르면 살짝 밉니다.

정기적으로 시스템 응용 프로그램의 UI의 스냅숏을 사용 하 고 해당 스냅숏으로 사용 하 여 문서. watchOS는 앱이이 스냅숏을 생성 하기 전에 해당 콘텐츠 및 UI를 업데이트할 수를 제공 합니다.

자세한 내용은 참조 하십시오 우리의 [백그라운드 작업](~/ios/watchos/platform/background-tasks.md) 가이드 및 Apple의 [WKSnapshotRefreshBackgroundTask 참조](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) 합니다.

<a name="User-Notifications" />

## <a name="user-notifications"></a>사용자 알림

사용자 알림 프레임 워크 watchOS 3에에서 도입 된 Apple Watch 로컬 및 원격 알림 배달을 지원 합니다. 이 프레임 워크를 사용 하 여 하루 또는 한 위치 시간 등 특정 조건에 따라 알림을 예약할 하를 수신 하 고 알림을 처리 합니다.

자세한 내용은 참조 하세요 우리의 [watchOS 3에 대 한 빠른 상호 작용 기술을](~/ios/watchos/platform/quick-interaction-techniques.md) 가이드입니다.

<a name="Watch-Connectivity-Framework-Enhancements" />

## <a name="watch-connectivity-framework-enhancements"></a>연결 프레임 워크 기능 향상을 시청

새 `HasContentPending` 의 속성은 [WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession) 클래스 세션 처리 해야 하는 백그라운드에서 데이터를 수신 했을 나타냅니다. 및 `RemainingComplicationUserInfoTransfers` 속성 남은 시간을 iOS 앱의 watchOS 복잡 함 중 하나를 업데이트할 수를 반환 합니다.

자세한 내용은 참조 하세요 우리의 [백그라운드 작업](~/ios/watchos/platform/background-tasks.md) 가이드입니다.

<a name="WatchKit-Framework-Enhancements" />

## <a name="watchkit-framework-enhancements"></a>WatchKit 프레임 워크의 향상 된 기능

3 watchOS WatchKit 프레임 워크는 다음을 비롯 한 여러 개선 된 기능이 포함 되어 있습니다.

- 응용 프로그램 수의 상태를 가져올 디지털 왕관 사용 하 여 새 [WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer) 클래스 및 사용자를 사용 하 여 왕관 회전할 때 업데이트를 받을 [WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate) 클래스입니다.
- [WKExtension](https://developer.apple.com/reference/watchkit/wkextension) 클래스에 포함 되어 이제는 `ApplicationState` 메서드 및 [WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate) 앱 응용 프로그램의 런타임 상태를 추적 하는 데 사용할 수 있는 상수입니다. `WKExtension` 또한 백그라운드 작업을 예약 하는 데 사용할 수 있는 두 개의 새 메서드를 제공 합니다.
- [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) 이제 새 기능이 포함 됩니다 `ApplicationWillEnterForeground`, `ApplicationDidEnterBackground` 및 `HandleBackgroundTasks` 응용 프로그램의 상태 변경을 모니터링 하 고 백그라운드 작업 업데이트를 처리 하는 메서드.
- 새 [WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) 클래스가 조사식 앱에 인식 제스처의 다음과 같은 형식을 제공 하도록 추가 되었습니다: [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer), [WKPanGestureRecognizer ](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer), [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) 및 [WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer)합니다.
- 새 [WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera) 모든 HomeKit IP 카메라 연결에 대 한 클래스는 인터페이스를 제공 합니다.
- 새 [WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie) 클래스를 사용 하면 응용 프로그램에서 사용자 누르면 실행 중인 영화로 바뀔 "포스터" 동영상을 표시 합니다.
- 새 [WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton) 클래스를 사용 하면 응용 프로그램에서 Apple Pay 단추를 누를 때 지불 요청을 시작 하는 해당 UI에 표시 합니다.
- 새 [WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene) 클래스 SceneKit 장면 Apple Watch 표시 하기 위한 인터페이스를 나타냅니다.
- 새 [WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene) 클래스 SpriteKit 장면 Apple Watch 표시 하기 위한 인터페이스를 나타냅니다.

자세한 내용은 참조 하세요 우리의 [watchOS 3에 대 한 빠른 상호 작용 기술을](~/ios/watchos/platform/quick-interaction-techniques.md) 가이드입니다.

<a name="Workout-App-Enhancements" />

## <a name="workout-app-enhancements"></a>워크 아웃 응용 프로그램의 향상 된 기능

새로운 3 watchOS 워크 아웃 관련 응용 프로그램에서 Apple Watch 백그라운드에서 실행 하는 기능이 있습니다. 응용 프로그램을 사용 하려면이 기능 (HealthKit 데이터에 액세스할 수) 포함 해야 합니다는 `WKBackgroundModes` 키에 `Info.plist` 값을 가진 파일 `workout-processing`합니다.

또한 개발자는 이제 쌍을 이루는 iPhone에서 iOS 응용 프로그램 버전에서 watchOS 워크 아웃 앱을 시작할 수가 있습니다.

자세한 내용은 참조 하세요 우리의 [워크 아웃 응용 프로그램의 향상 된 기능](~/ios/watchos/platform/workout-apps.md) 가이드입니다.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>추가 프레임 워크 변경 내용

주요 framework 변경 내용 및 위에 나열 된 추가 기능 뿐만 아니라 Apple watchOS 3에서에서 많은 추가 부 프레임 워크 변경을 했습니다.

자세한 내용은 참조 하세요 우리의 [추가 프레임 워크 변경](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md) 가이드입니다.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>사용되지 않는 API

다음 Api에서에서 사용 되지 watchOS 3:

- `UILocalNotification` UIKit의 클래스는 사용 되지 않으며 사용자 알림 프레임 워크를 대체 해야 합니다.

Apple의를 참조 하십시오. [watchOS 3.0 API 차이점을 2.2 watchOS](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html) 결함 및 변경의 전체 목록은 대 한 설명서입니다.


## <a name="related-links"></a>관련 링크

- [watchOS 샘플](https://developer.xamarin.com/samples/watchos/all/)
- [WatchOS 3의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
