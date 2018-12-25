---
title: Android Emulator 문제 해결
description: 이 아티클에서는 Android Emulator를 사용할 때 발생할 수 있는 문제를 진단하고 해결하는 방법을 설명합니다.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/27/2018
ms.openlocfilehash: 5f8d977c126cfe4bdfdb48470841ee17de6bda31
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117735"
---
# <a name="android-emulator-troubleshooting"></a>Android 에뮬레이터 문제 해결

_이 문서에서는 Android Emulator를 구성하고 실행하는 동안 발생하는 가장 일반적인 경고 메시지 및 문제에 대해 설명합니다. 또한 에뮬레이터 문제를 진단하는 데 도움이 되는 다양한 문제 해결 팁과 이러한 오류를 해결하는 솔루션에 대해 설명합니다._

::: zone pivot="windows"

## <a name="deployment-issues-on-windows"></a>Windows의 배포 문제

앱을 배포할 때 에뮬레이터에서 일부 오류 메시지가 표시될 수 있습니다. 가장 일반적인 오류 및 해결 방법이 여기에 설명되어 있습니다.

### <a name="deployment-errors"></a>배포 오류

에뮬레이터에 APK 설치 실패 또는 Android Debug Bridge(**adb**) 실행 실패에 대한 오류가 표시되면 Android SDK가 에뮬레이터에 연결할 수 있는지 확인합니다. 에뮬레이터 연결을 확인하려면 다음 단계를 수행합니다.

1. **Android Device Manager**에서 에뮬레이터를 시작합니다(가상 장치를 선택하고 **시작** 클릭).

2. 명령 프롬프트를 열고 **adb**가 설치된 폴더로 이동합니다. Android SDK가 기본 위치에 설치된 경우 **adb**는 **C:\\프로그램 파일(x86)\\Android\\android sdk\\플랫폼 도구\\adb.exe**에 있습니다. 그렇지 않은 경우, 컴퓨터에서 Android SDK의 위치에 대한 이 경로를 수정합니다.

3. 다음 명령을 입력합니다.

   ```shell
   adb devices
   ```

4. 에뮬레이터를 Android SDK에서 액세스할 수 있는 경우 에뮬레이터가 연결 디바이스 목록에 나타나야 합니다. 예:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. 에뮬레이터가 이 목록에 나타나지 않는 경우 **Android SDK Manager**를 시작하고 모든 업데이트를 적용한 다음, 다시 에뮬레이터를 시작해 봅니다.


### <a name="mmio-access-error"></a>MMIO 액세스 오류

**MMIO 액세스 오류가 발생 했습니다.** 라는 메시지가 표시되면 에뮬레이터를 다시 시작하세요.


<a name="gps-win" />

## <a name="missing-google-play-services"></a>Google Play 서비스 누락

에뮬레이터에서 실행 중인 가상 디바이스에 Google Play 서비스 또는 Google Play 스토어가 설치되어 있지 않은 경우, 이 조건은 이러한 패키지를 포함하지 않고 가상 디바이스를 만들 때 발생하는 경우가 많습니다. 가상 디바이스를 만들 때([Android Device Manager를 사용하여 가상 디바이스 관리](~/android/get-started/installation/android-emulator/device-manager.md) 참조) 다음 옵션 중 하나 또는 둘 다를 선택해야 합니다.

- **Google API** &ndash;에는 가상 장치에 Google Play 서비스가 포함되어 있습니다.
- **Google Play 스토어** &ndash;에는 가상 장치에 Google Play 스토어가 포함되어 있습니다.

예를 들어 이 가상 디바이스에는 Google Play 서비스 및 Google Play 스토어가 포함됩니다.

[![Google Play 서비스 및 Google Play 스토어가 활성화된 예제 AVD](troubleshooting-images/win/00-add-gps-w158-sml.png)](troubleshooting-images/win/00-add-gps-w158.png#lightbox)

> [!NOTE]
> Google Play 스토어 이미지는 픽셀, 픽셀 2, Nexus 5 및 Nexus 5X와 같은 몇 가지 기본 디바이스 유형에만 사용할 수 있습니다.


<a name="perf-win" />

## <a name="performance-issues"></a>성능 문제

성능 문제는 일반적으로 다음 문제 중 하나로 인해 발생합니다.

- 에뮬레이터가 하드웨어 가속 없이 실행 중입니다.

- 에뮬레이터에서 실행 중인 가상 디바이스가 x86 기반 시스템 이미지를 사용하지 않습니다.

다음 섹션에서는 이러한 시나리오를 자세히 설명합니다.

### <a name="hardware-acceleration-is-not-enabled"></a>하드웨어 가속을 사용할 수 없습니다.

하드웨어 가속을 사용하도록 설정하지 않은 경우 Device Manager에서 시작하면 Windows 하이버파이저 플랫폼(WHPX)이 제대로 구성되지 않았음을 나타내는 오류 메시지가 포함된 대화 상자가 생성됩니다.

![디바이스 관리자 경고 예제](troubleshooting-images/win/01-dev-mgr-warning-w158.png)

이 오류 메시지가 표시되면 하드웨어 가속을 확인하고 사용하도록 설정할 수 있는 단계에 대한 아래의 [하드웨어 가속 문제](#accel-issues-win)를 참조하세요.


### <a name="acceleration-is-enabled-but-the-emulator-runs-too-slowly"></a>가속을 사용할 수 있지만 에뮬레이터가 너무 느리게 실행됩니다. 

이 문제의 일반적인 원인은 가상 디바이스(AVD)에서 x86 기반 이미지를 사용하지 않기 때문입니다. 가상 디바이스를 만들 때([Android Device Manager를 사용하여 가상 디바이스 관리](~/android/get-started/installation/android-emulator/device-manager.md) 참조) x86 기반 시스템 이미지를 선택해야 합니다.

[![가상 장치용 x86 시스템 이미지 선택](troubleshooting-images/win/02-x86-virtual-device-w158-sml.png)](troubleshooting-images/win/02-x86-virtual-device-w158.png#lightbox)


<a name="accel-issues-win" />

## <a name="hardware-acceleration-issues"></a>하드웨어 가속 문제

하드웨어 가속에 Hyper-V 또는 HAXM을 사용하든 상관없이 구성 문제가 발생하거나 컴퓨터의 다른 소프트웨어와 충돌할 수 있습니다. 명령 프롬프트를 열고 다음 명령을 입력하여 하드웨어 가속이 활성화되었는지(및 에뮬레이터에서 사용 중인 가속 방법) 확인할 수 있습니다.

```cmd
"C:\Program Files (x86)\Android\android-sdk\emulator\emulator-check.exe" accel
```

이 명령은 Android SDK가 **C:\\프로그램 파일(x86)\\Android\\android-sdk**의 기본 위치에 설치되어 있다고 가정합니다. 그렇지 않은 경우 컴퓨터에서 Android SDK의 위치에 대한 위의 경로를 수정합니다.

### <a name="hardware-acceleration-not-available"></a>하드웨어 가속을 사용할 수 없음

Hyper-V를 사용할 수 있는 경우 **emulator-check.exe accel** 명령에서 다음 예제와 같은 메시지가 반환됩니다.

```cmd
HAXM is not installed, but Windows Hypervisor Platform is available.
```

HAXM을 사용할 수 있는 경우 다음 예제와 같은 메시지가 반환됩니다.

```cmd
HAXM version 6.2.1 (4) is installed and usable.
```

하드웨어 가속을 사용할 수 없는 경우 다음 예제와 같은 메시지가 표시됩니다(Hyper-V를 찾을 수 없는 경우 에뮬레이터에서 HAXM을 찾음).

```cmd
HAXM is not installed on this machine
```

하드웨어 가속을 사용할 수 없는 경우 컴퓨터에서 하드웨어 가속을 사용하는 방법에 대해 알아보려면 [Hyper-V로 가속화](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vswin#hyper-v-win)를 참조하세요.

### <a name="incorrect-bios-settings"></a>잘못된 BIOS 설정

BIOS가 하드웨어 가속을 지원하도록 제대로 구성되지 않은 경우 **emulator-check.exe accel** 명령을 실행할 때 다음 예제와 유사한 메시지가 표시됩니다.

```cmd
VT feature disabled in BIOS/UEFI
```

이 문제를 해결하려면 컴퓨터의 BIOS를 재부팅하고 다음 옵션을 활성화합니다.

- 가상화 기술(마더보드 제조업체에 따라 다른 레이블이 있을 수 있음).
- 하드웨어 강제 데이터 실행 방지.

하드웨어 가속이 활성화되어 있고 BIOS가 제대로 구성되어 있으면 에뮬레이터가 하드웨어 가속을 사용하여 성공적으로 실행되어야 합니다.
그러나 다음에 설명된 대로 Hyper-V 및 HAXM과 관련된 문제로 인해 문제가 계속 발생할 수 있습니다.


### <a name="hyper-v-issues"></a>Hyper-V 문제

경우에 따라 **Windows 기능 설정 또는 해제** 대화 상자에서 **Hyper-V**와 **Windows 하이퍼바이저 플랫폼**을 모두 사용하도록 설정하면 Hyper-V가 제대로 설정되지 않을 수 있습니다. Hyper-V가 활성화되었는지 확인하려면 다음 단계를 수행합니다.

1. Windows 검색 상자에 **powershell**을 입력합니다.

2. 검색 결과에서 **Windows PowerShell**을 마우스 오른쪽 단추로 클릭하고 **관리자 권한으로 실행**을 선택합니다.

3. PowerShell 콘솔에서 다음 명령을 입력합니다.

    ```powershell
    Get-WindowsOptionalFeature -FeatureName Microsoft-Hyper-V-All -Online
    ```

    Hyper-V가 활성화되어 있지 않으면 Hyper-V 상태가 **사용 안 함**임을 나타내는 다음 예제와 유사한 메시지가 표시됩니다.

    ```
    FeatureName      : Microsoft-Hyper-V-All
    DisplayName      : Hyper-V
    Description      : Provides services and management tools for creating and running virtual machines and their resources.
    RestartRequired  : Possible
    State            : Disabled
    CustomProperties : 
    ```

4. PowerShell 콘솔에서 다음 명령을 입력합니다.

    ```powershell
    Get-WindowsOptionalFeature -FeatureName HypervisorPlatform -Online
    ```
    하이퍼바이저가 활성화되어 있지 않으면 HypervisorPlatform 상태가 **사용 안 함**임을 나타내는 다음 예제와 유사한 메시지가 표시됩니다.

    ```
    FeatureName      : HypervisorPlatform
    DisplayName      : Windows Hypervisor Platform
    Description      : Enables virtualization software to run on the Windows hypervisor
    RestartRequired  : Possible
    State            : Disabled
    CustomProperties : 
    ```

Hyper-V 및/또는 HypervisorPlatform이 활성화되어 있지 않은 경우 다음 PowerShell 명령을 사용하여 활성화합니다.

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
Enable-WindowsOptionalFeature -Online -FeatureName HypervisorPlatform -All
```

이러한 명령이 완료되면 다시 부팅합니다. 

Hyper-V를 사용하도록 설정하는 방법에 대한 자세한 내용(배포 이미지 서비스 및 관리 도구를 사용하여 Hyper-V를 사용하도록 설정하는 기술 포함)은 [Hyper-V 설치](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)를 참조하세요.


### <a name="haxm-issues"></a>HAXM 문제

HAXM 문제는 다른 가상화 기술과의 충돌, 잘못된 설정 또는 만료된 HAXM 드라이버 때문인 경우가 많습니다.

### <a name="haxm-process-is-not-running"></a>HAXM 프로세스 실행 중 아님

HAXM이 설치된 경우 명령 프롬프트를 열고 다음 명령을 입력하여 HAXM 프로세스가 실행 중인지 확인할 수 있습니다.

```cmd
sc query intelhaxm
```

HAXM 프로세스가 실행 중인 경우 다음 결과와 비슷한 출력이 표시되어야 합니다.

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

`STATE`가 `RUNNING`으로 설정되지 않은 경우 [인텔 Hardware Accelerated Execution Manager 사용 방법](https://software.intel.com/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator)을 참조하여 문제를 해결합니다.


<a name="virt-conflicts" />

#### <a name="haxm-virtualization-conflicts"></a>HAXM 가상화 충돌

HAXM은 Hyper-v, Windows Device Guard 및 일부 바이러스 백신 소프트웨어 등을 사용하는 다른 기술과 충돌할 수 있습니다.

- **Hyper-V** &ndash; **Windows 10 2018년 4월 업데이트(빌드 1803)** 및 Hyper-V가 활성화되기 전에 Windows의 버전을 사용 중인 경우, HAXM을 활성화할 수 있도록 [Hyper-V 비활성화](#disable-hyperv)의 단계를 따릅니다.

- **Device Guard** &ndash;Device Guard 및 Credential Guard는 Windows 컴퓨터에서 Hyper-V가 비활성화되지 못하게 방지할 수 있습니다. Device Guard 및 Credential Guard를 사용하지 않으려면 [Device Guard 사용 안 함](#disable-devguard)을 참조하세요.

- **바이러스 백신 소프트웨어** &ndash; 하드웨어 지원 가상화(예: Avast)를 사용하는 바이러스 백신 소프트웨어를 실행 중인 경우 이 소프트웨어를 사용하지 않게 설정하거나 제거하고, 다시 부팅하고, Android 에뮬레이터를 다시 시도합니다.


#### <a name="incorrect-bios-settings"></a>잘못된 BIOS 설정

Windows PC에서 HAXM을 사용할 경우 가상화 기술(인텔 VT-x)이 BIOS에서 사용하도록 설정되어야 HAXM이 작동합니다. VT-x가 비활성화되어 있으면 Android Emulator를 시작할 때 다음과 유사한 오류가 표시됩니다.

**이 컴퓨터가 HAXM의 요구 사항을 충족하지만 인텔 가상화 기술(VT-x)이 켜져 있지 않습니다.**

이 오류를 해결하려면 컴퓨터를 BIOS로 부팅하고 VT-x와 SLAT(Second-Level Address Translation)를 모두 사용하도록 설정한 다음, 컴퓨터를 다시 Windows로 시작합니다.

<a name="disable-hyperv" />

#### <a name="disabling-hyper-v"></a>Hyper-V 비활성화

**Windows 10 2018년 4월 업데이트(빌드 1803)** 및 Hyper-V가 활성화되기 전에 Windows의 버전을 사용 중인 경우 Hyper-V를 비활성화하고 컴퓨터를 다시 부팅하여 HAXM을 설치하고 사용해야 합니다. **Windows 10 2018년 4월 업데이트(빌드 1803)** 이상을 사용하는 경우 Android Emulator 버전 27.2.7 이상은 하드웨어 가속에 HAXM 대신 Hyper-V를 사용할 수 있으므로 Hyper-V를 비활성화할 필요가 없습니다.

다음 단계를 따라 제어판에서 Hyper-V를 비활성화할 수 있습니다.

1. Windows 검색 상자에 **Windows 기능**을 입력하고 검색 결과에서 **Windows 기능 설정 또는 해제**를 선택합니다.

2. **Hyper-V**의 선택을 취소합니다.

    ![Windows 기능 대화 상자에서 Hyper-V 비활성화](troubleshooting-images/win/03-uncheck-hyper-v.png)

3. 컴퓨터를 다시 시작합니다.

또는 다음 PowerShell 명령을 사용하여 Hyper-V 하이퍼바이저를 비활성화할 수 있습니다.

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM 및 Microsoft Hyper-V는 동시에 활성 상태가 될 수 없습니다. 아쉽게도 컴퓨터를 다시 시작하지 않고 Hyper-V와 HAXM 간에 전환하는 방법은 없습니다. 

Device Guard 및 Credential Guard가 활성화된 경우 위의 단계를 수행해도 Hyper-V가 비활성화되지 않는 경우가 있습니다 Hyper-V를 비활성화할 수 없는 경우(또는 비활성화된 것 같지만 HAXM 설치는 여전히 실패하는 경우) 다음 섹션의 단계를 따라 Device Guard 및 Credential Guard를 비활성화하세요.

<a name="disable-devguard" />

#### <a name="disabling-device-guard"></a>Device Guard 비활성화

Device Guard 및 Credential Guard는 Windows 컴퓨터에서 Hyper-V가 비활성화되지 못하게 방지할 수 있습니다. 이러한 상황은 소유 조직에서 구성하고 제어하는 도메인 가입 머신에 종종 문제가 됩니다. Windows 10에서 다음 단계를 따라 **Device Guard**가 실행 중인지 확인하세요.

1. Windows 검색 상자에 **시스템 정보**를 입력하고 검색 결과에서 **시스템 정보**를 선택합니다.

2. **시스템 요약**에서 **Device Guard 가상화 기반 보안**이 존재하고 **실행 중** 상태인지 확인합니다.

   [![장치 가드가 존재하고 실행 중임](troubleshooting-images/win/04-device-guard-sml.png)](troubleshooting-images/win/04-device-guard.png#lightbox)

Device Guard가 활성화된 경우 다음 단계를 따라 비활성화합니다.

1. 이전 섹션에 설명된 대로 (**Windows 기능 사용/사용 안 함**에서) **Hyper-V**가 비활성화되었는지 확인합니다.

2. Windows 검색 상자에 **gpedit**를 입력하고 **그룹 정책 편집** 검색 결과를 선택합니다. 다음 단계에서는 **로컬 그룹 정책 편집기**를 시작합니다.

3. **로컬 그룹 정책 편집기**에서 **컴퓨터 구성 > 관리 템플릿 > 시스템 > Device Guard**로 이동합니다.

   [![로컬 그룹 정책 편집기의 Device Guard](troubleshooting-images/win/05-group-policy-editor-sml.png)](troubleshooting-images/win/05-group-policy-editor.png#lightbox)

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

7. 컴퓨터를 다시 시작합니다. 부팅 화면에서는 다음 메시지와 유사한 프롬프트가 나타납니다.

   **Credential Guard를 비활성화하시겠습니까?**

   메시지가 나타나면 표시된 키를 눌러 Credential Guard를 비활성화합니다.

8. 컴퓨터가 다시 부팅된 후 (이전 단계에 설명된 대로) Hyper-V가 비활성화되었는지 다시 확인하세요.

Hyper-V가 아직 비활성화되지 않은 경우 도메인 가입 컴퓨터의 정책으로 인해 Device Guard 또는 Credential Guard가 비활성화되지 않을 수 있습니다. 이 경우 도메인 관리자에게 Credential Guard의 옵트아웃할 수 있도록 예외를 요청할 수 있습니다. 또는 HAXM을 사용해야 하는 경우 도메인에 조인되지 않은 컴퓨터를 사용할 수 있습니다.


## <a name="additional-troubleshooting-tips"></a>추가 문제 해결 팁

다음 제안 사항은 Android 에뮬레이터 문제를 진단하는 데 유용한 경우가 많습니다.

### <a name="starting-the-emulator-from-the-command-line"></a>명령줄에서 에뮬레이터 시작

에뮬레이터가 아직 실행되고 있지 않으면 명령줄(Visual Studio 내에서가 아니라)에서 시작하여 해당 출력을 볼 수 있습니다. 일반적으로 Android 에뮬레이터 AVD 이미지는 다음 위치에 저장됩니다(*사용자 이름*을 Windows 사용자 이름으로 바꿈).

**C:\\Users\\*username*\\.android\\avd**

AVD의 폴더 이름을 전달하여 이 위치에서 AVD 이미지를 사용하여 에뮬레이터를 시작할 수 있습니다. 예를 들어 이 명령은 **Pixel_API_27**이라는 AVD를 시작합니다.

```cmd
"C:\Program Files (x86)\Android\android-sdk\emulator\emulator.exe" -partition-size 512 -no-boot-anim -verbose -feature WindowsHypervisorPlatform -avd Pixel_API_27 -prop monodroid.avdname=Pixel_API_27
```

이 예제에서는 Android SDK가 **C:\\프로그램 파일(x86)\\Android\\android-sdk**의 기본 위치에 설치되어 있다고 가정합니다. 그렇지 않은 경우 컴퓨터에서 Android SDK의 위치에 대한 위의 경로를 수정합니다.

이 명령을 실행하면 에뮬레이터가 시작하는 동안 여러 줄의 출력이 생성됩니다. 특히 하드웨어 가속이 활성화되어 있고 제대로 작동하는 경우 다음 예제와 같은 줄이 인쇄됩니다(이 예제에서는 HAXM이 하드웨어 가속에 사용됨).

```cmd
emulator: CPU Acceleration: working
emulator: CPU Acceleration status: HAXM version 6.2.1 (4) is installed and usable.
```

### <a name="viewing-device-manager-logs"></a>Device Manager 로그 보기

종종 Device Manager 로그를 확인하여 에뮬레이터 문제를 진단할 수 있습니다. 이러한 로그는 다음 위치에 기록됩니다.

**C:\\Users\\*username*\\AppData\\Roaming\\XamarinDeviceManager**

메모장과 같은 텍스트 편집기를 사용하여 각 **DeviceManager.log** 파일을 볼 수 있습니다. 다음 예제 로그 항목은 컴퓨터에서 HAXM을 찾을 수 없음을 나타냅니다.

```cmd
Component Intel x86 Emulator Accelerator (HAXM installer) r6.2.1 [Extra: (Intel Corporation)] not present on the system
```



::: zone-end
::: zone pivot="macos"

## <a name="deployment-issues-on-macos"></a>macOS에서 배포 문제

앱을 배포할 때 에뮬레이터에서 일부 오류 메시지가 표시될 수 있습니다. 가장 일반적인 오류 및 해결 방법은 아래에 설명되어 있습니다.

### <a name="deployment-errors"></a>배포 오류

에뮬레이터에 APK 설치 실패 또는 Android Debug Bridge(**adb**) 실행 실패에 대한 오류가 표시되면 Android SDK가 에뮬레이터에 연결할 수 있는지 확인합니다. 연결을 확인하려면 다음 단계를 수행합니다.

1. **Android Device Manager**에서 에뮬레이터를 시작합니다(가상 장치를 선택하고 **시작** 클릭).

2. 명령 프롬프트를 열고 **adb**가 설치된 폴더로 이동합니다. Android SDK가 기본 위치에 설치된 경우 **adb**는 **~/Library/Developer/Xamarin/android-sdk-macosx/platform-tools/adb**에 있습니다. 그렇지 않은 경우, 컴퓨터에서 Android SDK의 위치에 대한 이 경로를 수정합니다.

3. 다음 명령을 입력합니다.

   ```shell
   adb devices
   ```

4. 에뮬레이터를 Android SDK에서 액세스할 수 있는 경우 에뮬레이터가 연결 디바이스 목록에 나타나야 합니다. 예:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. 에뮬레이터가 이 목록에 나타나지 않는 경우 **Android SDK Manager**를 시작하고 모든 업데이트를 적용한 다음, 다시 에뮬레이터를 시작해 봅니다.


### <a name="mmio-access-error"></a>MMIO 액세스 오류

**MMIO 액세스 오류가 발생 했음**이 표시되면 에뮬레이터를 다시 시작하세요.

<a name="gps-mac" />

## <a name="missing-google-play-services"></a>Google Play 서비스 누락

에뮬레이터에서 실행 중인 가상 디바이스에 Google Play 서비스 또는 Google Play 스토어가 설치되어 있지 않은 경우, 이 조건은 일반적으로 패키지를 포함하지 않고 가상 디바이스를 만드는 경우에 발생합니다. 가상 디바이스를 만들 때([Android Device Manager를 사용하여 가상 디바이스 관리](~/android/get-started/installation/android-emulator/device-manager.md) 참조) 다음 중 하나 또는 둘 다를 선택해야 합니다.

- **Google API** &ndash;에는 가상 장치에 Google Play 서비스가 포함되어 있습니다.
- **Google Play 스토어** &ndash;에는 가상 장치에 Google Play 스토어가 포함되어 있습니다.

예를 들어 이 가상 디바이스에는 Google Play 서비스 및 Google Play 스토어가 포함됩니다.

[![Google Play 서비스 및 Google Play 스토어가 활성화된 예제 AVD](troubleshooting-images/mac/01-google-play-services-m75-sml.png)](troubleshooting-images/mac/01-google-play-services-m75.png#lightbox)

> [!NOTE]
> Google Play 스토어 이미지는 픽셀, 픽셀 2, Nexus 5 및 Nexus 5X와 같은 몇 가지 기본 디바이스 유형에만 사용할 수 있습니다.


<a name="perf-mac" />

## <a name="performance-issues"></a>성능 문제

성능 문제는 일반적으로 다음 문제 중 하나로 인해 발생합니다.

- 에뮬레이터가 하드웨어 가속 없이 실행 중입니다.

- 에뮬레이터에서 실행 중인 가상 디바이스가 x86 기반 시스템 이미지를 사용하지 않습니다.

다음 섹션에서는 이러한 시나리오를 자세히 설명합니다.

### <a name="hardware-acceleration-is-not-enabled"></a>하드웨어 가속을 사용할 수 없습니다.

하드웨어 가속을 사용하지 않으면 Android 에뮬레이터에 앱을 배포할 때 **디바이스가 가속 없이 실행됨**과 같은 메시지가 포함된 대화 상자가 팝업될 수 있습니다. 컴퓨터에서 하드웨어 가속이 활성화되어 있는지(또는 가속을 제공하는 기술을 알고 싶다면) 확실하지 않은 경우, 하드웨어 가속을 확인하고 활성화할 수 있는 단계에 대한 아래의 [하드웨어 가속 문제](#accel-issues-mac)를 참조하세요.


### <a name="acceleration-is-enabled-but-the-emulator-runs-too-slowly"></a>가속을 사용할 수 있지만 에뮬레이터가 너무 느리게 실행됩니다. 

이 문제의 일반적인 원인은 가상 디바이스에서 x86 기반 이미지를 사용하지 않기 때문입니다. 가상 디바이스를 만들 때([Android Device Manager를 사용하여 가상 디바이스 관리](~/android/get-started/installation/android-emulator/device-manager.md) 참조) x86 기반 시스템 이미지를 선택해야 합니다.

[![가상 장치용 x86 시스템 이미지 선택](troubleshooting-images/mac/02-x86-virtual-device-m75-sml.png)](troubleshooting-images/mac/02-x86-virtual-device-m75.png#lightbox)

<a name="accel-issues-mac" />

## <a name="hardware-acceleration-issues"></a>하드웨어 가속 문제

에뮬레이터의 하드웨어 가속을 위해 하이퍼바이저 프레임워크 또는 HAXM을 사용하든 상관없이 설치 문제 또는 오래된 버전의 macOS로 인해 발생하는 문제에 직면할 수 있습니다. 다음 섹션에서는 이 문제를 해결하는 데 도움을 줄 수 있습니다.

<a name="hypervisor-issues" />

### <a name="hypervisor-framework-issues"></a>하이퍼바이저 프레임워크 문제

최신 Mac에서 macOS 10.10 이상을 사용하는 경우 Android 에뮬레이터는 하드웨어 가속을 위해 하이퍼바이저 프레임워크를 자동으로 사용합니다. 그러나 10.10 이전 버전의 macOS를 실행하는 일부 이전 Mac 또는 Mac은 하이퍼바이저 프레임워크 지원을 제공하지 않을 수 있습니다.

Mac에서 하이퍼바이저 프레임워크를 지원하는지 여부를 확인하려면 터미널을 열고 다음 명령을 입력합니다.

```bash
sysctl kern.hv_support
```

Mac에서 하이퍼바이저 프레임워크를 지원하는 경우 위의 명령은 다음 결과를 반환합니다.

```bash
kern.hv_support: 1
```

Mac에서 하이퍼바이저 프레임워크를 사용할 수 없는 경우 [HAXM을 사용하여 가속화](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vsmac#haxm-mac)의 단계를 수행하여 가속화에 대해 HAXM을 대신 사용할 수 있습니다.


### <a name="haxm-issues"></a>HAXM 문제

Android Emulator가 제대로 시작되지 않는 경우 이 문제는 HAXM 관련 문제로 인해 자주 발생합니다. HAXM 문제는 다른 가상화 기술과의 충돌, 잘못된 설정 또는 만료된 HAXM 드라이버 때문인 경우가 많습니다. [HAXM 설치](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vsmac#install-haxm-mac)에서 설명한 단계를 통해 HAXM 드라이버를 다시 설치해 봅니다. 


## <a name="additional-troubleshooting-tips"></a>추가 문제 해결 팁

다음 제안 사항은 Android 에뮬레이터 문제를 진단하는 데 유용한 경우가 많습니다.

### <a name="starting-the-emulator-from-the-command-line"></a>명령줄에서 에뮬레이터 시작

에뮬레이터가 아직 실행되고 있지 않으면 명령줄(Mac용 Visual Studio 내에서가 아니라)에서 시작하여 해당 출력을 볼 수 있습니다. 일반적으로 Android 에뮬레이터 AVD 이미지는 다음 위치에 저장됩니다.

**~/.android/avd**

AVD의 폴더 이름을 전달하여 이 위치에서 AVD 이미지를 사용하여 에뮬레이터를 시작할 수 있습니다. 예를 들어 이 명령은 **Pixel_2_API_28**이라는 AVD를 시작합니다.

```cmd
~/Library/Developer/Xamarin/android-sdk-macosx/emulator/emulator -partition-size 512 -no-boot-anim -verbose -feature WindowsHypervisorPlatform -avd Pixel_2_API_28 -prop monodroid.avdname=Pixel_2_API_28
```

Android SDK가 기본 위치에 설치된 경우 에뮬레이터는 **~/Library/Developer/Xamarin/android-sdk-macosx/emulator** 디렉터리에 있습니다. 그렇지 않은 경우, Mac에서 Android SDK의 위치에 대한 이 경로를 수정합니다.

이 명령을 실행하면 에뮬레이터가 시작하는 동안 여러 줄의 출력이 생성됩니다. 특히 하드웨어 가속이 활성화되어 있고 제대로 작동하는 경우 다음 예제와 같은 줄이 인쇄됩니다(이 예제에서는 하이퍼바이저 프레임워크가 하드웨어 가속에 사용됨).

```cmd
emulator: CPU Acceleration: working
emulator: CPU Acceleration status: Hypervisor.Framework OS X Version 10.13
```

### <a name="viewing-device-manager-logs"></a>Device Manager 로그 보기

종종 Device Manager 로그를 확인하여 에뮬레이터 문제를 진단할 수 있습니다. 이러한 로그는 다음 위치에 기록됩니다.

**~/Library/Logs/XamarinDeviceManager**

각 **Android Devices.log** 파일을 두 번 클릭하여 콘솔 앱에서 열어 볼 수 있습니다. 다음 예제 로그 항목은 HAXM을 찾을 수 없음을 나타냅니다.

```cmd
Component Intel x86 Emulator Accelerator (HAXM installer) r6.2.1 [Extra: (Intel Corporation)] not present on the system
```

::: zone-end