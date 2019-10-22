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
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "68739150"
---
# <a name="xamarinforms-checkbox"></a>Xamarin.ios 확인란

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

Xamarin.ios `CheckBox`은 선택 하거나 비워 둘 수 있는 단추의 유형입니다. 확인란을 선택 하면 설정 된 것으로 간주 됩니다. 확인란이 비어 있으면 해제 된 것으로 간주 됩니다.

`CheckBox`는 `CheckBox`를 선택 했는지 여부를 나타내는 `IsChecked` 라는 `bool` 속성을 정의 합니다. 이 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체 에서도 지원 됩니다. 즉, 스타일을 지정할 수 있으며 데이터 바인딩의 대상이 될 수 있습니다.

> [!NOTE]
> 바인딩 가능한 `IsChecked` 속성은 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)의 기본 바인딩 모드입니다.

`CheckBox`는 사용자 조작을 통해 또는 응용 프로그램에서 `IsChecked` 속성을 설정 하는 경우 `IsChecked` 속성이 변경 될 때 발생 하는 `CheckedChanged` 이벤트를 정의 합니다. @No__t_1 이벤트와 함께 제공 되는 `CheckedChangedEventArgs` 개체에는 `bool` 형식의 `Value` 이라는 단일 속성이 있습니다. 이벤트가 발생 하면 `Value` 속성의 값이 `IsChecked` 속성의 새 값으로 설정 됩니다.

## <a name="create-a-checkbox"></a>확인란 만들기

다음 예제에서는 XAML로 `CheckBox`를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<CheckBox />
```

이 XAML은 다음 스크린샷에 표시 됩니다.

![IOS 및 Android에 대 한 빈 확인란의 스크린샷](checkbox-images/checkbox-empty.png "빈 확인란")

기본적으로 `CheckBox`는 비어 있습니다. @No__t_0는 사용자 조작을 사용 하 여 확인 하거나 `IsChecked` 속성을 `true`로 설정 하 여 확인할 수 있습니다.

```xaml
<CheckBox IsChecked="true" />
```

이 XAML은 다음 스크린샷에 표시 됩니다.

![IOS 및 Android에서 선택 된 확인란의 스크린샷](checkbox-images/checkbox-checked.png "선택 됨 확인란")

또는 코드에서 `CheckBox`를 만들 수 있습니다.

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>CheckBox 변경 상태에 응답

사용자 조작을 통해 또는 응용 프로그램에서 `IsChecked` 속성을 설정 하 여 `IsChecked` 속성이 변경 되 면 `CheckedChanged` 이벤트가 발생 합니다. 이 이벤트에 대 한 이벤트 처리기를 등록 하 여 변경에 응답할 수 있습니다.

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

코드 숨겨진 파일에는 `CheckedChanged` 이벤트에 대 한 처리기가 포함 되어 있습니다.

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

@No__t_0 인수는이 이벤트를 담당 하는 `CheckBox`입니다. 이를 사용 하 여 `CheckBox` 개체에 액세스 하거나 동일한 `CheckedChanged` 이벤트를 공유 하는 여러 `CheckBox` 개체를 구분할 수 있습니다.

또는 `CheckedChanged` 이벤트에 대 한 이벤트 처리기를 코드에 등록할 수 있습니다.

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>데이터 바인딩 확인란

데이터 바인딩 및 트리거를 사용 하 여 확인 되거나 비어 있는 `CheckBox`에 응답 하 여 `CheckedChanged` 이벤트 처리기를 제거할 수 있습니다.

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

이 예에서 [`Label`](xref:Xamarin.Forms.Label) 는 데이터 트리거의 바인딩 식을 사용 하 여 `CheckBox`의 `IsChecked` 속성을 모니터링 합니다. 이 속성이 `true` 되 면 `Label`의 `FontAttributes` 및 `FontSize` 속성이 변경 됩니다. @No__t_0 속성이 `false`로 반환 되는 경우 `Label`의 `FontAttributes` 및 `FontSize` 속성은 초기 상태로 다시 설정 됩니다.

다음 스크린샷에서 iOS 스크린샷은 `CheckBox` 비어 있을 때 [`Label`](xref:Xamarin.Forms.Label) 서식 지정을 보여 주지만, Android 스크린 샷에서는 `CheckBox` 확인 될 때 `Label` 서식 지정을 보여 줍니다.

[![IOS 및 Android에 대 한 데이터 바인딩된 확인란의 스크린샷](checkbox-images/checkbox-databinding.png "데이터 바인딩 확인란")](checkbox-images/checkbox-databinding-large.png#lightbox "데이터 바인딩 확인란")

트리거에 대 한 자세한 내용은 [Xamarin.ios 트리거](~/xamarin-forms/app-fundamentals/triggers.md)를 참조 하세요.

## <a name="disable-a-checkbox"></a>확인란 사용 안 함

경우에 따라 응용 프로그램은 검사 중인 `CheckBox` 올바른 작업이 아닌 상태를 입력 합니다. 이러한 경우 `IsEnabled` 속성을 `false`로 설정 하 여 `CheckBox`를 사용 하지 않도록 설정할 수 있습니다.

## <a name="checkbox-appearance"></a>확인란 모양

@No__t_0는 [`View`](xref:Xamarin.Forms.View) 클래스에서 상속 하는 속성 외에도 해당 색을 [`Color`](xref:Xamarin.Forms.Color)로 설정 하는 `Color` 속성도 정의 `CheckBox`.

```xaml
<CheckBox Color="Red" />
```

다음 스크린샷은 일련의 확인 된 `CheckBox` 개체를 보여 줍니다. 각 개체의 `Color` 속성은 다른 [`Color`](xref:Xamarin.Forms.Color)설정 됩니다.

![IOS 및 Android에서 색이 지정 된 확인란의 스크린샷](checkbox-images/checkbox-colors.png "색이 지정 된 확인란")

## <a name="checkbox-visual-states"></a>CheckBox 시각적 상태

`CheckBox`에는 `CheckBox`의 시각적 변경을 시작 하는 데 사용할 수 있는 `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) 있습니다.

다음 XAML 예제에서는 `IsChecked` 상태에 대 한 시각적 상태를 정의 하는 방법을 보여 줍니다.

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

이 예에서는 `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) `CheckBox`을 선택 하면 해당 `Color` 속성이 녹색으로 설정 되도록 지정 합니다. @No__t_0 `VisualState`는 `CheckBox` 정상 상태 이면 해당 `Color` 속성이 red로 설정 되도록 지정 합니다. 따라서 `CheckBox` 비어 있으면 빨간색이 고 확인 되 면 녹색이 되는 것이 전반적인 효과가 있습니다.

시각적 상태에 대 한 자세한 내용은 [Xamarin. Forms 시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [확인란 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Xamarin Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.ios 시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)
