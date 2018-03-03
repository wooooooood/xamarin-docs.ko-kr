---
title: "바인딩 가능한 속성"
description: "Xamarin.forms에 공용 언어 런타임 (CLR) 속성의 기능을 바인딩할 수 있는 속성에 의해 확장 됩니다. 바인딩 가능한 속성은 속성의 값 Xamarin.Forms 속성 시스템에 의해 추적 되는 위치는 특수 한 유형의 속성. 이 문서에서는 바인딩 가능 속성을 소개 하 고 만들고 하 게 사용 하는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: ab8c4cfd92a048efb87f7508e53fc024a9c46405
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="bindable-properties"></a>바인딩 가능한 속성

_Xamarin.forms에 공용 언어 런타임 (CLR) 속성의 기능을 바인딩할 수 있는 속성에 의해 확장 됩니다. 바인딩 가능한 속성은 속성의 값 Xamarin.Forms 속성 시스템에 의해 추적 되는 위치는 특수 한 유형의 속성. 이 문서에서는 바인딩 가능 속성을 소개 하 고 만들고 하 게 사용 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

바인딩 가능 속성을 갖는 속성을 백업 하 여 CLR 속성 기능을 확장 한 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 필드와 속성을 백업 하는 대신 형식입니다. 바인딩 가능한 속성의 목적은 데이터 바인딩, 스타일, 템플릿, 지 속성 시스템을 제공 하 고 값이 부모-자식 관계를 통해 설정 합니다. 또한 바인딩 가능한 속성의 속성 값, 속성 변경 내용을 모니터링 하는 콜백 유효성 검사 기본값을 제공할 수 있습니다.

다음과 같은 기능 중 하나 이상을 지원 하도록 바인딩 가능한 속성으로 속성을 구현 해야 합니다.

- 유효한 역할 *대상* 데이터 바인딩에 대 한 속성.
- 속성을 통해 설정 된 [스타일](~/xamarin-forms/user-interface/styles/index.md)합니다.
- 속성의 형식에 대 한 기본값과 다른 속성 기본값을 제공 합니다.
- 속성의 값의 유효성 검사.
- 속성 변경 내용을 모니터링 합니다.

Xamarin.Forms 바인딩 가능한 속성의 예로 [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/), [ `Button.BorderRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/), 및 [ `StackLayout.Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/)합니다. 각 바인딩 가능한 속성에는 해당 `public static readonly` 형식의 속성이 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 동일한 클래스에 노출 되는 및 바인딩 가능한 속성의 식별자입니다. 예를 들어의 해당 바인딩 가능 속성 식별자는 `Label.Text` 속성은 [ `Label.TextProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Label.TextProperty/)합니다.

<a name="consuming-bindable-property" />

## <a name="creating-and-consuming-a-bindable-property"></a>만들기 및 바인딩 가능한 속성 사용

바인딩 가능한 속성을 만드는 프로세스는 다음과 같습니다.

1. 만들기는 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 중 하나를 사용 하 여 인스턴스는 [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) 메서드 오버 로드 합니다.
1. 속성 접근자에 대 한 정의 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 인스턴스.

모든 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) UI 스레드에서 인스턴스를 만들어야 합니다. 즉, UI 스레드에서 실행 되는 코드에만 가져올 하거나 바인딩 가능한 속성의 값을 설정할 수 있습니다. 그러나 `BindableProperty` 인스턴스와 UI 스레드로 마샬링에서 다른 스레드에서 액세스할 수는 [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) 메서드.

### <a name="creating-a-property"></a>속성 만들기

만들려는 `BindableProperty` 인스턴스를 포함 하는 클래스에서 파생 되어야 합니다는 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) 클래스입니다. 그러나는 `BindableObject` 클래스는 클래스 계층에서 높은 사용자 인터페이스 기능 지원 바인딩 가능한 속성에 대 한 대부분의 클래스를 사용 합니다.

바인딩 가능한 속성을 선언 하 여 만들 수 있습니다는 `public static readonly` 형식의 속성이 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)합니다. 바인딩 가능한 속성 중 하나의 반환 된 값으로 설정 해야는 [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) 메서드 오버 로드 합니다. 선언의 본문 내에 있어야 합니다. [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) 파생 클래스를 있지만 다른 모든 멤버 정의 밖에 있습니다.

여기에 최소한 식별자 지정 해야 합니다를 만들 때 한 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/), 함께 다음 매개 변수:

- 이름에서 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)합니다.
- 속성의 형식입니다.
- 소유 하는 개체의 형식입니다.
- 속성에 대 한 기본 값입니다. 이렇게 하는 속성은 항상 반환 특정 기본값을 설정 하지 않으면 하 고 속성의 형식에 대 한 기본 값과에서 다를 수 있습니다. 기본 값이 됩니다 복원할 때는 [ `ClearValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.ClearValue/p/Xamarin.Forms.BindableProperty/) 바인딩 가능 속성 메서드를 호출 합니다.

다음 코드는 식별자와 4 개의 필수 매개 변수에 대해 값이 되는 바인딩 가능한 속성의 예를 보여 줍니다.

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

그렇기 때문에 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 명명 된 인스턴스 `EventName`, 형식의 `string`합니다. 속성을 소유는 `EventToCommandBehavior` 클래스와의 기본값이 `null`합니다. 바인딩 가능한 속성에 대 한 명명 규칙은 바인딩 가능한 속성 식별자에 지정 된 속성 이름과 일치 해야 하는 `Create` 메서드를 추가 하는 "Property"입니다. 따라서 위의 예제에서 바인딩 가능한 속성 식별자는 `EventNameProperty`합니다.

필요에 따라 만들 때 한 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 인스턴스를 다음 매개 변수를 지정할 수 있습니다.

- 바인딩 모드입니다. 속성 값이 변경 전파 됩니다 방향을 지정 하는이 사용 됩니다. 기본 바인딩 모드 변경에서 전파 됩니다는 *소스* 에 *대상*합니다.
- 속성 값을 설정 하면 호출 되는 유효성 검사 대리자입니다. 자세한 내용은 참조 [유효성 검사 콜백은](#validation)합니다.
- 속성이 속성 값 변경 될 때 호출 될 대리자를 변경 합니다. 자세한 내용은 참조 [속성 변경 내용을 감지](#propertychanges)합니다.
- 속성 값은 변경 될 때 호출 될 대리자를 변경 하는 속성입니다. 이 대리자에 속성 변경 대리자와 동일한 서명이 있습니다.
- 속성 값 변경 될 때 호출 되는 강제 값 대리자입니다. 자세한 내용은 참조 [값 콜백을 Coerce](#coerce)합니다.
- A `Func` 기본 속성 값을 초기화 하는 데 사용 되는 합니다. 자세한 내용은 참조 [Default Value Func를 사용 하 여 만들기](#defaultfunc)합니다.

### <a name="creating-accessors"></a>접근자 만들기

속성 접근자는 바인딩 가능한 속성에 액세스 하려면 속성 구문을 사용 해야 합니다. `Get` 접근자 바인딩 가능한 해당 속성에 포함 된 값을 반환 해야 합니다. 이 작업을 호출 하 여 수행할 수는 [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) 메서드는 값을 가져올에 바인딩 가능한 속성 식별자에 전달 하 고 다음 결과 필요한 형식으로 캐스팅 합니다. `Set` 접근자는 해당 바인딩 가능한 속성의 값을 설정 해야 합니다. 이 작업을 호출 하 여 수행할 수는 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) 설정 값 및 값을 설정 하는 바인딩 가능한 속성 식별자에 전달 하는 메서드.

다음 코드 예제에 대 한 접근자는 `EventName` 바인딩 가능한 속성:

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### <a name="consuming-a-bindable-property"></a>바인딩 가능한 속성 사용

바인딩 가능한 속성을 만든 후에 XAML 또는 코드에서 사용할 수 있습니다. XAML 네임 스페이스 선언 나타내는 CLR 네임 스페이스 이름 및 필요에 따라 어셈블리 이름으로는 접두사와 네임 스페이스를 선언 하 여 작업을 수행 합니다. 자세한 내용은 참조 [XAML 네임 스페이스](~/xamarin-forms/xaml/namespaces.md)합니다.

다음 코드 예제에서는 사용자 지정 유형을 참조 하는 응용 프로그램 코드와 동일한 어셈블리 내에 정의 된 바인딩 가능한 속성을 포함 하는 사용자 지정 형식에 대 한 XAML 네임 스페이스를 보여 줍니다.

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

네임 스페이스 선언을 설정할 때 사용 되는 `EventName` 으로 설명한 다음 XAML 코드 예제에서 바인딩 가능한 속성:

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

만들 때 한 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 인스턴스를 바인딩 가능한 속성을 고급 시나리오를 사용 하도록 설정할 수 있는 선택적 매개 변수는 여러 가지가 있습니다. 이 섹션에는 이러한 시나리오 탐색합니다.

<a name="propertychanges" />

### <a name="detecting-property-changes"></a>속성 변경 내용 감지

A `static` 속성 변경 콜백 메서드를 지정 하 여 바인딩 가능한 속성에 등록할 수는 `propertyChanged` 에 대 한 매개 변수는 [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) 메서드. 지정된 된 콜백 메서드는 바인딩 가능한 속성의 값이 변경 될 때 호출 됩니다.

다음 코드 예제는 방법을 `EventName` 바인딩 가능 속성 레지스터는 `OnEventNameChanged` 속성 변경 콜백 메서드로 메서드:

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

속성 변경 콜백 메서드에서 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) 를 소유 하는 클래스의 인스턴스를 변경 하 고 두 값을 보고 했습니다 나타내는 매개 변수는 사용 `object` 이전 및 새 값의 매개 변수를 나타냅니다 바인딩 가능한 속성입니다.

<a name="validation" />

### <a name="validation-callbacks"></a>유효성 검사 콜백

A `static` 지정 하 여 바인딩 가능한 속성에 유효성 검사 콜백 메서드를 등록할 수 있습니다는 `validateValue` 에 대 한 매개 변수는 [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) 메서드. 지정된 된 콜백 메서드는 바인딩 가능한 속성의 값을 설정 하면 호출 됩니다.

다음 코드 예제는 방법을 `Angle` 바인딩 가능 속성 레지스터는 `IsValidValue` 메서드를 유효성 검사 콜백 메서드로:

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

유효성 검사 콜백은 값과 함께 제공 되 고 반환 해야 `true` 그렇지 않으면 값이 속성에 대 한 유효한 경우 `false`합니다. 유효성 검사 콜백을 반환 하는 경우 예외가 발생 합니다 `false`, 개발자가 처리 해야 합니다. 바인딩 가능한 속성을 설정 유효성 검사 콜백 메서드를 사용 하는 일반적인 integer 또는 double 값을 제한 됩니다. 예를 들어는 `IsValidValue` 메서드는 속성 값이 있는지 확인 한 `double` 0-360 사이입니다.

<a name="coerce" />

### <a name="coerce-value-callbacks"></a>강제 값 콜백

A `static` 강제 값을 지정 하 여 바인딩 가능한 속성에 콜백 메서드를 등록할 수 있습니다는 `coerceValue` 에 대 한 매개 변수는 [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) 메서드. 지정된 된 콜백 메서드는 바인딩 가능한 속성의 값이 변경 될 때 호출 됩니다.

강제 값 콜백을 속성의 값이 변경 될 때 바인딩 가능한 속성을 다시 평가 강제로 실행 하는 데 사용 됩니다. 예를 들어 강제 값 콜백이 하나의 바인딩 가능한 속성의 값이 다른 바인딩 가능한 속성의 값 보다 큰 인지 확인 데 사용할 수 있습니다.

다음 코드 예제는 방법을 `Angle` 바인딩 가능 속성 레지스터는 `CoerceAngle` 강제 값 콜백 메서드로 메서드:

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

`CoerceAngle` 의 값을 확인 하는 메서드는 `MaximumAngle` 속성을 쓰고는 `Angle` 속성 값이 보다 큰 값으로 강제 변환는 `MaximumAngle` 속성 값입니다.

<a name="defaultfunc" />

### <a name="creating-a-default-value-with-a-func"></a>기본값을 Func를 사용 하 여 만들기

A `Func` 다음 코드 예제에서와 같이 사용을 바인딩 가능한 속성의 기본값을 초기화할 수 있습니다.

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator` 매개 변수가 설정 되는 `Func` 를 호출 하는 [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) 반환 하는 메서드는 `double` 에 사용 되는 글꼴에 대 한 명명 된 크기를 나타내는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 네이티브 플랫폼에 있습니다.

## <a name="summary"></a>요약

이 문서는 바인딩 가능한 속성에 대 한 소개를 제공 하 고 만들고 하 게 사용 하는 방법을 살펴보았습니다. 바인딩 가능한 속성은 속성의 값 Xamarin.Forms 속성 시스템에 의해 추적 되는 위치는 특수 한 유형의 속성.


## <a name="related-links"></a>관련 링크

- [XAML 네임스페이스](~/xamarin-forms/xaml/namespaces.md)
- [이벤트: 명령 동작 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [유효성 검사 콜백을 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/xaml/validationcallback/)
- [Coerce 값 콜백을 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/xaml/coercevaluecallback/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
