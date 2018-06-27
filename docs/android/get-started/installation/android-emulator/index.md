---
title: Google Android Emulator 설정
description: Google Android Emulator를 다양한 구성으로 실행하여 다양한 장치를 시뮬레이션할 수 있습니다. 이 가이드에서는 앱 테스트를 위해 Android Emulator를 준비하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: e5ba2cc23ea9751ca60644d3eb5b7e3f31bbb6bb
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732533"
---
# <a name="google-android-emulator-setup"></a>Google Android Emulator 설정

_이 가이드에서는 앱 테스트를 위해 Google Android Emulator를 준비하는 방법을 설명합니다._


## <a name="overview"></a>개요

Google Android Emulator를 다양한 구성으로 실행하여 다양한 장치를 시뮬레이션할 수 있습니다. _가상 장치_에서 각 구성이 호출됩니다. 에뮬레이터에서 앱을 배포하고 테스트하는 경우 Nexus 또는 Pixel 전화기와 같은 물리적 Android 장치를 시뮬레이션하는 미리 구성되거나 사용자 지정 가상 장치를 선택합니다.

아래에 나열된 섹션에서는 최대 성능을 위해 Google Android 에뮬레이터를 가속화하는 방법, Android Device Manager를 사용하여 가상 장치를 만들고 사용자 지정하는 방법 및 가상 장치의 프로필 속성을 사용자 지정하는 방법을 설명합니다. 또한 문제 해결 섹션에서는 일반적인 설정 문제 및 해결 방법을 설명합니다.

## <a name="sections"></a>섹션

### <a name="hardware-acceleration-for-emulator-performanceandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[에뮬레이터 성능에 대한 하드웨어 가속](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Android Emulator 성능을 최대화하기 위해 컴퓨터를 준비하는 방법입니다.
Google Android Emulator는 하드웨어 가속이 없으면 매우 느려질 수 있기 때문에 이 에뮬레이터를 사용하기 전에 컴퓨터에 하드웨어 가속을 활성화하는 것이 좋습니다.

### <a name="managing-virtual-devices-with-the-android-device-managerandroidget-startedinstallationandroid-emulatordevice-managermd"></a>[Android Device Manager를 사용하여 가상 장치 관리](~/android/get-started/installation/android-emulator/device-manager.md)

Android Device Manager를 사용하여 가상 장치를 만들고 사용자 지정하는 방법입니다.

### <a name="editing-android-virtual-device-propertiesandroidget-startedinstallationandroid-emulatordevice-propertiesmd"></a>[Android 가상 장치 속성 편집](~/android/get-started/installation/android-emulator/device-properties.md)

Android Device Manager를 사용하여 Android 가상 장치의 프로필 속성을 편집하는 방법입니다.

### <a name="troubleshooting-emulator-setup-problemsandroidget-startedinstallationandroid-emulatortroubleshootingmd"></a>[에뮬레이터 설정 문제 해결](~/android/get-started/installation/android-emulator/troubleshooting.md)

Android Emulator를 설정할 때 발생할 수 있는 Android Device Manager 문제를 진단하고 해결하는 방법입니다.


Android Emulator를 구성한 후 에뮬레이터를 시작하고 앱 테스트 및 디버깅에 사용하는 방법에 대한 자세한 내용은 [Google Android Emulator를 사용하여 디버깅](~/android/deploy-test/debugging/android-sdk-emulator/index.md)을 참조하세요.


> [!NOTE]
> Android SDK Tools 버전 **26.0.1** 이상부터 Google은 새로운 CLI(명령줄 인터페이스) 도구를 위해 기존의 AVD/SDK 관리자에 대한 지원을 중단했습니다. 이러한 사용 중단 변경으로 인해 이제 Android Tools 26.0.1 이상에서 Google SDK/Device Manager 대신 Xamarin SDK/Device Manager가 사용됩니다. Xamarin SDK Manager에 대한 자세한 내용은 [Android SDK 설정](~/android/get-started/installation/android-sdk.md)을 참조하세요.

