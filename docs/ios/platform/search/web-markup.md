---
title: Xamarin.iOS에 웹 태그를 사용 하 여 검색
description: 이 문서에서는 Xamarin.iOS 앱으로 다시 연결 되는 웹 기반 검색 결과 생성 하는 방법에 설명 합니다. 인덱싱, 스마트 앱 배너, 범용 링크 등을 사용 하 여 검색 가능한 응용 프로그램의 웹 사이트를 만드는 웹 콘텐츠를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 438a65de3eb78f849493e3478bce5522a325d0cd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787996"
---
# <a name="search-with-web-markup-in-xamarinios"></a>Xamarin.iOS에 웹 태그를 사용 하 여 검색

웹 사이트를 통해 해당 콘텐츠에 대 한 액세스를 제공 하는 앱에 대 한 (뿐 아니라 앱 내에서), Apple에서 탐색 하 고 사용자의 iOS 9 장치에 앱에 대 한 직접 링크를 제공 하는 특별 한 링크와 함께 웹 콘텐츠 표시할 수 있습니다.

IOS 앱 이미 모바일 직접 링크를 지원 하며 웹 사이트 딥 링크 앱 내에서 사과 콘텐츠를 표시 하는 경우 _Applebot_ 웹 크롤러가이 콘텐츠 인덱싱 및 클라우드 인덱스에 자동으로 추가 됩니다.

[![](web-markup-images/webmarkup01.png "클라우드 인덱스 개요")](web-markup-images/webmarkup01.png#lightbox)

스포트라이트 검색 및 Safari 검색 결과에서 이러한 결과 Apple 노출할 합니다.
사용자가 누를 경우 결과 다음 중 하나 (수행 되었으며 설치 된 앱) 다음 응용 프로그램의 콘텐츠를 수행 됩니다.

[![](web-markup-images/webmarkup02.png "심층 검색 결과에 웹 사이트에서 연결 합니다.")](web-markup-images/webmarkup02.png#lightbox)

## <a name="enabling-web-content-indexing"></a>웹 콘텐츠 인덱싱 사용

가지 하면 응용 프로그램의 콘텐츠를 검색할 수 있도록 웹 태그를 사용 하는 데 필요한 네 가지 단계가 있습니다.

1. Apple 검색 하 고로 정의 하 여 앱의 웹 사이트를 인덱싱할 수 있는지 확인은 **지원** 또는 **마케팅** iTunes Connect에서에서 웹 사이트입니다.
2. 응용 프로그램의 웹 사이트에 모바일 직접 링크를 구현 하는 필요한 태그를 포함 되어 있는지 확인 합니다. 자세한 내용은 아래 섹션을 참조 하십시오.
3. 처리 하 여 iOS 앱의 딥 링크를 사용 하도록 설정 합니다.
4. 최종 사용자에 게 풍부 하 고 매력적인 결과 제공 하는 응용 프로그램의 웹 사이트에 표시 된 구조화 된 데이터에 대 한 태그를 추가 합니다. 이 단계는 엄격 하 게 필수 사항이 아니나, Apple에서는 것이 좋습니다.

다음 섹션에서는 이러한 단계를 자세히 살펴봅니다.

## <a name="make-your-apps-website-discoverable"></a>응용 프로그램의 웹 사이트를 검색 가능

로 사용 하는 Apple 응용 프로그램의 웹 사이트 찾기 하는 가장 쉬운 방법은 **지원** 또는 **마케팅** iTunes Connect 통해 Apple에 앱을 제출할 때 웹 사이트입니다.

## <a name="using-smart-app-banners"></a>스마트 앱 배너를 사용 하 여

응용 프로그램에 명확한 링크를 제공 하도록 웹 사이트에 스마트 앱 내 배너를 제공 합니다. 응용 프로그램이 아직 설치 되지 않은 경우 Safari에 자동으로 사용자가 앱을 설치 하도록 묻습니다. 그렇지 않은 경우 사용 하 여 얻을 수는 **보기** 링크를 웹 사이트에서 앱을 시작 합니다. 예를 들어 스마트 앱 내 배너를 만들려면 다음 코드를 사용할 수 있습니다.

```xml
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

자세한 내용은 Apple의를 참조 하십시오 [스마트 앱 배너를 사용 하 여 앱 승격](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html) 설명서입니다.

## <a name="using-universal-links"></a>범용 링크를 사용 하 여

새로운 iOS 9에 유니버설 링크는 다음을 제공 하 여 스마트 앱 배너 또는 기존 사용자 지정 URL 스키마 보다 더 효율적을 제공 합니다.

- **고유** – 둘 이상의 웹 사이트에서 동일한 URL을 요구할 수 없습니다.
- **보안** – 서명 된 인증서가 필요 하 고 유효 하 게 웹 사이트를 소유 하는 웹 사이트에 연결 된 앱입니다.
- **유연한** – 최종 사용자 웹 사이트 또는 응용 프로그램 URL을 시작 하는지 여부를 제어할 수 있습니다.
- **유니버설** – 웹 사이트와 응용 프로그램의 콘텐츠를 정의 하는 동일한 URL을 사용할 수 있습니다.

## <a name="using-twitter-cards"></a>Twitter 카드를 사용 하 여

딥 링크 Twitter 카드를 사용 하는 응용 프로그램의 콘텐츠를 제공할 수 있습니다. 예를 들어:

```xml
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

자세한 내용은 Twitter의를 참조 하십시오 [카드 프로토콜 Twitter](http://dev.twitter.com/cards/mobile) 설명서입니다.

## <a name="using-facebook-app-links"></a>Facebook 응용 프로그램 링크를 사용 하 여

딥 링크는 Facebook 앱의 링크를 사용 하 여 응용 프로그램의 콘텐츠를 제공할 수 있습니다. 예를 들어:

```xml
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

자세한 내용은 Facebook의를 참조 하십시오 [앱 링크](http://applinks.org) 설명서입니다.

## <a name="opening-deep-links"></a>딥 링크 열기

열기 및 Xamarin.iOS 앱의 딥 링크를 표시 하기 위한 지원을 추가 해야 합니다. 편집 된 **AppDelegate.cs** 파일을 재정의 `OpenURL` 사용자 지정 URL 형식을 처리 하려면 메서드. 예를 들어:

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

위의 코드에서 찾고 포함 하는 URL에 대 한 `/appname` 의 값을 전달 하 고 `query` (`123` 이 예에서) 사용자에 게 요청된 된 콘텐츠를 표시 하려면 앱의 사용자 지정 보기 컨트롤러에 있습니다.

## <a name="providing-rich-results-with-structured-data"></a>구조화 된 데이터를 사용 하 여 서식 있는 결과 제공합니다.

구조화 된 데이터 태그를 포함 하 여 제목 및 설명 단순히 이외에 최종 사용자에 게 풍부한 검색 결과 제공할 수 있습니다. 이미지, 응용 프로그램 특정 데이터 (예: 등급) 및 동작을 구조화 된 데이터 태그를 사용 하 여 결과 포함 합니다.

서식 있는 결과가 더 많은 참여 되 고 향상 시킬 수 있습니다 클라우드에서 순위와 상호 작용 하는 더 많은 사용자가 유인 하 여 검색 인덱스를 기반으로 합니다.

구조적 데이터 태그를 제공 하는 한 가지 옵션을 오픈 그래프를 사용 합니다. 예를 들어:

```xml
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

자세한 내용은 참조 하십시오는 [오픈 그래프](http://ogp.me) 웹 사이트입니다.

구조화 된 데이터 표시를 위해 또 다른 일반적인 형식은 schema.org의 마이크로 데이터 형식이입니다. 예를 들어:

```xml
<div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
    <span itemprop="ratingValue">4** stars -
    <span itemprop="reviewCount">255** reviews


```

동일한 정보는 다음과 같은 schema.org의 LD JSON 형식으로 나타낼 수 있습니다.

```xml
<script type="application/ld+json">
    "@content":"http://schema.org",
    "@type":"AggregateRating",
    "ratingValue":"4",
    "reviewCount":"255"
</script>
```

다음은 최종 사용자에 게 풍부한 검색 결과 제공 하 여 웹 사이트에서 메타 데이터의 예입니다.

[![](web-markup-images/deeplink01.png "서식 있는 구조화 된 데이터 태그를 통해 결과 검색")](web-markup-images/deeplink01.png#lightbox)

Apple에는 현재 schema.org에서 다음과 같은 스키마 형식을 지원합니다.

 - AggregateRating
 - ImageObject
 - InteractionCount
 - 제공
 - 조직
 - PriceRange
 - 레시피
 - SearchAction

이러한 체계 형식에 대 한 자세한 내용은 참조 하십시오 [schema.org](http://schema.org)합니다.

## <a name="providing-actions-with-structured-data"></a>구조화 된 데이터를 사용 하 여 동작을 제공

특정 유형의 구조적 데이터를 사용 하면 검색 결과 최종 사용자에 의해 동작 가능 합니다. 현재 다음 작업이 지원 됩니다.

 - 전화 번호를 전화를 걸고 있습니다.
 - 주어진된 주소에 대 한 지도 방향을 가져오는 중입니다.
 - 오디오 또는 비디오 파일을 재생 합니다.

예를 들어, 전화 번호를 동작 정의 다음과 같습니다.

```xml
<div itemscope itemtype="http://schema.org/Organization">
    <span itemprop="telephone">(408) 555-1212**


```

이 검색 결과 최종 사용자에 게 표시 되 면 하는 경우 결과에 작은 전화 아이콘이 표시 됩니다. 사용자가 있는 아이콘을 지정 된 번호로 호출 됩니다.

다음 HTML 검색 결과에서 오디오 파일을 재생 하는 작업을 추가 합니다.

```xml
<div itemscope itemtype="http://schema.org/AudioObject">
    <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**


```

마지막으로, 다음 HTML에서 검색 결과를 작업을 추가 합니다.

```xml
<div itemscope itemtype="http://schema.org/PostalAddress">
    <span itemprop="streetAddress">1 Infinite Loop**
    <span itemprop="addressLocality">Cupertino**
    <span itemprop="addressRegion">CA**
    <span itemprop="postalCode">95014**


```

자세한 내용은 Apple의를 참조 하십시오 [앱 검색 개발자 사이트](http://developer.apple.com/ios/search/)합니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [앱 검색 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
