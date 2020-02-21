---
title: 에뮬레이터 성능에 대한 하드웨어 가속(Hyper-V & HAXM)
description: 이 아티클에서는 컴퓨터의 하드웨어 가속 기능을 사용하여 Android Emulator 성능을 최대화하는 방법을 설명합니다.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/27/2018
ms.openlocfilehash: a724a21dfffead307ca3d65d5ff134cf2d7c90db
ms.sourcegitcommit: 24883be72e485e5311dd0eb91f9a22f78eeec11a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2020
ms.locfileid: "77374038"
---
# <a name="hardware-acceleration-for-emulator-performance-hyper-v--haxm"></a>에뮬레이터 성능에 대한 하드웨어 가속(Hyper-V & HAXM)

_이 아티클에서는 컴퓨터의 하드웨어 가속 기능을 사용하여 Android Emulator 성능을 최대화하는 방법을 설명합니다._

Visual Studio를 통해 Android 디바이스가 지원되지 않거나 실용적이지 않은 경우 Android 에뮬레이터를 사용하여 개발자가 Xamarin.Android 애플리케이션을 쉽게 테스트하고 디버그할 수 있습니다.
그러나 Android 에뮬레이터는 실행되는 컴퓨터에서 하드웨어 가속을 사용할 수 없는 경우 너무 늦게 실행됩니다. 컴퓨터의 가상화 기능과 함께 특별한 x86 가상 디바이스 이미지를 사용하여 Android 에뮬레이터의 성능을 상당히 개선할 수 있습니다.

| 시나리오    | HAXM        | WHPX       | Hypervisor.Framework |
| ----------- | ----------- | -----------| ----------- |
| Intel 프로세서가 있습니다. | X | X | X |
| AMD 프로세서가 있습니다.   |   | X |   |
| Hyper-V를 지원하려고 합니다. |   | X |   |
| 중첩된 가상화를 지원하려고 합니다. |   | 제한됨 |   |
| Docker와 같은 기술을 사용하려고 합니다.  |   | X | X |

::: zone pivot="windows"

## <a name="accelerating-android-emulators-on-windows"></a>Windows에서 Android 에뮬레이터 가속화

다음 가상화 기술은 Android 에뮬레이터를 가속화하는 데 사용할 수 있습니다.

1. **Microsoft의 Hyper-V 및 Windows 하이퍼바이저 플랫폼(WHPX)**
   [Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/)는 물리적 호스트 컴퓨터에서 가상화된 컴퓨터 시스템을 실행할 수 있게 해 주는 Windows의 가상화 기능입니다.

2. **Intel의 HAXM(Hardware Accelerated Execution Manager)**
   HAXM은 Intel CPU를 실행하는 컴퓨터에 대한 가상화 엔진입니다.

Windows에서 최상의 경험을 위해서는 WHPX를 사용하여 Android 에뮬레이터를 가속하는 것이 좋습니다. 컴퓨터에서 WHPX를 사용할 수 없는 경우 HAXM을 사용할 수 있습니다. Android 에뮬레이터는 다음 기준이 충족되는 경우 자동으로 하드웨어 가속을 사용합니다.

- 개발 컴퓨터에서 하드웨어 가속을 사용하고 활성화할 수 있습니다.

- 에뮬레이터가 **x86** 기반 가상 디바이스용으로 생성된 시스템 이미지를 실행 중입니다.

> [!IMPORTANT]
> VM 가속화된 에뮬레이터는 VirtualBox, VMWare 또는 Docker가 호스팅하는 VM과 같은 다른 VM 내에서 실행할 수 없습니다. [시스템 하드웨어에서 직접](https://developer.android.com/studio/run/emulator-acceleration.html#extensions) Android Emulator를 실행해야 합니다.

Android 에뮬레이터를 시작하고 디버깅하는 방법에 대한 자세한 내용은 [Android Emulator에서 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)을 참조하세요.

<a name="hyper-v-win" />

## <a name="accelerating-with-hyper-v"></a>Hyper-V를 사용하여 가속화

Hyper-V를 사용하도록 설정하기 전에 다음 섹션을 읽고 컴퓨터에서 Hyper-V를 지원하는지 확인합니다.

### <a name="verifying-support-for-hyper-v"></a>Hyper-V에 대한 지원 확인

Hyper-V는 Windows 하이퍼바이저 플랫폼에서 실행됩니다. Hyper-V와 함께 Android 에뮬레이터를 사용하려면 컴퓨터가 Windows 하이퍼바이저 플랫폼을 지원하기 위해 다음 조건을 충족해야 합니다.

- 컴퓨터 하드웨어는 다음 요구 사항을 충족해야 합니다.

  - SLAT(Second Level Address Translation)가 포함된 64비트 Intel 또는 AMD Ryzen CPU.
  - VM 모니터 모드 확장(Intel CPU의 VT-c)에 대한 CPU 지원.
  - 최소 4GB 메모리.

- 컴퓨터의 BIOS에서 다음 항목을 활성화해야 합니다.

  - 가상화 기술(마더보드 제조업체에 따라 다른 레이블이 있을 수 있음).
  - 하드웨어 강제 데이터 실행 방지.

- Windows 10 2018년 4월 업데이트(빌드 1803) 이상으로 컴퓨터를 업데이트해야 합니다. 다음 단계를 수행하여 Windows 버전이 최신인지 확인할 수 있습니다.

  1. Windows 검색 상자에 **정보**를 입력합니다.
  2. 검색 결과에서 **사용자 PC 정보**를 선택합니다.
  3. **정보** 대화 상자에서 **Windows 사양** 섹션으로 아래로 스크롤합니다.
  4. **버전** 적어도 1803인지 확인합니다.

      [![Windows 사양](hardware-acceleration-images/win/01-about-windows-w10-sml.png)](hardware-acceleration-images/win/01-about-windows-w10.png#lightbox)

컴퓨터 하드웨어 및 소프트웨어가 Hyper-V와 호환되는지 확인하려면 명령 프롬프트를 열고 다음 명령을 입력합니다.

```cmd
systeminfo
```

나열된 모든 Hyper-V 요구 사항의 값이 **예**이면 컴퓨터에서 Hyper-V를 지원할 수 있습니다. 예를 들어:

[![systeminfo 출력 예제](hardware-acceleration-images/win/02-systeminfo-w158-sml.png)](hardware-acceleration-images/win/02-systeminfo-w158.png#lightbox)

### <a name="enabling-hyper-v-acceleration"></a>Hyper-V 가속 사용

컴퓨터가 위의 조건을 충족하는 경우 다음 단계를 수행하여 Hyper-V로 Android 에뮬레이터를 가속화합니다.

1. Windows 검색 상자에 **Windows 기능**을 입력하고 검색 결과에서 **Windows 기능 설정 또는 해제**를 선택합니다. **Windows 기능** 대화 상자에서 **Hyper-V** 및 **Windows 하이퍼바이저 플랫폼**을 모두 활성화합니다.

    [![Hyper-V 및 Windows 하이퍼바이저 플랫폼 사용](hardware-acceleration-images/win/03-hyper-v-settings-w158-sml.png)](hardware-acceleration-images/win/03-hyper-v-settings-w158.png#lightbox)

   다음과 같이 변경한 후 컴퓨터를 다시 부팅합니다.
   
> [!IMPORTANT]
>
> Windows 10 October 2018 업데이트(RS5) 이상에서는 Hyper-V만 사용하도록 설정하면 됩니다. Hyper-V가 자동으로 WHPX(Windows Hypervisor Platform)를 사용하기 때문입니다.

2. **[Visual Studio 15.8 이상](https://visualstudio.microsoft.com/vs/) 설치**(이 버전의 Visual Studio는 Hyper-V로 Android 에뮬레이터를 실행하기 위한 IDE 지원을 제공합니다).

3. **Android Emulator 패키지 27.2.7 이상을 설치합니다**. 이 패키지를 설치하려면 Visual Studio에서 **도구 > Android > Android SDK Manager**로 이동합니다. **도구** 탭을 선택하고 Android 에뮬레이터 버전이 적어도 27.2.7인지 확인합니다. 또한 Android SDK Tools 버전이 26.1.1 이상인지 확인합니다.

    [![Android SDK 및 도구 대화 상자](hardware-acceleration-images/win/04-sdk-manager-w158-sml.png)](hardware-acceleration-images/win/04-sdk-manager-w158.png#lightbox)

가상 디바이스를 만들 때([Android Device Manager를 사용하여 가상 디바이스 관리](~/android/get-started/installation/android-emulator/device-manager.md) 참조) **x86** 기반 시스템 이미지를 선택해야 합니다. ARM 기반 시스템 이미지를 사용하면 가상 디바이스가 가속화되지 않고 느리게 실행됩니다.

## <a name="accelerating-with-haxm"></a>HAXM을 통한 가속화

컴퓨터에서 Hyper-V를 지원하지 않는 경우 HAXM을 사용하여 Android 에뮬레이터를 가속화합니다. HAXM을 사용하려면 [Device Guard를 사용하지 않도록 설정](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vswin#disable-devguard)해야 합니다.

### <a name="verifying-haxm-support"></a>HAXM 지원 확인

하드웨어에서 HAXM을 지원하는지 확인하려면 [내 프로세서가 Intel 가상화 기술을 지원하나요?](https://www.intel.com/content/www/us/en/support/processors/000005486.html)의 단계를 따릅니다.
하드웨어에서 HAXM을 지원하는 경우 다음 단계를 수행하여 HAXM이 이미 설치되어 있는지 확인할 수 있습니다.

1. 명령 프롬프트 창을 열고 다음 명령을 입력합니다.

    ```cmd
    sc query intelhaxm
    ```

2. 출력을 검사하여 HAXM 프로세스가 실행 중인지 확인합니다. 그럴 경우 `intelhaxm` 상태가 `RUNNING`으로 나열된 출력이 표시되어야 합니다. 예를 들어:

    ![HAXM을 사용할 수 하는 경우 sc 쿼리 명령의 출력](hardware-acceleration-images/win/05-sc_query-w158.png)

   `STATE`가 `RUNNING`으로 설정되어 있지 않으면 HAXM이 설치되지 않습니다.

컴퓨터에서 HAXM을 지원할 수 있지만 HAXM이 설치되지 않은 경우 다음 섹션의 단계에 따라 HAXM을 설치합니다.

<a name="install-haxm-win" />

### <a name="installing-haxm"></a>HAXM 설치

Windows용 HAXM 설치 패키지는 [Intel Hardware Accelerated Execution Manager](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager) 페이지에서 사용할 수 있습니다. HAXM을 다운로드하여 설치하려면 다음 단계를 수행합니다.

1. Intel 웹 사이트에서 최신 Windows용 [HAXM 가상화 엔진](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager/) 설치 관리자를 다운로드합니다. Intel 웹 사이트에서 직접 HAXM 설치 프로그램을 다운로드할 경우의 장점은 최신 버전을 사용할 수 있다는 점입니다.

2. **intelhaxm-android.exe**를 실행하여 HAXM 설치 관리자를 시작합니다. 설치 관리자 대화 상자에서 기본값을 적용합니다.

   ![Intel Hardware Accelerated Execution Manager 설치 창](hardware-acceleration-images/win/06-haxm-installer.png)

가상 디바이스를 만들 때([Android Device Manager를 사용하여 가상 디바이스 관리](~/android/get-started/installation/android-emulator/device-manager.md) 참조) **x86** 기반 시스템 이미지를 선택해야 합니다. ARM 기반 시스템 이미지를 사용하면 가상 디바이스가 가속화되지 않고 느리게 실행됩니다.

## <a name="troubleshooting"></a>문제 해결

하드웨어 가속 문제 해결에 대한 도움말은 Android 에뮬레이터 [문제 해결](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vswin#accel-issues-win) 가이드를 참조하세요.

::: zone-end
::: zone pivot="macos"

## <a name="accelerating-android-emulators-on-macos"></a>macOS에서 Android 에뮬레이터 가속화

다음 가상화 기술은 Android 에뮬레이터를 가속화하는 데 사용할 수 있습니다.

1. **Apple의 하이퍼바이저 프레임워크**.
   [하이퍼바이저](https://developer.apple.com/documentation/hypervisor)는 Mac에서 가상 머신을 실행할 수 있도록 하는 macOS 10.10 이상의 기능입니다.

2. **Intel의 HAXM(Hardware Accelerated Execution Manager)**
   [HAXM](https://software.intel.com/articles/intel-hardware-accelerated-execution-manager-intel-haxm)은 Intel CPU를 실행하는 컴퓨터에 대한 가상화 엔진입니다.

하이퍼바이저 프레임워크를 사용하여 Android 에뮬레이터를 가속화하는 것이 좋습니다. Mac에서 하이퍼바이저 프레임워크를 사용할 수 없는 경우 HAXM을 사용할 수 있습니다. Android 에뮬레이터는 다음 기준이 충족되는 경우 자동으로 하드웨어 가속을 사용합니다.

- 개발 컴퓨터에서 하드웨어 가속을 사용하고 활성화할 수 있습니다.

- 에뮬레이터가 **x86** 기반 가상 디바이스용으로 생성된 시스템 이미지를 실행 중입니다.

> [!IMPORTANT]
>
> VM 가속화된 에뮬레이터는 VirtualBox, VMWare 또는 Docker가 호스팅하는 VM과 같은 다른 VM 내에서 실행할 수 없습니다. [시스템 하드웨어에서 직접](https://developer.android.com/studio/run/emulator-acceleration.html#extensions) Android Emulator를 실행해야 합니다.

Android 에뮬레이터를 시작하고 디버깅하는 방법에 대한 자세한 내용은 [Android Emulator에서 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)을 참조하세요.

<a name="hypervisor" />

## <a name="accelerating-with-the-hypervisor-framework"></a>하이퍼바이저 프레임워크를 통한 가속화

하이퍼바이저 프레임워크와 함께 Android 에뮬레이터를 사용하려면 Mac이 다음 조건을 충족해야 합니다.

- Mac은 macOS 10.10 이상을 실행해야 합니다.

- Mac의 CPU가 하이퍼바이저 프레임워크를 지원할 수 있어야 합니다.

Mac이 이러한 조건을 충족하는 경우 Android 에뮬레이터는 가속을 위해 자동으로 하이퍼바이저 프레임워크를 사용합니다. 하이퍼바이저 프레임워크가 Mac에서 지원되는지 확실하지 않은 경우 Mac이 하이퍼바이저를 지원하는지 확인하는 방법에 대한 [문제 해결](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vsmac#hypervisor-issues) 가이드를 참조하세요.

하이퍼바이저 프레임워크가 Mac에서 지원되지 않는 경우 HAXM을 사용하여 Android 에뮬레이터를 가속화할 수 있습니다(다음 설명 참조).

<a name="haxm-mac" />

## <a name="accelerating-with-haxm"></a>HAXM을 통한 가속화

Mac에서 하이퍼바이저 프레임워크를 지원하지 않는 경우(또는 macOS 10.10 이전 버전을 사용하는 경우) **Intel의 Hardware Accelerated Execution Manager**([HAXM](https://software.intel.com/articles/intel-hardware-accelerated-execution-manager-intel-haxm))를 사용하여 Android 에뮬레이터 속도를 높일 수 있습니다.

HAXM과 함께 Android 에뮬레이터를 처음으로 사용하기 전에 HAXM이 설치되어 있고 사용할 Android 에뮬레이터에서 지원되는지 확인하는 것이 좋습니다.

### <a name="verifying-haxm-support"></a>HAXM 지원 확인

다음 단계를 수행하여 HAXM이 이미 설치되어 있는지 확인할 수 있습니다.

1. 터미널을 열고 다음 명령을 입력합니다.

    ```bash
    ~/Library/Developer/Xamarin/android-sdk-macosx/tools/emulator -accel-check
    ```

   이 명령은 Android SDK가 **~/Library/Developer/Xamarin/android-sdk-macosx**의 기본 위치에 설치되어 있다고 가정합니다. 그렇지 않은 경우 Mac에서 Android SDK의 위치에 대한 위의 경로를 수정합니다.

2. HAXM이 설치된 경우 위의 명령은 다음 결과와 유사한 메시지를 반환합니다.

    ```bash
    HAXM version 7.2.0 (3) is installed and usable.
    ```

   HAXM이 설치되지 *않은* 경우 다음 출력과 유사한 메시지가 반환됩니다.

    ```bash
    HAXM is not installed on this machine (/dev/HAX is missing).
    ```

HAXM이 설치되지 않은 경우 다음 섹션의 단계에 따라 HAXM을 설치하세요.

<a name="install-haxm-mac" />

### <a name="installing-haxm"></a>HAXM 설치

macOS용 HAXM 설치 패키지는 [Intel Hardware Accelerated Execution Manager](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager) 페이지에서 사용할 수 있습니다. HAXM을 다운로드하여 설치하려면 다음 단계를 수행합니다.

1. Intel 웹 사이트에서 최신 macOS용 [HAXM 가상화 엔진](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager/) 설치 관리자를 다운로드합니다.

2. HAXM 설치 관리자를 실행합니다. 설치 관리자 대화 상자에서 기본값을 적용합니다.

   [![Intel Hardware Accelerated Execution Manager 설치 창](hardware-acceleration-images/mac/01-haxm-installer-sml.png)](hardware-acceleration-images/mac/01-haxm-installer.png#lightbox)

## <a name="troubleshooting"></a>문제 해결

하드웨어 가속 문제 해결에 대한 도움말은 Android 에뮬레이터 [문제 해결](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vsmac#accel-issues-mac) 가이드를 참조하세요.

::: zone-end

## <a name="related-links"></a>관련 링크

- [Android Emulator에서 앱 실행](https://developer.android.com/studio/run/emulator)
