---
title: 날짜 선택
description: DialogFragment 고 DatePickerDialog를 사용 하 여 달력 날짜를 선택 합니다.
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 8f4e6318d904efb2f77c36732fc6519699f72ac9
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241138"
---
# <a name="date-picker"></a>날짜 선택

## <a name="overview"></a>개요

사용자는 Android 응용 프로그램에 데이터를 입력 해야 합니다 경우 하는 경우가 있습니다. 을 지원 하기 위해이 사용 하 여 Android 프레임 워크를 제공 합니다 [ `DatePicker` ](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/) 위젯 및 [ `DatePickerDialog` ](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/) 합니다. `DatePicker` 장치 및 응용 프로그램 간에 일관 된 인터페이스에서 연도, 월 및 일을 선택할 수 있습니다. 합니다 `DatePickerDialog` 캡슐화 하는 도우미 클래스는 `DatePicker` 대화 상자에서.

최신 Android 응용 프로그램을 표시 해야 합니다 `DatePickerDialog` 에 [ `DialogFragment` ](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)합니다. 이 DatePicker 팝업 대화 상자를 표시 하려면 응용 프로그램을 사용 하면 또는 활동에 포함 합니다. 또한는 `DialogFragment` 수명 주기 및 구현 해야 하는 코드의 양을 줄이는 대화 상자, 표시를 관리 합니다.

이 가이드에서는 사용 하는 방법을 보여 줍니다 합니다 `DatePickerDialog`에 래핑된는 `DialogFragment`합니다. 샘플 응용 프로그램은 표시 된 `DatePickerDialog` 사용자 작업에 단추를 클릭할 때 모달 대화 상자로 합니다. 날짜는 사용자에 의해 설정 된 경우는 `TextView` 선택 된 날짜를 사용 하 여 업데이트 됩니다.

[![날짜 선택 대화 상자에서 날짜 스크린 샷의 선택 단추](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>요구 사항

이 가이드에 대 한 샘플 응용 프로그램의 대상이 Android 4.1 (API 레벨
16) 이상 하지만 Android 3.0 (API 레벨 11 이상)에 적용 됩니다. Android 지원 라이브러리 v4 프로젝트에 일부 코드 변경 내용을 추가 하 여 이전 버전의 Android 지원 하기 위해는 것이 가능 합니다.

## <a name="using-the-datepicker"></a>DatePicker를 사용 하 여

이 샘플을 확장 합니다 `DialogFragment`합니다. 서브 클래스를 호스트 하 고 표시를 `DatePickerDialog`:

![클로즈업의 날짜 선택 대화 상자](date-picker-images/image-02.png)

사용자는 날짜를 선택 하 고 클릭 하는 경우는 **확인** 단추를 `DatePickerDialog` 메서드를 호출 합니다 [ `IOnDateSetListener.OnDateSet` ](https://developer.xamarin.com/api/member/Android.App.DatePickerDialog+IOnDateSetListener.OnDateSet/p/Android.Widget.DatePicker/System.Int32/System.Int32/System.Int32/)합니다.
이 인터페이스를 호스트 하 여 구현 됩니다 `DialogFragment`합니다. 사용자가 클릭할 경우 합니다 **취소** 단추를 자체는 조각 및 대화 상자 해제 됩니다.

여러 가지 방법으로 `DialogFragment` 호스팅 작업에 선택한 날짜를 반환할 수 있습니다.

1. **메서드를 호출 하거나 속성을 설정** &ndash; The 활동 속성 또는이 값이 설정에 맞게 메서드를 제공할 수 있습니다.

2. **이벤트를 발생 시키는** &ndash; 는 `DialogFragment` 될 이벤트를 정의할 수 있을 때 발생 `OnDateSet` 가 호출 됩니다.

3. **사용 하 여는 `Action`**  &ndash; 는 `DialogFragment` 호출할 수는 `Action<DateTime>` 작업의 날짜를 표시 하 합니다. 활동 제공 합니다 `Action<DateTime` 인스턴스화할 때를 `DialogFragment`합니다. 이 샘플은 세 번째 기법을 사용 하 고를 활동 제공 하도록 요구할를 `Action<DateTime>` 에 `DialogFragment`합니다.



### <a name="extending-dialogfragment"></a>DialogFragment 확장

표시는 첫 번째 단계는 `DatePickerDialog` 서브클래싱하 `DialogFragment` 를 구현 하 고는 `IOnDateSetListener` 인터페이스:

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

합니다 `NewInstance` 에 새 메서드가 호출 되어 `DatePickerFragment`입니다. 이 메서드는 `Action<DateTime>` 사용자가 클릭할 때 호출 되는 **확인** 단추를 `DatePickerDialog`입니다.

Android에서는 메서드를 호출 된 조각이 표시 될 경우 `OnCreateDialog`합니다. 이 메서드는 새로 만듭니다 `DatePickerDialog` 개체와 현재 날짜 및 콜백 개체 초기화 (의 현재 인스턴스는는 `DatePickerFragment`).


> [!NOTE]
> 주의 월의 값 때 `IOnDateSetListener.OnDateSet` 가 호출 됩니다 0 ~ 11 및 하지 1 ~ 12의 범위 내에 있습니다. 월의 날짜는 1에서 31 (따라 매월 선택 된)의 범위에 있게 됩니다.



### <a name="showing-the-datepickerfragment"></a>DatePickerFragment 표시

이제는 `DialogFragment` 되었습니다 구현이 섹션에서는 검사 활동에서 조각을 사용 하는 방법입니다. 이 가이드와 함께 제공 되는 샘플 앱에서 활동 인스턴스화를 `DialogFragment` 를 사용 하 여 합니다 `NewInstance` 팩터리 메서드 및 호출 하는 다음 표시 `DialogFragment.Show`합니다. 인스턴스화의 일부로 합니다 `DialogFragment`, 활동 전달를 `Action<DateTime>`, 날짜가 표시 됩니다는 `TextView` 활동에 의해 호스팅되는:

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

이 샘플에는 표시 하는 방법을 설명는 `DatePicker` Android 활동의 일부로 팝업 모달 대화 상자로 위젯입니다. 샘플 DialogFragment 구현을 제공 하 고 설명 된 `IOnDateSetListener` 인터페이스입니다. 이 샘플에도 DialogFragment 선택한 날짜를 표시 하는 작업 호스트 상호 작용할 수는 방법을 보여 줍니다.


## <a name="related-links"></a>관련 링크

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [DatePicker](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)
- [DatePickerDialog](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)
- [DatePickerDialog.IOnDateSetListener](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog+IOnDateSetListener/)
- [날짜 선택](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/datepicker/select_a_date)
