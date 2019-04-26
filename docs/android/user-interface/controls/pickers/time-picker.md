---
title: 시간 선택기
description: TimePickerDialog 및 DialogFragment를 사용 하 여 시간을 선택
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: faf2c35b49b0b02b9f3b16e19494d2e447361d84
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61283727"
---
# <a name="time-picker"></a>시간 선택기

사용자는 시간을 선택할 수 있는 방법을 제공을 사용할 수 있습니다 [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)합니다. Android 앱은 일반적으로 사용 `TimePicker` 사용 하 여 [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/) 시간 값을 선택 하기 위한 &ndash; 장치 및 응용 프로그램 전체에서 일관 된 인터페이스를 보장할 수 있습니다. `TimePicker` 12 시간제 또는 24 시간 AM/PM 모드로 하루 중 시간을 선택할 수 있습니다.
`TimePickerDialog` 캡슐화 하는 도우미 클래스는 `TimePicker` 대화 상자에서.

[![예제 작업에서 시간 선택 대화 상자 스크린샷](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>개요

최신 Android 응용 프로그램을 표시 합니다 `TimePickerDialog` 에 [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)합니다. 따라서 응용 프로그램을 표시할 수는 `TimePicker` 팝업 대화 하거나 활동에 포함 합니다. 또한는 `DialogFragment` 관리 수명 주기 및 구현 해야 하는 코드의 양을 줄이는 대화 상자를 표시 합니다.

이 가이드를 사용 하는 방법에 설명 합니다 `TimePickerDialog`에 래핑된는 `DialogFragment`합니다. 샘플 응용 프로그램 표시는 `TimePickerDialog` 사용자 작업에 단추를 클릭할 때 모달 대화 상자로 합니다. 에 사용자가 설정 되 면 대화 상자를 종료 하 고 처리기를 업데이트는 `TextView` 선택 된 시간을 사용 하 여 작업 화면입니다.

## <a name="requirements"></a>요구 사항

이 가이드에 대 한 샘플 응용 프로그램의 대상이 Android 4.1 (API 레벨
16) 이상 이지만 Android 3.0 (API 레벨 11 이상)를 사용 하 여 사용할 수 있습니다. Android 지원 라이브러리 v4 프로젝트에 일부 코드 변경 내용을 추가 하 여 이전 버전의 Android 지원 하기 위해는 것이 가능 합니다.

## <a name="using-the-timepicker"></a>TimePicker를 사용 하 여

이 예제에서는 확장 `DialogFragment`; 서브 클래스 구현의 `DialogFragment` (호출 `TimePickerFragment` 아래)를 호스트 하 고 표시를 `TimePickerDialog`입니다. 샘플 앱이 처음 시작 하는 경우 표시 됩니다는 **선택 시간** 위에 있는 단추는 `TextView` 선택한 시간을 표시 하려면 사용할:

[![초기 샘플 앱 화면](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

클릭할 때 합니다 **선택 시간** 예제에서는 앱 시작 단추를 `TimePickerDialog` 이 스크린샷에 표시 된 것 처럼:

[![앱에서 표시 하는 기본 시간 선택 대화 상자 스크린샷](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

에 `TimePickerDialog`시간을 선택 하 고 클릭 하는 **확인** 원인 단추를 `TimePickerDialog` 메서드를 호출 하 [IOnTimeSetListener.OnTimeSet](https://developer.xamarin.com/api/member/Android.App.TimePickerDialog+IOnTimeSetListener.OnTimeSet/p/Android.Widget.TimePicker/System.Int32/System.Int32/System.Int32/)합니다.
이 인터페이스를 호스트 하 여 구현 됩니다 `DialogFragment` (`TimePickerFragment`에 아래 설명 된 대로). 클릭 하 여 **취소** 단추를 클릭 하면 대화 상자를 닫을 수를 확인 하 고 조각.

`DialogFragment` 호스팅 작업 세 가지 방법 중 하나를 선택한 시간을 반환합니다.

1. **메서드 호출 또는 속성을 설정** &ndash; The 활동 속성 또는이 값이 설정에 맞게 메서드를 제공할 수 있습니다.

2. **이벤트를 발생 시키는** &ndash; 는 `DialogFragment` 될 이벤트를 정의할 수 있을 때 발생 `OnTimeSet` 가 호출 됩니다.

3. **사용 하는 `Action`**  &ndash; 는 `DialogFragment` 호출할 수 있습니다는 `Action<DateTime>` 작업의 시간을 표시 하 합니다. 활동 제공 합니다 `Action<DateTime` 인스턴스화할 때를 `DialogFragment`합니다. 

이 예제에서는 사용는 세 번째 기법은 작업 공급을 `Action<DateTime>` 처리기를 `DialogFragment`.



## <a name="start-an-app-project"></a>앱 프로젝트를 시작 합니다.

라는 새 Android 프로젝트를 시작 **TimePickerDemo** (Xamarin.Android 프로젝트 만들기와 잘 모르는 경우 참조 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) 새 프로젝트를 만드는 방법).

편집할 **Resources/layout/Main.axml** 다음 XML을 사용 하 여 해당 내용을 바꿉니다.

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

이 기본 [LinearLayout](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 사용 하 여를 [TextView](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 시간을 표시 하는 [단추](https://developer.xamarin.com/api/type/Android.Widget.Button/) 열리는 `TimePickerDialog`합니다. 참고는이 레이아웃을 사용 하 여 하드 코드 된 문자열 및 차원 앱 간단 하 고 이해 하기 쉽습니다 &ndash; 프로덕션 앱을 일반적으로 이러한 값에 대 한 리소스를 사용 (에서 볼 수 있듯이 합니다 [DatePicker](https://github.com/xamarin/recipes/blob/master/Recipes/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) 코드 예제).

편집할 **MainActivity.cs** 내용을 다음 코드로 바꿉니다.

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

를 빌드 및이 예제를 실행 하 여 다음 스크린샷과 유사한 초기 화면을 표시 됩니다.

[![초기 앱 화면](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

클릭 하는 **선택 시간** 단추는 아무 작업도 수행 하지 때문에 `DialogFragment` 표시할 아직 구현 되지 않았습니다는 `TimePicker`합니다.
이 만들려면 다음 단계는 `DialogFragment`합니다.



## <a name="extending-dialogfragment"></a>DialogFragment 확장

확장할 `DialogFragment` 사용에 대 한 `TimePicker`에서 파생 된 서브 클래스를 만드는 데 필요한 것 `DialogFragment` 구현 하 고 `TimePickerDialog.IOnTimeSetListener`입니다. 다음 클래스를 추가 합니다 **MainActivity.cs**:

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

이 `TimePickerFragment` 클래스는 더 작은 부분으로 분류 하 고 다음 섹션에서 설명 합니다.


### <a name="dialogfragment-implementation"></a>DialogFragment 구현

`TimePickerFragment` 여러 메서드를 구현 합니다: 팩터리 메서드, 대화 인스턴스화 방법, 및 `OnTimeSet` 에 필요한 처리기 메서드 `TimePickerDialog.IOnTimeSetListener`합니다.

-   `TimePickerFragment` 서브 클래스 `DialogFragment`합니다. 또한 구현 합니다 `TimePickerDialog.IOnTimeSetListener` 인터페이스 (즉, 필요한 제공 `OnTimeSet` 메서드):

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG` 로깅 목적으로 자동으로 초기화 됩니다 (*MyTimePickerFragment* 변경할 수 있습니다 사용 하려는 모든 문자열)입니다. `timeSelectedHandler` 작업은 null 참조 예외를 방지 하기 위해 빈 대리자를 초기화 합니다.

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   합니다 `NewInstance` 팩터리 메서드가 호출 되어 새 `TimePickerFragment`합니다. 이 메서드는 `Action<DateTime>` 를 클릭할 때 호출 되는 처리기는 **확인** 단추를 `TimePickerDialog`:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   조각이 표시 될 경우 Android 호출을 `DialogFragment` 메서드 [OnCreateDialog](https://developer.xamarin.com/api/member/Android.App.DialogFragment.OnCreateDialog/p/Android.OS.Bundle/)합니다. 
    이 메서드를 만듭니다 `TimePickerDialog` 개체 및 작업을 콜백 개체를 사용 하 여 초기화 (의 현재 인스턴스는는 `TimePickerFragment`), 및 현재 시간:

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

-   사용자에서 시간 설정을 변경 하는 경우는 `TimePicker` 대화 상자에서를 `OnTimeSet` 메서드가 실행 됩니다. `OnTimeSet` 만듭니다는 `DateTime` 현재 날짜 및 시간 (시간 및 분)의 병합을 사용자가 선택한를 사용 하 여 개체:

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   이 `DateTime` 개체를 전달 하는 `timeSelectedHandler` 등록 된는 `TimePickerFragment` 개체를 만들 때. `OnTimeSet` (이 처리기는 다음 섹션에서 구현 됩니다.)는 선택한 시간에 작업의 시간 표시를 업데이트 하려면이 처리기를 호출 합니다.

    ```csharp
    timeSelectedHandler (selectedTime);
    ```



## <a name="displaying-the-timepickerfragment"></a>TimePickerFragment 표시

이제는 `DialogFragment` 되었습니다 시간을 인스턴스화하는 구현 합니다 `DialogFragment` 를 사용 하 여는 `NewInstance` 팩터리 메서드를 호출 하 여 표시 [DialogFragment.Show](https://developer.xamarin.com/api/member/Android.App.DialogFragment.Show/p/Android.App.FragmentManager/System.String/):

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

후 `TimeSelectOnClick` 인스턴스화하는 `TimePickerFragment`를 만들고 전달 된 시간 값을 사용 하 여 작업의 시간 표시를 업데이트 하는 무명 메서드의 대리자에 전달 합니다. 마지막으로 시작 합니다 `TimePicker` 대화 조각 (통해 `DialogFragment.Show`) 표시할는 `TimePicker` 사용자에 게 합니다.

끝에 `OnCreate` 메서드를 이벤트 처리기를 연결 하려면 다음 줄을 추가 합니다 **선택 시간** 대화 상자를 시작 하는 단추:

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

경우는 **선택 시간** 단추를 클릭 `TimeSelectOnClick` 표시할 호출할를 `TimePicker` 사용자에 게 대화 조각입니다.



## <a name="try-it"></a>실습

앱을 빌드하고 실행합니다. 클릭할 때 합니다 **선택 시간** 단추를 `TimePickerDialog` (이 경우, 12 시간 AM/PM 모드)에서 작업에 대 한 기본 시간 형식으로 표시 됩니다:

[![AM/PM 모드로 시간 대화 상자가 표시 됩니다.](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
클릭 하면 **확인** 에 `TimePicker` 대화 상자에서 처리기 활동의 업데이트 `TextView` 와 선택한 시간 후 종료 됩니다.

[![A/M 시간은 활동 TextView에 표시 됩니다.](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

다음으로 코드를 다음 줄을 추가 `OnCreateDialog` 직후 `is24HourFormat` 선언 및 초기화 합니다.

```csharp
is24HourFormat = true;
```

이 변경 되도록 전달할 플래그를 `TimePickerDialog` 되도록 생성자 `true` 24 시간 모드를 사용 하는 호스팅 작업의 시간 형식 대신 사용 됩니다. 빌드하고 앱을 다시 실행 하는 경우 클릭 합니다 **선택 시간** 단추를 `TimePicker` 대화가 이제 24 시간 형식에 표시 됩니다:

[![24 시간 형식으로의 TimePicker 대화 상자](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

처리기를 호출 하므로 [DateTime.ToShortTimeString](xref:System.DateTime.ToShortDateString*) 작업의 시간을 인쇄 하려면 `TextView`에 계속 기본 12 시간 AM/PM 형식으로 인쇄 합니다.



## <a name="summary"></a>요약

이 문서에 표시 하는 방법을 설명는 `TimePicker` Android 활동에서 팝업 모달 대화 상자로 위젯입니다. 샘플을 제공 하지 않습니다 `DialogFragment` 구현을 설명 하 고는 `IOnTimeSetListener` 인터페이스입니다. 이 샘플도 설명 하는 방법을 `DialogFragment` 선택한 시간을 표시 하는 작업 호스트와 상호 작용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)
- [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)
- [TimePickerDialog.IOnTimeSetListener](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog+IOnTimeSetListener/)
- [TimePickerDemo (샘플)](https://developer.xamarin.com/samples/monodroid/UserInterface/TimePickerDemo)
