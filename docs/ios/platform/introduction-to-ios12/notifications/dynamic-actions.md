---
title: Xamarin.ios의 동적 알림 작업 단추
description: IOS 12를 사용 하는 알림 콘텐츠 확장은 알림과 함께 표시 되는 작업 단추를 추가, 제거 및 업데이트할 수 있습니다. 이 문서에서는 Xamarin.ios와 함께 동적 알림 작업 단추를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 6B34AD78-5117-42D0-B6E7-C8B4B453EAFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/04/2018
ms.openlocfilehash: 1a4380e321035b8948f9b40bdce052161025d5f3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652589"
---
# <a name="dynamic-notification-action-buttons-in-xamarinios"></a>Xamarin.ios의 동적 알림 작업 단추

IOS 12에서 알림은 연결 된 작업 단추를 동적으로 추가, 제거 및 업데이트할 수 있습니다. 이러한 사용자 지정을 통해 사용자에 게 알림 콘텐츠와 직접 관련 된 작업과 사용자의 상호 작용을 제공할 수 있습니다.

## <a name="sample-app-redgreennotifications"></a>샘플 앱: RedGreenNotifications

이 가이드의 코드 조각은 [RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications) 샘플 앱에서 제공 됩니다 .이 샘플 앱에서는 ios 12에서 xamarin.ios를 사용 하 여 알림 작업 단추를 사용 하는 방법을 보여 줍니다.

이 샘플 앱은 두 가지 유형의 로컬 알림 (빨간색 및 녹색)을 보냅니다.
앱에서 알림을 보낸 후 3D 터치를 사용 하 여 사용자 지정 사용자 인터페이스를 확인 합니다. 그런 다음 알림의 작업 단추를 사용 하 여 표시 되는 이미지를 회전 합니다. 이미지가 회전 하면 필요에 따라 **회전 다시 설정** 단추가 표시 되 고 사라집니다.

이 가이드의 코드 조각은이 샘플 앱에서 제공 됩니다.

## <a name="default-action-buttons"></a>기본 작업 단추

알림의 범주가 기본 작업 단추를 결정 합니다.

응용 프로그램이 시작 되는 동안 알림 범주를 만들고 등록 합니다.
예를 들어 [샘플 앱](#sample-app-redgreennotifications)에서의 `AppDelegate` 메서드는 `FinishedLaunching` 다음을 수행 합니다.

- 빨간색 알림에 대 한 범주 하나를 정의 하 고 다른 범주를 녹색 알림으로 정의 합니다.
- 다음을 호출 하 여 이러한 범주를 등록 합니다.[`SetNotificationCategories`](xref:UserNotifications.UNUserNotificationCenter.SetNotificationCategories*)
메서드`UNUserNotificationCenter`
- 단일를 연결 합니다.[`UNNotificationAction`](xref:UserNotifications.UNNotificationAction)
각 범주에

다음 샘플 코드에서는이 작업을 수행 하는 방법을 보여 줍니다.

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional | UNAuthorizationOptions.ProvidesAppNotificationSettings;
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        // ...
        var rotateTwentyDegreesAction = UNNotificationAction.FromIdentifier("rotate-twenty-degrees-action", "Rotate 20°", UNNotificationActionOptions.None);

        var redCategory = UNNotificationCategory.FromIdentifier(
            "red-category",
            new UNNotificationAction[] { rotateTwentyDegreesAction },
            new string[] { },
            UNNotificationCategoryOptions.CustomDismissAction
        );

        var greenCategory = UNNotificationCategory.FromIdentifier(
            "green-category",
            new UNNotificationAction[] { rotateTwentyDegreesAction },
            new string[] { },
            UNNotificationCategoryOptions.CustomDismissAction
        );

        var set = new NSSet<UNNotificationCategory>(redCategory, greenCategory);
        center.SetNotificationCategories(set);
    });
    // ...
}
```

해당 코드를 기반으로 하는 모든 알림[`Content.CategoryIdentifier`](xref:UserNotifications.UNNotificationContent.CategoryIdentifier)
는 "빨간색-범주" 또는 "녹색 범주"는 기본적으로 **20 ° 회전** 동작 단추를 표시 합니다.

## <a name="in-app-handling-of-notification-action-buttons"></a>알림 작업 단추의 앱 내 처리

`UNUserNotificationCenter`에는 형식의 [`IUNUserNotificationCenterDelegate`](xref:UserNotifications.IUNUserNotificationCenterDelegate) 속성이있습니다.`Delegate`

샘플 앱에서는 자체 `AppDelegate` 를 사용자 알림 센터의 `FinishedLaunching`대리자로 설정 합니다.

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = // ...
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        center.Delegate = this;
        // ...
```

그런 다음 `AppDelegate` 을 구현 합니다.[`DidReceiveNotificationResponse`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.DidReceiveNotificationResponse*)
작업 단추 탭을 처리 하려면 다음을 수행 합니다.

```csharp
[Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
{
    if (response.IsDefaultAction)
    {
        Console.WriteLine("ACTION: Default");
    }
    if (response.IsDismissAction)
    {
        Console.WriteLine("ACTION: Dismiss");
    }
    else
    {
        Console.WriteLine($"ACTION: {response.ActionIdentifier}");
    }

    completionHandler();
        }
```

이 구현 `DidReceiveNotificationResponse` 에서는 알림의 **20 ° 회전** 동작 단추가 처리 되지 않습니다. 대신 알림의 콘텐츠 확장은이 단추의 탭을 처리 합니다. 다음 섹션에서는 알림 작업 단추 처리에 대해 자세히 설명 합니다.

## <a name="action-buttons-in-the-notification-content-extension"></a>알림 콘텐츠 확장의 작업 단추

알림 콘텐츠 확장은 알림에 대 한 사용자 지정 인터페이스를 정의 하는 보기 컨트롤러를 포함 합니다.

이 뷰 컨트롤러는의 및 `GetNotificationActions` `SetNotificationActions` 메서드를 사용할 수 있습니다.[`ExtensionContext`](xref:UIKit.UIViewController.ExtensionContext)
알림 작업 단추를 액세스 하 고 수정 하는 속성입니다.

샘플 앱에서 알림 콘텐츠 확장의 뷰 컨트롤러는 이미 존재 하는 작업 단추를 탭 하 여 응답 하는 경우에만 작업 단추를 수정 합니다.

> [!NOTE]
> 알림 콘텐츠 확장 프로그램은 [iunnotificationcontentextension](xref:UserNotificationsUI.IUNNotificationContentExtension)의 일부로 선언 된 해당 뷰 [`DidReceiveNotificationResponse`](xref:UserNotificationsUI.UNNotificationContentExtension_Extensions.DidReceiveNotificationResponse*) 컨트롤러의 메서드에 있는 작업 단추 탭에 응답할 수 있습니다.
>
> `DidReceiveNotificationResponse` [위에서 설명한](#in-app-handling-of-notification-action-buttons)메서드와 이름을 공유 하지만이는 다른 메서드입니다.
>
> 알림 콘텐츠 확장 프로그램이 단추 탭 처리를 완료 한 후에는 주 응용 프로그램이 동일한 단추 탭을 처리 하도록 지시할 지 여부를 선택할 수 있습니다. 이렇게 하려면 [Unnotificationcontentextensionresponseoption](xref:UserNotificationsUI.UNNotificationContentExtensionResponseOption) 의 적절 한 값을 완료 처리기에 전달 해야 합니다.
>
> - `Dismiss`알림 인터페이스를 해제 해야 하며, 기본 앱이 단추 탭을 처리할 필요가 없음을 나타냅니다.
> - `DismissAndForwardAction`알림 인터페이스를 해제 해야 하며, 기본 앱에서 단추 탭도 처리 해야 함을 나타냅니다.
> - `DoNotDismiss`알림 인터페이스를 해제 해서는 안 되며 기본 앱이 단추 탭을 처리할 필요가 없음을 나타냅니다.

콘텐츠 확장의 `DidReceiveNotificationResponse` 메서드는 탭 된 작업 단추를 확인 하 고, 알림의 인터페이스에서 이미지를 회전 하 고, 작업 **다시 설정** 단추를 표시 하거나 숨깁니다.

```csharp
[Export("didReceiveNotificationResponse:completionHandler:")]
public void DidReceiveNotificationResponse(UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
{
    var rotationAction = ExtensionContext.GetNotificationActions()[0];

    if (response.ActionIdentifier == "rotate-twenty-degrees-action")
    {
        rotationButtonTaps += 1;

        double radians = (20 * rotationButtonTaps) * (2 * Math.PI / 360.0);
        Xamagon.Transform = CGAffineTransform.MakeRotation((float)radians);

        // 9 rotations * 20 degrees = 180 degrees. No reason to
        // show the reset rotation button when the image is half
        // or fully rotated.
        if (rotationButtonTaps % 9 == 0)
        {
            ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction });
        }
        else if (rotationButtonTaps % 9 == 1)
        {
            var resetRotationAction = UNNotificationAction.FromIdentifier("reset-rotation-action", "Reset rotation", UNNotificationActionOptions.None);
            ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction, resetRotationAction });
        }
    }

    if (response.ActionIdentifier == "reset-rotation-action")
    {
        rotationButtonTaps = 0;

        double radians = (20 * rotationButtonTaps) * (2 * Math.PI / 360.0);
        Xamagon.Transform = CGAffineTransform.MakeRotation((float)radians);

        ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction });
    }

    completionHandler(UNNotificationContentExtensionResponseOption.DoNotDismiss);
}
```

이 경우 메서드는 완료 처리기에 `UNNotificationContentExtensionResponseOption.DoNotDismiss` 전달 됩니다. 이는 알림의 인터페이스가 열린 상태를 유지 하는 것을 의미 합니다.

## <a name="related-links"></a>관련 링크

- [샘플 앱-RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Xamarin.ios의 사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md)
- [조치 가능한 알림 유형 선언](https://developer.apple.com/documentation/usernotifications/declaring_your_actionable_notification_types?language=objc)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [사용자 알림의 새로운 기능 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [모범 사례 및 사용자 알림의 새로운 기능 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [원격 알림 생성 (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
