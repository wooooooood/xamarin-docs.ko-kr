---
title: "Android SDK 에뮬레이터 실행"
description: "Android SDK 에뮬레이터를 통해 앱을 디버그하는 방법"
ms.topic: article
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 89768d2562814091f0e5894c4af2edd67d68cb00
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="running-the-android-sdk-emulator"></a>Android SDK 에뮬레이터 실행

이 가이드에서는 앱 디버그 및 테스트를 위해 Android SDK 에뮬레이터에서 가상 장치를 시작하는 방법을 알아봅니다.

## <a name="using-a-pre-configured-virtual-device"></a>미리 구성된 가상 장치 사용

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio에는 장치 드롭다운 메뉴에 표시되는 미리 구성된 가상 장치가 있습니다. 예를 들어 다음 Visual Studio 2017 스크린 샷에서는 몇 가지 미리 구성된 가상 장치를 제공합니다.

-   **VisualStudio\_android-23\_arm\_phone**

-   **VisualStudio\_android-23\_arm\_tablet**

-   **VisualStudio\_android-23\_x86\_phone** 

-   **VisualStudio\_android-23\_x86\_tablet** 

[![가상 장치](running-the-emulator-images/win/01-virtual-devices-sml.png)](running-the-emulator-images/win/01-virtual-devices.png#lightbox)

일반적으로 **VisualStudio\_android-23\_x86\_phone** 가상 장치를 선택하여 전화 앱을 테스트 및 디버그합니다. 미리 구성된 가상 장치 중 하나가 요구 사항에 부합할 경우(즉 앱의 대상 API 수준에 맞는 경우) [에뮬레이터 시작](#launching)으로 건너뛰어 에뮬레이터에서 앱 실행을 시작합니다. Android API 수준에 대해 아직 잘 모르겠으면 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조하세요.

Xamarin.Android 프로젝트에서 사용 가능한 가상 장치와 호환되지 않는 대상 프레임워크 수준을 사용할 경우, 드롭다운 메뉴의 **지원되지 않는 장치**에 사용할 수 없는 가상 장치가 나열됩니다. 예를 들어 다음 프로젝트는 대상 프로레임워크가 **Android 7.1 Nougat(API 25)**로 설정되었는데 이는 기본 제공되는 **Android 6.0** 가상 장치와 호환되지 않습니다.

[![호환되지 않는 가상 장치](running-the-emulator-images/win/02-incompatible-level-sml.png)](running-the-emulator-images/win/02-incompatible-level.png#lightbox)

**최소 Android 대상 변경**을 클릭하여 프로젝트의 최소 Android 버전을 사용 가능한 가상 장치의 API 수준에 맞게 변경할 수 있습니다. 또는 나중에 [가상 장치 구성](#virtualdevice)에서 설명한 대로 **Android Emulator Manager**를 사용하여 대상 API 수준을 지원하는 새 가상 장치를 만들 수 있습니다. 새 API 수준에 대해 가상 장치를 구성할 수 있으려면 먼저 해당 API 수준에 대한 시스템 이미지를 설치해야 합니다. &ndash; 이에 대해서는 다음 섹션에서 설명합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac에는 장치 드롭다운 메뉴에 표시되는 미리 구성된 가상 장치가 있습니다. 예를 들어 다음 스크린 샷에서는 몇 가지 미리 구성된 두 가상 장치를 제공합니다.

-   **Android\_Accelerated\_x86**

-   **Android\_ARMv7a**

[![가상 장치](running-the-emulator-images/mac/01-virtual-devices-sml.png)](running-the-emulator-images/mac/01-virtual-devices.png#lightbox)

일반적으로 **Android\_Accelerated\_x86** 가상 장치를 선택하여 전화 앱을 테스트 및 디버그합니다. 미리 구성된 이 가상 장치가 요구 사항에 부합할 경우(즉 앱의 대상 API 수준에 맞는 경우) [에뮬레이터 시작](#launching)으로 건너뛰어 에뮬레이터에서 앱 실행을 시작합니다. Android API 수준에 대해 아직 잘 모르겠으면 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조하세요.

-----

## <a name="creating-custom-virtual-devices"></a>사용자 지정 가상 장치 만들기

사용자 지정 가상 장치를 만들려면 Xamarin Android 장치 관리자나 Android SDK의 일부인 레거시 Google 에뮬레이터 관리자를 사용해야 합니다. 가상 장치 만들기 및 사용자 지정에 대한 자세한 내용은 [Xamarin Android 장치 관리자](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)를 참조하세요.
레거시 Google 에뮬레이터 관리자를 사용하려면 [Google 에뮬레이터 관리자](~/android/get-started/installation/android-emulator/google-emulator-manager.md)를 참조하세요.

참고 Android 8.0 Oreo를 대상으로 하는 개발에서는 Xamarin Android 장치 관리자를 사용해야 합니다.

<a name="launching" />

## <a name="launching-the-emulator"></a>에뮬레이터 시작

IDE 위에는 **디버그** 또는 **릴리스** 모드를 선택하는 데 사용할 수 있는 드롭다운 메뉴가 있습니다. **디버그**를 선택하면 디버거가 에뮬레이터 안에 실행 중인 응용 프로그램 프로세스에 연결됩니다. 

장치 드롭다운 메뉴에서 가상 장치를 선택한 후 **디버그** 또는 **릴리스** 모드를 선택한 다음 **재생** 단추를 클릭하여 응용 프로그램을 실행합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![디버그 및 릴리스 모드, 재생 단추](running-the-emulator-images/win/17-debug-release-sml.png)](running-the-emulator-images/win/17-debug-release.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![디버그 및 릴리스 모드, 재생 단추](running-the-emulator-images/mac/16-debug-release-sml.png)](running-the-emulator-images/mac/16-debug-release.png#lightbox)

-----

Android 에뮬레이터가 시작된 후 Xamarin.Android가 앱을 에뮬레이터에 배포하기 시작합니다. 에뮬레이터가 구성된 가상 장치 이미지로 앱을 실행합니다. Android SDK 에뮬레이터의 스크린 샷 예가 아래에 표시되어 있습니다(이 에뮬레이터는 이름이 **MyApp**인 빈 앱을 실행 중).

![빈 앱을 실행 중인 에뮬레이터](running-the-emulator-images/emulator-running.png)

에뮬레이터는 계속 실행될 수 있습니다. 즉 앱을 실행할 때마다 매번 종료하고 다시 시작하지 않아도 됩니다. 처음 Xamarin.Android 앱을 에뮬레이터에서 실행하면 대상 API 수준에 대한 Xamarin.Android 공유 런타임이 설치되고 이후 응용 프로그램이 설치됩니다. 런타임 설치에는 몇 분 정도 걸릴 수 있으므로 기다려야 합니다. 첫 번째 Xamarin.Android 앱이 에뮬레이터에 배포된 후에만 런타임 설치가 수행됩니다. &ndash; 이후의 배포는 앱만 에뮬레이터에 복사하는 것이므로 속도가 빨라집니다.

Android SDK 에뮬레이터 사용에 대한 자세한 내용은 다음 Android 개발자 항목을 참조하세요.

-   [화면에서 탐색](https://developer.android.com/studio/run/emulator.html#navigate)

-   [에뮬레이터에서 기본적인 작업 수행](https://developer.android.com/studio/run/emulator.html#tasks)

-   [확장된 컨트롤, 설정 및 도움말 사용](https://developer.android.com/studio/run/emulator.html#extended)

