---
title: 작업이 있는 iOS Backgrounding
description: 이 문서에서는 백그라운드 작업을 사용 하 여 응용 프로그램이 백그라운드에 배치 된 후 장기 실행 작업을 수행 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: f39ab83e00db1abd6508d26a9280fb708e681445
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915765"
---
# <a name="ios-backgrounding-with-tasks"></a>작업이 있는 iOS Backgrounding

IOS에서 backgrounding를 수행 하는 가장 간단한 방법은 backgrounding 요구 사항을 작업으로 나누고 백그라운드에서 작업을 실행 하는 것입니다. 작업은 엄격 하 게 제한 되며, 일반적으로 응용 프로그램이 iOS 6의 배경으로 이동한 후에는 600 초 (10 분)의 처리 시간과 iOS 7 이상에서 10 분 이내에 발생 합니다.

백그라운드 작업은 다음과 같은 세 가지 범주로 나눌 수 있습니다.

1. **백그라운드 안전 작업** -응용 프로그램이 백그라운드에 들어가기 위해 중단 하지 않으려는 태스크가 있는 응용 프로그램의 모든 위치에서 호출 됩니다.
1. **DidEnterBackground 작업** -정리 및 상태 저장을 지원 하기 위해 `DidEnterBackground` 응용 프로그램 수명 주기 메서드를 실행 하는 동안 호출 됩니다.
1. **백그라운드 전송 (ios 7 이상)** -ios 7에서 네트워크 전송을 수행 하는 데 사용 되는 특수 한 유형의 백그라운드 작업입니다. 일반 작업과 달리 백그라운드 전송에는 미리 결정 된 시간 제한이 없습니다.

백그라운드 안전 및 `DidEnterBackground` 작업은 몇 가지 사소한 차이점이 있지만 iOS 6 및 iOS 7 모두에서 사용 하기에 안전 합니다. 이러한 두 가지 유형의 작업을 더 자세히 살펴보겠습니다.

## <a name="creating-background-safe-tasks"></a>백그라운드 안전 작업 만들기

일부 응용 프로그램에는 응용 프로그램의 상태가 변경 될 때까지 iOS에서 중단 하지 않아야 하는 태스크가 포함 되어 있습니다. 이러한 작업을 중단 하지 않도록 보호 하는 한 가지 방법은 장기 실행 작업으로 iOS에 등록 하는 것입니다. 사용자가 앱을 백그라운드에 배치할 때 작업이 중단 되지 않도록 하려면 응용 프로그램의 어디에서 나이 패턴을 사용할 수 있습니다. 이 패턴의 좋은 방법은 새 사용자의 등록 정보를 서버에 전송 하거나 로그인 정보를 확인 하는 것과 같은 작업입니다.

다음 코드 조각에서는 백그라운드에서 실행 되도록 작업을 등록 하는 방법을 보여 줍니다.

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

등록 프로세스는 `taskID`고유 식별자로 작업을 쌍으로 연결한 다음 일치 하는 `BeginBackgroundTask` 및 `EndBackgroundTask` 호출로 래핑합니다. 식별자를 생성 하려면 `UIApplication` 개체에 대 한 `BeginBackgroundTask` 메서드를 호출한 다음 일반적으로 새 스레드에서 장기 실행 작업을 시작 합니다. 작업이 완료 되 면 `EndBackgroundTask`를 호출 하 고 동일한 식별자를 전달 합니다. 이는 `BeginBackgroundTask` 호출에 일치 하는 `EndBackgroundTask`없는 경우 iOS에서 응용 프로그램을 종료 하기 때문에 중요 합니다.

> [!IMPORTANT]
> 백그라운드 안전 작업은 응용 프로그램의 요구 사항에 따라 주 스레드나 백그라운드 스레드에서 실행 될 수 있습니다.

## <a name="performing-tasks-during-didenterbackground"></a>DidEnterBackground 중 작업 수행

장기 실행 작업을 백그라운드에서 안전 하 게 만드는 것 외에도, 응용 프로그램을 백그라운드에 배치 하는 동안 등록을 사용 하 여 작업을 시작할 수 있습니다. iOS는 응용 프로그램이 백그라운드에 들어가기 전에 응용 프로그램 상태를 저장 하 고, 사용자 데이터를 저장 하 고, 중요 한 콘텐츠를 암호화 하는 데 사용할 수 있는 `DidEnterBackground` 이라는 *AppDelegate* 클래스의 이벤트 메서드를 제공 합니다. 응용 프로그램은이 메서드에서 반환 하는 데 약 5 초를 포함 하거나 종료 됩니다. 따라서 완료 하는 데 5 초 이상이 소요 될 수 있는 정리 작업을 `DidEnterBackground` 메서드 내에서 호출할 수 있습니다. 이러한 작업은 별도의 스레드에서 호출 해야 합니다.

프로세스는 장기 실행 작업을 등록 하는 프로세스와 거의 동일 합니다. 다음 코드 조각에서는이 작업을 수행 하는 방법을 보여 줍니다.

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

이전 예제에서와 같이 `BeginBackgroundTask`를 통해 작업을 등록 하는 `AppDelegate`에서 `DidEnterBackground` 메서드를 재정의 하는 것부터 시작 합니다. 다음으로 새 스레드를 생성 하 고 장기 실행 작업을 수행 합니다. 이제 `DidEnterBackground` 메서드가 이미를 반환 했으므로 장기 실행 작업 내에서 `EndBackgroundTask` 호출이 수행 됩니다.

> [!IMPORTANT]
> iOS는 [감시 메커니즘](https://developer.apple.com/library/ios/qa/qa1693/_index.html) 을 사용 하 여 응용 프로그램의 UI가 응답성을 유지 하도록 합니다. `DidEnterBackground` 너무 많은 시간을 소비 하는 응용 프로그램은 UI에서 응답 하지 않게 됩니다. 백그라운드에서 실행 하는 시작 off 작업을 사용 `DidEnterBackground` 하면 적절 한 시간 내에를 반환 하 여 UI 응답성을 유지 하 고 watchdog이 응용 프로그램을 중단 하지 않도록 방지할 수 있습니다.

## <a name="handling-background-task-time-limits"></a>백그라운드 작업 시간 제한 처리

iOS는 백그라운드 작업을 실행할 수 있는 기간에 대해 엄격한 제한을 설정 하 고, 할당 된 시간 내에 `EndBackgroundTask` 호출이 수행 되지 않으면 응용 프로그램이 종료 됩니다. 남은 backgrounding 시간을 추적 하 고 필요한 경우 만료 처리기를 사용 하 여 iOS 응용 프로그램 종료를 방지할 수 있습니다.

### <a name="accessing-background-time-remaining"></a>남은 백그라운드 시간 액세스

등록 된 작업이 있는 응용 프로그램을 백그라운드로 이동 하면 등록 된 작업을 실행 하는 데 600 초 정도 걸립니다. `UIApplication` 클래스의 정적 `BackgroundTimeRemaining` 속성을 사용 하 여 작업이 완료 되어야 하는 시간을 확인할 수 있습니다. 다음 코드는 백그라운드 작업에서 남은 시간 (초)을 제공 합니다.

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>만료 처리기를 사용 하 여 앱 종료 방지

IOS는 `BackgroundTimeRemaining` 속성에 대 한 액세스를 제공 하는 것 외에도 **만료 처리기**를 통해 백그라운드 시간 만료를 처리 하는 정상적인 방법을 제공 합니다. 작업에 할당 된 시간이 만료 될 때 실행 되는 선택적 코드 블록입니다. 만료 처리기의 코드는 `EndBackgroundTask`를 호출 하 고 작업 ID를 전달 합니다 .이는 앱이 정상적으로 작동 하 고 작업에 시간이 오래 걸리는 경우에도 iOS에서 앱을 종료 하지 못하도록 합니다. `EndBackgroundTask`은 만료 처리기 내에서 호출 해야 하며 일반적인 실행 과정에서 호출 해야 합니다. 

만료 처리기는 아래 그림과 같이 람다 식을 사용 하 여 익명 함수로 표현 됩니다.

```csharp
Task.Factory.StartNew( () => {

    //expirationHandler only called if background time allowed exceeded
    var taskId = UIApplication.SharedApplication.BeginBackgroundTask(() => {
        Console.WriteLine("Exhausted time");
        UIApplication.SharedApplication.EndBackgroundTask(taskId); 
    });
    while(myFlag == true)
    {
        Console.WriteLine(UIApplication.SharedApplication.BackgroundTimeRemaining);
        myFlag = SomeCalculationNeedsMoreTime();
    }
    //Only called if loop terminated due to myFlag and not expiration of time
    UIApplication.SharedApplication.EndBackgroundTask(taskId);
});
```

만료 처리기가 코드를 실행 하는 데 필요 하지는 않지만 항상 백그라운드 작업과 함께 만료 처리기를 사용 해야 합니다.

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>IOS 7 이상의 백그라운드 작업

백그라운드 작업과 관련 하 여 iOS 7의 가장 큰 변화는 작업의 구현 방법이 아니라 실행 되는 경우입니다.

IOS 7 이전에는 백그라운드에서 실행 되는 작업을 완료 하는 데 600 초가 있었습니다. 이 제한의 한 가지 이유는 백그라운드에서 실행 중인 작업이 작업 기간 동안 장치를 절전 모드에서 해제 하는 것입니다.

 [![](ios-backgrounding-with-tasks-images/ios6.png "Graph of the task keeping the app awake pre-iOS 7")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

iOS 7 백그라운드 처리는 배터리 수명이 긴 경우에 최적화 되어 있습니다. IOS 7에서 backgrounding는 장치를 활성 상태로 유지 하는 대신 장치를 절전 모드에서 해제 하는 것이 아니라, 장치가 전화 통화, 알림, 수신 메일 등을 처리 하기 위해 절전 모드를 해제 하는 경우에는 청크를 대신 처리 합니다. 일반적인 중단. 다음 다이어그램은 태스크가 분할 되는 방법에 대 한 통찰력을 제공 합니다.

 [![](ios-backgrounding-with-tasks-images/ios7.png "Graph of the task being broken into chunks post-iOS 7")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

작업 실행 시간이 더 이상 연속 되지 않으므로 iOS 7에서 네트워크 전송을 수행 하는 작업을 다르게 처리 해야 합니다. 개발자는 `NSURlSession` API를 사용 하 여 네트워크 전송을 처리 하는 것이 좋습니다. 다음 섹션은 백그라운드 전송에 대 한 개요입니다.

 <a name="background-transfers" />

## <a name="background-transfers"></a>백그라운드 전송

IOS 7의 백그라운드 전송 백본은 새로운 `NSURLSession` API입니다. `NSURLSession`를 사용 하 여 다음 작업을 만들 수 있습니다.

1. 네트워크 및 장치 중단을 통해 콘텐츠를 전송 합니다.
1. 대량 파일 ( *백그라운드 전송 서비스* )을 업로드 하 고 다운로드 합니다.

이것이 어떻게 작동 하는지 자세히 살펴보겠습니다.

### <a name="nsurlsession-api"></a>NSURLSession API

 `NSURLSession`는 네트워크를 통해 콘텐츠를 전송 하기 위한 강력한 API입니다. 네트워크 중단 및 응용 프로그램 상태 변경을 통해 데이터 전송을 처리 하는 일련의 도구를 제공 합니다.

`NSURLSession` API는 하나 이상의 세션을 만들고, 네트워크를 통해 관련 데이터의 블록을 셔틀 하는 작업을 생성 합니다. 작업은 비동기적으로 실행 되어 데이터를 빠르고 안정적으로 전송 합니다. `NSURLSession`는 비동기 이기 때문에 모든 세션에는 전송이 완료 될 때 시스템 및 응용 프로그램에서 알 수 있도록 완료 처리기 블록이 필요 합니다.

사전 iOS 7 및 사후 iOS 7 모두에서 유효한 네트워크 전송을 수행 하려면 `NSURLSession`을 큐에서 전송할 수 있는지 확인 하 고 일반 백그라운드 작업을 사용 하 여 다음이 아닌 경우 전송을 수행 합니다.

```csharp
if ([NSURLSession class]) {
  // Create a background session and enqueue transfers
}
else {
  // Start a background task and transfer directly
  // Do NOT make calls to update the UI here!
}
```

> [!IMPORTANT]
> IOS 6은 백그라운드 UI 업데이트를 지원 하지 않고 응용 프로그램을 종료 하므로 iOS 6 규격 코드의 백그라운드에서 UI를 업데이트 하는 호출을 피합니다.

`NSURLSession` API에는 인증을 처리 하 고, 실패 한 전송을 관리 하 고, 클라이언트 쪽 오류를 보고 하는 풍부한 기능 집합이 포함 되어 있습니다. IOS 7에 도입 된 작업 실행 시간의 중단을 연결 하는 데 도움이 되며, 대량 파일을 빠르고 안정적으로 전송할 수 있는 지원도 제공 합니다. 다음 섹션에서는이 두 번째 기능을 살펴봅니다.

### <a name="background-transfer-service"></a>백그라운드 전송 서비스

IOS 7 이전에는 백그라운드에서 파일을 업로드 하거나 다운로드 하는 것은 신뢰할 수 없습니다. 백그라운드 작업을 실행 하는 데 시간이 제한 되지만 파일을 전송 하는 데 걸리는 시간은 네트워크와 파일 크기에 따라 달라 집니다. IOS 7에서는 `NSURLSession`를 사용 하 여 파일을 성공적으로 업로드 하 고 다운로드할 수 있습니다. 백그라운드에서 많은 파일의 네트워크 전송을 처리 하는 특정 `NSURLSession` 세션 유형을 *백그라운드 전송 서비스*라고 합니다.

백그라운드 전송 서비스를 사용 하 여 시작 된 전송은 운영 체제에서 관리 하며 인증 및 오류를 처리 하는 Api를 제공 합니다. 전송은 임의 시간 제한에 의해 바인딩되지 않으므로 대량 파일을 업로드 또는 다운로드 하 고 백그라운드에서 콘텐츠를 자동으로 업데이트 하는 데 사용할 수 있습니다. 서비스를 구현 하는 방법에 대 한 자세한 내용은 [백그라운드 전송 연습](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md) 을 참조 하세요.

백그라운드 전송 서비스는 응용 프로그램이 백그라운드에서 콘텐츠를 새로 고칠 수 있도록 백그라운드 가져오기 또는 원격 알림과 쌍을 이룹니다. 다음 두 섹션에서는 iOS 6 및 iOS 7 모두에서 백그라운드에서 실행 되도록 전체 응용 프로그램을 등록 하는 개념을 소개 합니다.
