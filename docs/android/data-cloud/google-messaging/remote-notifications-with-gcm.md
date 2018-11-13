---
title: Google Cloud Messaging을 사용 하 여 원격 알림
description: 이 연습에서는 Xamarin.Android 응용 프로그램에서 Google Cloud Messaging을 사용 하 여 푸시 알림을 라고도 하는 원격 알림을 구현 하는 방법에 대 한 단계별 설명은 제공 합니다. Google Cloud Messaging (GCM) 사용 하 여 통신을 구현 해야 하는 다양 한 클래스에 설명 합니다, 그리고 Android 매니페스트 GCM에 대 한 액세스 권한을 설정 하는 방법을 설명 하 고 엔드-투-엔드 샘플 테스트 프로그램을 사용 하 여 메시지를 보여 줍니다.
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/12/2018
ms.openlocfilehash: be96683a2e63ed802169543dcee55a3431e42130
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528808"
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Google Cloud Messaging을 사용 하 여 원격 알림

_이 연습에서는 Xamarin.Android 응용 프로그램에서 Google Cloud Messaging을 사용 하 여 푸시 알림을 라고도 하는 원격 알림을 구현 하는 방법에 대 한 단계별 설명은 제공 합니다. Google Cloud Messaging (GCM) 사용 하 여 통신을 구현 해야 하는 다양 한 클래스에 설명 합니다, 그리고 Android 매니페스트 GCM에 대 한 액세스 권한을 설정 하는 방법을 설명 하 고 엔드-투-엔드 샘플 테스트 프로그램을 사용 하 여 메시지를 보여 줍니다._

> [!NOTE]
> GCM에 의해 대체 되었습니다 [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM 서버 및 클라이언트 Api [되지](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) 더 이상 사용할 수 있는 한 빨리 2019 년 4 월 11 일을 합니다.

## <a name="gcm-notifications-overview"></a>GCM 알림 개요

이 연습에서는 Google Cloud Messaging (GCM)를 사용 하 여 원격 알림을 구현 하는 Xamarin.Android 응용 프로그램을 만듭니다 (라고도 *푸시 알림*). 원격 메시징에 대 한 GCM을 사용 하는 다양 한 의도 및 수신기 서비스 구현 하겠습니다 및 응용 프로그램 서버를 시뮬레이션 하는 명령줄 프로그램을 사용 하 여 구현을 테스트 합니다. 

FCM Firebase Cloud Messaging ()는 새 버전의 GCM &ndash; Google GCM 보다는 FCM을 사용 하 여 적극 권장 합니다. GCM을 현재 사용 중인 경우 FCM에 업그레이드 것이 좋습니다. FCM에 대 한 자세한 내용은 참조 하세요. [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)합니다. 

이 연습을 진행할 수 있습니다, 전에 Google의 GCM 서버를 사용 하는 데 필요한 자격 증명을 획득 해야 합니다. 이 프로세스는 설명 [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)합니다. 특히 해야는 *API 키* 와 *보낸 사람 ID* 이 연습에서 예제 코드에 삽입 합니다. 

Xamarin.Android GCM 사용이 가능한 클라이언트 앱을 만드는 다음 단계를 사용 하겠습니다.

1.  GCM 서버와의 통신에 필요한 추가 패키지를 설치 합니다.
2.  앱 GCM 서버에 대 한 액세스 권한을 구성 합니다.
3.  Google Play 서비스의 존재 여부를 확인 하는 코드를 구현 합니다. 
4.  Gcm 등록 토큰에 대 한 협상 하는 등록 의도 서비스를 구현 합니다.
5.  GCM 등록 토큰 업데이트를 수신 하는 인스턴스 ID 수신기 서비스를 구현 합니다.
6.  GCM 통해 앱 서버에서 원격 메시지를 수신 하는 GCM 수신기 서비스를 구현 합니다.

이 앱 이라는 새로운 GCM 기능을 사용할지 *항목에서는 메시징*합니다. 항목 메시징 앱 서버 개별 장치 목록이 아닌 토픽에 메시지를 보냅니다. 해당 토픽을 구독 하는 장치는 푸시 알림으로 항목 메시지를 받을 수 있습니다. 참조 Google의 GCM 항목 메시징에 대 한 자세한 내용은 [구현 항목 메시징](https://developers.google.com/cloud-messaging/topic-messaging)합니다. 

클라이언트 앱 준비 되 면 명령줄 구현에서는 C# 응용 프로그램을 통해 GCM 클라이언트 앱에 푸시 알림을 보냅니다. 

## <a name="walkthrough"></a>연습

시작 하려면 만들겠습니다 라는 새 빈 솔루션 **RemoteNotifications**합니다. 그런 다음 새 Android 프로젝트를 기반으로 하는이 솔루션에 추가 해 보겠습니다 합니다 **Android 앱** 템플릿. 이 프로젝트를 호출 해 보겠습니다 **ClientApp**합니다. (Xamarin.Android 프로젝트 만들기와 잘 모르는 경우 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).) 합니다 **ClientApp** 프로젝트 GCM 통해 원격 알림을 수신 하는 Xamarin.Android 클라이언트 응용 프로그램에 대 한 코드를 포함 합니다. 

### <a name="add-required-packages"></a>필요한 패키지를 추가 합니다.

클라이언트 앱 코드를 구현할 수 있습니다, 전에 GCM와의 통신에 사용 하는 여러 패키지 설치 해야 합니다. 또한 아직 설치 되지 않은 경우 장치에 Google Play 스토어 응용 프로그램을 추가 해야 합니다.

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Xamarin Google Play Services GCM 패키지를 추가 합니다.

Google Cloud Messaging에서 메시지를 수신 하는 [Google Play Services](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/) 프레임 워크는 장치에 있어야 합니다. 이 프레임 워크를 하지 않고 Android 응용 프로그램을 GCM 서버에서 메시지를 받을 수 없습니다. Google Play 서비스 실행 백그라운드에서 Android 장치는 전원이 켜져 있지만 조용히 GCM에서 메시지를 수신 대기 합니다. 이러한 메시지가 도착 하면 Google Play 서비스는 의도에 메시지를 변환 하 고에 등록 된 응용 프로그램에 이러한 의도 브로드캐스트합니다. 

Visual Studio에서 마우스 오른쪽 단추로 클릭 **참조 > NuGet 패키지 관리...** Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가...** . 검색할 **Xamarin Google Play 서비스-GCM** 하 고이 패키지를 설치 합니다 **ClientApp** 프로젝트: 

[![Google Play Services 설치](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

설치 하는 경우 **Xamarin Google Play 서비스-GCM**하십시오 **Xamarin Google Play 서비스-기본** 자동으로 설치 됩니다. 오류가 발생 하는 경우 프로젝트의 변경 *최소 Android 대상* 이외의 값으로 설정 **SDK 버전을 사용 하 여 컴파일** NuGet 설치 다시 시도 합니다. 

다음으로 편집 **MainActivity.cs** 추가한 다음 `using` 문:

```csharp
using Android.Gms.Common;
using Android.Util;
```

이렇게 하면 형식 Google Play Services GMS 패키지에 사용할 코드를 및 GMS 사용 하 여이 트랜잭션을 추적 하는 데 사용 하는 로깅 기능을 추가 합니다. 

#### <a name="google-play-store"></a>Google Play 스토어

GCM에서 메시지를 받으려면 Google Play 스토어 응용 프로그램을 장치에 설치 되어야 합니다. (응용 프로그램을 Google Play가 장치에 설치 될 때마다 Google Play 스토어 설치 되어 이기 때문에 테스트 장치에 이미 설치 되어 있습니다.) Google Play 없이 Android 응용 프로그램을 GCM에서 메시지를 받을 수 없습니다. Google Play 스토어 앱이 장치에 설치 합니다. 아직 없는 경우 방문 합니다 [Google Play](https://support.google.com/googleplay) 웹 사이트를 다운로드 하 여 Google Play를 설치 합니다. 

또는 (필요가 없습니다 Android 에뮬레이터에서 Google Play 스토어를 설치 하려면) 테스트 장치 대신 2.2 이상을 Android를 실행 하는 Android 에뮬레이터를 사용할 수 있습니다. 그러나 에뮬레이터를 사용 하는 경우 GCM에 연결할 Wi-fi를 사용 해야 하며 포트를 열어야 여러 Wi-fi 방화벽에서이 연습의 뒷부분에서 설명 했 듯이 합니다. 

### <a name="set-the-package-name"></a>패키지 이름 설정

[Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md), GCM 설정 앱의 패키지 이름을 지정 했습니다 (이 패키지 이름으로도 사용 합니다 *응용 프로그램 ID* API 키 및 보낸 사람 ID 사용 하 여 연결 된). 에 대 한 속성을 열어 보겠습니다 합니다 **ClientApp** 프로젝트 및 패키지 이름을이 문자열로 설정 합니다. 이 예에서는 패키지 이름을 `com.xamarin.gcmexample`:

[![패키지 이름 설정](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

클라이언트 앱이 패키지 이름은 그렇지 않은 경우에서 GCM 등록 토큰을 받을 수 있습니다 됩니다 수 있는지 *정확 하 게* Google 개발자 콘솔에 회사 이름을 입력 하는 패키지 이름과 일치 합니다. 

### <a name="add-permissions-to-the-android-manifest"></a>Android 매니페스트 권한 추가

Android 응용 프로그램에는 다음 사용 권한을 구성한 후에 Google Cloud Messaging에서 알림을 받을 수 있어야 합니다. 

-   `com.google.android.c2dm.permission.RECEIVE` &ndash; 이 앱을 등록 하 고 Google Cloud Messaging에서 메시지를 수신 하는 권한을 부여 합니다. (용도 `c2dm` 의미? 이 _클라우드-장치 메시징_, GCM에 이제 사용 되지 않는 선행 작업입니다. 
    GCM 사용 하 여 여전히 `c2dm` 해당 권한 문자열의 여러.) 

-   `android.permission.WAKE_LOCK` &ndash; (선택 사항) 메시지를 수신 하는 동안 절전 모드로 전환에서 장치의 CPU를 방지 합니다. 

-   `android.permission.INTERNET` &ndash; 클라이언트 앱이 GCM을 사용 하 여 통신할 수 있도록 인터넷 액세스를 부여 합니다. 

-   *package_name* `.permission.C2D_MESSAGE` &ndash; android 응용 프로그램을 등록 하 고 단독으로 모든 C2D를 수신 하기 위한 권한이 요청 (클라우드-장치) 메시지입니다. 합니다 *package_name* 접두사는 사용자 응용 프로그램 ID와 같습니다. 

Android 매니페스트에서 이러한 사용 권한을 설정 합니다. 편집할 **AndroidManifest.xml** 그 내용을 다음 XML로 바꿉니다. 

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

위의 xml에서 변경할 *YOUR_PACKAGE_NAME* 클라이언트 앱 프로젝트에 대 한 패키지 이름. 예를 들어, `com.xamarin.gcmexample`을 입력합니다. 

### <a name="check-for-google-play-services"></a>Google Play 서비스에 대 한 확인

이 연습에서는 앱을 만드는 핵심 단일 `TextView` UI에 있습니다. 이 앱이 GCM 사용 하 여 상호 작용을 직접 나타내지 않습니다. 보려면 출력 창을 확인에서는 대신 어떻게 GCM 사용 하 여이 앱 핸드셰이크 도착 했을 때 새 알림을 알림 표시줄을 확인 하 고 있습니다. 

먼저 메시지 영역에 대 한 레이아웃을 만들어 보겠습니다. 편집할 **Resources.layout.Main.axml** 그 내용을 다음 XML로 바꿉니다. 

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

저장할 **Main.axml** 하 고 닫습니다.

클라이언트 앱을 시작 하는 경우 원하는 GCM에 문의 하기 전에 Google Play 서비스를 사용할 수 있는지 확인 합니다. 편집할 **MainActivity.cs** 바꾸고는 ``count`` 다음 인스턴스 변수 선언과 변수 선언 인스턴스: 

```csharp
TextView msgText;
```

다음으로 다음 메서드를 추가 합니다 **MainActivity** 클래스: 

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

이 코드는 장치에 Google Play Services APK가 설치 되어 있는지 확인 합니다. 이 설치 되어 있지 않으면 APK를 Google Play 스토어에서 다운로드 (또는 장치의 시스템 설정에서 사용 하도록 설정) 사용자에 게 지시 하는 메시지 영역에서 메시지가 표시 됩니다. 클라이언트 앱을 시작할 때이 검사를 실행 하려는 것 이므로이 메서드에 대 한 호출의 끝에 추가한 `OnCreate`합니다. 


다음으로 대체 합니다 `OnCreate` 메서드를 다음 코드로:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();
}
```

이 코드는 Google Play Services APK의 있는지 여부를 확인 하 고 메시지 영역에 결과 씁니다. 

보겠습니다 완전히 다시 빌드하고 앱을 실행 합니다. 다음 스크린샷과 같은 화면이 표시 됩니다. 

[![Google Play 서비스를 사용할 수](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

이 결과 얻지 못한 경우 장치에서 Google Play Services APK 설치 되어 있는지 확인 합니다 **Xamarin Google Play 서비스-GCM** 패키지에 추가 됩니다 하 **ClientApp** 설명 된 대로 프로젝트 이전 합니다. 빌드 오류를 받게 되 면 솔루션을 정리 하 고 프로젝트를 다시 작성 합니다. 

다음으로, GCM에 게 문의 하 고 등록 토큰을 가져오는 코드를 작성 합니다.

### <a name="register-with-gcm"></a>GCM에 등록

앱 앱 서버에서 원격 알림을 받을 수 있습니다, 전에 GCM에 등록 하 고 등록 토큰을 가져옵니다. GCM을 사용 하 여 응용 프로그램을 등록 하는 작업에 의해 처리 됩니다는 `IntentService` 만든 것입니다. 우리의 `IntentService` 다음 단계를 수행 합니다. 

1.  사용 하는 [InstanceID](https://developers.google.com/instance-id/) 앱 서버에 액세스 하는 클라이언트 앱에 권한을 부여 하는 보안 토큰을 생성 하는 API입니다. 반환 시 니 등록 토큰 GCM에서.

2.  (앱 서버에 필요한) 하는 경우 앱 서버에 등록 토큰을 전달 합니다.

3.  하나 이상의 알림 항목에서는 채널을 구독합니다.

이 구현 후 `IntentService`을 테스트할 경우 다시 등록 하는 토큰에서 얻게 GCM 보려는 것입니다.

이라는 새 파일 추가 **RegistrationIntentService.cs** 템플릿 코드를 다음으로 바꿉니다.


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

위의 샘플 코드를 변경 *YOUR_SENDER_ID* 클라이언트 앱 프로젝트에 대 한 보낸 사람 ID 수입니다. 프로젝트에 대 한 보낸 사람 ID를 가져오려면: 

1.  에 로그인 합니다 [Google 클라우드 콘솔](https://console.cloud.google.com/) 풀 다운 메뉴에서에서 프로젝트 이름을 선택 합니다. 에 **정보를 프로젝트** 프로젝트에 대해 표시 되는 창 클릭 **프로젝트 설정으로 이동**:

    [![XamarinGCM 프로젝트 선택](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2.  에 **설정을** 페이지에서 찾을 합니다 **프로젝트 번호** &ndash; 프로젝트에 대 한 보낸 사람 id:

    [![프로젝트 번호 표시](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

시작 하려는 우리의 `RegistrationIntentService` 앱 실행을 시작할 때입니다. 편집 **MainActivity.cs** 수정 합니다 `OnCreate` 메서드 있도록 우리의 `RegistrationIntentService` Google Play 서비스의 존재 여부 확인 한 후 시작: 

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

이제 각 부분에 살펴보겠습니다 `RegistrationIntentService` 작동 방식을 이해 합니다. 

에서는 주석을 추가 하는 먼저 우리 `RegistrationIntentService` 서비스 시스템에서 인스턴스화할 필요가 임을 나타내려면 다음 특성을 사용 하 여: 

```csharp
[Service (Exported = false)]
```

합니다 `RegistrationIntentService` 생성자에는 작업자 스레드 이름을 *RegistrationIntentService* 디버깅을 쉽게 만듭니다. 

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

핵심 기능이 `RegistrationIntentService` 에 `OnHandleIntent` 메서드. GCM을 사용 하 여 앱 등록 하는 방법을 확인 하려면이 코드를 살펴보겠습니다.


##### <a name="request-a-registration-token"></a>등록 토큰을 요청

`OnHandleIntent` Google의는 먼저 호출 [InstanceID.GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) 에서 GCM 등록 토큰을 요청 하는 방법입니다. 이 코드를 래핑할를 `lock` 동시에 발생 하는 여러 등록 의도의 가능성을 방지 하기 위해 &ndash; 는 `lock` 하면 이러한 의도 순차적으로 처리 됩니다. 등록 토큰을 가져오려면, 실패 하면 예외가 throw 됩니다 하 고 오류를 기록 했습니다. 등록에 성공 하면 `token` GCM에서 다시 가져온 등록 토큰으로 설정 됩니다. 

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

##### <a name="forward-the-registration-token-to-the-app-server"></a>앱 서버에 등록 토큰을 전달

등록 토큰을 얻게 되는 경우 (즉, 예외가 throw 되지 않은), 호출 `SendRegistrationToAppServer` 사용자의 등록을 연결 하는 서버 쪽 계정 (해당 되는 경우)를 사용 하 여 토큰 유지 관리 되는 응용 프로그램입니다. 앱 서버의 디자인에 따라이 구현 때문에 빈 메서드 여기에 제공 됩니다. 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

일부 경우에 앱 서버는 사용자의 등록 토큰; 필요가 없습니다. 이 경우이 메서드는 생략할 수 있습니다. 앱 서버에 등록 토큰을 보내면 `SendRegistrationToAppServer` 서버에 토큰을 보냈는지 여부를 나타내는 부울을 유지 해야 합니다. 이 부울이 false 인 경우 `SendRegistrationToAppServer` 토큰에서 앱 서버로 보내는 &ndash; 토큰 이전 호출에서 앱 서버로 이미 전송 된이 고, 그렇지 합니다. 


##### <a name="subscribe-to-the-notification-topic"></a>알림 항목 구독

호출 다음에 `Subscribe` 을 나타내기 위해 메서드에 알림 토픽을 구독 하려는 GCM에 있습니다. `Subscribe`를 호출 합니다 [GcmPubSub.Subscribe](https://developers.google.com/android/reference/com/google/android/gms/gcm/GcmPubSub.html#subscribe&#40;java.lang.String,%20java.lang.String,%20android.os.Bundle&#41;) API 아래에 있는 모든 메시지에 클라이언트 앱을 구독할 `/topics/global`:

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

앱 서버 알림 메시지를 보내야 `/topics/global` 를 받을 경우. 참고 항목에서 이름을 지정 하는 `/topics` 이러한 이름에 동의 하는 앱 서버 및 클라이언트 앱으로, 원하는 대로 지정할 수 있습니다. (이름을 당사가 여기서 `global` 앱 서버에서 지 원하는 모든 항목에 메시지를 수신 하려고 한다는 것을 나타냅니다.) 

서버 쪽에서 메시징 GCM 항목에 대 한 자세한 내용은 참조 Google [항목에 메시지 보내기](https://developers.google.com/cloud-messaging/topic-messaging)합니다. 


#### <a name="implement-an-instance-id-listener-service"></a>인스턴스 ID 수신기 서비스를 구현 합니다.

등록 토큰의 고유 성과 보안; 그러나 클라이언트 앱 (또는 GCM) 해야 할 수 앱 다시 설치 또는 보안 문제가 발생할 경우 등록 토큰을 새로 고칩니다. 따라서 구현 해야 합니다는 `InstanceIdListenerService` GCM에서 토큰 새로 고침 요청에 응답 합니다. 

이라는 새 파일 추가 **InstanceIdListenerService.cs** 템플릿 코드를 다음으로 바꿉니다. 

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

주석 달기 `InstanceIdListenerService` 서비스가 시스템에서 인스턴스화할 필요가 있는지 및 GCM 등록 토큰을 받을 수 있는지를 나타내는 특성을 사용 하 여 (라고도 *인스턴스 ID*) 새로 고침 요청: 

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

합니다 `OnTokenRefresh` 서비스에서 메서드 시작의 `RegistrationIntentService` 새 등록 토큰을 가로챌 수 있도록 합니다.


#### <a name="test-registration-with-gcm"></a>GCM 사용 하 여 테스트 등록

보겠습니다 완전히 다시 빌드하고 앱을 실행 합니다. GCM 등록 토큰을 성공적으로 받게 등록 토큰을 출력 창에 표시 됩니다. 예를 들어: 

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>다운스트림 메시지 처리 

지금까지 구현한 코드는 "설정" 코드만; Google Play Services가 설치 되어 있고 원격 알림을 수신 하기 위한 클라이언트 앱을 준비 하려면 GCM 및 앱 서버와 협상 하는 경우 참조를 확인 합니다. 그러나 아직 실제로 수신 하 고 다운스트림 알림 메시지를 처리 하는 코드를 구현 합니다. 이 위해 구현 해야 합니다는 *GCM 수신기 서비스*합니다. 이 서비스는 앱 서버에서 항목 메시지를 수신 하 고 로컬로 해당 알림으로 브로드캐스트합니다. 이 서비스를 구현 하는 후에 메시지를 보낼 GCM 구현 올바르게 작동 하는 경우 볼 수 있도록 테스트 프로그램을 만듭니다. 


#### <a name="add-a-notification-icon"></a>알림 아이콘 추가

이 알림 시작 되 면 알림 영역에 나타나는 작은 아이콘을 먼저 추가 해 보겠습니다. 복사할 수 있습니다 [이 아이콘](remote-notifications-with-gcm-images/ic-stat-ic-notification.png) 프로젝트를 만들거나 사용자 고유의 사용자 지정 아이콘입니다. 아이콘 파일 이름을 **ic_stat_button_click.png** 에 복사 합니다 **리소스/drawable** 폴더입니다. 사용 하 여 **추가 > 기존 항목...**  프로젝트에서이 아이콘 파일을 포함 하도록 합니다.


#### <a name="implement-a-gcm-listener-service"></a>GCM 수신기 서비스 구현

이라는 새 파일 추가 **GcmListenerService.cs** 템플릿 코드를 다음으로 바꿉니다.

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

각 섹션에 살펴보겠습니다 우리의 `GcmListenerService` 작동 방식을 이해 합니다. 

에서는 주석을 추가 하는 먼저 `GcmListenerService` 나타내고이 서비스는 시스템에서 인스턴스화할 필요가 GCM 메시지를 수신 하는 것을 나타내려면 의도 필터를 포함 하는 특성으로: 

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

때 `GcmListenerService` GCM에서 메시지를 수신 합니다 `OnMessageReceived` 메서드가 실행 됩니다. 이 메서드는 전달 기능에서 메시지 콘텐츠를 추출 `Bundle`, 메시지 내용 (따라서 출력 창에서 볼 수 것), 로그 및 호출 `SendNotification` 로컬 알림을 받은 메시지 콘텐츠를 사용 하 여 시작 하려면: 

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

`SendNotification` 메서드 `Notification.Builder` 알림을 한 다음 만들기를 사용 하는 `NotificationManager` 알림을 시작 합니다. 효과적으로이 사용자에 게 표시 되는 로컬 알림으로 원격 알림 메시지를 변환 합니다.
사용에 대 한 자세한 내용은 `Notification.Builder` 하 고 `NotificationManager`를 참조 하세요 [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)합니다.


#### <a name="declare-the-receiver-in-the-manifest"></a>매니페스트에서 수신기를 선언 합니다.

GCM에서 메시지를 받을 수 있습니다, 전에 GCM 수신기 Android 매니페스트에서 선언 해야 합니다. 편집할 **AndroidManifest.xml** 바꾸고는 `<application>` 다음 XML 사용 하 여 섹션: 

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

위의 xml에서 변경할 *YOUR_PACKAGE_NAME* 클라이언트 앱 프로젝트에 대 한 패키지 이름. 연습 예제에서는 패키지 이름은 `com.xamarin.gcmexample`합니다. 

이 XML에서 각 설정에 대해 살펴보겠습니다.

|설정|설명|
|---|---|
|`com.google.android.gms.gcm.GcmReceiver`|앱 캡처하고 들어오는 푸시 알림 메시지를 처리 하는 GCM 수신기를 구현 함을 선언 합니다.|
|`com.google.android.c2dm.permission.SEND`|GCM 서버 에서만 앱에 직접 메시지를 보낼 수 있음을 선언 합니다.|
|`com.google.android.c2dm.intent.RECEIVE`|앱이 GCM에서 브로드캐스트 메시지를 처리는 광고 의도 필터입니다.|
|`com.google.android.c2dm.intent.REGISTRATION`|앱에서 새 등록 의도 처리 하는 광고 의도 필터 (즉, 우리가 구현한 인스턴스 ID 수신기 서비스를).|

데코레이팅 할 수 있습니다 또는 `GcmListenerService` XML;에 지정 하는 것이 아니라 이러한 특성을 사용 하 여 여기서 지정 하 **AndroidManifest.xml** 코드 샘플을 쉽게 수행할 수 있도록 합니다. 


### <a name="create-a-message-sender-to-test-the-app"></a>앱을 테스트 하는 메시지 보낸 사람 만들기

추가 해 보겠습니다는 C# 데스크톱 콘솔 응용 프로그램 프로젝트를 솔루션을 호출할 **MessageSender**합니다. 이 콘솔 응용 프로그램 사용 하 여 응용 프로그램 서버를 시뮬레이션 &ndash; 알림 메시지를 전송할 **ClientApp** GCM을 통해. 


#### <a name="add-the-jsonnet-package"></a>Json.NET 패키지 추가

이 콘솔 응용 프로그램에서 클라이언트 앱에 전송할 알림 메시지를 포함 하는 JSON 페이로드를 구축 하 고 있습니다. 사용 하 여 합니다 **Json.NET** 패키지 **MessageSender** 손쉽게 GCM에서 필요한 JSON 개체를 만들 수 있도록 합니다. Visual Studio에서 마우스 오른쪽 단추로 클릭 **참조 > NuGet 패키지 관리...** Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가...** . 

검색 된 **Json.NET** 패키지 및 프로젝트에 설치 합니다. 

[![Json.NET 패키지 설치](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)


#### <a name="add-a-reference-to-systemnethttp"></a>System.Net.Http에 대 한 참조를 추가 합니다.

에 대 한 참조를 추가 해야 했습니다 `System.Net.Http` 인스턴스화할 수 있도록는 `HttpClient` GCM에 테스트 메시지를 전송 합니다. 에 **MessageSender** 프로젝트를 마우스 오른쪽 단추로 클릭 **참조 > 참조 추가** 아래로 스크롤하여 표시 될 때까지 **System.Net.Http**합니다. 옆에 확인 표시를 추가할 **System.Net.Http** 누릅니다 **확인**합니다. 


#### <a name="implement-code-that-sends-a-test-message"></a>테스트 메시지를 전송 하는 코드를 구현 합니다.

**MessageSender**을 편집 **Program.cs** 그 내용을 다음 코드로 바꿉니다.

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

위의 코드에서 변경할 *YOUR_API_KEY* 클라이언트 앱 프로젝트에 대 한 API 키입니다. 

이 테스트 앱 서버는 GCM에 다음 JSON 형식의 메시지를 보냅니다.

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

따라서 GCM 클라이언트 앱에이 메시지를 전달합니다. 빌드 해 보겠습니다 **MessageSender** 명령줄에서 수행할 수 있는 콘솔 창을 엽니다.



### <a name="try-it"></a>실습

이제 클라이언트 앱을 테스트할 준비가 되었습니다. 에뮬레이터를 사용 하는 경우 또는 장치는 GCM Wi-fi 상에 서와 통신 하는 경우 다음 TCP 포트를 통해 GCM 메시지에 대 한 방화벽에서 열어야: 5228, 5229, 및 5230 합니다.

클라이언트 앱을 시작 하 고 출력 창. 이후에 `RegistrationIntentService` 등록을 성공적으로 수신 GCM에서 토큰을 출력 창에서 표시 해야 토큰 다음과 같은 로그 출력을 사용 하 여:

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

이 시점에서 클라이언트 앱이 원격 알림 메시지를 받을 준비가 됩니다. 명령줄에서 실행 합니다 **MessageSender.exe** 프로그램 클라이언트 앱에 "Hello, Xamarin" 알림 메시지를 보내려고 합니다.
아직 빌드되지 경우 합니다 **MessageSender** 프로젝트를 지금 만듭니다.

실행할 **MessageSender.exe** Visual Studio에서 명령 프롬프트를 열고 합니다 **MessageSender/bin/Debug** 디렉터리와 직접 명령 실행:

```cmd
MessageSender.exe
```

실행할 **MessageSender.exe** Mac 용 Visual Studio에서 터미널 세션을 열고 **MessageSender/bin/Debug** 디렉터리와 실행을 사용 하 여 mono **MessageSender.exe** 

```bash
mono MessageSender.exe
```

GCM 및 클라이언트 앱으로 다시 전달할 메시지에 대 일 분 걸릴 수 있습니다. 메시지가 성공적으로 수신 되 면 출력 창에 다음과 같은 출력이 나타납니다 했습니다. 

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

또한 새 알림 아이콘에 알림 표시줄에 나타나는 유의 해야 합니다. 

[![장치에서 알림 아이콘이 표시 됩니다.](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

알림을 보려면 알림 표시줄을 열면이 원격 알림을 표시 됩니다.

[![알림 메시지가 표시 됩니다.](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

축, 앱의 첫 번째 원격 알림을 받았습니다.

Note 앱이 강제로 중지 하는 경우 GCM 메시지가 수신 되지 않습니다. 알림을 강제로 중지 후 다시 시작 하려면 앱을 수동으로 다시 시작 해야 합니다. 이 Android 정책에 대 한 자세한 내용은 참조 하세요. [중지 된 응용 프로그램에서 컨트롤을 시작](https://developer.android.com/about/versions/android-3.1.html#launchcontrols) 이 고 [stack overflow 게시물](http://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267)합니다. 

 
## <a name="summary"></a>요약

이 연습에서는 Xamarin.Android 응용 프로그램에서 원격 알림을 구현 하기 위한 단계를 자세히 설명 합니다. GCM 통신에 필요한 추가 패키지를 설치 하는 방법을 설명 하 고 GCM 서버에 대 한 액세스에 대 한 앱 사용 권한을 구성 하는 방법을 설명 했습니다. Google Play 서비스의 존재 여부 확인 하는 방법, 내재 된 서비스 등록 및 gcm 등록 토큰에 대 한 협상 하는 인스턴스 ID 수신기 서비스를 구현 하는 방법 및 GCM 수신기를 구현 하는 방법을 보여 주는 예제 코드 제공 수신 하 고 원격 알림 메시지를 처리 하는 서비스입니다. 마지막으로, GCM 통해 클라이언트 앱에 테스트 알림을 보낼 명령줄 테스트 프로그램을 구현 했습니다. 


## <a name="related-links"></a>관련 링크

- [GCM RemoteNotifications (샘플)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications)
- [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
