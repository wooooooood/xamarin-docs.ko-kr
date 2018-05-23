---
title: NuGet 구성 요소 참조를 업데이트합니다.
description: 앱을 미래 보증 NuGet 패키지와 구성 요소 참조를 대체 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
author: asb3993
ms.author: amburns
ms.date: 04/18/2018
ms.openlocfilehash: e9c056d37577a280b66bb3aa7ded19e966bca43b
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2018
---
# <a name="updating-component-references-to-nuget"></a>NuGet 구성 요소 참조를 업데이트합니다.

> [!IMPORTANT]
> 구성 요소 저장소가 2018 년 5 월 15를 기준으로 중지 되었습니다 (이 클로저 원래 [발표](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/) 2017 년 11 월에에서).
>
> Xamarin 구성 요소 Visual Studio에서 더 이상 지원 되지 NuGet 패키지에 의해 교체 해야 합니다. 프로젝트에서 구성 요소 참조를 수동으로 제거 하려면 다음 지침을 따릅니다.

NuGet 패키지에 추가 하는 데 이러한 지침을 참조 [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) 또는 [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)합니다.

인기 있는 Xamarin 목록이 [플러그 인 및 라이브러리](https://github.com/xamarin/XamarinComponents/blob/master/README.md) NuGet 패키지로 사용할 수 없는 구성 요소에 대 한 대안을 찾을 수 있도록 사용할 수 있습니다.

## <a name="manually-removing-component-references"></a>구성 요소 참조를 수동으로 제거

15.6 버전의 Visual Studio 및 Mac 용 Visual Studio의 7.4 릴리스에서 더 이상 프로젝트에서 구성 요소를 지원 합니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio에는 프로젝트를 로드 하는 경우는 수동으로 제거 해야 모든 구성 요소 프로젝트에서 설명 하는 다음과 같은 대화 상자가 표시 됩니다.

![경고 대화를 설명 하는 구성 요소 프로젝트에 및를 제거 해야 합니다](component-nuget-images/component-alert-vs.png)

제거 하려면 구성 요소 프로젝트에서:

1. 열기는 **.csproj** 파일입니다. 이렇게 하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **프로젝트 언로드**합니다. 

2. 언로드된 프로젝트에 다시를 마우스 오른쪽 단추로 클릭 하 고 선택 **{your-프로젝트-name}.csproj 편집**합니다.

3. 파일에 대 한 모든 참조 찾기 `XamarinComponentReference`합니다. 다음 예제와 비슷한 같아야 합니다.

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

4. 에 대 한 참조를 제거 `XamarinComponentReference` 파일을 저장 합니다. 위의 예에서 전체를 제거 해도 안전은 `ItemGroup`합니다.

5. 파일을 저장 후 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **프로젝트 다시 로드**합니다.

6. 솔루션의 각 프로젝트에 대해 위의 단계를 반복 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 용 Visual Studio에는 프로젝트를 로드 하는 경우는 수동으로 제거 해야 모든 구성 요소 프로젝트에서 설명 하는 다음과 같은 대화 상자가 표시 됩니다.

![경고 대화를 설명 하는 구성 요소 프로젝트에 및를 제거 해야 합니다](component-nuget-images/component-alert.png)

제거 하려면 구성 요소 프로젝트에서:

1. .Csproj 파일을 엽니다. 이렇게 하려면 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **도구 > 파일 편집**합니다.

2. 파일에 대 한 모든 참조 찾기 `XamarinComponentReference`합니다. 다음 예제와 비슷한 같아야 합니다.

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

3. 에 대 한 참조를 제거 `XamarinComponentReference` 파일을 저장 합니다. 위의 예에서 것이 안전 전체를 제거 하려면 `ItemGroup`

4. 솔루션의 각 프로젝트에 대해 위의 단계를 반복 합니다.

-----

> [!WARNING]
> 다음 지침에서는 이전 버전의 Visual Studio 에서만 작동합니다.
> **구성 요소** 노드 Visual Studio 2017 또는 Mac.에 대 한 Visual Studio의 현재 릴리스에서 사용할 수 없게 됩니다.

다음 섹션에는 NuGet 패키지에 구성 요소 참조를 변경 하려면 기존 Xamarin 솔루션을 업데이트 하는 방법을 설명 합니다.

- [NuGet 패키지를 포함 하는 구성 요소](#contain)
- [NuGet 대체 된 구성 요소](#replace)

대부분의 구성 요소에서 위 범주 중 하나에 해당 합니다.
해당 하는 NuGet 패키지를 갖고, 읽기에 표시 되지 않는 구성 요소를 사용 하는 경우는 [NuGet 마이그레이션 경로가 없는 구성 요소](#require-update) 아래 섹션.

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
- [인기 있는 Xamarin 플러그 인 및 라이브러리의 목록](https://github.com/xamarin/XamarinComponents/blob/master/README.md)
- [설치 하 고 사용 NuGet 패키지 (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [NuGet 패키지 (Mac) 포함 하 여](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
