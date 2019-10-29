---
title: 날짜 선택
description: DatePickerDialog 및 DialogFragment를 사용 하 여 달력 날짜 선택
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/22/2018
ms.openlocfilehash: b54a0795dee0bd7a02b419da497f9a1b3f3ee7ff
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029211"
---
# <a name="android-date-picker"></a>Android 날짜 선택

## <a name="overview"></a>개요

사용자가 Android 응용 프로그램에 데이터를 입력 해야 하는 경우가 있습니다. 이를 지원 하기 위해 Android 프레임 워크는 [`DatePicker`](xref:Android.Widget.DatePicker) 위젯 및 [`DatePickerDialog`](xref:Android.App.DatePickerDialog) 를 제공 합니다. 사용자는 `DatePicker`를 사용 하 여 장치와 응용 프로그램에서 일관 된 인터페이스의 연도, 월 및 일을 선택할 수 있습니다. `DatePickerDialog`는 대화 상자에서 `DatePicker`를 캡슐화 하는 도우미 클래스입니다.

최신 Android 응용 프로그램은 [`DialogFragment`](xref:Android.App.DialogFragment)에 `DatePickerDialog`를 표시 해야 합니다. 이렇게 하면 응용 프로그램에서 DatePicker를 팝업 대화 상자로 표시 하거나 활동에 포함 시킬 수 있습니다. 또한 `DialogFragment`는 대화의 수명 주기와 표시를 관리 하 여 구현 해야 하는 코드의 양을 줄입니다.

이 가이드에서는 `DialogFragment`에 래핑된 `DatePickerDialog`를 사용 하는 방법을 보여 줍니다. 사용자가 활동의 단추를 클릭 하면 샘플 응용 프로그램은 `DatePickerDialog`를 모달 대화 상자로 표시 합니다. 사용자가 날짜를 설정 하면 `TextView` 선택한 날짜로 업데이트 됩니다.

[선택 날짜 단추의 스크린샷 및 날짜 선택 대화 상자 차례로![](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>요구 사항

이 가이드의 샘플 응용 프로그램은 Android 4.1 (API 수준)을 대상으로 합니다.
16) 이상 이지만 Android 3.0 (API 레벨 11 이상)에 적용할 수 있습니다. Android 지원 라이브러리 v4를 프로젝트에 추가 하 고 일부 코드를 변경 하 여 이전 버전의 Android를 지원할 수 있습니다.

## <a name="using-the-datepicker"></a>DatePicker 사용

이 샘플은 `DialogFragment`를 확장 합니다. 서브 클래스는 `DatePickerDialog`을 호스트 하 고 표시 합니다.

![날짜 선택 대화 상자 확대/확대](date-picker-images/image-02.png)

사용자가 날짜를 선택 하 고 **확인** 단추를 클릭 하면 `DatePickerDialog` [`IOnDateSetListener.OnDateSet`](xref:Android.App.DatePickerDialog.IOnDateSetListener.OnDateSet*)메서드가 호출 됩니다.
이 인터페이스는 호스팅 `DialogFragment`에 의해 구현 됩니다. 사용자가 **취소** 단추를 클릭 하면 조각 및 대화 상자가 자동으로 해제 됩니다.

`DialogFragment`에서 선택한 날짜를 호스팅 활동으로 반환할 수 있는 여러 가지 방법이 있습니다.

1. 작업에서이 값을 설정 하는 속성 또는 메서드를 제공할 수 &ndash; **메서드를 호출 하거나 속성을 설정** 합니다.

2. `DialogFragment`에서 `OnDateSet`를 호출할 때 발생 하는 이벤트를 정의할 수 &ndash; **이벤트를 발생 시킵니다** .

3. **`Action`&ndash; 사용** 하 여 `DialogFragment` `Action<DateTime>`를 호출 하 여 활동의 날짜를 표시할 수 있습니다. 활동은 `DialogFragment`를 인스턴스화할 때 `Action<DateTime`을 제공 합니다. 이 샘플에서는 세 번째 기법을 사용 하며 활동에서 `DialogFragment`에 대 한 `Action<DateTime>`를 제공 해야 합니다.

### <a name="extending-dialogfragment"></a>DialogFragment 확장

`DatePickerDialog`를 표시 하는 첫 번째 단계는 `DialogFragment` 서브 클래스 하 고 `IOnDateSetListener` 인터페이스를 구현 하는 것입니다.

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

새 `DatePickerFragment`을 인스턴스화하기 위해 `NewInstance` 메서드가 호출 됩니다. 이 메서드는 사용자가 `DatePickerDialog`의 **확인** 단추를 클릭할 때 호출 되는 `Action<DateTime>`을 사용 합니다.

조각이 표시 될 때 Android는 `OnCreateDialog`메서드를 호출 합니다. 이 메서드는 새 `DatePickerDialog` 개체를 만들고 현재 날짜 및 콜백 개체 (`DatePickerFragment`의 현재 인스턴스)로 초기화 합니다.

> [!NOTE]
> `IOnDateSetListener.OnDateSet`가 호출 될 때 월의 값은 0에서 11 사이 이며 1에서 12 사이 여야 합니다. 월의 일은 선택한 월에 따라 1에서 31 사이입니다.

### <a name="showing-the-datepickerfragment"></a>DatePickerFragment 표시

이제 `DialogFragment` 구현 되었으므로이 섹션에서는 활동에서 조각을 사용 하는 방법을 살펴봅니다. 이 가이드와 함께 제공 되는 샘플 앱에서 활동은 `NewInstance` factory 메서드를 사용 하 여 `DialogFragment`을 인스턴스화한 다음 `DialogFragment.Show`를 호출 합니다. `DialogFragment`를 인스턴스화하는 일부로 활동은 `Action<DateTime>`를 전달 합니다. 그러면 활동에서 호스트 되는 `TextView` 날짜를 표시 합니다.

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

이 샘플에서는 `DatePicker` 위젯을 Android 활동의 일부로 팝업 모달 대화 상자로 표시 하는 방법에 대해 설명 했습니다. 샘플 DialogFragment 구현을 제공 하 고 `IOnDateSetListener` 인터페이스에 대해 설명 했습니다. 이 샘플은 또한 DialogFragment 호스트 활동과 상호 작용 하 여 선택한 날짜를 표시 하는 방법을 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [DialogFragment](xref:Android.App.DialogFragment)
- [DatePicker](xref:Android.Widget.DatePicker)
- [DatePickerDialog](xref:Android.App.DatePickerDialog)
- [DatePickerDialog. IOnDateSetListener](xref:Android.App.DatePickerDialog.IOnDateSetListener)
- [날짜 선택](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/datepicker/select_a_date)
