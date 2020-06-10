---
title: NuGet에 대 한 구성 요소 참조 업데이트
description: 이 문서에서는 Xamarin 구성 요소 저장소가 더 이상 사용 되지 않기 때문에 NuGet 패키지로 구성 요소 참조를 대체 하 여 앱을 증명 하는 방법에 대해 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
author: davidortinau
ms.author: daortin
ms.date: 04/18/2018
ms.openlocfilehash: f81df8ac253e53b16c3ab09bf80d66a7b6324854
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571521"
---
# <a name="updating-component-references-to-nuget"></a>NuGet에 대 한 구성 요소 참조 업데이트

> [!IMPORTANT]
> 구성 요소 저장소는 2018 년 5 월 15 일에 중단 되었습니다 (이 클로저는 원래 11 월 2017에 [발표](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/) 됨).
>
> Xamarin 구성 요소는 Visual Studio에서 더 이상 지원 되지 않으며 NuGet 패키지로 대체 되어야 합니다. 프로젝트에서 구성 요소 참조를 수동으로 제거 하려면 아래 지침을 따르세요.

[Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) 또는 [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)에서 NuGet 패키지를 추가 하는 방법은 다음 지침을 참조 하세요.

NuGet 패키지로 사용할 수 없는 구성 요소에 대 한 대안을 찾는 데 도움이 되는 인기 있는 Xamarin [플러그 인 및 라이브러리](https://github.com/xamarin/XamarinComponents/blob/master/README.md) 목록이 제공 됩니다.

## <a name="manually-removing-component-references"></a>수동으로 구성 요소 참조 제거

Mac용 Visual Studio의 Visual Studio 및 7.4 릴리스 15.6 릴리스는 더 이상 프로젝트의 구성 요소를 지원 하지 않습니다. 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio에 프로젝트를 로드 하는 경우 프로젝트에서 수동으로 구성 요소를 제거 해야 함을 설명 하는 다음과 같은 대화 상자가 표시 됩니다.

![프로젝트에서 구성 요소를 찾았지만 제거 해야 함을 설명 하는 경고 대화 상자](component-nuget-images/component-alert-vs.png)

프로젝트에서 구성 요소를 제거 하려면 다음을 수행 합니다.

1. **.csproj** 파일을 엽니다. 이렇게 하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 언로드**를 선택 합니다. 

2. 언로드된 프로젝트를 마우스 오른쪽 단추로 다시 클릭 하 고 **{프로젝트 이름} .Csproj 편집**을 선택 합니다.

3. 파일에서에 대 한 참조를 찾습니다 `XamarinComponentReference` . 다음 예제와 유사 하 게 표시 됩니다.

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

4. 에 대 한 참조를 제거 `XamarinComponentReference` 하 고 파일을 저장 합니다. 위의 예제에서는 전체를 제거 하는 것이 안전 `ItemGroup` 합니다.

5. 파일이 저장 되 면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 다시 로드**를 선택 합니다.

6. 솔루션의 각 프로젝트에 대해 위의 단계를 반복 합니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

프로젝트를 Mac용 Visual Studio 로드 하는 경우 프로젝트에서 수동으로 구성 요소를 제거 해야 함을 설명 하는 다음과 같은 대화 상자가 표시 됩니다.

![프로젝트에서 구성 요소를 찾았지만 제거 해야 함을 설명 하는 경고 대화 상자](component-nuget-images/component-alert.png)

프로젝트에서 구성 요소를 제거 하려면 다음을 수행 합니다.

1. .csproj 파일을 엽니다. 이렇게 하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **도구 > 파일 편집**을 선택 합니다.

2. 파일에서에 대 한 참조를 찾습니다 `XamarinComponentReference` . 다음 예제와 유사 하 게 표시 됩니다.

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

3. 에 대 한 참조를 제거 `XamarinComponentReference` 하 고 파일을 저장 합니다. 위의 예제에서 전체를 제거 하는 것이 안전 합니다.`ItemGroup`

4. 솔루션의 각 프로젝트에 대해 위의 단계를 반복 합니다.

-----

> [!WARNING]
> 다음 지침은 이전 버전의 Visual Studio 에서만 작동 합니다.
> **구성 요소** 노드는 Visual Studio 2017 또는 Mac용 Visual Studio의 현재 릴리스에서 더 이상 사용할 수 없습니다.

다음 섹션에서는 기존 Xamarin 솔루션을 업데이트 하 여 NuGet 패키지에 대 한 구성 요소 참조를 변경 하는 방법을 설명 합니다.

- [NuGet 패키지를 포함 하는 구성 요소](#contain)
- [NuGet 대체를 사용 하는 구성 요소](#replace)

대부분의 구성 요소는 위의 범주 중 하나에 속합니다.
동일한 NuGet 패키지를 포함 하지 않는 구성 요소를 사용 하는 경우 아래 [nuget 마이그레이션 경로를 사용 하지 않고 구성 요소](#require-update) 섹션을 참조 하세요.

<a name="contain"></a>

## <a name="components-that-contain-nuget-packages"></a>NuGet 패키지를 포함 하는 구성 요소

대부분의 구성 요소에는 NuGet 패키지가 이미 포함 되어 있으며, 마이그레이션 경로는 단순히 구성 요소 참조를 삭제 하는 것입니다.

솔루션의 구성 요소를 두 번 클릭 하 여 구성 요소에 NuGet 패키지가 이미 포함 되어 있는지 여부를 확인할 수 있습니다.

![구성 요소 노드 확장](component-nuget-images/solution-sml.png)

**패키지** 탭은 구성 요소에 포함 된 NuGet 패키지를 나열 합니다.

![패키지 탭에 NuGet 포함](component-nuget-images/packages-tab-sml.png)

**어셈블리** 탭은 비어 있습니다.

![어셈블리 탭이 비어 있습니다.](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>솔루션 업데이트

솔루션을 업데이트 하려면 솔루션에서 **구성 요소** 항목을 삭제 합니다.

![구성 요소 삭제](component-nuget-images/delete-component-sml.png)

NuGet 패키지는 **패키지** 노드에 나열 된 상태를 유지 하 고 앱이 정상적으로 컴파일되고 실행 됩니다. 나중에이 패키지의 업데이트는 **NuGet** 업데이트 기능을 통해 수행 됩니다.

![NuGet 패키지 업데이트](component-nuget-images/nuget-update-sml.png)

<a name="replace"></a>

## <a name="components-with-nuget-replacements"></a>NuGet 대체를 사용 하는 구성 요소

구성 요소 정보 페이지 **어셈블리** 탭에 아래와 같은 항목이 포함 된 경우 해당 NuGet 패키지를 수동으로 찾아야 합니다.

![어셈블리 포함](component-nuget-images/assemblies-tab-sml.png)

**패키지** 탭은 비어 있을 수 있습니다.

![](component-nuget-images/packages-tab-empty-sml.png)

_NuGet 종속성을 포함할 수 있지만 무시할 수 있습니다._

대체 NuGet 패키지가 있는지 확인 하려면 구성 요소 이름 또는 작성자를 사용 하 여 [NuGet.org](https://www.nuget.org/packages)를 검색 합니다.

예를 들어 다음을 검색 하 여 인기 있는 **sqlite-net-pcl** 패키지를 찾을 수 있습니다.

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl)– 제품 이름입니다.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum)– 작성자의 프로필입니다.

### <a name="updating-the-solution"></a>솔루션 업데이트

NuGet에서 구성 요소를 사용할 수 있는지 확인 한 후에는 다음 단계를 수행 합니다.

#### <a name="delete-the-component"></a>구성 요소 삭제

솔루션의 구성 요소를 마우스 오른쪽 단추로 클릭 하 고 **제거**를 선택 합니다.

![구성 요소 제거](component-nuget-images/remove-component-sml.png)

그러면 구성 요소와 참조가 삭제 됩니다. 이렇게 하면 해당 하는 NuGet 패키지를 추가 하 여 대체할 때까지 빌드가 중단 됩니다.

#### <a name="add-the-nuget-package"></a>Nuget 패키지 추가

1. **패키지** 노드를 마우스 오른쪽 단추로 클릭 하 고 **패키지 추가**...를 선택 합니다.
2. 이름 또는 작성자에 의해 NuGet 대체를 검색 합니다.

    ![](component-nuget-images/nuget-search-sml.png)

3. **패키지 추가**를 누릅니다.

NuGet 패키지는 종속성과 함께 프로젝트에 추가 됩니다.
빌드를 수정 해야 합니다. 빌드가 계속 실패할 경우 각 오류를 조사 하 여 구성 요소와 NuGet 패키지 간에 API 차이가 있는지 확인 합니다.

<a name="require-update"></a>

## <a name="components-without-a-nuget-migration-path"></a>NuGet 마이그레이션 경로가 없는 구성 요소

응용 프로그램에 사용 되는 구성 요소에 대 한 대체를 즉시 찾지 못할 경우 걱정 하지 마세요. 기존 구성 요소는 Visual Studio 15.5에서 계속 작동 하며 **구성 요소** 노드는 평소와 같이 솔루션에 표시 됩니다.

그러나 이후 Visual Studio 릴리스는 구성 요소를 복원 하거나 업데이트 _하지 않습니다_ .
즉, 새 컴퓨터에서 솔루션을 여는 경우 구성 요소가 다운로드 및 설치 되지 않습니다. 작성자가 업데이트를 제공할 수 없습니다. 다음을 계획 해야 합니다.

- 구성 요소에서 어셈블리를 추출 하 고 프로젝트에서 직접 참조 합니다.
- 구성 요소 작성자에 게 문의 하 고 NuGet로 마이그레이션할 계획을 요청 합니다.
- 다른 NuGet 패키지를 조사 하거나, 구성 요소가 오픈 소스인 경우 소스 코드를 검색 합니다.

많은 구성 요소 공급 업체는 아직 NuGet으로 마이그레이션하는 작업을 수행 하 고 있으며 기타 (상업적으로 제공 되는 제품 포함)는 대체 배달 옵션을 조사할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [인기 있는 Xamarin 플러그 인 및 라이브러리 목록](https://github.com/xamarin/XamarinComponents/blob/master/README.md)
- [NuGet 패키지 설치 및 사용 (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [NuGet 패키지 포함 (Mac)](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
