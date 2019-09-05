---
title: watchOS에서 Xamarin 알림
description: 이 문서에서는 Xamarin에서 watchOS 알림을 사용 하는 방법을 설명 합니다. 알림을 생성 하 고 테스트 알림을 만드는 알림 컨트롤러를 설명 합니다.
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 9ca50171e43ef98e5b4e5fbd7bd236f74d35da8f
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286773"
---
# <a name="watchos-notifications-in-xamarin"></a>watchOS에서 Xamarin 알림

포함 된 iOS 앱을 지 원하는 경우 watch 앱에서 알림을 받을 수 있습니다. 하지만 하지 있도록 기본 제공 알림 처리 방법이 *필요* 아래에 설명 된 추가 알림 지원을 추가 하려면 알림 동작을 사용자 지정 하려는 경우 모양 계속 읽어 보십시오.

참조 된 [iOS 알림](~/ios/platform/user-notifications/deprecated/index.md) 알림 지원을 iOS 앱 솔루션에 추가 하는 방법은 문서.

## <a name="creating-notification-controllers"></a>알림 컨트롤러 만들기

알림 컨트롤러는 스토리 보드 segue 이러한 트리거는 특수 한 종류의에 있습니다. 새 끌어 오면 **알림 인터페이스 컨트롤러** 스토리 보드에 자동으로 더 segue 연결:

![](notifications-images/notification-storyboard1.png "연결 된 segue 사용 하 여 새 알림 인터페이스 컨트롤러")

알림을 segue 경우을 선택 합니다. 해당 속성을 편집할 수 있습니다.

![](notifications-images/notification-storyboard2.png "알림을 선택한 segue")

컨트롤러를 정의한 후는 WatchKitCatalog에서이 예제와 같이 보일 수 있습니다.

![](notifications-images/notifications-segue.png "알림 속성")


알림의 다음과 같은 두 종류가 있습니다.

- 시스템에서 정의한 스크롤할 때 스크롤할 때 정적 뷰가 아닙니다.

- **장기 모양을** 스크롤할 수-사용자가 정의한 사용자 지정 가능한 보기! 간단 하 고 정적 버전 및 더 복잡 한 동적 버전을 지정할 수 있습니다.

### <a name="short-look-notification-controller"></a>짧은 표시 알림 컨트롤러

앱 아이콘만, 앱 이름 및 알림 제목 문자열의 짧은 모양을 UI 구성 됩니다.

사용자는 알림을 무시 하지 않으면, 시스템 자세한 정보를 제공 하는 장기 모양의 알림을 자동으로 전환 합니다.


### <a name="long-look-notification-controller"></a>알림 컨트롤러 장기-확인

OS를 다양 한 요인 기준으로 정적 또는 동적 보기를 표시할지 여부를 결정 합니다. 정적 인터페이스를 제공 하며, 필요에 따라 알림에 대 한 동적 인터페이스를 포함 합니다.

#### <a name="static"></a>정적

정적 뷰는 간단 하 고 신속 하 게 표시 해야 합니다.

![](notifications-images/notification-static.png "정적 뷰")

#### <a name="dynamic"></a>동적

동적 뷰는 더 많은 데이터를 표시 하 고 더 많은 대화형 기능을 제공할 수 있습니다.

![](notifications-images/notification-dynamic.png "동적 뷰")


## <a name="generating-notifications"></a>알림 생성

원격 서버에서 알림을 가져올 수 있습니다 ([Apple 푸시 알림 서비스](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html), 또는 APNS) 또는 iOS 앱에서 로컬로 생성 될 수 있습니다.

참조 된 [iOS 알림 연습](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) 로컬 알림을 생성 하는 방법의 예 및 [WatchNotifications 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications) 작업 예제입니다.

로컬 알림 있어야 합니다 `AlertTitle` Apple Watch-에 표시할 설정를 `AlertTitle` 문자열 짧은 모양을 인터페이스에 표시 됩니다. 모두를 `AlertTitle` 하 고 `AlertBody` 알림 목록에 표시 됩니다 및 `AlertBody` 장기 모양을 인터페이스에 표시 됩니다.

보여 주는이 스크린샷 합니다 `AlertTitle` 알림 목록에 표시 되 및 `AlertBody` 장기 모양을 인터페이스에 표시 (사용 하 여 합니다 [샘플 코드](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications)):

![](notifications-images/watch-notificationslist-sml.png "이 스크린샷은 알림 목록에 표시 되 고 AlertTitle") ![](notifications-images/watch-notificationcontroller-sml.png "The AlertBody 장기 모양을 인터페이스에 표시")

## <a name="testing-notifications"></a>테스트 알림

(로컬 및 원격)만 테스트할 수 있습니다 제대로 장치에를 사용 하 여 시뮬레이션할 수 있지만 한 **.json** iOS 시뮬레이터에에서는 파일입니다.

### <a name="testing-on-apple-watch"></a>Apple Watch 테스트

Apple Watch 알림을 테스트할 때 유의 [Apple 설명서](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) 다음 상태:

> 앱의 로컬 또는 원격 알림 중 하나가 사용자의 iPhone에 도착 하면 iOS Apple Watch 또는 iPhone에 해당 알림을 표시할지 여부를 결정 합니다.

이 사실을 alluding은 iOS는 알림을 iPhone에서 또는 시계에 나타날지 여부를 결정 합니다. 쌍을 이루는 iPhone 활성 상태인 경우 알림의 받을 때 알림을 iPhone에 표시 될 가능성이 및 *되지* 시계에 라우팅됩니다.

시계에 알림을 표시 하도록 (전원 단추를 한 번 누르면) 하는 iPhone 화면 해제 또는 절전 하도록 합니다. 쌍을 이루는 시계는 범위 내에 있고 전원이 손목이에서 착용는, 알림을 있습니다 라우팅됩니다 하 고 (함께 미세한) 시계에 표시 합니다.

### <a name="testing-on-the-ios-simulator"></a>IOS 시뮬레이터에서 테스트

있습니다 *해야* iOS 시뮬레이터에서에서 알림 모드를 테스트할 때 테스트 JSON 페이로드를 제공 합니다. 경로 설정 합니다 **사용자 지정 실행 인수** mac 용 Visual Studio의 창

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Watch 확장으로 설정 된 경우 Mac 용 visual Studio는 추가 옵션을 표시 합니다 **시작 프로젝트**합니다.
조사식 확장 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **사용 하 여 실행 > 매개 변수 사용자 지정 하는 중...** :

[![](notifications-images/runwith-customparams-sml.png "사용자 지정 속성을 사용 하 여 실행")](notifications-images/runwith-customparams.png#lightbox)

열립니다는 **실행 인수** 포함 하는 창을 **WatchKit** 탭 합니다. 선택 **알림을** JSON 페이로드를 제공 하 고 다음 키를 누릅니다 **Execute** 시뮬레이터에서 watch 앱을 시작 하려면:

[![](notifications-images/runwith-execargs-sml.png "알림 페이로드 기본값 선택")](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio를 마우스 오른쪽 단추로 클릭에서 편집 하려면 조사식 확장에서 테스트 알림 페이로드를 설정 하는 **프로젝트 속성**합니다. 로 이동 합니다 **디버그** 섹션 및 목록 (이 자동으로 표시 된 프로젝트에 포함 된 모든 JSON 파일)에서 알림을 JSON 파일을 선택 합니다.

[![](notifications-images/runwith-execargs-sml-vs.png "알림 JSON 파일을 선택 합니다.")](notifications-images/runwith-execargs-vs.png#lightbox)

조사식 확장의 경우는 **시작 프로젝트**, 아래와 같이 Visual Studio 추가 옵션이 표시 됩니다. 중 하나를 선택 합니다 **알림을** watch 앱 시작 하는 옵션 **알림** (속성 창에서 선택 된 JSON 파일을 사용 하 여) 모드:

![](notifications-images/runwith-vs.png "장치 메뉴")

-----

기본 알림 컨트롤러 기본 페이로드 JSON 파일을 사용 하 여 시뮬레이터에서 테스트 하는 경우 다음과 같이 나타납니다.

![](notifications-images/notification-debug-sml.png "예제 알림")

사용 하 여도 가능 합니다 [명령줄](~/ios/watchos/troubleshooting.md#command_line) iOS 시뮬레이터를 시작 하려면.

### <a name="example-notification-payload"></a>알림 페이로드 예제

에 [조사식 키트 카탈로그](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) 있습니다 샘플 페이로드 JSON 파일의 예는 **NotificationPayload.json** (아래 참조).

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

- [(로컬 알림) WatchNotifications (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications)
- [WatchKitCatalog (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple Watch 키트 알림을 docs](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
