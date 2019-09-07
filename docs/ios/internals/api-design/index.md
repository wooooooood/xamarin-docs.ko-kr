---
title: Xamarin.ios API 디자인
description: Xamarin.ios Api를 설계 하는 데 사용 된 원칙과이를 목적과 관련 된 방법을 안내 합니다.
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 843aeda14ad8c47014b577bdce8004872b12865d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70753461"
---
# <a name="xamarinios-api-design"></a>Xamarin.ios API 디자인

Mono의 일부인 핵심 기본 클래스 라이브러리 외에도 [xamarin.ios](http://www.xamarin.com/iOS) 는 개발자가 mono를 사용 하 여 네이티브 iOS 응용 프로그램을 만들 수 있도록 다양 한 ios api에 대 한 바인딩과 함께 제공 됩니다.

Xamarin.ios의 핵심에는 CoreGraphics 및 [OPENGL ES](#opengles)와 같은 iOS c 기반 api에 C# 대 한 바인딩 뿐만 아니라 전 세계와 세계를 연결 하는 interop 엔진이 있습니다.

Monotouch.dialog 코드와 통신 하는 하위 수준 런타임은 [Cruntime](#objcruntime)에 있습니다. 이 위에는 [Foundation](#foundation), CoreFoundation 및 [uikit](#uikit) 에 대 한 바인딩이 제공 됩니다.

## <a name="design-principles"></a>디자인 원칙

다음은 Xamarin.ios 바인딩에 대 한 몇 가지 디자인 원칙입니다 (이는 Xamarin.ios에도 적용 되며 macOS에서 목표-C에 대 한 Mono 바인딩입니다).

- [프레임 워크 디자인 지침](https://docs.microsoft.com/dotnet/standard/design-guidelines) 따르기
- 개발자가 목표-C 클래스를 서브클래싱하는 것을 허용 합니다.

  - 기존 클래스에서 파생
  - 연결 하려면 기본 생성자를 호출 합니다.
  - 재정의 메서드는 재정의 시스템에서 C#수행 해야 합니다.
  - 서브클래싱은 표준 구문을 C# 사용 해야 합니다.

- 개발자에 게 목표-C 선택기를 노출 하지 않음
- 임의의 목표-C 라이브러리를 호출 하는 메커니즘을 제공 합니다.
- 일반적인 목표-C 작업을 쉽고 빠르게 수행할 수 있습니다.
- 목표-C 속성을 속성 C# 으로 노출
- 강력한 형식의 API를 노출 합니다.

  - 형식 안전성 향상
  - 런타임 오류 최소화
  - 반환 형식에 대 한 IDE IntelliSense 가져오기
  - IDE 팝업 설명서를 허용 합니다.

- Api의 IDE 내 탐색을 권장 합니다.

  - 예를 들어 다음과 같이 약하게 형식화 된 배열을 노출 하는 대신

    ```objc
    NSArray *getViews
    ```

    다음과 같이 강력한 형식을 노출 합니다.

    ```csharp
    NSView [] Views { get; set; }
    ```

    Mac용 Visual Studio 이렇게 하면 API를 검색 하는 동안 자동 완성 기능을 수행 하 고, 반환 된 `System.Array` 값에서 모든 작업을 사용할 수 있게 하며, 반환 값이 LINQ에 참여할 수 있도록 합니다.

- 네이티브 C# 형식:

  - [`NSString`않게`string`](~/ios/internals/api-design/nsstring.md)
  - `int` C# C# 사용 하 여`[Flags]` 열거형 및 열거형으로 사용 해야 하는 매개변수를설정합니다.`uint`
  - 형식 중립적인 `NSArray` 개체 대신, 배열을 강력한 형식의 배열로 노출 합니다.
  - 이벤트 및 알림에 대해 사용자에 게 다음 중에서 선택할 수 있습니다.

    - 기본적으로 강력한 형식의 버전
    - 고급 사용 사례에 대 한 약한 형식의 버전

- 목표-C 대리자 패턴 지원:

  - C#이벤트 시스템
  - 대리자 C# (람다, 무명 메서드 및 `System.Delegate`)를 목표-C api에 블록으로 노출 합니다.

### <a name="assemblies"></a>어셈블리

Xamarin.ios에는 *Xamarin.ios 프로필*을 구성 하는 많은 어셈블리가 포함 되어 있습니다. [어셈블리](~/cross-platform/internals/available-assemblies.md) 페이지에 자세한 정보가 있습니다.

### <a name="major-namespaces"></a>주요 네임 스페이스

#### <a name="objcruntime"></a>ObjCRuntime

[Objcruntime](xref:ObjCRuntime) 네임 스페이스를 사용 하 여 개발자는와 C# 목표 사이를 연결할 수 있습니다.
이는 Cocoa # 및 Gtk #의 경험을 기반으로 iOS 용으로 특별히 설계 된 새로운 바인딩입니다.

#### <a name="foundation"></a>Mfc

[Foundation](xref:Foundation) 네임 스페이스는 iOS의 일부인 목표-c Foundation 프레임 워크와 상호 운용 하도록 디자인 된 기본 데이터 형식을 제공 하며,이는 목표-c의 개체 지향 프로그래밍에 대 한 기본입니다.

Xamarin.ios는 목표-C C# 의 클래스 계층 구조에서 미러링합니다. 예를 들어, 목표-C 기본 클래스 [Nsobject](https://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) 는 C# [Foundation.](xref:Foundation.NSObject)n e s 개체를 통해 사용할 수 있습니다.

이 네임 스페이스는 기본 목표-C Foundation 형식에 대 한 바인딩을 제공 하지만 일부 경우에는 기본 형식을 .NET 형식에 매핑 했습니다. 예를 들어:

- [Nsstring](https://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) 및 [nsstring](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html)를 처리 하는 대신 런타임은 API 전체에 C# [문자열](xref:System.String)s와 강력한 형식의 [배열을](xref:System.Array)제공 합니다.

- 개발자가 현재 Xamarin.ios에 의해 바인딩되지 않은 타사 목표-C Api, 기타 iOS Api 또는 Api를 바인딩할 수 있도록 하는 다양 한 도우미 Api가 여기에 제공 됩니다.

바인딩 Api에 대 한 자세한 내용은 [Xamarin.ios 바인딩 생성기](~/cross-platform/macios/binding/binding-types-reference.md) 섹션을 참조 하세요.

##### <a name="nsobject"></a>NSObject

[Nsobject](xref:Foundation.NSObject) 유형은 모든 목표-C 바인딩의 기초가 됩니다. Xamarin.ios 형식은 iOS CocoaTouch Api에서 두 가지 형식의 클래스를 미러링합니다. C 형식 (일반적으로 CoreFoundation 형식 이라고 함)과 목표-C 형식 (모두 NSObject 클래스에서 파생 됨).

관리 되지 않는 형식을 미러링 하는 각 형식에 대해 [핸들](xref:Foundation.NSObject.Handle) 속성을 통해 네이티브 개체를 가져올 수 있습니다.

Mono는 모든 개체에 대 한 가비지 수집을 제공 하지만는 `Foundation.NSObject` [system.object](xref:System.IDisposable) 인터페이스를 구현 합니다. 즉, 가비지 수집기가 시작 될 때까지 기다리지 않고도 지정 된 NSObject의 리소스를 명시적으로 해제할 수 있습니다. 큰 NSObjects를 사용 하는 경우 중요 합니다 (예: 큰 데이터 블록에 대 한 포인터를 포함할 수 있는 UIImages).

형식이 결정적 종료를 수행 해야 하는 경우에는 [Nsobject를 재정의 합니다. dispose (bool) 메서드](xref:Foundation.NSObject.Dispose(System.Boolean)) 는 dispose로 설정 합니다. dispose는 "bool disposing"이 고, true로 설정 된 경우 사용자가 명시적으로 호출 되어 dispose 메서드가 호출 되 고 있음을 의미 합니다. 개체에 대 한 Dispose () 값이 false 이면 종료자 스레드의 종료자에서 Dispose (bool disposing) 메서드가 호출 되 고 있음을 의미 합니다.

##### <a name="categories"></a>범주

Xamarin.ios 8.10부터에서 C#목표-C 범주를 만들 수 있습니다.

특성을 사용 하 여 `Category` 특성에 대 한 인수로 확장할 형식을 지정 하 여이 작업을 수행 합니다. 다음 예에서는 NSString을 확장 합니다.

```csharp
[Category (typeof (NSString))]
```

각 범주 메서드는 특성을 `Export` 사용 하 여 메서드를 목표 C로 내보내기 위한 일반적인 메커니즘을 사용 합니다.

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

모든 관리 되는 확장 메서드는 정적 이어야 하지만의 C#확장 메서드에 표준 구문을 사용 하 여 객관적인 C 인스턴스 메서드를 만들 수 있습니다.

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

확장 메서드의 첫 번째 인수는 메서드가 호출 된 인스턴스입니다.

전체 예제:

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

이 예제에서는 toUpper 인스턴스 클래스에 기본 인스턴스 메서드를 추가 합니다 .이 메서드는 목표-C에서 호출 될 수 있습니다.

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

이를 사용 하는 한 가지 시나리오는 코드 베이스의 전체 클래스 집합에 메서드를 추가 하는 것입니다. 예를 들어 `UIViewController` , 모든 인스턴스는 회전할 수 있는 것으로 보고 합니다.

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

PreserveAttribute는 응용 프로그램의 크기를 줄이기 위해 응용 프로그램을 처리 하는 단계에서 mtouch (형식 멤버 또는 형식 멤버를 유지 하기 위해)를 알리는 데 사용 되는 사용자 지정 특성입니다.

애플리케이션이 정적으로 연결하지 않은 모든 멤버는 제거됩니다. 따라서이 특성은 정적으로 참조 되지 않지만 응용 프로그램에 필요한 멤버를 표시 하는 데 사용 됩니다.

예를 들어 형식을 동적으로 인스턴스화하는 경우 형식의 기본 생성자를 유지하는 것이 좋습니다. XML serialization을 사용하는 경우 형식의 속성을 유지하는 것이 좋습니다.

같은 형식의 모든 멤버 또는 형식 자체에 이 특성을 적용할 수 있습니다. 전체 형식을 유지 하려는 경우 형식에 대해 [Preserve (AllMembers = true)] 구문을 사용할 수 있습니다.

#### <a name="uikit"></a>UIKit

[Uikit](xref:UIKit) 네임 스페이스는 C# 클래스 형식으로 CocoaTouch을 구성 하는 모든 UI 구성 요소에 대 한 일대일 매핑을 포함 합니다. API는 C# 언어에서 사용 되는 규칙을 따르도록 수정 되었습니다.

C#대리자는 일반적인 작업에 대해 제공 됩니다. 자세한 내용은 [대리자](#delegates) 섹션을 참조 하세요.

#### <a name="opengles"></a>OpenGLES

OpenGLES의 경우, CoreGraphics 데이터 형식 및 구조를 사용 하도록 수정 된 OpenGL에 [OpenTK](http://www.opentk.com/) API의 [수정 된 버전](xref:OpenTK) 을 배포 하 고 iOS에서 제공 되는 기능만 제공 합니다.

OpenGLES 1.1 기능은 [ES11.GL 형식을](xref:OpenTK.Graphics.ES11.GL)통해 사용할 수 있습니다.

OpenGLES 2.0 기능은 [ES20.GL 형식을](xref:OpenTK.Graphics.ES20.GL)통해 사용할 수 있습니다.

OpenGLES 3.0 기능은 [ES30.GL 형식을](xref:OpenTK.Graphics.ES30.GL)통해 사용할 수 있습니다.

### <a name="binding-design"></a>바인딩 디자인

Xamarin.ios는 단순히 기본 목표-C 플랫폼에 대 한 바인딩이 아닙니다. .NET 유형 시스템을 확장 하 고 시스템을 디스패치 하 여 C# 더 나은 Blend 및 목표를 달성 합니다.

P/Invoke는 Windows 및 Linux에서 네이티브 라이브러리를 호출 하는 데 유용한 도구 이거나, Windows의 COM interop에 대 한 IJW 지원을 사용할 수 있기 때문에 Xamarin.ios는 목표-C 개체에 C# 대 한 바인딩 개체를 지원 하기 위해 런타임을 확장 합니다.

Xamarin.ios 응용 프로그램을 만드는 사용자에 게는 다음 몇 가지 섹션에서 설명 하는 것이 필요 하지 않지만 개발자가 작업을 수행 하는 방법을 이해 하 고 더 복잡 한 응용 프로그램을 만들 때이를 지원할 수 있습니다.

#### <a name="types"></a>유형

잘 이해 하는 경우 C# 에 C# 는 형식이 낮은 수준의 Foundation 형식 대신 노출 됩니다.  즉, [API는 NSString 대신 C# "string" 형식을 사용](~/ios/internals/api-design/nsstring.md) 하 고 nsstring를 노출 하는 C# 대신 강력한 형식의 배열을 사용 합니다.

일반적으로 xamarin.ios 및 xamarin.ios 디자인에서는 기본 `NSArray` 개체가 노출 되지 않습니다. 대신 런타임은를 일부 `NSArray` `NSObject` 클래스의 강력한 형식의 배열로 자동 변환 합니다. 따라서 Xamarin.ios는 NSArray를 반환 하기 위해 GetViews와 같은 약한 형식의 메서드를 노출 하지 않습니다.

```csharp
NSArray GetViews ();
```

대신, 바인딩은 다음과 같이 강력한 형식의 반환 값을 노출 합니다.

```csharp
UIView [] GetViews ();
```

에 `NSArray` 는`NSArray` 를 직접 사용 해야 하는 경우를 제외 하 고 API 바인딩에서 사용 하지 않는 것이 좋습니다.

또한 `CGRect` `CGPoint` `RectangleF` CoreGraphicsAPI`System.Drawing` 를 노출 `CGSize` 하는 대신 Classic API에서 구현 으로대체했습니다.`PointF` `SizeF`개발자가 OpenTK를 사용 하는 기존 OpenGL 코드를 유지 하는 데 도움이 될 수 있습니다. 새 64 비트 **Unified API**를 사용 하는 경우 CoreGraphics API를 사용 해야 합니다.

#### <a name="inheritance"></a>상속

Xamarin.ios API 디자인을 통해 개발자는 파생 클래스에서 "override" 키워드를 사용 하 고 "base" C# C# 를 사용 하 여 기본 구현에 연결 하는 것과 같은 방식으로 네이티브 목표-C 형식을 확장할 수 있습니다. 키워드로.

이 설계를 통해 개발자는 전체 목표-C 시스템이 이미 Xamarin.ios 라이브러리 내에 래핑되어 있으므로 개발 프로세스의 일부로 목표-C 선택기를 처리 하지 않아도 됩니다.

#### <a name="types-and-interface-builder"></a>형식 및 Interface Builder

Interface Builder에서 만든 형식의 인스턴스인 .net 클래스를 만들 때 단일 `IntPtr` 매개 변수를 사용 하는 생성자를 제공 해야 합니다.
이는 관리 되는 개체 인스턴스를 관리 되지 않는 개체에 바인딩하는 데 필요 합니다.
코드는 다음과 같이 한 줄로 구성 됩니다.

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

#### <a name="delegates"></a>대리자

목표-C는 C# 각 언어의 word 대리자에 대해 서로 다른 의미를 갖습니다.

CocoaTouch에 대 한 온라인에서 볼 수 있는 설명서에서 대리자는 일반적으로 메서드 집합에 응답 하는 클래스의 인스턴스입니다. 이는 C# 인터페이스와 매우 비슷하며 메서드가 항상 필수는 아니지만 차이가 있습니다.

이러한 대리자는 UIKit 및 기타 CocoaTouch Api에서 중요 한 역할을 합니다. 이러한 작업은 다양 한 작업을 수행 하는 데 사용 됩니다.

- 코드에 알림을 제공 하려면 (또는 Gtk +의 C# 이벤트 배달과 유사)
- 데이터 시각화 컨트롤에 대 한 모델을 구현 합니다.
- 컨트롤의 동작을 구동 하려면입니다.

프로그래밍 패턴은 컨트롤의 동작을 변경 하기 위한 파생 클래스 만들기를 최소화 하도록 설계 되었습니다. 이 솔루션은 몇 년 동안 다른 GUI 도구 키트가 수행 하는 것에 대 한 스피릿와 비슷합니다. Gtk의 신호, Qt 슬롯, Winforms 이벤트, WPF/Silverlight 이벤트 등에 있습니다. 수백 개의 인터페이스 (각 작업에 대해 하나씩)를 사용 하지 않거나 개발자가 필요 하지 않은 메서드를 너무 많이 구현 하도록 요구 하기 위해, 목표 C는 선택적 메서드 정의를 지원 합니다. 이는 모든 메서드 C# 를 구현 해야 하는 인터페이스와는 다릅니다.

목적-C 클래스에서이 프로그래밍 패턴을 사용 하는 클래스가 일반적으로 호출 `delegate`되는 속성을 노출 하는 것을 확인할 수 있습니다 .이 속성은 인터페이스의 필수 부분과 0 개 이상의 선택적 부분을 구현 하는 데 필요 합니다.

Xamarin.ios에서는 이러한 대리자에 바인딩하는 세 가지 상호 배타적인 메커니즘이 제공 됩니다.

1. [Via 이벤트](#via-events).
2. [속성을 `Delegate` 통해 강력한 형식](#strongly-typed-via-a-delegate-property)
3. [속성을 통해 느슨하게 `WeakDelegate` 형식화 된](#loosely-typed-via-the-weakdelegate-property)

예를 들어 [Uiwebview 보기](https://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html) 클래스를 살펴보세요. 이는 [대리자](https://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate) 속성에 할당 된 [uiwebviewdelegate](https://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html) 인스턴스로 디스패치합니다.

##### <a name="via-events"></a>Via 이벤트

많은 형식의 경우 xamarin.ios는 `UIWebViewDelegate` C# 이벤트에 대 한 호출을 전달 하는 적절 한 대리자를 자동으로 만듭니다. `UIWebView`의 경우:

- [WebViewDidStartLoad](https://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:) 메서드는 [uiwebview 보기. loadstarted](xref:UIKit.UIWebView.LoadStarted) 이벤트에 매핑됩니다.
- [WebViewDidFinishLoad](https://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:) 메서드는 [uiwebview 보기. loadfinished](xref:UIKit.UIWebView.LoadFinished) 이벤트에 매핑됩니다.
- [웹 보기: didFailLoadWithError](https://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:) 메서드는 [Uiwebview 보기. loaderror](xref:UIKit.UIWebView.LoadError) 이벤트에 매핑됩니다.

예를 들어이 간단한 프로그램은 웹 보기를 로드 하는 경우 시작 및 종료 시간을 기록 합니다.

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```

##### <a name="via-properties"></a>Via 속성

이벤트는 둘 이상의 구독자가 이벤트에 있을 수 있는 경우에 유용 합니다. 또한 이벤트는 코드에서 반환 값이 없는 경우에만 제한 됩니다.

코드에서 값을 반환 해야 하는 경우에는 속성을 대신 옵트인 합니다. 즉, 개체의 지정 된 시간에 메서드를 하나만 설정할 수 있습니다.

예를 들어이 메커니즘을 사용 하 여에 대 한 `UITextField`처리기의 화면에서 키보드를 해제할 수 있습니다.

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

이 `UITextField`경우 `ShouldReturn` 의 속성은 부울 값을 반환 하는 대리자를 인수로 사용 하 고, TextField가 눌린 반환 단추를 사용 하 여 작업을 수행 해야 하는지 여부를 결정 합니다. 이 메서드에서는 호출자에 대해 *true* 를 반환 하지만 화면에서 키보드를 제거 합니다 .이는 textfield를 호출할 `ResignFirstResponder`때 발생 합니다.

##### <a name="strongly-typed-via-a-delegate-property"></a>대리자 속성을 통해 강력한 형식

이벤트를 사용 하지 않으려는 경우 고유한 [Uiwebviewdelegate](xref:UIKit.UIWebViewDelegate) 서브 클래스를 제공 하 고이를 [ui웹 후크. delegate](xref:UIKit.UIWebView.Delegate) 속성에 할당할 수 있습니다. UIWebView 보기. 대리자가 할당 되 면 UIWebView 보기 이벤트 디스패치 메커니즘은 더 이상 작동 하지 않으며, 해당 이벤트가 발생할 때 UIWebViewDelegate 메서드가 호출 됩니다.

예를 들어이 단순 유형은 웹 보기를 로드 하는 데 걸리는 시간을 기록 합니다.

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

위의는 코드에서 다음과 같이 사용 됩니다.

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

위의 내용은 UIWebViewer를 만들고 메시지에 응답 하기 위해 만든 클래스인 알림 인스턴스로 메시지를 보내도록 지시 합니다.

이 패턴은 특정 컨트롤에 대 한 동작을 제어 하는 데도 사용 됩니다. 예를 들어 uiwebview 보기의 경우에는 uiwebview 보기 `UIWebView` 의 경우에는 [uiwebview 보기. shouldstartload](xref:UIKit.UIWebView.ShouldStartLoad) 속성을 사용 하 여에서 `UIWebView` 페이지를 로드할지 여부를 제어 합니다.

패턴은 몇 가지 컨트롤에 대 한 요청 시 데이터를 제공 하는 데도 사용 됩니다. 예를 들어 [Uitableview](xref:UIKit.UITableView) 컨트롤은 강력한 테이블 렌더링 컨트롤이 고, 모양과 콘텐츠는 모두 [Uitableviewdatasource](xref:UIKit.UITableViewDataSource) 의 인스턴스에 의해 결정 됩니다.

### <a name="loosely-typed-via-the-weakdelegate-property"></a>WeakDelegate 속성을 통해 느슨하게 형식화 되었습니다.

강력한 형식의 속성 외에도 개발자가 원하는 경우 다른 항목을 바인딩할 수 있도록 하는 약한 형식의 대리자도 있습니다.
강력한 `Delegate` 형식의 속성이 xamarin.ios의 바인딩에서 노출 되는 모든 위치에서 해당 `WeakDelegate` 속성도 노출 됩니다.

을 사용 하 `WeakDelegate`는 경우 [내보내기](xref:Foundation.ExportAttribute) 특성을 사용 하 여 클래스를 적절 하 게 데코레이팅 하 여 선택기를 지정 해야 합니다. 예를 들어:

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

`WeakDelegate` 속성이 할당`Delegate` 된 후에는 속성이 사용 되지 않습니다. 또한 [내보내기] 하려는 상속 된 기본 클래스에서 메서드를 구현 하는 경우 공용 메서드로 만들어야 합니다.

## <a name="mapping-of-the-objective-c-delegate-pattern-to-c"></a>C에 대 한 목표-C 대리자 패턴 매핑\#

다음과 같은 목표-C 샘플이 표시 되는 경우

```objc
foo.delegate = [[SomethingDelegate] alloc] init]
```

이는 "SomethingDelegate" 클래스의 인스턴스를 만들고 생성 하 고 foo 변수의 delegate 속성에 값을 할당 하는 언어를 지시 합니다. 이 메커니즘은 Xamarin.ios C# 에서 지원 되며 구문은 다음과 같습니다.

```csharp
foo.Delegate = new SomethingDelegate ();
```

Xamarin.ios에서 목표-C 대리자 클래스에 매핑되는 강력한 형식의 클래스를 제공 했습니다. 이를 사용 하려면 Xamarin.ios의 구현에 정의 된 메서드를 서브클래싱 하 고 재정의 합니다. 작동 방식에 대 한 자세한 내용은 아래의 "모델" 섹션을 참조 하세요.

### <a name="mapping-delegates-to-c"></a>C에 대리자 매핑\#

일반적으로 UIKit는 두 가지 형식으로 객관적인 C 대리자를 사용 합니다.

첫 번째 폼은 구성 요소 모델에 대 한 인터페이스를 제공 합니다. 예를 들어 보기에 대 한 데이터를 제공 하는 메커니즘 (예: 목록 보기에 대 한 데이터 저장소 기능)을 제공 합니다.  이러한 경우 항상 적절 한 클래스의 인스턴스를 만들고 변수를 할당 해야 합니다.

다음 예제에서는 문자열을 사용 하는 `UIPickerView` 모델에 대 한 구현을 제공 합니다.

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

두 번째 형태는 이벤트에 대 한 알림을 제공 하는 것입니다. 이러한 경우 위에 설명 된 형식으로 API를 계속 표시 하지만 빠른 작업에 사용 하 고에서 C# C#익명 대리자 및 람다 식과 통합 하는 것이 더 간단한 이벤트도 제공 합니다.

예를 들어 다음과 같이 이벤트를 `UIAccelerometer` 구독할 수 있습니다.

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

두 옵션은 적절 한 경우에 사용할 수 있지만 프로그래머는 하나를 선택 해야 합니다. 강력한 형식의 응답자/대리자의 고유한 인스턴스를 만들고 할당 하면 C# 이벤트가 작동 하지 않습니다. C# 이벤트를 사용 하는 경우 응답자/delegate 클래스의 메서드는 호출 되지 않습니다.

사용 `UIWebView` 된 이전 예제는 다음과 같이 3.0 람다 C# 식을 사용 하 여 작성할 수 있습니다.

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```

#### <a name="responding-to-events"></a>이벤트에 응답

목적-C 코드에서 여러 컨트롤에 대 한 이벤트 처리기 및 여러 컨트롤에 대 한 이벤트 처리기가 동일한 클래스에 호스트 되는 경우가 있습니다. 클래스가 메시지에 응답 하 고 클래스가 메시지에 응답 하는 동안 개체를 함께 연결 하는 것이 가능 합니다.

앞서 설명한 것 처럼 Xamarin.ios는 C# 이벤트 기반 프로그래밍 모델 및 목표-C 대리자 패턴을 모두 지원 합니다. 여기서는 대리자를 구현 하 고 원하는 메서드를 재정의 하는 새 클래스를 만들 수 있습니다.

서로 다른 여러 작업에 대 한 응답기는 모두 동일한 클래스 인스턴스에 호스트 되는 객관적인 C의 패턴을 지원할 수도 있습니다. 그러나 이렇게 하려면 Xamarin.ios 바인딩의 하위 수준 기능을 사용 해야 합니다.

예를 들어 클래스가 클래스의 동일한 인스턴스에서 `UITextFieldDelegate.textFieldShouldClear`: 메시지 `UIWebViewDelegate.webViewDidStartLoad`와:에 모두 응답 하도록 하려면 [Export] 특성 선언을 사용 해야 합니다.

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

메서드 C# 이름은 중요 하지 않습니다. [Export] 특성에 전달 되는 문자열은 모두 중요 합니다.

이 스타일의 프로그래밍을 사용 하는 경우 C# 매개 변수가 런타임 엔진에서 전달할 실제 형식과 일치 하는지 확인 합니다.

#### <a name="models"></a>모델

UIKit 저장소 시설이 나 도우미 클래스를 사용 하 여 구현 되는 응답자의 경우 일반적으로이를 목표-C 코드에서 대리자로 참조 하며,이를 프로토콜로 구현 합니다.

목적-C 프로토콜은 인터페이스와 유사 하지만 선택적 메서드를 지원 합니다. 즉, 프로토콜이 작동 하기 위해 일부 메서드를 구현 해야 하는 것은 아닙니다.

모델을 구현 하는 방법에는 두 가지가 있습니다. 수동으로 구현 하거나 강력한 형식의 기존 정의를 사용할 수 있습니다.

Xamarin에서 바인딩되지 않은 클래스를 구현 하려고 하면 수동 메커니즘이 필요 합니다. 매우 쉽게 수행할 수 있습니다.

- 런타임에 등록할 클래스에 플래그 지정
- 재정의할 각 메서드에서 실제 선택기 이름으로 [Export] 특성을 적용 합니다.
- 클래스를 인스턴스화하고 전달 합니다.

예를 들어 다음은 UIApplicationDelegate 프로토콜 정의의 선택적 메서드 중 하나만 구현 합니다.

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

목표-C 선택기 이름 ("applicationDidFinishLaunching:")은 내보내기 특성으로 선언 되 고 클래스는 `[Register]` 특성을 사용 하 여 등록 됩니다.

Xamarin.ios는 수동 바인딩이 필요 하지 않은 강력한 형식의 선언 (사용할 준비가 됨)을 제공 합니다. 이 프로그래밍 모델을 지원 하기 위해 Xamarin.ios 런타임은 클래스 선언에서 [Model] 특성을 지원 합니다. 이렇게 하면 메서드가 명시적으로 구현 되지 않은 경우 클래스의 모든 메서드를 연결 하지 않아야 함을 런타임에 알립니다.

즉, UIKit에서 선택적 메서드를 포함 하는 프로토콜을 나타내는 클래스는 다음과 같이 작성 됩니다.

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

일부 메서드만 구현 하는 모델을 구현 하려면 관심 있는 메서드를 재정의 하 고 다른 메서드는 무시 하기만 하면 됩니다. 런타임은 원래 메서드를 목표로 하는 것이 아니라 덮어쓴 메서드만 후크 합니다.

이전 수동 샘플에 해당 하는 기능은 다음과 같습니다.

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

장점은 선택기, 인수의 형식 또는에 대 C#한 매핑을 찾을 목적-C 헤더 파일을 자세히 살펴보고 강력한 형식과 함께 Mac용 Visual Studio에서 intellisense를 가져올 필요가 없다는 것입니다.

#### <a name="xib-outlets-and-c"></a>XIB 출 선 및 C\#

> [!IMPORTANT]
> 이 섹션에서는 XIB 파일을 사용할 때와의 IDE 통합에 대해 설명 합니다. Xamarin Designer for iOS 사용 하는 경우 아래와 같이 IDE의 속성 섹션에서 **id > 이름** 아래에 이름을 입력 하 여 바꿉니다.
>
> [![](images/designeroutlet.png "IOS 디자이너에서 항목 이름 입력")](images/designeroutlet.png#lightbox)
>
>IOS 디자이너에 대 한 자세한 내용은 [Ios 디자이너 소개](~/ios/user-interface/designer/introduction.md#how-it-works) 문서를 참조 하세요.

이는에서 콘센트가와 C# 통합 되는 방법에 대 한 하위 수준 설명 이며 xamarin.ios의 고급 사용자를 위해 제공 됩니다. Mac용 Visual Studio 사용 하는 경우 사용자를 위해 비행에서 생성 된 코드를 사용 하 여 내부적으로 매핑이 자동으로 수행 됩니다.

Interface Builder를 사용 하 여 사용자 인터페이스를 디자인 하는 경우 응용 프로그램의 모양을 디자인 하 고 몇 가지 기본 연결을 설정 합니다. 런타임에 프로그래밍 방식으로 정보를 페치 하거나 런타임에 컨트롤의 동작을 변경 하거나 런타임에 컨트롤을 수정 하려는 경우 일부 컨트롤을 관리 코드에 바인딩해야 합니다.

이렇게 하려면 몇 가지 단계를 수행 합니다.

1. **파일의 소유자**에 게 **콘센트 선언을** 추가 합니다.
1. **파일의 소유자**에 게 컨트롤을 연결 합니다.
1. UI와 연결을 XIB/NIB 파일에 저장 합니다.
1. 런타임에 NIB 파일을 로드 합니다.
1. 콘센트 변수에 액세스 합니다.

단계 (1) ~ (3)는 Interface Builder를 사용 하 여 인터페이스를 빌드하기 위한 Apple 설명서에서 다룹니다.

Xamarin.ios를 사용 하는 경우 응용 프로그램에서 UIViewController에서 파생 되는 클래스를 만들어야 합니다. 다음과 같이 구현 됩니다.

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

그런 다음 NIB 파일에서 ViewController를 로드 하려면 다음을 수행 합니다.

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

그러면 NIB에서 사용자 인터페이스가 로드 됩니다. 이제 콘센트에 액세스 하려면 런타임에 액세스 하려는 런타임에 알려야 합니다. 이렇게 하려면 하위 클래스에서 `UIViewController` 속성을 선언 하 고 [Connect] 특성으로 주석을 추가 해야 합니다. 다음과 같습니다.

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

속성 구현은 실제 원시 형식에 대 한 값을 실제로 인출 하 고 저장 하는 것입니다.

Mac용 Visual Studio 및 인터페이스 작성기를 사용 하는 경우이에 대해 걱정할 필요가 없습니다. Mac용 Visual Studio는 프로젝트의 일부로 컴파일되는 partial 클래스의 코드를 사용 하 여 선언 된 모든 콘센트를 자동으로 미러링합니다.

#### <a name="selectors"></a>선택기

목표-C 프로그래밍의 핵심 개념은 선택기입니다. 선택기를 전달 해야 하는 Api를 통해 또는 코드에서 선택기에 응답할 것으로 예상 하는 경우가 많습니다.

에서 C# 새 선택기를 만드는 것은 매우 쉽습니다. `ObjCRuntime.Selector` 클래스의 새 인스턴스를 만들고이를 필요로 하는 API의 모든 위치로 결과를 사용 하기만 하면 됩니다. 예를 들어:

```csharp
var selector_add = new Selector ("add:plus:");
```

C# 메서드는 선택기 호출에 응답 하 고, `NSObject` 형식에서 상속 해야 하며, 특성을 C# `[Export]` 사용 하 여 해당 메서드를 선택기 이름으로 데코레이팅 해야 합니다. 예를 들어:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

선택기 이름은 모든 중간 및 후행 콜론 (":")을 포함 하 여 정확 하 게 일치 **해야** 합니다 (있는 경우).

#### <a name="nsobject-constructors"></a>NSObject 생성자

에서 `NSObject` 파생 되는 xamarin.ios의 대부분의 클래스는 개체의 기능에 특정 한 생성자를 노출 하지만 즉시 명확 하지 않은 다양 한 생성자도 노출 합니다.

생성자는 다음과 같이 사용 됩니다.

```csharp
public Foo (IntPtr handle)
```

이 생성자는 런타임에서 클래스를 관리 되지 않는 클래스에 매핑해야 할 때 클래스를 인스턴스화하는 데 사용 됩니다. 이는 XIB/NIB 파일을 로드할 때 발생 합니다.  이 시점에서 목표-C 런타임은 관리 되지 않는 세계에서 개체를 만들고이 생성자를 호출 하 여 관리 되는 쪽을 초기화 합니다.

일반적으로 핸들 매개 변수를 사용 하 여 기본 생성자를 호출 하 고 본문에서 필요한 모든 초기화를 수행 하기만 하면 됩니다.

```csharp
public Foo ()
```

이것은 클래스에 대 한 기본 생성자 이며 xamarin.ios에서 제공 하는 클래스에서 Foundation. nsobject 클래스와 사이에 있는 모든 클래스를 초기화 하 고 끝에서이를 클래스의 목표-C `init` 메서드에 연결 합니다.

```csharp
public Foo (NSObjectFlag x)
```

이 생성자는 인스턴스를 초기화 하는 데 사용 되지만 코드가 끝에서 목표-C "init" 메서드를 호출 하는 것을 방지 합니다. 초기화를 위해 이미 등록 했거나 (생성자에서를 사용 `[Export]` 하는 경우) 다른 평균을 통해 초기화를 이미 수행한 경우 일반적으로이를 사용 합니다.

```csharp
public Foo (NSCoder coder)
```

이 생성자는 NSCoding 인스턴스에서 개체가 초기화 되는 경우에 제공 됩니다. 자세한 내용은 Apple의 [보관 및 Serialization 프로그래밍 가이드](https://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i) 를 참조 하세요.

#### <a name="exceptions"></a>예외

Xamarin.ios API 디자인은 목표로 C 예외를 C# 발생 시 키 지 않습니다. 디자인은 첫 번째 위치의 가비지를 목표로 전송 하지 않도록 하 고, 생성 되어야 하는 모든 예외는 잘못 된 데이터가 목표-C 환경에 전달 되기 전에 바인딩 자체에서 생성 되도록 합니다.

#### <a name="notifications"></a>알림

IOS 및 OS X 모두에서 개발자는 기본 플랫폼에서 브로드캐스팅하는 알림을 구독할 수 있습니다. 이 작업은 메서드를 `NSNotificationCenter.DefaultCenter.AddObserver` 사용 하 여 수행 됩니다. 메서드 `AddObserver` 는 두 개의 매개 변수를 사용 합니다. 하나는 구독 하려는 알림이 고, 다른 하나는 알림이 발생할 때 호출 되는 메서드입니다.

Xamarin.ios 및 Xamarin.ios에서 다양 한 알림에 대 한 키는 알림을 트리거하는 클래스에서 호스팅됩니다. 예를 들어에 `UIMenuController` 의해 발생 하는 알림은 이름 "Notification"로 끝나는 `UIMenuController` 클래스의 속성으로 `static NSString` 호스팅됩니다.

### <a name="memory-management"></a>메모리 관리

Xamarin.ios에는 더 이상 사용 되지 않는 리소스를 해제 하는 데 필요한 가비지 수집기가 있습니다. 가비지 수집기 외에도에서 `NSObject` 파생 되는 모든 개체는 `System.IDisposable` 인터페이스를 구현 합니다.

#### <a name="nsobject-and-idisposable"></a>NSObject 및 IDisposable

인터페이스 노출는 개발자가 많은 양의 메모리 블록을 캡슐화 할 수 있는 개체를 해제할 수 있는 편리한 방법입니다. 예를 `UIImage` 들어,은 매우 다양 한 포인터 처럼 보이지만 2mb 이미지를 가리킬 수 있습니다. `IDisposable` ) 및 기타 중요 및 유한 리소스 (예: 비디오 디코딩 버퍼).

NSObject는 IDisposable 인터페이스와 [.Net Dispose 패턴](https://msdn.microsoft.com/library/fs2xkftw.aspx)을 구현 합니다. 이렇게 하면 NSObject를 서브클래싱하는 개발자가 Dispose 동작을 재정의 하 고 주문형 리소스를 해제할 수 있습니다. 예를 들어 다양 한 이미지를 유지 하는이 뷰 컨트롤러를 살펴보겠습니다.

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

관리 되는 개체를 삭제 하면 더 이상 유용 하지 않습니다. 여전히 개체에 대 한 참조가 있지만이 시점에서 개체는 모든 의도 및 목적에 대 한 것입니다. 일부 .NET Api는 삭제 된 개체의 메서드에 액세스 하려고 할 때 ObjectDisposedException을 throw 하 여이를 확인 합니다. 예를 들면 다음과 같습니다.

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

"Image" 변수에 계속 액세스할 수 있는 경우에도 잘못 된 참조 이며 이미지를 보유 하는 목표-C 개체를 더 이상 가리키지 않습니다.

하지만에서 C# 개체를 삭제 하는 것은 개체가 반드시 제거 되는 것을 의미 하지는 않습니다. 개체에 대 한 참조 C# 를 해제 하는 것이 전부입니다. Cocoa 환경에서 자체 사용에 대 한 참조를 유지 했을 수 있습니다. 예를 들어 UIImageView의 이미지 속성을 이미지로 설정한 다음 이미지를 삭제 하는 경우 기본 UIImageView는 자체 참조를 사용 했 고이 개체를 사용 하기 전에는이 개체에 대 한 참조를 유지 합니다.

#### <a name="when-to-call-dispose"></a>Dispose를 호출 하는 경우

개체를 제거 하는 데 Mono가 필요한 경우 Dispose를 호출 해야 합니다. 사용 사례는 Mono에서 NSObject 실제로 메모리 나 정보 풀과 같은 중요 한 리소스에 대 한 참조를 보유 하 고 있음을 알 수 없는 경우입니다. 이러한 경우 Mono가 가비지 수집 주기를 수행 하기를 기다리는 대신 Dispose를 호출 하 여 메모리에 대 한 참조를 즉시 해제 해야 합니다.

내부적으로 Mono는 [문자열에서 C# nsstring 참조](~/ios/internals/api-design/nsstring.md)를 만들 때 가비지 수집기에서 수행 해야 하는 작업 양을 줄이기 위해 즉시 삭제 합니다. 처리할 개체가 적을수록 GC가 실행 되는 속도가 빨라집니다.

#### <a name="when-to-keep-references-to-objects"></a>개체에 대 한 참조를 유지 하는 경우

자동 메모리 관리에서 발생 하는 한 가지 부작용은 GC가 사용 되지 않는 개체에 대 한 참조가 없는 경우 해당 개체를 제거 하는 것입니다. 예를 들어 최상위 뷰 컨트롤러나 최상위 창을 보관 하는 지역 변수를 만든 다음 뒤로 이동 하는 경우와 같이이 경우에는 놀라운 부작용이 발생할 수 있습니다.

정적 또는 인스턴스 변수에 대 한 참조를 개체에 유지 하지 않는 경우 Mono는 해당 변수에 대 한 Dispose () 메서드를 호출 하 고 개체에 대 한 참조를 해제 합니다. 이는 미해결 참조일 뿐 이므로 목표-C 런타임은 개체를 소멸 시킵니다.

## <a name="related-links"></a>관련 링크

- [바인딩 필드](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
