---
title: Xamarin.Forms 로컬 알림
description: 이 문서에서는 Xamarin.Forms에서 로컬 알림을 주고받는 방법을 설명합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 40e040f216ddda40931273f4e7f5614964862fe8
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137596"
---
# <a name="local-notifications-in-xamarinforms"></a>Xamarin.Forms의 로컬 알림

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/local-notifications)

로컬 알림은 모바일 디바이스에 설치된 애플리케이션이 보내는 경고입니다. 로컬 알림은 흔히 다음과 같은 기능에 사용됩니다.

- 일정 이벤트
- 미리 알림
- 위치 기반 트리거

플랫폼마다 로컬 알림의 생성, 표시, 사용량을 다르게 처리합니다. 이 문서에서는 Xamarin.Forms를 사용하여 로컬 알림을 주고받는 플랫폼 간 추상화를 만드는 방법을 설명합니다.

[![iOS 및 Android의 로컬 알림 애플리케이션](local-notifications-images/local-notifications-msg-cropped.png)](local-notifications-images/local-notifications-msg.png#lightbox)

## <a name="create-a-cross-platform-interface"></a>플랫폼 간 인터페이스 만들기

Xamarin.Forms 애플리케이션은 기본 플랫폼 구현에 대한 걱정 없이 알림을 만들고 사용해야 합니다. 다음 `INotificationManager` 인터페이스는 공유 코드 라이브러리에서 구현되며, 애플리케이션이 알림과 상호 작용하는 데 사용할 수 있는 플랫폼 간 API를 정의합니다.

```csharp
public interface INotificationManager
{
    event EventHandler NotificationReceived;

    void Initialize();

    int ScheduleNotification(string title, string message);

    void ReceiveNotification(string title, string message);
}
```

이 인터페이스는 각 플랫폼 프로젝트에서 구현됩니다. `NotificationReceived` 이벤트를 사용하면 애플리케이션이 들어오는 알림을 처리할 수 있습니다. `Initialize` 메서드는 알림 시스템을 준비하는 데 필요한 모든 네이티브 플랫폼 논리를 수행합니다. `ScheduleNotification` 메서드는 알림을 보냅니다. 메시지가 수신되면 기본 플랫폼에서 `ReceiveNotification` 메서드를 호출합니다.

## <a name="consume-the-interface-in-xamarinforms"></a>Xamarin.Forms에서 인터페이스 사용

인터페이스를 만든 후에는 플랫폼 구현이 아직 만들어지지 않은 경우에도 공유 Xamarin.Forms 프로젝트에서 인터페이스를 사용할 수 있습니다. 샘플 애플리케이션에는 다음 내용이 있는 **MainPage.xaml**이라는 `ContentPage`가 포함되어 있습니다.

```xaml
<StackLayout Margin="0,35,0,0"
             x:Name="stackLayout">
    <Label Text="Click the button to create a local notification."
           TextColor="Red"
           HorizontalOptions="Center"
           VerticalOptions="Start" />
    <Button Text="Create Notification"
            HorizontalOptions="Center"
            VerticalOptions="Start"
            Clicked="OnScheduleClick"/>
</StackLayout>
```

레이아웃에는 사용자에 대한 지침이 있는 `Label` 요소와 탭할 때 알림을 예약하는 `Button`이 포함되어 있습니다.

`MainPage` 클래스 코드 숨김이 알림 보내기 및 받기를 처리합니다.

```csharp
public partial class MainPage : ContentPage
{
    INotificationManager notificationManager;
    int notificationNumber = 0;

    public MainPage()
    {
        InitializeComponent();

        notificationManager = DependencyService.Get<INotificationManager>();
        notificationManager.NotificationReceived += (sender, eventArgs) =>
        {
            var evtData = (NotificationEventArgs)eventArgs;
            ShowNotification(evtData.Title, evtData.Message);
        };
    }

    void OnScheduleClick(object sender, EventArgs e)
    {
        notificationNumber++;
        string title = $"Local Notification #{notificationNumber}";
        string message = $"You have now received {notificationNumber} notifications!";
        notificationManager.ScheduleNotification(title, message);
    }

    void ShowNotification(string title, string message)
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            var msg = new Label()
            {
                Text = $"Notification Received:\nTitle: {title}\nMessage: {message}"
            };
            stackLayout.Children.Add(msg);
        });
    }
}
```

`MainPage` 클래스 생성자는 Xamarin.Forms `DependencyService`를 사용하여 `INotificationManager`의 플랫폼별 인스턴스를 검색합니다. `OnScheduleClicked` 메서드는 `INotificationManager` 인스턴스를 사용하여 새 알림을 예약합니다. `ShowNotification` 메서드는 `NotificationReceived` 이벤트에 연결된 이벤트 처리기에서 호출되고, 이벤트가 호출될 때 페이지에 새 `Label`을 삽입합니다.

Xamarin.Forms `DependencyService`에 대한 자세한 내용은 [Xamarin.Forms DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/introduction.md)를 참조하세요.

## <a name="create-the-android-interface-implementation"></a>Android 인터페이스 구현 만들기

Xamarin.Forms 애플리케이션이 Android에서 알림을 주고받으려면 애플리케이션이 `INotificationManager` 인터페이스의 구현을 제공해야 합니다.

### <a name="create-the-androidnotificationmanager-class"></a>AndroidNotificationManager 클래스 만들기

`AndroidNotificationManager` 클래스는 `INotificationManager` 인터페이스를 구현합니다.

```csharp
using Android.Support.V4.App;
using Xamarin.Forms;
using AndroidApp = Android.App.Application;

[assembly: Dependency(typeof(LocalNotifications.Droid.AndroidNotificationManager))]
namespace LocalNotifications.Droid
{
    public class AndroidNotificationManager : INotificationManager
    {
        const string channelId = "default";
        const string channelName = "Default";
        const string channelDescription = "The default channel for notifications.";
        const int pendingIntentId = 0;

        public const string TitleKey = "title";
        public const string MessageKey = "message";

        bool channelInitialized = false;
        int messageId = -1;
        NotificationManager manager;

        public event EventHandler NotificationReceived;

        public void Initialize()
        {
            CreateNotificationChannel();
        }

        public int ScheduleNotification(string title, string message)
        {
            if (!channelInitialized)
            {
                CreateNotificationChannel();
            }

            messageId++;

            Intent intent = new Intent(AndroidApp.Context, typeof(MainActivity));
            intent.PutExtra(TitleKey, title);
            intent.PutExtra(MessageKey, message);

            PendingIntent pendingIntent = PendingIntent.GetActivity(AndroidApp.Context, pendingIntentId, intent, PendingIntentFlags.OneShot);

            NotificationCompat.Builder builder = new NotificationCompat.Builder(AndroidApp.Context, channelId)
                .SetContentIntent(pendingIntent)
                .SetContentTitle(title)
                .SetContentText(message)
                .SetLargeIcon(BitmapFactory.DecodeResource(AndroidApp.Context.Resources, Resource.Drawable.xamagonBlue))
                .SetSmallIcon(Resource.Drawable.xamagonBlue)
                .SetDefaults((int)NotificationDefaults.Sound | (int)NotificationDefaults.Vibrate);

            var notification = builder.Build();
            manager.Notify(messageId, notification);

            return messageId;
        }

        public void ReceiveNotification(string title, string message)
        {
            var args = new NotificationEventArgs()
            {
                Title = title,
                Message = message,
            };
            NotificationReceived?.Invoke(null, args);
        }

        void CreateNotificationChannel()
        {
            manager = (NotificationManager)AndroidApp.Context.GetSystemService(AndroidApp.NotificationService);

            if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
            {
                var channelNameJava = new Java.Lang.String(channelName);
                var channel = new NotificationChannel(channelId, channelNameJava, NotificationImportance.Default)
                {
                    Description = channelDescription
                };
                manager.CreateNotificationChannel(channel);
            }

            channelInitialized = true;
        }
    }
}
```

네임스페이스 위의 `assembly` 특성은 `INotificationManager` 인터페이스 구현을 `DependencyService`에 등록합니다.

Android에서 애플리케이션은 알림을 위한 여러 개의 채널을 정의할 수 있습니다. `Initialize` 메서드는 샘플 애플리케이션이 알림을 보내는 데 사용하는 기본 채널을 만듭니다. `ScheduleNotification` 메서드는 알림을 만들고 보내는 데 필요한 플랫폼별 논리를 정의합니다. 마지막으로 `ReceiveNotification` 메서드는 메시지가 수신될 때 Android OS에 의해 호출되며, 이벤트 처리기를 호출합니다.

> [!NOTE]
> `Application` 클래스는 `Xamarin.Forms` 네임스페이스와 `Android.App` 네임스페이스 모두에서 정의되므로 둘을 구별하기 위해 `using` 문에서 `AndroidApp` 별칭이 정의됩니다.

### <a name="handle-incoming-notifications-on-android"></a>Android에서 들어오는 알림 처리

`MainActivity` 클래스는 들어오는 알림을 검색하여 `AndroidNotificationManager` 인스턴스에 알려야 합니다. `MainActivity` 클래스의 `Activity` 특성은 `LaunchMode.SingleTop`의 `LaunchMode` 값을 지정해야 합니다.

```csharp
[Activity(
        //...
        LaunchMode = LaunchMode.SingleTop]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        // ...
    }
```

`SingleTop` 모드는 애플리케이션이 포그라운드에 있는 동안 `Activity`의 여러 인스턴스가 시작되지 않도록 합니다. 이 `LaunchMode`는 보다 복잡한 알림 시나리오에서 여러 활동을 시작하는 애플리케이션에는 적합하지 않을 수 있습니다. `LaunchMode` 열거형 값에 대한 자세한 내용은 [Android 활동 LaunchMode](https://developer.android.com/guide/topics/manifest/activity-element#lmode)를 참조하세요.

`MainActivity`에서 클래스는 들어오는 알림을 받도록 수정됩니다.

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    // ...

    global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
    LoadApplication(new App());
    CreateNotificationFromIntent(Intent);
}

protected override void OnNewIntent(Intent intent)
{
    CreateNotificationFromIntent(intent);
}

void CreateNotificationFromIntent(Intent intent)
{
    if (intent?.Extras != null)
    {
        string title = intent.Extras.GetString(AndroidNotificationManager.TitleKey);
        string message = intent.Extras.GetString(AndroidNotificationManager.MessageKey);
        DependencyService.Get<INotificationManager>().ReceiveNotification(title, message);
    }
}
```

`CreateNotificationFromIntent` 메서드는 `intent` 인수에서 알림 데이터를 추출하고 `ReceiveNotification` 메서드를 사용하여 `AndroidNotificationManager`에 제공합니다. `CreateNotificationFromIntent` 메서드는 `OnCreate` 메서드와 `OnNewIntent` 메서드에서 호출됩니다.

- 알림 데이터에 의해 애플리케이션이 시작되면 `Intent` 데이터가 `OnCreate` 메서드에 전달됩니다.
- 애플리케이션이 이미 전경에 있는 경우 `Intent` 데이터가 `OnNewIntent` 메서드에 전달됩니다.

Android는 여러 고급 알림 옵션을 제공합니다. 자세한 내용은 [Xamarin.Android의 알림](~/android/app-fundamentals/notifications/index.md)을 참조하세요.

## <a name="create-the-ios-interface-implementation"></a>iOS 인터페이스 구현 만들기

Xamarin.Forms 애플리케이션이 iOS에서 알림을 주고받으려면 애플리케이션이 `INotificationManager`의 구현을 제공해야 합니다.

### <a name="create-the-iosnotificationmanager-class"></a>iOSNotificationManager 클래스 만들기

`iOSNotificationManager` 클래스가 `INotificationManager` 인터페이스를 구현합니다.

```csharp
[assembly: Dependency(typeof(LocalNotifications.iOS.iOSNotificationManager))]
namespace LocalNotifications.iOS
{
    public class iOSNotificationManager : INotificationManager
    {
        int messageId = -1;

        bool hasNotificationsPermission;

        public event EventHandler NotificationReceived;

        public void Initialize()
        {
            // request the permission to use local notifications
            UNUserNotificationCenter.Current.RequestAuthorization(UNAuthorizationOptions.Alert, (approved, err) =>
            {
                hasNotificationsPermission = approved;
            });
        }

        public int ScheduleNotification(string title, string message)
        {
            // EARLY OUT: app doesn't have permissions
            if(!hasNotificationsPermission)
            {
                return -1;
            }

            messageId++;

            var content = new UNMutableNotificationContent()
            {
                Title = title,
                Subtitle = "",
                Body = message,
                Badge = 1
            };

            // Local notifications can be time or location based
            // Create a time-based trigger, interval is in seconds and must be greater than 0
            var trigger = UNTimeIntervalNotificationTrigger.CreateTrigger(0.25, false);

            var request = UNNotificationRequest.FromIdentifier(messageId.ToString(), content, trigger);
            UNUserNotificationCenter.Current.AddNotificationRequest(request, (err) =>
            {
                if (err != null)
                {
                    throw new Exception($"Failed to schedule notification: {err}");
                }
            });

            return messageId;
        }

        public void ReceiveNotification(string title, string message)
        {
            var args = new NotificationEventArgs()
            {
                Title = title,
                Message = message
            };
            NotificationReceived?.Invoke(null, args);
        }
    }
}
```

네임스페이스 위의 `assembly` 특성은 `INotificationManager` 인터페이스 구현을 `DependencyService`에 등록합니다.

iOS에서는 알림 예약을 시도하기 전에 알림을 사용할 권한을 요청해야 합니다. `Initialize` 메서드는 로컬 알림을 사용할 수 있는 권한을 요청합니다. `ScheduleNotification` 메서드는 알림을 만들고 보내는 데 필요한 논리를 정의합니다. 마지막으로 `ReceiveNotification` 메서드는 메시지가 수신될 때 iOS에 의해 호출되며, 이벤트 처리기를 호출합니다.

### <a name="handle-incoming-notifications-on-ios"></a>iOS에서 들어오는 알림 처리

iOS에서 들어오는 메시지를 처리하려면 `UNUserNotificationCenterDelegate`를 서브클래스화하는 대리자를 만들어야 합니다. 샘플 애플리케이션은 `iOSNotificationReceiver` 클래스를 정의합니다.

```csharp
public class iOSNotificationReceiver : UNUserNotificationCenterDelegate
{
    public override void WillPresentNotification(UNUserNotificationCenter center, UNNotification notification, Action<UNNotificationPresentationOptions> completionHandler)
    {
        DependencyService.Get<INotificationManager>().ReceiveNotification(notification.Request.Content.Title, notification.Request.Content.Body);

        // alerts are always shown for demonstration but this can be set to "None"
        // to avoid showing alerts if the app is in the foreground
        completionHandler(UNNotificationPresentationOptions.Alert);
    }
}
```

이 클래스는 `DependencyService`를 사용하여 `iOSNotificationManager` 클래스의 인스턴스를 가져오고, 들어오는 알림 데이터를 `ReceiveNotification` 메서드에 제공합니다.

`AppDelegate` 클래스는 애플리케이션 시작 중에 사용자 지정 대리자를 지정해야 합니다. `AppDelegate` 클래스는 애플리케이션 시작 중에 `iOSNotificationReceiver` 개체를 `UNUserNotificationCenter` 대리자로 지정해야 합니다. 이는 `FinishedLaunching` 메서드에서 발생합니다.

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    global::Xamarin.Forms.Forms.Init();

    UNUserNotificationCenter.Current.Delegate = new iOSNotificationReceiver();

    LoadApplication(new App());
    return base.FinishedLaunching(app, options);
}
```

iOS는 여러 고급 알림 옵션을 제공합니다. 자세한 내용은 [Xamarin.iOS의 알림](~/ios/platform/user-notifications/index.md)을 참조하세요.

## <a name="test-the-application"></a>애플리케이션 테스트

플랫폼 프로젝트에 `INotificationManager` 인터페이스의 등록된 구현이 포함되면 두 플랫폼 모두에서 애플리케이션을 테스트할 수 있습니다. 애플리케이션을 실행하고 **알림 예약** 단추를 클릭하여 알림을 만듭니다.

Android에서는 알림 영역에 알림이 표시됩니다. 알림을 탭하면 애플리케이션이 알림을 수신하고 **알림 예약** 단추 아래 메시지를 표시합니다.

![Android의 로컬 알림](local-notifications-images/local-notifications-android.png)

iOS에서는 들어오는 알림이 사용자 입력 필요없이 애플리케이션에 의해 자동으로 수신됩니다. 애플리케이션이 알림을 수신하고 **알림 예약** 단추 아래에 메시지를 표시합니다.

![iOS의 로컬 알림](local-notifications-images/local-notifications-ios.png)

## <a name="related-links"></a>관련 링크

- [샘플 프로젝트](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/local-notifications)
- [Xamarin.Android의 알림](~/android/app-fundamentals/notifications/index.md)
- [Xamarin.iOS의 알림](~/ios/platform/user-notifications/index.md)
- [Xamarin.Forms Dependency.Service](~/xamarin-forms/app-fundamentals/dependency-service/introduction.md)
