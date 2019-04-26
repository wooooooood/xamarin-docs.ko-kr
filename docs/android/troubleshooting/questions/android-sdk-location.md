---
title: Android SDK 위치를 어디에 설정할 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 11/16/2017
ms.openlocfilehash: c004fb7717f78750e7ac1e8dc1856a32ba808638
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61227688"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Android SDK 위치를 어디에 설정할 수 있나요?

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio로 이동 **도구 > 옵션 > Xamarin > Android 설정** 검토 하 고 Android SDK 위치를 설정 합니다.

[![기본 설정 예제에서는 위치 탭](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

각 경로 대 한 기본 위치는 다음과 같습니다.

- Java Development Kit 위치: 

    **C:\\Program Files\\Java\\jdk1.8.0_131**

- Android SDK 위치: 

    **C:\\Program Files (x86)\\Android\\android-sdk**

- Android NDK 위치: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

NDK의 버전 번호가 달라질 수 있는 참고 합니다. Of 예를 들어 **r13b-android-ndk**와 같은 이전 버전을 수 있습니다 **r10e ndk-android-** 합니다.

Android SDK 위치를 설정 하려면 Android SDK 디렉터리의 전체 경로 입력 합니다 **Android SDK 위치** 상자입니다. 파일 탐색기에서 Android SDK 위치를 탐색, 주소 표시줄에서 경로 복사 및에이 경로 붙여 넣습니다 합니다 **Android SDK 위치** 상자입니다.
예를 들어 Android SDK 위치에 있으면 **c:\\사용자\\username\\AppData\\로컬\\Android\\Sdk**, 합니다 이전경로선택취소 **Android SDK 위치** 상자에이 경로 붙여 넣고 클릭 **확인**합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac 용 Visual Studio로 이동 **기본 설정 > 프로젝트 > SDK 위치 > Android**합니다. 에 **Android** 페이지를 클릭 합니다 **위치** 보고 SDK 위치를 설정 하는 탭:

[![기본 설정 예제에서는 위치 탭](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

각 경로 대 한 기본 위치는 다음과 같습니다.

- Android SDK 위치: 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Android NDK 위치: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Java SDK (JDK) 위치: 

    **/usr**

NDK의 버전 번호가 달라질 수 있는 참고 합니다. Of 예를 들어 **r14b-android-ndk**와 같은 이전 버전을 수 있습니다 **r10e ndk-android-** 합니다.

Android SDK 위치를 설정 하려면 Android SDK 디렉터리의 전체 경로 입력 합니다 **Android SDK 위치** 상자입니다. Finder에서 키를 눌러 Android SDK 폴더를 선택할 수 있습니다 **CTRL +&#8984;+ I** 폴더 정보를 보려면 클릭 하 고 오른쪽의 경로 끌어 **위치:** 를 복사한 다음 붙여 넣습니다 합니다 **Android SDK 위치** 상자에 **위치** 탭 합니다. 예를 들어 Android SDK 위치에 있는 경우 **~/Library/Developer/Android/Sdk**, 이전 경로에서 선택을 취소 합니다 **Android SDK 위치** 상자에이 경로 붙여 넣고 클릭 **확인**.

-----
