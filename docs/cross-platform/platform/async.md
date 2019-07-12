---
title: 비동기 지원 개요
description: 이 문서를 사용한 비동기 프로그래밍에 설명 하 고에 도입 된 개념을 기다릴 C# 5 쉽게 비동기 코드를 작성 합니다.
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: cca147f0c5dd1a217f464ffbed2a1ad2618c9b80
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830190"
---
# <a name="async-support-overview"></a>비동기 지원 개요

_C#비동기 프로그래밍을 단순화 하기 위해 두 키워드를 도입 5: async 및 await를 합니다. 이러한 키워드를 사용 하 여 다른 스레드에서 장기 실행 작업 (예: 네트워크 액세스)를 실행 하려면 작업 병렬 라이브러리를 활용 하는 간단한 코드를 작성 하 고 완료 시 결과 쉽게 액세스할 수 있습니다. 최신 버전의 Xamarin.iOS 및 Xamarin.Android 지원 async 및 await-이 문서에서는 설명 및 Xamarin을 사용 하 여 새 구문을 사용 하는 예제를 제공 합니다._

Xamarin의 비동기 지원의 Mono 3.0을 기반으로 빌드되고 API 프로필은 모바일 친화적인 Silverlight 버전을.NET 4.5의 된 모바일 친화적인 버전에서 업그레이드 합니다.

## <a name="overview"></a>개요

이 문서는 새 비동기를 소개 하 고 await 키워드를 Xamarin.iOS 및 Xamarin.Android에서 비동기 메서드를 구현 하는 몇 가지 간단한 예제를 안내 합니다.

자세한 설명은의 새로운 비동기 기능에 대 한 C# 5 (샘플 및 다른 사용 시나리오에 대 한 많은 포함) 문서를 참조 [비동기 프로그래밍](https://docs.microsoft.com/dotnet/csharp/async)합니다.

샘플 응용 프로그램 (주 스레드를 차단) 없이 간단한 비동기 웹 요청을 다운로드 한 html 및 문자 수를 사용 하 여 UI를 업데이트 합니다.

 [![](async-images/AsyncAwait_427x368.png "샘플 응용 프로그램 주 스레드를 차단 하지 않고 간단한 비동기 웹 요청을 다운로드 한 html 및 문자 수를 사용 하 여 UI를 업데이트 합니다.")](async-images/AsyncAwait.png#lightbox)

Xamarin의 비동기 지원의 Mono 3.0을 기반으로 빌드되고 API 프로필은 모바일 친화적인 Silverlight 버전을.NET 4.5의 된 모바일 친화적인 버전에서 업그레이드 합니다.

## <a name="requirements"></a>요구 사항

C#5 기능 하려면 6.4 Xamarin.iOS 및 Xamarin.Android 4.8에 포함 된 Mono 3.0이 필요 합니다. 에 Mono, Xamarin.iOS, Xamarin.Android 및 Xamarin.Mac 활용을 업그레이드 하 라는 메시지가 표시 됩니다.

## <a name="using-async-amp-await"></a>비동기를 사용 하 여 &amp; await

 `async` 및 `await` 새로운 C# 언어 기능을 쉽게 응용 프로그램의 주 스레드를 차단 하지 않고 장기 실행 작업을 수행 하는 스레드 코드를 작성할 수 있도록 작업 병렬 라이브러리와 함께 작동 합니다.

## <a name="async"></a>async

### <a name="declaration"></a>선언

`async` 키워드는 메서드 선언에서 (또는 무명 메서드 또는 람다)에 배치 됩니다를 비동기적으로 실행 되는 코드가 포함 되어 있는지를 나타내는 ie. 호출자의 스레드를 차단 합니다.

메서드를 사용 하 여 표시할지 `async` 하나 이상 포함 해야 await 식 및 문의 합니다. 없으면 `await`s에에서 있는 메서드를 동기적으로 실행 됩니다 (동일한 것 처럼 없습니다 `async` 한정자). 또한 이렇게 하면 컴파일러 경고 (하지만 오류가 아닙니다).

### <a name="return-types"></a>반환 형식

비동기 메서드를 반환할지를 `Task`, `Task<TResult>` 또는 `void`합니다.

지정 된 `Task` 메서드는 다른 값을 반환 하지 않는 경우 반환 형식입니다.

지정 `Task<TResult>` 메서드는 값을 반환 하는 경우 여기서 `TResult` 반환 되는 형식입니다 (같은 `int`예를 들어).

`void` 반환 형식은 필요한 이벤트 처리기에 주로 사용 됩니다. Void 반환 비동기 메서드를 호출 하는 코드 없는 `await` 결과입니다.

### <a name="parameters"></a>매개 변수

비동기 메서드를 선언할 수 없습니다 `ref` 또는 `out` 매개 변수입니다.

## <a name="await"></a>await

Await 연산자를 비동기로 표시 된 메서드 내에서 작업에 적용할 수 있습니다. 메서드를 해당 지점에서 실행을 중지 하 고 대기 작업이 완료 될 때까지 발생 합니다.

Await를 사용 하 여 호출자의 스레드 – 차단 하지 않습니다 호출자에 게 제어를 반환 하는 대신 합니다. 즉, 호출 스레드가 차단 되지 않는지, 예를 들어 사용자 인터페이스 스레드는 차단 되지 작업을 기다리는 경우 하므로 합니다.

작업이 완료 되 면 코드에서 동일한 시점에 실행 메서드를 다시 시작 합니다. 여기에 (있는 경우)는 try – catch – finally 블록의 try 범위를 반환 합니다. await finally 블록 또는 catch에서 사용할 수 없습니다.

에 대해 자세히 알아보세요 [await](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/await) Microsoft Docs에서.

## <a name="exception-handling"></a>예외 처리

비동기 메서드 내에서 발생 하는 예외는 작업에 저장 되 고 작업을 하는 경우에 throw `await`ed 합니다. 이러한 예외 포착 하 고 try / catch 블록 내에서 처리 될 수 있습니다.

## <a name="cancellation"></a>취소

완료 하려면 시간이 오래 걸리는 비동기 메서드는 취소를 지원 해야 합니다. 일반적으로 취소는 다음과 같이 호출 됩니다.

- `CancellationTokenSource` 개체가 만들어집니다.
- `CancellationTokenSource.Token` 인스턴스가 취소 가능한 비동기 메서드에 전달 됩니다.
- 호출 하 여 취소가 요청 된 `CancellationTokenSource.Cancel` 메서드.

다음 태스크는 자체를 취소 하 고 취소를 승인.

취소에 대한 자세한 내용은 [비동기 애플리케이션 미세 조정(C#)](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application)을 참조하세요.

## <a name="example"></a>예제

다운로드 합니다 [예제에서는 Xamarin 솔루션](https://developer.xamarin.com/samples/mobile/AsyncAwait/) (iOS 및 Android) 용의 작업 예제를 보려면 `async` 및 `await` 모바일 앱에서. 예제 코드는이 섹션에서 자세히 설명 합니다.

### <a name="writing-an-async-method"></a>비동기 메서드를 작성합니다.

다음 메서드를 보여 줍니다 코딩 방법을 `async` 메서드는 `await`ed 작업:

```csharp
public async Task<int> DownloadHomepage()
{
    var httpClient = new HttpClient(); // Xamarin supports HttpClient!

    Task<string> contentsTask = httpClient.GetStringAsync("http://xamarin.com"); // async method!

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

이러한 점은 note:

-  메서드 선언에 포함 된 `async` 키워드입니다.
-  반환 형식은 `Task<int>` 호출 코드에 액세스할 수 있도록 합니다 `int` 이 메서드에서 계산 되는 값입니다.
-  Return 문이 `return exampleInt;` 정수 개체 – 메서드가 반환 하는 팩트는 `Task<int>` 일부 언어 개선 기능입니다.


### <a name="calling-an-async-method-1"></a>1 비동기 메서드를 호출합니다.

이 단추 클릭 이벤트 처리기는 위에서 설명한 메서드를 호출 하려면 Android 샘플 응용 프로그램에서 확인할 수 있습니다.

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

-  익명 대리자가 async 키워드 접두사입니다.
-  작업을 반환 하는 비동기 메서드 DownloadHomepage <int> sizeTask 변수에 저장 된 합니다.
-  SizeTask 변수에 코드를 기다립니다.  *이* 메서드가 일시 중단 되 고 자체 스레드에서 비동기 작업이 완료 될 때까지 호출 코드에 제어를 반환 하는 위치입니다.
-  실행 않습니다 *되지* 있습니다 작성 하려는 작업에도 불구 하 고 메서드의 첫 줄에 태스크를 만들 때 일시 중지 합니다. Await 키워드는 실행이 일시 중지 위치를 나타냅니다.
-  비동기 작업이 완료 되 면 intResult 설정 되 고 await 줄에서 원래 스레드에서 계속 실행 됩니다.


### <a name="calling-an-async-method-2"></a>2 비동기 메서드를 호출합니다.

IOS 샘플 응용 프로그램에서 예제는 대체 방법을 보여 주기 위해 약간 다르게 기록 됩니다. 대신 익명 대리자를 사용 하는 것이 예제에서는 선언는 `async` 일반 이벤트 처리기와 같은 할당 되는 이벤트 처리기:

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

이벤트 처리기 메서드는 다음 다음과 같이 정의 됩니다.

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

몇 가지 중요 사항:

-  메서드가로 표시 `async` 반환 하지만 `void` 합니다. 일반적으로 이렇게 이벤트 처리기에 대 한 (그렇지 않은 경우 반환을 `Task` 또는 `Task<TResult>` ).
-  코드 `await` 에 대 한 s 합니다 `DownloadHomepage` 변수에 할당 한 직접 메서드 ( `intResult` ) 중간 사용 했던 이전 예와 달리 `Task<int>` 작업이 참조 하는 변수입니다.  *이* 여기서 컨트롤은 호출자에 게 반환 비동기 메서드가 완료 될 때까지 다른 스레드에서 위치입니다.
-  비동기 메서드를 완료 하 고 반환을 때 실행을 다시 시작을 `await` 즉, 정수 결과 반환 하 고 다음 UI 위젯을 렌더링 합니다.


## <a name="summary"></a>요약

비동기를 사용 하 고 await 주 스레드를 차단 하지 않고 백그라운드 스레드에서 장기 실행 작업을 생성 하는 데 필요한 코드를 크게 간소화 합니다. 또한 쉽게 작업이 완료 되 면 결과 액세스할 수 있습니다.

이 문서는 Xamarin.iOS 및 Xamarin.Android에 대 한 새로운 언어 키워드 및 예제 개요를 제공 했습니다.



## <a name="related-links"></a>관련 링크

- [AsyncAwait (샘플)](https://developer.xamarin.com/samples/mobile/AsyncAwait/)
- [이 세대으로 콜백을 문으로 이동](https://tirania.org/blog/archive/2013/Aug-15.html)
- [데이터 (iOS) (샘플)](https://developer.xamarin.com/samples/monotouch/Data/)
- [HttpClient (iOS) (샘플)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
- [MapKitSearch (iOS) (샘플)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [비동기 프로그래밍](https://docs.microsoft.com/dotnet/csharp/async)
- [Async 애플리케이션 미세 조정(C#)](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application)
- [Await, UI 및 교착 상태 및! 아 내!](https://devblogs.microsoft.com/pfxteam/await-and-ui-and-deadlocks-oh-my/)
- [완료 될 때마다 작업을 처리)](https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete/)
- [TAP(작업 기반 비동기 패턴)](https://msdn.microsoft.com/library/hh873175.aspx)
- [에 비동기 C# 키워드를 도입 하는 방법에 대 한 5 (Eric Lippert의 블로그)-](http://blogs.msdn.com/b/ericlippert/archive/2010/11/11/whither-async.aspx)
