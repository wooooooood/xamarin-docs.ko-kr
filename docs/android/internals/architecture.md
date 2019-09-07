---
title: 아키텍처
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 06817c563f12425e5c339cb8f2560f37f9ace0b5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756685"
---
# <a name="architecture"></a>아키텍처

Xamarin Android 응용 프로그램은 Mono 실행 환경에서 실행 됩니다.
이 실행 환경은 Android 런타임 (ART) 가상 머신과 side-by-side 방식으로 실행 됩니다. 두 런타임 환경은 모두 Linux 커널 위에서 실행 되며 개발자가 기본 시스템에 액세스할 수 있도록 하는 다양 한 Api를 사용자 코드에 노출 합니다. Mono 런타임은 C 언어로 작성 됩니다.

[시스템](xref:System), [System.IO](xref:System.IO), [System.Net](xref:System.Net) 및 나머지 .net 클래스 라이브러리를 사용 하 여 기본 Linux 운영 체제 기능에 액세스할 수 있습니다.

Android에서 오디오, 그래픽, OpenGL 및 전화 통신과 같은 대부분의 시스템 기능은 네이티브 응용 프로그램에 직접 사용할 수 없으며, [java](xref:Java.Lang). * 네임 스페이스 또는 [android](xref:Android). * 네임 스페이스. 중 하나에 있는 Android Runtime java api를 통해서만 노출 됩니다. 아키텍처는 대략적으로 다음과 같습니다.

[![커널 및 기타 .NET/Java + 바인딩 위의 Mono 및 아트 다이어그램](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Xamarin Android 개발자는에 의해 노출 되는 Java Api에 대 한 연결을 제공 하는 .NET Api를 호출 하 여 운영 체제의 다양 한 기능에 액세스할 수 있습니다 (낮은 수준 액세스의 경우). Android 런타임입니다.

Android 클래스가 Android 런타임 클래스와 통신 하는 방법에 대 한 자세한 내용은 [API 디자인](~/android/internals/api-design.md) 문서를 참조 하세요.

## <a name="application-packages"></a>애플리케이션 패키지

Android 응용 프로그램 패키지는 *.apk* 파일 확장명을 가진 ZIP 컨테이너입니다. Xamarin Android 응용 프로그램 패키지는 일반적인 Android 패키지와 동일한 구조와 레이아웃을 가지 며 다음과 같이 추가 되었습니다.

- 응용 프로그램 어셈블리 (IL 포함)는 *어셈블리* 폴더 내에 압축 되지 않은 상태로 *저장* 됩니다. 릴리스 빌드에서 프로세스를 시작 하는 동안 *. apk* 는 프로세스에 대 한 *mmap ()* 이 고 어셈블리는 메모리에서 로드 됩니다. 따라서 어셈블리를 실행 하기 전에 추출할 필요가 없기 때문에 앱 시작 속도를 높일 수 있습니다.  
- *참고:* Assembly [. location](xref:System.Reflection.Assembly.Location) 및 [assembly](xref:System.Reflection.Assembly.CodeBase) 와 같은 어셈블리 위치 정보는 릴리스 빌드에서 *사용할 수 없습니다* . 이러한 항목은 고유 파일 시스템 항목으로 존재 하지 않으며 사용 가능한 위치를 갖지 않습니다.

- Mono 런타임을 포함 하는 네이티브 라이브러리는 *.apk* 내에 있습니다. Xamarin.ios 응용 프로그램에는 원하는/대상 Android 아키텍처에 대 한 네이티브 라이브러리가 포함 되어야 합니다 (예: *armeabi-v7a* , *armeabi-v7a-armeabi-v7a* , *x86* ). Xamarin Android 응용 프로그램은 적절 한 런타임 라이브러리가 포함 된 경우를 제외 하 고 플랫폼에서 실행할 수 없습니다.

Xamarin Android 응용 프로그램에는 android에서 관리 코드를 호출할 수 있도록 하는 *Android 호출 가능 래퍼* 도 포함 됩니다.

## <a name="android-callable-wrappers"></a>Android 호출 가능 래퍼

- **Android 호출 가능 래퍼** 는 android 런타임에서 관리 코드를 호출 해야 하는 경우 언제 든 지 사용 되는 [JNI](https://en.wikipedia.org/wiki/Java_Native_Interface) 브리지입니다. Android 호출 가능 래퍼는 가상 메서드를 재정의 하 고 Java 인터페이스를 구현할 수 있는 방법입니다. 자세한 내용은 [Java 통합 개요](~/android/platform/java-integration/index.md) 문서를 참조 하세요.

<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>관리 되는 호출 가능 래퍼

관리 되는 호출 가능 래퍼는 관리 코드에서 Android 코드를 호출 하 고 가상 메서드를 재정의 하 고 Java 인터페이스를 구현 하는 데 필요한 시간에 사용 되는 JNI 브리지입니다. 전체 [Android](xref:Android). * 및 관련 네임 스페이스는 [jar 바인딩을](~/android/platform/binding-java-library/index.md)통해 생성 된 관리 되는 호출 가능 래퍼입니다.
관리 되는 호출 가능 래퍼는 관리 되는 형식과 Android 형식 간에 변환 하 고 JNI를 통해 기본 Android 플랫폼 메서드를 호출 합니다.

생성 된 각 관리 되는 호출 가능 래퍼에는 Java 전역 참조 ( [IJavaObject](xref:Android.Runtime.IJavaObject.Handle) 속성을 통해 액세스 가능)가 포함 됩니다. 전역 참조를 사용 하 여 Java 인스턴스와 관리 되는 인스턴스 간의 매핑을 제공 합니다. 전역 참조는 제한 된 리소스입니다. 에뮬레이터에서 한 번에 2000 전역 참조만 존재할 수 있지만 대부분의 하드웨어는 한 번에 하나의 52000 전역 참조를 사용할 수 있습니다.

전역 참조가 생성 되 고 삭제 되는 시기를 추적 하려면 [gref](~/android/troubleshooting/index.md)시스템 [속성](~/android/troubleshooting/index.md) 을 포함 하도록 설정 하면 됩니다.

관리 되는 호출 가능 래퍼에서 node.js [()](xref:Java.Lang.Object.Dispose) 를 호출 하 여 전역 참조를 명시적으로 해제할 수 있습니다. 그러면 Java 인스턴스와 관리 되는 인스턴스 간의 매핑이 제거 되 고 Java 인스턴스를 수집할 수 있습니다. 관리 코드에서 Java 인스턴스를 다시 액세스 하는 경우 관리 되는 호출 가능 래퍼가 새로 생성 됩니다.

인스턴스가 스레드 간에 실수로 공유 될 수 있는 경우 관리 되는 호출 가능 래퍼를 삭제할 때 주의가 필요 합니다. 인스턴스를 삭제 하면 다른 스레드의 참조에 영향을 줍니다. 최대 안전을 `Dispose()` *위해 항상 새* 인스턴스를 할당 하 고 캐시 된 인스턴스가 아닌 새 인스턴스를 할당 하는 인스턴스 `new` *를 통해 할당* 된 인스턴스만 실수로 인스턴스를 공유할 수 있습니다. 임계값.

## <a name="managed-callable-wrapper-subclasses"></a>관리 되는 호출 가능 래퍼 서브 클래스

관리 되는 호출 가능 래퍼 서브 클래스는 모든 "흥미로운" 응용 프로그램별 논리가 존재할 수 있는 위치입니다. 여기에는 사용자 지정 [Android.App.Activity](xref:Android.App.Activity) 서브 클래스 (예: 기본 프로젝트 템플릿의 [Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) 형식)가 포함 됩니다. 특히 [registerattribute](xref:Android.Runtime.RegisterAttribute) 사용자 지정 특성 또는 [registerattribute](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) 를 포함 *하지* 않는 DoNotGenerateAcw *하위 클래스입니다* . 기본값은 *false*입니다.

관리 되는 호출 가능 래퍼와 같이 관리 되는 호출 가능 래퍼 서브 클래스에는 [Java. 개체. Handle](xref:Java.Lang.Object.Handle) 속성을 통해 액세스할 수 있는 전역 참조도 포함 됩니다. 관리 되는 호출 가능 래퍼의 경우와 마찬가지로, [Java. system.object. Dispose ()](xref:Java.Lang.Object.Dispose)를 호출 하 여 전역 참조를 명시적으로 해제할 수 있습니다.
관리 되는 호출 가능 래퍼와 달리 인스턴스를 *삭제*하기 전에 해당 인스턴스를 삭제 하기 전에 *수행 해야 하는 일은 Java* 인스턴스 (Android 호출 가능 래퍼의 인스턴스)와 관리 되는 인스턴스.

### <a name="java-activation"></a>Java 활성화

Java에서 acw ( [Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md) )를 만들면 acw 생성자가 해당 C# 생성자를 호출 합니다. 예를 들어, *Mainactivity* 의 acw에는 *mainactivity*의 기본 생성자를 호출 하는 기본 생성자가 포함 됩니다. 이 작업은 ACW 생성자 내에서 *Typemanager. Activate ()* 호출을 통해 수행 됩니다.

결과의 다른 생성자 시그니처는 *(IntPtr, JniHandleOwnership)* 생성자입니다. *(IntPtr, JniHandleOwnership)* 생성자는 Java 개체가 관리 코드에 노출 되 고 관리 되는 호출 가능 래퍼가 JNI 핸들을 관리 하기 위해 생성 되어야 할 때마다 호출 됩니다. 일반적으로이 작업은 자동으로 수행 됩니다.

관리 되는 호출 가능 래퍼 서브 클래스에서 *(IntPtr, JniHandleOwnership)* 생성자를 수동으로 제공 해야 하는 두 가지 시나리오가 있습니다.

1. [Android. 응용 프로그램이](xref:Android.App.Application) 서브클래싱된 경우. *응용 프로그램* 은 특수 합니다. 기본 *응용 프로그램* 생성자는 호출 *되지 않으며* , [대신 (IntPtr, JniHandleOwnership) 생성자를 제공 해야](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105)합니다.

2. 기본 클래스 생성자에서 가상 메서드를 호출 합니다.

(2)은 누설 추상화입니다. 에서 C#와 같이 Java에서 생성자의 가상 메서드 호출은 항상 가장 많이 파생 된 메서드 구현을 호출 합니다. 예를 들어 [TextView (Context, AttributeSet, int) 생성자](xref:Android.Widget.TextView#ctor*) 는 TextView [TextView getDefaultMovementMethod ()](https://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod())를 호출 합니다 .이 메서드는 [. defaultmovementmethod 속성](xref:Android.Widget.TextView.DefaultMovementMethod)으로 바인딩됩니다.
따라서 [Logtextbox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) 형식이 (1) [서브 클래스 TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [TextView를 재정의](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45)하 고 (3) [XML을 통해 해당 클래스의 인스턴스를 활성화](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) 하는 경우 재정의 된 *defaultmovementmethod* 속성은 ACW 생성자를 실행 하기 전에 호출 하 고 생성자를 C# 실행 하기 전에 발생 합니다.

이는 ACW LogTextBox 인스턴스가 먼저 관리 코드를 입력 한 다음 [Logtextbox (Context, IAttributeSet, int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41)를 호출 하는 경우 [LogTextView (IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) 생성자를 통해 인스턴스 logtextbox를 인스턴스화하여 지원 됩니다. ACW 생성자가 실행 될 때 *동일한 인스턴스의* 생성자.

이벤트 순서:

1. 레이아웃 XML은 [Contentview](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41)에 로드 됩니다.

2. Android는 Layout 개체 그래프를 인스턴스화하고 monodroid의 인스턴스를 인스턴스화합니다 *. logtextbox의* acw는 *apidemo* 입니다.

3. *Monodroid. apidemo* 생성자는 [TextView](https://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) 생성자를 실행 합니다.

4. *TextView* 생성자는 *Apidemo () getDefaultMovementMethod ()* 를 호출 합니다.

5. *monodroid () apidemo* () *를 호출 하* 는 *TextView ()* [를호출하는getDefaultMovementMethod()를호출합니다.&lt; TextView&gt; (handle, JniHandleOwnership DoNotTransfer)](xref:Java.Lang.Object.GetObject*) .

6. *TextView&gt;()는 핸들에 해당 하는 인스턴스가 이미 있는지 확인 합니다.&lt;* C# 있는 경우 반환 됩니다. 이 시나리오에서는 *개체. GetObject&lt;&gt;t ()* 가 하나를 만들어야 합니다.

7. *개체. GetObject&lt;T&gt;()* 는 *logtextbox (IntPtr, JniHandleOwneship)* 생성자를 찾고 호출 하 고, *핸들과* 만든 인스턴스 간에 매핑을 만들고, 만든 인스턴스를 반환 합니다.

8. *TextView () n_GetDefaultMovementMethod ()* 는 *Logtextbox. DefaultMovementMethod* 속성 getter를 호출 합니다.

9. 컨트롤은 실행을 완료 하는 *TextView* 생성자로 반환 됩니다.

10. *Apidemo monodroid* 생성자가 실행 되어 *Typemanager. Activate ()* 를 호출 합니다.

11. *Logtextbox (Context, IAttributeSet, int)* 생성자는 *(7)에서 만든 동일한 인스턴스에서* 실행 됩니다.

12. (IntPtr, JniHandleOwnership) 생성자를 찾을 수 없는 경우 MissingMethodException] (f: MissingMethodException)가 throw 됩니다.

<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>중간에 Dispose () 호출

JNI 핸들과 해당 C# 인스턴스 간에 매핑이 있습니다. Node.js. Dispose ()는이 매핑을 중단 합니다. 매핑이 중단 된 후 JNI 핸들이 관리 코드에 들어가면 Java 활성화와 같이 표시 되 고 *(IntPtr, JniHandleOwnership)* 생성자가 확인 되 고 호출 됩니다. 생성자가 없으면 예외가 throw 됩니다.

예를 들어 다음과 같은 관리 되는 호출 가능 Wraper 하위 클래스가 있다고 가정 합니다.

```csharp
class ManagedValue : Java.Lang.Object {

    public string Value {get; private set;}

    public ManagedValue (string value)
    {
        Value = value;
    }

    public override string ToString ()
    {
        return string.Format ("[Managed: Value={0}]", Value);
    }
}
```

인스턴스를 만들고 ()의 인스턴스를 삭제 하 고 관리 되는 호출 가능 래퍼가 다시 만들어지도록 합니다.

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

프로그램이 다음과 같이 소멸 됩니다.

```shell
E/mono    ( 2906): Unhandled Exception: System.NotSupportedException: Unable to activate instance of type Scratch.PrematureDispose.ManagedValue from native handle 4051c8c8 --->
System.MissingMethodException: No constructor found for Scratch.PrematureDispose.ManagedValue::.ctor(System.IntPtr, Android.Runtime.JniHandleOwnership)
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateProxy (System.Type type, IntPtr handle, JniHandleOwnership transfer) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   --- End of inner exception stack trace ---
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object.GetObject (IntPtr handle, JniHandleOwnership transfer, System.Type type) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object._GetObject[IJavaObject] (IntPtr handle, JniHandleOwnership transfer) [0x00000
```

하위 클래스에 *(IntPtr, JniHandleOwnership)* 생성자가 포함 된 경우 형식의 *새* 인스턴스가 생성 됩니다. 따라서 인스턴스는 새 인스턴스 이므로 모든 인스턴스 데이터를 "손실" 하는 것으로 나타납니다. 값은 null입니다.

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Java 개체가 더 이상 사용 되지 않거나 하위 클래스에 인스턴스 데이터가 없고 *(IntPtr, JniHandleOwnership)* 생성자가 제공 된 경우에만 관리 되는 호출 가능 래퍼 서브 클래스의 *Dispose ()만 삭제* 합니다.

## <a name="application-startup"></a>응용 프로그램 시작

활동, 서비스 등을 시작 하면 Android는 먼저 활동/서비스를 호스트 하는 프로세스가 이미 실행 중인지 확인 합니다. 이러한 프로세스가 없으면 새 프로세스가 생성 되 고 [androidmanifest](https://developer.android.com/guide/topics/manifest/manifest-intro.html) 을 읽고 [/manifest/application/@android:name](https://developer.android.com/guide/topics/manifest/application-element.html#nm) 특성에 지정 된 형식이 로드 되 고 인스턴스화됩니다. 그런 다음 [/manifest/application/provider/@android:name](https://developer.android.com/guide/topics/manifest/provider-element.html#nm) 특성 값으로 지정 된 모든 형식이 인스턴스화되고 해당 [contentprovider% 28)](xref:Android.Content.ContentProvider.AttachInfo*) 메서드가 호출 됩니다. Xamarin Android는 mono를 추가 하 여이에 후크 합니다 *.* 빌드 프로세스 중에 MonoRuntimeProvider *contentprovider* To AndroidManifest. *Mono입니다. AttachInfo ()* 메서드는 Mono 런타임을 프로세스로 로드 하는 작업을 담당 합니다. MonoRuntimeProvider.
이 시점 전에 Mono를 사용 하려는 시도는 실패 합니다. ( *참고*: 이러한 이유로 Mono를 초기화 하기 전에 [응용 프로그램 인스턴스가](xref:Android.App.Application) 만들어지기 때문에 JniHandleOwnership 하위 클래스에서 [(IntPtr,) 생성자](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103)를 제공 해야 하는 형식입니다.)

프로세스 초기화가 완료 되 면 `AndroidManifest.xml` 을 (를) 실행 하 여 작업/서비스/등의 클래스 이름을 찾습니다. 예를 들어 [ /manifest/application/activity/@android:name 특성](https://developer.android.com/guide/topics/manifest/activity-element.html#nm) 은 로드할 활동의 이름을 결정 하는 데 사용 됩니다. 활동의 경우이 유형은 [android. Activity](xref:Android.App.Activity)를 상속 해야 합니다.
지정 된 형식이 클래스를 통해 로드 됩니다 [. forName ()](https://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (형식이 Java 형식 이어야 하며, 따라서 Android 호출 가능 래퍼). Android 호출 가능 래퍼 인스턴스를 만들면 해당 C# 형식의 인스턴스 생성이 트리거됩니다. 그러면 Android에서 [onCreate (번들)](https://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) 를 호출 하 여 해당 [onCreate (번들)](xref:Android.App.Activity.OnCreate*) 이 호출 되 고 경합이 발생 합니다.
