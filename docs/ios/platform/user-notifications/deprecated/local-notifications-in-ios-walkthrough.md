---
title: 연습-Xamarin.iOS에서 로컬 알림 사용
description: 이 섹션에서는 Xamarin.iOS 응용 프로그램에서 로컬 알림을 사용 하는 방법을 살펴보겠습니다. 만들기 및 앱에서 수신 될 때의 경고는 팝업 알림을 게시의 기본 사항에서는 보여 줍니다.
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 7589784563906d60fc8026feac9ea16362463bfa
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102635"
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>연습-Xamarin.iOS에서 로컬 알림 사용

_이 섹션에서는 Xamarin.iOS 응용 프로그램에서 로컬 알림을 사용 하는 방법을 살펴보겠습니다. 만들기 및 앱에서 수신 될 때의 경고는 팝업 알림을 게시의 기본 사항에서는 보여 줍니다._

> [!IMPORTANT]
> IOS 9에 관련 된이 섹션의 정보 및 prior 되지 않았습니다 여기 이전 iOS 버전을 지원 하도록 합니다. IOS 10 이상에서 참조 하십시오 합니다 [사용자 알림 프레임 워크 가이드](~/ios/platform/user-notifications/index.md) iOS 장치에서 로컬 및 원격 알림을 지원 하기 위해.

## <a name="walkthrough"></a>연습

로컬 알림 작업에 표시 되는 간단한 응용 프로그램을 만들 수 있습니다. 이 응용 프로그램에 단일 단추를 사용 해야 합니다. 단추를 클릭 하는 경우 로컬 알림을 만듭니다. 지정 된 기간 동안 경과 후 알림이 표시를 살펴보겠습니다.


1. Mac 용 Visual Studio에서 새 단일 보기 iOS 솔루션을 만들고 호출할 `Notifications`합니다.
1. 열기는 `Main.storyboard` 파일과 보기로 단추가 끌기. 단추 이름을 **단추**, 하 고 제목을 **알림 추가**합니다. 일부을 설정할 수도 있습니다 [제약 조건](~/ios/user-interface/designer/designer-auto-layout.md) 이 시점에서 단추: 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "단추에 몇 가지 제약 조건을 설정합니다.")
1. 편집을 `ViewController` 클래스 및 ViewDidLoad 메서드에 다음 이벤트 처리기를 추가 합니다.

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

1. 다음 파일을 편집 `AppDelegate.cs`, 먼저 다음 코드를 추가 합니다 `FinishedLaunching` 메서드. 에서는 되므로 장치가 iOS 8을 실행 하는 경우 확인 확인란을 선택한 것 **필요한** 알림을 받을 사용자의 사용 권한을 요청 해야 합니다.

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

                UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(okayAlertController, true, null);

                // reset our badge
                UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
            }

    ```

1. 로컬 알림으로 인해 알림을 시작한 있는 경우를 처리 해야 합니다. 메서드를 편집할 `FinishedLaunching` 에 `AppDelegate` 다음의 코드 조각을 포함 하려면:


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

1. 마지막으로, 응용 프로그램을 실행 합니다. Ios 8 묻는 알림을 허용 하려면. 클릭 **확인** 을 클릭 한 다음는 **알림을 추가할** 단추입니다. 잠시 후 다음 스크린샷에 표시 된 것 처럼 경고 대화 상자를 표시 됩니다.

    ![](local-notifications-in-ios-walkthrough-images/image0.png "알림을 전송 하는 기능을 확인") ![](local-notifications-in-ios-walkthrough-images/image1.png "추가 알림 단추") ![](local-notifications-in-ios-walkthrough-images/image2.png "알림 경고 대화 상자")

## <a name="summary"></a>요약

이 연습에는 만들기 및 iOS에서 알림을 게시에 대 한 다양 한 API를 사용 하는 방법을 보여 주었습니다. 또한 사용자에 게 일부 응용 프로그램 특정 피드백을 제공 하는 배지와 함께 응용 프로그램 아이콘을 업데이트 하는 방법을 보여 줍니다.


## <a name="related-links"></a>관련 링크

- [로컬 알림 (샘플)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [로컬 및 푸시 알림 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
