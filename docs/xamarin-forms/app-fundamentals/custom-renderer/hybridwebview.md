---
title: Hybridwebview 구현
description: 이 문서에는 JavaScript에서 C# 코드를 호출할 수 있도록 플랫폼별 웹 컨트롤을 개선 하는 방법을 보여 주는 HybridWebView 사용자 지정 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/19/2018
ms.openlocfilehash: aa060bd16bc0220f6a6026106ff6c8d786daebc1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105040"
---
# <a name="implementing-a-hybridwebview"></a>Hybridwebview 구현

_Xamarin.Forms 사용자 지정 사용자 인터페이스 컨트롤 레이아웃 및 화면에 컨트롤 배치에 사용 되는 뷰 클래스에서 파생 되어야 합니다. 이 문서에는 JavaScript에서 C# 코드를 호출할 수 있도록 플랫폼별 웹 컨트롤을 개선 하는 방법을 보여 주는 HybridWebView 사용자 지정 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다._

모든 Xamarin.Forms 보기에 네이티브 컨트롤의 인스턴스를 만드는 각 플랫폼에 대 한는 함께 제공 되는 렌더러. 경우는 [ `View` ](xref:Xamarin.Forms.View) ios에서는 Xamarin.Forms 응용 프로그램에서 렌더링 되는 `ViewRenderer` 클래스가 인스턴스화되면 네이티브를 다시 인스턴스화하는 `UIView` 제어 합니다. Android 플랫폼에는 `ViewRenderer` 클래스를 인스턴스화하는 `View` 컨트롤입니다. Windows 플랫폼 (UWP (유니버설), 합니다 `ViewRenderer` 클래스 인스턴스화합니다 네이티브 `FrameworkElement` 컨트롤입니다. 렌더러 및 Xamarin.Forms 컨트롤에 매핑되는 네이티브 컨트롤 클래스에 대 한 자세한 내용은 참조 하세요. [렌더러 기본 클래스 및 네이티브 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다.

다음 다이어그램 간의 관계는 [ `View` ](xref:Xamarin.Forms.View) 및 구현 하는 해당 네이티브 컨트롤:

![](hybridwebview-images/view-classes.png "뷰 클래스와 해당 구현 네이티브 클래스 간의 관계")

플랫폼별 사용자 지정에 대 한 사용자 지정 렌더러를 만들어 구현 하려면 렌더링 프로세스를 사용할 수는 [ `View` ](xref:Xamarin.Forms.View) 각 플랫폼에서 합니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_HybridWebView) 는 `HybridWebView` 사용자 지정 컨트롤입니다.
1. [소비](#Consuming_the_HybridWebView) 는 `HybridWebView`Xamarin.Forms에서.
1. [만들](#Creating_the_Custom_Renderer_on_each_Platform) 에 대 한 사용자 지정 렌더러를 `HybridWebView` 각 플랫폼에서 합니다.

각 항목은 이제 설명 되어 다시 구현 하는 `HybridWebView` JavaScript에서 C# 코드를 호출할 수 있도록 플랫폼별 웹 컨트롤을 강화 하는 렌더러. `HybridWebView` 인스턴스가 해당 이름을 입력 하는 사용자에 게 요청 하는 HTML 페이지에 표시할 사용 됩니다. 그런 다음 사용자가 HTML 단추를 클릭 하면 JavaScript 함수는 호출 하는 C# `Action` 사용자 이름을 포함 하는 팝업을 표시 하는 합니다.

호출 C# JavaScript에서 프로세스에 대 한 자세한 내용은 참조 하세요 [호출 C# JavaScript에서](#Invoking_C_from_JavaScript)합니다. HTML 페이지에 대 한 자세한 내용은 참조 하세요. [웹 페이지 만들기](#Creating_the_Web_Page)합니다.

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>HybridWebView 만들기

합니다 `HybridWebView` 사용자 지정 컨트롤을 서브클래싱하 여 생성할 수는 [ `View` ](xref:Xamarin.Forms.View) 클래스에 다음 코드 예제 에서처럼:

```csharp
public class HybridWebView : View
{
  Action<string> action;
  public static readonly BindableProperty UriProperty = BindableProperty.Create (
    propertyName: "Uri",
    returnType: typeof(string),
    declaringType: typeof(HybridWebView),
    defaultValue: default(string));

  public string Uri {
    get { return (string)GetValue (UriProperty); }
    set { SetValue (UriProperty, value); }
  }

  public void RegisterAction (Action<string> callback)
  {
    action = callback;
  }

  public void Cleanup ()
  {
    action = null;
  }

  public void InvokeAction (string data)
  {
    if (action == null || data == null) {
      return;
    }
    action.Invoke (data);
  }
}
```

`HybridWebView` 사용자 지정 컨트롤.NET Standard 라이브러리 프로젝트에서 생성 되 고 컨트롤에 대 한 다음 API를 정의 합니다.

- `Uri` 로드할 웹 페이지의 주소를 지정 하는 속성입니다.
- A `RegisterAction` 등록 하는 메서드는 `Action` 컨트롤을 사용 하 여 합니다. 등록 된 작업을 통해 참조 되는 HTML 파일에 포함 된 JavaScript에서 호출할는 `Uri` 속성입니다.
- A `CleanUp` 등록에 대 한 참조를 제거 하는 메서드 `Action`합니다.
- `InvokeAction` 등록 된 호출 하는 메서드 `Action`합니다. 이 메서드는 각 플랫폼 특정 프로젝트에 사용자 지정 렌더러를에서 호출 됩니다.

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>HybridWebView 사용

`HybridWebView` 사용자 지정 컨트롤에서에서 참조할 수 있습니다 XAML.NET Standard 라이브러리 프로젝트에서 해당 위치에 대 한 네임 스페이스를 선언 하 고 사용자 지정 컨트롤에서 네임 스페이스 접두사를 사용 하 여 여 합니다. 다음 코드 예제에서는 방법을 `HybridWebView` XAML 페이지에서 사용자 지정 컨트롤을 사용할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             x:Class="CustomRenderer.HybridWebViewPage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <local:HybridWebView x:Name="hybridWebView" Uri="index.html"
          HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
    </ContentPage.Content>
</ContentPage>
```

`local` 원하는 네임 스페이스 접두사를 이름을 지정할 수 있습니다. 그러나 합니다 `clr-namespace` 고 `assembly` 값 사용자 지정 컨트롤의 세부 정보를 일치 해야 합니다. 네임 스페이스를 선언 하는 사용자 지정 컨트롤을 참조 하는 접두사가 사용 됩니다.

다음 코드 예제에서는 방법을 `HybridWebView` C# 페이지에서 사용자 지정 컨트롤을 사용할 수 있습니다.

```csharp
public class HybridWebViewPageCS : ContentPage
{
  public HybridWebViewPageCS ()
  {
    var hybridWebView = new HybridWebView {
      Uri = "index.html",
      HorizontalOptions = LayoutOptions.FillAndExpand,
      VerticalOptions = LayoutOptions.FillAndExpand
    };
    ...
    Padding = new Thickness (0, 20, 0, 0);
    Content = hybridWebView;
  }
}
```

`HybridWebView` 인스턴스를 사용 하 여 각 플랫폼에서 네이티브 웹 컨트롤을 표시 됩니다. 있기 `Uri` 각 플랫폼별 프로젝트에 저장 된 및 네이티브 웹 컨트롤에서 표시 되는 HTML 파일을 설정 합니다. JavaScript 함수를 호출 하는 C#을 사용 하 여 해당 이름을 입력 하 라는 메시지 렌더링된 된 HTML `Action` HTML 단추 클릭에 응답에서 합니다.

`HybridWebViewPage` 다음 코드 예제와 같이 JavaScript에서 호출할 작업을 등록 합니다.

```csharp
public partial class HybridWebViewPage : ContentPage
{
  public HybridWebViewPage ()
  {
    ...
    hybridWebView.RegisterAction (data => DisplayAlert ("Alert", "Hello " + data, "OK"));
  }
}
```

이 작업을 호출 합니다 [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) 하 여 표시 되는 HTML 페이지에 입력 한 이름을 제공 하는 모달 팝업을 표시 하는 메서드는 `HybridWebView` 인스턴스.

사용자 지정 렌더러는 이제 C# 코드를 JavaScript에서 호출할 수 있도록 하 여 플랫폼별 웹 컨트롤을 강화 하기 위해 각 응용 프로그램 프로젝트에 추가할 수 있습니다.

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 서브 클래스를 만든를 `ViewRenderer<T1,T2>` 사용자 지정 컨트롤을 렌더링 하는 클래스입니다. 첫 번째 형식 인수에 대 한 렌더러가 경우에 사용자 지정 컨트롤 해야 `HybridWebView`합니다. 두 번째 형식 인수는 사용자 지정 보기를 구현 하는 네이티브 컨트롤 이어야 합니다.
1. 재정의 `OnElementChanged` 맞게 사용자 지정 컨트롤 및 쓰기 논리를 렌더링 하는 메서드. 이 메서드는 해당 하는 Xamarin.Forms 사용자 지정 컨트롤이 만들어질 때 호출 됩니다.
1. 추가 `ExportRenderer` 특성을 사용자 지정 렌더러 클래스 Xamarin.Forms 사용자 지정 컨트롤을 렌더링 하 사용 수를 지정할 수 있습니다. 이 특성은 Xamarin.Forms를 사용 하 여 사용자 지정 렌더러를 등록 하는 데 사용 됩니다.

> [!NOTE]
> 대부분의 Xamarin.Forms 요소 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공 하는 선택 사항입니다. 사용자 지정 렌더러를 등록 하지 않은 경우 컨트롤의 기본 클래스에 대 한 기본 렌더러 사용 됩니다. 그러나 사용자 지정 렌더러 필요한 각 플랫폼 프로젝트에서 렌더링 하는 경우는 [보기](xref:Xamarin.Forms.View) 요소입니다.

다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램에서 각 프로젝트의 책임을 보여 줍니다.

![](hybridwebview-images/solution-structure.png "HybridWebView 사용자 지정 렌더러 프로젝트 책임")

합니다 `HybridWebView` 사용자 지정 컨트롤에서 파생 되는 플랫폼별 렌더러 클래스에 의해 렌더링 되는 `ViewRenderer` 각 플랫폼에 대 한 클래스입니다. 이 인해 각 `HybridWebView` 다음 스크린샷과에서 같이 플랫폼별 웹 컨트롤에 렌더링 하는 사용자 지정 컨트롤:

![](hybridwebview-images/screenshots.png "각 플랫폼에서 HybridWebView")

합니다 `ViewRenderer` 노출 클래스는 `OnElementChanged` 해당 네이티브 웹 컨트롤을 렌더링 하는 Xamarin.Forms 사용자 지정 컨트롤을 만들 때 호출 되는 메서드. 이 메서드는 `ElementChangedEventArgs` 포함 된 매개 변수 `OldElement` 및 `NewElement` 속성입니다. 이러한 속성은 Xamarin.Forms 요소를 나타냅니다는 렌더러 *되었습니다* 에 연결 및 Xamarin.Forms 요소는 렌더러 *는* 을 각각 연결 합니다. 샘플 응용 프로그램에서을 `OldElement` 속성이 `null` 와 `NewElement` 속성에 대 한 참조가 포함 됩니다는 `HybridWebView` 인스턴스.

재정의 된 버전을 `OnElementChanged` 각 플랫폼별 렌더러 클래스에서 메서드를 네이티브 웹 컨트롤 인스턴스화 및 사용자 지정을 수행 하면 됩니다. 합니다 `SetNativeControl` 네이티브 웹 컨트롤을 인스턴스화하 메서드를 사용 해야 하며이 메서드는 컨트롤 참조 할당 됩니다는 `Control` 속성입니다. 또한를 통해 렌더링 되는 Xamarin.Forms 컨트롤에 대 한 참조를 가져올 수 있습니다는 `Element` 속성입니다.

일부 경우에는 `OnElementChanged` 메서드는 여러 번 호출할 수 있습니다. 따라서 메모리 누수를 방지 하려면 주의 해야 새 네이티브 컨트롤을 인스턴스화할 때. 사용자 지정 렌더러에서 새 네이티브 컨트롤을 인스턴스화할 때 사용하는 방법은 다음 코드 예제에 표시되어 있습니다.

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

`Control` 속성이 `null`일 경우 새 네이티브 컨트롤은 한 번만 인스턴스화되어야 합니다. 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결된 경우 이 컨트롤만 구성해야 합니다. 마찬가지로, 사용자 지정 렌더러가 변경된 항목에 연결된 경우 구독 대상 이벤트 처리기의 구독이 취소되어야 합니다. 이 접근 방식을 채택 메모리 누수의 영향을 받지 않으며 성능이 뛰어난 사용자 지정 렌더러를 만드는 데 도움이 됩니다.

으로 데코 레이트 된 각 사용자 지정 렌더러 클래스는 `ExportRenderer` Xamarin.Forms를 사용 하 여 렌더러를 등록 하는 특성입니다. 특성 매개 변수 두 개 – 렌더링 되 고 Xamarin.Forms 사용자 지정 컨트롤의 형식 이름 및 사용자 지정 렌더러의 형식 이름입니다. `assembly` 접두사 특성에 특성을 전체 어셈블리에 적용 되도록 지정 합니다.

다음 섹션에서는 각 네이티브 웹 컨트롤, 호출 C# JavaScript에서 프로세스 및 각 플랫폼별 사용자 지정 렌더러 클래스에서이 작업의 구현에 의해 로드 웹 페이지의 구조를 설명 합니다.

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>웹 페이지 만들기

다음 코드 예제에서 표시 되는 웹 페이지를 보여 줍니다.는 `HybridWebView` 사용자 지정 컨트롤:

```html
<html>
<body>
<script src="http://code.jquery.com/jquery-1.11.0.min.js"></script>
<h1>HybridWebView Test</h1>
<br/>
Enter name: <input type="text" id="name">
<br/>
<br/>
<button type="button" onclick="javascript:invokeCSCode($('#name').val());">Invoke C# Code</button>
<br/>
<p id="result">Result:</p>
<script type="text/javascript">
function log(str)
{
    $('#result').text($('#result').text() + " " + str);
}

function invokeCSCode(data) {
    try {
        log("Sending Data:" + data);
        invokeCSharpAction(data);
    }
    catch (err){
          log(err);
    }
}
</script>
</body>
</html>
```

웹 페이지에서 자신의 이름을 입력할 수 있습니다는 `input` 요소를 제공 하 고는 `button` C# 코드 클릭할 때 호출 하는 요소입니다. 이 달성 하기 위한 프로세스는 다음과 같습니다.

- 사용자가 클릭할 때 합니다 `button` 요소를 `invokeCSCode` 의 값을 사용 하 여 JavaScript 함수를 호출는 `input` 함수에 전달 되는 요소입니다.
- 합니다 `invokeCSCode` 함수 호출을 `log` C#에 보내는 데이터를 표시 하는 함수 `Action`합니다. 그런 다음 호출 하는 `invokeCSharpAction` 메서드를 호출 하 고 C# `Action`에서 받은 매개 변수를 전달 합니다 `input` 요소입니다.

`invokeCSharpAction` JavaScript 웹 페이지에 정의 되지 않은 함수와를 각 사용자 지정 렌더러를 통해 삽입 됩니다.

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>C# JavaScript에서 호출

호출 C# JavaScript에서 프로세스는 각 플랫폼에서 동일 합니다.

- 사용자 지정 렌더러 네이티브 웹 컨트롤을 만들고 지정 된 HTML 파일을 로드 합니다 `HybridWebView.Uri` 속성입니다.
- 사용자 지정 렌더러를 삽입 하는 웹 페이지가 로드 되 면는 `invokeCSharpAction` 웹 페이지에 JavaScript 함수입니다.
- 사용자 이름을 입력 하 고 HTML 클릭할 하는 경우 `button` 요소를 `invokeCSCode` 를 차례로 호출 하는 함수가 호출 되는 `invokeCSharpAction` 함수입니다.
- 합니다 `invokeCSharpAction` 함수를 호출 하는 사용자 지정 렌더러의 메서드를 호출 합니다 `HybridWebView.InvokeAction` 메서드.
- 합니다 `HybridWebView.InvokeAction` 메서드를 호출 하는 등록 된 `Action`합니다.

다음 섹션에서는이 프로세스는 각 플랫폼에서 구현 되는 방식을 설명 합니다.

### <a name="creating-the-custom-renderer-on-ios"></a>IOS에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 iOS 플랫폼에 대 한 사용자 지정 렌더러를 보여 줍니다.

```csharp
[assembly: ExportRenderer (typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.iOS
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, WKWebView>, IWKScriptMessageHandler
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.webkit.messageHandlers.invokeAction.postMessage(data);}";
        WKUserContentController userController;

        protected override void OnElementChanged (ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                userController = new WKUserContentController ();
                var script = new WKUserScript (new NSString (JavaScriptFunction), WKUserScriptInjectionTime.AtDocumentEnd, false);
                userController.AddUserScript (script);
                userController.AddScriptMessageHandler (this, "invokeAction");

                var config = new WKWebViewConfiguration { UserContentController = userController };
                var webView = new WKWebView (Frame, config);
                SetNativeControl (webView);
            }
            if (e.OldElement != null) {
                userController.RemoveAllUserScripts ();
                userController.RemoveScriptMessageHandler ("invokeAction");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup ();
            }
            if (e.NewElement != null) {
                string fileName = Path.Combine (NSBundle.MainBundle.BundlePath, string.Format ("Content/{0}", Element.Uri));
                Control.LoadRequest (new NSUrlRequest (new NSUrl (fileName, false)));
            }
        }

        public void DidReceiveScriptMessage (WKUserContentController userContentController, WKScriptMessage message)
        {
            Element.InvokeAction (message.Body.ToString ());
        }
    }
}
```

`HybridWebViewRenderer` 클래스에 지정 된 웹 페이지를 로드 합니다 `HybridWebView.Uri` 네이티브 속성 [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) 컨트롤 및 `invokeCSharpAction` JavaScript 함수는 웹 페이지에 삽입 합니다. 사용자가 해당 이름을 입력 하 고 HTML을 클릭 하면 `button` 요소를 `invokeCSharpAction` JavaScript 함수를 실행 사용 하 여는 `DidReceiveScriptMessage` 메서드가 웹 페이지에서 메시지를 받은 후 호출 됩니다. 이 메서드를 호출 하 여 `HybridWebView.InvokeAction` 팝업을 표시할 등록 된 작업을 호출 하는 메서드.

이 기능을 수행 하려면 다음과 같습니다.

- 제공한를 `Control` 속성은 `null`, 다음 작업이 수행 됩니다.
  - A [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) 메시지를 게시 하 고 웹 페이지 사용자 스크립트 삽입을 허용 하는 인스턴스가 생성 됩니다.
  - A [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) 삽입할 인스턴스가 생성 되는 `invokeCSharpAction` 웹 페이지가 로드 된 후 웹 페이지에 JavaScript 함수입니다.
  - [ `WKUserContentController.AddScript` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/) 메서드를 추가 합니다 [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) 콘텐츠 컨트롤러 인스턴스입니다.
  - [ `WKUserContentController.AddScriptMessageHandler` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/) 라는 스크립트 메시지 처리기를 추가 하는 메서드 `invokeAction` 에 [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) JavaScript 함수를 발생할 수 있는 인스턴스 `window.webkit.messageHandlers.invokeAction.postMessage(data)` 모든 정의 사용 하는 모든 웹 보기의 프레임을 `WKUserContentController` 인스턴스.
  - A [ `WKWebViewConfiguration` ](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/) 인스턴스를 만들와 [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) 콘텐츠 컨트롤러로 설정 하는 인스턴스.
  - [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) 컨트롤을 인스턴스화하는 `SetNativeControl` 메서드를 호출에 대 한 참조를 할당 하는 `WKWebView` 컨트롤을 `Control` 속성.
- 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결 되어 있는지를 제공 합니다.
  - 합니다 [ `WKWebView.LoadRequest` ](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/) 에 지정 된 HTML 파일을 로드 하는 메서드는 `HybridWebView.Uri` 속성입니다. 코드 파일에 저장 되도록 지정 합니다 `Content` 프로젝트의 폴더입니다. 웹 페이지가 표시 되 면는 `invokeCSharpAction` JavaScript 함수는 웹 페이지에 삽입 됩니다.
- 때 렌더러가 변경 내용에 연결 됩니다.
  - 리소스가 해제 됩니다.

> [!NOTE]
> `WKWebView` 클래스 iOS 8 이상 에서만 지원 됩니다.

### <a name="creating-the-custom-renderer-on-android"></a>Android에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 Android 플랫폼에 대 한 사용자 지정 렌더러를 보여 줍니다.

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.Droid
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Android.Webkit.WebView>
    {
        const string JavascriptFunction = "function invokeCSharpAction(data){jsBridge.invokeAction(data);}";
        Context _context;

        public HybridWebViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                var webView = new Android.Webkit.WebView(_context);
                webView.Settings.JavaScriptEnabled = true;
                webView.SetWebViewClient(new JavascriptWebViewClient($"javascript: {JavascriptFunction}"));
                SetNativeControl(webView);
            }
            if (e.OldElement != null)
            {
                Control.RemoveJavascriptInterface("jsBridge");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup();
            }
            if (e.NewElement != null)
            {
                Control.AddJavascriptInterface(new JSBridge(this), "jsBridge");
                Control.LoadUrl($"file:///android_asset/Content/{Element.Uri}");
            }
        }
    }
}
```

합니다 `HybridWebViewRenderer` 클래스에 지정 된 웹 페이지를 로드 합니다 `HybridWebView.Uri` 네이티브 속성 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 컨트롤 및 `invokeCSharpAction` 후 웹 페이지에 JavaScript 함수는 웹 페이지에 삽입 로드를 사용 하 여 완료 합니다 `OnPageFinished` 의 재정의 `JavascriptWebViewClient` 클래스:

```csharp
public class JavascriptWebViewClient : WebViewClient
{
    string _javascript;

    public JavascriptWebViewClient(string javascript)
    {
        _javascript = javascript;
    }

    public override void OnPageFinished(WebView view, string url)
    {
        base.OnPageFinished(view, url);
        view.EvaluateJavascript(_javascript, null);
    }
}
```

사용자가 해당 이름을 입력 하 고 HTML을 클릭 하면 `button` 요소는 `invokeCSharpAction` JavaScript 함수가 실행 됩니다. 이 기능을 수행 하려면 다음과 같습니다.

- 제공한를 `Control` 속성은 `null`, 다음 작업이 수행 됩니다.
  - 네이티브 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 인스턴스가 만들어질, 컨트롤에서 JavaScript가 설정으로 `JavascriptWebViewClient` 인스턴스가의 구현으로 설정 되어 `WebViewClient`입니다.
  - 합니다 `SetNativeControl` 메서드는 네이티브에 대 한 참조를 할당할 [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 컨트롤을 `Control` 속성입니다.
- 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결 되어 있는지를 제공 합니다.
  - 합니다 [ `WebView.AddJavascriptInterface` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/) 메서드는 새 삽입 `JSBridge` 인스턴스를 주 프레임으로 이름을 지정 하는 WebView JavaScript 컨텍스트의 `jsBridge`합니다. 메서드를 사용 하면이 `JSBridge` JavaScript에서 액세스 해야 하는 클래스입니다.
  - 합니다 [ `WebView.LoadUrl` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/) 에 지정 된 HTML 파일을 로드 하는 메서드는 `HybridWebView.Uri` 속성입니다. 코드 파일에 저장 되도록 지정 합니다 `Content` 프로젝트의 폴더입니다.
  - 에 `JavascriptWebViewClient` 클래스는 `invokeCSharpAction` JavaScript 함수는 페이지 로드가 완료 되 면 웹 페이지에 삽입 합니다.
- 때 렌더러가 변경 내용에 연결 됩니다.
  - 리소스가 해제 됩니다.

경우는 `invokeCSharpAction` JavaScript 함수는 실행를 차례로 호출 합니다 `JSBridge.InvokeAction` 메서드를 다음 코드 예제에 나와 있는:

```csharp
public class JSBridge : Java.Lang.Object
{
  readonly WeakReference<HybridWebViewRenderer> hybridWebViewRenderer;

  public JSBridge (HybridWebViewRenderer hybridRenderer)
  {
    hybridWebViewRenderer = new WeakReference <HybridWebViewRenderer> (hybridRenderer);
  }

  [JavascriptInterface]
  [Export ("invokeAction")]
  public void InvokeAction (string data)
  {
    HybridWebViewRenderer hybridRenderer;

    if (hybridWebViewRenderer != null && hybridWebViewRenderer.TryGetTarget (out hybridRenderer))
    {
      hybridRenderer.Element.InvokeAction (data);
    }
  }
}
```

클래스에서 파생 되어야 합니다 `Java.Lang.Object`, 및 JavaScript로 노출 되는 메서드가 지정 되어야 합니다는 `[JavascriptInterface]` 고 `[Export]` 특성입니다. 따라서 경우는 `invokeCSharpAction` , 호출 하는 JavaScript 함수는 웹 페이지에 삽입 하 고 실행 됩니다는 `JSBridge.InvokeAction` 데코 레이트 되 응답으로 메서드는 `[JavascriptInterface]` 및 `[Export("invokeAction")]` 특성. 차례로 합니다 `InvokeAction` 메서드를 호출 하는 `HybridWebView.InvokeAction` 메서드를이 되는 팝업을 표시할 등록 된 작업을 호출 합니다.

> [!NOTE]
> 사용 하는 프로젝트를 `[Export]` 특성에 대 한 참조를 포함 해야 `Mono.Android.Export`, 또는 컴파일러 오류가 발생 합니다.

합니다 `JSBridge` 클래스를 유지 관리를 `WeakReference` 에 `HybridWebViewRenderer` 클래스. 두 클래스 간에 순환 참조가 만들기를 방지 하기 위해서입니다. 자세한 내용은 참조 [약한 참조](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx) MSDN에 있습니다.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP의 사용자 지정 렌더러 만들기

다음 코드 예제에서는 UWP에 대 한 사용자 지정 렌더러를 보여 줍니다.

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.UWP
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Windows.UI.Xaml.Controls.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.external.notify(data);}";

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                SetNativeControl(new Windows.UI.Xaml.Controls.WebView());
            }
            if (e.OldElement != null)
            {
                Control.NavigationCompleted -= OnWebViewNavigationCompleted;
                Control.ScriptNotify -= OnWebViewScriptNotify;
            }
            if (e.NewElement != null)
            {
                Control.NavigationCompleted += OnWebViewNavigationCompleted;
                Control.ScriptNotify += OnWebViewScriptNotify;
                Control.Source = new Uri(string.Format("ms-appx-web:///Content//{0}", Element.Uri));
            }
        }

        async void OnWebViewNavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
        {
            if (args.IsSuccess)
            {
                // Inject JS script
                await Control.InvokeScriptAsync("eval", new[] { JavaScriptFunction });
            }
        }

        void OnWebViewScriptNotify(object sender, NotifyEventArgs e)
        {
            Element.InvokeAction(e.Value);
        }
    }
}
```

`HybridWebViewRenderer` 클래스에 지정 된 웹 페이지를 로드 합니다 `HybridWebView.Uri` 네이티브 속성 `WebView` 컨트롤 및 `invokeCSharpAction` JavaScript 함수는 웹 페이지에 로드 된 후 웹 페이지에 삽입와 `WebView.InvokeScriptAsync` 메서드. 사용자가 해당 이름을 입력 하 고 HTML을 클릭 하면 `button` 요소를 `invokeCSharpAction` JavaScript 함수를 실행 사용 하 여는 `OnWebViewScriptNotify` 웹 페이지에서 알림을 받으면 호출할 메서드. 이 메서드를 호출 하 여 `HybridWebView.InvokeAction` 팝업을 표시할 등록 된 작업을 호출 하는 메서드.

이 기능을 수행 하려면 다음과 같습니다.

- 제공한를 `Control` 속성은 `null`, 다음 작업이 수행 됩니다.
  - `SetNativeControl` 메서드는 새 네이티브 인스턴스화할 `WebView` 제어 하 고 하에 대 한 참조를 할당 합니다 `Control` 속성.
- 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결 되어 있는지를 제공 합니다.
  - 에 대 한 이벤트 처리기를 `NavigationCompleted` 및 `ScriptNotify` 이벤트 등록 됩니다. `NavigationCompleted` 될 때 이벤트 발생 하거나 네이티브 `WebView` 컨트롤 콘텐츠를 현재 로드 완료 되었거나 실패 한 탐색 합니다. 합니다 `ScriptNotify` 이벤트가 발생할 때 네이티브 콘텐츠 `WebView` 컨트롤은 응용 프로그램에는 문자열을 전달 하려면 JavaScript를 사용 합니다. 웹 페이지 발생 합니다 `ScriptNotify` 를 호출 하 여 이벤트 `window.external.notify` 전달 하는 동안는 `string` 매개 변수입니다.
  - 합니다 `WebView.Source` 속성이 지정 된 HTML 파일의 URI로 설정 되는 `HybridWebView.Uri` 속성입니다. 코드 파일에 저장 되어 있다고 가정 합니다 `Content` 프로젝트의 폴더입니다. 웹 페이지가 표시 되 면 합니다 `NavigationCompleted` 이벤트는 발생 및 `OnWebViewNavigationCompleted` 메서드가 호출 됩니다. 합니다 `invokeCSharpAction` JavaScript 함수를 사용 하 여 웹 페이지에 삽입 되는 `WebView.InvokeScriptAsync` 탐색 성공적으로 완료 하는 메서드를 제공 합니다.
- 때 렌더러가 변경 내용에 연결 됩니다.
  - 이벤트 구독을 않습니다.

## <a name="summary"></a>요약

이 문서에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 주었습니다는 `HybridWebView` JavaScript에서 C# 코드를 호출할 수 있도록 플랫폼별 웹 컨트롤을 개선 하는 방법을 보여 주는 사용자 지정 컨트롤입니다.


## <a name="related-links"></a>관련 링크

- [CustomRendererHybridWebView (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [JavaScript에서 C# 호출](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
