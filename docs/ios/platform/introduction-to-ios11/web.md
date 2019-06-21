---
title: IOS 11에서에서 WebKit 및 Safari 변경
description: 이 문서는 WebKit 및 iOS 11의에서 Safari 서비스 프레임 워크 변경 내용을 설명 합니다. 필요한 SFSafariViewController의 업데이트 및 WKWebView의 새로운 기능 스타일 지정을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/12/2017
ms.openlocfilehash: 5ced73b1f3f5b8207ae1258dcb01a78c94df217d
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67268923"
---
# <a name="webkit-and-safari-changes-in-ios-11"></a>IOS 11에서에서 WebKit 및 Safari 변경

iOS 11 WebKit 및 SafariServices 변경 내용을 포함 하는 Safari 웹 브라우저-Safari 11.0 –의 새 버전을 소개 합니다. 이 가이드에서는 이러한 변경 내용을 살펴봅니다.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` 웹 콘텐츠를 표시 하거나 앱에서 사용자 인증 옵션으로 iOS 9에서에서 도입 되었습니다. 해당 기능에 대 한 자세한 정보를 찾을 수 있습니다 합니다 [웹 보기](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) 가이드입니다.

iOS 11을 사용자가 앱 및 웹 간에 더 원활한 환경을 제공 Safari 보기 컨트롤러에 스타일 업데이트 도입 되었습니다. 예를 들어 주소 표시줄 지금 제공 Safari 뷰 컨트롤러의 최소 브라우저가 아닌 앱에서 브라우저를 사용 한다는 느낌의 제거 합니다. 설정 하 여 앱의 색 구성표를 사용 하 여 맞게 색 구성표를 사용자 지정할 수도 있습니다는 `preferredBarTintColor` 고 `PreferredControlTintColor` 속성:

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

다음 코드 조각은 다음 이미지에 표시 된 대로 자주색 및 흰색에서 해당 막대를 렌더링 합니다.

![자주색 녹색과 흰색으로 렌더링 하는 필요한 SFSafariViewController 막대](web-images/image1.png)

Safari 뷰 컨트롤러 나오는 해제 단추를 설정 하 여 변경할 수도 있습니다는 `DismissButtonStyle` 속성을 `Done`하십시오 `Close`, 또는 `Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![변경 단추 텍스트를 해제 합니다.](web-images/image2.png)

이 값을 변경할 수 있지만 `SFSafariViewController` 표시 됩니다.


Safari 뷰 컨트롤러 내에서 표시 되는 내용에 따라 메뉴 모음까지 스크롤하면으로 축소 하지는 확인 해야 할 수도 있습니다. 새 설정 하 여 활성화 됩니다 `BarCollapsedEnabled` 속성을 `false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![사용 하지 않도록 설정 축소 막대](web-images/image3.png)

Apple는 iOS 11에서에서 Safari 뷰 컨트롤러의 개인 정보를 업데이트도 했습니다. 이제 앱 별 기준으로 아닌 Safari 뷰 컨트롤러의 모든 인스턴스 간에 존재 쿠키 및 로컬 저장소와 같은 데이터를 검색 합니다. 이렇게 하면 사용자 검색 작업 앱 내의 개인 있습니다.

추가 기능 등 Url에 대 한 지원을 끌어서에 대 한 지원 `window.open()` 에 추가 되었습니다 `SFSafariViewController` iOS 11의에서 합니다. 이러한 새 기능에 대 한 자세한 정보를 찾을 수 있습니다 [Apple 필요한 SFSafariViewController 설명서](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor)합니다.


## <a name="webkit"></a>WebKit

`WKWebView` 사용자에 게 웹 콘텐츠를 표시 하는 수단으로 ios 8 WebKit의 일부로 도입 되었습니다. 것 보다 훨씬 더 사용자 지정 가능한 `SFSafariViewController`, 탐색 및 사용자 인터페이스를 직접 만들 수 있습니다.

Apple의 세 가지 주요 향상 했습니다 `WKWebView` iOS 11 사용 하 여: 

- 쿠키를 관리 하는 기능
- 콘텐츠 필터링
- 사용자 지정 리소스를 로드 합니다. 

쿠키 관리 새로운 이루어집니다 [ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) 클래스를 추가 및 삭제할 쿠키를 WKWebView에 저장 된 모든 쿠키를 얻으려면 하 고 변경 내용을 저장 하는 쿠키를 확인할 수 있습니다.

콘텐츠 필터링을 사용 하면 사용자에 게 표시 되는 콘텐츠의 형식을 관리할 수 있습니다 것이 안전 하 고 제품군 잘 작동 되도록 할 수 있도록 하 고, 필요한 경우 사용자의 선택 그룹에만 사용할 수 있습니다. 이 새로운 구현 [ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist) 클래스, 트리거 및 작업 json에서의 쌍을 제공 합니다. 이러한 트리거 및 작업에 대 한 자세한 내용은 Apple에서 찾을 수 있습니다 [콘텐츠 차단 규칙](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) 가이드입니다.

이제 iOS 11은 사용자 지정할 수 있습니다 `WKWebView` 웹 콘텐츠에 대 한 로드 사용자 지정 리소스를 사용 하 여 합니다. 이 통해 구현 되는 `IWKUrlSchemeHandler` 인터페이스를 웹 키트 네이티브 되지 않는 URL 체계를 처리할 수 있습니다. 이 인터페이스에 start 및 stop 메서드를 구현 해야 합니다.

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

처리기를 구현한 설정 하려면 사용 합니다 `SetUrlSchemeHandler` 속성에는 `WKWebViewConfiguration`합니다. 그런 다음 사용자 지정 체계를 사용 하는 항목의 URL을 로드:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

