---
title: Android Wear 에뮬레이터에서 디버그
description: 이 문서에서는 에뮬레이터에서 Xamarin.Android Wear 응용 프로그램을 디버그 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: 699fb3cc3a5730e8ab2c677feb7cdfbdcf106aeb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61308277"
---
# <a name="debug-android-wear-on-an-emulator"></a>Android Wear 에뮬레이터에서 디버그

_이 문서에서는 에뮬레이터에서 Xamarin.Android Wear 응용 프로그램을 디버그 하는 방법을 설명 합니다._

## <a name="debug-wear-on-emulator-overview"></a>Wear Emulator 개요에서 디버그

물리적 하드웨어에서 응용 프로그램을 실행 하거나 에뮬레이터 또는 시뮬레이터를 사용 하 여 Android Wear 응용 프로그램 개발에 필요 합니다. 하드웨어를 사용하는 것이 가장 좋은 방법이지만 이것이 실용적이지 않은 경우가 있습니다. 대부분의 경우에서 간단 하 고 아래 설명 된 대로 에뮬레이터를 사용 하 여 Android Wear 하드웨어를 시뮬레이션/에뮬레이션에 보다 비용 효율적인 수 있습니다. Android Wear 앱 배포 및 실행 프로세스에 익숙해 참조 아직 없는 경우 [안녕하세요, Wear](~/android/wear/get-started/hello-wear.md)합니다.

## <a name="configure-the-android-emulator"></a>Android 에뮬레이터를 구성 합니다.

Wear 앱에서 에뮬레이터를 실행 하려면 Android SDK Android 에뮬레이터를 설치 하 고 Android Wear 용 구성 해야 합니다. 전체 Android SDK 에뮬레이터 설치 및 구성 정보를 참조 하세요 [Android Emulator 설정](~/android/get-started/installation/android-emulator/index.md)합니다.

Wear 가상 장치를 만들 때 Android Wear 장치 프로필을 선택 합니다. (같은 **Android Wear 사각형**). 성능 향상된을 위해 사용 된 Wear **x86** 이 예제와 같이 CPU/ABI:

[![Wear 가상 장치 구성 예제](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png#lightbox)


## <a name="launch-the-wear-virtual-device"></a>Wear 가상 장치 시작 

Android Wear 가상 장치를 만든 후 디버깅을 시작 하기 전에 IDE 장치 풀 다운 메뉴에서 선택할 수 있습니다. 가상 장치 풀 다운 장치에서 사용할 수 없는 경우 프로젝트는 Android 인지 확인 *Wear* 가상 장치로 수준 앱 프로젝트는 Android 앱 프로젝트가 아닌 및 해당 대상 API 수준에 동일한 API로 설정 되어 있습니다. 예를 들어:

[![Wear AVD를 Visual Studio 장치 메뉴에서 선택](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png#lightbox)

Android emulator 시작 된 후 Xamarin.Android Wear 앱을 에뮬레이터에 배포 됩니다. 에뮬레이터가 구성된 가상 디바이스 이미지로 앱을 실행합니다.

이 표시 되 면 예상한 것 (또는 다른 중간 화면)에서 첫 번째입니다. 조사식 에뮬레이터를 시작 하는 데 사용할 수 있습니다. 

![에뮬레이터에만 표시 됩니다. 보기...](debug-on-emulator-images/please-wait.png)

에뮬레이터는 계속 실행될 수 있습니다. 즉 앱을 실행할 때마다 매번 종료하고 다시 시작하지 않아도 됩니다.

 
## <a name="summary"></a>요약
 
이 가이드는 Wear 개발에 대 한 Android 에뮬레이터를 구성 하 고 디버깅을 위해 Wear 가상 장치를 시작 하는 방법을 설명 합니다.
