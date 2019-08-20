---
title: Razor 템플릿을 사용 하 여 HTML 뷰 빌드
description: " HTML을 렌더링 하기 위해 전체 화면 웹 페이지를 사용 하는 것은 특히 웹 사이트 프로젝트의 HTML, JavaScript 및 CSS가 이미 있는 경우 플랫폼 간 방식으로 복잡 한 서식을 렌더링 하는 간단 하 고 효과적인 방법입니다."
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: asb3993
ms.author: amburns
ms.date: 07/24/2018
ms.openlocfilehash: d822a4dc50d3f33ba4c217b8fcc557acc2bfdb3e
ms.sourcegitcommit: 0df727caf941f1fa0aca680ec871bfe7a9089e7c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69621030"
---
# <a name="building-html-views-using-razor-templates"></a>Razor 템플릿을 사용 하 여 HTML 뷰 빌드

모바일 개발 분야에서 "하이브리드 앱" 이란 용어는 일반적으로 호스트 된 웹 뷰어 컨트롤 내에서 일부 (또는 모든)의 화면을 HTML 페이지로 표시 하는 응용 프로그램을 의미 합니다.

HTML 및 JavaScript를 사용 하 여 모바일 앱을 빌드할 수 있는 몇 가지 개발 환경이 있지만 이러한 앱은 복잡 한 처리 나 UI 효과를 달성 하려고 할 때 성능 문제를 발생 시킬 수 있으며 플랫폼 에서도 제한 됩니다. 사용자가 액세스할 수 있는 기능입니다.

Xamarin은 특히 Razor HTML 템플릿 엔진을 활용 하는 경우 둘 다를 위한 최상의 방법을 제공 합니다. Xamarin을 사용 하면 JavaScript 및 CSS를 사용 하는 플랫폼 간 템플릿 기반 HTML 뷰를 유연 하 게 빌드할 수 있을 뿐만 아니라을 사용 하 여 C#기본 플랫폼 api에 대 한 완전 한 액세스와 빠른 처리를 사용할 수 있습니다.

이 문서에서는 Razor 템플릿 엔진을 사용 하 여 Xamarin을 사용 하는 모바일 플랫폼에서 사용할 수 있는 HTML + JavaScript + CSS 뷰를 빌드하는 방법을 설명 합니다.

## <a name="using-web-views-programmatically"></a>프로그래밍 방식으로 웹 보기 사용

Razor에 대해 알아보기 전에이 섹션에서는 웹 보기를 사용 하 여 HTML 콘텐츠를 직접 표시 하는 방법에 대해 설명 합니다. 특히 앱 내에서 생성 되는 HTML 콘텐츠입니다.

Xamarin은 iOS와 Android 모두에서 기본 플랫폼 Api에 대 한 완전 한 액세스를 제공 하므로를 사용 하 여 C#HTML을 쉽게 만들고 표시할 수 있습니다. 각 플랫폼에 대 한 기본 구문은 다음과 같습니다.

### <a name="ios"></a>iOS

Xamarin.ios의 UIWebView 보기 컨트롤에 HTML을 표시 하면 다음과 같은 몇 줄의 코드도 사용 됩니다.

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

UIWebView 보기 컨트롤을 사용 하는 방법에 대 한 자세한 내용은 [IOS UIWebView 보기](http://docs.xamarin.com/recipes/ios/content_controls/web_view/) 조리법을 참조 하세요.

### <a name="android"></a>Android

Xamarin을 사용 하 여 웹 보기 컨트롤에 HTML을 표시 하는 것은 몇 줄의 코드로 수행 됩니다.

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);

// enable JavaScript execution in your html view so you can provide "alerts" and other js
webView.SetWebChromeClient(new WebChromeClient());

var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

웹 보기 컨트롤 사용에 대 한 자세한 내용은 [Android 웹 보기](http://docs.xamarin.com/recipes/android/controls/webview/) 조리법을 참조 하세요.

### <a name="specifying-the-base-directory"></a>기본 디렉터리 지정

두 플랫폼 모두에 HTML 페이지의 기본 디렉터리를 지정 하는 매개 변수가 있습니다. 이미지 및 CSS 파일과 같은 리소스에 대 한 상대 참조를 확인 하는 데 사용 되는 장치의 파일 시스템 위치입니다. 예를 들어 다음과 같은 태그

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

이러한 파일을 참조 하세요. **.css**, **원숭이 jpg** 및 **jscript.** 기본 디렉터리 설정은이 파일이 페이지에 로드 될 수 있도록 해당 파일이 있는 위치를 웹 뷰에 알려 줍니다.

#### <a name="ios"></a>iOS

다음 C# 코드를 사용 하 여 템플릿 출력이 iOS에서 렌더링 됩니다.

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

기본 디렉터리는 응용 프로그램이 설치 `NSBundle.MainBundle.BundleUrl` 된 디렉터리를 참조 하는로 지정 됩니다. **Resources** 폴더의 모든 파일이 다음 위치에 복사 됩니다. 예를 들어 여기에 표시 된 **스타일 .css 파일입니다** .

 ![iPhoneHybrid 솔루션](images/image1_240x163.png)

모든 정적 콘텐츠 파일의 빌드 작업은 **BundleResource**이어야 합니다.

 ![iOS 프로젝트 빌드 작업: BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

또한 웹 보기에 html 문자열이 표시 되는 경우 Android에서 기본 디렉터리를 매개 변수로 전달 해야 합니다.

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

특수 문자열 **file:///android_asset/** 는 응용 프로그램의 android 자산 폴더를 참조 합니다. 여기에는 **스타일 .css** 파일이 포함 되어 있습니다.

 ![AndroidHybrid 솔루션](images/image3_240x167.png)

모든 정적 콘텐츠 파일의 빌드 작업은 **Androidasset**이어야 합니다.

 ![Android 프로젝트 빌드 작업: AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>HTML C# 및 JavaScript에서 호출

Html 페이지가 웹 뷰로 로드 되 면 페이지를 서버에서 로드 하는 경우와 마찬가지로 링크와 폼을 처리 합니다. 즉, 사용자가 링크를 클릭 하거나 양식을 전송 하면 웹 보기는 지정 된 대상으로 이동 하려고 합니다.

외부 서버 (예: google.com)에 대 한 링크의 경우 웹 보기는 외부 웹 사이트 (인터넷 연결이 있는 것으로 가정)를 로드 하려고 합니다.

```html
<a href="http://google.com/">Google</a>
```

링크를 기준으로 하는 경우에는 웹 뷰가 기본 디렉터리에서 해당 콘텐츠를 로드 하려고 시도 합니다. 이 작업을 수행 하려면 네트워크 연결이 필요 하지 않습니다. 콘텐츠가 장치의 앱에 저장 되기 때문입니다.

```html
<a href="somepage.html">Local content</a>
```

양식 작업은 동일한 규칙을 따릅니다.

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

클라이언트에서 웹 서버를 호스팅하지 않습니다. 그러나 현재 응답성이 뛰어난 디자인 패턴에서 사용 되는 것과 동일한 서버 통신 기술을 사용 하 여 HTTP GET을 통해 서비스를 호출 하 고 JavaScript를 내보내거나 웹 뷰에서 이미 호스팅된 JavaScript를 호출 하 여 비동기적으로 응답을 처리할 수 있습니다. 이렇게 하면 처리를 위해 HTML에서 코드에 C# 데이터를 쉽게 전달 하 고 결과를 다시 html 페이지에 표시할 수 있습니다.

IOS와 Android 모두 응용 프로그램 코드에서 이러한 탐색 이벤트를 가로채는 메커니즘을 제공 하 여 앱 코드가 응답 (필요한 경우) 할 수 있도록 합니다. 이 기능은 네이티브 코드가 웹 뷰와 상호 작용할 수 있으므로 하이브리드 앱을 빌드하는 데 중요 합니다.

#### <a name="ios"></a>iOS

응용 프로그램 코드가 탐색 요청 (예: 링크 클릭)을 처리할 수 있도록 하기 위해 iOS의 웹 뷰에서 ShouldStartLoad 이벤트를 재정의할 수 있습니다. 메서드 매개 변수는 모든 정보를 제공 합니다.

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

그런 다음 이벤트 처리기를 할당 합니다.

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

Android에서는 간단히 WebViewClient를 서브클래싱하 고 탐색 요청에 응답 하는 코드를 구현 합니다.

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, IWebResourceRequest request) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

그런 다음 웹 보기에서 클라이언트를 설정 합니다.

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>C에서 JavaScript 호출\#

웹 보기에 새 HTML 페이지를 로드 하는 것 외에도 C# 코드는 현재 표시 된 페이지 내에서 JavaScript를 실행할 수 있습니다. 전체 javascript 코드 블록은 문자열을 사용 C# 하 여 생성 하거나 실행할 수 있습니다. 또는 태그를 통해 `script` 페이지에서 이미 사용할 수 있는 javascript에 대 한 메서드 호출을 만들 수 있습니다.

#### <a name="android"></a>Android

실행할 JavaScript 코드를 만든 다음 "javascript:"로 접두사로 붙이고 웹 보기에 해당 문자열을 로드 하도록 지시 합니다.

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

iOS 웹 보기는 JavaScript를 호출 하는 메서드를 제공 합니다.

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>요약

이 섹션에서는 다음을 포함 하 여 Xamarin을 사용 하 여 하이브리드 응용 프로그램을 빌드할 수 있도록 Android 및 iOS에서 웹 보기 컨트롤의 기능을 소개 했습니다.

- 코드에 생성 된 문자열에서 HTML을 로드 하는 기능
- 로컬 파일 (CSS, JavaScript, 이미지 또는 기타 HTML 파일)을 참조할 수 있습니다.
- 코드에서 C# 탐색 요청을 가로챌 수 있는 기능
- 코드에서 C# JavaScript를 호출 하는 기능입니다.


다음 섹션에서는 Razor를 소개 하며,이를 통해 하이브리드 앱에서 사용할 HTML을 쉽게 만들 수 있습니다.

## <a name="what-is-razor"></a>Razor 란?

Razor는 처음에 서버에서 실행 되 고 웹 브라우저에 제공할 HTML을 생성 하는 ASP.NET MVC와 함께 도입 된 템플릿 엔진입니다.

Razor 템플릿 엔진은를 사용 C# 하 여 표준 HTML 구문을 확장 하므로 레이아웃을 표현 하 고 CSS 스타일 시트와 JavaScript를 쉽게 통합할 수 있습니다. 템플릿은 사용자 지정 형식이 될 수 있으며 템플릿에서 직접 액세스할 수 있는 모델 클래스를 참조할 수 있습니다. 주요 이점 중 하나는 HTML과 C# 구문을 쉽게 혼합할 수 있는 기능입니다.

Razor 템플릿은 서버 쪽 사용으로 제한 되지 않으며 Xamarin 앱에도 포함 될 수 있습니다. 웹 뷰로 작업 하는 기능과 함께 Razor 템플릿을 사용 하면 Xamarin을 사용 하 여 정교한 플랫폼 간 하이브리드 응용 프로그램을 빌드할 수 있습니다.

### <a name="razor-template-basics"></a>Razor 템플릿 기본 사항

Razor 템플릿 파일의 확장명은 **cshtml** 입니다. **새 파일** 대화 상자의 텍스트 템플릿 섹션에서 Xamarin 프로젝트에 추가할 수 있습니다.

 ![새 파일-Razor 템플릿](images/image5_400x201.png)

간단한 Razor 템플릿 ( **Razorview. cshtml**)이 아래에 나와 있습니다.

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

일반 HTML 파일과 다음과 같은 차이점을 확인 합니다.

- 기호 `@` 는 Razor 템플릿에서 특별 한 의미를 가지 며, 다음 식을 C# 계산 함을 나타냅니다.
- `@model`지시문은 항상 Razor 템플릿 파일의 첫 번째 줄로 표시 됩니다.
- `@model` 지시문 뒤에는 형식이와 야 합니다. 이 예제에서는 간단한 문자열이 템플릿에 전달 되지만 사용자 지정 클래스 일 수도 있습니다.
- 는 `@Model` 템플릿 전체에서 참조 되는 경우 생성 될 때 템플릿에 전달 되는 개체에 대 한 참조를 제공 합니다 (이 예제에서는 문자열이 됩니다).
- IDE는 템플릿 (확장명이 **cshtml** 인 파일)에 대 한 partial 클래스를 자동으로 생성 합니다. 이 코드는 볼 수 있지만 편집할 수는 없습니다.
 ![Razorview. cshtml](images/image6_125x34.png) partial 클래스의 이름을 razorview로 지정 하 여. cshtml 템플릿 파일 이름과 일치 시킵니다. 이 이름은 코드에서 C# 템플릿을 참조 하는 데 사용 됩니다.
- `@using`문을 Razor 템플릿 맨 위에 포함 하 여 추가 네임 스페이스를 포함할 수도 있습니다.


그런 다음 다음 C# 코드를 사용 하 여 최종 HTML 출력을 생성할 수 있습니다. 모델은 렌더링 된 템플릿 출력에 포함 되는 "Hello World" 문자열로 지정 됩니다.

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

다음은 iOS 시뮬레이터 및 Android Emulator의 웹 보기에 표시 되는 출력입니다.

 ![Hello World](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>추가 Razor 구문

이 섹션에서는 사용을 시작 하는 데 도움이 되는 몇 가지 기본 Razor 구문 소개 하겠습니다. 이 단원의 예제에서는 다음 클래스를 데이터로 채우고 Razor를 사용 하 여 표시 합니다.

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

모든 예제에서는 다음 데이터 초기화 코드를 사용 합니다.

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>모델 속성 표시

모델은 속성이 있는 클래스인 경우이 예제 템플릿과 같이 Razor 템플릿에서 쉽게 참조 됩니다.

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    </body>
</html>
```

다음 코드를 사용 하 여 문자열로 렌더링할 수 있습니다.

```csharp
var template = new RazorView () { Model = animal };
var page = template.GenerateString ();
```

최종 출력은 iOS 시뮬레이터 및 Android Emulator에 대 한 웹 보기에 표시 됩니다.

 ![고가](images/image8_516x160.png)

#### <a name="c-statements"></a>C#할당문

이 예 C# 에서는 모델 속성 업데이트 및 보존 기간 계산과 같은 보다 복잡 한 템플릿이 템플릿에 포함 될 수 있습니다.

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

코드를와 함께 사용 C# `@()`하 여 복잡 한 단일 선 식 (연령 서식 지정)을 작성할 수 있습니다.

를 C# 사용 `@{}`하 여 여러 문을 쓸 수 있습니다.

#### <a name="if-else-statements"></a>If else 문

코드 분기는이 템플릿 예제 `@if` 와 같이로 표현 될 수 있습니다.

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

#### <a name="loops"></a>하며

같은 `foreach` 루핑 구문을 추가할 수도 있습니다. 루프 변수 ( `@food` 이 경우)에 접두사를사용하여HTML로렌더링할수있습니다.`@`

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

위의 템플릿 출력은 iOS 시뮬레이터 및 Android Emulator에서 실행 되는 것으로 표시 됩니다.

 ![이에 해당 하는 pert X 원숭이](images/image9_520x277.png)

이 섹션에서는 간단한 읽기 전용 보기를 렌더링 하기 위해 Razor 템플릿 사용에 대 한 기본 사항을 설명 했습니다. 다음 섹션에서는 HTML 뷰 및 C#에서 JavaScript 간에 사용자 입력을 허용 하 고 상호 운용할 수 있는 Razor를 사용 하 여 더 많은 앱을 빌드하는 방법을 설명 합니다.

## <a name="using-razor-templates-with-xamarin"></a>Xamarin에서 Razor 템플릿 사용

이 섹션에서는 Mac용 Visual Studio의 솔루션 템플릿을 사용 하 여 고유한 하이브리드 응용 프로그램 빌드를 사용 하는 방법을 설명 합니다. **새 > 솔루션 > 파일** 에서 사용할 수 있는 세 가지 템플릿이 있습니다. 창:

- **Android > 앱 > Android 웹 보기 응용 프로그램**
- **iOS > 앱 > 웹 보기 응용 프로그램**
- **ASP.NET MVC 프로젝트**



**새 솔루션** 창은 IPhone 및 Android 프로젝트에 대 한 것 처럼 보입니다. 오른쪽에 있는 솔루션 설명에는 Razor 템플릿 엔진에 대 한 지원이 강조 표시 되어 있습니다.

 ![IPhone 및 Android 솔루션 만들기](images/image13_1139x959.png)

기존 Xamarin 프로젝트에 **. cshtml** Razor 템플릿을 쉽게 추가할 수 있으며 , 이러한 솔루션 템플릿을 사용할 필요는 없습니다. iOS 프로젝트에서는 Razor를 사용 하기 위해 Storyboard가 필요 하지 않습니다. 프로그래밍 방식으로 모든 뷰에 UIWebView 보기 컨트롤을 추가 하 고 코드에서 C# Razor 템플릿 전체를 렌더링할 수 있습니다.

IPhone 및 Android 프로젝트에 대 한 기본 템플릿 솔루션 콘텐츠는 다음과 같습니다.

 ![iPhone 및 Android 템플릿](images/image10_428x310.png)

이 템플릿은 데이터 모델 개체를 사용 하 여 Razor 템플릿을 로드 하 고, 사용자 입력을 처리 하 고, JavaScript를 통해 사용자에 게 다시 전달 하는 즉시 사용 가능한 응용 프로그램 인프라를 제공 합니다.

솔루션의 중요 한 부분은 다음과 같습니다.

- **스타일 .css** 파일 등의 정적 콘텐츠
- Razor. cshtml 템플릿 파일 (예: **Razorview. cshtml** )
- **ExampleModel.cs** 와 같은 Razor 템플릿에서 참조 되는 모델 클래스입니다.
- `MainActivity` Android`iPhoneHybridViewController` 및 iOS의에서 웹 뷰를 만들고 템플릿을 렌더링 하는 플랫폼별 클래스입니다.


다음 섹션에서는 프로젝트의 작동 방식에 대해 설명 합니다.

### <a name="static-content"></a>정적 콘텐츠

정적 콘텐츠에는 웹 보기에 표시 되는 HTML 파일에서 연결 하거나 참조할 수 있는 CSS 스타일 시트, 이미지, JavaScript 파일 또는 기타 콘텐츠가 포함 됩니다.

템플릿 프로젝트에는 하이브리드 앱에 정적 콘텐츠를 포함 하는 방법을 보여 주는 최소 스타일 시트가 포함 되어 있습니다. CSS 스타일 시트는 다음과 같이 템플릿에서 참조 됩니다.

```html
<link rel="stylesheet" href="style.css" />
```

JQuery와 같은 프레임 워크를 포함 하 여 필요한 모든 스타일 시트와 JavaScript 파일을 추가할 수 있습니다.

### <a name="razor-cshtml-templates"></a>Razor cshtml 템플릿

템플릿에는 HTML/JavaScript와 C#간에 데이터를 전달 하는 데 도움이 되는 미리 작성 된 코드가 포함 된 Razor **. cshtml** 파일이 포함 되어 있습니다. 이렇게 하면 모델에서 읽기 전용 데이터를 표시 하지 않는 정교한 하이브리드 앱을 빌드할 수 있을 뿐 아니라 HTML에서 사용자 입력을 받아 처리 또는 저장을 위해 코드에 C# 다시 전달할 수도 있습니다.

#### <a name="rendering-the-template"></a>템플릿 렌더링

템플릿에서를 `GenerateString` 호출 하면 웹 뷰에 표시할 준비가 된 HTML이 렌더링 됩니다. 템플릿에서 모델을 사용 하는 경우 렌더링 하기 전에 해당 모델을 제공 해야 합니다. 이 다이어그램에서는 렌더링 작동 방식을 보여 줍니다. 지정 된 파일을 찾기 위해 제공 된 기본 디렉터리를 사용 하 여 런타임에 웹 뷰에서 정적 리소스를 확인 하는 것은 아닙니다.

 ![Razor 순서도](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>템플릿에서 C# 코드 호출

다시 호출 C# 되는 렌더링 된 웹 뷰에서의 통신은 웹 보기에 대 한 URL을 설정한 다음 웹 뷰를 다시 로드 하지 C# 않고 네이티브 요청을 처리 하기 위해에서 요청을 가로채는 방식으로 수행 됩니다.

예는 RazorView의 단추가 처리 되는 방식에 표시 될 수 있습니다. 이 단추에는 다음과 같은 HTML이 있습니다.

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

JavaScript `InvokeCSharpWithFormValues` 함수는 HTML 폼에서 모든 값을 읽고 웹 보기에 대해를 `location.href` 설정 합니다.

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

이렇게 하면 사용자 지정 스키마를 사용 하 여 웹 뷰를 URL로 이동 하려고 합니다 (예: `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

네이티브 웹 뷰에서이 탐색 요청을 처리할 때이를 가로챌 수 있습니다. IOS에서는 UIWebView 보기의 HandleShouldStartLoad 이벤트를 처리 하 여이 작업을 수행 합니다. Android에서는 단순히 폼에서 사용 되는 WebViewClient를 서브클래싱하 며 ShouldOverrideUrlLoading를 재정의 합니다.

이러한 두 탐색 인터셉터의 내부는 본질적으로 동일 합니다.

먼저 웹 보기가 로드 하려고 하는 URL을 확인 하 고 사용자 지정 체계 (`hybrid:`)로 시작 하지 않는 경우 정상적으로 탐색을 허용 합니다.

사용자 지정 URL 체계의 경우 체계와 "?" 사이에 있는 모든 URL의 처리할 메서드 이름 (이 경우 "UpdateLabel")입니다. 쿼리 문자열의 모든 항목은 메서드 호출에 대 한 매개 변수로 처리 됩니다.

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel`이 샘플에서는 textbox 매개 변수 (문자열 앞에 "C# 표시")에서 최소한의 문자열 조작을 수행한 다음 웹 뷰로 다시 호출 합니다.

URL을 처리 한 후 메서드는 웹 뷰가 사용자 지정 URL 탐색을 완료 하지 않도록 탐색을 중단 합니다.

#### <a name="manipulating-the-template-from-c"></a>C에서 템플릿 조작\#

에서 렌더링 된 HTML 웹 보기와 C# 의 통신은 웹 뷰에서 JavaScript를 호출 하 여 수행 됩니다. IOS에서는 uiwebview 보기에서를 호출 `EvaluateJavascript` 하 여이 작업을 수행 합니다.

```csharp
webView.EvaluateJavascript (js);
```

Android에서는 `"javascript:"` url 스키마를 사용 하 여 javascript를 url로 로드 하 여 웹 보기에서 javascript를 호출할 수 있습니다.

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>진정한 하이브리드 앱 만들기

이러한 템플릿은 각 플랫폼에서 네이티브 컨트롤을 사용 하지 않습니다. 전체 화면이 단일 웹 뷰로 채워집니다.

HTML은 프로토타입 제작에 적합 하 고, 다양 한 텍스트 및 응답성이 뛰어난 레이아웃을 비롯 하 여 웹이 가장 적합 한 항목의 종류를 표시 합니다. 그러나 모든 작업이 HTML 및 JavaScript에 적합 한 것은 아닙니다. 예를 들어 긴 데이터 목록을 통해 스크롤하면 네이티브 UI 컨트롤을 사용 하 여 더 효율적으로 실행 됩니다 (예: iOS 또는 Android의 ListView에서 UITableView).

템플릿의 웹 보기는 플랫폼별 컨트롤을 사용 하 여 쉽게 확대 될 수 있습니다. iOS 디자이너에서 **mainstoryboard.storyboard** 를 편집 하거나 Android의 **리소스/레이아웃/기본** 을 편집 하기만 하면 됩니다.

### <a name="razortodo-sample"></a>RazorTodo 샘플

[Razortodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo) 리포지토리에는 완전히 html 기반 앱과 네이티브 컨트롤과 html을 결합 하는 앱 간의 차이점을 보여 주는 두 개의 별도 솔루션이 포함 되어 있습니다.

- **Razortodo** -Razor 템플릿을 사용 하는 완전히 HTML 기반 앱입니다.
- **RazorNativeTodo** -IOS 및 Android 용 네이티브 목록 뷰 컨트롤을 사용 하지만 HTML 및 Razor를 사용 하 여 편집 화면을 표시 합니다.


이러한 Xamarin 앱은 iOS 및 Android에서 실행 되며, PCLs (이식 가능한 클래스 라이브러리)를 활용 하 여 데이터베이스 및 모델 클래스와 같은 공통 코드를 공유 합니다. Razor 파일은 여러 플랫폼에서 쉽게 공유할 수 있도록 PCL에 포함 될 수도 있습니다.

두 샘플 앱은 네이티브 플랫폼에서 Twitter 공유 및 텍스트 음성 변환 Api를 통합 하며, Xamarin을 사용 하는 하이브리드 응용 프로그램은 HTML Razor 템플릿 기반 뷰에서 모든 기본 기능에 액세스할 수 있다는 것을 보여 줍니다.

**Razortodo** 앱은 목록 및 편집 뷰에 HTML Razor 템플릿을 사용 합니다. 즉, 공유 이식 가능한 클래스 라이브러리 (데이터베이스 및 **. cshtml** Razor 템플릿 포함)에서 거의 완전히 앱을 빌드할 수 있습니다. 아래 스크린샷에는 iOS 및 Android 앱이 나와 있습니다.

 ![RazorTodo](images/Both_700x290.png)

**RazorNativeTodo** 앱은 편집 뷰에 HTML Razor 템플릿을 사용 하지만 각 플랫폼에서 네이티브 스크롤 목록을 구현 합니다. 다음과 같은 다양 한 이점을 제공 합니다.

- 성능-기본 스크롤 컨트롤은 가상화를 사용 하 여 매우 긴 데이터 목록이 있더라도 빠르고 부드러운 스크롤을 보장 합니다.
- 네이티브 환경-iOS 및 Android의 빠른 스크롤 인덱스 지원과 같이 플랫폼 관련 UI 요소를 쉽게 사용할 수 있습니다.


Xamarin을 사용 하 여 하이브리드 앱을 빌드하는 경우의 주요 혜택은 처음 샘플과 같이 완전히 HTML 기반의 사용자 인터페이스로 시작한 다음 필요할 때 플랫폼별 기능을 추가할 수 있습니다 (두 번째 샘플에서 볼 수 있음). IOS 및 Android의 기본 목록 화면 및 HTML Razor 편집 화면은 아래와 같습니다.

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>요약

이 문서에서는 하이브리드 응용 프로그램 빌드를 용이 하 게 하는 iOS 및 Android에서 사용할 수 있는 웹 보기 컨트롤의 기능에 대해 설명 했습니다.

그런 다음를 사용 하 여 Xamarin 앱에서 쉽게 HTML을 생성 하는 데 사용할 수 있는 Razor 템플릿 엔진 및 구문에 대해 설명 했습니다. **cshtml** Razor 템플릿 파일입니다. 또한 Xamarin을 사용 하 여 하이브리드 응용 프로그램을 신속 하 게 빌드할 수 있도록 하는 Mac용 Visual Studio 솔루션 템플릿도 설명 했습니다.

마지막으로 웹 뷰를 네이티브 사용자 인터페이스 및 Api와 결합 하는 방법을 보여 주는 RazorTodo 샘플이 도입 되었습니다.

### <a name="related-links"></a>관련 링크

- [RazorTodo 샘플](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3-Razor 뷰 엔진 (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Razor 구문을 사용한 ASP.NET 웹 프로그래밍 소개 (Microsoft)](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
