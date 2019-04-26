---
title: Google Maps API 키 가져오기
description: 추가 하기 위한 Google Maps API 키를 얻는 방법 앱에 기능을 매핑합니다.
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: bfeb9d8fa2a0b5a9b18ab8266500586e2e3b6c68
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61155431"
---
# <a name="obtaining-a-google-maps-api-key"></a>Google Maps API 키 가져오기

Android에서 Google Maps 기능을 사용 하려면 google Maps API 키를 등록 해야 합니다. 이 작업을 수행 될 때까지 응용 프로그램에 맵이 아니라 표에서 비어 있는 표시 됩니다. Google Maps Android API v2 키를 받아야-이전 Google Maps Android API 키 v1의 키 작동 하지 않습니다.

Maps API v2 키를 가져오는 단계는 다음과 같습니다.

1.  응용 프로그램 서명에 사용 되는 키 저장소의 SHA-1(secure 지문을 검색 합니다.
2.  Google Api 콘솔에서 프로젝트를 만듭니다.
3.  API 키를 가져옵니다.


## <a name="obtaining-your-signing-key-fingerprint"></a>서명 키 지문 가져오기

Google Maps API 키를 요청 하려면 응용 프로그램 서명에 사용 되는 키 저장소의 sha-1 지문 알 필요가 있습니다.
일반적으로 디버그 키 저장소에 대 한 sha-1 지문 결정 해야 하 고 릴리스에 대 한 응용 프로그램에 서명 하는 데 사용 되는 키 저장소에 대 한 sha-1 지문 다음 의미 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin.Android 응용 프로그램의 디버그 버전에 서명 하는 데 사용 되는 키 저장소를 기본적으로 다음 위치에서 찾을 수 있습니다:

**C:\\사용자가\\[USERNAME]\\AppData\\로컬\\Xamarin\\Android 용 Mono\\debug.keystore**

JDK에서 `keytool` 명령을 실행하면 키 저장소에 대한 정보를 가져올 수 있습니다. 이 도구는 일반적으로 Java bin 디렉터리에 있습니다.

**C:\\Program Files (x86)\\Java\\jdk[VERSION]\\bin\\keytool.exe**

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Xamarin.Android 응용 프로그램의 디버그 버전에 서명 하는 데 사용 되는 키 저장소를 기본적으로 다음 위치에서 찾을 수 있습니다:

**/Users/[USERNAME]/.local/share/Xamarin/Mono Android/debug.keystore에 대 한**

JDK에서 `keytool` 명령을 실행하면 키 저장소에 대한 정보를 가져올 수 있습니다. 이 도구는 일반적으로 Java bin 디렉터리에 있습니다.

**/System/Library/Java/JavaVirtualMachines/[VERSION].jdk/Contents/Home/bin/keytool**

-----


Keytool (위에 표시 된 파일 경로 사용) 다음 명령을 사용 하 여 실행 합니다.

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Debug.keystore 예제

(자동으로 생성 된 수에 대 한 디버깅을 위해)는 기본 디버그 키에 대해이 명령을 사용 합니다.

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

Google Play에 앱을 배포 하는 경우 이어야 합니다 [개인 키를 사용 하 여 서명](~/android/deploy-test/signing/index.md)합니다.
`keytool` 개인 키 세부 정보 및 프로덕션 Google Maps API 키를 만드는 데 사용 되는 결과 SHA-1(secure 지문을 사용 하 여 실행 해야 합니다. 업데이트 해야 합니다 **AndroidManifest.xml** 배포 하기 전에 올바른 Google Maps API 키가 있는 파일입니다.

### <a name="keytool-output"></a>Keytool 출력

콘솔 창 출력은 다음과 같이 표시 됩니다.

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

SHA-1(secure 지문을 사용 하 여 (후 나열 **SHA1**)이이 가이드의 뒷부분에 나오는.

## <a name="creating-an-api-project"></a>API 프로젝트 만들기

서명 키 저장소의 sha-1 지문에 검색 한 후 Google Api 콘솔에서 새 프로젝트를 만듭니다 (또는 기존 프로젝트에 Google Maps Android API v2 서비스를 추가) 하는 데 필요한 것입니다.

1. 브라우저에서로 이동 합니다 [Google 개발자 콘솔 API 및 서비스 대시보드](https://console.developers.google.com/apis/dashboard/) 클릭 **프로젝트를 선택**합니다. 프로젝트 이름을 클릭 하거나 클릭 하 여 새로 만듭니다 **새 프로젝트**:

   [![Google 개발자 콘솔 만들기 프로젝트 단추](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. 새 프로젝트를 만든 경우에 프로젝트 이름을 입력 합니다 **새 프로젝트** 대화 상자가 표시 됩니다. 이 대화 상자에 프로젝트 이름을 기반으로 하는 고유한 프로젝트 ID를 제조할 것입니다. 다음을 클릭 합니다 **만들기** 이 예와 같이 단추:

   [![새 프로젝트 XamarinMapsDemo 라고 합니다.](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. 프로젝트가 생성 되는 1 분 정도 후 및 이동 합니다 **대시보드** 프로젝트의 페이지입니다. 여기에서 클릭 **API 및 서비스를 사용 하도록 설정**:

   [![라이브러리 섹션에서 Google Maps Android API를 클릭합니다.](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. **API 라이브러리** 페이지에서 클릭 **Android 용 Maps SDK**합니다. 다음 페이지에서 클릭 **사용** 이 프로젝트에 대 한 서비스를 활성화 하려면:

   [![대시보드 섹션에서 설정 단추를 클릭](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

이 시점에서 API 프로젝트를 만들었으며 Google Maps Android API v2에 추가 되었습니다. 그러나에 대 한 자격 증명을 만들 때까지 프로젝트에서이 API을 사용할 수 없습니다. 다음 섹션에서는이 키를 사용 하도록 권한이 부여 된 있도록 Xamarin.Android 응용 프로그램 허용 목록 및 API 키를 만드는 방법을 설명 합니다.

## <a name="obtaining-the-api-key"></a>API 키 가져오기

후 합니다 **Google 개발자 콘솔** API 프로젝트 되었습니다 생성 해야 하는 Android API 키를 만듭니다. Xamarin.Android 응용 프로그램 Android 맵 API v2에 대 한 액세스를 부여 된 API 키가 있어야 합니다.

1. **Android 용 Maps SDK** 표시 되는 페이지 (클릭 한 후 **사용 하도록 설정** 이전 단계에서)로 이동 합니다 **자격 증명** 탭을 클릭는 **만들기 자격 증명** 단추:

   [![Android 자격 증명 메시지에 대 한 지도 SDK](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. 클릭 **API 키**:

   [![프로젝트 대화 상자에 자격 증명 추가](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. 이 단추를 클릭 한 후에 API 키 생성 됩니다. 다음은 앱에만이 키를 사용 하 여 Api를 호출할 수 있도록이 키를 제한 해야 합니다. 클릭 **제한 키**:

   [![자격 증명 페이지에서 키 제한을 클릭합니다.](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. 변경 합니다 **이름** 필드를 **API 키 1** 에 사용 되는 키를 기억 하는 데 도움이 되는 이름으로 (**XamarinMapsDemoKey** 이 예제에서 사용). 다음을 클릭 합니다 **Android 앱** 라디오 단추:

   [![자격 증명 페이지에서 Android 앱을 선택합니다.](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. Sha-1 지문에 추가 하려면 클릭 **+ 추가 패키지 이름 및 지문**:

   [![추가 패키지 이름 및 지문을 클릭](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. 앱의 패키지 이름을 입력 하 고는 sha-1 인증서 지문을 입력 (을 통해 얻은 `keytool` 이 가이드의 앞부분에 설명 된 대로). 다음 예에서는 패키지에 대 한 이름을 `XamarinMapsDemo` 입력 이면 뒤에에서 가져온 sha-1 인증서 지문 **debug.keystore**:

   [![패키지 이름 입력은 com.xamarin.docs.android.map](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. Google Maps에 액세스할 APK에 대 한 순서로 sha-1 지문 포함 하며 APK에 서명 하는 데 사용 하는 모든 keystore (디버그 및 릴리스)에 대 한 이름을 패키지 note 합니다. 예를 들어, 디버그 및 릴리스 APK를 생성 하기 위한 다른 컴퓨터에 대 한 컴퓨터를 사용 하는 경우 포함 해야 첫 번째 컴퓨터의 디버그 키 저장소에서 sha-1 인증서 지문 및의 릴리스 키 저장소에서 sha-1 인증서 지문 두 번째 컴퓨터입니다. 클릭 **+ 추가 패키지 이름 및 지문** 이 예와 같이 다른 지문 및 패키지 이름을 추가 하려면:

   [![다른 sha-1 인증서를 만들고 다른 지문 추가](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. **저장** 단추를 클릭하여 변경 내용을 저장합니다. 다음으로, API 키의 목록으로 돌아가게 됩니다. 이전에 만든 다른 API 키가 있으면 해당도 여기에 나열 됩니다. 이 예제에서는 하나의 API 키 (이전 단계에서 만든) 나열 됩니다.

   [![XamarinMapsDemoKey는 API 키 목록에 표시 됩니다.](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>청구 계정에 프로젝트 연결

11 2018 년 6 월부터 API 키 작동 하지 않습니다 (경우에이 서비스는 여전히 모바일 앱 용) 프로젝트 청구 계정에 연결 되지 않은 경우.

1. 햄버거 메뉴 단추를 클릭 하 고 선택 합니다 **청구** 페이지:

   [![햄버거 메뉴의 청구 섹션 선택](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. 프로젝트를 클릭 하 여 대금 청구 계정에 연결 **청구 계정을 연결할** 뒤 **대금 청구 계정 만들기** 표시 된 팝업 (계정이 없다면 안내 새로 만들려면):

   [![청구 계정에 링크 프로젝트](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>프로젝트에 키 추가

마지막으로,이 API 키를 추가 합니다 **AndroidManifest.XML** Xamarin.Android 앱의 파일입니다. 다음 예에서 `YOUR_API_KEY` 이전 단계에서 생성 된 API 키를 사용 하 여 대체 됨:

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
- [keytool](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
