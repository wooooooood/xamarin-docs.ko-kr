---
title: 백그라운드 전송 및 NSURLSession Xamarin.iOS에서
description: 이 문서는 백그라운드 전송 및 NSUrlSession를 큰 이미지의 다운로드를 시작 하 고 앱이 백그라운드에서 배치 되 면 해당 다운로드를 계속 사용 하는 방법을 보여 주는 연습을 제공 합니다.
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4e525388290d92901e68e61f1ffa81866f5aac4d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61392216"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>백그라운드 전송 및 NSURLSession Xamarin.iOS에서

백그라운드 전송 배경이 구성 하 여 시작 됩니다 `NSURLSession` 및 업로드 또는 다운로드 작업 큐에 넣어야 합니다. IOS 응용 프로그램의 완료 처리기를 호출 하 여 응용 프로그램을 알려 작업 동안 응용 프로그램 backgrounded, 일시 중단 또는 종료를 완료 하는 경우 *AppDelegate*합니다. 다음 다이어그램에서는 작업에서이 보여 줍니다.

 [![](background-transfer-walkthrough-images/transfer.png "백그라운드 전송 배경이 NSURLSession를 구성 하 여 시작 되 고 큐에 넣어야 업로드 또는 다운로드 작업")](background-transfer-walkthrough-images/transfer.png#lightbox)

이 모양을 코드를 확인해 보겠습니다.

## <a name="configuring-a-background-session"></a>백그라운드 세션 구성

백그라운드 세션을 만들려면 새 `NSUrlSession` 사용 하 여 구성 하 고는 `NSUrlSessionConfiguration` 개체입니다.

실행 작업의 종류 및 구성 개체를 세션에서 수행할 수 있는 작업을 결정 합니다.
사용 하 여 구성 된 세션을 `CreateBackgroundSessionConfiguration` 메서드는 별도 프로세스에서 실행 되 고 데이터 및 배터리 수명 유지 하기 위해 임의 (WiFi) 전송을 수행 합니다.
다음 코드 샘플을 사용 하 여 백그라운드 전송 세션의 적절 한 설치를 `CreateBackgroundSessionConfiguration` 메서드와 고유한 문자열 식별자:

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

세션 구성 개체를 외에도 세션 대리자 및 큐도 필요합니다.
큐는 작업은 완료 하는 순서를 결정 합니다. 세션 대리자 전송 프로세스 및 인증을 처리, 캐싱 및 세션 관련 된 문제 기타 chaperones 합니다.

## <a name="working-with-tasks-and-delegates"></a>작업 및 대리자 사용

백그라운드 세션에서 구성한 했으므로 전송을 처리 하는 작업을 시작 해 보겠습니다. 에서는 추적할 수를 사용 하 여 이러한 작업을는 `NSUrlSessionDelegate` 인스턴스 세션 대리자를 호출 합니다. 세션 대리자는 깨울 종료 또는 일시 중단 된 응용 프로그램을 백그라운드에서 핸들 인증, 오류 또는 전송 완료 하는 일을 담당 합니다.

`NSUrlSessionDelegate` 전송 상태를 확인 하려면 다음 기본 메서드를 제공 합니다.

-  *DidFinishEventsForBackgroundSession* -이 메서드는 모든 작업을 완료 하 고 전송이 완료 될 때 호출 됩니다.
-  *DidReceiveChallenge* -인증이 필요한 경우 요청 자격 증명을 호출 합니다.
-  *DidBecomeInvalidWithError* -경우에 호출된 된 `NSURLSession` 무효화 됩니다.


백그라운드 세션 보다 특수화 된 대리자를 실행 하는 작업의 형식에 따라 필요 합니다. 백그라운드 세션 두 종류의 작업으로 제한 됩니다.

-  *작업을 업로드* -형식의 작업 `NSUrlSessionUploadTask` 사용 합니다 `NSUrlSessionTaskDelegate` 에서 상속 하는 `NSUrlSessionDelegate` 합니다. 이 대리자를 추적 하는 추가 방법 업로드 진행률, 핸들 HTTP 리디렉션 및 기타 정보를 제공 합니다.
-  *작업을 다운로드할* -형식의 작업 `NSUrlSessionDownloadTask` 사용 합니다 `NSUrlSessionDownloadDelegate` 에서 상속 하는 `NSUrlSessionTaskDelegate` 합니다. 이 대리자에 대 한 모든 메서드 업로드 작업 뿐만 아니라 다운로드 진행률을 추적 하 고 다운로드 작업이 다시 시작 되거나 완료 시기를 결정 하는 다운로드 전용 메서드를 제공 합니다.


다음 코드는 URL에서 이미지를 다운로드 하려면 사용할 수 있는 작업을 정의 합니다. 호출 하 여 작업을 시작 했습니다 `CreateDownloadTask` 는 백그라운드 세션에 URL 요청을 전달 합니다.

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

다음으로이 세션의 모든 다운로드 작업을 추적할 새 세션 다운로드 대리자를 만듭니다.

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

다운로드 작업의 진행률을 확인 하고자 하는 경우 재정의할 수 있습니다는 `DidWriteData` UI를 업데이트 하는 방법 및 진행률을 추적 합니다. UI 업데이트는 응용 프로그램을 전경에 되었거나 응용 프로그램을 열면 다음에 사용자에 대 한 대기를 하는 경우 즉시 표시 됩니다.

세션 대리자 API 작업을 사용 하 여 상호 작용 하기 위한 광범위 한 도구 키트를 제공 합니다. 세션의 전체 목록은 메서드를 대리자, 참조는 `NSUrlSessionDelegate` API 설명서입니다.

> [!IMPORTANT]
> 호출 하 여 UI 스레드에서 UI 업데이트에 대 한 모든 호출을 명시적으로 실행 될 해야 하므로 백그라운드 스레드에서 백그라운드 세션이 시작 됩니다 `InvokeOnMainThread` iOS 앱을 종료 하지 않아도 됩니다. 


## <a name="handling-transfer-completion"></a>처리 전송 완료

마지막 단계는 응용 프로그램 세션에 연결 된 모든 작업이 완료 될 때 알 수 있도록 하 고 새 콘텐츠를 처리 합니다.

에 *AppDelegate*, 구독을 `HandleEventsForBackgroundUrl` 이벤트. 응용 프로그램이 백그라운드 입력을 전송 세션이 실행 되 고이 메서드가 호출 되 고 미국 시스템이 완료 처리기를 제공 하는 통과:

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

완료 처리기를 사용 하 여 iOS 응용 프로그램 완료 되 면 알 수 있도록 처리 합니다.

세션 전송을 처리 하도록 여러 작업을 생성할 수 기억 하십시오. 마지막 작업이 완료 되 면 종료 또는 일시 중단 된 응용 프로그램을 백그라운드에 다시 시작 됩니다. 그런 다음 응용 프로그램 다시 연결할 합니다 `NSURLSession` 호출이 고 고유한 세션 식별자를 사용 하 여 `DidFinishEventsForBackgroundSession` 세션 대리자에서. 이 메서드는 응용 프로그램의 전송의 결과 반영 하도록 UI 업데이트를 포함 하 여 새 콘텐츠를 처리할 수 있게 합니다.

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

새 콘텐츠 처리를 마치고 나면 이라고 완료 처리기가 시스템에 안전 하 게 응용 프로그램의 스냅숏을 만들고 절전 모드로 돌아갑니다 알립니다.

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

이 연습에서는 iOS 7에서에서 백그라운드 전송 서비스를 구현 하는 기본 단계에 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [간단한 백그라운드 전송 (샘플)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
