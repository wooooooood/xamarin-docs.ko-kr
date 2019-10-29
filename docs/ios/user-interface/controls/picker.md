---
title: Xamarin.ios의 선택 컨트롤
description: 이 문서에서는 Xamarin.ios 앱에서 선택기 컨트롤을 디자인 하 고 사용 하는 방법을 설명 합니다. 코드 및 iOS 디자이너에서 선택기를 구현 하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/14/2018
ms.openlocfilehash: ac96363378e91c60956d28352535733c7e954e6a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021990"
---
# <a name="picker-control-in-xamarinios"></a>Xamarin.ios의 선택 컨트롤

[`UIPickerView`](xref:UIKit.UIPickerView) 를 사용 하면 휠와 유사한 인터페이스의 개별 구성 요소를 스크롤하여 목록에서 값을 선택할 수 있습니다.

선택기는 날짜 및 시간을 선택 하는 데 자주 사용 됩니다. Apple에서 제공 하는 [`UIDatePicker`](xref:UIKit.UIDatePicker)
이 용도의 클래스입니다.

이 문서에서는 `UIPickerView` 및 `UIDatePicker` 컨트롤을 구현 하 고 사용 하는 방법을 설명 합니다.

## <a name="uipickerview"></a>UIPickerView

### <a name="implementing-a-picker"></a>선택 구현

새 `UIPickerView`을 인스턴스화하여 선택기를 구현 합니다.

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

### <a name="pickers-and-storyboards"></a>선택 및 스토리 보드

**IOS 디자이너**에서 선택기를 만들려면 **도구 상자** 에서 디자인 화면으로 **선택 뷰** 를 끌어 옵니다.

![선택 뷰를 디자인 화면으로 끌어 옵니다.](picker-images/image1.png "선택 뷰를 디자인 화면으로 끌어 옵니다.")

### <a name="working-with-a-picker-control"></a>선택 컨트롤 작업

선택기는 _모델_ 을 사용 하 여 데이터와 상호 작용 합니다.

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    var pickerModel = new PeopleModel(personLabel);
    personPicker.Model = pickerModel;
}
```

[`UIPickerViewModel`](xref:UIKit.UIPickerViewModel) 기본 클래스는 두 개의 인터페이스를 구현 [`IUIPickerDataSource`](xref:UIKit.IUIPickerViewDataSource)
그리고 [`IUIPickerViewDelegate`](xref:UIKit.IUIPickerViewDelegate)는 선택기의 데이터를 지정 하는 다양 한 메서드와 상호 작용을 처리 하는 방법을 선언 합니다.

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

선택기에는 여러 열 또는 _구성 요소가_있을 수 있습니다. 구성 요소는 선택기를 여러 섹션으로 분할 하 여 더 쉽고 구체적인 데이터를 선택할 수 있도록 합니다.

![두 구성 요소가 있는 선택기](picker-images/image3.png "두 구성 요소가 있는 선택기")

선택기의 구성 요소 수를 지정 하려면 [`GetComponentCount`](xref:UIKit.UIPickerViewModel.GetComponentCount(UIKit.UIPickerView)) 을 사용 합니다. 
메서드를 재정의합니다.

### <a name="customizing-a-pickers-appearance"></a>선택기의 모양 사용자 지정

선택 모양을 사용자 지정 하려면 [`UIPickerView.UIPickerViewAppearance`](xref:UIKit.UIPickerView.UIPickerViewAppearance) 을 사용 합니다.
클래스 또는 `UIPickerViewModel`의 [`GetView`](xref:UIKit.UIPickerViewModel.GetView(UIKit.UIPickerView,System.nint,System.nint,UIKit.UIView)) 및 [`GetRowHeight`](xref:UIKit.UIPickerViewModel.GetRowHeight(UIKit.UIPickerView,System.nint)) 메서드를 재정의 합니다.

## <a name="uidatepicker"></a>UIDatePicker

### <a name="implementing-a-date-picker"></a>날짜 선택 구현

`UIDatePicker`를 인스턴스화하여 날짜 선택기를 구현 합니다.

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

### <a name="date-pickers-and-storyboards"></a>날짜 선택기 및 스토리 보드

**IOS 디자이너**에서 날짜 선택을 만들려면 **도구 상자** 에서 디자인 화면으로 **날짜 선택을** 끌어 옵니다.

![날짜 선택기를 디자인 화면으로 끌어 옵니다.](picker-images/image2.png "날짜 선택기를 디자인 화면으로 끌어 옵니다.")

### <a name="date-picker-properties"></a>날짜 선택 속성

#### <a name="minimum-and-maximum-date"></a>최소 및 최대 날짜

[`MinimumDate`](xref:UIKit.UIDatePicker.MinimumDate) 및 [`MaximumDate`](xref:UIKit.UIDatePicker.MaximumDate) 날짜 선택에서 사용할 수 있는 날짜 범위를 제한 합니다. 예를 들어 다음 코드는 날짜 선택을 현재 시간 까지의 60 년으로 제한 합니다.

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
> `DateTime`를 `NSDate`에 명시적으로 캐스팅할 수 있습니다.
>
> ```csharp
> DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
> DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
> ```

#### <a name="minute-interval"></a>분 간격

[`MinuteInterval`](xref:UIKit.UIDatePicker.MinuteInterval) 속성은 선택 기가 분을 표시 하는 간격을 설정 합니다.

```csharp
datePickerView.MinuteInterval = 10;
```

#### <a name="mode"></a>모드

날짜 선택기는 아래에 설명 된 4 가지 [모드](xref:UIKit.UIDatePickerMode)를 지원 합니다.

##### <a name="uidatepickermodetime"></a>UIDatePickerMode. 시간

`UIDatePickerMode.Time` 시간 및 분 선택기와 AM 또는 PM 지정 (선택 사항)이 포함 된 시간을 표시 합니다.

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

![UIDatePickerMode. 시간](picker-images/image8.png "UIDatePickerMode. 시간")

##### <a name="uidatepickermodedate"></a>UIDatePickerMode. 날짜

`UIDatePickerMode.Date` 월, 일 및 연도 선택기를 사용 하 여 날짜를 표시 합니다.

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

![UIDatePickerMode. 날짜](picker-images/image7.png "UIDatePickerMode. 날짜")

선택기의 순서는 기본적으로 시스템 로캘을 사용 하는 날짜 선택의 로캘에 따라 달라 집니다. 위의 이미지는 `en_US` 로캘의 선택기 레이아웃을 보여 주지만, 다음은 순서를 Day로 변경 합니다. | 월 | 연도가

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![일 | 월 | 연도가](picker-images/image9.png "일 | 월 | 연도가")

##### <a name="uidatepickermodedateandtime"></a>UIDatePickerMode. DateAndTime

`UIDatePickerMode.DateAndTime`는 날짜, 시간 및 분의 축약 된 보기와 오전 또는 PM 지정 (12 또는 24 시간제 사용 여부에 따라 다름)을 표시 합니다.

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

![UIDatePickerMode. DateAndTime](picker-images/image6.png "UIDatePickerMode. DateAndTime")

[`UIDatePickerMode.Date`](#uidatepickermodedate)와 마찬가지로 선택기의 순서와 12 또는 24 시간 시계를 사용 하는 것은 날짜 선택의 로캘에 따라 다릅니다.

> [!TIP]
> 모드 `UIDatePickerMode.Time`, `UIDatePickerMode.Date` 또는 `UIDatePickerMode.DateAndTime`에서 날짜 선택기의 값을 캡처하려면 `Date` 속성을 사용 합니다. 이 값은 `NSDate` 저장 됩니다.

##### <a name="uidatepickermodecountdowntimer"></a>UIDatePickerMode. CountDownTimer

`UIDatePickerMode.CountDownTimer` 시간 및 분 값을 표시 합니다.

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

!["UIDatePickerMode. CountDownTimer"](picker-images/image5.png "UIDatePickerMode. CountDownTimer")

`CountDownDuration` 속성은 `UIDatePickerMode.CountDownTimer` 모드에서 날짜 선택기의 값을 캡처합니다. 예를 들어 현재 날짜에 카운트다운 값을 추가 하려면 다음을 수행 합니다.

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="nsdateformatter"></a>NSDateFormatter

`NSDate`형식을 지정 하려면 [`NSDateFormatter`](xref:Foundation.NSDateFormatter)를 사용 합니다.

`NSDateFormatter`를 사용 하려면 해당 [`ToString`](xref:Foundation.NSDateFormatter.ToString(Foundation.NSDate)) 메서드를 호출 합니다. 예를 들면,

```csharp
var date = NSDate.Now;
var formatter = new NSDateFormatter();
formatter.DateStyle = NSDateFormatterStyle.Full;
formatter.TimeStyle = NSDateFormatterStyle.Full;
var formattedDate = formatter.ToString(d);
// Tuesday, August 14, 2018 at 11:20:42 PM Mountain Daylight Time
```

##### <a name="dateformat"></a>DateFormat

`NSDateFormatter`의 [`DateFormat`](xref:Foundation.NSDateFormatter.DateFormat) 속성 (문자열)은 사용자 지정 가능한 날짜 형식 지정을 허용 합니다.

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

##### <a name="timestyle"></a>TimeStyle

[`TimeStyle`](xref:Foundation.NSDateFormatter.TimeStyle) 속성 (`NSDateFormatter`의 [`NSDateFormatterStyle`](xref:Foundation.NSDateFormatterStyle) 는 미리 결정 된 스타일을 기준으로 시간 형식을 지정 합니다.

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

다양 한 `NSDateFormatterStyle` 값은 다음과 같이 시간을 표시 합니다.

- `NSDateFormatterStyle.Full`: 오후 7:46:00 동부 일광 절약 시간
- `NSDateFormatterStyle.Long`:7:47:00 PM EDT
- `NSDateFormatterStyle.Medium`: 오후 7:47:00
- `NSDateFormatterSytle.Short`: 오후 7:47

##### <a name="datestyle"></a>DateStyle

`NSDateFormatter`의 [`DateStyle`](xref:Foundation.NSDateFormatter.DateStyle) 속성 (`NSDateFormatterStyle`)은 미리 지정 된 스타일을 기준으로 날짜 형식을 지정 합니다.

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

다양 한 `NSDateFormatterStyle` 값은 다음과 같이 날짜를 표시 합니다.

- `NSDateFormatterStyle.Full`:7:48 년 8 월 2 일 수요일 오후 시 2017
- `NSDateFormatterStyle.Long`:2017 년 8 월 2 일 7:49 PM
- `NSDateFormatterStyle.Medium`:8 월 2 일, 2017 오후 7:49
- `NSDateFormatterStyle.Short`:8/2/17, 7:50 PM

> [!NOTE]
> `DateFormat` 및 `DateStyle` / `TimeStyle` 날짜 및 시간 형식을 지정 하는 다양 한 방법을 제공 합니다. 가장 최근에 설정 된 속성은 날짜 포맷터의 출력을 결정 합니다.

## <a name="related-links"></a>관련 링크

- [PickerControl (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/pickercontrol)
