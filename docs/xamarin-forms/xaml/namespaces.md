---
title: Xamarin.Forms의 XAML 네임 스페이스
description: XAML 네임 스페이스 선언에 xmlns XML 특성을 사용합니다. 이 문서는 XAML 네임 스페이스 구문을 소개하고 XAML 네임 스페이스를 선언하여 유형에 접근하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/21/2018
ms.openlocfilehash: 85b9297a62cfb90485be2cbd927abfdcfec2f13c
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563045"
---
# <a name="xaml-namespaces-in-xamarinforms"></a>Xamarin.Forms의 XAML 네임 스페이스

_XAML 네임 스페이스 선언에 대 한 xmlns XML 특성을 사용합니다. 이 문서는 XAML 네임 스페이스 구문을 소개 하 고 형식에 액세스 하는 XAML 네임 스페이스를 선언 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

XAML 파일의 루트 요소 내에 항상 있는 두 개의 XAML 네임 스페이스 선언이 있습니다. 다음 XAML 코드 예제와 같이 첫 번째는 기본 네임 스페이스를 정의합니다.

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

기본 네임 스페이스는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)와 같이 접두사가 없는 XAML 파일 내에 정의된 요소가 Xamarin.Forms 클래스를 참조하도록 지정합니다.

두 번째 네임 스페이스 선언에서는 다음 XAML 코드 예제와 같이 `x` 접두사를 사용합니다.

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML은 접두사를 사용하여 기본이 아닌 네임 스페이스를 선언하고 네임 스페이스 내에서 유형을 참조할 때 접두사를 사용합니다. `x` 네임 스페이스 선언은 XAML에 정의된 요소인 `x`가 접두사로 사용되며 XAML(특히 2009 XAML 사양)에 고유한 요소 및 특성에 사용되도록 지정합니다.

다음 표는 Xamarin.Forms에서 지원하는 `x` 네임 스페이스 특성을 설명합니다.

|구문|설명|
|--- |--- |
|`x:Arguments`|기본이 아닌 생성자 또는 팩터리 메서드 개체 선언에 대한 생성자 인수를 지정합니다.|
|`x:Class`|XAML에 정의된 클래스의 네임 스페이스 및 클래스 이름을 지정합니다. 클래스 이름은 코드 숨김 파일의 클래스 이름과 일치해야 합니다. 해당 구문은 XAML 파일의 루트 요소에만 나타날 수 있다는 것을 참고하십시오.|
|`x:DataType`|XAML 요소 및 해당 자식 요소가 바인딩할 개체의 유형을 지정합니다.|
|`x:FactoryMethod`|개체를 초기화하는 데 사용할 수 있는 팩터리 메서드를 지정합니다.|
|`x:FieldModifier`|명명된 XAML 요소의 생성된 필드에 대한 액세스 수준을 지정합니다.|
|`x:Key`|`ResourceDictionary`에서 각 리소스에 대해 고유한 사용자 지정 키를 지정합니다. 키 값은 XAML 리소스를 검색하는 데 사용되며 일반적으로 `StaticResource` 태그 확장에 대한 인수로 사용됩니다.|
|`x:Name`|XAML 요소의 런타임 개체 이름을 지정합니다. `x:Name`을 설정하는 것은 코드에서 변수를 선언하는 것과 비슷합니다.|
|`x:TypeArguments`|제네릭 유형 생성자에 제네릭 유형 인수를 지정합니다.|

`x:DataType` 특성에 대한 자세한 내용은 [컴파일 된 바인딩](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)을 참조하십시오. `x:FieldModifier` 특성에 대한 자세한 내용은 [필드 한정자](~/xamarin-forms/xaml/field-modifiers.md)를 참조하십시오. `x:Arguments`, `x:FactoryMethod` 및 `x:TypeArguments` 특성에 대한 자세한 내용은 [XAML의 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)을 참조하십시오.

XAML에서 네임 스페이스 선언은 부모 요소에서 자식 요소로 상속됩니다. 따라서 XAML 파일의 루트 요소 네임 스페이스를 정의하면 해당 파일의 모든 요소가 네임 스페이스 선언을 상속합니다.

## <a name="declaring-namespaces-for-types"></a>유형에 대한 네임 스페이스 선언

유형은 접두사가 있는 XAML 네임 스페이스를 선언하고 CLR(Common Language Runtime, 공용 언어 런타임) 네임 스페이스 이름을 지정하는 네임 스페이스 선언 및 선택적 어셈블리 이름을 사용하여 XAML에서 참조할 수 있습니다. 이것은 네임 스페이스 선언 내에서 다음 키워드에 대한 값을 정의하여 수행됩니다.

- **clr-namespace:** 또는 **using:** – CLR 네임 스페이스는 XAML 요소로 표시를 위해 유형을 포함하는 어셈블리 내에 선언됩니다. 해당 키워드는 필수입니다.
- **assembly=** – 참조된 CLR 네임 스페이스를 포함하는 어셈블리입니다. 해당 값은 파일 확장명이 없는 어셈블리의 이름입니다. 어셈블리 경로는 어셈블리를 참조할 XAML 파일이 포함된 프로젝트 파일의 참조로 설정해야 합니다. **clr-namespace** 값이 해당 유형을 참조하는 응용 프로그램 코드와 동일한 어셈블리 내에 있으면 해당 키워드를 생략할 수 있습니다.

`clr-namespace` 또는 `using` 토큰을 해당 값과 분리하는 문자는 콜론인 반면, `assembly` 토큰을 해당 값에서 분리하는 문자는 등호입니다. 두 개의 토큰 사이에 사용할 문자는 세미콜론입니다.

다음 코드 예제는 XAML 네임 스페이스 선언을 보여줍니다.

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

또는 다음과 같이 작성할 수 있습니다.

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

`local` 접두사는 네임 스페이스 내의 유형이 응용 프로그램에 대해 로컬임을 나타내는 데 사용되는 규칙입니다. 또는 유형이 다른 어셈블리에 있는 경우 다음 XAML 코드 예제에서와 같이 어셈블리 이름도 네임 스페이스 선언에 정의해야 합니다.

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

다음 XAML 코드 예제에서와 같이 가져온 네임 스페이스에서 유형의 인스턴스를 선언할 때 네임 스페이스 접두사가 지정됩니다.

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

## <a name="summary"></a>요약

이 문서에서는 XAML 네임 스페이스 구문을 소개하고 XAML 네임 스페이스를 선언하여 유형에 접근하는 방법을 보여줍니다. XAML은 네임 스페이스 선언에 `xmlns` XAML 특성을 사용하여 접두사가 있는 XAML 네임 스페이스를 선언하여 XML에서 유형을 참조할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [XAML의 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)
