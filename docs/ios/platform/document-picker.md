---
title: "문서 선택"
description: "문서 선택 뷰 컨트롤러 샌드박스 응용 프로그램의 외부 파일에 대 한 사용자 액세스 권한을 부여합니다. 것은 앱 간의 문서를 공유 하기 위한 간단한 메커니즘입니다. 또한 사용자가 여러 응용 프로그램으로 단일 문서를 편집할 수 있으므로 보다 복잡 한 워크플로가 있습니다. 이 문서에서는 Xamarin.iOS 응용 프로그램에서 문서 선택기를 사용 하 여를 소개 하 고 지 원하는 데 필요한 icloud와 문서에 변경 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 4a8f1632076a12b1737ba8294ac8b2f28f19dc77
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="document-picker"></a>문서 선택

_문서 선택 뷰 컨트롤러 샌드박스 응용 프로그램의 외부 파일에 대 한 사용자 액세스 권한을 부여합니다. 것은 앱 간의 문서를 공유 하기 위한 간단한 메커니즘입니다. 또한 사용자가 여러 응용 프로그램으로 단일 문서를 편집할 수 있으므로 보다 복잡 한 워크플로가 있습니다. 이 문서에서는 Xamarin.iOS 응용 프로그램에서 문서 선택기를 사용 하 여를 소개 하 고 지 원하는 데 필요한 icloud와 문서에 변경 합니다._

문서 선택기에는 문서를 앱 간에 공유할 수 있습니다. 이러한 문서는 다른 응용 프로그램 디렉터리 또는 iCloud에 저장할 수 있습니다. 문서 집합을 통해 공유 [문서 공급자 확장](~/ios/platform/extensions.md) 사용자가 자신의 장치에 설치 합니다. 

문서 앱 및 클라우드 간에 동기화를 유지 하는의 어려움 때문에 소개 하는 일정량 복잡성이 필요 합니다.

## <a name="requirements"></a>요구 사항

이 문서에 제공 된 단계를 완료 하려면 다음이 요구 됩니다.

-  **Xcode 7 및 iOS 8 이상이** – Apple의 Xcode 7 및 iOS 8 또는 최신 Api을 설치 하 고 개발자의 컴퓨터에 구성 해야 합니다.
-  **Visual Studio 또는 Mac 용 Visual Studio** – Mac 용 Visual Studio의 최신 버전을 설치 해야 합니다.
-  **iOS 장치** – iOS 8을 실행 하는 iOS 장치 이상.

## <a name="changes-to-icloud"></a>ICloud에 대 한 변경

Apple의 iCloud 서비스에 문서 선택의 새로운 기능을 구현 하려면 다음과 같은 변경은 적용 했습니다.

-  ICloud 데몬 완전히 다시 작성 되었습니다 CloudKit를 사용 하 여 합니다.
-  기능은 기존 iCloud iCloud 드라이브 이름을 변경 합니다.
-  ICloud에 Microsoft Windows 운영 체제에 대 한 지원이 추가 되었습니다.
-  Mac OS Finder에서 iCloud 폴더 추가 되었습니다.
-  iOS 장치는 Mac OS iCloud 폴더의 콘텐츠를 액세스할 수 있습니다.


## <a name="what-is-a-document"></a>문서는 무엇입니까?

Icloud와의 문서를 참조 하는 경우 하나의 독립 실행형 엔터티 이며 따라서 사용자가 인식 해야 합니다. 사용자는 문서를 수정 하거나 (사용 하 여 전자 메일, 예를 들어) 다른 사용자와 공유할 할 수 있습니다.

몇 가지 형식의 파일을 사용자는 즉시 파일을 인식 페이지와 같은 문서, Keynote 또는 숫자 있습니다. 그러나 iCloud는이 개념에 제한이 없습니다. 예를 들어 (예: 체스 일치) 게임의 상태는 문서로 처리 및 iCloud에 저장 된 수 있습니다. 이 파일은 사용자의 장치 간에 전달 될 수 없습니다 및에 마지막으로 다른 장치에서 게임을 선택할 수 있도록 합니다.

## <a name="dealing-with-documents"></a>문서 처리

Xamarin을 사용한 문서 선택기를 사용 하는 데 필요한 코드를 살펴보지이 문서는 예정 icloud와 문서 작업에 대 한 모범 사례 하 고 문서 선택을 지 원하는 데 필요한 변경 기존 Api에는 다양 합니다.

### <a name="using-file-coordination"></a>파일 조정 사용

여러 다른 위치에서 파일을 수정할 수 있습니다, 때문에 데이터 손실을 방지 하기 위해 조정에 사용 되어야 합니다.

 [![](document-picker-images/image1.png "파일 조정 사용")](document-picker-images/image1.png#lightbox)

위의 그림에서는 살펴보겠습니다.

1.  코디 네이션 파일을 사용 하 여 iOS 장치 문서를 새로 만들고 iCloud 폴더에 저장 합니다.
2.  icloud와 모든 장치에 배포에 대 한 클라우드로 수정된 된 파일을 저장합니다.
3.  연결 된 Mac iCloud 폴더에서에서 수정된 된 파일에 게 표시 하 고 변경 내용을 파일에 복사할 파일 조정을 사용 합니다.
4.  코디 네이션 파일을 사용 하지 않는 장치 파일에 변경 되 고 iCloud 폴더에 저장 합니다. 이러한 변경 내용은 다른 장치에 복제 즉시 적용 됩니다.

원래 가정 된 파일을 편집 iOS 장치 또는 Mac 이제 해당 변경 내용이 손실 및 조정 되지 않은 장치에서 파일의 버전으로 변경 합니다. 데이터 손실을 방지 하기 위해 파일 조정은 클라우드 기반 문서와 함께 작업 하는 경우 반드시 필요 합니다.

### <a name="using-uidocument"></a>UIDocument를 사용 하 여

 `UIDocument` 작업을 손쉽게 (또는 `NSDocument` macOS에서) 개발자에 대 한 모든 중요 한 역할을 수행 하 여 합니다. 응용 프로그램의 UI를 차단 하지 못하도록 하려면 백그라운드 큐를 사용 하 여 빌드한 조정 하 여 파일을 제공 합니다.

 `UIDocument` 여러 노출 개발자 모든 용도 대 한 Xamarin 응용 프로그램의 개발 작업 쉽게 수행할 수 있도록 고급 수준의 Api가 필요 합니다.

다음 코드에서는의 서브 클래스 `UIDocument` 저장 하 고 iCloud에서 텍스트를 검색 하는 데 사용할 수 있는 일반 텍스트 기반 문서를 구현 하려면:

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

`GenericTextDocument` 위에서 설명한 클래스 문서 선택 및 외부 문서 Xamarin.iOS 8 응용 프로그램에서 작업할 때이 문서 전체에서 사용 됩니다.

## <a name="asynchronous-file-coordination"></a>비동기 파일 조정

iOS 8 새 파일 조정 Api를 통해 몇 가지 새로운 비동기 파일 조정 기능을 제공합니다. IOS 8 하기 전에 모든 기존 파일 조정 Api은 완전히 동기 였습니다. 즉, 개발자가 자신의 배경 파일 조정 응용 프로그램의 UI를 차단 하지 않도록 설정 하려면 큐를 구현 해야 합니다.

새 `NSFileAccessIntent` 클래스 파일과 조정이 필요한 형식을 제어 하는 몇 가지 옵션을 가리키는 URL을 포함 합니다. 다음 코드에서는 한 위치에서 파일 이동 의도 사용 하 여:

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

## <a name="discovering-and-listing-documents"></a>검색 하 고 문서를 나열

기존를 사용 하 여 검색 하 고 문서를 나열 하는 방법은 것 `NSMetadataQuery` Api입니다. 이 섹션은 새로운 기능이 추가 설명 `NSMetadataQuery` 문서와 함께 작업 하는 전보다 훨씬 쉬워졌습니다.

### <a name="existing-behavior"></a>기존 동작

8, iOS 전에 `NSMetadataQuery` 와 같은 로컬 픽업 파일 변경에 속도 느리게 나타났습니다: 삭제를 만들고 이름을 바꿉니다.

 [![](document-picker-images/image2.png "NSMetadataQuery 로컬 파일 변경 내용 개요")](document-picker-images/image2.png#lightbox)

위의 다이어그램에서

1.  응용 프로그램 컨테이너에 이미 있는 파일에 대 한 `NSMetadataQuery` 기존 `NSMetadata` 미리 만든 기록과 spooled 응용 프로그램에 즉시 사용할 수 있도록 합니다.
1.  응용 프로그램의 응용 프로그램 컨테이너에 새 파일을 만듭니다.
1.  하기 전에 지연이 `NSMetadataQuery` 응용 프로그램 컨테이너를 수정 하 고 필요한 만듭니다 `NSMetadata` 레코드입니다.


생성에서의 지연으로 인해는 `NSMetadata` 레코드, 응용 프로그램에 두 개의 데이터 원본 열: 클라우드에 대 한 개와 로컬 파일 변경에 대 한 변경 내용을 기반으로 합니다.

### <a name="stitching"></a>연결

8, iOS에서 `NSMetadataQuery` 연결 이라는 새로운 기능에 직접 사용 하기 쉽습니다.

 [![](document-picker-images/image3.png "새로운 기능으로 NSMetadataQuery 호출 연결")](document-picker-images/image3.png#lightbox)

연결을 사용 하 여 위의 다이어그램에:

1.  응용 프로그램 컨테이너에 이미 있는 파일에 대 한 이전 처럼 `NSMetadataQuery` 기존 `NSMetadata` 레코드 미리 만들어 및 스풀링 합니다.
1.  응용 프로그램 파일 조정을 사용 하 여 응용 프로그램 컨테이너에 새 파일을 만듭니다.
1.  응용 프로그램 컨테이너에서 후크 표시를 수정 하 고 호출 `NSMetadataQuery` 필요한 만들려는 `NSMetadata` 레코드입니다.
1.  `NSMetadata` 레코드 파일을 직접 후 만들어지고 응용 프로그램에 사용할 수 있습니다.


연결을 사용 하 여 응용 프로그램이 더 이상 사용 하 여 로컬 모니터링을 데이터 원본 및 클라우드 기반 파일 변경 합니다. 응용 프로그램에 의존할 수 있습니다 이제 `NSMetadataQuery` 직접 합니다.

> [!IMPORTANT]
> **참고**: 위 섹션에 제공 된 대로 응용 프로그램 파일 조정을 사용 하는 경우에 작동 연결 합니다. 코디 네이션 파일을 사용 하지 않으면 Api 기본적 기존 이전 버전 iOS 8 동작으로 표시 됩니다.




### <a name="new-ios-8-metadata-features"></a>새로운 iOS 8 메타 데이터 기능

다음과 같은 새로운 기능에 추가한 `NSMetadataQuery` iOS 8에서에서:

-   `NSMetatadataQuery` 클라우드의 저장 된 로컬이 아닌 문서를 나열할 이제 있습니다.
-  클라우드 기반 문서에 대 한 메타 데이터 정보에 액세스 하려면 새 Api가 추가 되었습니다. 
-  새로운 `NSUrl_PromisedItems` API를 사용할 수 있는 콘텐츠를 로컬로 있을 수 있는 파일의 파일 특성에 액세스 합니다.
-  사용 하 여는 `GetPromisedItemResourceValue` 메서드를 지정된 된 파일에 대 한 정보를 얻거나 사용는 `GetPromisedItemResourceValues` 메서드를 한 번에 둘 이상의 파일에 정보를 가져옵니다.


메타 데이터를 처리 하기 위한 두 개의 새로운 파일 조정 플래그 추가 되었습니다.

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


위의 플래그와 함께 문서 파일의 내용을 사용할 수에 로컬로 사용할 수 있도록 않아도 됩니다.

다음 코드 세그먼트를 사용 하는 방법을 보여 줍니다 `NSMetadataQuery` 을 쿼리하고 특정 파일의 존재 여부에 대 한 존재 하지 않는 경우 파일 빌드:

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

Apple 응용 프로그램에 대 한 문서를 나열 하는 경우 최상의 사용자 환경을 미리 보기를 사용 하는 것을 판단 합니다. 이 작업에 사용할 문서 신속 하 게 식별할 수 있도록 최종 사용자가 컨텍스트를 제공 합니다.

IOS 8 하기 전에 사용자 지정 구현이 필요 문서 미리 보기를 표시 합니다. IOS를 처음 8은 개발자 문서 미리 보기를 신속 하 게 작업할 수 있도록 하는 파일 시스템 특성입니다.

#### <a name="retrieving-document-thumbnails"></a>문서 미리 보기를 검색합니다. 

호출 하 여는 `GetPromisedItemResourceValue` 또는 `GetPromisedItemResourceValues` 메서드 `NSUrl_PromisedItems` API는 `NSUrlThumbnailDictionary`, 반환 됩니다. 이 사전에 현재 있는 유일한 키는 `NSThumbnial1024X1024SizeKey` 조건과 해당 `UIImage`합니다.

#### <a name="saving-document-thumbnails"></a>문서 미리 보기를 저장 하는 중

사용 하 여 미리 보기를 저장 하는 가장 쉬운 방법은 `UIDocument`합니다. 호출 하 여는 `GetFileAttributesToWrite` 의 메서드는 `UIDocument` 및 축소판 그림을 설정, 자동으로 저장 됩니다 문서 파일을 합니다. ICloud 데몬 이러한 변경 내용이 표시 되 고 iCloud에 전파 됩니다. Mac OS X에서 미리 보기는 자동으로 개발자에 대 한 빠른 보기 플러그 인에서 생성 됩니다.

Xamarin iOS 8에서에서 문서 선택 뷰 컨트롤러를 구현할 수는 기존 API의 수정 내용 함께 위치에 따라 icloud와 문서 작업의 기본 모바일 응용 프로그램입니다.


## <a name="enabling-icloud-in-xamarin"></a>Xamarin에서 iCloud를 사용 하도록 설정

문서 선택 Xamarin.iOS 응용 프로그램에서 사용할 수 iCloud 지원 응용 프로그램 및 Apple을 통해 사용할 수 있어야 합니다. 

다음 단계 연습 iCloud에 대 한 프로 비전 과정입니다.

1. Icloud와 컨테이너를 만듭니다.
2. Icloud와 응용 프로그램 서비스를 포함 하는 응용 프로그램 ID를 만듭니다.
3. 이 응용 프로그램 ID를 포함 하는 프로 비전 프로필 만들기

[기능 작업](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) 가이드에서는 처음 두 단계를 안내 합니다. 단계에 따라 프로 비전 프로필을 만들려면는 [프로 비전 프로필](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile) 가이드입니다.



다음 단계 연습 iCloud에 대 한 응용 프로그램 구성 과정:

다음을 수행합니다.

1.  Mac 또는 Visual Studio에 대 한 Visual Studio에서 프로젝트를 엽니다.
2.  에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 옵션을 선택 합니다.
3.  옵션 대화 상자 선택에서 **iOS 응용 프로그램**, 확인 하는 **번들 식별자** 에 정의 된 것과 일치 **앱 ID** 응용 프로그램에 대 한 위에서 만든 합니다. 
4.  선택 **iOS 번들 서명**, 선택는 **개발자 Identity** 및 **프로 비전 프로필** 위에서 만든 합니다.
5.  클릭는 **확인** 단추 변경 내용을 저장 하 고 대화 상자를 닫습니다.
6.  마우스 오른쪽 단추로 클릭 `Entitlements.plist` 에 **솔루션 탐색기** 편집기에서 엽니다.

    > [!IMPORTANT]
> **참고**: Visual Studio에서 선택 하면 편집기를 열려면 자격을 마우스 오른쪽 단추로 클릭 하 여 할 수 **연결 프로그램...** 속성 목록 편집기를 선택 하 고

7.  확인 **iCloud를 사용 하도록 설정** , **icloud와 문서** , **키-값 저장소** 및 **CloudKit** 합니다.
8.  확인 된 **컨테이너** (위에서 만든) 하는 동안 응용 프로그램에 대 한 존재 합니다. 예: `iCloud.com.your-company.AppName`
9.  파일의 변경 내용을 저장합니다.

권한에 대 한 자세한 내용은 참조는 [자격과 작업](~/ios/deploy-test/provisioning/entitlements.md) 가이드입니다.

위치에 위의 설정을 통해 응용 프로그램 이제 צ ְ ײ 클라우드 기반 문서와 새 문서 선택 뷰 컨트롤러입니다.

## <a name="common-setup-code"></a>일반적인 설치 코드

문서 선택 뷰 컨트롤러를 시작 하기 전에 필요한 표준 설치 코드 몇 가지 있습니다. 응용 프로그램의 수정 하 여 시작 `AppDelegate.cs` 파일을 다음과 같이 되도록 합니다.

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
                    // Yes, inform caller and save location the the Application Container
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
> **참고**: 위의 코드는 위의 Discovering 및 문서 나열 하는 섹션에서 코드를 포함 합니다. 것은 여기에 제공 된 전체를 실제 응용 프로그램에 표시 합니다. 편의상이 예제에서는 작업 하드 코드 된 단일 파일 (`test.txt`)만 있습니다.

위의 코드 보다 쉽게 응용 프로그램의 나머지 부분에서 작업할 수 있도록 여러 개의 iCloud 드라이브 바로 가기를 표시 합니다.

다음으로, 보기 또는 뷰 컨테이너 문서 선택기를 사용 하 여 하거나 클라우드 기반 문서 작업에 다음 코드를 추가 합니다.

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

이렇게 하면 추가 대 한 바로 가기는 `AppDelegate` 위에서 만든 iCloud 바로 가기에 액세스 하 고 있습니다.

이 코드 위치에서 Xamarin iOS 8 응용 프로그램에서 문서 선택 뷰 컨트롤러 구현를 살펴보면을 보겠습니다.

## <a name="using-the-document-picker-view-controller"></a>문서 선택 뷰 컨트롤러를 사용 하 여

IOS 8 하기 전에 응용 프로그램 내에서 응용 프로그램 외부의 문서를 확인할 수 없으므로 있기 때문에 다른 응용 프로그램에서 문서에 액세스 하기 매우 어렵습니다 했습니다.

### <a name="existing-behavior"></a>기존 동작

 [![](document-picker-images/image31.png "기존 동작 개요")](document-picker-images/image31.png#lightbox)

IOS 8 하기 전에 외부 문서에 액세스에 대해 살펴보겠습니다.

1.  먼저 사용자는 원래 문서를 만든 응용 프로그램을 열고 해야 합니다.
1.  해당 문서를 선택 및 `UIDocumentInteractionController` 새 응용 프로그램에 문서를 보내는 데 사용 됩니다.
1.  마지막으로 원래 문서의 복사본은 새 응용 프로그램의 컨테이너에 배치 됩니다.


여기에서 문서가을 열고 편집할 두 번째 응용 프로그램에 사용할 수 있습니다.

### <a name="discovering-documents-outside-of-an-apps-container"></a>컨테이너는 응용 프로그램의 외부에 있는 문서를 검색합니다.

IOS 8의에서 응용 프로그램은 자체 응용 프로그램 컨테이너 외부에 있는 문서를 쉽게 액세스할 수 있습니다:

 [![](document-picker-images/image32.png "컨테이너는 응용 프로그램의 외부에 있는 문서를 검색합니다.")](document-picker-images/image32.png#lightbox)

새 icloud와 문서 선택기를 사용 하 여 ( `UIDocumentPickerViewController`)를 직접 검색 하 고 응용 프로그램 컨테이너 외부에서 액세스할 수는 iOS 응용 프로그램입니다. `UIDocumentPickerViewController` 제공 하는 메커니즘에 대 한 액세스를 부여 하 고 편집 하는 것을 사용자에 대 한 사용 권한을 통해 문서를 검색 합니다.

응용 프로그램 해야 옵트인 해당 문서를 icloud와 문서 선택에에서 표시 및 다른 응용 프로그램에 검색 하 고 작업을 사용할 수 있어야 합니다. Xamarin iOS 8 응용 프로그램이 해당 응용 프로그램 컨테이너 공유, 편집을 `Info.plist` 표준 텍스트 편집기에서 파일을 사전의 맨 아래에 다음 두 줄을 추가 (사이 `<dict>...</dict>` 태그):

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

`UIDocumentPickerViewController` 사용자 문서를 선택할 수 있도록 하는 유용한 새 UI를 제공 합니다. Xamarin iOS 8 응용 프로그램 문서 선택 뷰 컨트롤러에 표시 하려면 다음을 수행 합니다.

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
> **참고**: 개발자 호출 해야 합니다는 `StartAccessingSecurityScopedResource` 의 메서드는 `NSUrl` 외부 문서에 액세스할 수 있습니다. `StopAccessingSecurityScopedResource` 잠금을 해제 하는 보안으로 문서에 로드 된 메서드를 호출 해야 합니다.

### <a name="sample-output"></a>샘플 출력

위의 코드는 iPhone 장치에서 실행 하는 경우 문서 선택기를 표시 하는 방법의 예는 다음과 같습니다.

1.  사용자가 응용 프로그램을 시작 하 고는 주 인터페이스 표시 됩니다.   
 
    [![](document-picker-images/image33.png "표시 되는 주 인터페이스")](document-picker-images/image33.png#lightbox)
1.  사용자 탭의 **작업** 화면 위쪽에 단추를 선택 하 라는 메시지가 표시 되 고는 **문서 공급자** 사용 가능한 공급자 목록에서:   
 
    [![](document-picker-images/image34.png "사용 가능한 공급자 목록에서 문서 공급자를 선택 합니다.")](document-picker-images/image34.png#lightbox)
1.  **문서 선택 뷰-컨트롤러** 표시 되는 선택 된 **문서 공급자**:   
 
    [![](document-picker-images/image35.png "표시 되는 문서 선택 뷰-컨트롤러")](document-picker-images/image35.png#lightbox)
1.  에 사용자가을 **문서 폴더** 내용이 표시 되도록 합니다.   
 
    [![](document-picker-images/image36.png "문서 폴더 내용")](document-picker-images/image36.png#lightbox)
1.  사용자가을 선택는 **문서** 및 **문서 선택** 닫혀 있습니다.
1.  주 인터페이스는 다시 표시는 **문서** 외부 컨테이너와 내용이 표시에서 로드 되었습니다.


표시 되는 실제 문서 선택 뷰 컨트롤러 사용자가 장치에 설치 하 고 문서 선택 모드는 구현 된 문서 공급자에 따라 달라 집니다. 위의 예에서 열기 모드를 사용 하는, 다른 모드 유형 아래에서 자세히 살펴봅니다.

## <a name="managing-external-documents"></a>외부 문서 관리

8, iOS 하기 전에, 위에서 설명한 것 처럼 응용 프로그램 문서 컨테이너 응용 프로그램의 일부분 이었던만 액세스할 수 없습니다. IOS 8에서에서 응용 프로그램의 외부 원본에서 문서에 액세스할 수 있습니다.

 [![](document-picker-images/image37.png "관리 외부 문서 개요")](document-picker-images/image37.png#lightbox)

외부 소스에서 문서를 선택할 때 참조 문서는 원래 문서를 가리키는 응용 프로그램 컨테이너에 기록 됩니다.

기존 응용 프로그램에는이 새 기능 추가 지원 하기 위해 몇 가지 새로운 기능에 추가한는 `NSMetadataQuery` API입니다. 일반적으로 응용 프로그램 응용 프로그램 컨테이너 내에 살고 있는 문서를 나열 하려면 어디에서 나 문서 범위를 사용 합니다. 이 범위를 사용 하 여 응용 프로그램 컨테이너 내에서 문서에만 표시할 계속 됩니다.

새 어디에서 나 외부 문서 범위를 사용 하 여 응용 프로그램 컨테이너 외부 live 및에 대 한 메타 데이터를 반환 하는 문서를 반환 합니다. `NSMetadataItemUrlKey` 문서가 실제로 위치한 URL을 가리키고 됩니다.

가끔 응용 프로그램 번째 참조에서 가리키는 문서를 사용 하려고 하지 않습니다. 대신, 응용 프로그램 참조 문서를 직접 사용 하려고 합니다. 예를 들어, 응용 프로그램 ui에서 응용 프로그램의 폴더에 문서를 표시 하거나 사용자 폴더 내부에 대 한 참조를 이동할 수 있도록 할 수 있습니다.

8, iOS에서 새 `NSMetadataItemUrlInLocalContainerKey` 참조 문서를 직접 액세스를 제공 합니다. 이 키는 응용 프로그램 컨테이너에 있는 외부 문서에 대 한 실제 참조를 가리킵니다.

`NSMetadataUbiquitousItemIsExternalDocumentKey` 문서는 외부 응용 프로그램의 컨테이너에 있는지 여부를 테스트 하는 데 사용 됩니다. `NSMetadataUbiquitousItemContainerDisplayNameKey` 외부 문서의 원래 복사본을 보유 하는 컨테이너의 이름에 액세스 하는 데 사용 됩니다.

### <a name="why-document-references-are-required"></a>문서 참조는 필요한 이유

해당 iOS 8 대 한 참조를 사용 하 여 외부 문서에 액세스 하는 주요 이유는 보안입니다. 응용 프로그램이 없습니다. 다른 응용 프로그램의 컨테이너에 액세스 권한을 부여 합니다. 문서 선택만 작업을 수행할 수 있는,이 다르므로 아웃-프로세스의 실행 중인 시스템 전체 액세스 합니다.

응용 프로그램 컨테이너 외부 문서를 시작 하는 유일한 방법은 문서 선택을 사용 하 여 이며 URL에서 반환 된 경우 고 선택기 보안 범위입니다. 보안 범위가 지정 된 URL에는 문서에 대 한 응용 프로그램 액세스 권한을 부여 하는 데 필요한 권한 범위가 지정 된 정보와 함께 문서를 선택 합니다. 충분 한 정보가 들어 있습니다.

보안 범위 URL 문자열로 serialize 된 다음 deserialize 하는 경우 보안 정보는 손실 될 되며 파일의 URL에서 액세스할 수 있는 것을 고려해 야 합니다. 문서 참조 기능은는 이러한 Url에서 가리키는 파일에 다시 가져올 메커니즘을 제공 합니다.

따라서 응용 프로그램을 구입 하는 경우는 `NSUrl` 참조 문서 중 하나에서 이미 연결 된 보안 범위 있으며는 해당 파일에 액세스 하는 데 사용할 수 있습니다. 이러한 이유로 것이 가장 좋습니다 사용 하 여 개발자 `UIDocument` 대부분이 정보 및 프로세스를 처리 하기 때문에 있습니다.

### <a name="using-bookmarks"></a>책갈피 사용

항상 상태 복원을 수행 하는 경우 예를 들어 특정 문서에 복귀할 응용 프로그램의 문서를 열거할 수 있는 것은 아닙니다. iOS 8 직접 지정된 된 문서를 대상으로 하는 책갈피를 만들 수 있는 메커니즘을 제공 합니다.

다음 코드에서 책갈피 만듭니다는 `UIDocument`의 `FileUrl` 속성:

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

기존 책갈피 API는 기존에 대해 책갈피를 만드는 데 사용 되 `NSUrl` 를 저장 하 고 로드할 수는 외부 파일에 직접 액세스할 수 있도록 합니다. 다음 코드는 위에서 만든 책갈피를 복원 합니다.

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

## <a name="open-vs-import-mode-and-the-document-picker"></a>Vs를 엽니다. 가져오기 모드 및 문서 선택

문서 선택 뷰 컨트롤러 작업의 두 가지 모드를 제공합니다.

1.  **모드 엽니다** -이 모드에서는 사용자가을 선택 하 고 외부 문서 문서 선택 응용 프로그램 컨테이너에 있는 보안 범위가 지정 된 책갈피를 만들면 됩니다.   
 
    [![](document-picker-images/image37.png "보안 응용 프로그램 컨테이너에서 책갈피 범위")](document-picker-images/image37.png#lightbox)
1.  **가져오기 모드** -이 모드에서는 사용자가 선택 하 고 외부 문서, 문서 선택 하지 책갈피를 만들어야 하지만 대신, 해당 파일을 임시 위치 복사한이 위치에 있는 문서에 대 한 응용 프로그램 액세스를 제공 합니다.   
 
    [![](document-picker-images/image38.png "문서 선택 파일을 임시 위치에 복사 되며이 위치에 문서에 응용 프로그램 액세스를 제공 합니다.")](document-picker-images/image38.png#lightbox)   
 어떤 이유로 든 응용 프로그램 종료 되 면 일단 임시 위치 비워지고 파일이 제거 합니다. 응용 프로그램을 파일에 대 한 액세스를 유지 관리 하는 경우 복사본을 확인 하 고 응용 프로그램 컨테이너에 넣습니다.


열기 모드는 응용 프로그램이 다른 응용 프로그램과 함께 공동 작업 및 해당 응용 프로그램으로 문서에 대 한 변경 내용을 공유 하고자 할 때 유용 합니다. 가져오기 모드는 응용 프로그램이 다른 응용 프로그램과 문서에 해당 수정 작업을 공유 하지 않을 때 사용 됩니다.

## <a name="making-a-document-external"></a>문서를 외부 만들기

위에서 언급 한 대로 iOS 8 응용 프로그램 컨테이너 응용 프로그램 컨테이너 자체의 외부 액세스 권한이 없는 경우 응용 프로그램은 로컬 또는 임시 위치에는 자체 컨테이너에 쓸 다음 위치를 선택 하는 사용자에 게 응용 프로그램 컨테이너 외부 결과 문서를 이동 하는 특수 문서 모드를 사용 하 여 수 있습니다.

외부 위치에 문서를 이동 하려면 다음을 수행 합니다.

1.  먼저 로컬 또는 임시 위치에서 새 문서를 만듭니다.
1.  만들기는 `NSUrl` 새 문서를 가리키는 합니다.
1.  새 문서 선택 보기 컨트롤러를 열고 전달 된 `NSUrl` 의 모드와 `MoveToService` 합니다. 
1.  새 위치를 선택 되 면 문서 현재 위치에서 새 위치로 이동 됩니다.
1.  참조 문서 파일 만드는 응용 프로그램에서에 여전히 액세스할 수 있도록 응용 프로그램의 응용 프로그램 컨테이너에 기록 됩니다.


외부 위치에 문서를 이동 하려면 다음 코드를 사용할 수 있습니다. `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

위 프로세스에 의해 반환 되는 참조 문서 문서 선택의 열기 모드에 의해 만들어진와 정확히 같습니다. 그러나 응용 프로그램에 대 한 참조를 유지 하지 않고 문서를 이동 하려는 경우가 있습니다.

문서 참조를 생성 하지 않고 이동 하려면 사용 된 `ExportToService` 모드입니다. 예: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

사용 하는 경우는 `ExportToService` 모드에서는 문서 외부 컨테이너에 복사 되 고 기존 복사본은 원래 위치에 유지 됩니다.

## <a name="document-provider-extensions"></a>문서 공급자 확장

Ios 8, Apple 최종 사용자가 실제로 있는 위치에 관계 없이 클라우드 기반 문서에 액세스할 수 있게 되기를 원합니다. 이 목표를 달성 하기 iOS 8는 새 문서 공급자 확장 메커니즘을 제공 합니다.

### <a name="what-is-a-document-provider-extension"></a>문서 공급자 확장 이란?

간단히 말하면 문서 공급자 확장 수 있는 방법이 개발자 또는 제 3 자에 대 한 정확히 같은 방식으로 기존 iCloud 저장소 위치에 액세스할 수 있는 응용 프로그램 대체 문서 저장소를 제공 합니다.

사용자 이러한 대체 저장소 위치 중 하나는 문서 선택에서 선택할 수 있고 (열기, 가져오기, 이동 또는 내보내기)이 해당 위치에 있는 파일로 작업 정확히 동일한 액세스 모드를 사용할 수 있습니다.

이 두 가지 다른 확장을 사용 하 여 구현 됩니다.

-  **선택기 확장 문서** – 제공은 `UIViewController` 한 대체 저장소 위치에서 문서를 선택 하려면 사용자에 대 한 그래픽 인터페이스를 제공 하는 하위 클래스입니다. 이 하위 클래스는 문서 선택 뷰-컨트롤러의 일부로 표시 됩니다.
-  **파일 확장명을 제공** –이 실제로 파일 내용을 제공 하를 처리 하는 UI가 아닌 확장 합니다. 이러한 확장은 파일 조정을 통해 제공 됩니다 ( `NSFileCoordinator` ). 이 경우에 중요 한 파일 조정이 필요 합니다.


다음 다이어그램에서는 문서 공급자 확장 작업 시 일반적인 데이터 흐름을 보여 줍니다.

 [![](document-picker-images/image39.png "이 다이어그램 문서 공급자 확장 작업 시 일반적인 데이터 흐름을 보여 줍니다.")](document-picker-images/image39.png#lightbox)

다음 프로세스가 발생합니다.

1.  응용 프로그램 사용자가 파일을 사용 하려면를 선택 하려면 허용 하려면 문서 선택 컨트롤러를 표시 합니다.
1.  사용자 대체 파일 위치 및 사용자 지정 선택 `UIViewController` 확장 사용자 인터페이스를 표시 하기 위해 호출 됩니다.
1.  사용자가이 위치에서 파일을 선택 하 고 URL 문서 선택기로 다시 전달 됩니다.
1.  파일의 URL을 선택 하 고에서 작동 하도록 사용자에 대 한 응용 프로그램에 반환 하는 문서 선택 합니다.
1.  URL은 응용 프로그램에 파일 내용을 반환 하는 파일 코디네이터에 게 전달 됩니다.
1.  파일 코디네이터 파일을 검색 하려면 사용자 지정 확장명 공급자를 호출 합니다.
1.  파일의 내용은 파일 코디네이터에 게 반환 됩니다.
1.  파일의 내용은 응용 프로그램에 반환 됩니다.


### <a name="security-and-bookmarks"></a>보안 및 책갈피

이 섹션에는 보안 및 영구 파일 문서 공급자 확장 작동 하는 책갈피를 통해 액세스 하는 방법을 빠르게 확인을 소요 됩니다. Icloud와 문서 공급자를 응용 프로그램 컨테이너에 보안 및 책갈피를 자동으로 저장 하는 것으로, 달리 문서 공급자 확장 문서 참조 시스템의 일부가 되지 않기 때문에 하지 않습니다.

예를 들어: 자체 회사 전체의 보안 데이터 저장소를 제공 하는 엔터프라이즈 환경, 관리자가 회사 기밀 정보에 액세스 하거나 공용 iCloud 서버에서 처리를 적용 하지 않습니다. 따라서 기본 제공 문서 참조 시스템을 사용할 수 없습니다.

책갈피 시스템을 계속 사용할 수 있습니다 및 파일 공급자 확장 올바르게 책갈피가 지정 된 URL을 처리 하 고 그가 가리키는 문서의 내용을 반환 해야 합니다.

보안상의 이유로 iOS 8에 대 한 응용 프로그램에는 파일 공급자 내의 식별자에 대 한 액세스 정보를 유지 하는 격리 계층에 있습니다. 파일에 대 한 모든 액세스가 격리 계층에 의해 제어는 점에 유의 해야 합니다.

다음 다이어그램에서는 책갈피 및 문서 공급자 확장 작업 시 데이터 흐름을 보여 줍니다.

 [![](document-picker-images/image40.png "이 다이어그램 책갈피 및 문서 공급자 확장 작업 시 데이터 흐름을 보여 줍니다.")](document-picker-images/image40.png#lightbox)

다음 프로세스가 발생합니다.

1.  응용 프로그램을 백그라운드 입력 및 상태를 유지 해야 합니다. 호출 `NSUrl` 대체 저장소에서 파일에 책갈피를 만들 수 있습니다.
1.  `NSUrl` 문서에 영구 URL을 가져올 확장명 공급자를 호출 합니다. 
1.  에 대 한 문자열로 URL을 반환 하는 확장명 공급자는 `NSUrl` 합니다.
1.  `NSUrl` 책갈피에 URL을 함께 묶는 하 고 응용 프로그램에 반환 합니다.
1.  책갈피를 응용 프로그램에서 백그라운드에서 깨어 났 군 하 상태를 복원 해야 하는 경우 전달 `NSUrl` 합니다.
1.  `NSUrl` 공급자 접두사가 있는 파일의 URL 호출합니다.
1.  파일을 액세스 하 고 파일의 위치를 반환 하는 파일 확장명 공급자 `NSUrl` 합니다.
1.  파일 위치 보안 정보로 제공 됩니다 하 고 응용 프로그램에 반환 됩니다.


여기에서 응용 프로그램 파일 액세스 하 고 일반적인 방법으로 작업할 수 있습니다.

### <a name="writing-files"></a>파일 쓰기

이 섹션에는 개요 문서 공급자 확장 works와 대체 위치에 파일 쓰기 방법 소요 됩니다. IOS 응용 프로그램 정보를 디스크에 저장할지 컨테이너 내에서 응용 프로그램 파일 조정을 사용 합니다. 파일에 성공적으로 기록한 후에 곧 파일 공급자 확장 프로그램 변경의 알림을 받게 됩니다.

이 시점에서 파일 공급자 확장 수 대체 위치에 파일을 업로드를 시작 (또는 파일 더티 및 필요한 업로드로 표시).

### <a name="creating-new-document-provider-extensions"></a>새 문서 공급자 확장 만들기

새 문서 공급자 확장 만들기 소개이 문서의 범위를 벗어났습니다. 이 정보는 여기는 것을 보여 사용자가 iOS 장치에 로드 된 확장에 따라, 응용 프로그램 있을 수 액세스 iCloud 위치를 제공 합니다. Apple 외부 문서 저장소 위치를 제공 합니다.

문서 선택기를 사용 하는 경우 개발자는 이러한 사실을 알고 있어야 하 고 외부 문서를 사용 합니다. 이러한 문서 iCloud에 호스팅되는 가정 하지 있습니다.

저장소 공급자 또는 문서 선택 확장을 만드는 방법에 대 한 자세한 내용은 참조 하십시오는 [응용 프로그램 확장 소개](~/ios/platform/extensions.md) 문서.

## <a name="migrating-to-icloud-drive"></a>Icloud와 드라이브로 마이그레이션

Ios 8에서 사용자가 문서 시스템 기존 iCloud를 사용 하 여 iOS 7 (및 이전 시스템)에 사용 하거나 기존 문서를 새 iCloud 드라이브 메커니즘으로 마이그레이션할 수 있습니다를 계속 하도록 선택할 수 있습니다.

Mac OS X Yosemite, 사과 제공 하지 않습니다는 이전 버전과 호환성 모든 문서 iCloud 드라이브로 마이그레이션해야 할 하거나 더 이상 업데이트할 수 장치 간에 합니다.

사용자 계정에 드라이브 iCloud로 마이그레이션된 장치만 iCloud 드라이브를 사용 하 여 해당 장치에서 문서에 변경 내용을 전파할 수 됩니다.

> [!IMPORTANT]
> **참고**: 개발자에 게이 문서에서 다루는 새 기능을 사용자의 계정이 iCloud 드라이브로 마이그레이션되는 경우에 사용할 수만 있도록 인식 합니다. 

## <a name="summary"></a>요약

이 문서 드라이브 iCloud와 새 문서 선택 보기 컨트롤러를 지 원하는 데 필요한 Api 기존 iCloud에 변경 내용을 검사가 수행 됩니다. 파일 조정 및 것이 중요 클라우드 기반 문서로 작업할 때이 검사가 있습니다. Xamarin.iOS 응용 프로그램에서 클라우드 기반 문서를 사용 하도록 설정 하는 데 필요한 설치 프로그램을 대상 있으며 문서 선택 보기 컨트롤러를 사용 하는 응용 프로그램의 응용 프로그램 컨테이너 외부 문서 작업에 대 한 소개 설명을 제공 합니다.

또한이 문서에서 문서 공급자 확장 클라우드 기반 문서를 처리할 수 있는 응용 프로그램을 작성할 때 개발자를 알고 있어야 하 고 간단 하 게 다룹니다.

## <a name="related-links"></a>관련 링크

- [DocPicker (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
- [응용 프로그램 확장 프로그램 소개](~/ios/platform/extensions.md)
