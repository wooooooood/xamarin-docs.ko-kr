---
title: Xamarin.iOS에서 NSUserActivity 사용 하 여 검색
description: 이 문서를 추천 및 Safari에서 검색할 수 있도록 하는 NSUserActivity 인덱싱하는 방법을 설명 합니다. 검색 결과에 NSUserActivity의 선택에 응답 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: acfff90b4b983f92718bb9af1f587a73ec0f8da7
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104260"
---
# <a name="search-with-nsuseractivity-in-xamarinios"></a>Xamarin.iOS에서 NSUserActivity 사용 하 여 검색

`NSUserActivity` iOS 8에서에서 도입 되었으며 핸드 오프에 대 한 데이터를 제공 하는 데 사용 됩니다.
다른 iOS 장치에서 실행 중인 앱의 다른 인스턴스로 전달할 수 있습니다 하는 앱의 특정 부분에 활동을 만들 수 있습니다. 수신 장치에 계속 사용자 중단을 바로 선택 이전 장치에서 시작 작업 수 있습니다. 핸드 오프를 사용 하 여에 대 한 자세한 내용은 참조 하십시오 우리의 [핸드 오프 소개](~/ios/platform/handoff.md) 설명서.

IOS 9 접하는 `NSUserActivity` (공개적으로 및 개인적으로) 인덱싱된 및 스포트라이트 검색 및 Safari에서 검색할 수 있습니다. 표시는 `NSUserActivity` 검색 및 추가 인덱싱 가능한 메타 데이터로 작업 iOS 장치에서 검색 결과에 나열 수 있습니다.

[![](nsuseractivity-images/apphistory01.png "앱 기록 개요")](nsuseractivity-images/apphistory01.png#lightbox)

사용자가 앱에서 작업에 속하는 검색 결과 선택 하는 경우 앱을 시작할 수는 및에 설명 된 작업을 `NSUserActivity` 다시 시작 하 고 사용자에 게 표시 됩니다.

다음 속성을 `NSUserActivity` 앱 검색을 지 원하는 데 사용 됩니다.

 - `EligibleForHandoff` - `true`를 핸드 오프 작업에서이 작업을 사용할 수 있습니다.
 - `EligibleForSearch` - `true`,이 작업을 장치에서 인덱스에 추가 하 고 검색 결과에 표시 합니다.
 - `EligibleForPublicIndexing` - `true`,이 작업을 Apple의 클라우드 기반 인덱스를 추가 하 고 iOS 장치에서 앱을 이미 설치 되지 않은 (검색)를 통해 사용자에 게 표시 합니다. 참조 된 [공용 검색 인덱싱](#Public-Search-Indexing) 대 한 자세한 내용은 아래 섹션입니다.
 - `Title` – 작업에 대 한 제목을 제공 하 고 검색 결과에 표시 됩니다. 사용자 자체 제목 텍스트를 검색할 수도 있습니다.
 - `Keywords` -인덱싱된 되며 최종 사용자가 검색을 수행 하는 작업에 설명 하는 데 사용 하는 문자열의 배열이입니다.
 - `ContentAttributeSet` -입니다는 `CSSearchableItemAttributeSet` 추가 세부 정보에서 작업을 설명 하 고 검색 결과에 서식 있는 콘텐츠를 제공 하는 데 사용 합니다.
 - `ExpirationDate` –만 지정 된 날짜로 표시 됩니다 하기 위한 작업을 하려는 경우에 해당 날짜를 제공할 수 있습니다.
 - `WebpageURL` – 웹에서 활동을 볼 수 있습니다 하거나 앱 지 원하는 경우 Safari의 딥 링크를 보려면 이곳을 방문에 대 한 링크를 설정할 수 있습니다.

## <a name="nsuseractivity-quickstart"></a>NSUserActivity 빠른 시작

다음이 지침을 검색할 수 있는 구현에 따라 `NSUserActivity` 앱에서:

- [활동 유형 식별자 만들기](#creatingtypeid)
- [활동 만들기](#createactivity)
- [작업에 응답](#respondactivity)
- [공용 검색 인덱싱](#indexing)

몇 가지 [추가적인 혜택](#benefits) 사용 하 여 `NSUserActivity` 콘텐츠를 검색할 수 있도록 합니다.

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>활동 유형 식별자 만들기

만들려는 해야 검색 작업을 만들 수 있습니다, 전에 _활동 유형 식별자_ 식별 하기 위해. 활동 형식 식별자가 짧은 문자열에 추가 합니다 `NSUserActivityTypes` 앱의 배열을 **Info.plist** 지정된 된 사용자 활동 형식에 고유 하 게 식별 하는 데 사용 되는 파일입니다. 앱을 지원 하 고 앱 검색에 노출 하는 각 작업에 대 한 배열에 하나의 항목이 됩니다. 

Apple에서 충돌을 방지 하려면 활동 형식 식별자에 대 한 역방향 DNS 스타일 표기법을 사용 하 여 제안 합니다. 예를 들어: `com.company-name.appname.activity` 특정 앱에 대 한 작업 기반 또는 `com.company-name.activity` 여러 앱에서 실행할 수 있는 작업에 대 한 합니다.

활동 유형 식별자를 만들 때 사용 되는 `NSUserActivity` 활동의 type을 식별 하는 인스턴스. 활동 검색 결과 눌러 사용자의 결과로 계속 되 면 활동 유형 (앱의 팀 ID)와 함께 작업을 계속 시작 하는 앱을 결정 합니다.

편집이 동작을 지 원하는 데 필요한 활동 형식 식별자를 만들려면 합니다 **Info.plist** 파일을 전환할 합니다 **원본** 보기. 추가 된 `NSUserActivityTypes` 형식은 식별자를 만들고 키:

[![](nsuseractivity-images/type01.png "Plist 편집기에서 필요한 식별자 고 NSUserActivityTypes 키")](nsuseractivity-images/type01.png#lightbox)

위의 예제에서는 검색 작업에 대 한 새로운 활동 유형 식별자를 하나 만들었습니다 (`com.xamarin.platform`). 자신의 앱을 만들 때의 내용을 바꿉니다는 `NSUserActivityTypes` 앱 특정 활동에 활동 형식 식별자를 사용 하 여 배열을 지원 합니다.

<a name="createactivity" />

## <a name="creating-an-activity"></a>활동 만들기

다음은 호스트 하는 장치에서 인덱스 검색을 위한 작업을 만드는 예제입니다.

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

추가할 수 있었습니다 추가 세부 정보를 설정 하 여는 `ContentAttributeSet` 속성을이 `NSUserActivity` 다음과 같습니다:

[![](nsuseractivity-images/apphistory02.png "추가 세부 정보 검색 개요")](nsuseractivity-images/apphistory02.png#lightbox)

사용 하 여를 `ContentAttributeSet` 최종 사용자가 상호 작용을 유도 하는 다양 한 검색 결과 만들 수 있습니다.

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>작업에 응답

검색 결과 탭 하거나 사용자에 게 응답할 (`NSUserActivity`)이 앱에 대 한 편집 합니다 **AppDelegate.cs** 파일을 재정의 `ContinueUserActivity` 메서드. 예를 들어:

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

이 전달 요청에 응답 하는 데 동일한 메서드 재정의 note 합니다. 이제 사용자가 Spotlight 검색 결과에서 앱의 링크를 클릭 하면 앱 됩니다 수 전경 (아직 실행 하지 않은 경우 시작) 및 콘텐츠, 탐색 또는 링크를 나타내는 기능 표시 됩니다.

[![](nsuseractivity-images/apphistory03.png "검색에서 이전 상태로 복원")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>공용 검색 인덱싱

위에서 말한 것 처럼 iOS 9 쉽게 앱 콘텐츠 및 사용자가 이미 검색 하 고 지정 된 iOS 장치를 사용할 수 있는 기능에 대 한 검색 액세스를 제공 합니다. 공용 인덱싱을 사용 하 여 iOS 9를 통해 사용자가 아직 검색 콘텐츠 또는 기능이 아직 (또는 앱도 설치 하지 않은 사용자)에 대 한 결과를 얻으려면 이러한 검색에서 너무 합니다.

공용 인덱싱 같은 방식으로 작동합니다.

1. 앱에 대 한 작업을 만들 때에 public로 표시할 수 있습니다.
2. 공용 활동 Apple에 전송 되며 클라우드에서 인덱싱됩니다.
3. 자세한 사용자가 장치에서 앱과 상호 작용 하 여 특정 기능 또는 활동에 의해 표시 되는 콘텐츠를 사용 하 여, 차수에서 증가 합니다.
4. 앱이 설치 되지 않은 경우에 공용 인기 결과 다른 사용자에 게 제공 됩니다.
5. (작업 URL이 포함 됩니다) 하는 경우 스포트라이트 검색 및 Safari에서 이러한 공용 결과에 표시 됩니다.

위에서 만든 개인 검색 작업을 수행 하 고 공용 확장 수 있습니다.

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

설정 하 여 공용 인덱싱에 대 한 활동을 설정한 단지 `EligibleForPublicIndexing = true`에 자동으로 추가 됩니다 Apple의 공용 클라우드 인덱스를 의미 하지는 않습니다. 다음 조건이 처음에 충족 되어야 합니다.

1. 검색 결과에 표시 해야 하 고 많은 사용자가 선택할 수 있습니다. 결과 활동 engagement 임계값에 도달 될 때까지 개인 설정을 유지 합니다.
2. 인덱싱된 공용으로 설정 되 고 사용자 고유의 데이터를 방지 앱 프로 비전 합니다.

<a name="benefits" />

## <a name="additional-benefits"></a>추가 혜택

앱 검색을 통해 채택 하 여 `NSUserActivity` 앱에서 볼 수도 다음과 같은 기능:

- **핸드 오프** -앱 검색 콘텐츠를 탐색을 공개 하는 및/또는 핸드 오프로 동일한 메커니즘을 사용 하 여 기능 때문에 (`NSUserActivity`), 하나의 장치에서 작업을 시작 하 고 다른 계속 하려면 앱의 사용자를 쉽게 허용할 수 있습니다.
- **Siri 제안** -Siri 제안 일반적으로 표준 추천 단어와 함께 앱에서 근무 중인 직원 수 자동으로 제안 합니다.
- **Siri 스마트 미리 알림** -사용자가 앱에서 활동에 대 한 알리는 Siri를 요청할 수 있게 됩니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [앱 검색 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [블로그 게시물 및 샘플](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
