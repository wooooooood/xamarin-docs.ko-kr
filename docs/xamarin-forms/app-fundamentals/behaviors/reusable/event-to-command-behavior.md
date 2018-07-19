---
title: 재사용 가능한 EventToCommandBehavior
description: 명령을 사용 하 여 상호 작용 하도록 설계 되지 않았습니다 하는 컨트롤을 사용 하 여 명령을 연결 동작을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms 동작을 사용 하 여 이벤트가 발생 하는 경우 명령을 호출 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 3151179b6ff6d26b74a87ded747310646b304603
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996323"
---
# <a name="reusable-eventtocommandbehavior"></a>재사용 가능한 EventToCommandBehavior

_명령을 사용 하 여 상호 작용 하도록 설계 되지 않았습니다 하는 컨트롤을 사용 하 여 명령을 연결 동작을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms 동작을 사용 하 여 이벤트가 발생 하는 경우 명령을 호출 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

합니다 `EventToCommandBehavior` 클래스에 대 한 응답으로 명령을 실행 하는 재사용 가능한 Xamarin.Forms 사용자 지정 동작입니다 *모든* 이벤트 발생 합니다. 기본적으로 이벤트 인수를 이벤트 명령에 전달 됩니다 및 필요에 따라 수에 대 한 변환 하 여는 [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) 구현 합니다.

동작 속성을 다음 동작을 사용 하도록 설정 해야 합니다.

- **EventName** – 동작을 수신 대기 이벤트의 이름입니다.
- **명령** – **ICommand** 실행 될 수 있습니다. 동작을 찾으려고 시도 합니다 `ICommand` 인스턴스를 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 을 부모 요소 로부터 상속 될 수 있는 연결 된 컨트롤의.

다음 선택적 동작 속성을 설정할 수도 있습니다.

- **CommandParameter** –는 `object` 는 명령에 전달 됩니다.
- **변환기** 는 – [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) 간에 전달 될 때 이벤트 인수 데이터의 형식을 변경 되는 구현 *원본* 고 *대상*바인딩 엔진에서.

## <a name="creating-the-behavior"></a>동작 만들기

`EventToCommandBehavior` 클래스에서 파생 되는 `BehaviorBase<T>` 클래스에서 파생 됩니다 합니다 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) 클래스. 목적은 `BehaviorBase<T>` 클래스를 필요로 하는 모든 Xamarin.Forms 동작에 대 한 기본 클래스를 제공 하는 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 연결된 된 컨트롤에 설정 되도록 동작 합니다. 이렇게 하면 동작을 바인딩할 수 있고 실행 합니다 `ICommand` 에 지정 된는 `Command` 속성 동작을 사용 하는 경우.

`BehaviorBase<T>` 클래스는 재정의 제공 [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 설정 하는 메서드를 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 동작 및는 재정의 가능한 [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))정리 하는 메서드는 `BindingContext`합니다. 이 클래스에 연결된 된 컨트롤에 대 한 참조를 저장 하는 또한는 `AssociatedObject` 속성입니다.

### <a name="implementing-bindable-properties"></a>바인딩 가능한 속성 구현

합니다 `EventToCommandBehavior` 4 클래스 정의 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 사용자를 실행 하는 경우 이벤트가 발생 하는 경우 명령을 정의 합니다. 이러한 속성은 다음 코드 예제에 나와 있습니다.

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

경우는 `EventToCommandBehavior` 클래스 사용 되는 `Command` 속성에 바인딩된 데이터 여야 합니다는 `ICommand` 에 정의 된 이벤트 발생에 대 한 응답에서 실행 될는 `EventName` 속성. 동작 증명을 찾습니다 합니다 `ICommand` 에 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 연결 된 컨트롤의 합니다.

기본적으로 이벤트에 대 한 이벤트 인수를 명령에 전달 됩니다. 간에 전달 될 때이 데이터가 필요에 따라 변환 될 수 *원본* 하 고 *대상* 지정 하 여 바인딩 엔진에 의해를 [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) 구현으로는 `Converter` 속성 값입니다. 지정 하 여 명령에 매개 변수를 전달 하 되 또는 `CommandParameter` 속성 값입니다.

### <a name="implementing-the-overrides"></a>재정의 구현합니다.

합니다 `EventToCommandBehavior` 재정의 클래스를 [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 및 [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드를 `BehaviorBase<T>` 클래스에 다음 코드 예제에 표시 된 대로:

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

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 메서드를 호출 하 여 설치를 수행 합니다 `RegisterEvent` 의 값을 전달 하는 메서드는 `EventName` 속성을 매개 변수로 합니다. [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드를 호출 하 여 정리를 수행 합니다 `DeregisterEvent` 의 값을 전달 하는 메서드는 `EventName` 속성을 매개 변수로 합니다.

### <a name="implementing-the-behavior-functionality"></a>동작 기능을 구현

동작의 목적은 정의한 명령을 실행 하는 것을 `Command` 속성에서 정의한 이벤트 발생에 대 한 응답에서을 `EventName` 속성입니다. 동작의 핵심 기능을 다음 코드 예제에 표시 됩니다.

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

합니다 `RegisterEvent` 메서드에 대 한 응답으로 실행 되는 `EventToCommandBehavior` 의 값을 수신 하며 컨트롤에 연결 되는 `EventName` 속성을 매개 변수로 합니다. 메서드는 다음에 정의 된 이벤트를 찾으려고 시도 합니다 `EventName` 연결 된 컨트롤의 속성입니다. 이벤트를 찾을 수 있습니다는 `OnEvent` 메서드가 이벤트 처리기 방법으로 등록 됩니다.

합니다 `OnEvent` 에 정의 된 이벤트 발생에 대 한 응답에서 메서드를 실행 합니다 `EventName` 속성입니다. 제공한 합니다 `Command` 속성이 유효한 참조 `ICommand`, 메서드에 전달할 매개 변수를 검색 하려고 시도 `ICommand` 같이:

- 경우는 `CommandParameter` 매개 변수를 정의 하는 속성을 검색 합니다.
- 그렇지 않은 경우, 합니다 `Converter` 속성 정의 [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) 구현에서 변환기가 실행 되 고 간에 전달 될 때 이벤트 인수 데이터를 변환 *원본* 및 *대상* 바인딩 엔진에서.
- 이 고, 그렇지 이벤트 인수를 매개 변수로 간주 됩니다.

데이터 바인딩된 `ICommand` 다음 실행 되는 명령에 제공 하는 매개 변수에 전달 합니다 [ `CanExecute` ](xref:Xamarin.Forms.Command.CanExecute(System.Object)) 메서드가 반환 되는 `true`합니다.

여기에 표시 되지 않지만 합니다 `EventToCommandBehavior` 도 포함 되어 있습니다를 `DeregisterEvent` 에서 실행 되는 메서드는 [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드. `DeregisterEvent` 메서드를 찾아에 정의 된 이벤트를 등록 취소를 사용 하는 `EventName` 속성 잠재적인 메모리 누수를 정리 합니다.

## <a name="consuming-the-behavior"></a>동작 사용

합니다 `EventToCommandBehavior` 클래스에 연결할 수는 [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) 다음 XAML 코드 예제에서 설명한 것 처럼 컨트롤의 컬렉션:

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

`Command` 동작의 속성은 데이터를 바인딩할를 `OutputAgeCommand` 연결 된 ViewModel의 속성 하는 동안는 `Converter` 속성을 `SelectedItemConverter` 인스턴스를 반환 하는 [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem)의 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 에서 [ `SelectedItemChangedEventArgs` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs)합니다.

런타임에 동작 컨트롤과 상호 작용에 응답 합니다. 항목을 선택 하는 경우는 [ `ListView` ](xref:Xamarin.Forms.ListView)의 [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트가 발생 하 고 실행 하는 `OutputAgeCommand` ViewModel에 합니다. ViewModel이 업데이트를 차례로 `SelectedItemText` 속성은를 [ `Label` ](xref:Xamarin.Forms.Label) 다음 스크린샷과에서 같이 바인딩합니다.

[![](event-to-command-behavior-images/screenshots-sml.png "샘플 EventToCommandBehavior 사용 하 여 응용 프로그램")](event-to-command-behavior-images/screenshots.png#lightbox "EventToCommandBehavior 사용 하 여 응용 프로그램 샘플")

이 동작을 사용 하 여 이벤트가 발생 하는 경우 명령을 실행할 때의 장점은 명령을 명령을 사용 하 여 상호 작용 하도록 설계 되지 않은 컨트롤에 연결할 수입니다. 또한이 코드 숨김 파일에서 상용구 이벤트 처리 코드 양을 제거합니다.

## <a name="summary"></a>요약

이 문서는 이벤트가 발생할 때 명령을 호출 하는 Xamarin.Forms 동작을 사용 하 여 보여 줍니다. 명령을 사용 하 여 상호 작용 하도록 설계 되지 않았습니다 하는 컨트롤을 사용 하 여 명령을 연결 동작을 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [EventToCommand 동작 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [동작](xref:Xamarin.Forms.Behavior)
- [동작<T>](xref:Xamarin.Forms.Behavior`1)
