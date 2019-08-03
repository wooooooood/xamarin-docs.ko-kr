---
title: Xamarin.ios 확인란
description: Xamarin.ios 확인란은 선택 하거나 비워 둘 수 있는 단추의 유형입니다. 확인란을 선택 하면 설정 된 것으로 간주 됩니다. 확인란이 비어 있으면 해제 된 것으로 간주 됩니다.
ms.prod: xamarin
ms.assetid: B8B9268B-BCB8-42B9-B08C-C0F22C137238
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: f78ca9d2cf7a9e57b81c5d923c64b36a7982c4b0
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739150"
---
# <a name="xamarinforms-checkbox"></a>Xamarin.ios 확인란

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

Xamarin.ios는 선택 하거나 `CheckBox` 비워 둘 수 있는 단추의 유형입니다. 확인란을 선택 하면 설정 된 것으로 간주 됩니다. 확인란이 비어 있으면 해제 된 것으로 간주 됩니다.

`CheckBox`이 선택 `bool` 되었는지 여부 `IsChecked`를 나타내는 라는 속성을 정의 합니다. `CheckBox` 이 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에 의해 지원 됩니다. 즉, 스타일을 지정할 수 있으며 데이터 바인딩의 대상이 될 수 있습니다.

> [!NOTE]
> 바인딩 `IsChecked` 가능한 속성은의 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)기본 바인딩 모드입니다.

`CheckBox`사용자 조작을 `CheckedChanged` 통해 또는 응용 프로그램이 `IsChecked` 속성을 `IsChecked` 설정 하는 경우 속성이 변경 될 때 발생 하는 이벤트를 정의 합니다. 이벤트와 `CheckedChanged` 함께 제공 되는 `bool` `Value` `CheckedChangedEventArgs` 개체에는 형식의 이라는 단일 속성이 있습니다. 이벤트가 발생 하면 `Value` 속성의 값이 `IsChecked` 속성의 새 값으로 설정 됩니다.

## <a name="create-a-checkbox"></a>확인란 만들기

다음 예제에서는 인스턴스화하는 방법을 보여 줍니다는 `CheckBox` XAML에서:

```xaml
<CheckBox />
```

이 XAML은 다음 스크린샷에 표시 됩니다.

![IOS 및 Android에 대 한 빈 확인란의 스크린샷](checkbox-images/checkbox-empty.png "빈 확인란")

기본적으로는 `CheckBox` 비어 있습니다. 사용자 조작을 통해를 확인 하거나 `IsChecked` 속성을로 `true`설정할 수있습니다.`CheckBox`

```xaml
<CheckBox IsChecked="true" />
```

이 XAML은 다음 스크린샷에 표시 됩니다.

![IOS 및 Android에서 선택 된 확인란의 스크린샷](checkbox-images/checkbox-checked.png "선택 됨 확인란")

또는 코드에서 `CheckBox` 를 만들 수 있습니다.

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>CheckBox 변경 상태에 응답

속성이 변경 되 면 사용자 조작을 통해 또는 응용 프로그램에서 `IsChecked` 속성을 설정 하 `CheckedChanged` 는 경우 이벤트가 발생 합니다. `IsChecked` 이 이벤트에 대 한 이벤트 처리기를 등록 하 여 변경에 응답할 수 있습니다.

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

코드 숨김 파일에 대 한 처리기가 포함 된 `CheckedChanged` 이벤트:

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

합니다 `sender` 인수가 `CheckBox` 이 이벤트를 담당 합니다. 에 액세스 하는 데 사용할 수 있습니다는 `CheckBox` 개체 또는 여러 구별할 수 `CheckBox` 공유 하는 동일한 개체 `CheckedChanged` 이벤트입니다.

또는 `CheckedChanged` 이벤트에 대 한 이벤트 처리기를 코드에 등록할 수 있습니다.

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>데이터 바인딩 확인란

데이터 바인딩 및 트리거를 사용 하 여 확인 되거나 비어 있는에 응답 `CheckBox` 하 여 이벤트처리기를제거할수있습니다.`CheckedChanged`

```xaml
<CheckBox x:Name="checkBox" />
<Label Text="Lorem ipsum dolor sit amet, elit rutrum, enim hendrerit augue vitae praesent sed non, lorem aenean quis praesent pede.">
    <Label.Triggers>
        <DataTrigger TargetType="Label"
                     Binding="{Binding Source={x:Reference checkBox}, Path=IsChecked}"
                     Value="true">
            <Setter Property="FontAttributes"
                    Value="Italic, Bold" />
            <Setter Property="FontSize"
                    Value="Large" />
        </DataTrigger>
    </Label.Triggers>
</Label>
```

이 예제에서는 [`Label`](xref:Xamarin.Forms.Label) 데이터 트리거의 바인딩 식을 사용 하 여의 `IsChecked` `CheckBox`속성을 모니터링 합니다. 이 속성이 `true`이면의 `FontAttributes` `FontSize` 및속성은변경됩니다.`Label` 속성이로`false` 반환되`FontSize` 면의 및속성이초기상태로다시설정됩니다.`Label` `FontAttributes` `IsChecked`

다음 [`Label`](xref:Xamarin.Forms.Label) 스크린샷에는 `CheckBox` 이 비어 있을 때 iOS 스크린샷에서 서식 지정이 표시 `CheckBox` 되 고 Android 스크린샷에는를 선택 하면 `Label` 형식이 표시 됩니다.

[ ![IOS 및 Android](checkbox-images/checkbox-databinding.png "데이터 바인딩된 확인란") 의 데이터 바인딩된 확인란 스크린샷] (checkbox-images/checkbox-databinding-large.png#lightbox "데이터 바인딩 확인란")

트리거에 대 한 자세한 내용은 [Xamarin.ios 트리거](~/xamarin-forms/app-fundamentals/triggers.md)를 참조 하세요.

## <a name="disable-a-checkbox"></a>확인란 사용 안 함

경우에 `CheckBox` 따라 응용 프로그램은 검사 중인가 유효한 작업이 아닌 상태로 전환 됩니다. 이러한 경우에는 `CheckBox` `IsEnabled` 속성을로 `false`설정 하 여를 비활성화할 수 있습니다.

## <a name="checkbox-appearance"></a>확인란 모양

`CheckBox` `CheckBox` [클래스에서상속`Color`](xref:Xamarin.Forms.Color)되는 속성 외에도에서 색을로 설정 하는 속성을정의합니다.`Color` [`View`](xref:Xamarin.Forms.View)

```xaml
<CheckBox Color="Red" />
```

다음 스크린샷은 일련의 선택 `CheckBox` 된 개체를 보여 줍니다. 각 개체 `Color` 의 속성은 다른 [`Color`](xref:Xamarin.Forms.Color)로 설정 됩니다.

![IOS 및 Android에서 색이 지정 된 확인란의 스크린샷](checkbox-images/checkbox-colors.png "색이") 지정 된 확인란

## <a name="checkbox-visual-states"></a>CheckBox 시각적 상태

`CheckBox`에는이 선택 될 때의 `CheckBox` 시각적 변경을 시작 하는 데 사용할 수 [있는가있습니다.`VisualState`](xref:Xamarin.Forms.VisualState) `IsChecked`

다음 XAML 예제에 대 한 시각적 상태를 정의 하는 방법을 보여 줍니다는 `IsChecked` 상태:

```xaml
<CheckBox ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Color"
                            Value="Red" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="IsChecked">
                <VisualState.Setters>
                    <Setter Property="Color"
                            Value="Green" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</CheckBox>
```

이 예제 `IsChecked` `CheckBox` 에서는를 선택`Color` 하면 해당 속성이 녹색으로 설정 되도록 [지정합니다.`VisualState`](xref:Xamarin.Forms.VisualState) 는가 정상 상태 이면 해당`Color` 속성은 red로 설정 되도록 지정합니다.`VisualState` `CheckBox` `Normal` 따라서가 비어 있는 경우에 `CheckBox` 는 빨간색이 고 확인 되 면 녹색이 면 전체 효과가 있습니다.

시각적 상태에 대 한 자세한 내용은 [Xamarin. Forms 시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [확인란 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Xamarin Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.ios 시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)
