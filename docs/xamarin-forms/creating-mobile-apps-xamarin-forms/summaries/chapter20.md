---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 20. Async and file I/O''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ad71dc5f5389f1676698a761a138b3f76ffa9fa0
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136684"
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>20장의 요약 정보입니다. 비동기 및 파일 I/O

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)

> [!NOTE] 
> 이 페이지의 정보는 Xamarin.Forms가 책에 제공된 자료와는 다르게 사용되는 경우를 설명합니다.

 그래픽 사용자 인터페이스는 사용자 입력 이벤트에 순차적으로 응답해야 합니다. 따라서 사용자 입력 이벤트의 모든 처리가 단일 스레드(종종 *주 스레드* 또는 *UI 스레드*라고도 함)에서 발생해야 합니다.

사용자는 그래픽 사용자 인터페이스가 응답할 것으로 기대합니다. 즉, 프로그램에서 사용자 입력 이벤트를 신속하게 처리해야 합니다. 이렇게 할 수 없는 경우 처리를 실행 보조 스레드로 이관해야 합니다.

이 책의 여러 샘플 프로그램에서는 [`WebRequest`](xref:System.Net.WebRequest) 클래스를 사용했습니다. 이 클래스에서 [`BeginGetResponse`](xref:System.Net.WebRequest.BeginGetResponse(System.AsyncCallback,System.Object)) 메서드는 완료 시 콜백 함수를 호출하는 작업자 스레드를 시작합니다. 그러나 해당 콜백 함수는 작업자 스레드에서 실행되므로 프로그램이 사용자 인터페이스에 액세스하려면 [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) 메서드를 호출해야 합니다.

> [!NOTE]
> Xamarin.Forms 프로그램은 인터넷을 통해 파일에 액세스하기 위해 [`WebRequest`](xref:System.Net.WebRequest)가 아닌 [`HttpClient`](xref:System.Net.Http.HttpClient)를 사용해야 합니다. `HttpClient`는 비동기 작업을 지원합니다.

비동기 처리에 대한 최신 접근 방식은 .NET 및 C#에서 사용할 수 있습니다. 여기에는 [`Task`](xref:System.Threading.Tasks.Task) 및 [`Task<TResult>`](xref:System.Threading.Tasks.Task`1) 클래스와 [`System.Threading`](xref:System.Threading) 및 [`System.Threading.Tasks`](xref:System.Threading.Tasks) 네임스페이스의 기타 형식은 물론 C# 5.0 `async` 및 `await` 키워드도 포함됩니다. 이 장에서는 중점적으로 다루는 내용입니다.

## <a name="from-callbacks-to-await"></a>콜백에서 대기

`Page` 클래스 자체에는 경고 상자를 표시하는 세 가지 비동기 메서드가 포함되어 있습니다.

- `Task` 개체를 반환하는 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String))
- `Task<bool>` 개체를 반환하는 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String))
- `Task<string>` 개체를 반환하는 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[]))

`Task` 개체는 이러한 메서드가 탭 기반 비동기 패턴(TAP)을 구현함을 나타냅니다. 이러한 `Task` 개체는 메서드에서 빠르게 반환됩니다. `Task<T>` 반환 값은 작업이 완료될 때 `TResult` 형식의 값을 사용할 수 있는 "프라미스"를 구성합니다. `Task` 반환 값은 완료되지만 값을 반환하지 않는 비동기 작업을 나타냅니다.

이러한 모든 경우, `Task`는 사용자가 경고 상자를 닫을 때 완료됩니다.  

### <a name="an-alert-with-callbacks"></a>콜백이 있는 경고

[**AlertCallbacks**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) 샘플에서는 콜백 메서드를 사용하여 `Task<bool>` 반환 개체 및 `Device.BeginInvokeOnMainThread` 호출을 처리하는 방법을 보여 줍니다.

### <a name="an-alert-with-lambdas"></a>람다가 있는 경고

[**AlertLambdas**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) 샘플에서는 `Task` 및 `Device.BeginInvokeOnMainThread` 호출을 처리하는 데 익명 람다 함수를 사용하는 방법을 보여 줍니다.  

### <a name="an-alert-with-await"></a>await가 있는 경고

보다 간단한 방법에는 C# 5에 도입된 `async` 및 `await` 키워드가 포함됩니다. [**AlertAwait**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) 샘플에서는 이를 사용하는 방법을 보여 줍니다.

### <a name="an-alert-with-nothing"></a>아무 것도 없는 경고

비동기 메서드에서 `Task<TResult>`가 아닌 `Task`를 반환하는 경우, 비동기 작업이 완료될 때 알 필요가 없으면 프로그램에서 이러한 기술을 사용할 필요가 없습니다. [**NothingAlert**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) 샘플에서는 이를 보여 줍니다.

### <a name="saving-program-settings-asynchronously"></a>비동기로 프로그램 설정 저장

[**SaveProgramChanges**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) 샘플에서는 `OnSleep`메서드를 재정의하지 않고 변경되는 프로그램 설정을 저장하기 위해 `Application`의 [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) 메서드를 사용하는 방법을 보여 줍니다.

### <a name="a-platform-independent-timer"></a>플랫폼 독립적 타이머

[`Task.Delay`](xref:System.Threading.Tasks.Task.Delay(System.Int32))를 사용하여 플랫폼 독립적 타이머를 만들 수 있습니다. [**TaskDelayClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) 샘플에서는 이를 보여 줍니다.

## <a name="file-inputoutput"></a>파일 입력/출력

일반적으로 .NET [`System.IO`](xref:System.IO) 네임스페이스는 파일 I/O 지원의 소스입니다. 이 네임스페이스의 일부 메서드가 비동기 작업을 지원하지만 대부분은 그렇지 않습니다. 네임스페이스는 정교한 파일 I/O 함수를 수행하는 몇 가지 간단한 메서드 호출도 지원합니다.

### <a name="good-news-and-bad-news"></a>좋은 소식과 나쁜 소식

Xamarin.Forms에서 지원하는 모든 플랫폼은 애플리케이션에 비공개로 적용되는 애플리케이션 로컬 스토리지인 &mdash; 스토리지를 지원합니다.

Xamarin.iOS 및 Xamarin.Android 라이브러리에는 Xamarin에서 이러한 두 플랫폼에 대해 명시적으로 조정된 .NET 버전이 포함되어 있습니다. 여기에는 이 두 플랫폼에서 애플리케이션 로컬 스토리지를 사용하여 파일 I/O를 수행하는 데 사용할 수 있는 `System.IO`의 클래스가 포함됩니다.

그러나 Xamarin.Forms PCL에서 이러한 `System.IO` 클래스를 검색하면 찾을 수 없습니다. 문제는 Microsoft가 Windows 런타임 API에 대한 파일 I/O를 완전히 개선했다는 것입니다. Windows 8.1, Windows Phone 8.1 및 유니버설 Windows 플랫폼을 대상으로 하는 프로그램은 파일 I/O에 `System.IO`를 사용하지 않습니다.

이는 [`DependencyService`](xref:Xamarin.Forms.DependencyService)를 사용해야 함을 의미합니다(먼저 [**9장. 플랫폼별 API 호출**](chapter09.md)에 설명된 대로 파일 I/O 구현).

> [!NOTE]
> 이식 가능한 클래스 라이브러리가 .NET Standard 2.0 라이브러리로 대체되었으며 .NET Standard 2.0은 모든 Xamarin.Forms 플랫폼에 대해 [`System.IO`](xref:System.IO) 형식을 지원합니다. 대부분의 파일 I/O 작업에 더 이상 `DependencyService`를 사용할 필요가 없습니다. 파일 I/O에 대한 최신 접근 방식은 [Xamarin.Forms의 파일 처리](~/xamarin-forms/data-cloud/data/files.md)를 참조하세요.

### <a name="a-first-shot-at-cross-platform-file-io"></a>플랫폼 간 파일 I/O에 대한 첫 번째 샷

[**TextFileTryout**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) 샘플에서는 파일 I/O에 대한 [`IFileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) 인터페이스를 정의하고 모든 플랫폼에서 이 인터페이스 구현을 정의합니다. 그러나 Windows 런타임 파일 I/O 메서드가 비동기이기 때문에 Windows 런타임 구현은 이 인터페이스의 메서드에서 작동하지 않습니다.

### <a name="accommodating-windows-runtime-file-io"></a>Windows 런타임 파일 I/O 수용

Windows 런타임에서 실행되는 프로그램은 애플리케이션 로컬 스토리지를 포함한 파일 I/O에 [`Windows.Storage`](/uwp/api/Windows.Storage) 및 [`Windows.Storage.Streams`](/uwp/api/Windows.Storage.Streams) 네임스페이스의 클래스를 사용합니다. Microsoft가 UI 스레드 차단을 방지하기 위해 50밀리초 이상이 필요한 모든 작업은 비동기로 처리되어야 한다고 결정했으므로 이러한 파일 I/O 메서드는 대부분 비동기입니다.

이 새로운 방법을 보여 주는 코드는 다른 애플리케이션에서 사용할 수 있도록 라이브러리에 저장됩니다.

## <a name="platform-specific-libraries"></a>플랫폼별 라이브러리

재사용 가능한 코드를 라이브러리에 저장하는 것이 유리합니다. 재사용 가능한 코드의 여러 부분이 완전히 다른 운영 체제용인 경우에는 확실히 훨씬 더 어렵습니다.

[**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) 솔루션에서 한 가지 방법을 보여 줍니다. 이 솔루션에는 다음 7개 프로젝트가 포함되어 있습니다.

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), 일반적인 Xamarin.Forms PCL
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), iOS 클래스 라이브러리
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), Android 클래스 라이브러리
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), 유니버설 Windows 클래스 라이브러리
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), 모든 Windows 플랫폼에 공통적인 코드에 대한 공유 프로젝트

모든 개별 플랫폼 프로젝트( **Xamarin.FormsBook.Platform.WinRT** 제외)에는 **Xamarin.FormsBook.Platform**에 대한 참조가 있습니다. 세 개의 Windows 프로젝트에는 **Xamarin.FormsBook.Platform.WinRT**에 대한 참조가 있습니다.

모든 프로젝트는 Xamarin.Forms 애플리케이션 솔루션의 프로젝트에서 직접 참조하지 않는 경우 라이브러리를 로드하는 정적 `Toolkit.Init` 메서드를 포함하고 있습니다.

**Xamarin.FormsBook.Platform** 프로젝트는 새 [`IFileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) 인터페이스를 포함합니다. 이제 모든 메서드는 접미사가 `Async`인 이름을 가지며 `Task` 개체를 반환합니다.

**Xamarin.FormsBook.Platform.WinRT** 프로젝트는 Windows 런타임에 대한 [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) 클래스를 포함합니다.

**Xamarin.FormsBook.Platform.iOS** 프로젝트는 iOS에 대한 [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) 클래스를 포함합니다. 이제 이러한 메서드는 비동기여야 합니다. 일부 메서드는 `StreamWriter` 및 `StreamReader`에 정의된 비동기 버전의 메서드를 사용합니다([`WriteAsync`](xref:System.IO.StreamWriter.WriteAsync(System.String)) 및 [`ReadToEndAsync`](xref:System.IO.StreamReader.ReadToEndAsync)). 다른 메서드는 [`FromResult`](xref:System.Threading.Tasks.Task.FromResult*) 메서드를 사용하여 결과를 `Task` 개체로 변환합니다.

**Xamarin.FormsBook.Platform.Android** 프로젝트에는 비슷한 Android용 [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) 클래스가 포함되어 있습니다.

또한 **Xamarin.FormsBook.Platform** 프로젝트에는 `DependencyService` 개체의 사용을 용이하게 하는 [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) 클래스도 포함되어 있습니다.

이러한 라이브러리를 사용하려면 애플리케이션 솔루션에 **Xamarin.FormsBook.Platform** 솔루션의 모든 프로젝트가 포함되어야 하고, 각 애플리케이션 프로젝트에는 **Xamarin.FormsBook.Platform**의 해당 라이브러리에 대한 참조가 있어야 합니다.

[**TextFileAsync**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) 솔루션에서는 **Xamarin.FormsBook.Platform** 라이브러리를 사용하는 방법을 보여 줍니다. 각 프로젝트에는 `Toolkit.Init`에 대한 호출이 포함되어 있습니다. 애플리케이션은 비동기 파일 I/O 함수를 사용합니다.

### <a name="keeping-it-in-the-background"></a>백그라운드에서 유지

사용자 인터페이스 스레드로 다시 전환되지 않도록 [`ConfigureAwait`](xref:System.Threading.Tasks.Task`1.ConfigureAwait(System.Boolean)) 메서드를 사용하면 Windows 런타임 [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) 클래스 &mdash;의 `WriteFileAsync` 및 `ReadFileASync` 메서드 등과 같이 여러 비동기 메서드를 호출하는 라이브러리의 메서드를 약간 더 효율적으로 만들 수 있습니다.

### <a name="dont-block-the-ui-thread"></a>UI 스레드를 차단하지 않습니다.

경우에 따라서는 메서드의 [`Result`](xref:System.Threading.Tasks.Task`1.Result) 속성을 사용하여 `ContinueWith` 또는 `await`의 사용을 피하는 것이 좋습니다. UI 스레드를 차단하거나 애플리케이션을 중단시킬 수 있는 경우, 피해야 합니다.

## <a name="your-own-awaitable-methods"></a>사용자 고유의 대기 가능 메서드

일부 코드를 [`Task.Run`](xref:System.Threading.Tasks.Task.Run(System.Action)) 메서드 중 하나로 전달하여 비동기로 실행할 수 있습니다. 일부 오버헤드를 처리할 수 있는 비동기 메서드 내에서 `Task.Run`을 호출할 수 있습니다.

다양한 `Task.Run` 패턴이 아래에 설명되어 있습니다.

### <a name="the-basic-mandelbrot-set"></a>기본 Mandelbrot 집합

Mandelbrot 집합을 실시간으로 그리기 위해 [ **Xamarin.Forms.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 `System.Numerics` 네임스페이스와 비슷한 [`Complex`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) 구조가 있습니다.

[**MandelbrotSet**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) 샘플에는 기본 흑백 Mandelbrot 집합을 계산하고 [`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)를 사용하여 비트맵에 배치하는 코드 숨김 파일에 `CalculateMandeblotAsync` 메서드가 있습니다.

### <a name="marking-progress"></a>진행률 표시

비동기 메서드의 진행률을 보고하려면 [`Progress<T>`](xref:System.Progress`1) 클래스를 인스턴스화하고 [`IProgress<T>`](xref:System.IProgress`1) 형식의 인수를 포함하는 비동기 메서드를 정의할 수 있습니다. 이는 [**MandelbrotProgress**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) 샘플에서 설명합니다.

### <a name="cancelling-the-job"></a>작업 취소

취소 가능한 비동기 메서드를 작성할 수도 있습니다. 이름이 [`CancellationTokenSource`](xref:System.Threading.CancellationTokenSource)인 클래스로 시작합니다. [`Token`](xref:System.Threading.CancellationTokenSource.Token) 속성은 [`CancellationToken`](xref:System.Threading.CancellationToken) 형식의 값입니다. 이 값은 비동기 함수로 전달됩니다. 프로그램은 `CancellationTokenSource`(일반적으로 사용자에 의한 작업에 대한 응답)의 [`Cancel`](xref:System.Threading.CancellationTokenSource.Cancel) 메서드를 호출하여 비동기 함수를 취소합니다.

비동기 메서드는 `CancellationToken`의 [`IsCancellationRequested`](xref:System.Threading.CancellationToken.IsCancellationRequested) 속성을 주기적으로 확인하고 속성이 `true`인 경우 종료하거나, 간단히 [`ThrowIfCancellationRequested`](xref:System.Threading.CancellationToken.ThrowIfCancellationRequested) 메서드를 호출하여 메서드를 [`OperationCancelledException`](xref:System.OperationCanceledException)으로 끝낼 수 있습니다.

[**MandelbrotCancellation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) 샘플에서는 취소 가능한 함수를 사용하는 방법을 보여 줍니다.

### <a name="an-mvvm-mandelbrot"></a>MVVM Mandelbrot

[**MandelbrotXF**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) 샘플은 보다 광범위한 사용자 인터페이스를 포함하고 있으며, 대부분 [`MandelbrotModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) 및 [`MandelbrotViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs) 클래스를 기반으로 합니다.

[![Mandelbrot X F의 삼중 스크린샷](images/ch20fg13-small.png "MVVM Mandelbrot")](images/ch20fg13-large.png#lightbox "MVVM Mandelbrot")

## <a name="back-to-the-web"></a>웹으로 돌아가기

일부 샘플에서 사용되는 [`WebRequest`](xref:System.Net.WebRequest) 클래스는 비동기 프로그래밍 모델 또는 APM이라고 하는 구형 비동기 프로토콜을 사용합니다. [`TaskFactory`](xref:System.Threading.Tasks.TaskFactory`1) 클래스의 `FromAsync` 메서드 중 하나를 사용하여 이러한 클래스를 최신 TAP 프로토콜로 변환할 수 있습니다. [**ApmToTap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) 샘플에서는 이 경우를 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [20장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [20장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [파일 작업](~/xamarin-forms/data-cloud/data/files.md)
