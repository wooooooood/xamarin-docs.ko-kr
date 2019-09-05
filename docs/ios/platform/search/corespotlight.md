---
title: Xamarin.ios에서 핵심 스포트라이트를 사용 하 여 검색
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 핵심 스포트라이트를 사용 하 여 앱 내 콘텐츠에 대 한 링크를 제공 하는 방법을 설명 합니다. 검색 가능한 항목을 만들고, 복원 하 고, 업데이트 하 고, 삭제 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/20/2017
ms.openlocfilehash: 845257bc11d24865a01a992e99d39ad6c578b42c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291455"
---
# <a name="search-with-core-spotlight-in-xamarinios"></a>Xamarin.ios에서 핵심 스포트라이트를 사용 하 여 검색

핵심 스포트라이트는 응용 프로그램 내의 콘텐츠에 대 한 링크를 추가, 편집 또는 삭제 하는 데이터베이스와 유사한 API를 제공 하는 iOS 9의 새로운 프레임 워크입니다. 핵심 스포트라이트를 사용 하 여 추가 된 항목은 iOS 장치에서 스포트라이트 검색에서 사용할 수 있습니다.

핵심 스포트라이트를 사용 하 여 인덱싱할 수 있는 콘텐츠 형식의 예는 Apple의 메시지, 메일, 일정 및 메모 앱을 참조 하세요. 현재는 모두 핵심 스포트라이트를 사용 하 여 검색 결과를 제공 합니다.

## <a name="creating-an-item"></a>항목 만들기

다음은 항목을 만들고 핵심 스포트라이트를 사용 하 여 인덱싱하는 예입니다.

```csharp
using CoreSpotlight;
...

// Create attributes to describe an item
var attributes = new CSSearchableItemAttributeSet();
attributes.Title = "App Center Test";
attributes.ContentDescription = "Automatically test your app on 1,000 devices in the cloud.";

// Create item
var item = new CSSearchableItem ("1", "products", attributes);

// Index item
CSSearchableIndex.DefaultSearchableIndex.Index (new CSSearchableItem[]{ item }, (error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

이 정보는 검색 결과에서 다음과 같이 표시 됩니다.

[![](corespotlight-images/corespotlight01.png "핵심 스포트라이트 검색 결과 개요")](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>항목 복원

사용자가 앱 `AppDelegate` 에 대 한 핵심 스포트라이트를 통해 검색 결과에 추가 된 항목을 탭 하면 메서드가 `ContinueUserActivity` 호출 됩니다 (이 메서드는에 `NSUserActivity`도 사용 됨). 예를 들어:

```csharp
public override bool ContinueUserActivity (UIApplication application,
   NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchableItem.ActionType.ToString ()) {
            // Display content for searchable item...
        }
        break;
    }

    return true;
}
```

이번에는가 `ActivityType` `CSSearchableItem.ActionType`인 활동을 확인 합니다.

## <a name="updating-an-item"></a>항목 업데이트

핵심 스포트라이트를 사용 하 여 만든 인덱스 항목을 수정 해야 하는 경우가 있을 수 있습니다. 예를 들어 제목 또는 미리 보기 이미지를 변경 해야 합니다. 이렇게 변경 하려면 인덱스를 처음 만들 때 사용한 것과 동일한 방법을 사용 합니다.
항목을 만들고 수정 `CSSearchableItem` 된 특성을 포함 하는 새 `CSSearchableItemAttributeSet` 을 연결 하는 데 사용 된 것과 동일한 ID를 사용 하 여 새를 만듭니다.

[![](corespotlight-images/corespotlight02.png "항목 업데이트 개요")](corespotlight-images/corespotlight02.png#lightbox)

이 항목이 검색 가능한 인덱스에 기록 되 면 기존 항목이 새 정보로 업데이트 됩니다.

## <a name="deleting-an-item"></a>항목 삭제

핵심 스포트라이트는 더 이상 필요 하지 않은 인덱스 항목을 삭제 하는 여러 가지 방법을 제공 합니다.

먼저 식별자로 항목을 삭제할 수 있습니다. 예를 들면 다음과 같습니다.

```csharp
// Delete Items by ID
CSSearchableIndex.DefaultSearchableIndex.Delete(new string[]{"1","16"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

다음으로 도메인 이름으로 인덱스 항목 그룹을 삭제할 수 있습니다. 예:

```csharp
// Delete by Domain Name
CSSearchableIndex.DefaultSearchableIndex.DeleteWithDomain(new string[]{"domain-name"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

마지막으로 다음 코드를 사용 하 여 모든 인덱스 항목을 삭제할 수 있습니다.

```csharp
// Delete all index items
CSSearchableIndex.DefaultSearchableIndex.DeleteAll((error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

## <a name="additional-core-spotlight-features"></a>추가 핵심 스포트라이트 기능

핵심 스포트라이트는 인덱스를 정확 하 고 최신 상태로 유지 하는 데 도움이 되는 다음과 같은 기능을 제공 합니다.

- **Batch 업데이트 지원** – 앱에서 동시에 많은 인덱스 그룹을 만들거나 수정 해야 하는 경우 한 번의 호출로 `Index` 전체 일괄 처리를 `CSSearchableIndex` 클래스의 메서드로 보낼 수 있습니다.
- **인덱스 변경 내용에 응답** – 앱 `CSSearchableIndexDelegate` 을 사용 하면 검색 가능한 인덱스에서 변경 내용 및 알림에 응답할 수 있습니다.
- **데이터 보호 적용** – 데이터 보호 클래스를 사용 하 여 핵심 스포트라이트를 사용 하 여 검색 가능한 인덱스에 추가 하는 항목에 대 한 보안을 구현할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [앱 검색 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
