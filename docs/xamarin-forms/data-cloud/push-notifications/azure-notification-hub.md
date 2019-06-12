---
title: Azure Notification Hubs 및 Xamarin.Forms를 사용 하 여 푸시 알림을 보내고 받도록
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에 플랫폼 간 푸시 알림을 보내는 Azure Notification Hubs를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 07D13195-3A0D-4C95-ACF0-143A9084973C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 05/23/2019
ms.openlocfilehash: 474398922bf00e3a430166d8b2e073d200e6ed6e
ms.sourcegitcommit: c2bba24233624c2ec0e9ee9827310ca022212a2c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66835414"
---
# <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-xamarinforms"></a>Azure Notification Hubs 및 Xamarin.Forms를 사용 하 여 푸시 알림을 보내고 받도록

[![샘플 다운로드](~/media/shared/download.png)샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/WebServices/AzureNotificationHub)

모바일 응용 프로그램에 백 엔드 시스템에서 알림을 제공할 정보를 푸시하십시오. Apple, Google 및 기타 플랫폼은 각 자신의 푸시 알림 서비스 (PNS) 있어야 합니다. Azure Notification Hubs를 사용 하면 백 엔드 응용 프로그램 배포 각 플랫폼별 PNS로 알림을 처리 하는 단일 허브와 통신할 수 있도록 플랫폼 알림을 중앙 집중화할 수 있습니다.

다음이 단계를 수행 하 여 모바일 앱에 Azure Notification Hubs를 통합 합니다.

1. [푸시 알림 서비스 및 Azure 알림 허브 설정](#set-up-push-notification-services-and-azure-notification-hub)합니다.
1. [템플릿 및 태그를 사용 하는 방법을 이해](#register-templates-and-tags-with-the-azure-notification-hub)합니다.
1. [플랫폼 간 Xamarin.Forms 응용 프로그램을 만드는](#xamarinforms-application-functionality)합니다.
1. [푸시 알림에 대 한 네이티브 Android 프로젝트 구성](#configure-the-android-application-for-notifications)합니다.
1. [푸시 알림에 대 한 네이티브 iOS 프로젝트 구성](#configure-ios-for-notifications)합니다.
1. [Azure 알림 허브를 사용 하 여 알림을 테스트](#test-notifications-in-the-azure-portal)합니다.
1. [알림을 보내도록 백 엔드 응용 프로그램을 만드는](#create-a-notification-dispatcher)합니다.

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>푸시 알림 서비스 및 Azure 알림 허브 설정

Xamarin.Forms 모바일 앱을 사용 하 여 Azure Notification Hubs를 통합 하는 것을 Xamarin 네이티브 응용 프로그램을 사용 하 여 Azure Notification Hubs를 통합 하는 것과 비슷합니다. 설정 된 **FCM 응용 프로그램** Firebase 콘솔 단계에 따라 [푸시 알림을 Azure Notification Hubs를 사용 하 여 xamarin.android](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm#create-a-firebase-project-and-enable-firebase-cloud-messaging). Xamarin.Android 자습서를 사용 하 여 다음 단계를 수행 합니다.

1. Android 패키지 이름에는 같은 정의 `com.xamarin.notifysample`, 샘플에서 사용 됩니다.
1. 다운로드 **google-services.json** Firebase 콘솔에서. 이후 단계에서 Android 응용 프로그램에이 파일을 추가 합니다.
1. Azure 알림 허브 인스턴스를 만들고 이름을 지정 합니다. 이 문서 및 샘플에서는 `xdocsnotificationhub` 허브 이름으로 합니다.
1. 복사는 FCM **서버 키** 로 저장 합니다 **API 키** 아래에 있는 **Google (GCM/FCM)** Azure 알림 허브에 합니다.

다음 스크린샷에서 Azure 알림 허브에 Google 플랫폼 구성을 보여줍니다.

![Azure 알림 허브 Google 구성 스크린샷](azure-notification-hub-images/fcm-notification-hub-config.png "Azure 알림 허브 Google 구성")

MacOS 컴퓨터를 iOS 장치에 대 한 설치를 완료 해야 합니다. 초기 단계를 수행 하 여 APNS 설정 [푸시 알림을 Azure Notification Hubs를 사용 하 여 xamarin.ios](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started#generate-the-certificate-signing-request-file)합니다. Xamarin.iOS 자습서를 사용 하 여 다음 단계를 수행 합니다.

1. IOS 번들 식별자를 정의 합니다. 이 문서 및 샘플에서는 `com.xamarin.notifysample` 번들 식별자로.
1. 서명 요청 (CSR (인증서) 파일을 만들고 사용 하 여 푸시 알림 인증서를 생성 합니다.
1. 푸시 알림 인증서 업로드 **Apple (APNS)** Azure 알림 허브에 있습니다.

다음 스크린샷에서 Azure 알림 허브에 Apple 플랫폼 구성을 보여줍니다.

![Azure 알림 허브 Apple 구성 스크린샷](azure-notification-hub-images/apns-notification-hub-config.png "Azure 알림 허브 Apple 구성")

## <a name="register-templates-and-tags-with-the-azure-notification-hub"></a>Azure 알림 허브를 사용 하 여 템플릿 및 태그를 등록

Azure 알림 허브에는 허브를 사용 하 여 등록, 템플릿을 정의 하 고 태그를 구독할 모바일 응용 프로그램에 필요 합니다. 플랫폼별 PNS 핸들을 Azure 알림 허브의 식별자를 연결 하는 등록 합니다. 등록에 대 한 자세한 내용은 참조 하세요 [등록 관리](/azure/notification-hubs/notification-hubs-push-notification-registration-management)합니다.

템플릿 매개 변수가 있는 메시지 템플릿을 지정 하는 장치를 허용 합니다. 장치별로 태그 당 들어오는 메시지를 사용자 지정할 수 있습니다. 템플릿에 대 한 자세한 내용은 참조 하세요 [템플릿](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)합니다.

뉴스, 스포츠, 날씨 등 메시지 범주를 구독할 태그를 사용할 수 있습니다. 라는 단일 매개 변수를 사용 하 여 샘플 응용 프로그램 정의 기본 템플릿을 편의상 `messageParam` 라는 단일 태그 및 `default`합니다. 더 복잡 한 시스템에서 사용자 고유의 태그 사용자 개인 설정 된 알림에 대 한 장치에서 메시지를 사용할 수 있습니다. 태그에 대 한 자세한 내용은 참조 하세요 [라우팅 및 태그 식](/azure/notification-hubs/notification-hubs-tags-segment-push-message)합니다.

메시지를 받으려면 각 네이티브 응용 프로그램이 단계를 수행 해야 합니다.

1. 플랫폼 PNS에서에서 PNS 핸들 또는 토큰을 가져옵니다.
1. Azure 알림 허브 PNS 핸들을 등록 합니다.
1. 보내는 메시지와 동일한 매개 변수를 포함 하는 서식 파일을 지정 합니다.
1. 나가는 메시지를 대상으로 하는 태그에 가입 합니다.

다음이 단계에서 각 플랫폼에 대해 자세히 설명 합니다 [알림에 대 한 Android 응용 프로그램을 구성](#configure-the-android-application-for-notifications) 및 [iOS 알림에 대 한 구성](#configure-ios-for-notifications) 섹션.

## <a name="xamarinforms-application-functionality"></a>Xamarin.Forms 응용 프로그램 기능

샘플 Xamarin.Forms 응용 프로그램에 푸시 알림 메시지의 목록을 표시합니다. 이 통해를 `AddMessage` 메서드를 UI에 지정 된 푸시 알림 메시지를 추가 합니다. 또한이 메서드는 UI에 추가 되 고 중복 메시지를 방지 하 고 모든 스레드에서 호출 될 수 있도록 주 스레드에서 실행 됩니다. 다음 코드에서는 `AddMessage` 메서드를 보여 줍니다.

```csharp
public void AddMessage(string message)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (messageDisplay.Children.OfType<Label>().Where(c => c.Text == message).Any())
        {
            // Do nothing, an identical message already exists
        }
        else
        {
            Label label = new Label()
            {
                Text = message,
                HorizontalOptions = LayoutOptions.CenterAndExpand,
                VerticalOptions = LayoutOptions.Start
            };
            messageDisplay.Children.Add(label);
        }
    });
}
```

샘플 응용 프로그램에는 **AppConstants.cs** 플랫폼 프로젝트에서 사용 되는 속성을 정의 하는 파일입니다. 이 파일은 Azure 알림 허브에서 값을 사용 하 여 사용자 지정 해야 합니다. 다음 코드에서는 합니다 **AppConstants.cs** 파일:

```csharp
public static class AppConstants
{
    public static string NotificationChannelName { get; set; } = "XamarinNotifyChannel";
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string ListenConnectionString { get; set; } = "< Insert your DefaultListenSharedAccessSignature >";
    public static string DebugTag { get; set; } = "XamarinNotify";
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string FCMTemplateBody { get; set; } = "{\"data\":{\"message\":\"$(messageParam)\"}}";
    public static string APNTemplateBody { get; set; } = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";
}
```

다음 값을 사용자 지정 `AppConstants` 샘플 응용 프로그램을 Azure 알림 허브에 연결 하려면:

* `NotificationHubName`: Azure portal에서 만든 Azure 알림 허브의 이름을 사용 합니다.
* `ListenConnectionString`: 이 값에서 Azure 알림 허브에 위치한 **액세스 정책**합니다.

다음 스크린샷은 Azure portal에서 이러한 값은 위치 보여 줍니다.

![Azure 알림 허브 액세스 정책 스크린샷](azure-notification-hub-images/notification-hub-access-policy.png "Azure 알림 허브 액세스 정책")

## <a name="configure-the-android-application-for-notifications"></a>알림에 대 한 Android 응용 프로그램 구성

수신 하 고 알림 처리 Android 응용 프로그램을 구성 하려면 다음 단계를 완료 합니다.

1. Android 구성 **패키지 이름을** Firebase 콘솔에서 패키지 이름과 일치 하도록 합니다.
1. Google Play, Firebase 및 Azure Notification Hubs와 상호 작용할 다음 NuGet 패키지를 설치 합니다.
    1. Xamarin.GooglePlayServices.Base.
    1. Xamarin.Firebase.Messaging.
    1. Xamarin.Azure.NotificationHubs.Android.
1. 복사 합니다 `google-services.json` 프로젝트에 FCM 설치 하는 동안 다운로드 하 고 빌드 작업을 설정 하는 파일 `GoogleServicesJson`합니다.
1. [Firebase 통신할 AndroidManifest.xml 구성](#configure-android-manifest)합니다.
1. [Firebase 및 Azure 알림 허브를 사용 하 여 응용 프로그램 등록 사용 하는 `FirebaseInstanceIdService` ](#register-using-a-custom-firebaseinstanceidservice)합니다.
1. [사용 하 여 메시지 처리를 `FirebaseMessagingService` ](#process-messages-with-a-firebasemessagingservice)합니다.
1. [Xamarin.Forms UI 들어오는 알림을 추가할](#add-incoming-notifications-to-the-xamarinforms-ui)합니다.

> [!NOTE]
> 합니다 **GoogleServicesJson** 빌드 작업의 일부인 합니다 **Xamarin.GooglePlayServices.Base** NuGet 패키지. Visual Studio 2019 시작 하는 동안 사용 가능한 빌드 작업을 설정합니다. 표시 되지 않으면 **GoogleServicesJson** 빌드 작업으로 NuGet 패키지를 설치한 후 Visual Studio 2019를 다시 시작 합니다.

### <a name="configure-android-manifest"></a>Android 매니페스트 구성

합니다 `receiver` 내의 요소는 `application` Firebase와 통신 하도록 앱을 허용 하는 요소입니다. `uses-permission` 요소 메시지를 처리 하 고 Azure 알림 허브를 사용 하 여 등록 하도록 앱을 허용 합니다. 전체 **AndroidManifest.xml** 아래 예제와 비슷하게 표시 됩니다.

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="YOUR_PACKAGE_NAME" android:installLocation="auto">
  <uses-sdk android:minSdkVersion="21" />
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
  <uses-permission android:name="android.permission.WAKE_LOCK" />
  <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
  <application android:label="Notification Hub Sample">
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
      <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
      </intent-filter>
    </receiver>
  </application>
</manifest>
```

### <a name="register-using-a-custom-firebaseinstanceidservice"></a>사용자 지정 FirebaseInstanceIdService를 사용 하 여 등록

Firebase는 PNS에서 장치를 고유 하 게 식별 하는 토큰을 발급 합니다. 토큰 수명은 긴 하지만 경우에 따라 새로 고쳐집니다. 토큰을 발급 하거나 새로 고칠 응용 프로그램을 Azure 알림 허브를 사용 하 여 새 토큰을 등록 해야 합니다. 등록에서 파생 된 클래스의 인스턴스에 의해 처리 됩니다 `FirebaseInstanceIdService`합니다.

샘플 응용 프로그램에서 `FirebaseRegistrationService` 클래스에서 상속 `FirebaseInstanceIdService`합니다. 이 클래스에는 `IntentFilter` 포함 된 `com.google.firebase.INSTANCE_ID_EVENT`, Android OS를 자동으로 호출을 허용 `OnTokenRefresh` Firebase에서 토큰을 발급 한 경우.

다음 코드에서는 사용자 지정 `FirebaseInstanceIdService` 샘플 응용 프로그램에서:

```csharp
[Service]
[IntentFilter(new [] { "com.google.firebase.INSTANCE_ID_EVENT"})]
public class FirebaseRegistrationService : FirebaseInstanceIdService
{
    public override void OnTokenRefresh()
    {
        string token = FirebaseInstanceId.Instance.Token;

        // NOTE: logging the token is not recommended in production but during
        // development it is useful to test messages directly from Firebase
        Log.Info(AppConstants.DebugTag, $"Token received: {token}");

        SendRegistrationToServer(token);
    }

    void SendRegistrationToServer(string token)
    {
        try
        {
            NotificationHub hub = new NotificationHub(AppConstants.NotificationHubName, AppConstants.ListenConnectionString, this);

            // register device with Azure Notification Hub using the token from FCM
            Registration reg = hub.Register(token, AppConstants.SubscriptionTags);

            // subscribe to the SubscriptionTags list with a simple template.
            string pnsHandle = reg.PNSHandle;
            var cats = string.Join(", ", reg.Tags);
            var temp = hub.RegisterTemplate(pnsHandle, "defaultTemplate", AppConstants.FCMTemplateBody, AppConstants.SubscriptionTags);
        }
        catch (Exception e)
        {
            Log.Error(AppConstants.DebugTag, $"Error registering device: {e.Message}");
        }
    }
}
```

합니다 `SendRegistrationToServer` 의 메서드는 `FirebaseRegistrationClass` Azure 알림 허브를 사용 하 여 장치를 등록 하 고 템플릿 사용 하 여 태그를 구독 합니다. 호출을 단일 태그를 정의 하는 샘플 응용 프로그램 `default` 단일 매개 변수를 사용 하 여 템플릿 호출 `messageParam` 에 **AppConstants.cs** 파일입니다. 등록, 태그 및 템플릿에 대 한 자세한 내용은 참조 하세요. [Azure 알림 허브를 사용 하 여 템플릿 및 태그를 등록](#register-templates-and-tags-with-the-azure-notification-hub)

### <a name="process-messages-with-a-firebasemessagingservice"></a>FirebaseMessagingService 사용 하 여 메시지 처리

들어오는 메시지가 라우트되는 `FirebaseMessagingService` 인스턴스를 로컬 알림을으로 변환 될 수 있습니다. 호출 하는 클래스를 포함 하는 샘플 응용 프로그램에서 Android 프로젝트 `FirebaseService` 에서 상속 되는 `FirebaseMessagingService`합니다. 이 클래스에는 `IntentFilter` 포함 된 `com.google.firebase.MESSAGING_EVENT`, Android OS를 자동으로 호출을 허용 `OnMessageReceived` 푸시 알림 메시지가 수신 되는 경우.

다음 예제는 `FirebaseService` 샘플 응용 프로그램에서:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseService : FirebaseMessagingService
{
    public override void OnMessageReceived(RemoteMessage message)
    {
        base.OnMessageReceived(message);
        string messageBody = string.Empty;

        if (message.GetNotification() != null)
        {
            messageBody = message.GetNotification().Body;
        }

        // NOTE: test messages sent via the Azure portal will be received here
        else
        {
            messageBody = message.Data.Values.First();
        }

        // convert the incoming message to a local notification
        SendLocalNotification(messageBody);

        // send the incoming message directly to the MainPage
        SendMessageToMainPage(messageBody);
    }

    void SendLocalNotification(string body)
    {
        var intent = new Intent(this, typeof(MainActivity));
        intent.AddFlags(ActivityFlags.ClearTop);
        intent.PutExtra("message", body);
        var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

        var notificationBuilder = new NotificationCompat.Builder(this)
            .SetContentTitle("XamarinNotify Message")
            .SetSmallIcon(Resource.Drawable.ic_launcher)
            .SetContentText(body)
            .SetAutoCancel(true)
            .SetShowWhen(false)
            .SetContentIntent(pendingIntent);

        if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
        {
            notificationBuilder.SetChannelId(AppConstants.NotificationChannelName);
        }

        var notificationManager = NotificationManager.FromContext(this);
        notificationManager.Notify(0, notificationBuilder.Build());
    }

    void SendMessageToMainPage(string body)
    {
        (App.Current.MainPage as MainPage)?.AddMessage(body);
    }
}
```

들어오는 메시지와 로컬 알림을로 변환할지를 `SendLocalNotification` 메서드. 이 메서드를 만듭니다 `Intent` 메시지 콘텐츠를 배치 합니다 `Intent` 으로 `string` `Extra`합니다. 포그라운드 또는 백그라운드에서 앱이 있는지 여부를 사용자가 로컬 알림을 누를 때 합니다 `MainActivity` 가 실행 된 후이 통해 메시지 내용에 대 한 액세스 권한이 `Intent` 개체입니다.

로컬 알림 및 `Intent` 예제를 실행 하려면 사용자가 알림을 탭의 작업을 수행 합니다. 사용자는 응용 프로그램 상태가 변경 되기 전에 작업을 수행 해야 하는 경우 이것이 바람직합니다. 그러나 다음 경우에 따라 사용자 작업 없이 메시지 데이터에 액세스 하는 것이 좋습니다. 앞의 예제도 현재에 직접 메시지를 보냅니다 `MainPage` 인스턴스는 `SendMessageToMainPage` 메서드. 프로덕션 환경에서는 단일 메시지 유형에 대해 두 메서드를 구현 하는 경우는 `MainPage` 개체는 사용자가 알림을 탭 하면 중복 메시지를 얻을 수 있습니다.

> [!NOTE]
> Android 응용 프로그램이 포그라운드 또는 백그라운드에서 실행 중인 경우 푸시 알림을 받습니다. 푸시 알림의 받을 때 주 `Activity` 는 실행 되지 않는이 샘플의 범위를 벗어납니다 서비스를 구현 해야 합니다. 자세한 내용은 참조 하세요. [Android 서비스 만들기](/xamarin/android/app-fundamentals/services/)

### <a name="add-incoming-notifications-to-the-xamarinforms-ui"></a>Xamarin.Forms UI에 들어오는 알림 추가

`MainActivity` 클래스가 알림을 처리 하 고 들어오는 메시지 데이터를 관리할 수 있는 권한을 얻으려면 해야 합니다. 다음 코드에서는 전체 `MainActivity` 구현 합니다.

```csharp
[Activity(Label = "NotificationHubSample", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, LaunchMode = LaunchMode.SingleTop)]
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(savedInstanceState);

        global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
        LoadApplication(new App());

        if (IsPlayServiceAvailable() == false)
        {
            throw new Exception("This device does not have Google Play Services and cannot receive push notifications.");
        }

        CreateNotificationChannel();
    }

    protected override void OnNewIntent(Intent intent)
    {
        if (intent.Extras != null)
        {
            var message = intent.GetStringExtra("message");
            (App.Current.MainPage as MainPage)?.AddMessage(message);
        }

        base.OnNewIntent(intent);
    }

    bool IsPlayServiceAvailable()
    {
        int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
        if (resultCode != ConnectionResult.Success)
        {
            if (GoogleApiAvailability.Instance.IsUserResolvableError(resultCode))
                Log.Debug(AppConstants.DebugTag, GoogleApiAvailability.Instance.GetErrorString(resultCode));
            else
            {
                Log.Debug(AppConstants.DebugTag, "This device is not supported");
            }
            return false;
        }
        return true;
    }

    void CreateNotificationChannel()
    {
        // Notification channels are new as of "Oreo".
        // There is no need to create a notification channel on older versions of Android.
        if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
        {
            var channelName = AppConstants.NotificationChannelName;
            var channelDescription = String.Empty;
            var channel = new NotificationChannel(channelName, channelName, NotificationImportance.Default)
            {
                Description = channelDescription
            };

            var notificationManager = (NotificationManager)GetSystemService(NotificationService);
            notificationManager.CreateNotificationChannel(channel);
        }
    }
}
```

합니다 `Activity` 응용 프로그램을 설정 하는 특성 `LaunchMode` 에 `SingleTop`입니다. 이 시작 모드에만이 작업의 단일 인스턴스를 허용 하려면 Android OS 알려 줍니다. 수신이 시작 모드를 사용 하 여 `Intent` 라우팅되는 데이터를 `OnNewIntent` 메시지 데이터를 추출 하 고 전송 하는 메서드를를 `MainPage` 를 통해 인스턴스를 `AddMessage` 메서드. 응용 프로그램을 다른 시작 모드를 사용 하는 경우 처리 해야 `Intent` 데이터 다르게 합니다.

합니다 `OnCreate` 메서드 라는 도우미 메서드를 사용 하 여 `IsPlayServiceAvailable` 장치에서 Google Play 서비스를 지원 하도록 합니다. Google Play 서비스를 지원 하지 않는 장치나 에뮬레이터로 Firebase에서 푸시 알림을 받을 수 없습니다.

## <a name="configure-ios-for-notifications"></a>IOS 알림에 대 한 구성

알림을 수신 하도록 iOS 응용 프로그램을 구성 하기 위한 프로세스 다음과 같습니다.

1. 구성 된 **번들 식별자** 에 **Info.plist** 프로 비전 프로필에 사용 되는 값과 일치 하도록 파일.
1. 추가 된 **푸시 알림을 사용 하도록 설정** 옵션을 합니다 **Entitlements.plist** 파일입니다.
1. 추가 된 **Xamarin.Azure.NotificationHubs.iOS** NuGet 패키지를 프로젝트입니다.
1. [APNS 사용 하 여 알림 등록](#register-for-notifications-with-apns)합니다.
1. [Azure 알림 허브를 사용 하 여 응용 프로그램을 등록 하 고 태그에 가입](#register-with-azure-notification-hub-and-subscribe-to-tags)합니다.
1. [Xamarin.Forms UI APNS 알림을 추가할](#add-apns-notifications-to-xamarinforms-ui)합니다.

다음 스크린 샷에 표시 된 **푸시 알림을 사용 하도록 설정** 옵션을 선택 합니다 **Entitlements.plist** Visual Studio 내의 파일:

![푸시 알림 자격 스크린샷](azure-notification-hub-images/push-notification-entitlement.png "푸시 알림 권한")

### <a name="register-for-notifications-with-apns"></a>APNS 사용 하 여 알림 등록

합니다 `FinishedLaunching` 의 메서드를 **AppDelegate.cs** 원격 알림을 등록할 수 파일을 재정의 해야 합니다. 등록은 장치에서 사용 되는 iOS 버전에 따라 다릅니다. 샘플 응용 프로그램에서 iOS 프로젝트를 재정의 합니다 `FinishedLaunching` 메서드를 호출할 `RegisterForRemoteNotifications` 다음 예와에서 같이:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    global::Xamarin.Forms.Forms.Init();
    LoadApplication(new App());

    base.FinishedLaunching(app, options);

    RegisterForRemoteNotifications();

    return true;
}

void RegisterForRemoteNotifications()
{
    // register for remote notifications based on system version
    if (UIDevice.CurrentDevice.CheckSystemVersion(10, 0))
    {
        UNUserNotificationCenter.Current.RequestAuthorization(UNAuthorizationOptions.Alert |
            UNAuthorizationOptions.Sound |
            UNAuthorizationOptions.Sound,
            (granted, error) =>
            {
                if (granted)
                    InvokeOnMainThread(UIApplication.SharedApplication.RegisterForRemoteNotifications);
            });
    }
    else if (UIDevice.CurrentDevice.CheckSystemVersion(8, 0))
    {
        var pushSettings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
        new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(pushSettings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();
    }
    else
    {
        UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
        UIApplication.SharedApplication.RegisterForRemoteNotificationTypes(notificationTypes);
    }
}
```

### <a name="register-with-azure-notification-hub-and-subscribe-to-tags"></a>Azure 알림 허브에 등록 하 고 태그에 가입

장치가 성공적으로 등록 한 시 원격 알림 합니다 `FinishedLaunching` iOS 호출 메서드를를 `RegisteredForRemoteNotifications` 메서드. 다음 작업을 수행 하려면이 메서드를 재정의 해야 합니다.

1. 인스턴스화하는 `SBNotificationHub`합니다.
1. 모든 기존 등록의 등록을 취소 합니다.
1. 알림 허브에 장치를 등록 합니다.
1. 템플릿 사용 하 여 특정 태그에 가입 합니다.

장치, 템플릿 및 태그의 등록에 대 한 자세한 내용은 참조 [Azure 알림 허브를 사용 하 여 템플릿 및 태그를 등록](#register-templates-and-tags-with-the-azure-notification-hub)합니다. 다음 코드를 장치 및 템플릿을 등록 하는 방법을 보여 줍니다.

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    Hub = new SBNotificationHub(AppConstants.ListenConnectionString, AppConstants.NotificationHubName);

    // update registration with Azure Notification Hub
    Hub.UnregisterAllAsync(deviceToken, (error) =>
    {
        if (error != null)
        {
            Debug.WriteLine($"Unable to call unregister {error}");
            return;
        }

        var tags = new NSSet(AppConstants.SubscriptionTags.ToArray());
        Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                Debug.WriteLine($"RegisterNativeAsync error: {errorCallback}");
            }
        });

        var templateExpiration = DateTime.Now.AddDays(120).ToString(System.Globalization.CultureInfo.CreateSpecificCulture("en-US"));
        Hub.RegisterTemplateAsync(deviceToken, "defaultTemplate", AppConstants.APNTemplateBody, templateExpiration, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                if (errorCallback != null)
                {
                    Debug.WriteLine($"RegisterTemplateAsync error: {errorCallback}");
                }
            }
        });
    });
}
```

> [!NOTE]
> 원격 알림 신청 네트워크 연결이 없는 경우에 실패할 수 있습니다. 재정의 하도록 선택할 수 있습니다는 `FailedToRegisterForRemoveNotifications` 등록 오류를 처리 하는 방법입니다.

### <a name="add-apns-notifications-to-xamarinforms-ui"></a>Xamarin.Forms UI에 APNS 알림 추가

장치를 원격 알림을 iOS 호출을 수신 하는 경우는 `ReceivedRemoteNotification` 메서드. 들어오는 메시지를 JSON으로 변환 됩니다는 `NSDictionary` 개체와 `ProcessNotification` 메서드 사전에서 값을 추출 하 고 Xamarin.Forms 보냅니다 `MainPage` 인스턴스. 합니다 `ReceivedRemoteNotifications` 메서드를 호출 하도록 재정의 됩니다 `ProcessNotification` 다음 코드 에서처럼:

```csharp
public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
{
    ProcessNotification(userInfo, false);
}

void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
{
    // make sure we have a payload
    if (options != null && options.ContainsKey(new NSString("aps")))
    {
        // get the APS dictionary and extract message payload. Message JSON will be converted
        // into a NSDictionary so more complex payloads may require more processing
        NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
        string payload = string.Empty;
        NSString payloadKey = new NSString("alert");
        if (aps.ContainsKey(payloadKey))
        {
            payload = aps[payloadKey].ToString();
        }

        if (!string.IsNullOrWhiteSpace(payload))
        {
            (App.Current.MainPage as MainPage)?.AddMessage(payload);
        }

    }
    else
    {
        Debug.WriteLine($"Received request to process notification but there was no payload.");
    }
}
```

## <a name="test-notifications-in-the-azure-portal"></a>Azure portal에서 알림 테스트

Azure Notification Hubs를 사용 하면 응용 프로그램 테스트 메시지를 받을 수 있는지 확인할 수 있습니다. 합니다 **테스트 보내기** 알림 허브에 섹션에서는 대상 플랫폼을 선택 하 고 메시지를 보낼 수 있습니다. 설정 합니다 **태그 식으로 보내기** 하 `default` 응용 프로그램에 대 한 템플릿 등록에 메시지를 송신할는 `default` 태그입니다. 클릭 하는 **보낼** 단추 메시지와 함께 도달 하는 장치 수가 포함 된 보고서를 생성 합니다. 다음 스크린샷은 Azure portal에서는 Android 알림 테스트를 보여 줍니다.

![Azure 알림 허브 테스트 메시지의 스크린 샷](azure-notification-hub-images/azure-notification-hub-test-send.png "Azure 알림 허브 테스트 메시지")

### <a name="testing-tips"></a>테스트 팁

1. 응용 프로그램에 푸시 알림을 받을 수 있는지 테스트 하는 경우 물리적 장치를 사용 해야 합니다. 푸시 알림을 받도록 올바르게 iOS 및 android 가상 장치를 구성할 수 있습니다.
1. 샘플 Android 응용 프로그램 Firebase 토큰을 발급 하는 경우 되 면 해당 토큰과 템플릿을 등록 합니다. 테스트 중에 새 토큰을 요청 하 여 Azure 알림 허브를 사용 하 여 다시 등록 해야 합니다. 프로젝트 정리, 삭제 하는 가장 좋은 방법은 강제적 합니다 `bin` 및 `obj` 폴더를 다시 빌드하고 배포 하기 전에 장치에서 응용 프로그램을 제거 합니다.
1. 푸시 알림 흐름의 많은 부분을 비동기적으로 실행 됩니다. 중단점 적중 횟수 또는 예기치 않은 순서로 적중 하지이 될 수 있습니다. 응용 프로그램 흐름을 방해 하지 않고 실행을 추적 하도록 장치 또는 디버그 로깅을 사용 합니다. 사용 하 여 Android 장치 로그를 필터링 합니다 `DebugTag` 에 지정 된 `Constants`합니다.

## <a name="create-a-notification-dispatcher"></a>알림 발송자 만들기

Azure Notification Hubs 플랫폼 장치에 알림 디스패치를 위해 백 엔드 응용 프로그램을 사용 합니다. 이 샘플 사용 하 여 알림 디스패치를 보여 줍니다.는 **NotificationDispatcher** 콘솔 응용 프로그램입니다. 응용 프로그램에는 **DispatcherConstants.cs** 파일에 다음 속성을 정의 합니다.

```csharp
public static class DispatcherConstants
{
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string FullAccessConnectionString { get; set; } = "< Insert your DefaultFullSharedAccessSignature >";
}
```

구성 해야 합니다 **DispatcherConstants.cs** 에 맞게 Azure 알림 허브 구성과 일치 합니다. 값을 `SubscriptionTags` 속성에는 클라이언트 앱에서 사용 되는 값과 일치 해야 합니다. `NotificationHubName` 속성은 Azure 알림 허브 인스턴스의 이름입니다. 합니다 `FullAccessConnectionString` 속성은 알림 허브에 대 한 액세스 키 **액세스 정책**합니다. 다음 스크린샷은의 위치를 `NotificationHubName` 및 `FullAccessConnectionString` Azure 포털의 속성:

![Azure 알림 허브 이름 및 FullAccessConnectionString 스크린 샷](azure-notification-hub-images/notification-hub-full-access-policy.png "Azure 알림 허브 이름과 FullAccessConnectionString")

콘솔 응용 프로그램은 각 반복 `SubscriptionTags` 의 인스턴스를 사용 하 여 구독자에 게 알림을 보내고 값은 `NotificationHubClient` 클래스입니다. 다음 코드는 콘솔 응용 프로그램을 보여 줍니다. `Program` 클래스:

``` csharp
class Program
{
    static int messageCount = 0;

    static void Main(string[] args)
    {
        Console.WriteLine($"Press the spacebar to send a message to each tag in {string.Join(", ", DispatcherConstants.SubscriptionTags)}");
        WriteSeparator();
        while (Console.ReadKey().Key == ConsoleKey.Spacebar)
        {
            SendTemplateNotificationsAsync().GetAwaiter().GetResult();
        }
    }

    private static async Task SendTemplateNotificationsAsync()
    {
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(DispatcherConstants.FullAccessConnectionString, DispatcherConstants.NotificationHubName);
        Dictionary<string, string> templateParameters = new Dictionary<string, string>();

        messageCount++;

        // Send a template notification to each tag. This will go to any devices that
        // have subscribed to this tag with a template that includes "messageParam"
        // as a parameter
        foreach (var tag in DispatcherConstants.SubscriptionTags)
        {
            templateParameters["messageParam"] = $"Notification #{messageCount} to {tag} category subscribers!";
            try
            {
                await hub.SendTemplateNotificationAsync(templateParameters, tag);
                Console.WriteLine($"Sent message to {tag} subscribers.");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Failed to send template notification: {ex.Message}");
            }
        }

        Console.WriteLine($"Sent messages to {DispatcherConstants.SubscriptionTags.Length} tags.");
        WriteSeparator();
    }

    private static void WriteSeparator()
    {
        Console.WriteLine("==========================================================================");
    }
}
```

샘플 콘솔 응용 프로그램을 실행 하는 경우 메시지를 보낼 수 스페이스바를 누를 수 있습니다. 번호가 매겨진된 알림, 응용 프로그램에서 수신 해야 하는 클라이언트를 실행 하는 장치 제대로 구성 되어 제공 됩니다.

## <a name="related-links"></a>관련 링크

* [푸시 알림 템플릿](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)합니다.
* [장치 등록 관리](/azure/notification-hubs/notification-hubs-push-notification-registration-management)합니다.
* [라우팅 및 태그 식](/azure/notification-hubs/notification-hubs-tags-segment-push-message)합니다.
* [Xamarin.Android Azure Notification Hubs 자습서](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)합니다.
* [Xamarin.iOS Azure Notification Hubs 자습서](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)합니다.
