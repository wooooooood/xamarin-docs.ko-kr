---
title: Android를 포함 하는.NET
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: topgenorth
ms.author: toopge
ms.date: 06/15/2018
ms.openlocfilehash: e90d1e6258d4cfd9c918c566c9e18c358ee7668a
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067352"
---
# <a name="net-embedding-on-android"></a>Android를 포함 하는.NET

경우에 따라 기존 네이티브 Android 프로젝트에 Xamarin.NET 라이브러리를 추가 하는 것이 좋습니다. 이 위해 사용할 수 있습니다는 [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/) 을 네이티브 Java 기반 Android 앱에 통합할 수 있는 네이티브 라이브러리를.NET 라이브러리는 도구입니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android 요구 사항

포함 하는.NET 작업할 Xamarin.Android에 대 한 다음이 필요 합니다.

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) 나중에 설치 되어 있어야 합니다.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) 나중에 설치 되어 있어야 합니다.

-   **Java 개발자 키트** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 나중에 설치 되어 있어야 합니다.


## <a name="using-embeddinator-4000"></a>Embeddinator 4000를 사용 하 여

네이티브 Android 프로젝트에서.NET 라이브러리를 사용 하려면 다음 단계를 사용 합니다.

1.  C# Android 라이브러리 프로젝트를 만듭니다.

2.  설치 [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/)합니다.

3.  찾을 **Embeddinator 4000.exe** 에 추가 하 여 **경로**합니다. 예를 들어:

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4.  라이브러리 어셈블리에서 Embeddinator 4000를 실행 합니다. 예를 들어:

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Android Studio에서 Java 프로젝트에서 생성 된 AAR 파일을 사용 합니다.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android 요구 사항

포함 하는.NET 작업할 Xamarin.Android에 대 한 다음이 필요 합니다.

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) 나중에 설치 되어 있어야 합니다.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) 나중에 설치 되어 있어야 합니다.

-   **Java 개발자 키트** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 나중에 설치 되어 있어야 합니다.

-   **모노** &ndash; [모노 5.0](http://www.mono-project.com/download/) 나중에 설치 되어 있어야 (모노는 Mac에 대 한 Visual Studio와 함께 설치 됨).


## <a name="using-embeddinator-4000"></a>Embeddinator 4000를 사용 하 여

네이티브 Android 프로젝트에서.NET 라이브러리를 사용 하려면 다음 단계를 사용 합니다.

1.  C# Android 라이브러리 프로젝트를 만듭니다.

2.  설치 [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/)합니다.

3.  찾을 **Embeddinator 4000.exe** 추가 **모노** 를 사용자의 경로입니다. 예를 들어:

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4.  라이브러리 어셈블리에서 Embeddinator 4000를 실행 합니다. 예를 들어:

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Android Studio에서 Java 프로젝트에서 생성 된 AAR 파일을 사용 합니다.

-----

사용 및 명령줄 옵션에 설명 되어는 [Embeddinator 4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c) 설명서입니다.


## <a name="callbacks"></a>콜백

에 대 한 자세한 내용은 [C#과 Java 간의 호출](callbacks.md)합니다.

## <a name="samples"></a>샘플

* [날씨 샘플 응용 프로그램](https://github.com/jamesmontemagno/embeddinator-weather)
