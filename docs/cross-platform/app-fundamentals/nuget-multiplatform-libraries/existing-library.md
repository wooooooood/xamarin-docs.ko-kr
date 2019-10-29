---
title: 기존 라이브러리 프로젝트에서 NuGet 만들기
description: 이 문서에서는 기존 라이브러리 프로젝트에서 NuGet 패키지를 만들어 다른 개발자와 코드를 공유할 수 있도록 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: davidortinau
ms.author: daortin
ms.date: 04/20/2017
ms.openlocfilehash: 30158056b8adbdd5aba4cc311220a22502ea9968
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016779"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>기존 라이브러리 프로젝트에서 NuGet 만들기

기존 PCL 또는 .NET Standard 라이브러리는 **프로젝트 옵션** 창을 통해 nuget로 설정할 수 있습니다.

1. **Solution Pad** 에서 라이브러리 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **옵션**을 선택 합니다.

2. **NuGet 패키지 > 메타 데이터** 섹션으로 이동 하 고 **일반** 탭에서 [필요한 정보](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) 를 모두 입력 합니다.

   [![](existing-library-images/existing-metadata-sml.png "Enter required metadata")](existing-library-images/existing-metadata.png#lightbox)

3. 필요에 따라 **세부 정보** 탭에서 [메타 데이터를 추가](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) 합니다.

4. 메타 데이터가 구성 되 면 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **Nuget 패키지 만들기** 를 선택할 수 있습니다 **. nupkg** NuGet 패키지 **파일은 구성** 에 따라 디버그 또는 릴리스 중 하나에 저장 됩니다.

   ![](existing-library-images/create-nuget-package.png "Choose Create NuGet Package from the right-click menu")

5. _모든_ 빌드 또는 배포에서 nuget 패키지를 만들려면 프로젝트를 빌드할 때 nuget **패키지 > 빌드** 섹션 및 틱 **nuget 패키지 만들기**로 이동 합니다.

    [![](existing-library-images/existing-tickbox-sml.png "Tick to create a NuGet package")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> NuGet 패키지를 빌드하면 빌드 프로세스의 속도가 느려질 수 있습니다. 이 상자를 선택 않는 경우 프로젝트 상황에 맞는 메뉴 (위의 4 단계에 표시 됨)에서 언제 든 지 수동으로 NuGet 패키지를 생성할 수 있습니다.

## <a name="verifying-the-output"></a>출력 확인

NuGet 패키지는 ZIP 파일 이기도 하므로 생성 된 패키지의 내부 구조를 검사할 수 있습니다.

이 스크린샷에서는 PCL 기반 NuGet의 내용을 보여 줍니다. 단일 PCL 어셈블리만 포함 됩니다.

![](existing-library-images/nuget-output.png "Files contained in the NuGet package")

## <a name="related-links"></a>관련 링크

- [메타데이터 가이드](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
