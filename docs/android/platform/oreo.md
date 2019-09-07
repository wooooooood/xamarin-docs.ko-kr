---
title: Oreo 기능
description: Android를 사용 하 여 최신 버전의 Android 용 앱을 개발 하는 방법을 알아봅니다.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 07/06/2018
ms.openlocfilehash: 0387cd91bd24080417a5e9763410d68b6e688555
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757502"
---
# <a name="oreo-features"></a>Oreo 기능

_Android를 사용 하 여 최신 버전의 Android 용 앱을 개발 하는 방법을 알아봅니다._

[Android 8.0 Oreo](https://developer.android.com/index.html) 는 Google에서 제공 하는 최신 버전의 android입니다. Android Oreo는 Xamarin Android 개발자에 게 유용한 여러 가지 새로운 기능을 제공 합니다. 이러한 기능에는 알림 채널, 알림 배지, XML의 사용자 지정 글꼴, 다운로드 가능한 글꼴, 자동 채우기 및 PIP (그림 그림)가 포함 됩니다. Android Oreo는 이러한 새로운 기능에 대 한 새로운 Api를 포함 하 고 있으며, Xamarin. Android 8.0 이상을 사용 하는 경우 Xamarin Android 앱에서 이러한 Api를 사용할 수 있습니다.

[![Android Oreo 주인공 이미지](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

이 문서는 Android 8.0 Oreo 용 Xamarin Android 앱 개발을 시작 하는 데 도움이 되는 구성입니다. 필요한 업데이트를 설치 하 고, SDK를 구성 하 고, 테스트를 위한 에뮬레이터 (또는 장치)를 만드는 방법을 설명 합니다. 또한 Android 8.0 Oreo의 새로운 기능에 대 한 개요를 제공 하 고, Xamarin Android 앱에서 Android Oreo 기능을 사용 하는 방법을 보여 주는 샘플 앱에 대 한 링크도 제공 합니다.

## <a name="requirements"></a>요구 사항

Xamarin 기반 앱에서 Android Oreo 기능을 사용 하려면 다음이 필요 합니다.

- **Visual Studio** &ndash; Windows를 사용 하는 경우 Visual Studio 버전 15.5 이상이 필요 합니다.  Mac을 사용 하는 경우 Mac용 Visual Studio 버전 7.2.0가 필요 합니다.

- Visual Studio를 사용 하 여 **xamarin android** &ndash; xamarin android 8.0 이상 버전을 설치 하 고 구성 해야 합니다.

- **Android SDK** &ndash; Android SDK Manager를 통해 Android SDK 8.0 (API 26) 이상을 설치 해야 합니다.

## <a name="getting-started"></a>시작하기

Android Oreo에서 android 사용을 시작 하려면 Android Oreo 프로젝트를 만들기 전에 최신 도구 및 SDK 패키지를 다운로드 하 여 설치 해야 합니다.

1. 최신 버전의 Visual Studio로 업데이트 합니다.

2. SDK Manager를 통해 **Android 8.0.0 (API 26)** 이상 패키지 및 도구를 설치 합니다.

3. Android Oreo (API 26)를 대상으로 하는 새 Xamarin Android 프로젝트를 만듭니다.

4. Android Oreo apps 테스트를 위한 에뮬레이터 또는 장치를 구성 합니다.

이러한 각 단계는 다음 섹션에 설명 되어 있습니다.

### <a name="update-visual-studio-and-xamarinandroid"></a>Visual Studio 및 Xamarin Android 업데이트

Visual Studio에 Android Oreo 지원을 추가 하려면 다음을 수행 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

- Visual Studio 2019의 경우 [SDK Manager](~/android/get-started/installation/android-sdk.md) 를 사용 하 여 API 수준 26.0 이상을 설치 합니다.

- Visual Studio 2017을 사용 하는 경우:

    1. Visual Studio 2017 버전 15.7 이상으로 업데이트 합니다 ( [Visual studio 2017 업데이트](https://docs.microsoft.com/visualstudio/install/update-visual-studio)참조).

    2. [SDK Manager](~/android/get-started/installation/android-sdk.md) 를 사용 하 여 API 수준 26.0 이상을 설치 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

- [Mac용 Visual Studio 업데이트](https://docs.microsoft.com/visualstudio/mac/update)에서 설명한 대로 안정적인 최신 버전의 Mac용 Visual Studio로 업데이트 합니다.

-----

Android Oreo Xamarin 지원에 대 한 자세한 내용은 [Xamarin android 8.0 릴리스](https://docs.microsoft.com/xamarin/android/release-notes/8/8.0/)정보를 참조 하세요.

### <a name="install-the-android-sdk"></a>Android SDK 설치

Xamarin. Android 8.0를 사용 하 여 프로젝트를 만들려면 먼저 Xamarin Android SDK Manager를 사용 하 여 **Android 8.0-Oreo** 이상용 SDK 플랫폼을 설치 해야 합니다. 또한 Android SDK Tools 26.0 이상 버전을 설치 해야 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. SDK Manager를 시작 합니다. Visual Studio에서 **도구 > Android > Android SDK Manager**)를 클릭 합니다.

2. **Android 8.0-Oreo** 패키지를 설치 합니다. Android SDK 에뮬레이터를 사용 하는 경우 필요한 **x86** 시스템 이미지를 포함 해야 합니다.

    [![Android SDK Manager에서 Android 8.0 패키지 선택](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. **Android SDK Tools 26.0.2** 이상, **Android SDK Platform tools 26.0.0** 이상을 설치 하 고 **빌드-도구 26.0.0** 이상 버전을 Android SDK 합니다.

    [![Android SDK 관리자에서 Android SDK Tools 26 선택](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. SDK Manager를 시작 합니다. Mac용 Visual Studio에서 **도구 > SDK manager**를 클릭 합니다.

2. **Android 8.0-Oreo** SDK 패키지를 설치 합니다. Android SDK 에뮬레이터를 사용 하는 경우 필요한 **x86** 시스템 이미지를 포함 해야 합니다.

    [![SDK Manager에서 Android 8.0 패키지 선택](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. **Android SDK Tools 26.0.2** 이상, **Android SDK Platform tools 26.0.0** 이상을 설치 하 고 **빌드-도구 26.0.0** 이상 버전을 Android SDK 합니다.

    [![SDK Manager에서 Android SDK Tools 26 선택](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----

### <a name="start-a-xamarinandroid-project"></a>Xamarin Android 프로젝트 시작

새 Xamarin Android 프로젝트를 만듭니다. Xamarin을 사용 하 여 Android 개발을 처음 접하는 경우에는 [Hello, android](~/android/get-started/hello-android/index.md) 를 참조 하 여 xamarin.ios 프로젝트를 만드는 방법에 대해 알아보세요.

Android 프로젝트를 만들 때 Android 8.0 이상 버전을 대상으로 하도록 버전 설정을 구성 해야 합니다. 예를 들어 Android 8.0에 대 한 프로젝트를 대상으로 하려면 프로젝트의 대상 Android API 수준을 **android 8.0 (API 26)** 로 구성 해야 합니다. 대상 프레임 워크 수준을 API 26 이상으로 설정 하는 것이 좋습니다. Android API 수준 수준을 구성 하는 방법에 대 한 자세한 내용은 [ANDROID Api 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조 하세요.

### <a name="configure-an-emulator-or-device"></a>에뮬레이터 또는 장치 구성

Android SDK Tools 26.0 이상을 설치한 후 기본 Google GUI 기반 AVD 관리자를 시작 하려고 하면 다음과 같은 오류 대화 상자가 표시 될 수 있습니다 .이 대화 상자에는 명령줄 AVD 관리자 도구 **avdmanager** 를 대신 사용 하도록 지시 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Android Emulator 관리자 경고 대화 상자](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Android Emulator 관리자 경고 대화 상자](oreo-images/mac/03-avd-warning.png)

-----

Google에서 더 이상 API 26.0 이상을 지 원하는 독립 실행형 GUI AVD 관리자를 제공 하지 않기 때문에이 메시지가 표시 됩니다. Android 8.0 Oreo의 경우 Xamarin Android Emulator Manager 또는 명령줄 `avdmanager` 도구를 사용 하 여 android Oreo에 대 한 가상 장치를 만들어야 합니다.

Android Device Manager를 사용 하 여 가상 장치를 만들고 관리 하려면 [Android Device Manager를 사용 하 여 가상 장치 관리](~/android/get-started/installation/android-emulator/device-manager.md)를 참조 하세요.
Android Device Manager 없이 가상 장치를 만들려면 다음 섹션의 단계를 수행 합니다.

#### <a name="creating-virtual-devices-using-avdmanager"></a>Avdmanager를 사용 하 여 가상 장치 만들기

**Avdmanager** 를 사용 하 여 새 가상 장치를 만들려면 다음 단계를 수행 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 명령 프롬프트 창을 열고를 컴퓨터의 `JAVA_HOME` Java SDK 위치로 설정 합니다. 일반적인 Xamarin 설치의 경우 다음 명령을 사용할 수 있습니다.

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2. Android SDK `bin` 폴더의 위치 `PATH`를에 추가 합니다.
    일반적인 Xamarin 설치의 경우 다음 명령을 사용할 수 있습니다.

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3. 명령 프롬프트 창을 닫고 새 명령 프롬프트 창을 엽니다. [Avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) 명령을 사용 하 여 새 가상 장치를 만듭니다. 예를 들어 API 레벨 26의 x86 시스템 이미지를 사용 하 여 avd **-Oreo-8.0** 라는 avd 라는 이름을 만들려면 다음 명령을 사용 합니다.

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4. **사용자 지정 하드웨어 프로필을 만들지** 묻는 메시지가 표시 되 면 **아니요** 를 입력 하 고 기본 하드웨어 프로필을 사용할 수 있습니다. **예**를 표시 하는 경우 **avdmanager** 는 하드웨어 프로필을 사용자 지정 하기 위한 질문 목록을 표시 합니다.

**Avdmanager** 를 통해 가상 장치를 만들면 장치 풀 다운 메뉴에 포함 됩니다.

[![새 AVD가 장치 풀 다운 메뉴에 추가 됨](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **터미널** 창을 열고 Mac에서 Android SDK tools 디렉터리의 위치로 변경 합니다. 일반적인 Xamarin 설치의 경우 다음 명령을 사용할 수 있습니다.

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2. [Avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) 명령을 사용 하 여 새 가상 장치를 만듭니다. 예를 들어 API 레벨 26의 x86 시스템 이미지를 사용 하 여 avd **-Oreo-8.0** 라는 avd 라는 이름을 만들려면 다음 명령을 사용 합니다.

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3. **사용자 지정 하드웨어 프로필을 만들지** 묻는 메시지가 표시 되 면 **아니요** 를 입력 하 고 기본 하드웨어 프로필을 사용할 수 있습니다. **예**를 표시 하는 경우 **avdmanager** 는 하드웨어 프로필을 사용자 지정 하기 위한 질문 목록을 표시 합니다.

**Avdmanager** 를 사용 하 여 가상 장치를 만든 후에는 장치 풀 다운 메뉴에 포함 됩니다.

[![새 AVD가 장치 풀 다운 메뉴에 추가 됨](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

테스트 및 디버깅을 위해 Android 에뮬레이터를 구성 하는 방법에 대 한 자세한 내용은 [Android Emulator의 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)을 참조 하세요.

Nexus 또는 픽셀과 같은 물리적 장치를 사용 하는 경우 OTA (air) 업데이트에서 자동으로 장치를 업데이트 하거나 시스템 이미지를 다운로드 하 여 장치를 직접 플래시 할 수 있습니다. Android Oreo 장치를 수동으로 업데이트 하는 방법에 대 한 자세한 내용은 [Nexus 및 픽셀 장치용 팩터리 이미지](https://developers.google.com/android/images)를 참조 하세요.

## <a name="new-features"></a>새 기능

Android Oreo에는 알림 채널, 알림 배지, XML의 사용자 지정 글꼴, 다운로드 가능한 글꼴, 자동 채우기, 그림 그림 등 다양 한 새로운 기능 및 기능이 도입 되었습니다. 다음 섹션에서는 이러한 기능을 강조 하 고 앱에서 사용을 시작 하는 데 도움이 되는 링크를 제공 합니다.

### <a name="notification-channels"></a>알림 채널

*알림 채널* 은 알림에 대 한 앱 정의 범주입니다.
전송 해야 하는 각 알림 유형에 대 한 알림 채널을 만들 수 있으며, 앱 사용자가 선택한 항목을 반영 하는 알림 채널을 만들 수 있습니다. 새 알림 채널 기능을 사용 하면 다양 한 종류의 알림을 사용자에 게 세분화 하 여 제어할 수 있습니다. 예를 들어 메시징 앱을 구현 하는 경우 사용자가 만든 각 대화 그룹에 대해 별도의 알림 채널을 만들 수 있습니다.

[알림 채널](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) 은 알림 채널을 만들고 로컬 알림을 게시 하는 데 사용 하는 방법을 설명 합니다. 실제 코드 예제는 [Notificationchannels](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-notificationchannels) 샘플을 참조 하세요. 이 샘플 앱은 두 채널을 관리 하 고 추가 알림 옵션을 설정 합니다.

### <a name="notification-badges"></a>알림 배지

알림 배지는 다음 스크린샷에 표시 된 것 처럼 앱 아이콘 위에 표시 되는 작은 점입니다.

[![앱 아이콘에 대 한 알림 배지 예제](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

이러한 점은 해당 앱 아이콘 &ndash; 에 연결 된 앱에서 하나 이상의 알림 채널에 대 한 새 알림이 있음을 나타내며, 사용자가 아직 해제 하거나 처리 하지 않은 알림입니다. 사용자는 경고 배지와 연결 된 알림을 한눈에 볼 수 있도록 아이콘을 길게 누를 수 있습니다 .이는 appeaars 하는 긴 누름 메뉴에서 알림을 해제 하거나 동작을 수행 합니다.

알림 배지에 대 한 자세한 내용은 Android 개발자 [알림 배지](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) 항목을 참조 하세요.

### <a name="custom-fonts-in-xml"></a>XML의 사용자 지정 글꼴

Android Oreo는 *XML의 글꼴*을 도입 하 여 사용자 지정 글꼴을 리소스로 통합할 수 있게 해줍니다. OpenType ( **.bf**) 및 TrueType ( **. .ttf**) 글꼴 형식이 지원 됩니다. 리소스로 글꼴을 추가 하려면 다음을 수행 합니다.

1. **리소스/글꼴** 폴더를 만듭니다.

2. 글꼴 파일 (예: **.ttf** 및 **. m f f** 파일)을 **리소스/글꼴**에 복사 합니다. 

3. 필요한 경우 Android 파일 명명 규칙을 준수 하도록 각 글꼴 파일의 이름을 바꿉니다 (즉, 파일 이름에 소문자 *a-z*, *0-9*및 밑줄만 사용). 예를 들어 글꼴 파일 `Pacifico-Regular.ttf` 의 이름을과 같은 `pacifico.ttf`이름으로 바꿀 수 있습니다.

4. 레이아웃 XML에서 새 `android:fontFamily` 특성을 사용 하 여 사용자 지정 글꼴을 적용 합니다. 예를 들어 다음 `TextView` 선언은 추가 된 **pacifico. .ttf** font 리소스를 사용 합니다.

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

또한 스타일 및 가중치 세부 정보 뿐만 아니라 여러 글꼴을 설명 하는 글꼴 패밀리 XML 파일을 만들 수 있습니다. 자세한 내용은 [XML의](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) Android 개발자 글꼴 항목을 참조 하세요.

### <a name="downloadable-fonts"></a>다운로드 가능한 글꼴

Android Oreo부터 앱은 APK에 번들을 포함 하지 않고 공급자에서 글꼴을 요청할 수 있습니다. 필요한 경우에만 글꼴이 네트워크에서 다운로드 됩니다. 이 기능은 APK 크기를 줄이고 휴대폰 메모리와 셀룰러 데이터 사용량을 절약 합니다. Android 지원 라이브러리 26 패키지를 설치 하 여 Android API 버전 14 이상에서이 기능을 사용할 수도 있습니다.

앱에 글꼴이 필요한 경우 다운로드할 글꼴을 지정 하 `FontsRequest` 여 개체를 만든 다음이 `FontsContract` 를 메서드에 전달 하 여 글꼴을 다운로드 합니다. 다음 단계에서는 글꼴 다운로드 프로세스에 대해 자세히 설명 합니다.

1. [글꼴 요청](https://developer.android.com/reference/android/provider/FontRequest.html) 개체를 인스턴스화합니다. 

2. [FontsContract Requestcallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html)을 서브 클래스 하 고 인스턴스화합니다.

3. 글꼴 요청의 완료를 처리 하는 데 사용 되는 [OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) 메서드를 구현 합니다.

4. 글꼴 요청 프로세스 중에 발생 하는 오류를 앱에 알리는 데 사용 되는 [OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) 메서드를 구현 합니다.

5. [FontsContract](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) 메서드를 호출 하 여 글꼴 공급자에서 글꼴을 검색 합니다. 

`RequestFonts` 메서드를 호출 하면 먼저 해당 글꼴이에 `RequestFont`대해 이전 호출에서 해당 글꼴이 로컬로 캐시 되는지 확인 합니다. 캐시 되지 않은 경우 글꼴 공급자를 호출 하 고, 글꼴을 비동기적으로 검색 한 다음, 메서드를 `OnTypeFaceRetrieved` 호출 하 여 결과를 다시 앱에 전달 합니다.

[다운로드 가능한 글꼴](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-downloadablefonts) 샘플에서는 Android Oreo에 도입 된 다운로드 가능한 글꼴 기능을 사용 하는 방법을 보여 줍니다. 

글꼴을 다운로드 하는 방법에 대 한 자세한 내용은 Android Developer [다운로드 가능한 글꼴](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) 항목을 참조 하세요.

### <a name="autofill"></a>목록의

Android Oreo의 새로운 자동 _채우기_ 프레임 워크를 사용 하면 사용자가 로그인, 계정 만들기 및 신용 카드 트랜잭션과 같은 반복적인 작업을 보다 쉽게 처리할 수 있습니다. 사용자는 정보를 다시 입력 하는 데 걸리는 시간을 단축 하 여 입력 오류가 발생할 수 있습니다. 자동 채우기 프레임 워크를 사용 하 여 앱을 사용할 수 있으려면 시스템 설정에서 자동 채우기 서비스를 사용 하도록 설정 해야 합니다 (사용자가 자동 채우기를 사용 하거나 사용 하지 않도록 설정할 수 있음).

[Autofillframework](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-autofillframework) 샘플에서는 자동 채우기 프레임 워크를 사용 하는 방법을 보여 줍니다. 여기에는 자동으로 채워야 하는 보기와 클라이언트 활동에 자동 채우기 데이터를 제공할 수 있는 서비스를 포함 하는 클라이언트 활동의 구현이 포함 됩니다.

새 자동 채우기 기능 및 자동 채우기를 위해 앱을 최적화 하는 방법에 대 한 자세한 내용은 Android Developer 자동 [채우기 프레임 워크](https://developer.android.com/guide/topics/text/autofill.html) 항목을 참조 하세요.

### <a name="picture-in-picture-pip"></a>그림 그림 (PIP)

Android Oreo를 사용 하면 다른 활동의 화면을 겹치게 하는 활동을 PIP (그림 내 그림) 모드로 시작할 수 있습니다. 이 기능은 비디오 재생을 위한 것입니다.

앱의 활동에서 PIP 모드를 사용할 수 있도록 지정 하려면 Android 매니페스트에서 다음 플래그를 true로 설정 합니다.

```xml
android:supportsPictureInPicture
```

PIP 모드에 있을 때 활동이 동작 하는 방식을 지정 하려면 새 PIP [Inpip params](https://developer.android.com/reference/android/app/PictureInPictureParams.html) 개체를 사용 합니다. `PictureInPictureParams`PIP 모드에서 활동을 초기화 하 고 업데이트 하는 데 사용 하는 매개 변수 집합을 나타냅니다 (예: 활동의 기본 가로 세로 비율). Android Oreo에서에 `Activity` 추가 된 새로운 PIP 메서드는 다음과 같습니다.

- [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; 활동을 PIP 모드로 전환 합니다. 작업은 화면의 모퉁이에 배치 되 고 나머지 화면은 화면에 있던 이전 작업으로 채워집니다.

- [Setpin, Params 매개 변수](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; 활동의 PIP 구성 설정을 업데이트 합니다 (예: 가로 세로 비율 변경).

그림 [샘플에서는](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-pictureinpicture) Oreo에 도입 된 핸드헬드 장치에 대 한 PiP (그림-사진) 모드의 기본 사용법을 보여 줍니다. 이 샘플은 표시 모드나 기타 작업 간을 전환 하는 동안 중단 없이 계속 되는 비디오를 재생 합니다.

### <a name="other-features"></a>기타 기능

Android Oreo에는 다른 여러 가지 새로운 기능, 위치 API, 앱에 대 한 배경 제한, 앱에 대 한 다양 한 색 영역 색, 새로운 오디오 코덱, 웹 보기 기능 향상, 향상 된 키보드 탐색 지원 및에 대 한 새로운 AAudio (pro 오디오) API 등이 포함 되어 있습니다. 대기 시간이 짧은 고성능 오디오. 이러한 기능에 대 한 자세한 내용은 Android 개발자 [Android Oreo 기능 및 api](https://developer.android.com/about/versions/oreo/android-8.0.html) 항목을 참조 하세요.

## <a name="behavior-changes"></a>동작 변경

Android Oreo에는 기존 앱의 기능에 영향을 줄 수 있는 다양 한 시스템 및 API 동작 변경 내용이 포함 되어 있습니다. 이러한 변경 내용은 다음과 같습니다.

### <a name="background-execution-limits"></a>백그라운드 실행 제한

사용자 환경을 개선 하기 위해 Android Oreo는 백그라운드에서 실행 하는 동안 수행할 수 있는 앱에 대 한 제한을 적용 합니다. 예를 들어 사용자가 비디오를 시청 하거나 게임을 재생 하는 경우 백그라운드에서 실행 중인 앱이 포그라운드에서 실행 되는 비디오 집약적 앱의 성능을 저하 시킬 수 있습니다. 따라서 Android Oreo는 사용자와 직접 상호 작용 하지 않는 앱에 다음과 같은 제한 사항을 적용 합니다.

1. **백그라운드 서비스 제한 사항** &ndash; 앱이 백그라운드에서 실행 되는 경우에도 서비스를 만들고 사용할 수 있는 몇 분의 창이 있습니다. 이 창의 끝에 Android는 앱의 백그라운드 서비스를 중지 하 고 _유휴_상태로 처리 합니다.

2. **브로드캐스트 제한 사항** &ndash; Android 7.0 (API 25) 앱이 수신 하도록 등록 하는 브로드캐스트에 대 한 제한 사항이 있습니다. Android Oreo는 이러한 제한 사항을 보다 엄격 하 게 만듭니다. 예를 들어 Android Oreo apps는 더 이상 매니페스트에 암시적 브로드캐스트에 대 한 브로드캐스트 수신기를 등록할 수 없습니다.

새 백그라운드 실행 제한에 대 한 자세한 내용은 Android Developer [Background Execution limits](https://developer.android.com/about/versions/oreo/background.html) 항목을 참조 하세요.

### <a name="breaking-changes"></a>주요 변경 사항

Android Oreo 이상을 대상으로 하는 앱은 해당 하는 경우 다음 변경을 지원 하도록 앱을 수정 해야 합니다.

- Android Oreo는 개별 알림의 우선 순위를 설정 하는 기능을 지지 합니다. 대신 알림 채널을 만들 때 권장 되는 중요도 수준을 설정 합니다. 알림 채널에 할당 하는 중요도 수준은 게시 하는 모든 알림 메시지에 적용 됩니다.

- Android Oreo를 대상으로 하 `PendingIntent.GetService()` 는 앱의 경우 백그라운드에서 시작 된 서비스에 적용 되는 새로운 제한으로 인해가 작동 하지 않습니다. Android Oreo를 대상으로 하는 경우에는 대신 [pendingintent](xref:Android.App.PendingIntent.GetBroadcast*)를 사용 해야 합니다.  

## <a name="sample-code"></a>예제 코드

Android Oreo 기능을 활용 하는 방법을 보여 주는 몇 가지 Xamarin Android 샘플을 사용할 수 있습니다.

- [NotificationsChannels](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-notificationchannels) 는 Android Oreo에 도입 된 새 알림 채널 시스템을 사용 하는 방법을 보여 줍니다. 이 샘플에서는 두 개의 알림 채널을 관리 합니다. 하나는 기본 중요도가 고 다른 하나는 중요도가 높습니다.

- [그림에](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-pictureinpicture) 는 Oreo에서 도입 된 핸드헬드 장치에 대 한 PiP (그림-사진) 모드의 기본 사용법을 보여 줍니다. 이 샘플은 표시 모드나 기타 작업 간을 전환 하는 동안 중단 없이 계속 되는 비디오를 재생 합니다.

- [Autofillframework는 자동](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-autofillframework) 채우기 프레임 워크의 사용을 보여 줍니다. 여기에는 자동으로 채워야 하는 보기와 클라이언트 활동에 자동 채우기 데이터를 제공할 수 있는 서비스를 포함 하는 클라이언트 활동의 구현이 포함 됩니다.

- [다운로드 가능한 글꼴](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-downloadablefonts) 은 앞에서 설명한 다운로드 가능한 글꼴 기능을 사용 하는 방법의 예를 제공 합니다.

- [EmojiCompat](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-emojicompat) 는 EmojiCompat 지원 라이브러리의 사용법을 보여 줍니다. 이 라이브러리를 사용 하 여 앱이 누락 된 tofu 문자를 "" 문자로 표시 하지 않도록 방지할 수 있습니다.

- [위치 업데이트 보류 중 의도](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-androidplaylocation-locupdpendintent) 는를 `PendingIntent`사용 하 여 장치 위치에 대 한 업데이트를 가져오는 location API의 사용법을 보여 줍니다.

- [위치 업데이트 포그라운드 서비스](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-androidplaylocation-locupdfgservice) 는 location API를 사용 하 여 바인딩된 및 시작 된 포그라운드 서비스를 사용 하 여 장치 위치에 대 한 업데이트를 가져오는 방법을 보여 줍니다.

## <a name="video"></a>비디오

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**Android 8.0 Oreo 개발C#**

## <a name="summary"></a>요약

이 문서에서는 android Oreo을 소개 하 고 Android Oreo에 대 한 최신 도구 및 패키지를 설치 하 고 구성 하는 방법을 설명 했습니다. Android Oreo에서 사용할 수 있는 주요 기능에 대 한 개요를 제공 하 고 몇 가지 새로운 기능에 대 한 소스 코드 예제 링크를 제공 합니다. Android Oreo 용 앱 만들기를 시작 하는 데 도움이 되는 API 설명서 및 Android 개발자 항목에 대 한 링크가 포함 되어 있습니다. 또한 기존 앱에 영향을 줄 수 있는 가장 중요 한 Android Oreo 동작 변경 내용도 강조 표시 되어 있습니다.

## <a name="related-links"></a>관련 링크

- [Android 8.0 Oreo](https://developer.android.com/index.html)
