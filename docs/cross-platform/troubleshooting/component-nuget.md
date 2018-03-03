---
title: "NuGet 구성 요소 참조를 업데이트합니다."
description: "앱을 미래 보증 NuGet 패키지와 구성 요소 참조를 대체 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: f3dbfb52d4fbcb4dd65f695a862f6b041d2b22c0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="updating-component-references-to-nuget"></a>NuGet 구성 요소 참조를 업데이트합니다.

_앱을 미래 보증 NuGet 패키지와 구성 요소 참조를 대체 합니다._

이 가이드에서는 NuGet 패키지에 구성 요소 참조를 변경 하려면 기존 Xamarin 솔루션을 업데이트 하는 방법에 설명 합니다.

- [NuGet 패키지를 포함 하는 구성 요소](#contain)
- [NuGet 대체 된 구성 요소](#replace)

대부분의 구성 요소에서 위 범주 중 하나에 해당 합니다.
해당 하는 NuGet 패키지를 갖고, 읽기에 표시 되지 않는 구성 요소를 사용 하는 경우는 [NuGet 마이그레이션 경로가 없는 구성 요소](#require-update) 아래 섹션.

자세한 지침은 NuGet 패키지에 추가 하기 위한 이러한 페이지를 참조 [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) 또는 [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)합니다.

<a name="contain" />

## <a name="components-that-contain-nuget-packages"></a>NuGet 패키지를 포함 하는 구성 요소

많은 구성 요소가 NuGet 패키지를 이미 포함 하 고 마이그레이션 경로 단순히에 구성 요소 참조를 삭제 합니다.

구성 요소는 솔루션의 구성 요소를 두 번 클릭 하 여 NuGet 패키지를 이미 포함 되는지 여부를 확인할 수 있습니다.

![확장 구성 요소 노드](component-nuget-images/solution-sml.png)

**패키지** 탭에서 구성 요소에 포함 된 모든 NuGet 패키지를 나열 합니다.

![NuGet을 포함 하는 패키지 탭](component-nuget-images/packages-tab-sml.png)

**어셈블리** 탭이 비어 있게 됩니다.

![어셈블리 탭 비어 있습니다.](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>솔루션을 업데이트합니다.

솔루션을 업데이트 하려면 삭제는 **구성 요소** 솔루션에서 항목:

![구성 요소를 삭제 합니다.](component-nuget-images/delete-component-sml.png)

NuGet 패키지에 나열 된 상태로 유지 됩니다는 **패키지** 노드 및 응용 프로그램을 컴파일하고 정상적으로 실행 합니다. 이 패키지에 대 한 업데이트를 통해 수행 됩니다는 나중에 **Nuget** 업데이트 기능:

![NuGet 패키지를 업데이트 합니다.](component-nuget-images/nuget-update-sml.png)


<a name="replace" />

## <a name="components-with-nuget-replacements"></a>NuGet 대체 된 구성 요소

하는 경우 구성 요소 정보 페이지 **어셈블리** 탭 아래와 같이 항목을 포함, 해당 하는 NuGet 패키지를 수동으로 찾을 해야 합니다.

![어셈블리를 포함합니다.](component-nuget-images/assemblies-tab-sml.png)

**패키지** 탭 아마도 비어 있게 됩니다.

![](component-nuget-images/packages-tab-empty-sml.png)

_NuGet 종속성 포함 될 수 있지만 이러한 무시 될 수 있습니다._


검색 한 대체 NuGet 패키지가 있는지를 확인 하려면 [NuGet.org](https://www.nuget.org/packages), 구성 요소 이름을 사용 하 여 또는 또는 만든이가 있습니다.

예를 들어, 찾을 수 있습니다는 인기 있는 **sqlite net pcl** 검색 하 여 패키지:

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl) -제품 이름입니다.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum) -작성자의 프로필입니다.


### <a name="updating-the-solution"></a>솔루션을 업데이트합니다.

NuGet에서 사용할 수 있는 구성 요소가 확인 하 고 나면 다음이 단계를 따르십시오.

#### <a name="delete-the-component"></a>구성 요소를 삭제 합니다.

솔루션의 구성 요소를 마우스 오른쪽 단추로 클릭 하 고 선택 **제거**:

![구성 요소를 제거 합니다.](component-nuget-images/remove-component-sml.png)

구성 요소 및 모든 참조를 삭제 합니다. 바꾸거나 해당 하는 NuGet 패키지를 추가 하기 전에 빌드를 끊어집니다.

#### <a name="add-the-nuget-package"></a>NuGet 패키지에 추가

1. 마우스 오른쪽 단추로 클릭는 **패키지** 노드 선택 **패키지 추가 중...** .
2. 이름이 나 작성자 NuGet 교체에 대 한 검색:

  ![](component-nuget-images/nuget-search-sml.png)

3. 키를 눌러 **패키지 추가**합니다.

NuGet 패키지는 모든 종속성과 함께 프로젝트에 추가 됩니다.
이 빌드를 수정 해야 합니다. 빌드 계속 실패 하면, NuGet 패키지와 구성 요소 간의 API 차이점 있었는지 확인 하려면 각 오류를 조사 합니다.

<a name="require-update" />

## <a name="components-without-a-nuget-migration-path"></a>NuGet 마이그레이션 경로가 없는 구성 요소

응용 프로그램에서 사용 되는 구성 요소에 대 한 대체 즉시 찾을 수 없는 경우에 고려 하지 마십시오. Visual Studio 15.5에서 작동 하도록 기존 구성 요소는 계속 및 **구성 요소** 노드 솔루션에 평소와 같이 표시 됩니다.

그러나 이후 Visual Studio 릴리스에서 _하지_ 복원 하거나 구성 요소를 업데이트 합니다.
즉, 새 컴퓨터에 솔루션을 열면 구성 요소가 없습니다 다운로드 되 고 설치 해야 합니다. 및 작성자를 한 업데이트를 제공 하는 일을 할 수 있습니다. 계획을 세워야 합니다.

* 어셈블리 구성 요소에서 추출 하 고 프로젝트에서 직접 참조할 합니다.
* 구성 요소를 만든을 NuGet로 마이그레이션할 계획에 대 한 요청.
* 대체 NuGet 패키지를 조사 하거나 경우에 오픈 소스 소스 코드를 검색 합니다.

대부분의 구성 요소 공급 업체에서 NuGet을로 마이그레이션 작업 및 다른 사용자 (상업적으로 사용할 수 있는 제품 포함) 다른 배달 옵션을 조사 하 게 될 수 있습니다.


## <a name="related-links"></a>관련 링크

- [설치 하 고 사용 NuGet 패키지 (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [NuGet 패키지 (Mac) 포함 하 여](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
