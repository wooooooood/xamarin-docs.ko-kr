---
title: Xamarin.iOS에서 UI 스레드 작업
description: 이 문서에서는 Xamarin.iOS에서 UI 스레드를 사용 하는 방법을 설명 합니다. UI 스레드 실행에 설명, 백그라운드 스레드 예제를 제공 하 고 async/await를 검사 합니다.
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: e4485c485b708bdec06f7f1dc22f0bf33e07e982
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827749"
---
# <a name="working-with-the-ui-thread-in-xamarinios"></a>Xamarin.iOS에서 UI 스레드 작업

응용 프로그램 사용자 인터페이스는 항상 단일 스레드, 다중 스레드 장치 에서도 – 하나의 표현이 화면 및 표시 되는 항목의 변경 내용이 단일 '액세스 지점'를 통해 조정 해야 합니다. 이 예를 들어 동시에 같은 픽셀을 업데이트 하는 동안에서 여러 스레드를 방지 합니다.

코드 변경 내용을 사용자 인터페이스 컨트롤 기본에서 (또는 UI) 스레드를 확인만 해야 합니다. UI 업데이트 (예: 콜백 또는 백그라운드 스레드) 다른 스레드에서 발생 하는 화면에 렌더링 되지 않을 수 있습니다 또는 충돌이 발생할 수 있습니다.

## <a name="ui-thread-execution"></a>UI 스레드 실행

뷰에서 컨트롤을 만드는 하거나 터치 등 사용자가 시작한 이벤트를 처리 하는 경우 코드는 이미 UI 스레드의 컨텍스트에서 실행 됩니다.

코드 작업에 콜백을 백그라운드 스레드에서 실행 되는 경우 다음 가능성이 주 UI 스레드에서 실행 되지 않습니다. 이 경우에 래핑해야 코드에 대 한 호출에서 `InvokeOnMainThread` 또는 `BeginInvokeOnMainThread` 같이:

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

합니다 `InvokeOnMainThread` 메서드가에 정의 되어 `NSObject` UIKit 개체 (예: 보기 또는 뷰 컨트롤러)에 정의 된 메서드 내에서 호출 될 수 있도록 합니다.

Xamarin.iOS 응용 프로그램을 디버깅 하는 동안 코드를 잘못 된 스레드에서 UI 컨트롤에 액세스 하려고 하는 경우 오류가 throw 됩니다. 이를 추적 하 고 InvokeOnMainThread 메서드를 사용 하 여 이러한 문제를 해결할 수 있습니다. 이 디버깅 하는 동안 발생 하며 릴리스 빌드에서 오류가 발생 하지 않습니다. 다음과 같은 오류 메시지가 표시 됩니다.

 ![](ui-thread-images/image10.png "UI 스레드 실행")

 <a name="Background_Thread_Example" />


## <a name="background-thread-example"></a>백그라운드 스레드 예제

다음은 사용자 인터페이스 컨트롤에 액세스 하려고 하는 예 (한 `UILabel`) 간단한 스레드를 사용 하 여 백그라운드 스레드에서:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

코드는 throw 하는 `UIKitThreadAccessException` 디버깅 하는 동안. 문제 해결 (및 사용자 인터페이스 컨트롤은 주 UI 스레드에서 액세스할만 있는지 확인) 내에서 UI 컨트롤을 참조 하는 코드를 래핑하는 `InvokeOnMainThread` 같이 식:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

이 문서의 예제에서는 나머지에 대해이 사용 하지 않아도 됩니다. 이지만 중요 한 경우 앱이 실행 하면 네트워크 요청을 기억 하는 개념에 알림 센터 또는 다른 실행 되는 완료 처리기를 필요로 하는 다른 방법을 사용 하 여 스레드입니다.

 <a name="Async_Await_Example" />


## <a name="asyncawait-example"></a>Async/Await 예제

사용 하는 경우는 C# 5 async/await 키워드 `InvokeOnMainThread` 필요 하지 않은 대기 작업이 완료 되 면 호출 스레드에서 메서드를 계속 합니다.

이 코드를 예로 들어 (await 데모 용도로 지연 메서드 호출에 사용할) (TouchUpInside 처리기는) UI 스레드에서 호출 되는 비동기 메서드를 보여 줍니다. 포함 하는 메서드를 UI 스레드에서 호출 했으므로 UI 등의 작업에서 텍스트를 설정 된 `UILabel` 표시 또는 `UIAlertView` 백그라운드 스레드에서 비동기 작업이 완료 된 후에 안전 하 게 호출할 수 있습니다.

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

백그라운드 스레드 (주 UI 스레드가 아닌)에서 비동기 메서드를 호출 하면 다음 `InvokeOnMainThread` 해야 할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/monotouch/Controls/)
- [스레딩](~/ios/app-fundamentals/threading.md)
