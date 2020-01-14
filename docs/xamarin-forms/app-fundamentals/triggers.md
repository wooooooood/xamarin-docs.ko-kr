---
title: Xamarin.Forms 트리거
description: 이 문서에서는 Xamarin.Forms 트리거를 사용하여 XAML에서 사용자 인터페이스 변경에 응답하는 방법을 설명합니다. 트리거를 사용하면 XAML에서 이벤트 또는 속성 변경에 따라 컨트롤의 모양을 변경하는 작업을 선언적으로 표현할 수 있습니다.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 056bb16c76887661f054422b2c682a91e6bfa466
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489897"
---
# <a name="xamarinforms-triggers"></a>Xamarin.Forms 트리거

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithtriggers)

트리거를 사용하면 XAML에서 이벤트 또는 속성 변경에 따라 컨트롤의 모양을 변경하는 작업을 선언적으로 표현할 수 있습니다.

트리거를 컨트롤에 직접 할당하거나 여러 컨트롤에 적용될 페이지 수준 또는 애플리케이션 수준 리소스 사전에 추가할 수 있습니다.

트리거에는 다음 네 가지 유형이 있습니다.

- [속성 트리거](#property) - 컨트롤의 속성이 특정 값으로 설정될 때 발생합니다.

- [데이터 트리거](#data) - 데이터 바인딩을 사용하여 다른 컨트롤의 속성을 기반으로 하여 트리거합니다.

- [이벤트 트리거](#event) - 컨트롤에서 이벤트가 발생할 때 발생합니다.

- [다중 트리거](#multi) - 작업이 발생하기 전에 여러 트리거 조건을 설정할 수 있도록 합니다.

<a name="property" />

## <a name="property-triggers"></a>속성 트리거

간단한 트리거는 컨트롤의 트리거 컬렉션에 `Trigger` 요소를 추가하는 XAML에서만 표현할 수 있습니다.
다음 예제에서는 포커스를 받을 때 `Entry` 배경색을 변경하는 트리거를 보여 줍니다.

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
            <!-- multiple Setters elements are allowed -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

트리거 선언에서 중요한 부분은 다음과 같습니다.

- **TargetType** - 트리거가 적용되는 컨트롤 형식입니다.

- **Property** - 모니터링되는 컨트롤의 속성입니다.

- **Value** - 모니터링되는 속성에 대해 발생할 때 트리거가 활성화되는 값입니다.

- **Setter** - 트리거 조건이 충족될 때 `Setter` 요소의 컬렉션을 추가할 수 있습니다. 설정할 `Property` 및 `Value`를 지정합니다.

- **EnterActions 및 ExitActions**(표시되지 않음) - 코드로 작성되며 `Setter` 요소에 추가하여(또는 대신) 사용할 수 있습니다. 이러한 모든 부분은 [아래와 같이 설명됩니다](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Style(스타일)을 사용하여 Trigger(트리거) 적용

트리거는 컨트롤, 페이지 또는 `ResourceDictionary`애플리케이션의 `Style` 선언에도 추가할 수 있습니다. 다음 예제에서는 페이지의 모든 `Entry` 컨트롤에 적용된다고 나타내는 암시적 스타일(즉, `Key`가 설정되지 않음)을 선언합니다.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                    <!-- multiple Setters elements are allowed -->
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

<a name="data" />

## <a name="data-triggers"></a>데이터 트리거

데이터 트리거는 다른 컨트롤을 모니터링하는 데이터 바인딩을 사용하여 `Setter`가 호출되도록 합니다. 속성 트리거의 `Property` 특성 대신 지정된 값을 모니터링하는 `Binding` 특성을 설정합니다.

다음 예제에서는 다른 컨트롤의 속성을 참조하는 `{Binding Source={x:Reference entry}, Path=Text.Length}` 데이터 바인딩 구문을
사용합니다. `entry`의 길이가 0이면 트리거가 활성화됩니다. 이 샘플에서 트리거는 입력이 비어 있을 때 단추를 비활성화합니다.

```xaml
<!-- the x:Name is referenced below in DataTrigger-->
<!-- tip: make sure to set the Text="" (or some other default) -->
<Entry x:Name="entry"
       Text=""
       Placeholder="required field" />

<Button x:Name="button" Text="Save"
        FontSize="Large"
        HorizontalOptions="Center">
    <Button.Triggers>
        <DataTrigger TargetType="Button"
                     Binding="{Binding Source={x:Reference entry},
                                       Path=Text.Length}"
                     Value="0">
            <Setter Property="IsEnabled" Value="False" />
            <!-- multiple Setters elements are allowed -->
        </DataTrigger>
    </Button.Triggers>
</Button>
```

> [!TIP]
> `Path=Text.Length`를 평가할 때 항상 대상 속성(예:`Text=""`)에 기본값을 제공합니다. 그렇지 않으면 `null`이 되고 트리거가 예상대로 작동하지 않기 때문입니다.

`Setter`를 지정하는 것 외에도 [`EnterActions` 및 `ExitActions`](#enterexit)도 제공할 수 있습니다.

<a name="event" />

## <a name="event-triggers"></a>이벤트 트리거

`EventTrigger` 요소에는 아래 예제의 `"Clicked"`와 같이 `Event` 속성만 필요합니다.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

`Setter` 요소는 없고 오히려 `local:NumericValidationTriggerAction`에서 정의된 클래스에 대한 참조가 있음에 유의하세요. 이 클래스는 페이지의 XAML에서 `xmlns:local`을 선언해야 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

클래스 자체는 `TriggerAction`을 구현합니다. 즉 트리거 이벤트가 발생할 때마다 호출되는 `Invoke` 메서드에 대한 재정의를 제공해야 합니다.

트리거 작업 구현에서 수행해야 하는 작업은 다음과 같습니다.

- 트리거가 적용될 컨트롤 형식에 해당하는 제네릭 매개 변수를 사용하여 제네릭 `TriggerAction<T>` 클래스를 구현합니다. `VisualElement`와 같은 수퍼클래스를 사용하여 다양한 컨트롤에서 작동하는 트리거 작업을 작성하거나 `Entry`와 같은 컨트롤 형식을 지정할 수 있습니다.

- 트리거 조건이 충족될 때마다 호출되는 `Invoke` 메서드를 재정의합니다.

- 필요에 따라 트리거를 선언할 때 XAML에서 설정할 수 있는 속성을 공개합니다. 이에 대한 예제는 함께 제공되는 애플리케이션 예제의 `VisualElementPopTriggerAction` 클래스를 참조하세요.

```csharp
public class NumericValidationTriggerAction : TriggerAction<Entry>
{
    protected override void Invoke (Entry entry)
    {
        double result;
        bool isValid = Double.TryParse (entry.Text, out result);
        entry.TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

그런 다음 XAML에서 이벤트 트리거를 사용할 수 있습니다.

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

`ResourceDictionary`에서 트리거를 공유할 때 주의하세요. 하나의 인스턴스가 컨트롤 간에 공유되므로 한 번 구성된 상태가 모든 컨트롤에 적용됩니다.

이벤트 트리거는 [아래에서 설명하는](#enterexit) `EnterActions` 및 `ExitActions`를 지원하지 않습니다.

<a name="multi" />

## <a name="multi-triggers"></a>다중 트리거

`MultiTrigger`는 둘 이상의 조건이 있을 수 있다는 점을 제외하고는 `Trigger` 또는 `DataTrigger`와 비슷합니다. `Setter`가 트리거되기 전에 모든 조건이 true여야 합니다.

두 개의 서로 다른 입력(`email` 및 `phone`)에 바인딩되는 단추 트리거의 예제는 다음과 같습니다.

```xaml
<MultiTrigger TargetType="Button">
    <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference email},
                                   Path=Text.Length}"
                               Value="0" />
        <BindingCondition Binding="{Binding Source={x:Reference phone},
                                   Path=Text.Length}"
                               Value="0" />
    </MultiTrigger.Conditions>
    <Setter Property="IsEnabled" Value="False" />
    <!-- multiple Setter elements are allowed -->
</MultiTrigger>
```

`Conditions` 컬렉션에도 다음과 같은 `PropertyCondition` 요소가 포함될 수 있습니다.

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>"모두가 필수인" 다중 트리거 작성

모든 조건이 true인 경우에만 다중 트리거에서 컨트롤을 업데이트합니다. "영(0)보다 큰 Text.Length" 조건이 필요하지만 XAML에서는 표시할 수 없으므로 "모든 필드의 길이가 영(0)임"(예: 모든 입력이 완료되어야 하는 로그인 페이지)에 대한 테스트는 까다롭습니다.

이 작업은 `IValueConverter`를 사용하여 수행할 수 있습니다. 아래의 변환기 코드는 `Text.Length` 바인딩을 필드가 비어 있는지 여부를 나타내는 `bool`로 변환합니다.

```csharp
public class MultiTriggerConverter : IValueConverter
{
    public object Convert(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        if ((int)value > 0) // length > 0 ?
            return true;            // some data has been entered
        else
            return false;            // input is empty
    }

    public object ConvertBack(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        throw new NotSupportedException ();
    }
}
```

다중 트리거에서 이 변환기를 사용하려면 먼저 사용자 지정 `xmlns:local` 네임스페이스 정의와 함께 해당 변환기를 페이지의 리소스 사전에 추가합니다.

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

XAML은 아래와 같습니다. 첫 번째 다중 트리거 예제와의 차이점은 다음과 같습니다.

- 단추는 기본적으로 `IsEnabled="false"`로 설정되어 있습니다.
- 다중 트리거 조건은 변환기를 사용하여 `Text.Length` 값을 `boolean`으로 변환합니다.
- 모든 조건이 `true`이면 setter에서 단추의 `IsEnabled` 속성을 `true`로 설정합니다.

```xaml
<Entry x:Name="user" Text="" Placeholder="user name" />

<Entry x:Name="pwd" Text="" Placeholder="password" />

<Button x:Name="loginButton" Text="Login"
        FontSize="Large"
        HorizontalOptions="Center"
        IsEnabled="false">
  <Button.Triggers>
    <MultiTrigger TargetType="Button">
      <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference user},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
        <BindingCondition Binding="{Binding Source={x:Reference pwd},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
      </MultiTrigger.Conditions>
      <Setter Property="IsEnabled" Value="True" />
    </MultiTrigger>
  </Button.Triggers>
</Button>
```

다음 스크린샷에서는 위의 두 가지 다중 트리거 예제 사이의 차이점을 보여 줍니다. 화면의 위쪽에서는 `Entry`의 텍스트 입력만으로 **저장** 단추를 사용할 수 있습니다.
화면의 아래쪽에서는 두 필드 모두에 데이터가 포함될 때까지 **로그인** 단추가 비활성 상태로 유지됩니다.

![](triggers-images/multi-requireall.png "MultiTrigger Examples")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>EnterActions 및 ExitActions

트리거가 발생할 때 변경 내용을 구현하는 또 다른 방법은 `EnterActions` 및 `ExitActions` 컬렉션을 추가하고 `TriggerAction<T>` 구현을 지정하는 것입니다.

[`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions) 컬렉션은 트리거 조건이 충족될 때 호출되는 [`TriggerAction`](xref:Xamarin.Forms.TriggerAction) 개체의 `IList`를 정의하는 데 사용됩니다. [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions) 컬렉션은 트리거 조건이 더 이상 충족되지 않을 때 호출되는 `TriggerAction` 개체의 `IList`를 정의하는 데 사용됩니다.

> [!NOTE]
> `EnterActions` 및 `ExitActions` 컬렉션에 정의된 [`TriggerAction`](xref:Xamarin.Forms.TriggerAction) 개체는 [`EventTrigger`](xref:Xamarin.Forms.EventTrigger) 클래스에 의해 무시됩니다.    

트리거에서 `Setter`뿐만 아니라 `EnterActions` 및 `ExitActions`도 *모두* 제공할 수 있지만, `Setter`이(가) 즉시 호출된다는 점을 주의하세요(`EnterAction` 또는 `ExitAction`이 완료될 때까지 기다리지 않음). 또는 코드에 있는 모든 작업을 수행할 수 있으며 `Setter`는 전혀 사용하지 않습니다.

```xaml
<Entry Placeholder="enter job title">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="Entry.IsFocused" Value="True">
            <Trigger.EnterActions>
                <local:FadeTriggerAction StartsFrom="0"" />
            </Trigger.EnterActions>

            <Trigger.ExitActions>
                <local:FadeTriggerAction StartsFrom="1" />
            </Trigger.ExitActions>
            <!-- You can use both Enter/Exit and Setter together if required -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

항상 그렇듯이 XAML에서 클래스가 참조되는 경우 다음과 같이 `xmlns:local`과 같은 네임스페이스를 선언해야 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

`FadeTriggerAction` 코드는 아래와 같습니다.

```csharp
public class FadeTriggerAction : TriggerAction<VisualElement>
{
    public int StartsFrom { set; get; }

    protected override void Invoke(VisualElement sender)
    {
        sender.Animate("FadeTriggerAction", new Animation((d) =>
        {
            var val = StartsFrom == 1 ? d : 1 - d;
            // so i was aiming for a different color, but then i liked the pink :)
            sender.BackgroundColor = Color.FromRgb(1, val, 1);
        }),
        length: 1000, // milliseconds
        easing: Easing.Linear);
    }
}
```

## <a name="related-links"></a>관련 링크

- [트리거 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithtriggers)
- [Xamarin.Forms API 설명서](xref:Xamarin.Forms.TriggerAction`1)
