---
title: "연결 된 동작"
description: "연결 된 동작에는 하나 이상의 연결 된 속성을 가진 정적 클래스입니다. 이 문서를 만들고 연결 된 동작을 사용 하는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 84e60e8ce698e3d87db3e1bdc61613325ad831c8
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="attached-behaviors"></a>연결 된 동작

_연결 된 동작에는 하나 이상의 연결 된 속성을 가진 정적 클래스입니다. 이 문서를 만들고 연결 된 동작을 사용 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

연결된 된 속성은 특수 한 유형의 바인딩 가능한 속성입니다. 하나의 클래스에 정의 되어 있 다른 개체에 연결 하 고 XAML에서 인식할 수 있는 속성 이름과 마침표로 구분 되는 클래스를 포함 하는 특성으로.

연결된 된 속성을 정의할 수는 `propertyChanged` 컨트롤에 속성이 설정 된 경우 등, 속성의 값이 변경 될 때 실행 될 대리자입니다. 경우는 `propertyChanged` 대리자를 실행, 기반이 되 고, 연결 된 컨트롤 및 속성에 대 한 이전 및 새 값을 포함 하는 매개 변수 참조를 전달 했습니다. 속성을 다음과 같이 전달 되는 참조를 조작 하 여에 연결 된 컨트롤에 새 기능을 추가 하려면이 대리자를 사용할 수 있습니다.

1. `propertyChanged` 대리자 캐스팅으로 수신 하는 컨트롤 참조는 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)을 동작은 컨트롤 형식을 강화 하기 위해 설계 되었습니다.
1. `propertyChanged` 코어 동작 기능을 구현 하는 컨트롤에 의해 노출 되는 이벤트에 대 한 컨트롤 또는 레지스터 이벤트 처리기의 메서드를 호출 컨트롤의 속성을 수정 하는 대리자입니다.

연결 된 동작을 가진 문제는에 정의 되어 있는지 한 `static` 클래스와 `static` 속성 및 메서드. 따라서 어려운 된 상태에 있는 연결 된 동작을 만들 수 있습니다. 또한 Xamarin.Forms 동작 동작 생성에 대 한 기본 접근 방법으로 연결 된 동작을 대체 했습니다. Xamarin.Forms 동작에 대 한 자세한 내용은 참조 [Xamarin.Forms 동작](~/xamarin-forms/app-fundamentals/behaviors/creating.md) 및 [다시 사용할 수 있는 동작](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md)합니다.

## <a name="creating-an-attached-behavior"></a>연결 된 동작 만들기

샘플 응용 프로그램을 보여 줍니다.는 `NumericValidationBehavior`에 사용자가 입력 한 값을 강조 표시 하는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 없으면 빨간색으로 제어는 `double`합니다. 다음 코드 예제에는 동작이 표시 됩니다.

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

`NumericValidationBehavior` 라는 연결된 된 속성을 포함 하는 클래스 `AttachBehavior` 와 `static` getter 및 setter를 추가 또는 제거 하는 연결 수 있는 컨트롤에 대 한 동작을 제어 하는 합니다. 이 연결 속성 레지스터는 `OnAttachBehaviorChanged` 속성의 값이 변경 될 때 실행 되는 메서드. 이 메서드를 등록 하거나에 대 한 이벤트 처리기를 등록 취소는 [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) 의 값에 따른 이벤트는 `AttachBehavior` 연결 된 속성입니다. 동작의 핵심 기능을 제공는 `OnEntryTextChanged` 에 입력 한 값을 구문 분석 하는 메서드는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 사용자 및 설정으로는 `TextColor` 속성 값이 없는 경우에 빨강을는 `double`합니다.

## <a name="consuming-an-attached-behavior"></a>연결 된 동작 사용

`NumericValidationBehavior` 클래스를 추가 하 여 사용할 수는 `AttachBehavior` 연결 된 속성을는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 다음 XAML 코드 예제에서와 같이 제어:

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

에 해당 하는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) C#에서 다음 코드 예제에 표시 됩니다.

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

런타임에 동작 구현에 따라 동작 컨트롤과 상호 작용에 응답 합니다. 다음 스크린샷에서 잘못 된 입력에 응답 하는 연결 된 동작을 보여 줍니다.

[![](attached-images/screenshots-sml.png "샘플 응용 프로그램 연결 된 동작으로")](attached-images/screenshots.png#lightbox "샘플 연결 된 동작으로 응용 프로그램")

> [!NOTE]
> 특정 컨트롤 형식 (또는 많은 컨트롤에 적용할 수 있는 슈퍼 클래스)에 연결 된 동작으로 작성 하 고 호환 되는 컨트롤에만 추가 해야 합니다. 알 수 없는 동작이 발생 하 고 동작 구현에 따라는 호환 되지 않는 컨트롤에 대 한 동작을 연결 하려고 합니다.

### <a name="removing-an-attached-behavior-from-a-control"></a>컨트롤에서 연결 된 동작을 제거합니다.

`NumericValidationBehavior` 클래스를 설정 하 여 컨트롤에서 제거할 수는 `AttachBehavior` 연결 된 속성을 `false`다음 XAML 코드 예제에서와 같이,:

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

에 해당 하는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) C#에서 다음 코드 예제에 표시 됩니다.

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

런타임에 `OnAttachBehaviorChanged` 메서드 됩니다 때 실행의 값은 `AttachBehavior` 연결된 속성이로 설정 된 `false`합니다. `OnAttachBehaviorChanged` 메서드는 취소에 대 한 이벤트 처리기를 등록 한 다음는 [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) 이벤트를 사용자가 컨트롤과 상호 작용 하는 동작 실행 되지 않습니다.

## <a name="summary"></a>요약

이 문서를 만들고 연결 된 동작을 사용 하는 방법을 보여 줍니다. 연결 된 동작은 `static` 하나 이상의 연결 된 속성을 사용 하 여 클래스입니다.


## <a name="related-links"></a>관련 링크

- [연결 된 동작 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
