---
title: Xamarin.ios의 알림 관리
description: 이 문서에서는 Xamarin.ios를 사용 하 여 iOS 12에 도입 된 새로운 알림 관리 기능을 활용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F1D90729-F85A-425B-B633-E2FA38FB4A0C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
ms.openlocfilehash: 671b6c00a41d719a7ccb8247fd4a7bc008d91adf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031900"
---
# <a name="notification-management-in-xamarinios"></a>Xamarin.ios의 알림 관리

IOS 12에서 운영 체제는 알림 센터 및 설정 앱에서 앱의 알림 관리 화면으로 딥 링크 될 수 있습니다. 이 화면에서는 사용자가 앱이 보내는 다양 한 알림 유형을 옵트인 및 옵트아웃 (opt in) 할 수 있습니다.

## <a name="sample-app-redgreennotifications"></a>샘플 앱: RedGreenNotifications

알림 관리의 작동 원리에 대 한 예제를 보려면 [RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications) 샘플 앱을 살펴보세요.

이 샘플 응용 프로그램은 두 가지 유형의 알림 (빨간색 및 녹색)을 보내고 사용자가 두 종류의 옵트인 또는 옵트아웃 (opt in) 할 수 있는 화면을 제공 합니다.

이 가이드의 코드 조각은이 샘플 앱에서 제공 됩니다.

## <a name="notification-management-screen"></a>알림 관리 화면

샘플 앱에서 `ManageNotificationsViewController`은 사용자가 빨간색 알림과 녹색 알림을 독립적으로 사용 하거나 사용 하지 않도록 설정할 수 있는 사용자 인터페이스를 정의 합니다. 표준 [`UIViewController`](xref:UIKit.UIViewController)
각 알림 유형에 대 한 [`UISwitch`](xref:UIKit.UISwitch) 를 포함 하는입니다. 이러한 알림 유형에 대 한 스위치를 설정/해제 하면 사용자 기본값에서 해당 알림 유형에 대 한 사용자의 기본 설정이 저장 됩니다.

```csharp
partial void HandleRedNotificationsSwitchValueChange(UISwitch sender)
{
    NSUserDefaults.StandardUserDefaults.SetBool(sender.On, RedNotificationsEnabledKey);
}
```

> [!NOTE]
> 또한 알림 관리 화면에서는 사용자가 앱에 대 한 알림을 완전히 비활성화 했는지 여부를 확인 합니다. 그렇다면 개별 알림 유형에 대 한 토글을 숨깁니다. 이렇게 하려면 알림 관리 화면에서 다음을 수행 합니다.
>
> - [`UNUserNotificationCenter.Current.GetNotificationSettingsAsync`](xref:UserNotifications.UNUserNotificationCenter.GetNotificationSettingsAsync) 를 호출 하 고 [`AuthorizationStatus`](xref:UserNotifications.UNNotificationSettings.AuthorizationStatus) 속성을 검사 합니다.
> - 앱에 대 한 알림이 완전히 사용 하지 않도록 설정 된 경우 개별 알림 유형에 대 한 토글을 숨깁니다.
> - 사용자가 언제 든 지 iOS 설정에서 알림을 사용 하거나 사용 하지 않도록 설정할 수 있으므로 응용 프로그램이 포그라운드로 이동할 때마다 알림이 사용 하지 않도록 설정 되었는지 다시 확인 합니다.

알림을 보내는 샘플 앱의 `ViewController` 클래스는 알림이 사용자가 실제로 받으려는 유형 인지 확인 하기 위해 로컬 알림을 보내기 전에 사용자의 기본 설정을 확인 합니다.

```csharp
partial void HandleTapRedNotificationButton(UIButton sender)
{
    bool redEnabled = NSUserDefaults.StandardUserDefaults.BoolForKey(ManageNotificationsViewController.RedNotificationsEnabledKey);
    if (redEnabled)
    {
        // ...
```

## <a name="deep-link"></a>딥 링크

iOS 딥 알림 센터의 앱 알림 관리 화면에 대 한 링크와 설정 앱의 앱 알림 설정입니다. 이를 용이 하 게 하려면 앱에서 다음을 수행 해야 합니다.

- 앱의 알림 권한 부여 요청에 `UNAuthorizationOptions.ProvidesAppNotificationSettings`를 전달 하 여 알림 관리 화면을 사용할 수 있음을 표시 합니다.
- [`IUNUserNotificationCenterDelegate`](xref:UserNotifications.IUNUserNotificationCenterDelegate)에서 `OpenSettings` 메서드를 구현 합니다.

### <a name="authorization-request"></a>권한 부여 요청

운영 체제에 알림 관리 화면이 사용 가능 하다는 것을 나타내려면 앱에서 `UNUserNotificationCenter`의 `RequestAuthorization` 방법으로 `UNAuthorizationOptions.ProvidesAppNotificationSettings` 옵션 (필요한 다른 알림 배달 옵션과 함께)을 전달 해야 합니다.

예를 들어 샘플 앱 `AppDelegate`에서 다음을 수행 합니다.

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

응용 프로그램의 알림 관리 화면에 대 한 시스템 심층 링크에 의해 호출 되는 `OpenSettings` 메서드는 사용자가 해당 화면으로 직접 이동 해야 합니다.

샘플 앱에서이 메서드는 필요한 경우 `ManageNotificationsViewController`에 segue를 수행 합니다.

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

- [샘플 앱-RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Xamarin.ios의 사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [사용자 알림의 새로운 기능 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [모범 사례 및 사용자 알림의 새로운 기능 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [원격 알림 생성 (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
