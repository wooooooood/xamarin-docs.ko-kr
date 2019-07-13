---
title: NuGet에 대 한 새 다중 플랫폼 라이브러리 만들기
description: 이 문서에는 NuGet을 사용 하 여 사용에 대 한 다중 플랫폼 라이브러리를 만드는 방법을 설명 합니다. 이 기술은.NET 기본 클래스 라이브러리에서 전적으로 표현 될 수 있습니다 하 고 따라서 플랫폼별 코드 없이 모든 대상 플랫폼에서 실행 되는 알고리즘 및 비즈니스 논리에도 적합 합니다.
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 6371c2af15eab9c5124212eefd9cf70d07b945d4
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864726"
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>NuGet에 대 한 새 다중 플랫폼 라이브러리 만들기

PCL을 사용 하는 다중 플랫폼 라이브러리 프로젝트 만들기 또는.NET Standard 의미 결과 NuGet ASP.NET 프로젝트 또는 WinForms, WPF 또는 UWP를 사용 하 여 데스크톱 앱을 포함 하 여 대상 프로필을 지 원하는 모든.NET 프로젝트에 추가할 수 있습니다.

선택한 PCL 또는.NET Standard 프로필 뿐만 아니라 추가 되는 다른 모든 Nuget에서 지 원하는 코드 라이브러리를 포함할 수 있습니다.
이 비즈니스 논리 및.NET 기본 클래스 라이브러리에서 완전히 표현할 수 있는 알고리즘에 적합 합니다.

단일 어셈블리로 생성 되어 NuGet 패키지로 빌드됩니다.

플랫폼 특정 기능을 나중에 필요한 경우 [플랫폼별 프로젝트를 추가할 수 있습니다](#add-platforms)합니다.

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>다중 플랫폼 라이브러리 NuGet을 만드는 단계

1. 선택 **파일 > 새 솔루션** (또는 기존 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 프로젝트**).

2. 선택할 **다중 플랫폼 라이브러리** 에서 합니다 **다중 플랫폼 > 라이브러리** 섹션:

   [![](single-codebase-images/mulitplatform-library-sml.png "단일 코드 베이스에 대해 다중 플랫폼 라이브러리 구성")](single-codebase-images/mulitplatform-library.png#lightbox)

3. 입력을 **이름** 하 고 **설명**를 선택한 **모든 플랫폼에 대 한 단일**:

   [![](single-codebase-images/single-configure-sml.png "단일 코드 베이스에 대해 다중 플랫폼 라이브러리 구성")](single-codebase-images/single-configure.png#lightbox)

4. 마법사를 완료합니다. 단일 라이브러리 프로젝트를 솔루션에 만들어집니다.

5. 새 라이브러리 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택한 **옵션**합니다. **빌드 > 일반** 섹션에서는 **대상 프레임 워크** 설정할 –.NET 이식 가능한 PCL 프로필 또는.NET Standard 버전을 선택 합니다.

   [![](single-codebase-images/single-choose-type-sml.png "PCL 또는.NET Standard 라이브러리 형식에 대 한 선택")](single-codebase-images/single-choose-type.png#lightbox)

6. 또한를 **프로젝트 옵션** 창을 열려면 합니다 **NuGet 패키지 > 메타 데이터** enter 섹션의 [필수 메타 데이터](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (뿐만 아니라 모든 선택적 메타 데이터):

   [![](single-codebase-images/single-metadata-sml.png "필수 메타 데이터를 입력 합니다.")](single-codebase-images/single-metadata.png#lightbox)

7. 선택한 라이브러리 프로젝트를 마우스 오른쪽 단추로 클릭 **NuGet 패키지 만들기** (빌드 또는 솔루션 배포) 및 **.nupkg** NuGet 패키지 파일에 저장 됩니다는 **/bin/** 폴더 (디버그 또는 릴리스 구성에 따라):

   ![](single-codebase-images/create-nuget-package.png "NuGet 패키지 파일을 저장할 bin 폴더에서 디버그 또는 릴리스 구성에 따라")


## <a name="verifying-the-output"></a>출력 확인

NuGet 패키지는 ZIP 파일 또한 생성된 된 패키지의 내부 구조를 검사 하는 것이 가능 합니다.

이 스크린샷에서 단일 PCL 어셈블리만 포함 됩니다. PCL 기반 NuGet –의 콘텐츠를 보여줍니다.

![](single-codebase-images/nuget-output.png "NuGet 패키지에 포함 된 파일")

<a name="add-platforms" />

## <a name="adding-platform-specific-code"></a>플랫폼별 코드 추가

PCL 기반 및.NET Standard 기반 프로젝트 (예: iOS 또는 Android 기능) 플랫폼 특정 참조를 포함할 수 없습니다.

기존 PCL 프로젝트 또는.NET Standard 프로젝트에 플랫폼별 코드를 포함 하도록 확장할 필요한 경우이 가능 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 **추가 > 플랫폼 구현 추가 중...** :

[![](single-codebase-images/add-later-sml.png "플랫폼 구현 메뉴 추가")](single-codebase-images/add-later.png#lightbox)

하나 이상의 플랫폼 프로젝트를 솔루션에 추가할 수 있습니다 하 고 공유 프로젝트에 기존 PCL 또는.NET Standard 라이브러리를 필요에 따라 변환할 수 있습니다.

[![](single-codebase-images/add-later-platforms-sml.png "IOS, Android 및 공유 프로젝트와 같은 플랫폼 옵션 추가")](single-codebase-images/add-later-platforms-sml.png#lightbox)

공유 프로젝트를 변환한 후 방문 합니다 **프로젝트 옵션 > NuGet 패키지 > 참조 어셈블리**
[섹션](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) 필요한 프로필 선택 되어 있는지 확인 하 고 (있도록는 NuGet은 프로젝트에서 이전에 사용한 것과 호환 되도록 계속).


## <a name="related-links"></a>관련 링크

- [메타데이터 가이드](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
