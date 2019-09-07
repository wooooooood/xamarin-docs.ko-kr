---
title: NuGet에 대 한 새 플랫폼별 라이브러리 프로젝트 만들기
description: 이 문서에서는 여러 플랫폼에 대 한 플랫폼별 코드를 포함 하는 단일 NuGet 패키지를 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: 73f44acad3e30e4301a69e5f2422cd4dd1a3dbf5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766566"
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>NuGet에 대 한 새 플랫폼별 라이브러리 프로젝트 만들기

IOS, Android 등의 특정 플랫폼을 대상으로 하는 다중 플랫폼 라이브러리 프로젝트는 공유 프로젝트에서 가장 잘 작동 합니다.

NuGet에는 iOS 및 Android 관련 코드와 둘 다에 공통적인 .NET 코드가 포함 될 수 있습니다.

여러 어셈블리가 생성 되어 단일 NuGet 패키지로 빌드됩니다. NuGet 표준은 Xamarin.ios 및 Android 프로젝트와 같은 지원 되는 모든 프로젝트 형식에 패키지를 추가할 수 있는지 확인 합니다.

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>플랫폼 간 라이브러리 NuGet을 만드는 단계

1. **파일 > 새 솔루션** 을 선택 하거나 기존 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 프로젝트**를 선택 합니다.

2. **다중 플랫폼 > 라이브러리** 섹션에서 **다중 플랫폼 라이브러리** 를 선택 합니다.

    [![](platform-specific-images/mulitplatform-library-sml.png "단일 코드 베이스에 대 한 다중 플랫폼 라이브러리 구성")](platform-specific-images/multiplatform-library.png#lightbox)

3. **이름** 및 **설명을**입력 하 고 **플랫폼별**를 선택 합니다.

    [![](platform-specific-images/specific-configure-sml.png "IOS 및 Android 용 플랫폼별 라이브러리 구성")](platform-specific-images/specific-configure.png#lightbox)

4. 마법사를 완료합니다. 솔루션에 추가 되는 프로젝트는 다음과 같습니다.

    - **Android 프로젝트** – android 관련 코드를이 프로젝트에 선택적으로 추가할 수 있습니다.
    - **Ios 프로젝트** – ios 관련 코드를이 프로젝트에 선택적으로 추가할 수 있습니다.
    - **NuGet 프로젝트** -이 프로젝트에 코드가 추가 되지 않습니다. 다른 프로젝트를 참조 하 고 NuGet 패키지 출력에 대 한 메타 데이터 구성을 포함 합니다.
    - **공유 프로젝트** – 컴파일러 지시문 내의 `#if` 플랫폼별 코드를 포함 하 여이 프로젝트에 공용 코드를 추가 해야 합니다.

5. NuGet 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **옵션**을 선택한 다음 **Nuget 패키지 > 메타 데이터** 섹션을 열고 [필요한 메타](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) 데이터 (선택 사항)를 입력 합니다.

    [![](platform-specific-images/specific-metadata-sml.png "필요한 메타 데이터 입력")](platform-specific-images/specific-metadata.png#lightbox)

6. 또한 **프로젝트 옵션** 창에서 **참조 어셈블리** 섹션을 열고 공유 라이브러리가 "bait 및 스위치"를 통해 지원할 PCL 프로필을 선택 합니다.

    ![](platform-specific-images/specific-reference-assemblies.png "또한 프로젝트 옵션 창에서 참조 어셈블리 섹션을 열고 공유 라이브러리가 bait 및 스위치를 통해 지원할 PCL 프로필을 선택 합니다.")

    > [!NOTE]
    > "Bait and switch"는 PCL 어셈블리가 라이브러리에 의해 노출 되는 API만 포함 한다는 것을 의미 합니다 (플랫폼별 코드를 포함할 수 없음). NuGet이 Xamarin 프로젝트에 추가 되 면 공유 라이브러리가 PCL에 대해 컴파일되며 플랫폼 특정 어셈블리에는 iOS 또는 Android 프로젝트에서 실제로 사용 하는 코드가 포함 됩니다.

7. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **Nuget 패키지 만들기** (또는 솔루션 빌드 또는 배포)를 선택 합니다 **. nupkg** NuGet 패키지 파일은 구성에 따라 **/Co/** 폴더 (디버그 또는 릴리스)에 저장 됩니다.

    ![](platform-specific-images/create-nuget-package.png "NuGet 패키지 파일은 구성에 따라 디버그 또는 릴리스 중 하나를 bin 폴더에 저장 됩니다.")

## <a name="verifying-the-output"></a>출력 확인

NuGet 패키지는 ZIP 파일 이기도 하므로 생성 된 패키지의 내부 구조를 검사할 수 있습니다.

이 스크린샷에는 iOS 및 Android를 지 원하는 플랫폼별 NuGet의 내용이 표시 되 고 두 개의 참조 어셈블리가 선택 되어 있습니다.

![](platform-specific-images/nuget-output.png "NuGet 패키지에 포함 된 파일")

## <a name="related-links"></a>관련 링크

- [메타데이터 가이드](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
