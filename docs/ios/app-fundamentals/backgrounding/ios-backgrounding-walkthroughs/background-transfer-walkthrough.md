---
title: Xamarin.ios의 백그라운드 전송 및 NSURLSession
description: 이 문서에서는 백그라운드 전송 및 NSUrlSession를 사용 하 여 대량 이미지의 다운로드를 시작 하는 방법을 보여 주는 연습을 제공 하 고 앱이 백그라운드에 배치 될 때 해당 다운로드를 계속 합니다.
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 3e27cffa9e2605c3697536f226fe87fbbf1bfbbd
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286888"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>Xamarin.ios의 백그라운드 전송 및 NSURLSession

백그라운드 전송은 백그라운드 `NSURLSession` 를 구성 하 고 예외로 인해 업로드 또는 다운로드 작업을 시작 합니다. 응용 프로그램이 backgrounded, suspended 또는 종료 된 상태에서 작업이 완료 되 면 iOS는 응용 프로그램의 *AppDelegate*에서 완료 처리기를 호출 하 여 응용 프로그램에 알립니다. 다음 다이어그램에서는이를 수행 하는 방법을 보여 줍니다.

 [![](background-transfer-walkthrough-images/transfer.png "백그라운드 전송은 백그라운드 NSURLSession 및 예외로 인해 업로드 또는 다운로드 작업을 구성 하 여 시작 됩니다.")](background-transfer-walkthrough-images/transfer.png#lightbox)

코드에 표시 되는 모양을 살펴보겠습니다.

## <a name="configuring-a-background-session"></a>백그라운드 세션 구성

백그라운드 세션을 만들려면 새 `NSUrlSession` 를 만들고 `NSUrlSessionConfiguration` 개체를 사용 하 여 구성 합니다.

구성 개체는 세션에서 수행할 수 있는 작업과 실행할 수 있는 태스크의 종류를 결정 합니다.
메서드를 `CreateBackgroundSessionConfiguration` 사용 하 여 구성 된 세션은 별도의 프로세스에서 실행 되며, 데이터 및 배터리 수명을 보존 하기 위해 임의의 (WiFi) 전송을 수행 합니다.
다음 코드 샘플에서는 `CreateBackgroundSessionConfiguration` 메서드와 고유 문자열 식별자를 사용 하 여 백그라운드 전송 세션을 적절 하 게 설정 하는 방법을 보여 줍니다.

```csharp
public partial class SimpleBackgroundTransferViewController : UIViewController
{
  NSUrlSession session = null;

  NSUrlSessionConfiguration configuration =
      NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.SimpleBackgroundTransfer.BackgroundSession");
  session = NSUrlSession.FromConfiguration
      (configuration, (NSUrlSessionDelegate) new MySessionDelegate(), new NSOperationQueue());

}
```

구성 개체와는 별도로 세션에도 세션 대리자와 큐가 필요 합니다.
큐는 작업이 완료 되는 순서를 결정 합니다. 세션 대리자는 전송 프로세스를 chaperones 하 고 인증, 캐싱 및 기타 세션 관련 문제를 처리 합니다.

## <a name="working-with-tasks-and-delegates"></a>작업 및 대리자 작업

백그라운드 세션을 구성 했으므로 이제 전송을 처리 하는 작업을 시작 하겠습니다. 세션 대리자 라는 `NSUrlSessionDelegate` 인스턴스를 사용 하 여 이러한 작업을 추적할 수 있습니다. 세션 대리자는 백그라운드에서 종료 되거나 일시 중단 된 응용 프로그램의 절전 모드를 해제 하 여 인증, 오류 또는 전송 완료를 처리 합니다.

는 `NSUrlSessionDelegate` 전송 상태를 확인 하는 다음과 같은 기본 메서드를 제공 합니다.

- *DidFinishEventsForBackgroundSession* -이 메서드는 모든 작업이 완료 되 고 전송이 완료 되 면 호출 됩니다.
- *DidReceiveChallenge* -권한 부여가 필요한 경우 자격 증명을 요청 하기 위해 호출 됩니다.
- *DidBecomeInvalidWithError* -가 `NSURLSession` 무효화 되 면 호출 됩니다.


백그라운드 세션에는 실행 중인 작업의 유형에 따라 보다 특수화 된 대리자가 필요 합니다. 백그라운드 세션은 다음과 같은 두 가지 유형의 작업으로 제한 됩니다.

- *작업 업로드* -형식의 `NSUrlSessionUploadTask` 작업은 `NSUrlSessionTaskDelegate` 에서 `NSUrlSessionDelegate` 상속 되는를 사용 합니다. 이 대리자는 업로드 진행률을 추적 하 고, HTTP 리디렉션을 처리 하는 등의 추가 메서드를 제공 합니다.
- *다운로드 작업* -형식의 `NSUrlSessionDownloadTask` 작업에서 `NSUrlSessionTaskDelegate` 상속 되 `NSUrlSessionDownloadDelegate` 는를 사용 합니다. 이 대리자는 다운로드 작업의 모든 메서드 뿐만 아니라 다운로드 진행률을 추적 하 고 다운로드 작업을 다시 시작 하거나 완료 한 시간을 결정 하는 다운로드 관련 방법을 제공 합니다.


다음 코드는 URL에서 이미지를 다운로드 하는 데 사용할 수 있는 작업을 정의 합니다. 백그라운드 세션에서를 호출 `CreateDownloadTask` 하 고 URL 요청을 전달 하 여 작업을 시작 합니다.

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

다음으로이 세션의 모든 다운로드 작업을 추적 하기 위해 새 세션 다운로드 대리자를 만듭니다.

```csharp
public class MySessionDelegate : NSUrlSessionDownloadDelegate
{
  public override void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
  {
    Console.WriteLine (string.Format ("DownloadTask: {0}  progress: {1}", downloadTask, progress));
    InvokeOnMainThread( () => {
      // update UI with progress bar, if desired
    });
  }
  ...
}
```

다운로드 작업의 진행 상황을 확인 하려면 메서드를 `DidWriteData` 재정의 하 여 진행률을 추적 하 고 UI도 업데이트할 수 있습니다. 응용 프로그램이 포그라운드에서 표시 되거나 다음에 응용 프로그램을 열 때 사용자가 대기 하는 경우 UI 업데이트가 즉시 표시 됩니다.

세션 대리자 API는 작업과 상호 작용 하기 위한 광범위 한 도구 키트를 제공 합니다. 세션 대리자 메서드의 전체 목록은 `NSUrlSessionDelegate` API 설명서를 참조 하세요.

> [!IMPORTANT]
> 백그라운드 세션은 백그라운드 스레드에서 시작 되므로 앱을 종료 하지 않도록를 호출 `InvokeOnMainThread` 하 여 ui 스레드에서 ui를 업데이트 하는 모든 호출을 명시적으로 실행 해야 합니다. 


## <a name="handling-transfer-completion"></a>전송 완료 처리

마지막 단계는 세션과 연결 된 모든 작업이 완료 되었을 때 응용 프로그램에 알 수 있도록 하 고 새 콘텐츠를 처리 하는 것입니다.

*AppDelegate*에서 `HandleEventsForBackgroundUrl` 이벤트를 구독 합니다. 응용 프로그램이 백그라운드에 들어가면 전송 세션이 실행 되는 동안이 메서드가 호출 되 고 시스템에서 완료 처리기를 전달 합니다.

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

응용 프로그램의 처리가 완료 되 면 iOS에서이를 알 수 있도록 완료 처리기를 사용 합니다.

세션에서 전송을 처리 하는 몇 가지 작업을 생성할 수 있음을 기억 합니다. 마지막 작업이 완료 되 면 일시 중단 또는 종료 된 응용 프로그램이 백그라운드에서 다시 시작 됩니다. 그런 다음 응용 프로그램은 고유한 세션 식별자를 `NSURLSession` 사용 하 여에 다시 연결 하 고 `DidFinishEventsForBackgroundSession` 세션 대리자에서를 호출 합니다. 이 메서드는 전송 결과를 반영 하도록 UI 업데이트를 포함 하 여 새 콘텐츠를 처리할 수 있는 응용 프로그램의 기회입니다.

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

새 콘텐츠 처리를 완료 한 후에는 완료 처리기를 호출 하 여 시스템에서 응용 프로그램의 스냅숏을 만들고 절전 모드로 돌아갈 수 있음을 알립니다.

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  var appDelegate = UIApplication.SharedApplication.Delegate as AppDelegate;

  // Handle new information, update UI, etc.

  // call completion handler when you're done
  if (appDelegate.backgroundSessionCompletionHandler != null) {
    NSAction handler = appDelegate.backgroundSessionCompletionHandler;
    appDelegate.backgroundSessionCompletionHandler = null;
    handler.Invoke ();
  }
}
```

이 연습에서는 iOS 7에서 백그라운드 전송 서비스를 구현 하는 기본 단계를 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [간단한 백그라운드 전송 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplebackgroundtransfer)
