---
title: Android를 포함 하는.NET
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: lobrien
ms.author: laobri
ms.date: 06/15/2018
---

# <a name="net-embedding-on-android"></a>Android를 포함 하는.NET

일부 경우에는 기존 네이티브 Android 프로젝트에는 Xamarin.NET 라이브러리를 추가 하는 것이 좋습니다. 이 위해 사용할 수 있습니다 합니다 [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/) 도구를 네이티브 Java 기반 Android 앱에 통합할 수 있는 네이티브 라이브러리를.NET 라이브러리를 설정 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android 요구 사항

.NET 포함 하려면 Xamarin.Android에 대 한 다음이 필요 합니다.

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) 나중에 설치 해야 합니다.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) 나중에 설치 해야 합니다.

-   **Java Developer Kit** &ndash; [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 나중에 설치 해야 합니다.


## <a name="using-embeddinator-4000"></a>Embeddinator 4000을 사용 하 여

네이티브 Android 프로젝트의.NET 라이브러리를 사용 하려면 다음 단계를 사용 합니다.

1.  만들기는 C# Android 라이브러리 프로젝트.

2.  설치할 [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/)합니다.

3.  찾습니다 **Embeddinator 4000.exe** 에 추가 하 여 **경로**합니다. 예를 들어:

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4.  라이브러리 어셈블리에 Embeddinator 4000을 실행 합니다. 예를 들어:

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Android Studio에서 Java 프로젝트에서 생성된 된 AAR 파일을 사용 합니다.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android 요구 사항

.NET 포함 하려면 Xamarin.Android에 대 한 다음이 필요 합니다.

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) 나중에 설치 해야 합니다.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) 나중에 설치 해야 합니다.

-   **Java Developer Kit** &ndash; [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 나중에 설치 해야 합니다.

-   **Mono** &ndash; [Mono 5.0](https://www.mono-project.com/download/) 나중에 설치 해야 합니다 (mono는 Mac 용 Visual Studio와 함께 설치 됨).


## <a name="using-embeddinator-4000"></a>Embeddinator 4000을 사용 하 여

네이티브 Android 프로젝트의.NET 라이브러리를 사용 하려면 다음 단계를 사용 합니다.

1.  만들기는 C# Android 라이브러리 프로젝트.

2.  설치할 [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/)합니다.

3.  찾습니다 **Embeddinator 4000.exe** 추가한 **mono** 경로에 있습니다. 예를 들어:

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4.  라이브러리 어셈블리에 Embeddinator 4000을 실행 합니다. 예를 들어:

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Android Studio에서 Java 프로젝트에서 생성된 된 AAR 파일을 사용 합니다.

-----

사용량 및 명령줄 옵션에 설명 되어는 [Embeddinator 4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c) 설명서.


## <a name="callbacks"></a>콜백

에 대 한 자세한 [간의 호출 C# 및 Java](callbacks.md)합니다.

## <a name="samples"></a>샘플

* [날씨 샘플 앱](https://github.com/jamesmontemagno/embeddinator-weather)
