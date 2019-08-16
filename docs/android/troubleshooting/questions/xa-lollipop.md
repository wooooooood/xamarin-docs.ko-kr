---
title: 어떤 Xamarin.Android 버전에서 Lollipop 지원이 추가되었나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 4fe1bd4dda9a54eb3a1692f07d1069adb39345cb
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69523284"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>어떤 Xamarin.Android 버전에서 Lollipop 지원이 추가되었나요?

> [!NOTE]
> 이 가이드는 원래 Android L preview 용으로 작성 되었습니다.

- [Xamarin android 4.17](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.17/index.md) 는 Android L Preview 지원을 추가 했습니다.
- [Xamarin android 4.20](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.20/index.md) 는 android 롤리팝 지원을 추가 했습니다.

Xamarin은 Xamarin 도구의 현재 안정적인 릴리스만 적극적으로 지원 합니다. 아래 정보는 이전 버전의 도구에 대해 "있는 그대로" 제공 됩니다. Xamarin 릴리스에 대 한 최신 정보는 [여기](http://releases.xamarin.com/)를 확인 하세요.

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>Android L Preview의 "API 수준 21 용 android jar 없음"

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

다음 오류 메시지 (또는 유사)가 표시 될 수 있습니다.

```cmd
Error 1 Could not find android.jar for API Level 21.
```

이 메시지는 API 레벨 21에 대 한 Android SDK 플랫폼이 설치 되어 있지 않음을 의미 합니다. Android SDK Manager (**도구 > Android SDK 관리자 ...** )에 설치 하거나, 설치 된 API 버전을 대상으로 하는 Xamarin Android 프로젝트를 변경 합니다.

이 문제에 대 한 몇 가지 해결 방법이 있습니다.

1. API 19이 하를 대상으로 하도록 프로젝트를 변경 합니다.

2. Android-21에서 android-L로 android-21 폴더의 이름을 바꿉니다. (최상의 방법은 임시 수정 으로만 사용 해야 하며 전혀 작동 하지 않을 수 있습니다.)

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. 일시적으로 Android API 수준 21 "L" preview [1]로 다시 다운 그레이드:

    1. **\\% LOCALAPPDATA% android\\android\\-sdk platform\\android-21** 을 삭제 합니다. 
    2. [1]을 **C: Users\\&lt;\\username\\AppData Local android android-sdk\\플랫폼으로 추출 하 여 만듭니다.\\\\&gt;\\** **android-L** 폴더입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

다음 오류 메시지 (또는 유사)가 표시 될 수 있습니다.

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

즉, API 레벨 21에 대 한 Android SDK 플랫폼이 설치 되어 있지 않습니다. Android SDK Manager (도구 > SDK Manager ...)에 설치 하거나, 설치 된 API 버전을 대상으로 하는 Xamarin Android 프로젝트를 변경 합니다.

이 문제에 대 한 몇 가지 해결 방법이 있습니다.

1. API 19이 하를 대상으로 하도록 프로젝트를 변경 합니다.

2. Android-21에서 android-L로 android-21 폴더의 이름을 바꿉니다. (최상의 방법은 임시 수정 으로만 사용 해야 하며 전혀 작동 하지 않을 수 있습니다.)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. 일시적으로 Android API 수준 21 "L" preview [1]로 다시 다운 그레이드:

    1. **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21** 삭제
    2. **/Users/username/Library/Developer/Xamarin/android-sdk-macosx** 에 [1]을 (를) 추출 하 여 **android-L** 폴더를 만듭니다.

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
