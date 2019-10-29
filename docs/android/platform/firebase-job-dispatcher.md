---
title: Firebase 작업 디스패처
description: 이 가이드에서는 Google에서 Firebase 작업 디스패처 라이브러리를 사용 하 여 백그라운드 작업을 예약 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
ms.openlocfilehash: 280fe11f935db0a364f3342b22bb9544cdda1e6d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020239"
---
# <a name="firebase-job-dispatcher"></a>Firebase 작업 디스패처

_이 가이드에서는 Google에서 Firebase 작업 디스패처 라이브러리를 사용 하 여 백그라운드 작업을 예약 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Android 응용 프로그램을 사용자에 게 응답성을 유지 하는 가장 좋은 방법 중 하나는 백그라운드에서 복잡 하거나 오래 실행 되는 작업이 수행 되도록 하는 것입니다. 그러나 백그라운드 작업은 사용자의 장치 환경에 부정적인 영향을 주지 않습니다. 

예를 들어 백그라운드 작업은 3 ~ 4 분 마다 웹 사이트를 폴링하여 특정 데이터 집합에 대 한 변경 내용을 쿼리할 수 있습니다. 심각 하지는 않지만 배터리 수명에 심각한 영향을 미칠 수 있습니다. 응용 프로그램은 장치를 반복적으로 절전 모드에서 해제 하 고, CPU를 더 높은 전원 상태로 상승 하 고, 라디오를 켜고, 네트워크를 요청 하 고, 결과를 처리 합니다. 장치가 즉시 작동 중단 되 고 저전원 유휴 상태로 복귀 되지 않으므로 성능이 저하 됩니다. 잘못 예약 된 백그라운드 작업은 불필요 하 고 과도 한 전원 요구 사항으로 장치를 실수로 유지할 수 있습니다. 이 처럼 보입니다. (웹 사이트 폴링)는 상대적으로 짧은 시간 안에 장치를 사용할 수 없게 렌더링 합니다.

Android는 백그라운드에서 작업을 수행 하는 데 도움이 되는 다음과 같은 Api를 제공 하지만 지능적 작업 예약에는 충분 하지 않습니다. 

- **[의도](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** 서비스 &ndash; 의도 서비스는 작업을 수행 하는 데 유용 하지만 작업을 예약 하는 방법은 제공 하지 않습니다.
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; 이러한 api는 작업 예약만 허용 하지만 실제로 작업을 수행할 수 있는 방법은 제공 하지 않습니다. 또한 AlarmManager는 시간 기반 제약 조건만 허용 합니다. 즉, 특정 시간에 또는 특정 기간이 경과한 후에 알람을 발생 시킵니다. 
- **[Jobscheduler &ndash; jobscheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)** 는 운영 체제에서 작업을 예약 하는 데 사용할 수 있는 뛰어난 API입니다. 그러나 API 레벨 21 이상을 대상으로 하는 Android 앱에 대해서만 사용할 수 있습니다. 
- Android 앱 &ndash; **[브로드캐스트 수신기](~/android/app-fundamentals/broadcast-receivers.md)** 는 시스템 차원의 이벤트 또는 의도에 대 한 응답으로 작업을 수행 하도록 브로드캐스트 수신기를 설정할 수 있습니다. 그러나 브로드캐스트 수신기는 작업을 실행 해야 하는 경우에 대 한 제어를 제공 하지 않습니다. 또한 Android 운영 체제의 변경 내용으로 인해 브로드캐스트 수신기가 작동 하는 시기 또는 응답할 수 있는 작업 종류가 제한 됩니다. 

백그라운드 작업을 효율적으로 수행 하는 두 가지 주요 기능 ( _백그라운드 작업_ 또는 _작업_이 라고도 함)은 다음과 같습니다.

1. **작업 &ndash;를 지능적으로 예약** 하는 것은 응용 프로그램이 백그라운드에서 작업을 수행 하는 경우 적절 한 시민으로 작업을 수행 하는 것이 중요 합니다. 응용 프로그램에서 작업 실행을 요구 하지 않아야 하는 것이 가장 좋습니다. 대신, 응용 프로그램은 작업을 실행할 수 있는 경우 충족 해야 하는 조건을 지정 하 고 조건이 충족 될 때 해당 작업을 실행 하도록 예약 해야 합니다. 이렇게 하면 Android에서 작업을 지능적으로 수행할 수 있습니다. 예를 들어 네트워킹에 관련 된 오버 헤드를 최대한 활용 하기 위해 네트워크 요청을 동시에 실행 하도록 일괄 처리할 수 있습니다.
2. 백그라운드 작업을 수행 하는 코드 &ndash; **작업을 캡슐화** 하는 작업은 사용자 인터페이스와 독립적으로 실행 될 수 있는 불연속 구성 요소에 캡슐화 해야 하며, 일부 작업에 대해 작업이 완료 되지 않으면 비교적 쉽게 일정을 조정할 수 있습니다. 문서화.

Firebase 작업 디스패처는 Google의 라이브러리로 서, 백그라운드 작업 예약을 간소화 하는 흐름 API를 제공 합니다. Google Cloud Manager를 대체 하기 위한 것입니다. Firebase 작업 디스패처는 다음 Api로 구성 됩니다.

- `Firebase.JobDispatcher.JobService`은 백그라운드 작업에서 실행 되는 논리를 사용 하 여 확장 해야 하는 추상 클래스입니다.
- 작업을 시작 해야 하는 경우 `Firebase.JobDispatcher.JobTrigger` 선언 합니다. 이는 일반적으로 작업을 시작 하기 전에 30 초 이상 기다린 후 5 분 내에 작업을 실행 하는 등의 시간 창으로 표현 됩니다.
- `Firebase.JobDispatcher.RetryStrategy`는 작업이 제대로 실행 되지 않을 때 수행 해야 하는 작업에 대 한 정보를 포함 합니다. 다시 시도 전략은 작업을 다시 실행 하기 전에 대기 하는 시간을 지정 합니다. 
- `Firebase.JobDispatcher.Constraint`는 장치가 동일 하지 않은 네트워크에 있거나 요금을 청구 하는 등 작업을 실행 하기 전에 충족 해야 하는 조건을 설명 하는 선택적 값입니다.
- `Firebase.JobDispatcher.Job`은의 이전 Api를 `JobDispatcher`에서 예약할 수 있는 작업 단위로 통합 하는 API입니다. `Job.Builder` 클래스는 `Job`를 인스턴스화하는 데 사용 됩니다.
- `Firebase.JobDispatcher.JobDispatcher`은 앞의 세 가지 Api를 사용 하 여 운영 체제 작업을 예약 하 고 필요한 경우 작업을 취소 하는 방법을 제공 합니다.

Firebase 작업 디스패처에 대 한 작업을 예약 하려면 Xamarin.ios 응용 프로그램에서 `JobService` 클래스를 확장 하는 형식으로 코드를 캡슐화 해야 합니다. `JobService`은 작업 수명 중에 호출할 수 있는 세 가지 수명 주기 메서드를 포함 합니다.

- **`bool OnStartJob(IJobParameters parameters)`** &ndash;이 메서드는 작업이 수행 되 고 항상 구현 되어야 하는 위치입니다. 주 스레드에서 실행 됩니다. 이 메서드는 남은 작업이 있는 경우 `true`을 반환 하 고 작업이 완료 되 면 `false` 합니다. 
- **`bool OnStopJob(IJobParameters parameters)`** &ndash; 어떤 이유로 작업이 중지 될 때 호출 됩니다. 나중에 작업을 다시 예약 해야 하는 경우 `true`을 반환 해야 합니다.
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash;이 메서드는 `JobService` 비동기 작업을 완료 했을 때 호출 됩니다. 

작업을 예약 하기 위해 응용 프로그램은 `JobDispatcher` 개체를 인스턴스화합니다. 그런 다음 `Job.Builder`를 사용 하 여 작업을 실행할 작업을 예약 하는 `JobDispatcher`에 제공 되는 `Job` 개체를 만듭니다.

이 가이드에서는 Firebase 작업 디스패처를 Xamarin Android 응용 프로그램에 추가 하 고이를 사용 하 여 백그라운드 작업을 예약 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

Firebase 작업 디스패처에는 Android API 수준 9 이상이 필요 합니다. Firebase 작업 디스패처 라이브러리는 Google Play 서비스에서 제공 하는 일부 구성 요소에 의존 합니다. 장치에 Google Play 서비스 설치 되어 있어야 합니다.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Xamarin Android에서 Firebase 작업 디스패처 라이브러리 사용

Firebase 작업 디스패처를 시작 하려면 먼저 xamarin.ios 프로젝트에 [Firebase 디스패처 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) 를 추가 합니다. NuGet 패키지 관리자에서 **Firebase 디스패처** 패키지 (여전히 시험판 상태)를 검색 합니다.

Firebase Job 디스패처 라이브러리를 추가한 후 `JobService` 클래스를 만든 다음 `FirebaseJobDispatcher`인스턴스와 함께 실행 되도록 예약 합니다.

### <a name="creating-a-jobservice"></a>JobService 만들기

Firebase 작업 디스패처 라이브러리에서 수행 하는 모든 작업은 `Firebase.JobDispatcher.JobService` 추상 클래스를 확장 하는 형식에서 수행 해야 합니다. `JobService` 만들기는 Android framework를 사용 하 여 `Service`을 만드는 것과 매우 비슷합니다. 

1. `JobService` 클래스 확장
2. `ServiceAttribute`를 사용 하 여 하위 클래스를 데코 레이트 합니다. 반드시 필요한 것은 아니지만 `JobService`디버깅에 도움이 되도록 `Name` 매개 변수를 명시적으로 설정 하는 것이 좋습니다. 
3. `IntentFilter`를 추가 하 여 `JobService`를 **Androidmanifest**에 선언 합니다. 이를 통해 Firebase 작업 디스패처 라이브러리에서 `JobService`를 찾아 호출 하는 데도 도움이 됩니다.

다음 코드는 TPL을 사용 하 여 작업을 비동기적으로 수행 하는 응용 프로그램에 대 한 가장 간단한 `JobService`의 예입니다.

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

작업을 예약 하기 전에 `Firebase.JobDispatcher.FirebaseJobDispatcher` 개체를 만들어야 합니다. `FirebaseJobDispatcher`은 `JobService`일정을 예약 합니다. 다음 코드 조각은 `FirebaseJobDispatcher`의 인스턴스를 만드는 한 가지 방법입니다. 

 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

위의 코드 조각에서 `GooglePlayDriver`은 `FirebaseJobDispatcher` 장치에서 Google Play 서비스 일정 Api 중 일부와 상호 작용 하는 데 도움이 되는 클래스입니다. `context` 매개 변수는 활동과 같은 Android `Context`입니다. 현재 `GooglePlayDriver`는 Firebase 작업 디스패처 라이브러리의 유일한 `IDriver` 구현입니다. 

Firebase 작업 디스패처에 대 한 Xamarin Android 바인딩은 `Context`에서 `FirebaseJobDispatcher`를 만드는 확장 메서드를 제공 합니다. 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

`FirebaseJobDispatcher` 인스턴스화된 후에는 `Job`를 만들고 `JobService` 클래스에서 코드를 실행할 수 있습니다. `Job`은 `Job.Builder` 개체에 의해 생성 되며 다음 섹션에서 설명 합니다.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Firebase를 사용 하 여 작업을 만듭니다.

`Firebase.JobDispatcher.Job` 클래스는 `JobService`를 실행 하는 데 필요한 메타 데이터를 캡슐화 합니다. @ No__t_0_에는 작업을 실행 하기 전에 충족 되어야 하는 제약 조건, `Job` 반복 될 경우 또는 작업을 실행 하는 트리거 등의 정보가 포함 되어 있습니다.  최소한 `Job`에는 _태그_ (`FirebaseJobDispatcher`작업을 식별 하는 고유 문자열)와 실행 해야 하는 `JobService` 형식이 있어야 합니다. Firebase 작업 디스패처가 작업을 실행할 때 `JobService`를 인스턴스화합니다.  `Firebase.JobDispatcher.Job.JobBuilder` 클래스의 인스턴스를 사용 하 여 `Job`를 만듭니다. 

다음 코드 조각은 Xamarin. Android 바인딩을 사용 하 여 `Job`를 만드는 방법에 대 한 가장 간단한 예제입니다.

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder` 작업에 대 한 입력 값에 대 한 몇 가지 기본 유효성 검사를 수행 합니다. `Job.Builder`에서 `Job`을 만들 수 없는 경우 예외가 throw 됩니다.  `Job.Builder`는 다음 기본값을 사용 하 여 `Job`를 만듭니다.

- 장치를 다시 부팅 `Job` 다시 부팅 되 면 장치가 다시 부팅 될 때까지 `Job`의 _수명_ (실행 예약 시간)은 장치를 다시 &ndash; 부팅 해야 합니다.
- `Job` &ndash; 되풀이 되지 않으며 한 번만 실행 됩니다.
- `Job`는 가능한 한 빨리 실행 되도록 예약 됩니다.
- `Job`에 대 한 기본 재시도 전략은 _지 수 백오프_ 를 사용 하는 것입니다 (아래 섹션의 [RetryStrategy 설정](#Setting_a_RetryStrategy)섹션에서 자세히 설명).

### <a name="scheduling-a-job"></a>작업 예약

`Job`를 만든 후에는 `FirebaseJobDispatcher` 실행 하기 전에 예약 해야 합니다. `Job`를 예약 하는 방법에는 두 가지가 있습니다.

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

`FirebaseJobDispatcher.Schedule`에서 반환 되는 값은 다음 정수 값 중 하나입니다.

- `Job` 성공적으로 예약 &ndash;를 `FirebaseJobDispatcher.ScheduleResultSuccess` 합니다.
- `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; 일부 알 수 없는 문제가 발생 하 여 `Job` 예약 되지 않았습니다.
- `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; 잘못 된 `IDriver` 사용 되었거나 `IDriver`를 사용할 수 없습니다. 
- &ndash; `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` `Trigger` 지원 되지 않습니다.
- `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; 서비스가 올바르게 구성 되지 않았거나 사용할 수 없습니다.

### <a name="configuring-a-job"></a>작업 구성

작업을 사용자 지정할 수 있습니다. 작업을 사용자 지정할 수 있는 방법의 예는 다음과 같습니다.

- `Job` &ndash; [작업에 매개 변수를 전달](#Passing_Parameters_to_a_Job) 하려면 작업을 수행 하기 위해 추가 값이 필요할 수 있습니다 (예: 파일 다운로드).
- 특정 조건이 충족 될 때만 작업을 실행 해야 할 수 &ndash; [제약 조건을 설정](#Setting_Constraints) 합니다. 예를 들어 장치가 요금을 청구 하는 경우에만 `Job`를 실행 합니다. 
- Firebase 작업 디스패처가 응용 프로그램에서 작업을 실행 해야 하는 시간을 지정할 수 있도록 `Job` &ndash;를 실행 하는 [시기를 지정](#Setting_Job_Triggers) 합니다.  
- [실패 한 &ndash; 작업에 대 한 재시도 전략을 선언](#Setting_a_RetryStrategy) 합니다. _다시 시도 전략_ 은 완료 하지 못한 `Jobs`에서 수행할 작업에 대 한 지침 `FirebaseJobDispatcher` 제공 합니다. 

이러한 각 항목에 대해서는 다음 섹션에서 자세히 설명 합니다.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>작업에 매개 변수 전달

매개 변수는 `Job.Builder.SetExtras` 메서드와 함께 전달 되는 `Bundle`를 만들어 작업에 전달 됩니다.

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

`Bundle`는 `OnStartJob` 메서드의 `IJobParameters.Extras` 속성에서 액세스할 수 있습니다.

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>제약 조건 설정

제약 조건을 통해 장치의 비용 또는 배터리 소모를 줄일 수 있습니다. `Firebase.JobDispatcher.Constraint` 클래스는 이러한 제약 조건을 정수 값으로 정의 합니다.

- 장치가 연결 되지 않은 네트워크에 연결 된 경우에만 작업을 실행 &ndash; `Constraint.OnUnmeteredNetwork` 합니다. 이는 사용자가 데이터 요금을 발생 시 키 지 않도록 방지 하는 데 유용 합니다.
- &ndash; `Constraint.OnAnyNetwork` 장치가 연결 된 네트워크에서 작업을 실행 합니다. `Constraint.OnUnmeteredNetwork`와 함께 지정 하는 경우이 값은 우선 순위를 갖습니다.
- 장치가 충전 중인 경우에만 작업을 실행 &ndash; `Constraint.DeviceCharging` 합니다.

제약 조건은 `Job.Builder.SetConstraint` 메서드를 사용 하 여 설정 됩니다. 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

`JobTrigger`는 작업을 시작 해야 하는 시기에 대 한 지침을 운영 체제에 제공 합니다. `JobTrigger`에는 `Job` 실행 해야 하는 예약 된 시간을 정의 하는 _실행 창이_ 있습니다. 실행 창에는 _시작 창_ 값과 _종료 창_ 값이 있습니다. 시작 기간은 장치가 작업을 실행 하기 전에 대기 해야 하는 시간 (초)이 고, 종료 기간 값은 `Job`를 실행 하기 전까지 대기 하는 최대 시간 (초)입니다. 

`Firebase.Jobdispatcher.Trigger.ExecutionWindow` 메서드를 사용 하 여 `JobTrigger`를 만들 수 있습니다.  예 `Trigger.ExecutionWindow(15,60)` 들어 작업을 예약 된 시간에서 15 ~ 60 초 사이에 실행 해야 함을 의미 합니다. `Job.Builder.SetTrigger` 메서드는 다음에 사용 됩니다. 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

작업에 대 한 기본 `JobTrigger`은 `Trigger.Now`값으로 표시 되며,이 값은 작업이 예약 된 후 가능한 한 빨리 실행 되도록 지정 합니다.

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>RetryStrategy 설정

`Firebase.JobDispatcher.RetryStrategy`는 실패 한 작업을 다시 실행 하기 전에 장치에서 사용 해야 하는 지연의 양을 지정 하는 데 사용 됩니다. `RetryStrategy`에는 실패 한 작업을 다시 예약 하는 데 사용 되는 시간 기준 알고리즘과 작업을 예약 해야 하는 기간을 지정 하는 실행 기간을 정의 하는 _정책이_있습니다. 이러한 다시 _예약 창은_ 두 값으로 정의 됩니다. 첫 번째 값은 작업을 다시 예약 하기 전까지 대기 하는 시간 (초) ( _초기 백오프_ 값)이 고, 두 번째 값은 작업을 실행 해야 하는 최대 시간 (초) ( _최대 백오프_ 값)입니다. 

두 가지 유형의 재시도 정책은 다음 int 값으로 식별 됩니다.

- _지 수 백오프_ 정책을 &ndash; `RetryStrategy.RetryPolicyExponential` 각 실패 후에 초기 백오프 값이 급격히 증가 합니다. 작업이 처음으로 실패 하면 라이브러리는 작업을 다시 예약 하기 전에 지정 된 초기 간격을 대기 합니다. 예 &ndash; 30 초. 작업이 실패 하는 두 번째 경우 라이브러리는 작업을 실행 하기 전에 최소 60 초 정도 기다립니다. 세 번째 실패 후 라이브러리는 120 초 정도 대기 하 게 됩니다. Firebase 작업 디스패처 라이브러리의 기본 `RetryStrategy`는 `RetryStrategy.DefaultExponential` 개체로 표시 됩니다. 초기 백오프 30 초이 고 최대 백오프는 3600 초입니다.
- `RetryStrategy.RetryPolicyLinear` &ndash;이 전략은 작업이 성공할 때까지 설정 된 간격으로 실행 되도록 다시 예약 해야 하는 _선형 백오프_ . 선형 백오프는 가능한 한 빨리 완료 되어야 하는 작업 또는 자신을 신속 하 게 해결 하는 문제에 가장 적합 합니다. Firebase 작업 디스패처 라이브러리는 30 초 이상 3600 초 이상의 다시 예약 기간을 포함 하는 `RetryStrategy.DefaultLinear` 정의 합니다.

`FirebaseJobDispatcher.NewRetryStrategy` 메서드를 사용 하 여 사용자 지정 `RetryStrategy`를 정의할 수 있습니다. 세 개의 매개 변수를 사용 합니다.

1. &ndash; `int policy` _정책은_ 이전 `RetryStrategy` 값, `RetryStrategy.RetryPolicyLinear`또는 `RetryStrategy.RetryPolicyExponential`중 하나입니다.
2. `int initialBackoffSeconds` &ndash; _초기 백오프_ 는 작업을 다시 실행 하기 전에 필요한 지연 시간 (초)입니다. 이 값의 기본값은 30 초입니다. 
3. _최대 백오프_ 값 &ndash; `int maximumBackoffSeconds` 작업을 다시 실행 하기 전에 지연 하는 최대 시간 (초)을 선언 합니다. 기본값은 3600 초입니다. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>작업 취소

예약 된 모든 작업을 취소 하거나 `FirebaseJobDispatcher.CancelAll()` 메서드나 `FirebaseJobDispatcher.Cancel(string)` 메서드를 사용 하 여 단일 작업만 취소할 수 있습니다.

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

두 메서드는 모두 정수 값을 반환 합니다.

- 작업이 성공적으로 취소 &ndash; `FirebaseJobDispatcher.CancelResultSuccess`입니다.
- &ndash; `FirebaseJobDispatcher.CancelResultUnknownError` 오류가 발생 하 여 작업을 취소할 수 없습니다.
- `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; 사용할 수 있는 유효한 `IDriver` 없으므로 `FirebaseJobDispatcher` 작업을 취소할 수 없습니다.

## <a name="summary"></a>요약

이 가이드에서는 Firebase 작업 디스패처를 사용 하 여 백그라운드에서 작업을 지능적으로 수행 하는 방법에 대해 설명 했습니다. `JobService` 수행할 작업을 캡슐화 하는 방법 및 `FirebaseJobDispatcher`를 사용 하 여 해당 작업을 예약 하 고, `JobTrigger` 조건을 지정 하 고 `RetryStrategy`를 사용 하 여 오류를 처리 하는 방법을 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [NuGet의 Firebase 디스패처](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [firebase-GitHub의 디스패처](https://github.com/firebase/firebase-jobdispatcher-android)
- [Firebase 디스패처 바인딩](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [인텔리전트 작업-예약](https://developer.android.com/topic/performance/scheduling.html)
- [Android 배터리 및 메모리 최적화-Google i/o 2016 (비디오)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
