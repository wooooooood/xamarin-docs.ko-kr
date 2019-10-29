---
title: Android에 .NET 포함
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: davidortinau
ms.author: daortin
ms.date: 06/15/2018
ms.openlocfilehash: fef422b799ab5280aef205f4d5e55fd91050da39
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73007333"
---
# <a name="net-embedding-on-android"></a>Android에 .NET 포함

일부 경우에 기존 네이티브 Android 프로젝트에 Xamarin .NET 라이브러리를 추가 하는 것이 좋습니다. 이렇게 하려면 [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/) 도구를 사용 하 여 네이티브 Java 기반 Android 앱에 통합할 수 있는 네이티브 라이브러리로 .net 라이브러리를 전환할 수 있습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="xamarinandroid-requirements"></a>Xamarin Android 요구 사항

Xamarin.ios가 .NET 포함을 사용 하려면 다음이 필요 합니다.

- **Xamarin.ios &ndash;** [xamarin. android 7.5](https://visualstudio.microsoft.com/xamarin/) 이상이 설치 되어 있어야 합니다.

- **Android Studio** &ndash;   [Android Studio 3.x](https://developer.android.com/studio/) 이상 버전을 설치해야 합니다.

- **Java 개발자 키트** &ndash;   [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 이상을 설치해야 합니다.

## <a name="using-embeddinator-4000"></a>Embeddinator 사용-4000

네이티브 Android 프로젝트에서 .NET 라이브러리를 사용 하려면 다음 단계를 사용 합니다.

1. Android 라이브러리 C# 프로젝트를 만듭니다.

2. [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/)을 설치 합니다.

3. **Embeddinator-4000** 를 찾아서 **경로**에 추가 합니다. 예를 들면,

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4. 라이브러리 어셈블리에서 Embeddinator-4000를 실행 합니다. 예를 들면,

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5. Android Studio의 Java 프로젝트에서 생성 된 AAR 파일을 사용 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="xamarinandroid-requirements"></a>Xamarin Android 요구 사항

Xamarin.ios가 .NET 포함을 사용 하려면 다음이 필요 합니다.

- **Xamarin.ios &ndash;** [xamarin. android 7.5](https://visualstudio.microsoft.com/xamarin/) 이상이 설치 되어 있어야 합니다.

- **Android Studio** &ndash;   [Android Studio 3.x](https://developer.android.com/studio/) 이상 버전을 설치해야 합니다.

- **Java 개발자 키트** &ndash;   [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 이상을 설치해야 합니다.

- **Mono** &ndash;   [Mono 5.0](https://www.mono-project.com/download/) 이상이 설치되어 있어야 합니다(mono는 Mac용 Visual Studio와 함께 설치됨).

## <a name="using-embeddinator-4000"></a>Embeddinator 사용-4000

네이티브 Android 프로젝트에서 .NET 라이브러리를 사용 하려면 다음 단계를 사용 합니다.

1. Android 라이브러리 C# 프로젝트를 만듭니다.

2. [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/)을 설치 합니다.

3. **Embeddinator-4000** 를 찾고 경로에 **mono** 를 추가 합니다. 예를 들면,

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4. 라이브러리 어셈블리에서 Embeddinator-4000를 실행 합니다. 예를 들면,

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5. Android Studio의 Java 프로젝트에서 생성 된 AAR 파일을 사용 합니다.

-----

사용 및 명령줄 옵션은 [Embeddinator-4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c) 설명서에 설명 되어 있습니다.

## <a name="callbacks"></a>콜백

[및 Java를 호출 하는 방법에 대해 알아봅니다. C# ](callbacks.md)

## <a name="samples"></a>샘플

- [날씨 샘플 앱](https://github.com/jamesmontemagno/embeddinator-weather)
