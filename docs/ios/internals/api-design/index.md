---
title: Xamarin.iOS API 디자인
description: 이 문서에서는 Xamarin.iOS Api 및 목표를 가리킵니다. 이러한 관계를 설계 하는 데 사용 하는 원칙 중 일부를 설명합니다
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 75904ad91df7795c538e736eabb6c6000847b449
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233655"
---
# <a name="xamarinios-api-design"></a>Xamarin.iOS API 디자인

Core, Mono 포함 된 기본 클래스 라이브러리 외에도 [Xamarin.iOS](http://www.xamarin.com/iOS) 다양 한 iOS 개발자 Mono를 사용 하 여 네이티브 iOS 응용 프로그램을 만들 수 있도록 Api에 대 한 바인딩을 포함 합니다.

Xamarin.iOS의 핵심은 iOS CoreGraphics C 기반 Api에 대 한 바인딩 뿐만 아니라 Objective-c로 세계를 사용 하 여 C# 세계를 연결 하는 interop 엔진 및 [OpenGL ES](#OpenGLES)합니다.

Objective-c 코드를 사용 하 여 통신 하도록 하위 수준 런타임 상태인 [MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime)합니다. 에 대 한이 바인딩이이 기반으로 [Foundation](#MonoTouch.Foundation), CoreFoundation, 및 [UIKit](#MonoTouch.UIKit) 제공 됩니다.

## <a name="design-principles"></a>디자인 원칙

다음은 디자인 원칙은 Xamarin.iOS 바인딩에 대 한 (에 적용 Xamarin.Mac, macOS에서 Objective C 용 Mono 바인딩)의 일부입니다.

- 수행 된 [Framework 디자인 지침](https://docs.microsoft.com/dotnet/standard/design-guidelines)
- Objective-c 하위 클래스입니다. 클래스에는 개발자를 허용 합니다.

  - 기존 클래스에서 파생
  - 체인에 기본 생성자를 호출 합니다.
  - C#의 재정의 시스템과 이루어져야 합니다. 메서드를 재정의 합니다.
  - C# 표준 구문을 사용 하 여 작동 해야 서브클래싱

- 개발자가 Objective-c 선택기를 노출 하지 마십시오.
- 임의의 Objective-c 라이브러리를 호출 하는 메커니즘을 제공 합니다.
- Objective-c 일반적인 어렵습니다 Objective-c 작업 가능한 확인
- C# 속성으로 Objective-c로 속성을 노출 합니다.
- 강력한 API를 노출 합니다.

  - 형식 안전성을 높일
  - 런타임 오류를 최소화 합니다.
  - 반환 형식에서 IDE IntelliSense를 받기
  - IDE 팝업 설명서에 대 한 허용

- IDE에서 탐색을 api는 것이 좋습니다.

  - 약한 형식의 배열은 다음과 같이 노출 하는 대신에 예를 들어:
    
    ```objc
    NSArray *getViews
    ```
    다음과 같은 강력한 형식을 노출 합니다.
    
    ```csharp
    NSView [] Views { get; set; }
    ```
    
    이 API를 검색 하는 동안 자동 완성 작업을 수행 하는 기능이 Mac 용 Visual Studio를 제공을 사용 하면 모든는 `System.Array` 반환된 된 값에서 사용 가능한 작업 LINQ에 참여 하려면 반환 값을 허용 하 고 있습니다.

- 네이티브 C# 형식:

  - [`NSString` 됩니다. `string`](~/ios/internals/api-design/nsstring.md)
  - 설정할 `int` 하 고 `uint` C# 열거형을 사용 하 여 C# 열거형 열거형 었어야 하는 매개 변수 `[Flags]` 특성
  - 형식 중립적인 대신 `NSArray` 개체를 강력한 형식의 배열로 배열을 노출 합니다.
  - 이벤트 및 알림에 대 한 중에서 선택할 사용자에 게 제공 합니다.

    - 기본적으로 강력한 형식의 버전
    - 고급 사용 사례에 대 한 약한 형 버전

- Objective-c 대리자 패턴을 지원 합니다.

    - C# 이벤트 시스템
    - C# 대리자 노출 (람다 식, 무명 메서드 및 `System.Delegate`) 요소로 Objective C api

### <a name="assemblies"></a>어셈블리

Xamarin.iOS를 구성 하는 어셈블리의 여러 포함 된 *Xamarin.iOS 프로필*합니다. 합니다 [어셈블리](~/cross-platform/internals/available-assemblies.md) 페이지에 자세한 정보가 있습니다.

### <a name="major-namespaces"></a>주 네임 스페이스 

<a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

합니다 [ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/) C#과 보여주기 위한 것입니다. 세계를 개발자가 네임 스페이스
Cocoa # 및 Gtk # 경험을 바탕으로 iOS를 위해 특별히 설계 된 새 바인딩입니다.

<a name="MonoTouch.Foundation" />

#### <a name="foundation"></a>Foundation

합니다 [Foundation](xref:Foundation) 네임 스페이스에는 iOS의 일부인 Objective-c Foundation 프레임 워크와 상호 운용 하도록 설계 된 기본 데이터 형식 이므로 개체 지향 프로그래밍 하기 위한 것입니다.에 대 한 기본 제공

Xamarin.iOS는 보여주기 위한 것입니다. 클래스의 계층 구조를 C#의 미러링 Objective-c로 기본 클래스 예를 들어 [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) 를 통해 C#에서 사용 가능 [Foundation.NSObject](xref:Foundation.NSObject)합니다.

이 네임 스페이스에서는 기본 Objective-c Foundation 형식에 대 한 바인딩을 제공 하지만 일부의 경우에 매핑한 내부 형식이.NET 형식으로 합니다. 예를 들어:

- 처리 하는 대신 [NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) 하 고 [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html), C#으로 이러한 런타임이 노출 [문자열](xref:System.String)s 강력한 형식의 [배열](xref:System.Array)전체 s API입니다.

- 다양 한 도우미 Api는 개발자가 타사 Api 또는 Api Xamarin.iOS를 통해 현재 연결 되어 있지 않은 다른 iOS Objective-c로 Api를 바인딩할 수 있도록 여기 노출 됩니다.

바인딩 Api에 대 한 자세한 내용은 참조는 [Xamarin.iOS 바인딩 생성기](~/cross-platform/macios/binding/binding-types-reference.md) 섹션입니다.


##### <a name="nsobject"></a>NSObject

합니다 [NSObject](xref:Foundation.NSObject) 형식은 모든 Objective-c 바인딩 위한 기초입니다. Xamarin.iOS 형식 반영 된 CocoaTouch Api iOS에서 형식의 두 클래스: (이러한 모든 NSObject 클래스에서 파생) Objective-c 형식과 C 형식 (일반적으로 CoreFoundation 형식 이라고 함).

관리 되지 않는 형식 반영 하는 각 형식에 대해를 통해 네이티브 개체를 가져올 수 있기를 [처리](xref:Foundation.NSObject.Handle) 속성입니다.

Mono는 모든 개체를 가비지 수집을 제공 하는 동안 합니다 `Foundation.NSObject` 구현 합니다 [System.IDisposable](xref:System.IDisposable) 인터페이스입니다. 즉, 시작 하는 기능에 가비지 수집기를 기다릴 필요 없이 지정 된 모든 NSObject의 리소스를 명시적으로 해제할 수 있습니다. 이것이 중요 한 많은 NSObjects, 데이터의 큰 블록에 대 한 포인터를 보유 하는 예를 들어 UIImages를 사용 하는 경우입니다.

형식이 명확한 종료를 수행 하는 경우 재정의 [NSObject.Dispose(bool) 메서드](xref:Foundation.NSObject.Dispose(System.Boolean)) Dispose의 매개 변수는 "bool disposing", 경우 설정이 true로 의미 때문에 Dispose 메서드가 호출 되는 사용자 개체에 명시적으로 호출된 Dispose ()입니다. 값이 false 인 경우 종료자 스레드에서 Dispose (bool disposing) 메서드는 종료자에서 호출 되는 것을 의미 합니다. []()


##### <a name="categories"></a>범주

Xamarin.iOS 8.10를 사용 하 여 시작 하는 것은 C#에서 Objective-c 범주를 만들 수 있습니다.

이렇게를 사용 하 여를 `Category` 특성, 특성에 대 한 인수로 확장할 형식을 지정 합니다. 예를 들어 다음 예제에서는 NSString 확장 합니다.

    [Category (typeof (NSString))]

각 범주 메서드는 Objective-c로 사용 하 여 메서드 내보내기에 대 한 일반 메커니즘을 사용 하 여 `Export` 특성:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

모든 관리 되는 확장 메서드는 정적 이어야 합니다. 이지만 C#에서 확장 메서드에 대 한 표준 구문을 사용 하 여 Objective-c 인스턴스 메서드를 만들 수 있습니다.

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

및 확장 메서드의 첫 번째 인수는 메서드가 호출 된 인스턴스가 됩니다.

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

이 예제에서는 네이티브 toUpper 인스턴스 메서드를 보여주기 위한 것입니다.에서 호출할 수 있는 NSString 클래스에 추가

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

이 방법이 유용한 시나리오 중 하나는 코드 베이스에서 클래스의 전체 집합에 메서드를 추가, 예를 들어, 이렇게 하면 모든 `UIViewController` 인스턴스에서 회전 수를 보고 합니다.

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

PreserveAttribute 경우 형식 또는 형식 멤버를 유지 하기 위해 – Xamarin.iOS 배포 도구-mtouch를 알리는 데 사용 되는 사용자 지정 특성을 해당 크기를 줄이기 위해 응용 프로그램 처리 되는 경우 단계

애플리케이션이 정적으로 연결하지 않은 모든 멤버는 제거됩니다. 따라서이 특성은 정적으로 참조 되지 않지만 여전히 응용 프로그램에 필요한 멤버를 표시 하는 데 사용 됩니다.

예를 들어 형식을 동적으로 인스턴스화하는 경우 형식의 기본 생성자를 유지하는 것이 좋습니다. XML serialization을 사용하는 경우 형식의 속성을 유지하는 것이 좋습니다.

같은 형식의 모든 멤버 또는 형식 자체에 이 특성을 적용할 수 있습니다. 전체 형식을 유지 하려는 경우 구문을 사용할 수 있습니다 [유지 (AllMembers = true)] 형식에 있습니다.

<a name="MonoTouch.UIKit" />

#### <a name="uikit"></a>UIKit

합니다 [UIKit](xref:UIKit) 네임 스페이스에는 모든 C# 클래스의 형태로 CocoaTouch를 구성 하는 UI 구성 요소에 일대일 매핑을 포함 합니다. API는 C# 언어에 사용 되는 규칙을 따라야 수정 되었습니다.

C# 대리자는 일반적인 작업에 제공 됩니다. 참조 된 [대리자](#Delegates) 자세한 내용은 섹션입니다.

<a name="OpenGLES" />

#### <a name="opengles"></a>OpenGLES

배포 합니까 OpenGLES에 대 한는 [수정 버전](https://developer.xamarin.com/api/namespace/OpenTK/) 의 합니다 [OpenTK](http://www.opentk.com/) API에만 노출 뿐만 아니라 OpenGL CoreGraphics 데이터 형식 및 구조를 사용 하 여 수정 된 개체 지향 바인딩이 합니다 iOS에서 사용할 수 있는 기능입니다.

OpenGLES 1.1 기능은 문서화 된 ES11.GL 형식을 통해 사용할 수 있습니다 [여기](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/) 형식입니다.

OpenGLES 2.0 기능은 문서화 된 ES20.GL 형식을 통해 사용할 수 있습니다 [여기](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/) 형식입니다.

OpenGLES 3.0 기능은 문서화 된 ES30.GL 형식을 통해 사용할 수 있습니다 [여기](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/) 형식입니다.


### <a name="binding-design"></a>디자인 바인딩

Xamarin.iOS 기본 Objective-c 플랫폼에 대 한 바인딩을 단순히 아닙니다. .NET 형식 시스템 및 더 나은 blend C#을 보여주기 위한 것입니다. 디스패치 시스템 확장

IJW 지원 Windows에서 COM interop에 대 한 사용 설정할 수 있습니다 또는 P/Invoke는 Windows 및 Linux에서 네이티브 라이브러리를 호출 하는 데 유용한 도구와 마찬가지로 Xamarin.iOS 런타임에서 바인딩 C# 개체가 Objective-c 개체를 확장 합니다.

몇 가지 섹션을 통해 개발자는 Xamarin.iOS 응용 프로그램을 만드는 있는 사용자에 게 필요 하지 않습니다. 다음의 설명을 작업을 완료 하 고 더 복잡 한 응용 프로그램을 만들 때 도움이 됩니다 하는 방법을 이해 합니다.



#### <a name="types"></a>유형

여기서는 것이 좋습니다, C# 형식 C# universe에 하위 수준 기반 형식 대신 노출 됩니다.  즉 [API는 C# "string" 형식을 사용 하 여 NSString 대신](~/ios/internals/api-design/nsstring.md) NSArray 노출 하는 대신 강력한 형식의 C# 배열 사용 합니다.

일반적으로 기본 Xamarin.iOS 및 Xamarin.Mac 디자인에서에서 `NSArray` 개체 노출 되지 않습니다. 대신, 런타임에서 자동으로 변환 합니다 `NSArray`일부 강력한 형식화 된 배열에의 `NSObject` 클래스입니다. 따라서 Xamarin.iOS는 NSArray 반환할 GetViews 같은 약한 형식의 메서드를 노출 하지 않습니다.

```csharp
NSArray GetViews ();
```

대신, 바인딩에 다음과 같이 강력한 형식의 반환 값을 노출합니다.

```csharp
UIView [] GetViews ();
```

노출 하는 방법 중 몇 가지 `NSArray`를 사용 하려는 코너 사례에 대 한는 `NSArray` 직접 API 바인딩에 사용은 권장 되지 않습니다. 하지만 합니다.

또한 합니다 **클래식 API** 노출 하는 대신 `CGRect`, `CGPoint` 및 `CGSize` CoreGraphics api, 교체 가진 합니다 `System.Drawing` 구현 `RectangleF`, `PointF`고 `SizeF` 는 도움을 주므로 개발자 OpenTK를 사용 하는 기존 OpenGL 코드를 유지 합니다. 새로운 64 비트를 사용 하는 경우 **Unified API**, CoreGraphics API를 사용 해야 합니다.

<a name="Inheritance" />

#### <a name="inheritance"></a>상속

Xamarin.iOS API 디자인을 사용 하면 개발자가 네이티브 Objective-c 형식 "override" 키워드를 사용 하 여 파생된 클래스에서 뿐만 아니라 "기본" C# 키워드를 사용 하 여 기본 구현에 연결는 C# 형식으로 확장은 동일한 방식으로 확장할 수 있습니다.

이 디자인 전체 Objective-c로 시스템 Xamarin.iOS 라이브러리 내에서 래핑된 이미 있으므로 개발자를 개발 프로세스의 일부로 Objective-c 선택기를 사용 하 여 처리 하지 않으려면을 수 있습니다.


#### <a name="types-and-interface-builder"></a>형식 및 Interface Builder

단일을 사용 하는 생성자를 제공 해야 하는 Interface Builder에서 만든 형식의 인스턴스인.NET 클래스를 만들면 `IntPtr` 매개 변수입니다.
이 관리 되지 않는 개체를 사용 하 여 관리 되는 개체 인스턴스를 바인딩할 필요 합니다.
코드를 다음과 같이 한 줄으로 이루어져 있습니다.

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

<a name="Delegates" />


#### <a name="delegates"></a>대리자

Objective-c와 C# 단어가 대리자에 대 한 서로 다른 의미로 각 언어에 있습니다.

Objective-c로 전 세계와는 CocoaTouch에 대 한 온라인 설명서에서 대리자는 일반적으로 메서드 집합에 응답 하는 클래스의 인스턴스입니다. 이 메서드는 항상 필수는 차이 사용 하 여 C# 인터페이스에서 매우 비슷합니다.

이러한 대리자 UIKit 및 기타 CocoaTouch Api에서 중요 한 역할을 수행 합니다. 이러한 옵션은 다양 한 작업을 수행 하는 데 사용 됩니다.

-  코드 (C# 또는 Gtk + 이벤트 배달 유사)에 대 한 알림을 제공 합니다.
-  데이터 시각화 컨트롤에 대 한 모델을 구현 합니다.
-  컨트롤의 동작을 합니다.


프로그래밍 패턴은 컨트롤에 대 한 동작을 변경 하려면 파생된 클래스의 생성을 최소화 하도록 설계 되었습니다. 이 솔루션은 비슷하며 년에 걸쳐 수행 하는 다른 GUI 도구 키트가 보이는 않은: Gtk의 신호를 Qt 슬롯, 이벤트 Winforms, WPF/Silverlight 이벤트 등. Objective-c 수백 개의 인터페이스 (각 작업에 대해 하나씩) 하거나 필요 하지 않은 많은 메서드를 구현 하는 개발자를 방지 하려면 선택적 메서드 정의 지원 합니다. 이 필요한 모든 메서드를 구현 해야 하는 C# 인터페이스와 다릅니다.

Objective-c 클래스에서 보면이 프로그래밍 패턴을 사용 하는 클래스 라는 일반적으로 속성을 노출 하 `delegate`, 인터페이스의 필수 부분 및 0 개 이상, 선택적 부분을 구현 하는 데 필요한 합니다.

이러한 대리자를 바인딩할 상호 배타적인 세 가지 메커니즘은 Xamarin.iOS에서 제공 됩니다.

1.  [이벤트를 통해](#Via_Events)합니다.
2.  [통해 강력한 형식의 `Delegate` 속성](#StrongDelegate)
3.  [통해 느슨하게 형식화 된 `WeakDelegate` 속성](#WeakDelegate)

예를 들어 합니다 [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html) 클래스입니다. 디스패치할이 [UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html) 에 할당 되는 인스턴스는 [대리자](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate) 속성입니다.

<a name="Via_Events" />

##### <a name="via-events"></a>이벤트를 통해

여러 형식에 대 한 Xamarin.iOS에서 자동으로 만듭니다 전달할는 적절 한 대리자는 `UIWebViewDelegate` C# 이벤트를 호출 합니다. `UIWebView`의 경우:

-  합니다 [webViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:) 메서드에 매핑되는 [UIWebView.LoadStarted](xref:UIKit.UIWebView.LoadStarted) 이벤트입니다.
-  합니다 [webViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:) 메서드에 매핑되는 [UIWebView.LoadFinished](xref:UIKit.UIWebView.LoadFinished) 이벤트입니다.
-  합니다 [webView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:) 메서드에 매핑되는 [UIWebView.LoadError](xref:UIKit.UIWebView.LoadError) 이벤트입니다.

예를 들어,이 간단한 프로그램을 웹 로드를 볼 때 시작 시간과 종료 시간을 기록 합니다.

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```


##### <a name="via-properties"></a>속성을 통해

이벤트는 이벤트에 대 한 구독자 둘 이상 있을 때 유용 합니다. 또한 이벤트는 사례로 제한 있는 코드에서 반환 값이 없습니다.

코드 값을 반환 하려면 필요한 경우를 선택 했습니다 대신 속성입니다. 즉, 개체에 지정된 된 시간에 하나의 메서드를 설정할 수 있습니다.

예를 들어,에 대 한 처리기에서 화면에 키보드를 해제할이 메커니즘을 사용할 수 있습니다는 `UITextField`:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

합니다 `UITextField`의 `ShouldReturn` 속성이 경우에 인수로 부울 값을 반환 하 고 있는지 여부를 텍스트 필드 이야기도 누르는 돌아가기 단추를 사용 하 여 결정 하는 대리자입니다. 이 메서드에서 반환 *true* 호출자에 게 화면에서 키보드를 제거 했습니다 (텍스트 필드를 호출 하는 경우에 이런 `ResignFirstResponder`).

<a name="StrongDelegate"/>

##### <a name="strongly-typed-via-a-delegate-property"></a>대리자 속성을 통해 강력한

이벤트를 사용 하지 않도록 하려는 경우 제공할 수 있습니다 고유한 [UIWebViewDelegate](xref:UIKit.UIWebViewDelegate) 서브 클래스에 할당 하는 [UIWebView.Delegate](xref:UIKit.UIWebView.Delegate) 속성입니다. UIWebView.Delegate에 할당 된 UIWebView 이벤트 디스패치 메커니즘은 작동 하지 않습니다 후 UIWebViewDelegate 메서드는 해당 이벤트가 발생할 때 호출 됩니다.

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

위와 같이 코드에서 사용 됩니다.

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

위의 UIWebViewer 만들어지고이 알림, 메시지에 응답 하기 위해 만든 하는 클래스의 인스턴스로 메시지를 보내도록 지시 합니다.

이 패턴은도 UIWebView 경우의 예를 들어 특정 컨트롤에 대 한 동작을 제어 하는 데 사용 됩니다는 [UIWebView.ShouldStartLoad](xref:UIKit.UIWebView.ShouldStartLoad) 속성을 사용 하면 합니다 `UIWebView` 인스턴스 제어를 여부를 `UIWebView` 로드 됩니다는 여부 페이지입니다.

패턴을 몇 가지 컨트롤에 대 한 요청 시 데이터를 제공 하도 사용 됩니다. 예를 들어 합니다 [UITableView](xref:UIKit.UITableView) 컨트롤은 강력한 표 렌더링 컨트롤 및 모양과 내용을 모두 구동 하는 인스턴스는 [UITableViewDataSource](xref:UIKit.UITableViewDataSource)

<a name="WeakDelegate"/>

### <a name="loosely-typed-via-the-weakdelegate-property"></a>느슨한 WeakDelegate 속성을 통해

강력한 형식의 속성 외에도 개발자가 필요한 경우 작업을 다르게 바인딩할 수 있도록 형식화 된 약한 대리자도 됩니다.
강력한 형식의 everywhere `Delegate` 속성을 해당 하는 Xamarin.iOS의가 바인딩에 노출 `WeakDelegate` 속성도 노출 됩니다.

사용 하는 경우는 `WeakDelegate`를 올바르게 사용 하 여 클래스를 지정 하는 일을 담당 하는 합니다 [내보내기](xref:Foundation.ExportAttribute) 선택기를 지정 하는 특성입니다. 예를 들어:

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

확인 되 면 합니다 `WeakDelegate` 속성에 할당 된는 `Delegate` 속성이 사용 되지 것입니다. 또한 [내보내기]를 추가 하려는 상속된 된 기본 클래스의 메서드를 구현 하는 경우 확인 해야 해당 공용 메서드.


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>Objective-c 대리자 패턴에서 C로의 매핑&#35;

Objective C 샘플에는 다음과 같은 표시 되 면:

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

이렇게 하면 언어를 만들어서 "SomethingDelegate" 클래스의 인스턴스를 생성 하 고 foo 변수에 대리자 속성에 값을 할당 합니다. 이 메커니즘은 Xamarin.iOS를 통해 지원 되며 C# 구문:

```csharp
foo.Delegate = new SomethingDelegate ();
```

Xamarin.iOS Objective-c에 매핑되는 강력한 형식의 클래스 대리자 클래스를 준비 했습니다. 이 사용 하려면 있습니다 서브클래싱 되며 Xamarin.iOS의 구현에 의해 정의 된 메서드를 재정의 합니다. 작동 방식에 대 한 자세한 내용은 섹션 "모델" 아래를 참조 하세요.


##### <a name="mapping-delegates-to-c35"></a>대리자에서 C로 매핑&#35;

UIKit 일반적 두 가지 형태로 Objective-c 대리자를 사용합니다.

첫 번째 형태는 구성 요소 모델에 대 한 인터페이스를 제공합니다. 예는 목록 보기에 대 한 데이터 저장소 시설 등 뷰 요청 시 데이터를 제공 하는 메커니즘입니다.  이러한 경우에 항상 적절 한 클래스의 인스턴스를 만듭니다를 변수를 할당 합니다.

다음 예에서는 제공 된 `UIPickerView` 문자열을 사용 하는 모델에 대 한 구현을:

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

두 번째 형태 이벤트에 대 한 알림을 제공 하는 것입니다. 이러한 경우에도 위에서 설명한 형태로 API를 노출 하지만 제공 빠른 작업에 사용할 간단 하 고 익명 대리자와 람다 식은 C#을 사용 하 여 통합 해야 하는 C# 이벤트를 합니다.

예를 들어, 구독할 수 있습니다 `UIAccelerometer` 이벤트:

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

두 옵션은 쉽게 이해, 하는데 프로그래머 둘 중 하나를 선택 해야 하는 경우에 사용할 수 있습니다. 강력한 형식의 응답자/대리자의 고유한 인스턴스를 만들고 할당 하는 경우 C# 이벤트 작동 되지 않습니다. C# 이벤트를 사용 하면 응답자/대리자 클래스의 메서드 호출 되지 않습니다.

앞의 예제는 `UIWebView` 같이 C# 3.0 람다를 사용 하 여 작성할 수 있습니다.

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>이벤트에 응답

Objective C 코드에서 경우에 따라 여러 컨트롤 및 여러 컨트롤에 대 한 정보는 공급자에 대 한 이벤트 처리기 호스팅될 동일한 클래스. 이것이 가능 클래스는 메시지에 응답 하 고 클래스는 메시지에 응답으로, 개체를 함께 연결 하는 것이 가능 합니다.

이전에 설명한 대로 Xamarin.iOS 모두는 C# 이벤트 기반 프로그래밍 모델을 지원 및 Objective-c 대리자 패턴을 하는 새 클래스를 만들 수 있는 대리자를 구현 및 원하는 메서드를 재정의 합니다.

것도 가능 Objective C의 패턴을 지원 하기 위해 여러 다양 한 작업에 대 한 응답 자가 모든 호스팅되는 클래스의 동일한 인스턴스에 있는 합니다. 이렇게 하려면 하지만 Xamarin.iOS 바인딩의 하위 수준 기능을 사용 해야 합니다.

예를 들어, 클래스를 둘 다에 응답 하려는 경우는 `UITextFieldDelegate.textFieldShouldClear`: 메시지 및 `UIWebViewDelegate.webViewDidStartLoad`: 클래스의 동일한 인스턴스에서 [내보내기] 특성 선언을 사용 해야 합니다.

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

C# 이름의 방법이 중요 합니다. 중요 한 것 모두 [내보내기] 특성에 전달 된 문자열입니다.

이러한 프로그래밍 스타일을 사용 하는 경우 C# 매개 변수는 런타임 엔진을 전달 하는 실제 형식을 일치 하는지 확인 합니다.

<a name="Models" />

#### <a name="models"></a>모델

UIKit 저장소 시설 또는 응답자 도우미 클래스를 사용 하 여 구현 되는 이러한 일반적으로 라고 Objective C 코드에서 대리자 및 프로토콜로 구현 됩니다.

Objective-c 프로토콜 인터페이스는 하지만 선택적 메서드를 지 – 즉, 모든 메서드의 구현 해야 작동 하는 프로토콜에 대 한 합니다.

모델을 구현 하는 방법은 두 가지가 있습니다. 수동으로 구현 하거나 기존 강력한 형식의 정의 사용 합니다.


수동 메커니즘이 Xamarin.iOS에서 바인딩되지 않은 클래스를 구현 하려고 할 때 반드시 합니다. 매우 작업을 수행 하는 것이 쉽습니다.

-  런타임에 등록에 대 한 클래스 플래그
-  재정의 하려는 각 방법에 대해 실제 선택기 이름의 [내보내기] 특성을 적용 합니다.
-  클래스를 인스턴스화하고 전달 합니다.

예를 들어, 다음 중 하나만 구현 선택적 메서드 UIApplicationDelegate 프로토콜 정의:

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Objective-c 선택기 이름 ("applicationDidFinishLaunching:")는 Export 특성을 사용 하 여 선언 된 클래스를 사용 하 여 등록 됩니다는 `[Register]` 특성입니다.

Xamarin.iOS에는 강력한 형식의 선언을 사용할 수 있는 상태가 수동 바인딩이 필요 하지 않은 제공 합니다. 이 프로그래밍 모델을 지원 하려면 Xamarin.iOS 런타임은 클래스 선언에는 [모델] 특성을 지원 합니다. 이 메서드는 경우가 아니면 클래스의 모든 메서드를 연결 하지 해야 하는 런타임으로, 명시적으로 구현 된 것을 알립니다.

즉 UIKit, 다음과 같은 선택적 메서드를 사용 하 여 프로토콜을 나타내는 클래스에 기록 됩니다.

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

만 일부의 메서드를 구현 하는 모델을 구현 하려는 경우 수행 해야 하는 관심 있는 다른 메서드를 무시 하는 메서드를 재정의. 런타임에서만 원래 메서드를 Objective-c로 전 세계 하지 덮어쓸된 메서드를 연결 합니다.

이전 수동 샘플에 해당 하는 다음과 같습니다.

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

장점은 선택기, 인수, 또는 C#에 대 한 매핑 유형을 찾으려면 Objective C 헤더 파일에 살펴볼 필요도 없을 인지, 해당 아이콘 intellisense Visual Studio에서 Mac에 대 한 강력한 형식과 함께


#### <a name="xib-outlets-and-c35"></a>XIB 출 선 및 C&#35;

> [!IMPORTANT]
> 이 섹션에서는 XIB 파일을 사용 하는 경우 콘센트를 사용 하 여 IDE 통합을 설명 합니다. IOS 용 Xamarin 디자이너를 사용 하는 경우이 모든 대체에서 이름을 입력 하 여 **Identity > 이름** 아래와 같이 IDE의 Properties 섹션에서:
>
> [![](images/designeroutlet.png "IOS 디자이너에서에서 항목 이름 입력")](images/designeroutlet.png#lightbox)
>
>IOS 디자이너에 대 한 자세한 내용은 살펴보시기 합니다 [iOS 디자이너 소개](~/ios/user-interface/designer/introduction.md#how-it-works) 문서.

C#을 사용 하 여 출 선 통합 하는 방법에 대 한 하위 수준 설명을 이며 Xamarin.iOS의 고급 사용자를 위해 제공 됩니다. 매핑 Mac 용 Visual Studio를 사용 하 여 완료 되 면 자동으로 사용 하 여 백그라운드에서 비행의 코드 생성.

Interface Builder를 사용 하 여 사용자 인터페이스를 디자인할 때 응용 프로그램의 모양을 디자인만 됩니다 하 고 일부 기본 연결 됩니다. 프로그래밍 방식으로 정보를 가져올, 런타임 시 컨트롤의 동작을 변경 하거나 런타임에 컨트롤을 수정 하려는 경우 관리 코드에 일부의 컨트롤을 바인딩할 합니다.

이 몇 가지 단계로 수행 됩니다.

1.  추가 합니다 **출 선 선언** 에 사용자 **파일의 소유자**합니다.
1.  컨트롤을 연결 합니다 **파일의 소유자**합니다.
1.  UI 및 XIB/NIB 파일에 대 한 연결을 저장 합니다.
1.  런타임 시 NIB 파일을 로드 합니다.
1.  출 선 변수에 액세스 합니다.


(3)의 단계 (1)은 Interface Builder를 사용 하 여 인터페이스를 작성 하는 것에 대 한 Apple 설명서에서 다룹니다.

Xamarin.iOS를 사용 하면 응용 프로그램 UIViewController에서 파생 된 클래스를 작성 해야 합니다. 구현 되는 다음과 같은 것:

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

이 NIB에서 사용자 인터페이스를 로드합니다. 이제 콘센트에 액세스 하려면 액세스 하려는 런타임에 알리는 데는 것이 반드시 합니다. 이 작업을 수행 하는 `UIViewController` 속성을 선언 하 고 [연결] 특성으로 주석을 달 서브 클래스 해야 합니다. 다음과 같습니다.

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

속성 구현은 실제로 인출 하 고 실제 네이티브 형식에 대 한 값을 저장 하는 것입니다.

Mac 및 InterfaceBuilder 용 Visual Studio를 사용 하는 경우이에 대해 걱정할 필요가 없습니다. Mac 용 visual Studio 프로젝트의 일환으로 컴파일되는 partial 클래스에서 코드를 사용 하 여 선언 된 모든 콘센트를 자동으로 반영 됩니다.

#### <a name="selectors"></a>선택기

Objective C 프로그래밍의 핵심 개념은 선택기입니다. 선택기를 전달 해야 하거나 선택기에 응답 하도록 코드를 예상 하는 Api에서 종종 돌아옵니다.

C#에서 새 선택기를 만들기는 매우 쉽습니다 –의 새 인스턴스를 만들면는 `ObjCRuntime.Selector` 클래스 및 필요로 하는 API의 모든 위치에서 결과 사용 합니다. 예를 들어:

```csharp
var selector_add = new Selector ("add:plus:");
```

상속 해야 합니다는 C# 메서드 응답 선택 기가 호출으로,에 대 한 합니다 `NSObject` 형식 및 C# 메서드를 사용 하 여 선택기 이름의 데코레이팅 해야 합니다는 `[Export]` 특성입니다. 예를 들어:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

해당 선택기를 확인 합니다. 이름 **해야** 모든 중간 및 후행 콜론을 포함 하 여 정확히 일치 (":")가 있으면 합니다.

#### <a name="nsobject-constructors"></a>NSObject 생성자

대부분의 클래스에서 파생 되는 Xamarin.iOS에서 `NSObject` 생성자 개체의 기능에만 노출 하지만 확실 하지 않은 다양 한 생성자도 노출 됩니다.

생성자는 다음과 같이 사용 됩니다.

```csharp
public Foo (IntPtr handle)
```

이 생성자는 클래스를 인스턴스화하는 런타임 클래스는 관리 되지 않는 클래스를 매핑할 해야 하는 경우 사용 됩니다. XIB/NIB 파일을 로드 하는 경우 발생 합니다.  이 시점에서 Objective-c 런타임에서 개체는 관리 되지 않는 환경에서 만들고 관리 쪽을 초기화 하려면이 생성자가 호출 됩니다.

일반적으로 하기만 하면 핸들 매개 변수를 사용 하 여 및 본문에 기본 생성자를 호출, 필요한 초기화 작업을 수행 됩니다.

```csharp
public Foo ()
```

이것은 클래스에 대 한 기본 생성자 및 클래스를 제공 하는 Xamarin.iOS에서이 사이 Foundation.NSObject 클래스와 클래스의 모든 초기화 끝에 관리용이 Objective-c `init` 클래스의 메서드.

```csharp
public Foo (NSObjectFlag x)
```

이 생성자, 인스턴스를 초기화는 있지만 코드 끝 Objective-c "init" 메서드를 호출 하지 못하게 됩니다. 일반적으로 사용이 이미 초기화를 위해 등록 한 경우 (사용 하는 경우 `[Export]` 생성자에 대해) 또는 때 아직 다른 의미를 통해 초기화 합니다.

```csharp
public Foo (NSCoder coder)
```

이 생성자는 개체 NSCoding 인스턴스에서 초기화 되는 사례에 대해 제공 됩니다. 자세한 내용은 Apple의을 참조 하세요. [보관 및 Serialization 프로그래밍 가이드입니다.](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>예외

Xamarin.iOS API 디자인에는 C# 예외로 Objective-c 예외 발생 하지 않습니다. 처음부터 Objective-c로 전 세계에 전송할 수 없는 가비지 디자인 적용 및 잘못 된 데이터는 전에 생성 해야 하는 모든 예외 바인딩 자체에서 생성 되는 Objective-c로 전 세계에 전달.

#### <a name="notifications"></a>알림

IOS 및 OS X에서 개발자는 기본 플랫폼에 의해 브로드캐스팅 되는 알림을 구독할 수 있습니다. 사용 하 여 이렇게는 `NSNotificationCenter.DefaultCenter.AddObserver` 메서드. `AddObserver` 알림을 구독할 하려는 것; 알림이 발생할 때 호출할 메서드는 다른 메서드는 두 개의 매개 변수를 사용 합니다.

Xamarin.iOS 및 Xamarin.Mac, 다양 한 알림에 대 한 키 알림을 트리거하는 클래스에 호스트 됩니다. 알림을 발생 하는 예를 들어,를 `UIMenuController` 으로 호스팅됩니다 `static NSString` 속성에는 `UIMenuController` 클래스 이름을 "알림"로 끝나는 합니다.

### <a name="memory-management"></a>메모리 관리

Xamarin.iOS에는 알아서 더 이상 사용 될 때 리소스를 해제 하는 가비지 수집기에 없습니다. 가비지 수집기에 외에도 모든 개체에서 파생 된 `NSObject` 구현 된 `System.IDisposable` 인터페이스입니다.

#### <a name="nsobject-and-idisposable"></a>NSObject 및 IDisposable

노출 된 `IDisposable` 인터페이스는 개발자를 지 원하는 큰 메모리 블록을 캡슐화 할 수 있는 개체를 해제 하는 편리한 방법 (예를 들어를 `UIImage` 방금 한 무고 포인터 처럼 보이지만 2mb 이미지를 가리킬 수 없습니다 ) 및 기타 중요 한 한정 된 리소스 (예: 비디오 디코딩 버퍼).

NSObject IDisposable 인터페이스를 구현 합니다. 그리고 합니다 [.NET Dispose 패턴](http://msdn.microsoft.com/library/fs2xkftw.aspx)합니다. 이렇게 하면 개발자가 NSObject 삭제 동작을 재정의 하 고 필요에 따라 자신의 리소스를 해제 하려면 해당 하위 클래스입니다. 예를 들어, 다양 한 이미지 주위를 유지 하는이 뷰 컨트롤러를 것이 좋습니다.

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

관리 되는 개체 삭제 될 때 유용한는 더 이상입니다. 개체에 대 한 참조를 겪을 수 있습니다 아닌 개체에 대 한 의도 나 용도 유효한이 시점에서. 일부.NET Api는 예를 들어 삭제 된 개체에서 메서드를 액세스 하려는 경우는 ObjectDisposedException을 throw 하 여이 확인 합니다.

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

변수의 "이미지"를 이용할 수는 것은 실제로 잘못 된 참조와 더 이상 이미지를 보유 하는 Objective-c 개체를 가리킵니다.

하지만 C#에서 개체 삭제 되지는지 않습니다 개체 소멸 반드시 않습니다. C# 개체에 대 한 참조를 해제 하기만 하면 됩니다. Cocoa 환경 수 주위 자체 용도 대 한 참조를 유지 한 것 같습니다. 예를 들어, 이미지, UIImageView의 Image 속성을 설정한 경우 이미지를 삭제 한 다음, 기본 UIImageView 수행 된 자체 참조 및 완료 될 때까지이 개체에 대 한 참조를 유지 하는 사용 합니다.

#### <a name="when-to-call-dispose"></a>Dispose를 호출 하는 경우

개체의 없애는 Mono를 사용 해야 하는 경우에 Dispose를 호출 해야 합니다. Mono는 프로그램 NSObject 정보 풀이나 메모리와 같은 중요 한 리소스에 대 한 참조를 보유 하 고 실제로 알지 때 사용할 수 있는 사례를 표시 합니다. 이러한 경우에 가비지 컬렉션 주기를 수행 하는 Mono를 기다리지 않고 메모리에 대 한 참조를 즉시 해제는 Dispose를 호출 해야 합니다.

Mono를 만들 때 내부적으로 [NSString C# 문자열에서 참조](~/ios/internals/api-design/nsstring.md), 가비지 수집기는 작업을 수행 하는 작업의 양을 줄일 수는 즉시 삭제 됩니다 것입니다. 더 빨리 GC를 처리 하기 위한 해결 방법은 개체의 수가 적을수록 실행 됩니다.

#### <a name="when-to-keep-references-to-objects"></a>개체에 대 한 참조를 유지 하는 경우

한 가지 부작용 자동 메모리 관리 하는 경우에 대 한 참조가 없는으로 GC 사용 되지 않는 개체의 rid 가져오기 이 경우에 따라 수 부작용 놀라운 예를 들어, 상위 수준 보기 컨트롤러를 보유 하는 로컬 변수를 만들거나 사용자 모르게 사용자의 최상위 창 및 해당 있으면 사라집니다.

개체에는 참조에 정적에서 또는 인스턴스 변수를 유지 하지 않습니다 Mono 다행히에 dispose () 메서드를 호출 하 고 개체에 대 한 참조를 해제 합니다. 만 해결 되지 않은 참조가 생길 수, 있으므로 Objective-c 런타임에서 개체를 삭제 됩니다.

## <a name="related-links"></a>관련 링크

- [필드 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
