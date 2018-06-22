---
title: Xamarin.Android 사용 하 여 시작된 서비스
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: c0aeeaad3798dd840e69c6da6d7857298f4da3c1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767468"
---
# <a name="started-services-with-xamarinandroid"></a>Xamarin.Android 사용 하 여 시작된 서비스

## <a name="started-services-overview"></a>시작된 서비스 개요

시작된 된 서비스는 일반적으로 클라이언트에 직접적인 피드백이 나 결과 제공 하지 않고 작업의 단위를 수행 합니다. 작업 단위의 예로 서버에 파일을 업로드 하는 서비스입니다. 클라이언트 장치에서 웹 사이트에 파일 업로드 서비스에 요청을 수행 합니다. 서비스는 자동 모드로 파일 (경우에 업로드할 앱 포그라운드에서 활동이 없습니다.), 업로드가 완료 되 면 자동으로 종료 하 고 있습니다. 시작된 서비스 응용 프로그램의 UI 스레드에서 실행 됩니다를 나타내려고 하는 것이 유용 합니다. 즉, UI 스레드를 차단 하는 작업을 수행 하는 서비스가 있으면 그 해야를 만들고 필요에 따라 스레드를 삭제 합니다.

바인딩된 서비스와 달리 "순수" 시작된 서비스와 클라이언트 간의 통신 채널이 있습니다. 즉, 시작된 되는 서비스 바인딩된 서비스 보다 몇 가지 다른 수명 주기 메서드를 구현 합니다. 다음은 시작된 서비스의 일반적인 수명 주기 메서드를 강조 표시합니다.

* `OnCreate` &ndash; 서비스를 처음 시작할 때 한 번을 호출 됩니다. 이 초기화 코드를 구현 해야 합니다.
* `OnBind` &ndash; 이 메서드는 시작된 서비스에 일반적으로 집합에 연결 되도록 클라이언트 없는 있지만 모든 서비스 클래스에서 구현 합니다. 이 때문에 시작된 되는 서비스는만 반환 `null`합니다. (즉, 바인딩된 서비스와 시작된 서비스의 조합) 하이브리드 서비스 구현 하 고 반환 하는 반면, 한 `Binder` 클라이언트에 대 한 합니다.
* `OnStartCommand` &ndash; 에 대 한 호출에 대 한 응답에서 서비스를 시작 하는 각 요청에 대해 호출 `StartService` 또는 시스템에 의해 다시 시작 합니다. 이 서비스가 모든 장기 실행 태스크를 시작할 수 있습니다. 메서드는 `StartCommandResult` 나타내는 값 방법을 시스템 메모리 부족으로 인해 종료 된 후 서비스를 다시 시작을 처리 해야 하는 경우 또는 합니다. 이 호출은 주 스레드에서 수행 합니다. 이 메서드는 아래에서 자세히 설명 되어 있습니다.
* `OnDestroy` &ndash; 이 메서드는 서비스 소멸 될 때 호출 됩니다. 모든 최종 수행 하는 데 필요한 정리를 합니다.

시작된 서비스에 대 한 중요 한 메서드는는 `OnStartCommand` 메서드. 될 때마다 호출 되며 서비스 작업을 수행 하는 요청을 받습니다. 다음 코드 조각의 한 예로 `OnStartCommand`: 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

첫 번째 매개 변수는 `Intent` 개체 작업을 수행 하는 방법에 대 한 메타 데이터를 포함 합니다. 두 번째 매개 변수를 포함 한 `StartCommandFlags` 메서드 호출에 대 한 정보를 제공 하는 값입니다. 이 매개 변수는 가능한 두 값 중 하나를 가집니다.

* `StartCommandFlag.Redelivery` &ndash; 즉는 `Intent` 은 이전에 다시 배달은 `Intent`합니다. 서비스를 반환 하는 경우이 값이 제공 `StartCommandResult.RedeliverIntent` 하지만를 제대로 종료 수 전에 중지 되었습니다.
* `StartCommandFlag.Retry` &dash; 이 값이 이전 하는 경우 수신 `OnStartCommand` Android 이전의 시도가 실패와 같은 목적으로 서비스를 다시 시작을 시도 하 고 호출이 실패 했습니다.
 
마지막으로 세 번째 매개 변수는 요청을 식별 하는 응용 프로그램에 고유한 정수 값입니다. 있기 여러 호출자가 동일한 서비스 개체를 호출할 수 있습니다. 이 값은 지정된 된 요청 서비스를 시작 하려면 사용 하 여 서비스를 중지 요청에 연결 하려면 사용 합니다. 섹션에서 자세히 살펴봅니다 [서비스를 중지](#Stopping_the_Service)합니다. 

값 `StartCommandResult` 서비스에서 반환 되는 제안 값으로 Android에 서비스 리소스 제약 조건으로 인해 종료 될 경우 수행할 작업에 있습니다. 세 가지 가능한 값에 대 한 `StartCommandResult`:

* **[StartCommandResult.NotSticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.NotSticky/)**  &ndash; 이 값에 종료 하는 서비스를 다시 시작할 필요가 없는 Android를 나타냅니다. 이 예를 들어 응용 프로그램 갤러리의 미리 보기를 생성 하는 서비스를 것이 좋습니다. 그럴를 축소판 그림을 즉시 다시 중요 한 서비스가 종료 될 경우 &ndash; 축소판 그림 다음에 앱이 실행 다시 만들 수 있습니다.
* **[StartCommandResult.Sticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.Sticky/)**  &ndash; 이렇게 하면 Android 서비스를 다시 시작 있지만 서비스를 시작 하는 마지막 의도 배달할 수는 없습니다. 를 처리 하기 위해 보류 중인 의도 없는 경우는 `null` 의도 매개 변수에 대해 제공 됩니다. 이러한 예로 음악 플레이어 응용 프로그램 들 수 있습니다. 음악 재생 서비스가 준비 시작 되도록 하지만 마지막 곡을 재생 됩니다. 
* **[StartCommandResult.RedeliverIntent](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.RedeliverIntent/)**  &ndash; 이 값을 확인할 서비스를 다시 시작 및 다시 마지막을 배달 하는 Android `Intent`합니다. 이러한 예는 응용 프로그램에 대 한 데이터 파일을 다운로드 하는 서비스입니다. 서비스가 종료 될 경우 데이터 파일을 다운로드할 수 여전히 필요 합니다. 반환 하 여 `StartCommandResult.RedeliverIntent`Android 서비스에 의도 (보유 하는 다운로드할 파일의 URL)도 제공할 서비스를 다시 시작 하는 경우. 이렇게 하면 다운로드를 다시 시작 하거나 (에 따라 코드의 정확한 구현)를 다시 시작 합니다.

네 번째 값에 대 한는 `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask`합니다. 이 값이 반환 하 여 `OnStartCommand` Android는 서비스를 계속 하는 방법에 대해 설명 종료 했습니다. 이 값 서비스를 시작 하려면 일반적으로 사용 되지 않습니다.

시작된 서비스의 키 수명 주기 이벤트는이 다이어그램에 표시 됩니다. 

![수명 주기 메서드 호출 되는 순서를 보여 주는 다이어그램](started-services-images/started-service-01.png "수명 주기 메서드 호출 되는 순서를 보여 주는 다이어그램입니다.")


<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>서비스 중지

시작된 서비스는 계속 실행 무기한; Android에 충분 한 리소스가 시스템으로 서비스를 실행 하는 유지 됩니다. 클라이언트는 서비스를 중지 해야 하거나 서비스 작업을 완료 자체가 중지 될 수 있습니다. 서비스를 중지 하는 방법은 두 가지가 있습니다. 
 
* **[Android.Content.Context.StopService()](https://developer.xamarin.com/api/member/Android.Content.Context.StopService/p/Android.Content.Intent/)**  &ndash; 클라이언트 (예: 활동)를 호출 하 여 서비스 중지를 요청할 수는 `StopService` 메서드: 

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

* **[Android.App.Service.StopSelf()](https://developer.xamarin.com/api/member/Android.App.Service.StopSelf()/)**  &ndash; 서비스 종료 될 수 있습니다 자체를 호출 하 여는 `StopSelf`:

    ```csharp
    StopSelf();
    ```
    
### <a name="using-startid-to-stop-a-service"></a>StartId를 사용 하 여 서비스를 중지 하려면

서비스가 다시 시작 될 여러 호출자가 요청할 수 있습니다. 처리 중인 시작 요청이 면 서비스 צ ְ ײ는 `startId` 전달 되는 `OnStartCommand` 서비스 중간에 중단 되지 않도록 합니다. `startId` 에 대 한 최신 호출에 해당 됩니다 `StartService`, 및가 호출 될 때마다 증가 됩니다. 따라서 경우 후속 요청을 `StartService` 에 대 한 호출 아직 낮아지는 하지 `OnStartCommand`, 서비스를 호출할 수 `StopSelfResult`의 최신 값을 전달 `startId` 수신한 (단순히 호출 하는 대신 `StopSelf`). 호출 하는 경우 `StartService` 호출이 대응 하는 아직 낮아지는 하지 `OnStartCommand`, 때문에 시스템 서비스를 중지 하지 것입니다는 `startId` 에 사용 되는 `StopSelf` 호출을 최신 일치 하지 것입니다 `StartService` 호출 합니다.


## <a name="related-links"></a>관련 링크

- [StartedServicesDemo (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/StartedServicesDemo/)
- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service)
- [Android.App.StartCommandFlags](https://developer.xamarin.com/api/type/Android.App.StartCommandFlags)
- [Android.App.StartCommandResult](https://developer.xamarin.com/api/type/Android.App.StartCommandResult)
- [Android.Content.BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Android.Content.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent)
- [Android.OS.Handler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Android.Widget.Toast](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
- [상태 표시줄 아이콘](http://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)
