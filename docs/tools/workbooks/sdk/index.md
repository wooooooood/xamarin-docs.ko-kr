---
title: "Xamarin 통합 문서 SDK 시작"
ms.topic: article
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 3978b046a8ab4d42cbf86bf524452a033b5dbb4d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Xamarin 통합 문서 SDK 시작

이 문서에서는 Xamarin 통합 문서에 대 한 통합 개발 시작 하기 위한 요약 가이드가 제공 합니다. 이 중 상당 부분이 안정적인 Xamarin 통합 문서와 함께 작동 하지만 **로드 NuGet 패키지를 통해 통합은 통합 문서 1.3에서 에서만 지원 됩니다**, 알파 채널 작성 시에 있습니다.

# <a name="general-overview"></a>일반 개요

Xamarin 통합 문서 통합은 사용 하는 작은 라이브러리는 [ `Xamarin.Workbooks.Integrations` NuGet] [ nuget] 향상 된 환경을 제공 하도록 에이전트 Xamarin 통합 문서 및 관리자와 통합 하는 SDK입니다.

통합 개발을 시작 하기 위한 주요 3 단계가 있습니다-여기에 간략하게 설명 하 합니다.

## <a name="creating-the-integration-project"></a>통합 프로젝트 만들기

가장 잘 통합 라이브러리는 다중 플랫폼 라이브러리도 개발 됩니다. 사용 가능한 에이전트, 과거 및 미래에 최상의 통합 기능을 제공 하려고 하기 때문에 광범위 하 게 지원 되는 라이브러리의 집합을 선택 합니다. 광범위 한 지원에 대 한 "이식 가능한 라이브러리" 템플릿을 사용 하는 것이 좋습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![이식 가능한 라이브러리 Mac 용 Visual Studio 템플릿](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![이식 가능한 라이브러리 템플릿 Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

Visual Studio에서 이식 가능한 라이브러리에 대 한 다음 대상 플랫폼을 선택 했는지 확인 하려면:

[![이식 가능한 라이브러리 플랫폼 Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

라이브러리 프로젝트를 만든 후에 대 한 참조를 추가 우리의 `Xamarin.Workbooks.Integration` NuGet 라이브러리 NuGet 패키지 관리자를 통해 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Mac 용 NuGet Visual Studio](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

프로젝트의 일부로 한 만든 빈 클래스를 삭제 하려는-있하지 않습니다 이후에 필요 것이 있습니다. 다음이 단계를 완료 했으면 통합 작성을 시작할 준비가 된 것입니다.

## <a name="building-an-integration"></a>통합 빌드

간단한 통합을 빌드합니다. 실제로 언제 색 녹색, 각 개체에는 표현으로 녹색 추가 하도록 합니다. 먼저 라는 새 클래스를 만듭니다 `SampleIntegration`를 구현 하는 것을 확인 하 고 우리의 [ `IAgentIntegration` ] [ integration-type] 인터페이스:

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

추가 원하는 작업을 수행 하는 [표현](~/tools/workbooks/sdk/representations.md) 은 녹색이 하는 모든 개체에 대 한 합니다. 표현 공급자를 사용 하 여 수행 합니다. 상속 하는 공급자는 [ `RepresentationProvider` ] [ reppr] 클래스-짐에 대 한 त ु म 재정의 [ `ProvideRepresentations` ] [ prrep]:

```csharp
using Xamarin.Interactive.Representations;

class SampleRepresentationProvider : RepresentationProvider
{
    public override IEnumerable<object> ProvideRepresentations (object obj)
    {
        // This corresponds to Pantone 2250 XGC, our favorite color.
        yield return new Color (0.0, 0.702, 0.4314);
    }
}
```

에서는 반환 되는 [ `Color` ] [ color], 미리 만들어진이 SDK의 표현 형식입니다.
여기에 반환 형식의 임을 알 수는 `IEnumerable<object>` &mdash;표현 공급자는 개체에 대 한 많은 표현을 반환할 수 있습니다! 모든 표현 공급자는 것이 중요 하 게 전달 되 고 어떤 개체에 대 한 어떠한가 정도 하지 모든 개체에 대해 호출 됩니다.

마지막 단계는 실제로 우리 공급자 에이전트를 등록 하 고 통합 문서는 통합 형식을 찾을 수 있는 위치를 알 것입니다. 이 코드를 추가할 공급자를 등록 하려면는 `IntegrateWith` 에서 메서드는 `SampleIntegration` 앞에서 만든 클래스:

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

통합 형식을 설정 어셈블리 수준 특성을 통해 수행 됩니다. 편의 위해 통합 종류로 동일한 클래스 또는 프로그램 AssemblyInfo.cs에 넣을 수 있습니다.

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

개발 하는 동안 알게 될 수 있습니다이 사용 하는 편리한 방법을 [ `AddProvider` 오버 로드] [ addprovider] 에 `RepresentationManager` 통합 문서 표현을 제공 하려면 간단한 콜백을 등록할 수 있도록 하는 로 이동한 다음 해당 코드를 프로그램 `RepresentationProvider` 완료 되 면 구현 합니다. 렌더링에 대 한 예는 [ `OxyPlot` ] [ oxyplot] `PlotModel` 다음과 같을 수 있습니다.

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> 이러한 Api 신속 하 게 시작 하 고, 실행할 수 있지만을 사용 하 여 전체 통합 전달 권장 하지 않을 것&mdash;거의 제어할 형식이 클라이언트에서 처리 되는 방법을 제공 합니다.

등록 된 표현으로 통합을 제공할 준비가 되었습니다!

## <a name="shipping-your-integration"></a>통합 전달

통합을 배송 하려면 NuGet 패키지에 추가 해야 합니다.
기존 라이브러리의 NuGet이 포함 된 배송할 수 또는 새 패키지를 만드는 경우 시작 지점으로이 템플릿을.nuspec 파일을 사용할 수 있습니다.
데이터를 통합 관련이 있는 섹션 입력 해야 합니다. 가장 중요 한 부분은 모든 통합에 대 한 파일에 있어야 한다는 `xamarin.interactive` 패키지의 루트 디렉터리입니다. 이 속성을 사용 하면 기존 패키지를 사용 하거나 새로 만들 여부에 관계 없이 통합에 대 한 모든 관련 파일을 쉽게 찾을 수 있습니다.

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
    <metadata>
      <id>$YourNuGetPackage$</id>
      <version>$YourVersion$</version>
      <authors>$YourNameHere$</authors>
      <projectUrl>$YourProjectPage$</projectUrl>
      <description>A short description of your library.</description>
    </metadata>
    <files>
      <file src="Path\To\Your\Integration.dll" target="xamarin.interactive" />
    </files>
</package>
```

NuGet을 압축할 수.nuspec 파일을 만든 후 다음과 같이 합니다.

```csharp
nuget pack MyIntegration.nuspec
```

정책을 게시 하 고 [NuGet][nugetorg]합니다. 발생 되 면 원하는 통합 문서에서 참조 하 고 작동을 확인 수 있습니다. 아래 스크린샷에서이 문서에 빌드 및 통합 문서에는 NuGet 패키지를 설치 하는 샘플 통합을 패키지 했습니다 했습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![통합 문서와 통합](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![통합 문서와 통합](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

표시 되지 않는 모든 공지 `#r` 지시문 이나 통합 초기화에 아무 것도-통합 문서에 자동으로 처리 하는 모든 드립니다 내부적!

## <a name="next-steps"></a>다음 단계

SDK를 구성 하는 이동 요소에 대 한 자세한 내용은 밖의 설명서를 확인해 보세요 및 [통합 예제](~/tools/workbooks/samples/index.md) 에서 실행 되는 사용자 지정 JavaScript를 제공 하는 등의 통합을 수행할 수 있는 추가 작업에 대 한 통합 문서 클라이언트입니다.

[integration-type]: /api/type/Xamarin.Interactive.IAgentIntegration/
[repman-api]: /api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[color]: /api/type/Xamarin.Interactive.Representations.Color/
[xir]: /api/namespace/Xamarin.Interactive.Representations/
[reppr]: /api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[prrep]: /api/member/Xamarin.Interactive.Representations.RepresentationProvider.ProvideRepresentations/p/System.Object/
[nugetorg]: https://nuget.org
[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
[addprovider]: /api/member/Xamarin.Interactive.Representations.IRepresentationManager.AddProvider/
[oxyplot]: http://www.oxyplot.org/
