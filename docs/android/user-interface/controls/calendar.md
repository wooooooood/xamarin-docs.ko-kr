---
title: 일정
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d48af16f50c5a4482342d323b5c73b9c237cc83a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767978"
---
# <a name="calendar"></a>일정


## <a name="calendar-api"></a>달력 API

달력 Api Android 4에 도입 된 새로운 집합이 달력 공급자에 데이터를 쓰거나 읽을 하도록 설계 된 응용 프로그램을 지원 합니다. 이러한 Api는 다양 한 읽기 및 쓰기 이벤트, 참석자 및 알림 기능을 비롯 한 일정 데이터와 상호 작용 옵션을 지원 합니다. 응용 프로그램에서 달력 공급자를 사용 하면 API를 통해 추가 데이터는 Android 4와 함께 제공 되는 기본 제공 일정 앱에 표시 됩니다.


## <a name="adding-permissions"></a>사용 권한 추가

응용 프로그램에서 새 일정 Api 사용할 때 가장 먼저 해야 할 Android 매니페스트에 적절 한 사용 권한을 추가 됩니다. 에 추가 해야 권한은 `android.permisson.READ_CALENDAR` 및 `android.permission.WRITE_CALENDAR`읽기 또는 일정 데이터를 작성 하는지 여부에 따라 합니다.


## <a name="using-the-calendar-contract"></a>달력 계약을 사용 하 여

사용 권한을 설정 하면 상호 작용할 수 있습니다 일정 데이터를 사용 하 여는 `CalendarContract` 클래스입니다. 이 클래스는 응용 프로그램은 일정 공급자 상호 작용 하는 경우 사용할 수 있는 데이터 모델을 제공 합니다. `CalendarContract` 응용 프로그램이 일정 및 이벤트 등의 달력 엔터티로 Uri를 확인할 수 있습니다. 또한 일정의 이름 및 ID 또는 이벤트의 시작 및 종료 날짜 등의 각 엔터티에의 다양 한 필드와 상호 작용 하는 방법을 제공 합니다.

일정 API를 사용 하는 예제를 살펴 보겠습니다. 이 예제에서는 달력에 새 이벤트를 추가 하는 방법 뿐만 아니라 캘린더 및 해당 이벤트를 열거 하는 방법을 검토 합니다.


## <a name="listing-calendars"></a>일정을 나열

첫째, 일정 앱에 등록 되어 있는 달력을 열거 하는 방법을 살펴보겠습니다. 이 위해 인스턴스화할 수 우리는 `CursorLoader`합니다. Android 3.0 (API 11)에 도입 된 `CursorLoader` 를 사용 하 여 사용 하는 `ContentProvider`합니다. 일정 및; 반환 하려는 열에 대 한 콘텐츠 Uri를 지정 해야 여기에 최소한 이 열 사양이 라고는 _프로젝션_합니다.

호출의 `CursorLoader.LoadInBackground` 메서드를 달력 공급자 등의 데이터에 대 한 콘텐츠 공급자를 쿼리할 수 있습니다.
`LoadInBackground` 실제 로드 작업을 수행 하 고 반환 된 `Cursor` 쿼리의 결과 함께 합니다.

`CalendarContract` 데 도움을 주는 우리는 콘텐츠를 모두 지정 `Uri` 및 프로젝션 합니다. 내용을 가져올 `Uri` 달력 쿼리를 사용 하기만 하면 우리는 `CalendarContract.Calendars.ContentUri` 다음과 같이 속성:

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

사용 하는 `CalendarContract` 있는 달력을 지정 하려면 열 선택 했으므로 매우 간단 합니다. 필드에 추가 `CalendarContract.Calendars.InterfaceConsts` 배열에는 클래스입니다. 다음 코드에서 일정의 ID를 포함 하는 예를 들어, 표시 이름 및 계정 이름:

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

`Id` 사용 하는 경우를 포함 하는 것이 중요 한 `SimpleCursorAdapter` 곧 알 수 있듯이 데이터는 UI에 바인딩할 합니다. 인스턴스화할에서는 콘텐츠 Uri 및 위치에 프로젝션 된는 `CursorLoader` 호출는 `CursorLoader.LoadInBackground` 아래 표시 된 대로 일정 데이터와 함께 커서를 반환 하는 메서드:

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

이 예제에 대 한 UI에는 `ListView`, 한 달력을 나타내는 목록에서 각 항목입니다. 다음 XML 포함 된 태그를 보여 줍니다.는 `ListView`:

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

또한 다음과 같이 별도 XML 파일에 넣고 각 목록 항목에 대 한 UI를 지정 해야 합니다.

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

이 시점부터 것은 현재 커서 위치에서 데이터는 UI에 바인딩할 방금 일반 Android 코드입니다. 에서는 한 `SimpleCursorAdapter` 다음과 같습니다.

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

위의 코드에서 어댑터 사용에 지정 된 열은 `sourceColumns` 배열 하 고에서 사용자 인터페이스 요소에 기록는 `targetResources` 커서에 있는 각 일정 항목에 대 한 배열입니다. 여기에 활동의 서브 클래스는 `ListActivity`; 포함 된 `ListAdapter` 속성에는 어댑터 설정 합니다.

다음은 최종 결과에 표시 되는 일정 정보를 보여 주는 스크린 샷은 `ListView`:

[![CalendarDemo 두 일정 항목을 표시 하는 에뮬레이터에서 실행](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)



## <a name="listing-calendar-events"></a>일정 이벤트 목록

다음 지정 된 일정에 대 한 이벤트를 열거 하는 방법을 살펴보겠습니다.
위의 예제를 쌓아 살펴보겠습니다 사용자가 달력 중 하나를 선택 하는 이벤트 목록을입니다. 따라서 항목 선택을 이전 코드에서 처리 해야 합니다.

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

이 코드에서는 유형의 활동 열려는 의도 만들려는 `EventListActivity`, 의도에 일정의 ID를 전달 합니다. 달력 이벤트에 대 한 쿼리를 알고 ID 필요 합니다. 에 `EventListActivity`의 `OnCreate` 메서드를에서 제공 된 ID를 검색할 수는 `Intent` 아래와 같이:

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

이제 보겠습니다이 대 한 쿼리 이벤트 일정 id입니다. 이벤트를 쿼리 하는 프로세스는 이전에 달력의 목록에 대 한 쿼리 म 방식과 유사 하 게 협력 합니다이 경우에만 `CalendarContract.Events` 클래스입니다. 다음 코드는 이벤트를 검색 하는 쿼리를 만듭니다.

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

이 코드에서는 먼저 콘텐츠를 가져오도록 `Uri` 의 이벤트에 대해는 `CalendarContract.Events.ContentUri` 속성입니다. 그런 다음 eventsProjection 배열에서 검색 하려는 이벤트 열을 지정 합니다.
인스턴스화할 마지막으로 `CursorLoader` 이 정보 및 호출 로더의 `LoadInBackground` 반환 하는 메서드는 `Cursor` 와 이벤트 데이터입니다.

UI에 이벤트 데이터를 표시 하려면 태그와 코드 달력 목록을 표시 하려면 이전에 수행 했던 것 처럼 사용할 수 있습니다. 사용 하 여 다시 `SimpleCursorAdapter` 데이터를 바인딩하는 `ListView` 다음 코드에 나와 있는 것 처럼:

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

이 코드와 이전 달력 목록 표시를 사용 하는 코드 간의 주요 차이점은 사용 하는 `ViewBinder`, 줄에 설정 된:

```csharp
adapter.ViewBinder = new ViewBinder ();
```

`ViewBinder` 클래스를 더 보기에 값을 바인딩할 우리는 방법을 제어할 수 있습니다. 이 경우 사용 것 밀리초 이벤트 시작 시간 날짜 문자열을 변환할 대상 다음 구현에 표시 된 대로 합니다.

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

아래와 같이 이벤트 목록이 표시 됩니다.

[![세 가지 일정 이벤트를 표시 하는 예제 앱의 스크린 샷](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)



## <a name="adding-a-calendar-event"></a>일정 이벤트를 추가합니다.

일정 데이터를 읽는 방법을 살펴보았습니다. 이제 달력에 이벤트를 추가 하는 방법을 살펴보겠습니다. 이렇게 하려면 수를 포함 해야 합니다는 `android.permission.WRITE_CALENDAR` 앞에서 언급 한 사용 권한. 이벤트에는 일정을 추가 하려면 수행 합니다.

1.  만들기는 `ContentValues` 인스턴스.
1.  키를 사용 하 여는 `CalendarContract.Events.InterfaceConsts` 를 채우는 클래스는 `ContentValues` 인스턴스.
1.  이벤트의 시작에 대 한 표준 시간대를 설정 하 고 종료 시간입니다.
1.  사용 하 여 한 `ContentResolver` 달력에 이벤트 데이터를 삽입 합니다.


다음 코드에서는 다음이 단계를 보여 줍니다.

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

되는 경우 표준 시간대를 형식의 예외를 설정 하지 않으면 म `Java.Lang.IllegalArgumentException` throw 됩니다. Epoch 이후의 시간 (밀리초)로 이벤트 시간 값을 나타내야, 때문에 생성 한 `GetDateTimeMS` 메서드 (에서 `EventListActivity`) 당사의 날짜 사양 밀리초 형식으로 변환 하려면:

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

이벤트 목록 UI에 단추를 추가 하 고 위의 코드를 실행 하는 경우 단추의 클릭 이벤트 처리기, 이벤트 일정에 추가 되 고 아래와 같이 목록에서 업데이트:

[![샘플 이벤트 추가 단추를 클릭 하는 달력 이벤트와 예제 응용 프로그램의 스크린 샷](calendar-images/13.png)](calendar-images/13.png#lightbox)

일정 앱을 열고에서는 이벤트 기록 되어 있는지 있습니다도 표시 됩니다 했습니다.

[![선택한 일정 이벤트를 표시 하는 일정 앱의 스크린 샷](calendar-images/14.png)](calendar-images/14.png#lightbox)

볼 수 있듯이 Android 액세스할 수 있도록 강력 하 고 쉽게 검색 한 일정 데이터를 유지 하 일정 기능을 원활 하 게 통합 응용 프로그램을 허용 합니다.


## <a name="related-links"></a>관련 링크

- [달력 데모 (샘플)](https://developer.xamarin.com/samples/CalendarDemo/)
- [아이스크림 샌드위치 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
