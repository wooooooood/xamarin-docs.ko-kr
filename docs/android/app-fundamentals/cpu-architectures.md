---
title: CPU 아키텍처
description: Xamarin Android는 32 비트 및 64 비트 장치를 비롯 한 몇 가지 CPU 아키텍처를 지원 합니다. 이 문서에서는 하나 이상의 Android 지원 CPU 아키텍처에 앱을 대상으로 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D4BC889D-9164-49BB-9B7B-F6C4E4E109F1
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2019
ms.openlocfilehash: 59047b8564db6415ea3c47d7dcb72b5d0c66d1dd
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755584"
---
# <a name="cpu-architectures"></a>CPU 아키텍처

_Xamarin Android는 32 비트 및 64 비트 장치를 비롯 한 몇 가지 CPU 아키텍처를 지원 합니다. 이 문서에서는 하나 이상의 Android 지원 CPU 아키텍처에 앱을 대상으로 하는 방법을 설명 합니다._

## <a name="cpu-architectures-overview"></a>CPU 아키텍처 개요

릴리스를 위해 앱을 준비할 때 앱이 지 원하는 플랫폼 CPU 아키텍처를 지정 해야 합니다. 단일 APK가 여러 서로 다른 아키텍처를 지원하는 머신 코드를 포함할 수 잇습니다. 아키텍처 관련 코드의 각 컬렉션은 ABI ( *응용 프로그램 이진 인터페이스* )와 연결 됩니다. 각 ABI는 런타임에이 컴퓨터 코드가 Android와 상호 작용 하는 방법을 정의 합니다.
작동 방식에 대 한 자세한 내용은 참조는 [다중 코어 장치 &amp; Xamarin. Android](~/android/deploy-test/multicore-devices.md)합니다.

## <a name="how-to-specify-supported-architectures"></a>지원 되는 아키텍처를 지정 하는 방법

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

일반적으로 앱이 **릴리스에**대해 구성 된 경우 아키텍처 (또는 아키텍처)를 명시적으로 선택 합니다. 앱이 **디버그**를 사용 하도록 구성 된 경우에는 **Shared Runtime 사용** 및 **빠른 배포 사용** 옵션을 사용할 수 있으며,이 옵션을 사용 하면 명시적 아키텍처를 선택할 수 없습니다.

Visual Studio의 **솔루션 탐색기** 아래에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다. **Android 옵션** 페이지에서 **패키징 속성** 섹션을 확인 하 고 **Shared Runtime 사용** 이 사용 하지 않도록 설정 되어 있는지 확인 합니다 .이 기능을 해제 하면 지원할 abis 명시적으로 선택할 수 있습니다. **고급** 단추를 클릭 하 고 **지원 되는 아키텍처**에서 지원할 아키텍처를 확인 합니다.

[![Armeabi-v7a 및 armeabi-v7a-armeabi-v7a 선택](cpu-architectures-images/vs/01-abi-selections-sml.png)](cpu-architectures-images/vs/01-abi-selections.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

일반적으로 앱이 **릴리스에**대해 구성 된 경우 아키텍처 (또는 아키텍처)를 명시적으로 선택 합니다. 앱이 **디버그**를 사용 하도록 구성 된 경우 **공유 Mono 런타임** 및 **빠른 어셈블리 배포** 사용 옵션을 사용 하도록 설정 하 여 명시적 아키텍처를 선택할 수 없습니다.

Mac용 Visual Studio의 **Solution** pad에서 프로젝트를 찾고 프로젝트 옆의 기어 아이콘을 클릭 한 다음 **옵션**을 선택 합니다. **프로젝트 옵션** 대화 상자에서 **Android 빌드**를 클릭 합니다. **일반** 탭을 클릭 하 고 **공유 Mono 런타임 사용** 이 사용 하지 않도록 설정 되어 있는지 확인 합니다 .이 기능을 해제 하면 지원할 abis 명시적으로 선택할 수 있습니다. **고급** 탭을 클릭 하 고 지원 되는 **abis**에서 지원할 아키텍처를 확인 합니다.

[![Armeabi-v7a 및 armeabi-v7a-armeabi-v7a 선택](cpu-architectures-images/xs/01-abi-selections-sml.png)](cpu-architectures-images/xs/01-abi-selections.png#lightbox)

-----

Xamarin.Android는 다음과 같은 아키텍처를 지원합니다.

- **armeabi-v7a** &ndash; 하나 이상의 ARMv5TE 명령 집합을 지 원하는 ARM 기반 cpu 는 스레드로부터 안전 하지 않으며 다중 CPU 장치에서 사용 하면 안 됩니다.`armeabi`

> [!NOTE]
> [Xamarin. Android 9.2](https://docs.microsoft.com/xamarin/android/release-notes/9/9.2#removal-of-support-for-armeabi-cpu-architecture)는에서 `armeabi` 더 이상 지원 되지 않습니다.

- **armeabi-armeabi-v7a** &ndash; 하드웨어 부동 소수점 작업 및 다중 CPU (SMP) 장치를 사용 하는 ARM 기반 cpu 컴퓨터 코드 `armeabi-v7a` 는 ARMv5 장치에서 실행 되지 않습니다.

- **arm64-arm64-v8a** &ndash; 64 비트 ARMv8 아키텍처를 기반으로 하는 cpu입니다.

- **x86** &ndash; X86 (또는 IA-32) 명령 집합을 지 원하는 cpu. 이 명령 집합은 MMX, SSE, SSE2 및 SSE3 지침을 포함 하 여 Pentium Pro와 동일 합니다.

- **x86_64** 64 비트 x86 ( *x64* 및 *AMD64*라고도 함) 명령 집합을 지 원하는 cpu.

Xamarin Android는 **릴리스** 빌드 `armeabi-v7a` 에 대해 기본적으로로 설정 됩니다. 이 설정은 보다 `armeabi`뛰어난 성능을 제공 합니다. 64 비트 ARM 플랫폼을 대상으로 하는 경우 (예: Nexus 9)을 선택 `arm64-v8a`합니다. X86 장치에 앱을 배포 하는 경우를 선택 `x86`합니다. 대상 x86 장치에서 64 비트 CPU 아키텍처를 사용 하는 경우를 `x86_64`선택 합니다.

## <a name="targeting-multiple-platforms"></a>여러 플랫폼 대상 지정

여러 CPU 아키텍처를 대상으로 하는 경우 두 개 이상의 ABI를 선택할 수 있습니다 (더 큰 APK 파일 크기의 비용). 각 지원 되는 아키텍처에 대 한 별도의 APK를 만들려면 **선택한 ABI 별 패키지 생성 (. apk)** 옵션 ( [패키징 속성 설정](~/android/deploy-test/release-prep/index.md#Set_Packaging_Properties)에서 설명)을 사용할 수 있습니다.

64 비트 장치를 대상으로 **arm64-arm64-v8a** 또는 **x86_64** 를 선택할 필요가 없습니다. 64 비트 하드웨어에서 응용 프로그램을 실행 하는 데는 64 비트 지원이 필요 하지 않습니다. 예를 들어 64 비트 ARM 장치 (예: [Nexus 9](http://www.google.com/nexus/9/))는 용 `armeabi-v7a`으로 구성 된 앱을 실행할 수 있습니다. 64 비트 지원을 사용 하도록 설정 하는 주요 이점은 앱이 더 많은 메모리를 처리할 수 있도록 하는 것입니다.

> [!NOTE]
> 2018년 8월부터 새 앱은 API 레벨 26을 대상으로 해야 하고, 2019년 8월부터 앱은 32비트 버전 외에 [‘64비트 버전을 제공’](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html)해야 합니다.

## <a name="additional-information"></a>추가 정보

경우에 따라 APK의 크기를 줄이거나 앱에 특정 CPU 아키텍처와 관련 된 공유 라이브러리가 있으므로 각 아키텍처에 대해 별도의 APK를 만들어야 할 수도 있습니다.
이 방법에 대 한 자세한 내용은 [ABI 관련 APKs 빌드](~/android/deploy-test/building-apps/abi-specific-apks.md)를 참조 하세요.
