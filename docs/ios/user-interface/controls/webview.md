---
title: 자마린.iOS의 웹 뷰
description: 이 문서에서는 Xamarin.iOS 앱이 웹 콘텐츠를 표시할 수 있는 다양한 방법에 대해 설명합니다. WKWebView, SFSafariViewController, 사파리 및 앱 전송 보안에 대해 설명합니다.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: cc6c87452bf5312d66e33b7c49d3dc216eceb7d6
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628291"
---
# <a name="web-views-in-xamarinios"></a>자마린.iOS의 웹 뷰

iOS Apple은 앱 개발자가 앱에 웹 보기 기능을 통합할 수 있는 여러 가지 방법을 출시했습니다. 대부분의 사용자는 iOS 장치에서 기본 제공 Safari 웹 브라우저를 사용하므로 다른 앱의 웹 보기 기능이 이 환경과 일치할 것으로 예상합니다. 동일한 제스처가 작동하고 성능이 파에 표시되며 기능이 동일하기를 기대합니다.

iOS 11은 모두와 `WKWebView` `SFSafariViewController`에 새로운 변경 사항을 도입했다. 이에 대한 자세한 내용은 [iOS 11 가이드의 웹 변경 사항을](~/ios/platform/introduction-to-ios11/web.md) 참조하세요.

## <a name="wkwebview"></a>WK웹뷰

`WKWebView`iOS 8에서 앱 개발자가 모바일 Safari와 유사한 웹 브라우징 인터페이스를 구현할 수 있도록 했습니다. 이것은 부분적으로, 니트로 자바 스크립트 `WKWebView` 엔진을 사용 한다는 사실에, 모바일 사파리에서 사용 하는 동일한 엔진. `WKWebView`성능 향상, 사용자 친화적인 제스처 내장, 웹 페이지와 앱 간의 상호 작용 용이성 으로 인해 가능한 경우 UIWebView를 통해 항상 사용해야 합니다.

`WKWebView`UIWebView와 거의 동일한 방식으로 앱에 추가할 수 있지만 개발자는 UI/UX 및 기능을 훨씬 더 많이 제어할 수 있습니다. 웹 뷰 개체를 만들고 표시하면 요청된 페이지가 표시되지만 뷰표시 방법, 사용자가 탐색하는 방법 및 사용자가 뷰를 종료하는 방법을 제어할 수 있습니다.  

아래 코드는 Xamarin.iOS 앱에서 를 `WKWebView` 시작하는 데 사용할 수 있습니다.

```csharp
WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
View.AddSubview(webView);

var url = new NSUrl("https://docs.microsoft.com");
var request = new NSUrlRequest(url);
webView.LoadRequest(request);
```

네임스페이스에 있으므로 `WKWebView` 클래스의 맨 위에 지시문을 사용하여 이 것을 추가해야 합니다. `WebKit`

`WKWebView`Xamarin.Mac 앱에서도 사용할 수 있으며 크로스 플랫폼 Mac/iOS 앱을 만드는 경우 사용해야 합니다.

[자바스크립트 경고 처리](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) 레시피는 자바스크립트와 함께 WKWebView 사용에 대한 정보도 제공합니다.

## <a name="sfsafariviewcontroller"></a>SF사파리뷰컨트롤러

 `SFSafariViewController`는 앱에서 웹 콘텐츠를 제공하는 최신 방법이며 iOS 9 이상에서 사용할 수 있습니다. 또는 `UIWebView` `WKWebView`다른 `SFSafariViewController` 뷰컨트롤러이므로 다른 뷰와 함께 사용할 수 없습니다. 뷰 컨트롤러를 표시하는 것과 동일한 방식으로 새 뷰 `SFSafariViewController` 컨트롤러로 표시해야 합니다.

 `SFSafariViewController`본질적으로 앱에 포함될 수 있는 '미니 사파리'입니다. WKWebView처럼 그것은 동일한 니트로 자바 스크립트 엔진을 사용하지만, 또한 자동 채우기, 리더, 모바일 사파리와 쿠키와 데이터를 공유 할 수있는 기능과 같은 추가 사파리 기능의 범위를 제공합니다. 사용자와 사용자 간의 `SFSafariViewController` 상호 작용은 앱에서 액세스할 수 없습니다. 앱은 기본 Safari 기능에 액세스할 수 없습니다.

또한 기본적으로 **완료** 단추를 구현하여 사용자가 앱으로 쉽게 돌아갈 수 있도록 하고, 앞으로 및 뒤로 탐색 단추를 사용하여 사용자가 웹 페이지 스택을 탐색할 수 있도록 합니다. 또한 사용자에게 예상되는 웹 페이지에 있는 안심할 수 있는 주소 표시줄을 제공합니다. 주소 표시줄은 사용자가 URL을 변경할 수 없습니다.

이러한 구현은 변경할 수 `SFSafariViewController` 없으므로 앱에서 사용자 지정 없이 웹 페이지를 표시하려는 경우 기본 브라우저로 사용하는 것이 좋습니다.

아래 코드는 Xamarin.iOS 앱에서 를 `SFSafariViewController` 시작하는 데 사용할 수 있습니다.

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

이렇게 하면 다음과 같은 웹 보기가 생성됩니다.

[![SFSafariViewController를 가진 예제 웹 보기](webview-images/sfsafariviewcontroller.png)](webview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

아래 코드를 사용하여 앱 내에서 모바일 Safari 앱을 열 수도 있습니다.

```csharp
var url = new NSUrl("https://docs.microsoft.com");

UIApplication.SharedApplication.OpenUrl(url);
```

이렇게 하면 다음과 같은 웹 보기가 생성됩니다.

[![Safari에 표시되는 웹 페이지](webview-images/safari.png)](webview-images/safari.png#lightbox)

앱에서 Safari로 사용자를 탐색하는 것은 일반적으로 항상 피해야 합니다. 대부분의 사용자는 응용 프로그램 외부에서 탐색을 기대하지 않으므로 앱에서 멀리 이동하면 사용자가 앱을 반환하지 않아 본질적으로 참여가 사라질 수 있습니다.

iOS 9 의 향상된 기능을 통해 사용자는 Safari 페이지의 왼쪽 상단 모서리에 제공되는 뒤로 버튼을 통해 앱으로 쉽게 돌아갈 수 있습니다.

## <a name="app-transport-security"></a>앱 전송 보안

앱 전송 보안 또는 *ATS는* 모든 인터넷 통신이 보안 연결 모범 사례를 준수하도록 iOS 9에서 Apple에 의해 도입되었습니다.

앱에서 구현하는 방법을 포함하여 ATS에 대한 자세한 내용은 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 가이드를 참조하십시오.

## <a name="uiwebview-deprecation"></a>UIWebView 사용 중단

`UIWebView`는 앱에서 웹 콘텐츠를 제공하는 Apple의 레거시 방법입니다. iOS 2.0에서 출시되었으며 8.0으로 더 이상 사용되지 않습니다.

> [!IMPORTANT]
> `UIWebView`는 사용되지 않습니다. 이 컨트롤을 사용하는 새 앱은 [2020년 4월 현재 App Store에 허용되지 않으며 이 컨트롤을 사용하는 앱 업데이트는 2020년 12월까지 허용되지 않습니다.](https://developer.apple.com/news/?id=12232019b)
>
> [Apple의 `UIWebView` 설명서에](https://developer.apple.com/documentation/uikit/uiwebview) 따르면 [`WKWebView`](#wkwebview) 앱은 대신 사용해야 합니다.
>
> Xamarin.Forms를 사용하는 동안 `UIWebView` 사용 중단 경고(ITMS-90809)와 관련된 리소스를 찾는 경우 [Xamarin.Forms WebView](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809) 설명서를 참조하세요.

지난 6개월 동안 iOS 애플리케이션을 제출한 개발자는 App Store에서 더 이상 사용되지 `UIWebView` 않는 것에 대한 경고를 받았을 수 있습니다.

API의 사용 중단은 일반적입니다. Xamarin.iOS는 사용자 지정 특성을 사용하여 해당 API에 신호를 지정하고 사용 가능한 경우 대체 기능을 개발자에게 다시 제안합니다. 이번에는 다른 점과 훨씬 덜 일반적이지만 제출 시 Apple의 App Store에서 사용 중단이 **적용된다는** 것입니다.

안타깝게도 `UIWebView` 형식을 제거하는 `Xamarin.iOS.dll` 것은 [이진 변경 입니다.](https://docs.microsoft.com/dotnet/core/compatibility/categories#binary-compatibility) 이 변경은 지원되지 않거나 더 이상 다시 컴파일할 수 있는 라이브러리(예: 닫힌 소스)를 포함하여 기존 제3자 라이브러리를 중단합니다. 이렇게 하면 개발자에게만 추가 문제가 발생합니다. 따라서 *아직*형식을 제거 하지 않습니다.

[Xamarin.iOS 13.16에서](https://docs.microsoft.com/xamarin/ios/release-notes/13/13.16) 마이그레이션하는 데 도움이 되는 새로운 `UIWebView`검색 및 도구를 사용할 수 있습니다.

### <a name="detection"></a>감지

최근에 apple App Store에 iOS 응용 프로그램을 제출한 경우 이 상황이 응용 프로그램에 적용되는지 궁금할 수 있습니다.

알아내기 위해 프로젝트의 `--warn-on-type-ref=UIKit.UIWebView` **추가 mtouch 인수에** 추가할 수 있습니다. 이렇게 하면 응용 프로그램 `UIWebView` **내부의** 더 이상 사용되지 않는 참조(및 모든 종속성)에 대한 참조가 경고됩니다. 관리되는 링커가 **실행되기 전과** **후에** 형식을 보고하는 데 다른 경고가 사용됩니다.

다른 경고와 마찬가지로 을 사용하여 `-warnaserror:`오류로 전환할 수 있습니다. 이 기능은 확인 후 새 종속성이 `UIWebView` 추가되지 않도록 하려는 경우에 유용할 수 있습니다. 다음은 그 예입니다.

* `-warnaserror:1502`참조가 미리 연결된 어셈블리에서 발견되면 오류를 보고합니다.
* `-warnaserror:1503`참조가 링크된 후 어셈블리에서 발견되면 오류를 보고합니다.

사전/사후 연결 결과가 유용하지 않은 경우 경고를 무음으로 지정할 수도 있습니다. 다음은 그 예입니다.

* `-nowarn:1502`참조가 미리 연결된 어셈블리에서 발견되면 경고를 **보고하지 않습니다.**
* `-nowarn:1503`연결된 후 어셈블리에서 참조가 발견되면 경고를 **보고하지 않습니다.**

### <a name="removal"></a>제거

모든 응용 프로그램은 고유합니다. 응용 `UIWebView` 프로그램에서 제거하려면 사용 방법과 위치에 따라 다른 단계가 필요할 수 있습니다. 가장 일반적인 시나리오는 다음과 같습니다.

- 응용 프로그램 내부를 `UIWebView` 사용할 수 없습니다. 모든 것이 괜찮습니다. AppStore에 제출할 때 경고가 **없어야** 합니다. 다른 것은 당신에게서 필요하지 않습니다.
- 응용 프로그램에 `UIWebView` 의해 직접 사용. 예를 들어 `UIWebView`을 제거하여 `WKWebView` 최신(iOS 8) 또는 `SFSafariViewController` (iOS 9) 유형으로 바꿉니다. 이 작업이 완료되면 관리링커에 대한 참조가 `UIWebView` 표시되지 않으며 최종 앱 바이너리가 추적되지 않습니다.
- 간접 사용. `UIWebView`응용 프로그램에서 사용하는 관리 또는 네이티브 중 일부 타사 라이브러리에 존재할 수 있습니다. 이 상황이 최신 릴리스에서 이미 해결될 수 있으므로 외부 종속성을 최신 버전으로 업데이트하여 시작합니다. 그렇지 않은 경우 라이브러리의 메인테이너에게 문의하여 업데이트 계획에 대해 문의하십시오.

또는 다음 방법을 시도해 볼 수 있습니다.

1. **Xamarin.Forms를**사용하는 경우 이 [블로그 게시물을](https://devblogs.microsoft.com/xamarin/uiwebview-deprecation-xamarin-forms/)읽어보십시오.
1. 관리되는 링커(전체 프로젝트 또는 적어도 사용 `UIWebView`종속성)를 사용하도록 설정하여 참조하지 않은 경우 제거될 수 *있습니다.* 이 문제는 해결되지만 코드 링커를 안전하게 만들기 위해 추가 작업이 필요할 수 있습니다.
1. 관리되는 링커 설정을 변경할 수 없는 경우 아래의 특수 한 경우를 참조 하십시오.

#### <a name="applications-cannot-use-the-linker-or-change-its-settings"></a>응용 프로그램은 링커를 사용할 수 없습니다(또는 해당 설정을 변경)

어떤 이유로 관리링커를 사용하지 **않는** 경우(예: **링크 안 함)** `UIWebView` Apple에 제출한 바이너리 앱에 기호가 남아 있고 거부될 수 있습니다.

*강력한* 해결책은 프로젝트의 `--optimize=force-rejected-types-removal` **추가 mtouch 인수에**추가하는 것입니다. 이렇게 하면 응용 `UIWebView` 프로그램에서 추적이 제거됩니다. 그러나 형식을 참조하는 코드는 제대로 작동하지 **않습니다(예외** 또는 충돌 예상). 이 방법은 코드에 연결할 수 없는 경우(정적 분석을 통해 연결할 수 있는 경우에도) 사용해야 합니다.

#### <a name="support-for-ios-7x-or-earlier"></a>iOS 7.x(또는 이전) 지원

`UIWebView`v2.0 이후 iOS의 일부가 되었습니다. 가장 일반적인 대체품은 `WKWebView` (iOS 8) 및 `SFSafariViewController` (iOS 9)입니다. 응용 프로그램이 여전히 이전 iOS 버전을 지원하는 경우 다음 옵션을 고려해야 합니다.

* iOS 8을 최소 대상 버전(빌드 시간 결정)으로 만듭니다.
* 앱이 `WKWebView` iOS 8+ (런타임 결정)에서 실행되는 경우에만 사용합니다.

#### <a name="applications-not-submitted-to-apple"></a>Apple에 제출되지 않은 신청서

응용 프로그램이 Apple에 제출되지 않은 경우 향후 iOS 릴리스에서 제거할 수 있으므로 더 이상 사용되지 않는 API에서 벗어나도록 계획해야 합니다. 그러나 사용자 고유의 시간표를 사용하여 이 전환을 수행할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [웹 뷰(샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
