---
title: Xamarin.iOS에서 중요 한 알림
description: 이 문서에서는 Xamarin.iOS를 사용 하 여 중요 한 경고를 사용 하는 방법을 설명 합니다. 중요 한 알림, 12, iOS를 사용 하 여 도입 된 방해 금지 인지에 관계 없이 소리를 재생 하는 중단 알림을 또는 신호음 스위치 해제 됩니다.
ms.prod: xamarin
ms.assetid: 75742257-081D-44F4-B49E-FB807DF85262
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: 699d19228d2dee92f7a730bba4186a3aa5f21b04
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233239"
---
# <a name="critical-alerts-in-xamarinios"></a>Xamarin.iOS에서 중요 한 알림

IOS 12 사용 하 여 앱 위험 경고를 보낼 수 있습니다. 방해 금지 설정 여부에 관계 없이 소리를 재생 하는 중요 한 알림 또는 벨소리 스위치 해제 되어 있습니다. 이러한 알림은 중단 이며 사용자 즉각적인 작업을 수행 해야 하는 경우에 사용 해야 합니다.

## <a name="custom-critical-alert-entitlement"></a>중요 한 사용자 지정 경고 자격

먼저 앱에서 중요 한 경고를 표시 하도록 [중요 한 사용자 지정 경고 알림을 자격 요청](https://developer.apple.com/contact/request/notifications-critical-alerts-entitlement/) Apple에서.

이 자격은 Apple에서 받아 사용 하려면 앱을 구성 하는 방법에 대 한 모든 관련된 지침에 따라 추가 사용자 지정 [자격](~/ios/deploy-test/provisioning/entitlements.md) 앱의 **Entitlements.plist** 파일입니다. 그런 다음 구성에 **iOS Bundle Signing** 옵션을 사용 하 여 **Entitlements.plist** 시뮬레이터와 장치 모두에서 앱을 서명 하는 경우.

## <a name="request-authorization"></a>권한 부여를 요청 합니다.

앱의 알림을 허용 하거나 거부 하 라는 메시지를 표시 하는 앱의 알림 인증 요청 합니다. 알림 권한 부여 요청을 중요 한 경고를 보낼 수 있는 권한 요청 하는 경우 앱에서는 제공 사용자 위험 경고에 옵트인 할 수 있는 기회입니다.

다음 코드를 적절 한 전달 하 여 중요 한 알림 및 표준 알림와 소리를 보낼 수 있는 요청 [`UNAuthorizationOptions`](xref:UserNotifications.UNAuthorizationOptions)
값을 [ `RequestAuthorization` ](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*):

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

## <a name="local-critical-alerts"></a>로컬 중요 한 경고

로컬 중요 한 경고를 보내도록 만들기를 [`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent)
설정 및 해당 `Sound` 속성을 합니다.

- `UNNotificationSound.DefaultCriticalSound`를 사용 하는 기본 위험 알림 소리 합니다.
- `UNNotificationSound.GetCriticalSound`앱 및 볼륨에 함께 제공 되는 사용자 지정 하는 사운드를 지정할 수 있습니다.

그런 다음 만들기를 `UNNotificationRequest` 알림에서 콘텐츠 및 알림 센터에 추가:

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
> 앱에 대 한 활성화 되지 않은 경우 중요 한 알림이 배달 되지 않습니다. 첫 번째 나타나는 프롬프트와 함께 중요 한 경고를 보내도록 하나의 앱 사용 권한 요청을 시간, 사용자를 사용 하도록 설정 하거나 앱의 중요 한 경고를 사용 하지 않도록 설정할 수도 있습니다 **알림을** ios 섹션 **설정을**앱.

## <a name="remote-critical-alerts"></a>원격 중요 한 알림

원격 위험 경고에 대 한 정보를 참조 하세요 합니다 [사용자 알림에서 새 란](https://developer.apple.com/videos/play/wwdc2018/710/) WWDC 2018에서 세션 및 [원격 알림을 생성](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) 문서.

## <a name="related-links"></a>관련 링크

- [Xamarin.iOS에서 사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [사용자 알림 (WWDC 2018)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2018/710/)
- [모범 사례 및 사용자 알림 (WWDC 2017)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2017/708/)
- [원격 알림을 (Apple) 생성](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
