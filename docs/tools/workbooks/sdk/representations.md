---
title: Xamarin 통합 문서에 표현
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: a5593ac902bfd2478cd8587aeef7e4a3926627dd
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2018
---
# <a name="representations-in-xamarin-workbooks"></a>Xamarin 통합 문서에 표현

## <a name="representations"></a>표현

통합 문서 또는 검사기 세션 내에서 실행 되 고 결과 (예: 값 또는 식의 결과 반환 하는 메서드)를 생성 하는 코드는 에이전트에서 표현 파이프라인을 통해 처리 됩니다. 정수와 같은 기본 형식 제외 하 고 모든 개체를 멤버 대화형 그래프를 생성 하기 위해 반영와 클라이언트 더 다양 하 게 렌더링할 수 있는 대체 표현을 제공 하는 프로세스를 통해 합니다. 개체 크기 및 깊이의 지연 및 대화형 리플렉션 및 원격 작업으로 인해 주기 및 무한 열거 가능 형식) (포함 안전 하 게 지원 됩니다.

Xamarin 통합 문서는 결과의 풍부한 렌더링을 위해 사용할 수 있는 클라이언트 및 모든 에이전트에 일반적인 몇 가지 종류를 제공 합니다. [`Color`][xir-color] 여기서 예: iOS에서 에이전트는 변환에 대 한 이러한 형식의 한 가지 예는 `CGColor` 또는 `UIColor` 로 개체는 `Xamarin.Interactive.Representations.Color` 개체입니다.

공통 된 표현을 뿐만 아니라 SDK를 통합 하는 에이전트에서 사용자 지정 표현을 직렬화 표현에 클라이언트를 렌더링 하기 위한 Api를 제공 합니다.

## <a name="external-representations"></a>외부 표현

[`Xamarin.Interactive.IAgent.RepresentationManager`][repman] 등록 하는 기능을 제공 된 [ `RepresentationProvider` ] [ repp], 임의 개체에서 렌더링할를 알 수 없는 형식으로 변환 하는 통합 구현 해야 합니다. 이러한 알 수 없는 형태를 구현 해야 합니다는 [ `ISerializableObject` ] [ serobj] 인터페이스입니다.

구현 된 `ISerializableObject` 인터페이스 정확 하 게 직렬화 개체를 제어 하는 Serialize 메서드에 추가 합니다. `Serialize` 메서드에서 예상 되는 속성은 serialize 할 및 됩니다는 최종 이름은 개발자 지정 정확 하 게 됩니다. 보면는 `Person` 개체에서 우리의 [`KitchenSink` 샘플] [샘플]에이 수식이 어떻게 확인할 수 있습니다.

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

원래 개체에서 속성의 하위 집합 또는 상위 집합 제공 주고자 म 않으며 수와 `Serialize`합니다. 예를 들어에서는 미리 계산 된 제공 하려면 다음과 같이 할 수 `Age` 속성 `Person`:

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
> 생성 하는 Api `ISerializableObject` 개체 직접에서 처리할 필요 하지 않습니다는 `RepresentationProvider`합니다. 표시 하려는 개체가 **하지** 는 `ISerializableObject`에서 래핑하여 처리 하려는 프로그램 `RepresentationProvider`합니다.

### <a name="rendering-a-representation"></a>렌더링 한 표현

렌더러는 JavaScript에서 구현 하 고를 통해 표시 되는 개체의 JavaScript 버전에 액세스할 수 있는 `ISerializableObject`합니다. JavaScript 복사본에는 한 `$type` .NET 형식 이름을 나타내는 문자열 속성입니다.

물론 JavaScript 바닐라 컴파일함으로써 클라이언트 통합 코드에 대 한 TypeScript를 사용 하는 것이 좋습니다. 어떤 방법을 사용 하는 SDK 제공 [입력 항목] [ typings] TypeScript에서 직접 참조 하거나 간단히 참조를 수동으로 작성 바닐라 JavaScript는 것이 좋습니다 경우 수입니다.

렌더링 하기 위한 주요 통합 지점은 `xamarin.interactive.RendererRegistry`:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

여기에서 `PersonRenderer` 구현 하는 `Renderer` 인터페이스입니다. 참조는 [입력 항목] [ typings] 내용을 확인 합니다.

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
[xir-color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[repman]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[repp]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[serobj]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Serialization.ISerializableObject/
