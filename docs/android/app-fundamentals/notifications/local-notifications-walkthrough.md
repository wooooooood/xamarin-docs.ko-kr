---
title: 연습-Xamarin.Android에서 로컬 알림 사용
description: 이 연습에는 Xamarin.Android 응용 프로그램에서 로컬 알림을 사용 하는 방법을 보여 줍니다. 만들기 및 로컬 알림을 게시의 기본 사항을 보여 줍니다. 사용자가 알림 영역에 알림이 두 번째 작업을 시작 합니다.
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/16/2018
ms.openlocfilehash: 7cf1dde6c65d2971cecd0a59a2e11d6c2d50ee2a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119191"
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>연습-Xamarin.Android에서 로컬 알림 사용

_이 연습에는 Xamarin.Android 응용 프로그램에서 로컬 알림을 사용 하는 방법을 보여 줍니다. 만들기 및 로컬 알림을 게시의 기본 사항을 보여 줍니다. 사용자가 알림 영역에 알림이 두 번째 작업을 시작 합니다._


## <a name="overview"></a>개요

이 연습에서는 사용자 작업에 단추를 클릭할 때 알림이 발생 하는 Android 응용 프로그램을 만듭니다. 사용자가 알림을 클릭 하면 사용자가 첫 번째 작업에서 단추를 클릭 한 횟수를 표시 하는 두 번째 작업을 시작 합니다.

다음 스크린샷은이 응용 프로그램의 몇 가지 예를 보여 줍니다.

[![알림 사용 하 여 예제 스크린샷](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)

> [!NOTE]
> 이 가이드에 중점을 두고 합니다 [NotificationCompat Api](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) 에서 합니다 [Android 지원 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)합니다. 이러한 Api는 역호환성 최대 Android 4.0 (API 수준 14).

## <a name="creating-the-project"></a>프로젝트 만들기

시작 하려면 만들겠습니다 사용 하 여 새 Android 프로젝트를 **Android 앱** 템플릿. 이 프로젝트를 호출 해 보겠습니다 **LocalNotifications**합니다. (Xamarin.Android 프로젝트 만들기와 잘 모르는 경우 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).)

리소스 파일을 편집 **values/Strings.xml** 알림 채널을 만들 때 사용할 두 개의 추가 문자열 리소스 포함 되도록 합니다.


```xml
<?xml version="1.0" encoding="utf-8"?>

<resources>
  <string name="Hello">Hello World, Click Me!</string>
  <string name="ApplicationName">Notifications</string>

  <string name="channel_name">Local Notifications</string>
  <string name="channel_description">The count from MainActivity.</string>
</resources>
```

### <a name="add-the-androidsupportv4-nuget-package"></a>Android.Support.V4 NuGet 패키지 추가

이 연습에서는 사용 `NotificationCompat.Builder` 빌드하는 로컬 알림. 에 설명 된 대로 [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)에 포함 되어야 합니다는 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 사용 하려면 프로젝트의 NuGet `NotificationCompat.Builder`.

다음으로, 편집할 **MainActivity.cs** 다음을 추가 합니다 `using` 문 있도록의 형식을 `Android.Support.V4.App` 코드에 사용할 수 있습니다:

```csharp
using Android.Support.V4.App;
```

또한 해야 좋을까요 사용 중인 컴파일러 하려면 선택을 취소 합니다 `Android.Support.V4.App` 버전이 `TaskStackBuilder` 대신 `Android.App` 버전입니다. 다음 추가 `using` 문을 한 모호함을 해결 합니다.

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```

### <a name="create-the-notification-channel"></a>알림 채널을 만들려면

다음으로, 하는 메서드를 추가 `MainActivity` 는 (필요한 경우) 알림 채널을 만듭니다.

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

업데이트 된 `OnCreate` 이 새 메서드를 호출 하는 방법.

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();
}
```


### <a name="define-the-notification-id"></a>알림 ID를 정의 합니다.

이 알림 및 알림 채널에 대 한 고유 ID를 해야 합니다. 편집할 **MainActivity.cs** 추가한 다음 정적 인스턴스 변수에 `MainActivity` 클래스:

```csharp
static readonly int NOTIFICATION_ID = 1000;
static readonly string CHANNEL_ID = "location_notification";
internal static readonly string COUNT_KEY = "count";
```

### <a name="add-code-to-generate-the-notification"></a>알림을 생성 하는 코드를 추가 합니다.

다음으로, 단추에 대 한 새 이벤트 처리기를 생성 해야 `Click` 이벤트입니다. 다음 메서드를 추가 `MainActivity`:

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

합니다 `OnCreate` MainActivity 메서드의 호출 알림 채널을 만들고 할당 해야 합니다.는 `ButtonOnClick` 메서드를는 `Click` (대체 템플릿에서 제공 하는 대리자 이벤트 처리기) 단추의 이벤트:

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


### <a name="create-a-second-activity"></a>두 번째 작업 만들기

이제이 알림을 클릭 하는 경우 Android를 표시 하는 다른 활동을 작성 해야 합니다. 이라는 프로젝트에 다른 Android 활동 추가 **SecondActivity**합니다. 오픈 **SecondActivity.cs** 해당 내용을이 코드로 바꿉니다.

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

또한 리소스의 레이아웃을 만들어야 **SecondActivity**합니다. 새 **Android 레이아웃** 파일을 프로젝트 호출 **Second.axml**합니다. 편집할 **Second.axml** 다음 레이아웃 코드에 붙여 넣습니다.

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


### <a name="add-a-notification-icon"></a>알림 아이콘 추가

마지막으로 알림을 시작 되 면 알림 영역에 나타나는 작은 아이콘을 추가 합니다. 복사할 수 있습니다 [이 아이콘](local-notifications-walkthrough-images/ic-stat-button-click.png) 프로젝트를 만들거나 사용자 고유의 사용자 지정 아이콘입니다. 아이콘 파일 이름을 **ic\_stat\_단추\_click.png** 복사 합니다 **리소스/drawable** 폴더입니다. 사용 하 여 **추가 > 기존 항목...**  프로젝트에서이 아이콘 파일을 포함 하도록 합니다.


### <a name="run-the-application"></a>응용 프로그램 실행

응용 프로그램을 빌드 및 실행합니다. 다음 스크린샷과 유사 하 게 첫 번째 활동을 사용 하 여 표시 합니다.

[![첫 번째 활동 스크린 샷](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

단추를 클릭 하면 알림에 대 한 작은 아이콘에 알림 영역에 나타나는지 유의 해야 합니다.

[![알림 아이콘 표시](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

아래로 살짝 알림 서랍을 노출 하는 경우 알림이 표시 됩니다.

[![알림 메시지](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

알림을 클릭 하면, 사라져야 합니다 및 다른 작업을 시작 해야 하는 경우 &ndash; 다음 스크린샷과 같이 어느 정도 확인 합니다.

[![두 번째 활동 스크린 샷](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

지금까지 이 시점에서 Android 로컬 알림 연습 완료 하 고를 참조할 수 있는 작업 예제를 사용 합니다. 알림을 자세히 여기에서 설명 했습니다 보다 자세한 정보가 필요한 경우 걸리는 살펴보겠습니다 많이 [알림에 대 한 Google 설명서](http://developer.android.com/guide/topics/ui/notifiers/notifications.html)합니다.


## <a name="summary"></a>요약

사용 되는이 연습에서는 `NotificationCompat.Builder` 만들고 알림을 표시 합니다. 알림 사용 하 여 사용자 상호 작용에 응답 하는 방법으로 두 번째 작업을 시작 하는 방법의 기본 예제에 알아보았습니다 및 두 번째 활동 데이터를 전송에서 첫 번째 작업을 보여 줍니다.


## <a name="related-links"></a>관련 링크

- [LocalNotifications (샘플)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Android Oreo 알림 채널](https://blog.xamarin.com/android-oreo-notification-channels/)
- [알림](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
