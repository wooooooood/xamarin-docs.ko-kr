---
title: NuGet에 대 한 새 다중 플랫폼 라이브러리 만들기
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ee508d40423e3757f7e2934b7682f840ebf8b86a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/10/2018
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>NuGet에 대 한 새 다중 플랫폼 라이브러리 만들기

PCL을 사용 하는 다중 플랫폼 라이브러리 프로젝트 만들기 또는.NET 표준 의미 결과 NuGet ASP.NET 프로젝트 또는 WinForms, WPF 또는 UWP를 사용 하 여 데스크톱 응용 프로그램을 포함 하 여 대상 프로필을 지 원하는 모든.NET 프로젝트에 추가할 수 있습니다.

라이브러리에는 추가 된 다른 모든 NuGets 뿐만 아니라 선택한 PCL 또는 표준.NET 프로필에서 지 원하는 코드 포함 될 수 있습니다.
이 비즈니스 논리 및.NET 기본 클래스 라이브러리에서 완전히 표현할 수 있는 알고리즘에 적합 합니다.

단일 어셈블리를 만들고 NuGet 패키지에 기본 제공 됩니다.

나중에 플랫폼 특정 기능이 필요한 경우 [플랫폼별 프로젝트를 추가할 수 있습니다](#add-platforms)합니다.

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>다중 플랫폼 라이브러리 NuGet을 만드는 단계

1. 선택 **파일 > 새 솔루션** (또는 기존 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 프로젝트**).

2. 선택 **다중 플랫폼 라이브러리** 에서 **다중 플랫폼 > 라이브러리** 섹션:

  [![](single-codebase-images/mulitplatform-library-sml.png "단일 코드 베이스에 대 한 다양 한 플랫폼 라이브러리 구성")](single-codebase-images/mulitplatform-library.png#lightbox)

3. 입력 한 **이름** 및 **설명**, 선택 **모든 플랫폼에 대 한 단일**:

  [![](single-codebase-images/single-configure-sml.png "단일 코드 베이스에 대 한 다양 한 플랫폼 라이브러리 구성")](single-codebase-images/single-configure.png#lightbox)

4. 마법사를 완료합니다. 단일 라이브러리 프로젝트를 솔루션에 만들어집니다.

5. 새 라이브러리 프로젝트를 마우스 오른쪽 단추로 클릭 한 다음 선택 **옵션**합니다. **빌드 > 일반** 섹션에서는 **대상 프레임 워크** 설정할 –.NET 이식 가능한 PCL 프로 파일 또는 표준.NET 버전을 선택 합니다.

  [![](single-codebase-images/single-choose-type-sml.png "PCL 또는.NET 표준 라이브러리 형식에 대 한 선택")](single-codebase-images/single-choose-type.png#lightbox)

6. 또한는 **프로젝트 옵션** 창을 열려면는 **NuGet 패키지 > 메타 데이터** 섹션 및 입력의 [필요한 메타 데이터가](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (뿐만 아니라 모든 선택적 메타 데이터):

  [![](single-codebase-images/single-metadata-sml.png "필요한 메타 데이터를 입력 합니다.")](single-codebase-images/single-metadata.png#lightbox)

7. 라이브러리 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **NuGet 패키지 만들기** (또는 빌드하거나 솔루션 배포) 및 **.nupkg** NuGet 패키지 파일에 저장 됩니다는 **/bin/** 폴더 (디버그 또는 릴리스 구성에 따라):

  ![](single-codebase-images/create-nuget-package.png "NuGet 패키지 파일에에서 저장 됩니다 bin 폴더 디버그 또는 릴리스 구성에 따라")


## <a name="verifying-the-output"></a>출력 확인

NuGet 패키지는 ZIP 파일도 생성 되는 패키지의 내부 구조를 검사 하는 것이 불가능 합니다.

이 스크린 샷 PCL 어셈블리를만 포함 되어 PCL 기반 NuGet – 내용을 보여 줍니다.

![](single-codebase-images/nuget-output.png "NuGet 패키지에 포함 된 파일")

<a name="add-platforms" />

## <a name="adding-platform-specific-code"></a>플랫폼별 코드 추가

PCL 기반 및 프로젝트 형식에 기반.NET 표준 플랫폼 특정 (예: iOS 또는 Android 기능)에 대 한 참조를 포함할 수 없습니다.

기존 PCL 프로젝트 또는 표준.NET 프로젝트를 해야 경우 플랫폼별 코드를 포함 하도록 확장 해야 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 **추가 > 플랫폼 구현 추가 중...** :

[![](single-codebase-images/add-later-sml.png "추가 플랫폼 구현 메뉴")](single-codebase-images/add-later.png#lightbox)

하나 이상의 플랫폼 프로젝트를 솔루션에 추가할 수 있으며, 기존 PCL 또는 표준.NET 라이브러리는 공유 프로젝트에 선택적으로 변환 될 수 있습니다.:

[![](single-codebase-images/add-later-platforms-sml.png "IOS, Android 및 공유 프로젝트와 같은 플랫폼 옵션 추가")](single-codebase-images/add-later-platforms-sml.png#lightbox)

공유 프로젝트를 변환한 후 방문는 **프로젝트 옵션 > NuGet 패키지 > 참조 어셈블리**
[섹션](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) 필요한 모든 프로필에서 선택 되어 있는지 확인 하 고 (하는 NuGet에서 이전에 사용한 프로젝트와 호환 되도록 계속).


## <a name="related-links"></a>관련 링크

- [메타데이터 가이드](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
