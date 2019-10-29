---
title: 웹 보기
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: c1fcb16bd40b818b27b57b877534e051a789a6c9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029016"
---
# <a name="xamarinandroid-web-view"></a>Xamarin Android 웹 보기

[`WebView`](xref:Android.Webkit.WebView) 를 사용 하 여 웹 페이지를 볼 수 있는 사용자 고유의 창을 만들 수 있습니다 (또는 전체 브라우저를 개발할 수도 있음). 이 자습서에서는 간단한 [`Activity`](xref:Android.App.Activity) 를 만듭니다.
이를 통해 웹 페이지를 보고 탐색할 수 있습니다.

새 프로젝트를 **만듭니다.**

**리소스/레이아웃/기본. axml** 을 열고 다음을 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

이 응용 프로그램은 인터넷에 액세스 하므로 Android 매니페스트 파일에 적절 한 권한을 추가 해야 합니다. 프로젝트의 속성을 열어 응용 프로그램에서 작동 하는 데 필요한 권한을 지정 합니다. 아래와 같이 `INTERNET` 사용 권한을 사용 하도록 설정 합니다.

![Android 매니페스트에서 인터넷 권한 설정](web-view-images/01-set-internet-permissions.png)

이제 **MainActivity.cs** 을 열고 Webkit에 대 한 using 지시문을 추가 합니다.

```csharp
using Android.Webkit;
```

`MainActivity` 클래스의 위쪽에서 [`WebView`](xref:Android.Webkit.WebView) 개체를 선언 합니다.

```csharp
WebView web_view;
```

**웹 보기** 에 URL을 로드 하 라는 메시지가 표시 되 면 기본적으로 기본 브라우저에 요청을 위임 합니다. **웹 보기** 에서 기본 브라우저가 아닌 URL을 로드 하도록 하려면 `Android.Webkit.WebViewClient` 서브 클래스를 로드 하 고 `ShouldOverriderUrlLoading` 메서드를 재정의 해야 합니다. 이 사용자 지정 `WebViewClient`의 인스턴스가 `WebView`에 제공 됩니다. 이렇게 하려면 `MainActivity`안에 다음 중첩 된 `HelloWebViewClient` 클래스를 추가 합니다.

```csharp
public class HelloWebViewClient : WebViewClient
{
    public override bool ShouldOverrideUrlLoading (WebView view, string url)
    {
        view.LoadUrl(url);
        return false;
    }
}
```

`ShouldOverrideUrlLoading`에서 `false`반환 하는 경우 현재 `WebView` 인스턴스가 요청을 처리 하 고 추가 작업이 필요 하지 않다는 것을 Android에 알립니다. 

API 수준 24 이상을 대상으로 하는 경우 `string`대신 두 번째 인수에 대 한 `IWebResourceRequest`를 사용 하는 `ShouldOverrideUrlLoading` 오버 로드를 사용 합니다.

```csharp
public class HelloWebViewClient : WebViewClient
{
    // For API level 24 and later
    public override bool ShouldOverrideUrlLoading (WebView view, IWebResourceRequest request)
    {
        view.LoadUrl(request.Url.ToString());
        return false;
    }
}
```

다음으로 [`OnCreate()`](xref:Android.App.Activity.OnCreate*)) 메서드에 대해 다음 코드를 사용 합니다.

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    web_view = FindViewById<WebView> (Resource.Id.webview);
    web_view.Settings.JavaScriptEnabled = true;
    web_view.SetWebViewClient(new HelloWebViewClient());
    web_view.LoadUrl ("https://www.xamarin.com/university");
}
```

이렇게 하면 [`Activity`](xref:Android.App.Activity) 된 레이아웃의 [`WebView`](xref:Android.Webkit.WebView) 멤버를 초기화 하 고 [`JavaScriptEnabled`](xref:Android.Webkit.WebSettings.JavaScriptEnabled)
`= true`를 사용 하 여 [`WebView`](xref:Android.Webkit.WebView) 에 javascript를 사용 하도록 설정 합니다 (에 대 한 자세한 내용은 javascript의 [javascript 조리법에서](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript) JavaScript에서 C\# 함수를 호출 하는 방법에 대 한 정보입니다. 마지막으로 초기 웹 페이지가 [`LoadUrl(String)`](xref:Android.Webkit.WebView)와 함께 로드 됩니다.

앱을 빌드하고 실행합니다. 다음 스크린샷에 표시 된 것 처럼 간단한 웹 페이지 뷰어 앱이 표시 됩니다.

[웹 보기를 표시 하는 앱의 예![](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

**뒤로** 단추 키 누름을 처리 하려면 다음 using 문을 추가 합니다.

```csharp
using Android.Views;
```

그런 다음 `HelloWebView` 활동 내에 다음 메서드를 추가 합니다.

```csharp
public override bool OnKeyDown (Android.Views.Keycode keyCode, Android.Views.KeyEvent e)
{
    if (keyCode == Keycode.Back && web_view.CanGoBack ())
    {
        web_view.GoBack ();
        return true;
    }
    return base.OnKeyDown (keyCode, e);
}
```

이 [`OnKeyDown(int, KeyEvent)`](xref:Android.App.Activity.OnKeyDown*)
활동을 실행 하는 동안 단추를 누를 때마다 콜백 메서드가 호출 됩니다. 내부의 조건은 [`KeyEvent`](xref:Android.Views.KeyEvent) 를 사용 하 여 누른 키가 **뒤로** 단추이 고 [`WebView`](xref:Android.Webkit.WebView) 가 실제로 뒤로 이동할 수 있는지 여부를 확인 합니다 (기록이 있는 경우). 둘 다 true 이면 [`GoBack()`](xref:Android.Webkit.WebView.GoBack) 메서드가 호출 됩니다 .이 메서드는 [`WebView`](xref:Android.Webkit.WebView) 기록에서 한 단계 뒤로 이동 합니다. `true`를 반환 하면 이벤트가 처리 되었음을 나타냅니다. 이 조건이 충족 되지 않으면 이벤트는 다시 시스템으로 전송 됩니다.

애플리케이션을 다시 실행합니다. 이제 링크를 팔 로우 하 고 페이지 기록을 통해 다시 탐색할 수 있습니다.

[작업 중인 뒤로 단추의 스크린샷 예![](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)

*이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유한 작업을 기반으로 하 고*
[*Creative Commons 2.5 특성 라이선스*](https://creativecommons.org/licenses/by/2.5/)에 설명 된 용어에 따라 사용 되는 수정 내용입니다.

## <a name="related-links"></a>관련 링크

- [JavaScript에서 C# 호출](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
- [Webkit](xref:Android.Webkit.WebView)
- [KeyEvent](xref:Android.Webkit.WebView)
