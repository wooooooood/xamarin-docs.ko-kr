---
title: "워크 아웃 앱"
description: "이 문서에서는 이러한 향상 된 기능에 설명 Apple 워크 아웃 앱 watchOS 3 및 Xamarin에서이 구현 하는 방법에 적용 했습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 2282a340811d9932f9df3a1343b22ffc35247e54
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="workout-apps"></a>워크 아웃 앱

_이 문서에서는 이러한 향상 된 기능에 설명 Apple 워크 아웃 앱 watchOS 3 및 Xamarin에서이 구현 하는 방법에 적용 했습니다._


새로운 3 watchOS 워크 아웃 관련 응용 프로그램에서 실행 하는 백그라운드에서 Apple Watch 고 HealthKit 데이터에 액세스할 수 있습니다. 해당 부모 iOS 10 기반 앱에는 다음과 같은 사용자 개입 없이 watchOS 3 기반 응용 프로그램을 실행 하는 기능.

다음 항목에 대해 자세히 설명합니다.

## <a name="about-workout-apps"></a>워크 아웃 앱에 대 한

상태 및 적합성에 대 한 목표 방향으로 하루 중 몇 시간 감수할의 적합성 및 워크 아웃 응용 프로그램의 사용자가 항상 전용 수 있습니다. 결과적으로, 정확 하 게 수집 하 고 데이터를 표시 하 고는 Apple 상태와 원활 하 게 통합 응답을 사용 하기 쉬운 앱 기대 합니다.

잘 디자인 된 적합성 또는 워크 아웃 앱 사용자가 해당 작업의 적합성에 대 한 목표에 도달 하도록 차트를 수 있습니다. Apple Watch 사용 하 여 적합성 및 워크 아웃 응용 프로그램에 바로 액세스할 심 박수 칼로리 및 활동 검색 합니다.

[![](workout-apps-images/workout01.png "적합성 및 워크 아웃 응용 프로그램 예제")](workout-apps-images/workout01.png#lightbox)

WatchOS 3, 새 _배경 실행_ 제공 워크 아웃 관련 응용 프로그램에서 Apple Watch 백그라운드에서 실행 하 고 HealthKit 데이터에 액세스할 수 있습니다.

이 문서는 배경 실행 기능을 소개, 워크 아웃 앱 수명 주기를 포괄 및 워크 아웃 응용 프로그램을 사용자의 기여할 수는 어떻게 표시 _활동 링_ Apple Watch 있습니다.

## <a name="about-workout-sessions"></a>워크 아웃 세션에 대 한

모든 워크 아웃 응용 프로그램의 핵심은는 _워크 아웃 세션_ (`HKWorkoutSession`) 있는 사용자를 시작 하 고 중지할 수 있습니다. 워크 아웃 세션 API 구현 하기 쉬운 및와 같은 워크 아웃 응용 프로그램에 여러 가지 이점을 제공:

- 동작 및 칼로리 활동 형식에 따라 검색을 진행 합니다.
- 사용자의 활동 링에 대 한 자동 구성 정보
- 세션에서 작업 하는 동안 응용 프로그램 자동으로 때마다 표시 됩니다는 사용자 (하거나 자신의 손목 발생 하거나 Apple Watch 상호 작용) 장치를 절전 모드 해제 합니다.

## <a name="about-background-running"></a>배경 실행에 대 한

위에서 설명한 대로 백그라운드에서 실행 되도록 watchOS 3와 워크 아웃 앱을 설정할 수 있습니다. 배경 워크 아웃 응용 프로그램 실행을 사용 하 여 백그라운드에서 실행 하는 동안 Apple Watch 센서에서 데이터를 처리할 수 있습니다. 예를 들어 응용 프로그램을 계속 모니터링할 수는 사용자의 심 박수 더 이상 화면에 표시 되는 경우에 합니다.

또한 배경 실행 haptic 현재 진행 상황을 사용자에 게 알리는 경고를 보내는 것과 같은 활성 워크 아웃 세션 중 언제 든 지 사용자에 게 실시간 피드백을 제공 하는 기능을 제공 합니다.

또한 배경 실행 응용 프로그램을 신속 하 게 되므로 사용자의 Apple Watch 신속 하 게 내용을 최신 데이터는 해당 사용자 인터페이스를 업데이트할 수 있습니다.

Apple Watch 높은 성능을 유지 하기 위해 백그라운드에서 실행을 사용 하 여 watch 앱 배터리를 절약 하기 위해 백그라운드 작업의 양을 제한 해야 합니다. 응용 프로그램은 백그라운드에서 과도 한 CPU를 사용 하는 경우 watchOS 하 여 일시 중단 얻을 수 있습니다.

### <a name="enabling-background-running"></a>실행 하는 백그라운드 활성화

백그라운드에서 실행을 사용 하도록 설정 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**, 조사식 확장 도우미 iPhone 앱을 두 번 클릭 `Info.plist` 편집을 위해 열 파일입니다.
2. 전환 하는 **소스** 보기: 

    [![](workout-apps-images/plist01.png "소스 뷰")](workout-apps-images/plist01.png#lightbox)
3. 라는 새 키를 추가 `WKBackgroundModes` 설정 하 고는 **형식** 를 `Array`: 

    [![](workout-apps-images/plist02.png "WKBackgroundModes 라는 새 키를 추가 합니다.")](workout-apps-images/plist02.png#lightbox)
4. 배열에 새 항목 추가 **형식** 의 `String` 값 `workout-processing`: 

    [![](workout-apps-images/plist03.png "문자열 형식 및 처리 워크 아웃의 값을 가진 배열에 새 항목 추가")](workout-apps-images/plist03.png#lightbox)
5. 파일의 변경 내용을 저장합니다.

## <a name="starting-a-workout-session"></a>워크 아웃 세션 시작

워크 아웃 세션을 시작 하는 세 가지 주요 단계가 있습니다.

[![](workout-apps-images/workout02.png "워크 아웃 세션을 시작 하는 세 가지 주요 단계")](workout-apps-images/workout02.png#lightbox)

1. 응용 프로그램 데이터 HealthKit에 액세스 하려면 권한 부여를 요청 해야 합니다.
2. 시작 되 고 워크 아웃 형식에 대 한 워크 아웃 구성 개체를 만듭니다.
3. 페이지를 만들고 새로 만든된 워크 아웃 구성을 사용 하 여 워크 아웃 세션을 시작 합니다.

### <a name="requesting-authorization"></a>권한 부여를 요청합니다.

응용 프로그램 사용자의 HealthKit 데이터에 액세스 하기 전에 요청 하 고 사용자 로부터 권한 부여를 수신 해야 것입니다. 워크 아웃 응용 프로그램의 특성에 따라 다음과 같은 유형의 요청 만들므로 될 수 있습니다.

- 데이터를 쓰는 권한 부여:
    - 체력 단련
- 데이터를 읽을 권한 부여:
    - 소비 되는 에너지
    - 거리
    - 심 박수  

응용 프로그램 권한 부여를 요청할 수 있습니다, 전에 HealthKit에 액세스 하 여 설정 해야 합니다.

다음을 수행합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 `Entitlements.plist` 파일을 두 번 클릭하여 엽니다.
2. 아래쪽으로 스크롤하여 확인 **사용 HealthKit**: 

    [![](workout-apps-images/auth01.png "Enable HealthKit 확인")](workout-apps-images/auth01.png#lightbox)
3. 파일의 변경 내용을 저장합니다.
4. 지침에 따라는 [명시적 앱 ID 및 프로비저닝 프로필](~/ios/platform/healthkit.md) 및 [앱 ID 및 프로비저닝 프로필와 Xamarin.iOS 앱 연결](~/ios/platform/healthkit.md) 의 섹션은 [소개 HealthKit](~/ios/platform/healthkit.md) 올바르게 앱 프로 비전 하는 문서입니다.
5. 지침을 사용 하 여 마지막으로 [상태 키트 프로그래밍](~/ios/platform/healthkit.md) 및 [the 사용자에서 권한을 요청](~/ios/platform/healthkit.md) 의 섹션은 [HealthKit 소개](~/ios/platform/healthkit.md) 요청에 대 한 문서 사용자의 HealthKit 데이터 저장소에 액세스할 수 있는 권한입니다.

### <a name="setting-the-workout-configuration"></a>워크 아웃 구성 설정

워크 아웃 구성 개체를 사용 하 여 워크 아웃 세션이 만들어지지 않습니다 (`HKWorkoutConfiguration`) 워크 아웃 형식을 지정 하는 (같은 `HKWorkoutActivityType.Running`) 및 워크 아웃 위치 (같은 `HKWorkoutSessionLocationType.Outdoor`):

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
    ActivityType = HKWorkoutActivityType.Running,
    LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>워크 아웃 세션 대리자 만들기 

워크 아웃 세션 중에 발생할 수 있는 이벤트를 처리 하려면 응용 프로그램 워크 아웃 세션 대리자 인스턴스를 만들려고 할 수 있습니다. 프로젝트에 새 클래스를 추가 하 고 오프 기초는 `HKWorkoutSessionDelegate` 클래스입니다. 옥외 실행의 예제를 그 수는 다음과 같습니다.

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }
        #endregion
    }
}
```

이 클래스 워크 아웃 세션 변경의 상태와 발생할 수 있는 몇 가지 이벤트를 생성 합니다. (`DidChangeToState`) 워크 아웃 세션이 실패 하는 경우 (`DidFail`). 

### <a name="creating-a-workout-session"></a>워크 아웃 세션 만들기

새 워크 아웃 세션을 만들고 사용자의 기본 HealthKit 저장소에 대해 시작 하려면 위에서 만든 워크 아웃 구성 및 워크 아웃 세션 대리자를 사용 하 여:

```csharp
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...

private void StartOutdoorRun ()
{
    // Create a workout configuration
    var configuration = new HKWorkoutConfiguration () {
        ActivityType = HKWorkoutActivityType.Running,
        LocationType = HKWorkoutSessionLocationType.Outdoor
    };

    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (configuration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

앱이 워크 아웃 세션을 시작 하는 경우 사용자가 자신의 watch 화면에 다시 전환 아주 작은 녹색 "man 실행 중" 아이콘 글꼴 위에 표시 됩니다.

[![](workout-apps-images/workout03.png "아주 작은 녹색 실행 중인 매뉴얼 아이콘 글꼴 위에 표시")](workout-apps-images/workout03.png#lightbox)

사용자가이 아이콘을 하는 경우은 돌아갑니다 다시 응용 프로그램.

## <a name="data-collection-and-control"></a>데이터 수집 및 제어

워크 아웃 세션 구성 되어 시작, 되 면 응용 프로그램 (예: 사용자의 심 박수) 세션에 대 한 데이터를 수집 하 고 세션의 상태를 제어 해야 합니다.

[![](workout-apps-images/workout04.png "데이터 수집 및 제어 다이어그램")](workout-apps-images/workout04.png#lightbox)

1. **샘플을 관찰** -응용 프로그램에 따라 처리 되 고 사용자에 게 표시 되는 HealthKit에서 정보를 검색 해야 합니다.
2. **이벤트를 관찰** -앱 HealthKit 하거나 (예: 사용자는 워크 아웃 일시 중지) 응용 프로그램의 UI에서 생성 되는 이벤트에 응답 해야 합니다.
3. **실행 상태가** -세션이 시작 된 하 고 현재 실행 중인 합니다.
4. **일시 중지 됨 상태로 전환** -사용자가 현재 워크 아웃 세션을 일시 중지 하 고 나중에 다시 시작할 수 있습니다. 사용자는 단일 워크 아웃 세션에 여러 번 실행 되 고 일시 중지 된 상태 간에 전환할 수 있습니다.
5. **워크 아웃 세션을 종료** -사용자 수 워크 아웃 세션을 종료 하는 데 어느 시점 하거나 자체적으로 (예: 두 마일 실행) 하는 요금된 워크 아웃 되었으면 종료 고 만료 될 수 있습니다.

마지막 단계는 사용자의 HealthKit 데이터 저장소에 워크 아웃 세션의 결과 저장 하는 것입니다.

### <a name="observing-healthkit-samples"></a>HealthKit 샘플을 관찰합니다.

열려는 응용 프로그램 해야 합니다는 _앵커 개체 쿼리_ HealthKit 데이터의 각 지점에 대 한 관심 있는을 심 박수 또는 활성 에너지 구울 등입니다. 관찰 되 고 각 데이터 요소에 대 한 응용 프로그램에 전송 될 때 새 데이터를 캡처하기 위해 만든 수 업데이트 처리기 할 수 있습니다.

이러한 데이터 요소에서 응용 프로그램 (예: 총 실행된 거리)입니다. 합계는 바로 누적 되므로 하 고 필요에 따라 해당 사용자 인터페이스를 업데이트할 수 있습니다. 또한 응용 프로그램에 도달할 때 특정 목표 또는 성과 같은 다음 마일 실행을 완료 하는 경우 사용자에 알릴 수 있습니다.

다음 샘플 코드를 살펴보겠습니다.

```csharp
private void ObserveHealthKitSamples ()
{
    // Get the starting date of the required samples
    var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

    // Get data from the local device
    var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
    var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

    // Assemble compound predicate
    var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

    // Get ActiveEnergyBurned
    var queryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
        // Valid?
        if (error == null) {
            // Yes, process all returned samples
            foreach (HKSample sample in addedObjects) {
                var quantitySample = sample as HKQuantitySample;
                ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);
            }
            
            // Update User Interface
            ...
        }
    });

    // Start Query
    HealthStore.ExecuteQuery (queryActiveEnergyBurned);
                                          
}
```

시작 날짜를 사용 하 여에 대 한 데이터를 가져와야 하는 것을 설정 하는 조건자를 만들는 `GetPredicateForSamples` 메서드. 사용 하 여 HealthKit 정보를 추출 하는 장치 집합을 만듭니다는 `GetPredicateForObjectsFromDevices` 이 경우 로컬 Apple Watch 메서드 (`HKDevice.LocalDevice`). 두 개의 조건자는 복합 조건자에 결합 됩니다 (`NSCompoundPredicate`)를 사용 하 여 `CreateAndPredicate` 메서드.

새 `HKAnchoredObjectQuery` 필요한 데이터 요소에 대해 생성 됩니다 (이 경우 `HKQuantityTypeIdentifier.ActiveEnergyBurned` 활성 에너지 구울 데이터 요소에 대 한), 반환 된 데이터의 양에 제한이 없습니다 (`HKSampleQuery.NoLimit`) 응용 프로그램에 반환 되 고는 데이터를 처리 하는 업데이트 처리기가 정의 하 고 HealthKit 합니다. 

업데이트 처리기에는 새 데이터는 지정 된 데이터 요소에 대 한 응용 프로그램에 전달 되는 언제 든 지 호출 됩니다. 오류가 반환 되 면 응용 프로그램 수 안전 하 게 데이터를 읽을, 필요한 계산을 확인 및 필요에 따라 해당 UI를 업데이트 합니다.

코드 샘플의 모든 반복 실행 (`HKSample`)에서 반환 되는 `addedObjects` 배열 하 고 수량 샘플으로 캐스팅 (`HKQuantitySample`). 그런 다음 줄을 값으로이 샘플의 double 값을 가져오고 (`HKUnit.Joule`)는 워크 아웃 구워 활성 에너지의 누계를 누적 업데이트 및 사용자 인터페이스를 업데이트 합니다.

### <a name="achieved-goal-notification"></a>목표 달성된 알림

위에서 언급 한 것 처럼 (예: 실행의 첫 번째 마일 완료) 워크 아웃 응용 프로그램에서 목표를 달성 하는 사용자 때 보낼 수 있습니다 haptic 피드백 Taptic 엔진을 통해 사용자에 게 합니다. 사용자 자신의 손목 피드백 메시지가 표시 되는 이벤트를 발생시킬지 것이 더 이후 응용 프로그램의 UI를이 시점에서 업데이트도 해야 합니다.

Haptic 피드백을 재생 하려면 다음 코드를 사용 합니다.

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>이벤트를 관찰합니다.

이벤트는 응용 프로그램 사용자의 워크 아웃 하는 동안 특정 요소를 강조 하는 데 사용할 수 있는 타임 스탬프입니다. 응용 프로그램에서 직접 만든 되 고는 워크 아웃에 저장 된 일부 이벤트 및 HealthKit으로 일부 이벤트를 자동으로 생성 됩니다.

앱 덮어씁니다 이벤트 HealthKit에 의해 만들어진 것을 볼 수는 `DidGenerateEvent` 의 메서드는 `HKWorkoutSessionDelegate`:

```csharp
using System.Collections.Generic;
...

public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Save HealthKit generated event
    WorkoutEvents.Add (@event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Lap:
        break;
    case HKWorkoutEventType.Marker:
        break;
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

Apple이 watchOS 3에서에서 다음과 같은 새 이벤트 유형을 추가 하 고 있습니다.

- `HKWorkoutEventType.Lap` -는 균등 한 거리 부분에서 워크 아웃 분해 하는 이벤트입니다. 예: 하나의 및 실행 하는 동안 추적을 표시 합니다.
- `HKWorkoutEventType.Marker` -내에서 워크 아웃에서 관심 있는 임의의 점이 됩니다. 예를 들어 옥외 실행 경로에 특정 시점에 도달 합니다.

이러한 새로운 형식이 응용 프로그램에서 작성 및 그래프 및 통계를 만드는에서 나중에 사용할 워크 아웃에 저장 합니다.

표식 이벤트를 만들려면 다음을 수행 합니다.

```csharp
using System.Collections.Generic;
...

public float MilesRun { get; set; }
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public void ReachedNextMile ()
{
    // Create and save marker event
    var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
    WorkoutEvents.Add (markerEvent);

    // Notify user
    NotifyUserOfReachedMileGoal (++MilesRun);
}
```

이 코드는 표식 이벤트의 새 인스턴스를 만듭니다 (`HKWorkoutEvent`) 및 이벤트 (나중에 쓰여질 워크 아웃 세션)의 개인 컬렉션에 저장 하 고 haptics 통해 이벤트의 사용자에 게 알립니다.

### <a name="pausing-and-resuming-workouts"></a>일시 중지 및 재개 체력 단련

워크 아웃 세션에서 언제 든 사용자 수 일시 중단 된 워크 아웃 하 고 나중에 다시 시작 합니다. 예를 들어 중요 한 호출을 수행 하 고 호출이 완료 된 후 실행을 다시 시작 하는 실내 실행 일시 중지 수 있습니다.

응용 프로그램의 UI 일시 중지 하 고 사용자가 자신의 작업을 중단 하는 동안 Apple Watch 전원 및 데이터 공간을 절약할 수 있도록 HealthKit 호출) 하는 경우 (여는 워크 아웃을 다시 시작 하는 방법을 제공 해야 합니다. 또한 응용 프로그램 워크 아웃 세션 일시 중지 된 상태에 있을 때 받을 수 있는 모든 새 데이터 요소를 무시 해야 합니다.

HealthKit는 일시 중지 및 일시 중지 및 다시 시작 이벤트를 생성 하 여 호출 계속 응답 합니다. 워크 아웃 세션을 일시 중지 된 동안 새 이벤트 나 데이터 없습니다 보내집니다 응용 프로그램에 HealthKit 하 여 세션을 다시 시작할 때까지.

다음 코드를 사용 하 여 일시 중지 되 고 워크 아웃 세션을 다시 시작 합니다.

```csharp
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public HKWorkoutSession WorkoutSession { get; set;}
...

public void PauseWorkout ()
{
    // Pause the current workout
    HealthStore.PauseWorkoutSession (WorkoutSession);
}

public void ResumeWorkout ()
{
    // Pause the current workout
    HealthStore.ResumeWorkoutSession (WorkoutSession);
}
```

재정의 하 여 HealthKit에서 생성 되는 일시 중지 및 다시 시작 이벤트를 처리할 수 있습니다는 `DidGenerateEvent` 의 메서드는 `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);

    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

### <a name="motion-events"></a>동작 이벤트

또한 새로운 watchOS 3은 일시 중지 동작 (`HKWorkoutEventType.MotionPaused`) 동작 다시 시작 (`HKWorkoutEventType.MotionResumed`) 이벤트입니다. 이러한 이벤트는 발생 자동으로 HealthKit 하 여 실행 중인 워크 아웃 하는 동안 사용자가 시작 하 고 이동을 중지 합니다.

응용 프로그램 동작 일시 중지 이벤트를 받으면 사용자를 다시 시작 동작 및 동작을 다시 시작 이벤트를 받을 때까지 데이터 수집 중지 해야 합니다. 응용 프로그램의 응용 프로그램 일시 중지 동작 이벤트에 대 한 응답에서 워크 아웃 세션을 일시 중지할지 합니다.

> [!IMPORTANT]
> 동작 일시 중지 및 다시 시작 동작 이벤트 RunningWorkout 활동 형식에만 지원 됩니다 (`HKWorkoutActivityType.Running`).

다시, 재정의 하 여 이러한 이벤트를 처리할 수 있습니다는 `DidGenerateEvent` 의 메서드는 `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    }
}

```

## <a name="ending-and-saving-the-workout-session"></a>종료 하 고 워크 아웃 세션 저장

사용자는 해당 워크 아웃 완료 되 면 응용 프로그램 현재 워크 아웃 세션을 종료 하 고 HealthKit 데이터베이스에 저장 해야 합니다. HealthKit에 저장 하는 체력 단련 워크 아웃 활동 목록에 자동으로 표시 됩니다.

새로운 iOS 10에 여기에 사용자의 iphone도 워크 아웃 활동 목록입니다. 따라서 근처 Apple Watch 없으면 휴대폰에서의 워크 아웃 표시 됩니다.

타사 앱 수 이제 사용자의 일별 이동 목표에 영향을 체력 단련 에너지 샘플을 포함 하는 활동 응용 프로그램에서 사용자의 이동 링 업데이트 됩니다.

다음 단계 함께 종료 되 고 워크 아웃 세션을 저장 해야 합니다.

[![](workout-apps-images/workout05.png "종료 하 고 워크 아웃 세션 다이어그램 저장")](workout-apps-images/workout05.png#lightbox)

1. 첫째, 응용 프로그램 워크 아웃 세션을 종료 해야 합니다.
2. 워크 아웃 세션 HealthKit에 저장 됩니다.
3. 저장된 워크 아웃 세션에 소비 되는 에너지 또는 거리와 같은 모든 샘플을 추가 합니다.

### <a name="ending-the-session"></a>세션 종료

워크 아웃 세션을 종료 하려면 호출는 `EndWorkoutSession` 의 메서드는 `HKHealthStore` 전달는 `HKWorkoutSession`:

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
...

public void EndOutdoorRun ()
{
    // End the current workout session
    HealthStore.EndWorkoutSession (WorkoutSession);
}
```

해당 표준 모드로 장치 센서 초기화 됩니다. 에 대 한 콜백을 받게 됩니다는 워크 아웃 종료 HealthKit 끝나면는 `DidChangeToState` 의 메서드는 `HKWorkoutSessionDelegate`:

```csharp
public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
{
    // Take action based on the change in state
    switch (toState) {
    ...
    case HKWorkoutSessionState.Ended:
        StopObservingHealthKitSamples ();
        RaiseEnded ();
        break;
    }

}
```

### <a name="saving-the-session"></a>세션 저장

앱이 워크 아웃 세션 종료 되 면 한 워크 아웃 만들려고 해야 합니다 (`HKWorkout`) (이벤트) 함께 HealthKit 데이터 저장소에 저장 (`HKHealthStore`):

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
public float MilesRun { get; set; }
public double ActiveEnergyBurned { get; set;}
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

private void SaveWorkoutSession ()
{
    // Build required workout quantities 
    var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
    var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

    // Create any required metadata
    var metadata = new NSMutableDictionary ();
    metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

    // Create workout
    var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                    WorkoutSession.StartDate, 
                                    NSDate.Now, 
                                    WorkoutEvents.ToArray (), 
                                    energyBurned, 
                                    distance, 
                                    metadata);

    // Save to HealthKit
    HealthStore.SaveObject (workout, (successful, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (successful) {

            }
        } else {
            // Report error
        }
    });

}
```

이 코드에서는로 워크 아웃에 대 한 거리 및 총 소비 되는 에너지 양이 필요 `HKQuantity` 개체입니다. 워크 아웃 정의 메타 데이터의 사전 만들고는 워크 아웃 위치를 지정 합니다.

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

새 `HKWorkout` 동일한 개체를 만든 `HKWorkoutActivityType` 로 `HKWorkoutSession`, 시작 및 끝 날짜, 이벤트 (위의 섹션에서 누적 되 고)를 구운 에너지 목록이 전체 거리 및 메타 데이터 사전입니다. 이 개체는 상태 저장소에 저장 하 고 오류 처리입니다.  

### <a name="adding-samples"></a>샘플을 추가합니다.

워크 아웃 샘플 집합을 저장 하는 응용 프로그램, HealthKit 주어진된 워크 아웃와 관련 된 모든 샘플 나중에 응용 프로그램 HealthKit를 쿼리할 수 있도록 샘플은 자체 워크 아웃 사이의 연결을 생성 합니다. 응용 프로그램 수이 정보를 사용 하 여 워크 아웃 데이터에서 그래프를 생성 고 워크 아웃 타임 라인에 대해 플롯할 합니다.

활동 응용 프로그램의 이동 링에 기여 하는 응용 프로그램에 대 한 에너지 샘플은 저장 된 워크 아웃 포함 해야 합니다. 또한, 거리 및 에너지에 대 한 합계 저장된 워크 아웃 응용 프로그램에 연결 하는 모든 샘플의 합을 일치 해야 합니다.

샘플에 저장 된 워크 아웃을 추가 하려면 다음을 수행 합니다.

```csharp
using System.Collections.Generic;
using WatchKit;
using HealthKit;
...

public HKHealthStore HealthStore { get; private set; }
public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
...


private void SaveWorkoutSamples (HKWorkout workout)
{
    // Add samples to saved workout
    HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (success) {

            }
        } else {
            // Report error
        }
    });
}
```

필요에 따라 앱 계산할 수 있으며 샘플 또는 메가 샘플 하나 (고 워크 아웃의 전체 범위에 걸쳐) 연결 된 저장 된 워크 아웃을 가져오는의 작은 하위 집합을 만듭니다.

## <a name="workouts-and-ios-10"></a>체력 단련 및 iOS 10

모든 watchOS 3 워크 아웃 응용 프로그램에는 부모 iOS 10 기반된 워크 아웃 앱과 iOS 10에 새 iOS 앱이를 사용 하 여 Apple Watch 워크 아웃 모드 (사용자 개입 없음)에 배치 되며 백그라운드에서 실행 모드에서 watchOS 응용 프로그램을 실행 하는 워크 아웃을 시작할 수 있습니다 ( 참조[배경 실행에 대 한](#About-Background-Running) 위의 자세한 세부 정보에 대 한).

WatchOS 앱이 실행 되는 동안 WatchConnectivity 메시징 및 통신 부모 iOS 앱을 사용할 수 있습니다.

이 프로세스의 작동 방식에 대해 살펴보겠습니다.

[![](workout-apps-images/workout06.png "iPhone 및 Apple Watch 통신 다이어그램")](workout-apps-images/workout06.png#lightbox)

1. IPhone 앱을 만듭니다는 `HKWorkoutConfiguration` 개체 및 워크 아웃 유형 및 위치를 설정 합니다.
2. `HKWorkoutConfiguration` 개체가 모두 보내질 Apple Watch 버전의 앱 및 시스템에서 시작 되어 아직 실행 하지 않은 경우.
3. 사용 하 여 전달 된 워크 아웃 구성에서 watchOS 3 응용 프로그램에서 새 워크 아웃 세션을 시작 (`HKWorkoutSession`).

> [!IMPORTANT]
> Apple Watch는 워크 아웃 시작 하도록 부모 iPhone 앱의 순서로 watchOS 3 앱 배경 실행 해야 사용 하도록 설정 합니다. 참조 하십시오 [배경 실행을 사용 하도록 설정](#Enabling-Background-Running) 위에 더 자세한 합니다.

이 프로세스는 watchOS 3 응용 프로그램에서 직접 워크 아웃 세션을 시작 하는 과정에 매우 비슷합니다. IPhone, 다음 코드를 사용 합니다.

```csharp
using System;
using HealthKit;
using WatchConnectivity;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
#endregion
...

private void StartOutdoorRun ()
{
    // Can the app communicate with the watchOS version of the app?
    if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
        // Create a workout configuration
        var configuration = new HKWorkoutConfiguration () {
            ActivityType = HKWorkoutActivityType.Running,
            LocationType = HKWorkoutSessionLocationType.Outdoor
        };

        // Start watch app
        HealthStore.StartWatchApp (configuration, (success, error) => {
            // Handle any errors
            if (error == null) {
                // Was the save successful
                if (success) {
                    ...
                }
            } else {
                // Report error
                ...
            }
        });
    }
}
```

이 코드에서는 watchOS 버전의 앱을 설치 하 고 iPhone 버전 먼저 데이터베이스에 연결할 수 있습니다.

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
    ...
}
```

생성 한 다음는 `HKWorkoutConfiguration` 평소와 같이 사용 하 여는 `StartWatchApp` 의 메서드는 `HKHealthStore` Apple Watch 보내고 응용 프로그램 및 워크 아웃 세션을 시작 합니다.

조사식 OS 응용 프로그램에 다음 코드를 사용 하 고는 `WKExtensionDelegate`:

```csharp
using WatchKit;
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...


public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
{
    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

소요 되는 `HKWorkoutConfiguration` 새 및 `HKWorkoutSession` 사용자 지정의 인스턴스를 연결 하 고 `HKWorkoutSessionDelegate`합니다. 워크 아웃 세션이 사용자의 HealthKit 상태 저장소에 대 한 시작 됩니다.

## <a name="bringing-all-the-pieces-together"></a>모든는 조각 통합

이 문서에 포함 된 정보의 모든 취한 watchOS 3 기반된 워크 아웃 응용 프로그램 및 해당 상위 10 iOS 기반된 워크 아웃 응용 프로그램을 포함 다음 부분으로 구성 합니다.

1. **iOS 10 `ViewController.cs`**  -조사식 연결 세션 및 Apple Watch 워크 아웃의 시작을 처리 합니다.
2. **watchOS 3 `ExtensionDelegate.cs`**  -watchOS 3 버전의 워크 아웃 앱을 처리 합니다.
3. **watchOS 3 `OutdoorRunDelegate.cs`**  -사용자 지정 `HKWorkoutSessionDelegate` 이벤트를 처리 하는 워크 아웃에 대 한 합니다.

> [!IMPORTANT]
> 다음 섹션에 표시 된 코드에는 워크 아웃 앱 watchOS 3에에서 제공 하는 새, 향상 된 기능을 구현 하는 데 필요한 파트만 포함 됩니다. 지원 되는 모든 코드와 코드 시키며 UI가 업데이트를 포함 되어 있지 않으면 하지만 watchOS 밖의 설명서를 따라 쉽게 만들 수 있습니다.<p/>



### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController.cs` 워크 아웃 응용 프로그램의 상위 10 iOS 버전의 파일이 다음 코드를 포함 합니다.

```csharp
using System;
using HealthKit;
using UIKit;
using WatchConnectivity;

namespace MonkeyWorkout
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
        public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Private Methods
        private void InitializeWatchConnectivity ()
        {
            // Is Watch Connectivity supported?
            if (!WCSession.IsSupported) {
                // No, abort
                return;
            }

            // Is the session already active?
            if (ConnectivitySession.ActivationState != WCSessionActivationState.Activated) {
                // No, start session
                ConnectivitySession.ActivateSession ();
            }
        }

        private void StartOutdoorRun ()
        {
            // Can the app communicate with the watchOS version of the app?
            if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
                // Create a workout configuration
                var configuration = new HKWorkoutConfiguration () {
                    ActivityType = HKWorkoutActivityType.Running,
                    LocationType = HKWorkoutSessionLocationType.Outdoor
                };

                // Start watch app
                HealthStore.StartWatchApp (configuration, (success, error) => {
                    // Handle any errors
                    if (error == null) {
                        // Was the save successful
                        if (success) {
                            ...
                        }
                    } else {
                        // Report error
                        ...
                    }
                });
            }
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Start Watch Connectivity
            InitializeWatchConnectivity ();
        }
        #endregion
    }
}
```

### <a name="extensiondelegatecs"></a>ExtensionDelegate.cs

`ExtensionDelegate.cs` 워크 아웃 응용 프로그램의 watchOS 3 버전의 파일이 다음 코드를 포함 합니다.

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
        public OutdoorRunDelegate RunDelegate { get; set; }
        #endregion

        #region Constructors
        public ExtensionDelegate ()
        {
            
        }
        #endregion

        #region Private Methods
        private void StartWorkoutSession (HKWorkoutConfiguration workoutConfiguration)
        {
            // Create workout session
            // Start workout session
            NSError error = null;
            var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

            // Successful?
            if (error != null) {
                // Report error to user and return
                return;
            }

            // Create workout session delegate and wire-up events
            RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

            RunDelegate.Failed += () => {
                // Handle the session failing
                ...
            };

            RunDelegate.Paused += () => {
                // Handle the session being paused
                ...
            };

            RunDelegate.Running += () => {
                // Handle the session running
                ...
            };

            RunDelegate.Ended += () => {
                // Handle the session ending
                ...
            };
            
            RunDelegate.ReachedMileGoal += (miles) => {
                // Handle the reaching a session goal
                ...
            };

            RunDelegate.HealthKitSamplesUpdated += () => {
                // Update UI as required
                ...
            };

            // Start session
            HealthStore.StartWorkoutSession (workoutSession);
        }

        private void StartOutdoorRun ()
        {
            // Create a workout configuration
            var workoutConfiguration = new HKWorkoutConfiguration () {
                ActivityType = HKWorkoutActivityType.Running,
                LocationType = HKWorkoutSessionLocationType.Outdoor
            };

            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion

        #region Override Methods
        public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
        {
            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion
    }
}
```

### <a name="outdoorrundelegatecs"></a>OutdoorRunDelegate.cs

`OutdoorRunDelegate.cs` 워크 아웃 응용 프로그램의 watchOS 3 버전의 파일이 다음 코드를 포함 합니다.

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Private Variables
        private HKAnchoredObjectQuery QueryActiveEnergyBurned;
        #endregion

        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        public float MilesRun { get; set; }
        public double ActiveEnergyBurned { get; set;}
        public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
        public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;

        }
        #endregion

        #region Private Methods
        private void ObserveHealthKitSamples ()
        {
            // Get the starting date of the required samples
            var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

            // Get data from the local device
            var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
            var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

            // Assemble compound predicate
            var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

            // Get ActiveEnergyBurned
            QueryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
                // Valid?
                if (error == null) {
                    // Yes, process all returned samples
                    foreach (HKSample sample in addedObjects) {
                        // Accumulate totals
                        var quantitySample = sample as HKQuantitySample;
                        ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);

                        // Save samples
                        WorkoutSamples.Add (sample);
                    }

                    // Inform caller
                    RaiseHealthKitSamplesUpdated ();
                }
            });

            // Start Query
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
                                                  
        }

        private void StopObservingHealthKitSamples ()
        {
            // Stop query
            HealthStore.StopQuery (QueryActiveEnergyBurned);
        }

        private void ResumeObservingHealthkitSamples ()
        {
            // Resume current queries 
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
        }

        private void NotifyUserOfReachedMileGoal (float miles)
        {
            // Play haptic feedback
            WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);

            // Raise event
            RaiseReachedMileGoal (miles);
        }

        private void SaveWorkoutSession ()
        {
            // Build required workout quantities
            var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
            var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

            // Create any required metadata
            var metadata = new NSMutableDictionary ();
            metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

            // Create workout
            var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                            WorkoutSession.StartDate, 
                                            NSDate.Now, 
                                            WorkoutEvents.ToArray (), 
                                            energyBurned, 
                                            distance, 
                                            metadata);

            // Save to HealthKit
            HealthStore.SaveObject (workout, (successful, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (successful) {
                        // Add samples to workout
                        SaveWorkoutSamples (workout);
                    }
                } else {
                    // Report error
                    ...
                }
            });

        }

        private void SaveWorkoutSamples (HKWorkout workout)
        {
            // Add samples to saved workout
            HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (success) {
                        ...
                    }
                } else {
                    // Report error
                    ...
                }
            });
        }
        #endregion

        #region Public Methods
        public void PauseWorkout ()
        {
            // Pause the current workout
            HealthStore.PauseWorkoutSession (WorkoutSession);
        }

        public void ResumeWorkout ()
        {
            // Pause the current workout
            HealthStore.ResumeWorkoutSession (WorkoutSession);
        }

        public void ReachedNextMile ()
        {
            // Create and save marker event
            var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
            WorkoutEvents.Add (markerEvent);

            // Notify user
            NotifyUserOfReachedMileGoal (++MilesRun);
        }

        public void EndOutdoorRun ()
        {
            // End the current workout session
            HealthStore.EndWorkoutSession (WorkoutSession);
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                StopObservingHealthKitSamples ();
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                if (fromState == HKWorkoutSessionState.Paused) {
                    ResumeObservingHealthkitSamples ();
                } else {
                    ObserveHealthKitSamples ();
                }
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                StopObservingHealthKitSamples ();
                SaveWorkoutSession ();
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);

            // Save HealthKit generated event
            WorkoutEvents.Add (@event);

            // Take action based on the type of event
            switch (@event.Type) {
            case HKWorkoutEventType.Lap:
                ...
                break;
            case HKWorkoutEventType.Marker:
                ...
                break;
            case HKWorkoutEventType.MotionPaused:
                ...
                break;
            case HKWorkoutEventType.MotionResumed:
                ...
                break;
            case HKWorkoutEventType.Pause:
                ...
                break;
            case HKWorkoutEventType.Resume:
                ...
                break;
            }
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();
        public delegate void OutdoorRunMileGoalDelegate (float miles);

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }

        public event OutdoorRunMileGoalDelegate ReachedMileGoal;
        internal void RaiseReachedMileGoal (float miles)
        {
            if (this.ReachedMileGoal != null) this.ReachedMileGoal (miles);
        }

        public event OutdoorRunEventDelegate HealthKitSamplesUpdated;
        internal void RaiseHealthKitSamplesUpdated ()
        {
            if (this.HealthKitSamplesUpdated != null) this.HealthKitSamplesUpdated ();
        }
        #endregion
    }
}
```

## <a name="best-practices"></a>모범 사례

Apple 디자인 및 watchOS 3 및 iOS 10 워크 아웃 응용 프로그램을 구현 하는 경우 다음 모범 사례를 사용 하 여 제안:

- IPhone 및 10 iOS 버전의 앱에 연결할 수 없는 경우에 watchOS 3 워크 아웃 앱 여전히 작동 인지 확인 합니다.
- GPS 없이 거리 샘플을 생성할 수 있기 때문 GPS를 사용할 수 없는 경우 HealthKit 거리를 사용 합니다.
- Apple Watch 또는 iPhone에서의 워크 아웃 시작 사용자 수 있습니다.
- 해당 기록 데이터 보기에서 다른 소스 (예: 다른 타사 앱)에서 체력 단련 표시할 앱을 허용 합니다.
- 앱에서 기록 데이터 삭제 표시 체력 단련 하지 않은 것을 확인 합니다.

## <a name="summary"></a>요약

이 문서는 향상 된 기능 검사가 수행 Apple 워크 아웃 앱 watchOS 3 및 Xamarin에서이 구현 하는 방법에 적용 했습니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [HealthKit 소개](~/ios/platform/healthkit.md)
