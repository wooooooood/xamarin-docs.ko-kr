---
title: 시간 선택기
description: TimePickerDialog 및 DialogFragment를 사용 하 여 시간 선택
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: dfee003ba327b199974ae277a93cb1ca55a81b0d
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522887"
---
# <a name="android-time-picker"></a>Android 시간 선택

사용자가 시간을 선택할 수 있는 방법을 제공 하려면 [Timepicker](xref:Android.Widget.TimePicker)를 사용 합니다. Android 앱은 일반적 `TimePicker` 으로 시간 값 &ndash; 을 선택 하기 위해 [time 대화 상자](xref:Android.App.TimePickerDialog) 와 함께 사용 되며, 장치 및 응용 프로그램 간에 일관 된 인터페이스를 보장 하는 데 도움이 됩니다. `TimePicker`사용자가 24 시간 또는 오전 12 시/오후 모드에서 하루 중 시간을 선택할 수 있습니다.
`TimePickerDialog`는 대화 상자에서를 `TimePicker` 캡슐화 하는 도우미 클래스입니다.

[![작업 중인 시간 선택 대화 상자의 예제 스크린샷](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>개요

최신 Android 응용 프로그램은 `TimePickerDialog` 를 [dialogfragment](xref:Android.App.DialogFragment)에 표시 합니다. 이렇게 하면 응용 프로그램에서를 `TimePicker` 팝업 대화 상자로 표시 하거나 활동에 포함할 수 있습니다. 또한은 `DialogFragment` 대화의 수명 주기와 표시를 관리 하 여 구현 해야 하는 코드의 양을 줄입니다.

이 가이드에서는에 래핑된 `TimePickerDialog` `DialogFragment`를 사용 하는 방법을 보여 줍니다. 샘플 응용 프로그램은 사용자 `TimePickerDialog` 가 활동의 단추를 클릭할 때를 모달 대화 상자로 표시 합니다. 사용자가 시간을 설정 하면 대화 상자가 종료 되 고 처리기가 활동 화면에서를 `TextView` 선택한 시간으로 업데이트 합니다.

## <a name="requirements"></a>요구 사항

이 가이드의 샘플 응용 프로그램은 Android 4.1 (API 수준)을 대상으로 합니다.
16) 이상 이지만 Android 3.0 (API 레벨 11 이상)에서 사용할 수 있습니다. Android 지원 라이브러리 v4를 프로젝트에 추가 하 고 일부 코드를 변경 하 여 이전 버전의 Android를 지원할 수 있습니다.

## <a name="using-the-timepicker"></a>TimePicker 사용

이 예제에서는 `DialogFragment`을 (를) 확장 `DialogFragment` 합니다. `TimePickerFragment` (아래에 있는)의 `TimePickerDialog`하위 클래스 구현에서는을 (를) 호스팅합니다. 샘플 앱이 처음 시작 될 때 선택 된 시간을 표시 하는 데 사용 `TextView` 되는 위에 **선택 시간** 단추가 표시 됩니다.

[![초기 샘플 앱 화면](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

[ **선택 시간** ] 단추를 클릭 하면 예제 앱은 다음 스크린샷에 `TimePickerDialog` 표시 된 것 처럼을 시작 합니다.

[![앱에서 표시 하는 기본 시간 선택 대화 상자의 스크린샷](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

에서 시간을 선택 하 고 **확인** 단추를 클릭 하면에서 `TimePickerDialog` 메서드를 호출 합니다 [. ontimeset](xref:Android.App.TimePickerDialog.IOnTimeSetListener.OnTimeSet*). `TimePickerDialog`
이 인터페이스는 호스팅 `DialogFragment` (`TimePickerFragment`아래 설명 참조)에 의해 구현 됩니다. **취소** 단추를 클릭 하면 조각 및 대화 상자가 해제 됩니다.

`DialogFragment`다음 세 가지 방법 중 하나로 호스팅 활동에 선택한 시간을 반환 합니다.

1. **메서드 호출 또는 속성 설정** &ndash; 활동은이 값을 설정 하기 위해 구체적으로 속성 또는 메서드를 제공할 수 있습니다.

2. **이벤트 발생** 는가 호출 될 때 `OnTimeSet` 발생 하는 이벤트를 정의할 수 있습니다. `DialogFragment` &ndash;

3. 를 사용 하 여를 호출 하`Action<DateTime>` 여 활동의 시간을 표시할 수 있습니다. **`Action`** &ndash; `DialogFragment` 활동은를 `DialogFragment`인스턴스화할 때 `Action<DateTime` 를 제공 합니다.

이 샘플에서는 활동에서에 `Action<DateTime>` 처리기 `DialogFragment`를 제공 해야 하는 세 번째 기법을 사용 합니다.

## <a name="start-an-app-project"></a>앱 프로젝트 시작

**Time Kerdemo** 라는 새 Android 프로젝트를 시작 합니다. xamarin.ios 프로젝트를 만드는 방법을 잘 모르는 경우에는 [Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md) 를 참조 하 여 새 프로젝트를 만드는 방법을 알아보세요.

**리소스/레이아웃/기본. axml** 을 편집 하 고 내용을 다음 xml로 바꿉니다.

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

이는 시간을 표시 하는 [TextView](xref:Android.Widget.TextView) 와을 여 `TimePickerDialog`는 [단추](xref:Android.Widget.Button) 를 표시 하는 기본 [LinearLayout](xref:Android.Widget.LinearLayout) 입니다. 이 레이아웃은 하드 코드 된 문자열과 차원을 사용 하 여 앱을 보다 간단 하 고 쉽게 이해할 &ndash; 수 있도록 합니다. 프로덕션 앱은 일반적으로 이러한 값에 대해 리소스를 사용 합니다 ( [DatePicker](https://github.com/xamarin/recipes/blob/master/Recipes/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) 코드 예제에서 볼 수 있음).

**MainActivity.cs** 를 편집 하 고 내용을 다음 코드로 바꿉니다.

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

이 예제를 작성 하 고 실행 하면 다음 스크린샷과 비슷한 초기 화면이 표시 되어야 합니다.

[![초기 앱 화면](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

을 표시 하기 위해 `DialogFragment` 이 아직 `TimePicker`구현 되지 않았기 때문에 **선택 시간** 단추를 클릭 해도 아무 작업도 수행 되지 않습니다.
다음 단계는이 `DialogFragment`를 만드는 것입니다.

## <a name="extending-dialogfragment"></a>DialogFragment 확장

에서 `DialogFragment` `DialogFragment` 사용 하기 위해를 확장 하려면에서 파생 되 고을 구현 `TimePickerDialog.IOnTimeSetListener`하는 하위 클래스를 만들어야 합니다. `TimePicker` **MainActivity.cs**에 다음 클래스를 추가 합니다.

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

이 `TimePickerFragment` 클래스는 더 작은 부분으로 세분화 되며 다음 섹션에서 설명 합니다.

### <a name="dialogfragment-implementation"></a>DialogFragment 구현

`TimePickerFragment`는 팩터리 메서드, 대화 상자 인스턴스화 메서드 및 `OnTimeSet` 에 `TimePickerDialog.IOnTimeSetListener`필요한 처리기 메서드 등의 몇 가지 메서드를 구현 합니다.

- `TimePickerFragment`는의 `DialogFragment`서브 클래스입니다. 또한 인터페이스를 `TimePickerDialog.IOnTimeSetListener` 구현 합니다. 즉, 필요한 `OnTimeSet` 메서드를 제공 합니다.

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

- `TAG`는 로깅을 위해 초기화 됩니다 (*Mytimepickerfragment* 를 사용 하려는 문자열로 변경할 수 있음). 작업 `timeSelectedHandler` 은 null 참조 예외를 방지 하기 위해 빈 대리자로 초기화 됩니다.

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

- 팩터리 메서드를 호출 하 여 새 `TimePickerFragment`를 인스턴스화합니다. `NewInstance` 이 메서드는 사용자 `Action<DateTime>` 가에서 `TimePickerDialog` **확인** 단추를 클릭할 때 호출 되는 처리기를 사용 합니다.

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

- 조각이 표시 될 때 Android는 `DialogFragment` [oncreatedialog](xref:Android.App.DialogFragment.OnCreateDialog*)메서드를 호출 합니다.
    이 메서드는 새 `TimePickerDialog` 개체를 만들고 활동, 콜백 개체 (의 현재 인스턴스 `TimePickerFragment`) 및 현재 시간을 사용 하 여 초기화 합니다.

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

- 사용자가 `TimePicker` 대화 상자 `OnTimeSet` 에서 시간 설정을 변경 하는 경우 메서드가 호출 됩니다. `OnTimeSet`현재 날짜 `DateTime` 와 사용자가 선택한 시간 (시 및 분)의 병합을 사용 하 여 개체를 만듭니다.

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


- 이 `DateTime` 개체는 생성 시 `TimePickerFragment` 개체 `timeSelectedHandler` 에 등록 된에 전달 됩니다. `OnTimeSet`활동의 시간 표시를 선택한 시간으로 업데이트 하기 위해이 처리기를 호출 합니다 (이 처리기는 다음 섹션에서 구현 됨).

    ```csharp
    timeSelectedHandler (selectedTime);
    ```

## <a name="displaying-the-timepickerfragment"></a>Time Kerfragment 표시

가 구현 되었으므로 이제 `NewInstance` 팩터리 메서드를 사용 하 여를 `DialogFragment` 인스턴스화하고 dialogfragment를 호출 하 여 표시 해야 [합니다. Show](xref:Android.App.DialogFragment.Show*): `DialogFragment`

`MainActivity`에 다음 메서드를 추가합니다.

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

는 `TimeSelectOnClick` 를`TimePickerFragment`인스턴스화한 후 전달 된 시간 값을 사용 하 여 활동의 시간 표시를 업데이트 하는 무명 메서드에 대 한 대리자를 만들고 전달 합니다. 마지막으로 사용자 `TimePicker` `TimePicker` 에 게를 표시 하는 `DialogFragment.Show`대화 상자 조각 (via)을 시작 합니다.

`OnCreate` 메서드 끝에 다음 줄을 추가 하 여 대화 상자를 시작 하는 **선택 시간** 단추에 이벤트 처리기를 연결 합니다.

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

**선택 시간** 단추를 클릭 `TimeSelectOnClick` 하면 사용자에 게 `TimePicker` 대화 상자 조각을 표시 하기 위해가 호출 됩니다.

## <a name="try-it"></a>실습

앱을 빌드하고 실행합니다. **선택 시간** 단추 `TimePickerDialog` 를 클릭 하면가 활동에 대 한 기본 시간 형식으로 표시 됩니다 (이 경우 12 시간 AM/PM 모드).

[![시간 대화 상자는 AM/PM 모드로 표시 됩니다.](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
`TimePicker` 대화 상자에서 **확인** 을 클릭 하면 처리기가 활동을 선택한 시간 `TextView` 으로 업데이트 한 후 종료 됩니다.

[![A/M 시간이 활동 TextView 표시 됩니다.](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

다음에 `is24HourFormat` 는가 선언 되 고 초기화 된 `OnCreateDialog` 직후에 다음 코드 줄을 추가 합니다.

```csharp
is24HourFormat = true;
```

이렇게 변경 하면 호스팅 활동의 시간 형식 `TimePickerDialog` 대신 24 시간 `true` 모드를 사용 하도록 생성자에 전달 된 플래그가 강제로 적용 됩니다. 앱을 빌드 및 실행 하는 경우 **선택 시간** 단추 `TimePicker` 를 클릭 하면 이제 24 시간 형식으로 대화 상자가 표시 됩니다.

[![24 시간 형식의 TimePicker 대화 상자](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

처리기가 [ToShortTimeString](xref:System.DateTime.ToShortDateString*) 를 호출 하 여 시간을 활동의 `TextView`에 인쇄 하기 때문에 시간은 여전히 기본 12 시간 AM/PM 형식으로 인쇄 됩니다.

## <a name="summary"></a>요약

이 문서에서는 Android 작업에서 `TimePicker` 위젯을 팝업 모달 대화 상자로 표시 하는 방법을 설명 했습니다. 샘플 `DialogFragment` 구현을 제공 하 고 인터페이스에 `IOnTimeSetListener` 대해 설명 했습니다. 또한이 샘플에서는가 `DialogFragment` 호스트 작업과 상호 작용 하 여 선택한 시간을 표시 하는 방법을 보여 주었습니다.

## <a name="related-links"></a>관련 링크

- [DialogFragment](xref:Android.App.DialogFragment)
- [TimePicker](xref:Android.Widget.TimePicker)
- [TimePickerDialog](xref:Android.App.TimePickerDialog)
- [TimePickerDialog.IOnTimeSetListener](xref:Android.App.TimePickerDialog.IOnTimeSetListener)
- [Time Kerdemo (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-timepickerdemo)
