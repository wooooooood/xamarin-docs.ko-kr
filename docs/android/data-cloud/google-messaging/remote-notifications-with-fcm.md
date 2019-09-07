---
title: Firebase 클라우드 메시징으로 원격 알림
description: 이 연습에서는 Firebase 클라우드 메시징을 사용 하 여 Xamarin Android 응용 프로그램에서 원격 알림 (푸시 알림)을 구현 하는 방법에 대 한 단계별 설명을 제공 합니다. FCM (Firebase Cloud Messaging)와 통신 하는 데 필요한 다양 한 클래스를 구현 하는 방법을 보여 주고, FCM에 대 한 액세스를 위해 Android 매니페스트를 구성 하는 방법에 대 한 예제를 제공 하 고, Firebase를 사용 하 여 다운스트림 메시징을 보여줍니다. 콘솔이.
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: c76b22c84851c8952dc4e9181966632cf6e38041
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754674"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>Firebase 클라우드 메시징으로 원격 알림

_이 연습에서는 Firebase 클라우드 메시징을 사용 하 여 Xamarin Android 응용 프로그램에서 원격 알림 (푸시 알림)을 구현 하는 방법에 대 한 단계별 설명을 제공 합니다. FCM (Firebase Cloud Messaging)와 통신 하는 데 필요한 다양 한 클래스를 구현 하는 방법을 보여 주고, FCM에 대 한 액세스를 위해 Android 매니페스트를 구성 하는 방법에 대 한 예제를 제공 하 고, Firebase를 사용 하 여 다운스트림 메시징을 보여줍니다. 콘솔이._

## <a name="fcm-notifications-overview"></a>FCM 알림 개요

이 연습에서는 FCM 메시징의 essentials를 설명 하기 위해 **Fcmclient** 라는 기본 앱이 만들어집니다. **Fcmclient** 는 Google Play 서비스 있는지 확인 하 고, FCM에서 등록 토큰을 수신 하 고, Firebase 콘솔에서 보내는 원격 알림을 표시 하 고, 토픽 메시지를 구독 합니다.

[![앱의 예제 스크린샷](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

다음 항목 영역을 탐색 합니다.

1. 백그라운드 알림

2. 토픽 메시지

3. 포그라운드 알림

이 연습을 수행 하는 동안 **Fcmclient** 에 기능을 증분 방식으로 추가 하 고 장치 또는 에뮬레이터에서 실행 하 여 FCM와 상호 작용 하는 방식을 이해 합니다. FCM 서버를 사용 하 여 라이브 앱 트랜잭션을 미러링 모니터 하는 데 로깅을 사용 하 고, Firebase Console Notification GUI에 입력 하는 FCM 메시지에서 알림이 생성 되는 방법을 관찰 합니다.

## <a name="requirements"></a>요구 사항

Firebase 클라우드 메시징에서 보낼 수 있는 [다양 한 유형의 메시지](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) 를 숙지 하는 것이 좋습니다. 메시지 페이로드에 따라 클라이언트 앱에서 메시지를 수신 하 고 처리 하는 방법이 결정 됩니다.

이 연습을 진행 하려면 먼저 Google의 FCM 서버를 사용 하는 데 필요한 자격 증명을 얻어야 합니다. 이 프로세스는 [Firebase 클라우드 메시징](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#setup_fcm)에 설명 되어 있습니다.
특히이 연습에서 제시 하는 예제 코드와 함께 사용할 **google 서비스의 json** 파일을 다운로드 해야 합니다. Firebase 콘솔에서 프로젝트를 아직 만들지 않은 경우 (또는 아직 **google 서비스** 파일을 다운로드 하지 않은 경우) [클라우드 메시징 Firebase](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)을 참조 하세요.

예제 앱을 실행 하려면 Firebase와 호환 Android 테스트 장치 또는 에뮬레이터가 필요 합니다. Firebase 클라우드 메시징은 Android 4.0 이상에서 실행 되는 클라이언트를 지원 하며, 이러한 장치에도 Google Play 스토어 앱이 설치 되어 있어야 합니다 (Google Play 서비스 9.2.1 이상이 필요 함). 장치에 Google Play 스토어 앱을 아직 설치 하지 않은 경우 [Google Play](https://support.google.com/googleplay) 웹 사이트를 방문 하 여 다운로드 하 고 설치 합니다. 또는 테스트 장치 대신 Google Play 서비스 설치 된 Android SDK 에뮬레이터를 사용할 수 있습니다. Android SDK 에뮬레이터를 사용 하는 경우 Google Play 스토어를 설치할 필요가 없습니다.

## <a name="start-an-app-project"></a>앱 프로젝트 시작

시작 하려면 **Fcmclient**라는 비어 있는 새 Xamarin Android 프로젝트를 만듭니다. Xamarin Android 프로젝트를 만드는 방법을 잘 모르는 경우 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)를 참조 하세요.
새 앱을 만든 후 다음 단계는 패키지 이름을 설정 하 고 FCM와 통신 하는 데 사용 되는 여러 NuGet 패키지를 설치 하는 것입니다.

### <a name="set-the-package-name"></a>패키지 이름 설정

[Firebase 클라우드 메시징](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)에서 FCM 사용 앱에 대 한 패키지 이름을 지정 했습니다. 이 패키지 이름은 [API 키](firebase-cloud-messaging.md#fcm-in-action-api-key)와 연결 된 [*응용 프로그램 ID*](./firebase-cloud-messaging.md#fcm-in-action-app-id) 로도 사용 됩니다. 이 패키지 이름을 사용 하도록 앱을 구성 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **Fcmclient** 프로젝트의 속성을 엽니다.

2. **Android 매니페스트** 페이지에서 패키지 이름을 설정 합니다.

다음 예에서는 패키지 이름이로 `com.xamarin.fcmexample`설정 됩니다.

[![패키지 이름 설정](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

**Android 매니페스트**를 업데이트 하는 동안 `Internet` 사용 권한이 설정 되어 있는지도 확인 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Fcmclient** 프로젝트의 속성을 엽니다.

2. **Android 응용 프로그램** 페이지에서 패키지 이름을 설정 합니다.

다음 예에서는 패키지 이름이로 `com.xamarin.fcmexample`설정 됩니다.

[![패키지 이름 설정](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

**Android 매니페스트**를 업데이트 하는 동안 `INTERNET` **필요한 권한**으로 사용 권한이 설정 되어 있는지도 확인 합니다.

-----

> [!IMPORTANT]
> 이 패키지 이름이 Firebase 콘솔에 입력 된 패키지 이름과 *정확 하 게* 일치 하지 않는 경우 클라이언트 앱은 FCM에서 등록 토큰을 받을 수 없게 됩니다.

### <a name="add-the-xamarin-google-play-services-base-package"></a>Xamarin Google Play 서비스 기본 패키지를 추가 합니다.

Firebase 클라우드 메시징은 Google Play 서비스에 따라 달라 지므로 xamarin [Google Play 서비스](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) 기반 NuGet 패키지를 xamarin.ios 프로젝트에 추가 해야 합니다. 버전 29.0.0.2 이상이 필요 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio에서 **참조 > NuGet 패키지 관리**...를 마우스 오른쪽 단추로 클릭 합니다.

2. **찾아보기** 탭을 클릭 하 고 **xamarin.googleplayservices.base**를 검색 합니다.

3. 이 패키지를 **Fcmclient** 프로젝트에 설치 합니다.

    [![Google Play 서비스 기반 설치](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac용 Visual Studio에서 패키지를 마우스 오른쪽 단추로 클릭 하 **> 패키지 추가**...를 클릭 합니다.

2. **Xamarin.googleplayservices.base**를 검색 합니다.

3. 이 패키지를 **Fcmclient** 프로젝트에 설치 합니다.

    [![Google Play 서비스 기반 설치](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

NuGet을 설치 하는 동안 오류가 발생 하는 경우 **Fcmclient** 프로젝트를 닫았다가 다시 열고 NuGet 설치를 다시 시도 합니다.

**Xamarin.googleplayservices.base**를 설치 하면 필요한 모든 종속성도 설치 됩니다. **MainActivity.cs** 를 편집 하 고 다음 `using` 문을 추가 합니다.

```csharp
using Android.Gms.Common;
```

이 문은 xamarin.googleplayservices.base의 `GoogleApiAvailability` 클래스를 **fcmclient** 코드에서 사용할 수 있도록 만듭니다.
`GoogleApiAvailability`Google Play 서비스 있는지 확인 하는 데 사용 됩니다.

### <a name="add-the-xamarin-firebase-messaging-package"></a>Xamarin Firebase Messaging 패키지 추가

FCM에서 메시지를 수신 하려면 [Xamarin Firebase Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet 패키지를 앱 프로젝트에 추가 해야 합니다. 이 패키지가 없으면 Android 응용 프로그램이 FCM 서버에서 메시지를 수신할 수 없습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio에서 **참조 > NuGet 패키지 관리**...를 마우스 오른쪽 단추로 클릭 합니다.

2. **Firebase**를 검색 합니다.

3. 이 패키지를 **Fcmclient** 프로젝트에 설치 합니다.

    [![Xamarin Firebase Messaging 설치](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac용 Visual Studio에서 패키지를 마우스 오른쪽 단추로 클릭 하 **> 패키지 추가**...를 클릭 합니다.

2. **Firebase**를 검색 합니다.

3. 이 패키지를 **Fcmclient** 프로젝트에 설치 합니다.

    [![Xamarin Firebase Messaging 설치](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----

**Firebase**를 설치 하면 필요한 모든 종속성도 설치 됩니다.

그런 다음 **MainActivity.cs** 를 편집 하 고 다음 `using` 문을 추가 합니다.

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

처음 두 문은 **Firebase** NuGet 패키지의 형식을 **fcmclient** 코드에서 사용할 수 있도록 합니다. **Android.** 는 FMS로 트랜잭션을 관찰 하는 데 사용 되는 로깅 기능을 추가 합니다.

### <a name="add-googleplayservices-json"></a>Google Services JSON 파일 추가

다음 단계는 프로젝트의 루트 디렉터리에 **google-service. json** 파일을 추가 하는 것입니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **Google-service. json** 을 프로젝트 폴더에 복사 합니다.

2. 앱 프로젝트에 **google-service. json** 을 추가 합니다 ( **솔루션 탐색기**의 **모든 파일 표시** 를 클릭 하 고 **google-service. json**을 마우스 오른쪽 단추로 클릭 한 다음 **프로젝트에 포함**을 선택).

3. **솔루션 탐색기** 창에서 **google-service json** 을 선택 합니다.

4. **속성** 창에서 **빌드 작업** 을 **GoogleServicesJson**로 설정 합니다.

    [![빌드 작업을 GoogleServicesJson로 설정](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)

    > [!NOTE] 
    > **GoogleServicesJson** 빌드 작업이 표시 되지 않으면 솔루션을 저장 하 고 닫은 다음 다시 엽니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Google-service. json** 을 프로젝트 폴더에 복사 합니다.

2. 응용 프로그램 프로젝트에 **google-service json** 을 추가 합니다.

3. **Google-service. json**을 마우스 오른쪽 단추로 클릭 합니다.

4. **빌드 작업** 을 **GoogleServicesJson**로 설정 합니다.

    [![빌드 작업을 GoogleServicesJson로 설정](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)

-----

**Google-** GoogleServicesJson가 프로젝트에 추가 되 고 (그리고 빌드 작업이 설정 된 경우) 빌드 프로세스는 클라이언트 ID와 [API 키](./firebase-cloud-messaging.md#fcm-in-action-api-key) 를 추출한 다음 병합/생성 **된에 이러한 자격 증명을 추가 합니다.**  **Obj/Debug/Android/androidmanifest**에 상주 하는 AndroidManifest .xml입니다. 이 병합 프로세스는 FCM 서버에 연결 하는 데 필요한 모든 권한 및 기타 FCM 요소를 자동으로 추가 합니다.

## <a name="check-for-google-play-services-and-create-a-notification-channel"></a>Google Play 서비스 확인 및 알림 채널 만들기

Google은 Google Play 서비스 기능에 액세스 하기 전에 Android 앱이 Google Play 서비스 APK가 있는지 확인 하는 것이 좋습니다. 자세한 내용은 [Google Play Services 확인](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)을 참조 하세요.

앱의 UI에 대 한 초기 레이아웃이 먼저 생성 됩니다. **리소스/레이아웃/기본. axml** 을 편집 하 고 내용을 다음 xml로 바꿉니다.

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

`TextView` Google Play 서비스 설치 여부를 나타내는 메시지를 표시 하는 데 사용 됩니다. **주. axml**에 변경 내용을 저장 합니다.

**MainActivity.cs** 를 편집 하 고 다음 인스턴스 변수를 `MainActivity` 클래스에 추가 합니다.

```csharp
public class MainActivity : AppCompatActivity
{
    static readonly string TAG = "MainActivity";

    internal static readonly string CHANNEL_ID = "my_notification_channel";
    internal static readonly int NOTIFICATION_ID = 100;

    TextView msgText;
```

및 `CHANNEL_ID` [`CreateNotificationChannel`](#create-notification-channel-code) `MainActivity` 변수는이 연습의 뒷부분에서에 추가 될 메서드에서 사용 됩니다. `NOTIFICATION_ID`

다음 예제에서 메서드는 `OnCreate` 앱이 FCM Services를 사용 하려고 시도 하기 전에 Google Play 서비스를 사용할 수 있는지 확인 합니다.
`MainActivity` 클래스에 다음 메서드를 추가 합니다.

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

이 코드는 장치에서 Google Play 서비스 APK가 설치 되어 있는지 확인 합니다. 설치 되지 않은 경우 사용자가 Google Play 스토어에서 apk를 다운로드 `TextBox` 하거나 장치의 시스템 설정에서 사용 하도록 지시 하는 메시지가에 표시 됩니다.

<a name="create-notification-channel-code"></a>Android 8.0 (API 레벨 26) 이상에서 실행 되는 앱은 알림을 게시 하기 위한 [_알림 채널_](~/android/app-fundamentals/notifications/local-notifications.md) 을 만들어야 합니다.  다음 메서드를 `MainActivity` 클래스에 추가 하 여 알림 채널을 만듭니다 (필요한 경우).

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

`IsPlayServicesAvailable`는의 `OnCreate` 끝에서 호출 되므로 앱이 시작 될 때마다 Google Play 서비스 검사가 실행 됩니다. Android 8 `CreateNotificationChannel` 이상을 실행 하는 장치에 대 한 알림 채널이 있는지 확인 하기 위해 메서드가 호출 됩니다. 앱 `OnResume` 에 메서드가 있으면에서를 `OnResume` 호출 `IsPlayServicesAvailable` 해야 합니다. 앱을 완전히 다시 빌드하고 실행 합니다. 모든 항목이 올바르게 구성 된 경우 다음과 같은 화면이 표시 됩니다.

[![앱은 Google Play 서비스를 사용할 수 있음을 나타냅니다.](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

이 결과를 얻지 못한 경우 Google Play 서비스 APK가 장치에 설치 되어 있는지 확인 합니다 (자세한 내용은 [Google Play 서비스 설정](https://developers.google.com/android/guides/setup)참조).
또한 앞에서 설명한 대로 **Fcmclient** 프로젝트에 **xamarin.ios** 패키지를 추가 했는지 확인 합니다.

## <a name="add-the-instance-id-receiver"></a>인스턴스 ID 수신기 추가

다음 단계는를 확장 `FirebaseInstanceIdService` 하는 서비스를 추가 하 여 [Firebase 등록 토큰](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token)의 생성, 회전 및 업데이트를 처리 하는 것입니다. FCM가 장치에 메시지를 보낼 수 있으려면 서비스가필요합니다.`FirebaseInstanceIdService` `FirebaseInstanceIdService` 서비스가 클라이언트 앱에 추가 되 면 앱은 자동으로 FCM 메시지를 수신 하 고 앱이 backgrounded 될 때마다 알림으로 표시 합니다.

### <a name="declare-the-receiver-in-the-android-manifest"></a>Android 매니페스트에서 수신기를 선언 합니다.

**Androidmanifest .xml** 을 편집 하 고 `<receiver>` `<application>` 섹션에 다음 요소를 삽입 합니다.

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

- 각 응용 프로그램 인스턴스에 대 한 [고유 식별자](https://developers.google.com/instance-id/) 를 제공 하는 구현을선언합니다.`FirebaseInstanceIdReceiver` 또한이 수신자 인증 하 고 작업에 권한을 부여 합니다.

- 내부 선언 `FirebaseInstanceIdInternalReceiver` 서비스를 안전 하 게 시작 하는 데 사용 되는 구현 합니다.

- [앱 ID](./firebase-cloud-messaging.md#fcm-in-action-app-id) 는 [프로젝트에 추가](#add-googleplayservices-json)된 **google 서비스의 json** 파일에 저장 됩니다. Xamarin Android Firebase 바인딩은 토큰 `${applicationId}` 을 앱 id로 대체 합니다. 클라이언트 앱이 앱 id를 제공 하는 데 추가 코드는 필요 하지 않습니다.

`FirebaseInstanceIdReceiver` `FirebaseInstanceId` 는및`FirebaseInstanceIdService`이벤트를받아 파생 된 클래스에 전달 하는입니다.`WakefulBroadcastReceiver` `FirebaseMessaging`

### <a name="implement-the-firebase-instance-id-service"></a>Firebase Instance ID 서비스 구현

FCM를 사용 하 여 응용 프로그램을 등록 하는 작업은 `FirebaseInstanceIdService` 사용자가 제공 하는 사용자 지정 서비스에 의해 처리 됩니다.
`FirebaseInstanceIdService`다음 단계를 수행 합니다.

1. 는 [인스턴스 ID API](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) 를 사용 하 여 클라이언트 앱이 FCM 및 앱 서버에 액세스할 수 있는 권한을 부여 하는 보안 토큰을 생성 합니다. 반환 시 앱은 FCM에서 [등록 토큰](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token) 을 다시 가져옵니다.

2. 앱 서버에 필요한 경우 등록 토큰을 앱 서버에 전달 합니다.

**MyFirebaseIIDService.cs** 라는 새 파일을 추가 하 고 해당 템플릿 코드를 다음으로 바꿉니다.

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

이 서비스는 등록 `OnTokenRefresh` 토큰을 처음 생성 하거나 변경할 때 호출 되는 메서드를 구현 합니다. 가 `OnTokenRefresh` 실행 될 때 `FirebaseInstanceId.Instance.Token` 속성에서 최신 토큰을 검색 합니다 (FCM에 의해 비동기적으로 업데이트 됨). 이 예제에서는 출력 창에서 볼 수 있도록 새로 고쳐진 토큰이 기록 됩니다.

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh`는 자주 호출 되지 않습니다. 다음과 같은 상황에서 토큰을 업데이트 하는 데 사용 됩니다.

- 앱을 설치 하거나 제거 하는 경우

- 사용자가 앱 데이터를 삭제 하는 경우

- 앱에서 인스턴스 ID를 지우는 경우

- 토큰 보안이 손상 된 경우

Google의 [인스턴스 id](https://developers.google.com/instance-id/guides/android-implementation) 설명서에 따라 FCM instance id 서비스는 앱이 토큰을 주기적으로 새로 고치도록 요청 합니다 (일반적으로 6 개월 마다).

`OnTokenRefresh`또한를 `SendRegistrationToAppServer` 호출 하 여 사용자의 등록 토큰을 응용 프로그램에서 유지 관리 되는 서버 쪽 계정 (있는 경우)에 연결 합니다.

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

이 구현은 앱 서버 디자인에 따라 달라 지므로이 예제에서는 빈 메서드 본문을 제공 합니다. 앱 서버에서 FCM 등록 정보를 요구 하는 `SendRegistrationToAppServer` 경우를 수정 하 여 사용자의 FCM 인스턴스 ID 토큰을 앱에 의해 유지 관리 되는 서버 쪽 계정에 연결 합니다. 토큰은 클라이언트 앱에 불투명 합니다.

토큰이 앱 서버 `SendRegistrationToAppServer` 에 전송 되 면는 토큰을 서버에 보냈는지 여부를 나타내는 부울을 유지 해야 합니다. 이 부울이 false 이면에서 `SendRegistrationToAppServer` 앱 서버로 &ndash; 토큰을 보내고, 그렇지 않으면 이전 호출에서 토큰을 이미 앱 서버에 보냈습니다. 예를 들어이 `FCMClient` 예제와 같은 경우에는 앱 서버에 토큰이 필요 하지 않으므로이 예제에서는이 메서드가 필요 하지 않습니다.

## <a name="implement-client-app-code"></a>클라이언트 앱 코드 구현

이제 수신기 서비스가 준비 되었으므로 이러한 서비스를 활용 하도록 클라이언트 앱 코드를 작성할 수 있습니다. 다음 섹션에서는 등록 토큰 ( *인스턴스 ID 토큰*이 라고도 함)을 기록 하는 단추를 UI에 추가 하 고, 알림에서 앱이 시작 될 때 정보 `MainActivity` 를 보기 `Intent` 위해 추가 코드를 추가 합니다.

[![앱 화면에 추가 된 로그 토큰 단추](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokens"></a>로그 토큰

이 단계에서 추가 된 코드는 데모 목적 &ndash; 으로만 사용 됩니다. 프로덕션 클라이언트 앱은 등록 토큰을 기록할 필요가 없습니다. **리소스/레이아웃/기본. axml** 을 편집 하 고 `Button` `TextView` 요소 바로 뒤에 다음 선언을 추가 합니다.

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

이 코드는 **로그 토큰** 단추를 누를 때 출력 창에 현재 토큰을 기록 합니다.

### <a name="handle-notification-intents"></a>알림 의도 처리

사용자가 **fcmclient**에서 발급 된 알림을 탭 하면 해당 알림 메시지와 함께 제공 되는 모든 데이터는 `Intent` 특별 한 기능으로 제공 됩니다. **MainActivity.cs** 를 편집 하 고를 `OnCreate` `IsPlayServicesAvailable`호출 하기 전에 다음 코드를 메서드의 맨 위에 추가 합니다.

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

앱의 시작 관리자 `Intent` 는 사용자가 알림 메시지를 탭 할 때 발생 하므로,이 코드는에 있는 모든 데이터 `Intent` 를 출력 창에 기록 합니다. 다른 `Intent` 가 발생 `click_action` 해야 하는 경우 알림 메시지의 필드를로 `Intent` 설정 해야 합니다 .를 지정 하지 `click_action` 않으면 `Intent` 시작 관리자가 사용 됩니다.

## <a name="background-notifications"></a>백그라운드 알림

**Fcmclient** 앱을 빌드하고 실행 합니다. **로그 토큰** 단추가 표시 됩니다.

[![로그 토큰 단추가 표시 됩니다.](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

**로그 토큰** 단추를 누릅니다. IDE 출력 창에 다음과 같은 메시지가 표시 되어야 합니다.

[![출력 창에 표시 되는 인스턴스 ID 토큰](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

**토큰** 으로 레이블이 지정 된 긴 문자열은 Firebase 콘솔 &ndash; 에 붙여넣을 인스턴스 ID 토큰입니다 .를 선택 하 고이 문자열을 클립보드에 복사 합니다. 인스턴스 ID 토큰이 표시 되지 않는 경우 `OnCreate` 메서드 맨 위에 다음 줄을 추가 하 여 **google-json** 이 올바르게 구문 분석 되었는지 확인 합니다.

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

출력 `google_app_id` 창에 기록 되는 값은 **google-service. json**에 기록 된 `mobilesdk_app_id` 값과 일치 해야 합니다.

### <a name="send-a-message"></a>메시지 보내기

[Firebase 콘솔](https://console.firebase.google.com)에 로그인 하 고, 프로젝트를 선택 하 고, **알림**을 클릭 하 고, **첫 번째 메시지 보내기**를 클릭 합니다.

[![첫 번째 메시지 보내기 단추](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

**메시지 작성** 페이지에서 메시지 텍스트를 입력 하 고 **단일 장치**를 선택 합니다. IDE 출력 창에서 인스턴스 ID 토큰을 복사 하 여 Firebase 콘솔의 **FCM 등록 토큰** 필드에 붙여넣습니다.

[![메시지 작성 대화 상자](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Android 장치 (또는 에뮬레이터)에서 Android **개요** 단추를 탭 하 고 홈 화면을 터치 하 여 앱을 배경으로 합니다. 장치가 준비 되 면 Firebase 콘솔에서 **메시지 보내기** 를 클릭 합니다.

[![메시지 보내기 단추](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

**메시지 검토** 대화 상자가 표시 되 면 **보내기**를 클릭 합니다.
알림 아이콘은 장치 또는 에뮬레이터의 알림 영역에 표시 되어야 합니다.

[![알림 아이콘이 표시 됨](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

알림 아이콘을 열어 메시지를 표시 합니다. 알림 메시지는 Firebase 콘솔의 **메시지 텍스트** 필드에 입력 한 것과 정확히 일치 해야 합니다.

[![알림 메시지가 장치에 표시 됩니다.](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

알림 아이콘을 탭 하 여 **Fcmclient** 앱을 시작 합니다. Fcmclient로 전송 되는 추가 기능은 IDE 출력 창에 나열 됩니다. `Intent`

[![키, 메시지 ID 및 축소 키의 목적 목록](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

이 예제에서 **from** 키는 앱의 Firebase 프로젝트 번호 (이 예제 `41590732`에서는)로 설정 되 고 collapse_key는 패키지 이름 ( )으로 설정**됩니다.**
메시지가 수신 되지 않으면 장치 (또는 에뮬레이터)에서 **Fcmclient** 앱을 삭제 하 고 위의 단계를 반복 합니다.

> [!NOTE]
> 앱을 강제로 닫으면 FCM에서 알림 배달이 중지 됩니다. Android는 백그라운드 서비스 브로드캐스트가 실수로 중지 된 응용 프로그램의 구성 요소를 시작 하거나 불필요 하 게 시작 하지 않도록 합니다. 이 동작에 대 한 자세한 내용은 중지 된 [응용 프로그램에서 컨트롤 시작](https://developer.android.com/about/versions/android-3.1.html#launchcontrols)을 참조 하세요. 이러한 이유로 응용 프로그램을 실행 하 고 디버그 세션 &ndash; 에서 중지할 때마다 수동으로 제거 해야 합니다. 이렇게 하면 FCM는 메시지를 계속 받을 수 있도록 새 토큰을 생성 합니다.

### <a name="add-a-custom-default-notification-icon"></a>사용자 지정 기본 알림 아이콘 추가

이전 예제에서는 알림 아이콘이 응용 프로그램 아이콘으로 설정 되어 있습니다. 다음 XML은 알림에 대 한 사용자 지정 기본 아이콘을 구성 합니다. Android에는 알림 아이콘이 명시적으로 설정 되지 않은 모든 알림 메시지에 대 한이 사용자 지정 기본 아이콘이 표시 됩니다.

사용자 지정 기본 알림 아이콘을 추가 하려면 **리소스/그릴** 수 있는 디렉터리에 아이콘을 추가 하 고 **androidmanifest .xml**을 편집 하 고 `<meta-data>` `<application>` 섹션에 다음 요소를 삽입 합니다.

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

이 예제에서는 **리소스/그릴 수 있는/ic\_stat\_\_ic 알림** 에 있는 알림 아이콘을 사용자 지정 기본 알림 아이콘으로 사용 합니다. 사용자 지정 기본 아이콘이 **Androidmanifest** 에 구성 되어 있지 않고 알림 페이로드에 설정 된 아이콘이 없는 경우 Android는 위의 알림 아이콘 스크린샷에 표시 된 것 처럼 응용 프로그램 아이콘을 알림 아이콘으로 사용 합니다.

## <a name="handle-topic-messages"></a>항목 메시지 처리

따라서 지금까지 작성 된 코드는 등록 토큰을 처리 하 고 앱에 원격 알림 기능을 추가 합니다. 다음 예제에서는 *토픽 메시지* 를 수신 하 고이를 원격 알림으로 사용자에 게 전달 하는 코드를 추가 합니다. 토픽 메시지는 특정 항목을 구독 하는 하나 이상의 장치에 전송 되는 FCM 메시지입니다. 토픽 메시지에 대 한 자세한 내용은 [항목 메시징](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)을 참조 하세요.

### <a name="subscribe-to-a-topic"></a>토픽 구독

**리소스/레이아웃/기본. axml** 을 편집 하 고 다음 `Button` 선언을 이전 `Button` 요소 바로 뒤에 추가 합니다.

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

이 XML은 레이아웃에 **알림 구독** 단추를 추가 합니다.
**MainActivity.cs** 를 편집 하 고 `OnCreate` 메서드의 끝에 다음 코드를 추가 합니다.

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

이 코드는 레이아웃에서 **알림 구독** 단추를 찾고 등록 된 항목인 _news_를 전달 하 여를 호출 `FirebaseMessaging.Instance.SubscribeToTopic`하는 코드에 클릭 처리기를 할당 합니다. 사용자가 **구독** 단추를 누르면 앱이 _news_ 항목을 구독 합니다. 다음 섹션에서는 Firebase Console Notification GUI에서 _news_ 토픽 메시지가 전송 됩니다.

### <a name="send-a-topic-message"></a>토픽 메시지 보내기

앱을 제거 하 고 다시 빌드한 다음 다시 실행 합니다. **알림 구독** 단추를 클릭 합니다.

[![알림 구독 단추](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

앱이 성공적으로 구독 되 면 IDE 출력 창에 **동기화 성공** 이 표시 됩니다.

[![출력 창에 항목 동기화 성공 메시지가 표시 됨](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

토픽 메시지를 보내려면 다음 단계를 사용 합니다.

1. Firebase 콘솔에서 **새 메시지**를 클릭 합니다.

2. **메시지 작성** 페이지에서 메시지 텍스트를 입력 하 고 **항목**을 선택 합니다.

3. **항목** 풀 다운 메뉴에서 기본 제공 항목인 **news**:를 선택 합니다.

    [![뉴스 항목 선택](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4. Android 장치 (또는 에뮬레이터)에서 Android **개요** 단추를 탭 하 고 홈 화면을 터치 하 여 앱을 배경으로 합니다.

5. 장치가 준비 되 면 Firebase 콘솔에서 **메시지 보내기** 를 클릭 합니다.

6. 로그 출력에서 **/항목/뉴스** 를 보려면 IDE 출력 창을 확인 합니다.

    [![/Topic/news의 메시지가 표시 됩니다.](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

이 메시지가 출력 창에 표시 되 면 알림 아이콘이 Android 장치의 알림 영역에도 표시 되어야 합니다. 알림 아이콘을 열어 항목 메시지를 확인 합니다.

[![토픽 메시지가 알림으로 표시 됩니다.](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

메시지가 수신 되지 않으면 장치 (또는 에뮬레이터)에서 **Fcmclient** 앱을 삭제 하 고 위의 단계를 반복 합니다.

## <a name="foreground-notifications"></a>포그라운드 알림

포그라운드 apps에서 알림을 받으려면를 구현 `FirebaseMessagingService`해야 합니다. 이 서비스는 데이터 페이로드를 받고 업스트림 메시지를 전송 하는 데에도 필요 합니다. 다음 예제에서는 결과 앱을 확장 `FirebaseMessagingService` &ndash; 하는 서비스를 구현 하는 방법을 보여 줍니다 .이는 포그라운드에서 실행 되는 동안 원격 알림을 처리할 수 있습니다.

### <a name="implement-firebasemessagingservice"></a>FirebaseMessagingService 구현

`FirebaseMessagingService` 서비스는 Firebase에서 메시지를 받고 처리 하는 작업을 담당 합니다. 각 앱은 `OnMessageReceived` 이 형식을 서브클래싱하 고를 재정의 하 여 들어오는 메시지를 처리 해야 합니다. 응용 프로그램이 전경에 `OnMessageReceived` 있으면 콜백이 항상 메시지를 처리 합니다.

> [!NOTE]
> 앱에는 들어오는 Firebase 클라우드 메시지를 처리 하는 10 초가 있습니다. 이 보다 오래 걸리는 작업은 [Android 작업 Scheduler](~/android/platform/android-job-scheduler.md) 또는 [Firebase 작업 디스패처](~/android/platform/firebase-job-dispatcher.md)와 같은 라이브러리를 사용 하 여 백그라운드 실행을 예약 해야 합니다.

**MyFirebaseMessagingService.cs** 라는 새 파일을 추가 하 고 해당 템플릿 코드를 다음으로 바꿉니다.

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

새 FCM 메시지가 `MyFirebaseMessagingService`전달 되도록 의도필터를선언해야합니다.`MESSAGING_EVENT`

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

클라이언트 앱이 FCM에서 메시지를 받으면는 해당 `OnMessageReceived` `GetNotification` 메서드를 호출 하 여 전달 `RemoteMessage` 된 개체에서 메시지 콘텐츠를 추출 합니다. 그런 다음 IDE 출력 창에서 볼 수 있도록 메시지 콘텐츠를 기록 합니다.

```csharp
var body = message.GetNotification().Body;
Log.Debug(TAG, "Notification Message Body: " + body);
```

> [!NOTE]
> 에서 중단점을 설정 하 `FirebaseMessagingService`는 경우 FCM에서 메시지를 배달 하는 방법 때문에 디버깅 세션이 이러한 중단점에 도달 하거나 적중 하지 않을 수 있습니다.

### <a name="send-another-message"></a>다른 메시지 보내기

앱을 제거 하 고 다시 빌드하고 다시 실행 한 후 다음 단계에 따라 다른 메시지를 보냅니다.

1. Firebase 콘솔에서 **새 메시지**를 클릭 합니다.

2. **메시지 작성** 페이지에서 메시지 텍스트를 입력 하 고 **단일 장치**를 선택 합니다.

3. IDE 출력 창에서 토큰 문자열을 복사 하 고 이전과 같이 Firebase 콘솔의 **FCM 등록 토큰** 필드에 붙여넣습니다.

4. 응용 프로그램이 포그라운드에서 실행 중인지 확인 하 고 Firebase 콘솔에서 **메시지 보내기** 를 클릭 합니다.

    [![콘솔에서 다른 메시지 보내기](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5. **메시지 검토** 대화 상자가 표시 되 면 **보내기**를 클릭 합니다.

6. 들어오는 메시지는 IDE 출력 창에 기록 됩니다.

    [![출력 창에 인쇄 되는 메시지 본문](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)

### <a name="add-a-local-notification-sender"></a>로컬 알림 보낸 사람 추가

이 나머지 예제에서 들어오는 FCM 메시지는 응용 프로그램이 포그라운드에서 실행 되는 동안 시작 되는 로컬 알림으로 변환 됩니다. **MyFirebaseMessageService.cs** 를 편집 하 고 다음 `using` 문을 추가 합니다.

```csharp
using FCMClient;
using System.Collections.Generic;
```

`MyFirebaseMessagingService`에 다음 메서드를 추가합니다.

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

이 알림을 백그라운드 알림과 구분 하기 위해이 코드는 응용 프로그램 아이콘과 다른 아이콘을 사용 하 여 알림을 표시 합니다. [Ic\_stat\_icnotification\_.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) 파일을 **리소스/그릴** 수 있는 파일에 추가 하 고 **fcmclient** 프로젝트에 포함 합니다.

메서드 `SendNotification` 는를 `NotificationCompat.Builder` 사용 하 여 알림을 `NotificationManagerCompat` 만들며 알림을 시작 하는 데 사용 됩니다. 알림은 사용자가 앱 `PendingIntent` 을 열고에 `messageBody`전달 된 문자열의 내용을 볼 수 있도록 하는을 포함 합니다. 에 대 한 `NotificationCompat.Builder`자세한 내용은 [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)을 참조 하세요.

메서드`OnMessageReceived` 끝 `SendNotification` 에서 메서드를 호출 합니다.

```csharp
public override void OnMessageReceived(RemoteMessage message)
{
    Log.Debug(TAG, "From: " + message.From);

    var body = message.GetNotification().Body;
    Log.Debug(TAG, "Notification Message Body: " + body);
    SendNotification(body, message.Data);
}
```

이러한 변경 `SendNotification` 의 결과로 앱이 전경에 있는 동안 알림이 수신 될 때마다가 실행 되 고 알림 영역에 알림이 표시 됩니다.

앱이 백그라운드에 있으면 메시지 [의 페이로드](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) 는 메시지 처리 방법을 결정 합니다.

- **알림** 메시지는 시스템 트레이에 전송 됩니다. &ndash; 로컬 알림이 표시 됩니다. 사용자가 알림을 탭 하면 앱이 시작 됩니다.
- **데이터** 메시지는에 의해 `OnMessageReceived`처리 됩니다. &ndash;
- **모두** &ndash; 알림과 데이터 페이로드가 모두 있는 메시지는 시스템 트레이에 배달 됩니다. 앱이 시작 되 면 데이터 페이로드가 앱을 시작 하는 `Extras` 데 사용 `Intent` 된의에 표시 됩니다.

이 예제에서 앱이 backgrounded `SendNotification` 인 경우 메시지에 데이터 페이로드가 있으면이 실행 됩니다. 그렇지 않으면이 연습의 앞부분에서 설명한 백그라운드 알림이 시작 됩니다.

### <a name="send-the-last-message"></a>마지막 메시지 보내기

앱을 제거 하 고 다시 빌드하고 다시 실행 한 후 다음 단계를 사용 하 여 마지막 메시지를 보냅니다.

1. Firebase 콘솔에서 **새 메시지**를 클릭 합니다.

2. **메시지 작성** 페이지에서 메시지 텍스트를 입력 하 고 **단일 장치**를 선택 합니다.

3. IDE 출력 창에서 토큰 문자열을 복사 하 고 이전과 같이 Firebase 콘솔의 **FCM 등록 토큰** 필드에 붙여넣습니다.

4. 응용 프로그램이 포그라운드에서 실행 중인지 확인 하 고 Firebase 콘솔에서 **메시지 보내기** 를 클릭 합니다.

    [![전경 메시지 보내기](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

이번에는 출력 창에 기록 된 메시지도 새 알림으로 &ndash; 패키지 됩니다. 알림 아이콘은 응용 프로그램이 포그라운드에서 실행 되는 동안 알림 트레이에 표시 됩니다.

[![전경 메시지의 알림 아이콘](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

알림을 열면 Firebase Console Notification GUI에서 마지막으로 보낸 메시지가 표시 됩니다.

[![전경 알림이 전경 아이콘으로 표시 됨](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)

## <a name="disconnecting-from-fcm"></a>FCM에서 연결 끊기

토픽에서 구독을 취소 하려면 [FirebaseMessaging](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging) 클래스에서 [UnsubscribeFromTopic](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29) 메서드를 호출 합니다. 예를 들어 이전에 구독 한 _뉴스_ 항목에서 구독을 취소 하려면 다음 처리기 코드를 사용 하 여 **구독 취소** 단추를 레이아웃에 추가할 수 있습니다.

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

FCM에서 장치 등록을 취소 하려면 [FirebaseInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) 클래스에서 [deleteinstanceid](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29) 메서드를 호출 하 여 인스턴스 ID를 삭제 합니다. 예:

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

이 메서드 호출은 인스턴스 ID와 연결 된 데이터를 삭제 합니다. 결과적으로, 장치에 대 한 FCM 데이터의 주기적인 보내기가 중단 됩니다.

## <a name="troubleshooting"></a>문제 해결

다음은 Xamarin. Android에서 Firebase 클라우드 메시징을 사용 하는 경우 발생할 수 있는 문제 및 해결 방법에 대 한 설명입니다.

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp가 초기화 되지 않았습니다.

일부 경우에 다음과 같은 오류 메시지가 표시 될 수 있습니다.

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

이 문제는 솔루션을 정리 하 고 프로젝트를 다시 빌드하는 방법으로 해결할 수 있는 알려진 문제입니다 (**빌드 > 정리 솔루션**, **빌드 > 솔루션**작성).

## <a name="summary"></a>요약

이 연습에서는 Xamarin.ios 응용 프로그램에서 Firebase 클라우드 메시징 원격 알림을 구현 하는 단계를 자세히 설명 합니다. FCM 통신에 필요한 필수 패키지를 설치 하는 방법에 대해 설명 하 고 FCM 서버에 액세스 하기 위해 Android 매니페스트를 구성 하는 방법을 설명 했습니다. Google Play 서비스 있는지 확인 하는 방법을 보여 주는 예제 코드를 제공 했습니다. 등록 토큰에 대해 FCM를 사용 하 여 협상 하는 인스턴스 ID 수신기 서비스를 구현 하는 방법과이 코드가 앱을 backgrounded 하는 동안 백그라운드 알림을 만드는 방법을 설명 했습니다. 토픽 메시지를 구독 하는 방법에 대해 설명 하 고, 응용 프로그램이 포그라운드에서 실행 되는 동안 원격 알림을 수신 하 고 표시 하는 데 사용 되는 메시지 수신기 서비스의 예제 구현을 제공 했습니다.

## <a name="related-links"></a>관련 링크

- [FCMNotifications (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/firebase-fcmnotifications)
- [Firebase 클라우드 메시징](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [FCM 메시지 정보](https://firebase.google.com/docs/cloud-messaging/concept-options)
