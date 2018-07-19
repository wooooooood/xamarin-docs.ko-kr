---
title: Android Emulator 문제 해결
description: 이 아티클에서는 Android Emulator를 사용할 때 발생할 수 있는 문제를 진단하고 해결하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 241f38cbfe013776b2e36b8102ae4b90cf610d80
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935284"
---
# <a name="android-emulator-troubleshooting"></a>Android Emulator 문제 해결

_이 아티클에서는 Android Emulator를 구성하고 실행하는 동안 발생하는 가장 일반적인 경고 메시지 및 문제가 해결 방법 및 설명과 함께 설명됩니다._

<a name="perfwarn" />

## <a name="performance-warnings"></a>성능 경고

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio 2017 15.4 버전부터는 앱을 Android Emulator에 처음 배포할 때 성능 경고 대화 상자가 표시될 수 있습니다. 이러한 경고 대화 상자는 아래에 설명되어 있습니다.

### <a name="computer-does-not-contain-an-intel-procesor"></a>컴퓨터에 인텔 프로세서 없음

![컴퓨터에 인텔 프로세서 없음](troubleshooting-images/01-no-intel-processor.png)

이 대화 상자가 나타나면 컴퓨터에 Android SDK 에뮬레이터 가속에 필요한 인텔 프로세서가 없는 것입니다. 컴퓨터에 인텔 프로세서가 없다면 개발에 물리적 Android 장치를 사용하는 것이 좋습니다.

### <a name="hyper-v-is-installed-or-active"></a>Hyper-V 설치 또는 활성 상태

![Hyper-V 설치 또는 활성 상태](troubleshooting-images/02-hyper-v-active.png)

이 대화 상자가 표시되면 Hyper-V가 설치되었거나 활성 상태라, 사용하지 않게 설정해야 합니다. [Hyper-V를 사용하지 않도록 설정](#disable-hyperv)에서 이 문제 해결 방법을 설명합니다.

### <a name="haxm-is-not-installed"></a>HAXM이 설치되어 있지 않음

![HAXM이 설치되어 있지 않음](troubleshooting-images/03-haxm-not-installed.png)

이 대화 상자는 컴퓨터에 인텔 프로세서가 있고 Hyper-V가 사용하지 않게 설정되었으나 HAXM이 설치되지 않았음을 나타냅니다.
[HAXM 설치](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm)에서 HAXM 설치 단계를 설명합니다.

### <a name="haxm-process-not-running"></a>HAXM 프로세스 실행 중 아님

![HAXM 프로세스 실행 중 아님](troubleshooting-images/04-haxm-process-not-running.png)

이 대화 상자는 컴퓨터에 인텔 프로세서가 있고 Hyper-V가 사용하지 않게 설정되었으며 인텔 HAXM이 설치되었으나 HAXM 프로세스가 실행 중이 아닌 경우에 표시됩니다. 이 문제를 해결하려면 상승된 권한으로 명령 프롬프트 창을 열고 다음 명령을 입력합니다.

```cmd
sc query intelhaxm
```

HAXM 프로세스가 실행 중인 경우 다음과 비슷한 출력이 표시되어야 합니다.

```cmd
SERVICE_NAME: intelhaxm
    TYPE               : 1  KERNEL_DRIVER
    STATE              : 4  RUNNING
                            (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
    WIN32_EXIT_CODE    : 0  (0x0)
    SERVICE_EXIT_CODE  : 0  (0x0)
    CHECKPOINT         : 0x0
    WAIT_HINT          : 0x0
```

`STATE`가 `RUNNING`으로 설정되지 않은 경우 [인텔 Hardware Accelerated Execution Manager 사용 방법](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator)을 참조하여 문제를 해결합니다.


### <a name="other-failures"></a>기타 실패

![기타 실패](troubleshooting-images/05-other-failure.png)

이 대화 상자는 컴퓨터에 인텔 프로세서가 있고 Hyper-V가 사용하지 않게 설정되었으며 인텔 HAXM이 설치되었으나 에뮬레이터가 알 수 없는 이유로 시작에 실패한 경우에 표시됩니다.
이 오류를 해결하려면 [인텔 Hardware Accelerated Execution Manager 사용 방법](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator)을 참조하세요.

### <a name="disabling-performance-warnings"></a>성능 경고 사용 안 함

성능 경고를 표시하지 않기 위해 사용하지 않게 설정할 수 있습니다. Visual Studio에서 **도구 > 옵션 > Xamarin > Android 설정**을 클릭하고 **AVD 가속이 지원되지 않는 경우 경고(HAXM)** 옵션을 사용하지 않게 설정합니다.

[![AVD 가속 경고를 사용하지 않도록 설정](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac용 Visual Studio 빌드 7.2(빌드 559)부터는 앱을 Android Emulator에 처음 배포할 때 성능 경고 대화 상자가 표시될 수 있습니다. 이러한 경고 대화 상자는 아래에 설명되어 있습니다.

### <a name="haxm-is-not-installed"></a>HAXM이 설치되어 있지 않음

![HAXM이 설치되어 있지 않음](troubleshooting-images/03-haxm-not-installed.png)

이 대화 상자는 HAXM이 설치되어 있지 않음을 나타냅니다.
[HAXM 설치](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm)에서 HAXM 설치 단계를 설명합니다.

### <a name="haxm-process-not-running"></a>HAXM 프로세스 실행 중 아님

![HAXM 프로세스 실행 중 아님](troubleshooting-images/04-haxm-process-not-running.png)

HAXM 프로세스가 실행되지 않는 경우 이 대화 상자가 표시됩니다. 이 문제를 해결하려면 [인텔 Hardware Accelerated Execution Manager 사용 방법](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator)을 참조하세요.

### <a name="other-failures"></a>기타 실패

![기타 실패](troubleshooting-images/05-other-failure.png)

이 대화 상자는 에뮬레이터가 어떤 알 수 없는 이유로 시작에 실패했을 때 표시됩니다. 이 오류를 해결하려면 [인텔 Hardware Accelerated Execution Manager 사용 방법](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator)을 참조하세요.

-----

## <a name="deployment-issues"></a>배포 문제

에뮬레이터에 APK 설치 실패 또는 Android Debug Bridge(**adb**) 실행 실패와 관련한 오류가 발생할 경우 Android SDK가 에뮬레이터에 연결할 수 있는지 확인합니다. 이렇게 하려면 다음 단계를 따릅니다.

1. **Android Device Manager**에서 에뮬레이터를 시작합니다(가상 장치를 선택하고 **시작** 클릭).

2. 명령 프롬프트를 열고 **adb**가 설치된 폴더로 이동합니다. 예를 들어 Windows에서는 **C:\\Program Files (x86)\\Android\\android-sdk\\platform-tools\\adb.exe**가 될 수 있습니다.

3. 다음 명령을 입력합니다.

   ```shell
   adb devices
   ```

4. 에뮬레이터를 Android SDK에서 액세스할 수 있는 경우 에뮬레이터가 연결 장치 목록에 나타나야 합니다. 예:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. 에뮬레이터가 이 목록에 나타나지 않는 경우 **Android SDK Manager**를 시작하고 모든 업데이트를 적용한 다음, 다시 에뮬레이터를 시작해 봅니다.


## <a name="haxm-issues"></a>HAXM 문제

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Android Emulator가 제대로 시작되지 않으면 HAXM에 문제가 있을 수 있습니다. HAXM 문제는 다른 가상화 기술과의 충돌, 잘못된 설정 또는 만료된 HAXM 드라이버 때문인 경우가 많습니다.

<a name="virt-conflicts" />

### <a name="haxm-virtualization-conflicts"></a>HAXM 가상화 충돌

HAXM은 Hyper-v, Windows Device Guard 및 일부 바이러스 백신 소프트웨어 등을 사용하는 다른 기술과 충돌할 수 있습니다.

- **Hyper-v** &ndash; **Windows 10 2018년 4월 업데이트(빌드 1803)** 및 Hyper-V가 활성화되기 전에 Windows의 버전을 사용 중인 경우 [Hyper-V 비활성화](#disable-hyperv)의 단계를 따릅니다.

- **Device Guard** &ndash;Device Guard 및 Credential Guard는 Windows 컴퓨터에서 Hyper-V가 비활성화되지 못하게 방지할 수 있습니다. Device Guard 및 Credential Guard를 사용하지 않으려면 [Device Guard 사용 안 함](#disable-devguard)을 참조하세요.

- **바이러스 백신 소프트웨어** &ndash; 하드웨어 지원 가상화(예: Avast)를 사용하는 바이러스 백신 소프트웨어를 실행 중인 경우 이 소프트웨어를 사용하지 않게 설정하거나 제거한 다음 다시 부팅하고 Android SDK 에뮬레이터를 다시 시도합니다.


### <a name="incorrect-bios-settings"></a>잘못된 BIOS 설정

Windows PC에서 HAXM을 사용할 경우 가상화 기술(인텔 VT-x)이 BIOS에서 사용하도록 설정되어야 HAXM이 작동합니다. VT-x를 사용하지 않는 경우 Android Emulator를 시작할 때 다음과 비슷한 오류가 표시됩니다.

**이 컴퓨터가 HAXM의 요구 사항을 충족하지만 인텔 가상화 기술(VT-x)이 켜져 있지 않습니다.**

이 오류를 해결하려면 컴퓨터를 BIOS로 부팅하고 VT-x와 SLAT(Second Level Address Translation)를 모두 사용하도록 설정한 다음 컴퓨터를 다시 Windows로 시작합니다.

<a name="disable-hyperv" />

### <a name="disabling-hyper-v"></a>Hyper-V 비활성화

**Windows 10 2018년 4월 업데이트(빌드 1803)** 및 Hyper-V가 활성화되기 전에 Windows의 버전을 사용 중인 경우 Hyper-V를 비활성화하고 컴퓨터를 다시 부팅하여 HAXM을 설치하고 사용해야 합니다. **Windows 10 2018년 4월 업데이트(빌드 1803)** 이상을 사용하는 경우 Android Emulator 버전 27.2.7 이상은 하드웨어 가속에 HAXM 대신 Hyper-V를 사용할 수 있으므로 Hyper-V를 비활성화할 필요가 없습니다.

다음 단계를 따라 제어판에서 Hyper-V를 비활성화할 수 있습니다.

1. Windows 검색 상자에 **프로그램 및**을 입력한 후 **프로그램 및 기능** 검색 결과를 클릭합니다.

2. 제어판의 **프로그램 및 기능** 대화 상자에서 **Windows 기능 사용/사용 안 함**을 클릭합니다.

    ![Windows 기능 켜기 또는 끄기](troubleshooting-images/win/07-turn-windows-features.png)

3. **Hyper-V**를 선택 취소하고 컴퓨터를 다시 시작합니다.

    ![Windows 기능 대화 상자에서 Hyper-V 비활성화](troubleshooting-images/win/08-uncheck-hyper-v.png)

또는 다음 Powershell cmdlet을 사용하여 Hyper-V를 비활성화할 수 있습니다.

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM 및 Microsoft Hyper-V는 동시에 활성 상태가 될 수 없습니다. 아쉽게도 컴퓨터를 다시 시작하지 않고 Hyper-V와 HAXM 간에 전환하는 방법은 없습니다. 

Device Guard 및 Credential Guard가 활성화된 경우 위의 단계를 수행해도 Hyper-V가 비활성화되지 않는 경우가 있습니다 Hyper-V를 비활성화할 수 없는 경우(또는 비활성화된 것 같지만 HAXM 설치는 여전히 실패하는 경우) 다음 섹션의 단계를 따라 Device Guard 및 Credential Guard를 비활성화하세요.

<a name="disable-devguard" />

### <a name="disabling-device-guard"></a>Device Guard 비활성화

Device Guard 및 Credential Guard는 Windows 컴퓨터에서 Hyper-V가 비활성화되지 못하게 방지할 수 있습니다. 이는 종종 소유 조직에서 구성하고 제어하는 도메인 가입 컴퓨터에 문제가 될 수 있습니다.
Windows 10에서 다음 단계를 따라 **Device Guard**가 실행 중인지 확인하세요.

1. **Windows 검색**에서 **시스템 정보**를 입력하여 **시스템 정보** 앱을 시작합니다.

2. **시스템 요약**에서 **Device Guard 가상화 기반 보안**이 존재하고 **실행 중** 상태인지 확인합니다.

   [![장치 가드가 존재하고 실행 중임](troubleshooting-images/win/09-device-guard-sml.png)](troubleshooting-images/win/09-device-guard.png#lightbox)

Device Guard가 활성화된 경우 다음 단계를 따라 비활성화합니다.

1. 이전 섹션에 설명된 대로 (**Windows 기능 사용/사용 안 함**에서) **Hyper-V**가 비활성화되었는지 확인합니다.

2. Windows 검색 상자에 **gpedit**를 입력하고 **그룹 정책 편집** 검색 결과를 선택합니다. 그러면 **로컬 그룹 정책 편집기**가 실행됩니다.

3. **로컬 그룹 정책 편집기**에서 **컴퓨터 구성 > 관리 템플릿 > 시스템 > Device Guard**로 이동합니다.

   [![로컬 그룹 정책 편집기의 Device Guard](troubleshooting-images/win/10-group-policy-editor-sml.png)](troubleshooting-images/win/10-group-policy-editor.png#lightbox)

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

Android Emulator가 제대로 시작되지 않으면 HAXM에 문제가 있을 수 있습니다. HAXM 문제는 다른 가상화 기술과의 충돌, 잘못된 설정 또는 만료된 HAXM 드라이버 때문인 경우가 많습니다. [HAXM 설치](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm)에서 설명한 단계를 통해 HAXM 드라이버를 다시 설치해 봅니다. 

-----

