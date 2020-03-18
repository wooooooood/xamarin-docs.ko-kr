---
title: Oreo 기능
description: Xamarin.Android를 사용하여 최신 버전 Android용 앱을 개발하는 방법을 알아봅니다.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: davidortinau
ms.author: daortin
ms.date: 07/06/2018
ms.openlocfilehash: 56430f8c4988c16a31f9806b0ffb8b6355d6340b
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019993"
---
# <a name="oreo-features"></a>Oreo 기능

_Xamarin.Android를 사용하여 최신 버전 Android용 앱을 개발하는 방법을 알아봅니다._

[Android 8.0 Oreo](https://developer.android.com/index.html)는 Google에서 사용할 수 있는 최신 버전의 Android입니다. Android Oreo는 Xamarin.Android 개발자에게 도움이 되는 여러 가지 새 기능을 제공합니다. 이러한 기능에는 알림 채널, 알림 배지, XML 사용자 지정 글꼴, 다운로드 가능한 글꼴, 자동 채우기 및 PIP(Picture in Picture)가 포함됩니다. Android Oreo는 이러한 새 기능을 위한 새로운 API를 포함하고 있으며, Xamarin.Android 8.0 이상을 사용하는 경우 Xamarin.Android 앱에서 이러한 API를 사용할 수 있습니다.

[![Android Oreo 히어로 이미지](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

이 문서는 Android 8.0 Oreo용 Xamarin.Android 앱 개발을 시작하는 데 도움이 되도록 구성되었습니다. 필요한 업데이트를 설치하고, SDK를 구성하고, 테스트할 에뮬레이터(또는 디바이스)를 준비하는 방법을 설명합니다. 또한 Android 8.0 Oreo의 새 기능을 개략적으로 설명하고 Xamarin.Android 앱에서 Android Oreo 기능을 사용하는 방법을 보여 주는 샘플 앱에 대한 링크도 제공합니다.

## <a name="requirements"></a>요구 사항

Xamarin 기반 앱에서 Android Oreo 기능을 사용하려면 다음이 필요합니다.

- **Visual Studio** &ndash; Windows를 사용하는 경우 Visual Studio 버전 15.5 이상이 필요합니다.  Mac을 사용하는 경우 Mac용 Visual Studio 버전 7.2.0이 필요합니다.

- **Xamarin.Android** &ndash; Visual Studio에서 Xamarin.Android 8.0  이상을 설치하고 구성해야 합니다.

- **Android SDK** &ndash; Android SDK 관리자를 통해 Android SDK 8.0(API 26) 이상을 설치해야 합니다.

## <a name="getting-started"></a>시작

Xamarin.Android에서 Android Oreo를 사용하려면 먼저 최신 도구 및 SDK 패키지를 다운로드하여 설치해야 합니다. 그러면 Android Oreo 프로젝트를 만들 수 있습니다.

1. 최신 버전의 Visual Studio로 업데이트합니다.

2. SDK 관리자를 통해 **Android 8.0.0(API 26)** 이상 패키지 및 도구를 설치합니다.

3. Android Oreo(API 26)를 대상으로 하는 새 Xamarin.Android 프로젝트를 만듭니다.

4. Android Oreo 앱을 테스트하기 위한 에뮬레이터 또는 디바이스를 구성합니다.

각 단계는 다음 섹션에서 설명합니다.

### <a name="update-visual-studio-and-xamarinandroid"></a>Visual Studio 및 Xamarin.Android 업데이트

Visual Studio에 Android Oreo 지원을 추가하려면 다음을 수행합니다.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

- Visual Studio 2019의 경우 [SDK 관리자](~/android/get-started/installation/android-sdk.md)를 사용하여 API 수준 26.0 이상을 설치합니다.

- Visual Studio 2017을 사용하는 경우

    1. Visual Studio 2017 버전 15.7 이상으로 업데이트합니다([Visual Studio 2017업데이트](https://docs.microsoft.com/visualstudio/install/update-visual-studio) 참조).

    2. [SDK 관리자](~/android/get-started/installation/android-sdk.md)를 사용하여 API 수준 26.0 이상을 설치합니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

- [Mac용 Visual Studio 업데이트](https://docs.microsoft.com/visualstudio/mac/update)에 설명된 대로 Mac용 Visual Studio의 최신 안정화 버전으로 업데이트합니다.

-----

Android Oreo에 대한 Xamarin 지원에 대한 자세한 내용은 [Xamarin.Android 8.0 릴리스 정보](https://docs.microsoft.com/xamarin/android/release-notes/8/8.0/)를 참조하세요.

### <a name="install-the-android-sdk"></a>Android SDK 설치

Xamarin.Android 8.0을 통해 프로젝트를 만들려면 먼저 Xamarin Android SDK 관리자를 사용하여 **Android 8.0 - Oreo** 이상 버전용 SDK 플랫폼을 설치해야 합니다. 또한 Android SDK Tools 26.0 이상 버전을 설치해야 합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. SDK 관리자를 시작합니다(Visual Studio에서 **도구 > Android > Android SDK 관리자**를 클릭).

2. **Android 8.0 - Oreo** 패키지를 설치합니다. Android SDK 에뮬레이터를 사용하는 경우 필요한 **x86** 시스템 이미지를 포함해야 합니다.

    [![Android SDK 관리자에서 Android 8.0 패키지 선택](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. **Android SDK Tools 26.0.2** 이상, **Android SDK Platform-Tools 26.0.0** 이상 및 **Android SDK Build-Tools 26.0.0**(이상)을 설치합니다.

    [![Android SDK 관리자에서 Android SDK Tools 26 선택](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

1. SDK 관리자를 시작합니다(Mac용 Visual Studio에서 **도구 -> SDK 관리자**를 클릭).

2. **Android 8.0 - Oreo** SDK 패키지를 설치합니다. Android SDK 에뮬레이터를 사용하는 경우 필요한 **x86** 시스템 이미지를 포함해야 합니다.

    [![SDK 관리자에서 Android 8.0 패키지 선택](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. **Android SDK Tools 26.0.2** 이상, **Android SDK Platform-Tools 26.0.0** 이상 및 **Android SDK Build-Tools 26.0.0**(이상)을 설치합니다.

    [![SDK 관리자에서 Android SDK Tools 26 선택](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----

### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android 프로젝트 시작

새 Xamarin.Android 프로젝트를 만듭니다. Xamarin을 사용한 Android 개발을 처음 접하는 경우 [Hello, Android](~/android/get-started/hello-android/index.md)를 참조하여 Xamarin.Android 프로젝트를 만드는 방법에 대해 알아보세요.

Android 프로젝트를 만들 때는 Android 8.0 이상을 대상으로 버전 설정을 구성해야 합니다. 예를 들어 Android 8.0용 프로젝트를 대상으로 하려면 프로젝트의 대상 Android API 수준을 **Android 8.0(API 26)** 으로 구성해야 합니다. 또한 대상 프레임워크 수준을 API 26 이상으로 설정하는 것이 좋습니다. Android API 레벨을 구성하는 방법에 대한 자세한 내용은 [Android API 레벨 이해](~/android/app-fundamentals/android-api-levels.md)를 참조하세요.

### <a name="configure-an-emulator-or-device"></a>에뮬레이터 또는 디바이스 구성

Android SDK Tools 26.0 이상을 설치한 후 기본 Google GUI 기반 AVD 관리자를 시작하려고 하면 다음과 같이 명령줄 AVD 관리자 도구 **avdmanager**를 대신 사용하도록 지시하는 오류 대화 상자가 표시될 수 있습니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Android Emulator 관리자 경고 대화 상자](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

![Android Emulator 관리자 경고 대화 상자](oreo-images/mac/03-avd-warning.png)

-----

Google에서 API 26.0 이상을 지원하는 독립 실행형 GUI AVD 관리자를 더 이상 제공하지 않기 때문에 이 메시지가 표시됩니다. Android 8.0 Oreo의 경우 Xamarin Android Emulator Manager 또는 명령줄 `avdmanager` 도구를 사용하여 Android Oreo용 가상 디바이스를 만들어야 합니다.

Android 디바이스 관리자를 사용하여 가상 디바이스를 만들고 관리하려면 [Android 디바이스 관리자를 사용하여 가상 디바이스 관리](~/android/get-started/installation/android-emulator/device-manager.md)를 참조하세요.
Android 디바이스 관리자 없이 가상 디바이스를 만들려면 다음 섹션의 단계를 수행합니다.

#### <a name="creating-virtual-devices-using-avdmanager"></a>avdmanager를 사용하여 가상 디바이스 만들기

**avdmanager**를 사용하여 새 가상 디바이스를 만들려면 다음 단계를 수행합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. 명령 프롬프트 창을 열고 `JAVA_HOME`을 컴퓨터의 Java SDK 위치로 설정합니다. 일반적인 Xamarin 설치의 경우 다음 명령을 사용할 수 있습니다.

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2. Android SDK `bin` 폴더의 위치를 `PATH`에 추가합니다.
    일반적인 Xamarin 설치의 경우 다음 명령을 사용할 수 있습니다.

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3. 이 명령 프롬프트 창을 닫고 새 명령 프롬프트 창을 엽니다. [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) 명령을 사용하여 새 가상 디바이스를 만듭니다. 예를 들어 API 수준 26용 x86 시스템 이미지를 사용하여 **AVD-Oreo-8.0**이라는 AVD를 만들려면 다음 명령을 사용합니다.

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4. **사용자 지정 하드웨어 프로필을 만드시겠습니까? [아니요]** 라는 메시지가 표시되면 **아니요**를 입력하고 기본 하드웨어 프로필을 사용할 수 있습니다. **예**로 답한 경우 **avdmanager**에 하드웨어 프로필을 사용자 지정하기 위한 질문 목록이 표시됩니다.

**avdmanager**를 사용하여 가상 디바이스를 만들면 해당 디바이스가 디바이스 풀 다운 메뉴에 포함됩니다.

[![디바이스 풀 다운 메뉴에 추가된 새 AVD](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

1. **터미널** 창을 열고 Mac에서 Android SDK Tools 디렉터리의 위치로 변경합니다. 일반적인 Xamarin 설치의 경우 다음 명령을 사용할 수 있습니다.

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2. [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) 명령을 사용하여 새 가상 디바이스를 만듭니다. 예를 들어 API 수준 26용 x86 시스템 이미지를 사용하여 **AVD-Oreo-8.0**이라는 AVD를 만들려면 다음 명령을 사용합니다.

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3. **사용자 지정 하드웨어 프로필을 만드시겠습니까? [아니요]** 라는 메시지가 표시되면 **아니요**를 입력하고 기본 하드웨어 프로필을 사용할 수 있습니다. **예**로 답한 경우 **avdmanager**에 하드웨어 프로필을 사용자 지정하기 위한 질문 목록이 표시됩니다.

**avdmanager**를 사용하여 가상 디바이스를 만들면 해당 디바이스가 디바이스 풀 다운 메뉴에 포함됩니다.

[![디바이스 풀 다운 메뉴에 추가된 새 AVD](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

테스트 및 디버깅을 위해 Android Emulator를 구성하는 방법에 대한 자세한 내용은 [Android Emulator에서 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)을 참조하세요.

Nexus 또는 Pixel 같은 물리적 디바이스를 사용하는 경우 자동 OTA 업데이트를 통해 디바이스를 업데이트할 수도 있고, 시스템 이미지를 다운로드하여 디바이스를 직접 플래시할 수도 있습니다. Android Oreo 디바이스를 수동으로 업데이트하는 방법에 대한 자세한 내용은 [Nexus 및 Pixel 디바이스용 팩터리 이미지](https://developers.google.com/android/images)를 참조하세요.

## <a name="new-features"></a>새 기능

Android Oreo에는 알림 채널, 알림 배지, XML 사용자 지정 글꼴, 다운로드 가능한 글꼴, 자동 채우기, PIP 등 다양한 새 기능이 도입되었습니다. 다음 섹션에서는 이러한 기능을 중점적으로 설명하고 앱에서 이러한 기능을 사용하는 데 도움이 되는 링크를 제공합니다.

### <a name="notification-channels"></a>알림 채널

*알림 채널*은 알림에 대한 앱 정의 범주입니다.
전송해야 하는 각 알림 유형에 대한 알림 채널을 만들 수 있으며, 앱 사용자가 선택한 항목을 반영하는 알림 채널을 만들 수 있습니다. 새 알림 채널 기능을 사용하면 사용자에게 다양한 종류의 알림을 세분화하여 제어하는 기능을 제공할 수 있습니다. 예를 들어 메시징 앱을 구현하는 경우 사용자가 만든 각 대화 그룹에 대해 별도의 알림 채널을 만들 수 있습니다.

[알림 채널](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan)은 알림 채널을 만들고 이를 사용하여 로컬 알림을 게시하는 방법을 설명합니다. 실제 코드 예제는 [NotificationChannels](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-notificationchannels) 샘플을 참조하세요. 이 샘플 앱은 두 채널을 관리하고 추가 알림 옵션을 설정합니다.

### <a name="notification-badges"></a>알림 배지

알림 배지는 다음 스크린샷에 표시된 것처럼 앱 아이콘 위에 표시되는 작은 점입니다.

[![앱 아이콘 위 알림 배지 예제](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

이러한 점은 해당 앱 아이콘에 연결된 앱에서 하나 이상의 알림 채널에 새 알림이 있음을 나타내며, 이는 사용자가 아직 해제하거나 처리하지 않은 알림입니다. 사용자는 아이콘을 길게 눌러 경고 배지와 연결된 알림을 한눈에 보고 표시되는 길게 누름 메뉴에서 알림을 해제하거나 작업을 수행할 수 있습니다.

알림 배지에 대한 자세한 내용은 Android 개발자 [알림 배지](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) 항목을 참조하세요.

### <a name="custom-fonts-in-xml"></a>XML 사용자 지정 글꼴

Android Oreo에서는 *XML 글꼴*가 도입되어 사용자 지정 글꼴을 리소스로 통합할 수 있습니다. OpenType( **.otf**) 및 TrueType ( **.ttf**) 글꼴 형식이 지원됩니다. 글꼴을 리소스로 추가하려면 다음을 수행합니다.

1. **Resources/font** 폴더를 만듭니다.

2. 글꼴 파일(예 **: .ttf** 및 **.bf** 파일)을 **Resources/font**로 복사합니다. 

3. 필요한 경우 Android 파일 명명 규칙을 준수하도록 각 글꼴 파일의 이름을 바꿉니다(파일 이름에 소문자 *a-z*, *0-9* 및 밑줄만 사용). 예를 들어 글꼴 파일 `Pacifico-Regular.ttf`을 `pacifico.ttf` 같은 이름으로 바꿀 수 있습니다.

4. 레이아웃 XML에서 새로운 `android:fontFamily` 특성을 사용하여 사용자 지정 글꼴을 적용합니다. 예를 들어 다음 `TextView` 선언에서는 추가된 **pacifico.ttf** 글꼴 리소스를 사용합니다.

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

또한 여러 글꼴과 스타일 및 가중치 세부 정보를 설명하는 글꼴 패밀리 XML 파일을 만들 수 있습니다. 자세한 내용은 Android 개발자 [XML 글꼴](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) 항목을 참조하세요.

### <a name="downloadable-fonts"></a>다운로드 가능한 글꼴

Android Oreo부터 앱은 글꼴을 APK에 번들링하지 않고 공급자에게 요청할 수 있습니다. 필요한 경우에만 글꼴이 네트워크에서 다운로드됩니다. 이 기능은 APK 크기를 줄이고 휴대폰 메모리와 셀룰러 데이터 사용량을 절약합니다. Android 지원 라이브러리 26 패키지를 설치하여 Android API 버전 14 이상에서 이 기능을 사용할 수도 있습니다.

앱에 글꼴이 필요한 경우 다운로드할 글꼴을 지정하여 `FontsRequest` 개체를 만든 다음 `FontsContract` 메서드에 전달하여 글꼴을 다운로드합니다. 다음 단계에서는 글꼴 다운로드 프로세스에 대해 자세히 설명합니다.

1. [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) 개체를 인스턴스화합니다. 

2. [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html)을 서브클래스 및 인스턴스화합니다.

3. 글꼴 요청 완료를 처리하는 데 사용되는 [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) 메서드를 구현합니다.

4. 글꼴 요청 과정에서 발생하는 모든 오류를 앱에 알리는 데 사용되는 [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) 메서드를 구현합니다.

5. [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) 메서드를 호출하여 글꼴 공급자에서 글꼴을 검색합니다. 

`RequestFonts` 메서드를 호출하면 먼저 (`RequestFont`에 대한 이전 호출에서) 글꼴이 로컬로 캐시되었는지 확인합니다. 캐시되지 않은 경우 글꼴 공급자를 호출하고, 글꼴을 비동기적으로 검색한 다음 `OnTypeFaceRetrieved` 메서드를 호출하여 결과를 다시 앱에 전달합니다.

[Downloadable Fonts](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-downloadablefonts) 샘플에서는 Android Oreo에 도입된 다운로드 가능한 글꼴 기능을 사용하는 방법을 보여 줍니다. 

글꼴을 다운로드하는 방법에 대한 자세한 내용은 Android 개발자 [다운로드 가능한 글꼴](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) 항목을 참조하세요.

### <a name="autofill"></a>자동 채우기

Android Oreo의 새로운 _자동 채우기_ 프레임워크를 사용하면 사용자가 로그인, 계정 만들기 및 신용 카드 거래와 같은 반복적인 작업을 보다 쉽게 처리할 수 있습니다. 사용자는 정보를 다시 입력하는 데 걸리는 시간을 단축할 수 있습니다(입력 오류가 발생할 수 있음). 자동 채우기 프레임워크를 사용하여 앱을 사용할 수 있으려면 시스템 설정에서 자동 채우기 서비스를 사용하도록 설정해야 합니다(사용자가 자동 채우기를 사용하거나 사용하지 않도록 설정할 수 있음).

[AutofillFramework](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-autofillframework) 샘플에서는 자동 채우기 프레임워크를 사용하는 방법을 보여 줍니다. 여기에는 자동으로 채워야 하는 보기가 있는 클라이언트 작업의 구현과 클라이언트 작업에 자동 채우기 데이터를 제공할 수 있는 서비스의 구현이 포함됩니다.

새로운 자동 채우기 기능 및 자동 채우기를 위해 앱을 최적화하는 방법에 대한 자세한 내용은 Android 개발자 [자동 채우기 프레임워크](https://developer.android.com/guide/topics/text/autofill.html) 항목을 참조하세요.

### <a name="picture-in-picture-pip"></a>PIP(Picture in Picture)

Android Oreo를 사용하면 다른 작업의 화면을 겹치게 하는 작업을 PIP(Picture in Picture) 모드로 시작할 수 있습니다. 이 기능은 비디오 재생을 위한 것입니다.

앱의 적업에서 PIP 모드를 사용할 수 있도록 지정하려면 Android 매니페스트에서 다음 플래그를 true로 설정합니다.

```xml
android:supportsPictureInPicture
```

PIP 모드에 있을 때 작업이 동작하는 방식을 지정하려면 새로운 [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) 개체를 사용합니다. `PictureInPictureParams`는 PIP 모드에서 작업을 초기화하고 업데이트하는 데 사용하는 매개 변수 집합을 나타냅니다(예: 작업의 기본 가로 세로 비율). Android Oreo에서 다음 새 PIP 메서드가 `Activity`에 추가되었습니다.

- [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; 작업을 PIP 모드로 전환합니다. 작업은 화면의 모서리에 배치되고 나머지 화면은 화면에 있던 이전 작업으로 채워집니다.

- [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; 작업의 PIP 구성 설정을 업데이트합니다(예: 가로 세로 비율 변경).

[PictureInPicture](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-pictureinpicture) 샘플에서는 Oreo에 도입된 휴대 디바이스용 PiP(Picture in Picture) 모드 기본 사용법을 보여 줍니다. 이 샘플은 디스플레이 모드 또는 다른 작업 간을 전환하는 동안 중단 없이 계속 비디오를 재생합니다.

### <a name="other-features"></a>기타 기능

Android Oreo에는 이모지 지원 라이브러리, 위치 API, 백그라운드 제한, 앱의 폭넓은 색상 범위, 새로운 오디오 코덱, WebView 기능 향상, 향상된 키보드 탐색 지원 및 고성능 저지연 오디오를 위한 새 AAudio(pro audio) API 등 다른 여러 가지 새로운 기능이 포함되어 있습니다. 이러한 기능에 대한 자세한 내용은 Android 개발자 [Android Oreo 기능 및 API](https://developer.android.com/about/versions/oreo/android-8.0.html) 항목을 참조하세요.

## <a name="behavior-changes"></a>동작 변경 내용

Android Oreo에는 기존 앱의 기능에 영향을 줄 수 있는 다양한 시스템 및 API 동작 변경 내용이 포함되어 있습니다. 이러한 변경 내용은 다음과 같습니다.

### <a name="background-execution-limits"></a>백그라운드 실행 제한

사용자 환경을 개선하기 위해 Android Oreo는 앱이 백그라운드에서 실행되는 동안 수행할 수 있는 작업에 대한 제한을 적용합니다. 예를 들어 사용자가 비디오를 시청하거나 게임을 플레이하는 경우 백그라운드에서 실행 중인 앱이 포그라운드에서 실행되는 비디오 집약적 앱의 성능을 저하시킬 수 있습니다. 따라서 Android Oreo는 사용자와 직접 상호 작용하지 않는 앱에 다음과 같은 제한을 적용합니다.

1. **백그라운드 서비스 제한** &ndash; 앱은 백그라운드에서 실행되는 경우 몇 분 동안 서비스를 만들고 사용하도록 허용됩니다. 이 기간이 끝나면 Android는 앱의 백그라운드 서비스를 중지하고 _유휴_로 처리합니다.

2. **브로드캐스트 제한** &ndash; Android 7.0(API 25)에서는 앱이 수신을 등록하는 브로드캐스트에 대한 제한을 적용했습니다. Android Oreo는 이 제한을 더 엄격하게 적용합니다. 예를 들어 Android Oreo 앱은 더 이상 매니페스트에 암시적 브로드캐스트를 위한 브로드캐스트 수신기를 등록할 수 없습니다.

새 백그라운드 실행 제한에 대한 자세한 내용은 Android 개발자 [백그라운드 실행 제한](https://developer.android.com/about/versions/oreo/background.html) 항목을 참조하세요.

### <a name="breaking-changes"></a>주요 변경 사항

Android Oreo 이상을 대상으로 하는 앱은 해당하는 경우 다음 변경을 지원하도록 앱을 수정해야 합니다.

- Android Oreo에서는 개별 알림의 우선 순위를 설정하는 기능을 더 이상 사용하지 않습니다. 대신 알림 채널을 만들 때 권장 중요도 수준을 설정합니다. 알림 채널에 할당하는 중요도 수준은 게시하는 모든 알림 메시지에 적용됩니다.

- Android Oreo를 대상으로 하는 앱은 백그라운드에서 시작된 서비스에 적용되는 새로운 제한으로 인해 `PendingIntent.GetService()`가 작동하지 않습니다. Android Oreo를 대상으로 하는 경우에는 [PendingIntent.GetBroadcast](xref:Android.App.PendingIntent.GetBroadcast*)를 대신 사용해야 합니다.  

## <a name="sample-code"></a>샘플 코드

Android Oreo 기능을 활용하는 방법을 보여 주는 여러 Xamarin.Android 샘플이 있습니다.

- [NotificationsChannels](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-notificationchannels)는 Android Oreo에 도입된 새 알림 채널 시스템을 사용하는 방법을 보여 줍니다. 이 샘플에서는 두 개의 알림 채널을 관리합니다. 하나는 기본 중요도이고 다른 하나는 높은 중요도입니다.

- [PictureInPicture](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-pictureinpicture)는 Oreo에 도입된 휴대 디바이스용 PiP(Picture in Picture) 모드의 기본 사용법을 보여 줍니다. 이 샘플은 디스플레이 모드 또는 다른 작업 간을 전환하는 동안 중단 없이 계속 비디오를 재생합니다.

- [AutofillFramework](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-autofillframework)는 자동 채우기 프레임워크의 사용법을 보여 줍니다. 여기에는 자동으로 채워야 하는 보기가 있는 클라이언트 작업의 구현과 클라이언트 작업에 자동 채우기 데이터를 제공할 수 있는 서비스의 구현이 포함됩니다.

- [Downloadable Fonts](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-downloadablefonts)는 앞에서 설명한 다운로드 가능한 글꼴 기능을 사용하는 방법에 대한 예제를 제공합니다.

- [EmojiCompat](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-emojicompat)는 EmojiCompat 지원 라이브러리의 사용법을 보여 줍니다. 이 라이브러리를 사용하여 앱이 누락된 이모지 문자를 "tofu" 문자로 표시하는 것을 방지할 수 있습니다.

- [Location Updates Pending Intent](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-androidplaylocation-locupdpendintent)는 `PendingIntent`를 사용하여 디바이스 위치에 대한 업데이트를 가져오는 위치 API의 사용법을 보여 줍니다.

- [Location Updates Foreground Service](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-androidplaylocation-locupdfgservice)는 바인딩 및 시작된 포그라운드 서비스를 사용하여 디바이스 위치에 대한 업데이트를 가져오는 위치 API의 사용법을 보여 줍니다.

## <a name="video"></a>비디오

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**C#를 사용하여 Android 8.0 Oreo 개발**

## <a name="summary"></a>요약

이 문서에서는 Android Oreo를 소개하고, Android Oreo에서 Xamarin.Android 개발을 위한 최신 도구 및 패키지를 설치하고 구성하는 방법을 설명했습니다. Android Oreo에서 사용할 수 있는 주요 기능에 대한 개요를 제공하고 몇 가지 새로운 기능에 대한 소스 코드 예제 링크를 제공했습니다. 여기에는 Android Oreo용 앱을 만드는 과정을 시작하는 데 도움이 되는 API 설명서 및 Android 개발자 항목에 대한 링크가 포함되어 있습니다. 기존 앱에 영향을 줄 수 있는 가장 중요한 Android Oreo 동작 변경 내용도 강조 표시되어 있습니다.

## <a name="related-links"></a>관련 링크

- [Android 8.0 Oreo](https://developer.android.com/index.html)
