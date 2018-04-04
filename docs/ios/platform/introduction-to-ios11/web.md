---
title: IOS 11에서에서 웹 변경
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/12/2016
ms.openlocfilehash: 5cbf1d2f6c7b8a110cb65cad81df18f9f0568fda
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="web-changes-in-ios-11"></a>IOS 11에서에서 웹 변경

iOS 11 WebKit 및 SafariServices 변경 되어 Safari 웹 브라우저 – Safari 11.0 –의 새 버전을 소개 합니다. 이 가이드에는 이러한 변경 내용을 탐색합니다.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` 웹 콘텐츠 표시 또는 앱에서 사용자를 인증에 대 한 옵션으로 iOS 9에서에서 도입 되었습니다. 해당 기능에 대 한 자세한 정보를 찾을 수는 [웹 보기](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) 가이드입니다.

사용자는 웹 및 응용 프로그램 간에 보다 원활한 환경을 제공 하는 Safari 보기 컨트롤러에 iOS 11에 스타일 업데이트가 도입 되었습니다. 예를 들어 주소 지금 제공 Safari 뷰 컨트롤러 미니 브라우저가 아닌 앱 내 브라우저의 느낌 표시줄의 제거 합니다. 설정 하 여 응용 프로그램의 색 구성표를 맞추는 색 구성표를 사용자 지정할 수도 있습니다는 `preferredBarTintColor` 및 `PreferredControlTintColor` 속성:

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

다음 코드 조각은 다음 그림에 표시 된 대로 자주색 및 흰색에서 막대를 렌더링 합니다.

![자주색 흰색 렌더링 SFSafariViewController 막대](web-images/image1.png)

Safari 뷰 컨트롤러에 해제 단추를 설정 하 여 변경할 수도 있습니다는 `DismissButtonStyle` 속성을 `Done`, `Close`, 또는 `Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![변경 단추 텍스트를 해제 합니다.](web-images/image2.png)

이 값을 변경 하는 동안 `SFSafariViewController` 표시 됩니다.


Safari 뷰 컨트롤러 내에 표시 되는 콘텐츠에 따라 메뉴 모음 사용자 스크롤으로 축소 하지 않습니다 확인 해야 할 수 있습니다. 이 기능은 새 설정으로 사용 `BarCollapsedEnabled` 속성을 `false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![사용 하지 않도록 설정 축소 막대](web-images/image3.png)

Apple가 또한 11 ios에서 Safari 뷰 컨트롤러의 개인 정보를 업데이트를 만들었습니다. 이제, 쿠키 및 로컬 저장소에만 존재 앱 별 무휴로 아닌 Safari 뷰-컨트롤러의 모든 인 스턴에서와 같은 데이터를 검색 합니다. 이렇게 하면 검색 활동 사용자는 앱 내에서 개인있지 않습니다.

추가 기능 등을 끌어서 Url에 대 한 지원을 놓은 대 한 지원 `window.open()` 에 추가 `SFSafariViewController` ios 11. 이러한 새 기능에 대 한 자세한 정보를 찾을 수 [Apple의 SFSafariViewController 설명서](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor)합니다.


## <a name="webkit"></a>WebKit

`WKWebView` 사용자 웹 콘텐츠를 표시 하는 방법으로 iOS 8에에서 WebKit의 일부로 도입 되었습니다. 보다 훨씬 더 사용자 지정 가능한은 `SFSafariViewController`, 탐색 및 사용자 인터페이스를 직접 만들 수 있습니다.

Apple에 대 한 세 가지 주요 향상 기능을 도입 `WKWebView` ios 11: 

- 쿠키를 관리 하는 기능
- 콘텐츠 필터링
- 사용자 지정 리소스 로드 합니다. 

쿠키 관리 새 통해 수행 되며 [ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) 클래스를 추가 하 고는 WKWebView에 저장 된 모든 쿠키 하 고 변경 내용을 저장 하는 쿠키를 관찰 하는 쿠키를 삭제 합니다.

콘텐츠 필터링을 사용 하면 사용자에 게 표시할 콘텐츠의 형식을 관리할 수 있습니다 있으므로 안전 하 고, 패밀리에 친숙 하며, 인지 하 고 필요한 경우 사용자의 선택 그룹에만 사용할 수입니다. 이 새로운 구현 [ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist) 쌍 트리거 및 JSON의 동작을 제공 하 여 클래스입니다. 이러한 트리거 및 작업에 대 한 자세한 내용은 Apple에 있습니다 [콘텐츠 차단 규칙](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) 가이드입니다.

이제 iOS 11은 사용자 지정할 수 있습니다 `WKWebView` 웹 콘텐츠를 로드 하는 사용자 지정 리소스를 사용 합니다. 이 통해 구현 됩니다는 `IWKUrlSchemeHandler` 인터페이스 웹 키트에 네이티브 하지 않은 URL 스키마를 처리할 수 있습니다. 이 인터페이스를 구현 해야 하는 시작 및 중지 메서드를 있습니다.

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

처리기 구현 된 후 설정 하려면 사용 된 `SetUrlSchemeHandler` 속성에는 `WKWebViewConfiguration`합니다. 그런 다음 로드할 사용자 지정 체계를 사용 하는 특정 항목의 URL:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

