---
title: Xamarin.ios에서 향상 된 사용자 알림
description: 이 문서에서는 iOS 10에 도입 된 사용자 알림 프레임 워크에 대해 설명 합니다. 로컬 알림, 원격 알림, 알림 관리, 알림 작업 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/02/2017
ms.openlocfilehash: 0ec63162a21333d0ff831ded1ab17a3d8bb0efaa
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769371"
---
# <a name="enhanced-user-notifications-in-xamarinios"></a>Xamarin.ios에서 향상 된 사용자 알림

IOS 10의 새로운 기능으로, 사용자 알림 프레임 워크를 사용 하면 로컬 및 원격 알림을 배달 하 고 처리할 수 있습니다. 이 프레임 워크를 사용 하 여 앱 또는 앱 확장은 위치 또는 시간 등의 조건 집합을 지정 하 여 로컬 알림의 배달을 예약할 수 있습니다.

## <a name="about-user-notifications"></a>사용자 알림 정보

위에서 설명한 것 처럼 새 사용자 알림 프레임 워크는 로컬 및 원격 알림을 배달 하 고 처리할 수 있습니다. 이 프레임 워크를 사용 하 여 앱 또는 앱 확장은 위치 또는 시간 등의 조건 집합을 지정 하 여 로컬 알림의 배달을 예약할 수 있습니다.

또한 앱 또는 확장은 사용자의 iOS 장치에 전달 될 때 로컬 및 원격 알림을 수신 하 고 잠재적으로 수정할 수 있습니다.

새 사용자 알림 UI 프레임 워크를 사용 하면 앱 또는 앱 확장이 사용자에 게 표시 될 때 로컬 및 원격 알림의 모양을 사용자 지정할 수 있습니다.

이 프레임 워크는 앱이 사용자에 게 알림을 제공할 수 있는 다음과 같은 방법을 제공 합니다.

- **시각적 경고** -알림이 화면 위쪽에서 배너로 롤업되는 위치입니다.
- **Sound 및 Vibrations** -알림과 연결할 수 있습니다.
- **앱 아이콘 배지** -앱의 아이콘에는 읽지 않은 전자 메일 메시지의 수와 같은 새 콘텐츠를 사용할 수 있음을 보여 주는 배지가 표시 됩니다.

또한 사용자의 현재 컨텍스트에 따라 알림이 표시 되는 방법에는 여러 가지가 있습니다.

- 장치의 잠금이 해제 되 면 화면 맨 위에서 배너로 롤오버 됩니다.
- 장치가 잠기면 사용자의 잠금 화면에 알림이 표시 됩니다.
- 사용자가 알림을 누락 하는 경우 알림 센터를 열고 사용 가능한 모든 대기 중인 알림을 볼 수 있습니다.

Xamarin.ios 앱에는 보낼 수 있는 두 가지 유형의 사용자 알림이 있습니다.

- **로컬 알림** -사용자 장치에 로컬로 설치 된 앱에서 전송 됩니다.
- **원격 알림** -원격 서버에서 전송 되 고 사용자에 게 제공 되거나 앱 콘텐츠의 백그라운드 업데이트를 트리거합니다.

### <a name="about-local-notifications"></a>로컬 알림 정보

IOS 앱에서 보낼 수 있는 로컬 알림에는 다음과 같은 기능 및 특성이 있습니다.

- 사용자의 장치에 로컬인 앱에서 전송 됩니다. 
- 이러한 트리거는 시간 또는 위치 기반 트리거 중 하나를 사용 하도록 구성할 수 있습니다. 
- 앱은 사용자의 장치를 사용 하 여 알림을 예약 하 고 트리거 조건이 충족 될 때 표시 됩니다.
- 사용자가 알림과 상호 작용할 때 앱은 콜백을 받게 됩니다.

로컬 알림의 예는 다음과 같습니다.

- 일정 경고
- 알림 메시지
- 위치 인식 트리거

자세한 내용은 Apple의 [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/documentation/usernotifications) 설명서를 참조 하세요.

### <a name="about-remote-notifications"></a>원격 알림 정보

IOS 앱에서 보낼 수 있는 원격 알림에는 다음과 같은 기능 및 특성이 있습니다.

- 앱에는 통신 하는 서버 쪽 구성 요소가 있습니다.
- APNs (Apple Push Notification Service)는 개발자의 클라우드 기반 서버에서 사용자의 장치에 대 한 원격 알림의 최상의 전달을 전송 하는 데 사용 됩니다.
- 앱이 원격 알림을 받으면 사용자에 게 표시 됩니다.
- 사용자가 알림과 상호 작용할 때 앱은 콜백을 받게 됩니다.

원격 알림의 예는 다음과 같습니다.

- 뉴스 경고
- 스포츠 업데이트
- 인스턴트 메시징 메시지

IOS 앱에 사용할 수 있는 원격 알림은 다음과 같은 두 가지 유형이 있습니다.

- **사용자 측** -장치에서 사용자에 게 표시 됩니다.
- **자동 업데이트** -백그라운드에서 iOS 앱의 콘텐츠를 업데이트 하는 메커니즘을 제공 합니다. 자동 업데이트가 수신 되 면 앱은 원격 서버에 연결 하 여 최신 콘텐츠를 끌어올 수 있습니다.

자세한 내용은 Apple의 [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/documentation/usernotifications) 설명서를 참조 하세요.

### <a name="about-the-existing-notifications-api"></a>기존 알림 API 정보

Ios 10 이전에는 ios 앱이를 사용 `UIApplication` 하 여 시스템에 알림을 등록 하 고 해당 알림이 트리거되는 방법 (시간 또는 위치)을 예약 합니다.

기존 알림 API로 작업할 때 개발자에 게 발생할 수 있는 몇 가지 문제가 있습니다.

- 코드 중복이 발생할 수 있는 로컬 또는 원격 알림에 다른 콜백이 필요 합니다.
- 앱은 시스템에 예약 된 후 알림에 대 한 제어를 제한 했습니다.
- 모든 Apple의 기존 플랫폼에서 서로 다른 수준의 지원이 있습니다.

### <a name="about-the-new-user-notification-framework"></a>새 사용자 알림 프레임 워크 정보

IOS 10을 사용 하는 경우 Apple은 위에서 언급 한 기존 `UIApplication` 메서드를 대체 하는 새로운 사용자 알림 프레임 워크를 도입 했습니다.

사용자 알림 프레임 워크는 다음과 같은 기능을 제공 합니다.

- 이전 메서드로 기능 패리티를 포함 하는 친숙 한 API를 사용 하면 기존 프레임 워크의 코드를 쉽게 이식할 수 있습니다.
- 에는 더 다양 한 알림이 사용자에 게 전송 될 수 있도록 하는 확장 된 콘텐츠 옵션 집합이 포함 되어 있습니다.
- 로컬 알림과 원격 알림은 동일한 코드 및 콜백에 의해 처리 될 수 있습니다.
- 사용자가 알림과 상호 작용할 때 앱에 전송 되는 콜백 처리 프로세스를 간소화 합니다.
- 알림을 제거 하거나 업데이트 하는 기능을 포함 하 여 보류 중이거나 배달 된 알림의 관리 기능이 향상 되었습니다.
- 앱에서 알림의 표시를 수행 하는 기능을 추가 합니다.
- 앱 확장 내에서 알림을 예약 하 고 처리 하는 기능을 추가 합니다.
- 알림 자체에 대 한 새 확장 지점을 추가 합니다. 

새 사용자 알림 프레임 워크는 Apple에서 지 원하는 다음을 비롯 한 여러 플랫폼에서 통합 알림 API를 제공 합니다. 

- **iOS** -알림을 관리 하 고 예약 하기 위한 완전 한 지원입니다.
- **tvOS** -로컬 및 원격 알림에 대 한 앱 아이콘을 배지 하는 기능을 추가 합니다.
- **watchOS** -사용자의 페어링된 iOS 장치에서 Apple Watch로 알림을 전달 하는 기능을 추가 하 고 감시 앱에서 직접 로컬 알림을 수행할 수 있는 기능을 감시 앱에 제공 합니다.

자세한 내용은 Apple의 [Usernotifications 프레임 워크 참조](https://developer.apple.com/reference/usernotifications) 및 [Usernotificationsui](https://developer.apple.com/reference/usernotificationsui) 설명서를 참조 하세요.

## <a name="preparing-for-notification-delivery"></a>알림 배달 준비

IOS 앱이 사용자에 게 알림을 보내기 전에 앱이 시스템에 등록 되어 있어야 하며, 알림이 사용자에 게 중단 되기 때문에 앱에서 권한을 명시적으로 요청 해야 합니다.

사용자가 앱에 대해 승인할 수 있는 세 가지 수준의 알림 요청이 있습니다.

- 배너가 표시 됩니다.
- 소리 경고.
- 앱 아이콘을 배지.

또한 로컬 알림과 원격 알림 모두에 대해 이러한 승인 수준을 요청 하 고 설정 해야 합니다.

`FinishedLaunching` `UNAuthorizationOptions` 의메서드에다음코드를추가하고원하는알림유형()을설정하여앱이시작되는즉시알림권한을`AppDelegate` 요청 해야 합니다.

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

또한 사용자는 언제 든 지 장치에서 **설정** 앱을 사용 하 여 앱에 대 한 알림 권한을 변경할 수 있습니다. 앱은 다음 코드를 사용 하 여 알림을 표시 하기 전에 사용자가 요청한 알림 권한을 확인 해야 합니다.

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
});    
``` 

### <a name="configuring-the-remote-notifications-environment"></a>원격 알림 환경 구성

IOS 10의 새로운 기능으로 개발자는 개발 또는 프로덕션 환경에서 실행 되는 환경 푸시 알림을 OS에 알려야 합니다. 이 정보를 제공 하지 않으면 Itunes 앱 스토어에 제출 될 때 앱이 거부 되 고 다음과 같은 알림이 발생할 수 있습니다.

> 푸시 알림 권한 없음-앱에 Apple의 푸시 알림 서비스에 대 한 API가 포함 되어 `aps-environment` 있지만 앱의 서명에 자격이 없습니다.

필요한 자격을 제공 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. `Entitlements.plist` **Solution Pad** 파일을 두 번 클릭 하 여 편집용으로 엽니다.
2. **원본** 뷰로 전환 합니다. 

    [![](enhanced-user-notifications-images/setup01.png "원본 뷰")](enhanced-user-notifications-images/setup01.png#lightbox)
3. 단추를 **+** 클릭 하 여 새 키를 추가 합니다.
4. 속성 `aps-environment` 에 대해를 입력 하 `String` 고, `development` **형식** 을 그대로 유지 하 `production` 고, **값**에 대해 또는를 입력 합니다. 

    [![](enhanced-user-notifications-images/setup02.png "Ap 환경 속성")](enhanced-user-notifications-images/setup02.png#lightbox)
5. 파일의 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. `Entitlements.plist` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집용으로 엽니다.
2. 단추를 **+** 클릭 하 여 새 키를 추가 합니다.
3. 속성 `aps-environment` 에 대해를 입력 하 `String` 고, `development` **형식** 을 그대로 유지 하 `production` 고, **값**에 대해 또는를 입력 합니다. 

    [![](enhanced-user-notifications-images/setup02w.png "Ap 환경 속성")](enhanced-user-notifications-images/setup02.png#lightbox)
4. 파일의 변경 내용을 저장합니다.

-----

### <a name="registering-for-remote-notifications"></a>원격 알림에 등록 하는 중

앱이 원격 알림을 보내고 받는 경우에도 기존 `UIApplication` API를 사용 하 여 토큰을 _등록_ 해야 합니다. 이 등록을 위해서는 장치에 라이브 네트워크 연결 액세스 APNs가 있어야 하며,이는 앱에 전송 되는 필수 토큰을 생성 합니다. 그러면 앱은 원격 알림을 등록 하기 위해 개발자의 서버 쪽 앱에이 토큰을 전달 해야 합니다.

[![](enhanced-user-notifications-images/token01.png "토큰 등록 개요")](enhanced-user-notifications-images/token01.png#lightbox)

다음 코드를 사용 하 여 필요한 등록을 초기화 합니다.

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

개발자의 서버 쪽 앱에 전송 되는 토큰은 원격 알림을 보낼 때 서버에서 APNs로 전송 되는 알림 페이로드의 일부로 포함 되어야 합니다.

[![](enhanced-user-notifications-images/token02.png "알림 페이로드의 일부로 포함 된 토큰입니다.")](enhanced-user-notifications-images/token02.png#lightbox)

토큰은 알림을 열거나 알림에 응답 하는 데 사용 되는 앱과 알림과 함께 연결 되는 키 역할을 합니다.

자세한 내용은 Apple의 [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/documentation/usernotifications) 설명서를 참조 하세요.

## <a name="notification-delivery"></a>알림 배달

앱이 완전히 등록 되 고, 사용자가에서 요청 하 고 요청 하는 데 필요한 권한을 부여 하 여 이제 앱이 알림을 보내고 받을 준비가 되었습니다. 

### <a name="providing-notification-content"></a>알림 콘텐츠 제공

IOS 10을 처음 사용 하는 경우 모든 알림에는 알림 콘텐츠의 **본문과** 함께 항상 표시 되는 **제목과** **부제목** 이 모두 포함 되어 있습니다. 또한 새는 알림 콘텐츠에 **미디어 첨부 파일** 을 추가 하는 기능입니다.

로컬 알림의 콘텐츠를 만들려면 다음 코드를 사용 합니다.

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

원격 알림의 경우 프로세스는 다음과 유사 합니다.

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

### <a name="scheduling-when-a-notification-is-sent"></a>알림을 보낼 때 예약

만든 알림의 콘텐츠를 사용 하 여 앱은 *트리거*를 설정 하 여 사용자에 게 알림이 표시 되는 시점을 예약 해야 합니다. iOS 10은 다음과 같은 네 가지 트리거 유형을 제공 합니다.

- **푸시 알림** -원격 알림과 독점적으로 사용 되며 APNs가 장치에서 실행 중인 앱에 알림 패키지를 보낼 때 트리거됩니다.
- **시간 간격** -시간 간격에서 시작 하 여 일정을 예약 하는 데 사용할 수 있습니다. 예를 들면 `var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`
- **일정 날짜** -특정 날짜 및 시간에 대해 로컬 알림이 예약 되도록 허용 합니다.
- **위치 기반** -iOS 장치에서 특정 지리적 위치를 입력 하거나 종료 하거나 Bluetooth 오류 신호에 대 한 지정 된 근접 위치에 있을 때 로컬 알림을 예약할 수 있습니다.

로컬 알림이 준비 되 면 앱은 `Add` `UNUserNotificationCenter` 개체의 메서드를 호출 하 여 사용자에 게 표시를 예약 해야 합니다. 원격 알림의 경우 서버 쪽 앱은 알림 페이로드를 APNs에 보내고, 그러면이 사용자의 장치에 패킷을 보냅니다.

모든 부분을 함께 가져오는 샘플 로컬 알림은 다음과 같습니다.

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

## <a name="handling-foreground-app-notifications"></a>포그라운드 앱 알림 처리

IOS 10을 처음 접하는 앱은 전경에 있고 알림이 트리거될 때 알림을 다르게 처리할 수 있습니다. 를 `UNUserNotificationCenterDelegate` 제공 하 고 `WillPresentNotification` 메서드를 구현 하 여 앱은 알림을 표시 하는 데 책임이 있습니다. 예:

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

이 코드는 단순히 응용 프로그램 출력 `UNNotification` 에의 콘텐츠를 작성 하 고 시스템에 알림에 대 한 표준 경고를 표시 하도록 요청 하는 것입니다. 

응용 프로그램이 전경에 있을 때 알림 자체를 표시 하 고 시스템 기본값을 사용 하지 않으려는 경우 완료 처리기에 전달 `None` 합니다. 예제:

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

이 코드를 저장 하 고 편집을 `AppDelegate.cs` 위해 파일을 열고 메서드를 `FinishedLaunching` 다음과 같이 변경 합니다.

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

이 코드는 앱이 활성 `UNUserNotificationCenterDelegate` 상태이 고 포그라운드에서 알림을 `UNUserNotificationCenter` 처리할 수 있도록 위의 사용자 지정을 현재에 연결 합니다.

## <a name="notification-management"></a>알림 관리

IOS 10의 새로운 기능으로, 알림 관리는 보류 중인 알림과 배달 알림에 대 한 액세스를 제공 하 고 이러한 알림을 제거, 업데이트 또는 승격 하는 기능을 추가 합니다.

알림 관리의 중요 한 부분은 알림이 생성 되 고 시스템에서 예약 될 때 알림에 할당 된 _요청 식별자_ 입니다. 원격 알림의 경우이는 HTTP 요청 헤더의 새 `apps-collapse-id` 필드를 통해 할당 됩니다.

요청 식별자는 앱에서 알림 관리를 수행 하려는 알림을 선택 하는 데 사용 됩니다.

### <a name="removing-notifications"></a>알림 제거

시스템에서 보류 중인 알림을 제거 하려면 다음 코드를 사용 합니다.

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

이미 제공 된 알림을 제거 하려면 다음 코드를 사용 합니다.

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>기존 알림 업데이트

기존 알림을 업데이트 하려면 원하는 매개 변수를 수정 하 여 새 알림 (예: 새 트리거 시간)을 만들고 수정 해야 하는 알림과 동일한 요청 식별자를 사용 하 여 시스템에 추가 하면 됩니다. 예제:

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

이미 제공 된 알림의 경우 기존 알림은 홈 및 잠금 화면 및 알림 센터 (사용자가 이미 읽은 경우)의 목록 맨 위로 업데이트 되 고 승격 됩니다.

## <a name="working-with-notification-actions"></a>알림 작업 사용

IOS 10에서 사용자에 게 전달 되는 알림은 정적이 지 않으며, 사용자가 기본 제공 작업에서 사용자 지정 작업으로 상호 작용할 수 있는 여러 가지 방법을 제공 합니다.

IOS 앱에서 응답할 수 있는 작업에는 다음 세 가지 유형이 있습니다.

- **기본 작업** -사용자가 알림을 탭 하 여 앱을 열고 지정 된 알림의 세부 정보를 표시 하는 경우입니다.
- **사용자 지정 작업** -iOS 8에 추가 되었으며 사용자가 앱을 시작할 필요 없이 알림에서 직접 사용자 지정 작업을 수행할 수 있는 빠른 방법을 제공 합니다. 사용자 지정 가능한 제목이 있는 단추 목록 또는 백그라운드에서 실행할 수 있는 텍스트 입력 필드 (앱에 요청을 수행 하는 데 약간의 시간이 지정 된 경우) 또는 포그라운드 (앱이 포그라운드에서 시작 하는 fu)로 표시 될 수 있습니다. 요청을 입력 합니다. 사용자 지정 작업은 iOS와 watchOS에서 모두 사용할 수 있습니다.
- **작업 해제** -이 작업은 사용자가 지정 된 알림을 해제할 때 앱에 전송 됩니다.

### <a name="creating-custom-actions"></a>사용자 지정 작업 만들기

시스템에 사용자 지정 작업을 만들고 등록 하려면 다음 코드를 사용 합니다.

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

새 `UNNotificationAction`를 만들 때 단추에 표시 되는 고유한 ID와 제목이 할당 됩니다. 기본적으로 작업은 백그라운드 작업으로 만들어지므로 작업 동작을 조정 하기 위한 옵션을 지정할 수 있습니다 (예: 전경 작업으로 설정).

만든 각 작업을 범주와 연결 해야 합니다. 새 `UNNotificationCategory`를 만들 때 범주에 있는 작업의 용도에 대 한 자세한 정보와 범주의 동작을 제어 하기 위한 몇 가지 옵션을 제공 하는 고유한 id, 수행할 수 있는 작업 목록, 의도 id 목록이 할당 됩니다.

마지막으로 모든 범주가 `SetNotificationCategories` 메서드를 사용 하 여 시스템에 등록 됩니다.

### <a name="presenting-custom-actions"></a>사용자 지정 작업 프레젠테이션

사용자 지정 작업 및 범주 집합을 만들고 시스템에 등록 한 후에는 로컬 또는 원격 알림에서 표시할 수 있습니다.

원격 알림에 대해 위에서 만든 범주 `category` 중 하 나와 일치 하는 원격 알림 페이로드의를 설정 합니다. 예:

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

로컬 알림의 경우 `CategoryIdentifier` `UNMutableNotificationContent` 개체의 속성을 설정 합니다. 예를 들어:

```csharp
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
content.CategoryIdentifier = "message";
...
```

이 ID는 위에서 만든 범주 중 하 나와 일치 해야 합니다.

### <a name="handling-dismiss-actions"></a>작업 해제 처리

위에서 설명한 것 처럼 사용자가 알림을 해제할 때 해제 작업을 앱에 보낼 수 있습니다. 표준 동작이 아니기 때문에 범주를 만들 때 옵션을 설정 해야 합니다. 예를 들어:

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>작업 응답 처리

사용자가 위에서 만든 사용자 지정 작업 및 범주와 상호 작용 하는 경우 앱에서 요청 된 작업을 수행 해야 합니다. 를 `UNUserNotificationCenterDelegate` 제공 하 고 `UserNotificationCenter` 메서드를 구현 하 여이 작업을 수행 합니다. 예를 들어:

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

전달 `UNNotificationResponse` 된 클래스 `ActionIdentifier` 에는 기본 작업 또는 해제 작업 중 하나를 사용할 수 있는 속성이 있습니다. 사용자 `response.Notification.Request.Identifier` 지정 작업을 테스트 하는 데 사용 합니다.

속성 `UserText` 에는 모든 사용자 텍스트 입력 값이 포함 됩니다. 속성 `Notification` 에는 트리거와 알림 콘텐츠를 포함 하는 요청이 포함 된 원래 알림이 포함 됩니다. 앱은 트리거 유형을 기반으로 하는 로컬 또는 원격 알림 인지를 결정할 수 있습니다.

> [!NOTE]
> iOS 12를 사용 하면 사용자 지정 알림 UI에서 런타임에 작업 단추를 수정할 수 있습니다. 자세한 내용은 [동적 알림 작업 단추](~/ios/platform/introduction-to-ios12/notifications/dynamic-actions.md) 설명서를 참조 하세요.

## <a name="working-with-service-extensions"></a>서비스 확장 작업

원격 알림을 사용 하 여 작업 하는 경우 _서비스 확장_ 은 알림 페이로드 내에서 종단 간 암호화를 사용 하도록 설정 하는 방법을 제공 합니다. 서비스 확장은 사용자에 게 표시 되기 전에 알림의 표시 된 콘텐츠를 확대 하거나 대체 하는 주 목적을 사용 하 여 백그라운드에서 실행 되는 사용자가 아닌 인터페이스 확장 (iOS 10에서 사용 가능)입니다. 

[![](enhanced-user-notifications-images/extension01.png "서비스 확장 개요")](enhanced-user-notifications-images/extension01.png#lightbox)

서비스 확장은 신속 하 게 실행 되며 시스템에서 실행 되는 짧은 시간 동안만 제공 됩니다. 서비스 확장이 할당 된 시간 내에 작업을 완료 하지 못할 경우 대체 (fallback) 메서드가 호출 됩니다. 대체가 실패 하면 원래 알림 콘텐츠가 사용자에 게 표시 됩니다.

서비스 확장의 몇 가지 가능한 용도는 다음과 같습니다.

- 원격 알림 콘텐츠의 종단 간 암호화를 제공 합니다.
- 원격 알림에 첨부 파일을 추가 하 여 보강

### <a name="implementing-a-service-extension"></a>서비스 확장 구현

Xamarin.ios 앱에서 서비스 확장을 구현 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac용 Visual Studio에서 앱 솔루션을 엽니다.
2. **Solution Pad** 에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 프로젝트 추가**를 선택 합니다.
3. **IOS** > **확장** 알림 서비스 확장을 선택 하 고 다음 단추를 클릭 합니다. >  

    [![](enhanced-user-notifications-images/extension02.png "알림 서비스 확장 선택")](enhanced-user-notifications-images/extension02.png#lightbox)
4. 확장의 **이름을** 입력 하 고 **다음** 단추를 클릭 합니다. 

    [![](enhanced-user-notifications-images/extension03.png "확장의 이름 입력")](enhanced-user-notifications-images/extension03.png#lightbox)
5. 필요한 경우 **프로젝트 이름** 및/또는 **솔루션 이름을** 조정 하 고 **만들기** 단추를 클릭 합니다. 

    [![](enhanced-user-notifications-images/extension04.png "프로젝트 이름 및/또는 솔루션 이름 조정")](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio에서 앱 솔루션을 엽니다.
2. **솔루션 탐색기** 에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 프로젝트**...를 선택 합니다.
3. **C# Visual > IOS 확장 > Notification Service 확장**을 선택 합니다.

    [![](enhanced-user-notifications-images/extension01.w157-sml.png "알림 서비스 확장 선택")](enhanced-user-notifications-images/extension01.w157.png#lightbox)
4. 확장의 **이름을** 입력 하 고 **확인** 단추를 클릭 합니다.

-----

> [!IMPORTANT]
> 서비스 확장의 번들 식별자는 끝에 추가 된 `.appnameserviceextension` 주 앱의 번들 식별자와 일치 해야 합니다. 예를 들어 기본 앱에의 `com.xamarin.monkeynotify`번들 식별자가 있는 경우 서비스 확장에는 번들 `com.xamarin.monkeynotify.monkeynotifyserviceextension`식별자가 있어야 합니다. 확장이 솔루션에 추가 될 때 자동으로 설정 됩니다. 

알림 서비스 확장에는 필요한 기능을 제공 하기 위해 수정 해야 하는 주 클래스가 하나 있습니다. 예:

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

첫 번째 메서드인 `DidReceiveNotificationRequest`는 알림 식별자 뿐만 아니라 `request` 개체를 통해 알림 콘텐츠도 전달 됩니다. 전달 `contentHandler` 된를 호출 하 여 사용자에 게 알림을 제공 해야 합니다.

두 번째 메서드인 `TimeWillExpire`는 서비스 확장에서 요청을 처리 하기 위해 시간이 실행 되기 직전에 호출 됩니다. 서비스 확장이 할당 된 시간 `contentHandler` 내에를 호출 하지 못하면 원래 콘텐츠가 사용자에 게 표시 됩니다.

### <a name="triggering-a-service-extension"></a>서비스 확장 트리거

서비스 확장을 만들어 앱과 함께 제공 하면 장치에 전송 된 원격 알림 페이로드를 수정 하 여 트리거할 수 있습니다. 예를 들어:

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

새 `mutable-content` 키는 원격 알림 콘텐츠를 업데이트 하기 위해 서비스 확장을 시작 해야 함을 지정 합니다. 키 `encrypted-content` 에는 서비스 확장에서 사용자에 게 표시 하기 전에 암호를 해독할 수 있는 암호화 된 데이터가 포함 됩니다.

다음 예제 서비스 확장을 살펴보세요.

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

이 코드는 `encrypted-content` 키에서 암호화 된 콘텐츠를 해독 하 고 새 `UNMutableNotificationContent`를 만든 다음 `Body` `contentHandler` 속성을 해독 된 콘텐츠로 설정 하 고을 사용 하 여 사용자에 게 알림을 표시 합니다.

## <a name="summary"></a>요약

이 문서에서는 iOS 10에 의해 사용자 알림이 향상 된 모든 방법에 대해 설명 했습니다. 새 사용자 알림 프레임 워크와 Xamarin.ios 앱 또는 앱 확장에서 사용 하는 방법을 제공 합니다.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [UserNotifications 프레임 워크 참조](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/documentation/usernotifications)
