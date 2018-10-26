---
title: Xamarin.Android 사용 하 여 시작된 된 서비스
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 9e7dabf87314d87ab5ab335c220c0e292e56073b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110897"
---
# <a name="started-services-with-xamarinandroid"></a>Xamarin.Android 사용 하 여 시작된 된 서비스

## <a name="started-services-overview"></a>시작된 서비스 개요

시작된 서비스는 일반적으로 클라이언트에 결과 나 직접 피드백을 제공 하지 않고 작업 단위를 수행 합니다. 작업 단위의 예로 서버에 파일을 업로드 하는 서비스입니다. 클라이언트 웹 사이트에 장치에서 파일을 업로드 하는 서비스 요청을 수행 합니다. 서비스를 자동으로 파일을 업로드 합니다 (경우에 앱 포그라운드에서 활동이 없습니다.), 업로드가 완료 되 면 자동으로 종료 하 고 있습니다. 응용 프로그램의 UI 스레드에서 시작된 서비스를 실행할를 실현 하는 것이 반드시 합니다. 즉, 서비스를 UI 스레드를 차단 하는 작업을 수행 하려면 해야 만들기를 필요에 따라 스레드를 삭제 합니다.

바인딩된 서비스와 달리 "순수" 시작된 서비스와 해당 클라이언트 간의 통신 채널이 있습니다. 즉, 시작된 서비스 바인딩된 서비스 보다 몇 가지 다른 수명 주기 메서드를 구현 합니다. 다음은 시작된 서비스의 일반적인 수명 주기 메서드를 보여줍니다.

* `OnCreate` &ndash; 서비스를 처음 시작할 때 한 번 호출 됩니다. 초기화 코드를 구현 해야 하는 위치입니다.
* `OnBind` &ndash; 하지만이 메서드는 시작된 서비스에 일반적으로 바인딩된 클라이언트 없습니다 모든 서비스 클래스에 의해 구현 되어야 합니다. 따라서 시작된 서비스 반환 `null`합니다. 반면, 하이브리드 서비스 (즉, 바인딩된 서비스와 시작된 서비스의 조합)에 있는 구현 하 고 반환 하는 `Binder` 클라이언트에 대 한 합니다.
* `OnStartCommand` &ndash; 에 대 한 호출에 대 한 응답에서 서비스를 시작 하려면 각 요청에 대 한 호출 `StartService` 또는 시스템에서 다시 시작 합니다. 서비스는 장기 실행 작업을 시작할 수 있는 위치입니다. 메서드는 `StartCommandResult` 나타내는 값 어떻게 시스템 메모리 부족으로 인해 종료 된 후 서비스를 다시 시작 처리 하는 경우 또는 합니다. 이 호출은 주 스레드에서 수행 됩니다. 이 메서드는 아래에서 자세히 설명 되어 있습니다.
* `OnDestroy` &ndash; 이 메서드는 서비스 소멸 될 때 호출 됩니다. 모든 최종 수행 하 되 필요한 정리 합니다.

시작된 서비스에 대 한 중요 한 메서드는는 `OnStartCommand` 메서드. 때마다 호출 되는 서비스 작업을 수행 하려면 요청을 수신 합니다. 다음 코드 조각은의 한 예로 `OnStartCommand`: 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

첫 번째 매개 변수는 `Intent` 수행 하는 작업에 대 한 메타 데이터를 포함 하는 개체입니다. 두 번째 매개 변수를 포함 한 `StartCommandFlags` 메서드 호출에 대 한 정보를 제공 하는 값입니다. 이 매개 변수는 가능한 두 값 중 하나:

* `StartCommandFlag.Redelivery` &ndash; 즉 합니다 `Intent` 은 이전에 다시 배달 됩니다 `Intent`합니다. 서비스를 반환한 경우이 값이 제공 `StartCommandResult.RedeliverIntent` 하지만를 제대로 종료 수 전에 중지 되었습니다.
* `StartCommandFlag.Retry` &dash; 이 값이 이전 하는 경우 수신 `OnStartCommand` 호출이 실패 하 고 Android 이전 실패 한 시도 동일한 의도 사용 하 여 서비스를 다시 시작 하려고 합니다.
 
마지막으로 세 번째 매개 변수는 요청을 식별 하는 응용 프로그램에 고유한 정수 값입니다. 있기 여러 호출자가 동일한 서비스 개체를 호출할 수 있습니다. 이 값은 서비스를 시작 하도록 지정된 된 요청을 사용 하 여 서비스를 중지 하도록 요청 하려면 연결에 사용 됩니다. 섹션에서 자세히 다룰 [서비스를 중지](#Stopping_the_Service)합니다. 

값 `StartCommandResult` 에 의해 반환 된 서비스 제안으로 Android에 대 한 서비스 리소스 제약으로 인해 종료 될 경우 수행할 작업입니다. 세 가지 가능한 값에 대 한 `StartCommandResult`:

* **[StartCommandResult.NotSticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.NotSticky/)**  &ndash; 이 값에 하는 서비스를 다시 시작 하는 데 필요한 아닌 Android 지시 합니다. 이 예를 들어, 앱에서 갤러리의 미리 보기를 생성 하는 서비스를 고려 합니다. 미리 보기를 즉시 다시 중요 하지 서비스가 종료 될 경우 &ndash; 미리 보기 앱을 실행한 다음에 다시 만들 수 있습니다.
* **[StartCommandResult.Sticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.Sticky/)**  &ndash; 그러면 Android 서비스를 다시 시작이 아니라 서비스를 시작 하는 마지막 의도 제공 합니다. 를 처리 하기 위해 보류 중인 의도 없는 경우는 `null` 의도 매개 변수에 대해 제공 됩니다. 이 예는 음악 플레이어 앱 수 있습니다. 음악을 재생 하는 데 서비스 준비 다시 시작 됩니다 있지만 마지막 song을 재생 합니다. 
* **[StartCommandResult.RedeliverIntent](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.RedeliverIntent/)**  &ndash; 이 값은 서비스를 다시 시작 하 고 마지막으로 다시 제공 하는 Android 알 `Intent`합니다. 이 예제는 앱에 대 한 데이터 파일을 다운로드 하는 서비스. 서비스 종료 될 경우 데이터 파일을 아직 다운로드 해야 합니다. 반환 하 여 `StartCommandResult.RedeliverIntent`Android 서비스 (보유 하는 다운로드할 파일의 URL) 의도 방법도 제공 서비스를 다시 시작 하는 경우. 이렇게 하면 다운로드를 다시 시작 하거나 (에 따라 코드의 정확한 구현을) 다시 시작 합니다.

네 번째 값 `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask`합니다. 이 값은 반환 `OnStartCommand` Android는 서비스를 계속 하는 방법에 대해 설명 하 고 종료 했습니다. 이 값이 서비스를 시작 하려면 일반적으로 사용 되지 않습니다.

시작된 서비스의 키 수명 주기 이벤트는이 다이어그램에 나와 있습니다. 

![수명 주기 메서드는 호출 되는 순서를 보여 주는 다이어그램](started-services-images/started-service-01.png "수명 주기 메서드는 호출 되는 순서를 보여 주는 다이어그램입니다.")


<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>서비스를 중지합니다.

시작된 서비스는 계속 실행 무기한; Android는 리소스가 충분 한 시스템으로 실행 하는 서비스를 유지 합니다. 클라이언트는 서비스를 중지 해야 하거나 서비스 작업이 완료 되 면 자체가 중지 될 수 있습니다. 서비스를 중지 하는 방법은 두 가지 있습니다. 
 
* **[Android.Content.Context.StopService()](https://developer.xamarin.com/api/member/Android.Content.Context.StopService/p/Android.Content.Intent/)**  &ndash; 호출 하 여 서비스를 중지 (예: 활동) 클라이언트가 요청할 수는 `StopService` 메서드: 

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

* **[Android.App.Service.StopSelf()](https://developer.xamarin.com/api/member/Android.App.Service.StopSelf()/)**  &ndash; 서비스 수 자동으로 종료를 호출 하 여는 `StopSelf`:

    ```csharp
    StopSelf();
    ```
    
### <a name="using-startid-to-stop-a-service"></a>StartId를 사용 하 여 서비스를 중지 하려면

여러 호출자는 서비스를 시작할 수 있는지를 요청할 수 있습니다. 서비스를 사용할 수는 처리 중인 시작 요청이 있으면 합니다 `startId` 에 전달 되는 `OnStartCommand` 를 너무 일찍 중지할 서비스를 방지 하기 위해. 합니다 `startId` 최신의 호출에 해당 됩니다 `StartService`, 호출 될 때마다 증가 하 고 합니다. 따라서 경우 후속 요청을 `StartService` 호출에서 아직 되었습니다 되지 `OnStartCommand`, 서비스가 호출할 수 `StopSelfResult`, 최신 값을 전달 `startId` 받은 (간단히 호출 하는 대신 `StopSelf`). 호출 하는 경우 `StartService` 해당 호출에서 아직 발생 하지는 `OnStartCommand`, 때문에 시스템 서비스를 중지 되지 것입니다는 `startId` 에 사용 되는 합니다 `StopSelf` 호출 최신에 해당 하지 `StartService` 호출 합니다.


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
