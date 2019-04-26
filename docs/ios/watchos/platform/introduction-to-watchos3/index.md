---
title: watchOS 3 소개
description: 이 문서에서는 Xamarin 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 watchOS 3에서에서 사용할 수 있는 기능을 소개합니다.
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/07/2017
ms.openlocfilehash: 0428a0df157e359ab34a6a71dbba31bdeb6962fa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61224064"
---
# <a name="introduction-to-watchos-3"></a>watchOS 3 소개

_이 문서에서는 Xamarin 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 watchOS 3에서에서 사용할 수 있는 기능을 소개합니다._

이 문서는 다음 항목을 설명 합니다.

- [WatchOS 3에서 새 란](#Whats-New-in-watchOS-3)
    - [Apple 지불 향상](#Apple-Pay-Enhancements) Apple Watch 앱의 결제에 대 한 지원을 추가 합니다.
    - [백그라운드 작업](#Background-Tasks) 앱 사용자가 필요할 때 대비 되어 백그라운드에서 해당 정보를 업데이트 하는 기능을 제공 합니다.
    - [복잡성이 향상 된 기능](#Complications-Enhancements) 앱에 대 한 새로운 기능을 제공 하는 watchOS 3에 대 한 내용이 있습니다.
    - [새로 사용 가능한 프레임 워크](#Newly-Available-Frameworks) watchOS 앱에 대 한 노출 했습니다.
    - [자동 제안](#Proactive-Suggestions) 앱 사용자에 게 정보를 사전에 표시할 수 있습니다.
    * 몇 가지 [보안 및 개인 정보 보호 향상](#Security-and-Privacy-Enhancements) watchOS 3 하였습니다.
    - [스냅숏 및 도킹](#Snapshots-and-Dock) 앱 watchOS 앱에 대 한 빠른 액세스를 사용 하 여 사용자를 제공 합니다.
    - [사용자 알림](#User-Notifications) 로컬 및 원격 알림이 사용자에 게 제공 합니다.
    * 여러 [조사식 연결 프레임 워크 향상 된 기능](#Watch-Connectivity-Framework-Enhancements) watchOS 3 이루어졌습니다.
    * 몇 가지 [WatchKit Framework의 향상 된](#WatchKit-Framework-Enhancements) watchOS 3 이루어졌습니다.
    - [운동 앱 향상 된 기능](#Workout-App-Enhancements) 는 단련 하는 새로운 기능은 제공 관련 Apple Watch 앱.
- [추가 프레임 워크 변경 내용](#Additional-Framework-Changes) watchOS 3 전체 이루어졌습니다.
- [사용 되지 않는 Api](#Deprecated-APIs) watchOS 3에에서 있습니다.

<a name="Whats-New-in-watchOS-3" />

## <a name="whats-new-in-watchos-3"></a>WatchOS 3에서 새 란

Apple이 Api 및 서비스에 대 한 몇 가지 새 기존 기능을 비롯 한 향상 된 기능 함께 watchOS 3에에서 추가 합니다.

<a name="Apple-Pay-Enhancements" />

## <a name="apple-pay-enhancements"></a>Apple Pay 향상 된 기능

WatchOS 3, PassKit framework Apple Watch 실행 되는 앱에 대 한 실제 상품 및 서비스) (의 안전 하 고 앱에서 지불에 대 한 지원을 수 있도록 확장 되었습니다.

새 [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) 하 고 [PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate) 있고 사용자 지불 요청 권한을 부여할 수 있는 인터페이스에 응답 하는 클래스입니다.

자세한 내용을 참조 하세요 우리의 [Apple 지불 향상](~/ios/watchos/platform/apple-pay.md) 가이드입니다.

<a name="Background-Tasks" />

## <a name="background-tasks"></a>백그라운드 작업

watchOS 3에서는 앱을 열기 전에 사용자가 해야 하는 콘텐츠가 있는지 확인 합니다. 해당 정보를 업데이트 하는 데 사용할 수 있는 몇 가지 백그라운드 작업을 소개 합니다.

다음 새 백그라운드 작업을 사용할 수 있습니다.

- **백그라운드 앱 새로 고침** - [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) 작업 앱이 백그라운드에서 해당 상태를 업데이트할 수 있게 합니다. 일반적으로 여기에 사용 하 여 인터넷에서 새 콘텐츠를 다운로드 하는 등 다른 작업을 [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession)합니다.
- **스냅숏 새로 고침 백그라운드** - [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) 작업 시스템은 도킹 스테이션을 채우는 데 사용할 스냅숏을 전에 해당 콘텐츠 및 UI를 업데이트 하려면 앱을 사용 합니다.
- **조사식 연결 백그라운드** - [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) 쌍을 이루는 iPhone에서 백그라운드 데이터를 받을 때 앱에 대 한 작업을 시작 합니다.
- **URL 세션 백그라운드** - [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) 백그라운드 전송 하는 권한 부여 필요 하거나 (성공적으로 또는 오류)를 완료 하는 경우 앱에 대 한 작업을 시작 합니다.

자세한 내용을 참조 하세요 우리의 [백그라운드 작업](~/ios/watchos/platform/background-tasks.md) 가이드입니다.

<a name="Complications-Enhancements" />

## <a name="complications-enhancements"></a>복잡성이 향상 된 기능

복잡성은 한 눈에 유용한 정보를 제공 하는 작은 visual 요소입니다. 선택한 watch 화면에 따라 사용자는 하나 이상의 Complication 사용 하 여 시계 모드를 사용자 지정할 수 있습니다.

watchOS 3 앱 사용자가 해당 정보는 일목요연 시계 모드에서 액세스할 수 있도록 watch 앱에 대 한 하나 이상의 Complication를 만드는 기능을 제공 합니다.

또한 복잡성에는 다음과 같은 이점을 제공합니다.

- 신속 하 게 사용자 시계 모드에서 직접 complication의 탭 하 여 앱을 시작할 수 있습니다.
- 조사식 얼굴 원인에 대 있는 앱의 복잡성 중 하나는 백그라운드에서 앱을 시작 하려고 준비-시작 상태의 앱을 유지 하려면 시스템에 보관 메모리 및 업데이트 하는 추가 시간으로 제공 합니다.
- 복잡성은 하루에 최소 50 개의 강제 업데이트 보장 됩니다.
- Apple Watch 얼굴 갤러리에서 제공 됩니다는 복잡성을 포함 하는 경우 (Apple의를 참조 하세요 [갤러리에 추가 복잡성](https://developer.apple.com/documentation/clockkit/adding_complications_to_the_gallery) 자세한 내용은 설명서).

WatchOS 3에에서 ClockKit 프레임 워크를 이제 초대형 복잡성에 대 한 몇 가지 새 템플릿을 같은 [CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext) 고 [CLKComplicationTemplateExtraLargeRingImage ](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). 또한 지역화할 수 있는 텍스트를 만들려면의 새 메서드를 사용 합니다 [CLKTextProvider](https://developer.apple.com/reference/clockkit/clktextprovider) 클래스입니다.

자세한 내용을 참조 하세요 우리의 [watchOS 3에 대 한 빠른 상호 작용 기술](~/ios/watchos/platform/quick-interaction-techniques.md) 가이드입니다.

<a name="Newly-Available-Frameworks" />

## <a name="newly-available-frameworks"></a>새로 사용 가능한 프레임 워크

watchOS 3와 같은 이전에 불가능 했던 몇 가지 기존 Apple 프레임 워크를 포함 합니다.

- **SceneKit** -대부분의 조명, 음영, 애니메이션, 물리학와 같은 다른 플랫폼에서 제공 되는 기능 및 파티클 시스템을 포함 하 여 watch 앱의 UI에 3D를 포함 하도록 SceneKit을 사용 하 여 모델링 합니다. 3D 공간 오디오, 사용자 지정 메탈 또는 OpenGL 셰이더, Core 이미지 필터 및 물리적 기반 자료 지원 되지 않습니다.
- **SpriteKit** -사용 하 여 SpriteKit를 렌더링 하 고 대부분의 작업, 물리, 조명 및 파티클 시스템 등의 다른 플랫폼에서 사용할 수 있는 기능을 포함 하 여 앱 watch 앱의 UI에서 스프라이트 애니메이션을 적용 합니다. 3D 공간 오디오, 비디오 재생 및 Core 이미지 필터는 지원 되지 않습니다.
- **AVFoundation** 오디오를 재생 하 고 관리 합니다.
- **CloudKit** -watch 앱와 iCloud 컨테이너 간에 데이터를 이동 합니다.
- **오디오 핵심** -오디오 스트림, 복잡 한 버퍼 및 시간 값을 나타내기 위한 데이터 형식을 관리 합니다.
- **GameKit** -소셜 게임을 만듭니다.

<a name="Proactive-Suggestions" />

## <a name="proactive-suggestions"></a>자동 제안

watchOS 3 사전 내에서 사용자에 게 정보를 제공 하는 앱을 사용 하면 컨텍스트를 지정 합니다. 이 기능을 지원 하기를 [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) 이제는 `MapItem` 앱에 다른 앱에서 나중에 사용할 위치 정보를 제공 하는 속성입니다.

자세한 내용을 참조 하세요 우리의 [자동 제안 소개](~/ios/watchos/platform/proactive-suggestions.md) 가이드입니다.

<a name="Security-and-Privacy-Enhancements" />

## <a name="security-and-privacy-enhancements"></a>보안 및 개인 정보 보호 향상

Apple은 보안 및 개인 정보 보호 개발자가 앱의 보안을 강화 하 고 최종 사용자의 개인 정보 보호에 도움이 되는 watchOS 3에에서 모두 몇 가지 향상 되었습니다.

WatchOS 3 (이상)에서 실행 중인 앱에서 하나 이상의 개인 특정 키를 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하려면 의도 해야 정적으로 선언 하는 결과적으로, 해당 `Info.plist` 앱에 액세스 하려는 이유는 사용자에 게 설명 하는 파일입니다.

WatchOS 3 iOS 10 사용 하 여 이러한 변경 내용을 공유 하므로 iOS 10을 참조 하세요 [보안 및 개인 정보 보호 향상](~/ios/app-fundamentals/security-privacy.md) 자세한 가이드입니다.

<a name="Snapshots-and-Dock" />

## <a name="snapshots-and-dock"></a>스냅숏 및 도킹

WatchOS 3에에서 Apple 도킹 스테이션 사용자가 즐겨 찾는 앱을 고정 하 고 신속 하 게 액세스할 수 있는 추가 되었습니다. 사용자의 Apple Watch 쪽 단추를 누르면 고정 된 앱 스냅숏 갤러리에 표시 됩니다. 사용자는 왼쪽 또는 오른쪽 원하는 앱을 찾은 다음 실행 중인 앱의 인터페이스를 사용 하 여 스냅숏을 대체 시작 앱 탭에서 살짝 밀 수입니다.

정기적으로 시스템은 앱의 UI의 스냅숏을 생성 하 고 이러한 스냅숏을 사용 하 여 문서를 채웁니다. watchOS 앱이이 스냅숏을 수행 하기 전에 해당 콘텐츠 및 UI를 업데이트할 수 있는 기회를 제공 합니다.

자세한 내용은 참조 하십시오 우리의 [백그라운드 작업](~/ios/watchos/platform/background-tasks.md) 가이드 및 Apple [WKSnapshotRefreshBackgroundTask 참조](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) 합니다.

<a name="User-Notifications" />

## <a name="user-notifications"></a>사용자 알림

WatchOS 3에에서 도입 된 사용자 알림 프레임 워크는 Apple Watch 로컬 및 원격 알림 배달을 지원 합니다. 이 프레임 워크를 사용 하 여 위치 또는 시간 등 특정 조건에 따라 알림을 예약 하 고을 받고 알림을 처리 합니다.

자세한 내용을 참조 하세요 우리의 [watchOS 3에 대 한 빠른 상호 작용 기술](~/ios/watchos/platform/quick-interaction-techniques.md) 가이드입니다.

<a name="Watch-Connectivity-Framework-Enhancements" />

## <a name="watch-connectivity-framework-enhancements"></a>연결 프레임 워크 향상 된 기능 보기

새 `HasContentPending` 의 속성을 [WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession) 클래스 세션 처리 해야 하는 백그라운드에서 데이터를 수신 했음을 나타냅니다. 및 `RemainingComplicationUserInfoTransfers` 속성 나머지 시간에 iOS 앱 해당 watchOS Complication 업데이트 수를 반환 합니다.

자세한 내용을 참조 하세요 우리의 [백그라운드 작업](~/ios/watchos/platform/background-tasks.md) 가이드입니다.

<a name="WatchKit-Framework-Enhancements" />

## <a name="watchkit-framework-enhancements"></a>WatchKit 프레임 워크 향상 된 기능

watchOS 3 WatchKit 프레임 워크는 다음을 비롯 한 여러 개선 된 기능을 포함 합니다.

- 앱 수의 상태를 가져올 디지털 Crown 새 [WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer) 클래스 및 사용 하 여 crown를 회전할 때 업데이트를 수신 합니다 [WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate) 클래스입니다.
- 합니다 [WKExtension](https://developer.apple.com/reference/watchkit/wkextension) 클래스에 포함 되어 이제는 `ApplicationState` 메서드 및 [WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate) 앱은 앱의 런타임 상태를 추적 하는 데 사용할 수 있는 상수입니다. `WKExtension` 또한 백그라운드 작업을 예약할 수 있는 두 개의 새 메서드를 제공 합니다.
- [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) 이제 새 `ApplicationWillEnterForeground`, `ApplicationDidEnterBackground` 고 `HandleBackgroundTasks` 앱의 상태 변경을 모니터링 하 고 백그라운드 작업 업데이트를 처리 하는 메서드.
- 새 [WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) 제스처 인식 watch 앱의 다음 형식을 제공 하도록 클래스가 추가 되었습니다. [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer), [WKPanGestureRecognizer](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer)합니다 [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) 하 고 [WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer)합니다.
- 새 [WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera) 모든 HomeKit 연결 IP 카메라에 대 한 클래스 인터페이스를 제공 합니다.
- 새 [WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie) 클래스를 사용 하면 응용 프로그램에서 사용자가이 누를 때 실행 중인 동영상으로 대체 되는 "포스터" 영화를 표시 합니다.
- 새 [WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton) 클래스를 사용 하면 응용 프로그램에서 Apple Pay 단추를 누를 때 결제 요청을 시작 하는 UI를 표시 합니다.
- 새 [WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene) 클래스 SceneKit 장면 Apple Watch 표시 하기 위한 인터페이스를 제공 합니다.
- 새 [WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene) 클래스 SpriteKit 장면 Apple Watch 표시 하기 위한 인터페이스를 제공 합니다.

자세한 내용을 참조 하세요 우리의 [watchOS 3에 대 한 빠른 상호 작용 기술](~/ios/watchos/platform/quick-interaction-techniques.md) 가이드입니다.

<a name="Workout-App-Enhancements" />

## <a name="workout-app-enhancements"></a>운동 앱 고급 기능

새 watchOS 3, 운동 관련 앱 Apple Watch 백그라운드에서 실행할 수 있습니다. 앱이이 기능을 사용 하도록 설정 (및 HealthKit 데이터에 액세스)를 포함 해야 합니다 `WKBackgroundModes` 키를 `Info.plist` 값을 사용 하 여 파일 `workout-processing`합니다.

또한 개발자는 이제 쌍을 이루는 iPhone에서 iOS 응용 프로그램 버전에서 watchOS 운동 앱을 시작할 수가 있습니다.

자세한 내용을 참조 하세요 우리의 [운동 앱 향상 된 기능](~/ios/watchos/platform/workout-apps.md) 가이드입니다.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>프레임 워크 추가 변경 내용

주요 프레임 워크 변경 내용 및 위에 나열 된 추가 외에 Apple watchOS 3에에서 많은 사소한 추가 프레임 워크 변경을 했습니다.

자세한 내용을 참조 하세요 우리의 [추가 프레임 워크 변경 내용](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md) 가이드입니다.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>사용되지 않는 API

다음 Api에서에서 사용 되지 watchOS 3:

- `UILocalNotification` UIKit의 클래스 사용 되지 않으며 사용자 알림 프레임 워크를 사용 하 여 대체 해야 합니다.

Apple의를 참조 하세요 [watchOS 3.0 API 차이점을 2.2 watchOS](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html) 결함 및 변경 내용 목록은 설명서.


## <a name="related-links"></a>관련 링크

- [watchOS 샘플](https://developer.xamarin.com/samples/watchos/all/)
- [WatchOS 3의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
