---
title: Xamarin.ios에서 NSUserActivity를 사용 하 여 검색
description: 이 문서에서는 NSUserActivity를 인덱싱하고, 스포트라이트 및 Safari에서 검색할 수 있도록 하는 방법을 설명 합니다. 검색 결과에서 NSUserActivity 선택에 응답 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 0b673128d675d825d4c0564929dbd3896c09b0c5
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70198502"
---
# <a name="search-with-nsuseractivity-in-xamarinios"></a>Xamarin.ios에서 NSUserActivity를 사용 하 여 검색

`NSUserActivity`는 iOS 8에서 도입 되었으며 전달 데이터를 제공 하는 데 사용 됩니다.
앱의 특정 부분에서 활동을 만든 다음 다른 iOS 장치에서 실행 되는 앱의 다른 인스턴스로 전달할 수 있습니다. 그러면 수신 장치는 이전 장치에서 시작 된 작업을 계속할 수 있습니다. 그러면 사용자가 남은 위치에서 바로 이동 합니다. 핸드 오프를 사용 하는 방법에 대 한 자세한 내용은 전달 설명서 [소개](~/ios/platform/handoff.md) 를 참조 하세요.

IOS 9의 `NSUserActivity` 새로운 기능과 함께 인덱싱 (공개적 및 개인용) 하 여 스포트라이트 검색 및 Safari에서 검색할 수 있습니다. 를 `NSUserActivity` 검색 가능한 것으로 표시 하 고 인덱싱 가능한 메타 데이터를 추가 하 여 작업을 iOS 장치의 검색 결과에 나열할 수 있습니다.

[![](nsuseractivity-images/apphistory01.png "앱 기록 개요")](nsuseractivity-images/apphistory01.png#lightbox)

사용자가 앱에서 작업에 속한 검색 결과를 선택 하면 앱이 시작 되 고에 `NSUserActivity` 설명 된 활동이 다시 시작 되어 사용자에 게 표시 됩니다.

의 `NSUserActivity` 다음 속성은 앱 검색을 지 원하는 데 사용 됩니다.

- `EligibleForHandoff`– 인 `true`경우 전달 작업에이 작업을 사용할 수 있습니다.
- `EligibleForSearch`– 인 `true`경우이 작업은 장치 인덱스에 추가 되 고 검색 결과에 표시 됩니다.
- `EligibleForPublicIndexing`– 인 `true`경우이 작업은 Apple의 클라우드 기반 인덱스에 추가 되 고 iOS 장치에 앱을 아직 설치 하지 않은 사용자 (검색을 통해)에 표시 됩니다. 자세한 내용은 아래의 [공용 검색 인덱싱](#public-search-indexing) 섹션을 참조 하세요.
- `Title`– 활동의 제목을 제공 하 고 검색 결과에 표시 됩니다. 사용자는 제목 자체의 텍스트를 검색할 수도 있습니다.
- `Keywords`– 인덱스 되어 최종 사용자가 검색할 수 있는 작업을 설명 하는 데 사용 되는 문자열 배열입니다.
- `ContentAttributeSet`– 작업을 `CSSearchableItemAttributeSet` 자세히 설명 하 고 검색 결과에 다양 한 콘텐츠를 제공 하는 데 사용 되는입니다.
- `ExpirationDate`– 활동이 지정 된 날짜 까지만 표시 되도록 하려면 해당 날짜를 여기에 제공할 수 있습니다.
- `WebpageURL`– 작업을 웹에서 볼 수 있거나 앱이 Safari의 딥 링크를 지 원하는 경우 여기에서 방문 하도록 링크를 설정할 수 있습니다.

## <a name="nsuseractivity-quickstart"></a>NSUserActivity 빠른 시작

앱에서 검색 가능한 `NSUserActivity` 를 구현 하려면 다음 지침을 따르세요.

- [활동 유형 식별자 만들기](#creatingtypeid)
- [활동 만들기](#createactivity)
- [작업에 응답](#respondactivity)
- [공용 검색 인덱싱](#indexing)

를 사용 `NSUserActivity` 하 여 콘텐츠를 검색할 수 있도록 하는 몇 가지 [추가 이점이](#benefits) 있습니다.

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>활동 유형 식별자 만들기

검색 활동을 만들려면 먼저 해당 활동 _유형 식별자_ 를 만들어 식별할 수 있어야 합니다. 활동 유형 식별자는 지정 된 사용자 활동 유형을 고유 하 `NSUserActivityTypes` 게 식별 하는 데 사용 되는 앱의 **info.plist** 파일 배열에 추가 된 짧은 문자열입니다. 앱이 지원 하 고 앱 검색에 노출 하는 각 작업에 대해 배열에 하나의 항목이 있습니다. 

Apple은 충돌을 방지 하기 위해 활동 형식 식별자에 대 한 역방향 DNS 스타일 표기법을 사용 하는 것을 제안 합니다. 예를 들어 `com.company-name.appname.activity` 특정 앱 기반 활동의 경우 `com.company-name.activity` 또는 여러 앱에서 실행할 수 있는 활동의 경우입니다.

활동 유형 식별자는 활동 유형을 식별 하는 `NSUserActivity` 인스턴스를 만들 때 사용 됩니다. 사용자가 검색 결과를 누르는 결과로 활동이 계속 되 면 활동 유형 (앱의 팀 ID와 함께)이 활동을 계속 하기 위해 시작할 앱을 결정 합니다.

이 동작을 지원 하기 위해 필요한 활동 형식 식별자를 만들려면 **info.plist** 파일을 편집 하 고 **원본** 뷰로 전환 합니다. 다음 형식 `NSUserActivityTypes` 으로 키를 추가 하 고 식별자를 만듭니다.

[![](nsuseractivity-images/type01.png "Info.plist 편집기의 NSUserActivityTypes 키 및 필수 식별자")](nsuseractivity-images/type01.png#lightbox)

위의 예제에서는 검색 작업 (`com.xamarin.platform`)에 대해 하나의 새 활동 형식 식별자를 만들었습니다. 사용자 고유의 앱을 만들 때 `NSUserActivityTypes` 배열의 내용을 앱이 지 원하는 활동에 특정 한 활동 유형 식별자로 바꿉니다.

<a name="createactivity" />

## <a name="creating-an-activity"></a>활동 만들기

다음은 장치에서 인덱스를 호스트 하는 검색에 대 한 활동을 만드는 예입니다.

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.BecomeCurrent();
```

다음과 같이 `ContentAttributeSet` `NSUserActivity` 의 속성을 설정 하 여 자세한 정보를 추가할 수 있습니다.

[![](nsuseractivity-images/apphistory02.png "추가 검색 정보 개요")](nsuseractivity-images/apphistory02.png#lightbox)

를 사용 하 `ContentAttributeSet` 여 최종 사용자가 상호 작용할 수 있도록 하는 다양 한 검색 결과를 만들 수 있습니다.

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>작업에 응답

앱에 대 한 검색 결과 (`NSUserActivity`)를 누르는 사용자에 게 응답 하려면 **AppDelegate.cs** `ContinueUserActivity` 파일을 편집 하 고 메서드를 재정의 합니다. 예:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    }

    return true;
}
```

이는 전달 요청에 응답 하는 데 사용 되는 것과 동일한 메서드 재정의입니다. 이제 사용자가 스포트라이트 검색 결과의 앱에서 링크를 클릭 하면 앱이 포그라운드로 (또는 아직 실행 되지 않은 경우 시작 됨) 해당 링크로 표시 되는 콘텐츠, 탐색 또는 기능이 표시 됩니다.

[![](nsuseractivity-images/apphistory03.png "검색에서 이전 상태 복원")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>공용 검색 인덱싱

위에서 언급 한 바와 같이 iOS 9를 사용 하면 사용자가 지정 된 iOS 장치에서 이미 검색 하 여 사용 했던 앱 콘텐츠 및 기능에 대 한 검색 액세스를 쉽게 제공할 수 있습니다. IOS 9는 공용 인덱싱을 사용 하 여 콘텐츠 또는 기능을 아직 검색 하지 않은 사용자 (또는 앱을 설치한 적이 없는 사용자)를 검색 하 여 검색 결과를 얻을 수 있는 방법을 제공 합니다.

공용 인덱싱은 다음과 같은 방식으로 작동 합니다.

1. 앱에 대 한 작업을 만들 때이를 공용으로 표시할 수 있습니다.
2. 공용 활동은 Apple에 전송 되 고 클라우드에서 인덱싱됩니다.
3. 사용자가 장치에서 앱을 조작 하 고 특정 기능이 나 활동에서 나타내는 콘텐츠를 사용 하는 경우 순위가 증가 합니다.
4. 앱이 설치 되어 있지 않은 경우에도 다른 사용자가 자주 사용 하는 공용 결과를 사용할 수 있습니다.
5. 이러한 공용 결과는 스포트라이트 검색 및 Safari (활동에 URL이 포함 된 경우)에 표시 됩니다.

위에서 만든 개인 검색 작업을 사용 하 여 공용으로 확장할 수 있습니다.

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.EligibleForPublicIndexing = true;
activity.BecomeCurrent();
```

에서 작업을 설정 하 여 `EligibleForPublicIndexing = true`공용 인덱싱을 설정 했기 때문에 Apple의 공용 클라우드 인덱스에 자동으로 추가 되는 것을 의미 하지는 않습니다. 먼저 다음 조건을 충족 해야 합니다.

1. 검색 결과에 표시 되 고 여러 사용자가 선택 해야 합니다. 활동 참여 임계값이 충족 될 때까지 결과가 private으로 유지 됩니다.
2. 앱 프로 비전은 사용자 관련 데이터가 인덱싱되지 않고 public이 되지 않도록 합니다.

<a name="benefits" />

## <a name="additional-benefits"></a>추가 혜택

앱에서을 통해 `NSUserActivity` 앱 검색을 채택 하 여 다음과 같은 기능을 얻을 수도 있습니다.

- **핸드** 오프-앱 검색은 전달 (`NSUserActivity`)과 동일한 메커니즘을 사용 하 여 콘텐츠, 탐색 및/또는 기능을 노출 하므로 앱 사용자가 한 장치에서 작업을 시작 하 고 다른 장치에서 계속 작업을 수행할 수 있습니다.
- **Siri 제안** -siri 제안이 일반적으로 수행 하는 표준 제안과 함께 앱에서 근무 중인 자동으로 제안 될 수 있습니다.
- **Siri 스마트 미리 알림** -사용자가 앱의 활동에 대해 알리도록 siri를 요청할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [앱 검색 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [블로그 게시물 & 샘플](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
