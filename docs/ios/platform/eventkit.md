---
title: Xamarin.iOS에서 EventKit
description: 이 문서는 EventKit 및 Xamarin.iOS에서 사용 하는 방법을 설명 합니다. 달력, 달력 이벤트 및 알림 살펴보고 EventKit, 등을 사용 하 여 프로그래밍할 때 일반적으로 사용 하는 클래스에 설명 합니다.
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: ea03c8b382e2de29bd20ab1d696d7abb7733e182
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120231"
---
# <a name="eventkit-in-xamarinios"></a>Xamarin.iOS에서 EventKit

iOS에는 두 가지 일정 관련 응용 프로그램이 기본 제공: 일정 응용 프로그램 및 미리 알림 응용 프로그램입니다. 일정 응용 프로그램의 일정 데이터를 관리 하는 방법을 이해 하기에 충분히 간단 이지만 미리 알림 응용 프로그램 덜 명확 합니다. 미리 알림 실제로 기한, 완료 되 면 있을 때의 측면에서 상호 연결 하는 날짜 수 등입니다. 달력 이벤트 또는 알림 라는 한 위치에서 든 iOS에서 모든 일정 데이터를 저장 하는 이와 같이 합니다 *달력 데이터베이스*합니다.

EventKit 프레임 워크에 액세스 하는 방법을 제공 합니다 *달력*, *일정 이벤트*, 및 *미리 알림* 달력 데이터베이스를 저장 하는 데이터입니다. 액세스 일정 및 일정에 이벤트 사용 가능한 iOS 4 이후 되었지만 미리 알림 액세스가 iOS 6의에서 새로운 기능입니다.

이 가이드에서 설명 하겠습니다.

-   **기본 사항 EventKit** –이 주 클래스를 통해 EventKit의 기본적인 부분 하 고 용도 대해 설명 합니다. 이 섹션은 문서의 다음 부분을 처리 하기 전에 읽어야 합니다. 
-   **일반적인** – 일반적인 [작업] 섹션와 같은 일반적인 작업을 수행 하는 방법에 대 한 빠른 참조 하려는; 달력을 열거, 만들기, 저장 및 검색 일정에 대 한 기본 제공 컨트롤러를 사용 하 여 뿐만 아니라 이벤트 및 알림 만들기 및 일정 이벤트를 수정 합니다. 이 섹션에서는 특정 작업에 대 한 참조일 수 하려는 것 처럼 앞-뒤 읽기 수 필요 합니다. 


이 가이드의 모든 작업은 동반 샘플 응용 프로그램에서 사용할 수 있습니다.

 [![](eventkit-images/01.png "동반 샘플 응용 프로그램 화면")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>요구 사항

EventKit iOS 4.0에에서 도입 합니다. 하지만 미리 알림 데이터에 대 한 액세스는 iOS 6.0에서에서 도입 되었습니다. 따라서 일반 EventKit 개발을 위해 해야 적어도 대상 버전 4.0 및 6.0 미리 알림입니다.

또한 미리 알림 응용 프로그램에는 미리 알림 데이터도 제공 되지를 먼저 추가 하지 않는 한 의미는 시뮬레이터는 제공 되지 않습니다. 또한 실제 장치에서 사용자에 게 액세스 요청만 표시 됩니다. 이와 같이 EventKit 개발은 가장 장치에서 테스트 합니다.

## <a name="event-kit-basics"></a>이벤트 키트 기본 사항

EventKit에서 작업할 때 공용 클래스 및 용도 파악 하도록 반드시 합니다. 찾을 수 있습니다 이러한 클래스의 모든 합니다 `EventKit` 하 고 `EventKitUI` (에 대 한는 `EKEventEditController`).

### <a name="eventstore"></a>EventStore

합니다 *EventStore* EventKit에서 모든 작업을 수행할 필요 하므로 클래스는 EventKit에서 가장 중요 한 클래스입니다. 영구 저장소 또는 모든 EventKit 데이터에 대 한 데이터베이스 엔진의 생각할 수 있습니다. `EventStore` 일정 및 일정 응용 프로그램에서 달력 이벤트와 미리 알림 응용 프로그램에서 미리 알림을에 액세스할 수 있습니다.

때문에 `EventStore` 은 데이터베이스 엔진, 같은 수명이 즉 것은 생성 되 고 응용 프로그램 인스턴스의 수명 동안 최소한으로 제거 해야 합니다. 실제로 것이 좋습니다는 인스턴스를 만든 후는 `EventStore` 응용 프로그램에서 유지 관련 참조 하는 응용 프로그램의 전체 수명에 대 한 더 이상 필요 하지 않습니다 알 수 없는 경우. 모든 호출이 단일으로 이동 해야는 또한 `EventStore` 인스턴스. 이러한 이유로 사용할 수 있는 단일 인스턴스를 유지 하는 것에 대 한 단일 항목 패턴 것이 좋습니다.

#### <a name="creating-an-event-store"></a>이벤트 저장소 만들기

다음 코드에서는 효율적으로의 단일 인스턴스를 만들고는 `EventStore` 클래스 및 응용 프로그램 내에서 정적으로 사용할 수 있도록 합니다.

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

위 코드의 인스턴스를 인스턴스화하지 Singleton 패턴을 사용 하는 `EventStore` 응용 프로그램이 로드 하는 경우. `EventStore` 액세스할 수 있습니다 전 세계에서 응용 프로그램 내에서 다음과 같이 합니다.

```csharp
App.Current.EventStore;
```

여기의 모든 예제를 참조 하므로이 패턴을 사용 합니다 `EventStore` 를 통해 `App.Current.EventStore`합니다.

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>일정 및 미리 알림 데이터에 대 한 액세스를 요청합니다.

EventStore 통해 모든 데이터에 액세스 하도록 허용 하기, 전에 응용 프로그램 먼저 미리 알림 데이터를 해야 하는 것에 따라 또는 일정 이벤트 데이터에 대 한 액세스를 요청 해야 합니다. 이 용이 하 게 합니다 `EventStore` 라는 메서드를 노출 `RequestAccess` 는-호출 될 때-응용 프로그램이 어떤 에따라미리알림데이터또는일정데이터에대한액세스를요청하고있는지여부를알려사용자에게경고보기를표시하는`EKEntityType`에 전달 됩니다. 경고 보기, 발생 하기 때문에 호출이 비동기 이며 변수로 전달 하는 완료 처리기를 호출 합니다는 `NSAction` (또는 람다)를 수신 하는 두 개의 매개 변수, 액세스 권한 부여 되었는지 여부의 부울 및 `NSError`는 null이 아닌 경우 요청에서 오류 정보를 포함 합니다. 예를 들어 코딩 된 다음 경고 보기 요청 부여 되지 않은 경우 달력 이벤트 데이터 및 표시에 대 한 액세스를 요청 됩니다.

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

요청이 승인 되 면이 저장 됩니다 응용 프로그램을 장치에 설치 되어 있고 사용자에 게 경고 팝업 됩니다.
그러나 액세스 리소스를 부여 하는 미리 알림 또는 일정 이벤트의 형식에만 제공 됩니다. 둘 다에 액세스 해야 하는 응용 프로그램을 하는 경우 둘 다를 요청 해야 합니다.

있기 때문에 권한, 기억 하기 위해 요청 될 때마다 항상 작업을 수행 하기 전에 액세스를 요청 하는 것이 좋습니다 비교적 저렴 합니다.

또한 개별 (비 UI) 스레드에서 완료 처리기가 호출 하기 때문에 UI 완료 처리기에 대 한 업데이트를 호출 해야 통해 `InvokeOnMainThread`, 그렇지 않으면 예외가 throw 됩니다, 그리고 및 잡히지, 하는 경우 응용 프로그램의 작동이 중단 됩니다.

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType` 형식을 설명 하는 열거형은 `EventKit` 항목 또는 데이터입니다. 두 값을 포함 합니다. `Event` 및 미리 알림입니다. 다양 한 메서드를 포함 하 되 `EventStore.RequestAccess` 하기가 `EventKit` 어떤 종류의 데이터에 대 한 액세스를 가져오거나 검색 합니다.

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar* 달력 이벤트 그룹을 포함 하는 달력을 나타냅니다. 와 같은 많은 다른 부분에 저장할 수 달력에서 로컬로 *iCloud*의 타사 공급자 위치와 같은 *Exchange Server* 또는 *Google*등입니다. 여러 번 `EKCalendar` 알리는 데 `EventKit` 이벤트를 찾으려는 위치 또는 저장할 수 있는 위치입니다.

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController* 에서 찾을 수 있습니다는 `EventKitUI` 네임 스페이스 및 일정 이벤트를 만들거나 편집 하려면 사용할 수 있는 기본 제공 컨트롤러입니다. 마찬가지로 기본 제공 카메라 컨트롤러에서 `EKEventEditController` UI를 표시 하 고 저장을 처리에서 어려운 않습니다.

### <a name="ekevent"></a>EKEvent

 *EKEvent* 일정 이벤트를 나타냅니다. 둘 다 `EKEvent` 하 고 `EKReminder` 에서 상속 `EKCalendarItem` 있고 필드와 같은 `Title`, `Notes`등입니다.

### <a name="ekreminder"></a>EKReminder

 *EKReminder* 미리 알림 항목을 나타냅니다.

### <a name="ekspan"></a>EKSpan

*EKSpan* 이벤트 되풀이 수 있고 두 값을 수정 하는 경우 이벤트의 범위를 설명 하는 열거형은: *크기가 줄어들게* 하 고 *FutureEvents*합니다. `ThisEvent` 반면, 참조 되는 계열에서 특정 이벤트에 변경만 수행 되는 의미 `FutureEvents` 이벤트 및 모든 이후 되풀이 간격에 적용 됩니다.

## <a name="tasks"></a>작업

사용 편의성을 위해 EventKit 사용량에 분할 된 다음 섹션에서 설명 하는 일반적인 작업을 합니다.

### <a name="enumerate-calendars"></a>달력을 열거 합니다.

장치에서 사용자가 구성한 달력을 열거 하려면 호출 `GetCalendars` 에 `EventStore` 수신 하려는 달력 (알림 또는 이벤트)의 형식을 전달 하 고:

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>추가 하거나 기본 컨트롤러를 사용 하 여 이벤트를 수정 합니다.

합니다 *EKEventEditViewController* 만들거나 일정 응용 프로그램을 사용 하는 경우 사용자에 게 표시 되는 동일한 UI 사용 하 여 이벤트를 편집 하려면 많은 주요 작업을 수행 합니다.

 [![](eventkit-images/02.png "일정 응용 프로그램을 사용 하는 경우 사용자에 게 표시 되는 UI")](eventkit-images/02.png#lightbox)

를 사용 하려면이 오지 가비지 수집 메서드 내에서 선언 되 면 되도록 클래스 수준 변수 선언 해야 합니다.

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

유틸리티를 실행 하려면 다음: 인스턴스화할에 대 한 참조를 제공 합니다 `EventStore`를 연결 하는 *EKEventEditViewDelegate* 에 대리자를 사용 하 여 표시 한 다음 `PresentViewController`:

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

필요에 따라 이벤트를 미리 채울 하려는 경우 (아래 그림과 같이), 새로운 이벤트를 만들 수 있습니다 또는 저장 된 이벤트를 검색할 수 있습니다.

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

수행 하려는 경우 UI를 미리 채우기, 컨트롤러에서 이벤트 속성을 설정 해야 합니다.

```csharp
eventController.Event = newEvent;
```

기존 이벤트를 사용 하려면 참조는 *이벤트 ID 별로 검색* 나중에 섹션입니다.

대리자를 재정의 해야 합니다 `Completed` 사용자 대화 상자를 사용 하 여 완료 되 면 컨트롤러에서 호출 하는 메서드:

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

필요에 따라 대리자를 확인할 수 있습니다 합니다 *동작* 에 `Completed` 이벤트 및 다시 저장, 수정 또는 취소 된 경우 다른 작업을 수행할 방법, 항목:

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

### <a name="creating-an-event-programmatically"></a>이벤트를 프로그래밍 방식으로 만들기

코드에서 이벤트를 만들려면 사용 합니다 *FromStore* 팩터리 메서드를는 `EKEvent` 클래스 및 모든 데이터를 설정:

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

달력 이벤트에서 저장을 설정 해야 하지만 기본값을 사용할 수 없는 기본 설정에 있는 경우:

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

이벤트를 저장 하려면 호출을 *SaveEvent* 메서드를 `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

저장 되 고 나면 합니다 *가져오거나 EventIdentifier* 속성은 이벤트를 검색 하려면 나중에 사용할 수 있는 고유 식별자를 사용 하 여 업데이트 됩니다.

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier` 문자열 형식의 GUID입니다.

### <a name="create-a-reminder-programmatically"></a>프로그래밍 방식으로 미리 알림 만들기

코드에서 미리 알림을 만드는 것은 일정 이벤트를 만들 때 훨씬 동일 합니다.

```csharp
EKReminder reminder = EKReminder.Create ( App.Current.EventStore );
reminder.Title = "Do something awesome!";
reminder.Calendar = App.Current.EventStore.DefaultCalendarForNewReminders;
```

에 저장 하려면 호출을 *SaveReminder* 메서드를 `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveReminder ( reminder, true, out e );
```

### <a name="retrieving-an-event-by-id"></a>ID 별 이벤트를 검색합니다.

ID를 사용 하 여 이벤트를 검색 하는 *EventFromIdentifier* 메서드를 `EventStore` 전달 합니다 `EventIdentifier` 이벤트에서 가져와:

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

이벤트에 대 한 방법이 두 가지 다른 식별자 속성이 있지만 `EventIdentifier` 은이 작동 하는 유일한 알고리즘입니다.

### <a name="retrieving-a-reminder-by-id"></a>미리 알림 ID로 검색

미리 알림을 검색 하려면 사용 합니다 *GetCalendarItem* 메서드는 `EventStore` 전달를 *CalendarItemIdentifier*:

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

때문에 `GetCalendarItem` 반환을 `EKCalendarItem`,으로 캐스팅 되어야 합니다 `EKReminder` 미리 알림 데이터에 액세스 하거나로 인스턴스를 사용 해야 하는 경우는 `EKReminder` 나중입니다.

사용 하지 않는 `GetCalendarItem` 달력 이벤트에 대 한 문서 작성 당시에 따라 작동 하지 않습니다.

### <a name="deleting-an-event"></a>이벤트 삭제

일정 이벤트를 삭제 하려면 호출 *RemoveEvent* 에 사용자 `EventStore` 이벤트를 지정 하 고 적절 한 참조를 전달 하 고 `EKSpan`:

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

하지만 참고 삭제 된 이벤트 이벤트 참조 됩니다 `null`합니다.

### <a name="deleting-a-reminder"></a>미리 알림 삭제

미리 알림을 삭제 하려면 호출 *RemoveReminder* 에 `EventStore` 알리는에 대 한 참조를 전달:

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

위의 코드에서 캐스트를 됩니다 `EKReminder`이므로 `GetCalendarItem` 검색 하는 데 사용 된

### <a name="searching-for-events"></a>이벤트에 대 한 검색

일정 이벤트를 검색 하려면 만들어야 합니다는 *NSPredicate* 를 통해 개체를 *PredicateForEvents* 메서드는 `EventStore`. `NSPredicate` 쿼리는 일치 항목을 찾기 위해 사용 하는 iOS 개체:

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

만든 후는 `NSPredicate`를 사용 하 여는 *EventsMatching* 메서드를 `EventStore`:

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

쿼리 동기 (블로킹) 되며 새 스레드 또는 작업을 수행 하는 작업을 스핀업 할 수 있도록 쿼리에 따라 시간이 오래 걸릴 수 있습니다 note 합니다.

### <a name="searching-for-reminders"></a>미리 알림을 검색

미리 알림을 검색 하는 것은 비슷한 이벤트 조건자에 필요 하지만 호출 되므로 이미 비동기 스레드를 차단 하는 방법에 대 한 걱정할 필요가 없습니다.

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

이 문서에서는 EventKit framework의 중요 한 부분 및 다양 한 가장 일반적인 작업의 개요를 제공 했습니다. 그러나 EventKit 프레임 워크를 매우 크고 강력한 이며 포함 하지 않은 도입 된 기능에 여기에서 같은: 되풀이 이벤트를 등록 하 고 달력 데이터베이스에서 변경 내용을 수신 하 고 구성 하는 경보를 구성 하는 업데이트를 일괄 처리 지역 구분 및 기타 정보를 설정 합니다.  자세한 내용은 Apple의을 참조 하세요 [일정 및 미리 알림 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)합니다.


## <a name="related-links"></a>관련 링크

- [일정 (샘플)](https://developer.xamarin.com/samples/monotouch/Calendars/)
- [iOS 6 소개](~/ios/platform/introduction-to-ios6/index.md)
- [일정 및 미리 알림 소개](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
