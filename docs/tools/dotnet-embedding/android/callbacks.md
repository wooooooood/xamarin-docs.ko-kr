---
title: Android에서 콜백
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: c6eaf4dd90b172053b4b87e3427cfe35213c6727
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117163"
---
# <a name="callbacks-on-android"></a>Android에서 콜백

Java에서 호출 C# 범주는 다소를 *위험한 비즈니스*합니다. 즉 있기를 *패턴* 에서 콜백에 대 한 C# java;는 것이 예상 보다 더 복잡 합니다.

Java 용 적합 하 게 하는 콜백을 수행 하기 위한 세 가지 옵션 설명 하겠습니다.

- 추상 클래스
- 인터페이스
- 가상 메서드

## <a name="abstract-classes"></a>추상 클래스

이 가장 쉬운 방법은 대 한 경로가 콜백을 사용할 것을 권장 하므로 `abstract` 가장 간단한 형태로 작업 콜백을 가져올만 하려는 경우.

부터 시작 하겠습니다는 C# 클래스를 구현 하는 Java 싶습니다.

```csharp
[Register("mono.embeddinator.android.AbstractClass")]
public abstract class AbstractClass : Java.Lang.Object
{
    public AbstractClass() { }

    public AbstractClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public abstract string GetText();
}
```

이 작업을 하려면 세부 정보는 다음과 같습니다.

- `[Register]` 좋은 패키지 이름을 생성-java에서를 자동으로 생성 된 패키지 이름 없이 표시 됩니다.
- 서브클래싱 `Java.Lang.Object` Xamarin.Android의 Java 생성기를 통해 클래스를 실행 하려면.NET 포함 하는 신호입니다.
- 빈 생성자:는 원하는 Java 코드에서 사용 됩니다.
- `(IntPtr, JniHandleOwnership)` 생성자: Xamarin.Android가을 만들기 위한 사용 되는 C#-Java 개체에 해당 합니다.
- `[Export]` Java에 메서드 노출 하는 Xamarin.Android 신호를 보냅니다. Java 전 세계에서 소문자 메서드를 사용 하므로 메서드 이름을 변경할 수도 있습니다.

다음 확인을 C# 시나리오를 테스트 하는 방법.

```csharp
[Register("mono.embeddinator.android.JavaCallbacks")]
public class JavaCallbacks : Java.Lang.Object
{
    [Export("abstractCallback")]
    public static string AbstractCallback(AbstractClass callback)
    {
        return callback.GetText();
    }
}
```
`JavaCallbacks` 것으로이 테스트 하려면 클래스일 수는 `Java.Lang.Object`합니다.

이제는 AAR 생성에.NET 어셈블리에서.NET 포함을 실행 합니다. 참조 된 [Getting Started guide](~/tools/dotnet-embedding/get-started/java/android.md) 세부 정보에 대 한 합니다.

Android Studio로 AAR 파일을 가져온 후 단위 테스트를 작성해 보겠습니다.

```java
@Test
public void abstractCallback() throws Throwable {
    AbstractClass callback = new AbstractClass() {
        @Override
        public String getText() {
            return "Java";
        }
    };

    assertEquals("Java", callback.getText());
    assertEquals("Java", JavaCallbacks.abstractCallback(callback));
}
```
따라서 했습니다.

- 구현 된 `AbstractClass` 익명 형식 사용 하 여 java
- 인스턴스를 반환 하는지 확인 수행 `"Java"` Java에서
- 인스턴스를 반환 하는지 확인 수행 `"Java"` 에서C#
- 추가 `throws Throwable`, 이후 C# 생성자를 사용 하 여 현재 표시 된 `throws`

이 단위 테스트 실행에서는-인와 같은 오류와 함께 실패할 것:

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

무엇이 같습니다는 `Invoker` 형식입니다. 서브 클래스를 이것이 `AbstractClass` 전달 하는 C# Java에 대 한 호출 합니다. Java 개체를 입력 하는 경우는 C# world 및 해당 C# 형식은 추상을 Xamarin.Android을 자동으로 찾습니다는 C# 접미사를 사용 하 여 형식을 `Invoker` 내에서 사용할 C# 코드입니다.

이 사용 하 여 Xamarin.Android `Invoker` 무엇 보다도 Java 바인딩 프로젝트에 대 한 패턴입니다.

구현은 다음과 같습니다 `AbstractClassInvoker`:
```csharp
class AbstractClassInvoker : AbstractClass
{
    IntPtr class_ref, id_gettext;

    public AbstractClassInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(AbstractClassInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public override string GetText()
    {
        if (id_gettext == IntPtr.Zero)
            id_gettext = JNIEnv.GetMethodID(class_ref, "getText", "()Ljava/lang/String;");
        IntPtr lref = JNIEnv.CallObjectMethod(Handle, id_gettext);
        return GetObject<Java.Lang.String>(lref, JniHandleOwnership.TransferLocalRef)?.ToString();
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

여기에 약간 상당히 했습니다.

- 접미사를 사용 하 여 클래스를 추가 `Invoker` 해당 서브 클래스 `AbstractClass`
- 추가 `class_ref` 는 Java 클래스 JNI 참조를 포함 하는 C# 클래스
- 추가 `id_gettext` java JNI 참조를 포함 하 `getText` 메서드
- 포함 된 `(IntPtr, JniHandleOwnership)` 생성자
- 구현 `ThresholdType` 고 `ThresholdClass` 에 대 한 세부 정보를 알아야 Xamarin.Android에 대 한 요구 사항으로는 `Invoker`
- `GetText` Java를 조회 하는 데 필요한 `getText` JNI 적절 한 시그니처가 있는 메서드 호출
- `Dispose` 에 대 한 참조의 선택을 취소 하는 데 필요한 것 `class_ref`

이 클래스를 추가 하 고 새 AAR 생성 유닛 테스트를 통과 합니다. 콜백에 대 한이 패턴은 볼 수 있듯이 *이상적인*, 하지만 실행 취소가 가능 합니다.

Java 상호 운용성에 대 한 내용은 참조는 놀라운 [Xamarin.Android 설명서](~/android/platform/java-integration/working-with-jni.md) 이 주제에 있습니다.

## <a name="interfaces"></a>인터페이스

인터페이스는 하나의 세부 정보를 제외 하 고 추상 클래스와 거의 비슷하게: Xamarin.Android에 Java를 생성 하지 않습니다. .NET 포함 하기 전에 없는 Java를 구현 하는 다양 한 시나리오 이므로이 C# 인터페이스입니다.

다음을 가정해 보겠습니다 C# 인터페이스.

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```

`IJavaObject` .NET 포함 하는 Xamarin.Android 인터페이스는 그렇지 않은 경우이 정확히 동일 신호를 보냅니다는 `abstract` 클래스입니다.

Xamarin.Android는이 인터페이스에 대 한 Java 코드를 생성 하지 하므로 추가 하려면 다음이 Java 프로그램 C# 프로젝트:

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```

어디서 나 파일을 배치할 수 있지만 해당 빌드 작업을 설정 해야 `AndroidJavaSource`합니다. 이 신호를 보내는.NET 포함 AAR 파일로 컴파일됩니다에 적절 한 디렉터리에 복사 합니다.

다음으로 `Invoker` 동일 구현 됩니다.

```csharp
class IJavaCallbackInvoker : Java.Lang.Object, IJavaCallback
{
    IntPtr class_ref, id_send;

    public IJavaCallbackInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(IJavaCallbackInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public void Send(string text)
    {
        if (id_send == IntPtr.Zero)
            id_send = JNIEnv.GetMethodID(class_ref, "send", "(Ljava/lang/String;)V");
        JNIEnv.CallVoidMethod(Handle, id_send, new JValue(new Java.Lang.String(text)));
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

AAR 파일을 생성 한 후 다음 전달 단위로 작성할 수 있습니다 하는 Android Studio에서 테스트:

```java
class ConcreteCallback implements IJavaCallback {
    public String text;
    @Override
    public void send(String text) {
        this.text = text;
    }
}

@Test
public void interfaceCallback() {
    ConcreteCallback callback = new ConcreteCallback();
    JavaCallbacks.interfaceCallback(callback, "Java");
    assertEquals("Java", callback.text);
}
```

## <a name="virtual-methods"></a>가상 메서드

재정의 `virtual` Java는 하지만 좋은 경험 하지 않습니다.

다음에 있다고 가정해 보겠습니다 C# 클래스:

```csharp
[Register("mono.embeddinator.android.VirtualClass")]
public class VirtualClass : Java.Lang.Object
{
    public VirtualClass() { }

    public VirtualClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public virtual string GetText() { return "C#"; }
}
```

수행한 경우는 `abstract` 하나의 세부 정보를 제외 하 고 작동할 것 위의 클래스 예제: _Xamarin.Android를 조회 하지는 `Invoker`_ 합니다.

이 문제를 해결 하려면 수정 된 C# 클래스 `abstract`:

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

이 이상적이 지 않습니다. 하지만이 시나리오 작업을 가져옵니다. Xamarin.Android는 승차 합니다 `VirtualClassInvoker` 및 Java를 사용 하 여 `@Override` 메서드.

## <a name="callbacks-in-the-future"></a>나중에 콜백

몇 가지 수행 이러한 시나리오를 향상 시킬 수:

1. `throws Throwable` C# 생성자는이에 고정 [PR](https://github.com/xamarin/java.interop/pull/170)합니다.
1. 인터페이스를 지원 Xamarin.Android에서 Java 생성기를 확인 합니다.
    - 빌드 작업을 사용 하 여 Java 소스 파일을 추가 하는 것에 대 한 필요성을 제거 `AndroidJavaSource`합니다.
1. Xamarin.Android 로드 하는 방법을 확인을 `Invoker` 가상 클래스에 대 한 합니다.
    - 이 클래스를 표시할 필요가 우리의 `virtual` 예제에서는 `abstract`합니다.
1. 생성 `Invoker` 자동으로 포함 하는.NET 클래스
    - 이 복잡 하지만 맞지 않을 것입니다. Xamarin.Android는 다음과 비슷한 Java 바인딩 프로젝트는 이미 수행 됩니다.

여기에서 수행할 작업을 많이 있지만.NET 포함 하려면 이러한 개선할 수 있습니다.

## <a name="further-reading"></a>추가 정보

* [Android 시작](~/tools/dotnet-embedding/get-started/java/android.md)
* [예비 Android 조사](~/tools/dotnet-embedding/android/index.md)
* [.NET 포함 제한 사항](~/tools/dotnet-embedding/limitations.md)
* [오픈 소스 프로젝트 기여](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)
