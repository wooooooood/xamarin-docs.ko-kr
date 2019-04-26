---
title: 어떤 Xamarin.Android 버전에서 Lollipop 지원이 추가되었나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7e31f9ad46a04b648a6a1f24c075426f7d98a663
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61228265"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>어떤 Xamarin.Android 버전에서 Lollipop 지원이 추가되었나요?

**참고:** 이 가이드는 Android L 미리 보기에 대 한 원래 작성 되었습니다.

-   [Xamarin.Android 4.17](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.17/) Android L 미리 보기 지원이 추가 되었습니다.
-   [Xamarin.Android 4.20](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.20/) Android Lollipop 지원이 추가 되었습니다.

Xamarin 능동적 Xamarin 도구의 현재 안정적인 릴리스를 지원합니다. 아래 정보는 "으로-는" 이전 버전의 도구에 대 한 합니다. Xamarin 릴리스에 대 한 최신 정보를 확인 하세요 [여기](http://releases.xamarin.com/)합니다.

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>Android L 미리 보기에서 "API 수준 21 android.jar 없음"

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

합니다 (또는 이와 유사한) 다음과 같은 오류 메시지가 표시 될 수 있습니다.

```cmd
Error 1 Could not find android.jar for API Level 21.
```

이 메시지는 API 수준 21 용 Android SDK 플랫폼이 설치 되어 있지 않음을 의미 합니다. Android SDK Manager에서 설치 하거나 (**도구 > Android SDK 관리자 열기...** ), 또는 설치 된 API 버전을 대상으로 Xamarin.Android 프로젝트를 변경 합니다.

이 문제에 대 한 몇 가지 대안 가지가 있습니다.

1. API 19 대상으로 하도록 프로젝트를 변경 하거나, 줄이세요.

2. Android L. android 21에서 android 21 폴더 이름 바꾸기 (기껏해야는 임시적인 해결으로만 사용 해야 하며 작동할 수 없습니다 잘 전혀)

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. 일시적으로 Android API 수준 21 "L" 미리 보기 [1]로 다시 다운 그레이드 합니다.

    1.  삭제 된 **% LOCALAPPDATA %\\Android\\android sdk\\플랫폼\\android 21** 
    2.  [1]에 압축을 풉니다 **c:\\사용자가\\&lt;username&gt;\\AppData\\로컬\\Android\\android sdk\\플랫폼**  만들려면를 **android-L** 폴더입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

합니다 (또는 이와 유사한) 다음과 같은 오류 메시지가 표시 될 수 있습니다.

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

즉, API 수준 21 용 Android SDK 플랫폼이 설치 되어 있지 않습니다. Android SDK Manager에서 설치 하거나 (도구 > SDK Manager...), 또는 설치 된 API 버전을 대상으로 Xamarin.Android 프로젝트를 변경 합니다.

이 문제에 대 한 몇 가지 대안 가지가 있습니다.

1. API 19 대상으로 하도록 프로젝트를 변경 하거나, 줄이세요.

2. Android L. android 21에서 android 21 폴더 이름 바꾸기 (기껏해야는 임시적인 해결으로만 사용 해야 하며 작동할 수 없습니다 잘 전혀)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. 일시적으로 Android API 수준 21 "L" 미리 보기 [1]로 다시 다운 그레이드 합니다.

    1.  삭제 **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**
    2.  [1]에 압축을 풉니다 **/Users/username/Library/Developer/Xamarin/android-sdk-macosx** 만들려는 **android-L** 폴더입니다.

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
