---
title: .NET Standard
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: 20bbb6386de3fb83df22de5df6e9ee99e3913eaa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="net-standard"></a>.NET Standard

## <a name="using-net-standard-library-projects-to-share-code"></a>.NET 표준 라이브러리 프로젝트를 사용 하 여 코드를 공유

.NET 표준 라이브러리는 모든 .NET 런타임에서 사용할 수 있는 .NET API의 공식 사양입니다. 표준 라이브러리는 .NET 에코시스템의 균일성을 높이기 위한 것입니다.
[ECMA 335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md)에서는 .NET 런타임 동작에 대한 균일성을 계속 설정하지만 .NET 라이브러리 구현의 .NET BCL(기본 클래스 라이브러리)에 대한 유사한 사양은 없습니다.

상상할 수 있는 것으로 간단해 진 차세대 [이식 가능한 클래스 라이브러리](https://msdn.microsoft.com/library/gg597391.aspx)합니다.
.NET Core를 포함 하 여 모든.NET 플랫폼에 대 한 균일 한 API 사용 하는 단일 라이브러리는 방금 단일.NET 표준 라이브러리를 만들고.NET 표준 플랫폼을 지 원하는 모든 런타임에서 사용 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

이 섹션을 만들고 Mac.에 대 한 Visual Studio를 사용 하 여.NET 표준 라이브러리를 사용 하는 방법을 안내합니다 완전 한 구현에 대 한.NET 표준 라이브러리 예제 단원을 참조 하십시오.

### <a name="creating-a-net-standard-library"></a>.NET 표준 라이브러리 만들기

.NET 표준 라이브러리 솔루션에 추가 매우 단순 합니다.

1. 새 프로젝트 추가 대화 상자에서 선택 된 `.NET Core` 범주 및 선택 `Class Library(.NET Core)`합니다.

  **참고:** 이 서식 파일을 이름이 변경 될 `.NET Standard` Mac.에 대 한 Visual Studio의 이후 버전에서

  ![.NET Core 클래스 라이브러리 만들기](net-standard-images/vsm01.png)

2. .NET 표준 라이브러리 프로젝트는 솔루션 탐색기에 표시 된 대로 표시 됩니다. 사용 하 여 라이브러리 종속성 노드에 표시 됩니다는 [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/)합니다.

  ![솔루션의 종속성 노드의.NET 표준 나타냅니다.](net-standard-images/vsm02.png)

#### <a name="editing-net-standard-library-settings"></a>.NET 표준 라이브러리 설정 편집

.NET 표준 라이브러리 설정 확인 및 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 변경할 수 `Options` 이 스크린 샷에 표시 된 것 처럼:

![프로젝트 옵션에는 표준.NET 대상 프레임 워크를 편집 합니다.](net-standard-images/vsm03.png)

버전을 변경할 수는 내부 `netstandard` 변경 하 여는 `Target Framework` 드롭다운 값입니다.

**또한:** 편집할 수는 `.csproj` 이 값을 변경 하려면 직접 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

이 섹션을 만들고 Visual Studio를 사용 하 여.NET 표준 라이브러리를 사용 하는 방법을 안내 합니다. 완전 한 구현에 대 한.NET 표준 라이브러리 예제 단원을 참조 하십시오.

### <a name="creating-a-net-standard-library"></a>.NET 표준 라이브러리 만들기

#### <a name="visual-studio-2017"></a>Visual Studio 2017

.NET 표준 라이브러리 솔루션에 추가 매우 단순 합니다.

1. 새 프로젝트 추가 대화 상자에서 선택 된 `.NET Standard` 범주 및 선택 `Class Library(.NET Standard)`합니다.

  ![](net-standard-images/vs01.png "새 표준.NET 클래스 라이브러리 만들기")

2. .NET 표준 라이브러리 프로젝트는 솔루션 탐색기에 표시 된 대로 표시 됩니다. 사용 하 여 라이브러리 종속성 노드에 표시 됩니다는 [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/)합니다.

  ![](net-standard-images/vs02.png "솔루션의 표준.NET 프로젝트")

#### <a name="editing-net-standard-library-settings"></a>.NET 표준 라이브러리 설정 편집

.NET 표준 라이브러리 설정 확인 및 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 변경할 수 `Properties` 이 스크린 샷에 표시 된 것 처럼:

![](net-standard-images/vs03.png "다른 프로젝트와 동일한 방식으로.NET 표준 라이브러리를 참조 합니다.")

버전을 변경할 수는 내부 `netstandard` 변경 하 여는 `Target Framework` 드롭다운 값입니다.

**또한:** 편집할 수는 `.csproj` 이 값을 변경 하려면 직접 합니다.

#### <a name="using-net-standard-library"></a>.NET 표준 라이브러리를 사용 하 여

.NET 표준 라이브러리를 만든 후 일반적으로 참조를 추가 같은 방식으로 호환 되는 모든 응용 프로그램 또는 라이브러리 프로젝트에 대 한 참조를 추가할 수 있습니다. Visual Studio에서 참조 노드를 마우스 오른쪽 단추로 클릭 하 고 선택 `Add Reference...` 다음 전환 하는 `Solution : Projects` 표시 된 것 처럼 탭:

![](net-standard-images/vs04.png "Visual Studio에서 참조 노드를 마우스 오른쪽 단추로 클릭 하 고 선택한 참조 추가... 다음 표시 된 것 처럼 솔루션 프로젝트 탭으로 전환")

-----

