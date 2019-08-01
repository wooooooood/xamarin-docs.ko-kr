---
title: 재사용 가능한 EffectBehavior
description: 동작은 컨트롤에 효과를 추가하고 코드 숨김 파일에서 표준 효과 처리 코드를 제거하는 데 유용한 방법입니다. 이 문서에서는 Xamarin.Forms 동작을 만들고 사용하여 는 컨트롤에 효과를 추가하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 38ecd765b1c6bc81054b2c42426b6c15bb99b9d9
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68650982"
---
# <a name="reusable-effectbehavior"></a>재사용 가능한 EffectBehavior

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-effectbehavior)

_동작은 컨트롤에 효과를 추가하고 코드 숨김 파일에서 표준 효과 처리 코드를 제거하는 데 유용한 방법입니다. 이 문서에서는 Xamarin.Forms 동작을 만들고 사용하여 는 컨트롤에 효과를 추가하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

`EffectBehavior` 클래스는 재사용 가능한 Xamarin.Forms 사용자 지정 동작입니다. 즉 동작이 컨트롤에 연결되면 [`Effect`](xref:Xamarin.Forms.Effect) 인스턴스를 컨트롤에 추가하고, 동작이 컨트롤에서 분리되면 `Effect` 인스턴스를 제거합니다.

동작을 사용하기 위해 설정해야 하는 동작 속성은 다음과 같습니다.

- **Group** – 효과 클래스에 대한 [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 특성의 값입니다.
- **Name** – 효과 클래스에 대한 [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) 특성의 값입니다.

효과에 대한 자세한 내용은 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)를 참조하세요.

> [!NOTE]
> `EffectBehavior`는 [효과 동작 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-effectbehavior)에 있는 사용자 지정 클래스이며, Xamarin.Forms의 일부가 아닙니다.

## <a name="creating-the-behavior"></a>동작 만들기

`EffectBehavior` 클래스는 [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) 클래스에서 파생되며, 여기서 `T`는 [`View`](xref:Xamarin.Forms.View)입니다. 즉 `EffectBehavior` 클래스를 모든 Xamarin.Forms 컨트롤에 연결할 수 있습니다.

### <a name="implementing-bindable-properties"></a>바인딩 가능한 속성 구현

`EffectBehavior` 클래스는 두 개의 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 인스턴스를 정의합니다. 이러한 인스턴스는 동작이 컨트롤에 연결되면 [`Effect`](xref:Xamarin.Forms.Effect)를 컨트롤에 추가하는 데 사용됩니다. 이러한 속성은 다음 코드 예제와 같습니다.

```csharp
public class EffectBehavior : Behavior<View>
{
  public static readonly BindableProperty GroupProperty =
    BindableProperty.Create ("Group", typeof(string), typeof(EffectBehavior), null);
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(EffectBehavior), null);

  public string Group {
    get { return (string)GetValue (GroupProperty); }
    set { SetValue (GroupProperty, value); }
  }

  public string Name {
    get { return(string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }
  ...
}
```

`EffectBehavior`가 사용되면 `Group` 속성을 효과에 대한 [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 특성의 값으로 설정해야 합니다. 또한 `Name` 속성을 효과 클래스에 대한 [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) 특성의 값으로 설정해야 합니다.

### <a name="implementing-the-overrides"></a>재정의 구현

`EffectBehavior` 클래스는 다음 코드 예제와 같이 [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) 클래스의 [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 및 [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드를 재정의합니다.

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  protected override void OnAttachedTo (BindableObject bindable)
  {
    base.OnAttachedTo (bindable);
    AddEffect (bindable as View);
  }

  protected override void OnDetachingFrom (BindableObject bindable)
  {
    RemoveEffect (bindable as View);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

[`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 메서드는 `AddEffect` 메서드를 호출하여 연결된 컨트롤을 매개 변수로 전달함으로써 설치를 수행합니다. [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드는 `RemoveEffect` 메서드를 호출하여 연결된 컨트롤을 매개 변수로 전달함으로써 정리를 수행합니다.

### <a name="implementing-the-behavior-functionality"></a>동작 기능 구현

동작의 목적은 동작이 컨트롤에 연결되면 `Group` 및 `Name` 속성에 정의된 [`Effect`](xref:Xamarin.Forms.Effect)를 컨트롤에 추가하고, 동작이 컨트롤에서 분리되면 `Effect`를 제거하는 것입니다. 핵심 동작 기능은 다음 코드 예제와 같습니다.

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  void AddEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Add (GetEffect ());
    }
  }

  void RemoveEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Remove (GetEffect ());
    }
  }

  Effect GetEffect ()
  {
    if (!string.IsNullOrWhiteSpace (Group) && !string.IsNullOrWhiteSpace (Name)) {
      return Effect.Resolve (string.Format ("{0}.{1}", Group, Name));
    }
    return null;
  }
}
```

`AddEffect` 메서드는 컨트롤에 연결되는 `EffectBehavior`에 대한 응답으로 실행되며, 연결된 컨트롤을 매개 변수로 받습니다. 그런 다음, 이 메서드는 검색된 효과를 컨트롤의 [`Effects`](xref:Xamarin.Forms.Element.Effects) 컬렉션에 추가합니다. `RemoveEffect` 메서드는 컨트롤에서 분리되는 `EffectBehavior`에 대한 응답으로 실행되며, 연결된 컨트롤을 매개 변수로 받습니다. 그런 다음, 이 메서드는 컨트롤의 [`Effects`](xref:Xamarin.Forms.Element.Effects) 컬렉션에서 효과를 제거합니다.

`GetEffect` 메서드는 [`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String)) 메서드를 사용하여 [`Effect`](xref:Xamarin.Forms.Effect)를 검색합니다. 효과는 `Group` 및 `Name` 속성 값의 연결을 통해 찾습니다. 플랫폼에서 효과를 제공하지 않으면 `Effect.Resolve` 메서드에서 `null`이 아닌 값을 반환합니다.

## <a name="consuming-the-behavior"></a>동작 사용

`EffectBehavior` 클래스는 다음 XAML 코드 예제와 같이 컨트롤의 [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) 컬렉션에 연결할 수 있습니다.

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Behaviors>
    <local:EffectBehavior Group="Xamarin" Name="LabelShadowEffect" />
  </Label.Behaviors>
</Label>
```

동등한 C# 코드는 다음 코드 예제와 같습니다.

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};
label.Behaviors.Add (new EffectBehavior {
  Group = "Xamarin",
  Name = "LabelShadowEffect"
});
```

동작의 `Group` 및 `Name` 속성은 각 플랫폼별 프로젝트의 효과 클래스에 대한 [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 및 [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) 특성의 값으로 설정됩니다.

런타임에 동작이 [`Label`](xref:Xamarin.Forms.Label) 컨트롤에 연결되면 `Xamarin.LabelShadowEffect`가 컨트롤의 [`Effects`](xref:Xamarin.Forms.Element.Effects) 컬렉션에 추가됩니다. 그러면 다음 스크린샷과 같이 `Label` 컨트롤에 표시되는 텍스트에 그림자가 추가됩니다.

![](effect-behavior-images/screenshots.png "EffectsBehavior가 있는 애플리케이션 샘플")

컨트롤에서 효과를 추가하고 제거하는 데 이러한 동작을 사용하면 표준 효과 처리 코드를 코드 숨김 파일에서 제거할 수 있다는 이점이 있습니다.

## <a name="summary"></a>요약

이 문서에서는 동작을 사용하여 컨트롤에 효과를 추가하는 방법을 보여 주었습니다. `EffectBehavior` 클래스는 재사용 가능한 Xamarin.Forms 사용자 지정 동작입니다. 즉 동작이 컨트롤에 연결되면 [`Effect`](xref:Xamarin.Forms.Effect) 인스턴스를 컨트롤에 추가하고, 동작이 컨트롤에서 분리되면 `Effect` 인스턴스를 제거합니다.


## <a name="related-links"></a>관련 링크

- [효과](~/xamarin-forms/app-fundamentals/effects/index.md)
- [효과 동작(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-effectbehavior)
- [동작](xref:Xamarin.Forms.Behavior)
- [동작&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
