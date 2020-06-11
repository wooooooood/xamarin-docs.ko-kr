---
제목: " Xamarin.Forms DatePicker" description: "DatePicker은 Xamarin.Forms 사용자가 날짜를 선택할 수 있는 뷰입니다. 이 문서에서는 응용 프로그램에서 DatePicker를 사용 하는 방법을 설명 Xamarin.Forms 합니다. "
assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9: xamarin-forms author: davidbritch: dabritch:: 06/04/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-datepicker"></a>Xamarin.FormsDatePicker

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)

_Xamarin.Forms사용자가 날짜를 선택할 수 있는 뷰입니다._

는 Xamarin.Forms [`DatePicker`](xref:Xamarin.Forms.DatePicker) 플랫폼의 날짜 선택 컨트롤을 호출 하 고 사용자가 날짜를 선택할 수 있게 합니다. `DatePicker`는 8 개의 속성을 정의 합니다.

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate)[`DateTime`](xref:System.DateTime)1900 년의 첫 번째 날로 기본 설정 된 형식입니다.
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate)`DateTime`2100 년의 마지막 날로 기본 설정 되는 형식의입니다.
- [`Date`](xref:Xamarin.Forms.DatePicker.Date)형식으로 `DateTime` , 선택 된 날짜로, 값이 기본값으로 설정 [`DateTime.Today`](xref:System.DateTime.Today) 됩니다.
- [`Format`](xref:Xamarin.Forms.DatePicker.Format)`string` [표준](/dotnet/standard/base-types/standard-date-and-time-format-strings/) 또는 [사용자 지정](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .net 형식 문자열의 형식으로, 기본적으로 "D"는 long 날짜 패턴입니다.
- [`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor)형식으로 [`Color`](xref:Xamarin.Forms.Color) , 선택 된 날짜를 표시 하는 데 사용 되는 색입니다. 기본값은 [`Color.Default`](xref:Xamarin.Forms.Color.Default) 입니다.
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes)형식의 [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) 이며 기본값은 [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None) 입니다.
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily)형식의 `string` 이며 기본값은 `null` 입니다.
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize)형식의 `double` 이며 기본값은-1.0입니다.
- `double` 형식의 `CharacterSpacing`은 `DatePicker` 텍스트를 구성하는 문자 사이의 간격입니다.

는 `DatePicker` [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) 사용자가 날짜를 선택할 때 이벤트를 발생 시킵니다.

> [!WARNING]
> 및를 설정 하는 경우 `MinimumDate` `MaximumDate` `MinimumDate` 가 항상 보다 작거나 같은지 확인 `MaximumDate` 합니다. 그렇지 않으면가 `DatePicker` 예외를 발생 시킵니다.

내부적으로는 `DatePicker` `Date` 가 `MinimumDate` 와 (포함) 사이에 있는지 확인 `MaximumDate` 합니다. `MinimumDate`또는 `MaximumDate` 가 설정 되어 있지 않기 때문 `Date` 에 `DatePicker` 의 값이 조정 됩니다 `Date` .

모든 8 개의 속성은 개체에 의해 지원 되므로 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 스타일을 지정할 수 있으며 속성은 데이터 바인딩의 대상이 될 수 있습니다. `Date`속성의 기본 바인딩 모드를 사용 하는 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) 경우이는 [MVVM (모델-뷰-ViewModel)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처를 사용 하는 응용 프로그램의 데이터 바인딩 대상이 될 수 있음을 의미 합니다.

## <a name="initializing-the-datetime-properties"></a>DateTime 속성 초기화

코드에서 `MinimumDate` `MaximumDate` `Date` 형식의 값에 대 한, 및 속성을 초기화할 수 있습니다 `DateTime` .

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

`DateTime`Xaml에서 값을 지정 하면 xaml 파서는 인수를 사용 하 여 문자열을 값으로 변환 하는 메서드를 사용 합니다 `DateTime.Parse` `CultureInfo.InvariantCulture` `DateTime` . 날짜는 다음과 같이 정확한 형식으로 지정 해야 합니다. 두 자리 월, 두 자리 날짜 및 네 자리 연도는 슬래시로 구분 됩니다.

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

`BindingContext`의 속성이 `DatePicker` , 및 형식의 속성을 포함 하는 viewmodel의 인스턴스로 설정 된 경우 `DateTime` `MinDate` `MaxDate` `SelectedDate` (예:) 다음과 같이를 인스턴스화할 수 있습니다 `DatePicker` .

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

이 예제에서는 세 개의 속성이 모두 viewmodel의 해당 속성으로 초기화 됩니다. 속성에는 `Date` 의 바인딩 모드가 있으므로 `TwoWay` 사용자가 선택 하는 모든 새 날짜는 viewmodel에 자동으로 반영 됩니다.

에 해당 `DatePicker` 속성에 대 한 바인딩이 없는 경우 `Date` 응용 프로그램은 `DateSelected` 사용자가 새 날짜를 선택할 때 알리도록 이벤트에 처리기를 연결 해야 합니다.

글꼴 속성을 설정 하는 방법에 대 한 자세한 내용은 [글꼴](~/xamarin-forms/user-interface/text/fonts.md)을 참조 하십시오.

## <a name="datepicker-and-layout"></a>DatePicker 및 레이아웃

, 또는와 같은 제한 되지 않은 가로 레이아웃 옵션을 사용할 `Center` 수 `Start` `End` `DatePicker` 있습니다.

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

그러나이 방법은 권장 되지 않습니다. 속성의 설정에 따라 `Format` 선택 된 날짜에 다른 표시 너비가 필요할 수 있습니다. 예를 들어, "D" 형식 문자열은 `DateTime` 날짜를 긴 형식으로 표시 하 고 "수요일, 9 월 12 일에 2018"는 "금요일, 5 월 4 일 2018" 보다 큰 표시 너비를 요구 합니다. 플랫폼에 따라 이러한 차이로 인해 `DateTime` 보기가 레이아웃의 너비를 변경 하거나 표시를 잘라낼 수 있습니다.

> [!TIP]
> 에서의 기본 설정을 사용 하는 것이 가장 좋습니다 `HorizontalOptions` .이 설정은 `Fill` `DatePicker` 셀에 넣을 때 너비를 사용 하지 않습니다 `Auto` `DatePicker` `Grid` .

## <a name="datepicker-in-an-application"></a>응용 프로그램의 DatePicker

[**DaysBetweenDates**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) 샘플에는 `DatePicker` 해당 페이지에 두 개의 뷰가 포함 되어 있습니다. 이러한 값은 두 날짜를 선택 하는 데 사용할 수 있으며 프로그램은 해당 날짜 사이의 일 수를 계산 합니다. 프로그램은 및 속성의 설정을 변경 하지 `MinimumDate` `MaximumDate` 않으므로 두 날짜가 1900에서 2100 사이 여야 합니다.

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

각 `DatePicker` `Format` 에는 긴 날짜 형식의 "D" 속성이 할당 됩니다. 또한 개체에는 `endDatePicker` 해당 속성을 대상으로 하는 바인딩이 있습니다 `MinimumDate` . 바인딩 소스는 `Date` 개체의 선택 된 속성입니다 `startDatePicker` . 이를 통해 종료 날짜는 항상 시작 날짜와 같거나 그 이후 여야 합니다. 두 개체 외에도 `DatePicker` 에는 `Switch` "모두 포함 된 총 일 수" 라는 레이블이 지정 됩니다.

두 `DatePicker` 뷰에는 이벤트에 연결 된 처리기가 `DateSelected` 있으며,에는 해당 `Switch` 이벤트에 연결 된 `Toggled` 처리기가 있습니다. 이러한 이벤트 처리기는 코드 숨김이 파일에 있으며 두 날짜 사이에 일의 새 계산을 트리거합니다.

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

샘플을 처음 실행할 때 두 뷰 모두 `DatePicker` 오늘 날짜로 초기화 됩니다. 다음 스크린샷에서는 iOS 및 Android에서 실행 되는 프로그램을 보여 줍니다.

[![날짜 사이의 일 수](datepicker-images/DaysBetweenDatesStart.png "날짜 사이의 일 수")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "날짜 사이의 일 수")

디스플레이 중 하나를 누르면 `DatePicker` 플랫폼 날짜 선택이 호출 됩니다. 플랫폼은이 날짜 선택기를 매우 다양 한 방식으로 구현 하지만 각 접근 방식은 해당 플랫폼의 사용자에 게 친숙 합니다.

[![날짜 사이의 일 수 선택](datepicker-images/DaysBetweenDatesSelect.png "날짜 사이의 일 수 선택")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "날짜 사이의 일 수 선택")

> [!TIP]
> Android에서 `DatePicker` `CreateDatePickerDialog` 사용자 지정 렌더러에서 메서드를 재정의 하 여 대화 상자를 사용자 지정할 수 있습니다. 이렇게 하면 예를 들어 추가 단추를 대화 상자에 추가할 수 있습니다.

두 날짜를 선택 하면 응용 프로그램은 해당 날짜 사이의 일 수를 표시 합니다.

[![날짜 결과의 일 수](datepicker-images/DaysBetweenDatesResult.png "날짜 결과의 일 수")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "날짜 결과의 일 수")

## <a name="related-links"></a>관련 링크

- [DaysBetweenDates 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)
- [DatePicker API](xref:Xamarin.Forms.DatePicker)
