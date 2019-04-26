---
title: Xamarin.iOS에서 동적 알림 실행 단추
description: IOS 12 사용 하 여 알림 콘텐츠 확장 수 추가, 제거 및 알림을 함께 표시 되는 작업 단추를 업데이트 합니다. 이 문서에서는 Xamarin.iOS를 사용 하 여 동적 알림 실행 단추를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 6B34AD78-5117-42D0-B6E7-C8B4B453EAFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/04/2018
ms.openlocfilehash: 5611d673ecc7af896fd3a9e566e184e408b6b367
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60876060"
---
# <a name="dynamic-notification-action-buttons-in-xamarinios"></a>Xamarin.iOS에서 동적 알림 실행 단추

Ios 12에서 알림 수 동적으로 추가, 제거 및 해당 관련된 실행 단추를 업데이트 합니다. 이러한 사용자 지정을 통해 알림 콘텐츠 및 사용자의 상호 작용에 직접 관련 된 작업을 사용 하 여 사용자를 제공할 수 있습니다.

## <a name="sample-app-redgreennotifications"></a>샘플 앱: RedGreenNotifications

이 가이드의 코드 조각에서 제공 합니다 [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications) Xamarin.iOS를 사용 하 여 iOS 12의에서 알림 실행 단추를 사용 하 여 작업 하는 방법을 보여 주는 샘플 앱입니다.

이 샘플 앱은 두 가지 유형의 로컬 알림 보냅니다: 빨강, 녹색입니다.
앱 보내기 알림을 알게 된 후 해당 사용자 지정 사용자 인터페이스를 보려는 3D 터치를 사용 합니다. 그런 다음 표시 되는 이미지를 회전 하려면 알림의 작업 단추를 사용 합니다. 이미지를 회전할 때를 **회전 재설정** 단추가 표시 되 고 필요에 따라 사라집니다.

이 샘플 앱에서이 가이드에서 코드 조각을 제공 됩니다.

## <a name="default-action-buttons"></a>기본 작업 단추

알림을 범주는 해당 기본 실행 단추를 결정합니다.

만들고 응용 프로그램을 시작 하는 동안에 알림 범주를 등록 합니다.
예를 들어 합니다 [샘플 앱](#sample-app-redgreennotifications)의 `FinishedLaunching` 메서드의 `AppDelegate` 다음을 수행:

- 빨간색 알림에 대 한 하나의 범주 및 녹색 알림에 대 한 다른 정의
- 호출 하 여 이러한 범주를 등록 합니다 [`SetNotificationCategories`](xref:UserNotifications.UNUserNotificationCenter.SetNotificationCategories*)
메서드 `UNUserNotificationCenter`
- 단일 연결 [`UNNotificationAction`](xref:UserNotifications.UNNotificationAction)
각 범주에

다음 샘플 코드 작동 방식을 보여 줍니다.

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

이 코드에서는 모든 알림 갖는 기반 [`Content.CategoryIdentifier`](xref:UserNotifications.UNNotificationContent.CategoryIdentifier)
"빨간색-범주" 또는 "녹색 범주"는 기본적으로 표시 되는 **20도 회전** 작업 단추입니다.

## <a name="in-app-handling-of-notification-action-buttons"></a>앱에 알림 작업 단추 처리

`UNUserNotificationCenter` 에 `Delegate` 형식의 속성 [ `IUNUserNotificationCenterDelegate` ](xref:UserNotifications.IUNUserNotificationCenterDelegate)합니다.

샘플 앱에서 `AppDelegate` 자체에 대 한 사용자 알림 센터의 대리자로 설정 `FinishedLaunching`:

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

그런 다음 `AppDelegate` 구현 [`DidReceiveNotificationResponse`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.DidReceiveNotificationResponse*)
실행 단추를 처리 하도록 다음을 탭 합니다.

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

이 구현의 `DidReceiveNotificationResponse` 알림의 처리 하지 않습니다 **20도 회전** 작업 단추입니다. 대신, 알림 콘텐츠 확장 탭이 단추를 처리합니다. 다음 섹션에서는 추가 알림 작업 단추 처리를 설명합니다.

## <a name="action-buttons-in-the-notification-content-extension"></a>알림 콘텐츠 확장의 실행 단추

알림 콘텐츠 확장 알림에 대 한 사용자 지정 인터페이스를 정의 하는 뷰 컨트롤러를 포함 합니다.

이 뷰 컨트롤러를 사용할 수는 `GetNotificationActions` 고 `SetNotificationActions` 메서드는 [`ExtensionContext`](xref:UIKit.UIViewController.ExtensionContext)
속성 액세스 및 수정 알림의 작업 단추입니다.

샘플 앱에서 알림 콘텐츠 확장의 뷰 컨트롤러는 기존 작업 단추 탭에 응답 하는 경우에 실행 단추를 수정 합니다.

> [!NOTE]
> 알림 콘텐츠 확장에서 해당 뷰 컨트롤러는 작업 단추 탭으로 응답할 수 있습니다 [ `DidReceiveNotificationResponse` ](xref:UserNotificationsUI.UNNotificationContentExtension_Extensions.DidReceiveNotificationResponse*) 메서드를의 일부로 선언 [IUNNotificationContentExtension](xref:UserNotificationsUI.IUNNotificationContentExtension)합니다.
>
> 와 이름을 공유 하지만 합니다 `DidReceiveNotificationResponse` 메서드 [위에서 설명한](#in-app-handling-of-notification-action-buttons), 다른 메서드입니다.
>
> 알림 콘텐츠 확장 단추를 눌러 처리 완료 되 면 기본 응용 프로그램에는 동일한 단추 탭을 처리 하도록 지시를 여부를 선택할 수 있습니다. 이렇게 하려면 적절 한 값을 전달 해야 합니다 [UNNotificationContentExtensionResponseOption](xref:UserNotificationsUI.UNNotificationContentExtensionResponseOption) 완료 처리기에:
>
> - `Dismiss` 알림 인터페이스를 해제 해야, 및 기본 앱 단추 탭을 처리할 필요가 없습니다를 나타냅니다.
> - `DismissAndForwardAction` 알림 인터페이스를 해제 해야, 및는 기본 앱도 처리 하는 단추 탭을 나타냅니다.
> - `DoNotDismiss` 알림 인터페이스를 해제 하지 해야, 및 기본 앱 단추 탭을 처리할 필요가 없습니다를 나타냅니다.

콘텐츠 확장의 `DidReceiveNotificationResponse` 메서드는 작업 단추를 눌렀는지 확인, 알림 인터페이스를 표시 하거나 숨깁니다에서 이미지를 회전을 **재설정** 작업 단추:

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

이 경우이 메서드를 전달 `UNNotificationContentExtensionResponseOption.DoNotDismiss` 완료 처리기에 있습니다. 이 알림의 인터페이스 개방를 의미 합니다.

## <a name="related-links"></a>관련 링크

- [샘플 앱 – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Xamarin.iOS에서 사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md)
- [실행 가능한 알림 형식을 선언합니다.](https://developer.apple.com/documentation/usernotifications/declaring_your_actionable_notification_types?language=objc)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [사용자 알림 (WWDC 2018)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2018/710/)
- [모범 사례 및 사용자 알림 (WWDC 2017)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2017/708/)
- [원격 알림을 (Apple) 생성](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
