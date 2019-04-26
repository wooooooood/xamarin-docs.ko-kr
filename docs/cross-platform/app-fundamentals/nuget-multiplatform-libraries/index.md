---
title: NuGet 다중 플랫폼 라이브러리 프로젝트 (즉, Nugetizer 3000)
description: 이 문서에서는 플랫폼 간에 코드를 공유 하는 NuGet 패키지를 자동으로 만들려면 Nugetizer 3000 도구를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 6d3f7b316e397705ecb9bd404007dcd9ef5aa183
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266789"
---
# <a name="nuget-multiplatform-library-projects-nugetizer-3000"></a>NuGet 다중 플랫폼 라이브러리 프로젝트 (Nugetizer 3000)

_' Nugetizer 3000'를 사용 하 여 플랫폼 간에 코드를 공유 하는 NuGet 패키지를 자동으로 만듭니다._

자동으로 사용 하 여 플랫폼 간에 코드를 공유 하는 NuGet 패키지를 만드는 것이 불가능 합니다 _Nugetizer 3000_합니다. 이렇게이 하면 새 또는 기존 라이브러리 프로젝트에서 NuGet 패키지를 만들 수는 **다중 플랫폼 라이브러리 프로젝트**합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Nugetizer 3000 Mac 용 Visual Studio에 포함 된 &ndash; 검색할 합니다 **라이브러리 > Mulitplatform 라이브러리** 프로젝트 형식에는 **파일 > 새로 만들기** 창:

[![](images/mulitplatform-library-sml.png "새 다중 플랫폼 라이브러리 창을 만듭니다.")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 Nugetizer 3000을 사용 하세요 [다운로드 하 고 VSIX 설치 관리자 실행](http://bit.ly/nugetizer-2017)합니다.

-----

## <a name="building-nuget-packages"></a>NuGet 패키지 빌드

프로젝트의 모든 빌드 출력을 완료 하는 NuGet 패키지에 업로드 하거나 다른 앱을 사용 하 여 내부적으로 코드를 공유 하는 데 사용 될 수 있는 구성 되 면 [NuGet.org](https://www.nuget.org)합니다.

이 기능을 사용 하는 방법은 세 가지 시나리오가 있습니다.

- [기존 라이브러리 프로젝트](existing-library.md)

  기존 PCL (또는.NET Standard) 프로젝트에서 NuGet 패키지를 만듭니다.

- [새 다중 플랫폼 라이브러리 프로젝트 만들기](single-codebase.md)

  새 라이브러리는 PCL 또는.NET Standard를 사용 하 여 NuGet 통해 공통 코드를 공유를 만듭니다.

- [새 플랫폼별 라이브러리 프로젝트 만들기](platform-specific.md)

  새 라이브러리 및 iOS 및 Android에 대 한 플랫폼별 코드를 포함 하 고 공유 프로젝트를 사용 하 여 iOS 또는 Android 관련 기능을 지 원하는 플랫폼 특정 프로젝트와 공통 코드를 포함 하는 NuGet을 만듭니다.

참조 된 [메타 데이터 가이드](metadata.md) 모든 NuGet 패키지에 추가 되어야 하는 필수 및 선택적 메타 데이터에 대 한 자세한 내용은 합니다.

## <a name="further-nuget-information"></a>자세한 내용은 NuGet

에 대해 자세히 알아보세요 [Xamarin 용 Nuget을 수동으로 만드는](~/cross-platform/app-fundamentals/nuget-manual.md) 하는 방법과 [NuGet 패키지를 앱에서 포함](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)합니다.

Microsoft의 [NuGet 설명서](https://docs.microsoft.com/nuget/) 에 대 한 자세한 정보가 합니다 **.nupkg** 형식 및 Visual Studio에서 NuGet 패키지를 사용 합니다.

NuGet 패키지를 프로젝트에 대 한 디자인 토론 (즉, NuGetizer 3000)에서 사용할 수는 [NuGet GitHub 리포지토리](https://github.com/NuGet/Home/wiki/NuGetizer-3000)합니다.

## <a name="related-links"></a>관련 링크

- [NuGetizer 3000 사용 사례](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Xamarin 용 NuGet 패키지를 수동으로 만들기](~/cross-platform/app-fundamentals/nuget-manual.md)
- [NuGet 설명서](https://docs.microsoft.com/nuget/)
