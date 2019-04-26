---
title: 기존 라이브러리 프로젝트에서 NuGet 만들기
description: 이 문서에는 코드를 다른 개발자와 공유할 수 있도록 기존 라이브러리 프로젝트에서 NuGet 패키지를 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: asb3993
ms.author: amburns
ms.date: 04/20/2017
ms.openlocfilehash: 7f407b22d1793d585ae40aeae8c2d9b7616784e6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61267819"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>기존 라이브러리 프로젝트에서 NuGet 만들기

기존 PCL 또는.NET Standard 라이브러리는 Nuget을 통해로 변환할 수 있습니다 합니다 **프로젝트 옵션** 창:

1. 라이브러리 프로젝트를 마우스 오른쪽 단추로 클릭 합니다 **Solution Pad** 선택한 **옵션**합니다.

2. 로 이동 합니다 **NuGet 패키지 > 메타 데이터** 섹션 및 모든 입력는 [필요한 정보를](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) 에 **일반** 탭:

  [![](existing-library-images/existing-metadata-sml.png "필수 메타 데이터를 입력 합니다.")](existing-library-images/existing-metadata.png#lightbox)

3. 필요에 따라 [추가 메타 데이터 추가](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) 에 **세부 정보** 탭 합니다.

4. 메타 데이터를 구성한 후에 프로젝트를 마우스 오른쪽 단추로 클릭 및 선택할 수 있습니다 **NuGet 패키지 만들기** 하며 **.nupkg** NuGet 패키지 파일에 저장 됩니다는 **/bin/** 폴더 (디버그 또는 릴리스 구성에 따라).

  ![](existing-library-images/create-nuget-package.png "오른쪽 클릭 메뉴에서 NuGet 패키지 만들기를 선택 합니다.")

5. NuGet 패키지를 만들려면 _모든_ 빌드 또는 배포로 이동 합니다 **NuGet 패키지 > 빌드** 섹션과 눈금 **프로젝트를 빌드할 때 NuGet 패키지를 만듭니다**:

    [![](existing-library-images/existing-tickbox-sml.png "NuGet 패키지를 만들려면 선택 합니다.")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> NuGet 빌드 패키지 빌드 프로세스 속도가 떨어질 수 있습니다. 이 상자를 선택 하지는 경우 언제 든 지 (위의 4 단계에서 표시) 프로젝트 상황에 맞는 메뉴에서 NuGet 패키지를 수동으로 생성할 수 있습니다.

## <a name="verifying-the-output"></a>출력 확인

NuGet 패키지는 ZIP 파일 또한 생성된 된 패키지의 내부 구조를 검사 하는 것이 가능 합니다.

이 스크린샷에서 단일 PCL 어셈블리만 포함 됩니다. PCL 기반 NuGet –의 콘텐츠를 보여줍니다.

![](existing-library-images/nuget-output.png "NuGet 패키지에 포함 된 파일")


## <a name="related-links"></a>관련 링크

- [메타데이터 가이드](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
