---
title: Xamarin 제거
description: 컴퓨터에서 Xamarin 제품 제거
ms.topic: article
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: 2c2ba84167924916c3bec27379d33c47e8dab360
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="uninstalling-xamarin"></a>Xamarin 제거

> [!IMPORTANT]
> 이 문서에서는 Windows 또는 Mac 컴퓨터에서 Xamarin Studio 또는 다른 Xamarin 제품을 제거하는 방법을 설명합니다. Mac용 Visual Studio를 제거하는 방법에 대한 자세한 내용은 docs.microsoft.com에서 [제거](https://docs.microsoft.com/visualstudio/mac/uninstall) 가이드를 참조하세요.

Xamarin Studio와 같은 독립형 앱을 포함하는 플랫폼 간 응용 프로그램 개발을 지원하는 여러 가지 Xamarin 제품 및 Visual Studio에서 Xamarin 지원과 같은 다른 앱에 대한 확장이 있습니다.

이 가이드에서는 macOS 또는 Windows의 Visual Studio에서 Xamarin 기능을 제거하는 방법을 설명합니다.

1.  [Xamarin Studio 제거](#uninstallxamarinstudio)
  1.  [Mono 제거](#uninstallmono)
  1.  [Uninstalling Xamarin.Android](#uninstallandroid)
  1.  [Uninstalling Xamarin.iOS](#uninstallios)
  1.  [Uninstalling Xamarin.Mac](#uninstallmac)
  2.  [검사기 및 Workbooks 제거](#uninstallworkbooks)
1.  [Windows에서 Xamarin 제거](#uninstallwindows)
  1.  [Visual Studio 2015 이하에서 Xamarin 제거](#uninstallvs2015)
  1.  [Visual Studio 2017에서 Xamarin 제거](#uninstallvs2017)
1.  [Mac용 Visual Studio 제거](#uninstallvsmac)

유니버설 설치 관리자를 사용하여 Xamarin을 다시 설치해야 하는 경우 항상 컴퓨터를 먼저 다시 부팅하는 것이 좋습니다.

## <a name="uninstalling-xamarin-on-mac"></a>Mac에서 Xamarin 제거

각 제품을 개별적으로 제거하려면 이 가이드의 관련 섹션을 참조하세요. 이 가이드의 모든 내용을 따라 전체 Xamarin 도구 집합을 제거할 수 있습니다.

[스크립트 제거](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh)를 사용하는 도움말은 이 가이드의 맨 아래에서 [스크립트 제거 사용](#uninstallscript)으로 이동합니다.

<a name="uninstallxamarinstudio" />

### <a name="uninstall-xamarin-studio"></a>Xamarin Studio 제거

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

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Mono SDK(MDK) 제거

Mono는 Microsoft .NET Framework의 오픈 소스 구현으로서 Xamarin.iOS, Xamarin.Android 및 Xamarin.Mac 등 모든 Xamarin 제품에서 C#으로 이러한 플랫폼을 개발하는 데 사용됩니다.

> [!IMPORTANT]
> Mono를 사용하는 Xamarin 이외에도 Unity와 같은 응용 프로그램이 있습니다. Mono를 제거하기 전에 Mono에 대한 다른 종속성이 없는지 확인하세요.

컴퓨터에서 Mono 프레임워크를 제거하려면 터미널에서 다음 명령을 실행합니다.

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Xamarin.Android 제거

Xamarin.Android를 설치하고 사용하려면 Android SDK, Java SDK 등의 몇 가지 항목이 필요합니다. 이러한 필수 구성 요소에 대한 자세한 정보는 [수동 설치](https://docs.microsoft.com/visualstudio/mac/installation/) 가이드에서 제공됩니다.

Xamarin.Android를 제거하려면 다음 명령을 사용합니다.

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Android SDK 및 Java SDK 제거

Android SDK는 Android 응용 프로그램 개발에 필요합니다. Android SDK의 모든 부분을 완전히 제거하려면 다음과 같이 **~/Library/Developer/Xamarin/**에서 해당 파일을 찾아 **휴지통**으로 이동합니다.

 [![](uninstalling-xamarin-images/image2.png "Android SDK의 모든 부분을 완전히 제거하려면 다음과 같이 해당 파일을 찾아 휴지통으로 이동합니다.")](uninstalling-xamarin-images/image2.png#lightbox)

Java SDK(JDK)는 이미 Mac OS X에 포함되어 있으므로 제거할 필요가 없습니다.

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Xamarin.iOS 제거

Xamarin.iOS를 사용하면 Mac의 Xamarin Studio에서 C# 또는 F#으로 iOS 응용 프로그램을 개발할 수 있습니다.
이전 버전의 Xamarin.iOS에서는 Visual Studio의 iOS 개발을 지원하기 위해 Xamarin Build Host가 자동으로 함께 설치됩니다. 컴퓨터에서 두 가지 요소를 모두 제거하려면 다음 단계를 수행합니다.

터미널에서 다음 명령을 사용하여 파일 시스템에서 모든 Xamarin.iOS 파일을 제거합니다.

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
```

#### <a name="uninstall-the-mac-build-host"></a>Mac 빌드 호스트 제거

참고: Xamarin 4로 업데이트한 경우 이미 제거되었을 수 있습니다. 터미널에서 다음 명령을 실행하여 빌드 호스트 응용 프로그램을 제거합니다.

```bash
sudo rm -rf "/Applications/Xamarin.iOS Build Host.app"
```

빌드 호스트 프로세스 또는 `launchd` 작업은 특정 포트에서 실행하거나 수신 대기할 수 있습니다.
터미널에서 `launchctl list | grep com.xamarin.mtvs.buildserver`을 실행하여 상태를 확인할 수 있습니다.

```bash
sudo launchctl unload /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
sudo rm -f /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
```

<a name="uninstallmac" />

### <a name="uninstall-xamarinmac"></a>Xamarin.Mac 제거

Xamarin Studio가 정상적으로 제거되면 다음 두 가지 명령으로 Mac 컴퓨터에서 Xamarin.Mac 제품과 라이선스를 각각 제거할 수 있습니다.

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks-and-inspector"></a>Workbooks 및 Inspector 제거

다음 Bash 명령은 Xamarin 검사기 및 Workbooks 버전 1.2.2 이상을 제거합니다.

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

이전 버전은 [Workbooks](~/tools/workbooks/install.md#uninstall-macos) 제거 가이드를 참조하세요.

### <a name="uninstall-the-xamarin-installer"></a>Xamarin 설치 관리자 제거

Xamarin Universal 설치 관리자의 모든 흔적을 제거하려면 다음 명령을 사용합니다.

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Preferences/Xamarin/
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>스크립트 제거 사용

[스크립트 제거](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh)는 컴퓨터에서 Xamarin을 제거합니다. 스크립트 제거를 사용하려면:

1.  스크립트 제거를 다운로드하고 다운로드 위치를 적어둡니다. 기본적으로 **/Downloads** 디렉터리입니다.
1.  **터미널**을 열고 스크립트를 다운로드한 위치로 작업 디렉터리를 변경합니다.

        $ cd /location/of/file

1. 스크립트를 실행 가능으로 설정하고 **sudo**로 실행합니다.

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

1. 마지막으로 제거 스크립트를 삭제합니다.

이 시점에서 사용자의 컴퓨터에서 Xamarin은 제거되어야 합니다.

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Windows에서 Xamarin 제거

<a name="uninstallvs2015" />

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 및 이전 버전

**제어판**을 통해 Windows 컴퓨터에서 Xamarin을 제거할 수 있습니다. 아래 그림과 같이 **프로그램 및 기능** 또는 **프로그램 > 프로그램 제거**로 이동합니다.

 [![](uninstalling-xamarin-images/image3.png "프로그램 및 기능 또는 프로그램으로 이동 다음과 같이 프로그램 제거")](uninstalling-xamarin-images/image3.png#lightbox) [![](uninstalling-xamarin-images/image3.png "프로그램 및 기능 또는 프로그램으로 이동 다음과 같이 프로그램 제거")](uninstalling-xamarin-images/image3.png#lightbox)

Xamarin Studio를 제거하려면 프로그램 목록에서 **Xamarin Studio 5.x.x**를 찾아 **제거** 단추를 클릭합니다. Visual Studio용 Xamarin 확장 및 SDK를 제거하려면 프로그램 목록에서 **Xamarin**을 찾고 **제거**를 클릭합니다. 아래 스크린샷에 설명되어 있습니다.

 [![](uninstalling-xamarin-images/image4a.png "이 스크린샷에 설명되어 있습니다.")](uninstalling-xamarin-images/image4a.png#lightbox)

이러한 프로그램은 모든 Xamarin 구성 요소에 대해 완전히 제거될 수 있습니다.

-  Android SDK


  [![](uninstalling-xamarin-images/image5.png "이러한 프로그램은 모든 Xamarin 구성 요소에 대해 완전히 제거될 수 있습니다.")](uninstalling-xamarin-images/image5.png#lightbox)
-  GTK#


  [![](uninstalling-xamarin-images/image6.png "이러한 프로그램은 모든 Xamarin 구성 요소에 대해 완전히 제거될 수 있습니다.")](uninstalling-xamarin-images/image6.png#lightbox)
-  Xamarin Universal 설치 관리자


 [![](uninstalling-xamarin-images/image7.png "이러한 프로그램은 모든 Xamarin 구성 요소에 대해 완전히 제거될 수 있습니다.")](uninstalling-xamarin-images/image7.png#lightbox)
-  Java SDK(다른 종속성이 있을 수 있으므로 제거할 때 유의하세요.)


 [![](uninstalling-xamarin-images/image8.png "다른 종속성이 있을 수 있으므로 Java SDK를 제거할 때 유의하세요.")](uninstalling-xamarin-images/image8.png#lightbox)

Visual Studio를 완전히 제거하려면 [Microsoft의 지침](https://msdn.microsoft.com/library/mt720585.aspx)을 따르세요.


<a name="uninstallvs2017" />

## <a name="visual-studio-2017"></a>Visual Studio 2017

설치 관리자 앱을 사용하여 Visual Studio 2017에서 Xamarin을 제거할 수 있습니다.

1. **시작 메뉴**를 사용하여 **Visual Studio 설치 관리자**를 엽니다.

  [![](uninstalling-xamarin-images/vs2017-01-sml.png "Visual Studio 설치 관리자 시작")](uninstalling-xamarin-images/vs2017-01.png#lightbox)

1. 변경하려는 인스턴스의 **수정** 단추를 누릅니다.

  [![](uninstalling-xamarin-images/vs2017-02-sml.png "수정 단추 누르기")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

1. **워크로드** 탭에서 **.NET을 사용하는 모바일 개발** 옵션의 선택을 취소합니다(**모바일 및 게임** 섹션에서).

  [![](uninstalling-xamarin-images/vs2017-03-sml.png "모바일 개발 워크로드 선택 취소")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

1. 창의 오른쪽 아래에 있는 **수정** 단추를 클릭합니다.
1. 설치 관리자는 선택이 취소된 구성 요소를 제거합니다(설치 관리자가 변경하기 전에 Visual Studio 2017이 닫힘).

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
>Visual Studio 설치 관리자에서 **복구** 옵션을 실행하여 해결할 수 있습니다. 그러면 누락된 구성 요소를 다시 설치합니다.

<a name="uninstallvsmac" />

## <a name="uninstalling-visual-studio-for-mac"></a>Mac용 Visual Studio 제거

Mac용 Visual Studio를 제거하지만 Xamarin Studio를 유지하려면 **/Applications** 디렉터리에서 **Visual Studio.app**을 찾아 휴지통으로 끌어놓습니다. 또는 아래 그림과 같이 마우스 오른쪽 단추를 클릭하고 **Move to Trash**(휴지통으로 이동)를 선택합니다.

 [![](uninstalling-xamarin-images/image9.png "Visual Studio 아이콘을 마우스 오른쪽 단추로 클릭하고 휴지통으로 이동을 선택합니다.")](uninstalling-xamarin-images/image9.png#lightbox)

컴퓨터에서 Xamarin을 완전히 제거하려면 먼저 Mac용 Visual Studio를 삭제한 다음, [Xamarin Studio 제거](#uninstallxamarinstudio) 섹션에 나열된 단계에 따릅니다.

## <a name="summary"></a>요약

이 문서에서는 터미널 명령을 사용하여 Mac에서 Xamarin을 완전히 제거할 뿐만 아니라 **프로그램 및 기능** 옵션(Visual Studio 2015 이하) 및 Visual Studio 2017용 **Visual Studio 설치 관리자**를 사용하여 Windows 컴퓨터에서 Xamarin을 제거하는 방법을 알아봅니다.


## <a name="related-links"></a>관련 링크

- [스크립트 제거(샘플)](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh)
