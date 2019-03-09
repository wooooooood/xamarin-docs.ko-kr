---
title: 포함 하는.NET 설치
description: 이 문서에서는.NET 포함을 설치 하는 방법을 설명 합니다. 한편으로 도구를 실행 하는 방법에 설명 바인딩을 생성 하는 방법을 사용자 지정 MSBuild 대상 및 필요한 빌드 후 단계를 사용 하는 방법을 자동으로 합니다.
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
author: chamons
ms.author: chhamo
ms.date: 04/18/2018
ms.openlocfilehash: 2a572748c21d2a640add3346d1162f4b6bdc8e99
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668441"
---
# <a name="installing-net-embedding"></a>포함 하는.NET 설치

## <a name="installing-net-embedding-from-nuget"></a>NuGet에서 포함 하는.NET 설치

선택 **추가 > NuGet 패키지 추가...**  하 고 설치 **Embeddinator 4000** NuGet 패키지 관리자에서:

![NuGet 패키지 관리자](images/visualstudionuget.png)

이렇게 하면 설치 됩니다 **Embeddinator 4000.exe** 하 고 **objcgen** 에 **패키지/Embeddinator-4000/도구** 디렉터리.

일반적으로 최신 릴리스의 Embeddinator-4000 다운로드에 대 한 선택 해야 합니다. Objective-c 지원을 위해서는 0.4 이상.

## <a name="running-manually"></a>수동으로 실행

NuGet 설치 했으므로 직접 도구를 실행할 수 있습니다.

- (MacOS) 터미널 또는 명령 프롬프트 (Windows) 열기
- 솔루션 루트에 디렉터리를 변경
- 도구에 설치 됩니다.
    - **. / 패키지/Embeddinator부터 4000 까지입니다. [VERSION] / 도구/objcgen** (Objective-c)
    - **. / 패키지/Embeddinator부터 4000 까지입니다. [VERSION]/tools/Embeddinator-4000.exe** (Java/C)
- Macos에서는 **objcgen** 직접 실행할 수 있습니다.
- Windows에 온 **Embeddinator 4000.exe** 직접 실행할 수 있습니다.
- Macos에서는 **Embeddinator 4000.exe** 사용 하 여 실행 해야 **mono**:
    - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

각 명령 호출 다양 한 플랫폼 관련 설명서에 나열 된 매개 변수가 필요 합니다.

## <a name="automatic-binding-generation"></a>자동 바인딩 생성

자동으로 빌드 프로세스의 일부.NET 포함을 실행 하는 방법은 두 가지가 있습니다.

- 사용자 지정 MSBuild 대상
- 빌드 후 단계

이 문서는 모두를 설명 하는 동안 사용자 지정 MSBuild 대상을 사용 하는 것이 좋습니다. 명령줄에서 빌드할 때 사후 빌드 단계는 반드시 실행 되지 않습니다.

### <a name="custom-msbuild-targets"></a>사용자 지정 MSBuild 대상

MSbuild 대상으로 사용 하 여 빌드를 사용자 지정 하려면 먼저 만듭니다는 **Embeddinator 4000.targets** 다음과 유사한 csproj에 옆에 있는 파일:

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

여기에서 플랫폼별 설명서에 나열 된.NET 포함 호출 중 하나를 사용 하 여 명령의 텍스트 채워야 합니다.

사용 하려면이 대상:

- Mac 용 Visual Studio 2017 또는 Visual Studio에서 프로젝트를 닫습니다.
- 라이브러리 csproj 텍스트 편집기에서 열기
- 최종 위에서 맨 아래에 다음이 줄 추가 `</Project>` 줄:

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- 프로젝트를 다시 엽니다.

### <a name="post-build-steps"></a>빌드 후 단계

.NET 포함 실행할 빌드 후 단계를 추가 하는 단계는 IDE에 따라 달라 집니다.

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

Mac 용 Visual Studio로 이동 **프로젝트 옵션 > 빌드 > 사용자 지정 명령** 추가한는 **빌드 후** 단계입니다.

플랫폼 관련 설명서에 나열 된 명령을 설정 합니다.

> [!NOTE]
> NuGet에서 설치한 버전 번호를 사용 해야 합니다.

진행 중인 개발에서 수행 하려는 경우는 C# 프로젝트를 추가할 수 있습니다도 정리를 사용자 지정 명령 합니다 **출력** .NET 포함을 실행 하기 전에 디렉터리:

```shell
rm -Rf '${SolutionDir}/output/'
```

![사용자 지정 빌드 작업](images/visualstudiocustombuild.png)

> [!NOTE]
> 생성 된 바인딩 하 여 지정 된 디렉터리에 배치 됩니다 합니다 `--outdir` 또는 `--out` 매개 변수입니다. 일부 플랫폼 패키지 이름에 대 한 제한 사항으로 생성 된 바인딩 이름을 달라질 수 있습니다.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

동일한 작업을 기본적으로 설정 됩니다 있지만 Visual Studio 2017에서 메뉴 약간 다릅니다. 셸 명령 또한 약간 다릅니다.

로 이동 **프로젝트 옵션 > 빌드 이벤트** 에 플랫폼 특정 설명서에 나열 된 명령을 입력 합니다 **빌드 후 이벤트 명령줄** 상자입니다. 예를 들어:

![Windows에 포함 하는.NET](images/visualstudiowindows.png)
