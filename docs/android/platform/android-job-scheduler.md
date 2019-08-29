---
title: Android 작업 스케줄러
description: 이 가이드에서는 Android 작업 Scheduler API를 사용 하 여 백그라운드 작업을 예약 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/19/2018
ms.openlocfilehash: 95d4194e0ed1a1da435a233e40a74f506c49b539
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119868"
---
# <a name="android-job-scheduler"></a>Android 작업 스케줄러

_이 가이드에서는 android 5.0 (API 레벨 21) 이상을 실행 하는 Android 장치에서 사용할 수 있는 Android 작업 스케줄러 API를 사용 하 여 백그라운드 작업을 예약 하는 방법을 설명 합니다._


## <a name="overview"></a>개요 

Android 응용 프로그램을 사용자에 게 응답성을 유지 하는 가장 좋은 방법 중 하나는 백그라운드에서 복잡 하거나 오래 실행 되는 작업이 수행 되도록 하는 것입니다. 그러나 백그라운드 작업은 사용자의 장치 환경에 부정적인 영향을 주지 않습니다. 

예를 들어 백그라운드 작업은 3 ~ 4 분 마다 웹 사이트를 폴링하여 특정 데이터 집합에 대 한 변경 내용을 쿼리할 수 있습니다. 심각 하지는 않지만 배터리 수명에 심각한 영향을 미칠 수 있습니다. 응용 프로그램은 장치를 반복적으로 절전 모드에서 해제 하 고, CPU를 더 높은 전원 상태로 상승 하 고, 라디오를 켜고, 네트워크를 요청 하 고, 결과를 처리 합니다. 장치가 즉시 작동 중단 되 고 저전원 유휴 상태로 복귀 되지 않으므로 성능이 저하 됩니다. 잘못 예약 된 백그라운드 작업은 불필요 하 고 과도 한 전원 요구 사항으로 장치를 실수로 유지할 수 있습니다. 이 처럼 보입니다. (웹 사이트 폴링)는 상대적으로 짧은 시간 안에 장치를 사용할 수 없게 렌더링 합니다.

Android는 백그라운드에서 작업을 수행 하는 데 도움이 되는 다음과 같은 Api를 제공 하지만 지능적 작업 예약에는 충분 하지 않습니다. 

- **[의도 서비스](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; 내재 된 서비스는 작업을 수행 하는 데 유용 하지만 작업을 예약 하는 방법은 제공 하지 않습니다.
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; 이러한 api는 작업 예약만 허용 하지만 실제로 작업을 수행할 수 있는 방법은 제공 하지 않습니다. 또한 AlarmManager는 시간 기반 제약 조건만 허용 합니다. 즉, 특정 시간에 또는 특정 기간이 경과한 후에 알람을 발생 시킵니다. 
- **[브로드캐스트 수신기](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Android 앱은 시스템 차원의 이벤트 또는 의도에 대 한 응답으로 작업을 수행 하도록 브로드캐스트 수신기를 설정할 수 있습니다. 그러나 브로드캐스트 수신기는 작업을 실행 해야 하는 경우에 대 한 제어를 제공 하지 않습니다. 또한 Android 운영 체제의 변경 내용으로 인해 브로드캐스트 수신기가 작동 하는 시기 또는 응답할 수 있는 작업 종류가 제한 됩니다. 

백그라운드 작업을 효율적으로 수행 하는 두 가지 주요 기능 ( _백그라운드 작업_ 또는 _작업_이 라고도 함)은 다음과 같습니다.

1. **지능적으로 작업 예약** &ndash; 응용 프로그램이 백그라운드에서 작업을 수행 하는 경우에는 적절 한 시민으로 작업을 수행 하는 것이 중요 합니다. 응용 프로그램에서 작업 실행을 요구 하지 않아야 하는 것이 가장 좋습니다. 대신, 응용 프로그램은 작업을 실행할 수 있는 경우 충족 해야 하는 조건을 지정 하 고 조건이 충족 될 때 작업을 수행할 운영 체제를 사용 하 여 해당 작업을 예약 해야 합니다. 이렇게 하면 Android에서 작업을 실행 하 여 장치에서 효율성을 최대한 유지할 수 있습니다. 예를 들어 네트워킹에 관련 된 오버 헤드를 최대한 활용 하기 위해 네트워크 요청을 동시에 실행 하도록 일괄 처리할 수 있습니다.
2. **작업 캡슐화** &ndash; 백그라운드 작업을 수행 하는 코드는 사용자 인터페이스와 독립적으로 실행 될 수 있는 불연속 구성 요소에 캡슐화 되어야 하며, 어떤 이유로 작업이 완료 되지 않으면 비교적 쉽게 일정을 조정할 수 있습니다.

Android 작업 Scheduler는 흐름 API를 제공 하는 Android 운영 체제에 기본 제공 되는 프레임 워크로 서 백그라운드 작업 예약을 간소화 합니다.  Android 작업 스케줄러는 다음 형식으로 구성 됩니다.

- 는 `Android.App.Job.JobScheduler` Android 응용 프로그램을 대신 하 여 작업을 예약 하 고, 실행 하 고, 필요한 경우 작업을 취소 하는 데 사용 되는 시스템 서비스입니다.
- `Android.App.Job.JobService` 는 응용 프로그램의 주 스레드에서 작업을 실행 하는 논리를 사용 하 여 확장 해야 하는 추상 클래스입니다. 즉,에서 `JobService` 작업을 비동기적으로 수행 하는 방법을 담당 합니다.
- `Android.App.Job.JobInfo` 개체에는 작업을 실행 해야 할 때 Android를 안내 하는 기준이 포함 됩니다.

Android 작업 스케줄러를 사용 하 여 작업을 예약 하려면 xamarin.ios 응용 프로그램에서 `JobService` 클래스를 확장 하는 클래스에 코드를 캡슐화 해야 합니다. `JobService`에는 작업 수명 중에 호출할 수 있는 세 가지 수명 주기 방법이 있습니다.

- **Bool OnStartJob (JobParameters 매개 변수)** 이 메서드는 작업을 수행 `JobScheduler` 하기 위해에서 호출 되며 응용 프로그램의 주 스레드에서 실행 됩니다. &ndash; 에서 `JobService` 작업을 비동기적으로 수행 하 고 `true` , 남은 작업이 있거나 `false` , 작업이 완료 된 경우에는를 수행 해야 합니다.
    
    에서 `JobScheduler` 이 메서드를 호출 하면 작업 기간 동안 Android에서 wakelock을 요청 하 고 유지 합니다. 작업이 완료 되 면 `JobService` `JobFinished` 메서드 (다음에 설명)를 호출 하 여이 `JobScheduler` 팩트를에 알리기 위한 책임이 있습니다.

- **Jobfinished (Jobfinished 매개 변수, Bool needsReschedule)** 에서 작업을 수행 `JobScheduler` 한다는 것을 `JobService` 알리기 위해에서이 메서드를 호출 해야 합니다. &ndash; 가 `JobFinished` 호출 되지 않은 경우는 `JobScheduler` wakelock을 제거 하지 않으므로 불필요 한 배터리 드레이닝이 발생 합니다. 

- **Bool OnStopJob (JobParameters 매개 변수)** &ndash; Android에서 작업을 중간에 중지할 때 호출 됩니다. 다시 시도 조건 `true` 에 따라 작업을 다시 예약 해야 하는 경우를 반환 합니다 (아래에서 자세히 설명).

작업이 실행 될 수 있는 시기를 제어 하는 _제약 조건이_ 나 _트리거_ 를 지정할 수 있습니다. 예를 들어 장치가 요금을 청구 하거나 사진을 만들 때 작업을 시작 하는 경우에만 실행 되도록 작업을 제한할 수 있습니다.

이 가이드에서는 클래스를 `JobService` 구현 하 고를 `JobScheduler`사용 하 여 예약 하는 방법에 대해 자세히 설명 합니다.

## <a name="requirements"></a>요구 사항

Android 작업 스케줄러에는 Android API 레벨 21 (Android 5.0) 이상이 필요 합니다. 

## <a name="using-the-android-job-scheduler"></a>Android 작업 스케줄러 사용

Android JobScheduler API를 사용 하는 세 가지 단계가 있습니다.

1. 작업을 캡슐화 할 JobService 형식을 구현 합니다.
2. 개체를 `JobInfo.Builder` 사용 하 여 작업 `JobInfo` 을 실행 `JobScheduler` 하기 위한 조건을 보유 하는 개체를 만듭니다. 
3. 을 사용 하 여 `JobScheduler.Schedule`작업을 예약 합니다.

### <a name="implement-a-jobservice"></a>JobService 구현

Android 작업 스케줄러 라이브러리에서 수행 하는 모든 작업은 추상 클래스를 `Android.App.Job.JobService` 확장 하는 형식에서 수행 해야 합니다. 를 `JobService` 만드는 것은 Android framework를 사용 `Service` 하 여을 만드는 것과 매우 비슷합니다. 

1. 클래스를 `JobService` 확장 합니다.
2. 를 사용 `ServiceAttribute` 하 여 하위 클래스를 데코 `Name` 레이트 하 고 매개 변수를 패키지 이름 및 클래스 이름으로 구성 된 문자열로 설정 합니다 (다음 예제 참조).
3. `Permission` 의 속성 `ServiceAttribute` 을 문자열로`android.permission.BIND_JOB_SERVICE`설정 합니다.
4. 작업을 수행 하는 코드를 추가 하 여 메서드를재정의합니다.`OnStartJob` Android는 응용 프로그램의 주 스레드에서이 메서드를 호출 하 여 작업을 실행 합니다. 응용 프로그램이 차단 되는 것을 방지 하기 위해 스레드에서 몇 밀리초를 수행 해야 하는 작업은 더 오래 걸릴 수 있습니다.
5. 작업이 완료 되 면에서 `JobService` 메서드를 `JobFinished` 호출 해야 합니다. 이 메서드는이 `JobService` 작업을 `JobScheduler` 수행 하는 방법을 나타냅니다. 를 호출 `JobFinished` 하지 않으면 장치에 불필요 `JobService` 한 요청이 발생 하 여 배터리 수명을 단축할 수 있습니다. 
6. `OnStopJob` 메서드를 재정의 하는 것도 좋습니다. 이 메서드는 작업이 완료 되기 전에 작업이 종료 될 때 Android에서 호출 되며 리소스를 적절 하 게 `JobService` 삭제할 수 있는 기회를 제공 합니다. 작업을 다시 예약 `true` 해야 하는 경우이 메서드는을 반환 하 `false` 고, 작업을 다시 실행 하기 위해 desireable 않으면를 반환 해야 합니다.
   

다음 코드는 TPL을 사용 하 여 비동기적 `JobService` 으로 작업을 수행 하는 응용 프로그램에 대 한 가장 간단한 예제입니다.

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

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>작업을 예약 하는 JobInfo 만들기

Xamarin Android 응용 프로그램은를 `JobService` 직접 인스턴스화하지 않으며 대신 개체 `JobScheduler`를 `JobInfo` 에 전달 합니다. 는 요청 `JobService` 된 개체를 인스턴스화하고의 메타 데이터 `JobInfo`에 `JobService` 따라을 예약 하 고 실행 합니다. `JobScheduler` 개체 `JobInfo` 는 다음 정보를 포함 해야 합니다.

- **JobId** 에서 작업을 식별 하는 데 사용 되는 값입니다.`int` `JobScheduler` &ndash; 이 값을 다시 사용 하면 기존 작업이 업데이트 됩니다. 값은 응용 프로그램에 대해 고유 해야 합니다. 
- **Jobservice** 이 매개 변수 `ComponentName` 는에서 `JobScheduler` 작업을 실행 하는 데 사용 해야 하는 형식을 명시적으로 식별 하는입니다. &ndash; 
  

이 확장 메서드는 활동과 같은 Android `JobInfo.Builder` `Context`를 사용 하 여를 만드는 방법을 보여 줍니다.

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

Android 작업 Scheduler의 강력한 기능은 작업이 실행 되는 시기 또는 작업이 실행 될 수 있는 상황을 제어 하는 기능입니다. 다음 표에서는 앱이 작업을 실행할 수 `JobInfo.Builder` 있는 경우에 영향을 줄 수 있는의 몇 가지 방법에 대해 설명 합니다.  


|  메서드 | Description   |
|---|---|
| `SetMinimumLatency`  | 작업이 실행 되기 전에 관찰 해야 하는 지연 (밀리초)을 지정 합니다. |
| `SetOverridingDeadline`  | 이 시간 (밀리초)이 경과 하기 전에 작업을 실행 해야 하는를 선언 합니다. |
| `SetRequiredNetworkType`  | 작업에 대 한 네트워크 요구 사항을 지정 합니다. |
| `SetRequiresBatteryNotLow` | 장치가 사용자에 게 "배터리 부족" 경고를 표시 하지 않는 경우에만 작업이 실행 될 수 있습니다. |
| `SetRequiresCharging` | 작업은 배터리가 충전 된 경우에만 실행 될 수 있습니다. |
| `SetDeviceIdle` | 장치가 사용 중일 때 작업이 실행 됩니다. |
| `SetPeriodic` | 작업이 정기적으로 실행 되도록 지정 합니다. |
| `SetPersisted` | 작업은 장치 재부팅 간에 perisist 되어야 합니다. | 


는 `SetBackoffCriteria` 에서 작업을 다시 실행 하기 전에 `JobScheduler` 대기 해야 하는 시간에 대 한 몇 가지 지침을 제공 합니다. 백오프 조건에는 지연 시간 (밀리초의 기본값은 30 초)과 사용 해야 하는 백오프 유형 ( _백오프 정책이_ 나 _재시도 정책이_라고도 함)이 있습니다. 두 정책은 `Android.App.Job.BackoffPolicy` 열거형에 캡슐화 됩니다.

- `BackoffPolicy.Exponential`&ndash; 지 수 백오프 정책은 각 실패 후에 초기 백오프 값을 지 수로 늘립니다. 작업이 처음으로 실패 하면 라이브러리는 작업을 다시 예약 하기 전에 지정 된 초기 간격을 대기 합니다 (예: 30 초). 작업이 실패 하는 두 번째 경우 라이브러리는 작업을 실행 하기 전에 최소 60 초 정도 기다립니다. 세 번째 실패 후 라이브러리는 120 초 정도 대기 하 게 됩니다. 이것은 기본값입니다.
- `BackoffPolicy.Linear`&ndash; 이 전략은 작업이 성공할 때까지 설정 된 간격으로 실행 되도록 다시 예약 되어야 하는 선형 백오프. 선형 백오프는 가능한 한 빨리 완료 되어야 하는 작업 또는 자신을 신속 하 게 해결 하는 문제에 가장 적합 합니다. 

`JobInfo` 개체 만들기에 대 한 자세한 내용은 [ `JobInfo.Builder` 클래스에 대 한 Google 설명서](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html)를 참조 하세요.

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>JobInfo를 통해 작업에 매개 변수 전달

매개 변수는 `Job.Builder.SetExtras` 메서드와 함께 전달 되는를 `PersistableBundle` 만들어 작업에 전달 됩니다.

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

는의 `Android.App.Job.JobParameters.Extras` `PersistableBundle` 메서드에서속성`OnStartJob` 을 통해 액세스 됩니다. `JobService`

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>작업 예약

작업을 예약 하기 위해 Xamarin Android 응용 프로그램은 `JobScheduler` 시스템 서비스에 대 한 참조를 가져오고 이전 단계에서 만든 `JobInfo` 개체를 사용 하 여 메서드를 `JobScheduler.Schedule` 호출 합니다. `JobScheduler.Schedule`는 두 정수 값 중 하나를 사용 하 여 즉시 반환 됩니다.

- **Jobscheduler. resultsuccess** &ndash; 작업이 성공적으로 예약 되었습니다. 
- **Jobscheduler. resultfailure** &ndash; 작업을 예약할 수 없습니다. 이 문제는 일반적으로 충돌 `JobInfo` 하는 매개 변수 때문에 발생 합니다.

이 코드는 작업을 예약 하 고 예약 시도의 결과를 사용자에 게 알리는 예제입니다.

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

예약 된 모든 작업을 취소 하거나 `JobsScheduler.CancelAll()` 메서드 `JobScheduler.Cancel(jobId)` 또는 메서드를 사용 하 여 단일 작업만 취소할 수 있습니다.

```csharp
// Cancel all jobs
jobScheduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>요약

이 가이드에서는 Android 작업 스케줄러를 사용 하 여 백그라운드에서 작업을 지능적으로 수행 하는 방법에 대해 설명 했습니다. 에서 수행 `JobService` 되는 작업을 캡슐화 하는 방법과를 `JobScheduler` 사용 하 여 해당 작업을 예약 하는 방법을 설명 하 고를 사용 하 `JobTrigger` 여 조건을 지정 하 `RetryStrategy`고 오류를로 처리 하는 방법을 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [인텔리전트 작업-예약](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler API 참조](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [JobScheduler를 사용 하 여 pro와 같은 작업 예약](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android 배터리 및 메모리 최적화-Google i/o 2016 (비디오)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler-이력서 Ruppert](https://www.youtube.com/watch?v=aSjBBPYjelE)
