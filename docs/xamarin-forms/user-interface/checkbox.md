---
title: Xamarin.Forms 확인란
description: Xamarin.Forms 확인란에는 확인할 수 있습니다 하거나 또는 빈 단추 형식입니다. 확인란을 선택 하면에 있는 것으로 간주 합니다. 확인란이 비어 있으면 해제 간주 합니다.
ms.prod: xamarin
ms.assetid: B8B9268B-BCB8-42B9-B08C-C0F22C137238
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: 42631b1b67dc1d342e9f8666916604e68ee158d8
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67517922"
---
# <a name="xamarinforms-checkbox"></a>Xamarin.Forms 확인란

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CheckBoxDemos)

Xamarin.Forms `CheckBox` 수는 단추의 형식을 선택 하거나 빈 수는 있습니다. 확인란을 선택 하면에 있는 것으로 간주 합니다. 확인란이 비어 있으면 해제 간주 합니다.

`CheckBox` 정의 `bool` 라는 속성이 `IsChecked`를 나타내는 여부를 `CheckBox` 확인란이 선택 되어 합니다. 이 속성은 또한 지를 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 개체 스타일을 지정할 수 데이터 바인딩의 대상 수를 의미 합니다.

> [!NOTE]
> 합니다 `IsChecked` 바인딩할 수 있는 속성이의 기본 바인딩 모드 [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay)합니다.

`CheckBox` 정의 `CheckedChanged` 될 때 발생 한 이벤트를 `IsChecked` 사용자 조작을 통해 또는 응용 프로그램 설정 속성 변경의 `IsChecked` 속성. 합니다 `CheckedChangedEventArgs` 와 함께 제공 되는 개체를 `CheckedChanged` 이벤트 라는 단일 속성을 가진 `Value`, 형식의 `bool`합니다. 경우 이벤트가의 값을 `Value` 속성의 새 값으로 설정 되는 `IsChecked` 속성.

## <a name="create-a-checkbox"></a>확인란 만들기

다음 예제에서는 인스턴스화하는 방법을 보여 줍니다는 `CheckBox` XAML에서:

```xaml
<CheckBox />
```

이 XAML은 다음 스크린샷에 표시 된 모양이 결과:

![IOS 및 Android에는 빈 확인란 스크린샷](checkbox-images/checkbox-empty.png "빈 확인란")

기본적으로 `CheckBox` 비어 있습니다. 합니다 `CheckBox` 사용자 조작 하거나 설정 하 여 확인할 수 있습니다 합니다 `IsChecked` 속성을 `true`:

```xaml
<CheckBox IsChecked="true" />
```

이 XAML은 다음 스크린샷에 표시 된 모양이 결과:

![IOS 및 Android에서 선택 된 확인란, 스크린샷](checkbox-images/checkbox-checked.png "확인란 선택")

또는 `CheckBox` 코드에서 만들 수 있습니다.

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>상태를 변경 하는 확인란을 선택에 응답

경우는 `IsChecked` 사용자 조작을 통해 또는 응용 프로그램 설정 속성 변경을 `IsChecked` 속성인을 `CheckedChanged` 이벤트가 발생 합니다. 변경 내용에 응답 하도록이 이벤트에 대 한 이벤트 처리기를 등록할 수 있습니다.

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

에 대 한 이벤트 처리기 또는 `CheckedChanged` 코드에서 이벤트를 등록할 수 있습니다.

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>데이터 바인딩 확인란

합니다 `CheckedChanged` 응답할 데이터 바인딩 및 트리거를 사용 하 여 이벤트 처리기를 제거할 수 있습니다는 `CheckBox` 선택 또는 빈:

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

이 예제에서는 [ `Label` ](xref:Xamarin.Forms.Label) 바인딩 식 데이터 트리거를 사용 하 여 모니터링를 `IsChecked` 속성을 `CheckBox`입니다. 경우이 속성은 `true`는 `FontAttributes` 및 `FontSize` 의 속성을 `Label` 변경 합니다. 경우는 `IsChecked` 속성에 반환 `false`, `FontAttributes` 및 `FontSize` 속성의는 `Label` 를 초기 상태로 다시 설정 됩니다.

다음 스크린샷에서 iOS 스크린샷은 [ `Label` ](xref:Xamarin.Forms.Label) 때 서식 지정을 `CheckBox` Android 스크린샷에서 비어 있는 경우는 `Label` 때 서식 지정는 `CheckBox` 확인란이 선택 되어:

[![데이터의 스크린샷 iOS 및 Android에서 확인란을 바인딩된](checkbox-images/checkbox-databinding.png "확인란을 선택 하는 데이터 바인딩")](checkbox-images/checkbox-databinding-large.png#lightbox "확인란을 선택 하는 데이터 바인딩")

트리거에 대 한 자세한 내용은 참조 하세요. [Xamarin.Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)합니다.

## <a name="disable-a-checkbox"></a>Checkbox를 사용 하지 않도록 설정

응용 프로그램 상태로 전환 하는 경우에 따라 여기서는 `CheckBox` 확인할 것은 올바른 작업이 아닙니다. 이러한 경우에는 `CheckBox` 설정 하 여 비활성화할 수 있습니다 해당 `IsEnabled` 속성을 `false`입니다.

## <a name="checkbox-appearance"></a>확인란 모양

속성 외에 `CheckBox` 에서 상속 되는 [ `View` ](xref:Xamarin.Forms.View) 클래스 `CheckBox` 정의 `Color` 해당 색을 설정 하는 속성을 [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<CheckBox Color="Red" />
```

다음 스크린샷에서 일련의 확인 표시 `CheckBox` 개체를 각 개체에 해당 `Color` 속성을 다른 집합 [ `Color` ](xref:Xamarin.Forms.Color):

![IOS 및 Android에서 색이 지정 된 확인란 스크린샷](checkbox-images/checkbox-colors.png "색이 지정 된 확인란")

## <a name="checkbox-visual-states"></a>시각적 상태 확인란

`CheckBox` 에 `IsChecked` [ `VisualState` ](xref:Xamarin.Forms.VisualState) 시각적으로 변화를 시작 하려면 사용할 수 있는 `CheckBox` 확인 되는 경우.

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

이 예제에서는 합니다 `IsChecked` [ `VisualState` ](xref:Xamarin.Forms.VisualState) 되도록 지정 합니다 `CheckBox` 확인란이 해당 `Color` 속성을 녹색으로 설정 됩니다. `Normal` `VisualState` 되도록 지정 합니다 `CheckBox` 정상적인 상태의 해당 `Color` 빨강으로 설정 됩니다. 따라서 전체적인 효과는 `CheckBox` 는 빨간색 이며 비어 있는 경우 녹색을 선택 합니다.

시각적 상태에 대 한 자세한 내용은 참조 하세요. [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)합니다.

## <a name="related-links"></a>관련 링크

- [확인란을 데모 (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CheckBoxDemos)
- [Xamarin.Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
