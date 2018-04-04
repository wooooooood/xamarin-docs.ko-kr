---
title: C 시작
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 8dff45de6de7c9492b199f323656778ac5c34d57
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-c"></a>C 시작


## <a name="requirements"></a>요구 사항

C로 포함 하는.NET을 사용 하려면 Mac 또는 Windows 실행 하는 컴퓨터에 필요 합니다.

* macOS 10.12 (시에라) 이상
* Xcode 8.3.2 이상 버전

* Windows 7, 8, 10 이상
* Visual Studio 2015 이상

* [Mono](http://www.mono-project.com/download/)


## <a name="installation"></a>설치

다음 단계는 다운로드 하 고 컴퓨터에 포함 하는.NET 도구를 설치 합니다.

C 및 Java 생성기에 대 한 이진 빌드 곧 출시 예정인 있지만 여전히 사용할 수 없습니다.

참조에서 git 리포지토리를 사용 하 여 빌드할 수는 대신는 [기여](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) 지침에 대 한 문서입니다.


## <a name="generation"></a>생성

C 코드를 생성 하려면 C 언어를 대상으로 오른쪽 플래그를 전달 하는 포함 하는.NET 도구를 호출 합니다.

### <a name="windows"></a>Windows:

```csharp
$ build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=windows --compile managed.dll
```

호출 해야 Visual Studio 명령 셸에서 특정 Visual Studio 버전에 있습니다 대상으로 하는 것입니다.

### <a name="macos"></a>macOS

```csharp
$ mono build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=macos --compile managed.dll
```

### <a name="output-files"></a>출력 파일

모든 코드가 정상적으로 작동 하는 경우 다음과 같은 출력이 나타납니다.

```csharp
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

이후는 `--compile` 플래그 도구에 전달 된,.NET 포함 해야 또한 컴파일한 출력 파일 생성된 된 파일 옆에 찾을 수 있는 공유 라이브러리에 `libmanaged.dylib` macOS 등의 파일 및 `managed.dll` Windows에서 합니다.

공유 라이브러리를 포함할 수 있습니다를 사용 하는 `managed.h` C 헤더 파일에 해당 하는 각 C 선언 제공 하는 관리 되는 Api 라이브러리 및 링크 합니다. 앞에서 언급 한 컴파일된 공유 라이브러리입니다.

## <a name="further-reading"></a>추가 정보

* [Embeddinator 제한 사항](~/tools/dotnet-embedding/limitations.md)
* [오픈 소스 프로젝트에 기여 하](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)
