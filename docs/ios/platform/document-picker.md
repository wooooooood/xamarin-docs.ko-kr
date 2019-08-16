---
title: Xamarin.ios의 문서 선택기
description: 이 문서에서는 iOS 문서 선택기에 대해 설명 하 고 Xamarin.ios에서이를 사용 하는 방법을 설명 합니다. ICloud, 문서, 일반적인 설정 코드, 문서 공급자 확장 등을 살펴봅니다.
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: cd38facb62c5864f1c933611d8d9dcda94589066
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528232"
---
# <a name="document-picker-in-xamarinios"></a>Xamarin.ios의 문서 선택기

문서 선택기를 사용 하면 앱 간에 문서를 공유할 수 있습니다. 이러한 문서는 iCloud 또는 다른 앱의 디렉터리에 저장 될 수 있습니다. 문서는 사용자가 장치에 설치한 [문서 공급자 확장](~/ios/platform/extensions.md) 집합을 통해 공유 됩니다. 

앱과 클라우드 간에 문서를 동기화 된 상태로 유지 하는 어려움 때문에 필요한 복잡성을 특정 하 게 소개 합니다.

## <a name="requirements"></a>요구 사항

이 문서에 제공 된 단계를 완료 하려면 다음이 필요 합니다.

- **Xcode 7 및 ios 8 이상** – Apple의 Xcode 7 및 ios 8 이상 api를 개발자의 컴퓨터에 설치 하 고 구성 해야 합니다.
- **Visual Studio 또는 Mac용 Visual Studio** – Mac용 Visual Studio 최신 버전을 설치 해야 합니다.
- **Ios 장치** – ios 8 이상을 실행 하는 ios 장치입니다.

## <a name="changes-to-icloud"></a>ICloud의 변경 내용

문서 선택의 새 기능을 구현 하기 위해 Apple의 iCloud 서비스가 다음과 같이 변경 되었습니다.

- ICloud 데몬이 CloudKit를 사용 하 여 완전히 다시 작성 되었습니다.
- 기존 iCloud 기능의 이름이 iCloud 드라이브로 바뀌었습니다.
- Microsoft Windows OS에 대 한 지원이 iCloud에 추가 되었습니다.
- ICloud 폴더가 Mac OS Finder에 추가 되었습니다.
- iOS 장치는 Mac OS iCloud 폴더의 내용에 액세스할 수 있습니다.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

## <a name="what-is-a-document"></a>문서 란 무엇 인가요?

ICloud의 문서를 참조 하는 경우이는 단일 독립 실행형 엔터티 이며 사용자가 해당 문서를 인식 해야 합니다. 사용자가 문서를 수정 하거나 전자 메일을 사용 하 여 다른 사용자와 공유할 수 있습니다.

사용자가 문서 (예: 페이지, 키 노트 또는 숫자 파일)로 즉시 인식할 수 있는 여러 형식의 파일이 있습니다. 그러나 iCloud는 이러한 개념으로 제한 되지 않습니다. 예를 들어 게임의 상태 (예: 체스 일치)를 문서로 처리 하 고 iCloud에 저장할 수 있습니다. 이 파일은 사용자의 장치 간에 전달 될 수 있으며, 다른 장치에서 남은 게임을 선택할 수 있습니다.

## <a name="dealing-with-documents"></a>문서 처리

Xamarin과 함께 문서 선택기를 사용 하는 데 필요한 코드를 살펴보기 전에이 문서에서는 iCloud 문서 작업에 대 한 모범 사례와 문서 선택기를 지 원하는 데 필요한 기존 Api에 대 한 몇 가지 수정 사항을 소개 합니다.

### <a name="using-file-coordination"></a>파일 조정 사용

파일은 여러 위치에서 수정할 수 있으므로 데이터 손실을 방지 하려면 조정을 사용 해야 합니다.

 [![](document-picker-images/image1.png "파일 조정 사용")](document-picker-images/image1.png#lightbox)

위의 그림을 살펴보겠습니다.

1. 파일 조정을 사용 하는 iOS 장치는 새 문서를 만들어 iCloud 폴더에 저장 합니다.
2. iCloud는 모든 장치에 배포 하기 위해 수정 된 파일을 클라우드에 저장 합니다.
3. 연결 된 Mac은 iCloud 폴더에서 수정 된 파일을 확인 하 고 파일 조정을 사용 하 여 변경 내용을 파일에 복사 합니다.
4. 파일 조정을 사용 하지 않는 장치에서는 파일을 변경 하 고 iCloud 폴더에 저장 합니다. 이러한 변경 내용은 다른 장치에 즉시 복제 됩니다.

원본 iOS 장치 또는 Mac에서 파일을 편집 한다고 가정 하면 변경 내용이 손실 되 고 조정 되지 않은 장치에서 파일의 버전으로 덮어쓰여집니다. 데이터 손실을 방지 하기 위해 클라우드 기반 문서를 사용할 때 파일 조정을 사용 해야 합니다.

### <a name="using-uidocument"></a>UIDocument 사용

 `UIDocument`개발자를 위한 모든 작업 `NSDocument` 을 수행 하 여 간단 하 게 (또는 macos) 작업을 수행 합니다. 응용 프로그램의 UI를 차단 하지 않도록 백그라운드 큐와 기본 제공 파일 조정을 제공 합니다.

 `UIDocument`는 개발자가 필요로 하는 모든 목적을 위해 Xamarin 응용 프로그램의 개발 노력을 용이 하 게 하는 여러 개의 상위 수준 Api를 노출 합니다.

다음 코드에서는의 `UIDocument` 서브 클래스를 만들어 iCloud에서 텍스트를 저장 하 고 검색 하는 데 사용할 수 있는 일반 텍스트 기반 문서를 구현 합니다.

```csharp
using System;
using Foundation;
using UIKit;

namespace DocPicker
{
    public class GenericTextDocument : UIDocument
    {
        #region Private Variable Storage
        private NSString _dataModel;
        #endregion

        #region Computed Properties
        public string Contents {
            get { return _dataModel.ToString (); }
            set { _dataModel = new NSString(value); }
        }
        #endregion

        #region Constructors
        public GenericTextDocument (NSUrl url) : base (url)
        {
            // Set the default document text
            this.Contents = "";
        }

        public GenericTextDocument (NSUrl url, string contents) : base (url)
        {
            // Set the default document text
            this.Contents = contents;
        }
        #endregion

        #region Override Methods
        public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Were any contents passed to the document?
            if (contents != null) {
                _dataModel = NSString.FromData( (NSData)contents, NSStringEncoding.UTF8 );
            }

            // Inform caller that the document has been modified
            RaiseDocumentModified (this);

            // Return success
            return true;
        }

        public override NSObject ContentsForType (string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Convert the contents to a NSData object and return it
            NSData docData = _dataModel.Encode(NSStringEncoding.UTF8);
            return docData;
        }
        #endregion

        #region Events
        public delegate void DocumentModifiedDelegate(GenericTextDocument document);
        public event DocumentModifiedDelegate DocumentModified;

        internal void RaiseDocumentModified(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentModified != null) {
                this.DocumentModified (document);
            }
        }
        #endregion
    }
}
```

위에서 `GenericTextDocument` 설명한 클래스는 xamarin.ios 8 응용 프로그램에서 문서 선택기 및 외부 문서를 사용할 때이 문서 전체에서 사용 됩니다.

## <a name="asynchronous-file-coordination"></a>비동기 파일 조정

iOS 8은 새로운 파일 조정 Api를 통해 몇 가지 새로운 비동기 파일 조정 기능을 제공 합니다. IOS 8 이전에는 기존의 모든 파일 조정 Api가 완전히 동기화 되었습니다. 즉, 개발자는 파일 조정이 응용 프로그램의 UI를 차단 하지 않도록 자체 백그라운드 큐를 구현 해야 했습니다.

새 `NSFileAccessIntent` 클래스에는 파일을 가리키는 URL과 필요한 조정 형식을 제어 하기 위한 몇 가지 옵션이 포함 되어 있습니다. 다음 코드에서는 의도를 사용 하 여 한 위치에서 다른 위치로 파일을 이동 하는 방법을 보여 줍니다.

```csharp
// Get source options
var srcURL = NSUrl.FromFilename ("FromFile.txt");
var srcIntent = NSFileAccessIntent.CreateReadingIntent (srcURL, NSFileCoordinatorReadingOptions.ForUploading);

// Get destination options
var dstURL = NSUrl.FromFilename ("ToFile.txt");
var dstIntent = NSFileAccessIntent.CreateReadingIntent (dstURL, NSFileCoordinatorReadingOptions.ForUploading);

// Create an array
var intents = new NSFileAccessIntent[] {
    srcIntent,
    dstIntent
};

// Initialize a file coordination with intents
var queue = new NSOperationQueue ();
var fileCoordinator = new NSFileCoordinator ();
fileCoordinator.CoordinateAccess (intents, queue, (err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}",err.LocalizedDescription);
    }
});
```

## <a name="discovering-and-listing-documents"></a>문서 검색 및 나열

문서를 검색 하 고 나열 하는 방법은 기존 `NSMetadataQuery` api를 사용 하는 것입니다. 이 섹션에서는 이전 보다 훨씬 더 쉽게 `NSMetadataQuery` 문서를 사용할 수 있도록에 추가 된 새로운 기능에 대해 설명 합니다.

### <a name="existing-behavior"></a>기존 동작

IOS 8 이전에는 `NSMetadataQuery` 삭제, 생성 및 이름 변경 등의 픽업 로컬 파일 변경에 대 한 속도가 느립니다.

 [![](document-picker-images/image2.png "NSMetadataQuery 로컬 파일 변경 개요")](document-picker-images/image2.png#lightbox)

위의 다이어그램에서:

1. 응용 프로그램 컨테이너에 이미 있는 파일의 경우에 `NSMetadataQuery` 는 응용 `NSMetadata` 프로그램에서 즉시 사용할 수 있도록 기존 레코드를 미리 만들고 스풀링 합니다.
1. 응용 프로그램은 응용 프로그램 컨테이너에 새 파일을 만듭니다.
1. 이전에는 응용 프로그램 `NSMetadataQuery` 컨테이너에 대 한 수정 내용을 확인 하 고 필요한 `NSMetadata` 레코드를 만들기 전에 지연이 발생 합니다.


`NSMetadata` 레코드 생성의 지연으로 인해 응용 프로그램에는 로컬 파일 변경과 클라우드 기반 변경에 대 한 두 개의 데이터 원본이 열려 있어야 했습니다.

### <a name="stitching"></a>중철

IOS 8 `NSMetadataQuery` 에서는 중철 이라는 새 기능을 사용 하 여 직접를 사용 하는 것이 더 쉽습니다.

 [![](document-picker-images/image3.png "중철 라는 새로운 기능이 있는 NSMetadataQuery")](document-picker-images/image3.png#lightbox)

위의 다이어그램에서 중철 사용:

1. 이전 처럼 응용 프로그램 컨테이너에 이미 있는 파일의 경우에는 `NSMetadataQuery` 기존 `NSMetadata` 레코드를 미리 만들고 스풀링 합니다.
1. 응용 프로그램은 파일 조정을 사용 하 여 응용 프로그램 컨테이너에 새 파일을 만듭니다.
1. 응용 프로그램 컨테이너의 후크가 수정 사항을 확인 하 고 필요한 `NSMetadataQuery` `NSMetadata` 레코드를 만들기 위한 호출을 확인 합니다.
1. 레코드 `NSMetadata` 는 파일 바로 뒤에 생성 되며 응용 프로그램에서 사용할 수 있게 됩니다.


중철를 사용 하 여 응용 프로그램은 더 이상 로컬 및 클라우드 기반 파일 변경 내용을 모니터링 하기 위해 데이터 원본을 열 필요가 없습니다. 이제 응용 프로그램에서 직접를 `NSMetadataQuery` 사용할 수 있습니다.

> [!IMPORTANT]
> 중철는 응용 프로그램이 위의 섹션에서 설명한 대로 파일 조정을 사용 하는 경우에만 작동 합니다. 파일 조정을 사용 하지 않는 경우 Api는 기존 사전 iOS 8 동작을 기본값으로 사용 합니다.




### <a name="new-ios-8-metadata-features"></a>새 iOS 8 메타 데이터 기능

IOS 8의에 `NSMetadataQuery` 는 다음과 같은 새로운 기능이 추가 되었습니다.

- `NSMetatadataQuery`이제 클라우드에 저장 된 로컬이 아닌 문서를 나열할 수 있습니다.
- 클라우드 기반 문서에서 메타 데이터 정보에 액세스 하기 위해 새 Api가 추가 되었습니다. 
- 로컬에서 사용 가능한 `NSUrl_PromisedItems` 콘텐츠를 사용 하거나 사용 하지 않을 수 있는 파일의 파일 특성에 액세스 하는 새로운 API가 있습니다.
- 메서드를 사용 하 여 지정 된 파일에 대 한 정보를 `GetPromisedItemResourceValues` 가져오거나 메서드를 사용 하 여 한 번에 두 개 이상의 파일에 대 한 정보를 가져옵니다. `GetPromisedItemResourceValue`


메타 데이터를 처리 하기 위해 두 개의 새로운 파일 조정 플래그가 추가 되었습니다.

- `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
- `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


위의 플래그를 사용 하면 문서 파일의 콘텐츠를 사용 하기 위해 로컬에서 사용할 필요가 없습니다.

다음 코드 세그먼트는를 사용 `NSMetadataQuery` 하 여 특정 파일의 존재를 쿼리하고 파일이 없는 경우 파일을 빌드하는 방법을 보여 줍니다.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;


#region Static Properties
public const string TestFilename = "test.txt"; 
#endregion

#region Computed Properties
public bool HasiCloud { get; set; }
public bool CheckingForiCloud { get; set; }
public NSUrl iCloudUrl { get; set; }

public GenericTextDocument Document { get; set; }
public NSMetadataQuery Query { get; set; }
#endregion

#region Private Methods
private void FindDocument () {
    Console.WriteLine ("Finding Document...");

    // Create a new query and set it's scope
    Query = new NSMetadataQuery();
    Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

    // Build a predicate to locate the file by name and attach it to the query
    var pred = NSPredicate.FromFormat ("%K == %@"
        , new NSObject[] {
            NSMetadataQuery.ItemFSNameKey
            , new NSString(TestFilename)});
    Query.Predicate = pred;

    // Register a notification for when the query returns
    NSNotificationCenter.DefaultCenter.AddObserver (this,
            new Selector("queryDidFinishGathering:"),           NSMetadataQuery.DidFinishGatheringNotification,
            Query);

    // Start looking for the file
    Query.StartQuery ();
    Console.WriteLine ("Querying: {0}", Query.IsGathering);
}

[Export("queryDidFinishGathering:")]
public void DidFinishGathering (NSNotification notification) {
    Console.WriteLine ("Finish Gathering Documents.");

    // Access the query and stop it from running
    var query = (NSMetadataQuery)notification.Object;
    query.DisableUpdates();
    query.StopQuery();

    // Release the notification
    NSNotificationCenter.DefaultCenter.RemoveObserver (this
        , NSMetadataQuery.DidFinishGatheringNotification
        , query);

    // Load the document that the query returned
    LoadDocument(query);
}

private void LoadDocument (NSMetadataQuery query) {
    Console.WriteLine ("Loading Document...");    

    // Take action based on the returned record count
    switch (query.ResultCount) {
    case 0:
        // Create a new document
        CreateNewDocument ();
        break;
    case 1:
        // Gain access to the url and create a new document from
        // that instance
        NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

        // Load the document
        OpenDocument (url);
        break;
    default:
        // There has been an issue
        Console.WriteLine ("Issue: More than one document found...");
        break;
    }
}
#endregion

#region Public Methods
public void OpenDocument(NSUrl url) {

    Console.WriteLine ("Attempting to open: {0}", url);
    Document = new GenericTextDocument (url);

    // Open the document
    Document.Open ( (success) => {
        if (success) {
            Console.WriteLine ("Document Opened");
        } else
            Console.WriteLine ("Failed to Open Document");
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public void CreateNewDocument() {
    // Create path to new file
    // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
    var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
    var docPath = Path.Combine (docsFolder, TestFilename);
    var ubiq = new NSUrl (docPath, false);

    // Create new document at path 
    Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
    Document = new GenericTextDocument (ubiq);

    // Set the default value
    Document.Contents = "(default value)";

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
        Console.WriteLine ("Save completion:" + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
        } else {
            Console.WriteLine ("Unable to Save Document");
        }
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public bool SaveDocument() {
    bool successful = false;

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
        Console.WriteLine ("Save completion: " + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
            successful = true;
        } else {
            Console.WriteLine ("Unable to Save Document");
            successful=false;
        }
    });

    // Return results
    return successful;
}
#endregion

#region Events
public delegate void DocumentLoadedDelegate(GenericTextDocument document);
public event DocumentLoadedDelegate DocumentLoaded;

internal void RaiseDocumentLoaded(GenericTextDocument document) {
    // Inform caller
    if (this.DocumentLoaded != null) {
        this.DocumentLoaded (document);
    }
}
#endregion
```

### <a name="document-thumbnails"></a>문서 미리 보기

Apple은 응용 프로그램에 대 한 문서를 나열할 때 최상의 사용자 환경을 미리 보기를 사용 하는 것으로 생각 합니다. 그러면 최종 사용자 컨텍스트가 제공 되므로 작업 하려는 문서를 빠르게 식별할 수 있습니다.

IOS 8 이전에는 문서 미리 보기에 사용자 지정 구현이 필요 했습니다. IOS 8의 새로운 기능은 개발자가 문서 미리 보기를 빠르게 사용할 수 있도록 하는 파일 시스템 특성입니다.

#### <a name="retrieving-document-thumbnails"></a>문서 축소판 그림 검색 

API, `GetPromisedItemResourceValue` `GetPromisedItemResourceValues` a`NSUrlThumbnailDictionary`가 반환 되 면 또는 메서드를 호출 합니다. `NSUrl_PromisedItems` 현재이 사전의 `NSThumbnial1024X1024SizeKey` 유일한 키는와 일치 `UIImage`하는 항목입니다.

#### <a name="saving-document-thumbnails"></a>문서 미리 보기 저장

축소판 그림을 저장 하는 가장 쉬운 방법은를 `UIDocument`사용 하는 것입니다. 의 메서드`GetFileAttributesToWrite` 를 호출 하 고미리보기를설정하면문서파일이인경우자동으로저장됩니다.`UIDocument` ICloud 데몬에는이 변경 내용이 표시 되 고 iCloud로 전파 됩니다. Mac OS X에서 빠른 보기 플러그 인을 통해 개발자에 대 한 미리 보기가 자동으로 생성 됩니다.

기존 API에 대 한 수정 사항과 함께 iCloud 기반 문서로 작업 하는 기본 사항을 사용 하 여 Xamarin iOS 8 모바일 응용 프로그램에서 문서 선택 보기 컨트롤러를 구현할 준비가 되었습니다.


## <a name="enabling-icloud-in-xamarin"></a>Xamarin에서 iCloud 사용

문서 선택기를 Xamarin.ios 응용 프로그램에서 사용할 수 있으려면 먼저 응용 프로그램과 Apple을 통해 iCloud 지원을 사용 하도록 설정 해야 합니다. 

다음 단계에서는 iCloud에 대 한 프로 비전 프로세스를 연습 합니다.

1. ICloud 컨테이너를 만듭니다.
2. ICloud App Service를 포함 하는 앱 ID를 만듭니다.
3. 이 앱 ID를 포함 하는 프로 비전 프로필을 만듭니다.

[기능 사용](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) 가이드는 처음 두 단계를 안내 합니다. 프로 비전 프로필을 만들려면 프로 [비전 프로필](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) 가이드의 단계를 따릅니다.



다음 단계에서는 iCloud 용 응용 프로그램을 구성 하는 프로세스를 설명 합니다.

다음을 수행합니다.

1. Mac용 Visual Studio 또는 Visual Studio에서 프로젝트를 엽니다.
2. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 옵션을 선택 합니다.
3. 옵션 대화 상자에서 **IOS 응용 프로그램**을 선택 하 고 **번들 식별자** 가 응용 프로그램에 대해 위에서 만든 **앱 ID** 에 정의 된 것과 일치 하는지 확인 합니다. 
4. **IOS 번들 서명**을 선택 하 고 위에서 만든 **개발자 Id** 및 **프로 비전 프로필** 을 선택 합니다.
5. **확인** 단추를 클릭 하 여 변경 내용을 저장 하 고 대화 상자를 닫습니다.
6. `Entitlements.plist` **솔루션 탐색기** 에서를 마우스 오른쪽 단추로 클릭 하 여 편집기에서 엽니다.

    > [!IMPORTANT]
    > Visual Studio에서 마우스 오른쪽 단추를 클릭 하 고 **연결 프로그램 ...** 을 선택 하 여 자격 편집기를 열어야 할 수도 있습니다. 속성 목록 편집기를 선택 합니다.

7. ICloud, **Icloud 문서** , **키-값 저장소** 및 **cloudkit** **사용** 을 선택 합니다.
8. 위에서 만든 응용 프로그램에 대 한 **컨테이너가** 있는지 확인 합니다. 예: `iCloud.com.your-company.AppName`
9. 파일의 변경 내용을 저장합니다.

자격에 대 한 자세한 내용은 [자격 사용](~/ios/deploy-test/provisioning/entitlements.md) 가이드를 참조 하세요.

위의 설정이 적용 되 면 응용 프로그램은 이제 클라우드 기반 문서와 새 문서 선택 보기 컨트롤러를 사용할 수 있습니다.

## <a name="common-setup-code"></a>일반 설정 코드

문서 선택 보기 컨트롤러를 시작 하기 전에 몇 가지 표준 설정 코드가 필요 합니다. 먼저 응용 프로그램의 `AppDelegate.cs` 파일을 수정 하 여 다음과 같이 만듭니다.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;

namespace DocPicker
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Static Properties
        public const string TestFilename = "test.txt"; 
        #endregion

        #region Computed Properties
        public override UIWindow Window { get; set; }
        public bool HasiCloud { get; set; }
        public bool CheckingForiCloud { get; set; }
        public NSUrl iCloudUrl { get; set; }

        public GenericTextDocument Document { get; set; }
        public NSMetadataQuery Query { get; set; }
        public NSData Bookmark { get; set; }
        #endregion

        #region Private Methods
        private void FindDocument () {
            Console.WriteLine ("Finding Document...");

            // Create a new query and set it's scope
            Query = new NSMetadataQuery();
            Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

            // Build a predicate to locate the file by name and attach it to the query
            var pred = NSPredicate.FromFormat ("%K == %@",
                new NSObject[] {NSMetadataQuery.ItemFSNameKey
                , new NSString(TestFilename)});
            Query.Predicate = pred;

            // Register a notification for when the query returns
            NSNotificationCenter.DefaultCenter.AddObserver (this
                , new Selector("queryDidFinishGathering:")
                , NSMetadataQuery.DidFinishGatheringNotification
                , Query);

            // Start looking for the file
            Query.StartQuery ();
            Console.WriteLine ("Querying: {0}", Query.IsGathering);
        }


        [Export("queryDidFinishGathering:")]
        public void DidFinishGathering (NSNotification notification) {
            Console.WriteLine ("Finish Gathering Documents.");

            // Access the query and stop it from running
            var query = (NSMetadataQuery)notification.Object;
            query.DisableUpdates();
            query.StopQuery();

            // Release the notification
            NSNotificationCenter.DefaultCenter.RemoveObserver (this
                , NSMetadataQuery.DidFinishGatheringNotification
                , query);

            // Load the document that the query returned
            LoadDocument(query);
        }

        private void LoadDocument (NSMetadataQuery query) {
            Console.WriteLine ("Loading Document...");  

            // Take action based on the returned record count
            switch (query.ResultCount) {
            case 0:
                // Create a new document
                CreateNewDocument ();
                break;
            case 1:
                // Gain access to the url and create a new document from
                // that instance
                NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
                var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

                // Load the document
                OpenDocument (url);
                break;
            default:
                // There has been an issue
                Console.WriteLine ("Issue: More than one document found...");
                break;
            }
        }
        #endregion

        #region Public Methods

        public void OpenDocument(NSUrl url) {

            Console.WriteLine ("Attempting to open: {0}", url);
            Document = new GenericTextDocument (url);

            // Open the document
            Document.Open ( (success) => {
                if (success) {
                    Console.WriteLine ("Document Opened");
                } else
                    Console.WriteLine ("Failed to Open Document");
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        public void CreateNewDocument() {
            // Create path to new file
            // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
            var docPath = Path.Combine (docsFolder, TestFilename);
            var ubiq = new NSUrl (docPath, false);

            // Create new document at path 
            Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
            Document = new GenericTextDocument (ubiq);

            // Set the default value
            Document.Contents = "(default value)";

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
                Console.WriteLine ("Save completion:" + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                } else {
                    Console.WriteLine ("Unable to Save Document");
                }
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        /// <summary>
        /// Saves the document.
        /// </summary>
        /// <returns><c>true</c>, if document was saved, <c>false</c> otherwise.</returns>
        public bool SaveDocument() {
            bool successful = false;

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
                Console.WriteLine ("Save completion: " + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                    successful = true;
                } else {
                    Console.WriteLine ("Unable to Save Document");
                    successful=false;
                }
            });

            // Return results
            return successful;
        }
        #endregion

        #region Override Methods
        public override void FinishedLaunching (UIApplication application)
        {

            // Start a new thread to check and see if the user has iCloud
            // enabled.
            new Thread(new ThreadStart(() => {
                // Inform caller that we are checking for iCloud
                CheckingForiCloud = true;

                // Checks to see if the user of this device has iCloud
                // enabled
                var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer(null);

                // Connected to iCloud?
                if (uburl == null)
                {
                    // No, inform caller
                    HasiCloud = false;
                    iCloudUrl =null;
                    Console.WriteLine("Unable to connect to iCloud");
                    InvokeOnMainThread(()=>{
                        var okAlertController = UIAlertController.Create ("iCloud Not Available", "Developer, please check your Entitlements.plist, Bundle ID and Provisioning Profiles.", UIAlertControllerStyle.Alert);
                        okAlertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        Window.RootViewController.PresentViewController (okAlertController, true, null);
                    });
                }
                else
                {   
                    // Yes, inform caller and save location the Application Container
                    HasiCloud = true;
                    iCloudUrl = uburl;
                    Console.WriteLine("Connected to iCloud");

                    // If we have made the connection with iCloud, start looking for documents
                    InvokeOnMainThread(()=>{
                        // Search for the default document
                        FindDocument ();
                    });
                }

                // Inform caller that we are no longer looking for iCloud
                CheckingForiCloud = false;

            })).Start();
                
        }
        
        // This method is invoked when the application is about to move from active to inactive state.
        // OpenGL applications should use this method to pause.
        public override void OnResignActivation (UIApplication application)
        {
        }
        
        // This method should be used to release shared resources and it should store the application state.
        // If your application supports background execution this method is called instead of WillTerminate
        // when the user quits.
        public override void DidEnterBackground (UIApplication application)
        {
            // Trap all errors
            try {
                // Values to include in the bookmark packet
                var resources = new string[] {
                    NSUrl.FileSecurityKey,
                    NSUrl.ContentModificationDateKey,
                    NSUrl.FileResourceIdentifierKey,
                    NSUrl.FileResourceTypeKey,
                    NSUrl.LocalizedNameKey
                };

                // Create the bookmark
                NSError err;
                Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

                // Was there an error?
                if (err != null) {
                    // Yes, report it
                    Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
                }
            }
            catch (Exception e) {
                // Report error
                Console.WriteLine ("Error: {0}", e.Message);
            }
        }
        
        // This method is called as part of the transition from background to active state.
        public override void WillEnterForeground (UIApplication application)
        {
            // Is there any bookmark data?
            if (Bookmark != null) {
                // Trap all errors
                try {
                    // Yes, attempt to restore it
                    bool isBookmarkStale;
                    NSError err;
                    var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

                    // Was there an error?
                    if (err != null) {
                        // Yes, report it
                        Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
                    } else {
                        // Load document from bookmark
                        OpenDocument (srcUrl);
                    }
                }
                catch (Exception e) {
                    // Report error
                    Console.WriteLine ("Error: {0}", e.Message);
                }
            }

        }
        
        // This method is called when the application is about to terminate. Save data, if needed.
        public override void WillTerminate (UIApplication application)
        {
        }
        #endregion

        #region Events
        public delegate void DocumentLoadedDelegate(GenericTextDocument document);
        public event DocumentLoadedDelegate DocumentLoaded;

        internal void RaiseDocumentLoaded(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentLoaded != null) {
                this.DocumentLoaded (document);
            }
        }
        #endregion
    }
}

```

> [!IMPORTANT]
> 위의 코드에는 위의 문서 검색 및 나열 섹션의 코드가 포함 되어 있습니다. 실제 응용 프로그램에 표시 되는 것 처럼 전체에 표시 됩니다. 간단히 하기 위해이 예제는 하드 코드 된 단일 파일 (`test.txt`) 에서만 작동 합니다.

위의 코드는 응용 프로그램의 나머지 부분에서 보다 쉽게 작업할 수 있도록 몇 가지 iCloud 드라이브 바로 가기를 제공 합니다.

다음으로, 문서 선택기를 사용 하거나 클라우드 기반 문서로 작업 하는 모든 뷰 또는 뷰 컨테이너에 다음 코드를 추가 합니다.

```csharp
using CloudKit;
...

#region Computed Properties
/// <summary>
/// Returns the delegate of the current running application
/// </summary>
/// <value>The this app.</value>
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

그러면에 대 `AppDelegate` 한 바로 가기가 추가 되 고 위에서 만든 iCloud 바로 가기에 액세스할 수 있습니다.

이 코드를 사용 하 여 Xamarin iOS 8 응용 프로그램에서 문서 선택기 보기 컨트롤러를 구현 하는 방법을 살펴보겠습니다.

## <a name="using-the-document-picker-view-controller"></a>문서 선택기 보기 컨트롤러 사용

IOS 8 이전에는 앱 내에서 응용 프로그램 외부의 문서를 검색할 수 있는 방법이 없기 때문에 다른 응용 프로그램의 문서에 액세스 하는 것은 매우 어렵습니다.

### <a name="existing-behavior"></a>기존 동작

 [![](document-picker-images/image31.png "기존 동작 개요")](document-picker-images/image31.png#lightbox)

IOS 8 이전의 외부 문서에 액세스 하는 방법을 살펴보겠습니다.

1. 먼저 사용자는 처음에 문서를 만든 응용 프로그램을 열어야 합니다.
1. 문서를 선택 하 고 `UIDocumentInteractionController` 를 사용 하 여 문서를 새 응용 프로그램으로 보냅니다.
1. 마지막으로, 원래 문서의 복사본이 새 응용 프로그램의 컨테이너에 배치 됩니다.


이 문서에서 두 번째 응용 프로그램을 열고 편집할 수 있습니다.

### <a name="discovering-documents-outside-of-an-apps-container"></a>앱 컨테이너 외부에서 문서 검색

IOS 8에서 응용 프로그램은 자체 응용 프로그램 컨테이너 외부의 문서에 쉽게 액세스할 수 있습니다.

 [![](document-picker-images/image32.png "앱 컨테이너 외부에서 문서 검색")](document-picker-images/image32.png#lightbox)

IOS 응용 프로그램은 새 iCloud 문서 `UIDocumentPickerViewController`선택기 ()를 사용 하 여 응용 프로그램 컨테이너 외부에서 직접 검색 하 고 액세스할 수 있습니다. 는 `UIDocumentPickerViewController` 사용자가 권한을 통해 검색 된 문서에 대 한 액세스를 부여 하 고 편집 하는 메커니즘을 제공 합니다.

응용 프로그램은 해당 문서가 iCloud 문서 선택기에 표시 되 고 다른 응용 프로그램에서 해당 문서를 검색 하 고 사용할 수 있도록 옵트인 (opt in) 해야 합니다. Xamarin iOS 8 응용 프로그램이 해당 응용 프로그램 컨테이너를 공유 하도록 하려면 표준 `Info.plist` 텍스트 편집기에서 파일을 편집 하 고 다음 두 줄을 사전의 아래쪽 ( `<dict>...</dict>` 태그 사이)에 추가 합니다.

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

는 `UIDocumentPickerViewController` 사용자가 문서를 선택할 수 있는 훌륭한 새 UI를 제공 합니다. Xamarin iOS 8 응용 프로그램에서 문서 선택 보기 컨트롤러를 표시 하려면 다음을 수행 합니다.

```csharp
using MobileCoreServices;
...

// Allow the Document picker to select a range of document types
        var allowedUTIs = new string[] {
            UTType.UTF8PlainText,
            UTType.PlainText,
            UTType.RTF,
            UTType.PNG,
            UTType.Text,
            UTType.PDF,
            UTType.Image
        };

        // Display the picker
        //var picker = new UIDocumentPickerViewController (allowedUTIs, UIDocumentPickerMode.Open);
        var pickerMenu = new UIDocumentMenuViewController(allowedUTIs, UIDocumentPickerMode.Open);
        pickerMenu.DidPickDocumentPicker += (sender, args) => {

            // Wireup Document Picker
            args.DocumentPicker.DidPickDocument += (sndr, pArgs) => {

                // IMPORTANT! You must lock the security scope before you can
                // access this file
                var securityEnabled = pArgs.Url.StartAccessingSecurityScopedResource();

                // Open the document
                ThisApp.OpenDocument(pArgs.Url);

                // IMPORTANT! You must release the security lock established
                // above.
                pArgs.Url.StopAccessingSecurityScopedResource();
            };

            // Display the document picker
            PresentViewController(args.DocumentPicker,true,null);
        };

pickerMenu.ModalPresentationStyle = UIModalPresentationStyle.Popover;
PresentViewController(pickerMenu,true,null);
UIPopoverPresentationController presentationPopover = pickerMenu.PopoverPresentationController;
if (presentationPopover!=null) {
    presentationPopover.SourceView = this.View;
    presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Down;
    presentationPopover.SourceRect = ((UIButton)s).Frame;
}
```

> [!IMPORTANT]
> 개발자는 외부 문서에 `StartAccessingSecurityScopedResource` 액세스 `NSUrl` 하기 전에의 메서드를 호출 해야 합니다. 문서가 `StopAccessingSecurityScopedResource` 로드 되는 즉시 보안 잠금을 해제 하려면 메서드를 호출 해야 합니다.

### <a name="sample-output"></a>샘플 출력

다음은 iPhone 장치에서 실행 될 때 위의 코드가 문서 선택기를 표시 하는 방법의 예입니다.

1. 사용자가 응용 프로그램을 시작 하면 기본 인터페이스가 표시 됩니다.   
 
    [![](document-picker-images/image33.png "기본 인터페이스가 표시 됩니다.")](document-picker-images/image33.png#lightbox)
1. 사용자가 화면 위쪽에 있는 **작업** 단추를 탭 하면 사용 가능한 공급자 목록에서 **문서 공급자** 를 선택 하 라는 메시지가 표시 됩니다.   
 
    [![](document-picker-images/image34.png "사용 가능한 공급자 목록에서 문서 공급자를 선택 합니다.")](document-picker-images/image34.png#lightbox)
1. 선택한 **문서 공급자**에 대해 **문서 선택기 보기 컨트롤러가** 표시 됩니다.   
 
    [![](document-picker-images/image35.png "문서 선택기 보기 컨트롤러가 표시 됩니다.")](document-picker-images/image35.png#lightbox)
1. 사용자는 **문서 폴더** 를 탭 하 여 해당 내용을 표시 합니다.   
 
    [![](document-picker-images/image36.png "문서 폴더 내용")](document-picker-images/image36.png#lightbox)
1. 사용자가 **문서** 를 선택 하면 **문서 선택** 이 닫힙니다.
1. 주 인터페이스는 다시 표시 되 고, **문서** 는 외부 컨테이너에서 로드 되 고 콘텐츠는 표시 됩니다.


문서 선택기 보기 컨트롤러의 실제 표시는 사용자가 장치에 설치 하 고 문서 선택기 모드가 구현 된 문서 공급자에 따라 달라 집니다. 위의 예에서는 Open 모드를 사용 하 고 있으며, 다른 모드 유형에 대해서는 아래에서 자세히 설명 합니다.

## <a name="managing-external-documents"></a>외부 문서 관리

위에서 설명한 것 처럼 iOS 8 이전에 응용 프로그램은 응용 프로그램 컨테이너의 일부인 문서에만 액세스할 수 있었습니다. IOS 8에서 응용 프로그램은 외부 소스에서 문서에 액세스할 수 있습니다.

 [![](document-picker-images/image37.png "외부 문서 관리 개요")](document-picker-images/image37.png#lightbox)

사용자가 외부 소스에서 문서를 선택 하면 원본 문서를 가리키는 참조 문서가 응용 프로그램 컨테이너에 기록 됩니다.

이 새로운 기능을 기존 응용 프로그램에 추가 하는 `NSMetadataQuery` 데 도움이 되도록 API에 몇 가지 새로운 기능이 추가 되었습니다. 일반적으로 응용 프로그램은 응용 프로그램 컨테이너 내에 상주 하는 문서를 나열 하는 데 사용할 수 있는 문서 범위를 사용 합니다. 이 범위를 사용 하면 응용 프로그램 컨테이너 내의 문서만 계속 표시 됩니다.

새 데이터를 사용 하는 외부 문서 범위를 사용 하면 응용 프로그램 컨테이너 외부에 있는 문서를 반환 하 고 그에 대 한 메타 데이터를 반환 합니다. 는 `NSMetadataItemUrlKey` 문서가 실제로 위치한 URL을 가리킵니다.

응용 프로그램에서 참조 하는 문서에 대 한 작업을 수행 하지 않으려는 경우가 있습니다. 대신, 앱이 참조 문서를 직접 사용 하려고 합니다. 예를 들어, 응용 프로그램은 UI의 응용 프로그램 폴더에 문서를 표시 하거나 사용자가 폴더 내에서 참조를 이동할 수 있도록 합니다.

IOS 8에서는 참조 문서에 `NSMetadataItemUrlInLocalContainerKey` 직접 액세스할 수 있는 새가 제공 됩니다. 이 키는 응용 프로그램 컨테이너의 외부 문서에 대 한 실제 참조를 가리킵니다.

는 `NSMetadataUbiquitousItemIsExternalDocumentKey` 문서가 응용 프로그램의 컨테이너 외부에 있는지 여부를 테스트 하는 데 사용 됩니다. 는 외부 문서의 원래 복사본을 보유 하는 컨테이너의 이름에 액세스 하는 데 사용 됩니다.`NSMetadataUbiquitousItemContainerDisplayNameKey`

### <a name="why-document-references-are-required"></a>문서 참조가 필요한 이유

IOS 8에서 외부 문서에 액세스 하기 위해 참조를 사용 하는 주된 이유는 보안입니다. 응용 프로그램에 다른 응용 프로그램의 컨테이너에 대 한 액세스 권한이 부여 되지 않았습니다. 가 out-of-process로 실행 되 고 시스템 전체에 액세스할 수 있으므로 문서 선택기만이 작업을 수행할 수 있습니다.

응용 프로그램 컨테이너 외부의 문서를 가져오는 유일한 방법은 문서 선택기를 사용 하 고, 선택에서 반환 된 URL은 보안 범위가 지정 된 경우입니다. 보안 범위 URL에는 문서에 대 한 응용 프로그램 액세스 권한을 부여 하는 데 필요한 범위 지정 권한과 함께 문서를 선택 하는 데 필요한 정보만 포함 되어 있습니다.

보안 범위가 지정 된 URL을 문자열로 serialize 한 다음 deserialize 하는 경우 보안 정보가 손실 되 고 URL에서 파일에 액세스할 수 없게 됩니다. 문서 참조 기능은 이러한 Url에서 가리키는 파일을 다시 가져오는 메커니즘을 제공 합니다.

따라서 응용 프로그램에서 참조 문서 `NSUrl` 중 하나 로부터를 가져오는 경우 이미 보안 범위가 연결 되어 있으며 파일에 액세스 하는 데 사용할 수 있습니다. 따라서 개발자는이 모든 정보 및 프로세스를 처리 하기 때문 `UIDocument` 에를 사용 하는 것이 좋습니다.

### <a name="using-bookmarks"></a>책갈피 사용

상태 복원을 수행 하는 경우와 같이 특정 문서에 다시 가져오기 위해 응용 프로그램의 문서를 열거 하는 것은 항상 불가능 합니다. iOS 8은 지정 된 문서를 직접 대상으로 지정 하는 책갈피를 만드는 메커니즘을 제공 합니다.

다음 코드는 `UIDocument`의 `FileUrl` 속성에서 책갈피를 만듭니다.

```csharp
// Trap all errors
try {
    // Values to include in the bookmark packet
    var resources = new string[] {
        NSUrl.FileSecurityKey,
        NSUrl.ContentModificationDateKey,
        NSUrl.FileResourceIdentifierKey,
        NSUrl.FileResourceTypeKey,
        NSUrl.LocalizedNameKey
    };

    // Create the bookmark
    NSError err;
    Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

    // Was there an error?
    if (err != null) {
        // Yes, report it
        Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
    }
}
catch (Exception e) {
    // Report error
    Console.WriteLine ("Error: {0}", e.Message);
}
```

기존 책갈피 API는 외부 파일에 대 한 직접 액세스를 제공 `NSUrl` 하기 위해 저장 하 고 로드할 수 있는 기존에 대 한 책갈피를 만드는 데 사용 됩니다. 다음 코드는 위에서 만든 책갈피를 복원 합니다.

```csharp
if (Bookmark != null) {
    // Trap all errors
    try {
        // Yes, attempt to restore it
        bool isBookmarkStale;
        NSError err;
        var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

        // Was there an error?
        if (err != null) {
            // Yes, report it
            Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
        } else {
            // Load document from bookmark
            OpenDocument (srcUrl);
        }
    }
    catch (Exception e) {
        // Report error
        Console.WriteLine ("Error: {0}", e.Message);
    }
}
```

## <a name="open-vs-import-mode-and-the-document-picker"></a>Open vs. 가져오기 모드 및 문서 선택기

문서 선택기 뷰 컨트롤러는 두 가지 작업 모드를 제공 합니다.

1. **열기 모드** –이 모드에서 사용자가 및 외부 문서를 선택 하면 문서 선택기는 응용 프로그램 컨테이너에 보안 범위가 지정 된 책갈피를 만듭니다.   
 
    [![](document-picker-images/image37.png "응용 프로그램 컨테이너의 보안 범위 책갈피")](document-picker-images/image37.png#lightbox)
1. **가져오기 모드** –이 모드에서 사용자가 및 외부 문서를 선택 하면 문서 선택기에서 책갈피를 만들지 않고 파일을 임시 위치에 복사한 다음이 위치의 문서에 대 한 액세스를 응용 프로그램에 제공 합니다.   
 
    [![](document-picker-images/image38.png "문서 선택은 파일을 임시 위치에 복사 하 고 응용 프로그램에이 위치에 있는 문서에 대 한 액세스를 제공 합니다.")](document-picker-images/image38.png#lightbox)   
 응용 프로그램이 어떤 이유로 든 종료 되 면 임시 위치가 비워지고 파일이 제거 됩니다. 응용 프로그램은 파일에 대 한 액세스를 유지 해야 하는 경우 복사본을 만들어 응용 프로그램 컨테이너에 저장 해야 합니다.


열기 모드는 응용 프로그램에서 다른 응용 프로그램과 공동 작업을 수행 하 고 문서의 모든 변경 내용을 해당 응용 프로그램과 공유 하려는 경우에 유용 합니다. 가져오기 모드는 응용 프로그램에서 다른 응용 프로그램과의 문서 수정 내용을 공유 하지 않으려는 경우에 사용 됩니다.

## <a name="making-a-document-external"></a>외부 문서 만들기

위에서 설명한 것 처럼 iOS 8 응용 프로그램은 자체 응용 프로그램 컨테이너 외부의 컨테이너에 액세스할 수 없습니다. 응용 프로그램은 로컬 또는 임시 위치에 자신의 컨테이너에 쓸 수 있으며, 특별 한 문서 모드를 사용 하 여 응용 프로그램 컨테이너 외부의 결과 문서를 사용자가 선택한 위치로 이동할 수 있습니다.

문서를 외부 위치로 이동 하려면 다음을 수행 합니다.

1. 먼저 로컬 또는 임시 위치에 새 문서를 만듭니다.
1. 새 문서 `NSUrl` 를 가리키는를 만듭니다.
1. 새 문서 선택기 뷰 컨트롤러를 열고의 `NSUrl` `MoveToService` 모드를 사용 하 여이를 전달 합니다. 
1. 사용자가 새 위치를 선택 하면 문서가 현재 위치에서 새 위치로 이동 됩니다.
1. 응용 프로그램을 만드는 중에도 파일에 액세스할 수 있도록 참조 문서는 앱의 응용 프로그램 컨테이너에 기록 됩니다.


다음 코드를 사용 하 여 문서를 외부 위치로 이동할 수 있습니다.`var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

위의 프로세스에서 반환 된 참조 문서는 문서 선택기의 열기 모드로 만든 참조 문서와 동일 합니다. 그러나 응용 프로그램에서 문서에 대 한 참조를 유지 하지 않고 문서를 이동 하려는 경우가 있습니다.

참조를 생성 하지 않고 문서를 이동 하려면 `ExportToService` 모드를 사용 합니다. 예: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

`ExportToService` 모드를 사용 하는 경우 문서는 외부 컨테이너에 복사 되 고 기존 복사본은 원래 위치에 남아 있습니다.

## <a name="document-provider-extensions"></a>문서 공급자 확장

IOS 8을 사용 하는 경우 Apple은 최종 사용자가 실제로 존재 하는 위치에 관계 없이 클라우드 기반 문서에 액세스할 수 있도록 원합니다. 이러한 목표를 달성 하기 위해 iOS 8은 새로운 문서 공급자 확장 메커니즘을 제공 합니다.

### <a name="what-is-a-document-provider-extension"></a>문서 공급자 확장 이란?

간단히 말해서, 문서 공급자 확장은 개발자 또는 타사에서 기존 iCloud 저장소 위치와 정확히 같은 방식으로 액세스할 수 있는 응용 프로그램 대체 문서 저장소에 제공 하는 방법입니다.

사용자는 문서 선택기에서 이러한 대체 저장소 위치 중 하나를 선택할 수 있으며, 동일한 액세스 모드 (열기, 가져오기, 이동 또는 내보내기)를 사용 하 여 해당 위치의 파일 작업을 수행할 수 있습니다.

이는 두 가지 확장을 사용 하 여 구현 됩니다.

- **문서 선택기 확장** – 사용자가 `UIViewController` 대체 저장소 위치에서 문서를 선택할 수 있는 그래픽 인터페이스를 제공 하는 하위 클래스를 제공 합니다. 이 하위 클래스는 문서 선택기 보기 컨트롤러의 일부로 표시 됩니다.
- **파일 제공 확장명** – 실제로 파일 내용을 제공 하는 것을 처리 하는 비 UI 확장입니다. 이러한 확장은 파일 조정 ( `NSFileCoordinator` )을 통해 제공 됩니다. 이는 파일 조정이 필요한 또 다른 중요 한 사례입니다.


다음 다이어그램에서는 문서 공급자 확장을 사용 하는 경우 일반적인 데이터 흐름을 보여 줍니다.

 [![](document-picker-images/image39.png "이 다이어그램은 문서 공급자 확장 작업 시 일반적인 데이터 흐름을 보여 줍니다.")](document-picker-images/image39.png#lightbox)

다음 프로세스가 발생 합니다.

1. 응용 프로그램은 사용자가 사용할 파일을 선택할 수 있는 문서 선택기 컨트롤러를 제공 합니다.
1. 사용자는 대체 파일 위치를 선택 하 고 사용자 `UIViewController` 지정 확장 프로그램을 호출 하 여 사용자 인터페이스를 표시 합니다.
1. 사용자가이 위치에서 파일을 선택 하면 URL이 문서 선택기로 다시 전달 됩니다.
1. 문서 선택기는 파일의 URL을 선택 하 고 사용자가 작업할 수 있도록 응용 프로그램에 반환 합니다.
1. URL은 파일의 콘텐츠를 응용 프로그램에 반환 하기 위해 파일 코디네이터로 전달 됩니다.
1. 파일 코디네이터는 사용자 지정 파일 공급자 확장명을 호출 하 여 파일을 검색 합니다.
1. 파일의 내용이 파일 코디네이터로 반환 됩니다.
1. 파일의 내용이 응용 프로그램에 반환 됩니다.


### <a name="security-and-bookmarks"></a>보안 및 책갈피

이 섹션에서는 책갈피를 통한 보안 및 영구 파일 액세스와 문서 공급자 확장의 작동 방식을 간략히 살펴봅니다. 응용 프로그램 컨테이너에 보안과 책갈피를 자동으로 저장 하는 iCloud 문서 공급자와 달리 문서 공급자 확장은 문서 참조 시스템에 포함 되지 않기 때문에 그렇지 않습니다.

예: 회사 전체의 보안 데이터 저장소를 제공 하는 엔터프라이즈 설정에서 관리자는 기밀 회사 정보를 공용 iCloud 서버에서 액세스 하거나 처리 하지 않도록 합니다. 따라서 기본 제공 문서 참조 시스템은 사용할 수 없습니다.

책갈피 시스템은 여전히 사용할 수 있으며, 책갈피 된 URL을 올바르게 처리 하 고 해당 URL이 가리키는 문서의 내용을 반환 하는 파일 공급자 확장의 책임입니다.

보안을 위해 iOS 8에는 어떤 응용 프로그램에 액세스 하는 응용 프로그램에 대 한 정보를 유지 하는 격리 계층이 파일 공급자 내에 있습니다. 모든 파일 액세스는이 격리 계층에 의해 제어 됩니다.

다음 다이어그램에서는 책갈피와 문서 공급자 확장을 사용 하 여 작업할 때의 데이터 흐름을 보여 줍니다.

 [![](document-picker-images/image40.png "이 다이어그램에서는 책갈피 및 문서 공급자 확장을 사용 하 여 작업할 때의 데이터 흐름을 보여 줍니다.")](document-picker-images/image40.png#lightbox)

다음 프로세스가 발생 합니다.

1. 응용 프로그램이 백그라운드에서 시작 하 여 상태를 유지 해야 하는 경우 을 호출 `NSUrl` 하 여 대체 저장소에 파일에 대 한 책갈피를 만듭니다.
1. `NSUrl`파일 공급자 확장명을 호출 하 여 문서에 대 한 영구 URL을 가져옵니다. 
1. 파일 공급자 확장은 URL을에 문자열로 `NSUrl` 반환 합니다.
1. 는 `NSUrl` URL을 책갈피로 묶어 응용 프로그램에 반환 합니다.
1. 응용 프로그램이 백그라운드에서 시작 하 여 상태를 복원 해야 하는 경우 책갈피를에 `NSUrl` 전달 합니다.
1. `NSUrl`파일의 URL을 사용 하 여 파일 공급자 확장명을 호출 합니다.
1. 파일 확장명 공급자는 파일에 액세스 하 고 파일의 위치를로 `NSUrl` 반환 합니다.
1. 파일 위치는 보안 정보와 함께 제공 되며 응용 프로그램으로 반환 됩니다.


여기에서 응용 프로그램은 파일에 액세스 하 고 정상적으로 작업할 수 있습니다.

### <a name="writing-files"></a>파일 쓰기

이 섹션에서는 문서 공급자 확장을 사용 하 여 대체 위치에 파일을 작성 하는 방법을 간략히 살펴봅니다. IOS 응용 프로그램은 파일 조정을 사용 하 여 응용 프로그램 컨테이너 내에서 디스크에 정보를 저장 합니다. 파일이 성공적으로 작성 되 고 나면 파일 공급자 확장에 변경 내용이 알려집니다.

이 시점에서 파일 공급자 확장은 대체 위치에 파일을 업로드 하기 시작할 수 있습니다 (또는 파일을 더티로 표시 하 고 업로드를 요구).

### <a name="creating-new-document-provider-extensions"></a>새 문서 공급자 확장 만들기

새 문서 공급자 확장을 만드는 과정은이 소개 문서에서 다루지 않습니다. 이 정보는 사용자가 iOS 장치에서 로드 한 확장에 따라 응용 프로그램이 Apple에서 제공 하는 iCloud 위치 외부의 문서 저장소 위치에 액세스할 수 있음을 보여 주기 위해 여기에 제공 됩니다.

개발자는 문서 선택기를 사용 하 고 외부 문서를 사용할 때 이러한 사실을 알고 있어야 합니다. 이러한 문서는 iCloud에서 호스트 되는 것으로 가정 하면 안 됩니다.

저장소 공급자 또는 문서 선택기 확장을 만드는 방법에 대 한 자세한 내용은 [앱 확장 소개](~/ios/platform/extensions.md) 문서를 참조 하세요.

## <a name="migrating-to-icloud-drive"></a>ICloud 드라이브로 마이그레이션

IOS 8에서 사용자는 iOS 7 (및 이전 시스템)에서 사용 되는 기존 iCloud 문서 시스템을 계속 사용 하거나 기존 문서를 새 iCloud 드라이브 메커니즘으로 마이그레이션하도록 선택할 수 있습니다.

Mac OS X Yosemite에서 Apple은 이전 버전과의 호환성을 제공 하지 않으므로 모든 문서를 iCloud 드라이브로 마이그레이션해야 하며 더 이상 장치 간에 업데이트 되지 않습니다.

사용자 계정이 iCloud 드라이브로 마이그레이션된 후에는 iCloud 드라이브를 사용 하는 장치만 해당 장치에서 문서에 변경 내용을 전파할 수 있습니다.

> [!IMPORTANT]
> 이 문서에서 다루는 새로운 기능은 사용자 계정이 iCloud 드라이브로 마이그레이션된 경우에만 사용할 수 있다는 것을 알고 있어야 합니다. 

## <a name="summary"></a>요약

이 문서에서는 iCloud 드라이브 및 새 문서 선택기 보기 컨트롤러를 지 원하는 데 필요한 기존 iCloud Api에 대 한 변경 내용을 설명 했습니다. 파일 조정을 수행 하 고 클라우드 기반 문서로 작업할 때 중요 한 이유입니다. Xamarin.ios 응용 프로그램에서 클라우드 기반 문서를 사용 하도록 설정 하는 데 필요한 설정에 대해 설명 하 고, 문서 선택기 보기 컨트롤러를 사용 하 여 앱의 응용 프로그램 컨테이너 외부 문서에 대 한 작업을 소개 합니다.

또한이 문서에서는 문서 공급자 확장 및 클라우드 기반 문서를 처리할 수 있는 응용 프로그램을 작성할 때 개발자가 알아야 하는 이유에 대해 간략하게 설명 합니다.

## <a name="related-links"></a>관련 링크

- [DocPicker (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-docpicker)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
- [앱 확장 소개](~/ios/platform/extensions.md)
