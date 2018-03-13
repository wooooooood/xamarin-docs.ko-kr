---
title: "웹 보기"
ms.topic: article
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 0d418786a7364946e4e20100157fa0907b66deeb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="web-view"></a>웹 보기

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 웹 페이지 보기에 대 한 고유한 창을 만들 (또는 개발 하는 전체 브라우저) 수 있습니다. 이 자습서에서는 간단한 만듭니다 [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) 를 볼 수 있으며 웹 페이지를 탐색 합니다.

라는 새 프로젝트 만들기 **HelloWebView**합니다.

열기 **Resources/Layout/Main.axml** 하 고 다음을 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

이 응용 프로그램은 인터넷에 액세스 하기 때문에 매니페스트 파일은 Android에 적절 한 사용 권한을 추가 해야 합니다. 응용 프로그램이 작동 하는 데 필요한 사용 권한을 지정 하 여 프로젝트의 속성을 엽니다. 사용 하도록 설정 된 `INTERNET` 아래와 같이 사용 권한:

![Android 매니페스트에 인터넷 권한 설정](web-view-images/01-set-internet-permissions.png)

이제 열 **MainActivity.cs** using 추가 Webkit에 대 한 지시문:

```csharp
using Android.Webkit;
```

맨 위에 있는 `MainActivity` 클래스를 선언 하는 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 개체:

```csharp
WebView web_view;
```

경우는 **WebView** 는 URL을 로드 하도록 요청을 기본적으로 위임 하는 기본 브라우저를 요청 합니다. 있어야는 **WebView** URL (기본 브라우저 아님)를 로드, 하위 클래스 해야 `Android.Webkit.WebViewClient` 재정의 `ShouldOverriderUrlLoading` 메서드. 이 사용자 지정 인스턴스 `WebViewClient` 에 제공 되는 `WebView`합니다. 이렇게 하려면 다음 중첩 된 추가 `HelloWebViewClient` 내 클래스 `MainActivity`:

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

때 `ShouldOverrideUrlLoading` 반환 `false`, Android에 알립니다 하는 현재 `WebView` 요청을 처리 하는 인스턴스 및 추가 조치가 필요 하지 않습니다. 

API 수준 24 이상을 대상으로 하는 경우의 오버 로드를 사용 하 여 `ShouldOverrideUrlLoading` 를 사용 하는 `IWebResourceRequest` 대신 두 번째 인수는 `string`:

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

를 사용 하 여에 대 한 다음 코드는 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) 메서드:

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

이 멤버를 초기화 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 에서 항목으로는 [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) 레이아웃에 대 한 JavaScript를 사용 하도록 설정 하 고는 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 와[ `JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (참조는 [호출 C\# JavaScript에서](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript) C 호출 하는 방법에 대 한 정보에 대 한 조리법\# JavaScript에서 함수). 와 초기 웹 페이지가 로드 되는 마지막으로, [ `LoadUrl(String)` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl)합니다.

앱을 빌드하고 실행합니다. 다음 스크린샷에 표시 된 것과는 간단한 웹 페이지 뷰어 앱을 나타나야 합니다.

[![보기를 표시 하는 응용 프로그램의 예](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

처리 하는 **다시** 키 누름 단추, 다음 추가 문을 사용 하 여:

```csharp
using Android.Views;
```

다음으로 내 다음 메서드를 추가 합니다.는 `HelloWebView` 활동:

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

이 [ `OnKeyDown(int, KeyEvent)` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent)) 활동이 실행 되는 동안에 단추를 누를 때마다 콜백 메서드가 호출 됩니다. 사용 하 여 내의 조건을 [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/) 키를 눌렀는지 여부를 확인 하려면는 **다시** 단추를 사용할지 여부와 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 실제로 수 다시 (있는 경우 기록을)을 탐색 합니다. 둘 다를 경우 [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/) 다시 한 단계는 이동 되는 메서드가 호출 되는 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 기록 합니다. 반환 `true` 이벤트가 처리 되었는지를 나타냅니다. 이 조건이 충족 되지 않으면이 이벤트는 시스템으로 다시 전송 됩니다.

응용 프로그램을 다시 실행합니다. 이제 링크를 통해 / 탐색을 다시 페이지 기록 수: 있습니다.

[![예제 작업에서 뒤로 단추 스크린샷](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)


*이 페이지의 일부는 Android 열려 있는 소스 프로젝트에서 공유 하 고 만들고에 설명 된 조건에 따라 사용 작업에 따라 수정 된*
[*Creative Commons 2.5 Attribution 라이선스* ](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>관련 링크

- [JavaScript에서 C# 호출](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
