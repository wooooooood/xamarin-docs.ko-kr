---
title: 비동기 지원 개요
description: 이 문서에서는 비동기 및 대기를 사용한 프로그래밍에 대해 설명 C# 하 고 5에 도입 된 개념을 사용 하 여 비동기 코드를 보다 쉽게 작성할 수 있습니다.
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 8978dbce97948d02d520b788d024fb50f4884635
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488883"
---
# <a name="async-support-overview"></a>비동기 지원 개요

_C#5 비동기 프로그래밍을 간소화 하는 두 개의 키워드 (비동기 및 대기)가 도입 되었습니다. 이러한 키워드를 사용 하 여 작업 병렬 라이브러리를 활용 하는 간단한 코드를 작성 하 여 다른 스레드에서 장기 실행 작업 (예: 네트워크 액세스)을 실행 하 고 완료 시 결과에 쉽게 액세스할 수 있습니다. 최신 버전의 Xamarin.ios 및 Xamarin Android는 async 및 wait를 지원 합니다 .이 문서에서는 Xamarin과 함께 새 구문을 사용 하는 방법에 대 한 설명과 예제를 제공 합니다._

Xamarin의 비동기 지원은 Mono 3.0 foundation을 기반으로 하며, 모바일에서 사용할 수 있는 Silverlight 버전의 API 프로필을 .NET 4.5의 모바일 친화적인 버전으로 업그레이드 합니다.

## <a name="overview"></a>개요

이 문서에서는 새로운 async 및 wait 키워드를 소개 하 고 Xamarin.ios 및 Xamarin.ios에서 비동기 메서드를 구현 하는 몇 가지 간단한 예제를 안내 합니다.

5의 C# 새로운 비동기 기능에 대 한 자세한 내용은 (많은 샘플과 다양 한 사용 시나리오 포함) [비동기 프로그래밍](https://docs.microsoft.com/dotnet/csharp/async)문서를 참조 하세요.

이 샘플 응용 프로그램은 기본 스레드를 차단 하지 않고 간단한 비동기 웹 요청을 만든 다음 다운로드 된 html 및 문자 개수로 UI를 업데이트 합니다.

 [![](async-images/AsyncAwait_427x368.png "The sample application makes a simple asynchronous web request without blocking the main thread then updates the UI with the downloaded html and character count")](async-images/AsyncAwait.png#lightbox)

Xamarin의 비동기 지원은 Mono 3.0 foundation을 기반으로 하며, 모바일에서 사용할 수 있는 Silverlight 버전의 API 프로필을 .NET 4.5의 모바일 친화적인 버전으로 업그레이드 합니다.

## <a name="requirements"></a>요구 사항

C#5 기능에는 Xamarin.ios 6.4 및 Xamarin. Android 4.8에 포함 된 Mono 3.0이 필요 합니다. Mono, Xamarin.ios, xamarin.ios 및 Xamarin.ios를 사용 하도록 업그레이드 하 라는 메시지가 표시 됩니다.

## <a name="using-async-amp-await"></a>비동기 &amp; 사용 대기

 `async` 및 `await`은 작업 C# 병렬 라이브러리와 함께 작동 하 여 응용 프로그램의 주 스레드를 차단 하지 않고 장기 실행 작업을 수행 하는 스레드 코드를 쉽게 작성할 수 있도록 하는 새로운 언어 기능입니다.

## <a name="async"></a>async

### <a name="declaration"></a>선언

`async` 키워드는 비동기 (ie)로 실행할 수 있는 코드가 포함 되어 있음을 나타내기 위해 메서드 선언 (또는 람다 또는 무명 메서드에서)에 배치 됩니다. 호출자의 스레드를 차단 하지 않습니다.

`async` 표시 된 메서드는 하나 이상의 wait 식 또는 문을 포함 해야 합니다. 메서드에 `await` 문이 없는 경우 동기적으로 실행 됩니다 (`async` 한정자가 없는 경우와 동일). 이로 인해 컴파일러 경고가 발생 하 고 오류는 발생 하지 않습니다.

### <a name="return-types"></a>반환 유형

비동기 메서드는 `Task``Task<TResult>` 또는 `void`를 반환 해야 합니다.

메서드에서 다른 값을 반환 하지 않는 경우 `Task` 반환 형식을 지정 합니다.

메서드에서 값을 반환 해야 하는 경우 `Task<TResult>`를 지정 합니다. 여기서 `TResult`은 반환 되는 형식 (예: `int`)입니다.

`void` 반환 형식은 주로 필요로 하는 이벤트 처리기에 사용 됩니다. Void를 반환 하는 비동기 메서드를 호출 하는 코드는 결과에 `await` 수 없습니다.

### <a name="parameters"></a>매개 변수

비동기 메서드는 `ref` 또는 `out` 매개 변수를 선언할 수 없습니다.

## <a name="await"></a>await

Wait 연산자는 async로 표시 된 메서드 내의 작업에 적용할 수 있습니다. 그러면 메서드가 해당 시점에 실행을 중지 하 고 작업이 완료 될 때까지 기다립니다.

Wait를 사용 하면 호출자의 스레드를 차단 하지 않고 제어가 호출자에 게 반환 됩니다. 즉, 호출 스레드가 차단 되지 않으므로 예를 들어 작업을 대기 하는 동안 사용자 인터페이스 스레드가 차단 되지 않습니다.

작업이 완료 되 면 메서드는 코드의 동일한 지점에서 실행을 다시 시작 합니다. 여기에는 try-catch-finally 블록의 try 범위로의 반환 (있는 경우)이 포함 됩니다. wait는 catch 또는 finally 블록에서 사용할 수 없습니다.

Microsoft Docs [에 대 한 자세한](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/await) 내용은을 참조 하세요.

## <a name="exception-handling"></a>예외 처리

비동기 메서드 내에서 발생 하는 예외는 태스크에 저장 되 고 태스크가 `await`되 면 throw 됩니다. 이러한 예외는 catch 블록 내에서 catch 하 고 처리할 수 있습니다.

## <a name="cancellation"></a>취소

완료 하는 데 시간이 오래 걸리는 비동기 메서드는 취소를 지원 해야 합니다. 일반적으로 취소는 다음과 같이 호출 됩니다.

- `CancellationTokenSource` 개체가 만들어집니다.
- `CancellationTokenSource.Token` 인스턴스가 취소 가능한 비동기 메서드에 전달 됩니다.
- `CancellationTokenSource.Cancel` 메서드를 호출 하 여 취소를 요청 합니다.

그런 다음 작업 자체를 취소 하 고 취소를 승인 합니다.

취소에 대한 자세한 내용은 [비동기 애플리케이션 미세 조정(C#)](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application)을 참조하세요.

## <a name="example"></a>예

[예제 Xamarin 솔루션](https://docs.microsoft.com/samples/xamarin/mobile-samples/asyncawait/) (IOS 및 Android 모두)을 다운로드 하 여 모바일 앱의 `async` 및 `await`에 대 한 작업 예제를 확인 합니다. 이 섹션에서는 예제 코드에 대해 자세히 설명 합니다.

### <a name="writing-an-async-method"></a>비동기 메서드 작성

다음 메서드는 `await`ed 작업을 사용 하 여 `async` 메서드를 코딩 하는 방법을 보여 줍니다.

```csharp
public async Task<int> DownloadHomepage()
{
    var httpClient = new HttpClient(); // Xamarin supports HttpClient!

    Task<string> contentsTask = httpClient.GetStringAsync("https://visualstudio.microsoft.com/xamarin"); // async method!

    // await! control returns to the caller and the task continues to run on another thread
    string contents = await contentsTask;

    ResultEditText.Text += "DownloadHomepage method continues after async call. . . . .\n";

    // After contentTask completes, you can calculate the length of the string.
    int exampleInt = contents.Length;

    ResultEditText.Text += "Downloaded the html and found out the length.\n\n\n";

    ResultEditText.Text += contents; // just dump the entire HTML

    return exampleInt; // Task<TResult> returns an object of type TResult, in this case int
}
```

다음 사항에 유의 하세요.

- 메서드 선언에는 `async` 키워드가 포함 됩니다.
- 반환 형식은 `Task<int>` 이므로 호출 하는 코드는이 메서드에서 계산 된 `int` 값에 액세스할 수 있습니다.
- 반환 `return exampleInt;` 문은 정수 개체입니다. 메서드가 `Task<int>` 반환 하는 사실은 언어 향상의 일부입니다.

### <a name="calling-an-async-method-1"></a>비동기 메서드 호출 1

Android 샘플 응용 프로그램에서 위에 설명 된 메서드를 호출 하는이 단추 클릭 이벤트 처리기를 찾을 수 있습니다.

```csharp
GetButton.Click += async (sender, e) => {

    Task<int> sizeTask = DownloadHomepage();

    ResultTextView.Text = "loading...";
    ResultEditText.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await sizeTask;

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultTextView.Text = "Length: " + intResult ;
    // "returns" void, since it's an event handler
};
```

참고:

- 익명 대리자에는 async 키워드 접두사가 있습니다.
- 비동기 메서드 DownloadHomepage는 sizeTask 변수에 저장 된\<int > 작업을 반환 합니다.
- SizeTask 변수에 대 한 기다립니다 코드입니다.  *이* 위치는 메서드가 일시 중단 되 고 비동기 작업이 자체 스레드에서 완료 될 때까지 제어를 호출 코드로 반환 하는 위치입니다.
- 작업을 만들 때에도 메서드의 첫 번째 줄에 작업이 생성 되 면 실행이 일시 중지 *되지* 않습니다. Wait 키워드는 실행이 일시 중지 된 위치를 나타냅니다.
- 비동기 작업이 완료 되 면 intResult가 설정 되 고 wait 줄에서 원래 스레드에서 실행이 계속 됩니다.

### <a name="calling-an-async-method-2"></a>비동기 메서드 2 호출

IOS 샘플 응용 프로그램에서는이 예제가 약간 다르게 기록 되어 다른 방법을 보여 줍니다. 익명 대리자를 사용 하는 대신이 예제에서는 일반 이벤트 처리기와 같이 할당 된 `async` 이벤트 처리기를 선언 합니다.

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

그런 다음 이벤트 처리기 메서드는 다음과 같이 정의 됩니다.

```csharp
async void HandleTouchUpInside (object sender, EventArgs e)
{
    ResultLabel.Text = "loading...";
    ResultTextView.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await DownloadHomepage();

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultLabel.Text = "Length: " + intResult ;
}
```

몇 가지 중요 한 사항은 다음과 같습니다.

- 메서드가 `async`로 표시 되어 있지만 `void`를 반환 합니다. 이는 일반적으로 이벤트 처리기에 대해서만 수행 됩니다. 그렇지 않으면 `Task` 또는 `Task<TResult>`을 반환 합니다.
- `DownloadHomepage` 메서드의 `await` 키워드는 중간 `Task<int>` 변수를 사용 하 여 작업을 참조 하는 이전 예제와 달리 변수 (`intResult`)에 직접 할당 됩니다.  *이* 위치는 비동기 메서드가 다른 스레드에서 완료 될 때까지 제어가 호출자에 게 반환 되는 위치입니다.
- 비동기 메서드가 완료 되 고 반환 되는 경우 `await`에서 실행이 다시 시작 되어 정수 결과가 반환 된 다음 UI 위젯에 렌더링 됩니다.

## <a name="summary"></a>요약

Async 및 wait를 사용 하면 주 스레드를 차단 하지 않고 백그라운드 스레드에서 장기 실행 작업을 생성 하는 데 필요한 코드를 크게 간소화할 수 있습니다. 또한 작업이 완료 되 면 결과에 쉽게 액세스할 수 있도록 합니다.

이 문서에는 Xamarin.ios 및 Xamarin.ios의 새로운 언어 키워드와 예제에 대 한 개요가 제공 됩니다.

## <a name="related-links"></a>관련 링크

- [AsyncAwait (샘플)](https://docs.microsoft.com/samples/xamarin/mobile-samples/asyncawait/)
- [세대의 ' Go To ' 문으로 콜백](https://tirania.org/blog/archive/2013/Aug-15.html)
- [데이터 (iOS) (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/data/)
- [HttpClient (iOS) (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/httpclient/)
- [MapKitSearch (iOS) (샘플)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [비동기 프로그래밍](https://docs.microsoft.com/dotnet/csharp/async)
- [Async 애플리케이션 미세 조정(C#)](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application)
- [대기 하 고, UI를 대기 하 고, 교착 상태를 기다립니다. 오.](https://devblogs.microsoft.com/pfxteam/await-and-ui-and-deadlocks-oh-my/)
- [작업이 완료 되 면 처리)](https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete/)
- [TAP(작업 기반 비동기 패턴)](https://msdn.microsoft.com/library/hh873175.aspx)
- [비동기 C# 5 (Eric Lippert의 블로그) – 키워드 소개 정보](https://blogs.msdn.microsoft.com/ericlippert/2010/11/11/asynchrony-in-c-5-part-six-whither-async/)
