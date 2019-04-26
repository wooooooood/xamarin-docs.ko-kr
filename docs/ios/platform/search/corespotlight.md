---
title: Xamarin.iOS에서 핵심 스포트라이트 검색
description: 이 문서에서는 Xamarin.iOS 응용 프로그램의 핵심 스포트라이트를 사용 하 여 앱에서 콘텐츠 링크를 제공 하는 방법을 설명 합니다. 만들기, 복원, 업데이트 및 검색 가능한 항목을 삭제 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: fb9ddcc39bd33199dc370897250cd0d74597612f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61248487"
---
# <a name="search-with-core-spotlight-in-xamarinios"></a>Xamarin.iOS에서 핵심 스포트라이트 검색

핵심 스포트라이트는 추가, 편집 또는 앱 내 콘텐츠의 링크가 삭제 하는 데이터베이스와 비슷한 API를 제공 하는 iOS 9 용 새 프레임 워크입니다. 핵심 스포트라이트를 사용 하 여 추가 된 항목을 iOS 장치에 대 한 스포트라이트 검색에서 사용할 수 있습니다.

핵심 스포트라이트를 사용 하 여 인덱싱할 수 있는 내용 유형에의 한 예로, Apple의 메시지, 메일, 일정 및 메모 앱 확인 합니다. 모든 현재 사용 하 여 핵심 스포트라이트 검색 결과 제공 합니다.

## <a name="creating-an-item"></a>항목 만들기

다음은 항목 만들기 및 핵심 스포트라이트를 사용 하 여 인덱싱의 예입니다.

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

사용자 앱에 대 한 핵심 스포트라이트를 통해 검색 결과 추가할 항목을 누르면 합니다 `AppDelegate` 메서드 `ContinueUserActivity` 호출 됩니다 (이 메서드는 또한 `NSUserActivity`). 예를 들어:

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

이 현재 우리는 having 활동에 대 한 검사를 `ActivityType` 의 `CSSearchableItem.ActionType`합니다.

## <a name="updating-an-item"></a>항목 업데이트

때 핵심 스포트라이트를 사용 하 여 만든 인덱스 항목을 수정 해야, 제목 또는 미리 보기 이미지를 변경 해야 하는 등 번 있을 수 있습니다. 이 변경을 수행 하려면 초기에 인덱스를 만들 때 사용한 것과 동일한 방법을 사용 합니다.
새 만들겠습니다 `CSSearchableItem` 항목을 만들고 새 연결에 사용한 것과 동일한 ID를 사용 하 여 `CSSearchableItemAttributeSet` 수정 된 특성을 포함 하 합니다.

[![](corespotlight-images/corespotlight02.png "업데이트 된 항목 개요")](corespotlight-images/corespotlight02.png#lightbox)

이 항목은 검색 가능한 인덱스에 기록 하는 경우 기존 항목은 새 정보로 업데이트 됩니다.

## <a name="deleting-an-item"></a>항목 삭제

핵심 스포트라이트 더 이상 필요한 경우 인덱스 항목을 삭제 하려면 여러 가지 방법으로 제공 합니다.

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

다음으로, 도메인 이름으로 인덱스 항목의 그룹을 삭제할 수 있습니다. 예를 들어:

```csharp
// Delete by Domain Name
CSSearchableIndex.DefaultSearchableIndex.DeleteWithDomain(new string[]{"domain-name"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

마지막으로, 다음 코드를 사용 하 여 모든 인덱스 항목을 삭제할 수 있습니다.

```csharp
// Delete all index items
CSSearchableIndex.DefaultSearchableIndex.DeleteAll((error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```
## <a name="additional-core-spotlight-features"></a>추가 코어 추천 기능

핵심 스포트라이트에 인덱스를 정확 하 고 최신 상태로 유지 하는 데 도움이 되는 다음 기능이 있습니다.

- **업데이트 지원 일괄 처리** – 앱을 만들거나 동시 큰 그룹의 인덱스를 수정 하는 경우 전체 일괄 처리가 보낼 수 있습니다 합니다 `Index` 메서드를 `CSSearchableIndex` 한 번의 호출에서 클래스.
- **인덱스 변경 내용에 응답할** 사용 하 여 –는 `CSSearchableIndexDelegate` 앱에서 검색 가능한 인덱스 변경 내용 및 알림 응답할 수 있습니다.
- **데이터 보호를 적용** – 데이터 보호 클래스를 사용 하 여 보안을 구현할 수 핵심 스포트라이트를 사용 하 여 검색 가능한 인덱스에 추가 하는 항목입니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [앱 검색 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
