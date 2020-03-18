---
title: Android SDK 위치를 어디에 설정할 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 11/16/2017
ms.openlocfilehash: 8685be4bb1cc45ff04dc8d9f7d8e64e7b1483b60
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027019"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Android SDK 위치를 어디에 설정할 수 있나요?

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio에서 **도구 > 옵션 > Xamarin > Android 설정**으로 이동하여 Android SDK 위치를 확인하고 설정합니다.

[![기본 설정의 예제 위치 탭](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

각 경로의 기본 위치는 다음과 같습니다.

- Java Development Kit 위치: 

    **C:\\Program Files\\Java\\jdk1.8.0_131**

- Android SDK 위치: 

    **C:\\Program Files (x86)\\Android\\android-sdk**

- Android NDK 위치: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

NDK의 버전 번호는 다를 수 있습니다. 예를 들어 **android-ndk-r13b** 대신 **android-ndk-r10e**와 같은 이전 버전일 수 있습니다.

Android SDK 위치를 설정하려면 **Android SDK 위치** 상자에 Android SDK 디렉터리의 전체 경로를 입력합니다. 파일 탐색기에서 Android SDK 위치로 이동하여 주소 표시줄에서 경로를 복사하고 이 경로를 **Android SDK 위치** 상자에 붙여넣을 수 있습니다.
예를 들어 Android SDK 위치가 **C:\\Users\\username\\AppData\\Local\\Android\\Sdk**에 있는 경우 **Android SDK 위치** 상자에서 이전 경로를 지우고, 이 경로를 붙여넣고, **확인**을 클릭합니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

Mac용 Visual Studio에서 **기본 설정 > 프로젝트 > SDK 위치 > Android**로 이동합니다. **Android** 페이지에서 **위치** 탭을 클릭하여 SDK 위치를 확인하고 설정합니다.

[![기본 설정의 예제 위치 탭](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

각 경로의 기본 위치는 다음과 같습니다.

- Android SDK 위치: 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Android NDK 위치: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- JDK(Java SDK) 위치: 

    **/usr**

NDK의 버전 번호는 다를 수 있습니다. 예를 들어 **android-ndk-r14b** 대신 **android-ndk-r10e**와 같은 이전 버전일 수 있습니다.

Android SDK 위치를 설정하려면 **Android SDK 위치** 상자에 Android SDK 디렉터리의 전체 경로를 입력합니다. Finder에서 Android SDK 폴더를 선택하고, **CTRL+&#8984;+I**를 눌러 폴더 정보를 확인하고, **위치:** 의 오른쪽에 있는 경로를 클릭하고 드래그하여 복사한 다음, **위치** 탭의 **Android SDK 위치** 상자에 붙여넣습니다. 예를 들어 Android SDK 위치가 **~/Library/Developer/Android/Sdk**에 있는 경우 **Android SDK 위치** 상자에서 이전 경로를 지우고, 이 경로를 붙여넣고, **확인**을 클릭합니다.

-----
