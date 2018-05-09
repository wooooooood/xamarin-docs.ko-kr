---
title: 향상 된 사용자 알림
description: 이 문서에는 모든 사용자가 알림 iOS 10 및 Xamarin.iOS 앱에서 사용 하는 방법으로 향상 된 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: b27d415240f3b8cd25c4bc54f6d176c50e42a250
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="enhanced-user-notifications"></a>향상 된 사용자 알림

_이 문서에는 모든 사용자가 알림 iOS 10 및 Xamarin.iOS 앱에서 사용 하는 방법으로 향상 된 방법을 설명 합니다._

10, 배달 및 로컬 및 원격 알림 처리를 위한 프레임 워크에서는 사용자 알림 iOS를 처음 사용 합니다. 응용 프로그램 또는 응용 프로그램 확장은이 프레임 워크를 사용 하 여 위치와 같은 조건 집합이 또는 시간을 지정 하 여 로컬 알림 배달을 예약할 수 있습니다.

## <a name="about-user-notifications"></a>사용자 알림 정보

위에서 설명한 대로 배달 및 로컬 및 원격 알림 처리를 위한 새로운 사용자 알림 프레임 워크 수 있습니다. 응용 프로그램 또는 응용 프로그램 확장은이 프레임 워크를 사용 하 여 위치와 같은 조건 집합이 또는 시간을 지정 하 여 로컬 알림 배달을 예약할 수 있습니다.

또한 응용 프로그램 또는 확장 받을 (하 고 잠재적으로 수정할 수) 알림을 로컬 및 원격 사용자의 iOS 장치에 배달 될 때는 합니다.

새 사용자 알림 UI 프레임 워크에는 응용 프로그램 또는 사용자에 게 제공 하는 경우에 로컬 및 원격 알림 표시를 사용자 지정에 대 한 응용 프로그램 확장 수 있습니다.

이 프레임 워크에서는 앱 사용자에 게 알림을 배달할 수 있는 다음과 같은 방법으로 제공 합니다.

- **Visual 경고** -알림 위치 배너로 화면 맨 위에서부터 세분화 합니다.
- **사운드 및 진동** -알림과 연결 될 수 있습니다.
- **응용 프로그램 아이콘 배지** -응용 프로그램의 아이콘이 새 콘텐츠를 읽지 않은 전자 메일 메시지 수와 같이 사용할 수 있는지를 보여 주는 배지를 표시 합니다.

또한 사용자의 현재 컨텍스트에 따라 여러 가지 있는 알림이 표시 됩니다.

- 장치가 잠긴 경우 알림을 공개 아래로 화면 맨 위에서부터 배너로.
- 장치가 잠겨 있으면 사용자의 잠금 화면에 알림을 표시 됩니다.
- 사용자가 알림을 누락, 하는 경우 알림 센터를 열 수 있으며 있는 모든 사용 가능한 대기 알림을 보려면 있습니다.

Xamarin.iOS 앱에 두 가지 유형의 사용자 알림을 보낼 수 있습니다.

- **로컬 알림을** -이러한 사용자가 장치에 로컬로 설치 된 앱으로 전송 됩니다.
- **원격 알림** -원격에서 보낸 사용자에 게 서버와 하나 또는 백그라운드 업데이트의 응용 프로그램의 콘텐츠를 트리거합니다.

### <a name="about-local-notifications"></a>로컬 알림 정보

다음 기능 및 특성에 있는 iOS 앱을 보낼 수 있는 로컬 알림에 있을:

- 사용자의 장치에서 로컬로 사용 되는 앱으로 전송 됩니다. 
- 구성할 수 시간 또는 위치를 사용 하도록 트리거를 기반으로 합니다. 
- 응용 프로그램 사용자의 장치에 알림을 예약 하 고 트리거 조건이 충족 되 면 표시 됩니다.
- 알림을와 상호 작용할 응용 프로그램 콜백을 받게 됩니다.

로컬 알림을의 몇 가지 예는 다음과 같습니다.

- 일정 알림
- 미리 알림 경고
- 위치 인식 트리거

자세한 내용은 Apple의를 참조 하십시오 [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) 설명서입니다.

### <a name="about-remote-notifications"></a>원격 알림 정보

다음 기능 및 특성에 있는 iOS 앱을 보낼 수 있는 원격 알림에 있을:

- 응용 프로그램에 통신 하는 서버 쪽 구성 요소입니다.
- Apple 푸시 알림 서비스 (APNs)은 개발자의 클라우드 기반 서버에서 사용자의 장치에 원격 알림을 전달 하는 최상의 노력을 전송 하는 데 사용 됩니다.
- 응용 프로그램 원격 알림을 수신 하는 경우 사용자에 게 표시 됩니다.
- 알림을와 상호 작용할 응용 프로그램 콜백을 받게 됩니다.

원격 알림의 몇 가지 예는 다음과 같습니다.

- 뉴스 경고
- 스포츠 업데이트
- 인스턴트 메시징 메시지

IOS 응용 프로그램에 사용할 수 있는 두 가지 유형의 원격 알림이 됩니다.

- **사용자 연결** -이 장치에서 사용자에 게 표시 됩니다.
- **자동 업데이트** -이러한 백그라운드에서 iOS 앱의 내용을 업데이트 하는 메커니즘을 제공 합니다. 자동 업데이트를 받으면 제거 서버 풀에서 최신 콘텐츠 다운 앱 교류할 수 있습니다.

자세한 내용은 Apple의를 참조 하십시오 [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) 설명서입니다.

### <a name="about-the-existing-notifications-api"></a>기존 알림 API에 대 한

사용 하는 iOS 앱 10 iOS 전에 `UIApplication` 시스템으로 알림을 등록 하 고 (또는 중 하나가 시간 위치)이 알림은 실행 방법 예약 합니다.

개발자는 기존 알림 API 사용 하는 경우 발생할 수 있는 몇 가지 문제가 있습니다.

- 로컬 또는 원격 알림을 코드의 중복 발생할 수 있습니다에 필요한 다른 콜백 있었습니다.
- 응용 프로그램 시스템으로 예약한 후 알림 제어를 제한 했습니다.
- Apple의 기존 플랫폼의 모든 서로 다른 수준으로 지원 했습니다.

### <a name="about-the-new-user-notification-framework"></a>새 사용자 알림 프레임 워크에 대 한

Apple iOS 10과 기존를 대체 하는 새 사용자 알림 framework 소개 했습니다 `UIApplication` 메서드 위에 언급 합니다.

사용자 알림 프레임 워크는 다음을 제공합니다.

- 이전 방법을 쉽게 코드를 이식 하는 기존 프레임 워크에서 사용 하 여 기능 패리티를 포함 하는 친숙 한 API.
- 다양 한 알림 사용자에 게 메시지를 허용 하는 확장된 콘텐츠 옵션 집합이 포함 되어 있습니다.
- 로컬 및 원격 알림을 모두 동일한 코드 및 콜백을 처리할 수 있습니다.
- 알림을와 상호 작용할 때 앱에 전송 되는 콜백을 처리 하는 과정을 간소화 합니다.
- 향상 된 관리 보류 중 및 배달 알림을 제거 하거나 업데이트 알림 기능을 포함 합니다.
- 앱에서 표시 알림 작업을 수행 하는 기능을 추가 합니다.
- 예약 하 고 응용 프로그램 확장 내에서 알림을 처리 하는 기능을 추가 합니다.
- 알림 자체에 대 한 새 확장 지점을 추가합니다. 

Apple에서는 포함 된 플랫폼의 여러 통합된 알림을 API를 제공 하는 새로운 사용자 알림 프레임 워크: 

- **iOS** -완벽 하 게 관리 하 고 알림을 예약할 지원 합니다.
- **tvOS** -로컬 및 원격 알림에 대 한 배지 앱 아이콘에 있는 기능을 추가 합니다.
- **watchOS** -알림을 사용자의 쌍으로 연결 된 iOS 장치에서 자신의 Apple Watch 전달 하는 기능을 추가 하 고 조사식 앱 조사식 자체에서 직접 로컬 알림을 수행 하는 기능을 제공 합니다.

자세한 내용은 Apple의를 참조 하십시오 [UserNotifications 프레임 워크 참조](https://developer.apple.com/reference/usernotifications) 및 [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui) 설명서입니다.

## <a name="preparing-for-notification-delivery"></a>알림 배달에 대 한 준비

IOS 하기 전에 앱에 알림을 보낼 수는 시스템에 응용 프로그램을 등록 해야 사용자 및 응용 프로그램을 보내기 전에 권한을 명시적으로 요청 해야 알림이 사용자에 게 중단 이기 때문에 있습니다.

세 개의 서로 다른 수준의 알림 요청을 응용 프로그램에 대 한 사용자를 승인할 수 있습니다.

- 배너를 표시합니다.
- 사운드 알림을 제공 합니다.
- 앱 아이콘을 배지 합니다.

또한 이러한 승인 수준 요청한 고 알림을 로컬 및 원격 모두에 대해 설정 해야 합니다.

다음 코드를 추가 하 여 앱이 시작 되는 즉시 알림 권한을 요청할 수는 `FinishedLaunching` 의 메서드는 `AppDelegate` 원하는 알림 유형을 설정 하 고 (`UNAuthorizationOptions`):

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

또한 사용자는 응용 프로그램에서 사용 하는 모든 시간에 대 한 알림 권한을 변경할 항상 수는 **설정을** 장치에 앱입니다. 앱 다음 코드를 사용 하 여 알림을 표시 하기 전에 사용자의 요청 된 알림 권한 검사 해야 합니다.

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
}); 
``` 

### <a name="configuring-the-remote-notifications-environment"></a>원격 알림을 환경 구성

새로운 iOS 10에 개발자 환경을 푸시 알림 개발 또는 프로덕션에서 실행 중인 운영 체제에 게 알려야 합니다. 이 정보를 제공 하지 않으면 다음과 비슷한는 알림과 함께 iTune 앱 스토어에 제출 하는 경우 거부 되 고 응용 프로그램에서 발생할 수 있습니다.

> 누락 된 푸시 알림 자격-앱 Apple 푸시 알림 서비스에 대 한 API를 포함 하지만 `aps-environment` 자격 응용 프로그램의 서명에서 누락 되었습니다.

필요한 권리를 제공 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 두 번 클릭는 `Entitlements.plist` 파일에 **솔루션 패드** 를 편집 하기 위해 엽니다.
2. 전환 하는 **소스** 보기: 

    [![](enhanced-user-notifications-images/setup01.png "소스 뷰")](enhanced-user-notifications-images/setup01.png#lightbox)
3. 클릭는 **+** 단추를 새 키를 추가 합니다.
4. 입력 `aps-environment` 에 대 한는 **속성**, 둡니다는 **형식** 으로 `String` 하나를 입력 하 고 `development` 또는 `production` 에 대 한는 **값**: 

    [![](enhanced-user-notifications-images/setup02.png "Aps 환경 속성")](enhanced-user-notifications-images/setup02.png#lightbox)
5. 파일의 변경 내용을 저장합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 두 번 클릭는 `Entitlements.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
3. 클릭는 **+** 단추를 새 키를 추가 합니다.
4. 입력 `aps-environment` 에 대 한는 **속성**, 둡니다는 **형식** 으로 `String` 하나를 입력 하 고 `development` 또는 `production` 에 대 한는 **값**: 

    [![](enhanced-user-notifications-images/setup02w.png "Aps 환경 속성")](enhanced-user-notifications-images/setup02.png#lightbox)
5. 파일의 변경 내용을 저장합니다.

-----

### <a name="registering-for-remote-notifications"></a>원격 알림 등록

응용 프로그램을 보내고 원격 알림을 받기, 하는 경우 여전히 할 해야 합니다 _토큰 등록_ 기존를 사용 하 여 `UIApplication` API입니다. 이 등록 하려면 장치를 생성 하는 응용 프로그램에 전송 될 필요한 토큰 APNs는 라이브 네트워크 연결 액세스. 응용 프로그램은 다음 원격 알림을 등록 하는 개발자의 서버 쪽 앱을이 토큰을 전달 해야 합니다.

[![](enhanced-user-notifications-images/token01.png "토큰 등록 개요")](enhanced-user-notifications-images/token01.png#lightbox)

다음 코드를 사용 하 여 필요한 등록 초기화:

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

알림 페이로드는 get의 일부를 보낼 때 서버에서 APNs 원격 알림을 보낼 때 포함 되도록 서버 쪽 응용 프로그램 개발자의로 전송 하는 토큰이 필요 합니다.

[![](enhanced-user-notifications-images/token02.png "알림 페이로드의 일부로 포함 된 토큰")](enhanced-user-notifications-images/token02.png#lightbox)

토큰이 함께 알림과 열거나 알림에 응답 하는 데 사용 되는 앱에 연결 하는 키로 사용 됩니다.

자세한 내용은 Apple의를 참조 하십시오 [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) 설명서입니다.

## <a name="notification-delivery"></a>알림 배달

앱과 완전히 등록 하 고에서 요청 하는 데 필요한 권한이 부여 된 사용자가 이제 앱을 보내고 알림을 받을 준비가 합니다. 

### <a name="providing-notification-content"></a>알림 콘텐츠를 제공합니다.

새로운 iOS 10에 모든 알림 모두 포함 한 **제목** 및 **부제목** 를 항상 표시 됩니다는 **본문** 알림 콘텐츠의 합니다. 또한 새로 만들었거나는 추가 하는 기능 **미디어 첨부 파일** 알림 내용에 있습니다.

로컬 알림을의 콘텐츠를 만들려면 다음 코드를 사용 합니다.

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

원격 알림에 대 한 프로세스와 비슷합니다.

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

### <a name="scheduling-when-a-notification-is-sent"></a>전송 된 알림에 한 때를 예약 합니다.

응용 프로그램이 해야 만든 알림의 콘텐츠로 알림을 설정 하 여 사용자에 표시 되는 시점을 예약 하는 *트리거*합니다. iOS 10에는 4 개의 다른 트리거 형식을 제공합니다.

- **푸시 알림** -원격 알림과 함께 단독으로 사용 하 고 APNs 전송할 알림 장치에서 실행 중인 앱을 패키지 하는 경우에 트리거됩니다.
- **시간 간격** -이제 주소와 끝 이후의 특정 시점으로 간격 시작 시간에서 예약 될 로컬 알림을 허용 합니다. 예를 들면 `var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`과 같습니다.
- **날짜** -특정 날짜 및 시간에 대해 예약할 수 로컬 알림을 허용 합니다.
- **위치 기반** -iOS 장치 시작 또는 특정 지리적 위치를 종료 하거나 모든 Bluetooth 탐지 장치에 주어진 근접 때 예약 될 로컬 알림을 허용 합니다.

응용 프로그램을 호출 해야 로컬 알림을 준비 되 면는 `Add` 의 메서드는 `UNUserNotificationCenter` 예약 사용자에 게 표시 하는 개체입니다. 원격 알림에 대 한 서버 쪽 앱 알림 페이로드를 보냅니다 APNs, 사용자의 장치에 로그온 패킷을 보냅니다.

모든 부분이 함께 가져오기 샘플 로컬 알림 같을 수 있습니다.

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

새로운 iOS 10에는 응용 프로그램 알림을 처리할 수 다르게 포그라운드에서 되 고 알림이 트리거됩니다. 제공 하 여는 `UNUserNotificationCenterDelegate` 및 구현 된 `UserNotificationCenter` 메서드를 응용 프로그램 알림을 표시 하는 것에 대 한 책임 걸릴 수 있습니다. 예를 들어:

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

이 코드의 내용을 쓰는 단순히는 `UNNotification` 알림에 대 한 표준 경고를 표시 하려면 응용 프로그램 출력 및 시스템에 요청을 합니다. 

응용 프로그램을 전경에 되었을 때 알림을 자체를 표시 하지 시스템 기본값 사용을 전달 하려는 경우 `None` 완료 처리기에 있습니다. 예제:

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

이 코드 위치에서 사용 하 여 열 여 `AppDelegate.cs` 편집을 위해 파일을 변경는 `FinishedLaunching` 메서드를 다음과 같이:

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

사용자 지정 연결 됨이 코드는 `UNUserNotificationCenterDelegate` 위쪽에서 현재 `UNUserNotificationCenter` 활성화 되어 있는 동안 앱 알림을 처리할 수 있도록 및 포그라운드에서 합니다.

## <a name="notification-management"></a>알림, 관리

새로운 iOS 10에 알림 관리 보류 중 및 배달 알림 액세스를 제공 하며, 제거, 업데이트 하거나 이러한 알림을 승격 하는 기능을 추가 합니다.

알림 관리의 중요 한 부분이 되는 _요청 식별자_ 생성 되어 시스템으로 예약 하는 경우 알림을에 할당 된 합니다. 원격 알림에 대 한이를 통해 할당 된 새 `apps-collapse-id` HTTP 요청 헤더에서 필드입니다.

요청 식별자는 앱에 알림 관리를 수행 하려고 하는 알림을 선택 하는 데 사용 됩니다.

### <a name="removing-notifications"></a>알림 제거

보류 중인 알림 시스템에서을 제거 하려면 다음 코드를 사용 합니다.

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

이미 배달 된 알림을 제거 하려면 다음 코드를 사용 합니다.

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>기존 알림 업데이트

기존 알림을 업데이트 하려면 단순히 새로운 알림이 필요한 매개 변수 (예: 새 트리거 시간) 수정 만들고 수정 해야 알림으로 동일한 요청 식별자를 사용 하 여 시스템에 추가 합니다. 예제:


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

이미 전달 된 알림에 대 한 기존 알림은 업데이트 가져오고 이미 읽은 사용자가 홈 및 잠금 화면에서 알림 센터에서는 목록의 위쪽으로 승격 합니다.

## <a name="working-with-notification-actions"></a>알림 작업 사용

IOS 10에서에서 사용자에 게 배달 되는 알림은 정적 및 (사용자 지정 작업에 기본 제공)에서 이러한 사용자 상호 작용할 수 있는 몇 가지 방법을 제공 합니다.

세 가지 방법으로 iOS 앱에 응답할 수 있습니다. 있는 작업의 수 있습니다:

- **기본 동작이** -사용자가 앱을 열고 지정 된 알림 세부 정보를 표시에 대 한 알림을 누르면입니다.
- **사용자 지정 작업** -iOS 8에서에서 추가 된 이러한 및 응용 프로그램을 실행 하지 않고도 알림을에서 직접 사용자 지정 태스크를 수행 하려면 사용자에 대 한 빠른 방법을 제공 합니다. 사용자 지정 가능한 제목이 단추 목록이 또는 텍스트 입력된 필드 (여기서 응용 프로그램 요청을 처리 하는 데 시간이 적은 양의 제공) 배경이 나 전경에서 실행할 수 있는 (여기서는 앱은 시작 fu 포그라운드로 표시 될 수 있습니다. lfill 요청)입니다. IOS 및 watchOS 둘 다에서 사용할 수 있는 사용자 지정 동작입니다.
- **동작을 해제** -사용자 지정된 알림을 해제할 때이 작업이 응용 프로그램에 전송 됩니다.

### <a name="creating-custom-actions"></a>사용자 지정 동작 만들기

을 만들고 사용자 지정 작업은 시스템을 등록 하려면 다음 코드를 사용 합니다.

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

새로 만들 때 `UNNotificationAction`, 고유 ID 및 단추에 나타나는 제목을 할당 됩니다. 옵션 제공 (예를 들어이 속성을 설정 전경 작업 수) 동작의 동작을 조정 될 수 있지만 작업 기본적으로 백그라운드 작업으로 생성 됩니다.

만든 작업을 각각 범주와 연결 해야 합니다. 새로 만들 때 `UNNotificationCategory`, 고유 ID가 할당, 수행할 수는 작업 목록은, 작업 범주에는 의도 대 한 자세한 내용 및 범주에 속하는 동작을 제어 하는 데는 옵션을 제공 하도록 의도 Id 목록입니다.

마지막으로 사용 하 여 시스템 등록 된 모든 범주는 `SetNotificationCategories` 메서드.

### <a name="presenting-custom-actions"></a>사용자 지정 작업을 제공합니다.

사용자 지정 작업 및 범주 집합을 만들고 시스템에 등록 된 있는 로컬 또는 원격 알림을 표시할 수 있습니다.

원격 알림 설정는 `category` 위에서 만든 범주 중 하 나와 일치 하는 원격 알림 페이로드입니다. 예를 들어:

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

로컬 알림에 대 한 설정에서 `CategoryIdentifier` 의 속성은 `UNMutableNotificationContent` 개체입니다. 예를 들어:

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

위에서 언급 한 것 처럼 사용자가 알림을 해제할 때 해제 작업을 응용 프로그램에 보낼 수 있습니다. 이 표준 동작을 않으므로 옵션을 범주를 만들 때 설정 해야 합니다. 예를 들어:

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>작업 응답 처리

사용자 지정 작업 및 위에서 생성 된 범주와 상호 작용할 응용 프로그램에서 요청 된 작업을 수행 해야 합니다. 이 작업을 제공 하 여 수행 됩니다는 `UNUserNotificationCenterDelegate` 및 구현 된 `UserNotificationCenter` 메서드. 예를 들어:

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

전달 된에서 `UNNotificationResponse` 클래스에는 `ActionIdentifier` 속성 중 하나를 수행할 수 있는 기본 작업 또는 해제 작업 여야 합니다. 사용 하 여 `response.Notification.Request.Identifier` 를 모든 사용자 지정 작업에 대해 테스트 합니다.

`UserText` 속성 모든 사용자 텍스트 입력의 값을 보유 합니다. `Notification` 트리거를 사용 하 여 요청을 포함 하는 원래 알림 및 알림 콘텐츠 속성에 저장 합니다. 응용 프로그램은 로컬 었 경우 나 원격 알림 트리거의 형식에 따라 결정할 수 있습니다.

## <a name="working-with-service-extensions"></a>서비스 확장 사용

원격 알림, 작업할 때 _서비스 확장_ 알림 페이로드 내에서 종단 간 암호화를 사용 하는 방법을 제공 합니다. 서비스 확장은 확장 또는 사용자에 게 표시 하기 전에 알림 표시 된 콘텐츠를 교체의 기본 목적은 백그라운드에서 실행 하는 사용자 인터페이스가 아닌 확장 (iOS 10에서에서 사용 가능). 

[![](enhanced-user-notifications-images/extension01.png "서비스 확장 개요")](enhanced-user-notifications-images/extension01.png#lightbox)

서비스 확장 신속 하 게 실행 되어야 하 고 시스템에서 실행 하는 시간이 짧은 시간을 지정만 됩니다. 서비스 확장명 실패는 할당 된 시간 내에 작업을 완료 하기 대체 메서드가 호출 됩니다. 대체 실패 하면 원래 알림 콘텐츠 나타납니다 사용자에 게 합니다.

서비스 확장의 몇 가지 잠재적인 사용은 다음과 같습니다.

- 원격 알림 콘텐츠의 종단 간 암호화를 제공합니다.
- 보강 하기 위해 원격 알림 첨부 파일을 추가 합니다.

### <a name="implementing-a-service-extension"></a>서비스 확장 프로그램 구현

Xamarin.iOS 앱에서 서비스 확장을 구현 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac.에 대 한 Visual Studio에서 응용 프로그램의 솔루션을 열으십시오
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭는 **솔루션 패드** 선택 **추가** > **새 프로젝트 추가**합니다.
3. 선택 **iOS** > **확장** > **알림 서비스 확장** 클릭는 **다음** 단추: 

    [![](enhanced-user-notifications-images/extension02.png "알림 서비스 확장 선택")](enhanced-user-notifications-images/extension02.png#lightbox)
4. 입력 한 **이름** 확장과 클릭에 대 한는 **다음** 단추: 

    [![](enhanced-user-notifications-images/extension03.png "확장에 대 한 이름을 입력 합니다.")](enhanced-user-notifications-images/extension03.png#lightbox)
5. 조정 된 **프로젝트 이름** 및/또는 **솔루션 이름** 필요 하 고 클릭는 **만들기** 단추: 

    [![](enhanced-user-notifications-images/extension04.png "프로젝트 이름 및/또는 솔루션 이름 조정")](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio에서 응용 프로그램의 솔루션을 엽니다.
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭는 **솔루션 탐색기** 선택 **추가 > 새 프로젝트...** .
3. 선택 **Visual C# > 확장 iOS > 알림 서비스 확장**:

    [![](enhanced-user-notifications-images/extension01.w157-sml.png "알림 서비스 확장 선택")](enhanced-user-notifications-images/extension01.w157.png#lightbox)
4. 입력 한 **이름** 확장과 클릭에 대 한는 **확인** 단추입니다.

-----

> [!IMPORTANT]
> 서비스 확장에 대 한 번들 식별자에는 주 응용 프로그램의 번들 식별자와 일치 해야 `.appnameserviceextension` 끝에 추가 합니다. 예를 들어, 기본 응용 프로그램의 번들 식별자 있으면 `com.xamarin.monkeynotify`, 서비스 확장의 번들 Id를 포함 해야 `com.xamarin.monkeynotify.monkeynotifyserviceextension`합니다. 확장 솔루션에 추가 될 때 자동으로 설정 해야 합니다. 

필요한 기능을 제공 하도록 수정 해야 하는 알림 서비스 확장에는 하나의 기본 클래스가 있습니다. 예를 들어:

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

첫 번째 방법은 `DidReceiveNotificationRequest`, 알림 식별자 뿐만 아니라 알림 콘텐츠를 통해 전달 되는 `request` 개체입니다. 전달 된에서 `contentHandler` 를 호출 하는 알림을 사용자에 게 표시 해야 합니다.

두 번째 방법은 `TimeWillExpire`, 시간은 요청을 처리 하도록 서비스 확장에 대 한 실행 되기 전에 호출 됩니다. 호출에 실패 하면 서비스 확장의 `contentHandler` 할당 된 시간 동안에는 원본 콘텐츠 사용자에 게 표시 됩니다.

### <a name="triggering-a-service-extension"></a>서비스 확장에 트리거

서비스 확장 만들고 앱과 함께 제공, 장치에 전송 원격 알림 페이로드를 수정 하 여 트리거할 수 있습니다. 예를 들어:

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

새 `mutable-content` 키는 원격 알림 콘텐츠를 업데이트 하려면 실행 하도록 서비스 확장 필요 함을 지정 합니다. `encrypted-content` 사용자에 게 제공 하기 전에 서비스 확장 해독할 수 있는 암호화 된 데이터를 보유 하는 키입니다.

다음 예제에서는 서비스 확장을 살펴보겠습니다.

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

이 코드의 암호화 된 콘텐츠를 암호 해독의 `encrypted-content` 키, 새 `UNMutableNotificationContent`, 설정는 `Body` 암호가 해독 된 콘텐츠를 사용 하 여는 `contentHandler` 사용자에 게 알림 표시를 합니다.

## <a name="summary"></a>요약

이 문서의 모든 사용자가 알림 iOS 10에서 향상 된 방법을 검사가 수행 됩니다. Xamarin.iOS 앱 또는 앱 확장에 사용 하는 방법 및 새 사용자 알림 프레임 워크를 제공.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications 프레임 워크 참조](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
