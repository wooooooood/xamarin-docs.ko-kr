---
title: Xamarin.Forms 동작
description: Xamarin.Forms 동작은 Behavior 또는 Behavior<T> 클래스에서 파생되어 만들어집니다. 이 문서에서는 Xamarin.Forms 동작을 만들고 사용하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 5b94202628c1bcc09d5d2191cb803d58c3e0f0f8
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53054883"
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms 동작

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)

_Xamarin.Forms 동작은 Behavior 또는 Behavior<T> 클래스에서 파생되어 만들어집니다. 이 문서에서는 Xamarin.Forms 동작을 만들고 사용하는 방법을 보여줍니다._

## <a name="overview"></a>개요

Xamarin.Forms 동작을 만들기 위한 프로세스는 다음과 같습니다.

1. [`Behavior`](xref:Xamarin.Forms.Behavior) 또는 [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) 클래스에서 상속되는 클래스를 만듭니다. 여기서 `T`는 동작이 적용되어야 하는 컨트롤의 형식입니다.
1. 필요한 설정을 수행하도록 [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 메서드를 재정의합니다.
1. 필요한 정리를 수행하도록 [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드를 재정의합니다.
1. 동작의 핵심 기능을 구현합니다.

다음 코드 예제에서 표시된 구조가 됩니다.

```csharp
public class CustomBehavior : Behavior<View>
{
    protected override void OnAttachedTo (View bindable)
    {
        base.OnAttachedTo (bindable);
        // Perform setup
    }

    protected override void OnDetachingFrom (View bindable)
    {
        base.OnDetachingFrom (bindable);
        // Perform clean up
    }

    // Behavior implementation
}
```

동작이 컨트롤에 연결된 후 즉시 [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 메서드가 발생합니다. 이 메서드는 연결된 컨트롤에 대한 참조를 받고, 이벤트 처리기를 등록하거나 동작 기능을 지원하는 데 필요한 다른 설정을 수행하는 데 사용할 수 있습니다. 예를 들어 컨트롤에서 이벤트를 구독할 수 있습니다. 그런 다음, 동작 기능은 이벤트에 대한 이벤트 처리기에서 구현됩니다.

동작이 컨트롤에서 제거될 때 [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드가 발생합니다. 이 메서드는 연결된 컨트롤에 대한 참조를 받고, 필요한 모든 정리를 수행하는 데 사용됩니다. 예를 들어 메모리 누수를 방지하기 위해 컨트롤에서 이벤트를 구독 취소할 수 있습니다.

그런 다음, 적절한 컨트롤의 [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) 컬렉션에 연결하여 동작을 사용할 수 있습니다.

## <a name="creating-a-xamarinforms-behavior"></a>Xamarin.Forms Behavior 만들기

샘플 애플리케이션은 사용자가 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤에 입력한 값이 `double`이 아닌 경우 빨간색으로 강조 표시하는 `NumericValidationBehavior`를 설명합니다. 동작은 다음 코드 예제에 나와 있습니다.

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    protected override void OnAttachedTo(Entry entry)
    {
        entry.TextChanged += OnEntryTextChanged;
        base.OnAttachedTo(entry);
    }

    protected override void OnDetachingFrom(Entry entry)
    {
        entry.TextChanged -= OnEntryTextChanged;
        base.OnDetachingFrom(entry);
    }

    void OnEntryTextChanged(object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

`NumericValidationBehavior`는 [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) 클래스에서 파생되며, 여기서 `T`는 [`Entry`](xref:Xamarin.Forms.Entry)입니다. [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 메서드는 메모리 누수를 방지하도록 `TextChanged` 이벤트를 등록 취소하는 [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드를 사용하여 [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) 이벤트에 대한 이벤트 처리기를 등록합니다. 동작의 핵심 기능은 `OnEntryTextChanged` 메서드에서 제공됩니다. 이는 사용자가 `Entry`에 입력한 값을 구문 분석하고, 값이 `double`이 아닌 경우 [`TextColor`](xref:Xamarin.Forms.Entry.TextColor) 속성을 빨간색으로 설정합니다.

> [!NOTE]
> 동작은 스타일을 통해 공유되고 여러 컨트롤에 적용할 수 있으므로 Xamarin.Forms는 동작의 `BindingContext`를 설정하지 않습니다.

## <a name="consuming-a-xamarinforms-behavior"></a>Xamarin.Forms Behavior 사용

모든 Xamarin.Forms 컨트롤에는 다음 XAML 코드 예제에서 설명한 것처럼 하나 이상의 동작을 추가할 수 있는 [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) 컬렉션이 있습니다.

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

C#의 해당하는 [`Entry`](xref:Xamarin.Forms.Entry)가 다음 코드 예제에 나와 있습니다.

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

런타임 시 동작은 동작 구현에 따라 응답하여 컨트롤과 상호 작용합니다. 다음 스크린샷에서는 잘못된 입력에 응답하는 동작을 설명합니다.

[![](creating-images/screenshots-sml.png "Xamarin.Forms Behavior를 사용하는 애플리케이션 예제")](creating-images/screenshots.png#lightbox "Xamarin.Forms Behavior를 사용하는 애플리케이션 예제")

> [!NOTE]
> 동작은 특정 컨트롤 형식(또는 여러 컨트롤에 적용할 수 있는 슈퍼클래스)에 작성되며, 호환 컨트롤에만 추가해야 합니다. 호환되지 않는 컨트롤에 동작을 연결하면 예외가 throw됩니다.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>스타일과 함께 Xamarin.Forms Behavior 사용

동작은 명시적 또는 암시적 스타일에 의해 사용될 수도 있습니다. 그러나 속성은 읽기 전용이므로 컨트롤의 [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) 속성을 설정하는 스타일을 만드는 것은 불가능합니다. 솔루션은 동작 추가 및 제거를 제어하는 동작 클래스에 연결된 속성을 추가하는 것입니다. 프로세스는 다음과 같습니다.

1. 동작이 추가되는 컨트롤에 동작의 추가 또는 제거를 제어하는 데 사용될 동작 클래스에 연결된 속성을 추가합니다. 연결된 속성이 속성 값이 변경될 때 실행할 `propertyChanged` 대리자를 등록하는지 확인합니다.
1. 연결된 속성에 대한 `static` getter 및 setter를 만듭니다.
1. `propertyChanged` 대리자에서 논리를 구현하여 동작을 추가 및 제거합니다.

다음 코드 예제에서는 `NumericValidationBehavior` 추가 및 제거를 제어하는 연결된 속성을 보여줍니다.

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached ("AttachBehavior", typeof(bool), typeof(NumericValidationBehavior), false, propertyChanged: OnAttachBehaviorChanged);

    public static bool GetAttachBehavior (BindableObject view)
    {
        return (bool)view.GetValue (AttachBehaviorProperty);
    }

    public static void SetAttachBehavior (BindableObject view, bool value)
    {
        view.SetValue (AttachBehaviorProperty, value);
    }

    static void OnAttachBehaviorChanged (BindableObject view, object oldValue, object newValue)
    {
        var entry = view as Entry;
        if (entry == null) {
            return;
        }

        bool attachBehavior = (bool)newValue;
        if (attachBehavior) {
            entry.Behaviors.Add (new NumericValidationBehavior ());
        } else {
            var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
            if (toRemove != null) {
                entry.Behaviors.Remove (toRemove);
            }
        }
    }
    ...
}
```

`NumericValidationBehavior` 클래스에는 `static` getter 및 setter와 함께 `AttachBehavior`라는 연결된 속성이 포함되어 있습니다. 이는 클래스가 연결될 컨트롤에 동작의 추가 또는 제거를 제어합니다. 이 연결된 속성은 속성 값이 변경될 때 실행할 `OnAttachBehaviorChanged` 메서드를 등록합니다. 이 메서드는 `AttachBehavior` 연결된 속성의 값을 기반으로 컨트롤에 동작을 추가 또는 제거합니다.

다음 코드 예제는 `AttachBehavior` 연결된 속성을 사용하는 `NumericValidationBehavior`에 대한 *명시적* 스타일을 보여주며, [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤에 적용될 수 있습니다.

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

다음 코드 예제에 설명된 대로 `StaticResource` 태그 확장을 사용하여 해당 [`Style`](xref:Xamarin.Forms.VisualElement.Style) 속성을 `Style` 인스턴스에 설정하여 [`Style`](xref:Xamarin.Forms.Style)을 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤에 적용할 수 있습니다.

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

스타일에 대한 자세한 내용은 [스타일](~/xamarin-forms/user-interface/styles/index.md)을 참조하세요.

> [!NOTE]
> XAML에서 설정되거라 쿼리되는 동작에 바인딩 가능한 속성을 추가할 수 있지만 상태가 있는 동작을 만드는 경우 `Style` 및 `ResourceDictionary`에서 컨트롤 간에 공유해서는 안 됩니다.

### <a name="removing-a-behavior-from-a-control"></a>컨트롤에서 동작 제거

[`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드는 동작이 컨트롤에서 제거되면 발생되며, 메모리 누수를 방지하도록 이벤트 구독 취소와 같은 필요한 모든 정리를 수행하는 데 사용됩니다. 그러나 컨트롤의 [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) 컬렉션이 `Remove` 또는 `Clear` 메서드에 의해 수정되지 않는 한 동작은 컨트롤에서 암시적으로 제거되지 않습니다. 다음 코드 예제에서는 컨트롤의 `Behaviors` 컬렉션에서 특정 동작 제거를 보여줍니다.

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

또는 다음 코드 예제에서 설명한 것처럼 컨트롤의 [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) 컬렉션을 지울 수 있습니다.

```csharp
entry.Behaviors.Clear();
```

또한 페이지가 탐색 스택에서 팝되는 경우 동작은 컨트롤에서 암시적으로 제거되지 않습니다. 대신 페이지가 범위를 벗어나기 전에 명시적으로 제거되어야 합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms 동작을 만들고 사용하는 방법을 설명했습니다. Xamarin.Forms 동작은 [`Behavior`](xref:Xamarin.Forms.Behavior) 또는 [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) 클래스에서 파생되어 만들어집니다.


## <a name="related-links"></a>관련 링크

- [Xamarin.Forms Behavior(샘플)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [스타일을 사용하여 적용된 Xamarin.Forms Behavior(샘플)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [Behavior](xref:Xamarin.Forms.Behavior)
- [Behavior<T>](xref:Xamarin.Forms.Behavior`1)
