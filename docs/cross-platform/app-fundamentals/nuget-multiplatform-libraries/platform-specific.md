---
title: NuGet에 대 한 새 플랫폼별 라이브러리 프로젝트 만들기
description: 이 문서에는 여러 플랫폼에 대 한 플랫폼별 코드를 포함 하는 단일 NuGet 패키지를 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 4be010448d963462ccf06c263ddfad7ba1d9feae
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832034"
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>NuGet에 대 한 새 플랫폼별 라이브러리 프로젝트 만들기

IOS 및 Android와 같은 특정 플랫폼을 대상으로 하는 다중 플랫폼 라이브러리 프로젝트는 공유 프로젝트를 사용 하 여 가장 효과적입니다.

NuGet으로 iOS 및 Android 관련 코드를 둘 다에 공통적으로 적용 하는.NET 코드를 포함할 수 있습니다.

여러 어셈블리 생성 및 단일 NuGet 패키지에 기본 제공 됩니다. 표준 NuGet 패키지를 Xamarin.iOS 및 Android 프로젝트와 같은 모든 지원 되는 프로젝트 형식에 추가할 수 있는지 확인 합니다.

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>플랫폼 간 라이브러리 NuGet을 만드는 단계

1. 선택 **파일 > 새 솔루션** (또는 기존 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 프로젝트**).

2. 선택할 **다중 플랫폼 라이브러리** 에서 합니다 **다중 플랫폼 > 라이브러리** 섹션:

    [![](platform-specific-images/mulitplatform-library-sml.png "단일 코드 베이스에 대해 다중 플랫폼 라이브러리 구성")](platform-specific-images/multiplatform-library.png#lightbox)

3. 입력을 **이름** 하 고 **설명**를 선택한 **플랫폼별**:

    [![](platform-specific-images/specific-configure-sml.png "IOS 및 Android에 대 한 플랫폼별 라이브러리를 구성 합니다.")](platform-specific-images/specific-configure.png#lightbox)

4. 마법사를 완료합니다. 다음 프로젝트를 솔루션에 추가 됩니다.

    - **Android 프로젝트** -이 프로젝트에 필요에 따라 Android 관련 코드를 추가할 수 있습니다.
    - **iOS 프로젝트** -이 프로젝트에 필요에 따라 iOS 관련 코드를 추가할 수 있습니다.
    - **NuGet 프로젝트** – 코드 없이이 프로젝트에 추가 됩니다. 다른 프로젝트를 참조 하 고 NuGet 패키지 출력에 대 한 메타 데이터 구성을 포함 합니다.
    - **프로젝트를 공유** – 일반적인 코드 내에서 플랫폼별 코드를 포함 하 여이 프로젝트에 추가 해야 `#if` 컴파일러 지시문입니다.

5. NuGet 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **옵션**를 연 다음 합니다 **NuGet 패키지 > 메타 데이터** 섹션 및 입력 합니다 [필수 메타 데이터](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (약관과 선택 사항으로 메타 데이터):

    [![](platform-specific-images/specific-metadata-sml.png "필수 메타 데이터를 입력 합니다.")](platform-specific-images/specific-metadata.png#lightbox)

6. 또한 합니다 **프로젝트 옵션** 창을 열려면 합니다 **참조 어셈블리** 섹션 및 공유 라이브러리는 "bait 및 스위치"를 통해 지원 되는 PCL 프로필 선택:

    ![](platform-specific-images/specific-reference-assemblies.png "프로젝트 옵션 창에서 섹션을 참조 어셈블리를 열어 bait 및 스위치를 통해 공유 라이브러리는 지원 되는 PCL 프로필 선택")

    > [!NOTE]
    > "Bait 및 스위치" PCL 어셈블리 (플랫폼별 코드를 포함할 수 없습니다) 라이브러리에서 노출 된 API에만 포함 되어 있음을 의미 합니다. Xamarin 프로젝트에 NuGet 추가 되 면 공유 라이브러리는 PCL을 컴파일할 수는 있지만 iOS 또는 Android 프로젝트에서 실제로 사용 되는 코드를 포함 하는 플랫폼별 어셈블리.

7. 선택한 프로젝트를 마우스 오른쪽 단추로 클릭 **NuGet 패키지 만들기** (빌드 또는 솔루션 배포) 및 **.nupkg** NuGet 패키지 파일에 저장 됩니다는 **/bin/** 폴더 ( 디버그 또는 릴리스 구성에 따라).

    ![](platform-specific-images/create-nuget-package.png "NuGet 패키지 파일에 저장할 bin 폴더에서 디버그 또는 릴리스 구성에 따라")


## <a name="verifying-the-output"></a>출력 확인

NuGet 패키지는 ZIP 파일 또한 생성된 된 패키지의 내부 구조를 검사 하는 것이 가능 합니다.

이 스크린샷은 iOS 및 Android에서 지 원하는 두 명의 참조 어셈블리는 플랫폼 특정 NuGet의 내용을 선택 합니다.

![](platform-specific-images/nuget-output.png "NuGet 패키지에 포함 된 파일")


## <a name="related-links"></a>관련 링크

- [메타데이터 가이드](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
