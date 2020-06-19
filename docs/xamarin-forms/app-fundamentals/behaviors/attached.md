---
title: 연결된 동작
description: 연결된 동작은 연결된 속성이 하나 이상 있는 정적 클래스입니다. 이 문서에서는 연결된 동작을 만들고 사용하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 08b1c738dc87bb9373436a3fd96486eb15341112
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139427"
---
# <a name="attached-behaviors"></a>연결된 동작

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-attachednumericvalidationbehavior)

_연결된 동작은 연결된 속성이 하나 이상 있는 정적 클래스입니다. 이 문서에서는 연결된 동작을 만들고 사용하는 방법을 설명합니다._

## <a name="overview"></a>개요

연결된 속성은 특수한 형식의 바인딩 가능 속성입니다. 이는 한 클래스에서 정의되지만 다른 개체에 연결되어 있으며, XAML에서 마침표로 구분된 클래스 및 속성 이름을 포함하는 특성으로 인식할 수 있습니다.

연결된 속성은 컨트롤에 속성이 설정된 경우와 같이 속성 값이 변경되면 실행될 `propertyChanged` 대리자를 정의할 수 있습니다. `propertyChanged` 대리자가 실행되면 이를 연결할 컨트롤에 대한 참조 및 속성에 대한 이전 및 새 값을 포함하는 매개 변수가 전달됩니다. 다음과 같이 전달된 참조를 조작하여 속성이 연결된 컨트롤에 새 기능을 추가하는 데 이 대리자를 사용할 수 있습니다.

1. `propertyChanged` 대리자는 [`BindableObject`](xref:Xamarin.Forms.BindableObject)로 수신된 컨트롤 참조를 성능 향상을 위해 동작이 설계된 컨트롤 형식으로 캐스트합니다.
1. `propertyChanged` 대리자는 핵심 동작 기능을 구현하기 위해 컨트롤의 속성을 수정하거나, 컨트롤의 메서드를 호출하거나, 컨트롤에 의해 노출된 이벤트에 이벤트 처리기를 등록합니다.

연결된 동작과 관련된 문제는 `static` 속성 및 메서드를 통해 `static` 클래스에 정의됩니다. 이 때문에 상태가 있는 연결된 동작을 만들기가 어려워집니다. 또한 Xamarin.Forms 동작은 동작 생성을 위한 기본 접근 방식으로 연결된 동작을 대체하였습니다. Xamarin.Forms 동작에 대한 자세한 내용은 [Xamarin.Forms 동작](~/xamarin-forms/app-fundamentals/behaviors/creating.md) 및 [재사용 가능한 동작](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md)을 참조하세요.

## <a name="creating-an-attached-behavior"></a>연결된 동작 만들기

샘플 애플리케이션은 사용자가 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤에 입력한 값이 `double`이 아닌 경우 빨간색으로 강조 표시하는 `NumericValidationBehavior`를 설명합니다. 동작이 다음 코드 예제에 나와 있습니다.

```csharp
public static class NumericValidationBehavior
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached (
            "AttachBehavior",
            typeof(bool),
            typeof(NumericValidationBehavior),
            false,
            propertyChanged:OnAttachBehaviorChanged);

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
            entry.TextChanged += OnEntryTextChanged;
        } else {
            entry.TextChanged -= OnEntryTextChanged;
        }
    }

    static void OnEntryTextChanged (object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

`NumericValidationBehavior` 클래스에는 `static` getter 및 setter와 함께 `AttachBehavior`라는 연결된 속성이 포함되어 있습니다. 이는 클래스가 연결될 컨트롤에 동작의 추가 또는 제거를 제어합니다. 이 연결된 속성은 속성 값이 변경될 때 실행할 `OnAttachBehaviorChanged` 메서드를 등록합니다. 이 메서드는 `AttachBehavior` 연결된 속성의 값에 따라 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 이벤트에 대한 이벤트 처리기를 등록 또는 등록 해제합니다. 동작의 핵심 기능은 `OnEntryTextChanged` 메서드에서 제공됩니다. 이는 사용자가 [`Entry`](xref:Xamarin.Forms.Entry)에 입력한 값을 구문 분석하여 값이 `double`이 아닌 경우 빨간색으로 표시하도록 `TextColor` 속성을 설정합니다.

## <a name="consuming-an-attached-behavior"></a>연결된 동작 사용

다음과 같은 XAML 코드 예제에 설명된 대로 `AttachBehavior` 연결된 속성을 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤에 추가하면 `NumericValidationBehavior`를 사용할 수 있습니다.

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

해당하는 C#의 [`Entry`](xref:Xamarin.Forms.Entry)가 다음 코드 예제에 나와 있습니다.

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

런타임 시 동작은 동작 구현에 따라 응답하여 컨트롤과 상호 작용합니다. 다음 스크린샷에서는 잘못된 입력에 응답하는 연결된 동작을 설명합니다.

[![](attached-images/screenshots-sml.png "Sample Application with Attached Behavior")](attached-images/screenshots.png#lightbox "Sample Application with Attached Behavior")

> [!NOTE]
> 연결된 동작은 특정 컨트롤 형식(또는 여러 컨트롤에 적용할 수 있는 슈퍼클래스)에 작성되며 호환 컨트롤에만 추가해야 합니다. 호환되지 않는 컨트롤에 동작을 연결하려고 하면 알 수 없는 동작이 발생하고 동작 구현에 따라 결과가 달라집니다.

### <a name="removing-an-attached-behavior-from-a-control"></a>컨트롤에서 연결된 동작 제거

다음과 같은 XAML 코드 예제에 설명된 대로 `AttachBehavior` 연결된 속성을 `false`로 설정하면 컨트롤에서 `NumericValidationBehavior` 클래스를 제거할 수 있습니다.

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

C#의 해당하는 [`Entry`](xref:Xamarin.Forms.Entry)가 다음 코드 예제에 나와 있습니다.

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

런타임 시 `AttachBehavior` 연결된 속성의 값이 `false`로 설정된 경우 `OnAttachBehaviorChanged` 메서드가 실행됩니다. 그런 다음, `OnAttachBehaviorChanged` 메서드는 사용자가 컨트롤과 상호 작용할 때 동작이 실행되지 않도록 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 이벤트에 대한 이벤트 처리기를 등록 해제합니다.

## <a name="summary"></a>요약

이 문서에서는 연결된 동작을 만들고 사용하는 방법을 설명했습니다. 연결된 동작은 연결된 속성이 하나 이상 있는 `static` 클래스입니다.

## <a name="related-links"></a>관련 링크

- [연결된 동작(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-attachednumericvalidationbehavior)
