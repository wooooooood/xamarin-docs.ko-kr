---
title: .NET 포함 설치
description: 이 문서에서는 .NET 포함을 설치 하는 방법을 설명 합니다. 도구를 직접 실행 하는 방법, 바인딩을 자동으로 생성 하는 방법, 사용자 지정 MSBuild 대상 사용 방법 및 필요한 빌드 후 단계에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
author: chamons
ms.author: chhamo
ms.date: 04/18/2018
ms.openlocfilehash: 47efbaa12475f627b5963cb6613c3441a1d96aac
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227848"
---
# <a name="installing-net-embedding"></a>.NET 포함 설치

## <a name="installing-net-embedding-from-nuget"></a>NuGet에서 .NET 포함 설치

**추가 > nuget 패키지 추가** ...를 선택 하 고 nuget 패키지 관리자에서 **Embeddinator-4000** 을 설치 합니다.

![NuGet 패키지 관리자](images/visualstudionuget.png)

이렇게 하면 **Embeddinator-4000** 및 **objcgen** 가 **패키지/Embeddinator-4000/tools** 디렉터리에 설치 됩니다.

일반적으로 Embeddinator-4000의 최신 릴리스를 다운로드 하도록 선택 해야 합니다. 목표-C 지원에는 0.4 이상이 필요 합니다.

## <a name="running-manually"></a>수동으로 실행

이제 NuGet이 설치 되었으므로 도구를 직접 실행할 수 있습니다.

- 터미널 (macOS) 또는 명령 프롬프트를 엽니다 (Windows).
- 디렉터리를 솔루션 루트로 변경
- 도구는 다음 위치에 설치 됩니다.
  - **./packages/Embeddinator-4000. [VERSION]/tools/objcgen** (목적-C)
  - **./packages/Embeddinator-4000. [VERSION]/tools/Embeddinator-4000.exe** (Java/C)
- MacOS에서 **objcgen** 는 직접 실행할 수 있습니다.
- Windows에서는 **Embeddinator-4000** 을 직접 실행할 수 있습니다.
- MacOS에서 Embeddinator-4000는 **mono**를 사용 하 여 실행 해야 **합니다** .
  - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

각 명령 호출에는 플랫폼별 설명서에 나열 된 많은 매개 변수가가 필요 합니다.

## <a name="automatic-binding-generation"></a>자동 바인딩 생성

빌드 프로세스의 .NET 포함 부분을 자동으로 실행 하는 방법에는 두 가지가 있습니다.

- 사용자 지정 MSBuild 대상
- 빌드 후 단계

이 문서에서는 두 가지 방법에 대해 설명 하지만 사용자 지정 MSBuild 대상을 사용 하는 것이 좋습니다. 명령줄에서 빌드할 때 빌드 후 단계가 반드시 실행 되는 것은 아닙니다.

### <a name="custom-msbuild-targets"></a>사용자 지정 MSBuild 대상

MSbuild 대상을 사용 하 여 빌드를 사용자 지정 하려면 먼저 다음과 같이 .csproj 옆에 **Embeddinator** 파일을 만듭니다.

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

여기서는 플랫폼별 설명서에 나열 된 .NET 포함 호출 중 하나를 사용 하 여 명령 텍스트를 채워야 합니다.

이 대상을 사용 하려면:

- Visual Studio 2017 또는 Mac용 Visual Studio에서 프로젝트를 닫습니다.
- 텍스트 편집기에서 라이브러리 .csproj를 엽니다.
- 최종 `</Project>` 줄의 맨 아래에 다음 줄을 추가 합니다.

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- 프로젝트 다시 열기

### <a name="post-build-steps"></a>빌드 후 단계

.NET 포함을 실행 하기 위한 빌드 후 단계를 추가 하는 단계는 IDE에 따라 다릅니다.

#### <a name="visual-studio-for-mac"></a>Mac용 Visual Studio

Mac용 Visual Studio에서 프로젝트 옵션으로 이동 하 여 **사용자 지정 명령 > 빌드하고** **빌드 후** 단계를 추가 > 합니다.

플랫폼별 설명서에 나열 된 명령을 설정 합니다.

> [!NOTE]
> NuGet에서 설치한 버전 번호를 사용 해야 합니다.

C# 프로젝트에 대 한 지속적인 개발을 수행 하려는 경우 .net 포함을 실행 하기 전에 **출력** 디렉터리를 정리 하는 사용자 지정 명령을 추가할 수도 있습니다.

```shell
rm -Rf '${SolutionDir}/output/'
```

![사용자 지정 빌드 작업](images/visualstudiocustombuild.png)

> [!NOTE]
> 생성 된 바인딩은 `--outdir` 또는 `--out` 매개 변수로 표시 되는 디렉터리에 배치 됩니다. 일부 플랫폼에는 패키지 이름에 대 한 제한이 있으므로 생성 된 바인딩 이름은 다를 수 있습니다.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

기본적으로 동일한 작업을 설정 하지만 Visual Studio 2017의 메뉴는 약간 다릅니다. 셸 명령도 약간 다릅니다.

프로젝트 옵션으로 이동 하 여 **빌드 이벤트 >** 플랫폼별 설명서에 나열 된 명령을 **빌드 후 이벤트 명령줄** 상자에 입력 합니다. 예:

![Windows에 .NET 포함](images/visualstudiowindows.png)
 