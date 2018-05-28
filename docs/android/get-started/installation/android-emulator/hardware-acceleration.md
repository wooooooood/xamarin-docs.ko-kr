---
title: Android Emulator 하드웨어 가속
description: Google Android Emulator 성능을 최대화하기 위해 컴퓨터를 준비하는 방법
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/10/2018
ms.openlocfilehash: 2f0bb6f1371b9ce1b925b876851d58f3c4d01419
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Android Emulator 하드웨어 가속

Google Android Emulator에 하드웨어 가속이 없으면 매우 느립니다. x86 하드웨어를 대상으로 지정하고 두 가지 가상화 기술 중 하나인 특별한 에뮬레이터 하드웨어 이미지를 사용하여 Google Android Emulator의 성능을 상당히 개선할 수 있습니다.

1. **Microsoft의 Hyper-V 및 하이퍼바이저 플랫폼** &ndash; Hyper-V는 물리적 호스트에서 실행 중인 가상화된 컴퓨터 시스템을 사용하도록 설정하는 Windows 10에서 지원되는 가상화 구성 요소입니다. 가속화된 Google Android Emulator 이미지에 대해 권장된 가상화 기술입니다. Hyper-V에 대한 자세한 내용은 [Windows 10의 Hyper-V 가이드](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/)를 참조하세요.
2. **Intel의 HAXM(Hardware Accelerated Execution Manager)** &ndash; Intel CPU를 실행하는 컴퓨터의 가상화 엔진입니다. Hyper-V를 사용할 수 있는 개발자를 위한 권장된 가상화 엔진입니다.

Android SDK Manager는 [구성 및 사용](~/android/deploy-test/debugging/android-sdk-emulator/index.md)에 설명된 대로 **x86** 기반 가상 장치에서 특별히 에뮬레이터 이미지를 실행하도록 지원되는 경우 하드웨어 가속을 사용할 수 있습니다.

## <a name="hyper-v-overview"></a>Hyper-V 개요

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Hyper-V 지원은 현재 미리 보기 상태입니다.

Windows 10을 사용하는 개발자(2018년 4월 업데이트)는 Microsoft의 Hyper-V를 사용하는 것이 좋습니다. Xamarin용 Visual Studio Tools를 통해 Android 장치가 지원되지 않거나 비현실적인 경우 개발자가 Xamarin.Android 응용 프로그램을 쉽게 테스트하고 디버그할 수 있습니다.

Hyper-V 및 Google Android Emulator를 사용하여 시작하려면:

1. **Windows 2018년 4월 10일 업데이트로 업데이트(빌드 1803)** &ndash; 실행 중인 Windows 버전을 확인하려면 Cortana 검색 표시줄을 클릭하고 **정보**를 입력합니다. 검색 결과에서 **사용자 PC 정보**를 선택합니다. **정보** 대화 상자에서 **Windows 사양** 섹션으로 아래로 스크롤합니다. **버전**은 적어도 1803이어야 합니다.

    [![Windows 사양](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Hyper-V 및 Windows 하이퍼바이저 플랫폼 모두 설정** &ndash; Cortana 검색 표시줄에서 **Windows 기능 설정 또는 해제**를 입력합니다.
   **Windows 기능** 대화 상자에서 아래로 스크롤하여 **Windows 하이퍼바이저 플랫폼**을 사용할 수 있도록 합니다.

    [![Hyper-V 및 Windows 하이퍼바이저 플랫폼 설정](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

    Hyper-V 및 Windows 하이퍼바이저 플랫폼을 사용하도록 설정한 후에 Windows를 다시 부팅해야 할 수도 있습니다.

3. **[Visual Studio 15.8 미리 보기 1 이상](https://www.visualstudio.com/vs/preview/)** 설치&ndash; 이 버전의 Visual Studio에서는 Hyper-V를 지원하는 Google Android Emulator를 시작하기 위해 IDE 지원을 제공합니다.

4. **Google Android Emulator 패키지 27.2.7 이상 설치** &ndash; 이 패키지를 설치하려면 Visual Studio에서 **도구 > Android > Android SDK Manager**로 이동합니다. **도구** 탭을 선택하고 Android Emulator 구성 요소가 적어도 버전 27.2.7인지 확인합니다.

    [![Android SDK 및 도구 대화 상자](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Android Emulator 버전이 27.3.1 이전인 경우 **알려진 문제**(다음)에 설명된 추가 해결 단계를 적용합니다.


### <a name="known-issues"></a>알려진 문제

-   에뮬레이터 버전이 27.2.7 이상이지만 27.3.1 이전인 경우 Hyper-V를 사용하려면 다음 해결 방법이 필요합니다.
    1.  파일이 없는 경우 **C:\\Users\\_username_\\.android** 폴더에서 **advancedFeatures.ini**라는 파일을 만듭니다.
    2.  다음 줄을 **advancedFeatures.ini**에 추가합니다.
        ```
        WindowsHypervisorPlatform = on
        ```

-   특정 Intel 및 AMD 기반 프로세서를 사용하는 경우에 성능이 떨어질 수 있습니다.

-   Android 응용 프로그램은 배포 시 로드하는 데 비정상적인 시간이 걸릴 수 있습니다.

-   MMIO 액세스 오류는 일시적으로 Android Emulator의 부팅을 방해합니다. 에뮬레이터를 다시 시작하면 이 문제가 해결됩니다.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Hyper-V 지원에는 Windows 10이 필요합니다. 자세한 내용은 [Hyper-V 요구 사항](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements)을 참조하세요.

-----

## <a name="haxm-overview"></a>HAXM 개요

HAXM은 Intel VT(가상화 기술)를 사용하여 호스트 컴퓨터에서Android 앱 에뮬레이션 속도를 높이는 하드웨어 기반 가상화 엔진(하이퍼바이저)입니다. HAXM은 Intel에서 제공하는 Android x86 에뮬레이터 이미지와 공식 Android SDK Manager와 함께 VT 지원 시스템에서 더 빠른 Android 에뮬레이션을 지원합니다 

VT 기능이 있는 Intel CPU가 탑재된 컴퓨터에서 개발 중인 경우 HAXM을 활용하여 Google Android Emulator의 속도를 크게 높일 수 있습니다(사용 중인 CPU가 VT를 지원하는지 확인하려면 [프로세서가 Intel 가상화 기술을 지원하는지 확인](https://www.intel.com/content/www/us/en/support/processors/000005486.html) 참조).

> [!NOTE]
> VM 가속화된 에뮬레이터는 VirtualBox, VMWare 또는 Docker가 호스팅하는 VM과 같은 다른 VM 내에서 실행할 수 없습니다. [시스템 하드웨어에서 직접](https://developer.android.com/studio/run/emulator-acceleration.html#extensions) Google Android 에뮬레이터를 실행해야 합니다.

Google Android Emulator를 처음으로 사용하기 전에 HAXM이 설치되어 있고 Google Android Emulator에서 사용할 수 있는지 확인하는 것이 좋습니다.

### <a name="verifying-haxm-installation"></a>HAXM 설치 확인

에뮬레이터가 시작될 때 **Android Emulator 시작 중** 창을 보면 HAXM을 사용할 수 있는지 확인할 수 있습니다. Google Android Emulator를 시작하려면 다음을 수행합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **도구 > Android > Android Emulator 관리자**를 클릭하여 Android Emulator 관리자를 시작합니다.

    [![Android Emulator 관리자 메뉴 항목 위치](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. 다음과 유사한 **성능 경고** 대화 상자가 표시되면 해당 컴퓨터에 HAXM이 아직 설치되지 않았거나 올바르게 구성되지 않은 것입니다.

    ![HAXM이 준비되지 않았다는 성능 경고 대화 상자](hardware-acceleration-images/win/11-perf-warn.png)

   이와 같은 **성능 경고** 대화 상자가 표시될 경우 [성능 경고](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn)를 참조하여 원인을 파악하고 근본적인 문제를 해결합니다.

3. **x86** 이미지(예: **VisualStudio\_android-23\_x86\_phone**)를 선택하고 **시작**을 클릭한 후 **실행**을 클릭합니다.

    ![기본 가상 장치 이미지를 사용하여 Google Android Emulator 시작](hardware-acceleration-images/win/02-start-default-avd.png)

4. 에뮬레이터가 시작될 때 **Android Emulator 시작 중** 대화 상자가 표시되기를 기다리세요. HAXM이 설치된 경우 이 스크린샷에 표시된 것처럼 **HAX가 작동 중이고 에뮬레이터가 고속 가상 모드로 실행 중입니다**라는 메시지가 표시됩니다.

    ![Android Emulator 시작 중 대화 상자에 HAXM이 사용 가능한 것으로 표시됨](hardware-acceleration-images/win/03-haxm-detected.png)

   이 메시지가 표시되지 않으면 HAXM이 설치되지 않은 것일 수 있습니다. 예를 들어 다음은 HAXM을 사용할 수 없을 경우 표시될 수 있는 메시지의 스크린샷입니다.

    ![이 컴퓨터에서 HAXM을 사용할 수 없음](hardware-acceleration-images/win/04-haxm-error.png)

   컴퓨터에서 HAXM을 사용할 수 없는 경우 다음 섹션의 단계를 따라 HAXM을 설치하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **도구 > Google 에뮬레이터 관리자**를 클릭하여 Android Emulator 관리자를 시작합니다.

    [![Android Emulator 관리자 메뉴 항목 위치](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. 다음과 유사한 **성능 경고** 대화 상자가 표시되면 해당 컴퓨터에 HAXM이 아직 설치되지 않았거나 올바르게 구성되지 않은 것입니다.

    ![HAXM이 준비되지 않았다는 성능 경고 대화 상자](hardware-acceleration-images/mac/04-avd-warning.png)

   이와 같은 **성능 경고** 대화 상자가 표시될 경우 [성능 경고](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn)를 참조하여 원인을 파악하고 근본적인 문제를 해결합니다.

3. **x86** 이미지(예: **Android\_Accelerated\_x86**)를 선택하고 **시작**을 클릭한 후 **실행**을 클릭합니다.

    [![기본 가상 장치 이미지를 사용하여 Google Android Emulator 시작](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. 에뮬레이터가 시작될 때 **Android Emulator 시작 중** 대화 상자가 표시되기를 기다리세요. HAXM이 설치된 경우 이 스크린샷에 표시된 것처럼 **HAX가 작동 중이고 에뮬레이터가 고속 가상 모드로 실행 중입니다**라는 메시지가 표시됩니다.

    ![Android Emulator 시작 중 대화 상자에 HAXM이 사용 가능한 것으로 표시됨](hardware-acceleration-images/mac/03-haxm-detected.png)

   컴퓨터에서 HAXM을 사용할 수 없는 경우(예: _Intel HAXM이 올바르게 설치되었고 사용 가능한지 확인하십시오_와 비슷한 오류 메시지가 표시되는 경우) 다음 섹션의 단계를 따라 HAXM을 설치하세요.


-----

<a name="install-haxm" />

### <a name="installing-haxm"></a>HAXM 설치

에뮬레이터가 시작되지 않으면 HAXM을 수동으로 설치해야 할 수 있습니다. Windows와 macOS용 HAXM 설치 패키지는 [Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) 페이지에 있습니다. 수동으로 HAXM을 다운로드하여 설치하려면 다음 단계를 따르세요.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Intel 웹 사이트에서 최신 Windows용 [HAXM 가상화 엔진](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) 설치 관리자를 다운로드합니다. Intel 웹 사이트에서 직접 HAXM 설치 프로그램을 다운로드할 경우의 장점은 최신 버전을 사용할 수 있다는 점입니다.

   또는 SDK Manager를 사용하여 HAXM 설치 관리자를 다운로드할 수 있습니다(SDK Manager에서 **도구 > 추가 > Intel x86 Emulator Accelerator(HAXM 설치 관리자)** 클릭). Android SDK는 일반적으로 다음 위치에 HAXM 설치 관리자를 다운로드합니다.

   **C:\\Program Files (x86)\\Android\\android-sdk\\extras\\intel\\Hardware\_Accelerated\_Execution\_Manager**

   SDK Manager는 HAXM을 설치하지 않으며 HAXM 설치 관리자를 위의 위치로 다운로드하기만 하므로 사용자가 수동으로 실행해야 합니다.

2. **intelhaxm-android.exe**를 실행하여 HAXM 설치 관리자를 시작합니다. 설치 관리자 대화 상자에서 기본값을 적용합니다.

   ![Intel Hardware Accelerated Execution Manager 설치 창](hardware-acceleration-images/win/05-haxm-installer.png)

## <a name="hardware-acceleration-and-amd-cpus"></a>하드웨어 가속 및 AMD CPU

현재 Google Android 에뮬레이터는 [Linux에서만](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies) AMD 하드웨어 가속을 지원하므로 Windows를 실행하는 AMD 기반 컴퓨터에는 하드웨어 가속을 사용할 수 없습니다.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Intel 웹 사이트에서 최신 macOS용 [HAXM 가상화 엔진](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) 설치 관리자를 다운로드합니다.

2. HAXM 설치 관리자를 실행합니다. 설치 관리자 대화 상자에서 기본값을 적용합니다.

   [![Intel Hardware Accelerated Execution Manager 설치 창](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>관련 링크

* [Android Emulator에서 앱 실행](https://developer.android.com/studio/run/emulator)
