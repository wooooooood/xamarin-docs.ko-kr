---
title: C 시작
description: 이 문서에서는 .NET 포함을 사용 하 여 C 응용 프로그램에 .NET 코드를 포함 하는 방법을 설명 합니다. Visual Studio 2019 및 Mac용 Visual Studio에서 .NET 포함을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
author: conceptdev
ms.author: crdun
ms.date: 04/19/2018
ms.openlocfilehash: 1dc68a709f8e1f864961bbe87af112b648b0dd2a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278735"
---
# <a name="getting-started-with-c"></a>C 시작

## <a name="requirements"></a>요구 사항

C에 .NET 포함을 사용 하려면 다음을 실행 하는 Mac 또는 Windows 컴퓨터가 필요 합니다.

### <a name="macos"></a>macOS

* macOS 10.12 (시에라리온) 이상
* Xcode 8.3.2 이상
* [Mono](https://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7, 8, 10 이상
* Visual Studio 2015 이상

## <a name="installing-net-embedding-from-nuget"></a>NuGet에서 .NET 포함 설치

프로젝트에 대 한 .NET 포함을 설치 하 고 구성 하려면 다음 [지침](~/tools/dotnet-embedding/get-started/install/install.md) 을 따르세요.

구성 해야 하는 명령 호출은 다음과 같습니다 (버전 번호 및 경로가 다를 수 있음).

### <a name="visual-studio-for-mac"></a>Mac용 Visual Studio

```shell
mono {SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=macos --compile managed.dll
```

### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
$(SolutionDir)\packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=windows --compile managed.dll
```

## <a name="generation"></a>생성

### <a name="output-files"></a>출력 파일

모든 것이 제대로 진행 되 면 다음과 같은 출력이 표시 됩니다.

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

플래그가 도구에 전달 되었으므로 .net 포함은 출력 파일을 공유 라이브러리로 컴파일해야 합니다 .이 라이브러리는 생성 된 파일, macos의 dylib 파일 및 Windows의 **관리 되** 는 파일 옆에서 찾을 수 있습니다. `--compile`

공유 라이브러리를 사용 하려면 관리 되는 각 라이브러리 Api에 해당 하는 C 선언을 제공 하 고 이전에 언급 한 컴파일된 공유 라이브러리와 연결 되는 **관리 되는 .h** c 헤더 파일을 포함할 수 있습니다.

## <a name="further-reading"></a>추가 정보

* [.NET 포함 제한 사항](~/tools/dotnet-embedding/limitations.md)
* [오픈 소스 프로젝트에 기여](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)
