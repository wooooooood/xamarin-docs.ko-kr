---
title: 백그라운드 전송 및 NSURLSession Xamarin.iOS에
description: 이 문서에서는 백그라운드 전송 및 NSUrlSession 큰 이미지에 대 한 다운로드를 시작 하 고 백그라운드에서 응용 프로그램을 배치 하면 해당 다운로드 계속를 사용 하는 방법을 보여 주는 연습을 제공 합니다.
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 08a0ba1337c0d28d1f0d60d04394ccaf4a9ccfc7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783741"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>백그라운드 전송 및 NSURLSession Xamarin.iOS에

백그라운드 전송은 배경을 구성 하 여 시작 됩니다 `NSURLSession` 및 업로드 또는 다운로드 작업 큐에 삽입 합니다. IOS 응용 프로그램의 완료 처리기를 호출 하 여 응용 프로그램에 알리는 응용 프로그램 backgrounded, 일시 중단 또는 종료 하는 동안 태스크가 완료 되 면 경우 *AppDelegate*합니다. 다음 다이어그램에서는 작업에서이 보여 줍니다.

 [![](background-transfer-walkthrough-images/transfer.png "백그라운드 전송 배경 NSURLSession를 구성 하 여 시작 되 고 다시 업로드 또는 다운로드 작업")](background-transfer-walkthrough-images/transfer.png#lightbox)

이 기능 코드에서 살펴보겠습니다.

## <a name="configuring-a-background-session"></a>백그라운드 세션 구성

백그라운드 세션을 하려면 새 만들고 `NSUrlSession` 사용 하 여 구성 하 고는 `NSUrlSessionConfiguration` 개체입니다.

세션 수행할 수 있는 구성 개체를 결정 하 고 실행할 수 종류의 작업.
사용 하 여 구성 된 세션은 `CreateBackgroundSessionConfiguration` 메서드가 별도 프로세스에서 실행 되 고 데이터 및 배터리 수명을 유지 하기 위해 임의 (WiFi) 전송을 수행 합니다.
다음 코드 예제에서는 적절 한 설정을 사용 하 여 백그라운드 전송 세션의는 `CreateBackgroundSessionConfiguration` 메서드와 고유 문자열 식별자:

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

세션 구성 개체 외에도 세션 대리자와 큐도 필요합니다.
큐는 작업을 수행 합니다 순서를 결정 합니다. 세션 대리자 전송 프로세스 및 인증을 처리, 캐싱 및 세션 관련 기타 문제 chaperones 합니다.

## <a name="working-with-tasks-and-delegates"></a>작업 대리자와 작업

이제는 백그라운드 세션을 구성한 전송을 처리 하는 작업을 시작 해 보겠습니다. 우리는 추적 수를 사용 하 여 이러한 작업은는 `NSUrlSessionDelegate` 인스턴스 세션 대리자를 호출 합니다. 세션 대리자는 종료 또는 일시 중지 하는 응용 프로그램 백그라운드에서 핸들 인증, 오류 또는 전송 완료를 다시 시작 합니다.

`NSUrlSessionDelegate` 전송 상태를 확인 하려면 다음과 같은 기본 방법을 제공 합니다.

-  *DidFinishEventsForBackgroundSession* -이 메서드는 모든 작업을 완료 하 고 전송이 완료 될 때 호출 됩니다.
-  *DidReceiveChallenge* -인증이 필요한 경우 요청 자격 증명을 호출 합니다.
-  *DidBecomeInvalidWithError* -경우 호출된 되는 `NSURLSession` 무효화 됩니다.


백그라운드 세션 더 특수 한 대리자를 실행 하는 작업의 형식에 따라 필요 합니다. 백그라운드 세션 두 종류의 작업으로 제한 됩니다.

-  *작업 업로드* -형식의 작업 `NSUrlSessionUploadTask` 사용는 `NSUrlSessionTaskDelegate` 에서 상속한 `NSUrlSessionDelegate` 합니다. 이 대리자를 추적 하는 추가 방법을 업로드 진행률, 핸들 HTTP 리디렉션 등을 제공 합니다.
-  *다운로드 작업* -형식의 작업 `NSUrlSessionDownloadTask` 사용는 `NSUrlSessionDownloadDelegate` 에서 상속한 `NSUrlSessionTaskDelegate` 합니다. 이 대리자에 대 한 모든 방법 업로드 작업 뿐만 아니라 다운로드 진행률을 추적 하 고 다운로드 작업에 다시 시작 또는 완료 된 시기를 결정 하는 다운로드 전용 메서드를 제공 합니다.


다음 코드는 URL에서 이미지를 다운로드 하는 데 사용할 수 있는 작업을 정의 합니다. 호출 하 여 작업 시작과 म `CreateDownloadTask` 우리의 백그라운드 세션 및 URL 요청을 전달 합니다.

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

다음으로이 세션의 모든 다운로드 작업을 추적할 새 세션 다운로드 대리자를 만들겠습니다.

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

재정의할 수 있습니다은 다운로드 작업의 진행률을 확인 하고자 하는 경우는 `DidWriteData` 메서드를 진행률을 추적 하 고 포함 하 여 UI를 업데이트 합니다. UI 업데이트는 응용 프로그램을 전경에 되었거나 응용 프로그램을 열 때 다음에 사용자에 대 한 대기는 경우 즉시 표시 됩니다.

세션 대리자 API 작업과 상호 작용 하기 위한 광범위 한 도구 키트를 제공 합니다. 세션의 전체 목록에 대 한 메서드를 대리자를 가리키는 `NSUrlSessionDelegate` API 설명서입니다.

> [!IMPORTANT]
> 모든 호출에서는 UI 업데이트를 명시적으로 실행 해야 UI 스레드에서 호출 하 여 하므로 백그라운드 스레드에서 백그라운드 세션을 시작한 `InvokeOnMainThread` iOS 응용 프로그램 종료를 방지 하려면. 


## <a name="handling-transfer-completion"></a>처리 전송 완료

마지막 단계는 응용 프로그램 세션에 연결 된 모든 작업이 완료 될 때 알 수 있도록 하 고 새 콘텐츠를 처리 합니다.

에 *AppDelegate*를 구독 하는 `HandleEventsForBackgroundUrl` 이벤트입니다. 응용 프로그램이 백그라운드를 입력 하 고 전송 세션이 실행 되 고,이 메서드가 호출 되 고 완료 처리기 우리 시스템이 통과:

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

म 완료 처리기 하는 데 사용할 iOS 응용 프로그램이 완료 되 면 알고 처리 합니다.

세션 전송을 처리 하도록 여러 작업을 생성할 수는 점에 유의 하세요. 마지막 작업이 완료 되 면 일시 중단 또는 종료 된 응용 프로그램은를 배경으로 다시 시작 합니다. 응용 프로그램에 다시 연결 된 `NSURLSession` 고유 세션 식별자 및 호출을 사용 하 여 `DidFinishEventsForBackgroundSession` 세션 대리자에 있습니다. 이 메서드는 전송의 결과 반영 하도록 UI 업데이트를 포함 하 여 새 콘텐츠를 처리 하는 응용 프로그램의 기회:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

새 콘텐츠 처리를 완료 한 후 완료 처리기에 응용 프로그램의 스냅숏을 만들고 절전 모드로 돌아갑니다를 안전 하 게 알고 시스템 이라고 합니다.

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

이 연습에서는 iOS 7에서에서 백그라운드 전송 서비스를 구현 하는 기본 단계 포함 되어 있습니다.



## <a name="related-links"></a>관련 링크

- [간단한 백그라운드 전송 (샘플)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
