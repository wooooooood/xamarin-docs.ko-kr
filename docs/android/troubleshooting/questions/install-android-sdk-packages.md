---
title: Android SDK 패키지를 설치 해야 하나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2018
ms.openlocfilehash: 04a07d5b7f37222515136e5592f31a4583b02fe3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107536"
---
# <a name="which-android-sdk-packages-should-i-install"></a>Android SDK 패키지를 설치 해야 하나요?

Android SDK 설치 개발 하기 위한 최소 필수 패키지를 모두 자동으로 포함 되지 않습니다. 개별 개발자 다르지만 해야 다음 패키지를 일반적으로 Xamarin.Android를 사용 하 여 개발 하기 위해 필요한 됩니다.

## <a name="tools"></a>도구

SDK manager에서 도구 폴더에서 최신 도구를 설치 합니다.

- Android SDK Tools
- Android SDK 플랫폼 도구
- Android SDK 빌드 도구

## <a name="android-platforms"></a>Android 플랫폼

최소 대상으로 설정한 Android 버전에 대 한 "SDK 플랫폼"을 설치 합니다. 

예를 들면 다음과 같습니다.

- 대상 API 23
- 최소 API 23

API 23에 대 한 SDK 플랫폼을 설치 하기만

- 대상 API 23
- 최소 API 15

API 15 및 23에 대 한 SDK 플랫폼을 설치 해야 합니다. 참고 (하더라도 해당 API 수준으로 후반) 대상 사이의 최소 API 수준을 설치할 필요가 없습니다.

## <a name="system-images"></a>시스템 이미지

만 Google에서 기본 제공 Android 에뮬레이터를 사용 하려는 경우 필요 합니다. 자세한 내용은 참조 하세요. [Android Emulator 설정](~/android/get-started/installation/android-emulator/index.md)

## <a name="extras"></a>Extras
Android SDK Extras 일반적으로 필요 하지 않습니다. 하지만 사용 사례에 따라 필요한 수 있으므로 이러한 알아야 할 것이 유용 합니다.

## <a name="further-reading"></a>추가 정보
다음 가이드에서는 이러한 옵션 및 자세히 여러 패키지에 대 한 SDK manager에 사용할 수 있습니다: [Android SDK Manager 설치 가이드](http://www.themethodology.net/2015/02/android-sdk-manager-setup-for.html?m=1)

