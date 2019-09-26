---
title: 포그라운드 서비스
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/19/2018
ms.openlocfilehash: 6f3427641ba4ace3b640fcc970fd33f55087a9c8
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "68644106"
---
# <a name="foreground-services"></a>포그라운드 서비스

포그라운드 서비스는 바운드 서비스 또는 시작 된 서비스의 특수 한 형식입니다. 서비스에서 사용자가 적극적으로 인식 해야 하는 작업을 수행 하는 경우도 있습니다. 이러한 서비스를 _포그라운드 서비스_라고 합니다. 포그라운드 서비스의 예로는 사용자에 게 구동 또는 이동 중에 대 한 지침을 제공 하는 앱이 있습니다. 앱이 백그라운드에 있더라도 서비스에서 제대로 작동 하는 데 충분 한 리소스를 보유 하 고 있으며 사용자는 빠르고 편리 하 게 앱에 액세스할 수 있어야 합니다. Android 앱의 경우 포그라운드 서비스는 "일반" 서비스 보다 높은 우선 순위를 받아야 하며 포그라운드 서비스는 서비스가 실행 되는 동안 Android에서 `Notification` 표시 하는를 제공 해야 함을 의미 합니다.

포그라운드 서비스를 시작 하려면 앱에서 서비스를 시작 하도록 Android에 지시 하는 의도를 디스패치 해야 합니다. 그런 다음 서비스는 Android를 사용 하 여 포그라운드 서비스로 등록 해야 합니다. Android 8.0 이상에서 실행 되는 앱은 `Context.StartForegroundService` 메서드를 사용 하 여 서비스를 시작 해야 하는 반면, 이전 버전의 Android를 사용 하는 장치에서 실행 되는 앱은`Context.StartService`

이 C# 확장 메서드는 포그라운드 서비스를 시작 하는 방법의 예입니다. Android 8.0 이상에서는 `StartForegroundService` 메서드를 사용 합니다. 그렇지 않으면 이전 `StartService` 메서드가 사용 됩니다.

```csharp
public static void StartForegroundServiceCompat<T>(this Context context, Bundle args = null) where T : Service
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

## <a name="registering-as-a-foreground-service"></a>포그라운드 서비스로 등록

포그라운드 서비스가 시작 된 후에는를 [`StartForeground`](xref:Android.App.Service.StartForeground*)호출 하 여 Android에 등록 해야 합니다. 서비스가 `Service.StartForegroundService` 메서드를 사용 하 여 시작 되었지만 자체 등록 되지 않은 경우 Android는 서비스를 중지 하 고 앱에 응답 하지 않는 플래그를 지정 합니다.

`StartForeground`두 매개 변수를 사용 합니다. 두 매개 변수는 모두 필수입니다.

- 서비스를 식별 하기 위해 응용 프로그램 내에서 고유한 정수 값입니다.
- 서비스가 실행 되는 동안 Android가 상태 표시줄에 표시 하는 개체입니다.`Notification`

서비스를 실행 하는 동안에는 Android가 상태 표시줄에 알림을 표시 합니다. 알림은 최소한 서비스가 실행 중인 사용자에 게 시각적 신호를 제공 합니다. 가장 이상적으로 알림은 사용자에 게 응용 프로그램에 대 한 바로 가기를 제공 하거나 응용 프로그램을 제어 하는 몇 가지 작업 단추를 제공 해야 합니다. 이에 대 한 예는 음악 플레이어 &ndash; 입니다. 표시 되는 알림은 음악을 일시 중지/재생 하거나, 이전 곡으로 되감거나, 다음 노래를 건너뛰도록 하는 단추를 포함할 수 있습니다. 

이 코드 조각은 서비스를 포그라운드 서비스로 등록 하는 예제입니다.   

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

이전 알림에는 다음과 같은 상태 표시줄 알림이 표시 됩니다.

![상태 표시줄의 알림을 보여 주는 이미지](foreground-services-images/foreground-services-01.png "상태 표시줄의 알림을 보여 주는 이미지")

이 스크린샷은 사용자가 서비스를 제어할 수 있도록 하는 두 가지 작업을 포함 하는 알림 트레이의 확장 된 알림을 보여 줍니다.

![확장 된 알림을 보여 주는 이미지](foreground-services-images/foreground-services-02.png "확장 된 알림을 표시 하는 이미지입니다.")

알림에 대 한 자세한 내용은 [Android 알림](~/android/app-fundamentals/notifications/index.md) 가이드의 [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md) 섹션에서 확인할 수 있습니다.

## <a name="unregistering-as-a-foreground-service"></a>포그라운드 서비스로 등록 취소

서비스는 메서드 `StopForeground`를 호출 하 여 자체 목록을 전경 서비스로 해제할 수 있습니다. `StopForeground`는 서비스를 중지 하지는 않지만 알림 아이콘을 제거 하 고 필요한 경우이 서비스를 종료할 수 있음을 Android에 알립니다.

표시 되는 상태 표시줄 알림은 메서드에 전달 `true` 하 여 제거할 수도 있습니다. 

```csharp
StopForeground(true);
```

`StopSelf` 또는`StopService`에 대 한 호출을 통해 서비스가 중단 되 면 상태 표시줄 알림이 제거 됩니다.

## <a name="related-links"></a>관련 링크

- [Android.App.Service](xref:Android.App.Service)
- [Android.App.Service.StartForeground](xref:Android.App.Service.StartForeground*)
- [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-foregroundservicedemo)
