---
title: Razor 템플릿을 사용 하 여 빌드 HTML 보기
description: " 전체 화면 웹 페이지를 사용 하 여 HTML을 렌더링 하는 간단 하 고 효과적인 방법 특히 이미 있는 경우 HTML, JavaScript 및 CSS 웹 사이트 프로젝트에서 플랫폼 간 방식으로 복잡 한 서식 지정을 렌더링할 수 있습니다."
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: asb3993
ms.author: amburns
ms.date: 07/24/2018
ms.openlocfilehash: 539f59b9835cab6281327bcd1a37482ef82b62cc
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67650171"
---
# <a name="building-html-views-using-razor-templates"></a>Razor 템플릿을 사용 하 여 빌드 HTML 보기

모바일 개발 환경에서 "하이브리드 앱" 이라는 용어는 일반적으로 호스트 된 웹 뷰어 컨트롤 내에서 HTML 페이지와 해당 화면의 일부 (또는 모든)를 제공 하는 응용 프로그램에 참조 합니다.

하지만 수 있는 몇 가지 개발 환경이 모바일에서 앱 빌드 완전히 HTML 및 JavaScript로 이러한 앱은 복잡 한 처리 또는 UI 효과 수행 하려고 할 때 성능 문제가 저하 될 수 있습니다 하며 플랫폼에도 제한 기능에 액세스할 수 있습니다.

Xamarin은 Razor HTML 템플릿 엔진을 활용 하는 경우에 특히의 두 장점 제공 합니다. JavaScript 및 CSS를 사용 하지만 또한 기본 플랫폼 Api에 대 한 전체 액세스 권한이 하 고 빠른 사용 하 여 처리 하는 플랫폼 간 템플릿 기반 HTML 보기를 빌드할 수 있는 Xamarin을 사용 하 여 C#입니다.

이 문서에서는 Xamarin을 사용 하 여 모바일 플랫폼에서 사용할 수 있는 HTML + JavaScript + CSS 뷰를 작성 하는 Razor 템플릿 엔진을 사용 하는 방법을 설명 합니다.

## <a name="using-web-views-programmatically"></a>프로그래밍 방식으로 웹 보기를 사용 하 여

에서는 Razor에 알아봅니다 전에이 섹션에서는 HTML 콘텐츠를 직접 – 표시할 웹 뷰를 사용 하는 방법을 구체적으로 앱 내에서 생성 되는 HTML 콘텐츠를 다룹니다.

Xamarin 만들어 C#을 사용 하 여 HTML을 표시 하는 것이 간단 하므로 iOS 및 Android에서 기본 플랫폼 Api에 대 한 전체 액세스를 제공 합니다. 각 플랫폼에 대 한 기본 구문은 다음과 같습니다.

### <a name="ios"></a>iOS

코드 몇 줄을 사용도 Xamarin.iOS에서 UIWebView 컨트롤의 HTML을 표시 합니다.

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

참조 된 [UIWebView iOS](http://docs.xamarin.com/recipes/ios/content_controls/web_view/) 레시피 UIWebView 컨트롤을 사용 하 여 대 한 자세한 내용은 합니다.

### <a name="android"></a>Android

Xamarin.Android를 사용 하 여 WebView 컨트롤의 HTML 표시를 몇 줄의 코드로 수행 됩니다.

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);

// enable JavaScript execution in your html view so you can provide "alerts" and other js
webView.SetWebChromeClient(new WebChromeClient());

var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

참조 된 [Android WebView](http://docs.xamarin.com/recipes/android/controls/webview/) 레시피 WebView 컨트롤을 사용 하 여 대 한 자세한 내용은 합니다.

### <a name="specifying-the-base-directory"></a>기본 디렉터리를 지정합니다.

두 플랫폼 모두에서 HTML 페이지에 대 한 기본 디렉터리를 지정 하는 매개 변수가 있습니다. 이미지 및 CSS 파일과 같은 리소스에 대 한 상대 참조를 해결 하는 데 사용 되는 장치의 파일 시스템 위치입니다. 예를 들어 같은 태그

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

이러한 파일을 참조: **style.css**를 **monkey.jpg** 하 고 **jscript.js**합니다. 기본 디렉터리 설정 이러한 파일이 있는 페이지에 로드 될 수 있도록 웹 보기를 알려 줍니다.

#### <a name="ios"></a>iOS

템플릿 출력은 다음 C# 코드를 사용 하 여 iOS에서 렌더링 됩니다.

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

기본 디렉터리로 지정 됩니다 `NSBundle.MainBundle.BundleUrl` 는 디렉터리를 참조 하는 응용 프로그램이 설치 됩니다. 에 있는 모든 파일을 **리소스** 폴더와 같은이 위치에 복사 됩니다는 **style.css** 여기에 표시 된 파일:

 ![iPhoneHybrid 솔루션](images/image1_240x163.png)

모든 정적 콘텐츠 파일에 대 한 빌드 동작 **BundleResource**:

 ![iOS 프로젝트 빌드 작업: BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

Android는 기본 디렉터리는 웹 뷰의 html 문자열 표시 될 때 매개 변수로 전달할 수 있도록 해야 합니다.

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

특별 한 문자열 **file:///android_asset/** 앱에서 표시 된 다음 포함 된 Android 자산 폴더에 참조를 **style.css** 파일입니다.

 ![AndroidHybrid 솔루션](images/image3_240x167.png)

모든 정적 콘텐츠 파일에 대 한 빌드 동작 **AndroidAsset**합니다.

 ![Android 프로젝트 빌드 작업: AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>호출 C# HTML 및 JavaScript

Html 페이지는 웹 보기에 로드 되 면 처리 링크와 forms 서버에서 페이지가 로드 된 경우 처럼 합니다. 즉, 사용자가 링크를 클릭 하거나 폼을 전송 하는 경우 지정된 된 대상으로 이동 하는 웹 뷰의 시도 합니다.

(예: google.com) 외부 서버에 연결 되어 있으면 웹 보기는 외부 웹 사이트 (인터넷 연결이 있는지 가정)를 로드를 시도 합니다.

```html
<a href="http://google.com/">Google</a>
```

링크는 상대적 기본 디렉터리에서 해당 콘텐츠를 로드 하는 웹 뷰의 하려고 합니다. 물론 네트워크 연결이 없습니다이 작동 하려면 필요한 콘텐츠 장치의 앱에 저장 됩니다.

```html
<a href="somepage.html">Local content</a>
```

폼 작업 동일한 규칙을 따릅니다.

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

클라이언트에서 웹 서버를 호스트 하려는 없습니다. 그러나 사용 하 여 오늘날의 반응 형 디자인 패턴에서 사용 되는 동일한 서버 통신 기술은 HTTP GET을 통해 서비스를 호출 하 고 JavaScript를 내보내 응답을 비동기적으로 처리 (또는 호출 JavaScript는 웹 뷰의 이미 호스트). 이 옵션을 사용 하면 HTML의 처리 후 결과 HTML 페이지의 다시 표시에 대 한 C# 코드에 다시 데이터를 쉽게 전달할 수 있습니다.

IOS 및 Android 앱 코드는 (필요한 경우)에 응답할 수 있도록 이러한 탐색 이벤트를 응용 프로그램 코드에 대 한 메커니즘을 제공 합니다. 이 기능은 웹 보기와 상호 작용 하는 네이티브 코드를 수 있기 때문에 하이브리드 앱을 구축 하는 것이 중요 합니다.

#### <a name="ios"></a>iOS

응용 프로그램 코드 (예: 링크 클릭) 탐색 요청을 처리할 수 있도록 iOS에서 웹 뷰 ShouldStartLoad 이벤트를 재정의할 수 있습니다. 모든 정보를 제공 하는 메서드 매개 변수

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

및 다음 이벤트 처리기를 할당 합니다.

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

Android에서 단순히 하위 클래스는 WebViewClient입니다 차례로 탐색 요청에 응답 하는 코드를 구현 합니다.

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, IWebResourceRequest request) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

및 다음 웹 보기에서 클라이언트를 설정 합니다.

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>JavaScript에서 호출C#

새 HTML 페이지를 로드 하 게 웹 보기를 지시 하는 것 외에도 C# 코드에서 현재 표시 된 페이지 내에서 JavaScript를 실행할 수도 있습니다. 사용 하 여 전체 JavaScript 코드 블록을 만들 수 있습니다 C# 문자열 및 실행 또는 javascript를 통해 페이지에서 이미 사용할 수 있는 메서드 호출을 만들 수 있습니다 `script` 태그입니다.

#### <a name="android"></a>Android

JavaScript 코드를 실행할 수 있으며 그런 다음 앞에 접두사를 사용 하 여 만드는 "javascript:" 웹 보기를 해당 문자열을 로드 하도록 지시 합니다.

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

iOS 웹 보기에는 특히 JavaScript를 호출 하는 방법을 제공 합니다.

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>요약

이 섹션에서는 Xamarin 사용 하 여 하이브리드 응용 프로그램을 빌드할 수 있도록 하는 iOS 및 Android 모두에 웹 보기 컨트롤의 기능을 소개 했습니다 포함:

-  코드에서 생성 하는 문자열에서 HTML을 로드 하는 기능
-  로컬 파일 (CSS, JavaScript, 이미지 또는 기타 HTML 파일)을 참조 하는 기능
-  C# 코드에서 탐색 요청을 가로챌 수 있는 기능
-  JavaScript에서 호출 하는 기능은 C# 코드입니다.


다음 섹션에서는 쉽게 하이브리드 앱에서 사용 하기 위해 HTML을 만들 수 있도록 하는 Razor를 소개 합니다.

## <a name="what-is-razor"></a>Razor는 무엇입니까?

Razor는 템플릿 엔진 서버에서 실행 하 고 웹 브라우저에 게 제공 하는 HTML을 생성 하려면 원래 ASP.NET MVC를 사용 하 여 도입 된 경우

Razor 템플릿 엔진을 사용 하 여 표준 HTML 구문을 확장 C# express 레이아웃 및 CSS 스타일 시트 및 JavaScript를 쉽게 통합할 수 있도록 합니다. 템플릿은 속성이 서식 파일에서 직접 액세스할 수 있습니다 및 사용자 지정 형식일 수 있는 모델 클래스를 참조할 수 있습니다. 주요 이점 중 하나는 쉽게 HTML 및 C# 구문을 혼합 기능입니다.

Razor 템플릿 서버 쪽 사용에 제한 되지 않습니다., Xamarin 앱에 포함 될 수도 있습니다. 프로그래밍 방식으로 웹 보기를 사용 하는 기능과 더불어 Razor 템플릿을 사용 하 여 정교한 플랫폼 간 하이브리드 응용 프로그램을 Xamarin을 사용 하 여 빌드할 수 있습니다.

### <a name="razor-template-basics"></a>Razor 템플릿 기본 사항

Razor 템플릿 파일을 **.cshtml** 파일 확장명입니다. 텍스트 템플릿 섹션에서 Xamarin 프로젝트에 추가 합니다 **새 파일** 대화 상자:

 ![새 파일-Razor 템플릿](images/image5_400x201.png)

간단한 Razor 템플릿 ( **RazorView.cshtml**) 아래에 표시 됩니다.

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

일반 HTML 파일에서 다음과 같은 차이점을 확인 합니다.

-  `@` 기호 Razor 템플릿에서 특별 한 의미가 – 다음 식은 C# 평가할 나타냅니다.
- `@model` 지시문은 항상 Razor 템플릿 파일의 첫 번째 줄으로 표시 됩니다.
-  `@model` 지시문 뒤에 야 형식입니다. 이 예제에서는 간단한 문자열을 템플릿에 전달 되는 하지만 사용자 지정 클래스도 될 수 없습니다.
-  때 `@Model` 참조 하는 템플릿 전체에서 (이 예에서는 문자열)에서 생성 되 면 템플릿에 전달 된 개체에 대 한 참조를 제공 합니다.
-  IDE에서 템플릿에 대 한 partial 클래스를 자동으로 생성 됩니다 (사용 하 여 파일을 **.cshtml** 확장). 이 코드를 볼 수 있지만 편집할 수는 없습니다.
 ![RazorView.cshtml](images/image6_125x34.png) partial 클래스는.cshtml 템플릿 파일 이름과 일치 하도록 RazorView 라고 합니다. C# 코드에서 템플릿을 참조 하는 데 사용 되는이 이름입니다.
- `@using` 문을 추가 네임 스페이스를 포함 하는 Razor 템플릿 맨 위에 있는 포함 될 수 있습니다.


그런 다음 다음 C# 코드를 사용 하 여 최종 HTML 출력을 생성할 수 있습니다. Note 문자열 "Hello World" 렌더링 된 템플릿 출력에 통합 됩니다는 모델을 지정 했습니다.

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

IOS 시뮬레이터 및 Android 에뮬레이터에서 웹 보기에 표시 된 출력은 다음과 같습니다.

 ![Hello World](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>자세한 Razor 구문

이 섹션을 시작 하는 데 몇 가지 기본 Razor 구문 소개 하겠습니다 사용 합니다. 이 섹션의 예제 데이터를 사용 하 여 다음 클래스를 채울 Razor를 사용 하 여 표시 합니다.

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

모든 예제에서는 다음 데이터 초기화 코드를 사용합니다.

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>모델 속성 표시

모델 속성을 사용 하는 경우이 예제에서는 템플릿에 표시 된 것 처럼 Razor 템플릿에서 참조 쉽게 된:

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    </body>
</html>
```

이 다음 코드를 사용 하 여 문자열로 렌더링할 수 있습니다.

```csharp
var template = new RazorView () { Model = animal };
var page = template.GenerateString ();
```

최종 출력은 다음과 같습니다. iOS 시뮬레이터 및 Android 에뮬레이터에서는 웹 뷰의

 ![루 퍼 트](images/image8_516x160.png)

#### <a name="c-statements"></a>C# 문

더 복잡 한 C# 모델 속성 업데이트 등이 예제에서 Age 계산 템플릿에 포함할 수 있습니다.

```html
@model Monkey
<html>
    <body>
    @{
        Model.Name = "Rupert X. Monkey";
        Model.Birthday = new DateTime(2011,3,1);
    }
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    </body>
</html>
```

사용 하 여 코드를 에워싸는 방식으로 복잡 한 줄으로 C# 식 (예: 기간 형식)을 작성할 수 있습니다 `@()`합니다.

사용 하 여 묶어 여러 C# 문을 작성할 수 있습니다 `@{}`합니다.

#### <a name="if-else-statements"></a>If else 문

코드 분기를 사용 하 여 표현 될 수 있습니다 `@if` 이 템플릿 예제에 표시 된 대로 합니다.

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <p>@Model.FavoriteFoods.Count favorites</p>
    }
    </body>
</html>
```

#### <a name="loops"></a>루프

루프와 같은 구문을 `foreach` 추가할 수도 있습니다. `@` 루프 변수에서 접두사를 사용할 수 있습니다 ( `@food` 여기서) html에서 렌더링 합니다.

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <ul>
            @foreach (var food in @Model.FavoriteFoods) {
                <li>@food</li>
            }
        </ul>
    }
    </body>
</html>
```

위의 템플릿의 출력에는 iOS 시뮬레이터 및 Android 에뮬레이터에서 실행 중인 표시 됩니다.

 ![루 퍼 트 X Monkey](images/image9_520x277.png)

이 섹션에서는 Razor 템플릿을 사용 하 여 간단한 읽기 전용 뷰를 렌더링 하는 기본적인 방법을 설명 했습니다. 다음 섹션에는 사용자 입력을 허용 하 고 HTML 보기의 JavaScript에서는 상호 운용할 수 있는 Razor를 사용 하 여 전체 앱을 빌드하는 방법을 설명 하 고 C#입니다.

## <a name="using-razor-templates-with-xamarin"></a>Razor 템플릿을 사용 하 여 Xamarin을 사용 하 여

이 섹션에서는 mac 용 Visual Studio에서 솔루션 템플릿을 사용 하 여 하이브리드 응용 프로그램 빌드를 사용 하는 방법에 설명 사용할 수 있는 세 가지 템플릿을 가지는 **파일 > 새로 만들기 > 솔루션...**  창:

- **Android > 앱 > Android WebView 응용 프로그램**
- **iOS > 앱 > WebView 응용 프로그램**
- **ASP.NET MVC 프로젝트**



합니다 **새 솔루션** iPhone 및 Android 프로젝트에 대 한 다음과 같은 창이 나타나는-오른쪽에 있는 솔루션 설명을 Razor 템플릿 엔진에 대 한 지원 강조 표시 합니다.

 ![IPhone 및 Android 솔루션 만들기](images/image13_1139x959.png)

쉽게 추가할 수 있는 참고를 **.cshtml** Razor 템플릿을 *모든* 기존 Xamarin 프로젝트에서 필요 없는 이러한 솔루션 템플릿을 사용 하 여 합니다. iOS 프로젝트 하거나; Razor를 사용 하는 스토리 보드를 필요 하지 않습니다. 단순히 UIWebView 컨트롤 보기에 프로그래밍 방식으로 추가 하 고 Razor 템플릿을 전체 C# 코드에서 렌더링할 수 있습니다.

IPhone 및 Android 프로젝트에 대 한 기본 서식 파일 솔루션 콘텐츠는 다음과 같습니다.

 ![iPhone 및 Android 템플릿](images/image10_428x310.png)

템플릿은 데이터 모델 개체를 사용 하 여 Razor 템플릿 로드, 사용자 입력을 처리 및 JavaScript 통해 사용자에 게 다시 통신을 준비-go 응용 프로그램 인프라를 제공 합니다.

솔루션의 중요 한 부분은 다음과 같습니다.

-  와 같은 정적 콘텐츠를 **style.css** 파일입니다.
-  Razor 템플릿 파일을.cshtml **RazorView.cshtml** 합니다.
-  클래스와 같은 Razor 템플릿에서 참조 하는 모델링할 **ExampleModel.cs** 합니다.
-  웹 보기를 만들고와 같은 서식 파일을 렌더링 하는 플랫폼 관련 클래스를 `MainActivity` android 및 `iPhoneHybridViewController` iOS에서.


다음 섹션에서는 프로젝트 작동 하는 방법을 설명 합니다.

### <a name="static-content"></a>정적 콘텐츠

정적 콘텐츠는 CSS 스타일 시트, 이미지, JavaScript 파일 또는에서 연결 되거나 웹 보기에 표시 되는 HTML 파일에서 참조 될 수 있는 기타 콘텐츠를 포함 합니다.

서식 파일 프로젝트에는 최소한의 스타일 시트를 하이브리드 앱에 정적 콘텐츠를 포함 하는 방법을 보여 주기 위해 포함 됩니다. CSS 스타일 시트는 다음과 같은 템플릿에서 참조 됩니다.

```html
<link rel="stylesheet" href="style.css" />
```

모든 스타일 시트 및 JQuery와 같은 프레임 워크를 포함 해야 하는 JavaScript 파일을 추가할 수 있습니다.

### <a name="razor-cshtml-templates"></a>Razor cshtml 템플릿

Razor를 포함 하는 템플릿을 **.cshtml** 에 미리 작성 된 코드가 HTML/JavaScript 간에 데이터를 통신 하는 데는 파일 및 C#합니다. 이 수 하지만 모델에서 읽기 전용 데이터를 표시 하지만 HTML의 사용자 입력을 허용 및 전달 되는 복잡 한 하이브리드 앱 처리 또는 저장을 위해 C# 코드를 빌드.

#### <a name="rendering-the-template"></a>템플릿을 렌더링합니다.

호출 된 `GenerateString` 템플릿에서 HTML 렌더링 웹 보기에 표시 하기 위해 준비 합니다. 서식 파일에서 렌더링 하기 전에 제공 해야 하는 다음 모델을 사용 합니다. 이 다이어그램에서는 렌더링 작동 방식 – 하지 정적 리소스를 제공 된 기본 디렉터리를 사용 하 여 지정 된 파일을 찾는 데 런타임 시 웹 보기 해결 되는 보여 줍니다.

 ![Razor 순서도](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>템플릿에서 C# 코드를 호출합니다.

웹 보기에 대 한 URL을 설정 하 고 다음에 웹 보기를 다시 로드 하지 않고 기본 요청을 처리 하려면 C#에서 요청을 가로채 통신 C#에 다시 호출 하는 렌더링 된 웹 보기에서 수행 됩니다.

예제에서 RazorView의 단추를 처리 하는 방법을 볼 수 있습니다. 단추에는 다음 HTML에 있습니다.

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

합니다 `InvokeCSharpWithFormValues` JavaScript 함수 집합과 HTML 양식에서 모든 값을 읽고는 `location.href` 웹 보기:

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

웹 보기 (예: 사용자 지정 체계를 사용 하 여 URL로 이동 하려고이 `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

기본 웹 보기에서이 탐색 요청을 처리할 때이 차단할 수가 있습니다. Ios에서 UIWebView의 HandleShouldStartLoad 이벤트를 처리 하면 됩니다. Android에서에서는 단순히 서브 클래스는 WebViewClient 양식에서 사용 하 고 ShouldOverrideUrlLoading를 재정의 합니다.

이러한 두 탐색 인터셉터의 내부 구조는 거의 동일 합니다.

먼저 웹 보기를 로드 하려고 하는 URL을 확인 및 사용자 지정 체계로 시작 하지 않는 경우 (`hybrid:`)를 정상적으로 탐색을 허용 합니다.

체계 간 URL에 모든 사용자 지정 URL 구성표와 "?" (이 경우 "UpdateLabel")에서 처리 해야 할 메서드 이름이입니다. 쿼리 문자열의 모든 메서드 호출에 매개 변수로 처리 됩니다.

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` 이 샘플에는 최소한의 문자열 조작 ("C# 가" 문자열 앞), 텍스트 매개 변수에 않으며 웹 보기를 다시 호출 합니다.

URL을 처리 한 후 메서드는 웹 뷰의 사용자 지정 URL로 이동 완료 하려고 시도 하지 않습니다 있도록 탐색을 중단 합니다.

#### <a name="manipulating-the-template-from-c"></a>C#에서 템플릿을 조작

통신에서 렌더링된 된 HTML 웹 뷰를 C# 웹 보기에서 JavaScript를 호출 하 여 수행 됩니다. Ios의 경우 이렇게 호출 하 여 `EvaluateJavascript` 는 UIWebView에서:

```csharp
webView.EvaluateJavascript (js);
```

Android에서는 JavaScript에서에서 호출할 수 있습니다 웹 뷰는 JavaScript를 사용 하 여 URL을 로드 하 여는 `"javascript:"` URL 체계:

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>앱을 만드는 진정한 하이브리드

이러한 템플릿은 없도록 각 플랫폼에 네이티브 컨트롤 사용 – 단일 웹 보기를 사용 하 여 전체 화면 채워집니다.

HTML 프로토타입 생성에 대 한 유용할 수 있습니다 이며 작업의 종류를 표시 합니다. 웹 서식 있는 텍스트 및 반응 형 레이아웃 등 가장 합니다. 하지만 일부 작업은 HTML 및 JavaScript – 데이터의 긴 목록을 스크롤할에 적합 한 더 잘 (iOS에서 UITableView) 또는 ListView와 같은 네이티브 UI 컨트롤을 사용 하 여 Android에서 예를 들어 수행 합니다.

웹 보기 템플릿에서 플랫폼 특정 컨트롤을 간단히 편집을 사용 하 여 쉽게 확장할 수 있습니다 합니다 **MainStoryboard.storyboard** iOS 디자이너에서 또는 **Resources/layout/Main.axml** Android에서.

### <a name="razortodo-sample"></a>RazorTodo 샘플

합니다 [RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo) 리포지토리 완전히 HTML 기반 앱과 네이티브 컨트롤을 사용 하 여 HTML을 결합 하는 앱 간의 차이 표시 하려면 두 가지 별도 솔루션을 포함 합니다.

-  **RazorTodo** -Razor 템플릿을 사용 하 여 완전히 HTML 기반 앱.
-  **RazorNativeTodo** -iOS 및 Android 용 네이티브 목록 뷰 컨트롤을 사용 하지만 HTML 및 Razor에 편집 화면이 표시 됩니다.


Xamarin 앱이 iOS 및 Android에서 이식 가능한 클래스 라이브러리 (Pcl) 데이터베이스 및 모델 클래스와 같은 일반적인 코드 공유를 활용 하 여 둘 다에서 실행 합니다. Razor **.cshtml** 템플릿을 쉽게 플랫폼 간에 공유 되지 있도록 PCL에 포함할 수도 있습니다.

샘플 앱을 모두 Twitter에서 공유 및 네이티브 플랫폼에서 Xamarin 사용한 하이브리드 응용 프로그램 계속 액세스할 있는지 모든 기본 기능에 HTML Razor 템플릿 기반 보기에서 보여 주는 텍스트 음성 변환 Api를 통합 합니다.

합니다 **RazorTodo** 앱 목록 및 편집 보기에 대 한 HTML Razor 템플릿을 사용 합니다. 즉, 앱을 공유 하는 이식 가능한 클래스 라이브러리에 거의 완전히 구축할 수 있습니다 (데이터베이스를 포함 하 고 **.cshtml** Razor 템플릿). 아래 스크린샷에서 iOS 및 Android 앱을 보여 줍니다.

 ![RazorTodo](images/Both_700x290.png)

합니다 **RazorNativeTodo** 앱을 HTML Razor 템플릿 편집 뷰를 사용 하지만 각 플랫폼에서 네이티브 스크롤 목록을 구현 합니다. 이 다양을 한 이점 포함 하 여 제공 합니다.

-  성능-네이티브 스크롤 컨트롤 데이터의 매우 긴 목록을 사용 하 여 더 빠르고 부드러운 스크롤 되도록 가상화를 사용 합니다.
-  네이티브 환경-플랫폼별 UI 요소를 쉽게 같은 iOS 및 Android에서 빠른 스크롤 인덱스 지원을 사용 하도록 설정 합니다.


Xamarin 사용 하 여 하이브리드 앱을 구축 하는 주요 이점은 완전히 HTML 기반 사용자 인터페이스 (예: 첫 번째 예제)를 사용 하 여 시작 하 고 추가한 다음 (두 번째 예제와) 같이 필요한 경우 플랫폼별 기능 수 있는지는 합니다. 기본 목록 화면 및 HTML Razor 모두 iOS의 화면이 편집한 Android 아래에 표시 됩니다.

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>요약

이 문서에서는 iOS 및 Android 빌드를 쉽게 수행할 수 있는 사용 가능한 웹 뷰 컨트롤의 기능을 설명 했습니다 하이브리드 응용 프로그램입니다.

다음 Razor 템플릿 엔진 및 HTML을 쉽게 사용 하 여 Xamarin 앱에서 생성 하는 구문을 설명 합니다. **cshtml** Razor 템플릿 파일입니다. 또한 솔루션 템플릿을 사용 하면 빠르게는 Xamarin 사용 하 여 하이브리드 응용 프로그램을 빌드하기 시작 하는 Mac 용 Visual Studio를 설명 합니다.

마지막으로 네이티브 사용자 인터페이스 및 Api를 사용 하 여 웹 뷰를 결합 하는 방법을 보여 주는 RazorTodo 샘플을 도입 되었습니다.

### <a name="related-links"></a>관련 링크

- [RazorTodo 샘플](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3-Razor 보기 엔진 (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Razor 구문 (Microsoft)를 사용 하 여 ASP.NET 웹 프로그래밍 소개](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
