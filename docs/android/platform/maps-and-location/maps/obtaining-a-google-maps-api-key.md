---
title: Google Maps API 키 가져오기
description: 앱에 맵 기능을 추가하기 위해 Google Maps API 키를 가져오는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: 371876d087c7027d4cfe2d2d9ada8b0dbedb5dd5
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "75488974"
---
# <a name="obtaining-a-google-maps-api-key"></a>Google Maps API 키 가져오기

Android에서 Google Maps 기능을 사용하려면 Google에 Maps API 키를 등록해야 합니다. 이를 수행하기 전까지는 애플리케이션에 맵 대신 빈 그리드만 표시됩니다. Google Maps Android API v2 키를 얻어야 합니다. 이전 Google Maps Android API 키 v1의 키는 작동하지 않습니다.

Maps API v2 키를 얻으려면 다음 단계를 수행해야 합니다.

1. 애플리케이션 로그인에 사용되는 키 저장소의 SHA-1 지문을 검색합니다.
2. Google API 콘솔에서 프로젝트를 만듭니다.
3. API 키를 가져옵니다.

## <a name="obtaining-your-signing-key-fingerprint"></a>서명 키 지문 가져오기

Google에서 Maps API 키를 요청하려면 애플리케이션 로그인에 사용되는 키 저장소의 SHA-1 지문을 알아야 합니다.
일반적으로 디버그 키 저장소에 대한 SHA-1 지문을 확인한 후 릴리스에 대한 애플리케이션 서명에 사용되는 키 저장소에 대한 SHA-1 지문을 확인해야 합니다.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

기본적으로 Xamarin.Android 애플리케이션의 디버그 버전 서명에 사용되는 키 저장소는 다음 위치에서 찾을 수 있습니다.

**C:\\Users\\[USERNAME]\\AppData\\Local\\Xamarin\\Mono for Android\\debug.keystore**

JDK에서 `keytool` 명령을 실행하면 키 저장소에 대한 정보를 가져올 수 있습니다. 이 도구는 일반적으로 Java bin 디렉터리에 있습니다.

**C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_[VERSION]\\bin\\keytool.exe**

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

기본적으로 Xamarin.Android 애플리케이션의 디버그 버전 서명에 사용되는 키 저장소는 다음 위치에서 찾을 수 있습니다.

**/Users/[USERNAME]/.local/share/Xamarin/Mono for Android/debug.keystore**

JDK에서 `keytool` 명령을 실행하면 키 저장소에 대한 정보를 가져올 수 있습니다. 이 도구는 일반적으로 Java bin 디렉터리에 있습니다.

**/System/Library/Java/JavaVirtualMachines/[VERSION].jdk/Contents/Home/bin/keytool**

-----

다음 명령을 사용하여 keytool 실행(위에 표시된 파일 경로 사용):

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Debug.keystore 예

기본 디버그 키(디버깅을 위해 자동으로 생성됨)의 경우 다음 명령을 사용합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----

### <a name="production-keys"></a>프로덕션 키

Google Play 스토어에 앱을 배포할 때는 [프라이빗 키로 서명](~/android/deploy-test/signing/index.md)되어야 합니다.
프라이빗 키 세부 정보로 `keytool`을 실행해야 합니다. 결과로 생성되는 SHA-1 지문은 프로덕션 Google Maps API 키를 만들기 위해 사용됩니다. 배포하기 전 올바른 Google Maps API 키로 **AndroidManifest.xml** 파일을 업데이트해야 합니다.

### <a name="keytool-output"></a>Keytool 출력

콘솔 창에 다음과 비슷한 출력이 표시됩니다.

```shell
Alias name: androiddebugkey
Creation date: Jan 01, 2016
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 4aa9b300
Valid from: Mon Jan 01 08:04:04 UTC 2013 until: Mon Jan 01 18:04:04 PST 2033
Certificate fingerprints:
    MD5:  AE:9F:95:D0:A6:86:89:BC:A8:70:BA:34:FF:6A:AC:F9
    SHA1: BB:0D:AC:74:D3:21:E1:43:07:71:9B:62:90:AF:A1:66:6E:44:5D:75
    Signature algorithm name: SHA1withRSA
    Version: 3
```

이 가이드의 뒷부분에 나오는 SHA-1 지문(**SHA1** 뒤에 나열됨)을 사용합니다.

## <a name="creating-an-api-project"></a>API 프로젝트 만들기

서명 키 저장소의 SHA-1 지문을 검색한 후에는 Google API 콘솔에 새 프로젝트를 만들어야 합니다(또는 기존 프로젝트에 Google Maps Android API v2 서비스 추가).

1. 브라우저에서 [Google Developers Console API & Services Dashboard(Google 개발자 콘솔 API 및 서비스 대시보드)](https://console.developers.google.com/apis/dashboard/)로 이동하고 **Select a project(프로젝트 선택)** 를 클릭합니다. 프로젝트 이름을 클릭하거나 **NEW PROJECT(새 프로젝트)** 를 클릭하여 새 이름을 만듭니다.

   [![Google 개발자 콘솔 CREATE PROJECT(프로젝트 만들기) 단추](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. 새 프로젝트를 만들었으면 표시된 **New Project(새 프로젝트)** 대화 상자에 프로젝트 이름을 입력합니다. 이 대화 상자는 프로젝트 이름을 기준으로 고유한 프로젝트 ID를 만듭니다. 그런 후, 다음 예에 표시된 것처럼 **Create(만들기)** 단추를 클릭합니다.

   [![이름이 XamarinMapsDemo인 새 프로젝트](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. 1분 정도 지나면 프로젝트가 생성되고 프로젝트의 **Dashboard(대시보드)** 페이지가 표시됩니다. 여기에서 **ENABLE APIS AND SERVICES(API 및 서비스 사용)** 을 클릭합니다.

   [![Library(라이브러리) 섹션에서 Google Maps Android API 클릭](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. **API Library(API 라이브러리)** 페이지에서 **Maps SDK for Android(Android용 Maps SDK)** 를 클릭합니다. 다음 페이지에서 **ENABLE(사용)** 을 클릭하여 이 프로젝트에 대해 서비스를 설정합니다.

   [![Dashboard(대시보드) 섹션에서 ENABLE(사용) 단추 클릭](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

이제 API 프로젝트가 생성되고 Google Maps Android API v2가 여기에 추가되었습니다. 하지만 이에 대한 자격 증명을 만들기 전까지는 프로젝트에서 이 API를 사용할 수 없습니다. 다음 섹션에서는 API 키를 만들고 이 키를 사용하도록 Xamarin.Android 애플리케이션을 허용 목록에 등록하는 방법을 설명합니다.

## <a name="obtaining-the-api-key"></a>API 키 가져오기

**Google 개발자 콘솔** API 프로젝트가 생성된 다음에는 Android API 키를 만들어야 합니다. Xamarin.Android 애플리케이션에 Android Map API v2 액세스 권한을 부여하려면 API 키가 있어야 합니다.

1. 이전 단계에서 **ENABLE(사용)** 을 클릭한 후 표시되는 **Maps SDK for Android(Android용 Maps SDK)** 페이지에서 **Credentials(자격 증명)** 탭으로 이동하고 **Create credentials(자격 증명 만들기)** 단추를 클릭합니다.

   [![Android용 Maps SDK 자격 증명 메시지](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. **API key(API 키)** 를 클릭합니다.

   [![프로젝트에 자격 증명 추가 대화 상자](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. 이 단추를 클릭하면 API 키가 생성됩니다. 그 다음에는 사용자의 앱만 이 키로 API를 호출할 수 있도록 이 키를 제한할 필요가 있습니다. **RESTRICT KEY(키 제한)** 를 클릭합니다.

   [![Credentials(자격 증명) 페이지에서 Restrict Key(키 제한) 클릭](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. **API Key 1(API 키 1)** 로 표시된 **Name(이름)** 필드를 키 용도를 효과적으로 나타낼 수 있는 이름으로 바꿉니다(이 예에서는 **XamarinMapsDemoKey** 사용). 그런 다음, **Android apps(Android 앱)** 라디오 단추를 클릭합니다.

   [![Credentials(자격 증명) 페이지에서 Android 앱 선택](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. SHA-1 지문을 추가하기 위해 **+ Add package name and fingerprint(+ 패키지 이름 및 지문 추가)** 를 클릭합니다.

   [![앱 패키지 이름 및 지문 클릭](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. 앱의 패키지 이름을 입력하고 SHA-1 인증서 지문(이 가이드의 앞부분에 설명된 대로 `keytool`을 통해 획득)을 입력합니다. 다음 예에서는 `XamarinMapsDemo`에 대한 패키지 이름을 입력하고, **debug.keystore**에서 가져온 SHA-1 인증서 지문을 표시합니다.

   [![입력한 패키지 이름 com.xamarin.docs.android.map](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. APK가 Google Maps에 액세스할 수 있으려면 APK 서명에 사용하는 모든 키 저장소(디버그 및 릴리스)에 대해 SHA-1 지문 및 패키지 이름을 포함해야 합니다. 예를 들어 디버그용으로 하나의 컴퓨터를 사용하고 릴리스 APK를 생성하기 위해 다른 컴퓨터를 사용할 경우, 첫 번째 컴퓨터의 디버그 키 저장소에서 가져온 SHA-1 인증서 지문과 두 번째 컴퓨터의 릴리스 키 저장소에서 가져온 SHA-1 인증서 지문을 포함해야 합니다. **+ Add package name and fingerprint(+ 패키지 이름 및 지문 추가)** 를 클릭하여 이 예에 표시된 것처럼 다른 지문 및 패키지 이름을 추가합니다.

   [![다른 지문을 추가하여 또 다른 SHA-1 인증서 만들기](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. **저장** 단추를 클릭하여 변경 내용을 저장합니다. 그러면 API 키 목록으로 돌아갑니다. 이전에 만든 다른 API 키가 있으면 그것들도 여기에 나열됩니다. 이 예에서는 이전 단계에서 만든 하나의 API 키만 나열됩니다.

   [![API 키 목록에 표시된 XamarinMapsDemoKey](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>청구 가능 계정에 프로젝트 연결

2018년 6월 11일부터는 해당 서비스가 모바일 앱에 대해 무료로 제공되더라도 프로젝트가 청구 가능 계정에 연결되어 있지 않은 경우 API 키가 작동하지 않습니다.

1. 햄버거 메뉴 단추를 클릭하고 **Billing(청구)** 페이지를 선택합니다.

   [![햄버거 메뉴 청구 섹션 선택](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. 표시된 팝업에서 **CREATE BILLING ACCOUNT(청구 계정 만들기)** 다음에 표시된 **Link a billing account(청구 계정 연결)** 을 클릭하여 청구 계정에 프로젝트를 연결합니다(계정이 없으면 새 계정 만들기 안내가 표시됨).

   [![청구 계정에 프로젝트 연결](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>프로젝트에 키 추가

마지막으로 이 API 키를 Xamarin.Android 앱의 **AndroidManifest.XML** 파일에 추가합니다. 다음 예에서는 `YOUR_API_KEY`가 이전 단계에서 생성된 API 키로 바뀝니다.

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionName="4.10" package="com.xamarin.docs.android.mapsandlocationdemo"
    android:versionCode="10">
...
  <application android:label="@string/app_name">
    <!-- Put your Google Maps V2 API Key here. -->
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
    <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
  </application>
</manifest>
```

## <a name="related-links"></a>관련 링크

- [Google API 콘솔](https://code.google.com/apis/console/)
- [Google Maps API 키](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](https://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
