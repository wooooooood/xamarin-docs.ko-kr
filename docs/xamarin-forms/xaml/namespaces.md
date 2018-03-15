---
title: "XAML 네임스페이스"
description: "XAML 네임 스페이스 선언에 대 한 xmlns XML 특성을 사용합니다. 이 문서는 XAML 네임 스페이스 구문을 소개 하 고 형식 액세스 하는 XAML 네임 스페이스를 선언 하는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/10/2017
ms.openlocfilehash: 55b83151e9c345096aeb0bfdd686d50c5fde62fd
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="xaml-namespaces"></a>XAML 네임스페이스

_XAML 네임 스페이스 선언에 대 한 xmlns XML 특성을 사용합니다. 이 문서는 XAML 네임 스페이스 구문을 소개 하 고 형식 액세스 하는 XAML 네임 스페이스를 선언 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

항상 XAML 파일의 루트 요소 내에 있는 XAML 네임 스페이스 선언을 두 가지가 있습니다. 다음 XAML 코드 예제에 표시 된 대로 첫 번째 기본 네임 스페이스를 정의 합니다.

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

기본 네임 스페이스 접두사가 없는 XAML 파일에 정의 된 요소 Xamarin.Forms 클래스와 같은 참조 지정 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)합니다.

두 번째 네임 스페이스 선언에 사용 된 `x` 다음 XAML 코드 예제에 표시 된 대로 접두사:

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML 네임 스페이스 내에서 형식을 참조할 때 사용 되는 접두사와 기본이 아닌 네임 스페이스를 선언 하려면 접두사를 사용 합니다. `x` 의 접두사와 함께 XAML 내에 정의 된 요소는 네임 스페이스 선언을 지정 `x` XAML (특히 2009 XAML 사양)에 내장 된 요소 및 특성에 사용 됩니다.

다음 표에서 윤곽선은 `x` Xamarin.Forms에서 지 원하는 네임 스페이스 특성:

|구문|설명|
|--- |--- |
|`x:Arguments`|기본이 아닌 생성자 또는 팩터리 메서드 개체 선언에 대 한 생성자 인수를 지정합니다.|
|`x:Class`|XAML에서 정의 된 클래스에 대 한 네임 스페이스 및 클래스 이름을 지정 합니다. 클래스 이름에는 코드 숨김 파일의 클래스 이름을 일치 해야 합니다. 이 생성자는 XAML 파일의 루트 요소에만 나타날 수 있는 참고 합니다.|
|`x:FactoryMethod`|개체를 초기화 하는 데 사용할 수 있는 팩터리 메서드를 지정 합니다.|
|`x:Key`|각 리소스에 대 한 사용자 지정 고유 키에 지정 된 `ResourceDictionary`합니다. 키의 값 XAML 리소스를 검색 하는 데 사용 되 고에 대 한 인수로 일반적으로 사용 되는 `StaticResource` 태그 확장 합니다.|
|`x:Name`|XAML 요소에 대 한 런타임 개체 이름을 지정합니다. 설정 `x:Name` 코드에서 변수를 선언 하는 것과 비슷합니다.|
|`x:TypeArguments`|제네릭 형식의 생성자에 대 한 제네릭 형식 인수를 지정합니다.|

에 대 한 자세한 내용은 `x:Arguments`, `x:FactoryMethod`, 및 `x:TypeArguments` 특성 참조 [XAML의 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)합니다.

Xaml에서는 자식 요소에 네임 스페이스 선언을 부모 요소 로부터 상속합니다. 따라서 XAML 파일의 루트 요소에서 네임 스페이스를 정의 하면 해당 파일 내의 모든 요소는 네임 스페이스 선언을 상속 합니다.

## <a name="declaring-namespaces-for-types"></a>형식에 대 한 네임 스페이스를 선언합니다.

형식은 공용 언어 런타임 (CLR) 네임 스페이스 이름 및 필요에 따라 어셈블리 이름을 지정 하는 네임 스페이스 선언 사용는 접두사가 포함 된 XAML 네임 스페이스를 선언 하 여 XAML에서 참조할 수 있습니다. 이 작업은 네임 스페이스 선언 내에서 다음 키워드에 대 한 값을 정의 하 여 수행 됩니다.

- **clr 네임 스페이스:** 또는 **를 사용 하 여:** – XAML 요소로 노출 되는 형식을 포함 하는 어셈블리 내에서 CLR 네임 스페이스 선언 합니다. 이 키워드를 사용 합니다.
- **어셈블리 =** – 참조 된 CLR 네임 스페이스를 포함 하는 어셈블리입니다. 이 값은 파일 확장명이 없는 어셈블리의 이름입니다. 어셈블리를 참조 하는 XAML 파일을 포함 하는 프로젝트 파일에 대 한 참조로는 어셈블리의 경로를 설정 해야 합니다. 경우에이 키워드를 생략할 수 있습니다는 **clr 네임 스페이스** 값이 형식을 참조 하 고 있는 응용 프로그램 코드와 동일한 어셈블리 내에 있습니다.

구분 문자에 유의 `clr-namespace` 또는 `using` 토큰과 해당 값은 콜론으로 반면 문자 구분 하는 `assembly` 토큰과 해당 값은 등호 기호입니다. 두 토큰 사이 사용할 문자는 세미콜론입니다.

다음 코드 예제는 XAML 네임 스페이스 선언을 보여 줍니다.

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

또는이으로 작성할 수 있습니다.

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

`local` 접두사는 네임 스페이스 내의 형식 응용 프로그램에 로컬인 나타내는 데 사용 되는 규칙입니다. 또는 형식을 다른 어셈블리에 있는 경우 어셈블리 이름도 정의 해야 네임 스페이스 선언에 다음 XAML 코드 예제에서와 같이:

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

다음 XAML 코드 예제에서 보여 준로 가져온된 네임 스페이스에서 형식의 인스턴스를 선언 하는 경우에 다음 네임 스페이스 접두사 지정 됩니다.

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

## <a name="summary"></a>요약

이 문서는 XAML 네임 스페이스 구문 도입 하 고 형식 액세스 하는 XAML 네임 스페이스를 선언 하는 방법을 보여 줍니다. XAML에서 사용 하 여 `xmlns` 접두사가 포함 된 XAML 네임 스페이스를 선언 하 여 XAML에서 네임 스페이스 선언 및 형식에 대 한 XML 특성을 참조할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [XAML의 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)
