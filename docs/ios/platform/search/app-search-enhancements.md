---
title: Xamarin.iOS에서 앱 검색 기능 향상
description: 이 문서에서는 향상 된 Apple iOS 10과 Xamarin.iOS에서 구현 하는 방법에서 앱 검색 했습니다.
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: e61cb20f12f6373ef57beb759b933d3dd9b6e76e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61408137"
---
# <a name="app-search-enhancements-in-xamarinios"></a>Xamarin.iOS에서 앱 검색 기능 향상

_이 문서에서는 향상 된 Apple iOS 10과 Xamarin.iOS에서 구현 하는 방법에서 앱 검색 했습니다._

Ios 10에서 Apple 라우드 딥 링크, 앱에서 검색, 검색 연속 및 유효성 검사 결과의 시각화와 같은 앱 검색에 몇 가지 향상을 했습니다. 이 문서에서는 Xamarin.iOS 앱에서 이러한 기능을 구현 다룰 것입니다.

## <a name="about-app-search-enhancements"></a>앱 검색 기능 향상에 대 한

IOS 10에서에서 핵심 스포트라이트를 같은 앱 검색에 몇 가지 향상 된 기능 제공:

- **(사용 하 여 차등 개인) 라우드 딥 링크 인기도** -검색 결과에서 앱 딥 링크 된 콘텐츠를 홍보 하는 방법을 제공 합니다.
- **앱에서 검색** -새 `CSSearchQuery` 클래스를 설명 하는 메일, 메시지 및 메모 앱의 작동 방식을 유사한 앱 스포트라이트 검색 기능을 제공 합니다.
- **연속 검색** -추천 또는 Safari에서 검색을 시작 하 고 앱을 열 수 있도록 하 고 검색을 계속 합니다.
- **유효성 검사 결과의 시각화** -Apple [앱 검색 API 유효성 검사 도구](https://search.developer.apple.com/appsearch-validation-tool) preforming 테스트 하는 경우 이제 웹 사이트의 태그 및 딥 링크에 대 한 시각적 표현을 표시 됩니다.
- **앱 이미지 공유 메시지** -스포트라이트 검색에서 표시할 메시지 (메시지 앱 확장을)를 통해 공유에 대해 제공 된 인기 있는 앱에서 이미지를 허용 합니다.

다음 섹션에서는 이러한 항목을 더 자세히 다룰 것입니다.

## <a name="crowdsourced-deep-link-popularity"></a>널리 사용 되 고 라우드 딥 링크

앱에 인기 있는 딥 링크 뒤에 사용자를 사용 하 여 사용 하 여 사용자의 id를 보호 하면서 앱의 순위를 개선 하기 위해이 정보 검색 결과에서 콘텐츠의 빈도 계산 하는 메커니즘을 제공 하는 iOS 10  *차등 개인*합니다.

앱의 사용 하는 `NSUserActivity` 딥 링크 Url을 제공 하 고 있는 개체를 `EligibleForPublicIndexing` 속성으로 설정 `true`의 하위 집합을 제출 하는 iOS 10 *차등 개인 해시* Apple 서버. 이 정보는 검색 결과에서 인기 있는 앱에서 콘텐츠를 승격 하려면 다음 사용 됩니다.

Xamarin.iOS 앱의 딥 링크의 구현에 대 한 자세한 내용은 참조 하십시오 우리의 [NSUserActivity 사용 하 여 검색](~/ios/platform/search/nsuseractivity.md) 설명서.

## <a name="in-app-searching"></a>앱 검색

새 구현한 [CSSearchQuery](https://developer.apple.com/reference/corespotlight/cssearchquery) 클래스 추천의 검색과 일치 하는 규칙 기술 사용자에 게는 앱 (방법과 유사 메일, 메시지 및 메모를 나 가지 않고 자체 내에서 콘텐츠를 찾기 위해 앱을 제공할 수 있습니다 작업)입니다.

일반적으로 앱을 지 `CSSearchQuery` 자체적으로 별도 검색 인덱스를 유지 관리할 필요가 없습니다. 

## <a name="search-continuation"></a>검색 연속

IOS 9, Apple 검색 Api를 도입 했습니다. (같은 핵심 스포트라이트 `NSUserActivity` 및 웹 태그) 심층-원하는 대로 사용자가 추천 및 Safari 검색 인터페이스를 사용 하 여 해당 콘텐츠를 검색할 수 있도록 앱 내에서 콘텐츠를 제공 합니다. 참조 우리의 [새 검색 Api](~/ios/platform/search/index.md) 자세한 세부 정보에 대 한 설명서입니다.

IOS 10 Apple이이 기능으로 사용자가 추천 또는 Safari에서 검색을 시작 하 고 앱을 열 때 검색을 계속할 수 있도록 하 여 작성 합니다. 

이 기능을 구현 하려면 앱의 편집 `Info.plist` 파일을 추가 합니다 `CoreSpotlightContinuation` 형식의 키 **부울** 해당 값을 설정 하 고 `YES`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](app-search-enhancements-images/search01.png "CoreSpotlightContinuation Info.plist 파일 편집")](app-search-enhancements-images/search01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](app-search-enhancements-images/searchw01.png "CoreSpotlightContinuation Info.plist 파일 편집")](app-search-enhancements-images/search01.png#lightbox)

-----

검색 결과 계속 사용자에 게 응답할 (`NSUserActivity`)를 편집 합니다 `AppDelegate.cs` 파일을 재정의 `ContinueUserActivity` 메서드. 예를 들어:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchQuery.ContinuationActionType) {
            var search = userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString);
            // Continue user's search here...
        }
        break;
    }

    return true;
}
```

이 코드는 쿼리 연속 작업 유형을 찾습니다 (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`)를 다음에서 사용자의 현재 쿼리를 읽습니다 합니다 `NSUserActivity` 클래스의 사용자 정보 사전 (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`). 여기에서 앱 사용자의 검색을 계속 하려면 작업을 수행 해야 합니다.

Xamarin.iOS 앱에 검색을 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [핵심 스포트라이트를 사용 하 여 검색](~/ios/platform/search/corespotlight.md) 설명서.

## <a name="visualization-of-validation-results"></a>유효성 검사 결과 시각화

Apple [앱 검색 API 유효성 검사 도구](https://search.developer.apple.com/appsearch-validation-tool) 이제 웹 사이트의 태그 및 딥 링크에 대 한 시각적 표현을 표시 됩니다 (태그에서 같은 정의 포함 하 여 [Schema.org](http://schema.org/)) preforming 테스트 하는 경우.

유효성 검사 도구를 사용 하면 개발자는 Applebot 웹 크롤러 제목, 설명, URL 및 기타와 같은 사이트에 대 한 인덱스에 정보를 지원 요소를 볼 수 있습니다.

웹 태그를 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [웹 태그로 검색](~/ios/platform/search/web-markup.md) 설명서.

## <a name="message-app-image-sharing"></a>공유 하는 메시지 앱 이미지

메시지의 공유에 대 한 이미지를 제공 하는 메시지 앱 확장을, 하는 경우 사용자는 앱을 종료 하지 않고도 메시지 내에서 인기 있는 이미지에 대 한 스포트라이트 검색을 수행할 수 있도록 확장을 구성할 수 있습니다.

이 기능을 사용 하려면 다음을 수행 합니다.

1. 메시지 앱 확장을 만듭니다.
2. 추가 된 `com.apple.developer.associated-domains` 앱의 자격에 메시지 앱 확장을 공유 하는 이미지를 호스팅하는 웹 도메인 목록을 포함 합니다. 각 도메인에 대해 지정 된 `spotlight-image-search` 서비스입니다.
3. 추가 된 `apple-app-site-association` 이미지를 호스팅하는 웹 사이트에는 파일입니다. 이 파일에 대 한 사전이 포함 된 `spotlight-image-search` 서비스 및 앱의 ID입니다. 번들 id. 뒤에 팀 ID 또는 앱 ID 접두사를 포함 합니다. 파일 경로 및 패턴 스포트라이트로 인덱싱되고 인기 있는 이미지 검색에 포함 되는 최대 500 개를 포함할 수 있습니다. 자세한 내용은 Apple의를 참조 하세요 [만들기 및 연결 파일을 업로드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4) 설명서.
4. 웹 사이트를 크롤링할 Applebot를 허용 합니다. Apple의를 참조 하세요 [에 대 한 Applebot](https://support.apple.com/HT204683) 설명서.

참조 우리의 [메시지 앱 통합](~/ios/platform/message-app-integration/index.md) 자세한 세부 정보에 대 한 설명서입니다.

## <a name="summary"></a>요약

이 문서에서는 이러한 향상 된 부분이 Apple iOS 10과 Xamarin.iOS에서 구현 하는 방법에서 앱 검색 했습니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
