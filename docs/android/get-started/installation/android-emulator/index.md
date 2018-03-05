---
title: "Android Emulator 설정"
description: "이 섹션에서는 앱 테스트를 위해 Android SDK Emulator를 준비하는 방법을 설명합니다. 최상의 성능을 위해 에뮬레이터를 가속화하는 방법을 설명하고, 에뮬레이터 관리자를 사용하여 가상 장치를 만들고 사용자 지정하는 방법을 보여줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/25/2018
ms.openlocfilehash: 854ca06abc8be2f55f3e95a8ac3bd87c78af19cf
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="android-emulator-setup"></a>Android Emulator 설정

_이 섹션에서는 앱 테스트를 위해 Android SDK Emulator를 준비하는 방법을 설명합니다. 최상의 성능을 위해 에뮬레이터를 가속화하는 방법을 설명하고, 에뮬레이터 관리자를 사용하여 가상 장치를 만들고 사용자 지정하는 방법을 보여줍니다._


## <a name="overview"></a>개요

Google Android SDK 에뮬레이터를 다양한 구성으로 실행하여 다양한 장치를 시뮬레이션할 수 있습니다. 이러한 구성은 각각 _가상 장치_로 생성됩니다. 이 가이드에서는 성능 개선을 위해 Android Emulator를 가속화하고, Xamarin Android Emulator 관리자 또는 기존의 Google 에뮬레이터 관리자를 사용하여 가상 장치를 만드는 방법을 알아봅니다.


> [!NOTE]
> **참고:** Android SDK Tools 버전 **26.0.1** 이상부터 Google은 새로운 CLI(명령줄 인터페이스) 도구를 위해 기존의 AVD/SDK 관리자에 대한 지원을 중단했습니다. 이러한 사용 중단 변경으로 인해 이제 Android Tools 26.0.1 이상에서 Google SDK/에뮬레이터 관리자 대신 Xamarin SDK/장치 관리자가 사용됩니다. (Xamarin SDK Manager에 대한 자세한 내용은 [Android SDK 설정](~/android/get-started/installation/android-sdk.md)을 참조하세요).


## <a name="sections"></a>섹션

### <a name="hardware-accelerationandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[하드웨어 가속](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Android SDK 에뮬레이터 성능을 최대화하기 위해 컴퓨터를 준비하는 방법입니다. Android SDK 에뮬레이터는 하드웨어 가속이 없으면 매우 느려질 수 있기 때문에 Android SDK 에뮬레이터를 사용하기 전에 컴퓨터에 하드웨어 가속을 활성화하는 것이 좋습니다.

### <a name="xamarin-android-device-managerandroidget-startedinstallationandroid-emulatorxamarin-device-managermd"></a>[Xamarin Android 장치 관리자](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)

Xamarin Android 장치 관리자를 사용하여 Android SDK 에뮬레이터 가상 장치를 만들고 사용자 지정하는 방법입니다. **Xamarin Android 장치 관리자**(현재 미리 보기 상태)는 레거시 Google 에뮬레이터 관리자를 대체하기 위해 고안되었습니다. Android Oreo 8.0 이상을 대상으로 하는 경우 Google 에뮬레이터 관리자 대신 Xamarin Android 장치 관리자를 사용해야 합니다.

### <a name="google-emulator-managerandroidget-startedinstallationandroid-emulatorgoogle-emulator-managermd"></a>[Google 에뮬레이터 관리자](~/android/get-started/installation/android-emulator/google-emulator-manager.md)

레거시 Google 에뮬레이터 관리자를 사용하여 Android SDK 에뮬레이터 가상 장치를 만들고 사용자 지정하는 방법입니다. Android SDK Tools 버전 25.2.5 이하를 유지하면 원래 Google 에뮬레이터 관리자와 Google Android 에뮬레이터를 계속 함께 실행할 수 있습니다.

Android SDK 에뮬레이터를 구성한 후 에뮬레이터를 시작하고 앱 테스트 및 디버깅에 사용하는 방법에 대한 자세한 내용은 [Android SDK 에뮬레이터](~/android/deploy-test/debugging/android-sdk-emulator/index.md)를 참조하세요.
