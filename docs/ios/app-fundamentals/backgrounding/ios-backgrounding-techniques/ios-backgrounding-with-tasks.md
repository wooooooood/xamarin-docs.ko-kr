---
title: iOS Backgrounding 작업
description: 이 문서에는 백그라운드 태스크를 사용 하 여는 응용 프로그램이 백그라운드에서 배치 되 면 장기 실행 작업을 수행 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: c8d1abebf6dec2b7b5fe76d57ff851fad457f2a8
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669832"
---
# <a name="ios-backgrounding-with-tasks"></a>iOS Backgrounding 작업

IOS의 backgrounding 수행 하는 가장 간단한 방법은 backgrounding 요구 작업을 중단 하 고 백그라운드에서 작업을 실행 하는 경우 작업에서 엄격한 시간 제한이 되며 일반적으로 가져오고 약 600 초 (10 분) 응용 프로그램이 iOS 6, 백그라운드로 이동 후 처리 시간이 10 분 이상 iOS 7 이상에서.

백그라운드 작업 세 가지 범주로 나눌 수 있습니다.

1.  **백그라운드 스레드로부터 안전한 작업** 호출-어디서 나 작업 해야 하는 응용 프로그램에서 원하지 중단 된 응용 프로그램이 백그라운드를 입력 해야 합니다.
1.  **DidEnterBackground 태스크** 하는 동안 호출-는 `DidEnterBackground` 정리 및 상태 저장 하는 데 도움이 되는 응용 프로그램 수명 주기 메서드.
1.  **전송 (iOS 7 이상)을 백그라운드** -iOS 7에서 네트워크 전송을 수행 하는 특수 한 유형의 백그라운드 작업을 사용 합니다. 일반 작업과 달리 백그라운드 전송은 미리 결정된 된 제한 시간을 갖지 않습니다.


백그라운드 스레드로부터 안전 하 고 `DidEnterBackground` 작업은 iOS 6 및 iOS 7, 몇 가지 사소한 차이점이 있지만 둘 다에서 사용 하기에 안전 합니다. 이러한 두 종류의 작업을 더 자세히 알아보겠습니다.

## <a name="creating-background-safe-tasks"></a>백그라운드 스레드로부터 안전한 작업 만들기

일부 응용 프로그램 응용 프로그램 상태를 변경 해야 iOS에 의해 중단 하지 않아야 하는 작업을 포함 합니다. 중단 으로부터 이러한 작업을 보호 하는 한 가지 방법은 장기 실행 작업으로 iOS를 사용 하 여 하를 등록 하는 것입니다. 사용할 수 있습니다이 패턴 어디서 나 응용 프로그램에서 원하지 않는 중단 되 고 작업을 백그라운드로 사용자 put 앱을 해야 합니다. 이 패턴에 대 한 좋은 후보 작업 같은 서버에 새 사용자의 등록 정보를 보내거나 로그인 정보를 확인 하는 것입니다.

다음 코드는 백그라운드에서 실행 하는 작업을 등록 하는 방법을 보여 줍니다.

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

등록 프로세스를 고유 식별자를 사용 하 여 작업 쌍 `taskID`, 하 고 일치에 래핑되어 `BeginBackgroundTask` 및 `EndBackgroundTask` 호출 합니다. 식별자를 생성 하려면에 대 한 호출을 확인 했습니다 합니다 `BeginBackgroundTask` 메서드는 `UIApplication` 개체, 한 다음 새 스레드에서 장기 실행 작업을 일반적으로 시작 합니다. 호출 작업이 완료 되 면 `EndBackgroundTask` 동일한 식별자를 전달 합니다. 왜냐하면 중요 한 경우 iOS 응용 프로그램을 종료 합니다는 `BeginBackgroundTask` 호출 되지 않은 일치 하는 `EndBackgroundTask`합니다.

> [!IMPORTANT]
> 백그라운드 스레드로부터 안전한 작업 주 스레드나 응용 프로그램의 요구에 따라 백그라운드 스레드에서 실행할 수 있습니다.


## <a name="performing-tasks-during-didenterbackground"></a>DidEnterBackground 하는 동안 작업을 수행합니다.

장기 실행 작업을 백그라운드 스레드로부터 안전한 작업을 수행 하는 것 외에도 응용 프로그램이 백그라운드에 배치 되는 대로 작업을 시작 하려면 등록을 사용할 수 있습니다. iOS에서 이벤트 메서드를 제공 합니다 *AppDelegate* 라는 클래스 `DidEnterBackground` , 응용 프로그램 상태를 저장 하 고, 사용자 데이터를 저장 하 고, 응용 프로그램이 백그라운드에 들어가기 전에 중요 한 콘텐츠를 암호화를 사용할 수 있는 합니다. 응용 프로그램은이 메서드에서 반환할 약 5 초 또는 종료 됩니다. 완료 하는 데 5 초 이상 소요 되는 정리 작업에서 호출할 수 있습니다 따라서 내는 `DidEnterBackground` 메서드. 이러한 작업을 별도 스레드에서 호출 해야 합니다.

프로세스는 장기 실행 작업을 등록 하는 거의 동일 합니다. 다음 코드 조각에서는 작업에서이 보여 줍니다.

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

재정의 하 여 시작 합니다 `DidEnterBackground` 에서 메서드를 `AppDelegate`필요한 작업을 통해 등록 하는 경우, `BeginBackgroundTask` 앞의 예제에서 수행한 것 처럼. 그런 다음 새 스레드를 생성 하 고 우리의 장기 실행 작업을 수행 합니다. 합니다 `EndBackgroundTask` 이제 호출에서 장기 실행 태스크 내에서 이후에 `DidEnterBackground` 메서드가 이미 반환 했습니다.

> [!IMPORTANT]
> iOS를 사용 하는 [메커니즘 watchdog](https://developer.apple.com/library/ios/qa/qa1693/_index.html) 응용 프로그램의 UI 응답성이 유지 되도록 합니다. 응용 프로그램에서 너무 많은 시간을 `DidEnterBackground` UI에 응답 하지 않게 됩니다. 허용 작업이 백그라운드에서 실행 될 때까지 정기적인 `DidEnterBackground` UI 응답성을 유지 하 고 응용 프로그램을 종료에서 watchdog를 방지, 적절 한 시기에 반환 합니다.


## <a name="handling-background-task-time-limits"></a>처리 백그라운드 작업 시간 제한

iOS에서 엄격한 제한을 장기 백그라운드 작업을 실행 하는 방법 및 경우 배치를 `EndBackgroundTask` 할당된 된 시간 내에서 호출 되지 않으면, 응용 프로그램이 종료 됩니다. 추적 하 여 남은 시간 backgrounding 및 필요한 경우 만료 처리기를 사용 하 여,에서는 iOS 응용 프로그램 종료를 방지할 수 있습니다.

### <a name="accessing-background-time-remaining"></a>남은 백그라운드 시간에 액세스

등록 된 태스크를 사용 하 여 응용 프로그램을 가져옵니다 백그라운드로 이동 하는 경우 등록 된 태스크를 실행 하려면 약 600 초를 받습니다. 정적을 사용 하 여 완료 하는 작업에 걸리는 시간을 확인할 수 있습니다 `BackgroundTimeRemaining` 의 속성을 `UIApplication` 클래스입니다. 다음 코드는 백그라운드 태스크를 떠난 초 단위로 시간을 얻게 됩니다.

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>만료 처리기를 사용 하 여 응용 프로그램 종료를 방지합니다.

에 대 한 액세스를 제공 하는 것 외에도 합니다 `BackgroundTimeRemaining` 속성을 iOS 백그라운드 시간 만료를 처리 하는 적절 한 방법을 제공 합니다는 **만료 처리기**합니다. 이 작업에 할당 된 시간이 만료 되려고 할 때 실행 됩니다 하는 코드는 선택적 블록입니다. 만료 처리기의 코드 호출 `EndBackgroundTask` 앱이 잘 동작 하 고 iOS 작업 시간이 초과 될 경우에 앱을 종료 하는 것을 금지 의미 작업 ID 전달 합니다. `EndBackgroundTask` 만료 처리기 내에서 뿐만 아니라 정상적인 실행 중에 호출 되어야 합니다. 

만료 처리기는 아래와 같이 람다 식을 사용 하 여 익명 함수로 표시 됩니다.

```csharp
Task.Factory.StartNew( () => {

    //expirationHandler only called if background time allowed exceeded
    var taskId = UIApplication.SharedApplication.BeginBackgroundTask(() => {
        Console.WriteLine("Exhausted time");
        UIApplication.SharedApplication.EndBackgroundTask(taskId); 
    });
    while(myFlag == true)
    {
        Console.WriteLine(UIApplication.SharedApplication.TimeRemaining);
        myFlag = SomeCalculationNeedsMoreTime();
    }
    //Only called if loop terminated due to myFlag and not expiration of time
    UIApplication.SharedApplication.EndBackgroundTask(taskId);
});
```

만료 처리기 코드를 실행 하려면 필요 하지 않은 항상 백그라운드 작업을 사용 하 여 만료 처리기를 사용 해야 합니다.

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>IOS 7 이상에서 백그라운드 작업

백그라운드 작업와 관련 하 여 iOS 7에서에서 가장 크게 변화 된 작업을 구현 하는 방법이 되지 않지만 실행 때입니다.

사전 iOS 7, 백그라운드에서 실행 중인 태스크 해야 600 초 완료 사실을 기억 하십시오. 이 제한에 대 한 이유는 백그라운드에서 실행 중인 태스크를 보관할 때 장치가 절전 모드 해제 작업 기간에 대 한입니다.

 [![](ios-backgrounding-with-tasks-images/ios6.png "앱 절전 모드 해제 사전 iOS 7을 유지 하는 작업의 그래프")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

iOS 7 백그라운드 처리는 더 긴 배터리 수명을 위해 최적화 됩니다. IOS 7, backgrounding 됩니다 편의적: 장치를 절전 모드 해제 상태로 유지 하는 대신 작업 준수 절전 대신 전화 통화, 알림, 들어오는 전자 메일 및 기타 처리에 장치가 절전 때 청크에서 처리를 수행 하는 장치가 작동 하는 경우 일반적인 중단 합니다. 다음 다이어그램에서는 작업 손상 될 수 있습니다 하는 방법에 대 한 정보를 합니다.

 [![](ios-backgrounding-with-tasks-images/ios7.png "청크 후 iOS 7으로 구분 되는 작업의 그래프")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

태스크 실행이 시간을 더 이상 연속 아니므로 작업 네트워크 전송을 수행 하는 iOS 7에서에서 다르게 처리 되어야 합니다. 개발자가 사용 하는 것이 좋습니다는 `NSURlSession` 네트워크 전송을 처리 하는 API입니다. 다음 섹션은 백그라운드 전송의 개요입니다.

 <a name="background-transfers" />

## <a name="background-transfers"></a>백그라운드 전송

IOS 7에서에서 백그라운드 전송의 핵심은 새 `NSURLSession` API. `NSURLSession` 이 기능을 사용 하면 작업을 만들 수 있습니다.

1.  네트워크 및 장치 중단을 통해 콘텐츠를 전송 합니다.
1.  큰 파일 업로드 및 다운로드 ( *백그라운드 전송 서비스* ).


이 수식이 어떻게 좀 더 자세히 살펴보겠습니다.

### <a name="nsurlsession-api"></a>NSURLSession API

 `NSURLSession` 네트워크를 통해 콘텐츠를 전송 하기 위한 강력한 API입니다. 네트워크 중단 및 응용 프로그램 상태 변화를 통해 데이터 전송을 처리 하는 도구 집합을 제공 합니다.

`NSURLSession` API는 네트워크를 통해 관련 된 데이터의 블록 셔틀에 작업을 다시 생성 하는 하나 또는 여러 개의 세션을 만듭니다. 빠르고 안정적으로 데이터를 전송 작업을 비동기적으로 실행 합니다. 때문에 `NSURLSession` 는 비동기적 이며 모든 세션 필요는 완료 처리기 블록 시스템 및 응용 프로그램에는 전송이 완료 될 때 알립니다 수 있도록 합니다.

사전 iOS 7 및 후 iOS 7에서 유효한 네트워크 전송을 수행 하는 경우를 확인는 `NSURLSession` 일반 백그라운드 작업이 없으면 전송 수행 하는 데 및 큐에 넣기 전송을 사용할 수 있습니다.

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
> IOS 6 백그라운드 UI 업데이트를 지원 하지 않으며 응용 프로그램을 종료 하는 대로 iOS 6 규격 코드를 백그라운드에서 UI를 업데이트 하는 호출 하지 마세요.


`NSURLSession` API는 다양 한 인증을 처리 하 고 실패 한 전송을 관리 하지 서버 쪽--하지만 클라이언트 쪽 오류를 보고 하는 기능을 포함 합니다. 작업에서 중단 iOS 7에서에서 도입 된 시간을 실행 하는 브리지를 통해 하 고 빠르고 안정적으로 대용량 파일을 전송 하는 것에 대 한 지원 기능을 제공 합니다. 다음 섹션에서는이 두 번째 기능을 살펴봅니다.

### <a name="background-transfer-service"></a>백그라운드 전송 서비스

IOS 7, 전에 백그라운드에서 파일 업로드 또는 다운로드에서 신뢰할 수 없습니다. 백그라운드 작업을 실행 하려면 제한 된 시간을 가져오지만, 네트워크 및 파일의 크기를 사용 하 여 파일을 전송 하는 데 걸리는 시간이 달라 집니다. Ios 7에서 사용할 수 있습니다는 `NSURLSession` 성공적으로 업로드 하 고 큰 파일을 다운로드 합니다. 특정 `NSURLSession` 백그라운드에서 대용량 파일의 네트워크 전송을 처리 하는 세션 유형 이라고 합니다 *백그라운드 전송 서비스*합니다.

백그라운드 전송 서비스를 사용 하 여 시작 하는 전송 운영 체제에서 관리 하 고 인증 및 오류를 처리 하는 Api를 제공 합니다. 전송은 임의의 시간 제한으로 바인딩되지 않은, 때문에 업로드 또는 대용량 파일 다운로드, 배경 등의 콘텐츠를 자동 업데이트를 사용할 수 있습니다. 참조 된 [백그라운드 전송 연습](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md) 서비스를 구현 하는 방법에 대 한 자세한 내용은 합니다.

백그라운드 전송 서비스는 백그라운드 가져오기 또는 응용 프로그램을 백그라운드에서 콘텐츠를 새로 고칠 수 있도록 원격 알림을 이룹니다 경우가 많습니다. 다음 두 섹션에서는 iOS 6 및 iOS 7에서 백그라운드에서 실행 되도록 전체 응용 프로그램 등록의 개념을 소개 했습니다.

