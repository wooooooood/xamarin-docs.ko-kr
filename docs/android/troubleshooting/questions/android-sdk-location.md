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
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027019"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Android SDK 위치를 어디에 설정할 수 있나요?

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 **도구 > 옵션 > Xamarin > Android 설정** 으로 이동 하 여 Android SDK 위치를 확인 하 고 설정 합니다.

[기본 설정의 예제 위치 탭![](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

각 경로에 대 한 기본 위치는 다음과 같습니다.

- Java Development Kit 위치: 

    **C:\\프로그램 파일\\Java\\jdk 1.8.0 _131**

- Android SDK 위치: 

    **C:\\Program Files (x86)\\Android\\android-sdk**

- Android NDK 위치: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

NDK의 버전 번호는 다를 수 있습니다. 예를 들어 **r13b**대신 **android-ndk-r10e**와 같은 이전 버전이 될 수 있습니다.

Android SDK 위치를 설정 하려면 **Android SDK location** 상자에 Android SDK 디렉터리의 전체 경로를 입력 합니다. 파일 탐색기에서 Android SDK 위치로 이동 하 여 주소 표시줄에서 경로를 복사 하 고이 경로를 **Android SDK 위치** 상자에 붙여 넣을 수 있습니다.
예를 들어 Android SDK 위치가 **C:\\사용자\\사용자 이름\\AppData\\로컬\\Android\\SDK**인 경우 **Android SDK 위치** 상자에서 이전 경로를 선택 취소 하 고이 경로를 붙여넣습니다. 을 클릭 하 고 **확인**을 클릭 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac용 Visual Studio에서 **기본 설정 > 프로젝트 > SDK 위치 > Android**로 이동 합니다. **Android** 페이지에서 **위치** 탭을 클릭 하 여 SDK 위치를 확인 하 고 설정 합니다.

[기본 설정의 예제 위치 탭![](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

각 경로에 대 한 기본 위치는 다음과 같습니다.

- Android SDK 위치: 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Android NDK 위치: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- JDK (Java SDK) 위치: 

    **/usr**

NDK의 버전 번호는 다를 수 있습니다. 예를 들어 **r14b**대신 **android-ndk-r10e**와 같은 이전 버전이 될 수 있습니다.

Android SDK 위치를 설정 하려면 **Android SDK location** 상자에 Android SDK 디렉터리의 전체 경로를 입력 합니다. Finder에서 Android SDK 폴더를 선택 하 고 **ctrl&#8984;+ + I** 를 눌러 폴더 정보를 확인 한 다음 경로를 클릭 **하 여 위치 탭의** **Android SDK 위치** 상자에 붙여넣을 수 **있습니다.** 예를 들어 Android SDK 위치가 **~/Library/Developer/Android/Sdk**인 경우 **Android SDK 위치** 상자에서 이전 경로를 지우고이 경로를 붙여넣고 **확인**을 클릭 합니다.

-----
