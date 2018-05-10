---
title: Razor 서식 파일을 사용 하 여 빌드 HTML 뷰
description: " HTML을 렌더링 하는 전체 화면 웹 페이지를 사용 하 여 특히 이미 있는 경우 HTML, Javascript 및 CSS 웹 사이트 프로젝트에서 플랫폼 간 방식으로 복합 서식 지정을 렌더링 하는 간단 하 고 효율적인 방법을 수 있습니다."
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 48d7778bf3225401f2819909ae6be320cfa881e3
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="building-html-views-using-razor-templates"></a>Razor 서식 파일을 사용 하 여 빌드 HTML 뷰

모바일 개발 환경에서 "하이브리드 app" 라는 용어는 일반적으로 호스트 된 웹 뷰어 컨트롤 내의 HTML 페이지로 일부 또는 전체를 해당 화면을 제공 하는 응용 프로그램을 나타냅니다.

수 있는 일부 개발 환경 가지 모바일에서 앱을 빌드하고 완전히 HTML 및 Javascript에서 해당 앱 복잡 한 처리 또는 UI 효과 수행 하려고 할 때 성능 문제를 저하 될 수는 있지만 플랫폼 제한 되어 기능에 액세스할 수 있습니다.

Xamarin은 특히 HTML Razor 템플릿 엔진을 활용 하는 경우 두 가지 장점을 모두 제공 합니다. Xamarin을 사용한 Javascript 및 CSS를 사용 하지만 또한 기본 플랫폼 Api 및 C#을 사용 하 여 빠른 처리에 대 한 전체 액세스를 있는 플랫폼 간 템플릿 기반 하는 HTML 뷰를 작성할 수 있는 유연성을 해야 합니다.

이 문서에서는 Razor 템플릿 엔진 빌드 Xamarin을 사용 하 여 모바일 플랫폼에서 사용할 수 있는 HTML + Javascript + CSS 뷰를 사용 하는 방법을 설명 합니다.

## <a name="using-web-views-programmatically"></a>웹 보기를 사용 하 여 프로그래밍 방식으로

Razor에 대 한 배울 전에이 섹션에서는 직접 – HTML 콘텐츠를 표시할 웹 보기를 사용 하는 방법을 구체적으로 응용 프로그램 내에서 생성 되는 HTML 콘텐츠를 설명 합니다.

Xamarin 쉽게 만들고 C#을 사용 하 여 HTML을 표시 하려면 iOS와 Android에서 기본 플랫폼 Api에 대 한 전체 액세스를 제공 합니다. 각 플랫폼에 대 한 기본 구문은 다음과 같습니다.

### <a name="ios"></a>iOS

코드 몇 줄 사용도 Xamarin.iOS에 UIWebView 컨트롤의 HTML을 표시 합니다.

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

참조는 [iOS UIWebView](http://docs.xamarin.com/recipes/ios/content_controls/web_view/) 레시피에 대 한 자세한 내용은 UIWebView 컨트롤을 사용 하 여 합니다.

### <a name="android"></a>Android

Xamarin.Android를 사용 하 여 WebView 컨트롤에서 HTML을 표시 합니다. 단 몇 줄의 코드로 수행 됩니다.

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

참조는 [Android WebView](http://docs.xamarin.com/recipes/android/controls/webview/) 레시피에 대 한 자세한 내용은 WebView 컨트롤을 사용 하 여 합니다.

### <a name="specifying-the-base-directory"></a>기본 디렉터리를 지정합니다.

두 플랫폼 모두에서 HTML 페이지에 대 한 기본 디렉터리를 지정 하는 매개 변수는 합니다. CSS 파일 및 이미지와 같은 리소스에 대 한 상대 참조를 확인 하는 데 사용 되는 장치의 파일 시스템에 위치입니다. 예를 들어 같은 태그

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

이러한 파일을 참조: **style.css**, **monkey.jpg** 및 **jscript.js**합니다. 기본 디렉터리 설정 이러한 파일이 있는 페이지에 로드 될 수 있도록 웹 보기를 알려 줍니다.

#### <a name="ios"></a>iOS

다음 C# 코드로 iOS에 있는 템플릿 출력 렌더링 됩니다.

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

기본 디렉터리를로 지정 `NSBundle.MainBundle.BundleUrl` 이 응용 프로그램에 설치 된 디렉터리를 가리킵니다. 에 있는 모든 파일은 **리소스** 폴더와 같은이 위치에 복사 하는 **style.css** 여기에 표시 된 파일:

 ![iPhoneHybrid 솔루션](images/image1_240x163.png)

모든 정적 콘텐츠 파일에 대 한 빌드 작업이 **BundleResource**:

 ![iOS 프로젝트 빌드 작업: BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

Android에는 또한 html 문자열 웹 보기에 표시 되는 매개 변수로 전달 될 기본 디렉터리가 필요 합니다.

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

특수 문자열 **file:///android_asset/** 앱에서 표시 된 여기 포함 된 Android Assets 폴더를 참조는 **style.css** 파일입니다.

 ![AndroidHybrid 솔루션](images/image3_240x167.png)

모든 정적 콘텐츠 파일에 대 한 빌드 작업이 **AndroidAsset**합니다.

 ![Android 프로젝트 빌드 작업: AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>HTML 및 Javascript에서 C# 호출

웹 보기에는 html 페이지 로드 되 면 취급 링크와 forms 페이지가 하는 서버에서 로드 된 경우와 마찬가지로 합니다. 즉, 사용자가 링크를 클릭 하거나가 양식을 전송 하는 경우 지정된 된 대상으로 이동 하는 웹 보기 시도 합니다.

외부 서버 (예: google.com)에 연결 되어 웹 보기는 외부 웹 사이트 (인터넷에 연결 되어 가정)를 로드 하려고 시도 합니다.

```html
<a href="http://google.com/">Google</a>
```

링크는 상대적 웹 보기는 기본 디렉터리에서 해당 콘텐츠를 로드 하려고 시도 합니다. 물론 없습니다 네트워크 연결이 필요 작동 하도록 하는이 콘텐츠는 장치에 앱에 저장 되어 있는 합니다.

```html
<a href="somepage.html">Local content</a>
```

양식 동작은 동일한 규칙을 따릅니다.

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

클라이언트에서 웹 서버를 호스팅하도록 것 그러나 오늘날의 반응 형 디자인 패턴에 사용 되는 동일한 서버 통신 기술은 사용 하 여 HTTP GET을 통해 서비스를 호출 하 하 고 표시 하 고 Javascript에서 응답을 비동기적으로 처리할 수 있습니다 (또는 웹 보기에서 호출 Javascript 이미 호스트). 이렇게 하면 쉽게 데이터를 전달 하는 HTML을 다시 처리 한 다음 그 결과 HTML 페이지에 다시 표시에 대 한 C# 코드에 있습니다.

IOS 및 Android 앱 코드 (필요한 경우)에 응답할 수 있도록 이러한 탐색 이벤트를 가로챌 수 있는 응용 프로그램 코드에 대 한 메커니즘을 제공 합니다. 이 기능은 웹 보기와 상호 작용 하는 네이티브 코드 수 있기 때문에 하이브리드 응용 프로그램을 구축 하는 데 중요 합니다.

#### <a name="ios"></a>iOS

응용 프로그램 코드 (예: 링크 클릭) 탐색 요청을 처리할 수 있도록 iOS의 웹 보기에서 ShouldStartLoad 이벤트를 재정의할 수 있습니다. 모든 정보를 제공 하는 메서드 매개 변수

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

한 다음 이벤트 처리기를 할당 합니다.

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

Android에서 단순히 하위 클래스 WebViewClient 한 다음 탐색 요청에 응답 하는 코드를 구현 합니다.

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, string url) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

및 다음 웹 보기에서 클라이언트를 설정 합니다.

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>C#에서 Javascript를 호출합니다.

새 HTML 페이지를 로드 하는 웹 보기를 지시 하는 것 외에도 C# 코드의 현재 표시 된 페이지 내에서 사용 하는 Javascript 실행 수도 있습니다. Javascript 통해 해당 페이지에서 이미 제공에 대 한 메서드 호출을 만들 수 있습니다 또는 전체 Javascript 코드 블록을 C# 문자열을 사용 하 여 만든를 실행 하 고 `script` 태그입니다.

#### <a name="android"></a>Android

Javascript 코드 실행 되 고 다음 사용 하 여 접두사를 만들기 "javascript:" 하 고 해당 문자열을 로드 웹 보기를 지시 합니다.

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

iOS 웹 보기 특히 Javascript를 호출 하는 메서드를 제공 합니다.

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>요약

이 섹션에서 Android와 iOS xamarin을 하이브리드 응용 프로그램을 빌드할 수 있는 웹 뷰 컨트롤의 기능을 소개 했습니다 포함 하 여:

-  HTML 코드에서 생성 된 문자열에서 로드 하는 기능
-  로컬 파일 (CSS, Javascript, 이미지 또는 기타 HTML 파일)를 참조 하는 기능
-  C# 코드에서는 탐색 요청을 가로챌 수 있는 기능
-  C# 코드에서 Javascript를 호출 하는 기능.


다음 섹션을 사용 하려면 HTML 하이브리드 앱을 만드는 쉽게 Razor를 소개 합니다.

## <a name="what-is-razor"></a>Razor 란?

Razor 원래 서버에서 실행 하 고 웹 브라우저에 게 제공 하는 HTML을 생성 하려면 ASP.NET MVC와 함께 도입 된 템플릿 엔진입니다.

Razor 템플릿 엔진 express 레이아웃 및 CSS 스타일 시트 및 Javascript를 쉽게 통합할 수 있도록 C#과 표준 HTML 구문을 확장 합니다. 템플릿에서 모델 클래스, 사용자 정의 형식일 수 있습니다 및 속성을 가진 서식 파일에서 직접 액세스할 수 있습니다를 참조할 수 있습니다. 주요 이점 중 하나는 HTML 및 C# 구문 쉽게 혼합 하는 기능입니다.

Razor 템플릿 서버 쪽 사용에 제한 되지는 않습니다., Xamarin 앱에 포함 될 수도 있습니다. 프로그래밍 방식으로 웹 뷰를 사용 하는 기능과 더불어 Razor 템플릿을 사용 하면 Xamarin으로 빌드된를 정교한 플랫폼 간 하이브리드 응용 프로그램.

### <a name="razor-template-basics"></a>Razor 템플릿 기본 사항

Razor 템플릿 파일을 **.cshtml** 파일 확장명입니다. 텍스트 템플릿 섹션에서 Xamarin 프로젝트에 추가 된 **새 파일** 대화 상자:

 ![새 파일-Razor 템플릿](images/image5_400x201.png)

단순 Razor 템플릿 ( **RazorView.cshtml**)는 다음과 같습니다.

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

일반 HTML 파일에서 다음과 같은 차이점을 확인 합니다.

-  `@` 기호 Razor 템플릿에서 특별 한 의미가 – 것을 의미 다음 식은 C# 계산 되도록 합니다.
- `@model` 지시문은 항상 Razor 템플릿 파일의 첫 번째 줄으로 표시 됩니다.
-  `@model` 지시문 뒤에 형식와 야 합니다. 이 예제는 간단한 문자열에 전달 되는 서식 파일을 하지만 사용자 지정 클래스도 될 수 없습니다.
-  때 `@Model` 참조 되 고 서식 파일을 통해 (이 예에서는 문자열)에서 생성 될 때 서식 파일에 전달 되는 개체에 대 한 참조를 제공 합니다.
-  IDE에서 서식 파일에 대 한 partial 클래스를 자동으로 생성 됩니다 (사용 하 여 파일의 **.cshtml** 확장명). 이 코드를 볼 수는 있지만 편집 하지 말아야 합니다.
 ![RazorView.cshtml](images/image6_125x34.png) partial 클래스 이름이 RazorView.cshtml 템플릿 파일 이름과 일치 하도록 합니다. C# 코드에서 서식 파일을 참조 하는 데 사용 되는이 이름입니다.
- `@using` 문을 추가 네임 스페이스를 포함 하도록 Razor 서식 파일의 맨 아래에 포함 될 수도 있습니다.


그런 다음 다음 C# 코드가 사용 된 최종 HTML 출력을 생성할 수 있습니다. Note 문자열 "Hello World" 렌더링 된 템플릿 출력에 통합 됩니다는 모델을 지정 했습니다.

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

IOS 시뮬레이터 및 Android 에뮬레이터에서 웹 보기에 표시 된 출력은 다음과 같습니다.

 ![Hello World](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>추가 Razor 구문

시작할 수 있도록 몇 가지 기본 Razor 구문을 소개 하는 것이 섹션에서는 사용 하 여 합니다. 이 섹션의 예제 데이터를 사용 하 여 다음 클래스를 채울 Razor를 사용 하 여 표시 합니다.

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

모든 예에서는 다음 데이터 초기화 코드를 사용합니다.

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>모델 속성 표시

모델 속성을 사용 하 여 클래스 이면이 예에서는 서식 파일에 표시 된 대로 Razor 서식 파일에서 참조 쉽게 된:

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    </body>
</html>
```

이 다음 코드를 사용 하는 문자열을 렌더링할 수 있습니다.

```csharp
var template = new RazorView () { Model = animal };
var page = template.GenerateString ();
```

IOS 시뮬레이터 및 Android 에뮬레이터에서 웹 보기에서 원인이 될 수는 다음과 같습니다.

 ![루 퍼 트](images/image8_516x160.png)

#### <a name="c-statements"></a>C# 문

더 복잡 한 C# 템플릿에서 모델 속성 업데이트 등이 예에서 나이 계산에 포함 될 수 있습니다.

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

복잡 한 줄 C# 식 (예: 연령 서식) 묶어 사용 하 여 코드를 작성할 수 있습니다 `@()`합니다.

사용 하 여 묶으면 여러 C# 문을 작성할 수 있습니다 `@{}`합니다.

#### <a name="if-else-statements"></a>If else 문

코드 분기도 표현할 수 `@if` 이 서식 파일 예제에 표시 된 대로 합니다.

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

루프 구문 같은 `foreach` 추가할 수도 있습니다. `@` 루프 변수에서 접두사를 사용할 수 있습니다 ( `@food` 이 예제의) html에서 렌더링 합니다.

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

IOS 시뮬레이터 및 Android 에뮬레이터에서 실행 되는 위의 서식 파일의 출력 표시 됩니다.

 ![루 퍼 트 X 원숭이](images/image9_520x277.png)

이 섹션 Razor 템플릿을 사용 하 여 간단한 읽기 전용 뷰를 렌더링 하는 기본적인 검사가 수행 됩니다. 다음 섹션에는 Razor를 사용 하 여 사용자 입력을 허용 하 고 Javascript HTML 보기 및 C# 간의 상호 운용할 수 있는 전체 앱을 빌드하는 방법을 설명 합니다.

## <a name="using-razor-templates-with-xamarin"></a>Xamarin을 사용한 Razor 템플릿 사용

이 섹션에서는 Mac.에 대 한 Visual Studio에서 솔루션 템플릿을 사용 하 여 하이브리드 응용 프로그램 빌드를 사용 하는 방법에 설명 세 개의 템플릿으로를 제공 하는 **파일 > 새로 만들기 > 솔루션 중...**  창:

- **Android > 앱 > Android WebView 응용 프로그램**
- **iOS > 앱 > WebView 응용 프로그램**
- **ASP.NET MVC 프로젝트**



**새 솔루션** iPhone 및 Android 프로젝트에 대 한 다음과 같은 창-솔루션 설명 오른쪽에는 Razor 템플릿 엔진에 대 한 지원 강조 표시 합니다.

 ![IPhone 및 Android 솔루션 만들기](images/image13_1139x959.png)

쉽게 추가할 수 있습니다는 **.cshtml** Razor 템플릿 *모든* Xamarin 프로젝트를 기존 필요 없는 이러한 솔루션 템플릿을 사용 하려면. iOS 프로젝트 하거나; Razor를 사용 하는 스토리 보드를 필요 하지 않습니다. 단순히 UIWebView 컨트롤 보기 중 하나에 프로그래밍 방식으로 추가 하 고 전체 C# 코드에서 Razor 템플릿을 렌더링할 수 있습니다.

IPhone 및 Android 프로젝트에 대 한 기본 서식 파일 솔루션 콘텐츠는 다음과 같습니다.

 ![iPhone 및 Android 템플릿](images/image10_428x310.png)

서식 파일 데이터 모델 개체와 함께 Razor 서식 파일을 로드 하 고 사용자 입력을 처리 하 고 Javascript 통해 사용자에 게 다시 전달 하도록 준비 휴대용 응용 프로그램 인프라를 제공 합니다.

솔루션의 중요 한 부분은 다음과 같습니다.

-  와 같은 정적 콘텐츠는 **style.css** 파일입니다.
-  Razor.cshtml 템플릿 파일 같은 **RazorView.cshtml** 합니다.
-  모델 클래스와 같은 Razor 템플릿에서 참조 되는 **ExampleModel.cs** 합니다.
-  웹 보기를 만들고와 같은 서식 파일을 렌더링 하는 플랫폼 특정 클래스의 `MainActivity` Android에서 및 `iPhoneHybridViewController` iOS에서 합니다.


다음 섹션의 프로젝트 작동 하는 방법을 설명 합니다.

### <a name="static-content"></a>정적 콘텐츠

정적 콘텐츠는 CSS 스타일 시트, 이미지, Javascript 파일 또는 연결에서 또는 웹 보기에 표시 되는 HTML 파일에서 참조 될 수 있는 기타 콘텐츠를 포함 합니다.

서식 파일 프로젝트 하이브리드 앱에서 정적 콘텐츠를 포함 하는 방법을 보여 주기 위한 최소한의 스타일 시트를 포함 합니다. CSS 스타일 시트 템플릿을 다음과 같이 참조 됩니다.

```html
<link rel="stylesheet" href="style.css" />
```

모든 스타일 시트 및 JQuery와 같은 프레임 워크 등 필요한 Javascript 파일을 추가할 수 있습니다.

### <a name="razor-cshtml-templates"></a>Razor cshtml 템플릿

서식 파일에는 Razor 포함 **.cshtml** 에 HTML/Javascript와 C# 간의 데이터 통신을 위해 코드를 기록한 사전 파일. 이렇게 하면 수 하 하지 않는 모델에서 읽기 전용 데이터를 표시 하지만 HTML의 사용자 입력을 허용 전달 정교한 하이브리드 앱 처리 또는 저장에 대 한 C# 코드에 다시 빌드입니다.

#### <a name="rendering-the-template"></a>서식 파일을 렌더링

호출 된 `GenerateString` 서식 파일에 HTML을 렌더링 웹 보기에 표시 하기 위해 준비 합니다. 경우 서식 파일 사용 모델을 렌더링 하기 전에 제공 해야 합니다. 이 다이어그램 렌더링의 작동 원리 – 하지 정적 리소스는 제공 된 기본 디렉터리를 사용 하 여 지정 된 파일을 찾을 수 런타임에 웹 보기에서 확인 하는 보여 줍니다.

 ![Razor 순서도](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>서식 파일을 C# 코드 호출

C#에 콜백할 수 렌더링된 웹 보기에서 통신 웹 보기에 대 한 URL을 설정 하 고 다음 웹 보기를 다시 로드 하지 않고 네이티브 요청을 처리 하는 C#에서 요청을 차단 하 여 수행 됩니다.

예제에서 RazorView의 단추를 처리 하는 방법을 볼 수 있습니다. 단추에는 다음 HTML에 있습니다.

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

`InvokeCSharpWithFormValues` Javascript 함수는 HTML 폼 및 집합에서 모든 값을 읽고는 `location.href` 웹 보기:

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

웹 보기 않음 (예: 사용자 지정 체계를 사용 하 여 URL 탐색 하려고이 `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

기본 웹 보기가 탐색 요청을 처리할 때 한 것을 차단할 수 있습니다. IOS의 경우이 UIWebView HandleShouldStartLoad 이벤트를 처리 하 여 수행 됩니다. Android에서는에서는 단순히 하위 클래스는 WebViewClient 된 폼에 사용 하 고 ShouldOverrideUrlLoading를 재정의 합니다.

이러한 두 가지 탐색 인터셉터의 내부는 거의 동일 합니다.

첫째, 웹 보기를 로드 하려고 하는 URL을 확인 하 고 사용자 지정 스키마가 시작 되지 않으면 (`hybrid:`), 정상적으로 탐색을 허용 합니다.

사용자 지정 URL 체계 URL의 체계 사이 있는 모든 것에 대 한 및 "?" (이 경우 "UpdateLabel")에서 처리 되어야 하는 메서드 이름이입니다. 쿼리 문자열의 모든 개체에서 메서드 호출에 매개 변수로 처리 됩니다.

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` 이 샘플에서 (앞에 추가 "C#을 대상 이라고" 문자열), textbox 매개 변수에서 최소한의 문자열 조작의 역할과 다음 웹 보기를 다시 호출 합니다.

URL을 처리 한 후 웹 보기는 사용자 지정 URL을 탐색을 완료 하지 않도록 메서드 탐색을 중단 합니다.

#### <a name="manipulating-the-template-from-c"></a>C#에서 서식 파일을 조작

C#에서 통신을 렌더링 된 HTML 웹 보기에는 웹 보기에서 Javascript를 호출 하 여 수행 됩니다. Ios, 이렇게 호출 하 여 `EvaluateJavascript` 는 UIWebView에:

```csharp
webView.EvaluateJavascript (js);
```

Android에서는 Javascript에서에서 호출할 수 있습니다 웹 보기는 Javascript를 사용 하 여 URL을 로드 하 여는 `"javascript:"` URL 체계:

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>응용 프로그램을 만드는 진정한 하이브리드

이러한 템플릿은 되지 않도록 각 플랫폼에서 네이티브 컨트롤의 사용 – 단일 웹 보기는 전체 화면은 채워집니다.

HTML 하며 될 수 있습니다, 프로토타입 생성에 대 한 훌륭한 종류의 항목이 표시 웹 서식 있는 텍스트 및 반응 형 레이아웃 등 가장 합니다. 그러나 일부 작업은 HTML 및 Javascript – 데이터의 긴 목록을 통해 스크롤할에 적합 한 더 잘 (iOS에 UITableView) 또는 ListView 같은 네이티브 UI 컨트롤을 사용 하 여 Android에서 예를 들어 수행 합니다.

서식 파일에서 웹 보기 컨트롤과 플랫폼별 – 단순히 편집 쉽게 확장할 수 있습니다는 **MainStoryboard.storyboard** iOS 디자이너에서 또는 **Resources/layout/Main.axml** Android에서.

### <a name="razortodo-sample"></a>RazorTodo 샘플

[RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo) 저장소에 대 한 두 개의 별도 해결 HTML 네이티브 컨트롤을 결합 하는 앱과 완전히 HTML 기반 응용 프로그램 간의 차이 보여 줍니다.

-  **RazorTodo** -Razor 템플릿을 사용 하 여 완전히 HTML 기반 응용 프로그램입니다.
-  **RazorNativeTodo** -iOS 및 Android 용 네이티브 목록 보기 컨트롤을 사용 하지만 HTML과 Razor 편집 화면을 표시 합니다.


Xamarin 앱이 iOS 및 Android에서 이식 가능한 클래스 라이브러리 (PCLs) 데이터베이스 및 모델 클래스와 같은 공통 코드를 공유를 사용 하 여 모두 실행 됩니다. Razor **.cshtml** 템플릿 하므로 플랫폼 간에 쉽게 공유 하는 PCL에 포함할 수도 있습니다.

두 샘플 앱에는 Twitter 공유 및 네이티브 플랫폼에서 Xamarin 사용한 하이브리드 응용 프로그램 여전히 액세스할 있는지 원본으로 사용 하는 모든 기능에 HTML Razor 템플릿 기반 뷰에서 보여 주는 텍스트 음성 변환 Api 통합 합니다.

**RazorTodo** 응용 프로그램 목록 및 편집 보기에 대 한 HTML Razor 템플릿을 사용 합니다. 즉, 공유 된 이식 가능한 클래스 라이브러리에서 거의 완전히 응용 프로그램을 작성할 수 있습니다 (데이터베이스를 포함 하 고 **.cshtml** Razor 템플릿). 아래 스크린샷과 iOS 및 Android 앱을 보여 줍니다.

 ![RazorTodo](images/Both_700x290.png)

**RazorNativeTodo** 앱 편집 보기에 대 한 HTML Razor 템플릿 사용 하지만 각 플랫폼에 네이티브 스크롤 목록을 구현 합니다. 이 다양 한 다음과 같은 이점 제공 합니다.

-  성능-네이티브 스크롤 컨트롤 매우 긴 데이터 목록이 된 경우에 스크롤 하는 빠르고, 부드러운 되도록 가상화를 사용 합니다.
-  원래 환경-플랫폼 특정 UI 요소를 쉽게 iOS 및 Android에서 빠른 스크롤 인덱스 지원 등을 사용 합니다.


Xamarin 하이브리드 앱 개발의 주요 이점은 완전히 HTML 기반 사용자 인터페이스 (예: 첫 번째 샘플)를 시작 하 고 다음 (두 번째 샘플은)와 같이 필요한 경우 플랫폼 특정 기능을 추가할 수 있는지는 합니다. 네이티브 목록 화면 및 HTML Razor 화면 모두 iOS에서 편집한 Android 아래에 표시 됩니다.

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>요약

이 문서 작성을 용이 하 게 하는 Android 및 iOS에서 사용할 수 있는 웹 보기 컨트롤의 기능을 설명 했습니다 하이브리드 응용 프로그램입니다.

그런 다음 Razor 템플릿 엔진 및 Xamarin 앱을 사용 하 여 HTML을 쉽게 생성 하는 데 사용할 수 있는 구문을 설명 합니다. **cshtml** Razor 템플릿 파일입니다. 또한 신속 하 게 수 있는 솔루션 템플릿을 Xamarin 사용한 하이브리드 응용 프로그램을 제작 해 Mac 용 Visual Studio 에서도 설명 합니다.

마지막으로 네이티브 사용자 인터페이스 및 Api와 웹 보기를 결합 하는 방법을 보여 주는 RazorTodo 샘플을 도입 되었습니다.

### <a name="related-links"></a>관련 링크

- [RazorTodo 샘플](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3-Razor 뷰 엔진 (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Razor 구문 (Microsoft)를 사용 하 여 ASP.NET 웹 프로그래밍 소개](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
