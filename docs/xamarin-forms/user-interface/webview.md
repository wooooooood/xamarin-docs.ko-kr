---
title: Xamarin.Forms 웹 보기
description: 이 문서에서는 사용자에 게 로컬 표시 하는 Xamarin.Forms WebView 클래스를 사용 하는 방법 또는 네트워크 웹 콘텐츠 및 문서를 설명 합니다.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: df004bd2a580e48137162d28ca3974521266ae7a
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245646"
---
# <a name="xamarinforms-webview"></a>Xamarin.Forms 웹 보기

[WebView](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) 웹 및 HTML을 표시 하기 위한 뷰는 응용 프로그램의 콘텐츠입니다. 와 달리 `OpenUri`, 사용자 장치에서 웹 브라우저를 사용 하는 `WebView` 앱 내에서 HTML 콘텐츠를 표시 합니다.

이 가이드는 다음 섹션으로 구성 됩니다.

- **[콘텐츠](#Content)**  &ndash; WebView 포함 된 HTML, 웹 페이지 및 HTML 문자열을 포함 하 여 다양 한 콘텐츠 소스를 지원 합니다.
- **[탐색](#Navigation)**  &ndash; WebView 특정 페이지로 이동 하 고 다시 살펴보자면에 대 한 지원이 포함 됩니다.
- **[이벤트](#Events)**  &ndash; 수신 대기 하 고 웹 보기에서 사용자가 수행한 작업에 응답 합니다.
- **[성능](#Performance)**  &ndash; 각 플랫폼에서 WebView의 성능 특성에 알아봅니다.
- **[사용 권한](#Permissions)**  &ndash; WebView 응용 프로그램에서 작동할 수 있도록 사용 권한을 설정 하는 방법을 알아봅니다.
- **[레이아웃](#Layout)**  &ndash; WebView에 배치 된 방법에 대 한 몇 가지 매우 특정 요구 사항이 있습니다. WebView 제대로 표시 되는지 확인 하는 방법에 알아봅니다.

![](webview-images/in-app-browser.png "앱 내 브라우저에서")

## <a name="content"></a>콘텐츠

웹 보기는 다음과 같은 유형의 내용에 대 한 지원으로 제공 됩니다.

- HTML & CSS 웹 사이트 &ndash; WebView HTML & CSS, JavaScript 지원을 비롯 하 여 사용 하 여 작성 된 웹 사이트에 대 한 완전 한 지원을 제공 합니다.
- 문서 &ndash; 네이티브 구성 요소를 사용 하 여 각 플랫폼에서 WebView 구현 된 이므로 웹 보기는 각 플랫폼에서 볼 수 있는 문서를 지 원하는 합니다. 즉, PDF 파일은 iOS 및 Android에서 작동 합니다.
- HTML 문자열 &ndash; WebView 메모리에서 HTML 문자열을 표시할 수 있습니다.
- 로컬 파일 &ndash; WebView 응용 프로그램에 포함 된 위의 콘텐츠 형식 중 하나를 제공할 수 있습니다.

> [!NOTE]
> `WebView` Windows에서 지원 하지 않습니다 Silverlight, Flash 또는 ActiveX 컨트롤을 사용 하는 경우에 해당 플랫폼에서 Internet Explorer를 통해 지원 됩니다.

### <a name="websites"></a>웹 사이트

인터넷에서 웹 사이트를 표시 하려면 설정는 `WebView`의 [ `Source` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebViewSource/) 속성을 문자열 URL:

```csharp
var browser = new WebView {
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> Url 지정 된 프로토콜와 완벽 하 게 구성 되어야 합니다 (예: 있어야 "http://" 또는 "https://"에 같고).

#### <a name="ios-and-ats"></a>iOS 및 AT

IOS 버전 9, 이후 기본적으로 보안 모범 사례를 구현 하는 서버와 통신 하는 응용 프로그램 허용 됩니다. 값을 설정 해야 `Info.plist` 안전 하지 않은 서버와 통신 하도록 설정 합니다.

> [!NOTE]
> 응용 프로그램에서 안전 하지 않은 웹 사이트에 대 한 연결을 필요한 경우 항상 입력할 도메인으로 사용 하 여 예외 `NSExceptionDomains` AT 사용 하 여 완전히 해제 하는 대신 `NSAllowsArbitraryLoads`합니다. `NSAllowsArbitraryLoads` 극단적인 응급 상황에만 사용 해야 합니다.

다음은 AT 요구 사항을 사용 하지 않으려면 (이 case xamarin.com)에서 특정 도메인을 사용 하도록 설정 하는 방법을 보여 줍니다.

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

일부 도메인에서 트러스트 되지 않은 도메인에 추가 보안 혜택이 하는 동안 신뢰할 수 있는 사이트를 사용할 수 있도록 AT를 무시 하에 사용 하는 것이 좋습니다. 다음 응용 프로그램에 대 한 AT의 약화 된 보안 방법을 보여 줍니다.

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md) iOS 9의에서이 새로운 기능에 대 한 자세한 내용은 합니다.

### <a name="html-strings"></a>HTML 문자열

인스턴스를 만드는 해야 코드에서 동적으로 정의 된 html 문자열을 표시 하려는 경우 [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![](webview-images/html-string.png "HTML 문자열을 표시 하는 웹 보기")

위의 코드에서 `@` HTML 문자열 리터럴, 모든 일반적인 이스케이프 문자는 무시 표시 하는 데 사용 됩니다.

### <a name="local-html-content"></a>HTML 콘텐츠를 로컬

WebView HTML, CSS에서에서 콘텐츠를 표시 하 고 Javascript 응용 프로그램 내에 포함 합니다. 예를 들어:

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

참고 일부 플랫폼에 동일한 글꼴 위의 CSS에 지정 된 글꼴 각 플랫폼에 대해 사용자 지정할 수 있도록 해야 합니다.

디스플레이 로컬 콘텐츠 사용 하는 `WebView`, 요, HTML 파일을 연 다음 내용을 문자열로 문자열을 로드 해야는 `Html` 속성의는 `HtmlWebViewSource`합니다. 파일을 열에 대 한 자세한 내용은 참조 하십시오. [파일 작업](~/xamarin-forms/app-fundamentals/files.md)합니다.

다음 스크린샷에서 각 플랫폼에서 로컬 콘텐츠를 표시 하는 결과 보여 줍니다.

![](webview-images/local-content.png "로컬 콘텐츠 웹 보기 표시")

첫 페이지 로드 된 있지만 `WebView` HTML에서 제공 하는 위치에 대해 알지 못합니다. 로컬 리소스를 참조 하는 페이지를 처리할 때 문제입니다. 현상이 발생할 수 있습니다 하의 예로 별도 JavaScript 파일의 각 다른 페이지를 사용 하면 로컬 페이지 링크를 사용 하거나 CSS 스타일 시트에는 페이지 링크를 들 수 있습니다.  

이 문제를 해결 하려면 구별 하는 `WebView` 파일 시스템에서 파일을 찾을 수 있는 위치입니다. 설정 하 여 그렇게는 `BaseUrl` 속성에는 `HtmlWebViewSource` 에서 사용 하는 `WebView`합니다.

결정 해야 하는 각 운영 체제에서 파일 시스템 다르기 때문에 각 플랫폼에서 해당 URL입니다. Xamarin.Forms 노출는 `DependencyService` 각 플랫폼에서 런타임 시 종속성 확인에 대 한 합니다.

사용 하는 `DependencyService`, 먼저 각 플랫폼에서 구현할 수 있는 인터페이스를 정의 합니다.

```csharp
public interface IBaseUrl { string Get(); }
```

Note 인터페이스를 구현 하는 각 플랫폼에서 될 때까지 응용 프로그램 실행 되지 않습니다. 공용 프로젝트에서 설정 해야 하 고 있는지 확인은 `BaseUrl` 를 사용 하는 `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

그런 다음 각 플랫폼에 대 한 인터페이스의 구현을 제공 합니다.

#### <a name="ios"></a>iOS

Ios에서 프로젝트의 루트 디렉터리에서 웹 콘텐츠를 위치 또는 **리소스** ड 빌드 작업 ि र ॅ *BundleResource*아래 설명 된 대로,:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/ios-vs.png "IOS의 로컬 파일")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](webview-images/ios-xs.png "IOS의 로컬 파일")

-----

`BaseUrl` 주 번들의 경로를 설정 해야 합니다.

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

Android에서는 배치 HTML, CSS 및 이미지 빌드 작업으로 자산 폴더에 *AndroidAsset* 아래 설명 된 대로:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/android-vs.png "Android에서 로컬 파일")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](webview-images/android-xs.png "Android에서 로컬 파일")

-----

Android에서는 `BaseUrl` 로 설정 해야 `"file:///android_asset/"`:

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

Android에서는 파일에 **자산** 폴더에 의해 노출 되는 현재 Android 컨텍스트를 통해 액세스할 수도 있습니다는 `MainActivity.Instance` 속성:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html"))) {
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

유니버설 Windows 플랫폼 (UWP) 프로젝트에서 HTML, CSS 및 이미지에에서 배치 프로젝트 루트로 설정 된 빌드 동작을 가진 *콘텐츠*합니다.

`BaseUrl` 로 설정 해야 `"ms-appx-web:///"`:

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

웹 보기를 통해 여러 메서드 및 속성을 사용할 수 있는 탐색을 지원 합니다.

- **GoForward()** &ndash; 경우 `CanGoForward` 가 true 이면 호출 `GoForward` 방문한 다음 페이지를 앞으로 이동 합니다.
- **GoBack()** &ndash; 경우 `CanGoBack` 가 true 이면 호출 `GoBack` 마지막 방문한 페이지를 탐색 합니다.
- **CanGoBack** &ndash; `true` 페이지를 다시 탐색 하는 경우 `false` 브라우저 시작 URL에 있으면 합니다.
- **CanGoForward** &ndash; `true` 사용자가 뒤로 탐색 하 고 개체를 방문한 이미 있는 페이지에 앞으로 이동할 수 있습니다.

페이지 내에서 `WebView` 멀티 터치 제스처를 지원 하지 않습니다. 반드시 되도록 하려면 해당 콘텐츠 모바일 액세스에 최적화 하 고 확대/축소에 대 한 필요 없이 나타납니다.

일반적으로 응용 프로그램 내에서 링크를 표시 하는 `WebView`, 장치의 브라우저 대신 합니다. 이런 경우, 일반 탐색할 수 있도록 하는 것이 유용 하지만 사용자 적중 시작 링크에는 백업, 응용 프로그램 일반적인 앱 보기를 반환 해야 합니다.

이 시나리오를 사용 하는 기본 제공 탐색 메서드 및 속성을 사용 합니다.

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

웹 보기 상태에서 변경에 대응할 수 있도록 두 개의 이벤트를 발생 시킵니다.

- **탐색** &ndash; WebView 새 페이지를 로드할 시작 될 때 발생 하는 이벤트입니다.
- **탐색** &ndash; 이벤트 발생 페이지가 로드 되 고 탐색이 중지 되었습니다.

로드 하는 데 오랜 시간이 걸리는 웹 페이지를 사용 하 여 예상 되는 경우 이러한 이벤트를 사용 하 여 상태 표시기를 구현 하는 것이 좋습니다. 예를 들어 XAML는 다음과 같습니다.

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

두 개의 이벤트 처리기.

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

이 인해 (로드)는 다음과 같은 출력:

![](webview-images/loading-start.png "WebView 탐색 이벤트 예제")

완료 된 부하:

![](webview-images/loading-end.png "WebView 탐색 이벤트 예제")

## <a name="performance"></a>성능

최근 발전 기술을 채택 하 여 하드웨어 가속 렌더링 및 JavaScript 컴파일와 같은 인기 있는 웹 브라우저의 각 살펴보았습니다. 안타깝게도, 보안 제한 때문에 대부분의 이러한 고급 기능을 통해 사용할 수 없었습니다에서의 iOS equaivalent `WebView`, `UIWebView`합니다. Xamarin.Forms `WebView` 사용 하 여 `UIWebView`합니다. 사용자 지정 렌더러를 사용 하 여 작성 해야 하는 문제가 경우 `WKWebView`, 더 빠른 검색을 지 원하는입니다. `WKWebView` iOS 8 이상 버전 에서만 지원 됩니다.

기본적으로 Android에서 WebView은 약 기본 제공 브라우저 만큼 빠릅니다.

[UWP WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) Microsoft Edge 렌더링 엔진을 사용 합니다. 데스크톱 및 태블릿 장치 자체 Edge 브라우저를 사용 하 여 동일한 성능을 표시 됩니다.

## <a name="permissions"></a>사용 권한

에 대 한 순서 대로 `WebView` 작동 하도록 하는 각 플랫폼에 대 한 권한이 설정 되어 있는지 확인 해야 합니다. 일부 플랫폼에서는 유의 `WebView` 릴리스용으로 작성 된 때가 아니라 하지만 디버그 모드에서 작동 합니다. Android에서는 인터넷 액세스에 대 한 것과 같은 일부 권한을 디버그 모드일 때는 Mac 용 Visual Studio에서 기본적으로 설정 되어 때문입니다.

- **UWP** &ndash; 네트워크 콘텐츠를 표시할 때 인터넷 (클라이언트 및 서버) 기능이 필요 합니다.
- **Android** &ndash; 필요 `INTERNET` 네트워크에서 콘텐츠를 표시 하는 경우에 합니다. 로컬 콘텐츠 필요한 특별 한 권한이 없습니다.
- **iOS** &ndash; 필요한 특별 한 권한이 없습니다.

## <a name="layout"></a>레이아웃

다른 대부분 Xamarin.Forms 보기와 달리 `WebView` 있어야 `HeightRequest` 및 `WidthRequest` StackLayout 또는 RelativeLayout에 포함 된 때를 지정 합니다. 이러한 속성을 지정 하지 않으면는 `WebView` 렌더링 되지 것입니다.

다음 예에서는 작업을 발생 하는 레이아웃을 보여 줍니다. 렌더링 `WebView`s:

WidthRequest & HeightRequest StackLayout:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

WidthRequest & HeightRequest RelativeLayout:

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

그리드 *없이* WidthRequest & HeightRequest 합니다. 표는 요청 된 높이 너비를 지정 하지 않아도 되는 몇 가지 레이아웃 중 하나입니다.:

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


## <a name="related-links"></a>관련 링크

- [웹 보기 (샘플) 사용](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [웹 보기 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
