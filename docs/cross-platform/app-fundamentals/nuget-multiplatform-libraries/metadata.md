---
title: NuGet 메타 데이터 편집
description: 프로젝트 옵션을 사용 하 여 다중 플랫폼 라이브러리에 대 한 NuGet 메타 데이터를 편집 하려면
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 6d30f564d54b96d358d37059f9dababaf8f3314e
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2018
---
# <a name="editing-nuget-metadata"></a>NuGet 메타 데이터 편집

_프로젝트 옵션을 사용 하 여 다중 플랫폼 라이브러리에 대 한 NuGet 메타 데이터를 편집 하려면_

라이브러리 프로젝트 형식 (예: PCL 또는 표준.NET 또는 새 NuGet 프로젝트 형식)는 **NuGet 패키지** 섹션은 **프로젝트 옵션** 창.

**메타 데이터** 섹션에 사용 된 값에 구성 된 [ **.nuspec** NuGet 패키지 매니페스트 파일](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file)합니다.

## <a name="required-information"></a>필요한 정보

**일반** 탭에는 NuGet 패키지를 입력 해야 하는 4 개의 필드:

[![](metadata-images/metadata-general-sml.png "NuGet 패키지 필요한 메타 데이터 창")](metadata-images/metadata-general.png#lightbox)

- **ID** – Nuget.org (또는 패키지가 배포 아무 곳에 나) 내에서 고유 해야 하는 패키지 식별자입니다. 이 따라 [지침](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) 만 URL에 사용할 수 있는 문자를 사용 하 고 (공백 없이 대부분의 특수 문자를 방지 하 고).
- **버전** – 버전 번호와 일치 선택 [NuGet의 버전 관리 규칙](https://docs.microsoft.com/nuget/create-packages/dependency-versions)합니다.
- **작성자** – 이름 쉼표로 구분한 목록입니다.
- **설명** – 사용자가 패키지를 선택 하는 경우 표시 되는 패키지의 기능의 개요입니다.

> [!NOTE]
> NuGet 또는 다른 사용자에 게 배포 하기 위해 새 버전을 작성할 때 버전 번호를 증분 해야 합니다.

자세한 내용은 참조는 [요소 참조 필요한](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements) 에 자세한 내용은에 이러한 자세한 지침 대로 [고유한 패키지 식별자를 선택 하 고 버전 번호를 설정](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) 및 [ 패키지 유형을 설정](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type)합니다.

> [!IMPORTANT]
> 이 탭에서 모든 필드를 입력 해야 합니다. 그렇지 않은 경우 오류 메시지가 표시 됩니다: _"프로젝트는 없으므로 NuGet 메타 데이터는 NuGet 패키지 생성 되지 것입니다. 프로젝트 옵션에서 메타 데이터 섹션에는 NuGet 패키지 메타 데이터를 지정할 수 있습니다 "_

## <a name="optional-metadata"></a>선택적 메타 데이터

**세부 정보** 탭 NuGet 패키지 매니페스트 파일에 포함 될 선택적 필드를 포함 합니다.

[![](metadata-images/metadata-detail-sml.png "NuGet 패키지의 선택적 메타 데이터 창")](metadata-images/metadata-detail.png#lightbox)

참조는 [참조 하는 요소에는 선택적](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements) 필수 및 선택적 필드에 대 한 자세한 내용은 합니다.

> [!NOTE]
> NuGet 패키지에 배포 되는 경우 [NuGet.org](https://www.nuget.org) 가능한 한 많은 정보를 제공 하는 것이 좋습니다.


## <a name="related-links"></a>관련 링크

- [.nuspec 참조](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
