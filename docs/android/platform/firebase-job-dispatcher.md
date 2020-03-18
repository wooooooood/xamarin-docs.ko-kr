---
title: Firebase 작업 디스패처
description: 이 가이드에서는 Google의 Firebase 작업 디스패처 라이브러리를 사용하여 백그라운드 작업을 예약하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
ms.openlocfilehash: 280fe11f935db0a364f3342b22bb9544cdda1e6d
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020239"
---
# <a name="firebase-job-dispatcher"></a>Firebase 작업 디스패처

_이 가이드에서는 Google의 Firebase 작업 디스패처 라이브러리를 사용하여 백그라운드 작업을 예약하는 방법을 설명합니다._

## <a name="overview"></a>개요

Android 애플리케이션에서 사용자 응답성을 유지하는 가장 좋은 방법 중 하나는 복잡하거나 장기 실행되는 작업이 백그라운드에서 실행되도록 하는 것입니다. 그러나 백그라운드 작업이 사용자의 디바이스 환경에 부정적인 영향을 주지 않는 것이 중요합니다. 

예를 들어 백그라운드 작업은 3~4분마다 웹 사이트를 폴링하여 특정 데이터 세트에 대한 변경 내용을 쿼리할 수 있습니다. 무해한 것처럼 보이지만 배터리 사용 시간에 치명적인 영향을 미칠 수 있습니다. 애플리케이션은 반복적으로 디바이스를 절전 모드에서 해제하고, CPU를 더 높은 전원 상태로 상승하고, 무선을 켜고, 네트워크를 요청하고, 결과를 처리합니다. 디바이스가 즉시 작동 중단되고 저전원 유휴 상태로 복귀하지 않으므로 상황은 더 심각해집니다. 잘못 예약된 백그라운드 작업은 디바이스에서 불필요하고 과도한 전원 요구 사항을 지속할 수 있습니다. 이처럼 겉으로는 무해한 작업(웹 사이트 폴링)이 비교적 짧은 시간에 디바이스를 불안정하게 만듭니다.

Android는 다음과 같이 백그라운드에서 작업을 수행하는 데 도움이 되는 API를 제공하지만 인텔리전트 작업 예약에는 충분하지 않습니다. 

- **[의도 서비스](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; 의도 서비스는 작업을 수행하는 데 유용하지만 작업을 예약하는 방법은 제공하지 않습니다.
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; 이러한 API는 작업 예약만 허용하고 실제로 작업을 수행할 수 있는 방법은 제공하지 않습니다. 또한 AlarmManager는 시간 기반 제약 조건만 허용합니다. 즉, 특정 시간에 또는 특정 기간이 경과한 후에 알람을 발생시킵니다. 
- **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)** &ndash; JobSchedule은 운영 체제와 함께 작동하여 작업을 예약하는 뛰어난 API입니다. 그러나 API 레벨 21 이상을 대상으로 하는 Android 앱에만 사용할 수 있습니다. 
- **[브로드캐스트 수신기](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Android 앱은 시스템 차원의 이벤트 또는 의도에 대한 응답으로 작업을 수행하도록 브로드캐스트 수신기를 설정할 수 있습니다. 그러나 브로드캐스트 수신기는 작업을 실행해야 하는 경우에 대한 제어를 제공하지 않습니다. 또한 Android 운영 체제의 변경 내용으로 인해 브로드캐스트 수신기가 작동하는 시기 또는 응답할 수 있는 작업 종류가 제한됩니다. 

백그라운드 작업(_백그라운드 작업_ 또는 _작업_이라고도 함)을 효율적으로 수행하기 위한 두 가지 주요 기능이 있습니다.

1. **지능적으로 작업 예약** &ndash; 애플리케이션이 백그라운드에서 작업을 수행하는 경우 협조적으로 작업을 수행하는 것이 중요합니다. 이상적으로는 애플리케이션이 작업 실행을 요구하지 않는 것이 가장 좋습니다. 대신, 애플리케이션에서는 작업을 실행할 수 있을 때 충족해야 하는 조건을 지정한 다음, 조건이 충족될 때 작업을 수행하도록 해당 작업을 예약해야 합니다. 이렇게 하면 Android에서 작업을 지능적으로 수행할 수 있습니다. 예를 들어 네트워킹에 관련된 오버헤드를 최대한 활용하기 위해 네트워크 요청을 동시에 실행하도록 일괄 처리할 수 있습니다.
2. **작업을 캡슐화** &ndash; 백그라운드 작업을 수행하는 코드는 사용자 인터페이스와 독립적으로 실행될 수 있는 개별 구성 요소에 캡슐화해야 하며 어떤 이유로 작업이 완료되지 않을 경우에는 비교적 쉽게 일정을 조정할 수 있습니다.

Firebase 작업 디스패처는 백그라운드 작업 예약을 간소화하는 흐름 API를 제공하는 Google의 라이브러리입니다. Google Cloud Manager를 대체하기 위해 개발되었습니다. Firebase 작업 디스패처는 다음 API로 구성됩니다.

- `Firebase.JobDispatcher.JobService`는 백그라운드 작업에서 실행될 논리를 사용하여 확장해야 하는 추상 클래스입니다.
- `Firebase.JobDispatcher.JobTrigger`는 작업을 시작해야 하는 시간을 선언합니다. 일반적으로 시간 기간으로 표현됩니다. 예를 들어 30초 이상 기다렸다가 작업을 시작하되, 작업을 5분 이내로 실행하도록 선언합니다.
- `Firebase.JobDispatcher.RetryStrategy`는 작업이 제대로 실행되지 않을 때 수행할 조치에 대한 정보를 포함합니다. 다시 시도 전략은 작업을 다시 실행할 때까지 기다려야 하는 시간을 지정합니다. 
- `Firebase.JobDispatcher.Constraint`는 디바이스가 비-요금제 네트워크에 연결되어 있거나 충전 중인 경우처럼 작업을 실행하려면 충족해야 하는 조건을 설명하는 선택적 값입니다.
- `Firebase.JobDispatcher.Job`은 이전 API를 `JobDispatcher`에서 예약할 수 있는 작업 단위로 통합하는 API입니다. `Job.Builder` 클래스는 `Job`을 인스턴스화하는 데 사용됩니다.
- `Firebase.JobDispatcher.JobDispatcher`는 위의 세 가지 API를 사용하여 운영 체제 작업을 예약하고, 필요한 경우 작업을 취소하는 방법을 제공합니다.

Firebase 작업 디스패처로 작업을 예약하려면 Xamarin.Android 애플리케이션에서 `JobService` 클래스를 확장하는 형식으로 코드를 캡슐화해야 합니다. `JobService`에는 작업 수명 주기 중에 호출할 수 있는 다음과 같은 세 가지 수명 주기 메서드가 있습니다.

- **`bool OnStartJob(IJobParameters parameters)`** &ndash; 이 메서드는 작업이 수행되고 항상 구현되어야 하는 위치입니다. 주 스레드에서 실행됩니다. 이 메서드는 남은 작업이 있으면 `true`을 반환하고, 작업이 완료되면 `false`를 반환 합니다. 
- **`bool OnStopJob(IJobParameters parameters)`** &ndash; 어떤 이유로 작업이 중지될 때 호출됩니다. 나중에 작업을 다시 예약해야 하는 경우 `true`를 반환해야 합니다.
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; 이 메서드는 `JobService`에서 비동기 작업을 완료했을 때 호출됩니다. 

작업을 예약하기 위해 애플리케이션은 `JobDispatcher` 개체를 인스턴스화합니다. 그런 다음, `Job.Builder`를 사용하여 `Job` 개체를 만듭니다. 이 개체는 `JobDispatcher`에 제공되고 디스패처는 실행할 작업을 예약합니다.

이 가이드에서는 Firebase 작업 디스패처를 Xamarin.Android 애플리케이션에 추가하고 백그라운드 작업을 예약하는 데 사용하는 방법을 설명합니다.

## <a name="requirements"></a>요구 사항

Firebase 작업 디스패처를 사용하려면 Android API 레벨 9 이상이 필요합니다. Firebase 작업 디스패처 라이브러리는 Google Play 서비스에서 제공하는 일부 구성 요소를 사용하므로 디바이스에 Google Play 서비스가 설치되어 있어야 합니다.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Xamarin.Android에서 Firebase 작업 디스패처 라이브러리 사용

Firebase 작업 디스패처를 시작하려면 먼저 xamarin.Android 프로젝트에 [Xamarin.Firebase.JobDispatcher NuGet 패키지](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)를 추가합니다. NuGet 패키지 관리자에서 **Xamarin.Firebase.JobDispatcher** 패키지를 검색합니다(여전히 시험판 상태).

Firebase 작업 디스패처 라이브러리를 추가한 후에는 `FirebaseJobDispatcher` 인스턴스를 사용하여 `JobService` 클래스를 만들고 실행되도록 예약합니다.

### <a name="creating-a-jobservice"></a>JobService 만들기

Firebase 작업 디스패처 라이브러리에서 수행하는 모든 작업은 `Firebase.JobDispatcher.JobService` 추상 클래스를 확장하는 형식으로 수행해야 합니다. `JobService` 만들기는 Android 프레임워크를 사용하여 `Service`를 만드는 방법과 매우 비슷합니다. 

1. `JobService` 클래스 확장
2. `ServiceAttribute`를 사용하여 서브클래스를 데코레이트합니다. 반드시 필요한 것은 아니지만, `JobService` 디버깅에 도움이 되도록 `Name` 매개 변수를 명시적으로 설정하는 것이 좋습니다. 
3. `IntentFilter`를 추가하여 **AndroidManifest.xml**에서 `JobService`를 선언합니다. 이렇게 하면 마찬가지로 Firebase 작업 디스패처 라이브러리가 `JobService`를 찾아 호출하는 데 도움이 됩니다.

다음 코드는 TPL을 사용하여 일부 작업을 비동기적으로 수행하는 애플리케이션에 대한 가장 간단한 `JobService` 예제입니다.

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

작업을 예약하려면 먼저 `Firebase.JobDispatcher.FirebaseJobDispatcher` 개체를 만들어야 합니다. `FirebaseJobDispatcher`는 `JobService` 일정을 예약하는 역할을 담당합니다. 다음 코드 조각은 `FirebaseJobDispatcher`의 인스턴스를 만드는 한 가지 방법입니다. 

 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

위의 코드 조각에서 `GooglePlayDriver`는 `FirebaseJobDispatcher`가 디바이스에서 Google Play 서비스의 일부 예약 API와 상호 작용하는 데 도움이 되는 클래스입니다. `context` 매개 변수는 작업 같은 Android `Context`입니다. 현재 `GooglePlayDriver`는 Firebase 작업 디스패처 라이브러리에 구현된 유일한 `IDriver`입니다. 

Firebase 작업 디스패처에 대한 Xamarin.Android 바인딩은 `Context`에서 `FirebaseJobDispatcher`를 만드는 확장 메서드를 제공합니다. 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

`FirebaseJobDispatcher`가 인스턴스화된 후에는 `Job`을 만들고 `JobService` 클래스에서 코드를 실행할 수 있습니다. `Job`은 `Job.Builder` 개체에 의해 생성되며 다음 섹션에서 설명하겠습니다.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Job.Builder를 사용하여 Firebase.JobDispatcher.Job 만들기

`Firebase.JobDispatcher.Job` 클래스는 `JobService`를 실행하는 데 필요한 메타데이터를 캡슐화하는 역할을 담당합니다. `Job`에는 작업을 실행하려면 충족해야 하는 제약 조건, `Job`이 되풀이 작업인지 여부, 작업을 실행하는 트리거 등의 정보가 포함되어 있습니다.  적어도 `Job`에는 _태그_(`FirebaseJobDispatcher` 작업을 식별하는 고유 문자열) 및 실행해야 하는 `JobService` 형식이 있어야 합니다. Firebase 작업 디스패처는 작업을 실행할 시간이 되면 `JobService`를 인스턴스화합니다.  `Job`은 `Firebase.JobDispatcher.Job.JobBuilder` 클래스의 인스턴스를 사용하여 생성됩니다. 

다음 코드 조각은 Xamarin.Android 바인딩을 사용하여 `Job`을 만드는 가장 간단한 방법을 보여주는 예제입니다.

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder`는 작업의 입력 값에 대한 몇 가지 기본 유효성 검사를 수행합니다. `Job.Builder`에서 `Job`을 만들 수 없는 경우 예외가 throw됩니다.  `Job.Builder`는 다음 기본값을 사용하여 `Job`을 만듭니다.

- `Job`의 _수명_(실행하도록 예약되는 시간)은 디바이스가 다시 부팅될 때까지입니다. 디바이스가 다시 부팅되면 `Job`이 손실됩니다.
- `Job`은 되풀이되지 않고 한 번만 실행됩니다.
- `Job`은 최대한 빨리 실행되도록 예약됩니다.
- `Job`에 대한 기본 다시 시도 전략은 _지수 백오프_를 사용하는 것입니다(아래의 [RetryStrategy 설정](#Setting_a_RetryStrategy) 섹션에서 자세히 설명).

### <a name="scheduling-a-job"></a>작업 예약

`Job`을 만든 후에는 `FirebaseJobDispatcher`를 사용하여 작업이 실행되도록 예약해야 합니다. `Job`을 예약하는 다음 두 가지 메서드가 있습니다.

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

`FirebaseJobDispatcher.Schedule`에서 반환하는 값은 다음 정수 값 중 하나입니다.

- `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; `Job`이 성공적으로 예약되었습니다.
- `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; 알 수 없는 문제가 발생하여 `Job`을 예약하지 못했습니다.
- `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; 잘못된 `IDriver`가 사용되었거나 어떤 이유로 `IDriver`를 사용할 수 없습니다. 
- `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; `Trigger`가 지원되지 않습니다.
- `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; 서비스가 올바르게 구성되지 않았거나 사용할 수 없습니다.

### <a name="configuring-a-job"></a>작업 구성

작업을 사용자 지정할 수 있습니다. 다음은 작업을 사용자 지정하는 방법의 예입니다.

- [작업에 매개 변수 전달](#Passing_Parameters_to_a_Job) &ndash; 작업을 수행하려면(예: 파일 다운로드) `Job`에 추가 값이 필요할 수 있습니다.
- [제약 조건 설정](#Setting_Constraints) &ndash; 특정 조건이 충족될 때만 작업을 실행해야 하는 경우가 있습니다. 예를 들어 디바이스가 충전 중일 때만 `Job`을 실행합니다. 
- [`Job` 실행 시기 지정](#Setting_Job_Triggers) &ndash; Firebase 작업 디스패처는 애플리케이션에서 작업 실행 시기를 지정할 수 있습니다.  
- [실패한 작업의 다시 시도 전략 선언](#Setting_a_RetryStrategy) &ndash; _다시 시도 전략_은 완료되지 않은 `Jobs`를 어떻게 처리할 것인지에 대한 지침을 `FirebaseJobDispatcher`에 제공합니다. 

이러한 각 토픽은 다음 섹션에서 자세히 설명합니다.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>작업에 매개 변수 전달

매개 변수는 `Job.Builder.SetExtras` 메서드와 함께 전달되는 `Bundle`을 만들어 작업에 전달됩니다.

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

`Bundle`은 `OnStartJob` 메서드의 `IJobParameters.Extras` 속성을 통해 액세스할 수 있습니다.

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>제약 조건 설정

제약 조건을 설정하여 디바이스의 비용 또는 배터리 소모를 줄일 수 있습니다. `Firebase.JobDispatcher.Constraint` 클래스는 이러한 제약 조건을 정수 값으로 정의합니다.

- `Constraint.OnUnmeteredNetwork` &ndash; 디바이스가 비-요금제 네트워크에 연결된 경우에만 작업을 실행합니다. 사용자가 데이터 요금을 발생시키지 않도록 방지하는 데 유용합니다.
- `Constraint.OnAnyNetwork` &ndash; 디바이스가 어떤 네트워크에서 연결되어 있든 작업을 실행합니다. `Constraint.OnUnmeteredNetwork`와 함께 지정할 경우 이 값이 우선 순위를 갖습니다.
- `Constraint.DeviceCharging` &ndash; 디바이스가 충전 중일 때만 작업을 실행합니다.

제약 조건은 다음과 같은 `Job.Builder.SetConstraint` 메서드를 사용하여 설정됩니다. 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

`JobTrigger`는 작업을 시작해야 하는 시기에 대한 지침을 운영 체제에 제공합니다. `JobTrigger`에는 `Job`이 실행되어야 하는 예약 시간을 정의하는 _실행 시간_이 있습니다. 실행 시간은 _시작 시간_ 값과 _종료 시간_ 값으로 구성됩니다. 시작 기간은 디바이스에서 작업을 실행할 때까지 대기해야 하는 시간(초)이고, 종료 시간 값은 `Job`을 실행할 때까지 대기해야 하는 최대시간(초)입니다. 

`JobTrigger`는 `Firebase.Jobdispatcher.Trigger.ExecutionWindow` 메서드를 사용하여 만들 수 있습니다.  예를 들어 `Trigger.ExecutionWindow(15,60)`는 작업이 예약된 시간으로부터 15~60초 사이에 실행해야 한다는 의미입니다. `Job.Builder.SetTrigger` 메서드는 다음 용도로 사용됩니다. 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

작업의 기본 `JobTrigger`는 `Trigger.Now` 값으로 표시되며, 이 값은 예약 후 최대한 빨리 작업이 실행되도록 지정합니다.

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>RetryStrategy 설정

`Firebase.JobDispatcher.RetryStrategy`는 실패한 작업을 다시 실행하기 전에 디바이스에서 사용해야 하는 지연의 양을 지정하는 데 사용됩니다. `RetryStrategy`에는 실패한 작업을 다시 예약하는 데 사용되는 시간 기반 알고리즘, 그리고 작업을 예약해야 하는 시간을 지정하는 실행 시간을 정의하는 _정책_이 있습니다. 이 _다시 예약 시간_은 두 가지 값을 통해 정의됩니다. 첫 번째 값은 작업을 다시 예약하기 전까지 대기하는 시간(초)(_초기 백오프_ 값)이고, 두 번째 값은 작업을 실행하기 전까지 대기해야 하는 최대 시간(초)(_최대 백오프_ 값)입니다. 

두 가지 형식의 다시 시도 정책은 다음 정수 값으로 구분됩니다.

- `RetryStrategy.RetryPolicyExponential` &ndash; _지수 백오프_ 정책은 각 실패 후 초기 백오프 값을 기하급수적으로 늘립니다. 작업이 처음으로 실패하면 라이브러리는 지정된 초기 간격(예: 30초) 동안 대기한 후 작업을 다시 예약합니다. 작업이 두 번째로 실패하면 라이브러리는 작업을 실행하기 전에 최소 60초 동안 대기합니다. 세 번째 실패 후에는 라이브러리가 120초 동안 대기하는 식으로 늘어납니다. Firebase 작업 디스패처 라이브러리의 기본 `RetryStrategy`는 `RetryStrategy.DefaultExponential` 개체로 표시됩니다. 초기 백오프는 30초이고 최대 백오프는 3600초입니다.
- `RetryStrategy.RetryPolicyLinear` &ndash; 이 전략은 (작업이 성공할 때까지) 설정된 간격으로 실행되도록 작업을 다시 예약해야 하는 _선형 백오프_입니다. 선형 백오프는 최대한 빨리 완료되어야 하는 작업 또는 자체적으로 신속하게 해결하는 문제에 가장 적합합니다. Firebase 작업 디스패처 라이브러리는 다시 예약 시간이 최소 30초 최대 3600초인 `RetryStrategy.DefaultLinear`를 정의합니다.

`FirebaseJobDispatcher.NewRetryStrategy` 메서드를 사용하여 사용자 지정 `RetryStrategy`를 정의할 수 있습니다. 이 메서드는 다음 세 가지 매개 변수를 사용합니다.

1. `int policy` &ndash; _정책_은 앞에서 나온 `RetryStrategy` 값, `RetryStrategy.RetryPolicyLinear` 또는 `RetryStrategy.RetryPolicyExponential` 중 하나입니다.
2. `int initialBackoffSeconds` &ndash; _초기 백오프_는 작업을 다시 실행하기 전에 대기해야 하는 지연 시간(초)입니다. 기본값은 30초입니다. 
3. `int maximumBackoffSeconds` &ndash; _최대 백오프_ 값은 작업을 다시 실행하기 전에 대기해야 하는 최대 시간(초)을 선언합니다. 기본값은 3600초입니다. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>작업 취소

`FirebaseJobDispatcher.CancelAll()` 메서드 또는 `FirebaseJobDispatcher.Cancel(string)` 메서드를 사용하여 예약된 모든 작업을 취소하거나 단일 작업만 취소할 수 있습니다.

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

다음과 같이 두 메서드 모두 정수 값을 반환합니다.

- `FirebaseJobDispatcher.CancelResultSuccess` &ndash; 작업이 성공적으로 취소되었습니다.
- `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; 오류로 인해 작업을 취소할 수 없습니다.
- `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; 유효한 `IDriver`가 없어서 `FirebaseJobDispatcher`에서 작업을 취소할 수 없습니다.

## <a name="summary"></a>요약

이 가이드에서는 Firebase 작업 디스패처를 사용하여 백그라운드에서 지능적으로 작업을 수행하는 방법에 대해 설명했습니다. 수행할 작업을 `JobService`로 캡슐화하는 방법과 `FirebaseJobDispatcher`를 사용하여 해당 작업을 예약하고, `JobTrigger`를 사용하여 조건을 지정하고, `RetryStrategy`를 사용하여 오류를 처리하는 방법을 설명했습니다.

## <a name="related-links"></a>관련 링크

- [NuGet의 Xamarin.Firebase.JobDispatcher](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [GitHub의 firebase-job-dispatcher](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher 바인딩](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [인텔리전트 작업 예약](https://developer.android.com/topic/performance/scheduling.html)
- [Android 배터리 및 메모리 최적화 - Google I/O 2016(비디오)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
