---
title: Xamarin.iOS에서 임시 알림
description: 이 문서에서는 Xamarin.iOS를 사용 하 여 임시 알림과 작동 하도록 하는 방법을 설명 합니다. 임시 알림, iOS 12에에서 도입 된 명시적 사용자 권한 없이 자동 알림을 보내도록 응용 프로그램을 허용 합니다.
ms.prod: xamarin
ms.assetid: 5DCB36B9-2637-48AE-8FC0-F6124F08AC48
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: d4a90ee0b85144269d80815820fe09fa0fcd3c58
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233157"
---
# <a name="provisional-notifications-in-xamarinios"></a>Xamarin.iOS에서 임시 알림

임시 알림을 사용자의 명시적 사전 동의 없이 알림을 배달 하는 앱을 허용 합니다. 이러한 알림을 자동으로 도착 하 고 사용자가 해당 지속적인된 배달 안팎에서 선택 하기 전에 미리 볼 수 있는 알림 센터에만 표시 합니다.

알림 센터에서 앱 해야 provisional 알림 배달을 중지를 일시적으로 전달 하 계속 하거나 더 확실 하 게 제공 하기 시작 하 사용자 지정할 수 있습니다.

## <a name="sample-app-redgreennotifications"></a>샘플 앱: RedGreenNotifications

살펴봅니다 합니다 [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications) provisional 알림을 전송 하는 샘플 앱입니다.

## <a name="sending-provisional-notifications"></a>임시 알림 보내기

임시 알림을 보내려면 제공 `UNAuthorizationOptions.Provisional` 하기 위한 옵션으로는 [`RequestAuthorization`](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*)
메서드의 `UNUserNotificationCenter`:

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

사용자 임시 주요 배달 알림을 승격 하는 경우는 `UNAuthorizationOptions` 에 전달 된 값 `RequestAuthorization` 새 알림 배달 설정을 결정 합니다 (위의 코드에서는 `UNAuthorizationOptions.Alert` 및 `UNAuthorizationOptions.Sound`).

## <a name="related-links"></a>관련 링크

- [샘플 앱 – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Xamarin.iOS에서 사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [사용자 알림 (WWDC 2018)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2018/710/)
- [모범 사례 및 사용자 알림 (WWDC 2017)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2017/708/)
- [원격 알림을 (Apple) 생성](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
