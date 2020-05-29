---
제목: " Xamarin.Forms 웹 보기" 설명: "이 문서에서는 웹 보기 클래스를 사용 하 여 Xamarin.Forms 로컬 또는 네트워크 웹 콘텐츠와 문서를 사용자에 게 표시 하는 방법을 설명 합니다."
assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5: xamarin-forms author: davidbritch: dabritch:: 05/06/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-webview"></a>Xamarin.Forms웹

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)

[`WebView`](xref:Xamarin.Forms.WebView)는 앱에서 웹 및 HTML 콘텐츠를 표시 하는 보기입니다.

![앱 브라우저에서](webview-images/in-app-browser.png)

## <a name="content"></a>콘텐츠

`WebView`는 다음과 같은 콘텐츠 형식을 지원 합니다.

- HTML & CSS websites 웹 &ndash; 보기에는 JavaScript 지원을 비롯 하 여 html & css를 사용 하 여 작성 된 웹 사이트에 대 한 모든 지원이 있습니다
- 문서 &ndash; 보기는 각 플랫폼에서 네이티브 구성 요소를 사용 하 여 구현 되므로 웹 보기는 각 플랫폼에서 볼 수 있는 문서를 표시할 수 있습니다. 즉, iOS 및 Android에서 PDF 파일이 작동 합니다.
- HTML 문자열 &ndash; 웹 보기는 메모리의 html 문자열을 표시할 수 있습니다.
- 로컬 파일 &ndash; 웹 보기는 앱에 포함 된 모든 콘텐츠 형식을 제공할 수 있습니다.

> [!NOTE]
> `WebView`Windows에서는 해당 플랫폼에서 Internet Explorer가 지 원하는 경우에도 Silverlight, Flash 또는 ActiveX 컨트롤을 지원 하지 않습니다.

### <a name="websites"></a>Websites

인터넷에서 웹 사이트를 표시 하려면 `WebView` 의 [`Source`](xref:Xamarin.Forms.WebViewSource) 속성을 문자열 URL로 설정 합니다.

```csharp
var browser = new WebView
{
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> Url은 지정 된 프로토콜로 완전히 구성 되어야 합니다 (즉, "http://" 또는 "https://"가 앞에와 야 함).

#### <a name="ios-and-ats"></a>iOS 및 ATS

버전 9부터 iOS는 응용 프로그램이 기본적으로 최상의 보안을 구현 하는 서버와 통신할 수 있도록 허용 합니다. `Info.plist`안전 하지 않은 서버와의 통신을 사용 하려면에서 값을 설정 해야 합니다.

> [!NOTE]
> 응용 프로그램에 보안 되지 않은 웹 사이트에 대 한 연결이 필요한 경우에는를 `NSExceptionDomains` 사용 하 여 ATS off를 완전히 사용 하는 대신를 사용 하 여 도메인을 항상 예외로 입력 해야 합니다 `NSAllowsArbitraryLoads` . `NSAllowsArbitraryLoads`매우 긴급 한 상황 에서만 사용 해야 합니다.

다음은 특정 도메인 (이 경우 xamarin.com)을 사용 하 여 ATS 요구 사항을 무시 하는 방법을 보여 줍니다.

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

ATS를 무시 하는 도메인을 사용 하는 것이 좋습니다. 신뢰할 수 없는 도메인에 대 한 추가 보안을 애플리케이션을 빌드하고 하는 동안 신뢰할 수 있는 사이트를 사용할 수 있습니다. 다음은 응용 프로그램에 대해 ATS를 사용 하지 않도록 설정 하는 안전 하지 않은 방법을 보여 줍니다.

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

코드에 동적으로 정의 된 HTML 문자열을 표시 하려면 다음과 같이의 인스턴스를 만들어야 합니다 [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource) .

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

위의 코드에서 `@` 는 HTML을 [축 자 문자열 리터럴로](/dotnet/csharp/programming-guide/strings/#regular-and-verbatim-string-literals)표시 하는 데 사용 됩니다. 즉, 대부분의 이스케이프 문자는 무시 됩니다.

> [!NOTE]
> `WidthRequest` `HeightRequest` [`WebView`](xref:Xamarin.Forms.WebView) 가 자식인 레이아웃에 따라의 및 속성을 설정 하 여 HTML 콘텐츠를 확인 해야 할 수도 있습니다 `WebView` . 예를 들어이는에 필요 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 합니다.

### <a name="local-html-content"></a>로컬 HTML 콘텐츠

웹 보기는 앱 내에 포함 된 HTML, CSS 및 JavaScript의 콘텐츠를 표시할 수 있습니다. 예:

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

시트

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

위의 CSS에 지정 된 글꼴은 모든 플랫폼에 동일한 글꼴이 있는 것이 아니라 각 플랫폼에 대해 사용자 지정 해야 합니다.

을 사용 하 여 로컬 콘텐츠를 표시 하려면 `WebView` HTML 파일을 다른 것 처럼 연 다음의 속성에 문자열을 문자열로 로드 해야 합니다 `Html` `HtmlWebViewSource` . 파일 열기에 대 한 자세한 내용은 [파일 작업](~/xamarin-forms/data-cloud/data/files.md)을 참조 하세요.

다음 스크린샷에는 각 플랫폼에서 로컬 콘텐츠를 표시 한 결과가 나와 있습니다.

![로컬 콘텐츠를 표시 하는 웹 보기](webview-images/local-content.png)

첫 번째 페이지가 로드 되어도는 `WebView` HTML의 출처를 알지 못합니다. 이는 로컬 리소스를 참조 하는 페이지를 처리할 때 발생 하는 문제입니다. 로컬 페이지를 서로 연결 하거나, 페이지에서 별도의 JavaScript 파일을 사용 하거나, CSS 스타일 시트에 대 한 페이지 링크를 사용 하는 경우를 예로 들 수 있습니다.  

이를 해결 하려면 `WebView` 파일 시스템에서 파일을 찾을 위치를 알려 주어 야 합니다. 에서 사용 하는에 대 한 속성을 설정 하 여이 작업을 수행 `BaseUrl` `HtmlWebViewSource` `WebView` 합니다.

각 운영 체제의 파일 시스템은 서로 다르기 때문에 각 플랫폼에서 해당 URL을 확인 해야 합니다. Xamarin.Forms`DependencyService`각 플랫폼에서 런타임에 종속성을 확인 하기 위한를 노출 합니다.

를 사용 하려면 `DependencyService` 먼저 각 플랫폼에서 구현할 수 있는 인터페이스를 정의 합니다.

```csharp
public interface IBaseUrl { string Get(); }
```

각 플랫폼에서 인터페이스가 구현 될 때까지 앱이 실행 되지 않습니다. 공통 프로젝트에서를 사용 하 여를 설정 해야 합니다 `BaseUrl` `DependencyService` .

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

그런 다음 각 플랫폼에 대 한 인터페이스 구현을 제공 해야 합니다.

#### <a name="ios"></a>iOS

IOS에서 웹 콘텐츠는 아래와 같이 *BundleResource*빌드 작업을 사용 하 여 프로젝트의 루트 디렉터리 또는 **Resources** 디렉터리에 배치 되어야 합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![IOS의 로컬 파일](webview-images/ios-vs.png)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

![IOS의 로컬 파일](webview-images/ios-xs.png)

-----

는 `BaseUrl` 주 번들의 경로로 설정 해야 합니다.

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

Android에서을 `BaseUrl` 로 설정 해야 합니다 `"file:///android_asset/"` .

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

Android에서 **자산** 폴더의 파일은 속성에 의해 노출 되는 현재 Android 컨텍스트를 통해 액세스할 수도 있습니다 `MainActivity.Instance` .

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html")))
{
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>범용 Windows 플랫폼

UWP (유니버설 Windows 플랫폼) 프로젝트에서 빌드 작업을 *내용*으로 설정 하 여 프로젝트 루트에 HTML, CSS 및 이미지를 넣습니다.

는 `BaseUrl` 로 설정 해야 합니다 `"ms-appx-web:///"` .

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

웹 보기에서 사용할 수 있는 몇 가지 메서드 및 속성을 탐색할 수 있습니다.

- **Goforward ()** &ndash; `CanGoForward`이 true 이면를 호출 하면 `GoForward` 다음에 방문한 페이지로 이동 합니다.
- **GoBack ()** &ndash; `CanGoBack`이 true 이면를 호출 하면 `GoBack` 마지막으로 방문한 페이지로 이동 합니다.
- **CanGoBack** &ndash; `true`다시 탐색할 페이지가 있으면이 고, `false` 브라우저가 시작 URL에 있으면입니다.
- **CanGoForward** &ndash; `true`사용자가 뒤로 탐색 하 여 이미 방문한 페이지로 이동할 수 있는 경우입니다.

페이지 내에서 `WebView` 는 멀티 터치 제스처를 지원 하지 않습니다. 콘텐츠가 모바일에 최적화 되 고 확대/축소를 요구 하지 않고 표시 되는지 확인 하는 것이 중요 합니다.

응용 프로그램에서 장치 브라우저가 아니라 내에 링크를 표시 하는 것이 일반적입니다 `WebView` . 이러한 상황에서 일반적인 탐색을 허용 하는 것이 유용 하지만, 시작 링크에 있는 동안 사용자가 다시 방문 하면 앱이 일반 앱 보기로 돌아옵니다.

기본 제공 탐색 메서드 및 속성을 사용 하 여이 시나리오를 사용 하도록 설정 합니다.

먼저 브라우저 보기에 대 한 페이지를 만듭니다.

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

코드 숨김으로:

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

정말 간단하죠.

![웹 보기 탐색 단추](webview-images/in-app-browser.png)

## <a name="events"></a>이벤트

웹 보기 상태 변경 내용에 응답 하는 데 도움이 되는 다음 이벤트를 발생 시킵니다.

- [`Navigating`](xref:Xamarin.Forms.WebView.Navigating)– 웹 보기에서 새 페이지를 로드 하기 시작 하면 이벤트가 발생 합니다.
- [`Navigated`](xref:Xamarin.Forms.WebView.Navigated)– 페이지가 로드 되 고 탐색이 중지 되 면 이벤트가 발생 합니다.
- [`ReloadRequested`](xref:Xamarin.Forms.WebView.ReloadRequested)– 현재 콘텐츠를 다시 로드 하는 요청을 만들 때 발생 하는 이벤트입니다.

[`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs)이벤트와 함께 제공 되는 개체에는 [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) 다음과 같은 네 가지 속성이 있습니다.

- `Cancel`– 탐색을 취소할지 여부를 나타냅니다.
- `NavigationEvent`– 발생 한 탐색 이벤트입니다.
- `Source`– 탐색을 수행한 요소입니다.
- `Url`– 탐색 대상입니다.

[`WebNavigatedEventArgs`](xref:Xamarin.Forms.WebNavigatedEventArgs)이벤트와 함께 제공 되는 개체에는 [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) 다음과 같은 네 가지 속성이 있습니다.

- `NavigationEvent`– 발생 한 탐색 이벤트입니다.
- `Result`– 열거형 멤버를 사용 하 여 탐색 결과를 설명 합니다 [`WebNavigationResult`](xref:Xamarin.Forms.WebNavigationResult) . 유효한 값은 `Cancel`, `Failure`, `Success` 및 `Timeout`입니다.
- `Source`– 탐색을 수행한 요소입니다.
- `Url`– 탐색 대상입니다.

로드 하는 데 시간이 오래 걸리는 웹 페이지를 사용 하는 것으로 예상 되는 경우 및 이벤트를 사용 하 여 상태 표시기를 구현 하는 것이 좋습니다 [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) . 예:

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

두 이벤트 처리기는 다음과 같습니다.

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

이로 인해 다음과 같이 출력 됩니다 (로딩).

![웹 보기 탐색 이벤트 예제](webview-images/loading-start.png)

로드 완료:

![웹 보기 탐색 이벤트 예제](webview-images/loading-end.png)

## <a name="reloading-content"></a>콘텐츠 다시 로드

[`WebView`](xref:Xamarin.Forms.WebView)에는 `Reload` 현재 콘텐츠를 다시 로드 하는 데 사용할 수 있는 메서드가 있습니다.

```csharp
var webView = new WebView();
...
webView.Reload();
```

`Reload`메서드가 호출 되 면 `ReloadRequested` 현재 콘텐츠를 다시 로드 하도록 요청 되었음을 나타내는 이벤트가 발생 합니다.

## <a name="performance"></a>성능

널리 사용 되는 웹 브라우저는 하드웨어 가속 렌더링 및 JavaScript 컴파일과 같은 기술을 채택 합니다. 4.4 이전 버전에서는 Xamarin.Forms Xamarin.Forms `WebView` 클래스가 iOS에서 구현 되었습니다 `UIWebView` . 그러나이 구현에서는 이러한 기술을 대부분 사용할 수 없었습니다. 따라서 4.4부터 Xamarin.Forms 은 Xamarin.Forms `WebView` `WkWebView` 더 빠른 검색을 지 원하는 클래스에 의해 iOS에서 구현 됩니다.

> [!NOTE]
> IOS에서에는 `WkWebViewRenderer` 인수를 허용 하는 생성자 오버 로드가 있습니다 `WkWebViewConfiguration` . 이렇게 하면 렌더러를 만들 때 구성할 수 있습니다.

응용 프로그램은 호환성을 위해 iOS 클래스를 사용 하 여를 구현할 수 있습니다 `UIWebView` Xamarin.Forms `WebView` . 응용 프로그램에 대 한 iOS 플랫폼 프로젝트의 **AssemblyInfo.cs** 파일에 다음 코드를 추가 하 여이 작업을 수행할 수 있습니다.

```csharp
// Opt-in to using UIWebView instead of WkWebView.
[assembly: ExportRenderer(typeof(Xamarin.Forms.WebView), typeof(Xamarin.Forms.Platform.iOS.WebViewRenderer))]
```

`WebView`Android의 경우 기본적으로 기본 제공 브라우저 만큼 빠르게 제공 됩니다.

[UWP 웹 보기](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) 에서는 Microsoft Edge 렌더링 엔진을 사용 합니다. 데스크톱 및 태블릿 장치에는 Edge 브라우저 자체를 사용 하는 것과 동일한 성능이 표시 되어야 합니다.

## <a name="permissions"></a>사용 권한

가 `WebView` 작동 하려면 각 플랫폼에 대 한 사용 권한이 설정 되어 있는지 확인 해야 합니다. 일부 플랫폼에서는 `WebView` 디버그 모드에서 작동 하지만 릴리스를 위해 빌드할 때는 작동 하지 않습니다. Android에서 인터넷에 액세스 하는 것과 같은 일부 사용 권한은 기본적으로 디버그 모드에서 Mac용 Visual Studio 하 여 설정 되기 때문입니다.

- **UWP** &ndash; 네트워크 콘텐츠를 표시 하는 경우 인터넷 (클라이언트 & 서버) 기능이 필요 합니다.
- **Android** &ndash; `INTERNET`네트워크에서 콘텐츠를 표시 하는 경우에만 필요 합니다. 로컬 콘텐츠에는 특별 한 권한이 필요 하지 않습니다.
- **iOS** &ndash; 특별 한 권한이 필요 하지 않습니다.

## <a name="layout"></a>Layout

대부분의 다른 Xamarin.Forms 뷰와 달리에서는 `WebView` `HeightRequest` `WidthRequest` stacklayout 또는 RelativeLayout에 포함 된 경우 및가 지정 되어야 합니다. 이러한 속성을 지정 하지 않으면 `WebView` 가 렌더링 되지 않습니다.

다음 예제에서는 렌더링을 수행 하는 레이아웃을 보여 줍니다 `WebView` .

HeightRequest가 있는 StackLayout &:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

RelativeLayout가 Wrequest & HeightRequest로:

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

HeightRequest를 & 하는 wrequest가 *없는* 그리드. 그리드는 요청 된 높이 및 너비를 지정 하지 않아도 되는 몇 가지 레이아웃 중 하나입니다.

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

## <a name="invoking-javascript"></a>JavaScript 호출

[`WebView`](xref:Xamarin.Forms.WebView)c #에서 JavaScript 함수를 호출 하 고 모든 결과를 호출 하는 c # 코드에 반환 하는 기능을 포함 합니다. 이 [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) 작업은 [웹 보기](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) 샘플의 다음 예제에 표시 된 메서드를 사용 하 여 수행 됩니다.

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

[`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*)메서드는 인수로 지정 된 JavaScript를 평가 하 고 모든 결과를로 반환 합니다 `string` . 이 예제에서는 `factorial` 의 계승을 반환 하는 JavaScript 함수가 호출 됩니다 `number` . 이 JavaScript 함수는가 로드 하는 로컬 HTML 파일에 정의 되어 [`WebView`](xref:Xamarin.Forms.WebView) 있으며 다음 예제에 나와 있습니다.

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

## <a name="cookies"></a>쿠키

에 대해 쿠키를 설정할 수 있습니다. 그러면이 쿠키는 [`WebView`](xref:Xamarin.Forms.WebView) 웹 요청과 함께 지정 된 URL에 전송 됩니다. 이렇게 하려면 개체를에 추가 하 여이를 수행 `Cookie` `CookieContainer` 합니다. 그러면이 개체는 바인딩 가능한 속성의 값으로 설정 됩니다 `WebView.Cookies` . 다음 코드에서 그 예를 볼 수 있습니다.

```csharp
using System.Net;
using Xamarin.Forms;
// ...

CookieContainer cookieContainer = new CookieContainer();
Uri uri = new Uri("https://dotnet.microsoft.com/apps/xamarin", UriKind.RelativeOrAbsolute);

Cookie cookie = new Cookie
{
    Name = "XamarinCookie",
    Expires = DateTime.Now.AddDays(1),
    Value = "My cookie",
    Domain = uri.Host,
    Path = "/"
};
cookieContainer.Add(uri, cookie);
webView.Cookies = cookieContainer;
webView.Source = new UrlWebViewSource { Url = uri.ToString() };
```

이 예제에서는 `Cookie` 개체에 단일가 추가 되 `CookieContainer` 고,이 개체는 속성의 값으로 설정 됩니다 `WebView.Cookies` . 에서 [`WebView`](xref:Xamarin.Forms.WebView) 지정 된 URL로 웹 요청을 보내는 경우 쿠키는 요청과 함께 전송 됩니다.

## <a name="uiwebview-deprecation-and-app-store-rejection-itms-90809"></a>UIWebView 보기 사용 중단 및 앱 스토어 거부 (ITMS-90809)

4 월 2020부터 [Apple은](https://developer.apple.com/news/?id=12232019b) 아직 사용 되지 않는 API를 사용 하는 앱을 거부 `UIWebView` 합니다. Xamarin.Forms가 기본값으로 전환 되었지만 `WKWebView` 이진 파일에는 여전히 이전 SDK에 대 한 참조가 있습니다 Xamarin.Forms . 현재 [iOS 링커](~/ios/deploy-test/linker.md) 동작은이를 제거 하지 않습니다 `UIWebView` . 따라서 앱 스토어에 제출할 때 사용 되지 않는 API가 여전히 앱에서 참조 되는 것으로 나타납니다.

이 문제를 해결 하기 위해 링커의 미리 보기 버전을 사용할 수 있습니다. 미리 보기를 사용 하도록 설정 하려면 링커에 추가 인수를 제공 해야 `--optimize=experimental-xforms-product-type` 합니다.

이 작업을 수행 하기 위한 필수 구성 요소는 다음과 같습니다.

- ** Xamarin.Forms 4.5 이상**. Xamarin.Forms앱에서 재질 시각적 개체를 사용 하는 경우 4.6 이상이 필요 합니다.
- **Xamarin.ios 13.10.0.17 이상** [Visual Studio에서](~/cross-platform/troubleshooting/questions/version-logs.md#version-information)xamarin.ios 버전을 확인 합니다. 이 버전의 Xamarin.ios는 Mac용 Visual Studio 8.4.1 및 Visual Studio 16.4.3에 포함 되어 있습니다.
- **에 대 한 `UIWebView` 참조를 제거 **합니다. 코드에를 `UIWebView` 사용 하는 또는 클래스에 대 한 참조가 없어야 합니다 `UIWebView` .

참조를 검색 하 고 제거 하는 방법에 대 한 자세한 내용은 `UIWebView` [uiwebview 보기](~/ios/user-interface/controls/webview.md#uiwebview-deprecation)사용 중단을 참조 하세요.

### <a name="configure-the-linker"></a>링커 구성

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

링커가 참조를 제거 하려면 다음 단계를 수행 합니다 `UIWebView` .

1. **IOS 프로젝트 속성 열기** &ndash; IOS 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.
1. **IOS 빌드 섹션** &ndash; 으로 이동 합니다. **IOS 빌드** 섹션을 선택 합니다.
1. **추가 mtouch 인수** &ndash; 를 업데이트 합니다. **추가 mtouch 인수** 에서이 플래그를 추가 `--optimize=experimental-xforms-product-type` 합니다 (이미 여기에 있을 수 있는 값 외에). 참고:이 플래그는 **SDK로만** 설정 된 **링커 동작과** 함께 작동 하거나 **모두 연결**합니다. 어떤 이유로 든 링커 동작을 모두로 설정할 때 오류가 표시 되는 경우이는 응용 프로그램 코드 또는 링커 안전 하지 않은 타사 라이브러리에서 문제가 될 가능성이 높습니다. 링커에 대 한 자세한 내용은 [Xamarin.ios 앱 연결](~/ios/deploy-test/linker.md)을 참조 하세요.
1. **모든 빌드 구성 업데이트** &ndash; 창 맨 위에 있는 **구성** 및 **플랫폼** 목록을 사용 하 여 모든 빌드 구성을 업데이트할 수 있습니다. 업데이트 하는 가장 중요 한 구성은 **릴리스/iPhone** 구성으로, 일반적으로 앱 스토어 전송용 빌드를 만드는 데 사용 되기 때문입니다.

이 스크린샷에는 새 플래그가 설정 된 창이 표시 됩니다.

[![IOS 빌드 섹션에서 플래그 설정](webview-images/iosbuildblade-vs-sml.png)](webview-images/iosbuildblade-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

링커가 참조를 제거 하려면 다음 단계를 수행 합니다 `UIWebView` .

1. **IOS 프로젝트 옵션 열기** &ndash; IOS 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **옵션**을 선택 합니다.
1. **IOS 빌드 섹션** &ndash; 으로 이동 합니다. **IOS 빌드** 섹션을 선택 합니다.
1. **_mtouch_ ** &ndash; **추가 _mtouch_ 인수** 에서 추가 mtouch 인수를 업데이트 합니다 .이 플래그는 `--optimize=experimental-xforms-product-type` 이미 여기에 있을 수 있는 값과 함께 추가 합니다. 참고:이 플래그는 **SDK로만** 설정 된 **링커 동작과** 함께 작동 하거나 **모두 연결**합니다. 어떤 이유로 든 링커 동작을 모두로 설정할 때 오류가 표시 되는 경우이는 응용 프로그램 코드 또는 링커 안전 하지 않은 타사 라이브러리에서 문제가 될 가능성이 높습니다. 링커에 대 한 자세한 내용은 [Xamarin.ios 앱 연결](~/ios/deploy-test/linker.md)을 참조 하세요.
1. **모든 빌드 구성 업데이트** &ndash; 창 맨 위에 있는 **구성** 및 **플랫폼** 목록을 사용 하 여 모든 빌드 구성을 업데이트할 수 있습니다. 업데이트 하는 가장 중요 한 구성은 **릴리스/iPhone** 구성으로, 일반적으로 앱 스토어 전송용 빌드를 만드는 데 사용 되기 때문입니다.

이 스크린샷에는 새 플래그가 설정 된 창이 표시 됩니다.

[![IOS 빌드 섹션에서 플래그 설정](webview-images/iosbuildblade-xs-sml.png)](webview-images/iosbuildblade-xs.png#lightbox)

-----

이제 새 (릴리스) 빌드를 만들고 앱 스토어에 제출할 때 사용 되지 않는 API에 대 한 경고가 표시 되지 않습니다.

## <a name="related-links"></a>관련 링크

- [웹 보기 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)
- [웹 보기 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview)
- [UIWebView 보기 사용 중단](~/ios/user-interface/controls/webview.md#uiwebview-deprecation)
