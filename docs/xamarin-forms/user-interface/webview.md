---
title: Xamarin.Forms WebView
description: 이 문서에서는 사용자에 게 로컬 주도록 Xamarin.Forms WebView 클래스를 사용 하는 방법 또는 네트워크 웹 콘텐츠 및 문서를 설명 합니다.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2018
ms.openlocfilehash: 8d68afaf0edf178bba6f18d3071de029e111edee
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118671"
---
# <a name="xamarinforms-webview"></a>Xamarin.Forms WebView

[`WebView`](xref:Xamarin.Forms.WebView) 앱에서 웹 및 HTML 콘텐츠를 표시 하기 위한 뷰입니다. 와 달리 `OpenUri`, 사용자 장치의 웹 브라우저에 적용 되 `WebView` 앱 내에서 HTML 콘텐츠를 표시 합니다.

![](webview-images/in-app-browser.png "앱 내 브라우저에서")

## <a name="content"></a>콘텐츠

`WebView` 다음 유형의 콘텐츠를 지원합니다.

- HTML 및 CSS websites &ndash; 웹 보기 HTML 및 JavaScript 지원을 비롯 하 여 CSS를 사용 하 여 작성 하는 웹 사이트에 대 한 완전 한 지원을 제공 합니다.
- 문서 &ndash; WebView를 네이티브 구성 요소를 사용 하 여 각 플랫폼에서 된 구현 되므로 WebView 수는 각 플랫폼에서 볼 수 있는 문서를 표시 합니다. 즉, PDF 파일 iOS 및 Android에서 작동 합니다.
- HTML 문자열 &ndash; WebView 메모리에서 HTML 문자열을 표시할 수 있습니다.
- 로컬 파일 &ndash; WebView 앱에 포함 된 위의 콘텐츠 형식 중 하나를 제공할 수 있습니다.

> [!NOTE]
> `WebView` Windows에서 지원 하지 않습니다 Silverlight, Flash 또는 모든 ActiveX 컨트롤을 해당 플랫폼에서 Internet Explorer에서 지원 되는 경우에.

### <a name="websites"></a>웹 사이트

인터넷에서 웹 사이트를 표시 하려면 설정 합니다 `WebView`의 [ `Source` ](xref:Xamarin.Forms.WebViewSource) 속성을 문자열 URL:

```csharp
var browser = new WebView {
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> 지정 된 프로토콜을 사용 하 여 Url을 완벽 하 게 구성 되어야 합니다 (예: 있어야 "http://" 또는 "https://" 앞에).

#### <a name="ios-and-ats"></a>iOS 및 ATS

IOS 버전 9, 이후 기본적으로 최상의 보안을 구현 하는 서버와 통신 하도록 응용 프로그램 허용 됩니다. 값을 설정 해야 `Info.plist` 안전 하지 않은 서버와 통신할 수 있도록 합니다.

> [!NOTE]
> 도메인으로 사용 하 여 예외를 항상 입력 해야 응용 프로그램 안전 하지 않은 웹 사이트에 연결을 요구 하는 경우 `NSExceptionDomains` ATS 사용 하 여 완전히 해제 하는 대신 `NSAllowsArbitraryLoads`합니다. `NSAllowsArbitraryLoads` 극단적인 비상 시에 사용 해야 합니다.

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
```

만 ATS를 사용 하는 동안 트러스트 되지 않은 도메인에 대 한 추가 보안에서 신뢰할 수 있는 사이트를 사용 하 게 사용 하지 않으려면 일부 도메인을 사용 하도록 설정 하는 것이 좋습니다. 다음 앱에 대 한 ATS를 사용 하지 않도록 설정 떨어집니다 메서드를 보여 줍니다.

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md) iOS 9의에서이 새로운 기능에 대 한 자세한 내용은 합니다.

### <a name="html-strings"></a>HTML 문자열

인스턴스를 만들 해야 코드에서 동적으로 정의 하는 html 문자열을 제공 하려는 경우 [ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![](webview-images/html-string.png "HTML 문자열을 표시 하는 WebView")

위의 코드에서 `@` HTML을 리터럴, 모든 일반적인 이스케이프 문자는 무시 되는 문자열로 표시 하는 데 사용 됩니다.

### <a name="local-html-content"></a>로컬 HTML 콘텐츠

WebView에서 HTML, CSS 콘텐츠를 표시할 수 및 앱 내에서 Javascript를 포함 합니다. 예를 들어:

```html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamrin.Forms</h1>
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

로 표시 로컬 콘텐츠를 `WebView`를 HTML 파일을 연 다음에 문자열 내용을 로드 해야는 `Html` 의 속성은 `HtmlWebViewSource`합니다. 파일 열기에 대 한 자세한 내용은 참조 하세요. [파일로 작업할](~/xamarin-forms/app-fundamentals/files.md)합니다.

다음 스크린샷에서 각 플랫폼에서 로컬 콘텐츠를 표시 하는 결과 보여 줍니다.

![](webview-images/local-content.png "웹 보기 로컬 콘텐츠 표시")

첫 페이지 로드 하지만 `WebView` 알지 HTML에서 발생 한 위치입니다. 로컬 리소스를 참조 하는 페이지를 처리할 때 문제가 됩니다. 발생 하는 수의 예로 별도 JavaScript 파일의 각 다른 페이지를 사용 하면 로컬 페이지 링크를 사용 하거나 페이지는 CSS 스타일 시트에 연결 하는 경우를 들 수 있습니다.  

이 해결 하려면 지시 해야 합니다 `WebView` 파일 시스템에서 파일을 찾을 위치입니다. 설정 하 여 이렇게 합니다 `BaseUrl` 속성에는 `HtmlWebViewSource` 에서 사용 하는 `WebView`.

결정 해야 하는 각 운영 체제에서 파일 시스템 다르기 때문에 각 플랫폼에서 해당 URL입니다. Xamarin.Forms 노출는 `DependencyService` 각 플랫폼에서 런타임 시 종속성을 확인 합니다.

사용 하는 `DependencyService`, 먼저 각 플랫폼에서 구현할 수 있는 인터페이스를 정의 합니다.

```csharp
public interface IBaseUrl { string Get(); }
```

Note 인터페이스를 각 플랫폼에서 구현 될 때까지 앱 실행 되지 않습니다. Common 프로젝트에서 설정 해야 있는지 확인 합니다 `BaseUrl` 를 사용 하는 `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

각 플랫폼에 대 한 인터페이스의 구현은 다음 제공 되어야 합니다.

#### <a name="ios"></a>iOS

Ios, 웹 콘텐츠를 프로젝트의 루트 디렉터리에 배치 해야 하거나 **리소스** 빌드 작업 디렉터리 *BundleResource*아래 설명 된 대로,:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](webview-images/ios-vs.png "IOS에서 로컬 파일")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](webview-images/ios-xs.png "IOS에서 로컬 파일")

-----

`BaseUrl` 기본 번들의 경로를 설정 해야 합니다.

```csharp
[assembly: Dependency (typeof (BaseUrl_iOS))]
namespace WorkingWithWebview.iOS{
  public class BaseUrl_iOS : IBaseUrl {
    public string Get() {
      return NSBundle.MainBundle.BundlePath;
    }
  }
}
```

#### <a name="android"></a>Android

Android, HTML, CSS 및 이미지에에서 배치 빌드 작업을 사용 하 여 자산 폴더 *AndroidAsset* 아래 설명 된 대로:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](webview-images/android-vs.png "Android에서 로컬 파일")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](webview-images/android-xs.png "Android에서 로컬 파일")

-----

Android에는 `BaseUrl` 로 설정 해야 `"file:///android_asset/"`:

```csharp
[assembly: Dependency (typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android {
  public class BaseUrl_Android : IBaseUrl {
    public string Get() {
      return "file:///android_asset/";
    }
  }
}
```

Android에서의 파일을 **자산** 폴더에 의해 노출 되는 Android 현재 컨텍스트를 통해 액세스할 수도 있습니다는 `MainActivity.Instance` 속성:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html"))) {
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

유니버설 Windows 플랫폼 (UWP) 프로젝트에서 HTML, CSS 및 이미지에에서 배치 프로젝트 루트 설정 된 빌드 작업을 사용 하 여 *콘텐츠*합니다.

합니다 `BaseUrl` 로 설정 해야 `"ms-appx-web:///"`:

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

- **GoForward()** &ndash; 하는 경우 `CanGoForward` 가 true 이면 호출 `GoForward` 다음 방문 페이지로 이동 합니다.
- **GoBack()** &ndash; 하는 경우 `CanGoBack` 가 true 이면 호출 `GoBack` 방문한 마지막 페이지로 이동 됩니다.
- **CanGoBack** &ndash; `true` 페이지를 다시 이동할 경우 `false` 브라우저 시작 URL에 있는 경우.
- **CanGoForward** &ndash; `true` 사용자가 뒤로 탐색 하 고 이미 방문한 적이 있는 페이지로 진행할 수 있습니다.

페이지 내에서 `WebView` 멀티 터치 제스처를 지원 하지 않습니다. 반드시 되도록 해당 콘텐츠는 모바일에 최적화 된 및 확대/축소에 대 한 필요 없이 표시 됩니다.

일반적으로 응용 프로그램 내에서 링크를 표시 하는 `WebView`, 장치의 브라우저를 대신 합니다. 이러한 상황에서는 일반 탐색을 허용 하는 데 유용 하지만 앱 사용자 적중 시작 링크 중일 때를 백업 하는 경우 정상적인 앱 보기 돌아가야 합니다.

기본 제공 탐색 메서드 및 속성을 사용 하 여이 시나리오를 사용 하도록 설정 합니다.

브라우저 보기에 대 한 페이지를 만들어 시작 합니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.InAppDemo"
Title="In App Browser">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal" Padding="10,10">
                <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="backClicked" />
                <Button Text="Forward" HorizontalOptions="End" Clicked="forwardClicked" />
            </StackLayout>
            <WebView x:Name="Browser" WidthRequest="1000" HeightRequest="1000" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

이 코드 숨김에서:

```csharp
public partial class InAppDemo : ContentPage
{
  //sets the URL for the browser in the page at creation
    public InAppDemo (string URL)
    {
        InitializeComponent ();
        Browser.Source = URL;
    }


    private void backClicked(object sender, EventArgs e)
    {
    // Check to see if there is anywhere to go back to
        if (Browser.CanGoBack) {
            Browser.GoBack ();
        } else { // If not, leave the view
            Navigation.PopAsync ();
        }
    }

    private void forwardClicked(object sender, EventArgs e)
    {
        if (Browser.CanGoForward) {
            Browser.GoForward ();
        }
    }
}
```

정말 간단하죠.

![](webview-images/in-app-browser.png "WebView 탐색 단추")

## <a name="events"></a>이벤트

웹 보기 상태에서 변경 내용에 응답할 수 있도록 두 개의 이벤트를 발생 시킵니다.

- **탐색** &ndash; WebView 새 페이지를 로드할 시작 될 때 발생 하는 이벤트입니다.
- **탐색할** &ndash; 이벤트 발생 페이지가 로드 되 고 탐색이 중지 되었습니다.

로드 시간이 오래 걸리는 웹 페이지를 사용 하 여 예상 되는 경우에 해당 이벤트를 사용 하 여 상태 표시기를 구현 하는 것이 좋습니다. 예를 들어는 XAML은 다음과 같습니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.LoadingDemo" Title="Loading Demo">
  <ContentPage.Content>
    <StackLayout>
      <Label x:Name="LoadingLabel"
        Text="Loading..."
        HorizontalOptions="Center"
        IsVisible="false" />
      <WebView x:Name="Browser"
      HeightRequest="1000"
      WidthRequest="1000"
      Navigating="webOnNavigating"
      Navigated="webOnEndNavigating" />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

두 개의 이벤트 처리기:

```csharp
void webOnNavigating (object sender, WebNavigatingEventArgs e)
{
    LoadingLabel.IsVisible = true;
}

void webOnEndNavigating (object sender, WebNavigatedEventArgs e)
{
    LoadingLabel.IsVisible = false;
}
```

이 인해 다음 출력 (로드):

![](webview-images/loading-start.png "WebView 탐색 이벤트 예제")

완료 된 로드 합니다.

![](webview-images/loading-end.png "WebView 탐색할된 이벤트 예")

## <a name="performance"></a>성능

하드웨어 가속 렌더링 및 JavaScript 컴파일와 같은 인기 있는 웹 브라우저를 이제 기술을 채택 합니다. Ios의 경우 기본적으로 Xamarin.Forms `WebView` 에 의해 구현 됩니다는 `UIWebView` 클래스 및 이러한 여러 기술은이 구현에서 사용할 수 없는 합니다. 그러나 응용 프로그램에서 옵트인 iOS를 사용 하 여 `WkWebView` Xamarin.Forms를 구현 하는 클래스 `WebView`를 더 빠르게 검색 하는 것이 지 합니다. 다음 코드를 추가 하 여 수행할 수 있습니다 합니다 **AssemblyInfo.cs** 응용 프로그램에 대 한 iOS 플랫폼 프로젝트에서 파일:

```csharp
// Opt-in to using WkWebView instead of UIWebView.
[assembly: ExportRenderer(typeof(WebView), typeof(Xamarin.Forms.Platform.iOS.WkWebViewRenderer))]
```

`WebView` 기본적으로 Android에 대 한 기본 제공 브라우저 빨리입니다.

합니다 [UWP WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) Microsoft Edge 렌더링 엔진을 사용 합니다. 데스크톱 및 태블릿 장치 자체 Edge 브라우저를 사용 하는 것과 같은 성능을 표시 됩니다.

## <a name="permissions"></a>사용 권한

에 대 한 순서로 `WebView` 하려면 각 플랫폼에 대 한 권한이 설정 되어 있는지 확인 해야 합니다. 일부 플랫폼에서는 유의 `WebView` 릴리스용으로 빌드된 때가 아니라 하지만 디버그 모드에서 작동 합니다. 이것은 Android에서 인터넷 액세스에 대 한 것과 같은 일부 권한은 디버그 모드에 있을 때 Mac 용 Visual Studio에서 기본적으로 설정 됩니다.

- **UWP** &ndash; 네트워크 콘텐츠를 표시 하는 경우 인터넷 (클라이언트 및 서버) 기능이 필요 합니다.
- **Android** &ndash; 필요 `INTERNET` 네트워크에서 콘텐츠를 표시 하는 경우에 합니다. 로컬 콘텐츠 필요한 특별 한 권한이 없습니다.
- **iOS** &ndash; 필요한 특별 한 권한이 없습니다.

## <a name="layout"></a>레이아웃

다른 대부분의 Xamarin.Forms 보기와 달리 `WebView` 되어 있어야 `HeightRequest` 및 `WidthRequest` StackLayout 또는 RelativeLayout에 포함 된 경우 지정 된 합니다. 이러한 속성을 지정 하지 않으면는 `WebView` 렌더링 되지 것입니다.

다음 예제에서는 작업에서 발생 하는 레이아웃을 보여 줍니다 렌더링 `WebView`s:

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

AbsoluteLayout *없이* WidthRequest & HeightRequest:

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

표 형태 *없이* WidthRequest & HeightRequest 합니다. 표는 요청 된 높이 및 너비를 지정 하지 않아도 되는 몇 가지 레이아웃 중 하나입니다.:

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

합니다 [ `WebView` ](xref:Xamarin.Forms.WebView) C#에서 JavaScript 함수를 호출 하 고 C# 코드를 호출 하려면 결과 반환 하는 기능을 포함 합니다. 주게 됩니다 합니다 [ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) 의 다음 예제에 나와 있는 메서드를 [WebView](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) 샘플:

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

합니다 [ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) 인수로 지정 되 고 모든 결과 반환 하는 JavaScript를 평가 하는 메서드를 `string`입니다. 이 예제는 `factorial` 계승값을 반환 하는 JavaScript 함수가 호출 되 `number` 결과적으로 합니다. 이 JavaScript 함수는 로컬 HTML에 정의 된 파일을 [ `WebView` ](xref:Xamarin.Forms.WebView) 로드 하 고 다음 예와에서 같습니다:

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

## <a name="related-links"></a>관련 링크

- [WebView (샘플)를 사용 하 여 작업](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [WebView (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
