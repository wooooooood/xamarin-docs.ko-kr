---
title: API 키를 매핑하는 Google 얻기
description: 추가 하기 위한 Google 맵 API 키를 가져오는 방법을 응용 프로그램에 기능을 매핑합니다.
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/25/2018
ms.openlocfilehash: 365bc56c70ef903622c3a4583a30460f907b4ec9
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935056"
---
# <a name="obtaining-a-google-maps-api-key"></a>API 키를 매핑하는 Google 얻기

Android에서 Google 맵 기능을 사용 하려면 Google 사용 하 여 지도 API 키에 대 한 등록 해야 합니다. 이 작업을 수행 될 때까지 응용 프로그램에 맵 대신 빈 표 형태만 표시 됩니다. Google 맵 Android API v2 키를 받아야-이전 Google 맵 Android API 키 v 1에서 키가 작동 하지 않습니다.

지도 API v2 키를 가져오는 단계는 다음과 같습니다.

1.  응용 프로그램에 서명 하는 데 사용 되는 키 저장소의 s h A-1 지문을 검색 합니다.
2.  Google Api 콘솔에 프로젝트를 만듭니다.
3.  API 키를 가져오세요.


## <a name="obtaining-your-signing-key-fingerprint"></a>서명 키 지문 가져오기

Google에서 지도 API 키를 요청 하려면 응용 프로그램에 서명 하는 데 사용 되는 키 저장소의 s h A-1 지문 알고 있어야 합니다.
이 경우 일반적으로 s h A-1 릴리스에 대 한 응용 프로그램에 서명 하는 데 사용 되는 키 저장소에 대 한 지문 다음 및 디버그 키 저장소에 대 한 s h A-1 지문 결정 해야 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

응용 프로그램 수 Xamarin.Android의 디버그 버전에 서명 하는 데 사용 되는 키 저장소 기본적으로 다음 위치에서 찾을 수 있습니다:

**C:\\사용자\\[USERNAME]\\AppData\\로컬\\Xamarin\\Android 용 모노\\debug.keystore**

JDK에서 `keytool` 명령을 실행하면 키 저장소에 대한 정보를 가져올 수 있습니다. 이 도구는 일반적으로 Java bin 디렉터리에 있습니다.

**C:\\Program Files (x86)\\Java\\jdk [버전]\\bin\\keytool.exe**

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

응용 프로그램 수 Xamarin.Android의 디버그 버전에 서명 하는 데 사용 되는 키 저장소 기본적으로 다음 위치에서 찾을 수 있습니다:

**/Users/[USERNAME]/.local/share/Xamarin/Mono Android/debug.keystore에 대 한**

JDK에서 `keytool` 명령을 실행하면 키 저장소에 대한 정보를 가져올 수 있습니다. 이 도구는 일반적으로 Java bin 디렉터리에 있습니다.

**/System/Library/Java/JavaVirtualMachines/[VERSION].jdk/Contents/Home/bin/keytool**

-----


Keytool (위에 표시 된 파일 경로 사용 하 여) 다음 명령을 사용 하 여 실행 합니다.

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Debug.keystore 예제

(자동으로 생성 된 사용자에 대 한 디버깅을 위해) 기본 디버그 키에 대해이 명령을 사용 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----


### <a name="production-keys"></a>프로덕션 키

Google Play에 앱을 배포할 때는 것 [개인 키로 서명](~/android/deploy-test/signing/index.md)합니다.
`keytool` 개인 키 정보 및 결과 s h A-1 비교 하 여 프로덕션 Google 맵 API 키를 만드는 데 사용 하 여 실행 해야 합니다. 업데이트는 **AndroidManifest.xml** 배포 하기 전에 올바른 Google 지도 API 키 파일입니다.

### <a name="keytool-output"></a>Keytool 출력

콘솔 창에 출력 하는 다음과 같은 내용이 나타납니다.

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

s h A-1 지문을 사용 하 여 (후 나열 **SHA1**)이이 가이드의 뒷부분에 나오는 합니다.

## <a name="creating-an-api-project"></a>API 프로젝트 만들기

서명 키 저장소는 s h A-1 지문의 검색 한 후에 Google Api 콘솔에서 새 프로젝트를 만듭니다 (또는 기존 프로젝트에 Google 맵 Android API v2 서비스를 추가) 하는 데 필요한 합니다.

1. 브라우저에서로 이동 된 [Google 개발자 콘솔 API 및 서비스 대시보드](https://console.developers.google.com/apis/dashboard/) 클릭 **프로젝트를 선택**합니다. 프로젝트 이름을 클릭 하거나 클릭 하 여 새로 만들 **새 프로젝트**:

   [![Google 개발자 콘솔 만들 프로젝트 단추](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. 새 프로젝트를 만든 경우에 프로젝트 이름을 입력 된 **새 프로젝트** 표시 되는 대화 상자. 이 대화 상자에 프로젝트 이름을 기반으로 하는 고유한 프로젝트 ID 제조할 것입니다. 그런 다음 클릭는 **만들기** 이 예제에 표시 된 대로 단추:

   [![새 프로젝트 XamarinMapsDemo 라고 합니다.](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. 1 분 정도 지난 후 프로젝트가 생성 되 고 이동 합니다는 **대시보드** 프로젝트의 페이지입니다. 여기에서 클릭 **API 및 서비스 사용**:

   [![Google Android API 라이브러리 섹션에서 지도 클릭 하면](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. **API 라이브러리** 페이지 **Maps SDK for Android**합니다. 다음 페이지에서 클릭 **사용** 이 프로젝트에 대 한 서비스를 활성화 하려면:

   [![대시보드 섹션에서 설정 단추를 클릭 하면](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

이 시점에서 API 프로젝트를 만든 및 Google 맵 Android API v 2에 추가 되었습니다. 그러나에 대 한 자격 증명을 만들 때까지 프로젝트에서이 API를 사용할 수 없습니다. 다음 섹션에는이 키를 사용 하도록 권한이 부여 된 있도록 API 키 및 허용 목록 Xamarin.Android 응용 프로그램을 만드는 방법을 설명 합니다.

## <a name="obtaining-the-api-key"></a>API 키 가져오기

이후에 **Google 개발자 콘솔** API 프로젝트 되었습니다 생성 해야 하는 Android API 키를 만듭니다. Android 지도 API v 2에 대 한 액세스를 부여 Xamarin.Android 응용 프로그램 API 키가 있어야 합니다.

1. 에 **Maps SDK for Android** 표시 되는 페이지 (클릭 한 후 **사용** 이전 단계에서)로 이동는 **자격 증명** 탭을 클릭는 **만들기 자격 증명** 단추:

   [![Android 자격 증명 메시지에 대 한 맵을 SDK](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. 클릭 **API 키**:

   [![자격 증명을 대화 상자에 프로젝트 추가](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. 이 단추를 클릭 하면 API 키가 생성 됩니다. 다음은 앱에만이 키와 Api를 호출할 수 있도록이 키를 제한 해야 합니다. 클릭 **RESTRICT 키**:

   [![자격 증명 페이지에서 제한 키를 클릭 하면](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. 변경 된 **이름** 필드를 **API 키 1** 키 용도를 기억 하는 데 도움이 되는 이름에 (**XamarinMapsDemoKey** 이 예제에서 사용). 그런 다음 클릭 하 고 **Android 앱** 라디오 단추:

   [![자격 증명 페이지에서 Android 앱을 선택 하면](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. 비교 하 여 s h A-1을 추가 하려면 클릭 **+ 패키지 이름 및 지문 추가**:

   [![추가 패키지 이름 및 지문](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. 앱의 패키지 이름을 입력 하 고는 s h A-1 인증서 지문을 입력 (을 통해 얻은 `keytool` 이 가이드 앞부분에서 설명한 대로). 다음 예제에서는 패키지에 대 한 이름을 `XamarinMapsDemo` 입력 하며, 그에서 가져온 s h A-1 인증서 지문은 **debug.keystore**:

   [![입력 한 패키지 이름이 com.xamarin.docs.android.map은](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. 을 프로그램 APK Google Maps에 액세스할 수 있도록 해야 s h A-1 지문을 포함 하 여 APK 서명에 사용 하는 모든 키 저장소 (디버그 및 릴리스)에 대 한 이름을 패키지 note 합니다. 예를 들어, 디버그 및 릴리스 APK를 생성 하기 위한 다른 컴퓨터에 대 한 컴퓨터를 사용 하는 경우 포함 해야 첫 번째 컴퓨터의 디버그 키 저장소에서 s h A-1 인증서 지문 및의 릴리스 키 저장소에서 s h A-1 인증서 지문 두 번째 컴퓨터입니다. 클릭 **+ 패키지 이름 및 지문 추가** 이 예제에 표시 된 대로 다른 지문 및 패키지 이름을 추가 하려면:

   [![다른 s h A-1 인증서를 만듭니다 다른 지문 추가](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. **저장** 단추를 클릭하여 변경 내용을 저장합니다. 다음으로, API 키의 목록으로 돌아갑니다. 앞서 만든 다른 API 키가 있으면 것도 여기에 나열 됩니다. 이 예제 (이전 단계에서 만든) API 키를 하나만 표시 됩니다.

   [![XamarinMapsDemoKey는 API 키 목록에 표시 됩니다.](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>프로젝트 청구 계정에 연결

API 키 년 6 월 11 2018를 시작 하 고 프로젝트 (경우에이 서비스는 여전히 모바일 앱에 대 한) 청구 계정에 연결 되지 않은 경우 되지 않습니다.

1. 햄버거 메뉴 단추를 클릭 하 고 선택 된 **청구** 페이지:

   [![햄버거 메뉴 청구 섹션을 선택 하면](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. 프로젝트를 클릭 하 여 요금 청구 계정에 연결 **요금 청구 계정을 연결** 이어서 **요금 청구 계정 만들기** 표시 된 팝업에서 (계정이 없으면 안내를 새로 만들):

   [![요금 청구 계정에 연결 프로젝트](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>프로젝트에 키를 추가합니다.

이 API 키를 마지막으로 추가 된 **AndroidManifest.XML** Xamarin.Android 앱의 파일입니다. 다음 예에서 `YOUR_API_KEY` 은 이전 단계에서 생성 되는 API 키로 대체 되어야 합니다.

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

- [Google APIs Console](https://code.google.com/apis/console/)
- [Google는 API 키를 매핑](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
