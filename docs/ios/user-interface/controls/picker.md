---
title: Xamarin.iOS에서 선택 컨트롤
description: 이 문서에서는 디자인 하 여 Xamarin.iOS 앱에 대 한 선택 컨트롤을 사용 하는 방법을 설명 합니다. IOS 디자이너 및 코드에서에 선택기를 구현 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/14/2018
ms.openlocfilehash: 946cba08e1e504962c093f67e336d72b654a3a41
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61229161"
---
# <a name="picker-control-in-xamarinios"></a>Xamarin.iOS에서 선택 컨트롤

A [ `UIPickerView` ](xref:UIKit.UIPickerView) 휠 유사한 인터페이스의 개별 구성 요소를 스크롤하여 목록에서 값을 선택할 수 있습니다.

선택기는 날짜 및 시간을 선택 하는 데 자주 사용 됩니다. Apple에서 제공 합니다 [`UIDatePicker`](xref:UIKit.UIDatePicker)
이 목적을 위해 클래스입니다.

구현 및 사용 하는 방법을 설명 합니다 `UIPickerView` 고 `UIDatePicker` 컨트롤입니다.

## <a name="uipickerview"></a>UIPickerView

### <a name="implementing-a-picker"></a>선택기를 구현합니다.

새 인스턴스화하여 선택기를 구현 `UIPickerView`:

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width, 
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
    )
);
```

### <a name="pickers-and-storyboards"></a>선택기 및 스토리 보드

에 선택기를 만들려면 합니다 **iOS 디자이너**, 끌어를 **선택 보기** 에서 **도구 상자** 디자인 화면으로.

![선택 뷰를 디자인 화면으로 끌어](picker-images/image1.png "선택 뷰를 디자인 화면에 끌어 옵니다.")

### <a name="working-with-a-picker-control"></a>선택 컨트롤 사용

선택기를 사용 하는 _모델_ 데이터와 상호 작용 합니다.

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    var pickerModel = new PeopleModel(personLabel);
    personPicker.Model = pickerModel;
}
```

합니다 [ `UIPickerViewModel` ](xref:UIKit.UIPickerViewModel) 두 인터페이스를 구현 하는 기본 클래스 [`IUIPickerDataSource`](xref:UIKit.IUIPickerViewDataSource)
및 [ `IUIPickerViewDelegate` ](xref:UIKit.IUIPickerViewDelegate)선택기의 데이터를 지정 하는 다양 한 메서드를 선언 하 고 상호 작용을 처리 하는 방법을:

```csharp
public class PeopleModel : UIPickerViewModel
{
    public string[] names = new string[] {
            "Amy Burns",
            "Kevin Mullins",
            "Craig Dunn",
            "Joel Martinez",
            "Charles Petzold",
            "David Britch",
            "Mark McLemore",
            "Tom Opegenorth",
            "Joseph Hill",
            "Miguel De Icaza"
        };

    private UILabel personLabel;

    public PeopleModel(UILabel personLabel)
    {
        this.personLabel = personLabel;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 2;
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return names.Length;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        if (component == 0)
            return names[row];
        else
            return row.ToString();
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        personLabel.Text = $"This person is: {names[pickerView.SelectedRowInComponent(0)]},\n they are number {pickerView.SelectedRowInComponent(1)}";
    }

    public override nfloat GetComponentWidth(UIPickerView picker, nint component)
    {
        if (component == 0)
            return 240f;
        else
            return 40f;
    }

    public override nfloat GetRowHeight(UIPickerView picker, nint component)
    {
        return 40f;
    }
```

선택기는 열을 여러 개 있을 수 있습니다 또는 _구성 요소_합니다. 구성 요소에 파티션 여러 섹션으로 쉽고 보다 구체적인 데이터 선택에 대 한 허용을 선택:

![두 구성 요소를 사용 하 여 선택기](picker-images/image3.png "두 구성 요소를 사용 하 여 선택")

선택에서 구성 요소 수를 지정 하려면 사용 합니다 [`GetComponentCount`](xref:UIKit.UIPickerViewModel.GetComponentCount(UIKit.UIPickerView)) 
메서드를 재정의합니다.

### <a name="customizing-a-pickers-appearance"></a>선택기의 모양 사용자 지정

선택기의 모양을 사용자 지정 하려면 사용 합니다 [`UIPickerView.UIPickerViewAppearance`](xref:UIKit.UIPickerView.UIPickerViewAppearance)
클래스 또는 재정의 [ `GetView` ](xref:UIKit.UIPickerViewModel.GetView(UIKit.UIPickerView,System.nint,System.nint,UIKit.UIView)) 하 고 [ `GetRowHeight` ](xref:UIKit.UIPickerViewModel.GetRowHeight(UIKit.UIPickerView,System.nint)) 의 메서드는 `UIPickerViewModel`합니다.

## <a name="uidatepicker"></a>UIDatePicker

### <a name="implementing-a-date-picker"></a>날짜 선택기를 구현합니다.

날짜 선택기를 인스턴스화하여 구현 된 `UIDatePicker`:

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width,
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
     )
);
```

### <a name="date-pickers-and-storyboards"></a>날짜 선택 및 스토리 보드

날짜 선택기를 만들려면 합니다 **iOS 디자이너**, 끌어를 **날짜 선택** 에서 **도구 상자** 디자인 화면으로.

![날짜 선택기를 디자인 화면으로 끌어](picker-images/image2.png "날짜 선택기를 디자인 화면으로 끌어 옵니다.")

### <a name="date-picker-properties"></a>날짜 선택 속성

#### <a name="minimum-and-maximum-date"></a>최소 및 최대 날짜

[`MinimumDate`](xref:UIKit.UIDatePicker.MinimumDate) 및 [ `MaximumDate` ](xref:UIKit.UIDatePicker.MaximumDate) 날짜 선택 컨트롤에서 사용할 수 있는 날짜 범위를 제한 합니다. 예를 들어, 다음 코드는 제한 있는 현재 이르는 60 년 날짜 선택:

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();
components.Year = -60;
NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);
datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

> [!TIP]
> 명시적으로 캐스팅할 수는 `DateTime` 에 `NSDate`:
> ```csharp
> DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
> DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
> ```

#### <a name="minute-interval"></a>분 간격

합니다 [ `MinuteInterval` ](xref:UIKit.UIDatePicker.MinuteInterval) 속성 선택기는 분을 표시 하는 데는 간격을 설정 합니다.

```csharp
datePickerView.MinuteInterval = 10;
```

#### <a name="mode"></a>모드

날짜 선택 지원 네 [모드](xref:UIKit.UIDatePickerMode)아래 설명:

##### <a name="uidatepickermodetime"></a>UIDatePickerMode.Time

`UIDatePickerMode.Time` 시간 및 분 선택기를 및 선택적는 AM 또는 PM 지정을 사용 하 여 시간을 표시합니다.

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

![UIDatePickerMode.Time](picker-images/image8.png "UIDatePickerMode.Time")

##### <a name="uidatepickermodedate"></a>UIDatePickerMode.Date

`UIDatePickerMode.Date` 월, 일 및 연도 선택기를 사용 하 여 날짜를 표시합니다.

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

![UIDatePickerMode.Date](picker-images/image7.png "UIDatePickerMode.Date")

선택기의 순서는 기본적으로 시스템 로캘은 날짜 선택기의 로캘에 따라 달라 집니다. 위의 이미지에서 선택기의 레이아웃을 보여 줍니다.는 `en_US` 로캘, 하지만 다음 날에 순서를 변경 | 월 | 연도:

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![일 | 월 | 연도](picker-images/image9.png "일 | 월 | 연도")

##### <a name="uidatepickermodedateandtime"></a>UIDatePickerMode.DateAndTime

`UIDatePickerMode.DateAndTime` 날짜, 시간 및 분 (12 또는 24 시간제 사용 되는지 여부) 따라 선택적 AM 또는 PM 지정 시간으로 단축 표시:

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

![UIDatePickerMode.DateAndTime](picker-images/image6.png "UIDatePickerMode.DateAndTime")

와 마찬가지로 [ `UIDatePickerMode.Date` ](#uidatepickermodedate), 날짜 선택의 로캘에 따라 달라 집니다 선택기의 순서 및 12 또는 24 시간제를 사용 합니다.

> [!TIP]
> 사용 합니다 `Date` 속성을 날짜 선택 모드에서 변수의 값을 캡처할 `UIDatePickerMode.Time`를 `UIDatePickerMode.Date`, 또는 `UIDatePickerMode.DateAndTime`합니다. 이 값으로 저장 됩니다는 `NSDate`합니다.

##### <a name="uidatepickermodecountdowntimer"></a>UIDatePickerMode.CountDownTimer

`UIDatePickerMode.CountDownTimer` 시간 및 분 값을 표시합니다.

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

!["UIDatePickerMode.CountDownTimer"](picker-images/image5.png "UIDatePickerMode.CountDownTimer")

합니다 `CountDownDuration` 속성에서 날짜 선택의 가치를 캡처하 `UIDatePickerMode.CountDownTimer` 모드입니다. 예를 들어 현재 날짜에 카운트다운 값을 추가 합니다.

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="nsdateformatter"></a>NSDateFormatter

형식을 지정 하는 `NSDate`를 사용 하 여는 [ `NSDateFormatter` ](xref:Foundation.NSDateFormatter)합니다.

사용 하는 `NSDateFormatter`, 호출 해당 [ `ToString` ](xref:Foundation.NSDateFormatter.ToString(Foundation.NSDate)) 메서드. 예를 들어:

```csharp
var date = NSDate.Now;
var formatter = new NSDateFormatter();
formatter.DateStyle = NSDateFormatterStyle.Full;
formatter.TimeStyle = NSDateFormatterStyle.Full;
var formattedDate = formatter.ToString(d);
// Tuesday, August 14, 2018 at 11:20:42 PM Mountain Daylight Time
```

##### <a name="dateformat"></a>DateFormat

[ `DateFormat` ](xref:Foundation.NSDateFormatter.DateFormat) 속성 (문자열)는 `NSDateFormatter` 사용자 지정 가능한 날짜 형식 지정을 허용 합니다.

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

##### <a name="timestyle"></a>TimeStyle

[ `TimeStyle` ](xref:Foundation.NSDateFormatter.TimeStyle) 속성 (프로그램 [ `NSDateFormatterStyle` ](xref:Foundation.NSDateFormatterStyle) 의 `NSDateFormatter` 미리 결정 된 스타일을 기반으로 시간 형식 지정:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

다양 한 `NSDateFormatterStyle` 값 시간을 다음과 같이 표시 합니다.

- `NSDateFormatterStyle.Full`: 46: 오후 7:00 동부 일광 절약 시간
- `NSDateFormatterStyle.Long`: 7:47:00 PM EDT
- `NSDateFormatterStyle.Medium`: 47: 오후 7:00
- `NSDateFormatterSytle.Short`: 47 오후 7

##### <a name="datestyle"></a>DateStyle

합니다 [ `DateStyle` ](xref:Foundation.NSDateFormatter.DateStyle) 속성 (을 `NSDateFormatterStyle`)의 `NSDateFormatter` 미리 결정 된 스타일에 따라 날짜 서식 지정:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

다양 한 `NSDateFormatterStyle` 값이 다음과 같이 날짜를 표시 합니다.

- `NSDateFormatterStyle.Full`: 수요일, 2017 년 8 월 2, 오후 7 시 48 분
- `NSDateFormatterStyle.Long`: 2017 년 8 월 2, 오후 7 시 49 분
- `NSDateFormatterStyle.Medium`: 2017 년 8 월 2, 오후 7시 49분
- `NSDateFormatterStyle.Short`: 8/2/17을 오후 7 시 50 분

> [!NOTE]
> `DateFormat` 및 `DateStyle` / `TimeStyle` 날짜 및 시간 형식 지정 하는 여러 가지 방법을 제공 합니다. 가장 최근에 집합 속성 날짜 포맷터의 출력을 결정 합니다.

## <a name="related-links"></a>관련 링크

- [PickerControl (샘플)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
