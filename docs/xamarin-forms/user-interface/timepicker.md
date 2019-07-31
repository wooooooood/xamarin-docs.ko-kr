---
title: Xamarin.Forms TimePicker
description: TimePicker는 사용자가 시간을 선택할 수 있는 Xamarin.ios 뷰입니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 TimePicker를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2E99FB23-B82D-4EB4-AFB3-5002E736E7B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2018
ms.openlocfilehash: d5c4cc6600c8192718257abf4ef1cbec49c12eee
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656479"
---
# <a name="xamarinforms-timepicker"></a>Xamarin.Forms TimePicker

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)

_시간을 선택할 수 있도록 하는 Xamarin.Forms 뷰._

Xamarin.Forms [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) 플랫폼의 시간 선택 컨트롤을 호출 하 고 시간을 선택할 수 있습니다. `TimePicker` 다음 속성을 정의합니다.

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) 형식의 `TimeSpan`, 기본값은 선택한 시간을 `TimeSpan` 0입니다. `TimeSpan` 형식은 자정 이후의 시간을 나타냅니다.
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) 형식의 `string`, [표준](/dotnet/standard/base-types/standard-date-and-time-format-strings/) 하거나 [사용자 지정](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .NET 형식 문자열 "t", 짧은 시간 패턴 기본값은입니다.
- [`TextColor`](xref:Xamarin.Forms.TimePicker.TextColor) 형식의 [ `Color` ](xref:Xamarin.Forms.Color), 기본값은 선택한 시간을 표시 하는 데 색 [ `Color.Default` ](xref:Xamarin.Forms.Color.Default)합니다.
- [`FontAttributes`](xref:Xamarin.Forms.TimePicker.FontAttributes) 형식의 [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), 기본값은 [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None)합니다.
- [`FontFamily`](xref:Xamarin.Forms.TimePicker.FontFamily) 형식의 `string`, 기본값은 `null`합니다.
- [`FontSize`](xref:Xamarin.Forms.TimePicker.FontSize) 형식의 `double`,-1.0 기본값은입니다.

이러한 모든 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 스타일 지정할 수 있습니다 하 고 속성에는 데이터 바인딩 대상이 될 수 있음을 의미 하는 개체입니다. [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) 속성이의 기본 바인딩 모드를 [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay)를 사용 하는 응용 프로그램에서 데이터 바인딩 대상을 사용할 수 있다는 의미는 [ 모델-뷰-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처입니다.

합니다 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) 이벤트를 새 선택 표시를 포함 하지 않습니다 [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) 값입니다. 알림을 받아야 하는 경우에 대 한 처리기를 추가할 수 있습니다 합니다 [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) 이벤트입니다.

## <a name="initializing-the-time-property"></a>시간 속성 초기화

코드에서 초기화할 수 있습니다 합니다 [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) 속성 형식의 값을 `TimeSpan`:

```csharp
TimePicker timePicker = new TimePicker
{
  Time = new TimeSpan(4, 15, 26) // Time set to "04:15:26"
};
```

경우는 [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) XAML에서 속성을 지정 하면를 값으로 변환 됩니다를 `TimeSpan` 및 밀리초는 0 보다 크거나 및 시간 보다 작거나 24 인지 유효성을 검사 합니다. 시간 구성 요소는 콜론으로 구분 해야 합니다.

```xaml
<TimePicker Time="4:15:26" />
```

경우는 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 속성을 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) 형식의 속성을 포함 하는 ViewModel의 인스턴스로 설정 됩니다 `TimeSpan` 라는 `SelectedTime` 인스턴스화할 수 있습니다 (예) `TimePicker` 같이:

```xaml
<TimePicker Time="{Binding SelectedTime}" />
```

이 예제에서는 합니다 [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) 속성은 초기화는 `SelectedTime` ViewModel의 속성입니다. 때문에 `Time` 속성에 바인딩 모드 [ `TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), 사용자가 ViewModel에 자동으로 전파 되는 모든 새 시간입니다.

경우는 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) 에 바인딩이 없습니다 해당 [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) 속성을 응용 프로그램에는 처리기를 연결 해야 합니다 [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) 이벤트 새 시간을 선택할 때 수 형식이 없습니다.

글꼴 속성을 설정 하는 방법에 대 한 내용은 [글꼴](~/xamarin-forms/user-interface/text/fonts.md)합니다.

## <a name="timepicker-and-layout"></a>TimePicker 및 레이아웃

와 같은 비제한 가로 레이아웃 옵션을 사용 하는 것이 불가능 `Center`, `Start`, 또는 `End` 사용 하 여 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker):

```xaml
<TimePicker ···
            HorizontalOptions="Center"
            ··· />
```

그러나이 권장 되지 않습니다. 설정에 따라 합니다 [ `Format` ](xref:Xamarin.Forms.TimePicker.Format) 속성을 선택한 시간에는 다른 표시 너비 필요할 수 있습니다. "T" 형식 문자열을 지정 하면 예를 들어 합니다 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) 긴 형식으로 시간을 표시 하려면 보기 하며 "오전 4시 15분: 26" "오전 4시 15분"의 짧은 시간 형식 ("t") 보다 큰 표시 너비입니다. 플랫폼에 따라서는 이러한 차이로 인해 발생할 수 있습니다는 `TimePicker` 잘린 것으로 표시 또는 레이아웃의 너비를 변경 합니다.

> [!TIP]
> 기본값을 사용 하는 것이 좋습니다 `HorizontalOptions` 설정 `Fill` 사용 하 여 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker), 및 너비는 사용할 수 없습니다 `Auto` 전환할 때 `TimePicker` 에 `Grid` 셀입니다.

## <a name="timepicker-in-an-application"></a>응용 프로그램에서 TimePicker

합니다 [ **SetTimer** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) 샘플에 포함 되어 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker)를 [ `Entry` ](xref:Xamarin.Forms.Entry), 및 [ `Switch` ](xref:Xamarin.Forms.Switch) 페이지에서 보기. `TimePicker` 시간을 선택 하려면 사용할 수 있으며 텍스트의 사용자를 알리는 경고 대화 상자가 표시 됩니다 발생 시간입니다는 `Entry`제공는 `Switch` 에 설정/해제 합니다. XAML 파일을 다음과 같습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SetTimer"
             x:Class="SetTimer.MainPage">
    <StackLayout>
        ...
        <Entry x:Name="_entry"
               Placeholder="Enter event to be reminded of" />
        <Label Text="Select the time below to be reminded at." />
        <TimePicker x:Name="_timePicker"
                    Time="11:00:00"
                    Format="T"
                    PropertyChanged="OnTimePickerPropertyChanged" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Enable timer:" />
            <Switch x:Name="_switch"
                    HorizontalOptions="EndAndExpand"
                    Toggled="OnSwitchToggled" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

합니다 [ `Entry` ](xref:Xamarin.Forms.Entry) 선택한 시간에 발생 하는 경우 표시 되는 미리 알림 텍스트를 입력할 수 있습니다. 합니다 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) 할당 되는 [ `Format` ](xref:Xamarin.Forms.TimePicker.Format) 긴 시간 형식에 대 한 "T"의 속성입니다. 에 연결 된 이벤트 처리기에는 [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) 이벤트와 [ `Switch` ](xref:Xamarin.Forms.Switch) 처리기에 연결 된 해당 [ `Toggled` ](xref:Xamarin.Forms.Switch.Toggled) 이벤트입니다. 이러한 이벤트 처리기 코드 숨김 파일 및 호출에는 `SetTriggerTime` 메서드:

```csharp
public partial class MainPage : ContentPage
{
    DateTime _triggerTime;

    public MainPage()
    {
        InitializeComponent();

        Device.StartTimer(TimeSpan.FromSeconds(1), OnTimerTick);
    }

    bool OnTimerTick()
    {
        if (_switch.IsToggled && DateTime.Now >= _triggerTime)
        {
            _switch.IsToggled = false;
            DisplayAlert("Timer Alert", "The '" + _entry.Text + "' timer has elapsed", "OK");
        }
        return true;
    }

    void OnTimePickerPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        if (args.PropertyName == "Time")
        {
            SetTriggerTime();
        }
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        SetTriggerTime();
    }

    void SetTriggerTime()
    {
        if (_switch.IsToggled)
        {
            _triggerTime = DateTime.Today + _timePicker.Time;
            if (_triggerTime < DateTime.Now)
            {
                _triggerTime += TimeSpan.FromDays(1);
            }
        }
    }
}
```

`SetTriggerTime` 을 기반으로 타이머 시간을 계산 하는 메서드를 `DateTime.Today` 속성 값 및 `TimeSpan` 에서 반환 된 값을 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker). 이 작업이 필요 하기 때문에 `DateTime.Today` 속성에서 반환을 `DateTime` 현재 날짜를 나타내는 했지만 자정에 한 번. 타이머 시간이 이미 지난 지금 내일 되도록 간주 됩니다.

타이머 틱 초 마다 실행 합니다 `OnTimerTick` 확인 하는 메서드 여부를 [ `Switch` ](xref:Xamarin.Forms.Switch) 에 사용할지 여부와 현재 시간 보다 큽니다. 되었거나 타이머에 같음. 타이머에 발생 합니다 [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) 메서드 참고로 사용자에 게 경고 대화 상자를 제공 합니다.

샘플을 처음 실행할 때 합니다 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) 보기가 11를 초기화 합니다. 탭의 `TimePicker` 플랫폼 시간 선택기를 호출 합니다. 매우 다양 한 방식에서 시간 선택기를 구현 하는 플랫폼 이지만 각 접근 방식은 해당 플랫폼의 사용자에 게 친숙 합니다.

[![시간 선택](timepicker-images/timepicker-open.png "시간 선택")](timepicker-images/timepicker-open-large.png#lightbox "시간 선택")

> [!TIP]
> Android에서 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) 재정의 하 여 대화 상자를 사용자 지정할 수 있습니다는 `CreateTimePickerDialog` 사용자 지정 렌더러에서 메서드. 이 사용 하면 예를 들어 대화 상자에 추가할 추가 단추입니다.

시간을 선택한 후 선택한 시간에 표시 됩니다는 [ `TimePicker` ](xref:Xamarin.Forms.TimePicker):

[![선택한 시간](timepicker-images/timepicker-selected.png "선택한 시간")](timepicker-images/timepicker-selected-large.png#lightbox "선택한 시간")

제공한 합니다 [ `Switch` ](xref:Xamarin.Forms.Switch) 설정/해제 응용 프로그램에서 위치에 있는 텍스트의 사용자를 상기 시켜 경고 대화 상자를 표시 합니다 [ `Entry` ](xref:Xamarin.Forms.Entry) 선택한 시간 발생 하는 경우:

[![타이머 팝업](timepicker-images/timer-test.png "타이머 팝업")](timepicker-images/timer-test-large.png#lightbox "타이머 팝업")

경고 대화 상자가 표시 되는 즉시 합니다 [ `Switch` ](xref:Xamarin.Forms.Switch) 가 off 위치로 전환 합니다.

## <a name="related-links"></a>관련 링크

- [SetTimer 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)
- [TimePicker API](xref:Xamarin.Forms.TimePicker)
