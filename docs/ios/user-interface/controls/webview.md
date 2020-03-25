---
title: Xamarin.ios의 웹 보기
description: 이 문서에서는 Xamarin.ios 앱이 웹 콘텐츠를 표시 하는 다양 한 방법에 대해 설명 합니다. WKWebView, SFSafariViewController, Safari 및 앱 전송 보안에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 7c469a011a70840cfe94a7f87ed77f03968a3525
ms.sourcegitcommit: ec112800a76089ab1db66fe24b8bbcc510e067b4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80159823"
---
# <a name="web-views-in-xamarinios"></a>Xamarin.ios의 웹 보기

IOS Apple의 수명 동안 앱 개발자가 앱에서 웹 보기 기능을 통합 하는 다양 한 방법을 출시 했습니다. 대부분의 사용자는 iOS 장치에서 기본 제공 Safari 웹 브라우저를 활용 하므로 다른 앱의 웹 보기 기능이이 환경과 일치 하는 것으로 간주 됩니다. 동일 하 게 작동 하는 것으로 간주 되 고 성능이 동일 하 고 기능이 동일 하 게 됩니다.

iOS 11에는 `WKWebView` 및 `SFSafariViewController`에 대 한 새로운 변경 사항이 도입 되었습니다. 이러한 항목에 대 한 자세한 내용은 [iOS 11의 웹 변경 가이드](~/ios/platform/introduction-to-ios11/web.md) 가이드를 참조 하세요.

## <a name="wkwebview"></a>WKWebView

앱 개발자가 모바일 Safari와 유사한 웹 검색 인터페이스를 구현할 수 있도록 iOS 8에서 도입 된 `WKWebView`. 이는 부분적으로 모바일 Safari에서 사용 하는 것과 동일한 엔진인 Nitro Javascript 엔진을 사용 하는 `WKWebView`입니다. `WKWebView`은 향상 된 성능, 사용자에 게 친숙 한 제스처 및 웹 페이지와 앱 간의 상호 작용 용이성으로 인해 가능 하면 항상 UIWebView 보기를 통해 사용 해야 합니다.

`WKWebView`는 UIWebView 보기와 거의 동일한 방식으로 앱에 추가할 수 있지만, 개발자는 UI/UX 및 기능을 훨씬 더 많이 제어할 수 있습니다. 웹 뷰 개체를 만들고 표시 하면 요청 된 페이지가 표시 됩니다. 그러나 보기가 표시 되는 방법, 사용자가 탐색 하는 방법 및 사용자가 보기를 종료 하는 방법을 제어할 수 있습니다.  

아래 코드를 사용 하 여 Xamarin.ios 앱에서 `WKWebView`를 시작할 수 있습니다.

```csharp
WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
View.AddSubview(webView);

var url = new NSUrl("https://docs.microsoft.com");
var request = new NSUrlRequest(url);
webView.LoadRequest(request);
```

`WKWebView` `WebKit` 네임 스페이스에 있으므로이 using 지시문을 클래스의 맨 위에 추가 해야 합니다.

`WKWebView`은 Xamarin.ios 앱 에서도 사용할 수 있으며 플랫폼 간 Mac/iOS 앱을 만드는 경우 사용 해야 합니다.

[Javascript 경고 처리](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) 조리법은 javascript와 함께 WKWebView를 사용 하는 방법에 대 한 정보도 제공 합니다.

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

[SFSafariViewController를 사용 하 여 예제 웹 보기 ![](webview-images/sfsafariviewcontroller.png)](webview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

아래 코드를 사용 하 여 앱 내에서 모바일 Safari 앱을 열 수도 있습니다.

```csharp
var url = new NSUrl("https://docs.microsoft.com");

UIApplication.SharedApplication.OpenUrl(url);
```

그러면 다음과 같은 웹 보기가 생성 됩니다.

[Safari에 표시 되는 웹 페이지 ![](webview-images/safari.png)](webview-images/safari.png#lightbox)

사용자를 앱에서 Safari로 이동 하는 것은 일반적으로 항상 피해 야 합니다. 대부분의 사용자는 응용 프로그램 외부에서 탐색을 필요로 하지 않으므로 앱에서 벗어나면 사용자는이를 반환 하지 않을 수 있으며,이는 기본적으로 참여를 중단 합니다.

iOS 9의 향상 된 기능을 통해 사용자는 Safari 페이지의 왼쪽 위 모서리에 있는 뒤로 단추를 통해 쉽게 앱으로 돌아갈 수 있습니다.

## <a name="app-transport-security"></a>앱 전송 보안

앱 전송 보안 또는 *ATS* 는 모든 인터넷 통신이 보안 연결 모범 사례를 준수 하도록 iOS 9의 Apple에서 도입 되었습니다.

앱에서 응용 프로그램을 구현 하는 방법을 비롯 하 여 ATS에 대 한 자세한 내용은 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 가이드를 참조 하세요.

## <a name="uiwebview-deprecation"></a>UIWebView 보기 사용 중단

`UIWebView` 앱에서 웹 콘텐츠를 제공 하는 Apple의 레거시 방법입니다. IOS 2.0에 출시 되었으며 8.0부터 사용 되지 않습니다.

> [!IMPORTANT]
> `UIWebView`는 사용되지 않습니다. 이 컨트롤을 사용 하는 새 앱은 [4 월 2020을 기준으로 앱 스토어에 허용 되지 않으며,이 컨트롤을 사용 하는 앱 업데이트는 12 월 2020에 허용 되지](https://developer.apple.com/news/?id=12232019b)않습니다.
>
> [Apple의 `UIWebView` 설명서](https://developer.apple.com/documentation/uikit/uiwebview) 에서는 대신 [`WKWebView`](#wkwebview) 를 사용 해야 합니다.
>
> Xamarin.Forms를 사용하는 동안 `UIWebView` 사용 중단 경고(ITMS-90809)와 관련된 리소스를 찾는 경우 [Xamarin.Forms WebView](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809) 설명서를 참조하세요.

지난 6 개월 동안 iOS 응용 프로그램을 제출한 개발자가 앱 스토어에서 경고를 수신 했을 수 있습니다 .이에 대 한 자세한 `UIWebView`는 사용 되지 않습니다.

결함 Api는 일반적입니다. Xamarin.ios는 사용자 지정 특성을 사용 하 여 해당 Api를 알리고 (사용 가능한 경우 대체를 제안) 개발자에 게 다시 제공 합니다. 이 시간은 서로 다르며, 일반적으로 사용 중단은 전송 시 Apple의 앱 스토어에서 **적용** 됩니다.

아쉽게도 `Xamarin.iOS.dll`에서 `UIWebView` 형식을 제거 하는 것은 [이진 주요 변경 사항](https://docs.microsoft.com/dotnet/core/compatibility/categories#binary-compatibility)입니다. 이렇게 변경 하면 지원 되지 않거나 더 이상 다시 컴파일할 수 없는 일부 (예: 닫힌 소스)를 비롯 하 여 기존 타사 라이브러리가 중단 됩니다. 이렇게 하면 개발자에 대 한 추가 문제도 발생 합니다. 따라서 *아직*형식을 제거 하지 않습니다.

[Xamarin.ios 13.16](https://docs.microsoft.com/xamarin/ios/release-notes/13/13.16) 부터는 `UIWebView`에서 마이그레이션하는 데 도움이 되는 새로운 검색 및 도구를 사용할 수 있습니다.

### <a name="detection"></a>감지

최근 iOS 응용 프로그램을 Apple 앱 스토어에 제출 have'nt이 상황이 응용 프로그램에 적용 되는지 궁금할 수 있습니다.

프로젝트의 **추가 mtouch 인수** 에 `--warn-on-type-ref=UIKit.UIWebView`를 추가 하 여 찾을 수 있습니다. 이렇게 하면 응용 프로그램 내에서 사용 되지 않는 `UIWebView` 및 모든 해당 종속성 **에 대 한 참조가 경고** 됩니다. 관리 되는 링커가 실행 **되기 전과** **후** 에 다양 한 경고를 사용 하 여 형식을 보고 합니다.

`-warnaserror:`를 사용 하 여 경고를 다른 오류로 설정할 수 있습니다. 이는 확인 후 `UIWebView`에 대 한 새 종속성이 추가 되지 않도록 하려는 경우에 유용할 수 있습니다. 다음은 그 예입니다.

* `-warnaserror:1502`는 미리 연결 된 어셈블리에서 참조를 찾을 경우 오류를 보고 합니다.
* 사후 연결 된 어셈블리에서 참조가 발견 되 면 `-warnaserror:1503`에서 오류를 보고 합니다.

사전/사후 링크 결과가 유용 하지 않을 경우 경고를 경고할 수도 있습니다. 다음은 그 예입니다.

* `-nowarn:1502`는 미리 연결 된 어셈블리에서 참조를 찾을 경우 경고를 보고 **하지 않습니다** .
* 사후 연결 된 어셈블리에서 참조를 찾은 경우에는 `-nowarn:1503`에서 경고를 보고 **하지** 않습니다.

### <a name="removal"></a>제거

모든 응용 프로그램은 고유 합니다. 응용 프로그램에서 `UIWebView`를 제거 하는 방법은 사용 방법과 위치에 따라 다른 단계가 필요할 수 있습니다. 가장 일반적인 시나리오는 다음과 같습니다.

- 응용 프로그램 내에 `UIWebView`을 사용 하지 않습니다. 모든 것이 정상입니다. AppStore에 제출할 때 경고가 **없어야 합니다.** 다른 항목은 필요 하지 않습니다.
- 응용 프로그램에서 `UIWebView`를 직접 사용 합니다. `UIWebView`사용을 제거 하 여 시작 합니다. 예를 들어 최신 `WKWebView` (iOS 8) 또는 `SFSafariViewController` (iOS 9) 형식으로 바꿉니다. 이 작업이 완료 되 면 관리 되는 링커는 `UIWebView`에 대 한 참조를 볼 수 없으며, 최종 응용 프로그램 이진 파일에는이를 추적 하지 않습니다.
- 간접 사용. `UIWebView`는 응용 프로그램에서 사용 되는 일부 타사 라이브러리 (관리 또는 네이티브)에 있을 수 있습니다. 최신 릴리스에서는이 상황이 이미 해결 되었을 수 있으므로 외부 종속성을 최신 버전으로 업데이트 하 여 시작 합니다. 그렇지 않은 경우 라이브러리의 유지 관리자에 게 문의 하 고 업데이트 계획을 요청 합니다.

또는 다음과 같은 방법을 시도해 볼 수 있습니다.

1. **Xamarin.ios**를 사용 하는 경우이 [블로그 게시물](https://devblogs.microsoft.com/xamarin/uiwebview-deprecation-xamarin-forms/)을 읽어 보세요.
1. (전체 프로젝트에서 또는 적어도 `UIWebView`를 사용 하는 종속성에서) 관리 되는 링커를 사용 하도록 설정 합니다. 참조 되지 않는 경우에는 제거 될 수 *있습니다* . 이렇게 하면 문제를 해결할 수 있지만 코드를 링커에 안전 하 게 만들기 위해 추가 작업이 필요할 수 있습니다.
1. 관리 되는 링커 설정을 변경할 수 없는 경우 아래 특수 한 경우를 참조 하세요.

#### <a name="applications-cannot-use-the-linker-or-change-its-settings"></a>응용 프로그램에서 링커를 사용할 수 없거나 해당 설정을 변경할 수 없습니다.

경우에 따라 관리 되는 링커를 사용 **하지 않는** 경우 (예: **링크 하지 않음**) `UIWebView` 기호가 Apple에 제출 하는 이진 앱에 남아 있으며,이는 거부 될 수 있습니다.

*강제* 솔루션은 프로젝트의 **추가 mtouch 인수**에 `--optimization=force-rejected-types-removal`를 추가 하는 것입니다. 이렇게 하면 응용 프로그램에서 `UIWebView` 추적을 제거 합니다. 그러나 형식을 참조 하는 모든 코드는 제대로 작동 **하지** 않습니다 (예외 또는 충돌 발생). 이 접근 방식은 정적 분석을 통해 연결할 수 있는 경우에도 런타임에 코드에 연결할 수 없는 경우에만 사용 해야 합니다.

#### <a name="support-for-ios-7x-or-earlier"></a>IOS 8.x (또는 이전 버전)에 대 한 지원

`UIWebView`은 v2.0 이후 iOS의 일부입니다. 가장 일반적인 교체는 `WKWebView` (iOS 8) 및 `SFSafariViewController` (iOS 9)입니다. 응용 프로그램에서 여전히 이전 iOS 버전을 지 원하는 경우 다음 옵션을 고려해 야 합니다.

* IOS 8을 최소 대상 버전 (빌드 시간 결정)으로 설정 합니다.
* 앱이 iOS 8 이상 (런타임 결정)에서 실행 되는 경우에만 `WKWebView`을 사용 합니다.

#### <a name="applications-not-submitted-to-apple"></a>Apple에 제출 되지 않은 응용 프로그램

응용 프로그램이 Apple에 제출 되지 않은 경우 이후 iOS 릴리스에서 제거 될 수 있으므로 사용 되지 않는 API에서 벗어나 이동할 계획을 세워야 합니다. 그러나 사용자 고유의 시간표를 사용 하 여이 전환을 수행할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [웹 보기 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
