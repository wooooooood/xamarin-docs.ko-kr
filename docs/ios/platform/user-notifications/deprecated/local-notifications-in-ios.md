---
title: Xamarin.ios의 알림
description: 이 섹션에서는 Xamarin.ios에서 로컬 알림을 구현 하는 방법을 보여 줍니다. IOS 알림의 다양 한 UI 요소에 대해 설명 하 고 알림을 만들고 표시 하는 것과 관련 된 API에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/13/2018
ms.openlocfilehash: 7f2619010a410cabc54074e669ff4f1ea24bd0fa
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655502"
---
# <a name="notifications-in-xamarinios"></a>Xamarin.ios의 알림

> [!IMPORTANT]
> 이 섹션의 정보는 iOS 9 및 이전과 관련이 있습니다. IOS 10 이상에서는 [사용자 알림 프레임 워크 가이드](~/ios/platform/user-notifications/index.md)를 참조 하세요.

iOS에는 알림이 수신 되었음을 사용자에 게 표시 하는 세 가지 방법이 있습니다.

- **사운드 또는 진동** -iOS에서 사용자에 게 알리는 소리를 재생할 수 있습니다. 사운드가 사용 하지 않도록 설정 된 경우 장치를 진동로 구성할 수 있습니다.
- **경고** -알림에 대 한 정보를 사용 하 여 화면에 대화 상자를 표시할 수 있습니다.
- **배지** -알림이 게시 될 때 응용 프로그램 아이콘에 숫자 (직장 배지가 달린)를 표시할 수 있습니다.

iOS는 로컬 및 원격 모든 알림을 사용자에 게 표시 하는 *알림 센터* 도 제공 합니다. 사용자는 화면 맨 위에서 아래로 살짝 밀어이에 액세스할 수 있습니다.

![알림 센터](local-notifications-in-ios-images/image13.png "알림 센터")

## <a name="creating-local-notifications-in-ios"></a>IOS에서 로컬 알림 만들기

iOS를 사용 하면 매우 간단 하 게 로컬 알림을 만들고 처리할 수 있습니다.
먼저 iOS 8에서는 응용 프로그램이 사용자에 게 알림을 표시 하는 권한을 요청 해야 합니다. 로컬 알림을 보내기 전에 응용 프로그램에 다음 코드를 추가 합니다. [연결 된 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/localnotifications) 은이를 **AppDelegate**의 **시작** 된 edstarted 메서드에 배치 합니다.

```csharp
var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes(
    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
);
application.RegisterUserNotificationSettings(notificationSettings);
```

[![로컬 알림 전송 기능 확인](local-notifications-in-ios-images/image0-sml.png "로컬 알림 전송 기능 확인")](local-notifications-in-ios-images/image0.png#lightbox)

로컬 알림을 예약 `UILocalNotification` 하려면 개체를 만들고를 `FireDate`설정 하 `ScheduleLocalNotification` 고 `UIApplication.SharedApplication` 개체의 메서드를 통해 일정을 예약 합니다. 다음 코드 조각에서는 나중에 1 분이 발생 하는 알림을 예약 하 고 메시지와 함께 경고를 표시 하는 방법을 보여 줍니다.

```csharp
UILocalNotification notification = new UILocalNotification();
notification.FireDate = NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

다음 스크린샷에서는이 경고가 표시 되는 모습을 보여 줍니다.

[![](local-notifications-in-ios-images/image2-sml.png "예제 경고")](local-notifications-in-ios-images/image2.png#lightbox)

사용자가 알림을 *허용 하지* 않도록 선택한 경우 아무 것도 표시 되지 않습니다.

숫자를 사용 하 여 응용 프로그램 아이콘에 배지를 적용 하려면 다음 줄 코드와 같이 설정할 수 있습니다.

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

아이콘을 사용 하 여 소리를 재생 하려면 다음 코드 조각에 표시 된 것 처럼 알림의 SoundName 속성을 설정 합니다.

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

알림 소리가 30 초 보다 길면 iOS는 대신 기본 소리를 재생 합니다.

> [!IMPORTANT]
> IOS 시뮬레이터에는 대리자 알림을 두 번 발생 시키는 버그가 있습니다. 장치에서 응용 프로그램을 실행 하는 경우에는이 문제가 발생 하지 않습니다.

## <a name="handling-notifications"></a>알림 처리

iOS 응용 프로그램은 거의 똑같은 방식으로 원격 및 로컬 알림을 처리 합니다. 응용 프로그램을 `ReceivedLocalNotification` 실행 하는 경우 `AppDelegate` 클래스의 메서드 `ReceivedRemoteNotification` 또는 메서드가 호출 되 고 알림 정보가 매개 변수로 전달 됩니다.

응용 프로그램은 다양 한 방식으로 알림을 처리할 수 있습니다. 예를 들어 응용 프로그램은 사용자에 게 일부 이벤트를 알리는 경고를 표시할 수 있습니다. 또는 서버에 파일을 동기화 하는 것과 같이 프로세스가 완료 되었음을 사용자에 게 알리는 경고를 표시 하는 데 사용할 수 있습니다.

다음 코드에서는 로컬 알림을 처리 하 고 경고를 표시 하 고 배지 번호를 0으로 다시 설정 하는 방법을 보여 줍니다.

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

응용 프로그램이 실행 되 고 있지 않으면 iOS는 소리를 재생 하 고 아이콘 배지를 해당 하는 대로 업데이트 합니다. 사용자가 경고와 연결 된 응용 프로그램을 시작 하면 응용 프로그램이 시작 되 고 `FinishedLaunching` 앱 대리자의 메서드가 호출 되 고 매개 변수를 `launchOptions` 통해 알림 정보가 전달 됩니다. 옵션 사전에 키 `UIApplication.LaunchOptionsLocalNotificationKey`가 포함 되어 있으면는 `AppDelegate` 응용 프로그램이 로컬 알림에서 시작 된 것을 알고 있습니다. 다음 코드 조각에서는이 프로세스를 보여 줍니다.

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

원격 알림의 `launchOptions` 경우에는 원격 알림 페이로드를 포함 하 `NSDictionary` 는 `LaunchOptionsRemoteNotificationKey` 와 연결 된가 있습니다. `alert` ,`badge`및 키`sound` 를 통해 알림 페이로드를 추출할 수 있습니다. 다음 코드 조각에서는 원격 알림을 가져오는 방법을 보여 줍니다.

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>요약

이 섹션에서는 Xamarin.ios에서 알림을 만들고 게시 하는 방법을 살펴보았습니다. 에서 `ReceivedLocalNotification` 메서드`ReceivedRemoteNotification`또는 메서드를 재정의 하 여 응용 프로그램이 알림에 대응할 수 있는 방법을 보여 줍니다. `AppDelegate`

## <a name="related-links"></a>관련 링크

- [로컬 알림 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/localnotifications)
- [개발자를 위한 로컬 및 푸시 알림](https://developer.apple.com/notifications/)
- [로컬 및 푸시 알림 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
