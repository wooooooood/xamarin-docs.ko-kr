---
title: 디버깅
description: Xamarin.Android 앱을 테스트하고 디버그하는 방법
ms.topic: article
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/16/2018
ms.openlocfilehash: 61d7bb1b84b225146c5ebc67e9dc89b2b17a8c21
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2018
---
# <a name="debugging"></a>디버깅

## <a name="debugging-overview"></a>디버깅 개요

Android 응용 프로그램을 개발하려면 물리적 하드웨어에서 또는 에뮬레이터나 시뮬레이터를 사용하여 응용 프로그램을 실행하는 과정이 필요합니다. 하드웨어를 사용하는 것이 가장 좋은 방법이지만 이것이 실용적이지 않은 경우가 있습니다. 많은 경우 아래 설명되어 있는 에뮬레이터 중 하나를 사용하여 Android 하드웨어를 시뮬레이션/에뮬레이션하는 것이 더 간단하고 비용 효율적일 수 있습니다.


### <a name="android-sdk-emulatorandroiddeploy-testdebuggingandroid-sdk-emulatorindexmd"></a>[Android SDK 에뮬레이터](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

이 문서에서는 Android SDK에 제공되는 기본 에뮬레이터를 사용하는 방법을 설명합니다. 이 에뮬레이터는 Windows용 Visual Studio 및 Mac용 Visual Studio에 제공됩니다.

### <a name="visual-studio-android-emulatorandroiddeploy-testdebuggingvisual-studio-android-emulatormd"></a>[Visual Studio Android Emulator](~/android/deploy-test/debugging/visual-studio-android-emulator.md)

이 문서에서는 Visual Studio 2015에 내장된 Android 에뮬레이터를 사용하여 Xamarin.Android 앱을 디버그하고 테스트하는 방법을 설명합니다. 이 에뮬레이터는 Visual Studio 2015를 사용하며 사용자 지정 장치 프로필이 필요하지 않는 경우에 적합합니다.

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[장치에서 디버깅](~/android/deploy-test/debugging/debug-on-device.md)

이 문서에서는 Xamarin.Android 응용 프로그램이 Visual Studio 또는 Mac용 Visual Studio에서 직접 배포되도록 물리적 Android 장치를 구성하는 방법을 보여줍니다.

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md)

개발자가 응용 프로그램 디버그에 사용하는 아주 일반적인 트릭은 `Console.WriteLine`을 사용하는 것입니다. 하지만 Android와 같은 모바일 플랫폼에는 콘솔이 없습니다. Android 장치에는 앱을 작성하는 동안 활용할 수 있는 로그가 제공됩니다. 이 명령은 검색을 위해 입력하는 명령 때문에 **logcat**이라고도 합니다. 이 아티클에서는 **logcat**을 사용하는 방법을 설명합니다.

> [!WARNING]
> **Xamarin Android Player**는 더 이상 사용되지 않습니다. 자세한 내용은 [블로그 게시물의 공지](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/)를 참조하세요.
