---
title: Xamarin Android를 사용 하 여 서비스 시작
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 1b7bed0fc6dba1d9f80524ac3429b7fdcb751ab9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755067"
---
# <a name="started-services-with-xamarinandroid"></a>Xamarin Android를 사용 하 여 서비스 시작

## <a name="started-services-overview"></a>서비스 개요 시작

시작 된 서비스는 일반적으로 클라이언트에 직접 피드백 또는 결과를 제공 하지 않고 작업 단위를 수행 합니다. 작업 단위의 예는 서버에 파일을 업로드 하는 서비스입니다. 클라이언트는 서비스에 대 한 요청을 수행 하 여 장치에서 웹 사이트로 파일을 업로드 합니다. 서비스는 파일을 자동으로 업로드 하 고 (앱이 포그라운드에서 작업을 하지 않은 경우에도) 업로드가 완료 되 면 자동으로 종료 됩니다. 시작 된 서비스는 응용 프로그램의 UI 스레드에서 실행 됩니다. 즉, 서비스가 UI 스레드를 차단 하는 작업을 수행 하는 경우 필요에 따라 스레드를 만들고 삭제 해야 합니다.

바인딩된 서비스와 달리 "pure" 시작 된 서비스와 해당 클라이언트 간의 통신 채널은 없습니다. 즉, 시작 된 서비스는 바인딩된 서비스와는 다른 수명 주기 메서드를 구현 합니다. 다음 목록은 시작 된 서비스의 일반적인 수명 주기 메서드를 강조 표시 합니다.

- `OnCreate`&ndash; 서비스가 처음 시작 될 때 한 번 호출 됩니다. 초기화 코드를 구현 해야 하는 위치입니다.
- `OnBind`&ndash; 이 메서드는 모든 서비스 클래스에서 구현 해야 하지만 시작 된 서비스에는 일반적으로 클라이언트가 바인딩되어 있지 않습니다. 이로 인해 시작 된 서비스는만 반환 `null`합니다. 이와 달리 하이브리드 서비스 (바인딩된 서비스와 시작 된 서비스의 조합)는 클라이언트에 대해를 `Binder` 구현 하 고 반환 해야 합니다.
- `OnStartCommand`&ndash; 에`StartService` 대 한 호출 또는 시스템 다시 시작에 대 한 응답으로 서비스를 시작 하는 각 요청에 대해 호출 됩니다. 서비스에서 장기 실행 작업을 시작할 수 있는 위치입니다. 메서드는 메모리 부족 `StartCommandResult` 으로 인해 종료 된 후 시스템에서 서비스를 다시 시작 해야 하는지 여부를 나타내는 값을 반환 합니다. 이 호출은 주 스레드에서 수행 됩니다. 이 방법은 아래에 자세히 설명 되어 있습니다.
- `OnDestroy`&ndash; 이 메서드는 서비스가 제거 될 때 호출 됩니다. 필요한 최종 정리 작업을 수행 하는 데 사용 됩니다.

시작 된 서비스 `OnStartCommand` 에 대 한 중요 한 메서드는 메서드입니다. 서비스에서 작업을 수행 하는 요청을 받을 때마다 호출 됩니다. 다음 코드 조각은의 `OnStartCommand`예제입니다. 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

첫 번째 매개 변수는 `Intent` 수행할 작업에 대 한 메타 데이터를 포함 하는 개체입니다. 두 번째 매개 변수는 `StartCommandFlags` 메서드 호출에 대 한 일부 정보를 제공 하는 값을 포함 합니다. 이 매개 변수는 다음 두 가지 가능한 값 중 하나입니다.

- `StartCommandFlag.Redelivery`즉, `Intent` 은 이전`Intent`을 다시 배달 하는 것입니다. &ndash; 이 값은 서비스가 반환 `StartCommandResult.RedeliverIntent` 되었지만 제대로 종료 되기 전에 중지 된 경우에 제공 됩니다.
-`StartCommandFlag.Retry`&dash; 이전 `OnStartCommand` 호출이 실패 하 고 Android에서 이전에 실패 한 시도와 동일한 의도를 사용 하 여 서비스를 다시 시작 하려고 시도 하는 경우이 값이 수신 됩니다.

마지막으로 세 번째 매개 변수는 요청을 식별 하는 응용 프로그램에 고유한 정수 값입니다. 여러 호출자가 동일한 서비스 개체를 호출할 수 있습니다. 이 값은 서비스를 시작 하기 위해 지정 된 요청을 사용 하 여 서비스를 중지 하는 요청을 연결 하는 데 사용 됩니다. 이에 대해서는 [서비스 중지](#Stopping_the_Service)섹션에서 자세히 설명 합니다. 

이 값 `StartCommandResult` 은 서비스에서 리소스 제약 조건으로 인해 서비스가 종료 되는 경우 수행할 작업에 대 한 제안으로 서비스에 의해 반환 됩니다. 에 `StartCommandResult`는 다음과 같은 세 가지 값을 사용할 수 있습니다.

- **[Startcommandresult. notsticky](xref:Android.App.StartCommandResult.NotSticky)** &ndash; 이 값은 Android에서 종료 된 서비스를 다시 시작할 필요가 없음을 나타냅니다. 이에 대 한 예로 앱에서 갤러리에 대 한 미리 보기를 생성 하는 서비스를 생각해 보세요. 서비스가 종료 되 &ndash; 면 다음에 앱을 실행할 때 축소판 그림을 다시 만들 수 있습니다.
- **[Startcommandresult. 스티커](xref:Android.App.StartCommandResult.Sticky)** &ndash; 이는 서비스를 다시 시작 하지만 서비스를 시작한 마지막 의도를 제공 하지는 않습니다. 처리할 `null` 보류 중인 의도가 없으면이 의도 매개 변수에 대해가 제공 됩니다. 이에 대 한 예는 음악 플레이어 앱이 될 수 있습니다. 서비스는 음악을 재생할 준비를 다시 시작 하지만 마지막 노래를 재생 합니다.
- **[Startcommandresult.](xref:Android.App.StartCommandResult.RedeliverIntent)** &ndash; 이 값은 Android에서 서비스를 다시 시작 하 고 마지막 `Intent`을 다시 배달 하도록 지시 합니다. 이에 대 한 예는 앱에 대 한 데이터 파일을 다운로드 하는 서비스입니다. 서비스가 중지 된 경우에도 데이터 파일을 다운로드 해야 합니다. 을 반환 `StartCommandResult.RedeliverIntent`하면 Android에서 서비스를 다시 시작할 때 다운로드할 파일의 URL을 보유 하 고 있는 의도도 제공 됩니다. 이렇게 하면 코드의 정확한 구현에 따라 다운로드를 다시 시작 하거나 다시 시작할 수 있습니다.

에 `StartCommandResult` 는네번째`StartCommandResult.ContinuationMask`값이 있습니다. &ndash; 이 값은에 의해 `OnStartCommand` 반환 되며 Android에서 종료 된 서비스를 계속 하는 방법을 설명 합니다. 일반적으로이 값은 서비스를 시작 하는 데 사용 되지 않습니다.

시작 된 서비스의 키 수명 주기 이벤트는 다음 다이어그램에 나와 있습니다. 

![수명 주기 메서드가 호출 되는 순서를 보여 주는 다이어그램](started-services-images/started-service-01.png "수명 주기 메서드가 호출 되는 순서를 보여 주는 다이어그램")

<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>서비스 중지

시작 된 서비스는 무기한으로 계속 실행 됩니다. Android는 충분 한 시스템 리소스가 있는 한 서비스를 계속 실행 합니다. 클라이언트에서 서비스를 중지 해야 합니다. 그렇지 않으면 서비스가 해당 작업을 완료 했을 때 중지 될 수 있습니다. 서비스를 중지 하는 방법에는 다음 두 가지가 있습니다. 

- **[StopService ()](xref:Android.Content.Context.StopService*)** 클라이언트 (예: 작업)는 메서드를 `StopService` 호출 하 여 서비스 중지를 요청할 수 있습니다. &ndash;

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

- **[Android. Service. StopSelf ()](xref:Android.App.Service.StopSelf*)** 서비스는 다음을 `StopSelf`호출 하 여 종료 될 수 있습니다. &ndash;

    ```csharp
    StopSelf();
    ```

### <a name="using-startid-to-stop-a-service"></a>StartId를 사용 하 여 서비스 중지

여러 호출자가 서비스 시작을 요청할 수 있습니다. 처리 중인 시작 요청이 있는 경우 서비스는에 `startId` `OnStartCommand` 전달 된를 사용 하 여 서비스가 중간에 중지 되지 않도록 할 수 있습니다. 는 `startId` 에 대 `StartService`한 최신 호출에 해당 하며 호출 될 때마다 증가 됩니다. 따라서에 `StartService` 대 한 후속 요청이 아직를 `StopSelfResult` `OnStartCommand`호출 하지 않은 경우 서비스는를 호출 하 여를 호출 하는 대신 `StopSelf`수신 된 최신 값 `startId` 을 전달 합니다. 에 `StartService` 대 한 호출로 인해에 대 `OnStartCommand`한 해당 호출이 아직 `StopSelf` 발생 하지 않은 경우에는 호출에 사용 된가 `startId` 최신 `StartService` 호출에 해당 하지 않으므로 시스템에서 서비스를 중지 하지 않습니다.

## <a name="related-links"></a>관련 링크

- [StartedServicesDemo (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-startedservicesdemo)
- [Android.App.Service](xref:Android.App.Service)
- [Android.App.StartCommandFlags](xref:Android.App.StartCommandFlags)
- [Android.App.StartCommandResult](xref:Android.App.StartCommandResult)
- [Android.Content.BroadcastReceiver](xref:Android.Content.BroadcastReceiver)
- [Android.Content.Intent](xref:Android.Content.Intent)
- [Android.OS.Handler](xref:Android.OS.Handler)
- [Android.Widget.Toast](xref:Android.Widget.Toast)
- [상태 표시줄 아이콘](https://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)
