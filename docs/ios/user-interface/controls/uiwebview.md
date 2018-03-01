---
title: "웹 보기"
description: "구체화 iOS 웹 보기 옵션"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 83e01bba82713b55a0a69858c2a83aa243f71588
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="web-views"></a>웹 보기

Apple는 iOS의 수명 동안 다양 한 방법으로 앱에서 웹 보기 기능을 통합 하는 응용 프로그램 개발자에 대 한를 출시 했습니다. 대부분의 사용자는 iOS 장치에서 기본 제공 Safari 웹 브라우저를 사용 하며 따라서 다른 앱의 기능을 웹 보기는이 환경과 일치를 바랍니다. 동일 하 게 par, 및 기능에 대 한 성능 작동 하도록 하는 동일한 제스처 기대 합니다.

이 문서에서는 살펴볼 것 각 Apple에서 제공한 세 가지 웹 보기: `UIWebView`, `WKWebview`, 및 `SFSafariViewController`등의 유사점 및 차이점 및 방법을 사용할 수 있습니다. 

iOS 11 둘 다에 새로운 변경 사항이 도입 되었습니다. `WKWebView` 및 `SFSafariViewController`합니다. 이러한 이벤트에 대해 자세한 내용은 참조는 [iOS 11 가이드의 변경 내용을 웹](~/ios/platform/introduction-to-ios11/web.md) 가이드입니다.

## <a name="uiwebview"></a>UIWebView

`UIWebView` 방법은 Apple의 레거시 응용 프로그램에서 웹 콘텐츠를 제공 합니다. 2.0, iOS에서 릴리스된 하 고 8.0부터 더 이상 사용 되지 합니다.

IOS 버전 8.0 이전 지원 하려는 경우 사용 해야 합니다 `UIWebView`합니다. 때문을 `UIWebView` 은 덜 최적화 성능에 대 한 대체 방법 보다, 것이 좋습니다 사용자가 iOS 버전을 확인 해야 함을 합니다. 경우 것 8.0 또는 사용 하 여 위의 옵션 중 하나에 대해 설명 아래 더 나은 사용자 환경을 만듭니다.
 
UIWebView Xamarin.iOS 앱을 추가 하려면 다음 코드를 사용 합니다.
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

다음 웹 보기를 생성합니다.

[ ![](uiwebview-images/webview.png "ScalesPagesToFit의 효과")](uiwebview-images/webview.png)

사용 하 여 대 한 자세한 내용은 `UIWebView`, 다음 레시피를 참조 하십시오.


- [웹 페이지를 로드 합니다.](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_a_web_page/)
- [로컬 콘텐츠를 로드 합니다.](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_local_content/)
- [-웹 문서 로드](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_non-web_documents/)

## <a name="wkwebview"></a>WKWebView

`WKWebView` iOS 8 하면 웹 검색 모바일 Safari와 비슷한 인터페이스를 구현 하려면 응용 프로그램 개발자가에서 도입 되었습니다. 이것은 팩트로 부분적으로 인해는 `WKWebView` 모바일 Safari에서 사용 하는 동일한 엔진 Nitro Javascript 엔진을 사용 합니다. `WKWebView` 항상 사용 해야 over UIWebView 된에 기록할 수는 [성능을 향상 시킬](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), 기본 제공 웹 페이지와 응용 프로그램 간의 상호 작용의 용이성 및 제스처를 사용자에 게 친숙 합니다.
  
`WKWebView` 수에 추가할 수 앱 UIWebView에 거의 동일한 방법으로 개발자 UI/UX와 기능에 대해 훨씬 더 많은 제어 해야 합니다. 하지만 만들기 및 웹 view 개체를 표시는 뷰에 표시 되는 방식을 어떻게 탐색할 수 있습니다, 하 고 사용자가 보기를 종료 하는 방법을 제어할 수 있습니다 요청된 된 페이지에 표시 됩니다.  

아래 코드를 사용 하 여을 시작할 수 있습니다는 `WKWebView` Xamarin.iOS 앱에서:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

다음 웹 보기를 생성합니다.

[ ![](uiwebview-images/wkwebview.png "ScalesPagesToFit 없이 예제 웹 보기")](uiwebview-images/wkwebview.png)

사항에 유의 해야 `WKWebView` 이므로 WebKit 네임 스페이스, 클래스의 맨 위에 지시문을 사용 하 여 추가 해야 합니다.

`WKWebView` Xamarin.Mac 앱 내에서 사용할 수 있으며 따라서 하려는 경우는 플랫폼 간 Mac/iOS 앱을 만드는 경우 사용 하는 것이 좋습니다.

[JavaScript 경고 처리](https://developer.xamarin.com/recipes/ios/content_controls/web_view/handle_javascript_alerts/) 레시피에 대 한 정보도 WKWebView Javascript 사용

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` 응용 프로그램에서 웹 콘텐츠를 제공 하는 최신 방법 이며 이상 iOS 9에서에서 제공 됩니다. 와 달리 `UIWebView` 또는 `WKWebView`, `SFSafariViewController` 보기 컨트롤러가 고 다른 보기와 사용할 수 없습니다. 표시 하도록 `SFSafariViewController` 새 보기 컨트롤러로 같은 방식으로 사용자를 나타낼 모든 뷰-컨트롤러입니다.
 
 `SFSafariViewController` 기본적으로 '미니 safari' 앱에 포함할 수 있는입니다. WKWebView 처럼이 클래스는 동일한 Nitro Javascript 엔진을 사용 하 여 하지만 또한 자동 채우기, 판독기 및 모바일 safari 쿠키와 데이터를 공유 하는 기능 등의 추가 Safari 기능 범위를 제공 합니다. 사용자 간의 상호 작용 및 `SFSafariViewController` 앱에 액세스할 수 있습니다. 앱에서 기본 Safari 기능에 대 한 액세스를 않아도 됩니다.
 
또한 기본적으로 구현 하는 **수행** 단추를 응용 프로그램으로 쉽게 돌아와 및 앞으로 및 뒤로 탐색 단추를 사용자가 직접 웹 페이지의 스택을 통해 이동 하도록 허용 하는 사용자에 게 허용 합니다. 또한도 제공 사용자 예상된 웹 페이지에 더는 기능을 제공 하는 바 주소를 사용 합니다. 주소 표시줄에 url을 변경할 수가 없도록 합니다. 

이러한 구현은 변경할 수 없으며, 따라서 `SFSafariViewController` 는 앱에서 사용자 지정 하지 않고 웹 페이지를 제공 하려는 경우 기본 브라우저로 사용 하기에 적합 합니다.

아래 코드를 사용 하 여을 시작할 수 있습니다는 `SFSafariViewController` Xamarin.iOS 앱에서:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

다음 웹 보기를 생성합니다.

[ ![](uiwebview-images/sfsafariviewcontroller.png "SFSafariViewController로는 예제 웹 보기")](uiwebview-images/sfsafariviewcontroller.png)

## <a name="safari"></a>Safari

아래 코드를 사용 하 여 응용 프로그램 내에서 모바일 Safari 앱을 열 수 이기도 합니다.

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

다음 웹 보기를 생성합니다.

[ ![](uiwebview-images/safari.png "Safari에 제공 된 웹 페이지")](uiwebview-images/safari.png)

Safari로 응용 프로그램에서 사용자가 탐색 일반적으로 항상 사용 하지 않아야 합니다. 대부분 사용자가 응용 프로그램 외부의 탐색을 기대 하지 앱 벗어나 탐색 하면 사용자가 돌아가지 않을 수 있습니다, 기본적으로 서비스를 중지 하면 되므로 합니다.

iOS 9 향상 된 기능을 Safari 페이지의 왼쪽된 위 모퉁이에서 제공 되는 뒤로 단추를 통해 응용 프로그램으로 쉽게 되돌릴 수 있습니다.

## <a name="app-transport-security"></a>앱의 전송 보안

앱 전송 보안 또는 *AT* Apple에서 iOS 9 모든 인터넷 통신 보안 모범 사례를 연결 하는에 맞도록에서 도입 되었습니다.

AT에 대 한 자세한 내용은를 참조 응용 프로그램에서 구현 하는 방법을 비롯 하 여 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 가이드입니다.

## <a name="related-links"></a>관련 링크

- [웹 보기로 (샘플)](https://developer.xamarin.com/samples/monotouch/WebView/)
