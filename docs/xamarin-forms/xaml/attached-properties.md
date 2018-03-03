---
title: "연결된 속성"
description: "연결된 된 속성은 특수 한 유형의 바인딩 가능한 속성을 하나의 클래스에 정의 되었지만 다른 개체에 연결 하는 클래스를 포함 하는 특성으로 XAML에서 인식할 수 있는 및 마침표로 구분 되는 속성 이름입니다. 이 문서에서는 연결 된 속성을 소개 하 고 만들고 하 게 사용 하는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 7112812c843ccbcd6c24ea028deae3c09851b03d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="attached-properties"></a>연결된 속성

_연결된 된 속성은 특수 한 유형의 바인딩 가능한 속성을 하나의 클래스에 정의 되었지만 다른 개체에 연결 하는 클래스를 포함 하는 특성으로 XAML에서 인식할 수 있는 및 마침표로 구분 되는 속성 이름입니다. 이 문서에서는 연결 된 속성을 소개 하 고 만들고 하 게 사용 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

속성을 사용 하지 않는 자체 클래스를 정의 하는 속성에 대 한 값을 할당 하는 개체를 연결 합니다. 예를 들어, 자식 요소 צ ְ ײ 연결 속성을 사용자 인터페이스에 표시 되는 방법의 부모 요소에 게 알립니다. [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) 행과 열을 설정 하 여 지정할 자식 컨트롤을 사용 하는 `Grid.Row` 및 `Grid.Column` 연결 된 속성입니다. `Grid.Row` 및 `Grid.Column` 의 자식 요소에 대해 설정 된 연결 된 속성은 한 `Grid`, 대신에 `Grid` 자체입니다.

바인딩 가능 속성은 다음 시나리오에 연결 된 속성으로 구현 해야 합니다.

- 속성 설정 메커니즘을 사용할 수 있는 클래스에 대 한 이외의 할 필요가 있으면 정의 하는 클래스가 됩니다.
- 때이 클래스는 다른 클래스와 쉽게 통합 되어야 하는 서비스를 나타냅니다.

바인딩 가능한 속성에 대 한 자세한 내용은 참조 [바인딩 가능 속성](~/xamarin-forms/xaml/bindable-properties.md)합니다.

## <a name="creating-and-consuming-an-attached-property"></a>만들기 및 연결된 된 속성 사용

연결된 된 속성을 만드는 프로세스는 다음과 같습니다.

1. 만들기는 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 중 하나를 사용 하 여 인스턴스는 [ `CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) 메서드 오버 로드 합니다.
1. 제공 `static` `Get` *PropertyName* 및 `Set` *PropertyName* 접근자와 연결 된 속성에 대 한 메서드.

### <a name="creating-a-property"></a>속성 만들기

다른 형식에 사용 하기 위해 연결된 된 속성을 만들 때의 속성이 생성 된 클래스에서 파생 않아도 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)합니다. 그러나는 *대상* 속성 접근자에 대해, 이어야 하거나 컨트롤에서 파생 해야 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)합니다.

연결된 된 속성을 선언 하 여 만들 수 있습니다는 `public static readonly` 형식의 속성이 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)합니다. 바인딩 가능한 속성 중 하나의 반환 된 값으로 설정 해야는 [ `BindableProperty.CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) 메서드 오버 로드 합니다. 하지만 다른 모든 멤버 정의 외부에서 소유 하는 클래스의 본문 내에서 선언 해야 합니다.

다음 코드에서는 연결된 된 속성의 예를 보여 줍니다.

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

이렇게 하면 연결된 된 속성 이름이 만들어집니다. `HasShadow`, 형식의 `bool`합니다. 속성을 소유는 `ShadowEffect` 클래스와의 기본값이 `false`합니다. 연결 된 속성에 대 한 명명 규칙은 연결 된 속성 식별자에 지정 된 속성 이름과 일치 해야 하는 `CreateAttached` 메서드를 추가 하는 "Property"입니다. 따라서 위의 예제에서 연결 된 속성 식별자는 `HasShadowProperty`합니다.

만들기를 만드는 동안 지정할 수 있는 매개 변수를 포함 하 여 바인딩 가능한 속성에 대 한 자세한 내용은 참조 [만들기 및 바인딩 가능한 속성을 사용](~/xamarin-forms/xaml/bindable-properties.md#consuming-bindable-property)합니다.

### <a name="creating-accessors"></a>접근자 만들기

정적 `Get` *PropertyName* 및 `Set` *PropertyName* 메서드는 연결 된 속성에 대 한 접근자로, 그렇지 않으면 속성 시스템은 하지 사용 하는 연결 된 속성입니다. `Get` *PropertyName* 접근자는 다음 서명이 준수 해야 합니다.

```csharp
public static valueType GetPropertyName(BindableObject target)
```

`Get` *PropertyName* 접근자에 해당 포함 된 값을 반환 해야 `BindableProperty` 연결 된 속성에 대 한 필드입니다. 이 작업을 호출 하 여 수행할 수는 [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) 메서드는 값을 가져올에 바인딩 가능한 속성 식별자에 전달 하 고 다음을 필수 형식 결과 값을 캐스팅 합니다.

`Set` *PropertyName* 접근자는 다음 서명이 준수 해야 합니다.

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

`Set` *PropertyName* 접근자는 해당 값을 설정 해야 `BindableProperty` 연결 된 속성에 대 한 필드입니다. 이 작업을 호출 하 여 수행할 수는 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) 설정 값 및 값을 설정 하는 바인딩 가능한 속성 식별자에 전달 하는 메서드.

두 접근자에 대해는 *대상* 개체의 수 또는 컨트롤에서 파생 해야 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)합니다.

다음 코드 예제에 대 한 접근자는 `HasShadow` 연결 된 속성:

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

### <a name="consuming-an-attached-property"></a>연결된 된 속성 사용

연결된 된 속성을 만든 후에 XAML 또는 코드에서 사용할 수 있습니다. XAML 네임 스페이스 선언 나타내는 공용 언어 런타임 (CLR) 네임 스페이스 이름 및 필요에 따라 어셈블리 이름으로는 접두사와 네임 스페이스를 선언 하 여 작업을 수행 합니다. 자세한 내용은 참조 [XAML 네임 스페이스](~/xamarin-forms/xaml/namespaces.md)합니다.

다음 코드 예제에서는 사용자 지정 유형을 참조 하는 응용 프로그램 코드와 동일한 어셈블리 내에 정의 된 연결 된 속성을 포함 하는 사용자 지정 형식에 대 한 XAML 네임 스페이스를 보여 줍니다.

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

네임 스페이스 선언은 다음 XAML 코드 예제에서 보여 준으로 특정 컨트롤에 연결 된 속성을 설정할 때 사용 됩니다.

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

해당하는 C# 코드가 다음 코드 예제에 표시됩니다.

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consuming-an-attached-property-with-a-style"></a>스타일과 연결된 된 속성 사용

또한 연결 된 속성 스타일으로 컨트롤에 추가할 수 있습니다. 다음 XAML 코드 예제는 *명시적* 스타일을 사용 하 여 `HasShadow` 연결 된 속성에 적용할 수 있는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 컨트롤:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 에 적용할 수는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 설정 하 여 해당 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성을는 `Style` 는 를사용하여인스턴스`StaticResource`태그 확장, 다음 코드 예제에서와 같이:

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

스타일에 대 한 자세한 내용은 참조 [스타일](~/xamarin-forms/user-interface/styles/index.md)합니다.

## <a name="advanced-scenarios"></a>고급 시나리오

연결된 된 속성을 만들 때의 고급 연결된 속성 시나리오가 가능 하도록 설정할 수 있는 선택적 매개 변수는 여러 가지가 있습니다. 속성 변경 내용 검색, 속성 값 유효성 검사 및 변환 속성 값이 포함 됩니다. 자세한 내용은 참조 [고급 시나리오](~/xamarin-forms/xaml/bindable-properties.md#advanced)합니다.

## <a name="summary"></a>요약

이 문서는 연결 된 속성에 대 한 소개를 제공 하 고 만들고 하 게 사용 하는 방법을 살펴보았습니다. 연결된 된 속성은 특수 한 유형의 바인딩 가능한 속성을 다른 개체에 연결 되 고 XAML에서 인식할 수 있지만 하나의 클래스에서 속성 이름과 마침표로 구분 되는 클래스를 포함 하는 특성으로 정의 합니다.


## <a name="related-links"></a>관련 링크

- [바인딩 가능한 속성](~/xamarin-forms/xaml/bindable-properties.md)
- [XAML 네임스페이스](~/xamarin-forms/xaml/namespaces.md)
- [그림자 효과 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
