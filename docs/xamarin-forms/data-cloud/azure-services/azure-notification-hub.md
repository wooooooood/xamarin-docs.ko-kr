---
title: Azure Notification Hubs 및 Xamarin.Forms를 사용하여 푸시 알림 보내기 및 받기
description: 이 문서에서는 Azure Notification Hubs를 사용하여 Xamarin.Forms 애플리케이션에 플랫폼 간 푸시 알림을 보내는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 07D13195-3A0D-4C95-ACF0-143A9084973C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/27/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
- Firebase
ms.openlocfilehash: 5f7b83c1fc907de790b382aabde0c5a957e5a8bb
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84565423"
---
# <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-xamarinforms"></a>Azure Notification Hubs 및 Xamarin.Forms를 사용하여 푸시 알림 보내기 및 받기

[![샘플 다운로드](~/media/shared/download.png)샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurenotificationhub/)

푸시 알림은 백엔드 시스템에서 모바일 애플리케이션으로 정보를 전달합니다. Apple, Google 및 기타 플랫폼에는 각각 자체 푸시 알림 서비스(PNS)가 있습니다. Azure Notification Hubs를 사용하면 여러 플랫폼에서 알림을 중앙 집중화하여 백엔드 애플리케이션에서 각 플랫폼별 PNS에 알림을 배포하는 단일 허브와 통신할 수 있습니다.

다음 단계를 수행하여 모바일 앱에 Azure Notification Hubs를 통합합니다.

1. [푸시 알림 서비스 및 Azure Notification Hub를 설정합니다](#set-up-push-notification-services-and-azure-notification-hub).
1. [템플릿 및 태그를 사용하는 방법을 이해합니다](#register-templates-and-tags-with-the-azure-notification-hub).
1. [여러 플랫폼에서 사용 가능한 Xamarin.Forms 애플리케이션을 만듭니다.](#xamarinforms-application-functionality)
1. [푸시 알림에 대한 네이티브 Android 프로젝트를 구성합니다](#configure-the-android-application-for-notifications).
1. [푸시 알림에 대한 네이티브 iOS 프로젝트를 구성합니다](#configure-ios-for-notifications).
1. [Azure Notification Hub를 사용한 테스트 알림](#test-notifications-in-the-azure-portal).
1. [알림을 보내는 백엔드 애플리케이션을 만듭니다](#create-a-notification-dispatcher).

> [!NOTE]
> [Azure 구독](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://aka.ms/azfree-docs-mobileapps)을 만듭니다.

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>푸시 알림 서비스 및 Azure Notification Hub 설정

Azure Notification Hubs를 Xamarin.Forms 모바일 앱과 통합하는 것은 Azure Notification Hubs를 Xamarin 네이티브 애플리케이션과 통합하는 것과 비슷합니다. [Azure Notification Hubs를 사용하여 Xamarin.Android에 푸시 알림](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm#create-a-firebase-project-and-enable-firebase-cloud-messaging)의 Firebase 콘솔 단계에 따라 FCM(Firebase 클라우드 메시징) 애플리케이션을 설정합니다. Xamarin.Android 자습서를 사용하여 다음 단계를 완료합니다.

1. 예제에서 사용되는 `com.xamarin.notifysample`과 같은 Android 패키지 이름을 정의합니다.
1. Firebase 콘솔에서 `google-services.json`을 다운로드합니다. 이후 단계에서 Android 애플리케이션에 이 파일을 추가할 것입니다.
1. Azure Notification Hub 인스턴스를 만들고 이름을 지정합니다. 이 문서 및 샘플에서는 `xdocsnotificationhub`를 허브 이름으로 사용합니다.
1. FCM **서버 키**를 복사하여 Azure Notification Hub의 **Google(GCM/FCM)** 아래에서 **API 키**로 저장합니다.

다음 스크린샷은 Azure Notification Hub의 Google 플랫폼 구성을 보여 줍니다.

![Azure Notification Hub Google 구성의 스크린샷](azure-notification-hub-images/fcm-notification-hub-config.png "Azure Notification Hub Google 구성")

iOS 디바이스에 대한 설정을 완료하려면 macOS 머신이 필요합니다. [Azure Notification Hubs를 사용하여 Xamarin.iOS에 푸시 알림](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started#generate-the-certificate-signing-request-file)의 시작 단계에 따라 APNS(Apple Push Notification Services)를 설정합니다. Xamarin.iOS 자습서를 사용하여 다음 단계를 완료합니다.

1. iOS 번들 식별자를 정의합니다. 이 문서 및 샘플에서는 `com.xamarin.notifysample`를 번들 식별자로 사용합니다.
1. CSR(인증서 서명 요청) 파일을 만들고 이 파일을 사용하여 푸시 알림 인증서를 생성합니다.
1. Azure Notification Hub의 **Apple(APNS)** 아래에 푸시 알림 인증서를 업로드합니다.

다음 스크린샷은 Azure Notification Hub의 Apple 플랫폼 구성을 보여 줍니다.

![Azure Notification Hub Apple 구성의 스크린샷](azure-notification-hub-images/apns-notification-hub-config.png "Azure Notification Hub Apple 구성")

## <a name="register-templates-and-tags-with-the-azure-notification-hub"></a>Azure Notification Hub에 템플릿 및 태그 등록

Azure Notification Hub를 사용하려면 모바일 애플리케이션을 허브에 등록하고, 템플릿을 정의하고, 태그를 구독해야 합니다. 등록은 플랫폼별 PNS 핸들을 Azure Notification Hub의 식별자에 연결합니다. 등록에 대해 자세히 알아보려면 [등록 관리](/azure/notification-hubs/notification-hubs-push-notification-registration-management)를 참조하세요.

템플릿을 사용하여 디바이스에서 매개 변수화된 메시지 템플릿을 지정할 수 있습니다. 들어오는 메시지는 디바이스별, 태그별로 사용자 지정할 수 있습니다. 템플릿에 대한 자세한 내용은 [템플릿](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)을 참조하세요.

태그를 사용하여 뉴스, 스포츠, 날씨 등의 메시지 범주를 구독할 수 있습니다. 간단히 하기 위해 애플리케이션 예제는 `messageParam`이라는 단일 매개 변수와 `default`라는 단일 태그를 사용하여 기본 템플릿을 정의합니다. 더 복잡한 시스템에서는 사용자별 태그를 사용하여 디바이스에서 사용자에게 맞춤형 알림 메시지를 보낼 수 있습니다. 태그에 대한 자세한 내용은 [라우팅 및 태그 식](/azure/notification-hubs/notification-hubs-tags-segment-push-message)을 참조하세요.

메시지를 성공적으로 수신하려면 각 네이티브 애플리케이션에서 다음 단계를 수행해야 합니다.

1. 플랫폼 PNS에서 PNS 핸들 또는 토큰을 가져옵니다.
1. PNS 핸들을 Azure Notification Hub에 등록합니다.
1. 나가는 메시지와 동일한 매개 변수를 포함하는 템플릿을 지정합니다.
1. 나가는 메시지의 대상으로 지정된 태그를 구독합니다.

해당 단계는 [알림을 위한 Android 애플리케이션 구성](#configure-the-android-application-for-notifications) 및 [알림을 위한 iOS 구성](#configure-ios-for-notifications) 섹션에서 각 플랫폼에 대한 세부 정보에 설명되어 있습니다.

## <a name="xamarinforms-application-functionality"></a>Xamarin.Forms 애플리케이션 기능

Xamarin.Forms 애플리케이션 예제는 푸시 알림 메시지 목록을 표시합니다. 이는 지정된 푸시 알림 메시지를 UI에 추가하는 `AddMessage` 메서드로 구현됩니다. 또한 이 메서드는 중복 메시지가 UI에 추가되는 것을 방지하며, 모든 스레드에서 호출될 수 있도록 주 스레드에서 실행됩니다. 다음 코드에서는 `AddMessage` 메서드를 보여 줍니다.

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

애플리케이션 예제에는 플랫폼 프로젝트에서 사용하는 속성을 정의하는 `AppConstants.cs` 파일이 포함되어 있습니다. 이 파일은 Azure Notification Hub의 값으로 사용자 지정해야 합니다. 다음 코드는 `AppConstants.cs` 파일을 보여 줍니다.

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

`AppConstants`에서 다음 값을 사용자 지정하여 애플리케이션 예제를 Azure Notification Hub에 연결합니다.

* `NotificationHubName`: Azure Portal에서 만든 Azure Notification Hub의 이름을 사용합니다.
* `ListenConnectionString`: 이 값은 Azure Notification Hub의 **액세스 정책** 아래에 있습니다.

다음 스크린샷은 Azure Portal에서 해당 값이 있는 위치를 보여 줍니다.

![Azure Notification Hub 액세스 정책의 스크린샷](azure-notification-hub-images/notification-hub-access-policy.png "Azure Notification Hub 액세스 정책")

## <a name="configure-the-android-application-for-notifications"></a>알림을 위한 Android 애플리케이션 구성

알림을 받고 처리하도록 Android 애플리케이션을 구성하려면 다음 단계를 완료합니다.

1. Firebase 콘솔에서 패키지 이름과 일치하도록 Android **패키지 이름**을 구성합니다.
1. Google Play, Firebase 및 Azure Notification Hubs와 상호 작용하려면 다음 NuGet 패키지를 설치합니다.
    1. `Xamarin.GooglePlayServices.Base`
    1. `Xamarin.Firebase.Messaging`
    1. `Xamarin.Azure.NotificationHubs.Android`
1. FCM 설치 중에 다운로드 한 `google-services.json` 파일을 프로젝트에 복사하고 빌드 작업을 `GoogleServicesJson`으로 설정합니다.
1. Firebase와 통신하도록 `AndroidManifest.xml`을 [구성](#configure-android-manifest)합니다.
1. 메시지를 처리하도록 `FirebaseMessagingService`를 [재정의](#override-firebasemessagingservice-to-handle-messages)합니다.
1. 들어오는 알림을 Xamarin.Forms UI에 [추가](#add-incoming-notifications-to-the-xamarinforms-ui)합니다.

> [!NOTE]
> `GoogleServicesJson` 빌드 작업은 `Xamarin.GooglePlayServices.Base` NuGet 패키지의 일부입니다. Visual Studio 2019는 시작하는 동안 사용 가능한 빌드 작업을 설정합니다. `GoogleServicesJson`이 빌드 작업으로 표시되지 않으면 NuGet 패키지를 설치한 후 Visual Studio 2019를 다시 시작합니다.

### <a name="configure-android-manifest"></a>Android 매니페스트 구성

`application` 요소 내의 `receiver` 요소를 통해 앱이 Firebase와 통신할 수 있습니다. 앱은 `uses-permission` 요소를 사용하여 메시지를 처리하고 Azure Notification Hub에 등록할 수 있습니다. 전체 `AndroidManifest.xml`은 아래 예제와 유사합니다.

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

### <a name="override-firebasemessagingservice-to-handle-messages"></a>메시지를 처리하도록 `FirebaseMessagingService` 재정의

Firebase에 등록하고 메시지를 처리하려면 `FirebaseMessagingService` 클래스를 서브클래싱합니다. 애플리케이션 예제는 `FirebaseMessagingService`를 서브클래싱하는 `FirebaseService` 클래스를 정의합니다. 이 클래스는 `com.google.firebase.MESSAGING_EVENT` 필터를 포함하는 `IntentFilter` 특성으로 태그가 지정됩니다. Android는 이 필터를 사용하여 들어오는 메시지가 처리되도록 이 클래스에 전달할 수 있습니다.

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseService : FirebaseMessagingService
{
    // ...
}

```

애플리케이션이 시작되면 Firebase SDK는 Firebase 서버에서 고유한 토큰 식별자를 자동으로 요청합니다. 요청이 성공적으로 완료되면 `FirebaseService` 클래스에서 `OnNewToken` 메서드가 호출됩니다. 샘플 프로젝트는 이 메서드를 재정의하고 Azure Notification Hubs에 토큰을 등록합니다.

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

`SendRegistrationToServer` 메서드는 Azure Notification Hub에 디바이스를 등록하고 템플릿을 사용하여 태그를 구독합니다. 애플리케이션 예제는 `default`라는 단일 태그와 `AppConstants.cs` 파일의 `messageParam`이라는 단일 매개 변수를 사용하여 템플릿을 정의합니다. 등록, 태그 및 템플릿에 대한 자세한 내용은 [Azure Notification hubs에 템플릿 및 태그 등록](#register-templates-and-tags-with-the-azure-notification-hub)을 참조하세요.

메시지가 수신되면 `FirebaseService` 클래스에서 `OnMessageReceived` 메서드가 호출됩니다.

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

들어오는 메시지는 `SendLocalNotification` 메서드를 사용하여 로컬 알림으로 변환됩니다. 이 메서드는 새 `Intent`를 만들고 메시지 콘텐츠를 `string` `Extra`로 `Intent`에 배치합니다. 사용자가 로컬 알림을 탭하면 앱이 포그라운드에 있든 백그라운드에 있든 관계없이 `MainActivity`가 시작되며 `Intent` 개체를 통해 메시지 내용에 액세스할 수 있습니다.

로컬 알림 및 `Intent` 예제에서는 사용자가 알림 탭하기 동작을 수행해야 합니다. 이는 애플리케이션 상태가 변경되기 전에 사용자가 조치를 취해야 하는 경우에 적합합니다. 그러나 때에 따라 사용자 동작을 요구하지 않고 메시지 데이터에 액세스할 수 있습니다. 이전 예제에서도 `SendMessageToMainPage` 메서드를 사용하여 메시지를 현재 `MainPage` 인스턴스로 직접 보냅니다. 프로덕션에서 단일 메시지 유형에 대해 두 메서드를 모두 구현하는 경우 사용자가 알림을 탭하면 `MainPage` 개체가 중복 메시지를 받게 됩니다.

> [!NOTE]
> Android 애플리케이션은 백그라운드 또는 포그라운드로 실행되는 경우에만 푸시 알림을 받게 됩니다. 기본 `Activity`가 실행되고 있지 않을 때 푸시 알림을 받으려면 이 샘플의 범위를 벗어난 서비스를 구현해야 합니다. 자세한 내용은 [Android 서비스 만들기](/xamarin/android/app-fundamentals/services/)를 참조하세요.

### <a name="add-incoming-notifications-to-the-xamarinforms-ui"></a>Xamarin.Forms UI에 들어오는 알림 추가

`MainActivity` 클래스는 알림을 처리하고 들어오는 메시지 데이터를 관리할 수 있는 권한을 얻어야 합니다. 다음 코드는 `MainActivity` 구현을 보여 줍니다.

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

`Activity` 특성은 `LaunchMode` 애플리케이션을 `SingleTop`으로 설정합니다. 이 시작 모드는 Android OS에 이 작업의 단일 인스턴스만 허용하도록 지시합니다. 이 시작 모드에서 들어오는 `Intent` 데이터는 `OnNewIntent` 메서드로 라우팅됩니다. 이 메서드는 메시지 데이터를 추출하여 `AddMessage` 메서드를 통해 `MainPage` 인스턴스로 보냅니다. 애플리케이션에서 다른 시작 모드를 사용하는 경우에는 `Intent` 데이터를 다르게 처리해야 합니다.

`OnCreate` 메서드는 `IsPlayServiceAvailable`이라는 도우미 메서드를 사용하여 디바이스에서 Google Play 서비스를 지원하는지 확인합니다. Google Play 서비스를 지원하지 않는 에뮬레이터 또는 디바이스는 Firebase에서 푸시 알림을 받을 수 없습니다.

## <a name="configure-ios-for-notifications"></a>알림을 위한 iOS 구성

알림을 받도록 iOS 애플리케이션을 구성하는 프로세스는 다음과 같습니다.

1. 프로비저닝 프로필에 사용되는 값과 일치하도록 `Info.plist` 파일의 **번들 식별자**를 구성합니다.
1. **푸시 알림 사용** 옵션을 `Entitlements.plist` 파일에 추가합니다.
1. `Xamarin.Azure.NotificationHubs.iOS` NuGet 패키지를 프로젝트에 추가합니다.
1. APNS를 사용하여 알림을 [등록](#register-for-notifications-with-apns)합니다.
1. Azure Notification Hub에 애플리케이션을 [등록](#register-with-azure-notification-hub-and-subscribe-to-tags)하고 태그를 구독합니다.
1. APNS 알림을 Xamarin.Forms UI에 [추가](#add-apns-notifications-to-xamarinforms-ui)합니다.

다음 스크린샷은 Visual Studio 내의 `Entitlements.plist` 파일에서 선택된 **푸시 알림 사용** 옵션을 보여 줍니다.

![푸시 알림 자격의 스크린샷](azure-notification-hub-images/push-notification-entitlement.png "푸시 알림 자격")

### <a name="register-for-notifications-with-apns"></a>APNS를 사용하여 알림 등록

원격 알림을 등록하려면 `AppDelegate.cs` 파일의 `FinishedLaunching` 메서드를 재정의해야 합니다. 등록은 디바이스에서 사용되는 iOS 버전에 따라 다릅니다. 애플리케이션 예제의 iOS 프로젝트는 다음 예제와 같이 `RegisterForRemoteNotifications`를 호출하도록 `FinishedLaunching` 메서드를 재정의합니다.

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

### <a name="register-with-azure-notification-hub-and-subscribe-to-tags"></a>Azure Notification Hub에 등록하고 태그 구독

`FinishedLaunching` 메서드 중에 디바이스가 원격 알림에 성공적으로 등록되면 iOS는 `RegisteredForRemoteNotifications` 메서드를 호출합니다. 다음 작업을 수행하려면 이 메서드를 재정의해야 합니다.

1. `SBNotificationHub`를 인스턴스화합니다.
1. 기존 등록을 취소합니다.
1. 알림 허브에 디바이스를 등록합니다.
1. 템플릿으로 특정 태그를 구독합니다.

디바이스, 템플릿 및 태그의 등록에 대한 자세한 내용은 [Azure Notification hubs에 템플릿 및 태그 등록](#register-templates-and-tags-with-the-azure-notification-hub)을 참조하세요. 다음 코드는 디바이스 및 템플릿의 등록을 보여 줍니다.

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
> 네트워크에 연결되지 않은 경우 원격 알림에 대한 등록이 실패할 수 있습니다. 등록 오류를 처리하도록 `FailedToRegisterForRemoveNotifications` 메서드를 재정의할 수 있습니다.

### <a name="add-apns-notifications-to-xamarinforms-ui"></a>Xamarin.Forms UI에 APNS 알림 추가

디바이스가 원격 알림을 수신하면 iOS는 `ReceivedRemoteNotification` 메서드를 호출합니다. 들어오는 메시지 JSON은 `NSDictionary` 개체로 변환되고, `ProcessNotification` 메서드는 사전에서 값을 추출하여 Xamarin.Forms `MainPage` 인스턴스로 보냅니다. `ReceivedRemoteNotifications` 메서드는 다음 코드와 같이 `ProcessNotification`을 호출하도록 재정의됩니다.

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

Azure Notification Hubs를 사용하여 애플리케이션에서 테스트 메시지를 받을 수 있는지 확인할 수 있습니다. 알림 허브의 **테스트 보내기** 섹션을 사용하여 대상 플랫폼을 선택하고 메시지를 보낼 수 있습니다. **태그 식으로 보내기**를 `default`로 설정하면 `default` 태그에 대해 템플릿을 등록한 애플리케이션으로 메시지가 전송됩니다. **보내기** 단추를 클릭하면 메시지와 함께 연결된 디바이스의 수를 포함하는 보고서가 생성됩니다. 다음 스크린샷은 Azure Portal의 Android 알림 테스트를 보여 줍니다.

![Azure Notification Hub 테스트 메시지의 스크린샷](azure-notification-hub-images/azure-notification-hub-test-send.png "Azure Notification Hub 테스트 메시지")

### <a name="testing-tips"></a>테스트 팁

1. 애플리케이션에서 푸시 알림을 받을 수 있는지 테스트하는 경우 물리적 디바이스를 사용해야 합니다. Android 및 iOS 가상 디바이스가 푸시 알림을 받도록 올바르게 구성되지 않았을 수 있습니다.
1. Android 애플리케이션 예제는 Firebase 토큰이 발급될 때 토큰 및 템플릿을 한 번 등록합니다. 테스트하는 동안 새 토큰을 요청하고 Azure Notification Hub에 다시 등록해야 할 수 있습니다. 이렇게 하기 위한 최선의 방법은 프로젝트를 정리하고, `bin` 및 `obj` 폴더를 삭제하고, 다시 빌드 및 배포하기 전에 디바이스에서 애플리케이션을 제거하는 것입니다.
1. 푸시 알림 흐름의 많은 부분이 비동기적으로 실행됩니다. 이로 인해 중단점이 적중되지 않거나 예기치 않은 순서로 적중될 수 있습니다. 애플리케이션 흐름을 중단하지 않고 디바이스 또는 디버그 로깅을 사용하여 실행을 추적합니다. `Constants`에 지정된 `DebugTag`를 사용하여 Android 디바이스 로그를 필터링합니다.
1. Visual Studio에서 디버깅이 중지되면 앱이 강제로 닫힙니다. 디버깅 프로세스의 일부로 시작된 모든 메시지 수신기 또는 기타 서비스가 닫히고 메시지 이벤트에 응답하지 않게 됩니다.

## <a name="create-a-notification-dispatcher"></a>알림 디스패처 만들기

Azure Notification Hubs에서 백엔드 애플리케이션을 사용하여 여러 플랫폼에서 디바이스에 알림을 디스패치할 수 있습니다. 이 샘플은 콘솔 애플리케이션을 사용한 알림 디스패치를 보여 줍니다. 애플리케이션은 다음 속성을 정의하는 `DispatcherConstants.cs` 파일을 포함합니다.

```csharp
public static class DispatcherConstants
{
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string FullAccessConnectionString { get; set; } = "< Insert your DefaultFullSharedAccessSignature >";
}
```

Azure Notification Hub 구성과 일치하도록 `DispatcherConstants.cs` 파일을 구성해야 합니다. `SubscriptionTags` 속성의 값은 클라이언트 앱에서 사용되는 값과 일치해야 합니다. `NotificationHubName` 속성은 Azure Notification Hub 인스턴스의 이름입니다. `FullAccessConnectionString` 속성은 알림 허브 **액세스 정책**에 있는 선택키입니다. 다음 스크린샷은 Azure Portal의 `NotificationHubName` 및 `FullAccessConnectionString` 속성의 위치를 보여 줍니다.

![Azure Notification Hub 이름 및 FullAccessConnectionString의 스크린샷](azure-notification-hub-images/notification-hub-full-access-policy.png "Azure Notification Hub 이름 및 FullAccessConnectionString")

콘솔 애플리케이션은 각 `SubscriptionTags` 값을 반복하고 `NotificationHubClient` 클래스의 인스턴스를 사용하여 구독자에게 알림을 보냅니다. 다음 코드는 콘솔 애플리케이션 `Program` 클래스를 보여 줍니다.

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

콘솔 애플리케이션 예제가 실행될 때 스페이스바를 눌러 메시지를 보낼 수 있습니다. 클라이언트 애플리케이션을 실행하는 디바이스는 올바르게 구성된 경우 번호가 매겨진 알림을 받게 됩니다.

## <a name="related-links"></a>관련 링크

* [푸시 알림 템플릿](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)입니다.
* [디바이스 등록 관리](/azure/notification-hubs/notification-hubs-push-notification-registration-management).
* [라우팅 및 태그 식](/azure/notification-hubs/notification-hubs-tags-segment-push-message).
* [Xamarin.Android Azure Notification Hubs 자습서](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm).
* [Xamarin.iOS Azure Notification Hubs 자습서](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started).
