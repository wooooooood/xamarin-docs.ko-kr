---
title: 어떤 Android SDK 패키지를 설치해야 하나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2018
ms.openlocfilehash: 24c70c2e869f59091a1519af6d1165dbea9cc467
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "76725062"
---
# <a name="which-android-sdk-packages-should-i-install"></a>어떤 Android SDK 패키지를 설치해야 하나요?

Android SDK를 설치하면 개발에 필요한 최소 패키지가 자동으로 모두 포함되지는 않습니다. 개발자마다 요구 사항이 다르지만, Xamarin.Android를 사용하여 개발하는 데 일반적으로 다음 패키지가 필요합니다.

## <a name="tools"></a>도구

SDK 관리자의 Tools 폴더에서 최신 도구를 설치합니다.

- Android SDK Tools
- Android SDK Platform-Tools
- Android SDK Build-Tools

## <a name="android-platforms"></a>Android 플랫폼

최소 및 대상으로 설정한 Android 버전용 "SDK 플랫폼"을 설치합니다.

예:

- 대상 API 23
- 최소 API 23

API 23용 SDK 플랫폼만 설치하면 됩니다.

- 대상 API 23
- 최소 API 15

API 15 및 23용 SDK 플랫폼을 설치해야 합니다. 해당 API 수준으로 이식하는 경우에도 최소와 대상 사이의 API 수준을 설치할 필요는 없습니다.

## <a name="system-images"></a>시스템 이미지

Google에서 기본 Android 에뮬레이터를 사용하려는 경우에만 필요합니다. 자세한 내용은 [Android Emulator 설정](~/android/get-started/installation/android-emulator/index.md)을 참조하세요.

## <a name="extras"></a>Extras
Android SDK Extras는 일반적으로 필요하지 않습니다. 그러나 사용 사례에 따라 필요할 수 있으므로 이에 대해 알고 있으면 유용합니다.
