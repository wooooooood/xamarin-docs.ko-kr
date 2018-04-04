---
title: UI 스레드 작업
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 72f161001509519fb02a652f23eaa7805a55f7ca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-the-ui-thread"></a>UI 스레드 작업

응용 프로그램 사용자 인터페이스는 항상 단일 스레드, 다중 스레드 장치 에서도 – 화면에 하나의 표현인 하 고 표시 되는 내용 변경 내용이 단일 '액세스 지점'를 통해 조정 해야 합니다. 이 예를 들어 동시에 같은 픽셀을 업데이트 하는 동안에서 여러 스레드를 차단 합니다.

코드에는 사용자 인터페이스 컨트롤의 주에서 (또는 UI)에 대 한 변경 스레드만 수행 해야 합니다. UI 업데이트 (예: 콜백 또는 백그라운드 스레드)는 다른 스레드에서 발생 하는 화면에 렌더링 되지 않을 수 있습니다 또는 충돌이 발생할 수 있습니다.

## <a name="ui-thread-execution"></a>UI 스레드 실행

뷰에서 컨트롤을 만드는 지 아니면 터치 등 사용자가 시작한 이벤트를 처리, 코드 파일이 이미 UI 스레드의 컨텍스트에서 실행 됩니다.

작업 또는 콜백을 백그라운드 스레드에서 실행 되는 코드는 다음 경우 주 UI 스레드에서 실행 되지 않을 가능성이입니다. 이 경우에 대 한 호출에서 코드를 넣는 해야 `InvokeOnMainThread` 또는 `BeginInvokeOnMainThread` 다음과 같이 합니다.

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

`InvokeOnMainThread` 메서드가에 정의 되어 `NSObject` 되므로 모든 UIKit 개체 (예: 보기 또는 뷰-컨트롤러)에 정의 된 메서드 내에서 호출할 수 있습니다.

Xamarin.iOS 응용 프로그램을 디버깅 하는 동안 잘못 된 스레드에서 UI 컨트롤에 액세스 하려고 하는 경우 오류가 throw 됩니다. 이를 추적 하 고 InvokeOnMainThread 방법으로 이러한 문제를 해결 하도록 도와줍니다. 이 디버깅 하는 동안 발생 하며 릴리스 빌드에 오류가 발생 하지 않습니다. 다음과 같은 오류 메시지가 표시 됩니다.

 ![](ui-thread-images/image10.png "UI 스레드 실행")

 <a name="Background_Thread_Example" />


## <a name="background-thread-example"></a>백그라운드 스레드 예제

사용자 인터페이스 컨트롤에 액세스 하려고 하는 예 (한 `UILabel`)는 간단한 스레드를 사용 하 여 백그라운드 스레드에서:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

코드를 throw 하는 `UIKitThreadAccessException` 디버깅 하는 동안 합니다. 문제 해결 (및 사용자 인터페이스 컨트롤만 주 UI 스레드와에서 관리자 액세스 하는지를 확인)에 안에 UI 컨트롤을 참조 하는 코드를 래핑하는 `InvokeOnMainThread` 다음과 같은 식:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

절대로 필요 하지만이 문서에 있는 예제의 나머지 부분에 대 한이 응용 프로그램에서는 네트워크 요청 때 기억해 야 할 중요 한 개념을 사용 하 여 알림 센터 또는 다른에서 실행 되는 완료 처리기를 필요로 하는 다른 방법을 사용 스레드입니다.

 <a name="Async_Await_Example" />


## <a name="asyncawait-example"></a>Async/Await 예제

C# 5 async/await 키워드를 사용 하는 경우 `InvokeOnMainThread` 필요 하지 않은 대기 중인된 작업이 완료 되 면 메서드는 호출 스레드에서 계속 됩니다.

이 예제 코드 (설명을 위해 순수 하 게 하는 지연 메서드 호출에 기다리는) (이 TouchUpInside 처리기) UI 스레드에서 호출 하는 비동기 메서드를 보여 줍니다. 포함 하는 메서드를 UI 스레드에서 호출 했으므로 UI 등의 작업에는 텍스트 설정는 `UILabel` 또는 표시는 `UIAlertView` 백그라운드 스레드에서 비동기 작업이 완료 된 후에 안전 하 게 호출할 수 있습니다.

```csharp
async partial void button2_TouchUpInside (UIButton sender)
{
    textfield1.ResignFirstResponder ();
    textfield2.ResignFirstResponder ();
    textview1.ResignFirstResponder ();
    label1.Text = "async method started";
    await Task.Delay(1000); // example purpose only
    label1.Text = "1 second passed";
    await Task.Delay(2000);
    label1.Text = "2 more seconds passed";
    await Task.Delay(1000);
    new UIAlertView("Async method complete", "This method", 
               null, "Cancel", null)
        .Show();
    label1.Text = "async method completed";
}
```

백그라운드 스레드 (하지 주 UI 스레드)에서 비동기 메서드를 호출 하는 다음 `InvokeOnMainThread` 필요한 할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/Controls/)
- [스레딩](~/ios/app-fundamentals/threading.md)
