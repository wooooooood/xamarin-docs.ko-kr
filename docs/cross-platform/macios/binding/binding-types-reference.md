---
title: 바인딩 유형 참조 가이드
description: 이 참조 가이드를 만들 때 이해 하는 데 필요한 개념 및 다양 한 특성에 설명 합니다 C# Objective-c 라이브러리 바인딩.
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
author: conceptdev
ms.author: crdun
ms.date: 03/06/2018
ms.openlocfilehash: d460bf867ce09e614be76d0a4a7ffef01420cf82
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266507"
---
# <a name="binding-types-reference-guide"></a>바인딩 유형 참조 가이드

이 문서에서는 바인딩 드라이브 API 계약 파일에 주석을 추가 하는 데 사용할 수 하 고 코드를 생성 하는 속성 목록 설명

Xamarin.iOS 및 Xamarin.Mac API 계약 작성 된 C# Objective-c 코드에 표시 되는 방식을 정의 하는 인터페이스 정의로 주로 C#합니다. 과정에 다양 한 인터페이스 선언을 plus API 계약 필요할 수 있는 몇 가지 기본 형식 정의 합니다. 소개 바인딩 형식에 대 한 부록 가이드를 참조 하세요 [Objective-c 라이브러리 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md)합니다.

## <a name="type-definitions"></a>형식 정의입니다.

구문:

```csharp
[BaseType (typeof (BTYPE))
interface MyType [: Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

모든 인터페이스에는 계약 정의에 [ `[BaseType]` ](#BaseTypeAttribute) 생성 된 개체에 대 한 기본 형식을 선언 하는 특성입니다. 위의 선언에는 `MyType` 클래스 C# 바인딩합니다 Objective C 형식에 호출 되도록 형식이 생성 될 `MyType`합니다.

Typename 후 모든 형식을 지정 하는 경우 (위 예제의 `Protocol1` 하 고 `Protocol2`) 인터페이스 상속 구문을 사용 하 여 이러한 인터페이스의 내용을 인라인 됩니다에 대 한 계약의 일부가 된 것 처럼 `MyType`합니다.
Xamarin.iOS 화면 형식 프로토콜을 채택 하는 방식으로 메서드 및 속성 형식 자체에 프로토콜에 선언 된 모든 인라인 처리 합니다.

에서는 다음 방법의 Objective C 선언 `UITextField` Xamarin.iOS 계약에서 정의 됩니다.

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

와 같이 작성 될 수는 C# API 계약:

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

구성할 수 있을 뿐만 아니라 인터페이스에 다른 특성을 적용 하 여 코드 생성의 다른 많은 측면을 제어할 수 있습니다 합니다 [ `[BaseType]` ](#BaseTypeAttribute) 특성입니다.


### <a name="generating-events"></a>이벤트를 생성합니다.

Xamarin.iOS 및 Xamarin.Mac API 디자인의 기능 중 하나는 Objective-c 대리자 클래스를 매핑 했습니다 C# 이벤트 및 콜백 합니다. 등의 속성을 할당 하 여 Objective-c로 프로그래밍 패턴을 채택 해야 할지 사용자 인스턴스별 별로 선택할 수 있습니다 `Delegate` 또는 Objective-c 런타임에서, 호출 하는 다양 한 메서드를 구현 하는 클래스의 인스턴스 선택 된 C#-스타일 이벤트와 속성을 지정 합니다.

알려주세요 Objective-c로 모델을 사용 하는 방법에 대 한 예제를 참조 하세요.

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

위의 예제에서 위해 두 개의 메서드를 덮어쓰는 했습니다에 지시 하는 부울 값을 반환 해야 하는 콜백을 하나는 스크롤 이벤트 되었고 및 두 번째는 알림을 볼 수 있습니다는 `scrollView` 으로 스크롤하고 해야 하는지 여부를 여부를 가장 합니다.

C# 모델을 사용 하면 라이브러리의를 사용 하 여 알림을 수신 대기 하도록 합니다 C# 값을 반환 하는 콜백을 후크 이벤트 구문 또는 속성 구문.

이 방법을 C# 동일한 기능을 람다를 사용 하 여 모양에 대 한 코드:

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

이벤트 (void 반환 형식을 가진) 값을 반환 하지 때문에 여러 복사본을 연결할 수 있습니다. `ShouldScrollToTop` 이벤트가 아닙니다. 대신이 형식 사용 하 여는 속성 `UIScrollViewCondition` 는 다음이 서명이 있습니다.:

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

반환을 `bool` 값이 예에서 람다 구문 있게에서 값을 반환 합니다 `MakeDecision` 함수입니다.

바인딩 생성기에서 생성 이벤트와 같이 클래스를 연결 하는 속성 `UIScrollView` 사용 하 여 해당 `UIScrollViewDelegate` (및 이러한 모델 클래스)를 호출 주석을 추가 하 여 이렇게 하 [ `[BaseType]` ](#BaseTypeAttribute) 정의 `Events` 고 `Delegates` 매개 변수 (아래 설명 참조). 주석을 추가 하는 것 외에도 합니다 [ `[BaseType]` ](#BaseTypeAttribute) 이러한 매개 변수를 사용 하 여 반드시 필요한 몇 가지 자세한 구성 요소 생성기에 알림을 보내야 합니다.

둘 이상의 매개 변수를 사용 하는 이벤트에 대 한 (Objective C에서 규칙은 대리자 클래스의 첫 번째 매개 변수는 sender 개체의 인스턴스) 생성 된 하려는 이름을 제공 해야 `EventArgs` 클래스입니다. 그러려면 합니다 [ `[EventArgs]` ](#EventArgsAttribute) 모델 클래스의 메서드 선언에서 특성입니다. 예를 들어:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

위의 선언을 생성 합니다는 `UIImagePickerImagePickedEventArgs` 에서 파생 된 클래스 `EventArgs` 매개 변수를 모두 팩 및 합니다 `UIImage` 및 `NSDictionary`합니다. 생성기가 오류를 생성 합니다.

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

다음에서 노출 하는 `UIImagePickerController` 클래스:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

모델 메서드 값을 반환 하는 다른 방식으로 바인딩됩니다. 생성 된 이름을 요구할 C# 대리자 (메서드에 대 한 서명) 및 기본 값을 사용자가 자신 구현을 제공 하지 않는 경우에 반환할 수도 있습니다. 예를 들어를 `ShouldScrollToTop` 이 정의 됩니다.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

위의 만듭니다는 `UIScrollViewCondition` 서명을 사용 하 여 대리자는 위에 표시 된 및 사용자는 구현을 제공 하지 않으면, 반환 값 true가 됩니다.

이외에 [ `[DefaultValue]` ](#DefaultValueAttribute) 특성을 사용할 수도 있습니다는 [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute) 호출 또는 합니다 지정된된매개변수의값을반환하는생성기를지시하는특성[ `[NoDefaultValue]` ](#NoDefaultValueAttribute) 지시 생성기는 기본값이 있는 매개 변수입니다.

<a name="BaseTypeAttribute" />

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

사용 된 `Name` Objective-c로 전 세계에서이 형식을 바인딩할 이름을 제어 하는 속성입니다. 일반적으로 제공 하는이 C# .NET Framework 디자인 지침 준수 되었지만 해당 규칙을 따르지 않는 Objective C에서 이름에 매핑되는 이름을 입력 합니다.

예제에서는 다음과 같은 경우 Objective-c로 매핑합니다 `NSURLConnection` 형식을 `NSUrlConnection`"URL" 대신 "Url"을 사용 하는.NET Framework 디자인 지침,:

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

지정 된 이름을 생성 된 값으로는 `[Register]` 바인딩에서 특성입니다. 하는 경우 `Name` 지정 하지 않으면 형식의 짧은 이름에 대 한 값으로는 `[Register]` 생성된 된 출력에는 특성입니다.

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType.Events 및 BaseType.Delegates

드라이브를 생성 하려면 이러한 속성을 사용 하는 C#-이벤트가 생성된 된 클래스의 스타일 지정 합니다. 링크는 Objective-c 대리자 클래스를 사용 하 여 지정 된 클래스에 사용 됩니다. 클래스는 알림 및 이벤트에 대리자 클래스를 사용 하는 많은 경우 발생 합니다. 예를 들어 한 `BarcodeScanner` 과 함께 사용 해야 `BardodeScannerDelegate` 클래스입니다. 합니다 `BarcodeScanner` 클래스는 일반적으로 것이 `Delegate` 지정 하는 경우가 인스턴스의 속성 `BarcodeScannerDelegate` while이 작동 하려면 사용자에 게 노출 하려는 C#-스타일 이벤트 인터페이스와 같은 고 이러한 경우에는 를사용할수있습니다`Events` 하 고 `Delegates` 의 속성을 [ `[BaseType]` ](#BaseTypeAttribute) 특성입니다.

이러한 속성과 동일한 요소 수 있어야 합니다.는 항상 함께 설정 및 동기화를 유지 합니다. 합니다 `Delegates` 래핑할 하려는 각-약한 대리자에 대 한 하나의 문자열을 포함 하는 배열 및 `Events` 배열에 포함 된 연결 하려는 각 형식에 대 한 형식입니다.

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

이 클래스의 새 인스턴스를 만들 때이 특성을 적용 하면 해당 개체의 인스턴스를 유지할지 주위까지 참조 하는 메서드는 `KeepRefUntil` 를 호출 했습니다. 코드를 사용 하는 개체에 대 한 참조를 유지 하려면 사용자를 원하지 않는 경우 Api의 사용 편의성을 개선 하는 데 유용 합니다. 이 속성의 값은 메서드 이름을 `Delegate` 클래스를 함께에서 사용 해야 합니다는 `Events` 및 `Delegates` 속성도 있습니다.

다음 예제에서 사용 하는 방법을 보여 줍니다. `UIActionSheet` Xamarin.iOS에서:

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

이 특성은 인터페이스 정의에 적용 되 면 생성기 기본 생성자를 생성 하는 형태 수 없게 됩니다.

개체 클래스의 다른 생성자 중 하나를 사용 하 여 초기화를 해야 하는 경우이 특성을 사용 합니다.


### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

인터페이스 정의에이 특성이 적용 되 면 기본 생성자를 private로 플래그는 것입니다. 즉, 확장 파일에서이 클래스의 개체를 내부적으로 인스턴스화할 여전히 있습니다 있지만 방금 옵션이 해당 클래스의 사용자가 액세스할 수 있습니다.

<a name="CategoryAttribute" />

### <a name="categoryattribute"></a>CategoryAttribute

Objective C 범주를 바인딩할로 노출 하는 형식 정의에이 특성을 사용 하 여 C# 기능을 노출 하는 Objective-c 방식을 반영 하는 확장 메서드입니다.

범주는 클래스에서 사용 가능한 메서드와 속성 집합을 확장 하는 데 사용 하는 Objective-c 메커니즘입니다.   실제로 사용할 하거나 기본 클래스의 기능을 확장 하 (예를 들어 `NSObject`) 특정 프레임 워크에 연결 되는 경우 (예를 들어 `UIKit`), 해당 메서드를 사용할 수 있지만 새 프레임 워크에 연결 된 경우에 수행 합니다.   경우에 따라 다른 기능으로는 클래스의 기능을 구성 하려면 사용 됩니다.   이들은 비슷하며,이를 C# 확장 메서드.

이 범주는 모양을 주기-c:는

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

위의 예제에서 액세스할 수에 라이브러리의 인스턴스를 확장 하는 것입니다 `UIView` 메서드를 사용 하 여 `makeBackgroundRed`입니다.

바인딩하려는 이러한 경우 사용할 수 있습니다 합니다 [ `[Category]` ](#CategoryAttribute) 인터페이스 정의에 특성입니다.   사용 하는 경우는 [ `[Category]` ](#CategoryAttribute) 특성의 의미를 [ `[BaseType]` ](#BaseTypeAttribute) 특성이 확장 형식으로 확장 하는 기본 클래스를 지정 하는 데 사용 되 고에서 변경 합니다.

에서는 다음 방법을 `UIView` 확장은 바인딩된 및 변환 C# 확장 메서드:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

위의 만듭니다는 `MyUIViewExtension` 포함 하는 클래스는 `MakeBackgroundRed` 확장 메서드.   즉, 이제를 호출할 수 있음을 `MakeBackgroundRed` 에서 `UIView` 목표 c 얻게 동일한 기능을 제공 하는 하위 클래스

일부 경우에서 찾을 수 있습니다 **정적** 다음 예와에서 같이 범주 내에서 멤버:

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

이로 인해 프로그램 **잘못** 범주 C# 인터페이스 정의:

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

이 올바르지 않습니다. 때문에 사용 하는 `BoolMethod` 확장 해야 인스턴스의 `FooObject` 는 ObjC 바인딩 하지만 **정적** 확장 때문 방식의 부작용이 C# 확장 메서드는 구현 .

위의 정의 사용 하는 유일한 방법은 다음과 같습니다. 다음 보기 힘든 코드

```csharp
(null as FooObject).BoolMethod (range);
```

이 문제를 방지 하기 위한 권장 사항을 인라인 하는 것을 `BoolMethod` 내에 정의 합니다 `FooObject` 인터페이스 정의 자체, 이렇게 하면 것 처럼이 확장을 호출 하 `FooObject.BoolMethod (range)`.

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

에서는 경고가 표시 됩니다 (BI1117) 귀하를 찾을 때마다를 [ `[Static]` ](#StaticAttribute) 내에서 멤버를 [ `[Category]` ](#CategoryAttribute) 정의 합니다. 실제로 하도록 하려는 경우 [ `[Static]` ](#StaticAttribute) 내에서 멤버에 [ `[Category]` ](#CategoryAttribute) 사용 하 여 경고를 억제 수 정의 `[Category (allowStaticMembers: true)]` 나멤버를데코레이팅하여또는[ `[Category]` ](#CategoryAttribute) 인터페이스와 정의 [ `[Internal]` ](#InternalAttribute)합니다.

<a name="StaticAttribute_Class" />

### <a name="staticattribute"></a>StaticAttribute

정적 클래스에서 파생 되지 않는 것이 특성은 클래스에 적용 될 때만 생성 됩니다 `NSObject`이므로 [ `[BaseType]` ](#BaseTypeAttribute) 특성이 무시 됩니다. 정적 클래스는 C 공용 변수를 노출 하려면를 호스트 하는 데 사용 됩니다.

예를 들어:

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

생성 된 C# 다음 API 사용 하 여 클래스:

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```

## <a name="protocolmodel-definitions"></a>프로토콜/모델 정의

모델은 일반적으로 프로토콜 구현에서 사용 됩니다.
런타임만에 등록 하는 Objective-c로 실제로 덮어쓴 메서드는 다릅니다.
그렇지 않으면 메서드는 등록 되지 않았습니다.

때 일반적으로 즉 하위도 플래그가 지정 된 클래스는 `ModelAttribute`, 기본 메서드를 호출 해야 하면 합니다.   해당 메서드를 호출 하면 예외가 throw 됩니다, 그리고 전체 동작을 재정의 하는 방법에 대 한 서브 클래스에서 구현 하는 작업이 있습니다.

<a name="AbstractAttribute" />

### <a name="abstractattribute"></a>AbstractAttribute

기본적으로 프로토콜의 일부 멤버는 필수입니다. 이렇게 하면 사용자의 서브 클래스를 만들 수는 `Model` 단순히 클래스에서 파생 하 여 개체 C# 관심 메서드만 재정의 합니다. 경우에 따라 Objective-c 계약에 필요한 사용자는이 메서드의 구현을 (함께 플래그 지정 됩니다 것를 `@required` Objective C에서 지시문). 이러한 경우에 사용 하 여 이러한 메서드를 플래그 지정 해야 합니다 `[Abstract]` 특성입니다.

`[Abstract]` 특성 메서드 또는 속성에 적용할 수 있습니다 및 생성자가 클래스는 추상 클래스를 추상으로 생성 된 멤버를 플래그 합니다.

다음 Xamarin.iOS에서 수행 됩니다.

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

사용자는 모델 개체에서이 특정 메서드에 대 한 메서드를 제공 하지 않는 경우 모델 메서드에서 반환할 기본값 지정

구문:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

에 대 한 다음과 같은 허수 대리자 클래스의 예를 들어를 `Camera` 클래스를 제공 하 고는 `ShouldUploadToServer` 에서 속성으로 노출할 것입니다는 `Camera` 클래스. 경우 사용자는 `Camera` 클래스는 명시적으로 설정 하지 않습니다는 람다 true 또는 false 응답할 수 있는 값을 반환 하는 기본 값이 경우 false가 됩니다에 지정한 값을 `DefaultValue` 특성:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

허수 클래스에서 처리기를 설정 하는 사용자를이 값이 무시 됩니다.

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

참고: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute)하십시오 [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute)합니다.

<a name="DefaultValueFromArgumentAttribute" />

### <a name="defaultvaluefromargumentattribute"></a>DefaultValueFromArgumentAttribute

구문:

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

모델 클래스의 값을 반환 하는 메서드에 제공 하는 경우이 특성에는 사용자 자신의 메서드 또는 람다를 제공 하지 않은 경우 지정된 된 매개 변수의 값을 반환 하는 생성기를 지시 합니다.

예제:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

위의 경우에 사용자를 `NSAnimation` 클래스 중 하나를 사용 하도록 선택한는 C# 이벤트/속성을 설정 하지 않은 `NSAnimation.ComputeAnimationCurve` 메서드 또는 람다, 반환 값에 게 진행률 매개 변수에 전달 된 값입니다.

참고: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute), [`[DefaultValue]`](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>IgnoredInDelegateAttribute

경우에 따라는이 특성을 추가 하기를 사용 하 여 데코 레이트 된 모든 메서드의 생성 되지 않도록 하려면 생성기는 지시 하므로 모델 클래스에서 속성을 호스트 하는 클래스로 대리자 또는 이벤트를 노출할 것이 좋습니다.

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

이 특성 이름의 대리자 시그니처를 사용 하도록 설정 하는 값을 반환 하는 모델 메서드에서 사용 됩니다.

예제:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

위의 정의 사용 하 여 생성기를 다음 공용 선언을 생성 합니다.

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>DelegateApiNameAttribute

이 특성은 호스트 클래스에서 생성 된 속성의 이름을 변경 하는 생성기를 허용 하는 데 사용 됩니다. 것이 유용한 경우가 FooDelegate 클래스 메서드의 이름을 대리자 클래스에 적합 하지만 속성으로 호스트 클래스에서 홀수 보입니다.

또한이 매우 유용한와 필요한 경우 두 개 또는 FooDelegate 클래스에가 하는 대로 이름이 지정 하는 것이 오버 로드 메서드가 유지 하지만 더 나은 이름이 지정 된 호스트 클래스에서이 노출 하려면.

예제:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

위의 정의 사용 하 여 생성기 호스트 클래스에 다음 공용 선언을 생성 합니다.

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

<a name="EventArgsAttribute" />

### <a name="eventargsattribute"></a>EventArgsAttribute

둘 이상의 매개 변수를 사용 하는 이벤트에 대 한 (Objective C에서 규칙은 대리자 클래스의 첫 번째 매개 변수는 sender 개체의 인스턴스)는 생성 된 EventArgs 클래스에 대 한 원하는 이름을 지정 해야 합니다. 사용 하 여 이렇게 합니다 `[EventArgs]` 의 메서드 선언에서 특성에 `Model` 클래스입니다.

예를 들어:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

위의 선언은 생성을 `UIImagePickerImagePickedEventArgs` EventArgs에서 파생 되는 매개 변수를 모두 팩 클래스를 `UIImage` 및 `NSDictionary`합니다. 생성기가 오류를 생성 합니다.

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

다음에서 노출 하는 `UIImagePickerController` 클래스:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```


### <a name="eventnameattribute"></a>EventNameAttribute

이 특성은 이벤트 또는 클래스에서 생성 하는 속성의 이름을 변경 하는 생성기를 허용 하도록 사용 됩니다. 것이 유용한 경우가 모델 클래스 메서드의 이름을 모델 클래스에 적합 하지만 이벤트 또는 속성으로 홀수 원래 클래스에 표시 됩니다.

예를 들어 합니다 `UIWebView` 에서 다음 비트를 사용 하는 `UIWebViewDelegate`:

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

위의 노출 `LoadingFinished` 에서 메서드로 합니다 `UIWebViewDelegate`, 있지만 `LoadFinished` 에서 최대 후크 할 이벤트로는 `UIWebView`:

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```

<a name="ModelAttribute" />

### <a name="modelattribute"></a>ModelAttribute

적용 하는 경우는 `[Model]` 특성 런타임 API 계약의 형식 정의에 노출할 호출 클래스에 메서드를 사용자가 클래스의 메서드를 덮어쓸 경우 특별 한 코드를 생성 합니다. 이 특성은 일반적으로 Objective-c 대리자 클래스를 래핑하는 모든 Api에 적용 됩니다.

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

모델에 대 한 메서드가 기본 반환 값을 제공 하지 않음을 지정 합니다.

응답 하 여 Objective-c 런타임에서 작동 `false` Objective C 런타임 요청에 지정된 된 선택기는이 클래스에서 구현 하는 경우를 결정 합니다.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

참고: [ `[DefaultValue]` ](#DefaultValueAttribute), [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)  

<a name="ProtocolAttribute" />

## <a name="protocols"></a>프로토콜

Objective-c 프로토콜 개념에 실제로 존재 하지 않는 C#입니다. 프로토콜은 C# 인터페이스 있지만 한다는 점에서 차이가 모든 메서드 및 프로토콜에 선언 된 속성을 채택 하는 클래스에서 구현 되어야 합니다. 대신 메서드 및 속성의 일부는 선택 사항입니다.

모델 클래스로 일부 프로토콜은 일반적으로 사용, 이러한 사용 하 여 바인딩된 해야 합니다 [ `[Model]` ](#ModelAttribute) 특성입니다.

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

Xamarin.iOS 7.0 새롭고 향상 된 프로토콜 바인딩 기능부터 통합 되어 있습니다.  포함 된 모든 정의 `[Protocol]` 특성 프로토콜을 사용 한다고 가정 하는 방법은 크게 개선 하는 세 가지 지원 클래스를 실제로 생성 됩니다.

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

합니다 **클래스 구현은** 의 개별 메서드를 재정의 하 고 전체 형식 안전성을 가져올 수 있는 완전 한 추상 클래스를 제공 합니다. 하지만로 인해 C# 다중 상속을 지원 하지 않는, 시나리오가 다른 기본 클래스를 필요로 하지만 인터페이스를 구현 수 있습니다.

이 경우 생성 된 **인터페이스 정의** 제공 됩니다.  이 프로토콜에서 필요한 모든 메서드를 사용 하는 인터페이스입니다.  개발자를 단순히 인터페이스를 구현 하 여 프로토콜을 구현 하려는 있습니다.  런타임은 프로토콜 도입으로 형식을 자동으로 등록 됩니다.

알림 인터페이스만 필요한 메서드를 나열 하는 선택적 메서드를 노출 합니다.   이 프로토콜을 채택 하는 클래스 필요한 메서드를 확인 하는 전체 형식 받습니다 있지만를 약한 형식 지정 (수동으로 내보내기 특성을 사용 하 여 및 서명 일치)에 의존 해야 하는 선택적 프로토콜 메서드에 대 한 의미 합니다.

바인딩 도구는 편리 하 게 프로토콜을 사용 하는 API를 사용 하려면, 모든 선택적 메서드를 노출 하는 확장 메서드 클래스도 생성 됩니다.   이 API를 사용 중인으로 수 프로토콜 메서드를 모두 있는 것으로 처리할 것을 의미 합니다.

API에서 프로토콜 정의 사용 하려는 경우에 API 정의에서 기본 빈 인터페이스를 작성 해야 합니다.   MyProtocol API에서 사용 하려는 경우이 작업을 수행 해야 합니다.

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

위의 필요 하기 때문에 바인딩 시간에는 `IMyProtocol` 존재 하지 않습니다, 즉 빈 인터페이스를 제공 해야 하는 이유입니다.

### <a name="adopting-protocol-generated-interfaces"></a>프로토콜에서 생성 된 인터페이스를 채택합니다.

구현할 때마다 다음과 같은 프로토콜에 대해 생성 된 인터페이스 중 하나:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

인터페이스 메서드의 구현은 자동으로 내보내지는 적절 한 이름을 사용 하 여이에 해당 되므로:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

암시적 또는 명시적으로 인터페이스를 구현 하는 경우에 중요 하지 않습니다.

### <a name="protocol-inlining"></a>인라인 프로토콜

프로토콜을 채택으로 선언 된 기존 Objective-c로 형식에 바인딩하는 동안 하려는 인라인 프로토콜을 직접. 이를 위해서는 단순히 선언 프로토콜 없이 인터페이스로 [ `[BaseType]` ](#BaseTypeAttribute) 특성 및 사용자 인터페이스에 대 한 기본 인터페이스 목록에서 프로토콜을 나열 합니다.

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

이 섹션의 특성은 형식의 개별 멤버에 적용 됩니다: 속성 및 메서드 선언 합니다.


### <a name="alignattribute"></a>AlignAttribute

속성 반환 형식에 대 한 맞춤 값을 지정 하는 데 사용 합니다. 특정 속성에는 특정 경계에 맞춰야 하는 주소에 대 한 포인터 사용 (예를 들어 일부를 사용 하 여 이런 Xamarin.iOS에서 `GLKBaseEffect` 16 바이트 수 있어야 하는 속성 정렬). 이 속성을 사용 하 여 getter를 데코 레이트 하 고 맞춤 값을 사용할 수 있습니다. 이 일반적으로 사용 되는 `OpenTK.Vector4` 및 `OpenTK.Matrix4` Objective C Api와 통합 하는 경우 형식입니다.

예제:

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```


### <a name="appearanceattribute"></a>AppearanceAttribute

`[Appearance]` 특성 모양을 manager 도입 있는 iOS 5로 제한 됩니다.

`[Appearance]` 메서드 또는 속성에 참여 하는 특성을 적용할 수는 `UIAppearance` 프레임 워크입니다. 이 특성은 메서드 또는 클래스에서 속성에 적용할 때이 클래스의 모든 인스턴스 또는 특정 조건과 일치 하는 경우 스타일을 지정 하는 데 사용 되는 강력한 형식의 모양 클래스를 만드는 바인딩 생성기를 안내 합니다.

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

위의 UIToolbar에 다음 코드를 생성 됩니다.

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

사용 합니다 `[AutoReleaseAttribute]` 메서드 및 속성에서 메서드를 메서드 호출을 래핑하려면는 `NSAutoReleasePool`합니다.

Objective-c에서는 기본값에 추가 되는 값을 반환 하는 방법도 `NSAutoReleasePool`합니다. 기본적으로 이러한 이동 스레드 `NSAutoReleasePool`, Xamarin.iOS도 관리 되는 개체와 개체에 대 한 참조를 유지, 때문에 대 한 추가 참조를 유지 하려는 없습니다 있지만 `NSAutoReleasePool` 스레드까지만 드레이닝 가져오기는 다음 스레드에서 반환 컨트롤 또는 기본 루프를 다시 이동 합니다.

이 특성은 예를 들어 많은 속성에 적용 됩니다 (예를 들어 `UIImage.FromFile`)을 기본값으로 추가 된 개체를 반환 하는 `NSAutoReleasePool`합니다. 이 특성이 없으면 이미지는 보존할으로 기본 루프에 스레드 컨트롤을 반환 하지 않았습니다. 스레드 Uf 항상 유지 되는 배경 다운로더의 일종 되었으며 작업을 기다리는 이미지는 공개 되지 않는 수 있습니다.

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

`[ForcedTypeAttribute]` 반환 되는 관리 되지 않는 개체 바인딩 정의에 설명 된 형식과 일치 하지 않는 경우에 관리 되는 형식 만들기를 적용 하는 데 사용 됩니다.

헤더에 설명 된 형식의 네이티브 메서드의 반환된 형식이 일치 하지 않는 경우이 유용한, 예를 들어 Objective-c 정의에서 다음을 수행할 `NSURLSession`:

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

반환 하는 것이 명확 하 게 상태를 `NSURLSessionDownloadTask` 인스턴스, 하지만 아직 것 **반환** 는 `NSURLSessionTask`, 슈퍼 클래스는 따라서 하지 변환할 및 `NSURLSessionDownloadTask`. 형식이 안전한 컨텍스트에서 것 이므로 `InvalidCastException` 현상이 발생 합니다.

헤더 파일에 대 한 설명을 준수 하 고 방지 하기 위해 합니다 `InvalidCastException`, `[ForcedTypeAttribute]` 사용 됩니다.

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

합니다 `[ForcedTypeAttribute]` 라는 부울 값을 받기도 `Owns` 즉 `false` 기본적으로 `[ForcedType (owns: true)]`입니다. 수행 하려면 매개 변수는 소유 하는 [소유권 정책을](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html) 에 대 한 **Core Foundation** 개체입니다.

`[ForcedTypeAttribute]` 매개 변수, 속성 및 반환 값에만 유효 합니다.

<a name="BindAsAttribute" />

### <a name="bindasattribute"></a>BindAsAttribute

`[BindAsAttribute]` 바인딩할 수 있습니다 `NSNumber`, `NSValue` 및 `NSString`(열거형)를 더 정확 하 게 C# 형식입니다. 특성을 더 유용 하 고 정확 하 게 만드는 데 사용할 수는 네이티브 API 통한.NET API.

(반환 값)에 대 한 메서드, 매개 변수 및 속성을 데코 레이트 `BindAs`합니다. 단, 멤버는 **안** 내를 `[Protocol]` 또는 [ `[Model]` ](#ModelAttribute) 인터페이스입니다.

예를 들어:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

출력 됩니다.

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

하면 내부적으로 `bool?`  <->  `NSNumber` 하 고 `CGRect`  <->  `NSValue` 변환 합니다.

현재 지원 되는 캡슐화 형식은 다음과 같습니다.

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

다음 C# 에서 캡슐화를 지 원하는 데이터 형식을 `NSValue`:

* CGAffineTransform
* NSRange
* CGVector
* SCNMatrix4
* CLLocationCoordinate2D
* SCNVector3
* SCNVector4
* CGPoint / PointF
* Cgrect는 크기 / RectangleF
* CGSize / SizeF
* UIEdgeInsets
* UIOffset
* MKCoordinateSpan
* CMTimeRange
* CMTime
* CMTimeMapping
* CATransform3D

#### <a name="nsnumber"></a>NSNumber

다음 C# 에서 캡슐화를 지 원하는 데이터 형식을 `NSNumber`:

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

[`[BindAs]`](#BindAsAttribute) 사용 하 여 conjuntion 작동 [NSString 상수에서 지 원하는 열거형](#enum-attributes) 예를 들어 향상 된.NET API를 만들 수 있습니다:

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

출력 됩니다.

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

작업을 처리 합니다 `enum`  <->  `NSString` 제공 된 열거형 형식을 경우에 변환 [ `[BindAs]` ](#BindAsAttribute) 은 [NSString 상수를 기준으로 지원](#enum-attributes)합니다.

#### <a name="arrays"></a>배열

[`[BindAs]`](#BindAsAttribute) 또한 배열 지원의 지원 되는 형식, 예를 들어 다음 API 정의 할 수 있습니다.

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

출력 됩니다.

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

`rects` 매개 변수를 캡슐화 됩니다는 `NSArray` 포함 하는 `NSValue` 각각에 대 한 `CGRect` 배열을 반환 하면 및 `CAScroll?` 는 만든 반환 된 값을 사용 하 여 `NSArray` 포함 된 `NSStrings`합니다.

<a name="BindAttribute" />

### <a name="bindattribute"></a>BindAttribute

`[Bind]` 특성이 두 메서드 또는 속성 선언 및 다른 개별 getter 또는 setter 속성에 적용할 때 적용할 때 하나를 사용 합니다.

메서드 또는 속성의 효과에 사용 되는 경우는 `[Bind]` 특성이 지정된 된 선택기를 호출 하는 메서드를 생성 하는 것입니다. 결과 생성 된 메서드가으로 데코레이팅되지 않은 하지만 합니다 [ `[Export]` ](#ExportAttribute) 특성, 메서드 재정의에서 하지 참여할 수 있음을 의미 합니다. 와 함께에서 일반적으로이 `[Target]` Objective-c 확장 메서드를 구현 하는 것에 대 한 특성입니다.

예를 들어:

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

Getter 또는 setter를 사용 하는 경우는 `[Bind]` 특성 속성의 getter 및 setter Objective-c 선택기 이름을 생성할 때 코드 생성기에서 유추 된 기본값을 변경 하는 데 사용 됩니다. 이름 가진 속성의 플래그를 지정 하는 경우 기본적으로 `fooBar`, 생성기는 생성을 `fooBar` getter에 대 한 내보내기 및 `setFooBar:` setter. 몇 가지 경우에 Objective-c 따르지이 규칙에서 일반적으로 이러한 이름을 변경 합니다 getter 되도록 `isFooBar`합니다.
이 생성기를 알리기 위해이 특성을 사용 합니다.

예를 들어:

```csharp
// Default behavior
[Export ("active")]
bool Active { get; set; }

// Custom naming with the Bind attribute
[Export ("visible")]
bool Visible { [Bind ("isVisible")] get; set; }
```

<a name="AsyncAttribute" />

### <a name="asyncattribute"></a>AsyncAttribute

만 Xamarin.iOS 6.3에서 사용할 수 있는 이상.

완료 처리기가 마지막 인수로 사용 하는 메서드에이 특성을 적용할 수 있습니다.

사용할 수는 `[Async]` 마지막 인수는 콜백 메서드 특성입니다.  이 메서드를 적용 하면 바인딩 생성기 접미사를 사용 하 여 해당 메서드의 버전을 생성 합니다 `Async`합니다.  매개 변수를 사용 하는 콜백, 하는 경우 반환 됩니다는 `Task`, 콜백 매개 변수, 결과 됩니다는 `Task<T>`합니다.

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

다음은이 비동기 메서드를 생성 합니다.

```csharp
Task LoadFileAsync (string file);
```

콜백을 여러 매개 변수를 사용 하는 경우 설정 해야 합니다 `ResultType` 또는 `ResultTypeName` 모든 속성을 포함 하는 생성 된 형식의 원하는 이름을 지정 합니다.

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

다음에이 비동기 메서드에서 발생 합니다. 여기서 `FileLoading` 둘 다에 액세스 하는 속성을 포함 `files` 고 `byteCount`:

```csharp
Task<FileLoading> LoadFile (string file);
```

콜백 마지막 매개 변수인 경우는 `NSError`, 그런 다음 생성 된 `Async` 메서드는 값이 null 인 경우 및 생성 된 비동기 메서드는 작업 예외를 설정 하는 경우 확인 합니다.

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

위의에서는 다음 비동기 메서드로를 생성합니다.

```csharp
Task<string> UploadAsync (string file);
```

오류 시 결과 작업 해야로 예외를 `NSErrorException` 결과 래핑하는 `NSError`합니다.

#### <a name="asyncattributeresulttype"></a>AsyncAttribute.ResultType

이 속성을 사용 하 여 반환 값을 지정 `Task` 개체입니다.   이 매개 변수는 기존 형식, core api 정의 중 하나에서 정의 해야 합니다.

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute.ResultTypeName

이 속성을 사용 하 여 반환 값을 지정 `Task` 개체입니다.   이 매개 변수는 원하는 형식 이름의 이름, 생성기는 일련의 속성이 콜백을 사용 하는 각 매개 변수에 대해 하나씩 생성 됩니다.

#### <a name="asyncattributemethodname"></a>AsyncAttribute.MethodName

생성 된 비동기 메서드의 이름을 사용자 지정 하려면이 속성을 사용 합니다.   기본값은 메서드 이름을 사용 하 여 텍스트 "Async"를 추가,이 기본값을 변경 하는 데 사용할 수 있습니다.

### <a name="disablezerocopyattribute"></a>DisableZeroCopyAttribute

이 특성 문자열 매개 변수 또는 문자열 속성에 적용 되 고 대신에서 새 NSString 인스턴스 만들기 및이 매개 변수에 대 한 0-복사 문자열 마샬링을 사용 하지 않는 하는 코드 생성기는 C# 문자열입니다.
생성자가 복사 0 문자열 마샬링 중 하나를 사용 하 여 사용 하도록 지시 하는 경우 문자열에만이 특성은 필요 합니다 `--zero-copy` 명령줄 옵션 또는 어셈블리 수준 특성을 설정 `ZeroCopyStringsAttribute`합니다.

이것이 필요한 경우 속성 되도록 Objective-c에서 선언 된 위치에 `retain` 또는 `assign` 속성 대신를 `copy` 속성. 이 일반적으로 잘못 "최적화 된" 개발자가 타사 라이브러리에서 발생 합니다. 일반적으로 `retain` 또는 `assign` `NSString` 속성이 이후 잘못 된 `NSMutableString` 또는 사용자에서 파생 된 클래스 `NSString` 약간 주요 라이브러리 코드의 지식 없이 문자열의 내용을 변경할 수는 응용 프로그램입니다. 일반적으로이 섣부른 최적화로 인해 발생합니다.

목표를 c: 다음은 이러한 두 속성

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```


### <a name="disposeattribute"></a>DisposeAttribute

적용 하는 경우는 `[DisposeAttribute]` 클래스에 추가할 코드 조각을 제공 하는 `Dispose()` 클래스의 메서드를 구현 합니다.

하므로 `Dispose` 메서드를 자동으로 생성 됩니다는 `bmac-native` 및 `btouch-native` 사용 해야 하는 도구를 `[Dispose]` 특성으로 생성 된 코드가 삽입을 `Dispose` 메서드 구현.

예를 들어:

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```

<a name="ExportAttribute" />

### <a name="exportattribute"></a>ExportAttribute

`[Export]` 특성 메서드 또는 Objective-c 런타임에 노출 될 속성 플래그를 지정 하는 데 사용 됩니다. 이 특성은 바인딩 도구 및 실제 Xamarin.iOS 및 Xamarin.Mac 런타임 간에 공유 됩니다. 메서드에 대 한 매개 변수 전달 되는 생성 된 코드를 정확 하 게 속성, getter 및 setter 내보내기는 기본 선언에 따라 생성 됩니다 (섹션을 참조 합니다 [ `[BindAttribute]` ](#BindAttribute) 변경 하는 방법에 대 한 내용은 바인딩 도구 동작)입니다.

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

합니다 [선택기](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html) 기본 Objective-c 메서드 또는 바인딩되는 속성의 이름을 나타냅니다.

#### <a name="exportattributeargumentsemantic"></a>ExportAttribute.ArgumentSemantic

<a name="FieldAttribute" />

### <a name="fieldattribute"></a>FieldAttribute

이 특성 C 전역 변수를 요청 시 로드 되 고 노출 하는 필드로 노출 하는 데 사용 됩니다 C# 코드입니다. 일반적으로이 하는 데 필요한 C 또는 Objective-c로 정의 된 및 일부 Api를 사용 하거나 토큰 수 있고 해당 값은 불투명로 사용 해야 하는 상수 값-사용자 코드 하는 것입니다.

구문:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

`symbolName` C 기호를 사용 하 여 연결 됩니다. 기본적으로이 로드 하는 형식이 정의 되어 있는 이름이 네임 스페이스에서 유추 되는 라이브러리에서 됩니다. 이 없는 경우 기호 조회 되는 라이브러리를 전달 해야 합니다 `libraryName` 매개 변수입니다. 사용 하 여 정적 라이브러리에 연결 하는 경우 `__Internal` 으로 `libraryName` 매개 변수입니다.

생성 된 속성은 정적 항상.

필드 특성을 사용 하 여 플래그가 지정 된 속성은 다음 형식의 수 있습니다.

* `NSString`
* `NSArray`
* `nint` / `int` / `long`
* `nuint` / `uint` / `ulong`
* `nfloat` / `float`
* `double`
* `CGSize`
* `System.IntPtr`
* 열거형

Setter는 지원 되지 않습니다 [NSString 상수에서 지 원하는 열거형](#enum-attributes)를 바인딩할 수 있습니다 수동으로 필요한 경우 있지만.

예제:

```csharp
[Static]
interface CameraEffects {
     [Field ("kCameraEffectsZoomFactorKey", "CameraLibrary")]
     NSString ZoomFactorKey { get; }
}
```

<a name="InternalAttribute" />

### <a name="internalattribute"></a>InternalAttribute

`[Internal]` 특성을 메서드 또는 속성에 적용할 수 있고 효과를 사용 하 여 생성 된 코드에 플래그를 지정 합니다 `internal` C# 생성된 된 어셈블리에 코드를 코드에만 액세스할 수 있도록 하는 키워드입니다. 이 일반적으로 데 너무 낮은 수준의 Api을 숨기고 시 또는 생성기에서 지원 되지 않는 Api에 대 한 개선 하려는 최적이 아닌 공용 API를 제공 하거나 일부 수 작업 코딩 해야 합니다.

바인딩을 디자인할 때 일반적으로이 특성을 사용 하 여 속성 또는 메서드를 숨길 하는 메서드 또는 속성을 선택한 다음 다른 이름을 입력에 C# 보완 지원 파일을 노출 하는 강력한 형식의 래퍼를 추가 하는 것은 기본 기능입니다.

예를 들어:

```csharp
[Internal]
[Export ("setValue:forKey:")]
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

그런 다음 지원 파일에서 다음과 같은 코드가 있을 수 있습니다.

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

<a name="IsThreadStaticAttribute" />

### <a name="isthreadstaticattribute"></a>IsThreadStaticAttribute

이 특성에는.NET을 사용 하 여 주석을 추가할 속성의 지원 필드가 플래그 `[ThreadStatic]` 특성입니다. 이 필드는 스레드 정적 변수는 경우에 유용 합니다.

### <a name="marshalnativeexceptions-xamarinios-606"></a>MarshalNativeExceptions (Xamarin.iOS 6.0.6)

이 특성을 메서드 지원 네이티브 (Objective-c) 예외를 확인 합니다.
호출 하는 대신 `objc_msgSend` 를 직접 호출 안내를 사용자 지정 trampoline ObjectiveC 예외를 catch 하 고 관리 되는 예외를 마샬링합니다.

현재 일부만 `objc_msgSend` 서명을 지 (찾아볼 수 서명을 누락 monotouch_ 실패 하는 바인딩을 사용 하는 앱의 네이티브 연결 하는 경우 지원 되지 않습니다 하는 경우 *_objc_msgSend* 기호), 자세히 수 있지만 요청에 추가 합니다.


### <a name="newattribute"></a>NewAttribute

이 특성이 메서드에 적용 되 고 속성 생성기가을 생성 합니다 `new` 선언 앞에 키워드입니다.

기본 클래스에 이미 존재 하는 하위 클래스에서 동일한 메서드 또는 속성 이름의 도입할 때 컴파일러 경고 방지를 위해 사용 됩니다.

<a name="NotificationAttribute" />

### <a name="notificationattribute"></a>NotificationAttribute

생성자 생성 한 강력한 도우미 Notifications 클래스를 할 필드에이 특성을 적용할 수 있습니다.

이 특성을 하지 않는 페이로드를 전달 하는 알림에 대 한 인수 없이 사용할 수 있습니다 하거나 지정할 수 있습니다는 `System.Type` "EventArgs"로 끝나는 이름의 일반적으로 API 정의에서 다른 인터페이스를 참조 하는 합니다. 생성기 바뀝니다 인터페이스 클래스를 서브클래싱하는 `EventArgs` 여기에 나열 된 속성을 모두 포함 됩니다. [ `[Export]` ](#ExportAttribute) 특성을 사용 해야는 `EventArgs` 클래스 값을 인출 Objective-c로 사전 조회 하는 데 키의 이름을 표시 합니다.

예를 들어:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

위의 코드는 중첩 된 클래스를 생성 하는 `MyClass.Notifications` 다음 메서드를 사용 하 여:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

사용자 코드의 다음 쉽게 알림을 구독할 수에 게시 합니다 [NSDefaultCenter](xref:Foundation.NSNotificationCenter.DefaultCenter) 다음과 같은 코드를 사용 하 여:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

또는 확인할 특정 개체를 설정할 수 있습니다. 전달 하는 경우 `null` 에 `objectToObserve` 이 메서드에 다른 피어와 마찬가지로 작동 합니다.

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

반환 된 값 `ObserveDidStart` 쉽게 다음과 같은 알림 수신을 중지할 데 사용할 수 있습니다.

```csharp
token.Dispose ();
```

하거나 호출할 수 있습니다 [NSNotification.DefaultCenter.RemoveObserver](xref:Foundation.NSNotificationCenter.RemoveObserver(Foundation.NSObject)) 토큰을 전달 합니다. 도우미를 지정 해야 알림이 매개 변수를 포함할 경우 `EventArgs` 다음과 같은 인터페이스:

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

위의 생성 됩니다는 `MyScreenChangedEventArgs` 클래스를 `ScreenX` 및 `ScreenY` 에서 데이터를 인출 하는 속성을 [NSNotification.UserInfo](xref:Foundation.NSNotification.UserInfo) 키를 사용 하 여 사전 `ScreenXKey` 및 `ScreenYKey` 각각 적절 한 변환을 적용 합니다. 합니다 `[ProbePresence]` 특성을 사용 하는 생성기에 대 한 키에 설정 된 경우 프로브를 `UserInfo`, 값을 추출 하려고 하는 대신 합니다. 이 경우 키의 현재 상태 (일반적으로 부울 값)의 값에 사용 됩니다.

이 옵션을 사용 하면 다음과 같은 코드를 작성할 수 있습니다.

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

경우에 따라 사전에 전달 된 값과 관련 된 상수가 없는 있습니다.  Apple 공용 기호 상수를 사용 하는 경우도 및 문자열 상수를 사용 하는 경우도 있습니다.  기본적으로는 [ `[Export]` ](#ExportAttribute) 제공 하는 특성 `EventArgs` 클래스는 이름을 사용 하 여 지정 된 공용 기호를 런타임에 조회할 수 있습니다.  이 경우 아니며 대신 보낸다고 가정 합니다. 문자열 상수를 조회할 수를 전달 하는 경우는 `ArgumentSemantic.Assign` 내보내기 특성 값입니다.

**8.4 Xamarin.iOS의 새로운 기능**

경우에 따라 알림을 시작 인수 없이 수명 따라서 활용할 [ `[Notification]` ](#NotificationAttribute) 허용 되는 인수 없이 합니다.  하지만 알림 매개 변수를 소개 하는 경우도 있습니다.  이 시나리오를 지원 하려면 특성을 두 번 이상 적용할 수 있습니다.

바인딩을 개발 하는 기존 사용자 코드의 중단을 방지 하려는 경우에서 기존 알림 설정:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

다음과 같은 알림 특성을 두 번 나열 하는 버전:

```csharp
interface MyClass {
    [Notification]
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

<a name="NullAllowedAttribute" />

### <a name="nullallowedattribute"></a>NullAllowedAttribute

속성 값을 허용으로 플래그 속성에 적용 되 면 `null` 할당할 수 있습니다. 참조 형식에 대해서만 유효합니다.

이 지정된 된 매개 변수는 null 일 수 있습니다 하 고 전달 하기 위한 검사가 수행 되어야 함을 나타냅니다 메서드 시그니처에서 매개 변수에 적용 될 때 `null` 값입니다.

바인딩 도구 Objective-c로 전달 하기 전에 할당 되는 값에 대 한 확인을 생성 및 throw 하는 검사를 생성 하는 참조 형식에이 특성이 없는 경우는 `ArgumentNullException` 할당 된 값이 `null`합니다.

예를 들어:

```csharp
// In properties

[NullAllowed]
UIImage IconFile { get; set; }

// In methods
void SetImage ([NullAllowed] UIImage image, State forState);
```

<a name="OverrideAttribute" />

### <a name="overrideattribute"></a>OverrideAttribute

이 특성을 사용 하 여이 특정 메서드에 대 한 바인딩을 사용 하 여 플래그 지정 해야 바인딩 생성기를 지시 하는 `override` 키워드입니다.

### <a name="presnippetattribute"></a>PreSnippetAttribute

이 특성을 사용 하 여 입력된 매개 변수를 확인 한 후 있지만 목표 C.를 호출 하기 전에 삽입 하는 일부 코드를 삽입 합니다.

예제:

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```

### <a name="prologuesnippetattribute"></a>PrologueSnippetAttribute

생성된 된 메서드의 유효성이 검사 되는 매개 변수 앞에 삽입 될 하는 일부 코드를 삽입 하려면이 특성을 사용할 수 있습니다.

예제:

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```

### <a name="postgetattribute"></a>PostGetAttribute

이 클래스에서 값을 가져올 수에서 지정된 된 속성을 호출 하는 바인딩 생성기를 지시 합니다.

이 속성은 참조 된 개체 그래프 유지 하는 참조 개체를 가리키는 캐시를 새로 고칠 일반적으로 사용 됩니다. 일반적으로에 표시 추가/제거와 같은 작업을 포함 하는 코드입니다. 이 메서드는 실제로 사용 되는 개체에 대 한 관리 되는 참조를 유지 후 요소가 추가 되거나 제거 되도록 내부 캐시를 업데이트할 수 있도록 사용 됩니다. 바인딩 도구는 지정 된 바인딩의 모든 참조 개체에 대 한 지원 필드를 생성 하기 때문에 이것이 가능 합니다.

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

이 경우에 `Dependencies` 속성을 추가 하거나 종속성을 제거한 후 호출 됩니다는 `NSOperation` 실제 나타내는 그래프 있다는 것을 보장 하는 개체에 메모리 누수 뿐만 아니라 메모리 손상을 방지 하는 개체를 로드 합니다.

### <a name="postsnippetattribute"></a>PostSnippetAttribute

이 특성을 사용 하 여 일부 삽입 C# 소스 코드 내부 Objective-c로 메서드를 호출한 후 삽입할 코드

예제:

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```

### <a name="proxyattribute"></a>ProxyAttribute

이 특성은 프록시 개체도 플래그를 지정 하는 값을 반환 하도록 적용 됩니다. 일부 Objective C Api 반환 프록시 개체를 사용자 바인딩에서 구분할 수 있습니다. 이 특성의 효과 플래그는 개체를 지정 하는 것을 `DirectBinding` 개체입니다. Xamarin.Mac의 시나리오를 볼 수 있습니다 합니다 [이 버그에 대 한 논의](https://bugzilla.novell.com/show_bug.cgi?id=670844)합니다.

### <a name="retainlistattribute"></a>RetainListAttribute

관리 되는 참조 매개 변수를 유지 하거나 매개 변수를 내부 참조를 제거 하려면 생성기에 지시 합니다. 이 참조 하는 개체에 사용 됩니다.

구문:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

경우 값 `doAdd` 가 true 이면 매개 변수는 추가할를 `__mt_{0}_var List<NSObject>;`입니다. 여기서 `{0}` 바뀝니다는 주어진 `listName`합니다. Api 상호 보완적인 부분 클래스에서이 지원 필드를 선언 해야 합니다.

예를 참조 하세요. [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) 고 [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin.iOS 6.0)

생성기를 호출 하는 것을 나타내려면 형식을 반환 하도록 적용할 수 있습니다 `Release` 반환 하기 전에 개체에 있습니다. 메서드는 가장 일반적인 시나리오는 autoreleased 개체가) (아니라 보존 된 개체를 제공 하는 경우만 필요

예제:

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

또한이 특성 Xamarin.iOS 런타임은 objective-c 이러한 함수에서 반환 시 개체를 유지 해야 할 것을 알 수 있도록 생성된 된 코드에 전파 됩니다.


### <a name="sealedattribute"></a>SealedAttribute

Sealed로 생성된 된 메서드를 플래그를 설정 하려면 생성기에 지시 합니다. 이 특성을 지정 하지 않으면 기본 경우 (가상 메서드, 추상 메서드 또는 기타 특성을 사용 하는 방법에 따라 재정의) 가상 메서드를 생성 합니다.

<a name="StaticAttribute" />

### <a name="staticattribute"></a>StaticAttribute

경우는 `[Static]` 특성이 적용 되는 메서드 또는 속성, 정적 메서드 또는 속성을 생성 합니다. 이 특성은 지정 하지 않으면는 인스턴스 메서드 또는 속성 생성기를 생성 합니다.


### <a name="transientattribute"></a>TransientAttribute

이 플래그 속성 값은 일시적인 항목 즉, iOS에서 일시적으로 만들어진 하지만 오래 지속 되지 않은 개체이 특성을 사용 합니다. 이 특성은 속성에 적용 되 면 생성기를 관리 되는 클래스 개체에 대 한 참조를 유지 하지 않습니다는이 속성에 대 한 지원 필드를 만들지 않습니다.

<a name="WrapAttribute" />

### <a name="wrapattribute"></a>WrapAttribute

Xamarin.iOS/Xamarin.Mac 바인딩의 디자인에는 `[Wrap]` 특성은 강력한 형식의 개체를 포함 하는 약한 형식의 개체를 래핑하는 데 사용 합니다. 이러한 기능은 일반적으로 형식으로 선언 된 Objective-c 대리자 개체를 사용 하 여 주로 `id` 또는 `NSObject`합니다. Xamarin.iOS 및 Xamarin.Mac 사용 되는 규칙 형식으로 해당 대리자 또는 데이터 소스를 노출 하는 `NSObject` 및 규칙 "Weak" + 노출 되지 않도록 이름을 사용 하 여 이름을 지정 합니다. `id delegate` Objective-c에서 속성으로 노출 될 것을 `NSObject WeakDelegate { get; set; }` API 계약 파일에는 속성입니다.

이지만 일반적으로이 대리자에 할당 된 값을 강력한 형식 하므로 강력한 형식을 노출 하 고 적용 합니다 `[Wrap]` 특성, 즉 사용자가 하위 수준 tric에 의존 하는 경우 일부 세밀 하 게 제어 해야 하는 경우 또는 약한 형식을 사용 하도록 선택할 수는 ks, 또는 대부분의 회사에 대 한 강력한 형식의 속성을 사용할 수 있습니다.

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

다음은 사용자는 대리자의 약한 형식의 버전을 사용 하는 방법입니다.

```csharp
// The weak case, user has to roll his own
class SomeObject : NSObject {
    [Export ("doDemo")]
    void CallbackForDoDemo () {}

}

var demo = new Demo ();
demo.WeakDelegate = new SomeObject ();
```

이 강력한 형식의 버전을 사용 하는 방법의 장점은 사용자는 C#의 형식 시스템 및 override 키워드를 사용 하 여 자신의 의도 선언 하는 그에 없는지를 수동으로 사용 하 여 메서드를 장식 하 고 `[Export]`, 이후로 사용자에 대 한 바인딩 작업을 수행 했습니다.

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```

또 다른 용도 `[Wrap]` 특성은 메서드의 강력한 형식의 버전을 지원 하도록 합니다.  예를 들어:

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

생성 하는 멤버 `[Wrap]` 되지 `virtual` 해야 하는 경우 기본적으로는 `virtual` 멤버를 설정할 수 있습니다 `true` 선택적 `isVirtual` 매개 변수입니다.

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

이 섹션에서는 메서드 정의의 매개 변수에 적용할 수 있는 특성을 설명 합니다. 물론 `[NullAttribute]` 전체적으로 속성에 적용 되는 합니다.

<a name="BlockCallback" />

### <a name="blockcallback"></a>BlockCallback

이 특성의 매개 변수 형식에 적용 됩니다 C# 해당 매개 변수로 따르는 Objective-c 바인더에 알리기 위해 대리자 선언 차단 호출 규칙 및이 방식으로 마샬링하고 합니다.

이 일반적으로 다음과 같은 목표를 c:에 정의 된 콜백에 대 한 사용

```objc
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

참고 항목: [CCallback](#CCallback)합니다.

<a name="CCallback" />

### <a name="ccallback"></a>CCallback

이 특성의 매개 변수 형식에 적용 됩니다 C# 대리자에 매개 변수 호출 규칙이 C ABI 함수 포인터에 대 한 준수를 이러한 방식으로 마샬링하고 바인더에 알리기 위해 선언 합니다.

이 일반적으로 다음과 같은 목표를 c:에 정의 된 콜백에 대 한 사용

```objc
typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

참고 항목: [BlockCallback](#BlockCallback).

### <a name="params"></a>매개 변수

사용할 수는 `[Params]` 정의에서 "params" 삽입 생성기가 하는 메서드 정의의 마지막 배열 매개 변수에 특성입니다.   따라서 선택적 매개 변수를 통해 손쉽게 바인딩할 수 있습니다.

예를 들어, 다음 정의:

```csharp
[Export ("loadFiles:")]
void LoadFiles ([Params]NSUrl [] files);
```

다음 코드를를 작성할 수 있습니다.

```csharp
foo.LoadFiles (new NSUrl (url));
foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));
```

이 추가적인 이점이 요소 전달 위해서 배열을 만드는 데 사용자가 필요 하지 않습니다.

<a name="plainstring" />

### <a name="plainstring"></a>PlainString

사용할 수는 `[PlainString]` 특성으로 매개 변수를 전달 하는 대신 C 문자열로 문자열을 전달 하는 바인딩 생성기를 지시 하는 문자열 매개 변수 앞에 `NSString`합니다.

대부분의 Objective C Api 사용 `NSString` 매개 변수가 있지만 소수의 Api 노출를 `char *` 대신 문자열을 전달 하기 위한 API는 `NSString` 변형 합니다.
사용 하 여 `[PlainString]` 이러한 경우입니다.

예를 들어, 다음 Objective-c로 선언 합니다.

```csharp
- (void) setText: (NSString *) theText;
- (void) logMessage: (char *) message;
```

다음과 같은 바인딩되어야 합니다.

```csharp
[Export ("setText:")]
void SetText (string theText);

[Export ("logMessage:")]
void LogMessage ([PlainString] string theText);
```

### <a name="retainattribute"></a>RetainAttribute

지정 된 매개 변수에 대 한 참조를 유지 하는 생성기를 지시 합니다. 생성기는이 필드에 대 한 백업 저장소를 제공 하거나 이름을 지정할 수 있습니다 (의 `WrapName`)에서 값을 저장 합니다. Objective-c 매개 변수로 전달 되는 및 Objective-c 개체의이 복사본에만 유지 됩니다을 알고 있는 경우 관리 되는 개체에 대 한 참조를 저장할 때 유용 합니다. 예를 들어,와 같은 API `SetDisplay (SomeObject)` SetDisplay를 한 번에 하나의 개체를 표시할 수 있습니다만 수 있으므로이 특성을 사용 합니다. 추적 하기 위해 둘 이상의 개체 (예: 스택 같은 API) 해야 하는 경우 사용 된 `[RetainList]` 특성입니다.

구문:

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```


### <a name="retainlistattribute"></a>RetainListAttribute

관리 되는 참조 매개 변수를 유지 하거나 매개 변수를 내부 참조를 제거 하려면 생성기에 지시 합니다. 이 참조 하는 개체에 사용 됩니다.

구문:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

경우 값 `doAdd` 가 true 이면 매개 변수는 추가할를 `__mt_{0}_var List<NSObject>`입니다. 여기서 `{0}` 바뀝니다는 주어진 `listName`합니다. Api 상호 보완적인 부분 클래스에서이 지원 필드를 선언 해야 합니다.

예를 참조 하세요. [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) 고 [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)


### <a name="transientattribute"></a>TransientAttribute

이 특성 매개 변수에 적용 되 고에 Objective-c에서 전환 하는 경우에 사용 됩니다 C#입니다.  다양 한 Objective-c로 전환 하는 동안 `NSObject` 매개 변수 개체의 관리 되는 표현으로 래핑됩니다.

런타임은 네이티브 개체에 대 한 참조를 개체에 관리 되는 마지막 참조가 사라졌는지 GC가 실행 될 때까지 참조를 유지 합니다.

몇 가지 경우에 중요 한 것은 C# 네이티브 개체에 대 한 참조를 유지 하지 않도록 하는 런타임입니다.  이 경우에 따라 기본 네이티브 코드에 매개 변수의 수명 주기는 특별 한 동작이 연결 하는 경우에 발생 합니다.  예를 들어: 매개 변수에 대해 소멸자는 몇 가지 정리 작업을 수행 하거나 일부 중요 한 리소스를 삭제 합니다.

이 특성에는 Objective-c로 돌아가기 덮어쓸 메서드에서 반환 하는 경우 가능 하면 삭제 될 개체를 원하는 런타임에 알립니다.

규칙은 간단 합니다: 런타임은 네이티브 개체에서 새 관리 되는 표현을 생성 해야 함수 끝의 네이티브 개체에 대 한 보존 수 삭제 되 고 관리 되는 개체의 핸들 속성이 지워집니다.   즉, 관리 되는 개체에 대 한 참조를 유지 하는 경우 참조 하는 쓸모 없게 됩니다 (메서드를 호출 하면 예외가 발생).

전달 된 개체를 만들지 않은 경우 또는 개체의 처리 되지 않은 관리 되는 표현을 이미 있는 경우 강제 삭제는 현재 위치를 사용 하지 않습니다. 


## <a name="property-attributes"></a>속성 특성

<a name="NotImplementedAttribute" />

### <a name="notimplementedattribute"></a>NotImplementedAttribute

이 특성은 여기서 속성 getter 사용 하 여 기본 클래스에서 도입 된 하 고 변경할 수 있는 하위 클래스 소개 setter는 Objective-c 관용구를 지원 하기 위해 사용 됩니다.

이후 C# setter와 getter를 해야 하는이 모델에서는 기본 클래스를 지원 하지 않으며 하위 클래스를 사용할 수는 [OverrideAttribute](#OverrideAttribute)합니다.

이 특성은 속성 setter에만 사용 및 목표 C에서 변경할 수 있는 관용구를 지 원하는 데는

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

<a name="enum-attributes" />

## <a name="enum-attributes"></a>Enum 특성

매핑 `NSString` 상수 열거형 값에는 향상 된.NET API를 만드는 간편한 방법입니다. 다음과 같습니다.

* 코드 완성을 표시 하 여 더 유용할 수 있습니다 **만** API에 대 한 올바른 값
* 형식 안전성을 추가 하는 다른 사용할 수 없습니다 `NSString` ; 잘못 된 컨텍스트에서 상수 및
* 코드 완성 기능을 잃지 않고 짧은 API 목록 표시를 만드는 몇 가지 상수를 숨길 수 있습니다.

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

위의 바인딩 정의에서 생성기를 만듭니다는 `enum` 자체 만듭니다 및는 `*Extensions` 열거형 값 사이의 두 가지 방법 변환 메서드를 포함 하는 정적 형식 및 `NSString` 상수입니다. 이 즉, 상수 계속 이러한 API의 일부가 아닌 경우에 개발자에 게 사용할 수 있습니다.

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

데코레이팅 할 수 있습니다 **하나의** 이 특성을 사용 하 여 열거형 값입니다. 이 열거형 값의 알려지지 않은 지정 하는 경우 반환 되는 상수 됩니다.

위의 예제:

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

열거형 값이 없는 데코레이팅된 경우에 `NotSupportedException` throw 됩니다.

### <a name="errordomainattribute"></a>ErrorDomainAttribute

오류 코드는 열거형 값으로 바인딩됩니다. 일반적으로 오류 도메인에 되며 항상 어느 하나에 적용 되는 찾기 (또는 더 있는 경우) 쉽지 않습니다.

그 자체가 열거형 오류 도메인과 연결 하려면이 특성을 사용할 수 있습니다.

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

확장 메서드를 호출할 수 있습니다 `GetDomain` 도메인 오류 상수 가져오려고 합니다.

### <a name="fieldattribute"></a>FieldAttribute

이 동일 `[Field]` 상수 형식 내에 사용 되는 특성입니다. 또한 사용할 수 열거형 내 특정 상수를 사용 하 여 값을 매핑할 합니다.

A `null` 경우는 열거형 값이 반환 되도록 지정 하려면 값을 사용할 수 있습니다를 `null` `NSString` 상수 지정 됩니다.

위의 예제:

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

없으면 `null` 값이 있는지는 `ArgumentNullException` throw 됩니다.

## <a name="global-attributes"></a>전역 특성

전역 특성은 사용 하 여 적용 하거나를 `[assembly:]` 같은 특성 한정자를 [ `[LinkWithAttribute]` ](#LinkWithAttribute) 수 또는 같은 어디에서 나 사용 합니다 [ `[Lion]` ](#SinceAndLionAttributes) 및 [ `[Since]` ](#SinceAndLionAttributes) 특성입니다.

<a name="LinkWithAttribute" />

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

이 예를 들어,이 특성이 어셈블리 수준에서 적용 되는 [CorePlot 바인딩](https://github.com/mono/monotouch-bindings/tree/master/CorePlot) 사용:

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

사용 하는 경우는 `[LinkWith]` 특성을 지정 된 `libraryName` 사용자가 관리 되지 않는 종속성 뿐만 아니라 올바르게 사용 하는 데 필요한 명령줄 플래그를 포함 하는 단일 DLL에 전달할 수 있도록 결과 어셈블리에 포함 되는 Xamarin.iOS에서 라이브러리입니다.

것도 제공 하지 않아도 가능를 `libraryName`있으며이 경우는 `LinkWith` 만 추가 링커 플래그를 지정 하려면 특성을 사용할 수 있습니다.

``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
```

#### <a name="linkwithattribute-constructors"></a>LinkWithAttribute 생성자

이러한 생성자를 사용 하면 연결할 결과 어셈블리를 지 원하는 라이브러리 지원 되는 대상과 라이브러리에 링크 하는 데 필요한 모든 선택적 라이브러리 플래그를 포함 하는 라이브러리를 지정할 수 있습니다.

`LinkTarget` 인수 Xamarin.iOS에서 유추 되 고 설정할 필요가 없습니다.

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

합니다 `ForceLoad` 속성을 사용 하 여를 결정 여부는 `-force_load` 네이티브 라이브러리를 연결 하는 것에 대 한 링크 플래그를 사용 합니다. 지금은이 항상 적용 됩니다.

#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute.Frameworks

모든 프레임 워크에 바인딩되는 라이브러리 하드 요구 사항이 있는지 (이외의 `Foundation` 및 `UIKit`)를 설정 해야를 `Frameworks` 필요한 플랫폼 프레임 워크의 공백으로 구분 된 목록을 포함 하는 문자열 속성입니다. 예를 들어 필요한 라이브러리를 바인딩하는 경우 `CoreGraphics` 및 `CoreText`를 설정 합니다 `Frameworks` 속성을 `"CoreGraphics CoreText"`.

#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute.IsCxx

이 속성을 결과 실행 파일을 사용 하 여 컴파일할 수 해야 할 경우 true로 설정 된 C++ 컴파일러는 C 컴파일러는 기본값을 대신 합니다. 바인딩 라이브러리 작성 된 경우이 사용 하 여 C++입니다.

#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute.LibraryName

번들로 관리 되지 않는 라이브러리의 이름입니다. 확장명이 ".a" 이며 여러 플랫폼 (예를 들어, ARM 및 시뮬레이터 x86)에 대 한 개체 코드를 포함할 수 있습니다.

이전 버전의 Xamarin.iOS 확인 합니다 `LinkTarget` 있지만 플랫폼 지원 라이브러리를 확인 하는 속성은 이제 자동으로 검색 및 `LinkTarget` 속성은 무시 됩니다.

#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute.LinkerFlags

`LinkerFlags` 문자열 바인딩 작성자가 추가 링커 플래그 필요한 네이티브 라이브러리 응용 프로그램에 연결 하는 경우를 지정 하는 방법을 제공 합니다.

예를 들어 네이티브 라이브러리 libxml2 및 zlib를 필요로 하는 경우 설정 합니다 `LinkerFlags` 문자열을 `"-lxml2 -lz"`입니다.

#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute.LinkTarget

이전 버전의 Xamarin.iOS 확인 합니다 `LinkTarget` 있지만 플랫폼 지원 라이브러리를 확인 하는 속성은 이제 자동으로 검색 및 `LinkTarget` 속성은 무시 됩니다.

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute.NeedsGccExceptionHandling

이 속성을 연결 되어 있는 라이브러리는 GCC 예외 처리 (gcc_eh) 라이브러리를 필요로 하는 경우 true로 설정 합니다.

#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute.SmartLink

합니다 `SmartLink` Xamarin.iOS 확인할 수 있도록 하려면 true로 설정 해야 하는지 여부를 `ForceLoad` 요소가 필요한 합니다.

#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute.WeakFrameworks

`WeakFrameworks` 속성이 동일한 방식으로 작동 합니다 `Frameworks` 링크 타임에 제외 하 고 속성을는 `-weak_framework` 지정자 gcc 각 나열 된 프레임 워크에 대 한 전달 됩니다.

`WeakFrameworks` 수 있도록 라이브러리 및 응용 프로그램 플랫폼 프레임 워크에 대 한 약한 링크는 사용할 수 있도록 필요에 따라 이러한 경우 사용할 수 있지만 사용 하지 않는 라이브러리에 추가 기능을 추가 하려는 경우 유용에 강한 종속성 최신 iOS의 버전입니다. 취약 한 연결에 대 한 자세한 내용은 Apple 설명서를 참조 [약한 연결](https://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html)합니다.

약한 연결에 대 한 좋은 후보 것 `Frameworks` 계정 같은 `CoreBluetooth`, `CoreImage`, `GLKit`, `NewsstandKit` 및 `Twitter` 때문만 iOS 5에서에서 사용할 수 있습니다.

<a name="SinceAndLionAttributes" />

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) 및 LionAttribute (macOS)

사용 된 `[Since]` 특정 시점으로 도입 되 고 있는 것으로 플래그 Api에 특성입니다. 기본 클래스, 메서드 또는 속성 사용할 수 없는 경우에 런타임 문제를 일으킬 수 있는 메서드와 형식 플래그를 설정 하려면 특성을 사용할만 해야 합니다.

구문:

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

이 해야 일반적 수에 적용 되지 않습니다 열거형, 제약 조건 또는 새 구조는 이전 버전의 운영 체제를 사용 하 여 장치에서 실행 되는 경우 런타임 오류가 발생 하지는 것입니다.

형식에 적용할 경우 예제:

```csharp
// Type introduced with iOS 4.2
[Since (4,2)]
[BaseType (typeof (UIPrintFormatter))]
interface UIViewPrintFormatter {
    [Export ("view")]
    UIView View { get; }
}
```

새 멤버에 적용 하는 경우 예제:

```csharp
[BaseType (typeof (UIViewController))]
public interface UITableViewController {
    [Export ("tableView", ArgumentSemantic.Retain)]
    UITableView TableView { get; set; }

    [Since (3,2)]
    [Export ("clearsSelectionOnViewWillAppear")]
    bool ClearsSelectionOnViewWillAppear { get; set; }
```

`[Lion]` 특성 Lion를 사용 하 여 도입 된 형식에 대 한 동일한 방식으로 적용 됩니다. 사용 하는 이유 `[Lion]` 주요 OS X 릴리스 드물게 발생 하 고 운영 체제 버전 번호로 보다 해당 코드명 기억 하기가 iOS를 자주 수정 되는 iOS에서 사용 되는 보다 구체적인 버전 번호를 비교

### <a name="adviceattribute"></a>AdviceAttribute

개발자에 게 더 편리 하 게 사용할 수 있는 다른 Api에 대 한 힌트를 제공 하려면이 특성을 사용 합니다.   예를 들어에서 API의 강력한 형식의 버전을 제공 하는 경우에 향상 된 API 개발자를 약한 형식의 특성에이 특성을 사용할 수 있습니다.

이 특성의 정보는 설명서에 표시 되 고 개선 하는 방법에 대 한 사용자 제안 하는 도구를 개발할 수 있습니다.

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

만 Xamarin.iOS 5.4에서 사용할 수 있는 이상.

이 특성을 설정 하면 생성기는이 특정 라이브러리에 대 한 바인딩 (사용 하 여 적용 `[assembly:]`) 빠른 0 복사 문자열 마샬링 형식을 사용 해야 합니다. 이 특성은 명령줄 옵션을 전달 `--zero-copy` 생성기로 합니다.

0-복사를 사용 하 여 문자열을 생성기 효과적으로 사용 하 여 동일한 C# 새 생성 없이 Objective C를 사용 하는 문자열로 문자열 `NSString` 개체와 데이터를 복사 하지 않고는 C# 문자열을 Objective C 문자열입니다. 0 복사 문자열을 사용 하 여의 유일한 단점은 되지 않도록 해야 하는 문자열 속성을 래핑하기로 플래그 지정 하는 `retain` 또는 `copy` 에 `[DisableZeroCopy]` 특성이 설정 합니다. 이 및 필요 하므로 0 복사 문자열에 대 한 핸들을 스택에 할당 되는 함수 반환 시 올바르지 않습니다.

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

어셈블리 수준 특성을 적용할 수도 있습니다 및 어셈블리의 모든 형식에 적용 됩니다.

```csharp
[assembly:ZeroCopyStrings]
```

## <a name="strongly-typed-dictionaries"></a>강력한 형식의 사전

Xamarin.iOS 8.0으로 래핑하는 강력한 형식의 클래스를 쉽게 만들 수 있는 기능을 도입 했습니다 `NSDictionaries`합니다.

항상 사용할 수 있었습니다 하는 동안 합니다 [DictionaryContainer](xref:Foundation.DictionaryContainer) 수동 API와 함께 데이터 형식이 아니며 이제이 작업을 수행 하는 것이 훨씬 간단 합니다.  자세한 내용은 [강력한 형식 표시](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types)합니다.

<a name="StrongDictionary" />

### <a name="strongdictionary"></a>StrongDictionary

이 특성은 인터페이스에 적용할 때 생성기는 클래스에서 파생 되는 인터페이스와 동일한 이름 가진를 생성 [DictionaryContainer](xref:Foundation.DictionaryContainer) 강력한 형식의 인터페이스에 정의 된 각 속성을 설정 하 고 getter 및 setter를 사전에 대 한 합니다.

기존 인스턴스화할 수 있는 클래스가 자동으로 생성이 `NSDictionary` 에 만든 새 또는 합니다.

이 특성은 하나의 매개 변수 사전에서 요소에 액세스 하는 데 사용 되는 키를 포함 하는 클래스의 이름입니다.   기본적으로 특성을 사용 하 여 인터페이스의 각 속성 "키" 접미사를 사용 하 여 이름에 대 한 지정 된 형식의 멤버를 조회 합니다.

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

위의 경우에는 `MyOption` 클래스에 대 한 문자열 속성을 생성 `Name` 를 사용 하는 `MyOptionKeys.NameKey` 문자열을 검색 하려면 사전에 키로 합니다.   사용 된 `MyOptionKeys.AgeKey` 검색할 사전에 키로는 `NSNumber` 정수를 포함 하는

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

#### <a name="strong-dictionary-types"></a>강력한 dictionary 형식

다음 데이터 형식에서 지원 되는 `StrongDictionary` 정의:

|C#인터페이스 유형|`NSDictionary` 저장소 유형|
|---|---|
|`bool`|`Boolean` 에 저장 된 `NSNumber`|
|열거형 값|에 저장 된 정수는 `NSNumber`|
|`int`|에 저장 하는 32 비트 정수를 `NSNumber`|
|`uint`|에 저장 하는 32 비트 부호 없는 정수는 `NSNumber`|
|`nint`|`NSInteger` 에 저장 된 `NSNumber`|
|`nuint`|`NSUInteger` 에 저장 된 `NSNumber`|
|`long`|에 저장 하는 64 비트 정수를 `NSNumber`|
|`float`|로 저장 하는 32 비트 정수를 `NSNumber`|
|`double`|로 저장 하는 64 비트 정수를 `NSNumber`|
|`NSObject` 및 하위 클래스|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C#`Array` 의 `NSObject`|`NSArray`|
|C#`Array` 열거|`NSArray` 포함 된 `NSNumber` 값|
