---
title: "내 Android SDK 위치 설정"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 11/16/2017
ms.openlocfilehash: 0113cc15bf1de5e0e668b05c2b0288a6ead141b5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>내 Android SDK 위치 설정

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio에서로 이동 **도구 > 옵션 > Xamarin > Android 설정** 검토 하 고 Android SDK 위치를 설정 합니다.

[![기본 설정 예제 위치 탭](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

각 경로 대 한 기본 위치는 다음과 같습니다.

- Java 개발 키트 위치: 

    **C:\\Program Files\\Java\\jdk1.8.0_131**

- Android SDK 위치: 

    **C:\\Program Files (x86)\\Android\\android-sdk**

- Android NDK 위치: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

참고 하 여 NDK의 버전 번호가 다를 수 있습니다. 예를 들어 대신의 **android-ndk-r13b**와 같은 이전 버전을 수 있습니다 **android-ndk-r10e**합니다.

Android SDK 위치를 설정 하려면 Android SDK 디렉터리의 전체 경로 입력의 **Android SDK 위치** 상자입니다. 파일 탐색기에서 Android SDK 위치로 이동, 주소 표시줄에서 경로 복사 및에이 경로 붙여 넣을 수는 **Android SDK 위치** 상자입니다.
예를 들어, Android SDK 위치에 있으면 **c:\\사용자\\username\\AppData\\로컬\\Android\\Sdk**, 이전 경로 의선택을취소 **Android SDK 위치** 상자이 경로에 붙여 넣고 클릭 **확인**합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 용 Visual Studio에서로 이동 **기본 설정 > 프로젝트 > SDK 위치 > Android**합니다. 에 **Android** 페이지는 **위치** 보고는 SDK 위치를 설정 하는 탭:

[![기본 설정 예제 위치 탭](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

각 경로 대 한 기본 위치는 다음과 같습니다.

- Android SDK 위치: 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Android NDK 위치: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Java SDK (JDK) 위치: 

    **/usr**

참고 하 여 NDK의 버전 번호가 다를 수 있습니다. 예를 들어 대신의 **android-ndk-r14b**와 같은 이전 버전을 수 있습니다 **android-ndk-r10e**합니다.

Android SDK 위치를 설정 하려면 Android SDK 디렉터리의 전체 경로 입력의 **Android SDK 위치** 상자입니다. Finder에서 키를 눌러 Android SDK 폴더를 선택 하면 **CTRL +&#8984;+ I** 폴더 정보를 보려면 클릭 하 고 오른쪽의 경로 끕니다 **여기서:**를 복사한 다음 붙여 넣어를 **Android SDK 위치** 상자에 **위치** 탭 합니다. 예를 들어, Android SDK 위치에 있으면 **~/Library/Developer/Android/Sdk**, 이전 경로에서 선택을 취소는 **Android SDK 위치** 상자이 경로에 붙여 넣고 클릭 **확인**.

-----
