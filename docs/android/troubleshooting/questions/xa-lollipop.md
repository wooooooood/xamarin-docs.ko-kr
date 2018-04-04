---
title: Xamarin.Android의 버전 롤리팝 지원이 추가 되었습니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 065c68a373f67bb352b59dc88ef89daec8b51ef8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>Xamarin.Android의 버전 롤리팝 지원이 추가 되었습니다.

**참고:** 이 가이드는 Android L 미리 보기에 대 한 원래 작성 되었습니다.

-   [4.17 Xamarin.Android](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.17/) Android L 미리 보기 지원이 추가 되었습니다.
-   [Xamarin.Android 4.20](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.20/) Android 롤리팝 지원이 추가 되었습니다.

Xamarin만 적극적으로 현재 안정적인 버전의 Xamarin 도구를 지원합니다. 아래 정보는 "으로-는" 이전 버전의 도구에 대 한 합니다. Xamarin 릴리스에 대 한 최신 정보를 확인 하십시오 [여기](http://releases.xamarin.com/)합니다.

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>Android L 미리 보기에서 "에 대 한 API 수준 21 android.jar 없음"

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

(또는 유사한 곳) 다음 오류 메시지가 표시 될 수 있습니다.

```cmd
Error 1 Could not find android.jar for API Level 21.
```

이 메시지는 Android SDK 플랫폼 API 수준 21 용 설치 되어 있지 않음을 의미 합니다. Android SDK Manager를 설치 하거나 (도구 > 열기 Android SDK Manager...), 또는 설치 된 API 버전을 대상 Xamarin.Android 프로젝트를 변경 합니다.

이 문제에 대 한 몇 가지 대안 가지가 있습니다.

1. API 19을 대상으로 하도록 프로젝트를 변경 하거나 절감 합니다.

2. Android-21 편지함 android 21에서 android 12. 이름 바꾸기 (가장 좋은 경우 임시 수정과 사용 해야 하며 수 하지 작동 상태일 때 잘 전혀.)

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. Android API 수준 21 "L" 미리 보기 [1]로 다시 일시적으로 다운 그레이드 합니다.

    1.  삭제 된 **% LOCALAPPDATA %\\Android\\android sdk\\플랫폼\\android 21** 
    2.  [1]로 추출 **c:\\사용자\\<username>\\AppData\\로컬\\Android\\android sdk\\플랫폼** 만들려는 **android-L** 폴더입니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

(또는 유사한 곳) 다음 오류 메시지가 표시 될 수 있습니다.

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

즉, Android SDK 플랫폼 API 수준 21 용 설치 되어 있지 않습니다. Android SDK Manager를 설치 하거나 (도구 > SDK Manager...), 또는 설치 된 API 버전을 대상 Xamarin.Android 프로젝트를 변경 합니다.

이 문제에 대 한 몇 가지 대안 가지가 있습니다.

1. API 19을 대상으로 하도록 프로젝트를 변경 하거나 절감 합니다.

2. Android-21 편지함 android 21에서 android 12. 이름 바꾸기 (가장 좋은 경우 임시 수정과 사용 해야 하며 수 하지 작동 상태일 때 잘 전혀.)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Android API 수준 21 "L" 미리 보기 [1]로 다시 일시적으로 다운 그레이드 합니다.

    1.  Delete **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**
    2.  [1]로 추출 **/Users/username/Library/Developer/Xamarin/android-sdk-macosx** 만들려는 **android-L** 폴더입니다.

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
