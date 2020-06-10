---
제목: "" description: "의 XAML 네임 스페이스 Xamarin.Forms 는 네임 스페이스 선언에 XMLNS XML 특성을 사용 합니다. 이 문서에서는 XAML 네임 스페이스 구문을 소개 하 고 형식에 액세스 하는 XAML 네임 스페이스를 선언 하는 방법을 보여 줍니다. "
assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F: xamarin-forms author: davidbritch: dabritch:: 08/21/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xaml-namespaces-in-xamarinforms"></a>XAML 네임 스페이스Xamarin.Forms

_XAML은 네임 스페이스 선언에 xmlns XML 특성을 사용 합니다. 이 문서에서는 XAML 네임 스페이스 구문을 소개 하 고 형식에 액세스 하는 XAML 네임 스페이스를 선언 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

XAML 파일의 루트 요소 내에는 항상 두 개의 XAML 네임 스페이스 선언이 있습니다. 첫 번째는 다음 XAML 코드 예제에 표시 된 것 처럼 기본 네임 스페이스를 정의 합니다.

```xaml
xmlns="http://xamarin.com/schemas/2014/forms"
```

기본 네임 스페이스는 접두사가 없는 XAML 파일에 정의 된 요소가와 같은 클래스를 참조 하도록 지정 합니다 Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) .

`x`다음 XAML 코드 예제에 표시 된 것 처럼 두 번째 네임 스페이스 선언에는 접두사가 사용 됩니다.

```xaml
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML에서는 접두사를 사용 하 여 네임 스페이스 내에서 형식을 참조할 때 접두사를 사용 하 여 기본이 아닌 네임 스페이스를 선언 합니다. 네임 스페이스 선언은 xaml에서 접두사를 사용 하 여 `x` xaml에 정의 된 요소가 `x` xaml에 내장 된 요소 및 특성 (특히 2009 xaml 사양)에 사용 되도록 지정 합니다.

다음 표에서는 `x` 에서 지원 되는 네임 스페이스 특성을 간략하게 설명 합니다 Xamarin.Forms .

|구문|Description|
|--- |--- |
|`x:Arguments`|기본이 아닌 생성자 또는 팩터리 메서드 개체 선언에 대 한 생성자 인수를 지정 합니다.|
|`x:Class`|XAML에 정의 된 클래스의 네임 스페이스 및 클래스 이름을 지정 합니다. 클래스 이름은 코드 숨김이 파일의 클래스 이름과 일치 해야 합니다. 이 구문은 XAML 파일의 루트 요소에만 나타날 수 있습니다.|
|`x:DataType`|XAML 요소 및 해당 자식 요소를 바인딩할 개체의 형식을 지정 합니다.|
|`x:FactoryMethod`|개체를 초기화 하는 데 사용할 수 있는 팩터리 메서드를 지정 합니다.|
|`x:FieldModifier`|명명 된 XAML 요소에 대해 생성 된 필드의 액세스 수준을 지정 합니다.|
|`x:Key`|의 각 리소스에 대 한 고유한 사용자 정의 키를 지정 합니다 `ResourceDictionary` . 키의 값은 XAML 리소스를 검색 하는 데 사용 되며 일반적으로 태그 확장에 대 한 인수로 사용 됩니다 `StaticResource` .|
|`x:Name`|XAML 요소에 대 한 런타임 개체 이름을 지정 합니다. 설정은 `x:Name` 코드에서 변수를 선언 하는 것과 비슷합니다.|
|`x:TypeArguments`|제네릭 형식의 생성자에 대 한 제네릭 형식 인수를 지정 합니다.|

특성에 대 한 자세한 내용은 `x:DataType` [컴파일된 바인딩](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)을 참조 하세요. 특성에 대 한 자세한 내용은 `x:FieldModifier` [필드 한정자](~/xamarin-forms/xaml/field-modifiers.md)를 참조 하십시오. 및 특성에 대 한 자세한 내용은 `x:Arguments` `x:FactoryMethod` [XAML로 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)을 참조 하세요. 특성에 대 한 자세한 내용은 `x:TypeArguments` [XAML의 제네릭을 Xamarin.Forms 사용 ](generics.md)을 참조 하세요.

> [!NOTE]
> 위에 나열 된 네임 스페이스 특성 외에 Xamarin.Forms 도 네임 스페이스 접두사를 통해 사용할 수 있는 태그 확장을 포함 합니다 `x` . 자세한 내용은 [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md)사용을 참조 하세요.

XAML에서 네임 스페이스 선언은 부모 요소에서 자식 요소로 상속 됩니다. 따라서 XAML 파일의 루트 요소에서 네임 스페이스를 정의할 때 해당 파일 내의 모든 요소는 네임 스페이스 선언을 상속 합니다.

## <a name="declaring-namespaces-for-types"></a>형식에 대 한 네임 스페이스 선언

접두사를 사용 하 여 XAML 네임 스페이스를 선언 하 고 CLR (공용 언어 런타임) 네임 스페이스 이름 및 선택적으로 어셈블리 이름을 지정 하는 네임 스페이스 선언을 사용 하 여 xaml에서 형식을 참조할 수 있습니다. 이렇게 하려면 네임 스페이스 선언 내에서 다음 키워드에 대 한 값을 정의 합니다.

- **clr 네임 스페이스:** 또는 **USING:** -XAML 요소로 노출할 형식을 포함 하는 어셈블리 내에 선언 된 clr 네임 스페이스입니다. 이 키워드는 필수입니다.
- **assembly =** – 참조 된 CLR 네임 스페이스를 포함 하는 어셈블리입니다. 이 값은 파일 확장명이 없는 어셈블리의 이름입니다. 어셈블리 경로는 어셈블리를 참조 하는 XAML 파일이 포함 된 프로젝트 파일에서 참조로 설정 되어야 합니다. **Clr 네임 스페이스** 값이 형식을 참조 하는 응용 프로그램 코드와 동일한 어셈블리 내에 있는 경우이 키워드를 생략할 수 있습니다.

또는 토큰을 해당 값과 구분 하는 문자는 `clr-namespace` `using` 콜론 이지만, 해당 값과 토큰을 구분 하는 문자는 `assembly` 등호가 있습니다. 두 토큰 간에 사용할 문자는 세미콜론입니다.

다음 코드 예제는 XAML 네임 스페이스 선언을 보여 줍니다.

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

`local`접두사는 네임 스페이스 내의 형식이 응용 프로그램에 로컬인 것을 나타내는 데 사용 되는 규칙입니다. 또는 형식이 다른 어셈블리에 있는 경우 다음 XAML 코드 예제에서 보여 주는 것 처럼 네임 스페이스 선언에도 어셈블리 이름을 정의 해야 합니다.

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

다음 XAML 코드 예제에서 보여 주는 것 처럼 가져온 네임 스페이스에서 형식의 인스턴스를 선언 하는 경우 네임 스페이스 접두사를 지정 합니다.

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

사용자 지정 네임 스페이스 스키마 정의에 대 한 자세한 내용은 [XAML 사용자 지정 네임 스페이스](custom-namespace-schemas.md)스키마를 참조 하세요.

## <a name="summary"></a>요약

이 문서에는 XAML 네임 스페이스 구문이 도입 되었으며 형식에 액세스 하는 XAML 네임 스페이스를 선언 하는 방법을 보여 주었습니다. XAML은 `xmlns` 네임 스페이스 선언에 XML 특성을 사용 하며, 접두사가 있는 xaml 네임 스페이스를 선언 하 여 xaml에서 형식을 참조할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [XAML에서 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)
- [XAML의 제네릭Xamarin.Forms](generics.md)
