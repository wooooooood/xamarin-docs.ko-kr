---
title: 비동기 개요
description: '최신 버전의 C# 언어-버전 5 – 비동기 작업을 표현 하는 두 개의 새 키워드가 도입: async 및 await 합니다. 이러한 키워드를 사용 하 여 다른 스레드가에서 장기 실행 작업 (예: 네트워크 액세스)를 실행 하려면 작업 병렬 라이브러리를 활용 하는 간단한 코드를 작성 하 고 완료 시 결과 쉽게 액세스할 수 있도록 합니다. 최신 버전의 Xamarin.iOS 및 Xamarin.Android 지원 async 및 await-이 문서에서는 설명 및 Xamarin을 사용한 새 구문을 사용 하는 예제를 제공 합니다.'
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 5d6bb9581a4429502d9a70385b3ee2ff056f30ee
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="async-support-overview"></a>비동기 지원 개요

_최신 버전의 C# 언어-버전 5 – 비동기 작업을 표현 하는 두 개의 새 키워드가 도입: async 및 await 합니다. 이러한 키워드를 사용 하 여 다른 스레드가에서 장기 실행 작업 (예: 네트워크 액세스)를 실행 하려면 작업 병렬 라이브러리를 활용 하는 간단한 코드를 작성 하 고 완료 시 결과 쉽게 액세스할 수 있도록 합니다. 최신 버전의 Xamarin.iOS 및 Xamarin.Android 지원 async 및 await-이 문서에서는 설명 및 Xamarin을 사용한 새 구문을 사용 하는 예제를 제공 합니다._

Xamarin의 비동기 지원은 모노 3.0 foundation에서 구축 되며 모바일에서 잘 작동 Silverlight 버전을 모바일에서 잘 작동 버전의.NET 4.5가 되도록 되는 API 프로 파일을 업그레이드 합니다.

## <a name="overview"></a>개요

이 문서는 새로운 비동기를 소개 하 고 await 키워드 다음 Xamarin.iOS 및 Xamarin.Android에서 비동기 메서드를 구현 하는 몇 가지 간단한 예제를 통해 탐색 합니다.

자세한 내용은 C# 5 (다양 한 예제 및 여러 사용 시나리오 포함)의 새로운 비동기 기능에 대 한 MSDN 설명서를 참조 [Async 및 Await를 사용한 비동기 프로그래밍](http://msdn.microsoft.com/library/vstudio/hh191443.aspx)합니다.

샘플 응용 프로그램 (주 스레드를 차단) 없이 간단한 비동기 웹 요청 다음 다운로드 한 html 및 문자 수와 UI를 업데이트 합니다.

 [![](async-images/AsyncAwait_427x368.png "샘플 응용 프로그램 주 스레드를 차단 하지 않고는 간단한 비동기 웹 요청을 만들고 다운로드 한 html 및 문자 수와 UI를 업데이트 함")](async-images/AsyncAwait.png#lightbox)

Xamarin의 비동기 지원은 모노 3.0 foundation에서 구축 되며 모바일에서 잘 작동 Silverlight 버전을 모바일에서 잘 작동 버전의.NET 4.5가 되도록 되는 API 프로 파일을 업그레이드 합니다.

## <a name="requirements"></a>요구 사항

C# 5 기능 하려면 6.4 Xamarin.iOS 및 Xamarin.Android 4.8에 포함 되어 모노 3.0이 필요 합니다. 프로그램 모노, Xamarin.iOS, Xamarin.Android 및 Xamarin.Mac 활용을 업그레이드 하 라는 메시지가 표시 됩니다.

## <a name="using-async-amp-await"></a>비동기 사용 하 여 &amp; await

 `async` 및 `await` 새 C# 언어 기능을 쉽게 응용 프로그램의 주 스레드를 차단 하지 않고 장기 실행 작업을 수행 하는 스레드 코드를 작성할 수 있도록 작업 병렬 라이브러리와 함께 사용 됩니다.

## <a name="async"></a>async

### <a name="declaration"></a>선언

`async` 키워드는 메서드 선언에 (또는 무명 메서드 또는 람다) 놓입니다 비동기적으로 실행 되는 코드가 포함 되어 있는 것을 나타내기 위해 ie. 호출자의 스레드를 차단 하지 합니다.

메서드를로 `async` 하나 이상 포함 해야 await 식 또는 문입니다. 없는 경우 `await`s에에서 있는 메서드가 동기적으로 실행 한 다음 (동일한 없는 경우에 따라 없는 `async` 한정자). 컴파일러 경고 (않음 오류가 아닙니다)에서 그러면 메시지가 표시 됩니다.

### <a name="return-types"></a>반환 형식

비동기 메서드에서 반환 해야는 `Task`, `Task<TResult>` 또는 `void`합니다.

지정 된 `Task` 메서드가 다른 값을 반환 하지 않는 경우 반환 형식입니다.

지정 `Task<TResult>` 메서드는 값을 반환 하는 경우 여기서 `TResult` 되 고 반환 되는 형식 (같은 `int`예를 들면).

`void` 반환 형식은 필요로 하는 이벤트 처리기에 주로 사용 됩니다. Void를 반환 비동기 메서드를 호출 하는 코드 없습니다 `await` 결과에 있습니다.

### <a name="parameters"></a>매개 변수

비동기 메서드를 선언할 수 없습니다 `ref` 또는 `out` 매개 변수입니다.

## <a name="await"></a>await

Await 연산자는 비동기로 표시 된 메서드 내부 작업에 적용할 수 있습니다. 하면 메서드를 해당 지점에서 실행을 중지 하 고 작업이 완료 될 때까지 기다립니다.

Await를 사용 하 여 호출자의 스레드 – 차단 하지 않습니다 아니라 제어는 호출자에 게 반환 합니다. 즉, 예: 사용자 인터페이스 스레드 차단 되지 않습니다는 작업을 기다리면 때 하므로 호출 스레드가 차단 되지 않습니다.

작업이 완료 되는 메서드는 코드의 같은 지점에서 실행 다시 시작 합니다. 여기에 (있는 경우) try – catch – finally 블록의 try 범위를 반환 합니다. await finally 블록 또는 catch에서 사용할 수 없습니다.

에 대해 자세히 알아보세요 [msdn await](http://msdn.microsoft.com/library/vstudio/hh156528.aspx)합니다.

## <a name="exception-handling"></a>예외 처리

비동기 메서드 내에서 발생 하는 예외는 작업에 저장 되 고 작업을 하는 경우에 throw `await`ed 합니다. 이러한 예외는 발생 함와 try catch 블록 내에서 처리 될 수 있습니다.

## <a name="cancellation"></a>취소

비동기 메서드를 완료 하려면 시간이 오래 걸리는 취소를 지원 해야 합니다. 일반적으로 취소 다음과 같이 호출 됩니다.

- A `CancellationTokenSource` 개체가 만들어집니다.
- `CancellationTokenSource.Token` 인스턴스가 취소 가능한 비동기 메서드에 전달 됩니다.
- 호출 하 여 취소를 요청한는 `CancellationTokenSource.Cancel` 메서드.

그런 다음 태스크는 자신을 취소 하 고 취소를 승인.

취소에 대 한 자세한 내용은 참조 [비동기 작업을 취소 하는 방법을](http://msdn.microsoft.com/library/vstudio/jj155761.aspx) msdn 합니다.

## <a name="example"></a>예제

다운로드는 [예제 Xamarin 솔루션을](https://developer.xamarin.com/samples/mobile/AsyncAwait/) (에 대 한 iOS 및 Android)의 작업 예제를 보려면 `async` 및 `await` 모바일 앱에서 합니다. 예제 코드는이 섹션에서 자세히 설명 합니다.

### <a name="writing-an-async-method"></a>비동기 메서드를 작성합니다.

다음 방법을 보여 줍니다. 코딩 하는 방법 한 `async` 메서드는 `await`ed 작업:

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

-  메서드 선언에서 `async` 키워드입니다.
-  반환 형식이 `Task<int>` 호출 코드에 액세스할 수 있도록는 `int` 이 방법에서는 계산 값입니다.
-  Return 문이 `return exampleInt;` 정수 개체가 – 메서드가 반환 하는 팩트 `Task<int>` 의 언어 개선 기능 일부입니다.


### <a name="calling-an-async-method-1"></a>1 비동기 메서드를 호출합니다.

이 단추 클릭 이벤트 처리기는 위에서 설명한 메서드를 호출 하는 Android 샘플 응용 프로그램에서 확인할 수 있습니다.

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

메모:

-  익명 대리자의 비동기 키워드 접두사를 있습니다.
-  작업을 반환 하는 비동기 메서드 DownloadHomepage <int> sizeTask 변수에 저장 된 합니다.
-  코드는 sizeTask 변수에 기다립니다.  *이* 위치 하는 메서드가 일시 중단 되 고 자체 스레드에서 비동기 작업이 완료 될 때까지 컨트롤이 호출 코드에 반환 됩니다.
-  실행 않습니다 *하지* 있습니다 새로 만들 작업 불구 하 고 메서드의 첫 번째 줄에는 작업을 만들 때 일시 중지 합니다. Await 키워드는 실행이 일시 중지 위치를 나타냅니다.
-  비동기 작업이 완료 되 면 intResult 설정 되 고 await 줄에서 원래 스레드에서 실행이 계속 됩니다.


### <a name="calling-an-async-method-2"></a>2는 비동기 메서드를 호출합니다.

IOS 샘플 응용 프로그램에서이 예제는 대체 접근 방식을 설명 하기 위해 약간 다르게 기록 됩니다. 대신 익명 대리자를 사용 하 여 보다이 예제에서는 선언는 `async` 일반 이벤트 처리기와 같은 할당 되는 이벤트 처리기.

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

몇 가지 중요 한 사항:

-  메서드가로 표시 `async` 반환 하지만 `void` 합니다. 이벤트 처리기에 대 한만 이루어집니다 (그렇지 않으면 반환 됩니다는 `Task` 또는 `Task<TResult>` ).
-  코드 `await` 에 대 한 s는 `DownloadHomepage` 변수에 할당에서 직접 메서드 ( `intResult` ) 중간 했었습니다 위의 예와 달리 `Task<int>` 변수는 작업을 참조 합니다.  *이* 여기서 제어 기능이 회복 되기까지 호출자는 비동기 메서드가 완료 될 때까지 다른 스레드에서 위치입니다.
-  비동기 메서드가 완료 되 고 반환, 실행을 다시 시작은 `await` 즉, 정수 결과 반환 하 고 다음 UI 위젯에서 렌더링 합니다.


## <a name="summary"></a>요약

Async 사용 및 await 크게 주 스레드를 차단 하지 않고 백그라운드 스레드에서 장기 실행 작업을 생성 하는 데 필요한 코드를 간소화 합니다. 또한을 쉽게 결과 작업이 완료 되었을 때 액세스할 수 있습니다.

이 문서는 Xamarin.iOS 및 Xamarin.Android를 둘 다에 대 한 새로운 언어 키워드 및 예제에 대 한 개요를 제공 했습니다.



## <a name="related-links"></a>관련 링크

- [AsyncAwait (샘플)](https://developer.xamarin.com/samples/mobile/AsyncAwait/)
- [문에 콜백을 우리의 세대으로 이동](http://tirania.org/blog/archive/2013/Aug-15.html)
- [데이터 (iOS) (샘플)](https://developer.xamarin.com/samples/monotouch/Data/)
- [HttpClient (iOS) (샘플)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
- [MapKitSearch (iOS) (샘플)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [웹 세미나: C# 비동기 iOS 및 Android (비디오)](http://xamarin.wistia.com/medias/k27mc627xz)
- [비동기 사용한 비동기 프로그래밍 및 Await (MSDN)](http://msdn.microsoft.com/library/vstudio/hh191443.aspx)
- [(MSDN) 비동기 응용 프로그램 미세 조정](http://msdn.microsoft.com/library/vstudio/jj155761.aspx)
- [Await, 교착 상태와 UI 및! 오 내! (MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2011/01/13/10115163.aspx)
- [(MSDN) 완료 될 작업을 처리 합니다.](http://blogs.msdn.com/b/pfxteam/archive/2012/08/02/processing-tasks-as-they-complete.aspx)
- [TAP(작업 기반 비동기 패턴)](http://msdn.microsoft.com/library/hh873175.aspx)
- [키워드의 도입 하는 방법에 대 한 C# 5 (Eric Lippert 블로그) – 비동기](http://blogs.msdn.com/b/ericlippert/archive/2010/11/11/whither-async.aspx)
