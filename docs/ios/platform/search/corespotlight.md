---
title: "스포트라이트 코어를 사용 하 여 검색"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: b7db9082f05ea4db41ddb84d34be2ec9113f2ad5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="search-with-core-spotlight"></a>스포트라이트 코어를 사용 하 여 검색

코어 스포트라이트는 ios 9 추가, 편집 또는 앱 내에서 콘텐츠에 대 한 링크를 삭제 하는 데이터베이스와 비슷한 API를 표시 하는 새로운 프레임 워크입니다. 스포트라이트 코어를 사용 하 여 추가 된 항목을 iOS 장치에서 스포트라이트 검색에서 사용할 수 있습니다.

스포트라이트 코어를 사용 하 여 인덱싱할 수 있는 내용 유형에의 예로에 대 한 Apple의 메시지, 메일, 일정 및 메모 앱 확인 합니다. 모두 현재 사용 코어 스포트라이트 검색 결과 제공 합니다.

## <a name="creating-an-item"></a>항목 만들기

다음은 항목을 만들어 및 코어 스포트라이트를 사용 하 여 인덱싱 예입니다.

```csharp
using CoreSpotlight;
...

// Create attributes to describe an item
var attributes = new CSSearchableItemAttributeSet();
attributes.Title = "Test Cloud";
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

이 정보에는 검색 결과에서 다음과 같은 표시:

[![](corespotlight-images/corespotlight01.png "코어 스포트라이트 검색 결과 개요")](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>항목 복원

응용 프로그램에 대 한 핵심 스포트라이트를 통해 검색 결과에 추가 된 항목에 사용자가 누르면는 `AppDelegate` 메서드 `ContinueUserActivity` 라고 (에 대 한이 메서드는 또한 `NSUserActivity`). 예:

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

이 시간 우리는 having 활동에 대 한 검사는 `ActivityType` 의 `CSSearchableItem.ActionType`합니다.

## <a name="updating-an-item"></a>항목 업데이트

때 핵심 스포트라이트도 만든 인덱스 항목 수정 해야 할 제목 또는 미리 보기 이미지는 필요한 경우가 있을 수 있습니다. 이와 같이 변경 하는 인덱스를 처음 만들 때 사용한 것과 동일한 방법을 사용 합니다.
새 만듭니다 `CSSearchableItem` 을 항목을 만들고 새 연결에 사용한 것과 동일한 ID를 사용 하 여 `CSSearchableItemAttributeSet` 수정 된 특성이 포함 된:

[![](corespotlight-images/corespotlight02.png "업데이트 항목 개요")](corespotlight-images/corespotlight02.png#lightbox)

이 항목은 검색 가능한 인덱스에 기록 하는 경우 기존 항목은 새 정보로 업데이트 됩니다.

## <a name="deleting-an-item"></a>항목 삭제

코어 스포트라이트 더 이상 필요한 경우 인덱스 항목을 삭제 하는 여러 가지 방법을 제공 합니다.

첫째, 예를 들어 해당 id로 항목을 삭제할 수 있습니다.

```csharp
// Delete Items by ID
CSSearchableIndex.DefaultSearchableIndex.Delete(new string[]{"1","16"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

다음으로, 도메인 이름으로 인덱스 항목의 그룹을 삭제할 수 있습니다. 예:

```csharp
// Delete by Domain Name
CSSearchableIndex.DefaultSearchableIndex.DeleteWithDomain(new string[]{"domain-name"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

마지막으로, 다음 코드를 사용 하는 모든 인덱스 항목을 삭제할 수 있습니다.

```csharp
// Delete all index items
CSSearchableIndex.DefaultSearchableIndex.DeleteAll((error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```
## <a name="additional-core-spotlight-features"></a>추가 코어 스포트라이트 기능

코어 스포트라이트 인덱스를 정확 하 게 하 고 최신 상태로 유지 하는 데 도움이 되는 다음과 같은 기능에 있습니다.

- **일괄 업데이트 지원** – 응용 프로그램을 만들거나 동시에 큰 인덱스의 그룹을 수정 하는 경우 전체 일괄 처리가 보낼 수 있습니다는 `Index` 의 메서드는 `CSSearchableIndex` 한 번 호출에서 하는 클래스입니다.
- **인덱스 변경에 대응할** 사용 하 여 –는 `CSSearchableIndexDelegate` 앱 검색 가능한 인덱스 변경 내용 및 알림에 응답할 수 있습니다.
- **데이터 보호를 적용** – 데이터 보호 클래스를 사용 하 여 보안을 구현할 수 스포트라이트 코어를 사용 하 여 검색할 수 있는 인덱스에 추가 하는 항목에 있습니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [앱 검색 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
