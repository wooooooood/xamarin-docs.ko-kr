---
title: Android 작업 스케줄러
description: 이 가이드에서는 Android 작업 스케줄러 API를 사용 하 여 백그라운드 작업을 예약 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: dc72b7e4da330185b00541f923d9c4b64b91bc95
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2018
ms.locfileid: "30005689"
---
# <a name="android-job-scheduler"></a>Android 작업 스케줄러

_이 가이드에서는 Android 5.0 (API 수준 21)를 실행 하는 Android 장치에서 사용할 수 있는 Android 작업 스케줄러 API를 사용 하 여 백그라운드 작업을 예약 하는 방법을 이상._


## <a name="overview"></a>개요 

Android 응용 프로그램을 사용자에 응답성을 유지 하는 가장 좋은 방법 중 하나는 복잡 하거나 장기 실행 작업은 백그라운드에서 수행 되도록 하려면입니다. 그러나이 백그라운드 작업 부정적인 영향을 미치지 것입니다는 사용자의 경험 장치에서 중요 합니다. 

예를 들어 백그라운드 작업 웹 사이트 서너 개의 분 마다 쿼리에 특정 데이터 집합에 대 한 변경에 대 한 폴링할 수 있습니다. 이 방법은 치명적인 영향 배터리 수명에 동일 하 게 되었지만 심각 하지 않은 합니다. 응용 프로그램은 반복 해 서에서 장치를 깨우는, CPU 전원 상태를 더 높은 수준으로 상승, 라디오 전원을, 네트워크 요청을 하 고 결과 처리 합니다. 장치에서 즉시는 아닙니다 전원을 끄고 한 저전원 유휴 상태를 반환 하므로 더 심각한 가져옵니다. 실수로 잘못 예약 된 백그라운드 작업 불필요 한 과도 한 전원 요구를 사용 하 여 상태의 장치를 유지할 수도 있습니다. 이 겉보기 문제 없는 작업 (웹 사이트를 폴링하여) 사용 장치 비교적 짧은 기간 동안 사용할 수 없게 렌더링 합니다.

Android 백그라운드에서 작업 수행에 도움이 되도록 다음 Api를 제공 하지만 자체적으로 충분 하지 않습니다 지능형 작업 일정에 대 한 합니다. 

* **[하지만 의도 서비스](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; 의도 서비스는 작업을 예약할 수 없으므로 제공 작업을 수행 하기 위한 훌륭한 합니다.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; 이러한 Api 작업을 예약할 수 있지만 실제로 작업을 수행할 수 없으므로 제공 하도록 허용 합니다. 또한는 AlarmManager 허용 시간 기반 제약 조건, 즉, 특정 시간이 나 특정 기간을 경과한 후 경보 발생 합니다. 
* **[브로드캐스트 수신기](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; 는 Android 응용 프로그램 의도 또는 시스템 수준 이벤트에 대 한 응답에서 작업을 수행 하는 브로드캐스트 수신기를 설정할 수 있습니다. 그러나 브로드캐스트 수신기는 작업을 실행 해야 하는 경우에 대 한 제어를 제공 하지 않습니다. Android 운영 체제의 변경 내용에서 제한도 브로드캐스트 수신기는 작동 하는 경우 않거나에 응답할 수 있는 작업 유형입니다. 

효율적으로 백그라운드 작업을 수행 하는 데 두 가지 주요 기능이 (라고도 _백그라운드 작업_ 또는 _작업_):

1. **작업을 예약 하는 지능적으로** &ndash; 것이 중요 응용 프로그램은 이렇게 하는 동안 백그라운드에서 작업 한다는 것으로 제대로 작동 합니다. 이상적으로 응용 프로그램 작업을 실행할 수 요청 해서는 안 됩니다. 대신, 응용 프로그램 작업에서 수를 실행 하 고 다음 조건이 충족 되는 때 작업에서 수행 되는 운영 체제와 해당 작업을 예약할 때 위해 충족 해야 하는 조건을 지정 해야 합니다. 이렇게 하면 Android를 장치에서 최대 효율을 위해 작업을 실행할 수 있습니다. 예를 들어 네트워킹와 관련 된 오버 헤드를 최대한 사용할 수 있도록 한 번에 모두 실행 하려면 네트워크 요청을 일괄 처리 수 있습니다.
2. **작업을 캡슐화** &ndash; 백그라운드 작업을 수행 하는 코드는 사용자 인터페이스와 독립적으로 실행할 수 있으며 비교적 쉬운 작업 완료 되지 않으면 다시 예약 하는 개별 구성 요소에서 캡슐화 해야 어떤 이유로 합니다.

Android 작업 스케줄러는 일정 백그라운드 작업을 간소화 하기 위해 fluent API를 제공 하는 Android 운영 체제에 기본 제공 합니다.  Android 작업 스케줄러는 다음 형식의 구성 됩니다.

* `Android.App.Job.JobScheduler` 예약, 실행 및 필요한 경우, 작업 취소를 대신 하 여 Android 응용 프로그램에 사용 되는 시스템 서비스입니다.
* `Android.App.Job.JobService` 응용 프로그램의 주 스레드에서 작업을 실행 하는 논리로 확장 해야 하는 추상 클래스입니다. 즉는 `JobService` 어떻게 비동기적으로 수행 되는 작업을 담당 합니다.
* `Android.App.Job.JobInfo` 경우 작업이 실행 되는 경우 Android를 안내 하는 기준 개체에 포함 되어 있습니다.

Android 작업 스케줄러와 작업을 예약 하려면 Xamarin.Android 응용 프로그램을 확장 하는 클래스의 코드를 캡슐화 해야는 `JobService` 클래스입니다. `JobService` 수명 주기 메서드가 세 개는 작업의 수명 동안 호출할 수 있습니다.

* **bool (JobParameters 매개 변수) OnStartJob** &ndash; 이 메서드는 `JobScheduler` 작업 및 응용 프로그램의 주 스레드에서 실행을 수행 합니다. 책임은 `JobService` 비동기적으로 작업을 수행 하 고 `true` 있는 경우 올바르게 남은 작업, 동작 또는 `false` 는 작업을 수행 하는 경우.
    
    경우는 `JobScheduler` 이 메서드를 호출 요청 하 고 작업의 기간에 대 한 Android에서 wakelock 유지 합니다. 작업은 작업이 완료 되 면는 `JobService` 구별 하는 `JobScheduler` 호출 하 여 이러한 점에 `JobFinished` 메서드 (다음에 설명).

* **(JobParameters 매개 변수, bool needsReschedule) JobFinished** &ndash; 에서이 메서드를 호출 해야는 `JobService` 구별 하는 `JobScheduler` 는 작업을 수행 하는 합니다. 경우 `JobFinished` 를 호출 하지 않으면는 `JobScheduler` 불필요 한 배터리 소모를 발생 시키는 wakelock 제거 되지 것입니다. 

* **bool (JobParameters 매개 변수) OnStopJob** &ndash; 이 Android에서 해당 작업을 중지 중간 때 호출 됩니다. 반환 해야 `true` 경우 작업은 일정을 조정할 수 (자세히 아래 설명 참조)은 다시 시도 조건에 따라 합니다.

지정할 수는 _제약 조건_ 또는 _트리거_ 작업의 수 또는 실행 해야 시기는 제어할 됩니다입니다. 예를 들어 하 여 장치를 충전 못했거나 그림 때 작업을 시작 하는 경우에 실행 되는 작업을 제한 하는 것이 같습니다.

이 가이드를 구현 하는 방법을 자세히 설명 합니다는 `JobService` 클래스 하 고 사용 하 여 예약 된 `JobScheduler`합니다.

## <a name="requirements"></a>요구 사항

작업 스케줄러의 Android는 Android API 수준 21 (Android 5.0) 필요 이상. 

## <a name="using-the-android-job-scheduler"></a>Android 작업 스케줄러를 사용 하 여

Android JobScheduler API를 사용 하기 위한 세 가지 단계가 있습니다.

1. 작업을 캡슐화 하는 JobService 형식을 구현 합니다.
2. 사용 하 여 한 `JobInfo.Builder` 만들 개체는 `JobInfo` 개체에 대 한 조건을 포함 하는 `JobScheduler` 작업을 실행 하 합니다. 
3. 사용 하 여 작업 예약 `JobScheduler.Schedule`합니다.

### <a name="implement-a-jobservice"></a>JobService 구현

확장 하는 형식에서 Android 작업 스케줄러 라이브러리에서 수행 된 모든 작업을 수행 해야는 `Android.App.Job.JobService` 추상 클래스입니다. 만들기는 `JobService` 만드는 것과 매우 비슷합니다는 `Service` Android 프레임 워크: 

1. 확장 된 `JobService` 클래스입니다.
2. 와 하위 클래스를 데코레이팅하는 `ServiceAttribute` 설정 하 고는 `Name` 패키지 이름과 클래스의 이름으로 구성 된 문자열에 매개 변수 (다음 예제 참조).
3. 설정의 `Permission` 속성에는 `ServiceAttribute` 문자열에 `android.permission.BIND_JOB_SERVICE`합니다.
4. 재정의 `OnStartJob` 메서드를 사용 하는 작업을 수행 하는 코드를 추가 합니다. Android 작업을 실행 하는 응용 프로그램의 주 스레드에서이 메서드를 호출 합니다. 작업은 몇 밀리초 응용 프로그램을 차단 하지 않기 위해 스레드에서 수행 해야 함을 시간이 오래 걸립니다.
5. 작업이 완료 되는 경우는 `JobService` 호출 해야 합니다는 `JobFinished` 메서드. 이 메서드는 어떻게 `JobService` 지시는 `JobScheduler` 작업을 수행 합니다. 호출 하지 못하면 `JobFinished` 됩니다는 `JobService` 불필요 한 요청에서 배터리 수명을 단축 장치에 배치 하는 합니다. 
6. 또한 재정의 하는 것이 좋습니다는 `OnStopJob` 메서드. 이 메서드는 Android에서 작업이 종료 됩니다. 그가 완료 되 고 제공 하기 전에 `JobService` 모든 리소스를 제대로 처리할 수 있도록 합니다. 이 메서드를 반환 하도록 `true` 는 작업을 다시 예약 해야 하는 경우 또는 `false` 작업을 다시 실행 desireable 없는 경우.
   

다음 코드는 가장 간단한 예제 `JobService` 응용 프로그램에 대 한 몇 가지 작업을 비동기적으로 수행 하는 TPL을 사용 합니다.

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

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>작업을 예약 하려면 JobInfo 만들기

Xamarin.Android 응용 프로그램 인스턴스화할 수 없습니다는 `JobService` 를 직접 대신 됩니다 전달는 `JobInfo` 개체는 `JobScheduler`합니다. `JobScheduler` 요청 된 인스턴스화 `JobService` 일정 예약 및 실행 개체는 `JobService` 의 메타 데이터에 따라는 `JobInfo`합니다. A `JobInfo` 개체에는 다음 정보가 포함 되어야 합니다.

* **JobId** &ndash; 이것이 `int` 작업을 식별 하는 데 사용 되는 값은 `JobScheduler`합니다. 이 값을 다시 사용 하면 모든 기존 작업 업데이트 됩니다. 값은 응용 프로그램에 대해 고유 해야 합니다. 
* **JobService** &ndash; 이 매개 변수는 한 `ComponentName` 형식을 명시적으로 식별 하는 `JobScheduler` 작업을 실행 하는 데 사용 해야 합니다. 
  

이 확장 메서드를 만드는 방법을 보여는 `JobInfo.Builder` 는 Android와 `Context`, 작업 등:

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

// Sample usage - creates a JobBuilder for a DownloadJob andsets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creats a JobInfo object.
```

Android 작업 스케줄러의 강력한 기능은 작업이 실행 되는 경우 또는 실행 될 수 있습니다는 조건 작업을 제어할 수 있다는 점입니다. 다음 표에서 메서드 중 일부에 대해 설명 `JobInfo.Builder` 응용 프로그램에 작업을 실행할 때 영향을 줄 수 있는:  


|  메서드 | 설명   |
|---|---|
| `SetMinimumLatency`  | 작업 보다 먼저 관찰 해야 하는 지연 (밀리초)이 실행 되도록 지정 합니다. |
| `SetOverridingDeadline`  | 작업을 선언 하는이 시간 (밀리초)이 경과 하기 전에 실행 해야 합니다. |
| `SetRequiredNetworkType`  | 작업에 대 한 네트워크 요구 사항을 지정합니다. |
| `SetRequiresBatteryNotLow` | 장치는 사용자에 게 "배터리 부족" 경고를 표시 하지 않을 경우에 작업이 실행 될 수 있습니다. |
| `SetRequiresCharging` | 배터리를 충전 하는 경우에 작업이 실행 될 수 있습니다. |
| `SetDeviceIdle` | 장치가 많을 경우 작업이 실행 됩니다. |
| `SetPeriodic` | 작업이 정기적으로 실행 해야 함을 지정 합니다. |
| `SetPersisted` | 장치 다시 부팅 해도 perisist을 해야 하는 작업입니다. | 


`SetBackoffCriteria` 방법에는 몇 가지 지침을 제공 합니다. 시간 `JobScheduler` 작업을 다시 실행 하려고 하기 전에 대기 해야 합니다. 백오프 조건에는 두 가지: 사용 해야 하는 시간 (밀리초) (기본값 30 초) 및 백오프의 형식 지연 (라고도 _백오프 정책을_ 또는 _다시 시도 정책_) . 두 개의 정책에 캡슐화 되는 `Android.App.Job.BackoffPolicy` enum:

* `BackoffPolicy.Exponential` &ndash; 지 수 백오프 정책을 각 오류가 발생 한 후 초기 백오프 값을 기하급수적으로 증가 합니다. 작업이 실패 하는, 처음으로 라이브러리는 작업 – 예에서는 30 초 동안 다시 하기 전에 지정 된 초기 간격을 대기 합니다. 작업이 실패 하면 두 번째로 라이브러리는 60 초 이상 작업을 실행 하는 동안 대기 합니다. 세 번째 시도가 실패 한 라이브러리는 120 초, 대기 등에입니다. 기본값입니다.
* `BackoffPolicy.Linear` &ndash; 이 전략은 작업을 다시 예약 해야 하는 선형 백오프 (성공할 때까지 것) 간격으로 실행 합니다. 선형 백오프는 가능한 한 빨리 완료 해야 하는 작업에 대 한 또는 자체를 신속 하 게 해결할 수 있는 문제에 가장 적합 합니다. 

대 한 자세한 내용은 만듭니다는 `JobInfo` 개체를 읽어 보십시오 [에 대 한 Google의 설명서는 `JobInfo.Builder` 클래스](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html)합니다.

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>JobInfo 통해 작업에 매개 변수 전달

매개 변수를 만들어 작업에 전달 되는 `PersistableBundle` 와 함께 전달 되는 `Job.Builder.SetExtras` 메서드:

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

`PersistableBundle` 에서 액세스 되는 `Android.App.Job.JobParameters.Extras` 속성에는 `OnStartJob` 의 메서드는 `JobService`:

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>작업 예약

작업을 예약 하려면 Xamarin.Android 응용 프로그램에 대 한 참조 받습니다는 `JobScheduler` 시스템 서비스를 호출은 `JobScheduler.Schedule` 사용 하 여 메서드는 `JobInfo` 이전 단계에서 만든 개체입니다. `JobScheduler.Schedule` 두 정수 값 중 하나가 지정 된 즉시 반환 합니다.

* **JobScheduler.ResultSuccess** &ndash; 작업이 성공적으로 예약 되었습니다. 
* **JobScheduler.ResultFailure** &ndash; 작업을 예약할 수 없습니다. 일반적으로는 충돌 하는 `JobInfo` 매개 변수입니다.

이 코드는 작업을 예약 하 고 일정 시도의 결과의 사용자에 게 알리지의 예입니다.

```csharp
var jobScheduler = (JobScheduler)GetSystemService(JobSchedulerService);
var scheduleResult = jobScheduler.Schedule(jobInfo);

if (JobScheduler.ResultSuccess == scheduleResult)
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_success, Snackbar.LengthShort);
}
else
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_failure, Snackbar.LengthShort);
}
```
 
### <a name="cancelling-a-job"></a>작업 취소

예정 된 모든 작업을 취소할 수 인지만 사용 하 여 단일 작업에서 `JobsScheduler.CancelAll()` 메서드 또는 `JobScheduler.Cancel(jobId)` 메서드:

```csharp
// Cancel all jobs
jobSchduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>요약

이 가이드는 지능적으로 백그라운드에서 작업을 수행 하려면 Android 작업 스케줄러를 사용 하는 방법을 설명 합니다. 로 수행할 작업을 캡슐화 하는 방법을 설명 하는 `JobService` 방법과 사용 하는 `JobScheduler` 와 조건을 지정 하는 작업을 예약할는 `JobTrigger` 와 오류를 처리 하는 방법 및는 `RetryStrategy`합니다.

## <a name="related-links"></a>관련 링크

- [지능형 작업 예약](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler API 참조](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [JobScheduler 전문가가 같은 작업을 예약합니다.](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android 배터리 및 메모리 최적화-Google I/O 2016 (비디오)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler-René Ruppert-Xamarin 대학](https://www.youtube.com/watch?v=aSjBBPYjelE)