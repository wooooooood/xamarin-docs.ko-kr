---
title: 바인딩 가능한 속성
description: 이 문서에서는 바인딩 가능한 속성에 대해 소개 하 고 만들고이 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/02/2016
ms.openlocfilehash: 8dc53c37894af70d5183fe5c44b018fdf25af616
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563851"
---
# <a name="bindable-properties"></a>바인딩 가능한 속성

_Xamarin.Forms에서 공용 언어 런타임 (CLR) 속성의 기능 바인딩 가능한 속성으로 확장 됩니다. 바인딩 가능한 속성을 속성의 값 Xamarin.Forms 속성 시스템에서 추적 되는 속성의 특수 형식입니다. 이 문서에서는 바인딩 가능한 속성에 대해 소개 하 고 만들고이 사용 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

속성을 사용 하 여 백업 하 여 CLR 속성 기능을 확장 하는 바인딩 가능한 속성을 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 대신 필드를 사용 하 여 속성을 지 원하는 형식. 바인딩 가능한 속성의 목적은 데이터 바인딩, 스타일, 템플릿, 지 속성 시스템을 제공 하 고 값이 부모-자식 관계를 통해 설정. 또한 바인딩 가능한 속성 기본값을 속성 변경 내용을 모니터링 하는 콜백 및 속성 값의 유효성 검사를 제공할 수 있습니다.

다음 기능 중 하나 이상을 지원 하도록 바인딩 가능한 속성으로 속성을 구현 해야 합니다.

- 역할을 하는 유효한 *대상* 데이터 바인딩에 대 한 속성입니다.
- 속성을 통해 설정 된 [스타일](~/xamarin-forms/user-interface/styles/index.md)합니다.
- 속성의 형식에 대 한 기본값과 다른 값으로 기본 속성 값을 제공 합니다.
- 속성의 값의 유효성을 검사 합니다.
- 속성 변경 내용을 모니터링 합니다.

Xamarin.Forms 바인딩 가능한 속성의 예로 [ `Label.Text` ](xref:Xamarin.Forms.Label.Text)합니다 [ `Button.BorderRadius` ](xref:Xamarin.Forms.Button.BorderRadius), 및 [ `StackLayout.Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation)합니다. 각 바인딩 가능한 속성에는 해당 `public static readonly` 형식의 속성 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 동일한 클래스에서 노출 되는 및 바인딩 가능한 속성의 식별자입니다. 예를 들어는 해당 바인딩 가능한 속성에 대 한 식별자를 `Label.Text` 속성은 [ `Label.TextProperty` ](xref:Xamarin.Forms.Label.TextProperty)합니다.

<a name="consuming-bindable-property" />

## <a name="creating-and-consuming-a-bindable-property"></a>만들기 및 바인딩 가능한 속성 사용

바인딩 가능한 속성을 만드는 프로세스는 다음과 같습니다.

1. 만들기는 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 중 하나를 사용 하 여 인스턴스를 [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create*) 메서드 오버 로드 합니다.
1. 속성 접근자를 정의 합니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 인스턴스.

모든 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) UI 스레드에서 인스턴스를 만들어야 합니다. 즉, UI 스레드에서 실행 되는 코드에만 가져오기 하거나 바인딩 가능한 속성의 값을 설정할 수 있습니다. 그러나 `BindableProperty` 인스턴스를 사용 하 여 UI 스레드 마샬링을 통해 다른 스레드에서 액세스할 수 있습니다 합니다 [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) 메서드.

### <a name="creating-a-property"></a>속성 만들기

만들려는 `BindableProperty` 인스턴스를 포함 하는 클래스에서 파생 되어야 합니다는 [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) 클래스입니다. 그러나는 `BindableObject` 클래스 이므로 클래스 계층에서 높은 사용자 인터페이스 기능 지원 바인딩 가능한 속성에 대 한 대부분의 클래스를 사용 합니다.

바인딩 가능한 속성을 선언 하 여 만들 수 있습니다는 `public static readonly` 형식의 속성 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)합니다. 반환된 된 값 중 하나에 바인딩할 수 있는 속성을 설정 해야 합니다 [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) 메서드 오버 로드 합니다. 선언 본문 내에 있어야 합니다. [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) 클래스를 파생 하지만 멤버 정의 외부입니다.

최소한 식별자를 지정 해야 합니다를 만들 때를 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty), 다음 매개 변수와 함께 합니다.

- 이름을 합니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)합니다.
- 속성의 형식입니다.
- 소유 하는 개체의 형식입니다.
- 속성의 기본값입니다. 이 속성을 설정 하지 않으면 속성의 형식에 대 한 기본 값에서 다를 수 있습니다 때 항상 특정 기본 값을 반환 하는지 확인 합니다. 기본 값은 복원 될 때를 [ `ClearValue` ](xref:Xamarin.Forms.BindableObject.ClearValue(Xamarin.Forms.BindableProperty)) 바인딩 가능한 속성에서 메서드를 호출 합니다.

다음 코드에 식별자와 4 개의 필수 매개 변수의 값을 사용 하 여 바인딩 가능한 속성의 예를 보여 줍니다.

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

이렇게 한 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 명명 된 인스턴스 `EventName`, 형식의 `string`합니다. 속성을 소유 하는 `EventToCommandBehavior` 클래스 및 기본 값 `null`합니다. 바인딩 가능한 속성에 대 한 명명 규칙은 바인딩 가능한 속성 식별자에 지정 된 속성 이름과 일치 해야 하는 `Create` 메서드를 추가 하는 "Property"를 사용 하 여 합니다. 따라서 위의 예제에서 바인딩 가능한 속성 식별자는 `EventNameProperty`합니다.

필요에 따라 만들면를 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 인스턴스, 다음 매개 변수를 지정할 수 있습니다.

- 바인딩 모드입니다. 이 속성 값이 변경 됩니다 전파 되는 방향을 지정 하려면 사용 됩니다. 기본 바인딩 모드에서는 변경 내용에서 전파 됩니다 합니다 *소스* 에 *대상*합니다.
- 속성 값을 설정 하면 호출 되는 유효성 검사 대리자입니다. 자세한 내용은 [유효성 검사 콜백은](#validation)합니다.
- 속성이 속성 값을 변경 될 때 호출 될 대리자를 변경 합니다. 자세한 내용은 [속성 변경 내용을 감지](#propertychanges)합니다.
- 속성 값은 변경 될 때 호출 될 대리자를 변경 하는 속성입니다. 이 대리자에 속성 변경 대리자와 동일한 서명이 있습니다.
- 속성 값을 변경 될 때 호출 되는 강제 값 대리자입니다. 자세한 내용은 [강제 값 콜백을](#coerce)합니다.
- `Func` 기본 속성 값을 초기화 하는 데 사용 되는 합니다. 자세한 내용은 [Func를 사용 하 여 기본 값을 만들기](#defaultfunc)합니다.

### <a name="creating-accessors"></a>접근자 만들기

속성 접근자 속성 구문을 사용 하 여 바인딩 가능한 속성에 액세스 해야 합니다. `Get` 접근자는 해당 바인딩 가능한 속성에 포함 된 값을 반환 해야 합니다. 이 호출 하 여 수행할 수 있습니다 합니다 [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) 메서드 값을 가져올를 바인딩 가능 속성 식별자에 전달 하 고 다음 결과 필요한 형식으로 캐스팅 합니다. `Set` 접근자는 해당 바인딩 가능한 속성의 값을 설정 해야 합니다. 이 호출 하 여 수행할 수 있습니다 합니다 [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) 값 및 설정할 값을 설정 하는 바인딩 가능한 속성 식별자를 전달 하는 메서드.

다음 코드 예제에서는 대 한 접근자를 `EventName` 바인딩 가능 속성:

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### <a name="consuming-a-bindable-property"></a>바인딩 가능한 속성 사용

바인딩 가능한 속성을 만든 후에 XAML 또는 코드에서 사용할 수 있습니다. XAML에서 CLR 네임 스페이스 이름 및 필요에 따라 어셈블리 이름이 나타내는 네임 스페이스 선언을 사용 하 여 접두사를 사용 하 여 네임 스페이스를 선언 하 여 작업을 수행 합니다. 자세한 내용은 [XAML 네임 스페이스](~/xamarin-forms/xaml/namespaces.md)합니다.

다음 코드 예제에서는 사용자 지정 형식을 참조 하는 응용 프로그램 코드와 동일한 어셈블리 내에 정의 된 바인딩 가능한 속성을 포함 하는 사용자 지정 형식에 대 한 XAML 네임 스페이스를 보여 줍니다.

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

네임 스페이스 선언을 설정할 때 사용 되는 `EventName` 다음 XAML 코드 예제에서 설명한 것 처럼 바인딩 가능 속성:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

해당하는 C# 코드가 다음 코드 예제에 표시됩니다.

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  ...
});
```

<a name="advanced" />

## <a name="advanced-scenarios"></a>고급 시나리오

만들 때를 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 인스턴스를 바인딩 가능 속성을 고급 시나리오를 사용 하도록 설정할 수 있는 선택적 매개 변수는 여러 가지가 있습니다. 이 섹션에서는 이러한 시나리오를 살펴봅니다.

<a name="propertychanges" />

### <a name="detecting-property-changes"></a>속성 변경 내용 감지

A `static` 속성 변경 콜백 메서드를 지정 하 여 바인딩 가능한 속성을 사용 하 여 등록할 수 있습니다 합니다 `propertyChanged` 에 대 한 매개 변수를 [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) 메서드. 지정된 된 콜백 메서드는 바인딩 가능한 속성의 값이 변경 될 때 호출 됩니다.

다음 코드 예제에서는 하는 방법을 `EventName` 바인딩 가능 속성 레지스터는 `OnEventNameChanged` 속성 변경 콜백 메서드로 메서드:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create (
    "EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
...

static void OnEventNameChanged (BindableObject bindable, object oldValue, object newValue)
{
  // Property changed implementation goes here
}
```

속성 변경 콜백 메서드에서 [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) 매개 변수는 소유 하는 클래스의 인스턴스를 변경 하 고 두 값을 보고 했습니다 나타내는 데 `object` 의 이전 및 새 값을 표시 하는 매개 변수 바인딩 가능한 속성입니다.

<a name="validation" />

### <a name="validation-callbacks"></a>유효성 검사 콜백

A `static` 유효성 검사 콜백 메서드를 지정 하 여 바인딩 가능한 속성을 사용 하 여 등록할 수 있습니다 합니다 `validateValue` 에 대 한 매개 변수를 [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) 메서드. 바인딩 가능한 속성의 값을 설정 하면 지정된 된 콜백 메서드를 호출 됩니다.

다음 코드 예제에서는 하는 방법을 `Angle` 바인딩 가능 속성 레지스터는 `IsValidValue` 유효성 검사 콜백 메서드로 메서드:

```csharp
public static readonly BindableProperty AngleProperty =
  BindableProperty.Create ("Angle", typeof(double), typeof(HomePage), 0.0, validateValue: IsValidValue);
...

static bool IsValidValue (BindableObject view, object value)
{
  double result;
  bool isDouble = double.TryParse (value.ToString (), out result);
  return (result >= 0 && result <= 360);
}
```

유효성 검사 콜백은 값으로 제공 되 고 반환할지 `true` 그렇지 않으면 값이 속성에 대 한 유효한 경우 `false`합니다. 유효성 검사 콜백을 반환 하는 경우 예외가 발생 `false`, 개발자가 처리 해야 합니다. 유효성 검사 콜백 메서드의 일반적인 사용 바인딩 가능한 속성을 설정 하면 정수 또는 double 값을 제한 됩니다. 예를 들어 합니다 `IsValidValue` 메서드는 속성 값이 있는지 확인을 `double` 범위인 0-360입니다.

<a name="coerce" />

### <a name="coerce-value-callbacks"></a>강제 값 콜백

A `static` 강제 값 콜백 메서드를 지정 하 여 바인딩 가능한 속성을 사용 하 여 등록할 수 있습니다 합니다 `coerceValue` 에 대 한 매개 변수를 [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) 메서드. 지정된 된 콜백 메서드는 바인딩 가능한 속성의 값이 변경 될 때 호출 됩니다.

강제 값 콜백을 속성의 값이 변경 될 때 바인딩 가능한 속성의 재평가 강제로 실행 하는 데 사용 됩니다. 예를 들어, 하나의 바인딩 가능한 속성의 값을 다른 바인딩 가능한 속성의 값 보다 크지 않은 되도록 강제 값 콜백을 사용할 수 있습니다.

다음 코드 예제에서는 하는 방법을 `Angle` 바인딩 가능 속성 레지스터는 `CoerceAngle` 강제 값 콜백 메서드로 메서드:

```csharp
public static readonly BindableProperty AngleProperty = BindableProperty.Create (
  "Angle", typeof(double), typeof(HomePage), 0.0, coerceValue: CoerceAngle);
public static readonly BindableProperty MaximumAngleProperty = BindableProperty.Create (
  "MaximumAngle", typeof(double), typeof(HomePage), 360.0);
...

static object CoerceAngle (BindableObject bindable, object value)
{
  var homePage = bindable as HomePage;
  double input = (double)value;

  if (input > homePage.MaximumAngle) {
    input = homePage.MaximumAngle;
  }

  return input;
}
```

`CoerceAngle` 메서드 값을 확인를 `MaximumAngle` 속성인 경우에 `Angle` 속성 값이 보다 큰 값으로 강제 변환를 `MaximumAngle` 속성 값.

<a name="defaultfunc" />

### <a name="creating-a-default-value-with-a-func"></a>함수를 사용 하 여 기본값 만들기

`Func` 다음 코드 예제에서 설명한 것 처럼 바인딩 가능한 속성의 기본 값을 초기화 하기 사용할 수 있습니다.

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator` 매개 변수는 설정를 `Func` 를 호출 하는 합니다 [ `Device.GetNamedSize` ](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) 반환 하는 방법을 `double` 에 사용 되는 글꼴에 대 한 명명 된 크기를 나타내는 [ `Label` ](xref:Xamarin.Forms.Label) 네이티브 플랫폼입니다.

## <a name="summary"></a>요약

이 문서는 바인딩 가능한 속성에 대 한 소개를 제공 하 고 만들고이 사용 하는 방법을 보여 줍니다. 바인딩 가능한 속성을 속성의 값 Xamarin.Forms 속성 시스템에서 추적 되는 속성의 특수 형식입니다.


## <a name="related-links"></a>관련 링크

- [XAML 네임스페이스](~/xamarin-forms/xaml/namespaces.md)
- [이벤트: 명령 동작 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [유효성 검사 콜백 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/xaml/validationcallback/)
- [강제 값 콜백 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/xaml/coercevaluecallback/)
- [BindableProperty](xref:Xamarin.Forms.BindableProperty)
- [BindableObject](xref:Xamarin.Forms.BindableObject)
