---
title: Android 작업 스케줄러
description: 이 가이드에서는 Android 작업 스케줄러 API를 사용하여 백그라운드 작업을 예약하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/19/2018
ms.openlocfilehash: 10d2ae6ac35f02d75ef6e04a0531ec3f5dafd668
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "76940815"
---
# <a name="android-job-scheduler"></a>Android 작업 스케줄러

_이 가이드에서는 Android 5.0(API 수준 21) 이상을 실행하는 Android 디바이스에서 사용할 수 있는 Android 작업 스케줄러 API를 사용하여 백그라운드 작업을 예약하는 방법을 설명합니다._

## <a name="overview"></a>개요 

Android 애플리케이션에서 사용자 응답성을 유지하는 가장 좋은 방법 중 하나는 복잡하거나 장기 실행되는 작업이 백그라운드에서 실행되도록 하는 것입니다. 그러나 백그라운드 작업이 사용자의 디바이스 환경에 부정적인 영향을 주지 않는 것이 중요합니다. 

예를 들어 백그라운드 작업은 3~4분마다 웹 사이트를 폴링하여 특정 데이터 세트에 대한 변경 내용을 쿼리할 수 있습니다. 무해한 것처럼 보이지만 배터리 사용 시간에 치명적인 영향을 미칠 수 있습니다. 애플리케이션은 반복적으로 디바이스를 절전 모드에서 해제하고, CPU를 더 높은 전원 상태로 상승하고, 무선을 켜고, 네트워크를 요청하고, 결과를 처리합니다. 디바이스가 즉시 작동 중단되고 저전원 유휴 상태로 복귀하지 않으므로 상황은 더 심각해집니다. 잘못 예약된 백그라운드 작업은 디바이스에서 불필요하고 과도한 전원 요구 사항을 지속할 수 있습니다. 이처럼 겉으로는 무해한 작업(웹 사이트 폴링)이 비교적 짧은 시간에 디바이스를 불안정하게 만듭니다.

Android는 다음과 같이 백그라운드에서 작업을 수행하는 데 도움이 되는 API를 제공하지만 인텔리전트 작업 예약에는 충분하지 않습니다. 

- **[의도 서비스](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; 의도 서비스는 작업을 수행하는 데 유용하지만 작업을 예약하는 방법은 제공하지 않습니다.
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; 이러한 API는 작업 예약만 허용하고 실제로 작업을 수행할 수 있는 방법은 제공하지 않습니다. 또한 AlarmManager는 시간 기반 제약 조건만 허용합니다. 즉, 특정 시간에 또는 특정 기간이 경과한 후에 알람을 발생시킵니다. 
- **[브로드캐스트 수신기](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Android 앱은 시스템 차원의 이벤트 또는 의도에 대한 응답으로 작업을 수행하도록 브로드캐스트 수신기를 설정할 수 있습니다. 그러나 브로드캐스트 수신기는 작업을 실행해야 하는 경우에 대한 제어를 제공하지 않습니다. 또한 Android 운영 체제의 변경 내용으로 인해 브로드캐스트 수신기가 작동하는 시기 또는 응답할 수 있는 작업 종류가 제한됩니다. 

백그라운드 작업(_백그라운드 작업_ 또는 _작업_이라고도 함)을 효율적으로 수행하기 위한 두 가지 주요 기능이 있습니다.

1. **지능적으로 작업 예약** &ndash; 애플리케이션이 백그라운드에서 작업을 수행하는 경우 협조적으로 작업을 수행하는 것이 중요합니다. 이상적으로는 애플리케이션이 작업 실행을 요구하지 않는 것이 가장 좋습니다. 대신, 애플리케이션은 작업을 실행할 수 있는 경우 충족해야 하는 조건을 지정한 다음, 조건이 충족될 때 작업을 수행할 운영 체제를 사용하여 해당 작업을 예약해야 합니다. 이렇게 하면 Android가 작업을 실행하여 디바이스에서 효율성을 최대한 유지할 수 있습니다. 예를 들어 네트워킹에 관련된 오버헤드를 최대한 활용하기 위해 네트워크 요청을 동시에 실행하도록 일괄 처리할 수 있습니다.
2. **작업을 캡슐화** &ndash; 백그라운드 작업을 수행하는 코드는 사용자 인터페이스와 독립적으로 실행될 수 있는 개별 구성 요소에 캡슐화해야 하며 어떤 이유로 작업이 완료되지 않을 경우에는 비교적 쉽게 일정을 조정할 수 있습니다.

Android 작업 스케줄러는 흐름 API를 제공하는 Android 운영 체제에 기본 제공되는 프레임워크로 서 백그라운드 작업 예약을 간소화합니다.  Android 작업 스케줄러는 다음 형식으로 구성됩니다.

- `Android.App.Job.JobScheduler`는 Android 애플리케이션을 대신하여 작업을 예약하고, 실행하고, 필요한 경우 작업을 취소하는 데 사용되는 시스템 서비스입니다.
- `Android.App.Job.JobService`는 애플리케이션의 주 스레드에서 작업을 실행하는 논리를 사용하여 확장해야 하는 추상 클래스입니다. 즉, `JobService`는 작업을 비동기적으로 수행하는 방법을 담당합니다.
- `Android.App.Job.JobInfo` 개체에는 Android가 작업을 실행해야 하는 조건이 보관됩니다.

Android 작업 스케줄러를 사용하여 작업을 예약하려면 Xamarin.Android 애플리케이션에서 `JobService` 클래스를 확장하는 클래스에 코드를 캡슐화해야 합니다. `JobService`에는 작업 수명 주기 중에 호출할 수 있는 세 가지 수명 주기 메서드가 있습니다.

- **bool OnStartJob(JobParameters parameters)** &ndash; 이 메서드는 작업을 수행하기 위해 `JobScheduler`에서 호출되고 애플리케이션의 주 스레드에서 실행됩니다. 작업을 비동기적으로 수행하고 작업이 남은 경우 `true`를 반환하고 작업이 완료된 경우 `false`를 반환하는 것은 `JobService`의 책임입니다.
    
    `JobScheduler`에서 이 메서드를 호출하는 경우 작업 기간 동안 Android에서 wakelock을 요청하고 유지합니다. 작업이 완료되면 `JobFinished` 메서드(다음에 설명)를 호출하여 이 사실을 `JobScheduler`에 알리는 것은 `JobService`의 책임입니다.

- **JobFinished(JobParameters parameters, bool needsReschedule)** &ndash; 이 메서드는 `JobScheduler`에 작업 완료를 알리기 위해 `JobService`에서 호출해야 합니다. `JobFinished`를 호출하지 않으면 `JobScheduler`가 wakelock을 제거하지 않으므로 불필요하게 배터리가 소모됩니다. 

- **bool OnStopJob(JobParameters parameters)** &ndash; 이 메서드는 작업이 도중에 중단된 경우 Android에서 호출합니다. 재시도 조건에 따라 작업을 다시 예약해야 하는 경우에는 `true`를 반환해야 합니다(아래에서 자세히 설명).

작업을 실행할 수 있는 경우 또는 실행해야 하는 경우를 제어하는 _제약 조건_ 또는 _트리거_를 지정할 수 있습니다. 예를 들어 디바이스가 충전 중일 때만 실행되거나 사진을 촬영할 때 시작하도록 작업을 제한할 수 있습니다.

이 가이드에서는 `JobService` 클래스를 구현하고 `JobScheduler`를 사용하여 예약하는 방법에 대해 자세히 설명합니다.

## <a name="requirements"></a>요구 사항

Android 작업 스케줄러에는 Android API 수준 21(Android 5.0) 이상이 필요합니다. 

## <a name="using-the-android-job-scheduler"></a>Android 작업 스케줄러 사용

Android JobScheduler API를 사용하기 위한 세 단계가 있습니다.

1. 작업을 캡슐화할 JobService 형식을 구현합니다.
2. `JobInfo.Builder` 개체를 사용하여 `JobScheduler`가 작업을 실행할 조건을 보관하는 `JobInfo` 개체를 만듭니다. 
3. `JobScheduler.Schedule`를 사용하여 작업을 예약합니다.

### <a name="implement-a-jobservice"></a>JobService 구현

Android 작업 스케줄러 라이브러리에서 수행하는 모든 작업은 `Android.App.Job.JobService` 추상 클래스를 확장하는 형식에서 수행해야 합니다. `JobService` 만들기는 Android 프레임워크를 사용하여 `Service`를 만드는 것과 매우 비슷합니다. 

1. `JobService` 클래스를 확장합니다.
2. `ServiceAttribute`를 사용하여 서브클래스를 장식하고 `Name` 매개 변수를 패키지 이름 및 클래스 이름으로 구성된 문자열로 설정합니다(다음 예제 참조).
3. `ServiceAttribute`의 `Permission` 속성을 문자열 `android.permission.BIND_JOB_SERVICE`로 설정합니다.
4. `OnStartJob` 메서드를 재정의하여 작업을 수행할 코드를 추가합니다. Android는 애플리케이션의 주 스레드에서 이 메서드를 호출하여 작업을 실행합니다. 애플리케이션이 차단되는 것을 방지하기 위해 몇 밀리초 이상 걸리는 작업은 단일 스레드에서 수행해야 합니다.
5. 작업이 완료되면 `JobService`가 `JobFinished` 메서드를 호출해야 합니다. 이 메서드는 `JobService`가 `JobScheduler`에 작업 완료를 알리는 방법입니다. `JobFinished`를 호출하지 않으면 `JobService`가 디바이스에 불필요한 요구를 부과하여 배터리 사용 시간이 단축됩니다. 
6. `OnStopJob` 메서드를 재정의하는 것도 좋습니다. 이 메서드는 작업이 완료 전에 중단된 경우 Android에서 호출하며, `JobService`에게 리소스를 적절히 삭제할 수 있는 기회를 제공합니다. 작업을 다시 예약해야 하는 경우 이 메서드는 `true`를 반환하고, 작업을 다시 실행하는 것이 바람직하지 않은 경우 `false`를 반환합니다.

다음 코드는 TPL을 사용하여 일부 작업을 비동기적으로 수행하는 애플리케이션에 대한 가장 간단한 `JobService` 예제입니다.

```csharp
[Service(Name = "com.xamarin.samples.downloadscheduler.DownloadJob", 
         Permission = "android.permission.BIND_JOB_SERVICE")]
public class DownloadJob : JobService
{
    public override bool OnStartJob(JobParameters jobParams)
    {            
        Task.Run(() =>
        {
            // Work is happening asynchronously
                      
            // Have to tell the JobScheduler the work is done. 
            JobFinished(jobParams, false);
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(JobParameters jobParams)
    {
        // we don't want to reschedule the job if it is stopped or cancelled.
        return false; 
    }
}
```

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>작업을 예약하는 JobInfo 만들기

Xamarin.Android 애플리케이션은 `JobService`를 직접 인스턴스화하지 않는 대신 `JobScheduler`에 `JobInfo` 개체를 전달합니다. `JobScheduler`는 요청된 `JobService` 개체를 인스턴스화하고 `JobInfo`의 메타데이터에 따라 `JobService`를 예약 및 실행합니다. `JobInfo` 개체는 다음 정보를 포함해야 합니다.

- **JobId** &ndash; `JobScheduler`에 대한 작업을 식별하는 데 사용되는 `int` 값입니다. 이 값을 다시 사용하면 기존 작업이 업데이트됩니다. 값은 애플리케이션에 대해 고유해야 합니다. 
- **JobService** &ndash; 이 매개 변수는 `JobScheduler`가 작업을 실행하는 데 사용해야 하는 형식을 명시적으로 식별하는 `ComponentName`입니다. 

이 확장 메서드는 작업과 같은 Android `Context`를 사용하여 `JobInfo.Builder`을 만드는 방법을 보여 줍니다.

```csharp
public static class JobSchedulerHelpers
{
    public static JobInfo.Builder CreateJobBuilderUsingJobId<T>(this Context context, int jobId) where T:JobService
    {
        var javaClass = Java.Lang.Class.FromType(typeof(T));
        var componentName = new ComponentName(context, javaClass);
        return new JobInfo.Builder(jobId, componentName);
    }
}

// Sample usage - creates a JobBuilder for a DownloadJob and sets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creates a JobInfo object.
```

Android 작업 스케줄러의 강력한 특징 하나는 작업이 실행되는 시기 또는 작업이 실행될 수 있는 조건을 제어하는 기능입니다. 다음 표에서는 `JobInfo.Builder`에서 앱이 작업을 실행할 수 있는 시기에 영향을 줄 수 있도록 하는 몇 가지 방법을 설명합니다.  

|  메서드 | 설명   |
|---|---|
| `SetMinimumLatency`  | 작업이 실행되기 전에 관찰해야 하는 지연 시간(밀리초)을 지정합니다. |
| `SetOverridingDeadline`  | 이 시간(밀리초)이 경과하기 전에 작업을 실행해야 함을 선언합니다. |
| `SetRequiredNetworkType`  | 작업에 대한 네트워크 요구 사항을 지정합니다. |
| `SetRequiresBatteryNotLow` | 디바이스가 사용자에게 "배터리 부족" 경고를 표시하지 않는 경우에만 작업이 실행될 수 있습니다. |
| `SetRequiresCharging` | 작업이 배터리가 충전 중인 경우에만 실행될 수 있습니다. |
| `SetDeviceIdle` | 작업이 디바이스가 사용 중일 때 실행됩니다. |
| `SetPeriodic` | 작업이 정기적으로 실행되도록 지정합니다. |
| `SetPersisted` | 작업이 디바이스 재부팅 간에 지속되어야 합니다. | 

`SetBackoffCriteria`는 작업을 다시 실행하기 전에 `JobScheduler`가 대기해야 하는 시간에 대한 몇 가지 지침을 제공합니다. 백오프 기준에는 사용할 지연 시간(밀리초 단위, 기본값 30초) 및 백오프 유형(_백오프 정책_ 또는 _재시도 정책_이라고도 함)의 두 부분이 있습니다. 두 정책은 `Android.App.Job.BackoffPolicy` 열거형에 캡슐화됩니다.

- `BackoffPolicy.Exponential` &ndash; 지수 백오프 정책은 각 실패 후 초기 백오프 값을 기하급수적으로 늘립니다. 작업이 처음 실패하면 라이브러리는 작업을 다시 예약하기 전에 지정된 초기 간격(예: 30초)을 대기합니다. 작업이 두 번째 실패하면 라이브러리는 작업을 실행하기 전에 최소 60초 대기합니다. 세 번째 실패 후에는 라이브러리가 120초 대기하는 식으로 늘어납니다. 기본값입니다.
- `BackoffPolicy.Linear` &ndash; 이 전략은 (작업이 성공할 때까지) 설정된 간격으로 실행되도록 작업을 다시 예약해야 하는 선형 백오프입니다. 선형 백오프는 가능한 한 빨리 완료되어야 하는 작업 또는 스스로 신속하게 해결하는 문제에 가장 적합합니다. 

`JobInfo` 개체 만들기에 대한 자세한 내용은 [`JobInfo.Builder` 클래스에 대한 Google 설명서](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html)를 참조하세요.

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>JobInfo를 통해 작업에 매개 변수 전달

매개 변수는 `Job.Builder.SetExtras` 메서드와 함께 전달되는 `PersistableBundle`을 만들어 작업에 전달됩니다.

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

`PersistableBundle`은 `JobService`의 `OnStartJob` 메서드에서 `Android.App.Job.JobParameters.Extras` 속성을 통해 액세스할 수 있습니다.

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>작업 예약

작업을 예약하기 위해 Xamarin.Android 애플리케이션은 `JobScheduler` 시스템 서비스에 대한 참조를 가져온 후 이전 단계에서 만든 `JobInfo` 개체를 사용하여 `JobScheduler.Schedule` 메서드를 호출합니다. `JobScheduler.Schedule`은 두 정수 값 중 하나를 사용하여 즉시 반환됩니다.

- **JobScheduler.ResultSuccess** &ndash; 작업을 성공적으로 예약했습니다. 
- **JobScheduler.ResultFailure** &ndash; 작업을 예약하지 못했습니다. 이 오류는 일반적으로 `JobInfo` 매개 변수가 충돌하기 때문에 발생합니다.

이 코드는 작업을 예약하고 예약 시도의 결과를 사용자에게 알리는 예제입니다.

```csharp
var jobScheduler = (JobScheduler)GetSystemService(JobSchedulerService);
var scheduleResult = jobScheduler.Schedule(jobInfo);

if (JobScheduler.ResultSuccess == scheduleResult)
{
    var snackBar = Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_success, Snackbar.LengthShort);
    snackBar.Show();
}
else
{
    var snackBar = Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_failure, Snackbar.LengthShort);
    snackBar.Show();
}
```

### <a name="cancelling-a-job"></a>작업 취소

`JobsScheduler.CancelAll()` 메서드 또는 `JobScheduler.Cancel(jobId)` 메서드를 사용하여 예약된 모든 작업을 취소하거나 단일 작업만 취소할 수 있습니다.

```csharp
// Cancel all jobs
jobScheduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>요약

이 가이드에서는 Android 작업 스케줄러를 사용하여 백그라운드에서 작업을 지능적으로 수행하는 방법에 대해 설명했습니다. 수행할 작업을 `JobService`로 캡슐화하는 방법과 `JobScheduler`를 사용하여 해당 작업을 예약하고, `JobTrigger`를 사용하여 조건을 지정하고, `RetryStrategy`를 사용하여 오류를 처리하는 방법을 설명했습니다.

## <a name="related-links"></a>관련 링크

- [지능형 작업-예약](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler API 참조](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [JobScheduler를 사용하여 프로처럼 작업 예약](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android 배터리 및 메모리 최적화 - Google I/O 2016(비디오)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler - René Ruppert](https://www.youtube.com/watch?v=aSjBBPYjelE)
