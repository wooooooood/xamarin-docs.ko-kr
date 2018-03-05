---
title: "Android SDK 에뮬레이터 문제 해결"
description: "Android SDK 에뮬레이터 문제를 식별하여 해결하는 방법"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B05C3C5-E1F6-47A9-B098-C31E630194F6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 51cc7a4700e8cb3ece556b0ada841d70d5f2bb8b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="android-sdk-emulator-troubleshooting"></a>Android SDK 에뮬레이터 문제 해결

이 아티클에서는 Android SDK 에뮬레이터의 가장 일반적인 경고 메시지 및 문제(및 해결 방법)를 설명합니다.
 
<a name="perfwarn" />

## <a name="performance-warnings"></a>성능 경고

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio 2017 15.4 버전부터 처음 앱을 ndroid SDK 에뮬레이터에 배포하면 성능 경고 대화 상자가 표시될 수 있습니다. 이러한 경고 대화 상자는 아래에 설명되어 있습니다.

### <a name="computer-does-not-contain-an-intel-procesor"></a>컴퓨터에 인텔 프로세서 없음

![컴퓨터에 인텔 프로세서 없음](troubleshooting-images/01-no-intel-processor.png)

이 대화 상자가 나타나면 컴퓨터에 Android SDK 에뮬레이터 가속에 필요한 인텔 프로세서가 없는 것입니다. 컴퓨터에 인텔 프로세서가 없다면 개발에 물리적 Android 장치를 사용하는 것이 좋습니다.

### <a name="hyper-v-is-installed-or-active"></a>Hyper-V 설치 또는 활성 상태

![Hyper-V 설치 또는 활성 상태](troubleshooting-images/02-hyper-v-active.png)

이 대화 상자가 표시되면 Hyper-V가 설치되었거나 활성 상태라, 사용하지 않게 설정해야 합니다. [Hyper-V를 사용하지 않도록 설정](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv)에서 이 문제 해결 방법을 설명합니다. 

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


**STATE**가 **RUNNING**으로 설정되지 않은 경우 [인텔 Hardware Accelerated Execution Manager 사용 방법](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator)을 참조하여 문제를 해결합니다.


### <a name="other-failures"></a>기타 실패

![기타 실패](troubleshooting-images/05-other-failure.png)

이 대화 상자는 컴퓨터에 인텔 프로세서가 있고 Hyper-V가 사용하지 않게 설정되었으며 인텔 HAXM이 설치되었으나 에뮬레이터가 알 수 없는 이유로 시작에 실패한 경우에 표시됩니다.
이 오류를 해결하려면 [인텔 Hardware Accelerated Execution Manager 사용 방법](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator)을 참조하세요.

### <a name="disabling-performance-warnings"></a>성능 경고 사용 안 함

성능 경고를 표시하지 않기 위해 사용하지 않게 설정할 수 있습니다. Visual Studio에서 **도구 > 옵션 > Xamarin > Android 설정**을 클릭하고 **AVD 가속이 지원되지 않는 경우 경고(HAXM)** 옵션을 사용하지 않게 설정합니다.

[![AVD 가속 경고를 사용하지 않도록 설정](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac 빌드 7.2(빌드 559)부터 처음 앱을 ndroid SDK 에뮬레이터에 배포하면 성능 경고 대화 상자가 표시될 수 있습니다. 이러한 경고 대화 상자는 아래에 설명되어 있습니다.

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

<a name="solutions" />

## <a name="solutions-to-common-problems"></a>일반적인 문제에 대한 해결 방법

여러 일반적인 Android SDK 에뮬레이터 문제는 추가 소프트웨어를 설치하거나 컴퓨터 구성을 변경하여 해결할 수 있습니다. 다음 섹션에서는 이러한 문제를 설명하고 해결 방법을 제시합니다.

<a name="deployment" />

### <a name="deployment-issues"></a>배포 문제

에뮬레이터에 APK 설치 실패 또는 Android Debug Bridge(**adb**) 실행 실패와 관련한 오류가 발생할 경우 Android SDK가 에뮬레이터에 연결할 수 있는지 확인합니다. 이렇게 하려면 다음 단계를 따릅니다.

1. **AVD(Android Virtual Device) Manager**에서 에뮬레이터를 시작합니다(가상 장치를 선택하고 **시작** 클릭).

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


<a name="haxm-issues" />

### <a name="haxm-issues"></a>HAXM 문제

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Android SDK 에뮬레이터가 제대로 시작되지 않으면 보통은 HAXM의 문제입니다. HAXM 문제는 다른 가상화 기술과의 충돌, 잘못된 설정 또는 만료된 HAXM 드라이버 때문인 경우가 많습니다.

<a name="virt-conflicts" />

#### <a name="haxm-virtualization-conflicts"></a>HAXM 가상화 충돌

HAXM은 Hyper-v, Windows Device Guard 및 일부 바이러스 백신 소프트웨어 등을 사용하는 다른 기술과 충돌할 수 있습니다.

- **Hyper-V** &ndash; Hyper-V를 사용하는 Windows를 사용할 경우 [Hyper-V 사용 안 함](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv)의 단계를 따릅니다.

- **Device Guard** &ndash;Device Guard 및 Credential Guard는 Windows 컴퓨터에서 Hyper-V가 비활성화되지 못하게 방지할 수 있습니다. Device Guard 및 Credential Guard를 사용하지 않으려면 [Device Guard 사용 안 함](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-devguard)을 참조하세요.

- **바이러스 백신 소프트웨어** &ndash; 하드웨어 지원 가상화(예: Avast)를 사용하는 바이러스 백신 소프트웨어를 실행 중인 경우 이 소프트웨어를 사용하지 않게 설정하거나 제거한 다음 다시 부팅하고 Android SDK 에뮬레이터를 다시 시도합니다.

<a name="bios" />

#### <a name="incorrect-bios-settings"></a>잘못된 BIOS 설정

Windows PC에서 HAXM을 사용할 경우 가상화 기술(인텔 VT-x)이 BIOS에서 사용하도록 설정되어야 HAXM이 작동합니다. VT-x를 사용하지 않게 설정한 경우 Android SDK 에뮬레이터를 시작할 때 다음과 비슷한 오류가 표시됩니다.

**이 컴퓨터가 HAXM의 요구 사항을 충족하지만 인텔 가상화 기술(VT-x)이 켜져 있지 않습니다.**

이 오류를 해결하려면 컴퓨터를 BIOS로 부팅하고 VT-x와 SLAT(Second Level Address Translation)를 모두 사용하도록 설정한 다음 컴퓨터를 다시 Windows로 시작합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Android SDK 에뮬레이터가 제대로 시작되지 않으면 보통은 HAXM의 문제입니다. HAXM 문제는 다른 가상화 기술과의 충돌, 잘못된 설정 또는 만료된 HAXM 드라이버 때문인 경우가 많습니다. [HAXM 설치](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm)에서 설명한 단계를 통해 HAXM 드라이버를 다시 설치해 봅니다. 

-----
