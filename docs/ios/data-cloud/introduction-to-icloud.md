---
title: Xamarin.iOS와 iCloud를 사용 하 여
description: ICloud 및 Xamarin.iOS 응용 프로그램에서 사용 하는이 문서에 설명합니다. 키-값 저장소, 문서 저장소와 iCloud 백업에 설명 합니다.
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/09/2016
ms.openlocfilehash: d40c932c560b3827cccacd4f9bafa155dcf26056
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113328"
---
# <a name="using-icloud-with-xamarinios"></a>Xamarin.iOS와 iCloud를 사용 하 여

IOS 5에서에서 iCloud 저장소 API에는 응용 프로그램을을 중앙 위치로 사용자 문서 및 응용 프로그램별 데이터를 저장 하 고 모든 사용자 장치에서 해당 항목에 액세스할 수 있습니다.

사용 가능한 네 가지 유형의 저장소가 됩니다.

- **키-값 저장소** -에서 소량의 응용 프로그램을 사용 하 여 데이터를 공유 하는 사용자의 다른 장치 합니다.

- **UIDocument 저장소** -UIDocument의 서브 클래스를 사용 하 여 사용자의 iCloud 계정에서 문서 및 기타 데이터를 저장 합니다.

- **CoreData** -SQLite 데이터베이스 저장소입니다.

- **개별 파일 및 디렉터리** -많은 다른 파일이 파일 시스템에서 직접 관리에 대 한 합니다.

이 문서에서는 처음 두 (키-값 쌍 및 UIDocument 서브 클래스) 형식과 Xamarin.iOS에서 이러한 기능을 사용 하는 방법을 설명 합니다.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

## <a name="requirements"></a>요구 사항

- Xamarin.iOS의 안정적인 최신 버전
- Xcode 8 이상
- Visual Studio for Mac 또는 Visual Studio 2015 이상입니다.

## <a name="preparing-for-icloud-development"></a>ICloud 개발을 위한 준비

응용 프로그램에서 iCloud를 사용 하도록 구성 해야 합니다 [Apple 프로 비전 포털](https://developer.apple.com/account/ios/overview.action) 및 프로젝트 자체입니다. 전에 iCloud에 대 한 개발 (또는 샘플) 다음 단계를 수행 합니다.

ICloud에 액세스 하려면 응용 프로그램을 올바르게 구성 합니다.

-   **프로그램 팀 Id를 찾을** -에 로그인 [developer.apple.com](http://developer.apple.com) 방문 합니다 **Member Center > 계정 > 개발자 계정 요약** 에 팀 ID (또는 단일 개발자를 위한 개별 ID를 가져오려면 ). 10 자로 된 문자열 됩니다 ( **A93A5CM278** 예를 들어)-이 "컨테이너 식별자"의 일부를 형성 합니다.

-   **새 앱 ID를 만듭니다** -을 만들고 앱 ID에 설명 된 단계를 수행 합니다 [장치 프로 비전 가이드의 섹션 저장소 기술에 대 한 프로 비전](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)를 확인 해야 **iCloud** 으로 허용 되는 서비스:

 [![](introduction-to-icloud-images/icloud-sml.png "허용 되는 서비스와 iCloud 확인")](introduction-to-icloud-images/icloud.png#lightbox)

- **새 프로 비전 프로필을 만듭니다** -를 프로 비전 프로필 만들기에 설명 된 단계를 수행 합니다 [장치 프로 비전 가이드](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) 합니다.

- **Entitlements.plist에 컨테이너 식별자를 추가** -컨테이너 식별자 형식은 `TeamID.BundleID`합니다. 자세한 내용은 참조는 [자격](~/ios/deploy-test/provisioning/entitlements.md) 가이드입니다.

- **프로젝트 속성 구성** -Info.plist 파일 확인에 **번들 식별자** 일치 하는 **번들 ID** 인 경우에 설정 [앱 ID 만들기 ](~/ios/deploy-test/provisioning/capabilities/index.md); IOS 번들 서명 사용 하는 **프로 비전 프로필** 와 iCloud 앱 서비스 앱 ID를 포함 하는 및 **사용자 지정 자격** 파일을 선택 합니다. 이 모두에서 수행할 수 있습니다 Visual Studio에서 프로젝트 속성 창.

- **장치에서 iCloud를 사용 하도록 설정** 로 이동- **설정 > iCloud** 장치 로그인을 확인 합니다.
선택 하 고 설정 합니다 **문서 및 데이터** 옵션입니다.

- **ICloud 테스트 하려면 장치를 사용 해야** -시뮬레이터에서 작동 하지 것입니다.
사실 실제로 두 개 이상의 장치가 필요 모든 작업에서 iCloud에 보려는 동일한 Apple ID를 사용 하 여 로그인 합니다.


## <a name="key-value-storage"></a>키-값 저장소

키-값 저장소는 적은 양의 데이터 사용자 책 이나 잡지에 표시는 마지막 페이지와 같은 지속형된에서 장치-생각 하는 것입니다. 키-값 저장소 백업 데이터에 대 한 쓰일 수 없습니다.

가지 키-값 저장소를 사용 하는 경우 알아야 할 몇 가지 제한이 있습니다.

- **최대 키 크기** -키 이름은 64 바이트를 초과할 수 없습니다.

- **최 댓 값 크기** -64kb 이상의 단일 값을 저장할 수 없습니다.

- **앱에 대 한 키-값 저장소 최대 크기** -응용 프로그램 총에서 최대 64kb의 키-값 데이터를 저장할 수 있습니다. 해당 제한 초과 하는 키를 설정 하려고 하면 실패 하 고 이전 값이 유지 됩니다.

- **데이터 형식** -기본 형식만 같은 문자열, 숫자 및 부울 값을 저장할 수 있습니다.

합니다 **iCloudKeyValue** 예제에서는 작동 하는 방법을 보여 줍니다. 샘플 코드는 각 장치에 대 한 명명 된 키를 만듭니다: 하나의 장치에서이 키를 설정 하 고 다른 사용자에 게 전파 하는 값을 확인 수 있습니다. 또한 한 번에 여러 장치에서 편집 하는 경우 모든 장치에서 편집할 수 있는 "Shared" 라는 키를 생성 iCloud는 결정 "wins" (변경 내용에 대 한 타임 스탬프 사용) 값 및 전파를 가져옵니다.

이 스크린샷은 사용 하 여 샘플을 보여 줍니다. ICloud에서 변경 알림이 수신 될 때 화면 맨 아래에 있는 스크롤 텍스트 보기에 인쇄 되며 입력된 필드에서 업데이트 됩니다.



 [![](introduction-to-icloud-images/icloud-kv-arrows.png "장치 간의 메시지 흐름")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>설정 및 데이터를 검색 합니다.

이 코드는 문자열 값을 설정 하는 방법을 보여 줍니다.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

동기화를 호출 하면 값을 로컬 디스크 저장소로만 유지 됩니다. ICloud로 동기화 백그라운드에서 발생 하며 "강제 적용할 수 없습니다" 응용 프로그램 코드에서. 동기화는 네트워크 연결 상태가 정상으로 자주 업데이트 저하 (또는 연결 끊기) 네트워크 인 경우 훨씬 더 오래 걸릴 수 있지만 5 초 내 발생 합니다.

이 코드를 사용 하 여 값을 검색할 수 있습니다.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

로컬 데이터 저장소에서 값이 검색-이 방법을 "latest" 값을 검색할 iCloud 서버를 연결 하려고 시도 하지 않습니다. iCloud에는 로컬 데이터 저장소 자체 일정에 따라 업데이트 됩니다.

### <a name="deleting-data"></a>데이터 삭제

키-값 쌍을 완전히 제거 하려면 다음과 같이 Remove 메서드를 사용 합니다.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>변경 내용을 관찰합니다.

응용 프로그램 값을 관찰자를 추가 하 여 iCloud에서 변경 되 면 알림을 받을 수도 있습니다는 `NSNotificationCenter.DefaultCenter`합니다.
다음 코드에서 **KeyValueViewController.cs** `ViewWillAppear` 메서드는 키가 변경 되지 목록을 만들고 해당 알림을 수신 하는 방법을 보여 줍니다.

```csharp
keyValueNotification =
NSNotificationCenter.DefaultCenter.AddObserver (
    NSUbiquitousKeyValueStore.DidChangeExternallyNotification, notification => {
    Console.WriteLine ("Cloud notification received");
    NSDictionary userInfo = notification.UserInfo;

    var reasonNumber = (NSNumber)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangeReasonKey);
    nint reason = reasonNumber.NIntValue;

    var changedKeys = (NSArray)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangedKeysKey);
    var changedKeysList = new List<string> ();
    for (uint i = 0; i < changedKeys.Count; i++) {
        var key = changedKeys.GetItem<NSString> (i); // resolve key to a string
        changedKeysList.Add (key);
    }
    // now do something with the list...
});
```

코드의 로컬 복사본을 업데이트 하거나 새 값을 사용 하 여 UI를 업데이트 하는 등의 변경 된 키 목록으로 몇 가지 조치를 취할 수 있습니다.

변경 가능한 이유는: ServerChange (0), (1), InitialSyncChange QuotaViolationChange (2). 이유를 액세스 하 고 필요한 경우 다른 처리를 수행할 수 있습니다 (의 결과로 일부 키를 제거 해야 하는 예를 들어, 한 *QuotaViolationChange*).

## <a name="document-storage"></a>문서 저장소

iCloud 문서 저장소 앱과 사용자에 게 중요 한 데이터를 관리 하도록 설계 되었습니다. 파일 및 앱을 실행 하는 동안 동시 iCloud 기반 백업을 제공 하 고 모든 사용자 장치에서 기능을 공유 하는 다른 데이터 관리를 사용할 수 있습니다.

이 다이어그램에서는 서로 맞는지 그 방법을 보여 줍니다. 각 장치에 로컬 저장소 (UbiquityContainer) 및 디먼 클라우드에서 데이터를 보내고 처리 하는 운영 체제의 iCloud에 저장 된 데이터가 있습니다. 모든 파일에 액세스할 수는 UbiquityContainer FilePresenter/FileCoordinator 동시 액세스를 방지 하기 위해를 통해 수행 되어야 합니다. `UIDocument` 클래스를 구현 하기는; UIDocument를 사용 하는 방법을 보여 주는이 예제입니다.

 [![](introduction-to-icloud-images/icloud-overview.png "문서 저장소 개요")](introduction-to-icloud-images/icloud-overview.png#lightbox)

ICloudUIDoc 예제는 간단한 구현 `UIDocument` 단일 텍스트 필드가 포함 된 하위 클래스입니다. 텍스트를 렌더링 하는 `UITextView` 편집 빨간색으로 표시 하는 알림 메시지와 iCloud 다른 장치에서 전파 됩니다. 충돌 해결 등 고급 iCloud 기능을 사용 하 여 샘플 코드를 처리 하지 않습니다.

이 스크린샷은 텍스트를 변경 하 고 키를 눌러 후 샘플 응용 프로그램- **UpdateChangeCount** 문서를 통해 다른 장치를 icloud와 동기화 됩니다.

 [![](introduction-to-icloud-images/iclouduidoc.png "텍스트를 변경 하 고 UpdateChangeCount 키를 눌러 후 샘플 응용 프로그램을 보여 주는이 스크린샷")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

iCloudUIDoc 샘플 5 개 부분이 있습니다.

1. **액세스는 UbiquityContainer** -그렇다면와 iCloud를 사용 하는 경우 결정 응용 프로그램의 iCloud 저장소 영역에 대 한 경로입니다.

1. **UIDocument 하위 클래스를 만들어** -모델 개체와 iCloud 저장소 간을 중개 하는 클래스를 만듭니다.

1. **찾기 및 iCloud 문서 열기** -사용 `NSFileManager` 및 `NSPredicate` 를 icloud와 문서를 찾아서 열입니다.

1. **ICloud 문서 표시** -에서 속성을 노출 하 `UIDocument` UI 컨트롤과 상호 작용할 수 있도록 합니다.

1. **ICloud 문서 저장** -디스크와 iCloud에 UI에서 변경한 내용은 유지 되도록 합니다.

실행 (또는 실행할지) 모든 iCloud 작업이 비동기적으로 어떤 항목을 기다리는 동안 차단 하지 않습니다. 이 샘플에서 수행 하는 세 가지 방법으로 표시 됩니다.

 **스레드** - `AppDelegate.FinishedLaunching` 에 대 한 초기 호출 `GetUrlForUbiquityContainer` 주 스레드를 차단 하지 않으려면 다른 스레드에서 수행 됩니다.

 **NotificationCenter** 때 비동기 알림에 등록-등의 작업이 `NSMetadataQuery.StartQuery` 완료 합니다.

 **완료 처리기** 와 같은 비동기 작업 완료 시 실행 되는 방법을 전달- `UIDocument.Open`합니다.

### <a name="accessing-the-ubiquitycontainer"></a>UbiquityContainer 액세스

ICloud 문서 저장소를 사용 하 여 첫 번째 단계는 경우와 iCloud 사용 되는지 여부를 결정 하는 "보편화 컨테이너"의 위치 (장치에서 iCloud 지원 파일을 저장할 디렉터리).

이 코드는는 `AppDelegate.FinishedLaunching` 메서드 샘플입니다.

```csharp
// GetUrlForUbiquityContainer is blocking, Apple recommends background thread or your UI will freeze
ThreadPool.QueueUserWorkItem (_ => {
    CheckingForiCloud = true;
    Console.WriteLine ("Checking for iCloud");
    var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer (null);
    // OR instead of null you can specify "TEAMID.com.your-company.ApplicationName"

    if (uburl == null) {
        HasiCloud = false;
        Console.WriteLine ("Can't find iCloud container, check your provisioning profile and entitlements");

        InvokeOnMainThread (() => {
            var alertController = UIAlertController.Create ("No \uE049 available",
            "Check your Entitlements.plist, BundleId, TeamId and Provisioning Profile!", UIAlertControllerStyle.Alert);
            alertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Destructive, null));
            viewController.PresentViewController (alertController, false, null);
        });
    } else { // iCloud enabled, store the NSURL for later use
        HasiCloud = true;
        iCloudUrl = uburl;
        Console.WriteLine ("yyy Yes iCloud! {0}", uburl.AbsoluteUrl);
    }
    CheckingForiCloud = false;
});
```

이 샘플에서는 그렇게 수행 하지 않지만 Apple GetUrlForUbiquityContainer를 호출 하는 응용 프로그램은 전경으로 때마다 것이 좋습니다.

### <a name="creating-a-uidocument-subclass"></a>UIDocument 하위 클래스를 만들어

모든 iCloud 파일 및 디렉터리 (ie. UbiquityContainer 디렉터리에 저장 된 모든 항목) NSFilePresenter 프로토콜을 구현 하 고는 NSFileCoordinator 통해 작성 NSFileManager 메서드를 사용 하 여 관리 해야 합니다.
사용자가 직접 있지만 UIDocument 하는가 하는 하위 클래스입니다. 쓸 수는 없습니다는 모든 작업을 수행 하는 가장 간단한 방법은 모든 합니다.

ICloud를 사용 하려면 UIDocument 하위 클래스에서 구현 해야 하는 방법은 두 가지가 있습니다.

- **LoadFromContents** -NSData 사용자 모델 클래스 es를 개봉 하에 대 한 파일의 콘텐츠를 전달 합니다.

- **ContentsForType** -디스크 (및 클라우드)에 저장 하 여 모델 클래스 es NSData 표현을 제공 하에 대 한 요청입니다.

이 샘플 코드 **iCloudUIDoc\MonkeyDocument.cs** UIDocument를 구현 하는 방법을 보여 줍니다.

```csharp
public class MonkeyDocument : UIDocument
{
    // the 'model', just a chunk of text in this case; must easily convert to NSData
    NSString dataModel;
    // model is wrapped in a nice .NET-friendly property
    public string DocumentString {
        get {
            return dataModel.ToString ();
        }
        set {
            dataModel = new NSString (value);
        }
    }
    public MonkeyDocument (NSUrl url) : base (url)
    {
        DocumentString = "(default text)";
    }
    // contents supplied by iCloud to display, update local model and display (via notification)
    public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("LoadFromContents({0})", typeName);

        if (contents != null)
            dataModel = NSString.FromData ((NSData)contents, NSStringEncoding.UTF8);

        // LoadFromContents called when an update occurs
        NSNotificationCenter.DefaultCenter.PostNotificationName ("monkeyDocumentModified", this);
        return true;
    }
    // return contents for iCloud to save (from the local model)
    public override NSObject ContentsForType (string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("ContentsForType({0})", typeName);
        Console.WriteLine ("DocumentText:{0}",dataModel);

        NSData docData = dataModel.Encode (NSStringEncoding.UTF8);
        return docData;
    }
}
```

데이터 모델에이 경우 매우 간단한-단일 텍스트 필드입니다. 데이터 모델은 Xml 문서 또는 이진 데이터와 같이 필요한 만큼 복잡할 수 있습니다. UIDocument 구현의 주 역할 모델 클래스와 디스크에 저장/로드 될 수 있는 NSData 표현 간에 변환 하는 것입니다.

### <a name="finding-and-opening-icloud-documents"></a>찾기 및 icloud와 문서 열기

단일 파일-test.txt-샘플 앱만 처리 하므로 코드에서 **AppDelegate.cs** 만듭니다는 `NSPredicate` 및 `NSMetadataQuery` 해당 파일에 맞게 검색할 합니다. `NSMetadataQuery` 비동기적으로 실행 되 고 완료 되 면 알림을 보냅니다. `DidFinishGathering` 알림 observer에서 호출 하 고 쿼리를 중지 LoadDocument를 사용 하는 호출을 `UIDocument.Open` 파일을 로드 하 고 표시 하려고 하는 완료 처리기를 사용 하 여 메서드를 `MonkeyDocumentViewController`입니다.

```csharp
string monkeyDocFilename = "test.txt";
void FindDocument ()
{
    Console.WriteLine ("FindDocument");
    query = new NSMetadataQuery {
        SearchScopes = new NSObject [] { NSMetadataQuery.UbiquitousDocumentsScope }
    };

    var pred = NSPredicate.FromFormat ("%K == %@", new NSObject[] {
        NSMetadataQuery.ItemFSNameKey, new NSString (MonkeyDocFilename)
    });

    Console.WriteLine ("Predicate:{0}", pred.PredicateFormat);
    query.Predicate = pred;

    NSNotificationCenter.DefaultCenter.AddObserver (
        this,
        new Selector ("queryDidFinishGathering:"),
        NSMetadataQuery.DidFinishGatheringNotification,
        query
    );

    query.StartQuery ();
}

[Export ("queryDidFinishGathering:")]
void DidFinishGathering (NSNotification notification)
{
    Console.WriteLine ("DidFinishGathering");
    var metadataQuery = (NSMetadataQuery)notification.Object;
    metadataQuery.DisableUpdates ();
    metadataQuery.StopQuery ();

    NSNotificationCenter.DefaultCenter.RemoveObserver (this, NSMetadataQuery.DidFinishGatheringNotification, metadataQuery);
    LoadDocument (metadataQuery);
}

void LoadDocument (NSMetadataQuery metadataQuery)
{
    Console.WriteLine ("LoadDocument");

    if (metadataQuery.ResultCount == 1) {
        var item = (NSMetadataItem)metadataQuery.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);
        doc = new MonkeyDocument (url);

        doc.Open (success => {
            if (success) {
                Console.WriteLine ("iCloud document opened");
                Console.WriteLine (" -- {0}", doc.DocumentString);
                viewController.DisplayDocument (doc);
            } else {
                Console.WriteLine ("failed to open iCloud document");
            }
        });
    } // TODO: if no document, we need to create one
}
```

### <a name="displaying-icloud-documents"></a>ICloud 문서 표시

UIDocument 표시를 다른 모델 클래스는 다른 모든 않아야
- 속성은 사용자가 편집할 수 있습니다 및 모델에 다시 쓸 다음 UI 컨트롤에 표시 됩니다.

예제에서 **iCloudUIDoc\MonkeyDocumentViewController.cs** MonkeyDocument 텍스트 표시를 `UITextView`입니다. `ViewDidLoad` 전송 된 알림을 수신 하는 `MonkeyDocument.LoadFromContents` 메서드. `LoadFromContents` 호출 되 iCloud에 새 데이터가 파일에 대 한 알림 문서에 업데이트 되었음을 나타냅니다.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

샘플 코드 알림 처리기는 모든 충돌을 감지 하거나 확인 하지 않고이 경우 UI를 업데이트 하기 위한 메서드를 호출 합니다.

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>ICloud 문서를 저장 하는 중

UIDocument 호출할 수 있습니다 iCloud에 추가할 `UIDocument.Save` 직접 (에 대 한 새 문서에만 해당) 사용 하 여 기존 파일을 이동 또는 `NSFileManager.DefaultManager.SetUbiquitious`합니다. 예제 코드에서는이 코드를 사용 하 여 보편화 컨테이너에서 직접 새 문서를 만듭니다 (두 완료 처리기가 여기에 대 한 하나는 `Save` 작업과 다른 열기):

```csharp
var docsFolder = Path.Combine (iCloudUrl.Path, "Documents"); // NOTE: Documents folder is user-accessible in Settings
var docPath = Path.Combine (docsFolder, MonkeyDocFilename);
var ubiq = new NSUrl (docPath, false);
var monkeyDoc = new MonkeyDocument (ubiq);
monkeyDoc.Save (monkeyDoc.FileUrl, UIDocumentSaveOperation.ForCreating, saveSuccess => {
Console.WriteLine ("Save completion:" + saveSuccess);
if (saveSuccess) {
    monkeyDoc.Open (openSuccess => {
        Console.WriteLine ("Open completion:" + openSuccess);
        if (openSuccess) {
            Console.WriteLine ("new document for iCloud");
            Console.WriteLine (" == " + monkeyDoc.DocumentString);
            viewController.DisplayDocument (monkeyDoc);
        } else {
            Console.WriteLine ("couldn't open");
        }
    });
} else {
    Console.WriteLine ("couldn't save");
}
```

후속 문서에 저장 되지 않습니다"" 직접, 대신 알려줍니다 합니다 `UIDocument` 사용 하 여 변경 된 `UpdateChangeCount`, 디스크 작업에 저장 자동으로 예약 됩니다 및:

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>Icloud와 문서 관리

사용자의 icloud와 문서를 관리할 수 있습니다 합니다 **문서** 설정을 통해 응용 프로그램 외부에서 "보편화 컨테이너" 디렉터리 안쪽으로 살짝 밀어 삭제 하 고 파일 목록을 볼 수 있습니다. 응용 프로그램 코드는 사용자가 문서는 삭제 하는 위치는 상황을 처리할 수 있어야 합니다. 내부 응용 프로그램 데이터를 저장 하지 마십시오 합니다 **문서** 디렉터리입니다.

 [![](introduction-to-icloud-images/icloudstorage.png "ICloud 문서 워크플로 관리")](introduction-to-icloud-images/icloudstorage.png#lightbox)



또한 사용자에 게 알리는 iCloud 문서와 관련 된 해당 응용 프로그램의 상태는 장치에서 iCloud 지원 응용 프로그램을 제거 하려고 할 때 다른 경고를 받게 됩니다.

 [![](introduction-to-icloud-images/icloud-delete1.png "사용자가 장치에서 iCloud 지원 응용 프로그램을 제거 하려고 할 때 샘플 대화 상자")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "사용자가 장치에서 iCloud 지원 응용 프로그램을 제거 하려고 할 때 샘플 대화 상자")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>iCloud 백업

ICloud에 백업에는 개발자가 직접 액세스 하는 기능은 아니지만, 응용 프로그램을 디자인 하는 방법은 사용자 환경 영향을 줄 수 있습니다.
Apple에서 제공 [데이터 저장소 지침 iOS](http://developer.apple.com/icloud/documentation/data-storage/) 개발자가 iOS 응용 프로그램에 따라야 합니다.

가장 중요 한 고려 사항은 앱 (예: 문제 당 콘텐츠의 hundred-plus 메가바이트를 저장 하는 magazine 독자 응용 프로그램을) 사용자가 생성 되지 않은 큰 파일을 저장 하는 여부입니다. Apple이이 종류의 위치는 백업할 iCloud로 하 고 사용자의 iCloud 할당량을 불필요 하 게 입력 데이터를 저장 하지 않는 것을 선호 합니다.

많은 양의 데이터를 저장 하는 응용 프로그램 해야 하거나 저장 하지 않은 백업 (예: 사용자 디렉터리 중 하나 캐시 또는 tmp) 하거나 사용 하 여 `NSFileManager.SetSkipBackupAttribute` iCloud 백업 작업 중 무시 되도록 해당 파일에 플래그를 적용 합니다.

## <a name="summary"></a>요약

이 문서에서는 iOS 5에에서 포함 된 새 iCloud 기능을 도입 합니다. ICloud 사용 하도록 프로젝트를 구성 하는 데 필요한와 iCloud 기능을 구현 하는 방법의 예제를 제공 하는 단계를 조사 하 고 있습니다.

키-값 저장소 예제에서는 작은 양의 NSUserPreferences 저장 된 방식과 유사 하 게 데이터를 저장할 iCloud를 사용할 수 있는 방법을 보여 줍니다. UIDocument 예제에서는 어떻게 더 많은 복잡 한 데이터를 저장 하 고 iCloud 통해 여러 장치 간에 동기화 수를 보여줍니다.

마지막으로 iCloud 백업 추가 응용 프로그램 디자인에 영향을 줄 해야 하는 방법에 대 한 간략 한 논의 포함 되어 있습니다.



## <a name="related-links"></a>관련 링크

- [소개에 iCloud (샘플)](https://developer.xamarin.com/samples/monotouch/IntroductionToiCloud)
- [iCloud 세미나 샘플 코드](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [iCloud 세미나 슬라이드](http://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [iCloud 저장소](http://support.apple.com/kb/HT4847)
