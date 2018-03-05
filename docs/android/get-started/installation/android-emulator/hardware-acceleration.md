---
title: "Android Emulator 하드웨어 가속"
description: "Android SDK 에뮬레이터 성능을 최대화하기 위해 컴퓨터를 준비하는 방법"
ms.topic: article
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 12/22/2017
ms.openlocfilehash: 53dc85cab94bdf692e088d7c6eea6916d283ba84
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Android Emulator 하드웨어 가속

Android SDK 에뮬레이터는 하드웨어 가속이 없으면 매우 느려질 수 있기 때문에 Android SDK 에뮬레이터의 성능을 획기적으로 높이려면 Intel의 HAXM(Hardware Accelerated Execution Manager)을 사용하는 것이 좋습니다.

<a name="haxm-overview" />

## <a name="haxm-overview"></a>HAXM 개요

HAXM은 Intel VT(가상화 기술)를 사용하여 호스트 컴퓨터에서Android 앱 에뮬레이션 속도를 높이는 하드웨어 기반 가상화 엔진(하이퍼바이저)입니다. HAXM은 Intel에서 제공하는 Android x86 에뮬레이터 이미지와 공식 Android SDK Manager와 함께 VT 지원 시스템에서 더 빠른 Android 에뮬레이션을 지원합니다 VT 기능이 있는 Intel CPU가 탑재된 컴퓨터에서 개발 중인 경우 HAXM을 활용하여 Android SDK 에뮬레이터의 속도를 크게 높일 수 있습니다(사용 중인 CPU가 VT를 지원하는지 확인하려면 [프로세서가 Intel 가상화 기술을 지원하는지 확인](https://www.intel.com/content/www/us/en/support/processors/000005486.html) 참조).

HAXM을 사용할 수 있는 경우 Android SDK 에뮬레이터는 자동으로 이를 사용합니다. ([구성 및 사용](~/android/deploy-test/debugging/android-sdk-emulator/index.md)에 설명된 대로) **x86** 기반 가상 장치를 선택하면 가상 장치에서 하드웨어 가속을 위해 HAXM을 사용합니다. Android SDK 에뮬레이터를 처음으로 사용하기 전에 HAXM이 설치되어 있고 Android SDK 에뮬레이터에서 사용할 수 있는지 확인하는 것이 좋습니다.

> [!NOTE]
> **참고:** 가상 머신에서는 HAXM을 실행할 수 없습니다.

<a name="verify-haxm" />

## <a name="verifying-haxm-installation"></a>HAXM 설치 확인

에뮬레이터가 시작될 때 **Android Emulator 시작 중** 창을 보면 HAXM을 사용할 수 있는지 확인할 수 있습니다. Android SDK 에뮬레이터를 시작하려면 다음을 수행합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **도구 > Android > Android Emulator 관리자**를 클릭하여 Android Emulator 관리자를 시작합니다.

    [![Android Emulator 관리자 메뉴 항목 위치](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png)

2. 다음과 유사한 **성능 경고** 대화 상자가 표시되면 해당 컴퓨터에 HAXM이 아직 설치되지 않았거나 올바르게 구성되지 않은 것입니다.

    ![HAXM이 준비되지 않았다는 성능 경고 대화 상자](hardware-acceleration-images/win/11-perf-warn.png)

   이와 같은 **성능 경고** 대화 상자가 표시될 경우 [성능 경고](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn)를 참조하여 원인을 파악하고 근본적인 문제를 해결합니다.

3. **x86** 이미지(예: **VisualStudio\_android-23\_x86\_phone**)를 선택하고 **시작**을 클릭한 후 **실행**을 클릭합니다.

    ![기본 가상 장치 이미지를 사용하여 Android SDK 에뮬레이터 시작](hardware-acceleration-images/win/02-start-default-avd.png)

4. 에뮬레이터가 시작될 때 **Android Emulator 시작 중** 대화 상자가 표시되기를 기다리세요. HAXM이 설치된 경우 이 스크린샷에 표시된 것처럼 **HAX가 작동 중이고 에뮬레이터가 고속 가상 모드로 실행 중입니다**라는 메시지가 표시됩니다.

    ![Android Emulator 시작 중 대화 상자에 HAXM이 사용 가능한 것으로 표시됨](hardware-acceleration-images/win/03-haxm-detected.png)

   이 메시지가 표시되지 않으면 HAXM이 설치되지 않은 것일 수 있습니다. 예를 들어 다음은 HAXM을 사용할 수 없을 경우 표시될 수 있는 메시지의 스크린샷입니다.

    ![이 컴퓨터에서 HAXM을 사용할 수 없음](hardware-acceleration-images/win/04-haxm-error.png)

   컴퓨터에서 HAXM을 사용할 수 없는 경우 다음 섹션의 단계를 따라 HAXM을 설치하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **도구 > Google 에뮬레이터 관리자**를 클릭하여 Android Emulator 관리자를 시작합니다.

    [![Android Emulator 관리자 메뉴 항목 위치](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png)

2. 다음과 유사한 **성능 경고** 대화 상자가 표시되면 해당 컴퓨터에 HAXM이 아직 설치되지 않았거나 올바르게 구성되지 않은 것입니다.

    ![HAXM이 준비되지 않았다는 성능 경고 대화 상자](hardware-acceleration-images/mac/04-avd-warning.png)

   이와 같은 **성능 경고** 대화 상자가 표시될 경우 [성능 경고](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn)를 참조하여 원인을 파악하고 근본적인 문제를 해결합니다.

3. **x86** 이미지(예: **Android\_Accelerated\_x86**)를 선택하고 **시작**을 클릭한 후 **실행**을 클릭합니다.

    [![기본 가상 장치 이미지를 사용하여 Android SDK 에뮬레이터 시작](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png)

3. 에뮬레이터가 시작될 때 **Android Emulator 시작 중** 대화 상자가 표시되기를 기다리세요. HAXM이 설치된 경우 이 스크린샷에 표시된 것처럼 **HAX가 작동 중이고 에뮬레이터가 고속 가상 모드로 실행 중입니다**라는 메시지가 표시됩니다.

    ![Android Emulator 시작 중 대화 상자에 HAXM이 사용 가능한 것으로 표시됨](hardware-acceleration-images/mac/03-haxm-detected.png)

   컴퓨터에서 HAXM을 사용할 수 없는 경우(예: _Intel HAXM이 올바르게 설치되었고 사용 가능한지 확인하십시오_와 비슷한 오류 메시지가 표시되는 경우) 다음 섹션의 단계를 따라 HAXM을 설치하세요.


-----

<a name="install-haxm" />

## <a name="installing-haxm"></a>HAXM 설치

에뮬레이터가 시작되지 않으면 HAXM을 수동으로 설치해야 할 수 있습니다. Windows와 macOS용 HAXM 설치 패키지는 [Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) 페이지에 있습니다. 수동으로 HAXM을 다운로드하여 설치하려면 다음 단계를 따르세요.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Intel 웹 사이트에서 최신 Windows용 [HAXM 가상화 엔진](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) 설치 관리자를 다운로드합니다. Intel 웹 사이트에서 직접 HAXM 설치 프로그램을 다운로드할 경우의 장점은 최신 버전을 사용할 수 있다는 점입니다.

   또는 SDK Manager를 사용하여 HAXM 설치 관리자를 다운로드할 수 있습니다(SDK Manager에서 **도구 > 추가 > Intel x86 Emulator Accelerator(HAXM 설치 관리자)** 클릭). Android SDK는 일반적으로 다음 위치에 HAXM 설치 관리자를 다운로드합니다.

   **C:\\Program Files (x86)\\Android\\android-sdk\\extras\\intel\\Hardware\_Accelerated\_Execution\_Manager**

   SDK Manager는 HAXM을 설치하지 않으며 HAXM 설치 관리자를 위의 위치로 다운로드하기만 하므로 사용자가 수동으로 실행해야 합니다.

2. **intelhaxm-android.exe**를 실행하여 HAXM 설치 관리자를 시작합니다. 설치 관리자 대화 상자에서 기본값을 적용합니다.

   ![Intel Hardware Accelerated Execution Manager 설치 창](hardware-acceleration-images/win/05-haxm-installer.png)

다음과 같은 오류 대화 상자(_이 컴퓨터는 Intel VT-x(가상화 기술)를 지원하지 않거나 Hyper-에서 배타적으로 사용되고 있습니다V_)가 표시되면 HAXM을 설치하기 전에 Hyper-V를 비활성화해야 합니다.

![Hyper-V 충돌로 인해 HAXM을 설치할 수 없음](hardware-acceleration-images/win/06-cant-install-haxm.png)

다음 섹션에서는 Hyper-V를 비활성화하는 방법을 설명합니다.

<a name="disable-hyperv" />

## <a name="disabling-hyper-v"></a>Hyper-V 비활성화

Hyper-V가 활성화된 Windows를 사용 중인 경우 HAXM을 설치하고 사용하려면 Hyper-V를 비활성화하고 컴퓨터를 다시 부팅해야 합니다. 다음 단계를 따라 제어판에서 Hyper-V를 비활성화할 수 있습니다.

1. Windows 검색 상자에 **프로그램 및**을 입력한 후 **프로그램 및 기능** 검색 결과를 클릭합니다.

2. 제어판의 **프로그램 및 기능** 대화 상자에서 **Windows 기능 사용/사용 안 함**을 클릭합니다.

    ![Windows 기능 켜기 또는 끄기](hardware-acceleration-images/win/07-turn-windows-features.png)

3. **Hyper-V**를 선택 취소하고 컴퓨터를 다시 시작합니다.

    ![Windows 기능 대화 상자에서 Hyper-V 비활성화](hardware-acceleration-images/win/08-uncheck-hyper-v.png)

또는 다음 Powershell cmdlet을 사용하여 Hyper-V를 비활성화할 수 있습니다.

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM 및 Microsoft Hyper-V는 동시에 활성 상태가 될 수 없습니다. 아쉽게도 현재 컴퓨터를 다시 시작하지 않고 Hyper-V와 HAXM 간에 전환하는 방법은 없습니다. [Visual Studio Emulator for Android](~/android/deploy-test/debugging/visual-studio-android-emulator.md)(Hyper-V에 따라 다름)를 사용하려는 경우 컴퓨터를 다시 부팅하지 않고 Android SDK 에뮬레이터를 사용할 수는 없습니다. Hyper-V 및 HAXM을 모두 사용하는 한 가지 방법은 [하이퍼바이저를 사용하지 않는 부팅 항목 만들기](https://blogs.msdn.microsoft.com/virtual_pc_guy/2008/04/14/creating-a-no-hypervisor-boot-entry/)에 설명된 대로 다중 부팅 설치 프로그램을 만드는 것입니다.

Device Guard 및 Credential Guard가 활성화된 경우 위의 단계를 수행해도 Hyper-V가 비활성화되지 않는 경우가 있습니다 Hyper-V를 비활성화할 수 없는 경우(또는 비활성화된 것 같지만 HAXM 설치는 여전히 실패하는 경우) 다음 섹션의 단계를 따라 Device Guard 및 Credential Guard를 비활성화하세요.

<a name="disable-devguard" />

## <a name="disabling-device-guard"></a>Device Guard 비활성화

Device Guard 및 Credential Guard는 Windows 컴퓨터에서 Hyper-V가 비활성화되지 못하게 방지할 수 있습니다. 이는 종종 소유 조직에서 구성하고 제어하는 도메인 가입 컴퓨터에 문제가 될 수 있습니다.
Windows 10에서 다음 단계를 따라 **Device Guard**가 실행 중인지 확인하세요.

1. **Windows 검색**에서 **시스템 정보**를 입력하여 **시스템 정보** 앱을 시작합니다.

2. **시스템 요약**에서 **Device Guard 가상화 기반 보안**이 존재하고 **실행 중** 상태인지 확인합니다.

   [![장치 가드가 존재하고 실행 중임](hardware-acceleration-images/win/09-device-guard-sml.png)](hardware-acceleration-images/win/09-device-guard.png)

Device Guard가 활성화된 경우 다음 단계를 따라 비활성화합니다.

1. 이전 섹션에 설명된 대로 (**Windows 기능 사용/사용 안 함**에서) **Hyper-V**가 비활성화되었는지 확인합니다.

2. Windows 검색 상자에 **gpedit**를 입력하고 **그룹 정책 편집** 검색 결과를 선택합니다. 그러면 **로컬 그룹 정책 편집기**가 실행됩니다.

3. **로컬 그룹 정책 편집기**에서 **컴퓨터 구성 > 관리 템플릿 > 시스템 > Device Guard**로 이동합니다.

   [![로컬 그룹 정책 편집기의 Device Guard](hardware-acceleration-images/win/10-group-policy-editor-sml.png)](hardware-acceleration-images/win/10-group-policy-editor.png)

4. (위에 보이는 것과 같이) **가상화 기반 보안 켜기**를 **사용 안 함**으로 변경하고 **로컬 그룹 정책 편집기**를 종료합니다.

5. Windows 검색 상자에 **cmd**를 입력합니다. 검색 결과에 **명령 프롬프트**가 나타나면 **명령 프롬프트**를 마우스 오른쪽 단추로 클릭하고 **관리자 권한으로 실행**을 선택합니다.

6. 다음 명령을 복사하여 명령 프롬프트 창에 붙여넣습니다(**Z:** 드라이브가 사용중인 경우 사용되지 않는 드라이브 문자를 대신 선택).

        mountvol Z: /s
        copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
        bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
        bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
        mountvol Z: /d

7. 컴퓨터를 다시 시작합니다. 부팅 화면에서 다음과 같은 메시지가 표시됩니다.

   **Credential Guard를 비활성화하시겠습니까?**

   메시지가 나타나면 표시된 키를 눌러 Credential Guard를 비활성화합니다.

8. 컴퓨터가 다시 부팅된 후 (이전 단계에 설명된 대로) Hyper-V가 비활성화되었는지 다시 확인하세요.

Hyper-V가 아직 비활성화되지 않은 경우 도메인 가입 컴퓨터의 정책으로 인해 Device Guard 또는 Credential Guard가 비활성화되지 않을 수 있습니다. 이 경우 도메인 관리자에게 Credential Guard를 사용하지 않을 수 있도록 예외를 요청할 수 있습니다. 또는 도메인에 가입되지 않은 컴퓨터를 사용하여 HAXM을 사용할 수 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Intel 웹 사이트에서 최신 macOS용 [HAXM 가상화 엔진](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) 설치 관리자를 다운로드합니다.

2. HAXM 설치 관리자를 실행합니다. 설치 관리자 대화 상자에서 기본값을 적용합니다.

   [![Intel Hardware Accelerated Execution Manager 설치 창](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png)

-----
