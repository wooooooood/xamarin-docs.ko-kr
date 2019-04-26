---
title: NuGet 메타 데이터 편집
description: 이 문서에서는 프로젝트 옵션을 사용 하 여 다중 플랫폼 라이브러리에 대 한 NuGet 메타 데이터를 편집 하는 방법을 설명 합니다. 필수 및 선택적 메타 데이터에 설명 합니다.
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 3680b02003a844668b0b5c97e5d4c0d296ae3500
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266884"
---
# <a name="editing-nuget-metadata"></a>NuGet 메타 데이터 편집

_프로젝트 옵션을 사용 하 여 다중 플랫폼 라이브러리에 대 한 NuGet 메타 데이터 편집_

라이브러리 프로젝트 형식 (예: PCL 또는.NET Standard 또는 새 NuGet 프로젝트 형식)에 **NuGet 패키지** 섹션을 **프로젝트 옵션** 창입니다.

합니다 **메타 데이터** 섹션에 사용 된 값을 구성 합니다 [ **.nuspec** NuGet 패키지 매니페스트 파일](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file)합니다.

## <a name="required-information"></a>필요한 정보

합니다 **일반** 탭에는 NuGet 패키지를 생성 하려면 입력 해야 하는 4 개의 필드가 있습니다.

[![](metadata-images/metadata-general-sml.png "NuGet 패키지 필수 메타 데이터 창")](metadata-images/metadata-general.png#lightbox)

- **ID** – Nuget.org (또는 패키지가 배포 될 때마다) 내에서 고유 해야 하는 패키지 식별자입니다. 이 따라 [지침](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) URL에서 유효한 문자만 사용 하 고 (공백 없이 대부분의 특수 문자를 방지 하 고).
- **버전** – 사용 하 여 일관 된 버전 번호를 선택할 [NuGet의 버전 관리 규칙](https://docs.microsoft.com/nuget/create-packages/dependency-versions)합니다.
- **작성자** – 이름의 쉼표로 구분 된 목록입니다.
- **설명** – 사용자는 패키지를 선택 하면 표시 되는 패키지의 기능 개요.

> [!NOTE]
> NuGet 또는 다른 사용자에 게 배포 하기 위해 새 버전을 빌드할 때 버전 번호를 증분 해야 합니다.

자세한 내용은 참조는 [필요한 요소 참조](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements) 자세한도 이러한 상세 지침에 따라 [고유한 패키지 식별자 선택 및 버전 번호 설정](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) 및 [ 패키지 유형 설정](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type)합니다.

> [!IMPORTANT]
> 이 탭의 모든 필드를 입력 해야 합니다. 그렇지 않으면 오류 메시지가 표시 됩니다. _"프로젝트에 없으므로 NuGet 메타 데이터는 NuGet 패키지 생성 되지 않습니다. 프로젝트 옵션의 메타 데이터 섹션에서 NuGet 패키지 메타 데이터를 지정할 수 있습니다 "_

## <a name="optional-metadata"></a>선택적 메타 데이터

합니다 **세부 정보** 탭 NuGet 패키지 매니페스트 파일에 포함 될 선택적 필드를 포함 합니다.

[![](metadata-images/metadata-detail-sml.png "NuGet 패키지의 선택적 메타 데이터 창")](metadata-images/metadata-detail.png#lightbox)

참조를 [요소에 대 한 선택적 참조](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements) 필수 및 선택적 필드에 대 한 자세한 내용은 합니다.

> [!NOTE]
> NuGet 패키지를 통해 배포 되는 경우 [NuGet.org](https://www.nuget.org) 최대한 많은 정보를 제공 하는 것이 좋습니다.


## <a name="related-links"></a>관련 링크

- [.nuspec 참조](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
