---
title: 선택
description: 이 가이드에서는 디자인 하 고 선택 Xamarin.iOS 앱에서 작업을 설명 합니다.
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/02/2017
ms.openlocfilehash: e213124e870f1cca96a6078fd26bc7eeb1af55a1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="picker"></a>선택

선택 컨트롤 강조 표시 되 고 선택한 값을 사용 하 여 값의 스크롤 가능한 목록을 포함 하는 ' 휠 형식 ' 컨트롤을 표시 합니다. 사용자가 휠을 원하는 옵션을 선택 합니다.

하나의 특정 사용자 대/소문자 선택에 대 한 날짜 및/또는 시간을 설정할 수 있습니다. 이 Apple에 제공 하 라는 UIDatePicker UIPickerView 클래스의 사용자 지정 하위를 생성 했습니다.

문서에서는 구현 및 사용 하는 [선택기](#picker) 및 [날짜 선택](#datepicker) 컨트롤입니다.

<a name="picker" />

## <a name="picker"></a>선택

### <a name="implementing-a-picker"></a>선택기를 구현합니다.

새 인스턴스화하여 구현 되는 선택 [`UIPickerView`](https://developer.xamarin.com/api/type/UIKit.UIPickerView/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="pickers-and-storyboards"></a>선택 및 스토리 보드

IOS 디자이너를 만드는 UI를 사용 하는 경우 도구 상자에서 레이아웃에는 선택에 추가할 수 있습니다.

![](picker-images/image1.png)


### <a name="working-with-picker"></a>작업 선택

스토리 보드를 통해 할당 해야 합니다 또는 코드에는 코드 조각 선택기를 만든 후는 _모델_ 를 전달 하 고 데이터와 상호 작용할 수 있도록

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    var pickerModel = new PeopleModel(personLabel);

    personPicker.Model = pickerModel;
}
```

다음 코드에서는 모델의 예를 보여 줍니다.

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

먼저 사용자가 선택할 수에 대 한 다른 옵션을 제공할 일부 데이터를 전달 해야 합니다. 가능한이이 목록, 가능한 한 짧게 유지 하려고 하거나 필요한 하나 이상의 전화 걸기를 사용 하려고 하는 경우 (호출 *구성 요소*):

![두 구성 요소와 선택](picker-images/image3.png)

구성 요소 수를 설정 하려면 재정의 `GetComponentCount` 메서드: 

```csharp
public override nint GetComponentCount(UIPickerView pickerView)
{
    return 2;
}
```

반환 값 프로그램 선택 갖습니다 다이얼 수를 의미 합니다.

### <a name="customizing-appearance"></a>모양 사용자 지정
 
모양을 `UIPickerView` 를 사용 하 여 사용자 지정할 수 있습니다는 `UIPickerView.UIPickerViewAppearance` 나 클래스에서 재정의 된 `UIPickerViewModel.GetView` 및 `UIPickerViewModel.GetRowHeight` 의 메서드는 `UIPickerViewModel`합니다.


<a name="datepicker" />

## <a name="date-picker"></a>날짜 선택

### <a name="implementing-a-date-picker"></a>날짜 선택 구현

날짜 선택 새 인스턴스화 구현한 [ `UIDatePickerView` ](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="date-pickers-and-storyboards"></a>스토리 보드와 날짜 선택

UI를 만들를 iOS 디자이너를 사용 하는 경우는 **날짜 선택** 도구 상자에서 레이아웃에 추가할 수 있습니다. 속성 패드에서 다음 속성을 조정할 수 있습니다.

![](picker-images/image2.png)

* **모드** – The 날짜 및 시간 모드입니다. 이 날짜, 시간, 날짜 및 시간 또는 카운트다운 타이머를 수 있습니다. 
* **로캘** – 날짜 선택의 로캘입니다. 선택 **기본** 시스템 기본으로 설정 하거나 모든 특정 로캘로 설정 하도록 합니다.
* **간격** – 클록 옵션 표시 되는 증분 표시 합니다.
* **날짜, 최소 날짜, 최대 날짜** – 선택 가능한 날짜에 대 한 고 선택기 ½ ֳ µ 초기 날짜와 제약 조건으로 설정 합니다.

### <a name="configuring-the-datepicker"></a>DatePicker 구성

사용자를 사용 하 여 선택할 수 있는 날짜 범위를 제한할 수 있습니다는 `MinimumDate` 및 `MaximumDate` 속성입니다. 다음 코드 조각에서는 60 년 오늘 사이의 범위를 설정 하는 방법의 예를 보여 줍니다.

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();

components.Year = -60;

NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);

datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

또는 최소 및 최대 날짜 범위를 설정 하려면.NET 컨트롤을 사용할 수도 있습니다. 예를 들어:

```csharp
DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
```

설정할 수도 있습니다는 `MinuteInterval` 되 고 선택기 ½ ֳ µ 분 간격을 설정 하는 속성입니다. 다음 코드 조각 10의 간격 설정할 분 선택기 설정 데 사용할 수 있습니다.

```csharp
datePickerView.MinuteInterval = 10;
```

날짜 선택 사용 하 여 설정할 수 있는 네 가지 모드를 가지는 [ `UIDatePicker.Mode` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.Mode/) 속성입니다. 아래 목록에서는 각 하나를 구현 하는 방법을 보여 줍니다.

#### <a name="time"></a>시간

시간 모드 시간 및 분 선택기와 선택적 AM 또는 PM 지정 시간을 표시 합니다. 으로 설정 된는 `UIDatePickerMode.Time` 속성입니다. 예를 들어:

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

다음 그림에서는이 DatePicker 모드의 예를 보여 줍니다.

![](picker-images/image8.png)



#### <a name="date"></a>날짜

날짜 모드 월, 일 및 연도 선택기를 사용 하 여 날짜를 표시합니다. 으로 설정 된는 `UIDatePickerMode.Date` 속성입니다. 예를 들어:

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

다음 그림에서는이 DatePicker의 예를 보여 줍니다.

![](picker-images/image7.png)

선택기의 순서의 로캘에 따라 결정 된는 `UIDatePicker`합니다. 기본적으로 시스템 기본값을 설정 되어야 합니다. 위의 그림에서 선택기의 레이아웃을 보여 줍니다.는 `en_US` 하려면 하루를 변경 하지만 로캘 | 월 | 연도 레이아웃 로캘을 설정 하려면 다음 코드를 사용할 수 있습니다.

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![](picker-images/image9.png)


#### <a name="date-and-time"></a>날짜 및 시간

날짜 및 시간 모드는 12 또는 24 시간 형식을 사용 하는 경우에 날짜, 시간 및 분 및 선택적 AM 또는 PM 지정 dependings에 시간 shortend 뷰로를 표시 됩니다. 으로 설정 된는 `UIDatePickerMode.DateAndTime` 속성입니다. 예를 들어:

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

다음 그림에서는이 DatePicker의 예를 보여 줍니다.

![](picker-images/image6.png)

와 마찬가지로 [날짜](#Date)의 로캘에 따라 달라 집니다 12 또는 24 시간 형식 사용 하 여 선택기의 순서는는 `UIDatePicker`합니다.

#### <a name="countdown-timer"></a>카운트다운 타이머

카운트다운 타이머 모드는 시간 및 분 값을 표시합니다. 으로 설정 된는 `UIDatePickerMode.CountDownTimer` 속성입니다. 예를 들어:

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

다음 그림에서는이 DatePicker의 예를 보여 줍니다.

![](picker-images/image5.png)

사용할 수는 `CountDownDuration` 카운트다운 날짜 선택 하 여 값 dispayed 캡처하는 속성입니다. 예를 들어 카운트다운 값에는 현재 날짜를 추가 하려면 다음 코드를 사용할 수 있습니다.

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="formatting"></a>서식 

사용 하 여 시간, 날짜 및 프레젠테이션에 모드 값을 캡처할 수 있습니다는 `Date` 는 UIDatePicker 속성 (예: `datePickerView.Date`)가 NSDate 형식의 합니다. 이 날짜에 더 많은 다른 사람이 읽을 수 있는으로 표시할 [ `NSDateFormatter` ](https://developer.xamarin.com/api/type/Foundation.NSDateFormatter/)합니다. 다음 예에는이 클래스에서 사용 가능한 속성 중 일부를 사용 하는 방법을 보여 줍니다.

`DateFormat` 날짜를 표시 하는 방법을 나타내기 위해 문자열로 설정 되어 있습니다.

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

`TimeStyle` 속성 집합을 `NSDateFormatterStyle`:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

에 대 한 필드 `NSDateFormatterStyle` 다음과 같이 표시 합니다.

![](picker-images/timestyle.png)

`DateStyle` 속성 집합을 `NSDateFormatterStyle`:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

에 대 한 필드 `NSDateFormatterStyle` 다음과 같이 표시 합니다.

![](picker-images/datestyle.png)

다음 코드를 사용 하 여 다음 문자열에 서식이 지정 된 NSDate를 출력할 수 있습니다.

```csharp
dateLabel.Text = dateTimeformat.ToString(datePickerView.Date);
```

## <a name="related-links"></a>관련 링크

- [PickerControl (샘플)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
