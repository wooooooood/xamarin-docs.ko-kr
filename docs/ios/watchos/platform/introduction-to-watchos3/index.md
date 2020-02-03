---
title: watchOS 3 소개
description: 이 문서에서는 Xamarin 개발자를 위해 watchOS 3에서 사용할 수 있는 새로운 Api 및 수정 된 Api 및 기능을 모두 소개 합니다.
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/07/2017
ms.openlocfilehash: f849ad9d722e297438b3960f74953ff922be0e56
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725289"
---
# <a name="introduction-to-watchos-3"></a>watchOS 3 소개

_이 문서에서는 Xamarin 개발자를 위해 watchOS 3에서 사용할 수 있는 새로운 Api 및 수정 된 Api 및 기능을 모두 소개 합니다._

이 문서에서는 다음 항목을 다룹니다.

- [WatchOS 3의 새로운 기능](#Whats-New-in-watchOS-3)
  - [Apple Pay 향상](#Apple-Pay-Enhancements) 된 기능을 통해 Apple Watch에서 앱 내 지불액에 대 한 지원이 추가 되었습니다.
  - [백그라운드 작업](#Background-Tasks) 을 통해 앱이 백그라운드에서 정보를 업데이트 하는 기능을 제공 하므로 사용자가 필요할 때 준비가 됩니다.
  - 앱에 대 한 새로운 기능을 제공 하는 watchOS 3에 대 한 [복잡 한 기능이 향상](#Complications-Enhancements) 되었습니다.
  - [새로 제공 되는 프레임 워크](#Newly-Available-Frameworks) 는 watchOS apps에 대해 노출 됩니다.
  - [사전 제안을](#Proactive-Suggestions) 통해 앱은 사용자에 게 정보를 사전에 표시할 수 있습니다.
  - WatchOS 3에 대 한 몇 가지 [보안 및 개인 정보 향상 기능이 향상](#Security-and-Privacy-Enhancements) 되었습니다.
  - [스냅숏 및 도킹](#Snapshots-and-Dock) 은 사용자에 게 앱 watchOS 앱에 대 한 빠른 액세스를 제공 합니다.
  - [사용자 알림은](#User-Notifications) 사용자에 게 로컬 알림과 원격 알림을 모두 제공 합니다.
  - WatchOS 3에서는 몇 가지 [감시 연결 프레임 워크 기능이 향상](#Watch-Connectivity-Framework-Enhancements) 되었습니다.
  - WatchOS 3에는 몇 가지 [WatchKit Framework 기능이 향상](#WatchKit-Framework-Enhancements) 되었습니다.
  - [향상 된 응용 프로그램 향상](#Workout-App-Enhancements) 은 Apple Watch 앱에 대 한 새로운 기능을 제공 합니다.
- WatchOS 3에서 [프레임 워크를 추가로 변경할](#Additional-Framework-Changes) 수 있습니다.
- WatchOS 3에서 [사용 되지 않는 api](#Deprecated-APIs)

<a name="Whats-New-in-watchOS-3" />

## <a name="whats-new-in-watchos-3"></a>WatchOS 3의 새로운 기능

Apple은 다음을 비롯 한 기존 기능에 대 한 여러 향상 된 기능과 함께 watchOS 3의 새로운 Api 및 서비스를 추가 했습니다.

<a name="Apple-Pay-Enhancements" />

## <a name="apple-pay-enhancements"></a>향상 된 Apple Pay

WatchOS 3에서는 PassKit 프레임 워크를 확장 하 여 Apple Watch에서 실행 되는 앱에 대 한 안전한 앱 내 지불 (물리적 상품 및 서비스 모두)을 지원할 수 있습니다.

새 [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) 및 [PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate) 클래스를 사용 하 여 사용자가 지불 요청에 대 한 권한을 부여할 수 있는 인터페이스를 제공 하 고 응답 합니다.

자세한 내용은 [Apple Pay 향상 된 기능](~/ios/watchos/platform/apple-pay.md) 가이드를 참조 하세요.

<a name="Background-Tasks" />

## <a name="background-tasks"></a>백그라운드 작업

watchOS 3에는 앱이 정보를 업데이트 하는 데 사용할 수 있는 몇 가지 백그라운드 작업이 도입 되어 사용자가 파일을 열기 전에 필요한 콘텐츠가 있는지 확인 합니다.

다음과 같은 새 백그라운드 작업을 사용할 수 있습니다.

- **백그라운드 응용 프로그램 새로 고침** - [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) 작업을 사용 하면 앱이 백그라운드에서 상태를 업데이트할 수 있습니다. 일반적으로이 작업에는 [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession)를 사용 하 여 인터넷에서 새 콘텐츠를 다운로드 하는 등의 다른 작업이 포함 됩니다.
- **백그라운드 스냅숏 새로 고침** - [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) 작업을 사용 하면 시스템이 도크를 채우는 데 사용 되는 스냅숏을 만들기 전에 콘텐츠와 UI를 모두 업데이트할 수 있습니다.
- **백그라운드 감시 연결** -쌍을 이루는 iPhone의 배경 데이터를 받을 때 앱에 대 한 [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) 작업을 시작 합니다.
- **백그라운드 URL 세션** -백그라운드 전송에 권한 부여가 필요 하거나 완료 (오류)가 완료 되 면 앱에 대 한 [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) 작업이 시작 됩니다.

자세히 알아보려면 [백그라운드 작업](~/ios/watchos/platform/background-tasks.md) 가이드를 참조 하세요.

<a name="Complications-Enhancements" />

## <a name="complications-enhancements"></a>향상 된 기능

유용한 정보를 한 눈에 파악할 수 있는 작은 시각적 요소는 복잡 합니다. 선택 된 조사식에 따라 사용자는 하나 이상의 복잡 한 조사식 얼굴을 사용자 지정할 수 있습니다.

watchOS 3은 사용자가 시계 화면에서 정보를 한눈에 볼 수 있도록 앱에 watch 앱에 대해 하나 이상의 복잡 한 기능을 만들 수 있는 기능을 제공 합니다.

또한 다음과 같은 이점을 제공 합니다.

- 사용자는 시계 화면에서 바로 복잡 한 방식으로 앱을 신속 하 게 시작할 수 있습니다.
- 응용 프로그램의 감시에 대 한 문제 중 하나가 발생 하면 시스템은 백그라운드에서 앱을 시작 하 고 메모리에 유지 하 고 업데이트할 추가 시간을 제공 하는 즉시 시작 상태로 앱을 유지 합니다.
- 매우 복잡 한 일은 하루에 최소 50의 푸시 업데이트를 보장 합니다.
- 앱이 복잡 한 경우에는 Apple Watch 얼굴 갤러리에서 제공 됩니다.

WatchOS 3의 ClockKit 프레임 워크에는 이제 [CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext) 및 [CLKComplicationTemplateExtraLargeRingImage](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage)와 같은 매우 많은 복잡 한 새로운 템플릿이 포함 되어 있습니다. 또한 지역화 가능한 텍스트를 만들려면 [Clktextprovider](https://developer.apple.com/reference/clockkit/clktextprovider) 클래스의 새 메서드를 사용 합니다.

자세한 내용은 [watchOS에 대 한 빠른 상호 작용 기술 3](~/ios/watchos/platform/quick-interaction-techniques.md) 가이드를 참조 하세요.

<a name="Newly-Available-Frameworks" />

## <a name="newly-available-frameworks"></a>새로 사용 가능한 프레임 워크

watchOS 3에는 이전에 사용할 수 없었던 기존 Apple 프레임 워크가 다음과 같이 포함 되어 있습니다.

- **SceneKit** -조명, 음영, 애니메이션, 물리학 및 파티클 시스템과 같은 다른 플랫폼에서 사용할 수 있는 대부분의 기능을 포함 하 여 3d 모델을 watch 앱의 UI에 포함 하려면 SceneKit를 사용 합니다. 3D 공간 오디오, 사용자 지정 금속 또는 OpenGL 셰이더, 핵심 이미지 필터 및 물리적 기반 자료는 지원 되지 않습니다.
- **SpriteKit** -SpriteKit을 사용 하 여 작업, 물리, 조명 및 파티클 시스템과 같은 다른 플랫폼에서 사용할 수 있는 대부분의 기능을 포함 하 여 앱 감시 앱의 UI에서 스프라이트를 렌더링 하 고 애니메이션 효과를 적용 합니다. 3D 공간 오디오, 비디오 재생 및 핵심 이미지 필터는 지원 되지 않습니다.
- **Avfoundation** -오디오를 관리 하 고 재생 합니다.
- **Cloudkit** -watch 앱과 iCloud 컨테이너 간에 데이터를 이동 합니다.
- **핵심 오디오** -오디오 스트림을 나타내는 데이터 형식, 복잡 한 버퍼 및 시간 값을 관리 하는 데 사용할 수 있습니다.
- **GameKit** -소셜 게임을 만듭니다.

<a name="Proactive-Suggestions" />

## <a name="proactive-suggestions"></a>자동 제안

watchOS 3을 사용 하면 앱에서 지정 된 컨텍스트 내에서 사용자에 게 정보를 사전에 제공할 수 있습니다. 이 기능을 지원 하기 위해 [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) 에는 나중에 다른 앱에서 사용할 수 있도록 앱에서 위치 정보를 제공할 수 있도록 하는 `MapItem` 속성이 포함 되어 있습니다.

자세히 알아보려면 [사전 권장 사항 소개 가이드를](~/ios/watchos/platform/proactive-suggestions.md) 참조 하세요.

<a name="Security-and-Privacy-Enhancements" />

## <a name="security-and-privacy-enhancements"></a>보안 및 개인 정보 향상

Apple은 watchOS 3의 보안과 개인 정보 보호에 대 한 몇 가지 향상 된 기능을 통해 개발자가 앱의 보안을 개선 하 고 최종 사용자의 개인 정보를 확인 하는 데 도움이 됩니다.

따라서 watchOS 3 이상에서 실행 되는 앱은 사용자에 게 액세스 권한을 얻으려고 하는 이유를 설명 하는 개인 정보를 하나 이상 해당 `Info.plist` 파일에 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하는 의도를 정적으로 선언 해야 합니다.

WatchOS 3은 이러한 변경 내용을 iOS 10과 공유 하므로 iOS 10 [보안 및 개인 정보 향상](~/ios/app-fundamentals/security-privacy.md) 가이드를 참조 하 여 자세한 내용을 확인 하세요.

<a name="Snapshots-and-Dock" />

## <a name="snapshots-and-dock"></a>스냅숏 및 도킹

WatchOS 3에서 Apple은 사용자가 즐겨 찾는 앱을 고정 하 고 신속 하 게 액세스할 수 있는 도크를 추가 했습니다. 사용자가 Apple Watch의 측면 단추를 누르면 고정 된 앱 스냅숏의 갤러리가 표시 됩니다. 사용자는 왼쪽 또는 오른쪽으로 살짝 밀어 원하는 앱을 찾은 다음 앱을 탭 하 여 실행 중인 앱의 인터페이스로 스냅숏을 바꿀 수 있습니다.

시스템은 정기적으로 앱 UI의 스냅숏을 사용 하 고 이러한 스냅숏을 사용 하 여 문서를 채웁니다. watchOS를 사용 하면이 스냅숏이 만들어지기 전에 앱에서 콘텐츠 및 UI를 업데이트할 수 있습니다.

자세한 내용은 [백그라운드 작업](~/ios/watchos/platform/background-tasks.md) 가이드 및 Apple의 [WKSnapshotRefreshBackgroundTask 참조](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) 를 참조 하세요.

<a name="User-Notifications" />

## <a name="user-notifications"></a>사용자 알림

WatchOS 3에 도입 된 사용자 알림 프레임 워크는 Apple Watch에 대 한 로컬 알림과 원격 알림을 모두 제공 하도록 지원 합니다. 이 프레임 워크를 사용 하 여 시간 또는 위치와 같은 특정 조건에 따라 알림을 예약 하 고 알림을 수신 및 처리할 수 있습니다.

자세한 내용은 [watchOS에 대 한 빠른 상호 작용 기술 3](~/ios/watchos/platform/quick-interaction-techniques.md) 가이드를 참조 하세요.

<a name="Watch-Connectivity-Framework-Enhancements" />

## <a name="watch-connectivity-framework-enhancements"></a>연결 프레임 워크의 향상 된 기능 보기

[WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession) 클래스의 새 `HasContentPending` 속성은 세션이 처리 해야 하는 배경에 데이터를 수신 했음을 나타냅니다. `RemainingComplicationUserInfoTransfers` 속성은 iOS 앱이 watchOS의 복잡 한 업데이트를 업데이트할 수 있는 남은 시간을 반환 합니다.

자세히 알아보려면 [백그라운드 작업](~/ios/watchos/platform/background-tasks.md) 가이드를 참조 하세요.

<a name="WatchKit-Framework-Enhancements" />

## <a name="watchkit-framework-enhancements"></a>WatchKit Framework의 향상 된 기능

watchOS 3에는 다음과 같은 WatchKit 프레임 워크에 대 한 몇 가지 향상 된 기능이 포함 되어 있습니다.

- 앱은 새 [WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer) 클래스를 사용 하 여 Digital Crown 상태를 가져올 수 있으며, 사용자가 [WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate) 클래스를 사용 하 여 ccs (crown를 회전 하면 업데이트를 받을 수 있습니다.
- 이제 [WKExtension](https://developer.apple.com/reference/watchkit/wkextension) 클래스에 앱의 런타임 상태를 추적 하는 데 사용할 수 있는 `ApplicationState` 메서드 및 [WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate) 상수가 포함 됩니다. 또한 `WKExtension`에서는 백그라운드 작업을 예약 하는 데 사용할 수 있는 두 가지 새로운 메서드를 제공 합니다.
- 이제 [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) 에는 새로운 `ApplicationWillEnterForeground`, `ApplicationDidEnterBackground` 및 `HandleBackgroundTasks` 메서드가 포함 되어 앱의 상태 변경을 모니터링 하 고 백그라운드 작업 업데이트를 처리 합니다.
- Watch 앱에 [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer), [WKPanGestureRecognizer](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer), [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) 및 [WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer)유형의 제스처 인식을 제공 하기 위해 새 [WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) 클래스가 추가 되었습니다.
- 새 [WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera) 클래스는 모든 HomeKit 연결 된 IP 카메라에 대 한 인터페이스를 제공 합니다.
- 새 [WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie) 클래스를 사용 하면 앱에서 사용자가 탭 할 때 실행 중인 동영상으로 교체 되는 영화 "포스터"를 표시할 수 있습니다.
- 새 [WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton) 클래스를 사용 하면 앱에서 UI에 Apple Pay 단추를 표시 하 여 탭 할 때 지불 요청을 시작할 수 있습니다.
- 새 [WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene) 클래스는 Apple Watch에 SceneKit 장면을 표시 하기 위한 인터페이스를 제공 합니다.
- 새 [WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene) 클래스는 Apple Watch에 SpriteKit 장면을 표시 하기 위한 인터페이스를 제공 합니다.

자세한 내용은 [watchOS에 대 한 빠른 상호 작용 기술 3](~/ios/watchos/platform/quick-interaction-techniques.md) 가이드를 참조 하세요.

<a name="Workout-App-Enhancements" />

## <a name="workout-app-enhancements"></a>운동 앱 고급 기능

WatchOS 3의 새로운 기능으로, 진행 중인 관련 앱은 Apple Watch의 배경에서 실행할 수 있습니다. 이 기능을 사용 하도록 설정 하 고 HealthKit 데이터에 대 한 액세스 권한을 얻으려면 앱은 `workout-processing`값을 사용 하 여 `Info.plist` 파일에 `WKBackgroundModes` 키를 포함 해야 합니다.

또한 개발자는 쌍을 이루는 iPhone의 iOS 앱 버전에서 watchOS 체력 단련 앱을 시작할 수 있습니다.

자세한 내용은 [앱 개선 기능](~/ios/watchos/platform/workout-apps.md) 가이드를 참조 하세요.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>추가 프레임 워크 변경

Apple은 위에 나열 된 주요 프레임 워크 변경 및 추가 사항 외에도 watchOS 3에서 추가 사소한 프레임 워크 변경을 많이 수행 했습니다.

자세히 알아보려면 [추가 프레임 워크 변경](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md) 가이드를 참조 하세요.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>사용되지 않는 API

WatchOS 3에서 다음 Api는 더 이상 사용 되지 않습니다.

- UIKit의 `UILocalNotification` 클래스는 더 이상 사용 되지 않으며 사용자 알림 프레임 워크로 바꾸어야 합니다.

결함 및 변경 사항의 전체 목록은 Apple의 [watchOS 2.2 To watchOS 3.0 API 차이점 설명서를](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html) 참조 하세요.

## <a name="related-links"></a>관련 링크

- [watchOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+watchOS)
- [WatchOS 3의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
