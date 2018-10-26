---
title: Xamarin Workbooks SDK 시작
description: 이 문서에서는 Xamarin Workbooks 용 통합 개발에 사용할 수 있는 Xamarin SDK 통합 문서를 사용 하 여 시작 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 5800e98acbff147735ae4a6125979a4b47be2367
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115369"
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Xamarin Workbooks SDK 시작

이 문서에서는 Xamarin Workbooks 용 통합 개발 시작 하기 위한 요약 가이드가 제공 합니다. 안정적인 Xamarin Workbooks,이 중 상당 부분이 작동 하지만 **통합 문서 1.3 통합 NuGet 패키지를 로드 에서만 지원 됩니다**를 작성할 당시에는 알파 채널에서입니다.

## <a name="general-overview"></a>일반 개요

Xamarin Workbooks 통합이 사용 하는 작은 라이브러리는 [ `Xamarin.Workbooks.Integrations` NuGet] [ nuget] Xamarin Workbooks 및 Inspector를 사용 하 여 향상 된 환경을 제공 하도록 에이전트를 통합 하는 SDK입니다.

통합 개발을 시작 하기 위한 주요 단계 3 가지-여기에 간략하게 설명 합니다.

## <a name="creating-the-integration-project"></a>통합 프로젝트 만들기

가장 잘 통합 라이브러리는 다중 플랫폼 라이브러리로 개발 됩니다. 모든 사용 가능한 에이전트, 과거 및 미래의에서 최상의 통합을 제공 하려고 하기 때문에 광범위 하 게 지원 되는 라이브러리 집합을 선택 하는 것이 좋습니다. 광범위 하 게 지원에 대 한 "이식 가능한 라이브러리" 템플릿을 사용 하는 것이 좋습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![이식 가능한 라이브러리 Mac 용 Visual Studio 템플릿](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![이식 가능한 라이브러리 템플릿 Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

Visual Studio에서 이식 가능한 라이브러리에 대 한 다음 대상 플랫폼을 선택 했는지 확인 해야 합니다.

[![이식 가능한 라이브러리 플랫폼에 대 한 Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

라이브러리 프로젝트를 만든 후 추가에 대 한 참조는 `Xamarin.Workbooks.Integration` NuGet 라이브러리 NuGet 패키지 관리자를 통해.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![NuGet Visual Studio for Mac](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

프로젝트의 일부로 만든 빈 클래스를 삭제 하는 것이 좋습니다-있습니다 필요 것이 있습니다. 다음이 단계를 완료 했으면, 통합을 빌드할 준비가 되었습니다.

## <a name="building-an-integration"></a>통합 빌드

간단한 통합을 빌드 해 보겠습니다. 우리가 실제로 좋아하는 색 녹색, 각 개체에 표현으로 녹색을 추가 해 보겠습니다. 먼저 라는 새 클래스를 만듭니다 `SampleIntegration`를 구현 하 고 우리의 [ `IAgentIntegration` ] [ integration-type] 인터페이스:

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

추가 하는 원하는 작업을 수행 하는 [표현을](~/tools/workbooks/sdk/representations.md) 녹색이 되는 모든 개체에 대 한 합니다. 표현을 공급자를 사용 하 여이 작업을 수행 합니다. 공급자에서 상속 된 [ `RepresentationProvider` ] [ reppr] 클래스-짐에 대 한 하기만 재정의 [ `ProvideRepresentations` ] [ prrep]:

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

반환 된 [ `Color` ] [ color], 미리 작성 된 SDK의 표현 형식입니다.
여기에 반환 형식을 인지 하는 것이 보면를 `IEnumerable<object>` &mdash;표현을 공급자 개체에 대 한 여러 표현을 반환할 수 있습니다! 에 전달 되는 개체에 대 한 가정을 하지 해야 하므로 모든 개체에 대 한 모든 표현 공급자 라고 합니다.

마지막 단계는 실제로 에이전트를 사용 하 여 공급자를 등록 하 고 통합 문서는 통합 형식을 찾을 수 있는 위치를 지시 하는 것입니다. 공급자를 등록 하려면이 코드를 추가 합니다 `IntegrateWith` 의 메서드는 `SampleIntegration` 앞에서 만든 클래스:

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

통합 유형 어셈블리 수준 특성을 통해 이루어집니다. 에 AssemblyInfo.cs 또는 편의 위해 통합 형식으로 동일한 클래스에서이 넣을 수 있습니다.

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

개발 하는 동안 있습니다이 사용 하는 편리한 [ `AddProvider` 오버 로드] [ addprovider] 에서 `RepresentationManager` 통합 문서 내 표현을 제공 하는 간단한 콜백을 등록할 수 있는 해당 코드를 이동 하 고 프로그램 `RepresentationProvider` 완료 되 면 구현 합니다. 렌더링에 대 한 예제는 [ `OxyPlot` ] [ oxyplot] `PlotModel` 다음과 같을 수 있습니다.

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> 이러한 Api 시작 및 실행 하는 빠른 방법을 제공 하지만 사용만 전체 통합을 전달 하지는 것이 좋습니다&mdash;형식을 클라이언트에서 처리 되는 방식을 거의 제어를 제공 합니다.

등록 된 표현을 사용 하 여 통합 전달할 준비가 되었습니다!

## <a name="shipping-your-integration"></a>통합 전달

통합을 제공 하는 NuGet 패키지에 추가 해야 합니다.
기존 라이브러리의 NuGet을 사용 하 여 발송할 수 있습니다 하거나 새 패키지를 만드는 경우이 템플릿을.nuspec 파일을 시작 점으로 사용할 수 있습니다.
통합 관련이 있는 섹션을 작성 해야 합니다. 가장 중요 한 부분은 모든 통합에 대 한 파일에 있어야 한다는 `xamarin.interactive` 패키지의 루트 디렉터리입니다. 기존 패키지를 사용 하거나 새로 만듭니다 여부에 관계 없이 통합에 대 한 모든 관련 파일을 쉽게 찾을 수 있습니다.

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

게시 하 고 [NuGet][nugetorg]합니다. 것 되 면 모든 통합 문서에서 참조 하 여 작동을 확인 수 있습니다. 아래 스크린샷에서이 문서의 작성 하 고 통합 문서에 NuGet 패키지를 설치 하는 샘플 통합을 패키징 한 했습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![통합을 사용 하 여 통합 문서](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![통합을 사용 하 여 통합 문서](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

모든 표시 되지 않으면 알림 `#r` 지시문 이나 통합 초기화에 아무 것도-통합 문서에 처리 하는 모든를 백그라운드에서!

## <a name="next-steps"></a>다음 단계

SDK를 구성 하는 이동 부분에 대 한 자세한 정보에 대 한 다른 설명서를 확인 및 [통합 샘플](~/tools/workbooks/samples/index.md) 에서 실행 되는 사용자 지정 JavaScript를 제공 하는 등, 통합에서 수행할 수 있는 하는 추가 작업에 대 한 통합 문서 클라이언트입니다.

[integration-type]: https://developer.xamarin.com/api/type/Xamarin.Interactive.IAgentIntegration/
[repman-api]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[xir]: https://developer.xamarin.com/api/namespace/Xamarin.Interactive.Representations/
[reppr]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[prrep]: https://developer.xamarin.com/api/member/Xamarin.Interactive.Representations.RepresentationProvider.ProvideRepresentations/p/System.Object/
[nugetorg]: https://nuget.org
[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
[addprovider]: https://developer.xamarin.com/api/member/Xamarin.Interactive.Representations.IRepresentationManager.AddProvider/
[oxyplot]: http://www.oxyplot.org/
