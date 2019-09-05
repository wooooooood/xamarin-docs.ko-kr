---
title: Xamarin.ios의 앱 검색 향상 기능
description: 이 문서에서는 iOS 10의 앱 검색에 대 한 Apple의 향상 된 기능과 Xamarin.ios에서 앱을 구현 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/15/2017
ms.openlocfilehash: 2a2475bcc5eea48584c4aa128aafeeb326e41f8d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70280365"
---
# <a name="app-search-enhancements-in-xamarinios"></a>Xamarin.ios의 앱 검색 향상 기능

_이 문서에서는 iOS 10의 앱 검색에 대 한 Apple의 향상 된 기능과 Xamarin.ios에서 앱을 구현 하는 방법을 설명 합니다._

IOS 10에서 Apple은 라우드 소싱 딥 링크, 앱 내 검색, 검색 연속 및 유효성 검사 결과 시각화와 같은 앱 검색에 대해 몇 가지 향상 된 기능을 만들었습니다. 이 문서에서는 Xamarin.ios 앱에서 이러한 기능을 구현 하는 방법을 다룹니다.

## <a name="about-app-search-enhancements"></a>앱 검색 기능 향상 정보

IOS 10의 핵심 스포트라이트는 다음과 같은 앱 검색에 대 한 몇 가지 향상 된 기능을 제공 합니다.

- **라우드 소싱 딥 링크 인기도 (차등 개인 정보 포함)** -검색 결과에서 딥 링크 된 앱 콘텐츠를 승격 하는 방법을 제공 합니다.
- **앱 내 검색** -새 `CSSearchQuery` 클래스를 사용 하 여 메일, 메시지 및 메모 앱이 작동 하는 방식과 유사한 앱 내 스포트라이트 검색 기능을 제공 합니다.
- **연속 검색** -사용자가 스포트라이트 또는 Safari에서 검색을 시작한 다음 앱을 열고 검색을 계속할 수 있습니다.
- **유효성 검사 결과 시각화** -Apple의 [앱 검색 API 유효성 검사 도구](https://search.developer.apple.com/appsearch-validation-tool) 는 이제 테스트를 미리 구성할 때 웹 사이트의 태그 및 딥 링크를 시각적으로 표시 합니다.
- **메시지 앱 이미지 공유** -메시지에서 공유할 수 있도록 제공 되는 인기 있는 앱 이미지 (메시지 앱 확장을 통해)는 스포트라이트 검색에 표시 됩니다.

다음 섹션에서는 이러한 항목을 자세히 설명 합니다.

## <a name="crowdsourced-deep-link-popularity"></a>라우드 소싱 딥 링크 인기

iOS 10은 앱에 대 한 인기 있는 딥 링크의 빈도를 계산 하는 메커니즘을 제공 하며,이 정보를 사용 하 여 검색 결과에서 앱 콘텐츠의 순위를 개선 하는 동시에 차등을 사용 하 여 사용자의 id를 보호 합니다.  *개인 정보*.

개체를 사용 `NSUserActivity` 하 여 딥 링크 url을 제공 하 고 `EligibleForPublicIndexing` 속성이로 `true`설정 된 앱의 경우 iOS 10은 Apple의 서버에 *차등 개인 정보 취급 방침* 의 하위 집합을 제출 합니다. 이 정보는 검색 결과에서 널리 사용 되는 앱 내 콘텐츠를 승격 하는 데 사용 됩니다.

Xamarin.ios 앱에서 딥 링크를 구현 하는 방법에 대 한 자세한 내용은 [Search With NSUserActivity](~/ios/platform/search/nsuseractivity.md) 설명서를 참조 하세요.

## <a name="in-app-searching"></a>앱 내 검색

새 [Cssearchquery](https://developer.apple.com/reference/corespotlight/cssearchquery) 클래스를 구현 하 여 앱은 사용자가 앱을 떠나지 않고도 (메일, 메시지 및 메모 앱이 작동 하는 방식과 비슷함) 스포트라이트의 검색 및 일치 규칙 기술을 제공 하 여 자체 내에서 콘텐츠를 찾을 수 있습니다.

일반적으로를 지 원하는 `CSSearchQuery` 앱은 별도의 검색 인덱스를 유지 관리할 필요가 없습니다.

## <a name="search-continuation"></a>연속 검색

IOS 9에서 Apple은 검색 api (예: 핵심 스포트라이트 `NSUserActivity` 및 웹 태그)를 도입 하 여 앱 내에서 사용자가 스포트라이트 및 Safari 검색 인터페이스를 모두 사용 하 여 콘텐츠를 검색할 수 있도록 하는 기능을 제공 합니다. 자세한 내용은 [새 검색 api](~/ios/platform/search/index.md) 설명서를 참조 하세요.

IOS 10 Apple은 사용자가 스포트라이트 나 Safari에서 검색을 시작할 수 있도록 허용 하 여이 기능을 기반으로 한 다음 앱을 열 때 검색을 계속 합니다.

이 기능을 구현 하려면 앱의 `Info.plist` 파일을 편집 하 고 **부울** 형식의 `YES` `CoreSpotlightContinuation` 키를 추가 하 고 해당 값을로 설정 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](app-search-enhancements-images/search01.png "Info.plist 파일의 CoreSpotlightContinuation 편집")](app-search-enhancements-images/search01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](app-search-enhancements-images/searchw01.png "Info.plist 파일의 CoreSpotlightContinuation 편집")](app-search-enhancements-images/search01.png#lightbox)

-----

사용자에 게 응답 하 여 검색 결과를 계속`NSUserActivity`하려면 () `AppDelegate.cs` 파일을 편집 하 고 `ContinueUserActivity` 메서드를 재정의 합니다. 예를 들어:

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

이 코드는 쿼리 연속 작업 형식 (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`)을 찾은 다음 `NSUserActivity` 클래스의 사용자 정보 사전 (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`)에서 사용자의 현재 쿼리를 읽습니다. 여기에서 앱은 사용자의 검색을 계속 하기 위해 작업을 수행 해야 합니다.

Xamarin.ios 앱에서 검색을 사용 하는 방법에 대 한 자세한 내용은 [Core 스포트라이트를 사용한 검색](~/ios/platform/search/corespotlight.md) 설명서를 참조 하세요.

## <a name="visualization-of-validation-results"></a>유효성 검사 결과 시각화

이제 Apple의 [앱 검색 API 유효성 검사 도구](https://search.developer.apple.com/appsearch-validation-tool) 는 테스트를 미리 구성할 때 웹 사이트의 태그 및 딥 링크 ( [Schema.org](http://schema.org/)에 정의 된 태그 포함)의 시각적 표시를 표시 합니다.

개발자는 유효성 검사 도구를 사용 하 여 제목, 설명, URL 및 지원 되는 기타 모든 요소와 같은 사이트에 대해 인덱싱된 Ebot 웹 크롤러가 인덱싱된 정보를 볼 수 있습니다.

웹 마크업으로 작업 하는 방법에 대 한 자세한 내용은 [웹 태그를 사용 하 여 검색](~/ios/platform/search/web-markup.md) 설명서를 참조 하세요.

## <a name="message-app-image-sharing"></a>메시지 앱 이미지 공유

메시지 앱 확장에서 메시지 공유를 위한 이미지를 제공 하는 경우 사용자가 앱을 떠나지 않고도 메시지 내에서 인기 있는 이미지에 대 한 스포트라이트 검색을 수행할 수 있도록 확장을 구성할 수 있습니다.

이 기능을 사용 하도록 설정 하려면 다음을 수행 합니다.

1. 메시지 앱 확장을 만듭니다.
2. 를 `com.apple.developer.associated-domains` 앱의 자격에 추가 하 고 메시지 앱 확장이 공유 하는 이미지를 호스트 하는 웹 도메인 목록을 포함 합니다. 각 도메인에 대해 `spotlight-image-search` 서비스를 지정 합니다.
3. 이미지를 `apple-app-site-association` 호스트 하는 웹 사이트에 파일을 추가 합니다. 이 파일은 `spotlight-image-search` 서비스에 대 한 사전을 포함 하 고 앱 id (팀 id 또는 앱 id 접두사, 번들 id)를 포함 합니다. 이 파일에는 스포트라이트로 인덱싱되는 인기 있는 이미지 검색에 포함 되는 최대 500의 경로 및 패턴이 포함 될 수 있습니다. 자세한 내용은 Apple의 [연결 파일 만들기 및 업로드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4) 설명서를 참조 하세요.
4. 웹 사이트를 탐색 하는 데 사용할 수 있습니다. Apple의 [사과 Ebot](https://support.apple.com/HT204683) 설명서를 참조 하세요.

자세한 내용은 [메시지 앱 통합](~/ios/platform/message-app-integration/index.md) 설명서를 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 iOS 10의 앱 검색에 대 한 Apple의 향상 된 기능과 Xamarin.ios에서 앱을 구현 하는 방법에 대해 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
