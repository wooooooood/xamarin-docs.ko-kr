---
title: Xamarin.ios의 Provisional 알림
description: 이 문서에서는 Xamarin.ios를 사용 하 여 provisional 알림과 함께 작업 하는 방법을 설명 합니다. IOS 12에 도입 된 Provisional 알림을 통해 응용 프로그램은 명시적인 사용자 권한 없이 자동 알림을 보낼 수 있습니다.
ms.prod: xamarin
ms.assetid: 5DCB36B9-2637-48AE-8FC0-F6124F08AC48
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/04/2018
ms.openlocfilehash: d321e8061d3091abeaa3cff6a6af9172c981cb60
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291195"
---
# <a name="provisional-notifications-in-xamarinios"></a>Xamarin.ios의 Provisional 알림

Provisional 알림을 사용 하면 앱에서 사용자의 명시적인 사전 동의 없이 알림을 배달할 수 있습니다. 이러한 알림은 자동으로 도착 하 고 알림 센터에만 표시 됩니다. 그러면 사용자가 계속 해 서 배달 하거나 옵트아웃 하기 전에 미리 볼 수 있습니다.

알림 센터에서 사용자는 앱이 provisional 알림 배달을 중지 하거나, 되었으면를 계속 제공 하거나, 더 명확 하 게 전달 하기 시작 하도록 지정할 수 있습니다.

## <a name="sample-app-redgreennotifications"></a>샘플 앱: RedGreenNotifications

Provisional 알림을 보내는 [RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications) 샘플 앱을 살펴보세요.

## <a name="sending-provisional-notifications"></a>Provisional 알림 보내기

Provisional 알림을 전송 하려면을 옵션 `UNAuthorizationOptions.Provisional` 으로 제공 합니다.[`RequestAuthorization`](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*)
`UNUserNotificationCenter`메서드:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional;
    center.RequestAuthorization(options, (bool success, NSError error) => {
        // ...
    );
    return true;
}
```

사용자가 provisional `UNAuthorizationOptions` 알림을 중요 한 배달으로 승격 하는 경우에 `RequestAuthorization` 전달 된 값이 새 알림 배달 설정 (위 코드 `UNAuthorizationOptions.Alert` 에서는 및 `UNAuthorizationOptions.Sound`)을 결정 합니다.

## <a name="related-links"></a>관련 링크

- [샘플 앱-RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Xamarin.ios의 사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [사용자 알림의 새로운 기능 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [모범 사례 및 사용자 알림의 새로운 기능 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [원격 알림 생성 (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
