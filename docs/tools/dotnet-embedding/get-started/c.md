---
title: C 시작
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
author: topgenorth
ms.author: toopge
ms.date: 04/19/2018
ms.openlocfilehash: f3c238dc9805dafa922f8e32fb4b1935a3fa152c
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="getting-started-with-c"></a>C 시작

## <a name="requirements"></a>요구 사항

사용 하려면.NET 포함 C, Mac 또는 Windows 실행 하는 컴퓨터에 필요 합니다.

### <a name="macos"></a>macOS

* macOS 10.12 (시에라) 이상
* Xcode 8.3.2 이상 버전
* [Mono](http://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7, 8, 10 이상
* Visual Studio 2015 이상

## <a name="installing-net-embedding-from-nuget"></a>NuGet에서 포함 하는.NET 설치

이 따라 [지침](~/tools/dotnet-embedding/get-started/install/install.md) 설치 하 고 프로젝트에 대 한.NET 포함을 구성 합니다.

명령 호출을 구성 해야 (수 있는 버전 번호가 서로 다른 경로와) 같이 표시 됩니다.

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

```shell
mono {SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=macos --compile managed.dll
```

### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
$(SolutionDir)\packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=windows --compile managed.dll
```

## <a name="generation"></a>생성

### <a name="output-files"></a>출력 파일

모든 코드가 정상적으로 작동 하는 경우 다음과 같은 출력이 나타납니다.

```shell
Parsing assemblies...
    Parsed 'managed.dll'
Processing assemblies...
Generating binding code...
    Generated: managed.h
    Generated: managed.c
    Generated: mscorlib.h
    Generated: mscorlib.c
    Generated: embeddinator.h
    Generated: glib.c
    Generated: glib.h
    Generated: mono-support.h
    Generated: mono_embeddinator.c
    Generated: mono_embeddinator.h
```

이후는 `--compile` 플래그 도구에 전달 된,.NET 포함 해야 또한 컴파일한 출력 파일 생성된 된 파일 옆에 찾을 수 있는 공유 라이브러리에 **libmanaged.dylib** macOS 등 상의파일**managed.dll** Windows에서 합니다.

공유 라이브러리를 사용 하려면 포함는 **managed.h** 에 해당 하는 각 C 선언 제공 하는 C 헤더 파일에 관리 되는 Api 라이브러리 및 링크 합니다. 앞에서 언급 한 컴파일된 공유 라이브러리입니다.

## <a name="further-reading"></a>추가 정보

* [.NET 포함 제한 사항](~/tools/dotnet-embedding/limitations.md)
* [오픈 소스 프로젝트에 기여 하](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)
