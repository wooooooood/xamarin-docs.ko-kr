---
title: Xamarin.ios DatePicker
description: DatePicker는 사용자가 날짜를 선택할 수 있는 Xamarin.ios 뷰입니다. 이 문서에서는 Xamarin.ios 응용 프로그램에서 DatePicker를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 15d4159d89a463c0335d9c91b24b55151c91de8c
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695902"
---
# <a name="xamarinforms-datepicker"></a>Xamarin.ios DatePicker

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)

_사용자가 날짜를 선택할 수 있는 Xamarin.ios 뷰입니다._

Xamarin.ios [`DatePicker`](xref:Xamarin.Forms.DatePicker) 는 플랫폼의 날짜 선택기 컨트롤을 호출 하 고 사용자가 날짜를 선택할 수 있습니다. `DatePicker` 8 개의 속성을 정의 합니다.

- 1900 년의 첫째 날로 기본 설정 된 [`DateTime`](xref:System.DateTime)형식의 [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) 입니다.
- 2100 년의 마지막 날로 기본 설정 된 `DateTime` 형식의 [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) 입니다.
- 선택 된 날짜로 `DateTime` 형식 [`Date`](xref:Xamarin.Forms.DatePicker.Date) [`DateTime.Today`](xref:System.DateTime.Today)값이 기본값으로 설정 됩니다.
- 표준 또는 사용자 지정 .Net 형식 문자열 ("D"로 기본 설정 된 [표준](/dotnet/standard/base-types/standard-date-and-time-format-strings/) 또는 [사용자 지정](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .net `string` 형식 문자열)의 [`Format`](xref:Xamarin.Forms.DatePicker.Format) 는 long 날짜 패턴입니다.
- [`Color`](xref:Xamarin.Forms.Color)형식의 [`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor) 선택 된 날짜를 표시 하는 데 사용 되는 색입니다. 기본값은 [`Color.Default`](xref:Xamarin.Forms.Color.Default)입니다.
- [`FontAttributes`](xref:Xamarin.Forms.FontAttributes)형식의 [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) 이며 기본값은 [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None)입니다.
- `string` 형식의 [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) 이며 기본값은 `null`입니다.
- `double` 형식의 [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) 이며 기본값은-1.0입니다.
- `double` 형식의 `CharacterSpacing`은 `DatePicker` 텍스트 문자 사이의 간격입니다.

@No__t_0는 사용자가 날짜를 선택할 때 [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) 이벤트를 발생 시킵니다.

> [!WARNING]
> @No__t_0 및 `MaximumDate`를 설정 하는 경우 `MinimumDate`가 항상 `MaximumDate` 보다 작거나 같은지 확인 합니다. 그렇지 않으면 `DatePicker`는 예외를 발생 시킵니다.

내부적으로 `DatePicker`는 `Date` `MinimumDate`와 `MaximumDate` 사이에 있는지 확인 합니다. @No__t_0 또는 `MaximumDate`를 설정 하 여 `Date` 사이에 있지 않으면 `DatePicker`는 `Date` 값을 조정 합니다.

모든 8 개의 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, 스타일을 지정할 수 있으며 속성은 데이터 바인딩의 대상이 될 수 있습니다. @No__t_0 속성은 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)의 기본 바인딩 모드를 사용 하며,이는 [MVVM (모델-뷰-ViewModel)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처를 사용 하는 응용 프로그램의 데이터 바인딩 대상이 될 수 있음을 의미 합니다.

## <a name="initializing-the-datetime-properties"></a>DateTime 속성 초기화

코드에서는 `MinimumDate`, `MaximumDate` 및 `Date` 속성을 `DateTime` 형식의 값으로 초기화할 수 있습니다.

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

XAML에서 `DateTime` 값을 지정 하면 XAML 파서는 `CultureInfo.InvariantCulture` 인수와 함께 `DateTime.Parse` 메서드를 사용 하 여 문자열을 `DateTime` 값으로 변환 합니다. 날짜는 다음과 같이 정확한 형식으로 지정 해야 합니다. 두 자리 월, 두 자리 날짜 및 네 자리 연도는 슬래시로 구분 됩니다.

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

@No__t_1의 `BindingContext` 속성이 `DateTime` 형식의 속성 (예: `MinDate`, `MaxDate` 및 `SelectedDate`)을 포함 하는 viewmodel 인스턴스로 설정 된 경우 다음과 같이 `DatePicker`을 인스턴스화할 수 있습니다. :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

이 예제에서는 세 개의 속성이 모두 viewmodel의 해당 속성으로 초기화 됩니다. @No__t_0 속성에는 `TwoWay`의 바인딩 모드가 있으므로 사용자가 선택 하는 모든 새 날짜는 viewmodel에 자동으로 반영 됩니다.

@No__t_0에 `Date` 속성에 대 한 바인딩이 포함 되어 있지 않은 경우 응용 프로그램은 사용자가 새 날짜를 선택할 때 알리도록 `DateSelected` 이벤트에 처리기를 연결 해야 합니다.

글꼴 속성을 설정 하는 방법에 대 한 자세한 내용은 [글꼴](~/xamarin-forms/user-interface/text/fonts.md)을 참조 하십시오.

## <a name="datepicker-and-layout"></a>DatePicker 및 레이아웃

@No__t_3를 사용 하 여 `Center`, `Start` 또는 `End`와 같은 제한 되지 않은 가로 레이아웃 옵션을 사용할 수 있습니다.

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

그러나이 방법은 권장 되지 않습니다. @No__t_0 속성의 설정에 따라 선택한 날짜에는 다른 표시 너비가 필요할 수 있습니다. 예를 들어, "D" 형식 문자열을 사용 하면 `DateTime`는 긴 형식으로 날짜를 표시 하 고 "수요일, 9 월 12 일에 2018"는 "금요일, 5 월 4 일 2018" 보다 큰 표시 너비가 필요 합니다. 플랫폼에 따라 이러한 차이로 인해 `DateTime` 보기에서 레이아웃의 너비를 변경 하거나 표시를 자를 수 있습니다.

> [!TIP]
> @No__t_2와 함께 `Fill`의 기본 `HorizontalOptions` 설정을 사용 하는 것이 가장 좋지만 `DatePicker` 셀에 `Grid`을 넣을 때 `Auto` 너비를 사용 하지 않는 것이 좋습니다.

## <a name="datepicker-in-an-application"></a>응용 프로그램의 DatePicker

[**DaysBetweenDates**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) 샘플에는 해당 페이지에 두 개의 `DatePicker` 뷰가 포함 되어 있습니다. 이러한 값은 두 날짜를 선택 하는 데 사용할 수 있으며 프로그램은 해당 날짜 사이의 일 수를 계산 합니다. 프로그램은 `MinimumDate` 및 `MaximumDate` 속성의 설정을 변경 하지 않으므로 두 날짜가 1900에서 2100 사이 여야 합니다.

XAML 파일은 다음과 같습니다.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DaysBetweenDates"
             x:Class="DaysBetweenDates.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <StackLayout Margin="10">
        <Label Text="Days Between Dates"
               Style="{DynamicResource TitleStyle}"
               Margin="0, 20"
               HorizontalTextAlignment="Center" />

        <Label Text="Start Date:" />

        <DatePicker x:Name="startDatePicker"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <Label Text="End Date:" />

        <DatePicker x:Name="endDatePicker"
                    MinimumDate="{Binding Source={x:Reference startDatePicker},
                                          Path=Date}"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <StackLayout Orientation="Horizontal"
                     Margin="0, 0, 0, 30">
            <Label Text="Include both days in total: "
                   VerticalOptions="Center" />
            <Switch x:Name="includeSwitch"
                    Toggled="OnSwitchToggled" />
        </StackLayout>

        <Label x:Name="resultLabel"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

각 `DatePicker`에는 긴 날짜 형식으로 "D"의 `Format` 속성이 할당 됩니다. 또한 `endDatePicker` 개체에는 `MinimumDate` 속성을 대상으로 하는 바인딩이 있습니다. 바인딩 소스는 `startDatePicker` 개체의 선택 된 `Date` 속성입니다. 이를 통해 종료 날짜는 항상 시작 날짜와 같거나 그 이후 여야 합니다. 두 `DatePicker` 개체 외에도 `Switch`은 "모두 포함 된 총 일 수"로 표시 됩니다.

두 `DatePicker` 뷰에는 `DateSelected` 이벤트에 연결 된 처리기가 있으며 `Switch`에는 `Toggled` 이벤트에 연결 된 처리기가 있습니다. 이러한 이벤트 처리기는 코드 숨김이 파일에 있으며 두 날짜 사이에 일의 새 계산을 트리거합니다.

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    void OnDateSelected(object sender, DateChangedEventArgs args)
    {
        Recalculate();
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        Recalculate();
    }

    void Recalculate()
    {
        TimeSpan timeSpan = endDatePicker.Date - startDatePicker.Date +
            (includeSwitch.IsToggled ? TimeSpan.FromDays(1) : TimeSpan.Zero);

        resultLabel.Text = String.Format("{0} day{1} between dates",
                                            timeSpan.Days, timeSpan.Days == 1 ? "" : "s");
    }
}
```

샘플을 처음 실행할 때 두 `DatePicker` 뷰가 오늘 날짜로 초기화 됩니다. 다음 스크린샷은 iOS, Android 및 유니버설 Windows 플랫폼에서 실행 되는 프로그램을 보여 줍니다.

[![날짜 사이의 일 수](datepicker-images/DaysBetweenDatesStart.png "날짜 사이의 일 수")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "날짜 사이의 일 수")

@No__t_0 표시 중 하나를 누르면 플랫폼 날짜 선택이 호출 됩니다. 플랫폼은이 날짜 선택기를 매우 다양 한 방식으로 구현 하지만 각 접근 방식은 해당 플랫폼의 사용자에 게 친숙 합니다.

[![날짜 사이의 일 수 선택](datepicker-images/DaysBetweenDatesSelect.png "날짜 사이의 일 수 선택")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "날짜 사이의 일 수 선택")

> [!TIP]
> Android에서 사용자 지정 렌더러에서 `CreateDatePickerDialog` 메서드를 재정의 하 여 `DatePicker` 대화 상자를 사용자 지정할 수 있습니다. 이렇게 하면 예를 들어 추가 단추를 대화 상자에 추가할 수 있습니다.

두 날짜를 선택 하면 응용 프로그램은 해당 날짜 사이의 일 수를 표시 합니다.

[![날짜 결과의 일 수](datepicker-images/DaysBetweenDatesResult.png "날짜 결과의 일 수")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "날짜 결과의 일 수")

## <a name="related-links"></a>관련 링크

- [DaysBetweenDates 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)
- [DatePicker API](xref:Xamarin.Forms.DatePicker)
