---
title: 재사용 가능한 EventToCommandBehavior
description: 동작을 사용 하 여 명령을와 상호 작용 하도록 설계 되지 않은 컨트롤 명령을 연관 시킬 수 있습니다. 이 문서는 이벤트가 발생할 때 명령을 호출 하려면 Xamarin.Forms 동작을 사용 하 여 보여줍니다.
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 530c47703d72a3d199a35dbf04f4a0b3851921b9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="reusable-eventtocommandbehavior"></a>재사용 가능한 EventToCommandBehavior

_동작을 사용 하 여 명령을와 상호 작용 하도록 설계 되지 않은 컨트롤 명령을 연관 시킬 수 있습니다. 이 문서는 이벤트가 발생할 때 명령을 호출 하려면 Xamarin.Forms 동작을 사용 하 여 보여줍니다._

## <a name="overview"></a>개요

`EventToCommandBehavior` 클래스에 대 한 응답으로 명령을 실행 하는 재사용 가능한 Xamarin.Forms 사용자 지정 동작은 *모든* 이벤트 발생 합니다. 기본적으로 이벤트 인수 이벤트 명령에 전달 될 하 고 필요에 따라 수에 대 한 변환 하 여 프로그램 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) 구현 합니다.

다음 동작 속성 동작을 사용 하도록 설정 되어야 합니다.

- **EventName** – 동작 수신 하는 이벤트의 이름입니다.
- **명령** – **ICommand** 를 실행할 수 있습니다. 동작을 찾으려고 시도 `ICommand` 인스턴스의 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 부모 요소 로부터 상속 될 수 있는 연결 된 컨트롤의 합니다.

다음과 같은 선택적 동작 속성을 설정할 수도 있습니다.

- **CommandParameter** –는 `object` 하는 명령에 전달 됩니다.
- **변환기** –는 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) 간에 전달 될 때 이벤트 인수 데이터의 형식을 변경 하는 구현 *소스* 및 *대상*바인딩 엔진에 의해 합니다.

## <a name="creating-the-behavior"></a>동작 만들기

`EventToCommandBehavior` 클래스에서 파생 되는 `BehaviorBase<T>` 클래스에서 파생 됩니다는 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) 클래스입니다. 용도 `BehaviorBase<T>` 필요로 하는 모든 Xamarin.Forms 동작에 대 한 기본 클래스를 제공 하는 클래스는 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 연결된 된 컨트롤에 설정할 동작의 합니다. 이렇게 하면 동작 바인딩할 하 고 실행할 수는 `ICommand` 에 지정 된는 `Command` 동작이 사용 된 속성입니다.

`BehaviorBase<T>` 클래스는 재정의할 수 있는 제공 [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) 설정 하는 메서드는 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 동작 및는 overridable [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)정리 하는 메서드는 `BindingContext`합니다. 이 클래스에 연결된 된 컨트롤에 대 한 참조를 저장 하는 또한는 `AssociatedObject` 속성입니다.

### <a name="implementing-bindable-properties"></a>바인딩 가능 속성을 구현합니다.

`EventToCommandBehavior` 클래스 정의 4 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 사용자를 실행 하는 인스턴스는 이벤트가 발생할 때 명령을 정의 합니다. 이러한 속성은 다음 코드 예제에 나와 있습니다.

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  public static readonly BindableProperty EventNameProperty =
    BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
  public static readonly BindableProperty CommandProperty =
    BindableProperty.Create ("Command", typeof(ICommand), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty CommandParameterProperty =
    BindableProperty.Create ("CommandParameter", typeof(object), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty InputConverterProperty =
    BindableProperty.Create ("Converter", typeof(IValueConverter), typeof(EventToCommandBehavior), null);

  public string EventName { ... }
  public ICommand Command { ... }
  public object CommandParameter { ... }
  public IValueConverter Converter { ...  }
  ...
}
```

때는 `EventToCommandBehavior` 클래스 사용 되는 `Command` 속성에 바인딩된 데이터 해야는 `ICommand` 에 정의 된 이벤트 발생에 대 한 응답에서 실행 하는 `EventName` 속성입니다. 동작 증명을 찾습니다는 `ICommand` 에 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 연결된 된 컨트롤의 합니다.

기본적으로 이벤트에 대 한 이벤트 인수는 명령에 전달 됩니다. 간에 전달 될 때이 데이터 선택적으로 변환 될 수 *소스* 및 *대상* 지정 하 여 바인딩 엔진에 의해는 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) 구현을으로 `Converter` 속성 값입니다. 지정 하 여 명령에 매개 변수를 전달 하 되 또는 `CommandParameter` 속성 값입니다.

### <a name="implementing-the-overrides"></a>재정의 구현합니다.

`EventToCommandBehavior` 재정의 [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) 및 [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) 의 메서드는 `BehaviorBase<T>` 다음 코드 예제와 같이 클래스:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  protected override void OnAttachedTo (View bindable)
  {
    base.OnAttachedTo (bindable);
    RegisterEvent (EventName);
  }

  protected override void OnDetachingFrom (View bindable)
  {
    DeregisterEvent (EventName);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) 메서드를 호출 하 여 설치를 수행는 `RegisterEvent` 값을 전달 하는 메서드는 `EventName` 속성을 매개 변수로 합니다. [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) 호출 하 여 정리를 수행 하는 메서드는 `DeregisterEvent` 값을 전달 하는 메서드는 `EventName` 속성을 매개 변수로 합니다.

### <a name="implementing-the-behavior-functionality"></a>동작 기능을 구현합니다.

동작의 목적은에 정의 된 명령을 실행 하는 `Command` 속성으로 정의 된 이벤트 발생에 대 한 응답에는 `EventName` 속성입니다. 핵심 동작 기능은 다음 코드 예제에 표시 됩니다.

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  void RegisterEvent (string name)
  {
    if (string.IsNullOrWhiteSpace (name)) {
      return;
    }

    EventInfo eventInfo = AssociatedObject.GetType ().GetRuntimeEvent (name);
    if (eventInfo == null) {
      throw new ArgumentException (string.Format ("EventToCommandBehavior: Can't register the '{0}' event.", EventName));
    }
    MethodInfo methodInfo = typeof(EventToCommandBehavior).GetTypeInfo ().GetDeclaredMethod ("OnEvent");
    eventHandler = methodInfo.CreateDelegate (eventInfo.EventHandlerType, this);
    eventInfo.AddEventHandler (AssociatedObject, eventHandler);
  }

  void OnEvent (object sender, object eventArgs)
  {
    if (Command == null) {
      return;
    }

    object resolvedParameter;
    if (CommandParameter != null) {
      resolvedParameter = CommandParameter;
    } else if (Converter != null) {
      resolvedParameter = Converter.Convert (eventArgs, typeof(object), null, null);
    } else {
      resolvedParameter = eventArgs;
    }       

    if (Command.CanExecute (resolvedParameter)) {
      Command.Execute (resolvedParameter);
    }
  }
  ...
}
```

`RegisterEvent` 메서드에 대 한 응답으로 실행 되는 `EventToCommandBehavior` 의 값을 수신 하는 컨트롤에 연결 되는 `EventName` 속성을 매개 변수로 합니다. 메서드는 다음에 정의 된 이벤트를 찾으려고 시도 `EventName` 연결 된 컨트롤에서 속성입니다. 이벤트가 있을 수 있는 하는 `OnEvent` 메서드가 이벤트 처리기 방법으로 등록 됩니다.

`OnEvent` 메서드에 정의 된 이벤트 발생에 대 한 응답으로 실행 되는 `EventName` 속성입니다. 에 `Command` 속성 참조는 유효한 `ICommand`에 전달할 매개 변수를 검색 하려고는 `ICommand` 다음과 같습니다:

- 경우는 `CommandParameter` 매개 변수를 정의 하는 속성, 검색 됩니다.
- 그렇지 않은 경우, 고 `Converter` 속성 정의 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) 구현에서 변환기가 실행 되 고 간에 전달 될 때 이벤트 인수 데이터 변환 *소스* 및 *대상* 바인딩 엔진에 의해 합니다.
- 그렇지 않으면 이벤트 인수는 매개 변수로 간주 됩니다.

데이터 바인딩된 `ICommand` 은 다음에 전달 되어 실행 매개 변수를 제공 하는 명령으로는 [ `CanExecute` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Command.CanExecute/p/System.Object/) 메서드 반환 `true`합니다.

여기서는 표시 되지 않지만 `EventToCommandBehavior` 도 포함 되어는 `DeregisterEvent` 의해 실행 되는 메서드는 [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) 메서드. `DeregisterEvent` 메서드를 찾아에 정의 된 이벤트의 등록을 취소를 사용 하는 `EventName` 잠재적인 메모리 누수를 정리 하려면 속성입니다.

## <a name="consuming-the-behavior"></a>동작 사용

`EventToCommandBehavior` 클래스에 연결할 수는 [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) 다음 XAML 코드 예제에서와 같이 컨트롤의 컬렉션:

```xaml
<ListView ItemsSource="{Binding People}">
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}"
                                  Converter="{StaticResource SelectedItemConverter}" />
  </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

해당하는 C# 코드가 다음 코드 예제에 표시됩니다.

```csharp
var listView = new ListView ();
listView.SetBinding (ItemsView<Cell>.ItemsSourceProperty, "People");
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  Command = ((HomePageViewModel)BindingContext).OutputAgeCommand,
  Converter = new SelectedItemEventArgsToSelectedItemConverter ()
});

var selectedItemLabel = new Label ();
selectedItemLabel.SetBinding (Label.TextProperty, "SelectedItemText");
```

`Command` 동작의 속성은 데이터에 바인딩된는 `OutputAgeCommand` 연결 된 ViewModel 속성 동안는 `Converter` 속성이로 설정 되는 `SelectedItemConverter` 인스턴스를 반환 하는 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/)의 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 에서 [ `SelectedItemChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SelectedItemChangedEventArgs/)합니다.

런타임에 동작 컨트롤과 상호 작용에 응답 합니다. 항목을 선택 하는 경우는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) 이벤트는 발생 된 실행 될는 `OutputAgeCommand` ViewModel에 있습니다. ViewModel 업데이트이 `SelectedItemText` 속성 하는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 다음 스크린샷에 표시 된 것 처럼에 바인딩합니다.

[![](event-to-command-behavior-images/screenshots-sml.png "샘플 응용 프로그램으로 EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png#lightbox "샘플 EventToCommandBehavior와 응용 프로그램")

이벤트가 발생할 때 명령을 실행 하려면이 동작을 사용 하 여의 장점은 명령을 명령을 사용 하 여 상호 작용 하도록 설계 되지 않은 컨트롤에 연결할 수입니다. 또한이 코드 숨김 파일에서 보일 러 플레이트 이벤트 처리 코드를 제거합니다.

## <a name="summary"></a>요약

이 문서는 이벤트가 발생할 때 명령을 호출 하려면 Xamarin.Forms 동작을 사용 하 여 보여 줍니다. 동작을 사용 하 여 명령을와 상호 작용 하도록 설계 되지 않은 컨트롤 명령을 연관 시킬 수 있습니다.


## <a name="related-links"></a>관련 링크

- [EventToCommand 동작 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [동작](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [동작<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
