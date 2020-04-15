---
title: 디바이스 및 에뮬레이터에서 Xamarin.Android 디버그
description: Xamarin.Android 앱을 테스트하고 디버그하는 방법
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 3b3fa14ec81bd4f06322197b7140654f9086ce73
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "75556485"
---
# <a name="debug-xamarinandroid-apps"></a>Xamarin Android 앱 디버그

이 섹션에서는 디바이스 또는 에뮬레이터에서 Xamarin.Android 앱을 디버깅하는 방법을 설명합니다.

## <a name="debugging-overview"></a>디버깅 개요

Android 애플리케이션을 개발하려면 물리적 하드웨어에서 또는 에뮬레이터를 사용하여 애플리케이션을 실행하는 과정이 필요합니다. 하드웨어를 사용하는 것이 가장 좋은 방법이지만 이것이 실용적이지 않은 경우가 있습니다. 많은 경우 아래 설명되어 있는 에뮬레이터 중 하나를 사용하여 Android 하드웨어를 시뮬레이션/에뮬레이션하는 것이 더 간단하고 비용 효율적일 수 있습니다.

### <a name="debugging-on-the-android-emulator"></a>[Android Emulator에서 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)

이 아티클에서는 Visual Studio에서 Android Emulator를 시작하고 가상 디바이스에서 앱을 시작하는 방법을 설명합니다.

### <a name="debugging-on-a-device"></a>[디바이스에서 디버깅](~/android/deploy-test/debugging/debug-on-device.md)

이 문서에서는 Xamarin.Android 애플리케이션이 Visual Studio 또는 Mac용 Visual Studio에서 직접 배포되도록 물리적 Android 디바이스를 구성하는 방법을 보여줍니다.

### <a name="android-debug-log"></a>[Android 디버그 로그](~/android/deploy-test/debugging/android-debug-log.md)

개발자가 애플리케이션 디버그에 사용하는 아주 일반적인 트릭은 `Console.WriteLine`을 사용하는 것입니다. 하지만 Android와 같은 모바일 플랫폼에는 콘솔이 없습니다. Android 디바이스에는 앱을 작성하는 동안 활용할 수 있는 로그가 제공됩니다. 이 명령은 검색을 위해 입력하는 명령 때문에 **logcat**이라고도 합니다. 이 아티클에서는 **logcat**을 사용하는 방법을 설명합니다.
