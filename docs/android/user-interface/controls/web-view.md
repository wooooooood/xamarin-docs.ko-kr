---
title: 웹 보기
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8d7b0e1abc8eb11bf812a111764b9cccfb41e041
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241177"
---
# <a name="web-view"></a>웹 보기

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 웹 페이지 보기에 대 한 고유한 창을 만드는 (또는 심지어 전체 브라우저를 개발) 수 있습니다. 이 자습서에서는 간단한 만듭니다 [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) 볼 수를 웹 페이지를 탐색 합니다.

라는 새 프로젝트를 만듭니다 **HelloWebView**합니다.

오픈 **Resources/Layout/Main.axml** 하 고 다음을 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

이 응용 프로그램은 인터넷에 액세스 하기 때문에 적절 한 권한이 Android 매니페스트 파일 추가 해야 합니다. 응용 프로그램 작동 하는 데 필요한 사용 권한을 지정 하려면 프로젝트의 속성을 엽니다. 사용 하도록 설정 된 `INTERNET` 아래와 같이 권한:

![Android 매니페스트에서 인터넷 권한 설정](web-view-images/01-set-internet-permissions.png)

이제 열 **MainActivity.cs** 추가 하 여 및 Webkit에 대 한 지시문:

```csharp
using Android.Webkit;
```

맨 위에 있는 합니다 `MainActivity` 클래스를 선언 된 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 개체:

```csharp
WebView web_view;
```

경우는 **WebView** 는 URL을 로드 하 라는 메시지가 표시,이 기본적으로 위임 하는 기본 브라우저에 요청 합니다. 할 합니다 **WebView** URL 대신 기본 브라우저를 로드, 서브 클래스 해야 `Android.Webkit.WebViewClient` 재정의 및는 `ShouldOverriderUrlLoading` 메서드. 이 사용자 지정 인스턴스 `WebViewClient` 에 제공 되는 `WebView`합니다. 이 작업을 수행 하려면 다음 중첩을 추가 `HelloWebViewClient` 클래스 `MainActivity`:

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

때 `ShouldOverrideUrlLoading` 반환 `false`, Android에 대 한 신호를 보냅니다는 현재 `WebView` 요청을 처리 하는 인스턴스 및 추가 조치가 필요 하지 않습니다. 

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

다음으로, 다음 코드를 사용 합니다 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) 메서드:

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

멤버 초기화 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 에서 하나를 사용 하 여 합니다 [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) 레이아웃에 대 한 JavaScript를 사용 하도록 설정 하 고는 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 를사용하여[ `JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (참조를 [C 호출\# JavaScript에서](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript) C 호출 하는 방법에 대 한 정보에 대 한 레시피\# JavaScript에서 함수). 초기 웹 페이지를 사용 하 여 로드 되는 마지막으로, [ `LoadUrl(String)` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl)합니다.

앱을 빌드하고 실행합니다. 간단한 웹 페이지 뷰어 앱을 다음 스크린샷에 표시 된 것으로 표시 됩니다.

[![웹 보기를 표시 하는 앱의 예](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

처리 하는 **다시** 키 누름 단추, 다음 추가 문을 사용 하 여:

```csharp
using Android.Views;
```

다음으로 내에 다음 메서드를 추가 합니다 `HelloWebView` 활동:

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

이렇게 [ `OnKeyDown(int, KeyEvent)` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent)) 동작이 실행 되는 동안 단추를 누를 때마다 콜백 메서드가 호출 됩니다. 조건을 사용 하 여 내부를 [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/) 키를 눌렀는지 여부를 확인 하는 **다시** 단추를 사용할지 여부와 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 실제로 수 다시 (있는 경우 기록)을 탐색 합니다. 둘 다 true 인 경우 해당 [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/) 백 1 단계에서 이동 하는 메서드가 호출 되는 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 기록 합니다. 반환 `true` 이벤트가 처리 된 것을 나타냅니다. 이 조건이 충족 되지 않으면 이벤트 시스템에 다시 전송 됩니다.

응용 프로그램을 다시 실행합니다. 이제 링크를 따르고 페이지 기록을 탐색할 수: 있습니다.

[![작업에서 뒤로 단추의 예제 스크린샷](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)


*이 페이지의 일부는 생성 하 고 Android Open Source Project에서 공유 된 조건에 따라 사용 되는 작업에 따라 수정 합니다*
[*Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>관련 링크

- [JavaScript에서 C# 호출](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
