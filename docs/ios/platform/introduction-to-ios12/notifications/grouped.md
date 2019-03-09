---
title: Xamarin.iOS에서 그룹화 된 알림
description: IOS 12 사용 하 여 알림 센터 또는 응용 프로그램 또는 스레드 잠금 화면에서 알림 그룹 가능성이 있습니다. 이 문서는 스레드를 보내는 방법 및 Xamarin.iOS 사용 하 여 프레임인된 알림을 설명 합니다.
ms.prod: xamarin
ms.assetid: C6FA7C25-061B-4FD7-8E55-88597D512F3C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/04/2018
ms.openlocfilehash: 6798c4c5fa7502ba5e99cb8bc209468acaa4a9ec
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670027"
---
# <a name="grouped-notifications-in-xamarinios"></a>Xamarin.iOS에서 그룹화 된 알림

기본적으로 12 iOS 앱의 알림 모든 그룹에 배치를 합니다. 잠금 화면 및 알림 센터를 스택으로 맨 위에 가장 최근의 알림 사용 하 여이 그룹을 표시합니다. 사용자가 포함 하며 그룹을 전체적으로 해제 모든 알림을 볼 수 그룹을 확장할 수 있습니다.

앱 그룹 알림을 여 수도 스레드를 쉽게 찾아서 관심이 있는 특정 정보를 상호 작용 하는 사용자에 대 한 합니다.

## <a name="sample-app-groupednotifications"></a>샘플 앱: GroupedNotifications

Xamarin.iOS를 사용 하 여 그룹화 된 알림을 사용 하는 방법에 알아보려면 살펴보겠습니다 합니다 [GroupedNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/GroupedNotifications) 샘플 앱입니다.

샘플 앱이 다양 한 친구, 각 메시지에 대 한 알림을 보내고 스레드에서 그룹화를 사용 하 여 대화를 시뮬레이션 합니다. 또한 응용 프로그램 수준 그룹에 알림을 land를 프레임인된 하는 방법을 보여 줍니다.

이 샘플 앱에서이 가이드에서 코드 조각을 제공 됩니다.

## <a name="request-authorization-and-allow-foreground-notifications"></a>권한 부여를 요청 하 고 포그라운드 알림 허용

앱에는 로컬 알림을 보낼 수 있습니다, 전에 권한을 요청 해야 합니다. 샘플 앱의 [ `AppDelegate` ](xref:UIKit.UIApplicationDelegate)의 [ `FinishedLaunching` ](xref:UIKit.UIApplicationDelegate.FinishedLaunching(UIKit.UIApplication,Foundation.NSDictionary)) 메서드는이 권한을 요청 합니다.

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

합니다 [ `Delegate` ](xref:UserNotifications.UNUserNotificationCenter.Delegate) (설정 위에서)는 [ `UNUserNotificationCenter` ](xref:UserNotifications.UNUserNotificationCenter) 전경 앱 전달할완료처리기를호출하여는들어오는알림을표시할지여부를결정[`WillPresentNotification`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.WillPresentNotification(UserNotifications.IUNUserNotificationCenterDelegate,UserNotifications.UNUserNotificationCenter,UserNotifications.UNNotification,System.Action{UserNotifications.UNNotificationPresentationOptions})):

```csharp
[Export("userNotificationCenter:willPresentotification:withCompletionHandler:")]
public void WillPresentNotification(UNUserNotificationCenter center, UNNotification notification, System.Action<UNNotificationPresentationOptions> completionHandler)
{
    completionHandler(UNNotificationPresentationOptions.Alert);
}
```

합니다 [ `UNNotificationPresentationOptions.Alert` ](xref:UserNotifications.UNNotificationPresentationOptions) 매개 변수는 앱은 경고를 표시 하지만 소리를 재생 하거나 업데이트 하지 배지를 나타냅니다.

## <a name="threaded-notifications"></a>스레드 알림

샘플 앱의 탭 **Alice와 메시지** Alice 라는 친구를 사용 하 여 대화에 대 한 알림을 보내는 것을 표시 합니다.
이 대화의이 알림 동일한 스레드의 부분 되므로 잠금 화면 및 알림 센터를 그룹화 합니다.

다른 친구와 함께 대화를 시작 하려면 다음을 탭 합니다 **새 friend를 선택** 단추입니다. 이 대화에 대 한 알림을 별도 그룹에 나타납니다.

### <a name="threadidentifier"></a>ThreadIdentifier

샘플 앱이 시작 되는 새 스레드를 언제 든 지 고유 스레드 식별자를 만듭니다.

```csharp
void StartNewThread()
{
    threadId = $"message-{friend}";
    // ...
}
```

샘플 앱에서는 스레드 알림을 보내려면:

- 앱에 알림을 보내려면 권한 부여 여부를 확인 합니다.
- 만듭니다는 [`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent)
알림을 콘텐츠 및 설정에 대 한 개체는 [`ThreadIdentifier`](xref:UserNotifications.UNMutableNotificationContent.ThreadIdentifier)
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

동일한 스레드 식별자를 사용 하 여 동일한 앱에서 모든 알림이 동일한 알림 그룹에 표시 됩니다.

> [!NOTE]
> 원격 알림 스레드 식별자를 설정 하려면 추가 `thread-id` 알림의 JSON 페이로드를 키입니다. Apple의를 참조 하세요 [원격 알림을 생성](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) 자세한 문서.

### <a name="summaryargument"></a>SummaryArgument

`SummaryArgument` 알림을 알림을 속한 알림 그룹의 왼쪽 아래 모서리에 표시 되는 요약 텍스트가 미치는 영향을 지정 합니다. iOS에 대 한 전체 요약 설명을 만들려면 같은 그룹에 알림에서 요약 텍스트를 집계 합니다.

샘플 앱은 요약 인수로 메시지의 작성자를 사용 합니다. 이 방법을 사용 하면 Alice 사용 하 여 6 개의 알림 그룹에 대 한 요약 텍스트가 수 있습니다 **Alice와 m에서 자세한 알림을 5**합니다.

## <a name="unthreaded-notifications"></a>프레임인된 알림

샘플 앱의 각 탭 **약속 미리 알림을** 단추 다양 한 약속 미리 알림 중 하나를 보냅니다. 이러한 알림은 스레드 없습니다, 이후 잠금 화면에서 응용 프로그램 수준 알림 그룹에 알림 센터에 표시 됩니다.

프레임인된 알림을 보내도록, 샘플 앱의 `ScheduleUnthreadedNotification` 메서드 위와 유사한 코드를 사용 합니다.
하지만 설정 하지 않습니다 합니다 `ThreadIdentifier` 에 `UNMutableNotificationContent` 개체입니다.

## <a name="related-links"></a>관련 링크

- [샘플 앱 – GroupedNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/GroupedNotifications)
- [Xamarin.iOS에서 사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md)
- [사용자 알림 (WWDC 2018)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2018/710/)
- [그룹화 된 알림 (WWDC 2018)를 사용 하 여](https://developer.apple.com/videos/play/wwdc2018/711/)
- [모범 사례 및 사용자 알림 (WWDC 2017)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2017/708/)
- [원격 알림을 (Apple) 생성](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
