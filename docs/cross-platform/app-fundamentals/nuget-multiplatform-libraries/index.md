---
title: "NuGet 프로젝트 (Nugetizer 3000)"
description: "NuGet 패키지 ' Nugetizer 3000'를 사용 하 여 플랫폼 간 코드 공유를 자동으로 만듭니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: 49e7c00feb697d25d61a5e09b051c41945c260c6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="nuget-projects-nugetizer-3000"></a>NuGet 프로젝트 (Nugetizer 3000)

_NuGet 패키지 ' Nugetizer 3000'를 사용 하 여 플랫폼 간 코드 공유를 자동으로 만듭니다._

NuGet 패키지를 사용 하 여 플랫폼 간 코드 공유를 자동으로 만들 수는 _Nugetizer 3000_합니다. 이렇게이 하면 새 또는 기존 라이브러리 프로젝트에서 NuGet 패키지를 만들 수는 **다중 플랫폼 라이브러리 프로젝트**합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Nugetizer 3000 Mac 6.2에 대 한 Visual Studio와 함께 포함 되어 있습니다.

[![](images/mulitplatform-library-sml.png "새 다중 플랫폼 라이브러리 창 만들기")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio에서 Nugetizer 3000을 사용 하려면 하세요 [다운로드 하 고 VSIX 설치 관리자 실행](http://bit.ly/nugetizer-2017)합니다.

-----

## <a name="building-nuget-packages"></a>NuGet 패키지 빌드

프로젝트의 모든 빌드 출력에 업로드 하거나 다른 앱과 내부적으로 코드를 공유 하는 데 사용 될 수 있는 전체 NuGet 패키지를 구성한 후 [NuGet.org](https://www.nuget.org)합니다.

이 기능을 사용 하기 위한 세 가지 시나리오가 있습니다.

- [기존 라이브러리 프로젝트](existing-library.md)

  PCL (또는 표준.NET)에 대 한 기존 프로젝트에서 NuGet 패키지를 만듭니다.

- [새 다중 플랫폼 라이브러리 프로젝트 만들기](single-codebase.md)

  일반적인 코드는 PCL 또는 표준.NET을 사용 하 여 NuGet 통해 공유에 새 라이브러리를 만듭니다.

- [새 플랫폼 관련 라이브러리 프로젝트 만들기](platform-specific.md)

  새 라이브러리 및 iOS 및 Android에 대 한 플랫폼별 코드를 포함 하 고 공유 프로젝트를 사용 하 여 iOS 또는 Android 관련 기능을 지 원하는 플랫폼 관련 프로젝트와 공통 코드를 포함 하는 NuGet을 만듭니다.

참조는 [메타 데이터 가이드](metadata.md) 있는 NuGet 패키지에 추가 해야 하는 필수 및 선택적 메타 데이터에 대 한 자세한 내용은 합니다.


## <a name="further-nuget-information"></a>NuGet의 추가 정보

에 대해 자세히 알아보세요 [Xamarin에 대 한 NuGets을 수동으로 만드는](~/cross-platform/app-fundamentals/nuget-manual.md) 하는 방법과 [응용 프로그램에 NuGet 패키지를 포함할](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)합니다.

Microsoft의 [NuGet 설명서](https://docs.microsoft.com/nuget/) 에 보다 자세한 정보를 포함 된 **.nupkg** 형식 및 Visual Studio에서 NuGet 패키지를 사용 합니다.

NuGet 패키지가 프로젝트에 대 한 디자인 토론 (규칙 하위 NuGetizer 3000)을 사용할 수는 [NuGet GitHub 리포지토리](https://github.com/NuGet/Home/wiki/NuGetizer-3000)합니다.


## <a name="related-links"></a>관련 링크

- [NuGetizer 3000 사용 사례](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Xamarin에 대 한 NuGet 패키지를 수동으로 만들려면](~/cross-platform/app-fundamentals/nuget-manual.md)
- [NuGet 설명서](https://docs.microsoft.com/nuget/)
- [릴리스 정보](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#NuGetizer_3000)
