---
title: Java 시작
description: 이 문서에서는 Java를 사용 하 여 .NET 포함을 시작 하는 방법을 설명 합니다. 시스템 요구 사항, 설치 및 지원 되는 플랫폼에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
author: davidortinau
ms.author: daortin
ms.date: 03/28/2018
ms.openlocfilehash: 09ea33724c2b1184654ce7768ea1cb2525b62c28
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73007371"
---
# <a name="getting-started-with-java"></a>Java 시작

모든 지원 되는 플랫폼에 대 한 기본 사항을 설명 하는 Java 용 시작 페이지입니다.

## <a name="requirements"></a>요구 사항

Java에 .NET 포함을 사용 하려면 다음이 필요 합니다.

* Java 1.8 이상
* [Mono 5.0](https://www.mono-project.com/download/)

Mac의 경우:

* Xcode 8.3.2 이상

Windows의 경우:

* Visual Studio 2017 C++ 지원
* Windows 10 SDK

Android의 경우:

* [Xamarin Android 7.5](https://visualstudio.microsoft.com/xamarin/) 이상
* [Android Studio 3(sp3)](https://developer.android.com/studio/index.html) (Java 1.8 포함)

[Mac용 Visual Studio](https://visualstudio.microsoft.com/vs/mac/) 를 사용 하 여 C# 코드를 편집 하 고 컴파일할 수 있습니다.

> [!NOTE]
> 이전 버전의 Xcode, Visual Studio, Xamarin.ios, Android Studio 및 Mono는 작동할 _수_ 있지만 테스트 되지 않고 지원 되지 않습니다.

## <a name="installation"></a>설치

.NET 포함은 현재 [NuGet](https://www.nuget.org/packages/Embeddinator-4000/)에서 사용할 수 있습니다.

```shell
nuget install Embeddinator-4000
```

이렇게 하면 **Embeddinator-4000** 가 **패키지/Embeddinator-4000/tools** 디렉터리에 추가 됩니다.

또한 소스에서 .NET 포함을 빌드할 수 있습니다. 자세한 내용은 [git 리포지토리](https://github.com/mono/Embeddinator-4000/) 및 [기여](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md) 문서를 참조 하세요.

## <a name="platforms"></a>플랫폼

Java는 현재 macOS, Windows 및 Android에 대 한 미리 보기 상태입니다.

.NET 포함 도구에 `--platform=<platform>` 명령줄 인수를 전달 하 여 플랫폼을 선택 합니다. 현재 `macOS`, `Windows`및 `Android` 지원 됩니다.

### <a name="macos-and-windows"></a>macOS 및 Windows

개발을 위해 Java 1.8을 지 원하는 모든 Java IDE를 사용할 수 있습니다. 원하는 경우에도 Android Studio을 사용할 수 있습니다. [여기를 참조](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects)하십시오. 표준 Java JAR 파일과 마찬가지로 JAR 파일 출력을 사용할 수 있습니다.

### <a name="android"></a>Android

.NET 포함을 사용 하 여 Android 응용 프로그램을 만들기 전에 이미 Android 응용 프로그램을 개발 하도록 설정 했는지 확인 하세요. [다음 지침](~/tools/dotnet-embedding/get-started/java/android.md) 에서는 컴퓨터에서 Android 응용 프로그램을 이미 빌드 및 배포 했다고 가정 합니다.

Android Studio는 개발에 권장 되지만 [AAR 파일 형식](https://developer.android.com/studio/projects/android-library.html)에 대 한 지원이 있는 한 다른 ide는 작동 해야 합니다.

## <a name="further-reading"></a>추가 정보

* [Android 시작](~/tools/dotnet-embedding/get-started/java/android.md)
* [Android의 콜백](~/tools/dotnet-embedding/android/callbacks.md)
* [예비 Android 연구](~/tools/dotnet-embedding/android/index.md)
* [.NET 포함 제한 사항](~/tools/dotnet-embedding/limitations.md)
* [오픈 소스 프로젝트에 기여](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)
