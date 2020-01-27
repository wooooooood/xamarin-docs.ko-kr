---
title: Google Cloud Messaging로 원격 알림
description: 이 연습에서는 Xamarin Android 응용 프로그램에서 Google Cloud Messaging 사용 하 여 원격 알림 (푸시 알림)을 구현 하는 방법에 대 한 단계별 설명을 제공 합니다. GCM (Google Cloud Messaging와 통신 하기 위해 구현 해야 하는 다양 한 클래스에 대해 설명 하 고 GCM에 액세스 하기 위해 Android 매니페스트에서 사용 권한을 설정 하는 방법에 대해 설명 하 고 샘플 테스트 프로그램을 사용 하 여 종단 간 메시징을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/02/2019
ms.openlocfilehash: b621d61584cc39f669c662db3d1df264d9a92eb9
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723774"
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Google Cloud Messaging로 원격 알림

> [!WARNING]
> Google이 사용 되지 않는 GCM-4 월 10 일, 2018. 다음 문서 및 샘플 프로젝트는 더 이상 유지 되지 않을 수 있습니다. Google의 GCM 서버 및 클라이언트 Api는 2019 년 5 월 29 일에 곧 제거 될 예정입니다. Google은 GCM 앱을 FCM (Firebase Cloud Messaging)로 마이그레이션하는 것을 권장 합니다. GCM 사용 중단 및 마이그레이션에 대 한 자세한 내용은 [Google Cloud Messaging-사용 되지 않음](https://developers.google.com/cloud-messaging/)을 참조 하세요.
>
> Xamarin에서 Firebase Cloud Messaging을 사용 하 여 원격 알림을 시작 하려면 [FCM를 사용 하 여 원격 알림](remote-notifications-with-fcm.md)을 참조 하세요.

_이 연습에서는 Xamarin Android 응용 프로그램에서 Google Cloud Messaging 사용 하 여 원격 알림 (푸시 알림)을 구현 하는 방법에 대 한 단계별 설명을 제공 합니다. GCM (Google Cloud Messaging와 통신 하기 위해 구현 해야 하는 다양 한 클래스에 대해 설명 하 고 GCM에 액세스 하기 위해 Android 매니페스트에서 사용 권한을 설정 하는 방법에 대해 설명 하 고 샘플 테스트 프로그램을 사용 하 여 종단 간 메시징을 보여 줍니다._

## <a name="gcm-notifications-overview"></a>GCM 알림 개요

이 연습에서는 Google Cloud Messaging (GCM)를 사용 하 여 원격 알림을 구현 하는 Xamarin Android 응용 프로그램 ( *푸시 알림이*라고도 함)을 만듭니다. 원격 메시징의 GCM을 사용 하는 다양 한 의도 및 수신기 서비스를 구현 하 고 응용 프로그램 서버를 시뮬레이트하는 명령줄 프로그램을 사용 하 여 구현을 테스트 합니다.

이 연습을 진행 하려면 먼저 Google의 GCM 서버를 사용 하는 데 필요한 자격 증명을 얻어야 합니다. 이 프로세스는 [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)에 설명 되어 있습니다.
특히이 연습에서 제시 하는 예제 코드에 삽입할 *API 키* 및 *보낸 사람 ID* 가 필요 합니다.

GCM 사용 Xamarin Android 클라이언트 앱을 만들려면 다음 단계를 사용 합니다.

1. GCM 서버와 통신 하는 데 필요한 추가 패키지를 설치 합니다.
2. GCM 서버에 액세스 하기 위한 앱 사용 권한을 구성 합니다.
3. Google Play 서비스 있는지 확인 하는 코드를 구현 합니다.
4. 등록 토큰에 대해 GCM과 협상 하는 등록 의도 서비스를 구현 합니다.
5. GCM에서 등록 토큰 업데이트를 수신 하는 인스턴스 ID 수신기 서비스를 구현 합니다.
6. GCM을 통해 앱 서버에서 원격 메시지를 수신 하는 GCM 수신기 서비스를 구현 합니다.

이 앱은 *토픽 메시징*이라는 새로운 GCM 기능을 사용 합니다. 토픽 메시지에서 앱 서버는 개별 장치 목록이 아니라 토픽으로 메시지를 보냅니다. 해당 항목을 구독 하는 장치는 토픽 메시지를 푸시 알림으로 받을 수 있습니다.

클라이언트 앱이 준비 되 면 GCM을 통해 클라이언트 앱에 푸시 알림을 C# 전송 하는 명령줄 응용 프로그램을 구현 합니다.

## <a name="walkthrough"></a>연습

먼저 **RemoteNotifications**이라는 비어 있는 새 솔루션을 만들어 보겠습니다. 다음으로 **Android 앱** 템플릿을 기반으로 하는이 솔루션에 새 android 프로젝트를 추가 해 보겠습니다. 이 프로젝트를 **ClientApp**라고 하겠습니다. Xamarin Android 프로젝트를 만드는 방법을 잘 모르는 경우 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)를 참조 하세요. **ClientApp** 프로젝트에는 GCM을 통해 원격 알림을 받는 Xamarin Android 클라이언트 응용 프로그램에 대 한 코드가 포함 됩니다.

### <a name="add-required-packages"></a>필수 패키지 추가

클라이언트 앱 코드를 구현할 수 있으려면 GCM과의 통신에 사용할 여러 패키지를 설치 해야 합니다. 또한 아직 설치 되지 않은 경우에는 Google Play 스토어 응용 프로그램을 장치에 추가 해야 합니다.

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Xamarin Google Play 서비스 GCM 패키지를 추가 합니다.

Google Cloud Messaging에서 메시지를 수신 하려면 장치에 [Google Play 서비스](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/) framework가 있어야 합니다. 이 프레임 워크가 없으면 Android 응용 프로그램이 GCM 서버에서 메시지를 수신할 수 없습니다. Google Play 서비스는 Android 장치의 전원이 켜져 있고 GCM의 메시지를 자동으로 수신 대기 하는 동안 백그라운드에서 실행 됩니다. 이러한 메시지가 도착할 때 Google Play 서비스는 메시지를로 변환 하 고 이러한 의도를 등록 된 응용 프로그램에 브로드캐스트합니다.

Visual Studio에서 참조를 마우스 오른쪽 단추로 클릭 **> NuGet 패키지 관리 ...** ; Mac용 Visual Studio에서 패키지를 마우스 오른쪽 단추로 클릭 하 **> 패키지 추가**...를 클릭 합니다. **Xamarin Google Play 서비스-GCM** 을 검색 하 고이 패키지를 **ClientApp** 프로젝트에 설치 합니다.

[Google Play 서비스 설치 ![](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

**Xamarin Google Play 서비스-GCM**을 설치 하면 **Xamarin Google Play 서비스-Base** 가 자동으로 설치 됩니다. 오류가 발생 하는 경우 프로젝트의 *최소 Android 대상* 설정을 **SDK 버전을 사용 하 여 컴파일** 이외의 값으로 변경 하 고 NuGet 설치를 다시 시도 합니다.

그런 다음 **MainActivity.cs** 를 편집 하 고 다음 `using` 문을 추가 합니다.

```csharp
using Android.Gms.Common;
using Android.Util;
```

이렇게 하면 Google Play 서비스 GMS 패키지의 형식을 코드에서 사용할 수 있으며 GMS를 사용 하 여 트랜잭션을 추적 하는 데 사용할 로깅 기능을 추가 합니다.

#### <a name="google-play-store"></a>Google Play 스토어

GCM에서 메시지를 수신 하려면 Google Play 스토어 응용 프로그램이 장치에 설치 되어 있어야 합니다. Google Play 응용 프로그램이 장치에 설치 될 때마다 Google Play 스토어 설치 되어 테스트 장치에 이미 설치 되어 있을 수 있습니다. Google Play 하지 않으면 Android 응용 프로그램이 GCM에서 메시지를 받을 수 없습니다.
장치에 Google Play 스토어 앱을 아직 설치 하지 않은 경우 [Google Play](https://support.google.com/googleplay) 웹 사이트를 방문 하 여 Google Play를 다운로드 하 고 설치 합니다.

또는 테스트 장치 대신 Android 2.2 이상을 실행 하는 Android 에뮬레이터를 사용할 수 있습니다 (Android 에뮬레이터에 Google Play 스토어을 설치할 필요 없음). 그러나 에뮬레이터를 사용 하는 경우에는 Wi-fi를 사용 하 여 GCM에 연결 해야 하며,이 연습의 뒷부분에 설명 된 대로 Wi-fi 방화벽에서 여러 포트를 열어야 합니다.

### <a name="set-the-package-name"></a>패키지 이름 설정

[Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)에서 GCM 사용 앱에 대 한 패키지 이름을 지정 했습니다 .이 패키지 이름은 API 키 및 보낸 사람 id와 연결 된 *응용 프로그램 id* 로도 사용 됩니다. **ClientApp** 프로젝트에 대 한 속성을 열고 패키지 이름을이 문자열로 설정 하겠습니다. 이 예제에서는 패키지 이름을 `com.xamarin.gcmexample`로 설정 합니다.

[패키지 이름을 설정 하는 ![](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

이 패키지 이름이 Google Developer console에 입력 한 패키지 이름과 *정확 하 게* 일치 하지 않으면 클라이언트 앱은 GCM에서 등록 토큰을 받을 수 없습니다.

### <a name="add-permissions-to-the-android-manifest"></a>Android 매니페스트에 권한 추가

Android 응용 프로그램은 Google Cloud Messaging에서 알림을 수신 하기 전에 다음 권한이 구성 되어 있어야 합니다.

- `com.google.android.c2dm.permission.RECEIVE` &ndash;는 앱에 Google Cloud Messaging 메시지를 등록 하 고 받을 수 있는 권한을 부여 합니다. `c2dm`은 무엇을 의미 하나요? 이는 현재 사용 되지 않는 최신 선행 작업 인 _클라우드-장치 메시징을_의미 합니다.
    GCM은 아직 많은 사용 권한 문자열에 `c2dm`를 사용 합니다.)

- `android.permission.WAKE_LOCK` &ndash; (선택 사항)는 메시지를 수신 대기 하는 동안 장치 CPU가 절전 모드로 전환 되지 않도록 합니다.

- `android.permission.INTERNET` &ndash;는 인터넷 액세스를 허용 하므로 클라이언트 앱이 GCM과 통신할 수 있습니다.

- *package_name* &ndash;`.permission.C2D_MESSAGE`는 Android를 사용 하 여 응용 프로그램을 등록 하 고 모든 C2D (클라우드-장치) 메시지를 독점적으로 수신 하는 권한을 요청 합니다. *Package_name* 접두사는 응용 프로그램 ID와 동일 합니다.

Android 매니페스트에서 이러한 사용 권한을 설정 합니다. **Androidmanifest** 을 편집 하 고 내용을 다음 xml로 바꿉니다.

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

위의 XML에서 *YOUR_PACKAGE_NAME* 를 클라이언트 앱 프로젝트의 패키지 이름으로 변경 합니다. 예: `com.xamarin.gcmexample`.

### <a name="check-for-google-play-services"></a>Google Play 서비스 확인

이 연습에서는 UI의 단일 `TextView`를 사용 하 여 응용 프로그램 뼈를 만듭니다. 이 앱은 GCM과의 상호 작용을 직접 나타내지 않습니다. 대신, 출력 창을 시청 하 여 앱에서 GCM을 사용 하는 방법을 확인 하 고 알림 트레이가 도착할 때 새 알림이 있는지 확인 합니다.

먼저 메시지 영역에 대 한 레이아웃을 만들어 보겠습니다. **리소스** 를 편집 하 고 내용을 다음 xml로 바꿉니다.

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

**Main. axml** 을 저장 하 고 닫습니다.

클라이언트 앱이 시작 되 면 GCM에 연결 하기 전에 Google Play 서비스를 사용할 수 있는지 확인 하려고 합니다. **MainActivity.cs** 를 편집 하 고 ``count`` instance 변수 선언을 다음 인스턴스 변수 선언으로 바꿉니다.

```csharp
TextView msgText;
```

다음으로 **Mainactivity** 클래스에 다음 메서드를 추가 합니다.

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

이 코드는 장치에서 Google Play 서비스 APK가 설치 되어 있는지 확인 합니다. 설치 되지 않은 경우 사용자가 Google Play 스토어에서 APK를 다운로드 하거나 장치의 시스템 설정에서 사용 하도록 지시 하는 메시지가 메시지 영역에 표시 됩니다. 클라이언트 앱이 시작 될 때이 검사를 실행 하려고 하므로 `OnCreate`끝에이 메서드에 대 한 호출을 추가 합니다.

다음으로 `OnCreate` 메서드를 다음 코드로 바꿉니다.

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();
}
```

이 코드는 Google Play 서비스 APK가 있는지 확인 하 고 결과를 메시지 영역에 씁니다.

앱을 완전히 다시 빌드하고 실행 해 보겠습니다. 다음 스크린샷 처럼 표시 되는 화면이 표시 됩니다.

[![Google Play 서비스를 사용할 수 있습니다.](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

이 결과를 얻지 못한 경우 Google Play 서비스 APK가 장치에 설치 되어 있는지 확인 하 고, 이전에 설명한 대로 **Xamarin Google Play 서비스-GCM** 패키지가 **ClientApp** 프로젝트에 추가 되었는지 확인 합니다. 빌드 오류가 발생 하는 경우 솔루션을 정리 하 고 프로젝트를 다시 빌드합니다.

다음에는 GCM에 연결 하 고 등록 토큰을 다시 가져오는 코드를 작성 합니다.

### <a name="register-with-gcm"></a>GCM에 등록

앱은 앱 서버에서 원격 알림을 받을 수 있기 전에 GCM에 등록 하 고 등록 토큰을 다시 가져와야 합니다. GCM을 사용 하 여 응용 프로그램을 등록 하는 작업은 만든 `IntentService`에 의해 처리 됩니다. `IntentService`는 다음 단계를 수행 합니다.

1. 는 [InstanceID](https://developers.google.com/instance-id/) API를 사용 하 여 클라이언트 앱이 앱 서버에 액세스 하도록 권한을 부여 하는 보안 토큰을 생성 합니다. 반환 시 GCM에서 등록 토큰을 가져옵니다.

2. 앱 서버에 등록 토큰을 전달 합니다 (앱 서버에 필요한 경우).

3. 하나 이상의 알림 토픽 채널을 구독 합니다.

이 `IntentService`를 구현한 후에는 GCM에서 등록 토큰이 반환 되는지 테스트 합니다.

**RegistrationIntentService.cs** 라는 새 파일을 추가 하 고 템플릿 코드를 다음으로 바꿉니다.

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

위의 샘플 코드에서 *YOUR_SENDER_ID* 를 클라이언트 앱 프로젝트의 보낸 사람 ID 번호로 변경 합니다. 프로젝트의 보낸 사람 ID를 가져오려면 다음을 수행 합니다.

1. [Google Cloud Console](https://console.cloud.google.com/) 에 로그인 하 고 풀 다운 메뉴에서 프로젝트 이름을 선택 합니다. 프로젝트에 대해 표시 되는 **프로젝트 정보** 창에서 **프로젝트 설정으로 이동**을 클릭 합니다.

    [XamarinGCM 프로젝트 ![선택](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2. **설정** 페이지에서 프로젝트의 보낸 사람 ID &ndash; **프로젝트 번호** 를 찾습니다.

    [표시 된 ![프로젝트 번호](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

앱이 실행 되기 시작 하면 `RegistrationIntentService`을 시작 하려고 합니다. **MainActivity.cs** 를 편집 하 고 Google Play 서비스 있는지 확인 한 후 `RegistrationIntentService` 시작 되도록 `OnCreate` 메서드를 수정 합니다.

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

이제 `RegistrationIntentService`의 각 섹션을 살펴보고 어떻게 작동 하는지 살펴보겠습니다.

먼저, 다음 특성을 사용 하 여 `RegistrationIntentService`에 주석을 추가 하 여 서비스가 시스템에서 인스턴스화되지 않도록 지정 합니다.

```csharp
[Service (Exported = false)]
```

`RegistrationIntentService` 생성자는 디버깅을 용이 하 게 하기 위해 작업자 스레드 *Registrationintentservice* 의 이름을 지정 합니다.

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

`RegistrationIntentService`의 핵심 기능은 `OnHandleIntent` 메서드에 있습니다. 이 코드를 살펴보고 GCM을 사용 하 여 앱을 등록 하는 방법을 살펴봅니다.

#### <a name="request-a-registration-token"></a>등록 토큰 요청

`OnHandleIntent` 먼저 Google의 [InstanceID. GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) 메서드를 호출 하 여 GCM에서 등록 토큰을 요청 합니다. 이러한 의도를 순차적으로 처리 하도록 `lock` &ndash; 동시에 여러 등록 의도를 발생 시킬 수 있는 가능성을 방지 하기 위해이 코드를 `lock` 래핑합니다. 등록 토큰을 가져오지 못하면 예외가 throw 되 고 오류를 기록 합니다. 등록이 성공 하면 GCM에서 반환 된 등록 토큰으로 `token` 설정 됩니다.

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

#### <a name="forward-the-registration-token-to-the-app-server"></a>앱 서버에 등록 토큰 전달

등록 토큰을 가져오는 경우 (예외가 throw 되지 않음) `SendRegistrationToAppServer`을 호출 하 여 사용자의 등록 토큰을 응용 프로그램에 의해 유지 관리 되는 서버 쪽 계정 (있는 경우)에 연결 합니다. 이 구현은 앱 서버 디자인에 따라 달라 지므로 다음과 같은 빈 방법이 제공 됩니다.

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

경우에 따라 앱 서버에 사용자의 등록 토큰이 필요 하지 않습니다. 이 경우에는이 메서드를 생략할 수 있습니다. 등록 토큰이 앱 서버에 전송 되는 경우 토큰이 서버에 전송 되었는지 여부를 나타내는 부울을 유지 해야 `SendRegistrationToAppServer`. 이 부울이 false 인 경우 `SendRegistrationToAppServer`는 앱 &ndash; 서버에 토큰을 보내고, 그렇지 않으면 이전 호출에서 토큰을 이미 앱 서버에 보냈습니다.

#### <a name="subscribe-to-the-notification-topic"></a>알림 항목 구독

다음으로 `Subscribe` 메서드를 호출 하 여 알림 항목을 구독 하려는 GCM을 표시 합니다. `Subscribe`에서 GcmPubSub. 구독 API를 호출 하 여 클라이언트 앱을 `/topics/global`의 모든 메시지에 구독 합니다.

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

앱 서버는 알림 메시지를 수신 하는 경우 `/topics/global`에이 메시지를 보내야 합니다. 앱 서버와 클라이언트 앱이 모두 이러한 이름에 동의 하는 한 `/topics` 아래의 토픽 이름은 원하는 대로 지정할 수 있습니다. 여기서는 `global` 이름을 선택 하 여 앱 서버에서 지 원하는 모든 토픽에서 메시지를 받을지 여부를 표시 합니다.)

#### <a name="implement-an-instance-id-listener-service"></a>인스턴스 ID 수신기 서비스 구현

등록 토큰은 고유 하 고 안전 합니다. 그러나 클라이언트 앱 (또는 GCM)은 앱을 다시 설치 하거나 보안 문제가 발생 하는 경우 등록 토큰을 새로 고쳐야 할 수도 있습니다. 이러한 이유로 GCM에서 토큰 새로 고침 요청에 응답 하는 `InstanceIdListenerService`를 구현 해야 합니다.

**InstanceIdListenerService.cs** 라는 새 파일을 추가 하 고 템플릿 코드를 다음으로 바꿉니다.

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

다음 특성을 사용 하 `InstanceIdListenerService`에 주석을 추가 하 여 서비스가 시스템에서 인스턴스화되지 않고 GCM 등록 토큰 ( *인스턴스 ID*라고도 함) 새로 고침 요청을 받을 수 있음을 표시 합니다.

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

서비스의 `OnTokenRefresh` 메서드는 새 등록 토큰을 가로챌 수 있도록 `RegistrationIntentService`를 시작 합니다.

#### <a name="test-registration-with-gcm"></a>GCM을 사용한 테스트 등록

앱을 완전히 다시 빌드하고 실행 해 보겠습니다. GCM에서 등록 토큰을 성공적으로 수신 하면 출력 창에 등록 토큰이 표시 되어야 합니다. 예를 들면 다음과 같습니다.:

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>다운스트림 메시지 처리

지금까지 구현한 코드는 "설정" 코드에 불과합니다. Google Play 서비스 설치 되 고 GCM 및 앱 서버와 협상 하 여 원격 알림을 받도록 클라이언트 앱을 준비 하는지 확인 합니다. 그러나 실제로 다운스트림 알림 메시지를 받고 처리 하는 코드를 구현 해야 합니다. 이렇게 하려면 *GCM 수신기 서비스*를 구현 해야 합니다. 이 서비스는 앱 서버에서 토픽 메시지를 수신 하 고이를 알림으로 로컬로 브로드캐스트합니다. 이 서비스를 구현한 후에는 구현이 제대로 작동 하는지 확인할 수 있도록 GCM에 메시지를 보내는 테스트 프로그램을 만듭니다.

#### <a name="add-a-notification-icon"></a>알림 추가 아이콘

먼저 알림이 시작 될 때 알림 영역에 표시 되는 작은 아이콘을 추가 해 보겠습니다. [이 아이콘](remote-notifications-with-gcm-images/ic-stat-ic-notification.png) 을 프로젝트에 복사 하거나 사용자 지정 아이콘을 직접 만들 수 있습니다. 아이콘 파일의 이름을 **ic_stat_button_click .png** 로 표시 하 고 **리소스/그릴** 수 있는 폴더에 복사 합니다. **> 기존 항목 추가** ...를 사용 하 여 프로젝트에이 아이콘 파일을 포함 해야 합니다.

#### <a name="implement-a-gcm-listener-service"></a>GCM 수신기 서비스 구현

**GcmListenerService.cs** 라는 새 파일을 추가 하 고 템플릿 코드를 다음으로 바꿉니다.

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

`GcmListenerService`의 각 섹션을 살펴보고 어떻게 작동 하는지 살펴보겠습니다.

먼저 시스템에서이 서비스를 인스턴스화하지 않음을 나타내는 특성으로 `GcmListenerService`에 주석을 추가 하 고 GCM 메시지를 수신 함을 나타내는 의도 필터를 포함 합니다.

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

GCM에서 메시지를 수신 하는 `GcmListenerService` `OnMessageReceived` 메서드가 호출 됩니다. 이 메서드는 전달 된 `Bundle`에서 메시지 콘텐츠를 추출 하 고, 메시지 콘텐츠를 기록 하 여 (출력 창에서 볼 수 있도록), `SendNotification`를 호출 하 여 받은 메시지 콘텐츠로 로컬 알림을 시작할 수 있습니다.

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

`SendNotification` 메서드는 `Notification.Builder`를 사용 하 여 알림을 만든 다음 `NotificationManager`를 사용 하 여 알림을 시작 합니다. 이렇게 하면 원격 알림 메시지를 사용자에 게 표시 되는 로컬 알림으로 변환 합니다.
`Notification.Builder` 및 `NotificationManager`사용에 대 한 자세한 내용은 [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)을 참조 하세요.

#### <a name="declare-the-receiver-in-the-manifest"></a>매니페스트에서 수신기를 선언 합니다.

GCM에서 메시지를 받으려면 먼저 Android 매니페스트에서 GCM 수신기를 선언 해야 합니다. **Androidmanifest .xml** 을 편집 하 고 `<application>` 섹션을 다음 xml로 바꿉니다.

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

위의 XML에서 *YOUR_PACKAGE_NAME* 를 클라이언트 앱 프로젝트의 패키지 이름으로 변경 합니다. 연습 예제에서는 패키지 이름이 `com.xamarin.gcmexample`됩니다.

이 XML의 각 설정에서 수행 하는 작업을 살펴보겠습니다.

|설정|설명|
|---|---|
|`com.google.android.gms.gcm.GcmReceiver`|앱이 들어오는 푸시 알림 메시지를 캡처하고 처리 하는 GCM 수신기를 구현 하도록 선언 합니다.|
|`com.google.android.c2dm.permission.SEND`|GCM 서버만 앱에 직접 메시지를 보낼 수 있음을 선언 합니다.|
|`com.google.android.c2dm.intent.RECEIVE`|앱이 GCM의 브로드캐스트 메시지를 처리 한다는 의도 필터 광고입니다.|
|`com.google.android.c2dm.intent.REGISTRATION`|앱이 새로운 등록 의도를 처리 한다는 (즉, 인스턴스 ID 수신기 서비스를 구현 하 여) 의도 필터를 알립니다.|

또는 이러한 특성을 XML로 지정 하지 않고 `GcmListenerService` 데코레이팅 할 수 있습니다. 여기서는 코드 샘플을 보다 쉽게 수행할 수 있도록 **Androidmanifest** 에서 지정 합니다.

### <a name="create-a-message-sender-to-test-the-app"></a>메시지 보낸 사람을 만들어 앱 테스트

C# 데스크톱 콘솔 응용 프로그램 프로젝트를 솔루션에 추가 하 고 **MessageSender**를 호출 해 보겠습니다. 이 콘솔 응용 프로그램을 사용 하 여 응용 프로그램 &ndash; 서버를 시뮬레이트하여 GCM을 통해 **ClientApp** 에 게 알림 메시지를 전송 합니다.

#### <a name="add-the-jsonnet-package"></a>Json.NET 패키지 추가

이 콘솔 앱에서 클라이언트 앱에 보낼 알림 메시지를 포함 하는 JSON 페이로드를 작성 하 고 있습니다. **MessageSender** 에서 **Json.NET** 패키지를 사용 하 여 GCM에 필요한 Json 개체를 더 쉽게 빌드할 수 있습니다. Visual Studio에서 참조를 마우스 오른쪽 단추로 클릭 **> NuGet 패키지 관리 ...** ; Mac용 Visual Studio에서 패키지를 마우스 오른쪽 단추로 클릭 하 **> 패키지 추가**...를 클릭 합니다.

**Json.NET** 패키지를 검색 하 여 프로젝트에 설치 해 보겠습니다.

[Json.NET 패키지를 설치 하는 ![](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)

#### <a name="add-a-reference-to-systemnethttp"></a>시스템 .Net에 대 한 참조를 추가 합니다. Http

또한 테스트 메시지를 GCM으로 보내기 위한 `HttpClient`를 인스턴스화할 수 있도록 `System.Net.Http`에 대 한 참조를 추가 해야 합니다. **MessageSender** 프로젝트에서 참조를 마우스 오른쪽 단추로 클릭 하 **>** 참조를 추가 하 고 **시스템 .net. Http**가 표시 될 때까지 아래로 스크롤합니다. **시스템 .net. Http** 옆에 확인 표시를 입력 하 고 **확인**을 클릭 합니다.

#### <a name="implement-code-that-sends-a-test-message"></a>테스트 메시지를 보내는 코드 구현

**MessageSender**에서 **Program.cs** 를 편집 하 고 내용을 다음 코드로 바꿉니다.

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

위의 코드에서 *YOUR_API_KEY* 를 클라이언트 앱 프로젝트에 대 한 API 키로 변경 합니다.

이 테스트 앱 서버는 다음 JSON 형식 메시지를 GCM에 보냅니다.

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

그러면 GCM이이 메시지를 클라이언트 앱에 전달 합니다. **MessageSender** 를 빌드하고 명령줄에서 실행할 수 있는 콘솔 창을 엽니다.

### <a name="try-it"></a>시도해 보세요.

이제 클라이언트 앱을 테스트할 준비가 되었습니다. 에뮬레이터를 사용 하거나 장치가 Wi-fi를 통해 GCM과 통신 하는 경우에는 GCM 메시지에 대해 5228, 5229 및 5230를 통해 방화벽에서 다음 TCP 포트를 열어야 합니다.

클라이언트 앱을 시작 하 고 출력 창을 봅니다. `RegistrationIntentService`가 GCM에서 등록 토큰을 수신 하면 출력 창에 다음과 유사한 로그 출력이 포함 된 토큰이 표시 됩니다.

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

이 시점에서 클라이언트 앱은 원격 알림 메시지를 받을 준비가 된 것입니다. 명령줄에서 **MessageSender** 프로그램을 실행 하 여 클라이언트 앱에 "Hello, Xamarin" 알림 메시지를 보냅니다.
**MessageSender** 프로젝트를 아직 빌드하지 않은 경우 지금 수행 합니다.

Visual Studio에서 **MessageSender** 를 실행 하려면 명령 프롬프트를 열고 **MessageSender/bin/Debug** 디렉터리로 변경한 다음 명령을 직접 실행 합니다.

```cmd
MessageSender.exe
```

Mac용 Visual Studio에서 **MessageSender** 를 실행 하려면 터미널 세션을 열고 **MessageSender/bin/Debug** 로 변경한 다음 mono를 사용 하 여 MessageSender를 실행 **합니다.**

```bash
mono MessageSender.exe
```

메시지가 GCM을 통해 전파 되는 데 최대 1 분 정도 걸리며 클라이언트 앱에 다시 돌아갈 수 있습니다. 메시지가 성공적으로 수신 되 면 출력 창에 다음과 유사한 출력이 표시 됩니다.

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

또한 알림 트레이에 새 알림 아이콘이 표시 되는 것을 알 수 있습니다.

[장치에 ![알림 아이콘이 표시 됩니다.](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

알림 트레이를 열어 알림을 볼 때 원격 알림이 표시 되어야 합니다.

[![알림 메시지가 표시 됩니다.](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

축 하 합니다. 앱이 첫 번째 원격 알림을 받았습니다.

앱이 강제로 중지 된 경우 GCM 메시지는 더 이상 수신 되지 않습니다. 강제 중지 후 알림을 다시 시작 하려면 앱을 수동으로 다시 시작 해야 합니다. 이 Android 정책에 대 한 자세한 내용은 [중지 된 응용 프로그램에서 제어 시작](https://developer.android.com/about/versions/android-3.1.html#launchcontrols) 및이 [스택 오버플로 게시물](https://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267)을 참조 하세요.

## <a name="summary"></a>요약

이 연습에서는 Xamarin Android 응용 프로그램에서 원격 알림을 구현 하는 단계를 자세히 설명 합니다. GCM 통신에 필요한 추가 패키지를 설치 하는 방법에 대해 설명 했으며 GCM 서버에 액세스 하기 위한 앱 사용 권한을 구성 하는 방법을 설명 했습니다.
Google Play 서비스 있는지 확인 하는 방법, 등록 토큰에 대해 GCM과 협상 하는 등록 의도 서비스 및 인스턴스 ID 수신기 서비스를 구현 하는 방법 및 GCM 수신기를 구현 하는 방법을 보여 주는 예제 코드를 제공 했습니다. 원격 알림 메시지를 받고 처리 하는 서비스입니다. 마지막으로 GCM을 통해 클라이언트 앱에 테스트 알림을 전송 하는 명령줄 테스트 프로그램을 구현 했습니다.

## <a name="related-links"></a>관련 링크

- [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
