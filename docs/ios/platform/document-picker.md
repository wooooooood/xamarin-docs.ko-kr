---
title: Xamarin.iOS에서 문서 선택기
description: 이 문서는 iOS 문서 선택기를 설명 하 고 Xamarin.iOS에서 사용 하는 방법에 설명 합니다. ICloud, 문서, 일반적인 설치 코드, 문서 공급자 확장 및 자세히 살펴보기를 걸립니다.
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: ac77bbc27784d1b836f4a1d26365acd65ac63ae7
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111397"
---
# <a name="document-picker-in-xamarinios"></a>Xamarin.iOS에서 문서 선택기

문서 선택기에는 문서를를 앱 간에 공유할 수 있습니다. 이러한 문서 또는 다른 앱의 디렉터리 iCloud에 저장 될 수 있습니다. 집합을 통해 공유 문서 [문서 공급자 확장](~/ios/platform/extensions.md) 사용자가 자신의 장치에 설치 합니다. 

문서 앱 및 클라우드 간에 동기화를 유지 하는 어려움을 인해 일정량의 필요한 복잡성을 초래 합니다.

## <a name="requirements"></a>요구 사항

이 문서에 제공 된 단계를 완료 하려면 다음이 요구 됩니다.

-  **Xcode 7 및 iOS 8 이상** – Apple의 Xcode 7 및 iOS 8 또는 최신 Api 설치 하 고 개발자의 컴퓨터에서 구성 해야 합니다.
-  **Visual Studio 또는 Mac 용 Visual Studio** – Mac 용 Visual Studio의 최신 버전을 설치 해야 합니다.
-  **iOS 장치** – iOS 8을 실행 하는 iOS 장치 이상.

## <a name="changes-to-icloud"></a>ICloud로 변경

문서 선택기의 새로운 기능을 구현 하려면 Apple의 iCloud 서비스에 다음 변경 내용은 적용 했습니다.

-  ICloud 디먼 완전히 다시 작성 되었습니다 CloudKit을 사용 합니다.
-  된 기능을 기존 icloud 와를 iCloud를 드라이브 이름이 바뀝니다.
-  ICloud에 Microsoft Windows OS에 대 한 지원이 추가 되었습니다.
-  Mac OS 파인더에서 iCloud 폴더를 추가 되었습니다.
-  iOS 장치는 Mac OS iCloud 폴더의 콘텐츠를 액세스할 수 있습니다.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

## <a name="what-is-a-document"></a>문서 란?

ICloud에 문서를 참조 하는 경우 독립 실행형 단일 엔터티를 이며 사용자가 같이 인식 되어야 합니다. 사용자 문서를 수정 하거나 (사용 하 여 전자 메일, 예를 들어) 다른 사용자와 공유할 할 수도 있습니다.

사용자가 즉시 파일을 인식 페이지와 같은 문서, 키 노트 또는 숫자 파일의 여러 종류가 있습니다. 그러나 iCloud이이 개념에 제한 되지 않습니다. 예를 들어, (예: Chess 일치) 게임의 상태 문서로 처리 고 iCloud에 저장 될 수 있습니다. 이 파일은 사용자의 장치 간에 전달 될 수 있습니다 및에 마지막으로 다른 장치에서 게임을 선택할 수 있도록 합니다.

## <a name="dealing-with-documents"></a>문서를 사용 하 여 처리

Xamarin을 사용한 문서 선택기를 사용 하는 데 필요한 코드를 살펴보기,이 문서에서는 하려는 icloud와 문서를 사용 하 여 작업에 대 한 모범 사례를 설명 하 고 문서 선택기를 지 원하는 데 필요한 다양 한 기존 Api에 대 한 수정 합니다.

### <a name="using-file-coordination"></a>조정 파일을 사용 하 여

여러 다른 위치에서 파일을 수정할 수 있습니다, 되므로 조정 데이터 손실을 방지 하기 위해 사용 되어야 합니다.

 [![](document-picker-images/image1.png "조정 파일을 사용 하 여")](document-picker-images/image1.png#lightbox)

위의 그림에서는 살펴보겠습니다.

1.  조정 파일을 사용 하 여 iOS 장치를 새 문서를 만들고와 iCloud 폴더에 저장 합니다.
2.  iCloud는 모든 장치에 대 한 배포에 대 한 클라우드로 수정된 된 파일을 저장합니다.
3.  연결 된 Mac iCloud 폴더에서에서 수정된 된 파일을 확인 및 조정 파일을 사용 하 여 변경 내용을 파일에 복사 합니다.
4.  조정 파일을 사용 하지 않는 장치 파일에 변경 작업을 수행 하 고 iCloud 폴더에 저장 합니다. 이러한 변경 내용은 다른 장치에 즉시 복제 됩니다.

원래 가정 된 파일을 편집 iOS 장치 또는 Mac 이제 해당 변경 내용이 손실 및 조정 되지 않은 장치에서 파일의 버전으로 변경 합니다. 데이터 손실을 방지 하기 위해 조정 파일은 클라우드 기반 문서를 사용 하 여 작업 하는 경우.

### <a name="using-uidocument"></a>UIDocument를 사용 하 여

 `UIDocument` 간편 하 게 작업 (또는 `NSDocument` macos) 개발자에 대 한 모든 주요 작업을 수행 하 여 합니다. 응용 프로그램의 UI를 차단 하지 않도록 하려면 백그라운드 큐를 사용 하 여 조정 하 여 파일 작성을 제공 합니다.

 `UIDocument` 여러 개의 노출 개발자 모든 목적에 대 한 Xamarin 응용 프로그램의 개발 작업을 쉽게 한 상위 수준 Api에 필요 합니다.

다음 코드의 서브 클래스를 만들고 `UIDocument` 저장 하 고 iCloud에서 텍스트를 검색할 수 있는 일반 텍스트 기반 문서를 구현 하려면:

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

`GenericTextDocument` 문서 선택기 및 외부 문서 Xamarin.iOS 8 응용 프로그램에서 사용 하 여 작업할 때이 문서 전체에서 위에 제공 된 클래스를 사용할 수는 있습니다.

## <a name="asynchronous-file-coordination"></a>비동기 파일 조정

iOS 8 새 파일 조정 Api를 통해 몇 가지 새로운 비동기 파일 조정 기능을 제공합니다. IOS 8 하기 전에 모든 기존 파일 조정 Api 완전히 동기 되었습니다. 즉, 개발자 자신의 백그라운드 파일 조정 응용 프로그램의 UI를 차단 하지 않도록 설정 하려면 큐를 구현 하는 일을 담당 했습니다.

새 `NSFileAccessIntent` 클래스 파일 및 필요한 조정의 형식을 제어 하는 여러 옵션을 가리키는 URL을 포함 합니다. 다음 코드를 한 위치에서 다른 파일을 이동 하는 방법을 보여 줍니다 의도 사용 하 여:

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

## <a name="discovering-and-listing-documents"></a>검색 및 문서를 나열 합니다.

기존 사용 하 여 검색 하 고 문서를 나열 하는 방법은 `NSMetadataQuery` Api. 이 섹션에 추가 된 새 기능을 다룰 `NSMetadataQuery` 문서를 사용 하 여 작업 하는 훨씬 쉬워졌습니다.

### <a name="existing-behavior"></a>기존 동작

IOS 8 이전 `NSMetadataQuery` 느려졌습니다 픽업 로컬 파일 변경 내용에 같은: 삭제를 생성 하 고 이름을 바꿉니다.

 [![](document-picker-images/image2.png "NSMetadataQuery 로컬 파일 변경 개요")](document-picker-images/image2.png#lightbox)

위의 다이어그램에서

1.  응용 프로그램 컨테이너에 이미 있는 파일에 대 한 `NSMetadataQuery` 기존 `NSMetadata` 레코드 미리 만든 및 응용 프로그램에 즉시 사용할 수 있도록 스풀링됩니다.
1.  응용 프로그램 응용 프로그램 컨테이너에서 새 파일을 만듭니다.
1.  전에 지연이 `NSMetadataQuery` 응용 프로그램 컨테이너에 수정 사항을 확인 하 고 필요한 만듭니다 `NSMetadata` 레코드입니다.


생성의 지연으로 인해를 `NSMetadata` 레코드, 응용 프로그램도 두 개의 데이터 원본 열: 로컬 파일 변경 내용에 대 한 하나 및 클라우드 기반 변경 합니다.

### <a name="stitching"></a>연결

Ios 8 `NSMetadataQuery` 중철 라는 새로운 기능을 직접 사용 하기 쉽습니다.

 [![](document-picker-images/image3.png "새로운 기능을 사용 하 여 NSMetadataQuery 호출 연결")](document-picker-images/image3.png#lightbox)

위 다이어그램에서 연결을 사용합니다.

1.  응용 프로그램 컨테이너에 이미 있는 파일에 대 한 이전 처럼 `NSMetadataQuery` 기존 `NSMetadata` 레코드 미리 만들고 스풀링됩니다.
1.  응용 프로그램 조정 파일을 사용 하 여 응용 프로그램 컨테이너에서 새 파일을 만듭니다.
1.  수정 하 고 호출 응용 프로그램 컨테이너에서 후크를 볼 `NSMetadataQuery` 필요한 만들려면 `NSMetadata` 레코드입니다.
1.  `NSMetadata` 레코드 파일 바로 뒤에 생성 되 고 응용 프로그램에 제공 됩니다.


연결을 사용 하 여 응용 프로그램이 더 이상 로컬 모니터링 하는 데이터 원본 열기를 클라우드 기반 파일 변경 합니다. 응용 프로그램에 의존할 수 이제 `NSMetadataQuery` 직접.

> [!IMPORTANT]
> 위 섹션에 제공 된 대로 응용 프로그램 파일 조정을 사용 하는 경우에 연결 합니다. 조정 파일을 사용 하지 않는 경우 Api 기존 사전 iOS 8 동작을 기본값으로 합니다.




### <a name="new-ios-8-metadata-features"></a>새 iOS 8 메타 데이터 기능

다음과 같은 새로운 기능에 추가한 `NSMetadataQuery` ios 8:

-   `NSMetatadataQuery` 클라우드에 저장 된 로컬이 아닌 문서를 나열할 수 있습니다.
-  클라우드 기반 문서에서 메타 데이터 정보에 액세스 하려면 새 Api 추가 되었습니다. 
-  새로운 `NSUrl_PromisedItems` 해당 콘텐츠를 로컬로 있을 수 있는 파일의 파일 특성에 액세스 하는 API입니다.
-  사용 합니다 `GetPromisedItemResourceValue` 지정된 된 파일에 대 한 정보를 사용 하 여 메서드를 `GetPromisedItemResourceValues` 메서드를 한 번에 둘 이상의 파일에 정보를 가져오기.


메타 데이터를 처리 하기 위한 두 개의 새 파일 조정 플래그 추가 되었습니다.

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


위의 플래그를 사용 하 여 문서 파일의 내용을 사용할 수 있도록 하 고 로컬로 사용할 필요가 없습니다.

다음 코드 세그먼트를 사용 하는 방법을 보여 줍니다 `NSMetadataQuery` 특정 파일의 존재 여부에 대 한 쿼리 및 존재 하지 않는 경우 파일을 빌드합니다.

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

Apple 응용 프로그램에 대 한 문서를 나열 하는 경우 최상의 사용자 환경을 미리 보기를 사용 하는 느낌입니다. 사용 하려는 문서를 신속 하 게 식별할 수 있습니다 있도록 최종 사용자 컨텍스트를 제공 합니다.

IOS 8 하기 전에 사용자 지정 구현이 필요 문서 미리 보기를 표시 합니다. 새 ios 8은 개발자 문서 미리 보기와 함께 빠르게 사용할 수 있도록 하는 파일 시스템 특성입니다.

#### <a name="retrieving-document-thumbnails"></a>검색 문서 미리 보기 

호출 하 여 합니다 `GetPromisedItemResourceValue` 또는 `GetPromisedItemResourceValues` 메서드 `NSUrl_PromisedItems` API를 `NSUrlThumbnailDictionary`, 반환 됩니다. 이 사전의 현재 유일한 키가 합니다 `NSThumbnial1024X1024SizeKey` 및 해당 일치 `UIImage`합니다.

#### <a name="saving-document-thumbnails"></a>문서 미리 보기를 저장 하는 중

사용 하 여 미리 보기를 저장 하는 가장 쉬운 방법은 `UIDocument`합니다. 호출 하 여는 `GetFileAttributesToWrite` 메서드는 `UIDocument` 미리 보기 설정을 자동으로 저장 됩니다 문서 파일이 되 고 합니다. ICloud 디먼 이러한 변경 내용이 표시 되며 iCloud에 전파 됩니다. Mac OS X에서 미리 보기는 자동으로 개발자를 위한 빠른 보기 플러그 인을 통해 생성 됩니다.

기존 API에 대 한 수정 내용 함께 위치에 따라 iCloud 문서를 사용 하는 기본 사항을 익히는 준비가 Xamarin iOS 8에서에서 문서 선택기 뷰 컨트롤러 구현 모바일 응용 프로그램입니다.


## <a name="enabling-icloud-in-xamarin"></a>Xamarin에서 iCloud를 사용 하도록 설정

문서 선택기는 Xamarin.iOS 응용 프로그램에서 사용할 수 있습니다, iCloud 지원 응용 프로그램 및 Apple을 통해 사용 하도록 설정 해야 합니다. 

다음 단계 연습 iCloud에 대 한 프로 비전 과정입니다.

1. ICloud 컨테이너를 만듭니다.
2. ICloud 앱 서비스를 포함 하는 앱 ID를 만듭니다.
3. 이 앱 ID를 포함 하는 프로 비전 프로필 만들기

합니다 [기능을 사용 하 여 작업](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) 가이드에서는 처음 두 단계를 안내 합니다. 프로 비전 프로필을 만들려면의 단계를 수행 합니다 [프로 비전 프로필](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) 가이드입니다.



다음 단계 연습 iCloud에 대 한 응용 프로그램을 구성 하는 과정:

다음을 수행합니다.

1.  Mac 또는 Visual Studio 용 Visual Studio에서 프로젝트를 엽니다.
2.  에 **솔루션 탐색기**, 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 옵션을 선택 합니다.
3.  옵션 대화 상자에서 **iOS 응용 프로그램**, 되어 있는지 확인 합니다 **번들 식별자** 에 정의 된 일치 **앱 ID** 응용 프로그램에 대 한 위에서 만든 합니다. 
4.  선택 **iOS 번들 서명**를 선택 합니다 **개발자 Id** 하며 **프로 비전 프로필** 위에서 만든 합니다.
5.  클릭 합니다 **확인** 대화 상자를 닫고 변경 내용을 저장 하는 단추입니다.
6.  마우스 오른쪽 단추로 클릭 `Entitlements.plist` 에 **솔루션 탐색기** 편집기에서 엽니다.

    > [!IMPORTANT]
    > Visual Studio에서 해야 자격 편집기를 마우스 오른쪽 단추로 클릭 하 여 선택 **연결 프로그램...** 속성 목록 편집기를 선택 하 고

7.  확인 **iCloud를 사용 하도록 설정** , **iCloud 문서** 를 **키-값 저장소** 고 **CloudKit** 합니다.
8.  확인 합니다 **컨테이너** (위에서 만든) 하는 동안 응용 프로그램에 대 한 존재 합니다. 예: `iCloud.com.your-company.AppName`
9.  파일의 변경 내용을 저장합니다.

자격에 대 한 자세한 내용은 참조는 [자격](~/ios/deploy-test/provisioning/entitlements.md) 가이드입니다.

위치에 위의 설정을 사용 하면 응용 프로그램 클라우드 기반 문서와 새 문서 선택기 뷰 컨트롤러를 사용할 이제 수 있습니다.

## <a name="common-setup-code"></a>일반적인 설치 코드

문서 선택기 뷰 컨트롤러를 사용 하 여 시작 하기 전에 필요한 표준 설치 코드가 있습니다. 응용 프로그램을 수정 하 여 시작 `AppDelegate.cs` 파일을 다음과 같이 표시 되도록 합니다.

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
> 위의 코드는 위의 Discovering 및 나열 하는 문서 섹션에서 코드를 포함합니다. 표시 됩니다 여기서 전체를 실제 응용 프로그램에 표시 됩니다. 편의상이 예제에서는 단일 하드 코드 된 파일을 사용 하 여 작동 (`test.txt`)만 합니다.

위의 코드는 쉽게 응용 프로그램의 나머지 부분에서 작업할 수 있도록 여러 iCloud 드라이브 바로 가기를 제공 합니다.

다음, 뷰 또는 문서 선택기를 사용 하 여 또는 클라우드 기반 문서에 사용 될 컨테이너 보기에 다음 코드를 추가 합니다.

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

에 대 한 바로 가기를 추가 `AppDelegate` 위에서 만든 iCloud 바로 가기에 액세스 하 고 있습니다.

이 코드를 사용 하 여 Xamarin iOS 8 응용 프로그램에서 문서 선택기 뷰 컨트롤러를 구현에 대해를 살펴보겠습니다.

## <a name="using-the-document-picker-view-controller"></a>문서 선택기 뷰 컨트롤러를 사용 하 여

IOS 8 하기 전에 앱 내에서 응용 프로그램 외부에서 문서를 검색할 수 없기 때문에 다른 응용 프로그램에서 문서에 액세스 하는 것이 어려울 했습니다.

### <a name="existing-behavior"></a>기존 동작

 [![](document-picker-images/image31.png "기존 동작 개요")](document-picker-images/image31.png#lightbox)

IOS 8 이전 외부 문서에 액세스에 대해 살펴보겠습니다.

1.  먼저 사용자는 원래 문서를 생성 하는 응용 프로그램을 열려면 해야 합니다.
1.  해당 문서를 선택 하며 `UIDocumentInteractionController` 은 새 응용 프로그램에 문서를 보내는 데 있습니다.
1.  마지막으로 원래 문서의 복사본을 새 응용 프로그램의 컨테이너에 배치 됩니다.


여기에서 문서가 편집을 열고 두 번째 응용 프로그램에 사용할 수 있습니다.

### <a name="discovering-documents-outside-of-an-apps-container"></a>앱의 컨테이너 외부에서 문서를 검색합니다.

Ios 8에서 응용 프로그램은 손쉽게 자신의 응용 프로그램 컨테이너 외부 문서에 액세스할 수 있습니다.

 [![](document-picker-images/image32.png "앱의 컨테이너 외부에서 문서를 검색합니다.")](document-picker-images/image32.png#lightbox)

새 iCloud 문서 선택기를 사용 하 여 ( `UIDocumentPickerViewController`), iOS 응용 프로그램을 직접 검색 하 고 해당 응용 프로그램 컨테이너 외부에서 액세스 합니다. `UIDocumentPickerViewController` 메커니즘에 대 한 액세스를 부여 하 고 편집 된 사용자에 대 한 권한을 통해 문서 검색을 제공 합니다.

응용 프로그램 해야에 옵트인 해당 문서를 icloud와 문서 선택기에에서 표시 되며 다른 응용 프로그램에 검색 하 고 작업을 사용할 수 있습니다. Xamarin iOS 8 응용 프로그램의 응용 프로그램 컨테이너를 공유, 편집 `Info.plist` 표준 텍스트 편집기에서 파일을 사전 맨 아래에 다음 두 줄을 추가 (간에 `<dict>...</dict>` 태그).

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

`UIDocumentPickerViewController` 사용자가 문서를 선택할 수 있는 훌륭한 새 UI를 제공 합니다. Xamarin iOS 8 응용 프로그램에서 문서 선택기 뷰 컨트롤러를 표시 하려면 다음을 수행 합니다.

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
> 개발자를 호출 해야 합니다는 `StartAccessingSecurityScopedResource` 메서드는 `NSUrl` 외부 문서에 액세스할 수 있습니다. `StopAccessingSecurityScopedResource` 문서가 로드 되는 즉시 보안 잠금을 해제 하려면 메서드를 호출 해야 합니다.

### <a name="sample-output"></a>샘플 출력

위의 코드는 iPhone 장치에서 실행 하는 경우에 문서 선택기를 표시 하는 방법의 예는 다음과 같습니다.

1.  사용자가 응용 프로그램을 시작 하 고 기본 인터페이스 표시 됩니다.   
 
    [![](document-picker-images/image33.png "기본 인터페이스 표시 됩니다.")](document-picker-images/image33.png#lightbox)
1.  사용자 탭의 **작업** 화면의 맨 위에 있는 단추를 선택 하 라는 메시지가 표시 됩니다는 **문서 공급자** 사용 가능한 공급자 목록에서:   
 
    [![](document-picker-images/image34.png "사용 가능한 공급자 목록에서 문서 공급자를 선택 합니다.")](document-picker-images/image34.png#lightbox)
1.  합니다 **문서 선택기 뷰 컨트롤러** 표시는 선택한 **문서 공급자**:   
 
    [![](document-picker-images/image35.png "문서 선택기 뷰 컨트롤러 표시 됩니다.")](document-picker-images/image35.png#lightbox)
1.  사용자 탭에 **문서 폴더** 내용이 표시 되도록 합니다.   
 
    [![](document-picker-images/image36.png "문서 폴더 내용")](document-picker-images/image36.png#lightbox)
1.  사용자가 선택를 **문서** 하며 **문서 선택기** 닫혀 있습니다.
1.  기본 인터페이스를 다시 표시 합니다 **문서** 외부 컨테이너를 표시 하는 내용이 로드 됩니다.


문서 선택기 뷰 컨트롤러의 실제 화면을 사용자가 장치에 설치 하는 문서 선택 모드에 구현 된 문서 공급자에 따라 달라 집니다. 위의 예제에는 오픈 모드를 사용 하는, 다른 모드 형식 아래에서 자세히 설명 됩니다.

## <a name="managing-external-documents"></a>외부 문서 관리

IOS 8 하기 전에, 위에서 설명한 것 처럼 응용 프로그램이 해당 응용 프로그램 컨테이너의 일부인 문서에만 액세스할 수 있습니다. IOS 8에서에서 응용 프로그램의 외부 원본에서 문서를 액세스 하려면:

 [![](document-picker-images/image37.png "외부 문서 개요 관리")](document-picker-images/image37.png#lightbox)

사용자가 외부 원본에서 문서를 선택 하면 참조 문서는 원본 문서를 가리키는 응용 프로그램 컨테이너에 기록 됩니다.

기존 응용 프로그램에이 새 기능을 추가 하는 데 도움이 되, 몇 가지 새로운 기능에 추가한는 `NSMetadataQuery` API. 일반적으로 응용 프로그램이 해당 응용 프로그램 컨테이너 내에 살고 있는 문서를 나열 하는 어디에서 나 문서 범위를 사용 합니다. 이 범위를 사용 하 여 응용 프로그램 컨테이너 내에서 문서에만 표시할 계속 됩니다.

새 유비쿼터스 외부 문서 범위를 사용 하면 라이브 응용 프로그램 컨테이너 외부에 메타 데이터를 반환 하는 문서 반환 됩니다. `NSMetadataItemUrlKey` 문서가 실제로 위치한 URL을 가리키도록 합니다.

경우에 따라 응용 프로그램 하지 번째 참조에서 가리키는 문서를 사용 하려고 합니다. 대신 앱 참조 문서를 직접 사용 하려고 합니다. 예를 들어, ui에서 응용 프로그램의 폴더에 문서를 표시 하거나 폴더 내부에 대 한 참조를 이동할 수 있도록 앱 할 수 있습니다.

Ios 8 새 `NSMetadataItemUrlInLocalContainerKey` 참조 문서에 직접 액세스를 제공 되었습니다. 이 키는 응용 프로그램 컨테이너의 외부 문서에 대 한 실제 참조를 가리킵니다.

`NSMetadataUbiquitousItemIsExternalDocumentKey` 문서는 외부 응용 프로그램의 컨테이너에 있는지 여부를 테스트 하는 데 사용 됩니다. `NSMetadataUbiquitousItemContainerDisplayNameKey` 외부 문서 복사본을 보유 하는 컨테이너의 이름에 액세스 하는 데 사용 됩니다.

### <a name="why-document-references-are-required"></a>문서 참조는 필요한 이유

IOS 8 해당 참조를 사용 하 여 외부 문서에 액세스 하는 주된 이유는 보안입니다. 응용 프로그램이 없는 다른 응용 프로그램의 컨테이너에 대 한 액세스를 부여 됩니다. 실행 중인 out of process 이며 와이드 권한이 시스템 때문에 문서 선택기만, 가능 합니다.

응용 프로그램 컨테이너 외부에서 문서를 이동 하는 유일한 방법은 문서 선택기를 사용 하 여 이며 URL에서 반환 하는 경우 선택기 보안 범위입니다. 보안 범위가 지정 된 URL을 문서에 대 한 응용 프로그램 액세스 권한을 부여 하는 데 필요한 범위가 지정 된 권한 함께 문서를 선택 하려면 충분 한 정보를 포함 합니다.

것이 보안 범위가 지정 된 URL을 문자열로 serialize 된 다음 deserialize 하는 경우 보안 정보 손실 되 고이 파일의 URL에서 액세스할 수 있는 것을 명심 해야 합니다. 문서 참조 기능 이러한 Url가 가리키는 파일에 다시 전달 하는 메커니즘을 제공 합니다.

응용 프로그램을 획득 하는 경우는 `NSUrl` 참조 문서 중 하나에서 이미 연결 된 보안 범위 및 파일 액세스에 사용할 수 있습니다. 따라서 것이 가장 좋습니다 사용 하 여 개발자 `UIDocument` 에 모두이 정보와 프로세스를 처리 하기 때문에 있습니다.

### <a name="using-bookmarks"></a>책갈피 사용

항상 상태 복원을 수행 하는 경우 예를 들어 특정 문서를 다시 가져오려면 응용 프로그램의 문서를 열거할 수 있는 것은 아닙니다. iOS 8 직접 지정된 된 문서를 대상으로 하는 책갈피를 만들 수 있는 메커니즘을 제공 합니다.

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

기존 책갈피 API는 기존에 대해 책갈피를 만드는 데 `NSUrl` 저장 및 외부 파일에 직접 액세스할 수 있도록 로드할 수는 있습니다. 다음 코드는 위에서 만든 책갈피를 복원 됩니다.

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

## <a name="open-vs-import-mode-and-the-document-picker"></a>Vs를 엽니다. 가져오기 모드와 문서 선택기

문서 선택기 뷰 컨트롤러 작업의 두 가지 모드를 제공합니다.

1.  **모드를 엽니다** -이 모드에서는 사용자가 선택 하 고 외부 문서 문서 선택기 응용 프로그램 컨테이너에서 보안 범위가 지정 된 책갈피를 만들면 됩니다.   
 
    [![](document-picker-images/image37.png "보안 범위 응용 프로그램 컨테이너의 책갈피")](document-picker-images/image37.png#lightbox)
1.  **가져오기 모드로** -이 모드에서는 사용자가 선택 하 고 외부 문서, 문서 선택기 및 하지 것입니다 해당 책갈피를 만들어야 하지만 대신 임시 위치에 파일 복사, 문서에서이 위치에 대 한 응용 프로그램 액세스를 제공 합니다.   
 
    [![](document-picker-images/image38.png "문서 선택기 파일을 임시 위치에 복사 되며이 위치에 문서에 응용 프로그램 액세스를 제공 합니다.")](document-picker-images/image38.png#lightbox)   
 응용 프로그램이 어떤 이유로 든 종료 되 면 임시 위치를 비운 후 파일이 제거 합니다. 응용 프로그램을 파일에 대 한 액세스를 유지 관리 하는 경우 복사본을 만들고 해당 응용 프로그램 컨테이너에 배치 해야 하 합니다.


열기 모드 응용 프로그램이 다른 응용 프로그램을 사용 하 여 공동 작업 및 해당 응용 프로그램을 사용 하 여 문서에 변경한 내용을 공유 하고자 할 때 유용 합니다. 가져오기 모드는 응용 프로그램이 다른 응용 프로그램을 사용 하 여 해당 문서를 수정을 공유 하지 않으려면 때 사용 됩니다.

## <a name="making-a-document-external"></a>외부 문서를 만드는

위에서 언급 했 듯이 iOS 8 응용 프로그램을 자체 응용 프로그램 컨테이너 외부 컨테이너 액세스 권한이 없습니다. 응용 프로그램 자체 컨테이너를 로컬로 또는 임시 위치에 쓸 후 특별 한 문서 모드를 사용 하 여 응용 프로그램 컨테이너의 외부 결과 문서 위치를 선택 하는 사용자를 이동할 수 있습니다.

문서를 외부 위치로 이동 하려면 다음을 수행 합니다.

1.  로컬 또는 임시 위치에 새 문서를 먼저 만듭니다.
1.  만들기는 `NSUrl` 새 문서를 가리키는 합니다.
1.  새 문서 선택기 뷰 컨트롤러를 열고 전달 된 `NSUrl` 모드를 사용 하 여 `MoveToService` 입니다. 
1.  사용자가 새 위치를 선택 되 면 새 위치로 문서의 현재 위치에서 옮겨집니다.
1.  참조 문서를 만드는 응용 프로그램에서 여전히 파일에 액세스할 수 있도록 앱의 응용 프로그램 컨테이너에 쓰입니다.


다음 코드를 사용 하 여 외부 위치에 문서를 이동할 수 있습니다. `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

위의 프로세스에 의해 반환 된 참조 문서에 문서 선택기의 열기 모드에 의해 만들어진와 정확히 같습니다. 그러나 응용 프로그램 수에 대 한 참조를 유지 하지 않고 문서를 이동 하려는 경우가 있습니다.

문서 참조를 생성 하지 않고 이동 하려면 사용 된 `ExportToService` 모드입니다. 예: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

사용 하는 경우는 `ExportToService` 모드 문서 외부 컨테이너에 복사 되 고 기존 복사본을 원래 위치에 그대로 있습니다.

## <a name="document-provider-extensions"></a>문서 공급자 확장

Apple는 iOS 8 사용 하 여 최종 사용자가 실제로 있는 위치에 관계 없이 클라우드 기반 문서에 액세스할 수 있으려면를 하려고 합니다. 이 목표를 달성 하려면 iOS 8을 새 문서 공급자 확장 메커니즘을 제공 합니다.

### <a name="what-is-a-document-provider-extension"></a>문서 공급자 확장 이란?

간단히 말해서 문서 공급자 확장 방법이 개발자 또는 제 3 자에 대 한 기존 iCloud 저장소 위치로 정확히 동일한 방식으로 액세스할 수 있는 응용 프로그램 대체 문서 저장소로 제공 합니다.

사용자 문서 선택기에서 이러한 대체 저장소 위치 중 하나를 선택할 수 및 (열기, 가져오기, 이동 또는 내보내기)이 해당 위치에서 파일 작업을 정확 하 게 동일한 액세스 모드를 사용할 수 있습니다.

이 두 가지 다른 확장을 사용 하 여 구현 됩니다.

-  **문서 선택기 확장** – 제공 된 `UIViewController` 대체 저장소 위치에서 문서를 선택 하 여 사용자를 위한 그래픽 인터페이스를 제공 하는 하위 클래스입니다. 이 하위 문서 선택기 뷰 컨트롤러의 일부로 표시 됩니다.
-  **파일 확장명이 제공** –이 실제로 파일 콘텐츠를 제공 하는 UI가 아닌 확장 합니다. 이러한 확장 조정 파일을 통해 제공 됩니다 ( `NSFileCoordinator` ). 이 경우 다른 중요 한 파일 조정이 필요 합니다.


다음 다이어그램은 문서 공급자 확장을 사용 하 여 작업할 때 일반적인 데이터 흐름을 보여줍니다.

 [![](document-picker-images/image39.png "이 다이어그램 문서 공급자 확장을 사용 하 여 작업할 때 일반적인 데이터 흐름을 보여 줍니다.")](document-picker-images/image39.png#lightbox)

다음 프로세스에는 다음이 발생합니다.

1.  응용 프로그램 사용자를 사용 하려면 파일을 선택 하도록 허용 하는 문서 선택기 컨트롤러를 표시 합니다.
1.  사용자는 대체 파일 위치 및 사용자 지정 선택 `UIViewController` 확장 사용자 인터페이스를 표시 하기 위해 호출 됩니다.
1.  사용자가이 위치에서 파일을 선택 하 고 URL은 문서 선택기에 다시 전달 합니다.
1.  문서 선택기는 파일의 URL을 선택 하 고 사용자가 작업 하도록 응용 프로그램에 반환 합니다.
1.  URL은 응용 프로그램에 파일 콘텐츠를 반환할 파일 코디네이터에 게 전달 됩니다.
1.  파일 코디네이터 파일을 검색 하는 사용자 지정 파일 공급자 확장을 호출 합니다.
1.  파일의 내용은 파일 코디네이터에 게 반환 됩니다.
1.  파일의 내용은 응용 프로그램에 반환 됩니다.


### <a name="security-and-bookmarks"></a>보안 및 책갈피

이 섹션에서는 보안 및 영구 파일 공급자 확장 문서를 사용 하 여 책갈피가 작동을 통해 액세스 하는 방법 개요를 걸립니다. ICloud 컨테이너 응용 프로그램에 보안 및 책갈피를 자동으로 저장 하는 문서 공급자와 달리 문서 공급자 확장에는 문서 참조 시스템의 일부가 되지 않기 때문에 그렇지 않습니다.

예를 들어: 고유한 회사 전체에서 보안 데이터 저장소를 제공 하는 엔터프라이즈 환경에서 관리자가 회사 기밀 정보 액세스 또는 공용 iCloud 서버에서 처리를 적용 하지 않습니다. 따라서 기본 제공 문서 참조 시스템을 사용할 수 없습니다.

책갈피 시스템을 계속 사용할 수 있습니다 이며 올바르게 책갈피로 지정한 URL을 처리 하 고 여기에서 가리키는 문서의 내용을 반환 파일 공급자 확장 프로그램의 책임입니다.

보안상의 이유로 iOS 8에 대 한 응용 프로그램에는 파일 공급자 내의 식별자에 대 한 액세스 정보를 유지 하는 격리 계층에 있습니다. 파일에 대 한 모든 액세스가 격리 계층에 의해 제어 되는 점에 유의 해야 합니다.

다음 다이어그램은 책갈피 및 문서 공급자 확장을 사용 하 여 작업 하는 경우 데이터 흐름을 보여줍니다.

 [![](document-picker-images/image40.png "이 다이어그램 책갈피 및 문서 공급자 확장을 사용 하 여 작업 하는 경우 데이터 흐름을 보여 줍니다.")](document-picker-images/image40.png#lightbox)

다음 프로세스에는 다음이 발생합니다.

1.  응용 프로그램이 백그라운드 되려고 할 및 상태를 유지 해야 합니다. 호출 `NSUrl` 대체 storage에서 파일에 책갈피를 만들 수 있습니다.
1.  `NSUrl` 문서에 영구 URL을 얻기 위해 파일 공급자 확장을 호출 합니다. 
1.  파일 공급자 확장 하는 문자열로 URL을 반환 합니다.는 `NSUrl` 합니다.
1.  `NSUrl` 책갈피에 URL을 번들 및 응용 프로그램에 반환 합니다.
1.  응용 프로그램에서 백그라운드에서 깨어 났 군 하 상태를 복원 해야 하는 경우 책갈피가 전달 `NSUrl` 합니다.
1.  `NSUrl` 파일의 URL 사용 하 여 파일 공급자 확장을 호출합니다.
1.  파일 확장명 공급자 파일 액세스 및 파일의 위치를 반환 합니다 `NSUrl` 합니다.
1.  파일 위치는 보안 정보를 함께 이며 응용 프로그램에 반환 합니다.


여기에서 응용 프로그램 파일에 액세스 하 고 일반적인 방법으로 작업할 수 있습니다.

### <a name="writing-files"></a>파일 쓰기

이 섹션에서는 문서 공급자 확장 작업을 사용 하 여 대체 위치에 파일 작성 하는 방법을 빠르게 확인을 걸립니다. IOS 응용 프로그램 정보 응용 프로그램 컨테이너 내에서 디스크를 저장 하도록 조정 파일을 사용 합니다. 파일에 성공적으로 작성 한 후에 곧 파일 공급자 확장 프로그램 변경 했음을 알려줍니다.

이 시점에서 파일 공급자 확장 수 대체 위치에 파일 업로드를 시작 (또는 변경 하 고 필요한 업로드 파일로 표시).

### <a name="creating-new-document-provider-extensions"></a>새 문서 공급자 확장명 만들기

새 문서 공급자 확장을 만들어이 기초 문서의 범위를 벗어났습니다. 이 정보에는 여기에 표시 하려면, 사용자가 iOS 장치에 로드 된 확장에 따라 응용 프로그램에 액세스할 수 iCloud 위치를 제공 하는 Apple 외부의 문서 저장소 위치가 제공 됩니다.

문서 선택기를 사용 하는 경우 개발자는이 사실을 알고 있어야 하 고 외부 문서를 사용 합니다. 이러한 문서는 iCloud에서 호스팅되는 가정 하지 해야 합니다.

저장소 공급자 또는 문서 선택기 확장을 만드는 방법에 대 한 자세한 내용은 참조 하십시오 합니다 [앱 확장 소개](~/ios/platform/extensions.md) 문서.

## <a name="migrating-to-icloud-drive"></a>드라이브 iCloud로 마이그레이션

Ios 8의 경우 사용자는 문서 시스템이 기존 iCloud를 사용 하 여 iOS 7 (및 이전 시스템)에서 사용 하거나 새 iCloud 드라이브 메커니즘에 기존 문서를 마이그레이션하도록 선택할 수 있습니다 계속 하도록 선택할 수 있습니다.

Mac OS X yosemite를 Apple 제공 하지 않습니다는 이전 버전과 호환성 모든 문서를 icloud와 드라이브 마이그레이션해야 하거나 더 이상 업데이트할 장치의 합니다.

ICloud 드라이브에 사용자의 계정에서 마이그레이션된 후 장치만 iCloud 드라이브를 사용 하 여 해당 장치에서 문서에 변경 내용을 전파할 수 됩니다.

> [!IMPORTANT]
> 개발자는이 문서에서 다루는 새 기능 에서만 사용할 사용자 계정이 드라이브 iCloud로 마이그레이션된 경우 알고 있어야 합니다. 

## <a name="summary"></a>요약

이 문서에서는 기존 icloud와 iCloud 드라이브 및 새 문서 선택기 뷰 컨트롤러를 지 원하는 데 필요한 Api 변경 내용을 설명 했습니다. 조정 파일 및 해당 중요 한 이유는 클라우드 기반 문서를 작업할 때 이러한 부분이 있습니다. Xamarin.iOS 응용 프로그램에서 클라우드 기반 문서를 사용 하도록 설정 하는 데 필요한 설정에 설명 하 고 문서 선택기 뷰 컨트롤러를 사용 하 여 앱의 응용 프로그램 컨테이너 외부 문서를 사용 하는 소개 살펴보기 지정에 합니다.

또한이 문서에서는 문서 공급자 확장 되며 클라우드 기반 문서를 처리할 수 있는 응용 프로그램을 작성할 때 개발자 하 여 인식 해야 간략하게 설명 합니다.

## <a name="related-links"></a>관련 링크

- [DocPicker (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
- [앱 확장 소개](~/ios/platform/extensions.md)
