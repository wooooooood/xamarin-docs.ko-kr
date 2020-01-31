---
title: Google Maps API 키 가져오기
description: 앱에 지도 기능을 추가 하기 위해 Google Maps API 키를 가져오는 방법입니다.
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: 371876d087c7027d4cfe2d2d9ada8b0dbedb5dd5
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488974"
---
# <a name="obtaining-a-google-maps-api-key"></a>Google Maps API 키 가져오기

Android에서 Google Maps 기능을 사용 하려면 Google을 사용 하 여 Maps API 키를 등록 해야 합니다. 이 작업을 수행 하기 전에는 응용 프로그램에 맵 대신 빈 그리드만 표시 됩니다. 이전 Google Maps에서 Google Maps Android API v2 키 키를 가져와야 합니다. Android API 키 v1은 작동 하지 않습니다.

Maps API v2 키 가져오기에는 다음 단계가 포함 됩니다.

1. 응용 프로그램에 서명 하는 데 사용 되는 키 저장소의 SHA-1 지문을 검색 합니다.
2. Google Api 콘솔에서 프로젝트를 만듭니다.
3. API 키를 가져옵니다.

## <a name="obtaining-your-signing-key-fingerprint"></a>서명 키 지문 가져오기

Google에서 맵 API 키를 요청 하려면 응용 프로그램에 서명 하는 데 사용 되는 키 저장소의 SHA-1 지문을 알아야 합니다.
일반적으로이는 디버그 키 저장소에 대 한 SHA-1 지문을 결정 한 다음, 응용 프로그램을 릴리스에 서명 하는 데 사용 되는 키 저장소에 대 한 SHA-1 지문을 결정 해야 함을 의미 합니다.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

기본적으로 Xamarin.ios 응용 프로그램의 디버그 버전에 서명 하는 데 사용 되는 키 저장소는 다음 위치에서 찾을 수 있습니다.

**C:\\사용자\\[사용자 이름]\\AppData\\로컬\\Xamarin\\Mono for Android\\디버그. 키 저장소**

JDK에서 `keytool` 명령을 실행하면 키 저장소에 대한 정보를 가져올 수 있습니다. 이 도구는 일반적으로 Java bin 디렉터리에 있습니다.

**C:\\프로그램 파일\\Android\\jdk\\microsoft_dist_openjdk_ [버전]\\bin\\keytool .exe**

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

기본적으로 Xamarin.ios 응용 프로그램의 디버그 버전에 서명 하는 데 사용 되는 키 저장소는 다음 위치에서 찾을 수 있습니다.

**/사용자/[사용자 이름]/.local/share/Xamarin/Mono for Android/키 저장소**

JDK에서 `keytool` 명령을 실행하면 키 저장소에 대한 정보를 가져올 수 있습니다. 이 도구는 일반적으로 Java bin 디렉터리에 있습니다.

**/System/Library/Java/JavaVirtualMachines/[VERSION].jdk/Contents/Home/bin/keytool**

-----

다음 명령을 사용 하 여 keytool을 실행 합니다 (위에 표시 된 파일 경로 사용).

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>키 저장소 예제

디버깅을 위해 자동으로 만들어지는 기본 디버그 키의 경우 다음 명령을 사용 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----

### <a name="production-keys"></a>프로덕션 키

Google Play에 앱을 배포 하는 경우 [개인 키로 서명](~/android/deploy-test/signing/index.md)해야 합니다.
`keytool`은 개인 키 정보와 프로덕션 Google Maps API 키를 만드는 데 사용 되는 결과 SHA-1 지문을 사용 하 여 실행 해야 합니다. 배포 하기 전에 올바른 Google Maps API 키를 사용 하 여 **Androidmanifest .xml** 파일을 업데이트 해야 합니다.

### <a name="keytool-output"></a>Keytool 출력

콘솔 창에 다음 출력과 유사한 내용이 표시 되어야 합니다.

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

이 가이드의 뒷부분에 있는 SHA-1 지문 ( **SHA1**후에 나열 됨)을 사용 합니다.

## <a name="creating-an-api-project"></a>API 프로젝트 만들기

서명 키 저장소의 SHA-1 지문을 검색 한 후 Google Api 콘솔에서 새 프로젝트를 만들거나 기존 프로젝트에 Google Maps Android API v2 서비스를 추가 해야 합니다.

1. 브라우저에서 [Google 개발자 콘솔 API & 서비스 대시보드로](https://console.developers.google.com/apis/dashboard/) 이동 하 고 **프로젝트 선택**을 클릭 합니다. 프로젝트 이름을 클릭 하거나 **새 프로젝트**를 클릭 하 여 새 프로젝트를 만듭니다.

   [![Google 개발자 콘솔 프로젝트 만들기 단추](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. 새 프로젝트를 만든 경우 표시 되는 **새 프로젝트** 대화 상자에 프로젝트 이름을 입력 합니다. 이 대화 상자는 프로젝트 이름을 기반으로 하는 고유한 프로젝트 ID를 제조 합니다. 그런 다음이 예제에 표시 된 대로 **만들기** 단추를 클릭 합니다.

   [![새 프로젝트의 이름이 XamarinMapsDemo](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. 1 분이 지나면 프로젝트가 만들어지고 프로젝트의 **대시보드** 페이지로 이동 합니다. 여기에서 **API 및 서비스 사용**을 클릭 합니다.

   [![라이브러리 섹션에서 Google Maps Android API 클릭](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. **API 라이브러리** 페이지에서 **ANDROID 용 Maps SDK**를 클릭 합니다. 다음 페이지에서 **사용** 을 클릭 하 여이 프로젝트에 대 한 서비스를 켭니다.

   [![대시보드 섹션에서 사용 단추를 클릭](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

이제 API 프로젝트가 만들어지고 Google Maps Android API v2가 추가 되었습니다. 그러나이 API에 대 한 자격 증명을 만들 때까지 프로젝트에서이 API를 사용할 수 없습니다. 다음 섹션에서는 API 키를 만들고이 키를 사용할 수 있는 권한을 부여 하도록 Xamarin Android 응용 프로그램을 만드는 방법을 설명 합니다.

## <a name="obtaining-the-api-key"></a>API 키 가져오기

**Google 개발자 콘솔** api 프로젝트를 만든 후에는 Android api 키를 만들어야 합니다. Android Map API v 2에 대 한 액세스 권한을 부여 하기 전에 xamarin.ios 응용 프로그램에 API 키가 있어야 합니다.

1. 이전 단계에서 **사용** 을 클릭 한 후 표시 되는 **ANDROID 용 Maps SDK** 페이지에서 **자격 증명** 탭으로 이동 하 여 **자격 증명 만들기** 단추를 클릭 합니다.

   [![Android 자격 증명에 대 한 맵 SDK 메시지](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. **API 키**를 클릭 합니다.

   [![프로젝트에 자격 증명 추가 대화 상자](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. 이 단추를 클릭 하면 API 키가 생성 됩니다. 다음으로는 앱만이 키를 사용 하 여 Api를 호출할 수 있도록이 키를 제한 해야 합니다. **키 제한**을 클릭 합니다.

   [자격 증명 페이지에서 키 제한을 클릭 ![](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. **이름** 필드를 **API 키 1** 에서 이름으로 변경 합니다 (이 예제에서는**XamarinMapsDemoKey** 가 사용 됨). 다음으로 **Android 앱** 라디오 단추를 클릭 합니다.

   [![자격 증명 페이지에서 Android 앱을 선택](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. SHA-1 지문을 추가 하려면 **+ 패키지 이름 및 지문 추가**를 클릭 합니다.

   [![패키지 이름 및 지문 추가 클릭](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. 앱의 패키지 이름을 입력 하 고 SHA-1 인증서 지문 (이 가이드의 앞부분에서 설명한 대로 `keytool`를 통해 가져옴)을 입력 합니다. 다음 예제에서는 `XamarinMapsDemo`에 대 한 패키지 이름을 입력 하 고 그 뒤에 **키 저장소**에서 가져온 sha-1 인증서 지문을 입력 합니다.

   [![입력 한 패키지 이름은 com. xamarin.ios. 입니다.](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. Google Maps에 액세스할 APK에 대 한 순서로 sha-1 지문 포함 하며 APK에 서명 하는 데 사용 하는 모든 keystore (디버그 및 릴리스)에 대 한 이름을 패키지 note 합니다. 예를 들어, 디버그 및 릴리스 APK를 생성 하기 위한 다른 컴퓨터에 대 한 컴퓨터를 사용 하는 경우 포함 해야 첫 번째 컴퓨터의 디버그 키 저장소에서 sha-1 인증서 지문 및의 릴리스 키 저장소에서 sha-1 인증서 지문 두 번째 컴퓨터입니다. **+ 패키지 이름 및 지문 추가** 를 클릭 하 여 다음 예와 같이 다른 지문 및 패키지 이름을 추가 합니다.

   [![다른 지문을 추가 하 다른 SHA-1 인증서를 만듭니다.](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. **저장** 단추를 클릭하여 변경 내용을 저장합니다. 다음으로 API 키 목록으로 돌아갑니다. 이전에 만든 다른 API 키가 있는 경우 여기에도 나열 됩니다. 이 예제에서는 이전 단계에서 만든 API 키 한 개만 나열 됩니다.

   [![XamarinMapsDemoKey는 API 키 목록에 표시 됩니다.](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>청구 가능한 계정에 프로젝트 연결

2018 년 6 월 11 일부 터, 프로젝트가 청구 가능 계정에 연결 되어 있지 않으면 API 키가 작동 하지 않습니다 (서비스가 모바일 앱에 대해서도 계속 가능 함).

1. 햄버거 메뉴 단추를 클릭 하 고 **청구** 페이지를 선택 합니다.

   [![햄버거 메뉴 청구 섹션을 선택 하](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. 청구 계정에 **링크를** 클릭 하 고 표시 된 팝업에서 **청구 계정 만들기** 를 클릭 하 여 프로젝트를 청구 계정에 연결 합니다. 계정이 없는 경우 새 항목을 만들도록 안내 합니다.

   [![청구 계정에 프로젝트 링크](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>프로젝트에 키 추가

마지막으로, Xamarin Android 앱의 **Androidmanifest** 파일에이 API 키를 추가 합니다. 다음 예제에서 `YOUR_API_KEY`은 이전 단계에서 생성 된 API 키로 대체 됩니다.

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

- [Google Api 콘솔](https://code.google.com/apis/console/)
- [Google Maps API 키](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](https://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
