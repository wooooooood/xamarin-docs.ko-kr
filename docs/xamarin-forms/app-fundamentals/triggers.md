---
title: Xamarin.Forms 트리거
description: 이 문서에서는 Xamarin.Forms 트리거를 사용 하 여 XAML 사용 하 여 사용자 인터페이스 변경에 응답 하는 방법에 설명 합니다. 트리거를 사용 하면 이벤트 또는 속성 변경 내용을 기반으로 하는 컨트롤의 모양을 변경 하는 XAML에서 선언적으로 작업을 표현할 수 있습니다.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 954a0967e034e0321964e12ca0725ae2a85e3bc6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995539"
---
# <a name="xamarinforms-triggers"></a>Xamarin.Forms 트리거

트리거를 사용 하면 이벤트 또는 속성 변경 내용을 기반으로 하는 컨트롤의 모양을 변경 하는 XAML에서 선언적으로 작업을 표현할 수 있습니다.

트리거를 직접 컨트롤을 할당할 수도 있고 여러 컨트롤에 적용할 페이지 수준 또는 응용 프로그램 수준 리소스 사전에 추가할 수 있습니다.

네 가지 유형의 트리거는

* [속성 트리거](#property) -컨트롤의 속성은 특정 값으로 설정 된 경우 발생 합니다.

* [데이터 트리거](#data) -다른 컨트롤의 속성을 기반으로 하는 트리거를 바인딩할 데이터를 사용 합니다.

* [이벤트 트리거](#event) -컨트롤에서 이벤트가 발생할 때 발생 합니다.

* [다중 트리거](#multi) -여러 트리거 조건은 작업이 발생 하기 전에 설정할 수 있습니다.

<a name="property" />

## <a name="property-triggers"></a>속성 트리거

간단한 트리거가, XAML에서 순수 하 게 표현할 수 있는 추가 `Trigger` 컨트롤의 요소 컬렉션을 트리거합니다.
이 예제에서는 변경 하는 트리거를 `Entry` 배경색 키를 누른 채 포커스를 받을 때:

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
             Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
        </Trigger>
    </Entry.Triggers>
</Entry>
```

트리거의 선언의 중요 한 부분은 다음과 같습니다.

* **TargetType** -트리거가 적용 되는 컨트롤 형식입니다.

* **속성** -모니터링 되는 컨트롤의 속성입니다.

* **값** -모니터링 되는 속성에 대 한 경우 값을 되도록 트리거를 활성화 합니다.

* **Setter** -컬렉션 `Setter` 트리거 조건이 충족 되는 경우 및 요소를 추가할 수 있습니다. 지정 해야 합니다 `Property` 및 `Value` 설정 합니다.

* **EnterActions 및 ExitActions** (표시 되지 않음)-코드 작성 되 고 외에 (또는 instead of)에 사용할 수 있습니다 `Setter` 요소입니다. 이들은 [아래에 설명 된](#enterexit)합니다.

### <a name="applying-a-trigger-using-a-style"></a>스타일을 사용 하는 트리거를 적용 합니다.

트리거를 추가할 수도 있습니다는 `Style` 페이지나 응용 프로그램에서 컨트롤을 선언 `ResourceDictionary`합니다. 이 예제에서는 암시적 스타일을 선언 (ie. 없습니다 `Key` 설정할지) 모두에 적용 되는 의미 `Entry` 페이지의 컨트롤입니다.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

<a name="data" />

## <a name="data-triggers"></a>데이터 트리거

데이터 트리거 데이터 바인딩을 사용 하 여 다른 컨트롤을 모니터링 합니다 `Setter`호출 하도록 합니다. 대신 합니다 `Property` 속성 트리거 특성을 설정 합니다 `Binding` 특성에 지정 된 값에 대 한 모니터링을 합니다.

아래 예제에서는 데이터 바인딩 구문을 사용 하 여 `{Binding Source={x:Reference entry}, Path=Text.Length}` 다른 컨트롤의 속성을 참조 하는 방법입니다. 때의 길이 `entry` 가 0 이면 트리거는 활성화 됩니다. 이 샘플에서 트리거 입력이 비어 있을 때 단추를 사용 하지 않습니다.

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
        </DataTrigger>
    </Button.Triggers>
</Button>
```

팁: 평가할 때 `Path=Text.Length` (예: 대상 속성에 대 한 기본값을 항상 제공 `Text=""`) 하기 때문에 그렇지 않으면 `null` 것 같은 트리거가 작동 하지 않습니다.

지정 하는 것 외에도 `Setter`s를 제공할 수도 있습니다 [ `EnterActions` 하 고 `ExitActions` ](#enterexit)합니다.

<a name="event" />

## <a name="event-triggers"></a>이벤트 트리거

`EventTrigger` 요소에만 필요는 `Event` 속성을 같은 `"Clicked"` 아래 예제에서입니다.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

있으며 없습니다 `Setter` 요소 있지만 정의한 클래스에 대 한 참조 대신 `local:NumericValidationTriggerAction` 필요는 `xmlns:local` 의 XAML 페이지에 선언 해야:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

클래스 자체 구현 `TriggerAction` 즉에 대 한 재정의 제공 해야는 `Invoke` 트리거 이벤트가 발생할 때마다 호출 되는 메서드입니다.

트리거 동작 구현 해야 합니다.

* 제네릭 구현 `TriggerAction<T>` 트리거 적용할 컨트롤의 형식과 해당 제네릭 매개 변수를 사용 하 여 클래스입니다. 와 같은 슈퍼 클래스를 사용할 수 있습니다 `VisualElement` 다양 한 컨트롤을 사용 하거나 컨트롤 종류를 지정 하는 트리거 동작을 쓸 `Entry`합니다.

* 재정의 `Invoke` -이 메서드는 트리거 조건이 충족 합니다.

* 필요에 따라 트리거를 선언할 때는 XAML에서 설정할 수 있는 속성을 노출 (같은 `Anchor`, `Scale`, 및 `Length` 이 예제의).

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

트리거 동작에 의해 노출 되는 속성 XAML 선언에 다음과 같이 설정할 수 있습니다.

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

트리거를 공유 하는 경우 주의 `ResourceDictionary`를 한 번만 구성 된 모든 상태 모두에 적용 됩니다 있도록 하나의 인스턴스만 컨트롤 간에 공유 됩니다.

이벤트 트리거를 지원 하지 않습니다 `EnterActions` 하 고 `ExitActions` [아래에 설명 된](#enterexit)합니다.    

<a name="multi" />

## <a name="multi-triggers"></a>다중 트리거

A `MultiTrigger` 비슷합니다는 `Trigger` 또는 `DataTrigger` 제외 조건을 둘 이상 있을 수 있습니다. 모든 조건이 하기 전에 충족 해야 합니다.는 `Setter`s 트리거됩니다.

다음은 두 개의 다른 입력에 바인딩되는 단추에 대 한 트리거의 예 (`email` 고 `phone`):

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

합니다 `Conditions` 컬렉션을 포함할 수 있습니다 `PropertyCondition` 이 같은 요소:

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>"모든 필요한" 다중 트리거를 작성합니다.

다중 트리거는 모든 조건이 true 인 경우 해당 컨트롤을 업데이트 합니다. "모든 필드는 길이가 0" (예: 여기서 모든 입력을 완료 해야 하는 로그인 페이지)에 대 한 테스트는 조건 때문에 까다로울 "여기서 Text.Length > 0" 하지만이 XAML에서 표현할 수 없습니다.

사용 하 여이 작업을 수행할 수 있습니다는 `IValueConverter`합니다. 변환 아래 변환기 코드를 `Text.Length` 에 바인딩는 `bool` 필드를 빈 인지 여부를 나타내는:


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

이 변환기에서 다중 트리거를 사용 하려면 먼저 사전에 추가 페이지의 리소스 (사용자 지정 함께 `xmlns:local` 네임 스페이스 정의):

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

XAML은 다음과 같습니다. 첫 번째 다중 트리거 예제에서 다음과 같은 차이점이 note:

* 단추가 `IsEnabled="false"` 기본적으로 설정 합니다.
* 다중 트리거 조건을 변환기를 사용 하 여 설정 하는 `Text.Length` 부울 값입니다.
* 모든 조건이 때 `true`, setter는 단추의 `IsEnabled` 속성 `true`합니다.

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

이러한 스크린샷은 위의 두 가지 다중 트리거 예제 간의 차이 보여 줍니다. 화면 위쪽에 텍스트 입력에서 하나만 `Entry` 사용 하기에 충분 합니다 **저장** 단추.
화면 아래쪽에는 **로그인** 단추 비활성 상태로 유지 될 두 필드에 데이터가 포함 될 때까지 합니다.


![](triggers-images/multi-requireall.png "MultiTrigger 예제")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>EnterActions 및 ExitActions

추가 하 여 트리거를 발생 하는 경우 변경 내용을 구현 하는 또 다른 방법은 것 `EnterActions` 하 고 `ExitActions` 컬렉션과 지정 `TriggerAction<T>` 구현 합니다.

제공할 수 있습니다 *둘 다* `EnterActions` 및 `ExitActions` 뿐만 `Setter`트리거에서 있지만 주의를 `Setter`s 즉시 호출 되 (에 대 한 대기 하지 않도록 합니다 `EnterAction` 또는 `ExitAction` 를 완성). 모든 코드에서 수행 하 고 사용 하지 수 또는 `Setter`전혀 s입니다.

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

네임 스페이스와 같은 선언 해야 클래스를 XAML에서 참조할 때에 언제 든 지 대로 `xmlns:local` 다음과 같이 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

`FadeTriggerAction` 코드는 다음과 같습니다.

```csharp
public class FadeTriggerAction : TriggerAction<VisualElement>
{
    public FadeTriggerAction() {}

    public int StartsFrom { set; get; }

    protected override void Invoke (VisualElement visual)
    {
            visual.Animate("", new Animation( (d)=>{
                var val = StartsFrom==1 ? d : 1-d;
                visual.BackgroundColor = Color.FromRgb(1, val, 1);

            }),
            length:1000, // milliseconds
            easing: Easing.Linear);
    }
}
```

참고: `EnterActions` 하 고 `ExitActions` 가 무시 됩니다 **이벤트 트리거**합니다.



## <a name="related-links"></a>관련 링크

- [트리거 샘플](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Xamarin.Forms API 설명서](xref:Xamarin.Forms.TriggerAction`1)
