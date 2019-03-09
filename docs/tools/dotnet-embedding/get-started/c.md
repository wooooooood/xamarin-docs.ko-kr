---
title: C 시작
description: 이 문서에는 C 응용 프로그램에서.NET 코드를 포함 하려면 포함 하는.NET을 사용 하는 방법을 설명 합니다. Mac 용 Visual Studio 2017 및 Visual Studio에서.NET 포함을 사용 하는 방법에 설명
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
author: lobrien
ms.author: laobri
ms.date: 04/19/2018
ms.openlocfilehash: 66475b7629f01b4229539e1edc323491a7861dfe
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668402"
---
# <a name="getting-started-with-c"></a>C 시작

## <a name="requirements"></a>요구 사항

를 사용 하려면.NET 포함 C를 사용 하 여 Mac 또는 Windows 컴퓨터 실행을 해야 합니다.

### <a name="macos"></a>macOS

* macOS 10.12(sierra) 이상
* Xcode 8.3.2 이상
* [Mono](https://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7, 8, 10 이상
* Visual Studio 2015 이상

## <a name="installing-net-embedding-from-nuget"></a>NuGet에서 포함 하는.NET 설치

따르세요 [지침](~/tools/dotnet-embedding/get-started/install/install.md) 를 설치 하 여 프로젝트에 대 한.NET 포함을 구성 합니다.

구성 해야 하는 명령 호출 (가능한 경우 버전 번호가 다른 경로와) 같이 표시 됩니다.

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

정상적으로 작동 하는 경우 다음과 같은 출력이 나타납니다.

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

있으므로 합니다 `--compile` 플래그 도구에 전달 된,.NET 포함 해야도 컴파일한 출력 파일이 생성된 된 파일 옆에 있는 있습니다 공유 라이브러리로 **libmanaged.dylib** macOS에대한파일**managed.dll** Windows에서.

공유 라이브러리를 사용 하려면 포함 된 **managed.h** C 헤더 파일에 해당 하는 각 C 선언을 제공 하는 Api 라이브러리를 관리 하 고 앞에서 언급 한 링크 공유 라이브러리를 컴파일한 합니다.

## <a name="further-reading"></a>추가 정보

* [.NET 포함 제한 사항](~/tools/dotnet-embedding/limitations.md)
* [오픈 소스 프로젝트 기여](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)
