---
title: CPU 아키텍처
description: Xamarin.Android 32 비트 및 64 비트 장치를 포함 하 여 여러 CPU 아키텍처를 지원 합니다. 이 문서에서는 앱을 Android에서 지 원하는 CPU 아키텍처 하나 이상의 대상으로 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D4BC889D-9164-49BB-9B7B-F6C4E4E109F1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: dea5aaa16891893f649d5ec56f3e6b1ee9a18683
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="cpu-architectures"></a>CPU 아키텍처

_Xamarin.Android 32 비트 및 64 비트 장치를 포함 하 여 여러 CPU 아키텍처를 지원 합니다. 이 문서에서는 앱을 Android에서 지 원하는 CPU 아키텍처 하나 이상의 대상으로 하는 방법을 설명 합니다._

## <a name="cpu-architectures-overview"></a>CPU 아키텍처 개요

릴리스에 대 한 앱을 준비 하는 경우 어떤 플랫폼 CPU 아키텍처를 지정 해야 응용 프로그램 지원 합니다. 단일 APK가 여러 서로 다른 아키텍처를 지원하는 머신 코드를 포함할 수 잇습니다. 각 컬렉션 아키텍처 관련 코드에 연결 된 프로그램 *응용 프로그램 이진 인터페이스* (ABI). 각 ABI이 기계어 코드는 런타임 시 Android와 상호 작용 해야 방법을 정의 합니다.
이 과정에 대 한 자세한 내용은 참조 [다중 코어 장치 &amp; Xamarin.Android](~/android/deploy-test/multicore-devices.md)합니다.


## <a name="how-to-specify-supported-architectures"></a>지원 되는 아키텍처를 지정 하는 방법

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

일반적으로 명시적으로 선택 하는 아키텍처 (또는 아키텍처) 응용 프로그램에 대해 구성 된 경우 **릴리스**합니다. 응용 프로그램에 대해 구성 된 경우 **디버그**, **공유 런타임 사용** 및 **빠른 배포 사용** 옵션이 설정 되었으면는 명시적 아키텍처 선택 영역을 사용 하지 않도록 설정 합니다.

Visual Studio에서 두 번 클릭 **속성** 에서 프로젝트 아래의 **솔루션 탐색기** 선택 하 고는 **Android 옵션** 페이지. 클릭는 **패키징** 탭 하 고 있는지 확인 **공유 런타임 사용** 사용 되지 않는지 (해제 하면이 선택할 수 있습니다 명시적으로 지원 하기 위해 어떤 ABIs). 클릭는 **고급** 탭 고 **고급 속성**를 지원 하 고 아키텍처 확인:

[![Armeabi 및 armeabi v7a 선택](cpu-architectures-images/vs/01-abi-selections-sml.png)](cpu-architectures-images/vs/01-abi-selections.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

일반적으로 명시적으로 선택 하는 아키텍처 (또는 아키텍처) 응용 프로그램에 대해 구성 된 경우 **릴리스**합니다. 응용 프로그램에 대해 구성 된 경우 **디버그**, **공유 모노 런타임 사용** 및 **어셈블리 배포 빠른** 명시적 아키텍처를 방지 하는 옵션이 활성화 됩니다 선택할 수 있습니다.

Mac 용 Visual Studio에서 프로젝트를 찾습니다는 **솔루션** 패드 프로젝트 옆의 기어 아이콘을 선택 **옵션**합니다. 에 **프로젝트 옵션** 대화 상자를 클릭 하 여 **Android 빌드**합니다. 클릭는 **일반** 탭 하 고 있는지 확인 **공유 모노 런타임 사용** 사용 되지 않는지 (해제 하면이 선택할 수 있습니다 명시적으로 지원 하기 위해 어떤 ABIs). 클릭는 **고급** 탭 고 **지원 ABIs**를 지원 하 고 아키텍처에 대 한 ABIs 확인:

[![Armeabi 및 armeabi v7a 선택](cpu-architectures-images/xs/01-abi-selections-sml.png)](cpu-architectures-images/xs/01-abi-selections.png#lightbox)

-----


Xamarin.Android는 다음과 같은 아키텍처를 지원합니다.

-   **armeabi** &ndash; ARM 기반 Cpu를 최소한 ARMv5TE 명령 집합을 지원 합니다. `armeabi` 는 스레드로부터 안전 하지 않으며 다중 CPU 장치에 사용할 수 없습니다.

-   **armeabi v7a** &ndash; 하드웨어 부동 소수점 연산 및 여러 CPU (SMP) 장치를 사용 하 여 ARM 기반 Cpu. `armeabi-v7a` 기계어 코드 ARMv5 장치에서 실행 되지 것입니다.

-   **arm64 v8a** &ndash; 64 비트 ARMv8 아키텍처에 따라 Cpu.

-   **x86** &ndash; x86 (또는 i A 32)를 지 원하는 Cpu 명령 집합을 사용 합니다. 이 명령 집합은 MMX와 SSE, SSE2 및 SSE3 명령을 포함 하 여 Pentium pro입니다.

-   **x86_64** Cpu를 지 원하는 64 비트 x86 (라고도 *x64* 및 *AMD64*) 명령 집합을 사용 합니다.

기본적으로 Xamarin.Android `armeabi-v7a` 에 대 한 **릴리스** 빌드합니다. 이 설정은 보다 훨씬 더 나은 성능을 제공 `armeabi`합니다. (예: Nexus 9) 64 비트 ARM 플랫폼을 대상으로 하는 경우 선택 `arm64-v8a`합니다. x86 앱을 배포 하는 경우 장치를 `x86`합니다. 64 비트 CPU 아키텍처를 사용 하는 x86 대상 장치 선택 `x86_64`합니다.

## <a name="targeting-multiple-platforms"></a>여러 플랫폼을 대상으로 지정

여러 CPU 아키텍처를 대상으로 (저하 시켜 더 큰 APK 파일 크기) 둘 이상의 ABI를 선택할 수 있습니다. 사용할 수 있습니다는 **선택한 ABI 당 하나의 패키지 (.apk)를 생성** 옵션 (에 설명 된 [패키지 속성 설정](~/android/deploy-test/release-prep/index.md#Set_Packaging_Properties)) 각각에 대해 별도 APK를 만들려는 아키텍처를 지원 합니다.

선택 해야 **arm64 v8a** 또는 **x86_64** ; 64 비트 장치를 대상으로 64 비트 지원은 64 비트 하드웨어에서 앱을 실행 하지 않아도 됩니다. 예를 들어 64 비트 ARM 장치 (예:는 [Nexus 9](http://www.google.com/nexus/9/))에 대해 구성 된 앱을 실행할 수 `armeabi-v7a`합니다. 64 비트 지원을 사용 하도록 설정의 주요 이점은 더 많은 메모리를 앱에 대 한 수 있게 하는 것입니다.

> [!NOTE]
> 현재 64 비트 런타임 지원은 실험적인 기능 합니다. 64 비트 런타임을 *하지* 64 비트 장치에서 앱을 실행 하는 데 필요한 합니다. 

## <a name="additional-information"></a>추가 정보

각 아키텍처에 대해 별도 APK 만들어야 할 경우가 있습니다 (APK의 크기를 줄이려면 앱 특정 CPU 아키텍처와 관련 된 라이브러리 공유에 때문에 또는).
이 방법을 사용 하는 방법에 대 한 자세한 내용은 참조 [ABI 관련 APKs 빌드](~/android/deploy-test/building-apps/abi-specific-apks.md)합니다.
