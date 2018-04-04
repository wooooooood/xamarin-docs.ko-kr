---
title: Xamarin.Forms Behaviors
description: 동작 또는 동작에서 파생 하 여 Xamarin.Forms 동작 만들어집니다<T> 클래스입니다. 이 문서를 만들고 Xamarin.Forms 동작을 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 2848b554d2dbd6d3d69ae864846247b3612d64e6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms Behaviors

_동작 또는 동작에서 파생 하 여 Xamarin.Forms 동작 만들어집니다<T> 클래스입니다. 이 문서를 만들고 Xamarin.Forms 동작을 사용 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

Xamarin.Forms 동작을 만들기 위한 프로세스는 다음과 같습니다.

1. 상속 되는 클래스를 만듭니다는 [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) 또는 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) 클래스, 여기서 `T` 동작 적용 되어야 하는 컨트롤의 형식입니다.
1. 재정의 [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) 메서드를 필요한 설정을 수행 합니다.
1. 재정의 [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) 메서드를 필요한 정리를 수행 합니다.
1. 동작의 핵심 기능을 구현 합니다.

그 결과 다음 코드 예제에 나와 있는 구조가 같습니다.

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

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) 바로 동작이 연결 되는 컨트롤에 다음 메서드가 발생 합니다. 이 메서드를 연결 된 하 고 이벤트 처리기를 등록 하거나 동작 기능을 지 원하는 데 필요한 다른 설정을 수행 하는 데 사용 될 컨트롤에 대 한 참조를 받습니다. 예를 들어 컨트롤에 대 한 이벤트를 구독할 수 있습니다. 동작 기능은 이벤트에 대 한 이벤트 처리기에서 구현 합니다.

[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) 메서드는 동작이 컨트롤에서 제거 될 때 발생 합니다. 이 메서드는 있는 연결 된 하 고 필요한 정리를 수행 하는 데 사용 되는 컨트롤에 대 한 참조를 받습니다. 예를 들어 메모리 누수를 방지 하기 위해 컨트롤 이벤트에서 지 수신을 해지할 수 없습니다.

다음 동작에 연결 하 여 사용할 수는 [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) 적절 한 컨트롤의 컬렉션입니다.

## <a name="creating-a-xamarinforms-behavior"></a>Xamarin.Forms 동작 만들기

샘플 응용 프로그램을 보여 줍니다.는 `NumericValidationBehavior`에 사용자가 입력 한 값을 강조 표시 하는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 없으면 빨간색으로 제어는 `double`합니다. 다음 코드 예제에는 동작이 표시 됩니다.

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

`NumericValidationBehavior` 에서 파생 되는 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) 클래스, 여기서 `T` 는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)합니다. [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) 에 대 한 이벤트 처리기를 등록 하는 메서드는 [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) 이벤트의 경우와 [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) 메서드는 를등록취소`TextChanged`이벤트 메모리 누수 합니다. 동작의 핵심 기능을 제공는 `OnEntryTextChanged` 에 사용자가 입력 한 값을 구문 분석 하는 메서드는 `Entry`, 설정 및는 [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) 속성 값이 없는 경우에 빨강을는 `double`합니다.

> [!NOTE]
> Xamarin.Forms로 설정 하지는 `BindingContext` 은 동작의 동작을 공유 하 고 스타일을 통해 여러 컨트롤에 적용 될 수 있으므로 합니다.

## <a name="consuming-a-xamarinforms-behavior"></a>Xamarin.Forms 동작 사용

Xamarin.Forms는 모든 컨트롤에는 [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) 하나 이상의 동작 수 추가할 수 있는, 다음 XAML 코드 예제에서와 같이 컬렉션:

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

에 해당 하는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) C#에서 다음 코드 예제에 표시 됩니다.

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

런타임에 동작 구현에 따라 동작 컨트롤과 상호 작용에 응답 합니다. 다음 스크린샷에서 잘못 된 입력에 응답 동작을 보여 줍니다.

[![](creating-images/screenshots-sml.png "샘플 Xamarin.Forms 동작으로 응용 프로그램")](creating-images/screenshots.png#lightbox "샘플 Xamarin.Forms 동작으로 응용 프로그램")

> [!NOTE]
> 특정 컨트롤 형식 (또는 많은 컨트롤에 적용할 수 있는 슈퍼 클래스)에 동작으로 작성 하 고 호환 되는 컨트롤에만 추가 해야 합니다. 호환 되지 않는 컨트롤에 대 한 동작을 연결 하려고 하면 예외가 throw 됩니다.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>Xamarin.Forms 동작 스타일을 사용합니다.

동작 명시적 또는 암시적 스타일으로도 사용할 수 있습니다. 그러나 설정 하는 스타일을 만드는 [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) 속성이 읽기 전용 이므로 컨트롤의 속성 수 없으면입니다. 솔루션 추가 및 제거 동작을 제어 하는 동작 클래스에 연결된 된 속성을 추가 하는 것입니다. 프로세스는 다음과 같습니다.

1. 연결된 된 속성을 추가 하거나 제거할 연결 동작이 됩니다는 컨트롤에 대 한 동작을 제어 하는 동작 클래스에 추가 합니다. 연결 된 속성의 등록을 확인 한 `propertyChanged` 속성의 값이 변경 될 때 실행 될 대리자입니다.
1. 만들기는 `static` getter 및 setter 연결 된 속성에 대 한 합니다.
1. 논리를 구현는 `propertyChanged` 대리자를 추가 하 고 동작을 제거 합니다.

다음 코드 예제에서는 추가 및 제거를 제어 하는 연결된 된 속성의 `NumericValidationBehavior`:

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

`NumericValidationBehavior` 라는 연결된 된 속성을 포함 하는 클래스 `AttachBehavior` 와 `static` getter 및 setter를 추가 또는 제거 하는 연결 수 있는 컨트롤에 대 한 동작을 제어 하는 합니다. 이 연결 속성 레지스터는 `OnAttachBehaviorChanged` 속성의 값이 변경 될 때 실행 되는 메서드. 이 메서드를 추가 하거나 동작의 값에 따라 컨트롤을 제거는 `AttachBehavior` 연결 된 속성입니다.

다음 코드 예제는 *명시적* 스타일 지정에 대 한는 `NumericValidationBehavior` 를 사용 하는 `AttachBehavior` 연결 된 속성에 적용할 수 있습니다 및 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 컨트롤:

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 에 적용할 수는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 설정 하 여 컨트롤의 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성을는 `Style` 인스턴스에서 사용 하는 `StaticResource` 태그 확장, 다음 코드 예제에서와 같이:

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

스타일에 대 한 자세한 내용은 참조 [스타일](~/xamarin-forms/user-interface/styles/index.md)합니다.

> [!NOTE]
> 상태에 있는 바인딩 가능한 속성을 설정 하거나 만들면 동작 하는 경우 XAML에서 쿼리 하는 동작을 추가할 수는 있지만 이러한에서 컨트롤 사이 공유 하지 않아야는 `Style` 에 `ResourceDictionary`합니다.

### <a name="removing-a-behavior-from-a-control"></a>컨트롤에서 동작을 제거합니다.

[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) 메서드 동작을 컨트롤에서 제거 되 고 메모리 누수를 방지 하기 위해 이벤트를 구독 취소 하는 등 필요한 정리를 수행 하는 데 사용 되는 경우 발생 합니다. 그러나 동작은 암시적으로 제거 되지 컨트롤에서 하지 않는 한 컨트롤의 [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) 컬렉션에 의해 수정 됩니다는 `Remove` 또는 `Clear` 메서드. 다음 코드 예제에서는 컨트롤의에서 특정 동작을 제거 하는 방법을 보여 줍니다 `Behaviors` 컬렉션:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

또는 컨트롤의 [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) 다음 코드 예제에서와 같이 컬렉션을 지울 수 있습니다.

```csharp
entry.Behaviors.Clear();
```

또한 페이지는 탐색 스택에서 팝 하는 경우 동작 컨트롤에서 암시적으로 제거 되지 않습니다 note 합니다. 대신, 명시적으로 제거 해야 페이지 범위를 벗어난 이전 합니다.

## <a name="summary"></a>요약

이 문서를 만들고 Xamarin.Forms 동작을 사용 하는 방법을 보여 줍니다. Xamarin.Forms 동작에서 파생 하 여 생성 됩니다는 [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) 또는 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) 클래스입니다.


## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 동작 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [Xamarin.Forms 동작이 적용 된 스타일 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [동작](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [동작<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
