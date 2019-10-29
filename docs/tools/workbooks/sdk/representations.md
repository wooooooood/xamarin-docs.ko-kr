---
title: Xamarin Workbooks에 표시
description: 이 문서에서는 값을 반환 하는 모든 코드에 대해 다양 한 결과를 렌더링 하는 데 사용할 수 있는 Xamarin Workbooks 표현 파이프라인을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: c1afba6943faa03ee07a3ec70624f668748041cb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018589"
---
# <a name="representations-in-xamarin-workbooks"></a>Xamarin Workbooks에 표시

## <a name="representations"></a>형태로

통합 문서 또는 검사기 세션 내에서 실행 되 고 결과를 생성 하는 코드 (예: 값 또는 식의 결과를 반환 하는 메서드)는 에이전트의 표현 파이프라인을 통해 처리 됩니다. 정수와 같은 기본 형식을 제외 하 고 모든 개체는 대화형 멤버 그래프를 생성 하는 데 반영 되며, 클라이언트에서 더 많은 기능을 렌더링할 수 있는 대체 표현을 제공 하기 위해 프로세스를 진행 합니다. 지연 및 대화형 리플렉션 및 원격 기능으로 인해 모든 크기와 깊이의 개체가 안전 하 게 지원 됩니다 (주기 및 무한 열거형 포함).

Xamarin Workbooks는 모든 에이전트와 클라이언트에 공통적인 몇 가지 형식을 제공 하 여 다양 한 결과 렌더링을 가능 하 게 합니다. `Color`은 이러한 유형의 한 예입니다. 예를 들어 iOS의 경우 에이전트는 `CGColor` 또는 `UIColor` 개체를 `Xamarin.Interactive.Representations.Color` 개체로 변환 해야 합니다.

통합 SDK는 일반적인 표현 외에도 에이전트의 사용자 지정 표현과 클라이언트의 렌더링 표현을 직렬화 하는 Api를 제공 합니다.

## <a name="external-representations"></a>외부 표현

`Xamarin.Interactive.IAgent.RepresentationManager`은 `RepresentationProvider`를 등록 하는 기능을 제공 합니다 .이는 임의의 개체에서 렌더링을 위한 독립적인 형식으로 변환 하기 위해 통합에서 구현 해야 하는 기능입니다. 이러한 독립적인 형식은 `ISerializableObject` 인터페이스를 구현 해야 합니다.

`ISerializableObject` 인터페이스를 구현 하면 개체가 serialize 되는 방법을 정확 하 게 제어 하는 Serialize 메서드가 추가 됩니다. `Serialize` 메서드는 개발자가 직렬화 할 속성 및 최종 이름을 정확 하 게 지정 하는 것으로 예상 합니다. [`KitchenSink` 샘플] [샘플]에서 `Person` 개체를 살펴보면 어떻게 작동 하는지 확인할 수 있습니다.

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

원본 개체에서 속성의 상위 집합 또는 하위 집합을 제공 하려는 경우 `Serialize`를 사용 하 여이 작업을 수행할 수 있습니다. 예를 들어 다음과 같은 작업을 수행 하 여 `Person`에 미리 계산 된 `Age` 속성을 제공할 수 있습니다.

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; set; }
  public DateTime DateOfBirth { get; set; }

  // <snip>

  void ISerializableObject.Serialize (ObjectSerializer serializer)
  {
    serializer.Property (nameof (Name), Name);
    serializer.Property (nameof (DateOfBirth), DateOfBirth);

    // Let's pre-compute an Age property that's the person's age in years,
    // so we don't have to compute it in the renderer.
    var age = (DateTime.MinValue + (DateTime.Now - DateOfBirth)).Year - 1;
    serializer.Property ("Age", age)
  }
}
```

> [!NOTE]
> `ISerializableObject` 개체를 직접 생성 하는 Api는 `RepresentationProvider`에서 처리할 필요가 없습니다. 표시 하려는 개체가 `ISerializableObject`**아닌** 경우 `RepresentationProvider`에서 줄 바꿈을 처리 하는 것이 좋습니다.

### <a name="rendering-a-representation"></a>표현 렌더링

렌더러는 JavaScript에서 구현 되며 `ISerializableObject`를 통해 표현 되는 개체의 JavaScript 버전에 액세스할 수 있습니다. JavaScript 복사본에는 .NET 형식 이름을 나타내는 `$type` 문자열 속성도 있습니다.

물론 바닐라 JavaScript로 컴파일되는 클라이언트 통합 코드에는 TypeScript를 사용 하는 것이 좋습니다. 어느 쪽이 든 SDK는 TypeScript에서 직접 참조 하거나 바닐라 JavaScript 쓰기가 선호 되는 경우 수동으로 참조 될 수 있는 입력 항목 [을 제공 합니다][typings] .

렌더링의 기본 통합 지점은 `xamarin.interactive.RendererRegistry`입니다.

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

여기서 `PersonRenderer`는 `Renderer` 인터페이스를 구현 합니다. 자세한 내용은 [해당 항목을 참조][typings] 하세요.

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
