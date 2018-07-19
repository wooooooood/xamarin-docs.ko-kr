---
title: Android에서 콜백
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 3f18516643c00dc67fe533ecab00e1f415eb5c46
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33922824"
---
# <a name="callbacks-on-android"></a>Android에서 콜백

C#에서 java 전화는는 *위험성이*합니다. 그러나 즉는 *패턴* Java; C#에서 콜백이 것이 예상 보다 더 복잡 합니다.

Java 용 적합 하 게 하는 콜백을 수행 하는 데 세 가지 옵션을 설명 합니다.

- 추상 클래스
- 인터페이스
- 가상 메서드

## <a name="abstract-classes"></a>추상 클래스

이 작업은 콜백에 대 한 쉬운 경로 되므로 사용할 것을 권장 `abstract` 가져오는 가장 간단한 폼에서 작업 하는 콜백을 방금 하려고 합니다.

C# 클래스를 구현 하는 Java 같은부터 시작 하겠습니다.

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

- `[Register]` 좋은 패키지 이름을 생성 하 여 java-는 자동으로 생성 된 패키지 이름 없이 발생 합니다.
- 서브클래싱 `Java.Lang.Object` .NET 포함 하는 방식에 신호를 보냅니다 Xamarin.Android의 Java 생성기를 통해 클래스를 실행 합니다.
- 빈 생성자: Java 코드에서 사용 하 여 할 됩니다.
- `(IntPtr, JniHandleOwnership)` 생성자:는 C#을 만들기 위한 사용 하는 Xamarin.Android-Java 개체에 해당 합니다.
- `[Export]` Xamarin.Android java 메서드를 노출 하도록 신호를 보냅니다. 또한 메서드 이름, 소문자로 변환 메서드를 사용 하는 Java 세계 배치할 수 있으므로 변경할 수 있습니다.

다음 시나리오를 테스트 하려면 C# 메서드 आ म च ी:

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
`JavaCallbacks` 이 테스트 하려면 모든 클래스는로 사용 될 수는 `Java.Lang.Object`합니다.

이제는 AAR를 생성 하 여.NET 어셈블리의.NET 포함을 실행 합니다. 참조는 [시작 가이드](~/tools/dotnet-embedding/get-started/java/android.md) 대 한 자세한 내용은 합니다.

Android Studio로 AAR 파일을 가져온 후 단위 테스트를 써 보세요.

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
따라서 했습니다:

- 구현 된 `AbstractClass` 익명 형식으로 java
- 인스턴스를 반환 되었는지 `"Java"` Java에서
- 인스턴스를 반환 되었는지 `"Java"` C#에서
- 추가 `throws Throwable`C# 생성자와 현재 표시 된 이후, `throws`

이 단위 테스트로 읽고 나면-는 같은 오류와 함께 실패 합니다.

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

누락 된 항목과 같습니다는 `Invoker` 유형입니다. 이것은의 서브 클래스 `AbstractClass` C# Java에 대 한 호출을 전달 하 합니다. Java 개체는 C# 세계를 시작 하 고 해당 하는 C# 형식을 추상 경우 Xamarin.Android 접미사를 사용 하는 C# 형식에 대 한 자동으로 찾습니다 `Invoker` C# 코드에서 사용할 수 있도록 합니다.

이 사용 하 여 Xamarin.Android `Invoker` 무엇 보다도 Java 바인딩 프로젝트에 대 한 패턴입니다.

다음은 구현이 `AbstractClassInvoker`:
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

여기에 상당한은 했습니다:

- 접미사가 있는 클래스를 추가 `Invoker` 서브클래싱하는 `AbstractClass`
- 추가 `class_ref` 보유 하는 JNI에 대 한 참조는 Java 클래스 서브클래싱하는 C# 클래스
- 추가 `id_gettext` 는 java JNI 참조를 포함 하 `getText` 메서드
- 포함 된 `(IntPtr, JniHandleOwnership)` 생성자
- 구현 `ThresholdType` 및 `ThresholdClass` Xamarin.Android 세부 정보에 대 한 요구 사항으로는 `Invoker`
- `GetText` Java를 조회 하는 데 필요한 `getText` 적절 한 JNI 서명으로 메서드 호출
- `Dispose` 에 대 한 참조의 선택을 취소 하는 데만 필요 `class_ref`

이 클래스를 추가 하는 새 AAR 생성 후 우리의 단위 테스트가 통과 합니다. 콜백에 대 한이 패턴은 하지 볼 수 있듯이 *이상적인*, 하지만 행 할 수 있습니다.

Java interop에 대 한 자세한 내용은 참조는 놀라운 [Xamarin.Android 설명서](~/android/platform/java-integration/working-with-jni.md) 이 주제에 있습니다.

## <a name="interfaces"></a>인터페이스

인터페이스는 추상 클래스를 하나의 세부 정보를 제외 하 고 동일: Xamarin.Android Java을 생성 하지 않습니다. 즉,.NET 포함 하기 전에 다양 한 시나리오가 하지 Java C# 인터페이스를 구현 합니다.

다음 C# 인터페이스가 있는 경우를 가정해 보십시오.

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```

`IJavaObject` 데이터 웨어하우스는 Xamarin.Android 인터페이스 있지만 그렇지 않은 경우이 정확히 동일.NET 포함 하는 신호는 `abstract` 클래스입니다.

Xamarin.Android는이 인터페이스에 대 한 Java 코드를 생성 하지 이후 C# 프로젝트에 다음이 Java를 추가 합니다.

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```

파일을 어디에서 든 지 배치할 수 있지만 빌드 작업 설정 하십시오 `AndroidJavaSource`합니다. 이 신호를 보내는.NET 포함 AAR 파일에 컴파일된다를 적절 한 디렉터리에 복사 합니다.

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

AAR 파일을 생성 한 후 다음 전달 단위를 작성할 수 있습니다 Android Studio에서 테스트:

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

재정의 `virtual` java에서 건 가능한 경험 하지 않습니다.

다음 C# 클래스가 있는 경우를 가정해 봅니다.

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

한 경우는 `abstract` 하나의 세부 정보를 제외 하 고 작동할 것 클래스 위의 예에서: _Xamarin.Android를 조회 하지 않습니다는 `Invoker`_ 합니다.

이 문제를 해결 하려면 되도록 C# 클래스를 수정 `abstract`:

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

이 적절 하지만이 시나리오 작업을 가져옵니다. Xamarin.Android 가져옵니다는 `VirtualClassInvoker` 및 Java를 사용 하 여 `@Override` 방법에 있습니다.

## <a name="callbacks-in-the-future"></a>나중에 콜백

몇 가지 수행 이러한 시나리오를 향상 시킬 수 있습니다.

1. `throws Throwable` C#의 생성자는이에 고정 되어 [PR](https://github.com/xamarin/java.interop/pull/170)합니다.
1. 인터페이스를 지원 Xamarin.Android에서 Java 생성기를 확인 합니다.
    - 이렇게 하면 빌드 작업으로 Java 소스 파일을 추가할 필요가 제거 `AndroidJavaSource`합니다.
1. Xamarin.Android 로드 하는 방법을 확인 한 `Invoker` 가상 클래스입니다.
    - 클래스를 표시할 필요가 없으므로이 우리의 `virtual` 예제 `abstract`합니다.
1. 생성 `Invoker` 자동으로 포함 하는.NET에 클래스
    - 이 복잡 하지만 행 할 수 있다는 것입니다. Xamarin.Android 이미 뭔가 Java 바인딩 프로젝트에 대 한 다음과 유사 합니다.

여기에서 수행할 작업을 많이 있지만.NET 포함 하는 이러한 향상 된이 기능 수 있습니다.

## <a name="further-reading"></a>추가 정보

* [Android에서 시작](~/tools/dotnet-embedding/get-started/java/android.md)
* [사전 Android 연구](~/tools/dotnet-embedding/android/index.md)
* [.NET 포함 제한 사항](~/tools/dotnet-embedding/limitations.md)
* [오픈 소스 프로젝트에 기여 하](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)
