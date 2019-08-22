---
title: Xamarin Forms 스위치
description: Xamarin.ios 스위치는 사용자가 설정 및 해제 상태를 전환 하기 위해 조작할 수 있는 단추 유형입니다. 이 문서에서는 Switch 클래스를 사용 하 여 토글 UI 요소를 표시 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/18/2019
ms.openlocfilehash: 1f2ef838287e32df5df42f73e4b43816d618552d
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887876"
---
# <a name="xamarinforms-switch"></a>Xamarin Forms 스위치

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Xamarin.ios [`Switch`](xref:Xamarin.Forms.Switch) 컨트롤은 사용자가 `boolean` 값으로 표시 되는 설정/해제 상태를 전환 하기 위해 조작할 수 있는 가로 토글 단추입니다. 클래스 `Switch` 는에서 [`View`](xref:Xamarin.Forms.View)상속 됩니다.

다음 스크린샷에서는 iOS 및 `Switch` Android의 설정 / **해제** 상태에 있는 컨트롤을 보여 줍니다.

![IOS 및 Android에서 on 및 off 상태의 스위치 스크린샷](switch-images/switch-states-default.png "IOS 및 Android의 스위치")

컨트롤 `Switch` 은 다음 두 가지 속성을 정의 합니다.

* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)가 설정 `Switch` 되어 있는지 여부를 나타내는 값입니다.`boolean`
* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor)가 전환 또는 상태에서 렌더링 `Switch` 되는 방식에 영향을 주는입니다.`Color`
* `ThumbColor`는 스위치 엄지의입니다. `Color`

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에 의해 지원 됩니다. 즉, `Switch` 가 스타일을 지정 하 고 데이터 바인딩의 대상이 될 수 있습니다.

컨트롤 `Switch` `Toggled` 은 사용자조작이`IsToggled` 나 응용 프로그램에서 속성을설정하여속성이변경될때발생하는이벤트를정의합니다.`IsToggled` 이벤트와 `Toggled` 함께 제공 되는 `bool` `Value` `ToggledEventArgs` 개체에는 형식의 이라는 단일 속성이 있습니다. 이벤트가 발생 하면 속성의 값 `Value` 은 `IsToggled` 속성의 새 값을 반영 합니다.

## <a name="create-a-switch"></a>스위치 만들기

는 `Switch` XAML에서 인스턴스화될 수 있습니다. 속성을 설정 하 여를 `Switch`토글할 수 있습니다. `IsToggled` 기본적으로 `IsToggled` 속성은 `false`합니다. 다음 예제에서는 선택적 `Switch` `IsToggled` 속성 집합을 사용 하 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<Switch IsToggled="true"/>
```

코드 `Switch` 에서를 만들 수도 있습니다.

```csharp
Switch switchControl = new Switch { IsToggled = true };
```

## <a name="switch-appearance"></a>모양 전환

[`Switch`](xref:Xamarin.Forms.Switch) `ThumbColor` `OnColor` 클래스`Switch`에서상속 되는 속성 외에도 및 속성을 정의 합니다. [`View`](xref:Xamarin.Forms.View) `Color` `ThumbColor` `Switch` 속성을 설정 하 여 해당 상태로 전환 될 때 색을 정의 하 고, 속성을 설정 하 여 스위치 엄지의를 정의할 수 있습니다. `OnColor` 다음 예제에서는 이러한 속성을 설정 하 `Switch` 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<Switch OnColor="Orange"
        ThumbColor="Green" />
```

코드에서를 `Switch` 만들 때 속성을 설정할 수도 있습니다.

```csharp
Switch switch = new Switch { OnColor = Color.Orange, ThumbColor = Color.Green };
```

다음 스크린샷은 `Switch` `OnColor` 및 속성이`ThumbColor` 설정 된 on 및 **off** 토글 상태의을 보여 줍니다.

![IOS 및 Android에서 on 및 off 상태의 스위치 스크린샷](switch-images/switch-states-colors.png "IOS 및 Android의 스위치")

## <a name="respond-to-a-switch-state-change"></a>전환 상태 변경에 응답

속성이 변경 되 면 사용자 조작을 통해 또는 응용 프로그램에서 `IsToggled` 속성을 설정 하 `Toggled` 는 경우 이벤트가 발생 합니다. `IsToggled` 이 이벤트에 대 한 이벤트 처리기를 등록 하 여 변경에 응답할 수 있습니다.

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

이벤트 처리기의 `Switch` 인수는이이벤트의발생`sender` 을 담당 합니다. `sender` 속성을 사용 하 여 `Switch` 개체에 액세스 하거나 동일한 `Toggled` 이벤트 처리기를 공유 하 `Switch` 는 여러 개체를 구분할 수 있습니다.

이벤트 `Toggled` 처리기는 코드에서 할당 될 수도 있습니다.

```csharp
Switch switchControl = new Switch {...};
switchControl.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>스위치 데이터 바인딩

데이터 바인딩 및 트리거를 사용 하 여 변경 된 `Switch` 토글 상태에 응답 하 여 이벤트처리기를제거할수있습니다.`Toggled`

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

이 예제 [`Label`](xref:Xamarin.Forms.Label) 에서는의 바인딩 식을 `DataTrigger` 사용 하 여 `Switch` 명명 `styleSwitch`된의 `IsToggled` 속성을 모니터링 합니다. 이 속성이 `true` `FontSize` 이면의 `FontAttributes` 및`Label` 속성이 변경 됩니다. 속성이로`false` 반환되`FontSize` 면의 및속성이초기상태로다시설정됩니다.`Label` `FontAttributes` `IsToggled`

트리거에 대 한 자세한 내용은 [Xamarin.ios 트리거](~/xamarin-forms/app-fundamentals/triggers.md)를 참조 하세요.

## <a name="disable-a-switch"></a>스위치 사용 안 함

응용 프로그램은 설정/해제가 유효한 `Switch` 작업이 아닌 상태로 전환 될 수 있습니다. 이러한 경우에는 `Switch` `IsEnabled` 속성을로 `false`설정 하 여를 비활성화할 수 있습니다. 이렇게 하면 사용자가를 `Switch`조작할 수 없게 됩니다.

## <a name="related-links"></a>관련 링크

* [데모 전환](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
* [Xamarin Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)
