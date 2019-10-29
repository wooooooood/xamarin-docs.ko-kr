---
title: Xamarin.ios의 EventKit
description: 이 문서에서는 EventKit 및 Xamarin.ios에서이를 사용 하는 방법을 설명 합니다. 달력, 일정 이벤트 및 미리 알림을 설명 하 고, EventKit를 사용 하 여 프로그래밍할 때 일반적으로 사용 되는 클래스를 살펴봅니다.
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 1be6da2bbaf4aeffe00d90945bd06867f929c334
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032545"
---
# <a name="eventkit-in-xamarinios"></a>Xamarin.ios의 EventKit

iOS에는 달력 응용 프로그램 및 미리 알림 응용 프로그램 이라는 두 가지 달력 관련 응용 프로그램이 있습니다. 달력 응용 프로그램에서 일정 데이터를 관리 하는 방법을 이해할 수 있을 정도로 간단 하지만 미리 알림 응용 프로그램은 명확 하지 않습니다. 알림을 사용 하는 경우, 완료 시기 등을 기준으로 미리 알림이 실제로 연결 된 날짜를 사용할 수 있습니다. 이와 같이 iOS는 일정 이벤트 또는 미리 알림이 일정 *데이터베이스*라는 한 위치에서 모든 달력 데이터를 저장 합니다.

EventKit 프레임 워크는 일정 데이터베이스에서 저장 하는 일정, *일정 이벤트*및 *미리 알림* *데이터에 액세스*하는 방법을 제공 합니다. IOS 4부터 캘린더 및 일정 이벤트에 대 한 액세스를 사용할 수 있지만 미리 알림에 대 한 액세스는 iOS 6에서 새롭게 제공 됩니다.

이 가이드에서는 다음에 대해 다룹니다.

- **Eventkit 기본 사항** – 주요 클래스를 통해 기본 제공 되는 eventkit를 소개 하 고 사용법을 이해 합니다. 이 섹션은 문서의 다음 부분을 주요 당면 하기 전에 읽어야 합니다. 
- **일반 작업** – 일반 작업 섹션은와 같은 일반적인 작업을 수행 하는 방법에 대 한 빠른 참조를 제공 하기 위한 것입니다. 캘린더 이벤트를 만들고 수정 하는 데 기본 제공 컨트롤러를 사용 하는 것 뿐만 아니라 달력 열거, 일정 이벤트와 미리 알림 만들기, 저장 및 검색 이 섹션은 특정 작업에 대 한 참조 이므로 앞으로 읽기가 필요 하지 않습니다. 

이 가이드의 모든 태스크는 부록 샘플 응용 프로그램에서 사용할 수 있습니다.

 [![](eventkit-images/01.png "The companion sample application screens")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>요구 사항

EventKit는 iOS 4.0에서 도입 되었지만 미리 알림 데이터에 대 한 액세스는 iOS 6.0에서 도입 되었습니다. 따라서 일반적인 EventKit 개발을 수행 하려면 버전 4.0을 대상으로 하 고 미리 알림을 위해 6.0를 대상으로 해야 합니다.

또한 미리 알림 응용 프로그램은 시뮬레이터에서 사용할 수 없습니다. 즉, 먼저 추가 하지 않는 한 미리 알림 데이터를 사용할 수 없습니다. 또한 액세스 요청은 실제 장치의 사용자 에게만 표시 됩니다. 따라서 EventKit 개발은 장치에서 가장 잘 테스트 됩니다.

## <a name="event-kit-basics"></a>이벤트 키트 기본 사항

EventKit를 사용 하는 경우 공용 클래스 및 사용을 이해 하는 것이 중요 합니다. 이러한 모든 클래스는 `EventKit` 및 `EventKitUI` (`EKEventEditController`)에서 찾을 수 있습니다.

### <a name="eventstore"></a>EventStore

*Eventstore* 클래스는 eventstore에서 작업을 수행 하는 데 필요 하므로 eventstore에서 가장 중요 한 클래스입니다. 모든 EventKit 데이터에 대해 영구 저장소 또는 데이터베이스 엔진으로 간주할 수 있습니다. `EventStore`에서 일정 응용 프로그램의 달력과 일정 이벤트와 미리 알림 응용 프로그램의 미리 알림 모두에 액세스할 수 있습니다.

`EventStore`는 데이터베이스 엔진과 유사 하기 때문에 응용 프로그램 인스턴스 수명 중에 최대한 적은 시간을 만들고 제거 해야 한다는 것을 의미 합니다. 실제로 응용 프로그램에서 `EventStore` 인스턴스를 하나 만든 후에는 다시 필요 하지 않을 경우 응용 프로그램의 전체 수명 동안 해당 참조를 유지 하는 것이 좋습니다. 또한 모든 호출은 단일 `EventStore` 인스턴스로 이동 해야 합니다. 따라서 단일 인스턴스를 사용 가능 하 게 유지 하기 위해 Singleton 패턴을 사용 하는 것이 좋습니다.

#### <a name="creating-an-event-store"></a>이벤트 저장소 만들기

다음 코드는 `EventStore` 클래스의 단일 인스턴스를 만들고 응용 프로그램 내에서 정적으로 사용할 수 있도록 하는 효율적인 방법을 보여 줍니다.

```csharp
public class App
{
    public static App Current {
            get { return current; }
    }
    private static App current;

    public EKEventStore EventStore {
            get { return eventStore; }
    }
    protected EKEventStore eventStore;

    static App ()
    {
            current = new App();
    }
    protected App () 
    {
            eventStore = new EKEventStore ( );
    }
}
```

위의 코드는 Singleton 패턴을 사용 하 여 응용 프로그램이 로드 될 때 `EventStore`의 인스턴스를 인스턴스화합니다. 그러면 다음과 같이 응용 프로그램 내에서 전역적으로 `EventStore`에 액세스할 수 있습니다.

```csharp
App.Current.EventStore;
```

여기에 나오는 모든 예제에서는이 패턴을 사용 하므로 `App.Current.EventStore`를 통해 `EventStore`를 참조 합니다.

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>일정 및 미리 알림 데이터에 대 한 액세스 요청

응용 프로그램은 EventStore를 통해 데이터에 액세스 하도록 허용 하기 전에 먼저 필요한 항목에 따라 일정 이벤트 데이터 또는 미리 알림 데이터에 대 한 액세스를 요청 해야 합니다. 이를 용이 하 게 하기 위해 `EventStore` `RequestAccess` 라는 메서드를 노출 합니다 .이 메서드를 호출 하면에 전달 된 `EKEntityType`에 따라 응용 프로그램이 일정 데이터 또는 미리 알림 데이터에 대 한 액세스를 요청 하 고 있음을 사용자에 게 알리는 경고 보기가 표시 됩니다. 메서드. 경고 보기를 발생 시키므로 호출은 비동기적 이며 두 개의 매개 변수를 수신 하는 `NSAction` (또는 람다)로 전달 되는 완료 처리기를 호출 합니다. 액세스 권한이 부여 되었는지 여부를 나타내는 부울 값입니다. null이 아닌 경우 요청에 오류 정보가 포함 되어 있으면이 `NSError`고, 그렇지 않으면입니다. 예를 들어 다음 코딩 된는 일정 이벤트 데이터에 대 한 액세스를 요청 하 고 요청이 부여 되지 않은 경우 경고 보기를 표시 합니다.

```csharp
App.Current.EventStore.RequestAccess (EKEntityType.Event, 
    (bool granted, NSError e) => {
            if (granted)
                    //do something here
            else
                    new UIAlertView ( "Access Denied", 
"User Denied Access to Calendar Data", null,
"ok", null).Show ();
            } );
```

요청이 승인 되 면 응용 프로그램이 장치에 설치 되 고 사용자에 게 경고를 표시 하지 않는 한 해당 요청이 기억 됩니다.
그러나 액세스는 일정 이벤트 또는 미리 알림이 부여 된 리소스 유형에만 제공 됩니다. 응용 프로그램에서 두 가지 모두에 액세스 해야 하는 경우 둘 다 요청 해야 합니다.

사용 권한은 기억 되기 때문에 매번 요청을 만드는 것은 비교적 저렴 하므로, 작업을 수행 하기 전에 항상 액세스를 요청 하는 것이 좋습니다.

또한 완료 처리기가 별도의 (비 UI) 스레드에서 호출 되기 때문에 완료 처리기의 UI에 대 한 모든 업데이트는 `InvokeOnMainThread`를 통해 호출 되어야 합니다. 그렇지 않으면 예외가 throw 되 고 catch 되지 않으면 응용 프로그램의 작동이 중단 됩니다.

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType`은 `EventKit` 항목 또는 데이터의 형식을 설명 하는 열거형입니다. `Event`와 미리 알림 이라는 두 가지 값이 있습니다. 이 클래스는 액세스 하거나 검색할 데이터 종류 `EventKit`를 알려 주는 `EventStore.RequestAccess`를 포함 하 여 다양 한 방법으로 사용 됩니다.

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar* 은 달력 이벤트의 그룹을 포함 하는 달력을 나타냅니다. 일정은 로컬, *iCloud*, *Exchange Server* , *Google*등의 타사 공급자 위치에 저장 하는 등의 다양 한 위치에 저장 될 수 있습니다. 여러 번 `EKCalendar` 사용 하 여 이벤트를 찾을 위치 또는 저장할 위치를 `EventKit`를 알려 줍니다.

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController* 는 `EventKitUI` 네임 스페이스에서 찾을 수 있으며, 달력 이벤트를 편집 하거나 만드는 데 사용할 수 있는 기본 제공 컨트롤러입니다. 기본 제공 카메라 컨트롤러와 마찬가지로 `EKEventEditController` UI를 표시 하 고 저장을 처리 하는 데 많은 도움이 됩니다.

### <a name="ekevent"></a>EKEvent

 *Ekevent* 는 달력 이벤트를 나타냅니다. `EKEvent`와 `EKReminder` 모두 `EKCalendarItem`에서 상속 되며 `Title`, `Notes`등의 필드를 포함 합니다.

### <a name="ekreminder"></a>EKReminder

 *EKReminder* 는 미리 알림 항목을 나타냅니다.

### <a name="ekspan"></a>EKSpan

*EKSpan* 는 되풀이 가능한 이벤트를 수정할 때 이벤트의 범위를 설명 하는 열거형으로, 두 값 ( *ThisEvent* 및 *FutureEvents*)이 있습니다. `ThisEvent`는 참조 되는 계열의 특정 이벤트에 대해서만 변경 내용이 발생 하는 반면 `FutureEvents`는 해당 이벤트와 미래의 모든 되풀이에 영향을 줍니다.

## <a name="tasks"></a>태스크

편리 하 게 사용할 수 있도록 EventKit 사용은 다음 섹션에 설명 된 일반적인 작업으로 세분화 되었습니다.

### <a name="enumerate-calendars"></a>일정 열거

사용자가 장치에 구성한 일정을 열거 하려면 `EventStore`에서 `GetCalendars`를 호출 하 고 받으려는 일정 유형 (미리 알림 또는 이벤트)을 전달 합니다.

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>기본 제공 컨트롤러를 사용 하 여 이벤트 추가 또는 수정

일정 응용 프로그램을 사용할 때 사용자에 게 표시 되는 것과 동일한 UI를 사용 하 여 이벤트를 만들거나 편집 하려는 경우 *EKEventEditViewController* 는 많은 작업을 수행 합니다.

 [![](eventkit-images/02.png "The UI that is presented to the user when using the Calendar Application")](eventkit-images/02.png#lightbox)

이를 사용 하려면 메서드 내에서 선언 되는 경우 가비지 수집 되지 않도록 클래스 수준 변수로 선언 해야 합니다.

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

그런 다음 시작 하려면이를 인스턴스화하고, `EventStore`에 대 한 참조를 제공 하 고, *EKEventEditViewDelegate* 대리자를 연결 하 고, `PresentViewController`를 사용 하 여 표시 합니다.

```csharp
EventKitUI.EKEventEditViewController eventController = 
        new EventKitUI.EKEventEditViewController ();

// set the controller's event store - it needs to know where/how to save the event
eventController.EventStore = App.Current.EventStore;

// wire up a delegate to handle events from the controller
eventControllerDelegate = new CreateEventEditViewDelegate ( eventController );
eventController.EditViewDelegate = eventControllerDelegate;

// show the event controller
PresentViewController ( eventController, true, null );
```

필요에 따라 이벤트를 미리 채우려면 새 이벤트를 만들거나 (아래와 같이) 저장 된 이벤트를 검색할 수 있습니다.

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and exercise!";
newEvent.Notes = "This is your reminder to go and exercise for 30 minutes.”;
```

UI를 미리 채우려면 컨트롤러에서 이벤트 속성을 설정 해야 합니다.

```csharp
eventController.Event = newEvent;
```

기존 이벤트를 사용 하려면 나중에 *ID로 이벤트 검색* 섹션을 참조 하십시오.

대리자는 사용자가 대화 상자를 사용 하 여 완료 될 때 컨트롤러에서 호출 하는 `Completed` 메서드를 재정의 해야 합니다.

```csharp
protected class CreateEventEditViewDelegate : EventKitUI.EKEventEditViewDelegate
{
        // we need to keep a reference to the controller so we can dismiss it
        protected EventKitUI.EKEventEditViewController eventController;

        public CreateEventEditViewDelegate (EventKitUI.EKEventEditViewController eventController)
        {
                // save our controller reference
                this.eventController = eventController;
        }

        // completed is called when a user eith
        public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
        {
                eventController.DismissViewController (true, null);
                }
        }
}
```

필요에 따라 대리자에서 `Completed` 메서드의 *작업* 을 확인 하 여 이벤트를 수정 하 고 다시 저장 하거나 취소 된 경우 다른 작업을 수행할 수 있습니다.

```csharp
public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
{
        eventController.DismissViewController (true, null);

        switch ( action ) {

        case EKEventEditViewAction.Canceled:
                break;
        case EKEventEditViewAction.Deleted:
                break;
        case EKEventEditViewAction.Saved:
                // if you wanted to modify the event you could do so here,
// and then save:
                //App.Current.EventStore.SaveEvent ( controller.Event, )
                break;
        }
}
```

### <a name="creating-an-event-programmatically"></a>프로그래밍 방식으로 이벤트 만들기

코드에서 이벤트를 만들려면 `EKEvent` 클래스에서 *Fromstore* 팩터리 메서드를 사용 하 고 데이터를 설정 합니다.

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and do some exercise!";
newEvent.Notes = "This is your motivational event to go and do 30 minutes of exercise. Super important. Do this.";
```

이벤트를 저장할 일정을 설정 해야 하지만 기본 설정이 없는 경우에는 기본값을 사용할 수 있습니다.

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

이벤트를 저장 하려면 `EventStore`에서 *SaveEvent* 메서드를 호출 합니다.

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

저장 된 후에는 나중에 이벤트를 검색 하는 데 사용할 수 있는 고유 식별자를 사용 하 여 *EventIdentifier* 속성을 업데이트 합니다.

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier`은 문자열 형식의 GUID입니다.

### <a name="create-a-reminder-programmatically"></a>프로그래밍 방식으로 미리 알림 만들기

코드에서 미리 알림을 만드는 것은 일정 이벤트를 만드는 것과 매우 비슷합니다.

```csharp
EKReminder reminder = EKReminder.Create ( App.Current.EventStore );
reminder.Title = "Do something awesome!";
reminder.Calendar = App.Current.EventStore.DefaultCalendarForNewReminders;
```

저장 하려면 `EventStore`에서 *SaveReminder* 메서드를 호출 합니다.

```csharp
NSError e;
App.Current.EventStore.SaveReminder ( reminder, true, out e );
```

### <a name="retrieving-an-event-by-id"></a>ID로 이벤트 검색

ID로 이벤트를 검색 하려면 `EventStore`에서 *EventFromIdentifier* 메서드를 사용 하 여 이벤트에서 끌어온 `EventIdentifier`를 전달 합니다.

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

이벤트의 경우 두 가지 다른 식별자 속성이 있지만 `EventIdentifier`은이에 대해 작동 하는 유일한 속성입니다.

### <a name="retrieving-a-reminder-by-id"></a>ID로 미리 알림 검색

미리 알림을 검색 하려면 `EventStore` *Getcalendaritem* 메서드를 사용 하 고 *calendaritemidentifier*를 전달 합니다.

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

`GetCalendarItem`에서 `EKCalendarItem`를 반환 하므로 미리 알림 데이터에 액세스 하거나 나중에 해당 인스턴스를 `EKReminder`으로 사용 해야 하는 경우 `EKReminder`로 캐스팅 해야 합니다.

작성 당시에는 일정 이벤트에 대 한 `GetCalendarItem`를 사용 하지 마세요.

### <a name="deleting-an-event"></a>이벤트 삭제

일정 이벤트를 삭제 하려면 `EventStore`에서 *Removeevent* 를 호출 하 고 이벤트에 대 한 참조와 적절 한 `EKSpan`를 전달 합니다.

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

그러나 이벤트가 삭제 된 후에는 이벤트 참조가 `null`됩니다.

### <a name="deleting-a-reminder"></a>미리 알림 삭제

미리 알림을 삭제 하려면 `EventStore`에서 *RemoveReminder* 을 호출 하 고 미리 알림에 대 한 참조를 전달 합니다.

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

위의 코드에는 `GetCalendarItem`를 검색 하는 데 사용 되었기 때문에 `EKReminder`캐스팅이 있습니다.

### <a name="searching-for-events"></a>이벤트 검색

일정 이벤트를 검색 하려면 `EventStore`의 *PredicateForEvents* 메서드를 통해 *NSPredicate* 개체를 만들어야 합니다. `NSPredicate`는 iOS에서 일치 항목을 찾는 데 사용 하는 쿼리 데이터 개체입니다.

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

`NSPredicate`를 만들었으면 `EventStore`에서 *EventsMatching* 메서드를 사용 합니다.

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

쿼리가 동기 (차단) 되 고 쿼리에 따라 시간이 오래 걸릴 수 있으므로 새 스레드나 작업을 수행 하는 것이 좋습니다.

### <a name="searching-for-reminders"></a>미리 알림 검색

미리 알림 검색은 이벤트와 유사 합니다. 조건자가 필요 하지만 호출은 이미 비동기 이므로 스레드 차단에 대해 있으므로 걱정할 필요가 없습니다.

```csharp
// create our NSPredicate which we'll use for the query
NSPredicate query = App.Current.EventStore.PredicateForReminders ( null );

// execute the query
App.Current.EventStore.FetchReminders (
        query, ( EKReminder[] items ) => {
                // do someting with the items
        } );
```

## <a name="summary"></a>요약

이 문서는 EventKit 프레임 워크의 중요 한 부분과 가장 일반적인 몇 가지 작업에 대 한 개요를 제공 했습니다. 그러나 EventKit 프레임 워크는 매우 크고 강력 하며 여기에 소개 되지 않은 기능 (예: batch 업데이트, 경보 구성, 이벤트에 대 한 되풀이 구성, 일정 데이터베이스의 변경 내용 등록 및 수신 대기)을 포함 합니다. 지역 구분 등을 설정 합니다.  자세한 내용은 Apple의 [일정 및 미리 알림 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [달력 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/calendars)
- [iOS 6 소개](~/ios/platform/introduction-to-ios6/index.md)
- [일정 및 미리 알림 소개](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
