---
title: "Oreo 기능"
description: "Xamarin.Android를 사용 하 여 최신 버전의 Android 용 앱 개발을 시작 하는 방법입니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 478a285dc326b62bf2fc186599bfb7515988f9ee
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="oreo-features"></a>Oreo 기능

_Xamarin.Android를 사용 하 여 최신 버전의 Android 용 앱 개발을 시작 하는 방법입니다._

[Android 8.0 Oreo](https://developer.android.com/index.html) Google에서 사용 가능한 Android의 최신 버전입니다. Android Oreo Xamarin.Android 개발자에 게 관심 있는 여러 가지 새로운 기능을 제공합니다. 이러한 기능에 XML, 다운로드 글꼴, 자동 채우기, 및 그림 (PIP)의 그림에서 알림 채널, 알림 배지, 사용자 지정 글꼴 포함 됩니다. 이러한 Api는 Xamarin.Android 앱 Xamarin.Android 8.0을 사용 하는 경우에 사용할 수 이상 및 android Oreo 이러한 새 capabilties 위한 새 Api를 포함 됩니다.

[![Android Oreo hero 이미지](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png)

이 문서 구조 8.0 Oreo Android 용 Xamarin.Android 앱 개발에서 시작할 수 있도록 되어 있습니다. 필요한 업데이트를 설치, 구성 하는 SDK를 만들려면 에뮬레이터 (또는 장치) 테스트 하기 위한 방법을 설명 합니다. 또한 Xamarin.Android 앱에서 Android Oreo 기능을 사용 하는 방법을 보여 주는 샘플 앱에 대 한 링크가 있는 Android 8.0 Oreo에 새로운 기능의 개요를 제공 합니다.


<a name="requirements" />

## <a name="requirements"></a>요구 사항

다음은 Android Oreo 기능 Xamarin 기반 앱에서 사용 하려면 필요 합니다.

-   **Visual Studio** &ndash; Windows를 사용 하는 경우 Visual Studio의 15.5 이상 버전이 필요 합니다.  Mac을 사용 하는 경우 Mac 7.2.0 버전에 대 한 Visual Studio가 필요 합니다.

-   **Xamarin.Android** &ndash; Xamarin.Android 8.0 이상을 설치 하 고 Visual Studio를 사용 하 여 구성 해야 합니다.

-   **Android SDK** &ndash; Android SDK 8.0 (API 26) 나중에 설치 되어 있어야 Android SDK Manager를 통해 합니다.


<a name="gettingstarted" />

## <a name="getting-started"></a>시작

Android Oreo Xamarin.Android를 사용 하 여 시작 하려면 다운로드 하 고 Android Oreo 프로젝트를 만들려면 먼저 최신 도구와 SDK 패키지를 설치 해야 합니다.

1. Visual Studio의 최신 버전으로 업데이트 합니다.

2. 설치는 **Android 8.0.0 (API 26)** 또는 이후 패키지 및 SDK 관리자를 통해 도구입니다.

3. Android Oreo (API 26)를 대상으로 하는 새 Xamarin.Android 프로젝트를 만듭니다.

4. 에뮬레이터 또는 Android Oreo 앱 테스트에 대 한 장치를 구성 합니다.

이러한 각 단계는 다음 섹션에서 설명:


<a name="updates" />

### <a name="update-visual-studio-and-xamarinandroid"></a>Visual Studio 및 Xamarin.Android를 업데이트 합니다.

Visual Studio에 Android Oreo 지원을 추가 하려면 다음을 수행 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Visual Studio 2017 사용 중인 경우: 

    1. Visual Studio 2017 15.5 이후 버전에 업데이트 (참조 [업데이트 Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/update-visual-studio)).

    2. SDK Manager를 사용 하 여 [설치 지침](~/android/get-started/installation/android-sdk.md#installation) Xamarin SDK Manager를 설치할 수 있습니다.
       Google 실행형 26.0 이상은 API를 지 원하는 GUI SDK 관리자를 제공 하지 않기 때문에 Xamarin SDK Manager는 설치 해야 합니다.

-   SDK 도구 25로 다운 그레이드 하 고 사용 하는 것이 좋습니다에서는 Visual Studio 2015를 사용 하는 경우 이전 Google 에뮬레이터 관리자 GUI 합니다. SDK 도구 25 API 26, 27, 이상 함께 사용할 수 있습니다 및 새 플랫폼에 대 한 개발에 영향을 줄 되지 않습니다. 이렇게 하면 인터페이스 VS의 이전 버전에 대 한 Android SDK를 관리 하기 위한 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   에 설명 된 대로 Mac 용 Visual Studio 2017의 안정적인 최신 버전으로 업데이트 [Mac 용 Visual Studio 업데이트](https://docs.microsoft.com/en-us/visualstudio/mac/update)합니다.

-----

Xamarin Android Oreo 지원에 대 한 자세한 내용은 참조는 [Xamarin.Android 8.0 릴리스 정보](https://developer.xamarin.com/releases/android/xamarin.android_8/xamarin.android_8.0/)합니다.


<a name="sdk" />

### <a name="install-the-android-sdk"></a>Android SDK 설치

Xamarin.Android 8.0으로 프로젝트를 만들려면 먼저 사용 해야 Xamarin Android SDK Manager를 설치에 대 한 SDK 플랫폼 **Android 8.0-Oreo** 이상. Android SDK 도구 26.0 이상을 설치 해야 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. SDK 관리자를 시작 (Visual Studio에서 클릭 **도구 > Android > Android SDK Manager**).

2. 설치는 **Android 8.0-Oreo** 패키지 합니다. Android SDK 에뮬레이터를 사용 하는 경우 수를 포함 해야 합니다는 **x86** 해야 하는 시스템 이미지:

    [![Android SDK Manager에서 Android 8.0 패키지 선택](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png)

3. 설치 **Android SDK 도구 26.0.2** 이상을 **Android SDK 플랫폼 도구 26.0.0** 이상 및 **Android SDK 빌드-도구 26.0.0** (또는 이상):

    [![Android SDK Manager에서 Android SDK 도구 26 선택](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. SDK 관리자를 시작 (Mac 용 Visual Studio에서 클릭 **도구 > SDK Manager**).

2. 설치는 **Android 8.0-Oreo** SDK 패키지 합니다. Android SDK 에뮬레이터를 사용 하는 경우 수를 포함 해야 합니다는 **x86** 해야 하는 시스템 이미지:

    [![SDK Manager에서 Android 8.0 패키지 선택](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png)

3. 설치 **Android SDK 도구 26.0.2** 이상을 **Android SDK 플랫폼 도구 26.0.0** 이상 및 **Android SDK 빌드-도구 26.0.0** (또는 이상):

    [![SDK Manager에서 Android SDK 도구 26 선택](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png)

-----


<a name="xaproject" />

### <a name="start-a-xamarinandroid-project"></a>Start a Xamarin.Android Project

새 Xamarin.Android 프로젝트를 만듭니다. Xamarin 사용한 Android 개발을 처음 접하는 경우 참조 [Hello, Android](~/android/get-started/hello-android/index.md) Xamarin.Android 프로젝트를 만드는 방법에 대 한 자세한 내용은 합니다.

Android 프로젝트를 만들 때 Android 8.0 이상을 대상으로 지정할 버전 설정을 구성 해야 합니다. 예를 들어 Android 8.0에 대 한 프로젝트를 대상으로 구성 해야 하는 프로젝트 대상 Android API 수준 **Android 8.0 (API 26)**합니다. 대상 프레임 워크 수준 이상 API 26으로 설정 하는 것이 좋습니다. Android API 수준 수준을 구성 하는 방법에 대 한 자세한 내용은 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다.

<a name="emudev" />

### <a name="configure-an-emulator-or-device"></a>에뮬레이터 또는 장치를 구성 합니다.

Android SDK 도구 26.0를 설치한 후 기본 Google GUI 기반 AVD 관리자를 시작 하려고 하면 또는 이상 버전에서는 다음과 같은 오류 대화 상자, 명령줄 AVD 관리자 도구를 사용할 수를 지정 하는 발생할 수 있습니다 하는 경우 **avdmanager** 대신 :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android Emulator Manager 경고 대화 상자](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Android Emulator Manager 경고 대화 상자](oreo-images/mac/03-avd-warning.png)

-----

Google 실행형 26.0 이상은 API를 지 원하는 GUI AVD 관리자를 더 이상 제공 하기 때문에이 메시지가 표시 됩니다. Android 8.0 Oreo에 대 한 Xamarin Android Emulator Manager 나 명령줄 중 하나를 사용 해야 `avdmanager` 도구 Android Oreo에 대 한 가상 장치를 사용 합니다.

Xamarin Android 장치 관리자를 만들고 가상 장치 관리를 사용 하려면 참조 [Xamarin Android 장치 관리자](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)합니다.
Xamarin Android 에뮬레이터 관리자 없이 가상 장치를 만들려면 다음 섹션의 단계를 수행 합니다.


#### <a name="creating-virtual-devices-using-avdmanager"></a>Avdmanager 사용 하 여 가상 장치 만들기

사용 하도록 **avdmanager** 새 가상 장치를 만들려면 다음이 단계를 수행 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  명령 프롬프트 창을 열고 설정 `JAVA_HOME` 컴퓨터에서 Java SDK의 위치입니다. 일반적인 Xamarin 설치의 경우 다음 명령을 사용할 수 있습니다.

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Android SDK의 위치를 추가 `bin` 폴더를 사용자 `PATH`합니다.
    일반적인 Xamarin 설치의 경우 다음 명령을 사용할 수 있습니다.

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  명령 프롬프트 창을 닫고 새 명령 프롬프트 창을 엽니다. 사용 하 여 새 가상 장치 만들기는 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) 명령입니다. 예를 들어 라는 AVD를 **AVD-Oreo-8.0** 26 API 수준에 대 한 시스템 이미지 x86를 사용 하 여 다음 명령을 사용 합니다.

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  와 메시지가 나타나면 **[아니요] 사용자 지정 하드웨어 프로필을 만들려면 원하는** 입력할 수 있는 **없는** 기본 하드웨어 프로필 क र च 합니다. 시킬 경우 **예**, **avdmanager** 하드웨어 프로필을 사용자 지정에 대 한 질문의 목록을 표시 합니다.

후 **avdmanager** 가상 장치를 만들려면 장치 풀 다운 메뉴에 포함 될지 것입니다.

[![새 AVD 장치 풀 다운 메뉴에 추가](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  열기는 **터미널** 창과 mac Android SDK 도구 디렉터리의 위치 변경 일반적인 Xamarin 설치의 경우 다음 명령을 사용할 수 있습니다.

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  사용 하 여 새 가상 장치 만들기는 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) 명령입니다. 예를 들어 라는 AVD를 **AVD-Oreo-8.0** 26 API 수준에 대 한 시스템 이미지 x86를 사용 하 여 다음 명령을 사용 합니다.

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  와 메시지가 나타나면 **[아니요] 사용자 지정 하드웨어 프로필을 만들려면 원하는** 입력할 수 있는 **없는** 기본 하드웨어 프로필 क र च 합니다. 시킬 경우 **예**, **avdmanager** 표시 사용자 지정에 대 한 질문의 목록에서 하드웨어 프로필입니다.

사용 하 여 **avdmanager** 가상 장치를 만들려면 장치 풀 다운 메뉴에 포함 될지 것입니다.

[![새 AVD 장치 풀 다운 메뉴에 추가](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png)

-----

테스트 및 디버깅에 대 한 Android 에뮬레이터를 구성 하는 방법에 대 한 자세한 내용은 참조 [Android SDK 에뮬레이터](~/android/deploy-test/debugging/android-sdk-emulator/index.md)합니다.

장소 또는 픽셀 같은 물리적 장치를 사용 하는 경우에 무선 OTA 업데이트를 통해 자동을 통해 장치를 업데이트 하거나 시스템 이미지를 다운로드 하 고 장치를 직접 업데이트 수 합니다. Android Oreo에 장치를 수동으로 업데이트 하는 방법에 대 한 자세한 내용은 참조 [Nexus 장치와 픽셀에 대 한 기본 이미지](https://developers.google.com/android/images)합니다.


<a name="newfeatures" />

## <a name="new-features"></a>새 기능

Android Oreo 다양 한 새로운 기능 및 알림 채널, 알림 배지, XML의 글꼴을 사용자 지정, 다운로드 글꼴, 자동 채우기, 그림에서 그림 등의 기능을 소개합니다. 다음 섹션에서는 이러한 기능을 강조 표시 하 고 응용 프로그램에서 사용 하 여 시작할 수 있도록 링크를 제공 합니다.


<a name="notifchan" />

### <a name="notification-channels"></a>알림 채널

*알림 채널* 은 알림에 대 한 응용 프로그램 정의 범주입니다.
알림 전송, 해야 하는 각 형식에 대 한 알림 채널을 만들 수 있습니다 및 응용 프로그램의 사용자가 변경한 선택 항목을 반영 하기 위해 알림 채널을 만들 수 있습니다. 새 알림 채널 기능을 사용 하면 사용자가 다른 종류의 알림을 통해 세분화 된 제어를 제공할 수 있습니다. 예를 들어 메시징 응용 프로그램을 구현 하는 경우 사용자가 만든 각 대화 그룹에 대 한 별도 알림 채널을 만들 수 있습니다.

[알림 채널](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) 알림 채널을 만들고 로컬 알림을 게시에 대 한 사용 하는 방법을 설명 합니다. 실제 코드 예제를 참조 하십시오.는 [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) 샘플;이 샘플 응용 프로그램 채널 두 개를 관리 하 고 추가 알림 옵션을 설정 합니다.


<a name="notifbadge" />

### <a name="notification-badges"></a>알림 배지

알림 배지는이 스크린샷에 표시 된 대로 앱 아이콘 위에 표시 되는 작은 점.

[![응용 프로그램 아이콘에 알림 배지 예제](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png)

이러한 점은 해당 응용 프로그램 아이콘와 연결 된 응용 프로그램에서 하나 이상의 알림 채널에 대 한 새 알림이 나타냅니다 &ndash; 여기에 사용자가 아직 해제 하거나 작업을 수행 하는 알림이 표시 됩니다. 사용자 수 장기-아이콘에를 눌러 해제 있는지 또는 알림을 통해 장기 키를 눌러 메뉴에서 해당 appeaars 잠깐 알림 배지와 연결 된 알림 동안 합니다.

알림 배지에 대 한 자세한 내용은 Android 개발자 참조 [알림 배지](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) 항목입니다.


<a name="customfonts" />

### <a name="custom-fonts-in-xml"></a>XML의 글꼴을 사용자 지정

Android Oreo 소개 *XML에서 글꼴*, 리소스 그룹으로 사용자 지정 글꼴을 통합할 수 있게 합니다. OpenType (**.otf**) 및 트루타입 (**.ttf**) 글꼴 형식이 지원 됩니다. 글꼴 리소스를 추가 하려면 다음을 수행 합니다.

1. 만들기는 **리소스/글꼴** 폴더입니다.

2. 글꼴 파일을 복사 (예제에서는 **.ttf** 및 **.otf** 파일)를 **리소스/글꼴**합니다. 

3. 필요한 경우 파일의 이름을 바꿀 각 글꼴 Android 파일 명명 규칙을 따를 수 있도록 (즉, 사용 하 여 소문자만 *a ~ z*, *0-9*, 및 파일 이름에 밑줄). 예를 들어 글꼴 파일 `Pacifico-Regular.ttf` 와 같은 값으로 이름을 바꿀 수 없습니다. `pacifico.ttf`합니다.

4. New를 사용 하 여 사용자 지정 글꼴 적용 `android:fontFamily` 레이아웃 XML에서에서 특성입니다. 예를 들어, 다음 `TextView` 선언을 사용 하 여 추가 된 **pacifico.ttf** 글꼴 리소스:

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

또한 여러 글꼴 뿐 아니라 스타일 및 두께 세부 정보를 설명 하는 글꼴 패밀리 XML 파일을 만들 수 있습니다. 자세한 내용은 Android 개발자 참조 [XML에서 글꼴](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) 항목입니다.

<a name="dlfonts" />

### <a name="downloadable-fonts"></a>다운로드 가능한 글꼴

앱 Android Oreo 부터는 APK로 묶어 하지 않고 공급자에서 글꼴을 요청할 수 있습니다. 글꼴은 필요할 때만 네트워크에서 다운로드 됩니다. 이 기능은 전화 메모리 및 셀룰러 데이터 사용량을 절약 APK 크기를 줄여 줍니다. Android 지원 라이브러리로 26 패키지를 설치 하 여 Android API 버전 14 이상에서이 기능을 사용할 수도 있습니다.

만들 때 앱이 필요한 글꼴는 `FontsRequest` (다운로드 하려면 글꼴 지정) 하는 개체에 전달 된 후는 `FontsContract` 글꼴을 다운로드 하는 메서드. 다음 단계에는 글꼴 다운로드 과정을 자세히 설명합니다.

1.  인스턴스화하는 [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) 개체입니다. 

2.  서브 클래스 하 고 인스턴스화하여 [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html)합니다.

3.  구현 된 [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) 글꼴 요청 완료를 처리 하는 데 사용 되는 메서드.

4.  구현 된 [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) 메서드 글꼴 요청 프로세스 도중 발생 하는 오류가 발생 하는 앱을 알리는 데 사용 됩니다.

5.  호출 된 [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) 글꼴 공급자에서 글꼴을 검색 하는 메서드입니다. 

호출 하는 경우는 `RequestFonts` 메서드를 먼저 확인 하는 글꼴 로컬로 캐시 됩니다 (이전 호출에서 `RequestFont`). 캐시 되지 않으면 글꼴 공급자 호출, 글꼴을 비동기적으로 검색 하 고 다음 호출 하 여 응용 프로그램에 결과 전달 하면 `OnTypeFaceRetrieved` 메서드.

[다운로드 글꼴](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) 샘플 Android Oreo에 도입 된 글꼴을 다운로드할 수 있는 기능을 사용 하는 방법을 보여 줍니다. 

글꼴을 다운로드 하는 방법에 대 한 자세한 내용은 Android 개발자 참조 [다운로드 글꼴](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) 항목입니다.


<a name="autofill" />

### <a name="autofill"></a>자동 채우기

새 _자동 채우기_ Android Oreo에 프레임 워크를 통해 사용자가 쉽게 신용 카드 거래, 로그인 및 계정 만들기 등의 반복적인 작업을 처리할 수 있습니다. 사용자가 시간을 줄입니다 (이어질 수 입력 오류) 정보를 다시 입력 합니다. 응용 프로그램 자동 채우기 프레임 워크를 사용할 수 있습니다 (사용자가 활성화 하거나 비활성화할 수 자동 채우기)의 시스템 설정에서 자동 채우기 서비스 설정 해야 합니다.

[AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) 샘플 자동 채우기 프레임 워크의 사용법을 보여줍니다. Autofilled, 및 클라이언트 활동에 자동 채우기 데이터를 제공할 수 있는 서비스 해야 하는 뷰를 사용 하 여 클라이언트 활동의 구현이 포함 됩니다.

새 자동 채우기 기능 및 자동 채우기 용으로 앱을 최적화 하는 방법에 대 한 자세한 내용은 Android 개발자 참조 [자동 채우기 프레임 워크](https://developer.android.com/guide/topics/text/autofill.html) 항목입니다.


<a name="pip" />

### <a name="picture-in-picture-pip"></a>그림 (PIP)의 그림

Android Oreo 쉽게 처리할 수 그림에서 그림 (PIP) 모드에서 시작 하는 활동에 대 한 다른 작업의 화면을 중첩 합니다. 이 기능은 비디오 재생을 위한 것입니다.

앱의 활동 PIP 모드를 사용할 수 있는지를 지정 하려면 Android 매니페스트에서 true로 다음 플래그를 설정 합니다.

```xml
android:supportsPictureInPicture
```

PIP 모드일 때 활동 동작을 지정 하려면 새로운 사용 [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) 개체입니다. `PictureInPictureParams` 초기화 PIP 모드 (예를 들어 활동의 기본 가로 세로 비율)에서 활동을 업데이트 하는 데 사용 하는 매개 변수 집합을 나타냅니다. 에 추가 된 다음 새 PIP 메서드가 `Activity` Android Oreo에서:

-   [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; PIP 모드로 활동을 배치 합니다. 활동은 화면 아래에 있으며 화면의 나머지 화면에 있던 이전 작업으로 채워집니다.

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; 활동의 PIP 구성 설정 (예: 가로 세로 비율 변경)를 업데이트 합니다.

[PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) 샘플 Oreo에 도입 된 핸드헬드 장치에서 그림-에-그림 (PiP) 모드의 기본 사용법을 보여 줍니다. 샘플은 디스플레이 모드 또는 다른 활동 간에 앞뒤로 전환 하는 동안 중단 없이 계속 비디오를 재생 합니다.


<a name="other" />

### <a name="other-features"></a>기타 기능

다른 많은 Emoji 지원 라이브러리, 위치 API를 백그라운드 제한과 같은 새로운 기능, 광범위 한 색상 앱, 새 오디오 코덱, WebView의 향상 된 기능, 향상 된 키보드 탐색 지원 및에 대 한 새로운 AAudio (pro 오디오) API에 대 한 android Oreo 포함 이러한 기능에 대 한 자세한 정보에 대 한 고성능 대기 시간이 짧은 오디오 Android 개발자 참조 [Android Oreo 기능 및 Api](https://developer.android.com/about/versions/oreo/android-8.0.html) 항목입니다.


<a name="behavior" />

## <a name="behavior-changes"></a>동작 변경 내용

Android Oreo 다양 한 시스템 및 기존 앱의 기능에 영향을 줄 수 있는 API 변경 동작을 포함 합니다. 다음은 이러한 변경에 대 한 설명입니다.

<a name="bgsl" />

### <a name="background-execution-limits"></a>배경 실행 제한

환경을 개선 하기 위해 사용자를 Android Oreo에서는 앱에서 수행할 수 있는 작업에 제한을 설정 하는 동안 백그라운드에서 실행 되어야 합니다. 예를 들어 사용자가 비디오를 시청 하거나 게임을 하는 경우 백그라운드에서 실행 되는 앱이 포그라운드로 실행 되는 비디오를 많이 사용 응용 프로그램의 성능이 저하 될 수 없습니다. 결과적으로, Android Oreo 사용자와 직접 상호 작용 하지 앱에 다음과 같은 제한 사항이 넣습니다.

1.  **서비스 제한 백그라운드** &ndash; 를 몇 분간 여전히 수를 만들고 서비스를 사용 하는 창에는 응용 프로그램의 백그라운드에서 실행 중인 경우. 해당 창 끝 Android 응용 프로그램의 백그라운드 서비스를 중지 하 고 있는 것으로 취급 _유휴_합니다.

2.  **제한 사항 브로드캐스트** &ndash; Android 7.0 (API 25)를 받는 응용 프로그램을 등록 하는 브로드캐스트에 제한을 배치 합니다. Android Oreo를 사용 하면 이러한 제한 사항이 더 엄격한 있습니다. 예를 들어 Android Oreo 앱 매니페스트에서 암시적 브로드캐스트에 대 한 브로드캐스트 수신기 등록할 더 이상 수 없습니다.

새 배경 실행 제한에 대 한 자세한 내용은 Android 개발자 참조 [배경 실행 제한](https://developer.android.com/about/versions/oreo/background.html) 항목입니다.

<a name="breaking" />

### <a name="breaking-changes"></a>주요 변경 사항

Android Oreo 대상 또는 높을수록 해당 되는 다음과 같은 변경을 지원 하도록 앱을 수정 해야 하는 응용 프로그램의 경우:

- Android Oreo 더 이상 개별 알림의 우선 순위를 설정 하는 기능 하지 않는 합니다. 대신, 알림 채널을 만들 때 권장된 중요도 수준을 설정 합니다. 알림 채널에 할당 된 중요도 모든 알림 메시지를 게시 하 여에 적용 됩니다.

- Android Oreo를 대상으로 하는 앱에 대 한 `PendingIntent.GetService()` 백그라운드에서 시작 하는 서비스에 배치 하는 새로운 제한 되어 작동 하지 않습니다. Android Oreo 대상으로 하는 경우에 사용 해야 [PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/) 대신 합니다.  

<a name="sample_code" />

## <a name="sample-code"></a>샘플 코드

여러 가지 Xamarin.Android 샘플이 Android Oreo 기능을 활용 하는 방법을 보여 주는 사용할 수 있습니다.

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) Android Oreo에 도입 된 새 알림 채널 시스템을 사용 하는 방법을 보여 줍니다. 이 샘플에서는 두 개의 알림 채널을 관리: 기본 중요도 다른 한 대에 높은 중요도 사용 합니다.

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) Oreo에 도입 된 핸드헬드 장치에서 그림-에-그림 (PiP) 모드의 기본 사용법을 보여 줍니다. 샘플은 디스플레이 모드 또는 다른 활동 간에 앞뒤로 전환 하는 동안 중단 없이 계속 비디오를 재생 합니다.

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) 자동 채우기 프레임 워크의 사용법을 보여줍니다. Autofilled, 및 클라이언트 활동에 자동 채우기 데이터를 제공할 수 있는 서비스 해야 하는 뷰를 사용 하 여 클라이언트 활동의 구현이 포함 됩니다.

-   [다운로드 가능한 글꼴](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) 앞에서 설명한 다운로드 글꼴 기능을 사용 하는 방법의 예제를 제공 합니다.

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) EmojiCompat 지원 라이브러리의 사용 방법을 보여 줍니다. "두 부" 문자로 표시 된 누락 된 emoji 문자에서 응용 프로그램을 방지 하기 위해이 라이브러리를 사용할 수 있습니다.

-   [위치 업데이트 보류 중인 의도](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent) 장치의 위치에 대 한 업데이트를 가져올 위치 API의 사용법을 보여 줍니다를 사용 하는 `PendingIntent`합니다.

-   [위치 업데이트 전경 서비스](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService) 전경 경계와 시작 서비스를 사용 하 여 장치의 위치에 대 한 업데이트를 가져올 위치 API를 사용 하는 방법을 보여 줍니다.



<a name="summary" />

## <a name="summary"></a>요약

이 문서는 Android Oreo 도입 하 고 설치 하 고 Android Oreo에 있는 최신 도구와 Xamarin.Android 개발에 대 한 패키지를 구성 하는 방법을 설명 합니다. 몇 가지 새로운 기능에 대 한 예에서는 소스 코드에 대 한 링크가 있는 Android Oreo에서 사용할 수 있는 주요 기능의 개요를 제공 합니다. API 설명서에 대 한 링크를 포함 하 고 Oreo Android 용 응용 프로그램 만들기에 도움이 되는 Android 개발자 항목 시작 키를 누릅니다. 또한 기존 앱에 영향을 줄 수 있는 가장 중요 한 Android Oreo 동작 변경 내용 강조 표시 합니다.


## <a name="related-links"></a>관련 링크

- [Android 8.0 Oreo](https://developer.android.com/index.html)
