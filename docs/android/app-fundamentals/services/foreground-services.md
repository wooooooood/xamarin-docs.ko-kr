---
title: "전경 서비스"
ms.topic: article
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: d1267bc4a530deb6dfb6eb2e30bee2facabd8fed
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2018
---
# <a name="foreground-services"></a>전경 서비스

전경 서비스는 특수 한 유형의 바인딩된 서비스 또는 서비스 시작된. 경우에 따라 서비스 사용자가 적극적으로 인식 해야 하는 작업을 수행 합니다, 이러한 서비스 라고 _전경 서비스_합니다. 전경 서비스의 예를 제공 하는 사용자 지침에 운전 또는 보 행 하는 동안 응용 프로그램입니다. 경우에 응용 프로그램은 백그라운드로,에 서비스에 제대로 작동 하려면 충분 한 리소스가 있는지와 사용자에 게 응용 프로그램에 액세스 하는 빠르고 편리한 방법을 여전히 중요 합니다. Android 앱의 경우 즉, 전경 서비스 "일반" 서비스 보다 더 높은 우선 순위를 받아야 전경 서비스 제공 해야 합니다는 `Notification` Android 서비스가 실행 중인 상태로 표시 하 합니다.
 
전경 서비스를 시작 하려면 응용 프로그램 서비스를 시작 하는 Android에 알려 주는 의도 발송 해야 합니다. 그런 다음 서비스 Android 사용 하 여 포그라운드 서비스로 자체 등록 해야 합니다. Android 8.0 (또는 이상)에서 실행 되는 앱을 사용할지는 `Context.StartForegroundService` 메서드를 사용 하 여 이전 버전의 Android 장치에서 실행 되는 앱을 사용 해야 하는 동안 서비스를 시작 `Context.StartService`

이 C# 확장 메서드는 전경 서비스를 시작 하는 방법의 예시입니다. 사용할 Android 8.0에서 이상은 `StartForegroundService` 메서드, 그렇지 않으면 이전 `StartService` 메서드가 사용 됩니다.  

```csharp
public static void StartForegroundServiceComapt<T>(this Context context, Bundle args = null) where T : Service
{
    var intent = new Intent(context, typeof(T));
    if (args != null) 
    {
        intent.PutExtras(args);
    }

    if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.O)
    {
        context.StartForegroundService(intent);
    }
    else
    {
        context.StartService(intent);
    }
}
```

## <a name="registering-as-a-foreground-service"></a>전경 서비스로 등록 하는 중

전경 서비스가 시작 되 면이를 등록 해야 자체 Android를 호출 하 여는 [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)합니다. 으로 서비스를 시작 하는 경우는 `Service.StartForegroundService` 메서드 했지만 Android 서비스를 중지 하 고 응답 하지 않는 응용 프로그램 플래그 것 자체를 등록 하지 않습니다.

`StartForeground` 두 개의 매개 변수를 둘 다는 필수 사용:
 
* 서비스를 식별 하는 응용 프로그램 내에서 고유한 정수 값입니다.
* A `Notification` 으로 서비스가 실행 중인 Android에 대 한 상태 표시줄에 표시 하는 개체입니다.

서비스가 실행 중인 상태로 android에 대 한 상태 표시줄에 알림이 표시 됩니다. 여기에 최소한 알림 서비스가 실행 되는 사용자에 게 알리는 시각 신호를 제공 합니다. 이상적으로 알림 응용 프로그램 또는 일부 실행 단추 수 있는 응용 프로그램을 제어 하려면 바로 가기 사용 하 여 사용자를 제공 해야 합니다. 이 예는 음악 플레이어 &ndash; 표시 되는 알림/재생 음악, 이전 노래를 되감기 또는 다음 노래 이동 하는 단추 있을 수 있습니다. 

이 코드 조각은 전경 서비스로 등록 하는 서비스의 예입니다.   

```csharp
// This is any integer value unique to the application.
public const int SERVICE_RUNNING_NOTIFICATION_ID = 10000;

public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
{
    // Code not directly related to publishing the notification has been omitted for clarity.
    // Normally, this method would hold the code to be run when the service is started.
    
    var notification = new Notification.Builder(this)
        .SetContentTitle(Resources.GetString(Resource.String.app_name))
        .SetContentText(Resources.GetString(Resource.String.notification_text))
        .SetSmallIcon(Resource.Drawable.ic_stat_name)
        .SetContentIntent(BuildIntentToShowMainActivity())
        .SetOngoing(true)
        .AddAction(BuildRestartTimerAction())
        .AddAction(BuildStopServiceAction())
        .Build();

    // Enlist this instance of the service as a foreground service
    StartForeground(SERVICE_RUNNING_NOTIFICATION_ID, notification);
}
```

이전의 알림 다음과 유사한 되는 상태 표시줄 알림을 표시 됩니다.

![상태 표시줄에서 알림을 보여 주는 이미지](foreground-services-images/foreground-services-01.png "상태 표시줄에서 알림을 보여 주는 이미지")

사용자가 서비스를 제어할 수 있는 두 가지 작업 알림 표시줄에 확장 된 알림 스크린샷은:

![확장 된 알림을 보여 주는 이미지](foreground-services-images/foreground-services-02.png "확장 된 알림을 보여 주는 이미지입니다.")

알림에 대 한 자세한 정보는에서 사용할 수는 [로컬 알림을](~/android/app-fundamentals/notifications/local-notifications.md) 의 섹션은 [Android 알림을](~/android/app-fundamentals/notifications/index.md) 가이드입니다.

## <a name="unregistering-as-a-foreground-service"></a>전경 서비스로 등록 취소

서비스 해제 나열할 수 자체 전경 서비스로 메서드를 호출 하 여 `StopForeground`합니다. `StopForeground` 서비스를 중지 되지는 않지만 알림 아이콘 및이 서비스가 필요에 따라 종료 될 수 있는 Android 신호 제거 됩니다.

표시 되는 상태 표시줄 알림에 전달 하 여 제거 될 수 있습니다 `true` 메서드에: 

```csharp
StopForeground(true);
```

서비스를 호출 하 여 중단 되 면 `StopSelf` 또는 `StopService`, 상태 표시줄 알림 제거 됩니다.

## <a name="related-links"></a>관련 링크

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForegrond](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
