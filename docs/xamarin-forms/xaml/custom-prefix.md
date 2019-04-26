---
title: XAML Namespace Xamarin.Forms의 접두사를 권장 합니다.
description: XmlnsPrefixAttribute 클래스 XAML 사용에 대 한 XAML 네임 스페이스와 연결할 권장된 접두사를 지정 하려면 컨트롤 작성자가 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 7B315BEC-7A35-48F4-A9C7-EF40255E95FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2019
ms.openlocfilehash: 33f18b3f9c9ddb6ab31ca92e2f192ffad783ec0c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075104"
---
# <a name="xaml-namespace-recommended-prefixes-in-xamarinforms"></a>XAML Namespace Xamarin.Forms의 접두사를 권장 합니다.

`XmlnsPrefixAttribute` XAML 사용에 대 한 XAML 네임 스페이스와 연결할 권장된 접두사를 지정 하려면 컨트롤 작성자 클래스를 사용할 수 있습니다. 접두사는 XAML 개체 트리 serialization을 지원할 때 유용한 않았거나 디자인 환경을 조작할 때 XAML 편집 기능. 예를 들어:

- XAML 텍스트 편집기를 사용할 수는 `XmlnsPrefixAttribute` 초기 XAML 네임 스페이스의 힌트로 `xmlns` 매핑.
- XAML 디자인 환경에서 사용할 수는 `XmlnsPrefixAttribute` 개체에서 도구 상자 및 시각적 디자인 화면으로 끌어 올 때의 XAML에 대 한 매핑을 추가 합니다.

네임 스페이스 접두사를 사용 하 여 어셈블리 수준에서 정의 해야 하는 것이 좋습니다는 `XmlnsPrefixAttribute` 두 개의 인수를 사용 하는 생성자: XAML 네임 스페이스의 식별자를 지정 하는 문자열 및 권장 되는 접두사를 지정 하는 문자열:

```csharp
[assembly: XmlnsPrefix("http://xamarin.com/schemas/2014/forms", "xf")]
```

접두사는 접두사는 XAML 네임 스페이스에서 제공 하는 모든 직렬화 된 요소에 일반적으로 적용 되기 때문에 짧은 문자열을 사용 해야 합니다. 따라서 접두사 문자열 길이 serialize 된 XAML 출력의 크기에 눈에 띄는 영향을 미칠 수 있습니다.

> [!NOTE]
> 둘 이상의 `XmlnsPrefixAttribute` 어셈블리에 적용할 수 있습니다. 예를 들어 둘 이상의 XAML 네임 스페이스에 대 한 형식을 정의 하는 어셈블리에 있는 경우 각 XAML 네임 스페이스에 대 한 서로 다른 접두사 값을 정의할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [XAML 사용자 지정 네임스페이스 스키마](custom-namespace-schemas.md)
- [Xamarin.Forms의 XAML 네임 스페이스](namespaces.md)
