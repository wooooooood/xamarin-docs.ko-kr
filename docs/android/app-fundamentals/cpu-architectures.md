---
title: CPU 아키텍처
description: Xamarin.Android는 32 비트 및 64 비트 장치를 비롯 한 여러 CPU 아키텍처를 지원 합니다. 이 문서에서는 앱이 하나 이상의 Android-지원 되는 CPU 아키텍처를 대상 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: D4BC889D-9164-49BB-9B7B-F6C4E4E109F1
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: f2865858552d4445dff95c85767c41849c19cc29
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61018268"
---
# <a name="cpu-architectures"></a>CPU 아키텍처

_Xamarin.Android는 32 비트 및 64 비트 장치를 비롯 한 여러 CPU 아키텍처를 지원 합니다. 이 문서에서는 앱이 하나 이상의 Android-지원 되는 CPU 아키텍처를 대상 하는 방법에 설명 합니다._

## <a name="cpu-architectures-overview"></a>CPU 아키텍처 개요

어떤 플랫폼 CPU 아키텍처를 지정 해야 릴리스용 앱을 준비할 때 앱에서 지 원하는 프로그램입니다. 단일 APK가 여러 서로 다른 아키텍처를 지원하는 머신 코드를 포함할 수 잇습니다. 아키텍처 관련 코드의 각 컬렉션은 연관 된 *응용 프로그램 이진 인터페이스* (ABI). 각 abi 당이 기계어 코드로 런타임에 Android와 상호 작용 해야 하는 방법을 정의 합니다.
작동 하는 방법에 대 한 자세한 내용은 참조 하세요. [다중 코어 장치 &amp; Xamarin.Android](~/android/deploy-test/multicore-devices.md)합니다.


## <a name="how-to-specify-supported-architectures"></a>지원 되는 아키텍처를 지정 하는 방법

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

일반적으로 명시적으로 선택 하면 아키텍처 (또는 아키텍처)에 대 한 앱이 구성 하는 경우 **릴리스**합니다. 앱에 대해 구성 된 경우 **디버그**의 **공유 런타임 사용** 하 고 **빠른 배포 사용** 명시적 아키텍처 선택 영역을 사용 하지 않도록 설정 하는 옵션이 활성화 됩니다.

Visual Studio에서 마우스 오른쪽 단추로 클릭에서 프로젝트를 **솔루션 탐색기** 선택한 **속성**합니다. 아래는 **Android 옵션** 확인 페이지를 **패키징 속성** 섹션 및 확인 **공유 런타임 사용** 을 사용할 수 없습니다 (이 기능을 해제 하면를 명시적으로 지원 되는 Abi 선택). 클릭 합니다 **고급** 단추 및 아래 **지원 되는 아키텍처**를 지원 하 고 아키텍처를 확인:

[![Armeabi와 armeabi-v7a 선택](cpu-architectures-images/vs/01-abi-selections-sml.png)](cpu-architectures-images/vs/01-abi-selections.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

일반적으로 명시적으로 선택 하면 아키텍처 (또는 아키텍처)에 대 한 앱이 구성 하는 경우 **릴리스**합니다. 앱에 대해 구성 된 경우 **디버그**서 **공유 Mono 런타임 사용** 하 고 **빠른 어셈블리 배포** 옵션을 사용 하는 명시적 아키텍처를 방지 하는 선택할 수 있습니다.

Mac 용 Visual Studio에서 프로젝트를 찾습니다는 **솔루션** 패딩 프로젝트 옆에 있는 기어 아이콘을 클릭 하 고 선택 **옵션**합니다. 에 **프로젝트 옵션** 대화 상자에서 클릭 **Android 빌드**합니다. 클릭 합니다 **일반** 탭을 확인 하는 **공유 Mono 런타임 사용** 을 사용할 수 없습니다 (해제 하면이 선택할 수 있도록 명시적으로 지원 하기 위해 Abi). 클릭 합니다 **고급** 탭 및 아래 **지원 되는 Abi**를 지원 하 고 아키텍처용 Abi를 확인:

[![Armeabi와 armeabi-v7a 선택](cpu-architectures-images/xs/01-abi-selections-sml.png)](cpu-architectures-images/xs/01-abi-selections.png#lightbox)

-----


Xamarin.Android는 다음과 같은 아키텍처를 지원합니다.

-   **armeabi** &ndash; ARM 기반 Cpu 이상 ARMv5TE 명령 집합을 지원 합니다. `armeabi` 는 스레드 안전 하지 않으며 다중 CPU 장치에서 사용할 수 없습니다.

-   **armeabi-v7a** &ndash; 하드웨어 부동 소수점 연산 및 여러 CPU (SMP) 장치를 사용 하 여 ARM 기반 Cpu입니다. `armeabi-v7a` 기계어 코드는 ARMv5 장치에서 실행 되지 것입니다.

-   **arm64-v8a** &ndash; Cpu ARMv8 64 비트 아키텍처를 기반으로 합니다.

-   **x86** &ndash; x86 (또는 IA-32)를 지 원하는 Cpu의 명령 집합입니다. 이 명령 집합 MMX, SSE, SSE2 및 SSE3 지침을 비롯 한 Pentium Pro의 것과 같습니다.

-   **x86_64** 64 비트 x86을 지 원하는 Cpu의 (라고도 *x64* 하 고 *AMD64*) 명령 집합입니다.

기본적으로 Xamarin.Android `armeabi-v7a` 에 대 한 **릴리스** 빌드합니다. 이 설정은 보다 훨씬 뛰어난 성능을 제공 `armeabi`합니다. (예: Nexus 9) 64 비트 ARM 플랫폼을 대상으로 하는 경우 선택 `arm64-v8a`합니다. X86에 앱을 배포 하는 경우 장치를 `x86`입니다. 64 비트 CPU 아키텍처를 사용 하는 x86 대상 장치 선택 `x86_64`합니다.

## <a name="targeting-multiple-platforms"></a>여러 플랫폼을 대상으로

여러 CPU 아키텍처를 대상으로 더 큰 APK 파일 크기) (저하 둘 이상의 ABI를 선택할 수 있습니다. 사용할 수는 **선택한 ABI 당 하나의 패키지 (.apk) 생성** 옵션 (에 설명 된 [패키지 속성 설정](~/android/deploy-test/release-prep/index.md#Set_Packaging_Properties)) 각각에 대 한 별도 APK를 만드는 아키텍처를 지원 합니다.

선택할 필요가 없습니다 **arm64-v8a** 하거나 **x86_64** 64 비트 장치를 대상으로 64 비트 지원 64 비트 하드웨어에서 앱을 실행 하지 않아도 됩니다. 예를 들어, 64 비트 ARM 장치 (예는 [Nexus 9](http://www.google.com/nexus/9/))에 대해 구성 된 앱을 실행할 수 `armeabi-v7a`합니다. 64 비트 지원을 사용 하도록 설정 하는 주요 장점은 더 많은 메모리를 앱에 대 한 수 있도록 하는 것입니다.

> [!NOTE]
> 64 비트 런타임 지원은 현재 실험적인 기능입니다. 64 비트 런타임을 *되지* 64 비트 장치에서 앱을 실행 하는 데 필요한 합니다. 

## <a name="additional-information"></a>추가 정보

각 아키텍처에 대해 별도 APK를 만들어야 할 경우가 있습니다 (에 APK의 크기를 줄이기 위해 또는 특정 CPU 아키텍처와 관련 된 라이브러리를 공유 하는 앱에 있으므로).
이 방법에 대 한 자세한 내용은 참조 하세요. [ABI 관련 Apk 빌드](~/android/deploy-test/building-apps/abi-specific-apks.md)합니다.
