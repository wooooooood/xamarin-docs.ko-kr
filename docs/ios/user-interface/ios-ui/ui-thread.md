---
title: Xamarin.ios에서 UI 스레드 사용
description: 이 문서에서는 Xamarin.ios에서 UI 스레드를 사용 하는 방법을 설명 합니다. UI 스레드 실행에 대해 설명 하 고, 백그라운드 스레드 예제를 제공 하 고, 비동기/대기를 검사 합니다.
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: ab72034d7b565a31c59d997f03844b6c8c959785
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768175"
---
# <a name="working-with-the-ui-thread-in-xamarinios"></a>Xamarin.ios에서 UI 스레드 사용

응용 프로그램 사용자 인터페이스는 다중 스레드 장치 에서도 항상 단일 스레드입니다. 즉, 화면에 대 한 표현이 하나 뿐 이며 표시 되는 항목에 대 한 변경 내용은 단일 ' 액세스 지점 '을 통해 조정 해야 합니다. 이렇게 하면 여러 스레드가 동시에 동일한 픽셀을 업데이트 하려고 시도 하지 않습니다 (예:).

코드는 주 (또는 UI) 스레드의 사용자 인터페이스 컨트롤만 변경 해야 합니다. 다른 스레드에서 발생 한 모든 UI 업데이트 (예: 콜백 또는 백그라운드 스레드)가 화면에 렌더링 되지 않거나 충돌을 일으킬 수 있습니다.

## <a name="ui-thread-execution"></a>UI 스레드 실행

뷰에서 컨트롤을 만들거나 터치와 같이 사용자가 시작한 이벤트를 처리 하는 경우 코드는 UI 스레드의 컨텍스트에서 이미 실행 되 고 있습니다.

코드가 백그라운드 스레드에서 실행 중인 경우 작업이 나 콜백에서 주 UI 스레드에서 실행 되 고 있지 않을 수 있습니다. 이 경우 다음과 `InvokeOnMainThread` `BeginInvokeOnMainThread` 같이 호출에서 코드를 래핑해야 합니다.

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

메서드 `InvokeOnMainThread` 는에 `NSObject` 정의 되어 있으므로 모든 uikit 개체 (예: 뷰 또는 뷰 컨트롤러)에 정의 된 메서드 내에서 호출할 수 있습니다.

Xamarin.ios 응용 프로그램을 디버깅 하는 동안 코드에서 잘못 된 스레드의 UI 컨트롤에 액세스 하려고 하면 오류가 throw 됩니다. 이렇게 하면 InvokeOnMainThread 메서드를 사용 하 여 이러한 문제를 추적 하 고 해결할 수 있습니다. 이 오류는 디버깅 하는 동안에만 발생 하며 릴리스 빌드에서 오류를 throw 하지 않습니다. 오류 메시지는 다음과 같이 표시 됩니다.

 ![](ui-thread-images/image10.png "UI 스레드 실행")

 <a name="Background_Thread_Example" />

## <a name="background-thread-example"></a>백그라운드 스레드 예제

다음은 간단한 스레드를 사용 하 여 백그라운드 스레드에서 사용자 인터페이스 컨트롤 ( `UILabel`)에 액세스 하려고 시도 하는 예제입니다.

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

이 코드는 디버깅 `UIKitThreadAccessException` 하는 동안을 throw 합니다. 문제를 해결 하려면 (그리고 사용자 인터페이스 컨트롤이 주 UI 스레드에서만 액세스할 수 있는지 확인) 다음과 같이 `InvokeOnMainThread` 식 내에서 UI 컨트롤을 참조 하는 코드를 래핑합니다.

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

이 문서의 나머지 예제에서는이를 사용할 필요가 없지만, 앱에서 네트워크 요청을 할 때, 알림 센터 또는 다른 방법으로 실행 되는 완료 처리기를 필요로 하는 다른 방법을 사용 하는 경우 기억해 야 할 중요 한 개념입니다. 스레드.

 <a name="Async_Await_Example" />

## <a name="asyncawait-example"></a>Async/Wait 예

대기 작업이 완료 C# 되 면 호출 스레드에서 메서드가 `InvokeOnMainThread` 계속 되기 때문에 5 async/wait 키워드를 사용할 필요가 없습니다.

이 예제 코드 (데모용 으로만 사용 되는 지연 메서드 호출에 기다립니다)는 UI 스레드에서 호출 되는 비동기 메서드 (TouchUpInside 처리기)를 보여 줍니다. 포함 하는 메서드가 ui 스레드에서 호출 되기 때문에에 대 `UILabel` 한 텍스트를 설정 하거나를 `UIAlertView` 표시 하는 등의 ui 작업은 백그라운드 스레드에서 비동기 작업이 완료 된 후 안전 하 게 호출 될 수 있습니다.

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

비동기 메서드가 주 UI 스레드가 `InvokeOnMainThread` 아닌 백그라운드 스레드에서 호출 되는 경우에는 여전히 필요 합니다.

## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
- [스레딩](~/ios/app-fundamentals/threading.md)
