---
title: Xamarin에서 watchOS 알림
description: 이 문서에서는 Xamarin에서 watchOS 알림을 사용 하는 방법을 설명 합니다. 알림 컨트롤러를 만들고, 알림을 생성 하 고, 알림을 테스트 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 6be46d31ac2c16d02749519907d650588dbbcbe6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028223"
---
# <a name="watchos-notifications-in-xamarin"></a>Xamarin에서 watchOS 알림

시청 앱은 포함 하는 iOS 앱이 지원 되는 경우 알림을 받을 수 있습니다. 기본 제공 알림 처리가 있으므로 아래에 설명 된 추가 알림 지원을 추가할 *필요* 는 없지만 알림 동작 및 모양을 사용자 지정 하려는 경우에는을 참조 하세요.

솔루션에서 iOS 앱에 알림 지원을 추가 하는 방법에 대 한 자세한 내용은 [Ios 알림](~/ios/platform/user-notifications/deprecated/index.md) 문서를 참조 하세요.

## <a name="creating-notification-controllers"></a>알림 컨트롤러 만들기

스토리 보드 알림 컨트롤러에는 트리거를 트리거하는 특수 한 유형의 segue 있습니다. 새 **알림 인터페이스 컨트롤러** 를 스토리 보드로 끌면 자동으로 segue이 연결 됩니다.

![](notifications-images/notification-storyboard1.png "A new Notification Interface Controller with a segue attached")

알림 segue를 선택 하면 해당 속성을 편집할 수 있습니다.

![](notifications-images/notification-storyboard2.png "The notification segue selected")

컨트롤러를 사용자 지정한 후에는 WatchKitCatalog에서 다음 예제와 같이 보일 수 있습니다.

![](notifications-images/notifications-segue.png "The Notification Properties")

다음과 같은 두 가지 유형의 알림이 있습니다.

- 시스템에서 정의한 스크롤할 때 스크롤할 때 정적 뷰가 아닙니다.

- 사용자가 정의한 **길고** 스크롤 가능 하 고 사용자 지정이 가능한 뷰입니다. 더 간단한 정적 버전 및 보다 복잡 한 동적 버전을 지정할 수 있습니다.

### <a name="short-look-notification-controller"></a>단기 알림 컨트롤러

간단한 UI는 앱 아이콘, 앱 이름 및 알림 제목 문자열로 구성 됩니다.

사용자가 알림을 무시 하지 않으면 시스템은 자세한 정보를 제공 하는 긴 모양 알림으로 자동 전환 됩니다.

### <a name="long-look-notification-controller"></a>긴 알림 컨트롤러

OS는 여러 요소에 따라 정적 또는 동적 뷰를 표시할지 여부를 결정 합니다. 정적 인터페이스를 제공 해야 하며 필요에 따라 알림에 대 한 동적 인터페이스를 포함할 수도 있습니다.

#### <a name="static"></a>Static

정적 보기는 간단 하 고 신속 하 게 표시할 수 있어야 합니다.

![](notifications-images/notification-static.png "The static view")

#### <a name="dynamic"></a>동적

동적 보기는 더 많은 데이터를 표시 하 고 더 많은 상호 작용을 제공할 수 있습니다.

![](notifications-images/notification-dynamic.png "The dynamic view")

## <a name="generating-notifications"></a>알림 생성

알림은 원격 서버 ([Apple Push Notification Service](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)또는 APNS)에서 제공 되거나 iOS 앱에서 로컬로 생성 될 수 있습니다.

로컬 알림을 생성 하는 방법에 대 한 예제 및 작업 예제는 [WatchNotifications 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications) 을 보려면 [iOS 알림 연습](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) 을 참조 하세요.

로컬 알림에는 Apple Watch에 표시 되도록 설정 된 `AlertTitle` 있어야 합니다. `AlertTitle` 문자열이 단기 인터페이스에 표시 됩니다. `AlertTitle`와 `AlertBody` 모두 알림 목록에 표시 됩니다. `AlertBody`는 긴 모양 인터페이스에 표시 됩니다.

이 스크린샷에서는 알림 목록에 표시 되는 `AlertTitle` 및 긴 모양 인터페이스에 표시 되는 `AlertBody` ( [샘플 코드](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications)사용)를 보여 줍니다.

![](notifications-images/watch-notificationslist-sml.png "이 스크린샷은 알림 목록에 표시 되는 AlertTitle을 보여 줍니다.") ![](notifications-images/watch-notificationcontroller-sml.png "긴 모양 인터페이스에 표시 되는 AlertBody")

## <a name="testing-notifications"></a>테스트 알림

알림 (로컬 및 원격 모두)은 장치 에서만 제대로 테스트할 수 있지만 iOS 시뮬레이터의 **json** 파일을 사용 하 여 시뮬레이션할 수 있습니다.

### <a name="testing-on-apple-watch"></a>Apple Watch 테스트

Apple Watch에 대 한 알림을 테스트 하는 경우 [Apple 설명서](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) 에는 다음이 명시 되어 있습니다.

> 앱의 로컬 또는 원격 알림 중 하나가 사용자의 iPhone에 도착 하면 iOS는 iPhone 또는 Apple Watch에 해당 알림을 표시할지 여부를 결정 합니다.

이는 iOS에서 알림이 iPhone에 표시될지 아니면 시계에 표시될지를 결정 한다는 사실입니다. 알림이 수신 될 때 페어링된 iPhone이 활성화 되어 있으면 알림이 iPhone에 표시 되 고 감시로 라우팅되지 *않을* 수 있습니다.

알림이 보기에 표시 되는지 확인 하려면 iPhone 화면을 끄거나 (전원 단추를 한 번 누름) 절전 모드로 전환 합니다. 쌍을 이루는 Watch가 범위 내에 있고, 전원이 손목에서 마모 되는 경우, 해당 알림이 라우팅되고 표시 되 고 감시에 표시 됩니다 (미묘한).

### <a name="testing-on-the-ios-simulator"></a>IOS 시뮬레이터 테스트

IOS 시뮬레이터에서 알림 모드를 테스트할 때 테스트 JSON 페이로드를 제공 *해야* 합니다. Mac용 Visual Studio의 **사용자 지정 실행 인수** 창에서 경로를 설정 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac용 Visual Studio은 조사식 확장이 **시작 프로젝트로**설정 된 경우 추가 옵션을 표시 합니다.
조사식 확장 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **> 사용자 지정 매개 변수를 사용 하 여 실행**...을 선택 합니다.

[![](notifications-images/runwith-customparams-sml.png "Running with Custom Properties")](notifications-images/runwith-customparams.png#lightbox)

그러면 **WatchKit** 탭이 포함 된 **실행 인수** 창이 열립니다. **알림** 을 선택 하 고 JSON 페이로드를 제공한 다음 **실행** 을 눌러 시뮬레이터에서 조사식 앱을 시작 합니다.

[![](notifications-images/runwith-execargs-sml.png "Select Notification Payload Default")](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 테스트 알림 페이로드를 설정 하려면 조사식 확장을 마우스 오른쪽 단추로 클릭 하 여 **프로젝트 속성**을 편집 합니다. **디버그** 섹션으로 이동 하 여 목록에서 알림 json 파일을 선택 합니다. 프로젝트에 포함 된 모든 json 파일은 자동으로 나열 됩니다.

[![](notifications-images/runwith-execargs-sml-vs.png "Select a notifications JSON file")](notifications-images/runwith-execargs-vs.png#lightbox)

조사식 확장이 **시작 프로젝트인**경우 Visual Studio는 아래와 같이 추가 옵션을 표시 합니다. **알림 모드에서** 감시 앱을 시작 하는 **알림** 옵션 중 하나를 선택 합니다 (속성 창에서 선택한 JSON 파일 사용).

![](notifications-images/runwith-vs.png "The Device menu")

-----

기본 페이로드 JSON 파일을 사용 하 여 시뮬레이터에서 테스트할 때 기본 알림 컨트롤러는 다음과 같습니다.

![](notifications-images/notification-debug-sml.png "An example notification")

[명령줄](~/ios/watchos/troubleshooting.md#command_line) 을 사용 하 여 iOS 시뮬레이터를 시작할 수도 있습니다.

### <a name="example-notification-payload"></a>알림 페이로드 예

[Watch Kit 카탈로그](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) 샘플에는 예제 페이로드 Json 파일 **notificationpayload. json** (아래에 나열 되어 있습니다)이 있습니다.

```json
{
    "aps": {
        "alert": "Test message content",
        "title": "Optional title",
        "category": "myCategory"
        },

        "WatchKit Simulator Actions": [
        {
            "title": "First Button",
            "identifier": "firstButtonAction"
        }
        ],

        "customKey": "Use this file to define a testing payload for your notifications. The aps dictionary specifies the category, alert text and title. The WatchKit Simulator Actions array can provide info for one or more action buttons in addition to the standard Dismiss button. Any other top level keys are custom payload. If you have multiple such JSON files in your project, you'll be able to choose between them in when selecting to debug the notification interface of your Watch App."
    }
```

## <a name="related-links"></a>관련 링크

- [WatchNotifications (로컬 알림) (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications)
- [WatchKitCatalog (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple의 감시 키트 알림 문서](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
