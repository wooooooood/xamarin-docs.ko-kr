---
title: JNI 및 Xamarin 사용
description: Xamarin Android를 사용 하면 Java 대신 Android C# 앱을 작성할 수 있습니다. GoogleMaps 및 mono를 비롯 한 Java 라이브러리에 대 한 바인딩을 제공 하는 여러 어셈블리가 Xamarin.ios와 함께 제공 됩니다. 그러나 사용할 수 있는 모든 Java 라이브러리에 대해서는 바인딩이 제공 되지 않으며 제공 되는 바인딩은 모든 Java 형식 및 멤버를 바인딩할 수 없습니다. 바인딩되지 않은 Java 형식 및 멤버를 사용 하려면 JNI (Java Native Interface)를 사용할 수 있습니다. 이 문서에서는 JNI를 사용 하 여 Xamarin Android 응용 프로그램에서 Java 형식 및 멤버와 상호 작용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 0fa717a775ff2f1ace9e248a8afde8d373e8a1f8
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724346"
---
# <a name="working-with-jni-and-xamarinandroid"></a>JNI 및 Xamarin 사용

_Xamarin Android를 사용 하면 Java 대신 Android C# 앱을 작성할 수 있습니다. GoogleMaps 및 mono를 비롯 한 Java 라이브러리에 대 한 바인딩을 제공 하는 여러 어셈블리가 Xamarin.ios와 함께 제공 됩니다. 그러나 사용할 수 있는 모든 Java 라이브러리에 대해서는 바인딩이 제공 되지 않으며 제공 되는 바인딩은 모든 Java 형식 및 멤버를 바인딩할 수 없습니다. 바인딩되지 않은 Java 형식 및 멤버를 사용 하려면 JNI (Java Native Interface)를 사용할 수 있습니다. 이 문서에서는 JNI를 사용 하 여 Xamarin Android 응용 프로그램에서 Java 형식 및 멤버와 상호 작용 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

Java 코드를 호출 하는 MCW (관리 되는 호출 가능 래퍼)를 만드는 것은 항상 필요 하거나 가능 하지 않습니다. 대부분의 경우 "인라인" JNI은 바인딩되지 않은 Java 멤버의 일회용 사용을 위해 완벽 하 게 허용 되 고 유용 합니다. JNI를 사용 하 여 전체 .jar 바인딩을 생성 하는 것 보다 Java 클래스에서 단일 메서드를 호출 하는 것이 더 간단 합니다.

Xamarin.ios는 Android의 `android.jar` 라이브러리에 대 한 바인딩을 제공 하는 `Mono.Android.dll` 어셈블리를 제공 합니다. `Mono.Android.dll` 내에 없는 형식 및 멤버와 `android.jar` 내에 없는 형식은 수동으로 바인딩하는 데 사용할 수 있습니다. Java 형식 및 멤버를 바인딩하려면**JNI**( **java Native Interface** )를 사용 하 여 형식을 조회 하 고, 필드를 읽고 쓰고, 메서드를 호출 합니다.

Xamarin.ios의 JNI API는 개념적으로 .NET의 `System.Reflection` API와 매우 유사 합니다 .이를 통해 이름으로 형식 및 멤버를 조회 하 고, 필드 값을 읽고 쓰고, 메서드를 호출할 수 있습니다. JNI 및 `Android.Runtime.RegisterAttribute` 사용자 지정 특성을 사용 하 여 재정의를 지원 하기 위해 바인딩할 수 있는 가상 메서드를 선언할 수 있습니다. 에서 C#구현할 수 있도록 인터페이스를 바인딩할 수 있습니다.

이 문서에서는 다음에 대해 설명 합니다.

- JNI는 형식을 참조 합니다.
- 필드를 조회, 읽기 및 쓰기 하는 방법입니다.
- 메서드를 조회 하 고 호출 하는 방법입니다.
- 관리 코드에서 재정의할 수 있도록 가상 메서드를 노출 하는 방법입니다.
- 인터페이스를 노출 하는 방법입니다.

## <a name="requirements"></a>요구 사항

JNI는 [JNIEnv 네임 스페이스](xref:Android.Runtime.JNIEnv)를 통해 노출 되는 모든 버전의 Xamarin android에서 사용할 수 있습니다.
Java 형식 및 인터페이스를 바인딩하려면 Xamarin Android 4.0 이상을 사용 해야 합니다.

## <a name="managed-callable-wrappers"></a>관리 되는 호출 가능 래퍼

**Mcw**( **관리 되는 호출 가능 래퍼** )는 모든 JNI 기계를 래핑하는 Java 클래스 또는 인터페이스의 *바인딩입니다* .이를 통해 클라이언트 C# 코드에서 JNI의 기본 복잡성에 대해 걱정 하지 않아도 됩니다. 대부분의 `Mono.Android.dll`는 관리 되는 호출 가능 래퍼로 구성 됩니다.

관리 되는 호출 가능 래퍼는 두 가지 용도로 사용 됩니다.

1. 클라이언트 코드가 기본적인 복잡성을 알 필요가 없도록 JNI 사용을 캡슐화 합니다.
1. 하위 클래스 Java 형식으로 사용 하 고 Java 인터페이스를 구현할 수 있도록 합니다.

첫 번째 목적은 소비자가 사용할 간단 하 고 관리 되는 클래스 집합을 갖도록 복잡성의 편의성과 캡슐화를 위한 것입니다. 이렇게 하려면이 문서의 뒷부분에 설명 된 대로 다양 한 [JNIEnv](xref:Android.Runtime.JNIEnv) 멤버를 사용 해야 합니다. 관리 되는 호출 가능 래퍼가 반드시 &ndash; 필요한 것은 아닙니다. "인라인" JNI 사용은 완벽 하 게 허용 되며 바인딩되지 않은 Java 멤버의 일회용 사용에 유용 합니다. 하위 classing 및 인터페이스 구현을 사용 하려면 관리 되는 호출 가능 래퍼를 사용 해야 합니다.

## <a name="android-callable-wrappers"></a>Android 호출 가능 래퍼

Android 런타임 (ART)이 관리 코드를 호출 해야 할 때마다 ACW (android 호출 가능 래퍼)가 필요 합니다. 이러한 래퍼는 런타임에 아트를 사용 하 여 클래스를 등록할 방법이 없기 때문에 필요 합니다.
특히, 정의 된 [eclass](https://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) JNI 함수는 Android 런타임에서 지원 되지 않습니다. Android 호출 가능 래퍼는 런타임 형식 등록 지원 부족을 구성 합니다.

Android 코드에서 관리 코드에 재정의 되거나 구현 된 가상 또는 인터페이스 메서드를 실행 해야 할 때마다 Xamarin.ios는이 메서드가 적절 한 관리 되는 형식으로 디스패치되 게 하려면 Java 프록시를 제공 해야 합니다. 이러한 Java 프록시 형식은 동일한 생성자를 구현 하 고 재정의 된 기본 클래스 및 인터페이스 메서드를 선언 하는 "동일한" 기본 클래스 및 Java 인터페이스 목록을 관리 되는 형식으로 포함 하는 Java 코드입니다.

Android 호출 가능 래퍼는 [빌드 프로세스](~/android/deploy-test/building-apps/build-process.md)중에 **monodroid** 프로그램에 의해 생성 되며, 직접 또는 간접적 [으로 상속 하](xref:Java.Lang.Object)는 모든 형식에 대해 생성 됩니다.

### <a name="implementing-interfaces"></a>인터페이스 구현

Android 인터페이스 (예: [IComponentCallbacks](xref:Android.Content.IComponentCallbacks))를 구현 해야 하는 경우가 있습니다.

모든 Android 클래스 및 인터페이스는 [IJavaObject](xref:Android.Runtime.IJavaObject) 인터페이스를 확장 합니다. 따라서 모든 Android 형식은 `IJavaObject`를 구현 해야 합니다.
Xamarin.ios는 `IJavaObject`를 사용 하 여 지정 된 관리 되는 형식에 대 한 Java 프록시 (Android 호출 가능 래퍼)를 Android에 제공 하는 &ndash;이 팩트를 활용 합니다. **Monodroid** 는 `Java.Lang.Object` 서브 클래스 (`IJavaObject`를 구현 해야 함)만 찾으므로, 서브클래싱 `Java.Lang.Object`는 관리 코드에서 인터페이스를 구현 하는 방법을 제공 합니다. 예를 들면 다음과 같습니다.

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

*이 문서의 나머지 부분에서는 예 고 없이 변경 될 수 있는 구현 세부 정보를 제공* 합니다. 개발자는 내부적으로 진행 되는 작업에 대해 궁금한 점이 있을 수 있습니다.

예를 들어, 다음 C# 소스가 제공 됩니다.

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

이 **프로그램은** 다음과 같은 Android 호출 가능 래퍼를 생성 합니다.

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

기본 클래스는 유지 되 고 관리 코드 내에서 재정의 되는 각 메서드에 대해 네이티브 메서드 선언이 제공 됩니다.

### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute 및 ExportFieldAttribute

일반적으로 Xamarin.ios는 ACW를 구성 하는 Java 코드를 자동으로 생성 합니다. 이 세대는 클래스가 Java 클래스에서 파생 되 고 기존 Java 메서드를 재정의 하는 경우 클래스 및 메서드 이름을 기반으로 합니다. 그러나 일부 시나리오에서는 아래에 설명 된 대로 코드 생성이 적절 하지 않습니다.

- Android는 레이아웃 XML 특성에서 작업 이름을 지원 합니다 (예 [: android: onClick](xref:Android.Views.View.IOnClickListener.OnClick*) xml 특성). 지정 된 경우 팽창 View 인스턴스는 Java 메서드를 조회 하려고 합니다.

- [Java. Serializable](https://developer.android.com/reference/java/io/Serializable.html) 인터페이스에는 `readObject` 및 `writeObject` 메서드가 필요 합니다. 이러한 메서드는이 인터페이스의 멤버가 아니므로 해당 하는 관리 되는 구현에서 이러한 메서드를 Java 코드에 노출 하지 않습니다.

- [넣는](xref:Android.OS.Parcelable) 인터페이스는 구현 클래스에 `Parcelable.Creator`형식의 정적 필드 `CREATOR` 있어야 합니다. 생성 된 Java 코드에는 몇 가지 명시적 필드가 필요 합니다. 표준 시나리오를 사용 하 여 관리 코드의 Java 코드에서 필드를 출력할 수 있는 방법은 없습니다.

코드 생성은 Xamarin. Android 4.2부터 임의의 이름으로 임의의 Java 메서드를 생성 하는 솔루션을 제공 하지 않으므로, [Exportattribute](xref:Java.Interop.ExportAttribute) 및 [ExportFieldAttribute](xref:Java.Interop.ExportFieldAttribute) 는 위의 시나리오에 대 한 솔루션을 제공 하기 위해 도입 되었습니다. 두 특성은 모두 `Java.Interop` 네임 스페이스에 있습니다.

- `ExportAttribute` &ndash;는 메서드 이름과 예상 되는 예외 형식을 지정 하 여 Java에서 명시적 "throw"를 제공 합니다. 메서드에서 사용 되는 경우 메서드는 디스패치 코드를 생성 하는 Java 메서드를 관리 되는 메서드에 대 한 해당 JNI 호출로 "내보냅니다". `android:onClick` 및 `java.io.Serializable`와 함께 사용할 수 있습니다.

- `ExportFieldAttribute` &ndash;는 필드 이름을 지정 합니다. 필드 이니셜라이저로 작동 하는 메서드에 상주 합니다. `android.os.Parcelable`와 함께 사용할 수 있습니다.

#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>ExportAttribute 및 ExportFieldAttribute 문제 해결

- 코드 또는 종속 라이브러리의 일부 메서드에 `ExportAttribute` 또는 `ExportFieldAttribute`를 사용 하는 경우 **mono** 가 누락 되어 패키징을 수행할 수 없습니다. &ndash; **을 추가 해야 합니다.** 이 어셈블리는 Java의 콜백 코드를 지원 하기 위해 격리 되어 있습니다. 응용 프로그램에 크기를 추가 하므로 **Mono** 와는 별개입니다.

- 릴리스 빌드에서 내보내기 방법에 대 한 `MissingMethodException` &ndash; 릴리스 빌드에서는 내보내기 방법에 대해 발생 `MissingMethodException`. 이 문제는 최신 버전의 Xamarin. Android에서 해결 되었습니다.

### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` 및 `ExportFieldAttribute`는 Java 런타임 코드에서 사용할 수 있는 기능을 제공 합니다. 이 런타임 코드는 해당 특성에 따라 생성 된 JNI 메서드를 통해 관리 코드에 액세스 합니다. 결과적으로, 관리 되는 메서드가 바인딩하는 기존 Java 메서드는 없습니다. 따라서 Java 메서드는 관리 되는 메서드 시그니처에서 생성 됩니다.

그러나이 경우에는 완전히 결정 되지 않습니다. 특히 다음과 같이 관리 되는 형식과 Java 형식 간의 일부 고급 매핑에서 이러한 경우가 그렇습니다.

- InputStream
- OutputStream
- XmlPullParser
- XmlResourceParser

내보낸 메서드에 이러한 형식이 필요한 경우 `ExportParameterAttribute`를 사용 하 여 해당 매개 변수 또는 반환 값에 형식을 명시적으로 지정 해야 합니다.

### <a name="annotation-attribute"></a>Annotation 특성

Xamarin Android 4.2에서는 구현 형식을 특성 (system.string)으로 `IAnnotation` 변환 하 고 Java 래퍼의 주석 생성에 대 한 지원을 추가 했습니다.

이는 다음과 같은 방향성이 변경 됨을 의미 합니다.

- 바인딩 생성기는 `java.Lang.Deprecated`에서 `Java.Lang.DeprecatedAttribute`를 생성 합니다 (관리 코드에서 `[Obsolete]` 해야 함).

- 이는 기존 `Java.Lang.Deprecated` 클래스가 사라질 것을 의미 하지는 않습니다. 이러한 Java 기반 개체는 여전히 일반적인 Java 개체로 사용할 수 있습니다 (이러한 사용법이 있는 경우). `Deprecated` 및 `DeprecatedAttribute` 클래스가 있습니다.

- `Java.Lang.DeprecatedAttribute` 클래스가 `[Annotation]`으로 표시 되어 있습니다. 이 `[Annotation]` 특성에서 상속 된 사용자 지정 특성이 있는 경우 msbuild 작업은 ACW (Android 호출 가능 래퍼)에서 해당 사용자 지정 특성 (@Deprecated)에 대 한 Java 주석을 생성 합니다.

- 주석은 클래스, 메서드 및 내보낸 필드 (관리 코드의 메서드)로 생성 될 수 있습니다.

포함 하는 클래스 (주석이 추가 된 클래스 자체 또는 주석이 추가 된 멤버를 포함 하는 클래스)가 등록 되지 않은 경우 주석을 포함 하 여 전체 Java 클래스 소스가 전혀 생성 되지 않습니다. 메서드의 경우 `ExportAttribute`를 지정 하 여 명시적으로 생성 되 고 주석이 달린 메서드를 가져올 수 있습니다. 또한 Java 주석 클래스 정의를 "생성" 하는 기능이 아닙니다. 즉, 특정 주석에 대 한 사용자 지정 관리 되는 특성을 정의 하는 경우 해당 Java 주석 클래스가 포함 된 다른 jar 라이브러리를 추가 해야 합니다. 주석 형식을 정의 하는 Java 소스 파일을 추가 하는 것 만으로는 충분 하지 않습니다. Java 컴파일러는 **apt**와 동일한 방식으로 작동 하지 않습니다.

또한 다음과 같은 제한 사항이 적용 됩니다.

- 이 변환 프로세스는 지금 까지의 주석 유형에 `@Target` 주석을 고려 하지 않습니다.

- 속성에 대 한 특성은 작동 하지 않습니다. 대신 속성 getter 또는 setter에 대해 특성을 사용 합니다.

## <a name="class-binding"></a>클래스 바인딩

클래스에 바인딩하는 것은 기본 Java 형식의 호출을 단순화 하기 위해 관리 되는 호출 가능 래퍼를 작성 함을 의미 합니다.

에서 C# 재정의할 수 있도록 가상 및 추상 메서드를 바인딩하는 데 Xamarin. Android 4.0이 필요 합니다. 그러나 모든 버전의 Xamarin Android는 재정의를 지원 하지 않고 비가상 메서드, 정적 메서드 또는 가상 메서드를 바인딩할 수 있습니다.

바인딩에는 일반적으로 다음과 같은 항목이 포함 됩니다.

- [바인딩되는 Java 형식에 대 한 JNI 핸들](#_Looking_up_Java_Types)입니다.

- [바인딩된 각 필드에 대 한 필드 id 및 속성을 JNI](#_Instance_Fields)합니다.

- [바인딩된 각 메서드에 대 한 메서드 id 및 메서드를 JNI](#_Instance_Methods)합니다.

- 하위 classing이 필요한 경우 형식 선언 [에 registerattribute 사용자 지정](xref:Android.Runtime.RegisterAttribute) 특성을 포함 해야 합니다 [. DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) 을 `true`로 설정 합니다.

### <a name="declaring-type-handle"></a>형식 핸들 선언

필드 및 메서드 조회 메서드에는 선언 형식을 참조 하는 개체 참조가 필요 합니다. 규칙에 따라이는 `class_ref` 필드에 저장 됩니다.

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

`CLASS` 토큰에 대 한 자세한 내용은 [JNI 형식 참조](#_JNI_Type_References) 섹션을 참조 하세요.

### <a name="binding-fields"></a>바인딩 필드

Java 필드는 C# 속성으로 노출 됩니다. 예를 들어 java 필드 [java.lang.System.in](https://developer.android.com/reference/java/lang/System.html#in) 는 C# 속성 [Java.Lang.JavaSystem.In](xref:Java.Lang.JavaSystem.In)로 바인딩됩니다.
또한 JNI는 정적 필드와 인스턴스 필드를 구분 하므로 속성을 구현할 때 다른 메서드가 사용 됩니다.

필드 바인딩에는 세 가지 메서드 집합이 포함 됩니다.

1. *Get field id* 메서드입니다. *Get field id* 메서드는 *get field value* 및 *set field value* 메서드에서 사용할 필드 핸들을 반환 합니다. 필드 id를 얻으려면 선언 형식, 필드 이름 및 필드의 [JNI 형식 서명을](#JNI_Type_Signatures) 알아야 합니다.

1. *Get field value* 메서드 이러한 메서드는 필드 핸들이 필요 하며 Java에서 필드의 값을 읽습니다.
    사용할 메서드는 필드의 형식에 따라 달라 집니다.

1. *Set field value* 메서드 이러한 메서드는 필드 핸들이 필요 하며 Java 내에서 필드의 값을 작성 해야 합니다. 사용할 메서드는 필드의 형식에 따라 달라 집니다.

[정적 필드](#_Static_Fields) 는 [JNIEnv](xref:Android.Runtime.JNIEnv.GetStaticMethodID*), `JNIEnv.GetStatic*Field`및 [JNIEnv](xref:Android.Runtime.JNIEnv.SetStaticField*) 메서드를 사용 합니다.

 [인스턴스 필드](#_Instance_Fields) 는 [JNIEnv](xref:Android.Runtime.JNIEnv.GetFieldID*), `JNIEnv.Get*Field`및 [SetField](xref:Android.Runtime.JNIEnv.SetField*) 메서드를 사용 합니다.

예를 들어 정적 속성 `JavaSystem.In`는 다음과 같이 구현할 수 있습니다.

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

참고: [FromJniHandle](xref:Android.Runtime.InputStreamInvoker.FromJniHandle*) 를 사용 하 여 JNI 참조를 `System.IO.Stream` 인스턴스로 변환 하 고 [JNIEnv. GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) 는 로컬 참조를 반환 하기 때문에 `JniHandleOwnership.TransferLocalRef`를 사용 합니다.

대부분의 [Android 런타임](xref:Android.Runtime) 형식에는 JNI 참조를 원하는 형식으로 변환 하는 `FromJniHandle` 메서드가 있습니다.

### <a name="method-binding"></a>메서드 바인딩

Java 메서드는 C# 메서드 및 C# 속성으로 노출 됩니다. 예를 들어 java 메서드 [runFinalizersOnExit](https://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) 메서드는 [runFinalizersOnExit](xref:Java.Lang.Runtime.RunFinalizersOnExit*) 메서드로 바인딩되고, java. c a. g a [c. Getclass](https://developer.android.com/reference/java/lang/Object.html#getClass) 메서드는 [java. c](xref:Java.Lang.Object.Class) a c a. g a c 속성으로 바인딩됩니다.

메서드 호출은 다음과 같은 두 단계로 진행 됩니다.

1. 호출할 메서드의 *get 메서드 id* 입니다. *Get 메서드 id* 메서드는 메서드 호출 메서드에서 사용할 메서드 핸들을 반환 합니다. 메서드 id를 얻으려면 선언 형식, 메서드의 이름 및 메서드의 [JNI 형식 서명을](#JNI_Type_Signatures) 알아야 합니다.

1. 메서드를 호출합니다.

필드와 마찬가지로 메서드 id를 가져오고 메서드를 호출 하는 데 사용 하는 메서드는 정적 메서드와 인스턴스 메서드 사이에서 다릅니다.

[정적 메서드](#_Static_Methods_1) 는 [JNIEnv ()](xref:Android.Runtime.JNIEnv.GetStaticMethodID*) 를 사용 하 여 메서드 id를 조회 하 고 호출에 `JNIEnv.CallStatic*Method` 된 메서드 패밀리를 사용 합니다.

[인스턴스 메서드](#_Instance_Methods) 는 [JNIEnv](xref:Android.Runtime.JNIEnv.GetMethodID*) 을 사용 하 여 메서드 id를 조회 하 고 `JNIEnv.Call*Method` 및 `JNIEnv.CallNonvirtual*Method` 메서드 패밀리를 사용 하 여 호출 합니다.

메서드 바인딩은 단순히 메서드 호출 보다 많을 수 있습니다. 메서드 바인딩에는 메서드를 재정의 하거나 (추상 메서드와 최종이 아닌 메서드) 구현 (인터페이스 메서드의 경우) 할 수 있는 기능도 포함 되어 있습니다. [지원 상속, 인터페이스](#_Supporting_Inheritance,_Interfaces_1) 섹션은 가상 메서드 및 인터페이스 메서드를 지 원하는 복잡성을 다룹니다.

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>정적 메서드

정적 메서드를 바인딩하는 것은 `JNIEnv.GetStaticMethodID`를 사용 하 여 메서드 핸들을 가져온 다음 메서드의 반환 형식에 따라 적절 한 `JNIEnv.CallStatic*Method` 메서드를 사용 하는 것을 포함 합니다. 다음은 런타임에 대 한 바인딩의 예입니다 [. getruntime](https://developer.android.com/reference/java/lang/Runtime.html#getRuntime()) 메서드:

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

`id_getRuntime`정적 필드에 메서드 핸들을 저장 합니다. 이는 성능 최적화 이므로 모든 호출에서 메서드 핸들을 조회할 필요가 없습니다. 이러한 방식으로 메서드 핸들을 캐시할 필요는 없습니다. 메서드 핸들을 가져오면 [JNIEnv](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) 메서드를 호출 하는 데 사용 됩니다. `JNIEnv.CallStaticObjectMethod` 반환 된 Java 인스턴스의 핸들을 포함 하는 `IntPtr` 반환 합니다.
[JniHandleOwnership&lt;t&gt;(IntPtr,)](xref:Java.Lang.Object.GetObject*) 는 java 핸들을 강력한 형식의 개체 인스턴스로 변환 하는 데 사용 됩니다.

#### <a name="non-virtual-instance-method-binding"></a>비가상 인스턴스 메서드 바인딩

`final` 인스턴스 메서드 바인딩 또는 재정의를 요구 하지 않는 인스턴스 메서드는 `JNIEnv.GetMethodID`를 사용 하 여 메서드 핸들을 가져온 다음 메서드의 반환 형식에 따라 적절 한 `JNIEnv.Call*Method` 메서드를 사용 해야 합니다. `Object.Class` 속성에 대 한 바인딩의 예는 다음과 같습니다.

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

`id_getClass`정적 필드에 메서드 핸들을 저장 합니다.
이는 성능 최적화 이므로 모든 호출에서 메서드 핸들을 조회할 필요가 없습니다. 이러한 방식으로 메서드 핸들을 캐시할 필요는 없습니다. 메서드 핸들을 가져오면 [JNIEnv](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) 메서드를 호출 하는 데 사용 됩니다. `JNIEnv.CallStaticObjectMethod` 반환 된 Java 인스턴스의 핸들을 포함 하는 `IntPtr` 반환 합니다.
[JniHandleOwnership&lt;t&gt;(IntPtr,)](xref:Java.Lang.Object.GetObject*) 는 java 핸들을 강력한 형식의 개체 인스턴스로 변환 하는 데 사용 됩니다.

### <a name="binding-constructors"></a>바인딩 생성자

생성자는 이름이 `"<init>"`Java 메서드입니다. Java 인스턴스 메서드와 마찬가지로 `JNIEnv.GetMethodID`는 생성자 핸들을 조회 하는 데 사용 됩니다. Java 메서드와 달리 [JNIEnv 개체](xref:Android.Runtime.JNIEnv.NewObject*) 메서드는 생성자 메서드 핸들을 호출 하는 데 사용 됩니다. `JNIEnv.NewObject`의 반환 값은 JNI 로컬 참조입니다.

```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

일반적으로 클래스 바인딩은 클래스의 [하위 클래스를 만듭니다.](xref:Java.Lang.Object)
`Java.Lang.Object`서브클래싱 할 때 추가 의미 체계는 play로 제공 됩니다. `Java.Lang.Object` 인스턴스는 `Java.Lang.Object.Handle` 속성을 통해 Java 인스턴스에 대 한 전역 참조를 유지 합니다.

1. `Java.Lang.Object` 기본 생성자가 Java 인스턴스를 할당 합니다.

1. 형식에 `RegisterAttribute` 있고 `RegisterAttribute.DoNotGenerateAcw` `true` 되는 경우 `RegisterAttribute.Name` 형식의 인스턴스는 기본 생성자를 통해 생성 됩니다.

1. 그렇지 않으면 `this.GetType`에 해당 하는 acw ( [Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md) )가 기본 생성자를 통해 인스턴스화됩니다. Android 호출 가능 래퍼는 `RegisterAttribute.DoNotGenerateAcw` `true`으로 설정 되지 않은 모든 `Java.Lang.Object` 서브 클래스에 대해 패키지를 만드는 동안 생성 됩니다.

클래스 바인딩이 아닌 형식의 경우이는 예상 되는 의미 체계입니다. `Mono.Samples.HelloWorld.HelloAndroid` C# 인스턴스를 인스턴스화하면 생성 된 Android 호출 가능 래퍼로 Java `mono.samples.helloworld.HelloAndroid` 인스턴스를 생성 해야 합니다.

클래스 바인딩의 경우 Java 형식에 기본 생성자가 포함 되어 있거나 다른 생성자를 호출 하지 않아도 되는 경우이 동작은 올바른 동작 일 수 있습니다. 그렇지 않으면 다음 작업을 수행 하는 생성자를 제공 해야 합니다.

1. 기본 `Java.Lang.Object` 생성자 대신 [JniHandleOwnership (IntPtr,)](xref:Java.Lang.Object#ctor*) 를 호출 합니다. 새 Java 인스턴스가 생성 되는 것을 방지 하기 위해 필요 합니다.

1. Java 인스턴스를 만들기 전에 [java. 개체 핸들](xref:Java.Lang.Object.Handle) 의 값을 확인 하십시오. `Object.Handle` 속성은 Android 호출 가능 래퍼가 Java 코드에서 생성 된 경우 `IntPtr.Zero` 이외의 값을 가지 며, 만들어진 Android 호출 가능 래퍼 인스턴스를 포함 하도록 클래스 바인딩이 생성 됩니다. 예를 들어 Android가 `mono.samples.helloworld.HelloAndroid` 인스턴스를 만들 때 Android 호출 가능 래퍼가 먼저 생성 되 고, Java `HelloAndroid` 생성자는 생성자를 실행 하기 전에 `Object.Handle` 속성을 Java 인스턴스로 설정 하 여 해당 `Mono.Samples.HelloWorld.HelloAndroid` 형식의 인스턴스를 만듭니다.

1. 현재 런타임 형식이 선언 형식과 다른 경우 해당 Android 호출 가능 래퍼의 인스턴스를 만들고 [SetHandle](xref:Java.Lang.Object.SetHandle*) 를 사용 하 여 [JNIEnv](xref:Android.Runtime.JNIEnv.CreateInstance*)에서 반환 된 핸들을 저장 해야 합니다.

1. 현재 런타임 형식이 선언 형식과 같은 경우 Java 생성자를 호출 하 고 [SetHandle](xref:Java.Lang.Object.SetHandle*) 를 사용 하 여 `JNIEnv.NewInstance`에서 반환 된 핸들을 저장 합니다.

예를 들어, [java. 정수 (int)](https://developer.android.com/reference/java/lang/Integer.html#Integer(int)) 생성자를 살펴보세요. 다음으로 바인딩됩니다.

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

[JNIEnv](xref:Android.Runtime.JNIEnv.CreateInstance*) 메서드는 `JNIEnv.FindClass`에서 반환 된 값에 대 한 `JNIEnv.FindClass`, `JNIEnv.GetMethodID`, `JNIEnv.NewObject`및 `JNIEnv.DeleteGlobalReference`를 수행 하는 도우미입니다. 자세한 내용은 다음 섹션을 참조하세요.

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>상속, 인터페이스 지원

Java 형식을 서브클래싱 하거나 Java 인터페이스를 구현 하려면 패키징 프로세스 중에 모든 `Java.Lang.Object` 서브 클래스에 대해 생성 되는 acws ( [Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md) )를 생성 해야 합니다. ACW 생성은 [Android. registerattribute](xref:Android.Runtime.RegisterAttribute) 사용자 지정 특성을 통해 제어 됩니다.

형식의 C# 경우 `[Register]` 사용자 지정 특성 생성자에는 해당 Java 형식에 대 한 [JNI 단순 형식 참조](#_Simplified_Type_References_1) 의 인수가 하나 필요 합니다. 이를 통해 Java와 C#사이에 다른 이름을 제공할 수 있습니다.

Xamarin Android 4.0 이전에는 `[Register]` 사용자 지정 특성을 기존 Java 형식 "별칭"에 사용할 수 없습니다. 이는 ACW 생성 프로세스에서 발생 한 모든 `Java.Lang.Object` 서브 클래스에 대해 Acw를 생성 하기 때문입니다.

Xamarin Android 4.0에는 [DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) 속성이 도입 되었습니다. 이 속성은 ACW 생성 프로세스에서 주석이 추가 된 형식을 *건너뛰도록* 지시 하 여 패키지를 만들 때 acw가 생성 되지 않도록 하는 새로운 관리 되는 호출 가능 래퍼를 선언할 수 있도록 합니다. 이렇게 하면 기존 Java 형식을 바인딩할 수 있습니다. 예를 들어, 정수에를 추가 하 고 결과를 반환 하는 메서드 하나 (`add`)를 포함 하는 다음과 같은 간단한 Java 클래스 `Adder`을 살펴보겠습니다.

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

`Adder` 형식은 다음과 같이 바인딩할 수 있습니다.

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

여기에서 `Adder` C# 형식은 `Adder` Java 형식으로 *별칭* 을 합니다. `[Register]` 특성은 `mono.android.test.Adder` Java 형식의 JNI 이름을 지정 하는 데 사용 되 고, `DoNotGenerateAcw` 속성은 ACW 생성을 금지 하는 데 사용 됩니다. 이로 인해 `ManagedAdder` 형식에 대 한 ACW가 생성 되 고 `mono.android.test.Adder` 형식을 올바르게 서브 클래스 합니다. `RegisterAttribute.DoNotGenerateAcw` 속성을 사용 하지 않으면 Xamarin.ios 빌드 프로세스에서 새로운 `mono.android.test.Adder` Java 유형을 생성 합니다. 이렇게 하면 `mono.android.test.Adder` 형식이 두 개의 개별 파일에 두 번 표시 되므로 컴파일 오류가 발생 합니다.

### <a name="binding-virtual-methods"></a>가상 메서드 바인딩

Java `Adder` 형식을 서브 클래스 `ManagedAdder` 하지만 특히 흥미롭습니다. `Adder` 형식에서 C# 가상 메서드를 정의 하지 않으므로 `ManagedAdder` 아무것도 재정의할 수 없습니다.

서브 클래스에서 재정의할 수 있도록 하는 `virtual` 메서드를 바인딩하는 데는 다음 두 가지 범주에 해당 하는 몇 가지 작업이 필요 합니다.

1. **메서드 바인딩**

1. **메서드 등록**

#### <a name="method-binding"></a>메서드 바인딩

메서드 바인딩을 사용 하려면 C# `Adder` 정의에 두 개의 지원 멤버 (`ThresholdType`및 `ThresholdClass`를 추가 해야 합니다.

##### <a name="thresholdtype"></a>ThresholdType

`ThresholdType` 속성은 바인딩의 현재 형식을 반환 합니다.

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType`는 가상 메서드와 비가상 메서드 디스패치를 수행 해야 하는 시기를 결정 하기 위해 메서드 바인딩에서 사용 됩니다. 선언 C# 형식에 해당 하는 `System.Type` 인스턴스를 항상 반환 해야 합니다.

##### <a name="thresholdclass"></a>ThresholdClass

`ThresholdClass` 속성은 바인딩된 형식에 대 한 JNI 클래스 참조를 반환 합니다.

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass`은 비가상 메서드를 호출할 때 메서드 바인딩에 사용 됩니다.

#### <a name="binding-implementation"></a>바인딩 구현

메서드 바인딩 구현은 Java 메서드의 런타임 호출을 담당 합니다. 또한 메서드 등록의 일부인 사용자 지정 특성 선언 `[Register]` 포함 되어 있으며, 메서드 등록 섹션에서 설명 합니다.

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

`id_add` 필드에는 호출할 Java 메서드의 메서드 ID가 포함 되어 있습니다. `id_add` 값은 선언 클래스 (`class_ref`), Java 메서드 이름 (`"add"`) 및 메서드의 JNI 서명 (`"(II)I"`)을 필요로 하는 `JNIEnv.GetMethodID`에서 가져옵니다.

메서드 ID를 가져온 후에는 `GetType`를 `ThresholdType` 비교 하 여 가상 또는 비가상 디스패치가 필요한 지 여부를 확인 합니다. `Handle`에서 메서드를 재정의 하는 Java 할당 서브 클래스를 참조할 수 있으므로 `GetType` `ThresholdType`일치 하는 경우에는 가상 디스패치가 필요 합니다.

`GetType` `ThresholdType`와 일치 하지 않으면 `Adder` 서브클래싱된 (예: `ManagedAdder`), 서브 클래스가 `Adder.Add`를 호출한 경우에만 `base.Add`구현이 호출 됩니다. 이는 `ThresholdClass` 제공 되는 가상이 아닌 디스패치 사례입니다. `ThresholdClass` 호출할 메서드 구현을 제공 하는 Java 클래스를 지정 합니다.

#### <a name="method-registration"></a>메서드 등록

`Adder.Add` 메서드를 재정의 하는 업데이트 된 `ManagedAdder` 정의가 있다고 가정 합니다.

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

`Adder.Add`에 `[Register]` 사용자 지정 특성이 있음을 기억 하세요.

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

`[Register]` 사용자 지정 특성 생성자는 세 개의 값을 허용 합니다.

1. 이 경우 `"add"` Java 메서드의 이름입니다.

1. 이 경우 `"(II)I"` 메서드의 JNI 형식 시그니처입니다.

1. 이 경우 *커넥터 메서드* `GetAddHandler`입니다.
    커넥터 메서드는 나중에 설명 합니다.

처음 두 매개 변수를 사용 하 여 ACW 생성 프로세스에서 메서드를 재정의 하는 메서드 선언을 생성할 수 있습니다. 결과 ACW는 다음 코드 중 일부를 포함 합니다.

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

`@Override` 메서드는로 선언 됩니다 .이 메서드는 동일한 이름의 `n_`접두사 메서드에 위임 합니다. 이렇게 하면 Java 코드가 `ManagedAdder.add`를 호출할 때 재정의 C# `ManagedAdder.Add` 메서드를 실행할 수 있는 `ManagedAdder.n_add` 호출 됩니다.

따라서 가장 중요 한 질문은 `ManagedAdder.n_add` `ManagedAdder.Add`에 연결 되는 방법입니다.

Java `native` 메서드는 [JNI RegisterNatives 함수](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734)를 통해 Java (Android runtime) 런타임에 등록 됩니다.
`RegisterNatives`는 Java 메서드 이름, JNI 형식 시그니처 및 호출 하는 함수 포인터와 [JNI 호출 규칙](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715)을 포함 하는 구조체 배열을 사용 합니다.
함수 포인터는 두 개의 포인터 인수 다음에 메서드 매개 변수를 사용 하는 함수 여야 합니다. Java `ManagedAdder.n_add` 메서드는 다음 C 프로토타입이 있는 함수를 통해 구현 해야 합니다.

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.ios는 `RegisterNatives` 메서드를 노출 하지 않습니다. 대신 ACW와 MCW는 `RegisterNatives`을 호출 하는 데 필요한 정보를 제공 합니다. ACW는 메서드 이름 및 JNI 형식 서명을 포함 하 고, 누락 된 항목은 후크 할 함수 포인터입니다.

*커넥터 메서드가* 제공 되는 위치입니다. 세 번째 `[Register]` 사용자 지정 특성 매개 변수는 등록 된 형식에 정의 된 메서드의 이름 또는 매개 변수를 허용 하지 않고 `System.Delegate`를 반환 하는 등록 된 형식의 기본 클래스입니다. 반환 된 `System.Delegate`는 올바른 JNI 함수 서명이 있는 메서드를 참조 합니다. 마지막으로, 대리자가 Java에 제공 되는 것 처럼 커넥터 메서드가 반환 하는 대리자는 루트 *여야 합니다.*

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

`GetAddHandler` 메서드는 `n_Add` 메서드를 참조 하는 `Func<IntPtr, IntPtr, int, int,
int>` 대리자를 만든 다음 [JNINativeWrapper. system.delegate.createdelegate](xref:Android.Runtime.JNINativeWrapper.CreateDelegate*)를 호출 합니다.
`JNINativeWrapper.CreateDelegate`는 제공 된 메서드를 try/catch 블록으로 래핑하여 처리 되지 않은 예외가 처리 되 고 [Androidevent](xref:Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser) 이벤트가 발생 하도록 합니다. 결과 대리자는 GC가 대리자를 해제 하지 않도록 정적 `cb_add` 변수에 저장 됩니다.

마지막으로 `n_Add` 메서드는 JNI 매개 변수를 해당 하는 관리 되는 형식으로 마샬링하고 메서드 호출을 위임 합니다.

참고: Java 인스턴스를 통해 MCW를 가져올 때 항상 `JniHandleOwnership.DoNotTransfer`를 사용 합니다. 이러한 항목을 로컬 참조로 처리 하 여 `JNIEnv.DeleteLocalRef`를 호출 하면 관리&gt; Java&gt; 관리 되는 스택 전환이 중단 됩니다.

### <a name="complete-adder-binding"></a>Adder 바인딩 완료

`mono.android.tests.Adder` 형식에 대 한 완전 한 관리 바인딩은 다음과 같습니다.

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

다음 조건과 일치 하는 형식을 작성할 때:

1. 서브 클래스 `Java.Lang.Object`

1. `[Register]` 사용자 지정 특성 포함

1. `RegisterAttribute.DoNotGenerateAcw`은 `true`입니다.

그런 다음 GC 상호 작용의 경우 형식에는 런타임에 `Java.Lang.Object` 또는 `Java.Lang.Object` 하위 클래스를 참조할 수 있는 필드가 없어야 *합니다* . 예를 들어 `System.Object` 형식의 필드와 모든 인터페이스 형식이 허용 되지 않습니다. `Java.Lang.Object` 인스턴스를 참조할 수 없는 형식 (예: `System.String` 및 `List<int>`)이 허용 됩니다. 이러한 제한 사항은 GC에의 한 개체 수집을 중간에 방지 하는 것입니다.

`Java.Lang.Object` 인스턴스를 참조할 수 있는 인스턴스 필드가 형식에 포함 되어야 하는 경우 필드 형식을 `System.WeakReference` 하거나 `GCHandle`해야 합니다.

## <a name="binding-abstract-methods"></a>바인딩 추상 메서드

바인딩 `abstract` 메서드는 가상 메서드와 거의 동일 합니다. 두 가지 차이점이 있습니다.

1. 추상 메서드는 추상 메서드입니다. `[Register]` 특성 및 연결 된 메서드 등록을 그대로 유지 하 고 메서드 바인딩이 `Invoker` 형식으로 바로 이동 됩니다.

1. 추상 형식을 서브 클래스 하는 `abstract` `Invoker` 형식이 생성 됩니다. `Invoker` 형식은 기본 클래스에 선언 된 모든 추상 메서드를 재정의 해야 하며, 재정의 된 구현은 메서드 바인딩 구현 이지만 비가상 디스패치 사례는 무시 해도 됩니다.

예를 들어 위의 `mono.android.test.Adder.add` 메서드가 `abstract`되어 있다고 가정 합니다. C# 바인딩이 변경 되어 `Adder.Add` 추상이 되 고, `Adder.Add`구현 된 새 `AdderInvoker` 형식이 정의 됩니다.

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

`Invoker` 형식은 Java에서 만든 인스턴스에 대 한 JNI 참조를 가져올 때만 필요 합니다.

## <a name="binding-interfaces"></a>바인딩 인터페이스

바인딩 인터페이스는 개념적으로 가상 메서드를 포함 하는 바인딩 클래스와 유사 하지만 대부분의 세부 사항에 따라 미묘한 방법이 다릅니다. 다음 [Java 인터페이스 선언을](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14)참조 하세요.

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

인터페이스 바인딩에는 인터페이스 정의와 인터페이스 C# 에 대 한 호출자 정의가 포함 됩니다.

### <a name="interface-definition"></a>인터페이스 정의

인터페이스 C# 정의는 다음 요구 사항을 충족 해야 합니다.

- 인터페이스 정의에는 `[Register]` 사용자 지정 특성이 있어야 합니다.

- 인터페이스 정의는 `IJavaObject interface`확장 해야 합니다.
    이렇게 하지 않으면 ACWs가 Java 인터페이스에서 상속 하지 않습니다.

- 각 인터페이스 메서드는 해당 하는 Java 메서드 이름, JNI 서명 및 커넥터 메서드를 지정 하는 `[Register]` 특성을 포함 해야 합니다.

- 커넥터 메서드는 커넥터 메서드가 위치할 수 있는 형식을 지정 해야 합니다.

`abstract` 및 `virtual` 메서드를 바인딩할 때 커넥터 메서드는 등록 중인 형식의 상속 계층 구조 내에서 검색 됩니다. 인터페이스에는 본문이 포함 된 메서드가 없을 수 있으므로이는 작동 하지 않으므로 커넥터 메서드가 있는 위치를 나타내는 형식을 지정 해야 합니다. 형식은 커넥터 메서드 문자열 내에서 콜론 `':'`후에 지정 되며 호출자를 포함 하는 형식의 정규화 된 어셈블리 형식 이름 이어야 합니다.

인터페이스 메서드 선언은 *호환* 되는 형식을 사용 하 여 해당 Java 메서드를 변환 하는 것입니다. Java 기본 형식의 경우 호환 되는 형식이 해당 C# 형식입니다 (예: java `int` `int`됩니다 C# . 참조 형식의 경우 호환 되는 형식은 적절 한 Java 형식의 JNI 핸들을 제공할 수 있는 형식입니다.

인터페이스 멤버는 Java에 의해 직접 호출 되지 않습니다. &ndash; 호출은 호출자 형식 &ndash;를 통해 조정 되므로 일부 유연성이 허용 됩니다.

Java 진행률 인터페이스는 다음과 [같이에서 C# 선언할](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83)수 있습니다.

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

위의에서 Java `int[]` 매개 변수를 [JavaArray&lt;int&gt;](xref:Android.Runtime.JavaArray`1)에 매핑합니다.
반드시 필요한 것은 C# 아닙니다. `int[]`또는 `IList<int>`에 바인딩하거나 완전히 바인딩할 수도 있습니다. 어떤 형식을 선택 하든지 `Invoker` 호출을 위해 Java `int[]` 형식으로 변환할 수 있어야 합니다.

### <a name="invoker-definition"></a>호출자 정의

`Invoker` 형식 정의는 `Java.Lang.Object`를 상속 하 고, 적절 한 인터페이스를 구현 하 고, 인터페이스 정의에서 참조 되는 모든 연결 메서드를 제공 해야 합니다. 클래스 바인딩과 다른 하나 이상의 제안이 있습니다. `class_ref` 필드와 메서드 Id는 정적 멤버가 아닌 인스턴스 멤버 여야 합니다.

인스턴스 멤버를 사용 하는 이유는 Android 런타임에서 `JNIEnv.GetMethodID` 동작과 동일 해야 합니다. 이 동작은 Java 동작 일 수도 있고 테스트 되지 않았습니다. 선언 된 인터페이스가 아니라 구현 된 인터페이스에서 제공 되는 메서드를 조회할 때 `JNIEnv.GetMethodID` null을 반환 합니다. SortedMap [&lt;k, v&gt;](https://developer.android.com/reference/java/util/Map.html) 인터페이스를 구현 하는 [&lt;k, v&gt;](https://developer.android.com/reference/java/util/SortedMap.html) java 인터페이스를 살펴보겠습니다. Map은 [clear](https://developer.android.com/reference/java/util/Map.html#clear()) 메서드를 제공 하므로 SortedMap에 대 한 적절 한 `Invoker` 정의는 다음과 같습니다.

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

`JNIEnv.GetMethodID`는 `SortedMap` 클래스 인스턴스를 통해 `Map.clear` 메서드를 조회할 때 `null`를 반환 하므로 위의 내용이 실패 합니다.

이에 대 한 두 가지 솔루션이 있습니다. 모든 메서드가 제공 하는 인터페이스를 추적 하거나, 각 인터페이스에 대 한 `class_ref`를 포함 하거나, 모든 것을 인스턴스 멤버로 유지 하 고 인터페이스 형식이 아닌 가장 많이 파생 된 클래스 형식에 대해 메서드 조회를 수행 합니다. 후자는 **Mono**에서 수행 됩니다.

호출자 정의에는 생성자, `Dispose` 메서드, `ThresholdType` 및 `ThresholdClass` 멤버, `GetObject` 메서드, 인터페이스 메서드 구현 및 커넥터 메서드 구현의 여섯 개 섹션이 있습니다.

#### <a name="constructor"></a>생성자

생성자는 호출 중인 인스턴스의 런타임 클래스를 조회 하 고 인스턴스 `class_ref` 필드에 런타임 클래스를 저장 해야 합니다.

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

참고: `Handle` 속성은 `handle` 매개 변수가 아니라 생성자 본문 내에서 사용 해야 합니다. `handle` 매개 변수는 기본 생성자가 실행을 완료 한 후에는 유효 하지 않을 수 있습니다.

#### <a name="dispose-method"></a>Dispose 메서드

`Dispose` 메서드는 생성자에 할당 된 전역 참조를 해제 해야 합니다.

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

`ThresholdType` 및 `ThresholdClass` 멤버는 클래스 바인딩에서 발견 된 것과 동일 합니다.

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

[JavaCast&lt;t&gt;()](xref:Android.Runtime.Extensions.JavaCast*)를 지원 하려면 정적 `GetObject` 메서드가 필요 합니다.

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```

#### <a name="interface-methods"></a>인터페이스 메서드

인터페이스의 모든 메서드에는 JNI을 통해 해당 Java 메서드를 호출 하는 구현이 있어야 합니다.

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

커넥터 메서드 및 지원 인프라는 JNI 매개 변수를 적절 한 C# 형식으로 마샬링하는 일을 담당 합니다. Java `int[]` 매개 변수는 내의 C#`IntPtr` JNI `jintArray`으로 전달 됩니다. C# 인터페이스 호출을 지원 하기 위해 `IntPtr`를 `JavaArray<int>` 마샬링해야 합니다.

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

`JavaList<int>`보다 `int[]`를 선호 하는 경우 [GetArray ()](xref:Android.Runtime.JNIEnv.GetArray*) 를 대신 사용할 수 있습니다.

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

그러나이 `JNIEnv.GetArray` Vm 사이에 전체 배열을 복사 하므로, 크기가 크면 많은 GC가 추가 될 수 있습니다.

### <a name="complete-invoker-definition"></a>호출자 정의 완료

[전체 IAdderProgressInvoker 정의](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88)는 다음과 같습니다.

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

많은 JNIEnv 메서드는 `GCHandle`s와 유사한 *JNI* *개체 참조*를 반환 합니다. JNI는 로컬 참조, 전역 참조 및 약한 전역 참조의 세 가지 개체 참조 유형을 제공 합니다. 세 가지 모두 `System.IntPtr`로 표시 *되지만* (JNI 함수 형식 섹션에 따라) `JNIEnv` 메서드에서 반환 되는 일부 `IntPtr`는 참조가 아닙니다. 예를 들어 [JNIEnv](xref:Android.Runtime.JNIEnv.GetMethodID*) 는 `IntPtr`를 반환 하지만 개체 참조를 반환 하지 않는 `jmethodID`을 반환 합니다. 자세한 내용은 [JNI 함수 설명서](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html) 를 참조 하세요.

로컬 참조는 *대부분의* 참조 만들기 메서드에서 생성 됩니다.
Android에서는 지정 된 시간 (일반적으로 512)에 제한 된 수의 로컬 참조만 존재할 수 있습니다. JNIEnv를 통해 로컬 참조를 삭제할 수 있습니다 [.](xref:Android.Runtime.JNIEnv.DeleteLocalRef*)
JNI와 달리 개체 참조를 반환 하는 모든 reference JNIEnv 메서드가 로컬 참조를 반환 하는 것은 아닙니다. [JNIEnv 클래스](xref:Android.Runtime.JNIEnv.FindClass*) 는 *전역* 참조를 반환 합니다. 가능한 한 빨리 로컬 참조를 삭제 하는 것이 [좋습니다. 개체 주위에](xref:Java.Lang.Object) JniHandleOwnership [(IntPtr 핸들, transfer)](xref:Java.Lang.Object#ctor*) 생성자에 `JniHandleOwnership.TransferLocalRef`를 지정 하 고 개체를 구성 하는 것입니다.

전역 참조는 [JNIEnv](xref:Android.Runtime.JNIEnv.NewGlobalRef*) 및 [JNIEnv 클래스](xref:Android.Runtime.JNIEnv.FindClass*)에 의해 생성 됩니다.
JNIEnv를 사용 하 여 제거할 수 있습니다 [.](xref:Android.Runtime.JNIEnv.DeleteGlobalRef*)
에뮬레이터에는 대기 중인 전역 참조는 2000 개로 제한 되는 반면 하드웨어 장치에는 52000 전역 참조의 제한이 있습니다.

약한 전역 참조는 Android v 2.2 (이전 버전) 이상 에서만 사용할 수 있습니다. [JNIEnv. DeleteWeakGlobalRef](xref:Android.Runtime.JNIEnv.DeleteWeakGlobalRef*)를 사용 하 여 약한 전역 참조를 삭제할 수 있습니다.

### <a name="dealing-with-jni-local-references"></a>JNI 로컬 참조 처리

[JNIEnv GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*), [JNIEnv](xref:Android.Runtime.JNIEnv.GetStaticObjectField*), CallObjectMethod, JNIEnv, JNIEnv, JNI [,](xref:Android.Runtime.JNIEnv.CallObjectMethod*),,, [및](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) 는 java 개체에 대 한 로컬 참조를 포함 하는 `IntPtr`를 반환 하거나, java가 `null`를 반환 하는 경우 `IntPtr.Zero`을 반환 [합니다.](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) 한 번에 처리 될 수 있는 제한 된 수의 로컬 참조 (512 개 항목)로 인해 참조가 적시에 삭제 되도록 하는 것이 좋습니다. 로컬 참조는 다음과 같은 세 가지 방법으로 처리할 수 있습니다. 명시적으로 삭제 하 고,이를 포함 하는 `Java.Lang.Object` 인스턴스를 만들고 `Java.Lang.Object.GetObject<T>()`를 사용 하 여 그 주위에 관리 되는 호출 가능 래퍼를 만듭니다.

### <a name="explicitly-deleting-local-references"></a>명시적으로 로컬 참조 삭제

[JNIEnv](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) 는 로컬 참조를 삭제 하는 데 사용 됩니다. 로컬 참조를 삭제 한 후에는 더 이상 사용할 수 없습니다. 따라서 로컬 참조를 사용 하 여 마지막으로 수행 된 작업 인지 `JNIEnv.DeleteLocalRef` 확인 해야 합니다.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```

### <a name="wrapping-with-javalangobject"></a>Java. Lang 개체와 함께 래핑

`Java.Lang.Object`는 종료 JNI 참조를 래핑하는 데 사용할 수 있는 [(IntPtr 핸들, JniHandleOwnership transfer)](xref:Java.Lang.Object#ctor*) 생성자를 제공 합니다. [JniHandleOwnership](xref:Android.Runtime.JniHandleOwnership) 매개 변수는 `IntPtr` 매개 변수를 처리 하는 방법을 결정 합니다.

- [JniHandleOwnership. DoNotTransfer](xref:Android.Runtime.JniHandleOwnership.DoNotTransfer) &ndash; 생성 된 `Java.Lang.Object` 인스턴스는 `handle` 매개 변수에서 새 전역 참조를 만들며 `handle`는 변경 되지 않습니다.
    호출자는 필요한 경우 `handle`를 해제 해야 합니다.

- [JniHandleOwnership](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) 는 생성 된 `Java.Lang.Object` 인스턴스를 &ndash; `handle` 매개 변수에서 새 전역 참조를 만들며 `handle`는 JNIEnv를 사용 하 여 삭제 됩니다 [.](xref:Android.Runtime.JNIEnv.DeleteLocalRef*) 호출자는 `handle` 해제 해서는 안 되며 생성자가 실행을 완료 한 후에는 `handle`를 사용 하지 않아야 합니다.

- [JniHandleOwnership Globalref](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; 만든 `Java.Lang.Object` 인스턴스는 `handle` 매개 변수의 소유권을 갖습니다. 호출자가 `handle`를 해제 해서는 안 됩니다.

JNI 메서드 호출 메서드는 로컬 refs를 반환 하므로 일반적으로 `JniHandleOwnership.TransferLocalRef` 사용 됩니다.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

`Java.Lang.Object` 인스턴스가 가비지 수집 될 때까지 생성 된 전역 참조는 해제 되지 않습니다. 가능 하면 인스턴스를 삭제 하면 전역 참조가 해제 되 고 가비지 수집 속도가 빨라집니다.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```

### <a name="using-javalangobjectgetobjectlttgt"></a>Java.&lt;T&gt;() 사용

`Java.Lang.Object`는 지정 된 형식의 관리 되는 호출 가능 래퍼를 만드는 데 사용할 수 있는 [JniHandleOwnership&lt;t&gt;(IntPtr 핸들, transfer)](xref:Java.Lang.Object.GetObject*) 메서드를 제공 합니다.

유형 `T`는 다음 요구 사항을 충족 해야 합니다.

1. `T`은 참조 형식 이어야 합니다.

1. `T`는 `IJavaObject` 인터페이스를 구현해야 합니다.

1. `T` 추상 클래스 또는 인터페이스가 아니면 `T`는 매개 변수 형식이 `(IntPtr,
    JniHandleOwnership)` 인 생성자를 제공 해야 합니다.

1. 추상 클래스 또는 인터페이스 `T` 경우 `T`에 사용할 수 있는 *호출자* 가 *있어야 합니다* . 호출자는 `T`를 상속 하거나 `T`를 구현 하며 호출자 접미사가 있는 `T`와 동일한 이름을 가진 비추상 형식입니다. 예를 들어 T가 인터페이스 `Java.Lang.IRunnable` 경우 `Java.Lang.IRunnableInvoker` 형식이 있어야 하 고 필수 `(IntPtr,
    JniHandleOwnership)` 생성자를 포함 해야 합니다.

JNI 메서드 호출 메서드는 로컬 refs를 반환 하므로 일반적으로 `JniHandleOwnership.TransferLocalRef` 사용 됩니다.

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Java 형식 조회

JNI에서 필드 또는 메서드를 조회 하려면 필드 또는 메서드에 대 한 선언 형식을 먼저 조회 해야 합니다. [JNIEnv (string)](xref:Android.Runtime.JNIEnv.FindClass*)메서드는 Java 형식을 조회 하는 데 사용 됩니다. String 매개 변수는 *단순화 된 형식 참조* 또는 Java 형식에 대 한 *전체 형식 참조* 입니다. 단순화 및 전체 형식 참조에 대 한 자세한 내용은 [JNI 형식 참조 섹션](#_JNI_Type_References) 을 참조 하세요.

참고: 개체 인스턴스를 반환 하는 다른 모든 `JNIEnv` 메서드와 달리 `FindClass`는 지역 참조가 아닌 전역 참조를 반환 합니다.

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>인스턴스 필드

필드는 *필드 id*를 통해 조작 됩니다. 필드 Id는 필드가 정의 된 클래스, 필드 이름 및 필드의 [JNI 형식 시그니처](#JNI_Type_Signatures) 를 필요로 하는 JNIEnv를 통해 가져옵니다 [.](xref:Android.Runtime.JNIEnv.GetFieldID*)

필드 Id는 해제할 필요가 없으며 해당 Java 유형이 로드 되는 한 유효 합니다. Android는 현재 클래스 언로드를 지원 하지 않습니다.

인스턴스 필드를 조작 하는 방법에는 인스턴스 필드를 읽는 방법과 인스턴스 필드를 작성 하기 위한 두 가지 메서드가 있습니다. 모든 메서드 집합에 필드 값을 읽거나 쓰려면 필드 ID가 필요 합니다.

### <a name="reading-instance-field-values"></a>인스턴스 필드 값 읽기

인스턴스 필드 값을 읽기 위한 메서드 집합은 명명 패턴을 따릅니다.

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```

여기서 `*`은 필드의 유형입니다.

- [JNIEnv GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*) &ndash; `java.lang.Object`, 배열 및 인터페이스 형식과 같은 기본 제공 형식이 아닌 모든 인스턴스 필드의 값을 읽습니다. 반환 되는 값은 JNI 로컬 참조입니다.

- [JNIEnv GetBooleanField](xref:Android.Runtime.JNIEnv.GetBooleanField*) &ndash; `bool` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv GetByteField](xref:Android.Runtime.JNIEnv.GetByteField*) &ndash; `sbyte` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv 필드](xref:Android.Runtime.JNIEnv.GetCharField*) &ndash; `char` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv GetShortField](xref:Android.Runtime.JNIEnv.GetShortField*) &ndash; `short` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.GetIntField*) &ndash; `int` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv 필드](xref:Android.Runtime.JNIEnv.GetLongField*) &ndash; `long` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv GetFloatField](xref:Android.Runtime.JNIEnv.GetFloatField*) &ndash; `float` 인스턴스 필드의 값을 읽습니다.

- [JNIEnv GetDoubleField](xref:Android.Runtime.JNIEnv.GetDoubleField*) &ndash; `double` 인스턴스 필드의 값을 읽습니다.

### <a name="writing-instance-field-values"></a>인스턴스 필드 값 쓰기

인스턴스 필드 값을 쓰기 위한 메서드 집합은 명명 패턴을 따릅니다.

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

여기서 *type* 은 필드의 형식입니다.

- [JNIEnv SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `java.lang.Object`, 배열 및 인터페이스 형식과 같은 기본 제공 형식이 아닌 모든 필드의 값을 작성 합니다. `IntPtr` 값은 JNI local reference, JNI global reference, JNI weak global reference 또는 `IntPtr.Zero` (`null`) 일 수 있습니다.

- [JNIEnv SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `bool` 인스턴스 필드의 값을 작성 합니다.

- [JNIEnv SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `sbyte` 인스턴스 필드의 값을 작성 합니다.

- [JNIEnv SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `char` 인스턴스 필드의 값을 작성 합니다.

- [JNIEnv SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `short` 인스턴스 필드의 값을 작성 합니다.

- [JNIEnv SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `int` 인스턴스 필드의 값을 작성 합니다.

- [JNIEnv SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `long` 인스턴스 필드의 값을 작성 합니다.

- [JNIEnv SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `float` 인스턴스 필드의 값을 작성 합니다.

- [JNIEnv SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; `double` 인스턴스 필드의 값을 작성 합니다.

<a name="_Static_Fields" />

## <a name="static-fields"></a>정적 필드

정적 필드는 *필드 id*를 통해 조작 됩니다. 필드 Id는 필드가 정의 된 클래스, 필드 이름 및 필드의 [JNI 형식 시그니처](#JNI_Type_Signatures) 를 필요로 하는 JNIEnv를 통해 가져옵니다 [.](xref:Android.Runtime.JNIEnv.GetStaticFieldID*)

필드 Id는 해제할 필요가 없으며 해당 Java 유형이 로드 되는 한 유효 합니다. Android는 현재 클래스 언로드를 지원 하지 않습니다.

정적 필드를 조작 하는 방법에는 인스턴스 필드를 읽는 방법과 인스턴스 필드를 쓰는 데 사용할 수 있는 두 가지 메서드가 있습니다. 모든 메서드 집합에 필드 값을 읽거나 쓰려면 필드 ID가 필요 합니다.

### <a name="reading-static-field-values"></a>정적 필드 값 읽기

정적 필드 값을 읽기 위한 메서드 집합은 명명 패턴을 따릅니다.

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

여기서 `*`은 필드의 유형입니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) &ndash; `java.lang.Object`, 배열 및 인터페이스 형식과 같이 기본 형식이 아닌 정적 필드의 값을 읽습니다. 반환 되는 값은 JNI 로컬 참조입니다.

- [JNIEnv &ndash; GetStaticBooleanField](xref:Android.Runtime.JNIEnv.GetStaticBooleanField*) `bool` 정적 필드의 값을 읽습니다.

- [JNIEnv &ndash; GetStaticByteField](xref:Android.Runtime.JNIEnv.GetStaticByteField*) `sbyte` 정적 필드의 값을 읽습니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.GetStaticCharField*) &ndash; `char` 정적 필드의 값을 읽습니다.

- [JNIEnv &ndash; GetStaticShortField](xref:Android.Runtime.JNIEnv.GetStaticShortField*) `short` 정적 필드의 값을 읽습니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.GetStaticLongField*) `long` 정적 필드의 값 &ndash; 읽습니다.

- [JNIEnv &ndash; GetStaticFloatField](xref:Android.Runtime.JNIEnv.GetStaticFloatField*) `float` 정적 필드의 값을 읽습니다.

- [JNIEnv &ndash; GetStaticDoubleField](xref:Android.Runtime.JNIEnv.GetStaticDoubleField*) `double` 정적 필드의 값을 읽습니다.

### <a name="writing-static-field-values"></a>정적 필드 값 쓰기

정적 필드 값을 쓰기 위한 메서드 집합은 명명 패턴을 따릅니다.

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

여기서 *type* 은 필드의 형식입니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `java.lang.Object`, 배열 및 인터페이스 형식과 같이 기본 형식이 아닌 정적 필드의 값을 작성 합니다. `IntPtr` 값은 JNI local reference, JNI global reference, JNI weak global reference 또는 `IntPtr.Zero` (`null`) 일 수 있습니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `bool` 정적 필드의 값을 작성 합니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `sbyte` 정적 필드의 값을 작성 합니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `char` 정적 필드의 값을 작성 합니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `short` 정적 필드의 값을 작성 합니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `int` 정적 필드의 값을 작성 합니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `long` 정적 필드의 값을 작성 합니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `float` 정적 필드의 값을 작성 합니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; `double` 정적 필드의 값을 작성 합니다.

<a name="_Instance_Methods" />

## <a name="instance-methods"></a>인스턴스 메서드

인스턴스 메서드는 *메서드 id*를 통해 호출 됩니다. 메서드 Id는 메서드가 정의 된 형식, 메서드의 이름 및 메서드의 [JNI 형식 시그니처와](#JNI_Type_Signatures) [JNIEnv. GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*)를 통해 가져옵니다.

메서드 Id는 해제할 필요가 없으며 해당 하는 Java 형식이 로드 되는 동안 유효 합니다. Android는 현재 클래스 언로드를 지원 하지 않습니다.

메서드를 호출 하는 방법에는 메서드를 호출 하는 두 가지 메서드가 있습니다. 하나는 실제로 메서드를 호출 하 고, 다른 하나는 비가상 메서드를 호출 하는 두 메서드 집합 모두 메서드를 호출 하려면 메서드 ID가 필요 하며, 비가상 호출에는 호출할 클래스 구현을 지정 해야 합니다.

인터페이스 메서드는 선언 형식 내 에서만 조회할 수 있습니다. 확장/상속 된 인터페이스에서 제공 되는 메서드는 조회할 수 없습니다. 자세한 내용은 나중에 인터페이스/호출자 구현 구현 섹션을 참조 하세요.

클래스 또는 기본 클래스 또는 구현 된 인터페이스에 선언 된 모든 메서드를 조회할 수 있습니다.

### <a name="virtual-method-invocation"></a>가상 메서드 호출

메서드를 호출 하는 메서드 집합은 명명 패턴을 따릅니다.

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

여기서 `*`은 메서드의 반환 형식입니다.

- [JNIEnv CallObjectMethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*) &ndash; `java.lang.Object`, 배열 및 인터페이스와 같이 builtin이 아닌 형식을 반환 하는 메서드를 호출 합니다. 반환 되는 값은 JNI 로컬 참조입니다.

- [JNIEnv CallBooleanMethod](xref:Android.Runtime.JNIEnv.CallBooleanMethod*) &ndash; `bool` 값을 반환 하는 메서드를 호출 합니다.

- [JNIEnv CallByteMethod](xref:Android.Runtime.JNIEnv.CallByteMethod*) &ndash; `sbyte` 값을 반환 하는 메서드를 호출 합니다.

- [JNIEnv 메서드](xref:Android.Runtime.JNIEnv.CallCharMethod*) &ndash; `char` 값을 반환 하는 메서드를 호출 합니다.

- [JNIEnv CallShortMethod](xref:Android.Runtime.JNIEnv.CallShortMethod*) &ndash; `short` 값을 반환 하는 메서드를 호출 합니다.

- [JNIEnv 메서드](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; `long` 값을 반환 하는 메서드를 호출 합니다.

- [JNIEnv CallFloatMethod](xref:Android.Runtime.JNIEnv.CallFloatMethod*) &ndash; `float` 값을 반환 하는 메서드를 호출 합니다.

- [JNIEnv CallDoubleMethod](xref:Android.Runtime.JNIEnv.CallDoubleMethod*) &ndash; `double` 값을 반환 하는 메서드를 호출 합니다.

### <a name="non-virtual-method-invocation"></a>비가상 메서드 호출

비가상 메서드를 호출 하는 메서드 집합은 명명 패턴을 따릅니다.

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

여기서 `*`은 메서드의 반환 형식입니다. 비가상 메서드 호출은 일반적으로 가상 메서드의 기본 메서드를 호출 하는 데 사용 됩니다.

- JNIEnv `java.lang.Object`, 배열 및 인터페이스와 같이 builtin이 아닌 형식을 반환 하는 메서드를 비가상 [objectmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) &ndash; 합니다. 반환 되는 값은 JNI 로컬 참조입니다.

- [JNIEnv CallNonvirtualBooleanMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod*) &ndash;는 `bool` 값을 반환 하는 메서드를 거의 호출 하지 않습니다.

- [JNIEnv CallNonvirtualByteMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualByteMethod*) &ndash;는 `sbyte` 값을 반환 하는 메서드를 거의 호출 하지 않습니다.

- JNIEnv는 `char` 값을 반환 하는 메서드를 비 비가상 [charmethod](xref:Android.Runtime.JNIEnv.CallNonvirtualCharMethod*) &ndash; 호출 합니다.

- [JNIEnv CallNonvirtualShortMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualShortMethod*) &ndash;는 `short` 값을 반환 하는 메서드를 거의 호출 하지 않습니다.

- [JNIEnv CallNonvirtualLongMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualLongMethod*) &ndash;는 `long` 값을 반환 하는 메서드를 거의 호출 하지 않습니다.

- [JNIEnv CallNonvirtualFloatMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualFloatMethod*) &ndash;는 `float` 값을 반환 하는 메서드를 거의 호출 하지 않습니다.

- [JNIEnv CallNonvirtualDoubleMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod*) &ndash;는 `double` 값을 반환 하는 메서드를 거의 호출 하지 않습니다.

<a name="_Static_Methods" />

## <a name="static-methods"></a>정적 메서드

정적 메서드는 *메서드 id*를 통해 호출 됩니다. 메서드 Id는 메서드가 정의 된 형식, 메서드의 이름 및 메서드의 [JNI 형식 시그니처와](#JNI_Type_Signatures) [JNIEnv. GetStaticMethodID](xref:Android.Runtime.JNIEnv.GetStaticMethodID*)를 통해 가져옵니다.

메서드 Id는 해제할 필요가 없으며 해당 하는 Java 형식이 로드 되는 동안 유효 합니다. Android는 현재 클래스 언로드를 지원 하지 않습니다.

### <a name="static-method-invocation"></a>정적 메서드 호출

메서드를 호출 하는 메서드 집합은 명명 패턴을 따릅니다.

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

여기서 `*`은 메서드의 반환 형식입니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) 는 `java.lang.Object`, 배열 및 인터페이스와 같이 builtin이 아닌 형식을 반환 하는 정적 메서드를 호출 &ndash; 합니다. 반환 되는 값은 JNI 로컬 참조입니다.

- [JNIEnv CallStaticBooleanMethod](xref:Android.Runtime.JNIEnv.CallStaticBooleanMethod*) &ndash; `bool` 값을 반환 하는 정적 메서드를 호출 합니다.

- [JNIEnv CallStaticByteMethod](xref:Android.Runtime.JNIEnv.CallStaticByteMethod*) &ndash; `sbyte` 값을 반환 하는 정적 메서드를 호출 합니다.

- [JNIEnv](xref:Android.Runtime.JNIEnv.CallStaticCharMethod*) 는 `char` 값을 반환 하는 정적 메서드를 호출 &ndash; 합니다.

- [JNIEnv CallStaticShortMethod](xref:Android.Runtime.JNIEnv.CallStaticShortMethod*) &ndash; `short` 값을 반환 하는 정적 메서드를 호출 합니다.

- [JNIEnv 메서드](xref:Android.Runtime.JNIEnv.CallLongMethod*) 는 `long` 값을 반환 하는 정적 메서드를 호출 &ndash; 합니다.

- [JNIEnv CallStaticFloatMethod](xref:Android.Runtime.JNIEnv.CallStaticFloatMethod*) &ndash; `float` 값을 반환 하는 정적 메서드를 호출 합니다.

- [JNIEnv CallStaticDoubleMethod](xref:Android.Runtime.JNIEnv.CallStaticDoubleMethod*) &ndash; `double` 값을 반환 하는 정적 메서드를 호출 합니다.

<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>JNI 형식 서명

[JNI 형식 시그니처](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432) 는 메서드를 제외 하 고 [JNI 형식 참조](#_JNI_Type_References) (단순 형식 참조는 아니지만)입니다. 메서드를 사용 하는 경우 JNI 형식 시그니처는 `'('`여는 괄호와 함께 연결 된 모든 매개 변수 형식에 대 한 형식 참조, 닫는 괄호 `')'`, 메서드 반환 형식의 JNI 형식 참조가 차례로 나옵니다.

예를 들어 Java 메서드는 다음과 같습니다.

```java
long f(int n, String s, int[] array);
```

JNI 형식 시그니처는 다음과 같습니다.

```csharp
(ILjava/lang/String;[I)J
```

일반적으로 `javap` 명령을 사용 하 여 JNI 서명을 확인 하 *는 것이 좋습니다.* 예를 들어 [valueOf (String)](https://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) 메서드의 JNI 형식 시그니처는 "(ljava/Lang/String;) ljava/Lang/Thread $ state;" 이지만, [java.](https://developer.android.com/reference/java/lang/Thread.State.html#values) 메서드의 JNI 형식 시그니처는 "() [Ljava/Lang/Thread $ state;"입니다. 후행 세미콜론을 감시 합니다. 이러한 형식은 JNI 형식 시그니처의 *일부입니다.*

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>JNI 형식 참조

JNI 형식 참조는 Java 형식 참조와 다릅니다. JNI와 `java.lang.String` 같이 정규화 된 Java 형식 이름을 사용할 수 없습니다. 컨텍스트에 따라 JNI 변형 `"java/lang/String"` 또는 `"Ljava/lang/String;"`를 대신 사용 해야 합니다. 자세한 내용은 아래를 참조 하세요.
JNI 형식 참조에는 다음 네 가지 유형이 있습니다.

- **기본 제공**
- **시킨**
- **type**
- **array**

### <a name="built-in-type-references"></a>기본 제공 형식 참조

기본 제공 형식 참조는 기본 제공 값 형식을 참조 하는 데 사용 되는 단일 문자입니다. 매핑은 다음과 같습니다.

- `sbyte`에 대 한 `"B"`입니다.
- `short`에 대 한 `"S"`입니다.
- `int`에 대 한 `"I"`입니다.
- `long`에 대 한 `"J"`입니다.
- `float`에 대 한 `"F"`입니다.
- `double`에 대 한 `"D"`입니다.
- `char`에 대 한 `"C"`입니다.
- `bool`에 대 한 `"Z"`입니다.
- `void` 메서드 반환 형식에 대 한 `"V"`입니다.

<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>단순 형식 참조

단순 형식 참조는 [JNIEnv (string)](xref:Android.Runtime.JNIEnv.FindClass*)에서만 사용할 수 있습니다.
단순 형식 참조를 파생 하는 방법에는 두 가지가 있습니다.

1. 정규화 된 Java 이름에서 패키지 이름에 있는 모든 `'.'` 및 형식 이름 앞에 `'/'`를 사용 하 고 형식 이름 내의 모든 `'.'`를 `'$'`으로 바꿉니다.

1. `'unzip -l android.jar | grep JavaName'`의 출력을 읽습니다.

둘 중 하나에 해당 하는 경우 [java 형식 참조](https://developer.android.com/reference/java/lang/Thread.State.html) 를 단순화 된 형식 참조 `java/lang/Thread$State`매핑됩니다.

### <a name="type-references"></a>형식 참조

형식 참조는 기본 제공 형식 참조 또는 `'L'` 접두사와 `';'` 접미사를 사용 하는 단순 형식 참조입니다. [Java 형식의 경우](https://developer.android.com/reference/java/lang/String.html)에는 단순 형식 참조가 `"java/lang/String"`되는 반면 형식 참조는 `"Ljava/lang/String;"`.

형식 참조는 배열 형식 참조 및 JNI 시그니처와 함께 사용 됩니다.

형식 참조를 얻는 또 다른 방법은 `'javap -s -classpath android.jar fully.qualified.Java.Name'`의 출력을 읽는 것입니다.
관련 된 형식에 따라 생성자 선언 또는 메서드 반환 형식을 사용 하 여 JNI 이름을 확인할 수 있습니다. 예를 들면 다음과 같습니다.

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

`Thread.State`은 Java 열거형 형식 이므로 `valueOf` 메서드의 시그니처를 사용 하 여 형식 참조가 Ljava/lang/Thread $ State; 인지 확인할 수 있습니다.

### <a name="array-type-references"></a>배열 형식 참조

배열 형식 참조는 `'['` JNI 형식 참조에 접두사로 추가 됩니다.
배열을 지정 하는 경우에는 단순 형식 참조를 사용할 수 없습니다.

예를 들어 `int[]` `"[I"`, `int[][]` `"[[I"`, `java.lang.Object[]`은 `"[Ljava/lang/Object;"`입니다.

## <a name="java-generics-and-type-erasure"></a>Java 제네릭 및 형식 지우기

*대부분* 의 경우 JNI를 통해 표시 되는 Java generics는 *존재 하지 않습니다*.
일부 "주름"이 있지만 이러한 주름은 JNI 조회 하 여 제네릭 멤버를 호출 하는 방법이 아니라 Java가 제네릭과 상호 작용 하는 방법에 있습니다.

JNI을 통해 상호 작용 하는 경우 제네릭 형식 또는 멤버와 제네릭이 아닌 형식 또는 멤버 간에 차이가 없습니다. 예를 들어, 제네릭 형식 [&lt;t&gt;](https://developer.android.com/reference/java/lang/Class.html) 는 "raw" 제네릭 형식 `java.lang.Class`이며 둘 다 동일한 단순 형식 참조, `"java/lang/Class"`를 가집니다.

## <a name="java-native-interface-support"></a>Java Native Interface 지원

[JNIEnv](xref:Android.Runtime.JNIEnv) 는 Jave Native INTERFACE (JNI)에 대 한 관리 되는 래퍼입니다. JNI 함수는 명시적 `JNIEnv*` 매개 변수를 제거 하기 위해 변경 되었지만 `jobject`, `jclass`, `jmethodID`등 `IntPtr` 사용 되는 경우에는 [Java Native Interface 사양](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)내에서 선언 됩니다. 예를 들어 [JNI NewObject 함수](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517)를 살펴보겠습니다.

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

이는 [JNIEnv 개체](xref:Android.Runtime.JNIEnv.NewObject*) 메서드로 노출 됩니다.

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

두 호출 사이를 변환 하는 것은 매우 간단 합니다. C에서는 다음을 수행 합니다.

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

이 C# 에 해당 하는 항목은 다음과 같습니다.

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

IntPtr에 Java 개체 인스턴스가 있는 경우이를 사용 하 여 작업을 수행 하는 것이 좋습니다. [JNIEnv. CallVoidMethod ()](xref:Android.Runtime.JNIEnv.CallVoidMethod*) 와 같은 JNIEnv 메서드를 사용 하 여이 작업을 수행할 수 있지만, 이미 아날로그 C# 래퍼가 있는 경우 JNI 참조에 대해 래퍼를 생성 합니다. [JavaCast\<t >](xref:Android.Runtime.Extensions.JavaCast*) 확장 메서드를 통해이 작업을 수행할 수 있습니다.

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

또한 [\<t >](xref:Java.Lang.Object.GetObject*) 메서드를 사용할 수 있습니다.

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

또한 모든 JNI 함수에 있는 `JNIEnv*` 매개 변수를 제거 하 여 모든 JNI 함수를 수정 했습니다.

## <a name="summary"></a>요약

JNI를 직접 처리 하는 것은 모든 비용에서 피해 야 하는 끔찍한 경험입니다. 불행 하지만 항상 갖추고는 아닙니다. Android 용 Mono를 사용 하는 바인딩되지 않은 Java 사례에 도달 하면이 가이드에서 몇 가지 도움을 얻을 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Java Native Interface 사양](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java Native Interface 함수](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
