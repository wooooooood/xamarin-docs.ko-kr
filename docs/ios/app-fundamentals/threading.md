---
title: Xamarin.iOS에서 스레딩
description: 이 문서에서는 Xamarin.iOS 응용 프로그램에서 System.Threading Api를 사용 하는 방법에 설명 합니다. 작업 병렬 라이브러리에서는 응답성이 뛰어난 응용 프로그램 및 가비지 수집 빌드에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 05d015d8d255ccc8c6230b1a89e098e187b22b37
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784919"
---
# <a name="threading-in-xamarinios"></a>Xamarin.iOS에서 스레딩

Xamarin.iOS 런타임에 액세스할 수 개발자는.NET Api 스레드를 사용 하는 경우에 명시적으로 둘 다 스레딩 (`System.Threading.Thread, System.Threading.ThreadPool`)는 전체 범위의 Api를 지 원하는 뿐만 아니라 비동기 대리자 패턴 또는 BeginXXX 메서드를 사용 하는 경우에 암시적으로 및에서 작업 병렬 라이브러리입니다.



Xamarin을 사용 하는 적극 권장는 [작업 병렬 라이브러리](http://msdn.microsoft.com/library/dd460717.aspx) TPL ()는 몇 가지 이유로 응용 프로그램을 빌드하기 위한:
-  기본 TPL 스케줄러에 동적으로 크기가 계속 커집니다 프로세스가 수행, 여기서 CPU 시간에 대 한 경쟁 너무 많은 스레드를 종료 하는 시나리오를 방지 하면서으로 필요한 스레드 수가 스레드 풀에 작업 실행을 위임 합니다. 
-  TPL 작업 측면에서 작업에 대해 생각 하는 것이 쉽습니다. 있습니다 수 쉽게 조작할 예약, 해당 실행을 serialize 또는 풍부한 Api와 동시에 많은 시작 합니다. 
-  새 C# 비동기 언어 확장을 사용 하 여 프로그래밍 하기 위한 기초는 


스레드 풀 느린 크기가 계속 커집니다 필요에 따라 스레드 수는 시스템, 시스템 부하 및 응용 프로그램 호스트에서 사용할 수 있는 CPU 코어 수를 기반으로 합니다. 이 스레드 풀에서 메서드를 호출 하 여 사용할 수 `System.Threading.ThreadPool` 또는 기본값을 사용 하 여 `System.Threading.Tasks.TaskScheduler` (의 일부는 *병렬 프레임 워크*).

일반적으로 개발자는 루프를 실행 하는 기본 UI를 차단 하도록 원하지 및 응답성이 뛰어난 응용 프로그램을 만드는 데 필요한 경우 스레드를 사용 합니다.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>응답성이 뛰어난 응용 프로그램 개발

UI 요소에 대 한 액세스는 응용 프로그램에 대 한 주 루프를 실행 하는 동일한 스레드로 제한 되어야 합니다. 사용 하 여 코드 대기 해야 변경할 주 UI 스레드에서 하려면 [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/type/Foundation.NSObject/), 다음과 같이 합니다.

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

위의 경합 조건 작동이 중단 되는 잠재적으로 응용 프로그램으로 발생 하지 않고 주 스레드의 컨텍스트에서 대리자 내의 코드를 호출 합니다.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>스레딩 및 가비지 수집

실행 과정에서 Objective C 런타임 만들고 개체를 해제 합니다. 스레드의 현재 개체는 "자동 해제" Objective C 런타임은 해당 개체를 해제 하도록 플래그가 지정 하는 경우 `NSAutoReleasePool`합니다. Xamarin.iOS 솔루션이 생성 `NSAutoRelease` 풀에서 모든 스레드는 `System.Threading.ThreadPool` 및 주 스레드에 대 한 합니다. 이 확장에 의해 System.Threading.Tasks 기본 TaskScheduler를 사용 하 여 만든 모든 스레드를 설명 합니다.

사용 하 여 고유한 스레드를 만드는 경우 `System.Threading` 소유한를 제공할 필요가 `NSAutoRelease` 데이터 누출 되지 않도록 방지 하기 위해 풀입니다. 이렇게 하려면 단순히 코드의 다음 부분에 스레드를 래핑하십시오.

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

참고: Xamarin.iOS 5.2 이후 않아도 수 있도록 고유한 `NSAutoReleasePool` 이상 하나는 자동으로 사용자에 게 제공 합니다.


## <a name="related-links"></a>관련 링크

- [UI 스레드 작업](~/ios/user-interface/ios-ui/ui-thread.md)
