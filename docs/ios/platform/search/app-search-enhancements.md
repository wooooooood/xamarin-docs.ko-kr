---
title: "앱 검색의 향상 된 기능"
description: "이 문서에서는 향상 된 Apple iOS 10 및 Xamarin.iOS에 구현 하는 방법에 응용 프로그램 검색 했습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: af124c2ae0390c5321e9dd34158c7b53b33b2c48
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="app-search-enhancements"></a>앱 검색의 향상 된 기능

_이 문서에서는 향상 된 Apple iOS 10 및 Xamarin.iOS에 구현 하는 방법에 응용 프로그램 검색 했습니다._

10 ios에서는 Apple 몇 가지 향상 되었습니다 Crowdsourced 딥 링크, 앱에서 검색, 검색 연속 및 유효성 검사 결과 시각화 같은 응용 프로그램 검색 합니다. 이 문서에서는 Xamarin.iOS 앱에서 이러한 기능을 구현 설명 합니다.

## <a name="about-app-search-enhancements"></a>앱 검색의 향상 된 기능에 대 한

IOS 10에서에서 핵심 스포트라이트 여러 개선 된 기능 앱 검색와 같은 제공:

- **(차등 개인 정보)를 Crowdsourced 딥 링크 인기도** -검색 결과의 앱 딥 링크 된 콘텐츠를 승격 하는 방법을 제공 합니다.
- **앱에서 검색** -새로운 `CSSearchQuery` 메일, 메시지 및 메모 앱 작동 방법을 비슷합니다 앱에서 스포트라이트 검색 기능을 제공 하는 클래스입니다.
- **연속 검색** -스포트라이트 또는 Safari, 검색을 시작한 다음 앱을 열 수 있도록 허용 하 고 해당 검색을 계속 합니다.
- **유효성 검사 결과의 시각화** -Apple의 [앱 검색 API 유효성 검사 도구](https://search.developer.apple.com/appsearch-validation-tool) preforming 테스트 하는 경우 이제 웹 사이트의 태그와 딥 링크 시각적 표시 됩니다.
- **응용 프로그램 이미지 공유 메시지** -스포트라이트 검색에서 표시할 메시지 (메시지 응용 프로그램 확장)를 통해 공유 하는 데 제공 하는 인기 있는 앱에서 이미지를 허용 합니다.

다음 섹션에서는 다음이 항목에서 자세히 설명 합니다.

## <a name="crowdsourced-deep-link-popularity"></a>인기도 Crowdsourced 딥 링크

사용자가을 앱에 인기 있는 딥 링크 뒤 및 사용 하 여 사용자의 id를 사용 하 여 보호 하면서 앱의 등급을 개선 하기 위해이 정보 검색 결과에서 콘텐츠의 빈도 계산 하는 메커니즘을 제공 하는 iOS 10  *차등 개인 정보 보호*합니다.

응용 프로그램을 사용 하는에 대 한 `NSUserActivity` 딥 링크 Url을 제공 하 고 개체는 `EligibleForPublicIndexing` 속성이로 설정 `true`, iOS 10의 하위 집합을 제출 *차등 개인 정보 보호 해시* Apple의 서버에 있습니다. 이 정보는 검색 결과에 인기 앱에서 콘텐츠를 승격 하려면 다음 사용 됩니다.

Xamarin.iOS 앱의 딥 링크의 구현에 대 한 자세한 내용은 참조 하십시오 우리의 [NSUserActivity 사용 하 여 검색](~/ios/platform/search/nsuseractivity.md) 설명서입니다.

## <a name="in-app-searching"></a>앱에서 검색

새 구현 하 여 [CSSearchQuery](https://developer.apple.com/reference/corespotlight/cssearchquery) 클래스 스포트라이트의 검색 및 일치 하는 규칙 기술을 사용자가 응용 프로그램 (방식과 유사한 메일, 메시지 및 메모 앱을 자체 내에서 콘텐츠를 찾으려면 앱 제공할 수 있습니다 작업)입니다.

일반적으로 응용 프로그램을 지 원하는 `CSSearchQuery` 자체적으로 별도 검색 인덱스를 유지 관리할 필요가 없습니다. 

## <a name="search-continuation"></a>검색 연속

IOS 9, 사과 검색 Api를 도입 했습니다. (코어 스포트라이트 같은 `NSUserActivity` 및 웹 태그) 딥-원하는 대로 사용자 Safari와 스포트라이트 검색 인터페이스를 사용 하 여 해당 콘텐츠를 검색할 수 있도록 앱 내에서 콘텐츠를 제공 하기. 참조 우리의 [새로운 검색 Api](~/ios/platform/search/index.md) 자세한 세부 정보에 대 한 설명서입니다.

IOS에서 10 Apple 기반으로이 기능은 사용자 스포트라이트 또는 Safari, 검색을 시작 하 고 앱을 열 때 검색을 계속 합니다. 

이 기능을 구현 하려면 응용 프로그램의 편집 `Info.plist` 파일에서 추가 된 `CoreSpotlightContinuation` 형식의 키 **부울** 으로 값을 설정 하 고 `YES`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](app-search-enhancements-images/search01.png "CoreSpotlightContinuation Info.plist 파일에서 편집")](app-search-enhancements-images/search01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](app-search-enhancements-images/searchw01.png "CoreSpotlightContinuation Info.plist 파일에서 편집")](app-search-enhancements-images/search01.png#lightbox)

-----

검색 결과 계속 사용자에 게 응답 하도록 (`NSUserActivity`), 편집는 `AppDelegate.cs` 파일을 재정의 하는 `ContinueUserActivity` 메서드. 예:

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

이 코드에서 찾는 쿼리 연속 작업 동작 유형 (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`), 그런 다음에서 사용자의 현재 쿼리를 읽어는 `NSUserActivity` 클래스의 사용자 정보 사전 (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`). 여기에서 응용 프로그램 사용자의 검색을 계속 하려면는 작업을 수행 해야 합니다.

Xamarin.iOS 앱에서의 검색을 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [스포트라이트 코어를 사용 하 여 검색](~/ios/platform/search/corespotlight.md) 설명서입니다.

## <a name="visualization-of-validation-results"></a>유효성 검사 결과의 시각화

Apple의 [앱 검색 API 유효성 검사 도구](https://search.developer.apple.com/appsearch-validation-tool) 이제 웹 사이트의 태그 및 딥 링크의 시각적 표현이 표시 됩니다 (마크업에서와 같은 정의 포함 하 여 [Schema.org](http://schema.org/)) preforming 테스트 하는 경우.

유효성 검사 도구를 사용 하 여 개발자 Applebot 웹 크롤러 제목, 설명, URL 및 다른 모든 같은 사이트에 대 한 인덱싱된 포함 정보 지원 요소 볼 수 있습니다.

웹 태그를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [웹 태그로 검색](~/ios/platform/search/web-markup.md) 설명서입니다.

## <a name="message-app-image-sharing"></a>공유 응용 프로그램 이미지가 메시지

메시지 앱 확장에서는 메시지에 공유에 대 한 이미지를 제공 하는 경우 사용자 응용 프로그램을 여러 메시지를 내에서 인기 있는 이미지에 대 한 스포트라이트 검색을 수행할 수 있도록 확장을 구성할 수 있습니다.

이 기능을 사용 하려면 다음을 수행 합니다.

1. 메시지 앱 확장을 만듭니다.
2. 추가 된 `com.apple.developer.associated-domains` 응용 프로그램의 자격을 메시지 앱 확장을 공유 하는 이미지를 호스팅하는 웹 도메인의 목록을 포함 합니다. 각 도메인에 대해 지정 된 `spotlight-image-search` 서비스입니다.
3. 추가 `apple-app-site-association` 이미지를 호스팅하는 웹 사이트에는 파일입니다. 이 파일에 대 한 사전에 포함 되어는 `spotlight-image-search` 서비스 하 고 팀 ID 또는 앱 ID 접두사 뒤에 번들 id. 응용 프로그램의 ID를 포함 합니다. 파일 경로 스포트라이트에서 인덱싱되고 인기 있는 이미지 검색에 포함 되는 패턴 최대 500 개를 포함할 수 있습니다. 자세한 내용은 Apple의를 참조 하십시오 [만들기 및 연결 파일을 업로드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4) 설명서입니다.
4. 웹 사이트를 크롤링할 Applebot 허용 합니다. Apple의를 참조 하십시오 [에 대 한 Applebot](https://support.apple.com/en-us/HT204683) 설명서입니다.

참조 우리의 [메시지 응용 프로그램 통합](~/ios/platform/message-app-integration/index.md) 자세한 세부 정보에 대 한 설명서입니다.

## <a name="summary"></a>요약

이 문서는 향상 된 기능 검사가 수행 Apple iOS 10 및 Xamarin.iOS에 구현 하는 방법에 응용 프로그램 검색 했습니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
