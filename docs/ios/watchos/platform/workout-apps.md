---
title: watchOS에서 Xamarin 운동 앱
description: 이 문서에서는 향상 된 기능을 설명 Apple 운동 앱 watchOS 3 및 Xamarin에서 구현 하는 방법에 적용 했습니다.
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: d755160043191f93247fd09e99f23eb85831fa8b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113900"
---
# <a name="watchos-workout-apps-in-xamarin"></a>watchOS에서 Xamarin 운동 앱

_이 문서에서는 향상 된 기능을 설명 Apple 운동 앱 watchOS 3 및 Xamarin에서 구현 하는 방법에 적용 했습니다._


새 watchOS 3, 운동 관련 앱에 Apple Watch 백그라운드에서 실행 하 고 HealthKit 데이터에 액세스할 수 있습니다. 해당 부모 iOS 10 기반 앱 사용자의 개입 없이 watchOS 3 기반 응용 프로그램을 실행 하는 기능이 있습니다.

다음 항목에 대해 자세히 설명합니다.

## <a name="about-workout-apps"></a>운동 앱에 대 한

여러 시간 정하고 해당 상태 및 적합성에 대 한 목표 달성을 감수할 피트 니스 및 운동 앱의 사용자가 항상 전용 수 있습니다. 결과적으로 정확 하 게 수집 하 고 데이터를 표시 및 Apple 상태를 원활 하 게 통합 하는 앱이 응답 하 고 사용 하기 쉬운 기대 합니다.

잘 설계 된 적합성 또는 운동 앱에 사용자가 해당 작업의 적합성에 대 한 목표에 도달 하도록 차트를 수 있습니다. Apple Watch 사용 하 여 피트 니스 및 운동 앱에 즉시 액세스할 심장 박동수 칼로리 진행 및 활동 감지 합니다.

[![](workout-apps-images/workout01.png "피트 니스 및 운동 앱 예제")](workout-apps-images/workout01.png#lightbox)

WatchOS 3, 새 _백그라운드 실행_ 제공 운동 관련 앱 Apple Watch 백그라운드에서 실행 하 고 HealthKit 데이터에 액세스할 수 있습니다.

이 문서는 백그라운드 실행 기능, 운동 앱 수명 주기를 설명 및 운동 앱을 사용자의 기여할 수 방법을 보여 줍니다 _활동 링_ Apple Watch.

## <a name="about-workout-sessions"></a>운동 세션에 대 한

모든 운동 앱의 핵심은는 _운동 세션_ (`HKWorkoutSession`)는 사용자 시작 및 중지할 수 있습니다. 운동 세션 API 구현 하기는 쉽지만 및와 같은 운동 앱을 몇 가지 이점을 제공 합니다.

- 동작 및 칼로리 검색 활동 형식에 따라 진행 합니다.
- 사용자의 활동 링에 대 한 자동 기여 합니다.
- 세션에서 작업 하는 동안 앱을 자동으로 때마다 표시 됩니다 사용자 (자신의 손목 발생 또는 Apple Watch의 상호 작용 하거나) 장치를 다시 시작 합니다.

## <a name="about-background-running"></a>백그라운드 실행에 대 한

위에서 설명한 대로 백그라운드에서 실행 되도록 watchos 3 운동 앱을 설정할 수 있습니다. 백그라운드 실행 운동 앱을 사용 하 여 백그라운드에서 실행 하는 동안 Apple Watch 센서에서 데이터를 처리할 수 있습니다. 예를 들어, 앱을 계속 모니터링할 수 사용자의 핵심 속도 더 이상 화면에 표시 되는 경우에 합니다.

또한 백그라운드 실행 햅 틱 현재 진행률의 사용자에 게 알릴 경고를 보내는 등 활성 운동 세션 중 언제 든 지 사용자에 게 실시간 피드백을 제공 하는 기능을 제공 합니다.

또한 백그라운드에서 실행 되므로 사용자는 최신 데이터를 신속 하 게 해당 Apple Watch 보기 하는 경우 해당 사용자 인터페이스를 빠르게 업데이트 하려면 앱을 있습니다.

Apple Watch 높은 성능을 유지 하기 위해 백그라운드에서 실행을 사용 하 여 watch 앱을 배터리를 절약 하기 위해 백그라운드 작업의 크기를 제한 해야 합니다. 앱을 백그라운드에서 과도 한 CPU를 사용 하는 경우 watchOS에서 일시 중단 가져올 수 있습니다.

### <a name="enabling-background-running"></a>실행 중인 백그라운드를 사용 하도록 설정

백그라운드에서 실행을 사용 하도록 설정 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**, 조사식 확장의 도우미 iPhone 앱을 두 번 클릭 `Info.plist` 파일을 편집용으로 엽니다.
2. 으로 전환 합니다 **원본** 보기: 

    [![](workout-apps-images/plist01.png "소스 보기")](workout-apps-images/plist01.png#lightbox)
3. 라는 새 키를 추가 `WKBackgroundModes` 설정 합니다 **형식** 하려면 `Array`: 

    [![](workout-apps-images/plist02.png "WKBackgroundModes 라는 새 키를 추가 합니다.")](workout-apps-images/plist02.png#lightbox)
4. 새 항목을 사용 하 여 배열에 추가 합니다 **형식** 의 `String` 값 `workout-processing`: 

    [![](workout-apps-images/plist03.png "문자열의 형식 및 운동 처리 값을 사용 하 여 배열에 새 항목 추가")](workout-apps-images/plist03.png#lightbox)
5. 파일의 변경 내용을 저장합니다.

## <a name="starting-a-workout-session"></a>운동 세션을 시작

세 가지 주요 단계 운동 세션을 시작 하려면:

[![](workout-apps-images/workout02.png "운동 세션을 시작 하는 세 가지 주요 단계")](workout-apps-images/workout02.png#lightbox)

1. 앱은 데이터 HealthKit에 액세스 하려면 권한 부여를 요청 해야 합니다.
2. 운동 시작 되 고 형식의 운동 구성 개체를 만듭니다.
3. 페이지를 만들고 새로 만든된 운동 구성을 사용 하 여 운동 세션을 시작 합니다.

### <a name="requesting-authorization"></a>권한 부여를 요청합니다.

앱 사용자의 HealthKit 데이터에 액세스 하기 전에 요청 하 고 사용자 로부터 권한 부여를 수신 합니다. 운동 앱의 성격에 따라 다음과 같은 유형의 요청 해야 합니다.

- 데이터를 작성 하는 권한 부여:
    - 달리기
- 데이터를 읽을 수 있는 권한.
    - 소비 되는 에너지
    - 거리
    - 심장 박동수  

앱 권한 부여를 요청 하려면, 먼저 HealthKit에 액세스 하도록 구성 해야 합니다.

다음을 수행합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 `Entitlements.plist` 파일을 두 번 클릭하여 엽니다.
2. 확인 하 고 아래쪽으로 스크롤하여 **HealthKit 사용**: 

    [![](workout-apps-images/auth01.png "HealthKit 사용 확인")](workout-apps-images/auth01.png#lightbox)
3. 파일의 변경 내용을 저장합니다.
4. 지침에 따라 합니다 [명시적 앱 ID 및 프로 비전 프로필](~/ios/platform/healthkit.md) 및 [앱 ID 및 프로 비전 프로필 사용 하 여 Xamarin.iOS 앱 연결](~/ios/platform/healthkit.md) 부분은 [소개 HealthKit](~/ios/platform/healthkit.md) 문서를 올바르게 앱 프로 비전 합니다.
5. 지침을 마지막으로 사용 합니다 [상태 키트 프로그래밍](~/ios/platform/healthkit.md) 및 [the 사용자에서 권한을 요청](~/ios/platform/healthkit.md) 부분을 [HealthKit 소개](~/ios/platform/healthkit.md) 요청에 문서 사용자의 HealthKit 데이터 저장소에 액세스할 수 있는 권한입니다.

### <a name="setting-the-workout-configuration"></a>운동 구성 설정

운동 세션 운동 구성 개체를 사용 하 여 생성 됩니다 (`HKWorkoutConfiguration`) 운동 형식을 지정 하는 (같은 `HKWorkoutActivityType.Running`) 및 운동 위치 (같은 `HKWorkoutSessionLocationType.Outdoor`):

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
    ActivityType = HKWorkoutActivityType.Running,
    LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>운동 세션 대리자 만들기 

운동 세션 중 발생 하는 이벤트를 처리 하려면 앱 운동 세션 대리자 인스턴스를 만들려고 해야 합니다. 프로젝트에 새 클래스를 추가 하 고 해제의 자료를 `HKWorkoutSessionDelegate` 클래스입니다. 실외 실행을 예이 다음과 같을 수 있습니다.

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

이 클래스는 운동 세션 변경의 상태와 발생할 수 있는 여러 이벤트를 만듭니다 (`DidChangeToState`) 운동 세션이 실패 하는 경우 (`DidFail`). 

### <a name="creating-a-workout-session"></a>운동 세션 만들기

운동 세션을 새로 만들 사용자의 기본 HealthKit 저장소에 대해 시작을 위에서 만든 운동 구성과 운동 세션 대리자를 사용 하 여:

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

앱이 운동 세션을 시작 하 고 사용자가 해당 watch 화면으로 전환 하는 경우 발생 하는 위에 작은 녹색 "man 실행" 아이콘이 표시 됩니다.

[![](workout-apps-images/workout03.png "작은 녹색 실행 man 아이콘 표면 위에 표시")](workout-apps-images/workout03.png#lightbox)

사용자가이 아이콘을 하는 경우 이동 되며 다시 앱으로 합니다.

## <a name="data-collection-and-control"></a>데이터 수집 및 제어

운동 세션 구성 되었으며 시작 되 면 앱 (예: 사용자의 심장 박동수) 세션에 대 한 데이터를 수집 하 여 세션의 상태를 제어 해야 합니다.

[![](workout-apps-images/workout04.png "데이터 수집 및 제어 다이어그램")](workout-apps-images/workout04.png#lightbox)

1. **샘플을 관찰** -앱에서 HealthKit 수행 되며 사용자에 게 표시 하는 정보를 검색 해야 합니다.
2. **이벤트를 관찰** -앱 HealthKit 또는 앱의 UI (예: 사용자는 운동 일시 중지)에서 생성 되는 이벤트에 응답 해야 합니다.
3. **실행 상태가** -세션이 시작 된 되어 현재 실행 합니다.
4. **일시 중지 됨 상태로 전환** -사용자가 현재 운동 세션을 일시 중지 하 고 나중에 다시 시작할 수 있습니다. 사용자는 여러 번 단일 운동 세션에서에서 실행 중인 및 일시 중지 된 상태 간에 전환할 수 있습니다.
5. **운동 세션을 종료** -언제 든 지 사용자 운동 세션을 종료할 수 또는 만료 하 고 자체적으로 (예: 두 마일 실행) 요금제 만납니다 되었으면 끝날 수 있습니다.

운동 세션의 결과 사용자의 HealthKit 데이터 저장소에 저장 하는 최종 단계가입니다.

### <a name="observing-healthkit-samples"></a>HealthKit 샘플 관찰

앱을 열고 해야는 _앵커 개체 쿼리_ HealthKit 데이터의 각 지점에 대 한 관심을 거친 핵심 속도 또는 활성 에너지 같은 합니다. 관찰 된 각 데이터 요소에 업데이트 처리기를 앱에 전송 되는 새 데이터를 캡처에 만들 수 해야 합니다.

이러한 데이터 요소에서 앱 합계 (예: 총 실행된 거리)를 누적 하 고 필요에 따라 해당 사용자 인터페이스를 업데이트할 수 있습니다. 또한 특정 목표 또는 실행의 다음 마일을 완료 하는 등의 도전 과제에 도달한 경우 앱 사용자를 알릴 수 있습니다.

다음 샘플 코드를 살펴보십시오.

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

사용에 대 한 데이터를 가져올 하고자 하는 시작 날짜를 설정 하는 조건자를 만들면를 `GetPredicateForSamples` 메서드. HealthKit를 사용 하 여 정보 끌어오기 장치 집합을 만듭니다는 `GetPredicateForObjectsFromDevices` 메서드를이 경우 로컬 Apple Watch만 (`HKDevice.LocalDevice`). 두 개의 조건자는 복합 조건자에 결합 됩니다 (`NSCompoundPredicate`)를 사용 하 여 `CreateAndPredicate` 메서드.

새 `HKAnchoredObjectQuery` 필요한 데이터 요소에 대해 생성 됩니다 (이 예제의 `HKQuantityTypeIdentifier.ActiveEnergyBurned` 활성 에너지 구울 데이터 요소에 대 한), 반환 되는 데이터의 양에 제한이 없습니다 (`HKSampleQuery.NoLimit`) 앱에 반환 되 고는 데이터를 처리 하는 업데이트 처리기가 정의 하 고 HealthKit에서. 

업데이트 처리기에 지정 된 데이터 요소에 대 한 앱에 새 데이터가 전달 되는 언제 든 지 호출 됩니다. 오류가 반환 되 면 앱 안전 하 게 데이터를 읽고, 필요한 계산을 필요에 따라 UI를 업데이트 합니다.

코드 샘플의 모든 반복 (`HKSample`)에서 반환 된 `addedObjects` 배열 및 수량 샘플으로 캐스팅 하 (`HKQuantitySample`). 그런 다음 한 줄으로 된 샘플의 double 값을 가져오고 (`HKUnit.Joule`)를 실행 중인 총 활성 소비 되는 에너지를 운동에 대 한 누적 및 사용자 인터페이스를 업데이트 합니다.

### <a name="achieved-goal-notification"></a>목표 달성된 알림

위에서 설명한 것 처럼 사용자 운동 앱 (예: 실행의 첫 번째 마일)를 완료 하는 목표를 달성 하는 경우 보낼 수 있습니다 햅 틱 피드백 Taptic 엔진을 통해 사용자에 게 합니다. 사용자가 손목 피드백 메시지가 표시 되는 이벤트를 발생 시킵니다 아마 있으므로 앱 UI의이 시점에서 업데이트도 해야 합니다.

햅 틱 피드백을 재생 하려면 다음 코드를 사용 합니다.

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>이벤트를 관찰합니다.

이벤트는 앱 사용자의 단련 하는 동안 특정 요소를 강조 표시 하는 데 사용할 수 있는 타임 스탬프입니다. 일부 이벤트는 앱에서 직접 생성 되며는 운동에 저장 및 HealthKit에서 일부 이벤트를 자동으로 생성 됩니다.

HealthKit에서 만든 이벤트를 살펴 보려면 앱 재정의 `DidGenerateEvent` 메서드는 `HKWorkoutSessionDelegate`:

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

- `HKWorkoutEventType.Lap` -이벤트는 운동 동일한 거리 부분 분해를 위한 것입니다. 예: 하나의 살펴보기 실행 하는 동안 추적을 표시 합니다.
- `HKWorkoutEventType.Marker` -는 운동 내 관심의 임의 지점에 대 한 됩니다. 예를 들어 실외는 실행 경로에 특정 시점에 도달 합니다.

이러한 새로운 형식이 앱에서 만든 하 고 나중에 그래프를 만들고 통계에서 사용할 운동에 저장 될 수 있습니다.

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

이 코드는 표식 이벤트의 새 인스턴스를 만듭니다 (`HKWorkoutEvent`) 이벤트 (나중에 쓰여질 운동 세션)의 개인 컬렉션에 저장 하 고 haptics 통해 이벤트의 사용자에 게 알립니다.

### <a name="pausing-and-resuming-workouts"></a>일시 중지 및 재개 달리기

운동 세션에서 언제 든 사용자 수 일시 중단 된 운동 하 고 나중에 다시 시작 합니다. 예를 들어, 중요 한 호출을 수행 하 고 호출이 완료 된 후 실행을 다시 시작 하는 실내 실행을 일시 중지 될 수 있습니다 이러한 합니다.

앱의 UI는 일시 중지 하 고 사용자가 해당 작업을 중단 하는 동안 Apple Watch 전원 및 데이터 공간을 절약할 수 있도록는 운동 (HealthKit 호출) 하 여 다시 시작 하는 방법을 제공 해야 합니다. 또한 앱 운동 세션을 일시 중지 된 상태일 때 받을 수 있는 모든 새로운 데이터 요소를 무시 해야 합니다.

HealthKit 일시 중지 및 일시 중지 및 다시 시작 이벤트를 생성 하 여 호출을 다시 시작에 응답 합니다. 운동 세션 일시 중지 하는 동안 없는 새 이벤트 또는 데이터 전송할 앱 HealthKit 하 여 세션 재개 될 때까지 합니다.

다음 코드를 사용 하 여 일시 중지 하 고 운동 세션 다시 시작 합니다.

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

HealthKit에서 생성 되는 일시 중지 및 다시 시작 이벤트를 재정의 하 여 처리할 수는 `DidGenerateEvent` 메서드는 `HKWorkoutSessionDelegate`:

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

또한 새 watchOS 3에는 일시 중지 동작 (`HKWorkoutEventType.MotionPaused`) 및 다시 시작 동작 (`HKWorkoutEventType.MotionResumed`) 이벤트입니다. 이러한 이벤트는 발생 자동으로 HealthKit 하 여 실행 중인 단련 하는 동안 사용자 시작 되 고이 이동을 중지 합니다.

앱 동작 일시 중지 이벤트를 받으면 사용자를 다시 시작 동작 및 동작을 다시 시작 이벤트를 받을 때까지 데이터 수집 중지 해야 합니다. 앱 동작 일시 중지 이벤트에 대 한 응답에서 운동 세션 일시 중지 하지 해야 합니다.

> [!IMPORTANT]
> 동작 일시 중지 및 다시 시작 동작 이벤트 RunningWorkout 작업 유형 에서만 지원 됩니다 (`HKWorkoutActivityType.Running`).

이러한 이벤트를 재정의 하 여 다시 처리할 수는 `DidGenerateEvent` 메서드를 `HKWorkoutSessionDelegate`:

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

## <a name="ending-and-saving-the-workout-session"></a>종료 및 운동 세션 저장

사용자가 운동 완료 되 면 앱은 현재 운동 세션을 종료 하 고 HealthKit 데이터베이스에 저장 해야 합니다. HealthKit를 저장 하는 달리기 운동 활동 목록에서 자동으로 표시 됩니다.

새 ios 10, 여기에 사용자의 iPhone도 운동 작업 목록 목록입니다. 따라서 Apple Watch 주변에 없는 경우에는 운동 휴대폰에 표시 됩니다.

타사 앱 사용자의 일일 이동 목표에 이제 기여할 수 있도록 달리기 에너지 샘플을 포함 하는 작업 앱에서 사용자의 이동 링을 업데이트 합니다.

다음 단계를 종료 하는 운동 세션 저장 필요 합니다.

[![](workout-apps-images/workout05.png "종료 및 운동 세션 다이어그램 저장")](workout-apps-images/workout05.png#lightbox)

1. 먼저 앱 운동 세션을 종료 해야 합니다.
2. 운동 세션 HealthKit에 저장 됩니다.
3. 저장 된 운동 세션 (예: 소비 되는 에너지 또는 거리) 샘플을 추가 합니다.

### <a name="ending-the-session"></a>세션 종료

운동 세션을 종료 하려면 호출을 `EndWorkoutSession` 메서드를 `HKHealthStore` 전달는 `HKWorkoutSession`:

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

해당 일반 모드로 장치 센서가 되돌려집니다. 에 대 한 콜백의 받을 HealthKit는 운동 종료 완료 되 면 합니다 `DidChangeToState` 메서드는 `HKWorkoutSessionDelegate`:

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

만납니다를 만들어야 앱 운동 세션이 종료 된 후 (`HKWorkout`) 하 고 (이벤트) 및 HealthKit 데이터 저장소에 저장 (`HKHealthStore`):

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

이 코드에서는으로 운동 대 한 총 소비 되는 에너지와 거리 필요 `HKQuantity` 개체입니다. 운동을 정의 하는 메타 데이터의 사전이 만들어지고는 운동의 위치 지정:

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

새 `HKWorkout` 과 동일한 개체를 만들 `HKWorkoutActivityType` 으로 `HKWorkoutSession`, 시작 및 종료 날짜, 이벤트 (위의 섹션에서 누적 되 고), 에너지를 구운 목록 전체 거리 및 메타 데이터 사전입니다. 이 개체는 상태 저장소에 저장 하 고 오류 처리입니다.  

### <a name="adding-samples"></a>추가 샘플

만납니다 샘플 집합을 저장 하는 앱을 앱 지정된 만납니다와 연결 된 모든 샘플에 대 한 나중에 HealthKit를 쿼리할 수 있도록 HealthKit 샘플 자체 운동 사이의 연결을 생성 합니다. 이 정보를 사용 하 앱 운동 데이터에서 그래프를 생성 하 고 수 운동 타임 라인에 대 한 내용을 플로팅하 합니다.

Activity 앱의 이동 링에 기여 하는 앱에 대 한 저장 된 운동을 사용 하 여 에너지 샘플 포함 해야 합니다. 또한 거리 및 에너지에 대 한 합계 저장된 만납니다를 사용 하 여 앱을 연결 하는 샘플의 합계를 일치 해야 합니다.

샘플에 저장 된 만납니다를 추가 하려면 다음을 수행 합니다.

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

필요에 따라 앱을 계산 하 고 하나의 단위로 샘플 (운동을의 전체 범위에 걸쳐) 저장된 운동을 사용 하 여 연결을 가져오는 샘플의 작은 하위 집합을 만들 수 있습니다.

## <a name="workouts-and-ios-10"></a>IOS 10 및 달리기

모든 watchOS 3 워크 아웃 응용 프로그램에는 부모 iOS 10 기반된 워크 아웃 앱과 iOS 10에 새 iOS 앱이를 사용 하 여 Apple Watch 워크 아웃 모드 (사용자 개입 없음)에 배치 되며 백그라운드에서 실행 모드에서 watchOS 응용 프로그램을 실행 하는 워크 아웃을 시작할 수 있습니다 ( 참조[배경 실행에 대 한](#About-Background-Running) 위의 자세한 세부 정보에 대 한).

WatchOS 앱 실행 되는 동안 WatchConnectivity 메시징 및 상위 iOS 앱과의 통신에 사용할 수 있습니다.

이 프로세스의 작동 방식에 대해 살펴보겠습니다.

[![](workout-apps-images/workout06.png "iPhone 및 Apple Watch 통신 다이어그램")](workout-apps-images/workout06.png#lightbox)

1. IPhone 앱을 만듭니다는 `HKWorkoutConfiguration` 개체 및 운동 유형 및 위치를 설정 합니다.
2. `HKWorkoutConfiguration` 개체 Apple Watch 버전의 앱에 전송 되 고이 시스템에서 시작 되어 아직 실행 하지 않은 경우.
3. 사용 하 여 전달 된 운동 구성, watchOS 3 앱 새 운동 세션을 시작 (`HKWorkoutSession`).

> [!IMPORTANT]
> Apple Watch 만납니다를 시작 하려면 부모 iPhone 앱의 순서로 3 watchOS 앱 백그라운드 실행 해야 사용 하도록 설정 합니다. 참조 하세요 [백그라운드 실행 활성화](#Enabling-Background-Running) 위에 대 한 자세한 내용은 합니다.

이 프로세스는 3 watchOS 앱에서 직접 운동 세션을 시작 하는 프로세스와 매우 유사 합니다. IPhone에서 다음 코드를 사용 합니다.

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

이 코드는 앱의 watchOS 버전을 설치 하 고 iPhone 버전에 처음 연결할 수 있는지를 보장 합니다.

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
    ...
}
```

만듭니다는 `HKWorkoutConfiguration` 일반적으로 사용 하는 `StartWatchApp` 메서드의 `HKHealthStore` Apple Watch 보내고 앱 및 운동 세션을 시작 합니다.

Watch OS 앱의 다음 코드를 사용 하 여 `WKExtensionDelegate`:

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

걸리는 합니다 `HKWorkoutConfiguration` 새 만들고 `HKWorkoutSession` 사용자 지정 인스턴스 연결 `HKWorkoutSessionDelegate`합니다. 운동 세션은 사용자의 HealthKit Health 스토어에 대 한 시작 됩니다.

## <a name="bringing-all-the-pieces-together"></a>한 데 모아 모든는 조각

이 문서에 제공 된 정보를 모두 수행, watchOS 3 기반된 운동 앱 및 해당 상위 iOS 10 기반된 운동 앱 같습니다. 다음 부분으로 구성을

1. **iOS 10 `ViewController.cs`**  -Watch 연결 세션 및 Apple Watch 만납니다의 시작을 처리 합니다.
2. **watchOS 3 `ExtensionDelegate.cs`**  -운동 앱의 watchOS 3 버전을 처리 합니다.
3. **watchOS 3 `OutdoorRunDelegate.cs`**  -사용자 지정 `HKWorkoutSessionDelegate` 는 운동에 대 한 이벤트를 처리할 수 있습니다.

> [!IMPORTANT]
> 다음 섹션에 나와 있는 코드에는 운동 앱 watchOS 3에에서 제공 하는 새로운, 향상 된 기능을 구현 하는 데 필요한 파트만 포함 됩니다. 지원 되는 모든 코드 및 있고 UI를 업데이트 하는 코드 포함 되지 않지만 다른 watchOS 설명서에 따라 쉽게 만들 수 있습니다.<p/>



### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController.cs` 운동 앱의 상위 iOS 10 버전의 파일이 다음 코드를 포함 합니다.

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

`ExtensionDelegate.cs` 운동 앱의 watchOS 3 버전의 파일이 다음 코드를 포함 합니다.

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

`OutdoorRunDelegate.cs` 운동 앱의 watchOS 3 버전의 파일이 다음 코드를 포함 합니다.

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

Apple 디자인 및 watchOS 3 및 iOS 10에서 운동 앱을 구현할 때 다음 모범 사례를 사용 하 여 제안 합니다.

- IPhone 및 iOS 10 버전의 앱에 연결할 수 없는 경우에 watchOS 3 운동 앱 여전히 작동 되는지 확인 합니다.
- GPS 없이 거리 샘플을 생성할 수 있으므로 GPS를 사용할 수 없을 때 HealthKit 거리를 사용 합니다.
- Apple Watch 또는 iPhone에를 운동부터 사용자를 허용 합니다.
- 달리기 해당 기록 데이터 보기에서 다른 원본 (예: 다른 타사 앱)에서 표시할 앱을 허용 합니다.
- 앱에서 기록 데이터 삭제 표시 달리기 하지 않은 것을 확인 합니다.

## <a name="summary"></a>요약

이 문서에서는 이러한 향상 된 부분이 Apple 운동 앱 watchOS 3 및 Xamarin에서 구현 하는 방법에 적용 했습니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [HealthKit 소개](~/ios/platform/healthkit.md)
