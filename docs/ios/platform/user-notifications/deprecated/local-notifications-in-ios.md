---
title: "Xamarin.iOS의 알림"
description: "이 섹션에는 로컬 알림을 Xamarin.iOS에서 구현 하는 방법을 보여 줍니다. IOS 알림의 다양 한 UI 요소에 설명 하 고 API에 설명 합니다의 관련 만들기 및 알림을 표시 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 2a8ae55f9cc3e2dd4818dec96a35017c76cc9623
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="notifications-in-xamarinios"></a>Xamarin.iOS의 알림

_이 섹션에는 로컬 알림을 Xamarin.iOS에서 구현 하는 방법을 보여 줍니다. IOS 알림의 다양 한 UI 요소에 설명 하 고 API에 설명 합니다의 관련 만들기 및 알림을 표시 합니다._

> [!IMPORTANT]
> **참고:** iOS 9에 관련 된이 섹션의 정보 및 prior 되지 않았습니다 여기 이전 iOS 버전을 지원 하도록 합니다. IOS 10 이상에서 참조 하십시오는 [사용자 알림 프레임 워크 가이드](~/ios/platform/user-notifications/index.md) iOS 장치에서 로컬 및 원격 알림 지원에 대 한 합니다.

iOS에 알림이 받았음을 사용자에 게 나타내기 위해 세 가지가 있습니다.

-  **사운드 또는 진동** -iOS 사용자에 게 소리를 재생할 수 있습니다. 소리가 비활성화 된 경우 장치를 진동 할 구성할 수 있습니다.
-  **경고** -알림에 대 한 정보로 화면에 대화 상자를 표시 하는 것이 불가능 합니다.
-  **배지** 알림을 게시 되 면-숫자 (내부)의 응용 프로그램 아이콘에 표시 될 수 있습니다.


iOS 제공는 *알림 센터* 알림도 모두 함께, 로컬 및 원격 사용자에 게 표시할 합니다. 사용자가 화면 맨 위에서 아래로 살짝 밀어이 액세스할 수 있습니다.

 ![](local-notifications-in-ios-images/image13.png "알림 센터")

## <a name="creating-local-notifications-in-ios"></a>IOS에서 로컬 알림 만들기

iOS를 사용 하면 간단 하 게 만들고 로컬 알림을 처리할 수 있습니다.
첫째, iOS 8 필요 응용 프로그램 알림 표시를 사용자의 사용 권한을 요청 해야 합니다. 로컬 알림을 보내려고 시도 하기 전에 앱에 다음 코드를 추가-연결 된 샘플에 배치 된 **AppDelegate**의 **FinishedLaunching** 메서드.

```csharp
var settings = UIUserNotificationSettings.GetSettingsForTypes(
  UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound
  , null);
UIApplication.SharedApplication.RegisterUserNotificationSettings (settings);
```

  [![](local-notifications-in-ios-images/image0-sml.png "로컬 알림을 보낼 수를 확인 합니다.")](local-notifications-in-ios-images/image0.png#lightbox)

로컬 알림을 만들면 예약 하는 `UILocalNotification` 개체, 설정는 `FireDate`, 하 고 예약을 통해는 `ScheduleLocalNotification` 에서 메서드는 `UIApplication.SharedApplication` 개체입니다. 다음 코드 조각 앞으로 1 분에 실행 되 고 메시지와 함께 경고를 표시 하는 알림은 예약 하는 방법을 표시 됩니다.

```csharp
UILocalNotification notification = new UILocalNotification();
NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

다음 스크린샷은이 경고의 모양을 보여 줍니다.

  [![](local-notifications-in-ios-images/image2-sml.png "예제에서는 경고")](local-notifications-in-ios-images/image2.png#lightbox)

사용자를 선택 하는 경우 *없도록* 알림을 아무 것도 표시 됩니다.

숫자로 배지는 응용 프로그램 아이콘에 적용 하려는 경우 다음 코드 줄에에서 표시 된 대로를 설정할 수 있습니다.

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

다음 코드 조각에 나와 있는 것 처럼 순서 play에서 아이콘을와 소리 알림 SoundName 속성 설정:

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

Apple Human Interface Guidelines 당 알림을 소리를 재생 하는 경우 그 해야도 함께 배지 또는 사용자는 경고를 발생 시키는 응용 프로그램을 식별할 수 있도록 경고 합니다. 또한 소리가 30 초 보다 긴 경우 iOS 됩니다 기본 소리 재생 대신 합니다.

> [!IMPORTANT]
> **참고**: 대리자 알림을 두 번 발생 하는 iOS 시뮬레이터에서 버그가 있습니다. 장치에서 응용 프로그램을 실행 하는 경우에이 문제가 발생 하지 않습니다.

## <a name="handling-notifications"></a>알림 처리

iOS 응용 프로그램에는 거의 동일한 방식으로 원격 및 로컬 알림을 처리 합니다. 응용 프로그램이 실행 중인 경우는 `ReceivedLocalNotification` 메서드 또는 `ReceivedRemoteNotification` AppDelegate 클래스 메서드는 호출 하 고 알림 정보를 매개 변수로 전달 됩니다.

응용 프로그램에 다양 한 방식에서 알림을 처리할 수 있습니다. 예를 들어, 응용 프로그램 일부 이벤트에 대 한 사용자에 게 하는 경고를 표시 될 수 있습니다. 또는 알림을 동기화 파일 서버에 같은 프로세스가 완료 되었음을 사용자에 게 경고를 표시 하도록 사용할 수도 있습니다.

다음 코드에는 로컬 알림 메시지를 처리 하 고 경고 메시지가 표시 번호는 0으로 다시 설정 하는 방법을 보여 줍니다.

```csharp
 public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
        {
            // show an alert
            UIAlertController okayAlertController = UIAlertController.Create (notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
            okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
            viewController.PresentViewController (okayAlertController, true, null);

            // reset our badge
            UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
        }
```

응용 프로그램이 실행 되지 않는 경우 iOS는 소리를 재생 및/또는 해당 아이콘 배지를 업데이트 합니다. 사용자는 경고와 관련 된 응용 프로그램 시작 되 면은 시작 하 고 응용 프로그램 대리자에 대 한 FinishedLaunching 메서드는 호출 하 고 알림 정보 옵션 매개 변수를 통해 전달 됩니다. 옵션 사전 키를 포함 하는 경우 `UIApplication.LaunchOptionsLocalNotificationKey`는 AppDelegate 알 응용 프로그램이 로컬 알림을에서 시작 되었습니다. 다음 코드 조각에서는이 프로세스를 보여 줍니다.

```csharp
// check for a local notification
if (options.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
 {
     var localNotification = options[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
     if (localNotification != null)
     {
        UIAlertController okayAlertController = UIAlertController.Create (localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
        viewController.PresentViewController (okayAlertController, true, null);

         // reset our badge
         UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
     }
 }
```

옵션 사전에는 대신 원격 알림 이면는 `LaunchOptionsRemoteNotificationKey`, 그 키에 대 한 결과 값 및는 `NSDictionary` 원격 알림 페이로드 개체입니다. 알림과 배지, 소리 키를 통해 알림 페이로드를 추출할 수 있습니다. 다음 코드 조각에는 원격 알림을 가져오는 방법을 보여 줍니다.

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>요약

이 섹션에는 만들고 Xamarin.iOS에서 알림을 게시 하는 방법을 배웠습니다. 알림 재정의 하 여 응용 프로그램 수 대응 하는 방법을 표시는 `ReceivedLocalNotification` 메서드 또는 `ReceivedRemoteNotification` 에서 메서드는 `AppDelegate`합니다.


## <a name="related-links"></a>관련 링크

- [로컬 알림 (샘플)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [로컬 및 개발자에 대 한 알림을 푸시](https://developer.apple.com/notifications/)
- [로컬 및 푸시 알림 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
