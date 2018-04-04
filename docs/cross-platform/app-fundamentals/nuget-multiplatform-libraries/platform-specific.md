---
title: NuGet에 대 한 새 플랫폼 관련 라이브러리 프로젝트 만들기
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 0f244e614a40e444139d51a9466ccc7225a7fe68
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>NuGet에 대 한 새 플랫폼 관련 라이브러리 프로젝트 만들기

IOS 및 Android 등의 특정 플랫폼을 대상으로 하는 다중 플랫폼 라이브러리 프로젝트 공유 프로젝트에 가장 적합 합니다.

NuGet으로 iOS 및 Android 관련 코드 모두에 공통적인.NET 코드를 포함할 수 있습니다.

여러 어셈블리 생성 및 단일 NuGet 패키지에 기본 제공 됩니다. NuGet 표준 Xamarin iOS 및 Android 프로젝트와 같은 모든 지원 되는 프로젝트 형식에는 패키지를 추가할 수 있는지 확인 합니다.

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>플랫폼 간 라이브러리 NuGet을 만드는 단계

1. 선택 **파일 > 새 솔루션** (또는 기존 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 프로젝트**).

2. 선택 **다중 플랫폼 라이브러리** 에서 **다중 플랫폼 > 라이브러리** 섹션:

  [![](platform-specific-images/mulitplatform-library-sml.png "단일 코드 베이스에 대 한 다양 한 플랫폼 라이브러리 구성")](platform-specific-images/multiplatform-library.png#lightbox)

3. 입력 한 **이름** 및 **설명**, 선택 **플랫폼별**:

  [![](platform-specific-images/specific-configure-sml.png "IOS 및 Android 플랫폼 관련 라이브러리를 구성 합니다.")](platform-specific-images/specific-configure.png#lightbox)

4. 마법사를 완료합니다. 다음 프로젝트는 솔루션에 추가 됩니다.

  - **Android 프로젝트** – Android 관련 코드가 필요에 따라이 프로젝트에 추가할 수 있습니다.
  - **iOS 프로젝트** – iOS 관련 코드가 필요에 따라이 프로젝트에 추가할 수 있습니다.
  - **NuGet 프로젝트** – 코드 없이이 프로젝트에 추가 됩니다. 다른 프로젝트를 참조 하 고 NuGet 패키지 출력에 대 한 메타 데이터 구성을 포함 합니다.
  - **프로젝트 공유** – 일반적인 코드 내의 플랫폼별 코드를 포함 하 여이 프로젝트에 추가 해야 `#if` 컴파일러 지시문입니다.

5. NuGet 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **옵션**을 연 다음는 **NuGet 패키지 > 메타 데이터** 섹션 및 입력의 [필요한 메타 데이터가](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (모든 선택적으로 메타 데이터):

  [![](platform-specific-images/specific-metadata-sml.png "필요한 메타 데이터를 입력 합니다.")](platform-specific-images/specific-metadata.png#lightbox)

6. 또한는 **프로젝트 옵션** 창을 열려면는 **참조 어셈블리** 섹션 및 공유 라이브러리 "밥 and 스위치"를 통해 지원할 PCL 프로필 선택:

  ![](platform-specific-images/specific-reference-assemblies.png "프로젝트 옵션 창에서 참조 어셈블리 섹션을 열어 공유 라이브러리 밥 and 스위치를 통해 지원할 PCL 프로필 선택")

  > [!NOTE]
> "밥 and 스위치" PCL 어셈블리 (플랫폼별 코드를 포함할 수 없습니다) 라이브러리에 의해 노출 되는 API만 포함 되어 있음을 의미 합니다. NuGet는 Xamarin 프로젝트에 추가 되 면 공유 라이브러리를 사용 하는 PCL에 대해 컴파일되는 있지만 플랫폼별 어셈블리 실제로 iOS 또는 Android 프로젝트에서 사용 하는 코드를 포함 합니다.

7. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **NuGet 패키지 만들기** (또는 빌드하거나 솔루션 배포) 및 **.nupkg** NuGet 패키지 파일에 저장 됩니다는 **/bin/** 폴더 ( 디버그 또는 릴리스 구성에 따라).

  ![](platform-specific-images/create-nuget-package.png "NuGet 패키지 파일에에서 저장 됩니다 bin 폴더 디버그 또는 릴리스 구성에 따라")


## <a name="verifying-the-output"></a>출력 확인

NuGet 패키지는 ZIP 파일도 생성 되는 패키지의 내부 구조를 검사 하는 것이 불가능 합니다.

이 스크린 샷에 표시 iOS 및 Android에서 지 원하는 두 명의 참조 어셈블리는 플랫폼 특정 NuGet의 내용을 선택:

![](platform-specific-images/nuget-output.png "NuGet 패키지에 포함 된 파일")


## <a name="related-links"></a>관련 링크

- [메타데이터 가이드](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
