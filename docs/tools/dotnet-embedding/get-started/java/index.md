---
title: Java를 사용 하 여 시작
description: 이 문서에서는.NET 포함을 사용 하 여 Java를 사용 하 여 시작 하는 방법을 설명 합니다. 시스템 요구 사항, 설치 및 지원 되는 플랫폼에 설명 합니다.
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
author: lobrien
ms.author: laobri
ms.date: 03/28/2018
ms.openlocfilehash: 79a483743946c4f7509833867f2afe4b1e055183
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61198944"
---
# <a name="getting-started-with-java"></a>Java를 사용 하 여 시작

이 모든 지원 되는 플랫폼에 대 한 기본 사항을 다루는 Java에 대 한 시작된 페이지를 합니다.

## <a name="requirements"></a>요구 사항

.NET 포함 해야 하는 Java를 사용 하 여 사용 하려면 필요 합니다.

* Java 1.8 이상
* [Mono 5.0](https://www.mono-project.com/download/)

Mac:

* Xcode 8.3.2 이상

Windows에 대 한:

* Visual Studio 2017은 C++ 지원
* Windows 10 SDK

Android:

* [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) 이상
* [Android Studio 3.x](https://developer.android.com/studio/index.html) Java 1.8 사용 하 여

사용할 수 있습니다 [Mac 용 Visual Studio](https://visualstudio.microsoft.com/vs/mac/) 편집한 컴파일에 C# 코드입니다.

> [!NOTE]
> 이전 버전의 Xcode, Visual Studio, Xamarin.Android, Android Studio 및 Mono _수_ 작동 하지만 테스트 되지 않음 되며 지원 되지 않습니다.

## <a name="installation"></a>설치

.NET 포함 하는 것은 현재 사용할 수 [NuGet](https://www.nuget.org/packages/Embeddinator-4000/):

```shell
nuget install Embeddinator-4000
```

이 배치할 **Embeddinator 4000.exe** 에 **패키지/Embeddinator-4000/도구** 디렉터리입니다.

.NET 원본에서 포함을 빌드, 참조 수는 또한 우리의 [git 리포지토리](https://github.com/mono/Embeddinator-4000/) 하며 [영향을 주는](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md) 지침에 대 한 문서.

## <a name="platforms"></a>플랫폼

Java는 현재 macOS, Windows, Android 용 미리 보기 상태입니다.

플랫폼을 전달 하 여 선택 된 `--platform=<platform>` .NET 포함 도구에 명령줄 인수입니다. 현재 `macOS`하십시오 `Windows`, 및 `Android` 지원 됩니다.

### <a name="macos-and-windows"></a>macOS 및 Windows

개발을 위한 Java 1.8을 지 원하는 Java IDE를 사용할 수 있습니다. 이 Android Studio를 사용 하도 원한다 면 [여기](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects)합니다. 모든 표준 Java jar 파일에 출력 JAR 파일을 사용할 수 있습니다.

### <a name="android"></a>Android

만들 하려고 하기 전에 Android 응용 프로그램을 개발 하도록 이미 설정 되었는지 확인 하십시오.NET 포함을 사용 합니다. 합니다 [지침에 따라](~/tools/dotnet-embedding/get-started/java/android.md) 이미 성공적으로 빌드된 있고 컴퓨터에서 Android 응용 프로그램 배포를 가정 합니다.

Android Studio 개발용으로 권장 되지만 다른 Ide에 대 한 지원은으로 작동 해야 합니다 [AAR 파일 형식을](https://developer.android.com/studio/projects/android-library.html)합니다.

## <a name="further-reading"></a>추가 정보

* [Android 시작](~/tools/dotnet-embedding/get-started/java/android.md)
* [Android에서 콜백](~/tools/dotnet-embedding/android/callbacks.md)
* [예비 Android 조사](~/tools/dotnet-embedding/android/index.md)
* [.NET 포함 제한 사항](~/tools/dotnet-embedding/limitations.md)
* [오픈 소스 프로젝트 기여](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)
