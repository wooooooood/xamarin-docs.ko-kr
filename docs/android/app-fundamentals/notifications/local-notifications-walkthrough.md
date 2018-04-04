---
title: 연습-Xamarin.Android에 로컬 알림을 사용 하 여
description: 이 연습에는 로컬 알림을 Xamarin.Android 응용 프로그램에서 사용 하는 방법을 보여 줍니다. 만들기 및 로컬 알림을 게시의 기본 사항을 보여 줍니다. 사용자가 알림 영역에서 알림을 클릭, 두 번째 활동을 시작 합니다.
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/30/2018
ms.openlocfilehash: a2ca3755e3201263584447ba47ec36d2096386da
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>연습-Xamarin.Android에 로컬 알림을 사용 하 여

_이 연습에는 로컬 알림을 Xamarin.Android 응용 프로그램에서 사용 하는 방법을 보여 줍니다. 만들기 및 로컬 알림을 게시의 기본 사항을 보여 줍니다. 사용자가 알림 영역에서 알림을 클릭, 두 번째 활동을 시작 합니다._


## <a name="overview"></a>개요

이 연습에서는 알림을 활동에서 단추를 클릭할 때 발생 하는 Android 응용 프로그램에서는 만들어집니다. 사용자가 알림을 클릭 하면 사용자가 첫 번째 활동의 단추를 클릭 한 횟수를 표시 하는 두 번째 활동을 시작 합니다.

다음 스크린샷에서이 응용 프로그램의 몇 가지 예를 보여 줍니다.

[![알림 사용 하 여 예제 스크린 샷](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)



## <a name="walkthrough"></a>연습

를 시작 하려면 만들기 사용 하 여 새 Android 프로젝트는 **Android 앱** 템플릿. 이 프로젝트 부르겠습니다 **LocalNotifications**합니다. (Xamarin.Android 프로젝트 만들기에 익숙하지 않은 경우 참조 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).)


### <a name="add-the-androidsupportv4app-component"></a>Android.Support.V4.App 구성 요소를 추가 합니다.

이 연습에서 사용 하 여 `NotificationCompat.Builder` 우리의 로컬 알림 빌드할 수 있습니다. 에 설명 된 대로 [로컬 알림을](~/android/app-fundamentals/notifications/local-notifications.md), 포함 되어야 합니다는 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 사용 하려면 프로젝트에 NuGet `NotificationCompat.Builder`합니다.

다음으로 편집 **MainActivity.cs** 다음 추가 `using` 문을 있도록의 형식을 `Android.Support.V4.App` 코드에 사용할 수:

```csharp
using Android.Support.V4.App;
```

또한에서는 해야 있도록 사용 하는 컴파일러는 `Android.Support.V4.App` 버전의 `TaskStackBuilder` 보다는 `Android.App` 버전입니다. 다음 추가 `using` 문을 한 모호함을 해결 합니다.

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```


### <a name="define-the-notification-id"></a>알림 ID를 정의 합니다.

이 알림에 대 한 고유 ID를 필요 합니다. 편집 **MainActivity.cs** 다음 정적 인스턴스 변수를 추가 하 고는 `MainActivity` 클래스:

```csharp
private static readonly int ButtonClickNotificationId = 1000;
```


### <a name="add-code-to-generate-the-notification"></a>알림을 생성 하는 코드를 추가 합니다.

단추에 대 한 새 이벤트 처리기를 만들어야 한다는 다음으로, `Click` 이벤트입니다. 다음 메서드를 추가 `MainActivity`:

```csharp
private void ButtonOnClick (object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    Bundle valuesForActivity = new Bundle();
    valuesForActivity.PutInt ("count", count);

    // When the user clicks the notification, SecondActivity will start up.
    Intent resultIntent = new Intent(this, typeof (SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras (valuesForActivity);

    // Construct a back stack for cross-task navigation:
    TaskStackBuilder stackBuilder = TaskStackBuilder.Create (this);
    stackBuilder.AddParentStack (Java.Lang.Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent (resultIntent);

    // Create the PendingIntent with the back stack:            
    PendingIntent resultPendingIntent =
        stackBuilder.GetPendingIntent (0, (int)PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
        .SetAutoCancel (true)                    // Dismiss from the notif. area when clicked
        .SetContentIntent (resultPendingIntent)  // Start 2nd activity when the intent is clicked.
        .SetContentTitle ("Button Clicked")      // Set its title
        .SetNumber (count)                       // Display the count in the Content Info
        .SetSmallIcon(Resource.Drawable.ic_stat_button_click)  // Display this icon
        .SetContentText (String.Format(
            "The button has been clicked {0} times.", count)); // The message to display.

    // Finally, publish the notification:
    NotificationManager notificationManager =
        (NotificationManager)GetSystemService(Context.NotificationService);
    notificationManager.Notify(ButtonClickNotificationId, builder.Build());

    // Increment the button press count:
    count++;
}
```

에 `OnCreate` 메서드를이 할당 `ButtonOnClick` 메서드는 `Click` (대체 서식 파일이 제공 대리자 이벤트 처리기) 단추의 이벤트:

```csharp
button.Click += ButtonOnClick;
```


### <a name="create-a-second-activity"></a>두 번째 활동 만들기

이제 Android 우리의 알림을 클릭할 때 표시 하는 다른 활동을 만드는 필요 합니다. 라는 프로젝트에 다른 Android 활동을 추가 **SecondActivity**합니다. 열기 **SecondActivity.cs** 해당 내용을이 코드로 바꿉니다.

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
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Get the count value passed to us from MainActivity:
            int count = Intent.Extras.GetInt ("count", -1);

            // No count was passed? Then just return.
            if (count <= 0)
                return;

            // Display the count sent from the first activity:
            SetContentView (Resource.Layout.Second);
            TextView textView = FindViewById<TextView>(Resource.Id.textView);
            textView.Text = String.Format (
                "You clicked the button {0} times in the previous activity.", count);
        }
    }
}
```

에 대 한 리소스 레이아웃도 만들어야 **SecondActivity**합니다. 새로 추가 **Android 레이아웃** 파일을 프로젝트 라는 **Second.axml**합니다. 편집 **Second.axml** 다음 레이아웃 코드에 붙여 넣습니다.

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
        android:id="@+id/textView" />
</LinearLayout>
```


### <a name="add-a-notification-icon"></a>추가 알림 아이콘

마지막으로,이 알림을 시작할 때 알림 영역에 표시 되는 작은 아이콘을 추가 해 보겠습니다. 복사할 수는 있지만 [이 아이콘](local-notifications-walkthrough-images/ic-stat-button-click.png) 프로젝트에 하거나 사용자 지정 아이콘을 만듭니다. 아이콘 파일의 이름을 **ic\_stat\_단추\_click.png** 복사는 **리소스/그릴** 폴더입니다. 사용 해야 **추가 > 기존 항목...**  프로젝트에서이 아이콘 파일을 포함 하도록 합니다.


### <a name="run-the-application"></a>응용 프로그램 실행

빌드 및 응용 프로그램을 실행 해 보겠습니다. 다음 스크린샷과 유사 하 게 첫 번째 활동과 함께 표시 되어야 합니다.

[![첫 번째 활동 스크린 샷](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

단추를 클릭할 때 작은 아이콘에 알림에 대 한 알림 영역에 표시를 확인 해야 합니다.

[![알림 아이콘 표시](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

아래로 살짝 알림 서랍을 노출 하는 경우 알림을 표시 되어야 합니다.

[![알림 메시지](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

알림을 클릭 하 고, 사라져야 다른 활동을 실행 해야 하는 경우 &ndash; 다음 스크린 샷에서 같은 검색:

[![두 번째 활동 스크린 샷](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

지금까지 이 시점에서 Android 로컬 알림 연습 완료 하며를 참조할 수 있는 작업 예제를 해야 합니다. 알림을 대 한 자세한 우리는 여기 표시 된 것 보다 자세한 정보를 가져오려면 그러니까 살펴보겠습니다 많은 [알림을 통해 Google 설명서](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) 및 Android [알림](http://developer.android.com/design/patterns/notifications.html) 디자인 가이드입니다.



## <a name="summary"></a>요약

이 연습에서 사용한 `NotificationCompat.Builder` 만들고 알림을 표시 합니다. 기본 예제는 알림 사용 하 여 사용자 상호 작용에 응답 하는 방법으로 두 번째 활동을 시작 하는 방법에 살펴보았습니다 하 고 두 번째 활동을 첫 번째 활동에서 데이터 전송의 시연을 합니다.


## <a name="related-links"></a>관련 링크

- [LocalNotifications (샘플)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [알림을 통해 디자인 가이드 패턴](http://developer.android.com/design/patterns/notifications.html)
- [알림](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
