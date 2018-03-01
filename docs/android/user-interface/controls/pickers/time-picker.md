---
title: "시간 선택"
description: "TimePickerDialog 및 DialogFragment 사용 하 여 시간을 선택"
ms.topic: article
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 4b3299ad138b5cd74ce77cac1da49d21a833fe1a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="time-picker"></a>시간 선택

를 제공 하기 위해 사용자가 한 번에 선택할 수 있는 방법을 사용할 수 있습니다 [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)합니다. Android 앱은 일반적으로 사용 `TimePicker` 와 [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/) 시간 값을 선택 하기 위한 &ndash; 이렇게 하면 장치 및 응용 프로그램 전체에서 일관 된 인터페이스를 유지 하도록 합니다. `TimePicker` 사용자가을 12 시간제 또는 24 시간제 AM/PM 모드로 하루 중 시간을 선택할 수 있습니다.
`TimePickerDialog` 캡슐화 하는 도우미 클래스는 `TimePicker` 대화 상자에서.

[![예제 실행에서 시간 선택 대화 상자 스크린샷](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png)

## <a name="overview"></a>개요

최신 Android 응용 프로그램 표시는 `TimePickerDialog` 에 [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)합니다. 이렇게 하면 응용 프로그램을 표시 하는 `TimePicker` 팝업 대화 상자로 하거나 활동에 포함 합니다. 또한는 `DialogFragment` 관리 수명 주기 및 구현 해야 하는 코드의 양을 줄이거나 대화 상자를 표시 합니다.

사용 하는 방법은이 가이드는 `TimePickerDialog`에 래핑되어는 `DialogFragment`합니다. 샘플 응용 프로그램 표시는 `TimePickerDialog` 사용자가을 활동에 단추를 클릭 하면 모달 대화 상자로 합니다. 시간이 사용자가 설정 되 면 대화 상자를 종료 하 고 처리기를 업데이트 한 `TextView` 활동 화면의 선택 된 시간을 사용 합니다.

## <a name="requirements"></a>요구 사항

이 가이드에 대 한 샘플 응용 프로그램을 대상으로 Android 4.1 (API 수준
16) 또는 그 이상으로 하지만 Android 3.0 (API 수준 11 또는 이상)과 함께 사용할 수 있습니다. Android 지원 라이브러리 v4 일부 코드를 변경 하 고 프로젝트에 추가 하 여 이전 버전의 Android 지원 두는 것이 불가능 합니다.

## <a name="using-the-timepicker"></a>TimePicker를 사용 하 여

이 예에서는 확장 `DialogFragment`; 하위 클래스 구현의 `DialogFragment` (호출 `TimePickerFragment` 아래)를 호스트 하 고 표시는 `TimePickerDialog`합니다. 샘플 앱이 처음 시작 하는 경우 표시는 **선택 시간** 위의 단추는 `TextView` 선택 된 시간을 표시 하는 데 사용 될 됩니다:

[![초기 샘플 응용 프로그램 화면](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png)

클릭는 **선택 시간** 단추를 예제 앱을 시작할은 `TimePickerDialog` 이 스크린 샷에 표시 된 대로:

[![응용 프로그램에서 표시 되는 기본 시간 선택 대화 상자 스크린샷](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png)

에 `TimePickerDialog`, 시간을 선택 하 고 클릭 하는 **확인** 원인을 단추는 `TimePickerDialog` 메서드를 호출 하기 [IOnTimeSetListener.OnTimeSet](https://developer.xamarin.com/api/member/Android.App.TimePickerDialog+IOnTimeSetListener.OnTimeSet/p/Android.Widget.TimePicker/System.Int32/System.Int32/System.Int32/)합니다.
이 인터페이스는 호스팅하는 의해 구현 `DialogFragment` (`TimePickerFragment`아래 설명 된 대로). 클릭 하 고 **취소** 단추를 클릭 하면 조각 및 대화 상자를 닫을 수 있습니다.

`DialogFragment` 세 가지 방법 중 하나로 호스팅 Actvity를 선택한 시간을 반환합니다.

1. **메서드 호출 또는 속성을 설정할** &ndash; The 활동 속성이 나이 값이 설정에 맞게 메서드를 제공할 수 있습니다.

2. **이벤트를 발생 시키는** &ndash; 는 `DialogFragment` 수 있는 이벤트를 정의할 수 있을 때 발생 `OnTimeSet` 가 호출 됩니다.

3. **사용 하는 `Action`**  &ndash; 는 `DialogFragment` 호출할 수는 `Action<DateTime>` 활동에서 시간을 표시 하도록 합니다. 활동은 제공는 `Action<DateTime` 인스턴스화할 때는 `DialogFragment`합니다. 

이 예제에서는 사용 해야 하는 세 번째 방법 활동 공급은 `Action<DateTime>` 처리기에는 `DialogFragment`합니다.


<a name="start" />

## <a name="start-an-app-project"></a>응용 프로그램 프로젝트를 시작 합니다.

라는 새 Android 프로젝트 시작 **TimePickerDemo** (Xamarin.Android 프로젝트 만들기에 익숙하지 않은 경우 참조 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) 새 프로젝트를 만드는 방법에 알아보려면).

편집 **Resources/layout/Main.axml** 해당 내용을 다음 XML로 바꿉니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_gravity="center_horizontal"
    android:padding="16dp">
    <Button
        android:id="@+id/select_button"
        android:paddingLeft="24dp"
        android:paddingRight="24dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="PICK TIME"
        android:textSize="20dp" />
    <TextView
        android:id="@+id/time_display"
        android:layout_height="wrap_content"
        android:layout_width="match_parent"
        android:paddingTop="22dp"
        android:text="Picked time will be displayed here"
        android:textSize="24dp" />
</LinearLayout>
```

이것은 기본적인 [LinearLayout](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 와 [TextView](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 시간을 표시 하는 및 [단추](https://developer.xamarin.com/api/type/Android.Widget.Button/) 열립니다는 `TimePickerDialog`합니다. 이 레이아웃 및를 사용 하도록 하드 코드 된 문자열 차원 응용 프로그램 보다 간단 하 고 보다 쉽게 이해할 수 있도록 참고 &ndash; 이러한 값에 대 한 프로덕션 응용 프로그램 정상적으로 사용 하 여 리소스 (에서 볼 수 있듯이 [DatePicker](https://github.com/xamarin/recipes/blob/master/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) 코드 예제에서는).

편집 **MainActivity.cs** 해당 내용을 다음 코드로 바꿉니다.

```csharp
using Android.App;
using Android.Widget;
using Android.OS;
using System;
using Android.Util;
using Android.Text.Format;

namespace TimePickerDemo
{
    [Activity(Label = "TimePickerDemo", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        TextView timeDisplay;
        Button timeSelectButton;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            SetContentView(Resource.Layout.Main);
            timeDisplay = FindViewById<TextView>(Resource.Id.time_display);
            timeSelectButton = FindViewById<Button>(Resource.Id.select_button);
        }
    }
}
```

를 빌드 및 실행이 예에서는 다음 스크린 샷과 초기 화면을 표시 되어야 합니다.

[![초기 응용 프로그램 화면](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png)

클릭 하는 **선택 시간** 단추는 아무 작업도 수행 하기 때문에 `DialogFragment` 표시 하는 아직 구현 되지 않은 `TimePicker`합니다.
다음 단계는이 만드는 것 `DialogFragment`합니다.


<a name="extend_dialogfragment" />

## <a name="extending-dialogfragment"></a>DialogFragment 확장

확장 하려면 `DialogFragment` 와 함께 사용할 `TimePicker`에서 파생 된 하위 클래스를 만들 필요가 `DialogFragment` 구현 `TimePickerDialog.IOnTimeSetListener`합니다. 다음 클래스에 추가 **MainActivity.cs**:

```csharp
public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
{
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };

    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }

    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }

    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
        Log.Debug(TAG, selectedTime.ToLongTimeString());
        timeSelectedHandler (selectedTime);
    }
}
```

이 `TimePickerFragment` 클래스는 더 작은 부분으로 분할 하며 다음 섹션에서 설명 합니다.

<a name="details" />

### <a name="dialogfragment-implementation"></a>DialogFragment 구현

`TimePickerFragment` 여러 가지 방법을 구현: 팩터리 메서드, 대화 인스턴스화 방법, 및 `OnTimeSet` 에서 요구 하는 처리기 방법 `TimePickerDialog.IOnTimeSetListener`합니다.

-   `TimePickerFragment` 하위 클래스가 `DialogFragment`합니다. 또한 구현 하는 `TimePickerDialog.IOnTimeSetListener` 인터페이스 (즉, 필요한 제공 `OnTimeSet` 메서드):

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG` 로깅 목적으로 자동으로 초기화 됩니다 (*MyTimePickerFragment* 변경할 수는 문자열을 사용 하려는). `timeSelectedHandler` 작업이 null 참조 예외를 방지 하는 빈 대리자를 초기화 합니다.

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   `NewInstance` 팩터리 메서드를 호출 하는 새 인스턴스화할 `TimePickerFragment`합니다. 이 메서드는 `Action<DateTime>` 를 클릭할 때 호출 되는 처리기는 **확인** 단추는 `TimePickerDialog`:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   Android를 호출 하는 부분은 표시 될 때는 `DialogFragment` 메서드 [OnCreateDialog](https://developer.xamarin.com/api/member/Android.App.DialogFragment.OnCreateDialog/p/Android.OS.Bundle/)합니다. 
    이 메서드가 만드는 새 `TimePickerDialog` 개체 및 작업과 콜백 개체를 초기화 합니다 (변수인의 현재 인스턴스는 `TimePickerFragment`), 및 현재 시간:

    ```csharp
    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }
    ```

-   사용자에서 시간 설정을 변경 하는 경우는 `TimePicker` 대화 상자는 `OnTimeSet` 메서드가 호출 됩니다. `OnTimeSet` 만듭니다는 `DateTime` 개체의 현재 날짜 및 사용자가 선택한 시간 (시간과 분)의 병합을 사용 하 여:

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   이 `DateTime` 를 전달 하는 `timeSelectedHandler` 등록 된는 `TimePickerFragment` 를 만들 때에는 개체입니다. `OnTimeSet` 이 처리기 활동의 시간 표시 (이 처리기는 다음 섹션에서 구현) 선택한 시간에 업데이트를 호출 합니다.

    ```csharp
    timeSelectedHandler (selectedTime);
    ```


<a name="time_picker_fragment" />

## <a name="displaying-the-timepickerfragment"></a>TimePickerFragment 표시

이제는 `DialogFragment` 되었습니다, 구현 하는 인스턴스화하는 `DialogFragment` 를 사용 하 여는 `NewInstance` 팩터리 메서드를 호출 하 여 표시 [DialogFragment.Show](https://developer.xamarin.com/api/member/Android.App.DialogFragment.Show/p/Android.App.FragmentManager/System.String/):

다음 메서드를 추가 `MainActivity`:

```csharp
void TimeSelectOnClick (object sender, EventArgs eventArgs)
{
    TimePickerFragment frag = TimePickerFragment.NewInstance (
        delegate (DateTime time)
        {
            timeDisplay.Text = time.ToShortTimeString();
        });

    frag.Show(FragmentManager, TimePickerFragment.TAG);
}
```

후 `TimeSelectOnClick` 인스턴스화하는 `TimePickerFragment`를 만들고 시간에 전달 된 값으로 작업의 시간 표시를 업데이트 하는 무명 메서드의 대리자에 전달 합니다. 마지막으로 시작 됩니다는 `TimePicker` 대화 조각 (통해 `DialogFragment.Show`) 표시 하는 `TimePicker` 사용자에 게 합니다.

끝에는 `OnCreate` 메서드를 이벤트 처리기를 연결 하려면 다음 줄 추가 **선택 시간** 대화 상자를 시작 하는 단추:

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

경우는 **선택 시간** 단추를 클릭 하면 `TimeSelectOnClick` 표시 하는 데 호출 됩니다는 `TimePicker` 사용자에 게 대화 조각입니다.


<a name="try-it" />

## <a name="try-it"></a>실습

앱을 빌드하고 실행합니다. 클릭할 때는 **선택 시간** 단추는 `TimePickerDialog` (이 경우, 12 시간 AM/PM 모드)에서 작업에 대 한 기본 시간 형식으로 표시 됩니다.

[![AM/PM 모드로 시간 대화 상자가 표시 됩니다.](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png)
   
클릭할 때 **확인** 에 `TimePicker` 대화 상자에서 처리기 활동의 업데이트 `TextView` 선택한 시간 및 다음 종료 됩니다.

[![A/M 시간은 활동 TextView에 표시 됩니다.](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png)

다음으로 코드의 다음 줄을 추가 `OnCreateDialog` 직후 `is24HourFormat` 선언 되 고 초기화 합니다.

```csharp
is24HourFormat = true;
```

이 변경 내용을 강제로에 전달 된 플래그는 `TimePickerDialog` 생성자가 `true` 24 시간 모드를 사용 하는 호스팅 작업의 시간 형식 대신 사용 됩니다. 빌드하고 앱을 다시 실행 하는 경우 클릭는 **선택 시간** 단추를는 `TimePicker` 대화 24 시간 형식에 표시 됩니다.

[![24 시간 형식에서 TimePicker 대화 상자](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png)

처리기를 호출 하기 때문에 [DateTime.ToShortTimeString](https://msdn.microsoft.com/en-us/library/system.datetime.toshortdatestring%28v=vs.110%29.aspx) 는 활동의 시간을 인쇄 하려면 `TextView`, 시간이 계속 기본 12 시간제 AM/PM 형식으로 인쇄 됩니다.


<a name="summary" />

## <a name="summary"></a>요약

이 문서에서는 표시 하는 방법을 `TimePicker` Android 활동에서 팝업 모달 대화 상자로 위젯입니다. 샘플을 제공 `DialogFragment` 구현을 설명 하 고는 `IOnTimeSetListener` 인터페이스입니다. 이 샘플도 보여 줍니다. 방법을 `DialogFragment` 선택 된 시간을 표시 하는 활동 호스트와 상호 작용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)
- [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)
- [TimePickerDialog.IOnTimeSetListener](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog+IOnTimeSetListener/)
- [TimePickerDemo (샘플)](https://developer.xamarin.com/samples/monodroid/UserInterface/TimePickerDemo)
