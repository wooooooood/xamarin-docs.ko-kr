---
title: JNI 및 Xamarin.Android 사용
description: Xamarin.Android를 사용하여 Java 대신 C#으로 Android 앱을 작성할 수 있습니다. 일부 어셈블리는 Mono.Android.dll 및 Mono.Android.GoogleMaps.dll을 비롯한 Java 라이브러리에 대한 바인딩을 제공하는 Xamarin.Android가 제공됩니다. 그러나 사용할 수 있는 모든 Java 라이브러리에 대해 바인딩이 제공되는 것은 아니며 제공되는 바인딩이 모든 Java 형식 및 멤버를 바인딩할 수 있는 것은 아닙니다. 바인딩되지 않은 Java 형식 및 멤버를 사용하려면 JNI(Java Native Interface)를 사용할 수 있습니다. 이 문서에서는 JNI를 사용하여 Xamarin.Android 애플리케이션에서 Java 형식 및 멤버와 상호 작용하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 00c9c2e9f39943960d35c30602935ed109639cf4
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84567736"
---
# <a name="working-with-jni-and-xamarinandroid"></a>JNI 및 Xamarin.Android 사용

_Xamarin.Android를 사용하여 Java 대신 C#으로 Android 앱을 작성할 수 있습니다. 일부 어셈블리는 Mono.Android.dll 및 Mono.Android.GoogleMaps.dll을 비롯한 Java 라이브러리에 대한 바인딩을 제공하는 Xamarin.Android가 제공됩니다. 그러나 사용할 수 있는 모든 Java 라이브러리에 대해 바인딩이 제공되는 것은 아니며 제공되는 바인딩이 모든 Java 형식 및 멤버를 바인딩할 수 있는 것은 아닙니다. 바인딩되지 않은 Java 형식 및 멤버를 사용하려면 JNI(Java Native Interface)를 사용할 수 있습니다. 이 문서에서는 JNI를 사용하여 Xamarin.Android 애플리케이션에서 Java 형식 및 멤버와 상호 작용하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

Java 코드를 호출하는 MCW(관리형 호출 가능 래퍼)를 만드는 것은 항상 필요하거나 항상 가능한 것은 아닙니다. 대부분의 경우 "인라인" JNI는 완전히 허용되며 바인딩되지 않은 Java 멤버의 일회용 사용에 유용합니다. JNI를 사용하여 전체 .jar 바인딩을 생성하는 것보다 Java 클래스에 대해 단일 메서드를 호출하는 것이 더 간단합니다.

Xamarin.Android는 Android의 `android.jar` 라이브러리에 대한 바인딩을 제공하는 `Mono.Android.dll` 어셈블리를 제공합니다. `Mono.Android.dll` 내에 없는 형식 및 멤버와 `android.jar` 내에 없는 형식은 수동으로 바인딩하여 사용할 수 있습니다. Java 형식 및 멤버를 바인딩하려면 **JNI**(**Java Native Interface**)를 사용하여 형식을 조회하고, 필드를 읽거나 쓰고, 메서드를 호출합니다.

Xamarin.Android의 JNI API는 개념적으로 .NET의 `System.Reflection` API와 매우 유사합니다. 이 API를 통해 이름별로 형식 및 멤버를 조회하고, 필드 값을 읽거나 쓰고, 메서드를 호출할 수 있습니다. JNI 및 `Android.Runtime.RegisterAttribute` 사용자 지정 특성을 사용하여 재정의를 지원하기 위해 바인딩할 수 있는 가상 메서드를 선언할 수 있습니다. C#에서 구현할 수 있도록 인터페이스를 바인딩할 수 있습니다.

이 문서에서는 다음에 대해 설명합니다.

- JNI가 형식을 참조하는 방법
- 필드를 조회하고, 읽고, 쓰는 방법
- 메서드를 조회 및 호출하는 방법
- 관리 코드에서 재정의할 수 있도록 가상 메서드를 노출하는 방법
- 인터페이스를 노출하는 방법

## <a name="requirements"></a>요구 사항

[Android.Runtime.JNIEnv 네임스페이스](xref:Android.Runtime.JNIEnv)를 통해 노출되는 JNI는 모든 버전의 Xamarin.Android에서 사용할 수 있습니다.
Java 형식 및 인터페이스를 바인딩하려면 Xamarin.Android 4.0 이상을 사용해야 합니다.

## <a name="managed-callable-wrappers"></a>관리형 호출 가능 래퍼

**MCW**(**관리형 호출 가능 래퍼**)는 클라이언트 C# 코드가 JNI의 기본 복잡성을 걱정하지 않아도 되도록 모든 JNI 기계를 래핑하는 Java 클래스 또는 인터페이스의 ‘바인딩’입니다. 대부분의 `Mono.Android.dll`은 관리형 호출 가능 래퍼로 구성됩니다.

관리형 호출 가능 래퍼는 다음 두 가지 용도로 사용됩니다.

1. 클라이언트 코드가 기본적인 복잡성을 알 필요가 없도록 JNI 사용을 캡슐화합니다.
1. Java 형식을 서브클래싱하고 Java 인터페이스를 구현할 수 있도록 합니다.

첫 번째 목적은 소비자가 간단한 관리형 클래스 세트를 사용하도록 편리성을 제공하고 복잡성을 캡슐화하는 것입니다. 이렇게 하려면 이 문서의 뒷부분에 설명된 대로 다양한 [JNIEnv](xref:Android.Runtime.JNIEnv) 멤버를 사용해야 합니다. 관리형 호출 가능 래퍼가 반드시 엄격할 필요는 없습니다. 즉, "인라인" JNI 사용은 완벽하게 허용되며 바인딩되지 않은 Java 멤버의 일회용 사용에 유용합니다. 서브클래싱 및 인터페이스 구현을 사용하려면 관리형 호출 가능 래퍼를 사용해야 합니다.

## <a name="android-callable-wrappers"></a>Android 호출 가능 래퍼

Android 런타임(ART)이 관리 코드를 호출해야 할 때마다 ACW(Android 호출 가능 래퍼)가 필요합니다. 해당 래퍼는 런타임에 ART에 클래스를 등록할 방법이 없으므로 필요합니다.
(특히 [DefineClass](https://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) JNI 함수는 Android 런타임에서 지원되지 않습니다. 따라서 Android 호출 가능 래퍼는 런타임 형식 등록 지원이 부족합니다.

Android 코드에서 관리 코드에 재정의되거나 구현되는 가상 또는 인터페이스 메서드를 실행해야 할 때마다 Xamarin.Android는 이 메서드가 적절한 관리형 형식으로 디스패치되도록 Java 프록시를 제공해야 합니다. 해당 Java 프록시 형식은 관리형 형식과 “동일한” 기본 클래스 및 Java 인터페이스 목록이 있는 Java 코드로, 동일한 생성자를 구현하고, 재정의된 기본 클래스 및 인터페이스 메서드를 선언합니다.

Android 호출 가능 래퍼는 [빌드 프로세스](~/android/deploy-test/building-apps/build-process.md) 중에 **monodroid.exe** 프로그램에서 생성되며, (직접 또는 간접적으로) [Java.Lang.Object](xref:Java.Lang.Object)를 상속하는 모든 형식에 대해 생성됩니다.

### <a name="implementing-interfaces"></a>인터페이스 구현

Android 인터페이스(예: [Android.Content.IComponentCallbacks](xref:Android.Content.IComponentCallbacks))를 구현해야 하는 경우가 있습니다.

모든 Android 클래스 및 인터페이스는 [Android.Runtime.IJavaObject](xref:Android.Runtime.IJavaObject) 인터페이스를 확장합니다. 따라서 모든 Android 형식은 `IJavaObject`를 구현해야 합니다.
Xamarin.Android는 `IJavaObject`를 사용하여 지정된 관리형 형식에 대한 Java 프록시(Android 호출 가능 래퍼)를 Android에 제공합니다. **monodroid.exe**는 `Java.Lang.Object` 서브클래스(`IJavaObject`를 구현해야 함)만 찾으므로 `Java.Lang.Object`를 서브클래싱하여 관리 코드에서 인터페이스를 구현하는 방법을 제공합니다. 예를 들어:

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {
    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig) {
        // implementation goes here...
    }
    public void OnLowMemory () {
        // implementation goes here...
    }
}
```

### <a name="implementation-details"></a>구현 세부 정보

‘이 문서의 나머지 부분에서는 예고 없이 변경되는 구현 세부 정보를 제공합니다’(개발자가 내부적으로 진행되는 작업에 대해 궁금해할 수 있으므로 여기에 제공됨).

다음과 같은 C# 소스를 예를 들 수 있습니다.

```csharp
using System;
using Android.App;
using Android.OS;

namespace Mono.Samples.HelloWorld
{
    public class HelloAndroid : Activity
    {
        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);
            SetContentView (R.layout.main);
        }
    }
}
```

**mandroid.exe** 프로그램은 다음 Android 호출 가능 래퍼를 생성합니다.

```java
package mono.samples.helloWorld;

public class HelloAndroid extends android.app.Activity {
    static final String __md_methods;
    static {
        __md_methods =
            "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" +
            "";
        mono.android.Runtime.register (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                HelloAndroid.class,
                __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                "", this, new java.lang.Object[] { });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

기본 클래스는 유지되고 관리 코드 내에서 재정의되는 각 메서드에 대해 네이티브 메서드 선언이 제공됩니다.

### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute 및 ExportFieldAttribute

일반적으로 Xamarin.Android는 ACW를 구성하는 Java 코드를 자동으로 생성합니다. 이 생성은 클래스가 Java 클래스에서 파생되고 기존 Java 메서드를 재정의하는 경우 클래스 및 메서드 이름을 기준으로 합니다. 그러나 일부 시나리오에서는 아래에 설명된 대로 코드 생성이 적절하지 않습니다.

- Android는 레이아웃 XML 특성에서 작업 이름을 지원합니다(예: [android:onClick](xref:Android.Views.View.IOnClickListener.OnClick*) XML 특성). 지정된 경우 확장된 뷰 인스턴스가 Java 메서드를 조회하려고 합니다.

- [java.io.Serializable](https://developer.android.com/reference/java/io/Serializable.html) 인터페이스에는 `readObject` 및 `writeObject` 메서드가 필요합니다. 해당 메서드는 이 인터페이스의 멤버가 아니므로 해당하는 관리형 구현에서 해당 메서드를 Java 코드에 노출하지 않습니다.

- [android.os.Parcelable](xref:Android.OS.Parcelable) 인터페이스에 따르면 구현 클래스에 `Parcelable.Creator` 형식의 정적 필드 `CREATOR`가 있어야 합니다. 생성된 Java 코드에는 몇 가지 명시적 필드가 필요합니다. 표준 시나리오를 사용하여 관리 코드의 Java 코드에 필드를 출력할 방법이 없습니다.

코드 생성은 임의 이름으로 임의 Java 메서드를 생성하는 솔루션을 제공하지 않으므로 Xamarin.Android 4.2부터 위의 시나리오에 대한 솔루션을 제공하기 위해 [ExportAttribute](xref:Java.Interop.ExportAttribute) 및 [ExportFieldAttribute](xref:Java.Interop.ExportFieldAttribute)가 도입되었습니다. 다음과 같은 두 특성이 `Java.Interop` 네임스페이스에 있습니다.

- `ExportAttribute` &ndash; Java에서 명시적 "throw"를 제공하기 위해 메서드 이름과 예상되는 예외 형식을 지정합니다. 이 메서드는 다른 메서드에서 사용되는 경우 디스패치 코드를 생성하는 Java 메서드를 관리형 메서드에 대한 해당 JNI 호출로 "내보냅니다". `android:onClick` 및 `java.io.Serializable`과 함께 사용할 수 있습니다.

- `ExportFieldAttribute` &ndash; 필드 이름을 지정합니다. 필드 이니셜라이저로 작동하는 메서드에 상주합니다. `android.os.Parcelable`과 함께 사용할 수 있습니다.

#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>ExportAttribute 및 ExportFieldAttribute 문제 해결

- **Mono.Android.Export.dll** 누락으로 인해 패키징이 실패합니다. 코드 또는 종속 라이브러리의 일부 메서드에 `ExportAttribute` 또는 `ExportFieldAttribute`를 사용하는 경우 **Mono.Android.Export.dll**을 추가해야 합니다. 이 어셈블리는 Java의 콜백 코드를 지원하기 위해 격리되어 있습니다. 이로 인해 애플리케이션 크기가 증가하므로 **Mono.Android.dll**과는 별개입니다.

- 릴리스 빌드에서는 내보내기 방법에 대해 `MissingMethodException`이 발생하고, 릴리스 빌드에서는 내보내기 방법에 대해 `MissingMethodException`이 발생합니다. (이 문제는 최신 버전의 Xamarin.Android에서 해결되었습니다.)

### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` 및 `ExportFieldAttribute`는 Java 런타임 코드에서 사용할 수 있는 기능을 제공합니다. 이 런타임 코드는 해당 특성에 따라 생성된 JNI 메서드를 통해 관리 코드에 액세스합니다. 결과적으로, 관리형 메서드가 바인딩하는 기존 Java 메서드는 없습니다. 따라서 Java 메서드는 관리형 메서드 시그니처에서 생성됩니다.

그러나 이 경우는 완전한 결정 요인이 아닙니다. 다음과 같은 관리형 형식과 Java 형식 간의 일부 고급 매핑이 여기에 해당합니다.

- InputStream
- OutputStream
- XmlPullParser
- XmlResourceParser

내보낸 메서드에 해당 형식이 필요한 경우 `ExportParameterAttribute`를 사용하여 해당 매개 변수 또는 반환 값에 형식을 명시적으로 지정해야 합니다.

### <a name="annotation-attribute"></a>주석 특성

Xamarin.Android 4.2에서는 `IAnnotation` 구현 형식을 특성(System.Attribute)으로 변환하고, Java 래퍼에 주석 생성에 대한 지원을 추가했습니다.

즉, 다음과 같은 방향성이 변경됨을 의미합니다.

- 바인딩 생성기는 `java.Lang.Deprecated`에서 `Java.Lang.DeprecatedAttribute`를 생성합니다(관리 코드에서는 `[Obsolete]`여야 함).

- 그렇다고 해서 기존 `Java.Lang.Deprecated` 클래스가 사라지게 되는 것은 아닙니다. 해당 Java 기반 개체는 여전히 일반적인 Java 개체로 사용할 수 있습니다(해당 사용량이 있는 경우). `Deprecated` 및 `DeprecatedAttribute` 클래스가 생성됩니다.

- `Java.Lang.DeprecatedAttribute` 클래스가 `[Annotation]`으로 표시됩니다. 이 `[Annotation]` 특성에서 상속된 사용자 지정 특성이 있는 경우 msbuild 작업은 ACW(Android 호출 가능 래퍼)에서 해당 사용자 지정 특성(@Deprecated)에 대한 Java 주석을 생성합니다.

- 주석은 클래스, 메서드 및 내보낸 필드(관리 코드의 메서드)로 생성될 수 있습니다.

포함하는 클래스(주석 처리된 클래스 자체 또는 주석 처리된 멤버를 포함하는 클래스)가 등록되지 않은 경우 주석을 포함하는 전체 Java 클래스 소스가 전혀 생성되지 않습니다. 메서드의 경우 `ExportAttribute`를 지정하여 메서드를 명시적으로 생성하고 주석을 지정할 수 있습니다. 또한 이것이 Java 주석 클래스 정의를 "생성"하는 기능은 아닙니다. 즉, 특정 주석에 대한 사용자 지정 관리형 특성을 정의하는 경우 해당 Java 주석 클래스가 포함된 다른 .jar 라이브러리를 추가해야 합니다. 주석 형식을 정의하는 Java 소스 파일을 추가하는 것만으로는 충분하지 않습니다. Java 컴파일러는 **apt**와 동일한 방식으로 작동하지 않습니다.

또한 다음과 같은 제한 사항이 적용됩니다.

- 이 변환 프로세스는 지금까지 `@Target` 주석을 주석 형식으로 고려하지 않습니다.

- 속성에 대한 특성은 작동하지 않습니다. 대신 속성 getter 또는 setter에 대한 특성을 사용합니다.

## <a name="class-binding"></a>클래스 바인딩

클래스를 바인딩하는 것은 기본 Java 형식의 호출을 단순화하기 위해 관리형 호출 가능 래퍼를 작성하는 것을 의미합니다.

C#에서 재정의할 수 있도록 가상 및 추상 메서드를 바인딩하려면 Xamarin.Android 4.0이 필요합니다. 그러나 재정의를 지원하지 않고도 모든 버전의 Xamarin.Android는 비가상 메서드, 정적 메서드 또는 가상 메서드를 바인딩할 수 있습니다.

바인딩에는 일반적으로 다음과 같은 항목이 포함됩니다.

- [바인딩되는 Java 형식에 대한 JNI 핸들](#_Looking_up_Java_Types)

- [바인딩된 각 필드의 JNI 필드 ID 및 속성](#_Instance_Fields)

- [각 바인딩된 각 메서드의 JNI 메서드 ID 및 메서드](#_Instance_Methods)

- 서브클래싱이 필요하면 해당 형식에는 [RegisterAttribute.DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw)가 `true`로 설정된 형식 선언에 대해 [RegisterAttribute](xref:Android.Runtime.RegisterAttribute)사용자 지정 특성이 있어야 합니다.

### <a name="declaring-type-handle"></a>선언 형식 핸들

필드 및 메서드 조회 메서드에는 선언 형식을 참조하는 개체 참조가 필요합니다. 규칙에 따라 이 참조는 `class_ref` 필드에 저장됩니다.

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

`CLASS` 토큰에 대한 자세한 내용은 [JNI 형식 참조](#_JNI_Type_References) 섹션을 참조하세요.

### <a name="binding-fields"></a>바인딩 필드

Java 필드는 C# 속성으로 노출됩니다. 예를 들어, Java 필드 [java.lang.System.in](https://developer.android.com/reference/java/lang/System.html#in)은 C# 속성 [Java.Lang.JavaSystem.In](xref:Java.Lang.JavaSystem.In)으로 바인딩됩니다.
또한 JNI는 정적 필드와 인스턴스 필드 간을 구분하므로 속성을 구현할 때 다른 메서드가 사용됩니다.

필드 바인딩에는 다음 세 가지 메서드 세트가 사용됩니다.

1. *get field id* 메서드. *get field id* 메서드는 *get field value* 및 *set field value* 메서드에서 사용하는 필드 핸들을 반환하는 일을 담당합니다. 필드 ID를 가져오려면 선언 형식, 필드 이름 및 필드의 [JNI 형식 시그니처](#JNI_Type_Signatures)를 알아야 합니다.

1. *get field value* 메서드. 해당 메서드는 필드 핸들이 필요하며 Java에서 필드의 값을 읽습니다.
    사용할 메서드는 필드의 형식에 따라 달라집니다.

1. *set field value* 메서드 해당 메서드는 필드 핸들이 필요하며 Java 내에서 필드의 값을 작성합니다. 사용할 메서드는 필드의 형식에 따라 달라집니다.

[정적 필드](#_Static_Fields)는 [JNIEnv.GetStaticFieldID](xref:Android.Runtime.JNIEnv.GetStaticMethodID*), `JNIEnv.GetStatic*Field` 및 [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) 메서드를 사용합니다.

 [인스턴스 필드](#_Instance_Fields)는 [JNIEnv.GetFieldID](xref:Android.Runtime.JNIEnv.GetFieldID*), `JNIEnv.Get*Field` 및 [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) 메서드를 사용합니다.

예를 들어 정적 속성 `JavaSystem.In`은 다음과 같이 구현할 수 있습니다.

```csharp
static IntPtr in_jfieldID;
public static System.IO.Stream In
{
    get {
        if (in_jfieldId == IntPtr.Zero)
            in_jfieldId = JNIEnv.GetStaticFieldID (class_ref, "in", "Ljava/io/InputStream;");
        IntPtr __ret = JNIEnv.GetStaticObjectField (class_ref, in_jfieldId);
        return InputStreamInvoker.FromJniHandle (__ret, JniHandleOwnership.TransferLocalRef);
    }
}
```

참고: [InputStreamInvoker.FromJniHandle](xref:Android.Runtime.InputStreamInvoker.FromJniHandle*)을 사용하여 JNI 참조를 `System.IO.Stream` 인스턴스로 변환하고 [JNIEnv. GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*)에서 로컬 참조를 반환하기 때문에 `JniHandleOwnership.TransferLocalRef`를 사용합니다.

많은 [Android.Runtime](xref:Android.Runtime) 형식에는 JNI 참조를 원하는 형식으로 변환하는 `FromJniHandle` 메서드가 있습니다.

### <a name="method-binding"></a>메서드 바인딩

Java 메서드는 C# 메서드 및 C# 속성으로 노출됩니다. 예를 들어 Java 메서드 [java.lang.Runtime.runFinalizersOnExit](https://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) 메서드는 [Java.Lang.Runtime.RunFinalizersOnExit](xref:Java.Lang.Runtime.RunFinalizersOnExit*) 메서드로 바인딩되고 [java.lang.Object.getClass](https://developer.android.com/reference/java/lang/Object.html#getClass) 메서드는 [Java.Lang.Object.Class](xref:Java.Lang.Object.Class) 속성으로 바인딩됩니다.

메서드 호출은 다음과 같은 두 단계로 진행됩니다.

1. 호출할 메서드에 대한 *get method id* *get method id* 메서드는 메서드 호출 메서드에서 사용되는 메서드 핸들을 반환합니다. 메서드 ID를 가져오려면 선언 형식, 메서드의 이름 및 메서드의 [JNI 형식 시그니처](#JNI_Type_Signatures)를 알아야 합니다.

1. 메서드를 호출합니다.

필드와 마찬가지로, 메서드 ID를 가져오고 메서드를 호출하는 데 사용하는 메서드는 정적 메서드와 인스턴스 메서드 사이에서 다릅니다.

[정적 메서드](#_Static_Methods_1)는 [JNIEnv.GetStaticMethodID()](xref:Android.Runtime.JNIEnv.GetStaticMethodID*)를 사용하여 메서드 ID를 조회하고, 호출을 위해 `JNIEnv.CallStatic*Method` 메서드 패밀리를 사용합니다.

[인스턴스 메서드](#_Instance_Methods)는 [JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*)를 사용하여 메서드 ID를 조회하고, 호출을 위해 `JNIEnv.Call*Method` 및 `JNIEnv.CallNonvirtual*Method` 메서드 패밀리를 사용합니다.

메서드 바인딩은 단순한 메서드 호출 이상입니다. 메서드 바인딩에는 메서드를 재정의하거나(추상 메서드 및 최종이 아닌 메서드) 구현하는(인터페이스 메서드) 기능도 포함되어 있습니다. [지원 상속, 인터페이스](#_Supporting_Inheritance,_Interfaces_1) 섹션은 가상 메서드 및 인터페이스 메서드를 지원하는 복잡성에 대해 설명합니다.

<a name="_Static_Methods_1"></a>

#### <a name="static-methods"></a>정적 메서드

정적 메서드를 바인딩할 때는 `JNIEnv.GetStaticMethodID`를 사용하여 메서드 핸들을 가져온 다음, 메서드의 반환 형식에 따라 적절한 `JNIEnv.CallStatic*Method` 메서드를 사용합니다. 다음은 [Runtime.getRuntime](https://developer.android.com/reference/java/lang/Runtime.html#getRuntime()) 메서드에 대한 바인딩 예제입니다.

```csharp
static IntPtr id_getRuntime;

[Register ("getRuntime", "()Ljava/lang/Runtime;", "")]
public static Java.Lang.Runtime GetRuntime ()
{
    if (id_getRuntime == IntPtr.Zero)
        id_getRuntime = JNIEnv.GetStaticMethodID (class_ref,
                "getRuntime", "()Ljava/lang/Runtime;");

    return Java.Lang.Object.GetObject<Java.Lang.Runtime> (
            JNIEnv.CallStaticObjectMethod  (class_ref, id_getRuntime),
            JniHandleOwnership.TransferLocalRef);
}
```

`id_getRuntime` 정적 필드에 메서드 핸들을 저장합니다. 이 작업은 성능 최적화이므로 모든 호출에서 메서드 핸들을 조회할 필요가 없습니다. 해당 방식으로 메서드 핸들을 캐시할 필요는 없습니다. 일단 메서드 핸들을 가져오고 나면 [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*)를 사용하여 메서드를 호출합니다. `JNIEnv.CallStaticObjectMethod`는 반환된 Java 인스턴스의 핸들을 포함하는 `IntPtr`을 반환합니다.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object.GetObject*)는 Java 핸들을 강력한 형식의 개체 인스턴스로 변환하는 데 사용됩니다.

#### <a name="non-virtual-instance-method-binding"></a>비가상 인스턴스 메서드 바인딩

`final` 인스턴스 메서드 또는 재정의가 필요하지 않은 인스턴스 메서드를 바인딩할 때는 `JNIEnv.GetMethodID`를 사용하여 메서드 핸들을 가져온 다음, 메서드의 반환 형식에 따라 적절한 `JNIEnv.Call*Method` 메서드를 사용합니다. 다음은 `Object.Class` 속성에 대한 바인딩 예제입니다.

```csharp
static IntPtr id_getClass;
public Java.Lang.Class Class {
    get {
        if (id_getClass == IntPtr.Zero)
            id_getClass = JNIEnv.GetMethodID (class_ref, "getClass", "()Ljava/lang/Class;");
        return Java.Lang.Object.GetObject<Java.Lang.Class> (
                JNIEnv.CallObjectMethod (Handle, id_getClass),
                JniHandleOwnership.TransferLocalRef);
    }
}
```

`id_getClass` 정적 필드에 메서드 핸들을 저장합니다.
이 작업은 성능 최적화이므로 모든 호출에서 메서드 핸들을 조회할 필요가 없습니다. 해당 방식으로 메서드 핸들을 캐시할 필요는 없습니다. 일단 메서드 핸들을 가져오고 나면 [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*)를 사용하여 메서드를 호출합니다. `JNIEnv.CallStaticObjectMethod`는 반환된 Java 인스턴스의 핸들을 포함하는 `IntPtr`을 반환합니다.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object.GetObject*)는 Java 핸들을 강력한 형식의 개체 인스턴스로 변환하는 데 사용됩니다.

### <a name="binding-constructors"></a>바인딩 생성자

생성자는 이름이 `"<init>"`인 Java 메서드입니다. Java 인스턴스 메서드와 마찬가지로 `JNIEnv.GetMethodID`는 생성자 핸들을 조회하는 데 사용됩니다. Java 메서드와 달리, [JNIEnv.NewObject](xref:Android.Runtime.JNIEnv.NewObject*) 메서드는 생성자 메서드 핸들을 호출하는 데 사용됩니다. `JNIEnv.NewObject`의 반환 값은 다음과 같은 JNI 로컬 참조입니다.

```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

일반적으로 클래스 바인딩은 [Java.Lang.Object](xref:Java.Lang.Object)를 서브클래싱합니다.
`Java.Lang.Object`를 서브클래싱할 때 추가 의미 체계가 작동합니다. 즉, `Java.Lang.Object` 인스턴스는 `Java.Lang.Object.Handle` 속성을 통해 Java 인스턴스에 대한 전역 참조를 유지합니다.

1. `Java.Lang.Object` 기본 생성자는 Java 인스턴스를 할당합니다.

1. 형식에 `RegisterAttribute`가 있고 `RegisterAttribute.DoNotGenerateAcw`가 `true`이면 `RegisterAttribute.Name` 형식의 인스턴스는 기본 생성자를 통해 생성됩니다.

1. 그러지 않으면 `this.GetType`에 해당하는 ACW([Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md))가 기본 생성자를 통해 인스턴스화됩니다. Android 호출 가능 래퍼는 `RegisterAttribute.DoNotGenerateAcw`가 `true`으로 설정되지 않은 모든 `Java.Lang.Object` 서브클래스에 대해 패키지 생성 중에 생성됩니다.

클래스 바인딩이 아닌 형식의 경우 이것은 예상되는 의미 체계입니다. 즉, `Mono.Samples.HelloWorld.HelloAndroid` C# 인스턴스를 인스턴스화하면 생성된 Android 호출 가능 래퍼인 Java `mono.samples.helloworld.HelloAndroid` 인스턴스를 생성해야 합니다.

클래스 바인딩의 경우 Java 형식에 기본 생성자가 포함되어 있거나 다른 생성자를 호출하지 않아도 되는 경우 이것이 올바른 동작일 수 있습니다. 그러지 않으면 다음 작업을 수행하는 생성자를 제공해야 합니다.

1. 기본 `Java.Lang.Object` 생성자 대신, [Java.Lang.Object(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object#ctor*) 호출. 이 호출은 새 Java 인스턴스가 생성되지 않도록 하는 데 필요합니다.

1. Java 인스턴스를 만들기 전에 [Java.Lang.Object.Handle](xref:Java.Lang.Object.Handle)의 값을 확인하세요. `Object.Handle` 속성에는 Android 호출 가능 래퍼가 Java 코드에서 생성된 경우 `IntPtr.Zero` 이외의 값이 지정되고, 만들어진 Android 호출 가능 래퍼 인스턴스를 포함하도록 클래스 바인딩이 생성됩니다. 예를 들어, Android가 `mono.samples.helloworld.HelloAndroid` 인스턴스를 만들 때 Android 호출 가능 래퍼가 먼저 생성되고, Java `HelloAndroid` 생성자는 생성자를 실행하기 전에 `Object.Handle` 속성을 Java 인스턴스로 설정하여 해당 `Mono.Samples.HelloWorld.HelloAndroid` 형식의 인스턴스를 만듭니다.

1. 현재 런타임 형식이 선언 형식과 다른 경우 해당하는 Android 호출 가능 래퍼의 인스턴스를 만들고 [Object.SetHandle](xref:Java.Lang.Object.SetHandle*)를 사용하여 [JNIEnv.CreateInstance](xref:Android.Runtime.JNIEnv.CreateInstance*)에서 반환된 핸들을 저장해야 합니다.

1. 현재 런타임 형식이 선언 형식과 같은 경우 Java 생성자를 호출하고 [Object.SetHandle](xref:Java.Lang.Object.SetHandle*)를 사용하여 `JNIEnv.NewInstance`에서 반환된 핸들을 저장합니다.

예를 들어 [java.lang.Integer(int)](https://developer.android.com/reference/java/lang/Integer.html#Integer(int)) 생성자를 살펴보겠습니다. 다음으로 바인딩됩니다.

```csharp
// Cache the constructor's method handle for later use
static IntPtr id_ctor_I;

// Need [Register] for subclassing
// RegisterAttribute.Name is always ".ctor"
// RegisterAttribute.Signature is tye JNI type signature of constructor
// RegisterAttribute.Connector is ignored; use ""
[Register (".ctor", "(I)V", "")]
public Integer (int value)
    // 1. Prevent Object default constructor execution
    : base (IntPtr.Zero, JniHandleOwnership.DoNotTransfer)
{
    // 2. Don't allocate Java instance if already allocated
    if (Handle != IntPtr.Zero)
        return;

    // 3. Derived type? Create Android Callable Wrapper
    if (GetType () != typeof (Integer)) {
        SetHandle (
                Android.Runtime.JNIEnv.CreateInstance (GetType (), "(I)V", new JValue (value)),
                JniHandleOwnership.TransferLocalRef);
        return;
    }

    // 4. Declaring type: lookup &amp; cache method id...
    if (id_ctor_I == IntPtr.Zero)
        id_ctor_I = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
    // ...then create the Java instance and store
    SetHandle (
            JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value)),
            JniHandleOwnership.TransferLocalRef);
}
```

[JNIEnv.CreateInstance](xref:Android.Runtime.JNIEnv.CreateInstance*) 메서드는 `JNIEnv.FindClass`에서 반환된 값에 대해 `JNIEnv.FindClass`, `JNIEnv.GetMethodID`, `JNIEnv.NewObject` 및 `JNIEnv.DeleteGlobalReference`를 수행하기 위한 도우미입니다. 자세한 내용은 다음 섹션을 참조하십시오.

<a name="_Supporting_Inheritance,_Interfaces_1"></a>

### <a name="supporting-inheritance-interfaces"></a>상속 지원, 인터페이스

Java 형식을 서브클래싱하거나 Java 인터페이스를 구현하려면 패키징 프로세스 중에 모든 `Java.Lang.Object` 서브클래스에 대해 생성되는 ACW([Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md))를 생성해야 합니다. ACW 생성은 [Android.Runtime.RegisterAttribute](xref:Android.Runtime.RegisterAttribute) 사용자 지정 특성을 통해 제어됩니다.

C# 형식의 경우 `[Register]` 사용자 지정 특성 생성자에는 하나의 인수가 필요합니다. 따라서 해당 Java 형식에 대해 [JNI 단순 형식 참조](#_Simplified_Type_References_1)가 있습니다. 이를 통해 Java와 C# 사이에 다른 이름을 제공할 수 있습니다.

Xamarin.Android 4.0 이전에는 기존 Java 형식에 "별칭"을 지정하는 데 `[Register]` 사용자 지정 특성을 사용할 수 없습니다. 이는 ACW 생성 프로세스에서 발생한 모든 `Java.Lang.Object` 서브클래스에 대해 ACW를 생성하기 때문입니다.

Xamarin.Android 4.0에는 [RegisterAttribute.DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) 속성이 도입되었습니다. 이 속성은 ACW 생성 프로세스 중에 주석이 지정된 형식을 ‘건너뛰도록’ 하여 새로운 관리형 호출 가능 래퍼를 선언함으로써 패키지 생성 타임에 ACW가 생성되지 않도록 합니다. 이렇게 하면 기존 Java 형식을 바인딩할 수 있습니다. 예를 들어, 정수를 더하고 결과를 반환하는 하나의 메서드 `add`를 포함하는 다음과 같은 간단한 Java 클래스 `Adder`을 살펴보겠습니다.

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

`Adder` 형식은 다음과 같이 바인딩될 수 있습니다.

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public partial class Adder : Java.Lang.Object {
    static IntPtr class_ref = JNIEnv.FindClass ( "mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }
}
partial class ManagedAdder : Adder {
}
```

여기서 `Adder` C# 형식은 `Adder` Java 형식에 ‘별칭’을 지정합니다. `[Register]` 특성은 `mono.android.test.Adder` Java 형식의 JNI 이름을 지정하는 데 사용되고, `DoNotGenerateAcw` 속성은 ACW 생성을 금지하는 데 사용됩니다. 이로 인해 `ManagedAdder` 형식에 대한 ACW가 생성되고 그 결과로 `mono.android.test.Adder` 형식이 올바르게 서브클래싱됩니다. `RegisterAttribute.DoNotGenerateAcw` 속성을 사용하지 않았으면 Xamarin.Android 빌드 프로세스에서 새로운 `mono.android.test.Adder` Java 형식을 생성했을 것입니다. 이 경우 `mono.android.test.Adder` 형식이 두 개의 개별 파일에 두 번 표시되므로 컴파일 오류가 발생합니다.

### <a name="binding-virtual-methods"></a>가상 메서드 바인딩

`ManagedAdder`는 Java `Adder` 형식을 서브클래싱하지만 특히 흥미로운 것은 아닙니다. C# `Adder` 형식은 가상 메서드를 정의하지 않으므로 `ManagedAdder`은 어떤 항목도 재정의할 수 없습니다.

`virtual` 메서드를 바인딩하여 서브클래스의 재정의를 허용하려면 다음 두 범주에 해당하는 몇 가지 작업을 수행해야 합니다.

1. **메서드 바인딩**

1. **메서드 등록**

#### <a name="method-binding"></a>메서드 바인딩

메서드 바인딩을 사용하려면 C# `Adder` 정의에 두 개의 지원 멤버(`ThresholdType` 및 `ThresholdClass`를 추가해야 합니다.

##### <a name="thresholdtype"></a>ThresholdType

`ThresholdType` 속성은 바인딩의 현재 형식을 반환합니다.

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType`은 메서드 바인딩에서 가상 메서드 및 비가상 메서드 디스패치를 수행해야 하는 경우를 결정하는 데 사용됩니다. 항상 선언하는 C# 형식에 해당하는 `System.Type` 인스턴스를 반환해야 합니다.

##### <a name="thresholdclass"></a>ThresholdClass

`ThresholdClass` 속성은 바인딩된 형식에 대한 JNI 클래스 참조를 반환합니다.

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass`는 비가상 메서드를 호출할 때 메서드 바인딩에서 사용됩니다.

#### <a name="binding-implementation"></a>바인딩 구현

메서드 바인딩 구현 중에 Java 메서드의 런타임 호출이 수행됩니다. 이 작업은 메서드 등록의 일부인 `[Register]` 사용자 지정 특성 선언을 포함하며 메서드 등록 섹션에서 설명됩니다.

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }
}
```

`id_add` 필드에는 호출할 Java 메서드의 메서드 ID가 포함되어 있습니다. `id_add` 값은 선언 클래스(`class_ref`), Java 메서드 이름(`"add"`) 및 메서드의 JNI 시그니처(`"(II)I"`)가 필요한 `JNIEnv.GetMethodID`에서 가져옵니다.

메서드 ID를 가져온 후에는 `GetType`을 `ThresholdType`과 비교하여 가상 디스패치가 필요한지 또는 비가상 디스패치가 필요한지를 확인합니다. `Handle`에서 메서드를 재정의하는 Java 할당 서브클래스를 참조할 수 있으므로 `GetType`이 `ThresholdType`과 일치하는 경우에는 가상 디스패치가 필요합니다.

`GetType`이 `ThresholdType`과 일치하지 않으면 `Adder`가 서브클래싱되었으며(예: `ManagedAdder`), 서브클래스가 `Adder.Add`를 호출한 경우에만 `base.Add` 구현이 호출됩니다. 이것은 `ThresholdClass`가 제공되는 비가상 디스패치 사례입니다. `ThresholdClass`는 호출할 메서드의 구현을 제공하는 Java 클래스를 지정합니다.

#### <a name="method-registration"></a>메서드 등록

`Adder.Add` 메서드를 재정의하는 업데이트된 `ManagedAdder` 정의가 있다고 가정합니다.

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

다시 말하지만 `Adder.Add`에는 `[Register]` 사용자 지정 특성이 있습니다.

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

`[Register]` 사용자 지정 특성 생성자는 다음 3개의 값을 허용합니다.

1. Java 메서드의 이름(이 경우 `"add"`)

1. 메서드의 JNI 형식 시그니처(이 경우 `"(II)I"`)

1. *커넥터 메서드*(이 경우 `GetAddHandler`)
    커넥터 메서드는 뒷부분에 설명됩니다.

처음 두 매개 변수를 사용하여 ACW 생성 프로세스에서 메서드를 재정의하는 메서드 선언을 생성할 수 있습니다. 결과 ACW는 다음 코드 중 일부를 포함합니다.

```csharp
public class ManagedAdder extends mono.android.test.Adder {
    static final String __md_methods;
    static {
        __md_methods = "n_add:(II)I:GetAddHandler\n" +
            "";
        mono.android.Runtime.register (...);
    }
    @Override
    public int add (int p0, int p1) {
        return n_add (p0, p1);
    }
    private native int n_add (int p0, int p1);
    // ...
}
```

이름은 같고 `n_` 접두사가 붙은 메서드로 위임되는 `@Override` 메서드가 선언됩니다. 이렇게 하면 Java 코드가 `ManagedAdder.add`를 호출할 때 재정의 C# `ManagedAdder.Add` 메서드를 실행할 수 있도록 하는 `ManagedAdder.n_add`가 호출됩니다.

따라서 가장 중요한 질문은 `ManagedAdder.n_add`가 `ManagedAdder.Add`에 어떻게 연결되는가?입니다.

Java `native` 메서드는 [JNI RegisterNatives function](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734) 함수를 통해 Java(Android 런타임) 런타임에 등록됩니다.
`RegisterNatives`는 Java 메서드 이름, JNI 형식 시그니처 및 [JNI 호출 규칙](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715)에 따라 호출할 함수 포인터를 포함하는 구조체 배열을 사용합니다.
함수 포인터는 두 개의 포인터 인수 다음에 메서드 매개 변수를 사용하는 함수여야 합니다. Java `ManagedAdder.n_add` 메서드는 다음 C 프로토타입이 있는 함수를 통해 구현해야 합니다.

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.Android는 `RegisterNatives` 메서드를 노출하지 않습니다. 대신 ACW와 MCW는 `RegisterNatives`을 호출하는 데 필요한 정보를 제공합니다. 즉, ACW는 메서드 이름 및 JNI 형식 시그니처를 포함하고, 누락된 유일한 항목은 연결할 함수 포인터입니다.

여기에 ‘커넥터 메서드’가 옵니다. 세 번째 `[Register]` 사용자 지정 특성 매개 변수는 등록 된 형식에 정의된 메서드의 이름 또는 매개 변수를 허용하지 않고 `System.Delegate`를 반환하는 등록된 형식의 기본 클래스입니다. 그런 후 반환된 `System.Delegate`는 올바른 JNI 함수 시그니처가 있는 메서드를 나타냅니다. 마지막으로, 대리자가 Java에 제공될 때 GC가 수집하지 않도록 커넥터 메서드에서 반환하는 대리자는 루팅‘되어야 합니다’.

```csharp
#pragma warning disable 0169
static Delegate cb_add;
// This method must match the third parameter of the [Register]
// custom attribute, must be static, must return System.Delegate,
// and must accept no parameters.
static Delegate GetAddHandler ()
{
    if (cb_add == null)
        cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
    return cb_add;
}
// This method is registered with JNI.
static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
{
    Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
    return __this.Add (a, b);
}
#pragma warning restore 0169
```

`GetAddHandler` 메서드는 `n_Add` 메서드를 참조하는 `Func<IntPtr, IntPtr, int, int,
int>` 대리자를 만든 다음, [JNINativeWrapper.CreateDelegate](xref:Android.Runtime.JNINativeWrapper.CreateDelegate*)를 호출합니다.
`JNINativeWrapper.CreateDelegate`는 제공된 메서드를 try/catch 블록에 래핑하여 처리되지 않은 예외가 처리되고 [AndroidEvent.UnhandledExceptionRaiser](xref:Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser) 이벤트를 발생시킵니다. 결과 대리자는 GC가 대리자를 해제하지 않도록 정적 `cb_add` 변수에 저장됩니다.

마지막으로 `n_Add` 메서드는 JNI 매개 변수를 해당하는 관리형 형식으로 마샬링하고 메서드 호출을 위임합니다.

참고: Java 인스턴스를 통해 MCW를 가져올 때 항상 `JniHandleOwnership.DoNotTransfer`를 사용합니다. 해당 항목을 로컬 참조로 처리하여 `JNIEnv.DeleteLocalRef`를 호출하면 관리형 -&gt; Java -&gt; 관리형 스택 전환이 중단됩니다.

### <a name="complete-adder-binding"></a>완전한 Adder 바인딩

`mono.android.tests.Adder` 형식에 대한 완전한 관리형 바인딩은 다음과 같습니다.

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public class Adder : Java.Lang.Object {

    static IntPtr class_ref = JNIEnv.FindClass ("mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    protected override Type ThresholdType {
        get {return typeof (Adder);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

#region Add
    static IntPtr id_add;

    [Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }

#pragma warning disable 0169
    static Delegate cb_add;
    static Delegate GetAddHandler ()
    {
        if (cb_add == null)
            cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
        return cb_add;
    }

    static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
    {
        Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
        return __this.Add (a, b);
    }
#pragma warning restore 0169
#endregion
}
```

### <a name="restrictions"></a>제한

다음 조건과 일치하는 형식을 작성할 때 다음이 적용됩니다.

1. `Java.Lang.Object`를 서브클래싱합니다.

1. `[Register]` 사용자 지정 특성이 있습니다.

1. `RegisterAttribute.DoNotGenerateAcw`가 `true`인 경우

그런 다음, GC 상호 작용의 경우 형식에는 런타임에 `Java.Lang.Object` 또는 `Java.Lang.Object` 서브클래스를 참조할 수 있는 필드가 ‘없어야 합니다’. 예를 들어, `System.Object` 형식의 필드와 모든 인터페이스 형식은 허용되지 않습니다. `Java.Lang.Object` 인스턴스를 참조할 수 없는 형식(예: `System.String` 및 `List<int>`)이 허용됩니다. 해당 제한 사항으로 인해 GC를 통한 조기 개체 수집이 방지됩니다.

`Java.Lang.Object` 인스턴스를 참조할 수 있는 인스턴스 필드가 형식에 포함되어야 하는 경우 필드 형식은 `System.WeakReference` 또는 `GCHandle`이어야 합니다.

## <a name="binding-abstract-methods"></a>바인딩 추상 메서드

바인딩 `abstract` 메서드는 대체로 가상 메서드와 동일합니다. 다음과 같은 두 가지 차이점이 있습니다.

1. 추상 메서드는 추상적입니다. `[Register]` 특성 및 연결된 메서드 등록이 그대로 유지되며 메서드 바인딩이 `Invoker` 형식으로 바로 이동됩니다.

1. 추상 형식을 서브클래싱하는 비 `abstract` `Invoker` 형식이 생성됩니다. 비가상 디스패치 사례는 무시해도 되지만 `Invoker` 형식은 기본 클래스에 선언된 모든 추상 메서드를 재정의해야 하며, 재정의된 구현은 메서드 바인딩 구현입니다.

예를 들어 위의 `mono.android.test.Adder.add` 메서드가 `abstract`라고 가정합니다. `Adder.Add`가 추상적이고 `Adder.Add`를 구현한 새 `AdderInvoker` 형식이 정의되도록 C# 바인딩이 변경됩니다.

```csharp
partial class Adder {
    [Register ("add", "(II)I", "GetAddHandler")]
    public abstract int Add (int a, int b);

    // The Method Registration machinery is identical to the
    // virtual method case...
}

partial class AdderInvoker : Adder {
    public AdderInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    static IntPtr id_add;
    public override int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
    }
}
```

`Invoker` 형식은 Java에서 만든 인스턴스에 대한 JNI 참조를 가져올 때만 필요합니다.

## <a name="binding-interfaces"></a>바인딩 인터페이스

바인딩 인터페이스는 개념적으로 가상 메서드를 포함하는 바인딩 클래스와 유사하지만 대부분의 세부 사항은 미묘하게 차이를 보입니다. 다음 [Java 인터페이스 선언](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14)을 참조하세요.

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

인터페이스 바인딩은 두 부분인 C# 인터페이스 정의와 인터페이스에 대한 호출자 정의로 구성됩니다.

### <a name="interface-definition"></a>인터페이스 정의

C# 인터페이스 정의는 다음 요구 사항을 충족해야 합니다.

- 인터페이스 정의에는 `[Register]` 사용자 지정 특성이 있어야 합니다.

- 인터페이스 정의는 `IJavaObject interface`를 확장해야 합니다.
    이렇게 하지 않으면 ACW가 Java 인터페이스에서 상속하지 못합니다.

- 각 인터페이스 메서드는 해당하는 Java 메서드 이름, JNI 시그니처 및 커넥터 메서드를 지정하는 `[Register]` 특성을 포함해야 합니다.

- 커넥터 메서드는 커넥터 메서드가 있을 수 있는 형식도 지정해야 합니다.

`abstract` 및 `virtual` 메서드를 바인딩할 때 커넥터 메서드는 등록 중인 형식의 상속 계층 구조 내에서 검색됩니다. 인터페이스에는 본문이 포함된 메서드가 없을 수 있으므로 해당 방식이 작동하지 않습니다. 따라서 커넥터 메서드가 있는 위치를 나타내는 형식을 지정해야 합니다. 형식은 커넥터 메서드 문자열 내에서 콜론 `':'`을 입력한 후에 지정되면 호출자를 포함하는 형식의 어셈블리 정규화 형식 이름이어야 합니다.

인터페이스 메서드 선언은 ‘호환되는’ 형식을 사용하여 해당 Java 메서드를 변환하는 것입니다. Java 기본 형식의 경우 호환되는 형식이 해당 C# 형식입니다(예: Java `int`는 C# `int`임). 참조 형식의 경우 호환되는 형식은 적절한 Java 형식의 JNI 핸들을 제공할 수 있는 형식입니다.

인터페이스 멤버는 Java에 의해 직접 호출되지 않습니다. 즉, 호출은 호출자 형식을 통해 조정되므로 어느 정도 유연성이 허용됩니다.

Java 진행률 인터페이스를 [C#에서 다음으로 선언할 수 있습니다](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83).

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

위에서 Java `int[]` 매개 변수를 [JavaArray&lt;int&gt;](xref:Android.Runtime.JavaArray`1)에 매핑합니다.
이 작업이 반드시 필요한 것은 아닙니다. C# `int[]`또는 `IList<int>`에 바인딩하거나 완전히 다른 항목에 바인딩할 수도 있습니다. 어떤 형식을 선택하든지 `Invoker`는 호출을 위해 해당 형식을 Java `int[]` 형식으로 변환할 수 있어야 합니다.

### <a name="invoker-definition"></a>호출자 정의

`Invoker` 형식 정의는 `Java.Lang.Object`를 상속하고, 적절 한 인터페이스를 구현한 후 인터페이스 정의에서 참조되는 모든 연결 메서드를 제공해야 합니다. 클래스 바인딩과 다른 제안 사항이 하나 더 있습니다. `class_ref` 필드와 메서드 ID는 정적 멤버가 아닌 인스턴스 멤버여야 합니다.

인스턴스 멤버를 사용하는 이유는 Android 런타임의 `JNIEnv.GetMethodID` 동작과 관련이 있습니다. (이 동작은 Java 동작일 수도 있으며 테스트되지 않았습니다.) 선언된 인터페이스가 아니라 구현된 인터페이스에서 제공되는 메서드를 조회할 때 `JNIEnv.GetMethodID`는 null을 반환합니다. [java.util.Map&lt;K, V&gt;](https://developer.android.com/reference/java/util/Map.html) 인터페이스를 구현하는 [java.util.SortedMap&lt;K, V&gt;](https://developer.android.com/reference/java/util/SortedMap.html) Java 인터페이스를 고려해보세요. 맵은 [clear](https://developer.android.com/reference/java/util/Map.html#clear()) 메서드를 제공하므로 SortedMap에 대한 적절한 `Invoker` 정의는 다음과 같습니다.

```csharp
// Fails at runtime. DO NOT FOLLOW
partial class ISortedMapInvoker : Java.Lang.Object, ISortedMap {
    static IntPtr class_ref = JNIEnv.FindClass ("java/util/SortedMap");
    static IntPtr id_clear;
    public void Clear()
    {
        if (id_clear == IntPtr.Zero)
            id_clear = JNIEnv.GetMethodID(class_ref, "clear", "()V");
        JNIEnv.CallVoidMethod(Handle, id_clear);
    }
     // ...
}
```

`JNIEnv.GetMethodID`는 `SortedMap` 클래스 인스턴스를 통해 `Map.clear` 메서드를 조회할 때 `null`를 반환하므로 위의 내용은 실패합니다.

이에 대한 두 가지 솔루션이 있습니다. 모든 메서드의 출처가 되고 각 인터페이스에 대한 `class_ref`를 포함하는 인터페이스를 추적하거나 모든 항목을 인스턴스 멤버로 유지하고 인터페이스 형식이 아니라 가장 많이 파생 클래스 형식에 대해 메서드 조회를 수행하는 것입니다. 후자는 **Mono.Android.dll**에서 수행됩니다.

호출자 정의에는 생성자, `Dispose` 메서드, `ThresholdType` 및 `ThresholdClass` 멤버, `GetObject` 메서드, 인터페이스 메서드 구현 및 커넥터 메서드 구현의 여섯 가지 섹션이 있습니다.

#### <a name="constructor"></a>생성자

생성자는 호출 중인 인스턴스의 런타임 클래스를 조회하고 인스턴스 `class_ref` 필드에 런타임 클래스를 저장해야 합니다.

```csharp
partial class IAdderProgressInvoker {
    IntPtr class_ref;
    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref   = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }
}
```

참고: `Handle` 속성은 `handle` 매개 변수가 아닌 생성자 본문 내에서 사용해야 하며, Android v4.0에서와 같이 `handle` 매개 변수는 기본 생성자 실행이 완료된 후 유효하지 않게 될 수 있습니다.

#### <a name="dispose-method"></a>Dispose 메서드

`Dispose` 메서드는 생성자에 할당된 전역 참조를 해제해야 합니다.

```csharp
partial class IAdderProgressInvoker {
    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }
}
```

#### <a name="thresholdtype-and-thresholdclass"></a>ThresholdType 및 ThresholdClass

`ThresholdType` 및 `ThresholdClass` 멤버는 클래스 바인딩에서 발견된 것과 동일합니다.

```csharp
partial class IAdderProgressInvoker {
    protected override Type ThresholdType {
        get {
            return typeof (IAdderProgressInvoker);
        }
    }
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

#### <a name="getobject-method"></a>GetObject 메서드

정적 `GetObject` 메서드는 [JavaCast&lt;T&gt;()](xref:Android.Runtime.Extensions.JavaCast*)를 지원하는 데 필요합니다.

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```

#### <a name="interface-methods"></a>인터페이스 메서드

인터페이스의 모든 메서드에는 JNI을 통해 해당 Java 메서드를 호출하는 구현이 있어야 합니다.

```csharp
partial class IAdderProgressInvoker {
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd", "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd, new JValue (JNIEnv.ToJniHandle (values)), new JValue (currentIndex), new JValue (currentSum));
    }
}
```

#### <a name="connector-methods"></a>커넥터 메서드

커넥터 메서드 및 지원 인프라는 JNI 매개 변수를 적절한 C# 형식으로 마샬링하는 일을 담당합니다. Java `int[]` 매개 변수는 C# 내에서 `IntPtr`인 JNI `jintArray`으로 전달됩니다. C# 인터페이스 호출을 지원하기 위해 `IntPtr`를 `JavaArray<int>`로 마샬링해야 합니다.

```csharp
partial class IAdderProgressInvoker {
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
}
```

`int[]`이 `JavaList<int>`보다 선호되면 대신 [JNIEnv.GetArray()](xref:Android.Runtime.JNIEnv.GetArray*)를 사용할 수 있습니다.

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

그러나 이 `JNIEnv.GetArray`는 VM 사이에 전체 배열을 복사하므로, 대형 배열의 경우 GC 압력이 크게 가중될 수 있습니다.

### <a name="complete-invoker-definition"></a>전체 호출자 정의

[전체 IAdderProgressInvoker 정의](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88):

```csharp
class IAdderProgressInvoker : Java.Lang.Object, IAdderProgress {

    IntPtr class_ref;

    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }

    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }

    protected override Type ThresholdType {
        get {return typeof (IAdderProgressInvoker);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }

#region OnAdd
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd",
                    "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd,
                new JValue (JNIEnv.ToJniHandle (values)),
                new JValue (currentIndex),
new JValue (currentSum));
    }

#pragma warning disable 0169
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
#pragma warning restore 0169
#endregion
}
```

## <a name="jni-object-references"></a>JNI 개체 참조

많은 JNIEnv 메서드는 `GCHandle`과 비슷한 *JNI* ‘개체 참조’를 반환합니다. JNI는 로컬 참조, 전역 참조 및 약한 전역 참조의 세 가지 개체 참조 형식을 제공합니다. 해당 세 가지 형식은 모두 `System.IntPtr`로 표시‘되지만’(JNI 함수 형식 섹션에 따라) `JNIEnv` 메서드에서 반환되는 모든 `IntPtr`이 참조인 것은 아닙니다. 예를 들어 [JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*)는 `IntPtr`을 반환하지만 개체 참조를 반환하지 않고 `jmethodID`를 반환합니다. 자세한 내용은 [JNI 함수 설명서](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)를 참조하세요.

로컬 참조는 ‘대부분의’ 참조 생성 메서드를 통해 생성됩니다.
Android에서는 지정된 시간에 제한된 수(일반적으로 512개)의 로컬 참조만 존재할 수 있습니다. 로컬 참조는 [JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*)를 통해 삭제할 수 있습니다.
JNI와 달리, 개체 참조를 반환하는 모든 참조 JNIEnv 메서드가 로컬 참조를 반환하는 것은 아닙니다. 즉, [JNIEnv.FindClass](xref:Android.Runtime.JNIEnv.FindClass*)는 *전역* 참조를 반환합니다. 가능한 한 빨리, 로컬 참조를 삭제하는 것이 좋습니다. 개체 주위에 [Java.Lang.Object](xref:Java.Lang.Object)를 생성하고 [Java.Lang.Object(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object#ctor*) 생성자에 대해 `JniHandleOwnership.TransferLocalRef`을 지정하면 됩니다.

전역 참조는 [JNIEnv.NewGlobalRef](xref:Android.Runtime.JNIEnv.NewGlobalRef*) 및 [JNIEnv.FindClass](xref:Android.Runtime.JNIEnv.FindClass*)를 통해 생성됩니다.
[JNIEnv.DeleteGlobalRef](xref:Android.Runtime.JNIEnv.DeleteGlobalRef*)를 사용하여 제거할 수 있습니다.
에뮬레이터에는 대기 중인 전역 참조는 2,000개로 제한되지만 하드웨어 디바이스에는 약 52,000개의 전역 참조 제한이 있습니다.

약한 전역 참조는 Android v2.2(Froyo) 이상에서만 사용할 수 있습니다. 약한 전역 참조는 [JNIEnv.DeleteWeakGlobalRef](xref:Android.Runtime.JNIEnv.DeleteWeakGlobalRef*)를 사용하여 삭제할 수 있습니다.

### <a name="dealing-with-jni-local-references"></a>JNI 로컬 참조 처리

[JNIEnv.GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*), [JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*), [JNIEnv.CallObjectMethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*), [JNIEnv.CallNonvirtualObjectMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) 및 [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) 메서드는 Java 개체에 대한 JNI 로컬 참조를 포함하는 `IntPtr`을 반환하거나, Java가 `null`을 반환하는 경우에는 `IntPtr.Zero`를 반환합니다. 한 번에 처리될 수 있는 로컬 참조 수가 제한되므로(512개 항목) 참조가 적시에 삭제되도록 하는 것이 좋습니다. 로컬 참조는 다음과 같은 세 가지 방법으로 처리할 수 있습니다. 즉, 명시적으로 삭제하거나, 참조를 포함할 `Java.Lang.Object` 인스턴스를 만들거나, `Java.Lang.Object.GetObject<T>()`를 사용하여 주위에 관리형 호출 가능 래퍼를 만들 수 있습니다.

### <a name="explicitly-deleting-local-references"></a>명시적으로 로컬 참조 삭제

[JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*)는 로컬 참조를 삭제하는 데 사용됩니다. 로컬 참조를 삭제한 후에는 더 이상 사용할 수 없습니다. 따라서 `JNIEnv.DeleteLocalRef`가 로컬 참조를 사용하여 마지막으로 수행한 작업인지 확인해야 합니다.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```

### <a name="wrapping-with-javalangobject"></a>Java.Lang.Object로 래핑

`Java.Lang.Object`는 종료 JNI 참조를 래핑하는 데 사용할 수 있는 [Java.Lang.Object(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object#ctor*) 생성자를 제공합니다. [JniHandleOwnership](xref:Android.Runtime.JniHandleOwnership) 매개 변수는 `IntPtr` 매개 변수를 처리하는 방법을 결정합니다.

- [JniHandleOwnership.DoNotTransfer](xref:Android.Runtime.JniHandleOwnership.DoNotTransfer) &ndash; 생성된 `Java.Lang.Object` 인스턴스는 `handle` 매개 변수에서 새 전역 참조를 만들며 `handle`은 변경되지 않습니다.
    호출자는 필요한 경우 `handle`을 해제해야 합니다.

- [JniHandleOwnership.TransferLocalRef](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; 생성된 `Java.Lang.Object` 인스턴스는 `handle` 매개 변수에서 새 전역 참조를 만들며 `handle`은 [JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*)를 사용하여 삭제됩니다. 호출자는 `handle`을 해제해서는 안 되며 생성자가 실행을 완료한 후에는 `handle`을 사용하지 않아야 합니다.

- [JniHandleOwnership.TransferGlobalRef](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; 생성된 `Java.Lang.Object` 인스턴스 `handle` 매개 변수의 소유권을 갖습니다. 호출자가 `handle`를 해제하면 안 됩니다.

JNI 메서드 호출 메서드는 로컬 참조를 반환하므로 일반적으로 `JniHandleOwnership.TransferLocalRef`가 사용됩니다.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

`Java.Lang.Object` 인스턴스가 가비지 수집될 때까지 생성된 전역 참조는 해제되지 않습니다. 가능한 경우 인스턴스를 삭제하면 전역 참조가 해제되고 가비지 수집 속도가 빨라집니다.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```

### <a name="using-javalangobjectgetobjectlttgt"></a>Java.Lang.Object.GetObject&lt;T&gt;() 사용

`Java.Lang.Object`는 지정된 형식의 관리형 호출 가능 래퍼를 만드는 데 사용할 수 있는 [Java.Lang.Object.GetObject&lt;T&gt;(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object.GetObject*) 메서드를 제공합니다.

형식 `T`는 다음 요구 사항을 충족해야 합니다.

1. `T`는 참조 형식이어야 합니다.

1. `T`는 `IJavaObject` 인터페이스를 구현해야 합니다.

1. `T`가 추상 클래스 또는 인터페이스가 아니면 `T`는 매개 변수 형식이 `(IntPtr,
    JniHandleOwnership)`인 생성자를 제공해야 합니다.

1. `T`가 추상 클래스 또는 인터페이스인 경우 `T`에 대해 사용할 수 있는 *호출자*가 *반드시* 있어야 합니다. 호출자는 `T`를 상속하거나 `T`를 구현하며 `T`와 이름이 같고 호출자 접미사를 포함하는 비추상 형식입니다. 예를 들어 T가 인터페이스 `Java.Lang.IRunnable`인 경우 형식 `Java.Lang.IRunnableInvoker`가 있어야 하고 필수 `(IntPtr,
    JniHandleOwnership)` 생성자를 포함해야 합니다.

JNI 메서드 호출 메서드는 로컬 참조를 반환하므로 일반적으로 `JniHandleOwnership.TransferLocalRef`가 사용됩니다.

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types"></a>

## <a name="looking-up-java-types"></a>Java 형식 조회

JNI에서 필드 또는 메서드를 조회하려면 필드 또는 메서드에 대한 선언 형식을 먼저 조회해야 합니다. [Android.Runtime.JNIEnv.FindClass(string)](xref:Android.Runtime.JNIEnv.FindClass*)) 메서드는 Java 형식을 조회하는 데 사용됩니다. 문자열 매개 변수는 ‘단순 형식 참조’이거나 Java 형식의 경우 ‘전체 형식 참조’입니다.  단순 전체 형식 참조에 대한 자세한 내용은 [JNI 형식 참조 섹션](#_JNI_Type_References)을 참조하세요.

참고: 개체 인스턴스를 반환하는 다른 모든 `JNIEnv` 메서드와 달리 `FindClass`는 지역 참조가 아닌 전역 참조를 반환합니다.

<a name="_Instance_Fields"></a>

## <a name="instance-fields"></a>인스턴스 필드

필드는 ‘필드 ID’를 통해 조작됩니다. 필드 ID는 필드가 정의된 클래스, 필드 이름 및 필드의 [JNI 형식 시그니처](#JNI_Type_Signatures)가 필요한 [JNIEnv.GetFieldID](xref:Android.Runtime.JNIEnv.GetFieldID*)를 통해 가져옵니다.

필드 ID는 해제할 필요가 없으며 해당 Java 형식이 로드되기만 하면 유효합니다. (Android는 현재 클래스 언로드를 지원하지 않습니다.)

인스턴스 필드를 조작하기 위한 메서드에는 인스턴스 필드를 읽기 위한 메서드와 인스턴스 필드를 작성하기 위한 메서드가 있습니다. 모든 메서드 세트에는 필드 값을 읽거나 쓰기 위한 필드 ID가 필요합니다.

### <a name="reading-instance-field-values"></a>인스턴스 필드 값 읽기

인스턴스 필드 값을 읽기 위한 메서드 세트는 다음과 같은 명명 패턴을 따릅니다.

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```

여기서 `*`는 필드의 형식입니다.

- [JNIEnv.GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*) &ndash; `java.lang.Object`, 배열 및 인터페이스 형식과 같은 기본 제공 형식이 아닌 인스턴스 필드의 값을 읽습니다. 반환되는 값은 JNI 로컬 참조입니다.

- [JNIEnv.GetBooleanField](xref:Android.Runtime.JNIEnv.GetBooleanField*) &ndash; `bool` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv.GetByteField](xref:Android.Runtime.JNIEnv.GetByteField*) &ndash; `sbyte` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv.GetCharField](xref:Android.Runtime.JNIEnv.GetCharField*) &ndash; `char` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv.GetShortField](xref:Android.Runtime.JNIEnv.GetShortField*) &ndash; `short` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv.GetIntField](xref:Android.Runtime.JNIEnv.GetIntField*) &ndash; `int` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv.GetLongField](xref:Android.Runtime.JNIEnv.GetLongField*) &ndash; `long` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv.GetFloatField](xref:Android.Runtime.JNIEnv.GetFloatField*) &ndash; `float` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv.GetDoubleField](xref:Android.Runtime.JNIEnv.GetDoubleField*) &ndash; `double` 인스턴스 필드의 값을 읽습니다.

### <a name="writing-instance-field-values"></a>인스턴스 필드 값 쓰기

인스턴스 필드 값을 쓰기 위한 메서드 세트는 다음과 같은 명명 패턴을 따릅니다.

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

여기서 *Type*은 필드의 형식입니다.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `java.lang.Object`, 배열 및 인터페이스 형식과 같은 기본 제공 형식이 아닌 모든 필드의 값을 씁니다. `IntPtr` 값은 JNI 로컬 참조, JNI 전역 참조, JNI 약한 전역 참조 또는 `IntPtr.Zero`(`null`의 경우)일 수 있습니다.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))   &ndash; `bool` 인스턴스 필드의 값을 씁니다.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))   &ndash; `sbyte` 인스턴스 필드의 값을 씁니다.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))   &ndash; `char` 인스턴스 필드의 값을 씁니다.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))   &ndash; `short` 인스턴스 필드의 값을 씁니다.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))   &ndash; `int` 인스턴스 필드의 값을 씁니다.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))   &ndash; `long` 인스턴스 필드의 값을 씁니다.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))   &ndash; `float` 인스턴스 필드의 값을 씁니다.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*))   &ndash; `double` 인스턴스 필드의 값을 씁니다.

<a name="_Static_Fields"></a>

## <a name="static-fields"></a>정적 필드

정적 필드는 ‘필드 ID’를 통해 조작됩니다. 필드 ID는 필드가 정의된 클래스, 필드 이름 및 필드의 [JNI 형식 시그니처](#JNI_Type_Signatures)가 필요한 [JNIEnv.GetStaticFieldID](xref:Android.Runtime.JNIEnv.GetStaticFieldID*)를 통해 가져옵니다.

필드 ID는 해제할 필요가 없으며 해당 Java 형식이 로드되기만 하면 유효합니다. (Android는 현재 클래스 언로드를 지원하지 않습니다.)

정적 필드를 조작하기 위한 메서드에는 인스턴스 필드를 읽기 위한 메서드와 인스턴스 필드를 작성하기 위한 메서드가 있습니다. 모든 메서드 세트에는 필드 값을 읽거나 쓰기 위한 필드 ID가 필요합니다.

### <a name="reading-static-field-values"></a>정적 필드 값 읽기

정적 필드 값을 읽기 위한 메서드 세트는 다음과 같은 명명 패턴을 따릅니다.

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

여기서 `*`는 필드의 형식입니다.

- [JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) &ndash; `java.lang.Object`, 배열 및 인터페이스 형식과 같은 기본 제공 형식이 아닌 정적 필드의 값을 읽습니다. 반환되는 값은 JNI 로컬 참조입니다.

- [JNIEnv.GetStaticBooleanField](xref:Android.Runtime.JNIEnv.GetStaticBooleanField*) &ndash; `bool` 정적 필드의 값을 읽습니다.

- [JNIEnv.GetStaticByteField](xref:Android.Runtime.JNIEnv.GetStaticByteField*) &ndash; `sbyte` 정적 필드의 값을 읽습니다.

- [JNIEnv.GetStaticCharField](xref:Android.Runtime.JNIEnv.GetStaticCharField*) &ndash; `char` 정적 필드의 값을 읽습니다.

- [JNIEnv.GetStaticShortField](xref:Android.Runtime.JNIEnv.GetStaticShortField*) &ndash; `short` 정적 필드의 값을 읽습니다.

- [JNIEnv.GetStaticLongField](xref:Android.Runtime.JNIEnv.GetStaticLongField*) &ndash; `long` 정적 필드의 값을 읽습니다.

- [JNIEnv.GetStaticFloatField](xref:Android.Runtime.JNIEnv.GetStaticFloatField*) &ndash; `float` 정적 필드의 값을 읽습니다.

- [JNIEnv.GetStaticDoubleField](xref:Android.Runtime.JNIEnv.GetStaticDoubleField*) &ndash; `double` 정적 필드의 값을 읽습니다.

### <a name="writing-static-field-values"></a>정적 필드 값 쓰기

정적 필드 값을 쓰기 위한 메서드 세트는 다음과 같은 명명 패턴을 따릅니다.

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

여기서 *Type*은 필드의 형식입니다.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `java.lang.Object`, 배열 및 인터페이스 형식과 같은 기본 제공 형식이 아닌 모든 정적 필드의 값을 씁니다. `IntPtr` 값은 JNI 로컬 참조, JNI 전역 참조, JNI 약한 전역 참조 또는 `IntPtr.Zero`(`null`의 경우)일 수 있습니다.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))   &ndash; `bool` 정적 필드의 값을 씁니다.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))   &ndash; `sbyte` 정적 필드의 값을 씁니다.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))   &ndash; `char` 정적 필드의 값을 씁니다.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))   &ndash; `short` 정적 필드의 값을 씁니다.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))   &ndash; `int` 정적 필드의 값을 씁니다.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))   &ndash; `long` 정적 필드의 값을 씁니다.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))   &ndash; `float` 정적 필드의 값을 씁니다.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*))   &ndash; `double` 정적 필드의 값을 씁니다.

<a name="_Instance_Methods"></a>

## <a name="instance-methods"></a>인스턴스 메서드

인스턴스 메서드는 ‘메서드 ID’를 통해 호출됩니다. 메서드 ID는 메서드가 정의된 형식, 메서드 이름 및 메서드의 [JNI 형식 시그니처](#JNI_Type_Signatures)가 필요한 [JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*)를 통해 가져옵니다.

메서드 ID는 해제할 필요가 없으며 해당 Java 형식이 로드되기만 하면 유효합니다. (Android는 현재 클래스 언로드를 지원하지 않습니다.)

메서드를 호출하는 메서드에는 메서드를 가상으로 호출하는 메서드와 메서드를 비가상으로 호출하는 메서드가 있습니다. 두 메서드 세트 모두 메서드를 호출하려면 메서드 ID가 필요하며, 비가상 호출에서는 호출할 클래스 구현을 지정해야 합니다.

인터페이스 메서드는 선언 형식 내에서만 조회할 수 있으며, 확장/상속된 인터페이스에서 제공되는 메서드는 조회할 수 없습니다. 자세한 내용은 뒤쪽에 나오는 바인딩 인터페이스/호출자 구현 섹션을 참조하세요.

해당 클래스나 기본 클래스 또는 구현된 인터페이스에 선언된 모든 메서드는 조회할 수 있습니다.

### <a name="virtual-method-invocation"></a>가상 메서드 호출

메서드를 가상으로 호출하는 메서드 세트는 다음과 같은 명명 패턴을 따릅니다.

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

여기서 `*`는 메서드의 반환 형식입니다.

- [JNIEnv.CallObjectMethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*) &ndash; `java.lang.Object`, 배열 및 인터페이스와 같이 비기본 제공 형식을 반환하는 메서드를 호출합니다. 반환되는 값은 JNI 로컬 참조입니다.

- [JNIEnv.CallBooleanMethod](xref:Android.Runtime.JNIEnv.CallBooleanMethod*) &ndash; `bool` 값을 반환하는 메서드를 호출합니다.

- [JNIEnv.CallByteMethod](xref:Android.Runtime.JNIEnv.CallByteMethod*) &ndash; `sbyte` 값을 반환하는 메서드를 호출합니다.

- [JNIEnv.CallCharMethod](xref:Android.Runtime.JNIEnv.CallCharMethod*) &ndash; `char` 값을 반환하는 메서드를 호출합니다.

- [JNIEnv.CallShortMethod](xref:Android.Runtime.JNIEnv.CallShortMethod*) &ndash; `short` 값을 반환하는 메서드를 호출합니다.

- [JNIEnv.CallLongMethod](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; `long` 값을 반환하는 메서드를 호출합니다.

- [JNIEnv.CallFloatMethod](xref:Android.Runtime.JNIEnv.CallFloatMethod*) &ndash; `float` 값을 반환하는 메서드를 호출합니다.

- [JNIEnv.CallDoubleMethod](xref:Android.Runtime.JNIEnv.CallDoubleMethod*) &ndash; `double` 값을 반환하는 메서드를 호출합니다.

### <a name="non-virtual-method-invocation"></a>비가상 메서드 호출

메서드를 비가상으로 호출하는 메서드 세트는 다음과 같은 명명 패턴을 따릅니다.

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

여기서 `*`는 메서드의 반환 형식입니다. 비가상 메서드 호출은 일반적으로 가상 메서드의 기본 메서드를 호출하는 데 사용됩니다.

- [JNIEnv.CallNonvirtualObjectMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) &ndash; `java.lang.Object`, 배열 및 인터페이스와 같이 비기본 제공 형식을 반환하는 메서드를 비가상으로 호출합니다. 반환되는 값은 JNI 로컬 참조입니다.

- [JNIEnv.CallNonvirtualBooleanMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod*) &ndash; `bool` 값을 반환하는 메서드를 비가상으로 호출합니다.

- [JNIEnv.CallNonvirtualByteMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualByteMethod*) &ndash; `sbyte` 값을 반환하는 메서드를 비가상으로 호출합니다.

- [JNIEnv.CallNonvirtualCharMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualCharMethod*) &ndash; `char` 값을 반환하는 메서드를 비가상으로 호출합니다.

- [JNIEnv.CallNonvirtualShortMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualShortMethod*) &ndash; `short` 값을 반환하는 메서드를 비가상으로 호출합니다.

- [JNIEnv.CallNonvirtualLongMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualLongMethod*) &ndash; `long` 값을 반환하는 메서드를 비가상으로 호출합니다.

- [JNIEnv.CallNonvirtualFloatMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualFloatMethod*) &ndash; `float` 값을 반환하는 메서드를 비가상으로 호출합니다.

- [JNIEnv.CallNonvirtualDoubleMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod*) &ndash; `double` 값을 반환하는 메서드를 비가상으로 호출합니다.

<a name="_Static_Methods"></a>

## <a name="static-methods"></a>정적 메서드

정적 메서드는 ‘메서드 ID’를 통해 호출됩니다. 메서드 ID는 메서드가 정의된 형식, 메서드 이름 및 메서드의 [JNI 형식 시그니처](#JNI_Type_Signatures)가 필요한 [JNIEnv.GetStaticMethodID](xref:Android.Runtime.JNIEnv.GetStaticMethodID*)를 통해 가져옵니다.

메서드 ID는 해제할 필요가 없으며 해당 Java 형식이 로드되기만 하면 유효합니다. (Android는 현재 클래스 언로드를 지원하지 않습니다.)

### <a name="static-method-invocation"></a>정적 메서드 호출

메서드를 가상으로 호출하는 메서드 세트는 다음과 같은 명명 패턴을 따릅니다.

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

여기서 `*`는 메서드의 반환 형식입니다.

- [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) &ndash; `java.lang.Object`, 배열 및 인터페이스와 같이 비기본 제공 형식을 반환하는 정적 메서드를 호출합니다. 반환되는 값은 JNI 로컬 참조입니다.

- [JNIEnv.CallStaticBooleanMethod](xref:Android.Runtime.JNIEnv.CallStaticBooleanMethod*) &ndash; `bool` 값을 반환하는 정적 메서드를 호출합니다.

- [JNIEnv.CallStaticByteMethod](xref:Android.Runtime.JNIEnv.CallStaticByteMethod*) &ndash; `sbyte` 값을 반환하는 정적 메서드를 호출합니다.

- [JNIEnv.CallStaticCharMethod](xref:Android.Runtime.JNIEnv.CallStaticCharMethod*) &ndash; `char` 값을 반환하는 정적 메서드를 호출합니다.

- [JNIEnv.CallStaticShortMethod](xref:Android.Runtime.JNIEnv.CallStaticShortMethod*) &ndash; `short` 값을 반환하는 정적 메서드를 호출합니다.

- [JNIEnv.CallStaticLongMethod](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; `long` 값을 반환하는 정적 메서드를 호출합니다.

- [JNIEnv.CallStaticFloatMethod](xref:Android.Runtime.JNIEnv.CallStaticFloatMethod*) &ndash; `float` 값을 반환하는 정적 메서드를 호출합니다.

- [JNIEnv.CallStaticDoubleMethod](xref:Android.Runtime.JNIEnv.CallStaticDoubleMethod*) &ndash; `double` 값을 반환하는 정적 메서드를 호출합니다.

<a name="JNI_Type_Signatures"></a>

## <a name="jni-type-signatures"></a>JNI 형식 시그니처

[JNI 형식 시그니처](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432)는 메서드를 제외한 [JNI 형식 참조](#_JNI_Type_References)입니다(단순 형식 참조는 아님). 메서드를 사용하는 경우 JNI 형식 시그니처는 여는 괄호 `'('`, 함께 연결된 모든 매개 변수 형식에 대한 형식 참조(구분용 쉼표 또는 다른 기호 없음), 닫는 괄호 `')'`, 메서드 반환 형식의 JNI 형식 참조로 구성됩니다.

다음과 같은 Java 메서드를 예로 들어 보겠습니다.

```java
long f(int n, String s, int[] array);
```

JNI 형식 시그니처는 다음과 같습니다.

```csharp
(ILjava/lang/String;[I)J
```

일반적으로 `javap` 명령을 사용하여 JNI 시그니처를 결정하는 것이 ‘강력히’ 권장됩니다. 예를 들어, [java.lang.Thread.State.valueOf(String)](https://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) 메서드의 JNI 형식 시그니처는 "(Ljava/lang/String;)Ljava/lang/Thread$State;"이지만 [java.lang.Thread.State.values](https://developer.android.com/reference/java/lang/Thread.State.html#values) 메서드의 JNI 형식 시그니처는 "()[Ljava/lang/Thread$State;"입니다. 후행 세미콜론에 유의하세요. 해당 세미콜론은 JNI 형식 시그니처의 일부가 ‘됩니다’.

<a name="_JNI_Type_References"></a>

## <a name="jni-type-references"></a>JNI 형식 참조

JNI 형식 참조는 Java 형식 참조와 다릅니다. JNI에서 `java.lang.String`과 같은 정규화된 Java 형식 이름을 사용할 수 없습니다. 컨텍스트에 따라 JNI 변형 `"java/lang/String"` 또는 `"Ljava/lang/String;"`을 대신 사용해야 합니다. 자세한 내용은 아래를 참조하세요.
JNI 형식 참조에는 다음 네 가지 형식이 있습니다.

- **기본 제공**
- **단순**
- **type**
- **array**

### <a name="built-in-type-references"></a>기본 제공 형식 참조

기본 제공 형식 참조는 기본 제공 값 형식을 참조하는 데 사용되는 단일 문자입니다. 매핑은 다음과 같습니다.

- `sbyte`의 경우 `"B"`입니다.
- `short`의 경우 `"S"`입니다.
- `int`의 경우 `"I"`입니다.
- `long`의 경우 `"J"`입니다.
- `float`의 경우 `"F"`입니다.
- `double`의 경우 `"D"`입니다.
- `char`의 경우 `"C"`입니다.
- `bool`의 경우 `"Z"`입니다.
- `void` 메서드 반환 형식의 경우 `"V"`입니다.

<a name="_Simplified_Type_References_1"></a>

### <a name="simplified-type-references"></a>단순 형식 참조

단순 형식 참조는 [JNIEnv.FindClass(string)](xref:Android.Runtime.JNIEnv.FindClass*))에서만 사용할 수 있습니다.
단순 형식 참조를 파생하는 방법에는 다음 두 가지가 있습니다.

1. 정규화된 Java 이름에서 패키지 이름 내에 있고 형식 이름 앞에 있는 모든 `'.'`를 `'/'`로 바꾸고 형식 이름 내의 모든 `'.'`를 `'$'`로 바꿉니다.

1. `'unzip -l android.jar | grep JavaName'`의 출력을 읽습니다.

해당 두 가지 방법 중 하나를 사용하면 Java 형식 [java.lang.Thread.State](https://developer.android.com/reference/java/lang/Thread.State.html)가 단순 형식 참조 `java/lang/Thread$State`에 매핑됩니다.

### <a name="type-references"></a>형식 참조

형식 참조는 기본 제공 형식 참조나 `'L'` 접두사와 `';'` 접미사가 있는 단순 형식 참조입니다. Java 형식 [java.lang.String](https://developer.android.com/reference/java/lang/String.html)의 경우에는 단순 형식 참조가 `"java/lang/String"`이지만 형식 참조는 `"Ljava/lang/String;"`입니다.

형식 참조는 배열 형식 참조 및 JNI 시그니처와 함께 사용됩니다.

형식 참조를 가져오는 또 다른 방법은 `'javap -s -classpath android.jar fully.qualified.Java.Name'`의 출력을 읽는 것입니다.
관련된 형식에 따라, 생성자 선언 또는 메서드 반환 형식을 사용하여 JNI 이름을 확인할 수 있습니다. 예를 들어:

```shell
$ javap -classpath android.jar -s java.lang.Thread.State
Compiled from "Thread.java"
```

```java
public final class java.lang.Thread$State extends java.lang.Enum{
public static final java.lang.Thread$State NEW;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State RUNNABLE;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State BLOCKED;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TIMED_WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TERMINATED;
  Signature: Ljava/lang/Thread$State;
public static java.lang.Thread$State[] values();
  Signature: ()[Ljava/lang/Thread$State;
public static java.lang.Thread$State valueOf(java.lang.String);
  Signature: (Ljava/lang/String;)Ljava/lang/Thread$State;
static {};
  Signature: ()V
}
```

`Thread.State`는 Java 열거형 형식이므로 `valueOf` 메서드의 시그니처를 사용하여 형식 참조가 Ljava/lang/Thread$State;인지 확인할 수 있습니다.

### <a name="array-type-references"></a>배열 형식 참조

배열 형식 참조는 JNI 형식 참조에 접두사로 `'['`가 추가됩니다.
배열을 지정할 때는 단순 형식 참조를 사용할 수 없습니다.

예를 들어 `int[]`는 `"[I"`이고, `int[][]`는 `"[[I"`이고, `java.lang.Object[]`는 `"[Ljava/lang/Object;"`입니다.

## <a name="java-generics-and-type-erasure"></a>Java 제네릭 및 형식 지우기

*대부분*은 JNI를 통해 표시되는 것처럼 Java 제네릭은 존재하지 않습니다.
"좋은 방법"이 있지만 JNI가 제네릭 멤버를 조회하고 호출하는 방식이 아니라 Java가 제네릭과 상호 작용하는 방식일 고려해야 합니다.

JNI을 통해 상호 작용하는 경우 제네릭 형식 또는 멤버와 비제네릭 형식 또는 멤버 간에는 차이가 없습니다. 예를 들어, 제네릭 형식 [java.lang.Class&lt;T&gt;](https://developer.android.com/reference/java/lang/Class.html)도 "원시" 제네릭 형식 `java.lang.Class`이며, 두 형식 모두 동일한 단순 형식 참조 `"java/lang/Class"`를 포함합니다.

## <a name="java-native-interface-support"></a>Java 기본 인터페이스 지원

[Android.Runtime.JNIEnv](xref:Android.Runtime.JNIEnv)는 JNI(Jave 기본 인터페이스)에 대한 관리형 래퍼입니다. 명시적 `JNIEnv*` 매개 변수를 제거하도록 메서드가 변경되었으며 `jobject`, `jclass`, `jmethodID` 등을 대신해서 `IntPtr`이 사용되지만 JNI 함수는 [Java 기본 인터페이스 사양](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html) 내에서 선언됩니다. 예를 들어 [JNI NewObject 함수](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517)를 살펴보겠습니다.

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

이 함수는 [JNIEnv.NewObject](xref:Android.Runtime.JNIEnv.NewObject*) 메서드로 노출됩니다.

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

두 호출 사이를 전환하는 것은 매우 간단합니다. C에서는 다음을 수행합니다.

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

C#에 해당하는 항목은 다음과 같습니다.

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

IntPtr에 Java 개체 인스턴스가 있으면 이 인스턴스로 작업을 수행하려고 할 수 있습니다. [JNIEnv.CallVoidMethod()](xref:Android.Runtime.JNIEnv.CallVoidMethod*)와 같은 JNIEnv 메서드를 사용하여 이 작업을 수행할 수 있지만, 아날로그 C# 래퍼가 이미 있는 경우 JNI 참조에 대해 래퍼를 생성하는 것이 좋습니다. [Extensions.JavaCast\<T>](xref:Android.Runtime.Extensions.JavaCast*) 확장 메서드를 통해 다음과 같은 작업을 수행할 수 있습니다.

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

[Java.Lang.Object.GetObject\<T>](xref:Java.Lang.Object.GetObject*) 메서드를 사용할 수도 있습니다.

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

또한 모든 JNI 함수에 있는 `JNIEnv*` 매개 변수를 제거하여 모든 JNI 함수를 수정했습니다.

## <a name="summary"></a>요약

JNI를 직접 처리하는 것은 비용 문제 때문에 피해야 하는 부적절한 환경입니다. 안타깝지만 항상 피할 수 있는 것은 아닙니다. Android용 Mono에서 바인딩되지 않은 Java 사례가 발생하면 이 가이드에서 몇 가지 도움을 받을 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Java 기본 인터페이스 사양](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java 기본 인터페이스 함수](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
