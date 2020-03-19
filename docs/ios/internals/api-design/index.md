---
title: Xamarin.iOS API 디자인
description: Xamarin.iOs API 설계에 사용된 원칙과 이러한 원칙이 Objective-C와 관련되는 방식을 안내합니다.
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: a2435b30b7d5b468fca6c55d295c87b9a0d20652
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79303728"
---
# <a name="xamarinios-api-design"></a>Xamarin.iOS API 디자인

Mono의 일부인 핵심 기본 클래스 라이브러리 외에도, [Xamarin.iOS](~/ios/index.yml)는 개발자가 Mono를 사용해 네이티브 iOS 애플리케이션을 만들 수 있도록 다양한 iOS API 바인딩과 함께 제공됩니다.

Xamarin.iOS의 핵심은 C# 환경과 Objective-C 환경을 연결하는 interop 엔진이며, Xamarin.iOS에는 CoreGraphics 또는 [OpenGL](#opengles)과 같은 iOS C 기반 API용 바인딩도 포함됩니다.

Objective-C 코드와 통신하는 하위 수준 런타임은 [MonoTouch.ObjCRuntime](#objcruntime)에 있습니다. 이를 토대로 [Foundation](#foundation), CoreFoundation 및 [UIKit](#uikit)에 대한 바인딩이 제공됩니다.

## <a name="design-principles"></a>디자인 원칙

다음은 Xamarin.iOS 바인딩에 대한 몇 가지 디자인 원칙입니다(이는 Xamarin.Mac에도 적용되며, macOS에서는 Objective-C에 대한 Mono 바인딩입니다).

- [프레임워크 디자인 지침](https://docs.microsoft.com/dotnet/standard/design-guidelines) 준수
- 개발자가 Objective-C 클래스를 서브클래싱하도록 허용.

  - 기존 클래스에서 파생
  - 기본 생성자를 호출하여 연결
  - 재정의 메서드는 C#의 재정의 시스템에서 작동해야 함
  - 서브클래싱은 C# 표준 구문에서 작동해야 함

- 개발자에게 Objective-C 선택기를 노출하지 않음
- 임의의 Objective-C 라이브러리를 호출하는 메커니즘 제공
- 일반적인 Objective-C 작업을 용이하게 하고 까다로운 Objective-C 작업은 가능하게 함
- Objective-C 속성을 C# 속성으로 노출
- 강력한 형식의 API 노출:

  - 형식 안전성 향상
  - 런타임 오류 최소화
  - 반환 형식에 대한 IDE IntelliSense 가져오기
  - IDE 팝업 설명서 허용

- API의 IDE 내 탐색 권장

  - 예를 들어 다음과 같은 약한 형식의 배열을 노출하는 대신

    ```objc
    NSArray *getViews
    ```

    다음과 같이 강력한 형식을 노출합니다.

    ```csharp
    NSView [] Views { get; set; }
    ```

    이렇게 하면 Mac용 Visual Studio가 API를 검색하는 동안 자동 완성 기능을 수행할 수 있고, 모든 `System.Array` 작업을 반환된 값에 사용할 수 있으며, 반환 값이 LINQ에 참여할 수 있게 됩니다.

- 네이티브 C# 형식:

  - [`NSString`은 `string`](~/ios/internals/api-design/nsstring.md)이 됨
  - 열거형이어야 하는`int` 및 `uint` 매개 변수를 C# 열거형 및 `[Flags]` 특성을 가진 C# 열거형으로 변환
  - 형식 중립적인 `NSArray` 개체 대신 배열을 강력한 형식의 배열로 노출
  - 이벤트 및 알림에 대해 사용자에게 다음 중 하나의 선택 사항 제공:

    - 기본적으로 강력한 형식의 버전
    - 고급 사용 사례를 위한 약한 형식의 버전

- Objective-C 대리자 패턴 지원:

  - C# 이벤트 시스템
  - C# 대리자(람다, 무명 메서드 및 `System.Delegate`)를 Objective-C API에 블록으로 노출

### <a name="assemblies"></a>어셈블리

Xamarin.iOS에는 *Xamarin.iOS 프로필*을 구성하는 많은 어셈블리가 포함되어 있습니다. 자세한 내용은 [어셈블리](~/cross-platform/internals/available-assemblies.md) 페이지를 참조하세요.

### <a name="major-namespaces"></a>주요 네임스페이스

#### <a name="objcruntime"></a>ObjCRuntime

[ObjCRuntime](xref:ObjCRuntime) 네임스페이스를 사용하면 개발자가 C# 환경과 Objective-C 환경을 연결할 수 있습니다.
이는 Cocoa# 및 Gtk# 환경을 기반으로 iOS용으로 특별히 디자인된 새로운 바인딩입니다.

#### <a name="foundation"></a>Foundation

[Foundation](xref:Foundation) 네임스페이스는 iOS의 일부인 Objective-C Foundation 프레임워크와 상호 운용되도록 디자인된 기본 데이터 형식을 제공하며,이는 Objective-C의 개체 지향 프로그래밍을 위한 기준입니다.

Xamarin.iOS는 C#에서 Objective-C 클래스의 계층 구조를 미러링합니다. 예를 들어, Objective-C 기본 클래스 NSObject는 [Foundation.NSObject](xref:Foundation.NSObject)를 통해 C#에서 사용할 수 있습니다.

이 네임스페이스는 기본 Objective-C Foundation 형식에 대한 바인딩을 제공하지만, 몇 가지 경우 기본 형식이 .NET 형식에 매핑되어 있습니다. 예를 들어:

- 런타임은 NSString 및 [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html)를 처리하는 대신 API를 통해 C# [문자열](xref:System.String)과 강력한 형식의 [배열](xref:System.Array)로 노출합니다.

- 여기에는 다양한 도우미 API가 노출되어 있으며, 개발자는 타사 Objective-C API, 기타 iOS API 또는 현재 Xamarin.iOS로 바인딩되지 않은 API를 바인딩할 수 있습니다.

API 바인딩에 대한 자세한 내용은 [Xamarin.iOS 바인딩 생성기](~/cross-platform/macios/binding/binding-types-reference.md) 섹션을 참조하세요.

##### <a name="nsobject"></a>NSObject

[NSObject](xref:Foundation.NSObject) 형식은 모든 Objective-C 바인딩의 기초입니다. Xamarin.iOS 형식은 iOS CocoaTouch API에서 C 형식(일반적으로 CoreFoundation 형식이라고 함)과 Objective-C 형식(모두 NSObject 클래스에서 파생됨)이라는 두 가지 형식의 클래스를 미러링합니다.

관리되지 않는 형식을 미러링하는 각 형식에 대해 [핸들](xref:Foundation.NSObject.Handle) 속성을 통해 네이티브 개체를 가져올 수 있습니다.

Mono는 모든 개체에 대한 가비지 수집을 제공하지만 `Foundation.NSObject` 가 [System.IDisposable](xref:System.IDisposable) 인터페이스를 구현합니다. 즉, 가비지 수집기가 시작될 때까지 기다리지 않고도 지정된 NSObject의 리소스를 명시적으로 해제할 수 있습니다. 이는 많은 NSObjects를 사용하는 경우 중요합니다(예: 포인터를 큰 데이터 블록에 보유할 수도 있는 UIImages).

형식이 명확한 종료를 수행해야 하는 경우 [NSObject.Dispose(부울) 방법](xref:Foundation.NSObject.Dispose(System.Boolean))을 재정의하세요. Dispose에 대한 매개 변수는 "bool disposing"이며, true로 설정된 경우 이는 사용자가 개체에 대해 명시적으로 Dispose ()를 호출했으므로 Dispose 메서드가 호출되고 있음을 의미합니다. 값이 false이면 종료자 스레드의 종료자에서 Dispose(bool disposing) 메서드가 호출되고 있음을 의미합니다.

##### <a name="categories"></a>범주

Xamarin.iOS 8.10부터 C#에서 Objective-C 범주를 만들 수 있습니다.

이 작업은 `Category` 특성을 사용해 수행되며 특성에 대한 인수로 확장할 형식을 지정합니다. 다음 예제에서는 NSString을 확장해 보겠습니다.

```csharp
[Category (typeof (NSString))]
```

각 범주 메서드는 `Export` 특성을 사용 하여 메서드를 Objective-C로 내보내기 위해 일반적인 메커니즘을 사용합니다.

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

모든 관리 확장 메서드는 정적이어야 하지만, C#의 확장 메서드에 표준 구문을 사용하여 Objective-C 인스턴스 메서드를 만들 수 있습니다.

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

확장 메서드의 첫 번째 인수는 메서드가 호출된 인스턴스입니다.

완성된 예제:

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
}
```

이 예제에서는 네이티브 toUpper 인스턴스 메서드를 NSString 클래스에 추가합니다.이 클래스는 Objective-C에서 호출할 수 있습니다.

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAutoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

이 방식은 코드베이스의 전체 클래스 집합에 메서드를 추가하는 시나리오에서 유용합니다. 예를 들어, 이를 통해 모든 `UIViewController` 인스턴스가 회전 가능한 것으로 보고할 수 있습니다.

```csharp
[Category (typeof (UINavigationController))]
class Rotation_IOS6 {
      [Export ("shouldAutorotate:")]
      static bool ShouldAutoRotate (this UINavigationController self)
      {
          return true;
      }
}
```

##### <a name="preserveattribute"></a>PreserveAttribute

PreserveAttribute는 응용 프로그램의 크기를 줄이기 위해 응용 프로그램을 처리하는 단계에서 mtouch(Xamarin.iOS 배포 도구)가 형식 또는 형식 멤버를 유지하도록 알리는 데 사용되는 사용자 지정 특성입니다.

애플리케이션이 정적으로 연결하지 않은 모든 멤버는 제거됩니다. 따라서 이 특성은 정적으로 참조되지 않는 멤버를 표시하는 데 사용되지만 여전히 응용 프로그램에 필요합니다.

예를 들어 형식을 동적으로 인스턴스화하는 경우 형식의 기본 생성자를 유지하는 것이 좋습니다. XML serialization을 사용하는 경우 형식의 속성을 유지하는 것이 좋습니다.

같은 형식의 모든 멤버 또는 형식 자체에 이 특성을 적용할 수 있습니다. 전체 형식을 유지하려는 경우 해당 형식에 [Preserve (AllMembers = true)] 구문을 사용할 수 있습니다.

#### <a name="uikit"></a>UIKit

[UIKit](xref:UIKit) 네임스페이스에는 C# 클래스 형식으로 CocoaTouch를 구성하는 모든 UI 구성 요소에 대한 일대일 매핑이 포함되어 있습니다. API는 C# 언어에서 사용되는 규칙을 따르도록 수정되었습니다.

C# 대리자는 일반적인 작업을 위해 제공됩니다. 자세한 내용은 [대리자](#delegates) 섹션을 참조하세요.

#### <a name="opengles"></a>OpenGLES

OpenGLES의 경우 CoreGraphics 데이터 형식 및 구조를 사용하도록 수정된 OpenGL에 [OpenTK](https://opentk.net/) API의 [수정된 버전](xref:OpenTK)과 개체 지향 바인딩을 배포하고, iOS에서 제공되는 기능만 노출합니다.

OpenGLES 1.1 기능은 [ES11.GL 형식](xref:OpenTK.Graphics.ES11.GL)을 통해 사용할 수 있습니다.

OpenGLES 2.0 기능은 [ES20.GL 형식](xref:OpenTK.Graphics.ES20.GL)을 통해 사용할 수 있습니다.

OpenGLES 3.0 기능은 [ES30.GL 형식](xref:OpenTK.Graphics.ES30.GL)을 통해 사용할 수 있습니다.

### <a name="binding-design"></a>바인딩 디자인

Xamarin.iOS는 단순히 기본 Objective-C 플랫폼에 대한 바인딩이 아니며, .NET 형식 시스템을 확장하고 C#와 Objective-C를 더 효과적으로 혼합하도록 시스템을 디스패치합니다.

P/Invoke가 Windows 및 Linux에서 네이티브 라이브러리를 호출하는 데 유용한 도구이며 IJW 지원이 Windows에서 COM interop에 대해 사용될 수 있는 것처럼, Xamarin.iOS는 런타임을 확장하여 C# 개체를 Objective-C 개체로 바인딩하는 것을 지원합니다.

Xamarin.iOS 응용 프로그램을 만드는 사용자에게는 다음 몇 개 섹션의 설명이 꼭 필요하지는 않지만, 개발자가 작업을 수행하는 방법을 이해하고 더 복잡한 응용 프로그램을 만드는 데 도움을 줍니다.

#### <a name="types"></a>유형

적절히 완료된 경우 C# 영역에 하위 수준의 Foundation 형식 대신 C# 형식이 노출됩니다.  즉, [API가 NSString 대신 C# "문자열" 형식을 사용하고 ](~/ios/internals/api-design/nsstring.md) NSArray를 노출하는 대신 강력한 형식의 C# 배열을 사용합니다.

일반적으로 Xamarin.iOS 및 Xamarin.Mac 디자인에서는 기본 `NSArray` 개체가 노출되지 않습니다. 대신 런타임은 `NSArray`를 일부 `NSObject` 클래스의 강력한 형식의 배열로 자동 변환합니다. 따라서 Xamarin.iOS는 NSArray를 반환하기 위해 GetViews와 같은 약한 형식의 메서드를 노출하지 않습니다.

```csharp
NSArray GetViews ();
```

대신, 바인딩이 다음과 같이 강력한 형식의 반환 값을 노출합니다.

```csharp
UIView [] GetViews ();
```

`NSArray`를 직접 사용하려는 경우에는 `NSArray`에 몇 가지 방법이 노출되지만 API 바인딩에서 사용하지 않는 것이 좋습니다.

또한 **Classic API**에서 CoreGraphics API의 `CGRect`, `CGPoint`, `CGSize`를 사용하는 대신, 개발자가 OpenTK를 사용하는 기존 OpenGL을 유지할 수 있도록 `System.Drawing` 구현 `RectangleF`, `PointF` 및 `SizeF`로 교체했습니다. 새 64비트 **Unified API**를 사용하는 경우 CoreGraphics API를 사용해야 합니다.

#### <a name="inheritance"></a>상속

Xamarin.iOS API 디자인을 통해 개발자는 파생 클래스에서 "override" 키워드를 사용하고 "base" C# 키워드를 사용하여 기본 구현에 연결하여 C# 형식을 확장하는 것과 같은 방식으로 네이티브 Objective-C 형식을 확장할 수 있습니다.

이 디자인을 사용하면 전체 Objective-C 시스템이 이미 Xamarin.iOS 라이브러리 내에 래핑되어 있으므로 개발자는 Objective-C 선택기를 개발 프로세스의 일부로 처리하지 않아도 됩니다.

#### <a name="types-and-interface-builder"></a>형식 및 Interface Builder

Interface Builder에서 만든 형식의 인스턴스인 .NET 클래스를 만들 때 단일 `IntPtr` 매개 변수를 사용하는 생성자를 제공해야 합니다.
이는 관리되는 개체 인스턴스를 관리되지 않는 개체에 바인딩하는 데 필요합니다.
코드는 다음과 같이 한 줄로 구성됩니다.

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

#### <a name="delegates"></a>대리자

Objective-C와 C#은 각 언어의 단어 대리자에 대해 서로 다른 의미를 갖습니다.

Objective-C 환경과 CocoaTouch에 대한 온라인에서 볼 수 있는 설명서에서 대리자는 일반적으로 메서드 집합에 응답하는 클래스의 인스턴스입니다. 이는 C# 인터페이스와 매우 비슷하지만 메서드가 항상 필요하지는 않다는 차이점이 있습니다.

이러한 대리자는 UIKit 및 기타 CocoaTouch API에서 중요한 역할을 합니다. 이러한 대리자는 다음과 같은 다양한 작업을 수행하는 데 사용됩니다.

- 코드에 알림 제공(C# 또는 Gtk+의 이벤트 배달과 유사)
- 데이터 시각화 컨트롤에 대한 모델 구현
- 컨트롤의 동작을 구동

프로그래밍 패턴은 컨트롤을 위한 동작을 변경하기 위한 파생 클래스 생성을 최소화하도록 디자인되었습니다. 이 솔루션은 본질적으로 다른 GUI 도구 키트가 몇 년 동안 수행한 것과 비슷합니다. 예: Gtk의 신호, Qt 슬롯, Winforms 이벤트, WPF/Silverlight 이벤트 등. 수백 개의 인터페이스(각 작업별 하나씩)를 사용하거나 개발자가 불필요한 메서드를 너무 많이 구현할 필요가 없도록 Objective-C는 선택적 메서드 정의를 지원합니다. 이는 모든 메서드를 구현해야 하는 C# 인터페이스와 다릅니다.

Objective-C 클래스에서 이 프로그래밍 패턴을 사용하는 클래스가 일반적으로 `delegate`라고 하는 속성을 노출하는 것을 확인할 수 있습니다.이 속성은 인터페이스의 필수 부분과 0개 이상의 선택적 부분을 구현하는 데 필요합니다.

Xamarin.iOS에서는 이러한 대리자에 바인딩하는 세 가지 상호 배타적인 메커니즘이 제공됩니다.

1. [Via 이벤트](#via-events).
2. [`Delegate` 속성을 통한 강력한 형식](#strongly-typed-via-a-delegate-property)
3. [`WeakDelegate` 속성을 통한 느슨한 형식](#loosely-typed-via-the-weakdelegate-property)

UIWebView 클래스를 예로 들어 보겠습니다. 이 클래스는 대리자 속성에 할당된 UIWebViewDelegate 인스턴스로 디스패치합니다.

##### <a name="via-events"></a>Via 이벤트

많은 형식에 대해 Xamarin.iOS는 `UIWebViewDelegate` 호출을 C# 이벤트로 전달하는 적절한 대리자를 자동으로 만듭니다. `UIWebView`의 경우:

- WebViewDidStartLoad 메서드는 [UIWebView.LoadStarted](xref:UIKit.UIWebView.LoadStarted) 이벤트에 매핑됩니다.
- webViewDidFinishLoad 메서드는 [UIWebView.LoadFinished](xref:UIKit.UIWebView.LoadFinished) 이벤트에 매핑됩니다.
- webView:didFailLoadWithError 메서드는 [UIWebView.LoadError](xref:UIKit.UIWebView.LoadError) 이벤트에 매핑됩니다.

예를 들어 이 간단한 프로그램은 웹 보기를 로드할 때 시작 및 종료 시간을 기록합니다.

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```

##### <a name="via-properties"></a>Via 속성

이벤트는 해당 이벤트에 둘 이상의 구독자가 있을 수 있는 경우에 유용합니다. 또한 이벤트는 코드의 반환 값이 없는 경우에만 사용할 수 있습니다.

코드에서 값을 반환해야 하는 경우에는 속성 대신 옵트인합니다. 이는 개체의 지정된 시간에 메서드를 하나만 설정할 수 있음을 의미합니다.

예를 들어 이 메커니즘을 사용하여 `UITextField` 처리기의 화면에서 키보드를 해제할 수 있습니다.

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

이 경우 `UITextField`의 `ShouldReturn` 속성은 부울 값을 반환하는 대리자를 인수로 사용하고, TextField가 반환 버튼을 누른 상태로 작업을 수행해야 하는지 여부를 결정합니다. 이 메서드에서는 호출자에게 *true*를 반환하지만 화면에서 키보드를 제거합니다(이는 textfield가 `ResignFirstResponder`를 호출할 때 발생함).

##### <a name="strongly-typed-via-a-delegate-property"></a>대리자 속성을 통한 강력한 형식

이벤트를 사용하지 않으려는 경우 사용자 고유의 [UIWebViewDelegate](xref:UIKit.UIWebViewDelegate) 서브클래스를 제공하고 이를 [UIWebView.Delegate](xref:UIKit.UIWebView.Delegate) 속성에 할당할 수 있습니다. UIWebView.Delegate가 할당되면 UIWebView 이벤트 디스패치 메커니즘은 더 이상 작동하지 않으며, 해당 이벤트가 발생할 때 UIWebViewDelegate 메서드가 호출됩니다.

예를 들어 이 단순 형식은 웹 보기를 로드하는 데 걸리는 시간을 기록합니다.

```csharp
class Notifier : UIWebViewDelegate  {
    DateTime startTime, endTime;

    public override LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    public override LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}
```

위의 내용은 다음과 같은 코드에 사용됩니다.

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

위의 내용은 UIWebViewer를 만들고 메시지에 응답하기 위해 만든 클래스인 알림 인스턴스로 메시지를 보내도록 지시합니다.

이 패턴은 특정 컨트롤에 대한 동작을 제어하는 데에도 사용됩니다. 예를 들어 UIWebView의 경우에는 [UIWebView.ShouldStartLoad](xref:UIKit.UIWebView.ShouldStartLoad) 속성을 통해 `UIWebView` 인스턴스에서 `UIWebView`가 페이지를 로드할지 여부를 제어하도록 할 수 있습니다.

패턴은 몇 가지 컨트롤에 대해 요청 시 데이터를 제공하는 경우에도 사용됩니다. 예를 들어 [UITableView](xref:UIKit.UITableView) 컨트롤은 강력한 테이블 렌더링 컨트롤이며, 모양과 내용은 [UITableViewDataSource](xref:UIKit.UITableViewDataSource)의 인스턴스를 기반으로 합니다.

### <a name="loosely-typed-via-the-weakdelegate-property"></a>WeakDelegate 속성을 통한 느슨한 형식화

강력한 형식의 속성 외에도 개발자가 원할 경우 다른 방식으로 바인딩할 수 있는 약한 형식의 대리자도 있습니다.
강력한 형식의 `Delegate` 속성이 Xamarin.iOS의 바인딩에 노출되는 모든 위치에서 해당 `WeakDelegate` 속성도 함께 노출됩니다.

`WeakDelegate`를 사용하는 경우 사용자는 [Export](xref:Foundation.ExportAttribute) 특성을 사용하여 클래스를 적절하게 데코레이팅해 선택기를 지정해야 합니다. 예를 들어:

```csharp
class Notifier : NSObject  {
    DateTime startTime, endTime;

    [Export ("webViewDidStartLoad:")]
    public void LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    [Export ("webViewDidFinishLoad:")]
    public void LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}

[...]

var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.WeakDelegate = new Notifier ();
```

`WeakDelegate` 속성이 할당되면 `Delegate` 속성은 사용되지 않습니다. 또한 [Export]하려는 상속된 기본 클래스에서 메서드를 구현하는 경우 공용 메서드로 만들어야 합니다.

## <a name="mapping-of-the-objective-c-delegate-pattern-to-c"></a>Objective-C 대리자 패턴을 C로 매핑하기\#

다음과 같은 Objective-C 샘플이 표시되는 경우

```objc
foo.delegate = [[SomethingDelegate] alloc] init]
```

이는 "SomethingDelegate" 클래스의 인스턴스를 만들어 구축하고 foo 변수의 대리자 속성에 값을 할당하도록 언어에 지시합니다. 이 메커니즘은 Xamarin.iOS와 C#에서 지원되며 구문은 다음과 같습니다.

```csharp
foo.Delegate = new SomethingDelegate ();
```

Xamarin.iOS에서는 Objective-C 대리자 클래스에 매핑되는 강력한 형식의 클래스가 제공됩니다. 이를 사용하려면 Xamarin.iOS의 구현에 정의된 메서드를 서브클래싱하고 재정의해야 합니다. 작동 방식에 대한 자세한 내용은 아래의 "모델" 섹션을 참조하세요.

### <a name="mapping-delegates-to-c"></a>대리자를 C로 매핑\#

UIKit는 일반적으로 Objective-C 대리자를 두 가지 양식으로 사용합니다.

첫 번째 양식은 구성 요소 모델에 대한 인터페이스를 제공합니다. 보기에 대한 요청 시 데이터를 제공하는 메커니즘(예: 목록 보기에 대한 데이터 저장소 기능)을 예로 들 수 있습니다.  이러한 경우 항상 적절한 클래스의 인스턴스를 만들고 변수를 할당해야 합니다.

다음 예제에서는 문자열을 사용하는 모델에 대한 구현과 함께 `UIPickerView`를 제공합니다.

```csharp
public class SampleTitleModel : UIPickerViewTitleModel {

    public override string TitleForRow (UIPickerView picker, nint row, nint component)
    {
        return String.Format ("At {0} {1}", row, component);
    }
}

[...]

pickerView.Model = new MyPickerModel ();
```

두 번째 양식은 이벤트에 대한 알림을 제공하는 것입니다. 이러한 경우 위에 설명된 형식으로 API가 계속 노출되지만, 빠른 작업에 사용할 수 있고 C#에서 익명 대리자 및 람다 식과 통합하는 데 더 간단한 C# 이벤트도 제공됩니다.

예를 들어 `UIAccelerometer` 이벤트를 구독할 수 있습니다.

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

두 옵션은 적절한 경우에 사용할 수 있지만 프로그래머는 그 중 하나를 선택해야 합니다. 강력한 형식의 응답기/대리자의 고유한 인스턴스를 만들고 할당하면 C# 이벤트가 작동하지 않습니다. C# 이벤트를 사용하는 경우 응답기/대표자 클래스의 메서드는 결코 호출되지 않습니다.

`UIWebView`를 사용한 이전 예제는 다음과 같은 C# 3.0 람다를 사용하여 작성할 수 있습니다.

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```

#### <a name="responding-to-events"></a>이벤트에 응답

Objective-C 코드에서, 여러 컨트롤에 대한 이벤트 처리기와 여러 컨트롤에 대한 정보 공급자가 동일한 클래스에 호스트되는 경우가 있습니다. 클래스가 메시지에 응답하기 때문에 이러한 경우가 발생하며, 클래스가 메시지에 응답하는 동안 개체를 함께 연결하는 것이 가능합니다.

앞서 설명한 것처럼 Xamarin.iOS는 C# 이벤트 기반 프로그래밍 모델과 Objective-C 대리자 패턴을 모두 지원합니다. 따라서 대리자를 구현하고 원하는 메서드를 재정의하는 새 클래스를 만들 수 있습니다.

서로 다른 여러 작업에 대한 응답기가 모두 동일한 클래스 인스턴스에 호스트되는 Objective-C의 패턴을 지원할 수도 있습니다. 그러나 이렇게 하려면 Xamarin.iOS 바인딩의 하위 수준 기능을 사용해야 합니다.

예를 들어 클래스가 `UITextFieldDelegate.textFieldShouldClear`: 메시지와 `UIWebViewDelegate.webViewDidStartLoad`: 클래스의 동일한 인스턴스에 모두 응답하도록 하려면 [Export] 특성 선언을 사용해야 합니다.

```csharp
public class MyCallbacks : NSObject {
    [Export ("textFieldShouldClear:"]
    public bool should_we_clear (UITextField tf)
    {
        return true;
    }

    [Export ("webViewDidStartLoad:")]
    public void OnWebViewStart (UIWebView view)
    {
        Console.WriteLine ("Loading started");
    }
}
```

메서드에 대한 C# 이름은 중요하지 않습니다. 중요한 것은 [Export] 특성에 전달되는 문자열입니다.

이 스타일의 프로그래밍을 사용하는 경우 C# 매개 변수가 런타임 엔진에서 전달할 실제 형식과 일치하는지 확인합니다.

#### <a name="models"></a>모델

UIKit 저장소 시설이나, 도우미 클래스를 사용하여 구현되는 응답기의 경우 일반적으로 Objective-C 코드에서 대리자로 참조되며 프로토콜로 구현됩니다.

Objective-C 프로토콜은 인터페이스와 유사하지만 선택적 메서드를 지원합니다. 즉, 프로토콜의 작동을 위해 모든 메서드를 구현할 필요는 없습니다.

모델을 구현하는 방법에는 두 가지가 있습니다. 수동으로 구현하거나 강력한 형식의 기존 정의를 사용할 수 있습니다.

Xamarin.iOS에서 바인딩되지 않은 클래스의 구현을 시도하려면 수동 메커니즘이 필요합니다. 매우 쉽게 수행할 수 있습니다.

- 런타임에 등록할 클래스에 플래그 지정
- 재정의할 각 메서드에서 실제 선택기 이름으로 [Export] 특성을 적용합니다.
- 클래스를 인스턴스화하고 전달합니다.

예를 들어 다음은 UIApplicationDelegate 프로토콜 정의의 선택적 메서드 중 하나만 구현합니다.

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Objective-C 선택기 이름("applicationDidFinishLaunching:")은 내보내기 특성으로 선언되고 클래스는 `[Register]` 특성으로 등록됩니다.

Xamarin.iOS는 수동 바인딩이 필요하지 않은 강력한 형식의 선언(사용 준비 완료)을 제공합니다. 이 프로그래밍 모델을 지원하기 위해 Xamarin.iOS 런타임은 클래스 선언에서 [Model] 특성을 지원합니다. 이렇게 하여 메서드가 명시적으로 구현되지 않은 경우 클래스의 모든 메서드를 연결하지 않아야 함을 런타임에 알립니다.

즉, UIKit에서 선택적 메서드를 포함하는 프로토콜을 나타내는 클래스는 다음과 같이 작성됩니다.

```csharp
[Model]
public class SomeViewModel : NSObject {
    [Export ("someMethod:")]
    public virtual int SomeMethod (TheView view) {
       throw new ModelNotImplementedException ();
    }
    ...
}
```

일부 메서드만 구현하는 모델을 구현하려면 관심이 있는 메서드를 재정의하고 다른 메서드는 무시하면 됩니다. 런타임은 원본 메서드를 Objective-C 환경으로 후크하는 것이 아니라 덮어쓴 메서드만 후크합니다.

이전 수동 샘플에 해당하는 기능은 다음과 같습니다.

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

장점은 선택기, 인수의 형식 또는 C#에 대한 매핑을 찾기 위해 Objective-C 헤더 파일을 자세히 살펴볼 필요가 없으며, 강력한 형식과 함께 Mac용 Visual Studio에서 intellisense를 가져올 필요가 없다는 것입니다.

#### <a name="xib-outlets-and-c"></a>XIB 출선 및 C\#

> [!IMPORTANT]
> 이 섹션에서는 XIB 파일을 사용할 때의 출선과의 IDE 통합에 대해 설명합니다. Xamarin Designer for iOS를 사용하는 경우, 아래와 같이 IDE의 속성 섹션의 **ID > 이름**에 이름을 입력하여 모두 바꿉니다.
>
> [![](images/designeroutlet.png "Entering an item Name in the iOS Designer")](images/designeroutlet.png#lightbox)
>
>iOS 디자이너에 대한 자세한 내용은 [iOS 디자이너 소개](~/ios/user-interface/designer/introduction.md#how-it-works) 문서를 확인하세요.

이는 출선이 C#과 통합되는 방법에 대한 하위 수준의 설명이며 Xamarin.iOS의 고급 사용자를 위해 제공됩니다. Mac용 Visual Studio를 사용하는 경우 자동으로 플라이트에서 생성된 코드를 사용하여 내부적으로 매핑이 자동으로 수행됩니다.

Interface Builder를 사용하여 사용자 인터페이스를 디자인하는 경우 응용 프로그램의 모양만을 디자인하고 몇 가지 기본 연결을 설정하게 됩니다. 프로그래밍 방식으로 정보를 가져오거나 런타임에서 컨트롤의 동작을 변경하거나 런타임에서 컨트롤을 수정하려는 경우 일부 컨트롤을 관리 코드에 바인딩해야 합니다.

이 바인딩은 두 단계로 수행됩니다.

1. **파일 소유자**에 **출선 선언**을 추가합니다.
1. 컨트롤을 **파일 소유자**에 연결합니다.
1. UI와 연결을 XIB/NIB 파일에 저장합니다.
1. 런타임에서 NIB 파일을 로드합니다.
1. 출선 변수에 액세스합니다.

단계 (1) ~ (3)은 Interface Builder를 사용하여 인터페이스를 빌드하는 방법에 대한 Apple 설명서에서 다룹니다.

Xamarin.iOS를 사용하는 경우 응용 프로그램이 UIViewController에서 파생되는 클래스를 만들어야 합니다. 이 과정은 다음과 같이 구현됩니다.

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

그런 다음 NIB 파일에서 ViewController를 로드하려면 다음을 수행합니다.

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

그러면 NIB에서 사용자 인터페이스가 로드됩니다. 이제 출선에 액세스하려면 런타임에 액세스함을 알려야 합니다. 이렇게 하려면 `UIViewController` 서브클래스가 속성을 선언하고 [Connect] 특성으로 주석을 달아야 합니다. 다음과 같습니다.

```csharp
[Connect]
UITextField UserName {
    get {
        return (UITextField) GetNativeField ("UserName");
    }
    set {
        SetNativeField ("UserName", value);
    }
}
```

속성 구현은 실제 네이티브 형식에 대한 값을 실제로 가져오고 저장하는 것입니다.

Mac용 Visual Studio과 InterfaceBuilder를 사용하는 경우 이에 대해 걱정할 필요가 없습니다. Mac용 Visual Studio는 프로젝트의 일부로 컴파일되는 partial 클래스의 코드를 사용하여 선언된 모든 출선을 자동으로 미러링합니다.

#### <a name="selectors"></a>선택기

Objective-C 프로그래밍의 핵심 개념은 선택기입니다. 많은 API는 사용자가 선택기를 전달하거나 사용자의 코드가 선택기에 응답하도록 요구합니다.

C#에서 새 선택기를 만드는 것은 매우 쉽습니다. `ObjCRuntime.Selector` 클래스의 새 인스턴스를 만들고 이를 필요로 하는 API의 모든 장소에서 결과를 사용하기만 하면 됩니다. 예를 들어:

```csharp
var selector_add = new Selector ("add:plus:");
```

C# 메서드가 선택기 호출에 응답하는 경우 `NSObject` 형식에서 상속해야 하며, `[Export]` 특성을 사용하여 C# 메서드를 선택기 이름으로 데코레이팅해야 합니다. 예를 들어:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

선택기 이름은 모든 중간 및 후행 콜론(":", 있을 경우)을 포함하여 **반드시** 또 정확히 일치해야 합니다.

#### <a name="nsobject-constructors"></a>NSObject 생성자

`NSObject`에서 파생되는 Xamarin.iOS의 대부분의 클래스는 개체의 기능과 관련된 생성자를 노출하지만 즉시 알기 어려운 다양한 생성자도 노출합니다.

생성자는 다음과 같이 사용됩니다.

```csharp
public Foo (IntPtr handle)
```

이 생성자는 런타임이 사용자의 클래스를 비관리형 클래스로 매핑해야 하는 경우 사용자의 클래스를 인스턴스화하는 데 사용됩니다. 이는 XIB/NIB 파일을 로드할 때 발생합니다.  이 지점에서 Objective-C 런타임이 관리되지 않는 환경에 개체를 만들고 이 생성자는 관리되는 쪽을 초기화하도록 호출됩니다.

일반적으로 핸들 매개 변수를 사용하여 기본 생성자를 호출하고 필요한 모든 초기화를 본문에서 수행하기만 하면 됩니다.

```csharp
public Foo ()
```

이는 클래스에 대한 기본 생성자이며, Xamarin.iOS에서 제공하는 클래스에서 Foundation.NSObject 클래스와 그 사이의 모든 클래스를 초기화하고, 최종적으로 클래스의 Objective-C `init` 메서드에 연결합니다.

```csharp
public Foo (NSObjectFlag x)
```

이 생성자는 인스턴스를 초기화하는 데 사용되지만 코드가 끝에서 Objective-C "init" 메서드를 호출하는 것을 방지합니다. 초기화를 위해 이미 등록했거나(생성자에서 `[Export]`를 사용하는 경우) 다른 평균을 통해 초기화를 이미 수행한 경우 일반적으로 이를 사용합니다.

```csharp
public Foo (NSCoder coder)
```

이 생성자는 NSCoding 인스턴스에서 개체가 초기화되는 경우에 제공됩니다.

#### <a name="exceptions"></a>예외

Xamarin.iOS API 디자인은 Objective-C 예외를 C#로 발생시키지 않습니다. 디자인은 가비지를 첫 번째 위치의 Objective-C 환경으로 전송하지 않도록 하고, 생성되어야 하는 모든 예외는 잘못된 데이터가 Objective-C 환경에 전달되기 전에 바인딩 자체에서 생성되도록 합니다.

#### <a name="notifications"></a>알림

iOS 및 OS X 모두에서 개발자는 기본 플랫폼에서 브로드캐스팅하는 알림을 구독할 수 있습니다. 이는 `NSNotificationCenter.DefaultCenter.AddObserver` 메서드를 사용하여 실행됩니다. `AddObserver` 메서드는 두 개의 매개 변수를 사용 합니다. 하나는 구독하려는 알림이며 다른 하나는 알림이 발생할 때 호출되는 메서드입니다.

Xamarin.iOS 및 Xamarin.Mac에서 다양한 알림에 대한 키가 알림을 트리거하는 클래스에서 호스팅됩니다. 예를 들어 `UIMenuController`에서 발생시키는 알림은 "Notification"이라는 이름으로 끝나는 `UIMenuController` 클래스의 `static NSString` 속성으로 호스팅됩니다.

### <a name="memory-management"></a>메모리 관리

Xamarin.iOS에는 더 이상 사용되지 않는 리소스를 해제하는 데 필요한 가비지 수집기가 있습니다. 가비지 수집기 외에도 `NSObject`에서 파생되는 모든 개체는 `System.IDisposable` 인터페이스를 구현합니다.

#### <a name="nsobject-and-idisposable"></a>NSObject 및 IDisposable

`IDisposable` 인터페이스를 공개하는 것은 개발자가 큰 메모리 블록(예를 들어 `UIImage`는 위험성이 없는 포인터로 보일 수 있지만 2MB 이미지를 가리킬 수 있음)과 기타 중요하고 한정된 리소스(예: 비디오 디코딩 버퍼)의 큰 블록을 캡슐화할 수 있는 개체를 해제할 수 있는 편리한 방법입니다.

NSObject는 IDisposable 인터페이스와 [.NET Dispose 패턴](https://msdn.microsoft.com/library/fs2xkftw.aspx)을 구현합니다. 이렇게 하면 NSObject를 서브클래싱하는 개발자가 Dispose 동작을 재정의하고 요청 시 고유한 리소스를 해제할 수 있습니다. 예를 들어 여러 이미지를 유지하는 이 뷰 컨트롤러를 살펴보겠습니다.

```csharp
class MenuViewController : UIViewController {
    UIImage breakfast, lunch, dinner;
    [...]
    public override void Dispose (bool disposing)
    {
        if (disposing){
             if (breakfast != null) breakfast.Dispose (); breakfast = null;
             if (lunch != null) lunch.Dispose (); lunch = null;
             if (dinner != null) dinner.Dispose (); dinner = null;
        }
        base.Dispose (disposing)
    }
}
```

관리되는 개체를 삭제하면 컨트롤러는 더 이상 유용하지 않습니다. 여전히 개체에 대한 참조가 있지만 이 시점에서 개체는 사실상 유효하지 않습니다. 일부 .NET API는 삭제된 개체의 메서드에 액세스를 시도할 때 ObjectDisposedException을 Throw하여 이를 확인합니다. 예를 들면 다음과 같습니다.

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

"image" 변수에 계속 액세스할 수 있는 경우에도 실제로 잘못된 참조이며 이미지를 보유하는 Objective-C 개체를 더 이상 가리키지 않습니다.

하지만 C#에서 개체를 삭제하더라도 개체가 반드시 제거되는 것은 아닙니다. 개체에 대한 C#의 참조를 해제하기만 하면 됩니다. Cocoa 환경에서 자체 사용에 대한 참조를 유지했을 수 있습니다. 예를 들어 UIImageView의 이미지 속성을 이미지로 설정한 다음 이미지를 삭제하는 경우, 기본 UIImageView는 자체 참조를 사용했고 이 개체 사용을 마치기 전에는 이 개체에 대한 참조를 유지합니다.

#### <a name="when-to-call-dispose"></a>Dispose 호출 시기

개체를 제거하는 데 Mono가 필요한 경우 Dispose를 호출해야 합니다. NSObject가 메모리나 정보 풀과 같은 중요한 리소스에 대한 참조를 실제로 보유하고 있지만 Mono에서는 알 수 없는 경우를 예로 들 수 있습니다. 이러한 경우 Mono가 가비지 수집 주기를 수행하기를 기다리지 말고, Dispose를 호출하여 메모리에 대한 참조를 즉시 해제해야 합니다.

내부적으로는, Mono가 [C# 문자열에서 NSString 참조](~/ios/internals/api-design/nsstring.md)를 만드는 경우, 가비지 수집기에서 수행해야 할 작업량을 줄이기 위해 Mono가 해당 참조를 즉시 삭제합니다. 처리할 개체가 적을수록 GC가 실행되는 속도가 빨라집니다.

#### <a name="when-to-keep-references-to-objects"></a>개체에 대한 참조의 확인 시점

자동 메모리 관리에서 발생하는 한 가지 부작용은 사용되지 않는 개체에 대한 참조가 없는 경우 GC가 해당 개체를 제거한다는 것입니다. 예를 들어 최상위 뷰 컨트롤러나 최상위 창을 보유하는 지역 변수를 만든 다음 뒤쪽으로 사라지게 할 경우, 예상치 못한 부작용이 발생할 수 있습니다.

정적 또는 인스턴스 변수에 대한 참조를 개체에 유지하지 않는 경우 Mono는 해당 변수에 대한 Dispose() 메서드를 호출하고 개체에 대한 참조를 해제합니다. 이는 미해결 참조일 뿐이므로 Objective-C 런타임이 자동으로 개체를 소멸시킵니다.

## <a name="related-links"></a>관련 링크

- [바인딩 필드](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
