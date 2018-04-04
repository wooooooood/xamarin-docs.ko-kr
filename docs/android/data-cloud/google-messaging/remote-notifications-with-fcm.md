---
title: Firebase 사용 하 여 원격 알림을 클라우드 메시징
description: 이 연습에서는 Xamarin.Android 응용 프로그램에서 푸시 알림을 라고도 하는 원격 알림을 구현 하려면 Firebase Cloud Messaging을 사용 하는 방법에 대 한 단계별 설명은 제공 합니다. Firebase 클라우드 메시징 (FCM)와 통신에 필요한 Android 매니페스트 FCM에 대 한 액세스를 구성 하는 방법의 예제를 제공 하 고 다운스트림 메시징는 Firebase를 사용 하 여 보여 줍니다..는 다양 한 클래스를 구현 하는 방법을 보여 줍니다. 콘솔입니다.
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: c6e1d36d871b4bb41a1e53d6e58ba8940813b29f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>Firebase 사용 하 여 원격 알림을 클라우드 메시징

_이 연습에서는 Xamarin.Android 응용 프로그램에서 푸시 알림을 라고도 하는 원격 알림을 구현 하려면 Firebase Cloud Messaging을 사용 하는 방법에 대 한 단계별 설명은 제공 합니다. Firebase 클라우드 메시징 (FCM)와 통신에 필요한 Android 매니페스트 FCM에 대 한 액세스를 구성 하는 방법의 예제를 제공 하 고 다운스트림 메시징는 Firebase를 사용 하 여 보여 줍니다..는 다양 한 클래스를 구현 하는 방법을 보여 줍니다. 콘솔입니다._

## <a name="fcm-notifications-overview"></a>FCM 알림 개요

이 연습에서는 기본 응용 프로그램 호출 **FCMClient** FCM 메시징 기능을 설명 하기 위해 생성 됩니다. **FCMClient** Google Play 서비스 여부를 확인, FCM에서 등록 토큰을 받고를 Firebase 콘솔에서 보내는 원격 알림에 표시 및 항목 메시지에 등록 합니다.

[![응용 프로그램의 예제 스크린 샷](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

다음 항목 영역에 대해 알아봅니다.

1.  배경 알림이

2.  항목 메시지

3.  전경 알림

이 연습을 통해 증분 방식으로 추가한 기능을 **FCMClient** 장치 또는 FCM와 상호 작용 하는 방법을 이해 하는 에뮬레이터에서 실행 합니다. 로깅을 사용 하 여 라이브 앱 트랜잭션을 FCM 서버와 미러링 모니터 서버 및 알림 Firebase 콘솔 알림 GUI에 입력 하는 FCM 메시지에서 생성 되는 방법을 볼 수 있습니다. 

## <a name="requirements"></a>요구 사항

Google의 FCM 서버;를 사용 하는 데 필요한 자격 증명을 획득 해야이 연습을 진행할 수 있습니다, 전에 이 과정에 설명 되어 [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)합니다. 특히, 다운로드 해야는 **google services.json** 이 연습에 나와 있는 예제 코드에 사용할 파일을 합니다. 경우 있습니다 아직 만들지 않은 프로젝트 Firebase 콘솔에서 (아직 다운로드 하지 않은 경우 또는 **google services.json** 파일), 참조 [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)합니다. 

예제 앱을 실행 하려면 테스트 Android 장치 또는 에뮬레이터 사항은 Firebase와 호환 되는 해야 합니다. 이상에서 실행 중인 Android 2.3 (인형), 클라이언트를 지 원하는 firebase 클라우드 메시징 및 있어야 이러한 장치에서 Google Play 스토어 앱이 설치 된 (Google Play 서비스 9.2.1 또는 이상 버전이 필요). Google Play 스토어 앱이 장치에 설치 된 아직 없는 경우 방문는 [Google Play](https://support.google.com/googleplay) 웹 사이트를 다운로드 하 고 설치 합니다. 또는 (않아도 Android SDK 에뮬레이터를 사용 하는 경우 Google Play 스토어를 설치 하려면) 테스트 장치 대신 2.3 이상 Android를 실행 하는 Android SDK 에뮬레이터를 사용할 수 있습니다. 

## <a name="start-an-app-project"></a>응용 프로그램 프로젝트를 시작 합니다.

라는 새 빈 Xamarin.Android 프로젝트 만들기를 시작 하려면 **FCMClient**합니다. Xamarin.Android 프로젝트 만들기에 익숙하지 않은 경우 참조 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)합니다. 새 앱을 만든 후 패키지 이름을 설정 하 고 FCM와의 통신에 사용할 수 있는 몇 가지 NuGet 패키지를 설치 하는 다음 단계가입니다. 

### <a name="set-the-package-name"></a>패키지 이름 설정

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md), FCM 사용이 가능한 앱에 대 한 패키지 이름을 지정 합니다. 이 패키지 이름으로도 사용 된 *응용 프로그램 ID* API 키와 연결 된입니다. 이 패키지 이름을 사용 하 여 응용 프로그램을 구성 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  에 대 한 속성을 열고는 **FCMClient** 프로젝트. 

2.  에 **Android 매니페스트** 페이지에서 패키지 이름을 설정 합니다. 

다음 예제에서는 패키지 이름 설정 되어 `com.xamarin.fcmexample`: 

[![패키지 이름 설정](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

업데이트 하는 동안는 **Android 매니페스트**, 선택 사항을 확인 해야는 `Internet` 권한 사용 합니다. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  에 대 한 속성을 열고는 **FCMClient** 프로젝트. 

2.  에 **Android 응용 프로그램** 페이지에서 패키지 이름을 설정 합니다. 

다음 예제에서는 패키지 이름 설정 되어 `com.xamarin.fcmexample`: 

[![패키지 이름 설정](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

업데이트 하는 동안는 **Android 매니페스트**, 선택 사항을 확인 해야는 `INTERNET` 권한 사용 (아래 **필요한 권한**). 

-----

클라이언트 응용 프로그램을 패키지 이름이이 가리키지 않으면 FCM에서 등록 토큰을 받을 수 됩니다는 *정확히* Firebase 콘솔에 입력 한 패키지 이름과 일치 합니다. 

### <a name="add-the-xamarin-google-play-services-base-package"></a>Xamarin Google Play 서비스 기본 패키지를 추가 합니다.

Google Play 서비스에 따라 달라 Firebase Cloud Messaging는 [Xamarin Google Play 서비스-자료](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) Xamarin.Android 프로젝트에 NuGet 패키지를 추가 해야 합니다. 버전 29.0.0.2 필요 합니다 이상.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Visual Studio에서 마우스 오른쪽 단추로 클릭 **참조 > NuGet 패키지 관리...** . 

2.  클릭는 **찾아보기** 탭 및 검색할 **Xamarin.GooglePlayServices.Base**합니다. 

3.  이 패키지에 설치 된 **FCMClient** 프로젝트: 

    [![Google Play 서비스 자료를 설치합니다.](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가 중...** . 

2.  검색할 **Xamarin.GooglePlayServices.Base**합니다. 

3.  이 패키지에 설치 된 **FCMClient** 프로젝트: 

    [![Google Play 서비스 자료를 설치합니다.](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

NuGet 설치 하는 동안 오류가 발생 하면 닫습니다는 **FCMClient** 프로젝트를 다시 열고 마우스 NuGet 설치를 다시 시도 합니다. 

설치 하는 경우 **Xamarin.GooglePlayServices.Base**, 모든 필요한 종속성도 설치 됩니다. 편집 **MainActivity.cs** 다음 추가 `using` 문: 

```csharp
using Android.Gms.Common;
```

이 문을 사용 하면는 `GoogleApiAvailability` 클래스 **Xamarin.GooglePlayServices.Base** 를 사용할 수 있는 **FCMClient** 코드입니다. 
`GoogleApiAvailability` Google Play 서비스 있는지 확인 하는 데 사용 됩니다. 

### <a name="add-the-xamarin-firebase-messaging-package"></a>패키지를 메시징 Xamarin Firebase 추가

FCM에서 메시지를 받을 [Xamarin Firebase-메시징](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet 패키지를 응용 프로그램 프로젝트에 추가 해야 합니다. 이 패키지 없이 Android 응용 프로그램 FCM 서버에서 메시지를 받을 수 없습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Visual Studio에서 마우스 오른쪽 단추로 클릭 **참조 > NuGet 패키지 관리...** . 

2.  확인 **시험판 포함** 검색 한 **Xamarin.Firebase.Messaging**합니다. 

3.  이 패키지에 설치 된 **FCMClient** 프로젝트: 

    [![Xamarin Firebase 메시징 설치](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가 중...** . 

2.  확인 **시험판 패키지를 표시** 검색 한 **Xamarin.Firebase.Messaging**합니다. 

3.  이 패키지에 설치 된 **FCMClient** 프로젝트: 

    [![Xamarin Firebase 메시징 설치](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----
 
설치 하는 경우 **Xamarin.Firebase.Messaging**, 모든 필요한 종속성도 설치 됩니다.

다음에 편집 **MainActivity.cs** 다음 추가 `using` 문:

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

처음 두 문은의 형식 확인은 **Xamarin.Firebase.Messaging** NuGet 패키지를 사용할 수 있는 **FCMClient** 코드. **Android.Util** FMS 사용 하 여 트랜잭션을 관찰 하는 로깅 기능을 추가 합니다. 


### <a name="add-the-google-services-json-file"></a>Google 서비스 JSON 파일에 추가

다음 단계를 추가 하는 것은 **google services.json** 파일을 프로젝트의 루트 디렉터리에: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  복사 **google services.json** 프로젝트 폴더에 있습니다.

2.  추가 **google services.json** 응용 프로그램 프로젝트에 (클릭 **모든 파일 표시** 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 **google-services.json**을 선택한 후 **프로젝트에 포함**). 

3.  선택 **google services.json** 에 **솔루션 탐색기** 창.

4.  에 **속성** 창 설정는 **빌드 작업** 를 **GoogleServicesJson** (하는 경우는 **GoogleServicesJson** 빌드 작업이 표시 되지 않습니다 저장 하 고 솔루션을 닫았다가):

    [![빌드 작업 GoogleServicesJson을로 설정](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  복사 **google services.json** 프로젝트 폴더에 있습니다.

2.  추가 **google services.json** 응용 프로그램 프로젝트에 있습니다. 

3.  마우스 오른쪽 단추로 클릭 **google services.json**합니다.

4.  설정의 **빌드 작업** 를 **GoogleServicesJson**: 

    [![빌드 작업 GoogleServicesJson을로 설정](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)
 
-----
 

때 **google services.json** 프로젝트에 추가 (및 **GoogleServicesJson** 빌드 작업이 설정), 빌드 프로세스에서 클라이언트 ID와 API 키를 추출 하 고 다음에 병합 된 이러한 자격 증명을 추가 / 생성 된 **AndroidManifest.xml** 에 상주 하는 **obj/Debug/android/AndroidManifest.xml**합니다. 이 병합 프로세스는 자동으로 모든 사용 권한 및 FCM 서버에 연결에 필요한 다른 FCM 요소를 추가 합니다. 


## <a name="check-for-google-play-services"></a>Google Play 서비스에 대 한 확인

응용 프로그램의 UI에 대 한 초기 레이아웃을 먼저 생성 됩니다. 편집 **Resources/layout/Main.axml** 해당 내용을 다음 XML로 바꿉니다. 

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

이 `TextView` Google Play 서비스 설치 되어 있는지 여부를 나타내는 메시지를 표시 하는 데 사용 됩니다. 에 변경 내용을 저장 **Main.axml**합니다. 편집 **MainActivity.cs** 다음 인스턴스 변수 선언에 추가 하 고는 `MainActivity` 클래스: 

```csharp
TextView msgText;
```

Google에 Google Play 서비스 기능에 액세스 하기 전에 Android 앱에 Google 재생 서비스 APK 있는지 확인 하는 것이 좋습니다 (자세한 내용은 참조 [Google Play 서비스에 대 한 확인](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)). 다음 예제에서는 `OnCreate` 메서드는 응용 프로그램 FCM 서비스를 사용 하려고 하기 전에 Google Play 서비스를 사용할 수 있는지 확인 합니다. 다음 메서드를 추가 `MainActivity` 클래스: 

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

이 코드에는 장치에서 Google 재생 서비스 APK이 설치 되어 있는지를 확인 합니다. 에 메시지가 표시 됩니다 설치 되지 않은 경우는 `TextBox` 사용자는 APK Google Play 스토어에서 다운로드 하도록 (또는 장치의 시스템 설정에서 사용 하도록 설정) 하는 지시 합니다. 

`OnCreate` 메서드를 다음 코드로 바꿉니다. 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);
    IsPlayServicesAvailable ();
}
```

`IsPlayServicesAvailable` 끝에서 호출 `OnCreate` Google Play 서비스 각 실행은 응용 프로그램 시작 될 때마다 확인 되도록 합니다. 응용 프로그램에 있는 경우는 `OnResume` 호출 메서드를 `IsPlayServicesAvailable` 에서 `OnResume` 도 합니다. 완전히 다시 작성 하 고 응용 프로그램을 실행 합니다. 모두 제대로 구성 되어 다음 스크린샷과 같이 화면이 표시 되어야 합니다. 

[![응용 프로그램 Google Play 서비스를 사용할 수 있는지 나타냅니다.](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

이 결과 얻지 못한 경우 Google 재생 서비스 APK 장치에 설치 되어 있는지 확인 하십시오 (자세한 내용은 참조 [설정을을 Google Play 서비스](https://developers.google.com/android/guides/setup)). 또한 추가 했는지 확인는 **Xamarin.Google.Play.Services.Base** 패키지를 프로그램 **FCMClient** 앞에서 설명한 대로 프로젝트.


## <a name="add-the-instance-id-receiver"></a>인스턴스 ID 수신자를 추가 합니다.

확장 하는 서비스를 추가 하려면 다음 단계는 `FirebaseInstanceIdService` 회전 작성을 위한 Firebase 등록 토큰을 업데이트 하 고 있습니다. (등록 토큰에 대 한 자세한 내용은 [FCM 등록](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).) `FirebaseInstanceIdService` 를 장치에 메시지를 보낼 수 있게 되기를 FCM 서비스가 필요 합니다. 경우는 `FirebaseInstanceIdService` 앱 자동으로 FCM 메시지를 수신 하 고 응용 프로그램은 backgrounded 될 때마다 알림을으로 표시할 서비스 클라이언트 응용 프로그램에 추가 됩니다. 

### <a name="declare-the-receiver-in-the-android-manifest"></a>Android 매니페스트에서 수신기를 선언 합니다.

편집 **AndroidManifest.xml** 다음 삽입 `<receiver>` 요소는 `<application>` 섹션: 

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

이 XML은 다음을 수행합니다.

-   선언 된 `FirebaseInstanceIdReceiver` 를 제공 하는 [고유 식별자](https://developers.google.com/instance-id/) 각 응용 프로그램 인스턴스에 대 한 합니다. 또한이 수신기를 인증 하 고 작업에 권한을 부여 합니다. 

-   내부 선언 `FirebaseInstanceIdInternalReceiver` 안전 하 게 서비스를 시작 하는 데 사용 되는 구현 합니다.

`FirebaseInstanceIdReceiver` 는 `WakefulBroadcastReceiver` 받는 `FirebaseInstanceId` 및 `FirebaseMessaging` 이벤트에서 파생 된 클래스에 전달 하는 역할 `FirebaseInstanceIdService`합니다. 

### <a name="implement-the-firebase-instance-id-service"></a>Firebase 인스턴스 ID 서비스를 구현 합니다.

FCM을 응용 프로그램을 등록 하는 작업은 사용자 지정에서 처리 `FirebaseInstanceIdService` 제공 하는 서비스입니다. 
`FirebaseInstanceIdService` 다음 단계를 수행합니다. 

1.  사용 하 여는 [인스턴스 ID API](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) FCM와 응용 프로그램 서버에 액세스할 클라이언트 응용 프로그램 권한을 부여 하는 보안 토큰을 생성 하 합니다. 응용 프로그램에서에서 반환 되는 등록 토큰 FCM 합니다. 

2.  응용 프로그램 서버에 필요한 경우 응용 프로그램 서버에 등록 토큰을 전달 합니다.

라는 새 파일 추가 **MyFirebaseIIDService.cs** 해당 템플릿 코드는 다음과 같이 바꿉니다. 

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

이 서비스를 구현 하는 `OnTokenRefresh` 등록 토큰을 처음 만들거나 변경할 때 호출 되는 메서드입니다. 때 `OnTokenRefresh` 실행에서 최신 토큰 검색는 `FirebaseInstanceId.Instance.Token` 속성 (비동기적으로 의해 업데이트 될 FCM). 이 예제에서는 새로 고침된 토큰은 출력 창에서 볼 수 있도록 기록 됩니다. 

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` 자주 호출 됩니다: 다음과 같은 상황에서는 토큰을 업데이트 하는 데 사용 됩니다. 

-   때 응용 프로그램을 설치 하거나 제거 합니다.

-   때 사용자는 응용 프로그램 데이터를 삭제 합니다. 

-   인스턴스 id입니다. 응용 프로그램을 삭제 하는 경우

-   때 보안 토큰의가 손상 되었습니다.

Google에 따라 [인스턴스 ID](https://developers.google.com/instance-id/guides/android-implementation) 설명서, FCM 인스턴스 ID 서비스는 요청 해당 토큰이 주기적으로 (일반적으로 6 개월 마다) 앱에 새로 고침 하는 합니다. 

`OnTokenRefresh` 호출 또한 `SendRegistrationToAppServer` 사용자의 등록을 연결할 토큰 (있는 경우) 서버 쪽 계정을 사용 하 여 유지 관리 하는 응용 프로그램에서: 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

이 구현 하면 앱 서버의의 디자인에 의존 하기 때문에이 예에서 빈 메서드 본문이 제공 됩니다. 응용 프로그램 서버에 FCM 등록 정보가 필요 하면 수정 `SendRegistrationToAppServer` 에 응용 프로그램에서 유지 관리 모든 서버 쪽 계정을 사용자의 FCM 인스턴스 ID 토큰을 연결 합니다. (참고 토큰은 클라이언트 응용 프로그램에 불투명 합니다.) 

토큰의 앱 서버로 보내면 `SendRegistrationToAppServer` 토큰을 서버에 보냈는지 여부를 나타내는 부울을 유지 관리 해야 합니다. 이 부울이 false 이면 `SendRegistrationToAppServer` 의 앱 서버로 전송 합니다 &ndash; 토큰 이전 호출에서 응용 프로그램 서버에 이미 전송 된 그렇지 않은 경우. 경우에 따라 (이와 같은 `FCMClient` 예제), 응용 프로그램 서버 토큰을 필요 하지 않습니다; 따라서이 메서드는이 예제에 필요 하지 합니다. 

## <a name="implement-client-app-code"></a>클라이언트 앱 코드 구현

수신기 서비스를 저장 했으므로 이러한 서비스를 활용 하기 위해 클라이언트 앱 코드를 작성할 수 있습니다. 다음 섹션에서 단추 등록 토큰 로그온 UI에 추가 됩니다 (라고도 *인스턴스 ID 토큰*), 더 많은 코드에 추가 됩니다 `MainActivity` 보려는 `Intent` 에서 앱을 실행 하는 경우 정보는 알림: 

[![응용 프로그램 화면에 추가 로그 토큰 단추](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokens"></a>로그 토큰

이 단계에서 추가한 코드는 데모 목적만 제공 &ndash; 프로덕션 클라이언트 응용 프로그램 등록 토큰을 로그인 할 필요 없을 것입니다. 편집 **Resources/layout/Main.axml** 다음 추가 `Button` 선언 바로 뒤의 `TextView` 요소: 

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

편집 **MainActivity.cs** 에 다음 멤버 선언을 추가 하 고는 `MainActivity` 클래스:

```csharp
const string TAG = "MainActivity";
```

다음 코드의 끝에 추가 `OnCreate` 메서드: 

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

이 코드를 출력 창에 현재 토큰을 로그 때는 **로그 토큰** 단추 탭 됩니다. 

### <a name="handle-notification-intents"></a>알림 의도 처리 합니다.

사용자가에서 발급 한 알림을 누를 때 **FCMClient**, 해당 알림을 함께 제공 된 모든 데이터 메시지에서 사용할 수 `Intent` extras 합니다. 편집 **MainActivity.cs** 의 맨 위에 다음 코드를 추가 하 고는 `OnCreate` 메서드 (호출 하기 전에 `IsPlayServicesAvailable`): 

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

응용 프로그램의 실행 프로그램 `Intent` 하므로이 코드 모든 해당 데이터를 기록 합니다 사용자가 해당 알림 메시지를 누를 때 발생 하는 `Intent` 출력 창에 있습니다. 다른 `Intent` 발생 해야는 `click_action` 알림 메시지의 필드를 설정 해야 합니다 `Intent` (시작 관리자 `Intent` 하지 않으면 사용 되는 `click_action` 지정). 


## <a name="background-notifications"></a>배경 알림이

빌드 및 실행 된 **FCMClient** 응용 프로그램입니다. **로그 토큰** 단추가 표시 됩니다.

[![로그 토큰 단추가 표시 됩니다.](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

탭의 **로그 토큰** 단추입니다. IDE 출력 창에 다음과 같은 메시지가 나타납니다. 

[![출력 창에 표시 된 인스턴스 ID 토큰](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

긴 문자열 붙이지 **토큰** Firebase 콘솔 붙여넣으면 하는 인스턴스 ID 토큰 &ndash; 선택 하 고이 문자열을 클립보드에 복사 합니다. 인스턴스 ID 토큰을 표시 되지 않으면 다음 줄의 맨 위에 추가 `OnCreate` 되었는지 확인 하는 메서드 **google services.json** 올바르게 구문 분석:

```csharp
Log.Debug(TAG, "google app id: " + Resource.String.google_app_id);
```

`google_app_id` 출력 창에 기록 된 값과 일치 해야는 `mobilesdk_app_id` 에 기록 된 값 **google services.json**합니다. 

### <a name="send-a-message"></a>메시지 보내기

에 로그인 된 [Firebase 콘솔](https://console.firebase.google.com), 프로젝트를 선택, 클릭 **알림**, 클릭 하 고 **첫 번째 메시지 보내기**: 

[![보내기 Your 첫 번째 메시지 단추](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

에 **작성 메시지** 페이지 메시지 텍스트를 입력 하 고 선택 **단일 장치**합니다. IDE 출력 창에서 인스턴스 ID 토큰을 복사 하 고 붙여넣습니다는 **FCM 등록 토큰** Firebase 콘솔 필드: 

[![메시지 대화 상자를 구성 합니다.](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Android 장치 (또는 에뮬레이터에), 앱은 Android를 탭 하 여 백그라운드 **개요** 단추와 홈 화면을 터치 합니다. 클릭 하 여 장치 준비 되 면 **메시지 보내기** Firebase 콘솔에서: 

[![보내기 메시지 단추](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

경우는 **검토 메시지** 대화 상자가 표시 됩니다, 클릭 **보낼**합니다.
알림 아이콘은 장치 (또는 에뮬레이터)의 알림 영역에 표시 됩니다. 

[![알림 아이콘 표시 됩니다.](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

메시지를 보려면 알림 아이콘을 엽니다. 알림 메시지 해야 정확 하 게 입력 된 내용는 **메시지 텍스트** Firebase 콘솔 필드: 

[![장치에 알림 메시지가 표시 됩니다.](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

돌아가려면 알림 아이콘을 누르세요.는 **FCMClient** 응용 프로그램입니다. `Intent` 전송 extras **FCMClient** IDE 출력 창에 나열 됩니다. 

[![의도 extras 키, 메시지 ID와 축소 키 나열](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

이 예제는 **에서** 키가 응용 프로그램의 Firebase 프로젝트 번호로 설정 (이 예제에서는 `41590732`), 및 **collapse_key** 해당 패키지 이름으로 설정 됩니다 ( **com.xamarin.fcmexample**). 메시지를 받지 하지 않으면 삭제 해 보십시오는 **FCMClient** 응용 프로그램에 액세스 (장치나 에뮬레이터) 하 고 위의 단계를 반복 합니다. 


> [!NOTE]
> 경우 하면 강제 닫기 응용 프로그램을 알림 배달 FCM 중지 됩니다. Android에서 중지 된 응용 프로그램의 구성 요소 시작 하는 실수로 또는 불필요 하 게 백그라운드 서비스 브로드캐스트를 방지 합니다. (이 동작에 대 한 자세한 내용은 참조 [중지 된 응용 프로그램에서 컨트롤 시작](https://developer.android.com/about/versions/android-3.1.html#launchcontrols).) 이러한 이유로 필요는 때마다 응용 프로그램을 수동으로 제거 하 고 실행 하 고이 디버그 세션에서 작업을 중지할 &ndash; 이렇게 하면 FCM 메시지는 계속 받을 수 있도록 새 토큰을 생성할 수 있습니다.

### <a name="add-a-custom-default-notification-icon"></a>추가 사용자 지정 기본 알림 아이콘

이전 예제에서 알림 아이콘은 응용 프로그램 아이콘으로 설정 됩니다. 다음 XML 알림에 대 한 사용자 지정 기본 아이콘을 구성합니다. Android에 있는 알림 아이콘은 명시적으로 설정 하지 모든 알림 메시지에 대 한이 사용자 지정 기본 아이콘이 표시 됩니다. 

사용자 지정 기본 알림 아이콘을 추가 하려면 추가 프로그램 아이콘을는 **리소스/그릴** 디렉터리 편집 **AndroidManifest.xml**, 다음 삽입 `<meta-data>` 는 요소`<application>`섹션: 

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

이 예제에서는 알림 아이콘 하에 있는 **리소스/그릴/ic\_stat\_ic\_notification.png** 사용자 지정 기본 알림 아이콘으로 사용 됩니다. 사용자 지정 기본 아이콘에 구성 되어 있지 않으면 **AndroidManifest.xml** 및 아이콘이 알림 페이로드에 설정 된, Android (표시 된 대로 알림 아이콘 위의 스크린샷에) 알림 아이콘으로 응용 프로그램 아이콘을 사용 합니다. 

## <a name="handle-topic-messages"></a>항목 메시지 처리

지금까지 작성 한 코드에는 앱에 원격 알림 기능을 추가 및 등록 토큰을 처리 합니다. 다음 예제에서는 추가 코드를 수신 하는 *항목 메시지* 원격 알림을 사용자에 게 전달 합니다. 항목 메시지는 특정 항목에 등록 하는 하나 이상의 장치에 전송 되는 FCM 메시지. 항목 메시지에 대 한 자세한 내용은 참조 [항목 메시징](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)합니다. 

### <a name="subscribe-to-a-topic"></a>주제 구독

편집 **Resources/layout/Main.axml** 다음 추가 `Button` 선언 이전 직후 `Button` 요소:

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

이 XML에 추가 **알림을 구독** 레이아웃 단추입니다.
편집 **MainActivity.cs** 의 끝에 다음 코드를 추가 하 고는 `OnCreate` 메서드: 

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

이 코드를 찾습니다는 **알림을 구독** 단추 레이아웃에서 호출 하는 코드를 해당 클릭 처리기를 할당 하 고 `FirebaseMessaging.Instance.SubscribeToTopic`구독 된 항목에 전달 하 _뉴스_합니다. 사용자가 누를 때는 **Subscribe** 단추, 응용 프로그램 구독 하는 _뉴스_ 항목입니다. 다음 섹션에는 _뉴스_ Firebase 콘솔 알림 GUI에서 항목 메시지가 전송 됩니다. 

### <a name="send-a-topic-message"></a>항목 메시지 보내기

앱을 제거 하 고 다시 만드는 것을 다시 실행 합니다. 클릭는 **알림을 구독할** 단추:

[![구독 알림 단추](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

응용 프로그램에서 성공적으로 구독을 하는 경우 표시 되어야 **항목 동기화 완료** IDE에 출력 창: 

[![출력 창에 메시지를 성공 동기화 하는 항목 표시](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

다음 절차 항목 메시지를 보낼 수 있습니다.

1.  Firebase 콘솔에서 클릭 **새 메시지**합니다. 

2.  에 **작성 메시지** 페이지 메시지 텍스트를 입력 하 고 선택 **항목**합니다. 

3.  에 **항목** 풀 다운 메뉴에서 기본 제공 항목 선택 **뉴스**: 

    [![뉴스 항목을 선택 하면](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4.  Android 장치 (또는 에뮬레이터에), 앱은 Android를 탭 하 여 백그라운드 **개요** 단추와 홈 화면을 터치 합니다. 

5.  클릭 하 여 장치 준비 되 면 **메시지 보내기** Firebase 콘솔에서. 

6.  IDE 출력 창을 확인 **/항목/뉴스** 로그 출력에: 

    [![/Topic/news에서 메시지가 표시 됩니다.](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

이 메시지가 출력 창에 표시 되는 Android 장치에서 알림 영역에서 알림 아이콘도 나타납니다. 항목 메시지를 보려면 알림 아이콘을 엽니다. 

[![알림으로 항목 메시지가 표시 됩니다.](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

메시지를 받지 하지 않으면 삭제 해 보십시오는 **FCMClient** 응용 프로그램에 액세스 (장치나 에뮬레이터) 하 고 위의 단계를 반복 합니다. 

## <a name="foreground-notifications"></a>전경 알림

구현 해야 foregrounded 앱에서 알림을 받으려는 `FirebaseMessagingService`합니다. 이 서비스는 데이터 페이로드를 수신 하기 위한 및 업스트림 메시지를 보내는 데 필요한도 합니다. 다음 예제를 확장 하는 서비스를 구현 하는 방법을 설명 `FirebaseMessagingService` &ndash; 결과 앱 포그라운드에서 실행 되는 동안 원격 알림을 처리 하는 일을 할 수 있습니다. 

### <a name="implement-firebasemessagingservice"></a>FirebaseMessagingService 구현

`FirebaseMessagingService` 서비스에는 `OnMessageReceived` 들어오는 메시지를 처리 하기 위해 작성 하는 메서드입니다. `OnMessageReceived` 알림 메시지에 대 한 호출 *만* 전경에서 앱을 실행 하는 경우. 응용 프로그램의 백그라운드에서 실행 중인 경우 (이 연습의 앞부분에서 설명)으로 자동으로 생성 된 알림이 표시 됩니다. 

라는 새 파일 추가 **MyFirebaseMessagingService.cs** 해당 템플릿 코드는 다음과 같이 바꿉니다. 

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

`MESSAGING_EVENT` 의도 필터 새 FCM 메시지 이동 되 고 선언 합니다 `MyFirebaseMessagingService`: 

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

클라이언트 응용 프로그램에서 FCM, 메시지를 받으면 `OnMessageReceived` 전달 기능에서 메시지 콘텐츠를 추출 하 `RemoteMessage` 개체를 호출 하 여 해당 `GetNotification` 메서드. 다음으로 IDE 출력 창에서 볼 수 있도록 메시지 내용을 기록 합니다. 

```csharp
Log.Debug(TAG, "From: " + message.From);
Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
```

> [!NOTE]
> 중단점을 설정한 경우 `FirebaseMessagingService`하려면 디버깅 세션 수도 FCM 메시지를 배달 하는 방법으로 인해 이러한 중단점을 적중할 수 있습니다.
 

### <a name="send-another-message"></a>다른 메시지 보내기

앱을 제거, 다시 작성, 다시 실행 및 다른 메시지를 보내려면 다음이 단계를 수행 합니다.

1.  Firebase 콘솔에서 클릭 **새 메시지**합니다. 

2.  에 **작성 메시지** 페이지 메시지 텍스트를 입력 하 고 선택 **단일 장치**합니다. 

3.  IDE 출력 창에서 토큰 문자열을 복사 하 고 붙여넣습니다는 **FCM 등록 토큰** 이전과 Firebase 콘솔의 필드입니다. 

4.  응용 프로그램을 전경에 실행 되 고 있는지 확인 한 다음 클릭 **메시지 보내기** Firebase 콘솔에서: 

    [![콘솔에서 다른 메시지 보내기](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5.  경우는 **검토 메시지** 대화 상자가 표시 됩니다, 클릭 **보낼**합니다.

6.  들어오는 메시지가 IDE 출력 창에 기록 됩니다.

    [![출력 창에 출력할지 메시지 본문](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)


### <a name="add-a-local-notifications-sender"></a>로컬 알림을 보낸 사람 추가

나머지이 예제에서는 앱이 포그라운드에서 실행 되는 동안 시작 된 로컬 알림으로 들어오는 FCM 메시지 변환 됩니다. 편집 **MyFirebaseMessageService.cs** 다음 추가 `using` 문: 

```csharp
using FCMClient;
using System.Collections.Generic;
```

다음 메서드를 추가 `MyFirebaseMessagingService`: 

```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (string key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }
    var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

    var notificationBuilder = new Notification.Builder(this)
        .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
        .SetContentTitle("FCM Message")
        .SetContentText(messageBody)
        .SetAutoCancel(true)
        .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManager.FromContext(this);
    notificationManager.Notify(0, notificationBuilder.Build());
}
```

이 코드는이 알림을 배경 알림이 구분 하기 위해 applicatiion 아이콘에서 다른 아이콘을 사용 하 여 알림을 표시 합니다. 추가 [ic\_stat\_ic\_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) 를 **리소스/그릴 수 있는** 에 포함 된 **FCMClient** 프로젝트. 

`SendNotification` 메서드 `Notification.Builder` 는 알림을 만드는 및 `NotificationManager` 알림을 시작 하는 데 사용 됩니다. 알림을 보유 한 `PendingIntent` 사용자 앱을 열고로 전달 된 문자열의 내용을 볼 수 있도록 `messageBody`합니다. Android 앱과 함께 대상으로 하려면 해당 버전에 따라 사용 하려는 `NotificationCompat.Builder` 대신 합니다. 에 대 한 자세한 내용은 `Notification.Builder`, 참조 [로컬 알림을](~/android/app-fundamentals/notifications/local-notifications.md)합니다. 

호출 된 `SendNotification` 후에 `OnMessageReceived` 메서드: 

```csharp
SendNotification(message.GetNotification().Body, message.Data);
```

이러한 변경으로 인해 `SendNotification` 앱이 포그라운드에서 고 알림 영역에는 알림이 표시 됩니다는 알림의 받을 때마다 실행 됩니다. 응용 프로그램 backgrounded는 경우 `SendNotification` 실행 되지 않으며 (이 연습 앞부분에 나오는 그림 참조) 백그라운드 알림 대신 시작 됩니다. 

### <a name="send-the-last-message"></a>마지막 메시지 보내기

앱을 제거, 다시 작성, 다시 실행 후 다음 단계를 사용 하 여 마지막 메시지를 보낼:

1.  Firebase 콘솔에서 클릭 **새 메시지**합니다. 

2.  에 **작성 메시지** 페이지 메시지 텍스트를 입력 하 고 선택 **단일 장치**합니다. 

3.  IDE 출력 창에서 토큰 문자열을 복사 하 고 붙여넣습니다는 **FCM 등록 토큰** 이전과 Firebase 콘솔의 필드입니다. 

4.  응용 프로그램을 전경에 실행 되 고 있는지 확인 한 다음 클릭 **메시지 보내기** Firebase 콘솔에서: 

    [![전경 메시지 보내기](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

이 이번에 출력 창에 기록 된 메시지는 새로운 알림이에 패키지도 &ndash; 앱이 포그라운드에서 실행 되는 동안 알림 아이콘이 알림 표시줄에 나타납니다. 

[![전경 메시지에 대 한 알림 아이콘](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

알림을 열면 Firebase 콘솔 알림 GUI에서 보낸 마지막 메시지를 표시 됩니다. 

[![전경 알림 전경 아이콘과 함께 표시](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)

 
## <a name="troubleshooting"></a>문제 해결

다음 describe 문제 및 Xamarin.Android를 Firebase Cloud Messaging을 사용 하는 경우 발생할 수 있는 해결 방법

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp 초기화 되지 않았습니다.

경우에 따라이 오류 메시지가 표시 될 수 있습니다.

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

이 솔루션을 정리 하 고 프로젝트를 다시 해결할 수 있는 알려진된 문제 (**빌드 > 솔루션 정리**, **빌드 > 솔루션 다시 빌드**). 이 참조에 대 한 자세한 내용은 [포럼 토론](https://forums.xamarin.com/discussion/96263/default-firebaseapp-is-not-initialized-in-this-process)합니다.


## <a name="summary"></a>요약

이 연습에서는 자세한 Xamarin.Android 응용 프로그램에서 원격 알림을 Firebase Cloud Messaging을 구현 하기 위한 단계입니다. FCM 통신에 필요한 하는 데 필요한 패키지를 설치 하는 방법을 설명 하 고 Android 매니페스트 FCM 서버에 대 한 액세스를 구성 하는 방법을 설명 하기. Google Play 서비스 있는지 확인 하는 방법을 보여 주는 코드 예를 제공 하는 것. FCM 등록 토큰에 대 한 협상 하는 인스턴스 ID 수신기 서비스를 구현 하는 방법을 보여 주는 것 및 응용 프로그램은 backgrounded 동안 배경 알림이이 코드에서는 하는 방법을 설명 된 것입니다. 항목 메시지에 등록 하는 방법을 설명 하 고 수신 하 고 앱이 포그라운드에서 실행 되는 동안 원격 알림을 표시 하는 데 사용 되는 메시지 수신기 서비스의 구현 예제가 제공 합니다. 


## <a name="related-links"></a>관련 링크

- [FCMNotifications (샘플)](https://developer.xamarin.com/samples/monodroid/Firebase/FCMNotifications)
- [Firebase 클라우드 메시징](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
