---
title: 고급 통합 항목
description: 이 문서에서는 Xamarin Workbooks 통합과 관련 된 고급 항목을 설명 합니다. Xamarin 통합 문서 내에서 Xamarin.ios 패키지 및 API 노출에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 07dd0e64b90bb0aa11f0a7050e3b86f3203ce7de
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199983"
---
# <a name="advanced-integration-topics"></a>고급 통합 항목

통합 어셈블리는 [ `Xamarin.Workbooks.Integrations` NuGet][nuget]을 참조 해야 합니다. NuGet 패키지를 시작 하는 방법에 대 한 자세한 내용은 [빠른 시작 설명서](~/tools/workbooks/sdk/index.md) 를 참조 하세요.

클라이언트 통합도 지원 되며 동일한 디렉터리에 에이전트 통합 어셈블리와 동일한 이름의 JavaScript 또는 CSS 파일을 배치 하 여 시작 됩니다. 예를 들어 NuGet을 참조 하는 에이전트 통합 어셈블리의 이름이 `SampleExternalIntegration.dll` `SampleExternalIntegration.js` 인 경우 및 `SampleExternalIntegration.css` 는 클라이언트에도 통합 됩니다 (있는 경우). 클라이언트 통합은 선택 사항입니다.

외부 통합 자체는 에이전트를 호스트 하는 응용 프로그램 내에서 직접 제공 및 참조 하거나 사용 하려는 파일에 `.workbook` 배치 하는 것과 같이 NuGet로 패키지할 수 있습니다.

NuGet 패키지의 외부 통합 (에이전트 및 클라이언트)은 패키지가 참조 될 때 자동으로 로드 되며, 빠른 시작 설명서에 따라 통합 문서와 함께 제공 되는 통합 어셈블리는이를 참조 해야 합니다.

```csharp
#r "SampleExternalIntegration.dll"
```

이러한 방식으로 통합을 참조 하는 경우에는 클라이언트&mdash;에서 해당 통합을 로드 하지 않으며, 로드 하려면 통합에서 일부 코드를 호출 해야 합니다. 이 버그는 나중에 해결할 예정입니다.

PCL `Xamarin.Interactive` 은 몇 가지 중요 한 통합 api를 제공 합니다. 모든 통합은 최소한 통합 진입점을 제공 해야 합니다.

```csharp
using Xamarin.Interactive;

[assembly: AgentIntegration (typeof (AgentIntegration))]

class AgentIntegration : IAgentIntegration
{
    const string TAG = nameof (AgentIntegration);

    public void IntegrateWith (IAgent agent)
    {
        // hook into IAgent APIs
    }
}
```

이 시점에서 통합 어셈블리를 참조 하면 클라이언트에서 JavaScript 및 CSS 통합 파일이 암시적으로 로드 됩니다.

## <a name="apis"></a>API

통합 문서 또는 라이브 검사 세션에서 참조 하는 모든 어셈블리와 마찬가지로 해당 공용 Api는 세션에서 액세스할 수 있습니다. 따라서 사용자가 탐색할 수 있는 안전 하 고 합리적인 API 화면을 포함 하는 것이 중요 합니다.

통합 어셈블리는 실제로 응용 프로그램 또는 대상 SDK와 세션 간의 브리지입니다. 이 Api는 통합 문서 또는 라이브 검사 세션의 컨텍스트에서 명확 하 게 사용할 수 있는 새 Api를 제공 하거나, 공용 Api를 제공 하지 않고 개체 [표현](~/tools/workbooks/sdk/representations.md)생성 같은 "백그라운드에서" 작업을 수행 합니다.

> [!NOTE]
> 공용 이어야 하지만 IntelliSense를 통해 노출 되지 않아야 하는 api는 일반적인 `[EditorBrowsable (EditorBrowsableState.Never)]` 특성으로 표시할 수 있습니다.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
