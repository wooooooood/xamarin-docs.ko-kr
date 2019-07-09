---
title: Xamarin.Forms Switch
description: Xamarin.Forms 전환은 켜고 상태 사이 전환 하려면 사용자가 조작할 수 있는 단추 형식입니다. 이 문서에서는 스위치 클래스를 사용 하 여 토글 UI 요소를 표시 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/03/2019
ms.openlocfilehash: ed2d41ea2d9add658d9f07469568a298cdf8de59
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649696"
---
# <a name="xamarinforms-switch"></a>Xamarin.Forms Switch

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SwitchDemos)

Xamarin.Forms [ `Switch` ](xref:Xamarin.Forms.Switch) 사이 전환 하려면 및 상태를 해제 하 여 표현 되는 사용자가 조작할 수 있는 가로 설정/해제 단추가 `boolean` 값입니다. 합니다 `Switch` 클래스에서 상속 [ `View` ](xref:Xamarin.Forms.View)합니다.

다음 스크린 샷에 표시를 `Switch` 컨트롤에 해당 **에** 및 **해제** iOS 및 Android에서 상태를 설정/해제:

![스크린 샷의 스위치에서 iOS 및 Android에서 상태를 켜고](switch-images/switch-states-default.png "iOS 및 Android에서 전환")

`Switch` 컨트롤 두 속성을 정의 합니다.

* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor) `Color` 에 영향을 주는 하는 방법을 `Switch` 가 전환 된 렌더링 또는 **에**, 상태입니다.
* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) `boolean` 나타내는 값 여부를를 `Switch` 는 **에서**합니다.

이러한 속성에 의해 지원 됩니다는 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 개체를는 `Switch` 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

`Switch` 컨트롤을 정의 `Toggled` 때 발생 하는 이벤트를 `IsToggled` 사용자 조작을 통해 또는 응용 프로그램 설정 속성 변경의 `IsToggled` 속성. 합니다 `ToggledEventArgs` 와 함께 제공 되는 개체를 `Toggled` 이벤트 라는 단일 속성을 가진 `Value`, 형식의 `bool`합니다. 경우 이벤트가의 값을 `Value` 속성의 새 값을 반영 합니다 `IsToggled` 속성입니다.

## <a name="create-a-switch"></a>스위치 만들기

`Switch` XAML에서 인스턴스화할 수 있습니다. 해당 `IsToggled` 속성을 설정/해제를 설정할 수 있습니다는 `Switch`합니다. 기본적으로 `IsToggled` 속성은 `false`합니다. 다음 예제에서는 인스턴스화하는 방법을 보여 줍니다는 `Switch` 선택적 XAML에서 `IsToggled` 속성 집합:

```xaml
<Switch IsToggled="true"/>
```

' 코드에서 스위치를 만들 수도 있습니다.

```csharp
Switch switch = new Switch { IsToggled = true };
```

### <a name="switch-style-properties"></a>스위치 스타일 속성

`OnColor` 정의에 속성을 설정할 수 있습니다 합니다 `Switch` 색으로 전환 되는 경우 해당 **에서** 상태. 다음 예제에서는 인스턴스화하는 방법을 보여 줍니다는 `Switch` 사용 하 여 XAML에서는 `OnColor` 속성 집합:

```xaml
<Switch OnColor="Orange" />
```

합니다 `OnColor` 속성을 만들 때 설정할 수도 있습니다는 `Switch` 코드에서:

```csharp
Switch switch = new Switch { OnColor = Color.Orange };
```

다음 스크린 샷에 표시 된 `Switch` 에서 해당 **에** 및 **해제** 설정/해제 상태를 사용 하 여를 `OnColor` 속성이로 설정 `Color.Orange` iOS 및 Android에서:

![스크린 샷의 스위치에서 iOS 및 Android에서 상태를 켜고](switch-images/switch-states-oncolor.png "iOS 및 Android에서 전환")

## <a name="respond-to-a-switch-state-change"></a>스위치 상태 변경에 응답

경우는 `IsToggled` 사용자 조작을 통해 또는 응용 프로그램 설정 속성 변경을 `IsToggled` 속성인을 `Toggled` 이벤트가 발생 합니다. 변경 내용에 응답 하도록이 이벤트에 대 한 이벤트 처리기를 등록할 수 있습니다.

```xaml
<Switch Toggled="OnToggled" />
```

코드 숨김 파일에 대 한 처리기가 포함 된 `Toggled` 이벤트:

```csharp
void OnToggled(object sender, ToggledEventArgs e)
{
    // Perform an action after examining e.Value
}
```

합니다 `sender` 이벤트 처리기에서 인수가 `Switch` 이 이벤트를 발생 하는 일을 담당 합니다. 사용할 수는 `sender` 속성에 액세스 합니다 `Switch` 개체 또는 여러 구분 하기 위해 `Switch` 공유 하는 동일한 개체 `Toggled` 이벤트 처리기.

`Toggled` 코드에서 이벤트 처리기를 할당할 수도 있습니다.

```csharp
Switch switch = new Switch {...};
switch.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>데이터 바인딩 스위치

합니다 `Toggled` 응답할 데이터 바인딩 및 트리거를 사용 하 여 이벤트 처리기를 제거할 수 있습니다는 `Switch` 설정/해제 상태를 변경 합니다.

```xaml
<Switch x:Name="styleSwitch" />
<Label Text="Lorem ipsum dolor sit amet, elit rutrum, enim hendrerit augue vitae praesent sed non, lorem aenean quis praesent pede.">
    <Label.Triggers>
        <DataTrigger TargetType="Label"
                     Binding="{Binding Source={x:Reference styleSwitch}, Path=IsToggled}"
                     Value="true">
            <Setter Property="FontAttributes"
                    Value="Italic, Bold" />
            <Setter Property="FontSize"
                    Value="Large" />
        </DataTrigger>
    </Label.Triggers>
</Label>
```

이 예제에서는 [ `Label` ](xref:Xamarin.Forms.Label) 바인딩 식에서 사용 하 여를 `DataTrigger` 모니터링 하는 `IsToggled` 속성을 `Switch` 라는 `styleSwitch`합니다. 경우이 속성은 `true`는 `FontAttributes` 및 `FontSize` 의 속성을 `Label` 변경 됩니다. 경우는 `IsToggled` 속성에 반환 `false`, `FontAttributes` 및 `FontSize` 속성의는 `Label` 를 초기 상태로 다시 설정 됩니다.

트리거에 대 한 정보를 참조 하세요 [Xamarin.Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)합니다.

## <a name="disable-a-switch"></a>스위치를 사용 하지 않도록 설정

응용 프로그램 상태를 입력할 수 있는 여 `Switch` 설정/해제 대상인 것은 올바른 작업이 아닙니다. 이러한 경우에는 `Switch` 설정 하 여 비활성화할 수 있습니다 해당 `IsEnabled` 속성을 `false`입니다. 이 설정 하면 사용자가 조작할 수 없게 된 `Switch`합니다.

## <a name="related-links"></a>관련 링크

* [데모를 전환 합니다.](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SwitchDemos)
* [Xamarin.Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)