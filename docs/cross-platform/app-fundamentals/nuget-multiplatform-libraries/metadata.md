---
title: NuGet 메타 데이터 편집
description: 이 문서에서는 프로젝트 옵션을 사용 하 여 다중 플랫폼 라이브러리의 NuGet 메타 데이터를 편집 하는 방법을 설명 합니다. 필수 및 선택적 메타 데이터 모두에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: bf8efad28c7ec6acfd0e43403e8db14639a3c755
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289404"
---
# <a name="editing-nuget-metadata"></a>NuGet 메타 데이터 편집

_프로젝트 옵션을 사용 하 여 다중 플랫폼 라이브러리의 NuGet 메타 데이터 편집_

라이브러리 프로젝트 형식 (예: PCL 또는 .NET Standard 또는 새 NuGet 프로젝트 형식)에는 **프로젝트 옵션** 창에 **NuGet 패키지** 섹션이 있습니다.

**메타 데이터** 섹션은 [ **nuspec** NuGet 패키지 매니페스트 파일](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file)에 사용 되는 값을 구성 합니다.

## <a name="required-information"></a>필요한 정보

**일반** 탭에는 NuGet 패키지를 생성 하기 위해 입력 해야 하는 4 개의 필드가 있습니다.

[![](metadata-images/metadata-general-sml.png "NuGet 패키지 필수 메타 데이터 창")](metadata-images/metadata-general.png#lightbox)

- **ID** – Nuget.org 내에서 고유 해야 하는 패키지 식별자입니다 (또는 패키지가 배포 될 때마다). 이 [지침](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) 을 따르고 URL에 유효한 문자만 사용 합니다 (공백 없음 및 대부분의 특수 문자 사용 안 함).
- **버전** – [NuGet의 버전 관리 규칙과](https://docs.microsoft.com/nuget/create-packages/dependency-versions)일치 하는 버전 번호를 선택 합니다.
- **작성자** -쉼표로 구분 된 이름 목록입니다.
- **설명** – 사용자가 패키지를 선택할 때 표시 되는 패키지 기능에 대 한 개요입니다.

> [!NOTE]
> NuGet 또는 다른 사용자에 게 배포할 새 버전을 빌드할 때 버전 번호를 증분 해야 합니다.

자세한 내용은 [필수 요소 참조](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements) 를 참조 하세요. 자세한 내용은 [고유한 패키지 식별자를 선택 하 고 버전 번호를 설정](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) 하 고 [패키지 유형을 설정](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type)하는 방법에 대 한 자세한 지침을 참조 하세요.

> [!IMPORTANT]
> 이 탭의 모든 필드를 입력 해야 합니다. 그렇지 않으면 다음과 같은 오류 메시지가 표시 됩니다. _"프로젝트는 NuGet 메타 데이터를 포함 하지 않으므로 NuGet 패키지를 만들지 않습니다. NuGet 패키지 메타 데이터는 프로젝트 옵션의 메타 데이터 섹션에서 지정할 수 있습니다._

## <a name="optional-metadata"></a>선택적 메타 데이터

**세부 정보** 탭에는 NuGet 패키지 매니페스트 파일에 포함할 선택적 필드가 있습니다.

[![](metadata-images/metadata-detail-sml.png "NuGet 패키지 선택적 메타 데이터 창")](metadata-images/metadata-detail.png#lightbox)

필수 및 선택적 필드에 대 한 자세한 내용은 [선택적 요소 참조](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements) 를 참조 하세요.

> [!NOTE]
> NuGet 패키지를 [NuGet.org](https://www.nuget.org) 에 배포 하는 경우 최대한 많은 정보를 제공 하는 것이 좋습니다.


## <a name="related-links"></a>관련 링크

- [.nuspec 참조](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
