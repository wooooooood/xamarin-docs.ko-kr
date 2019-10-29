---
title: Xamarin.ios의 그룹화 된 알림
description: IOS 12를 사용 하면 응용 프로그램 또는 스레드별로 알림 센터 또는 잠금 화면에서 알림을 그룹화 할 수 있습니다. 이 문서에서는 Xamarin.ios를 사용 하 여 스레드 및 스레드 없는 알림을 보내는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: C6FA7C25-061B-4FD7-8E55-88597D512F3C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
ms.openlocfilehash: 6352de1483aea49a628cbb30d104906fde767afa
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031949"
---
# <a name="grouped-notifications-in-xamarinios"></a>Xamarin.ios의 그룹화 된 알림

기본적으로 iOS 12는 모든 앱의 알림을 그룹에 배치 합니다. 잠금 화면 및 알림 센터는이 그룹을 맨 위에 가장 최근의 알림이 있는 스택으로 표시 합니다. 사용자는 그룹을 확장 하 여 포함 된 모든 알림을 표시 하 고 그룹 전체를 해제할 수 있습니다.

앱은 스레드를 기준으로 알림을 그룹화 할 수도 있으므로 사용자가 관심 있는 특정 정보를 쉽게 찾고 상호 작용할 수 있습니다.

## <a name="sample-app-groupednotifications"></a>샘플 앱: GroupedNotifications

Xamarin.ios를 사용 하 여 그룹화 된 알림을 사용 하는 방법에 대 한 자세한 내용은 [Groupednotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-groupednotifications) 샘플 앱을 참조 하세요.

이 샘플 앱은 다양 한 친구와의 대화를 시뮬레이션 하 여 각 메시지에 대 한 알림을 보내고 스레드별로 그룹화 합니다. 또한 응용 프로그램 수준 그룹에서 스레드 알림이 발생 하지 않도록 하는 방법도 보여 줍니다.

이 가이드의 코드 조각은이 샘플 앱에서 제공 됩니다.

## <a name="request-authorization-and-allow-foreground-notifications"></a>권한 부여 요청 및 포그라운드 알림 허용

앱이 로컬 알림을 보낼 수 있기 전에이에 대 한 권한을 요청 해야 합니다. 샘플 앱의 [`AppDelegate`](xref:UIKit.UIApplicationDelegate)에서 [`FinishedLaunching`](xref:UIKit.UIApplicationDelegate.FinishedLaunching(UIKit.UIApplication,Foundation.NSDictionary)) 메서드는이 권한을 요청 합니다.

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    center.RequestAuthorization(UNAuthorizationOptions.Alert, (bool success, NSError error) =>
    {
        // Set the Delegate regardless of success; users can modify their notification
        // preferences at any time in the Settings app.
        center.Delegate = this;
    });
    return true;
}
```

[`UNUserNotificationCenter`](xref:UserNotifications.UNUserNotificationCenter) 에 대 한 [`Delegate`](xref:UserNotifications.UNUserNotificationCenter.Delegate) (위에서 설정)는 [`WillPresentNotification`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.WillPresentNotification(UserNotifications.IUNUserNotificationCenterDelegate,UserNotifications.UNUserNotificationCenter,UserNotifications.UNNotification,System.Action{UserNotifications.UNNotificationPresentationOptions}))에 전달 된 완료 처리기를 호출 하 여 포그라운드 앱에서 들어오는 알림을 표시할지 여부를 결정 합니다.

```csharp
[Export("userNotificationCenter:willPresentotification:withCompletionHandler:")]
public void WillPresentNotification(UNUserNotificationCenter center, UNNotification notification, System.Action<UNNotificationPresentationOptions> completionHandler)
{
    completionHandler(UNNotificationPresentationOptions.Alert);
}
```

[`UNNotificationPresentationOptions.Alert`](xref:UserNotifications.UNNotificationPresentationOptions) 매개 변수는 앱에서 경고를 표시 하지만 소리를 재생 하거나 배지를 업데이트 하지 않음을 나타냅니다.

## <a name="threaded-notifications"></a>스레드 알림

Alice 단추가 있는 샘플 앱의 **메시지** 를 반복적으로 탭 하 여 alice 라는 friend가 있는 대화에 대 한 알림을 보냅니다.
이 대화의 알림은 동일한 스레드에 속해 있으므로 잠금 화면과 알림 센터는 함께 그룹화 됩니다.

다른 친구와 대화를 시작 하려면 **새 친구 선택** 단추를 누릅니다. 이 대화에 대 한 알림은 별도의 그룹에 나타납니다.

### <a name="threadidentifier"></a>ThreadIdentifier

샘플 앱은 새 스레드를 시작할 때마다 고유한 스레드 식별자를 만듭니다.

```csharp
void StartNewThread()
{
    threadId = $"message-{friend}";
    // ...
}
```

스레드 알림을 보내기 위해 샘플 앱은 다음과 같습니다.

- 앱에 알림을 보낼 수 있는 권한이 있는지 여부를 확인 합니다.
- [`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent) 를 만듭니다.
알림 콘텐츠에 대 한 개체 이며 해당 [`ThreadIdentifier`](xref:UserNotifications.UNMutableNotificationContent.ThreadIdentifier) 을 설정 합니다.
위에서 만든 스레드 식별자입니다.
- 요청을 만들고 알림을 예약 합니다.

```csharp
async partial void ScheduleThreadedNotification(UIButton sender)
{
    var center = UNUserNotificationCenter.Current;

    UNNotificationSettings settings = await center.GetNotificationSettingsAsync();
    if (settings.AuthorizationStatus != UNAuthorizationStatus.Authorized)
    {
        return;
    }

    string author =  // ...
    string message = // ...

    var content = new UNMutableNotificationContent()
    {
        ThreadIdentifier = threadId,
        Title = author,
        Body = message,
        SummaryArgument = author
    };

    var request = UNNotificationRequest.FromIdentifier(
        Guid.NewGuid().ToString(),
        content,
        UNTimeIntervalNotificationTrigger.CreateTrigger(1, false)
    );

    center.AddNotificationRequest(request, null);

    // ...
}
```

동일한 스레드 식별자를 가진 동일한 앱의 모든 알림이 동일한 알림 그룹에 표시 됩니다.

> [!NOTE]
> 원격 알림에서 스레드 식별자를 설정 하려면 알림의 JSON 페이로드에 `thread-id` 키를 추가 합니다. 자세한 내용은 Apple의 [원격 알림 생성](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) 문서를 참조 하세요.

### <a name="summaryargument"></a>요약 인수

`SummaryArgument` 알림이 속한 알림 그룹의 왼쪽 아래 모퉁이에 표시 되는 요약 텍스트에 영향을 주는 방법을 지정 합니다. iOS는 동일한 그룹의 알림에서 요약 텍스트를 집계 하 여 전체 요약 설명을 만듭니다.

샘플 앱은 메시지 작성자를 요약 인수로 사용 합니다. 이 방법을 사용 하는 경우 Alice와의 6 가지 알림 그룹에 대 한 요약 텍스트는 **alice 및 Me에서 5 개 이상의 알림을**받을 수 있습니다.

## <a name="unthreaded-notifications"></a>스레드 없음 알림

샘플 앱의 **약속 미리 알림** 단추를 누를 때마다 다양 한 약속 미리 알림 알림 중 하나가 전송 됩니다. 이러한 미리 알림은 스레드 되지 않으므로 잠금 화면과 알림 센터의 응용 프로그램 수준 알림 그룹에 표시 됩니다.

스레드 없는 알림을 보내기 위해 샘플 앱의 `ScheduleUnthreadedNotification` 메서드는 위와 비슷한 코드를 사용 합니다.
그러나 `UNMutableNotificationContent` 개체에는 `ThreadIdentifier` 설정 되지 않습니다.

## <a name="related-links"></a>관련 링크

- [샘플 앱 – GroupedNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-groupednotifications)
- [Xamarin.ios의 사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md)
- [사용자 알림의 새로운 기능 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [그룹화 된 알림 사용 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/711/)
- [모범 사례 및 사용자 알림의 새로운 기능 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [원격 알림 생성 (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
