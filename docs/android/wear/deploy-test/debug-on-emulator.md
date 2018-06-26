---
title: Android 에뮬레이터 마모 디버그
description: 이 문서에서는 에뮬레이터 Xamarin.Android 마모 응용 프로그램을 디버깅 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2018
ms.openlocfilehash: baa8df87caf2c05d7b6202d5160c930e51656e10
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36934982"
---
# <a name="debug-android-wear-on-an-emulator"></a>Android 에뮬레이터 마모 디버그

_이 문서에서는 에뮬레이터 Xamarin.Android 마모 응용 프로그램을 디버깅 하는 방법을 설명 합니다._

## <a name="debug-wear-on-emulator-overview"></a>디버그 마모 에뮬레이터 개요

실제 하드웨어에서 응용 프로그램을 실행 또는 에뮬레이터 또는 시뮬레이터를 사용 하 여 필요 쓰는 유형 Android 응용 프로그램을 개발 합니다. 하드웨어를 사용하는 것이 가장 좋은 방법이지만 이것이 실용적이지 않은 경우가 있습니다. 대부분의 경우에서 보다 간단 하 고 시뮬레이션/하드웨어를 에뮬레이트하 쓰는 유형 Android 에뮬레이터를 사용 하 여 아래 설명 된 대로 더 비용 효율적인 수 있습니다. 실행 및 배포 프로세스에 잘 쓰는 유형 Android 앱 참조 아직 없는 경우 [안녕하세요, 마모](~/android/wear/get-started/hello-wear.md)합니다.

## <a name="configure-the-android-emulator"></a>Android 에뮬레이터 구성

마모 앱에서 에뮬레이터를 실행 하려면 Android SDK Android 에뮬레이터를 설치 하 고 쓰는 유형 Android에 맞게 구성 해야 합니다. 전체 Android SDK 에뮬레이터 설치 및 구성 정보를 참조 하십시오. [Android 에뮬레이터 설정](~/android/get-started/installation/android-emulator/index.md)합니다.

마모 가상 장치를 만들 때 Android 착용 장치 프로필을 선택 합니다. (예: **Android 마모 정사각형**). 성능 향상된을 위해 사용 된 마모 **x86** 이 예제와 같이 CPU/ABI:

[![마모 가상 장치 구성의 예](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png#lightbox)


## <a name="launch-the-wear-virtual-device"></a>마모 가상 장치 시작 

쓰는 유형 Android 가상 장치를 만든 후 디버깅을 시작 하기 전에 IDE에서 장치 풀 다운 메뉴에서 선택할 수 있습니다. 가상 장치 풀 다운 장치에서 사용할 수 없는 경우 프로젝트는 Android 인지 확인 *착용* 가상 장치로 수준 응용 프로그램 프로젝트 (Android 앱 프로젝트가 아닌 함)와 대상 API 레벨 동일한 API로 설정 되어 있습니다. 예를 들어:

[![Visual Studio 장치 메뉴에 쓰는 유형 AVD 선택](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png#lightbox)

Android 에뮬레이터 시작 된 후 Xamarin.Android 에뮬레이터에 마모 응용 프로그램을 배포 합니다. 에뮬레이터가 구성된 가상 장치 이미지로 앱을 실행합니다.

이 예상한 것 (또는 다른 중간 화면) 처음에 있습니다. 조사식 에뮬레이터를 시작 하려면 시간이 오래 걸릴 수 있습니다. 

![에뮬레이터에 분만 표시 됩니다. 보기...](debug-on-emulator-images/please-wait.png)

에뮬레이터는 계속 실행될 수 있습니다. 즉 앱을 실행할 때마다 매번 종료하고 다시 시작하지 않아도 됩니다.

 
## <a name="summary"></a>요약
 
이 가이드는 마모 개발을 위한 Android 에뮬레이터를 구성 하 고 디버깅을 위해 마모 가상 장치 시작 하는 방법을 설명 합니다.
