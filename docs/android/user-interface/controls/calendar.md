---
title: Xamarin Android 일정
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 3d74e2db541e1f30c7626cd1b08228c1e8f57a42
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029323"
---
# <a name="xamarinandroid-calendar"></a>Xamarin Android 일정

## <a name="calendar-api"></a>일정 API

Android 4에서 도입 된 새로운 일정 Api 집합은 일정 공급자의 데이터를 읽거나 쓰기 위해 디자인 된 응용 프로그램을 지원 합니다. 이러한 Api는 이벤트, 참석자 및 미리 알림을 읽고 쓰는 기능을 포함 하 여 일정 데이터를 통해 다양 한 상호 작용 옵션을 지원 합니다. 응용 프로그램에서 달력 공급자를 사용 하 여 API를 통해 추가 하는 데이터는 Android 4와 함께 제공 되는 기본 제공 일정 앱에 표시 됩니다.

## <a name="adding-permissions"></a>권한 추가

응용 프로그램에서 새로운 일정 Api를 사용할 때 가장 먼저 해야 할 일은 Android 매니페스트에 적절 한 권한을 추가 하는 것입니다. 추가 해야 하는 권한은 달력 데이터를 읽고 있는지 아니면 쓰고 있는지에 따라 `android.permisson.READ_CALENDAR` 및 `android.permission.WRITE_CALENDAR`됩니다.

## <a name="using-the-calendar-contract"></a>달력 계약 사용

사용 권한을 설정 하면 `CalendarContract` 클래스를 사용 하 여 달력 데이터와 상호 작용할 수 있습니다. 이 클래스는 응용 프로그램이 달력 공급자와 상호 작용할 때 사용할 수 있는 데이터 모델을 제공 합니다. `CalendarContract`를 사용 하면 응용 프로그램에서 일정 엔터티 (예: 달력 및 이벤트)에 대 한 Uri를 확인할 수 있습니다. 또한 달력의 이름, ID, 이벤트의 시작 및 종료 날짜와 같은 각 엔터티의 다양 한 필드와 상호 작용 하는 방법을 제공 합니다.

Calendar API를 사용 하는 예를 살펴보겠습니다. 이 예제에서는 일정 및 해당 이벤트를 열거 하는 방법 뿐만 아니라 일정에 새 이벤트를 추가 하는 방법을 살펴보겠습니다.

## <a name="listing-calendars"></a>일정 나열

먼저 달력 앱에 등록 된 달력을 열거 하는 방법을 살펴보겠습니다. 이렇게 하려면 `CursorLoader`를 인스턴스화할 수 있습니다. Android 3.0 (API 11)에 도입 된 `CursorLoader`는 `ContentProvider`를 사용 하는 기본 방법입니다. 최소한 달력 및 반환 하려는 열에 대 한 콘텐츠 Uri를 지정 해야 합니다. 이 열 사양을 _프로젝션_이라고 합니다.

`CursorLoader.LoadInBackground` 메서드를 호출 하면 일정 공급자와 같은 데이터에 대 한 콘텐츠 공급자를 쿼리할 수 있습니다.
`LoadInBackground` 실제 로드 작업을 수행 하 고 쿼리 결과와 함께 `Cursor`을 반환 합니다.

`CalendarContract` 콘텐츠 `Uri`와 프로젝션을 모두 지정 하는 데 도움이 됩니다. 일정 쿼리를 위한 콘텐츠 `Uri`를 가져오기 위해 다음과 같이 `CalendarContract.Calendars.ContentUri` 속성을 사용 하면 됩니다.

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

`CalendarContract`를 사용 하 여 원하는 달력 열을 똑같이 단순하게 지정 합니다. `CalendarContract.Calendars.InterfaceConsts` 클래스의 필드를 배열에 추가 하기만 하면 됩니다. 예를 들어 다음 코드에는 달력의 ID, 표시 이름 및 계정 이름이 포함 됩니다.

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

`Id`는 `SimpleCursorAdapter`를 사용 하 여 데이터를 UI에 바인딩하는 경우를 포함 하는 것이 중요 합니다. 콘텐츠 Uri 및 프로젝션을 준비 하면 `CursorLoader`을 인스턴스화하고 `CursorLoader.LoadInBackground` 메서드를 호출 하 여 아래와 같이 달력 데이터를 포함 하는 커서를 반환 합니다.

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

이 예제에 대 한 UI에는 단일 달력을 나타내는 목록의 각 항목이 있는 `ListView`포함 되어 있습니다. 다음 XML은 `ListView`를 포함 하는 태그를 보여 줍니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="fill_parent">
  <ListView
    android:id="@android:id/android:list"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />
</LinearLayout>
```

또한 다음과 같이 별도의 XML 파일에 저장 되는 각 목록 항목에 대해 UI를 지정 해야 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="wrap_content">
  <TextView android:id="@+id/calDisplayName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="16dip" />
  <TextView android:id="@+id/calAccountName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="12dip" />
</LinearLayout>
```

이 시점부터 커서의 데이터를 UI에 바인딩하는 것은 일반적인 Android 코드 일 뿐입니다. 다음과 같이 `SimpleCursorAdapter`를 사용 합니다.

```csharp
string[] sourceColumns = {
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName };

int[] targetResources = {
    Resource.Id.calDisplayName, Resource.Id.calAccountName };      

SimpleCursorAdapter adapter = new SimpleCursorAdapter (this,
    Resource.Layout.CalListItem, cursor, sourceColumns, targetResources);

ListAdapter = adapter;
```

위의 코드에서 어댑터는 `sourceColumns` 배열에 지정 된 열을 가져와 커서의 각 일정 항목에 대 한 `targetResources` 배열의 사용자 인터페이스 요소에 기록 합니다. 여기에 사용 되는 활동은 `ListActivity`의 서브 클래스입니다. 여기에는 어댑터를 설정 하는 `ListAdapter` 속성이 포함 됩니다.

다음은 `ListView`에 표시 되는 달력 정보가 있는 최종 결과를 보여 주는 스크린샷입니다.

[에뮬레이터에서 실행 되는![CalendarDemo, 두 개의 일정 항목 표시](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)

## <a name="listing-calendar-events"></a>일정 이벤트 나열

다음으로, 지정 된 일정에 대 한 이벤트를 열거 하는 방법을 살펴보겠습니다.
위의 예제를 기반으로 사용자가 일정 중 하나를 선택 하면 이벤트 목록이 표시 됩니다. 따라서 이전 코드에서 항목 선택 항목을 처리 해야 합니다.

```csharp
ListView.ItemClick += (sender, e) => {
    int i = (e as ItemEventArgs).Position;

    cursor.MoveToPosition(i);
    int calId =
        cursor.GetInt (cursor.GetColumnIndex (calendarsProjection [0]));

    var showEvents = new Intent(this, typeof(EventListActivity));
    showEvents.PutExtra("calId", calId);
    StartActivity(showEvents);
};
```

이 코드에서는 달력의 ID를 전달 하 여 `EventListActivity`형식의 활동을 열기 위한 의도를 만듭니다. 이벤트에 대해 쿼리할 달력을 알 수 있는 ID가 필요 합니다. `EventListActivity`의 `OnCreate` 메서드에서 아래와 같이 `Intent`에서 ID를 검색할 수 있습니다.

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

이제이 일정 ID에 대 한 쿼리 이벤트를 살펴보겠습니다. 이벤트를 쿼리 하는 프로세스는 이전에 `CalendarContract.Events` 클래스와 함께 사용할 수 있도록 이전 달력 목록을 쿼리 하는 방법과 비슷합니다. 다음 코드는 이벤트를 검색 하는 쿼리를 만듭니다.

```csharp
var eventsUri = CalendarContract.Events.ContentUri;

string[] eventsProjection = {
    CalendarContract.Events.InterfaceConsts.Id,
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart
};

var loader = new CursorLoader(this, eventsUri, eventsProjection,
                   String.Format ("calendar_id={0}", _calId), null, "dtstart ASC");
var cursor = (ICursor)loader.LoadInBackground();
```

이 코드에서는 먼저 `CalendarContract.Events.ContentUri` 속성에서 이벤트에 대 한 콘텐츠 `Uri`를 가져옵니다. 그런 다음 eventsProjection 배열에서 검색할 이벤트 열을 지정 합니다.
마지막으로,이 정보를 사용 하 여 `CursorLoader`를 인스턴스화하고 로더의 `LoadInBackground` 메서드를 호출 하 여 이벤트 데이터가 포함 된 `Cursor`을 반환 합니다.

UI에 이벤트 데이터를 표시 하기 위해 달력 목록을 표시 하기 전과 같은 방식으로 태그와 코드를 사용할 수 있습니다. 다음 코드와 같이 `SimpleCursorAdapter` 사용 하 여 데이터를 `ListView`에 바인딩합니다.

```csharp
string[] sourceColumns = {
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart };

int[] targetResources = {
    Resource.Id.eventTitle,
    Resource.Id.eventStartDate };

var adapter = new SimpleCursorAdapter (this, Resource.Layout.EventListItem,
    cursor, sourceColumns, targetResources);

adapter.ViewBinder = new ViewBinder ();       
ListAdapter = adapter;
```

이 코드와 앞에서 달력 목록을 표시 하기 위해 사용한 코드의 주요 차이점은 줄에 설정 된 `ViewBinder`의 사용입니다.

```csharp
adapter.ViewBinder = new ViewBinder ();
```

`ViewBinder` 클래스를 사용 하 여 보기에 값을 바인딩하는 방법을 세부적으로 제어할 수 있습니다. 이 경우 다음 구현에 표시 된 것 처럼이를 사용 하 여 이벤트 시작 시간을 밀리초에서 날짜 문자열로 변환 합니다.

```csharp
class ViewBinder : Java.Lang.Object, SimpleCursorAdapter.IViewBinder
{    
    public bool SetViewValue (View view, Android.Database.ICursor cursor,
        int columnIndex)
    {
        if (columnIndex == 2) {
            long ms = cursor.GetLong (columnIndex);

            DateTime date = new DateTime (1970, 1, 1, 0, 0, 0,
                DateTimeKind.Utc).AddMilliseconds (ms).ToLocalTime ();

            TextView textView = (TextView)view;
            textView.Text = date.ToLongDateString ();

            return true;
        }
        return false;
    }    
}
```

그러면 다음과 같이 이벤트 목록이 표시 됩니다.

[3 개의 일정 이벤트를 표시 하는 예제 앱의![스크린샷](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)

## <a name="adding-a-calendar-event"></a>일정 이벤트 추가

일정 데이터를 읽는 방법을 알아보았습니다. 이제 일정에 이벤트를 추가 하는 방법을 살펴보겠습니다. 이 작업을 수행 하려면 앞에서 언급 한 `android.permission.WRITE_CALENDAR` 권한을 포함 해야 합니다. 일정에 이벤트를 추가 하려면 다음을 수행 합니다.

1. `ContentValues` 인스턴스를 만듭니다.
1. `CalendarContract.Events.InterfaceConsts` 클래스의 키를 사용 하 여 `ContentValues` 인스턴스를 채웁니다.
1. 이벤트 시작 시간과 종료 시간에 대 한 표준 시간대를 설정 합니다.
1. `ContentResolver`를 사용 하 여 일정에 이벤트 데이터를 삽입 합니다.

아래 코드에서는 이러한 단계를 보여 줍니다.

```csharp
ContentValues eventValues = new ContentValues ();

eventValues.Put (CalendarContract.Events.InterfaceConsts.CalendarId,
    _calId);
eventValues.Put (CalendarContract.Events.InterfaceConsts.Title,
    "Test Event from M4A");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Description,
    "This is an event created from Xamarin.Android");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtstart,
    GetDateTimeMS (2011, 12, 15, 10, 0));
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtend,
    GetDateTimeMS (2011, 12, 15, 11, 0));

eventValues.Put(CalendarContract.Events.InterfaceConsts.EventTimezone,
    "UTC");
eventValues.Put(CalendarContract.Events.InterfaceConsts.EventEndTimezone,
    "UTC");

var uri = ContentResolver.Insert (CalendarContract.Events.ContentUri,
    eventValues);
```

표준 시간대를 설정 하지 않으면 `Java.Lang.IllegalArgumentException` 형식의 예외가 throw 됩니다. 이벤트 시간 값은 epoch 이후 밀리초 단위로 표시 되어야 하므로 날짜 사양을 밀리초 형식으로 변환 하는 `GetDateTimeMS` 메서드 (`EventListActivity`)를 만듭니다.

```csharp
long GetDateTimeMS (int yr, int month, int day, int hr, int min)
{
    Calendar c = Calendar.GetInstance (Java.Util.TimeZone.Default);

    c.Set (Java.Util.CalendarField.DayOfMonth, 15);
    c.Set (Java.Util.CalendarField.HourOfDay, hr);
    c.Set (Java.Util.CalendarField.Minute, min);
    c.Set (Java.Util.CalendarField.Month, Calendar.December);
    c.Set (Java.Util.CalendarField.Year, 2011);

    return c.TimeInMillis;
}
```

이벤트 목록 UI에 단추를 추가 하 고 단추의 click 이벤트 처리기에서 위의 코드를 실행 하는 경우 이벤트가 달력에 추가 되 고 아래와 같이 목록에서 업데이트 됩니다.

[일정 이벤트와 샘플 이벤트 추가 단추가 있는 예제 앱의![스크린샷](calendar-images/13.png)](calendar-images/13.png#lightbox)

일정 앱을 여는 경우에도 이벤트가 기록 됩니다.

[선택한 일정 이벤트를 표시 하는 일정 앱의![스크린샷](calendar-images/14.png)](calendar-images/14.png#lightbox)

여기에서 볼 수 있듯이 Android를 사용 하면 쉽고 간편 하 게 일정 데이터를 검색 및 유지할 수 있으므로 응용 프로그램에서 일정 기능을 원활 하 게 통합할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [달력 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/calendardemo)
- [아이스크림 및 사우스 샌드위치](https://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](https://developer.android.com/sdk/android-4.0.html)
