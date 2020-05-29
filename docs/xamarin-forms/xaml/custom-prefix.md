---
title: XAML 네임 스페이스 권장 접두사Xamarin.Forms
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 71ae523f40f3f7529c12f853778404e224fbae30
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138140"
---
# <a name="xaml-namespace-recommended-prefixes-in-xamarinforms"></a>XAML 네임 스페이스 권장 접두사Xamarin.Forms

`XmlnsPrefixAttribute`클래스를 사용 하 여 xaml 사용을 위해 xaml 네임 스페이스와 연결할 권장 접두사를 지정할 수 있습니다. 접두사는 xaml로의 개체 트리 serialization을 지원 하거나 XAML 편집 기능이 있는 디자인 환경과 상호 작용할 때 유용 합니다. 예를 들면 다음과 같습니다.

- XAML 텍스트 편집기는를 `XmlnsPrefixAttribute` 초기 xaml 네임 스페이스 매핑에 대 한 힌트로 사용할 수 있습니다 `xmlns` .
- XAML 디자인 환경에서는를 사용 `XmlnsPrefixAttribute` 하 여 개체를 도구 상자에서 시각적 디자인 화면으로 끌어 올 때 xaml에 매핑을 추가할 수 있습니다.

권장 되는 네임 스페이스 접두사는 두 개의 인수를 사용 하는 생성자를 사용 하 여 어셈블리 수준에서 정의 해야 합니다 `XmlnsPrefixAttribute` . 여기서는 XAML 네임 스페이스의 식별자를 지정 하는 문자열과 권장 접두사를 지정 하는 문자열입니다.

```csharp
[assembly: XmlnsPrefix("http://xamarin.com/schemas/2014/forms", "xf")]
```

접두사는 일반적으로 XAML 네임 스페이스에서 제공 되는 serialize 된 모든 요소에 적용 되므로 접두사는 짧은 문자열을 사용 해야 합니다. 따라서 접두사 문자열 길이는 serialize 된 XAML 출력의 크기에 크게 영향을 미칠 수 있습니다.

> [!NOTE]
> 하나 이상의 `XmlnsPrefixAttribute` 어셈블리에 적용할 수 있습니다. 예를 들어 둘 이상의 XAML 네임 스페이스에 대 한 형식을 정의 하는 어셈블리가 있는 경우 각 XAML 네임 스페이스에 대해 서로 다른 접두사 값을 정의할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [XAML 사용자 지정 네임스페이스 스키마](custom-namespace-schemas.md)
- [XAML 네임 스페이스Xamarin.Forms](namespaces.md)
