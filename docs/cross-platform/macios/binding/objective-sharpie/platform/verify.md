---
title: 목표 Sharpie 특성 확인
description: 이 문서에서는 목표 Sharpie에서 생성 된 [확인] 특성을 설명 합니다. [확인] 특성 목표 Sharpie 출력 확인 수동으로 해야 하는 개발자에 게 강조 표시 합니다.
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 4bca896afb4dfc96fd6c1d7cdf489feb6a879e31
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855031"
---
# <a name="objective-sharpie-verify-attributes"></a>목표 Sharpie 특성 확인

목표 Sharpie에서 생성 된 바인딩을 사용 하 여 주석을 달 수는 경우가 종종 있습니다를 `[Verify]` 특성입니다. 이러한 특성 해야 하는 나타냅니다 _확인_ (바인딩된 선언 위에 주석에서 제공 됩니다)는 원래 C/Objective-c 선언 사용 하 여 바인딩을 비교 하 여 바람직한 목표 Sharpie에가 하 합니다.

확인에 대 한 것이 좋습니다 _모든_ 선언으로 바인딩 되었지만 가능성이 _필요_ 선언을 주석으로 처리에 대 한는 `[Verify]` 특성입니다. 이것이 없기 때문에 대부분의 경우에서 충분 한 메타 데이터를 유추 가장 바인딩을 생성 하는 방법을 원래 기본 소스 코드에서입니다. 설명서 또는 최상의 바인딩 결정을 내리는 데 헤더 파일 내의 코드 주석을 참조 해야 합니다.

바인딩 인지 확인 한 후 수정 하거나, 올바른 수를 수정 _제거할_ 는 `[Verify]` 바인딩에서 특성입니다.

> [!IMPORTANT]
> `[Verify]` 특성은 바인딩을 확인 하도록 강제 됩니다 있도록 C# 컴파일 오류가 의도적으로 발생 합니다. 제거 해야 합니다 `[Verify]` 검토 (하 고 수정할 수 있는) 코드 특성입니다.

## <a name="verify-hints-reference"></a>힌트 참조를 확인 합니다.

아래 설명서를 사용 하 여 참조 특성에 제공 된 힌트 인수 간 수 있습니다. 생성 된 모든 설명서 `[Verify]` 특성 바인딩을 완료 된 후 콘솔도 제공 됩니다.

|`[Verify]` 힌트|설명|
|---|---|
|InferredFromPreceedingTypedef|일반적인 규칙에 따라이 선언의 이름을 유추 된는 바로 앞의 `typedef` 원래 기본 소스 코드에서입니다. 이 규칙은 모호한 올바른 유추 된 이름 인지 확인 합니다.|
|ConstantsInterfaceAssociation|바보 증명 확인할 방법은 없으며 Objective-c 인터페이스를 사용 하 여 extern 변수 선언은 연결할 수 있습니다. 이러한 인스턴스의 바인딩된 `[Field]` 속성 근처에서 구체적인 인터페이스에 가능한 경우 '상수'를 제거는 보다 직관적인 API를 생성 하기 위해 일부 인터페이스에서 완전히 인터페이스입니다.|
|MethodToProperty|Objective-c 메서드로 없는 매개 변수 및 반환 값 (void가 아닌 반환)와 같은 규칙으로 인해 C# 속성으로 바인딩 되었습니다. 이러한 방법을 자주 바인딩할 속성으로 편리한 API를 노출 하지만 가양성 발생 하는 경우도 있습니다 및 바인딩 메서드를 실제로 이어야 합니다.|
|StronglyTypedNSArray|네이티브 `NSArray*` 으로 바인딩 `NSObject[]`합니다. 예상 API 설명서 (예: 헤더 파일에 주석)를 통해 설정에 따라 바인딩에 배열을 더욱 강력 하 게 입력할 수 있습니다 또는 테스트를 통해 배열 내용을 검사 하 여 합니다. 예를 들어는 NSArray* NSNumber* 만 포함 된 instancescan 변수로 바인딩되어야 `NSNumber[]` 대신 `NSObject[]`합니다.|

사용 하 여 힌트에 대 한 설명서를 신속 하 게도 받을 수 있습니다는 `sharpie verify-docs` 도구, 예를 들어:

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

## <a name="related-links"></a>관련 링크

- [Objective-c 바인딩 라이브러리를 빌드할 Xamarin University 과정:](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University 과정: 목표 Sharpie 사용 하 여 Objective-c 바인딩 라이브러리를 빌드](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
