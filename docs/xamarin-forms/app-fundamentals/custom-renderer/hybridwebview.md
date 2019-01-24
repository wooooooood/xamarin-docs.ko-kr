---
title: HybridWebView 구현
description: 이 문서에서는 JavaScript에서 C# 코드를 호출할 수 있도록 플랫폼별 웹 컨트롤을 향상시키는 방법을 보여주는 HybridWebView 사용자 지정 컨트롤에 대한 사용자 지정 렌더러를 만드는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/19/2018
ms.openlocfilehash: f3b8cf7ec8a42ed031699d8f5e02f32c6eb61458
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53053869"
---
# <a name="implementing-a-hybridwebview"></a>HybridWebView 구현

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)

_Xamarin.Forms 사용자 지정 사용자 인터페이스 컨트롤은 화면에 레이아웃과 컨트롤을 배치하는 데 사용되는 보기 클래스에서 파생되어야 합니다. 이 문서에서는 JavaScript에서 C# 코드를 호출할 수 있도록 플랫폼별 웹 컨트롤을 향상시키는 방법을 보여주는 HybridWebView 사용자 지정 컨트롤에 대한 사용자 지정 렌더러를 만드는 방법을 보여줍니다._

모든 Xamarin.Forms 보기에는 네이티브 컨트롤의 인스턴스를 만드는 각 플랫폼에 대해 함께 제공되는 렌더러가 있습니다. iOS의 Xamarin.Forms 애플리케이션에서 [`View`](xref:Xamarin.Forms.View)을 렌더링하면 `ViewRenderer` 클래스가 인스턴스화되며, 차례로 네이티브 `UIView` 컨트롤이 인스턴스화됩니다. Android 플랫폼에서 `ViewRenderer` 클래스는 `View` 컨트롤을 인스턴스화합니다. UWP(유니버설 Windows 플랫폼)에서 `ViewRenderer` 클래스는 네이티브 `FrameworkElement` 컨트롤을 인스턴스화합니다. Xamarin.Forms 컨트롤에 매핑되는 렌더러 및 네이티브 컨트롤 클래스에 대한 자세한 내용은 [렌더러 기본 클래스 및 네이티브 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)을 참조하세요.

다음 다이어그램은 [`View`](xref:Xamarin.Forms.View) 및 이를 구현하는 해당 네이티브 컨트롤 간의 관계를 보여줍니다.

![](hybridwebview-images/view-classes.png "보기 클래스와 네이티브 클래스 구현 간의 관계")

렌더링 프로세스는 각 플랫폼에서 [`View`](xref:Xamarin.Forms.View)에 대한 사용자 지정 렌더러를 만들어 플랫폼별 사용자 지정을 구현하는 데 사용할 수 있습니다. 이 작업을 수행하는 프로세스는 다음과 같습니다.

1. `HybridWebView` 사용자 지정 컨트롤을 [만듭니다](#Creating_the_HybridWebView).
1. Xamarin.Forms에서 `HybridWebView`를 [사용합니다](#Consuming_the_HybridWebView).
1. 각 플랫폼의 `HybridWebView`에 대한 사용자 지정 렌더러를 [만듭니다](#Creating_the_Custom_Renderer_on_each_Platform).

이제 각 항목을 차례로 살펴보며 JavaScript에서 C# 코드를 호출할 수 있도록 플랫폼별 웹 컨트롤을 강화하는 `HybridWebView` 렌더러를 구현하겠습니다. `HybridWebView` 인스턴스는 사용자에게 이름 입력을 요청하는 HTML 페이지를 표시하는 데 사용됩니다. 사용자가 HTML 단추를 클릭하면 JavaScript 함수는 사용자 이름이 포함된 팝업을 표시하는 C# `Action`을 호출합니다.

JavaScript에서 C#을 호출하는 프로세스에 대한 자세한 내용은 [JavaScript에서 C# 호출](#Invoking_C_from_JavaScript)을 참조하세요. HTML 페이지에 대한 자세한 내용은 [웹 페이지 만들기](#Creating_the_Web_Page)를 참조하세요.

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>HybridWebView 만들기

다음 코드 예제와 같이 [`View`](xref:Xamarin.Forms.View) 클래스를 서브클래스로 만들어서 `HybridWebView` 사용자 지정 컨트롤을 만들 수 있습니다.

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

`HybridWebView` 사용자 지정 컨트롤은 .NET Standard 라이브러리 프로젝트에서 생성되고 컨트롤에 대한 다음 API를 정의합니다.

- 로드할 웹 페이지의 주소를 지정하는 `Uri` 속성.
- `Action`을 컨트롤에 등록하는 `RegisterAction` 메서드. 등록된 작업은 `Uri` 속성을 통해 참조되는 HTML 파일에 포함된 JavaScript에서 호출됩니다.
- 등록된 `Action`의 참조를 제거하는 `CleanUp` 메서드.
- 등록된 `Action`을 호출하는 `InvokeAction` 메서드. 이 메서드는 각 플랫폼별 프로젝트의 사용자 지정 렌더러에서 호출됩니다.

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>HybridWebView 사용

`HybridWebView` 사용자 지정 컨트롤은 해당 위치의 네임스페이스를 선언하고 사용자 지정 컨트롤의 네임스페이스 접두사를 사용하여 .NET 표준 라이브러리 프로젝트의 XAML에서 참조할 수 있습니다. 다음 코드 예제에서는 `HybridWebView` 사용자 지정 컨트롤을 XAML 페이지에서 사용하는 방법을 보여줍니다.

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

`local` 네임스페이스 접두사는 원하는 이름으로 지정할 수 있습니다. 그러나 `clr-namespace` 및 `assembly` 값은 사용자 지정 컨트롤의 세부 정보와 일치해야 합니다. 네임스페이스가 선언되면 사용자 지정 컨트롤을 참조하는 데 접두사가 사용됩니다.

다음 코드 예제에서는 C# 페이지에서 `HybridWebView` 사용자 지정 컨트롤을 사용하는 방법을 보여줍니다.

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

`HybridWebView` 인스턴스는 각 플랫폼에 네이티브 웹 컨트롤을 표시하는 데 사용됩니다. `Uri` 속성은 각 플랫폼별 프로젝트에 저장된 HTML 파일로 설정되며, 네이티브 웹 컨트롤을 통해 표시됩니다. 렌더링된 HTML은 사용자에게 이름 입력을 요청하고, HTML 단추 클릭에 대한 응답으로 C# `Action`을 호출하는 JavaScript 함수를 사용합니다.

`HybridWebViewPage`는 다음 코드 예제와 같이 JavaScript에서 호출할 작업을 등록합니다.

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

이 작업은 `HybridWebView` 인스턴스를 통해 HTML 페이지에서 입력된 이름을 표시하는 모달 팝업을 나타내는 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) 메서드를 호출합니다.

이제 사용자 지정 렌더러를 각 애플리케이션 프로젝트에 추가하여 JavaScript에서 C# 코드를 호출할 수 있도록 허용함으로써 플랫폼별 웹 컨트롤을 강화할 수 있습니다.

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 사용자 지정 컨트롤을 렌더링하는 `ViewRenderer<T1,T2>` 클래스의 서브클래스를 만듭니다. 첫 번째 형식 인수는 렌더러가 사용할(이 경우 `HybridWebView`) 사용자 지정 컨트롤이어야 합니다. 두 번째 형식 인수는 사용자 지정 보기를 구현할 네이티브 컨트롤이어야 합니다.
1. 사용자 지정 컨트롤을 렌더링하는 `OnElementChanged` 메서드를 정의하고 이를 사용자 지정하기 위한 논리를 작성합니다. 이 메서드는 해당 Xamarin.Forms 사용자 지정 컨트롤이 생성될 때 호출됩니다.
1. 사용자 지정 렌더러 클래스에 `ExportRenderer` 특성을 추가하여 Xamarin.Forms 사용자 지정 컨트롤을 렌더링하는 데 사용하도록 지정합니다. 이 특성은 사용자 지정 랜더러를 Xamarin.Forms에 등록하는 데 사용됩니다.

> [!NOTE]
> 대부분의 Xamarin.Forms 요소의 경우 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공하는 것은 선택 사항입니다. 사용자 지정 렌더러가 등록되지 않은 경우 컨트롤의 기본 클래스에 대한 기본 렌더러가 사용됩니다. 그러나 [보기](xref:Xamarin.Forms.View) 요소를 렌더링하는 경우에는 각 플랫폼 프로젝트에 사용자 지정 렌더러가 필요합니다.

다음 다이어그램은 샘플 애플리케이션에서 각 프로젝트의 책임과 이들 간의 관계를 보여줍니다.

![](hybridwebview-images/solution-structure.png "HybridWebView 사용자 지정 렌더러 프로젝트 책임")

`HybridWebView` 사용자 지정 컨트롤은 각 플랫폼의 `ViewRenderer` 클래스에서 모두 파생되는 플랫폼별 렌더러 클래스에 의해 렌더링됩니다. 그러면 다음 스크린샷과 같이 각 `HybridWebView` 사용자 지정 컨트롤이 플랫폼별 웹 컨트롤로 렌더링됩니다.

![](hybridwebview-images/screenshots.png "각 플랫폼의 HybridWebView")

`ViewRenderer` 클래스는 해당 네이티브 웹 컨트롤을 렌더링하기 위해 Xamarin.Forms 사용자 지정 컨트롤이 생성될 때 호출되는 `OnElementChanged` 메서드를 노출합니다. 이 메서드는 `OldElement` 및 `NewElement` 속성이 포함된 `ElementChangedEventArgs` 매개 변수를 가져옵니다. 이러한 속성은 랜더러가 연결*된* Xamarin.Forms 요소와 렌더러가 연결*되는* Xamarin.Forms 요소를 각각 나타냅니다. 샘플 애플리케이션에서 `OldElement` 속성은 `null`이고, `NewElement` 속성은 `HybridWebView` 인스턴스에 대한 참조를 포함합니다.

각 플랫폼별 렌더러 클래스에서 `OnElementChanged` 메서드의 재정의된 버전은 네이티브 웹 컨트롤 인스턴스화 및 사용자 지정을 수행하는 위치입니다. `SetNativeControl` 메서드는 네이티브 웹 컨트롤을 인스턴스화하는 데 사용되어야 하며, 이 메서드는 또한 `Control` 속성에 컨트롤 참조를 할당합니다. 또한 랜더링되는 Xamarin.Forms 컨트롤에 대한 참조는 `Element` 속성을 통해 얻을 수 있습니다.

그러나 일부 경우에는 `OnElementChanged` 메서드가 여러 번 호출될 수 있습니다. 따라서 메모리 누수를 방지하려면 새로운 네이티브 컨트롤을 인스턴스화할 때 주의를 기울여야 합니다. 사용자 지정 렌더러에서 새 네이티브 컨트롤을 인스턴스화할 때 사용하는 방법은 다음 코드 예제에 표시되어 있습니다.

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

`Control` 속성이 `null`일 경우 새 네이티브 컨트롤은 한 번만 인스턴스화되어야 합니다. 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결된 경우 이 컨트롤만 구성해야 합니다. 마찬가지로, 사용자 지정 렌더러가 변경된 항목에 연결된 경우 구독 대상 이벤트 처리기의 구독이 취소되어야 합니다. 이러한 방식을 채택하면 메모리 누수가 없는 성능이 뛰어난 사용자 지정 렌더러를 만드는 데 도움이 됩니다.

각 사용자 지정 렌더러 클래스는 랜더러를 Xamarin.Forms에 등록하는 `ExportRenderer` 속성으로 데코레이트됩니다. 이 특성은 렌더링되는 Xamarin.Forms 사용자 지정 컨트롤의 형식 이름과 지정 렌더러의 형식 이름이라는 두 가지 매개 변수를 사용합니다. 특성의 `assembly` 접두사는 특성이 전체 어셈블리에 적용되도록 지정합니다.

다음 섹션에서는 각 네이티브 웹 컨트롤에서 로드하는 웹 페이지의 구조, JavaScript에서 C#을 호출하는 프로세스, 각 플랫폼별 사용자 지정 렌더러 클래스에서 이를 구현하는 방법을 설명합니다.

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>웹 페이지 만들기

다음 코드 예제는 `HybridWebView` 사용자 지정 컨트롤이 표시할 웹 페이지를 보여줍니다.

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

이 웹 페이지는 사용자가 `input` 요소에서 자신의 이름을 입력하는 것을 허용하며, 클릭하면 C# 코드를 호출하는 `button` 요소를 제공합니다. 이를 구현하는 프로세스는 다음과 같습니다.

- 사용자가 `button` 요소를 클릭하면 `invokeCSCode` JavaScript 함수가 호출되고 `input` 요소의 값이 함수로 전달됩니다.
- `invokeCSCode` 함수는 C# `Action`으로 보내는 데이터를 표시하는 `log` 함수를 호출합니다. 그 후 C# `Action`을 호출하는 `invokeCSharpAction` 메서드를 호출하고, `input` 요소에서 받은 매개 변수를 전달합니다.

`invokeCSharpAction` JavaScript 함수는 웹 페이지에 정의되지 않으며, 각 사용자 지정 렌더러를 통해 삽입됩니다.

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>JavaScript에서 C# 호출

JavaScript에서 C#을 호출하는 프로세스는 각 플랫폼에서 동일합니다.

- 사용자 지정 렌더러는 네이티브 웹 컨트롤을 만들고 `HybridWebView.Uri` 속성에 지정된 HTML 파일을 로드합니다.
- 웹 페이지가 로드되면 사용자 지정 렌더러는 `invokeCSharpAction` JavaScript 함수를 웹 페이지에 삽입합니다.
- 사용자가 이름을 입력하고 HTML `button` 요소를 클릭하면 `invokeCSCode` 함수가 호출되고, 이 함수는 `invokeCSharpAction` 함수를 호출합니다.
- `invokeCSharpAction` 함수는 사용자 지정 렌더러의 메서드를 호출하고, 이 메서드는 `HybridWebView.InvokeAction` 메서드를 호출합니다.
- `HybridWebView.InvokeAction` 메서드는 등록된 `Action`을 호출합니다.

다음 섹션에서는 각 플랫폼에서 이 프로세스를 구현하는 방식을 설명합니다.

### <a name="creating-the-custom-renderer-on-ios"></a>iOS에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 iOS 플랫폼용 사용자 지정 렌더러를 보여줍니다.

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

`HybridWebViewRenderer` 클래스는 `HybridWebView.Uri` 속성에 지정된 웹 페이지를 네이티브 [`WKWebView`](https://developer.xamarin.com/api/type/WebKit.WKWebView/) 컨트롤에 로드하고, `invokeCSharpAction` JavaScript 함수는 이 웹 페이지에 삽입됩니다. 사용자가 이름을 입력하고 HTML `button` 요소를 클릭하면 `invokeCSharpAction` JavaScript 함수가 실행되고, 웹 페이지에서 메시지를 받은 후 `DidReceiveScriptMessage` 메서드가 호출됩니다. 그러면 이 메서드는 팝업을 표시하도록 등록된 작업을 호출하는 `HybridWebView.InvokeAction` 메서드를 호출합니다.

이 기능을 얻는 방법은 다음과 같습니다.

- `Control` 속성이 `null`이면 다음 작업이 수행됩니다.
  - 웹 페이지에 메시지를 게시하고 사용자 스크립트를 삽입하는 것을 허용하는 [`WKUserContentController`](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) 인스턴스가 만들어집니다.
  - 웹 페이지가 로드된 후 `invokeCSharpAction` JavaScript 함수를 웹 페이지에 삽입하는 [`WKUserScript`](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) 인스턴스가 만들어집니다.
  - [`WKUserContentController.AddScript`](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/) 메서드는 콘텐츠 컨트롤러에 [`WKUserScript`](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) 인스턴스를 추가합니다.
  - [`WKUserContentController.AddScriptMessageHandler`](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/) 메서드는 [`WKUserContentController`](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) 인스턴스에 `invokeAction`이라는 스크립트 메시지 처리기를 추가합니다. 그러면 `WKUserContentController` 인스턴스를 사용할 모든 웹 보기의 모든 프레임에서 JavaScript 함수 `window.webkit.messageHandlers.invokeAction.postMessage(data)`가 정의됩니다.
  - [`WKWebViewConfiguration`](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/) 인스턴스가 만들어지고, [`WKUserContentController`](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) 인스턴스가 콘텐츠 컨트롤러로 설정됩니다.
  - [`WKWebView`](https://developer.xamarin.com/api/type/WebKit.WKWebView/) 컨트롤이 인스턴스화되고, `WKWebView` 컨트롤의 참조를 `Control` 속성에 할당하는 `SetNativeControl` 메서드가 호출됩니다.
- 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우:
  - [`WKWebView.LoadRequest`](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/) 메서드는 `HybridWebView.Uri` 속성에 지정된 HTML 파일을 로드합니다. 이 코드는 프로젝트의 `Content` 폴더에 파일이 저장되도록 지정합니다. 웹 페이지가 표시되면 `invokeCSharpAction` JavaScript 함수가 웹 페이지에 삽입됩니다.
- 렌더러가 연결된 요소가 변경되면:
  - 리소스가 해제됩니다.

> [!NOTE]
> `WKWebView` 클래스는 iOS 8 이상에서만 지원됩니다.

### <a name="creating-the-custom-renderer-on-android"></a>Android에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 Android 플랫폼용 사용자 지정 렌더러를 보여줍니다.

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

`HybridWebViewRenderer` 클래스는 `HybridWebView.Uri` 속성에 지정된 웹 페이지를 네이티브 [`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 컨트롤에 로드하고, `invokeCSharpAction` JavaScript 함수는 이 웹 페이지에 삽입됩니다. 웹 페이지가 완전히 로드되면 `JavascriptWebViewClient` 클래스에서 `OnPageFinished`가 재정의됩니다.

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

사용자가 이름을 입력하고 HTML `button` 요소를 클릭하면 `invokeCSharpAction` JavaScript 함수가 실행됩니다. 이 기능을 얻는 방법은 다음과 같습니다.

- `Control` 속성이 `null`이면 다음 작업이 수행됩니다.
  - 네이티브 [`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 인스턴스가 만들어지고, 컨트롤에서 JavaScript를 사용하도록 설정되고, `WebViewClient`의 구현으로 `JavascriptWebViewClient` 인스턴스가 설정됩니다.
  - 네이티브 [`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 컨트롤의 참조를 `Control` 속성에 할당하는 `SetNativeControl` 메서드가 호출됩니다.
- 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우:
  - [`WebView.AddJavascriptInterface`](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/) 메서드는 WebView의 JavaScript 컨텍스트 주 프레임에 새 `JSBridge` 인스턴스를 삽입하고 이름을 `jsBridge`로 지정합니다. 이렇게 하면 `JSBridge` 클래스의 메서드를 JavaScript에서 액세스할 수 있습니다.
  - [`WebView.LoadUrl`](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/) 메서드는 `HybridWebView.Uri` 속성에 지정된 HTML 파일을 로드합니다. 이 코드는 프로젝트의 `Content` 폴더에 파일이 저장되도록 지정합니다.
  - `JavascriptWebViewClient` 클래스에서 페이지 로드가 완료되면 `invokeCSharpAction` JavaScript 함수가 웹 페이지에 삽입됩니다.
- 렌더러가 연결된 요소가 변경되면:
  - 리소스가 해제됩니다.

`invokeCSharpAction` JavaScript 함수는 실행되면 다음 코드 예제에 보이는 `JSBridge.InvokeAction` 메서드를 호출합니다.

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

클래스는 `Java.Lang.Object`에서 파생되어야 하며, JavaScript에 공개되는 메서드는 `[JavascriptInterface]` 및 `[Export]` 특성으로 데코레이트해야 합니다. 따라서 `invokeCSharpAction` JavaScript 함수는 `[JavascriptInterface]` 및 `[Export("invokeAction")]` 특성으로 데코레이트되었기 때문에 웹 페이지에 삽입 및 실행되면 `JSBridge.InvokeAction` 메서드를 호출하게 됩니다. 그러면 `InvokeAction` 메서드는 팝업을 표시하도록 등록된 작업을 호출하는 `HybridWebView.InvokeAction` 메서드를 호출합니다.

> [!NOTE]
> `[Export]` 특성을 사용하는 프로젝트는 `Mono.Android.Export` 참조를 포함해야 하며, 그렇지 않으면 컴파일러 오류가 발생합니다.

`JSBridge` 클래스는 `WeakReference`를 `HybridWebViewRenderer` 클래스로 유지합니다. 이는 두 클래스 간에 순환 참조가 만들어지지 않도록 방지하기 위한 조치입니다. 자세한 내용은 MSDN에서 [약한 참조](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx)를 살펴보세요.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP에서 사용자 지정 렌더러 만들기

다음 코드 예제는 UWP용 사용자 지정 렌더러를 보여줍니다.

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

`HybridWebViewRenderer` 클래스는 `HybridWebView.Uri` 속성에 지정된 웹 페이지를 네이티브 `WebView` 컨트롤에 로드하고, `WebView.InvokeScriptAsync` 메서드를 통해 웹 페이지가 로드되면 `invokeCSharpAction` JavaScript 함수가 이 웹 페이지에 삽입됩니다. 사용자가 이름을 입력하고 HTML `button` 요소를 클릭하면 `invokeCSharpAction` JavaScript 함수가 실행되고, 웹 페이지에서 알림을 받은 후 `OnWebViewScriptNotify` 메서드가 호출됩니다. 그러면 이 메서드는 팝업을 표시하도록 등록된 작업을 호출하는 `HybridWebView.InvokeAction` 메서드를 호출합니다.

이 기능을 얻는 방법은 다음과 같습니다.

- `Control` 속성이 `null`이면 다음 작업이 수행됩니다.
  - `SetNativeControl` 메서드가 호출되어 새 네이티브 `WebView` 컨트롤을 인스턴스화하고 `Control` 속성에 대한 참조를 할당합니다.
- 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우:
  - `NavigationCompleted` 및 `ScriptNotify` 이벤트에 대한 이벤트 처리기가 등록됩니다. 네이티브 `WebView` 컨트롤이 현재 콘텐츠를 완전히 로드하거나 탐색이 실패하면 `NavigationCompleted` 이벤트가 발생합니다. 네이티브 `WebView` 컨트롤의 콘텐츠가 JavaScript를 사용하여 애플리케이션에 문자열을 전달하면 `ScriptNotify` 이벤트가 발생합니다. 웹 페이지는 `string` 매개 변수를 전달하는 동안 `window.external.notify`를 호출하여 `ScriptNotify` 이벤트를 발생시킵니다.
  - `WebView.Source` 속성은 `HybridWebView.Uri` 속성에 지정된 HTML 파일의 URI로 설정됩니다. 이 코드는 프로젝트의 `Content` 폴더에 파일이 저장되는 것으로 가정합니다. 웹 페이지가 표시되면 `NavigationCompleted` 이벤트가 발생하고 `OnWebViewNavigationCompleted` 메서드가 호출됩니다. 탐색이 성공적으로 완료되면 `WebView.InvokeScriptAsync` 메서드를 통해 `invokeCSharpAction` JavaScript 함수가 웹 페이지에 삽입됩니다.
- 렌더러가 연결된 요소가 변경되면:
  - 이벤트가 구독 취소됩니다.

## <a name="summary"></a>요약

이 문서에서는 JavaScript에서 C # 코드를 호출할 수 있도록 플랫폼별 웹 컨트롤을 향상시키는 방법을 보여주는 `HybridWebView` 사용자 지정 컨트롤에 대한 사용자 지정 렌더러를 만드는 방법을 살펴보았습니다.


## <a name="related-links"></a>관련 링크

- [CustomRendererHybridWebView(샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [JavaScript에서 C# 호출](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
