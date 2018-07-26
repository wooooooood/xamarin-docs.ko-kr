---
title: Xamarin.iOS에서 웹 보기
description: 이 문서에서는 Xamarin.iOS 앱을 웹 콘텐츠를 표시할 수는 다양 한 방법을 설명 합니다. UIWebView, WKWebView, 필요한 SFSafariViewController, Safari 및 앱 전송 보안에 설명 합니다.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 56b0f0e910cc95ca50d1e5e460ce71a1c8f669a2
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242042"
---
# <a name="web-views-in-xamarinios"></a>Xamarin.iOS에서 웹 보기

Apple는 iOS의 수명 동안 다양 한 방법으로 해당 앱의 웹 보기 기능을 통합 하기 위해 앱 개발자를 위한를 출시 했습니다. 대부분의 사용자는 iOS 장치에서 기본 제공 Safari 웹 브라우저를 활용 하 고 따라서이 환경과 일치 하는 다른 앱의 웹 보기 기능이 예상 합니다. 동일한 제스처가 작동 되도록 par, 및 기능에 동일한 성능을 기대 합니다.

이 문서에서는 살펴봅니다 Apple에서 제공 하는 세 가지 웹 보기의 각: `UIWebView`, `WKWebview`, 및 `SFSafariViewController`등의 유사성 및 차이점을 어떻게 사용할 수 있습니다. 

iOS 11 모두에 새 변경 내용을 도입 `WKWebView` 고 `SFSafariViewController`입니다. 에 대 한 자세한 내용은 참조는 [iOS 11 가이드의 변경 내용을 웹](~/ios/platform/introduction-to-ios11/web.md) 가이드입니다.

## <a name="uiwebview"></a>UIWebView

`UIWebView` 앱에서 웹 콘텐츠를 제공 하는 Apple의 레거시 방법입니다. IOS 2.0에서에서 릴리스 되었으며 8.0을 기준으로 사용 되지 합니다.

IOS 버전 8.0 이전의 지원 하려는 경우 사용 해야 합니다 `UIWebView`합니다. 때문에 `UIWebView` 는 덜 최적화 성능에 대 한 대안에 비해, 것이 좋습니다는 사용자의 iOS 버전을 확인 해야 합니다. 하는 경우 해당 8.0 또는 사용 하 여 위의 옵션 중 하나에 대해 설명 아래 더 나은 사용자 환경을 만듭니다.
 
UIWebView Xamarin.iOS 앱에 추가 하려면 다음 코드를 사용 합니다.
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

이 다음 웹 보기를 생성합니다.

[![](uiwebview-images/webview.png "ScalesPagesToFit의 효과")](uiwebview-images/webview.png#lightbox)

사용 하 여 대 한 자세한 내용은 `UIWebView`, 다음 레시피를 참조 하세요.


- [웹 페이지를 로드 합니다.](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_a_web_page)
- [로컬 콘텐츠를 로드 합니다.](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_local_content)
- [비 웹 문서를 로드 합니다.](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_non-web_documents)

## <a name="wkwebview"></a>WKWebView

`WKWebView` 웹 브라우징 모바일 safari와 비슷한 인터페이스를 구현 하는 허용 앱 개발자 iOS 8에서에서 도입 되었습니다. 이 사실에 부분적으로 만료 되는 `WKWebView` Nitro Javascript 엔진, 모바일 Safari에서 사용 하는 동일한 엔진을 사용 합니다. `WKWebView` 항상 사용 해야 UIWebView로 인해 불가능 했던 over 합니다 [성능이 향상](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), 웹 페이지와 앱 간의 상호 작용의 용이성 및 친숙 한 제스처를 기본 제공 합니다.
  
`WKWebView` 수에 추가할 수 앱 UIWebView에 거의 동일한 방법으로 개발자 UI/UX 및 기능을 통해 더 많은 제어 해야 합니다. 하지만 만들기 및 웹 뷰 개체를 표시는 뷰에 제공 되는 방식은, 사용자를 탐색 하는 방법 및 사용자 보기를 종료 하는 방법을 제어할 수 있습니다 요청된 된 페이지에 표시 됩니다.  

시작 하려면 아래 코드를 사용할 수는 `WKWebView` Xamarin.iOS 앱에서:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

이 다음 웹 보기를 생성합니다.

[![](uiwebview-images/wkwebview.png "ScalesPagesToFit 없이 예제 웹 보기")](uiwebview-images/wkwebview.png#lightbox)

것이 중요 한 사실은 `WKWebView` 이므로 WebKit 네임 스페이스를 using 지시문 클래스의 맨 위에 추가 해야 합니다.

`WKWebView` Xamarin.Mac 앱 내에서 사용할 수 있으며 따라서 하려는 플랫폼 간 Mac/iOS 앱을 만드는 경우에 사용 하는 것이 좋습니다.

합니다 [JavaScript 경고 처리](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) 작성법에 대 한 정보도 WKWebView를 사용 하 여 Javascript를 사용 하 여

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` 앱에서 웹 콘텐츠를 제공 하는 최신 방법 이며 이상 iOS 9에서에서 사용할 수 있습니다. 와 달리 `UIWebView` 나 `WKWebView`, `SFSafariViewController` 뷰 컨트롤러 이며 다른 뷰를 사용 하 여 사용할 수 없습니다. 표시 해야 `SFSafariViewController` 새 뷰 컨트롤러로 동일한 방식으로 있습니다 초래 모든 뷰 컨트롤러입니다.
 
 `SFSafariViewController` 기본적으로 '미니 safari' 앱에 포함 될 수 있는 경우 WKWebView 같은 클래스는 같은 Nitro Javascript 엔진을 사용 하지만 또한 자동 채우기, 판독기 및 모바일 Safari를 사용 하 여 쿠키 및 데이터를 공유 하는 기능 같은 추가 Safari 기능의 범위를 제공 합니다. 사용자 간의 상호 작용 및 `SFSafariViewController` 에 앱에 액세스할 수 없습니다. 앱은 기본 Safari 기능에 액세스할 수 없습니다.
 
또한 기본적으로 구현 하는 **수행** 단추를 앱으로 쉽게 돌아와 전달 하 고 뒤로 탐색 단추를 사용자가 웹 페이지의 스택을 탐색을 허용 하는 사용자에 게 허용 합니다. 또한도 제공 사용자는 예상된 된 웹 페이지에는 마음의 평화를 제공 하는 바는 주소를 사용 하 여 합니다. 주소 표시줄에 url을 변경할 수가 없도록 합니다. 

이러한 구현은 변경할 수 없습니다.이 `SFSafariViewController` 앱에서 사용자 지정 하지 않고 웹 페이지를 제공 하려는 경우를 기본 브라우저로 사용 하는 데 적합 합니다.

시작 하려면 아래 코드를 사용할 수는 `SFSafariViewController` Xamarin.iOS 앱에서:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

이 다음 웹 보기를 생성합니다.

[![](uiwebview-images/sfsafariviewcontroller.png "필요한 SFSafariViewController 사용 하 여 예제 웹 보기")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

아래 코드를 사용 하 여 앱 내에서 모바일 Safari 앱을 열 수 이기도 합니다.

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

이 다음 웹 보기를 생성합니다.

[![](uiwebview-images/safari.png "Safari에서 제공 하는 웹 페이지")](uiwebview-images/safari.png#lightbox)

Safari로 앱에서 사용자를 이동 합니다. 일반적으로 항상 피해 야 합니다. 대부분의 사용자는 응용 프로그램 외부에서 탐색을 예상 하지 앱에서 이동 하면 사용자가 돌아가지 않을 수 있습니다, 기본적으로 engagement를 종료 하도록 합니다.

iOS 9 향상 Safari 페이지의 왼쪽된 위 모퉁이에 제공 된 뒤로 단추를 통해 앱으로 쉽게 돌아와 사용자를 허용 합니다.

## <a name="app-transport-security"></a>앱 전송 보안

앱 전송 보안, 또는 *ATS* iOS 모든 인터넷 통신 보안 연결 모범 사례를 따르는지 9에서에서 Apple에서 도입 되었습니다.

ATS에 대 한 자세한 내용은를 참조 앱에서 구현 하는 방법을 포함 하는 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 가이드입니다.

## <a name="related-links"></a>관련 링크

- [웹 보기로 (샘플)](https://developer.xamarin.com/samples/monotouch/WebView/)
