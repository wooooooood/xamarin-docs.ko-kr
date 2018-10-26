---
title: Xamarin.iOS에서 스레딩
description: 이 문서에서는 Xamarin.iOS 응용 프로그램에서 System.Threading Api를 사용 하는 방법을 설명 합니다. 설명 하는 작업 병렬 라이브러리, 응답성이 뛰어난 응용 프로그램 및 가비지 컬렉션을 작성 합니다.
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: b90c59f09217077262c3aced9ee9e5d07849c25c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106587"
---
# <a name="threading-in-xamarinios"></a>Xamarin.iOS에서 스레딩

Xamarin.iOS 런타임은 개발자에 액세스할.NET 스레딩 스레드를 사용 하는 경우에 명시적으로 둘 다 Api (`System.Threading.Thread, System.Threading.ThreadPool`)는 전체 범위의 Api를 지 원하는 뿐만 아니라 비동기 대리자 패턴 또는 BeginXXX 메서드를 사용 하는 경우에 암시적으로 및를 작업 병렬 라이브러리입니다.



Xamarin을 사용 하는 적극 권장 합니다 [Task Parallel Library](http://msdn.microsoft.com/library/dd460717.aspx) (TPL) 몇 가지 이유로 응용 프로그램을 빌드하기 위한:
-  기본 TPL 스케줄러에 동적으로 확장 프로세스는에 있는 CPU 시간에 대 한 경쟁 너무 많은 스레드를 종료 하는 시나리오를 방지 하는 동안 필요한 스레드 수가 스레드 풀에 작업 실행을 위임 합니다. 
-  TPL 작업와 관련 된 작업에 대해 생각 하는 것이 쉽습니다. 수 쉽게 조작할 예약한, 해당 실행을 직렬화 또는 다양 한 Api 사용 하 여 병렬로 여러 시작 합니다. 
-  새 C# 비동기 언어 확장을 사용 하 여 프로그래밍을 위한 기초가 됩니다. 


스레드 풀 느리게 증가 필요에 따라 스레드 수가 시스템, 시스템 부하 및 응용 프로그램 요구 사항에 맞는에서 사용할 수 있는 CPU 코어 수를 기준으로 합니다. 사용할 수 있습니다이 스레드 풀에서 메서드를 호출 하 여 `System.Threading.ThreadPool` 또는 기본값을 사용 하 여 `System.Threading.Tasks.TaskScheduler` (부분 합니다 *Parallel Frameworks*).

일반적으로 개발자는 응답성이 뛰어난 응용 프로그램을 만드는 데 필요한 기본 UI 루프 실행을 차단 하도록 않으려는 경우 스레드를 사용 합니다.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>응답성이 뛰어난 응용 프로그램 개발

UI 요소에 대 한 액세스는 응용 프로그램에 대 한 기본 루프를 실행 하는 동일한 스레드로 제한 되어야 합니다. 사용 하 여 코드를 큐 해야 변경 주 UI 스레드에서 않으려면 [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/type/Foundation.NSObject/), 다음과 같은:

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

위의 응용 프로그램를 잠재적으로 충돌할 수 있는 경합 없이 주 스레드나 컨텍스트에서 대리자 내에서 코드를 호출 합니다.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>스레딩 및 가비지 수집

실행 하는 과정에서 Objective-c 런타임에서 만들고 개체를 해제 합니다. 스레드의 현재 개체는 "자동-릴리스의" Objective-c 런타임에서 해당 개체에 대 한 플래그가 지정 된 경우 `NSAutoReleasePool`합니다. Xamarin.iOS를 만듭니다 `NSAutoRelease` 풀에서 모든 스레드는 `System.Threading.ThreadPool` 및 주 스레드에 대 한 합니다. 이 확장에 의해 System.Threading.Tasks 기본 TaskScheduler를 사용 하 여 만든 모든 스레드를 설명 합니다.

사용 하 여 고유한 스레드를 만드는 경우 `System.Threading` 자신이 제공 필요가 `NSAutoRelease` 데이터 누출 되지 않도록 방지 하기 위해 풀 합니다. 이렇게 하려면 단순히 다음 부분 코드에서에서 스레드를 래핑하십시오.

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

참고: Xamarin.iOS 5.2 이후 필요가 없습니다 수 있도록 고유한 `NSAutoReleasePool` 하나는 자동으로 사용자에 게 제공 하는 대로 더 이상.


## <a name="related-links"></a>관련 링크

- [UI 스레드 작업](~/ios/user-interface/ios-ui/ui-thread.md)
