---
title: watchOS에서 Xamarin 백그라운드 작업
description: 이 문서에는 백그라운드 태스크를 사용 하 여 watchos에서 Xamarin 백그라운드 작업의 형식에 살펴보겠습니다, 리소스를 사용 하 여 백그라운드 작업, 예약, 모범 사례 등을 구현 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/13/2017
ms.openlocfilehash: e28ba19fdc972b962f0dcd2757f1ba9087ac5c27
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831773"
---
# <a name="watchos-background-tasks-in-xamarin"></a>watchOS에서 Xamarin 백그라운드 작업

Watchos 3 가지는 watch 앱 수 해당 정보가 최신 상태로 유지 하는 세 가지 방법으로 

- 몇 가지 새 백그라운드 작업 중 하나를 사용 합니다. 
- (업데이트 하는 추가 시간 제공)에 해당 복잡성 중 하나 필요 합니다. 
- 새 도킹을 앱에 사용자 pin 필요 (여기서는 메모리에 유지 되 고 자주 업데이트) 합니다. 

## <a name="keeping-an-app-up-to-date"></a>앱을 최신 상태로 유지

모든 현재 및 업데이트 된 개발자 watchOS 앱의 데이터 및 사용자 인터페이스를 유지할 수는 방법에 설명 하기 전에이 섹션은 살펴보겠습니다 일반적인 집합을 사용 하 여 패턴과 사용자가 자신의 iPhone 및 하루 중에 따라 해당 Apple Watch 간에 이동 될 수 있습니다 하는 방법  시간 및 작업을 현재 수행 하 고 (예: 구동).

다음 예제를 참조하세요.

[![](background-tasks-images/update00.png "사용자가 하루 종일 해당 Apple Watch 및 iPhone 간에 이동 될 수 있습니다 하는 방법")](background-tasks-images/update00.png#lightbox)

1. 아침에 커피를 한 줄에서 대기 하는 동안 사용자가 몇 분 동안 자신의 iPhone에 최신 뉴스를 탐색합니다.
2. 커피숍에서 나가기 전에 신속 하 게 체크 인할는 복잡 한 해당 watch 화면에서를 사용 하 여 날씨 합니다.
3. 점심을 하기 전에 사용 하 여 맵 앱 iPhone의 주변 식당을 찾고 주소록에 클라이언트를 충족 하는 예약.
4. 음식점 출장 중 해당 Apple Watch 빠른 보기를 사용 하 여 알림을 받게, 점심 약속 해당 실행이 지연 알고 있는 합니다.
5. 저녁을 사용 하 여 맵 앱 iPhone의 트래픽 홈 구동 하기 전에 확인.
6. 방식에 일부 우유 선택 하도록 요청 하는 Apple Watch iMessage 알림의 받을 때 집 하 고 "확인" 응답을 보낼 빠른 응답 기능을 사용 합니다.

"눈" 때문에 (3 초 미만) 하는 방법을 사용자가 있습니다 일반적으로, Apple Watch 앱을 사용 하려는 특성 충분치 않은 응용 프로그램에서 원하는 정보를 가져오고 사용자에 게 표시 하기 전에 해당 UI를 업데이트 합니다.

새 Apple Api가 포함 3 watchOS 앱에 대 한 예약 수를 _백그라운드 새로 고침_ 원하는 정보가 준비 있어야 사용자가 요청할 및. 위에서 설명한 날씨 Complication의 예제를 수행 합니다.

[![](background-tasks-images/update01.png "날씨 Complication의 예")](background-tasks-images/update01.png#lightbox)

1. 특정 시간에 시스템에서 해제 하려면 앱 일정 합니다. 
2. 앱은 업데이트를 생성 해야 하는 정보를 가져옵니다.
3. 앱을 새 데이터를 반영 하도록 해당 사용자 인터페이스를 다시 생성 합니다.
4. 사용자 앱의 Complication에서 glances, 최신 정보를 사용자에 게 업데이트 될 때까지 기다리지 않고 있습니다.

위에 표시 된 것 처럼 watchOS 시스템 사용할 수 있는 매우 제한 된 풀에 있는 하나 이상의 작업을 사용 하 여 앱 실행:

[![](background-tasks-images/update02.png "WatchOS 시스템에 하나 이상의 작업을 사용 하 여 앱 실행")](background-tasks-images/update02.png#lightbox)

Apple 앱 자체 업데이트 프로세스가 완료 될 때까지 일단 보유 하 여이 작업의 최대한 (앱에 사용 하는 제한 된 리소스 이므로)을 제안 합니다.

시스템이 제공 하는 이러한 작업은 새 호출 하 여 `HandleBackgroundTasks` 메서드는 `WKExtensionDelegate` 위임 합니다. 예:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeyWatch.MonkeySeeExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Constructors
        public ExtensionDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request here
            ...
        }
        #endregion
    }
}
```

앱 지정된 된 작업이 완료 되 면 해당 반환 시스템에 완료 된 것을 표시 하 여:

[![](background-tasks-images/update03.png "작업이 완료 된 것을 표시 하 여 시스템에 반환 합니다.")](background-tasks-images/update03.png#lightbox)

<a name="New-Background-Tasks" />

## <a name="new-background-tasks"></a>새 백그라운드 작업

watchOS 3 앱 같은 앱을 열기 전에 사용자가 콘텐츠 있는지 확인 합니다. 해당 정보를 업데이트 하는 데 사용할 수 있는 몇 가지 백그라운드 작업을 제공 합니다.

- **백그라운드 앱 새로 고침** - [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) 작업 앱이 백그라운드에서 해당 상태를 업데이트할 수 있게 합니다. 일반적으로 여기에 사용 하 여 인터넷에서 새 콘텐츠를 다운로드 하는 등 다른 작업을 [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession)합니다.
- **스냅숏 새로 고침 백그라운드** - [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) 작업 시스템은 도킹 스테이션을 채우는 데 사용할 스냅숏을 전에 해당 콘텐츠 및 UI를 업데이트 하려면 앱을 사용 합니다.
- **조사식 연결 백그라운드** - [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) 쌍을 이루는 iPhone에서 백그라운드 데이터를 받을 때 앱에 대 한 작업을 시작 합니다.
- **URL 세션 백그라운드** - [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) 백그라운드 전송 하는 권한 부여 필요 하거나 (성공적으로 또는 오류)를 완료 하는 경우 앱에 대 한 작업을 시작 합니다.

이러한 작업은 아래 섹션에서 자세히 설명 합니다.

<a name="WKApplicationRefreshBackgroundTask" />

### <a name="wkapplicationrefreshbackgroundtask"></a>WKApplicationRefreshBackgroundTask

`WKApplicationRefreshBackgroundTask` 절전 나중에 앱을 예약할 수 있는 일반 작업은:

[![](background-tasks-images/update04.png "나중에 해제는 WKApplicationRefreshBackgroundTask")](background-tasks-images/update04.png#lightbox)

앱 작업의 런타임 내에서 모든 종류의 업데이트와 같은 로컬 처리 Complication 타임 라인을 수행 하거나 사용 하 여 필요한 일부 데이터를 인출 된 `NSUrlSession`합니다.


<a name="WKURLSessionRefreshBackgroundTask" />

### <a name="wkurlsessionrefreshbackgroundtask"></a>WKURLSessionRefreshBackgroundTask

시스템 전송할는 `WKURLSessionRefreshBackgroundTask` 데이터를 다운로드 하 여 앱에서 처리할 준비가 완료 경우:

[![](background-tasks-images/update05.png "데이터 다운로드가 완료 되 면 WKURLSessionRefreshBackgroundTask")](background-tasks-images/update05.png#lightbox)

앱은 백그라운드에서 데이터를 다운로드 하는 동안 실행를 남겨둘 수 없습니다. 대신 데이터에 대 한 요청을 예약 하는 앱을 일시 중지 되 고 시스템 다운로드가 완료 되 면 앱만 reawakening 데이터의 다운로드를 처리 합니다.

<a name="WKSnapshotRefreshBackgroundTask" />

### <a name="wksnapshotrefreshbackgroundtask"></a>WKSnapshotRefreshBackgroundTask

WatchOS 3에에서 Apple 도킹 스테이션 사용자가 즐겨 찾는 앱을 고정 하 고 신속 하 게 액세스할 수 있는 추가 되었습니다. 사용자의 Apple Watch 쪽 단추를 누르면 고정 된 앱 스냅숏 갤러리에 표시 됩니다. 사용자는 왼쪽 또는 오른쪽 원하는 앱을 찾은 다음 실행 중인 앱의 인터페이스를 사용 하 여 스냅숏을 대체 시작 앱 탭에서 살짝 밀 수입니다.

[![](background-tasks-images/update06.png "실행 중인 앱 인터페이스를 사용 하 여 스냅숏을 대체")](background-tasks-images/update06.png#lightbox)

시스템에 앱 UI의 스냅숏을 정기적으로 사용 (전송 하 여를 `WKSnapshotRefreshBackgroundTask`) 및 해당 스냅숏을 사용 하 여 도킹을 채웁니다. watchOS 앱이이 스냅숏을 수행 하기 전에 해당 콘텐츠 및 UI를 업데이트할 수 있는 기회를 제공 합니다.

앱에 대 한 미리 보기 및 시작 이미지 작동 하므로 스냅숏은 watchOS 3에서에서 매우 중요 합니다. 사용자 Dock의 앱에 도달, 하는 경우 전체 화면으로 확장 하 고, 전경을 입력 하 고 스냅숏이 최신 상태로 유지할 수는 반드시 실행 시작:

[![](background-tasks-images/update07.png "전체 화면으로 확장 사용자 Dock의 앱에 도달, 하는 경우")](background-tasks-images/update07.png#lightbox)

시스템 다시 발급할는 `WKSnapshotRefreshBackgroundTask` 스냅숏을 생성 하는 앱을 준비할 수 있습니다 (데이터 및 UI 업데이트) 하기 전에:

[![](background-tasks-images/update08.png "스냅숏을 생성 하기 전에 데이터 및 UI를 업데이트 하 여 앱을 준비할 수 있습니다.")](background-tasks-images/update08.png#lightbox)

앱 표시 하는 경우는 `WKSnapshotRefreshBackgroundTask` 완료 되 면 시스템은 자동으로 스냅숏을 앱 UI의 합니다.

> [!IMPORTANT]
> 항상 예약 해야는 `WKSnapshotRefreshBackgroundTask` 앱 새 데이터를 수신 하는 사용자 인터페이스를 업데이트 하거나 사용자 수정된 된 정보를 표시 되지 것입니다.




또한 사용자 앱에서 알림을 받습니다 전경으로 앱을 가져올 수 탭을 스냅숏도 시작 화면으로 작용 하 고 있으므로 최신 상태로 유지할 수 해야 합니다.

[![](background-tasks-images/update09.png "사용자 앱에서 알림을 수신 하 고가 전경으로 앱을 가져올 수")](background-tasks-images/update09.png#lightbox)

사용자가 watchOS 앱 상호 작용 하므로 1 시간 이상 되었으면, 해당 기본 상태로 돌아갈 수 됩니다. 기본 상태 다른 앱에 다양 한 작업을 의미할 수 있습니다 및 앱의 디자인에 따라 없는 경우 기본 상태에 있습니다.

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask" />

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>WKWatchConnectivityRefreshBackgroundTask

WatchOS 3에서에서 Apple watch 연결와 통합 백그라운드 새로 고침 API를 통해 새 `WKWatchConnectivityRefreshBackgroundTask`합니다. 이 새로운 기능을 사용 하 여 iPhone 앱 새 데이터를 제공할 수 watch 앱 상응 watchOS 앱은 백그라운드에서 실행 되는 동안:

[![](background-tasks-images/update10.png "IPhone 앱을 새 데이터를 제공할 수 watch 앱 상응 watchOS 앱은 백그라운드에서 실행 되는 동안")](background-tasks-images/update10.png#lightbox)

푸시 Complication를 시작할 앱 컨텍스트를 파일로 보내거나 iPhone 앱에서 사용자 정보를 업데이트 하는 절전 모드 해제 백그라운드에서 Apple Watch 앱.

Watch 앱을 통해 경우 절전 모드는 `WKWatchConnectivityRefreshBackgroundTask` 표준 API 메서드를 사용 하 여 iPhone 앱에서 데이터를 수신 하도록 해야 합니다.

[![](background-tasks-images/update11.png "WKWatchConnectivityRefreshBackgroundTask 데이터 흐름")](background-tasks-images/update11.png#lightbox)

1. 세션이 활성화를 확인 합니다.
2. 새 모니터링 `HasContentPending` 속성 값이 만큼 `true`, 앱에 아직 데이터를 처리 합니다. 모든 데이터를 처리 완료 될 때까지 이전에 앱 작업에 hold 해야 합니다.
3. 데이터가 더 이상 처리할 수 있을 때 (`HasContentPending = false`), 시스템에 반환할 완료 작업을 표시 합니다. 이렇게가 소진 되어 앱의 할당 된 백그라운드 런타임 충돌 보고를 생성 합니다.

<a name="The-Background-API-Lifecycle" />

## <a name="the-background-api-lifecycle"></a>백그라운드 API 수명 주기

에 함께 배치에서 모든 부분이 새 백그라운드 태스크 API의 일반적인 상호 작용 집합을 다음과 같이 보입니다.

[![](background-tasks-images/update12.png "백그라운드 API 수명 주기")](background-tasks-images/update12.png#lightbox)

1. 첫째, watchOS 앱을 백그라운드 작업을 나중에 특정 시점으로 중지 되어 수를 예약 합니다.
2. 앱을 시스템에서 해제 하 고 작업을 전송 합니다.
3. 앱 처리 작업이 필요 했습니다 어떤 작업을 완료 합니다.
4. 결과적으로 작업 처리를 앱에 필요할 자세한 배경을 사용 하 여 콘텐츠를 다운로드 하는 등 나중에 더 많은 작업을 완료 하는 작업을 예약 하려면는 `NSUrlSession`합니다.
5. 앱 작업 완료를 표시 하 고 시스템에 반환 합니다.

<a name="Using-Resources-Responsibly" />

## <a name="using-resources-responsibly"></a>리소스를 책임감 있게 사용

WatchOS 앱을 작동 하는지 책임감이 에코 시스템 내에서 해당 시스템의 공유 리소스가 소모를 제한 하 여 중요 한 것입니다.

다음 시나리오를 살펴보겠습니다.

[![](background-tasks-images/update13.png "WatchOS 앱을 해당 시스템의 공유 리소스가 소모를 제한합니다.")](background-tasks-images/update13.png#lightbox)

1. 사용자가 오후 1 시에 watchOS 앱을 시작 합니다.
2. 앱 절전 오후 2 시에 1 시간에 새 콘텐츠를 다운로드 하는 작업을 예약 합니다.
3. 오후 1 시 50 분 사용자에 해당 데이터와 UI를 업데이트할 수 있도록 앱을 다시 열립니다.
4. 10 분 후에 다시 작업 wake 앱 대신, 앱에는 나중에 오후 2 시 50 분에서 1 시간 동안 실행 되도록 작업을 다시 예약 해야 합니다.

모든 앱은 다른, 시스템 리소스를 절약 위의 표시 된 것 처럼 사용 패턴을 찾는 Apple에서 제안 합니다.

<a name="Implementing-Background-Tasks" />

## <a name="implementing-background-tasks"></a>백그라운드 작업 구현

예제를 위해이 문서에서는 사용자에 게 축구 점수를 보고 하는 가짜 MonkeySoccer 스포츠 앱을 사용 합니다. 

다음 일반적인 사용 시나리오를 살펴보겠습니다.

[![](background-tasks-images/update14.png "일반적인 사용 시나리오")](background-tasks-images/update14.png#lightbox)

사용자의 즐겨찾기 축구 팀에서 앱 사용자가 점수를 정기적으로 확인 되어야 하 고 30 분에 대 한 업데이트 간격으로 결정 오후 7 시에서 오후 9 시에 일치 하는 큰 항목을 재생 합니다.

1. 사용자가 앱을 열 및 30 분 후 백그라운드 업데이트에 대 한 작업을 예약 합니다. 백그라운드 API를 통해 하나의 형식의 백그라운드 작업을 지정된 된 시간에 실행 합니다.
2. 앱 작업을 받고 해당 데이터 및 UI를 업데이트 한 다음 일정 다른 백그라운드 작업이 30 분 후 합니다. 개발자가 다른 백그라운드 작업을 예약 하거나 앱 됩니다 하지 수 다시 이더라도 추가 업데이트를 가져오려고 하는 것입니다.
3. 마찬가지로 앱 작업 및 해당 데이터를 업데이트, UI를 업데이트 받고 다른 백그라운드 작업이 30 분 후 예약 합니다.
4. 동일한 프로세스를 반복 합니다.
5. 마지막 백그라운드 작업은 수신 하 고 앱 다시 해당 데이터와 UI를 업데이트 합니다. 때문에이 최종 점수를 새 배경 새로 고침을 예약 하지 않습니다. 

<a name="Scheduling-for-Background-Update" />

## <a name="scheduling-for-background-update"></a>백그라운드 업데이트에 대 한 일정

위의 시나리오를 MonkeySoccer 앱 수를 사용 하 여 다음 코드는 백그라운드 업데이트에 대 한 예약:

```csharp
private void ScheduleNextBackgroundUpdate ()
{
    // Create a fire date 30 minutes into the future
    var fireDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

    // Create 
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("LastActiveDate"), NSDate.FromTimeIntervalSinceNow(0));
    userInfo.Add (new NSString ("Reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleBackgroundRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

새로 만듭니다 `NSDate` 30 분 이후 when 앱 이더라도 수 하려는 만들고를 `NSMutableDictionary` 요청된 작업의 세부 정보를 포함 하 합니다. 합니다 `ScheduleBackgroundRefresh` 메서드는 `SharedExtension` 태스크 예약을 요청 하는 데 사용 됩니다.

이 반환 됩니다는 `NSError` 요청된 작업을 예약할 수 없는 경우.

<a name="Processing-the-Update" />

## <a name="processing-the-update"></a>업데이트 처리

다음으로 살펴볼 점수를 업데이트 하는 데 필요한 단계를 보여 주는 5 분 창에서:

[![](background-tasks-images/update15.png "점수를 업데이트 하는 데 필요한 단계를 보여 주는 5 분 창")](background-tasks-images/update15.png#lightbox)

1. 7시 30분: 02 PM에 앱을 시스템에 의해 활성화 하 고 업데이트 백그라운드 작업을 지정 합니다. 첫 번째 우선 순위는 서버에서 최신 점수를 가져오는 것입니다. 참조 [예약을 NSUrlSession](#Scheduling-a-NSUrlSession) 아래.
2. 7시 30분: 05에서 앱에는 원래 작업이 완료 되 면 시스템 절전 모드로 앱을 넣고를 계속 백그라운드에서 요청된 된 데이터를 다운로드 합니다.
3. 시스템에서 다운로드를 완료 하는 경우 다운로드 한 정보를 처리할 수 있도록 앱의 절전 모드를 새 작업을 만듭니다. 참조 [백그라운드 작업 처리](#Handling-Background-Tasks) 하 고 [다운로드 완료 처리](#Handling-the-Download-Completing) 아래. 
4. 앱의 업데이트 된 정보를 저장 하 고 작업 완료를 표시 합니다. 하지만 개발자는 Apple에서 해당 프로세스를 처리 하는 스냅숏 작업 일정을 제안 이때 앱의 사용자 인터페이스를 업데이트 하 고 싶은 생각이 들 수 있습니다. 참조 [스냅숏 업데이트를 예약](#Scheduling-a-Snapshot-Update) 아래.
5. 앱은 스냅숏 작업을 수신 하 고, 사용자 인터페이스를 업데이트, 작업 완료를 표시 합니다. 참조 [스냅숏 업데이트를 처리](#Handling-a-Snapshot-Update) 아래.

<a name="Scheduling-a-NSUrlSession" />

## <a name="scheduling-a-nsurlsession"></a>NSUrlSession를 예약합니다.

다음 코드는 최신 점수를 다운로드할 일정을 사용할 수 있습니다.

```csharp
private void ScheduleURLUpdateSession ()
{
    // Create new configuration
    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.example.urlsession");

    // Create new session
    var backgroundSession = NSUrlSession.FromConfiguration (configuration);

    // Create and start download task
    var downloadTask = backgroundSession.CreateDownloadTask (new NSUrl ("https://example.com/gamexxx/currentScores.json"));
    downloadTask.Resume ();
}
```

구성 하 고 새 `NSUrlSession`, 다음 해당 세션을 사용 하 여 새 다운로드를 만들어야 사용 하 여 작업을 `CreateDownloadTask` 메서드. 호출 된 `Resume` 다운로드 세션을 시작 하는 작업 메서드.

<a name="Handling-Background-Tasks" />

## <a name="handling-background-tasks"></a>백그라운드 작업 처리

재정의 하 여 합니다 `HandleBackgroundTasks` 메서드는 `WKExtensionDelegate`, 앱이 들어오는 백그라운드 작업을 처리할 수:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
        #endregion

        ...
        
        #region Public Methods
        public void CompleteTask (WKRefreshBackgroundTask task)
        {
            // Mark the task completed and remove from the collection
            task.SetTaskCompleted ();
            PendingTasks.Remove (task);
        }
        #endregion 

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request
            foreach (WKRefreshBackgroundTask task in backgroundTasks) {
                // Is this a background session task?
                var urlTask = task as WKUrlSessionRefreshBackgroundTask;
                if (urlTask != null) {
                    // Create new configuration
                    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

                    // Create new session
                    var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

                    // Keep track of all pending tasks
                    PendingTasks.Add (task);
                } else {
                    // Ensure that all tasks are completed
                    task.SetTaskCompleted ();
                }
            }
        }
        #endregion
        
        ...
    }
}
```

`HandleBackgroundTasks` 시스템 앱을 전송한는 모든 작업을 통해 메서드 주기 (에서 `backgroundTasks`)에 대 한 검색을 `WKUrlSessionRefreshBackgroundTask`. 세션에 다시 참가할 수 있으면 하나 및 연결 된 `NSUrlSessionDownloadDelegate` 다운로드 완료를 처리 하 (참조 [다운로드 완료를 처리](#Handling-the-Download-Completing) 아래):

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

컬렉션에 추가 하 여 완료 될 때까지 작업에 대 한 핸들을 유지:

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

모든 앱에 전송 작업의 필요를 완료 하 고 현재 처리 중인 모든 태스크에 대 한 표시 완료:

```csharp
if (urlTask != null) {
    ...
} else {
    // Ensure that all tasks are completed
    task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing" />

## <a name="handling-the-download-completing"></a>다운로드 완료를 처리합니다.

다음을 사용 하 여 MonkeySoccer 앱 `NSUrlSessionDownloadDelegate` 요청한 데이터를 다운로드 완료를 처리 하는 대리자:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class BackgroundSessionDelegate : NSUrlSessionDownloadDelegate
    {
        #region Computed Properties
        public ExtensionDelegate WatchExtensionDelegate { get; set; }

        public WKRefreshBackgroundTask Task { get; set; }
        #endregion

        #region Constructors
        public BackgroundSessionDelegate (ExtensionDelegate extensionDelegate, WKRefreshBackgroundTask task)
        {
            // Initialize
            this.WatchExtensionDelegate = extensionDelegate;
            this.Task = task;
        }
        #endregion

        #region Override Methods
        public override void DidFinishDownloading (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, NSUrl location)
        {
            // Handle the downloaded data
            ...

            // Mark the task completed
            WatchExtensionDelegate.CompleteTask (Task);

        }
        #endregion
    }
}
```

초기화 될 때 둘 다에 대 한 핸들을 유지 합니다 `ExtensionDelegate` 및 `WKRefreshBackgroundTask` 는 생성 된 것입니다. 재정의 `DidFinishDownloading` 다운로드 완료를 처리 하는 방법입니다. 사용 하 여는 `CompleteTask` 메서드는 `ExtensionDelegate` 작업이 완료 되었음을 알리고 보류 중인 작업의 컬렉션에서 제거 합니다. 참조 [백그라운드 작업 처리](#Handling-Background-Tasks) 위에 있습니다.

<a name="Scheduling-a-Snapshot-Update" />

## <a name="scheduling-a-snapshot-update"></a>스냅숏 업데이트를 예약합니다.

최신 점수를 사용 하 여 UI를 업데이트 하는 스냅숏 작업을 예약 하려면 다음 코드를 사용할 수 있습니다.

```csharp
private void ScheduleSnapshotUpdate ()
{
    // Create a fire date of now
    var fireDate = NSDate.FromTimeIntervalSinceNow (0);

    // Create user info dictionary
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
    userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleSnapshotRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

마찬가지로 `ScheduleURLUpdateSession` 만드는 새 메서드 위의 `NSDate` 이더라도 수 하려는 앱을 만듭니다는 `NSMutableDictionary` 요청된 작업의 세부 정보를 포함 하 합니다. 합니다 `ScheduleSnapshotRefresh` 메서드는 `SharedExtension` 태스크 예약을 요청 하는 데 사용 됩니다.

이 반환 됩니다는 `NSError` 요청된 작업을 예약할 수 없는 경우.

<a name="Handling-a-Snapshot-Update" />

## <a name="handling-a-snapshot-update"></a>스냅숏 업데이트를 처리합니다.

스냅숏 작업을 처리 하는 `HandleBackgroundTasks` 메서드 (참조 [백그라운드 작업 처리](#Handling-Background-Tasks) 위에) 다음과 같이 표시 하도록 수정 됩니다.

```csharp
public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
{
    // Handle background request
    foreach (WKRefreshBackgroundTask task in backgroundTasks) {
        // Take action based on task type
        if (task is WKUrlSessionRefreshBackgroundTask) {
            var urlTask = task as WKUrlSessionRefreshBackgroundTask;

            // Create new configuration
            var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

            // Create new session
            var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

            // Keep track of all pending tasks
            PendingTasks.Add (task);
        } else if (task is WKSnapshotRefreshBackgroundTask) {
            var snapshotTask = task as WKSnapshotRefreshBackgroundTask;

            // Update UI
            ...

            // Create a expiration date 30 minutes into the future
            var expirationDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

            // Create user info dictionary
            var userInfo = new NSMutableDictionary ();
            userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
            userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

            // Mark task complete
            snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
        } else {
            // Ensure that all tasks are completed
            task.SetTaskCompleted ();
        }
    }
}
```

처리 중인 작업의 형식에 대 한 메서드를 테스트 합니다. 경우는 `WKSnapshotRefreshBackgroundTask` 작업에 대 한 액세스를 얻을 수 있습니다.

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

메서드가 사용자 인터페이스를 업데이트 한 다음 만듭니다는 `NSDate` 스냅숏을 오래 된 경우 시스템에 게 합니다. 생성 된 `NSMutableDictionary` 새 스냅숏과 표시가이 정보를 사용 하 여 완료 된 스냅숏 작업을 설명 하기 위해 사용자 정보를 사용 하 여:

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

또한도 지시 스냅숏 작업 (첫 번째 매개 변수)의 기본 상태로 앱은 반환 되지 않습니다. 기본 상태 라는 개념이 있는 앱을 항상이 속성을 설정 해야 `true`합니다.

<a name="Working-Efficiently" />

## <a name="working-efficiently"></a>효율적으로 작동

볼 수 있듯이 MonkeySoccer 앱을 효율적으로 작동 하 고 새 watchOS를 사용 하 여 해당 점수를 업데이트 하는 5 분 창 위의 예에서 3 백그라운드 작업을 앱을 총 15 초에 대해 활성화만 했습니다. 

[![](background-tasks-images/update16.png "앱이 15 초 총 활성만")](background-tasks-images/update16.png#lightbox)

앱은 사용 가능한 Apple Watch 리소스와 배터리 수명에 영향을 줄이고이 고 시계에서 실행 중인 다른 앱을 더욱 효율적으로 사용 하도록 앱을 수 있습니다.

<a name="How-Scheduling-Works" />

## <a name="how-scheduling-works"></a>예약의 작동 방식

WatchOS 3 앱이 포그라운드에서 이지만, 실행 및 수 모든 유형의 데이터 업데이트와 같은 필요한 처리를 수행 하거나 해당 UI를 다시 그리도록 항상 예약 됩니다. 앱의 배경으로 움직이면 시스템에 의해 일시 중단 하 고 일반적으로 되 고 모든 런타임 작업이 중지 됩니다. 

앱이 백그라운드에서 이지만, 신속 하 게 특정 작업을 실행 하려면 시스템에서 대상이 될 수 있습니다. 즉, watchOS 2 시스템 길게 표시 알림 처리와 같은 작업을 수행 하거나 앱의 Complication 업데이트할 백그라운드 앱을 일시적으로 절전 수 있습니다. WatchOS 3에에서는 앱을 백그라운드에서 실행할 수 있는 몇 가지 새 가지가 있습니다.

앱이 백그라운드에서 이지만, 시스템에 몇 가지 제한 적용:

- 지정된 된 태스크를 완료 하려면 몇 초만 지정 됩니다. 시스템 고려 전달 하는 시간 뿐 아니라 얼마나 많은 CPU 전원도 앱이이 제한 파생에 사용 합니다.
- 해당 제한을 초과 하는 모든 앱에 다음 오류 코드를 사용 하 여 종료 됩니다.
    - **CPU** - 0xc51bad01
    - **시간** -0xc51bad02
- 시스템 앱 수행 하 라는 메시지가 표시 되는 백그라운드 작업의 형식을 기반으로 하는 다른 제한도 적용 됩니다. 예를 들어 `WKApplicationRefreshBackgroundTask` 고 `WKURLSessionRefreshBackgroundTask` 작업 다른 유형의 백그라운드 작업 보다 약간 더 런타임을 제공 됩니다.

<a name="Complications-and-App-Updates" />

### <a name="complications-and-app-updates"></a>복잡성 및 앱 업데이트

WatchOS 앱의 복잡성은 Apple watchOS 3에 추가 하는 새 백그라운드 작업을 하는 것 외에도 앱 백그라운드 업데이트를 받는 방법 및 시기에 영향을 줄을 가질 수 있습니다.

복잡성은 한 눈에 유용한 정보를 제공 하는 작은 visual 요소입니다. 선택한 watch 화면에 따라 사용자는 시계 모드에서 watchOS 3에서에서 watch 앱을 제공할 수 있는 하나 이상의 Complication 사용 하 여 사용자 지정할 수 있습니다.

사용자가 watch 화면에 앱의 복잡성 중 하나를 포함 하는 경우 또한 앱 업데이트 다음 혜택:

- 준비-시작에 앱을 유지 하려면 시스템이 상태, 여기서는 백그라운드에서 앱을 시작 하려고, 메모리 및 제공 업데이트 하는 추가 시간에 유지 합니다.
- 복잡성은 하루에 최소 50 개의 강제 업데이트 보장 됩니다.

개발자는 항상 사용자 위에 나열 된 이유로 해당 watch 화면에 추가 하도록 유인 하는 앱에 대 한 매력적인 복잡성을 만들려면 위해 노력 합니다.

WatchOS 2 복잡성이 앱이 백그라운드에서 작업 하는 동안 런타임 받았는지 기본 방법 이었습니다. WatchOS 3 시간 당 여러 업데이트를 받는 Complication 앱 확인 여전히 됩니다, 사용할 수 있지만 `WKExtensions` 자세한 런타임 복잡성이 해당 업데이트를 요청 합니다.

연결 된 iPhone 앱에서 Complication를 업데이트 하는 데 다음 코드를 살펴보십시오.

```csharp
using System;
using WatchConnectivity;
using UIKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
...

private void UpdateComplication ()
{

    // Get session and the number of remaining transfers
    var session = WCSession.DefaultSession;
    var transfers = session.RemainingComplicationUserInfoTransfers;

    // Create user info dictionary
    var iconattrs = new Dictionary<NSString, NSObject>
        {
            {new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0)},
            {new NSString ("reason"), new NSString ("UpdateScore")}
        };

    var userInfo = NSDictionary<NSString, NSObject>.FromObjectsAndKeys (iconattrs.Values.ToArray (), iconattrs.Keys.ToArray ());

    // Take action based on the number of transfers left
    if (transfers < 1) {
        // No transfers left, either attempt to send or inform
        // user of situation.
        ...
    } else if (transfers < 11) {
        // Running low on transfers, only send on important updates
        // else conserve for a significant change.
        ...
    } else {
        // Send data
        session.TransferCurrentComplicationUserInfo (userInfo);
    }
}
```

사용 하 여는 `RemainingComplicationUserInfoTransfers` 의 속성을 `WCSession` 얼마나 많은 우선 순위가 높은 앱 전송 보려는 떠난 일 한 다음 해당 번호를 기준으로 한 작업을 수행 합니다. 앱 전송 부족 해지면, 부분 업데이트를 보내는 방법에 보류를 중요 한 변경 내용이 있을 때 정보를 보냅니다.

<a name="Scheduling-and-Dock" />

### <a name="scheduling-and-the-dock"></a>일정 예약 및 도킹

WatchOS 3에에서 Apple 도킹 스테이션 사용자가 즐겨 찾는 앱을 고정 하 고 신속 하 게 액세스할 수 있는 추가 되었습니다. 사용자의 Apple Watch 쪽 단추를 누르면 고정 된 앱 스냅숏 갤러리에 표시 됩니다. 사용자는 왼쪽 또는 오른쪽 원하는 앱을 찾은 다음 실행 중인 앱의 인터페이스를 사용 하 여 스냅숏을 대체 시작 앱 탭에서 살짝 밀 수입니다.

[![](background-tasks-images/dock01.png "도킹 스테이션")](background-tasks-images/dock01.png#lightbox)

정기적으로 시스템은 앱의 UI의 스냅숏을 생성 하 고 이러한 스냅숏을 사용 하 여 문서를 채웁니다. watchOS 앱이이 스냅숏을 수행 하기 전에 해당 콘텐츠 및 UI를 업데이트할 수 있는 기회를 제공 합니다.

도킹 스테이션에 고정 된 앱 다음 기대할 수 있습니다.

- 최소 1 시간 마다 업데이트를 받게 됩니다. 앱 새로 고침 작업 및 스냅숏 작업을 모두 포함 합니다.
- 업데이트 예산 도킹 스테이션에서 응용 프로그램 간에 분산 됩니다. 더 적은 앱 사용자가 고정 더 잠재적인 업데이트 각 앱에서 수신 합니다.
- 앱은 다시 시작 신속 하 게 도킹 스테이션에서 선택 하도록 응용 프로그램 메모리에 유지 됩니다.

마지막으로 앱 사용자 실행으로 간주 됩니다 합니다 _가장 최근에 사용 됨_ 앱 도킹 스테이션에 있는 마지막 슬롯을 점유 합니다. 여기 있는에서 도킹 스테이션에 영구적으로 고정을 선택할 수 있습니다. 가장 최근에 사용한 다른 즐겨 찾는 앱 사용자가 도킹 스테이션에 이미 고정 된 처럼 처리 됩니다.

> [!IMPORTANT]
> 앱 홈 화면에만 추가 된 모든 일반 예약 지정 되지 않습니다. 일반 일정 예약 및 백그라운드 업데이트 앱 _해야 합니다_ 도킹 스테이션을 추가할 수 있습니다.

이 문서의 앞부분에서 언급 한 것 처럼 앱에 대 한 미리 보기 및 시작 이미지 작동 하므로 스냅숏을 watchOS 3에에서 매우 중요 합니다. 사용자 Dock의 앱에 도달, 하는 경우 전체 화면으로 확장 하 고, 전경을 입력 하 고 스냅숏을 최신 상태로 유지할 수는 명령적 이므로 실행을 시작 합니다.

앱의 UI의 새 스냅숏이 필요한 시스템을 결정 하는 경우 시간이 있을 수 있습니다. 이 상황에서 스냅숏 요청은 앱의 런타임 예산을 기준으로 계산 되지 않습니다. 다음은 시스템 스냅숏 요청을 트리거합니다.

- Complication 타임 라인 업데이트 합니다.
- 앱의 알림 사용 하 여 사용자 상호 작용 합니다.
- 전경에서 백그라운드 상태로 전환합니다.
- 따라서 백그라운드 상태로 있을 1 시간 후 앱의 기본 상태로 반환할 수 있습니다.
- 때 watchOS 먼저 부팅 됩니다.

<a name="Best-Practices" />

## <a name="best-practices"></a>최선의 구현 방법 

Apple는 백그라운드 작업과 함께 작업 하는 경우 다음 모범 사례를 제안 합니다.

- 앱을 업데이트 해야 하는 만큼 자주 예약 합니다. 앱이 실행 될 때마다 다시 해당 향후 수요를 평가 하 고 필요에 따라이 일정을 조정 합니다.
- 시스템 백그라운드 새로 고침 작업을 보내고 앱 업데이트가 필요 하지 않습니다, 업데이트로 실제로 필요할 때까지 작업을 지연 합니다.
- 앱에 제공 하는 모든 런타임 기회를 고려해 야 합니다.
    - 도킹 및 포그라운드 활성화 합니다.
    - 알림입니다.
    - Complication 업데이트 합니다.
    - 백그라운드 새로 고침 합니다.
- 사용 하 여 `ScheduleBackgroundRefresh` 와 같은 범용 백그라운드 런타임용:
    - 시스템에 정보를 폴링합니다.
    - 이후 예약 `NSURLSessions` 요청 백그라운드 데이터입니다. 
    - 알려진된 시간 전환 됩니다.
    - 트리거 Complication 업데이트 합니다.

<a name="Snapshot-Best-Practices" />

## <a name="snapshot-best-practices"></a>스냅숏 모범 사례

스냅숏 업데이트를 사용할 때 Apple에서는 다음 제안 합니다.

- 중요 한 콘텐츠 변경 내용이 있으면 예를 들어, 필요한 경우에 스냅숏을 무효화 합니다.
- 고주파 스냅숏을 무효화를 방지 합니다. 예를 들어 타이머 앱 스냅숏을 1 초 마다 업데이트 해서는 안 됩니다, 그리고 타이머를 종료 하는 경우에 수행 해야 합니다.

<a name="App-Data-Flow" />

## <a name="app-data-flow"></a>앱 데이터 흐름

Apple에 데이터 흐름 사용 하기 위한 다음을 제안 합니다.

[![](background-tasks-images/update17.png "앱 데이터 흐름 다이어그램")](background-tasks-images/update17.png#lightbox)

외부 이벤트 (예: 조사식 연결) 앱을 실행 됩니다. 이렇게 하면 응용 프로그램에서 해당 데이터 모델 (즉 앱의 현재 상태를 나타냄)을 업데이트 합니다. 새 스냅숏을 요청할 결과적으로 데이터 모델 변경의 앱을 업데이트 해야 해당 복잡성, 가능한 경우 백그라운드 시작 `NSURLSession` 더 많은 데이터를 끌어오고 백그라운드 추가로 예약을 새로 고칩니다.

<a name="The-App-Lifecycle" />

## <a name="the-app-lifecycle"></a>앱 수명 주기

도킹 및 즐겨 찾는 앱에 사용자를 이동 하 여 훨씬 더 많은 앱 간에 Apple 생각을 고정 하는 기능으로 인해 훨씬 더 자주 다음 이전과 watchos 2입니다. 결과적으로, 앱이이 변경을 처리 및 포그라운드 및 백그라운드 상태를 신속 하 게 이동 준비 해야 합니다.

Apple에는 다음 제안에 있습니다.

- 앱 전경 활성화를 입력 하면 가능한 한 빨리 모든 백그라운드 작업이 완료 되는지 확인 합니다.
- 모든 포그라운드 작업 완료를 호출 하 여 백그라운드에 들어가기 전에 `NSProcessInfo.PerformExpiringActivity`입니다.
- WatchOS 시뮬레이터에서에서 앱을 테스트할 때는 작업 예산을 하나도 적용 됩니다 제대로 기능을 테스트 하는 데 필요한 만큼 앱을 새로 고칠 수 있도록 합니다.
- ITunes Connect에 게시 하기 전에 해당 예산 이전 응용 프로그램을 실행 되지 않습니다 있도록 실제 Apple Watch 하드웨어에서 항상 테스트 합니다.
- Apple에서 제안 테스트 및 디버깅 하는 동안 Apple Watch 충전기에 유지 하 게 합니다.
- 모두 콜드 시작 하 고 앱 다시 시작를 철저히 테스트 했는지 확인 합니다.
- 모든 앱 작업 완료 되 고 있는지 확인 합니다.
- 최고 및 최악의 테스트할 Dock에 고정 되는 앱 수가 달라 지도록 사례 시나리오입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 향상 된 Apple에는 사용할 수 있는 방법을 watch 앱을 최신 상태로 유지 하 고 watchOS 검사가 수행 됩니다. 먼저 설명 새 모든 백그라운드 작업 Apple이 watchOS 3에에서 추가 합니다. 그런 다음 백그라운드 API 수명 주기 및 Xamarin watchOS 앱에서 백그라운드 작업을 구현 하는 방법을 설명 합니다. 마지막으로 어떻게 예약 작업을 설명 하 고 몇 가지 모범 사례를 제공 했습니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
