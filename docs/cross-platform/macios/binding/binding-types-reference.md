---
title: "바인딩 형식에 대 한 가이드"
ms.topic: article
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 568650a850b9db1fa22deef55eebb6a437e7e0b7
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/17/2018
---
# <a name="binding-types-reference-guide"></a>바인딩 형식에 대 한 가이드

이 문서에 코드 생성 및를 실행 하는 바인딩 API 계약 파일에 주석을 추가 하는 데 사용할 수 있는 특성의 목록을 설명 합니다.

Xamarin.iOS 및 Xamarin.Mac API 계약은 Objective C 코드를 C# 표시 되는 방식을 정의 하는 인터페이스 정의 주로으로 C#에서 기록 됩니다. 인터페이스 선언 및 API 계약이 필요할 수 있는 몇 가지 기본 형식 정의 혼합 하 여 작업을 수행 합니다. 바인딩 형식에 대 한 소개를 부록 가이드를 참조 하십시오. [바인딩 Objective C 라이브러리](~/cross-platform/macios/binding/objective-c-libraries.md)합니다.

## <a name="type-definitions"></a>형식 정의입니다.

구문:

```csharp
[BaseType (typeof (BTYPE))
interface MyType [: Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

가 계약 정의에서 모든 인터페이스는 `[BaseType]` 생성 된 개체에 대 한 기본 형식을 선언 하는 특성입니다. 위 선언에서 한 `MyType` 바인딩 Objective C 형식에 호출 되도록 C# 형식의 클래스를 생성할 **MyType**합니다.

Typename 후 하는 형식을 지정 하는 경우 (위의 예제에서 `Protocol1` 및 `Protocol2`) 인터페이스 상속 구문을 사용 하 여 이러한 인터페이스의 내용을 인라인 됩니다에 대 한 계약의 일부 되었을 것 마치 `MyType`합니다.
형식 프로토콜을 채택 Xamarin.iOS 표면에서 하는 방법은 메서드 및 속성 형식 자체에 프로토콜에 선언 된 모든 인라인 처리 합니다.

에서는 다음 방법을 Objective C 선언에 대 한 `UITextField` Xamarin.iOS 계약에 정의 됩니다.

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

C# API 계약으로 다음과 같이 작성할 수 있습니다:

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

BaseType 특성 구성 뿐만 아니라 인터페이스에 다른 특성을 적용 하 여 코드 생성의 다른 많은 측면을 제어할 수 있습니다.


### <a name="generating-events"></a>이벤트를 생성합니다.

Xamarin.iOS 및 Xamarin.Mac API 디자인의 한 가지 특징으로 C# 이벤트와 콜백을 Objective-c 대리자 클래스를 매핑할 것입니다. 같은 속성을 할당 하 여 Objective C 프로그래밍 패턴을 채택 하 게 할지를 사용자가 인스턴스 단위로에서 선택할 수 **대리자** 다양 한 메서드를 구현 하는 클래스의 인스턴스는 Objective C 런타임이 호출 또는 C#을 선택 하 여-스타일 지정 이벤트 및 속성입니다.

Objective C 모델을 사용 하는 방법의 한 가지 예 참조 알려 주세요.

```csharp
bool MakeDecision ()
{
    return true;
}

void Setup ()
{
     var scrollView = new UIScrollView (myRect);
     scrollView.Delegate = new MyScrollViewDelegate ();
     ...
}

class MyScrollViewDelegate : UIScrollViewDelegate {
    public override void Scrolled (UIScrollView scrollView)
    {
        Console.WriteLine ("Scrolled");
    }

    public override bool ShouldScrollToTop (UIScrollView scrollView)
    {
        return MakeDecision ();
    }
}
```

위의 예제에서 두 개의 메서드를 덮어쓰도록 선택한, 하나는 스크롤 이벤트와 통합 하였습니다 있으며 두 번째, 하는 알림을 위쪽으로 스크롤하여 채워야 여부는 scrollView에 지시 하는 부울 값을 반환 해야 하는 콜백을 볼 수 있습니다 또는 필요는 없습니다.

C# 모델을 사용 하면 라이브러리의 사용자 속성 구문 또는 C# 구문 이벤트를 사용 하 여 값을 반환 하는 콜백을 후크 알림을 수신 대기 하도록 합니다.

이 람다 사용과 같은 다음과 같은 기능에 대 한 C# 코드:

```csharp
void Setup ()
{
    var scrollview = new UIScrollView (myRect);
    // Event connection, use += and multiple events can be connected
    scrollView.Scrolled += (sender, eventArgs) { Console.WriteLine ("Scrolled"); }

    // Property connection, use = only a single callback can be used
    scrollView.ShouldScrollToTop = (sv) => MakeDecision ();
}
```

이벤트 (void 반환 형식을 가진) 값을 반환 하지 않는 이후 여러 복사본을 연결할 수 있습니다. `ShouldScrollToTop` 은 (는) 이벤트 유형 포함 하는 속성은 대신 `UIScrollViewCondition` 서명이 있는:

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

Bool 값을 반환 하면이 경우 람다 구문 통해 하는 값을 반환할는 `MakeDecision` 함수입니다.

이벤트와 같은 클래스를 연결 하는 속성과 생성 바인딩 생성기 지원 `UIScrollView` 와 해당 `UIScrollViewDelegate` (도 이러한 모델 클래스)를 호출에 주석 지정 하면 됩니다 프로그램 `BaseType` 함께 정의 `Events` 및 `Delegates`매개 변수 (아래 설명 참조). 주석을 추가 하는 것 외에도 `BaseType` 해당 매개 변수를 사용 해야 하는 몇 가지 구성 요소가 더 생성자에 게 알립니다.

둘 이상의 매개 변수를 사용 하는 이벤트에 대 한 (Objective C의 규칙은 대리자 클래스의 첫 번째 매개 변수는 보낸 사람에 게 개체의 인스턴스는)를 원하는 되도록 생성된 된 EventArgs 클래스에 대 한 이름을 제공 해야 합니다. 이러한 용도로 `EventArgs` 모델 클래스에서 메서드 선언에서 특성입니다. 예를 들어:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

위의 선언을 생성 됩니다는 `UIImagePickerImagePickedEventArgs` 에서 파생 된 클래스 `EventArgs` 및 매개 변수를 모두 팩는 `UIImage` 및 `NSDictionary`합니다. 생성기에서이 오류를 생성 합니다.

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

그런 다음 UIImagePickerController 클래스에서 다음을 제공합니다.

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

값을 반환 하는 모델 메서드를 다르게 바인딩됩니다. 이러한 생성된 된 C# 대리자 (메서드에 대 한 서명) 되 고 기본 값을 반환 해야 사용자 자신 구현을 제공 하지 않는 경우에 이름을 모두 필요 합니다. 예를 들어는 `ShouldScrollToTop` 이 정의 됩니다.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

위의 만듭니다는 `UIScrollViewCondition` 시그니처가 대리자 위에 나온 및 사용자를 구현 하지 않습니다, 반환 값 true 됩니다.

이외에 `DefaultValue` 특성을 사용할 수도 있습니다는 `DefaultValueFromArgument` 생성자 호출에 지정된 된 매개 변수의 값을 반환 하도록 지시 하는 또는 `NoDefaultValue` 기본값을 갖지 있다는 것을 생성자에 게 지시 하는 매개 변수입니다.


### <a name="basetypeattribute"></a>BaseTypeAttribute

구문:

```csharp
public class BaseTypeAttribute : Attribute {
        public BaseTypeAttribute (Type t);

        // Properties
        public Type BaseType { get; set; }
        public string Name { get; set; }
        public Type [] Events { get; set; }
        public string [] Delegates { get; set; }
        public string KeepRefUntil { get; set; }
}
```

#### <a name="basetypename"></a>BaseType.Name

사용 된 `Name` Objective-c 상황에서이 형식에 바인딩합니다 이름을 제어 하는 속성입니다. 이 C# 형식에 이름을.NET Framework 디자인 지침을 준수 하지만 해당 규칙을 따르지 않는 Objective C의 이름에 매핑되는 일반적으로 사용 됩니다.

예제에서는 다음과 같은 경우 Objective C 매핑할 `NSURLConnection` 형식을 `NSUrlConnection`, "URL" 대신 "Url"를 사용 하는.NET Framework 디자인 지침:

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

지정 된 이름을 생성 된 항목에 대 한 값으로 사용 하기 위해 지정 된 `[Register]` 바인딩에 특성입니다. 경우 `Name` 를 지정 하지 않으면 해당 형식의 약식 이름에 대 한 값으로 사용 됩니다는 `Register` 생성 된 출력에는 특성입니다.

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType.Events 및 BaseType.Delegates

C#의 생성을 진행 하는 데 이러한 속성을 사용 하는-생성 된 클래스에서 이벤트 스타일입니다. Objective C 대리자 클래스는 지정 된 클래스를 연결 하는 데 사용 됩니다. 대부분의 경우 클래스 알림 및 이벤트에 대리자 클래스를 사용 하는 경우 발생 합니다. 예를 들어 한 `BarcodeScanner` 과 함께 사용 해야 `BardodeScannerDelegate` 클래스입니다. `BarcodeScanner` 클래스의 인스턴스를 할당할 때 "대리자" 속성 것이 일반적으로 `BarcodeScannerDelegate` 에이 방법이 가능 하지만, 사용자에 게 노출 하는 C# 하려는-스타일 이벤트 인터페이스와 같은 및 사용 하 여 이러한 경우에는 `Events` 및 `Delegates` 의 속성은 `BaseType` 특성입니다.

이러한 속성은 항상 함께 설정 및 요소 수가 있고 동기화 되어야 합니다. `Delegates` 래핑할 하려는 각 약한 형식의 대리자에 대해 하나의 문자열을 포함 하는 배열 및 유형임을 연결 하려는 각 형식에 대 한 이벤트 배열에 포함 합니다.

```csharp
[BaseType (typeof (NSObject),
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIAccelerometerDelegate)})]
public interface UIAccelerometer {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIAccelerometerDelegate {
}
```


#### <a name="basetypekeeprefuntil"></a>BaseType.KeepRefUntil

이 클래스의 새 인스턴스를 만들 때이 특성을 적용 하는 경우 해당 개체의 인스턴스를 유지할지 주위에서 참조 하는 메서드의 될 때까지 `KeepRefUntil` 호출 했습니다. 이 사용자가 직접 코드를 사용 하도록 기준 개체에 대 한 참조를 유지 하도록 하지 않을 때 사용자 Api의 유용성을 개선 하는 데 유용 합니다. 이 속성의 값은에 있는 메서드의 이름은 `Delegate` 이 이벤트와 함께에서 사용 해야 하므로 클래스 및 `Delegates` 도 속성을 합니다.

다음 예제에서 사용 방법을 보여 줍니다 `UIActionSheet` Xamarin.iOS에서:

```csharp
[BaseType (typeof (NSObject), KeepRefUntil="Dismissed")]
[BaseType (typeof (UIView),
           KeepRefUntil="Dismissed",
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIActionSheetDelegate)})]
public interface UIActionSheet {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIActionSheetDelegate {
    [Export ("actionSheet:didDismissWithButtonIndex:"), EventArgs ("UIButton")]
    void Dismissed (UIActionSheet actionSheet, nint buttonIndex);
}
```


### <a name="disabledefaultctorattribute"></a>DisableDefaultCtorAttribute

이 특성은 인터페이스 정의에 적용 되는 생성기의 기본 생성자를 생성 수 없게 됩니다.

개체를 클래스에는 다른 생성자 중 하나를 사용 하 여 초기화 해야 하는 경우이 특성을 사용 합니다.


### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

이 특성은 인터페이스 정의에 적용 되는 기본 생성자는 private로 문법 됩니다. 즉, 확장 파일에서이 클래스의 개체를 내부적으로 인스턴스화할 여전히 있습니다 있지만 것 바로 못함 클래스의 사용자가 액세스할 수 있습니다.


### <a name="categoryattribute"></a>CategoryAttribute

Objective-c 범주를 바인딩하는 형식 정의에이 특성을 사용 하 고 Objective-c 항목이 방식에 확장 메서드는 C#으로 노출 하는 기능을 노출 합니다.

범주는 일련의 메서드와 클래스에서 사용할 수 있는 속성을 확장 하는 데는 Objective-c 메커니즘입니다.   실제로 기본 클래스의 기능을 확장 하거나 사용 됩니다 (예를 들어 `NSObject`) 특정 프레임 워크에 연결 된 경우 (예를 들어 `UIKit`)을 사용할 수는 있지만 새로운 프레임 워크에 연결 되어 있는 경우에 해당 메서드를 만드는 합니다.   경우에 따라 다른 기능으로는 클래스의 기능을 구성 하 사용 됩니다.   비슷하기 개념상에서 C# 확장 메서드입니다.

이 어떤 범주에에서 나타나는 모양을 Objective c:

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

위의 예에서는 경우에의 인스턴스를 확장 하는 것을 라이브러리 `UIView` 메서드로 `makeBackgroundRed`합니다.

사용할 수 있는 바인딩하려면는 `[Category]` 인터페이스 정의의 특성입니다.   사용 하는 경우는 `Category` 특성의 의미는 `[BaseType]` 특성이 기본 클래스를 확장 하, 확장 형식이 되도록 지정 하는 데 사용 되 고에서 변경 합니다.

에서는 다음 방법을 `UIView` 확장은 바인딩된 있으며 확장 메서드가 C#으로 변환 합니다.

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

위의 만듭니다는 `MyUIViewExtension` 포함 하는 클래스는 `MakeBackgroundRed` 확장 메서드.   즉, 이제 "MakeBackgroundRed" 호출할 수 있습니다에 `UIView` Objective c 얻을 동일한 기능을 제공 하는 하위 클래스

일부 경우에는 있습니다 **정적** 다음 예제에서와 같이 범주 내부 멤버:

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

나타낼는 **잘못** 범주 C# 인터페이스 정의:

```csharp
[Category]
[BaseType (typeof (FooObject))]
interface FooObject_Extensions {

    // Incorrect Interface definition
    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

이것은 잘못 된 때문에 사용 하는 `BoolMethod` 확장 인스턴스에 필요한 `FooObject` 는 ObjC 바인딩하는 있지만 **정적** 확장,이 C# 확장 메서드를 구현 하는 방법의 팩트로 인해 부작용으로 나타납니다.

위의 정의 사용 하는 유일한 방법은 다음 까다로운 코드에 의해 됩니다.

```csharp
(null as FooObject).BoolMethod (range);
```

인라인이 문제를 방지에 대 한 권장 되는 `BoolMethod` 내부 정의 `FooObject` 인터페이스 정의 자체, 이렇게 하면 같은 것은이 확장을 호출할 수 있습니다 `FooObject.BoolMethod (range)`합니다.

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

찾을 때마다 경고 (BI1117) 발급 됩니다는 `[Static]` 내부 멤버는 `[Category]` 정의 합니다. 원하는 경우 `[Static]` 내부 멤버 여 `[Category]` 정의 사용 하 여 경고 소리가 나 지 않도록 수 `[Category (allowStaticMembers: true)]` 하나 통해, 또는 멤버 또는 `[Category]` 인터페이스를 정의 `[Internal]`합니다.


### <a name="staticattribute"></a>StaticAttribute

방금 정적 클래스에서 파생 되지 않는 것이 특성이 클래스에 적용 되 면 기반인 `NSObject` 하므로 `[BaseType]` 특성은 무시 됩니다. 정적 클래스 C 공용 변수를 노출 하려면를 호스트 하는 데 사용 됩니다.

예를 들어:

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

다음 API 사용 하는 C# 클래스를 생성 합니다.

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```


## <a name="protocol-definitionsmodel"></a>프로토콜 정의/모델

모델은 일반적으로 프로토콜 구현에 의해 사용 됩니다.
런타임에서에 등록 됩니다 Objective-c 실제로 덮어썼습니다 메서드 한다는 점에서 다릅니다.
그렇지 않으면 메서드는 등록 되지 않습니다.

일반적으로 의미 하위 사용 되도록 지정 하는 클래스는 `ModelAttribute`, 기본 메서드를 호출 하지 않아야 합니다.   해당 메서드를 호출 하면 예외가 throw 됩니다, 그리고 재정의 하는 방법에 대 한 서브 클래스에 전체 동작을 구현 하는 것입니다.


### <a name="abstractattribute"></a>AbstractAttribute

기본적으로는 프로토콜의 일부인 멤버는 필수입니다. 이렇게 하면 사용자의 서브 클래스를 만들 수는 `Model` 단순히 C# 클래스에서 파생 하 고 처리 되는 메서드만 재정의 개체입니다. 경우에 따라 Objective-c 계약에 필요한 사용자가이 메서드에 대 한 구현을 제공 하 (하는 것으로 플래그가 지정는 @required Objective-c 지시문). 이러한 경우에 사용 하는 메서드를 플래그 지정 해야는 `Abstract` 특성입니다.

`Abstract` 특성 메서드 또는 속성에 적용할 수 있습니다 및 생성자가 추상 클래스로 "추상" 및 클래스로 생성 된 멤버 플래그를 지정 합니다.

다음에서 가져옵니다 Xamarin.iOS:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UITableViewDataSource {
    [Export ("tableView:numberOfRowsInSection:")]
    [Abstract]
    nint RowsInSection (UITableView tableView, nint section);
}
```

<a name="DefaultValueAttribute" />

### <a name="defaultvalueattribute"></a>DefaultValueAttribute

사용자는 모델 개체의이 특정 메서드에 대 한 메서드를 제공 하지 않는 경우 모델 메서드에 의해 반환 될 기본값 지정

구문:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

에 대 한 다음과 같은 가상 대리자 클래스의 예를 들어 한 `Camera` 클래스를 제공는 `ShouldUploadToServer` 있는에 속성으로 노출 됩니다는 `Camera` 클래스입니다. 하는 경우의 사용자는 `Camera` 클래스 명시적으로 설정 하지 않습니다는 true 또는 false 응답할 수 있는 lambda 값, 기본 값 반환이 경우 false가 됩니다, 변수에 지정한 값은 `DefaultValue` 특성:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

허수 클래스 처리기를 설정한 경우이 값 무시 합니다.

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

참고 항목: [NoDefaultValueAttribute](#NoDefaultValueAttribute), [DefaultValueFromArgumentAttribute](#DefaultValueFromArgumentAttribute)합니다.

<a name="DefaultValueFromArgumentAttribute" />

### <a name="defaultvaluefromargumentattribute"></a>DefaultValueFromArgumentAttribute

구문:

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

모델 클래스에는 값을 반환 하는 메서드를 제공 하는 경우이 특성은 메서드 또는 람다가 자신의 사용자 제공 하지 않은 경우 지정된 된 매개 변수의 값을 반환 하는 생성기를 지시 합니다.

예제:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

위의 경우에서의 사용자는 `NSAnimation` 클래스 C# 이벤트/속성을 사용 하 고 설정 하지 않은 `NSAnimation.ComputeAnimationCurve` 메서드 또는 람다, 반환 값에 게 진행률 매개 변수에 전달 된 값입니다.

참고 항목: [NoDefaultValueAttribute](#NoDefaultValueAttribute), [DefaultValueAttribute](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>IgnoredInDelegateAttribute

경우에 따라 하면 하지 하는 이벤트를 노출할 또는 대기자 속성에서 모델 클래스를 호스트 클래스에 있으므로이 특성을 추가 된 데코 레이트 된 모든 메서드의 생성 되지 않도록 하려면 생성기 지시 합니다.

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);

    [Export ("imagePickerController:didFinishPickingImage:"), IgnoredInDelegate)] // No event generated for this method
    void FinishedPickingImage (UIImagePickerController picker, UIImage image);
}
```

### <a name="delegatenameattribute"></a>DelegateNameAttribute

이 특성은 반환 값을 사용 하는 대리자 시그니처에의 이름을 설정 하는 모델 메서드에서 사용 됩니다.

예제:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

위의 정의 따르면 생성자는 다음 공용 선언을 생성 합니다.

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>DelegateApiNameAttribute

이 특성은 사용 하 여 호스트 클래스에서 생성 된 속성의 이름을 변경 하려면 생성기에 있도록 합니다. 것이 유용한 경우가 FooDelegate 클래스 메서드의 이름을 대리자 클래스에 적합 하지만 속성으로 호스트 클래스에서 이상 할 것입니다.

실제로 유용한 (및 필요한)이는 또한 있는 경우 두 또는 FooDelegate 클래스에 있는 그대로 라는 적합 하는 오버 로드 메서드가 유지 있지만 때 더 나은 이름이 지정 된 호스트 클래스에 노출 합니다.

예제:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

위의 정의 사용 하 여 생성기 호스트 클래스의 공용 다음 선언을 생성 합니다.

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

### <a name="eventargsattribute"></a>EventArgsAttribute

둘 이상의 매개 변수를 사용 하는 이벤트에 대 한 (Objective C의 규칙은 대리자 클래스의 첫 번째 매개 변수는 보낸 사람에 게 개체의 인스턴스는)를 원하는 되도록 생성된 된 EventArgs 클래스에 대 한 이름을 제공 해야 합니다. 이러한 용도로 `EventArgs` 에서 메서드 선언에서 특성 프로그램 `Model` 클래스입니다.

예를 들어:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

위의 선언을 생성 됩니다는 `UIImagePickerImagePickedEventArgs` EventArgs에서 파생 되 고 매개 변수를 모두를 압축 하는 클래스는 `UIImage` 및 `NSDictionary`합니다. 생성기에서이 오류를 생성 합니다.

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

그런 다음 UIImagePickerController 클래스에서 다음을 제공합니다.

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```


### <a name="eventnameattribute"></a>EventNameAttribute

이 특성은 사용 하 여 이벤트 클래스에서 생성 하는 속성의 이름을 변경 하려면 생성기에 있도록 합니다. 경우에 유용 때의 이름을 `Model` 클래스 메서드 모델 클래스에 적합 하지만 이벤트 또는 속성으로 원래 클래스에서 이상 할 것입니다.

예를 들어는 `UIWebView` 에서 다음 비트를 사용 하 여 `UIWebViewDelegate`:

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

위의 노출 `LoadingFinished` 의 메서드로 `UIWebViewDelegate`, 하지만 `LoadFinished` 최대에 후크 할 이벤트로는 `UIWebView`:

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```


### <a name="modelattribute"></a>ModelAttribute

적용 하는 경우는 `Model` 런타임 API 계약의 형식 정의에 특성은만 노출할 호출 클래스에 메서드를 사용자가 클래스의 메서드를 덮어쓸 경우 특수 한 코드를 생성 합니다. 이 특성은 일반적으로 Objective-c 대리자 클래스를 래핑하는 모든 Api에 적용 됩니다.

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

모델에 대 한 메서드는 기본 반환 값을 제공 하지 않기를 지정 합니다.

Objective C 런타임 "false" 응답 하 여 수는 지정된 된 선택기가이 클래스에서 구현 하는 경우를 확인 하려면 Objective C 런타임 요청 합니다.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

참고 항목: [DefaultValueAttribute](#DefaultValueAttribute) 및 [DefaultValueAttribute](#DefaultValueAttribute)합니다.

## <a name="protocols"></a>프로토콜

Objective C 프로토콜 개념 C#에서 실제로 존재 하지 않습니다. 프로토콜 C#의 인터페이스 유사 하지만 것을 채택 하는 클래스에서 구현 해야 하는 메서드 및 프로토콜에 선언 된 속성 중 일부만 한다는 점에서 다릅니다. 대신 메서드 및 속성의 일부는 선택 사항입니다.

일부 프로토콜은 일반적으로 모델 클래스로 사용, 해당 모델 특성을 사용 하 여 바인딩된 해야 합니다.

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}
```

바인딩 기능 새롭고 향상 된 프로토콜 MonoTouch 7.0 부터는 통합 되었습니다.  포함 된 모든 정의 `[Protocol]` 특성은 실제로 크게 프로토콜을 사용 하는 방식을 개선 하는 세 가지 지원 클래스를 생성 합니다.

```csharp
// Full method implementation, contains all methods
class MyProtocol : IMyProtocol {
    public void Say (string msg);
    public void Listen (string msg);
}

// Interface that contains only the required methods
interface IMyProtocol: INativeObject, IDisposable {
    [Export ("say:")]
    void Say (string msg);
}

// Extension methods
static class IMyProtocol_Extensions {
    public static void Optional (this IMyProtocol this, string msg);
    }
}
```

**클래스 구현을** 의 개별 메서드를 재정의 하 고 전체 형식 안전성을 얻을 수 있는 완전 한 추상 클래스를 제공 합니다. 그러나 C# 다중 상속을 지원 하지 않으므로, 인해 있습니다 시나리오는 서로 다른 기본 클래스를 필요로 하지만 인터페이스를 구현 해야 할 수 있습니다.

여기에 생성 된 **인터페이스 정의** 제공 합니다.  에 프로토콜에서 필요한 모든 메서드는 인터페이스입니다.  따라서 단순히 인터페이스를 구현 하 여 프로토콜을 구현 하려는 개발자가 있습니다.  런타임은 프로토콜 채택으로 형식을 자동으로 등록 됩니다.

고 해당 인터페이스만 필요한 메서드를 나열 합니다. 선택적 메서드를 노출 않는 됩니다.   즉, 프로토콜을 채택 하는 클래스 필요한 방법에 대 한 검사 하는 전체 형식 발생 합니다 있지만 약한 형식 지정 (수동으로 내보내기 특성을 사용 하 여 및 서명 일치)에 의존 해야 합니다는 선택적 프로토콜 방법에 대 한 합니다.

바인딩 도구는를 프로토콜을 사용 하는 API를 사용 하기 편리 하 게 하는 모든 선택적 메서드를 노출 하는 확장 메서드 클래스도 생성 합니다.   이 API를 사용 하는 상태로 됩니다 프로토콜 모든 메서드가 있는 것으로 취급할 수를 의미 합니다.

API에는 프로토콜 정의 사용 하려는 경우에 API 정의에서 기본 빈 인터페이스를 작성 해야 합니다.   MyProtocol API에서 사용 하려는 경우이 작업을 수행 해야 합니다.

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}

interface IMyProtocol {}

[BaseType (typeof(NSObject))]
interface MyTool {
    [Export ("getProtocol")]
    IMyProtocol GetProtocol ();
}
```

위의 필요 하기 때문에 바인딩할 때는 `IMyProtocol` 존재 하지 않습니다, 즉 빈 인터페이스를 제공 해야 하는 이유입니다.

### <a name="adopting-protocol-generated-interfaces"></a>프로토콜에서 생성 된 인터페이스를 채택합니다.

구현할 때마다 다음과 같이 프로토콜에 대해 생성 된 인터페이스 중 하나:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

인터페이스 메서드에 대 한 구현 자동으로 가져옵니다 내보내기 적절 한 이름으로이에 해당 하는 다음과 같습니다.

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

암시적 또는 명시적으로 인터페이스를 구현 하는 경우에 중요 하지 않습니다.

### <a name="protocol-inlining"></a>인라인 처리 프로토콜

프로토콜을 채택로 선언 되어 기존 Objective C 형식에 바인딩하는 동안 하려는 인라인 프로토콜 직접 합니다. 이렇게 하려면 단순히 프로토콜으로 선언 하지 않고 인터페이스 `[BaseType]` 특성 및 사용자 인터페이스에 대 한 기본 인터페이스 목록에 프로토콜을 나열 합니다.

예제:

```csharp
interface SpeakProtocol {
    [Export ("say:")]
    void Say (string msg);
}

[BaseType (typeof (NSObject))]
interface Robot : SpeakProtocol {
    [Export ("awake")]
    bool Awake { get; set; }
}
```


## <a name="member-definitions"></a>멤버 정의

이 섹션의 특성이 형식의 개별 멤버에 적용 됩니다: 속성 및 메서드 선언 합니다.


### <a name="alignattribute"></a>AlignAttribute

속성 반환 형식에 대 한 맞춤 값을 지정 하는 데 사용 합니다. 특정 속성 사용 특정 경계에 맞춰야 하는 주소에 대 한 포인터 (예를 들어 일부에서 이런 Xamarin.iOS에서 `GLKBaseEffect` 정렬 하는 16 바이트 해야 하는 속성). Getter를 데코레이팅하이 속성을 사용할 수 있으며 맞춤 값을 사용 하 여 있습니다. 일반적으로 사용 되며이 `OpenTK.Vector4` 및 `OpenTK.Matrix4` Objective-c Api와 통합 하는 경우 형식입니다.

예제:

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```


### <a name="appearanceattribute"></a>AppearanceAttribute

`Appearance` 특성 모양 관리자 도입 된 iOS5로 제한 됩니다.

`Appearance` 메서드 또는 속성에 참여 하는 모든 특성을 적용할 수 있습니다는 `UIAppearance` 프레임 워크입니다. 이 특성이 메서드 또는 클래스에서 속성에 적용 되 면이 클래스의 모든 인스턴스 또는 인스턴스에 특정 조건과 일치 하는 스타일을 지정 하는 데 사용 되는 모양을 강력한 형식의 클래스를 만드는 바인딩 생성기를 지시 합니다.

예제:

```csharp
public interface UIToolbar {
    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);
}
```

위의 UIToolbar의 다음 코드를 생성 합니다.

```csharp
public partial class UIToolbar {
    public partial class UIToolbarAppearance : UIView.UIViewAppearance {
        public virtual void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);
        public virtual UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics)
    }
    public static new UIToolbarAppearance Appearance { get; }
    public static new UIToolbarAppearance AppearanceWhenContainedIn (params Type [] containers);
}
```

### <a name="autoreleaseattribute-xamarinios-54"></a>AutoReleaseAttribute (Xamarin.iOS 5.4)

사용 하 여는 `AutoReleaseAttribute` 메서드에 메서드 호출을 래핑하려면 하도록 메서드 및 속성에는 `NSAutoReleasePool`합니다.

Objective C에는 기본값에 추가 하는 값을 반환 하는 몇 가지 메서드 `NSAutoReleasePool`합니다. 기본적으로 이러한을 거쳐야 스레드 `NSAutoReleasePool`, 하지만 Xamarin.iOS도 관리 되는 개체가으로 개체에 대 한 참조를 유지, 되므로 추가 내용을 변경 하지 않으려면 원하지 수는 `NSAutoReleasePool` 스레드 될 때까지 소모 가져오기 됩니다는 다음 스레드에 컨트롤을 반환 하거나 주 루프로 다시 이동 합니다.

이 특성은 예를 들어 높은 속성에 적용 됩니다 (예를 들어 `UIImage.FromFile`) 기본값에 추가 된 개체를 반환 하는 `NSAutoReleasePool`합니다. 이 특성이 없으면으로 주 루프에 스레드 컨트롤을 반환 하지 않았습니다 이미지를 유지 합니다. 스레드 Uf 일종의 배경 다운로더 항상 활성화 되어 있는 인시던트 였으며 작업을 대기 이미지 되지 해제 됩니다.

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

`ForcedTypeAttribute` 반환 되는 관리 되지 않는 개체에 바인딩 정의에 설명 된 형식과 일치 하지 않는 경우에 관리 되는 형식 만들기를 적용 하는 데 사용 됩니다.

헤더에 설명 된 형식의 네이티브 메서드의 반환된 형식을 일치 하지 않는 경우 유용, 예를 들어 다음 Objective-c 정의에서 걸릴 `NSURLSession`:

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

명확 하 게 반환 하는 것에 따르면는 `NSURLSessionDownloadTask` 인스턴스 하지만 아직 것 **반환** 는 `NSURLSessionTask`, 슈퍼 클래스 변수인 따라서 하지 변환할 및 `NSURLSessionDownloadTask`합니다. 형식이 안전한 컨텍스트 이므로 `InvalidCastException` 일이 발생 합니다.

헤더 파일에 대 한 설명 준수 하 고 방지 하는 `InvalidCastException`, `ForcedTypeAttribute` 사용 됩니다.

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

`ForcedTypeAttribute` 라는 부울 값을 받기도 `Owns` 즉 `false` 기본적으로 `[ForcedType (owns: true)]`합니다. 따르도록 매개 변수를 사용 하는 소유 하는 [소유권 정책을](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html) 에 대 한 **코어 Foundation** 개체입니다.

`ForcedTypeAttribute` 에서만 유효 `parameters`, `properties` 및 `return value`합니다.

### <a name="bindasattribute"></a>BindAsAttribute

`BindAsAttribute` 바인딩을 허용 `NSNumber`, `NSValue` 및 `NSString`(열거형)를 보다 정확 하 게 C# 형식으로. 특성 보다 효율적이 고, 더 정확 하 게 만드는 데 사용할 수는 네이티브 API 통해.NET API입니다.

(반환 값)에 대 한 메서드, 매개 변수 및 사용 하 여 속성 데코레이팅 할 수 있습니다 `BindAs`합니다. 유일한 제한 사항은 구성원은 **하지 않아야** 안에 `[Protocol]` 또는 `[Model]` 인터페이스입니다.

예를 들어:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

이 출력 됩니다.

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

작업이 수행 됩니다 내부적으로 `bool?`  <->  `NSNumber` 및 `CGRect`  <->  `NSValue` 변환 합니다.

현재 지원 되는 캡슐화 유형은 다음과 같습니다.

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

다음 C# 데이터 형식에서 /으로 캡슐화 할 사용할 `NSValue`:

* CGAffineTransform
* NSRange
* CGVector
* SCNMatrix4
* CLLocationCoordinate2D
* SCNVector3
* SCNVector4
* CGPoint / PointF
* CGRect / RectangleF
* CGSize / SizeF
* UIEdgeInsets
* UIOffset
* MKCoordinateSpan
* CMTimeRange
* CMTime
* CMTimeMapping
* CATransform3D

#### <a name="nsnumber"></a>NSNumber

다음 C# 데이터 형식에서 /으로 캡슐화 할 사용할 `NSNumber`:

* bool
* byte
* double
* float
* short
* int
* long
* sbyte
* ushort
* uint
* ulong
* nfloat
* nint
* nuint
* 열거형

#### <a name="nsstring"></a>NSString

`[BindAs]` 와 conjuntion에서 작동 하는 [NSString 상수 뒷받침 되며 열거형](#enum-attributes) 예를 들어 더 나은.NET API를 만들 수 있습니다.

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

이 출력 됩니다.

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

처리는 `enum`  <->  `NSString` 변환을 제공 된 열거형을 입력 하는 경우에 `[BindAs]` 은 [NSString 상수 뒷받침 되며](#enum-attributes)합니다.

#### <a name="arrays"></a>배열

`[BindAs]` 또한 배열을 지원 모든 지원 되는 형식의 예를 들어 다음 API 정의 포함할 수 있습니다.

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

이 출력 됩니다.

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

`rects` 에 매개 변수를 캡슐화 됩니다는 `NSArray` 를 포함 하는 `NSValue` 각각에 대해 `CGRect` 수 있으며 그러면 배열을 `CAScroll?` 는 만든 반환 된 값을 사용 하 여 `NSArray` 포함 된 `NSStrings`합니다.

### <a name="bindattribute"></a>BindAttribute

`Bind` 특성의 용도 메서드 또는 속성 선언과 개별 getter 또는 setter를 속성에 적용할 때 다른 항목에 적용 될 때 하나 있습니다.

메서드 또는 속성에 사용할 경우 바인딩 특성의 효과 지정 된 선택기를 호출 하는 메서드를 생성 하는 것입니다. 으로 생성된 된 결과 메서드를 데코레이팅하 지 않으면 하지만 `[Export]` 특성을 메서드가 재정의에 참여 하지 않을 수 있습니다. 이 일반적으로 함께에서 사용 되어는 `Target` Objective-c 확장 메서드를 구현 하기 위한 특성입니다.

예를 들어:

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

Getter 또는 setter를 사용 하는 경우는 `Bind` 특성을 사용 하는 속성에 대 한 getter 및 setter Objective-c 선택기 이름을 생성할 때 코드 생성기에 의해 유추 기본값을 변경 합니다. 기본적으로 이름은 "fooBar"를 사용 하 여 속성 플래그를 지정 하는 경우 생성자 생성 getter에 대 한 "fooBar" 내보내기 및 "setFooBar:" setter에 대 한 합니다. 일부 상황에서는 Objective-c이이 규칙을 따르지 않습니다, 그리고 일반적으로 getter 이름을 "isFooBar"으로 변경 합니다.
이 생성자를 알리기 위해이 특성을 사용 합니다.

예를 들어:

```csharp
// Default behavior
[Export ("active")]
bool Active { get; set; }

// Custom naming with the Bind attribute
[Export ("visible")]
bool Visible { [Bind ("isVisible")] get; set; }
```


### <a name="asyncattribute"></a>AsyncAttribute

만 Xamarin.iOS 6.3에서 사용할 수 이상입니다.

마지막 인수와 완료 처리기를 사용 하는 메서드로이 특성을 적용할 수 있습니다.

사용할 수는 `[Async]` 메서드에 적용 된 마지막 인수는 콜백을 특성입니다.  이 메서드를 적용 하면 바인딩 생성기에서 접미사와 해당 메서드의 버전을 생성 `Async`합니다.  콜백 매개 변수를 사용 하는 경우 반환 값 됩니다는 `Task`, 콜백 매개 변수를 사용 하는 경우 결과 작업 됩니다&lt;T&gt;합니다.

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

다음은이 비동기 메서드에 생성 합니다.

```csharp
Task LoadFileAsync (string file);
```

콜백이 여러 매개 변수를 사용 하는 경우 설정 해야는 `ResultType` 또는 `ResultTypeName` 생성 된 형식의 모든 속성을 포함 하는 원하는 이름을 지정할 수 있습니다.

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

다음에는이 비동기 메서드에 생성 됩니다 여기서 `FileLoading` "files"와 "byteCount"에 액세스 하는 속성이 포함 되어 있습니다.

```csharp
Task<FileLoading> LoadFile (string file);
```

콜백 마지막 매개 변수는 경우는 `NSError`의 생성 된 `Async` 메서드 확인 하는 경우 값이 null로, 해당 되는 경우 생성 된 비동기 메서드가 작업 예외를 설정 됩니다.

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

위의 다음 비동기 메서드를 생성합니다.

```csharp
Task<string> UploadAsync (string file);
```

오류가 발생할 경우 결과 작업이 갖습니다로 설정 하는 예외는 `NSErrorException` 결과 래핑하는 `NSError`합니다.

#### <a name="asyncattributeresulttype"></a>AsyncAttribute.ResultType

반환 하는 것에 대 한 값을 지정 하려면이 속성을 사용 하 여 `Task` 개체입니다.   이 매개 변수는 기존 형식, 핵심 api 정의 중 하나에 정의 되어야 하는 데 필요한 따라서 합니다.

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute.ResultTypeName

반환 하는 것에 대 한 값을 지정 하려면이 속성을 사용 하 여 `Task` 개체입니다.   이 매개 변수는 원하는 형식 이름, 생성자는 일련의 속성이 콜백을 사용 하는 각 매개 변수에 대해 하나씩 생성 됩니다.

#### <a name="asyncattributemethodname"></a>AsyncAttribute.MethodName

생성 된 비동기 메서드의 이름을 사용자 지정 하려면이 속성을 사용 합니다.   기본값은 "Async" 텍스트를 추가 하 고 메서드의 이름을 사용 하 여,이 기본값을 변경 하는 데 사용할 수 있습니다.

### <a name="disablezerocopyattribute"></a>DisableZeroCopyAttribute

이 특성 문자열 매개 변수 또는 문자열 속성에 적용 되 고이 매개 변수에 대 한 마샬링 0 복사 문자열을 사용 하지 않도록 하는 코드 생성기 및 대신 C# 문자열에서 새 NSString 인스턴스를 만듭니다.
이 특성에만 필요 문자열 생성기 0 복사 문자열 마샬링 중 하나를 사용 하 여 사용 하도록 지시 하는 경우는 `--zero-copy` 명령줄 옵션 또는 어셈블리 수준 특성을 설정 `ZeroCopyStringsAttribute`합니다.

이 필요한 경우 속성을 Objective C의 선언 된 위치에 "복사" 속성 대신 "유지" 또는 "할당" 속성 이어야 합니다. 이 일반적으로 잘못 "최적화" 개발자가 하는 타사 라이브러리에서 발생 합니다. 일반적으로 "유지" 또는 "할당" `NSString` 속성이 이후 올바른지 `NSMutableString` 또는 사용자에서 파생 된 클래스의 `NSString` 미세 하 게 응용 프로그램을 주요 라이브러리 코드의 지식 없이 문자열의 내용을 변경 될 수 있습니다. 일반적으로이 시기 상 조일 최적화로 인해 발생합니다.

Objective c:에 다음은 이러한 두 속성

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```


### <a name="disposeattribute"></a>DisposeAttribute

적용 하는 경우는 `DisposeAttribute` 에 추가할 수 있는 코드 조각을 제공 하는 클래스에는 `Dispose()` 클래스의 메서드를 구현 합니다.

이후는 `Dispose` 메서드를 자동으로 생성 됩니다는 `bmac-native` 및 `btouch-native` 사용 해야 하는 도구는 `Dispose` 특성으로 생성 된 일부 코드를 삽입할 `Dispose` 메서드 구현 합니다.

예를 들어:

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```


### <a name="exportattribute"></a>ExportAttribute

`Export` 특성은 메서드 또는 Objective-c 런타임에 노출 해야 하는 속성 플래그를 설정 하려면 사용 합니다. 이 특성은 바인딩 도구 및 실제 Xamarin.iOS 및 Xamarin.Mac 런타임 간에 공유 됩니다. 메서드의 경우 매개 변수는 생성 된 코드에 축 자에 대해 전달 된 속성, getter 및 setter 내보내기를 기본 선언에 따라 생성 됩니다 (섹션을 참조는 `BindAttribute` 바인딩 도구의 동작을 변경 하는 방법에 대 한 내용은).

구문:

```csharp
public enum ArgumentSemantic {
    None, Assign, Copy, Retain.
}

[AttributeUsage (AttributeTargets.Method | AttributeTargets.Constructor | AttributeTargets.Property)]
public class ExportAttribute : Attribute {
    public ExportAttribute();
    public ExportAttribute (string selector);
    public ExportAttribute (string selector, ArgumentSemantic semantic);
    public string Selector { get; set; }
    public ArgumentSemantic ArgumentSemantic { get; set; }
}
```

[선택기](http://developer.apple.com/library/ios/#documentation/cocoa/conceptual/objectivec/Chapters/ocSelectors.html) 메서드 또는 바인딩되는 속성의 기본 Objective-c 이름을 나타냅니다.


#### <a name="exportattributeargumentsemantic"></a>ExportAttribute.ArgumentSemantic


### <a name="fieldattribute"></a>FieldAttribute

이 특성은 필요할 때 로드 되 고 C# 코드에 노출 하는 필드로 C 전역 변수를 노출 하려면 사용 합니다. C 또는 C 목표에 정의 되어 및 일부 Api에 사용 되는 토큰 중 하나가 될 수 있죠 또는 해당 값이 불투명 하 고로 사용 해야 하는 상수 값을 가져오는 데 필요 일반적으로-사용자 코드에 의해 됩니다.

구문:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

`symbolName` 와 링크 하려면 C 기호입니다. 기본적으로이 로드 하는 형식이 정의 되어 있는 이름이 네임 스페이스에서 유추 되는 라이브러리에서 됩니다. 이 정보가 기호를 조회 되는 라이브러리를 전달 해야는 `libraryName` 매개 변수입니다. 정적 라이브러리를 연결 하 고 사용 하 여 "__Internal"로 `libraryName` 매개 변수입니다.

생성 된 속성은 항상 정적입니다.

Field 특성을 플래그로 속성 다음 유형 중 하나일 수 있습니다.

* `NSString`
* `NSArray`
* `nint` / `int` / `long`
* `nuint` / `uint` / `ulong`
* `nfloat` / `float`
* `double`
* `CGSize`
* `System.IntPtr`
* 열거형

Setter는 지원 되지 않습니다 [열거형 NSString 상수에 의해 백업](#enum-attributes), 수동으로 바인딩될 있습니다 필요한 경우 하지만 합니다.

예제:

```csharp
[Static]
interface CameraEffects {
     [Field ("kCameraEffectsZoomFactorKey", "CameraLibrary")]
     NSString ZoomFactorKey { get; }
}
```

### <a name="internalattribute"></a>InternalAttribute

`Internal` 효과가 "내부" C# 키워드와 생성된 된 어셈블리의 코드를 코드에만 액세스할 수 있도록 생성된 된 코드를 플래그 지정 및 특성은 메서드 또는 속성에 적용할 수 있습니다. 일반적으로 너무 낮은 수준의 Api을 숨기고 시 또는 생성기에서 지원 되지 않는 Api에 대 한 개선 하기 위해 원하는 최적이 아닌 공용 API를 제공 하거나 일부 직접 코딩을 요구 사용 됩니다.

바인딩 디자인, 일반적으로이 특성을 사용 하 여 속성 또는 메서드 숨기기 하는 메서드 또는 속성에 대 한 다른 이름을 제공 및 다음 C# 상호 보완적인 지원 파일을 추가할 때 노출 되는 강력한 형식의 래퍼를 때는 기본 기능입니다.

예를 들어:

```csharp
[Internal]
[Export ("setValue:forKey:");
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

그런 다음 프로그램 지원 파일에서 다음과 같은 몇 가지 코드를 사용할 수 있습니다.

```csharp
public NSObject this [NSObject idx] {
    get {
        return _GetValueForKey (idx);
    }
    set {
        _SetValueForKey (value, idx);
    }
}
```

### <a name="isthreadstaticattribute"></a>IsThreadStaticAttribute

이 특성 플래그는.NET으로 주석을 달아야 하는 속성에 대 한 지원 필드 `[ThreadStatic]` 특성입니다. 필드가 스레드 정적 변수 경우에 유용 합니다.

### <a name="marshalnativeexceptions-xamarinios-606"></a>MarshalNativeExceptions (Xamarin.iOS 6.0.6)

이 특성 메서드 지원 네이티브 (ObjectiveC) 예외를 확인 합니다.
호출 하는 대신 `objc_msgSend` 를 직접 호출 ObjectiveC 예외를 catch 하 고 관리 되는 예외를 마샬링합니다는 사용자 지정 trampoline 설명 합니다.

현재 일부만 `objc_msgSend` 서명이 지원 됩니다 (배우면 서명을 누락 monotouch_ 실패 하는 바인딩을 사용 하는 앱의 네이티브 연결 하는 경우 지원 되지 않습니다 경우*_objc_msgSend* 기호), 될 수 있습니다 더 있지만 요청에 추가 합니다.


### <a name="newattribute"></a>NewAttribute

이 특성은 메서드 및 속성을 선언 앞에 "new" 키워드를 생성 하는 생성기에 적용 됩니다.

동일한 메서드 또는 속성 이름은 기본 클래스에 이미 존재 하는 하위 클래스에 도입 된 때 컴파일러 경고를 방지 하기 위해 사용 됩니다.


### <a name="notificationattribute"></a>NotificationAttribute

강력한 형식의 도우미 알림 클래스 생성기 생성을 필드에이 특성을 적용할 수 있습니다.

이 특성을 사용 하 여 알림을 하지 않는 페이로드를 전송 하는 인수 없이 하거나 지정할 수 있습니다는 `System.Type` "EventArgs"로 끝나는 이름의 일반적으로 API 정의에서 다른 인터페이스를 참조 하는 합니다. 생성기 바뀝니다 인터페이스 클래스 서브클래싱하는 `EventArgs` 여기에 나열 된 속성을 모두 포함 됩니다. `[Export]` 특성을 사용 해야는 `EventArgs` 클래스 값을 인출 Objective-c 사전을 조회 하는 데 사용 되는 키의 이름을 표시 합니다.

예를 들어:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

위의 코드에서는 중첩된 된 클래스를 발생 `MyClass.Notifications` 다음 방법을 사용 하 여:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

사용자를 다음 쉽게 알림을 구독 하려면에 게시는 [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) 다음과 같은 코드를 사용 하 여:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

또는 관찰 하는 특정 개체를 설정할 수 있습니다. 전달 하는 경우 `null` 를 `objectToObserve` 이 메서드에 다른 피어와 동일 하 게 작동 합니다.

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

반환 된 값 `ObserveDidStart` 쉽게 다음과 같이 알림 수신을 중지할 데 사용할 수 있습니다.

```csharp
token.Dispose ();
```

호출할 수 있습니다 또는 [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject//) 토큰을 전달 합니다. 프로그램 알림 매개 변수를 포함 하는 경우에 도우미 지정 해야 `EventArgs` 다음과 같은 인터페이스:

```csharp
interface MyClass {
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}

// The helper EventArgs declaration
interface MyScreenChangedEventArgs {
    [Export ("ScreenXKey")]
    nint ScreenX { get; set; }

    [Export ("ScreenYKey")]
    nint ScreenY { get; set; }

    [Export ("DidGoOffKey")]
    [ProbePresence]
    bool DidGoOff { get; }
}
```

위의 생성 됩니다는 `MyScreenChangedEventArgs` 클래스와 `ScreenX` 및 `ScreenY` 에서 데이터를 인출 하는 속성의 [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) 키를 사용 하 여 사전 **ScreenXKey** 및 **ScreenYKey** 각각 적절 한 변환을 적용 합니다. `[ProbePresence]` 특성은 키 설정 된 경우를 검색 하는 생성기에 사용 된 `UserInfo`, 값을 추출 하는 것이 아니라 합니다. 이 경우 여기서 키의 존재는 값 (일반적으로 값에 대 한 부울)에 사용 됩니다.

이 옵션을 사용 하면 다음과 같은 코드를 작성할 수 있습니다.

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

경우에 따라 사전에 전달 된 값과 관련 된 상수가 없는 있습니다.  Apple 공용 기호 상수를 사용 하는 경우도 및 문자열 상수를 사용 하는 경우도 있습니다.  기본적으로는 `[Export]` 특성에 제공 된 프로그램 `EventArgs` 클래스 런타임에 조회 공용 기호 지정 된 이름을 사용 합니다.  경우 이것이 대신 할 문자열 상수 조회할 수 다음 전달 하는 경우는 `ArgumentSemantic.Assign` 내보내기 특성에는 값입니다.

**Xamarin.iOS 8.4의에서 새로운 기능**

경우에 따라 알림을 시작 합니다 인수 없이 수명 하므로 사용 `[Notification]` 인수를 허용 하지 않고 있습니다.  하지만 경우에 따라 알림에 대 한 매개 변수를 소개 합니다.  이 시나리오를 지원 하려면 특성을 두 번 이상 적용할 수 있습니다.

바인딩을 개발 하는 기존 사용자 코드를 수정 하지 않으려는 경우에서 기존 알림 설정:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

다음과 같이 알림 특성을 두 번 나열 된 버전:

```csharp
interface MyClass {
    [Notification]
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

### <a name="nullallowedattribute"></a>NullAllowedAttribute

이 속성에 적용 되 면 속성 값을 할당 해야 하는 null을 허용 하도록 플래그 지정 합니다. 참조 형식에 대해서는이 유효합니다.

메서드 시그니처에서 매개 변수에 적용 되는이 지정된 된 매개 변수를 null 일 수 있습니다 및 검사 없이 null 값 전달 하기 위해 수행 해야 함을 나타냅니다.

참조 형식에이 특성이 없으면 바인딩 도구 Objective-c 전달 하기 전에 할당 되는 값에 대 한 검사를 생성 하는 및이 발생 시키는 check가 생성 하는 프로그램 `ArgumentNullException` 할당 된 값이 null입니다.

예를 들어:

```csharp
// In properties

[NullAllowed]
UIImage IconFile { get; set; }

// In methods
void SetImage ([NullAllowed] UIImage image, State forState);
```

<a name="OverrideAttribute"/>

### <a name="overrideattribute"></a>OverrideAttribute

이 특정 메서드에 대 한 바인딩 "재정의" 키워드와 함께 플래그 지정 해야 바인딩 생성기에 하도록 지시 하려면이 특성을 사용 합니다.


### <a name="presnippetattribute"></a>PreSnippetAttribute

이 특성을 사용 하 여 입력된 매개 변수를 확인 한 후 하지만 Objective-c에 코드 호출 하기 전에 삽입 하는 코드를 삽입 합니다.

예제:

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```


### <a name="prologuesnippetattribute"></a>PrologueSnippetAttribute

매개 변수 중 생성 된 메서드의 유효성을 검사 하기 전에 삽입 하는 코드를 삽입 하려면이 특성을 사용할 수 있습니다.

예제:

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```


### <a name="postgetattribute"></a>PostGetAttribute

여기에서 값을 가져올 수 있는이 클래스에서 지정된 된 속성을 호출 하는 바인딩 생성기에 지시 합니다.

이 속성은 참조 된 개체 그래프를 보관 하는 개체를 가리키는 캐시를 새로 고칠 일반적으로 사용 됩니다. 일반적으로 표시 추가/제거와 같은 작업이 포함 된 코드를 합니다. 이 메서드는 실제로 사용 중인 개체에 대 한 관리 되는 참조를 유지 한 후 요소가 추가 되거나 제거 되도록 내부 캐시를 업데이트할 수 있도록 사용 됩니다. 바인딩 도구에 지정된 된 바인딩에 모든 참조 개체에 대 한 지원 필드를 생성 하기 때문에 이것이 가능 합니다.

예제:

```csharp
[BaseType (typeof (NSObject))]
[Since (4,0)]
public interface NSOperation {
    [Export ("addDependency:")][PostGet ("Dependencies")]
    void AddDependency (NSOperation op);

    [Export ("removeDependency:")][PostGet ("Dependencies")]
    void RemoveDependency (NSOperation op);

    [Export ("dependencies")]
    NSOperation [] Dependencies { get; }
}
```

이 경우에 `Dependencies` 속성을 추가 하거나에서 종속 요소를 제거한 후 호출 됩니다는 `NSOperation` 실제 나타내는 그래프 되도록 개체 메모리 누수 뿐만 아니라 메모리 손상을 방지 하는 개체를 로드 합니다.


### <a name="postsnippetattribute"></a>PostSnippetAttribute

이 특성을 사용 하 여 코드 내부 Objective-c 메서드를 호출한 후 삽입할 C# 소스 코드를 삽입 합니다.

예제:

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```


### <a name="proxyattribute"></a>ProxyAttribute

프록시 개체에 있는 것으로 플래그를 지정 하는 값을 반환 하려면이 특성은 적용 됩니다. 일부 Objective-c Api 반환 프록시 개체를 사용자 바인딩에서 구분할 수 있습니다. 이 특성의 효과 개체를 플래그 지정 하는 것을 `DirectBinding` 개체입니다. Xamarin.Mac의 시나리오에서는 볼 수 있습니다는 [이 버그에 대해 토론](https://bugzilla.novell.com/show_bug.cgi?id=670844)합니다.


### <a name="retainlistattribute"></a>RetainListAttribute

관리 되는 참조 매개 변수를 유지 하거나 내부 매개 변수 참조를 제거 하려면 생성기에 지시 합니다. 이 참조 하는 개체에 사용 됩니다.

구문:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

"DoAdd"의 값이 true 인 경우는 매개 변수는 추가 `__mt_{0}_var List<NSObject>;`합니다. 여기서 `{0}` 아래 템플릿으로 바뀝니다는 주어진 `listName`합니다. API에 상호 보완적인 부분 클래스에이 지원 필드를 선언 해야 합니다.

예를 참조 하십시오. [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) 및 [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin.iOS 6.0)

반환 형식 생성자를 호출 해야 나타내려면에 적용할 수 있습니다이 `Release` 반환 하기 전에 개체에 있습니다. 이 경우에 필요 (autoreleased 있는 개체를 가장 일반적인 시나리오) 대비 보존 개체는 메서드는 제공

예제:

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

또한이 특성 Xamarin.iOS 런타임 Objective-c를 이러한 함수에서 반환 하면 개체를 유지 해야 알 수 있도록 생성 된 코드에 전파 됩니다.


### <a name="sealedattribute"></a>SealedAttribute

Sealed로 생성된 된 메서드 플래그를 설정 하려면 생성기에 지시 합니다. 이 특성을 지정 하지 않으면 (가상 메서드, 추상 메서드 또는 다른 특성 사용법에 따라 재정의)는 가상 메서드를 생성 하는 것이 기본값이입니다.


### <a name="staticattribute"></a>StaticAttribute

경우는 `Static` 특성이 적용 된 메서드 또는 속성에이 정적 메서드 또는 속성을 생성 합니다. 이 특성을 지정 하지 않으면 메서드는 인스턴스 메서드 또는 속성 생성기는 생성 됩니다.


### <a name="transientattribute"></a>TransientAttribute

이 특성 값은 임시적인 플래그 속성을 사용 하 여, iOS에서 일시적으로 만들어진 하지만 오래 되지 않은 개체의 수명이 즉, 합니다. 이 특성 속성에 적용 되 면 생성자에서 관리 되는 클래스 개체에 대 한 참조를 보관 하지 않습니다는이 속성에 대 한 지원 필드를 만들지 않습니다.


### <a name="wrapattribute"></a>WrapAttribute

Xamarin.iOS/Xamarin.Mac 바인딩의 디자인에는 `Wrap` 특성 포함 된 강력한 형식의 개체를 약한 형식의 개체를 래핑하는 데 사용 됩니다. 이 고려해 Objective-c "위임" 개체 형식의 것으로 일반적으로 선언 되는 대부분와 `id` 또는 `NSObject`합니다. 이러한 대리자 또는 데이터 원본 형식으로 노출 하 Xamarin.iOS 및 Xamarin.Mac에서 사용 하는 규칙은 `NSObject` 및 규칙 "Weak" + 노출 되는 이름을 사용 하 여 이름을 지정 합니다. Objective C는 "id 대리자" 속성으로 노출 됩니다는 `NSObject WeakDelegate { get; set; }` API 계약 파일에서 속성입니다.

하지만 하므로 강력한 형식을 노출 하 고 적용이 대리자에 할당 된 값을 강력한 형식의은 일반적으로 `Wrap` 특성, 즉, 사용자가 일부 세밀 하 게 제어 해야 하는 경우 또는 하위 수준 tric에 의존 하는 경우 약한 형식을 사용 하도록 선택할 수 ks, 또는 업무의 대부분에 대 한 강력한 형식의 속성을 사용할 수 있습니다.

예제:

```csharp
[BaseType (typeof (NSObject))]
interface Demo {
     [Export ("delegate"), NullAllowed]
     NSObject WeakDelegate { get; set; }

     [Wrap ("WeakDelegate")]
     DemoDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
interface DemoDelegate {
    [Export ("doDemo")]
    void DoDemo ();
}
```

다음은 방법을 사용 합니다. 대리자의 약한 형식의 버전입니다.

```csharp
// The weak case, user has to roll his own
class SomeObject : NSObject {
    [Export ("doDemo")]
    void CallbackForDoDemo () {}

}

var demo = new Demo ();
demo.WeakDelegate = new SomeObject ();
```

이 사용자가 강력한 형식의 버전을 사용 하 여, 그 없는지를 수동으로 사용자 C#의 형식 시스템의 이점을 활용 하 고 override 키워드를 사용 하 여 그의 의도 선언 하는 확인할 방법 데코레이팅 사용 하 여 메서드 및 `Export`수행한 것 않아야 하므로 사용자에 대 한 바인딩을 사용할 수 있습니다.

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```


또 다른 용도 `Wrap` 특성은 메서드의 강력한 형식의 버전을 지원 하도록 합니다.   예를 들어:

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

에 의해 생성 된 멤버 `[Wrap]` 없는 `virtual` 해야 할 경우 기본적으로는 `virtual` 구성원으로 설정할 수 있습니다 `true` 선택적 `isVirtual` 매개 변수입니다.

```csharp
[BaseType (typeof (NSObject))]
interface FooExplorer {
    [Export ("fooWithContentsOfURL:")]
    void FromUrl (NSUrl url);

    [Wrap ("FromUrl (NSUrl.FromString (url))", isVirtual: true)]
    void FromUrl (string url);
}
```

## <a name="parameter-attributes"></a>매개 변수 특성

메서드 정의의 매개 변수에 적용할 수 있는 특성을 설명 하는이 섹션으로 `NullAttribute` 전체적으로 속성에 적용 되는 합니다.

<a name="BlockCallback" />

### <a name="blockcallback"></a>BlockCallback

이 특성에 매개 변수 Objective-c 블록 호출 규칙을 준수 하 고 이러한 방식으로 마샬링하고 바인더에 알리기 위해 C# 대리자 선언에서 매개 변수 형식에 적용 됩니다.

Objective c:에 다음과 같이 정의 된 콜백에 일반적으로 사용 됩니다.

```csharp
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

참고 항목: [CCallback](#CCallback)합니다.

<a name="CCallback" />

### <a name="ccallback"></a>CCallback

이 특성에 매개 변수 C ABI 함수 포인터 호출 규칙을 준수 하 고 이러한 방식으로 마샬링하고 바인더에 알리기 위해 C# 대리자 선언에서 매개 변수 형식에 적용 됩니다.

Objective c:에 다음과 같이 정의 된 콜백에 일반적으로 사용 됩니다.

    typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);

참고 항목: [BlockCallback](#BlockCallback)합니다.

### <a name="params"></a>매개 변수

사용할 수는 `[Params]` "params" 정의에 삽입할 생성기를 메서드 정의의 마지막 배열 매개 변수에 특성입니다.   따라서 쉽게 선택적 매개 변수를 허용 하도록 바인딩을 수 있습니다.

예를 들어, 다음 정의:

    [Export ("loadFiles:")]
    void LoadFiles ([Params]NSUrl [] files);

다음 코드를를 작성할 수 있습니다.

    foo.LoadFiles (new NSUrl (url));
    foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));

이 추가 된 사용자가을 요소 전달에 대 한 순수 하 게 배열을 만들 필요가 없습니다.

<a name="plainstring" />

### <a name="plainstring"></a>PlainString

사용할 수는 `[PlainString]` 특성으로 매개 변수를 전달 하는 대신 C 문자열로 문자열을 전달 하는 바인딩 생성기 하도록 지시 하려면 문자열 매개 변수 앞에 `NSString`합니다.

대부분의 Objective-c Api 사용 `NSString` 매개 변수가 있지만 소수의 Api 노출 한 `char *` 대신 문자열을 전달 하기 위한 API는 `NSString` 변형 합니다.
사용 하 여 `[PlainString]` 경우에서.

예를 들어, 다음 Objective-c 선언:

```csharp
- (void) setText: (NSString *) theText;
- (void) logMessage: (char *) message;
```

다음과 같이 바인딩되어야 합니다.

```csharp
[Export ("setText:")]
void SetText (string theText);

[Export ("logMessage:")]
void LogMessage ([PlainString] string theText);
```


### <a name="retainattribute"></a>RetainAttribute

생성자가 지정 된 매개 변수에 대 한 참조를 유지 하도록 지시 합니다. 생성기에서이 필드에 대 한 백업 저장소를 제공 합니다 또는 이름을 지정할 수 있습니다 (의 `WrapName`)에서 값을 저장 합니다. Objective C를 매개 변수로 전달 되 고 Objective-c 개체의이 복사본을 보관 합니다만 알고 있는 경우 관리 되는 개체에 대 한 참조를 보유 하는 것과 유용 합니다. 예를 들어,와 같은 API `SetDisplay (SomeObject)` SetDisplay는 한 번에 하나의 개체를 표시할 수만 것 이므로이 특성을 사용 합니다. 사용 추적 하기 위해 둘 이상의 개체 (예: 스택 모양의 API) 해야 할 경우는 `RetainList` 특성입니다.

구문:

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```


### <a name="retainlistattribute"></a>RetainListAttribute

관리 되는 참조 매개 변수를 유지 하거나 내부 매개 변수 참조를 제거 하려면 생성기에 지시 합니다. 이 참조 하는 개체에 사용 됩니다.

구문:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

"DoAdd"의 값이 true 인 경우는 매개 변수는 추가 `__mt_{0}_var List<NSObject>`합니다. 여기서 `{0}` 아래 템플릿으로 바뀝니다는 주어진 `listName`합니다. API에 상호 보완적인 부분 클래스에이 지원 필드를 선언 해야 합니다.

예를 참조 하십시오. [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) 및 [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)


### <a name="transientattribute"></a>TransientAttribute

이 특성을 매개 변수에 적용 하며 Objective C에서 C#로 전환 하는 경우에 사용 됩니다.  다양 한 Objective-c NSObjects 전환 하는 동안 매개 변수는 개체의 관리 되는 표현으로 래핑됩니다.

런타임에서 네이티브 개체에 대 한 참조를 개체에 관리 되는 마지막 참조는 사라지고 나타나고 GC가 실행 될 때까지 대 한 참조를 유지 합니다.

이 몇 가지 경우에는 네이티브 개체에 대 한 참조를 유지 하지 않도록 하려면 C# 런타임 중요 합니다.  이 경우에 따라 기본 네이티브 코드가 매개 변수 수명 주기를 특별 한 동작을 연결 하는 경우에 발생 합니다.  예: 매개 변수에 대해 소멸자에서 정리 작업을 수행 하거나 일부 소중한 리소스를 삭제 합니다.

이 특성에는 덮어쓴 메서드에서 Objective C에 게 다시 반환 하는 경우 가능 하면 삭제 될 개체를 원하는 런타임에 알립니다.

규칙은 간단: 런타임 네이티브 개체에서 새 관리 되는 표현 만들어야 했습니다 함수 끝의 네이티브 개체에 대 한 보관 기간 수가 삭제 되 고 관리 되는 개체의 핸들 속성이 지워집니다.   즉, 관리 되는 개체에 대 한 참조를 유지 한 경우 해당 참조가 쓸모 없게 됩니다 (메서드를 호출 하는 예외를 throw).

전달 된 개체를 만들지 않은 경우 또는 이미 있는 경우 개체의 처리 중인 관리 되는 표현은 강제 dispose 수행 되지 않습니다. 


## <a name="property-attributes"></a>속성 특성

### <a name="notimplementedattribute"></a>NotImplementedAttribute

이 특성은 여기서 getter 포함 하는 속성은 기본 클래스에 도입 된 하 고 변경할 수 있는 하위 클래스 소개 setter Objective-c 요소를 지 원하는 데 사용 됩니다.

C#는이 모델을 지원 하지 않습니다, setter 및 getter, 모두 필요한 기본 클래스 및 하위 클래스를 사용할 수 있으므로 [OverrideAttribute](#OverrideAttribute)합니다.

이 특성은 속성 setter에 사용만 하 고 목표 C에서 변경할 수 있는 요소를 지 원하는 데 사용 됩니다.

예제:

```csharp
[BaseType (typeof (NSObject))]
interface MyString {
    [Export ("initWithValue:")]
    IntPtr Constructor (string value);

    [Export ("value")]
    string Value {
        get;

    [NotImplemented ("Not available on MyString, use MyMutableString to set")]
        set;
    }
}

[BaseType (typeof (MyString))]
interface MyMutableString {
    [Export ("value")]
    [Override]
    string Value { get; set; }
}
```

<a name="enum-attributes"/>

## <a name="enum-attributes"></a>Enum 특성

매핑 `NSString` enum 값에 대 한 상수는 쉽게 더 나은.NET API를 만들 수 있습니다. 해당:

* 코드 완성 기능을 표시 하 여 보다 유용 하 게 되려면 허용 **만** API;에 대 한 올바른 값
* 형식 안전성을 추가 하는 다른에 사용할 수 없습니다 `NSString` 잘못 된 컨텍스트에서 상수 및
* 코드 완성 기능 손실 없이 짧은 API 목록 보기를 만드는 일부 상수를 숨길 수 있습니다.

예제:

```csharp
enum NSRunLoopMode {

    [DefaultEnumValue]
    [Field ("NSDefaultRunLoopMode")]
    Default,

    [Field ("NSRunLoopCommonModes")]
    Common,

    [Field (null)]
    Other = 1000
}
```

위의 바인딩 정의에서 생성자 만들어집니다는 `enum` 자체도 만듭니다는 `*Extensions` enum 값 사이 두 가지 방법 변환 메서드를 포함 하는 정적 형식 및 `NSString` 상수입니다. 이 즉, 상수 남아 이러한 API의 일부가 아닌 경우에 개발자에 게 사용할 수 있습니다.

예를 들면 다음과 같습니다.

```csharp
// using the NSString constant in a different API / framework / 3rd party code
CallApiRequiringAnNSString (NSRunLoopMode.Default.GetConstant ());
```

```csharp
// converting the constants from a different API / framework / 3rd party code
var constant = CallApiReturningAnNSString ();
// back into an enum value
CallApiWithEnum (NSRunLoopModeExtensions.GetValue (constant));
```

### <a name="defaultenumvalueattribute"></a>DefaultEnumValueAttribute

데코레이팅 할 수 있습니다 **하나** 이 특성으로 열거형 값입니다. 이 열거형 값의 알려져 있지 지정 하는 경우 반환 되는 상수 됩니다.

위 예제의:

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

열거형 값이 없는 데코 레이트 된 경우는 `NotSupportedException` throw 됩니다.

### <a name="errordomainattribute"></a>ErrorDomainAttribute

오류 코드는 열거형 값으로 바인딩됩니다. 일반적으로 하는 오류 도메인 이므로 항상 쉽게 적용 하는 어떤 찾기 (또는 있는 경우).

이 특성을 사용 하 여 그 자체가 열거형와 오류 도메인을 연결할 수 있습니다.

예제:

```csharp
    [Native]
    [ErrorDomain ("AVKitErrorDomain")]
    public enum AVKitError : nint {
        None = 0,
        Unknown = -1000,
        PictureInPictureStartFailed = -1001
    }
```

확장 메서드를 호출할 수 있습니다 `GetDomain` 도메인 오류 상수 얻으려고 합니다.

### <a name="fieldattribute"></a>FieldAttribute

이 동일 `[Field]` 상수 형식 내부에 사용 되는 특성입니다. 또한 사용할 수 있습니다 열거형 내 특정 상수를 사용 하 여 값을 매핑할 수 있습니다.

A `null` 경우는 열거형 값이 반환 되도록 지정 하려면 값을 사용할 수 있습니다는 `null` `NSString` 상수를 지정 합니다.

위 예제의:

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

없는 경우 `null` 이 값이 아니라면 `ArgumentNullException` throw 됩니다.

## <a name="global-attributes"></a>전역 특성

전역 특성은 사용 하 여 적용 하거나는 `[assembly:]` 같은 특성 한정자는 `LinkWithAttribute` 수 또는 같은 어디에서 나 사용는 `Lion` 및 `Since` 특성입니다.


### <a name="linkwithattribute"></a>LinkWithAttribute

개발자가 바인딩된 라이브러리는 gcc_flags를 수동으로 구성 하는 라이브러리의 소비자를 시작 하지 않고 다시 사용 하는 데 필요한 연결 플래그 및 라이브러리에 전달 된 추가 mtouch 인수를 지정 하는 어셈블리 수준 특성입니다.

구문:

```csharp
// In properties
[Flags]
public enum LinkTarget {
    Simulator    = 1,
    ArmV6    = 2,
    ArmV7    = 4,
    Thumb    = 8,
}

[AttributeUsage(AttributeTargets.Assembly, AllowMultiple=true)]
public class LinkWithAttribute : Attribute {
    public LinkWithAttribute ();
    public LinkWithAttribute (string libraryName);
    public LinkWithAttribute (string libraryName, LinkTarget target);
    public LinkWithAttribute (string libraryName, LinkTarget target, string linkerFlags);
    public bool ForceLoad { get; set; }
    public string Frameworks { get; set; }
    public bool IsCxx { get; set;  }
    public string LibraryName { get; }
    public string LinkerFlags { get; set; }
    public LinkTarget LinkTarget { get; set; }
    public bool NeedsGccExceptionHandling { get; set; }
    public bool SmartLink { get; set; }
    public string WeakFrameworks { get; set; }
}
```

이 특성은 어셈블리 수준에서 적용 되며, 예를 들어 값은 고 [CorePlot 바인딩](https://github.com/mono/monotouch-bindings/tree/master/CorePlot) 사용:

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

사용 하는 경우는 `LinkWith` 특성을 지정 된 `libraryName` 제대로 사용에 필요한 명령줄 플래그와 관리 되지 않는 종속성 모두 포함 된 단일 DLL을 제공 하는 사용자가 결과 어셈블리에 포함 되는 Xamarin.iOS에서 라이브러리입니다.

것도 제공 하지 않도록 가능는 `libraryName`있으며이 경우는 `LinkWith` 만 추가 링커 플래그를 지정할 특성을 사용할 수 있습니다.

 ``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
 ```


#### <a name="linkwithattribute-constructors"></a>LinkWithAttribute 생성자

이러한 생성자를 사용 하 여 라이브러리와 링크와 지원 되는 대상이 지 원하는 라이브러리 및 라이브러리와 연결 하는 데 필요한 모든 선택적 라이브러리 플래그를 결과 어셈블리에 포함를 지정할 수 있습니다.

이때 LinkTarget 인수 Xamarin.iOS에 의해 유추 하 고 설정할 필요가 없습니다.

예를 들면 다음과 같습니다.

```csharp
// Specify additional linker:
[assembly: LinkWith (LinkerFlags = "-sqlite3")]

// Specify library name for the constructor:
[assembly: LinkWith ("libDemo.a");

// Specify library name, and link target for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator);

// Specify only the library name, link target and linker flags for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator, SmartLink = true, ForceLoad = true, IsCxx = true);
```


#### <a name="linkwithattributeforceload"></a>LinkWithAttribute.ForceLoad

`ForceLoad` 속성을 사용 하 여를 결정 여부는 `-force_load` 링크 플래그가 네이티브 라이브러리 연결에 사용 됩니다. 지금은이 항상 true 여야 합니다.


#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute.Frameworks

프레임 워크에서이 바인딩되는 라이브러리에 하드 요구 사항이 경우 (이외의 `Foundation` 및 `UIKit`)를 설정 해야는 `Frameworks` 속성 필요한 플랫폼 프레임 워크는 공백으로 구분 된 목록을 포함 하는 문자열을 합니다. 예를 들어 필요한 라이브러리를 바인딩하는 경우 `CoreGraphics` 및 `CoreText`, 설정한는 `Frameworks` 속성을 `"CoreGraphics CoreText"`합니다.


#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute.IsCxx

이 속성을 결과 실행 파일을 c + + 컴파일러는 C 컴파일러는 기본값 대신 사용 하 여 컴파일할 수 해야 할 경우 true로 설정 합니다. 라이브러리에 바인딩하는 c + +에서 작성 된 경우이 사용 합니다.


#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute.LibraryName

번들로 관리 되지 않는 라이브러리의 이름입니다. "포함" 확장명을 가진 파일 이며 여러 플랫폼 (예를 들어, ARM 및 x86 시뮬레이터)에 대 한 개체 코드를 포함할 수 있습니다.

이전 버전의 Xamarin.iOS 검사는 `LinkTarget` 속성 라이브러리 지원 플랫폼을 확인할 수 있지만이 이제 자동 감지 및 `LinkTarget` 속성은 무시 됩니다.


#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute.LinkerFlags

`LinkerFlags` 문자열 바인딩 만든 추가 링커 플래그 필요한 응용 프로그램으로 네이티브 라이브러리를 연결 하는 경우 지정할 수 있는 방법을 제공 합니다.

예를 들어, 네이티브 라이브러리 libxml2 및 zlib를 필요로 하는 경우 설정의 `LinkerFlags` 문자열을 `"-lxml2 -lz"`합니다.


#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute.LinkTarget

이전 버전의 Xamarin.iOS 검사는 `LinkTarget` 속성 라이브러리 지원 플랫폼을 확인할 수 있지만이 이제 자동 감지 및 `LinkTarget` 속성은 무시 됩니다.

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute.NeedsGccExceptionHandling

이 속성을 연결 하 고 라이브러리 GCC 예외 처리 라이브러리 (gcc_eh) 경우 true로 설정 합니다.


#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute.SmartLink

`SmartLink` 속성 Xamarin.iOS 결정 하도록 하려면 true로 설정 해야 하는지 여부를 `ForceLoad` 의 필요 합니다.


#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute.WeakFrameworks

`WeakFrameworks` 속성이 동일한 방식으로 작동 하는 `Frameworks` 속성 링크 타임에 점을 제외 하 고는 `-weak_framework` 지정자 각 나열 된 프레임 워크에 대 한 gcc에 전달 됩니다.

`WeakFrameworks` 사용 하면 플랫폼 프레임 워크에 대 한 약하게 링크에 대 한 응용 프로그램 및 라이브러리를 사용 하지 않는 라이브러리에 추가 기능을 추가 하려는 경우 유용에 의존 최신 않지만 사용할 수 있는 경우 선택적으로 사용할 수 있습니다. iOS의 버전입니다. 약한 연결에 대 한 자세한 내용은 Apple 설명서를 참조에 [약한 연결](http://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html)합니다.

약한 연결 하기에 적합 것 `Frameworks` 계정을 같은 `CoreBluetooth`, `CoreImage`, `GLKit`, `NewsstandKit` 및 `Twitter` iOS 5에서에서 사용할 수만 있기 때문입니다.

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) 및 LionAttribute (macOS)

사용 하면는 `Since` 시간에 특정 지점에서 도입 되 고 있는 것으로 특성을 Api 플래그입니다. 특성으로 형식 및 기본 클래스, 메서드 또는 속성 사용할 수 없는 경우에 런타임 문제를 일으킬 수 있는 메서드를 나타내는 플래그만 사용 해야 합니다.

구문:

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

일반적 하지 적용 해야 열거형, 제약 조건 또는 새 구조에는 이전 버전의 운영 체제를 사용 하 여 장치에서 실행 될 경우 런타임 오류가 발생 하지는 하는 것 처럼 합니다.

형식에 적용할 때 예:

```csharp
// Type introduced with iOS 4.2
[Since (4,2)]
[BaseType (typeof (UIPrintFormatter))]
interface UIViewPrintFormatter {
    [Export ("view")]
    UIView View { get; }
}
```

새 멤버에 적용 될 때 예:

```csharp
[BaseType (typeof (UIViewController))]
public interface UITableViewController {
    [Export ("tableView", ArgumentSemantic.Retain)]
    UITableView TableView { get; set; }

    [Since (3,2)]
    [Export ("clearsSelectionOnViewWillAppear")]
    bool ClearsSelectionOnViewWillAppear { get; set; }
```

`Lion` 특성 Lion에 도입 된 형식에 대 한 같은 방식으로 적용 됩니다. 사용 하는 이유 `Lion` 주요 OS X 출시 거의 발생 하지 않으며 운영 체제 버전 번호로 해당 코드명 보다 기억 하도록 쉽게 하는 동안 해당 iOS 자주 수정 되는 iOS에 사용 되는 보다 구체적인 버전 번호와


### <a name="adviceattribute"></a>AdviceAttribute

개발자에 게 사용 하기가 더 편리할 수 있습니다 다른 Api에 대 한 힌트를 제공 하려면이 특성을 사용 합니다.   예를 들어에서 API의 강력한 형식의 버전을 제공 하는 경우에 더 나은 API 개발자에 보내도록 약한 형식의 특성에이 특성을 사용할 수 있습니다.

이 특성의 정보는 설명서에 표시 되 고 개선 하는 방법에 대 한 사용자 제시 하는 도구를 개발할 수 있습니다.

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

만 Xamarin.iOS 5.4에 사용할 수 이상입니다.

이 특성 생성자에 게 지시 하는이 특정 라이브러리에 대 한 바인딩을 (사용 하 여 적용 `[assembly:]`) 빠른 0 복사 문자열 마샬링 형식을 사용 해야 합니다. 이 특성은 해당 명령줄 옵션을 전달 하 여 `--zero-copy` 생성기에 있습니다.

를 사용 하면 0 복사 문자열에 대 한 생성기 효과적으로 사용 동일한 C# 문자열 Objective C가 새로 생성 헤드 없이 사용 하는 문자열로 `NSString` 개체와 Objective-c 문자열을 C# 문자열에서 데이터를 복사 하지 않고 있습니다. 유일한 0 복사 문자열을 사용 하 여의 단점은 발생 "유지" 또는 "복사" 플래그를 설정 하는 문자열 속성을 래핑하기입니다 갖는지 확인 해야 하는 `DisableZeroCopy` 특성이 설정 합니다. 이 0-복사 문자열에 대 한 핸들 스택에서 할당 및 함수 반환에 유효 하지 해야 합니다.

예제:

```csharp
[ZeroCopyStrings]
[BaseType (typeof (NSObject))]
interface MyBinding {
    [Export ("name")]
    string Name { get; set; }

    [Export ("domain"), NullAllowed]
    string Domain { get; set; }

    [DisablZeroCopy]
    [Export ("someRetainedNSString")]
    string RetainedProperty { get; set; }
}

```

어셈블리 수준 특성을 적용할 수 있습니다 하 고 어셈블리의 모든 형식에 적용 됩니다.

    [assembly:ZeroCopyStrings]

## <a name="strongly-typed-dictionaries"></a>강력한 형식의 사전

Xamarin.iOS 8.0으로 래핑하는 강력한 형식의 클래스를 쉽게 만들 수 있는 기능을 사용 했습니다 `NSDictionaries`합니다.

항상 사용할 수 있었지만 동안는 [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) 수동 API와 함께 데이터 형식을 더 이제 훨씬 간단 하 게이 작업을 수행 합니다.  자세한 내용은 참조 [강력한 형식에서 표시가](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types)합니다.


### <a name="strongdictionary"></a>StrongDictionary

이 특성을 인터페이스에 적용 하는 경우 생성자는에서 파생 된 인터페이스와 같은 이름 사용 하는 클래스를 생성 [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) 하 고 강력한 형식의 인터페이스에 정의 된 각 속성은 getter 및 setter는 사전에 대 한 합니다.

인스턴스화할 수 있는 클래스는 기존 계획에서 자동으로 생성이 `NSDictionary` new 만들어지고 또는 합니다.

이 특성 사전에서 요소에 액세스 하는 데 사용 되는 키를 포함 하는 클래스의 이름을 하나의 매개를 변수로 입력 합니다.   기본적으로 특성을 사용 하 여 인터페이스의 각 속성 이름 "키" 접미사에 대 한 지정된 된 형식에서 멤버를 조회할 됩니다.

예를 들어:

```csharp
[StrongDictionary ("MyOptionKeys")]
interface MyOption {
    string Name { get; set; }
    nint    Age  { get; set; }
}

[Static]
interface MyOptionKeys {
    // In Objective-C this is "NSString *MYOptionNameKey;"
    [Field ("MYOptionNameKey")]
    NSString NameKey { get; }

    // In Objective-C this is "NSString *MYOptionAgeKey;"
    [Field ("MYOptionAgeKey")]
    NSString AgeKey { get; }
}

```

위의 경우에는 `MyOption` 클래스는 문자열 속성에 대 한 생성 `Name` 를 사용 하는 `MyOptionKeys.NameKey` 문자열을 검색 하려면 사전에 키로 합니다.   사용 하 여는 `MyOptionKeys.AgeKey` 검색할 사전에 키로는 `NSNumber` int 포함 된

다른 키를 사용 하려는 경우 예를 들어 내보내기 특성 속성에 사용할 수 있습니다.

```csharp
[StrongDictionary ("MyColoringKeys")]
interface MyColoringOptions {
    [Export ("TheName")]  // Override the default which would be NameKey
    string Name { get; set; }

    [Export ("TheAge")] // Override the default which would be AgeKey
    nint    Age  { get; set; }
}

[Static]
interface MyColoringKeys {
    // In Objective-C this is "NSString *MYColoringNameKey"
    [Field ("MYColoringNameKey")]
    NSString TheName { get; }

    // In Objective-C this is "NSString *MYColoringAgeKey"
    [Field ("MYColoringAgeKey")]
    NSString TheAge { get; }
}
```

#### <a name="strong-dictionary-types"></a>강력한 사전 형식

다음과 같은 데이터 형식이 지원 되는 `StrongDictionary` 정의:

|C# 인터페이스 형식|`NSDictionary` 저장소 유형|
|---|---|
|`bool`|`Boolean` 에 저장 된 `NSNumber`|
|열거형 값|에 저장 된 정수는 `NSNumber`|
|`int`|32 비트 정수에 저장 된 `NSNumber`|
|`uint`|32 비트 부호 없는 정수에 저장 된 `NSNumber`|
|`nint`|`NSInteger` 에 저장 된 `NSNumber`|
|`nuint`|`NSUInteger` 에 저장 된 `NSNumber`|
|`long`|64 비트 정수에 저장 된 `NSNumber`|
|`float`|로 저장 하는 32 비트 정수는 `NSNumber`|
|`double`|로 저장 하는 64 비트 정수는 `NSNumber`|
|`NSObject` 및 하위 클래스|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C# `Array` 의 `NSObject`|`NSArray`|
|C# `Array` 열거|`NSArray` 포함 된 `NSNumber` 값|
