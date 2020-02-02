---
title: Azure Notification Hubs 및 Xamarin.ios를 사용 하 여 푸시 알림 보내기 및 받기
description: 이 문서에서는 Azure Notification Hubs를 사용 하 여 Xamarin Forms 응용 프로그램에 플랫폼 간 푸시 알림을 보내는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 07D13195-3A0D-4C95-ACF0-143A9084973C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/27/2019
ms.openlocfilehash: 8b633481d74810bc4d86d68f8c36d55980092510
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940315"
---
# <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-xamarinforms"></a>Azure Notification Hubs 및 Xamarin.ios를 사용 하 여 푸시 알림 보내기 및 받기

[![샘플 다운로드](~/media/shared/download.png)샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurenotificationhub/)

푸시 알림은 백 엔드 시스템에서 모바일 응용 프로그램으로 정보를 제공 합니다. Apple, Google 및 기타 플랫폼에는 각각 자체 푸시 알림 서비스 (PNS)가 있습니다. Azure Notification Hubs를 사용 하면 여러 플랫폼에서 알림을 중앙 집중화 하 여 백 엔드 응용 프로그램에서 단일 허브와 통신할 수 있으므로 각 플랫폼별 PNS 알림을 배포할 수 있습니다.

다음 단계를 수행 하 여 모바일 앱에 Azure Notification Hubs를 통합 합니다.

1. [푸시 Notification Services 및 Azure 알림 허브를 설정](#set-up-push-notification-services-and-azure-notification-hub)합니다.
1. [템플릿과 태그를 사용 하는 방법을 이해](#register-templates-and-tags-with-the-azure-notification-hub)합니다.
1. [플랫폼 간 Xamarin Forms 응용 프로그램을 만듭니다](#xamarinforms-application-functionality).
1. [푸시 알림에 대 한 네이티브 Android 프로젝트를 구성](#configure-the-android-application-for-notifications)합니다.
1. [푸시 알림에 대 한 네이티브 iOS 프로젝트를 구성](#configure-ios-for-notifications)합니다.
1. [Azure 알림 허브를 사용 하 여 알림을 테스트](#test-notifications-in-the-azure-portal)합니다.
1. [알림을 보내는 백 엔드 응용 프로그램을 만듭니다](#create-a-notification-dispatcher).

> [!NOTE]
> [Azure 구독](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://aka.ms/azfree-docs-mobileapps)을 만듭니다.

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>푸시 Notification Services 및 Azure Notification Hub 설정

Azure Notification Hubs를 Xamarin.ios 모바일 앱과 통합 하는 것은 Azure Notification Hubs를 Xamarin 네이티브 응용 프로그램과 통합 하는 것과 비슷합니다. [Azure Notification Hubs를 사용 하 여 xamarin.ios에 대 한 푸시 알림](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm#create-a-firebase-project-and-enable-firebase-cloud-messaging)의 Firebase 콘솔 단계를 수행 하 여 **FCM 응용 프로그램** 을 설정 합니다. Xamarin.ios 자습서를 사용 하 여 다음 단계를 완료 합니다.

1. 예제에서 사용 되는 `com.xamarin.notifysample`와 같은 Android 패키지 이름을 정의 합니다.
1. Firebase 콘솔에서 **google-service** 를 다운로드 합니다. 이후 단계에서 Android 응용 프로그램에이 파일을 추가 합니다.
1. Azure 알림 허브 인스턴스를 만들고 이름을 지정 합니다. 이 문서 및 샘플에서는 `xdocsnotificationhub`을 허브 이름으로 사용 합니다.
1. FCM **서버 키** 를 복사 하 고 Azure Notification Hub의 **Google (GCM/FCM)** 아래에 **API 키** 로 저장 합니다.

다음 스크린샷은 Azure 알림 허브의 Google platform 구성을 보여 줍니다.

![Azure 알림 허브 Google 구성의 스크린샷](azure-notification-hub-images/fcm-notification-hub-config.png "Azure 알림 허브 Google 구성")

IOS 장치에 대 한 설정을 완료 하려면 macOS 컴퓨터가 필요 합니다. [Azure Notification Hubs를 사용 하 여 xamarin.ios에 대 한 푸시 알림](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started#generate-the-certificate-signing-request-file)의 초기 단계를 수행 하 여 APNS를 설정 합니다. Xamarin.ios 자습서를 사용 하 여 다음 단계를 완료 합니다.

1. IOS 번들 식별자를 정의 합니다. 이 문서 및 샘플에서는 번들 식별자로 `com.xamarin.notifysample`를 사용 합니다.
1. CSR (인증서 서명 요청) 파일을 만들고이 파일을 사용 하 여 푸시 알림 인증서를 생성 합니다.
1. Azure 알림 허브의 **Apple (APNS)** 에 푸시 알림 인증서를 업로드 합니다.

다음 스크린샷은 Azure 알림 허브의 Apple 플랫폼 구성을 보여 줍니다.

![Azure 알림 허브 Apple 구성의 스크린샷](azure-notification-hub-images/apns-notification-hub-config.png "Azure 알림 허브 Apple 구성")

## <a name="register-templates-and-tags-with-the-azure-notification-hub"></a>Azure 알림 허브에 템플릿 및 태그 등록

Azure 알림 허브를 사용 하려면 모바일 응용 프로그램이 허브에 등록 하 고, 템플릿을 정의 하 고, 태그를 구독 해야 합니다. 등록은 플랫폼별 PNS 핸들을 Azure Notification Hub의 식별자에 연결 합니다. 등록에 대해 자세히 알아보려면 [등록 관리](/azure/notification-hubs/notification-hubs-push-notification-registration-management)를 참조 하세요.

템플릿을 통해 장치에서 매개 변수가 있는 메시지 템플릿을 지정할 수 있습니다. 들어오는 메시지는 태그별로 장치 별로 사용자 지정할 수 있습니다. 템플릿에 대 한 자세한 내용은 [템플릿](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)을 참조 하세요.

태그를 사용 하 여 뉴스, 스포츠, 날씨 등의 메시지 범주를 구독할 수 있습니다. 간단히 하기 위해 샘플 응용 프로그램은 `messageParam` 라는 단일 매개 변수와 `default`라는 단일 태그를 사용 하 여 기본 템플릿을 정의 합니다. 더 복잡 한 시스템에서는 사용자별 태그를 사용 하 여 사용자가 개인 설정 된 알림을 위해 장치에서 사용자에 게 메시지를 보낼 수 있습니다. 태그에 대 한 자세한 내용은 [라우팅 및 태그 식](/azure/notification-hubs/notification-hubs-tags-segment-push-message)을 참조 하세요.

메시지를 성공적으로 수신 하려면 각 네이티브 응용 프로그램에서 다음 단계를 수행 해야 합니다.

1. 플랫폼 PNS에서 PNS 핸들 또는 토큰을 가져옵니다.
1. PNS 핸들을 Azure 알림 허브에 등록 합니다.
1. 보내는 메시지와 동일한 매개 변수를 포함 하는 템플릿을 지정 합니다.
1. 보내는 메시지의 대상으로 지정 된 태그를 구독 합니다.

이러한 단계는 알림에 대 한 [Android 응용 프로그램 구성](#configure-the-android-application-for-notifications) 및 [알림에 대 한 iOS 구성](#configure-ios-for-notifications) 섹션에서 각 플랫폼에 대해 자세히 설명 합니다.

## <a name="xamarinforms-application-functionality"></a>Xamarin Forms 응용 프로그램 기능

샘플 Xamarin Forms 응용 프로그램은 푸시 알림 메시지 목록을 표시 합니다. 이는 지정 된 푸시 알림 메시지를 UI에 추가 하는 `AddMessage` 메서드로 구현 됩니다. 또한이 메서드는 중복 메시지가 UI에 추가 되는 것을 방지 하 고 모든 스레드에서 호출 될 수 있도록 주 스레드에서 실행 됩니다. 다음 코드에서는 `AddMessage` 메서드를 보여 줍니다.

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

샘플 응용 프로그램에는 플랫폼 프로젝트에서 사용 하는 속성을 정의 하는 **AppConstants.cs** 파일이 포함 되어 있습니다. 이 파일은 Azure 알림 허브의 값으로 사용자 지정 해야 합니다. 다음 코드는 **AppConstants.cs** 파일을 보여 줍니다.

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

`AppConstants`에서 다음 값을 사용자 지정 하 여 샘플 응용 프로그램을 Azure Notification Hub에 연결 합니다.

* `NotificationHubName`: Azure Portal에서 만든 Azure 알림 허브의 이름을 사용 합니다.
* `ListenConnectionString`:이 값은 Azure Notification Hub의 **액세스 정책**에서 찾을 수 있습니다.

다음 스크린샷은 Azure Portal에서 이러한 값이 있는 위치를 보여 줍니다.

![Azure Notification Hub 액세스 정책의 스크린샷](azure-notification-hub-images/notification-hub-access-policy.png "Azure 알림 허브 액세스 정책")

## <a name="configure-the-android-application-for-notifications"></a>알림에 대 한 Android 응용 프로그램 구성

알림을 받고 처리 하도록 Android 응용 프로그램을 구성 하려면 다음 단계를 완료 합니다.

1. Firebase 콘솔에서 패키지 이름과 일치 하도록 Android **패키지 이름을** 구성 합니다.
1. Google Play, Firebase 및 Azure Notification Hubs와 상호 작용 하려면 다음 NuGet 패키지를 설치 합니다.
    1. Xamarin.GooglePlayServices.Base.
    1. Xamarin.Firebase.Messaging.
    1. Xamarin.Azure.NotificationHubs.Android.
1. FCM 설치 중에 다운로드 한 `google-services.json` 파일을 프로젝트에 복사 하 고 빌드 작업을 `GoogleServicesJson`로 설정 합니다.
1. [Firebase와 통신 하도록 AndroidManifest을 구성](#configure-android-manifest)합니다.
1. [FirebaseMessagingService를 재정의 하 여 메시지를 처리](#override-firebasemessagingservice-to-handle-messages)합니다.
1. [들어오는 알림을 XAMARIN.IOS UI에 추가](#add-incoming-notifications-to-the-xamarinforms-ui)합니다.

> [!NOTE]
> **GoogleServicesJson** 빌드 작업은 **xamarin.googleplayservices.base** NuGet 패키지의 일부입니다. Visual Studio 2019는 시작 하는 동안 사용 가능한 빌드 작업을 설정 합니다. 빌드 작업으로 **GoogleServicesJson** 가 표시 되지 않으면 NuGet 패키지를 설치한 후 Visual Studio 2019을 다시 시작 합니다.

### <a name="configure-android-manifest"></a>Android 매니페스트 구성

`application` 요소 내의 `receiver` 요소를 사용 하면 앱이 Firebase와 통신할 수 있습니다. 앱은 `uses-permission` 요소를 사용 하 여 메시지를 처리 하 고 Azure Notification Hub에 등록할 수 있습니다. 전체 **Androidmanifest** 은 아래 예제와 같이 표시 됩니다.

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

### <a name="override-firebasemessagingservice-to-handle-messages"></a>FirebaseMessagingService를 재정의 하 여 메시지 처리

Firebase에 등록 하 고 메시지를 처리 하려면 `FirebaseMessagingService` 클래스의 하위 클래스를 사용 합니다. 샘플 응용 프로그램은 `FirebaseMessagingService`을 서브 클래스 하는 `FirebaseService` 클래스를 정의 합니다. 이 클래스는 `com.google.firebase.MESSAGING_EVENT` 필터를 포함 하는 `IntentFilter` 특성으로 태그가 지정 됩니다. 이 필터를 통해 Android는 처리를 위해 들어오는 메시지를이 클래스에 전달할 수 있습니다.

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseService : FirebaseMessagingService
{
    // ...
}

```

응용 프로그램이 시작 되 면 Firebase SDK가 Firebase 서버에서 고유한 토큰 식별자를 자동으로 요청 합니다. 요청이 성공적으로 완료 되 면 `FirebaseService` 클래스에서 `OnNewToken` 메서드가 호출 됩니다. 샘플 프로젝트는이 메서드를 재정의 하 고 Azure Notification Hubs에 토큰을 등록 합니다.

```csharp
public override void OnNewToken(string token)
{
    // NOTE: save token instance locally, or log if desired

    SendRegistrationToServer(token);
}

void SendRegistrationToServer(string token)
{
    try
    {
        NotificationHub hub = new NotificationHub(AppConstants.NotificationHubName, AppConstants.ListenConnectionString, this);

        // register device with Azure Notification Hub using the token from FCM
        Registration registration = hub.Register(token, AppConstants.SubscriptionTags);

        // subscribe to the SubscriptionTags list with a simple template.
        string pnsHandle = registration.PNSHandle;
        TemplateRegistration templateReg = hub.RegisterTemplate(pnsHandle, "defaultTemplate", AppConstants.FCMTemplateBody, AppConstants.SubscriptionTags);
    }
    catch (Exception e)
    {
        Log.Error(AppConstants.DebugTag, $"Error registering device: {e.Message}");
    }
}
```

`SendRegistrationToServer` 메서드는 Azure 알림 허브에 장치를 등록 하 고 템플릿을 사용 하 여 태그를 구독 합니다. 샘플 응용 프로그램은 `default` 이라는 단일 태그와 **AppConstants.cs** 파일에서 `messageParam` 라는 단일 매개 변수를 사용 하 여 템플릿을 정의 합니다. 등록, 태그 및 템플릿에 대 한 자세한 내용은 [Azure 알림 허브를 사용 하 여 템플릿 및 태그 등록](#register-templates-and-tags-with-the-azure-notification-hub)을 참조 하세요.

메시지가 수신 되 면 `FirebaseService` 클래스에서 `OnMessageReceived` 메서드가 호출 됩니다.

```csharp
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
    
    //Unique request code to avoid PendingIntent collision.
    var requestCode = new Random().Next();
    var pendingIntent = PendingIntent.GetActivity(this, requestCode, intent, PendingIntentFlags.OneShot);

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
```

들어오는 메시지는 `SendLocalNotification` 메서드를 사용 하 여 로컬 알림으로 변환 됩니다. 이 메서드는 새 `Intent` 만들고 메시지 콘텐츠를 `string` `Extra`로 `Intent`에 배치 합니다. 사용자가 로컬 알림을 탭 하면 앱이 포그라운드 또는 백그라운드에 있는지에 관계 없이 `MainActivity` 시작 되 고 `Intent` 개체를 통해 메시지 내용에 액세스할 수 있습니다.

로컬 알림 및 `Intent` 예제에서는 사용자가 알림 누르기 작업을 수행 해야 합니다. 이는 사용자가 응용 프로그램 상태가 변경 되기 전에 조치를 취해야 하는 경우에 적합 합니다. 그러나 경우에 따라 사용자 작업을 요구 하지 않고 메시지 데이터에 액세스할 수 있습니다. 이전 예제에서는 `SendMessageToMainPage` 메서드를 사용 하 여 메시지를 현재 `MainPage` 인스턴스로 직접 보냅니다. 프로덕션에서 단일 메시지 유형에 대해 두 메서드를 모두 구현 하는 경우 사용자가 알림을 탭 하면 `MainPage` 개체가 중복 메시지를 받게 됩니다.

> [!NOTE]
> Android 응용 프로그램은 백그라운드 또는 포그라운드로 실행 되는 경우에만 푸시 알림을 받게 됩니다. 주 `Activity` 실행 되 고 있지 않을 때 푸시 알림을 받으려면이 샘플의 범위를 벗어난 서비스를 구현 해야 합니다. 자세한 내용은 [Android 서비스 만들기](/xamarin/android/app-fundamentals/services/) 를 참조 하세요.

### <a name="add-incoming-notifications-to-the-xamarinforms-ui"></a>Xamarin.ios UI에 들어오는 알림 추가

`MainActivity` 클래스는 알림을 처리 하 고 들어오는 메시지 데이터를 관리할 수 있는 권한을 얻어야 합니다. 다음 코드에서는 전체 `MainActivity` 구현을 보여 줍니다.

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

        if (!IsPlayServiceAvailable())
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

`Activity` 특성은 응용 프로그램 `LaunchMode` `SingleTop`설정 합니다. 이 시작 모드는 Android OS에이 작업의 단일 인스턴스만 허용 하도록 지시 합니다. 이 시작 모드에서 들어오는 `Intent` 데이터는 `OnNewIntent` 메서드로 라우팅됩니다 .이 메서드는 메시지 데이터를 추출 하 여 `AddMessage` 메서드를 통해 `MainPage` 인스턴스로 보냅니다. 응용 프로그램에서 다른 시작 모드를 사용 하는 경우 `Intent` 데이터를 다르게 처리 해야 합니다.

`OnCreate` 메서드는 `IsPlayServiceAvailable` 라는 도우미 메서드를 사용 하 여 장치에서 Google Play 서비스를 지원 하는지 확인 합니다. Google Play 서비스를 지원 하지 않는 에뮬레이터 또는 장치는 Firebase에서 푸시 알림을 받을 수 없습니다.

## <a name="configure-ios-for-notifications"></a>알림에 대 한 iOS 구성

알림을 받도록 iOS 응용 프로그램을 구성 하는 프로세스는 다음과 같습니다.

1. 프로 비전 프로필에 사용 된 값과 일치 하도록 **info.plist** 파일에서 **번들 식별자** 를 구성 합니다.
1. **Info.plist** 파일에 **푸시 알림 사용** 옵션을 추가 합니다.
1. 프로젝트에 **Xamarin.ios** NuGet 패키지를 추가 합니다.
1. [APNS를 사용 하 여 알림을 등록](#register-for-notifications-with-apns)합니다.
1. [Azure 알림 허브에 응용 프로그램을 등록 하 고 태그를 구독](#register-with-azure-notification-hub-and-subscribe-to-tags)합니다.
1. [Xamarin. FORMS UI에 APNS 알림을 추가](#add-apns-notifications-to-xamarinforms-ui)합니다.

다음 스크린샷은 Visual Studio 내의 **info.plist** 파일에서 선택 된 **푸시 알림 사용** 옵션을 보여 줍니다.

![푸시 알림 권한 스크린샷](azure-notification-hub-images/push-notification-entitlement.png "푸시 알림 권한")

### <a name="register-for-notifications-with-apns"></a>APNS를 사용 하 여 알림 등록

원격 알림을 등록 하려면 **AppDelegate.cs** 파일의 `FinishedLaunching` 메서드를 재정의 해야 합니다. 등록은 장치에서 사용 되는 iOS 버전에 따라 다릅니다. 샘플 응용 프로그램의 iOS 프로젝트는 다음 예제와 같이 `FinishedLaunching` 메서드를 재정의 하 여 `RegisterForRemoteNotifications`를 호출 합니다.

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

### <a name="register-with-azure-notification-hub-and-subscribe-to-tags"></a>Azure 알림 허브에 등록 하 고 태그를 구독 합니다.

`FinishedLaunching` 메서드 중에 장치가 원격 알림에 성공적으로 등록 되 면 iOS는 `RegisteredForRemoteNotifications` 메서드를 호출 합니다. 다음 작업을 수행 하려면이 메서드를 재정의 해야 합니다.

1. `SBNotificationHub`를 인스턴스화합니다.
1. 기존 등록의 등록을 취소 합니다.
1. 알림 허브에 장치를 등록 합니다.
1. 템플릿으로 특정 태그를 구독 합니다.

장치, 템플릿 및 태그를 등록 하는 방법에 대 한 자세한 내용은 [Azure 알림 허브에 템플릿 및 태그 등록](#register-templates-and-tags-with-the-azure-notification-hub)을 참조 하세요. 다음 코드는 장치 및 템플릿의 등록을 보여 줍니다.

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    Hub = new SBNotificationHub(AppConstants.ListenConnectionString, AppConstants.NotificationHubName);

    // update registration with Azure Notification Hub
    Hub.UnregisterAll(deviceToken, (error) =>
    {
        if (error != null)
        {
            Debug.WriteLine($"Unable to call unregister {error}");
            return;
        }

        var tags = new NSSet(AppConstants.SubscriptionTags.ToArray());
        Hub.RegisterNative(deviceToken, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                Debug.WriteLine($"RegisterNativeAsync error: {errorCallback}");
            }
        });

        var templateExpiration = DateTime.Now.AddDays(120).ToString(System.Globalization.CultureInfo.CreateSpecificCulture("en-US"));
        Hub.RegisterTemplate(deviceToken, "defaultTemplate", AppConstants.APNTemplateBody, templateExpiration, tags, (errorCallback) =>
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
> 네트워크에 연결 되지 않은 경우 원격 알림에 대 한 등록이 실패할 수 있습니다. `FailedToRegisterForRemoveNotifications` 메서드를 재정의 하 여 등록 오류를 처리 하도록 선택할 수 있습니다.

### <a name="add-apns-notifications-to-xamarinforms-ui"></a>Xamarin.ios UI에 APNS 알림 추가

장치가 원격 알림을 수신 하면 iOS는 `ReceivedRemoteNotification` 메서드를 호출 합니다. 들어오는 메시지 JSON은 `NSDictionary` 개체로 변환 되 고 `ProcessNotification` 메서드는 사전에서 값을 추출 하 여 Xamarin.ios `MainPage` 인스턴스로 보냅니다. `ReceivedRemoteNotifications` 메서드는 다음 코드와 같이 `ProcessNotification`를 호출 하도록 재정의 됩니다.

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

## <a name="test-notifications-in-the-azure-portal"></a>Azure Portal의 테스트 알림

Azure Notification Hubs를 사용 하 여 응용 프로그램에서 테스트 메시지를 받을 수 있는지 확인할 수 있습니다. 알림 허브의 **테스트 보내기** 섹션을 사용 하 여 대상 플랫폼을 선택 하 고 메시지를 보낼 수 있습니다. **Send To Tag 식을** `default`로 설정 하면 `default` 태그에 대해 템플릿을 등록 한 응용 프로그램으로 메시지를 보냅니다. **보내기** 단추를 클릭 하면 메시지와 연결 된 장치 수가 포함 된 보고서가 생성 됩니다. 다음 스크린샷은 Azure Portal의 Android 알림 테스트를 보여 줍니다.

![Azure 알림 허브 테스트 메시지의 스크린샷](azure-notification-hub-images/azure-notification-hub-test-send.png "Azure 알림 허브 테스트 메시지")

### <a name="testing-tips"></a>테스트 팁

1. 응용 프로그램에서 푸시 알림을 받을 수 있는지 테스트 하는 경우 물리적 장치를 사용 해야 합니다. Android 및 iOS 가상 장치는 푸시 알림을 받기 위해 올바르게 구성 되지 않을 수 있습니다.
1. 샘플 Android 응용 프로그램은 Firebase 토큰이 발급 될 때 토큰 및 템플릿을 한 번 등록 합니다. 테스트 하는 동안 새 토큰을 요청 하 고 Azure 알림 허브에 다시 등록 해야 할 수 있습니다. 이렇게 하는 가장 좋은 방법은 프로젝트를 정리 하 고, `bin` `obj` 및 폴더를 삭제 하 고, 다시 빌드 및 배포 하기 전에 장치에서 응용 프로그램을 제거 하는 것입니다.
1. 푸시 알림 흐름의 많은 부분이 비동기적으로 실행 됩니다. 이로 인해 중단점이 적중 되지 않거나 예기치 않은 순서로 적중 될 수 있습니다. 응용 프로그램 흐름을 중단 하지 않고 추적 실행에 장치 또는 디버그 로깅을 사용 합니다. `Constants`에 지정 된 `DebugTag`를 사용 하 여 Android 장치 로그를 필터링 합니다.

## <a name="create-a-notification-dispatcher"></a>알림 디스패처 만들기

Azure Notification Hubs 백 엔드 응용 프로그램을 사용 하 여 여러 플랫폼에서 장치에 알림을 디스패치할 수 있습니다. 이 샘플에서는 **Notificationdispatcher** 콘솔 응용 프로그램을 사용 하 여 알림 디스패치를 보여 줍니다. 응용 프로그램에는 다음 속성을 정의 하는 **DispatcherConstants.cs** 파일이 포함 되어 있습니다.

```csharp
public static class DispatcherConstants
{
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string FullAccessConnectionString { get; set; } = "< Insert your DefaultFullSharedAccessSignature >";
}
```

Azure 알림 허브 구성과 일치 하도록 **DispatcherConstants.cs** 을 구성 해야 합니다. `SubscriptionTags` 속성의 값은 클라이언트 앱에서 사용 되는 값과 일치 해야 합니다. `NotificationHubName` 속성은 Azure 알림 허브 인스턴스의 이름입니다. `FullAccessConnectionString` 속성은 알림 허브 **액세스 정책**에 있는 선택 키입니다. 다음 스크린샷은 Azure Portal의 `NotificationHubName` 및 `FullAccessConnectionString` 속성의 위치를 보여 줍니다.

![Azure 알림 허브 이름 및 FullAccessConnectionString의 스크린샷](azure-notification-hub-images/notification-hub-full-access-policy.png "Azure 알림 허브 이름 및 FullAccessConnectionString")

콘솔 응용 프로그램은 각 `SubscriptionTags` 값을 반복 하 고 `NotificationHubClient` 클래스의 인스턴스를 사용 하 여 구독자에 게 알림을 보냅니다. 다음 코드는 콘솔 응용 프로그램 `Program` 클래스를 보여 줍니다.

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

샘플 콘솔 응용 프로그램을 실행할 때 스페이스바를 눌러 메시지를 보낼 수 있습니다. 클라이언트 응용 프로그램을 실행 하는 장치는 올바르게 구성 된 경우 번호가 매겨진 알림을 받게 됩니다.

## <a name="related-links"></a>관련 링크

* [푸시 알림 템플릿](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).
* [장치 등록 관리](/azure/notification-hubs/notification-hubs-push-notification-registration-management).
* [라우팅 및 태그 식](/azure/notification-hubs/notification-hubs-tags-segment-push-message)
* [Xamarin Android Azure Notification Hubs 자습서](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm).
* [Xamarin.ios Azure Notification Hubs 자습서](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started).
