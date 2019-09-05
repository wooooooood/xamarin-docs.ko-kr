---
title: Xamarin.ios의 중요 한 알림
description: 이 문서에서는 Xamarin.ios에서 중요 한 경고를 사용 하는 방법을 설명 합니다. IOS 12에 도입 된 중요 한 알림은 방해 금지 기능이 설정 되어 있는지 또는 벨소리 스위치가 꺼져 있는지에 관계 없이 소리를 재생 하는 중단 알림입니다.
ms.prod: xamarin
ms.assetid: 75742257-081D-44F4-B49E-FB807DF85262
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/04/2018
ms.openlocfilehash: 54a214215f77b66f6a4b134dcb8d27b26c44fb6c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291286"
---
# <a name="critical-alerts-in-xamarinios"></a>Xamarin.ios의 중요 한 알림

IOS 12를 사용 하면 앱이 중요 한 알림을 보낼 수 있습니다. 중요 한 알림은 방해 금지 기능을 사용 하도록 설정 했는지 여부에 관계 없이 소리를 재생 하 고, 벨소리 스위치를 해제 합니다. 이러한 알림은 중단 되며 사용자가 즉각적인 작업을 수행 해야 하는 경우에만 사용 해야 합니다.

## <a name="custom-critical-alert-entitlement"></a>사용자 지정 중요 알림 자격

앱에서 중요 한 경고를 표시 하려면 먼저 Apple에서 [사용자 지정 중요 경고 알림 자격을 요청](https://developer.apple.com/contact/request/notifications-critical-alerts-entitlement/) 합니다.

Apple에서이 자격을 수신 하 고 앱을 사용 하도록 구성 하는 방법에 대 한 연결 된 지침에 따라 앱의 **info.plist** 파일에 사용자 지정 [자격](~/ios/deploy-test/provisioning/entitlements.md) 을 추가 합니다. 그런 다음 시뮬레이터와 장치 모두에서 앱에 서명 하는 경우 **info.plist** 를 사용 하도록 **iOS 번들 서명** 옵션을 구성 합니다.

## <a name="request-authorization"></a>권한 부여 요청

앱의 알림 권한 부여 요청은 사용자에 게 앱의 알림을 허용 하거나 허용 하지 않도록 요청 하는 메시지를 표시 합니다. 알림 권한 부여 요청에서 중요 한 경고를 보낼 수 있는 권한을 요청 하면 앱은 사용자에 게 중요 한 경고를 옵트인 (opt in) 할 수 있는 기회를 제공 합니다.

다음 코드에서는 적절 한를 전달 하 여 중요 한 경고와 표준 알림 및 소리를 모두 보낼 수 있는 권한을 요청 합니다.[`UNAuthorizationOptions`](xref:UserNotifications.UNAuthorizationOptions)
[`RequestAuthorization`](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*)값:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.CriticalAlert;
    center.RequestAuthorization(options, (bool success, NSError error) => {
        // ...
    );
    return true;
}
```

## <a name="local-critical-alerts"></a>중요 한 로컬 경고

중요 한 로컬 경고를 보내려면 다음을 만듭니다.[`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent)
해당 `Sound` 속성을 다음 중 하나로 설정 합니다.

- `UNNotificationSound.DefaultCriticalSound`기본 중요 알림 소리를 사용 하는입니다.
- `UNNotificationSound.GetCriticalSound`-앱과 볼륨에 번들로 제공 되는 사용자 지정 사운드를 지정할 수 있습니다.

그런 다음 알림 콘텐츠에서 `UNNotificationRequest` 를 만들고 알림 센터에 추가 합니다.

```csharp
var content = new UNMutableNotificationContent()
{
    Title = "Critical alert title",
    Body = "Text of the critical alert",
    CategoryIdentifier = "my-critical-alert-category",
    // Sound = UNNotificationSound.DefaultCriticalSound
    Sound = UNNotificationSound.GetCriticalSound("my_critical_sound.m4a", 1.0f)
};

var request = UNNotificationRequest.FromIdentifier(
    Guid.NewGuid().ToString(),
    content,
    UNTimeIntervalNotificationTrigger.CreateTrigger(3, false)
);

var center = UNUserNotificationCenter.Current;
center.AddNotificationRequest(request, null);
```

> [!IMPORTANT]
> 앱에 대해 사용 하도록 설정 되지 않은 경우 중요 한 알림이 배달 되지 않습니다. 앱이 중요 한 경고를 보낼 수 있는 권한을 처음으로 요청할 때 표시 되는 메시지와 함께 iOS **설정** 앱의 앱 **알림** 섹션에서 중요 한 경고를 사용 하거나 사용 하지 않도록 설정할 수도 있습니다.

## <a name="remote-critical-alerts"></a>원격 중요 알림

원격 중요 한 경고에 대 한 자세한 내용은 WWDC 2018에서 [사용자 알림 세션의 새로운 기능](https://developer.apple.com/videos/play/wwdc2018/710/) 및 [원격 알림 생성](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) 문서를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [Xamarin.ios의 사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [사용자 알림의 새로운 기능 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [모범 사례 및 사용자 알림의 새로운 기능 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [원격 알림 생성 (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
