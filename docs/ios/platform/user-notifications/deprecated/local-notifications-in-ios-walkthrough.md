---
title: 연습-Xamarin.ios에서 로컬 알림 사용
description: 이 섹션에서는 Xamarin.ios 응용 프로그램에서 로컬 알림을 사용 하는 방법을 안내 합니다. 앱에서 수신 될 때 경고를 표시 하는 알림을 만들고 게시 하는 기본 사항을 설명 합니다.
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 764be6319e95b16dc043bebd2abfb27ba0696457
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031401"
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>연습-Xamarin.ios에서 로컬 알림 사용

_이 섹션에서는 Xamarin.ios 응용 프로그램에서 로컬 알림을 사용 하는 방법을 안내 합니다. 앱에서 수신 될 때 경고를 표시 하는 알림을 만들고 게시 하는 기본 사항을 설명 합니다._

> [!IMPORTANT]
> 이 섹션의 정보는 iOS 9 및 이전 버전과 관련이 있으며 이전 iOS 버전을 지원 하기 위해 여기에 남아 있습니다. IOS 10 이상에서는 iOS 장치에서 로컬 알림과 원격 알림을 모두 지원 하기 위한 [사용자 알림 프레임 워크 가이드](~/ios/platform/user-notifications/index.md) 를 참조 하세요.

## <a name="walkthrough"></a>연습

로컬 알림을 작동 중인 것으로 표시 하는 간단한 응용 프로그램을 만듭니다. 이 응용 프로그램에는 단일 단추가 있습니다. 단추를 클릭 하면 로컬 알림이 생성 됩니다. 지정 된 기간이 경과 된 후 알림이 표시 됩니다.

1. Mac용 Visual Studio에서 새 단일 뷰 iOS 솔루션을 만들고 `Notifications`호출 합니다.
1. `Main.storyboard` 파일을 열고 단추를 뷰로 끌어 옵니다. 단추 **단추의**이름을 지정 하 고 제목 **추가 알림을**지정 합니다. 이 시점에서 단추에 몇 가지 [제약 조건을](~/ios/user-interface/designer/designer-auto-layout.md) 설정할 수도 있습니다. 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "Setting some constraints on the button")
1. `ViewController` 클래스를 편집 하 고 ViewDidLoad 메서드에 다음 이벤트 처리기를 추가 합니다.

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

    이 코드는 소리를 사용 하는 알림을 만들고 아이콘 배지 값을 1로 설정 하 고 사용자에 게 경고를 표시 합니다.

1. 그런 다음 `AppDelegate.cs`파일을 편집 하 여 먼저 `FinishedLaunching` 메서드에 다음 코드를 추가 합니다. 장치가 iOS 8을 실행 하 고 있는지 확인 합니다 .이 경우 사용자가 알림을 받을 수 있는 권한을 **요청 해야 합니다** .

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
        var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes (
            UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
        );

        application.RegisterUserNotificationSettings (notificationSettings);
    }
    ```

1. `AppDelegate.cs`에도 알림이 수신 될 때 호출 되는 다음 메서드를 추가 합니다.

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

1. 로컬 알림으로 인해 알림이 시작 된 경우를 처리 해야 합니다. 다음 코드 조각을 포함 하도록 `AppDelegate`에서 메서드 `FinishedLaunching`를 편집 합니다.

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

1. 마지막으로 응용 프로그램을 실행 합니다. IOS 8에서 알림을 허용 하 라는 메시지가 표시 됩니다. **확인** 을 클릭 한 다음 **알림 추가** 단추를 클릭 합니다. 잠시 후에 다음 스크린샷에 표시 된 것 처럼 경고 대화 상자가 표시 됩니다.

    ![](local-notifications-in-ios-walkthrough-images/image0.png "알림 전송 기능 확인") ![](local-notifications-in-ios-walkthrough-images/image1.png "알림 추가 단추")
    ![](local-notifications-in-ios-walkthrough-images/image2.png "The notification alert dialog")

## <a name="summary"></a>요약

이 연습에서는 iOS에서 알림을 만들고 게시 하기 위해 다양 한 API를 사용 하는 방법을 살펴보았습니다. 또한 응용 프로그램 아이콘을 배지로 업데이트 하 여 사용자에 게 몇 가지 응용 프로그램별 피드백을 제공 하는 방법을 보여 주었습니다.

## <a name="related-links"></a>관련 링크

- [로컬 알림 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/localnotifications)
- [로컬 및 푸시 알림 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
