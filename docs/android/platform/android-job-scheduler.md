---
title: Android 작업 스케줄러
description: 이 가이드에서는 Android 작업 Scheduler API를 사용 하 여 백그라운드 작업을 예약 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/19/2018
ms.openlocfilehash: 10d2ae6ac35f02d75ef6e04a0531ec3f5dafd668
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940815"
---
# <a name="android-job-scheduler"></a>Android 작업 스케줄러

_이 가이드에서는 android 5.0 (API 레벨 21) 이상을 실행 하는 Android 장치에서 사용할 수 있는 Android 작업 스케줄러 API를 사용 하 여 백그라운드 작업을 예약 하는 방법을 설명 합니다._

## <a name="overview"></a>개요 

Android 응용 프로그램을 사용자에 게 응답성을 유지 하는 가장 좋은 방법 중 하나는 백그라운드에서 복잡 하거나 오래 실행 되는 작업이 수행 되도록 하는 것입니다. 그러나 백그라운드 작업은 사용자의 장치 환경에 부정적인 영향을 주지 않습니다. 

예를 들어 백그라운드 작업은 3 ~ 4 분 마다 웹 사이트를 폴링하여 특정 데이터 집합에 대 한 변경 내용을 쿼리할 수 있습니다. 심각 하지는 않지만 배터리 수명에 심각한 영향을 미칠 수 있습니다. 응용 프로그램은 장치를 반복적으로 절전 모드에서 해제 하 고, CPU를 더 높은 전원 상태로 상승 하 고, 라디오를 켜고, 네트워크를 요청 하 고, 결과를 처리 합니다. 장치가 즉시 작동 중단 되 고 저전원 유휴 상태로 복귀 되지 않으므로 성능이 저하 됩니다. 잘못 예약 된 백그라운드 작업은 불필요 하 고 과도 한 전원 요구 사항으로 장치를 실수로 유지할 수 있습니다. 이 처럼 보입니다. (웹 사이트 폴링)는 상대적으로 짧은 시간 안에 장치를 사용할 수 없게 렌더링 합니다.

Android는 백그라운드에서 작업을 수행 하는 데 도움이 되는 다음과 같은 Api를 제공 하지만 지능적 작업 예약에는 충분 하지 않습니다. 

- **[의도](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** 서비스 &ndash; 의도 서비스는 작업을 수행 하는 데 유용 하지만 작업을 예약 하는 방법은 제공 하지 않습니다.
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; 이러한 api는 작업 예약만 허용 하지만 실제로 작업을 수행할 수 있는 방법은 제공 하지 않습니다. 또한 AlarmManager는 시간 기반 제약 조건만 허용 합니다. 즉, 특정 시간에 또는 특정 기간이 경과한 후에 알람을 발생 시킵니다. 
- Android 앱 &ndash; **[브로드캐스트 수신기](~/android/app-fundamentals/broadcast-receivers.md)** 는 시스템 차원의 이벤트 또는 의도에 대 한 응답으로 작업을 수행 하도록 브로드캐스트 수신기를 설정할 수 있습니다. 그러나 브로드캐스트 수신기는 작업을 실행 해야 하는 경우에 대 한 제어를 제공 하지 않습니다. 또한 Android 운영 체제의 변경 내용으로 인해 브로드캐스트 수신기가 작동 하는 시기 또는 응답할 수 있는 작업 종류가 제한 됩니다. 

백그라운드 작업을 효율적으로 수행 하는 두 가지 주요 기능 ( _백그라운드 작업_ 또는 _작업_이 라고도 함)은 다음과 같습니다.

1. **작업 &ndash;를 지능적으로 예약** 하는 것은 응용 프로그램이 백그라운드에서 작업을 수행 하는 경우 적절 한 시민으로 작업을 수행 하는 것이 중요 합니다. 응용 프로그램에서 작업 실행을 요구 하지 않아야 하는 것이 가장 좋습니다. 대신, 응용 프로그램은 작업을 실행할 수 있는 경우 충족 해야 하는 조건을 지정 하 고 조건이 충족 될 때 작업을 수행할 운영 체제를 사용 하 여 해당 작업을 예약 해야 합니다. 이렇게 하면 Android에서 작업을 실행 하 여 장치에서 효율성을 최대한 유지할 수 있습니다. 예를 들어 네트워킹에 관련 된 오버 헤드를 최대한 활용 하기 위해 네트워크 요청을 동시에 실행 하도록 일괄 처리할 수 있습니다.
2. 백그라운드 작업을 수행 하는 코드 &ndash; **작업을 캡슐화** 하는 작업은 사용자 인터페이스와 독립적으로 실행 될 수 있는 불연속 구성 요소에 캡슐화 해야 하며 어떤 이유로 작업이 완료 되지 않으면 비교적 쉽게 일정을 조정할 수 있습니다.

Android 작업 Scheduler는 흐름 API를 제공 하는 Android 운영 체제에 기본 제공 되는 프레임 워크로 서 백그라운드 작업 예약을 간소화 합니다.  Android 작업 스케줄러는 다음 형식으로 구성 됩니다.

- `Android.App.Job.JobScheduler`은 Android 응용 프로그램을 대신 하 여 작업을 예약 하 고, 실행 하 고, 필요한 경우 작업을 취소 하는 데 사용 되는 시스템 서비스입니다.
- `Android.App.Job.JobService`는 응용 프로그램의 주 스레드에서 작업을 실행 하는 논리를 사용 하 여 확장 해야 하는 추상 클래스입니다. 즉, `JobService`는 작업을 비동기적으로 수행 하는 방법을 담당 합니다.
- 작업을 실행 해야 하는 경우 `Android.App.Job.JobInfo` 개체에는 Android를 안내 하는 기준이 포함 됩니다.

Android 작업 스케줄러를 사용 하 여 작업을 예약 하려면 Xamarin.ios 응용 프로그램에서 `JobService` 클래스를 확장 하는 클래스에 코드를 캡슐화 해야 합니다. `JobService`은 작업 수명 중에 호출할 수 있는 세 가지 수명 주기 메서드를 포함 합니다.

- **Bool OnStartJob (JobParameters 매개 변수)** &ndash;이 메서드는 `JobScheduler`에서 작업을 수행 하기 위해 호출 되 고 응용 프로그램의 주 스레드에서 실행 됩니다. 작업을 비동기적으로 수행 하 고 남은 작업이 있는 경우 `true`을 반환 `false` 하거나 작업이 완료 된 경우에는 `JobService` 책임이 있습니다.
    
    `JobScheduler`에서이 메서드를 호출 하는 경우 작업 기간 동안 Android에서 wakelock을 요청 하 고 유지 합니다. 작업이 완료 되 면 `JobFinished` 메서드 (다음에 설명)를 호출 하 여이 사실을 `JobScheduler`에 알리는 `JobService` 책임이 있습니다.

- **Jobfinished (Jobfinished parameters, Bool needsReschedule)** &ndash;이 메서드는 `JobService`에 의해 호출 되어야 작업이 완료 되었음을 `JobScheduler`에 게 알립니다. `JobFinished`를 호출 하지 않으면 `JobScheduler` wakelock 제거 되지 않으므로 불필요 한 배터리가 소모 됩니다. 

- **Bool OnStopJob (JobParameters 매개 변수)** &ndash;이 작업은 Android에서 작업을 중간에 중지할 때 호출 됩니다. 다시 시도 조건에 따라 작업을 다시 예약 해야 하는 경우에는 `true`을 반환 해야 합니다 (아래에서 자세히 설명).

작업이 실행 될 수 있는 시기를 제어 하는 _제약 조건이_ 나 _트리거_ 를 지정할 수 있습니다. 예를 들어 장치가 요금을 청구 하거나 사진을 만들 때 작업을 시작 하는 경우에만 실행 되도록 작업을 제한할 수 있습니다.

이 가이드에서는 `JobService` 클래스를 구현 하 고 `JobScheduler`를 사용 하 여 예약 하는 방법에 대해 자세히 설명 합니다.

## <a name="requirements"></a>요구 사항

Android 작업 스케줄러에는 Android API 레벨 21 (Android 5.0) 이상이 필요 합니다. 

## <a name="using-the-android-job-scheduler"></a>Android 작업 스케줄러 사용

Android JobScheduler API를 사용 하는 세 가지 단계가 있습니다.

1. 작업을 캡슐화 할 JobService 형식을 구현 합니다.
2. `JobInfo.Builder` 개체를 사용 하 여 작업을 실행 하기 위한 `JobScheduler` 조건을 보유 하는 `JobInfo` 개체를 만듭니다. 
3. `JobScheduler.Schedule`를 사용 하 여 작업을 예약 합니다.

### <a name="implement-a-jobservice"></a>JobService 구현

Android 작업 스케줄러 라이브러리에서 수행 하는 모든 작업은 `Android.App.Job.JobService` 추상 클래스를 확장 하는 형식에서 수행 해야 합니다. `JobService` 만들기는 Android framework를 사용 하 여 `Service`을 만드는 것과 매우 비슷합니다. 

1. `JobService` 클래스를 확장 합니다.
2. `ServiceAttribute`를 사용 하 여 하위 클래스를 장식 하 고 `Name` 매개 변수를 패키지 이름 및 클래스 이름으로 구성 된 문자열로 설정 합니다 (다음 예제 참조).
3. `ServiceAttribute`의 `Permission` 속성을 `android.permission.BIND_JOB_SERVICE`문자열로 설정 합니다.
4. 작업을 수행 하는 코드를 추가 하 여 `OnStartJob` 메서드를 재정의 합니다. Android는 응용 프로그램의 주 스레드에서이 메서드를 호출 하 여 작업을 실행 합니다. 응용 프로그램이 차단 되는 것을 방지 하기 위해 스레드에서 몇 밀리초를 수행 해야 하는 작업은 더 오래 걸릴 수 있습니다.
5. 작업이 완료 되 면 `JobService`은 `JobFinished` 메서드를 호출 해야 합니다. 이 메서드는 `JobService` 작업의 수행을 `JobScheduler`에 알리는 방법입니다. `JobFinished`를 호출 하지 않으면 장치에서 불필요 한 요구 사항이 발생 하 여 배터리 수명을 단축할 `JobService`. 
6. `OnStopJob` 메서드를 재정의 하는 것도 좋습니다. 이 메서드는 작업이 완료 되기 전에 작업이 종료 될 때 Android에서 호출 되며, `JobService` 제공 하 여 리소스를 적절 하 게 삭제할 수 있는 기회를 제공 합니다. 작업을 다시 예약 해야 하는 경우이 메서드는 `true`을 반환 하거나 작업을 다시 실행 하는 것이 바람직하지 않은 경우 `false` 합니다.

다음 코드는 TPL을 사용 하 여 작업을 비동기적으로 수행 하는 응용 프로그램에 대 한 가장 간단한 `JobService`의 예입니다.

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

Xamarin Android 응용 프로그램은 `JobService`를 직접 인스턴스화하지 않으며 대신 `JobScheduler`에 `JobInfo` 개체를 전달 합니다. `JobScheduler`은 요청 된 `JobService` 개체를 인스턴스화하고 `JobInfo`의 메타 데이터에 따라 `JobService` 예약 및 실행 합니다. `JobInfo` 개체는 다음 정보를 포함 해야 합니다.

- **JobId** &ndash; `JobScheduler`에 대 한 작업을 식별 하는 데 사용 되는 `int` 값입니다. 이 값을 다시 사용 하면 기존 작업이 업데이트 됩니다. 값은 응용 프로그램에 대해 고유 해야 합니다. 
- **Jobservice** &ndash;이 매개 변수는 `JobScheduler`에서 작업을 실행 하는 데 사용 해야 하는 형식을 명시적으로 식별 하는 `ComponentName`입니다. 

이 확장 메서드는 활동과 같은 Android `Context`를 사용 하 여 `JobInfo.Builder`을 만드는 방법을 보여 줍니다.

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

Android 작업 Scheduler의 강력한 기능은 작업이 실행 되는 시기 또는 작업이 실행 될 수 있는 상황을 제어 하는 기능입니다. 다음 표에서는 앱이 작업을 실행할 수 있는 시기에 영향을 줄 수 있도록 하는 `JobInfo.Builder`에 대 한 몇 가지 방법에 대해 설명 합니다.  

|  메서드 | 설명   |
|---|---|
| `SetMinimumLatency`  | 작업이 실행 되기 전에 관찰 해야 하는 지연 (밀리초)을 지정 합니다. |
| `SetOverridingDeadline`  | 이 시간 (밀리초)이 경과 하기 전에 작업을 실행 해야 하는를 선언 합니다. |
| `SetRequiredNetworkType`  | 작업에 대 한 네트워크 요구 사항을 지정 합니다. |
| `SetRequiresBatteryNotLow` | 장치가 사용자에 게 "배터리 부족" 경고를 표시 하지 않는 경우에만 작업이 실행 될 수 있습니다. |
| `SetRequiresCharging` | 작업은 배터리가 충전 된 경우에만 실행 될 수 있습니다. |
| `SetDeviceIdle` | 장치가 사용 중일 때 작업이 실행 됩니다. |
| `SetPeriodic` | 작업이 정기적으로 실행 되도록 지정 합니다. |
| `SetPersisted` | 작업은 장치 재부팅 간에 perisist 되어야 합니다. | 

`SetBackoffCriteria`는 작업을 다시 실행 하기 전에 `JobScheduler` 기다려야 하는 시간에 대 한 몇 가지 지침을 제공 합니다. 백오프 조건에는 지연 시간 (밀리초의 기본값은 30 초)과 사용 해야 하는 백오프 유형 ( _백오프 정책이_ 나 _재시도 정책이_라고도 함)이 있습니다. 두 정책은 `Android.App.Job.BackoffPolicy` 열거형에 캡슐화 됩니다.

- 지 수 백오프 정책을 &ndash; `BackoffPolicy.Exponential` 각 실패 후에 초기 백오프 값이 급격히 증가 합니다. 작업이 처음으로 실패 하면 라이브러리는 작업을 다시 예약 하기 전에 지정 된 초기 간격을 대기 합니다 (예: 30 초). 작업이 실패 하는 두 번째 경우 라이브러리는 작업을 실행 하기 전에 최소 60 초 정도 기다립니다. 세 번째 실패 후 라이브러리는 120 초 정도 대기 하 게 됩니다. 기본값입니다.
- `BackoffPolicy.Linear` &ndash;이 전략은 작업이 성공할 때까지 설정 된 간격으로 실행 되도록 다시 예약 해야 하는 선형 백오프. 선형 백오프는 가능한 한 빨리 완료 되어야 하는 작업 또는 자신을 신속 하 게 해결 하는 문제에 가장 적합 합니다. 

`JobInfo` 개체 만들기에 대 한 자세한 내용은 [`JobInfo.Builder` 클래스에 대 한 Google 설명서](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html)를 참조 하세요.

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>JobInfo를 통해 작업에 매개 변수 전달

매개 변수는 `Job.Builder.SetExtras` 메서드와 함께 전달 되는 `PersistableBundle`를 만들어 작업에 전달 됩니다.

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

`PersistableBundle`는 `JobService`의 `OnStartJob` 메서드에서 `Android.App.Job.JobParameters.Extras` 속성을 통해 액세스할 수 있습니다.

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>작업 예약

작업을 예약 하기 위해 Xamarin.ios 응용 프로그램은 `JobScheduler` 시스템 서비스에 대 한 참조를 가져온 후 이전 단계에서 만든 `JobInfo` 개체를 사용 하 여 `JobScheduler.Schedule` 메서드를 호출 합니다. `JobScheduler.Schedule`는 두 정수 값 중 하나를 사용 하 여 즉시 반환 됩니다.

- **Jobscheduler. ResultSuccess** &ndash; 작업이 성공적으로 예약 되었습니다. 
- **Jobscheduler. ResultFailure** &ndash; 작업을 예약할 수 없습니다. 이 오류는 일반적으로 `JobInfo` 매개 변수가 충돌 하기 때문에 발생 합니다.

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

예약 된 모든 작업을 취소 하거나 `JobsScheduler.CancelAll()` 메서드나 `JobScheduler.Cancel(jobId)` 메서드를 사용 하 여 단일 작업만 취소할 수 있습니다.

```csharp
// Cancel all jobs
jobScheduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>요약

이 가이드에서는 Android 작업 스케줄러를 사용 하 여 백그라운드에서 작업을 지능적으로 수행 하는 방법에 대해 설명 했습니다. `JobService` 수행할 작업을 캡슐화 하는 방법 및 `JobScheduler`를 사용 하 여 해당 작업을 예약 하 고, `JobTrigger` 조건을 지정 하 고 `RetryStrategy`를 사용 하 여 오류를 처리 하는 방법을 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [인텔리전트 작업-예약](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler API 참조](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [JobScheduler를 사용 하 여 pro와 같은 작업 예약](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android 배터리 및 메모리 최적화-Google i/o 2016 (비디오)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler-이력서 Ruppert](https://www.youtube.com/watch?v=aSjBBPYjelE)
