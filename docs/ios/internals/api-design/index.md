---
title: API 디자인
description: Xamarin.iOS API 디자인에 대 한 큐브 뷰
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: b7604633a5dfad6134d7b549299194ab6707a865
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="api-design"></a>API 디자인

핵심 모노의 일부인 기본 클래스 라이브러리 뿐만 아니라 [Xamarin.iOS](http://www.xamarin.com/iOS) 함께 다양 한 iOS 개발자가 모노도 기본 iOS 응용 프로그램 만들기를 허용 하도록 Api에 대 한 바인딩을 제공 합니다.

Xamarin.iOS 핵심인는 C# 세계 Objective-c 세계 뿐 아니라 iOS CoreGraphics C 기반 Api에 대 한 바인딩을 연결 하는 interop 엔진 및 [OpenGL ES](#OpenGLES)합니다.

Objective C 코드와 통신 하는 낮은 수준의 런타임이 [MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime)합니다. 에 대 한이 바인딩이이 기반으로 [Foundation](#MonoTouch.Foundation), CoreFoundation, 및 [UIKit](#MonoTouch.UIKit) 제공 됩니다.

## <a name="design-principles"></a>디자인 원칙

(문제에 적용할 Xamarin.Mac, macOS Objective C 용의 모노 바인딩) Xamarin.iOS 바인딩에 대 한 우리의 디자인 원칙 중 일부입니다.

- 수행 된 [Framework 디자인 지침](https://docs.microsoft.com/dotnet/standard/design-guidelines)
- Objective C 서브 클래스에는 개발자 허용:

  - 기존 클래스에서 파생
  - 에 연결 하 여 기본 생성자를 호출 합니다.
  - C#의 재정의 시스템으로 이루어져야 합니다. 메서드를 재정의 합니다.
  - C# 표준 구문을 함께 사용할 수 있어야 서브클래싱

- 개발자가 Objective-c 선택기를 노출 하지 마십시오.
- 임의의 Objective C 라이브러리를 호출할 수 있는 메커니즘 제공
- 쉽고 하드 Objective-c 작업 가능한 일반 Objective-c 작업 확인
- C# 속성으로 Objective-c 속성을 노출 합니다.
- 강력한 형식의 API를 노출 합니다.

  - 형식 안전성을 높일
  - 런타임 오류를 최소화
  - 반환 형식은 IDE IntelliSense 가져오기
  - IDE 팝업 설명서에 대 한 허용

- Api의 IDE에서 탐색 것을 권장 합니다.

  - 약한 형식의 배열은 다음과 같이 노출 하는 대신에 예를 들어:
    
    ```objc
    NSArray *getViews
    ```
    다음과 같이 강력한 형식을 노출 합니다.
    
    ```csharp
    NSView [] Views { get; set; }
    ```
    
    이 API를 검색 하는 동안 자동 완성 기능을 수행할 수 있는 기능 Mac 용 Visual Studio를 제공, 모든는 `System.Array` 반환된 된 값에서 사용 가능한 작업 반환 값을 LINQ에 참여할 수 있습니다.

- 네이티브 C# 형식:

  - [`NSString` 됩니다. `string`](~/ios/internals/api-design/nsstring.md)
  - 설정 `int` 및 `uint` 되 었어야 열거형에 열거형 C# 및 C# 열거형에는 매개 변수 `[Flags]` 특성
  - 형식 중립적 대신 `NSArray` 개체를 강력한 형식의 배열로 배열을 노출 합니다.
  - 이벤트 및 알림을 위한 사용자 사이 선택을 제공 합니다.

    - 기본적으로 강력한 형식의 버전
    - 고급 사용 사례에 대 한 약한 형식의 버전

- Objective C 대리자 패턴을 지원 합니다.

    - C# 이벤트 시스템
    - C# 대리자 노출 (무명 메서드, 람다 및 `System.Delegate`) 블록으로 Objective-c api

### <a name="assemblies"></a>어셈블리

Xamarin.iOS 다양 한 구성 하는 어셈블리를 포함 된 *Xamarin.iOS 프로필*합니다. [어셈블리](~/cross-platform/internals/available-assemblies.md) 페이지에 자세한 정보.

### <a name="major-namespaces"></a>주 네임 스페이스 

<a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

[ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/) 네임 스페이스에 없습니다. 목표와 C# 사이의 세계 브리지 개발자가 사용할 수
이 새 바인딩 Cocoa # 및 Gtk # 경험을 기반으로 하는 iOS 용으로 특별히 설계 되었습니다.

<a name="MonoTouch.Foundation" />

#### <a name="foundation"></a>Foundation

[Foundation](https://developer.xamarin.com/api/namespace/Foundation/) 네임 스페이스 iOS의 일부인 Objective C 기반 프레임 워크와 상호 운용 하도록 설계 된 기본 데이터 형식 및 개체 지향 Objective c에서 프로그래밍에 대 한 기반 인지를 제공 합니다.

Xamarin.iOS의 목표 없습니다. 클래스 계층 구조를 C#에서 미러링 Objective C 기본 클래스 예를 들어 [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) 통해 C#에서 사용 가능 [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/)합니다.

이 네임 스페이스의 내부 Objective-c Foundation 형식에 대 한 바인딩을 제공 하지만 일부의 경우에 म 매핑한 기본 형식은.NET 형식입니다. 예를 들어:

- 처리 하는 대신 [NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) 및 [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html), C#으로 런타임이 노출 [문자열](https://developer.xamarin.com/api/type/System.String/)s 강력한 형식의 [배열](https://developer.xamarin.com/api/type/System.Array/)전체에서 s API입니다.

- 다양 한 도우미 Api는 개발자가 제 3 자 Objective-c Api를 다른 iOS Api 또는 현재 Xamarin.iOS로 바인딩되지 않은 Api를 바인딩할 수 있도록 여기 노출 됩니다.

바인딩 Api에 대 한 자세한 내용은 참조는 [Xamarin.iOS 바인딩 생성기](~/cross-platform/macios/binding/binding-types-reference.md) 섹션.


##### <a name="nsobject"></a>NSObject

[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) 유형 모든 Objective-c 바인딩 위한 기본 토대입니다. Xamarin.iOS 형식 미러링할 iOS CocoaTouch Api에서에서 형식의 두 클래스: C 형식 (일반적으로 하는 이러한 CoreFoundation 형식) 및 (이러한 NSObject 클래스에서 파생 되는 모든) Objective C 형식입니다.

통해 네이티브 개체를 가져올 수는 관리 되지 않는 형식을 반영 하는 각 형식에 대해는 [처리](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) 속성입니다.

모노는 모든 사용자 개체에 대 한 가비지 컬렉션을 제공 하는 동안는 `Foundation.NSObject` 구현 하는 [System.IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/) 인터페이스입니다. 즉, 시작 기능에 가비지 수집기를 기다릴 필요 없이 모든 주어진된 NSObject의 리소스를 해제 명시적으로 수 있습니다. 이 많은 NSObjects, 데이터의 큰 블록에 대 한 포인터를 보유 하 UIImages 등을 사용 하는 경우에 중요 합니다.

형식이 명확한 종료를 수행 하는 경우 재정의 [NSObject.Dispose(bool) 메서드](https://developer.xamarin.com/api/type/Foundation.NSObject/%2fM%2fDispose) dispose 매개 변수는 "bool disposing", 하 고 하는 경우 설정할 것을 true로 의미 하기 때문에 프로그램 Dispose 메서드가 호출 되는 사용자 개체에 명시적으로 호출된 Dispose ()입니다. 값 false 이면 종료자 스레드에서 Dispose (bool disposing) 메서드가 종료자에서 호출 되는 것을 의미 합니다. []()


##### <a name="categories"></a>범주

Xamarin.iOS 8.10 것부터 C#에서 Objective-c 범주를 만들 수 있습니다.

이 작업은 수행를 사용 하 여는 `Category` 특성을 특성에 대 한 인수로 확장할 형식을 지정 합니다. 예를 들어 다음 예제에서는 NSString 확장 하 합니다.

    [Category (typeof (NSString))]

각 범주 메서드는 사용 하 여 Objective-c 메서드 내보내기에 대 한 일반 메커니즘을 사용 하 여 `Export` 특성:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

모든 관리 되는 확장 메서드는 정적 이어야 합니다. 그러나 C#의 확장 메서드에 대 한 표준 구문을 사용 하 여 Objective-c 인스턴스 메서드를 만들 수 있습니다.

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

및 확장 메서드의 첫 번째 인수에는 메서드가 호출 된 인스턴스가 됩니다.

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

이 예제에서는 NSString 클래스에 없습니다. 목표에서에서 호출할 수 있는 네이티브 toUpper 인스턴스 메서드를 추가 합니다.

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAudoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

코드 베이스에서 클래스의 전체 집합에이 방법이 유용한 시나리오 중 하나는 메서드를 추가 하는 것, 예를 들어, 이렇게 하면 모든 `UIViewController` 인스턴스에서 회전할 수를 보고 합니다.

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

PreserveAttribute는 크기를 줄여 응용 프로그램 처리 시 단계 중에 형식 또는 형식 멤버를 유지 하기 위해 – Xamarin.iOS 배포 도구 – mtouch 알리는 데 사용 되는 사용자 지정 특성입니다.

응용 프로그램이 정적으로 연결하지 않은 모든 멤버는 제거됩니다. 따라서이 특성은 정적으로 참조 하지 않지만 아직 응용 프로그램에 필요한 멤버를 표시 하는 데 사용 됩니다.

예를 들어 형식을 동적으로 인스턴스화하는 경우 형식의 기본 생성자를 유지하는 것이 좋습니다. XML serialization을 사용하는 경우 형식의 속성을 유지하는 것이 좋습니다.

같은 형식의 모든 멤버 또는 형식 자체에 이 특성을 적용할 수 있습니다. 전체 형식을 유지 하려는 경우 구문을 사용할 수 있습니다 [보존 (AllMembers = true)] 형식에 있습니다.

<a name="MonoTouch.UIKit" />

#### <a name="uikit"></a>UIKit

[UIKit](https://developer.xamarin.com/api/namespace/UIKit/) 네임 스페이스의 모든 C# 클래스의 형태로 CocoaTouch 구성 하는 UI 구성 요소에 대 한 일대일 매핑을 포함 합니다. API는 C# 언어에서 사용 되는 규칙을 따르도록 수정 되었습니다.

C# 대리자는 일반적인 작업에 제공 됩니다. 참조는 [대리자](#Delegates) 한 자세 합니다.

<a name="OpenGLES" />

#### <a name="opengles"></a>OpenGLES

배포에서는 OpenGLES는 [수정 버전](https://developer.xamarin.com/api/namespace/OpenTK/) 의 [OpenTK](http://www.opentk.com/) API를 노출 뿐만 아니라 CoreGraphics 데이터 형식 및 구조를 사용 하도록 수정 된 OpenGL에 대 한 개체 지향 바인딩을 iOS에서 사용할 수 있는 기능입니다.

OpenGLES 1.1 기능을 문서화 된 ES11.GL 형식을 통해 사용할 수 [여기](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/) 유형입니다.

OpenGLES 2.0 기능을 문서화 된 ES20.GL 형식을 통해 사용할 수 [여기](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/) 유형입니다.

OpenGLES 3.0 기능을 문서화 된 ES30.GL 형식을 통해 사용할 수 [여기](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/) 유형입니다.


### <a name="binding-design"></a>바인딩 디자인

Xamarin.iOS 기본 Objective-c 플랫폼에 대 한 바인딩을 단순히 않습니다. .NET 형식 시스템 및 더 나은 blend C# 및 없습니다. 목표에 디스패치 시스템 확장

것 처럼 P/Invoke는 Windows 및 Linux에서 네이티브 라이브러리를 호출 하는 유용한 정보를 제공 하거나 IJW로 지원을 사용할 수 있는 windows COM interop에 대 한, Xamarin.iOS 하면 런타임에서 바인딩 C# 개체 Objective-c 개체를 확장 합니다.

몇 개 섹션 사용자 Xamarin.iOS 응용 프로그램을 만드는 개발자에 게 필요 하지 않습니다. 다음에 설명 된 것이 완료 하 고 더 복잡 한 응용 프로그램을 만들 때 도움이 됩니다 하는 방법을 이해 합니다.



#### <a name="types"></a>유형

C# 형식 관점에서 여러분이 C# universe 하위 수준 기반 형식 대신 노출 됩니다.  즉 [API는 C# "string" 형식을 사용 하 여 NSString 대신](~/ios/internals/api-design/nsstring.md) 강력한 형식의 C# 배열 NSArray 노출 하는 대신 사용 하 여 합니다.

일반적으로 기본 Xamarin.iOS 및 Xamarin.Mac 디자인에서에서 `NSArray` 개체 노출 되지 않습니다. 런타임에 자동으로 변환 대신 `NSArray`일부의 강력한 형식의 배열에는 s `NSObject` 클래스입니다. 따라서 Xamarin.iOS는 NSArray 반환할 GetViews 같은 약한 형식의 메서드를 노출 하지 않습니다.

```csharp
NSArray GetViews ();
```

대신, 바인딩에 다음과 같이 강력한 형식의 반환 값을 노출합니다.

```csharp
UIView [] GetViews ();
```

노출 하는 방법 중 몇 가지 `NSArray`, 사용 하려는 코너 케이스에 대 한는 `NSArray` 를 직접 있지만 API 바인딩에 사용 좋습니다.

또한에 **클래식 API** 노출 하는 대신 `CGRect`, `CGPoint` 및 `CGSize` CoreGraphics API에서 교체 순위가 `System.Drawing` 구현 `RectangleF`, `PointF`및 `SizeF` 개발자 OpenTK를 사용 하는 기존 OpenGL 코드 유지 도움이 됩니다. 새로운 64 비트를 사용 하는 경우 **통합 API**, CoreGraphics API를 사용 해야 합니다.

<a name="Inheritance" />

#### <a name="inheritance"></a>상속

Xamarin.iOS API 디자인에는 개발자가 네이티브 Objective C 형식을 C# 형식을, "base" C# 키워드를 사용 하 여 기본 구현에 체인 뿐만 아니라 "override" 키워드를 사용 하 여 파생된 클래스에서 확장은 같은 방식으로 확장할 수 있습니다.

이 디자인 Xamarin.iOS 라이브러리 안에 전체 Objective-c 시스템이 래핑된 이미 있으므로 Objective-c 선택기는 개발 과정의 일부로 처리를 피하기 위해 개발자가 있습니다.


#### <a name="types-and-interface-builder"></a>형식 및 인터페이스 작성기

단일을 사용 하는 생성자를 제공 해야 하는 인터페이스 작성기로 만든 형식의 인스턴스인.NET 클래스를 만들 때 `IntPtr` 매개 변수입니다.
관리 되지 않는 개체를 사용 하 여 관리 되는 개체 인스턴스를 바인딩할 필요 합니다.
코드 한 줄을 다음과 같이 이루어져 있습니다.

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

<a name="Delegates" />


#### <a name="delegates"></a>대리자

Objective C 및 C# 각 언어의 단어 대리자에 대 한 다른 의미를 가질 합니다.

Objective C 전 세계에서 하 고 있습니다. CocoaTouch에 대 한 온라인 설명서에서 대리자는 일반적으로 메서드 집합에 응답 하는 클래스의 인스턴스입니다. 메서드는 항상 필수 된 이것이 차이점은 C# 인터페이스, 매우 비슷합니다.

이러한 대리자 UIKit 및 기타 CocoaTouch Api에서 중요 한 역할을 수행 합니다. 다양 한 작업을 수행 하는 데 사용 됩니다.

-  코드 (C# 또는 Gtk + 이벤트 전달과 비슷하지만)에 대 한 알림을 제공 합니다.
-  데이터 시각화 컨트롤에 대 한 모델을 구현 하 합니다.
-  실행 하는 컨트롤의 동작입니다.


프로그래밍 패턴은 컨트롤에 대 한 동작을 변경 하는 파생된 클래스의 생성을 최소화 하도록 설계 되었습니다. 이 솔루션은 수 년에 걸쳐 수행 하는 다른 GUI 도구 키트는 개념상에서 유사: Gtk의 신호는 슬롯 하, Winforms 이벤트, WPF/Silverlight 이벤트 등입니다. Objective-c (각 동작 마다 하나씩) 인터페이스의 수백 또는 필요 하지 않은 너무 많은 메서드를 구현 하는 개발자를 방지 하려면 선택적 메서드 정의 지원 합니다. 이 필요한 모든 메서드를 구현 해야 하는 C# 인터페이스 보다 다릅니다.

Objective C 클래스 나타납니다이 프로그래밍 패턴을 사용 하는 클래스 라는 일반적으로 속성을 노출 하 `delegate`, 변수인 인터페이스의 필수 부분 및 0 개 이상, 선택적 부분을 구현 하는 데 필요 합니다.

Xamarin.iOS에서 세 가지 상호 배타적인 메커니즘에는 이러한 대리자를 바인딩할 제공 됩니다.

1.  [이벤트를 통해](#Via_Events)합니다.
2.  [통해 강력한 형식의 `Delegate` 속성](#StrongDelegate)
3.  [통해 느슨하게 형식화는 `WeakDelegate` 속성](#WeakDelegate)

예를 들어는 [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html) 클래스입니다. 이 디스패치합니다는 [UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html) 에 할당 되는 인스턴스는 [위임](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate) 속성입니다.

<a name="Via_Events" />

##### <a name="via-events"></a>이벤트를 통해

많은 형식의 Xamarin.iOS에서 자동으로 만듭니다는 전달 하는 적절 한 대리자는 `UIWebViewDelegate` C# 이벤트를 호출 합니다. `UIWebView`의 경우:

-  [webViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:) 메서드가 매핑되는 [UIWebView.LoadStarted](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadStarted/) 이벤트입니다.
-  [webViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:) 메서드가 매핑되는 [UIWebView.LoadFinished](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadFinished/) 이벤트입니다.
-  [webView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:) 메서드가 매핑되는 [UIWebView.LoadError](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadError/) 이벤트입니다.

예를 들어이 간단한 프로그램 웹 로드를 볼 때 시작 및 종료 시간을 기록 합니다.

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```


##### <a name="via-properties"></a>Via 속성

이벤트는 이벤트에 대 한 구독자 여러 개 있을 수 있습니다 때 유용 합니다. 또한 이벤트는 사례를 제한 하는 코드에서 반환 값이 없습니다.

코드 값을 반환 하려면 필요한 경우에서는 선택한 대신 속성에 대 한 합니다. 즉, 개체에 지정된 된 시간에 단 하나의 메서드를 설정할 수 있습니다.

에 대 한 처리기에서 화면에서 키보드를 해제 하려면이 메커니즘을 사용할 수는 예를 들어 한 `UITextField`:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

`UITextField`의 `ShouldReturn` 속성이 경우에 인수로 부울 값을 반환 하 고 있는지 여부를 텍스트 필드 해야를 바탕으로 조치 반환 단추를 누르는 동작을 결정 하는 대리자입니다. 메서드에서 반환 *true* 호출자에 게 또한 키보드 화면에서 제거 되지만 (텍스트 필드를 호출 하면 이런 경우가 `ResignFirstResponder`).

<a name="StrongDelegate"/>

##### <a name="strongly-typed-via-a-delegate-property"></a>대리자 속성을 통해 강력한 형식

이벤트를 사용 하지 않으려면 제공할 수 있습니다 직접 [UIWebViewDelegate](https://developer.xamarin.com/api/type/UIKit.UIWebViewDelegate/) 하위 클래스에 할당 하는 [UIWebView.Delegate](https://developer.xamarin.com/api/property/UIKit.UIWebView.Delegate/) 속성입니다. UIWebView.Delegate에 할당 되 면 UIWebView 이벤트 디스패치 메커니즘 작동 하지 않습니다 되 고 해당 이벤트가 발생할 때 UIWebViewDelegate 메서드 호출 됩니다.

예를 들어, 단순 유형 웹 보기를 로드 하는 데 걸리는 시간을 기록 합니다.

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

위의 다음과 같은 코드에서 사용 됩니다.

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

위의 UIWebViewer 만들고 알림, 메시지에 응답 하기 위해 만든 하는 클래스의 인스턴스로 메시지를 보내도록 안내 합니다.

이 패턴은 또한는 UIWebView에서는 예를 들어 특정 컨트롤에 대 한 동작을 제어 하는 데 사용 됩니다는 [UIWebView.ShouldStartLoad](https://developer.xamarin.com/api/property/UIKit.UIWebView.ShouldStartLoad/) 속성을 사용 하면는 `UIWebView` 인스턴스 제어를 여부는 `UIWebView` 로드 됩니다는 여부 페이지입니다.

몇 가지 컨트롤에 대 한 요청 시 데이터를 제공 하는 패턴도 사용 됩니다. 예를 들어는 [UITableView](https://developer.xamarin.com/api/type/UIKit.UITableView/) 컨트롤은 강력한 표 렌더링 컨트롤 – 이며 모양과 내용을 모두의 인스턴스에서 [UITableViewDataSource](https://developer.xamarin.com/api/type/UIKit.UITableView/DataSource)

<a name="WeakDelegate"/>

### <a name="loosely-typed-via-the-weakdelegate-property"></a>느슨한 WeakDelegate 속성을 통해 형식

강력한 형식의 속성 외에도 개발자가 필요한 경우 작업을 다르게 바인딩할 수 있습니다 하는 약한 형식의 대리자도 됩니다.
강력한 형식의 everywhere `Delegate` Xamarin.iOS의 바인딩에 해당 속성을 노출 `WeakDelegate` 도 속성을 노출 합니다.

사용 하는 경우는 `WeakDelegate`를 올바르게 사용 하 여 클래스를 데코레이팅하는 일을 담당는 [내보내기](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) 선택기를 지정 하는 특성입니다. 예를 들어:

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

두 번 참고는 `WeakDelegate` 속성에 할당 하지는 `Delegate` 속성 사용 되지 것입니다. 또한 [내보내기]를 상속된 된 기본 클래스의 메서드를 구현 하는 경우 확인 해야 하는 공용 메서드.


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>Objective C 대리자 패턴에서 C로의 매핑이&#35;

Objective C 예제는 다음과 같은 표시 될 때:

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

이렇게 하면 언어를 생성 및 "SomethingDelegate" 클래스의 인스턴스를 생성 하 고 foo 변수에 대리자 속성에 값을 할당 합니다. 이 메커니즘은 지원 Xamarin.iOS 여 되며 C# 구문:

```csharp
foo.Delegate = new SomethingDelegate ();
```

Xamarin.iOS Objective C에 매핑되는 강력한 형식의 클래스 대리자 클래스 제공 했습니다. 를 사용 하려면 서브클래싱 선택한 Xamarin.iOS의 구현에 의해 정의 된 메서드를 재정의 합니다. 작동 방식에 대 한 자세한 내용은 섹션 "모델" 아래를 참조 하세요.


##### <a name="mapping-delegates-to-c35"></a>C 대리자에 매핑&#35;

UIKit 일반적 두 가지 형태로 Objective-c 대리자를 사용합니다.

첫 번째 형태는 구성 요소 모델에 대 한 인터페이스를 제공합니다. 목록 보기에 대 한 데이터 저장소 시설 같은 보기에 대 한 요청 시 데이터를 제공 하는 메커니즘 예를 들어으로입니다.  이러한 경우에 항상 적절 한 클래스의 인스턴스를 만들고 변수를 할당 합니다.

다음 예제에서는 제공 된 `UIPickerView` 문자열을 사용 하는 모델에 대 한 구현으로:

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

두 번째 형태 이벤트에 대 한 알림을 제공 하는 것입니다. 이 경우 여전히 여 위에 약술 된 형태로 API를 노출 하지만 제공 빠른 작업에 사용 하기 쉽고 익명 대리자 및 C#의 람다 식을 사용 하 여 통합 해야 하는 C# 이벤트와 합니다.

예를 들어 구독할 수 있습니다 `UIAccelerometer` 이벤트:

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

두 옵션은 쉽게 이해, 하지만 프로그래머 둘 중 하나를 선택 해야 합니다. 강력한 형식의 응답자/대리자의 인스턴스를 직접를 만들고 할당 하는 경우 C# 이벤트 작동 되지 않습니다. C# 이벤트를 사용 하 여 응답자/대리자 클래스의 메서드는 호출 되지 않습니다.

사용 하는 이전 예 `UIWebView` 다음과 같이 C# 3.0 람다를 사용 하 여 작성할 수 있습니다.

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>이벤트에 응답

Objective C 코드에서 경우에 따라 여러 개의 컨트롤 및 여러 컨트롤에 대 한 정보는 공급자에 대 한 이벤트 처리기에서에서 호스트 됩니다 동일한 클래스입니다. 클래스는 메시지에 응답 하 고 클래스는 메시지에 응답으로 가능한 이것이, 개체를 연결할 수 있습니다.

이전에 설명 된 대로 Xamarin.iOS 모두은 C# 이벤트 기반 프로그래밍 모델을 지원 하 고 Objective-c 대리자 패턴을 하는 새 클래스를 만들 수 있는 대리자를 구현 하 고 원하는 메서드를 재정의 합니다.

것도 가능 Objective-c의 패턴을 지 원하는 여러 다른 작업에 대 한 응답자 모두 호스팅되는 클래스의 동일한 인스턴스에 있습니다. 이 작업을 수행 하지만 하위 수준 Xamarin.iOS 바인딩의 기능을 사용 해야 합니다.

예를 들어, 클래스 둘 다에 응답 하도록 하려는 경우는 `UITextFieldDelegate.textFieldShouldClear`: 메시지 및 `UIWebViewDelegate.webViewDidStartLoad`: 클래스의 동일한 인스턴스에 [내보내기] 특성 선언을 사용 해야 합니다.

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

메서드에 대 한 C# 이름이 중요 합니다; 되지 않습니다. 중요 한 모든 그룹이 [내보내기] 특성에 전달 된 문자열입니다.

이 스타일의 프로그래밍을 사용할 때 C# 매개 변수는 런타임 엔진에 전달 하는 실제 형식과 일치 하는지 확인 합니다.

<a name="Models" />

#### <a name="models"></a>모델

UIKit 저장소 시설 또는 응답자 도우미 클래스를 사용 하 여 구현 되는 대리자로, Objective C 코드에서 일반적으로 참조는이 고 프로토콜로 구현 됩니다.

Objective C 프로토콜 인터페이스와 같은 하지만 선택적 메서드를 지원-즉, 모든 메서드의 구현 해야 작동 하도록 프로토콜에 대 한 합니다.

모델을 구현 하는 방법은 두 가지가 있습니다. 직접 구현할 수도 있고 기존 강력한 형식의 정의 사용할 수도 있습니다.


수동 메커니즘이 Xamarin.iOS로 바인딩되지 않은 클래스를 구현 하려고 할 때 반드시 합니다. 매우 작업을 수행 하는 것이 쉽습니다.

-  런타임에 등록에 대 한 클래스 플래그
-  재정의 하려는 각 방법에 대해 실제 선택기 이름의 [내보내기] 특성을 적용 합니다.
-  클래스를 인스턴스화하고 전달 합니다.

예를 들어, 다음 중 하나만 구현 선택적 메서드 UIApplicationDelegate 프로토콜 정의에:

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Objective C 선택기 이름 ("applicationDidFinishLaunching:") 내보내기 특성으로 선언 된 클래스에 등록 됩니다는 `[Register]` 특성 합니다.

Xamarin.iOS 강력한 형식의 선언을 사용할 수 있는 상태가 수동 바인딩이 필요 하지 않은 제공 합니다. 이 프로그래밍 모델을 지원 하려면 Xamarin.iOS 런타임 클래스 선언에 [모델] 특성을 지원 합니다. 이 메서드는 하지 않는 한 클래스의 모든 메서드를 연결 하지 해야 하는 런타임 명시적으로 구현 된 것을 알립니다.

즉 UIKit에서, 선택적 방법으로 프로토콜을 나타내는 클래스를 다음과 같이 기록 됩니다.

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

만 일부 메서드를 구현 하는 모델을 구현 하려는 경우 수행 해야 하는 관심 있는 다른 메서드를 무시 하는 메서드를 재정의 하십시오. 런타임에 덮어쓴된 메서드 Objective-c 세계에 원래 메서드 하지만 생성 합니다.

이전 수동 샘플에 해당이 합니다.

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

장점은 Objective C 헤더 파일을 선택기, 인수, 또는 C#에 대 한 매핑 유형을 알고 없어도, 해당 아이콘 intellisense Visual Studio에서, Mac 용 강력한 종류와 함께


#### <a name="xib-outlets-and-c35"></a>XIB 콘센트 및 C&#35;

> [!IMPORTANT]
> 이 섹션에서는 XIB 파일을 사용 하는 경우 콘센트와 IDE의 통합을 설명 합니다. Xamarin 디자이너를 사용 하 여 iOS 용,이 모든 대체에서 이름을 입력 하 여 **Identity > 이름** 다음과 같이 IDE의 속성 섹션에서:
>
> [![](images/designeroutlet.png "IOS 디자이너에서에서 항목 이름 입력")](images/designeroutlet.png#lightbox)
>
>IOS 디자이너에서 자세한 정보를 검토 하십시오는 [iOS 디자이너 소개](~/ios/user-interface/designer/introduction.md#how-it-works) 문서.

콘센트 C#와 통합 하는 방법에 대 한 하위 수준 설명을 이므로 Xamarin.iOS의 고급 사용자를 위해 제공 됩니다. 매핑 Mac 용 Visual Studio를 사용 하 여 완료 되 면 자동으로 사용 하 여 백그라운드는 항공편에 대 한 코드 생성.

인터페이스 작성기와 함께 사용자 인터페이스를 디자인할 때에 디자인 하 게 응용 프로그램의 모양 및 일부 기본 연결을 설정 합니다. 프로그래밍 방식으로 정보를 가져오기 하 고, 런타임에 컨트롤의 동작을 변경 하거나, 런타임에 컨트롤을 수정 하려는 경우 관리 코드에 일부 컨트롤을 바인딩할입니다.

이 작업은 몇 가지 단계에서 수행 됩니다.

1.  추가 **콘센트 선언** 하 여 **파일의 소유자**합니다.
1.  사용자 컨트롤을 연결 된 **파일의 소유자만**합니다.
1.  UI를 더한 XIB/NIB 파일에 대 한 연결을 저장 합니다.
1.  런타임 시 NIB 파일을 로드 합니다.
1.  콘센트 변수에 액세스 합니다.


(3)를 통해 단계 (1) 인터페이스 작성기 인터페이스를 빌드하기 위한 Apple 설명서에 설명 되어 있습니다.

Xamarin.iOS를 사용할 때 응용 프로그램 UIViewController에서 파생 되는 클래스를 만드는 필요 합니다. 구현 되는 다음과 같이 해당:

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

그런 다음 프로그램 ViewController NIB 파일에서을 로드 하려면 이렇게 합니다.

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

이 NIB에서 사용자 인터페이스를 로드합니다. 이제는 콘센트에 액세스 하려면 필요는 액세스 하는 런타임에 알리는 데입니다. 이 작업을 수행 하는 `UIViewController` 하위 클래스의 속성을 선언 하 고 [연결] 특성으로 주석을 달 해야 합니다. 다음과 같습니다.

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

속성 구현이 실제로 인출 하 고 실제 네이티브 형식에 대 한 값을 저장 하는 것입니다.

Mac 및 InterfaceBuilder 용 Visual Studio를 사용 하는 경우이에 대해 걱정할 필요가 없습니다. Mac 용 visual Studio 코드 프로젝트의 일부분으로 컴파일되는 partial 클래스에 선언 된 모든 콘센트를 자동으로 반영 됩니다.

#### <a name="selectors"></a>선택기

Objective C 프로그래밍의 핵심 개념 선택기입니다. 선택기에 전달 해야 하거나 선택기에 응답 하도록 코드를 예상 하는 Api에서 종종 옵니다.

C#에서 새 선택기 만들기를 매우 쉽게 –의 새 인스턴스를 만듭니다는 `ObjCRuntime.Selector` 클래스 및에서 필요로 하는 API에서 임의의 위치에서 결과 사용 합니다. 예를 들어:

```csharp
var selector_add = new Selector ("add:plus:");
```

상속 해야 합니다는 C# 메서드에 응답 하는 선택기 호출에 대 한는 `NSObject` 사용 하 여 선택기 이름의 형식 및 C# 메서드를 데코레이팅 해야 합니다는 `[Export]` 특성입니다. 예를 들어:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

해당 선택기를 확인 이름 **해야** 모든 중간 및 후행 콜론을 포함 하 여 정확히 일치 (":"), 있는 경우.

#### <a name="nsobject-constructors"></a>NSObject 생성자

대부분의 클래스에서 파생 되는 Xamarin.iOS에 `NSObject` 개체의 기능에만 생성자를 노출 하는 하지만 즉시 알 수 없는 다양 한 생성자도 노출 됩니다.

생성자는 다음과 같이 사용 됩니다.

```csharp
public Foo (IntPtr handle)
```

이 생성자는 클래스를 인스턴스화하는 런타임 클래스는 관리 되지 않는 클래스에 매핑할 해야 할 때 사용 됩니다. NIB XIB/파일을 로드할 때 발생 합니다.  이 시점에서 Objective C 런타임 됩니다 개체 관리 되지 않는 세계에서 만들고 관리 되는 쪽 초기화를이 생성자가 호출 됩니다.

일반적으로 하기만 하면이 핸들 매개 변수를 사용 하 고 본문에 기본 생성자를 호출 하는 필요한 초기화 작업을 수행 합니다.

```csharp
public Foo ()
```

이것은 기본 생성자는 클래스에 대 한 고 Xamarin.iOS 클래스를 제공에서이 중간에 Foundation.NSObject 클래스 및 모든 클래스를 초기화 합니다 끝나면이가 연결 Objective C `init` 클래스에 메서드.

```csharp
public Foo (NSObjectFlag x)
```

이 생성자의 인스턴스를 초기화 하지만 코드 끝 Objective-c "init" 메서드를 호출 하지 않도록 하려면 사용 됩니다. 일반적으로 사용이 이미 초기화를 위해 등록 한 경우 (사용 하는 경우 `[Export]` 생성자에서) 또는 다른 평균을 통해 프로그램 초기화 이미 완료 경우.

```csharp
public Foo (NSCoder coder)
```

이 생성자는 개체가 NSCoding 인스턴스에서 초기화 되 고 있는 경우에 제공 됩니다. 자세한 내용은 Apple의을 참조 하십시오. [보관 함이 있고 Serialization 프로그래밍 가이드입니다.](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>예외

Xamarin.iOS API 디자인 C# 예외로 Objective C 예외를 발생 하지 않습니다. 디자인 적용 해당 없는 가비지 Objective-c 전 세계에 처음에 전송 하 고 잘못 된 데이터를 계속 하기 전에 생성 해야 하는 모든 예외 바인딩 자체에 의해 생성 된 Objective-c 전 세계에 전달 합니다.

#### <a name="notifications"></a>알림

IOS 및 OS X에서는 개발자가 하는 기본 플랫폼으로 브로드캐스팅 되며 알림에 구독할 수 있습니다. 사용 하 여 이렇게는 `NSNotificationCenter.DefaultCenter.AddObserver` 메서드. `AddObserver` 메서드는 두 개의 매개 변수; 하나는 알림 구독 하려는; 다른 하나는 알림을 발생 하는 경우 호출할 메서드입니다.

Xamarin.iOS 및 Xamarin.Mac, 다양 한 알림에 대 한 키 알림을 트리거하는 클래스에 호스트 됩니다. 알림에 의해 발생 하는 예를 들어는 `UIMenuController` 으로 호스트 `static NSString` 속성에는 `UIMenuController` 클래스 이름 "알림"로 끝나는 합니다.

### <a name="memory-management"></a>메모리 관리

Xamarin.iOS 하므로 더 이상 사용 중인 경우 사용자에 대 한 리소스를 해제 하는 가비지 수집기에 없습니다. 가비지 수집기 외에도 모든 개체에서 파생 되는 `NSObject` 구현에서 `System.IDisposable` 인터페이스입니다.

#### <a name="nsobject-and-idisposable"></a>NSObject 및 IDisposable

노출 된 `IDisposable` 인터페이스는 편리한 방법 큰 메모리 블록을 캡슐화 할 수 있는 개체 해제의 개발자 지원입니다 (예를 들어 한 `UIImage` 문제 없는 포인터 것 처럼 보이지만 2mb 이미지를 가리킬 수 없습니다 ) 및 기타 중요 한 한정 된 리소스 (예: 비디오 디코딩 버퍼).

NSObject IDisposable 인터페이스를 구현 하 고는 [.NET Dispose 패턴](http://msdn.microsoft.com/en-us/library/fs2xkftw.aspx)합니다. 이렇게 하면 개발자가 NSObject 삭제 동작을 재정의 하 고 필요에 따라 사용자의 리소스를 해제 하려면 해당 하위 클래스입니다. 예를 들어 다양 한 이미지 주위에 유지 하는이 보기 컨트롤러:

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

관리 되는 개체를 삭제할 때에 유용 더 이상 합니다. 개체가 않습니다 예 모든 용도 유효한이 시점에서 없지만 개체에 대 한 참조 여전히 있을 수 있습니다. 일부.NET Api는 ObjectDisposedException 같이 삭제 된 개체에서 모든 메서드를 액세스 하려고 하면 throw 하 여이 확인 합니다.

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

변수 "이미지" 계속 액세스할 수 없습니다, 경우에 며 실제로 잘못 된 참조가 더 이상 이미지를 보관 된 Objective-c 개체를 가리킵니다.

하지만 C#에서 개체 삭제 의미 하지는 않습니다 개체가 소멸 반드시 됩니다. 수 있었던 C# 개체에 대 한 참조를 해제 하면 됩니다. Cocoa 환경 수 자체 용도 대 한 중심 대 한 참조를 유지 한 것 같습니다. 예를 들어 이미지 UIImageView의 Image 속성을 설정 하는 경우 이미지를 삭제 한 다음 기본 UIImageView 자체 참조 수행 하 고 완료 될 때까지이 개체에 대 한 참조를 유지 합니다 사용 합니다.

#### <a name="when-to-call-dispose"></a>Dispose를 호출 하는 경우

모노 개체 제거에 필요한 경우에 Dispose를 호출 해야 합니다. 모노가 프로그램 NSObject 메모리 또는 정보 풀와 같은 중요 한 리소스에 대 한 참조를 보유 하 고 실제로 알지 사용할 수 있는 경우에도 합니다. 이 경우 즉시 모노 수행할 가비지 수집 주기를 기다리지 않고 메모리에 대 한 참조를 해제 하는 Dispose를 호출 해야 합니다.

모노 생성할 때 내부적으로 [NSString C# 문자열에서 참조](~/ios/internals/api-design/nsstring.md), 가비지 수집기가 할 작업의 양을 줄일에 즉시 이러한를 삭제 합니다. 적은 개체를 다루기가 더 빨리 GC를 실행 합니다.

#### <a name="when-to-keep-references-to-objects"></a>개체에 대 한 참조를 유지 하는 경우

자동 메모리 관리 있는 부작용 하나에 참조가 없는 상태로 GC 사용 되지 않는 개체의 rid 가져오기입니다. 이 경우에 따라 수 의도 하지 않은 놀라운 예를 들어 최상위 보기 컨트롤러를 보유 하는 로컬 변수를 만들거나 사용자 모르게 없애는 최상위 창 고 하는 것입니다.

개체 인스턴스 변수 또는 정적 프로그램에 대 한 참조를 유지 하지 않으면 모노에서는에 dispose () 메서드를 호출 코드 및 개체에 대 한 참조를 해제 합니다. 수 있으므로 뛰어난만 참조, Objective C 런타임 드립니다 개체가 삭제 됩니다.

## <a name="related-links"></a>관련 링크

- [바인딩 필드](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
