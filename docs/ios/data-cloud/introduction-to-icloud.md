---
title: Xamarin.ios에서 iCloud 사용
description: 이 문서에서는 iCloud와 Xamarin.ios 응용 프로그램에서 사용 하는 방법에 대해 설명 합니다. 키-값 저장소, 문서 저장소 및 iCloud 백업에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/09/2016
ms.openlocfilehash: f2bc6ce6c709f99c744554d80b065e34961904ac
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105851"
---
# <a name="using-icloud-with-xamarinios"></a>Xamarin.ios에서 iCloud 사용

IOS 5의 iCloud 저장소 API를 사용 하면 응용 프로그램에서 사용자 문서 및 응용 프로그램별 데이터를 중앙 위치에 저장 하 고 모든 사용자의 장치에서 이러한 항목에 액세스할 수 있습니다.

사용할 수 있는 저장소에는 다음 네 가지 유형이 있습니다.

- **키-값 저장소** -사용자의 다른 장치에서 응용 프로그램과 소량의 데이터를 공유 합니다.

- **Uidocument 저장소** -uidocument의 서브 클래스를 사용 하 여 사용자의 iCloud 계정에 문서 및 기타 데이터를 저장 합니다.

- **CoreData** -SQLite 데이터베이스 저장소.

- **개별 파일 및 디렉터리** -파일 시스템에서 직접 다양 한 파일을 관리 하기 위한 것입니다.

이 문서에서는 처음 두 가지 유형인 키-값 쌍 및 UIDocument 서브 클래스를 설명 하 고 Xamarin.ios에서 이러한 기능을 사용 하는 방법을 설명 합니다.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

## <a name="requirements"></a>요구 사항

- Xamarin.ios의 안정적인 최신 버전
- Xcode 10
- Mac용 Visual Studio 또는 Visual Studio 2019.

## <a name="preparing-for-icloud-development"></a>ICloud 개발 준비

응용 프로그램은 [Apple 프로 비전 포털과](https://developer.apple.com/account/ios/overview.action) 프로젝트 자체에서 iCloud를 사용 하도록 구성 되어야 합니다. ICloud를 위해 개발 하거나 샘플을 사용해 보려면 아래 단계를 수행 합니다.

ICloud에 액세스 하도록 응용 프로그램을 올바르게 구성 하려면:

- [Developer.apple.com](https://developer.apple.com) 에 대 한 **teamid** -로그인을 찾은 다음 **회원 > 센터 > 개발자 계정 요약** 을 방문 하 여 팀 id (또는 단일 개발자를 위한 개별 id)를 가져옵니다. 10 자리 문자열 (예: **A93A5CM278** ) 이며, "컨테이너 식별자"의 일부를 형성 합니다.

- **새 앱 Id 만들기** -앱 id를 만들려면 [장치 프로 비전 가이드의 스토어 기술 프로 비전 섹션](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)에 설명 된 단계에 따라 **iCloud** 를 허용 되는 서비스로 확인 합니다.

 [![](introduction-to-icloud-images/icloud-sml.png "ICloud를 허용 되는 서비스로 확인")](introduction-to-icloud-images/icloud.png#lightbox)

- **새 프로 비전 프로필 만들기** -프로 비전 프로필을 만들려면 [장치 프로 비전 가이드](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) 에 설명 된 단계를 따릅니다.

- **컨테이너 식별자를 info.plist에 추가** 합니다. 컨테이너 식별자 형식은 `TeamID.BundleID`입니다. 자세한 내용은 [자격 사용](~/ios/deploy-test/provisioning/entitlements.md) 가이드를 참조 하세요.

- **프로젝트 속성 구성** -Info.plist 파일에서 **번들 식별자** 가 [앱 id를 만들](~/ios/deploy-test/provisioning/capabilities/index.md)때 설정 된 **번들 id** 와 일치 하는지 확인 합니다. IOS 번들 서명은 iCloud App Service와 앱 ID가 포함 된 **프로 비전 프로필** 을 사용 하 고 **사용자 지정 자격** 파일을 선택 합니다. Visual Studio의 프로젝트 속성 창에서이 작업을 모두 수행할 수 있습니다.

- **장치에서 Icloud 사용** - **설정 > icloud** 로 이동 하 여 장치가 로그인 했는지 확인 합니다.
**문서 & 데이터** 옵션을 선택 하 고 설정 합니다.

- **ICloud를 테스트 하려면 장치를 사용 해야** 합니다. 시뮬레이터에서는 작동 하지 않습니다.
실제로 동일한 Apple ID를 사용 하 여 로그인 한 장치를 둘 이상 사용 하 여 iCloud를 실행 하 고 있는지 확인 해야 합니다.

## <a name="key-value-storage"></a>키-값 저장소

키-값 저장소는 책 이나 잡지에서 마지막으로 본 페이지와 같이 사용자가 장치 간에 지속 될 수 있는 소량의 데이터를 위한 것입니다. 키-값 저장소는 데이터를 백업 하는 데 사용 하면 안 됩니다.

키-값 저장소를 사용할 때 알아야 할 몇 가지 제한 사항이 있습니다.

- **최대 키 크기** -키 이름은 64 바이트 보다 길 수 없습니다.

- **최대 값 크기** -단일 값에 64 보다 큰 값을 저장할 수 없습니다.

- **앱에 대 한 최대 키 값 저장소 크기** -응용 프로그램은 최대 64의 키-값 데이터만 저장할 수 있습니다. 이 제한을 초과 하는 키를 설정 하려고 하면 실패 하 고 이전 값이 유지 됩니다.

- **데이터 형식** -문자열, 숫자, 부울 등의 기본 형식만 저장할 수 있습니다.

**ICloudKeyValue** 예제에서는 작동 방식을 보여 줍니다. 샘플 코드는 각 장치에 대해 라는 키를 만듭니다. 한 장치에이 키를 설정 하 고 다른 사용자에 게 전파 되는 값을 볼 수 있습니다. 또한 모든 장치에서 편집할 수 있는 "공유" 라는 키를 만듭니다. 한 번에 많은 장치를 편집 하는 경우 iCloud는 변경 타임 스탬프를 사용 하 여 "wins" 값을 결정 하 고 전파 됩니다.

이 스크린샷에서는 사용 중인 샘플을 보여 줍니다. ICloud에서 변경 알림이 수신 되 면 화면 아래쪽의 스크롤 텍스트 보기에 인쇄 되 고 입력 필드에서 업데이트 됩니다.

 [![](introduction-to-icloud-images/icloud-kv-arrows.png "장치 간 메시지 흐름")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>데이터 설정 및 검색

이 코드는 문자열 값을 설정 하는 방법을 보여 줍니다.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

동기화를 호출 하면 값이 로컬 디스크 저장소에만 유지 됩니다. ICloud에 대 한 동기화는 백그라운드에서 수행 되며 응용 프로그램 코드에서 "강제" 할 수 없습니다. 네트워크 연결이 양호 하면 5 초 이내에 동기화가 자주 발생 하지만, 네트워크에 문제가 있거나 연결이 끊어진 경우 업데이트가 훨씬 더 오래 걸릴 수 있습니다.

이 코드를 사용 하 여 값을 검색할 수 있습니다.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

값은 로컬 데이터 저장소에서 검색 됩니다 .이 메서드는 iCloud 서버에 연결 하 여 "최신" 값을 가져오려고 시도 하지 않습니다. iCloud는 자체 일정에 따라 로컬 데이터 저장소를 업데이트 합니다.

### <a name="deleting-data"></a>데이터 삭제

키-값 쌍을 완전히 제거 하려면 다음과 같이 Remove 메서드를 사용 합니다.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>변경 내용 관찰

응용 프로그램은에 관찰자 `NSNotificationCenter.DefaultCenter`를 추가 하 여 iCloud에 의해 값이 변경 될 때 알림을 받을 수도 있습니다.
다음 **KeyValueViewController.cs** `ViewWillAppear` 메서드 코드는 이러한 알림을 수신 하 고 변경 된 키 목록을 만드는 방법을 보여 줍니다.

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

그런 다음 코드에서 변경 된 키 목록 (예: 로컬 복사본을 업데이트 하거나 새 값으로 UI 업데이트)을 사용 하 여 일부 작업을 수행할 수 있습니다.

가능한 변경 이유는 다음과 같습니다. ServerChange (0), InitialSyncChange (1) 또는 QuotaViolationChange (2). 이유에 액세스 하 고 필요한 경우 다른 처리를 수행할 수 있습니다. 예를 들어 *QuotaViolationChange*의 결과로 일부 키를 제거 해야 할 수 있습니다.

## <a name="document-storage"></a>문서 저장소

iCloud 문서 저장소는 앱 및 사용자에 게 중요 한 데이터를 관리 하도록 설계 되었습니다. 앱이 실행 해야 하는 파일 및 기타 데이터를 관리 하는 데 사용할 수 있으며, 동시에 모든 사용자의 장치에서 iCloud 기반 백업 및 공유 기능을 제공 합니다.

이 다이어그램은 모두 어떻게 통합 되는지 보여 줍니다. 각 장치에는 로컬 저장소 (UbiquityContainer)에 저장 된 데이터가 있으며 운영 체제의 iCloud 디먼은 클라우드에서 데이터를 보내고 받는 작업을 담당 합니다. 동시 액세스를 방지 하려면 UbiquityContainer에 대 한 모든 파일 액세스를 FilePresenter/FileCoordinator를 통해 수행 해야 합니다. 클래스 `UIDocument` 는 사용자를 위해이를 구현 합니다 .이 예제에서는 uidocument를 사용 하는 방법을 보여 줍니다.

 [![](introduction-to-icloud-images/icloud-overview.png "문서 저장소 개요")](introduction-to-icloud-images/icloud-overview.png#lightbox)

ICloudUIDoc 예제는 단일 텍스트 필드 `UIDocument` 를 포함 하는 간단한 서브 클래스를 구현 합니다. 에서 `UITextView` 렌더링 되는 텍스트는 iCloud에서 빨간색으로 표시 된 알림 메시지를 사용 하 여 다른 장치로 전파 됩니다. 예제 코드는 충돌 확인과 같은 고급 iCloud 기능을 처리 하지 않습니다.

이 스크린샷은 샘플 응용 프로그램을 보여 줍니다. 텍스트를 변경 하 고 **UpdateChangeCount** 를 누르면 문서가 iCloud를 통해 다른 장치로 동기화 됩니다.

 [![](introduction-to-icloud-images/iclouduidoc.png "이 스크린샷에서는 텍스트를 변경 하 고 UpdateChangeCount를 누르는 후의 샘플 응용 프로그램을 보여 줍니다.")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

ICloudUIDoc 샘플에는 5 개의 부분이 있습니다.

1. **UbiquityContainer에 액세스** -icloud를 사용 하도록 설정 했는지 확인 하 고, 응용 프로그램의 icloud 저장소 영역에 대 한 경로를 확인 합니다.

1. **UIDocument 하위 클래스 만들기** -iCloud 저장소와 모델 개체 간에 중간에 대 한 클래스를 만듭니다.

1. **Icloud 문서 찾기 및 열기** -및 `NSFileManager` `NSPredicate` 를 사용 하 여 icloud 문서를 찾아 엽니다.

1. **ICloud 문서 표시** -UI 컨트롤과 상호 작용할 `UIDocument` 수 있도록에서 속성을 노출 합니다.

1. **ICloud 문서 저장** -UI에서 변경한 내용이 디스크 및 iCloud에 유지 되는지 확인 합니다.

모든 iCloud 작업을 비동기적으로 실행 하거나 실행 하 여 문제가 발생 하기를 기다리는 동안 차단 되지 않도록 합니다. 샘플에서이 작업을 수행 하는 세 가지 다른 방법이 표시 됩니다.

 에 `AppDelegate.FinishedLaunching` 대`GetUrlForUbiquityContainer` 한 초기 호출에서 스레드는 주 스레드가 차단 되지 않도록 다른 스레드에서 수행 됩니다.

 **Notificationcenter** -완료와 `NSMetadataQuery.StartQuery` 같은 비동기 작업을 수행할 때 알림에 등록 합니다.

 **완료 처리기** -과 같은 `UIDocument.Open`비동기 작업 완료 시 실행할 메서드를 전달 합니다.

### <a name="accessing-the-ubiquitycontainer"></a>UbiquityContainer에 액세스

ICloud 문서 저장소를 사용 하는 첫 번째 단계는 iCloud를 사용 하도록 설정 했는지 여부와 "어디 container" (iCloud 사용 파일이 장치에 저장 되는 디렉터리)의 위치를 확인 하는 것입니다.

이 코드는 `AppDelegate.FinishedLaunching` 샘플의 메서드에 있습니다.

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

이 샘플은 그렇게 하지 않지만 앱이 포그라운드로 들어올 때마다 Apple은 GetUrlForUbiquityContainer를 호출 하는 것을 권장 합니다.

### <a name="creating-a-uidocument-subclass"></a>UIDocument 하위 클래스 만들기

모든 iCloud 파일 및 디렉터리 (UbiquityContainer 디렉터리에 저장 된 모든 항목)는 Nsfilemanager를 사용 하 여 Nsfilemanager 프로토콜을 구현 하 고 NSFileCoordinator를 통해 작성 해야 합니다.
모든 작업을 수행 하는 가장 간단한 방법은 직접 작성 하는 것이 아니라 모든 사용자를 위한 Uiui문서입니다.

ICloud를 사용 하려면 UIDocument 하위 클래스에서 구현 해야 하는 두 가지 방법이 있습니다.

- **LoadFromContents** -모델 클래스/es로 압축을 풀 수 있도록 파일 내용의 nsdata를 전달 합니다.

- 내용 **지정-디스크** (및 클라우드)에 저장할 모델 클래스/Es의 nsdata 표현을 제공 하는 요청입니다.

**ICloudUIDoc\MonkeyDocument.cs** 의이 샘플 코드에서는 uidocument를 구현 하는 방법을 보여 줍니다.

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

이 경우 데이터 모델은 매우 단순 하며 단일 텍스트 필드입니다. 데이터 모델은 Xml 문서 또는 이진 데이터와 같이 필요에 따라 복잡할 수 있습니다. UIDocument 구현의 주 역할은 디스크에 저장/로드할 수 있는 모델 클래스와 NSData 표현 사이를 변환 하는 것입니다.

### <a name="finding-and-opening-icloud-documents"></a>ICloud 문서 찾기 및 열기

샘플 앱은 단일 파일-test.txt만 처리 하므로 **AppDelegate.cs** 의 코드는를 만들고 `NSPredicate` `NSMetadataQuery` 해당 파일 이름을 확인 하는 데 사용 됩니다. 는 `NSMetadataQuery` 비동기식으로 실행 되 고 완료 되 면 알림을 보냅니다. `DidFinishGathering`알림 관찰자가 호출 하 고, 쿼리를 중지 하 고, 완료 처리기와 함께 `UIDocument.Open` 메서드를 사용 하 여 파일을 로드 하 고 `MonkeyDocumentViewController`에 표시 하는 loaddocument를 호출 합니다.

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

UIDocument를 표시 하는 것은 다른 모델 클래스와 다를 수 없습니다. 속성은 UI 컨트롤에 표시 됩니다 .이 속성은 사용자가 편집 하 고 모델에 다시 쓸 수 있습니다.

예제에서는 **iCloudUIDoc\MonkeyDocumentViewController.cs** 에 MonkeyDocument 텍스트 `UITextView`를 표시 합니다. `ViewDidLoad``MonkeyDocument.LoadFromContents` 메서드에서 보내는 알림을 수신 합니다. `LoadFromContents`iCloud에 파일에 대 한 새 데이터가 있으므로 알림이 문서를 업데이트 했음을 나타내는 경우 호출 됩니다.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

예제 코드 알림 처리기는 메서드를 호출 하 여 UI를 업데이트 합니다 .이 경우에는 충돌 검색 또는 해결이 필요 하지 않습니다.

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>ICloud 문서 저장

ICloud에 uidocument를 추가 하려면를 직접 호출 `UIDocument.Save` 하거나 (새 문서에만 해당)를 사용 하 여 `NSFileManager.DefaultManager.SetUbiquitious`기존 파일을 이동할 수 있습니다. 예제 코드는이 코드를 사용 하 여 어디 컨테이너에 직접 새 문서를 만듭니다. 여기에는 두 가지 완료 처리기 ( `Save` 작업에 대 한 작업 및 다른 하나는 열기)가 있습니다.

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

문서에 대 한 후속 변경 내용은 직접 "저장" 되지 않으며, 대신을 `UIDocument` 사용 `UpdateChangeCount`하 여 변경 되었음을 알리고, 디스크에 저장 작업을 자동으로 예약 합니다.

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>ICloud 문서 관리

사용자는 설정을 통해 응용 프로그램 외부에서 "어디 container"의 **documents** 디렉터리에 있는 iCloud 문서를 관리할 수 있습니다. 파일 목록을 보고 삭제로 살짝 밀기 할 수 있습니다. 응용 프로그램 코드는 사용자가 문서를 삭제 하는 상황을 처리할 수 있어야 합니다. **문서** 디렉터리에 내부 응용 프로그램 데이터를 저장 하지 마세요.

 [![](introduction-to-icloud-images/icloudstorage.png "ICloud 문서 관리 워크플로")](introduction-to-icloud-images/icloudstorage.png#lightbox)

사용자는 장치에서 iCloud 지원 응용 프로그램을 제거 하려고 할 때 다른 경고를 수신 하 여 해당 응용 프로그램과 관련 된 iCloud 문서의 상태를 알립니다.

 [![](introduction-to-icloud-images/icloud-delete1.png "사용자가 장치에서 iCloud 지원 응용 프로그램을 제거 하려고 하는 경우의 샘플 대화 상자")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "사용자가 장치에서 iCloud 지원 응용 프로그램을 제거 하려고 하는 경우의 샘플 대화 상자")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>iCloud 백업

ICloud로 백업 하는 것은 개발자가 직접 액세스할 수 있는 기능이 아니지만 응용 프로그램을 디자인 하는 방식은 사용자 환경에 영향을 줄 수 있습니다.
Apple은 개발자가 iOS 응용 프로그램에서 수행할 수 있는 [Ios 데이터 저장소 지침](https://developer.apple.com/icloud/documentation/data-storage/) 을 제공 합니다.

가장 중요 한 고려 사항은 앱이 사용자가 생성 되지 않은 큰 파일을 저장 하는지 여부입니다. 예를 들어 문제 당 수백 개 이상의 콘텐츠가 저장 된 잡지 reader 응용 프로그램입니다. Apple은 이러한 종류의 데이터를 iCloud로 백업 하 고 사용자의 iCloud 할당량을 불필요 하 게 입력 하는 것을 선호 합니다.

이 처럼 많은 양의 데이터를 저장 하는 응용 프로그램은 백업 되지 않은 사용자 디렉터리 중 하나에 저장 해야 합니다 (예: 캐시 또는 tmp)를 사용 `NSFileManager.SetSkipBackupAttribute` 하거나를 사용 하 여 백업 작업 중에 iCloud가 무시 하도록 해당 파일에 플래그를 적용 합니다.

## <a name="summary"></a>요약

이 문서에서는 iOS 5에 포함 된 새 iCloud 기능을 소개 했습니다. ICloud를 사용 하도록 프로젝트를 구성 하는 데 필요한 단계를 검토 한 다음 iCloud 기능을 구현 하는 방법에 대 한 예제를 제공 했습니다.

키-값 저장소 예에서는 iCloud를 사용 하 여 NSUserPreferences 설정이 저장 되는 방식과 비슷한 소량의 데이터를 저장 하는 방법을 보여 줍니다. UIDocument 예제에서는 iCloud를 통해 여러 장치에 더 복잡 한 데이터를 저장 하 고 동기화 하는 방법을 보여 주었습니다.

마지막으로 응용 프로그램 디자인에 영향을 주는 iCloud 백업 추가 방법에 대 한 간략 한 설명이 포함 되어 있습니다.

## <a name="related-links"></a>관련 링크

- [ICloud 소개 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/introductiontoicloud)
- [iCloud 세미나 샘플 코드](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [iCloud 세미나 슬라이드](https://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [iCloud 저장소](https://support.apple.com/kb/HT4847)
