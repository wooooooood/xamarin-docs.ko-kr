---
title: 연결 된 동작
description: 연결 된 동작에는 하나 이상의 연결 된 속성을 사용 하 여 정적 클래스입니다. 이 문서를 만들고 연결 된 동작을 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 2c9bd9ad4e7572b9eae6f0073da8a2c8f1e7c9fc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995348"
---
# <a name="attached-behaviors"></a>연결 된 동작

_연결 된 동작에는 하나 이상의 연결 된 속성을 사용 하 여 정적 클래스입니다. 이 문서를 만들고 연결 된 동작을 사용 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

연결된 된 속성에 바인딩할 수 있는 속성의 특수 형식입니다. 하나의 클래스에서 정의 되지만 다른 개체에 연결 되며 XAML에서 인식할 수 있는 속성 이름과 마침표로 구분 된 클래스를 포함 하는 특성으로 합니다.

연결된 된 속성을 정의할 수는 `propertyChanged` 컨트롤에 속성이 설정 된 경우와 같은 속성의 값이 변경 하는 경우 실행 될 대리자입니다. 경우는 `propertyChanged` 대리자 실행, 속성에 대 한 이전 및 새 값을 포함 하는 매개 변수는 되 고, 연결 된 컨트롤에 대 한 참조를 전달 했습니다. 에서는 다음과 같이 전달 되는 참조를 조작 하 여 속성에 연결 된 컨트롤에 새 기능을 추가 하려면이 대리자를 사용할 수 있습니다.

1. `propertyChanged` 대리자 캐스팅으로 받은 컨트롤 참조를 [ `BindableObject` ](xref:Xamarin.Forms.BindableObject)를 동작은 컨트롤 유형은 향상 시키기 위해 설계 합니다.
1. `propertyChanged` 핵심 동작 기능을 구현 하려면 컨트롤에 의해 노출 되는 이벤트에 대 한 컨트롤 또는 레지스터 이벤트 처리기의 메서드를 호출 컨트롤의 속성을 수정 하는 대리자입니다.

연결 된 동작에 문제가 됩니다에 정의 되어 있는지를 `static` 클래스를 사용 하 여 `static` 속성 및 메서드. 이 상태는 연결 된 동작을 만들려면 어렵습니다. 또한 Xamarin.Forms 동작 동작 생성 하는 기본 방법으로 연결 된 동작을 대체 했습니다. Xamarin.Forms 동작에 대 한 자세한 내용은 참조 하세요. [Xamarin.Forms 동작](~/xamarin-forms/app-fundamentals/behaviors/creating.md) 하 고 [재사용 가능한 동작](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md)합니다.

## <a name="creating-an-attached-behavior"></a>연결 된 동작을 만들기

샘플 응용 프로그램을 보여 줍니다는 `NumericValidationBehavior`에 사용자가 입력 한 값을 강조 표시 하는 [ `Entry` ](xref:Xamarin.Forms.Entry) 있지 않으면 빨간색으로 제어를 `double`입니다. 동작은 다음 코드 예제에 표시 됩니다.

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

합니다 `NumericValidationBehavior` 라는 연결된 된 속성을 포함 하는 클래스 `AttachBehavior` 사용 하 여는 `static` getter 및 setter를 추가 하거나 제거할은 연결 수 있는 컨트롤에 동작을 제어 하는 합니다. 이 연결 속성 레지스터는 `OnAttachBehaviorChanged` 속성의 값이 변경 될 때 실행 되는 메서드. 이 메서드를 등록 또는 취소에 대 한 이벤트 처리기를 등록 합니다 [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) 의 값을 기준으로 이벤트를 `AttachBehavior` 연결 된 속성입니다. 동작의 핵심 기능을 제공 합니다 `OnEntryTextChanged` 에 입력 한 값을 구문 분석 하는 메서드를를 [ `Entry` ](xref:Xamarin.Forms.Entry) 집합과 사용자에 의해를 `TextColor` 속성 값이 아닌 경우 빨간색을를 `double`입니다.

## <a name="consuming-an-attached-behavior"></a>연결 된 동작을 사용합니다.

`NumericValidationBehavior` 클래스를 추가 하 여 사용할 수는 `AttachBehavior` 연결 된 속성을 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤을 다음 XAML 코드 예제에서 설명한 것 처럼:

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

해당 [ `Entry` ](xref:Xamarin.Forms.Entry) C#에서 다음 코드 예제에 표시 됩니다.

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

런타임에 동작 동작 구현에 따라 컨트롤과 상호 작용에 응답 합니다. 다음 스크린샷에서 잘못 된 입력에 응답 연결 된 동작을 보여 줍니다.

[![](attached-images/screenshots-sml.png "연결 된 동작을 사용 하 여 응용 프로그램 예제")](attached-images/screenshots.png#lightbox "샘플 연결 된 동작을 사용 하 여 응용 프로그램")

> [!NOTE]
> 연결 된 동작은 특정 컨트롤 형식 (또는 여러 컨트롤에 적용할 수 있는 슈퍼 클래스)에 쓰고 호환 컨트롤에만 추가 해야 합니다. 호환 되지 않는 컨트롤에 동작을 연결 하는 동안 알 수 없는 동작이 발생 및 동작 구현에 따라 달라 집니다.

### <a name="removing-an-attached-behavior-from-a-control"></a>연결 된 동작을 컨트롤에서 제거

합니다 `NumericValidationBehavior` 클래스를 설정 하 여 컨트롤에서 제거할 수 있습니다 합니다 `AttachBehavior` 연결 된 속성을 `false`다음 XAML 코드 예제에서 설명한 것 처럼:

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

해당 [ `Entry` ](xref:Xamarin.Forms.Entry) C#에서 다음 코드 예제에 표시 됩니다.

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

런타임 시는 `OnAttachBehaviorChanged` 메서드 됩니다 때 실행 값을 `AttachBehavior` 연결된 속성이로 설정 되어 `false`. 합니다 `OnAttachBehaviorChanged` 메서드는 다음 등록 취소에 대 한 이벤트 처리기는 [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) 이벤트에는 동작을 사용자 컨트롤과 상호 작용 하는 대로 실행 되지 않습니다 보장 합니다.

## <a name="summary"></a>요약

이 문서를 만들고 연결 된 동작을 사용 하는 방법을 보여 줍니다. 연결 된 동작은 `static` 하나 이상의 연결 된 속성을 사용 하 여 클래스입니다.


## <a name="related-links"></a>관련 링크

- [연결 된 동작 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
