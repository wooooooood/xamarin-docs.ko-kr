---
title: IOS 11의 WebKit 및 Safari 변경 내용
description: 이 문서에서는 WebKit 및 iOS 11의 Safari 서비스 프레임 워크에 대 한 변경 내용을 설명 합니다. SFSafariViewController 및 WKWebView의 새로운 기능에서 스타일 업데이트를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/12/2017
ms.openlocfilehash: 6068dd148bfc3c2a778ca34753374bcecccb55d9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70752226"
---
# <a name="webkit-and-safari-changes-in-ios-11"></a>IOS 11의 WebKit 및 Safari 변경 내용

iOS 11에는 WebKit 및 SafariServices에 대 한 변경 내용이 포함 된 safari 웹 브라우저의 새 버전 (Safari 11.0)이 도입 되었습니다. 이 가이드는 이러한 변경 내용을 살펴봅니다.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController`는 웹 콘텐츠를 표시 하거나 앱에서 사용자를 인증 하는 옵션으로 iOS 9에서 도입 되었습니다. 해당 기능에 대 한 자세한 내용은 [웹 보기](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) 가이드에서 찾을 수 있습니다.

iOS 11은 Safari 보기 컨트롤러에 대 한 스타일 업데이트를 도입 하 여 사용자에 게 앱과 웹 간의 원활한 환경을 제공 합니다. 예를 들어 주소 표시줄을 제거 하면 이제 미니 브라우저가 아닌 앱 내 브라우저의 느낌이 Safari 보기 컨트롤러에 제공 됩니다. `preferredBarTintColor` 및`PreferredControlTintColor` 속성을 설정 하 여 응용 프로그램의 색 구성표에 맞게 색 구성표를 사용자 지정할 수도 있습니다.

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

다음 코드 조각에서는 다음 이미지에 표시 된 것 처럼 막대를 자주색 및 흰색으로 렌더링 합니다.

![자주색 및 흰색으로 렌더링 된 SFSafariViewController 막대](web-images/image1.png)

Safari 뷰 컨트롤러에 표시 되는 `DismissButtonStyle` 해제 단추는 속성을 `Done`, `Close`또는 `Cancel`로 설정 하 여 변경할 수도 있습니다.

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![해제 단추 텍스트 변경](web-images/image2.png)

가 표시 되는 동안 `SFSafariViewController` 이 값을 변경할 수 있습니다.

Safari 뷰 컨트롤러 내에 표시 되는 콘텐츠에 따라 사용자가 스크롤하면 메뉴 모음이 축소 되지 않도록 해야 할 수도 있습니다. 새 `BarCollapsedEnabled` 속성을로 `false`설정 하 여이 작업을 수행할 수 있습니다.

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![막대 축소 사용 안 함](web-images/image3.png)

또한 Apple은 iOS 11의 Safari 보기 컨트롤러에서 개인 정보를 업데이트 했습니다. 이제 쿠키 및 로컬 저장소와 같은 데이터 검색은 Safari 뷰 컨트롤러의 모든 인스턴스가 아닌 앱 별로 존재 합니다. 그러면 앱 내에서 사용자 검색 작업을 비공개로 유지 합니다.

Url에 대 한 끌어서 놓기 지원 및에 대 `window.open()` 한 지원 등의 추가 기능도 iOS 11에서에 `SFSafariViewController` 추가 되었습니다. 이러한 새로운 기능에 대 한 자세한 내용은 [Apple의 SFSafariViewController 설명서](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor)에서 확인할 수 있습니다.

## <a name="webkit"></a>WebKit

`WKWebView`는 사용자에 게 웹 콘텐츠를 표시 하는 수단으로 iOS 8의 WebKit 일부로 도입 되었습니다. 보다 `SFSafariViewController`사용자 지정이 훨씬 더 사용자 지정 되므로 사용자가 직접 탐색 및 사용자 인터페이스를 만들 수 있습니다.

Apple은 iOS 11에 대 한 `WKWebView` 세 가지 주요 개선 사항을 도입 했습니다. 

- 쿠키를 관리 하는 기능
- 콘텐츠 필터링
- 사용자 지정 리소스 로드 

쿠키 관리는 새 [`WKHttpCookieStore`](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) 클래스를 통해 수행 됩니다 .이 클래스를 사용 하 여 쿠키를 추가 및 삭제 하 고, WKWebView에 저장 된 모든 쿠키를 가져오고, 쿠키 저장소에서 변경 내용을 관찰할 수 있습니다.

콘텐츠 필터링을 사용 하면 사용자에 게 표시 되는 콘텐츠 형식을 관리 하 여 안전 하 고 잘 이해할 수 있으며, 필요한 경우 선택 된 사용자 그룹에만 사용할 수 있습니다. JSON에서 트리거와 작업 쌍을 [`WKContentRuleList`](https://developer.apple.com/documentation/webkit/wkcontentrulelist) 제공 하 여 새 클래스를 통해 구현 됩니다. 이러한 트리거 및 작업에 대 한 자세한 내용은 Apple의 [콘텐츠 차단 규칙](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) 가이드에서 찾을 수 있습니다.

이제 iOS 11에서 웹 콘텐츠에 대 `WKWebView` 한 사용자 지정 리소스 로드를 사용 하 여 사용자 지정할 수 있습니다. 이는 웹 키트의 `IWKUrlSchemeHandler` 기본이 아닌 URL 체계를 처리할 수 있는 인터페이스를 통해 구현 됩니다. 이 인터페이스에는 구현 해야 하는 시작 및 중지 메서드가 있습니다.

```csharp
public class MyHandler : NSObject, IWKUrlSchemeHandler {

    [Export("webView:startURLSchemeTask:")]
    public void StartUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        
        // Implement a IWKUrlSchemeTask here
        var response = new NSUrlResponse(urlSchemeTask.Request.Url, "text/html", ContentLength, null);
        urlSchemeTask.DidReceiveResponse(response);
        urlSchemeTask.DidReceiveData(someData);
        urlSchemeTask.DidFinish();
    }

    [Export("webView:stopURLSchemeTask:")]
    public void StopUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        throw new NotImplementedException();
    }

}
``` 

처리기가 구현 되 면이를 사용 하 여 `SetUrlSchemeHandler` `WKWebViewConfiguration`에 대 한 속성을 설정 합니다. 그런 다음 사용자 지정 스키마를 사용 하는 항목의 URL을 로드 합니다.

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```
