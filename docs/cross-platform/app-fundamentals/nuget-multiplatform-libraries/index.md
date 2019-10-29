---
title: NuGet 다중 플랫폼 라이브러리 프로젝트 (즉, Nugetizer 3000)
description: 이 문서에서는 Nugetizer 3000 도구를 사용 하 여 플랫폼 간에 코드를 공유 하는 NuGet 패키지를 자동으로 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: davidortinau
ms.author: daortin
ms.date: 07/25/2018
ms.openlocfilehash: 5744bb9947b196ee319535729338bcf64a5cd09e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016755"
---
# <a name="nuget-multiplatform-library-projects-nugetizer-3000"></a>NuGet 다중 플랫폼 라이브러리 프로젝트 (Nugetizer 3000)

_' Nugetizer 3000 '를 사용 하 여 플랫폼 간에 코드를 공유 하는 NuGet 패키지를 자동으로 만듭니다._

_Nugetizer 3000_를 사용 하 여 플랫폼 간에 코드를 공유 하는 NuGet 패키지를 자동으로 만들 수 있습니다. 이렇게 하면 기존 라이브러리 프로젝트에서 또는 새 **다중 플랫폼 라이브러리 프로젝트**를 만들어 NuGet 패키지를 만들 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Nugetizer 3000는 Mac용 Visual Studio &ndash; 포함 되어 있으며, **파일 > 새** 창에서 **라이브러리 > Mulitplatform 라이브러리** 프로젝트 형식을 찾을 수 있습니다.

[![](images/mulitplatform-library-sml.png "Create new Multiplatform Library window")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 Nugetizer 3000을 사용 하려면 [VSIX 설치 관리자를 다운로드 하 여 실행](https://bit.ly/nugetizer-2017)하세요.

-----

## <a name="building-nuget-packages"></a>NuGet 패키지 빌드

구성 되 면 프로젝트의 모든 빌드는 전체 NuGet 패키지를 출력 합니다 .이 패키지는 다른 앱과 내부적으로 코드를 공유 하거나 [NuGet.org](https://www.nuget.org)에 업로드 하는 데 사용할 수 있습니다.

이 기능을 사용 하는 세 가지 시나리오가 있습니다.

- [기존 라이브러리 프로젝트](existing-library.md)

  기존 PCL 또는 .NET Standard 프로젝트에서 NuGet 패키지를 만듭니다.

- [새 다중 플랫폼 라이브러리 프로젝트 만들기](single-codebase.md)

  PCL 또는 .NET Standard를 사용 하 여 NuGet을 통해 공통 코드를 공유 하는 새 라이브러리를 만듭니다.

- [새 플랫폼별 라이브러리 프로젝트 만들기](platform-specific.md)

  IOS 및 Android 용 플랫폼별 코드를 포함 하는 새 라이브러리 및 NuGet을 만들고, 공유 프로젝트를 사용 하 여 iOS 또는 Android 관련 기능을 지 원하는 공통 코드 및 플랫폼별 프로젝트를 포함 합니다.

NuGet 패키지에 추가 해야 하는 필수 및 선택적 메타 데이터에 대 한 자세한 내용은 [메타 데이터 가이드](metadata.md) 를 참조 하세요.

## <a name="further-nuget-information"></a>추가 NuGet 정보

[Xamarin에 대 한 nuget 수동 만들기](~/cross-platform/app-fundamentals/nuget-manual.md) 및 [응용 프로그램에 NuGet 패키지를 포함](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)하는 방법에 대해 자세히 알아보세요.

Microsoft의 [Nuget 설명서](https://docs.microsoft.com/nuget/) 에는 **Nupkg** 형식 및 Visual Studio의 nuget 패키지 사용에 대 한 자세한 내용이 포함 되어 있습니다.

NuGet 패키지 프로젝트에 대 한 디자인 토론 ( NuGetizer 3000)는 [NuGet GitHub 리포지토리에서](https://github.com/NuGet/Home/wiki/NuGetizer-3000)사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [NuGetizer-3000 사용 사례](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Xamarin에 대 한 NuGet 패키지 수동 만들기](~/cross-platform/app-fundamentals/nuget-manual.md)
- [NuGet 설명서](https://docs.microsoft.com/nuget/)
