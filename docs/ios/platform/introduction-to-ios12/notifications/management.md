---
title: Xamarin.iOS에서 알림 관리
description: 이 문서에서는 Xamarin.iOS를 사용 하 여 iOS 12에에서 도입 된 새 알림 관리 기능을 활용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F1D90729-F85A-425B-B633-E2FA38FB4A0C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: 96fce269784ad0ac41fd1685ac7ac6b957932bd8
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233174"
---
# <a name="notification-management-in-xamarinios"></a>Xamarin.iOS에서 알림 관리

Ios 12에서 운영 체제에는 알림 센터에서 딥 링크 및 설정 앱에서 앱의 알림 관리 화면을 수 있습니다. 이 화면을 사용 하는 사용자와 앱에서 다양 한 유형의 알림 보냅니다.

## <a name="sample-app-redgreennotifications"></a>샘플 앱: RedGreenNotifications

알림 관리 작동 하는 방법의 예제를 보려면 잠시 살펴 합니다 [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications) 샘플 앱입니다.

이 샘플 앱 – 빨강 및 녹색 – 두 가지 유형의 알림 보냅니다 하 고 참여 하거나 참여 하지 두 종류 할 수 있는 화면을 제공 합니다.

이 샘플 앱에서이 가이드에서 코드 조각을 제공 됩니다.

## <a name="notification-management-screen"></a>알림 관리 화면

샘플 앱에서 `ManageNotificationsViewController` 독립적으로 사용 하도록 설정 하 고 빨간색 알림 및 녹색 알림을 사용 하지 않도록 설정할 수 있는 사용자 인터페이스를 정의 합니다. 이것은 표준 [`UIViewController`](xref:UIKit.UIViewController)
포함 하는 [ `UISwitch` ](xref:UIKit.UISwitch) 각 알림 형식에 대 한 합니다. 알림의 형식 중 하나에 대 한 스위치를 설정/해제 저장, 사용자 기본값에 해당 알림 유형에 대 한 사용자의 기본 설정:

```csharp
partial void HandleRedNotificationsSwitchValueChange(UISwitch sender)
{
    NSUserDefaults.StandardUserDefaults.SetBool(sender.On, RedNotificationsEnabledKey);
}
```

> [!NOTE]
> 알림 관리 화면 사용자 앱에 대 한 알림을 완전히 비활성화 여부를 확인 합니다. 그렇다면 개별 알림 형식에 대 한 토글을 숨깁니다. 이렇게 하려면 알림 관리 화면:
>
> - 호출 [ `UNUserNotificationCenter.Current.GetNotificationSettingsAsync` ](xref:UserNotifications.UNUserNotificationCenter.GetNotificationSettingsAsync) 검사를 [ `AuthorizationStatus` ](xref:UserNotifications.UNNotificationSettings.AuthorizationStatus) 속성입니다.
> - 앱에 대 한 알림을 완전히 비활성화 된 경우에 대 한 개별 알림 유형 설정/해제를 숨깁니다.
> - 다시 하므로 사용자 수 활성화/비활성화 알림 iOS 설정에서에서 언제 든 지 응용 프로그램을 전경에 이동할 때마다 알림을 해제 되었는지 여부를 확인 합니다.

샘플 앱의 `ViewController` 클래스를 수신 하려는 실제로 되도록 알림 형식의 사용자 로컬 알림을 보내기 전에 사용자가 원하는 알림을 확인의 보냅니다.

```csharp
partial void HandleTapRedNotificationButton(UIButton sender)
{
    bool redEnabled = NSUserDefaults.StandardUserDefaults.BoolForKey(ManageNotificationsViewController.RedNotificationsEnabledKey);
    if (redEnabled)
    {
        // ...
```

## <a name="deep-link"></a>딥 링크

iOS 딥 링크 앱의 알림 관리 화면으로 알림 센터에서 설정 앱의 앱 알림 설정 합니다. 이 작업을 위해 다음과 같은 앱 같아야 합니다.

- 알림 관리 화면을 전달 하 여 사용할 수 있는지를 나타내는 `UNAuthorizationOptions.ProvidesAppNotificationSettings` 앱의 알림 인증 요청을 합니다.
- 구현 된 `OpenSettings` 메서드에서 [ `IUNUserNotificationCenterDelegate` ](xref:UserNotifications.IUNUserNotificationCenterDelegate)합니다.

### <a name="authorization-request"></a>권한 부여 요청

알림 관리 화면을 사용할 수 있는 앱을 전달 해야 하는 운영 체제에 알리기 위해 합니다 `UNAuthorizationOptions.ProvidesAppNotificationSettings` (함께 다른 알림 배달 옵션 필요) 옵션을 `RequestAuthorization` 메서드를 `UNUserNotificationCenter`합니다.

샘플 앱의 예를 들어 `AppDelegate`:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.ProvidesAppNotificationSettings | UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional;
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        // ...
```

### <a name="opensettings-method"></a>OpenSettings 메서드

`OpenSettings` 딥 링크 앱의 알림 관리 화면을 위해 시스템에서 호출 하는 메서드를 해당 화면으로 직접 사용자를 이동 해야 합니다.

샘플 앱에서이 메서드를 segue를 수행 합니다 `ManageNotificationsViewController` 필요한 경우:

```csharp
[Export("userNotificationCenter:openSettingsForNotification:")]
public void OpenSettings(UNUserNotificationCenter center, UNNotification notification)
{
    var navigationController = Window.RootViewController as UINavigationController;
    if (navigationController != null)
    {
        var currentViewController = navigationController.VisibleViewController;
        if (currentViewController is ViewController)
        {
            currentViewController.PerformSegue(ManageNotificationsViewController.ShowManageNotificationsSegue, this);
        }

    }
}
```

## <a name="related-links"></a>관련 링크

- [샘플 앱 – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Xamarin.iOS에서 사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [사용자 알림 (WWDC 2018)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2018/710/)
- [모범 사례 및 사용자 알림 (WWDC 2017)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2017/708/)
- [원격 알림을 (Apple) 생성](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
