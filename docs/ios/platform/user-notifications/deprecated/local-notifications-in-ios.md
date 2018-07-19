---
title: Xamarin.iOS에서 알림
description: 이 섹션에는 Xamarin.iOS에서 로컬 알림을 구현 하는 방법을 보여 줍니다. IOS 알림는 다양 한 UI 요소에 설명 하 고 API에 설명의 만들기 및 알림을 표시를 사용 하 여 관련 됩니다.
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/13/2018
ms.openlocfilehash: ef5ce97736e60ff3a03bc0d496eadcae8c6f7e61
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038536"
---
# <a name="notifications-in-xamarinios"></a>Xamarin.iOS에서 알림

> [!IMPORTANT]
> 이 섹션의 정보는 iOS 9 이전에 적용 됩니다. IOS 10 이상에서 참조 하십시오 합니다 [사용자 알림 프레임 워크 가이드](~/ios/platform/user-notifications/index.md)합니다.

iOS는 알림을 받았는지 사용자에 게 표시 하는 세 가지 방법에 있습니다.

- **소리 또는 진동을** -iOS 사용자에 게 알리는 소리를 재생할 수 있습니다. 소리를 사용 하지 않도록 설정 하는 경우 장치를 진동 할 구성할 수 있습니다.
- **경고** -알림에 대 한 정보를 사용 하 여 화면에 대화 상자를 표시 하는 것이 가능 합니다.
- **배지** 알림이 게시 되 면-숫자 (달린) 응용 프로그램 아이콘으로 표시 될 수 있습니다.

iOS에서는 *알림 센터* 모든 알림을 로컬 및 원격 사용자에 게 표시할 합니다. 사용자는 화면 맨 위에서 아래로 살짝 밀어이 액세스할 수 있습니다.

![알림 센터](local-notifications-in-ios-images/image13.png "알림 센터")

## <a name="creating-local-notifications-in-ios"></a>IOS에서 로컬 알림 만들기

iOS를 사용 하면 로컬 알림을 처리 하는 것이 간단 합니다.
먼저 iOS 8에는 알림을 표시 하는 사용자의 사용 권한을 요청 해야 응용 프로그램에 필요 합니다. 로컬 알림을 보내려고 시도 하기 전에 앱에 다음 코드를 추가- [샘플을 연결](https://developer.xamarin.com/samples/monotouch/LocalNotifications/) 에 배치 합니다 **AppDelegate**의 **FinishedLaunching** 메서드.

```csharp
var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes(
    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
);
application.RegisterUserNotificationSettings(notificationSettings);
```

[![로컬 알림을 보낼 수를 확인](local-notifications-in-ios-images/image0-sml.png "로컬 알림을 보낼 수를 확인 합니다.")](local-notifications-in-ios-images/image0.png#lightbox)

로컬 알림을 예약을 만들려면를 `UILocalNotification` 개체, 설정 합니다 `FireDate`를 통해 예약 및를 `ScheduleLocalNotification` 메서드를를 `UIApplication.SharedApplication` 개체. 다음 코드 조각은 앞으로 1 분에 실행 되며 메시지를 사용 하 여 경고를 표시 하는 알림을 예약 하는 방법을 보여 줍니다.

```csharp
UILocalNotification notification = new UILocalNotification();
NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

다음 스크린샷은이 경고의 모양을 보여 줍니다.

[![](local-notifications-in-ios-images/image2-sml.png "경고 예제")](local-notifications-in-ios-images/image2.png#lightbox)

사용자를 선택한 경우 *없도록* 알림 후 아무 것도 표시 됩니다.

숫자를 사용 하 여 응용 프로그램 아이콘에 배지를 적용 하려는 경우 다음 코드 줄에에서 표시 된 대로를 설정할 수 있습니다.

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

다음 코드 조각과에서 같이 순서 play에서 아이콘을 사용 하 여 소리 알림을 SoundName 속성 설정:

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

알림 소리 30 초 보다 긴 경우 iOS는 기본 소리 재생 대신 합니다.

> [!IMPORTANT]
> 대리자 알림을 두 번 발생 하는 iOS 시뮬레이터에서 버그가 있습니다. 장치에서 응용 프로그램을 실행 하는 경우에이 문제가 발생 하지 않습니다.

## <a name="handling-notifications"></a>알림 처리

iOS 응용 프로그램에는 거의 동일한 방식으로 원격 및 로컬 알림을 처리 합니다. 응용 프로그램이 실행 되는 경우는 `ReceivedLocalNotification` 메서드 또는 `ReceivedRemoteNotification` 메서드를 `AppDelegate` 클래스를 호출 하 고 알림 정보를 매개 변수로 전달 됩니다.

응용 프로그램에는 다양 한 방식 알림을 처리할 수 있습니다. 예를 들어, 응용 프로그램 일부 이벤트에 대 한 사용자에 게 알림 경고를 표시할 수 있는 합니다. 또는 서버에 동기화 할 파일과 같은 프로세스가 완료 되었음을 사용자에 게 경고를 표시 하도록 알림을 사용할 수 있습니다.

다음 코드는 로컬 알림을 처리 하 고 경고를 표시 배지 수를 0으로 다시 설정 하는 방법을 보여 줍니다.

```csharp
public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
{
    // show an alert
    UIAlertController okayAlertController = UIAlertController.Create(notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
    okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

    Window.RootViewController.PresentViewController(okayAlertController, true, null);

    // reset our badge
    UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
}
```

응용 프로그램을 실행 하지 않는 경우 iOS는 소리를 재생 및/또는 해당 아이콘 배지를 업데이트 합니다. 응용 프로그램이 시작 되는 사용자는 경고와 관련 된 응용 프로그램 시작 되 면 하며 `FinishedLaunching` 앱 대리자에서 메서드 호출 되며 알림 정보를 통해 전달 됩니다는 `launchOptions` 매개 변수입니다. 옵션 사전 키를 포함 하는 경우 `UIApplication.LaunchOptionsLocalNotificationKey`, 그런 다음 `AppDelegate` 알림을 로컬에서 실행 되는 알고 있습니다. 다음 코드 조각에서는이 프로세스를 보여 줍니다.

```csharp
// check for a local notification
if (launchOptions.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
{
    var localNotification = launchOptions[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
    if (localNotification != null)
    {
        UIAlertController okayAlertController = UIAlertController.Create(localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

        Window.RootViewController.PresentViewController(okayAlertController, true, null);

        // reset our badge
        UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
    }
}
```

원격 알림을 `launchOptions` 갖습니다는 `LaunchOptionsRemoteNotificationKey` 가 연결 된 `NSDictionary` 원격 알림 페이로드를 포함 하 합니다. 통해 알림 페이로드를 추출할 수 있습니다 합니다 `alert`, `badge`, 및 `sound` 키입니다. 다음 코드 조각은 원격 알림을 받는 방법을 보여 줍니다.

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>요약

이 섹션을 만들고 Xamarin.iOS에서 알림을 게시 하는 방법을 보여주었습니다. 알림 재정의 하 여 응용 프로그램 수 대응 하는 방법을 표시 하는 `ReceivedLocalNotification` 메서드 또는 `ReceivedRemoteNotification` 에서 메서드는 `AppDelegate`합니다.

## <a name="related-links"></a>관련 링크

- [로컬 알림 (샘플)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [로컬 및 푸시 알림을 개발자를 위한](https://developer.apple.com/notifications/)
- [로컬 및 푸시 알림 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
