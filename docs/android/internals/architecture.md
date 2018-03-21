---
title: "아키텍처"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 9834da444032622cc3547e7c99ca3de0e41bb603
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2018
---
# <a name="architecture"></a>아키텍처

Xamarin.Android 응용 프로그램 모노 실행 환경 내에서 실행 합니다.
이 실행 환경 실행--와 함께 Android 런타임 (아트) 키도 록 합니다. 런타임 환경 모두를 기반으로 Linux 커널을 실행 및 개발자가 기본 시스템에 액세스할 수 있도록 하는 사용자 코드에 다양 한 Api를 노출 합니다. 모노 런타임 C 언어에 기록 됩니다.

사용할 수 있습니다는 [시스템](http://msdn.microsoft.com/en-us/library/system.aspx), [System.IO](http://msdn.microsoft.com/en-us/library/system.io.aspx), [System.Net](http://msdn.microsoft.com/en-us/library/system.net.aspx) 및.NET의 나머지 부분 클래스 라이브러리를 원본으로 사용 하는 Linux 운영 체제 기능에 액세스 합니다.

Android에서는 대부분의 오디오, 그래픽, OpenGL 및 전화 통신와 같은 시스템 기능을 사용할 수 없는 직접 네이티브 응용 프로그램만 중 하나에 있는 Android 런타임 Java Api를 통해 제공 됩니다는 [Java](https://developer.xamarin.com/api/namespace/Java.Lang/). * 네임 스페이스 또는 [Android](https://developer.xamarin.com/api/namespace/Android/). * 네임 스페이스입니다. 이 아키텍처는 다음과 같은 대략적인:

[![커널 위아래 T/j + 바인딩 모노 및 아트의 다이어그램](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Xamarin.Android 개발자 (하위 수준 액세스)에 대 한 알고 있는.NET Api를 호출 하거나 브리지에 의해 노출 되는 Java Api에 제공 하는 Android 네임 스페이스에서 노출 된 클래스를 사용 하 여 운영 체제의 다양 한 기능에 액세스 Android 런타임입니다.

Android 클래스 Android 런타임 클래스와 통신 하는 방법에 대 한 자세한 내용은 참조는 [API 디자인](~/android/internals/api-design.md) 문서.


## <a name="application-packages"></a>응용 프로그램 패키지

Android 응용 프로그램 패키지는 ZIP 컨테이너는 *.apk* 파일 확장명입니다. Xamarin.Android 응용 프로그램 패키지 다음 항목이 추가 된 기본 Android 패키지와 같은 구조와 레이아웃을 가진:

-   응용 프로그램 어셈블리 (IL을 포함)는 *저장* 내에서 압축 되지 않은 *어셈블리* 폴더입니다. 릴리스에서 시작 빌드 프로세스 중의 *.apk* 은 *mmap()* 프로세스와 어셈블리에 ed가 메모리에서 로드 됩니다. 그러면 빠른 응용 프로그램 시작으로 어셈블리를 실행 하기 전에 추출할 필요가 없습니다. - *참고:* 어셈블리 위치 정보 등 [Assembly.Location](https://developer.xamarin.com/api/property/System.Reflection.Assembly.Location/) 및 [Assembly.CodeBase](https://developer.xamarin.com/api/property/System.Reflection.Assembly.CodeBase/)
    *에 의존할 수 없습니다* 릴리스에서 작성합니다. 고유한 파일 시스템 항목으로 존재 하지 않는 하 고 사용할 수 없는 위치를 갖습니다.


-   모노 런타임이 포함 된 네이티브 라이브러리는 내의 *.apk* 합니다. Xamarin.Android 응용 프로그램은 예: 원하는/대상 Android 아키텍처에 대 한 네이티브 라이브러리를 포함 해야 *armeabi* , *armeabi v7a* , *x86* 합니다. Xamarin.Android 응용 프로그램에서 적절 한 런타임 라이브러리가 포함 되어 있지 않으면 플랫폼에서 실행할 수 없습니다.


Xamarin.Android 응용 프로그램에도 포함 *Android 호출 가능 래퍼* Android 관리 되는 코드를 호출할 수 있도록 합니다.



## <a name="android-callable-wrappers"></a>Android 호출 가능 래퍼

- **Android 호출 가능 래퍼** 됩니다는 [JNI](http://en.wikipedia.org/wiki/Java_Native_Interface) Android 런타임에서 관리 코드를 호출 해야 하는 경우 든 지 사용 되는 브리지입니다. Android 호출 가능 래퍼 가상 메서드는 재정의할 수 있습니다 및 Java 인터페이스를 구현할 수 있습니다. 참조는 [Java 통합 개요](~/android/platform/java-integration/index.md) doc 더 합니다.


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>관리 되는 호출 가능 래퍼

관리 되는 호출 가능 래퍼는 관리 되는 코드가 Android 코드를 호출 하 고 가상 메서드를 재정의 하 고 Java 인터페이스 구현에 대 한 지원을 제공 해야 하 든 지 사용 되는 JNI 브리지입니다. 전체 [Android](https://developer.xamarin.com/api/namespace/Android/). * 및 관련된 네임 스페이스는 관리 되는 호출 가능 래퍼를 통해 생성 된 [.jar 바인딩](~/android/platform/binding-java-library/index.md)합니다.
관리 되는 호출 가능 래퍼는 JNI 통해 기본 Android 플랫폼 메서드를 호출 하는 관리 되는 / Android 형식 간 변환 하는 일을 담당 합니다.

유지 관리 되는 호출 가능 래퍼를 통해 액세스할 수 있는 Java 전역 참조 생성 각각는 [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) 속성입니다. 전역 참조 Java 인스턴스 및 관리 되는 인스턴스 간의 매핑을 제공 하는 데 사용 됩니다. 전역 참조는 제한 된 리소스: 에뮬레이터 허용 글로벌 참조만 2000 대부분의 하드웨어를 한 번에 존재할 넘는 52,000 전역 참조를 허용 하는 동안 한 번에 존재 합니다.

전역 참조 생성 되 고 삭제 하는 경우를 추적 하려면 설정할 수 있습니다는 [debug.mono.log](~/android/troubleshooting/index.md) 포함 하는 시스템 속성 [gref](~/android/troubleshooting/index.md)합니다.

전역 참조를 호출 하 여 명시적으로 해제할 수 있도록 [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) 관리 되는 호출 가능 래퍼에 있습니다. 이 Java 인스턴스 및 관리 되는 인스턴스 사이의 매핑을 제거 되며 사용 Java 인스턴스를 수집 합니다. 관리 코드에서 Java 인스턴스를 다시 액세스 하 고, 경우에 대 한 새 관리 되는 호출 가능 래퍼 만들어질 수 있습니다.

다른 모든 스레드에 대 한 참조에 영향을 줍니다 인스턴스가 실수로 인스턴스를 삭제 하는 대로 스레드 간에 공유할 수 있습니다 하는 경우 관리 되는 호출 가능 래퍼를 삭제 하는 경우 주의 해야 합니다. 최대 안전을 `Dispose()` 를 통해 할당 된 인스턴스 `new` *또는* 방법 중에서 어떤 있습니다 *알고* 항상 될 수 있는 캐시 된 인스턴스와 새 인스턴스를 할당 실수로 인스턴스 스레드 간에 공유 발생 합니다.



## <a name="managed-callable-wrapper-subclasses"></a>호출 가능 래퍼 하위 클래스를 관리합니다.

관리 되는 호출 가능 래퍼 하위 클래스는 모든 "흥미로운" 응용 프로그램별 논리 상주할 수 있습니다. 여기에 사용자 지정 [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) 서브 클래스 (같은 [Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) 기본 프로젝트 템플릿에서 유형). (모든 이들은 특히 *Java.Lang.Object* 작업할 하위 클래스 *하지* 포함 한 [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) 사용자 지정 특성 또는 [ RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) 은 *false*, 기본값입니다.)

마찬가지로 호출 가능 래퍼를 관리 하는 관리 되는 호출 가능 래퍼 하위 클래스를 통해 액세스할 수 있는 전역 참조를 포함할 수도 [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) 속성입니다. 관리 되는 호출 가능 래퍼와 전역 참조 해제할 수 있도록 명시적으로 호출 하 여 처럼 [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/)합니다.
관리 되는 호출 가능 래퍼 달리 *신중* 로 이러한 인스턴스를 삭제 하기 전에 수행 해야 할 *dispose ()*인스턴스의 ing Java 인스턴스 간의 매핑을 끊긴다는 (의 인스턴스는 Android 호출 가능 래퍼) 및 관리 되는 인스턴스.


### <a name="java-activation"></a>Java 정품 인증

경우는 [Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md) ACW 생성자는 해당 C# 생성자를 호출 하면, (ACW) Java에서 만들어집니다. 에 대 한 ACW 예를 들어 *MainActivity* 호출 하는 기본 생성자가 포함 됩니다 *MainActivity*의 기본 생성자입니다. (이 방식은 *TypeManager.Activate()* ACW 생성자 내에서 호출 합니다.)

결과의 다른 한 생성자 서명이:는 *(IntPtr, JniHandleOwnership)* 생성자입니다. *(IntPtr, JniHandleOwnership)* Java 개체는 관리 코드에 표시 되 고를 JNI 핸들을 관리 하는 관리 되는 호출 가능 래퍼 만들어야 때마다 생성자가 호출 됩니다. 이렇게 하려면 보통 자동으로 합니다.

두 가지 시나리오는는 *(IntPtr, JniHandleOwnership)* 호출 가능 래퍼 관리 되는 하위 클래스에 생성자를 수동으로 제공 해야 합니다.

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) 서브클래싱된 합니다. *응용 프로그램* 은 특수 한; 기본값 *응용 프로그램* 생성자는 *되지* 호출할 수 및 [(IntPtr, JniHandleOwnership) 생성자를 대신 제공 해야 합니다 ](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. 기본 클래스 생성자에서 가상 메서드를 호출 합니다.


(2)는 누설 추상화 합니다. 와 같이 C#, java 생성자에서 가상 메서드를 호출 항상 가장 많이 파생 된 메서드 구현을 호출합니다. 예를 들어는 [TextView (컨텍스트를 확인 하기 위해, int) 생성자](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/) 가상 메서드를 호출 [TextView.getDefaultMovementMethod()](http://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod())는 자동으로 바인딩되기는 [ TextView.DefaultMovementMethod 속성](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/)합니다.
따라서 형식 [LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) (1)로 된 [하위 클래스 TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [TextView.DefaultMovementMethod 재정의](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45), (3) [해당 인스턴스의 활성화 xml로 클래스](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) 재정의 된 *DefaultMovementMethod* 속성 ACW 생성자를 실행 하려면 준 고은 C# 생성자가 실행 되기 전에 발생 하기 전에 호출 됩니다.

LogTextBox 인스턴스를 인스턴스화하고이 지원 통해는 [LogTextView (IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) ACW LogTextBox 인스턴스 처음 실행 하는 경우 생성자는 다음 호출 하는 코드를 관리는 [ LogTextBox (컨텍스트, IAttributeSet, int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) 생성자 *동일한 인스턴스에서* ACW 생성자를 실행 합니다.

이벤트의 순서:

1.  레이아웃 XML에 로드 되는 [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41)합니다.

2.  Android 레이아웃 개체 그래프를 인스턴스화하고의 인스턴스를 만드는 데 *monodroid.apidemo.LogTextBox* 에 대 한 ACW *LogTextBox* 합니다.

3.  *monodroid.apidemo.LogTextBox* 생성자가 실행 된 [android.widget.TextView](http://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) 생성자입니다.

4.  *TextView* 생성자 호출 *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* 합니다.

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* invokes *LogTextBox.n_getDefaultMovementMethod()* , which invokes *TextView.n_GetDefaultMovementMethod()* , which invokes [Java.Lang.Object.GetObject&lt;TextView&gt; (handle, JniHandleOwnership.DoNotTransfer)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) .

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* 를 이미 있는지를 해당 C# 확인에 대 한 인스턴스 *처리* 합니다. 가 반환 됩니다. 이 시나리오에서는 하지 않으므로, 하므로 *Object.GetObject&lt;T&gt;()* 을 생성 해야 합니다.

7.  *Object.GetObject&lt;T&gt;()* 찾습니다는 *LogTextBox (IntPtr, JniHandleOwneship)* 생성자 호출, 간에 매핑을 만드는 *처리* 및 만들어진된 인스턴스에 생성된 된 인스턴스를 반환 합니다.

8.  *TextView.n_GetDefaultMovementMethod()* 호출는 *LogTextBox.DefaultMovementMethod* 속성 getter입니다.

9.  제어가 반환는 *android.widget.TextView* 생성자 실행을 완료 합니다.

10. *monodroid.apidemo.LogTextBox* 호출 하는 생성자가 실행 *TypeManager.Activate()* 합니다.

11. *LogTextBox (컨텍스트, IAttributeSet, int)* 생성자가 실행 *(7)에서 만든 동일한 인스턴스에서* 합니다.

12. ...


경우 (IntPtr, JniHandleOwnership) 생성자를 찾을 수 없는 경우 [System.MissingMethodException](https://developer.xamarin.com/api/type/System.MissingMethodException/) throw 됩니다.


<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>중간 dispose () 호출

JNI 핸들 및 해당 C# 인스턴스 간의 매핑이 있습니다. 이 매핑은 Java.Lang.Object.Dispose()을 중단합니다. Java 활성화 같이 JNI 핸들이 매핑을 손상 된 후 관리 되는 코드를 입력, 및 *(IntPtr, JniHandleOwnership)* 생성자를 확인 호출 됩니다. 생성자가 존재 하지 않는 경우 예외가 throw 됩니다.

예를 들어 다음과 같은 관리 되는 호출 가능 Wraper 하위 클래스를 가정합니다.

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

Dispose ()의 인스턴스를 작성 하 고 다시 만들어야 관리 되는 호출 가능 래퍼 발생:

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

프로그램 제거 됩니다.

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

하위 클래스에 포함 하는 경우는 *(IntPtr, JniHandleOwnership)* 생성자는 *새* 형식의 인스턴스를 만듭니다. 결과적으로, 새 인스턴스를 그대로 인스턴스가 "손실 되 면" 모든 인스턴스 데이터에 표시 됩니다. (참고 값은 null입니다.)

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

만 *dispose ()* 의 호출 가능 래퍼 하위 클래스를 관리 되는 Java 개체가 더 이상 사용 되지 것입니다 또는 하위 클래스 인스턴스 데이터가 없는 알고 및 *(IntPtr, JniHandleOwnership)* 생성자를 제공 했습니다.



## <a name="application-startup"></a>응용 프로그램 시작

활동, 서비스, 등에 시작 됩니다, Android는 먼저 확인 작업/서비스/등을 호스트 하는 실행 되는 프로세스 이미 있는지 확인 하려면. 해당 프로세스가 없습니다. 존재 하는 새 프로세스를 만들 경우는 [AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-intro.html) 읽고 지정 된 형식과 [ /manifest/application/@android:name ](http://developer.android.com/guide/topics/manifest/application-element.html#nm) 특성을 로드 하 고 인스턴스화합니다. 다음,에 지정 된 모든 형식을는 [ /manifest/application/provider/@android:name ](http://developer.android.com/guide/topics/manifest/provider-element.html#nm) 특성 값은 인스턴스화을 자신의 [ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/) 메서드를 호출 합니다. 추가 하 여이 Xamarin.Android 후크를 *모노 합니다. MonoRuntimeProvider* *ContentProvider* AndroidManifest.xml 빌드 프로세스 중에 있습니다. *모노 합니다. MonoRuntimeProvider.attachInfo()* 메서드는 모노 런타임 프로세스에 로드 합니다.
이 앞서 모노 사용 하려는 모든 시도가 실패 합니다. ( *참고*:이 인해 어떤 하위 클래스 유형 [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) 을 제공 해야는 [(IntPtr, JniHandleOwnership) 생성자](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103)와 마찬가지로 응용 프로그램 인스턴스 전의 모노를 초기화할 수 있습니다.)

프로세스 초기화가 완료 되 면 `AndroidManifest.xml` 를 참조 하는 활동/service/등 시작할의 클래스 이름을 찾을 수 있습니다. 예를 들어는 [ /manifest/application/activity/@android:name 특성](http://developer.android.com/guide/topics/manifest/activity-element.html#nm) 활동을의 이름을 결정 하는 데 사용 됩니다. 작업에 대 한이 형식이 상속 해야 [android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/)합니다.
지정 된 형식을 통해 로드 되 [class.forname ()](http://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (유형을 Java 되도록 해야 Android 호출 가능 래퍼 따라서 입력), 다음 인스턴스화됩니다. Android 호출 가능 래퍼 인스턴스 생성을 해당 C# 형식의 인스턴스를 만드는 과정을 트리거합니다. 그러면 android는 호출 [Activity.onCreate(Bundle)](http://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) , 해당 발생 [Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) 호출 되는 경쟁 잘 해제 합니다.
