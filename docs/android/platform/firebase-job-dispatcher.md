---
title: Firebase 작업 디스패처
description: 이 가이드에는 Google에서 Firebase 작업 발송자 라이브러리를 사용 하 여 백그라운드 작업을 예약 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 06/05/2018
ms.openlocfilehash: 0d512342f1c978c84341d20c298a9fa750800d84
ms.sourcegitcommit: 5db075bdd0b62d5d1d1567c267303a6a1888c8f2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34806805"
---
# <a name="firebase-job-dispatcher"></a>Firebase 작업 디스패처

_이 가이드에는 Google에서 Firebase 작업 발송자 라이브러리를 사용 하 여 백그라운드 작업을 예약 하는 방법을 설명 합니다._


## <a name="overview"></a>개요

Android 응용 프로그램을 사용자에 응답성을 유지 하는 가장 좋은 방법 중 하나는 복잡 하거나 장기 실행 작업은 백그라운드에서 수행 되도록 하려면입니다. 그러나이 백그라운드 작업 부정적인 영향을 미치지 것입니다는 사용자의 경험 장치에서 중요 합니다. 

예를 들어 백그라운드 작업 웹 사이트 서너 개의 분 마다 쿼리에 특정 데이터 집합에 대 한 변경에 대 한 폴링할 수 있습니다. 이 방법은 치명적인 영향 배터리 수명에 동일 하 게 되었지만 심각 하지 않은 합니다. 응용 프로그램은 반복 해 서에서 장치를 깨우는, CPU 전원 상태를 더 높은 수준으로 상승, 라디오 전원을, 네트워크 요청을 하 고 결과 처리 합니다. 장치에서 즉시는 아닙니다 전원을 끄고 한 저전원 유휴 상태를 반환 하므로 더 심각한 가져옵니다. 실수로 잘못 예약 된 백그라운드 작업 불필요 한 과도 한 전원 요구를 사용 하 여 상태의 장치를 유지할 수도 있습니다. 이 겉보기 문제 없는 작업 (웹 사이트를 폴링하여) 사용 장치 비교적 짧은 기간 동안 사용할 수 없게 렌더링 합니다.

Android 백그라운드에서 작업 수행에 도움이 되도록 다음 Api를 제공 하지만 자체적으로 충분 하지 않습니다 지능형 작업 일정에 대 한 합니다. 

* **[하지만 의도 서비스](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; 의도 서비스는 작업을 예약할 수 없으므로 제공 작업을 수행 하기 위한 훌륭한 합니다.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; 이러한 Api 작업을 예약할 수 있지만 실제로 작업을 수행할 수 없으므로 제공 하도록 허용 합니다. 또한는 AlarmManager 허용 시간 기반 제약 조건, 즉, 특정 시간이 나 특정 기간을 경과한 후 경보 발생 합니다. 
* **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)**  &ndash; The JobSchedule는 작업을 예약 하려면 운영 체제와 작동 하는 훌륭한 API입니다. 그러나이 경우에 API 수준 21을 대상으로 하는 Android 응용 프로그램에 사용할 수 있습니다. 
* **[브로드캐스트 수신기](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; 는 Android 응용 프로그램 의도 또는 시스템 수준 이벤트에 대 한 응답에서 작업을 수행 하는 브로드캐스트 수신기를 설정할 수 있습니다. 그러나 브로드캐스트 수신기는 작업을 실행 해야 하는 경우에 대 한 제어를 제공 하지 않습니다. Android 운영 체제의 변경 내용에서 제한도 브로드캐스트 수신기는 작동 하는 경우 않거나에 응답할 수 있는 작업 유형입니다. 

효율적으로 백그라운드 작업을 수행 하는 데 두 가지 주요 기능이 (라고도 _백그라운드 작업_ 또는 _작업_):

1. **작업을 예약 하는 지능적으로** &ndash; 것이 중요 응용 프로그램은 이렇게 하는 동안 백그라운드에서 작업 한다는 것으로 제대로 작동 합니다. 이상적으로 응용 프로그램 작업을 실행할 수 요청 해서는 안 됩니다. 대신, 응용 프로그램 작업에서 수를 실행 하 고 다음 조건이 충족 되는 때를 실행 하려면 해당 작업을 예약할 때 위해 충족 해야 하는 조건을 지정 해야 합니다. 이렇게 하면 Android를 지능적으로 작업을 수행할 수 있습니다. 예를 들어 네트워킹와 관련 된 오버 헤드를 최대한 사용할 수 있도록 한 번에 모두 실행 하려면 네트워크 요청을 일괄 처리 수 있습니다.
2. **작업을 캡슐화** &ndash; 백그라운드 작업을 수행 하는 코드는 사용자 인터페이스와 독립적으로 실행할 수 있으며 비교적 쉬운 작업 완료 되지 않으면 다시 예약 하는 개별 구성 요소에서 캡슐화 해야 어떤 이유로 합니다.

Firebase 작업 디스패처는 일정 백그라운드 작업을 간소화 하기 위해 fluent API를 제공 하는 Google에서 라이브러리입니다. Google 클라우드 관리자에 대 한 대체 되어야 하는 것이 됩니다. 다음 Api Firebase 작업 디스패처로 구성 됩니다.

* A `Firebase.JobDispatcher.JobService` 백그라운드 작업에서 실행 되는 논리로 확장 해야 하는 추상 클래스입니다.
* A `Firebase.JobDispatcher.JobTrigger` 작업을 시작 해야 하는 경우를 선언 합니다. 이 일반적으로 표시 된 시간을 창으로 예를 들어, 해당 작업을 시작 하기 전에 적어도 30 초 정도 기다립니다 있지만 5 분 이내에 작업을 실행 합니다.
* A `Firebase.JobDispatcher.RetryStrategy` 작업이 제대로 실행 하지 못할 때 수행할 하는 방법에 대 한 정보를 포함 합니다. 작업을 다시 실행 하려고 하기 전에 대기 하는 다시 시도 전략 기간을 지정 합니다. 
* A `Firebase.JobDispatcher.Constraint` 충족 해야 하는 작업을 실행 하려면 먼저 장치 unmetered 네트워크에는 조건을 설명 하는 선택적 값 이거나을 부과 합니다.
* `Firebase.JobDispatcher.Job` 이전 Api에는 단위 작업으로 예약할 수 있는를 통합 하는 api는 `JobDispatcher`합니다. `Job.Builder` 클래스 인스턴스화하는 데 사용 되는 `Job`합니다.
* A `Firebasee.JobDispatcher.JobDispatcher` 이전 세 가지 Api를 사용 하 여 운영 체제와 작업을 예약 하 고 필요한 경우 작업을 취소 하는 방법을 제공 합니다.

Firebase 작업 발송자를 사용 하는 작업을 예약 하려면 Xamarin.Android 응용 프로그램을 확장 하는 형식에는 코드 캡슐화 해야는 `JobService` 클래스입니다. `JobService` 수명 주기 메서드가 세 개는 작업의 수명 동안 호출할 수 있습니다.

* **`bool OnStartJob(IJobParameters parameters)`** &ndash; 이 방법은 여기서 작업이 발생 하 고 구현 되어야 합니다. 주 스레드에서 실행 됩니다. 이 메서드는 반환 `true` 없을 경우 남은 작업, 작업 또는 `false` 는 작업을 수행 하는 경우. 
* **`bool OnStopJob(IJobParameters parameters)`** &ndash; 이 작업은 몇 가지 이유로 중지 될 때 호출 됩니다. 반환 해야 `true` 나중에 대 한 작업 다시 예약 해야 하는 경우.
* **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; 이 메서드를 호출한 경우는 `JobService` 모든 비동기 작업을 완료 했습니다. 

작업을 예약 하려면 응용 프로그램 인스턴스화는 `JobDispatcher` 개체입니다. 그런 다음 `Job.Builder` 만드는 데 사용 되는 `Job` 위해 제공 되는 개체는 `JobDispatcher` 를 시도 하 고 작업이 실행 되도록 예약 됩니다.

이 가이드 Firebase 작업 발송자 Xamarin.Android 응용 프로그램에 추가 하 고 사용 하 여 백그라운드 작업을 예약 하는 방법에 설명 합니다.

## <a name="requirements"></a>요구 사항

Firebase 작업 발송자 Android API 수준 9 이상이 필요합니다. Google Play 서비스;에서 제공 하는 일부 구성 요소에 Firebase 작업 발송자 라이브러리 사용 장치에는 Google Play 서비스를 설치 해야 합니다.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Xamarin.Android에서 Firebase 작업 발송자 라이브러리 사용

Firebase 작업 발송자를 시작 하려면 먼저 추가 [Xamarin.Firebase.JobDispatcher NuGet 패키지](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) Xamarin.Android 프로젝트에 있습니다. 에 대 한 NuGet 패키지 관리자 검색는 **Xamarin.Firebase.JobDispatcher** 패키지 (내에 있는 사전 릴리스).

작업 디스패처 Firebase 라이브러리를 추가한 후 만들기는 `JobService` 클래스 및 다음의 인스턴스를 사용 하 여 실행 되도록 예약할는 `FirebaseJobDispatcher`합니다.


### <a name="creating-a-jobservice"></a>JobService 만들기

확장 하는 형식에서 Firebase 작업 발송자 라이브러리에 의해 수행 된 모든 작업을 수행 해야는 `Firebase.JobDispatcher.JobService` 추상 클래스입니다. 만들기는 `JobService` 만드는 것과 매우 비슷합니다는 `Service` Android 프레임 워크: 

1. 확장 된 `JobService` 클래스
2. 와 하위 클래스를 데코레이팅하는 `ServiceAttribute`합니다. 있지만 반드시 필요한 것이 좋습니다 명시적으로 설정 하는 `Name` 디버깅에 도움이 되도록 매개 변수는 `JobService`합니다. 
3. 추가 `IntentFilter` 선언 하는 `JobService` 에 **AndroidManifest.xml**합니다. 찾아 호출할 Firebase 작업 발송자 라이브러리에도 도움이 됩니다이 고 `JobService`합니다.

다음 코드는 가장 간단한 예제 `JobService` 응용 프로그램에 대 한 몇 가지 작업을 비동기적으로 수행 하는 TPL을 사용 합니다.

```csharp
[Service(Name = "com.xamarin.fjdtestapp.DemoJob")]
[IntentFilter(new[] {FirebaseJobServiceIntent.Action})]
public class DemoJob : JobService
{
    static readonly string TAG = "X:DemoService";

    public override bool OnStartJob(IJobParameters jobParameters)
    {
        Task.Run(() =>
        {
            // Work is happening asynchronously (code omitted)
                       
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // nothing to do.
        return false;
    }
}
```

### <a name="creating-a-firebasejobdispatcher"></a>FirebaseJobDispatcher 만들기

만들 필요는 모든 작업을 예약할 수 전에 `Firebase.JobDispatcher.FirebaseJobDispatcher` 개체입니다. `FirebaseJobDispatcher` 예약에 대 한 책임이 `JobService`합니다. 다음 코드 조각은의 인스턴스를 만드는 한 가지 방법은 `FirebaseJobDispatcher`: 
 
 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

이전 코드 조각에는 `GooglePlayDriver` 는 데 도움이 되는 클래스는 `FirebaseJobDispatcher` 장치에서 Google Play 서비스의 일정 Api의 일부와 상호 작용 합니다. 매개 변수 `context` 모든 Android은 `Context`, 작업 등입니다. 현재는 `GooglePlayDriver` 유일한 `IDriver` Firebase 작업 발송자 라이브러리에서 구현 합니다. 

Xamarin.Android 바인딩 Firebase 작업 발송자에 대 한 확장 메서드를 만들 제공는 `FirebaseJobDispatcher` 에서 `Context`: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

한 번의 `FirebaseJobDispatcher` 되었습니다 인스턴스화된 만들 수는 `Job` 의 코드를 실행 하 고는 `JobService` 클래스입니다. `Job` 만들어집니다는 `Job.Builder` 개체와 다음 섹션에서 설명 합니다.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Firebase.JobDispatcher.Job는 Job.Builder를 사용 하 여 만들기

`Firebase.JobDispatcher.Job` 클래스는 메타 데이터를 캡슐화 하는 데 실행 하는 데 필요한는 `JobService`합니다. A`Job` 되는 제약 조건 충족 해야 하 고 작업을 실행하기 전에 경우 같은 정보가 포함 되어는 `Job` 되풀이 또는 작업을 실행 하면 모든 트리거가 있습니다.  필요한 최소로는 `Job` 있어야는 _태그_ (작업을 식별 하는 고유 문자열은 `FirebaseJobDispatcher`)의 형식과 `JobService` 실행 해야 하는 합니다. Firebase 작업 발송자 인스턴스화는 `JobService` 작업을 실행 하는 시기입니다.  A `Job` 의 인스턴스를 사용 하 여 만들어집니다는 `Firebase.JobDispatcher.Job.JobBuilder` 클래스입니다. 

다음 코드 조각에 대해서는를 만드는 방법의 간단한 예는 `Job` Xamarin.Android 바인딩을 사용 하 여:

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder` 는 작업에 대 한 입력된 값에 몇 가지 기본 유효성 검사를 수행 합니다. 것에 대 한 수 없는 경우 예외가 throw 됩니다는 `Job.Builder` 만들려는 `Job`합니다.  `Job.Builder` 만들어집니다는 `Job` 기본값을 지정 합니다.

* A `Job`의 _수명_ 장치가 다시 부팅 될 때 까지만 됩니다 (시간 것을 예약 됩니다. 실행) &ndash; 장치를 다시 부팅은 `Job` 손실 됩니다.
* A `Job` 되풀이 되지 &ndash; 한 번만 실행 됩니다.
* A `Job` 가능한 한 빨리 실행 되도록 예약 됩니다.
* 에 대 한 기본 다시 시도 전략은 `Job` 사용 하는 _지 수 백오프_ (섹션에서 자세히 아래에 설명 된 [는 RetryStrategy 설정](#Setting_a_RetryStrategy))

### <a name="scheduling-a-job"></a>작업 예약

만든 후의 `Job`,으로 예약 하는 데 필요한는 `FirebaseJobDispatcher` 실행 하기 전에. 예약 하는 데에 두 가지가 `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

반환한 값 `FirebaseJobDispatcher.Schedule` 다음 정수 값 중 하나입니다.

* `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; `Job` 성공적으로 예약 되었습니다.
* `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; 알 수 없는 몇 가지 문제가 발생 수 없습니다는 `Job` 예약 되 고 있습니다.
* `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; 잘못 된 `IDriver` 사용한 또는 `IDriver` 어떻게 하 든 사용할 수 없습니다. 
* `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; `Trigger` 가 지원 되지 않습니다.
* `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; 서비스가 올바르게 구성 되지 않은 없거나 사용할 수 없습니다.
 
### <a name="configuring-a-job"></a>작업 구성

작업을 사용자 지정할 수는 있습니다. 작업 사용자 지정 가능 방법의 예는 다음과 같습니다.

* [작업에 매개 변수를 전달](#Passing_Parameters_to_a_Job) &ndash; A `Job` 파일을 다운로드 하는 예를 들어 작업을 수행 하기 위해 추가 값이 필요할 수 있습니다.
* [제약 조건을 설정](#Setting_Constraints) &ndash; 특정 조건이 충족 될 때만 작업을 실행 하도록 할 수도 있습니다. 예를 들어 실행만 `Job` 장치를 충전 하는 경우. 
* [시점을 지정는 `Job` 실행할지](#Setting_Job_Triggers) &ndash; The Firebase 작업 발송자 응용 프로그램을 한 번 작업 실행 시기를 지정 합니다.  
* [실패 한 작업에 다시 시도 전략을 선언](#Setting_a_RetryStrategy) &ndash; A _다시 시도 전략_ 지침을 제공는 `FirebaseJobDispatcher` 로 수행할 작업에 `Jobs` 를 완료 하는 실패 합니다. 

이러한 각 항목 살펴봅니다 더 다음 섹션에 있습니다.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-jarameters-to-a-job"></a>Jarameters 작업에 전달

매개 변수를 만들어 작업에 전달 되는 `Bundle` 와 함께 전달 되는 `Job.Builder.SetExtras` 메서드:

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

`Bundle` 에서 액세스 되는 `IJobParameters.Extras` 속성에는 `OnStartJob` 메서드:

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>제약 조건 설정

제약 조건 수 장치 비용 또는 배터리 소모를 줄일 수 있습니다. `Firebase.JobDispatcher.Constraint` 클래스 정수 값으로 이러한 제약 조건을 정의 합니다.

* `Constraint.OnUnmeteredNetwork` &ndash; 장치 unmetered 네트워크에 연결 된 경우에 작업을 실행 합니다. 사용자 데이터 요금이 발생 하지 않도록 하려면 유용 합니다.
* `Constraint.OnAnyNetwork` &ndash; 장치가 연결 된 모든 네트워크 작업을 실행 합니다. 와 함께 지정 된 경우 `Constraint.OnUnmeteredNetwork`,이 값이 우선적으로 적용 됩니다.
* `Constraint.DeviceCharging` &ndash; 장치를 충전 하는 경우에 작업을 실행 합니다.

제약 조건으로 설정 됩니다는 `Job.Builder.SetConstraint` 메서드: 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

`JobTrigger` 작업을 시작할 하는 방법에 대 한 운영 체제에 대 한 지침을 제공 합니다. A `JobTrigger` 에 _창 실행_ 시기에 대 한 예약 된 시간을 정의 하는 `Job` 실행 해야 합니다. 실행 창에는 _창을 시작_ 값과 _종료 윈도_ 값입니다. 시작 창이 장치 작업을 실행 하기 전에 대기할 시간 (초) 수 있으며 최종 창 값은 실행 하기 전에 대기할 시간 (초)의 최대 수는 `Job`합니다. 

A `JobTrigger` 으로 만들 수 있습니다는 `Firebase.Jobdispatcher.Trigger.ExecutionWindow` 메서드.  예를 들어 `Trigger.ExecutionWindow(15,60)` 예약 된 경우에서 15 ~ 60 초 사이 실행 해야 하는 작업을 의미 합니다. `Job.Builder.SetTrigger` 메서드 하는 데 사용 됩니다. 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

기본 `JobTrigger` 는 값으로 표현 되는 작업에 대 한 `Trigger.Now`, 예약 된 후 최대한 빨리는 작업을 실행할 수 있는지를 지정 하는...

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>RetryStrategy 설정

`Firebase.JobDispatcher.RetryStrategy` 장치는 지연 양을 사용 하도록 지정 하기 전에 실패 한 작업을 다시 실행 하는 데 사용 됩니다. A `RetryStrategy` 에 _정책_, 실패 한 작업 및 작업 예약 되도록 기간을 지정 하는 실행 창이 다시 예약 하는 시간 기준 알고리즘을 정의 하는 합니다. 이 _다시 예약 창_ 두 개의 값으로 정의 됩니다. 첫 번째 값은 작업 일정을 변경 하기 전에 대기할 시간 (초) 수 (의 _초기 백오프_ 값), 두 번째 숫자는 최대 시간 (초) 작업이 실행 해야 합니다 (의 _최대 백오프_값). 
 
다시 시도 정책에는 두 가지 유형의이 int 값으로 식별 됩니다.

* `RetryStrategy.RetryPolicyExponential` &ndash; _지 수 백오프_ 정책은 초기 백오프 값 기하급수적으로 증가할 각 실패 후 합니다. 작업이 실패 하는, 처음으로 라이브러리 대기 작업 일정을 변경 하기 전에 지정 된 _initial 간격 &ndash; 예제 30 초입니다. 작업이 실패 하면 두 번째로 라이브러리는 60 초 이상 작업을 실행 하는 동안 대기 합니다. 세 번째 시도가 실패 한 라이브러리는 120 초, 대기 등에입니다. 기본 `RetryStrategy` Firebase 작업 발송자 라이브러리에서 표현 되는 `RetryStrategy.DefaultExponential` 개체입니다. 30 초는 초기 백오프 및 3, 600 초의 최대 백오프 된 있습니다.
* `RetryStrategy.RetryPolicyLinear` &ndash; 이 전략은는 _선형 백오프_ (성공)까지 간격을 설정 하는 작업에 실행 되도록 다시 예약 해야 합니다. 선형 백오프는 가능한 한 빨리 완료 해야 하는 작업에 대 한 또는 자체를 신속 하 게 해결할 수 있는 문제에 가장 적합 합니다. 작업 디스패처 Firebase 라이브러리는 정의 `RetryStrategy.DefaultLinear` 최소 30 초의 다시 예약 기간이 있는 3600 초가 최대 합니다.

사용자 지정을 정의할 수는 `RetryStrategy` 와 `FirebaseJobDispatcher.NewRetryStrategy` 메서드. 세 개의 매개 변수를 사용 합니다.

1. `int policy` &ndash; _정책_ 이전 중 하나인 `RetryStrategy` 값 `RetryStrategy.RetryPolicyLinear`, 또는 `RetryStrategy.RetryPolicyExponential`합니다.
2. `int intialBackoffSeconds` &ndash; _초기 백오프_ 지연 되는 기간을 초 단위로 작업을 다시 실행 하려고 하기 전에 필요한 합니다. 이 대 한 기본값은 30 초입니다. 
3. `int maximumBackoffSeconds` &ndash; _최대 백오프_ 작업을 다시 실행 하려고 하기 전에 지연 시간 (초)의 최대 수를 선언 하는 값입니다. 기본값은 3600 초입니다. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>작업 취소

예정 된 모든 작업을 취소할 수 인지만 사용 하 여 단일 작업에서 `FirebaseJobDispatcher.CancelAll()` 메서드 또는 `FirebaseJobDispatcher.Cancel(string)` 메서드:

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

두 방법 중 하나는 정수 값을 반환 합니다.

* `FirebaseJobDispatcher.CancelResultSuccess` &ndash; 작업을 취소 했습니다.
* `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; 오류가 취소 하에서 작업 수 없습니다.
* `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; `FirebaseJobDispatcher` 유효 하지 않아 작업을 취소할 수 없는지 `IDriver` 사용할 수 있습니다.

## <a name="summary"></a>요약

이 가이드는 지능적으로 백그라운드에서 작업을 수행 하도록 Firebase 작업 발송자를 사용 하는 방법을 설명 합니다. 로 수행할 작업을 캡슐화 하는 방법을 설명 하는 `JobService` 방법과 사용 하는 `FirebaseJobDispatcher` 와 조건을 지정 하는 작업을 예약할는 `JobTrigger` 와 오류를 처리 하는 방법 및는 `RetryStrategy`합니다.


## <a name="related-links"></a>관련 링크

- [NuGet에 Xamarin.Firebase.JobDispatcher](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [GitHub의 디스패처-firebase-작업](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher 바인딩](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [지능형 작업 예약](https://developer.android.com/topic/performance/scheduling.html)
- [Android 배터리 및 메모리 최적화-Google I/O 2016 (비디오)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
