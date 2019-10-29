---
title: Xamarin.ios의 웹 보기
description: 이 문서에서는 Xamarin.ios 앱이 웹 콘텐츠를 표시 하는 다양 한 방법에 대해 설명 합니다. UIWebView 보기, WKWebView, SFSafariViewController, Safari 및 앱 전송 보안에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 210e59239957d3963a3d3275315a0eac14748ff8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021781"
---
# <a name="web-views-in-xamarinios"></a>Xamarin.ios의 웹 보기

IOS Apple의 수명 동안 앱 개발자가 앱에서 웹 보기 기능을 통합 하는 다양 한 방법을 출시 했습니다. 대부분의 사용자는 iOS 장치에서 기본 제공 Safari 웹 브라우저를 활용 하므로 다른 앱의 웹 보기 기능이이 환경과 일치 하는 것으로 간주 됩니다. 동일 하 게 작동 하는 것으로 간주 되 고 성능이 동일 하 고 기능이 동일 하 게 됩니다.

이 문서에서는 Apple에서 제공 하는 세 가지 웹 보기, `UIWebView`, `WKWebview`, `SFSafariViewController`, 유사성 및 차이점, 사용 방법 등을 살펴봅니다. 

iOS 11에는 `WKWebView` 및 `SFSafariViewController`에 대 한 새로운 변경 사항이 도입 되었습니다. 이러한 항목에 대 한 자세한 내용은 [iOS 11의 웹 변경 가이드](~/ios/platform/introduction-to-ios11/web.md) 가이드를 참조 하세요.

## <a name="uiwebview"></a>UIWebView

`UIWebView` 앱에서 웹 콘텐츠를 제공 하는 Apple의 레거시 방법입니다. IOS 2.0에 출시 되었으며 8.0부터 사용 되지 않습니다.

8\.0 이전의 iOS 버전을 지원 하려는 경우 `UIWebView`를 사용 해야 합니다. `UIWebView`는 다른 방법 보다 성능에 최적화 되지 않으므로 사용자의 iOS 버전을 확인 하는 것이 좋습니다. 8\.0 이상이 면 아래 설명 된 옵션 중 하나를 사용 하 여 더 나은 사용자 환경을 만들 수 있습니다.

Xamarin.ios 앱에 UIWebView 보기를 추가 하려면 다음 코드를 사용 합니다.

```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

그러면 다음과 같은 웹 보기가 생성 됩니다.

[![](uiwebview-images/webview.png "The effect of ScalesPagesToFit")](uiwebview-images/webview.png#lightbox)

`UIWebView`사용에 대 한 자세한 내용은 다음 조리법을 참조 하세요.

- [웹 페이지 로드](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_a_web_page)
- [로컬 콘텐츠 로드](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_local_content)
- [비 웹 문서 로드](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_non-web_documents)

## <a name="wkwebview"></a>WKWebView

앱 개발자가 모바일 Safari와 유사한 웹 검색 인터페이스를 구현할 수 있도록 iOS 8에서 도입 된 `WKWebView`. 이는 부분적으로 모바일 Safari에서 사용 하는 것과 동일한 엔진인 Nitro Javascript 엔진을 사용 하는 `WKWebView`입니다. 매우 [향상 된 성능](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), 사용자에 게 친숙 한 제스처 및 웹 페이지와 앱 간의 상호 작용 용이성으로 인해 항상 uiwebview 보기에서 사용 해야 합니다. `WKWebView`
  
`WKWebView`는 UIWebView 보기와 거의 동일한 방식으로 앱에 추가할 수 있지만, 개발자는 UI/UX 및 기능을 훨씬 더 많이 제어할 수 있습니다. 웹 뷰 개체를 만들고 표시 하면 요청 된 페이지가 표시 됩니다. 그러나 보기가 표시 되는 방법, 사용자가 탐색 하는 방법 및 사용자가 보기를 종료 하는 방법을 제어할 수 있습니다.  

아래 코드를 사용 하 여 Xamarin.ios 앱에서 `WKWebView`를 시작할 수 있습니다.

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

그러면 다음과 같은 웹 보기가 생성 됩니다.

[![](uiwebview-images/wkwebview.png "An example web view without ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

`WKWebView`는 WebKit 네임 스페이스에 있으므로이 using 지시문을 클래스의 맨 위에 추가 해야 합니다.

`WKWebView`은 Xamarin.ios 앱 내에서 사용할 수도 있으므로 플랫폼 간 Mac/iOS 앱을 만드는 경우이를 사용 하는 것을 고려할 수 있습니다.

Javascript [경고 처리](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) 조리법은 javascript와 함께 WKWebView를 사용 하는 방법에 대 한 정보도 제공 합니다.

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController

 `SFSafariViewController`는 앱에서 웹 콘텐츠를 제공 하는 최신 방법이 며 iOS 9 이상에서 사용할 수 있습니다. `UIWebView` 또는 `WKWebView`와 달리 `SFSafariViewController`는 뷰 컨트롤러 이므로 다른 뷰와 함께 사용할 수 없습니다. 뷰 컨트롤러를 표시 하는 것과 같은 방법으로 `SFSafariViewController`를 새 뷰 컨트롤러로 제공 해야 합니다.

 `SFSafariViewController`은 기본적으로 앱에 포함할 수 있는 ' 미니 safari '입니다. WKWebView와 마찬가지로 동일한 Nitro Javascript 엔진을 사용 하지만 자동 채우기, 판독기 및 모바일 Safari와 쿠키 및 데이터를 공유 하는 기능과 같은 추가 Safari 기능을 제공 합니다. 사용자와 `SFSafariViewController` 간의 상호 작용은 앱에 액세스할 수 없습니다. 앱은 기본 Safari 기능에 액세스할 수 없습니다.

또한 기본적으로 사용자가 앱으로 쉽게 돌아갈 수 있도록 하 고 사용자가 웹 페이지의 스택을 탐색할 수 있도록 하 여 사용자가 앱으로 쉽게 돌아가 **탐색 단추를 전달 하는 데** 사용할 수 있습니다. 또한 사용자에 게 예상 되는 웹 페이지에 있다는 점에 대 한 평화를 제공 하는 주소 표시줄을 제공 합니다. 주소 표시줄에서 사용자가 url을 변경할 수 없습니다. 

이러한 구현은 변경 될 수 없으므로 앱이 사용자 지정 없이 웹 페이지를 제공 하려는 경우에는 `SFSafariViewController`를 기본 브라우저로 사용 하는 것이 좋습니다.

아래 코드를 사용 하 여 Xamarin.ios 앱에서 `SFSafariViewController`를 시작할 수 있습니다.

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

그러면 다음과 같은 웹 보기가 생성 됩니다.

[![](uiwebview-images/sfsafariviewcontroller.png "An example web view with SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

아래 코드를 사용 하 여 앱 내에서 모바일 Safari 앱을 열 수도 있습니다.

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

그러면 다음과 같은 웹 보기가 생성 됩니다.

[![](uiwebview-images/safari.png "A web page presented in Safari")](uiwebview-images/safari.png#lightbox)

사용자를 앱에서 Safari로 이동 하는 것은 일반적으로 항상 피해 야 합니다. 대부분의 사용자는 응용 프로그램 외부에서 탐색을 필요로 하지 않으므로 앱에서 벗어나면 사용자는이를 반환 하지 않을 수 있으며,이는 기본적으로 참여를 중단 합니다.

iOS 9의 향상 된 기능을 통해 사용자는 Safari 페이지의 왼쪽 위 모서리에 있는 뒤로 단추를 통해 쉽게 앱으로 돌아갈 수 있습니다.

## <a name="app-transport-security"></a>앱 전송 보안

앱 전송 보안 또는 *ATS* 는 모든 인터넷 통신이 보안 연결 모범 사례를 준수 하도록 iOS 9의 Apple에서 도입 되었습니다.

앱에서 응용 프로그램을 구현 하는 방법을 비롯 하 여 ATS에 대 한 자세한 내용은 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 가이드를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [웹 보기 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
