---
title: 서비스 알림
description: 이 가이드에서는 Android 서비스에서 로컬 알림을 사용 하 여 사용자에 게 정보를 발송 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: b02785863f89ef6a273c52c09f45a99c17cb6242
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024529"
---
# <a name="service-notifications"></a>서비스 알림

_이 가이드에서는 Android 서비스에서 로컬 알림을 사용 하 여 사용자에 게 정보를 발송 하는 방법을 설명 합니다._

## <a name="service-notifications-overview"></a>서비스 알림 개요

Service notification을 사용 하면 Android 응용 프로그램이 전경에 있지 않아도 앱에서 사용자에 게 정보를 표시할 수 있습니다. 응용 프로그램에서 작업을 표시 하는 등 사용자에 대 한 작업을 제공 하는 알림이 있을 수 있습니다. 다음 코드 샘플에서는 서비스에서 사용자에 게 알림을 디스패치할 수 있는 방법을 보여 줍니다.

```csharp
[Service]
public class MyService: Service 
{
    // A notification requires an id that is unique to the application.
    const int NOTIFICATION_ID = 9000;
    
    public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
    {
        // Code omitted for clarity - here is where the service would do something.
    
        // Work has finished, now dispatch anotification to let the user know.
        Notification.Builder notificationBuilder = new Notification.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_notification_small_icon)
            .SetContentTitle(Resources.GetString(Resource.String.notification_content_title))
            .SetContentText(Resources.GetString(Resource.String.notification_content_text));
        
        var notificationManager = (NotificationManager)GetSystemService(NotificationService);
        notificationManager.Notify(NOTIFICATION_ID, notificationBuilder.Build());
    }
}
```

이 스크린샷은 표시 되는 알림의 예입니다.

[상태 표시줄에 표시 되는![알림 아이콘](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png#lightbox)

사용자가 위쪽에서 알림 화면을 아래로 이동 하면 전체 알림이 표시 됩니다.

![알림 트레이에 표시 됨](service-notifications-images/02-fullnotification.png)

## <a name="updating-a-notification"></a>알림 업데이트

알림을 업데이트 하기 위해 서비스는 동일한 알림 ID를 사용 하 여 알림을 다시 게시 합니다. Android는 필요에 따라 상태 표시줄에 알림을 표시 하거나 업데이트 합니다.

```csharp 
void UpdateNotification(string content)
{
   var notification = GetNotification(content, pendingIntent);

   NotificationManager notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
   notificationManager.Notify(NOTIFICATION_ID, notification);
}

Notification GetNotification(string content, PendingIntent intent)
{
   return new Notification.Builder(this)
           .SetContentTitle(tag)
           .SetContentText(content)
           .SetSmallIcon(Resource.Drawable.NotifyLg)
           .SetLargeIcon(BitmapFactory.DecodeResource(Resources, Resource.Drawable.Icon))
           .SetContentIntent(intent).Build();
}
```

알림에 대 한 자세한 내용은 [Android 알림](~/android/app-fundamentals/notifications/index.md) 가이드의 [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md) 섹션에서 확인할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Android의 로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)
