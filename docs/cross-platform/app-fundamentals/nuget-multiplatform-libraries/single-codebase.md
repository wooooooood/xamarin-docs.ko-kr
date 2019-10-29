---
title: NuGet에 대 한 새 다중 플랫폼 라이브러리 만들기
description: 이 문서에서는 NuGet과 함께 사용할 다중 플랫폼 라이브러리를 만드는 방법을 설명 합니다. 이 기법은 .NET 기본 클래스 라이브러리에서 완전히 표현할 수 있는 비즈니스 논리 및 알고리즘에 적합 하며, 따라서 플랫폼별 코드 없이 모든 대상 플랫폼에서 실행 됩니다.
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 7dabb420aa094e67fae689f47b3b64a8fe1a6ed4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016703"
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>NuGet에 대 한 새 다중 플랫폼 라이브러리 만들기

PCL 또는 .NET Standard를 사용 하는 다중 플랫폼 라이브러리 프로젝트를 만들면 결과 NuGet을 ASP.NET 프로젝트를 비롯 한 대상 프로필을 지 원하는 모든 .NET 프로젝트 또는 WinForms, WPF 또는 UWP를 사용 하는 데스크톱 응용 프로그램에 추가할 수 있습니다.

라이브러리는 선택한 PCL 또는 .NET Standard 프로필에서 지원 되는 코드와 추가 된 다른 Nuget 포함할 수 있습니다.
이는 전적으로 .NET 기본 클래스 라이브러리에서 표현할 수 있는 비즈니스 논리 및 알고리즘에 적합 합니다.

단일 어셈블리가 만들어지고 NuGet 패키지로 빌드됩니다.

나중에 플랫폼 특정 기능이 필요한 경우 [플랫폼별 프로젝트를 추가할 수 있습니다](#add-platforms).

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>다중 플랫폼 라이브러리 NuGet을 만드는 단계

1. **파일 > 새 솔루션** 을 선택 하거나 기존 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 프로젝트**를 선택 합니다.

2. **다중 플랫폼 > 라이브러리** 섹션에서 **다중 플랫폼 라이브러리** 를 선택 합니다.

   [![](single-codebase-images/mulitplatform-library-sml.png "Configure multi-platform library for a single code base")](single-codebase-images/mulitplatform-library.png#lightbox)

3. **이름** 및 **설명을**입력 하 고 **모든 플랫폼에 대해 단일**을 선택 합니다.

   [![](single-codebase-images/single-configure-sml.png "Configure multi-platform library for a single code base")](single-codebase-images/single-configure.png#lightbox)

4. 마법사를 완료합니다. 솔루션에 단일 라이브러리 프로젝트가 생성 됩니다.

5. 새 라이브러리 프로젝트를 마우스 오른쪽 단추로 클릭 한 다음 **옵션**을 선택 합니다. **빌드 > 일반** 섹션에서 **대상 프레임 워크** 를 설정할 수 있습니다. .net 휴대용 PCL 프로필 또는 .NET Standard 버전을 선택 합니다.

   [![](single-codebase-images/single-choose-type-sml.png "Choose PCL or .NET Standard for library type")](single-codebase-images/single-choose-type.png#lightbox)

6. 또한 **프로젝트 옵션** 창에서 **NuGet 패키지 > 메타 데이터** 섹션을 열고 [필요한 메타](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) 데이터 뿐만 아니라 선택적 메타 데이터를 입력 합니다.

   [![](single-codebase-images/single-metadata-sml.png "Enter required metadata")](single-codebase-images/single-metadata.png#lightbox)

7. 라이브러리 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **Nuget 패키지 만들기** (또는 솔루션 빌드 또는 배포)를 선택 **합니다. nupkg** NuGet 패키지 파일은 구성에 따라/l o b **/** 폴더 (디버그 또는 릴리스)에 저장 됩니다.

   ![](single-codebase-images/create-nuget-package.png "The NuGet package file will be saved in the bin folder either Debug or Release, depending on configuration")

## <a name="verifying-the-output"></a>출력 확인

NuGet 패키지는 ZIP 파일 이기도 하므로 생성 된 패키지의 내부 구조를 검사할 수 있습니다.

이 스크린샷에서는 PCL 기반 NuGet의 내용을 보여 줍니다. 단일 PCL 어셈블리만 포함 됩니다.

![](single-codebase-images/nuget-output.png "Files contained in the NuGet package")

<a name="add-platforms" />

## <a name="adding-platform-specific-code"></a>플랫폼별 코드 추가

PCL 기반 프로젝트 및 .NET Standard 기반 프로젝트는 플랫폼별 참조 (예: iOS 또는 Android 기능)를 포함할 수 없습니다.

플랫폼 특정 코드를 포함 하도록 기존 PCL 프로젝트 또는 .NET Standard 프로젝트를 확장 해야 하는 경우 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가 > 플랫폼 구현 추가**...를 선택 하 여이 작업을 수행할 수 있습니다.

[![](single-codebase-images/add-later-sml.png "Add platform implementation menu")](single-codebase-images/add-later.png#lightbox)

하나 이상의 플랫폼 프로젝트를 솔루션에 추가할 수 있으며, 기존 PCL 또는 .NET Standard 라이브러리를 선택적으로 공유 프로젝트로 변환할 수 있습니다.

[![](single-codebase-images/add-later-platforms-sml.png "Add platform options such as iOS, Android, and Shared Project")](single-codebase-images/add-later-platforms-sml.png#lightbox)

공유 프로젝트로 변환한 후에는 **프로젝트 옵션 > Nuget 패키지 > 참조 어셈블리**
[섹션](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) 을 방문 하 여 필요한 프로필이 선택 되었는지 확인 합니다. 이전에에서 사용 됨).

## <a name="related-links"></a>관련 링크

- [메타데이터 가이드](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
