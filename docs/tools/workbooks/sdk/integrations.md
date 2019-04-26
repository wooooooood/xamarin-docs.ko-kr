---
title: 고급 통합 항목
description: 이 문서에서는 Xamarin Workbooks 통합 관련 된 고급 항목을 설명 합니다. Xamarin 통합 문서 내에서 API 노출 되 고 Xamarin.Workbook.Integrations NuGet 패키지에 설명 합니다.
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 56ee709b78b8587c2717dc9d25a6357041812d23
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382247"
---
# <a name="advanced-integration-topics"></a>고급 통합 항목

통합 어셈블리 참조 해야 합니다 [ `Xamarin.Workbooks.Integrations` NuGet][nuget]합니다. 체크 아웃 우리의 [빠른 시작 설명서](~/tools/workbooks/sdk/index.md) NuGet 패키지를 사용 하 여 시작 하는 방법에 대 한 자세한 내용은 합니다.

클라이언트 통합도 지원 됩니다 및 에이전트 통합 어셈블리와 동일한 이름의 JavaScript 또는 CSS 파일이 동일한 디렉터리에 배치 하 여 시작 됩니다. 예를 들어 (NuGet 참조)이 표시 하는 에이전트 통합 어셈블리 이름은 `SampleExternalIntegration.dll`, 한 다음 `SampleExternalIntegration.js` 및 `SampleExternalIntegration.css` 있을 경우 클라이언트 에서도에 통합 될 예정입니다. 클라이언트 통합은 선택적입니다.

자체 외부 통합 NuGet으로 패키지, 제공 되 고 수와 함께 배치 하거나 에이전트를 호스트 하는 응용 프로그램 내에서 직접 참조는 `.workbook` 사용 하고자 하는 파일입니다.

NuGet 패키지에서 외부 통합 (에이전트 및 클라이언트) 패키지를 참조할 때 빠른 시작 설명서에 따라 통합 어셈블리 통합 문서와 함께 제공 되므로으로 참조 해야 하는 동안 자동으로 로드 됩니다.

```csharp
#r "SampleExternalIntegration.dll"
```

통합을 이러한 방식으로 참조할 때이 로드 되지 것입니다 클라이언트에서 바로&mdash;로드 하 게 통합에서 일부 코드를 호출 해야 합니다. 에서는 됩니다 수 주소 지정이 버그 나중에 있습니다.

`Xamarin.Interactive` PCL 몇 가지 중요 한 통합 Api를 제공 합니다. 모든 통합 통합 진입점을 제공 이상 해야 합니다.

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

이 시점에서 통합 어셈블리 참조 되 면 클라이언트 JavaScript 및 CSS 통합 파일을 암시적으로 로드 됩니다.

## <a name="apis"></a>API

라이브 또는 통합 문서에서 참조 하는 모든 어셈블리를 사용 하 여 세션을 검사 하는 대로 해당 공용 Api의 모든 세션에 액세스할 수 있습니다. 따라서 것이 안전 하 고 적절 한 API 화면을 탐색 하는 사용자에 대 한 것이 중요 합니다.

통합 어셈블리는 효과적으로 응용 프로그램 또는 관심 SDK와 세션 간의 연결. 것이 좋습니다는 통합 문서 또는 라이브 맥락에서 세션을 살펴보고 없는 공용 Api를 제공 하거나 단순히 개체를 생성 하는 등의 "내부적인" 작업을 수행 하는 새 Api를 제공할 수 있습니다 [표현을](~/tools/workbooks/sdk/representations.md)합니다.

> [!NOTE]
> 일반적인 public 이어야 하지만 IntelliSense를 통해 표시 되는 Api를 표시할 수 있습니다 `[EditorBrowsable (EditorBrowsableState.Never)]` 특성입니다.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
