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
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725062"
---
# <a name="which-android-sdk-packages-should-i-install"></a>어떤 Android SDK 패키지를 설치해야 하나요?

Android SDK를 설치 하면 개발에 필요한 모든 최소 패키지가 자동으로 포함 되지 않습니다. 개별 개발자 요구 사항은 다르지만 일반적으로 Xamarin.ios를 사용 하 여 개발 하려면 다음 패키지가 필요 합니다.

## <a name="tools"></a>工具

SDK manager의 Tools 폴더에서 최신 도구를 설치 합니다.

- Android SDK 工具
- Android SDK 플랫폼-도구
- Android SDK 빌드-도구

## <a name="android-platforms"></a>Android 플랫폼

최소 & 대상으로 설정한 Android 버전용 "SDK 플랫폼"을 설치 합니다.

예:

- 대상 API 23
- 최소 API 23

API 23 용 SDK 플랫폼을 설치 하기만 하면 됩니다.

- 대상 API 23
- 최소 API 15

API 15 및 23 용 SDK 플랫폼을 설치 해야 합니다. 해당 API 수준으로 이식 하는 경우에도 최소 및 대상 간에 API 수준을 설치할 필요는 없습니다.

## <a name="system-images"></a>시스템 이미지

Google에서 기본 Android 에뮬레이터를 사용 하려는 경우에만 필요 합니다. 자세한 내용은 [Android Emulator 설치](~/android/get-started/installation/android-emulator/index.md) 를 참조 하세요.

## <a name="extras"></a>기타
Android SDK 스페셜는 일반적으로 필요 하지 않습니다. 그러나 사용 사례에 따라 필요할 수 있으므로이에 대해 알고 있으면 유용 합니다.
