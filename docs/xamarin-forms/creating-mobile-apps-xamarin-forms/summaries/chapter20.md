---
title: "요약 장 20입니다. Async 및 파일 I/O"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0ac316bc2cef04a80958c047427845dbdcc4137f
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>요약 장 20입니다. Async 및 파일 I/O

 그래픽 사용자 인터페이스는 순차적으로 사용자 입력 이벤트에 응답 해야 합니다. 이 사용자 입력 이벤트의 모든 처리가 라고도 하는 한 스레드에서 발생 해야 의미는 *주 스레드* 또는 *UI 스레드*합니다.

사용자가 그래픽 사용자 인터페이스가 응답을 기대 합니다. 이 프로그램이 사용자 입력 이벤트를 신속 하 게 처리 해야 한다는 것을 의미 합니다. 가능 하지 않은 다음 처리 보조 스레드 실행을 있어 해야 합니다.

이 가이드의 몇 가지 샘플 프로그램 사용 하 여 [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) 클래스입니다. 이 클래스는 [ `BeginGetReponse` ](https://developer.xamarin.com/api/member/System.Net.WebRequest.BeginGetResponse/p/System.AsyncCallback/System.Object/) 메서드는 완료 되 면 콜백 함수를 호출 하는 작업자 스레드를 시작 합니다. 그러나 해당 콜백 함수가 실행 작업자 스레드에서 프로그램 호출 해야 하므로 [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) 메서드 사용자 인터페이스에 액세스할 수 있습니다.

.NET 및 C#에서 비동기 처리를 보다 최신 접근 방법 ´ ù. 여기에 [ `Task` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task/) 및 [ `Task<TResult>` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task%3CTResult%3E/) 클래스 및 기타 형식으로는 [ `System.Threading` ](https://developer.xamarin.com/api/namespace/System.Threading/) 및 [ `System.Threading.Tasks` ](https://developer.xamarin.com/api/namespace/System.Threading.Tasks/) 네임 스페이스, C# 5.0 뿐만 아니라 `async` 및 `await` 키워드입니다. 이 장에서 중점적입니다.

## <a name="from-callbacks-to-await"></a>Await에 대 한 콜백을에서

`Page` 클래스 자체는 경고 상자를 표시 하는 세 개의 비동기 메서드를 포함 합니다.

- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) 반환 된 `Task` 개체
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/System.String/) 반환 된 `Task<bool>` 개체
- [`DisplayActionSheet`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) 반환 된 `Task<string>` 개체

`Task` 개체 이러한 메서드 구현 탭 이라고 하는 작업 기반 비동기 패턴에는 것을 나타냅니다. 이러한 `Task` 개체 메서드에서 신속 하 게 반환 됩니다. `Task<T>` 값 구성 "프라미스"는 반환 형식의 값 `TResult` 작업이 완료 될 때 사용할 수 있습니다. `Task` 반환 값은 반환 된 값이 없는 완전 하지만 비동기 동작을 나타냅니다.

이러한 모든 경우에는 `Task` 사용자 경고 상자를 해제할 때 완료 되었습니다.  

### <a name="an-alert-with-callbacks"></a>콜백 사용 경고

[ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) 샘플에서는 처리 하는 방법을 보여 줍니다. `Task<bool>` 개체를 반환 하 고 `Device.BeginInvokeOnMainThread` 콜백 메서드를 사용 하 여 호출 합니다.

### <a name="an-alert-with-lambdas"></a>람다에서와 경고

[ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) 샘플에서는 처리에 대 한 익명 람다 함수를 사용 하는 방법을 보여 줍니다. `Task` 및 `Device.BeginInvokeOnMainThread` 호출 합니다.  

### <a name="an-alert-with-await"></a>Await와 경고

더 간단한 방법은 아니지만 `async` 및 `await` C# 5에 도입 된 키워드입니다. [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) 샘플 사용을 보여 줍니다.

### <a name="an-alert-with-nothing"></a>아무 것도 없으면 경고

비동기 메서드에서 반환 되 면 `Task` 대신 `Task<TResult>`, 비동기 작업이 완료 될 때를 알 필요가 없는 경우 이러한 기술 중 하나를 사용 하는 프로그램 필요 하지 않습니다. [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) 이 샘플을 보여 줍니다.

### <a name="saving-program-settings-asynchronously"></a>프로그램 설정을 저장 하는 비동기적으로

[ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) 샘플의 사용법을 보여줍니다는 [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) 방식의 `Application` 없이 변경 될 때 프로그램 설정을 저장 하려면 재정의 `OnSleep` 메서드.

### <a name="a-platform-independent-timer"></a>플랫폼 독립적인 타이머

사용 하는 것이 불가능 [ `Task.Delay` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Delay/p/System.Int32/) 플랫폼 독립적인 타이머를 만들 수 있습니다. [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) 이 샘플을 보여 줍니다.

## <a name="file-inputoutput"></a>파일 입/출력

일반적으로.NET [ `System.IO` ](https://developer.xamarin.com/api/namespace/System.IO/) 네임 스페이스를 소스 파일 I/O 지원 되었습니다. 이 네임 스페이스의 일부 메서드를 비동기 작업을 지원 하지만 대부분은 그렇지 않습니다. 네임 스페이스는 복잡 한 파일 I/O 기능을 수행 하는 몇 가지 간단한 메서드 호출도 지원 합니다.

### <a name="good-news-and-bad-news"></a>다행히도 및 나쁜 소식

Xamarin.Forms 지원 응용 프로그램에 대 한 로컬 저장소에서 지 원하는 모든 플랫폼 &mdash; 은 응용 프로그램에 개인 저장소.

Xamarin.iOS 및 Xamarin.Android 라이브러리의 Xamarin에이 두 플랫폼에 맞게 명시적으로.NET 버전을 포함 합니다. 클래스를 포함 하는 이러한 `System.IO` 이 두 플랫폼에서 응용 프로그램의 로컬 저장소와 파일 I/O를 수행 하는 데 사용할 수 있는 합니다.

그러나 이러한 검색 하는 경우 `System.IO` Xamarin.Forms PCL에 클래스, 없습니다 찾아서 있습니다. 이 문제는 Windows 런타임 API에 대 한 해당 Microsoft 완전히 새롭게 수정 파일 I/O. Windows 8.1, Windows Phone 8.1, 및 유니버설 Windows 플랫폼을 대상으로 하는 프로그램을 사용 하지 않는 `System.IO` 파일 I/O에 대 한 합니다.

즉, 사용 해야 합니다는 [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) (먼저에 설명 된 [ **장 9입니다. 플랫폼별 API 호출** ](chapter09.md) 파일 I/O를 구현 하 합니다.

### <a name="a-first-shot-at-cross-platform-file-io"></a>플랫폼 간 파일 I/O에서 첫 번째 샷

[ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) 샘플 정의 [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) 파일 I/O 및 모든 플랫폼에서이 인터페이스의 구현에 대 한 인터페이스입니다. 그러나 Windows 런타임 파일 I/O 메서드는 비동기 Windows 런타임 구현 방법을이 인터페이스에서 사용 하 여 작동 하지 않습니다.

### <a name="accommodating-windows-runtime-file-io"></a>Windows 런타임 파일 I/O를 수용합니다.

클래스를 사용 하는 Windows 런타임에서 실행 되는 프로그램의 [ `Windows.Storage` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.aspx) 및 [ `Windows.Storage.Streams` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.aspx) I/O 응용 프로그램의 로컬 저장소를 포함 하 여 파일에 대 한 네임 스페이스입니다. 확인할 때문에 Microsoft는 50 개 이상의 밀리초는 UI 스레드를 차단 하지 않기 위해 비동기 있어야 합니다. 필요한 모든 작업에는 이러한 파일 I/O 메서드는 비동기 대부분.

다른 응용 프로그램에서 사용할 수 있도록 라이브러리에이 새로운 접근 방식을 보여 주는 코드가 됩니다.

## <a name="platform-specific-libraries"></a>플랫폼별 라이브러리

라이브러리에 다시 사용할 수 있는 코드를 저장 하는 것이 유용 합니다. 이 완전히 다른 운영 체제에 대 한 재사용 가능한 코드의 다른 부분 있는 경우에 훨씬 더 어렵습니다.

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) 솔루션에는 한 가지 방법을 보여 줍니다. 이 솔루션에는 7 개의 다른 프로젝트가 포함 됩니다.

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), a normal Xamarin.Forms PCL
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), iOS 클래스 라이브러리
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), Android 클래스 라이브러리
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), 유니버설 Windows 클래스 라이브러리
- [**Xamarin.FormsBook.Platform.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Windows), Windows 8.1 대 한 PCL입니다.
- [**Xamarin.FormsBook.Platform.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinPhone), Windows Phone 8.1에 대 한 PCL
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), 모든 Windows 플랫폼에 공통적으로 적용 되는 코드에 대 한 공유 프로젝트

모든 개별 플랫폼 프로젝트 (제외 **Xamarin.FormsBook.Platform.WinRT**)에 대 한 참조가 **Xamarin.FormsBook.Platform**합니다. 세 개의 Windows 프로젝트에 대 한 참조를가지고 **Xamarin.FormsBook.Platform.WinRT**합니다.

모든 프로젝트에는 정적 포함 `Toolkit.Init` 메서드를 직접 Xamarin.Forms 응용 프로그램 솔루션의 프로젝트에서 참조 하는 경우 라이브러리를 로드 합니다.

**Xamarin.FormsBook.Platform** 프로젝트에 새 [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) 인터페이스입니다. 모든 메서드는 이제 이름이 있습니다 `Async` 접미사 및 반환 `Task` 개체입니다.

**Xamarin.FormsBook.Platform.WinRT** 프로젝트에 포함 된 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) Windows 런타임에 대 한 클래스입니다.

**Xamarin.FormsBook.Platform.iOS** 프로젝트에 포함 된 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) iOS에 대 한 클래스입니다. 이러한 메서드는 비동기식 이어야 합니다. 에 정의 된 메서드의 비동기 버전을 사용 하 여 메서드의 `StreamWriter` 및 `StreamReader`: [ `WriteAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamWriter.WriteAsync/p/System.String/) 및 [ `ReadToEndAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamReader.ReadToEndAsync()/)합니다. 다른 변환 결과를 한 `Task` 를 사용 하 여 개체는 [ `FromResult` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.FromResult%7BTResult%7D/p/TResult/) 메서드.

**Xamarin.FormsBook.Platform.Android** 프로젝트에 비슷한 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) Android에 대 한 클래스입니다.

**Xamarin.FormsBook.Platform** 프로젝트도 포함 되어는 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) 쉽게의 사용할 수 있는 클래스는 `DependencyService` 개체입니다.

이러한 라이브러리를 사용 하려면 응용 프로그램 솔루션의 모든 프로젝트를 포함 해야는 **Xamarin.FormsBook.Platform** 솔루션 및 각 응용 프로그램 프로젝트에는 해당 라이브러리에 대 한 참조가 있어야  **Xamarin.FormsBook.Platform**합니다.

[ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) 솔루션 사용 방법을 보여 줍니다.는 **Xamarin.FormsBook.Platform** 라이브러리입니다. 각 프로젝트에 대 한 호출에는 `Toolkit.Init`합니다. 응용 프로그램에서 비동기 파일 I/O를 활용 함수입니다.

### <a name="keeping-it-in-the-background"></a>백그라운드에서 유지

여러 비동기 메서드를 호출 하는 라이브러리의 메서드 &mdash; 와 같은 `WriteFileAsync` 및 `ReadFileASync` 메서드는 Windows 런타임에서 [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) 클래스 &mdash; 다소 만들 수 있습니다 사용 하 여 보다 효율적인는 [ `ConfigureAwait` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task%3CTResult%3E.ConfigureAwait/p/System.Boolean/) 사용자 인터페이스 스레드로 전환 되지 않게 하려면.

### <a name="dont-block-the-ui-thread"></a>UI 스레드를 차단 하지 않습니다!

사용을 방지 하기 위해 캐시 하는 경우에 따라 `ContinueWith` 또는 `await` 를 사용 하 여는 [ `Result` ](https://developer.xamarin.com/api/property/System.Threading.Tasks.Task%3CTResult%3E.Result/) 방법에 대 한 속성입니다. 이 응용 프로그램을 중지 하거나 UI 스레드를 차단할 수 있는 것에 대 한 피해 야 합니다.

## <a name="your-own-awaitable-methods"></a>사용자 고유의 awaitable 메서드

중 하나에 전달 하 여 일부 코드를 비동기적으로 실행할 수는 [ `Task.Run` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Run/p/System.Action/) 메서드. 호출할 수 있습니다 `Task.Run` 에서 일부 오버 헤드를 처리 하는 비동기 메서드 내에서.

다양 한 `Task.Run` 패턴을 아래에 설명 합니다.

### <a name="the-basic-mandelbrot-set"></a>기본 Mandelbrot 집합

집합을 실시간으로 Mandelbrot 그리려면는 [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) 구조에 있는 것과 유사한는 `System.Numerics` 네임 스페이스입니다.

[ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) 샘플에는 `CalculateMandeblotAsync` 메서드는 기본 흑백 Mandelbrot 집합을 계산 하 고 사용 하는 코드 숨김 파일에 [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)비트맵에 넣을 수 있습니다.

### <a name="marking-progress"></a>진행률 표시

비동기 메서드에서 진행률 보고를 인스턴스화할 수 있습니다는 [ `Progress<T>` ](https://developer.xamarin.com/api/type/System.Progress%3CT%3E/) 클래스 형식의 인수를 하기로 하 여 비동기 메서드를 정의 및 [ `IProgress<T>` ](https://developer.xamarin.com/api/type/System.IProgress%3CT%3E/)합니다. 이 확인할는 [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) 샘플.

### <a name="cancelling-the-job"></a>작업 취소

취소할 수 하는 비동기 메서드를 작성할 수 있습니다. 라는 클래스를 사용 하 여 하기 [ `CancellationTokenSource` ](https://developer.xamarin.com/api/type/System.Threading.CancellationTokenSource/)합니다. [ `Token` ](https://developer.xamarin.com/api/property/System.Threading.CancellationTokenSource.Token/) 속성 형식의 값은 [ `CancellationToken` ](https://developer.xamarin.com/api/type/System.Threading.CancellationToken/)합니다. 이 비동기 함수에 전달 됩니다. 프로그램이 호출 하는 [ `Cancel` ](https://developer.xamarin.com/api/member/System.Threading.CancellationTokenSource.Cancel()/) 메서드 `CancellationTokenSource` (일반적으로 사용자가 수행한 동작에 대 한 응답)에서 비동기 함수를 취소 합니다.

비동기 메서드를 주기적으로 확인할 수는 [ `IsCancellationRequested` ](https://developer.xamarin.com/api/property/System.Threading.CancellationToken.IsCancellationRequested/) 속성 `CancellationToken` 속성이 이면 종료 `true`, 호출 또는 [ `ThrowIfCancellationRequested` ](https://developer.xamarin.com/api/member/System.Threading.CancellationToken.ThrowIfCancellationRequested()/) 메서드, 로 끝나는 경우 메서드는 [ `OperationCancelledException` ](https://developer.xamarin.com/api/type/System.OperationCanceledException/)합니다.

[ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) 샘플 취소 가능한 함수를 사용 하는 방법을 보여 줍니다.

### <a name="an-mvvm-mandelbrot"></a>MVVM Mandelbrot

[ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) 샘플 더 광범위 한 사용자 인터페이스가 있으며 대부분 기반으로 한 [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) 및 [ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)클래스:

[![Mandelbrot X F의 삼중 스크린 샷](images/ch20fg13-small.png "MVVM Mandelbrot")](images/ch20fg13-large.png#lightbox "MVVM Mandelbrot")

## <a name="back-to-the-web"></a>웹 돌아가기

[ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) 일부 샘플에 사용 되는 클래스는 비동기 프로그래밍 모델 또는 APM 구식 비동기 프로토콜을 사용 합니다. 이러한 클래스 중 하나를 사용 하 여 최신 TAP 프로토콜 변환할 수 있습니다는 `FromAsync` 의 메서드는 [ `TaskFactory` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskFactory%3CTResult%3E/) 클래스입니다. [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) 이 샘플을 보여 줍니다.



## <a name="related-links"></a>관련 링크

- [장 20 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [20 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [파일 작업](~/xamarin-forms/app-fundamentals/files.md)
