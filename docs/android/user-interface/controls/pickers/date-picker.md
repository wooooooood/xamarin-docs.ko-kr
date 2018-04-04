---
title: 날짜 선택
description: DatePickerDialog 및 DialogFragment를 사용 하 여 달력 날짜를 선택 합니다.
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 916a9c74fa28b99e799eef80db822e86cfda617d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="date-picker"></a>날짜 선택

## <a name="overview"></a>개요

사용자를 Android 응용 프로그램으로 데이터를 입력 해야 경우 하는 경우가 있습니다. Android 프레임 워크를 제공을 지원 하기 위해이 [ `DatePicker` ](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/) 위젯 및 [ `DatePickerDialog` ](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/) 합니다. `DatePicker` 장치 및 응용 프로그램에 일관 된 인터페이스에서 연도, 월 및 일을 선택할 수 있습니다. `DatePickerDialog` 캡슐화 하는 도우미 클래스는 `DatePicker` 대화 상자에서.

최신 Android 응용 프로그램 표시는 `DatePickerDialog` 에 [ `DialogFragment` ](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)합니다. DatePicker 팝업 대화 상자를 표시 하려면 응용 프로그램을 사용 하면 활동에 포함 되거나입니다. 또한는 `DialogFragment` 관리 수명 주기 및 구현 해야 하는 코드의 양을 줄이거나 대화 상자를 표시 합니다.

이 가이드에서는 사용 하는 방법을 보여 줍니다는 `DatePickerDialog`에 래핑되어는 `DialogFragment`합니다. 샘플 응용 프로그램에 표시 됩니다는 `DatePickerDialog` 사용자가을 활동에 단추를 클릭 하면 모달 대화 상자로 합니다. 날짜는 사용자가 설정 된 경우는 `TextView` 선택 된 기간으로 업데이트 됩니다.

[![날짜 선택 대화 상자에서 다음 선택 날짜 스크린 샷 단추](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>요구 사항

이 가이드에 대 한 샘플 응용 프로그램을 대상으로 Android 4.1 (API 수준
16) 또는 그 이상으로 하지만 Android 3.0 (API 수준 11 또는 이상)에 적용 됩니다. Android 지원 라이브러리 v4 일부 코드를 변경 하 고 프로젝트에 추가 하 여 이전 버전의 Android 지원 두는 것이 불가능 합니다.

## <a name="using-the-datepicker"></a>DatePicker를 사용 하 여

이 예제를 확장 하면 `DialogFragment`합니다. 하위 호스트 되 고 표시 된 `DatePickerDialog`:

![날짜 선택 클로즈업 대화 상자](date-picker-images/image-02.png)

사용자가 날짜를 선택 하 고 클릭 하는 경우는 **확인** 단추는 `DatePickerDialog` 메서드를 호출 [ `IOnDateSetListener.OnDateSet` ](https://developer.xamarin.com/api/member/Android.App.DatePickerDialog+IOnDateSetListener.OnDateSet/p/Android.Widget.DatePicker/System.Int32/System.Int32/System.Int32/)합니다.
이 인터페이스는 호스팅하는 의해 구현 `DialogFragment`합니다. 사용자가 클릭할 경우의 **취소** 단추를 조각과 대화 자체 해제 됩니다.

여러 가지는 `DialogFragment` 호스팅 활동에 선택된 된 날짜를 반환할 수 있습니다.

1. **메서드를 호출 하거나 속성을 설정할** &ndash; The 활동 속성이 나이 값이 설정에 맞게 메서드를 제공할 수 있습니다.

2. **이벤트를 발생 시키는** &ndash; 는 `DialogFragment` 수 있는 이벤트를 정의할 수 있을 때 발생 `OnDateSet` 가 호출 됩니다.

3. **사용 하 여 프로그램 `Action`**  &ndash; 는 `DialogFragment` 호출할 수는 `Action<DateTime>` 활동에서 날짜를 표시 하 합니다. 활동은 제공는 `Action<DateTime` 인스턴스화할 때는 `DialogFragment`합니다. 이 샘플은 세 번째 방법을 사용 하 여 하 고 필요한 작업을 제공 합니다는 `Action<DateTime>` 에 `DialogFragment`합니다.



### <a name="extending-dialogfragment"></a>DialogFragment 확장

표시 하는 첫 번째 단계는 `DatePickerDialog` 서브클래싱하 `DialogFragment` 있고 구현할 수는 `IOnDateSetListener` 인터페이스:

```csharp
public class DatePickerFragment : DialogFragment, 
                                  DatePickerDialog.IOnDateSetListener
{
    // TAG can be any string of your choice.
    public static readonly string TAG = "X:" + typeof (DatePickerFragment).Name.ToUpper();
    
    // Initialize this value to prevent NullReferenceExceptions.
    Action<DateTime> _dateSelectedHandler = delegate { };
    
    public static DatePickerFragment NewInstance(Action<DateTime> onDateSelected)
    {
        DatePickerFragment frag = new DatePickerFragment();
        frag._dateSelectedHandler = onDateSelected;
        return frag;
    }
    
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        DateTime currently = DateTime.Now;
        DatePickerDialog dialog = new DatePickerDialog(Activity, 
                                                       this, 
                                                       currently.Year, 
                                                       currently.Month - 1,
                                                       currently.Day);
        return dialog;
    }
    
    public void OnDateSet(DatePicker view, int year, int monthOfYear, int dayOfMonth)
    {
        // Note: monthOfYear is a value between 0 and 11, not 1 and 12!
        DateTime selectedDate = new DateTime(year, monthOfYear + 1, dayOfMonth);
        Log.Debug(TAG, selectedDate.ToLongDateString());
        _dateSelectedHandler(selectedDate);
    }
}
```

`NewInstance` 메서드를 호출 하는 새 인스턴스화할 `DatePickerFragment`합니다. 이 메서드는 `Action<DateTime>` 사용자가 클릭할 때 호출 되는 **확인** 단추는 `DatePickerDialog`합니다.

Android에서는 메서드를 호출 조각 표시 될 때 `OnCreateDialog`합니다. 이 메서드는 새 만듭니다 `DatePickerDialog` 개체와 현재 날짜 및 콜백 개체 초기화 (변수인의 현재 인스턴스는 `DatePickerFragment`).


> [!NOTE]
> 점에 유의 하는 월의 값 때 `IOnDateSetListener.OnDateSet` 가 호출 11, 0 및 하지 1 ~ 12의 범위 내에 있습니다. 월의 날짜 범위는 1에서 31 (월이 선택한 따라) 됩니다.



### <a name="showing-the-datepickerfragment"></a>DatePickerFragment 표시

이제는 `DialogFragment` 되었습니다 구현이 섹션 검토 활동에 조각의 사용 하는 방법입니다. 이 가이드를 함께 제공 되는 샘플 응용 프로그램에서 활동 인스턴스화는 `DialogFragment` 를 사용 하는 `NewInstance` 팩터리 메서드 및 해당 호출 후 표시 `DialogFragment.Show`합니다. 인스턴스화의 일부로 `DialogFragment`, 활동 전달는 `Action<DateTime>`, 날짜를 표시 하는 한 `TextView` 활동에 의해 호스팅되는:

```csharp
[Activity(Label = "@string/app_name", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
    TextView _dateDisplay;
    Button _dateSelectButton;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);

        _dateDisplay = FindViewById<TextView>(Resource.Id.date_display);
        _dateSelectButton = FindViewById<Button>(Resource.Id.date_select_button);
        _dateSelectButton.Click += DateSelect_OnClick;
    }

    void DateSelect_OnClick(object sender, EventArgs eventArgs)
    {
        DatePickerFragment frag = DatePickerFragment.NewInstance(delegate(DateTime time)
                                                                 {
                                                                     _dateDisplay.Text = time.ToLongDateString();
                                                                 });
        frag.Show(FragmentManager, DatePickerFragment.TAG);
    }
}
```


## <a name="summary"></a>요약

이 샘플에 표시 하는 방법을 설명는 `DatePicker` Android 활동의 일부로 팝업 모달 대화 상자로 위젯입니다. 샘플 DialogFragment 구현을 제공 하 고 논의 `IOnDateSetListener` 인터페이스입니다. 이 샘플에도 DialogFragment 선택한 날짜를 표시 하는 활동 호스트와 상호 작용할 수 방법을 보여 줍니다.


## <a name="related-links"></a>관련 링크

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [DatePicker](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)
- [DatePickerDialog](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)
- [DatePickerDialog.IOnDateSetListener](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog+IOnDateSetListener/)
- [날짜를 선택 합니다.](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/android/controls/datepicker/select_a_date)
