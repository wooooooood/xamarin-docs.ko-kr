---
title: Xamarin.ios에서 웹 태그를 사용 하 여 검색
description: 이 문서에서는 Xamarin.ios 앱에 다시 연결 되는 웹 기반 검색 결과를 만드는 방법을 설명 합니다. 웹 콘텐츠 인덱싱을 사용 하도록 설정 하 고, 스마트 앱 배너, 유니버설 링크 등을 사용 하 여 앱의 웹 사이트를 검색할 수 있도록 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 3d5db2f060b59fc689bea99141342b0447ac8933
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031523"
---
# <a name="search-with-web-markup-in-xamarinios"></a>Xamarin.ios에서 웹 태그를 사용 하 여 검색

웹 사이트를 통해 해당 콘텐츠에 대 한 액세스를 제공 하는 앱의 경우 (앱 내 에서만) 웹 콘텐츠는 Apple에서 크롤링하는 특수 링크를 사용 하 여 표시 될 수 있으며 사용자의 iOS 9 장치에서 앱에 대 한 딥 링크를 제공 합니다.

IOS 앱이 이미 모바일 딥 링크를 지원 하 고 웹 사이트에서 앱 내의 콘텐츠에 대 한 딥 링크를 제공 하는 경우 Apple의 _남은 Ebot_ 웹 크롤러는이 콘텐츠를 인덱싱하고 해당 클라우드 인덱스에 자동으로 추가 합니다.

[![](web-markup-images/webmarkup01.png "Cloud Index overview")](web-markup-images/webmarkup01.png#lightbox)

Apple은 이러한 결과를 스포트라이트 검색 및 Safari 검색 결과에 노출 합니다.
사용자가 이러한 결과 중 하나를 탭 하면 (그리고 앱이 설치 된 경우) 앱의 콘텐츠로 이동 합니다.

[![](web-markup-images/webmarkup02.png "Deep linking from a website in search results")](web-markup-images/webmarkup02.png#lightbox)

## <a name="enabling-web-content-indexing"></a>웹 콘텐츠 인덱싱 사용

웹 태그를 사용 하 여 앱의 콘텐츠를 검색 가능 하 게 만드는 데 필요한 네 가지 단계가 있습니다.

1. Apple에서 iTunes Connect의 **지원** 또는 **마케팅** 웹 사이트로 정의 하 여 앱의 웹 사이트를 검색 하 고 인덱싱할 수 있는지 확인 합니다.
2. 앱의 웹 사이트에 모바일 딥 링크를 구현 하는 데 필요한 태그가 포함 되어 있는지 확인 합니다. 자세한 내용은 아래 섹션을 참조 하세요.
3. IOS 앱에서 심층 링크 처리를 사용 하도록 설정 합니다.
4. 최종 사용자에 게 풍부한 결과를 제공 하기 위해 앱의 웹 사이트에서 표시 하는 구조화 된 데이터에 대 한 태그를 추가 합니다. 이 단계를 반드시 수행 해야 하는 것은 아니지만 Apple에서 매우 권장 됩니다.

다음 섹션에서는 이러한 단계를 자세히 설명 합니다.

## <a name="make-your-apps-website-discoverable"></a>앱의 웹 사이트를 검색 가능 하도록 설정

Apple에서 앱의 웹 사이트를 찾는 가장 쉬운 방법은 iTunes Connect를 통해 Apple에 앱을 제출할 때 **지원** 또는 **마케팅** 웹 사이트로 사용 하는 것입니다.

## <a name="using-smart-app-banners"></a>스마트 앱 배너 사용

웹 사이트에 스마트 앱 배너를 제공 하 여 앱에 대 한 명확한 링크를 제공 합니다. 앱이 아직 설치 되지 않은 경우 Safari는 사용자에 게 앱을 설치 하 라는 메시지를 자동으로 표시 합니다. 그렇지 않으면 사용에서 **보기** 링크를 탭 하 여 웹 사이트에서 앱을 시작할 수 있습니다. 예를 들어 스마트 앱 배너를 만들려면 다음 코드를 사용할 수 있습니다.

```html
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

자세한 내용은 Apple의 [스마트 앱 배너를 사용 하 여 앱 승격](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html) 설명서를 참조 하세요.

## <a name="using-universal-links"></a>유니버설 링크 사용

IOS 9를 처음 접하는 경우에는 다음을 제공 하 여 범용 링크를 통해 스마트 앱 배너 또는 기존 사용자 지정 URL 구성표 대신 사용할 수 있습니다.

- **고유** – 둘 이상의 웹 사이트에서 동일한 URL을 요구할 수 없습니다.
- **보안** – 웹 사이트가 사용자가 소유 하 고 앱에 유효 하 게 연결 되어 있는지 확인 하는 웹 사이트에 서명 된 인증서가 필요 합니다.
- **유연** – 최종 사용자는 URL이 웹 사이트 또는 앱을 시작할지 여부를 제어할 수 있습니다.
- **유니버설** – 동일한 URL을 사용 하 여 웹 사이트와 앱 콘텐츠를 모두 정의할 수 있습니다.

## <a name="using-twitter-cards"></a>Twitter 카드 사용

Twitter 카드를 사용 하 여 앱 콘텐츠에 대 한 딥 링크를 제공할 수 있습니다. 예를 들면,

```html
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

자세한 내용은 Twitter의 [Twitter 카드 프로토콜](https://developer.twitter.com/en/docs/tweets/optimize-with-cards/overview/abouts-cards) 설명서를 참조 하세요.

## <a name="using-facebook-app-links"></a>Facebook 앱 링크 사용

Facebook 앱 링크를 사용 하 여 앱 콘텐츠에 대 한 딥 링크를 제공할 수 있습니다. 예를 들면,

```html
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

자세한 내용은 Facebook의 [앱 링크](https://developers.facebook.com/docs/applinks) 설명서를 참조 하세요.

## <a name="opening-deep-links"></a>딥 링크 열기

Xamarin.ios 앱에서 딥 링크를 열고 표시 하기 위한 지원을 추가 해야 합니다. **AppDelegate.cs** 파일을 편집 하 고 사용자 지정 URL 형식을 처리 하도록 `OpenURL` 메서드를 재정의 합니다. 예를 들면,

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

위의 코드에서 `/appname` 포함 하는 URL을 찾고 사용자에 게 요청 된 콘텐츠를 표시 하기 위해 앱의 사용자 지정 뷰 컨트롤러에 `query` (이 예제에서는`123`) 값을 전달 합니다.

## <a name="providing-rich-results-with-structured-data"></a>구조화 된 데이터를 사용 하 여 다양 한 결과 제공

구조화 된 데이터 태그를 포함 하 여 최종 사용자에 게 단순한 제목 및 설명 이상의 다양 한 검색 결과를 제공할 수 있습니다. 구조화 된 데이터 태그를 사용 하 여 이미지, 앱 특정 데이터 (예: 등급) 및 결과에 대 한 작업을 포함 합니다.

다양 한 결과를 활용 하 여 더 많은 사용자가 상호 작용 하도록 enticing 하 여 클라우드 기반 검색 인덱스에서 순위를 향상할 수 있습니다.

구조화 된 데이터 태그를 제공 하는 한 가지 옵션은 Open Graph를 사용 하는 것입니다. 예를 들면,

```html
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

자세한 내용은 Graph 웹 사이트 [열기](https://ogp.me) 를 참조 하세요.

구조화 된 데이터 태그의 또 다른 일반적인 형식은 schema. org의 마이크로 데이터 형식입니다. 예를 들면,

```html
<div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
  <span itemprop="ratingValue">4** stars -
  <span itemprop="reviewCount">255** reviews
```

동일한 정보를 schema. 조직의 JSON-LD 형식으로 나타낼 수 있습니다.

```html
<script type="application/ld+json">
  "@content":"http://schema.org",
  "@type":"AggregateRating",
  "ratingValue":"4",
  "reviewCount":"255"
</script>
```

다음은 최종 사용자에 게 다양 한 검색 결과를 제공 하는 웹 사이트의 메타 데이터 예를 보여 줍니다.

[![](web-markup-images/deeplink01.png "Rich search results via Structured Data Markup")](web-markup-images/deeplink01.png#lightbox)

현재 Apple은 schema.org에서 다음 스키마 유형을 지원 합니다.

- AggregateRating
- ImageObject
- InteractionCount
- 에서는
- 직
- PriceRange
- 피
- SearchAction

이러한 구성표 유형에 대 한 자세한 내용은 [schema.org](https://schema.org)를 참조 하세요.

## <a name="providing-actions-with-structured-data"></a>구조화 된 데이터에 대 한 작업 제공

특정 유형의 구조화 된 데이터를 사용 하면 최종 사용자가 검색 결과를 조치 가능 하 게 할 수 있습니다. 현재 지원 되는 작업은 다음과 같습니다.

- 전화 번호로 전화를 걸고 있습니다.
- 지정 된 주소에 대 한 지도 방향을 가져옵니다.
- 오디오 또는 비디오 파일 재생

예를 들어 전화 번호로 전화를 거는 작업을 정의 하는 것은 다음과 같습니다.

```html
<div itemscope itemtype="http://schema.org/Organization">
  <span itemprop="telephone">(408) 555-1212**
```

최종 사용자에 게이 검색 결과가 표시 되 면 결과에 작은 휴대폰 아이콘이 표시 됩니다. 사용자가 아이콘을 탭 하면 지정 된 숫자가 호출 됩니다.

다음 HTML은 검색 결과에서 오디오 파일을 재생 하는 작업을 추가 합니다.

```html
<div itemscope itemtype="http://schema.org/AudioObject">
  <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**
```

마지막으로 다음 HTML은 검색 결과에서 방향을 가져오는 작업을 추가 합니다.

```html
<div itemscope itemtype="http://schema.org/PostalAddress">
  <span itemprop="streetAddress">1 Infinite Loop**
  <span itemprop="addressLocality">Cupertino**
  <span itemprop="addressRegion">CA**
  <span itemprop="postalCode">95014**
```

자세한 내용은 Apple의 [앱 검색 개발자 사이트](https://developer.apple.com/ios/search/)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [앱 검색 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
