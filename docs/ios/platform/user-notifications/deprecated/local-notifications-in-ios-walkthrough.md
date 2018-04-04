---
title: 연습-Xamarin.iOS에 로컬 알림을 사용 하 여
description: 이 섹션 Xamarin.iOS 응용 프로그램에서 로컬 알림을 사용 하는 방법을 살펴봅니다. 만들기 및 앱에서 받은 경우 경고를 팝업 됩니다 나타내는 알림을 게시 하는 기본적인을 설명 합니다.
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: bb133d16f12249cbd31e4fce2b227162b4b28333
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>연습-Xamarin.iOS에 로컬 알림을 사용 하 여

_이 섹션 Xamarin.iOS 응용 프로그램에서 로컬 알림을 사용 하는 방법을 살펴봅니다. 만들기 및 앱에서 받은 경우 경고를 팝업 됩니다 나타내는 알림을 게시 하는 기본적인을 설명 합니다._

> [!IMPORTANT]
> IOS 9에 관련 된이 섹션의 정보 및 prior 되지 않았습니다 여기 이전 iOS 버전을 지원 하도록 합니다. IOS 10 이상에서 참조 하십시오는 [사용자 알림 프레임 워크 가이드](~/ios/platform/user-notifications/index.md) iOS 장치에서 로컬 및 원격 알림 지원에 대 한 합니다.

## <a name="walkthrough"></a>연습

작업에서 로컬 알림을 표시 하는 간단한 응용 프로그램을 만들 수 있도록 합니다. 이 응용 프로그램에 한 번의 단추를 갖습니다. 단추를 클릭할 때 로컬 알림이 만들어집니다. 지정 된 시간 동안이 경과한 후, 표시 알림을 살펴봅니다.


1. Mac 용 Visual Studio에서 새 단일 보기 iOS 솔루션을 만들고 호출할 `Notifications`합니다.
1. 열기는 `Main.storyboard` 파일과 보기 단추를 끌어 합니다. 단추 이름을 **단추**, 하 고 제목 **추가 알림**합니다. 일부를 설정 하려면 [제약 조건을](~/ios/user-interface/designer/designer-auto-layout.md) 은 단추가 시점에: 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "단추에 몇 가지 제약 조건이 설정")
1. 편집는 `ViewController` 클래스와 ViewDidLoad 메서드를 다음 이벤트 처리기를 추가 합니다.

    ```csharp
    button.TouchUpInside += (sender, e) =>
    {
        // create the notification
        var notification = new UILocalNotification();

        // set the fire date (the date time in which it will fire)
        notification.FireDate = NSDate.FromTimeIntervalSinceNow(60);

        // configure the alert
        notification.AlertAction = "View Alert";
        notification.AlertBody = "Your one minute alert has fired!";

        // modify the badge
        notification.ApplicationIconBadgeNumber = 1;

        // set the sound to be the default sound
        notification.SoundName = UILocalNotification.DefaultSoundName;

        // schedule it
        UIApplication.SharedApplication.ScheduleLocalNotification(notification);
    };
    ```

    이 코드에서는 소리를 사용 하 여 아이콘 배지 값을 1로 설정 하 고 사용자에 게 경고를 표시 하는 알림을 만듭니다.

1. 다음 파일을 편집 `AppDelegate.cs`, 먼저 다음 코드를 추가 `FinishedLaunching` 메서드. 장치가 iOS 8을 실행 하는 경우 참조는 하므로 하는 경우 확인란을 선택한 **필요한** 알림을 받을 사용자의 사용 권한을 요청 해야 합니다.

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
            var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes (
                UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
            );

            application.RegisterUserNotificationSettings (notificationSettings);
        }
    ```

1. 여전히 `AppDelegate.cs`, 알림의 받을 때 호출 되는 다음 메서드를 추가 합니다.

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

1. 여기서 알림을 로컬 알림을 때문에 시작 하는 경우를 처리 해야 합니다. 방법을 편집 `FinishedLaunching` 에 `AppDelegate` 코드의 다음 코드 조각은 포함 하려면:


    ```csharp
    // check for a notification

    if (launchOptions != null)
    {
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
    }

    ```

1. 마지막으로, 응용 프로그램을 실행 합니다. IOS에서 8 나타납니다 알림을 사용할 수 있도록 합니다. 클릭 **확인** 클릭 하 고는 **알림을 추가할** 단추입니다. 잠시 후 다음 스크린샷에 표시 된 것 처럼 경고 대화 상자에서 표시 됩니다.

    ![](local-notifications-in-ios-walkthrough-images/image0.png "알림을 보낼 수 있는 기능 확인") ![ ] (local-notifications-in-ios-walkthrough-images/image1.png "The 추가 알림 단추") ![ ] (local-notifications-in-ios-walkthrough-images/image2.png "알림 경고 대화 상자")

## <a name="summary"></a>요약

이 연습에서는 만들기 및 iOS에서 알림을 게시에 대 한 다양 한 API를 사용 하는 방법을 배웠습니다. 또한 응용 프로그램 아이콘 사용자에 게 일부 응용 프로그램 특정 피드백을 제공 하기 위해 배지를 업데이트 하는 방법을 보여 줍니다.


## <a name="related-links"></a>관련 링크

- [로컬 알림 (샘플)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [로컬 및 푸시 알림을 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
