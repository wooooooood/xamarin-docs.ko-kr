---
title: Xamarin에서 백그라운드 작업 watchOS
description: 이 문서에서는 Xamarin에서 watchOS를 사용 하 여 백그라운드 작업을 사용 하는 방법을 설명 합니다 .이 문서에서는 리소스를 사용 하 여 백그라운드 작업을 구현 하 고, 예약 하 고, 모범 사례를 구현 합니다.
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/13/2017
ms.openlocfilehash: b01fbbe813b778d3c2e1cabeba620ed48a46ecac
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287152"
---
# <a name="watchos-background-tasks-in-xamarin"></a>Xamarin에서 백그라운드 작업 watchOS

WatchOS 3을 사용 하는 경우 세 가지 주요 방법으로 시청 앱이 정보를 최신 상태로 유지할 수 있습니다. 

- 몇 가지 새로운 백그라운드 작업 중 하나를 사용 합니다. 
- Watch에 대 한 복잡 한 문제 중 하나 (업데이트에 추가 시간 제공) 
- 사용자가 앱에 고정 하 여 새 도크에 고정 하는 경우 (여기서는 메모리에 유지 되 고 자주 업데이트 됨) 

## <a name="keeping-an-app-up-to-date"></a>앱을 최신 상태로 유지

개발자가 watchOS 앱의 데이터와 사용자 인터페이스를 최신으로 유지 하 고 업데이트할 수 있는 모든 방법을 설명 하기 전에이 섹션에서는 일반적인 사용 패턴 집합을 살펴보고, 사용자가 해당 iPhone Apple Watch 및  현재 수행 중인 작업 및 시간 (예: 구동)입니다.

다음 예제를 참조하세요.

[![](background-tasks-images/update00.png "사용자가 하루 내내 iPhone과 해당 Apple Watch 사이를 이동할 수 있는 방법")](background-tasks-images/update00.png#lightbox)

1. 아침에는 커피를 줄 때까지 기다리는 동안 사용자는 몇 분 동안 iPhone에서 현재 뉴스를 찾아봅니다.
2. 커피를 떠나기 전에 시계를 신속 하 게 확인 합니다.
3. 식사 전에 iPhone의 Maps 앱을 사용 하 여 주변의 식당을 찾고 클라이언트를 충족 하는 예약을 예약할 수 있습니다.
4. 식당으로 이동 하는 동안에는 해당 Apple Watch에 대 한 알림이 표시 되 고,이를 신속 하 게 확인 하 여 점심 약속이 늦게 실행 되 고 있음을 알 수 있습니다.
5. 저녁에는 집을 구동 하기 전에 iPhone의 Maps 앱을 사용 하 여 트래픽을 확인 합니다.
6. Home 방법으로, 일부 우유를 선택 하도록 요청 하는 Apple Watch에 대 한 iMessage 알림을 받고, 빠른 회신 기능을 사용 하 여 응답을 "OK"로 보냅니다.

사용자가 Apple Watch 앱을 사용 하려는 방법에 대 한 "빠른 보기" (3 초 미만) 특성 때문에 일반적으로 앱에서 원하는 정보를 가져오고 UI를 업데이트 하 여 사용자에 게 표시 하기 전에는 시간이 충분 하지 않습니다.

WatchOS 3에 포함 된 새 Api를 사용 하 여 앱은 _백그라운드 새로 고침_ 을 예약 하 고 사용자가 요청 하기 전에 원하는 정보를 준비할 수 있습니다. 위에서 설명한 날씨의 예를 살펴보겠습니다.

[![](background-tasks-images/update01.png "날씨의 예")](background-tasks-images/update01.png#lightbox)

1. 앱이 특정 시간에 시스템에 의해 해제 예약 됩니다. 
2. 앱은 업데이트를 생성 하는 데 필요한 정보를 페치합니다.
3. 앱은 새 데이터를 반영 하기 위해 사용자 인터페이스를 다시 생성 합니다.
4. 사용자가 앱의 복잡 한 경우 업데이트를 기다릴 필요가 없는 최신 정보를 glances.

위에서 설명한 것 처럼 watchOS 시스템은 하나 이상의 작업을 사용 하 여 앱의 절전 모드를 해제 합니다 .이 작업은 매우 제한 된 풀을 사용할 수 있습니다.

[![](background-tasks-images/update02.png "WatchOS 시스템은 하나 이상의 작업을 사용 하 여 앱의 절전 모드를 해제 합니다.")](background-tasks-images/update02.png#lightbox)

Apple은 앱이 자신을 업데이트 하는 프로세스를 완료할 때까지이 작업을 보류 하 여 앱에 대 한 제한 된 리소스 이므로이 작업을 최대한 활용 하는 것이 좋습니다.

시스템은 `HandleBackgroundTasks` `WKExtensionDelegate` 대리자의 새 메서드를 호출 하 여 이러한 작업을 제공 합니다. 예를 들어:

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

앱이 지정 된 작업을 완료 하면 완료 된 것으로 표시 하 여 시스템에 반환 합니다.

[![](background-tasks-images/update03.png "작업이 완료 된 것으로 표시 하 여 시스템으로 돌아갑니다.")](background-tasks-images/update03.png#lightbox)

<a name="New-Background-Tasks" />

## <a name="new-background-tasks"></a>새 백그라운드 작업

watchOS 3에는 앱이 정보를 업데이트 하는 데 사용할 수 있는 몇 가지 백그라운드 작업이 도입 되었으며, 다음과 같이 앱을 열기 전에 사용자에 게 필요한 콘텐츠가 있는지 확인 하는 데 사용할 수 있습니다.

- **백그라운드 응용 프로그램 새로 고침** - [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) 작업을 사용 하면 앱이 백그라운드에서 상태를 업데이트할 수 있습니다. 일반적으로이 작업에는 [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession)를 사용 하 여 인터넷에서 새 콘텐츠를 다운로드 하는 등의 다른 작업이 포함 됩니다.
- **백그라운드 스냅숏 새로 고침** - [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) 작업을 사용 하면 시스템이 도크를 채우는 데 사용 되는 스냅숏을 만들기 전에 콘텐츠와 UI를 모두 업데이트할 수 있습니다.
- **백그라운드 감시 연결** -쌍을 이루는 iPhone의 배경 데이터를 받을 때 앱에 대 한 [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) 작업을 시작 합니다.
- **백그라운드 URL 세션** -백그라운드 전송에 권한 부여가 필요 하거나 완료 (오류)가 완료 되 면 앱에 대 한 [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) 작업이 시작 됩니다.

이러한 작업에 대해서는 아래 섹션에서 자세히 설명 합니다.

<a name="WKApplicationRefreshBackgroundTask" />

### <a name="wkapplicationrefreshbackgroundtask"></a>WKApplicationRefreshBackgroundTask

는 `WKApplicationRefreshBackgroundTask` 나중에 앱 해제 되도록 예약할 수 있는 일반 작업입니다.

[![](background-tasks-images/update04.png "WKApplicationRefreshBackgroundTask 해제 미래 날짜")](background-tasks-images/update04.png#lightbox)

작업의 런타임 내에서 앱은 복잡 한 타임 라인을 업데이트 하거나를 사용 하 여 필요한 데이터를 `NSUrlSession`인출 하는 등의 모든 종류의 로컬 처리를 수행할 수 있습니다.


<a name="WKURLSessionRefreshBackgroundTask" />

### <a name="wkurlsessionrefreshbackgroundtask"></a>WKURLSessionRefreshBackgroundTask

데이터가 다운로드를 마치고 앱 `WKURLSessionRefreshBackgroundTask` 에서 처리할 준비가 되 면 시스템에서을 (를) 보냅니다.

[![](background-tasks-images/update05.png "데이터 다운로드가 완료 되 면 WKURLSessionRefreshBackgroundTask")](background-tasks-images/update05.png#lightbox)

백그라운드에서 데이터를 다운로드 하는 동안에는 앱이 실행 되지 않습니다. 대신, 앱은 데이터에 대 한 요청을 예약 하 고, 일시 중단 되 고, 시스템이 데이터 다운로드를 처리 하 고, 다운로드가 완료 되 면 앱만 reawakening 합니다.

<a name="WKSnapshotRefreshBackgroundTask" />

### <a name="wksnapshotrefreshbackgroundtask"></a>WKSnapshotRefreshBackgroundTask

WatchOS 3에서 Apple은 사용자가 즐겨 찾는 앱을 고정 하 고 신속 하 게 액세스할 수 있는 도크를 추가 했습니다. 사용자가 Apple Watch의 측면 단추를 누르면 고정 된 앱 스냅숏의 갤러리가 표시 됩니다. 사용자는 왼쪽 또는 오른쪽으로 살짝 밀어 원하는 앱을 찾은 다음 앱을 탭 하 여 실행 중인 앱의 인터페이스로 스냅숏을 바꿀 수 있습니다.

[![](background-tasks-images/update06.png "스냅숏을 실행 중인 앱 인터페이스로 바꾸기")](background-tasks-images/update06.png#lightbox)

시스템은를 `WKSnapshotRefreshBackgroundTask`전송 하 여 앱 UI의 스냅숏을 주기적으로 가져오고 해당 스냅숏을 사용 하 여 도크를 채웁니다. watchOS를 사용 하면이 스냅숏이 만들어지기 전에 앱에서 콘텐츠 및 UI를 업데이트할 수 있습니다.

스냅숏은 앱에 대 한 미리 보기 및 시작 이미지 모두로 작동 하므로 watchOS 3에서 매우 중요 합니다. 사용자가 도크에서 앱에 대 한 작업을 수행 하는 경우 전체 화면으로 확장 되 고, 포그라운드를 입력 하 고 실행을 시작 하므로 스냅숏이 최신 상태를 유지 해야 합니다.

[![](background-tasks-images/update07.png "사용자가 도크에서 앱을 사용 하는 경우 전체 화면으로 확장 됩니다.")](background-tasks-images/update07.png#lightbox)

다시 말하지만 스냅숏이 만들어지기 전에 앱이 `WKSnapshotRefreshBackgroundTask` 데이터와 UI를 업데이트 하 여 준비할 수 있도록 시스템에서를 실행 합니다.

[![](background-tasks-images/update08.png "응용 프로그램은 스냅숏이 만들어지기 전에 데이터 및 UI를 업데이트 하 여 준비할 수 있습니다.")](background-tasks-images/update08.png#lightbox)

앱이 `WKSnapshotRefreshBackgroundTask` 완료를 표시 하면 시스템은 자동으로 앱의 UI에 대 한 스냅숏을 만듭니다.

> [!IMPORTANT]
> 앱에서 새 데이터를 수신 하 `WKSnapshotRefreshBackgroundTask` 고 해당 사용자 인터페이스를 업데이트 한 후에는 항상를 예약 하는 것이 중요 합니다. 그렇지 않으면 사용자에 게 수정 된 정보가 표시 되지 않습니다.




또한 사용자가 앱에서 알림을 수신 하 고 탭 하 여 응용 프로그램을 포그라운드로 가져올 때 스냅숏은 시작 화면으로 작동 하므로 최신 상태 여야 합니다.

[![](background-tasks-images/update09.png "사용자는 앱에서 알림을 수신 하 고 탭 하 여 앱을 포그라운드로 가져옵니다.")](background-tasks-images/update09.png#lightbox)

사용자가 watchOS 앱과 상호 작용 하 여 1 시간 이상 경과 된 경우에는 기본 상태로 돌아갈 수 있습니다. 기본 상태는 앱의 디자인에 따라 다른 앱에 대 한 다양 한 작업을 의미할 수 있으며, 기본 상태가 전혀 없을 수도 있습니다.

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask" />

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>WKWatchConnectivityRefreshBackgroundTask

WatchOS 3에서 Apple에는 새로운 `WKWatchConnectivityRefreshBackgroundTask`를 통해 백그라운드 새로 고침 API와의 통합 된 조사식 연결이 있습니다. 이 새로운 기능을 사용 하 여 iPhone 앱은 watchOS 앱이 백그라운드에서 실행 되는 동안 해당 watch 앱에 새로운 데이터를 전달할 수 있습니다.

[![](background-tasks-images/update10.png "IPhone 앱은 watchOS 앱이 백그라운드에서 실행 되는 동안 새 데이터를 해당 시청 앱에 전달할 수 있습니다.")](background-tasks-images/update10.png#lightbox)

복잡 한 푸시, 앱 컨텍스트를 시작 하거나 iPhone 앱에서 사용자 정보를 업데이트 하는 것은 백그라운드에서 Apple Watch 앱의 절전 모드를 해제 합니다.

Watch 앱을 통해 `WKWatchConnectivityRefreshBackgroundTask` 해제 하는 경우 iPhone 앱에서 데이터를 수신 하려면 표준 API 메서드를 사용 해야 합니다.

[![](background-tasks-images/update11.png "WKWatchConnectivityRefreshBackgroundTask 데이터 흐름")](background-tasks-images/update11.png#lightbox)

1. 세션이 활성화 되었는지 확인 합니다.
2. 새 `HasContentPending` 속성을 모니터링 합니다. 값이 이면 `true`앱에 처리할 데이터가 여전히 있습니다. 이전 처럼 앱은 모든 데이터 처리를 마칠 때까지 작업을 유지 해야 합니다.
3. 처리할 데이터 (`HasContentPending = false`)가 더 이상 없는 경우에는 작업을 완료로 표시 하 여 시스템에 반환 합니다. 이 작업을 수행 하지 못하면 응용 프로그램의 할당 된 백그라운드 런타임이 고갈 되어 충돌 보고서가 발생 합니다.

<a name="The-Background-API-Lifecycle" />

## <a name="the-background-api-lifecycle"></a>백그라운드 API 수명 주기

새 백그라운드 작업 API의 모든 부분을 함께 배치 하는 일반적인 상호 작용 집합은 다음과 같습니다.

[![](background-tasks-images/update12.png "백그라운드 API 수명 주기")](background-tasks-images/update12.png#lightbox)

1. 먼저 watchOS 앱은 나중에 특정 시점으로 중지 되어 하는 백그라운드 작업을 예약 합니다.
2. 앱은 시스템에 의해 해제 작업을 보냅니다.
3. 앱은 작업을 처리 하 여 필요한 모든 작업을 완료 합니다.
4. 작업 처리의 결과로 앱은 나중에 추가 작업을 완료 하기 위해 더 많은 백그라운드 작업을 예약 해야 할 수 있습니다. 예를 들어를 `NSUrlSession`사용 하 여 더 많은 콘텐츠를 다운로드할 수 있습니다.
5. 앱은 작업을 완료 된 것으로 표시 하 고 시스템에 반환 합니다.

<a name="Using-Resources-Responsibly" />

## <a name="using-resources-responsibly"></a>리소스 사용

WatchOS 앱은 시스템의 공유 리소스에 대 한 드레이닝을 제한 하 여이 에코 시스템 내에서 매우 중요 하 게 동작 합니다.

다음 시나리오를 살펴보세요.

[![](background-tasks-images/update13.png "WatchOS 앱은 시스템의 공유 리소스에 대 한 드레이닝을 제한 합니다.")](background-tasks-images/update13.png#lightbox)

1. 사용자가 1:00 PM에 watchOS 앱을 시작 합니다.
2. 앱은 1 시간 오후 2:00에 새 콘텐츠를 절전 모드에서 해제 하 고 다운로드 하는 작업을 예약 합니다.
3. 오후 1:50 시 사용자는 앱을 다시 열어이 시점에서 해당 데이터와 UI를 업데이트할 수 있도록 합니다.
4. 앱이 10 분 후에 다시 앱을 절전 모드에서 해제 하는 대신 1 시간 오후 2:50에 실행 되도록 작업을 다시 예약 해야 합니다.

모든 앱이 서로 다르지만 Apple에서는 시스템 리소스를 절약 하기 위해 위에 표시 된 것과 같은 사용 패턴을 찾을 것을 제안 합니다.

<a name="Implementing-Background-Tasks" />

## <a name="implementing-background-tasks"></a>백그라운드 작업 구현

예를 들어이 문서는 사용자에 게 축구 점수를 보고 하는 가짜 MonkeySoccer 스포츠 앱을 사용 합니다. 

일반적인 사용 시나리오는 다음과 같습니다.

[![](background-tasks-images/update14.png "일반적인 사용 시나리오")](background-tasks-images/update14.png#lightbox)

사용자가 즐겨 사용 하는 축구 팀이 7:00 PM에서 9:00 PM으로 큰 일치 항목을 재생 하 여 앱이 사용자가 점수를 정기적으로 확인 하 고 30 분의 업데이트 간격을 결정 하는 것으로 간주 해야 합니다.

1. 사용자가 앱을 열고 30 분 후에 백그라운드 업데이트에 대 한 작업을 예약 합니다. 백그라운드 API를 사용 하면 지정 된 시간에 하나의 백그라운드 작업을 실행할 수 있습니다.
2. 앱은 작업을 수신 하 고 해당 데이터와 UI를 업데이트 한 다음 30 분 후에 다른 백그라운드 작업을 예약 합니다. 개발자는 다른 백그라운드 작업을 예약 하는 것을 기억 하는 것이 중요 합니다. 그렇지 않으면 앱이 더 이상 업데이트를 awoken 수 없습니다.
3. 다시, 앱은 작업을 수신 하 고 해당 데이터를 업데이트 하 고, UI를 업데이트 하 고, 30 분 후에 다른 백그라운드 작업을 예약 합니다.
4. 동일한 프로세스가 다시 반복 됩니다.
5. 마지막 백그라운드 작업을 받고 앱에서 해당 데이터와 UI를 다시 업데이트 합니다. 이 점수는 최종 점수 이므로 새 백그라운드 새로 고침을 예약 하지 않습니다. 

<a name="Scheduling-for-Background-Update" />

## <a name="scheduling-for-background-update"></a>백그라운드 업데이트 예약

위의 시나리오에서 MonkeySoccer 앱은 다음 코드를 사용 하 여 백그라운드 업데이트를 예약할 수 있습니다.

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

앱이 awoken 될 `NSDate` 때 30 분 후에 새 30 분이 만들어지고 요청 된 작업의 세부 `NSMutableDictionary` 정보를 저장할을 만듭니다. 의 메서드는 예약 된 작업을 요청 하는 데 사용 됩니다.`SharedExtension` `ScheduleBackgroundRefresh`

요청 된 작업을 예약할 `NSError` 수 없는 경우 시스템은을 반환 합니다.

<a name="Processing-the-Update" />

## <a name="processing-the-update"></a>업데이트 처리

다음으로 점수를 업데이트 하는 데 필요한 단계를 보여 주는 5 분 창에 대해 자세히 살펴보겠습니다.

[![](background-tasks-images/update15.png "점수를 업데이트 하는 데 필요한 단계를 보여 주는 5 분 창")](background-tasks-images/update15.png#lightbox)

1. 오후 7:30:02 시 앱은 시스템에 의해 활성화 되며 업데이트 백그라운드 작업을 제공 합니다. 첫 번째 우선 순위는 서버에서 최신 점수를 가져오는 것입니다. 아래 [NSUrlSession 예약을](#Scheduling-a-NSUrlSession) 참조 하세요.
2. 7:30:05에 앱이 원래 작업을 완료 하면 시스템은 응용 프로그램을 절전 모드로 전환 하 고 요청 된 데이터를 백그라운드에서 계속 다운로드 합니다.
3. 시스템이 다운로드를 완료 하면 다운로드 한 정보를 처리할 수 있도록 앱의 절전 모드를 해제 하는 새 작업을 만듭니다. [백그라운드 작업 처리](#Handling-Background-Tasks) 및 아래의 [다운로드 완료 처리](#Handling-the-Download-Completing) 를 참조 하세요. 
4. 앱은 업데이트 된 정보를 저장 하 고 작업이 완료 된 것으로 표시 합니다. 지금은 개발자가 앱의 사용자 인터페이스를 업데이트 하려고 할 수 있지만 Apple에서 해당 프로세스를 처리 하는 스냅숏 작업 예약을 제안 합니다. 아래 [스냅숏 업데이트 예약을](#Scheduling-a-Snapshot-Update) 참조 하세요.
5. 앱은 스냅숏 작업을 수신 하 고, 사용자 인터페이스를 업데이트 하 고, 작업이 완료 된 것으로 표시 합니다. 아래의 [스냅숏 업데이트 처리를](#Handling-a-Snapshot-Update) 참조 하세요.

<a name="Scheduling-a-NSUrlSession" />

## <a name="scheduling-a-nsurlsession"></a>NSUrlSession 예약

다음 코드를 사용 하 여 최신 점수 다운로드를 예약할 수 있습니다.

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

새 `NSUrlSession`를 구성 하 고 만든 다음 해당 세션을 사용 하 여 메서드를 `CreateDownloadTask` 통해 새 다운로드 작업을 만듭니다. 다운로드 작업의 `Resume` 메서드를 호출 하 여 세션을 시작 합니다.

<a name="Handling-Background-Tasks" />

## <a name="handling-background-tasks"></a>백그라운드 작업 처리

의 메서드`HandleBackgroundTasks` 를 재정의 하 여 앱에서 들어오는 백그라운드 작업 을처리할수있습니다.`WKExtensionDelegate`

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

메서드 `HandleBackgroundTasks` 는 시스템에서를 검색 하는 모든 작업 (에서 `backgroundTasks`)을 `WKUrlSessionRefreshBackgroundTask`순환 합니다. 이 항목이 발견 되 면 세션에 다시 연결 하 고를 `NSUrlSessionDownloadDelegate` 연결 하 여 다운로드 완료를 처리 합니다 (아래의 [다운로드 완료 처리](#Handling-the-Download-Completing) 참조).

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

컬렉션에 추가 하 여 완료 될 때까지 태스크에 대 한 핸들을 유지 합니다.

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

앱에 전송 되는 모든 작업을 완료 해야 하며, 현재 처리 중인 모든 작업에 대해 완료로 표시 합니다.

```csharp
if (urlTask != null) {
  ...
} else {
  // Ensure that all tasks are completed
  task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing" />

## <a name="handling-the-download-completing"></a>다운로드 완료 처리

Monkeysoccer 앱은 다음 `NSUrlSessionDownloadDelegate` 대리자를 사용 하 여 다운로드 완료 및 요청 된 데이터 처리를 처리 합니다.

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

초기화 되 면 `ExtensionDelegate` `WKRefreshBackgroundTask` 및이를 생성 한에 대 한 핸들을 유지 합니다. 메서드를 `DidFinishDownloading` 재정의 하 여 다운로드 완료를 처리 합니다. `CompleteTask` 는`ExtensionDelegate` 의 메서드를 사용 하 여 작업을 완료 했음을 알리고 보류 중인 작업의 컬렉션에서 제거 합니다. 위의 [백그라운드 작업 처리](#Handling-Background-Tasks) 를 참조 하세요.

<a name="Scheduling-a-Snapshot-Update" />

## <a name="scheduling-a-snapshot-update"></a>스냅숏 업데이트 예약

다음 코드를 사용 하 여 스냅숏 작업에서 최신 점수를 사용 하 여 UI를 업데이트할 수 있도록 예약할 수 있습니다.

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

위의 메서드와 마찬가지로, 앱을 awoken 하 고 `NSDate` 요청 된 작업의 세부 정보를 저장 하는 `NSMutableDictionary` 를 만드는 데 사용할 새을 만듭니다. `ScheduleURLUpdateSession` 의 메서드는 예약 된 작업을 요청 하는 데 사용 됩니다.`SharedExtension` `ScheduleSnapshotRefresh`

요청 된 작업을 예약할 `NSError` 수 없는 경우 시스템은을 반환 합니다.

<a name="Handling-a-Snapshot-Update" />

## <a name="handling-a-snapshot-update"></a>스냅숏 업데이트 처리

스냅숏 작업을 처리 하기 위해 ( `HandleBackgroundTasks` 위의 [백그라운드 작업 처리](#Handling-Background-Tasks) 참조) 메서드는 다음과 같이 수정 됩니다.

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

메서드는 처리 중인 작업의 형식을 테스트 합니다. `WKSnapshotRefreshBackgroundTask` 인 경우 작업에 액세스할 수 있습니다.

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

메서드는 사용자 인터페이스를 업데이트 한 후 스냅숏이 `NSDate` 유효 하지 않을 때 시스템에 알리기 위해를 만듭니다. 사용자 정보를 `NSMutableDictionary` 사용 하 여를 만들어 새 스냅숏을 설명 하 고 스냅숏 태스크가 완료 된 것을이 정보로 표시 합니다.

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

또한 앱이 기본 상태 (첫 번째 매개 변수)로 반환 되지 않는다는 것을 스냅숏 작업에 지시 합니다. 기본 상태에 대 한 개념이 없는 앱은 항상이 속성을로 `true`설정 해야 합니다.

<a name="Working-Efficiently" />

## <a name="working-efficiently"></a>효율적으로 작업

WatchOS 3 백그라운드 작업을 효율적으로 사용 하 고 사용 하 여 MonkeySoccer 앱이 점수를 업데이트 하는 데 걸린 5 분 창의 위 예제와 같이 앱은 총 15 초 동안만 활성화 되었습니다. 

[![](background-tasks-images/update16.png "앱이 총 15 초 동안만 활성 상태 였습니다.")](background-tasks-images/update16.png#lightbox)

이렇게 하면 앱이 사용 가능한 Apple Watch 리소스 및 배터리 수명 모두에 미치는 영향을 줄일 수 있으며,이를 통해 앱이 시청에서 실행 되는 다른 앱과 더 원활 하 게 작동할 수 있습니다.

<a name="How-Scheduling-Works" />

## <a name="how-scheduling-works"></a>예약 작동 방법

WatchOS 3 앱은 포그라운드로 실행 되는 동안 항상 실행 되도록 예약 되며, 데이터 업데이트 또는 UI 다시 그리기와 같은 필요한 모든 종류의 처리 작업을 수행할 수 있습니다. 앱이 백그라운드로 이동 하면 일반적으로 시스템에서 일시 중단 되 고 모든 런타임 작업이 중단 됩니다. 

앱은 백그라운드에 있지만 특정 작업을 신속 하 게 실행 하기 위해 시스템에서 대상으로 지정할 수 있습니다. 따라서 watchOS 2에서 시스템은 백그라운드 앱을 일시적으로 절전 모드에서 해제 하 여 긴 모양 알림 처리 또는 앱의 복잡 한 업데이트 등의 작업을 수행할 수 있습니다. WatchOS 3에는 앱을 백그라운드에서 실행할 수 있는 몇 가지 새로운 방법이 있습니다.

앱이 백그라운드에 있는 동안 시스템은 몇 가지 제한 사항을 적용 합니다.

- 지정 된 작업을 완료 하는 데 몇 초 밖에 안 걸립니다. 시스템은 전달 된 시간 뿐만 아니라 앱이이 제한을 파생 시키는 데 소비 하는 CPU의 양을 고려 합니다.
- 제한을 초과 하는 앱은 다음 오류 코드와 함께 중단 됩니다.
  - **CPU** - 0xc51bad01
  - **시간** -0xc51bad02
- 시스템은 앱에서 수행 하도록 요청 하는 백그라운드 작업의 유형에 따라 다른 한도를 적용 합니다. 예 `WKApplicationRefreshBackgroundTask` 를 들어 및 `WKURLSessionRefreshBackgroundTask` 작업에는 다른 유형의 백그라운드 작업에 대해 약간 더 긴 런타임이 제공 됩니다.

<a name="Complications-and-App-Updates" />

### <a name="complications-and-app-updates"></a>복잡 한 앱 업데이트

Apple에서 watchOS 3에 추가 된 새로운 백그라운드 작업 외에도, watchOS 앱의 경우 앱이 백그라운드 업데이트를 받는 방법과 시기에 영향을 줄 수 있습니다.

유용한 정보를 한 눈에 파악할 수 있는 작은 시각적 요소는 복잡 합니다. 선택 된 조사식에 따라 사용자가 watchOS 3의 watch 앱에서 제공 하는 하나 이상의 복잡 한 조사식을 사용자 지정할 수 있습니다.

사용자가 응용 프로그램의 감시에 대 한 복잡 한 문제 중 하나를 포함 하는 경우 앱은 다음과 같은 업데이트 된 이점을 제공 합니다.

- 이를 통해 시스템은 백그라운드에서 앱을 시작 하 고, 메모리에 보관 하 고, 업데이트 하는 데 추가 시간을 제공 하는 즉시 시작 상태로 응용 프로그램을 유지할 수 있습니다.
- 매우 복잡 한 일은 하루에 최소 50의 푸시 업데이트를 보장 합니다.

개발자는 사용자가 위에 나열 된 이유 때문에 사용자가 감시에 추가 하는 것을 유도 하기 위해 앱에 대 한 매우 복잡 한 상황을 만들기 위해 노력 해야 합니다.

WatchOS 2에서는 응용 프로그램이 백그라운드에서 런타임을 수신 하는 기본 방법입니다. WatchOS 3에서는 복잡 한 앱이 시간당 여러 업데이트를 받을 수 있도록 보장 되지만를 사용 `WKExtensions` 하 여 더 많은 런타임을 요청 하 여 복잡 한 문제를 업데이트할 수 있습니다.

연결 된 iPhone 앱에서 복잡 한 사항을 업데이트 하는 데 사용 되는 다음 코드를 살펴보세요.

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

`RemainingComplicationUserInfoTransfers` 의 속성을 사용 하 여 앱에서 하루 동안 남은 우선 순위 전송 수를 확인 한 다음 해당 숫자를 기반으로 작업을 수행 합니다. `WCSession` 앱이 낮은 전송 실행을 시작 하면 사소한 업데이트를 보낼 때 보류 하 고 중요 한 변경 내용이 있는 경우에만 정보를 보낼 수 있습니다.

<a name="Scheduling-and-Dock" />

### <a name="scheduling-and-the-dock"></a>예약 및 도크

WatchOS 3에서 Apple은 사용자가 즐겨 찾는 앱을 고정 하 고 신속 하 게 액세스할 수 있는 도크를 추가 했습니다. 사용자가 Apple Watch의 측면 단추를 누르면 고정 된 앱 스냅숏의 갤러리가 표시 됩니다. 사용자는 왼쪽 또는 오른쪽으로 살짝 밀어 원하는 앱을 찾은 다음 앱을 탭 하 여 실행 중인 앱의 인터페이스로 스냅숏을 바꿀 수 있습니다.

[![](background-tasks-images/dock01.png "도크")](background-tasks-images/dock01.png#lightbox)

시스템은 정기적으로 앱 UI의 스냅숏을 사용 하 고 이러한 스냅숏을 사용 하 여 문서를 채웁니다. watchOS를 사용 하면이 스냅숏이 만들어지기 전에 앱에서 콘텐츠 및 UI를 업데이트할 수 있습니다.

도킹에 고정 된 앱은 다음을 예측할 수 있습니다.

- 시간당 업데이트를 하나 이상 받게 됩니다. 여기에는 응용 프로그램 새로 고침 작업과 스냅숏 태스크가 모두 포함 됩니다.
- 업데이트 예산이 도크의 모든 앱 간에 배포 됩니다. 따라서 사용자가 고정 한 앱의 수를 줄이면 각 앱에서 받게 될 가능성이 더 높은 업데이트가 제공 됩니다.
- 앱이 도킹에서 선택 될 때 앱이 신속 하 게 다시 시작 되도록 메모리에 보관 됩니다.

사용자가 실행 한 마지막 앱은 _가장 최근에 사용_ 된 앱으로 간주 되며 도크에서 마지막 슬롯을 차지 하 게 됩니다. 여기에서 사용자는 항구에 영구적으로 고정 하도록 선택할 수 있습니다. 가장 최근에 사용한 것은 사용자가 도킹에 이미 고정 한 다른 즐겨 찾는 앱 처럼 처리 됩니다.

> [!IMPORTANT]
> 홈 화면에만 추가 된 앱에는 정기적인 일정이 제공 되지 않습니다. 정기적인 일정 예약 및 백그라운드 업데이트를 받으려면 앱을 도크에 추가 _해야 합니다_ .

이 문서 앞부분에서 설명한 것 처럼 스냅숏은 앱에 대 한 미리 보기 및 시작 이미지 모두로 작동 하므로 watchOS 3에서 매우 중요 합니다. 사용자가 도크에서 앱에 대 한 작업을 하는 경우, 전체 화면으로 확장 되 고, 포그라운드를 입력 하 고 실행을 시작 하므로 스냅숏이 최신 상태를 유지 해야 합니다.

시스템이 응용 프로그램의 UI에 대 한 새 스냅숏이 필요한 것으로 결정 하는 경우가 있을 수 있습니다. 이 경우 스냅숏 요청은 앱의 런타임 예산에 대해 계산 되지 않습니다. 다음은 시스템 스냅숏 요청을 트리거합니다.

- 복잡 한 타임 라인 업데이트.
- 앱의 알림과의 사용자 상호 작용
- 포그라운드에서 백그라운드 상태로 전환
- 1 시간이 백그라운드 상태가 된 후에 앱이 기본 상태로 돌아갈 수 있습니다.
- WatchOS 처음 부팅 될 때.

<a name="Best-Practices" />

## <a name="best-practices"></a>최선의 구현 방법 

Apple에서는 백그라운드 작업을 수행할 때 다음과 같은 모범 사례를 제안 합니다.

- 앱을 업데이트 해야 하는 빈도를 예약 합니다. 앱이 실행 될 때마다 미래의 요구를 재평가 하 고 필요에 따라이 일정을 조정 해야 합니다.
- 시스템이 백그라운드 새로 고침 작업을 보내고 앱에 업데이트가 필요 하지 않은 경우 업데이트가 실제로 필요할 때까지 작업을 연기 합니다.
- 앱에서 사용할 수 있는 모든 런타임 기회를 고려 합니다.
  - 도킹 및 포그라운드 활성화.
  - 알림을.
  - 복잡 한 업데이트.
  - 백그라운드 새로 고침
- 다음과 `ScheduleBackgroundRefresh` 같은 범용 백그라운드 런타임에 사용 합니다.
  - 정보에 대 한 시스템 폴링.
  - 배경 데이터 `NSURLSessions` 를 요청 하는 미래를 예약 합니다. 
  - 알려진 시간 전환.
  - 복잡 한 업데이트를 트리거합니다.

<a name="Snapshot-Best-Practices" />

## <a name="snapshot-best-practices"></a>스냅숏 모범 사례

스냅숏 업데이트를 사용 하는 경우 Apple은 다음과 같은 제안 사항을 만듭니다.

- 필요한 경우에만 스냅숏을 무효화 합니다. 예를 들어 중요 한 내용이 변경 된 경우에 해당 합니다.
- 빈도가 높은 스냅숏 무효화를 방지 합니다. 예를 들어 타이머 앱은 초 마다 스냅숏을 업데이트 해서는 안 됩니다. 타이머가 종료 된 경우에만 수행 해야 합니다.

<a name="App-Data-Flow" />

## <a name="app-data-flow"></a>앱 데이터 흐름

Apple 데이터 흐름을 사용 하려면 다음을 제안 합니다.

[![](background-tasks-images/update17.png "앱 데이터 흐름 다이어그램")](background-tasks-images/update17.png#lightbox)

외부 이벤트 (예: 연결 감시)는 앱을 절전 모드에서 해제 합니다. 이렇게 하면 앱이 해당 데이터 모델 (앱 현재 상태를 나타냄)을 강제로 업데이트 합니다. 데이터 모델 변경의 결과로 앱은 해당 문제를 업데이트 하 고, 새 스냅숏을 요청 하 고, 더 많은 데이터를 가져오고 추가 `NSURLSession` 백그라운드 새로 고침을 예약 하기 위해 배경을 시작 해야 할 수 있습니다.

<a name="The-App-Lifecycle" />

## <a name="the-app-lifecycle"></a>앱 수명 주기

도킹 및 즐겨 사용 하는 앱을 고정 하는 기능 때문에 Apple은 사용자가 훨씬 더 많은 앱 간을 자주 이동 하는 것으로 간주 합니다. watchOS 2를 사용 하는 것으로 간주 됩니다. 따라서 앱은 이러한 변경을 처리할 준비가 되어 있고 포그라운드 및 배경 상태 사이를 빠르게 이동할 수 있어야 합니다.

Apple의 제안 사항은 다음과 같습니다.

- 포그라운드 활성화를 시작할 때 앱에서 가능한 한 빨리 백그라운드 작업을 완료 하도록 합니다.
- 을 호출 `NSProcessInfo.PerformExpiringActivity`하 여 백그라운드에 들어가기 전에 모든 포그라운드 작업을 완료 해야 합니다.
- WatchOS 시뮬레이터에서 앱을 테스트 하는 경우 응용 프로그램이 기능을 제대로 테스트 하는 데 필요한 만큼 새로 고칠 수 있도록 작업 예산이 적용 되지 않습니다.
- ITunes Connect에 게시 하기 전에 항상 실제 Apple Watch 하드웨어에서 테스트 하 여 앱이 예산을 지난 후에 실행 되지 않도록 합니다.
- Apple은 테스트 및 디버깅 중에 Apple Watch를 충전기에 유지 하는 것을 제안 합니다.
- 콜드 시작 및 앱 다시 시작이 모두 철저 하 게 테스트 되었는지 확인 합니다.
- 모든 앱 태스크가 완료 되는지 확인 합니다.
- 최적/최악의 시나리오를 모두 테스트 하기 위해 도크에 고정 된 앱의 수를 변경 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 watchOS의 향상 된 기능 및이를 사용 하 여 시청 앱을 최신 상태로 유지 하는 방법에 대해 설명 했습니다. 먼저 watchOS 3에 추가 된 Apple의 모든 새 백그라운드 작업에 대해 설명 했습니다. 그런 다음 백그라운드 API 수명 주기와 Xamarin watchOS 앱에서 백그라운드 작업을 구현 하는 방법을 살펴보았습니다. 마지막으로, 예약의 작동 방식에 대해 설명 하 고 몇 가지 모범 사례를 제공 했습니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
