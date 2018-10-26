---
title: Xamarin.Android에서 인 텐트 서비스
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 1301f34ad1f7a0069c542ba81bf237a673fd239d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112860"
---
# <a name="intent-services-in-xamarinandroid"></a>Xamarin.Android에서 인 텐트 서비스

## <a name="intent-services-overview"></a>인 텐트 서비스 개요

둘 다 시작 하 고 작업을 비동기적으로 수행할 수 해야 서비스를 유지 하기 위해 성능 부드러운 함을 의미 하는 주 스레드에서 실행 하는 서비스를 바인딩해야 합니다. 된 이러한 문제를 해결 하는 가장 간단한 방법 중 하나를 _작업자 큐 프로세서 패턴_단일 스레드 서비스를 제공 하는 큐에서 수행 되어야 하는 작업 위치, 합니다. 

합니다 [ `IntentService` ](https://developer.xamarin.com/api/type/Android.App.IntentService/) 의 서브 클래스는 `Service` 이 패턴의 Android 특정 구현을 제공 하는 클래스입니다. 작업자 스레드는 큐 서비스를 시작 하는 큐 작업을 관리할 및 끌어오기 요청을 작업자 스레드에서 실행 되도록 큐를 해제 합니다. `IntentService` 조용히 자기 자신을 중지 되 고 큐에 더 이상 작업이 있을 때 작업자 스레드를 제거 합니다.
 
작업을 만들어 큐로 전송 되는 `Intent` 전달 하는 `Intent` 에 `StartService` 메서드.

중지 하거나 중단할 수 없는 합니다 `OnHandleIntent` 메서드 `IntentService` 작동 하는 동안. 이 설계로 인해 프로그램 `IntentService` 상태 비저장 유지할지 &ndash; 활성 연결 또는 응용 프로그램의 나머지 부분에서 통신에 의존 하지 않아야 합니다. `IntentService` statelessly 작업 요청을 처리할 것입니다.

서브클래싱 하기 위한 두 가지 요구 사항이 `IntentService`:

1. 새 형식 (하위 클래스를 만들어 `IntentService`)만 재정의 `OnHandleIntent` 메서드.
2. 새 형식에 대 한 생성자를 사용 하려면 요청을 처리 하는 작업자 스레드 이름을 지정 하는 데 사용 되는 문자열입니다. 이 작업자 스레드의 이름은 응용 프로그램을 디버깅 하는 경우에 주로 사용 됩니다.

다음 코드는 `IntentService` 재정의 된 구현을 `OnHandleIntent` 메서드:

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

로 작업이 전달 됩니다는 `IntentService` 인스턴스화하여를 `Intent` 호출한 합니다 [ `StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/) 매개 변수로 해당 의도 사용 하 여 메서드. 매개 변수로 서비스에 전달할는 의도 `OnHandleIntent` 메서드. 이 코드 조각은 다음과 같습니다. 의도 작업 요청을 보내기의 예 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

`IntentService` 이 코드 조각에서 볼 수 있듯이 의도에서 값을 추출할 수 있습니다.  

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
