---
title: Xamarin Workbooks SDK 시작
description: 이 문서에서는 Xamarin Workbooks에 대 한 통합을 개발 하는 데 사용할 수 있는 Xamarin Workbooks SDK를 시작 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: 8e3dc65f9f615ff893f3526d53d99da25045c794
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283961"
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Xamarin Workbooks SDK 시작

이 문서에서는 Xamarin Workbooks에 대 한 통합 개발을 시작 하는 방법에 대 한 간략 한 가이드를 제공 합니다. 이 중 대부분은 안정적인 Xamarin Workbooks에서 작동 하지만 NuGet 패키지를 통해 통합을 로드 하는 것은 작성 시 알파 채널의 **통합 문서 1.3 에서만 지원 됩니다**.

## <a name="general-overview"></a>일반 개요

Xamarin Workbooks 통합은 [ `Xamarin.Workbooks.Integrations` NuGet][nuget] SDK를 사용 하 여 Xamarin Workbooks 및 검사기 에이전트와 통합 하 여 향상 된 환경을 제공 하는 작은 라이브러리입니다.

통합 개발을 시작 하기 위한 세 가지 주요 단계가 있습니다. 여기에서 간략하게 설명 합니다.

## <a name="creating-the-integration-project"></a>통합 프로젝트 만들기

통합 라이브러리는 다중 플랫폼 라이브러리로 가장 효과적으로 개발 됩니다. 과거와 미래의 모든 사용 가능한 에이전트에서 최상의 통합을 제공 하려는 경우 광범위 하 게 지원 되는 라이브러리 집합을 선택 하는 것이 좋습니다. 가장 광범위 한 지원을 위해 "이식 가능한 라이브러리" 템플릿을 사용 하는 것이 좋습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![이식 가능한 라이브러리 템플릿 Mac용 Visual Studio](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![이식 가능한 라이브러리 템플릿 Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

Visual Studio에서 이식 가능한 라이브러리에 대해 다음 대상 플랫폼을 선택 해야 합니다.

[![이식 가능한 라이브러리 플랫폼 Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

라이브러리 프로젝트를 만든 후에는 nuget 패키지 관리자를 통해 `Xamarin.Workbooks.Integration` nuget 라이브러리에 참조를 추가 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![NuGet Mac용 Visual Studio](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

프로젝트의 일부로 생성 된 빈 클래스를 삭제 하는 것이 좋습니다 .이에 대해서는 필요 하지 않습니다. 이러한 단계를 완료 하면 통합 빌드를 시작할 준비가 된 것입니다.

## <a name="building-an-integration"></a>통합 빌드

간단한 통합을 빌드할 예정입니다. 녹색을 정말 선호 하므로 각 개체에 대 한 표현으로 녹색 색을 추가 합니다. 먼저 이라는 `SampleIntegration`새 클래스를 만들고, `IAgentIntegration` 인터페이스를 구현 하도록 설정 합니다.

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

원하는 것은 녹색 인 모든 개체에 대 한 [표현을](~/tools/workbooks/sdk/representations.md) 추가 하는 것입니다. 표시 공급자를 사용 하 여이 작업을 수행 합니다. 공급자는 `RepresentationProvider` 클래스에서 상속 합니다. 즉,를 재정의 `ProvideRepresentations`해야 합니다.

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

SDK에서 미리 작성 `Color`된 표현 유형인를 반환 합니다.
`IEnumerable<object>` 여기&mdash;에서 반환 형식은 개체에 대 한 많은 표현을 반환 하는 것을 알 수 있습니다. 모든 개체에 대해 모든 표시 공급자가 호출 되므로 전달 되는 개체에 대 한 가정을 하지 않는 것이 중요 합니다.

마지막 단계는 실제로 공급자를 에이전트에 등록 하 고 통합 문서에 통합 형식을 제공 하는 것입니다. 공급자를 등록 하려면 앞에서 만든 `IntegrateWith` `SampleIntegration` 클래스의 메서드에 다음 코드를 추가 합니다.

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

통합 유형 설정은 어셈블리 차원의 특성을 통해 수행 됩니다. 이를 AssemblyInfo.cs에 배치 하거나 편의를 위해 통합 형식과 동일한 클래스에 넣을 수 있습니다.

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

개발 중에에서 `AddProvider` `RepresentationManager` 오버 로드를 사용 하는 것이 더 편리할 수 있습니다 .이를 통해 간단한 콜백을 등록 하 여 통합 문서 내에 표현을 제공한 다음 해당 코드를 `RepresentationProvider` 한 번에 구현으로 이동할 수 있습니다. 완료 되었습니다. [`OxyPlot`][oxyplot] 를`PlotModel` 렌더링 하는 예제는 다음과 같습니다.

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> 이러한 api를 사용 하면 신속 하 게 시작 하 고 실행할 수는 있지만, 전체 통합을 사용&mdash;하지 않는 것이 좋습니다.

표현이 등록 된 상태에서 통합을 배포할 준비가 되었습니다.

## <a name="shipping-your-integration"></a>통합 배송

통합을 제공 하려면 NuGet 패키지에 추가 해야 합니다.
기존 라이브러리의 NuGet과 함께 제공 하거나 새 패키지를 만드는 경우이 nuspec 파일을 시작 점으로 사용할 수 있습니다.
통합과 관련 된 섹션을 작성 해야 합니다. 가장 중요 한 부분은 통합에 대 한 모든 파일이 패키지의 루트에 있는 `xamarin.interactive` 디렉터리에 있어야 한다는 것입니다. 이를 통해 기존 패키지를 사용 하는지 아니면 새 패키지를 만들지에 관계 없이 통합에 대 한 모든 관련 파일을 쉽게 찾을 수 있습니다.

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

Nuspec 파일을 만들었으면 다음과 같이 NuGet을 압축할 수 있습니다.

```csharp
nuget pack MyIntegration.nuspec
```

그런 다음 [NuGet][nugetorg]에 게시 합니다. 거기에 있으면 모든 통합 문서에서이를 참조 하 여 작동 하는 것을 볼 수 있습니다. 아래 스크린샷에서는이 문서에서 빌드하고 NuGet 패키지를 통합 문서에 설치한 샘플 통합을 패키지 했습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![통합이 포함 된 통합 문서](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![통합이 포함 된 통합 문서](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

통합을 초기화 하는 `#r` 지시문 또는 모든 것이 표시 되지 않습니다. 통합 문서는 백그라운드에서 모든 작업을 처리 합니다.

## <a name="next-steps"></a>다음 단계

SDK를 구성 하는 이동 방법에 대 한 자세한 내용 및 통합 문서 클라이언트에서 실행 되는 사용자 지정 JavaScript를 제공 하는 것과 같이 통합에서 수행할 수 있는 추가 작업에 대 한 [샘플 통합](~/tools/workbooks/samples/index.md) 은 다른 설명서를 확인 하세요.

[nugetorg]: https://nuget.org
[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
[oxyplot]: http://www.oxyplot.org/
