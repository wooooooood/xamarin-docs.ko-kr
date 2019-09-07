---
title: 목표 Sharpie 확인 특성
description: 이 문서에서는 Sharpie에서 생성 된 [Verify] 특성에 대해 설명 합니다. [Verify] 특성은 목표 Sharpie의 출력을 수동으로 확인 해야 하는 개발자에 게 표시 됩니다.
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: conceptdev
ms.author: crdun
ms.date: 01/15/2016
ms.openlocfilehash: eb4a992255fa26be1e83f27f93755e0fd8bda0a4
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765683"
---
# <a name="objective-sharpie-verify-attributes"></a>목표 Sharpie 확인 특성

일반적으로 목표 Sharpie에서 생성 된 바인딩에는 `[Verify]` 특성으로 주석이 추가 됩니다. 이러한 특성은 바인딩을 원래 C/목표값-C 선언과 비교 하 여 (바인딩된 선언 위의 설명에 제공 됨) 목표 Sharpie이 올바른 것을 _확인_ 해야 함을 의미 합니다.

_모든_ 바인딩된 선언에는 확인을 사용 하는 것이 좋지만 `[Verify]` 특성으로 주석이 달린 선언에는이를 _반드시 사용 해야_ 합니다. 이는 대부분의 경우 원래 네이티브 소스 코드에 메타 데이터가 부족 하 여 바인딩을 가장 잘 만드는 방법을 유추할 수 없기 때문입니다. 최상의 바인딩 결정을 위해서는 헤더 파일 내에서 설명서 또는 코드 주석을 참조 해야 할 수 있습니다.

바인딩이 올바른지 확인 하거나 올바른 것으로 고정 한 후에는 바인딩에서 `[Verify]` 특성을 _제거_ 합니다.

> [!IMPORTANT]
> `[Verify]`특성은 의도적 C# 으로 바인딩을 확인 하도록 강제 컴파일 오류를 발생 시킵니다. 코드를 검토 하 `[Verify]` 고 수정 하는 경우에는이 특성을 제거 해야 합니다.

## <a name="verify-hints-reference"></a>힌트 참조 확인

특성에 제공 된 힌트 인수는 아래 설명서와 상호 참조할 수 있습니다. 생성 `[Verify]` 된 모든 특성에 대 한 설명서는 바인딩이 완료 된 후에도 콘솔에서 제공 됩니다.

|`[Verify]`힌트|Description|
|---|---|
|InferredFromPreceedingTypedef|이 선언의 이름은 원래 네이티브 소스 코드의 즉시 앞 `typedef` 일반 규칙에 따라 유추 됩니다. 이 규칙이 모호 하므로 유추 된 이름이 올바른지 확인 합니다.|
|ConstantsInterfaceAssociation|Extern 변수 선언이 연결 될 수 있는 목표-C 인터페이스를 결정 하는 바보 증명 방법은 없습니다. 이러한 인스턴스는 partial 인터페이스의 `[Field]` 속성으로, 보다 직관적인 API를 생성 하는 근접 한 인터페이스에서 근접 한 인터페이스에 바인딩되어 있으므로 ' 상수 ' 인터페이스를 완전히 제거 합니다.|
|MethodToProperty|목적-C 메서드는 매개 변수를 취하지 C# 않고 값 (void가 아닌 반환)을 반환 하는 등의 규칙으로 인해 속성으로 바인딩 되었습니다. 이러한 메서드는 더 좋은 API를 노출 하기 위한 속성으로 바인딩되어야 하지만 때로는 가양성이 발생 하 고 바인딩이 실제로 메서드 일 수 있습니다.|
|StronglyTypedNSArray|네이티브 `NSArray*` 가로 `NSObject[]`바인딩 되었습니다. API 설명서 (예: 헤더 파일의 주석)를 통해 설정 된 기대 수준에 따라 또는 테스트를 통해 배열 콘텐츠를 검사 하는 방법에 따라 바인딩에서 배열을 더 강력 하 게 입력할 수 있습니다. 예를 들어는 NSArray* NSNumber* 만 포함 된 instancescan 변수로 바인딩되어야 `NSNumber[]` 대신 `NSObject[]`합니다.|

`sharpie verify-docs` 도구를 사용 하 여 힌트에 대 한 설명서를 신속 하 게 받을 수도 있습니다. 예를 들면 다음과 같습니다.

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```
