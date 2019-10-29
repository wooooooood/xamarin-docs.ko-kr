---
title: 에뮬레이터에서 Android 마모 디버그
description: 이러한 문서에서는 에뮬레이터에서 Xamarin Android 마모 응용 프로그램을 디버깅 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: ca0a6884c05686bded25a2e515456ab192002a24
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028679"
---
# <a name="debug-android-wear-on-an-emulator"></a>에뮬레이터에서 Android 마모 디버그

_이러한 문서에서는 에뮬레이터에서 Xamarin Android 마모 응용 프로그램을 디버깅 하는 방법을 설명 합니다._

## <a name="debug-wear-on-emulator-overview"></a>에뮬레이터의 디버그 개요 개요

Android 마모 응용 프로그램을 개발 하려면 물리적 하드웨어에서 또는 에뮬레이터 또는 시뮬레이터를 사용 하 여 응용 프로그램을 실행 해야 합니다. 하드웨어를 사용하는 것이 가장 좋은 방법이지만 이것이 실용적이지 않은 경우가 있습니다. 대부분의 경우 아래에 설명 된 대로 에뮬레이터를 사용 하 여 Android 마모 된 하드웨어를 시뮬레이션/에뮬레이트 하는 것이 더 간단 하 고 비용 효율적일 수 있습니다. Android 앱을 배포 하 고 실행 하는 프로세스를 아직 잘 모르는 경우 [Hello, 마모](~/android/wear/get-started/hello-wear.md)를 참조 하세요.

## <a name="configure-the-android-emulator"></a>Android Emulator 구성

에뮬레이터에서 앱을 실행 하려면 Android SDK Android Emulator를 설치 하 고 Android 용으로 구성 해야 합니다. 전체 Android SDK 에뮬레이터 설치 및 구성 정보는 [Android Emulator 설치](~/android/get-started/installation/android-emulator/index.md)를 참조 하세요.

마모 된 가상 장치를 만들 때 android 마모 된 장치 프로필 (예: **Android 마모 사각형**)을 선택 합니다. 성능 향상을 위해이 예제에서 볼 때와 같이 마모 된 **x86** CPU/ABI를 사용 합니다.

[![예제 가상 장치 구성](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png#lightbox)

## <a name="launch-the-wear-virtual-device"></a>마모 된 가상 장치 시작 

Android 마모 된 가상 장치를 만든 후에는 디버깅을 시작 하기 전에 IDE의 장치 풀 다운 메뉴에서 선택할 수 있습니다. 장치 풀 다운에서 가상 장치를 사용할 수 없는 경우 *프로젝트가 android 앱* 프로젝트가 아닌 android 앱 프로젝트이 고 해당 대상 api 수준이 가상 장치와 동일한 API 수준으로 설정 되어 있는지 확인 합니다. 예를 들면,

[Visual Studio 장치 메뉴에서 마모 된 AVD를 선택![](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png#lightbox)

Android 에뮬레이터가 시작 된 후 Xamarin Android는 앱을 에뮬레이터에 배포 합니다. 에뮬레이터가 구성된 가상 디바이스 이미지로 앱을 실행합니다.

처음에이 (또는 다른 중간 화면)가 표시 되는 경우에는 걱정할 필요가 없습니다. 보기 에뮬레이터를 시작 하는 데 시간이 걸릴 수 있습니다. 

![보기 에뮬레이터에는 1 분만 표시 됩니다.](debug-on-emulator-images/please-wait.png)

에뮬레이터는 계속 실행될 수 있습니다. 즉 앱을 실행할 때마다 매번 종료하고 다시 시작하지 않아도 됩니다.

## <a name="summary"></a>요약

이 가이드에서는 개발을 위해 Android Emulator를 구성 하 고, 디버깅을 위해 마모 된 가상 장치를 시작 하는 방법을 설명 했습니다.
