---
title: 원격 알림 firebase Cloud Messaging
description: 이 연습에서는 Xamarin.Android 응용 프로그램에서 Firebase Cloud Messaging을 사용 하 여 푸시 알림을 라고도 하는 원격 알림을 구현 하는 방법에 대 한 단계별 설명은 제공 합니다. Firebase Cloud Messaging (FCM) 사용 하 여 통신에 필요한 Android 매니페스트 FCM에 대 한 액세스를 구성 하는 방법의 예제를 제공 하 고는 Firebase를 사용 하 여 다운스트림 메시징을 보여 줍니다.는 다양 한 클래스를 구현 하는 방법을 보여 줍니다. 콘솔입니다.
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: a50a2014e28becacb2c9f4965b7f3377be57ab16
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830316"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>원격 알림 firebase Cloud Messaging

_이 연습에서는 Xamarin.Android 응용 프로그램에서 Firebase Cloud Messaging을 사용 하 여 푸시 알림을 라고도 하는 원격 알림을 구현 하는 방법에 대 한 단계별 설명은 제공 합니다. Firebase Cloud Messaging (FCM) 사용 하 여 통신에 필요한 Android 매니페스트 FCM에 대 한 액세스를 구성 하는 방법의 예제를 제공 하 고는 Firebase를 사용 하 여 다운스트림 메시징을 보여 줍니다.는 다양 한 클래스를 구현 하는 방법을 보여 줍니다. 콘솔입니다._

## <a name="fcm-notifications-overview"></a>FCM 알림 개요

이 연습에서는 기본 앱을 호출 **FCMClient** FCM 메시징의 주요 정보를 설명 하기 위해 만들어집니다. **FCMClient** Google Play 서비스의 존재 여부 확인에서 FCM 등록 토큰을 받습니다, Firebase 콘솔에서 보내는 원격 알림 표시 및 항목 메시지를 구독 합니다.

[![예제 앱 스크린샷](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

다음 항목에서는 영역 살펴봅니다.

1.  백그라운드 알림

2.  항목 메시지

3.  전경 알림

이 연습에서는 증분 방식으로 기능을 추가 합니다 하 **FCMClient** 장치 또는 FCM과 상호 작용 하는 방법을 이해 하는 에뮬레이터에서 실행 합니다. 로깅을 사용 하 여 라이브 앱 트랜잭션 FCM 서버와 미러링 모니터 서버 및 알림 Firebase 콘솔 알림을 GUI에 입력 하는 FCM 메시지에서 생성 되는 방식을 볼 수 있습니다.

## <a name="requirements"></a>요구 사항


숙지 하는 데 도움이 됩니다 합니다 [다양 한 유형의 메시지](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) Firebase Cloud Messaging으로 보낼 수 있습니다. 메시지의 페이로드에 클라이언트 앱을 수신 하 고 메시지를 처리할 방법을 결정 합니다.

이 연습을 진행할 수 있습니다, 전에 Google의 FCM 서버를 사용 하는 데 필요한 자격 증명을 획득 해야 합니다. 이 프로세스는 설명 [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#setup_fcm)합니다.
특히 다운로드 해야 합니다 **google-services.json** 이 연습에서 예제 코드를 사용 하는 파일입니다. 경우 아직 만들지 않은 프로젝트 Firebase 콘솔에서 (아직 다운로드 하지 않은 경우 또는 합니다 **google-services.json** 파일)를 참조 하세요 [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)합니다.

예제 앱을 실행 하려면 Android 테스트 장치 또는 Firebase 호환 되어 있는 에뮬레이터를 해야 합니다. Android 4.0 이상에서 실행 하는 클라이언트를 지 원하는 firebase Cloud Messaging 및 이러한 장치는 Google Play 스토어 앱이 설치 되어 있어야 합니다. (Google Play Services 9.2.1 이상이 필요). Google Play 스토어 앱이 장치에 설치 합니다. 아직 없는 경우 방문 합니다 [Google Play](https://support.google.com/googleplay) 웹 사이트를 다운로드 하 여 설치 해야 합니다. 또는 Google Play 서비스를 설치 하는 대신 (필요가 없습니다 Android SDK 에뮬레이터를 사용 하는 경우 Google Play 스토어를 설치 하려면) 테스트 장치를 사용 하 여 Android SDK 에뮬레이터를 사용할 수 있습니다.

## <a name="start-an-app-project"></a>앱 프로젝트를 시작 합니다.

먼저 빈 Xamarin.Android 라는 새 프로젝트를 만듭니다 **FCMClient**합니다. Xamarin.Android 프로젝트 만들기와 잘 모르는 경우 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)합니다.
새 앱을 만든 후 다음 단계는 패키지 이름을 FCM와의 통신에 사용할 몇 가지 NuGet 패키지를 설치 합니다.

### <a name="set-the-package-name"></a>패키지 이름 설정

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md), FCM 지원 앱에 대 한 패키지 이름을 지정 합니다. 이 패키지 이름으로도 사용 합니다 [ *응용 프로그램 ID* ](./firebase-cloud-messaging.md#fcm-in-action-app-id) 연관 된를 [API 키](firebase-cloud-messaging.md#fcm-in-action-api-key)합니다. 이 패키지 이름을 사용 하도록 앱을 구성 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  에 대 한 속성을 엽니다는 **FCMClient** 프로젝트입니다.

2.  에 **Android 매니페스트** 페이지에서 패키지 이름을 설정 합니다.

패키지 이름을 다음 예에서 `com.xamarin.fcmexample`:

[![패키지 이름 설정](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

업데이트 하는 동안를 **Android 매니페스트**에 확인 해야 하는 `Internet` 권한을 사용 하도록 설정 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  에 대 한 속성을 엽니다는 **FCMClient** 프로젝트입니다.

2.  에 **Android 응용 프로그램** 페이지에서 패키지 이름을 설정 합니다.

패키지 이름을 다음 예에서 `com.xamarin.fcmexample`:

[![패키지 이름 설정](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

업데이트 하는 동안 합니다 **Android 매니페스트**도 확인 해야 하는 `INTERNET` 권한을 사용 하도록 설정 (아래 **필요한 권한**).

-----

> [!IMPORTANT]
> 클라이언트 앱이 패키지 이름은 그렇지 않은 경우 FCM에서 등록 토큰을 받을 수 없게 됩니다 *정확 하 게* Firebase 콘솔에 입력 된 패키지 이름과 일치 합니다.

### <a name="add-the-xamarin-google-play-services-base-package"></a>Xamarin Google Play 서비스 기본 패키지를 추가 합니다.

Firebase Cloud Messaging Google Play 서비스에 따라 달라 지므로 합니다 [Xamarin Google Play 서비스-기본](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) Xamarin.Android 프로젝트에 NuGet 패키지를 추가 해야 합니다. 버전 29.0.0.2가 필요 이상.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Visual Studio에서 마우스 오른쪽 단추로 클릭 **참조 > NuGet 패키지 관리...** .

2.  클릭 합니다 **찾아보기** 탭 및 검색할 **Xamarin.GooglePlayServices.Base**합니다.

3.  이 패키지를 설치 합니다 **FCMClient** 프로젝트:

    [![Google Play Services 자료를 설치합니다.](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가...** .

2.  검색할 **Xamarin.GooglePlayServices.Base**합니다.

3.  이 패키지를 설치 합니다 **FCMClient** 프로젝트:

    [![Google Play Services 자료를 설치합니다.](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

NuGet 설치 하는 동안 오류를 받게 되 면 닫습니다 합니다 **FCMClient** 프로젝트를 다시 열고 NuGet 설치를 다시 시도 합니다.

설치 하는 경우 **Xamarin.GooglePlayServices.Base**, 모든 필요한 종속성도 설치 됩니다. 편집할 **MainActivity.cs** 추가한 다음 `using` 문:

```csharp
using Android.Gms.Common;
```

이 문을 사용 하면 합니다 `GoogleApiAvailability` 클래스의 **Xamarin.GooglePlayServices.Base** 를 사용할 수 있습니다 **FCMClient** 코드입니다.
`GoogleApiAvailability` Google Play 서비스의 존재 여부 확인을 위해 사용 합니다.

### <a name="add-the-xamarin-firebase-messaging-package"></a>Xamarin Firebase Messaging 패키지를 추가 합니다.

FCM에서 메시지를 수신 하는 [Xamarin Firebase-메시징](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) 앱 프로젝트에 NuGet 패키지를 추가 해야 합니다. 이 패키지 없이 Android 응용 프로그램 FCM 서버에서 메시지를 받을 수 없습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Visual Studio에서 마우스 오른쪽 단추로 클릭 **참조 > NuGet 패키지 관리...** .

2. 검색할 **Xamarin.Firebase.Messaging**합니다.

3.  이 패키지를 설치 합니다 **FCMClient** 프로젝트:

    [![Xamarin Firebase 메시징 설치](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가...** .

2.  검색할 **Xamarin.Firebase.Messaging**합니다.

3.  이 패키지를 설치 합니다 **FCMClient** 프로젝트:

    [![Xamarin Firebase 메시징 설치](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----

설치 하는 경우 **Xamarin.Firebase.Messaging**, 모든 필요한 종속성도 설치 됩니다.

다음으로 편집 **MainActivity.cs** 추가한 다음 `using` 문:

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

처음 두 문은에서 형식을 확인 합니다 **Xamarin.Firebase.Messaging** NuGet 패키지를 사용할 수 있습니다 **FCMClient** 코드입니다. **Android.Util** FMS 사용 하 여 트랜잭션을 확인 하는 데 사용할 로깅 기능을 추가 합니다.

### <a name="add-googleplayservices-json"></a>Google 서비스 JSON 파일 추가

다음 단계를 추가 하는 것은 **google-services.json** 파일을 프로젝트의 루트 디렉터리:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  복사본 **google-services.json** 프로젝트 폴더에 있습니다.

2.  추가 **google-services.json** 앱 프로젝트에 (클릭 **모든 파일 표시** 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 **google-services.json**을 선택한 후 **프로젝트에 포함**).

3.  선택 **google-services.json** 에 **솔루션 탐색기** 창입니다.

4.  에 **속성** 창 설정 된 **빌드 작업** 에 **GoogleServicesJson**:

    [![GoogleServicesJson 빌드 작업 설정](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)

    > [!NOTE] 
    > 경우는 **GoogleServicesJson** 빌드 작업이 표시 되지 않습니다, 저장 및 솔루션을 닫은 다음 다시 여십시오.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  복사본 **google-services.json** 프로젝트 폴더에 있습니다.

2.  추가 **google-services.json** 앱 프로젝트에 있습니다.

3.  마우스 오른쪽 단추로 클릭 **google-services.json**합니다.

4.  설정 된 **빌드 작업** 하 **GoogleServicesJson**:

    [![GoogleServicesJson 빌드 작업 설정](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)

-----

때 **google-services.json** 프로젝트에 추가 됩니다 (하며 **GoogleServicesJson** 빌드 작업이 설정), 클라이언트 ID를 추출 하는 빌드 프로세스 및 [API 키](./firebase-cloud-messaging.md#fcm-in-action-api-key) 차례로 병합/생성이 자격 증명 추가 **AndroidManifest.xml** 에 있는 **obj/Debug/android/AndroidManifest.xml**합니다. 이 병합 프로세스는 자동으로 모든 사용 권한 및 FCM 서버 연결에 필요한 다른 FCM 요소를 추가 합니다.


## <a name="check-for-google-play-services-and-create-a-notification-channel"></a>Google Play 서비스를 확인 하 고 알림 채널을 만드는

Google에 Google Play 서비스 기능에 액세스 하기 전에 Android 앱의 Google Play Services APK가 있는지 확인 하는 것이 좋습니다 (자세한 내용은 [Google Play 서비스에 대 한 확인](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)).

앱의 UI에 대 한 초기 레이아웃을 먼저 만들어집니다. 편집할 **Resources/layout/Main.axml** 다음 XML을 사용 하 여 해당 내용을 바꿉니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">
    <TextView
        android:text=" "
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/msgText"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:padding="10dp" />
</LinearLayout>
```

이 `TextView` Google Play Services 설치 되어 있는지 여부를 나타내는 메시지를 표시 하 게 됩니다. 변경 내용을 저장 **Main.axml**합니다.


편집할 **MainActivity.cs** 다음 인스턴스 변수를 추가 하 고는 `MainActivity` 클래스:

```csharp
public class MainActivity : AppCompatActivity
{
    static readonly string TAG = "MainActivity";

    internal static readonly string CHANNEL_ID = "my_notification_channel";
    internal static readonly int NOTIFICATION_ID = 100;

    TextView msgText;
```

변수 `CHANNEL_ID` 하 고 `NOTIFICATION_ID` 메서드에서 사용할 [ `CreateNotificationChannel` ](#create-notification-channel-code) 에 추가 되는 `MainActivity` 이 연습의 뒷부분에서.


다음 예제에서는 `OnCreate` 메서드는 FCM 서비스를 사용 하려고 하기 전에 Google Play 서비스를 사용할 수 있는지 확인 합니다.
다음 메서드를 추가 합니다 `MainActivity` 클래스:

```csharp
public bool IsPlayServicesAvailable ()
{
    int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable (this);
    if (resultCode != ConnectionResult.Success)
    {
        if (GoogleApiAvailability.Instance.IsUserResolvableError (resultCode))
            msgText.Text = GoogleApiAvailability.Instance.GetErrorString (resultCode);
        else
        {
            msgText.Text = "This device is not supported";
            Finish ();
        }
        return false;
    }
    else
    {
        msgText.Text = "Google Play Services is available.";
        return true;
    }
}
```

이 코드는 장치에 Google Play Services APK가 설치 되어 있는지 확인 합니다. 에 메시지가 표시 됩니다 설치 되지 않은 경우는 `TextBox` 지시 하는 사용자가 Google Play 스토어에서 APK를 다운로드 하려면 (하거나 장치의 시스템 설정에서 사용 하도록 설정).

<a name="create-notification-channel-code"></a>이상 (API 레벨 26) Android 8.0에서 실행 되는 앱을 만들어야 합니다는 [ _알림 채널_ ](~/android/app-fundamentals/notifications/local-notifications.md) 해당 알림을 게시 합니다.  다음 메서드를 추가 합니다 `MainActivity` (필요한 경우) 알림 채널을 만드는 클래스:

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

    var channel = new NotificationChannel(CHANNEL_ID,
                                          "FCM Notifications",
                                          NotificationImportance.Default)
                  {

                      Description = "Firebase Cloud Messages appear in this channel"
                  };

    var notificationManager = (NotificationManager)GetSystemService(Android.Content.Context.NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

`OnCreate` 메서드를 다음 코드로 바꿉니다.

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();

    CreateNotificationChannel();
}
```

`IsPlayServicesAvailable` 끝에 라고 `OnCreate` Google Play 서비스 실행에는 각 앱 시작 될 때마다 확인 되도록 합니다. 메서드가 `CreateNotificationChannel` 이상인 Android 8을 실행 하는 장치에 대 한 알림 채널을 존재 하는지 확인 하기 위해 호출 합니다. 앱에는 `OnResume` 호출할 메서드를 `IsPlayServicesAvailable` 에서 `OnResume` 도 합니다. 완전히 다시 빌드하고 앱을 실행 합니다. 모든 작업은 올바르게 구성 되었는지, 다음 스크린샷과 같은 화면이 표시 됩니다.

[![앱은 Google Play 서비스를 사용할 수 있는지 나타냅니다.](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

이 결과 얻지 못한 경우 장치에서 Google Play Services APK 설치 되어 있는지 확인 (자세한 내용은 [설정은 Google Play 서비스](https://developers.google.com/android/guides/setup)).
또한 추가 했는지 확인 합니다 **Xamarin.Google.Play.Services.Base** 패키지를 프로그램 **FCMClient** 앞에서 설명한 대로 프로젝트.


## <a name="add-the-instance-id-receiver"></a>인스턴스 ID 수신기 추가

확장 하는 서비스를 추가 하려면 다음 단계는 `FirebaseInstanceIdService` 회전 생성, 처리를 업데이트 [Firebase 등록 토큰](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token)합니다. `FirebaseInstanceIdService` 서비스가 장치에 메시지를 보낼 수 있게 되기를 FCM에 대 한 필요 합니다. 경우는 `FirebaseInstanceIdService` 앱을 자동으로 FCM 메시지를 수신 하 고 앱은 backgrounded 때마다 알림으로 표시할 서비스 클라이언트 앱에 추가 됩니다.

### <a name="declare-the-receiver-in-the-android-manifest"></a>Android 매니페스트에서 수신기를 선언 합니다.

편집할 **AndroidManifest.xml** 하 고 다음을 삽입 `<receiver>` 요소를 `<application>` 섹션:

```xml
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver"
    android:exported="false" />
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
    </intent-filter>
</receiver>
```

이 XML은 다음을 수행 합니다.

-   선언를 `FirebaseInstanceIdReceiver` 구현을 제공 하는 [고유 식별자](https://developers.google.com/instance-id/) 각 앱 인스턴스에 대 한 합니다. 또한이 수신자 인증 하 고 작업에 권한을 부여 합니다.

-   내부 선언 `FirebaseInstanceIdInternalReceiver` 서비스를 안전 하 게 시작 하는 데 사용 되는 구현 합니다.

-   [앱 ID](./firebase-cloud-messaging.md#fcm-in-action-app-id) 에 저장 되는 **google-services.json** 있던 파일 [프로젝트에 추가](#add-googleplayservices-json)합니다. Xamarin.Android Firebase 바인딩 토큰 바뀝니다 `${applicationId}` 앱 ID; 인 추가 코드 없이 앱에 필요한 클라이언트 앱 ID를 제공 하려면

`FirebaseInstanceIdReceiver` 은 `WakefulBroadcastReceiver` 받는 `FirebaseInstanceId` 및 `FirebaseMessaging` 이벤트에서 파생 된 클래스를 전달 하는 역할 `FirebaseInstanceIdService`입니다.

### <a name="implement-the-firebase-instance-id-service"></a>Firebase 인스턴스 ID 서비스를 구현 합니다.

FCM을 사용 하 여 응용 프로그램을 등록 하는 작업은 사용자 지정에서 처리 `FirebaseInstanceIdService` 제공 하는 서비스입니다.
`FirebaseInstanceIdService` 다음 단계를 수행합니다.

1.  사용 하 여는 [인스턴스 ID API](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) FCM와 앱 서버에 액세스할 클라이언트 앱에 권한을 부여 하는 보안 토큰을 생성 합니다. 반환 시 앱을 다시 가져옵니다를 [등록 토큰](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token) FCM에서.

2.  앱 서버에 필요한 경우 앱 서버에 등록 토큰을 전달 합니다.

이라는 새 파일 추가 **MyFirebaseIIDService.cs** 및 해당 템플릿 코드를 다음으로 바꿉니다.

```csharp
using System;
using Android.App;
using Firebase.Iid;
using Android.Util;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class MyFirebaseIIDService : FirebaseInstanceIdService
    {
        const string TAG = "MyFirebaseIIDService";
        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "Refreshed token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }
        void SendRegistrationToServer(string token)
        {
            // Add custom implementation, as needed.
        }
    }
}
```

이 서비스를 구현 하는 `OnTokenRefresh` 등록 토큰을 처음 만들거나 변경할 때 호출 되는 메서드입니다. 때 `OnTokenRefresh` 실행에서 최신 토큰을 검색 하는 `FirebaseInstanceId.Instance.Token` 속성 (되는 FCM에서 비동기적으로 업데이트) 합니다. 이 예제에서는 출력 창에서 볼 수 있도록 새로 고침된 토큰은 기록 됩니다.

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` 가 자주 호출: 다음과 같은 상황 토큰 업데이트 데 사용 됩니다.

-   경우 앱을 설치 하거나 제거 합니다.

-   경우 사용자 앱 데이터를 삭제 합니다.

-   인스턴스 id입니다. 앱을 삭제 하는 경우

-   토큰의 보안이 손상 되었을 때.

Google에 따라 [INSTANCE-ID](https://developers.google.com/instance-id/guides/android-implementation) 설명서 FCM 인스턴스 ID 서비스는 요청는 앱 토큰을 새로 고치는 주기적으로 (일반적으로 6 개월 마다).

`OnTokenRefresh` 또한 호출 `SendRegistrationToAppServer` 사용자의 등록을 연결 하는 서버 쪽 계정 (해당 되는 경우)를 사용 하 여 토큰 유지 관리 되는 응용 프로그램에서:

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

앱 서버의 디자인에 따라이 구현 때문에 빈 메서드 본문은이 예제에 제공 됩니다. 앱 서버는 FCM 등록 정보에 필요한 경우 수정 `SendRegistrationToAppServer` 앱에서 유지 관리 하는 모든 서버 쪽 계정을 사용 하 여 사용자의 FCM 인스턴스 ID 토큰을 연결할 수 있도록 합니다. (참고 토큰이 클라이언트 앱에 불투명 합니다.)

앱 서버에 토큰을 보내면 `SendRegistrationToAppServer` 서버에 토큰을 보냈는지 여부를 나타내는 부울을 유지 해야 합니다. 이 부울이 false 인 경우 `SendRegistrationToAppServer` 토큰에서 앱 서버로 보내는 &ndash; 토큰 이전 호출에서 앱 서버로 이미 전송 된이 고, 그렇지 합니다. 일부 경우에 (이와 같은 `FCMClient` 예제)를 앱 서버 토큰을 사용 하지 않아도; 따라서이 메서드는이 예제에 필요 없습니다.

## <a name="implement-client-app-code"></a>클라이언트 앱 코드를 구현 합니다.

수신기 서비스를 배치한 했으므로 이러한 서비스를 활용 하려면 클라이언트 앱 코드를 작성할 수 있습니다. 다음 섹션에서는 단추 등록 토큰을 기록할 UI에 추가 됩니다 (라고도 합니다 *인스턴스 ID 토큰*), 더 많은 코드에 추가 됩니다 `MainActivity` 보려는 `Intent` 정보에서 앱을 실행 하는 경우는 알림:

[![앱 화면에 추가 하는 로그 토큰 단추](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokens"></a>로그 토큰

이 단계에서 추가한 코드는 설명 위해서만 제공 됩니다 &ndash; 프로덕션 클라이언트 앱 등록 토큰을 기록할 필요 없을 것입니다. 편집할 **Resources/layout/Main.axml** 다음을 추가 합니다 `Button` 선언 바로 뒤를 `TextView` 요소:

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

`MainActivity.OnCreate` 메서드의 끝에 다음 코드를 추가합니다.

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

이 코드는 현재 토큰을 출력 창에 기록 하면 합니다 **로그 토큰** 단추를 탭 할.

### <a name="handle-notification-intents"></a>알림 의도 처리 합니다.

사용자에서 발급 된 알림을 탭 하는 경우 **FCMClient**해당 알림을 함께 제공 되는 모든 데이터, 메시지에 사용할 수 있게 됩니다 `Intent` extras 합니다. 편집할 **MainActivity.cs** 의 맨 위에 다음 코드를 추가 합니다 `OnCreate` 메서드 (호출 하기 전에 `IsPlayServicesAvailable`):

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

앱 시작 관리자 `Intent` 이 코드는 함께 제공 되는 데이터를 로그인 하도록 사용자가 해당 알림 메시지를 누를 때 발생 합니다 `Intent` 출력 창에 있습니다. 다른 `Intent` 발생 해야 합니다 `click_action` 알림 메시지의 필드를 설정 해야 합니다 `Intent` (시작 관리자 `Intent` 하지 않은 경우에 `click_action` 지정).


## <a name="background-notifications"></a>백그라운드 알림

빌드 및 실행 합니다 **FCMClient** 앱. 합니다 **로그 토큰** 단추가 표시 됩니다.

[![로그 토큰 단추가 표시 되는지](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

탭의 **로그 토큰** 단추입니다. IDE 출력 창에 다음과 같은 메시지가 나타납니다.

[![출력 창에 표시 되는 인스턴스 ID 토큰](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

긴 문자열 레이블로 **토큰** Firebase 콘솔에 붙여넣기는 인스턴스 ID 토큰은 &ndash; 선택 하 고이 문자열을 클립보드에 복사 합니다. 인스턴스 ID 토큰에 표시 되지 않으면 다음 줄의 맨 위에 추가 합니다 `OnCreate` 되어 있는지 확인 하는 방법 **google-services.json** 올바르게 구문 분석:

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

합니다 `google_app_id` 출력 창에 기록 하는 값과 일치 해야 합니다 `mobilesdk_app_id` 에 기록 된 값 **google-services.json**합니다.

### <a name="send-a-message"></a>메시지 보내기

에 로그인 합니다 [Firebase 콘솔](https://console.firebase.google.com)프로젝트를 선택, 클릭 **알림을**를 클릭 하 고 **첫 번째 메시지 보내기**:

[![보내기 Your 첫 번째 메시지 단추](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

에 **Compose 메시지** 페이지에서 메시지 텍스트를 입력 하 고 선택 **단일 장치**합니다. IDE 출력 창에서 인스턴스 ID 토큰을 복사 하 고 붙여 넣습니다 합니다 **FCM 등록 토큰** Firebase 콘솔 필드:

[![메시지 대화 상자를 구성 합니다.](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Android를 탭 하 여 Android 장치 (또는 에뮬레이터)에서 앱을 백그라운드 **개요** 단추 및 홈 화면을 터치 합니다. 장치가 준비 되 면 클릭 **SEND MESSAGE** Firebase 콘솔에서:

[![보내기 메시지 단추](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

경우는 **검토 메시지** 대화 상자가 표시 됩니다, 클릭 **보낼**합니다.
알림 아이콘은 장치 (또는 에뮬레이터)의 알림 영역에 표시 됩니다.

[![알림 아이콘이 표시 됩니다.](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

메시지를 보려면 알림 아이콘을 엽니다. 알림 메시지 해야 정확 하 게 입력 된 항목의 **메시지 텍스트** Firebase 콘솔 필드:

[![장치에 알림 메시지가 표시 됩니다.](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

시작 알림 아이콘을 탭 합니다 **FCMClient** 앱. 합니다 `Intent` 보낼 extras **FCMClient** IDE 출력 창에 나열 됩니다.

[![의도 추가 기능 키, 메시지 ID와 축소 키 나열](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

이 예제에서는 합니다 **에서** Firebase 프로젝트 수는 앱의 키가 설정 (이 예제의 `41590732`), 및 **collapse_key** 해당 패키지 이름으로 설정 됩니다 ( **com.xamarin.fcmexample**).
메시지를 받지 않는 경우 시도 삭제 하는 **FCMClient** 장치 (또는 에뮬레이터)에서 앱 위의 단계를 반복 합니다.


> [!NOTE]
> 경우 있습니다 강제 닫기 앱, FCM 알림 배달 중지 됩니다. Android에서 중지 된 응용 프로그램의 구성 요소를 시작 하 실수로 또는 불필요 하 게 백그라운드 서비스 브로드캐스트를 방지 합니다. (이 동작에 대 한 자세한 내용은 참조 하세요. [중지 된 응용 프로그램에서 컨트롤 시작](https://developer.android.com/about/versions/android-3.1.html#launchcontrols).) 따라서 필요한 것 때마다 앱을 수동으로 제거 하 고 실행 하 고이 디버그 세션에서 중지 &ndash; 메시지는 계속 받을 수 있도록 새 토큰을 생성 하는 FCM 강제로이 있습니다.

### <a name="add-a-custom-default-notification-icon"></a>사용자 지정 기본 알림 아이콘이 추가

이전 예제에서 알림 아이콘은 응용 프로그램 아이콘으로 설정 됩니다. 다음 XML에는 알림에 대 한 사용자 지정 기본 아이콘을 구성합니다. Android에 있는 알림 아이콘은 명시적으로 설정 되지 모든 알림 메시지에 대 한이 사용자 지정 기본 아이콘이 표시 됩니다.

사용자 지정 기본 알림 아이콘을 추가 하려면 프로그램 아이콘을 추가 합니다 **리소스/drawable** 디렉터리 편집 **AndroidManifest.xml**, 다음을 삽입 하 고 `<meta-data>` 요소를 `<application>`섹션:

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

이 예제에서는 알림 아이콘 상주 하는 언제 **리소스/drawable/ic\_stat\_ic\_notification.png** 사용자 지정 기본 알림 아이콘으로 사용 됩니다. 사용자 지정 기본 아이콘의 구성 되지 않은 경우 **AndroidManifest.xml** 알림 페이로드가 없는 아이콘 설정 되어, Android (처럼 위의 알림 아이콘 스크린샷) 알림 아이콘으로 응용 프로그램 아이콘을 사용 합니다.

## <a name="handle-topic-messages"></a>항목의 메시지 처리

지금까지 작성 한 코드는 등록 토큰을 처리 하 고 앱에 원격 알림 기능을 추가 합니다. 다음 예제에 대 한 수신 대기 하는 코드를 추가 *항목 메시지* 원격 알림을 사용자에 게 전달 합니다. 항목 메시지는 특정 토픽을 구독 하는 하나 이상의 장치에 전송 되는 FCM 메시지입니다. 항목 메시지에 대 한 자세한 내용은 참조 하세요. [항목에서는 메시징](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)합니다.

### <a name="subscribe-to-a-topic"></a>토픽 구독

편집할 **Resources/layout/Main.axml** 다음을 추가 합니다 `Button` 선언 이전 직후 `Button` 요소:

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

추가 하는이 XML을 **알림을 구독 하려면** 레이아웃 단추입니다.
편집할 **MainActivity.cs** 의 끝에 다음 코드를 추가 하 고는 `OnCreate` 메서드:

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

이 코드를 찾습니다는 **알림을 구독 하려면** 레이아웃에서 단추를 호출 하는 코드에 해당 클릭 처리기를 할당 하 고 `FirebaseMessaging.Instance.SubscribeToTopic`구독된 항목에 전달 _뉴스_. 사용자가 누를 때 합니다 **구독** 앱 구독 단추를 _뉴스_ 항목. 다음 섹션에는 _뉴스_ Firebase 콘솔 알림을 GUI에서 항목 메시지를 보냅니다.

### <a name="send-a-topic-message"></a>항목 메시지 보내기

앱을 제거 하 고 다시 만드는 것을 다시 실행 합니다. 클릭 합니다 **알림을 구독할** 단추:

[![구독 알림 단추](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

앱에서 성공적으로 구독 하는 경우 나타납니다 **성공한 항목 동기화** IDE의 출력 창:

[![출력 창 항목 동기화 성공 메시지를 보여 줍니다.](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

항목 메시지를 보내려면 다음 단계를 사용 합니다.

1.  Firebase 콘솔에서 클릭 **새 메시지**합니다.

2.  에 **Compose 메시지** 페이지에서 메시지 텍스트를 입력 하 고 선택 **항목**합니다.

3.  에 **항목** 풀 다운 메뉴에서 기본 제공 항목 선택 **뉴스**:

    [![뉴스 항목 선택](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4.  Android를 탭 하 여 Android 장치 (또는 에뮬레이터)에서 앱을 백그라운드 **개요** 단추 및 홈 화면을 터치 합니다.

5.  장치가 준비 되 면 클릭 **SEND MESSAGE** Firebase 콘솔에서.

6.  보려는 IDE 출력 창을 확인 하십시오 **/항목/뉴스** 로그 출력에서:

    [![/Topic/news에서 메시지가 표시 됩니다.](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

이 메시지는 출력 창에 표시 됩니다, Android 장치에서 알림 영역에서 알림 아이콘도 나타납니다. 항목 메시지를 보려면 알림 아이콘을 엽니다.

[![항목 메시지 알림으로 표시 됩니다.](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

메시지를 받지 않는 경우 시도 삭제 하는 **FCMClient** 장치 (또는 에뮬레이터)에서 앱 위의 단계를 반복 합니다.

## <a name="foreground-notifications"></a>전경 알림

구현 해야 foregrounded 앱에서 알림을 받으려면 `FirebaseMessagingService`합니다. 또한이 서비스는 데이터 페이로드를 수신 하는 것에 대 한 업스트림 메시지를 보내는 데 필요 합니다. 다음 예제에서는 확장 하는 서비스를 구현 하는 방법을 보여 줍니다 `FirebaseMessagingService` &ndash; 결과 앱이 포그라운드에서 실행 되는 동안 원격 알림을 처리할 수 있게 됩니다.

### <a name="implement-firebasemessagingservice"></a>FirebaseMessagingService 구현

`FirebaseMessagingService` 서비스는 수신 및 Firebase 로부터 메시지를 처리 하는 일을 담당 합니다. 각 앱이 형식과 재정의 서브 클래스 해야는 `OnMessageReceived` 들어오는 메시지를 처리 합니다. 앱이 포그라운드에서 하는 경우는 `OnMessageReceived` 콜백 메시지를 항상 처리 됩니다.

> [!NOTE]
> 앱에 하나만 들어오는 Firebase 클라우드 메시지를 처리 하는 10 초입니다. 와 같은 라이브러리를 사용 하 여 백그라운드 실행 예약 되어야 합니다.이 보다 오래 소요 되는 모든 작업은 [Android 작업 스케줄러](~/android/platform/android-job-scheduler.md) 또는 [Firebase 작업 디스패처](~/android/platform/firebase-job-dispatcher.md)합니다.

이라는 새 파일 추가 **MyFirebaseMessagingService.cs** 및 해당 템플릿 코드를 다음으로 바꿉니다.

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Media;
using Android.Util;
using Firebase.Messaging;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class MyFirebaseMessagingService : FirebaseMessagingService
    {
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
        }
    }
}
```

유의 합니다 `MESSAGING_EVENT` 새 FCM 메시지 이동 되도록 의도 필터 선언 해야 `MyFirebaseMessagingService`:

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

클라이언트 앱 FCM에서 메시지를 받으면 `OnMessageReceived` 전달 기능에서 메시지 콘텐츠를 추출 `RemoteMessage` 개체를 호출 하 여 해당 `GetNotification` 메서드. 다음으로 IDE 출력 창에서 볼 수 있도록 메시지 콘텐츠를 기록 합니다.

```csharp
var body = message.GetNotification().Body;
Log.Debug(TAG, "Notification Message Body: " + body);
```

> [!NOTE]
> 중단점을 설정한 경우 `FirebaseMessagingService`, 디버깅 세션 수도 FCM 메시지를 배달 하는 방법으로 인해 이러한 중단점을 적중 하지 않을 수 있습니다.


### <a name="send-another-message"></a>다른 메시지 보내기

앱을 제거, 다시 작성, 다시 실행 및 다른 메시지를 보내려면 다음이 단계를 수행 합니다.

1.  Firebase 콘솔에서 클릭 **새 메시지**합니다.

2.  에 **Compose 메시지** 페이지에서 메시지 텍스트를 입력 하 고 선택 **단일 장치**합니다.

3.  IDE 출력 창에서 토큰 문자열을 복사 하 고 붙여 넣습니다 합니다 **FCM 등록 토큰** 이전과 Firebase 콘솔의 필드입니다.

4.  앱이 포그라운드에서 실행 중인지 확인 한 다음 클릭 **SEND MESSAGE** Firebase 콘솔에서:

    [![콘솔에서 다른 메시지 보내기](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5.  경우는 **검토 메시지** 대화 상자가 표시 됩니다, 클릭 **보낼**합니다.

6.  들어오는 메시지는 IDE 출력 창에 기록 됩니다.

    [![메시지 본문 출력 창에 인쇄](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)


### <a name="add-a-local-notification-sender"></a>로컬 알림 보낸 사람 추가

이 예에서 나머지 들어오는 FCM 메시지에는 앱이 포그라운드에서 실행 되는 동안 시작 되는 로컬 알림으로 변환 됩니다. 편집할 **MyFirebaseMessageService.cs** 추가한 다음 `using` 문:

```csharp
using FCMClient;
using System.Collections.Generic;
```

다음 메서드를 추가 `MyFirebaseMessagingService`:

<a name="sendnotification-method"></a>

```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (var key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }

    var pendingIntent = PendingIntent.GetActivity(this,
                                                  MainActivity.NOTIFICATION_ID,
                                                  intent,
                                                  PendingIntentFlags.OneShot);

    var notificationBuilder = new  NotificationCompat.Builder(this, MainActivity.CHANNEL_ID)
                              .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
                              .SetContentTitle("FCM Message")
                              .SetContentText(messageBody)
                              .SetAutoCancel(true)
                              .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(MainActivity.NOTIFICATION_ID, notificationBuilder.Build());
}
```

백그라운드 알림에서이 알림의 구분 하기 위해이 코드는 응용 프로그램 아이콘에서 다른 아이콘을 사용 하 여 알림을 표시 합니다. 파일에 추가 합니다 [ic\_stat\_ic\_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) 에 **리소스/drawable** 에 포함 된 **FCMClient** 프로젝트 .

합니다 `SendNotification` 메서드 `NotificationCompat.Builder` 알림을 만들려면 및 `NotificationManagerCompat` 알림을 시작 하는 데 사용 됩니다. 알림을 포함 한 `PendingIntent` 사용자가 앱을 열고를 전달 된 문자열의 내용을 볼 수 있는 `messageBody`합니다. 에 대 한 자세한 내용은 `NotificationCompat.Builder`를 참조 하세요 [로컬 알림을](~/android/app-fundamentals/notifications/local-notifications.md)합니다.

호출 된 `SendNotification` 메서드 끝에는 `OnMessageReceived` 메서드:

```csharp
public override void OnMessageReceived(RemoteMessage message)
{
    Log.Debug(TAG, "From: " + message.From);

    var body = message.GetNotification().Body;
    Log.Debug(TAG, "Notification Message Body: " + body);
    SendNotification(body, message.Data);
}
```

이러한 변경으로 인해 `SendNotification` 앱이 포그라운드에서 알림 영역에 알림이 표시 됩니다 하는 동안 알림의 받을 때마다 실행 됩니다.

앱이 백그라운드에서 하는 경우를 [메시지의 페이로드에](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) 메시지를 처리 하는 방법을 결정 합니다.

* **알림** &ndash; 전송할 메시지를 **시스템 트레이에서**합니다. 로컬 알림이 나타납니다. 사용자가 알림을 탭 하면 앱이 시작 됩니다.
* **데이터** &ndash; 메시지에서 처리 되는 `OnMessageReceived`합니다.
* **둘 다** &ndash; 시스템 트레이에 알림 및 데이터 페이로드는 메시지가 배달 됩니다. 앱이 시작 되는 데이터 페이로드에 표시 됩니다는 `Extras` 의 `Intent` 는 앱을 시작 하는 데 사용 된 합니다.

이 예제에서는 앱 backgrounded는 `SendNotification` 메시지의 데이터 페이로드를 실행 합니다. 그렇지 않은 경우 (이 연습의 앞부분에서 설명) 백그라운드 알림 시작 됩니다.

### <a name="send-the-last-message"></a>마지막 메시지를 전송 합니다.

앱을 제거, 다시, 다시 실행 한 다음 마지막 메시지를 보내려면 다음 단계를 사용 하 여:

1.  Firebase 콘솔에서 클릭 **새 메시지**합니다.

2.  에 **Compose 메시지** 페이지에서 메시지 텍스트를 입력 하 고 선택 **단일 장치**합니다.

3.  IDE 출력 창에서 토큰 문자열을 복사 하 고 붙여 넣습니다 합니다 **FCM 등록 토큰** 이전과 Firebase 콘솔의 필드입니다.

4.  앱이 포그라운드에서 실행 중인지 확인 한 다음 클릭 **SEND MESSAGE** Firebase 콘솔에서:

    [![전경 메시지 보내기](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

이 이때 출력 창에 기록 된 메시지는 또한 패키지 새 알림을 &ndash; 앱이 포그라운드에서 실행 되는 동안 알림 아이콘이 알림 표시줄에 나타납니다.

[![전경 메시지에 대 한 알림 아이콘](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

알림을 열면 Firebase 콘솔 알림을 GUI에서 보낸 마지막 메시지를 표시 됩니다.

[![전경 아이콘과 함께 표시 하는 포그라운드 알림](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)


## <a name="disconnecting-from-fcm"></a>FCM에서 연결을 끊는 중

항목에서 구독을 취소 하려면 호출을 [UnsubscribeFromTopic](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29) 메서드를 [FirebaseMessaging](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging) 클래스입니다. 예를 들어, 구독을 취소 하는 _뉴스_ 항목에서는 이전에 구독을 **Unsubscribe** 다음 처리기 코드를 사용 하 여 레이아웃에 단추를 추가할 수 없습니다:

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

FCM 모두에서 장치 등록을 취소 하려면 호출 하 여 인스턴스 ID를 삭제 합니다 [DeleteInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29) 메서드는 [FirebaseInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) 클래스입니다. 예를 들어:

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

이 메서드 호출은 인스턴스 ID와 연결 된 데이터를 삭제 합니다. 결과적으로 정기 장치로 FCM 데이터를 보내는 중지 됩니다.


## <a name="troubleshooting"></a>문제 해결

다음 describe 문제 및 Xamarin.Android를 사용 하 여 Firebase Cloud Messaging을 사용 하는 경우 발생할 수 있는 해결 방법

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp 초기화 되지 않았습니다.

일부 경우에이 오류 메시지가 표시 될 수 있습니다.

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

솔루션을 정리 하 고 프로젝트를 다시 작성 하 여 해결할 수 있는 알려진된 문제입니다 (**빌드 > 솔루션 정리**하십시오 **빌드 > 솔루션 다시 빌드**).

## <a name="summary"></a>요약

이 연습에서는 Xamarin.Android 응용 프로그램에서 원격 알림을 Firebase Cloud Messaging을 구현 하기 위한 단계를 자세히 설명 합니다. FCM 통신에 필요한 필요한 패키지를 설치 하는 방법을 설명 하 고 Android 매니페스트 FCM 서버에 대 한 액세스를 구성 하는 방법을 설명 했습니다. Google Play 서비스의 존재 여부 확인 하는 방법을 보여 주는 코드 예를 제공 합니다. Fcm 등록 토큰에 대 한 협상 하는 인스턴스 ID 수신기 서비스를 구현 하는 방법을 보여 줍니다 및 앱은 backgrounded 하는 동안이 코드 백그라운드 알림을 만드는 방법을 설명 했습니다. 항목 메시지를 구독 하는 방법을 설명 했습니다 및 수신 하 고 앱이 포그라운드에서 실행 되는 동안 원격 알림을 표시 하는 데 사용 되는 메시지 수신기 서비스의 예제 구현을 제공 하지 않습니다.


## <a name="related-links"></a>관련 링크

- [FCMNotifications (샘플)](https://developer.xamarin.com/samples/monodroid/Firebase/FCMNotifications)
- [Firebase 클라우드 메시징](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [FCM 메시지 정보](https://firebase.google.com/docs/cloud-messaging/concept-options)
