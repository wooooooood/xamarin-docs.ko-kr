---
title: Xamarin.Forms상자
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8399dde2e4e2c9fb53b38fca2923eb0e3bfc6ce3
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136476"
---
# <a name="xamarinforms-checkbox"></a>Xamarin.Forms상자

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

는 Xamarin.Forms `CheckBox` 선택 하거나 비워 둘 수 있는 단추의 유형입니다. 확인란을 선택 하면 설정 된 것으로 간주 됩니다. 확인란이 비어 있으면 해제 된 것으로 간주 됩니다.

`CheckBox``bool` `IsChecked` 이 선택 되었는지 여부를 나타내는 라는 속성을 정의 합니다 `CheckBox` . 이 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 스타일을 지정할 수 있으며 데이터 바인딩의 대상이 될 수 있습니다.

> [!NOTE]
> `IsChecked`바인딩 가능한 속성은의 기본 바인딩 모드 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) 입니다.

`CheckBox``CheckedChanged` `IsChecked` 사용자 조작을 통해 또는 응용 프로그램이 속성을 설정 하는 경우 속성이 변경 될 때 발생 하는 이벤트를 정의 합니다 `IsChecked` . `CheckedChangedEventArgs`이벤트와 함께 제공 되는 개체에는 `CheckedChanged` 형식의 이라는 단일 속성이 있습니다 `Value` `bool` . 이벤트가 발생 하면 속성의 값 `Value` 이 속성의 새 값으로 설정 됩니다 `IsChecked` .

## <a name="create-a-checkbox"></a>확인란 만들기

다음 예제에서는 XAML로를 인스턴스화하는 방법을 보여 줍니다 `CheckBox` .

```xaml
<CheckBox />
```

이 XAML은 다음 스크린샷에 표시 됩니다.

![IOS 및 Android에 대 한 빈 확인란의 스크린샷](checkbox-images/checkbox-empty.png "빈 확인란")

기본적으로는 `CheckBox` 비어 있습니다. `CheckBox`사용자 조작을 통해를 확인 하거나 속성을로 설정할 수 있습니다 `IsChecked` `true` .

```xaml
<CheckBox IsChecked="true" />
```

이 XAML은 다음 스크린샷에 표시 됩니다.

![IOS 및 Android에서 선택 된 확인란의 스크린샷](checkbox-images/checkbox-checked.png "선택 됨 확인란")

또는 `CheckBox` 코드에서를 만들 수 있습니다.

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>CheckBox 변경 상태에 응답

`IsChecked`속성이 변경 되 면 사용자 조작을 통해 또는 응용 프로그램에서 속성을 설정 하는 경우 `IsChecked` `CheckedChanged` 이벤트가 발생 합니다. 이 이벤트에 대 한 이벤트 처리기를 등록 하 여 변경에 응답할 수 있습니다.

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

코드를 포함 하는 파일에는 이벤트에 대 한 처리기가 포함 됩니다 `CheckedChanged` .

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

`sender`인수는 `CheckBox` 이 이벤트를 담당 합니다. 이를 사용 하 여 개체에 액세스 `CheckBox` 하거나 `CheckBox` 동일한 이벤트 처리기를 공유 하는 여러 개체를 구분할 수 있습니다 `CheckedChanged` .

또는 이벤트에 대 한 이벤트 처리기를 `CheckedChanged` 코드에 등록할 수 있습니다.

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>데이터 바인딩 확인란

`CheckedChanged`데이터 바인딩 및 트리거를 사용 하 여 `CheckBox` 확인 되거나 비어 있는에 응답 하 여 이벤트 처리기를 제거할 수 있습니다.

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

이 예제에서는 [`Label`](xref:Xamarin.Forms.Label) 데이터 트리거의 바인딩 식을 사용 하 여의 속성을 모니터링 합니다 `IsChecked` `CheckBox` . 이 속성이 `true` 이면 `FontAttributes` 의 및 속성은 `FontSize` `Label` 변경 됩니다. `IsChecked`속성이로 반환 되 면 `false` `FontAttributes` `FontSize` 의 및 속성이 `Label` 초기 상태로 다시 설정 됩니다.

다음 스크린샷에는 [`Label`](xref:Xamarin.Forms.Label) 이 비어 있을 때 iOS 스크린샷에서 서식 지정이 표시 `CheckBox` 되 고 Android 스크린샷에는를 `Label` 선택 하면 형식이 표시 `CheckBox` 됩니다.

[![IOS 및 Android에 대 한 데이터 바인딩된 확인란의 스크린샷](checkbox-images/checkbox-databinding.png "데이터 바인딩 확인란")](checkbox-images/checkbox-databinding-large.png#lightbox "데이터 바인딩 확인란")

트리거에 대 한 자세한 내용은 [ Xamarin.Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)를 참조 하세요.

## <a name="disable-a-checkbox"></a>확인란 사용 안 함

경우에 따라 응용 프로그램은 `CheckBox` 검사 중인가 유효한 작업이 아닌 상태로 전환 됩니다. 이러한 경우에는 `CheckBox` 속성을로 설정 하 여를 비활성화할 수 있습니다 `IsEnabled` `false` .

## <a name="checkbox-appearance"></a>CheckBox 모양

클래스에서 상속 되는 속성 외에 `CheckBox` [`View`](xref:Xamarin.Forms.View) `CheckBox` 도에서 색을 `Color` 로 설정 하는 속성을 정의 합니다 [`Color`](xref:Xamarin.Forms.Color) .

```xaml
<CheckBox Color="Red" />
```

다음 스크린샷은 일련의 선택 된 개체를 보여 줍니다 `CheckBox` . 각 개체의 `Color` 속성은 다른로 설정 됩니다 [`Color`](xref:Xamarin.Forms.Color) .

![IOS 및 Android에서 색이 지정 된 확인란의 스크린샷](checkbox-images/checkbox-colors.png "색이 지정 된 확인란")

## <a name="checkbox-visual-states"></a>CheckBox 시각적 상태

`CheckBox`에는 `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) 이 선택 될 때의 시각적 변경을 시작 하는 데 사용할 수 있는가 있습니다 `CheckBox` .

다음 XAML 예제에서는 상태에 대 한 시각적 상태를 정의 하는 방법을 보여 줍니다 `IsChecked` .

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

이 예제에서는를 `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) 선택 하면 해당 `CheckBox` `Color` 속성이 녹색으로 설정 되도록 지정 합니다. 는 `Normal` `VisualState` `CheckBox` 가 정상 상태 이면 해당 `Color` 속성은 red로 설정 되도록 지정 합니다. 따라서가 비어 있는 경우에는 `CheckBox` 빨간색이 고 확인 되 면 녹색이 면 전체 효과가 있습니다.

시각적 상태에 대 한 자세한 내용은 [ Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [확인란 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Xamarin.FormsInstead](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)
