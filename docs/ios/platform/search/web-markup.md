---
title: Xamarin.iOS에서 웹 태그로 검색
description: 이 문서에서는 Xamarin.iOS 앱에 다시 연결 하는 웹 기반 검색 결과 만드는 방법을 설명 합니다. 인덱싱, 검색 가능, 유니버설 링크를 스마트 앱 내 배너 등을 사용 하 여 앱의 웹 사이트를 만드는 웹 콘텐츠를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 4ee07e4b47ed9e1bdca0efc814ad44e513f68e80
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672367"
---
# <a name="search-with-web-markup-in-xamarinios"></a>Xamarin.iOS에서 웹 태그로 검색

웹 사이트를 통해 해당 콘텐츠에 대 한 액세스를 제공 하는 앱에 대 한 (에서 뿐만 아니라 앱 내에서), Apple에서 크롤링할 수는 사용자의 iOS 9 장치에 앱 딥 링크를 제공 하는 특별 한 링크를 사용 하 여 웹 콘텐츠 표시할 수 있습니다.

IOS 앱에서 이미 모바일 딥 링크 설정 지원 및 웹 사이트에는 딥 링크 앱 내에서 Apple의 콘텐츠를 표시 하는 경우 _Applebot_ 웹 크롤러가이 콘텐츠 인덱스 및 해당 클라우드 인덱스를 자동으로 추가 됩니다.

[![](web-markup-images/webmarkup01.png "클라우드 인덱스 개요")](web-markup-images/webmarkup01.png#lightbox)

Apple는 스포트라이트 검색 및 Safari 검색 결과에서 이러한 결과 노출 됩니다.
사용자가에 탭 하는 경우 결과 중 (있고 앱을 설치) 앱의 콘텐츠를 수행 합니다.

[![](web-markup-images/webmarkup02.png "심층 검색 결과에서 웹 사이트에서 연결 합니다.")](web-markup-images/webmarkup02.png#lightbox)

## <a name="enabling-web-content-indexing"></a>웹 콘텐츠 인덱싱 사용

가능 하면 앱의 콘텐츠 웹 태그를 사용 하 여 하는 데 필요한 네 가지 단계가 있습니다.

1. Apple 검색 하 고 앱의 웹 사이트를 정의 하 여 인덱스 수를 확인 합니다 **지원** 하거나 **마케팅** iTunes Connect에서에서 웹 사이트입니다.
2. 앱의 웹 사이트에 모바일 딥 링크 구현에 필요한 태그가 포함 되어 있는지 확인 합니다. 자세한 내용은 아래 섹션을 참조 하세요.
3. IOS 앱에서 처리 하는 딥 링크를 사용 하도록 설정 합니다.
4. 최종 사용자에 게는 풍부 하 고 유용한 결과 제공 하는 앱의 웹 사이트에 표시 하는 구조화 된 데이터에 대 한 태그를 추가 합니다. 이 단계에 꼭 필요 없을 때 Apple에서는 것이 좋습니다.

다음 섹션에서는 이러한 단계를 자세히 살펴봅니다.

## <a name="make-your-apps-website-discoverable"></a>앱의 웹 사이트를 검색 가능

Apple 앱의 웹 사이트를 찾을 필요가 쉬운 것으로 사용 합니다 **지원** 또는 **마케팅** iTunes Connect 통해 Apple에 앱을 제출할 때 웹 사이트입니다.

## <a name="using-smart-app-banners"></a>스마트 앱 내 배너를 사용 하 여

앱으로의 선택을 취소 링크를 제공 하 여 웹 사이트에는 스마트 앱 배너를 제공 합니다. 앱은 이미 설치 되어 있지 않으면 Safari에 자동으로 사용자에 게 앱을 설치할 묻습니다. 사용 하 여 탭 수이 고, 그렇지 합니다 **보기** 웹 사이트에서 앱을 시작 하는 링크입니다. 예를 들어 스마트 앱 배너를 만들려면 다음 코드를 사용할 수 있습니다.

```xml
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

자세한 내용은 Apple의를 참조 하세요 [스마트 앱 내 배너를 사용 하 여 앱 승격](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html) 설명서.

## <a name="using-universal-links"></a>유니버설 링크를 사용 하 여

새 ios 9, 유니버설 링크는 다음을 제공 하 여 스마트 앱 내 배너에 기존 사용자 지정 URL 구성표 더 나은 대안을 제공 합니다.

- **고유한** – 둘 이상의 웹 사이트에서 동일한 URL을 요청할 수 없습니다.
- **보안** –에 연결 된 웹 사이트를 소유 하 고 유효 하 게 보장 하는 웹 사이트에 대 한 서명 된 인증서가 필요 합니다 앱.
- **유연한** – 최종 사용자 웹 사이트 또는 앱 URL을 시작 하는지 여부를 제어할 수 있습니다.
- **유니버설** – 웹 사이트와 앱의 콘텐츠를 정의 하려면 동일한 URL을 사용할 수 있습니다.

## <a name="using-twitter-cards"></a>Twitter 카드를 사용 하 여

딥 링크를 Twitter 카드를 사용 하 여 앱의 콘텐츠를 제공할 수 있습니다. 예를 들어:

```xml
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

자세한 내용은 Twitter의를 참조 하세요 [Twitter 카드 프로토콜](http://dev.twitter.com/cards/mobile) 설명서.

## <a name="using-facebook-app-links"></a>Facebook 앱 링크를 사용 하 여

딥 링크 Facebook 앱 링크를 사용 하 여 앱의 콘텐츠를 제공할 수 있습니다. 예를 들어:

```xml
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

자세한 내용은 Facebook의를 참조 하세요 [앱 링크](http://applinks.org) 설명서.

## <a name="opening-deep-links"></a>딥 링크 열기

열기 및 Xamarin.iOS 앱의 딥 링크를 표시에 대 한 지원을 추가 해야 합니다. 편집 된 **AppDelegate.cs** 파일을 재정의 `OpenURL` 사용자 지정 URL 형식을 처리 하는 방법입니다. 예를 들어:

```csharp
public override bool OpenUrl (UIApplication application, NSUrl url, string sourceApplication, NSObject annotation)
{

    // Handling a URL in the form http://company.com/appname/?123
    try {
        var components = new NSUrlComponents(url,true);
        var path = components.Path;
        var query = components.Query;

        // Is this a known format?
        if (path == "/appname") {
            // Display the view controller for the content
            // specified in query (123)
            return ContentViewController.LoadContent(query);
        }
    } catch {
        // Ignore issue for now
    }

    return false;
}
```

위의 코드에서 우리가 찾고자 하는 포함 하는 URL `/appname` 의 값을 전달 하 고 `query` (`123` 이 예제의) 요청 된 콘텐츠를 사용자에 게 표시할 앱의 사용자 지정 뷰 컨트롤러에 있습니다.

## <a name="providing-rich-results-with-structured-data"></a>구조화 된 데이터를 사용 하 여 다양 한 결과 제공합니다.

구조화 된 데이터 태그를 포함 하 여 제목과 설명을 간단히 보다 우수한 최종 사용자에 게 다양 한 검색 결과 제공할 수 있습니다. 이미지, 앱 (예: 등급) 특정 데이터 및 구조화 된 데이터 태그를 사용 하 여 결과를 작업을 포함 합니다.

다양 한 결과가 더 많은 참여 되 고 향상 시킬 수 있습니다 프로그램 순위 클라우드에서 더 많은 사용자 상호 작용을 유인 하 여 검색 인덱스를 기반 합니다.

구조화 된 데이터 태그를 제공 하는 한 가지 옵션을 오픈 그래프를 사용 합니다. 예를 들어:

```xml
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

자세한 내용은 참조 하십시오 합니다 [오픈 그래프](http://ogp.me) 웹 사이트입니다.

구조화 된 데이터 표시를 위한 다른 일반적인 형식은 schema.org의 애프터 형식이입니다. 예를 들어:

```xml
<div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
    <span itemprop="ratingValue">4** stars -
    <span itemprop="reviewCount">255** reviews


```

Schema.org의 LD JSON 형식으로 동일한 정보를 나타낼 수 있습니다.

```xml
<script type="application/ld+json">
    "@content":"http://schema.org",
    "@type":"AggregateRating",
    "ratingValue":"4",
    "reviewCount":"255"
</script>
```

다음 최종 사용자에 게 풍부한 검색 결과 제공 하 여 웹 사이트에서 메타 데이터의 예를 보여 줍니다.

[![](web-markup-images/deeplink01.png "서식 있는 구조화 된 데이터 태그를 통해 결과 검색합니다.")](web-markup-images/deeplink01.png#lightbox)

Apple에는 현재 schema.org에서 다음과 같은 스키마 형식을 지원합니다.

 - AggregateRating
 - ImageObject
 - InteractionCount
 - 제품
 - 조직
 - PriceRange
 - 레시피
 - SearchAction

이러한 스키마 유형에 대 한 자세한 내용은 참조 하십시오 [schema.org](http://schema.org)합니다.

## <a name="providing-actions-with-structured-data"></a>구조화 된 데이터를 사용 하 여 작업을 제공합니다.

특정 유형의 구조화 된 데이터를 사용 하면 검색 결과 최종 사용자가 가능 해야 합니다. 현재 다음 작업이 지원 됩니다.

 - 전화 번호를 전화를 걸고 있습니다.
 - 지정 된 주소의 지도 방향을 가져옵니다.
 - 오디오 또는 비디오 파일을 재생 합니다.

예를 들어, 다음과 같은 전화 번호로 전화를 하는 작업 정의 표시 될 수 있습니다.

```xml
<div itemscope itemtype="http://schema.org/Organization">
    <span itemprop="telephone">(408) 555-1212**


```

이 검색 결과 최종 사용자에 게 표시 되 면 작은 전화 아이콘을 결과에 표시 됩니다. 사용자 아이콘을 탭 하는 경우 지정 된 번호로 호출 됩니다.

다음 HTML 검색 결과에서 오디오 파일을 재생 하는 작업을 추가 합니다.

```xml
<div itemscope itemtype="http://schema.org/AudioObject">
    <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**


```

마지막으로 다음 HTML 검색 결과에서 지침을 가져오는 작업을 추가 합니다.

```xml
<div itemscope itemtype="http://schema.org/PostalAddress">
    <span itemprop="streetAddress">1 Infinite Loop**
    <span itemprop="addressLocality">Cupertino**
    <span itemprop="addressRegion">CA**
    <span itemprop="postalCode">95014**


```

자세한 내용은 Apple의를 참조 하세요 [앱 검색 개발자 사이트](https://developer.apple.com/ios/search/)합니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [앱 검색 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
