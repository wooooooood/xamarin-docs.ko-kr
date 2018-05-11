---
title: Xamarin 제거
description: 컴퓨터에서 Xamarin 제품 제거
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: d1b88ad97a1cecaadd84226bca61c7f2b262438d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="uninstalling-xamarin"></a>Xamarin 제거

이 가이드에서는 macOS 또는 Windows의 Visual Studio에서 Xamarin을 제거하는 방법을 설명합니다.

유니버설 설치 관리자를 사용하여 Xamarin을 다시 설치해야 하는 경우 항상 컴퓨터를 먼저 다시 부팅하는 것이 좋습니다.

## <a name="uninstalling-xamarin-on-macos"></a>macOS에서 Xamarin 제거

각 제품을 개별적으로 제거하려면 이 가이드의 관련 섹션을 참조하세요. 이 가이드의 모든 내용을 따라, 나열된 제품을 포함하는 전체 Xamarin 도구 집합을 제거할 수 있습니다.

- [Mono](#uninstallmono)
- [Xamarin.Android](#uninstallandroid)
- [Xamarin.iOS](#uninstallios)
- [Xamarin.Mac](#uninstallmac)
- [검사기 및 Workbooks](#uninstallworkbooks)
- [Xamarin Profiler](#uninstallprofiler)
- [설치 프로그램](#uninstallinstaller)

> [!TIP]
> macOS 컴퓨터에서 Xamarin을 제거할 때 사용할 [제거 스크립트](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh)를 제공했습니다. 스크립트 사용에 대한 자세한 내용은 이 가이드의 [스크립트 제거 사용](#uninstallscript) 섹션을 참조하세요.

### <a name="uninstalling-visual-studio-for-mac"></a>Mac용 Visual Studio 제거

Mac에서 Xamarin을 제거하는 첫 번째 단계는 **/Applications** 디렉터리에서 **Visual Studio.app**을 찾아 **휴지통**으로 끌어놓는 것입니다. 또는 다음 이미지의 그림과 같이 마우스 오른쪽 단추를 클릭하고 **Move to Trash**(휴지통으로 이동)를 선택합니다.

![Visual Studio 응용 프로그램을 휴지통으로 이동](uninstalling-xamarin-images/uninstall-image1.png)

이 앱 번들을 삭제하면 Mac용 Visual Studio가 제거되지만 Xamarin과 관련된 다른 파일은 파일 시스템에 남을 수 있습니다.

Mac용 Visual Studio의 모든 흔적을 제거하려면 터미널에서 다음 명령을 실행합니다.

```bash
sudo rm -rf "/Applications/Visual Studio.app"
rm -rf ~/Library/Caches/VisualStudio
rm -rf ~/Library/Preferences/VisualStudio
rm -rf ~/Library/Preferences/Visual\ Studio
rm -rf ~/Library/Logs/VisualStudio
rm -rf ~/Library/VisualStudio
rm -rf ~/Library/Preferences/Xamarin/
rm -rf ~/Library/Developer/Xamarin
rm -rf ~/Library/Application\ Support/VisualStudio
rm -rf ~/Library/Application\ Support/VisualStudio/7.0/LocalInstall/Addins/
```

> [!NOTE]
> Mac용 Visual Studio를 제거하는 방법에 대한 자세한 내용은 docs.microsoft.com에서 [제거](https://docs.microsoft.com/visualstudio/mac/uninstall) 가이드를 참조하세요.

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Mono SDK(MDK) 제거

Mono는 .NET Framework의 오픈 소스 구현으로서 Xamarin.iOS, Xamarin.Android 및 Xamarin.Mac 등 모든 Xamarin 제품에서 C#으로 이러한 플랫폼을 개발하는 데 사용됩니다.

> [!WARNING]
> Mono를 사용하는 Xamarin 이외에도 Unity와 같은 응용 프로그램이 있습니다. Mono를 제거하기 전에 Mono에 대한 다른 종속성이 없는지 확인하세요.

Mono 프레임워크를 제거하려면 터미널에서 다음 명령을 실행합니다.

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Xamarin.Android 제거

Xamarin.Android를 제거할 때 제거해야 하는 Android SDK 및 Java SDK처럼 Xamarin.Android를 사용할 때 필요한 여러 항목이 있습니다. 이 섹션에서는 필요한 모든 부분을 제거하는 과정을 안내합니다.

Xamarin.Android를 제거하려면 터미널에서 다음 명령을 실행합니다.

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Android SDK 및 Java SDK 제거

Android SDK는 Android 응용 프로그램 개발에 필요합니다. Android SDK의 모든 부분을 완전히 제거하려면 **~/Library/Developer/Xamarin/** 에서 해당 파일을 찾아 **휴지통**으로 이동합니다.

Java SDK(JDK)는 이미 Mac OS X에 포함되어 있으므로 제거할 필요가 없습니다.

#### <a name="uninstall-android-avd"></a>Android AVD 제거

> [!WARNING]
> Mac용 Visual Studio 이외에 Android Studio와 같은 응용 프로그램에서도 Android AVD 및 추가 Android 구성 요소를 사용합니다.
> 이 디렉터리를 제거하면 Android Studio에서 프로젝트가 중단될 수 있습니다. 

Android AVD 및 기타 Android 구성 요소를 제거하려면 다음 명령을 사용합니다.

```bash
rm -rf ~/.android
```

Android AVD만 제거하려면 다음 명령을 사용합니다.

```bash
rm -rf ~/.android/avd
```

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Xamarin.iOS 제거

Xamarin.iOS를 사용하면 C# 또는 F#으로 iOS 응용 프로그램을 개발할 수 있습니다. 컴퓨터에서 Xamarin.iOS를 제거하려면 다음 단계를 수행합니다.

모든 Xamarin.iOS 파일을 제거하려면 터미널에서 다음 명령을 사용합니다.

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
sudo pkgutil --forget com.xamarin.xamarin.ios.pkg
```

<a name="uninstallmac" />

### <a name="uninstall-xamarinmac"></a>Xamarin.Mac 제거

다음 두 가지 명령으로 Mac에서 제품과 라이선스를 각각 제거하여 Xamarin.Mac을 컴퓨터에서 제거할 수 있습니다.

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks-and-inspector"></a>Workbooks 및 Inspector 제거

Xamarin 검사기 및 Workbooks 버전 1.2.2 이상을 제거하려면 터미널에서 다음 명령을 사용합니다.

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

이전 버전은 [Workbooks](~/tools/workbooks/install.md#uninstall-macos) 제거 가이드를 참조하세요.

<a name="uninstallprofiler" />

### <a name="uninstall-the-xamarin-profiler"></a>Xamarin Profiler 제거

Xamarin Profiler를 제거하려면 터미널에서 다음 명령을 사용합니다.

```bash
sudo rm -rf "/Applications/Xamarin Profiler.app"
```

<a name="uninstallinstaller" />

### <a name="uninstall-the-xamarin-installer"></a>Xamarin 설치 관리자 제거

Xamarin Universal 설치 관리자의 모든 추적을 제거하려면 다음 명령을 사용합니다.

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Caches/VisualStudioInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Logs/VisualStudioInstaller/
rm -rf ~/Library/Preferences/Xamarin/
rm -rf "~/Library/Preferences/Visual Studio/"
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>스크립트 제거 사용

이 제거 스크립트를 사용하면 Mac용 Visual Studio와 관련된 Xamarin 구성 요소를 한꺼번에 제거할 수 있습니다.

이 스크립트는 이 문서에 나와 있는 대부분의 명령을 포함합니다. 다음과 같은 두 가지 항목은 외부 종속성 문제의 소지가 있어 이 스크립트에 포함되지 않았습니다.

- Mono 제거
- Android AVD 제거

스크립트를 실행하려면 다음 단계를 수행하십시오.

1. 스크립트를 마우스 오른쪽 단추로 클릭하고 [다른 이름으로 저장...]을 선택하여 Mac에 파일을 저장합니다.

2.  **터미널**을 열고 스크립트를 다운로드한 위치로 작업 디렉터리를 변경합니다.

        $ cd /location/of/file

3. 스크립트를 실행 가능으로 설정하고 **sudo**로 실행합니다.

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

4. 마지막으로 제거 스크립트를 삭제합니다.

이 시점에서 사용자의 컴퓨터에서 Xamarin은 제거되어야 합니다.

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Windows에서 Xamarin 제거

Xamarin은 다음에서 지원됩니다.

- [Visual Studio 2017](#uninstallvs2017)
- [Visual Studio 2015](#uninstallvs2015)
- [Visual Studio 2013](#uninstallvs2015) [**지원되지 않음**]
- [Xamarin Studio](#uninstallxamarinstudio) [**지원되지 않음**]

<a name="uninstallvs2017" />

### <a name="visual-studio-2017"></a>Visual Studio 2017

설치 관리자 앱을 사용하여 Visual Studio 2017에서 Xamarin을 제거합니다.

1. **시작 메뉴**를 사용하여 **Visual Studio 설치 관리자**를 엽니다.

2. 변경하려는 인스턴스의 **수정** 단추를 누릅니다.

    [![](uninstalling-xamarin-images/vs2017-02-sml.png "수정 단추 누르기")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

3. **워크로드** 탭에서 **.NET을 사용하는 모바일 개발** 옵션의 선택을 취소합니다(**모바일 및 게임** 섹션에서).

    [![](uninstalling-xamarin-images/vs2017-03-sml.png "모바일 개발 워크로드 선택 취소")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

4. 창의 오른쪽 아래에 있는 **수정** 단추를 클릭합니다.

5. 설치 관리자는 선택이 취소된 구성 요소를 제거합니다(설치 관리자가 변경하기 전에 Visual Studio 2017이 닫힘).

    [![](uninstalling-xamarin-images/vs2017-04-sml.png "수정 단추 누르기")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

3단계에서 **개별 구성 요소** 탭을 전환하고 특정 구성 요소의 선택을 취소하여 개별 Xamarin 구성 요소(예: 프로파일러 또는 Workbooks)를 제거할 수 있습니다.

[![](uninstalling-xamarin-images/vs2017-components-sml.png "개별 구성 요소 제거")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Visual Studio 2017을 완전히 제거하려면 **시작** 단추 옆에 있는 3개의 표시줄 메뉴에서 **제거**를 선택합니다.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Visual Studio 완전히 제거")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> 릴리스 및 미리 보기 버전 등 Visual Studio의 두 개(이상) 인스턴스를 나란히(SxS) 설치한 경우 하나의 인스턴스를 제거하면 다음을 비롯한 다른 Visual Studio 인스턴스에서 일부 Xamarin 기능이 제거될 수 있습니다.
>
> - Xamarin Profiler
> - Xamarin Workbooks/검사기
> - Xamarin 원격 iOS 시뮬레이터
> - Apple Bonjour SDK
>
> 특정 조건에서 SxS 인스턴스 중 하나를 제거하면 이러한 기능을 잘못 제거할 수 있습니다. SxS 인스턴스를 제거한 후에 시스템에 남아 있는 Visual Studio 인스턴스에서 Xamarin 플랫폼의 성능이 저하될 수 있습니다.
>
>Visual Studio 설치 관리자에서 **복구** 옵션을 실행하여 해결합니다. 그러면 누락된 구성 요소를 다시 설치합니다.


## <a name="uninstalling-older-and-unsupported-products"></a>이전 버전 및 지원되지 않는 제품 제거

<a name="uninstallvs2015"></a>

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 및 이전 버전

Visual Studio 2015를 완전히 제거하려면 [visualstudio.com에서 Support Answer](https://www.visualstudio.com/vs/support/vs2015/uninstall-visual-studio-2015/)를 사용합니다.

**제어판**을 통해 Windows 컴퓨터에서 Xamarin을 제거할 수 있습니다. 아래 그림과 같이 **프로그램 및 기능** 또는 **프로그램 > 프로그램 제거**로 이동합니다.

 [![](uninstalling-xamarin-images/image3.png "여기 그림과 같이 [프로그램 및 기능] 또는 [프로그램 > 프로그램 제거]로 이동")](uninstalling-xamarin-images/image3.png#lightbox) 

[제어판]에서 표시되는 다음 중 하나를 제거합니다.

- Xamarin
- Windows용 Xamarin
- Xamarin.Android
- Xamarin.iOS
- Visual Studio용 Xamarin

탐색기에서 Xamarin Visual Studio 확장 폴더(프로그램 파일과 프로그램 파일(x86)을 포함한 모든 버전)에서 나머지 파일을 모두 삭제합니다.

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

다음 위치에 있어야 하는 Visual Studio의 MEF 구성 요소 캐시 디렉터리를 삭제합니다.

``` 
%LOCALAPPDATA%\Microsoft\VisualStudio\1*.0\ComponentModelCache
```

Windows가 **Extensions\Xamarin** 또는 **ComponentModelCache** 디렉터리에 대한 오버레이 파일을 저장했는지 확인하려면 **VirtualStore** 디렉터리를 체크 인합니다.

``` 
%LOCALAPPDATA%\VirtualStore
```

레지스트리 편집기(regedit)를 열고 다음 키를 찾습니다.

``` 
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls
```

이 패턴과 일치하는 항목을 찾아 삭제합니다.

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

이 키를 찾습니다.

``` 
HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\1*.0\ExtensionManager\PendingDeletions
```

Xamarin과 관련된 것으로 보이는 모든 항목을 삭제합니다. 예를 들어, `mono` 또는 `xamarin` 용어를 포함하는 모든 항목이 해당됩니다.

관리자 cmd.exe 명령 프롬프트를 연 다음, Visual Studio의 설치된 각 버전에 대해 `devenv /setup` 및 `devenv /updateconfiguration` 명령을 실행합니다. 예를 들어, Visual Studio 2015의 경우:

```cmd
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
```

<a name="uninstallxamarinstudio"></a>

### <a name="uninstall-xamarin-studio-on-windows"></a>Windows에서 Xamarin Studio 제거

**제어판**을 통해 Windows 컴퓨터에서 Xamarin Studio를 제거합니다. **프로그램 및 기능** 또는 **프로그램 > 프로그램 제거**로 이동 

Xamarin Studio를 제거하려면 프로그램 목록에서 **Xamarin Studio 5.x.x**를 찾아 **제거** 단추를 클릭합니다. 

### <a name="uninstall-xamarin-studio-on-mac"></a>Mac에서 Xamarin Studio 제거

Mac에서 Xamarin Studio를 제거하는 첫 번째 단계는 **/Applications** 디렉터리에서 **Xamarin Studio.app**을 찾아 **휴지통**으로 끌어놓는 것입니다. 또는 아래 그림과 같이 마우스 오른쪽 단추를 클릭하고 **Move to Trash**(휴지통으로 이동)를 선택합니다.

 [![](uninstalling-xamarin-images/image1.png "또는 다음 그림과 같이 마우스 오른쪽 단추를 클릭하고 휴지통으로 이동을 선택합니다.")](uninstalling-xamarin-images/image1.png#lightbox)

이 앱 번들을 삭제하면 Xamarin Studio가 제거되지만 Xamarin과 관련된 다른 파일은 파일 시스템에 남아 있습니다.

Xamarin Studio의 모든 흔적을 제거하려면 터미널에서 다음 명령을 실행해야 합니다.

```bash
sudo rm -rf "/Applications/Xamarin Studio.app"
rm -rf ~/Library/Caches/XamarinStudio-*
rm -rf ~/Library/Preferences/XamarinStudio-*
rm -rf ~/Library/Logs/XamarinStudio-*
rm -rf ~/Library/XamarinStudio-*
```

## <a name="summary"></a>요약

이 문서에서는 터미널 명령을 통해 Mac에서 Xamarin을 완전히 제거하는 방법을 설명했습니다. 또한 Windows 컴퓨터에서 **프로그램 및 기능** 옵션(Visual Studio 2015 및 이전 버전)을 통하거나 및 **Visual Studio 설치 관리자**(Visual Studio 2017)를 사용하여 Xamarin을 제거하는 방법도 설명했습니다.


## <a name="related-links"></a>관련 링크

- [스크립트 제거(샘플)](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh)
