---
title: 어떤 Xamarin.Android 버전에서 Lollipop 지원이 추가되었나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 9e36189c771ed0c91a6030fd0ab615ab9af4dd52
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73026709"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>어떤 Xamarin.Android 버전에서 Lollipop 지원이 추가되었나요?

> [!NOTE]
> 이 가이드는 원래 Android L 미리 보기용으로 작성되었습니다.

- [Xamarin.Android 4.17](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.17/index.md)에서 Android L 미리 보기 지원이 추가되었습니다.
- [Xamarin.Android 4.20](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.20/index.md)에서 Android Lollipop 지원이 추가되었습니다.

Xamarin은 Xamarin 도구의 안정적인 현재 릴리스만 지원합니다. 아래에서 제공하는 정보는 이전 버전의 도구에 대해 “있는 그대로” 제공됩니다. Xamarin 릴리스에 대한 최신 정보는 [릴리스 정보](https://docs.microsoft.com/xamarin/whats-new/#product-release-notes)를 확인하세요.

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>Android L 미리 보기의 “API 레벨 21에 대해 누락된 android.jar”

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

다음과 같거나 비슷한 오류 메시지가 표시될 수 있습니다.

```cmd
Error 1 Could not find android.jar for API Level 21.
```

이 메시지는 API 레벨 21용 Android SDK 플랫폼이 설치되지 않았음을 의미합니다. Android SDK 관리자에 설치하거나(**도구 > Android SDK 관리자 열기...** ) 이미 설치되어 있는 API 버전을 대상으로 하도록 Xamarin.Android 프로젝트를 변경하세요.

이 문제는 다음과 같은 몇 가지 방법으로 해결할 수 있습니다.

1. 프로젝트가 API 19 이하를 대상으로 하도록 변경합니다.

2. android-21 폴더의 이름을 android-21에서 android-L로 바꿉니다. (이 방법은 일시적인 수정으로만 사용해야 하며 전혀 작동하지 않을 수도 있습니다.)

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. 일시적으로 Android API 레벨 21 “L” 미리 보기[1]로 다운그레이드합니다.

    1. **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**을 삭제합니다. 
    2. [1]을 **C:\\사용자\\&lt;사용자 이름&gt;\\AppData\\Local\\Android\\android-sdk\\platforms**에 추출하여 **android-L** 폴더를 만듭니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

다음과 같거나 비슷한 오류 메시지가 표시될 수 있습니다.

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

이는 API 레벨 21용 Android SDK 플랫폼이 설치되지 않았음을 의미합니다. Android SDK 관리자에 설치하거나(도구 > SDK 관리자...) 이미 설치된 API 버전을 대상으로 하도록 Xamarin.Android 프로젝트를 변경하세요.

이 문제는 다음과 같은 몇 가지 방법으로 해결할 수 있습니다.

1. 프로젝트가 API 19 이하를 대상으로 하도록 변경합니다.

2. android-21 폴더의 이름을 android-21에서 android-L로 바꿉니다. (이 방법은 일시적인 수정으로만 사용해야 하며 전혀 작동하지 않을 수도 있습니다.)

   **~/라이브러리/Developer/Xamarin/android-sdk-macosx/android-21**

3. 일시적으로 Android API 레벨 21 “L” 미리 보기[1]로 다운그레이드합니다.

    1. **/사용자/사용자 이름/라이브러리/Developer/Xamarin/android-sdk-macosx/android-21**을 삭제합니다.
    2. [1]을 **/사용자/사용자 이름/라이브러리/Developer/Xamarin/android-sdk-macosx**에 추출하여 **android-L** 폴더를 만듭니다.

-----

[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
