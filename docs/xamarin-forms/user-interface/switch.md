---
title: Xamarin Forms 스위치
description: Xamarin.ios 스위치는 사용자가 설정 및 해제 상태를 전환 하기 위해 조작할 수 있는 단추 유형입니다. 이 문서에서는 Switch 클래스를 사용 하 여 토글 UI 요소를 표시 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/18/2019
ms.openlocfilehash: 88655aabdbd32db63aaf3330a18b0ad8105ea26c
ms.sourcegitcommit: b751605179bef8eee2df92cb484011a7dceb6fda
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77506533"
---
# <a name="xamarinforms-switch"></a>Xamarin Forms 스위치

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Xamarin.ios [`Switch`](xref:Xamarin.Forms.Switch) 컨트롤은 `boolean` 값으로 표시 되는 설정/해제 상태를 전환 하기 위해 사용자가 조작할 수 있는 가로 토글 단추입니다. `Switch` 클래스는 [`View`](xref:Xamarin.Forms.View)에서 상속 됩니다.

다음 스크린샷에서는 iOS 및 **Android에서 켜기 및** **끄기** 상태 설정의 `Switch` 컨트롤을 보여 줍니다.

![IOS 및 Android에서 on 및 off 상태의 스위치 스크린샷](switch-images/switch-states-default.png "IOS 및 Android의 스위치")

`Switch` 컨트롤은 다음 속성을 정의 합니다.

* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) 은 `Switch`가 **설정**되어 있는지 여부를 나타내는 `boolean` 값입니다.
* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor) 는 전환 또는 상태에서 `Switch` 렌더링 되는 **방식에 영향**을 주는 `Color`입니다.
* `ThumbColor`는 스위치 엄지의 `Color`입니다.

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, `Switch` 스타일을 지정 하 고 데이터 바인딩의 대상이 될 수 있습니다.

`Switch` 컨트롤은 사용자 조작을 통해 또는 응용 프로그램에서 `IsToggled` 속성을 설정 하는 경우 `IsToggled` 속성이 변경 될 때 발생 하는 `Toggled` 이벤트를 정의 합니다. `Toggled` 이벤트와 함께 제공 되는 `ToggledEventArgs` 개체에는 `bool`형식의 `Value`이라는 단일 속성이 있습니다. 이벤트가 발생 하면 `Value` 속성 값에 `IsToggled` 속성의 새 값이 반영 됩니다.

## <a name="create-a-switch"></a>스위치 만들기

`Switch`는 XAML에서 인스턴스화할 수 있습니다. `IsToggled` 속성을 설정 하 여 `Switch`를 전환할 수 있습니다. 기본적으로 `IsToggled` 속성은 `false`합니다. 다음 예제에서는 선택적 `IsToggled` 속성 집합을 사용 하 여 XAML에서 `Switch`를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<Switch IsToggled="true"/>
```

`Switch` 코드에서 만들 수도 있습니다.

```csharp
Switch switchControl = new Switch { IsToggled = true };
```

## <a name="switch-appearance"></a>모양 전환

[`View`](xref:Xamarin.Forms.View) 클래스에서 상속 [`Switch`](xref:Xamarin.Forms.Switch) 는 속성 외에도 `Switch` `OnColor` 및 `ThumbColor` 속성도 정의 합니다. `OnColor` 속성을 설정 하 여 **해당 상태로 전환** 될 때 `Switch` 색을 정의 하 고 `ThumbColor` 속성을 설정 하 여 스위치 엄지 단추의 `Color`를 정의할 수 있습니다. 다음 예제에서는 이러한 속성을 설정 하 여 XAML에서 `Switch`를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<Switch OnColor="Orange"
        ThumbColor="Green" />
```

코드에서 `Switch`를 만들 때 속성을 설정할 수도 있습니다.

```csharp
Switch switch = new Switch { OnColor = Color.Orange, ThumbColor = Color.Green };
```

다음 스크린샷은 `OnColor` 및 `ThumbColor` 속성이 설정 된 **설정** / **해제** 상태에서 `Switch`를 보여 줍니다.

![IOS 및 Android에서 on 및 off 상태의 스위치 스크린샷](switch-images/switch-states-colors.png "IOS 및 Android의 스위치")

## <a name="respond-to-a-switch-state-change"></a>전환 상태 변경에 응답

사용자 조작을 통해 또는 응용 프로그램에서 `IsToggled` 속성을 설정 하 여 `IsToggled` 속성이 변경 되 면 `Toggled` 이벤트가 발생 합니다. 이 이벤트에 대 한 이벤트 처리기를 등록 하 여 변경에 응답할 수 있습니다.

```xaml
<Switch Toggled="OnToggled" />
```

코드 숨겨진 파일에는 `Toggled` 이벤트에 대 한 처리기가 포함 되어 있습니다.

```csharp
void OnToggled(object sender, ToggledEventArgs e)
{
    // Perform an action after examining e.Value
}
```

이벤트 처리기의 `sender` 인수는이 이벤트의 발생을 담당 하는 `Switch`입니다. `sender` 속성을 사용 하 여 `Switch` 개체에 액세스 하거나 동일한 `Toggled` 이벤트 처리기를 공유 하는 여러 `Switch` 개체를 구분할 수 있습니다.

`Toggled` 이벤트 처리기는 코드에서 할당 될 수도 있습니다.

```csharp
Switch switchControl = new Switch {...};
switchControl.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>스위치 데이터 바인딩

데이터 바인딩 및 트리거를 사용 하 여 `Toggled` 이벤트 처리기를 제거 하 여 `Switch` 변경 설정/해제 상태에 응답할 수 있습니다.

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

이 예제에서 [`Label`](xref:Xamarin.Forms.Label) 은 `DataTrigger`의 바인딩 식을 사용 하 여 `styleSwitch`이라는 `Switch`의 `IsToggled` 속성을 모니터링 합니다. 이 속성이 `true`되 면 `Label`의 `FontAttributes` 및 `FontSize` 속성이 변경 됩니다. `IsToggled` 속성이 `false`로 반환 되는 경우 `Label`의 `FontAttributes` 및 `FontSize` 속성은 초기 상태로 다시 설정 됩니다.

트리거에 대 한 자세한 내용은 [Xamarin.ios 트리거](~/xamarin-forms/app-fundamentals/triggers.md)를 참조 하세요.

## <a name="disable-a-switch"></a>스위치 사용 안 함

응용 프로그램은 설정/해제 되는 `Switch` 유효한 작업이 아닌 상태를 입력할 수 있습니다. 이러한 경우 `IsEnabled` 속성을 `false`로 설정 하 여 `Switch`를 사용 하지 않도록 설정할 수 있습니다. 이렇게 하면 사용자가 `Switch`을 조작할 수 없게 됩니다.

## <a name="related-links"></a>관련 링크

* [데모 전환](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
* [Xamarin Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)
