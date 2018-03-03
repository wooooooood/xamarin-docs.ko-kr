---
title: "버전 정보 및 로그 내 어디서 찾을 수 있습니까?"
ms.topic: article
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2d6ab8b939b7a6f8c7e66985a8c4238064ee1543
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>버전 정보 및 로그 내 어디서 찾을 수 있습니까?

## <a name="outline"></a>윤곽선

- [버전 정보](#version-information)
    - Windows 버전 정보
    - Mac 버전 정보
    - Android SDK 도구, 플랫폼 도구 빌드-도구
- [IDE 및 설치 관리자 로그](#ide-and-installer-logs)
    - [Windows 로그](#windows-logs)
        - Xamarin Studio
        - Visual Studio용 Xamarin
        - Xamarin Universal installer
        - 개별 `.msi` 설치 관리자, 자세한 정보 로그
        - 자세한 로그 visual Studio 시작
    - [Mac 로그](#mac-logs)
        - 호스트 구축
    - Visual Studio for Mac
        - Xamarin Studio
        - Xamarin 설치 프로그램
- [자세한 정보 빌드 출력](#verbose-build-output-logs)
- [Xamarin.Android 및 Xamarin.iOS 앱에 대 한 로그를 디버그 합니다.](#debug-logs-for-xamarin-apps)
    - Android `adb` logcat 로그
    - iOS 시뮬레이터 로그온 (Mac)
    - iOS 장치 로그온 (Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />버전 정보

일반적으로 가장 효율적인 전송의 정보를 모두 복원는 **복사본 정보** 단추입니다. 그렇지 않은 경우 추가 정보를 요청 필요한 경우가 많습니다 있습니다. 예를 들어 운영 체제 버전, Xcode 버전 Android API 수준을 설치 하 고.NET 버전 모두 때 중요할 수 있습니다 문제를 해결할 수 있도록 지원 합니다.

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Windows 버전 정보

#### <a name="xamarin-studio"></a>Xamarin Studio

**도움말 >에 대 한 > 자세한 정보 표시 > 복사본 정보 [button]**

#### <a name="visual-studio"></a>Visual Studio

**도움말 >에 대 한 Microsoft Visual Studio > 정보 복사 [button]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Mac 버전 정보

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Visual Studio > Visual Studio에 대 한 > 자세한 정보 표시 > 복사본 정보 [button]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK 도구, 플랫폼 도구 빌드-도구

Android SDK Manager를 열고 맨 위에의 스크린샷의 찍을 **도구** 섹션.

![](https://kb.xamarin.com/customer/portal/attachments/337323 "Android SDK Manager의 스크린 샷 > 도구 폴더")

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**도구 > Android SDK Manager를 엽니다.**

#### <a name="visual-studio"></a>Visual Studio

SDK Manager 도구 모음 아이콘을 선택 합니다.

![](https://kb.xamarin.com/customer/portal/attachments/420238 "Visual Studio에서 시작 하는 Android SDK Manager 도구 모음 아이콘")

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />IDE 및 설치 관리자 로그

각 로그 위치에 대해 사용할 전체 로그 폴더를 연결 하 고 압축 해야 합니다.

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />Windows 로그

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Xamarin 용 도구 visual Studio

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[Visual Studio 설치 로그를 가져오는 방법](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> Xamarin "범용" 설치 관리자

`%LOCALAPPDATA%\Xamarin\Universal`

이러한 로그에서는 `XamarinInstaller.exe` 설치 관리자입니다.

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />개별 `.msi` 설치 관리자, 자세한 정보 로그

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

참조: [명령줄 옵션](http://msdn.microsoft.com/en-us/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />자세한 로그 visual Studio 시작

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

참조: [/Log (devenv.exe)](http://msdn.microsoft.com/en-us/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Mac 로그

선택할 수 있습니다는 **이동 > 폴더로 이동** 메뉴 항목 Finder에 복사 하 고 이러한 경로 있는 대화 상자에 붙여넣습니다.

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Mac용 Visual Studio

`~/Library/Logs/VisualStudio/7.0` (이 숫자는 사용 중인 버전에 따라 변경 될 수 있습니다.)

이 폴더 열 수도 있습니다를 통해 "개방형 로그 디렉터리-> Help"입니다.

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (이 숫자는 사용 중인 버전에 따라 변경 될 수 있습니다.)

이 폴더 열 수도 있습니다를 통해 "개방형 로그 디렉터리-> Help"입니다.

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />Xamarin "범용" 설치 관리자

`~/Library/Logs/XamarinInstaller/Universal`

이러한 로그에서는 `XamarinInstaller.dmg` 설치 관리자입니다.

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Xamarin 빌드 호스트

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />자세한 정보 빌드 출력

1.  사용 하도록 설정 [진단 MSBuild 출력](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)합니다.

2.  IOS 앱도 사용 하도록 설정 **verbose mtouch 출력** 추가 하 여 `-v -v -v -v` 아래 **프로젝트 속성 > iOS 빌드 > [tab] 일반 > 추가 옵션 > 추가 mtouch 인수**합니다.

    **Visual Studio (Windows)**

    [![](https://kb.xamarin.com/customer/portal/attachments/337319 "추가 mtouch 인수 입력된 필드에 4 개의 대시 v를 입력 합니다.")](https://kb.xamarin.com/customer/portal/attachments/337319)

    **Visual Studio for Mac**

    [![](https://kb.xamarin.com/customer/portal/attachments/337316 "추가 mtouch 인수 입력된 필드에 4 개의 대시 v를 입력 합니다.")](https://kb.xamarin.com/customer/portal/attachments/337316)

3.  정리 하 고 프로젝트를 다시 빌드하십시오.

4.  복사한 IDE에서 빌드 출력 텍스트 파일에 붙여 넣습니다.
     - Visual Studio (Windows): **보기 > 출력 >에서 출력 보기: 빌드**
     - Mac 용 visual Studio: **보기 > 패드 > 오류 > 빌드 출력 [tab]**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Xamarin.Android 및 Xamarin.iOS 앱에 대 한 로그를 디버그 합니다.

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**보기 > 패드 > 응용 프로그램 출력**

![](https://kb.xamarin.com/customer/portal/attachments/337290 "Mac 용 Visual Studio에서 응용 프로그램 출력 패드 아이콘")


(Note: 응용 프로그램 시작 된 후이 메뉴 항목만 표시 됩니다.)

### <a name="visual-studio"></a>Visual Studio

**보기 > 출력 >에서 출력 보기: 디버깅**

![](https://kb.xamarin.com/customer/portal/attachments/337292 "Visual Studio에서 디버그 옵션을 보여 주는 출력 채움")

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />Android [ `adb` ](http://developer.android.com/tools/help/adb.html) logcat 로그

실행 된 후의 `adb` 명령을, 다시 연결 하는 **android_logcat.txt** 바탕 화면에서 파일. 이러한 지침 하나만 장치가 연결 되어 있다고 가정 합니다.

참고 항목에서 [Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md) 페이지.

#### <a name="visual-studio"></a>Visual Studio

1.  **도구 > Android > Android Adb 명령 프롬프트를 시작 합니다.**
2.  로그를 정리 합니다. `adb logcat -c`
3.  문제를 재현 합니다.
4.  로그를 출력 합니다. `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1.  **도구 > Android SDK 명령 프롬프트 열기**
2.  로그를 정리 합니다. `adb logcat -c`
3.  문제를 재현 합니다.
4.  로그를 출력 합니다. `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />iOS 시뮬레이터 로그온 (Mac)

*   시스템 로그에 액세스 하려면 **디버그 > 시스템 로그 열기...**  iOS 시뮬레이터 앱의 합니다.

    ![](https://kb.xamarin.com/customer/portal/attachments/382617 "디버그 메뉴 열기 시스템 로그 옵션을 보여 주는")

*   시뮬레이터에서 충돌 보고서를 보려면 Console.app 열고 이동 `~/Library/Logs > DiagnosticReports`합니다.

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />iOS 장치 로그온 (Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**보기 > 패드 > iOS 장치 로그**

#### <a name="xcode"></a>Xcode 

**창 > 장치 > ${DeviceName}**

충돌 보고서는 아래에서 사용할 수는 **장치 로그 보기** 단추입니다. 공개 화살표 창의 맨 아래에 표시 되는 장치에 대 한 시스템 로그 <img alt="Disclosure arrow" src="https://kb.xamarin.com/customer/portal/attachments/382618" style="width: 15px; height: 12px;" />합니다.

#### <a name="xcode-5"></a>Xcode 5

**Window > Organizer > Devices [tab] > ${DeviceName}**
