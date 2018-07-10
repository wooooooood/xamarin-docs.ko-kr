---
title: Xamarin.Forms DatePicker
description: DatePicker 날짜를 선택할 수 있도록 Xamarin.Forms 뷰입니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 DatePicker를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/04/2018
ms.openlocfilehash: 31422f1067473ccb49a681098bd0809c451e7369
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935360"
---
# <a name="xamarinforms-datepicker"></a>Xamarin.Forms DatePicker

_날짜를 선택할 수 있도록 하는 Xamarin.Forms 뷰_

Xamarin.Forms [ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) 플랫폼의 날짜 선택 컨트롤을 호출 하 고 사용자가 날짜를 선택할 수 있습니다. `DatePicker` 8 가지 속성을 정의합니다.

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) 형식의 [ `DateTime` ](https://developer.xamarin.com/api/type/System.DateTime/), 1900 년의 첫 번째 날에는 기본값입니다.
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) 형식의 `DateTime`, 2100 연도의 마지막 날에 기본값.
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) 형식의 `DateTime`, 선택한 날짜를 기본값 [ `DateTime.Today` ](https://developer.xamarin.com/api/property/System.DateTime.Today/)합니다.
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) 형식의 `string`, a [표준](/dotnet/standard/base-types/standard-date-and-time-format-strings/) 또는 [사용자 지정](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .NET 형식 문자열 "D"로 기본 설정, 긴 날짜 패턴입니다.
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.TextColor/) 형식의 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/), 기본값은 선택한 날짜를 표시 하는데 사용 된 색 [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/)합니다.
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) 형식의 [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), 기본값은 [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None)합니다.
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) 형식의 `string`, 기본값은 `null`합니다.
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) 형식의 `double`,-1.0 기본값은입니다.

`DatePicker` 발생을 [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) 이벤트 사용자가 날짜를 선택 합니다.

> [!WARNING]
> 설정 하는 경우 `MinimumDate` 및 `MaximumDate`, 했는지 `MinimumDate` 보다 작거나 같음은 항상 `MaximumDate`합니다. 그렇지 않으면 `DatePicker` 예외가 발생 합니다.

내부적으로 `DatePicker` 되도록 `Date` 사이 `MinimumDate` 고 `MaximumDate`(포함). 경우 `MinimumDate` 또는 `MaximumDate` 설정 되어 있도록 `Date` 이들 간에 아닙니다 `DatePicker` 의 값을 조정 하는 `Date`합니다.

8 가지 속성 모두를 지 원하는 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 스타일 지정할 수 있습니다 하 고 속성에는 데이터 바인딩 대상이 될 수 있음을 의미 하는 개체입니다. `Date` 속성이의 기본 바인딩 모드를 [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay)를 사용 하는 응용 프로그램에서 데이터 바인딩 대상을 사용할 수 있다는 의미는 [-Model-view-viewmodel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처입니다.

## <a name="initializing-the-datetime-properties"></a>DateTime 속성 초기화

코드에서 초기화할 수 있습니다는 `MinimumDate`하십시오 `MaximumDate`, 및 `Date` 속성 형식의 값을 `DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

때를 `DateTime` 에 XAML, XAML 파서를 사용 하는 값이 지정를 `DateTime.Parse` 메서드를 `CultureInfo.InvariantCulture` 문자열을 변환 하는 인수를 `DateTime` 값. 정확한 형식으로 날짜를 지정 해야 합니다: 두 자리 월, 두 일 및 슬래시로 구분 네 자리 연도:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

경우는 `BindingContext` 속성을 `DatePicker` 형식의 속성이 포함 된 ViewModel의 인스턴스로 설정 됩니다 `DateTime` 라는 `MinDate`를 `MaxDate`, 및 `SelectedDate` 인스턴스화할 수 있습니다 (예)를 `DatePicker` 같이 :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

이 예제에서는 세 가지 속성 모두는 ViewModel의 해당 속성으로 초기화 됩니다. 때문에 합니다 `Date` 속성에 바인딩 모드 `TwoWay`, 사용자가 ViewModel에 자동으로 반영 되는 새 날짜입니다.

경우는 `DatePicker` 에 바인딩이 없습니다 해당 `Date` 속성을 응용 프로그램에는 처리기를 연결 해야 합니다 `DateSelected` 이벤트 수를 사용자가 새 날짜를 선택 하는 경우 알림을 받지 합니다.

글꼴 속성을 설정 하는 방법에 대 한 내용은 [글꼴](~/xamarin-forms/user-interface/text/fonts.md)합니다.

## <a name="datepicker-and-layout"></a>DatePicker 및 레이아웃

와 같은 비제한 가로 레이아웃 옵션을 사용 하는 것이 불가능 `Center`, `Start`, 또는 `End` 사용 하 여 `DatePicker`:

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

그러나이 권장 되지 않습니다. 설정에 따라는 `Format` 속성을 선택한 날짜에는 다른 표시 너비 필요할 수 있습니다. "D" 형식 문자열을 지정 하면 예를 들어 `DateTime` 긴 형식과 "수요일, 12 2018 년 9 월"에서 날짜를 표시 하려면 "Friday, 4, 2018 년 5 월" 보다 큰 표시 너비입니다. 플랫폼에 따라서는 이러한 차이로 인해 발생할 수 있습니다는 `DateTime` 잘린 것으로 표시 또는 레이아웃의 너비를 변경 합니다.

> [!TIP]
> 기본값을 사용 하는 것이 좋습니다 `HorizontalOptions` 설정 `Fill` 사용 하 여 `DatePicker`, 및의 너비는 사용할 수 없습니다 `Auto` 전환할 때 `DatePicker` 에서 `Grid` 셀입니다.

## <a name="datepicker-in-an-application"></a>응용 프로그램에서 DatePicker

합니다 [ **DaysBetweenDates** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) 샘플에 두는 `DatePicker` 해당 페이지 보기. 이러한 두 날짜를 선택할 수 및 해당 날짜 사이의 일 수를 계산 하는 프로그램입니다. 프로그램의 설정을 변경 하지는 `MinimumDate` 고 `MaximumDate` 속성, 두 날짜는 1900 2100 사이 여야 합니다.

XAML 파일을 다음과 같습니다.

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

각 `DatePicker` 할당 되는 `Format` 긴 날짜 형식에 대 한 "D"의 속성입니다. 또한 합니다 `endDatePicker` 개체에 대상으로 하는 바인딩이 해당 `MinimumDate` 속성입니다. 바인딩 소스는 선택한 `Date` 의 속성을 `startDatePicker` 개체입니다. 이렇게 하면 종료 날짜는 나중에 항상 같거나 시작 날짜입니다. 두 외에도 `DatePicker` 개체는 `Switch` "총에서 모두 일을 포함 합니다." 라고 합니다.

두 `DatePicker` 보기에는 연결 된 처리기는 `DateSelected` 이벤트 및 `Switch` 처리기에 연결 된 해당 `Toggled` 이벤트입니다. 이러한 이벤트 처리기의 코드 숨김 파일에 있으며 두 날짜 사이의 일 새 계산을 트리거:

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

이 샘플은 처음 실행 한 경우, 둘 다 `DatePicker` 뷰 오늘 날짜에 초기화 됩니다. 다음 스크린샷에서 iOS, Android 및 유니버설 Windows 플랫폼에서 실행 중인 프로그램이 보여 줍니다.

[![시작 날짜 사이의 일](datepicker-images/DaysBetweenDatesStart.png "시작 날짜 사이의 일")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "시작 날짜 사이의 일")

탭 중 하나는 `DatePicker` 플랫폼 날짜 선택기를 호출 하는 표시 됩니다. 매우 다양 한 방식에서이 날짜 선택기를 구현 하는 세 가지 플랫폼 이지만 각 접근 방식은 해당 플랫폼의 사용자에 게 친숙 합니다.

[![날짜 사이의 날짜 선택](datepicker-images/DaysBetweenDatesSelect.png "날짜 사이의 날짜 선택")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "날짜 사이의 날짜 선택")

두 날짜를 선택한 후 응용 프로그램에는 해당 날짜 사이의 일 수가 표시 됩니다.

[![결과 날짜 사이의 일](datepicker-images/DaysBetweenDatesResult.png "결과 날짜 사이의 일")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "결과 날짜 사이의 일")

## <a name="related-links"></a>관련 링크

- [DaysBetweenDates 샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)
- [DatePicker API](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)
