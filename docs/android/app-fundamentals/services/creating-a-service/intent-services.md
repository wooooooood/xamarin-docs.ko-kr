---
title: Xamarin.Android의 의도 서비스
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 80849213649707615f8bd8e941e1a51c6b54e76e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="intent-services-in-xamarinandroid"></a>Xamarin.Android의 의도 서비스

## <a name="intent-services-overview"></a>의도 서비스 개요

둘 다 시작 하 고 서비스를 비동기적으로 작업을 수행할 필요는 서비스를 유지 하기 위해서는 성능 부드러운 함을 의미 하 고 주 스레드에서 실행 바인딩됩니다. 와 이러한 문제를 해결 하는 가장 간단한 방법 중 하나는 _작업자 큐 프로세서 패턴_작업을 수행 해야 하는 단일 스레드 서비스를 제공 하는 큐에서 위치, 합니다. 

[ `IntentService` ](https://developer.xamarin.com/api/type/Android.App.IntentService/) 의 서브 클래스는 `Service` Android 특정 구현이이 패턴을 제공 하는 클래스입니다. 큐 작업에서는 큐를 처리 하는 작업자 스레드 시작 자신이 관리할 및 풀링 요청 작업자 스레드에서 실행 되도록 큐를 해제 합니다. `IntentService` 백그라운드 자체를 중지 하 고 더 이상 작업이 큐에 있을 때 작업자 스레드를 제거 합니다.
 
작업을 만들어서 큐로 전송 되는 `Intent` 와 전달 하는 `Intent` 에 `StartService` 메서드 합니다.

중지 하거나 중단할 수 없으면는 `OnHandleIntent` 메서드 `IntentService` 진행 되는 동안 합니다. 이 설계로 인해는 `IntentService` 유지 하는 상태 비저장 &ndash; 활성 연결 또는 응용 프로그램의 나머지 부분과에서 통신에 의존 하지 않아야 합니다. `IntentService` statelessly 작업 요청을 처리 하는 것입니다.

하위 클래스에 대 한 두 가지 요구 사항이 `IntentService`:

1. 새 형식 (하위 클래스에서 만든 `IntentService`)만 재정의 `OnHandleIntent` 메서드.
2. 새 형식에 대 한 생성자는 요청을 처리할 작업자 스레드에 이름을 지정 하는 데 사용 되는 문자열이 필요 합니다. 이 작업자 스레드의 이름은 응용 프로그램을 디버깅할 때 주로 사용 됩니다.

다음 코드는 `IntentService` 으로 재정의 된 구현을 `OnHandleIntent` 메서드:

```csharp
[Service]
public class DemoIntentService: IntentService
{
    public DemoIntentService () : base("DemoIntentService")
    {
    }
    
    protected override void OnHandleIntent (Android.Content.Intent intent)
    {
        Console.WriteLine ("perform some long running work");
        ...
        Console.WriteLine ("work complete");
    }
}
```

작업에 게 보내집니다는 `IntentService` 인스턴스화하여는 `Intent` 호출한 다음는 [ `StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/) 매개 변수로 해당 의도 메서드. 매개 변수로 의도 서비스에 전달 됩니다는 `OnHandleIntent` 메서드. 이 코드 조각은 프로그램 의도에 작업 요청을 보내는의 예입니다. 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

`IntentService` 이 코드 조각에서와 같이 값을 염두에서 추출할 수 있습니다.  

```csharp
protected override void OnHandleIntent (Android.Content.Intent intent)
{
    string fileToDownload = intent.GetStringExtra("file_to_download");
    
    Log.Debug("DemoIntentService", $"File to download: {fileToDownload}.");
}
```


## <a name="related-links"></a>관련 링크

- [IntentService](https://developer.xamarin.com/api/type/Android.App.IntentService/)
- [StartService](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)
