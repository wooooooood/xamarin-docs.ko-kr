---
title: 요약 20 장입니다. 비동기 및 파일 I/O
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 20 장 요약 합니다. 비동기 및 파일 I/O'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: a795b382b9bcc727b0b0872d29d30a501cfed0a6
ms.sourcegitcommit: f3f28722198e172d81c16bdeab0cb0a581a08dd0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51598901"
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>요약 20 장입니다. 비동기 및 파일 I/O

> [!NOTE] 
> 이 페이지에 대 한 참고 사항 Xamarin.Forms 책의 내용을에서 달라졌는지를 위치 하는 영역을 나타냅니다.

 그래픽 사용자 인터페이스를 순차적으로 사용자 입력 이벤트에 응답 해야 합니다. 즉, 모든 사용자 입력 이벤트 처리 라고도 하는 한 스레드에서 발생 해야 한다고 합니다 *주 스레드에서* 또는 *UI 스레드*합니다.

사용자가 응답 그래픽 사용자 인터페이스를 기대 합니다. 이 프로그램을 사용자 입력 이벤트를 신속 하 게 처리 해야 한다는 것을 의미 합니다. 가능 하지 않은 처리 한 후 보조 스레드 실행에 있어 해야 합니다.

이 가이드의 몇 가지 샘플 프로그램 사용 합니다 [ `WebRequest` ](xref:System.Net.WebRequest) 클래스입니다. 이 클래스에는 [ `BeginGetResponse` ](xref:System.Net.WebRequest.BeginGetResponse(System.AsyncCallback,System.Object)) 메서드 완료 되 면 콜백 함수를 호출 하는 작업자 스레드를 시작 합니다. 그러나 프로그램을 호출 해야 하므로 해당 콜백 함수가 작업자 스레드에서 실행 [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) 메서드가 사용자 인터페이스에 액세스 하도록 합니다.

> [!NOTE]
> Xamarin.Forms 프로그램을 사용할지 [ `HttpClient` ](xref:System.Net.Http.HttpClient) 대신 [ `WebRequest` ](xref:System.Net.WebRequest) 인터넷을 통해 파일에 액세스 합니다. `HttpClient` 비동기 작업을 지원합니다.

비동기 처리 하는 최신 방법을.NET 및 C#에서 제공 됩니다. 여기에 [ `Task` ](xref:System.Threading.Tasks.Task) 및 [ `Task<TResult>` ](xref:System.Threading.Tasks.Task`1) 클래스 및 기타 형식에는 [ `System.Threading` ](xref:System.Threading) 및 [ `System.Threading.Tasks` ](xref:System.Threading.Tasks) 네임 스페이스와 C# 5.0 `async` 하 고 `await` 키워드입니다. 이 장에서 중점적입니다.

## <a name="from-callbacks-to-await"></a>Await 콜백에서

`Page` 클래스 자체는 경고 상자를 표시 하는 세 가지 비동기 메서드를 포함 합니다.

- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) 반환 된 `Task` 개체
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) 반환 된 `Task<bool>` 개체
- [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) 반환 된 `Task<string>` 개체

`Task` 개체 이러한 메서드 구현 탭 이라고 하는 작업 기반 비동기 패턴을는 것을 나타냅니다. 이러한 `Task` 개체 메서드에서 신속 하 게 반환 됩니다. 합니다 `Task<T>` 값 구성 이라는 "약속"는 반환 형식의 값 `TResult` 작업이 완료 될 때 사용할 수 있습니다. `Task` 반환 값 값이 없으며 완료 하지만 반환 하는 비동기 작업을 나타냅니다.

이러한 모든 경우에는 `Task` 은 사용자가 경고 상자를 닫을 때 완료 됩니다.  

### <a name="an-alert-with-callbacks"></a>콜백 사용 하 여 경고

[ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) 샘플을 처리 하는 방법을 보여 줍니다 `Task<bool>` 개체를 반환 하 고 `Device.BeginInvokeOnMainThread` 콜백 메서드를 사용 하 여 호출 합니다.

### <a name="an-alert-with-lambdas"></a>람다를 사용 하 여 경고

[ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) 샘플 처리에 대 한 익명 람다 함수를 사용 하는 방법을 보여 줍니다 `Task` 고 `Device.BeginInvokeOnMainThread` 호출 합니다.  

### <a name="an-alert-with-await"></a>Await 사용 하 여 경고

더 간단한 방법은 포함 된 `async` 및 `await` C# 5에서 도입 된 키워드. 합니다 [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) 샘플 사용 방법을 보여 줍니다.

### <a name="an-alert-with-nothing"></a>아무 것도 없으면 경고

비동기 메서드를 반환 하는 경우 `Task` 대신 `Task<TResult>`, 비동기 작업이 완료 될 때를 알 필요가 없는 경우 이러한 기술 중 하나를 사용 하려면 프로그램 필요 하지 않습니다. 합니다 [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) 샘플에서는이 방법을 보여 줍니다.

### <a name="saving-program-settings-asynchronously"></a>프로그램 설정을 저장 하는 비동기적으로

합니다 [ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) 의 사용법을 보여 줍니다 합니다 [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) 메서드의 `Application` 없이 변경 될 때 해당 프로그램 설정을 저장 하려면 재정의 `OnSleep` 메서드.

### <a name="a-platform-independent-timer"></a>플랫폼 독립적인 타이머

사용 하는 것이 불가능 [ `Task.Delay` ](xref:System.Threading.Tasks.Task.Delay(System.Int32)) 플랫폼 독립적인 타이머를 만들 수 있습니다. 합니다 [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) 샘플에서는이 방법을 보여 줍니다.

## <a name="file-inputoutput"></a>파일 입/출력

일반적으로.NET [ `System.IO` ](xref:System.IO) 네임 스페이스에는 원본 파일 I/O 지원 되었습니다. 이 네임 스페이스의 일부 메서드는 비동기 작업을 지원 하지만 대부분은 그렇지 않습니다. 네임 스페이스는 복잡 한 파일 I/O 기능을 수행 하는 몇 가지 간단한 메서드 호출도 지원 합니다.

### <a name="good-news-and-bad-news"></a>좋은 소식과 나쁜 소식

Xamarin.Forms 지원 응용 프로그램에 대 한 로컬 저장소에서 지 원하는 모든 플랫폼 &mdash; 응용 프로그램에 개인 저장소.

Xamarin.iOS 및 Xamarin.Android 라이브러리에는 Xamarin에 명시적으로이 두 플랫폼에 맞게 조정할 수는.NET 버전에 포함 됩니다. 클래스를 포함 하는 이러한 `System.IO` 이 두 플랫폼에서 응용 프로그램 로컬 저장소를 사용 하 여 파일 I/O를 수행 하려면 사용할 수 있습니다.

그러나 이러한 항목에 대해 검색 하는 경우 `System.IO` Xamarin.Forms PCL에서 클래스를 찾을 수 없습니다 하 합니다. Windows 런타임 API에 대 한 해당 Microsoft 완전히 새롭게 수정 파일 I/O입니다. Windows 8.1, Windows Phone 8.1, 및 유니버설 Windows 플랫폼을 대상으로 하는 프로그램을 사용 하지 않는 `System.IO` 파일 I/O에 대 한 합니다.

즉, 사용 해야 합니다 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) (먼저 나오는 [ **9 장에서 합니다. 플랫폼별 API 호출** ](chapter09.md) 파일 I/O를 구현 합니다.

> [!NOTE]
> .NET Standard 2.0 지원 및.NET Standard 2.0 라이브러리를 사용 하 여 이식 가능한 클래스 Libaries 바뀌었습니다 [ `System.IO` ](xref:System.IO) 모든 Xamarin.Forms 플랫폼에 대 한 형식입니다. 더 이상 사용 하는 데 필요한 것을 `DependencyService` 대부분 파일 I/O 작업에 대 한 합니다. 참조 [Xamarin.Forms 파일 처리](~/xamarin-forms/app-fundamentals/files.md) 파일 I/O에는 최신 방법에 대 한 합니다.

### <a name="a-first-shot-at-cross-platform-file-io"></a>플랫폼 간 파일 I/O에 첫 번째 샷

합니다 [ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) 샘플 정의 [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) 파일 I/O 및 모든 플랫폼에서이 인터페이스의 구현에 대 한 인터페이스입니다. 그러나 Windows 런타임 파일 I/O 메서드는 비동기 Windows 런타임 구현이이 인터페이스에서 메서드를 사용 하 여 작동 하지 않습니다.

### <a name="accommodating-windows-runtime-file-io"></a>Windows 런타임 파일 I/O를 수용합니다.

Windows 런타임에서 실행 되는 프로그램의 클래스를 사용 합니다 [ `Windows.Storage` ](/uwp/api/Windows.Storage) 하 고 [ `Windows.Storage.Streams` ](/uwp/api/Windows.Storage.Streams) I/O, 응용 프로그램 로컬 저장소를 포함 하 여 파일에 대 한 네임 스페이스. Microsoft를 결정 하는 50 밀리초 이상 UI 스레드를 차단 하지 않으려면 비동기 여야 합니다. 필요한 모든 작업을 하기 때문에 이러한 파일 I/O 메서드는 비동기 대부분입니다.

다른 응용 프로그램에서 사용할 수 있도록 라이브러리에서 이러한 새 방법을 보여 주는 코드가 됩니다.

## <a name="platform-specific-libraries"></a>플랫폼별 라이브러리

라이브러리에 재사용 가능한 코드를 저장 하는 것이 유용 합니다. 다양 한 재사용 가능한 코드를 완전히 다른 운영 체제에 대 한 경우 훨씬 더 어렵습니다.

합니다 [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) 솔루션은 한 가지 방법을 보여 줍니다. 이 솔루션에는 7 개의 다른 프로젝트가 포함 됩니다.

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), a normal Xamarin.Forms PCL
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), iOS 클래스 라이브러리를
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), Android 클래스 라이브러리를
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), 유니버설 Windows 클래스 라이브러리
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), 모든 Windows 플랫폼에 공통 된 코드에 대 한 공유 프로젝트

모든 개별 플랫폼 프로젝트가 (제외 **Xamarin.FormsBook.Platform.WinRT**)에 대 한 참조가 **Xamarin.FormsBook.Platform**합니다. 세 개의 Windows 프로젝트에 대 한 참조가 **Xamarin.FormsBook.Platform.WinRT**합니다.

모든 프로젝트 포함 정적 `Toolkit.Init` Xamarin.Forms 응용 프로그램 솔루션의 프로젝트에서 직접 참조 하는 경우 라이브러리를 로드 하는 방법입니다.

합니다 **Xamarin.FormsBook.Platform** 프로젝트에 새 [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) 인터페이스입니다. 모든 메서드는 이제 이름의 `Async` 접미사 돌아가 `Task` 개체입니다.

**Xamarin.FormsBook.Platform.WinRT** 프로젝트를 포함 합니다 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) Windows 런타임에 대 한 클래스입니다.

**Xamarin.FormsBook.Platform.iOS** 프로젝트를 포함 합니다 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) iOS에 대 한 클래스입니다. 이제 이러한 메서드 비동기 여야 합니다. 에 정의 된 메서드의 비동기 버전을 사용 하 여 일부 메서드의 `StreamWriter` 하 고 `StreamReader`: [ `WriteAsync` ](xref:System.IO.StreamWriter.WriteAsync(System.String)) 하 고 [ `ReadToEndAsync` ](xref:System.IO.StreamReader.ReadToEndAsync)합니다. 다른 변환 결과를 `Task` 를 사용 하 여 개체를 [ `FromResult` ](xref:System.Threading.Tasks.Task.FromResult*) 메서드.

합니다 **Xamarin.FormsBook.Platform.Android** 프로젝트에 유사한 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) Android에 대 한 클래스입니다.

합니다 **Xamarin.FormsBook.Platform** 프로젝트도 포함 되어 있습니다를 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) 를 사용 하는 클래스는 `DependencyService` 개체입니다.

이러한 라이브러리를 사용 하려면 응용 프로그램 솔루션의 모든 프로젝트에 포함 해야 합니다 **Xamarin.FormsBook.Platform** 솔루션 및 각 응용 프로그램 프로젝트에는 해당 라이브러리에 대 한 참조가 있어야  **Xamarin.FormsBook.Platform**합니다.

합니다 [ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) 솔루션을 사용 하는 방법에 설명 합니다 **Xamarin.FormsBook.Platform** 라이브러리입니다. 각 프로젝트에 대 한 호출 `Toolkit.Init`합니다. 비동기 파일 i/o를 사용 하 여 응용 프로그램은 함수입니다.

### <a name="keeping-it-in-the-background"></a>백그라운드에서 지속적으로 업데이트

여러 비동기 메서드를 호출 하는 라이브러리의 메서드 &mdash; 와 같은 합니다 `WriteFileAsync` 하 고 `ReadFileASync` Windows 런타임의 메서드 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) 클래스 &mdash; 다소 만들 수 있습니다 사용 하 여 보다 효율적인 합니다 [ `ConfigureAwait` ](xref:System.Threading.Tasks.Task`1.ConfigureAwait(System.Boolean)) 방지 하는 사용자 인터페이스 스레드 전환 하는 방법.

### <a name="dont-block-the-ui-thread"></a>UI 스레드를 차단 하지 마십시오.

사용 하지 않도록에 캐시 하는 경우에 따라 `ContinueWith` 또는 `await` 사용 하 여 합니다 [ `Result` ](xref:System.Threading.Tasks.Task`1.Result) 메서드 속성입니다. UI 스레드를 차단할 수 있거나도 응용 프로그램이 중단에 대 한 피해 야 합니다.

## <a name="your-own-awaitable-methods"></a>사용자 고유의 awaitable 메서드

중 하나에 전달 하 여 일부 코드를 비동기적으로 실행할 수는 [ `Task.Run` ](xref:System.Threading.Tasks.Task.Run(System.Action)) 메서드. 호출할 수 있습니다 `Task.Run` 오버 헤드의 일부를 처리 하는 비동기 메서드 내에서.

다양 한 `Task.Run` 패턴은 아래에서 설명 합니다.

### <a name="the-basic-mandelbrot-set"></a>기본 Mandelbrot 집합

집합을 실시간으로 Mandelbrot 그릴 합니다 [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) 의 것과 유사한 구조를 `System.Numerics` 네임 스페이스입니다.

합니다 [ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) 샘플에는 `CalculateMandeblotAsync` 흑백 Mandelbrot 집합은 기본 계산을 사용 하 여 해당 코드 숨김 파일에서 메서드 [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)비트맵에 오도록 합니다.

### <a name="marking-progress"></a>진행률 표시

비동기 메서드에서 진행률 보고를 인스턴스화할 수 있습니다는 [ `Progress<T>` ](xref:System.Progress`1) 클래스 및 비동기 메서드가 있는 형식의 인수를 정의 [ `IProgress<T>` ](xref:System.IProgress`1)합니다. 에 설명 되어이 [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) 샘플입니다.

### <a name="cancelling-the-job"></a>작업 취소

또한을 취소할 수는 비동기 메서드를 작성할 수 있습니다. 라는 클래스를 사용 하 여 시작 하기 [ `CancellationTokenSource` ](xref:System.Threading.CancellationTokenSource)합니다. [ `Token` ](xref:System.Threading.CancellationTokenSource.Token) 속성을 사용 하면 형식의 값인 [ `CancellationToken` ](xref:System.Threading.CancellationToken)합니다. 이 비동기 함수에 전달 됩니다. 프로그램 호출을 [ `Cancel` ](xref:System.Threading.CancellationTokenSource.Cancel) 메서드의 `CancellationTokenSource` (일반적으로 사용자가 작업에 대 한 응답)에서 비동기 함수를 취소 하려면.

비동기 메서드를 정기적으로 확인 합니다 [ `IsCancellationRequested` ](xref:System.Threading.CancellationToken.IsCancellationRequested) 의 속성 `CancellationToken` 속성이 종료 하 고 `true`, 호출 또는 [ `ThrowIfCancellationRequested` ](xref:System.Threading.CancellationToken.ThrowIfCancellationRequested) 메서드에서 로 끝나는 경우 메서드는 [ `OperationCancelledException` ](xref:System.OperationCanceledException)합니다.

합니다 [ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) 취소 가능한 함수의 사용법을 보여 줍니다.

### <a name="an-mvvm-mandelbrot"></a>MVVM Mandelbrot

[ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) 샘플 보다 광범위 한 사용자 인터페이스가 있고 대부분 기반이 되는 [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) 하 고 [ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)클래스:

[![Mandelbrot X f 삼중 스크린 샷](images/ch20fg13-small.png "MVVM Mandelbrot")](images/ch20fg13-large.png#lightbox "MVVM Mandelbrot")

## <a name="back-to-the-web"></a>웹 돌아가기

합니다 [ `WebRequest` ](xref:System.Net.WebRequest) APM을 비동기 프로그래밍 모델을 호출 하는 해당 비동기 프로토콜을 사용 하 여 몇 가지 샘플에 사용 되는 클래스입니다. 이러한 클래스 중 하나를 사용 하 여 최신 탭 프로토콜을 변환할 수 있습니다는 `FromAsync` 의 메서드를 [ `TaskFactory` ](xref:System.Threading.Tasks.TaskFactory`1) 클래스입니다. 합니다 [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) 샘플에서는이 방법을 보여 줍니다.



## <a name="related-links"></a>관련 링크

- [20 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [20 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [파일 작업](~/xamarin-forms/app-fundamentals/files.md)
