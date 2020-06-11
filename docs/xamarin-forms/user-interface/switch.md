---
제목: " Xamarin.Forms switch" description: " Xamarin.Forms 스위치는 사용자가 설정 및 해제 상태를 전환 하기 위해 조작할 수 있는 단추의 유형입니다. 이 문서에서는 Switch 클래스를 사용 하 여 토글 UI 요소를 표시 하는 방법을 설명 합니다.
assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9: xamarin-forms author: profexorgeek: jusjohns:: 07/18/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-switch"></a>Xamarin.Forms바꿀

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Xamarin.Forms [`Switch`](xref:Xamarin.Forms.Switch) 컨트롤은 사용자가 값으로 표시 되는 설정/해제 상태를 전환 하기 위해 조작할 수 있는 가로 토글 단추입니다 `boolean` . `Switch`클래스는에서 상속 [`View`](xref:Xamarin.Forms.View) 됩니다.

다음 스크린샷에서는 `Switch` iOS 및 Android의 설정 **/** **해제** 상태에 있는 컨트롤을 보여 줍니다.

![IOS 및 Android에서 on 및 off 상태의 스위치 스크린샷](switch-images/switch-states-default.png "IOS 및 Android의 스위치")

`Switch`컨트롤은 다음 속성을 정의 합니다.

* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)가 `boolean` 설정 되어 있는지 여부를 나타내는 값입니다 `Switch` . **on**
* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor)가 `Color` `Switch` 전환 또는 상태에서 렌더링 되는 방식 **에**영향을 주는입니다.
* `ThumbColor`는 `Color` 스위치 엄지의입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉,가 `Switch` 스타일을 지정 하 고 데이터 바인딩의 대상이 될 수 있습니다.

`Switch`컨트롤은 `Toggled` `IsToggled` 사용자 조작이 나 응용 프로그램에서 속성을 설정 하 여 속성이 변경 될 때 발생 하는 이벤트를 정의 합니다 `IsToggled` . `ToggledEventArgs`이벤트와 함께 제공 되는 개체에는 `Toggled` 형식의 이라는 단일 속성이 있습니다 `Value` `bool` . 이벤트가 발생 하면 속성의 값은 `Value` 속성의 새 값을 반영 합니다 `IsToggled` .

## <a name="create-a-switch"></a>스위치 만들기

는 `Switch` XAML에서 인스턴스화될 수 있습니다. `IsToggled`속성을 설정 하 여를 토글할 수 있습니다 `Switch` . 기본적으로 `IsToggled` 속성은 `false`합니다. 다음 예제에서는 `Switch` 선택적 속성 집합을 사용 하 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다 `IsToggled` .

```xaml
<Switch IsToggled="true"/>
```

`Switch`코드에서를 만들 수도 있습니다.

```csharp
Switch switchControl = new Switch { IsToggled = true };
```

## <a name="switch-appearance"></a>모양 전환

클래스에서 상속 되는 속성 외에 [`Switch`](xref:Xamarin.Forms.Switch) [`View`](xref:Xamarin.Forms.View) `Switch` 도 및 속성을 정의 `OnColor` `ThumbColor` 합니다. `OnColor`속성을 설정 하 여 해당 `Switch` 상태로 전환 될 때 색을 정의 하 고 **on** , `ThumbColor` 속성을 설정 하 여 `Color` 스위치 엄지의를 정의할 수 있습니다. 다음 예제에서는 `Switch` 이러한 속성을 설정 하 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<Switch OnColor="Orange"
        ThumbColor="Green" />
```

코드에서를 만들 때 속성을 설정할 수도 있습니다 `Switch` .

```csharp
Switch switch = new Switch { OnColor = Color.Orange, ThumbColor = Color.Green };
```

다음 스크린샷은 `Switch` 및 속성이 설정 된 **on** 및 **off** 토글 상태의을 보여 줍니다 `OnColor` `ThumbColor` .

![IOS 및 Android에서 on 및 off 상태의 스위치 스크린샷](switch-images/switch-states-colors.png "IOS 및 Android의 스위치")

## <a name="respond-to-a-switch-state-change"></a>전환 상태 변경에 응답

`IsToggled`속성이 변경 되 면 사용자 조작을 통해 또는 응용 프로그램에서 속성을 설정 하는 경우 `IsToggled` `Toggled` 이벤트가 발생 합니다. 이 이벤트에 대 한 이벤트 처리기를 등록 하 여 변경에 응답할 수 있습니다.

```xaml
<Switch Toggled="OnToggled" />
```

코드를 포함 하는 파일에는 이벤트에 대 한 처리기가 포함 됩니다 `Toggled` .

```csharp
void OnToggled(object sender, ToggledEventArgs e)
{
    // Perform an action after examining e.Value
}
```

`sender`이벤트 처리기의 인수는이 이벤트의 발생을 `Switch` 담당 합니다. 속성을 사용 `sender` 하 여 개체에 액세스 `Switch` 하거나 `Switch` 동일한 이벤트 처리기를 공유 하는 여러 개체를 구분할 수 있습니다 `Toggled` .

`Toggled`이벤트 처리기는 코드에서 할당 될 수도 있습니다.

```csharp
Switch switchControl = new Switch {...};
switchControl.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>스위치 데이터 바인딩

`Toggled`데이터 바인딩 및 트리거를 사용 하 여 변경 된 토글 상태에 응답 하 여 이벤트 처리기를 제거할 수 있습니다 `Switch` .

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

이 예제에서는의 [`Label`](xref:Xamarin.Forms.Label) 바인딩 식을 사용 하 여 `DataTrigger` `IsToggled` 명명 된의 속성을 모니터링 합니다 `Switch` `styleSwitch` . 이 속성이 `true` 이면 `FontAttributes` `FontSize` 의 및 속성이 `Label` 변경 됩니다. `IsToggled`속성이로 반환 되 면 `false` `FontAttributes` `FontSize` 의 및 속성이 `Label` 초기 상태로 다시 설정 됩니다.

트리거에 대 한 자세한 내용은 [ Xamarin.Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)를 참조 하세요.

## <a name="disable-a-switch"></a>스위치 사용 안 함

응용 프로그램은 설정/해제 `Switch` 가 유효한 작업이 아닌 상태로 전환 될 수 있습니다. 이러한 경우에는 `Switch` 속성을로 설정 하 여를 비활성화할 수 있습니다 `IsEnabled` `false` . 이렇게 하면 사용자가를 조작할 수 없게 됩니다 `Switch` .

## <a name="related-links"></a>관련 링크

* [데모 전환](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
* [Xamarin.Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)
