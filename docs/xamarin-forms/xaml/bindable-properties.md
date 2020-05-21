---
title: Xamarin.ios 바인딩 가능 속성
description: 이 문서에서는 바인딩 가능한 속성에 대해 소개 하 고 이러한 속성을 만들고 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: 4151ac6f8cd9d860251ce1f27c7b342e0caa465c
ms.sourcegitcommit: bc0c1740aa0708459729c0e671ab3ff7de3e2eee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/15/2020
ms.locfileid: "83425782"
---
# <a name="xamarinforms-bindable-properties"></a>Xamarin.ios 바인딩 가능 속성

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)

바인딩 가능한 속성은 필드를 사용 하 여 속성을 백업 하는 대신 형식으로 속성을 백업 하 여 CLR 속성 기능을 확장 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 합니다. 바인딩 가능한 속성의 목적은 부모-자식 관계를 통해 설정 된 데이터 바인딩, 스타일, 템플릿 및 값을 지 원하는 속성 시스템을 제공 하는 것입니다. 또한 바인딩 가능한 속성은 기본값, 속성 값의 유효성 검사 및 속성 변경을 모니터링 하는 콜백을 제공할 수 있습니다.

속성은 다음 기능 중 하나 이상을 지원 하기 위해 바인딩 가능한 속성으로 구현 되어야 합니다.

- 데이터 바인딩의 유효한 *대상* 속성으로 작동 합니다.
- [스타일](~/xamarin-forms/user-interface/styles/index.md)을 통해 속성을 설정 합니다.
- 속성의 형식에 대 한 기본값과 다른 기본 속성 값을 제공 합니다.
- 속성 값의 유효성을 검사 합니다.
- 속성 변경 내용을 모니터링 합니다.

Xamarin.ios 바인딩 가능 속성의 예로는 [`Label.Text`](xref:Xamarin.Forms.Label.Text) , [`Button.BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius) 및가 [`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) 있습니다. 바인딩 가능한 각 속성에는 `public static readonly` [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 동일한 클래스에서 노출 되는 형식의 해당 필드와 바인딩 가능한 속성의 식별자가 있습니다. 예를 들어 속성에 대 한 해당 바인딩 가능한 속성 식별자는 `Label.Text` [`Label.TextProperty`](xref:Xamarin.Forms.Label.TextProperty) 입니다.

## <a name="create-a-bindable-property"></a>바인딩 가능한 속성 만들기

바인딩 가능한 속성을 만드는 프로세스는 다음과 같습니다.

1. [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)메서드 오버 로드 중 하나를 사용 하 여 인스턴스를 만듭니다 [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create*) .
1. 인스턴스에 대 한 속성 접근자를 정의 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 합니다.

모든 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 인스턴스는 UI 스레드에서 만들어야 합니다. 즉, UI 스레드에서 실행 되는 코드만 바인딩 가능한 속성의 값을 가져오거나 설정할 수 있습니다. 그러나 메서드를 사용 하 여 `BindableProperty` UI 스레드로 마샬링하기 때문에 다른 스레드에서 인스턴스를 액세스할 수 있습니다 [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) .

### <a name="create-a-property"></a>속성 만들기

인스턴스를 만들려면 `BindableProperty` 포함 하는 클래스가 클래스에서 파생 되어야 합니다 [`BindableObject`](xref:Xamarin.Forms.BindableObject) . 그러나 클래스는 클래스 `BindableObject` 계층 구조의 상위 클래스 이므로 사용자 인터페이스 기능에 사용 되는 대부분의 클래스는 바인딩 가능한 속성을 지원 합니다.

바인딩 가능한 속성은 형식의 속성을 선언 하 여 만들 수 있습니다 `public static readonly` [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 바인딩 가능한 속성은 메서드 오버 로드 중 하나의 반환 된 값으로 설정 되어야 합니다 [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) . 선언은 파생 클래스의 본문 내에 [`BindableObject`](xref:Xamarin.Forms.BindableObject) 있지만 멤버 정의 외부에 있어야 합니다.

최소한 다음 매개 변수와 함께를 만들 때 식별자를 지정 해야 합니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) .

- 의 이름 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 입니다.
- 속성의 형식입니다.
- 소유 하는 개체의 형식입니다.
- 속성의 기본값입니다. 이렇게 하면 속성이 설정 되지 않은 경우 속성은 항상 특정 기본값을 반환 하 고 속성의 형식에 대 한 기본값과 다를 수 있습니다. [`ClearValue`](xref:Xamarin.Forms.BindableObject.ClearValue(Xamarin.Forms.BindableProperty))바인딩 가능한 속성에 대해 메서드를 호출 하면 기본값이 복원 됩니다.

다음 코드에서는 네 개의 필수 매개 변수에 대 한 식별자 및 값을 사용 하 여 바인딩 가능한 속성의 예를 보여 줍니다.

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

그러면 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 형식의 이라는 인스턴스가 만들어집니다 `EventName` `string` . 속성은 클래스에 의해 소유 `EventToCommandBehavior` 되며 기본값은 `null` 입니다. 바인딩 가능한 속성에 대 한 명명 규칙은 바인딩 가능한 속성 식별자가 메서드에 지정 된 속성 이름과 일치 해야 한다는 것입니다 `Create` . 여기에는 "property"가 추가 됩니다. 따라서 위의 예제에서 바인딩 가능한 속성 식별자는 `EventNameProperty` 입니다.

필요에 따라 인스턴스를 만들 때 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 다음 매개 변수를 지정할 수 있습니다.

- 바인딩 모드입니다. 이는 속성 값 변경이 전파 되는 방향을 지정 하는 데 사용 됩니다. 기본 바인딩 모드에서 변경 내용은 *원본* 에서 *대상*으로 전파 됩니다.
- 속성 값이 설정 될 때 호출 되는 유효성 검사 대리자입니다. 자세한 내용은 [유효성 검사 콜백](#validation-callbacks)을 참조 하세요.
- 속성 값이 변경 될 때 호출 되는 속성 변경 대리자입니다. 자세한 내용은 [속성 변경 내용 검색](#detect-property-changes)을 참조 하세요.
- 속성 값이 변경 될 때 호출 되는 속성 변경 대리자입니다. 이 대리자에는 속성 변경 대리자와 동일한 시그니처가 있습니다.
- 속성 값이 변경 될 때 호출 되는 강제 값 대리자입니다. 자세한 내용은 [강제 값 콜백](#coerce-value-callbacks)을 참조 하세요.
- `Func`기본 속성 값을 초기화 하는 데 사용 되는입니다. 자세한 내용은 [Func를 사용 하 여 기본값 만들기](#create-a-default-value-with-a-func)를 참조 하세요.

### <a name="create-accessors"></a>접근자 만들기

속성 구문을 사용 하 여 바인딩 가능한 속성에 액세스 하려면 속성 접근자가 필요 합니다. `Get`접근자는 해당 하는 바인딩 가능 속성에 포함 된 값을 반환 해야 합니다. 이는 메서드를 호출 하 고 [`GetValue`](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) 값을 가져올 바인딩 가능한 속성 식별자를 전달한 다음 결과를 필요한 형식으로 캐스팅 하 여 수행할 수 있습니다. 접근자는 해당 하는 `Set` 바인딩 가능한 속성의 값을 설정 해야 합니다. 이는 메서드를 호출 하 고 [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) , 값을 설정할 바인딩 가능한 속성 식별자를 전달 하 고, 설정할 값을 전달 하 여 수행할 수 있습니다.

다음 코드 예제에서는 바인딩 가능한 속성에 대 한 접근자를 보여 줍니다 `EventName` .

```csharp
public string EventName
{
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

## <a name="consume-a-bindable-property"></a>바인딩 가능한 속성 사용

바인딩 가능한 속성을 만든 후에는 XAML 또는 코드에서 사용할 수 있습니다. XAML에서는 접두사를 사용 하 여 네임 스페이스를 선언 하 고, CLR 네임 스페이스 이름 및 선택적으로 어셈블리 이름을 나타내는 네임 스페이스 선언을 사용 하 여이를 구현 합니다. 자세한 내용은 [XAML 네임 스페이스](~/xamarin-forms/xaml/namespaces.md)를 참조 하세요.

다음 코드 예제에서는 사용자 지정 형식을 참조 하는 응용 프로그램 코드와 동일한 어셈블리 내에 정의 된 바인딩 가능한 속성을 포함 하는 사용자 지정 형식에 대 한 XAML 네임 스페이스를 보여 줍니다.

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

네임 스페이스 선언은 `EventName` 다음 XAML 코드 예제에 나와 있는 것 처럼 바인딩 가능한 속성을 설정할 때 사용 됩니다.

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

동등한 C# 코드는 다음 코드 예제와 같습니다.

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior
{
  EventName = "ItemSelected",
  ...
});
```

## <a name="advanced-scenarios"></a>고급 시나리오

인스턴스를 만들 때 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 고급 바인딩 가능 속성 시나리오를 사용 하도록 설정할 수 있는 여러 가지 선택적 매개 변수가 있습니다. 이 섹션에서는 이러한 시나리오를 살펴봅니다.

### <a name="detect-property-changes"></a>속성 변경 내용 검색

`static`속성 변경 콜백 메서드는 `propertyChanged` 메서드에 대 한 매개 변수를 지정 하 여 바인딩 가능한 속성을 사용 하 여 등록할 수 있습니다 [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) . 바인딩 가능한 속성 값이 변경 되 면 지정 된 콜백 메서드가 호출 됩니다.

다음 코드 예제에서는 `EventName` 바인딩 `OnEventNameChanged` 가능한 속성이 메서드를 속성 변경 콜백 메서드로 등록 하는 방법을 보여 줍니다.

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

속성 변경 콜백 메서드에서 [`BindableObject`](xref:Xamarin.Forms.BindableObject) 매개 변수를 사용 하 여 소유 하 고 있는 클래스의 인스턴스를 표시 하 고, 두 `object` 매개 변수의 값은 바인딩 가능한 속성의 이전 값과 새 값을 나타냅니다.

### <a name="validation-callbacks"></a>유효성 검사 콜백

`static` `validateValue` 메서드에 대 한 매개 변수를 지정 하 여 바인딩 가능한 속성을 사용 하 여 유효성 검사 콜백 메서드를 등록할 수 있습니다 [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) . 바인딩 가능한 속성 값이 설정 되 면 지정 된 콜백 메서드가 호출 됩니다.

다음 코드 예제에서는 `Angle` 바인딩 `IsValidValue` 가능한 속성이 메서드를 유효성 검사 콜백 메서드로 등록 하는 방법을 보여 줍니다.

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

유효성 검사 콜백은 값으로 제공 되며 `true` , 값이 속성에 대해 유효한 경우를 반환 하 고 그렇지 않으면를 반환 합니다 `false` . 유효성 검사 콜백이 반환 되는 경우 예외가 발생 합니다 `false` .이는 개발자가 처리 해야 합니다. 유효성 검사 콜백 메서드를 일반적으로 사용 하는 것은 바인딩 가능한 속성이 설정 될 때 정수 또는 double 값을 제약 하는 것입니다. 예를 들어 `IsValidValue` 메서드는 속성 값이 `double` 0 ~ 360 범위 내에 있는지 확인 합니다.

### <a name="coerce-value-callbacks"></a>강제 값 콜백

`static` `coerceValue` 메서드에 대 한 매개 변수를 지정 하 여 바인딩 가능한 속성을 사용 하 여 강제 값 콜백 메서드를 등록할 수 있습니다 [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) . 바인딩 가능한 속성 값이 변경 되 면 지정 된 콜백 메서드가 호출 됩니다.

> [!IMPORTANT]
> 형식에는 해당 `BindableObject` `CoerceValue` `BindableProperty` 강제 값 콜백을 호출 하 여 인수 값을 강제로 재평가 하기 위해 호출할 수 있는 메서드가 있습니다.

강제 값 콜백은 속성 값이 변경 될 때 바인딩 가능한 속성을 강제로 다시 평가 하는 데 사용 됩니다. 예를 들어, 강제 값 콜백을 사용 하 여 바인딩 가능한 속성의 값이 다른 바인딩 가능한 속성의 값 보다 크지 않은지 확인할 수 있습니다.

다음 코드 예제에서는 `Angle` 바인딩 `CoerceAngle` 가능한 속성이 메서드를 강제 값 콜백 메서드로 등록 하는 방법을 보여 줍니다.

```csharp
public static readonly BindableProperty AngleProperty = BindableProperty.Create (
  "Angle", typeof(double), typeof(HomePage), 0.0, coerceValue: CoerceAngle);
public static readonly BindableProperty MaximumAngleProperty = BindableProperty.Create (
  "MaximumAngle", typeof(double), typeof(HomePage), 360.0, propertyChanged: ForceCoerceValue);
...

static object CoerceAngle (BindableObject bindable, object value)
{
  var homePage = bindable as HomePage;
  double input = (double)value;

  if (input > homePage.MaximumAngle)
  {
    input = homePage.MaximumAngle;
  }
  return input;
}

static void ForceCoerceValue(BindableObject bindable, object oldValue, object newValue)
{
  bindable.CoerceValue(AngleProperty);
}
```

메서드는 속성 값을 `CoerceAngle` 확인 하 `MaximumAngle` 고, `Angle` 속성 값이 보다 큰 경우 값을 속성 값으로 강제 변환 합니다 `MaximumAngle` . 또한 속성이 변경 되 면 `MaximumAngle` `Angle` 메서드를 호출 하 여 속성에서 강제 값 콜백이 호출 됩니다 `CoerceValue` .

### <a name="create-a-default-value-with-a-func"></a>Func를 사용 하 여 기본값 만들기

`Func`다음 코드 예제에서 보여 주는 것 처럼을 사용 하 여 바인딩 가능한 속성의 기본값을 초기화할 수 있습니다.

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator`매개 변수는 `Func` [`Device.GetNamedSize`](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) `double` [`Label`](xref:Xamarin.Forms.Label) 네이티브 플랫폼의에서 사용 되는 글꼴의 명명 된 크기를 나타내는를 반환 하기 위해 메서드를 호출 하는로 설정 됩니다.

## <a name="related-links"></a>관련 링크

- [XAML 네임스페이스](~/xamarin-forms/xaml/namespaces.md)
- [명령 동작에 대 한 이벤트 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)
- [유효성 검사 콜백 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-validationcallback)
- [강제 값 콜백 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-coercevaluecallback)
- [BindableProperty API](xref:Xamarin.Forms.BindableProperty)
- [BindableObject API](xref:Xamarin.Forms.BindableObject)
