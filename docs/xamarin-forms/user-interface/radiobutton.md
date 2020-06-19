---
title: Xamarin.Forms라디오
description: Xamarin.FormsRadioButton은 사용자가 집합에서 하나의 옵션을 선택할 수 있도록 하는 단추 유형입니다. 각 옵션은 하나의 라디오 단추로 표시 되 고 그룹에서 라디오 단추 하나를 선택할 수 있습니다.
ms.prod: xamarin
ms.assetid: E2AA40E0-69A5-41DF-BFC4-C151CA657451
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/13/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f7cbd11f98127cb73514112dae785102ff9c51c0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127625"
---
# <a name="xamarinforms-radiobutton"></a>Xamarin.Forms라디오

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)

는 Xamarin.Forms `RadioButton` 사용자가 집합에서 하나의 옵션을 선택할 수 있도록 하는 단추 유형입니다. 각 옵션은 하나의 라디오 단추로 표시 되 고 그룹에서 라디오 단추 하나를 선택할 수 있습니다. 클래스는 `RadioButton` 클래스에서 상속 [`Button`](xref:Xamarin.Forms.Button) 됩니다.

다음 스크린샷에서는 `RadioButton` iOS 및 Android에서 지워진 상태와 선택 된 상태의 개체를 보여 줍니다.

![IOS 및 Android에서 선택 되 고 지워진 상태의 Radiobutton 스크린샷](radiobutton-images/radiobutton-states.png "IOS 및 Android의 라디오 단추")

> [!IMPORTANT]
> `RadioButton`는 현재 실험적 이며 플래그를 설정 하는 방법 으로만 사용할 수 있습니다 `RadioButton_Experimental` . 자세한 내용은 [실험적 플래그](~/xamarin-forms/internals/experimental-flags.md)를 참조 하세요.

`RadioButton`컨트롤은 다음 속성을 정의 합니다.

- `IsChecked``bool`는를 선택 했는지 여부를 정의 하는 형식의입니다 `RadioButton` . 이 속성은 `TwoWay` 바인딩을 사용 하며 기본값은 `false` 입니다.
- `GroupName`는 `string` `RadioButton` 함께 사용할 수 없는 컨트롤을 지정 하는 이름을 정의 하는 형식의입니다. 이 속성의 기본값은 `null` 입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

`RadioButton`컨트롤은 `CheckedChanged` `IsChecked` 사용자 또는 프로그래밍 방식 조작을 통해 속성이 변경 될 때 발생 하는 이벤트를 정의 합니다. `CheckedChangedEventArgs`이벤트와 함께 제공 되는 개체에는 `CheckedChanged` 형식의 이라는 단일 속성이 있습니다 `Value` `bool` . 이벤트가 발생 하면 속성의 값 `Value` 이 속성의 새 값으로 설정 됩니다 `IsChecked` .

또한 `RadioButton` 클래스는 클래스에서 다음과 같이 일반적으로 사용 되는 속성을 상속 합니다 [`Button`](xref:Xamarin.Forms.Button) .

- [`Command`](xref:Xamarin.Forms.Button.Command)`ICommand`는를 선택할 때 실행 되는 형식의입니다 `RadioButton` .
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)에 `object` 전달 되는 매개 변수인 형식의입니다 `Command` .
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)[`FontAttributes`](xref:Xamarin.Forms.FontAttributes)텍스트 스타일을 결정 하는 형식의입니다.
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)`string`글꼴 패밀리를 정의 하는 형식의입니다.
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)`double`글꼴 크기를 정의 하는 형식의입니다.
- [`Text`](xref:Xamarin.Forms.Button.Text)표시 되는 `string` 텍스트를 정의 하는 형식의입니다.
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)표시 되는 [`Color`](xref:Xamarin.Forms.Color) 텍스트의 색을 정의 하는 형식의입니다.

컨트롤에 대 한 자세한 내용은 [`Button`](xref:Xamarin.Forms.Button) [ Xamarin.Forms 단추](~/xamarin-forms/user-interface/button.md)를 참조 하세요.

## <a name="create-radiobuttons"></a>라디오 단추 만들기

다음 예제에서는 XAML에서 개체를 인스턴스화하는 방법을 보여 줍니다 `RadioButton` .

```xaml
<StackLayout>
    <Label Text="What's your favorite animal?" />
    <RadioButton Text="Cat" />
    <RadioButton Text="Dog" />
    <RadioButton Text="Elephant" />
    <RadioButton Text="Monkey"
                 IsChecked="true" />
</StackLayout>
```

이 예제에서 `RadioButton` 개체는 암시적으로 동일한 부모 컨테이너 내에 그룹화 됩니다. 이 XAML은 다음 스크린샷에 표시 됩니다.

![IOS 및 Android에서 암시적으로 그룹화 된 라디오 단추 스크린샷](radiobutton-images/radiobuttons.png "IOS 및 Android에서 암시적으로 그룹화 된 라디오 단추")

또는 `RadioButton` 코드에서 개체를 만들 수 있습니다.

```csharp
StackLayout stackLayout = new StackLayout
{
    Children =
    {
        new Label { Text = "What's your favorite animal?" },
        new RadioButton { Text = "Cat" },
        new RadioButton { Text = "Dog" },
        new RadioButton { Text = "Elephant" },
        new RadioButton { Text = "Monkey", IsChecked = true }
    }
};
```

## <a name="group-radiobuttons"></a>라디오 단추 그룹화

라디오 단추는 그룹에서 작동 하 고, 라디오 단추를 그룹화 하는 방법에는 두 가지가 있습니다.

- 동일한 부모 컨테이너 내에 배치 합니다. 이를 암시적 그룹 이라고 합니다.
- `GroupName`각 라디오 단추의 속성을 동일한 값으로 설정 합니다. 이를 명시적 그룹화 라고 합니다.

다음 XAML 예제에서는 `RadioButton` 속성을 설정 하 여 명시적으로 개체를 그룹화 하는 방법을 보여 줍니다 `GroupName` .

```xaml
<Label Text="What's your favorite color?" />
<RadioButton Text="Red"
             TextColor="Red"
             GroupName="colors" />
<RadioButton Text="Green"
             TextColor="Green"
             GroupName="colors" />
<RadioButton Text="Blue"
             TextColor="Blue"
             GroupName="colors" />
<RadioButton Text="Other"
             GroupName="colors" />
```

이 예제에서 각 `RadioButton` 는 동일한 값을 공유 하므로 함께 사용할 수 없습니다 `GroupName` . 이 XAML은 다음 스크린샷에 표시 됩니다.

![IOS 및 Android에서 명시적으로 그룹화 된 Radiobutton의 스크린샷](radiobutton-images/grouped-radiobuttons.png "IOS 및 Android에서 명시적으로 그룹화 된 라디오 단추")

## <a name="respond-to-a-radiobutton-state-change"></a>RadioButton 상태 변경에 응답

라디오 단추에는 두 가지 상태가 있습니다. 선택 하거나 선택 취소 합니다. 라디오 단추를 선택 하면 해당 `IsChecked` 속성은 `true` 입니다. 라디오 단추가 선택 취소 되 면 해당 `IsChecked` 속성은 `false` 입니다. 라디오 단추는 동일한 그룹의 다른 라디오 단추를 클릭 하 여 지울 수 있지만 다시 클릭 하 여 제거할 수는 없습니다. 그러나 속성을로 설정 하 여 라디오 단추를 프로그래밍 방식으로 지울 수 있습니다 `IsChecked` `false` .

`IsChecked`사용자 또는 프로그래밍 방식 조작을 통해 속성이 변경 되 면 `CheckedChanged` 이벤트가 발생 합니다. 이 이벤트에 대 한 이벤트 처리기를 등록 하 여 변경에 응답할 수 있습니다.

```xaml
<RadioButton Text="Red"
             TextColor="Red"
             GroupName="colors"
             CheckedChanged="OnColorsRadioButtonCheckedChanged" />
```

코드 숨김으로는 이벤트에 대 한 처리기가 포함 되어 있습니다 `CheckedChanged` .

```csharp
void OnColorsRadioButtonCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation
}
```

`sender`인수는 `RadioButton` 이 이벤트를 담당 합니다. 이를 사용 하 여 개체에 액세스 `RadioButton` 하거나 `RadioButton` 동일한 이벤트 처리기를 공유 하는 여러 개체를 구분할 수 있습니다 `CheckedChanged` .

또는 이벤트에 대 한 이벤트 처리기를 `CheckedChanged` 코드에 등록할 수 있습니다.

```csharp
RadioButton radioButton = new RadioButton { ... };
radioButton.CheckedChanged += (sender, e) =>
{
    // Perform required operation
};
```

> [!NOTE]
> 상태 변경에 응답 하는 다른 방법은을 `RadioButton` 정의 하 `ICommand` 고이를 속성에 할당 하는 것입니다 `RadioButton.Command` . 자세한 내용은 [Button: command Interface 사용](~/xamarin-forms/user-interface/button.md#using-the-command-interface)을 참조 하세요.

## <a name="radiobutton-visual-states"></a>RadioButton 시각적 상태

`RadioButton`에는 `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) 를 선택할 때 시각적 변경을 시작 하는 데 사용할 수 있는가 있습니다 `RadioButton` .

다음 XAML 예제에서는 상태에 대 한 시각적 상태를 정의 하는 방법을 보여 줍니다 `IsChecked` .

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="Normal">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Red" />
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="IsChecked">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Green" />
                                <Setter Property="Opacity"
                                        Value="1" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout>
        <Label Text="What's your favorite mode of transport?" />
        <RadioButton Text="Car"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Bike"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Train"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Walking"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
    </StackLayout>
</ContentPage>
```

이 예제에서 암시적 [`Style`](xref:Xamarin.Forms.Style)은 `RadioButton` 개체를 대상으로 지정합니다. 는 `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) 를 `RadioButton` 선택 하면 해당 `TextColor` 속성이 녹색으로 설정 되 고 값이 1로 설정 되도록 지정 합니다 `Opacity` . 는 `Normal` `VisualState` `RadioButton` 가 지워진 상태 이면 해당 `TextColor` 속성이 빨강으로 설정 되 고 값이 0.5로 설정 되도록 지정 합니다 `Opacity` . 따라서 전반적인 효과는를 `RadioButton` 지우면 빨강으로 투명 하 고 부분적으로 투명 하 게 되며 선택 시 투명도 없이 녹색입니다.

![IOS 및 Android의 시각적 상태별로 설정 된 RadioButton 모양의 스크린샷](radiobutton-images/ischecked-visualstate.png "IOS 및 Android의 RadioButton 시각적 상태")

시각적 개체 상태에 대한 자세한 내용은 [Xamarin.Forms 시각적 개체 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)를 참조하세요.

## <a name="disable-a-radiobutton"></a>라디오 단추 사용 안 함

경우에 따라 응용 프로그램은 `RadioButton` 검사 중인가 유효한 작업이 아닌 상태로 전환 됩니다. 이러한 경우에는 `RadioButton` 속성을로 설정 하 여를 비활성화할 수 있습니다 `IsEnabled` `false` .

## <a name="related-links"></a>관련 링크

- [RadioButton 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)
- [Xamarin.Forms 단추](~/xamarin-forms/user-interface/button.md)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
