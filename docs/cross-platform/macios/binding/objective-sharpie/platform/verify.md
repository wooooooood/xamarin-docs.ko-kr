---
title: "특성 확인"
ms.topic: article
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: cda523cd9d762c3a3c1570e2abd0acb8a264d5dd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="verify-attributes"></a>특성 확인


목표 Sharpie에서 생성 된 바인딩으로 주석을 달아야 합니다 하기가 `[Verify]` 특성입니다. 이러한 특성 표시 해야 하는 _확인_ 목표 Sharpie 원래 C/Objective-c 선언 (바인딩된 선언 위에 주석에서 제공 됩니다)를 사용 하 여 바인딩을 비교 하 여 올바른 동작인을가 하 합니다.

확인에 대 한 것이 좋습니다 _모든_ 선언 바인딩되지만 주로 _필요한_ 선언을 주석으로 처리에 대 한는 `[Verify]` 특성입니다. 즉, 대부분의 경우에 충분 한 메타 데이터에에서 없으면 가장 적합 한 바인딩을 생성 하는 방법을 유추 하 고 원래 네이티브 소스 코드. 문서 또는 최상의 바인딩 결정을 내리려면 헤더 파일 내의 코드 주석을 참조 해야 합니다.

바인딩이 확인 되 면 수정 하거나, 올바른 되도록 수정 _제거_ 는 `[Verify]` 바인딩에서 특성입니다.

> [!IMPORTANT]
> `[Verify]` 특성 하 여 바인딩 확인 해야 하는 C# 컴파일 오류가 의도적으로 발생 합니다. 제거 해야는 `[Verify]` 하는 경우 검토 (수정 가능) 코드 특성입니다.

## <a name="verify-hints-reference"></a>힌트 참조 확인

아래 문서를 사용 하 여 참조 특성에 제공 된 힌트 인수가 교차 수 있습니다. 생성 된 모든 작업에 대 한 설명서 `[Verify]` 특성 바인딩을 완료 된 후 콘솔도 제공 됩니다.

<table>
  <thead>
  <tr>
    <th>힌트를 확인 합니다.</th>
    <th>설명</th>
  </tr>
  </thead>
  <tbody>
  <tr>
    <td>InferredFromPreceedingTypedef</td>
    <td>이 선언의 이름에서 일반적인 규칙에 의해 유추 된의 바로 앞에 오는 <code>typedef</code> 원래 네이티브 소스 코드에 있습니다. 유추 된 이름이이 규칙은 모호 합니다. 정확한 지 확인 합니다.</td>
  </tr>
  <tr>
    <td>ConstantsInterfaceAssociation</td>
    <td>Objective C 인터페이스와 extern 변수 선언은 연결 될 수 있습니다를 결정 하는 절대 확실 한 것 방식은 없습니다. 이러한 인스턴스 바인딩된 <code>[Field]</code> 근처에서 구체적인 인터페이스에 가능한 '상수'를 제거 하는 보다 직관적인 API를 생성 하기 위해 부분 인터페이스의 속성에에서는 모두 인터페이스입니다.</td>
  </tr>
  <tr>
    <td>MethodToProperty</td>
    <td>Objective C 메서드 매개 변수 없이 차지 하 고 (void가 아닌 반환) 값 반환 등 규칙으로 인해 C# 속성으로 바인딩 되었습니다. 이와 같은 종종 메서드를 바인딩하는 속성으로는 더 좋은 API를 표시 하 거짓 긍정을 수행 하기도 있고 바인딩 메서드라고 실제로 해야 합니다.</td>
  </tr>
  <tr>
    <td>StronglyTypedNSArray</td>
    <td>네이티브 <code>NSArray*</code> 으로 바인딩 <code>NSObject[]</code>합니다. 더 강력 하 게 형식화 배열에서 기대 API 설명서 (예: 헤더 파일에 설명)을 통해 설정에 따라 바인딩을 수도 또는 테스트를 통해 배열 콘텐츠를 검사 하 여 합니다. 예를 들어 한 NSArray * NSNumber *만 포함 된 instancescan으로 바인딩할 수 <code>NSNumber[]</code> 대신 <code>NSObject[]</code>합니다.</td>
  </tr>
  </tbody>
</table>

또한 신속 하 게 사용 하 여 힌트에 대 한 설명서를 받을 수 있습니다는 `sharpie verify-docs` 예를 들어 도구:

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

