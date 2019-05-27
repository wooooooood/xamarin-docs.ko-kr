---
title: 재사용 가능한 EventToCommandBehavior
description: 동작은 명령과 상호 작용하도록 설계되지 않은 컨트롤과 명령을 연결하는 데 사용할 수 있습니다. 이 문서에서는 이벤트가 발생할 때 명령을 호출하도록 Xamarin.Forms 동작을 만들고 사용하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/09/2018
ms.openlocfilehash: 1bb3f319eb104a7425c3be820f5c91efe300737f
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65925174"
---
# <a name="reusable-eventtocommandbehavior"></a>재사용 가능한 EventToCommandBehavior

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/Behaviors/EventToCommandBehavior/)

_동작은 명령과 상호 작용하도록 설계되지 않은 컨트롤과 명령을 연결하는 데 사용할 수 있습니다. 이 문서에서는 이벤트가 발생할 때 명령을 호출하도록 Xamarin.Forms 동작을 만들고 사용하는 방법을 보여줍니다._

## <a name="overview"></a>개요

`EventToCommandBehavior` 클래스는 이벤트 발생에 대한 응답으로 명령을 실행하는 재사용 가능한 Xamarin.Forms 사용자 지정 동작입니다. 기본적으로 이벤트에 대한 이벤트 인수는 명령에 전달되며 [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) 구현을 통해 선택적으로 변환될 수 있습니다.

동작을 사용하기 위해 설정해야 하는 동작 속성은 다음과 같습니다.

- **EventName** – 동작이 수신 대기하는 이벤트의 이름입니다.
- **Command** – 실행될 `ICommand`입니다. 동작은 연결된 컨트롤의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)에서 `ICommand` 인스턴스를 찾으며 이것은 부모 요소에서 상속될 수 있습니다.

다음과 같은 선택적 동작 속성도 설정할 수 있습니다.

- **CommandParameter** – 명령에 전달될 `object`입니다.
- **Converter** – 바인딩 엔진이 원본과 대상 사이에서 데이터를 전달할 때 이벤트 인수의 데이터의 형식을 변경하는 [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) 구현입니다.

> [!NOTE]
> `EventToCommandBehavior`는 [EventToCommand 동작 샘플](https://developer.xamarin.com/samples/xamarin-forms/Behaviors/EventToCommandBehavior/)에 배치할 수 있는 사용자 지정 클래스이며, Xamarin.Forms의 일부가 아닙니다.

## <a name="creating-the-behavior"></a>동작 만들기

`EventToCommandBehavior` 클래스는 `BehaviorBase<T>` 클래스에서 파생되며 이 클래스는 [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) 클래스에서 파생됩니다. `BehaviorBase<T>` 클래스의 용도는 연결된 컨트롤에 동작의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 설정해야 하는 Xamarin.Forms 동작에 대한 기본 클래스를 제공하는 것입니다. 이렇게 하면 동작이 사용될 때 `Command` 속성에 의해 지정된 `ICommand`에 동작을 바인딩하고 실행할 수 있습니다.

`BehaviorBase<T>` 클래스는 동작의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 설정하는 재정의 가능한 [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 메서드와 `BindingContext`를 정리하는 재정의 가능한 [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드를 제공합니다. 또한 클래스는 연결된 컨트롤에 대한 참조를 `AssociatedObject` 속성에 저장합니다.

### <a name="implementing-bindable-properties"></a>바인딩 가능한 속성 구현

`EventToCommandBehavior` 클래스는 이벤트 발생시 사용자 정의 명령을 실행하는 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 인스턴스 4개를 정의합니다. 이러한 속성은 다음 코드 예제에 나와 있습니다.

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

`EventToCommandBehavior` 클래스가 사용되면 `Command` 속성은 `EventName` 속성에 정의된 이벤트 발생에 대한 응답으로 실행되는 `ICommand`에 바인딩된 데이터여야 합니다. 동작은 연결된 컨트롤의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)에서 `ICommand`를 찾을 것으로 예상됩니다.

기본적으로 이벤트에 대한 이벤트 인수는 명령에 전달됩니다. 이 데이터는 [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) 구현을 `Converter` 속성 값으로 지정하면 바인딩 엔진을 통해 원본에서 대상으로 전달되면서 선택적으로 변환될 수 있습니다. 또는 `CommandParameter` 속성 값을 지정하여 매개 변수를 명령에 전달할 수 있습니다.

### <a name="implementing-the-overrides"></a>재정의 구현

`EventToCommandBehavior` 클래스는 다음 코드 예제와 같이 `BehaviorBase<T>` 클래스의 [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 및 [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드를 재정의합니다.

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

[`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 메서드는 `RegisterEvent` 메서드를 호출하고 `EventName` 속성의 값을 매개 변수로 전달하여 설치를 수행합니다. [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드는 `DeregisterEvent` 메서드를 호출하고 `EventName` 속성의 값을 매개 변수로 전달하여 정리를 수행합니다.

### <a name="implementing-the-behavior-functionality"></a>동작 기능 구현

동작의 목적은 `EventName` 속성에 의해 정의된 이벤트 발생에 대한 응답으로 `Command` 속성에 의해 정의된 명령을 실행하는 것입니다. 핵심 동작 기능은 다음 코드 예제와 같습니다.

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

`RegisterEvent` 메서드는 컨트롤에 연결된 `EventToCommandBehavior`에 대한 응답으로 실행되며 `EventName` 속성의 값을 매개 변수로 받습니다. 그런 다음, 메서드는 연결된 컨트롤에서 `EventName` 속성에 정의된 이벤트 찾기를 시도합니다. 이벤트를 찾을 수 있으면 `OnEvent` 메서드가 이벤트의 처리기 메서드로 등록됩니다.

`OnEvent` 메서드는 `EventName` 속성에 정의된 이벤트 발생에 대한 응답으로 실행됩니다. `Command` 속성이 유효한 `ICommand`를 참조하는 경우, 메서드는 다음과 같이 `ICommand`에 전달할 매개 변수를 검색하려고 시도합니다.

- `CommandParameter` 속성이 매개 변수를 정의하면 해당 매개 변수가 검색됩니다.
- 그렇지 않은 경우, `Converter` 속성이 [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) 구현을 정의하면, 변환기가 실행되고 바인딩 엔진에 의해 이벤트 인수 데이터가 원본과 대상 사이에서 전달될 때 이벤트 인수 데이터를 변환합니다.
- 그렇지 않으면 이벤트 인수가 매개 변수로 간주됩니다.

[`CanExecute`](xref:Xamarin.Forms.Command.CanExecute(System.Object)) 메서드가 `true`를 반환하면 데이터 바인딩 `ICommand`가 실행되고 매개 변수가 명령에 전달됩니다.

여기에는 표시되지 않았지만 `EventToCommandBehavior`에는 [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드에 의해 실행되는 `DeregisterEvent` 메서드도 포함됩니다. `DeregisterEvent` 메서드는 잠재적인 메모리 누수를 정리하기 위해 `EventName` 속성에 정의된 이벤트를 찾고 등록을 취소하는 데 사용됩니다.

## <a name="consuming-the-behavior"></a>동작 사용

`EventToCommandBehavior` 클래스는 다음 XAML 코드 예제와 같이 컨트롤의 [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) 컬렉션에 연결할 수 있습니다.

```xaml
<ListView ItemsSource="{Binding People}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <TextCell Text="{Binding Name}" />
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.Behaviors>
        <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}" Converter="{StaticResource SelectedItemConverter}" />
    </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

동등한 C# 코드는 다음 코드 예제와 같습니다.

```csharp
var listView = new ListView();
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "People");
listView.ItemTemplate = new DataTemplate(() =>
{
    var textCell = new TextCell();
    textCell.SetBinding(TextCell.TextProperty, "Name");
    return textCell;
});
listView.Behaviors.Add(new EventToCommandBehavior
{
    EventName = "ItemSelected",
    Command = ((HomePageViewModel)BindingContext).OutputAgeCommand,
    Converter = new SelectedItemEventArgsToSelectedItemConverter()
});

var selectedItemLabel = new Label();
selectedItemLabel.SetBinding(Label.TextProperty, "SelectedItemText");
```

동작의 `Command` 속성은 연결된 ViewModel의 `OutputAgeCommand` 속성에 바인딩된 데이터이며, `Converter` 속성은 [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs)에서 [`ListView`](xref:Xamarin.Forms.ListView)의 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem)을 반환하는 `SelectedItemConverter` 인스턴스로 설정됩니다.

런타임 시 동작은 컨트롤과의 상호 작용에 응답합니다. [`ListView`](xref:Xamarin.Forms.ListView)에서 항목을 선택하면 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트가 발생하고 ViewModel에서 `OutputAgeCommand`가 실행됩니다. 그러면 다음 스크린샷과 같이 [`Label`](xref:Xamarin.Forms.Label)이 바인딩되는 ViewModel `SelectedItemText` 속성이 업데이트됩니다.

[![](event-to-command-behavior-images/screenshots-sml.png "EventToCommandBehavior가 포함된 샘플 애플리케이션")](event-to-command-behavior-images/screenshots.png#lightbox "EventToCommandBehavior가 포함된 샘플 애플리케이션")

이벤트가 발생할 때 이 동작을 사용하여 명령을 실행하는 이점은 명령과 상호 작용하도록 설계되지 않은 컨트롤과 명령을 연결할 수 있다는 점입니다. 또한 상용구 이벤트 처리 코드가 코드 숨김 파일에서 제거됩니다.

## <a name="summary"></a>요약

이 문서에서는 이벤트가 발생할 때 Xamarin.Forms 동작을 사용하여 명령을 호출하는 방법을 보여줍니다. 동작은 명령과 상호 작용하도록 설계되지 않은 컨트롤과 명령을 연결하는 데 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [EventToCommand 동작(샘플)](https://developer.xamarin.com/samples/xamarin-forms/Behaviors/EventToCommandBehavior/)
- [동작](xref:Xamarin.Forms.Behavior)
- [동작&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
