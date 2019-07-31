---
title: 바인딩 형식 참조 가이드
description: 이 참조 가이드에서는 목적-C 라이브러리에 대 한 바인딩을 만들 C# 때 이해 해야 하는 다양 한 특성 및 개념에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
author: conceptdev
ms.author: crdun
ms.date: 03/06/2018
ms.openlocfilehash: 65e551669edeebfb3c28d16b4eaa0f9e549eb291
ms.sourcegitcommit: 84764b9c51e769d6d6570a362af8451607c7e0d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68665701"
---
# <a name="binding-types-reference-guide"></a>바인딩 형식 참조 가이드

이 문서에서는 바인딩 및 생성 된 코드를 구동 하기 위해 API 계약 파일에 주석을 추가 하는 데 사용할 수 있는 특성 목록에 대해 설명 합니다.

Xamarin.ios 및 Xamarin.ios API 계약은 C# 일반적으로 목표-C 코드가 표시 되는 방식을 정의 하는 인터페이스 정의로 작성 됩니다. C# 이 프로세스에는 인터페이스 선언과 API 계약이 필요로 할 수 있는 몇 가지 기본 형식 정의가 혼합 되어 포함 됩니다. 바인딩 형식에 대 한 소개는 부록 가이드 [바인딩 목표-C 라이브러리](~/cross-platform/macios/binding/objective-c-libraries.md)를 참조 하세요.

## <a name="type-definitions"></a>형식 정의입니다.

구문:

```csharp
[BaseType (typeof (BTYPE))
interface MyType : [Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

생성 된 개체에 대 한 기본 형식을 선언 [`[BaseType]`](#BaseTypeAttribute) 하는 특성이 있는 계약 정의의 모든 인터페이스입니다. 위의 선언 `MyType` 에서 라는 `MyType`목표- C# C 형식에 바인딩되는 클래스 형식이 생성 됩니다.

위의 `Protocol1` `MyType`샘플에서 인터페이스 상속 구문을 사용 하 여 typename 뒤에 형식을 지정 하는 경우 해당 인터페이스의 콘텐츠는에 대 한 계약의 일부인 것 처럼 인라인 됩니다. `Protocol2`
Xamarin.ios가 프로토콜을 채택 하는 방식은 프로토콜에 선언 된 모든 메서드와 속성을 형식 자체로 인라이닝 하는 것입니다.

다음은에 대 한 `UITextField` 목표 C 선언이 xamarin.ios 계약에서 정의 되는 방법을 보여 줍니다.

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

다음과 같이 C# API 계약으로 작성 됩니다.

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

[`[BaseType]`](#BaseTypeAttribute) 특성을 구성 하는 것 뿐만 아니라 인터페이스에 다른 특성을 적용 하 여 코드 생성의 다른 여러 측면을 제어할 수 있습니다.


### <a name="generating-events"></a>이벤트 생성

Xamarin.ios 및 Xamarin.ios API 디자인의 한 가지 기능은 목표-C 대리자 클래스를 C# 이벤트 및 콜백으로 매핑하는 것입니다. 사용자는 목표-c 프로그래밍 패턴을 채택할 지 여부를 인스턴스 단위로 선택할 수 있으며, 목표-c 런타임이 호출할 다양 한 메서드 `Delegate` 를 구현 하는 클래스의 인스턴스와 같은 속성에 할당 하거나,를 통해 선택할 수 있습니다. C#스타일 이벤트 및 속성 선택

목표-C 모델을 사용 하는 방법에 대 한 한 가지 예를 살펴보겠습니다.

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

위의 예제에서는 두 개의 메서드, 즉 스크롤 이벤트가 발생 한 알림을, 두 번째 메서드를 덮어쓸지 `scrollView` 여부를 나타내는 부울 값을 반환 해야 하는 콜백, 두 번째 메서드를 덮어쓰도록 선택 했는지 확인할 수 있습니다. top 또는 not.

C# 모델을 사용 하면 라이브러리의 사용자가 C# 이벤트 구문이 나 속성 구문을 사용 하 여 알림을 수신 대기 하 여 값을 반환 해야 하는 콜백을 후크 할 수 있습니다.

이는 동일한 기능의 C# 코드가 람다를 사용 하는 것과 같습니다.

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

이벤트는 값을 반환 하지 않으므로 (void 반환 형식이 있음) 여러 복사본을 연결할 수 있습니다. 는 `ShouldScrollToTop` 이벤트가 아닙니다. 대신이 서명이 있는 형식의 `UIScrollViewCondition` 속성입니다.

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

`bool` 값을 반환 합니다 .이 경우에는 람다 구문을 사용 하 여 `MakeDecision` 함수에서 값을 반환 하기만 하면 됩니다.

바인딩 `UIScrollView` 생성기는와 같이 `UIScrollViewDelegate` 클래스를 연결 하는 이벤트 및 속성 생성을 지원 합니다. (이러한 모델 클래스를 사용 [`[BaseType]`](#BaseTypeAttribute) `Events` `Delegates`하여)정의에주석을추가하여이작업을수행합니다.매개 변수 (아래 설명 참조). 이러한 매개 변수를 사용 [`[BaseType]`](#BaseTypeAttribute) 하 여에 주석을 추가 하는 것 외에도 몇 가지 추가 구성 요소를 생성기에 알려야 합니다.

둘 이상의 매개 변수를 사용 하는 이벤트의 경우 (즉, 대리자 클래스의 첫 번째 매개 변수가 발신자 개체의 인스턴스인 경우) 생성 `EventArgs` 된 클래스에 대해 원하는 이름을 제공 해야 합니다. 이 작업은 모델 클래스 [`[EventArgs]`](#EventArgsAttribute) 의 메서드 선언에서 특성을 사용 하 여 수행 됩니다. 예를 들어:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

위의 선언은에서 `EventArgs` 파생 되 고 `UIImagePickerImagePickedEventArgs` `UIImage` 및의 두 `NSDictionary`매개 변수를 압축 하는 클래스를 생성 합니다. 생성자는 다음을 생성 합니다.

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

그런 다음 `UIImagePickerController` 클래스에서 다음을 노출 합니다.

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

값을 반환 하는 모델 메서드는 다르게 바인딩됩니다. 이러한 항목에는 생성 C# 된 대리자의 이름 (메서드에 대 한 서명)과 사용자가 구현을 제공 하지 않는 경우 반환할 기본값을 모두 사용 해야 합니다. 예를 들어 `ShouldScrollToTop` 다음과 같은 정의를 사용할 수 있습니다.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

위의에서는 위에 표시 된 `UIScrollViewCondition` 서명을 사용 하 여 대리자를 만들고, 사용자가 구현을 제공 하지 않으면 반환 값이 true가 됩니다.

[`[DefaultValue]`](#DefaultValueAttribute) 특성 외에도 생성자가 호출 [`[NoDefaultValue]`](#NoDefaultValueAttribute) 에서 지정 된 매개 [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute) 변수의 값을 반환 하도록 지시 하는 특성을 사용 하거나, 생성기에 기본값이 없음을 나타내는 매개 변수를 사용할 수 있습니다. 기본값.

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

`Name` 속성을 사용 하 여이 형식이 대상-C 세계에서 바인딩되는 이름을 제어 합니다. 이는 일반적으로 .NET Framework 디자인 지침 C# 을 준수 하는 이름을 형식에 지정 하는 데 사용 되지만, 해당 규칙을 따르지 않는 목표-C의 이름에 매핑됩니다.

예를 들어 다음 경우에는 .NET Framework 디자인 지침에서 " `NSURLConnection` url" `NSUrlConnection`대신 "url"을 사용 하므로 목표-C 형식을에 매핑합니다.

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

지정 된 이름이 바인딩에서 생성 `[Register]` 된 특성의 값으로 사용 됩니다. 을 `Name` 지정 하지 않으면 생성 된 출력에서 형식의 짧은 이름이 `[Register]` 특성에 대 한 값으로 사용 됩니다.

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType 및 BaseType 대리자

이러한 속성은 생성 된 클래스에서 스타일 생성 C#이벤트를 구동 하는 데 사용 됩니다. 지정 된 클래스를 목표-C 대리자 클래스와 연결 하는 데 사용 됩니다. 클래스에서 대리자 클래스를 사용 하 여 알림과 이벤트를 전송 하는 경우가 많습니다. 예를 들어 `BarcodeScanner` 에는 자매 `BardodeScannerDelegate` 클래스가 있습니다. 클래스 `BarcodeScanner` 에는 일반적으로 `Delegate` 의 `BarcodeScannerDelegate` 인스턴스를 할당 하는 속성이 있습니다 .이 작업을 수행 하는 동안 사용자 C#와 유사한 스타일 이벤트 인터페이스에 노출 하는 것이 좋습니다. 이러한 경우에는 다음을 사용 합니다 .`Events` 특성의 `Delegates` 및 속성입니다. [`[BaseType]`](#BaseTypeAttribute)

이러한 속성은 항상 함께 설정 되며 동일한 수의 요소를 포함 해야 하며 동기화 된 상태로 유지 되어야 합니다. 배열에는 래핑할 각 약하게 형식화 된 대리자에 대 한 문자열이 하나씩 포함 되어 `Events` 있으며, 배열에는 연결 하려는 각 형식에 대 한 형식이 하나씩 포함 되어 있습니다. `Delegates`

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

이 클래스의 새 인스턴스를 만들 때이 특성을 적용 하면에서 참조 `KeepRefUntil` 하는 메서드가 호출 될 때까지 해당 개체의 인스턴스가 계속 유지 됩니다. 사용자가 코드를 사용 하는 개체에 대 한 참조를 유지 하지 않으려는 경우 Api의 유용성을 향상 시키는 데 유용 합니다. 이 속성의 값은 `Delegate` 클래스의 메서드 이름 이므로 `Events` 및 `Delegates` 속성과 함께이 속성을 함께 사용 해야 합니다.

다음 예제에서는 xamarin.ios의에서 `UIActionSheet` 이를 사용 하는 방법을 보여 줍니다.

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

이 특성이 인터페이스 정의에 적용 되 면 생성기에서 기본 생성자를 생성 하지 않습니다.

클래스의 다른 생성자 중 하나를 사용 하 여 개체를 초기화 해야 하는 경우이 특성을 사용 합니다.


### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

인터페이스 정의에이 특성을 적용 하면 기본 생성자에 전용으로 플래그를 지정 합니다. 이는 여전히 확장 파일에서이 클래스의 개체를 내부적으로 인스턴스화할 수 있지만 클래스의 사용자가 액세스할 수 없는 것입니다.

<a name="CategoryAttribute" />

### <a name="categoryattribute"></a>CategoryAttribute

형식 정의에이 특성을 사용 하 여 목표-C 범주를 바인딩하고이를 C# 확장 메서드로 노출 하 여 c #에서 기능을 노출 하는 방식을 미러링합니다.

범주는 클래스에서 사용할 수 있는 메서드 및 속성 집합을 확장 하는 데 사용 되는 목표 C 메커니즘입니다.   실제로는 특정 프레임 워크가 연결 되어 있을 때 ( `NSObject` `UIKit`예:) 새 프레임 워크가 연결 된 경우에만 해당 메서드를 사용할 수 있도록 하는 등 기본 클래스의 기능을 확장 하는 데 사용 됩니다.   다른 경우에는 기능을 통해 클래스의 기능을 구성 하는 데 사용 됩니다.   이러한 메서드는 C# 확장 메서드와 유사 합니다.

다음은 목표-C에서 범주가 표시 되는 모양입니다.

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

위의 예제는 메서드 `UIView` `makeBackgroundRed`를 사용 하 여 인스턴스를 확장 하는 라이브러리에 있습니다.

이를 바인딩하려면 인터페이스 정의에서 [`[Category]`](#CategoryAttribute) 특성을 사용할 수 있습니다.   [`[Category]`](#CategoryAttribute) 특성을 사용 하는 경우 [`[BaseType]`](#BaseTypeAttribute) 특성의 의미가 확장 될 기본 클래스를 지정 하는 데 사용 되지 않습니다.

다음은 확장을 `UIView` 바인딩하고 C# 확장 메서드에 확장 하는 방법을 보여 줍니다.

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

위의에서는 `MakeBackgroundRed` 확장 메서드를 `MyUIViewExtension` 포함 하는 클래스를 만듭니다.   즉, 이제 모든 `MakeBackgroundRed` `UIView` 서브 클래스에서를 호출 하 여 목표-C에 대해 얻을 수 있는 것과 동일한 기능을 제공할 수 있습니다.

경우에 따라 다음 예제와 같이 범주 내에서 **정적** 멤버를 찾을 수 있습니다.

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

이로 인해 **잘못 된** 범주 C# 인터페이스 정의가 발생 합니다.

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

이는 `BoolMethod` C# 인스턴스를 필요로 하는 확장을 사용 하기 위한 것 이지만,objc정적확장을바인딩하는경우확장메서드를구현하는방법때문에부작용`FooObject` 입니다.

위의 정의를 사용 하는 유일한 방법은 다음 코드를 사용 하는 것입니다.

```csharp
(null as FooObject).BoolMethod (range);
```

이를 방지 하기 위한 권장 사항은 `BoolMethod` `FooObject` 인터페이스 정의 자체 내에서 정의를 인라인 하는 것입니다. 이렇게 하면 의도 `FooObject.BoolMethod (range)`한 것 처럼이 확장을 호출할 수 있습니다.

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

정의 내에서 멤버를 [`[Static]`](#StaticAttribute) 찾을 때마다 경고 (BI1117)가 실행 됩니다. [`[Category]`](#CategoryAttribute) [`[Static]`](#StaticAttribute) `[Category (allowStaticMembers: true)]` [`[Category]`](#CategoryAttribute) [`[Internal]`](#InternalAttribute)정의 내에 멤버를 포함 하려는 경우 또는를 사용 하 여 경고를 방해 하거나 멤버 또는 인터페이스 정의를로 데코레이팅 할 수 있습니다. [`[Category]`](#CategoryAttribute)

<a name="StaticAttribute_Class" />

### <a name="staticattribute"></a>StaticAttribute

이 특성이 클래스에 적용 되 면에서 `NSObject`파생 되지 않은 정적 클래스만 생성 [`[BaseType]`](#BaseTypeAttribute) 하므로 특성이 무시 됩니다. 정적 클래스는 노출 하려는 C 공용 변수를 호스트 하는 데 사용 됩니다.

예:

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

는 다음 API C# 를 사용 하 여 클래스를 생성 합니다.

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```

## <a name="protocolmodel-definitions"></a>프로토콜/모델 정의

모델은 일반적으로 프로토콜 구현에서 사용 됩니다.
런타임에서는 실제로 덮어쓴 메서드를 목표로 등록 하기만 한다는 점에서 차이가 있습니다.
그렇지 않으면 메서드가 등록 되지 않습니다.

일반적으로로 플래그가 지정 된 클래스의 서브 클래스를 사용 `ModelAttribute`하는 경우 기본 메서드를 호출 하면 안 됩니다.   해당 메서드를 호출 하면 예외가 throw 됩니다. 재정의 하는 모든 메서드에 대해 하위 클래스에 대 한 전체 동작을 구현 해야 합니다.

<a name="AbstractAttribute" />

### <a name="abstractattribute"></a>AbstractAttribute

기본적으로 프로토콜의 일부인 멤버는 필수가 아닙니다. 이를 통해 사용자는의 `Model` C# 클래스에서 파생 하 고 관심 있는 메서드만 재정의 하 여 개체의 서브 클래스를 만들 수 있습니다. 경우에 따라 목표 c 계약을 사용 하려면 사용자가이 메서드에 대 한 구현을 제공 해야 합니다. 즉, `@required` 목표-c에서 지시문을 사용 하 여 플래그가 지정 됩니다. 이러한 경우에는 `[Abstract]` 특성을 사용 하 여 이러한 메서드에 플래그를 지정 해야 합니다.

특성 `[Abstract]` 은 메서드 또는 속성에 적용 될 수 있으며, 생성기는 생성 된 멤버를 abstract로 플래그 지정 하 고 클래스는 추상 클래스로 플래그를 지정 합니다.

다음은 Xamarin.ios에서 가져온 것입니다.

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

사용자가 모델 개체의이 특정 메서드에 대 한 메서드를 제공 하지 않는 경우 모델 메서드에서 반환 되는 기본값을 지정 합니다.

구문:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

예를 들어 `Camera` 클래스에 대 한 다음 허수 대리자 클래스에서는 `Camera` 클래스에서 속성으로 `ShouldUploadToServer` 노출 되는를 제공 합니다. `Camera` 클래스의 사용자가 true 또는 false에 응답할 수 있는 람다로 값을 명시적으로 설정 하지 않으면이 경우 기본값은 다음과 같이 `DefaultValue` 특성에 지정 된 값인 false가 됩니다.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

사용자가 허수 클래스에서 처리기를 설정 하는 경우이 값은 무시 됩니다.

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

참고 항목: [`[NoDefaultValue]`](#NoDefaultValueAttribute), [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute).

<a name="DefaultValueFromArgumentAttribute" />

### <a name="defaultvaluefromargumentattribute"></a>DefaultValueFromArgumentAttribute

구문:

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

모델 클래스의 값을 반환 하는 메서드에서 제공 하는 경우이 특성은 사용자가 자신의 메서드나 람다를 제공 하지 않은 경우 지정 된 매개 변수의 값을 반환 하도록 생성기에 지시 합니다.

예제:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

위의 경우 `NSAnimation` 클래스의 사용자가 C# 이벤트/속성 중 하나를 사용 하도록 선택 하 고 메서드 또는 람다로 설정 `NSAnimation.ComputeAnimationCurve` 하지 않은 경우 반환 값은 progress 매개 변수에 전달 된 값입니다.

참고 항목: [`[NoDefaultValue]`](#NoDefaultValueAttribute),[`[DefaultValue]`](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>IgnoredInDelegateAttribute

경우에 따라 모델 클래스의 이벤트 또는 대리자 속성을 호스트 클래스에 노출 하지 않는 것이 좋을 수 있으므로이 특성을 추가 하면 해당 클래스를 사용 하 여 데코레이팅된 메서드가 생성 되지 않도록 생성기에 지시할 수 있습니다.

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

이 특성은 사용할 대리자 시그니처의 이름을 설정 하기 위해 값을 반환 하는 모델 메서드에서 사용 됩니다.

예제:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

위의 정의를 사용 하면 생성기에서 다음 공용 선언을 생성 합니다.

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>DelegateApiNameAttribute

이 특성은 생성기에서 호스트 클래스에 생성 된 속성의 이름을 변경할 수 있도록 하는 데 사용 됩니다. FooDelegate 클래스 메서드의 이름이 대리자 클래스에 적합 하지만 호스트 클래스에서 속성으로 홀수를 표시 하는 경우에 유용할 수 있습니다.

또한이 메서드는 FooDelegate 클래스에 있는 그대로 이름이 지정 된 것으로 유지 하는 데 적합 하지만 지정 된 이름을 사용 하 여 호스트 클래스에 노출 하려고 하는 오버 로드 메서드가 두 개 이상인 경우에 유용 하 게 사용 됩니다.

예제:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

위의 정의를 사용 하면 생성기가 host 클래스에서 다음 공용 선언을 생성 합니다.

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

<a name="EventArgsAttribute" />

### <a name="eventargsattribute"></a>EventArgsAttribute

둘 이상의 매개 변수를 사용 하는 이벤트의 경우 (즉, 대리자 클래스의 첫 번째 매개 변수가 보낸 사람 개체의 인스턴스인 경우) 생성 된 EventArgs 클래스에 대해 원하는 이름을 제공 해야 합니다. 클래스의 메서드 선언에서 `[EventArgs]` 특성을 사용 하 여 수행 됩니다. `Model`

예를 들어:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

위의 선언에서는 EventArgs에서 파생 `UIImagePickerImagePickedEventArgs` 되는 클래스를 생성 하 고 매개 변수 `UIImage` 및 `NSDictionary`를 모두 압축 합니다. 생성자는 다음을 생성 합니다.

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

그런 다음 `UIImagePickerController` 클래스에서 다음을 노출 합니다.

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```


### <a name="eventnameattribute"></a>EventNameAttribute

이 특성은 생성기가 클래스에서 생성 된 이벤트 또는 속성의 이름을 변경할 수 있도록 하는 데 사용 됩니다. 모델 클래스 메서드의 이름이 모델 클래스에 적합 하지만 원래 클래스에서 이벤트 나 속성으로 홀수를 확인 하는 경우에 유용 합니다.

예를 들어는 `UIWebView` 에서 다음 비트 `UIWebViewDelegate`를 사용 합니다.

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

위의는에서 `LoadingFinished` 메서드로 `UIWebViewDelegate`표시 하지만 `LoadFinished` 에서 `UIWebView`에 연결할 이벤트로 제공 됩니다.

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```

<a name="ModelAttribute" />

### <a name="modelattribute"></a>ModelAttribute

계약 API의 형식 `[Model]` 정의에 특성을 적용 하면 사용자가 클래스의 메서드를 덮어쓴 경우 런타임에 클래스의 메서드에 대 한 호출만 노출 하는 특수 코드가 생성 됩니다. 일반적으로이 특성은 목표-C 대리자 클래스를 래핑하는 모든 Api에 적용 됩니다.

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

모델의 메서드가 기본 반환 값을 제공 하지 않도록 지정 합니다.

이는 목표-c 런타임 요청에 응답 `false` 하 여 지정 된 selector가이 클래스에서 구현 되는지 확인 하는 방법으로 목표-c 런타임에서 작동 합니다.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

참고 항목: [`[DefaultValue]`](#DefaultValueAttribute),[`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)  

<a name="ProtocolAttribute" />

## <a name="protocols"></a>프로토콜

목표-C 프로토콜 개념은에 C#존재 하지 않습니다. 프로토콜은 C# 인터페이스와 비슷하지만 프로토콜에 선언 된 모든 메서드 및 속성을이를 채택 하는 클래스에서 구현 해야 하는 것은 아닙니다. 대신 일부 메서드 및 속성은 선택 사항입니다.

일부 프로토콜은 일반적으로 모델 클래스로 사용 되며, [`[Model]`](#ModelAttribute) 특성을 사용 하 여 바인딩되어야 합니다.

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

Xamarin.ios 7.0부터 새롭고 향상 된 프로토콜 바인딩 기능이 통합 되었습니다.  `[Protocol]` 특성을 포함 하는 모든 정의는 실제로 프로토콜을 사용 하는 방법을 크게 개선 하는 세 가지 지원 클래스를 생성 합니다.

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

**클래스 구현은** 의 개별 메서드를 재정의 하 고 완전 한 형식 안전성을 얻을 수 있는 완전 한 추상 클래스를 제공 합니다. 그러나 다중 상속 C# 을 지원 하지 않기 때문에 다른 기본 클래스를 요구 하지만 여전히 인터페이스를 구현 하려는 시나리오가 있습니다.

여기서는 생성 된 **인터페이스 정의가** 제공 됩니다.  이 인터페이스는 프로토콜에서 필요한 모든 메서드를 포함 하는 인터페이스입니다.  이를 통해 개발자는 단순히 인터페이스를 구현 하는 프로토콜을 구현할 수 있습니다.  런타임은 프로토콜을 채택 하는 형식으로 자동으로 등록 됩니다.

인터페이스는 필요한 메서드만 나열 하 고 선택적 메서드를 노출 합니다.   즉, 프로토콜을 채택 하는 클래스는 필요한 메서드에 대 한 전체 형식 검사를 수행 하지만 선택적 프로토콜 메서드에 대해 weak 형식 (내보내기 특성을 사용 하 여 수동으로, 시그니처와 일치)을 사용 해야 합니다.

프로토콜을 사용 하는 API를 편리 하 게 사용 하기 위해 바인딩 도구는 모든 선택적 메서드를 노출 하는 확장 메서드 클래스를 생성 합니다.   즉, API를 사용 하는 동안 모든 메서드를 포함 하는 것으로 프로토콜을 처리할 수 있습니다.

API에서 프로토콜 정의를 사용 하려면 API 정의에 빈 인터페이스를 작성 해야 합니다.   API에서 MyProtocol을 사용 하려면 다음을 수행 해야 합니다.

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

위의은 바인딩 타임 `IMyProtocol` 에가 없으므로 빈 인터페이스를 제공 해야 하기 때문에 필요 합니다.

### <a name="adopting-protocol-generated-interfaces"></a>프로토콜 생성 인터페이스 채택

프로토콜에 대해 생성 되는 인터페이스 중 하나를 구현할 때마다 다음과 같이 합니다.

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

인터페이스 메서드에 대 한 구현은 적절 한 이름을 사용 하 여 자동으로 내보내지고 다음과 동일 합니다.

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

인터페이스를 암시적 또는 명시적으로 구현 하는 경우에는 중요 하지 않습니다.

### <a name="protocol-inlining"></a>프로토콜 인라인

프로토콜을 채택 하는 것으로 선언 된 기존 목표-C 형식을 바인딩하는 동안 프로토콜을 직접 인라인 하는 것이 좋습니다. 이렇게 하려면 [`[BaseType]`](#BaseTypeAttribute) 특성을 사용 하지 않고 프로토콜을 인터페이스로 선언 하 고 인터페이스의 기본 인터페이스 목록에 프로토콜을 나열 합니다.

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

이 섹션의 특성은 속성 및 메서드 선언 형식의 개별 멤버에 적용 됩니다.


### <a name="alignattribute"></a>AlignAttribute

속성 반환 형식에 대 한 맞춤 값을 지정 하는 데 사용 됩니다. 특정 속성은 특정 경계에 맞춰야 하는 주소에 대 한 포인터를 사용 합니다 (예를 들어, 16 바이트 `GLKBaseEffect` 정렬 되어야 하는 일부 속성의 경우). 이 속성을 사용 하 여 getter를 장식 하 고 맞춤 값을 사용할 수 있습니다. 이는 일반적으로 객관적인 C `OpenTK.Vector4` api `OpenTK.Matrix4` 와 통합 될 때 및 형식과 함께 사용 됩니다.

예제:

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```


### <a name="appearanceattribute"></a>AppearanceAttribute

특성 `[Appearance]` 은 모양 관리자가 도입 된 iOS 5로 제한 됩니다.

특성 `[Appearance]` 은 `UIAppearance` 프레임 워크에 참여 하는 모든 메서드나 속성에 적용 될 수 있습니다. 이 특성이 클래스의 메서드나 속성에 적용 되는 경우 바인딩 생성기는이 클래스의 모든 인스턴스 또는 특정 조건과 일치 하는 인스턴스를 스타일 지정 하는 데 사용 되는 강력한 형식의 모양 클래스를 만들도록 지시 합니다.

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

위의 코드는 UIToolbar에서 다음 코드를 생성 합니다.

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

### <a name="autoreleaseattribute-xamarinios-54"></a>AutoReleaseAttribute (Xamarin.ios 5.4)

메서드 및 속성 `NSAutoReleasePool` 에대한를사용하여메서드호출을의메서드로`[AutoReleaseAttribute]` 래핑할 수 있습니다.

목적-C에는 기본값 `NSAutoReleasePool`에 추가 된 값을 반환 하는 몇 가지 메서드가 있습니다. 기본적으로 이러한 항목은 스레드로 `NSAutoReleasePool`이동 하지만, xamarin.ios는 관리 되는 개체가 유지 되는 한 개체에 대 한 참조를 유지 하기 때문에, 스레드를 사용할 때까지 완료 되는 추가 참조 `NSAutoReleasePool` 를 유지 하지 않으려고 할 수 있습니다. 제어를 다음 스레드로 반환 하거나 main 루프로 돌아갑니다.

이 특성은 예를 들어, 기본 `UIImage.FromFile` `NSAutoReleasePool`에 추가 된 개체를 반환 하는 높은 속성 (예:)에 적용 됩니다. 이 특성이 없으면 스레드가 주 루프로 제어를 반환 하지 않는 한 유지 됩니다. 스레드가 항상 활성 상태이 고 작업을 대기 하는 일종의 백그라운드 다운로더 Uf 이미지는 해제 되지 않습니다.

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

는 `[ForcedTypeAttribute]` 반환 된 관리 되지 않는 개체가 바인딩 정의에 설명 된 형식과 일치 하지 않는 경우에도 관리 되는 형식 만들기를 적용 하는 데 사용 됩니다.

이는 헤더에 설명 된 형식이 네이티브 메서드의 반환 된 형식과 일치 하지 않는 경우에 유용 합니다. 예를 들어에서 `NSURLSession`다음 목표-C 정의를 사용 합니다.

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

이 `NSURLSessionDownloadTask` 는 인스턴스를 반환 한다는 것을 명확 하 게 알려 주지만 슈퍼 클래스 `NSURLSessionTask`이며로 `NSURLSessionDownloadTask`변환할 수 없는를 **반환** 합니다. 형식이 안전한 컨텍스트에 `InvalidCastException` 있기 때문에이 발생 합니다.

헤더 설명을 준수 하 고을 방지 `InvalidCastException` `[ForcedTypeAttribute]` 하기 위해가 사용 됩니다.

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

에 `[ForcedTypeAttribute]` 는 기본적 `false` `Owns` 으로이라는부울값`[ForcedType (owns: true)]`도 허용 됩니다. 소유 매개 변수는 **핵심 기반** 개체에 대 한 [소유권 정책을](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html) 따르는 데 사용 됩니다.

는 `[ForcedTypeAttribute]` 매개 변수, 속성 및 반환 값에만 사용할 수 있습니다.

<a name="BindAsAttribute" />

### <a name="bindasattribute"></a>BindAsAttribute

에서는 `[BindAsAttribute]` 바인딩`NSNumber` C# 및 (열거형)을보다정확한형식으로사용할수`NSString`있습니다. `NSValue` 특성을 사용 하 여 네이티브 API에 대 한 보다 정확한 .NET API를 만들 수 있습니다.

반환 값에 대 한 메서드, 매개 변수 및 속성 `BindAs`을에 데코레이팅 할 수 있습니다. 유일한 제한 사항은 멤버가 `[Protocol]` 또는 [`[Model]`](#ModelAttribute) 인터페이스 내에 **있지 않아야** 한다는 것입니다.

예를 들어:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

출력:

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

내부적 `bool?` 으로 `NSNumber`  <->  및 변환이수행 <->  됩니다. `CGRect` `NSValue`

현재 지원 되는 캡슐화 유형은 다음과 같습니다.

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

다음 C# 데이터 형식은에서/로 `NSValue`캡슐화 될 수 있습니다.

* CGAffineTransform
* NSRange
* CGVector
* SCNMatrix4
* CLLocationCoordinate2D
* SCNVector3
* SCNVector4
* CGPoint/PointF
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

다음 C# 데이터 형식은에서/로 `NSNumber`캡슐화 될 수 있습니다.

* bool
* byte
* double
* FLOAT
* short
* ssNoversion
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

[`[BindAs]`](#BindAsAttribute)[는 NSString 상수로 지원 되는 열거형](#enum-attributes) 을 사용 하 여 conjuntion에서 작동 하므로 더 나은 .net API를 만들 수 있습니다. 예를 들면 다음과 같습니다.

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

출력:

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

제공 된 열거형 형식이 `enum` [nsstring 상수에 의해 지원](#enum-attributes)되는 [`[BindAs]`](#BindAsAttribute) 경우에만 `NSString` 변환을 처리  <->  합니다.

#### <a name="arrays"></a>배열

[`[BindAs]`](#BindAsAttribute)에서는 지원 되는 형식의 배열만 지원 합니다. 예를 들어 다음과 같은 API 정의를 사용할 수 있습니다.

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

출력:

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

매개 `rects` 변수는 각각 `NSArray` `NSValue` `CAScroll?` `NSArray` 에 대 한를 포함 하는에 캡슐화 되며 반환 된의 값을 사용 하 여 생성 된 배열을 가져옵니다. `CGRect` 포함 `NSStrings`하는입니다.

<a name="BindAttribute" />

### <a name="bindattribute"></a>BindAttribute

특성 `[Bind]` 은 메서드 또는 속성 선언에 적용 될 때 두 가지를 사용 하며, 속성의 개별 getter 또는 setter에 적용 되는 경우에는 두 가지를 사용 합니다.

메서드 또는 속성에 사용 되는 경우 `[Bind]` 특성의 효과는 지정 된 선택기를 호출 하는 메서드를 생성 하는 것입니다. 그러나 생성 된 결과로 생성 된 메서드는 [`[Export]`](#ExportAttribute) 특성으로 데코 레이트 되지 않습니다. 즉, 메서드 재정의에 참여할 수 없습니다. 일반적으로이 `[Target]` 특성은 목표-C 확장 메서드를 구현 하기 위한 특성과 함께 사용 됩니다.

예를 들어:

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

Getter 또는 setter `[Bind]` 에서 사용 되는 경우 속성에 대 한 getter 및 setter 목표-C 선택기 이름을 생성할 때 코드 생성기에 의해 유추 된 기본값을 변경 하는 데 특성이 사용 됩니다. 기본적으로 이름 `fooBar`으로 속성에 플래그를 지정 하는 경우 생성기는 getter 및 `fooBar` `setFooBar:` setter에 대해 내보내기를 생성 합니다. 일부 경우에는 목표 C가이 규칙을 따르지 않으며 일반적으로 getter 이름을로 `isFooBar`변경 합니다.
이 특성을 사용 하 여이의 생성기를 알립니다.

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

Xamarin.ios 6.3 이상 에서만 사용할 수 있습니다.

이 특성은 마지막 인수로 완료 처리기를 사용 하는 메서드에 적용할 수 있습니다.

마지막 인수가 콜백 인 `[Async]` 메서드에 특성을 사용할 수 있습니다.  이를 메서드에 적용 하는 경우 바인딩 생성기는 접미사 `Async`를 사용 하 여 해당 메서드의 버전을 생성 합니다.  콜백이 매개 변수를 사용 하지 않는 경우 반환 값 `Task`은이 됩니다 `Task<T>`. 콜백에서 매개 변수를 사용 하는 경우 결과는가 됩니다.

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

다음은이 비동기 메서드를 생성 합니다.

```csharp
Task LoadFileAsync (string file);
```

콜백이 여러 매개 변수를 사용 하는 경우 `ResultType` 또는 `ResultTypeName` 를 설정 하 여 모든 속성을 보유 하는 생성 된 형식의 원하는 이름을 지정 해야 합니다.

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

다음은이 비동기 메서드를 생성 합니다. `FileLoading` 여기에는 및 `byteCount`모두 `files` 에 액세스할 수 있는 속성이 포함 됩니다.

```csharp
Task<FileLoading> LoadFile (string file);
```

콜백의 `NSError`마지막 매개 변수가 이면 생성 `Async` 된 메서드는 값이 null이 아닌 경우를 확인 하 고, 해당 되는 경우 생성 된 비동기 메서드는 작업 예외를 설정 합니다.

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

위의는 다음과 같은 비동기 메서드를 생성 합니다.

```csharp
Task<string> UploadAsync (string file);
```

오류가 발생 하면 결과 작업에서 결과 `NSErrorException` `NSError`를 래핑하는로 설정 된 예외가 발생 합니다.

#### <a name="asyncattributeresulttype"></a>AsyncAttribute.ResultType

반환 `Task` 하는 개체의 값을 지정 하려면이 속성을 사용 합니다.   이 매개 변수는 기존 형식을 사용 하므로 핵심 api 정의 중 하나에서 정의 해야 합니다.

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute.ResultTypeName

반환 `Task` 하는 개체의 값을 지정 하려면이 속성을 사용 합니다.   이 매개 변수는 원하는 형식 이름 이름을 사용 하 고, 생성기는 콜백이 수행 하는 각 매개 변수에 대해 하나씩 일련의 속성을 생성 합니다.

#### <a name="asyncattributemethodname"></a>AsyncAttribute.MethodName

이 속성을 사용 하 여 생성 된 비동기 메서드의 이름을 사용자 지정할 수 있습니다.   기본값은 메서드 이름을 사용 하 고 "Async" 텍스트를 추가 하는 것입니다 .이 기본값을 변경 하려면이를 사용할 수 있습니다.

### <a name="disablezerocopyattribute"></a>DisableZeroCopyAttribute

이 특성은 문자열 매개 변수 또는 문자열 속성에 적용 되며,이 매개 변수에 대해 제로 복사 문자열 마샬링을 사용 하지 않도록 코드 생성기에 지시 하 고 대신 C# 문자열에서 새 nsstring 인스턴스를 만듭니다.
`--zero-copy` 명령줄 옵션을 사용 하거나 어셈블리 수준 특성 `ZeroCopyStringsAttribute`을 설정 하 여 0 복사 문자열 마샬링을 사용 하도록 생성기에 지시 하는 경우에만이 특성이 문자열에 필요 합니다.

이는 속성이 `retain` `copy` 속성이 아닌 또는 `assign` 속성이 되도록 목표-C에서 속성이 선언 된 경우에 필요 합니다. 이러한 문제는 일반적으로 개발자가 "최적화" 하 여 잘못 된 타사 라이브러리에서 발생 합니다. 일반적 `retain` 으로 또는의 `assign` `NSString` `NSMutableString` 사용자 파생클래스가라이브러리코드를인식하지않고도문자열의내용을변경할수있으므로또는속성이잘못되었습니다.`NSString` 프로그램별. 이는 일반적으로 중간 최적화로 인해 발생 합니다.

다음은 목표-C의 두 가지 속성을 보여 줍니다.

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```


### <a name="disposeattribute"></a>DisposeAttribute

를 클래스 `[DisposeAttribute]` 에 적용 하는 경우 클래스의 `Dispose()` 메서드 구현에 추가 되는 코드 조각을 제공 합니다.

메서드는 `Dispose` `bmac-native` 및 `[Dispose]` `Dispose` 도구에서 자동으로 생성 되기 때문에 특성을 사용 하 여 생성 된 메서드 구현에 코드를 삽입 해야 합니다. `btouch-native`

예를 들어:

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```

<a name="ExportAttribute" />

### <a name="exportattribute"></a>ExportAttribute

`[Export]` 특성은 목표-C 런타임에 노출 될 메서드 또는 속성에 플래그를 지정 하는 데 사용 됩니다. 이 특성은 바인딩 도구와 실제 Xamarin.ios 및 Xamarin.ios 런타임 간에 공유 됩니다. 메서드의 경우 매개 변수가 생성 된 코드에 따라 축 하 여 전달 됩니다. 속성의 경우 기본 선언에 따라 getter 및 setter 내보내기가 생성 됩니다. 바인딩 도구의 동작을 변경 [`[BindAttribute]`](#BindAttribute) 하는 방법에 대 한 자세한 내용은의 섹션을 참조 하세요.

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

[선택기](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html) 는 바인딩되는 기본 목표 C 메서드 또는 속성의 이름을 나타냅니다.

#### <a name="exportattributeargumentsemantic"></a>ExportAttribute.ArgumentSemantic

<a name="FieldAttribute" />

### <a name="fieldattribute"></a>FieldAttribute

이 특성은 C 전역 변수를 요청 시 로드 되 고 코드에 C# 노출 되는 필드로 노출 하는 데 사용 됩니다. 일반적으로 C 또는 객관적인 C에서 정의 된 상수 값을 가져오는 데 필요 하며, 일부 Api에서 사용 되는 토큰 이거나, 값이 불투명 하며 사용자 코드에서 그대로 사용 되어야 하는 상수 값을 가져오는 데 필요 합니다.

구문:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

는 `symbolName` 연결에 사용 되는 C 기호입니다. 기본적으로이는 형식이 정의 된 네임 스페이스에서 이름이 유추 되는 라이브러리에서 로드 됩니다. 기호가 조회 되는 라이브러리가 아니면 매개 변수를 `libraryName` 전달 해야 합니다. 정적 라이브러리를 연결 하는 경우를 `__Internal` `libraryName` 매개 변수로 사용 합니다.

생성 된 속성은 항상 정적입니다.

Field 특성으로 플래그가 지정 된 속성은 다음 형식일 수 있습니다.

* `NSString`
* `NSArray`
* `nint` / `int` / `long`
* `nuint` / `uint` / `ulong`
* `nfloat` / `float`
* `double`
* `CGSize`
* `System.IntPtr`
* 열거형

Setter는 [NSString 상수로](#enum-attributes)지원 되는 열거형에 대해 지원 되지 않지만 필요한 경우 수동으로 바인딩할 수 있습니다.

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

특성 `[Internal]` 은 메서드 또는 속성에 적용 될 수 있으며 생성 된 어셈블리의 코드에만 코드를 액세스할 수 `internal` 있도록 하는 C# 키워드를 사용 하 여 생성 된 코드에 플래그를 지정할 수 있습니다. 이는 일반적으로 더 낮은 수준의 Api를 숨기 거 나, 생성기에서 지원 되지 않는 Api에 대해 개선 하거나, 코드를 작성 하는 데 필요한 api를 제공 하는 데 사용 됩니다.

바인딩을 디자인할 때 일반적으로이 특성을 사용 하 여 메서드 또는 속성을 숨기고, 메서드 또는 속성에 대해 다른 이름을 제공한 다음, C# 보완 지원 파일에서를 노출 하는 강력한 형식의 래퍼를 추가 합니다. 기본 기능.

예를 들어:

```csharp
[Internal]
[Export ("setValue:forKey:")]
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

그러면 지원 파일에 다음과 같은 코드를 사용할 수 있습니다.

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

이 특성은 .net `[ThreadStatic]` 특성으로 주석을 추가할 속성의 지원 필드에 플래그를 지정 합니다. 이는 필드가 스레드 정적 변수인 경우에 유용 합니다.

### <a name="marshalnativeexceptions-xamarinios-606"></a>MarshalNativeExceptions (Xamarin.ios 6.0.6)

이 특성은 메서드가 네이티브 (목표값-C) 예외를 지원할 수 있도록 합니다.
호출은 직접 `objc_msgSend` 호출 하는 대신 ObjectiveC 예외를 catch 하 고 관리 되는 예외로 마샬링하는 사용자 지정 tramstststststststa를 통해 수행 됩니다.

현재는 몇 개의 `objc_msgSend` 서명만 지원 됩니다. (바인딩을 사용 하는 앱의 네이티브 링크가 누락 된 monotouch_ *_atec_ssend* 기호와 함께 실패 하는 경우 서명이 지원 되지 않는 경우), 요청 시 추가 될 수 있습니다.


### <a name="newattribute"></a>NewAttribute

이 특성은 생성기가 선언 앞에 키워드를 `new` 생성 하도록 하는 메서드 및 속성에 적용 됩니다.

동일한 메서드나 속성 이름이 기본 클래스에 이미 있는 서브 클래스에서 도입 될 때 컴파일러 경고를 방지 하는 데 사용 됩니다.

<a name="NotificationAttribute" />

### <a name="notificationattribute"></a>NotificationAttribute

이 특성을 필드에 적용 하 여 생성기에서 강력한 형식의 도우미 알림 클래스를 생성할 수 있습니다.

페이로드를 사용 하지 않는 알림에 대해 인수를 사용 하지 않고이 특성을 사용 하거나, 일반적 `System.Type` 으로 이름이 "EventArgs"로 끝나는 API 정의의 다른 인터페이스를 참조 하는를 지정할 수 있습니다. 생성기는 인터페이스를 서브 `EventArgs` 클래스로 변환 하 고 여기에 나열 된 모든 속성을 포함 합니다. 클래스에서 특성을 [`[Export]`](#ExportAttribute) 사용 하 여 값을 인출 하기 위해 목표-C 사전을 조회 하는 데 사용 되는 키의 이름을 나열 해야 합니다. `EventArgs`

예를 들어:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

위의 코드는 다음 메서드를 사용 하 `MyClass.Notifications` 여 중첩 된 클래스를 생성 합니다.

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

그러면 코드 사용자는 다음과 같은 코드를 사용 하 여 [Nsdefaultcenter](xref:Foundation.NSNotificationCenter.DefaultCenter) 에 게시 된 알림을 쉽게 구독할 수 있습니다.

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

또는 관찰할 특정 개체를 설정 합니다. `objectToObserve` 이 메서드에 전달 `null` 하는 경우 다른 피어와 마찬가지로 동작 합니다.

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

에서 `ObserveDidStart` 반환 된 값을 사용 하 여 다음과 같은 알림 수신을 쉽게 중지할 수 있습니다.

```csharp
token.Dispose ();
```

또는 [Nsnotification](xref:Foundation.NSNotificationCenter.RemoveObserver(Foundation.NSObject)) 를 호출 하 고 토큰을 전달할 수 있습니다. 알림이 매개 변수를 포함 하는 경우 다음과 같이 도우미 `EventArgs` 인터페이스를 지정 해야 합니다.

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

`MyScreenChangedEventArgs` 위의에서는 [nsnotification](xref:Foundation.NSNotification.UserInfo) 에서 데이터를 가져오는 `ScreenX` 및 `ScreenY` 속성을 사용 하 여 클래스를 생성 합니다 .이 키를 사용 하 여 `ScreenXKey` 각각키를사용하고를적용합니다.`ScreenYKey` 적절 한 변환. 특성은 값을 추출 하는 대신에서 키가 설정 `UserInfo`되어 있는 경우 생성기를 검색 하는 데 사용 됩니다. `[ProbePresence]` 키가 값 (일반적으로 부울 값의 경우) 인 경우에 사용 됩니다.

이렇게 하면 다음과 같은 코드를 작성할 수 있습니다.

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

사전에 전달 된 값과 연결 된 상수가 없는 경우도 있습니다.  Apple에서는 때때로 공용 기호 상수를 사용 하 고 문자열 상수를 사용 하기도 합니다.  기본적 [`[Export]`](#ExportAttribute) 으로 제공 `EventArgs` 된 클래스의 특성은 런타임에 조회할 공용 기호로 지정 된 이름을 사용 합니다.  그렇지 않은 경우에는 대신 문자열 상수로 조회 한 다음 내보내기 특성에 `ArgumentSemantic.Assign` 값을 전달 합니다.

**Xamarin.ios의 새로운 방법 8.4**

경우에 따라 인수 없이 알림이 시작 되므로 인수 없이를 [`[Notification]`](#NotificationAttribute) 사용할 수 있습니다.  그러나 경우에 따라 알림에 대 한 매개 변수가 도입 됩니다.  이 시나리오를 지원 하기 위해 특성을 두 번 이상 적용할 수 있습니다.

바인딩을 개발 중이 고 기존 사용자 코드가 손상 되지 않도록 하려면 다음에서 기존 알림을 설정 합니다.

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

다음과 같이 알림 특성을 두 번 표시 하는 버전으로

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

속성에 적용 되는 경우 값 `null` 을 할당할 수 있도록 속성에 플래그를 지정 합니다. 이는 참조 형식에만 유효 합니다.

이 값이 메서드 시그니처의 매개 변수에 적용 되는 경우 지정 된 매개 변수가 null 일 수 있고 값을 전달 `null` 하기 위해 확인을 수행 하지 않음을 나타냅니다.

참조 형식에이 특성이 없는 경우 바인딩 도구는 할당 된 값에 대해 확인을 생성 하 고,이를 목표 C에 전달 하기 전에 할당 된 값이 인 `ArgumentNullException` `null`경우을 throw 하는 검사를 생성 합니다.

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

이 특성을 사용 하 여이 특정 메서드에 대 한 바인딩이 `override` 키워드를 사용 하 여 플래그가 지정 되도록 바인딩 생성기에 지시 합니다.

### <a name="presnippetattribute"></a>PreSnippetAttribute

이 특성을 사용 하 여 입력 매개 변수의 유효성을 검사 한 후, 코드에서 목표-C를 호출 하기 전에 삽입할 코드를 삽입할 수 있습니다.

예제:

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```

### <a name="prologuesnippetattribute"></a>PrologueSnippetAttribute

이 특성을 사용 하 여 생성 된 메서드에서 매개 변수의 유효성을 검사 하기 전에 삽입할 코드를 삽입할 수 있습니다.

예제:

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```

### <a name="postgetattribute"></a>PostGetAttribute

이 클래스에서 지정 된 속성을 호출 하 여 해당 값을 가져오도록 바인딩 생성기에 지시 합니다.

이 속성은 일반적으로 개체 그래프를 참조 하는 참조 개체를 가리키는 캐시를 새로 고치는 데 사용 됩니다. 일반적으로 추가/제거와 같은 작업을 포함 하는 코드에 표시 됩니다. 이 메서드는 사용 중인 개체에 대 한 관리 되는 참조를 유지 하기 위해 요소를 추가 하거나 제거 하기 위해 내부 캐시를 업데이트 하는 데 사용 됩니다. 이는 바인딩 도구에서 지정 된 바인딩의 모든 참조 개체에 대 한 지원 필드를 생성 하기 때문에 가능 합니다.

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

이 경우 `Dependencies` `NSOperation` 개체에서 종속성을 추가 하거나 제거 하 여 속성이 호출 되며, 실제 로드 된 개체를 나타내는 그래프가 있는지 확인 하 여 메모리 누수 뿐만 아니라 메모리 손상을 방지 합니다.

### <a name="postsnippetattribute"></a>PostSnippetAttribute

이 특성을 사용 하 여 코드에서 C# 기본 목표-C 메서드를 호출한 후 삽입할 소스 코드를 삽입할 수 있습니다.

예제:

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```

### <a name="proxyattribute"></a>ProxyAttribute

이 특성은 반환 값에 적용 되어 프록시 개체로 플래그를 지정 합니다. 일부 목표 C Api는 사용자 바인딩과 구분할 수 없는 프록시 개체를 반환 합니다. 이 특성의 효과는 개체를 `DirectBinding` 개체로 플래그 지정 하는 것입니다. Xamarin.ios의 시나리오에서는 [이 버그에 대 한 설명을](https://bugzilla.novell.com/show_bug.cgi?id=670844)볼 수 있습니다.

### <a name="retainlistattribute"></a>RetainListAttribute

매개 변수에 대 한 관리 되는 참조를 유지 하거나 매개 변수에 대 한 내부 참조를 제거 하도록 생성기에 지시 합니다. 개체를 참조 하는 데 사용 됩니다.

구문:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

의 `doAdd` 값이 true 이면 매개 변수가에 추가 `__mt_{0}_var List<NSObject>;`됩니다. 여기서 `{0}` 는 지정 `listName`된로 바뀝니다. 보완 partial 클래스에서이 지원 필드를 API로 선언 해야 합니다.

예제는 [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) 및 [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs) 를 참조 하세요.

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin.ios 6.0)

반환 형식에 적용 하 여 생성기에서 반환 하기 전에 개체에 대해 `Release` 를 호출 해야 함을 나타낼 수 있습니다. 메서드가 유지 되는 개체를 제공 하는 경우에만 필요 합니다 (가장 일반적인 시나리오인 autoreleased 개체가 아닌).

예제:

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

또한이 특성은 생성 된 코드에 전파 되므로 Xamarin.ios 런타임에서는 이러한 함수의 목표 C로 반환 될 때 개체를 유지 해야 한다는 것을 알 수 있습니다.


### <a name="sealedattribute"></a>SealedAttribute

생성 된 메서드를 sealed로 플래그 지정 하도록 생성기에 지시 합니다. 이 특성을 지정 하지 않으면 기본값은 가상 메서드 (가상 메서드, 추상 메서드 또는 다른 특성이 사용 되는 방법에 따라 재정의)를 생성 하는 것입니다.

<a name="StaticAttribute" />

### <a name="staticattribute"></a>StaticAttribute

`[Static]` 특성이 메서드나 속성에 적용 되 면 정적 메서드 또는 속성이 생성 됩니다. 이 특성을 지정 하지 않으면 생성기는 인스턴스 메서드 또는 속성을 생성 합니다.


### <a name="transientattribute"></a>TransientAttribute

이 특성을 사용 하 여 값이 임시 인 속성 (즉, iOS에서 임시로 만들었지만 수명이 지속 되지 않는 개체)의 플래그를 지정 합니다. 이 특성이 속성에 적용 되 면 생성기는이 속성에 대 한 지원 필드를 만들지 않습니다. 즉, 관리 되는 클래스는 개체에 대 한 참조를 유지 하지 않습니다.

<a name="WrapAttribute" />

### <a name="wrapattribute"></a>WrapAttribute

Xamarin.ios/xamarin.ios 바인딩 `[Wrap]` 디자인에서 특성은 강력한 형식의 개체를 사용 하 여 약한 형식의 개체를 래핑하는 데 사용 됩니다. 이는 대개 또는 `id` `NSObject`형식으로 선언 된 목표-C 대리자 개체를 사용 하 여 수행 됩니다. Xamarin.ios 및 xamarin.ios에서 사용 되는 규칙은 이러한 대리자 또는 데이터 원본을 형식 `NSObject` 으로 노출 하 고 "Weak" 및 노출 되는 이름을 사용 하 여 이름을 지정 하는 것입니다. 목표-C의 `NSObject WeakDelegate { get; set; }` 속성은API계약파일에서속성으로노출됩니다.`id delegate`

그러나 일반적으로이 대리자에 할당 되는 값은 강력한 형식이 며, 강력한 형식을 제공 하 고 `[Wrap]` 특성을 적용 합니다. 즉, 사용자가 약간 세밀 하 게 제어 해야 하거나 낮은 수준의 tric를 사용 해야 하는 경우에는 약한 형식을 사용 하도록 선택할 수 있습니다. ks 또는 대부분의 작업에 강력한 형식의 속성을 사용할 수 있습니다.

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

사용자가 약한 형식의 대리자를 사용 하는 방법입니다.

```csharp
// The weak case, user has to roll his own
class SomeObject : NSObject {
    [Export ("doDemo")]
    void CallbackForDoDemo () {}

}

var demo = new Demo ();
demo.WeakDelegate = new SomeObject ();
```

사용자가 강력한 형식의 버전을 사용 하는 방법입니다. 즉, 사용자가 C#의 형식 시스템을 활용 하 고 override 키워드를 사용 하 여 의도를 선언 하 고 사용자가 메서드 `[Export]`를 수동으로 데코레이팅 할 필요가 없습니다. 사용자에 대 한 바인딩에서 작동 했습니다.

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```

`[Wrap]` 특성을 사용 하는 또 다른 방법은 강력한 형식의 메서드 버전을 지 원하는 것입니다.  예:

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

`[Wrap]` 에서 생성 하는 멤버는 `virtual` 기본적으로는 아닙니다. `virtual` 멤버가 필요한 경우 선택적 `isVirtual` 매개 변수로 설정할 `true` 수 있습니다.

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

이 단원에서는 속성 전체에 적용 `[NullAttribute]` 되는 및 메서드 정의의 매개 변수에 적용할 수 있는 특성에 대해 설명 합니다.

<a name="BlockCallback" />

### <a name="blockcallback"></a>BlockCallback

이 특성은 해당 매개 변수가 목표- C# C 블록 호출 규칙을 따르는지 바인더에 알리기 위해 대리자 선언에서 매개 변수 형식에 적용 되며, 이러한 방식으로 마샬링해야 합니다.

일반적으로이 작업은 목표-C에서 다음과 같이 정의 된 콜백에 사용 됩니다.

```objc
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

참고 항목: [Ccallback](#CCallback).

<a name="CCallback" />

### <a name="ccallback"></a>CCallback

이 특성은 해당 매개 변수가 C ABI C# 함수 포인터 호출 규칙을 준수 하 고 이러한 방식으로 마샬링해야 한다는 사실을 바인더에 알리기 위해 대리자 선언의 매개 변수 형식에 적용 됩니다.

일반적으로이 작업은 목표-C에서 다음과 같이 정의 된 콜백에 사용 됩니다.

```objc
typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

참고 항목: [BlockCallback](#BlockCallback).

### <a name="params"></a>Params

메서드 정의의 마지막 `[Params]` 배열 매개 변수에서 특성을 사용 하 여 생성기에서 정의에 "params"를 삽입할 수 있습니다.   이렇게 하면 선택적 매개 변수에 대 한 바인딩을 쉽게 허용할 수 있습니다.

예를 들어 다음과 같은 정의가 있습니다.

```csharp
[Export ("loadFiles:")]
void LoadFiles ([Params]NSUrl [] files);
```

다음 코드를 작성할 수 있습니다.

```csharp
foo.LoadFiles (new NSUrl (url));
foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));
```

이를 통해 사용자는 요소를 전달 하기 위해 배열을 만들 필요가 없다는 장점이 있습니다.

<a name="plainstring" />

### <a name="plainstring"></a>PlainString

문자열 매개 변수 앞 `[PlainString]` 에 특성을 사용 하 여 매개 변수를 `NSString`로 전달 하는 대신 문자열을 C 문자열로 전달 하도록 바인딩 생성기에 지시할 수 있습니다.

대부분의 목표 C api는 `NSString` 매개 변수를 사용 하지만, 몇 가지 api `char *` 는 `NSString` 변형 대신 문자열을 전달 하기 위한 api를 노출 합니다.
이러한 `[PlainString]` 경우를 사용 합니다.

예를 들어 다음과 같은 목표 C 선언이 있습니다.

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

지정 된 매개 변수에 대 한 참조를 유지 하도록 생성기에 지시 합니다. 생성기는이 필드에 대 한 백업 저장소를 제공 하거나 값을 저장할 이름 ( `WrapName`)을 지정할 수 있습니다. 이는 목표-C에 매개 변수로 전달 되는 관리 되는 개체에 대 한 참조를 유지 하는 데 유용 합니다. 예를 들어,와 같은 `SetDisplay (SomeObject)` API는 setdisplay에서 한 번에 하나의 개체만 표시할 가능성이 있으므로이 특성을 사용 합니다. 두 개 이상의 개체를 추적 해야 하는 경우 (예: 스택 같은 API의 경우) 특성을 `[RetainList]` 사용 합니다.

구문:

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```


### <a name="retainlistattribute"></a>RetainListAttribute

매개 변수에 대 한 관리 되는 참조를 유지 하거나 매개 변수에 대 한 내부 참조를 제거 하도록 생성기에 지시 합니다. 개체를 참조 하는 데 사용 됩니다.

구문:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

의 `doAdd` 값이 true 이면 매개 변수가에 추가 `__mt_{0}_var List<NSObject>`됩니다. 여기서 `{0}` 는 지정 `listName`된로 바뀝니다. 보완 partial 클래스에서이 지원 필드를 API로 선언 해야 합니다.

예제는 [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) 및 [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs) 를 참조 하세요.


### <a name="transientattribute"></a>TransientAttribute

이 특성은 매개 변수에 적용 되며, 목표-C에서로 전환 하는 경우 C#에만 사용 됩니다.  이러한 전환 중 다양 한 목표 C `NSObject` 매개 변수는 개체의 관리 되는 표현으로 래핑됩니다.

런타임에서는 네이티브 개체에 대 한 참조를 사용 하 고 개체에 대 한 마지막으로 관리 되는 참조가 사라지고 GC를 실행할 수 있을 때까지 참조를 유지 합니다.

일부 경우에는 C# 런타임에서 네이티브 개체에 대 한 참조를 유지 하지 않는 것이 중요 합니다.  이는 기본 네이티브 코드가 매개 변수의 수명 주기에 특수 동작을 연결한 경우에 발생할 수 있습니다.  예: 매개 변수에 대 한 소멸자는 일부 정리 작업을 수행 하거나 일부 소중한 리소스를 삭제 합니다.

이 특성은 덮어쓴 메서드에서를 다시 목표로 반환 하는 경우 가능한 경우 개체를 삭제 하도록 런타임에 알립니다.

규칙은 단순 합니다. 런타임이 네이티브 개체에서 관리 되는 새 표현을 만들어야 하는 경우에는 함수 끝에서 네이티브 개체의 보존 횟수가 삭제 되 고 관리 되는 개체의 Handle 속성이 지워집니다.   즉, 관리 되는 개체에 대 한 참조를 유지 하는 경우 해당 참조는 쓸모 없게 됩니다 .이에 대 한 메서드를 호출 하면 예외가 throw 됩니다.

전달 된 개체를 만들지 않았거나 개체의 관리 되는 관리 되는 표현이 이미 있는 경우 강제 삭제가 수행 되지 않습니다. 


## <a name="property-attributes"></a>속성 특성

<a name="NotImplementedAttribute" />

### <a name="notimplementedattribute"></a>NotImplementedAttribute

이 특성은 getter가 있는 속성이 기본 클래스에서 도입 되 고 변경 가능한 하위 클래스가 setter를 도입 하는 목표-C를 지 원하는 데 사용 됩니다.

C# 는이 모델을 지원 하지 않으므로 기본 클래스는 setter와 getter를 모두 포함 해야 하며, 하위 클래스는 [overrideattribute](#OverrideAttribute)를 사용할 수 있습니다.

이 특성은 속성 setter 에서만 사용 되며, 목표-C에서 변경 가능한 방법을 지 원하는 데 사용 됩니다.

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

## <a name="enum-attributes"></a>열거형 특성

상수 `NSString` 를 열거형 값에 매핑하는 것이 더 나은 .net API를 만드는 쉬운 방법입니다. 메서드

* API에 대 한 올바른 값 **만** 표시 하 여 코드 완성이 더 유용할 수 있습니다.
* 형식 안전성을 추가 하 고, 잘못 `NSString` 된 컨텍스트에서 다른 상수를 사용할 수 없습니다.
* 를 사용 하 여 일부 상수를 숨기고, 코드 완성 기능을 사용 하지 않고 더 짧은 API 목록을 표시 합니다.

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

위의 바인딩 정의에서 생성기는 자체를 `enum` 만들며, 열거형 값과 `NSString` 상수 사이에 `*Extensions` 두 가지 방법으로 변환 메서드를 포함 하는 정적 형식이 생성 됩니다. 즉, 개발자가 API의 일부가 아닌 경우에도 상수를 계속 사용할 수 있습니다.

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

이 특성을 사용 하 여 **하나의** 열거형 값을 데코레이팅 할 수 있습니다. 열거형 값을 알 수 없는 경우이 값이 반환 되는 상수가 됩니다.

위의 예제에서 다음을 수행 합니다.

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

열거형 값이 데코레이팅 `NotSupportedException` 되지 않으면이 throw 됩니다.

### <a name="errordomainattribute"></a>ErrorDomainAttribute

오류 코드는 열거형 값으로 바인딩됩니다. 일반적으로이에 대 한 오류 도메인이 있으며 적용 되는 항목을 찾을 수 없는 경우도 있습니다 (또는 있는 경우).

이 특성을 사용 하 여 오류 도메인을 열거형 자체와 연결할 수 있습니다.

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

그런 다음 확장 메서드 `GetDomain` 를 호출 하 여 모든 오류에 대 한 도메인 상수를 가져올 수 있습니다.

### <a name="fieldattribute"></a>FieldAttribute

이는 형식 내 `[Field]` 에서 상수에 사용 되는 특성과 동일 합니다. 열거형 내에서이 메서드를 사용 하 여 특정 상수를 사용 하 여 값을 매핑할 수도 있습니다.

값 `null` 을 사용 하 여 `NSString` 상수가 지정 된 `null` 경우 반환 되어야 하는 열거형 값을 지정할 수 있습니다.

위의 예제에서 다음을 수행 합니다.

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

값이 `null` `ArgumentNullException` 없는 경우이 throw 됩니다.

## <a name="global-attributes"></a>전역 특성

전역 특성은 `[assembly:]` [`[Lion]`](#SinceAndLionAttributes) 와 [`[LinkWithAttribute]`](#LinkWithAttribute) 같은 특성 한정자를 사용 하 여 적용 되거나 및 [`[Since]`](#SinceAndLionAttributes) 특성과 같은 어디에서 나 사용할 수 있습니다.

<a name="LinkWithAttribute" />

### <a name="linkwithattribute"></a>LinkWithAttribute

이는 라이브러리의 소비자가 라이브러리에 전달 된 gcc_flags 및 추가 mtouch 인수를 수동으로 구성 하지 않고 바인딩된 라이브러리를 다시 사용 하는 데 필요한 연결 플래그를 개발자가 지정할 수 있게 하는 어셈블리 수준 특성입니다.

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

이 특성은 어셈블리 수준에서 적용 됩니다. 예를 들어 [CorePlot 바인딩에서](https://github.com/mono/monotouch-bindings/tree/master/CorePlot) 사용 하는 것은 다음과 같습니다.

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

`[LinkWith]` 특성을 사용 하는 경우 지정 `libraryName` 된가 결과 어셈블리에 포함 되어 사용자가 관리 되지 않는 종속성을 포함 하는 단일 DLL 뿐만 아니라를 적절 하 게 사용 하는 데 필요한 명령줄 플래그를 제공할 수 있습니다. Xamarin.ios의 라이브러리.

를 제공 `libraryName`하지 않을 수도 있습니다 .이 `LinkWith` 경우 특성을 사용 하 여 추가 링커 플래그만 지정할 수 있습니다.

``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
```

#### <a name="linkwithattribute-constructors"></a>LinkWithAttribute 생성자

이러한 생성자를 사용 하 여 결과 어셈블리, 라이브러리에서 지 원하는 지원 되는 대상 및 라이브러리와 연결 하는 데 필요한 선택적 라이브러리 플래그와 연결 하 고 결과 어셈블리에 포함할 라이브러리를 지정할 수 있습니다.

인수는 `LinkTarget` xamarin.ios에서 유추 되며 설정할 필요가 없습니다.

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

속성은 `-force_load` 링크 플래그가 네이티브 라이브러리를 연결 하는 데 사용 되는지 여부를 결정 하는 데 사용 됩니다. `ForceLoad` 지금은 항상 true 여야 합니다.

#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute.Frameworks

바인딩되는 라이브러리의 프레임 워크 (및 `Foundation` `UIKit`제외)에서 하드 요구 사항이 있는 경우 필요한 플랫폼 프레임 워크의 공백으로 `Frameworks` 구분 된 목록이 포함 된 문자열로 속성을 설정 해야 합니다. `CoreGraphics` 예를 들어 및 `CoreText`가 필요한 라이브러리를 바인딩하는 경우에는 `Frameworks` 속성을로 `"CoreGraphics CoreText"`설정 합니다.

#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute.IsCxx

결과 실행 파일을 기본 (C 컴파일러) 대신 컴파일러를 C++ 사용 하 여 컴파일해야 하는 경우이 속성을 true로 설정 합니다. 바인딩하는 라이브러리를 작성 한 경우이를 사용 C++합니다.

#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute.LibraryName

번들로 묶을 관리 되지 않는 라이브러리의 이름입니다. 확장명이 ". a" 인 파일 이며, 여러 플랫폼에 대 한 개체 코드 (예: 시뮬레이터의 ARM 및 x86)를 포함할 수 있습니다.

이전 버전의 xamarin.ios `LinkTarget` 는 라이브러리에서 지원 되는 플랫폼을 확인 하기 위해 속성을 확인 했지만 이제 자동으로 검색 되 `LinkTarget` 고 속성이 무시 됩니다.

#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute.LinkerFlags

문자열 `LinkerFlags` 은 작성자가 네이티브 라이브러리를 응용 프로그램에 연결할 때 필요한 추가 링커 플래그를 지정 하는 방법을 제공 합니다.

예를 들어 네이티브 라이브러리에 libxml2 및 zlib가 필요한 경우 `LinkerFlags` 문자열을로 `"-lxml2 -lz"`설정 합니다.

#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute.LinkTarget

이전 버전의 xamarin.ios `LinkTarget` 는 라이브러리에서 지원 되는 플랫폼을 확인 하기 위해 속성을 확인 했지만 이제 자동으로 검색 되 `LinkTarget` 고 속성이 무시 됩니다.

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute.NeedsGccExceptionHandling

연결 하려는 라이브러리에 GCC 예외 처리 라이브러리 (gcc_eh)가 필요한 경우이 속성을 true로 설정 합니다.

#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute.SmartLink

Xamarin.ios가 필요한 지 여부를 `ForceLoad` 확인 하려면 속성을true로설정해야합니다.`SmartLink`

#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute.WeakFrameworks

속성 `WeakFrameworks` `Frameworks` 은`-weak_framework` 링크 타임에 표시 되는 각 프레임 워크에 대해 지정자를 gcc에 전달 한다는 점만 제외 하 고 속성과 동일한 방식으로 작동 합니다.

`WeakFrameworks`를 사용 하면 라이브러리와 응용 프로그램이 플랫폼 프레임 워크를 약하게 연결 하 여 사용할 수 있는 경우 선택적으로 사용할 수 있지만, 라이브러리에서 새로운 기능을 추가 하려는 경우 유용 하 게 사용할 수 있습니다. iOS의 버전. 약한 연결에 대 한 자세한 내용은 [Weak 연결](https://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html)에 대 한 Apple 설명서를 참조 하세요.

약한 연결 `Frameworks` 에 적합 한 것은 iOS 5 에서만 사용할 수 있는 `NewsstandKit` 계정 `Twitter` `CoreImage`, `CoreBluetooth`,, `GLKit`및와 비슷합니다.

<a name="SinceAndLionAttributes" />

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) 및 LionAttribute (macOS)

`[Since]` 특성을 사용 하 여 특정 시점에 도입 된 api에 플래그를 지정 합니다. 특성은 기본 클래스, 메서드 또는 속성을 사용할 수 없는 경우 런타임 문제를 일으킬 수 있는 형식 및 메서드에 플래그를 지정 하는 데만 사용 해야 합니다.

구문:

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

이전 버전의 운영 체제를 사용 하는 장치에서 실행 되는 경우 런타임 오류가 발생 하지 않으므로 일반적으로 열거, 제약 조건 또는 새 구조에는 적용 되지 않습니다.

형식에 적용 된 예:

```csharp
// Type introduced with iOS 4.2
[Since (4,2)]
[BaseType (typeof (UIPrintFormatter))]
interface UIViewPrintFormatter {
    [Export ("view")]
    UIView View { get; }
}
```

새 멤버에 적용 되는 경우의 예:

```csharp
[BaseType (typeof (UIViewController))]
public interface UITableViewController {
    [Export ("tableView", ArgumentSemantic.Retain)]
    UITableView TableView { get; set; }

    [Since (3,2)]
    [Export ("clearsSelectionOnViewWillAppear")]
    bool ClearsSelectionOnViewWillAppear { get; set; }
```

`[Lion]` 특성은 동일한 방식으로 적용 되지만, 사자에 도입 된 형식에는 적용 됩니다. Ios에서 사용 되 `[Lion]` 는 보다 구체적인 버전 번호를 사용 하는 이유는 ios가 자주 수정 되는 반면, 주 OS X 릴리스는 드물게 발생 하 고 해당 버전 번호 보다 코드명 운영 체제를 쉽게 기억할 수 있는 것입니다.

### <a name="adviceattribute"></a>AdviceAttribute

이 특성을 사용 하 여 개발자가 사용 하기에 더 편리할 수 있는 다른 Api에 대 한 힌트를 개발자에 게 제공 합니다.   예를 들어 강력한 형식의 API를 제공 하는 경우 약한 형식의 특성에이 특성을 사용 하 여 개발자에 게 더 나은 API를 지시할 수 있습니다.

이 특성의 정보는 설명서에 나와 있습니다. 이러한 정보를 활용 하 여 개선 하는 방법에 대 한 사용자 의견을 제공 하도록 도구를 개발할 수 있습니다.

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

Xamarin.ios 5.4 이상 에서만 사용할 수 있습니다.

이 특성은이 특정 라이브러리 (에 `[assembly:]`적용 되는 경우) 또는 형식에 대 한 바인딩이 고속 제로 복사 문자열 마샬링을 사용 해야 함을 생성기에 지시 합니다. 이 특성은 생성기에 명령줄 옵션 `--zero-copy` 을 전달 하는 것과 같습니다.

문자열에 대해 제로 복사를 사용 하는 경우 생성기는 새 C# `NSString` 개체 만들기를 발생 시 키 지 않고 C# 문자열에서로 데이터를 복사 하지 않고 목적이 사용 하는 문자열과 동일한 문자열을 효과적으로 사용 합니다. 목표-C 문자열입니다. 문자열 0 개를 사용 하는 경우의 유일한 단점은 사용자가 래핑하는 문자열 속성의 경우로 `retain` 플래그를 지정 하거나 `copy` `[DisableZeroCopy]` 특성 집합이 있는지 확인 해야 한다는 것입니다. 0 복사 문자열에 대 한 핸들이 스택에 할당 되 고 함수가 반환 될 때 유효 하지 않기 때문에이 작업이 필요 합니다.

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

어셈블리 수준에서 특성을 적용할 수도 있습니다 .이 특성은 어셈블리의 모든 형식에 적용 됩니다.

```csharp
[assembly:ZeroCopyStrings]
```

## <a name="strongly-typed-dictionaries"></a>강력한 형식의 사전

Xamarin.ios 8.0을 사용 하면 래핑하 `NSDictionaries`는 강력한 형식의 클래스를 쉽게 만들 수 있도록 지원 기능이 도입 되었습니다.

[DictionaryContainer](xref:Foundation.DictionaryContainer) 데이터 형식을 수동 API와 함께 사용할 수는 있지만 이제는이 작업을 수행 하는 것이 훨씬 더 간단 합니다.  자세한 내용은 [서피싱 강력한 형식](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types)을 참조 하세요.

<a name="StrongDictionary" />

### <a name="strongdictionary"></a>StrongDictionary

이 특성이 인터페이스에 적용 되 면 생성기는 [DictionaryContainer](xref:Foundation.DictionaryContainer) 에서 파생 된 인터페이스와 이름이 같은 클래스를 생성 하 고 인터페이스에 정의 된 각 속성을 강력한 형식의 getter로 전환 하 고 사전순.

이렇게 하면 기존 `NSDictionary` 에서 인스턴스화하거나 새로 만든 클래스를 자동으로 생성 합니다.

이 특성은 사전의 요소에 액세스 하는 데 사용 되는 키를 포함 하는 클래스의 이름인 하나의 매개 변수를 사용 합니다.   기본적으로 특성을 사용 하는 인터페이스의 각 속성은 지정 된 형식의 멤버를 "Key" 접미사를 사용 하는 이름으로 조회 합니다.

예:

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

위의 경우 클래스는 `MyOption` 를 사전에 키 `MyOptionKeys.NameKey` 로 사용 하 여 문자열 `Name` 을 검색 하는에 대 한 문자열 속성을 생성 합니다.   및는를 `MyOptionKeys.AgeKey` 사전에 키로 사용 하 여 int를 포함 `NSNumber` 하는을 검색 합니다.

다른 키를 사용 하려는 경우 속성에서 내보내기 특성을 사용할 수 있습니다. 예를 들면 다음과 같습니다.

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

`StrongDictionary` 정의에서 지원 되는 데이터 형식은 다음과 같습니다.

|C#인터페이스 유형|`NSDictionary`저장소 유형|
|---|---|
|`bool`|`Boolean`에 저장 됩니다.`NSNumber`|
|열거형 값|에 저장 된 정수`NSNumber`|
|`int`|32 비트 정수로 저장`NSNumber`|
|`uint`|에 저장 된 32 비트 부호 없는 정수`NSNumber`|
|`nint`|`NSInteger`에 저장 됩니다.`NSNumber`|
|`nuint`|`NSUInteger`에 저장 됩니다.`NSNumber`|
|`long`|64 비트 정수로 저장`NSNumber`|
|`float`|로 저장 된 32 비트 정수`NSNumber`|
|`double`|로 저장 된 64 비트 정수`NSNumber`|
|`NSObject`및 서브 클래스|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C#`Array` 의`NSObject`|`NSArray`|
|C#`Array` 열거의|`NSArray`값 `NSNumber` 포함|
