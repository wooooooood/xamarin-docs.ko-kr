---
title: 고급 Integration 항목
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 8ab0bb71d4c79895eca94899c3f277b466fe0eb1
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2018
---
# <a name="external-integrations"></a>외부 통합

통합 어셈블리를 참조 해야는 [ `Xamarin.Workbooks.Integrations` NuGet][nuget]합니다. 체크 아웃 우리의 [빠른 시작 설명서](~/tools/workbooks/sdk/index.md) NuGet 패키지와 함께 시작 하는 방법에 대 한 자세한 내용은 합니다.

클라이언트 통합 에서도 지원 및 JavaScript 또는 CSS 파일 에이전트 통합 어셈블리와 동일한 이름 가진 같은 디렉터리에 배치 하 여 시작 됩니다. 예를 들어, (참조 하는 NuGet) 에이전트 통합 어셈블리 이름을 지정 하는 경우 `SampleExternalIntegration.dll`, 다음 `SampleExternalIntegration.js` 및 `SampleExternalIntegration.css` 있을 경우에 클라이언트에 통합 될 것입니다. 클라이언트 통합은 선택적입니다.

외부 통합 자체 nuget 패키지로, 제공 및 에이전트를 호스팅하는 또는 단순히와 함께 배치 하는 응용 프로그램 내부에서 직접 참조할 수는 `.workbook` 하지 않고자 한다면 클래스를 사용 하는 파일입니다.

NuGet 패키지의 외부 통합 (에이전트 및 클라이언트) 패키지를 참조할 때 빠른 시작 설명서에 따라 통합 문서와 함께 제공 되는 통합 어셈블리 같이 참조 해야 합니다는 동안 자동으로 로드 됩니다.

```csharp
#r "SampleExternalIntegration.dll"
```

통합을 이러한 방식으로 참조, 하는 경우이 로드 되지 것입니다 클라이언트에서 바로&mdash;을 로드 하도록 통합에서 일부 코드를 호출 해야 합니다. म 합니다 해결이 버그 나중에 있습니다.

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

이 시점에서 해당 통합 어셈블리가 참조 되는지 되 면 클라이언트 JavaScript 및 CSS 통합 파일을 암시적으로 로드 합니다.

## <a name="apis"></a>API

실시간 또는 통합 문서에서 참조 하는 모든 어셈블리와 세션 조사 하는 대로 해당 공용 Api의 모든 세션에 액세스할 수 있는 됩니다. 따라서이 사용자가 탐색할 수에 대 한 안전 하 고 붙여 구분이 가능한 API 화면을 중요 합니다.

통합 어셈블리는 응용 프로그램 또는 관심 있는 SDK와 세션 간의 다리 사실상입니다. 세션을를 검사 하거나 없는 공용 Api를 제공 하 고 간단 하 게 개체를 생성 하는 것과 같은 "백그라운드 에서" 작업을 수행할 컨텍스트는 통합 문서 또는 라이브에 구체적으로 의미 있는 새로운 Api를 제공할 수 있다는 [표현을](~/tools/workbooks/sdk/representations.md)합니다.

> [!NOTE]
> 일반적인으로 public 이어야 하지만 IntelliSense를 통해 표시 되는 Api를 표시할 수 있으며 `[EditorBrowsable (EditorBrowsableState.Never)]` 특성입니다.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
