---
title: Oreo 기능
description: Xamarin.Android를 사용 하 여 최신 버전의 Android 용 앱 개발을 시작 하는 방법.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 07/06/2018
ms.openlocfilehash: 24a9fa0e954ddba1451ba8bf98216550d7d70b51
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58854784"
---
# <a name="oreo-features"></a>Oreo 기능

_Xamarin.Android를 사용 하 여 최신 버전의 Android 용 앱 개발을 시작 하는 방법._

[Android 8.0 Oreo](https://developer.android.com/index.html) 은 Android의 최신 버전의 Google에서 사용할 수 있습니다. Android Oreo는 Xamarin.Android 개발자에 게 관심 있는 여러 가지 새로운 기능을 제공합니다. 이러한 기능에서 XML을 다운로드할 수 있는 글꼴, 자동 채우기, 및 화면 속 화면 (PIP) 알림 채널, 배지 알림, 사용자 지정 글꼴 포함 됩니다. Android Oreo 이러한 새 기능에 대 한 새 Api를 포함 하 고 이러한 Api는 이상 Xamarin.Android 8.0을 사용 하는 경우에 Xamarin.Android 앱에 사용할 수 있습니다.

[![Android Oreo 영웅 이미지](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

이 문서에서는 Android 8.0 Oreo 용 Xamarin.Android 앱 개발을 시작할 수 있도록 구성 되었습니다. 이 필요한 업데이트를 설치, SDK를 구성 및 테스트에 에뮬레이터 (또는 장치)를 만드는 방법을 설명 합니다. 또한 Xamarin.Android 앱의 Android Oreo 기능을 사용 하는 방법을 보여 주는 샘플 앱에 대 한 링크를 사용 하 여 Android 8.0 Oreo의 새로운 기능에 대 한 개요를 제공 합니다.


## <a name="requirements"></a>요구 사항

다음 해야 Xamarin 기반 앱에서 Android Oreo 기능을 사용 합니다.

-   **Visual Studio** &ndash; Windows를 사용 하는 경우 Visual studio 15.5 이상 버전이 필요 합니다.  Mac을 사용 하는 경우 버전 7.2.0 Mac 용 Visual Studio가 필요 합니다.

-   **Xamarin.Android** &ndash; Xamarin.Android 8.0 이상을 설치 하 고 Visual Studio를 사용 하 여 구성 해야 합니다.

-   **Android SDK** &ndash; Android SDK 8.0 (API 26) 이상을 해야 설치 된 Android SDK Manager를 통해.



## <a name="getting-started"></a>시작

Android Oreo Xamarin.Android와 함께 사용 하 여 시작 하려면 다운로드 하며 Android oreo 용 프로젝트를 만들려면 먼저 최신 도구 및 SDK 패키지를 설치 합니다.

1. Visual Studio의 최신 버전으로 업데이트 합니다.

2. 설치 합니다 **Android 8.0.0 (API 26)** 이상 패키지 및 SDK 관리자를 통해 도구입니다.

3. Android Oreo (API 26)를 대상으로 하는 새 Xamarin.Android 프로젝트를 만듭니다.

4. 에뮬레이터 또는 Android oreo 용 앱을 테스트 하는 것에 대 한 장치를 구성 합니다.

이러한 각 단계는 다음 섹션에서 설명 됩니다.

### <a name="update-visual-studio-and-xamarinandroid"></a>Visual Studio를 업데이트 및 Xamarin.Android

Visual Studio에 Android Oreo 지원을 추가 하려면 다음을 수행 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

- Visual Studio 2019 사용 합니다 [SDK Manager](~/android/get-started/installation/android-sdk.md) API 수준 26.0 이상 설치 합니다.

- Visual Studio 2017을 사용 하는 경우

    1. Visual Studio 2017 버전 15.7 이상의 업데이트 (참조 [Visual Studio 2017 업데이트](https://docs.microsoft.com/visualstudio/install/update-visual-studio)).

    2. 사용 합니다 [SDK Manager](~/android/get-started/installation/android-sdk.md) API 수준 26.0 이상 설치 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

- 에 설명 된 대로 Mac 용 Visual Studio의 안정적인 최신 버전으로 업데이트 [Mac 용 Visual Studio 업데이트](https://docs.microsoft.com/visualstudio/mac/update)합니다.

-----

Android Oreo 용 Xamarin 지원에 대 한 자세한 내용은 참조는 [Xamarin.Android 8.0 릴리스](https://docs.microsoft.com/xamarin/android/release-notes/8/8.0/)합니다.



### <a name="install-the-android-sdk"></a>Android SDK 설치

Xamarin.Android 8.0을 사용 하 여 프로젝트를 만들려면 하면 먼저 사용 하 여 Xamarin Android SDK Manager에 대 한 SDK 플랫폼을 설치 하려면 **Android 8.0-Oreo** 이상. Android SDK Tools 26.0 이상 설치 해야 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. SDK Manager 시작 (Visual Studio에서 클릭 **도구 > Android > Android SDK Manager**).

2. 설치 합니다 **Android 8.0-Oreo** 패키지 있습니다. Android SDK 에뮬레이터를 사용 하는 경우 포함 해야 합니다 **x86** 해야 하는 시스템 이미지:

    [![Android SDK Manager에서 Android 8.0 패키지 선택](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. 설치할 **Android SDK Tools 26.0.2** 이상을 **Android SDK Platform-tools 26.0.0** 이상, 및 **Android SDK Build-tools 26.0.0** (또는 이상):

    [![Android SDK Manager에서 Android SDK Tools 26 선택](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. SDK Manager 시작 (Mac 용 Visual Studio에서 클릭 **도구 > SDK Manager**).

2. 설치 합니다 **Android 8.0-Oreo** SDK 패키지 있습니다. Android SDK 에뮬레이터를 사용 하는 경우 포함 해야 합니다 **x86** 해야 하는 시스템 이미지:

    [![SDK Manager에서 Android 8.0 패키지 선택](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. 설치할 **Android SDK Tools 26.0.2** 이상을 **Android SDK Platform-tools 26.0.0** 이상, 및 **Android SDK Build-tools 26.0.0** (또는 이상):

    [![SDK Manager에서 Android SDK Tools 26 선택](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----



### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android 프로젝트를 시작 합니다.

새 Xamarin.Android 프로젝트를 만듭니다. Xamarin 사용한 Android 개발을 처음 접하는 경우 참조 [Hello, Android](~/android/get-started/hello-android/index.md) Xamarin.Android 프로젝트 만들기에 대해 알아보려면 합니다.

Android 프로젝트를 만든 경우에 대상 Android 8.0 이상 버전 설정을 구성 해야 합니다. 예를 들어 Android 8.0 용 프로젝트가 대상으로 구성 해야 하는 프로젝트의 대상 Android API 레벨 **Android 8.0 (API 26)** 합니다. 대상 프레임 워크 수준 API 26 이상도 설정 하는 것이 좋습니다. Android API 수준 수준을 구성 하는 방법에 대 한 자세한 내용은 참조 하세요 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다.


### <a name="configure-an-emulator-or-device"></a>에뮬레이터 또는 장치를 구성 합니다.

Android SDK Tools 26.0 설치 후 기본 Google GUI 기반 AVD 관리자를 시작 하려는 경우 나중에 명령줄 AVD manager 도구를 사용 하 고 다음 오류 대화 상자를 표시 될 수 있습니다 **avdmanager** 대신 :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Android Emulator 관리자 경고 대화 상자](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Android Emulator 관리자 경고 대화 상자](oreo-images/mac/03-avd-warning.png)

-----

Google API 26.0 이상 지 원하는 GUI AVD manager 독립 실행형을 더 이상 제공 하므로이 메시지가 표시 됩니다. Android 8.0 Oreo 용 Xamarin Android Emulator 관리자 또는 명령줄 중 하나를 사용 해야 `avdmanager` Android Oreo 용 가상 장치를 만드는 도구입니다.

Android 장치 관리자를 만들고 가상 장치 관리를 사용 하려면 참조 [가상 장치 관리 Android 장치 관리자를 사용 하 여](~/android/get-started/installation/android-emulator/device-manager.md)입니다.
가상 장치 없이 Android 장치 관리자를 만들려면 다음 섹션의 단계를 따릅니다.


#### <a name="creating-virtual-devices-using-avdmanager"></a>Avdmanager 사용 하 여 가상 장치 만들기

사용 하도록 **avdmanager** 새 가상 장치를 만들려면 다음이 단계를 수행 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  명령 프롬프트 창을 열고 설정 `JAVA_HOME` 컴퓨터에서 Java SDK의 위치입니다. 일반적인 Xamarin 설치의 경우 다음 명령을 사용할 수 있습니다.

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Android SDK의 위치를 추가 `bin` 폴더에 `PATH`입니다.
    일반적인 Xamarin 설치의 경우 다음 명령을 사용할 수 있습니다.

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  명령 프롬프트 창을 닫고 새로운 명령 프롬프트 창을 엽니다. 사용 하 여 새 가상 장치 만들기를 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) 명령입니다. 예를 들어 라는 AVD를 만들려는 **8.0 Oreo-AVD-** 26, API 수준에 대 한 시스템 이미지는 x86을 사용 하 여 다음 명령을 사용 합니다.

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  사용 하 여 메시지가 나타나면 **[아니요] 사용자 지정 하드웨어 프로필을 만들려는** 입력할 수 있습니다 **없습니다** 및 기본 하드웨어 프로필을 적용 합니다. 반응을 **yes**, **avdmanager** 하드웨어 프로필을 사용자 지정에 대 한 질문의 목록으로 라는 메시지가 나타납니다.

한 후 **avdmanager** 가상 장치를 만들려면 장치 풀 다운 메뉴에 포함 될지 것입니다.

[![새 AVD 장치 풀 다운 메뉴에 추가](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  엽니다는 **터미널** 창과 mac에서 Android SDK tools 디렉터리의 위치 변경 일반적인 Xamarin 설치의 경우 다음 명령을 사용할 수 있습니다.

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  사용 하 여 새 가상 장치 만들기를 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) 명령입니다. 예를 들어 라는 AVD를 만들려는 **8.0 Oreo-AVD-** 26, API 수준에 대 한 시스템 이미지는 x86을 사용 하 여 다음 명령을 사용 합니다.

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  사용 하 여 메시지가 나타나면 **[아니요] 사용자 지정 하드웨어 프로필을 만들려는** 입력할 수 있습니다 **없습니다** 및 기본 하드웨어 프로필을 적용 합니다. 반응을 **yes**, **avdmanager** 하드웨어 프로필을 사용자 지정에 대 한 질문의 목록으로 라는 메시지가 나타납니다.

사용 하 여 **avdmanager** 가상 장치를 만들려면 장치 풀 다운 메뉴에 포함 될지 것입니다.

[![새 AVD 장치 풀 다운 메뉴에 추가](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

테스트 및 디버깅에 대 한 Android 에뮬레이터를 구성 하는 방법에 대 한 자세한 내용은 참조 하세요. [Android 에뮬레이터에서 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)합니다.

픽셀을 Nexus 등 물리적 장치를 사용 하는 경우에 무선 OTA 업데이트를 통해 자동을 통해 장치를 업데이트 하거나 시스템 이미지를 다운로드 하 고 장치를 직접 플래시 수 있습니다. Android Oreo에 장치를 수동으로 업데이트 하는 방법에 대 한 자세한 내용은 참조 하세요. [Nexus 픽셀 장치를 출하 시 이미지](https://developers.google.com/android/images)합니다.



## <a name="new-features"></a>새 기능

Android Oreo 다양 한 새로운 기능 및 알림 채널, 알림 배지, XML에서 사용자 지정 글꼴, 다운로드할 수 있는 글꼴, 자동 채우기, 및 그림-그림에서와 같은 기능을 소개합니다. 다음 섹션에서는 이러한 기능을 강조 하 고 앱에서 사용 하는 데 대 한 링크 시작을 제공 합니다.



### <a name="notification-channels"></a>알림 채널

*알림 채널* 알림에 대 한 범주 앱 정의 됩니다.
알림 전송 해야 하는 각 형식에 대 한 알림 채널을 만들고 앱의 사용자가 변경한 선택을 반영 하도록 알림 채널을 만들 수 있습니다. 새 알림 채널 기능을 사용 하면 사용자가 다른 종류의 알림 세부적으로 제어를 제공할 수 있습니다. 예를 들어, 메시징 앱을 구현 하는 경우 사용자가 만든 각 대화 그룹에 대 한 별도 알림 채널을 만들 수 있습니다.

[알림 채널](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) 알림 채널을 만들고 사용 하 여 로컬 알림을 게시 하는 방법을 설명 합니다. 실제 코드 예제를 참조 하세요. 합니다 [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) 샘플; 샘플 앱이 두 채널을 관리 하 고 추가 알림 옵션을 설정 합니다.



### <a name="notification-badges"></a>알림 배지

알림 배지가 스크린샷에 표시 된 대로 앱 아이콘 위에 표시 되는 작은 점 같습니다.

[![앱 아이콘에 알림 배지를 예제](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

이러한 점을 나타내는 앱 아이콘을 사용 하 여 연결 된 앱에서 하나 이상의 알림 채널에 대 한 새 알림이 &ndash; 이들은 사용자가 아직 해제 되지 않았거나 취해야 하는 알림입니다. 사용자 수 장기-아이콘 키를 눌러 해제 하거나 해당 appeaars 장기-키를 눌러 메뉴에서 알림에서 작동 하는 알림 배지를 사용 하 여 연관 된 알림도 살펴봅니다.

알림 배지에 대 한 자세한 내용은 Android 개발자 참조 [알림 배지](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) 항목입니다.



### <a name="custom-fonts-in-xml"></a>XML에서 사용자 지정 글꼴

Android Oreo 소개 *xml에서 글꼴*,이 통해 리소스로 사용자 지정 글꼴을 통합할 수 있습니다. OpenType (**.otf**) 및 트루타입 (**.ttf**) 글꼴 형식이 지원 됩니다. 글꼴을 리소스로 추가 하려면 다음을 수행 합니다.

1. 만들기는 **글꼴리소스/** 폴더입니다.

2. 글꼴 파일을 복사 (예에서 **.ttf** 하 고 **.otf** 파일)를 **리소스/글꼴**합니다. 

3. 필요한 경우 Android 파일 명명 규칙을 따를 수 있도록 각 글꼴 파일 바꿀 (즉, 사용 하 여 소문자만 *ㄱ-ㅎ*, *0-9*, 및 파일 이름에 밑줄). 예를 들어 글꼴 파일 `Pacifico-Regular.ttf` 와 같은 것으로 바꿀 수 없습니다 `pacifico.ttf`합니다.

4. New를 사용 하 여 사용자 지정 글꼴을 적용할 `android:fontFamily` 레이아웃 XML 특성입니다. 다음 예를 들어 `TextView` 선언을 사용 하 여 추가한 **pacifico.ttf** 글꼴 리소스:

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

또한 스타일 및 두께 정보 뿐만 아니라 여러 글꼴을 설명 하는 글꼴 패밀리 XML 파일을 만들 수 있습니다. 자세한 내용은 Android 개발자 참조 [xml에서 글꼴](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) 항목입니다.


### <a name="downloadable-fonts"></a>다운로드할 수 있는 글꼴

앱 Android Oreo 부터는 APK를 번들 하는 것이 아니라 공급자에서 글꼴을 요청할 수 있습니다. 글꼴은 필요할 때만 네트워크에서 다운로드 됩니다. 이 기능은 전화 메모리 및 셀룰러 데이터 사용을 절약 하 고 APK 크기를 줄여 줍니다. 또한 Android 지원 라이브러리 26 패키지를 설치 하 여 Android API 버전 14 이상에서이 기능을 사용할 수 있습니다.

만든 앱에서 글꼴을 필요한 경우는 `FontsRequest` 개체 (다운로드 하려면 글꼴을 지정 합니다.)를 전달는 `FontsContract` 글꼴을 다운로드 하는 방법입니다. 다음 단계를 자세히 글꼴 다운로드 프로세스를 설명합니다.

1.  인스턴스화하는 [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) 개체입니다. 

2.  하위 클래스 및 인스턴스화 [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html)합니다.

3.  구현 된 [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) 글꼴 요청 완료를 처리 하는 데 사용 되는 메서드.

4.  구현 된 [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) 메서드를 글꼴 요청 프로세스 동안 발생 하는 오류가 발생 하는 앱을 알리는 데 사용 됩니다.

5.  호출 된 [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) 글꼴 공급자에서 글꼴을 검색 하는 방법입니다. 

호출 하는 경우는 `RequestFonts` 메서드를 먼저 확인 하는 글꼴 로컬로 캐시 된 경우 (이전 호출에서 `RequestFont`). 캐시 되지 않으면 글꼴 공급자 호출, 글꼴을 비동기적으로 검색 하 고 다음 호출 하 여 앱에 다시 결과 전달 하면 `OnTypeFaceRetrieved` 메서드.

합니다 [다운로드할 수 있는 글꼴](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) 샘플 Android Oreo에서 도입 된 다운로드 가능한 글꼴 기능을 사용 하는 방법에 설명 합니다. 

글꼴을 다운로드 하는 방법에 대 한 자세한 내용은 Android 개발자 참조 [다운로드할 수 있는 글꼴](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) 항목입니다.



### <a name="autofill"></a>자동 채우기

새 _Autofill_ Android Oreo에서 프레임 워크를 통해 쉽게 로그인, 계정 만들기 및 신용 카드 거래와 같은 반복적인 작업을 처리할 수 있습니다. 사용자 정보 (입력 오류를 발생할 수 있습니다)를 다시 입력 시간을 줄입니다. 앱은 Autofill Framework를 사용 하 여 사용할 수 있도록, 자동 채우기 서비스를 (사용자 활성화 하거나 비활성화할 수 자동 채우기) 시스템 설정에서 활성화 되어야 합니다.

합니다 [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) Autofill Framework의 사용법을 보여 줍니다. Autofilled, 및 클라이언트 활동을 자동 채우기 데이터를 제공할 수 있는 서비스 해야 하는 뷰를 사용 하 여 클라이언트 작업의 구현이 포함 됩니다.

새로운 자동 채우기 기능 및 자동 채우기 기능에 대 한 앱을 최적화 하는 방법에 대 한 자세한 내용은 Android 개발자 참조 [Autofill Framework](https://developer.android.com/guide/topics/text/autofill.html) 항목입니다.



### <a name="picture-in-picture-pip"></a>그림 (PIP)의 그림

Android Oreo 수 있도록 그림에서 그림 (PIP) 모드에서 시작 하기 위한 작업에 대 한 다른 활동의 화면을 오버레이 합니다. 이 기능은 비디오 재생을 위한 것입니다.

앱의 활동 PIP 모드를 사용할 수 있는지를 지정 하려면 Android 매니페스트에 true로 다음 플래그를 설정 합니다.

```xml
android:supportsPictureInPicture
```

PIP 모드일 때 작업 동작을 지정 하려면 새 사용 [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) 개체입니다. `PictureInPictureParams` 초기화 하 고 PIP 모드 (예를 들어 활동의 기본 가로 세로 비율)의 활동을 업데이트 하는 데 사용할 수 있는 매개 변수 집합을 나타냅니다. 다음 새 PIP 메서드가 추가 되었습니다 `Activity` Android Oreo에서:

-   [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; PIP 모드로 설정 하 고 작업 합니다. 활동 화면 상단에 놓이고 화면 나머지 화면에 있는 이전 작업으로 채워집니다.

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; 활동의 PIP 구성 설정 (예를 들어, 가로 세로 비율 변경)을 업데이트 합니다.

합니다 [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) 샘플 Oreo에 도입 된 핸드헬드 장치에 대 한 그림-에-그림 (PiP) 모드의 기본 사용법을 보여 줍니다. 샘플은 디스플레이 모드 또는 다른 활동 간에 앞뒤로 전환 하는 동안 중단 없이 계속 되는 비디오를 재생 합니다.



### <a name="other-features"></a>기타 기능

Android Oreo 포함, 광범위 한 색상 앱, 새 오디오 코덱, WebView 향상 된 기능, 향상 된 키보드 탐색 지원 및에 대 한 새 AAudio (pro 오디오) API에 대 한 다른 많은 모 지 지원 라이브러리, 위치 API 백그라운드 제한과 같은 새로운 기능 고성능 지연율이 낮은 오디오, 이러한 기능에 대 한 자세한 내용은 Android 개발자 참조 [Android Oreo 기능 및 Api](https://developer.android.com/about/versions/oreo/android-8.0.html) 항목입니다.



## <a name="behavior-changes"></a>동작 변경 내용

Android Oreo 다양 한 시스템 및 기존 앱의 기능에 영향을 미칠 수 있는 API 변경 동작을 포함 합니다. 다음은 이러한 변경에 대 한 설명입니다.


### <a name="background-execution-limits"></a>백그라운드 실행 제한

사용자 환경을 개선 하기 위해 Android Oreo에 제한을 앱에서 수행할 수 있는 작업을 백그라운드에서 실행 하는 동안. 예를 들어 사용자가 비디오를 시청 하거나 게임을 하는 경우 백그라운드에서 실행 되는 앱 포그라운드에서 실행 되는 비디오를 많이 사용 앱의 성능을 저하 시킬 수 있습니다. 결과적으로, Android Oreo가 직접 사용자 상호 작용 하지 앱에는 다음과 같은 제한에 넣습니다.

1.  **서비스 제한 사항 백그라운드** &ndash; 는 몇 분의도 수 만들고 서비스를 사용 하는 창에는 앱이 백그라운드에서 실행 하는 경우. 해당 창 끝 Android 앱의 백그라운드 서비스가 중지 되 고 있는 것으로 취급 _유휴_합니다.

2.  **제한 브로드캐스트** &ndash; Android 7.0 (API 25) 앱을 받으려면 등록 하는 브로드캐스트에 제한 사항을 배치 합니다. Android Oreo를 사용 하면 이러한 제한 사항이 더 엄격한 있습니다. 예를 들어 Android Oreo 앱 매니페스트에서 암시적 브로드캐스트에 대 한 브로드캐스트 수신기를 등록할 더 이상 수 없습니다.

새 백그라운드 실행 제한에 대 한 자세한 내용은 Android 개발자 참조 [백그라운드 실행 제한](https://developer.android.com/about/versions/oreo/background.html) 항목입니다.


### <a name="breaking-changes"></a>주요 변경 사항

Android Oreo를 대상 또는 높은 해당 하는 경우 다음 변경 내용을 지원 하기 위해 해당 앱을 수정 해야 하는 응용 프로그램의 경우:

- Android Oreo 개별 알림의 우선 순위를 설정 하는 기능을 사용 합니다. 대신, 알림 채널을 만들 때 권장 되는 중요도 수준을 설정 합니다. 알림 채널을 할당할 중요도 수준을 모든 알림 메시지에 게시에 적용 됩니다.

- Android Oreo를 대상으로 하는 앱에 대 한 `PendingIntent.GetService()` 백그라운드에서 시작 하는 서비스에 배치 하는 새 제한으로 인해 작동 하지 않습니다. Android Oreo를 대상으로 하는 경우 사용 해야 [PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/) 대신 합니다.  


## <a name="sample-code"></a>샘플 코드

여러 Xamarin.Android 샘플은 Android Oreo 기능을 활용 하는 방법을 보여 줍니다.

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) Android Oreo에서 도입 된 새 알림 채널 시스템을 사용 하는 방법에 설명 합니다. 이 샘플에서는 두 개의 알림 채널을 관리 합니다: 기본 중요도 및 높은 중요도로 다른 합니다.

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) Oreo에 도입 된 핸드헬드 장치에 대 한 그림-에-그림 (PiP) 모드의 기본 사용법을 보여 줍니다. 샘플은 디스플레이 모드 또는 다른 활동 간에 앞뒤로 전환 하는 동안 중단 없이 계속 되는 비디오를 재생 합니다.

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) Autofill Framework의 사용을 보여 줍니다. Autofilled, 및 클라이언트 활동을 자동 채우기 데이터를 제공할 수 있는 서비스 해야 하는 뷰를 사용 하 여 클라이언트 작업의 구현이 포함 됩니다.

-   [다운로드할 수 있는 글꼴](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) 앞에서 설명한 다운로드 글꼴 기능을 사용 하는 방법의 예제를 제공 합니다.

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) EmojiCompat 지원 라이브러리의 사용을 보여 줍니다. "두 부" 문자로 표시 누락이 모 지 문자에서 앱을 방지 하기 위해이 라이브러리를 사용할 수 있습니다.

-   [위치 업데이트 Pending Intent](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent) 장치의 위치에 대 한 업데이트를 가져올 위치 API의 사용법을 보여 줍니다를 사용 하는 `PendingIntent`합니다.

-   [위치 업데이트 포그라운드 서비스](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService) 바인딩되고 시작 포그라운드 서비스를 사용 하 여 장치의 위치에 대 한 업데이트를 가져올 위치 API를 사용 하는 방법에 설명 합니다.


## <a name="video"></a>비디오

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**C#을 사용한 android 8.0 Oreo 개발**


## <a name="summary"></a>요약

이 문서에서는 Android Oreo 도입 하 고 설치 및 Android Oreo의 최신 도구 및 Xamarin.Android 개발에 대 한 패키지를 구성 하는 방법을 설명 합니다. 몇 가지 새로운 기능에 대 한 소스 코드 예제에 대 한 링크를 사용 하 여 Android Oreo에서 사용할 수 있는 주요 기능에 간략하게를 제공 합니다. API 설명서에 대 한 링크를 포함 하 고 Android Oreo 용 앱을 만들 수 있도록 Android 개발자 항목 시작 합니다. 또한 기존 앱에 영향을 줄 수 있는 가장 중요 한 Android Oreo 동작 변경 내용 강조 표시 합니다.


## <a name="related-links"></a>관련 링크

- [Android 8.0 Oreo](https://developer.android.com/index.html)
