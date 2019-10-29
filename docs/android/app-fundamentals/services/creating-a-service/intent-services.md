---
title: Xamarin Android의 의도 서비스
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: c58787a051bfc965cb7493138ed6114ac23ed04d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024841"
---
# <a name="intent-services-in-xamarinandroid"></a>Xamarin Android의 의도 서비스

## <a name="intent-services-overview"></a>의도 서비스 개요

시작 된 서비스와 바인딩된 서비스는 모두 주 스레드에서 실행 됩니다. 즉, 성능을 원활 하 게 유지 하려면 서비스에서 비동기적으로 작업을 수행 해야 합니다. 이러한 문제를 해결 하는 가장 간단한 방법 중 하나는 _작업자 큐 프로세서 패턴_을 사용 하는 것입니다. 여기에서 수행할 작업은 단일 스레드가 처리 하는 큐에 배치 됩니다.

[`IntentService`](xref:Android.App.IntentService) 은이 패턴의 Android 관련 구현을 제공 하는 `Service` 클래스의 서브 클래스입니다. 큐 작업을 관리 하 고, 큐를 서비스 하는 작업자 스레드를 시작 하 고, 작업자 스레드에서 실행할 요청을 끌어오기 합니다. 큐에 작업이 더 이상 없는 경우 `IntentService` 자동으로 중지 하 고 작업자 스레드를 제거 합니다.

작업은 `Intent` 만든 다음 해당 `Intent`를 `StartService` 메서드에 전달 하 여 큐에 전송 됩니다.

작업 하는 동안에는 `OnHandleIntent` 메서드 `IntentService`를 중지 하거나 중단할 수 없습니다. 이 디자인으로 인해 `IntentService`는 상태 비저장 &ndash; 유지 되어야 하며, 응용 프로그램의 나머지 부분 으로부터의 통신 또는 활성 연결을 사용 하지 않아야 합니다. `IntentService`는 작업 요청을 statelessly 처리 하기 위한 것입니다.

`IntentService`를 서브클래싱 하기 위한 두 가지 요구 사항이 있습니다.

1. 새 형식 (`IntentService`하위 클래스에서 생성)은 `OnHandleIntent` 메서드만 재정의 합니다.
2. 새 형식에 대 한 생성자에는 요청을 처리 하는 작업자 스레드의 이름을 결정 하는 데 사용 되는 문자열이 필요 합니다. 이 작업자 스레드의 이름은 응용 프로그램을 디버그할 때 주로 사용 됩니다.

다음 코드에서는 재정의 된 `OnHandleIntent` 메서드를 사용 하 여 `IntentService` 구현을 보여 줍니다.

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

작업은 `Intent`을 인스턴스화한 다음 해당 의도를 매개 변수로 사용 하 여 [`StartService`](xref:Android.Content.Context.StartService*) 메서드를 호출 하 여 `IntentService` 전송 됩니다. 의도는 `OnHandleIntent` 메서드에서 매개 변수로 서비스에 전달 됩니다. 이 코드 조각은 의도 한 대로 작업 요청을 보내는 예제입니다. 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

이 코드 조각에 나와 있는 것 처럼 `IntentService`는 의도에서 값을 추출할 수 있습니다.  

```csharp
protected override void OnHandleIntent (Android.Content.Intent intent)
{
    string fileToDownload = intent.GetStringExtra("file_to_download");

    Log.Debug("DemoIntentService", $"File to download: {fileToDownload}.");
}
```

## <a name="related-links"></a>관련 링크

- [IntentService](xref:Android.App.IntentService)
- [StartService](xref:Android.Content.Context.StartService*)
