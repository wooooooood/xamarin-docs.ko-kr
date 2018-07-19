---
title: Xamarin.Forms 동작
description: Xamarin.Forms 동작에서 동작 또는 동작을 파생 하 여 만들어진<T> 클래스입니다. 이 문서를 만들고 Xamarin.Forms 동작을 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 7e057567ec0bb72e9bcc016d4a9fef3af78a3ea1
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998897"
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms 동작

_Xamarin.Forms 동작에서 동작 또는 동작을 파생 하 여 만들어진<T> 클래스입니다. 이 문서를 만들고 Xamarin.Forms 동작을 사용 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

Xamarin.Forms 동작을 만들기 위한 프로세스는 다음과 같습니다.

1. 상속 되는 클래스를 만듭니다는 [ `Behavior` ](xref:Xamarin.Forms.Behavior) 또는 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) 클래스는 `T` 동작 적용 되어야 하는 컨트롤의 형식입니다.
1. 재정의 된 [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 필요한 설정을 수행 하는 방법입니다.
1. 재정의 된 [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 필요한 정리를 수행 하는 방법입니다.
1. 동작의 핵심 기능을 구현 합니다.

그러면 다음 코드 예제에 표시 되는 구조:

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

합니다 [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 메서드 동작을 컨트롤에 연결 된 직후에 발생 합니다. 이 메서드는 연결 되 고 이벤트 처리기를 등록 하 여 동작 기능을 지원 하는 데 필요한 다른 설정을 수행에 사용할 수는 컨트롤에 대 한 참조를 받습니다. 예를 들어 컨트롤에서 이벤트를 구독할 수 있습니다. 동작 기능은 이벤트에 대 한 이벤트 처리기에서 구현 합니다.

합니다 [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드 동작을 컨트롤에서 제거 될 때 발생 합니다. 이 메서드는 연결 되 고 필요한 정리를 수행 하는 데 사용 되는 컨트롤에 대 한 참조를 받습니다. 예를 들어, 메모리 누수를 방지 하기 위해 컨트롤에서 이벤트에서 지 구독 취소 수 없습니다.

동작에 연결 하 여 사용할 수 있습니다 합니다 [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) 적절 한 컨트롤의 컬렉션입니다.

## <a name="creating-a-xamarinforms-behavior"></a>Xamarin.Forms 동작 만들기

샘플 응용 프로그램을 보여 줍니다는 `NumericValidationBehavior`에 사용자가 입력 한 값을 강조 표시 하는 [ `Entry` ](xref:Xamarin.Forms.Entry) 있지 않으면 빨간색으로 제어를 `double`입니다. 동작은 다음 코드 예제에 표시 됩니다.

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

`NumericValidationBehavior` 에서 파생 되는 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) 클래스를 `T` 는 [ `Entry` ](xref:Xamarin.Forms.Entry)합니다. 합니다 [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) 에 대 한 이벤트 처리기를 등록 하는 메서드를 [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) 이벤트를 사용 하 여는 [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드를 등록 취소 합니다 `TextChanged`누수 메모리가 것을 방지 하는 이벤트입니다. 동작의 핵심 기능을 제공 합니다 `OnEntryTextChanged` 에 사용자가 입력 한 값을 구문 분석 하는 메서드를를 `Entry`, 설정 및는 [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor) 속성 값이 아닌 경우 빨간색을를 `double`입니다.

> [!NOTE]
> Xamarin.Forms는 설정 하지 않습니다는 `BindingContext` 동작, 동작을 공유 하 고 스타일을 통해 여러 컨트롤에 적용할 수 있으므로 합니다.

## <a name="consuming-a-xamarinforms-behavior"></a>Xamarin.Forms 동작 사용

모든 Xamarin.Forms 컨트롤에는 [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) 컬렉션에 추가 하는 하나 이상의 동작 수를 다음 XAML 코드 예제에서 설명한 것 처럼:

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

해당 [ `Entry` ](xref:Xamarin.Forms.Entry) C#에서 다음 코드 예제에 표시 됩니다.

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

런타임에 동작 동작 구현에 따라 컨트롤과 상호 작용에 응답 합니다. 다음 스크린샷에서 잘못 된 입력에 응답 동작을 보여 줍니다.

[![](creating-images/screenshots-sml.png "Xamarin.Forms 동작을 사용 하 여 응용 프로그램 예제")](creating-images/screenshots.png#lightbox "Xamarin.Forms 동작을 사용 하 여 응용 프로그램 샘플")

> [!NOTE]
> 동작은 특정 컨트롤 형식 (또는 여러 컨트롤에 적용할 수 있는 슈퍼 클래스)에 쓰고 호환 컨트롤에만 추가 해야 합니다. 호환 되지 않는 컨트롤에 동작을 연결 하는 동안 예외가 throw 됩니다.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>스타일을 사용 하 여 Xamarin.Forms 동작 사용

동작을 명시적 또는 암시적 스타일에 의해 사용 될 수도 있습니다. 그러나 설정 하는 스타일을 만드는 합니다 [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) 속성이 읽기 전용 이므로 컨트롤의 속성 수 없는 합니다. 솔루션 추가 및 제거 동작을 제어 하는 동작 클래스에 연결된 된 속성을 추가 하는 것입니다. 프로세스는 다음과 같습니다.

1. 동작 클래스를 추가 하거나 제거할 연결 동작을는 컨트롤에 동작을 제어 하는 데 사용할에 연결된 된 속성을 추가 합니다. 연결된 된 속성 등록 확인을 `propertyChanged` 속성의 값이 변경 될 때 실행 될 대리자입니다.
1. 만들기는 `static` 연결된 된 속성에 getter 및 setter.
1. 논리를 구현 합니다 `propertyChanged` 추가 동작을 제거 하는 대리자입니다.

다음 코드 예제에서는 추가 및 제거를 제어 하는 연결된 된 속성을 `NumericValidationBehavior`:

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

합니다 `NumericValidationBehavior` 라는 연결된 된 속성을 포함 하는 클래스 `AttachBehavior` 사용 하 여는 `static` getter 및 setter를 추가 하거나 제거할은 연결 수 있는 컨트롤에 동작을 제어 하는 합니다. 이 연결 속성 레지스터는 `OnAttachBehaviorChanged` 속성의 값이 변경 될 때 실행 되는 메서드. 이 메서드를 추가 하거나 동작의 값을 기반으로 컨트롤을 제거 합니다 `AttachBehavior` 연결 된 속성입니다.

다음 코드 예제는 *명시적* 에 대 한 스타일를 `NumericValidationBehavior` 사용 하는 `AttachBehavior` 연결 된 속성 및에 적용할 수 있습니다 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤:

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

[ `Style` ](xref:Xamarin.Forms.Style) 에 적용할 수는 [ `Entry` ](xref:Xamarin.Forms.Entry) 설정 하 여 해당 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 속성을는 `Style` 인스턴스에서 사용 하 여는 `StaticResource` 다음 코드 예제 에서처럼 태그 확장:

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

스타일에 대 한 자세한 내용은 참조 하세요. [스타일](~/xamarin-forms/user-interface/styles/index.md)합니다.

> [!NOTE]
> 상태에 있는 바인딩 가능한 속성을 설정 하거나 만드는 경우 동작 하는 XAML을 쿼리 하는 동작을 추가할 수는 있지만 이러한 컨트롤 간에 공유 하지 말아야를 `Style` 에 `ResourceDictionary`합니다.

### <a name="removing-a-behavior-from-a-control"></a>컨트롤에서 동작을 제거합니다.

합니다 [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) 메서드 동작을 컨트롤에서 제거 되 고 메모리 누수를 방지 하는 이벤트에서 구독 취소와 같은 필요한 정리를 수행 하는 데 사용 되는 경우 발생 합니다. 그러나 동작 암시적으로에서 제거 되지 않습니다 제어 하지 않는 한 컨트롤의 [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) 하 여 컬렉션이 수정 되는 `Remove` 또는 `Clear` 메서드. 다음 코드 예제에서 컨트롤의 특정 동작을 제거 하는 방법을 보여 줍니다 `Behaviors` 컬렉션:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

또는 컨트롤의 [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) 다음 코드 예제에서 설명한 것 처럼 컬렉션이 지워질 수 있습니다.

```csharp
entry.Behaviors.Clear();
```

또한 페이지 탐색 스택에서 팝 되 고 때 동작 컨트롤에서 암시적으로 제거 되지 않습니다 note 합니다. 대신 명시적으로 제거 해야 페이지 범위를 벗어나기 전에 합니다.

## <a name="summary"></a>요약

이 문서를 만들고 Xamarin.Forms 동작을 사용 하는 방법을 보여 줍니다. Xamarin.Forms 동작에서 파생 하 여 생성 되는 [ `Behavior` ](xref:Xamarin.Forms.Behavior) 또는 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) 클래스입니다.


## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 동작 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [스타일 (샘플)를 사용 하 여 Xamarin.Forms 동작 적용](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [동작](xref:Xamarin.Forms.Behavior)
- [동작<T>](xref:Xamarin.Forms.Behavior`1)
