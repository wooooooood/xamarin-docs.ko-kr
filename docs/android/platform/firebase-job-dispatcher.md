---
title: Firebase 작업 디스패처
description: 이 가이드에서는 Google에서 Firebase 작업 디스패처 라이브러리를 사용 하 여 백그라운드 작업을 예약 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: cb7e8aaca13405aedd422288421d497653ddbfe8
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761200"
---
# <a name="firebase-job-dispatcher"></a>Firebase 작업 디스패처

_이 가이드에서는 Google에서 Firebase 작업 디스패처 라이브러리를 사용 하 여 백그라운드 작업을 예약 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Android 응용 프로그램을 사용자에 게 응답성을 유지 하는 가장 좋은 방법 중 하나는 백그라운드에서 복잡 하거나 오래 실행 되는 작업이 수행 되도록 하는 것입니다. 그러나 백그라운드 작업은 사용자의 장치 환경에 부정적인 영향을 주지 않습니다. 

예를 들어 백그라운드 작업은 3 ~ 4 분 마다 웹 사이트를 폴링하여 특정 데이터 집합에 대 한 변경 내용을 쿼리할 수 있습니다. 심각 하지는 않지만 배터리 수명에 심각한 영향을 미칠 수 있습니다. 응용 프로그램은 장치를 반복적으로 절전 모드에서 해제 하 고, CPU를 더 높은 전원 상태로 상승 하 고, 라디오를 켜고, 네트워크를 요청 하 고, 결과를 처리 합니다. 장치가 즉시 작동 중단 되 고 저전원 유휴 상태로 복귀 되지 않으므로 성능이 저하 됩니다. 잘못 예약 된 백그라운드 작업은 불필요 하 고 과도 한 전원 요구 사항으로 장치를 실수로 유지할 수 있습니다. 이 처럼 보입니다. (웹 사이트 폴링)는 상대적으로 짧은 시간 안에 장치를 사용할 수 없게 렌더링 합니다.

Android는 백그라운드에서 작업을 수행 하는 데 도움이 되는 다음과 같은 Api를 제공 하지만 지능적 작업 예약에는 충분 하지 않습니다. 

- **[의도 서비스](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; 내재 된 서비스는 작업을 수행 하는 데 유용 하지만 작업을 예약 하는 방법은 제공 하지 않습니다.
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; 이러한 api는 작업 예약만 허용 하지만 실제로 작업을 수행할 수 있는 방법은 제공 하지 않습니다. 또한 AlarmManager는 시간 기반 제약 조건만 허용 합니다. 즉, 특정 시간에 또는 특정 기간이 경과한 후에 알람을 발생 시킵니다. 
- **[Jobscheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)** &ndash; Jobschedule은 운영 체제에서 작업을 예약 하는 데 사용할 수 있는 뛰어난 API입니다. 그러나 API 레벨 21 이상을 대상으로 하는 Android 앱에 대해서만 사용할 수 있습니다. 
- **[브로드캐스트 수신기](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Android 앱은 시스템 차원의 이벤트 또는 의도에 대 한 응답으로 작업을 수행 하도록 브로드캐스트 수신기를 설정할 수 있습니다. 그러나 브로드캐스트 수신기는 작업을 실행 해야 하는 경우에 대 한 제어를 제공 하지 않습니다. 또한 Android 운영 체제의 변경 내용으로 인해 브로드캐스트 수신기가 작동 하는 시기 또는 응답할 수 있는 작업 종류가 제한 됩니다. 

백그라운드 작업을 효율적으로 수행 하는 두 가지 주요 기능 ( _백그라운드 작업_ 또는 _작업_이 라고도 함)은 다음과 같습니다.

1. **지능적으로 작업 예약** &ndash; 응용 프로그램이 백그라운드에서 작업을 수행 하는 경우에는 적절 한 시민으로 작업을 수행 하는 것이 중요 합니다. 응용 프로그램에서 작업 실행을 요구 하지 않아야 하는 것이 가장 좋습니다. 대신, 응용 프로그램은 작업을 실행할 수 있는 경우 충족 해야 하는 조건을 지정 하 고 조건이 충족 될 때 해당 작업을 실행 하도록 예약 해야 합니다. 이렇게 하면 Android에서 작업을 지능적으로 수행할 수 있습니다. 예를 들어 네트워킹에 관련 된 오버 헤드를 최대한 활용 하기 위해 네트워크 요청을 동시에 실행 하도록 일괄 처리할 수 있습니다.
2. **작업 캡슐화** &ndash; 백그라운드 작업을 수행 하는 코드는 사용자 인터페이스와 독립적으로 실행 될 수 있는 불연속 구성 요소에 캡슐화 되어야 하며, 어떤 이유로 작업이 완료 되지 않으면 비교적 쉽게 일정을 조정할 수 있습니다.

Firebase 작업 디스패처는 Google의 라이브러리로 서, 백그라운드 작업 예약을 간소화 하는 흐름 API를 제공 합니다. Google Cloud Manager를 대체 하기 위한 것입니다. Firebase 작업 디스패처는 다음 Api로 구성 됩니다.

- 는 `Firebase.JobDispatcher.JobService` 백그라운드 작업에서 실행 되는 논리를 사용 하 여 확장 되어야 하는 추상 클래스입니다.
- 는 `Firebase.JobDispatcher.JobTrigger` 작업을 시작 해야 할 때를 선언 합니다. 이는 일반적으로 작업을 시작 하기 전에 30 초 이상 기다린 후 5 분 내에 작업을 실행 하는 등의 시간 창으로 표현 됩니다.
- 에 `Firebase.JobDispatcher.RetryStrategy` 는 작업이 제대로 실행 되지 않을 때 수행 해야 하는 작업에 대 한 정보가 포함 되어 있습니다. 다시 시도 전략은 작업을 다시 실행 하기 전에 대기 하는 시간을 지정 합니다. 
- 는 `Firebase.JobDispatcher.Constraint` 장치가 동일 하지 않은 네트워크에 있거나 요금을 청구 하는 등 작업을 실행 하기 전에 충족 해야 하는 조건을 설명 하는 선택적 값입니다.
- 는의 이전 api를에서 예약할 `JobDispatcher`수 있는 작업 단위로 통합 하는 API 입니다.`Firebase.JobDispatcher.Job` 클래스는를 `Job`인스턴스화하는 데 사용 됩니다. `Job.Builder`
- 는 `Firebase.JobDispatcher.JobDispatcher` 이전 세 가지 api를 사용 하 여 운영 체제 작업을 예약 하 고 필요한 경우 작업을 취소 하는 방법을 제공 합니다.

Firebase 작업 디스패처에 대 한 작업을 예약 하려면 xamarin.ios 응용 프로그램에서 `JobService` 클래스를 확장 하는 형식으로 코드를 캡슐화 해야 합니다. `JobService`에는 작업 수명 중에 호출할 수 있는 세 가지 수명 주기 방법이 있습니다.

- **`bool OnStartJob(IJobParameters parameters)`** &ndash; 이 메서드는 작업이 수행 되 고 항상 구현 되어야 하는 위치입니다. 주 스레드에서 실행 됩니다. 이 메서드는 남은 `true` 작업이 있는 경우를 반환 `false` 하 고 작업이 완료 되 면를 반환 합니다. 
- **`bool OnStopJob(IJobParameters parameters)`** &ndash; 이는 어떤 이유로 작업이 중지 될 때 호출 됩니다. 나중에 작업 `true` 을 다시 예약 해야 하는 경우를 반환 해야 합니다.
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** 이 메서드는가 비동기 작업 `JobService` 을 완료 했을 때 호출 됩니다. &ndash; 

작업을 예약 하기 위해 응용 프로그램은 개체를 `JobDispatcher` 인스턴스화합니다. 그런 다음를 사용 하 여 작업을 `Job` 실행할 작업을 예약 하 `JobDispatcher` 는에 제공 되는 개체를 만듭니다. `Job.Builder`

이 가이드에서는 Firebase 작업 디스패처를 Xamarin Android 응용 프로그램에 추가 하 고이를 사용 하 여 백그라운드 작업을 예약 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

Firebase 작업 디스패처에는 Android API 수준 9 이상이 필요 합니다. Firebase 작업 디스패처 라이브러리는 Google Play 서비스에서 제공 하는 일부 구성 요소에 의존 합니다. 장치에 Google Play 서비스 설치 되어 있어야 합니다.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Xamarin Android에서 Firebase 작업 디스패처 라이브러리 사용

Firebase 작업 디스패처를 시작 하려면 먼저 xamarin.ios 프로젝트에 [Firebase 디스패처 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) 를 추가 합니다. NuGet 패키지 관리자에서 **Firebase 디스패처** 패키지 (여전히 시험판 상태)를 검색 합니다.

Firebase Job 디스패처 라이브러리를 추가한 후 `JobService` 클래스를 만든 다음 인스턴스 `FirebaseJobDispatcher`를 사용 하 여 실행 되도록 예약 합니다.

### <a name="creating-a-jobservice"></a>JobService 만들기

Firebase 작업 디스패처 라이브러리에서 수행 하는 모든 작업은 추상 클래스를 `Firebase.JobDispatcher.JobService` 확장 하는 형식에서 수행 해야 합니다. 를 `JobService` 만드는 것은 Android framework를 사용 `Service` 하 여을 만드는 것과 매우 비슷합니다. 

1. `JobService` 클래스 확장
2. 를 사용 하 `ServiceAttribute`여 하위 클래스를 데코 레이트 합니다. 반드시 필요한 것은 아니지만를 `Name` `JobService`디버깅 하는 데 도움이 되도록 매개 변수를 명시적으로 설정 하는 것이 좋습니다. 
3. 를 추가 `JobService` 하 여를 **androidmanifest**에 선언 합니다. `IntentFilter` 이는 Firebase 작업 디스패처 라이브러리에서를 `JobService`찾아 호출 하는 데도 도움이 됩니다.

다음 코드는 TPL을 사용 하 여 비동기적 `JobService` 으로 작업을 수행 하는 응용 프로그램에 대 한 가장 간단한 예제입니다.

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

작업을 예약 하려면 먼저 `Firebase.JobDispatcher.FirebaseJobDispatcher` 개체를 만들어야 합니다. 는를 예약 하 `JobService`는 일을담당합니다.`FirebaseJobDispatcher` 다음 코드 조각은 인스턴스 `FirebaseJobDispatcher`를 만드는 한 가지 방법입니다. 

 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

위의 코드 조각 `GooglePlayDriver` 에서는 장치에서 Google Play 서비스의 일정 예약 api `FirebaseJobDispatcher` 중 일부와 상호 작용 하는 데 도움이 되는 클래스입니다. 매개 변수 `context` 는 활동과 같은 `Context`Android입니다. 현재 Firebase 작업 디스패처 라이브러리 `IDriver` 의 유일한 구현 입니다.`GooglePlayDriver` 

Firebase 작업 디스패처에 대 한 Xamarin Android 바인딩은에서을 `FirebaseJobDispatcher` `Context`만드는 확장 메서드를 제공 합니다. 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

이 인스턴스화된 후에는를 `Job` 만들고 `JobService` 클래스에서 코드를 실행할 수 있습니다. `FirebaseJobDispatcher` 는 `Job` `Job.Builder` 개체에 의해 생성 되며 다음 섹션에서 설명 됩니다.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Firebase를 사용 하 여 작업을 만듭니다.

클래스 `Firebase.JobDispatcher.Job` 는를 `JobService`실행 하는 데 필요한 메타 데이터를 캡슐화 합니다. 에는 작업을 실행 하기 전에 충족 되어야 하는 제약 조건, `Job` 가 반복 되는 경우 또는 작업을 실행 하는 트리거 등의 정보가포함되어있습니다.`Job`  최소한 `Job` 은 _태그_ (작업 `FirebaseJobDispatcher`을 식별 하는 고유 문자열)와 실행 해야 `JobService` 하는의 형식을 포함 해야 합니다. 작업을 실행할 시간이 면 Firebase 작업 `JobService` 디스패처가를 인스턴스화합니다.  는 `Job` 클래스`Firebase.JobDispatcher.Job.JobBuilder` 의 인스턴스를 사용 하 여 만듭니다. 

다음 코드 조각은 xamarin.ios 바인딩을 사용 하 여를 `Job` 만드는 가장 간단한 예제입니다.

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

은 `Job.Builder` 작업에 대 한 입력 값에 대 한 몇 가지 기본 유효성 검사를 수행 합니다. 에서 `Job.Builder` 를`Job`만들 수 없는 경우 예외가 throw 됩니다.  는 `Job.Builder` 다음과 같은 기본값 `Job` 을 사용 하 여을 만듭니다.

- 장치를 다시 부팅 &ndash; 하는경우장치를다시부팅하는데걸리는시간(실행예약시간)은장치를다시부팅하는경우에만손실됩니다.`Job` `Job`
- `Job` 이 반복&ndash; 되지 않으면 한 번만 실행 됩니다.
- 는 `Job` 가능한 한 빨리 실행 되도록 예약 됩니다.
- 에 대 한 `Job` 기본 재시도 전략은 _지 수 백오프_ 를 사용 하는 것입니다 (아래 섹션의 [RetryStrategy 설정](#Setting_a_RetryStrategy)섹션에서 자세히 설명).

### <a name="scheduling-a-job"></a>작업 예약

을 만든 `Job`후에는 `FirebaseJobDispatcher` 을 실행 하기 전에를 사용 하 여 예약 해야 합니다. 를 예약 하 `Job`는 방법에는 다음 두 가지가 있습니다.

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

에서 `FirebaseJobDispatcher.Schedule` 반환 되는 값은 다음 정수 값 중 하나입니다.

- `FirebaseJobDispatcher.ScheduleResultSuccess`가성공적으로예약되었습니다.&ndash; `Job`
- `FirebaseJobDispatcher.ScheduleResultUnknownError`일부 알 수 없는 문제가 발생 하 `Job` 여이 예약 되지 않았습니다. &ndash;
- `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable`잘못 된이 사용 되었거나를 `IDriver` 사용할 수 없습니다. `IDriver` &ndash; 
- `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger`&ndash; 은`Trigger` 지원 되지 않습니다.
- `FirebaseJobDispatcher.ScheduleResultBadService`&ndash; 서비스가 올바르게 구성 되지 않았거나 사용할 수 없습니다.

### <a name="configuring-a-job"></a>작업 구성

작업을 사용자 지정할 수 있습니다. 작업을 사용자 지정할 수 있는 방법의 예는 다음과 같습니다.

- [작업에 매개 변수 전달](#Passing_Parameters_to_a_Job) &ndash; 에서`Job` 파일을 다운로드 하는 등의 작업을 수행 하려면 추가 값이 필요할 수 있습니다.
- [제약 조건 설정](#Setting_Constraints) &ndash; 특정 조건이 충족 될 때만 작업을 실행 해야 할 수 있습니다. 예를 들어 장치가 요금을 `Job` 청구 하는 경우에만를 실행 합니다. 
- [에서`Job` ](#Setting_Job_Triggers) Firebase 작업을 실행 &ndash; 해야 하는 시간을 지정 하 여 응용 프로그램에서 작업을 실행 해야 하는 시간을 지정할 수 있습니다.  
- [실패 한 작업에 대 한 재시도 전략 선언](#Setting_a_RetryStrategy) 다시 시도 _전략_ 은 완료 하지 못한 `FirebaseJobDispatcher` 로 `Jobs` 수행할 작업에 대 한 지침을 제공 합니다. &ndash; 

이러한 각 항목에 대해서는 다음 섹션에서 자세히 설명 합니다.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>작업에 매개 변수 전달

매개 변수는 `Job.Builder.SetExtras` 메서드와 함께 전달 되는를 `Bundle` 만들어 작업에 전달 됩니다.

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

메서드의 `Bundle` 속성`IJobParameters.Extras` 에서에 액세스할 수 있습니다. `OnStartJob`

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>제약 조건 설정

제약 조건을 통해 장치의 비용 또는 배터리 소모를 줄일 수 있습니다. 클래스 `Firebase.JobDispatcher.Constraint` 는 이러한 제약 조건을 정수 값으로 정의 합니다.

- `Constraint.OnUnmeteredNetwork`&ndash; 장치가 연결 되지 않은 네트워크에 연결 된 경우에만 작업을 실행 합니다. 이는 사용자가 데이터 요금을 발생 시 키 지 않도록 방지 하는 데 유용 합니다.
- `Constraint.OnAnyNetwork`&ndash; 장치가 연결 되어 있는 모든 네트워크에서 작업을 실행 합니다. 과 `Constraint.OnUnmeteredNetwork`함께 지정 하는 경우이 값은 우선 순위를 갖습니다.
- `Constraint.DeviceCharging`&ndash; 장치가 요금을 청구 하는 경우에만 작업을 실행 합니다.

제약 조건은 `Job.Builder.SetConstraint` 메서드를 사용 하 여 설정 됩니다. 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

는 `JobTrigger` 작업을 시작 해야 하는 경우 운영 체제에 대 한 지침을 제공 합니다. 에 는`Job` 를 실행 해야 하는 예약 된 시간을 정의 하는 _실행 창이_ 있습니다.`JobTrigger` 실행 창에는 _시작 창_ 값과 _종료 창_ 값이 있습니다. 시작 기간은 장치가 작업을 실행 하기 전에 대기 해야 하는 시간 (초)이 고, 종료 기간 값은을 `Job`실행 하기 전에 대기 하는 최대 시간 (초)입니다. 

는 `JobTrigger` `Firebase.Jobdispatcher.Trigger.ExecutionWindow` 메서드를 사용 하 여 만들 수 있습니다.  예를 `Trigger.ExecutionWindow(15,60)` 들어 작업은 예약 된 시간에서 15 초에서 60 초 사이에 실행 되어야 합니다. `Job.Builder.SetTrigger` 메서드는 다음에 사용 됩니다. 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

작업의 `JobTrigger` 기본값은 값으로 표시 되며,이 `Trigger.Now`값은 작업이 예약 된 후 가능한 한 빨리 실행 되도록 지정 합니다.

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>RetryStrategy 설정

는 실패 한 작업을 다시 실행 하기 전에 장치에서 사용 해야 하는 지연의 양을 지정 하는 데 사용 됩니다.`Firebase.JobDispatcher.RetryStrategy` 에 `RetryStrategy` 는 실패 한 작업을 다시 예약 하는 데 사용 되는 시간 기준 알고리즘과 작업을 예약 해야 하는 창을 지정 하는 실행 기간을 정의 하는 _정책이_있습니다. 이러한 다시 _예약 창은_ 두 값으로 정의 됩니다. 첫 번째 값은 작업을 다시 예약 하기 전까지 대기 하는 시간 (초) ( _초기 백오프_ 값)이 고, 두 번째 값은 작업을 실행 해야 하는 최대 시간 (초) ( _최대 백오프_ 값)입니다. 

두 가지 유형의 재시도 정책은 다음 int 값으로 식별 됩니다.

- `RetryStrategy.RetryPolicyExponential`지 수 백오프 정책은 각 실패 후에 초기 백오프 값을 지 수로 늘립니다. &ndash; 작업이 처음으로 실패 하면 라이브러리는 작업 &ndash; 예제를 30 초 간격으로 다시 예약 하기 전에 지정 된 초기 간격을 대기 합니다 (_m). 작업이 실패 하는 두 번째 경우 라이브러리는 작업을 실행 하기 전에 최소 60 초 정도 기다립니다. 세 번째 실패 후 라이브러리는 120 초 정도 대기 하 게 됩니다. Firebase 작업 `RetryStrategy` 디스패처 라이브러리의 기본값은 `RetryStrategy.DefaultExponential` 개체로 표시 됩니다. 초기 백오프 30 초이 고 최대 백오프는 3600 초입니다.
- `RetryStrategy.RetryPolicyLinear`이 전략은 작업이 성공할 때까지 설정 된 간격으로 실행 되도록 다시 예약 되어야 하는 _선형 백오프._ &ndash; 선형 백오프는 가능한 한 빨리 완료 되어야 하는 작업 또는 자신을 신속 하 게 해결 하는 문제에 가장 적합 합니다. Firebase 작업 디스패처 라이브러리는 최소 30 `RetryStrategy.DefaultLinear` 초에서 최대 3600 초 사이의 다시 예약 기간을 포함 하는를 정의 합니다.

메서드`FirebaseJobDispatcher.NewRetryStrategy` 를 사용 하 여 사용자 지정 `RetryStrategy` 을 정의할 수 있습니다. 세 개의 매개 변수를 사용 합니다.

1. `int policy`&ndash; _정책은_ 이전 값`RetryStrategy` ,또는`RetryStrategy.RetryPolicyExponential`중 하나입니다. `RetryStrategy.RetryPolicyLinear`
2. `int initialBackoffSeconds`초기 백오프 작업을 다시 실행 하기 전에 필요한 지연 시간 (초)입니다. &ndash; 이 값의 기본값은 30 초입니다. 
3. `int maximumBackoffSeconds`Maximum 백오프 값은 작업을 다시 실행 하기 전에 지연 하는 최대 시간 (초)을 선언 합니다. &ndash; 기본값은 3600 초입니다. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>작업 취소

예약 된 모든 작업을 취소 하거나 `FirebaseJobDispatcher.CancelAll()` 메서드 `FirebaseJobDispatcher.Cancel(string)` 또는 메서드를 사용 하 여 단일 작업만 취소할 수 있습니다.

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

두 메서드는 모두 정수 값을 반환 합니다.

- `FirebaseJobDispatcher.CancelResultSuccess`&ndash; 작업이 성공적으로 취소 되었습니다.
- `FirebaseJobDispatcher.CancelResultUnknownError`&ndash; 오류가 발생 하 여 작업을 취소할 수 없습니다.
- `FirebaseJobDispatcher.CancelResult.NoDriverAvailable`사용할 수 있는 유효한 `FirebaseJobDispatcher` `IDriver` 메모리가 없으므로에서 작업을 취소할 수 없습니다. &ndash;

## <a name="summary"></a>요약

이 가이드에서는 Firebase 작업 디스패처를 사용 하 여 백그라운드에서 작업을 지능적으로 수행 하는 방법에 대해 설명 했습니다. 에서 수행 `JobService` 되는 작업을 캡슐화 하는 방법과를 `FirebaseJobDispatcher` 사용 하 여 해당 작업을 예약 하는 방법을 설명 하 고를 사용 하 `JobTrigger` 여 조건을 지정 하 `RetryStrategy`고 오류를로 처리 하는 방법을 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [NuGet의 Firebase 디스패처](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [firebase-GitHub의 디스패처](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher Binding](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [인텔리전트 작업-예약](https://developer.android.com/topic/performance/scheduling.html)
- [Android 배터리 및 메모리 최적화-Google i/o 2016 (비디오)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
