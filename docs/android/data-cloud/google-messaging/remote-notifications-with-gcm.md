---
title: "Google Cloud Messaging을 사용 하 여 원격 알림"
description: "이 연습에서는 Xamarin.Android 응용 프로그램에서 푸시 알림을 라고도 하는 원격 알림을 구현 하려면 Google Cloud Messaging을 사용 하는 방법에 대 한 단계별 설명은 제공 합니다. Google 클라우드 메시징 (GCM)와 통신 하기 위해 구현 해야 하는 다양 한 클래스를 설명 하 고 Android manifest GCM에 대 한 액세스 권한을 설정 하는 방법을 설명 종단 간 샘플 테스트 프로그램 메시징 코드를 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 64961e9c45c28ede4cc84f7b978da565be4426d9
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Google Cloud Messaging을 사용 하 여 원격 알림

_이 연습에서는 Xamarin.Android 응용 프로그램에서 푸시 알림을 라고도 하는 원격 알림을 구현 하려면 Google Cloud Messaging을 사용 하는 방법에 대 한 단계별 설명은 제공 합니다. Google 클라우드 메시징 (GCM)와 통신 하기 위해 구현 해야 하는 다양 한 클래스를 설명 하 고 Android manifest GCM에 대 한 액세스 권한을 설정 하는 방법을 설명 종단 간 샘플 테스트 프로그램 메시징 코드를 보여 줍니다._

## <a name="gcm-notifications-overview"></a>GCM 알림 개요

이 연습에서는 만들어 메시징 GCM (Google Cloud)를 사용 하 여 원격 알림을 구현 하는 Xamarin.Android 응용 프로그램 (라고도 *푸시 알림*). 원격 메시징에 대 한 GCM을 사용 하는 다양 한 의도 및 수신기 서비스 구현 하 고 응용 프로그램 서버를 시뮬레이션 하는 명령줄 프로그램으로 구현을 테스트 합니다. 

Firebase 클라우드 메시징 (FCM)는 새 버전의 GCM &ndash; GCM 아닌 FCM 사용 하 여 Google을 적극 권장 합니다. GCM, 현재 사용 중인 FCM로 업그레이드는 것이 좋습니다. FCM에 대 한 자세한 내용은 참조 [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)합니다. 

Google의 GCM 서버;를 사용 하는 데 필요한 자격 증명을 획득 해야이 연습을 진행할 수 있습니다, 전에 이 과정에 설명 되어 [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)합니다. 특히 필요 합니다는 *API 키* 및 *보낸 사람 ID* 이 연습에서 수행할 예제 코드에 삽입할 수 있습니다. 

Xamarin.Android GCM 지원 클라이언트 응용 프로그램을 만드는 다음 단계를 사용 합니다.

1.  GCM 서버와의 통신에 필요한 추가 패키지를 설치 합니다.
2.  GCM 서버에 대 한 액세스 권한을 응용 프로그램 구성 합니다.
3.  Google Play 서비스 있는지 확인 하기 위한 코드를 구현 합니다. 
4.  GCM 등록 토큰에 대 한 협상 하는 등록 의도 서비스를 구현 합니다.
5.  GCM에서 등록 토큰 업데이트를 수신 대기 하는 인스턴스 ID 수신기 서비스를 구현 합니다.
6.  GCM 통해 응용 프로그램 서버에서 원격 메시지를 수신 하는 GCM 수신기 서비스를 구현 합니다.

이 앱은 새 GCM 기능을 사용 합니다. *항목 메시징*합니다. 항목 메시징 응용 프로그램 서버 대신 개별 장치 목록에는 항목에 메시지를 보냅니다. 해당 항목에 등록 하는 장치를 푸시 알림으로 항목 메시지를 받을 수 있습니다. Google의 GCM 항목 메시징에 대 한 자세한 내용은 참조 [항목 메시징 구현](https://developers.google.com/cloud-messaging/topic-messaging)합니다. 

클라이언트 응용 프로그램 준비 되 면 명령줄 C# 응용 프로그램 GCM 통해 클라이언트 앱에 푸시 알림을 전송 하는 구현 합니다. 

## <a name="walkthrough"></a>연습

를 시작 하려면 라는 새 빈 솔루션을 만들어 보겠습니다 **RemoteNotifications**합니다. 다음으로 새 Android 프로젝트를 기반으로 하는이 솔루션에 추가 **Android 앱** 템플릿. 이 프로젝트 부르겠습니다 **ClientApp**합니다. (Xamarin.Android 프로젝트를 만드는 방법이 모르는 경우 참조 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).) **ClientApp** GCM 통해 원격 알림을 수신 하는 Xamarin.Android 클라이언트 응용 프로그램에 대 한 코드 프로젝트에 포함 됩니다. 

### <a name="add-required-packages"></a>필요한 패키지를 추가 합니다.

클라이언트 앱 코드를 구현할 수 있습니다, 전에 GCM과 통신 하는 데 사용할 여러 패키지를 설치 해야 합니다. 또한 아직 설치 되지 않은 경우 Google Play 스토어 응용 프로그램을 장치 추가 해야 합니다.

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Xamarin Google Play 서비스 GCM 패키지 추가

Google Cloud Messaging에서 메시지를 받을 [Google Play 서비스](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/) 프레임 워크는 장치에 있어야 합니다. 이 프레임 워크 없이 Android 응용 프로그램 GCM 서버에서 메시지를 받을 수 없습니다. Google Play 서비스 실행 백그라운드에서 Android 장치를 켤 때 백그라운드 GCM에서 메시지를 수신 대기 합니다. 이러한 메시지가 도착 하면 Google Play 서비스 의도를 메시지를 변환 하 한 다음 이러한 의도를 등록 된 응용 프로그램을 브로드캐스트합니다. 

Visual Studio에서 마우스 오른쪽 단추로 클릭 **참조 > NuGet 패키지 관리...** ; Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가 중...** . 검색할 **Xamarin Google Play 서비스-GCM** 에이 패키지를 설치 하 고는 **ClientApp** 프로젝트: 

[![Google Play 서비스를 설치합니다.](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

설치 하는 경우 **Xamarin Google Play 서비스-GCM**, **Xamarin Google Play 서비스-자료** 자동으로 설치 됩니다. 오류가 발생 하는 경우 프로젝트의 변경 *최소 Android 대상* 아닌 다른 값으로 설정을 **SDK 버전을 사용 하 여 컴파일** NuGet 설치를 다시 시도 하십시오. 

다음에 편집 **MainActivity.cs** 다음 추가 `using` 문:

```csharp
using Android.Gms.Common;
using Android.Util;
```

형식 Google 재생 서비스 GMS 패키지에이 코드에서 사용할 수 있고, GMS 사용 하 여이 트랜잭션을 추적 하는 데 사용할 로깅 기능을 추가 합니다. 

#### <a name="google-play-store"></a>Google Play 스토어

GCM에서 메시지를 받으려면 장치에서 Google Play 스토어 응용 프로그램을 설치 합니다. (Google Play 응용 프로그램은 장치에 설치 되어, 때마다 Google Play 스토어 설치 되어 테스트 장치에 이미 설치 되어 가능성이 큽니다.) Google Play 없이 Android 응용 프로그램 GCM에서 메시지를 받을 수 없습니다. Google Play 스토어 앱이 장치에 설치 된 아직 없는 경우 방문는 [Google Play](https://support.google.com/googleplay) 웹 사이트를 다운로드 하 여 Google Play를 설치 합니다. 

또는 (않아도 Android 에뮬레이터에 Google Play 스토어를 설치 하려면) 테스트 장치 대신 2.2 이상 Android를 실행 하는 Android 에뮬레이터를 사용할 수 있습니다. 그러나 에뮬레이터를 사용 하는 경우 GCM에 연결할 Wi-fi를 사용 해야 하며이 연습의 뒷부분에서 설명 된 대로 Wi-fi 방화벽에서 여러 포트를 열고 해야 합니다. 

### <a name="set-the-package-name"></a>패키지 이름 설정

[Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md), GCM 사용이 가능한 앱에 대 한 패키지 이름 지정 (이 패키지 이름으로도 사용는 *응용 프로그램 ID* 우리의 API 키 및 보낸 사람 ID와 연결 된). 에 대 한 속성을 열어 보겠습니다는 **ClientApp** 프로젝트 및 패키지 이름을이 문자열로 설정 합니다. 이 예제 패키지 이름을 설정 `com.xamarin.gcmexample`:

[![패키지 이름 설정](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

클라이언트 응용 프로그램을 패키지 이름이이 가리키지 않으면 GCM에서 등록 토큰을 받을 수 됩니다는 *정확히* Google 개발자 콘솔에 회사 이름을 입력 하는 패키지 이름과 일치 합니다. 

### <a name="add-permissions-to-the-android-manifest"></a>Android 매니페스트에서에 권한을 추가합니다

Android 응용 프로그램에 다음 사용 권한을 구성한 후에 Google Cloud Messaging에서 알림을 받을 수 있어야 합니다. 

-   `com.google.android.c2dm.permission.RECEIVE` &ndash; 앱 등록 하 고 Google Cloud Messaging에서 메시지를 받을 수 있는 권한을 부여 합니다. (용도 `c2dm` 의미? 이 _장치 메시징에 클라우드_, GCM에 이제 사용 되지 않는 선행 되 합니다. 
    GCM 여전히 사용 하 여 `c2dm` 해당 권한 문자열 다양 합니다.) 

-   `android.permission.WAKE_LOCK` &ndash; (선택 사항) 장치를에서 CPU에서 메시지를 수신 대기 하는 동안 절전 상태로 전환할 수 없습니다. 

-   `android.permission.INTERNET` &ndash; 클라이언트 응용 프로그램 GCM 통신할 수 있도록 인터넷 액세스를 부여 합니다. 

-   *package_name* `.permission.C2D_MESSAGE` &ndash; Android와 응용 프로그램을 등록 하 고 모든 C2D 단독으로 나타날 수 있는 권한을 요청 메시지 (장치로 클라우드). *package_name* 접두사는 응용 프로그램 ID와 동일 

Android 매니페스트에서 이러한 사용 권한을 설정 합니다. 편집 **AndroidManifest.xml** 하 고 내용을 다음 XML로 바꿉니다. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" 
    package="YOUR_PACKAGE_NAME" 
    android:versionCode="1" 
    android:versionName="1.0" 
    android:installLocation="auto">
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" />
    <permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" 
                android:protectionLevel="signature" />
    <application android:label="ClientApp" android:icon="@drawable/Icon">
    </application>
</manifest>
```

위 XML에서 변경 *YOUR_PACKAGE_NAME* 을 클라이언트 응용 프로그램 프로젝트에 대 한 패키지 이름입니다. 예를 들어, `com.xamarin.gcmexample`을 입력합니다. 

### <a name="check-for-google-play-services"></a>Google Play 서비스에 대 한 확인

이 연습에서는 단일 핵심 응용 프로그램을 만들고 `TextView` UI에 있습니다. 이 응용 프로그램 GCM와의 상호 작용을 직접 나타내지 않습니다. 대신, 출력 창을 시청 합니다 म 어떻게 우리의 앱 핸드셰이크가, GCM과 함께 도착 했을 때 새 알림에 대 한 알림 트레이 확인 하 고 있습니다. 

첫째, 메시지 영역에 대 한 레이아웃을 만들어 보겠습니다. 편집 **Resources.layout.Main.axml** 하 고 내용을 다음 XML로 바꿉니다. 

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

저장 **Main.axml** 하 고 닫습니다.

클라이언트 응용 프로그램 시작 될 때 원하는 GCM 문의 하기 전에 Google Play 서비스를 사용할 수 있는지 확인 합니다. 편집 **MainActivity.cs** 바꾸고는 ``count`` 다음 인스턴스 변수 선언은 변수 선언을 인스턴스: 

```csharp
TextView msgText;
```

다음 메서드를 다음으로 추가 된 **MainActivity** 클래스: 

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
            msgText.Text = "Sorry, this device is not supported";
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

이 코드에는 장치에서 Google 재생 서비스 APK이 설치 되어 있는지를 확인 합니다. 설치 되지 않은 경우에 APK Google Play 스토어에서 다운로드 (또는 장치의 시스템 설정에서 사용 하도록 설정)은 사용자 메시지 영역에는 메시지가 표시 됩니다. 이 메서드에 대 한 호출의 끝에 추가 하겠습니다 클라이언트 응용 프로그램을 시작할 때이 검사를 실행 하려는 것 이므로 `OnCreate`합니다. 


다음으로 바꾸기는 `OnCreate` 메서드를 다음 코드로:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();
}
```

이 코드는 Google 재생 서비스 APK 여부를 확인 하 고 결과 메시지 영역에 기록 합니다. 

보겠습니다 완전히 다시 작성 하 고 응용 프로그램을 실행 합니다. 다음 스크린 샷에서 같은 화면이 나타나야 합니다. 

[![Google Play 서비스를 사용할 수](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

이 결과 얻지 못한 경우 Google 재생 서비스 APK 장치에 설치 되어 있는지 확인은 **Xamarin Google Play 서비스-GCM** 패키지에 추가 됩니다 프로그램 **ClientApp** 설명 된 대로 프로젝트 이전 버전입니다. 빌드 오류가 발생할 경우 솔루션을 정리 하 고 프로젝트를 다시 빌드 해 보십시오. 

다음으로, GCM에 게 문의 하 고 등록 토큰을 가져오는 코드를 작성 합니다.

### <a name="register-with-gcm"></a>GCM 등록

응용 프로그램 응용 프로그램 서버에서 원격 알림을 받으려면, GCM 등록 하 고 등록 토큰을 가져오는 해야 해당 합니다. GCM을 응용 프로그램을 등록 하는 작업에서 처리 되는 `IntentService` 작성 하 합니다. 우리의 `IntentService` 다음 단계를 수행 합니다. 

1.  사용 하 여는 [InstanceID](https://developers.google.com/instance-id/) 응용 프로그램 서버에 액세스 하는 클라이언트 응용 프로그램 권한을 부여 하는 보안 토큰을 생성 하는 API입니다. 다시 등록 하는 토큰에서 얻게 GCM 합니다.

2.  (응용 프로그램 서버에 필요한) 하는 경우 응용 프로그램 서버에 등록 토큰을 전달 합니다.

3.  하나 이상의 알림 항목 채널을 구독합니다.

이 구현 후 `IntentService`, 하는 경우 다시 등록 하는 토큰에서 받는 GCM 볼를 테스트해 보겠습니다.

라는 새 파일 추가 **RegistrationIntentService.cs** 템플릿 코드는 다음과 같이 바꿉니다.


```csharp
using System;
using Android.App;
using Android.Content;
using Android.Util;
using Android.Gms.Gcm;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false)]
    class RegistrationIntentService : IntentService
    {
        static object locker = new object();

        public RegistrationIntentService() : base("RegistrationIntentService") { }

        protected override void OnHandleIntent (Intent intent)
        {
            try
            {
                Log.Info ("RegistrationIntentService", "Calling InstanceID.GetToken");
                lock (locker)
                {
                    var instanceID = InstanceID.GetInstance (this);
                    var token = instanceID.GetToken (
                        "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);

                    Log.Info ("RegistrationIntentService", "GCM Registration Token: " + token);
                    SendRegistrationToAppServer (token);
                    Subscribe (token);
                }
            }
            catch (Exception e)
            {
                Log.Debug("RegistrationIntentService", "Failed to get a registration token");
                return;
            }
        }

        void SendRegistrationToAppServer (string token)
        {
            // Add custom implementation here as needed.
        }

        void Subscribe (string token)
        {
            var pubSub = GcmPubSub.GetInstance(this);
            pubSub.Subscribe(token, "/topics/global", null);
        }
    }
}
```

위의 샘플 코드에서 변경 *YOUR_SENDER_ID* 클라이언트 응용 프로그램 프로젝트에 대 한 보낸 사람 ID 번호. 프로젝트에 대 한 보낸 사람 ID를 가져오려면: 

1.  에 로그인 된 [Google 클라우드 콘솔](https://console.cloud.google.com/) 풀 다운 메뉴에서에서 프로젝트 이름을 선택 합니다. 에 **정보 프로젝트** 프로젝트에 대해 표시 되는 창의 클릭 **프로젝트 설정으로 이동**:

    [![XamarinGCM 프로젝트를 선택합니다.](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2.  에 **설정** 페이지에서 찾은 **프로젝트 번호** &ndash; 프로젝트에 대 한 보낸 사람 ID입니다.

    [![프로젝트 번호 표시](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

시작 하려면 원하는 우리의 `RegistrationIntentService` 앱 실행을 시작할 때입니다. 편집 **MainActivity.cs** 수정 하 고는 `OnCreate` 메서드 있도록 우리의 `RegistrationIntentService` Google Play 서비스의 존재를 확인 한 후 시작: 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView(Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    if (IsPlayServicesAvailable ())
    {
        var intent = new Intent (this, typeof (RegistrationIntentService));
        StartService (intent);
    }
}
```

이제의 각 섹션에 살펴보겠습니다 `RegistrationIntentService` 작동 방식을 이해 하려면. 

주석을 먼저 우리의 `RegistrationIntentService` 서비스 시스템에 의해 인스턴스화될 하지 임을 나타내려면 다음 특성을 사용 합니다. 

```csharp
[Service (Exported = false)]
```

`RegistrationIntentService` 생성자 작업자 스레드 이름을 *RegistrationIntentService* 디버깅을 쉽게 만드는 합니다. 

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

핵심 기능 `RegistrationIntentService` 에 `OnHandleIntent` 메서드. GCM을 앱에 등록 방법을 확인 하려면이 코드를 살펴보겠습니다.


##### <a name="request-a-registration-token"></a>등록 토큰 요청

`OnHandleIntent` Google의 첫 번째로 호출 [InstanceID.GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) GCM에서 등록 토큰을 요청 하는 메서드. 이 코드를 래핑할는 `lock` 동시에 수행 되는 여러 등록 의도의 가능성을 방지 하 &ndash; 는 `lock` 하면 이러한 의도 순차적으로 처리 됩니다. 등록 토큰을 가져와서 된다면 예외가 throw 되 고 오류를 기록 했습니다. 등록에 성공 하면 `token` GCM에서 가져온 다시 등록 토큰으로 설정 됩니다. 

```csharp
static object locker = new object ();
...
try
{
    lock (locker)
    {
        var instanceID = InstanceID.GetInstance (this);
        var token = instanceID.GetToken (
            "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);
        ...
    }
}
catch (Exception e)
{
    Log.Debug ...
```

##### <a name="forward-the-registration-token-to-the-app-server"></a>응용 프로그램 서버에 등록 토큰을 전달 합니다.

등록 토큰을 얻을 수 있는 경우 (즉, 예외가 throw 되었으면) 이라고 `SendRegistrationToAppServer` 사용자의 등록을 연결 하 토큰 (있는 경우) 서버 쪽 계정을 사용 하 여 유지 관리 하는 응용 프로그램에 의해 합니다. 이 구현 하면 앱 서버의의 디자인에 의존 하기 때문에 빈 메서드 여기에 제공 됩니다. 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

경우에 따라 응용 프로그램 서버는 사용자의 등록 토큰; 필요 하지 않습니다. 이 경우이 메서드는 생략할 수 있습니다. 응용 프로그램 서버에 등록 토큰을 보내면 `SendRegistrationToAppServer` 토큰을 서버에 보냈는지 여부를 나타내는 부울을 유지 관리 해야 합니다. 이 부울이 false 이면 `SendRegistrationToAppServer` 의 앱 서버로 전송 합니다 &ndash; 토큰 이전 호출에서 응용 프로그램 서버에 이미 전송 된 그렇지 않은 경우. 


##### <a name="subscribe-to-the-notification-topic"></a>구독 하 여 알림 항목

호출 다음에 우리의 `Subscribe` 메서드를 나타내는 GCM 알림 항목 구독에에 있습니다. `Subscribe`, 이라고는 [GcmPubSub.Subscribe](https://developers.google.com/android/reference/com/google/android/gms/gcm/GcmPubSub.html#subscribe&#40;java.lang.String,%20java.lang.String,%20android.os.Bundle&#41;) 클라이언트 앱에서 모든 메시지를 구독 하는 API `/topics/global`:

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

응용 프로그램 서버에는 알림 메시지를 보내야 `/topics/global` 를 받을 하는 경우. 아래 항목 이름이 `/topics` 이러한 이름에 동의 하는 응용 프로그램 서버 및 클라이언트 응용 프로그램으로 사용자가 원하는 모든 될 수 있습니다. (이름을 여기에서 선택한 이유 `global` 응용 프로그램 서버에서 지 원하는 모든 항목에 메시지를 수신 한다고 나타냅니다.) 

서버 쪽에서 메시징 GCM 항목에 대 한 내용은 Google의을 참조 하십시오. [항목 메시징 보낼](https://developers.google.com/cloud-messaging/topic-messaging)합니다. 


#### <a name="implement-an-instance-id-listener-service"></a>인스턴스 ID 수신기 서비스를 구현 합니다.

등록 토큰은 고유 하 고 안전한; 그러나 클라이언트 응용 프로그램 (또는 GCM) 응용 프로그램 다시 설치 또는 보안 문제가 발생할 경우 등록 토큰을 새로 고치려면 해야 합니다. 이러한 이유로 구현 해야 합니다는 `InstanceIdListenerService` GCM에서 토큰 새로 고침 요청에 응답 합니다. 

라는 새 파일 추가 **InstanceIdListenerService.cs** 템플릿 코드는 다음과 같이 바꿉니다. 

```csharp
using Android.App;
using Android.Content;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
    class MyInstanceIDListenerService : InstanceIDListenerService
    {
        public override void OnTokenRefresh()
        {
            var intent = new Intent (this, typeof (RegistrationIntentService));
            StartService (intent);
        }
    }
}
```

주석 달기 `InstanceIdListenerService` 서비스가 시스템에서 인스턴스화할 수를 인지 하 고 GCM 등록 토큰을 받을 수 있는지 나타내려면 다음 특성을 사용 (호출 또한 *인스턴스 ID*) 새로 고침 요청: 

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

`OnTokenRefresh` 서비스에 대 한 메서드 시작는 `RegistrationIntentService` 새 등록 토큰을 가로챌 수 있도록 합니다.


#### <a name="test-registration-with-gcm"></a>GCM 사용 하 여 테스트 등록

보겠습니다 완전히 다시 작성 하 고 응용 프로그램을 실행 합니다. 성공적으로 GCM에서 등록 토큰을 수신 하는 경우 등록 토큰 출력 창에 표시 됩니다. 예: 

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>다운스트림 메시지 처리 

지금까지 구현한 코드는만 "설정" 코드; Google Play 서비스 설치 되어 있고 원격 알림을 수신 하기 위한 클라이언트 앱을 준비 하려면 GCM 및 응용 프로그램 서버와 협상 하는 경우를 확인 합니다. 그러나 해야 아직 실제로 수신 하 고 다운스트림 알림 메시지를 처리 하는 코드를 구현 합니다. 이 위해 구현 해야 합니다는 *GCM 수신기 서비스*합니다. 이 서비스 응용 프로그램 서버에서 항목 메시지를 받아서 알림을으로를 로컬로 브로드캐스팅합니다. 이 서비스를 구현 하는 후에 메시지를 보낼 GCM 구현은 올바르게 작동 하는 경우 볼 수 있도록 테스트 프로그램을 만들어 보겠습니다. 


#### <a name="add-a-notification-icon"></a>추가 알림 아이콘

이 알림을 시작할 때 알림 영역에 표시 되는 작은 아이콘을 먼저 추가 해 보겠습니다. 복사할 수는 있지만 [이 아이콘](remote-notifications-with-gcm-images/ic-stat-ic-notification.png) 프로젝트에 하거나 사용자 지정 아이콘을 만듭니다. 아이콘 파일의 이름을 **ic_stat_button_click.png** 복사는 **리소스/그릴** 폴더입니다. 사용 해야 **추가 > 기존 항목...**  프로젝트에서이 아이콘 파일을 포함 하도록 합니다.


#### <a name="implement-a-gcm-listener-service"></a>GCM 수신기 서비스를 구현 합니다.

라는 새 파일 추가 **GcmListenerService.cs** 템플릿 코드는 다음과 같이 바꿉니다.

```csharp
using Android.App;
using Android.Content;
using Android.OS;
using Android.Gms.Gcm;
using Android.Util;

namespace ClientApp
{
    [Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
    public class MyGcmListenerService : GcmListenerService
    {
        public override void OnMessageReceived (string from, Bundle data)
        {
            var message = data.GetString ("message");
            Log.Debug ("MyGcmListenerService", "From:    " + from);
            Log.Debug ("MyGcmListenerService", "Message: " + message);
            SendNotification (message);
        }

        void SendNotification (string message)
        {
            var intent = new Intent (this, typeof(MainActivity));
            intent.AddFlags (ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity (this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new Notification.Builder(this)
                .SetSmallIcon (Resource.Drawable.ic_stat_ic_notification)
                .SetContentTitle ("GCM Message")
                .SetContentText (message)
                .SetAutoCancel (true)
                .SetContentIntent (pendingIntent);

            var notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
            notificationManager.Notify (0, notificationBuilder.Build());
        }
    }
}
```

각 섹션에 살펴보겠습니다 우리의 `GcmListenerService` 작동 방식을 이해 하려면. 

주석을 먼저 `GcmListenerService` 특성이 표시 되는 시스템에 의해 인스턴스화될 수를이 서비스는 GCM 메시지를 받는 것을 나타내기 위해 의도 한 필터를 포함 합니다. 

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

때 `GcmListenerService` , GCM에서 메시지를 받은 `OnMessageReceived` 메서드가 호출 됩니다. 전달 기능에서 메시지 콘텐츠를 추출 하는이 메서드 `Bundle`, 메시지 콘텐츠 (하므로 출력 창에서 볼 수 있습니다), 기록 및 호출 `SendNotification` 로컬 알림을 받은 메시지 내용 사용 하 여 시작 하려면: 

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

`SendNotification` 메서드 `Notification.Builder` 사용 알림, 한 다음 만들려면는 `NotificationManager` 알림을 시작할 수 있습니다. 효과적으로이를 사용자에 게 제공할 때는 로컬 알림으로 원격 알림 메시지를 변환 합니다.
사용에 대 한 자세한 내용은 `Notification.Builder` 및 `NotificationManager`, 참조 [로컬 알림을](~/android/app-fundamentals/notifications/local-notifications.md)합니다.


#### <a name="declare-the-receiver-in-the-manifest"></a>매니페스트에 수신기를 선언 합니다.

GCM에서 메시지를 받을 수 있습니다, 전에 GCM 수신기 Android 매니페스트에서 선언 해야 합니다. 편집 **AndroidManifest.xml** 바꾸고는 `<application>` 다음 XML로 섹션: 

```xml
<application android:label="RemoteNotifications" android:icon="@drawable/Icon">
    <receiver android:name="com.google.android.gms.gcm.GcmReceiver" 
              android:exported="true" 
              android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="YOUR_PACKAGE_NAME" />
        </intent-filter>
    </receiver>
</application>
```

위 XML에서 변경 *YOUR_PACKAGE_NAME* 을 클라이언트 응용 프로그램 프로젝트에 대 한 패키지 이름입니다. 연습 예제 패키지 이름은 `com.xamarin.gcmexample`합니다. 

이 XML에 각 설정에 살펴보겠습니다.

<table>
    <thead>
        <tr>
            <th>설정</th>
            <th>설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>com.google.android.gms.gcm.GcmReceiver</code></td>
            <td>앱 캡처하고 들어오는 푸시 알림 메시지를 처리 하는 GCM 수신기를 구현 함을 선언 합니다.</td>
        </tr>
        <tr>
            <td><code>com.google.android.c2dm.permission.SEND</code></td>
            <td>GCM 서버 에서만 응용 프로그램에 직접 메시지를 보낼 수 있음을 선언 합니다.</td>
        </tr>
        <tr>
            <td><code>com.google.android.c2dm.intent.RECEIVE</code></td> 
            <td>앱에서 GCM에서 브로드캐스트 메시지를 처리 하는 광고 의도 필터입니다.</td>
        </tr>
        <tr>
            <td><code>com.google.android.c2dm.intent.REGISTRATION</code></td>
            <td>앱에서 새 등록 의도 처리 하는 광고 의도 필터 (즉, 구현 했습니다 인스턴스 ID 수신기 서비스).</td>
        </tr>
    </tbody>
</table>

데코레이팅 할 수 있습니다 `GcmListenerService` XML;에 지정 하는 대신 이러한 특성으로 여기 지정에서 **AndroidManifest.xml** 코드 예제를 보다 쉽게 따를 수 있도록 합니다. 


### <a name="create-a-message-sender-to-test-the-app"></a>앱을 테스트 하려면 메시지 보낸 사람 만들기

C# 데스크톱 콘솔 응용 프로그램 프로젝트를 솔루션에 추가 하 고이 호출 하겠습니다 **MessageSender**합니다. 에서는이 콘솔 응용 프로그램을 응용 프로그램 서버를 시뮬레이션 &ndash; 알림 메시지를 송신할 **ClientApp** GCM 통해 합니다. 


#### <a name="add-the-jsonnet-package"></a>Json.NET 패키지 추가

이 콘솔 응용 프로그램에서 클라이언트 응용 프로그램에 보내도록 하겠습니다 알림 메시지를 포함 하는 JSON 페이로드를 구축 하 고 있습니다. 에서는 **Json.NET** 군요 **MessageSender** GCM에 필요한 JSON 개체를 만들려면 쉽게 수행할 수 있도록 합니다. Visual Studio에서 마우스 오른쪽 단추로 클릭 **참조 > NuGet 패키지 관리...** ; Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가 중...** . 

찾아 보겠습니다는 **Json.NET** 패키지 및 프로젝트에 설치 합니다. 

[![Json.NET 패키지 설치](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)


#### <a name="add-a-reference-to-systemnethttp"></a>System.Net.Http에 대 한 참조를 추가 합니다.

에 대 한 참조를 추가 해야 또한 `System.Net.Http` म 인스턴스화할 수 있도록는 `HttpClient` GCM에 테스트 메시지를 보내기 위한 합니다. 에 **MessageSender** 프로젝트를 마우스 오른쪽 단추로 클릭 **참조 > 참조 추가** 나타날 때까지 아래로 스크롤하여 및 **System.Net.Http**합니다. 옆에 확인 표시를 추가할 **System.Net.Http** 클릭 **확인**합니다. 


#### <a name="implement-code-that-sends-a-test-message"></a>테스트 메시지를 보내는 코드를 구현 합니다.

**MessageSender**, 편집 **Program.cs** 하 고 내용을 다음 코드로 바꿉니다.

```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json.Linq;

namespace MessageSender
{
    class MessageSender
    {
        public const string API_KEY = "YOUR_API_KEY";
        public const string MESSAGE = "Hello, Xamarin!";

        static void Main (string[] args)
        {
            var jGcmData = new JObject();
            var jData = new JObject();

            jData.Add ("message", MESSAGE);
            jGcmData.Add ("to", "/topics/global");
            jGcmData.Add ("data", jData);

            var url = new Uri ("https://gcm-http.googleapis.com/gcm/send");
            try
            {
                using (var client = new HttpClient())
                {
                    client.DefaultRequestHeaders.Accept.Add(
                        new MediaTypeWithQualityHeaderValue("application/json"));

                    client.DefaultRequestHeaders.TryAddWithoutValidation (
                        "Authorization", "key=" + API_KEY);

                    Task.WaitAll(client.PostAsync (url,
                        new StringContent(jGcmData.ToString(), Encoding.Default, "application/json"))
                            .ContinueWith(response =>
                            {
                                Console.WriteLine(response);
                                Console.WriteLine("Message sent: check the client device notification tray.");
                            }));
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Unable to send GCM message:");
                Console.Error.WriteLine(e.StackTrace);
            }
        }
    }
}
```

위의 코드에서 변경 *YOUR_API_KEY* 클라이언트 응용 프로그램 프로젝트에 대 한 API 키에 있습니다. 

이 테스트 응용 프로그램 서버 GCM에 다음 JSON 형식의 메시지를 보냅니다.

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

차례로 GCM, 클라이언트 응용 프로그램에이 메시지를 전달합니다. 제작 해 보겠습니다 **MessageSender** 에서는 실행할 수 있는 것 명령줄에서 콘솔 창을 엽니다.



### <a name="try-it"></a>실습

이제 클라이언트 앱을 테스트할 준비가 되었습니다. 에뮬레이터를 사용 하는 경우 또는 장치와 통신 하는 경우 GCM과 wi-fi, 다음 TCP 포트를 통해 GCM 메시지에 대 한 방화벽에서 열어야: 5228, 5229, 및 5230 합니다.

클라이언트 앱을 시작 하 고 조사식 출력 창. 이후에 `RegistrationIntentService` 성공적으로 등록 하는 수신 GCM에서 토큰을 출력 창을 표시할지 토큰 다음과 같은 로그 출력:

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

이 시점에서 클라이언트 응용 프로그램 원격 알림 메시지를 받을 준비가 되었습니다. 명령줄에서 실행 하는 **MessageSender.exe** 프로그램 클라이언트 앱에 "Hello, Xamarin" 알림 메시지를 보내려고 합니다.
아직 빌드되지 하는 경우는 **MessageSender** 프로젝트를 지금 합니다.

실행 하려면 **MessageSender.exe** Visual Studio에서 명령 프롬프트를 열고, 변경 하는 **MessageSender/bin/Debug** 디렉터리와 명령 직접 실행:

```cmd
MessageSender.exe
```

실행 하려면 **MessageSender.exe** Mac 용 Visual Studio에서 터미널 세션을 열고, 변경 **MessageSender/bin/Debug** 디렉터리 및 실행을 사용 하 여 모노 **MessageSender.exe** 

```bash
mono MessageSender.exe
```

GCM 및 클라이언트 앱으로 다시 전파 하는 메시지에 대 한 분 걸릴 수 있습니다. 메시지가 성공적으로 수신 되 면 출력 창에 다음과 같은 출력 확인할: 

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

또한 알림 표시줄에 새 알림 아이콘이 나타나면 확인 해야 합니다. 

[![장치에 Notiication 아이콘 표시](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

알림을 보려면 알림 표시줄을 열 때이 원격 알림을 표시 되어야 합니다.

[![알림 메시지가 표시 됩니다.](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

축 하 합니다, 응용 프로그램의 첫 번째 원격 알림을 받았습니다!

Note 강제 중지 하는 경우 GCM 메시지가 수신 되지 않습니다. 알림을 강제로 중지 후 다시 시작 하려면 응용 프로그램에 수동으로 다시 시작 해야 합니다. 이 Android 정책에 대 한 자세한 내용은 참조 [중지 된 응용 프로그램에서 컨트롤 시작](https://developer.android.com/about/versions/android-3.1.html#launchcontrols) 이 [스택 오버플로 post](http://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267)합니다. 

 
## <a name="summary"></a>요약

이 연습에서는 자세한 Xamarin.Android 응용 프로그램에서 원격 알림을 구현 하기 위한 단계입니다. GCM 통신에 필요한 추가 패키지를 설치 하는 방법을 설명 하 고 앱 사용 권한 GCM 서버에 대 한 액세스를 구성 하는 방법을 설명 하기. Google Play 서비스 있는지 확인 하는 방법, 등록 의도 서비스와 협상 하 GCM 등록 토큰에 대 한 인스턴스 ID 수신기 서비스를 구현 하는 방법 및 GCM 수신기를 구현 하는 방법을 보여 주는 코드 예를 제공 수신 하 고 원격 알림 메시지를 처리 하는 서비스입니다. 마지막으로, GCM 통해 클라이언트 앱에 테스트 알림을 보낼 하는 명령줄 테스트 프로그램을 구현 했습니다. 


## <a name="related-links"></a>관련 링크

- [GCM RemoteNotifications (샘플)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications)
- [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
