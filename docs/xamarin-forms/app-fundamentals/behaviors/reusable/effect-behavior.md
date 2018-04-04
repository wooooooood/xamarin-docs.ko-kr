---
title: 재사용 가능한 EffectBehavior
description: 동작은 처리 코드 코드 숨김 파일에서 보일 러 플레이트 효과 제거 하는 컨트롤에 효과 추가 하는 데 유용한 접근 방법. 이 문서에서는 효과 컨트롤에 추가할 Xamarin.Forms 동작을 사용 하 여 보여줍니다.
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: a1612d1e87f0e05c859babd93fd03ac9a5736b47
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="reusable-effectbehavior"></a>재사용 가능한 EffectBehavior

_동작은 처리 코드 코드 숨김 파일에서 보일 러 플레이트 효과 제거 하는 컨트롤에 효과 추가 하는 데 유용한 접근 방법. 이 문서에서는 효과 컨트롤에 추가할 Xamarin.Forms 동작을 사용 하 여 보여줍니다._

## <a name="overview"></a>개요

`EffectBehavior` 클래스는 추가 하는 재사용 가능한 Xamarin.Forms 사용자 지정 동작은 [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) 동작의 컨트롤에 연결 하 고 제거 될 때 컨트롤 인스턴스는 `Effect` 동작은 인스턴스 컨트롤에서 분리 합니다.

다음 동작 속성 동작을 사용 하도록 설정 되어야 합니다.

- **그룹** –의 값은 [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) 효과 클래스에 대 한 특성입니다.
- **이름** –의 값은 [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) 효과 클래스에 대 한 특성입니다.

효과 대 한 자세한 내용은 참조 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)합니다.

## <a name="creating-the-behavior"></a>동작 만들기

`EffectBehavior` 클래스에서 파생 되는 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) 클래스, 여기서 `T` 는 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)합니다. 즉는 `EffectBehavior` 클래스 Xamarin.Forms 컨트롤에 연결 될 수 있습니다.

### <a name="implementing-bindable-properties"></a>바인딩 가능 속성을 구현합니다.

`EffectBehavior` 두 클래스 정의 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 추가 하는 데 사용 되는 인스턴스는 [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) 동작 컨트롤에 연결 될 때 컨트롤에 있습니다. 이러한 속성은 다음 코드 예제에 나와 있습니다.

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

경우는 `EffectBehavior` 사용 되는 `Group` 속성의 값으로 설정 해야는 [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) 효과 대 한 특성입니다. 또한는 `Name` 속성의 값으로 설정 해야는 [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) 효과 클래스에 대 한 특성입니다.

### <a name="implementing-the-overrides"></a>재정의 구현합니다.

`EffectBehavior` 재정의 [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) 및 [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) 의 메서드는 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) 다음 코드에 나와 있는 것 처럼 클래스 예:

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

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) 메서드를 호출 하 여 설치를 수행는 `AddEffect` 메서드를 매개 변수로 연결된 된 컨트롤에 전달 합니다. [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) 호출 하 여 정리를 수행 하는 메서드는 `RemoveEffect` 연결 된 컨트롤에 매개 변수로 전달 하는 메서드.

### <a name="implementing-the-behavior-functionality"></a>동작 기능을 구현합니다.

동작의 목적은 추가 하는 [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) 에 정의 된는 `Group` 및 `Name` 속성 동작의 컨트롤에 연결 하 고 제거할 때 컨트롤에는 `Effect` 동작은 때 컨트롤에서 분리 합니다. 핵심 동작 기능은 다음 코드 예제에 표시 됩니다.

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

`AddEffect` 메서드에 대 한 응답으로 실행 되는 `EffectBehavior` 매개 변수로 연결 된 컨트롤을 수신 하는 컨트롤에 연결 되 고 있습니다. 메서드는 다음 검색 된 효과 컨트롤의 추가 [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) 컬렉션입니다. `RemoveEffect` 메서드에 대 한 응답으로 실행 되는 `EffectBehavior` 매개 변수로 연결 된 컨트롤을 수신 하는 컨트롤에서 분리 되 고 있습니다. 메서드는 다음 효과 컨트롤의에서 제거 [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) 컬렉션입니다.

`GetEffect` 메서드는 [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) 를 검색할 메서드는 [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)합니다. 효과의 연결을 통해 위치한는 `Group` 및 `Name` 속성 값입니다. 플랫폼의 효과 제공 하지 않는 경우는 `Effect.Resolve` 메서드는 반환 이외`null` 값입니다.

## <a name="consuming-the-behavior"></a>동작 사용

`EffectBehavior` 클래스에 연결할 수는 [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) 다음 XAML 코드 예제에서와 같이 컨트롤의 컬렉션:

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

`Group` 및 `Name` 동작의 속성의 값으로 설정 됩니다는 [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) 및 [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) 각 플랫폼별에서 효과 클래스에 대 한 특성 프로젝트입니다.

동작에 연결 되어 있으면 런타임에 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 컨트롤의 `Xamarin.LabelShadowEffect` 컨트롤의에 추가 됩니다 [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) 컬렉션입니다. 그림자 하 여 표시 되는 텍스트에 추가 되 고 그 결과 `Label` 다음 스크린샷에서 같이 제어:

![](effect-behavior-images/screenshots.png "샘플 응용 프로그램을 EffectsBehavior")

이 동작을 사용 하 여 추가 하 고 컨트롤에서 효과 제거 하 장점은입니다 코드 숨김 파일에서 보일 러 플레이트 효과 처리 코드를 제거할 수 있습니다.

## <a name="summary"></a>요약

효과 컨트롤에 추가 하는 동작을 사용 하 여이 문서에 명시 합니다. `EffectBehavior` 클래스는 추가 하는 재사용 가능한 Xamarin.Forms 사용자 지정 동작은 [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) 동작의 컨트롤에 연결 하 고 제거 될 때 컨트롤 인스턴스는 `Effect` 동작은 인스턴스 컨트롤에서 분리 합니다.


## <a name="related-links"></a>관련 링크

- [효과](~/xamarin-forms/app-fundamentals/effects/index.md)
- [효과 동작 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [동작](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [동작<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
