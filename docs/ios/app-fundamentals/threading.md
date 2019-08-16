---
title: Xamarin.ios의 스레딩
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 system.string Api를 사용 하는 방법을 설명 합니다. 작업 병렬 라이브러리, 응답성이 뛰어난 응용 프로그램 빌드 및 가비지 수집에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: cbf0aace8806b3b253b523d9ed2001b466ae5237
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526946"
---
# <a name="threading-in-xamarinios"></a>Xamarin.ios의 스레딩

Xamarin.ios 런타임을 통해 개발자는 .net 스레딩 api에 대 한 액세스를 명시적으로 사용 하는 경우 (`System.Threading.Thread, System.Threading.ThreadPool`), 비동기 대리자 패턴 또는 BeginXXX 메서드를 사용 하는 경우 암시적으로, 그리고를 지 원하는 모든 api의 전체 범위를 사용할 수 있습니다. 작업 병렬 라이브러리.



Xamarin은 몇 가지 이유로 응용 프로그램을 빌드하기 위해 TPL ( [작업 병렬 라이브러리](https://msdn.microsoft.com/library/dd460717.aspx) )을 사용 하는 것이 좋습니다.
- 기본 TPL 스케줄러는 스레드 풀에 작업 실행을 위임 합니다. 그러면 프로세스가 수행 될 때까지 필요한 스레드 수가 동적으로 증가 하 고 CPU 시간에 대해 경쟁 하는 스레드가 너무 많이 발생 하는 시나리오는 피할 수 있습니다. 
- TPL 작업 측면에서 작업에 대해 생각 하는 것이 더 쉽습니다. 쉽게 조작 하 고, 일정을 예약 하 고, 실행을 직렬화 하거나, 다양 한 Api를 사용 하 여 여러 가지를 병렬로 실행할 수 있습니다. 
- 새 C# 비동기 언어 확장을 사용 하 여 프로그래밍 하는 데 사용 됩니다. 


스레드 풀은 시스템에서 사용할 수 있는 CPU 코어 수, 시스템 부하 및 응용 프로그램 요구에 따라 필요한 스레드 수를 천천히 증가 시킵니다. 에서 `System.Threading.ThreadPool` 메서드를 호출 하거나 기본 `System.Threading.Tasks.TaskScheduler` ( *병렬 프레임 워크*의 일부)을 사용 하 여이 스레드 풀을 사용할 수 있습니다.

일반적으로 개발자는 응답성이 뛰어난 응용 프로그램을 만들어야 하 고 주 UI 실행 루프를 차단 하지 않으려는 경우 스레드를 사용 합니다.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>응답성이 뛰어난 응용 프로그램 개발

UI 요소에 대 한 액세스는 응용 프로그램에 대 한 기본 루프를 실행 하는 것과 동일한 스레드로 제한 되어야 합니다. 스레드에서 주 UI를 변경 하려면 다음과 같이 [InvokeOnMainThread](xref:Foundation.NSObject)를 사용 하 여 코드를 큐에 대기 시켜야 합니다.

```csharp
MyThreadedRoutine ()  
{  
    var result = DoComputation ();  

    // we want to update an object that is managed by the main
    // thread; To do so, we need to ensure that we only access
    // this from the main thread:

    InvokeOnMainThread (delegate {  
        label.Text = "The result is: " + result;  
    });
}
```

위에서는 응용 프로그램의 작동이 중단 될 수 있는 경합 상태를 일으키지 않고 주 스레드의 컨텍스트에서 대리자 내부의 코드를 호출 합니다.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>스레딩 및 가비지 수집

실행 과정에서 목표 C 런타임은 개체를 만들고 해제 합니다. 개체에 "자동 릴리스" 플래그가 지정 된 경우 목표 C 런타임은 해당 개체를 스레드의 현재 `NSAutoReleasePool`로 해제 합니다. Xamarin.ios는 `NSAutoRelease` `System.Threading.ThreadPool` 주 스레드에 대해 및의 모든 스레드에 대해 하나의 풀을 만듭니다. 이는 TaskScheduler의 기본를 사용 하 여 만든 모든 스레드를 확장에 포함 합니다.

를 사용 하 여 `System.Threading` 사용자 고유의 스레드를 만드는 경우 데이터가 누출 되는 것을 방지 하기 위해 고유한 `NSAutoRelease` 풀을 제공 해야 합니다. 이렇게 하려면 다음 코드 조각으로 스레드를 래핑합니다.

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

참고: Xamarin.ios 5.2부터 사용자가 자동으로 제공 되므로 더 이상 직접 `NSAutoReleasePool` 제공할 필요가 없습니다.


## <a name="related-links"></a>관련 링크

- [UI 스레드 작업](~/ios/user-interface/ios-ui/ui-thread.md)
