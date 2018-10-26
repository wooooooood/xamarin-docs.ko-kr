---
title: Xamarin.iOS에서 향상 된 사용자 알림
description: 이 문서에서는 iOS 10에에서 도입 된 사용자 알림 프레임 워크를 설명 합니다. 로컬 알림, 원격 알림, 알림 관리, 알림 작업 및 자세히 설명합니다.
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: d1b1a59b432315532844f8fca3b613ff3392a7b5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108251"
---
# <a name="enhanced-user-notifications-in-xamarinios"></a>Xamarin.iOS에서 향상 된 사용자 알림

새 ios 10 프레임 워크를 제공 하 고 로컬 및 원격 알림이 처리에 대 한 허용 사용자 알림입니다. 앱 또는 앱 확장은이 프레임 워크를 사용 하 여, 위치와 같은 조건 집합 또는 시간을 지정 하 여 로컬 알림 배달을 예약할 수 있습니다.

## <a name="about-user-notifications"></a>사용자 알림 정보

위에서 설명한 대로 새 사용자 알림 프레임 워크를 제공 하 고 로컬 및 원격 알림이 처리 허용 합니다. 앱 또는 앱 확장은이 프레임 워크를 사용 하 여, 위치와 같은 조건 집합 또는 시간을 지정 하 여 로컬 알림 배달을 예약할 수 있습니다.

또한 앱 또는 확장 수 수신 (및 잠재적으로 수정할) 로컬 및 원격 알림이 사용자의 iOS 장치에 배달 하는 대로 합니다.

새 사용자 알림 UI 프레임 워크에는 앱 또는 앱 확장 사용자에 게 표시 될 때 로컬 및 원격 알림의 모양을 사용자 지정할 수 있습니다.

이 프레임 워크는 앱 사용자에 게 알림을 배달할 수 있는 다음과 같은 방법으로 제공 합니다.

- **시각적 경고가** -알림이 있는 배너로 화면 맨 위에서 세분화 합니다.
- **사운드 및 진동** -알림과 연결할 수 있습니다.
- **앱 아이콘 배지** -앱의 아이콘에서 새 콘텐츠를 읽지 않은 전자 메일 메시지의 수와 같은 사용할 수 있는지를 보여 주는 배지를 표시 하는 위치입니다.

또한 사용자의 현재 컨텍스트에 따라 여러 가지는 알림이 표시 됩니다.

- 장치 잠금 해제 된 경우 알림을 롤 다운 화면 맨 위에서 배너로 합니다.
- 장치가 잠겨 있는 경우 사용자의 잠금 화면에서 알림이 표시 됩니다.
- 사용자가 알림의 놓친 경우 알림 센터를 열 수 있으며 있는 모든 사용 가능한 대기 알림을 봅니다.

Xamarin.iOS 앱에 두 가지 유형의 사용자 알림을 보낼 수 있습니다.

- **로컬 알림** -이러한 사용자가 장치에 로컬로 설치 하는 앱에서 전송 됩니다.
- **원격 알림** -원격에서 전송 된 사용자에 게 표시 하는 서버와 하나 또는 앱의 내용의 백그라운드 업데이트를 트리거합니다.

### <a name="about-local-notifications"></a>로컬 알림 정보

IOS 앱을 보낼 수 있는 로컬 알림을 다음 기능 및 특성

- 사용자의 장치에서 로컬로 사용 되는 앱에서 전송 됩니다. 
- 구성할 수 있습니다 시간 또는 위치를 사용 하도록 트리거를 기반 합니다. 
- 앱 사용자의 장치를 사용 하 여 알림을 예약 하 고 트리거 조건이 충족 되 면 표시 됩니다.
- 사용자가 알림을 앱 콜백을 받습니다.

다음과 같은 몇 가지 예가 로컬 알림

- 일정 알림
- 미리 알림 경고
- 위치 인식 트리거

자세한 내용은 Apple의를 참조 하세요 [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) 설명서.

### <a name="about-remote-notifications"></a>원격 알림 정보

IOS 앱을 보낼 수 있는 원격 알림 기능 및 특성은:

- 앱에 통신 하는 서버 쪽 구성 요소입니다.
- Apple 푸시 알림 서비스 (APNs) 개발자의 클라우드 기반 서버에서 원격 알림을 전달 하는 최상의 사용자의 장치에 전송 됩니다.
- 앱 원격 알림을 수신 하는 경우 사용자에 게 표시 됩니다.
- 알림 메시지와 상호 작용 하는 사용자, 앱 콜백을 받습니다.

다음과 같은 몇 가지 예가 원격 알림

- 속보 알림
- 스포츠 업데이트
- 인스턴트 메시징 메시지

IOS 앱을 사용할 수 있는 두 가지 유형의 원격 알림은:

- **사용자 용** -이러한 장치에서 사용자에 게 표시 됩니다.
- **자동 업데이트** -이 백그라운드에서 iOS 앱의 콘텐츠를 업데이트 하는 메커니즘을 제공 합니다. 자동 업데이트를 수신 되 면 앱 최신 내용을 제거 서버 풀에 연결할 수 있습니다.

자세한 내용은 Apple의를 참조 하세요 [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) 설명서.

### <a name="about-the-existing-notifications-api"></a>기존 알림 API에 대 한

이전 iOS 10, iOS 앱은 사용 하 여 `UIApplication` 시스템을 사용 하 여 알림을 등록 하 고 어떻게 해당 알림을 트리거하도록 (하거나 하 여 시간 위치)을 예약 합니다.

개발자는 기존 알림 API 사용 하 여 작업할 때 발생할 수 있는 몇 가지 문제는 있습니다.

- 로컬 또는 원격 알림 코드 중복을 일으킬 수에 필요한 다른 콜백이 있었습니다.
- 시스템을 사용 하 여 예약한 후 앱 알림 컨트롤을 제한 했습니다.
- Apple의 기존 플랫폼의 모든 서로 다른 수준으로 지원 했습니다.

### <a name="about-the-new-user-notification-framework"></a>새 사용자 알림 프레임 워크에 대 한

Apple iOS 10 사용 하 여 새 사용자 알림 프레임 워크, 기존 대체 하는 도입 `UIApplication` 메서드 위에서 설명한 것입니다.

다음 사용자 알림 프레임 워크를 제공합니다.

- 쉽게 코드를 이식 하는 기존 프레임 워크에서 이전 메서드를 사용 하 여 기능 패리티를 포함 하는 친숙 한 API입니다.
- 다양 한 알림을 사용자에 게 보낼 수 있도록 확장 된 콘텐츠 옵션 집합을 포함 되어 있습니다.
- 로컬 및 원격 알림을 모두 동일한 코드 및 콜백을 처리할 수 있습니다.
- 알림을 사용 하 여 상호 작용할 때 앱에 전송 되는 콜백을 처리 하는 프로세스를 간소화 합니다.
- 보류 중 및 배달 알림 제거 하거나 업데이트 알림 등의 향상 된 관리 합니다.
- 앱에서 알림 표시를 수행 하는 기능을 추가 합니다.
- 예약 및 앱 확장 내에서 알림을 처리 하는 기능을 추가 합니다.
- 알림 자체에 대 한 새 확장 지점을 추가합니다. 

Apple에서는 포함 플랫폼의 여러 통합된 알림을 API를 제공 하는 새로운 사용자 알림 프레임 워크: 

- **iOS** -완벽 하 게 관리 하 고 알림 예약을 지원 합니다.
- **tvOS** -로컬 및 원격 알림이 앱 아이콘 배지 하는 기능을 추가 합니다.
- **watchOS** -해당 Apple Watch 사용자의 쌍으로 연결 된 iOS 장치에서 알림을 전달 하는 기능을 추가 하 고 watch 앱 watch 자체에서 직접 로컬 알림을 수행 하는 기능을 제공 합니다.

자세한 내용은 Apple의를 참조 하세요 [UserNotifications 프레임 워크 참조](https://developer.apple.com/reference/usernotifications) 하 고 [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui) 설명서.

## <a name="preparing-for-notification-delivery"></a>알림 배달에 대 한 준비

전에 iOS 앱에 알림을 보낼 수 시스템을 사용 하 여 앱을 등록 해야 하는 사용자 및 앱 보내기 전에 권한을 명시적으로 요청 해야 알림을 사용자에 게 중단 이기 때문에 있습니다.

알림 요청을 앱에 대 한 사용자를 승인할 수 있는 세 개의 다른 수준에는

- 배너를 표시합니다.
- 사운드 알림을 생성 합니다.
- 앱 아이콘을 배지입니다.

또한 이러한 승인 수준 요청 하 고 로컬 및 원격 알림을 설정 합니다.

다음 코드를 추가 하 여 앱이 시작 되는 즉시 notification 권한을 요청 해야 합니다 `FinishedLaunching` 메서드를 `AppDelegate` 원하는 알림 유형을 설정 하 고 (`UNAuthorizationOptions`):

```csharp
using UserNotifications;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    return true;
}
```

또한 사용자 언제 든 지 앱에 대 한 알림 권한을 변경할 항상 수는 **설정을** 장치의 앱입니다. 앱은 사용자의 요청 된 알림에 권한에 대 한 다음 코드를 사용 하 여 알림을 제공 하기 전에 확인 해야 합니다.

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
}); 
``` 

### <a name="configuring-the-remote-notifications-environment"></a>원격 알림 환경 구성

새 ios 10, 개발자 개발 또는 프로덕션으로 푸시 알림 어떤 환경에서 실행 중인 OS에 게 알려야 합니다. 이 정보를 제공 하는 오류는 다음과 유사한 알림이 iTune 앱 스토어에 제출 하는 경우 거부 됩니다. 앱에서 발생할 수 있습니다.

> 누락 된 푸시 알림 자격-앱 Apple 푸시 알림 서비스에 대 한 API를 포함 하지만 `aps-environment` 자격 응용 프로그램의 서명에서 누락 되었습니다.

필요한 자격을 제공 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 두 번 클릭 합니다 `Entitlements.plist` 파일을 **Solution Pad** 을 편집용으로 엽니다.
2. 으로 전환 합니다 **원본** 보기: 

    [![](enhanced-user-notifications-images/setup01.png "소스 보기")](enhanced-user-notifications-images/setup01.png#lightbox)
3. 클릭 합니다 **+** 단추를 새 키를 추가 합니다.
4. 입력 `aps-environment` 에 대 한는 **속성**를 유지 합니다 **형식** 으로 `String` 하나를 입력 하 고 `development` 또는 `production` 에 대 한를 **값**: 

    [![](enhanced-user-notifications-images/setup02.png "Ap 환경 속성")](enhanced-user-notifications-images/setup02.png#lightbox)
5. 파일의 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 두 번 클릭 합니다 `Entitlements.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다.
3. 클릭 합니다 **+** 단추를 새 키를 추가 합니다.
4. 입력 `aps-environment` 에 대 한는 **속성**를 유지 합니다 **형식** 으로 `String` 하나를 입력 하 고 `development` 또는 `production` 에 대 한를 **값**: 

    [![](enhanced-user-notifications-images/setup02w.png "Ap 환경 속성")](enhanced-user-notifications-images/setup02.png#lightbox)
5. 파일의 변경 내용을 저장합니다.

-----

### <a name="registering-for-remote-notifications"></a>원격 알림 등록

앱 전송 되며 원격 알림을 받기, 하는 경우 계속 해야 할 _토큰 등록_ 하 여 기존 `UIApplication` API. 이 등록 장치 앱에 보낼 필요한 토큰을 생성 하는 APNs를 라이브 네트워크 연결 액세스에 필요 합니다. 앱이 원격 알림을 등록 하는 개발자의 서버 쪽 앱이 토큰을 전달 해야 합니다.

[![](enhanced-user-notifications-images/token01.png "토큰 등록 개요")](enhanced-user-notifications-images/token01.png#lightbox)

다음 코드를 사용 하 여 필수 등록을 초기화 합니다.

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

개발자의 서버 쪽 앱에 전송 하는 토큰 알림 페이로드는 가져오기의 일부를 보낼 때 서버에서 APNs 원격 알림을 보낼 때 포함 되도록 해야 합니다.

[![](enhanced-user-notifications-images/token02.png "알림 페이로드가의 일부로 포함 된 토큰")](enhanced-user-notifications-images/token02.png#lightbox)

토큰을 열거나 알림에 응답 하는 데 앱 알림과 함께 연결 하는 키로 작동 합니다.

자세한 내용은 Apple의를 참조 하세요 [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) 설명서.

## <a name="notification-delivery"></a>알림 배달

앱을 사용 하 여 완전히 등록 하 고에서 요청 된 필요한 사용 권한이 부여 된 사용자가 앱이 이제 알림을 보내고 받도록 준비가 합니다. 

### <a name="providing-notification-content"></a>알림 콘텐츠를 제공합니다.

새 ios 10, 모든 알림을 모두 포함을 **제목** 하 고 **부제목** 는 항상 표시 됩니다는 **본문** 알림 콘텐츠. 또한 새로운 기능은 추가 **미디어 첨부 파일** 알림 콘텐츠를 합니다.

로컬 알림 콘텐츠를 만들려면 다음 코드를 사용 합니다.

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

원격 알림 프로세스는 유사 합니다.

```csharp
{
    "aps":{
        "alert":{
            "title":"Notification Title",
            "subtitle":"Notification Subtitle",
            "body":"This is the message body of the notification."
        },
        "badge":1
    }
}
```

### <a name="scheduling-when-a-notification-is-sent"></a>전송 되는 경우는 알림 예약

만든 알림 콘텐츠를 사용 하 여 앱을 설정 하 여 알림을 사용자에 게 표시 하는 경우 예약 해야는 *트리거*합니다. iOS 10에는 네 개의 다른 트리거 유형을 제공합니다.

- **푸시 알림** -Remote Notifications 단독으로 사용 하 고 APNs 보내는 알림 장치에서 실행 되는 앱을 패키지 하는 경우 트리거됩니다.
- **시간 간격** -이제 및 일부 이후에 종료 간격 시작 시간을 예약 로컬 알림을 허용 합니다. 예를 들면 `var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`과 같습니다.
- **날짜** -특정 날짜 및 시간을 예약 하도록 로컬 알림 허용 합니다.
- **위치 기반** -iOS 장치 또는 특정 지리적 위치를 벗어나지 들어가거나 탐지 모든 Bluetooth 장치에 지정 된 근접 하는 경우 예약 로컬 알림을 허용 합니다.

앱을 호출 해야 로컬 알림을 준비 되 면 합니다 `Add` 메서드는 `UNUserNotificationCenter` 사용자에 게 해당 디스플레이 예약 하는 개체입니다. 원격 알림을 서버 쪽 앱 알림 페이로드를 보냅니다 APNs, 사용자의 장치에는 패킷을 보냅니다.

모든 부분이 함께 가져오는 샘플 로컬 알림을와 같습니다.

```csharp
using UserNotifications;
...

var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);

var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

## <a name="handling-foreground-app-notifications"></a>전경 앱 알림 처리

새 ios 10, 앱 알림을 처리할 수 다르게 포그라운드에서 되 고 알림이 트리거됩니다. 제공 하 여는 `UNUserNotificationCenterDelegate` 구현 및는 `UserNotificationCenter` 메서드, 앱 알림을 표시 하는 것에 대 한 책임 걸릴 수 있습니다. 예를 들어:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        #region Constructors
        public UserNotificationCenterDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void WillPresentNotification (UNUserNotificationCenter center, UNNotification notification, Action<UNNotificationPresentationOptions> completionHandler)
        {
            // Do something with the notification
            Console.WriteLine ("Active Notification: {0}", notification);

            // Tell system to display the notification anyway or use
            // `None` to say we have handled the display locally.
            completionHandler (UNNotificationPresentationOptions.Alert);
        }
        #endregion
    }
}
```

이 코드는 단순히 내용을 작성 하는 `UNNotification` 알림에 대 한 표준 경고를 표시 하려면 응용 프로그램 출력 및 시스템에 요청 합니다. 

응용 프로그램을 전경에 했을 때 알림 자체를 표시 하지 시스템 기본값을 사용 하 여, 전달 하려는 경우 `None` 완료 처리기에 있습니다. 예제:

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

이 코드를 사용 하 여 열을 `AppDelegate.cs` 편집을 위해 파일을 변경 합니다 `FinishedLaunching` 검색할 메서드는 다음과 같은:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    // Watch for notifications while the app is active
    UNUserNotificationCenter.Current.Delegate = new UserNotificationCenterDelegate ();

    return true;
}
```

이 코드는 사용자 지정 연결 된 `UNUserNotificationCenterDelegate` 위에서 현재 `UNUserNotificationCenter` 활성 상태인 동안 앱 알림을 처리할 수 있도록 하 고 포그라운드에 있습니다.

## <a name="notification-management"></a>알림 관리

새 ios 10, 알림 관리 보류 중 및 배달 알림 액세스를 제공 하며, 제거, 업데이트 하거나 이러한 알림을 승격 하는 기능을 추가 합니다.

알림 관리는 중요 한 부분은 합니다 _요청 식별자_ 만들어지고 시스템을 사용 하 여 예약 된 경우 알림을에 할당 된 합니다. 새 통해 할당 된이 원격 알림을 `apps-collapse-id` HTTP 요청 헤더 필드입니다.

요청 식별자 알림을 선택 하면 앱에 알림 관리를 수행 하는 데 사용 됩니다.

### <a name="removing-notifications"></a>알림 제거

보류 중인 알림이 시스템에서 제거 하려면 다음 코드를 사용 합니다.

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

이미 배달된 알림을 제거 하려면 다음 코드를 사용 합니다.

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>기존 알림 업데이트

기존 알림을 업데이트 하려면 (예: 새 트리거 시간) 수정 원하는 매개 변수를 사용 하 여 새 알림을 만들려면 하 고 수정 해야 하는 알림으로 동일한 요청 식별자를 사용 하 여 시스템에 추가 합니다. 예제:


```csharp
using UserNotifications;
...

// Rebuild notification
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

// New trigger time
var trigger = UNTimeIntervalNotificationTrigger.CreateTrigger (10, false);

// ID of Notification to be updated
var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

// Add to system to modify existing Notification
UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

이미 전달 된 알림에 대 한 기존 알림 가져오기 업데이트를 이미 읽은 것은 사용자가 홈 및 잠금 화면에서 알림 센터에서 목록 맨 승격 합니다.

## <a name="working-with-notification-actions"></a>알림 작업 사용

Ios 10에서 사용자에 게 제공 되는 정적 되지 알림과 (에서 사용자 지정 작업에 기본 제공)으로 사용자 상호 작용할 수 있는 몇 가지 방법을 제공 합니다.

IOS 앱에 응답할 수 있는 작업에는 다음과 같은 세 종류가 있습니다.

- **기본 작업** -사용자가 앱을 열고 지정 된 알림의 세부 정보를 표시 하는 알림을 탭 하는 경우입니다.
- **사용자 지정 작업** -iOS 8에에서 추가 된이 고 사용자가 앱을 시작 하지 않고도 알림에서 직접 사용자 지정 태스크를 수행할 수 있는 빠른 방법을 제공 합니다. 사용자 지정 가능한 타이틀을 사용 하 여 단추의 목록 또는 텍스트 입력된 필드 (여기서 앱 적은 양의 요청을 처리 하는 시간 지정) 배경이 나 전경에서 실행할 수 있습니다. (여기서는 앱이 시작 fu 포그라운드에서 표시 될 수 있습니다. lfill 요청)입니다. 사용자 지정 작업은 iOS 및 watchOS에서 사용할 수 있습니다.
- **작업 해제** -이 작업 사용자 지정된 알림을 해제 하는 경우 앱에 전송 됩니다.

### <a name="creating-custom-actions"></a>사용자 지정 동작 만들기

를 만들고 사용자 지정 작업을 시스템에 등록 하려면 다음 코드를 사용 합니다.

```csharp
// Create action
var actionID = "reply";
var title = "Reply";
var action = UNNotificationAction.FromIdentifier (actionID, title, UNNotificationActionOptions.None);

// Create category
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);
    
// Register category
var categories = new UNNotificationCategory [] { category };
UNUserNotificationCenter.Current.SetNotificationCategories (new NSSet<UNNotificationCategory>(categories)); 
```

새로 만들 때 `UNNotificationAction`, 고유 ID 및 단추에 나타나는 제목을 할당 됩니다. 하지만 작업의 동작 (예를 들어 설정이 포그라운드 작업 수)를 조정 하려면 옵션을 제공할 수 있습니다 동작 기본적으로 백그라운드 작업으로 생성 됩니다.

만든 작업의 각 범주와 연결 되도록 해야 합니다. 새로 만들 때 `UNNotificationCategory`고유 ID가 할당, 수행할 수 있습니다 작업 목록, 범주에 작업의 의도 대 한 자세한 내용 및 범주의 동작을 제어 하는 몇 가지 옵션을 제공 하는 의도 Id 목록입니다.

마지막으로 사용 하 여 시스템 등록 된 모든 범주를 `SetNotificationCategories` 메서드.

### <a name="presenting-custom-actions"></a>사용자 지정 작업을 제공합니다.

사용자 지정 작업 및 범주 집합을 생성 및 시스템을 사용 하 여 등록 된, 일단 로컬 또는 원격 알림을 표시할 수 있습니다.

원격 알림 설정는 `category` 위에서 만든 범주 중 하 나와 일치 하는 원격 알림 페이로드에서 합니다. 예를 들어:

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

로컬 알림을 설정 합니다 `CategoryIdentifier` 의 속성을 `UNMutableNotificationContent` 개체. 예를 들어:

```csharp
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
content.CategoryIdentifier = "message";
...
```

다시이 ID는 위에서 만든 범주 중 하 나와 일치 해야 합니다.

### <a name="handling-dismiss-actions"></a>처리 작업을 해제

위에서 설명한 대로 사용자가 알림의 닫을 때 해제 작업을 앱에 보낼 수 있습니다. 표준 동작 하지 이므로 옵션을 범주를 만들 때 설정 해야 합니다. 예를 들어:

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>처리 작업 응답

사용자 지정 작업 및 위에서 만든 범주를 사용 하 여 사용자 상호 작용을 앱에서 요청 된 작업을 수행 해야 합니다. 이렇게 함으로써를 `UNUserNotificationCenterDelegate` 구현 및는 `UserNotificationCenter` 메서드. 예를 들어:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        ...

        #region Override Methods
        public override void DidReceiveNotificationResponse (UNUserNotificationCenter center, UNNotificationResponse response, Action completionHandler)
        {
            // Take action based on Action ID
            switch (response.ActionIdentifier) {
            case "reply":
                // Do something
                break;
            default:
                // Take action based on identifier
                if (response.IsDefaultAction) {
                    // Handle default action...
                } else if (response.IsDismissAction) {
                    // Handle dismiss action
                }
                break;
            }

            // Inform caller it has been handled
            completionHandler();
        }
        #endregion
    }
}
```

전달 된에서 `UNNotificationResponse` 클래스에는 `ActionIdentifier` 속성 중 하나를 수행할 수 있는 기본 작업 또는 해제 작업 여야 합니다. 사용 하 여 `response.Notification.Request.Identifier` 사용자 지정 작업에 대 한 테스트 합니다.

`UserText` 속성에는 모든 사용자 텍스트 입력의 값을 보유 합니다. `Notification` 속성 트리거를 사용 하 여 요청을 포함 하는 원래 알림 및 알림 콘텐츠를 보유 합니다. 앱은 경우 된 로컬 또는 원격 알림 트리거 형식에 따라 결정할 수 있습니다.

> [!NOTE]
> iOS 12는 런타임에 해당 실행 단추를 수정 하려면 사용자 지정 알림을 UI 수 있도록 만듭니다. 자세한 내용은 잠시 살펴 합니다 [동적 알림 작업 단추](~/ios/platform/introduction-to-ios12/notifications/dynamic-actions.md) 설명서.

## <a name="working-with-service-extensions"></a>서비스 확장 사용

원격 알림 작업할 때 _서비스 확장_ 알림 페이로드 내에서 종단 간 암호화를 사용 하도록 설정 하는 방법을 제공 합니다. 서비스 확장은 주로 확대 또는 사용자에 게 표시 되기 전에 알림의 표시 되는 콘텐츠의 대체를 사용 하 여 백그라운드에서 실행 되는 사용자 인터페이스가 아닌 확장 (iOS 10에서에서 사용 가능). 

[![](enhanced-user-notifications-images/extension01.png "서비스 확장 개요")](enhanced-user-notifications-images/extension01.png#lightbox)

서비스 확장 신속 하 게 실행 하 고 시스템에서 실행 하는 짧은 기간에만 부여 됩니다. 서비스 확장에 실패 하면 할당 된 시간 내에 작업을 완료 하는 대체 (fallback) 메서드를 호출 합니다. 대체 실패 하면 원래 알림 콘텐츠 표시할 사용자에 게 합니다.

서비스 확장의 몇 가지 잠재적인 용도 다음과 같습니다.

- 원격 알림 콘텐츠의 종단 간 암호화를 제공합니다.
- 보강 하 고 원격 알림을 첨부 파일을 추가 합니다.

### <a name="implementing-a-service-extension"></a>서비스 확장 프로그램 구현

Xamarin.iOS 앱에 서비스 확장을 구현 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac 용 Visual Studio에서 앱의 솔루션을 열으십시오
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭 합니다 **Solution Pad** 선택한 **추가** > **새 프로젝트 추가**합니다.
3. 선택 **iOS** > **확장** > **알림 서비스 확장** 을 클릭 합니다 **다음** 단추: 

    [![](enhanced-user-notifications-images/extension02.png "알림 서비스 확장 선택")](enhanced-user-notifications-images/extension02.png#lightbox)
4. 입력을 **이름을** 확장을 클릭 합니다 **다음** 단추: 

    [![](enhanced-user-notifications-images/extension03.png "확장의 이름을 입력 합니다.")](enhanced-user-notifications-images/extension03.png#lightbox)
5. 조정 합니다 **프로젝트 이름** 및/또는 **솔루션 이름** 필요 하 고 클릭 합니다 **만들기** 단추: 

    [![](enhanced-user-notifications-images/extension04.png "프로젝트 이름 및/또는 솔루션 이름을 조정합니다")](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio에서 앱의 솔루션을 엽니다.
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** 선택한 **추가 > 새 프로젝트...** .
3. 선택 **시각적 C# > iOS 확장 > 알림 서비스 확장**:

    [![](enhanced-user-notifications-images/extension01.w157-sml.png "알림 서비스 확장 선택")](enhanced-user-notifications-images/extension01.w157.png#lightbox)
4. 입력을 **이름을** 확장을 클릭 합니다 **확인** 단추입니다.

-----

> [!IMPORTANT]
> 서비스 확장에 대 한 번들 식별자에는 주 응용 프로그램의 번들 식별자와 일치 해야 `.appnameserviceextension` 끝에 추가 합니다. 예를 들어, 기본 앱의 번들 식별자에 있으면 `com.xamarin.monkeynotify`, 서비스 확장의 번들 식별자 있어야 `com.xamarin.monkeynotify.monkeynotifyserviceextension`합니다. 확장 솔루션에 추가 되 면 자동으로 설정 해야 합니다. 

필요한 기능을 제공 하도록 수정 해야 하는 알림 서비스 확장에서 기본 클래스를 하나 있습니다. 예를 들어:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyChatServiceExtension
{
    [Register ("NotificationService")]
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Computed Properties
        public Action<UNNotificationContent> ContentHandler { get; set; }
        public UNMutableNotificationContent BestAttemptContent { get; set; }
        #endregion

        #region Constructors
        protected NotificationService (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            ContentHandler = contentHandler;
            BestAttemptContent = (UNMutableNotificationContent)request.Content.MutableCopy ();

            // Modify the notification content here...
            BestAttemptContent.Title = $"{BestAttemptContent.Title}[modified]";

            ContentHandler (BestAttemptContent);
        }

        public override void TimeWillExpire ()
        {
            // Called just before the extension will be terminated by the system.
            // Use this as an opportunity to deliver your "best attempt" at modified content, otherwise the original push payload will be used.

            ContentHandler (BestAttemptContent);
        }
        #endregion
    }
}
```

첫 번째 메서드인 `DidReceiveNotificationRequest`, 알림 식별자 뿐만 아니라 알림 콘텐츠를 통해 전달 될는 `request` 개체입니다. 전달 된에서 `contentHandler` 사용자에 게 알림 표시를 호출 해야 합니다.

두 번째 `TimeWillExpire`, 시간은 약 요청을 처리 하는 데 서비스 확장에 대 한 만료 직전 호출 됩니다. 서비스 확장을 호출 하지 못한 경우는 `contentHandler` 할당 된 시간 내에 원래 내용이 사용자에 게 표시 됩니다.

### <a name="triggering-a-service-extension"></a>서비스 확장을 트리거

서비스 확장을 만들고 앱을 사용 하 여 배달를 사용 하 여 장치에 전송 된 원격 알림 페이로드를 수정 하 여 트리거할 수 있습니다. 예를 들어:

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

새 `mutable-content` 키 지정 서비스 확장을 원격 알림 콘텐츠 업데이트를 시작 해야 합니다. `encrypted-content` 키 서비스 확장을 사용자에 게 프레젠테이션하기 전에 해독할 수 있는 암호화 된 데이터를 보유 합니다.

다음 예제에서는 서비스 확장을 살펴보십시오.

```csharp
using UserNotification;

namespace myApp {
    public class NotificationService : UNNotificationServiceExtension {
    
        public override void DidReceiveNotificationRequest(UNNotificationRequest request, contentHandler) {
            // Decrypt payload
            var decryptedBody = Decrypt(Request.Content.UserInfo["encrypted-content"]);
            
            // Modify Notification body
            var newContent = new UNMutableNotificationContent();
            newContent.Body = decryptedBody;
            
            // Present to user
            contentHandler(newContent);
        }
        
        public override void TimeWillExpire() {
            // Handle out-of-time fallback event
            ...
        }
        
    }
}
```

암호화 된 콘텐츠를 해독 하는이 코드를 `encrypted-content` 키를 새로 만들고 `UNMutableNotificationContent`, 설정를 `Body` 속성을 사용 하 고 암호 해독 된 콘텐츠를 `contentHandler` 사용자에 게 알림을 표시 하 합니다.

## <a name="summary"></a>요약

이 문서에서는 모든 iOS 10에서 사용자 알림 기능이 강화 된 방법을 설명 했습니다. 새 사용자 알림 프레임 워크 및 Xamarin.iOS 앱 또는 앱 확장에서 사용 하는 방법을 제공 합니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications 프레임 워크 참조](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
