---
title: "서비스 알림"
description: "이 가이드로 인해 Android 서비스 정보는 사용자에 게 발송할 로컬 알림을 사용 될 수 있습니다는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: a5309b46b67a79225611aafb546b35e73d891b38
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="service-notifications"></a>서비스 알림

_이 가이드로 인해 Android 서비스 정보는 사용자에 게 발송할 로컬 알림을 사용 될 수 있습니다는 방법을 설명 합니다._


## <a name="service-notifications-overview"></a>서비스 알림 개요

Android 응용 프로그램은 전경에 없는 경우에 서비스 알림을 사용자에 게 정보를 표시 하는 응용 프로그램을 사용 합니다. 알림을 수신 응용 프로그램에서 활동을 표시 하는 등 사용자에 대 한 작업을 제공 하는 것이 불가능 합니다. 다음 코드 샘플 서비스는 사용자에 게 알림 수 디스패치 하는 방법을 보여 줍니다.

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

이 스크린 샷은 표시 되는 알림 메시지의 예:

[![상태 표시줄에 표시 되는 알림 아이콘](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png)

사용자 슬라이드 알림 화면 위쪽에서 아래쪽, 전체 알림이 표시 됩니다.

![Notication 알림 표시줄에 표시](service-notifications-images/02-fullnotification.png)


## <a name="updating-a-notification"></a>업데이트 알림

서비스에 같은 알림 ID를 사용 하 여 알림을 다시 게시는 점에 알림을 업데이트 하려면 Android을 표시 하거나 필요에 따라 상태 표시줄에서 알림을 업데이트 합니다.

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

알림에 대 한 자세한 정보는에서 사용할 수는 [로컬 알림을](~/android/app-fundamentals/notifications/local-notifications.md) 의 섹션은 [Android 알림을](~/android/app-fundamentals/notifications/index.md) 가이드입니다.


## <a name="related-links"></a>관련 링크

- [Android의 로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)
