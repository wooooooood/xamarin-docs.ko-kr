---
title: Azure 모바일 앱에서 푸시 알림 보내기
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에 Azure Mobile Apps에서 푸시 알림을 보내는 Azure Notification Hubs를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: A1EF400F-73F4-43E9-A0C3-1569A0F34A3B
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 2cfb15222c33309101366273d5bc9c42db68b436
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978116"
---
# <a name="sending-push-notifications-from-azure-mobile-apps"></a>Azure 모바일 앱에서 푸시 알림 보내기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)

_Azure Notification Hubs 다양 한 플랫폼 알림 시스템을 사용 하 여 통신할 수 있는 백 엔드의 복잡성을 제거 하는 동안 모든 모바일 플랫폼에 모든 백 엔드에서 모바일 푸시 알림을 보내기 위한 확장 가능한 푸시 인프라를 제공 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에 Azure Mobile Apps에서 푸시 알림을 보내는 Azure Notification Hubs를 사용 하는 방법에 설명 합니다._

> [!VIDEO https://youtube.com/embed/le2lDY22xwM]

**Azure 알림 허브 푸시 및 Xamarin.Forms 비디오**

푸시 알림은 응용 프로그램 계약 및 사용량을 높이기 위해 백 엔드 시스템에서 모바일 장치의 응용 프로그램으로 메시지와 같은 정보를 전송하는 데 사용됩니다. 알림은 사용자가 알림을 사용하는 대상 응용 프로그램을 활성화 하지 않은 경우에도 언제든지 보낼 수 있습니다.

다음 다이어그램에 나와 있는 것 처럼 모바일 장치를 통해 플랫폼 알림 시스템 (PNS), 푸시 알림을 보내는 백 엔드 시스템:

[![](azure-images/pns.png "플랫폼 알림 시스템")](azure-images/pns-large.png#lightbox "플랫폼 알림 시스템")

푸시 알림을 보내도록 백 엔드 시스템 클라이언트 응용 프로그램 인스턴스에 알림을 보내려면 플랫폼별 PNS에 연결 합니다. 이 상당히 복잡해 집니다 백 엔드의 플랫폼 간 푸시 알림, 필요한 경우 백 엔드는 각 플랫폼별 PNS API 및 프로토콜에 사용 해야 하므로.

Azure Notification Hubs이 복잡성을 줄이는이 다른 플랫폼 알림 시스템의 세부 정보를 추상화 하 여 다음 다이어그램에 나와 있는 것 처럼 단일 API 호출을 통해 보낼 플랫폼 간 알림을 허용 합니다.

[![](azure-images/notification-hub.png)](azure-images/notification-hub-large.png#lightbox)

푸시 알림을 보내도록 백 엔드 시스템만 연락처 Azure 알림 허브는 다양 한 플랫폼 알림 시스템을 사용 하 여 다시 통신, 백 엔드의 복잡성 감소는 보냅니다 푸시 알림 코드입니다.

Azure Mobile Apps는 notification hubs를 사용 하 여 푸시 알림에 대 한 기본 제공 지원 합니다. Azure Mobile Apps 인스턴스 Xamarin.Forms 응용 프로그램에서 푸시 알림을 보내기 위한 프로세스는 다음과 같습니다.

1. PNS 핸들을 반환 하는 Xamarin.Forms 응용 프로그램에 등록 합니다.
1. Azure Mobile Apps 인스턴스 알림을 보냅니다 해당 Azure 알림 허브에 장치를 대상으로 지정할 핸들을 지정 하 합니다.
1. Azure 알림 허브에 장치에 대 한 적절 한 PNS로 알림을 보냅니다.
1. PNS는 지정 된 장치는 알림을 보냅니다.
1. Xamarin.Forms 응용 프로그램 알림을 처리 하 고 표시 합니다.

샘플 응용 프로그램 데이터가 Azure Mobile Apps 인스턴스에 저장 된 할 일 목록 응용 프로그램을 보여 줍니다. 새 항목은 Azure Mobile Apps 인스턴스를에 추가 될 때마다 Xamarin.Forms 응용 프로그램에는 푸시 알림이 전송 됩니다. 다음 스크린샷은 받은 푸시 알림을 표시 하는 각 플랫폼을 보여 줍니다.

[![](azure-images/screenshots.png "샘플 응용 프로그램 푸시 알림의 받을")](azure-images/screenshots-large.png#lightbox "푸시 알림을 받는 응용 프로그램 샘플")

Azure Notification Hubs에 대 한 자세한 내용은 참조 하세요. [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/) 하 고 [Xamarin.Forms 앱에 푸시 알림 추가](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push/)합니다.

## <a name="azure-and-platform-notification-system-setup"></a>Azure 및 플랫폼 알림 시스템 설정

Azure Mobile Apps 인스턴스는 Azure 알림 허브를 통합 하는 프로세스는 다음과 같습니다.

1. Azure Mobile Apps 인스턴스를 만듭니다. 자세한 내용은 [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)합니다.
1. 알림 허브를 구성 합니다. 자세한 내용은 [알림 허브 구성](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-hub)합니다.
1. 푸시 알림을 보내도록 Azure Mobile Apps 인스턴스를 업데이트 합니다. 자세한 내용은 [푸시 알림을 전송 하도록 서버 프로젝트 업데이트](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#update-the-server-project-to-send-push-notifications)합니다.
1. 각 PNS를 등록 합니다.
1. 각 PNS와 통신 하는 알림 허브를 구성 합니다.

다음 섹션에서는 각 플랫폼에 대 한 추가 설치 지침을 제공 합니다.

### <a name="ios"></a>iOS

다음 단계를 추가로 푸시 알림 서비스 (APNS (Apple)는 Azure 알림 허브에서 사용 하려면 수행 해야 합니다.

1. Keychain Access 도구를 사용 하 여 푸시 인증서에 대 한 요청을 서명 인증서를 생성 합니다. 자세한 내용은 [푸시 인증서에 대 한 인증서 서명 요청 파일 생성](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#generate-the-certificate-signing-request-file-for-the-push-certificate) Azure 설명서 센터에 있습니다.
1. Apple Developer Center에서 푸시 알림 지원을 위해 Xamarin.Forms 응용 프로그램을 등록 합니다. 자세한 내용은 [푸시 알림을 위해 앱 등록](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-app-for-push-notifications) Azure 설명서 센터에 있습니다.
1. Apple Developer Center에서 푸시 알림을 사용 하도록 설정된 용 프로비저닝 프로필을 Xamarin.Forms 응용 프로그램을 만듭니다. 자세한 내용은 참조 하세요. [앱에 대 한 프로 비전 프로필을 만드는](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#create-a-provisioning-profile-for-the-app) Azure 설명서 센터에 있습니다.
1. APNS와 통신 하는 알림 허브를 구성 합니다. 자세한 내용은 [APNS에 대 한 알림 허브 구성](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-apns)합니다.
1. 새 앱 ID 및 프로 비전 프로필을 사용 하 여 Xamarin.Forms 응용 프로그램을 구성 합니다. 자세한 내용은 [Xamarin Studio에서 iOS 프로젝트를 구성](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-xamarin-studio) 또는 [Visual Studio에서 iOS 프로젝트 구성](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-visual-studio) Azure 설명서 센터에 있습니다.

### <a name="android"></a>Android

다음 단계를 추가로 FCM Firebase Cloud Messaging ()는 Azure 알림 허브에서 사용 하려면 수행 해야 합니다.

1. FCM 등록 합니다. 서버 API 키를 클라이언트 ID와 함께 자동으로 생성 되 고에서 압축을 `google-services.json` 다운로드 되는 파일입니다. 자세한 내용은 [사용 FCM Firebase Cloud Messaging ()](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#enable-firebase-cloud-messaging-fcm)합니다.
1. FCM을 사용 하 여 통신 하도록 알림 허브를 구성 합니다. 자세한 내용은 [Mobile Apps를 백 엔드 FCM을 사용 하 여 푸시 요청을 전송에 구성](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm)합니다.

### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

다음 단계를 추가로 Azure 알림 허브에서 Windows 알림 서비스 (WNS)를 사용 하려면 수행 해야 합니다.

1. Windows 알림 서비스 (WNS)를 등록 합니다. 자세한 내용은 [WNS를 사용 하 여 푸시 알림에 Windows 앱을 등록](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-windows-app-for-push-notifications-with-wns) Azure 설명서 센터에 있습니다.
1. WNS와 통신 하는 알림 허브를 구성 합니다. 자세한 내용은 [wns 알림 허브 구성](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-wns) Azure 설명서 센터에 있습니다.

## <a name="adding-push-notification-support-to-the-xamarinforms-application"></a>Xamarin.Forms 응용 프로그램에 푸시 알림 지원을 추가

다음 섹션에서는 각 플랫폼별 프로젝트에 푸시 알림을 지원 하기 위해 필요한 구현을 설명 합니다.

### <a name="ios"></a>iOS

IOS 응용 프로그램에서 푸시 알림 지원을 구현 하기 위한 프로세스는 다음과 같습니다.

1. 사용 하 여 푸시 알림 서비스 (APNS (Apple)에 등록 된 `AppDelegate.FinishedLaunching` 메서드. 자세한 내용은 [Apple 푸시 알림 시스템을 사용 하 여 등록](#ios_register)합니다.
1. 구현 된 `AppDelegate.RegisteredForRemoteNotifications` 등록 응답을 처리 하는 방법입니다. 자세한 내용은 [등록 응답 처리](#ios_registration_response)합니다.
1. 구현 된 `AppDelegate.DidReceiveRemoteNotification` 들어오는 푸시 알림을 처리 하는 방법입니다. 자세한 내용은 [들어오는 푸시 알림을 처리](#ios_process_incoming)합니다.

<a name="ios_register" />

### <a name="registering-with-the-apple-push-notification-service"></a>Apple Push Notification Service를 사용 하 여 등록

IOS 응용 프로그램에 푸시 알림을 받을 수를 사용 하 여 푸시 알림 서비스 (APNS (Apple), 고유한 장치 토큰을 생성 하 고 응용 프로그램에 반환할 해당 등록 해야 합니다. 등록에서 호출 되는 `FinishedLaunching` 의 재정의 `AppDelegate` 클래스:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    ...
    var settings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert, new NSSet());

    UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications();
    ...
}
```

APNS를 사용 하 여 iOS 응용 프로그램을 등록 하는 경우 푸시 알림 수신 하려는의 형식을 지정 해야 합니다. `RegisterUserNotificationSettings` 메서드를 등록 응용 프로그램을 받을 수 있습니다, 알림의 형식에 사용 하 여는 `RegisterForRemoteNotifications` APNS에서 푸시 알림을 받도록 등록 하는 메서드.

> [!NOTE]
> 호출 하지 못하면는 `RegisterUserNotificationSettings` 메서드 응용 프로그램에서 자동으로 수신 되는 푸시 알림을 생성 합니다.

<a name="ios_registration_response" />

### <a name="handling-the-registration-response"></a>등록 응답 처리

APNS 등록 요청을 백그라운드에서 발생합니다. IOS를 호출 하는 응답을 받으면 합니다 `RegisteredForRemoteNotifications` 의 재정의 `AppDelegate` 클래스:

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyAPNS}
    };

    // Register for push with the Azure mobile app
    Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
    push.RegisterAsync(deviceToken, templates);
}
```

이 메서드를 JSON으로 간단한 알림 메시지 템플릿을 만들고 알림 허브에서 템플릿 알림을 수신 하도록 장치를 등록 합니다.

> [!NOTE]
> `FailedToRegisterForRemoteNotifications` 재정의 네트워크 연결이 없는 상황을 처리 하기 위해 구현 해야 합니다. 사용자가 하는 동안 응용 프로그램을 시작할 수 있으므로이 중요 한 오프 라인입니다.

<a name="ios_process_incoming" />

### <a name="processing-incoming-push-notifications"></a>들어오는 푸시 알림을 처리합니다.

`DidReceiveRemoteNotification` 의 재정의 `AppDelegate` 클래스는 응용 프로그램이 실행 되 고 알림의 받을 때 호출 되는 경우 들어오는 푸시 알림을 처리 하는 데 사용 됩니다.

```csharp
public override void DidReceiveRemoteNotification(
    UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
    NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

    string alert = string.Empty;
    if (aps.ContainsKey(new NSString("alert")))
        alert = (aps[new NSString("alert")] as NSString).ToString();

    // Show alert
    if (!string.IsNullOrEmpty(alert))
    {
      var notificationAlert = UIAlertController.Create("Notification", alert, UIAlertControllerStyle.Alert);
      notificationAlert.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Cancel, null));
      UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(notificationAlert, true, null);
    }
}
```

`userInfo` 사전에 포함 되어 합니다 `aps` 값인 키를 `alert` 나머지 알림 데이터를 사용 하 여 사전입니다. 이 사전 검색은 사용 하 여는 `string` 대화 상자에 표시 되는 알림 메시지입니다.

> [!NOTE]
> 응용 프로그램 실행 중이 아닌 푸시 알림이 도착 했음을 응용 프로그램 시작 됩니다 있지만 `DidReceiveRemoteNotification` 메서드는 알림을 처리 하지 않습니다. 대신, 알림 페이로드를 가져오고에서 적절 하 게 응답 합니다 `WillFinishLaunching` 또는 `FinishedLaunching` 재정의 합니다.

APNS에 대 한 자세한 내용은 참조 하세요. [iOS에 푸시 알림](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)합니다.

### <a name="android"></a>Android

Android 응용 프로그램에서 푸시 알림 지원을 구현 하기 위한 프로세스는 다음과 같습니다.

1. 추가 된 [Xamarin.Firebase.Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet Android 프로젝트에 패키지 및 응용 프로그램의 대상 버전을 Android 7.0 이상으로 설정 합니다.
1. 추가 된 `google-services.json` Firebase 콘솔에서 Android 프로젝트의 루트에 다운로드 하 고 해당 빌드 작업을 설정 파일을 **GoogleServicesJson**합니다. 자세한 내용은 [Google Services JSON 파일을 추가](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)합니다.
1. Android 매니페스트에서 수신기를 선언 하 여 등록 Firebase 클라우드 메시징 (FCM) 사용 하 여 파일 및 구현 하는 `FirebaseRegistrationService.OnTokenRefresh` 메서드. 자세한 내용은 [Firebase Cloud Messaging 등록](#android_register_fcm)합니다.
1. Azure 알림 허브에 등록 된 `AzureNotificationHubService.RegisterAsync` 메서드. 자세한 내용은 [Azure 알림 허브 등록](#android_register_azure)합니다.
1. 구현 된 `FirebaseNotificationService.OnMessageReceived` 들어오는 푸시 알림을 처리 하는 방법입니다. 자세한 내용은 [푸시 알림의 콘텐츠 표시](#android_displaying_notification)합니다.

Firebase Cloud Messaging에 대 한 자세한 내용은 참조 하십시오 [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) 하 고 [Firebase Cloud Messaging과 Remote Notifications](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)합니다.

<a name="android_register_fcm" />

#### <a name="registering-with-firebase-cloud-messaging"></a>등록 Firebase Cloud Messaging

Android 응용 프로그램에 푸시 알림을 받을 수 있습니다, 전에는 FCM 등록 토큰을 생성 하 고 응용 프로그램에 반환 된 등록 해야 합니다. 등록 토큰에 대 한 자세한 내용은 참조 하세요. [FCM 사용 하 여 등록](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#registration)합니다.

이 방법으로 이루어집니다.

- Android 매니페스트에서 수신기를 선언 합니다. 자세한 내용은 [Android 매니페스트에서 수신기 선언](#declaring_a_receiver)합니다.
- Firebase 인스턴스 ID 서비스를 구현 합니다. 자세한 내용은 [Firebase 인스턴스 ID 서비스 구현](#implementing-firebase-instance-id-service)합니다.

<a name="declaring_a_receiver" />

##### <a name="declaring-the-receiver-in-the-android-manifest"></a>Android 매니페스트에서 수신기를 선언합니다.

편집할 **AndroidManifest.xml** 하 고 다음을 삽입 `<receiver>` 요소를 `<application>` 요소:

```xml
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
  <intent-filter>
    <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
    <category android:name="${applicationId}" />
  </intent-filter>
</receiver>
```

이 XML은 다음 작업을 수행합니다.

- 내부 선언 `FirebaseInstanceIdInternalReceiver` 서비스를 안전 하 게 시작 하는 데 사용 되는 구현 합니다.
- 선언 된 `FirebaseInstanceIdReceiver` 각 앱 인스턴스에 대 한 고유 식별자를 제공 하는 구현 합니다. 또한이 수신자 인증 하 고 작업에 권한을 부여 합니다.

`FirebaseInstanceIdReceiver` 받습니다 `FirebaseInstanceId` 하 고 `FirebaseMessaging` 이벤트에서 파생 된 클래스에 전달 하는 역할 `FirebasesInstanceIdService`.

<a name="implementing-firebase-instance-id-service" />

##### <a name="implementing-the-firebase-instance-id-service"></a>Firebase 인스턴스 ID 서비스를 구현합니다.

클래스를 파생 시켜 이루어집니다 FCM을 사용 하 여 응용 프로그램을 등록 합니다 `FirebaseInstanceIdService` 클래스입니다. 이 클래스는 FCM에 액세스 하는 클라이언트 응용 프로그램을 인증 하는 보안 토큰을 생성 하는 일을 담당 합니다. 샘플 응용 프로그램에서을 `FirebaseRegistrationService` 클래스에서 파생 되는 `FirebaseInstanceIdService` 클래스 하 고 다음 코드 예제에 표시 됩니다:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
public class FirebaseRegistrationService : FirebaseInstanceIdService
{
    const string TAG = "FirebaseRegistrationService";

    public override void OnTokenRefresh()
    {
        var refreshedToken = FirebaseInstanceId.Instance.Token;
        Log.Debug(TAG, "Refreshed token: " + refreshedToken);
        SendRegistrationTokenToAzureNotificationHub(refreshedToken);
    }

    void SendRegistrationTokenToAzureNotificationHub(string token)
    {
        // Update notification hub registration
        Task.Run(async () =>
        {
            await AzureNotificationHubService.RegisterAsync(TodoItemManager.DefaultManager.CurrentClient.GetPush(), token);
        });
    }
}
```

`OnTokenRefresh` 응용 프로그램에서 FCM 등록 토큰을 수신 하는 경우 메서드가 호출 됩니다. 토큰을 검색 하는 메서드는 `FirebaseInstanceId.Instance.Token` FCM에서 비동기적으로 업데이트 되는 속성입니다. `OnTokenRefresh` 메서드가 자주 호출 되 면 토큰은 응용 프로그램을 설치 하거나 제거, 사용자 응용 프로그램의 인스턴스 ID를 지울 때 응용 프로그램 데이터를 삭제 하는 경우에 업데이트 되므로 또는 토큰의 보안 된 경우 손상 됩니다. 또한 FCM 인스턴스 ID 서비스를 사용 하는 응용 프로그램 토큰 새로 고침의 정기적으로, 일반적으로 6 개월 마다 요청 합니다.

합니다 `OnTokenRefresh` 메서드는 호출을 `SendRegistrationTokenToAzureNotificationHub` Azure 알림 허브를 사용 하 여 사용자의 등록 토큰을 연결 하는 데 사용 되는 메서드.

<a name="android_register_azure" />

#### <a name="registering-with-the-azure-notification-hub"></a>Azure 알림 허브 등록

합니다 `AzureNotificationHubService` 클래스를 제공 합니다 `RegisterAsync` Azure 알림 허브를 사용 하 여 사용자의 등록 토큰을 연결 하는 메서드. 다음 코드 예제는 `RegisterAsync` 메서드를 호출한는 `FirebaseRegistrationService` 사용자의 등록 토큰 변경 될 때 클래스:

```csharp
public class AzureNotificationHubService
{
    const string TAG = "AzureNotificationHubService";

    public static async Task RegisterAsync(Push push, string token)
    {
        try
        {
            const string templateBody = "{\"data\":{\"message\":\"$(messageParam)\"}}";
            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBody}
            };

            await push.RegisterAsync(token, templates);
            Log.Info("Push Installation Id: ", push.InstallationId.ToString());
        }
        catch (Exception ex)
        {
            Log.Error(TAG, "Could not register with Notification Hub: " + ex.Message);
        }
    }
}
```

이 메서드는 JSON 및 레지스터 Firebase 등록 토큰을 사용 하 여 알림 허브에서 템플릿 알림을 수신 하는 간단한 알림 메시지 템플릿을 만듭니다. 이렇게 하면 Azure 알림 허브에서 보낸 알림은 등록 토큰이 나타내는 장치는 대상입니다.

<a name="android_displaying_notification" />

#### <a name="displaying-the-contents-of-a-push-notification"></a>푸시 알림의 콘텐츠 표시

클래스를 파생 시켜 이루어집니다 푸시 알림의 콘텐츠 표시는 `FirebaseMessagingService` 클래스입니다. 이 클래스는 재정의 가능한 포함 `OnMessageReceived` 응용 프로그램이 포그라운드에서 실행 되 고 있는지 응용 프로그램이 FCM에서 알림의 받을 때 호출 되는 메서드를 제공 합니다. 샘플 응용 프로그램에서을 `FirebaseNotificationService` 클래스에서 파생 되는 `FirebaseMessagingService` 클래스 하 고 다음 코드 예제에 표시 됩니다:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseNotificationService : FirebaseMessagingService
{
    const string TAG = "FirebaseNotificationService";

    public override void OnMessageReceived(RemoteMessage message)
    {
        Log.Debug(TAG, "From: " + message.From);

        // Pull message body out of the template
        var messageBody = message.Data["message"];
        if (string.IsNullOrWhiteSpace(messageBody))
            return;

        Log.Debug(TAG, "Notification message body: " + messageBody);
        SendNotification(messageBody);
    }

    void SendNotification(string messageBody)
    {
        var intent = new Intent(this, typeof(MainActivity));
        intent.AddFlags(ActivityFlags.ClearTop);
        var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

        var notificationBuilder = new NotificationCompat.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
            .SetContentTitle("New Todo Item")
            .SetContentText(messageBody)
            .SetContentIntent(pendingIntent)
            .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))
            .SetAutoCancel(true);

        var notificationManager = NotificationManager.FromContext(this);
        notificationManager.Notify(0, notificationBuilder.Build());
    }
}
```

응용 프로그램이 FCM에서 알림의 받을 때 합니다 `OnMessageReceived` 메서드는 메시지 콘텐츠를 추출 하 고 호출 된 `SendNotification` 메서드. 이 메서드는 메시지 콘텐츠를 알림 영역에 표시 되는 알림과 사용 하 여 응용 프로그램이 실행 되는 동안 실행 되는 로컬 알림으로 변환 합니다.

##### <a name="handling-notification-intents"></a>알림 의도 처리합니다.

사용자가 알림을 누르면 알림 메시지를 함께 제공 되는 모든 데이터에 사용할 수는 `Intent` extras 합니다. 다음 코드를 사용 하 여이 데이터를 추출할 수 있습니다.

```csharp
if (Intent.Extras != null)
{
  foreach (var key in Intent.Extras.KeySet())
  {
    var value = Intent.Extras.GetString(key);
    Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
  }
}
```

응용 프로그램의 실행 기 `Intent` 이 코드는 함께 제공 되는 데이터를 로그인 하도록 사용자가 해당 알림 메시지를 누를 때 발생 합니다 `Intent` 출력 창에.

### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

유니버설 Windows 플랫폼 (UWP) 하기 전에 응용 프로그램을 Windows 알림 서비스 (WNS)와 함께, 알림 채널을 반환 하는 등록 해야 하는 푸시 알림을 받을 수 있습니다. 등록 하 여 호출 되는 `InitNotificationsAsync` 의 메서드는 `App` 클래스:

```csharp
private async Task InitNotificationsAsync()
{
    var channel = await PushNotificationChannelManager
          .CreatePushNotificationChannelForApplicationAsync();

    const string templateBodyWNS =
        "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

    JObject headers = new JObject();
    headers["X-WNS-Type"] = "wns/toast";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyWNS},
        {"headers", headers} // Needed for WNS.
    };

    await TodoItemManager.DefaultManager.CurrentClient.GetPush()
          .RegisterAsync(channel.Uri, templates);
}
```

이 메서드는 푸시 알림 채널을 가져옵니다 JSON으로 알림 메시지 템플릿을 만들고 알림 허브에서 템플릿 알림을 수신 하도록 장치를 등록 합니다.

합니다 `InitNotificationsAsync` 에서 메서드가 호출 되는 `OnLaunched` 의 재정의 `App` 클래스:

```csharp
protected override async void OnLaunched(LaunchActivatedEventArgs e)
{
    ...
    await InitNotificationsAsync();
}
```

이렇게 하면 푸시 알림 등록이 생성 되었거나 때문 인지 확인 하는 WNS 푸시 채널이 항상 활성 응용 프로그램이 시작 될 때마다 새로 고쳐집니다.

푸시 알림이 수신 되 면 자동으로 표시 됩니다으로 *알림* – 메시지를 포함 하는 모덜리스 창입니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms 응용 프로그램에 Azure Mobile Apps에서 푸시 알림을 보내는 Azure Notification Hubs를 사용 하는 방법을 보여 줍니다. Azure 알림 허브는 다양한 플랫폼 알림 시스템과 통신해야 하는 백 엔드의 복잡성을 제거하면서 모든 백 엔드에서 모든 모바일 플랫폼으로 모바일 푸시 알림을 전송할 수 있는 확장 가능한 푸시 인프라를 제공합니다.


## <a name="related-links"></a>관련 링크

- [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/)
- [Xamarin.Forms 앱에 푸시 알림 추가](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/)
- [IOS의 푸시 알림](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)
- [Firebase 클라우드 메시징](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [TodoAzurePush (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
