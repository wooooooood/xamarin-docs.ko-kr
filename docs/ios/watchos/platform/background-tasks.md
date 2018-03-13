---
title: "백그라운드 작업"
description: "새 백그라운드 작업 watchOS 3을 사용 하 여 watch 앱에는 항상 최신 데이터 및 도킹 스냅숏을 포함 되도록 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/13/2017
ms.openlocfilehash: 83841e62d863bf4be4edef5c0b6b7d486f192f4d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="background-tasks"></a>백그라운드 작업

_새 백그라운드 작업 watchOS 3을 사용 하 여 watch 앱에는 항상 최신 데이터 및 도킹 스냅숏을 포함 되도록 합니다._

WatchOS 3는 watch 앱 정보를 유지할 수는 최신 크게 세 가지가 있습니다. 

- 새로운 여러 가지 백그라운드 작업 중 하나를 사용 합니다. 
- 시계 화면의 (업데이트 하는 추가 시간 제공)에 해당 문제 중 하나를 때 발생 합니다. 
- 새 도크를 앱에 사용자 pin 필요 (여기서는 메모리에 유지 하 고 자주 업데이트) 합니다. 

## <a name="keeping-an-app-up-to-date"></a>응용 프로그램을 최신 상태로 유지

모든 현재 및 업데이트 된 개발자 watchOS 응용 프로그램의 데이터 및 사용자 인터페이스를 유지할 수 방법을 다루기 전에이 섹션은 살펴보세요 일반적인 일련의 사용 패턴 및 사용자가 iPhone와 해당 Apple Watch 따라 종일 사이 이동 될 수 있습니다 어떻게  시간 및 활동 현재 수행 하 고 (예: 구동).

다음 예제를 참조하세요.

[![](background-tasks-images/update00.png "사용자가 iPhone와 해당 Apple Watch 하루 종일 사이 이동 될 수 있습니다 어떻게")](background-tasks-images/update00.png#lightbox)

1. 커피를 대 한 줄에 대기 하는 동안 아침에 사용자가 몇 분 동안 해당 iPhone에 현재 뉴스를 탐색합니다.
2. 신속 하 게 너무 짧으면을 떠나기 전에 해당 watch 화면에는 복잡 한와 날씨를 체크 합니다.
3. 런치, 하기 전에 사용 지도 앱 iPhone의 근처 식당을 찾아 주소록에 클라이언트를 충족 하기 위해 예약 됩니다.
4. 식당을 전송 하는 동안 자신의 Apple Watch 빠른 보기를 사용 하는 알림을 받게, 해당 런치 약속 늦게 실행 되는 잘 알고 합니다.
5. 저녁에 사용 지도 앱 iPhone의 트래픽을 홈 구동 하기 전에 확인 됩니다.
6. 방식에 일부 우유를 선택 하기 위해 묻는 자신의 Apple Watch iMessage 알림의 받을 집 하 고 "확인" 응답을 보낼 빠른 회신 기능을 사용 하 합니다.

"빠른 보기" 때문에 (3 초 미만) 사용자가 Apple Watch 앱을 있습니다 일반적으로 사용 하려는 방법의 특성에는 원하는 정보를 가져오고 사용자에 게 표시 하기 전에 해당 UI를 업데이트 하려면 앱에 대 한 충분 한 시간 되지 않습니다.

Api Apple 내에 포함 된 새를 사용 하 여 watchOS 3 응용 프로그램에 대 한 예약할 수는 _백그라운드 새로 고침_ 전에 사용자가 요청할 준비 원하는 정보를 갖고 있습니다. 위에서 설명한 날씨 Complication의 예제를 수행 합니다.

[![](background-tasks-images/update01.png "날씨 Complication의 예")](background-tasks-images/update01.png#lightbox)

1. 에 의해 해제 시스템 특정 시간에 응용 프로그램 예약 합니다. 
2. 응용 프로그램 업데이트를 생성 하려면 필요할 정보를 인출 합니다.
3. 응용 프로그램에 새 데이터를 반영 하기 위해 해당 사용자 인터페이스를 다시 생성 합니다.
4. 사용자 응용 프로그램의 복잡 함 중 하나에서 glances 때 사용자에 게 업데이트 될 때까지 기다리지 않고 최신 정보를 있습니다.

위의 예제와 같이 watchOS 시스템에 사용할 수 있는 매우 제한 된 풀에 있는 하나 이상의 작업을 사용 하 여 응용 프로그램 다시 시작:

[![](background-tasks-images/update02.png "하나 이상의 작업을 사용 하 여 응용 프로그램을 다시 시작 되어 watchOS 시스템")](background-tasks-images/update02.png#lightbox)

응용 프로그램에서 자체를 업데이트 하는 프로세스를 완료할 때까지 끌어다 놓으면 Apple 제안 (응용 프로그램에 사용 하는 제한 된 리소스 이므로)이이 작업을 최대한을 확보 합니다.

시스템은 이러한 작업 새 호출 하 여 `HandleBackgroundTasks` 의 메서드는 `WKExtensionDelegate` 위임 합니다. 예:

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

응용 프로그램 지정된 된 작업이 완료 되 면 해당 반환 시스템에 표시를 완료 하 여:

[![](background-tasks-images/update03.png "작업 완료 표시 하 여 시스템에 반환 합니다.")](background-tasks-images/update03.png#lightbox)

<a name="New-Background-Tasks" />

## <a name="new-background-tasks"></a>새 백그라운드 작업

watchOS 3 응용 프로그램 콘텐츠를에 있는지 확인 하면 아래와 같은 응용 프로그램을 열기 전에 해당 정보를 업데이트 하는 데 사용할 수 있는 몇 가지 백그라운드 작업을 도입 합니다.

- **응용 프로그램 새로 고침 백그라운드** - [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) 태스크 백그라운드에서 해당 상태를 업데이트 하려면 앱을 허용 합니다. 일반적으로 사용 하 여 인터넷에서 새 콘텐츠를 다운로드 하는 등의 다른 작업을이 포함 됩니다는 [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession)합니다.
- **스냅숏 새로 고침 백그라운드** - [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) 태스크 시스템이 도킹 스테이션을 채우는 데 사용 하는 스냅숏을 만들기 전에 해당 콘텐츠 및 UI를 업데이트 하려면 앱을 허용 합니다.
- **조사식 연결 백그라운드** - [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) 쌍을 이루는 iPhone에서 배경 데이터를 받을 때 응용 프로그램에 대 한 작업을 시작 합니다.
- **URL 세션 백그라운드** - [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) 백그라운드 전송 권한 부여가 필요한 되거나 (성공적으로 또는 오류)를 완료 되 면 응용 프로그램에 대 한 작업을 시작 합니다.

이러한 작업은 아래 섹션에서 자세히 설명 합니다.

<a name="WKApplicationRefreshBackgroundTask" />

### <a name="wkapplicationrefreshbackgroundtask"></a>WKApplicationRefreshBackgroundTask

`WKApplicationRefreshBackgroundTask` 미래 날짜에 절전 모드 해제 앱을 예약할 수 있는 일반 작업은:

[![](background-tasks-images/update04.png "나중에 해제 되지 않습니다 WKApplicationRefreshBackgroundTask")](background-tasks-images/update04.png#lightbox)

응용 프로그램 작업의 런타임 내에서 모든 종류의 업데이트와 같은 로컬 처리 Complication 타임 라인을 수행 하거나와 필요한 일부 데이터를 인출 하는 `NSUrlSession`합니다.


<a name="WKURLSessionRefreshBackgroundTask" />

### <a name="wkurlsessionrefreshbackgroundtask"></a>WKURLSessionRefreshBackgroundTask

시스템에서 보냅니다는 `WKURLSessionRefreshBackgroundTask` 데이터 마쳤음을 다운로드 하 고 응용 프로그램에서 처리할 준비가:

[![](background-tasks-images/update05.png "데이터 다운로드가 완료 되 WKURLSessionRefreshBackgroundTask")](background-tasks-images/update05.png#lightbox)

앱은 백그라운드에서 데이터를 다운로드 하는 동시에 실행를 남겨둘 수 없습니다. 대신, 데이터에 대 한 요청을 예약 하는 응용 프로그램 다음 일시 중단 되 고 시스템 다운로드가 완료 되 면 앱만 reawakening 데이터의 다운로드를 처리 합니다.

<a name="WKSnapshotRefreshBackgroundTask" />

### <a name="wksnapshotrefreshbackgroundtask"></a>WKSnapshotRefreshBackgroundTask

3 watchOS Apple 도킹 스테이션 사용자가 즐겨 찾는 앱 고정 고 저장해 두고 빠르게 액세스할 수를 추가 했습니다. Apple Watch 측면 단추를 누를 때 스냅숏 고정 된 응용 프로그램 갤러리 표시 됩니다. 사용자 수 왼쪽 이나 오른쪽으로 원하는 앱을 찾은 다음 유틸리티를 실행 중인 응용 프로그램 인터페이스 스냅숏을 바꿉니다 실행 하려면 앱을 누르면 살짝 밉니다.

[![](background-tasks-images/update06.png "실행 중인 앱 인터페이스와 스냅숏이 교체")](background-tasks-images/update06.png#lightbox)

취합니다 응용 프로그램의 UI의 스냅숏을 정기적으로 (전송 하 여는 `WKSnapshotRefreshBackgroundTask`) 및 해당 스냅숏으로 사용 하 여 도킹 합니다. watchOS는 앱이이 스냅숏을 생성 하기 전에 해당 콘텐츠 및 UI를 업데이트할 수를 제공 합니다.

응용 프로그램에 대 한 미리 보기와 시작 이미지로 작동 하므로 스냅숏은 watchOS 3에서에서 매우 중요 합니다. 도킹 스테이션에서 응용 프로그램에 사용자 settles, 하는 경우 전체 화면으로 확장 하 고, 입력 전경 되며 반드시 스냅숏이 최신 상태로 유지할 수 있도록 실행을 시작:

[![](background-tasks-images/update07.png "전체 화면으로 확장 됩니다 도킹 스테이션에서 응용 프로그램에 사용자 settles, 하는 경우")](background-tasks-images/update07.png#lightbox)

시스템에서 발생 하는 다시는 `WKSnapshotRefreshBackgroundTask` 스냅숏을 생성 하기 전에 응용 프로그램 데이터와 UI를 업데이트) 하는 것 (여 준비할 수 있도록 합니다.

[![](background-tasks-images/update08.png "스냅숏을 생성 하기 전에 데이터 및 UI를 업데이트 하 여 앱 준비할 수 있습니다.")](background-tasks-images/update08.png#lightbox)

응용 프로그램을 표시는 `WKSnapshotRefreshBackgroundTask` 완료 되 면 시스템은 자동으로 스냅숏을 응용 프로그램의 UI입니다.

> [!IMPORTANT]
> **참고:** 시간은 항상 해야는 ` WKSnapshotRefreshBackgroundTask` 응용 프로그램에서 새 데이터 받고 해당 사용자 인터페이스를 업데이트 하거나 사용자 수정된 내용은 표시 되지 것입니다.




또한 사용자 앱에서 알림을 수신 하 고 응용 프로그램은 전경으로 가져올 수 탭을 스냅숏 최신 되므로 역할을 실행 화면도 필요 합니다.

[![](background-tasks-images/update09.png "사용자 앱에서 알림을 수신 하 고 응용 프로그램은 전경으로 가져올 수 탭")](background-tasks-images/update09.png#lightbox)

사용자와 watchOS 응용 작용 한 후 1 시간 이상 경과 했는데도, 하는 경우 해당 기본 상태인으로 반환할 수 됩니다. 기본 상태로 다른 응용 프로그램에 서로 다른 것을 의미할 수 있습니다 및 응용 프로그램의 디자인에 따라, 없는 경우 기본 상태로 전혀 합니다.

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask" />

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>WKWatchConnectivityRefreshBackgroundTask

WatchOS 3에서에서 Apple가 통합 조사식 연결을 통해 새 백그라운드 새로 고침 api `WKWatchConnectivityRefreshBackgroundTask`합니다. 이 새로운 기능을 사용 하 여 iPhone 앱을 전달할 수 새로운 데이터 조사식이 응용 프로그램에 대응 watchOS 앱은 백그라운드에서 실행 되는 동안.

[![](background-tasks-images/update10.png "WatchOS 앱은 백그라운드에서 실행 되는 동안 iPhone 앱 조사식이 응용 프로그램에 대응을 새로운 데이터 전달할 수 있습니다.")](background-tasks-images/update10.png#lightbox)

푸시 복잡 함 중 하나를 시작할 응용 프로그램 컨텍스트는 파일 또는 iPhone 앱에서 사용자 정보를 업데이트는 백그라운드에서 Apple Watch 앱 다시 시작 됩니다.

Watch 앱을 통해 경우 절전 모드 해제는 `WKWatchConnectivityRefreshBackgroundTask` 하려면 iPhone 앱에서 데이터를 수신 하 고, 표준 API 메서드를 사용 해야 합니다.

[![](background-tasks-images/update11.png "WKWatchConnectivityRefreshBackgroundTask 데이터 흐름")](background-tasks-images/update11.png#lightbox)

1. 세션이 활성화 되어 있는지 확인 합니다.
2. 새 모니터링 `HasContentPending` 속성 값이 있다면 `true`, 응용 프로그램에 아직 데이터를 처리 합니다. 대로 이전에 응용 프로그램에 모든 데이터 처리를 완료 될 때까지 작업에 포함 해야 합니다.
3. 데이터가 더 이상 처리할 수 없을 때 (`HasContentPending = false`), 시스템에 반환 하기 위해 완료 작업을 표시 합니다. 이렇게 하지 충돌 보고를 생성 하는 응용 프로그램의 할당 된 배경 런타임을 소모 됩니다.

<a name="The-Background-API-Lifecycle" />

## <a name="the-background-api-lifecycle"></a>배경 API 수명 주기

모든 새 백그라운드 작업 API의 부분이 함께 배치의 상호 작용 하는 일반적인 집합 다음과 같은 형태가 됩니다.

[![](background-tasks-images/update12.png "배경 API 수명 주기")](background-tasks-images/update12.png#lightbox)

1. 첫째, watchOS 앱 백그라운드 작업을 나중에 특정 시점으로 중지 되어 수를 예약 합니다.
2. 앱을 시스템에 의해 해제 되지 않습니다 하 고 작업을 전송 합니다.
3. 응용 프로그램 작업이 필요한 모든 작업은 완료를 처리 합니다.
4. 따라서 작업 처리 응용 프로그램 예약 해야 할 수 자세한 배경을 알고 싶으면 작업을 사용 하 여 콘텐츠를 다운로드 하는 등 나중에 더 많은 작업을 완료 한 `NSUrlSession`합니다.
5. 응용 프로그램 작업 완료를 표시 하며 시스템에 반환 합니다.

<a name="Using-Resources-Responsibly" />

## <a name="using-resources-responsibly"></a>리소스를 책임감 있게 사용

watchOS 앱 작동 하는지 책임감 있게이 에코 시스템 내에서 해당 시스템의 공유 리소스가 소모를 제한 하 여 합니다.

다음 시나리오를 살펴보겠습니다.

[![](background-tasks-images/update13.png "WatchOS 앱 제한 시스템의 공유 리소스에서 해당 드레이닝")](background-tasks-images/update13.png#lightbox)

1. 사용자가 오후 1 시에 watchOS 응용 프로그램을 시작 합니다.
2. 응용 프로그램에는 절전 모드 해제 오후 2 시에 한 시간에 새로운 콘텐츠를 다운로드 하는 작업을 예약 합니다.
3. 오후 1 시 50 분 사용자이 이번에는 데이터와 UI를 업데이트 하는 응용 프로그램을 다시 엽니다.
4. 10 분 후에 다시 작업 wake 앱 말고 응용 프로그램 1 시간 오후 2 시 50 분에 나중에 실행 되도록 작업을 다시 예약 해야 합니다.

다른 모든 응용 프로그램 이지만, 사과 시스템 리소스를 절약 하려면 위의 표시 된 것 처럼, 사용 패턴을 찾고 제안 합니다.

<a name="Implementing-Background-Tasks" />

## <a name="implementing-background-tasks"></a>백그라운드 작업을 구현합니다.

이 문서는 예에서 축구 점수 사용자에 게 보고 하는 가짜 MonkeySoccer 스포츠 응용 프로그램을 사용 합니다. 

다음과 같은 일반적인 사용 시나리오를 살펴보겠습니다.

[![](background-tasks-images/update14.png "일반적인 사용 시나리오")](background-tasks-images/update14.png#lightbox)

사용자의 즐겨 찾는 축구 팀 오후 7시에서 오후 9시 일치 하는 큰 항목이 재생 하므로 응용 프로그램 사용자의 점수를 정기적으로 확인 수를 예상 해야 하 고 30 분에 대 한 업데이트 간격으로 결정 합니다.

1. 사용자가 앱을 열 및 30 분 후 백그라운드 업데이트에 대 한 작업을 예약 합니다. 배경 API 백그라운드 작업을 지정된 된 시간에 실행 되 고 한 가지 형식만이 있습니다.
2. 응용 프로그램 작업을 수신 하 고 데이터와 UI를 업데이트 한 다음 다른 일정 백그라운드 작업이 30 분 후 합니다. 다른 백그라운드 작업을 예약 하는 개발자 기억 또는 앱 됩니다 하지 수 다시 않아 더 많은 업데이트를 가져오려면 유용 합니다.
3. 다시, 응용 프로그램 작업 및 해당 데이터를 업데이트, UI를 업데이트 받아 다른 백그라운드 작업이 30 분 후 예약 합니다.
4. 동일한 프로세스를 다시 반복 됩니다.
5. 마지막 백그라운드 작업에서 수신 되 고 응용 프로그램 데이터와 UI를 업데이트 다시 됩니다. 이 최종 점수는 새 백그라운드 새로 고침을 예약 하지 않습니다. 

<a name="Scheduling-for-Background-Update" />

## <a name="scheduling-for-background-update"></a>백그라운드 업데이트에 대 한 일정

MonkeySoccer 앱 위의 시나리오 들어 백그라운드 업데이트에 대 한 일정을 다음 코드를 사용할 수 있습니다.:

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

만드는 새 `NSDate` 30 분 이후 when에 응용 프로그램 않아 수 하려고 하 고 만듭니다는 `NSMutableDictionary` 요청 된 작업의 세부 정보를 저장할 수 있습니다. `ScheduleBackgroundRefresh` 의 메서드는 `SharedExtension` 작업을 예약할 수 요청 하는 데 사용 됩니다.

반환 됩니다는 `NSError` 경우 요청한 작업을 예약 하지 못했습니다.

<a name="Processing-the-Update" />

## <a name="processing-the-update"></a>업데이트 처리

다음으로 점수를 업데이트 하는 데 필요한 단계를 보여 주는 5 분 창 자세히 보기를 수행 합니다.

[![](background-tasks-images/update15.png "점수를 업데이트 하는 데 필요한 단계를 보여 주는 5 분 창")](background-tasks-images/update15.png#lightbox)

1. 오후 7시 30분: 02 앱을 시스템에 의해 활성화 하 고 업데이트 백그라운드 작업을 지정 합니다. 첫 번째 우선 순위는 서버에서 최신 점수를 확보 하는 것입니다. 참조 [는 NSUrlSession 예약](#Scheduling-a-NSUrlSession) 아래 합니다.
2. 7시 30분: 05에 응용 프로그램에는 원래 작업에이 완료 되 면 시스템 응용 프로그램에서 절전 모드를 전환 하 고 계속 백그라운드에서 요청 된 데이터를 다운로드 합니다.
3. 시스템에서 다운로드를 완료 하는 경우 다운로드 한 정보를 처리할 수 있도록 응용 프로그램의 절전 모드를 새 작업을 만듭니다. 참조 [백그라운드 작업을 처리](#Handling-Background-Tasks) 및 [다운로드 완료 처리](#Handling-the-Download-Completing) 아래 합니다. 
4. 응용 프로그램 업데이트 된 정보를 저장 하 고 작업 완료를 표시 합니다. 하지만 Apple 해당 프로세스를 처리 하는 스냅숏 작업의 일정을 제안 개발자는이 시간에 응용 프로그램의 사용자 인터페이스를 업데이트 하려고 시도할 수 있습니다. 참조 [스냅숏 업데이트를 예약](#Scheduling-a-Snapshot-Update) 아래 합니다.
5. 응용 프로그램 스냅숏 작업을 수신 하 고, 사용자 인터페이스를 업데이트, 작업 완료를 표시 합니다. 참조 [스냅숏 업데이트를 처리](#Handling-a-Snapshot-Update) 아래 합니다.

<a name="Scheduling-a-NSUrlSession" />

## <a name="scheduling-a-nsurlsession"></a>NSUrlSession 예약

최신 점수의 다운로드를 예약 하려면 다음 코드를 사용할 수 있습니다.

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

구성 하 고 새 `NSUrlSession`, 다음 해당 세션을 사용 하 여 새 다운로드를 만들려는 사용 하 여는 `CreateDownloadTask` 메서드. 호출 된 `Resume` 다운로드 세션을 시작 하는 작업의 메서드.

<a name="Handling-Background-Tasks" />

## <a name="handling-background-tasks"></a>백그라운드 작업 처리

재정의 하 여는 `HandleBackgroundTasks` 의 메서드는 `WKExtensionDelegate`, 앱은 들어오는 백그라운드 작업을 처리할 수 있습니다.

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

`HandleBackgroundTasks` 메서드 모든 작업을 순환 하는 시스템에서 응용 프로그램을 보냈습니다 (에서 `backgroundTasks`)에 대 한 검색 한 `WKUrlSessionRefreshBackgroundTask`합니다. 세션에 다시 참여할 발견 되 고 첨부 한 `NSUrlSessionDownloadDelegate` 다운로드 완료를 처리 하 (참조 [다운로드 완료 처리](#Handling-the-Download-Completing) 아래):

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

컬렉션에 추가 하 여 완료 된 작업에 대 한 핸들 않음:

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

응용 프로그램에 게 해당 작업의 모든 필요한를 완료 하 고 현재 처리 중인 모든 작업에 대 한 완료 플래그:

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

다음을 사용 하 여 MonkeySoccer 앱 `NSUrlSessionDownloadDelegate` 대리자 다운로드 완료를 처리 하는 요청 된 데이터:

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

둘 다에 대 한 핸들 초기화 될 때 유지는 `ExtensionDelegate` 및 `WKRefreshBackgroundTask` 생성입니다. 재정의 `DidFinishDownloading` 다운로드 완료를 처리 하는 메서드. 다음 사용 하 여는 `CompleteTask` 의 메서드는 `ExtensionDelegate` 알리기 위해는 작업을 완료 되었으며 보류 중인 작업의 컬렉션에서 제거 합니다. 참조 [백그라운드 작업을 처리](#Handling-Background-Tasks) 위에 있습니다.

<a name="Scheduling-a-Snapshot-Update" />

## <a name="scheduling-a-snapshot-update"></a>스냅숏 업데이트를 예약합니다.

최신 점수가 있는 UI를 업데이트 하는 스냅숏 작업을 예약 하려면 다음 코드를 사용할 수 있습니다.

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

마찬가지로 `ScheduleURLUpdateSession` 만드는 새 위의 메서드 `NSDate` 않아 수 하려는 응용 프로그램을 만듭니다는 `NSMutableDictionary` 요청 된 작업의 세부 정보를 저장할 수 있습니다. `ScheduleSnapshotRefresh` 의 메서드는 `SharedExtension` 작업을 예약할 수 요청 하는 데 사용 됩니다.

반환 됩니다는 `NSError` 경우 요청한 작업을 예약 하지 못했습니다.

<a name="Handling-a-Snapshot-Update" />

## <a name="handling-a-snapshot-update"></a>스냅숏 업데이트를 처리합니다.

스냅숏 작업을 처리 하는 `HandleBackgroundTasks` 메서드 (참조 [백그라운드 작업을 처리](#Handling-Background-Tasks) 위에)를 다음과 같이 수정 됩니다.

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

메서드가 처리 중인 작업의 형식에 대 한 테스트 합니다. 이 경우는 `WKSnapshotRefreshBackgroundTask` 작업에 대 한 액세스를 얻습니다.

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

메서드는 사용자 인터페이스를 업데이트 한 다음 만듭니다는 `NSDate` 스냅숏을 유효 하지 않은 경우 시스템을 구별 하 합니다. 생성 한 `NSMutableDictionary` 새 스냅숏과 표시 스냅숏 작업이 완료 되었으나이 정보를 설명 하기 위해 사용자 정보로:

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

또한도 인지 스냅숏 작업 응용 프로그램 (첫 번째 매개 변수)의 기본 상태로 돌아오지 않는 합니다. 기본 상태의 개념이 포함 된 응용 프로그램을 항상이 속성을 설정 해야 `true`합니다.

<a name="Working-Efficiently" />

## <a name="working-efficiently"></a>효율적으로 작동

와 같이 MonkeySoccer 앱 효율적으로 작동 하 고 새 watchOS를 사용 하 여 해당 점수를 업데이트 하는 데 걸린 최대 5 분 창 위의 예에서 3 백그라운드 작업을 앱을 15 초 총 활성만: 

[![](background-tasks-images/update16.png "응용 프로그램은 15 초 총에 활성 상태 였던만")](background-tasks-images/update16.png#lightbox)

이 응용 프로그램 했을 사용할 수 있는 Apple Watch 리소스와 배터리 수명에 영향을 통해 줄어듭니다 응용 프로그램이 시계에서 실행 되는 다른 앱과 원활 하 게 작동 되도록 해줍니다.

<a name="How-Scheduling-Works" />

## <a name="how-scheduling-works"></a>일정 설정의 작동 방법

3 watchOS 앱이 포그라운드에서 이지만를 실행 하 고 수 모든 종류의 데이터 업데이트와 같은 필요한 처리를 수행 하거나 UI를 다시 그리기 항상 예약 됩니다. 앱의 배경으로 움직이면은 일반적으로 시스템에 의해 일시 중단 및 모든 런타임 작업이 중지 됩니다. 

백그라운드에서 응용 프로그램을 신속 하 게 특정 작업을 실행 하려면 시스템에서 대상이 될 수 있습니다. 따라서 2 watchOS 시스템 일시적으로 절전 모드가 해제 긴 모양의 알림을 처리 하는 등의 작업을 나 응용 프로그램의 복잡 함 중 하나를 업데이트 하는 백그라운드 응용 프로그램입니다. WatchOS 3에에서는 응용 프로그램을 백그라운드에서 실행 될 수 있는 여러 가지 방법이 있습니다.

백그라운드에서 응용 프로그램 이지만, 시스템 것에 대 한 몇 가지 제한 사항이 있습니다.

- 지정된 된 태스크를 완료 하려면 몇 초만 지정 됩니다. 시스템에서는 전달 된 시간의 양 뿐만 아니라이 한도 파생 하는 앱이 사용 CPU 전력이 얼마나도를 고려 합니다.
- 제한을 초과 하는 모든 앱 다음 오류 코드와 함께 종료 됩니다.
    - **CPU** - 0xc51bad01
    - **시간** -0xc51bad02
- 시스템에는 수행 하는 응용 프로그램을 요청 하는 백그라운드 작업의 유형에 따라 서로 다른 제한이 적용 됩니다. 예를 들어 `WKApplicationRefreshBackgroundTask` 및 `WKURLSessionRefreshBackgroundTask` 작업 다른 유형의 백그라운드 작업 보다 시간이 약간 더 런타임을 제공 됩니다.

<a name="Complications-and-App-Updates" />

### <a name="complications-and-app-updates"></a>복잡성 및 응용 프로그램 업데이트

Apple watchOS 3에 추가한 새 백그라운드 작업 외에도 watchOS 응용 프로그램의 복잡성 앱 백그라운드 업데이트를 받는 방법 및 시기에 영향을 가질 수 있습니다.

Complication은 작은 시각적 요소를 한 눈에 유용한 정보를 제공 합니다. 선택한 watch 화면을 따라 사용자는 watchOS 3에서에서 watch 앱으로 제공할 수 있는 하나 이상의 Complication와 watch 화면 사용자 지정 하는 기능을 있습니다.

사용자 자신의 watch 화면에서 응용 프로그램의 복잡성을 포함 하는 경우 제공 응용 프로그램 업데이트 다음 이점:

- 시스템이 준비-시작의 응용 프로그램을 유지 하기 위해 여기서는 백그라운드에서 응용 프로그램을 실행 하려고, 상태 업데이트 하는 추가 시간 제공 및 메모리에 유지 합니다.
- 복잡성 하루 최소 50 푸시 업데이트 보장 됩니다.

개발자가 앱에 대 한 눈에 띄는 복잡성을 이유는 위에 나열 된 자신의 watch 화면에 추가 하는 사용자를 유도 만들 하도록 항상 해야 합니다.

2 watchOS 문제가 응용 프로그램을 백그라운드에서 작업 하는 동안 런타임 받았는지는 기본 방법 이었습니다. 하지만 3 watchOS 시간당 한 여러 업데이트를 받을 Complication 앱 확인 여전히 됩니다, 사용할 수 `WKExtensions` 의 복잡성을 업데이트 하려면 더 많은 런타임 요청할 수 있습니다.

연결 된 iPhone 앱에서 Complication 업데이트 하는 데 다음 코드를 살펴보겠습니다.

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

사용 하 여는 `RemainingComplicationUserInfoTransfers` 의 속성은 `WCSession` 개수 우선 순위가 높은 전송 응용 프로그램을 보려면 없어진 하루 한 다음 해당 수에 따라 작업을 수행 합니다. 응용 프로그램을 실행 전송 하기 시작 하면 것과 사용할 수 있습니다 부분 업데이트를 보내는 방법에 중요 한 변경 될 때만 정보를 전송 합니다.

<a name="Scheduling-and-Dock" />

### <a name="scheduling-and-the-dock"></a>일정 예약 및 도킹

3 watchOS Apple 도킹 스테이션 사용자가 즐겨 찾는 앱 고정 고 저장해 두고 빠르게 액세스할 수를 추가 했습니다. Apple Watch 측면 단추를 누를 때 스냅숏 고정 된 응용 프로그램 갤러리 표시 됩니다. 사용자 수 왼쪽 이나 오른쪽으로 원하는 앱을 찾은 다음 유틸리티를 실행 중인 응용 프로그램 인터페이스 스냅숏을 바꿉니다 실행 하려면 앱을 누르면 살짝 밉니다.

[![](background-tasks-images/dock01.png "도킹 스테이션")](background-tasks-images/dock01.png#lightbox)

정기적으로 시스템 응용 프로그램의 UI의 스냅숏을 사용 하 고 해당 스냅숏으로 사용 하 여 문서. watchOS는 앱이이 스냅숏을 생성 하기 전에 해당 콘텐츠 및 UI를 업데이트할 수를 제공 합니다.

도킹 스테이션에 고정 된 앱 다음 예상할 수 있습니다.

- 최소 1 시간 마다 업데이트를 받게 됩니다. 응용 프로그램 새로 고침 작업 및 스냅숏 작업을 모두 포함 합니다.
- 업데이트 예산 도킹 스테이션에서 응용 프로그램의 모든 간 배포 됩니다. 더 적은 앱 사용자가 고정 된 각 앱은 수신 더 잠재적인 업데이트 합니다.
- 응용 프로그램은 다시 시작 신속 하 게 도킹 스테이션에서 선택 하도록 응용 프로그램 메모리에 유지 됩니다.

마지막 응용 프로그램 사용자 실행으로 간주 됩니다는 _가장 최근에 사용한_ 앱 도킹 스테이션에서 마지막 슬롯을 점유 합니다. 이 위치에서 있는 도킹 스테이션에 영구적으로 고정을 선택할 수 있습니다. 가장 최근에 사용한 다른 즐겨 찾는 앱 사용자가 도킹 스테이션에 이미 고정 된 처럼 처리 됩니다.

> [!IMPORTANT]
> **참고:** 정기적인 일정 앱 홈 화면에 추가 된만 지정 되지 것입니다. 일반 일정 예약 및 백그라운드 업데이트, 응용 프로그램 _해야_ 도킹 스테이션에 추가할 수 있습니다.

이 문서의 앞부분에서 설명 했 듯이 스냅숏은 watchOS 3에서에서 매우 중요 하므로 응용 프로그램에 대 한 미리 보기와 시작 이미지로 작동 합니다. 도킹 스테이션에서 응용 프로그램에 사용자 settles, 하는 경우 전체 화면으로 확장 하 고, 전경 입력 되며 반드시 스냅숏이 최신 상태로 유지할 수 있도록 실행을 시작 합니다.

응용 프로그램의 UI의 새 스냅숏이 필요한 시스템을 결정 하는 경우 시간이 있을 수 있습니다. 이 상황에서 스냅숏 요청 응용 프로그램의 런타임 예산에 따라 계산 되지 않습니다. 시스템 스냅숏 요청을 트리거하려면 다음 됩니다.

- Complication 타임 라인 업데이트 합니다.
- 응용 프로그램의 알림 사용 하 여 사용자 상호 작용 합니다.
- 백그라운드 상태 전경에서 전환 합니다.
- 따라서 된 배경 상태로 1 시간 후 응용 프로그램의 기본 상태로 반환할 수 있습니다.
- 때 watchOS 먼저 부팅 됩니다.

<a name="Best-Practices" />

## <a name="best-practices"></a>모범 사례 

백그라운드 작업과 함께 작업 하는 경우 다음 모범 사례를 제안 하는 Apple:

- 응용 프로그램을 업데이트 해야 하는 만큼 자주 예약 합니다. 앱이 실행 될 때마다 해당 향후 요구 사항 다시 확인 하 고 필요에 따라이 일정을 조정 합니다.
- 시스템 백그라운드 새로 고침 작업을 전송 하는 경우 앱에 대 한 업데이트 필요 하지 않습니다는 업데이트가 실제로 필요할 때까지 작업을 지연 합니다.
- 앱에 사용할 수 있는 모든 런타임 기회를 고려 합니다.
    - 도킹 및 전경색 활성화 합니다.
    - 알림입니다.
    - 복잡 함 중 하나를 업데이트 합니다.
    - 백그라운드 새로 고침 합니다.
- 사용 하 여 `ScheduleBackgroundRefresh` 과 같은 범용 배경 런타임용:
    - 자세한 내용은 시스템을 폴링합니다.
    - 미래의 예약 `NSURLSessions` 데이터를 요청할 배경. 
    - 알려진된 시간 전환 됩니다.
    - 트리거 Complication 업데이트 합니다.

<a name="Snapshot-Best-Practices" />

## <a name="snapshot-best-practices"></a>스냅숏에 대 한 유용한 정보

스냅숏 업데이트를 사용할 때 Apple에서는 다음 제안 합니다.

- 중요 한 콘텐츠 변경 내용이 있을 때 예를 들어, 필요한 경우에 스냅숏을 무효화 합니다.
- 스냅숏 무효화를 자주 발생 하지 않습니다. 예를 들어 타이머 응용 프로그램의 스냅숏 1 초 마다 업데이트 하지 않아야, 타이머 종료 된 경우에 수행 해야 합니다.

<a name="App-Data-Flow" />

## <a name="app-data-flow"></a>응용 프로그램 데이터 흐름

Apple는 데이터 흐름에 따라 작업에 대해 다음을 제안 합니다.

[![](background-tasks-images/update17.png "응용 프로그램 데이터 흐름 다이어그램")](background-tasks-images/update17.png#lightbox)

외부 이벤트 (예: 조사식 Connectivity) 응용 프로그램 다시 시작 합니다. 이렇게 하면 응용 프로그램에서 해당 데이터 모델 (응용 프로그램의 현재 상태를 나타냄)를 업데이트 합니다. 새 스냅숏으로 요청 결과적으로 데이터 모델 변경의 응용 프로그램을 업데이트 해야 합니다는 복잡성, 가능 배경 시작 `NSURLSession` 더 많은 데이터를 끌어오고 하 배경을 예약 하는 데 새로 고칩니다.

<a name="The-App-Lifecycle" />

## <a name="the-app-lifecycle"></a>앱 수명 주기

도킹 및 고정 하 고, 사과 생각 사용자 훨씬 더 많은 앱 간에 이동 하 고 즐겨 찾는 앱 수 때문에 훨씬 더 자주 다음 않았는데 watchOS 2와 합니다. 결과적으로, 앱이이 변경을 처리 하 고 신속 하 게 전경색과 배경색 상태 간에 이동 해야 합니다.

Apple에 다음 제안 사항이 있습니다.

- 응용 프로그램 전경 활성화 입력에 가능한 한 빨리 백그라운드 태스크가 완료 되는지 확인 합니다.
- 모든 foreground 작업 완료를 확인 합니다. 호출 하 여 백그라운드로 유입 되기 전에 `NSProcessInfo.PerformExpiringActivity`합니다.
- WatchOS 시뮬레이터에서에서 앱을 테스트할 때 작업 예산의 없음이 적용 응용 프로그램에 적절 한 기능을 테스트 하는 데 필요한 만큼 자주 새로 고칠 수 있도록.
- 항상 iTunes Connect에는 응용 프로그램 게시 하기 전에 예산을 지난 실행 되지 않습니다 수 있도록 실제 Apple Watch 하드웨어에서 테스트 합니다.
- Apple 제안 테스트 및 디버깅 하는 동안 Apple Watch 충전기에 유지 합니다.
- 모두 콜드 시작 및 다시 시작 응용 프로그램를 throughly 테스트 했는지 확인 합니다.
- 모든 응용 프로그램 작업 완료 되 고 있는지 확인 합니다.
- 최고 및 최악의 테스트에 도킹에 고정 된 앱의 수를 다르게 사례 시나리오입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서의 향상 된 Apple에 watchOS 있습니다 사용할 수 있는 방법을 watch 앱을 최신 상태로 유지 하 고 검사가 수행 됩니다. 첫째, 것 모든 새 포함 백그라운드 작업 Apple watchOS 3에에서 추가 되었습니다. 그런 다음 백그라운드 API 수명 주기 및 Xamarin watchOS 응용 프로그램에서 백그라운드 작업을 구현 하는 방법을 설명 합니다. 마지막으로 검사 일정 관리가 어떻게 작동 하 고 지정 하는 몇 가지 모범 사례.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
