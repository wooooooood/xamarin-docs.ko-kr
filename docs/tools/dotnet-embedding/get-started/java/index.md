---
title: Java 시작
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: e61e610de9186978e2924c0e69e7517a39a54f04
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-java"></a>Java 시작

이 지원 되는 모든 플랫폼에 대 한 기본 사항에 설명 하는 Java 용 시작된 페이지입니다.

## <a name="requirements"></a>요구 사항

사용 하면.NET 포함 Java 필요 합니다.

* Java 1.8 이상
* [모노 5.0](http://www.mono-project.com/download/)

For Mac:

* Xcode 8.3.2 이상 버전

Windows:

* C + +를 지 원하는 visual Studio 2017
* Windows 10 SDK

Android:

* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) 이상 버전
* [Android Studio 3.x](https://developer.android.com/studio/index.html) 1.8 Java와 함께

사용할 수 있습니다 [Mac 용 Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/) 를 편집 하 여 C# 코드를 컴파일합니다.

> [!NOTE]
> 이전 버전의 Xcode, Visual Studio, Xamarin.Android, Android Studio 및 모노 _수_ 작동 않지만 거치지 이며 지원 되지 않습니다.

## <a name="installation"></a>설치

현재 사용할 수는 포함 하는.NET [NuGet](https://www.nuget.org/packages/Embeddinator-4000/):

```shell
nuget install Embeddinator-4000
```

이 **Embeddinator 4000.exe** 에 **패키지/Embeddinator-4000/도구** 디렉터리입니다.

빌드 소스에서 포함 하는.NET, 참조 수 또한 우리의 [git 리포지토리](https://github.com/mono/Embeddinator-4000/) 및 [기여](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md) 지침에 대 한 문서입니다.

## <a name="platforms"></a>플랫폼

Java 현재 macOS, Windows 및 Android에 대 한 미리 보기 상태입니다.

플랫폼은 전달 하 여 선택 하는 `--platform=<platform>` 명령줄 인수를 포함 하는.NET 도구입니다. 현재 `macOS`, `Windows`, 및 `Android` 지원 됩니다.

### <a name="macos-and-windows"></a>macOS 및 창

개발을 위한 Java 1.8을 지 원하는 모든 Java IDE를 사용할 수 있습니다. Android Studio이 사용할 수 있습니다, 원하는 경우 [여기에서 볼](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects)합니다. 모든 표준 Java jar 파일 JAR 파일 출력을 사용할 수 있습니다.

### <a name="android"></a>Android

Android 응용 프로그램을 하나를 만들기 전에 개발 되도록 이미 설정 되어 있는지 확인 하십시오.NET 포함을 사용 하 여 합니다. [지침에 따라](~/tools/dotnet-embedding/get-started/java/android.md) 있고 라고 가정 하면 이미 성공적으로 작성 된 컴퓨터에서 Android 응용 프로그램을 배포 합니다.

Android Studio는 개발, 좋지만 기타 Ide에 대 한 지원은으로 작동 해야는 [AAR 파일 형식](https://developer.android.com/studio/projects/android-library.html)합니다.

## <a name="further-reading"></a>추가 정보

* [Android에서 시작](~/tools/dotnet-embedding/get-started/java/android.md)
* [Android에서 콜백](~/tools/dotnet-embedding/android/callbacks.md)
* [사전 Android 연구](~/tools/dotnet-embedding/android/index.md)
* [.NET 포함 제한 사항](~/tools/dotnet-embedding/limitations.md)
* [오픈 소스 프로젝트에 기여 하](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)
