---
title: Xamarin.ios TimePicker
description: TimePicker는 사용자가 시간을 선택할 수 있는 Xamarin.ios 뷰입니다. 이 문서에서는 Xamarin.ios 응용 프로그램에서 TimePicker를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2E99FB23-B82D-4EB4-AFB3-5002E736E7B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2018
ms.openlocfilehash: aae0791199b0e3042a3c619fcb11e7b877f52012
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695912"
---
# <a name="xamarinforms-timepicker"></a>Xamarin.ios TimePicker

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)

_사용자가 시간을 선택할 수 있도록 하는 Xamarin 폼 뷰입니다._

Xamarin.ios [`TimePicker`](xref:Xamarin.Forms.TimePicker) 는 플랫폼의 시간 선택 컨트롤을 호출 하 고 사용자가 시간을 선택할 수 있습니다. `TimePicker`는 다음 속성을 정의 합니다.

- 선택한 시간인 `TimeSpan` 형식의 [`Time`](xref:Xamarin.Forms.TimePicker.Time) 이며 기본값은 0 `TimeSpan`입니다. @No__t_0 형식은 자정 이후의 기간을 나타냅니다.
- 표준 또는 사용자 지정 .Net 형식 문자열 `string`의 [`Format`](xref:Xamarin.Forms.TimePicker.Format) 입니다. [표준](/dotnet/standard/base-types/standard-date-and-time-format-strings/) 또는 [사용자 지정](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .net 형식 문자열은 간단한 시간 패턴 인 "t"로 기본 설정 됩니다.
- [`Color`](xref:Xamarin.Forms.Color)형식의 [`TextColor`](xref:Xamarin.Forms.TimePicker.TextColor) 선택 된 시간을 표시 하는 데 사용 되는 색입니다. 기본값은 [`Color.Default`](xref:Xamarin.Forms.Color.Default)입니다.
- [`FontAttributes`](xref:Xamarin.Forms.FontAttributes)형식의 [`FontAttributes`](xref:Xamarin.Forms.TimePicker.FontAttributes) 이며 기본값은 [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None)입니다.
- `string` 형식의 [`FontFamily`](xref:Xamarin.Forms.TimePicker.FontFamily) 이며 기본값은 `null`입니다.
- `double` 형식의 [`FontSize`](xref:Xamarin.Forms.TimePicker.FontSize) 이며 기본값은-1.0입니다.
- `double` 형식의 `CharacterSpacing`은 `TimePicker` 텍스트 문자 사이의 간격입니다.

이러한 속성은 모두 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에 의해 지원 됩니다. 즉, 스타일을 지정할 수 있으며 속성은 데이터 바인딩의 대상이 될 수 있습니다. [@No__t_1](xref:Xamarin.Forms.TimePicker.Time) 속성은 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)의 기본 바인딩 모드를 사용 하며,이는 [MVVM (모델-뷰-ViewModel)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처를 사용 하는 응용 프로그램의 데이터 바인딩 대상이 될 수 있음을 의미 합니다.

[@No__t_1](xref:Xamarin.Forms.TimePicker) 에는 새로 선택한 [`Time`](xref:Xamarin.Forms.TimePicker.Time) 값을 나타내는 이벤트가 포함 되지 않습니다. 이에 대 한 알림이 필요한 경우 [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) 이벤트에 대 한 처리기를 추가할 수 있습니다.

## <a name="initializing-the-time-property"></a>Time 속성 초기화

코드에서 `TimeSpan` 형식의 값으로 [`Time`](xref:Xamarin.Forms.TimePicker.Time) 속성을 초기화할 수 있습니다.

```csharp
TimePicker timePicker = new TimePicker
{
  Time = new TimeSpan(4, 15, 26) // Time set to "04:15:26"
};
```

[@No__t_1](xref:Xamarin.Forms.TimePicker.Time) 속성이 XAML로 지정 된 경우 값은 `TimeSpan`으로 변환 되 고 밀리초 수가 0 보다 크거나 같고 시간 수가 24 보다 적은지 확인 하기 위해 유효성이 검사 됩니다. 시간 구성 요소는 콜론으로 구분 해야 합니다.

```xaml
<TimePicker Time="4:15:26" />
```

[@No__t_3](xref:Xamarin.Forms.TimePicker) 의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 속성이 이름이 `SelectedTime` `TimeSpan` 형식의 속성이 포함 된 ViewModel의 인스턴스로 설정 된 경우 (예:) 다음과 같이 `TimePicker`을 인스턴스화할 수 있습니다.

```xaml
<TimePicker Time="{Binding SelectedTime}" />
```

이 예제에서 [`Time`](xref:Xamarin.Forms.TimePicker.Time) 속성은 ViewModel의 `SelectedTime` 속성으로 초기화 됩니다. @No__t_0 속성에는 [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)의 바인딩 모드가 있으므로 사용자가 선택 하는 새 시간은 자동으로 ViewModel에 전파 됩니다.

[@No__t_1](xref:Xamarin.Forms.TimePicker) 에 [`Time`](xref:Xamarin.Forms.TimePicker.Time) 속성에 대 한 바인딩이 포함 되어 있지 않은 경우 응용 프로그램은 사용자가 새 시간을 선택할 때 알리도록 [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) 이벤트에 처리기를 연결 해야 합니다.

글꼴 속성을 설정 하는 방법에 대 한 자세한 내용은 [글꼴](~/xamarin-forms/user-interface/text/fonts.md)을 참조 하십시오.

## <a name="timepicker-and-layout"></a>TimePicker 및 레이아웃

[@No__t_4](xref:Xamarin.Forms.TimePicker)를 사용 하 여 `Center`, `Start` 또는 `End`와 같은 제한 되지 않은 가로 레이아웃 옵션을 사용할 수 있습니다.

```xaml
<TimePicker ···
            HorizontalOptions="Center"
            ··· />
```

그러나이 방법은 권장 되지 않습니다. [@No__t_1](xref:Xamarin.Forms.TimePicker.Format) 속성의 설정에 따라 선택한 시간에 다른 표시 너비가 필요할 수 있습니다. 예를 들어 "T" 형식 문자열을 사용 하면 [`TimePicker`](xref:Xamarin.Forms.TimePicker) 보기에서 긴 형식으로 시간을 표시 하 고 "4:15:26 am"은 "4:15 am"의 짧은 시간 형식 ("T") 보다 더 큰 표시 너비가 필요 합니다. 플랫폼에 따라 이러한 차이로 인해 `TimePicker` 보기에서 레이아웃의 너비를 변경 하거나 표시를 자를 수 있습니다.

> [!TIP]
> [@No__t_3](xref:Xamarin.Forms.TimePicker)와 함께 `Fill`의 기본 `HorizontalOptions` 설정을 사용 하는 것이 가장 좋지만 `TimePicker` 셀에 `Grid`을 넣을 때 `Auto` 너비를 사용 하지 않는 것이 좋습니다.

## <a name="timepicker-in-an-application"></a>응용 프로그램의 TimePicker

[**Settimer**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) 샘플에는 해당 페이지에 [`TimePicker`](xref:Xamarin.Forms.TimePicker), [`Entry`](xref:Xamarin.Forms.Entry)및 [`Switch`](xref:Xamarin.Forms.Switch) 보기가 포함 되어 있습니다. @No__t_0를 사용 하 여 시간을 선택할 수 있으며,이 시간이 되 면 `Switch` 설정 된 경우 `Entry`의 텍스트를 사용자에 게 알리는 경고 대화 상자가 표시 됩니다. XAML 파일은 다음과 같습니다.

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

[@No__t_1](xref:Xamarin.Forms.Entry) 를 사용 하면 선택한 시간이 발생할 때 표시 되는 미리 알림 텍스트를 입력할 수 있습니다. [@No__t_1](xref:Xamarin.Forms.TimePicker) 에는 자세한 시간 형식에 대 한 "t"의 [`Format`](xref:Xamarin.Forms.TimePicker.Format) 속성이 할당 됩니다. [@No__t_1](xref:Xamarin.Forms.BindableObject.PropertyChanged) 이벤트에 연결 된 이벤트 처리기가 있으며 [`Switch`](xref:Xamarin.Forms.Switch) 에 [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) 이벤트에 연결 된 처리기가 있습니다. 이러한 이벤트 처리기는 코드 숨김이 파일에 있으며 `SetTriggerTime` 메서드를 호출 합니다.

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

@No__t_0 메서드는 `DateTime.Today` 속성 값 및 [`TimePicker`](xref:Xamarin.Forms.TimePicker)에서 반환 된 `TimeSpan` 값을 기준으로 타이머 시간을 계산 합니다. @No__t_0 속성은 현재 날짜를 나타내는 `DateTime`를 반환 하지만 시간은 자정으로 반환 하기 때문에이 작업이 필요 합니다. 타이머 시간이 이미 오늘 경과 된 경우 내일 이라고 가정 합니다.

초당 타이머 틱은 [`Switch`](xref:Xamarin.Forms.Switch) 의 설정 여부와 현재 시간이 타이머 시간 보다 크거나 같은지 여부를 확인 하는 `OnTimerTick` 메서드를 실행 합니다. 타이머 시간이 발생 하면 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 메서드는 사용자에 게 경고 대화 상자를 미리 알림으로 표시 합니다.

샘플을 처음 실행할 때 [`TimePicker`](xref:Xamarin.Forms.TimePicker) 뷰는 11am으로 초기화 됩니다. @No__t_0 누르면 플랫폼 시간 선택이 호출 됩니다. 플랫폼은 매우 다양 한 방식으로 시간 선택을 구현 하지만 각 접근 방식은 해당 플랫폼의 사용자에 게 친숙 합니다.

[![시간 선택](timepicker-images/timepicker-open.png "시간 선택")](timepicker-images/timepicker-open-large.png#lightbox "시간 선택")

> [!TIP]
> Android에서 사용자 지정 렌더러에서 `CreateTimePickerDialog` 메서드를 재정의 하 여 [`TimePicker`](xref:Xamarin.Forms.TimePicker) 대화 상자를 사용자 지정할 수 있습니다. 이렇게 하면 예를 들어 추가 단추를 대화 상자에 추가할 수 있습니다.

시간을 선택한 후에는 선택한 시간이 [`TimePicker`](xref:Xamarin.Forms.TimePicker)에 표시 됩니다.

[![선택한 시간](timepicker-images/timepicker-selected.png "선택한 시간")](timepicker-images/timepicker-selected-large.png#lightbox "선택한 시간")

[@No__t_1](xref:Xamarin.Forms.Switch) 를 on 위치로 전환 하는 경우 선택한 시간이 발생 하면 응용 프로그램에서 [`Entry`](xref:Xamarin.Forms.Entry) 텍스트를 사용자에 게 알려 주는 경고 대화 상자를 표시 합니다.

[![타이머 팝업](timepicker-images/timer-test.png "타이머 팝업")](timepicker-images/timer-test-large.png#lightbox "타이머 팝업")

경고 대화 상자가 표시 되는 즉시 [`Switch`](xref:Xamarin.Forms.Switch) off 위치로 전환 됩니다.

## <a name="related-links"></a>관련 링크

- [SetTimer 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)
- [TimePicker API](xref:Xamarin.Forms.TimePicker)
