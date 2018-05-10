---
title: 포함 하는.NET 설치
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
author: chamons
ms.author: chhamo
ms.date: 4/18/2018
ms.openlocfilehash: 1675889dceb1d364abe74461b32aa4c895a144a0
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="installing-net-embedding"></a>포함 하는.NET 설치

## <a name="installing-net-embedding-from-nuget"></a>NuGet에서 포함 하는.NET 설치

선택 **추가 > NuGet 패키지 추가 중...**  설치 **Embeddinator 4000** NuGet 패키지 관리자에서:

![NuGet 패키지 관리자](images/visualstudionuget.png)

이렇게 하면 설치 됩니다 **Embeddinator 4000.exe** 및 **objcgen** 에 **패키지/Embeddinator-4000/도구** 디렉터리입니다.

일반적으로 다운로드 하기 위한 Embeddinator 4000 최신 릴리스를 선택 해야 합니다. Objective C을 지원 하려면 0.4 이상.

## <a name="running-manually"></a>수동으로 실행

NuGet를 설치 하 여 손으로 도구를 실행할 수 있습니다.

- 터미널 (macOS)를 열거나 명령 프롬프트 (Windows)
- 솔루션의 루트 디렉터리 변경
- 도구에 설치 됩니다.
    - **. / 패키지/Embeddinator부터 4000 까지입니다. [버전] / 도구/objcgen** (Objective C)
    - **. / 패키지/Embeddinator부터 4000 까지입니다. [VERSION]/tools/Embeddinator-4000.exe** (Java/C) 
- MacOS 등에서 **objcgen** 직접 실행할 수 있습니다. 
- Windows에서는 **Embeddinator 4000.exe** 직접 실행할 수 있습니다.
- MacOS 등에서 **Embeddinator 4000.exe** 으로 실행 해야 **모노**: 
    - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

각 명령 호출에는 다양 한 플랫폼 관련 설명서에 나열 된 매개 변수가 필요 합니다.

## <a name="automatic-binding-generation"></a>자동 바인딩 생성

자동으로 빌드 프로세스의 일부를 포함 하는.NET을 실행 하는 데는 다음과 같은 두 가지가 있습니다.

- 사용자 지정 MSBuild 대상
- 빌드 후 단계

이 문서는 모두를 설명 하지만 사용자 지정 MSBuild 대상을 사용 하는 것이 좋습니다. 명령줄에서 빌드할 때 post 빌드 단계는 반드시 실행 되지 않습니다.

### <a name="custom-msbuild-targets"></a>사용자 지정 MSBuild 대상

MSbuild 대상 된 빌드를 사용자 지정 하려면 먼저 만듭니다는 **Embeddinator 4000.targets** 다음과 유사한 프로그램 csproj 옆에 있는 파일:

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

여기에서 플랫폼 관련 설명서에 나열 된.NET 포함 호출 중 하나를 사용 하 여 명령의 텍스트 채워야 합니다.

사용 하려면이 대상:

- Mac 용 Visual Studio 2017 또는 Visual Studio에서 프로젝트를 닫습니다.
- 텍스트 편집기에서 라이브러리 csproj을 엽니다.
- 마지막으로 위에서 맨 아래에 다음이 줄 추가 `</Project>` 줄:

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- 프로젝트를 다시 여세요.

### <a name="post-build-steps"></a>빌드 후 단계

.NET 포함 실행 되도록 빌드 후 단계를 추가 하는 단계는 IDE에 따라 달라 집니다.

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

Mac 용 Visual Studio에서로 이동 **프로젝트 옵션 > 빌드 > 사용자 지정 명령** 추가 **빌드 후** 단계입니다.

플랫폼 특정 문서에 나열 된 명령을를 설정 합니다.

> [!NOTE]
> NuGet에서 설치할 버전 번호를 사용 하 고 있는지 확인

C# 프로젝트에서 개발 작업을 하려는 경우 정리 하는 사용자 지정 명령을 추가할 수 있습니다는 **출력** .NET 포함를 실행 하기 전에 디렉터리:

```shell
rm -Rf '${SolutionDir}/output/'
```

![사용자 지정 빌드 작업](images/visualstudiocustombuild.png)

> [!NOTE]
> 으로 지정 된 디렉터리에 생성 된 바인딩을 표시 됩니다는 `--outdir` 또는 `--out` 매개 변수입니다. 일부 플랫폼 패키지 이름에 대 한 제한이으로 생성 된 바인딩 이름을 달라질 수 있습니다.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

기본적으로 동일한 작업을 설정 해 드립니다 하지만 Visual Studio 2017에 메뉴 약간 다릅니다. 또한 셸 명령을 약간 다릅니다.

로 이동 **프로젝트 옵션 > 빌드 이벤트** 에 플랫폼 관련 설명서에 나열 된 명령을 입력 하 고 **빌드 후 이벤트 명령줄** 상자입니다. 예를 들어:

![.NET Windows에 포함](images/visualstudiowindows.png)
