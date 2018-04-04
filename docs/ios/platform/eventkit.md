---
title: EventKit
description: 이 가이드에 액세스 하 고는 EventKit를 통해 노출 된 달력 데이터베이스에 저장 하는 달력, CalendarEvents, 및 미리 알림을 데이터로 작업 하는 방법에 대 한 개요를 제공 합니다. 주요 클래스와 여러 가지 EventKit 프레임 워크와 관련 된 일반적인 작업 뿐만 아니라 EventKit 프로그래밍에서 해당 역할을 포함 합니다.
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a8439586ac92f8139cf9341611125352c85706e5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="eventkit"></a>EventKit

_이 가이드에 액세스 하 고는 EventKit를 통해 노출 된 달력 데이터베이스에 저장 하는 달력, CalendarEvents, 및 미리 알림을 데이터로 작업 하는 방법에 대 한 개요를 제공 합니다. 주요 클래스와 여러 가지 EventKit 프레임 워크와 관련 된 일반적인 작업 뿐만 아니라 EventKit 프로그래밍에서 해당 역할을 포함 합니다._

iOS에는 두 개의 일정 관련 응용 프로그램이 기본 제공: 일정 응용 프로그램 및 미리 알림을 응용 프로그램입니다. 달력 응용 프로그램은 일정 데이터를 관리 하는 방법을 이해할 수 있을 정도로 간단 있지만 미리 알림 응용 프로그램은 덜 명확 합니다. 미리 알림 날짜가 지정의 측면에서 금액, 완료 하는 경우가 있으면 점이 실제로 등입니다. 달력 이벤트 또는 미리 알림, 라는 한 위치에서 든 관계 없이 iOS에서 모든 일정 데이터를 저장 하는 이와 같이 *달력 데이터베이스*합니다.

EventKit 프레임 워크에 액세스 하는 방법을 제공는 *달력*, *일정 이벤트*, 및 *미리 알림* 달력 데이터베이스에 저장 된 데이터입니다. 액세스 일정 및 일정에 이벤트가 사용할 수 있는 iOS 4, 이후 이었지만 미리 알림에 대 한 액세스가 iOS 6의에서 새로운 기능입니다.

이 가이드에서 다루지을 맞추려고:

-   **EventKit 기본 사항** –이 주요 클래스를 통해 EventKit의 기본적인 부분을 소개 합니다 및 용도 대해 설명 합니다. 이 섹션은 문서의 다음 부분을 수행 하는 작업량과 하기 전에 읽어보십시오. 
-   **일반 작업** – 일반적인 작업 섹션와 같은 일반적인 작업을 수행 하는 방법에 대 한 빠른 참조 하기 위한 용도가; 달력 열거, 생성, 저장 및 검색에 대 한 기본 제공 컨트롤러를 사용 하 여 뿐만 아니라 이벤트 및 미리 알림, 일정 만들기 및 일정 이벤트를 수정 합니다. 이 섹션으로 특정 태스크에 대 한 참조 하는 것에 앞뒤에 읽을 수 필요 합니다. 


이 가이드의 모든 작업 도우미 샘플 응용 프로그램에서 제공 됩니다.

 [![](eventkit-images/01.png "도우미 샘플 응용 프로그램 화면")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>요구 사항

EventKit iOS 4.0에서에서 도입 된 하지만 미리 알림 데이터에 대 한 액세스 iOS 6.0에서에서 도입 되었습니다. 따라서 일반 EventKit 개발을 수행 하려면 해야 적어도 대상 버전 4.0 및 6.0 미리 알림입니다.

또한 미리 알림 응용 프로그램에서 사용할 수 없으면는 미리 알림 데이터도 제공 됩니다, 먼저 추가 하지 않으면 즉 시뮬레이터. 또한 실제 장치에서 사용자에 게 액세스 요청만 표시 됩니다. 따라서 EventKit 개발 가장 장치에서 테스트 합니다.

## <a name="event-kit-basics"></a>이벤트 키트 기본 사항

EventKit를 사용할 때에 일반적인 클래스와 그 사용 방법을 살펴봤으므로 해야 합니다. 찾을 수 있습니다 이러한 클래스의 모든는 `EventKit` 및 `EventKitUI` (에 `EKEventEditController`).

### <a name="eventstore"></a>EventStore

*EventStore* EventKit에서 모든 작업을 수행 하는 데 필요한이 클래스는 EventKit에서 가장 중요 한 클래스입니다. 영구 저장소 또는 모든 EventKit 데이터에 대 한 데이터베이스 엔진의 생각할 수 있습니다. `EventStore` 달력와 일정 응용 프로그램에서 달력 이벤트 뿐만 아니라 식 미리 알림을 미리 알림 응용 프로그램에 액세스할 수 있습니다.

때문에 `EventStore` 되는 데이터베이스 엔진에 같은 수명이 긴 즉 것은 생성 되 고 응용 프로그램 인스턴스 수명 동안 최소한으로 제거 해야 합니다. 실제로 것이 좋습니다 하의 한 인스턴스를 만든 후는 `EventStore` 응용 프로그램에서 유지 주위에 참조 하는 응용 프로그램의 전체 수명 동안 다시 필요 하지는 않습니다 알 수 없는 경우. 단일 모든 호출 또한 이동 해야 `EventStore` 인스턴스. 이러한 이유로 Singleton 패턴이 사용 가능한 단일 인스턴스를 유지 하는 데 좋습니다.

#### <a name="creating-an-event-store"></a>이벤트 저장소를 만듭니다.

다음 코드에서는의 단일 인스턴스를 만들 수 있는 효율적인 방법을 `EventStore` 클래스 및 응용 프로그램 내에서 정적으로 사용할 수 있도록 합니다.

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

위의 코드에서 단일 패턴을 사용 하 여의 인스턴스를 인스턴스화하는 `EventStore` 응용 프로그램을 로드 합니다. `EventStore` 다음에서 액세스할 수 전체적으로 응용 프로그램 내에서 다음과 같이 합니다.

```csharp
App.Current.EventStore;
```

여기에 모든 예제를 참조 하므로이 패턴 사용의 `EventStore` 통해 `App.Current.EventStore`합니다.

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>일정 및 미리 알림 데이터에 대 한 액세스를 요청합니다.

EventStore 통해 모든 데이터 액세스를 허용 하기 전에 응용 프로그램 먼저 미리 알림 데이터를 필요한 어느 것에 따라 또는 일정 이벤트 데이터에 대 한 액세스를 요청 해야 합니다. 이 용이 하 게 하려면는 `EventStore` 라는 메서드가 노출 `RequestAccess` 는-호출 될 때-응용 프로그램은 어떤 에따라미리알림데이터또는일정데이터에대한액세스를요청하라는사용자에게경고보기에표시됩니다`EKEntityType`전달 됩니다. 경고 보기, 발생 하기 때문에 해당 비동기적 이며 호출과 변수로 전달 된 완료 처리기를 호출 합니다는 `NSAction` (또는 람다)를 두 개를 수신할 매개 변수, 부울의 액세스가 부여 여부 및 `NSError`이며 null이 아닌 경우 요청에 오류 정보를 포함 합니다. 예를 들어 코딩 된 다음 달력 이벤트 데이터와 표시 요청이 부여 되지 않은 경우 경고 보기에 대 한 액세스를 요청 합니다.

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

요청에 부여 된 것 기억 합니다 응용 프로그램이 장치에 설치 되어 있고 사용자에 게 경고 팝업 됩니다.
그러나 액세스 리소스를 부여 미리 알림 또는 일정 이벤트의 형식에만 제공 됩니다. 응용 프로그램 둘 다에 대 한 액세스를 필요한 경우 둘 다를 요청 해야 합니다.

사용 권한 저장 되므로 상대적으로 경제적인 위해 요청 될 때마다 항상 작업을 수행 하기 전에 액세스를 요청 하는 것이 좋습니다.

또한 완료 처리기를 (UI 아님) 별도 스레드에서 호출 했으므로 완료 처리기에서 UI에 대 한 업데이트를 호출 해야 통해 `InvokeOnMainThread`, 그렇지 않으면 예외가 throw 됩니다, 그리고 및 잡히지, 하는 경우 응용 프로그램의 작동이 중단 됩니다.

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType` 형식을 설명 하는 열거형은 `EventKit` 항목 또는 데이터입니다. 두 값이: `Event` 및 미리 알림입니다. 다양 한 메서드를 포함 하 여에 사용 되는 `EventStore.RequestAccess` 하기가 `EventKit` 어떤 종류의 데이터에 액세스 하거나 검색 합니다.

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar* 일정 이벤트 그룹 포함 하는 달력을 나타냅니다. 와 같은 다른 위치에서 많은에 저장할 수 달력에서 로컬로 *iCloud*의 타사 공급자 위치와 같은 *Exchange Server* 또는 *Google*등입니다. 여러 번 `EKCalendar` 에 알리는 데 사용 되 `EventKit` 를 이벤트를 찾을 위치 또는 저장 위치를 합니다.

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController* 에서 찾을 수는 `EventKitUI` 네임 스페이스 이며는 편집 하거나 일정 이벤트 만들기에 사용할 수 있는 기본 제공 된 컨트롤러입니다. 카메라 컨트롤러에서 기본 제공과 거의 `EKEventEditController` 은 UI를 표시 하 고 저장 처리에서 중요 한 역할을 수행 합니다.

### <a name="ekevent"></a>EKEvent

 *EKEvent* 일정 이벤트를 나타냅니다. 둘 다 `EKEvent` 및 `EKReminder` 에서 상속 `EKCalendarItem` 있고 필드와 같은 `Title`, `Notes`등입니다.

### <a name="ekreminder"></a>EKReminder

 *EKReminder* 미리 알림 항목을 나타냅니다.

### <a name="ekspan"></a>EKSpan

*EKSpan* 되풀이 수 있는 두 개의 값을 가지는 이벤트를 수정 하는 경우 이벤트의 범위를 설명 하는 열거형은: *크기가 줄어들게* 및 *FutureEvents*합니다. `ThisEvent` 변경 내용을만 참조 되는 계열에서 특정 이벤트에 적용 되지만 의미 `FutureEvents` 해당 이벤트 및 모든 이후 되풀이 영향을 줍니다.

## <a name="tasks"></a>작업

경우 사용 편의성에 대 한 EventKit 사용량 분할 된 다음 섹션에서 설명 하는 일반적인 작업에 있습니다.

### <a name="enumerate-calendars"></a>달력을 열거 합니다.

사용자가 장치에서 구성 하는 달력을 열거 하려면 호출 `GetCalendars` 에 `EventStore` 수신 하려는 달력 (미리 알림 또는 이벤트)의 형식을 전달 하 고:

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>추가 하거나 기본 제공 컨트롤러를 사용 하는 이벤트를 수정 합니다.

*EKEventEditViewController* 만들거나 일정 응용 프로그램을 사용 하는 경우 사용자에 게 제공 되는 동일한 UI 사용 하 여 이벤트를 편집 하려면 많은 중요 한 역할을 수행 합니다.

 [![](eventkit-images/02.png "일정 응용 프로그램을 사용 하는 경우 사용자에 게 제공 되는 UI")](eventkit-images/02.png#lightbox)

를 사용 하려면 메서드 내에서 선언 되는 경우 가비지 수집 가져올 하지 않는 있도록 클래스 수준 변수로 선언 해야 합니다.

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

유틸리티를 실행 하려면 다음: 인스턴스화할에 대 한 참조를 지정 된 `EventStore`를 연결 하는 *EKEventEditViewDelegate* , 대리자를 사용 하 여 표시할 `PresentViewController`:

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

필요에 따라 미리 이벤트를 지정 하려는 경우 (아래 그림과 같이), 새로운 이벤트를 만들 수 있습니다 또는 저장 된 이벤트를 검색할 수 있습니다.

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

UI를 미리 채울 하지 않으려는 경우 컨트롤러에서 이벤트 속성을 설정 해야 합니다.

```csharp
eventController.Event = newEvent;
```

기존 이벤트를 사용 하려면 참조는 *이벤트 ID 별로 검색* 나중에 섹션.

대리자를 재정의 해야는 `Completed` 사용자 대화 상자와 완료 되 면 컨트롤러에서 호출 하는 메서드:

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

필요에 따라에서 대리자를 확인할 수 있습니다는 *동작* 에 `Completed` 이벤트 및 다시 저장, 수정 하거나 취소 되 면 다른 작업을 수행 하는 메서드, etcetera:

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

사용 하 여 코드에서 이벤트를 만들 수는 *FromStore* 에서 팩터리 메서드는 `EKEvent` 클래스와 데이터에 설정할 수:

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

달력 이벤트, 저장을 설정 해야 하지만 기본 설정 없음를 설정한 경우에 기본값을 사용할 수 있습니다.

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

이벤트를 저장 하려면 호출는 *SaveEvent* 에서 메서드는 `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

저장 된 후의 *가져오거나 EventIdentifier* 속성 이벤트를 검색 하려면 나중에 사용할 수 있는 고유 식별자로 업데이트 됩니다.

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier` 형식이 지정 된 GUID는 문자열입니다.

### <a name="create-a-reminder-programmatically"></a>프로그래밍 방식으로 미리 알림 만들기

코드에서 미리 알림을 만드는 일정 이벤트를 만드는 것과 상당히 같습니다.

```csharp
EKReminder reminder = EKReminder.Create ( App.Current.EventStore );
reminder.Title = "Do something awesome!";
reminder.Calendar = App.Current.EventStore.DefaultCalendarForNewReminders;
```

을 저장 하려면 호출는 *SaveReminder* 에서 메서드는 `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveReminder ( reminder, true, out e );
```

### <a name="retrieving-an-event-by-id"></a>이벤트 ID로 검색

이벤트를 검색 하는 ID를 사용 하 여는 *EventFromIdentifier* 에서 메서드는 `EventStore` 전달는 `EventIdentifier` 이벤트에서 왔:

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

이벤트,이 다른 두 개의 식별자 속성이 있지만 `EventIdentifier` 이 작동 하는 유일한 스냅숏이어야 합니다.

### <a name="retrieving-a-reminder-by-id"></a>미리 알림 ID로 검색

미리 알림을 검색 하려면 사용는 *GetCalendarItem* 에서 메서드는 `EventStore` 전달 하는 것은 *CalendarItemIdentifier*:

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

때문에 `GetCalendarItem` 반환는 `EKCalendarItem`,으로 캐스팅 해야 `EKReminder` 미리 알림 데이터에 액세스 하거나로 인스턴스를 사용 해야 하는 경우는 `EKReminder` 나중 합니다.

사용 하지 않는 `GetCalendarItem` 달력 이벤트에 대 한 문서를 작성할 당시에서 작동 하지 않습니다.

### <a name="deleting-an-event"></a>이벤트 삭제

일정 이벤트를 삭제 하려면 호출 *RemoveEvent* 에 프로그램 `EventStore` 에서 이벤트와 적절 한에 대 한 참조 및 `EKSpan`:

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

하지만 참고 이벤트를 삭제 한 다음 이벤트 참조 됩니다 `null`합니다.

### <a name="deleting-a-reminder"></a>미리 알림 삭제

미리 알림을 삭제 하려면 호출 *RemoveReminder* 에 `EventStore` 미리 알림에 대 한 참조 및:

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

위의 코드에서로 캐스팅은 `EKReminder`때문에, `GetCalendarItem` 검색 하는 데 사용한

### <a name="searching-for-events"></a>이벤트에 대 한 검색

일정 이벤트를 검색 하려면 만들어야는 *NSPredicate* 통해 개체는 *PredicateForEvents* 에서 메서드는 `EventStore`합니다. `NSPredicate` 쿼리인 데이터 개체나 해당 iOS 일치 항목을 찾기 위해 사용 합니다.

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

작성 한 후는 `NSPredicate`를 사용 하 여는 *EventsMatching* 에서 메서드는 `EventStore`:

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

Note 쿼리 동기 (블로킹) 하며 새 스레드 또는 작업을 수행 하는 작업 스핀업 할 수 있도록 쿼리를에 따라 시간이 오래 걸릴 수 있습니다.

### <a name="searching-for-reminders"></a>미리 알림 검색

미리 알림 검색 하는 것은; 이벤트와 비슷하지만 조건자가 필요한 되기는 하지만 이미 비동기 스레드를 차단 하는 방법에 대 한 걱정 하지 않아도 되므로:

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

이 문서 EventKit 프레임 워크의 중요 한 부분 및 몇 가지 가장 일반적인 작업의 개요를 제공 했습니다. 그러나 EventKit 프레임 워크 매우 크고 강력 이며 포함 하지 않은 도입 된 기능에 여기에서 같은: 일괄 업데이트를 되풀이 이벤트를 구성, 등록 및 달력 데이터베이스에서 변경 내용을 수신 대기 하는 경보 시스템을 구성 합니다. GeoFences 등을 설정 합니다.  자세한 내용은 Apple의을 참조 하십시오. [일정 및 미리 알림을 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)합니다.


## <a name="related-links"></a>관련 링크

- [달력 (샘플)](https://developer.xamarin.com/samples/monotouch/Calendars/)
- [iOS 6 소개](~/ios/platform/introduction-to-ios6/index.md)
- [일정 및 미리 알림을 소개](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
