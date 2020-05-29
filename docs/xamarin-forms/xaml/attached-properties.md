---
title: ''
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
ms.openlocfilehash: 1f26a4415a75b2b02fd7d6893e366ef81156f077
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138192"
---
# <a name="attached-properties"></a>연결된 속성

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)


연결 된 속성을 사용 하면 개체에서 자체 클래스가 정의 하지 않는 속성에 대 한 값을 할당할 수 있습니다. 예를 들어 자식 요소는 연결 된 속성을 사용 하 여 사용자 인터페이스에 표시 되는 방식에 대 한 부모 요소를 알릴 수 있습니다. [`Grid`](xref:Xamarin.Forms.Grid)컨트롤을 사용 하면 및 연결 된 속성을 설정 하 여 자식의 행과 열을 지정할 수 있습니다 `Grid.Row` `Grid.Column` . `Grid.Row`및는 `Grid.Column` 자체가 아닌의 자식인 요소에 대해 설정 되기 때문에 연결 된 속성입니다 `Grid` `Grid` .

다음 시나리오에서는 바인딩 가능한 속성을 연결 된 속성으로 구현 해야 합니다.

- 정의 하는 클래스 이외의 클래스에 사용할 수 있는 속성 설정 메커니즘이 필요할 때
- 클래스가 다른 클래스와 쉽게 통합 해야 하는 서비스를 나타내는 경우

바인딩 가능한 속성에 대 한 자세한 내용은 [바인딩 가능한 속성](~/xamarin-forms/xaml/bindable-properties.md)을 참조 하세요.

## <a name="create-an-attached-property"></a>연결 된 속성 만들기

연결 된 속성을 만드는 프로세스는 다음과 같습니다.

1. [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)메서드 오버 로드 중 하나를 사용 하 여 인스턴스를 만듭니다 [`CreateAttached`](xref:Xamarin.Forms.BindableProperty.CreateAttached*) .
1. `static` `Get` *Propertyname* 및 `Set` *propertyname* 메서드를 연결 된 속성에 대 한 접근자로 제공 합니다.

### <a name="create-a-property"></a>속성 만들기

다른 형식에서 사용할 연결 된 속성을 만들 때 속성이 만들어지는 클래스는에서 파생 될 필요가 없습니다 [`BindableObject`](xref:Xamarin.Forms.BindableObject) . 그러나 접근자의 *target* 속성은 이거나에서 파생 되어야 [`BindableObject`](xref:Xamarin.Forms.BindableObject) 합니다.

형식의 속성을 선언 하 여 연결 된 속성을 만들 수 있습니다 `public static readonly` [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 바인딩 가능한 속성은 [ `BindableProperty.CreateAttached` ] (f:) 중 하나의 반환 된 값으로 설정 해야 합니다 Xamarin.Forms . BindableProperty. CreateAttached (System.string, system.object, System.string, System.object, Xamarin.Forms . BindingMode, Xamarin.Forms . BindableProperty, Xamarin.Forms . BindableProperty. BindingPropertyChangedDelegate, Xamarin.Forms . BindableProperty, Xamarin.Forms . BindableProperty, Xamarin.Forms . BindableProperty. CreateDefaultValueDelegate) 메서드 오버 로드 선언은 소유 하는 클래스의 본문 내에 있지만 멤버 정의 외부에 있어야 합니다.

다음 코드에서는 연결 된 속성의 예를 보여 줍니다.

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

그러면 형식의 이라는 연결 된 속성이 만들어집니다 `HasShadow` `bool` . 속성은 클래스에 의해 소유 `ShadowEffect` 되며 기본값은 `false` 입니다. 연결 된 속성에 대 한 명명 규칙은 연결 된 속성 식별자가 메서드에 지정 된 속성 이름과 일치 해야 한다는 것입니다 `CreateAttached` . 여기에는 "property"가 추가 됩니다. 따라서 위의 예제에서 연결 된 속성 식별자는 `HasShadowProperty` 입니다.

생성 중에 지정할 수 있는 매개 변수를 포함 하 여 바인딩 가능한 속성을 만드는 방법에 대 한 자세한 내용은 [바인딩 가능한 속성 만들기](~/xamarin-forms/xaml/bindable-properties.md#consume-a-bindable-property)를 참조 하세요.

### <a name="create-accessors"></a>접근자 만들기

정적 `Get` *PropertyName* 및 `Set` *PropertyName* 메서드는 연결 된 속성에 대 한 접근자로 필요 합니다. 그렇지 않으면 속성 시스템에서 연결 된 속성을 사용할 수 없습니다. `Get` *PropertyName* 접근자는 다음 서명을 준수 해야 합니다.

```csharp
public static valueType GetPropertyName(BindableObject target)
```

`Get` *PropertyName* 접근자는 연결 된 속성의 해당 필드에 포함 된 값을 반환 해야 합니다 `BindableProperty` . 이는 [ `GetValue` ] (f:)를 호출 하 여 수행할 수 있습니다 Xamarin.Forms . BindableObject. GetValue ( Xamarin.Forms . BindableProperty)) 메서드는 값을 가져올 바인딩 가능한 속성 식별자를 전달 하 고 결과 값을 필요한 형식으로 캐스팅 합니다.

`Set` *PropertyName* 접근자는 다음 서명을 준수 해야 합니다.

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

`Set` *PropertyName* 접근자는 연결 된 속성에 해당 하는 필드의 값을 설정 해야 합니다 `BindableProperty` . 이는 [ `SetValue` ] (f:)를 호출 하 여 수행할 수 있습니다 Xamarin.Forms . BindableObject. SetValue ( Xamarin.Forms . BindableProperty, System.object) 메서드는 값을 설정할 바인딩 가능한 속성 식별자와 설정할 값을 전달 합니다.

두 접근자에 대해 *대상* 개체는의 이거나,에서 파생 되어야 [`BindableObject`](xref:Xamarin.Forms.BindableObject) 합니다.

다음 코드 예제에서는 연결 된 속성에 대 한 접근자를 보여 줍니다 `HasShadow` .

```csharp
public static bool GetHasShadow (BindableObject view)
{
  return (bool)view.GetValue (HasShadowProperty);
}

public static void SetHasShadow (BindableObject view, bool value)
{
  view.SetValue (HasShadowProperty, value);
}
```

### <a name="consume-an-attached-property"></a>연결 된 속성 사용

연결 된 속성이 생성 되 면 XAML 또는 코드에서 사용 될 수 있습니다. XAML에서는 접두사를 사용 하 여 네임 스페이스를 선언 하 고 CLR (공용 언어 런타임) 네임 스페이스 이름 및 선택적으로 어셈블리 이름을 나타내는 네임 스페이스 선언을 사용 하 여이를 구현 합니다. 자세한 내용은 [XAML 네임 스페이스](~/xamarin-forms/xaml/namespaces.md)를 참조 하세요.

다음 코드 예제에서는 사용자 지정 형식을 참조 하는 응용 프로그램 코드와 동일한 어셈블리 내에 정의 된 연결 된 속성을 포함 하는 사용자 지정 형식에 대 한 XAML 네임 스페이스를 보여 줍니다.

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

다음 XAML 코드 예제와 같이 특정 컨트롤에서 연결 된 속성을 설정 하는 경우 네임 스페이스 선언이 사용 됩니다.

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

동등한 C# 코드는 다음 코드 예제와 같습니다.

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consume-an-attached-property-with-a-style"></a>스타일을 사용 하 여 연결 된 속성 사용

연결 된 속성을 스타일을 기준으로 컨트롤에 추가할 수도 있습니다. 다음 XAML 코드 예제에서는 *explicit* `HasShadow` 컨트롤에 적용할 수 있는 연결 된 속성을 사용 하는 명시적 스타일을 보여 줍니다 [`Label`](xref:Xamarin.Forms.Label) .

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

다음 코드 예제에 설명된 대로 `StaticResource` 태그 확장을 사용하여 해당 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 속성을 `Style` 인스턴스로 설정하면 [`Style`](xref:Xamarin.Forms.Style)을 [`Label`](xref:Xamarin.Forms.Label)에 적용할 수 있습니다.

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

스타일에 대한 자세한 내용은 [스타일](~/xamarin-forms/user-interface/styles/index.md)을 참조하세요.

## <a name="advanced-scenarios"></a>고급 시나리오

연결 된 속성을 만들 때 고급 연결 속성 시나리오를 사용 하도록 설정할 수 있는 여러 가지 선택적 매개 변수가 있습니다. 여기에는 속성 변경 검색, 속성 값의 유효성 검사 및 강제 변환 속성 값이 포함 됩니다. 자세한 내용은 [고급 시나리오](~/xamarin-forms/xaml/bindable-properties.md#advanced-scenarios)를 참조 하십시오.

## <a name="related-links"></a>관련 링크

- [바인딩 가능한 속성](~/xamarin-forms/xaml/bindable-properties.md)
- [XAML 네임스페이스](~/xamarin-forms/xaml/namespaces.md)
- [그림자 효과(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)
- [BindableProperty API](xref:Xamarin.Forms.BindableProperty)
- [BindableObject API](xref:Xamarin.Forms.BindableObject)
