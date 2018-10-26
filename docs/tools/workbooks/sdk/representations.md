---
title: Xamarin Workbooks 표시
description: 이 문서는 다양 한 결과 값을 반환 하는 코드에 대 한 렌더링을 사용 하도록 설정 하는 Xamarin Workbooks 표현을 파이프라인을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: d9aafbe13e06875b6577a4d2308e419932fd1589
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103714"
---
# <a name="representations-in-xamarin-workbooks"></a>Xamarin Workbooks 표시

## <a name="representations"></a>표현

통합 문서 또는 검사기 세션 내에서 실행 되 고 결과 (예: 값 또는 식의 결과 반환 하는 메서드)를 생성 하는 코드는 에이전트에서 표현 파이프라인을 통해 처리 됩니다. 정수와 같은 기본 형식 제외 하 고 모든 개체 멤버 대화형 그래프를 생성 하기 위해 반영 됩니다 및 클라이언트는 더 풍부 하 게 렌더링할 수 있는 대체 표현을 제공 하는 프로세스를 거치게 됩니다. 모든 크기, 깊이의 개체는 안전 하 게 지연 및 대화형 리플렉션 및 원격으로 인해 주기 및 무한 열거 가능 형식) (포함 지원 됩니다.

Xamarin Workbooks 모든 에이전트 및 결과의 다양 한 렌더링을 사용할 수 있도록 클라이언트에 일반적인 몇 가지 형식을 제공 합니다. [`Color`][xir-color] 이러한 종류의 예로 예를 들어 iOS에서, 에이전트로 변환할 책임이 `CGColor` 나 `UIColor` 개체를 `Xamarin.Interactive.Representations.Color` 개체입니다.

SDK 통합을 공통 된 표현을 외에도 에이전트에서 사용자 지정 표현 직렬화 및 클라이언트의 표현을 렌더링에 대 한 Api를 제공 합니다.

## <a name="external-representations"></a>외부 표현

[`Xamarin.Interactive.IAgent.RepresentationManager`][repman] 등록 하는 기능을 제공 된 [ `RepresentationProvider` ] [ repp], 임의 개체에서 렌더링 하는 알 수 없는 형식으로 변환 하는 통합 구현 해야 하는 합니다. 알 수 없는 이러한 형태를 구현 해야 합니다 [ `ISerializableObject` ] [ serobj] 인터페이스입니다.

구현 된 `ISerializableObject` 인터페이스 개체 직렬화 되는 방식을 정확 하 게 제어 하는 직렬화 메서드를 추가 합니다. `Serialize` 메서드에서 예상 개발자 속성을 직렬화 되며 됩니다 최종 이름 지정 정확 하 게 됩니다. 살펴보면를 `Person` 개체는 [`KitchenSink` 샘플] [샘플], 작동 방식을 확인할 수 있습니다.

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

사용 하 여 수행 수 있고 원래 개체에서 속성의 하위 집합일 수 있도록 싶으면 `Serialize`합니다. 예를 들어에서는 미리 계산 된 제공 하려면 다음과 같이 수행할 수 있습니다 `Age` 속성을 `Person`:

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
> 생성 하는 Api `ISerializableObject` 개체 직접 않아도 처리할는 `RepresentationProvider`합니다. 표시 하려는 개체가 **되지** 는 `ISerializableObject`에서 래핑하여 처리 하려는 프로그램 `RepresentationProvider`합니다.

### <a name="rendering-a-representation"></a>표시를 렌더링합니다.

렌더러는 JavaScript에서 구현 되며 JavaScript 버전을 통해 표시 되는 개체에 액세스할 수 `ISerializableObject`입니다. JavaScript 복사본을 갖게 됩니다는 `$type` .NET 형식 이름을 나타내는 문자열 속성입니다.

물론 바닐라 JavaScript로 컴파일되는 클라이언트 통합 코드에 대 한 TypeScript를 사용 하는 것이 좋습니다. 어느 방법이 든 SDK 제공 [typings] [ typings] TypeScript에서 직접 참조 하거나 단순히 참조를 수동으로 작성 바닐라 JavaScript는 기본 설정 된 경우 수입니다.

렌더링에 대 한 기본 통합 지점은 `xamarin.interactive.RendererRegistry`:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

이때 `PersonRenderer` 구현 된 `Renderer` 인터페이스입니다. 참조 된 [typings] [ typings] 대 한 자세한 내용은 합니다.

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
[xir-color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[repman]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[repp]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[serobj]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Serialization.ISerializableObject/
