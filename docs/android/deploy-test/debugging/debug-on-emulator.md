---
title: Android Emulator에서 디버깅
description: 이 가이드에서는 Android Emulator를 사용하여 Visual Studio에서 앱을 시작하고 디버그하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: 6d595a487d87c7e30c87a0347d25404d0b2f7dbc
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/11/2018
ms.locfileid: "51527315"
---
# <a name="debugging-on-the-android-emulator"></a>Android Emulator에서 디버깅

_이 가이드에서는 앱을 디버그하고 테스트하기 위해 Android Emulator에서 가상 장치를 시작하는 방법을 알아봅니다._

## <a name="overview"></a>개요

Android Emulator(**.NET을 사용한 모바일 개발** 워크로드의 일부로 설치됨)를 다양한 구성으로 실행하여 다양한 Android 디바이스를 시뮬레이션할 수 있습니다. 이러한 구성은 각각 _가상 디바이스_로 생성됩니다. 이 가이드에서는 Visual Studio에서 에뮬레이터를 시작하고, 가상 디바이스에서 앱을 실행하는 방법을 알아봅니다. Android Emulator를 구성하고 새 가상 디바이스를 만드는 방법에 대한 자세한 내용은 [Android Emulator 설정](~/android/get-started/installation/android-emulator/index.md)을 참조하세요.


## <a name="using-a-pre-configured-virtual-device"></a>미리 구성된 가상 디바이스 사용

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에는 디바이스 드롭다운 메뉴에 표시되는 미리 구성된 가상 디바이스가 있습니다. 예를 들어 다음 Visual Studio 2017 스크린 샷에서는 몇 가지 미리 구성된 가상 디바이스를 제공합니다.

-   **VisualStudio\_android-23\_arm\_phone**

-   **VisualStudio\_android-23\_arm\_tablet**

-   **VisualStudio\_android-23\_x86\_phone** 

-   **VisualStudio\_android-23\_x86\_tablet** 

[![가상 장치](debug-on-emulator-images/win/01-virtual-devices-sml.png)](debug-on-emulator-images/win/01-virtual-devices.png#lightbox)

일반적으로 **VisualStudio\_android-23\_x86\_phone** 가상 디바이스를 선택하여 전화 앱을 테스트 및 디버그합니다. 미리 구성된 가상 디바이스 중 하나가 요구 사항에 부합할 경우(즉 앱의 대상 API 수준에 맞는 경우) [에뮬레이터 시작](#launching)으로 건너뛰어 에뮬레이터에서 앱 실행을 시작합니다. Android API 수준에 대해 아직 잘 모르겠으면 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조하세요.

Xamarin.Android 프로젝트에서 사용 가능한 가상 디바이스와 호환되지 않는 대상 프레임워크 수준을 사용할 경우, 드롭다운 메뉴의 **지원되지 않는 디바이스**에 사용할 수 없는 가상 디바이스가 나열됩니다. 예를 들어 다음 프로젝트는 대상 프로레임워크가 **Android 7.1 Nougat(API 25)** 로 설정되었는데 이는 이 예제에 나열되어 있는 **Android 6.0** 가상 디바이스와 호환되지 않습니다.

[![호환되지 않는 가상 장치](debug-on-emulator-images/win/02-incompatible-level-sml.png)](debug-on-emulator-images/win/02-incompatible-level.png#lightbox)

**최소 Android 대상 변경**을 클릭하여 프로젝트의 최소 Android 버전을 사용 가능한 가상 장치의 API 수준에 맞게 변경할 수 있습니다. 또는 [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)를 사용하여 대상 API 수준을 지원하는 새 가상 디바이스를 만들 수 있습니다.
새 API 수준에 대해 가상 디바이스를 구성할 수 있으려면 먼저 해당 API 수준에 대한 시스템 이미지를 설치해야 합니다([Xamarin.Android에 대한 Android SDK 설정](~/android/get-started/installation/android-sdk.md) 참조).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac에는 디바이스 드롭다운 메뉴에 표시되는 미리 구성된 가상 디바이스가 있습니다. 예를 들어 다음 스크린 샷에서는 몇 가지 미리 구성된 두 가상 디바이스를 제공합니다.

-   **Android\_Accelerated\_x86**

-   **Android\_ARMv7a**

[![가상 장치](debug-on-emulator-images/mac/01-virtual-devices-sml.png)](debug-on-emulator-images/mac/01-virtual-devices.png#lightbox)

일반적으로 **Android\_Accelerated\_x86** 가상 디바이스를 선택하여 전화 앱을 테스트 및 디버그합니다. 미리 구성된 이 가상 디바이스가 요구 사항에 부합할 경우(즉 앱의 대상 API 수준에 맞는 경우) [에뮬레이터 시작](#launching)으로 건너뛰어 에뮬레이터에서 앱 실행을 시작합니다. Android API 수준에 대해 아직 잘 모르겠으면 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조하세요.

-----

## <a name="editing-virtual-devices"></a>가상 디바이스 편집

가상 디바이스를 수정하려면(또는 새로 만들려면) [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)를 사용해야 합니다.


<a name="launching" />

## <a name="launching-the-emulator"></a>에뮬레이터 시작

Visual Studio 위에는 **디버그** 또는 **릴리스** 모드를 선택하는 데 사용할 수 있는 드롭다운 메뉴가 있습니다. **디버그**를 선택하면 디버거가 앱이 시작된 후 에뮬레이터 안에 실행 중인 응용 프로그램 프로세스에 연결됩니다. **릴리스** 모드를 선택하면 디버거가 비활성화됩니다(그러나 여전히 앱을 실행하고 디버그에 대한 로그 문을 사용할 수 있음). 디바이스 드롭다운 메뉴에서 가상 디바이스를 선택한 후 **디버그** 또는 **릴리스** 모드를 선택한 다음, 재생 단추를 클릭하여 응용 프로그램을 실행합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![디버그 및 릴리스 모드, 재생 단추](debug-on-emulator-images/win/17-debug-release-sml.png)](debug-on-emulator-images/win/17-debug-release.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![디버그 및 릴리스 모드, 재생 단추](debug-on-emulator-images/mac/16-debug-release-sml.png)](debug-on-emulator-images/mac/16-debug-release.png#lightbox)

-----

에뮬레이터가 시작된 후 Xamarin.Android가 앱을 에뮬레이터에 배포합니다. 에뮬레이터가 구성된 가상 디바이스 이미지로 앱을 실행합니다. Android Emulator의 예제 스크린샷은 아래에 표시됩니다. 이 예제에서 에뮬레이터는 **MyApp**이라는 빈 앱을 실행 중입니다.

![빈 앱을 실행 중인 에뮬레이터](debug-on-emulator-images/emulator-running.png)

에뮬레이터는 계속 실행될 수 있습니다. 즉 앱을 시작할 때마다 매번 종료하고 다시 시작하도록 기다리지 않아도 됩니다. 처음 Xamarin.Android 앱을 에뮬레이터에서 실행하면 대상 API 수준에 대한 Xamarin.Android 공유 런타임이 설치되고 이후 애플리케이션이 설치됩니다. 런타임 설치에는 몇 분 정도 걸릴 수 있으므로 기다려야 합니다. 첫 번째 Xamarin.Android 앱이 에뮬레이터에 배포된 후에만 런타임 설치가 수행됩니다. &ndash; 이후의 배포는 앱만 에뮬레이터에 복사하는 것이므로 속도가 빨라집니다.

<a name="quick-boot" />

## <a name="quick-boot"></a>빠른 부팅

최신 버전의 Android Emulator는 몇 초 이내에 에뮬레이터를 시작하는 _빠른 부팅_이라는 기능을 포함합니다. 에뮬레이터를 닫을 때 다시 시작될 때 해당 상태에서 빠르게 복원될 수 있도록 가상 디바이스 상태의 스냅숏을 찍습니다.
이 기능에 액세스하려면 다음이 필요합니다.

-   Android Emulator 버전 27.0.2 이상
-   Android SDK Tools 버전 26.1.1 이상

위에 나열된 버전의 에뮬레이터 및 SDK 도구가 설치되면 빠른 부팅 기능은 기본적으로 활성화됩니다. 

가상 디바이스의 첫 번째 콜드 부팅이 속도 향상 없이 발생됩니다. 스냅숏이 아직 생성되지 않았기 때문입니다.

![콜드 부팅 스크린샷](debug-on-emulator-images/cold-boot.png)

에뮬레이터를 종료하는 경우 빠른 부팅은 에뮬레이터의 상태를 스냅숏에 저장합니다.

![종료 시 상태 저장](debug-on-emulator-images/saving-state.png)

에뮬레이터는 에뮬레이터를 닫았을 때의 상태를 단순히 복원하므로 후속 가상 디바이스 시작은 훨씬 빠릅니다.

![다시 시작 시 상태 로드](debug-on-emulator-images/loading-state.png)


## <a name="troubleshooting"></a>문제 해결

에뮬레이터의 일반적인 문제에 대한 팁 및 해결 방법은 [Android Emulator 문제 해결](~/android/get-started/installation/android-emulator/troubleshooting.md)을 참조하세요.


## <a name="summary"></a>요약

이 가이드에서는 Xamarin.Android 앱을 실행하고 테스트하기 위해 Android Emulator를 구성하는 프로세스를 설명했습니다. 미리 구성된 가상 디바이스를 사용하여 에뮬레이터를 시작하기 위한 단계를 설명하고, Visual Studio에서 에뮬레이터에 응용 프로그램을 배포하기 위한 단계를 제공했습니다. 

Android Emulator를 사용하는 방법에 대한 자세한 내용은 다음 Android 개발자 항목을 참조하세요.

-   [화면에서 탐색](https://developer.android.com/studio/run/emulator.html#navigate)

-   [에뮬레이터에서 기본적인 작업 수행](https://developer.android.com/studio/run/emulator.html#tasks)

-   [확장된 컨트롤, 설정 및 도움말 사용](https://developer.android.com/studio/run/emulator.html#extended)

-   [빠른 부팅을 사용하여 에뮬레이터 실행](https://developer.android.com/studio/run/emulator#quickboot)
