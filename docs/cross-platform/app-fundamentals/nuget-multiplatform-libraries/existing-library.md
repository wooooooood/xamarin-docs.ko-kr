---
title: 기존 라이브러리 프로젝트에서 NuGet을 만드는
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: asb3993
ms.author: amburns
ms.date: 04/20/2017
ms.openlocfilehash: 466ec405d4951de2d47d075c748cbc20ac5074e6
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/10/2018
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>기존 라이브러리 프로젝트에서 NuGet을 만드는

기존 PCL 또는 표준.NET 라이브러리를 통해 NuGets로 변환할 수 있습니다는 **프로젝트 옵션** 창:

1. 라이브러리 프로젝트를 마우스 오른쪽 단추로 클릭는 **솔루션 패드** 선택 **옵션**합니다.

2. 이동 하는 **NuGet 패키지 > 메타 데이터** 섹션 및 모든 입력은 [필요한 정보를](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) 에 **일반** 탭:

  [![](existing-library-images/existing-metadata-sml.png "필요한 메타 데이터를 입력 합니다.")](existing-library-images/existing-metadata.png#lightbox)

3. 필요에 따라 [추가 메타 데이터 추가](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) 에 **세부 정보** 탭 합니다.

4. 메타 데이터 구성 되 면 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택할 수 **NuGet 패키지 만들기** 및 **.nupkg** NuGet 패키지 파일에 저장 됩니다는 **/bin/** 폴더 (디버그 또는 릴리스 구성에 따라).

  ![](existing-library-images/create-nuget-package.png "오른쪽 클릭 메뉴에서 NuGet 패키지 만들기를 선택 합니다.")

5. 에 NuGet 패키지를 만들려면 _모든_ 빌드 또는 배포로 이동는 **NuGet 패키지 > 빌드** 섹션 및 눈금 **프로젝트를 빌드할 때 NuGet 패키지를 만들**:

    [![](existing-library-images/existing-tickbox-sml.png "눈금 NuGet 패키지를 만들려면")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> NuGet 작성 패키지 빌드 프로세스를 느려질 수 있습니다. 이 상자가 선택 되어 있지, 생성할 수 있습니다 여전히 NuGet 패키지를 수동으로 프로젝트 상황에 맞는 메뉴 (위의 4 단계에서 표시 됨)에서 언제 든 지 합니다.

## <a name="verifying-the-output"></a>출력 확인

NuGet 패키지는 ZIP 파일도 생성 되는 패키지의 내부 구조를 검사 하는 것이 불가능 합니다.

이 스크린 샷 PCL 어셈블리를만 포함 되어 PCL 기반 NuGet – 내용을 보여 줍니다.

![](existing-library-images/nuget-output.png "NuGet 패키지에 포함 된 파일")


## <a name="related-links"></a>관련 링크

- [메타데이터 가이드](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
