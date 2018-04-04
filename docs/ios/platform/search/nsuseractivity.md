---
title: NSUserActivity 사용 하 여 검색
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 803fcce359bbe27ea19901afa766f5b7f4692e0c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="search-with-nsuseractivity"></a>NSUserActivity 사용 하 여 검색

`NSUserActivity` iOS 8에에서 도입 된 및 전달 하기 위해 데이터를 제공 하는 데 사용 됩니다.
다른 iOS 장치에서 실행 되는 앱의 다른 인스턴스에 전달 될 수 있는 응용 프로그램의 특정 부분에 활동을 만들 수 있습니다. 그런 다음 받는 장치 사용자 중단 오른쪽을 넘겨 이전 장치에서 시작 하는 활동을 계속할 수 있습니다. 전달 하기를 사용 하는 방법은 참조 하십시오 우리의 [핸드 오프 소개](~/ios/platform/handoff.md) 설명서입니다.

IOS 9 새 `NSUserActivity` (공개적으로 및 개인적으로) 인덱스 및 스포트라이트 검색 및 Safari에서 검색할 수 있습니다. 표시 하 여 한 `NSUserActivity` 으로 검색 하 고 추가 인덱싱 가능 메타 데이터를 활동 iOS 장치에서 검색 결과에 나열 수 있습니다.

[![](nsuseractivity-images/apphistory01.png "응용 프로그램 내용 개요")](nsuseractivity-images/apphistory01.png#lightbox)

사용자가 앱에서 활동에 속하는 검색 결과 선택 하는 경우 응용 프로그램을 시작할 및에 설명 된 활동의 `NSUserActivity` 다시 시작 하 고 사용자에 게 표시 됩니다.

다음 속성을 `NSUserActivity` 앱 검색을 지 원하는 데 사용 됩니다.

 - `EligibleForHandoff` - `true`, 전달 작업에이 작업을 사용할 수 있습니다.
 - `EligibleForSearch` - `true`,이 활동을 장치에 인덱스에 추가 하 고 검색 결과에 표시 됩니다.
 - `EligibleForPublicIndexing` - `true`,이 활동 Apple의 클라우드 기반 인덱스에 추가 되며 사람은 iOS 장치에서 앱을 아직 설치 하지 않은입니다 (검색)를 통해 사용자에 게 표시 합니다. 참조는 [공용 검색 인덱싱](#Public-Search-Indexing) 자세한 내용을 보려면 아래 섹션.
 - `Title` – 사용자의 작업에 제목을 제공 하 고 검색 결과에 표시 됩니다. 사용자가 자체 제목의 텍스트를 검색할 수도 있습니다.
 - `Keywords` -인덱싱된 되 고 최종 사용자가 검색 가능한 수행 하는 작업에 설명 하는 데 사용 하는 문자열의 배열이입니다.
 - `ContentAttributeSet` -입니다는 `CSSearchableItemAttributeSet` 더욱 자세하게에서 활동에 설명 하 고 검색 결과에 대 한 풍부한 콘텐츠를 제공 하는 데 사용 합니다.
 - `ExpirationDate` –만 표시할 특정된 날짜에 활동을 사용 하도록 하려는 경우 여기에 해당 날짜를 제공할 수 있습니다.
 - `WebpageURL` – 웹에서 활동을 볼 수 없거나 앱 지 원하는 경우 Safari의 딥 링크를 방문 여기에 대 한 링크를 설정할 수 있습니다.

## <a name="nsuseractivity-quickstart"></a>NSUserActivity 빠른 시작

검색 가능한 구현 하는이 방법은 다음과 `NSUserActivity` 응용 프로그램에서:

- [활동 유형 식별자 만들기](#creatingtypeid)
- [활동 만들기](#createactivity)
- [활동에 응답](#respondactivity)
- [공용 검색 인덱싱](#indexing)

일부 [추가 혜택](#benefits) 사용 하 여 `NSUserActivity` 콘텐츠를 검색할 수 있도록 하 합니다.

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>활동 유형 식별자 만들기

검색 작업을 만들 수 있습니다, 전에 만들어야 할 수는 _활동 유형 식별자_ 를 식별 합니다. 활동 형식 식별자는 짧은 문자열에 추가 된 `NSUserActivityTypes` 응용 프로그램의 배열을 **Info.plist** 지정된 된 사용자 활동 형식을 고유 하 게 식별 하는 데 사용 되는 파일입니다. 응용 프로그램을 지원 하 고 응용 프로그램 검색에 노출 하는 각 활동에 대 한 배열에 하나의 항목이 됩니다. 

Apple 활동 유형 식별자에 대 한 역방향 DNS 스타일 표기법을 사용 하 여 충돌을 피하기 위해 제안 합니다. 예를 들어: `com.company-name.appname.activity` 특정 앱에 대 한 기반 활동 또는 `com.company-name.activity` 여러 앱을 실행할 수 있는 작업에 대 한 합니다.

활동 유형 식별자 만들 때 사용 되는 `NSUserActivity` 활동의 유형을 식별 하는 인스턴스. 활동 검색 결과 눌러 사용자의 결과로 계속 되 면 활동 유형 (함께 응용 프로그램의 팀 ID)를 시작 작업을 계속 하려면는 응용 프로그램을 결정 합니다.

이 동작을 지원 하도록 필요한 작업 유형 식별자를 만들려면 편집는 **Info.plist** 파일을 전환 하는 **소스** 보기. 추가 `NSUserActivityTypes` 키를 다음 형식에서 id가 만들어집니다.

[![](nsuseractivity-images/type01.png "NSUserActivityTypes 키 및 plist 편집기에서 필요한 식별자")](nsuseractivity-images/type01.png#lightbox)

위의 예제에서 검색 작업에 대 한 새로운 활동 유형 식별자를 하나 만든 (`com.xamarin.platform`). 앱을 만들 때의 내용을 대체는 `NSUserActivityTypes` 앱 하 여 특정 활동 유형 식별자와 배열 지원.

<a name="createactivity" />

## <a name="creating-an-activity"></a>활동 만들기

다음은 호스팅되는 장치에서 인덱스 검색에 대 한 활동을 만드는 예입니다.

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

추가할 수도 있습니다 자세한 세부 정보를 설정 하 여는 `ContentAttributeSet` 속성 우리의 `NSUserActivity` 다음과 같습니다.

[![](nsuseractivity-images/apphistory02.png "추가 세부 정보 검색 개요")](nsuseractivity-images/apphistory02.png#lightbox)

사용 하 여는 `ContentAttributeSet` 와 상호 작용 하는 최종 사용자를 유도 하는 풍부한 검색 결과 만들 수 있습니다.

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>활동에 응답

검색 결과에서 누르기 사용자에 게 응답 하도록 (`NSUserActivity`) 앱에서 편집는 **AppDelegate.cs** 파일을 재정의 하는 `ContinueUserActivity` 메서드. 예를 들어:

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

이 전달 요청에 응답 하는 데 사용 하는 동일한 메서드 재정의 note 합니다. 이제 사용자가 스포트라이트 검색 결과에서 앱의 링크를 클릭 하면 앱 포그라운드로 (되거나 될 아직 실행 하지 않은 경우 시작) 하 고 콘텐츠, 탐색 또는 해당 링크로 표시 되는 기능을 표시 됩니다.

[![](nsuseractivity-images/apphistory03.png "검색에서 이전 상태를 복원 합니다.")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>공용 검색 인덱싱

위에서 설명한 것 처럼 iOS 9 쉽게 응용 프로그램 콘텐츠 및 사용자가 이미 검색 하 고 지정 된 iOS 장치에서 사용할 수 있는 기능에 대 한 검색 액세스할 수 있습니다. 공용 인덱싱을 사용 iOS 9를 통해 사용자가 하지 않은 검색 된 콘텐츠나 기능 아직 (또는 심지어 응용 프로그램을 설치 하지 않은 사용자)에 대 한 결과를 얻으려면 해당 검색에 너무 합니다.

공용 인덱싱 다음과 같은 방식으로 작동합니다.

1. 앱에 대 한 활동을 만들 때 공용으로 표시할 수 있습니다.
2. 공용 활동 Apple에 전송 되 고 클라우드에서 인덱싱됩니다.
3. 더 많은 사용자가 장치에서 앱과 상호 작용을 특정 기능이 나 활동에 의해 표현 되는 콘텐츠를 사용 하 여 순위에서 증가 합니다.
4. 설치 된 응용 프로그램 없는 경우에 인기 있는 공용 결과 다른 사용자에 게 제공 됩니다.
5. 이러한 공용 결과에 표시 됩니다 스포트라이트 검색 및 Safari (작업 URL이 포함 됩니다) 하는 경우.

म, 위에서 만든 개인 검색 작업을 수행 하 고 public 이어야 하 수 있습니다.

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

활동에 설정 하 여 공용 인덱싱에 대해 설정 되어 있으므로 방금 `EligibleForPublicIndexing = true`, 있는지 자동으로 추가 됩니다 Apple의 공용 클라우드 인덱스를 의미 하지는 않습니다. 다음 조건이 먼저 충족 되어야 합니다.

1. 검색 결과에 표시 되어야 하 고 많은 사용자가 선택할 수 있습니다. 결과 작업 engagement 임계값을 충족 될 때까지 개인 설정을 유지 합니다.
2. 앱 프로비저닝 모든 사용자 고유의 데이터 인덱싱되며 공개를 방지 합니다.

<a name="benefits" />

## <a name="additional-benefits"></a>추가 혜택

통해 앱 검색을 사용 하 여 `NSUserActivity` 응용 프로그램에서 가져올 수는 다음과 같은 기능이 있습니다.

- **핸드 오프** 앱 검색 콘텐츠를 탐색 공개 및/또는 전달으로 동일한 메커니즘을 사용 하는 기능 때문-(`NSUserActivity`), 하 한 장치에서 작업을 시작 하 고 다른 계속 응용 프로그램의 사용자를 쉽게 할 수 있습니다.
- **Siri 제안** -및 Siri 추천을 사용 하면 일반적으로 표준 추천, 근무 중인 앱에서 있습니다 수 자동으로 제안 합니다.
- **Siri 스마트 미리 알림** -사용자 응용 프로그램에서 활동에 대 한 알리는 Siri에 게 할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [앱 검색 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [블로그 게시물 및 샘플](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
