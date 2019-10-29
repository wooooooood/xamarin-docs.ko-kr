---
title: 연습-Xamarin.ios에서 로컬 알림 사용
description: 이 연습에서는 Xamarin Android 응용 프로그램에서 로컬 알림을 사용 하는 방법을 보여 줍니다. 로컬 알림 만들기 및 게시의 기본 사항을 보여 줍니다. 사용자가 알림 영역에서 알림을 클릭 하면 두 번째 작업을 시작 합니다.
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/16/2018
ms.openlocfilehash: 6d48d650b0900e71b7d3d4d5e1ff1ac919dcb948
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025541"
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>연습-Xamarin.ios에서 로컬 알림 사용

_이 연습에서는 Xamarin Android 응용 프로그램에서 로컬 알림을 사용 하는 방법을 보여 줍니다. 로컬 알림 만들기 및 게시의 기본 사항을 보여 줍니다. 사용자가 알림 영역에서 알림을 클릭 하면 두 번째 작업을 시작 합니다._

## <a name="overview"></a>개요

이 연습에서는 사용자가 활동의 단추를 클릭할 때 알림을 발생 시키는 Android 응용 프로그램을 만듭니다. 사용자가 알림을 클릭 하면 사용자가 첫 번째 활동에서 단추를 클릭 한 횟수를 표시 하는 두 번째 활동을 시작 합니다.

다음 스크린샷은이 응용 프로그램의 몇 가지 예를 보여 줍니다.

[알림과 함께 예제 스크린샷![](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)

> [!NOTE]
> 이 가이드에서는 [Android 지원 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)의 [notificationcompat api](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) 에 대해 집중적으로 설명 합니다. 이러한 Api는 Android 4.0 (API 수준 14)에 대 한 최대 이전 버전과의 호환성을 보장 합니다.

## <a name="creating-the-project"></a>프로젝트 만들기

먼저 **Android 앱** 템플릿을 사용 하 여 새 android 프로젝트를 만들어 보겠습니다. **Localnotifications**프로젝트를 호출 해 보겠습니다. Xamarin Android 프로젝트를 만드는 방법을 잘 모르는 경우 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)를 참조 하세요.

알림 채널을 만들 때 사용 되는 두 개의 추가 문자열 리소스를 포함 하도록 리소스 파일 **값/문자열 .xml** 을 편집 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>

<resources>
  <string name="Hello">Hello World, Click Me!</string>
  <string name="ApplicationName">Notifications</string>

  <string name="channel_name">Local Notifications</string>
  <string name="channel_description">The count from MainActivity.</string>
</resources>
```

### <a name="add-the-androidsupportv4-nuget-package"></a>Android. 지원 V4 NuGet 패키지를 추가 합니다.

이 연습에서는 `NotificationCompat.Builder`를 사용 하 여 로컬 알림을 작성 합니다. [로컬 알림에](~/android/app-fundamentals/notifications/local-notifications.md)설명 된 대로 `NotificationCompat.Builder`사용할 수 있도록 [Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet을 프로젝트에 포함 해야 합니다.

그런 다음, **MainActivity.cs** 를 편집 하 고 `Android.Support.V4.App`의 형식을 코드에서 사용할 수 있도록 다음 `using` 문을 추가 하겠습니다.

```csharp
using Android.Support.V4.App;
```

또한 `Android.App` 버전이 아닌 `Android.Support.V4.App` 버전의 `TaskStackBuilder`를 사용 하 고 있다는 것을 컴파일러에서 명확 하 게 지정 해야 합니다. 모호성을 해결 하기 위해 다음 `using` 문을 추가 합니다.

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```

### <a name="create-the-notification-channel"></a>알림 채널 만들기

다음으로, 필요한 경우 알림 채널을 만들 `MainActivity`에 메서드를 추가 합니다.

```csharp
void CreateNotificationChannel()
{
    if (Build.VERSION.SdkInt < BuildVersionCodes.O)
    {
        // Notification channels are new in API 26 (and not a part of the
        // support library). There is no need to create a notification
        // channel on older versions of Android.
        return;
    }

    var name = Resources.GetString(Resource.String.channel_name);
    var description = GetString(Resource.String.channel_description);
    var channel = new NotificationChannel(CHANNEL_ID, name, NotificationImportance.Default)
                  {
                      Description = description
                  };

    var notificationManager = (NotificationManager) GetSystemService(NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

이 새 메서드를 호출 하도록 `OnCreate` 메서드를 업데이트 합니다.

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();
}
```

### <a name="define-the-notification-id"></a>알림 ID 정의

알림 및 알림 채널에 대 한 고유 ID가 필요 합니다. **MainActivity.cs** 를 편집 하 고 다음 정적 인스턴스 변수를 `MainActivity` 클래스에 추가 해 보겠습니다.

```csharp
static readonly int NOTIFICATION_ID = 1000;
static readonly string CHANNEL_ID = "location_notification";
internal static readonly string COUNT_KEY = "count";
```

### <a name="add-code-to-generate-the-notification"></a>알림을 생성 하는 코드를 추가 합니다.

다음에는 단추 `Click` 이벤트에 대 한 새 이벤트 처리기를 만들어야 합니다. `MainActivity`에 다음 메서드를 추가합니다.

```csharp
void ButtonOnClick(object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    var valuesForActivity = new Bundle();
    valuesForActivity.PutInt(COUNT_KEY, count);

    // When the user clicks the notification, SecondActivity will start up.
    var resultIntent = new Intent(this, typeof(SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras(valuesForActivity);

    // Construct a back stack for cross-task navigation:
    var stackBuilder = TaskStackBuilder.Create(this);
    stackBuilder.AddParentStack(Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent(resultIntent);

    // Create the PendingIntent with the back stack:
    var resultPendingIntent = stackBuilder.GetPendingIntent(0, (int) PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    var builder = new NotificationCompat.Builder(this, CHANNEL_ID)
                  .SetAutoCancel(true) // Dismiss the notification from the notification area when the user clicks on it
                  .SetContentIntent(resultPendingIntent) // Start up this activity when the user clicks the intent.
                  .SetContentTitle("Button Clicked") // Set the title
                  .SetNumber(count) // Display the count in the Content Info
                  .SetSmallIcon(Resource.Drawable.ic_stat_button_click) // This is the icon to display
                  .SetContentText($"The button has been clicked {count} times."); // the message to display.

    // Finally, publish the notification:
    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(NOTIFICATION_ID, builder.Build());

    // Increment the button press count:
    count++;
}
```

MainActivity의 `OnCreate` 메서드는 호출을 수행 하 여 알림 채널을 만들고 단추의 `Click` 이벤트에 `ButtonOnClick` 메서드를 할당 해야 합니다 (템플릿에서 제공 하는 대리자 이벤트 처리기 대체).

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();

    // Display the "Hello World, Click Me!" button and register its event handler:
    var button = FindViewById<Button>(Resource.Id.MyButton);
    button.Click += ButtonOnClick;
}
```

### <a name="create-a-second-activity"></a>두 번째 활동 만들기

이제 사용자가 알림을 클릭할 때 Android에서 표시 하는 다른 작업을 만들어야 합니다. **SecondActivity**이라는 프로젝트에 다른 Android 활동을 추가 합니다. **SecondActivity.cs** 를 열고 내용을 다음 코드로 바꿉니다.

```csharp
using System;
using Android.App;
using Android.OS;
using Android.Widget;

namespace LocalNotifications
{
    [Activity(Label = "Second Activity")]
    public class SecondActivity : Activity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            // Get the count value passed to us from MainActivity:
            var count = Intent.Extras.GetInt(MainActivity.COUNT_KEY, -1);

            // No count was passed? Then just return.
            if (count <= 0)
            {
                return;
            }

            // Display the count sent from the first activity:
            SetContentView(Resource.Layout.Second);
            var txtView = FindViewById<TextView>(Resource.Id.textView1);
            txtView.Text = $"You clicked the button {count} times in the previous activity.";
        }
    }
}
```

또한 **SecondActivity**에 대 한 리소스 레이아웃을 만들어야 합니다. 프로젝트에 **두 번째. axml**이라는 새 **Android 레이아웃** 파일을 추가 합니다. **두 번째. axml** 을 편집 하 여 다음 레이아웃 코드를 붙여넣습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text=""
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:id="@+id/textView1" />
</LinearLayout>
```

### <a name="add-a-notification-icon"></a>알림 추가 아이콘

마지막으로, 알림이 시작 될 때 알림 영역에 표시 되는 작은 아이콘을 추가 합니다. [이 아이콘](local-notifications-walkthrough-images/ic-stat-button-click.png) 을 프로젝트에 복사 하거나 사용자 지정 아이콘을 직접 만들 수 있습니다. 아이콘 파일 **ic\_stat\_단추의 이름을 .png를 클릭\_** 하 고 **리소스/그릴** 수 있는 폴더에 복사 합니다. **> 기존 항목 추가** ...를 사용 하 여 프로젝트에이 아이콘 파일을 포함 해야 합니다.

### <a name="run-the-application"></a>애플리케이션 실행

애플리케이션을 빌드 및 실행합니다. 다음 스크린샷 처럼 첫 번째 활동이 표시 되어야 합니다.

[![First 활동 스크린샷](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

단추를 클릭 하면 알림에 대 한 작은 아이콘이 알림 영역에 표시 되는 것을 알 수 있습니다.

[![알림 아이콘이 표시 됩니다.](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

알림 서랍을 살짝 밀어 표시 하면 다음과 같은 알림이 표시 됩니다.

[![알림 메시지](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

알림을 클릭 하면 알림이 사라지고 다른 작업을 시작 하 &ndash; 다음 스크린샷에서 약간의 작업을 수행 해야 합니다.

[![Second 작업 스크린샷](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

지금까지 이 시점에서 Android 로컬 알림 연습을 완료 하 고 참조할 수 있는 작업 샘플을 사용할 수 있습니다. 여기에 표시 된 것 보다 더 많은 알림이 제공 되므로 자세한 정보를 보려는 경우 [에는 Google의 알림에 대 한 설명서](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)를 살펴보세요.

## <a name="summary"></a>요약

이 연습에서는 `NotificationCompat.Builder` 사용 하 여 알림을 만들고 표시 합니다. 이 예에서는 알림과의 사용자 상호 작용에 응답 하는 방법으로 두 번째 작업을 시작 하는 방법에 대 한 기본 예를 보여 주고 첫 번째 작업에서 두 번째 작업으로의 데이터 전송에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [LocalNotifications (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/localnotifications)
- [Android Oreo 알림 채널](https://blog.xamarin.com/android-oreo-notification-channels/)
- [알림](xref:Android.App.Notification)
- [NotificationManager](xref:Android.App.NotificationManager)
- [NotificationCompat 빌더](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](xref:Android.App.PendingIntent)
