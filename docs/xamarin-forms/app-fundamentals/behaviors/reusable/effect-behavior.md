---
title: 재사용 가능한 EffectBehavior
description: 동작 처리 코드 코드 숨김 파일에서 보일 러 접시 효과 제거 하는 컨트롤에 영향을 줄을 추가 하기 위한 유용한 접근 방식 이며 이 문서에서는 Xamarin.Forms 동작을 사용 하 여 컨트롤에 효과 추가 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 1ce7eda6f556041cbffc3793b00e8e2cba44d3d0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995783"
---
# <a name="reusable-effectbehavior"></a>재사용 가능한 EffectBehavior

_동작 처리 코드 코드 숨김 파일에서 보일 러 접시 효과 제거 하는 컨트롤에 영향을 줄을 추가 하기 위한 유용한 접근 방식 이며 이 문서에서는 Xamarin.Forms 동작을 사용 하 여 컨트롤에 효과 추가 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

`EffectBehavior` 클래스는 추가 하는 재사용 가능한 Xamarin.Forms 사용자 지정 동작을 [ `Effect` ](xref:Xamarin.Forms.Effect) 동작을 컨트롤에 연결 되 고 제거 하는 경우 컨트롤 인스턴스는 `Effect` 동작은 될 때 인스턴스 컨트롤에서 분리 합니다.

동작 속성을 다음 동작을 사용 하도록 설정 해야 합니다.

- **그룹** – 값은 [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 효과 클래스에 대 한 특성입니다.
- **이름** – 값은 [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) 효과 클래스에 대 한 특성입니다.

효과 대 한 자세한 내용은 참조 하세요. [효과](~/xamarin-forms/app-fundamentals/effects/index.md)합니다.

## <a name="creating-the-behavior"></a>동작 만들기

`EffectBehavior` 클래스에서 파생 되는 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) 클래스 여기서 `T` 는 [ `View` ](xref:Xamarin.Forms.View). 즉는 `EffectBehavior` 클래스 Xamarin.Forms 컨트롤에 연결 될 수 있습니다.

### <a name="implementing-bindable-properties"></a>바인딩 가능한 속성 구현

`EffectBehavior` 두 개의 클래스 정의 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 추가 하는 데 사용 되는 인스턴스를 [ `Effect` ](xref:Xamarin.Forms.Effect) 동작을 컨트롤에 연결 될 때 컨트롤에 합니다. 이러한 속성은 다음 코드 예제에 나와 있습니다.

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

경우는 `EffectBehavior` 사용 되는 `Group` 속성의 값으로 설정 해야 합니다 [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 효과 대 한 특성. 또한 합니다 `Name` 속성의 값으로 설정 해야 합니다 [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) 효과 클래스에 대 한 특성입니다.

### <a name="implementing-the-overrides"></a>재정의 구현합니다.

`EffectBehavior` 재정의 클래스를 [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 및 [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드를 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) 클래스에 다음 코드 에서처럼 예:

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

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 메서드를 호출 하 여 설치를 수행 합니다 `AddEffect` 메서드를 연결 된 컨트롤에 매개 변수로 전달 합니다. [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드를 호출 하 여 정리를 수행 합니다 `RemoveEffect` 메서드를 연결 된 컨트롤에 매개 변수로 전달 합니다.

### <a name="implementing-the-behavior-functionality"></a>동작 기능을 구현

동작의 목적은 추가 하는 것을 [ `Effect` ](xref:Xamarin.Forms.Effect) 에 정의 된를 `Group` 및 `Name` 동작을 컨트롤에 연결 되 고 제거 하는 경우 컨트롤 속성을 `Effect` 동작의 경우 컨트롤에서 분리 합니다. 동작의 핵심 기능을 다음 코드 예제에 표시 됩니다.

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

`AddEffect` 메서드에 대 한 응답으로 실행 되는 `EffectBehavior` 매개 변수로 연결된 된 컨트롤을 수신 하며 컨트롤에 연결 되 고 합니다. 메서드는 다음 검색된 결과를 컨트롤의 추가 [ `Effects` ](xref:Xamarin.Forms.Element.Effects) 컬렉션입니다. `RemoveEffect` 메서드에 대 한 응답으로 실행 되는 `EffectBehavior` 연결 된 컨트롤에 매개 변수로 받는 컨트롤에서 분리 되 고 합니다. 메서드는 컨트롤에서 효과 제거 합니다 [ `Effects` ](xref:Xamarin.Forms.Element.Effects) 컬렉션입니다.

합니다 `GetEffect` 메서드를 [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) 검색 하는 메서드를 [ `Effect` ](xref:Xamarin.Forms.Effect)합니다. 효과의 연결을 통해 위치한 합니다 `Group` 고 `Name` 속성 값입니다. 플랫폼 효과 제공 하지 않는 경우는 `Effect.Resolve` 메서드는 반환 된 비-`null` 값입니다.

## <a name="consuming-the-behavior"></a>동작 사용

합니다 `EffectBehavior` 클래스에 연결할 수는 [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) 다음 XAML 코드 예제에서 설명한 것 처럼 컨트롤의 컬렉션:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Behaviors>
    <local:EffectBehavior Group="Xamarin" Name="LabelShadowEffect" />
  </Label.Behaviors>
</Label>
```

해당하는 C# 코드가 다음 코드 예제에 표시됩니다.

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

합니다 `Group` 하 고 `Name` 동작의 속성 값으로 설정 됩니다는 [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 및 [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) 각 플랫폼별 결과 클래스에 대 한 특성 프로젝트입니다.

동작에 연결 될 때 런타임 시 합니다 [ `Label` ](xref:Xamarin.Forms.Label) 컨트롤을 `Xamarin.LabelShadowEffect` 컨트롤에 추가 됩니다 [ `Effects` ](xref:Xamarin.Forms.Element.Effects) 컬렉션입니다. 그러면 표시 되는 텍스트에 추가할 그림자를 `Label` 다음 스크린샷과에서 같이 제어:

![](effect-behavior-images/screenshots.png "EffectsBehavior 사용 하 여 샘플 응용 프로그램")

이 동작을 사용 하 여 추가 컨트롤에서 효과 제거 하의 장점은 코드 숨김 파일에서 보일 러 상용구 효과 처리 코드를 제거할 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 컨트롤에 영향을 줄을 추가 하는 동작을 사용 하 여 보여 줍니다. `EffectBehavior` 클래스는 추가 하는 재사용 가능한 Xamarin.Forms 사용자 지정 동작을 [ `Effect` ](xref:Xamarin.Forms.Effect) 동작을 컨트롤에 연결 되 고 제거 하는 경우 컨트롤 인스턴스는 `Effect` 동작은 될 때 인스턴스 컨트롤에서 분리 합니다.


## <a name="related-links"></a>관련 링크

- [효과](~/xamarin-forms/app-fundamentals/effects/index.md)
- [효과 동작 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [동작](xref:Xamarin.Forms.Behavior)
- [동작<T>](xref:Xamarin.Forms.Behavior`1)
