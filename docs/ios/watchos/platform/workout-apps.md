---
title: Xamarin에서 watchOS 체력 앱
description: 이 문서에서는 watchOS 3의 개선 앱에 대 한 Apple의 향상 된 기능 및 Xamarin에서 이러한 앱을 구현 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 0b1827d8936343cbf977395a788a466f1c3f60ac
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032748"
---
# <a name="watchos-workout-apps-in-xamarin"></a>Xamarin에서 watchOS 체력 앱

_이 문서에서는 watchOS 3의 개선 앱에 대 한 Apple의 향상 된 기능 및 Xamarin에서 이러한 앱을 구현 하는 방법을 설명 합니다._

WatchOS 3의 새로운 기능으로, 진행 중인 관련 앱은 Apple Watch의 백그라운드에서 실행 하 여 HealthKit 데이터에 액세스할 수 있습니다. 또한 부모 iOS 10 기반 앱은 사용자 개입 없이 watchOS 3 기반 앱을 시작할 수 있습니다.

다음 항목에 대해 자세히 설명합니다.

## <a name="about-workout-apps"></a>체력 단련 앱 정보

적합성 및 진행 중인 앱의 사용자는 자신의 상태 및 적합성 목표를 달성 하는 것이 매우 devoting 수 있습니다. 결과적으로 데이터를 정확 하 게 수집 하 고 표시 하 고 Apple 상태와 원활 하 게 통합 하는 응답성이 뛰어난 앱을 사용할 수 있습니다.

잘 설계 된 적합성 또는 체력 단련 앱은 사용자가 활동을 차트로 작성 하 여 적합성 목표를 달성 하는 데 도움이 됩니다. Apple Watch를 사용 하 여 적합성 및 체력 검사 앱은 하트 요금, calorie 된 굽기 및 활동 감지에 즉시 액세스할 수 있습니다.

[![](workout-apps-images/workout01.png "Fitness and workout app example")](workout-apps-images/workout01.png#lightbox)

WatchOS 3의 새로운 기능으로, _백그라운드 실행_ 은 Apple Watch의 백그라운드에서 실행 하 여 HealthKit 데이터에 액세스 하는 기능을 제공 합니다.

이 문서에서는 백그라운드 실행 기능을 소개 하 고, 체력 단련 앱 수명 주기를 다루며, 체력 앱이 Apple Watch에 대 한 사용자의 _활동 링_ 에 기여 하는 방법을 보여 줍니다.

## <a name="about-workout-sessions"></a>체력 단련 세션 정보

모든 체력 단련 앱의 핵심은 사용자가 시작 하 고 중지할 수 있는`HKWorkoutSession`( _체력 단련 세션_ )입니다. 체력 단련 세션 API는 구현 하기 쉬우며 다음과 같은 체력 단련 앱에 몇 가지 이점을 제공 합니다.

- 활동 유형을 기반으로 하는 동작 및 calorie 굽기 검색입니다.
- 사용자의 작업 링에 대 한 자동 기여입니다.
- 세션 중에는 사용자가 장치를 절전 모드에서 해제할 때마다 (손목를 발생 시키거나 Apple Watch와 상호 작용) 앱이 자동으로 표시 됩니다.

## <a name="about-background-running"></a>백그라운드 실행 정보

위에서 설명한 것 처럼 watchOS 3을 사용 하 여 백그라운드에서 실행 되도록 체력 단련 앱을 설정할 수 있습니다. 진행 중인 앱을 실행 하는 백그라운드를 사용 하면 백그라운드에서 실행 하는 동안 Apple Watch의 센서에서 데이터를 처리할 수 있습니다. 예를 들어 앱은 더 이상 화면에 표시 되지 않더라도 사용자의 핵심 요금을 계속 모니터링할 수 있습니다.

백그라운드 실행은 사용자에 게 현재 진행 상태를 알리기 위해 햅 경고를 보내는 등 활성 진행 중인 세션 중에 언제 든 지 사용자에 게 실시간 피드백을 제공 하는 기능을 제공 합니다.

또한 백그라운드 실행을 통해 앱은 사용자 인터페이스를 신속 하 게 업데이트할 수 있으므로 사용자가 신속 하 게 Apple Watch 때 최신 데이터를 사용할 수 있습니다.

Apple Watch에서 높은 성능을 유지 하기 위해 백그라운드 실행을 사용 하는 시청 앱은 배터리 절약을 위해 백그라운드 작업의 양을 제한 해야 합니다. 앱이 백그라운드에서 과도 한 CPU를 사용 하는 경우 watchOS에 의해 일시 중단 될 수 있습니다.

### <a name="enabling-background-running"></a>백그라운드 실행 설정

백그라운드 실행을 사용 하도록 설정 하려면 다음을 수행 합니다.

1. **솔루션 탐색기**에서 시청 확장의 도우미 앱 `Info.plist` 파일을 두 번 클릭 하 여 편집용으로 엽니다.
2. **원본** 뷰로 전환 합니다. 

    [![](workout-apps-images/plist01.png "The Source view")](workout-apps-images/plist01.png#lightbox)
3. `WKBackgroundModes` 라는 새 키를 추가 하 고 **형식을** `Array`로 설정 합니다. 

    [![](workout-apps-images/plist02.png "Add a new key called WKBackgroundModes")](workout-apps-images/plist02.png#lightbox)
4. `String` **형식** 및 `workout-processing`값을 사용 하 여 배열에 새 항목을 추가 합니다. 

    [![](workout-apps-images/plist03.png "Add a new item to the array with the Type of String and a value of workout-processing")](workout-apps-images/plist03.png#lightbox)
5. 파일의 변경 내용을 저장합니다.

## <a name="starting-a-workout-session"></a>체력 단련 세션 시작

다음 세 가지 주요 단계를 통해 진행 중 세션을 시작할 수 있습니다.

[![](workout-apps-images/workout02.png "The three main steps to starting a Workout Session")](workout-apps-images/workout02.png#lightbox)

1. 앱은 HealthKit의 데이터에 액세스 하기 위한 권한 부여를 요청 해야 합니다.
2. 시작 되는 체력의 유형에 대 한 체력 단련 구성 개체를 만듭니다.
3. 새로 만든 체력 구성을 사용 하 여 체력 단련 세션을 만들고 시작 합니다.

### <a name="requesting-authorization"></a>권한 부여 요청

앱이 사용자의 HealthKit 데이터에 액세스할 수 있으려면 먼저 사용자의 권한 부여를 요청 하 고 받아야 합니다. 다시 사용 앱의 특성에 따라 다음과 같은 유형의 요청을 수행할 수 있습니다.

- 데이터를 쓸 수 있는 권한 부여:
  - 워크 아웃
- 데이터를 읽을 수 있는 권한 부여:
  - 구운 에너지
  - 거리
  - 하트 요금  

앱이 권한 부여를 요청 하려면 먼저 HealthKit에 액세스 하도록 구성 해야 합니다.

다음을 수행합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 `Entitlements.plist` 파일을 두 번 클릭하여 엽니다.
2. 아래쪽으로 스크롤하고 **HealthKit 사용**을 선택 합니다. 

    [![](workout-apps-images/auth01.png "Check Enable HealthKit")](workout-apps-images/auth01.png#lightbox)
3. 파일의 변경 내용을 저장합니다.
4. [명시적 앱 id 및 프로 비전 프로필](~/ios/platform/healthkit.md) 의 지침을 따르고 앱 [Id 및 프로 비전 프로필](~/ios/platform/healthkit.md) 을 [HealthKit 소개](~/ios/platform/healthkit.md) 문서의 xamarin.ios 앱 섹션과 연결 하 여 앱을 올바르게 프로 비전 합니다.
5. 마지막으로 [HealthKit 소개](~/ios/platform/healthkit.md) 문서의 사용자 섹션에서 [프로그래밍 상태 키트](~/ios/platform/healthkit.md) 및 [요청 권한](~/ios/platform/healthkit.md) 의 지침을 사용 하 여 사용자의 HealthKit 데이터 저장소에 액세스할 수 있는 권한 부여를 요청 합니다.

### <a name="setting-the-workout-configuration"></a>체력 구성 설정

`HKWorkoutActivityType.Running`와 같은 체력 단련 유형 (예: `HKWorkoutSessionLocationType.Outdoor`)을 지정 하는`HKWorkoutConfiguration`(체력 단련) 구성 개체를 사용 하 여 체력 단련 세션이 생성 됩니다.

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
  ActivityType = HKWorkoutActivityType.Running,
  LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>체력 단련 세션 대리자 만들기 

작업 일지 세션 중에 발생할 수 있는 이벤트를 처리 하려면 앱에서 체력 세션 대리자 인스턴스를 만들어야 합니다. 프로젝트에 새 클래스를 추가 하 고 `HKWorkoutSessionDelegate` 클래스의 기반으로 합니다. 옥외 실행의 예는 다음과 같습니다.

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

이 클래스는 체력 세션의 상태가 변경 될 때 발생 하는 몇 가지 이벤트를 만들고 (`DidChangeToState`), 체력 워크 세션이 실패 하면 (`DidFail`) 발생 합니다. 

### <a name="creating-a-workout-session"></a>체력 단련 세션 만들기

위에서 만든 체력 단련 구성 및 체력 단련 세션 대리자를 사용 하 여 새 체력 세션을 만들고 사용자의 기본 HealthKit 저장소에 대해 시작 합니다.

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

앱이이 체력 이동 세션을 시작 하 고 사용자가 다시 시계 화면으로 전환 하는 경우 작은 녹색 "실행 중인 남자" 아이콘이 얼굴 위에 표시 됩니다.

[![](workout-apps-images/workout03.png "A tiny green running man icon displayed above the face")](workout-apps-images/workout03.png#lightbox)

사용자가이 아이콘을 탭 하면 앱으로 돌아갑니다.

## <a name="data-collection-and-control"></a>데이터 수집 및 제어

체력 단련 세션이 구성 되 고 시작 되 면 앱은 세션 (예: 사용자의 하트 요금)에 대 한 데이터를 수집 하 고 세션의 상태를 제어 해야 합니다.

[![](workout-apps-images/workout04.png "Data Collection and Control Diagram")](workout-apps-images/workout04.png#lightbox)

1. **샘플 관찰** -앱은 사용자에 게 표시 되 고 사용자에 게 표시 되는 HealthKit에서 정보를 검색 해야 합니다.
2. **이벤트 관찰** -앱은 HealthKit 또는 앱 UI (예: 체력을 일시 중지 하는 사용자)에서 생성 된 이벤트에 응답 해야 합니다.
3. **실행 중 상태를 입력** 합니다. 세션이 시작 되었으며 현재 실행 중입니다.
4. **일시 중지 된 상태를 입력** 합니다. 사용자가 현재 체력 단련 세션을 일시 중지 하 고 나중에 다시 시작할 수 있습니다. 사용자는 단일 체력 세션에서 실행 중 및 일시 중지 됨 상태를 여러 번 전환할 수 있습니다.
5. **종료 되는 세션 종료** -언제 든 지 사용자가 체력 워크 세션을 종료 하거나, 요금이 청구 될 수 있는 경우 (예: 2 마일 실행)에는 해당 세션을 종료 하 고 종료할 수 있습니다.

마지막 단계는 사용자의 HealthKit 데이터 저장소에 체력 세션의 결과를 저장 하는 것입니다.

### <a name="observing-healthkit-samples"></a>HealthKit 샘플 관찰

앱은 하트 요금 또는 활성 에너지와 같이 관심이 있는 각 HealthKit 데이터 요소에 대해 _Anchor 개체 쿼리_ 를 열어야 합니다. 관찰 되는 각 데이터 요소에 대해 앱에 전송 될 때 새 데이터를 캡처하려면 업데이트 처리기를 만들어야 합니다.

이러한 데이터 요소에서 앱은 합계 (예: 총 실행 거리)를 누적 하 고 필요에 따라 사용자 인터페이스를 업데이트할 수 있습니다. 또한 앱은 특정 목표 또는 성과 (예: 실행의 다음 마일 완료)에 도달 했을 때 사용자에 게 알릴 수 있습니다.

다음 샘플 코드를 살펴보세요.

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

`GetPredicateForSamples` 메서드를 사용 하 여 데이터를 가져올 시작 날짜를 설정 하는 조건자를 만듭니다. `GetPredicateForObjectsFromDevices` 메서드를 사용 하 여 HealthKit 정보를 가져올 수 있는 장치 집합을 만듭니다 .이 경우 로컬 Apple Watch (`HKDevice.LocalDevice`)만 사용 됩니다. 두 조건자는 `CreateAndPredicate` 메서드를 사용 하 여 복합 조건자 (`NSCompoundPredicate`)로 결합 됩니다.

원하는 데이터 요소에 대해 새 `HKAnchoredObjectQuery` 만들어집니다 (이 경우 활성 에너지 레코딩한 데이터 요소에 대 한 `HKQuantityTypeIdentifier.ActiveEnergyBurned`). 반환 되는 데이터의 양에 제한이 적용 되지 않습니다 (`HKSampleQuery.NoLimit`). HealthKit에서 앱으로 반환 되는 데이터를 처리 하도록 업데이트 처리기가 정의 됩니다. 

새 데이터가 지정 된 데이터 요소에 대 한 앱으로 배달 될 때마다 업데이트 처리기가 호출 됩니다. 오류가 반환 되지 않으면 앱은 데이터를 안전 하 게 읽고 필요한 계산을 수행한 다음 필요에 따라 UI를 업데이트할 수 있습니다.

이 코드는 `addedObjects` 배열에 반환 된 모든 샘플 (`HKSample`)에 대해 반복 하 여 수량 샘플 (`HKQuantitySample`)으로 캐스팅 합니다. 그런 다음 샘플의 double 값을 줄 (`HKUnit.Joule`)로 가져와, 체력을 위해 구운 활성 에너지의 누계를 누적 하 고 사용자 인터페이스를 업데이트 합니다.

### <a name="achieved-goal-notification"></a>달성 한 목표 알림

위에서 언급 한 것 처럼 사용자가 체력 단련 앱에서 목표를 달성 하는 경우 (예: 실행의 첫 번째 마일 완료) Taptic Engine를 통해 사용자에 게 햅 피드백을 보낼 수 있습니다. 사용자가 피드백을 요청 하는 이벤트를 보기 위해 손목을 발생 시킬 가능성이 높기 때문에 앱은이 시점에서 UI도 업데이트 해야 합니다.

햅 피드백을 재생 하려면 다음 코드를 사용 합니다.

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>이벤트 관찰

이벤트는 앱이 사용자의 체력을 확인할 때 특정 요소를 강조 표시 하는 데 사용할 수 있는 타임 스탬프입니다. 일부 이벤트는 앱에서 직접 만들고, 체력에 저장 되며, 일부 이벤트는 HealthKit에 의해 자동으로 생성 됩니다.

HealthKit에서 만든 이벤트를 관찰 하기 위해 앱은 `HKWorkoutSessionDelegate`의 `DidGenerateEvent` 메서드를 재정의 합니다.

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

Apple은 watchOS 3에 다음과 같은 새 이벤트 유형을 추가 했습니다.

- `HKWorkoutEventType.Lap`-체력을 동일한 거리 부분으로 나누는 이벤트입니다. 예를 들어를 실행 하는 동안 트랙 주위에 한 개의 랩 표시를 표시 합니다.
- `HKWorkoutEventType.Marker`-체력 내에서 임의의 관심 점에 대 한 것입니다. 예를 들어, 실외 실행 경로의 특정 지점에 도달 합니다.

이러한 새 형식은 앱에서 만들고 나중에 그래프 및 통계를 만드는 데 사용할 수 있도록 체력에 저장할 수 있습니다.

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

이 코드는 마커 이벤트 (`HKWorkoutEvent`)의 새 인스턴스를 만들어 이벤트의 전용 컬렉션에 저장 하 고 (나중에 다시 사용 세션에 기록 됨) haptics를 통해 이벤트의 사용자에 게 알립니다.

### <a name="pausing-and-resuming-workouts"></a>일시 중지 및 다시 시작

사용자는 체력을 일시적으로 일시 중지 하 고 나중에 다시 시작할 수 있습니다. 예를 들어 중요 한 호출을 수행 하기 위해 실내 실행을 일시 중지 하 고 호출이 완료 된 후에 실행을 다시 시작할 수 있습니다.

앱의 UI는 HealthKit을 호출 하 여 체력을 일시 중지 했다가 다시 시작 하는 방법을 제공 하 여 사용자가 작업을 일시 중단 하는 동안 전원 및 데이터 공간을 모두 절약할 수 Apple Watch 있도록 해야 합니다. 또한 앱은 진행 중인 세션이 일시 중지 된 상태일 때 수신 될 수 있는 새 데이터 요소를 무시 해야 합니다.

HealthKit은 일시 중지 및 재개 이벤트를 생성 하 여 호출 일시 중지 및 재개에 응답 합니다. 체력 단련 세션이 일시 중지 된 동안에는 세션이 다시 시작 될 때까지 새 이벤트 또는 데이터가 앱에 HealthKit 전송 되지 않습니다.

다음 코드를 사용 하 여 일시 중단 세션을 일시 중지 하 고 다시 시작 합니다.

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

HealthKit에서 생성 되는 Pause 및 Resume 이벤트는 `HKWorkoutSessionDelegate`의 `DidGenerateEvent` 메서드를 재정의 하 여 처리할 수 있습니다.

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

WatchOS 3의 새로운 기능은 일시 중지 된 동작 (`HKWorkoutEventType.MotionPaused`)과 동작 다시 시작 (`HKWorkoutEventType.MotionResumed`) 이벤트입니다. 이러한 이벤트는 사용자가 시작 하 고 이동을 중지할 때 실행 중인 체력을 HealthKit 하 여 자동으로 발생 합니다.

앱이 동작 일시 중지 된 이벤트를 받으면 사용자가 동작을 다시 시작 하 고 동작 다시 시작 이벤트가 수신 될 때까지 데이터 수집을 중지 해야 합니다. 앱은 동작 일시 중지 된 이벤트에 대 한 응답으로 진행 중인 세션을 일시 중지 하면 안 됩니다.

> [!IMPORTANT]
> 일시 중지 된 동작 및 동작 다시 시작 이벤트는 RunningWorkout 활동 형식 (`HKWorkoutActivityType.Running`)에 대해서만 지원 됩니다.

이러한 이벤트는 `HKWorkoutSessionDelegate`의 `DidGenerateEvent` 메서드를 재정의 하 여 처리할 수 있습니다.

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

## <a name="ending-and-saving-the-workout-session"></a>체력 단련 세션 종료 및 저장

사용자가 체력을 성공적으로 완료 하면 앱은 현재 HealthKit 세션을 종료 하 고이를 데이터베이스에 저장 해야 합니다. HealthKit에 저장 된 작업은 자동으로 체력 청구 작업 목록에 표시 됩니다.

IOS 10의 새로운 기능으로, 여기에는 사용자 iPhone의 체력 단련 활동 목록 목록도 포함 됩니다. 따라서 Apple Watch 근처에 있지 않더라도 체력은 휴대폰에 표시 됩니다.

에너지 샘플이 포함 된 작업은 이제 타사 앱이 사용자의 일일 이동 목표에 기여할 수 있도록 작업 앱에서 사용자의 이동 링을 업데이트 합니다.

다음 단계를 수행 하 여 진행 중인 세션을 종료 하 고 저장 해야 합니다.

[![](workout-apps-images/workout05.png "Ending and Saving the Workout Session Diagram")](workout-apps-images/workout05.png#lightbox)

1. 먼저 앱이 체력 단련 세션을 종료 해야 합니다.
2. HealthKit에는 체력 단련 세션이 저장 됩니다.
3. 저장 된 체력 세션에 샘플 (예: 에너지 레코딩한 또는 거리)을 추가 합니다.

### <a name="ending-the-session"></a>세션 종료

진행 중인 세션을 종료 하려면 `HKWorkoutSession`전달 하 `HKHealthStore`의 `EndWorkoutSession` 메서드를 호출 합니다.

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

그러면 장치 센서가 표준 모드로 다시 설정 됩니다. HealthKit 완료 되 면 `HKWorkoutSessionDelegate`의 `DidChangeToState` 메서드에 대 한 콜백을 받게 됩니다.

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

앱이 진행 중인 세션을 종료 한 후에는 체력 (`HKWorkout`)을 만들고 이벤트와 함께 HealthKit 데이터 저장소 (`HKHealthStore`)에 저장 해야 합니다.

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

이 코드는 `HKQuantity` 개체로 서 총 에너지의 총 양과 체력에 대 한 거리가 필요한를 만듭니다. 체력을 정의 하는 메타 데이터 사전이 만들어지고, 체력 위치가 다음과 같이 지정 됩니다.

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

새 `HKWorkout` 개체는 `HKWorkoutSession`와 동일한 `HKWorkoutActivityType`, 시작 및 종료 날짜, 이벤트 목록 (위 섹션에서 누적 됨), 사용 된 에너지, 총 거리 및 메타 데이터 사전이 생성 됩니다. 이 개체는 Health Store 및 처리 된 모든 오류에 저장 됩니다.  

### <a name="adding-samples"></a>샘플 추가

앱이 다시 시작 집합에 샘플 집합을 저장 하면 HealthKit는 샘플과 자동으로 연결을 생성 하 여 앱이 지정 된 체력에 연결 된 모든 샘플에 대해 나중에 HealthKit를 쿼리할 수 있도록 합니다. 이 정보를 사용 하 여 앱은 체력 데이터에서 그래프를 생성 하 고이를 체력 타임 라인에 대해 그릴 수 있습니다.

앱이 활동 앱의 이동 링에 기여 하려면 저장 된 체력을 포함 하는 에너지 샘플을 포함 해야 합니다. 또한 거리와 에너지 합계는 앱이 저장 된 체력에 연결 하는 샘플의 합계와 일치 해야 합니다.

저장 된 체력에 샘플을 추가 하려면 다음을 수행 합니다.

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

필요에 따라 앱은 더 작은 샘플의 하위 집합을 계산 하 고 만들 수 있으며, 그 다음에는 저장 된 체력에 연결 되는 1 개의 샘플 (전체 체력 범위에 걸친)을 계산 하 여 만들 수 있습니다.

## <a name="workouts-and-ios-10"></a>워크 아웃 및 iOS 10

모든 watchOS 3 체력 청구 앱은 부모 iOS 10 기반 체력 앱을 포함 하며, iOS 10에 새로 Apple Watch 추가 된이 iOS 앱을 사용 하 여 사용자 개입 없이 체력을 체력으로 전환 하 고 백그라운드 실행 모드에서 watchOS 앱을 실행 하는 체력을 시작할 수 있습니다 [(참조 자세한 내용은 위에서 실행 되는 bout 배경](#about-background-running)

WatchOS 앱이 실행 되는 동안 부모 iOS 앱과의 메시징 및 통신에 WatchConnectivity를 사용할 수 있습니다.

이 프로세스의 작동 원리를 살펴보세요.

[![](workout-apps-images/workout06.png "iPhone and Apple Watch communication diagram")](workout-apps-images/workout06.png#lightbox)

1. IPhone 앱은 `HKWorkoutConfiguration` 개체를 만들고, 체력 및 위치를 설정 합니다.
2. `HKWorkoutConfiguration` 개체는 Apple Watch 버전의 앱으로 전송 되며, 아직 실행 되지 않은 경우 시스템에 의해 시작 됩니다.
3. WatchOS 3 앱은 전달 된 체력 단련 구성을 사용 하 여 새 체력 단련 세션 (`HKWorkoutSession`)을 시작 합니다.

> [!IMPORTANT]
> 부모 iPhone 앱이 Apple Watch의 체력을 계속 시작 하려면 watchOS 3 앱에서 백그라운드 실행이 활성화 되어 있어야 합니다. 자세한 내용은 위에서 [실행 중인 백그라운드 사용](#enabling-background-running) 을 참조 하세요.

이 프로세스는 watchOS 3 앱에서 바로 체력 세션을 시작 하는 프로세스와 매우 비슷합니다. IPhone에서 다음 코드를 사용 합니다.

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

이 코드를 사용 하면 앱의 watchOS 버전이 설치 되 고 iPhone 버전이 먼저 연결 될 수 있습니다.

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
  ...
}
```

그런 다음 일반적인 방법으로 `HKWorkoutConfiguration`를 만들고 `HKHealthStore`의 `StartWatchApp` 메서드를 사용 하 여 Apple Watch로 보내고 앱과 체력 단련 세션을 시작 합니다.

그리고 시청 OS 앱의 `WKExtensionDelegate`에서 다음 코드를 사용 합니다.

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

`HKWorkoutConfiguration`를 사용 하 여 새 `HKWorkoutSession`를 만들고 사용자 지정 `HKWorkoutSessionDelegate`의 인스턴스를 연결 합니다. 사용자의 HealthKit Health 스토어에 대해 체력 단련 세션이 시작 됩니다.

## <a name="bringing-all-the-pieces-together"></a>모든 부분을 함께 가져오기

이 문서에 나와 있는 모든 정보를 사용 하 여 watchOS 3 기반 체력 앱 및 해당 부모 iOS 10 기반 체력 앱은 다음과 같은 부분을 포함할 수 있습니다.

1. **iOS 10 `ViewController.cs`** -감시 연결 세션 시작 및 Apple Watch의 체력을 처리 합니다.
2. **watchOS 3 `ExtensionDelegate.cs`** -watchOS 3 버전의 체력 앱을 처리 합니다.
3. **watchOS 3 `OutdoorRunDelegate.cs`** -체력에 대 한 이벤트를 처리 하는 사용자 지정 `HKWorkoutSessionDelegate`입니다.

> [!IMPORTANT]
> 다음 섹션에 표시 된 코드에는 watchOS 3의 체력 앱에 제공 되는 향상 된 새 기능을 구현 하는 데 필요한 부분만 포함 되어 있습니다. UI를 제공 하 고 업데이트 하는 모든 지원 코드와 코드는 포함 되지 않지만 다른 watchOS 설명서를 따라 쉽게 만들 수 있습니다.<p/>

### <a name="viewcontrollercs"></a>ViewController.cs

진행 중인 앱의 부모 iOS 10 버전에 있는 `ViewController.cs` 파일에는 다음 코드가 포함 됩니다.

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

WatchOS 3 버전의 체력 앱에 있는 `ExtensionDelegate.cs` 파일에는 다음 코드가 포함 됩니다.

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

WatchOS 3 버전의 체력 앱에 있는 `OutdoorRunDelegate.cs` 파일에는 다음 코드가 포함 됩니다.

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

Apple은 watchOS 3 및 iOS 10에서 자동으로 진행 되는 앱을 설계 하 고 구현할 때 다음 모범 사례를 사용 하는 것을 제안

- IPhone 및 iOS 10 버전의 앱에 연결할 수 없는 경우에도 watchOS 3 체력 앱이 계속 작동 하는지 확인 합니다.
- Gps를 사용 하지 않고 거리 샘플을 생성할 수 있으므로 GPS를 사용할 수 없는 경우 HealthKit 거리를 사용 합니다.
- 사용자가 Apple Watch 또는 iPhone에서 체력을 시작할 수 있도록 허용 합니다.
- 앱이 기록 데이터 보기에서 다른 원본 (예: 다른 타사 앱)의 업무를 표시 하도록 허용 합니다.
- 앱이 기록 데이터에서 삭제 된 워크를 표시 하지 않는지 확인 합니다.

## <a name="summary"></a>요약

이 문서에서는 watchOS 3의 개선 앱에 대 한 Apple의 향상 된 기능 및 Xamarin에서 이러한 앱을 구현 하는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [HealthKit 소개](~/ios/platform/healthkit.md)
