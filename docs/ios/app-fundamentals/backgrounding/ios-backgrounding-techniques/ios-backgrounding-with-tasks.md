---
title: "iOS 작업과 Backgrounding"
ms.topic: article
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ad75dfac55add7e03ffbdb910e0e62ebd0fd6c18
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="ios-backgrounding-with-tasks"></a>iOS 작업과 Backgrounding

IOS에서 backgrounding 수행 하는 가장 간단한 방법은 작업으로 backgrounding 요구 사항을 중단 하 고 백그라운드에서 작업을 실행 하는 합니다. 작업에서 엄격한 시간 제한이 되며 일반적으로 가져오고 약 600 초 (10 분) 6, iOS에서 배경으로 응용 프로그램 이동에 처리 시간이 10 분 이상 iOS 7 이상에서.

백그라운드 작업 세 가지 범주로 나눌 수 있습니다.

1.  **배경 안전한 작업** 어디에서 호출-는 작업이 있는 경우 응용 프로그램 않으려면 중단 된 응용 프로그램을 백그라운드 입력 해야 합니다.
1.  **DidEnterBackground 작업** 하는 동안 호출-는 `DidEnterBackground` 정리 및 상태 저장을 지원 하기 위해 응용 프로그램 수명 주기 메서드.
1.  **백그라운드 전송 (iOS 7 이상)** -백그라운드 작업에는 특수 한 유형의 iOS 7에서 네트워크 전송을 수행 하는 데 사용 합니다. 정기적인 작업 달리 백그라운드 전송에는 미리 결정된 된 시간 제한 되어 있지 않습니다.


배경 안전 하 게 보호 및 `DidEnterBackground` 작업은 iOS 6 및 iOS 7 약간의 차이가 둘 다에서 사용 하기에 안전 합니다. 이러한 두 종류의 작업을 자세히 조사 보겠습니다.

## <a name="creating-background-safe-tasks"></a>배경 안전한 작업 만들기

일부 응용 프로그램 응용 프로그램 상태를 변경 해야 iOS에 의해 중단 하지 않아야 하는 작업을 포함 합니다. 이러한 작업 중단을 방지 하기 위해 한 가지 방법은 장기 실행 작업으로 ios 등록 하는 합니다. 사용할 수 있습니다이 패턴 아무 곳 이나 응용 프로그램에서 원하지 중단 되는 작업을 배경으로 응용 프로그램 사용자 put를 해야 합니다. 이 패턴에 대 한 훌륭한 후보를 서버에 새 사용자의 등록 정보를 보내거나 로그인 정보를 확인 하는 태스크가 것입니다.

다음 코드 조각 작업이 백그라운드에서 실행 되도록 등록 하는 방법을 보여 줍니다.

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

등록 프로세스에 고유한 id 가진 작업 쌍 `taskID`, 일치에 배치 합니다 `BeginBackgroundTask` 및 `EndBackgroundTask` 호출 합니다. 에 대 한 호출의 식별자를 생성 하려면 하도록는 `BeginBackgroundTask` 에서 메서드는 `UIApplication` 개체, 한 다음 새 스레드에서 장기 실행 작업을 일반적으로 시작 합니다. 작업이 완료 될 때 호출할 `EndBackgroundTask` 동일한 식별자를 전달 합니다. IOS를 하는 경우 응용 프로그램 종료 때문에이 중요 한 `BeginBackgroundTask` 호출에 짝이 되는 없는 `EndBackgroundTask`합니다.

> [!IMPORTANT]
> 주 스레드 이거나 백그라운드 스레드, 응용 프로그램의 요구에 따라에서 배경 안전한 작업을 실행할 수 있습니다.


## <a name="performing-tasks-during-didenterbackground"></a>DidEnterBackground 하는 동안 작업을 수행합니다.

에 이외에 장기 실행 작업을 배경 안전 등록 백그라운드에 저장 되 고 응용 프로그램으로 작업을 시작할 사용할 수 있습니다. 에 이벤트 메서드를 제공 하는 iOS는 *AppDelegate* 라는 클래스 `DidEnterBackground` 응용 프로그램 상태 저장, 사용자 데이터를 저장 하 고, 응용 프로그램의 백그라운드에 들어가기 전에 중요 한 콘텐츠를 암호화를 사용할 수 있는 합니다. 에 약 5 초가 메서드에서 반환 하는 응용 프로그램에서 나 종료 가져오기 됩니다. 따라서에서 정리 태스크를 완료 하는 데 5 초 이상 소요 될 수 있는 호출할 수 있는 내부는 `DidEnterBackground` 메서드. 이러한 작업은 별도 스레드에서 호출 되어야 합니다.

프로세스는 장기 실행 작업을 등록 하는 거의 동일 합니다. 다음 코드 조각 작업에서이 보여줍니다.

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

재정의 하 여 시작 하면는 `DidEnterBackground` 에서 메서드는 `AppDelegate`우리의 작업을 통해 등록 여기서 `BeginBackgroundTask` 앞의 예제에서와 같이 합니다. 다음으로 새 스레드를 생성 하 고 우리의 장기 실행 작업을 수행 합니다. `EndBackgroundTask` 이제 호출에서 장기 실행 작업 내 이후로 `DidEnterBackground` 메서드는 반환 된 이미 있어야 합니다.

> [!IMPORTANT]
> iOS를 사용 하 여는 [메커니즘 감시](http://developer.apple.com/library/ios/qa/qa1693/_index.html) 응용 프로그램의 UI 응답성 유지 되도록 합니다. 에 너무 많은 시간을 소모 하는 응용 프로그램 `DidEnterBackground` UI에 응답 하지 않게 됩니다. 사용 하면 백그라운드에서 실행 하는 작업을 시작한 `DidEnterBackground` 반환할 적절 한 시간 UI 응답 성능을 유지 하 고 감시 장치 응용 프로그램을 종료 하지 못하도록 합니다.


## <a name="handling-background-task-time-limits"></a>처리 백그라운드 작업 시간 제한

iOS에 엄격 하 게 제한 장기 백그라운드 작업을 실행할 수는 방법과 경우 배치의 `EndBackgroundTask` 호출으로 할당된 된 시간 내에서 생성 되지 않고, 응용 프로그램이 종료 됩니다. 남은 backgrounding 시간 및 필요한 경우 만료 처리기를 사용 하는 추적, 응용 프로그램을 종료 하는 iOS를 피할 수 했습니다.

### <a name="accessing-background-time-remaining"></a>배경, 남은 시간에 액세스

응용 프로그램 등록된 작업을 백그라운드도 옮기면 등록된 된 작업 실행에 약 600 초를 받게 됩니다. 정적을 사용 하 여 완료 하는 작업에 걸리는 시간을 확인할 수 있습니다 `BackgroundTimeRemaining` 의 속성은 `UIApplication` 클래스입니다. 다음 코드는 백그라운드 작업이 남아 서 초 단위로 시간을 제공 합니다.

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>처리 된 만료 처리기 응용 프로그램 종료를 방지합니다.

에 대 한 액세스를 제공 하는 것 외에도 `BackgroundTimeRemaining` 속성, iOS 제공는 적절 한 방법을 통해 배경 시간 만료를 처리 하는 **만료 처리기**합니다. 이는 선택적 있는 코드 블록을 작업에 할당 된 시간이 만료 되려고 할 때 실행 됩니다. 만료 처리기의 코드 호출 `EndBackgroundTask` 앱 잘 작동 하 고 iOS 작업 시간이 초과 될 경우에 응용 프로그램을 종료 하는 것을 금지 작업 ID 전달 합니다. `EndBackgroundTask` 뿐만 아니라 실행의 정상적인 만료 처리기 내에서 호출 되어야 합니다. 

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
        Console.WriteLine(UIApplication.SharedApplication.TimeRemaining);
        myFlag = SomeCalculationNeedsMoreTime();
    }
    //Only called if loop terminated due to myFlag and not expiration of time
    UIApplication.SharedApplication.EndBackgroundTask(taskId);
});
```

만료 처리기 코드를 실행 하려면 필요 하지 않은 동안에 백그라운드 태스크로 항상 만료 처리기를 사용 해야 합니다.

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>IOS 7 이상에서 백그라운드 작업

백그라운드 작업을 고려 하 여 iOS 7에서에서 가장 크게 변화 하지 작업, 구현 방식 건을 실행할 때.

사전 iOS 7 백그라운드에서 실행 하는 작업 해야 할 600 초를 완료 하는 점에 유의 하세요. 이 제한에 대 한 이유는 백그라운드에서 실행 하는 작업은 장치 유지 절전 모드 해제 작업의 기간에 대 한입니다.

 [![](ios-backgrounding-with-tasks-images/ios6.png "절전 모드 해제 사전 iOS 7 앱을 유지 하는 작업의 그래프")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

iOS 7 백그라운드 처리는 배터리 수명에 대 한 최적화 됩니다. IOS 7 backgrounding 됩니다 편의적: 장치를 절전 모드 해제 상태 유지 하지 않고 작업 준수 장치 절전 모드로 전환 하 고 장치 다시 시작 되어 전화 통화, 알림, 들어오는 전자 메일 및 기타 처리 하는 경우 대신 청크에서의 처리를 수행 되 면 일반적인 중단 합니다. 다음 다이어그램에서는 작업 손상 될 수 있습니다 어떻게에 대 한 정보를 구성 합니다.

 [![](ios-backgrounding-with-tasks-images/ios7.png "청크 후 iOS 7 침입을 받고 작업의 그래프")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

태스크 실행이 시간을 더 이상 연속 없기 때문에 네트워크 전송을 수행 하는 작업 iOS 7에서에서 다르게 처리 되어야 합니다. 개발자는 사용 하 여 `NSURlSession` 네트워크 전송을 처리 하는 API입니다. 다음 섹션에는 백그라운드 전송의 한 개요입니다.

 <a name="background-transfers" />

## <a name="background-transfers"></a>백그라운드 전송

IOS 7에서에서 백그라운드 전송의 핵심은 새 `NSURLSession` API입니다. `NSURLSession` 작업을 만들 수 있도록 합니다.

1.  네트워크 및 장치 중단을 통해 콘텐츠를 전송 합니다.
1.  큰 파일 업로드 및 다운로드 ( *백그라운드 전송 서비스* ).


이 수식이 어떻게 자세히 살펴보겠습니다.

### <a name="nsurlsession-api"></a>NSURLSession API

 `NSURLSession` 네트워크를 통해 콘텐츠를 전송 하는 데는 강력한 API입니다. 네트워크 중단 및 응용 프로그램 상태 변화를 통해 데이터 전송을 처리 하는 도구 집합을 제공 합니다.

`NSURLSession` API 작업 관련된 데이터 블록의 네트워크를 통해 셔틀을 생성 하는 차례로 하나 또는 여러 세션을 만듭니다. 빠르고 안정적으로 데이터를 전송 하도록 작업을 비동기적으로 실행 합니다. 때문에 `NSURLSession` 는 비동기적 이며 모든 세션 시스템 및 응용 프로그램 하며 전송이 완료 된 시기를 알 수 있도록 완료 처리기 블록에 필요 합니다.

사전 iOS 7 및 이후 iOS 7에서 사용할 수 있는 네트워크 전송의 수행 하려면 확인 하는 경우는 `NSURLSession` enqueue 전송 하기 위해 사용할 수 있으며 정기적인 백그라운드 작업을 사용 하 여 없으면 전송 하지:

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
> IOS 6 백그라운드 UI 업데이트를 지원 하지 않으며 응용 프로그램을 종료 하는 iOS 6 규격 코드로 배경에서 UI를 업데이트할 수 있는 호출을 수행 하지 마십시오.


`NSURLSession` API에는 다양 한 인증 처리, 실패 한 전송을 관리 및 클라이언트 쪽 하지 서버 쪽--하지만 오류를 보고 기능을 포함 합니다. 작업의 중단 iOS 7에서에서 도입 된 시간을 실행 하는 브리지 도와 빠르고 안정적으로 대용량 파일을 전송에 대 한 지원 기능을 제공 합니다. 다음 섹션에는이 두 번째 기능을 탐색합니다.

### <a name="background-transfer-service"></a>백그라운드 전송 서비스

IOS 7 하기 전에 백그라운드에서 파일 업로드 또는 다운로드에서 신뢰할 수 없습니다. 백그라운드 작업을 실행 하려면 제한 된 시간을 가져오지만 네트워크 파일의 크기와 파일을 전송 하는 데 걸리는 시간이 달라 집니다. IOS 7에서에서 사용할 수는 `NSURLSession` 성공적으로 업로드 하 고 큰 파일을 다운로드 합니다. 특정 `NSURLSession` 백그라운드에서 큰 파일의 네트워크 전송을 처리 하는 세션 형식을 라고는 *백그라운드 전송 서비스*합니다.

백그라운드 전송 서비스를 사용 하 여 시작 하는 전송 운영 체제에 의해 관리 되 고 인증 및 오류를 처리 하기 위한 Api를 제공 합니다. 전송 된 임의의 시간 제한으로 바인딩되지, 업로드 하거나 큰 파일을 다운로드, 배경 등의 콘텐츠를 자동 업데이트를 사용할 수 수 있습니다. 참조는 [백그라운드 전송 연습](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md) 서비스를 구현 하는 방법에 대 한 자세한 내용은 합니다.

백그라운드 전송 서비스는 종종 배경 인출 또는 응용 프로그램을 백그라운드에서 콘텐츠를 새로 고칠 수 있도록 원격 알림이과 연결 됩니다. 다음 두 섹션에서 iOS 6 및 iOS 7에서 백그라운드에서 실행 하는 전체 응용 프로그램 등록의 개념을 소개 했습니다.

