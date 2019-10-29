---
title: 버전 정보 및 로그는 어디에서 확인할 수 있나요?
description: 이 문서에서는 Xamarin 버전 정보 및 로그를 찾는 위치를 설명 합니다. 이 정보는 문제를 진단 하거나, 버그를 제출 하거나, 지원을 받을 때 유용 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 68de58f499788d803aa0af6c68f20e2265b1d6b5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013168"
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>버전 정보 및 로그는 어디에서 확인할 수 있나요?

## <a name="outline"></a>윤곽선

- [버전 정보](#version-information)
  - Windows 버전 정보
  - Mac 버전 정보
  - Android SDK Tools, 플랫폼 도구, 빌드-도구
- [IDE 및 설치 관리자 로그](#ide-and-installer-logs)
  - [Windows 로그](#windows-logs)
    - Xamarin Studio
    - Visual Studio용 Xamarin
    - Xamarin Universal 설치 관리자
    - 개별 `.msi` 설치 관리자, 자세한 정보 로그
    - Visual Studio 시작, 자세한 정보 로그
  - [Mac 로그](#mac-logs)
    - 빌드 호스트
  - Visual Studio for Mac
    - Xamarin Studio
    - Xamarin 설치 관리자
- [자세한 정보 표시 빌드 출력](#verbose-build-output-logs)
- [Xamarin Android 및 Xamarin.ios 앱에 대 한 디버그 로그](#debug-logs-for-xamarin-apps)
  - Android `adb` logcat 로그
  - iOS 시뮬레이터 로그 (Mac)
  - iOS 장치 로그 (Mac의 경우)

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />버전 정보

일반적으로 **정보 복사** 단추에서 모든 정보를 다시 전송 하는 것이 가장 좋습니다. 그렇지 않으면 추가 정보를 요청 해야 하는 경우가 많습니다. 예를 들어, 운영 체제 버전, Xcode 버전, 설치 된 Android API 수준 및 .NET 버전은 문제 해결에 도움이 될 수 있습니다.

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a>Windows 버전 정보 <a id="windows-version-information" name="windows-version-information" />

#### <a name="xamarin-studio"></a>Xamarin Studio

**정보를 > 정보를 표시 하는 방법에 대 한 도움말 > > 복사 정보 [단추]**

#### <a name="visual-studio"></a>Visual Studio

**Microsoft Visual Studio > 정보 복사에 대 한 도움말 > [button]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Mac 버전 정보

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Visual studio > Visual Studio > 정보 > 복사 정보 표시 [단추]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK Tools, 플랫폼 도구, 빌드-도구

Android SDK 관리자를 열고 최상위 **도구** 섹션의 스크린샷을 찍습니다.

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Android SDK Manager > 도구를 엽니다.**

#### <a name="visual-studio"></a>Visual Studio

**Android > 도구 > Android SDK 관리자 열기 ...**

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />IDE 및 설치 관리자 로그

각 로그 위치에 대해 전체 로그 폴더를 압축 하 여 연결 해야 합니다.

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a>Windows 로그 <a id="windows-logs" name="windows-logs" />

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a>Xamarin에 대 한 <a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Visual Studio Tools

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[Visual Studio 설치 로그를 가져오는 방법](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> Xamarin "범용" 설치 관리자

`%LOCALAPPDATA%\Xamarin\Universal`

이러한 로그는 `XamarinInstaller.exe` 설치 관리자의 로그입니다.

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a>개별 `.msi` 설치 관리자, 자세한 로그 <a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

참조: [명령줄 옵션](https://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a>Visual Studio 시작, 자세한 로그 <a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

참조: [/log (devenv.exe)](https://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Mac 로그

Finder에서 **이동 > 폴더로 이동** 메뉴 항목을 선택 하 고이 경로를 복사 하 여 대화 상자에 붙여 넣을 수 있습니다.

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Mac용 Visual Studio

`~/Library/Logs/VisualStudio/7.0` (이 숫자는 사용 중인 버전에 따라 달라질 수 있음)

이 폴더는 "도움말-> 로그 디렉터리 열기"를 통해 열 수도 있습니다.

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (이 숫자는 사용 중인 버전에 따라 달라질 수 있음)

이 폴더는 "도움말-> 로그 디렉터리 열기"를 통해 열 수도 있습니다.

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />Xamarin "범용" 설치 관리자

`~/Library/Logs/XamarinInstaller/Universal`

이러한 로그는 `XamarinInstaller.dmg` 설치 관리자의 로그입니다.

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a>Xamarin 빌드 호스트 <a id="mac-build-host" name="mac-build-host" />

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />자세한 정보 표시 빌드 출력

1. [진단 MSBuild 출력](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)을 사용 하도록 설정 합니다.

2. IOS 응용 프로그램의 경우 **프로젝트 속성 > IOS 빌드 > 일반 (탭 >)** 에 `-v -v -v -v` 추가 하 고 추가 mtouch 인수 > 추가 옵션을 추가 하 여 **자세한 정보 표시 mtouch 출력** 을 사용 하도록 설정할 수도 있습니다.

3. 프로젝트를 정리 하 고 다시 빌드합니다.

4. IDE의 빌드 출력을 복사 하 여 텍스트 파일에 붙여 넣습니다.
     - Visual Studio (Windows): 출력 **> 표시 > 출력: 빌드**
     - Mac용 Visual Studio: **빌드 출력 > > 패드 > 오류 보기 (탭)**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a>Xamarin Android 및 Xamarin.ios 앱에 대 한 디버그 로그 <a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**응용 프로그램 출력 > > 패드 보기**

이 메뉴 항목은 앱이 시작 된 후에만 표시 됩니다.

### <a name="visual-studio"></a>Visual Studio

**출력 > 표시 > 디버그: 디버그**

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpsdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a>Android [`adb`](https://developer.android.com/tools/help/adb.html) logcat 로그 <a id="adb-logcat" name="adb-logcat" />

`adb` 명령을 실행 한 후 바탕 화면에서 **android_logcat** 파일을 다시 연결 합니다. 이 지침에서는 장치가 하나만 연결 되어 있다고 가정 합니다.

[Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md) 페이지도 참조 하세요.

#### <a name="visual-studio"></a>Visual Studio

1. **Android > 도구 > Android Adb 명령 프롬프트 시작**
2. 로그 정리: `adb logcat -c`
3. 문제를 재현 합니다.
4. 로그를 출력 합니다. `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. **도구 > Android SDK 명령 프롬프트 열기**
2. 로그 정리: `adb logcat -c`
3. 문제를 재현 합니다.
4. 로그를 출력 합니다. `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />iOS 시뮬레이터 로그 (Mac)

- 시스템 로그에 액세스 하려면 iOS 시뮬레이터 앱에서 **디버그 > 시스템 로그 열기** ...를 선택 합니다.

- 시뮬레이터에서 충돌 보고서를 보려면 Console. 앱을 열고 `~/Library/Logs > DiagnosticReports`로 이동 합니다.

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a>Mac에서 iOS 장치 로그 <a id="ios-device-logs" name="ios-device-logs" />

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**IOS 장치 로그 > > 패드 보기**

#### <a name="xcode"></a>Xcode

**Windows > 장치 > $ {장치 이름}**

충돌 보고서는 **장치 로그 보기** 단추 아래에서 사용할 수 있습니다. 장치의 시스템 로그는 창 아래쪽에 있는 노출 화살표 아래에 나타납니다.

#### <a name="xcode-5"></a>Xcode 5

**창 > 구성 도우미 > 장치 (탭) > $ {장치 이름}**
