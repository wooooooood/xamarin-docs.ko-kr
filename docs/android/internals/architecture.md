---
title: 아키텍처
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: ea66cda0e2a1935a430c064c9cebd4134d295729
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60954264"
---
# <a name="architecture"></a>아키텍처

Xamarin.Android 응용 프로그램 Mono 실행 환경 내에서 실행합니다.
이 실행 환경 실행-side-by-side Android 런타임 (최신) 가상 컴퓨터를 사용 하 여 합니다. 두 런타임 환경 Linux 커널 위에서 실행 되며 개발자가 기본 시스템에 액세스할 수 있도록 사용자 코드에 다양 한 Api를 노출 합니다. Mono 런타임 C 언어에 기록 됩니다.

사용할 수 있습니다는 [시스템](xref:System), [System.IO](xref:System.IO), [System.Net](xref:System.Net) 및.NET의 나머지 부분 클래스 라이브러리를 기본 Linux 운영 체제 기능에 액세스 합니다.

Android에서 오디오, 그래픽, OpenGL 및 전화 통신와 같은 시스템 기능 대부분은 네이티브 응용 프로그램에 직접 사용할 수 있는만 중 하나에 있는 Android 런타임 Java Api를 통해 노출 되는 [Java](https://developer.xamarin.com/api/namespace/Java.Lang/). * 네임 스페이스 또는 [Android](https://developer.xamarin.com/api/namespace/Android/). * 네임 스페이스입니다. 이 아키텍처는 다음과 같이 대략적으로:

[![커널 위아래.NET/Java + 바인딩 Mono와 최신의 다이어그램](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Xamarin.Android 개발자에 게 (낮은 수준의 액세스)에 대 한 알고 있는.NET Api를 호출 하거나 브리지에 의해 노출 되는 Java Api를 제공 하는 Android 네임 스페이스에서 노출 하는 클래스를 사용 하 여 운영 체제의 다양 한 기능에 액세스 Android 런타임입니다.

Android 클래스 Android 런타임 클래스를 사용 하 여 통신 하는 방법에 대 한 자세한 내용은 참조는 [API 디자인](~/android/internals/api-design.md) 문서.


## <a name="application-packages"></a>애플리케이션 패키지

Android 응용 프로그램 패키지를 사용 하 여 ZIP 컨테이너를 *.apk* 파일 확장명입니다. 다음 내용을 추가 하 여 기본 Android 패키지와 동일한 구조와 레이아웃을가 하는 Xamarin.Android 응용 프로그램 패키지:

-   응용 프로그램 어셈블리 (IL 포함)를 *저장* 내에서 압축 되지 않은 합니다 *어셈블리* 폴더입니다. 릴리스에서 시작 빌드 프로세스 중 합니다 *.apk* 됩니다 *mmap()* 어셈블리 및 프로세스로 ed가 메모리에서 로드 됩니다. 이 어셈블리를 실행 하기 전에 추출할 필요가 없으므로 빠른 응용 프로그램 시작을 허용 합니다.  
-   *참고:* 같은 위치 정보를 어셈블리 [Assembly.Location](xref:System.Reflection.Assembly.Location) 하 고 [Assembly.CodeBase](xref:System.Reflection.Assembly.CodeBase)
    *의존할 수 없습니다* 릴리스 빌드에서 합니다. 고유한 파일 시스템 항목으로 존재 하지 않는 하 고 사용할 수 없는 위치를 갖습니다.


-   Mono 런타임 포함 된 네이티브 라이브러리는 내에 존재 합니다 *.apk* 합니다. Xamarin.Android 응용 프로그램을 원하는 대상 Android 아키텍처에 대 한 네이티브 라이브러리를 예를 들어 포함 해야 합니다 *armeabi* 를 *armeabi-v7a* 하십시오 *x86* 합니다. 적절 한 런타임 라이브러리를 포함 하지 않는 경우 Xamarin.Android 응용 프로그램 플랫폼에서 실행할 수 없습니다.


Xamarin.Android 응용 프로그램을 포함할 수도 *Android 호출 가능 래퍼* Android 관리 되는 코드를 호출할 수 있도록 합니다.



## <a name="android-callable-wrappers"></a>Android 호출 가능 래퍼

- **Android 호출 가능 래퍼** 되는 [JNI](https://en.wikipedia.org/wiki/Java_Native_Interface) Android 런타임에서 관리 코드를 호출 해야 하는 경우 든 지 사용 되는 브리지입니다. Android 호출 가능 래퍼 가상 메서드는 재정의할 수 있습니다 및 Java 인터페이스를 구현할 수 있습니다. 참조 된 [Java 통합 개요](~/android/platform/java-integration/index.md) 자세한 문서.


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>관리 되는 호출 가능 래퍼

관리 되는 호출 가능 래퍼는 관리 되는 코드는 Android 코드를 호출 하 고 가상 메서드를 재정의 하 고 Java 인터페이스를 구현 하기 위한 지원을 제공 해야 하 든 지 사용 되는 JNI 연결 합니다. 전체 [Android](https://developer.xamarin.com/api/namespace/Android/)합니다. * 및 관련된 네임 스페이스는 관리 되는 호출 가능 래퍼를 통해 생성 된 [.jar 바인딩](~/android/platform/binding-java-library/index.md)합니다.
관리 되는 호출 가능 래퍼는 관리 되 고 Android 형식 간 변환 및 JNI 통해 기본 Android 플랫폼 메서드를 호출 하는 일을 담당 합니다.

Java 전역 참조를 통해 액세스할 수 있는 관리 되는 호출 가능 래퍼는 만든 각 합니다 [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) 속성입니다. 전역 참조는 관리 되는 인스턴스 및 Java 인스턴스 간의 매핑을 제공 하는 데 사용 됩니다. 전역 참조는 제한 된 리소스: 에뮬레이터는 대부분의 하드웨어 동시 존재 개 52,000 전역 참조를 허용 하는 동안 한 번에 존재만 2000 전역 참조를 허용 합니다.

전역 참조 생성 및 제거 하는 경우 추적을 설정할 수 있습니다 합니다 [debug.mono.log](~/android/troubleshooting/index.md) 포함 하는 시스템 속성 [gref](~/android/troubleshooting/index.md)합니다.

호출 하 여 전역 참조를 명시적으로 해제할 수 있습니다 [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) 관리 되는 호출 가능 래퍼에 있습니다. Java 인스턴스 및 관리 되는 인스턴스 간 매핑을 제거 되 고 Java 인스턴스 수집 되도록 허용 됩니다. Java 인스턴스에 다시 액세스 하는 관리 코드에서 경우에 대 한 새 관리 되는 호출 가능 래퍼를 만들어질 수 있습니다.

다른 스레드에서 참조에 영향을 줍니다 인스턴스 실수로 인스턴스 삭제와 스레드 간에 공유할 수 있습니다 하는 경우 관리 되는 호출 가능 래퍼 삭제 주의 해야 합니다. 만 최대 보안을 위해 `Dispose()` 를 통해 할당 된 인스턴스 `new` *또는* 메서드에서 있으며 *알고* 항상 될 수 있는 캐시 된 인스턴스와 새 인스턴스를 할당 스레드 간에 공유 되는 우발적 인스턴스를 발생 합니다.



## <a name="managed-callable-wrapper-subclasses"></a>호출 가능 래퍼 서브 클래스를 관리합니다.

관리 되는 호출 가능 래퍼 서브 클래스는 "흥미로운" 모든 응용 프로그램 관련 논리 존재할 수 있는 위치입니다. 여기에 사용자 지정 [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) 서브 클래스 (같은 합니다 [Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) 기본 프로젝트 템플릿에 형식). (이 특히 *Java.Lang.Object* 개라면 서브 클래스 *되지* 포함는 [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) 사용자 지정 특성 또는 [ RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) 됩니다 *false*, 기본값입니다.)

예: 관리 되는 호출 가능 래퍼를 관리 되는 호출 가능 래퍼 서브 클래스를 통해 액세스할 수 있는 전역 참조를 포함 합니다 [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) 속성입니다. 관리 되는 호출 가능 래퍼를 사용 하 여 전역 참조 명시적으로 해제할 수를 호출 하 여 처럼 [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/)합니다.
관리 되는 호출 가능 래퍼를 달리 *기울여야* 으로 이러한 인스턴스를 삭제 하기 전에 수행 해야 *dispose ()* 인스턴스의 현재 진행형 Java 인스턴스 간의 매핑을 중단 됩니다 (의 인스턴스는 Android 호출 가능 래퍼) 및 관리 되는 인스턴스.


### <a name="java-activation"></a>Java 활성화

경우는 [Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md) ACW 생성자는 해당 C# 생성자를 호출 하면, (ACW)은 Java에서 만들어집니다. 에 대 한 ACW 예를 들어 *MainActivity* 호출 하는 기본 생성자가 포함 됩니다 *MainActivity*의 기본 생성자입니다. (이렇게 합니다 *TypeManager.Activate()* ACW 생성자 내에서 호출 합니다.)

결과의 다른 생성자 시그니처를 하나는: 합니다 *(IntPtr, JniHandleOwnership)* 생성자입니다. 합니다 *(IntPtr, JniHandleOwnership)* Java 개체는 관리 코드에 노출 되며 JNI 핸들을 관리 하도록 작성 해야 하는 관리 되는 호출 가능 래퍼를 때마다 생성자가 호출 됩니다. 일반적으로 이렇게 자동으로 합니다.

두 가지 시나리오는 합니다 *(IntPtr, JniHandleOwnership)* 호출 가능 래퍼 관리 되는 서브 클래스에 생성자를 수동으로 제공 해야 합니다.

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) 서브클래싱된 합니다. *응용 프로그램* 는 특수 한; 기본값 *응용 프로그램* 생성자는 *되지* 를 호출 및 [(IntPtr, JniHandleOwnership) 대신 생성자를 제공 해야 합니다 ](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. 기본 클래스 생성자에서 가상 메서드 호출입니다.


(2)는 leaky 추상화 합니다. Java이 든, C#의 경우와 같이 생성자에서 가상 메서드를 호출은 항상 가장 많이 파생 된 메서드 구현을 호출 합니다. 예를 들어 합니다 [TextView (컨텍스트를 확인 하기 위해, int) 생성자](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/) 가상 메서드를 호출 [TextView.getDefaultMovementMethod()](https://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod()),으로 바인딩된는 [ TextView.DefaultMovementMethod 속성](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/)합니다.
따라서 형식을 [LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) (1)로 된 [TextView 하위 클래스입니다](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [TextView.DefaultMovementMethod 재정의](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45), (3) [의 인스턴스를 활성화 XML 통해 클래스](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) 재정의 된 *DefaultMovementMethod* ACW 생성자를 실행 하려면 유익한 전에 하기 전에 발생 하는 속성 호출 되는 C# 생성자 유익한 실행 합니다.

LogTextBox 인스턴스를 인스턴스화하고 지이 통해는 [LogTextView (IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) ACW LogTextBox 인스턴스를 처음 실행 하는 경우 생성자에 다음 호출 하는 코드를 관리 되는 [ (상황에 맞는, IAttributeSet, int) LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) 생성자 *동일한 인스턴스에서* ACW 생성자를 실행 하는 경우.

이벤트의 순서:

1.  레이아웃 XML에 로드 되는 [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41)합니다.

2.  Android 레이아웃 개체 그래프를 인스턴스화하고의 인스턴스를 인스턴스화합니다 *monodroid.apidemo.LogTextBox* 에 대 한 ACW *LogTextBox* 합니다.

3.  합니다 *monodroid.apidemo.LogTextBox* 생성자를 실행 합니다 [android.widget.TextView](https://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) 생성자입니다.

4.  합니다 *TextView* 생성자 호출 *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* 합니다.

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* 호출 *LogTextBox.n_getDefaultMovementMethod()* 를 호출 하는 *TextView.n_GetDefaultMovementMethod()* , 호출 [Java.Lang.Object.GetObject&lt;TextView&gt; (처리, JniHandleOwnership.DoNotTransfer)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) 합니다.

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* 가 이미 해당 C# 확인에 대 한 인스턴스 *처리할* 합니다. 가 반환 됩니다. 이 시나리오에서는 없는, 하므로 *Object.GetObject&lt;T&gt;()* 하나 만들어야 합니다.

7.  *Object.GetObject&lt;T&gt;()* 찾습니다 합니다 *(IntPtr, JniHandleOwneship) LogTextBox* 생성자 호출, 간에 매핑을 만듭니다 *처리* 및 생성된 된 인스턴스를 만든된 인스턴스를 반환 합니다.

8.  *TextView.n_GetDefaultMovementMethod()* 를 호출 하 여 *LogTextBox.DefaultMovementMethod* 속성 getter입니다.

9.  컨트롤이 반환 된 *android.widget.TextView* 실행을 완료 하는 생성자.

10. 합니다 *monodroid.apidemo.LogTextBox* 를 호출 하는 생성자가 실행 *TypeManager.Activate()* 합니다.

11. 합니다 *LogTextBox (컨텍스트에 IAttributeSet, int)* 생성자는 실행 *(7)에서 만든 동일한 인스턴스에서* 합니다.

12. 경우 (IntPtr, JniHandleOwnership) 생성자를 찾을 수 없으면 다음을 System.MissingMethodException](xref:System.MissingMethodException)이 throw 됩니다.

<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>중간 dispose () 호출

JNI 핸들을 해당 C# 인스턴스 사이의 매핑이 있습니다. 이 매핑은 Java.Lang.Object.Dispose()을 중단합니다. Java 활성화 같이 매핑을 연결이 끊어진 후 관리 되는 코드를 입력 하는 JNI 핸들을 하는 경우와 *(IntPtr, JniHandleOwnership)* 생성자를 검사 하 고 호출 됩니다. 생성자가 없으면 예외가 throw 됩니다.

예를 들어 다음 관리 되는 호출 가능 Wraper 하위 클래스입니다.

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

Dispose ()의 인스턴스를 작성 하 고 관리 되는 호출 가능 래퍼를 다시 만들어야 하면:

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

프로그램 중단 됩니다.

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

서브 클래스에 포함 되어는 *(IntPtr, JniHandleOwnership)* 생성자에는 *새* 형식의 인스턴스를 만듭니다. 결과적으로, 새 인스턴스를 그대로 모든 인스턴스 데이터를 "손실" 인스턴스가 표시 됩니다. (참고 값은 null입니다.)

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

만 *dispose ()* 의 Java 개체를 더 이상 사용 되지 것입니다 또는 서브 클래스 데이터가 포함 되지 않은 인스턴스는 알고 있는 경우 및 호출 가능 래퍼 서브 클래스를 관리 되는 *(IntPtr, JniHandleOwnership)* 생성자에 제공 되었습니다.



## <a name="application-startup"></a>응용 프로그램 시작

작업, 서비스, 등 시작 됩니다, Android는 먼저 확인 활동/service/etc 호스트를 실행 중인 프로세스가 이미 있는지 확인 합니다. 새 프로세스가 만들어짐 다음 이러한 프로세스가 있는 경우는 [AndroidManifest.xml](https://developer.android.com/guide/topics/manifest/manifest-intro.html) 를 읽고 지정 된 형식 합니다 [ /manifest/application/@android:name ](https://developer.android.com/guide/topics/manifest/application-element.html#nm) 특성을 로드 하 고 인스턴스화합니다. 다음에 지정 된 모든 형식을 합니다 [ /manifest/application/provider/@android:name ](https://developer.android.com/guide/topics/manifest/provider-element.html#nm) 특성 값은 인스턴스화된을 해당 [ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/) 메서드를 호출 합니다. 이를 추가 하 여 Xamarin.Android 후크를 *mono입니다. MonoRuntimeProvider* *ContentProvider* AndroidManifest.xml 빌드 프로세스 중에 있습니다. *mono입니다. MonoRuntimeProvider.attachInfo()* 메서드는 Mono 런타임을 프로세스로 로드 하는 일을 담당 합니다.
이 시점 전에 Mono를 사용 하려는 모든 시도가 실패 합니다. ( *참고*: 이 인해 서브 클래스 형식 [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) 제공 해야는 [(IntPtr, JniHandleOwnership) 생성자](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103)처럼 응용 프로그램 인스턴스는 전의 Mono를 초기화할 수 있습니다.)

프로세스 초기화가 완료 되 면 `AndroidManifest.xml` 를 참조 하는 활동/service/등에 시작의 클래스 이름을 찾을 수 있습니다. 예를 들어 합니다 [ /manifest/application/activity/@android:name 특성](https://developer.android.com/guide/topics/manifest/activity-element.html#nm) 로드 하기 위한 작업의 이름을 결정 하는 데 사용 됩니다. 작업에 대 한이 형식을 상속 해야 합니다 [android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/)합니다.
지정된 된 형식을 통해 로드 되 [class.forname ()](https://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (형식 Java 되어 있어야 하는 Android 호출 가능 래퍼 이므로 입력)를 인스턴스화합니다. Android 호출 가능 래퍼 인스턴스 생성 해당 C# 형식의 인스턴스 생성을 트리거합니다. 그러면 android는 호출 [Activity.onCreate(Bundle)](https://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) , 해당 하는 것이 그러면 [Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) 호출할 끝 해제를 합니다.
