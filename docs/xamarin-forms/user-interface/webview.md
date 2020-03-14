---
title: Xamarin.Forms WebView
description: 이 문서에서는 사용자에 게 로컬 주도록 Xamarin.Forms WebView 클래스를 사용 하는 방법 또는 네트워크 웹 콘텐츠 및 문서를 설명 합니다.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/04/2019
ms.openlocfilehash: 26fbe6af639c67a94408605ba456bb3a100d2355
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305890"
---
# <a name="xamarinforms-webview"></a>Xamarin.Forms WebView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)

[`WebView`](xref:Xamarin.Forms.WebView) 는 앱에서 웹 및 HTML 콘텐츠를 표시 하는 보기입니다.

![앱 브라우저에서](webview-images/in-app-browser.png)

## <a name="content"></a>콘텐츠

`WebView`는 다음과 같은 콘텐츠 형식을 지원 합니다.

- HTML & CSS websites &ndash; 웹 보기에는 JavaScript 지원을 비롯 하 여 HTML & CSS를 사용 하 여 작성 된 웹 사이트에 대 한 모든 지원이 있습니다
- 웹 보기는 각 플랫폼에서 네이티브 구성 요소를 사용 하 여 구현 되기 때문에 &ndash; 합니다. 웹 보기는 각 플랫폼에서 볼 수 있는 문서를 표시할 수 있습니다. 즉, PDF 파일 iOS 및 Android에서 작동 합니다.
- HTML 문자열 &ndash; 웹 보기는 메모리의 HTML 문자열을 표시할 수 있습니다.
- 로컬 파일 &ndash; 웹 보기는 앱에 포함 된 모든 콘텐츠 형식을 제공할 수 있습니다.

> [!NOTE]
> Windows의 `WebView`은 해당 플랫폼에서 Internet Explorer가 지 원하는 경우에도 Silverlight, Flash 또는 ActiveX 컨트롤을 지원 하지 않습니다.

### <a name="websites"></a>Websites

인터넷에서 웹 사이트를 표시 하려면 `WebView`의 [`Source`](xref:Xamarin.Forms.WebViewSource) 속성을 문자열 URL로 설정 합니다.

```csharp
var browser = new WebView
{
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> 지정 된 프로토콜을 사용 하 여 Url을 완벽 하 게 구성 되어야 합니다 (예: 있어야 "http://" 또는 "https://" 앞에).

#### <a name="ios-and-ats"></a>iOS 및 ATS

IOS 버전 9, 이후 기본적으로 최상의 보안을 구현 하는 서버와 통신 하도록 응용 프로그램 허용 됩니다. 안전 하지 않은 서버와 통신할 수 있도록 `Info.plist`에 값을 설정 해야 합니다.

> [!NOTE]
> 응용 프로그램에 보안 되지 않은 웹 사이트에 대 한 연결이 필요한 경우에는 `NSAllowsArbitraryLoads`를 사용 하 여 ATS를 완전히 켜는 대신 `NSExceptionDomains`를 사용 하 여 도메인을 예외로 입력 해야 합니다. `NSAllowsArbitraryLoads`은 심각한 응급 상황 에서만 사용 해야 합니다.

다음 ATS 요구 사항을 무시 하는 특정 도메인 (이 경우 xamarin.com)를 사용 하도록 설정 하는 방법에 설명 합니다.

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>xamarin.com</key>
            <dict>
                <key>NSIncludesSubdomains</key>
                <true/>
                <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSTemporaryExceptionMinimumTLSVersion</key>
                <string>TLSv1.1</string>
            </dict>
        </dict>
    </dict>
    ...
</key>
```

만 ATS를 사용 하는 동안 트러스트 되지 않은 도메인에 대 한 추가 보안에서 신뢰할 수 있는 사이트를 사용 하 게 사용 하지 않으려면 일부 도메인을 사용 하도록 설정 하는 것이 좋습니다. 다음 앱에 대 한 ATS를 사용 하지 않도록 설정 떨어집니다 메서드를 보여 줍니다.

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
    ...
</key>
```

IOS 9의이 새로운 기능에 대 한 자세한 내용은 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 을 참조 하세요.

### <a name="html-strings"></a>HTML 문자열

코드에 동적으로 정의 된 HTML 문자열을 표시 하려면 [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource)의 인스턴스를 만들어야 합니다.

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![HTML 문자열을 표시 하는 웹 보기](webview-images/html-string.png)

위의 코드에서 `@`는 HTML을 [축 자 문자열 리터럴로](/dotnet/csharp/programming-guide/strings/#regular-and-verbatim-string-literals)표시 하는 데 사용 됩니다. 즉, 대부분의 이스케이프 문자는 무시 됩니다.

> [!NOTE]
> `WebView`의 자식인 레이아웃에 따라 [`WebView`](xref:Xamarin.Forms.WebView) 의 `WidthRequest` 및 `HeightRequest` 속성을 설정 하 여 HTML 콘텐츠를 확인 해야 할 수 있습니다. 예를 들어 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 필요 합니다.

### <a name="local-html-content"></a>로컬 HTML 콘텐츠

WebView에서 HTML, CSS 콘텐츠를 표시할 수 및 앱 내에서 Javascript를 포함 합니다. 다음은 그 예입니다.

```html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamarin.Forms</h1>
    <p>This is an iOS web page.</p>
    <img src="XamarinLogo.png" />
  </body>
</html>
```

CSS:

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

참고 일부 플랫폼에 동일한 글꼴 위의 CSS에 지정 된 글꼴 각 플랫폼에 대해 사용자 지정 해야 합니다.

`WebView`를 사용 하 여 로컬 콘텐츠를 표시 하려면 HTML 파일을 다른 것 처럼 연 다음 `HtmlWebViewSource`의 `Html` 속성에 문자열로 로드 해야 합니다. 파일 열기에 대 한 자세한 내용은 [파일 작업](~/xamarin-forms/data-cloud/data/files.md)을 참조 하세요.

다음 스크린샷에서 각 플랫폼에서 로컬 콘텐츠를 표시 하는 결과 보여 줍니다.

![로컬 콘텐츠를 표시 하는 웹 보기](webview-images/local-content.png)

첫 페이지가 로드 되었지만 `WebView`는 HTML의 출처에 대해 알지 못합니다. 로컬 리소스를 참조 하는 페이지를 처리할 때 문제가 됩니다. 발생 하는 수의 예로 별도 JavaScript 파일의 각 다른 페이지를 사용 하면 로컬 페이지 링크를 사용 하거나 페이지는 CSS 스타일 시트에 연결 하는 경우를 들 수 있습니다.  

이를 해결 하려면 `WebView` 파일 시스템에서 파일을 찾을 위치를 알려 주어 야 합니다. 이렇게 하려면 `WebView`에서 사용 하는 `HtmlWebViewSource` `BaseUrl` 속성을 설정 합니다.

결정 해야 하는 각 운영 체제에서 파일 시스템 다르기 때문에 각 플랫폼에서 해당 URL입니다. Xamarin.ios는 런타임에 각 플랫폼에서 종속성을 확인 하기 위한 `DependencyService`를 노출 합니다.

`DependencyService`를 사용 하려면 먼저 각 플랫폼에서 구현할 수 있는 인터페이스를 정의 합니다.

```csharp
public interface IBaseUrl { string Get(); }
```

Note 인터페이스를 각 플랫폼에서 구현 될 때까지 앱 실행 되지 않습니다. 공통 프로젝트에서 `DependencyService`를 사용 하 여 `BaseUrl`를 설정 해야 합니다.

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

각 플랫폼에 대 한 인터페이스의 구현은 다음 제공 되어야 합니다.

#### <a name="ios"></a>iOS

IOS에서 웹 콘텐츠는 아래와 같이 *BundleResource*빌드 작업을 사용 하 여 프로젝트의 루트 디렉터리 또는 **Resources** 디렉터리에 배치 되어야 합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![IOS의 로컬 파일](webview-images/ios-vs.png)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

![IOS의 로컬 파일](webview-images/ios-xs.png)

-----

`BaseUrl`는 기본 번들의 경로로 설정 해야 합니다.

```csharp
[assembly: Dependency (typeof (BaseUrl_iOS))]
namespace WorkingWithWebview.iOS
{
  public class BaseUrl_iOS : IBaseUrl
  {
    public string Get()
    {
      return NSBundle.MainBundle.BundlePath;
    }
  }
}
```

#### <a name="android"></a>Android

Android에서 아래와 같이 빌드 작업 *Androidasset* 를 사용 하 여 자산 폴더에 HTML, CSS 및 이미지를 넣습니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Android의 로컬 파일](webview-images/android-vs.png)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

![Android의 로컬 파일](webview-images/android-xs.png)

-----

Android에서 `BaseUrl`은 `"file:///android_asset/"`으로 설정 해야 합니다.

```csharp
[assembly: Dependency (typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android
{
  public class BaseUrl_Android : IBaseUrl
  {
    public string Get()
    {
      return "file:///android_asset/";
    }
  }
}
```

Android에서 **자산** 폴더의 파일은 현재 android 컨텍스트를 통해 액세스할 수도 있습니다 .이 컨텍스트는 `MainActivity.Instance` 속성에 의해 노출 됩니다.

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html")))
{
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>범용 Windows 플랫폼

UWP (유니버설 Windows 플랫폼) 프로젝트에서 빌드 작업을 *내용*으로 설정 하 여 프로젝트 루트에 HTML, CSS 및 이미지를 넣습니다.

`BaseUrl`은 `"ms-appx-web:///"`으로 설정 해야 합니다.

```csharp
[assembly: Dependency(typeof(BaseUrl))]
namespace WorkingWithWebview.UWP
{
    public class BaseUrl : IBaseUrl
    {
        public string Get()
        {
            return "ms-appx-web:///";
        }
    }
}
```

## <a name="navigation"></a>탐색

WebView 여러 메서드 및 속성을 사용할 수 있는 탐색을 지원 합니다.

- **Goforward ()** &ndash; `CanGoForward` true 이면 `GoForward`를 호출 하면 다음에 방문한 페이지로 이동 합니다.
- **GoBack ()** &ndash; `CanGoBack` true 이면 `GoBack`를 호출 하면 마지막으로 방문한 페이지로 이동 합니다.
- **CanGoBack** &ndash; `true` 다시 탐색할 페이지가 있는 경우 브라우저의 시작 URL에 `false` 합니다.
- 사용자가 뒤로 탐색 하 고 이미 방문한 페이지로 이동할 수 있는 경우에는 **CanGoForward** &ndash; `true`.

페이지 내에서 `WebView`는 멀티 터치 제스처를 지원 하지 않습니다. 반드시 되도록 해당 콘텐츠는 모바일에 최적화 된 및 확대/축소에 대 한 필요 없이 표시 됩니다.

응용 프로그램에서 장치의 브라우저가 아닌 `WebView`내에 링크를 표시 하는 것이 일반적입니다. 이러한 상황에서는 일반 탐색을 허용 하는 데 유용 하지만 앱 사용자 적중 시작 링크 중일 때를 백업 하는 경우 정상적인 앱 보기 돌아가야 합니다.

기본 제공 탐색 메서드 및 속성을 사용 하 여이 시나리오를 사용 하도록 설정 합니다.

브라우저 보기에 대 한 페이지를 만들어 시작 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="WebViewSample.InAppBrowserXaml"
             Title="Browser">
    <StackLayout Margin="20">
        <StackLayout Orientation="Horizontal">
            <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="OnBackButtonClicked" />
            <Button Text="Forward" HorizontalOptions="EndAndExpand" Clicked="OnForwardButtonClicked" />
        </StackLayout>
        <!-- WebView needs to be given height and width request within layouts to render. -->
        <WebView x:Name="webView" WidthRequest="1000" HeightRequest="1000" />
    </StackLayout>
</ContentPage>
```

코드 숨김에서:

```csharp
public partial class InAppBrowserXaml : ContentPage
{
    public InAppBrowserXaml(string URL)
    {
        InitializeComponent();
        webView.Source = URL;
    }

    async void OnBackButtonClicked(object sender, EventArgs e)
    {
        if (webView.CanGoBack)
        {
            webView.GoBack();
        }
        else
        {
            await Navigation.PopAsync();
        }
    }

    void OnForwardButtonClicked(object sender, EventArgs e)
    {
        if (webView.CanGoForward)
        {
            webView.GoForward();
        }
    }
}
```

이것으로 끝입니다.

![웹 보기 탐색 단추](webview-images/in-app-browser.png)

## <a name="events"></a>이벤트

WebView에 상태에서 변경 내용에 응답할 수 있도록 다음 이벤트를 발생 시킵니다.

- [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) – 웹 보기에서 새 페이지를 로드 하기 시작 하면 이벤트가 발생 합니다.
- [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) – 페이지가 로드 되 고 탐색이 중지 될 때 발생 하는 이벤트입니다.
- [`ReloadRequested`](xref:Xamarin.Forms.WebView.ReloadRequested) – 현재 콘텐츠를 다시 로드 하는 요청을 수행할 때 발생 하는 이벤트입니다.

[`Navigating`](xref:Xamarin.Forms.WebView.Navigating) 이벤트와 함께 제공 되는 [`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs) 개체에는 네 가지 속성이 있습니다.

- `Cancel` – 탐색을 취소할지 여부를 나타냅니다.
- `NavigationEvent` – 발생 한 탐색 이벤트입니다.
- `Source` – 탐색을 수행 하는 요소입니다.
- `Url` – 탐색 대상입니다.

[`Navigated`](xref:Xamarin.Forms.WebView.Navigated) 이벤트와 함께 제공 되는 [`WebNavigatedEventArgs`](xref:Xamarin.Forms.WebNavigatedEventArgs) 개체에는 네 가지 속성이 있습니다.

- `NavigationEvent` – 발생 한 탐색 이벤트입니다.
- `Result` – [`WebNavigationResult`](xref:Xamarin.Forms.WebNavigationResult) 열거형 멤버를 사용 하 여 탐색 결과를 설명 합니다. 유효한 값은 `Cancel`, `Failure`, `Success` 및 `Timeout`입니다.
- `Source` – 탐색을 수행 하는 요소입니다.
- `Url` – 탐색 대상입니다.

로드 하는 데 시간이 오래 걸리는 웹 페이지를 사용 하는 것으로 예상 되는 경우 [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) 및 [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) 이벤트를 사용 하 여 상태 표시기를 구현 하는 것이 좋습니다. 다음은 그 예입니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="WebViewSample.LoadingLabelXaml"
             Title="Loading Demo">
    <StackLayout>
        <!--Loading label should not render by default.-->
        <Label x:Name="labelLoading" Text="Loading..." IsVisible="false" />
        <WebView HeightRequest="1000" WidthRequest="1000" Source="http://www.xamarin.com" Navigated="webviewNavigated" Navigating="webviewNavigating" />
    </StackLayout>
</ContentPage>
```

두 개의 이벤트 처리기:

```csharp
void webviewNavigating(object sender, WebNavigatingEventArgs e)
{
    labelLoading.IsVisible = true;
}

void webviewNavigated(object sender, WebNavigatedEventArgs e)
{
    labelLoading.IsVisible = false;
}
```

이 인해 다음 출력 (로드):

![웹 보기 탐색 이벤트 예제](webview-images/loading-start.png)

완료 된 로드 합니다.

![웹 보기 탐색 이벤트 예제](webview-images/loading-end.png)

## <a name="reloading-content"></a>콘텐츠를 다시 로드

[`WebView`](xref:Xamarin.Forms.WebView) 에는 현재 콘텐츠를 다시 로드 하는 데 사용할 수 있는 `Reload` 메서드가 있습니다.

```csharp
var webView = new WebView();
...
webView.Reload();
```

`Reload` 메서드가 호출 되 면 현재 콘텐츠를 다시 로드 하기 위한 요청이 수행 되었음을 나타내는 `ReloadRequested` 이벤트가 발생 합니다.

## <a name="performance"></a>성능

널리 사용 되는 웹 브라우저는 하드웨어 가속 렌더링 및 JavaScript 컴파일과 같은 기술을 채택 합니다. Xamarin. Forms 4.4 이전에는 `UIWebView` 클래스를 통해 iOS에서 Xamarin.ios `WebView`를 구현 했습니다. 그러나이 구현에서는 이러한 기술을 대부분 사용할 수 없었습니다. 따라서 Xamarin.ios 4.4은 더 빠른 검색을 지 원하는 `WkWebView` 클래스를 통해 iOS에서 `WebView` 구현 됩니다.

> [!NOTE]
> IOS에서 `WkWebViewRenderer`에 `WkWebViewConfiguration` 인수를 허용 하는 생성자 오버 로드가 있습니다. 이렇게 하면 렌더러를 만들 때 구성할 수 있습니다.

응용 프로그램은 호환성을 위해 iOS `UIWebView` 클래스를 사용 하 여 Xamarin.ios `WebView`를 구현 하는 것으로 돌아갈 수 있습니다. 응용 프로그램에 대 한 iOS 플랫폼 프로젝트의 **AssemblyInfo.cs** 파일에 다음 코드를 추가 하 여이 작업을 수행할 수 있습니다.

```csharp
// Opt-in to using UIWebView instead of WkWebView.
[assembly: ExportRenderer(typeof(Xamarin.Forms.WebView), typeof(Xamarin.Forms.Platform.iOS.WebViewRenderer))]
```

Android에서 기본적으로 `WebView`는 기본 제공 브라우저 만큼 빠르게 제공 됩니다.

[UWP 웹 보기](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) 에서는 Microsoft Edge 렌더링 엔진을 사용 합니다. 데스크톱 및 태블릿 장치 자체 Edge 브라우저를 사용 하는 것과 같은 성능을 표시 됩니다.

## <a name="permissions"></a>사용 권한

`WebView` 작동 하려면 각 플랫폼에 대 한 사용 권한이 설정 되어 있는지 확인 해야 합니다. 일부 플랫폼에서 `WebView`는 디버그 모드에서 작동 하지만 릴리스를 위해 빌드할 때는 그렇지 않습니다. 이것은 Android에서 인터넷 액세스에 대 한 것과 같은 일부 권한은 디버그 모드에 있을 때 Mac 용 Visual Studio에서 기본적으로 설정 됩니다.

- **UWP** &ndash;에는 네트워크 콘텐츠를 표시할 때 인터넷 (클라이언트 & 서버) 기능이 필요 합니다.
- **Android** &ndash;에는 네트워크에서 콘텐츠를 표시 하는 경우에만 `INTERNET` 필요 합니다. 로컬 콘텐츠 필요한 특별 한 권한이 없습니다.
- **iOS** &ndash;에는 특별 한 권한이 필요 하지 않습니다.

## <a name="layout"></a>레이아웃

대부분의 다른 Xamarin.ios 보기와 달리 `WebView`에는 StackLayout 또는 RelativeLayout에 포함 될 때 `HeightRequest` 및 `WidthRequest`를 지정 해야 합니다. 이러한 속성을 지정 하지 않으면 `WebView` 렌더링 되지 않습니다.

다음 예에서는 `WebView`를 렌더링 하는 레이아웃을 보여 줍니다.

WidthRequest와 HeightRequest StackLayout:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

WidthRequest와 HeightRequest RelativeLayout:

```xaml
<RelativeLayout>
    <Label Text="test"
        RelativeLayout.XConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=10}"
        RelativeLayout.YConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=20}" />
    <WebView Source="http://www.xamarin.com/"
        RelativeLayout.XConstraint="{ConstraintExpression Type=Constant,
                                     Constant=10}"
        RelativeLayout.YConstraint="{ConstraintExpression Type=Constant,
                                     Constant=50}"
        WidthRequest="1000" HeightRequest="1000" />
</RelativeLayout>
```

AbsoluteLayout HeightRequest & wrequest를 사용 *하지 않습니다* .

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

HeightRequest를 & 하는 wrequest가 *없는* 그리드. 표는 요청 된 높이 및 너비를 지정 하지 않아도 되는 몇 가지 레이아웃 중 하나입니다.:

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="100" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Label Text="test" Grid.Row="0" />
    <WebView Source="http://www.xamarin.com/" Grid.Row="1" />
</Grid>
```

## <a name="invoking-javascript"></a>JavaScript를 호출합니다.

[`WebView`](xref:Xamarin.Forms.WebView) 에는에서 C#JavaScript 함수를 호출 하 고 모든 결과를 호출 C# 코드에 반환 하는 기능이 포함 되어 있습니다. 이 작업은 [웹 보기](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) 샘플의 다음 예제에 표시 된 [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) 메서드를 사용 하 여 수행 됩니다.

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

[`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) 메서드는 인수로 지정 된 JavaScript를 평가 하 고 모든 결과를 `string`으로 반환 합니다. 이 예제에서는 `number`의 계승을 반환 하는 `factorial` JavaScript 함수가 호출 됩니다. 이 JavaScript 함수는 [`WebView`](xref:Xamarin.Forms.WebView) 로드 되는 로컬 HTML 파일에 정의 되어 있으며 다음 예제에 나와 있습니다.

```html
<html>
<body>
<script type="text/javascript">
function factorial(num) {
        if (num === 0 || num === 1)
            return 1;
        for (var i = num - 1; i >= 1; i--) {
            num *= i;
        }
        return num;
}
</script>
</body>
</html>
```

## <a name="uiwebview-deprecation-and-app-store-rejection-itms-90809"></a>UIWebView 보기 사용 중단 및 앱 스토어 거부 (ITMS-90809)

2020 년 4 월부터 [Apple은](https://developer.apple.com/news/?id=12232019b) 아직 사용 되지 않는 `UIWebView` API를 사용 하는 앱을 거부 합니다. Xamarin.ios는 기본값으로 `WKWebView`로 전환 되었지만 Xamarin.ios 이진 파일에는 여전히 이전 SDK에 대 한 참조가 있습니다. 현재 [iOS 링커](~/ios/deploy-test/linker.md) 동작은이를 제거 하지 않습니다. 따라서 앱 스토어에 제출할 때 사용 되지 않는 `UIWebView` API는 여전히 앱에서 참조 되는 것으로 나타납니다.

이 문제를 해결 하기 위해 링커의 미리 보기 버전을 사용할 수 있습니다. 미리 보기를 사용 하도록 설정 하려면 링커에 `--optimize=experimental-xforms-product-type` 추가 인수를 제공 해야 합니다. 

이 작업을 수행 하기 위한 필수 구성 요소는 다음과 같습니다.

- Xamarin.ios **4.5 이상** &ndash; 시험판 버전의 Xamarin. forms 4.5를 사용할 수 있습니다.
- **Xamarin.ios 13.10.0.17 이상** &ndash; [Visual Studio에서](~/cross-platform/troubleshooting/questions/version-logs.md#version-information)xamarin.ios 버전을 확인 합니다. 이 버전의 Xamarin.ios는 Mac용 Visual Studio 8.4.1 및 Visual Studio 16.4.3에 포함 되어 있습니다.
- **`UIWebView`에 대 한 참조를 제거** &ndash; 코드에 `UIWebView` 또는 `UIWebView`를 사용 하는 모든 클래스에 대 한 참조가 없어야 합니다.

### <a name="configure-the-linker-preview"></a>링커 미리 보기 구성

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

링커가 `UIWebView` 참조를 제거 하려면 다음 단계를 수행 합니다.

1. Ios **프로젝트 속성 &ndash; 열려면** ios 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.
1. Ios **빌드 섹션으로 이동 하** &ndash; **ios 빌드** 섹션을 선택 합니다.
1. 추가 **mtouch 인수** &ndash; 추가 **Mtouch 인수를 업데이트** 합니다 .이 플래그는이 플래그를 추가 합니다 `--optimize=experimental-xforms-product-type` (이미 있을 수 있는 값 외에). 참고:이 플래그는 **SDK로만** 설정 된 **링커 동작과** 함께 작동 하거나 **모두 연결**합니다. 어떤 이유로 든 링커 동작을 모두로 설정할 때 오류가 표시 되는 경우이는 응용 프로그램 코드 또는 링커 안전 하지 않은 타사 라이브러리에서 문제가 될 가능성이 높습니다. 링커에 대 한 자세한 내용은 [Xamarin.ios 앱 연결](~/ios/deploy-test/linker.md)을 참조 하세요.
1. 모든 빌드 구성을 **업데이트** &ndash; 창의 맨 위에 있는 **구성** 및 **플랫폼** 목록을 사용 하 여 모든 빌드 구성을 업데이트할 수 있습니다. 업데이트 하는 가장 중요 한 구성은 **릴리스/iPhone** 구성으로, 일반적으로 앱 스토어 전송용 빌드를 만드는 데 사용 되기 때문입니다.

이 스크린샷에는 새 플래그가 설정 된 창이 표시 됩니다.

[iOS 빌드 섹션에서 플래그를 설정 하는 ![](webview-images/iosbuildblade-vs-sml.png)](webview-images/iosbuildblade-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

링커가 `UIWebView` 참조를 제거 하려면 다음 단계를 수행 합니다.

1. Ios **프로젝트 옵션 &ndash; 열려면** ios 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **옵션**을 선택 합니다.
1. Ios **빌드 섹션으로 이동 하** &ndash; **ios 빌드** 섹션을 선택 합니다.
1. 추가  **_mtouch_ 인수** &ndash; 추가 mtouch 인수를 업데이트 합니다 .이 플래그는이 플래그를 추가 합니다 `--optimize=experimental-xforms-product-type` (이미 있을 수 있는 값 외에). 참고:이 플래그는 **SDK로만** 설정 된 **링커 동작과** 함께 작동 하거나 **모두 연결**합니다. 어떤 이유로 든 링커 동작을 모두로 설정할 때 오류가 표시 되는 경우이는 응용 프로그램 코드 또는 링커 안전 하지 않은 타사 라이브러리에서 문제가 될 가능성이 높습니다. 링커에 대 한 자세한 내용은 [Xamarin.ios 앱 연결](~/ios/deploy-test/linker.md)을 참조 하세요.
1. 모든 빌드 구성을 **업데이트** &ndash; 창의 맨 위에 있는 **구성** 및 **플랫폼** 목록을 사용 하 여 모든 빌드 구성을 업데이트할 수 있습니다. 업데이트 하는 가장 중요 한 구성은 **릴리스/iPhone** 구성으로, 일반적으로 앱 스토어 전송용 빌드를 만드는 데 사용 되기 때문입니다.

이 스크린샷에는 새 플래그가 설정 된 창이 표시 됩니다.

[iOS 빌드 섹션에서 플래그를 설정 하는 ![](webview-images/iosbuildblade-xs-sml.png)](webview-images/iosbuildblade-xs.png#lightbox)

-----

이제 새 (릴리스) 빌드를 만들고 앱 스토어에 제출할 때 사용 되지 않는 API에 대 한 경고가 표시 되지 않습니다.

## <a name="related-links"></a>관련 링크

- [웹 보기 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)
- [웹 보기 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview)
