---
title: 알림
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 1a681c2bda941d8fe015a8d4da8b99f4d85e441b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="notifications"></a>알림

포함 하는 iOS 앱이 지 원하는 경우 조사식 앱에서 알림을 받을 수 있습니다. 그러나 내장 된 알림 처리 하지 않으면 하므로 *필요* 아래에 설명 된 추가 알림 지원을 추가 하려면 알림 동작을 사용자 지정할 모양에 다음 읽기 하는 경우.

참조는 [iOS 알림](~/ios/platform/user-notifications/deprecated/index.md) 솔루션에 iOS 앱에 알림 지원 추가 대 한 자세한 내용은 문서.

## <a name="creating-notification-controllers"></a>알림 컨트롤러 만들기

스토리 보드는 특수 한 유형의 트리거하지 segue 알림 컨트롤러가지고 있어야 합니다. 새 끌어 오면 **알림 인터페이스 컨트롤러** 스토리 보드에 자동으로 미치게 될 연결 된 segue:

![](notifications-images/notification-storyboard1.png "연결 된 segue 사용 하 여 새 알림 인터페이스 컨트롤러")

알림을 segue 경우을 선택 합니다. 해당 속성을 편집할 수 있습니다.

![](notifications-images/notification-storyboard2.png "선택한 알림을 segue합니다")

컨트롤러를 사용자 지정한 후의 WatchKitCatalog에서이 예제에서와 같이 보일 수 있습니다.

![](notifications-images/notifications-segue.png "알림 속성")


두 가지 방법으로 알림:

- **짧은 모양을** -시스템에서 정의한 스크롤할 수 없는 정적 보기입니다.

- **장기 모양을** -스크롤 가능 하므로 사용자가 정의 된 사용자 지정 가능한 보기! 간단 하 고 정적 버전 및 더 복잡 한 동적 버전을 지정할 수 있습니다.

### <a name="short-look-notification-controller"></a>짧은 모양 알림 컨트롤러

앱 아이콘만, 응용 프로그램 이름 및 알림 제목 문자열 짧은 모양을 UI 구성 됩니다.

사용자는 알림을 무시 하지 않으면, 시스템 자세한 정보를 제공 하는 장기 모양의 알림을 자동으로 전환 됩니다.


### <a name="long-look-notification-controller"></a>장기 모양 알림 컨트롤러

운영 체제를 다양 한 요인에 따라 정적 또는 동적 보기를 표시할지 여부를 결정 합니다. 정적 인터페이스를 제공 하며, 필요에 따라 수 또한 알림에 대 한 동적 인터페이스를 포함 합니다.

#### <a name="static"></a>정적

정적 뷰를 간단 하 고 신속 하 게 표시 해야 합니다.

![](notifications-images/notification-static.png "정적 뷰")

#### <a name="dynamic"></a>동적

동적 뷰 더 많은 데이터를 표시할 수 있으며 더 많은 대화형 기능을 제공 합니다.

![](notifications-images/notification-dynamic.png "동적 뷰")


## <a name="generating-notifications"></a>알림 생성

원격 서버에서 알림을 가져올 수 있습니다 ([Apple 푸시 알림 서비스](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html), 또는 APNS) iOS 앱의 로컬에서 생성할 수 있습니다.

참조는 [iOS 알림 연습](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) 로컬 알림을 생성 하는 방법의 예 및 [WatchNotifications 샘플](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/) 작업 예제.

로컬 알림이 있어야는 `AlertTitle` Apple Watch-에 표시 되도록 설정 된 `AlertTitle` 문자열 짧은 모양을 인터페이스에 표시 됩니다. 둘 다는 `AlertTitle` 및 `AlertBody` 알림 목록에 표시 및 `AlertBody` 장기 모양을 인터페이스에 표시 됩니다.

이 스크린샷에서 `AlertTitle` 알림 목록에 표시 되 고 및 `AlertBody` 장기 모양을 인터페이스에 표시 (사용 하 여는 [샘플 코드](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)):

![](notifications-images/watch-notificationslist-sml.png "이 스크린샷은 알림 목록에 표시 되 고 AlertTitle") ![ ] (notifications-images/watch-notificationcontroller-sml.png "The AlertBody 장기 모양을 인터페이스에 표시")

## <a name="testing-notifications"></a>테스트 알림

(로컬 및 원격 모두)만 제대로에서 테스트할 수는 장치를 사용 하 여 있습니다 수 시뮬레이션할 수 있지만 한 **.json** 파일의 ios 시뮬레이터.

### <a name="testing-on-apple-watch"></a>Apple Watch 대 한 테스트

Apple Watch 대 한 알림을 테스트할 때 유의 해야 [Apple 설명서](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) 다음 상태:

> 응용 프로그램의 로컬 또는 원격 알림 중 하나는 사용자의 iPhone에 도착 하면 iOS를 Apple Watch 또는 iPhone에서 해당 알림을 표시할지 여부를 결정 합니다.

이 팩트 alluding은 알림을 시계 또는 iPhone에 나타날지 여부를 결정 하는 iOS입니다. 알림을 iPhone에 표시 될 가능성이 쌍을 이루는 iPhone 활성 상태인 경우 알림의 받을 때 및 *하지* 시계도 라우팅됩니다.

시계에 알림을 표시 되도록 (의 전원 단추를 한 번 누르면) iPhone 화면 해제 또는 절전 되기를 기다립니다. 쌍을 이루는 조사식 범위 내에 사용자 power에 있으며 프로그램 손목 착용 위치 되는, 알림을 있습니다 라우팅됩니다 하며 (함께 미묘한) 조사식에 표시 합니다.

### <a name="testing-on-the-ios-simulator"></a>IOS 시뮬레이터에 대 한 테스트

하면 *해야* iOS 시뮬레이터에서에서 알림 모드를 테스트할 때 테스트 JSON 페이로드를 제공 합니다. 경로 설정의 **사용자 지정 실행 인수가** Mac.에 대 한 Visual Studio의 창

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 용 visual Studio 조사식 확장으로 설정 된 경우 추가 옵션이 표시 됩니다는 **시작 프로젝트**합니다.
조사식 확장 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **실행으로 > 사용자 지정 매개 변수 중...** :
    
[![](notifications-images/runwith-customparams-sml.png "사용자 지정 속성을 가진 실행")](notifications-images/runwith-customparams.png#lightbox)
    
열립니다는 **실행 인수가** 창을 포함 하는 **WatchKit** 탭 합니다. 선택 **알림** JSON 페이로드를 제공 하 고 다음 키를 누릅니다 **Execute** 시뮬레이터에서 watch 앱을 시작 하려면:
    
[![](notifications-images/runwith-execargs-sml.png "알림 페이로드 기본값 선택")](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

편집 하려면 조사식 확장에서 Visual Studio를 마우스 오른쪽 단추로 누르고 테스트 알림 페이로드를 설정 하는 **프로젝트 속성**합니다. 이동 하 여 **디버그** 섹션 (자동으로 나열 하는 프로젝트에 포함 된 모든 JSON 파일) 목록에서 알림을 JSON 파일을 선택 하십시오.
    
[![](notifications-images/runwith-execargs-sml-vs.png "알림 JSON 파일을 선택 합니다.")](notifications-images/runwith-execargs-vs.png#lightbox)

조사식 확장명이 **시작 프로젝트**, 아래와 같이 Visual Studio 추가 옵션이 표시 됩니다. 중 하나를 선택는 **알림** 에서 watch 앱을 시작 하는 옵션을 **알림** (속성 창에서 선택한 JSON 파일을 사용 하 여) 모드:
    
![](notifications-images/runwith-vs.png "장치 메뉴에")

-----

기본 알림 컨트롤러 기본 페이로드 JSON 파일 시뮬레이터에 대 한 테스트 하는 경우 다음과 같이 보입니다.

![](notifications-images/notification-debug-sml.png "예제에서는 알림")

사용 하 여 이기도 [명령줄](~/ios/watchos/troubleshooting.md#command_line) 를 iOS 시뮬레이터를 시작 합니다.

### <a name="example-notification-payload"></a>예제 알림 페이로드

에 [조사식 키트 카탈로그](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) 있는 샘플은 JSON 파일의 예 페이로드 **NotificationPayload.json** (아래 참조).

```csharp
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

- [(로컬 알림) WatchNotifications (샘플)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)
- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Apple Watch 키트 알림 docs](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
