---
title: "JNI 작업"
description: "Xamarin.Android는 Java 대신 C# 내에서 Android 응용 프로그램 작성을 허용 합니다. 여러 어셈블리를 Mono.Android.dll 및 Mono.Android.GoogleMaps.dll를 비롯 한 Java 라이브러리에 대 한 바인딩을 제공 하는 Xamarin.Android 제공 됩니다. 그러나 모든 가능한 Java 라이브러리용 바인딩을 제공 되지 않습니다 있으며 모든 Java 형식 및 멤버 바인딩에 제공 되는 바인딩할 수 있습니다. 바인딩되지 않은 Java 형식 및 멤버를 사용 하는 Java 기본 인터페이스 (JNI)를 사용할 수 있습니다. 이 문서에는 Java 형식 및 Xamarin.Android 응용 프로그램에서 멤버와 상호 작용할 JNI를 사용 하는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: f14d456cba66142c51e0755cdfd3c6795bd1cf73
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="working-with-jni"></a>JNI 작업

_Xamarin.Android는 Java 대신 C# 내에서 Android 응용 프로그램 작성을 허용 합니다. 여러 어셈블리를 Mono.Android.dll 및 Mono.Android.GoogleMaps.dll를 비롯 한 Java 라이브러리에 대 한 바인딩을 제공 하는 Xamarin.Android 제공 됩니다. 그러나 모든 가능한 Java 라이브러리용 바인딩을 제공 되지 않습니다 있으며 모든 Java 형식 및 멤버 바인딩에 제공 되는 바인딩할 수 있습니다. 바인딩되지 않은 Java 형식 및 멤버를 사용 하는 Java 기본 인터페이스 (JNI)를 사용할 수 있습니다. 이 문서에는 Java 형식 및 Xamarin.Android 응용 프로그램에서 멤버와 상호 작용할 JNI를 사용 하는 방법을 보여 줍니다._


## <a name="overview"></a>개요

항상 것은 아닙니다 필요 하거나는 관리 되는 호출 가능 래퍼 MCW () 호출 Java 코드를 만들 수 있습니다. 대부분의 경우에서 "인라인" JNI은 완벽 하 게 가능 하 고 일회용 언바운드 Java 멤버 사용 하는 데 유용 합니다. JNI 전체.jar 바인딩을 생성 하는 Java 클래스에서 단일 메서드를 호출 하는 경우가 많습니다.

Xamarin.Android 제공는 `Mono.Android.dll` Android의에 대 한 바인딩을 제공 하는 어셈블리를 `android.jar` 라이브러리입니다. 형식 및 멤버를 표시 하지 내 `Mono.Android.dll` 내 없는 형식 및 `android.jar` 수동으로 바인딩할 사용 될 수 있습니다. 사용 Java 형식 및 멤버를 바인딩하려면는 **Java 기본 인터페이스** (**JNI**)를 조회할 형식, 필드, 선택한 메서드를 호출 합니다.

Xamarin.Android에 JNI API는 개념적으로 매우 유사한는 `System.Reflection` .net에서 API:는 형식을 검색 하 고 메서드 및 기타 멤버 이름으로 한 읽기 및 쓰기 필드 값을 호출 수 있습니다. JNI를 사용할 수 있습니다 및 `Android.Runtime.RegisterAttribute` 재정의 지원 하기 위해 바인딩할 수 있는 가상 메서드를 선언 하는 사용자 지정 특성입니다. C#에서 구현 될 수 있도록 인터페이스를 바인딩할 수 있습니다.

이 문서에 설명합니다.

-  어떻게 JNI 형식을 참조 합니다.
-  조회, 읽기 및 쓰기 필드 하는 방법.
-  조회 하 고 메서드를 호출 하는 방법.
-  관리 코드에서 재정의 허용 하도록 가상 메서드를 노출 하는 방법.
-  인터페이스를 노출 하는 방법.



## <a name="requirements"></a>요구 사항

JNI를 통해 노출 되는 [Android.Runtime.JNIEnv 네임 스페이스](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/), Xamarin.Android의 모든 버전에서 사용할 수 있습니다.
Java 형식 및 인터페이스를 바인딩하려면 Xamarin.Android 4.0 이상을 사용 해야 합니다.


## <a name="managed-callable-wrappers"></a>관리 되는 호출 가능 래퍼

A **호출 가능 래퍼 관리** (**MCW**)은 한 *바인딩* Java 클래스 또는 인터페이스를 C# 클라이언트 코드에 걱정할 필요 하지 않은 모든 JNI 기계를 래핑하는 JNI의 내재 된 복잡성입니다. 대부분의 `Mono.Android.dll` 관리 되는 호출 가능 래퍼 구성 됩니다.

호출 가능 래퍼는 관리 되는 두 가지 용도로 사용 합니다.

1.  클라이언트 코드는 근본적인 복잡성 때문에 관해 알아야 할 필요 하지 않은 있도록 JNI 사용을 캡슐화 합니다.
1.  Java 형식 하위 클래스로 분류 하 고 Java 인터페이스를 구현할 수 있도록 설정 합니다.

첫 번째 용도 순수 하 게 편의성 및 복잡성을 캡슐화 하 여 소비자가 사용할 클래스를 간단 하 고 관리 되는 여러 가지입니다. 다양 한를 사용 해야 [JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) 멤버의이 문서의 뒷부분에서 설명 합니다. 관리 되는 호출 가능 래퍼 있음을 명심 엄격 하 게 필요 하지 않지만 &ndash; JNI 사용 하 여 "인라인" 완벽 하 게 허용 되 고 일회용 언바운드 Java 멤버 사용 하는 데 유용 합니다. 관리 되는 호출 가능 래퍼를 사용 해야 하는 하위 클래스로 생성 및 인터페이스 구현.



## <a name="android-callable-wrappers"></a>Android 호출 가능 래퍼

Android 런타임 (아트) 관리 되는 코드를; 호출 해야 할 때마다 android 호출 가능 래퍼 (ACW) 증명이 합니다. 이러한 래퍼는 런타임 시 아트 클래스를 등록할 수 없기 때문에 필요 합니다.
(특히 여 [DefineClass](http://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) JNI 함수가 Android 런타임에서 지원 되지 않습니다. Android 호출 가능 래퍼 따라서 구성 런타임 형식 등록 지원 문제에 대 한 합니다.)

Android 코드를 실행 가상 하거나 인터페이스 메서드 재정의 또는 관리 코드에서 구현 하는 때마다에이 메서드는 적절 한 관리 되는 형식으로 발송 됩니다 있도록 Xamarin.Android Java 프록시를 제공 해야 합니다. 이러한 Java 프록시 형식은 동일한 생성자를 구현 하 고 클래스는 재정의 된 기본 클래스 및 인터페이스 메서드 선언 관리 되는 형식으로 "동일" 기본 클래스 및 Java 인터페이스 목록에 있는 Java 코드 됩니다.

에 의해 생성 된 android 호출 가능 래퍼는 **monodroid.exe** 중의 프로그램은 [빌드 프로세스](~/android/deploy-test/building-apps/build-process.md), (직접 또는 간접적으로) 상속 하는 모든 형식에 대해 생성 되 고 [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)합니다.



### <a name="implementing-interfaces"></a>인터페이스 구현

Android 인터페이스를 구현 해야 하는 경우가 있습니다 (예: [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/)).

모든 Android 클래스와 인터페이스를 확장 된 [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) 인터페이스; 따라서 모든 Android 형식을 구현 해야 `IJavaObject`합니다.
Xamarin.Android이이 사실을 활용 &ndash; 사용 하 여 `IJavaObject` 지정된 된 관리 되는 형식에 대 한 Java 프록시 (한 Android 호출 가능 래퍼)를 사용한 Android 수 있도록 합니다. 때문에 **monodroid.exe** 만 찾습니다 `Java.Lang.Object` 서브 클래스 (구현 해야 함 `IJavaObject`), 서브클래싱 `Java.Lang.Object` 관리 코드에서 인터페이스를 구현 하는 방법을 제공 합니다. 예:

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

*이 문서의 나머지 부분에서는 예 고 없이 변경 될 수 구현 세부 정보를 제공* (및 개발자가 내부에서의 진행 상황에 대 한 자세한 내용을 보려면 수 때문에 여기서 설명).

예를 들어 다음 C# 소스:

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

**mandroid.exe** 프로그램 다음 Android 호출 가능 래퍼를 생성 합니다.

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

기본 클래스는 유지 되는 관리 코드 내에서 재정의할 각 방법에 대해 네이티브 메서드 선언이 제공 됩니다에 유의 하십시오.



### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute 및 ExportFieldAttribute

일반적으로 Xamarin.Android ACW; 구성 하는 Java 코드를 자동으로 생성 클래스는 Java 클래스에서 파생 되 고 기존 Java 메서드를 재정의 하는 경우이 세대는 클래스 및 메서드 이름에 기반 합니다. 그러나 일부 시나리오에서는 코드 생성 적합 하지 않은, 아래 설명 된 대로:

-   Android에서는 작업 이름은 레이아웃 XML 특성을 예를 들어는 [android: onClick](https://developer.xamarin.com/api/member/Android.Views.View+IOnClickListener.OnClick/p/Android.Views.View/) XML 특성입니다. 지정 된 확장된 인스턴스 보기에서 Java 메서드를 조회 하려고 합니다.

-   [java.io.Serializable](http://developer.android.com/reference/java/io/Serializable.html) 인터페이스 필요 `readObject` 및 `writeObject` 메서드. 아니므로이 인터페이스의 멤버를 해당 관리 되는 구현에는 이러한 메서드 Java 코드를 노출 하지 않습니다.

-   [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.Os.Parcelable/) 인터페이스는 구현 클래스는 정적 필드를 갖도록 `CREATOR` 형식의 `Parcelable.Creator`합니다. 생성 된 Java 코드에는 몇 가지 명시적 필드가 필요합니다. 표준이 시나리오는 관리 코드에서 Java 코드의 출력 필드가 수 없으므로 있습니다.


코드 생성 임의의 이름 갖는 임의의 Java 메서드를 생성 하는 솔루션을 제공 하지 않으므로, Xamarin.Android 4.2 부터는 [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/) 및 [ExportFieldAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportFieldAttribute/) 되었습니다 위의 시나리오에 해당 하는 솔루션을 제공 하는 도입 되었습니다. 두 특성 모두에 있는 `Java.Interop` 네임 스페이스:

-   `ExportAttribute` &ndash; 메서드 이름 및 (명시적 "throw" java에서 부여)를 해당 예상 되는 예외 유형을 지정 합니다. 에 메서드를 사용 하는 경우 메서드는 "export" 관리 되는 메서드를 해당 JNI 호출에 디스패치 코드를 생성 하는 Java 메서드. 이 프로필은 사용할 수 있습니다 `android:onClick` 및 `java.io.Serializable`합니다.

-   `ExportFieldAttribute` &ndash; 필드 이름을 지정합니다. 필드 이니셜라이저도 작동 하는 방법에 상주 합니다. 이 프로필은 사용할 수 있습니다 `android.os.Parcelable`합니다.

[ExportAttribute](https://developer.xamarin.com/samples/monodroid/ExportAttribute/) 샘플 프로젝트에 이러한 특성을 사용 하는 방법을 보여 줍니다.


#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>ExportAttribute 및 ExportFieldAttribute 문제 해결

-   누락 되어 패키징 실패 **Mono.Android.Export.dll** &ndash; 사용한 경우 `ExportAttribute` 또는 `ExportFieldAttribute` 코드 또는 종속 라이브러리의 일부 메서드를 추가 해야  **Mono.Android.Export.dll**합니다. 이 어셈블리는 Java에서 콜백 코드를 지원 하기 위해 격리 됩니다. 와 별개인 **Mono.Android.dll** 응용 프로그램에 추가 크기를 추가 합니다.

-   릴리스 빌드에서 `MissingMethodException` 내보내기 방법에 대 한 발생 &ndash; 에서 릴리스 빌드 `MissingMethodException` 내보내기 방법에 대해 발생 합니다. (이 문제는 Xamarin.Android의 최신 버전에서 해결 합니다.)



### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` 및 `ExportFieldAttribute` 런타임에 코드에서 사용할 수는 Java 기능을 제공 합니다. 이 런타임 코드 사항에 해당 특성에 의해 생성 된 JNI 메서드를 통해 관리 되는 코드에 액세스 합니다. 결과적으로, 관리 되는 메서드에 바인딩하; 기존 Java 메서드가 없는 따라서 Java 메서드는 관리 되는 메서드 서명에서 생성 됩니다.

그러나,이 경우 완전 하 게 결정 하지 않습니다. 특히,이 관리 되는 형식 및 Java 형식와 같은 일부 고급 매핑에 마찬가지입니다.

-  InputStream
-  OutputStream
-  XmlPullParser
-  XmlResourceParser

내보내는 방법에 대 한 이러한 형식이 필요한 경우는 `ExportParameterAttribute` 명시적으로 해당 매개 변수를 제공 하거나 값 형식을 반환 하는 데 사용 해야 합니다.



### <a name="annotation-attribute"></a>특성 주석

Xamarin.Android 4.2에서으로 변환할 `IAnnotation` 구현 형식을 특성 (System.Attribute) 및 Java 래퍼에 주석 생성에 대 한 지원이 추가 되었습니다.

이 다음과 같은 방향 변경을 의미합니다.

-   바인딩 생성기에서는 생성 `Java.Lang.DeprecatedAttribute` 에서 `java.Lang.Deprecated` (해야 하는 동안 `[Obsolete]` 관리 코드에서).

-   기존 그렇다고 `Java.Lang.Deprecated` 클래스는 사라집니다. 이러한 Java 기반 개체 여전히으로 사용 될 수 일반적인 Java 개체 (이러한 기술이 사용 있는 경우). 됩니다 `Deprecated` 및 `DeprecatedAttribute` 클래스입니다.

-   `Java.Lang.DeprecatedAttribute` 클래스로 표시 되며 `[Annotation]` 합니다. 이 상속 되는 사용자 지정 특성 경우 `[Annotation]` 특성을 msbuild 작업에서 해당 사용자 지정 특성에 대 한 Java 주석이 생성 합니다 (@Deprecated) Android는 호출 가능 래퍼 (ACW)에서.

-   클래스, 메서드를 생성할 수 없습니다. 주석과 내보낼 필드 (이 메서드가 관리 코드에서).

(주석이 추가 된 클래스 자체 또는 주석이 추가 된 멤버를 포함 하는 클래스) 포함 하는 클래스 등록 되지 않은 경우 전체 Java 클래스 소스 생성 되지 않습니다 전혀 주석 포함 한. 메서드를 지정할 수 있습니다는 `ExportAttribute` 메서드를 명시적으로 생성 하 고 주석 처리를 가져올 합니다. 또한 "생성" Java 주석 클래스 정의 하는 기능이 아닙니다. 즉, 특정 주석에 대 한 관리 되는 사용자 지정 특성을 정의 하는 경우에 해당 하는 Java 주석 클래스를 포함 하는 다른.jar 라이브러리를 추가 해야 합니다. 주석 형식을 정의 하는 Java 소스 파일을 추가 하는 충분 하지 않습니다. Java 컴파일러는 동일한 방식으로 작동 하지 않습니다 **apt**합니다.

또한 다음과 같은 제한 사항이 적용 됩니다.

-   이 변환 프로세스를 고려 하지 않습니다 `@Target` 지금까지 주석에서 주석 형식입니다.

-   특성 속성을 작동 하지 않습니다. 속성 getter 또는 setter에 대 한 특성을 대신 사용 합니다.



## <a name="class-binding"></a>클래스 바인딩

바인딩 클래스 기본 Java 형식의 호출을 간소화 하기 위해 관리 되는 호출 가능 래퍼 작성 하는 것을 의미 합니다.

C#에서 재정의 허용 하기 위해 가상 및 추상 메서드 바인딩 Xamarin.Android 4.0이 필요 합니다. 하지만 모든 버전의 Xamarin.Android 재정의 지원 하지 않고 비가상 메서드, 정적 메서드 또는 가상 메서드 바인딩할 수 있습니다.

일반적으로 바인딩을는 다음 항목이 들어 있습니다.

-  A [JNI 바인딩되는 Java 형식에 대 한 핸들](#_Looking_up_Java_Types)합니다.

-  [JNI 필드 Id 및 각 바인딩된 필드에 대 한 속성](#_Instance_Fields)합니다.

-  [JNI 메서드 Id와 메서드 각각에 대해 바인딩된 메서드](#_Instance_Methods)합니다.

-  하위 클래스로 생성이 필요한 경우 형식 해야는 [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) 형식 선언에 사용자 지정 특성 [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) 로 설정 `true`합니다.



### <a name="declaring-type-handle"></a>형식 핸들 선언

필드 및 메서드 조회 방법에는 해당 선언 형식을 참조 하는 개체 참조를 해야 합니다. 규칙에 따라에 보유 되므로이 `class_ref` 필드:

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

참조는 [JNI 형식 참조](#_JNI_Type_References) 에 대 한 자세한 내용은 섹션은 `CLASS` 토큰입니다.


### <a name="binding-fields"></a>바인딩 필드

C# 속성, 예를 들어 Java 필드도 노출 된 Java 필드 [java.lang.System.in](http://developer.android.com/reference/java/lang/System.html#in) C# 속성으로 바인딩된 [Java.Lang.JavaSystem.In](https://developer.xamarin.com/api/property/Java.Lang.JavaSystem.In/)합니다.
또한 정적 필드 및 인스턴스 필드를 구분 하는 JNI, 이후 다양 한 방법은 구현할 때 사용할 속성입니다.

필드 바인딩 세 가지 메서드가 포함 됩니다.

1.  *필드 id를 가져올* 메서드. *필드 id를 가져올* 메서드는 필드를 처리 하는 반환 하는 데는 *필드 값을 가져오려면* 및 *필드 값을 설정* 메서드를 사용 합니다. 필드 id를 얻으려면 알면 선언 하는 필드의 이름을 입력 해야 하 고 [JNI 형식 시그니처](#_JNI_Type_Signatures) 필드의 합니다.

1.  *필드 값을 가져오려면* 메서드. 이러한 메서드는 필요한 필드 핸들 및 Java에서 필드의 값을 읽는 역할을 합니다.
    메서드를 사용 하 여 필드의 형식에 따라 다릅니다.

1.  *필드 값을 설정할* 메서드. 이러한 메서드는 필요한 필드 핸들 및 Java 내에서 필드의 값을 작성 하는 일을 담당 합니다. 메서드를 사용 하 여 필드의 형식에 따라 다릅니다.


 [정적 필드](#_Static_Fields) 사용는 [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/), `JNIEnv.GetStatic*Field`, 및 [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/) 메서드.

 [인스턴스 필드](#_Instance_Fields) 사용는 [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/), `JNIEnv.Get*Field`, 및 [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/) 메서드.

예를 들어 정적 속성 `JavaSystem.In` 으로 구현할 수 있습니다.

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

참고: 사용 하 여 [InputStreamInvoker.FromJniHandle](https://developer.xamarin.com/api/member/Android.Runtime.InputStreamInvoker.FromJniHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) JNI 참조로 변환 하는 `System.IO.Stream` 고 인스턴스를 사용 하는 `JniHandleOwnership.TransferLocalRef` 때문에 [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) 로컬 참조를 반환합니다.

대부분의 [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) 형식 `FromJniHandle` 는 JNI 변환 하는 메서드를 원하는 형식으로 참조 합니다.



### <a name="method-binding"></a>메서드 바인딩

Java 메서드는 메서드로 C# 및 C# 속성으로 노출 됩니다. 예를 들어 Java 메서드 [java.lang.Runtime.runFinalizersOnExit](http://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) 메서드 자동으로 바인딩되기는 [Java.Lang.Runtime.RunFinalizersOnExit](https://developer.xamarin.com/api/member/Java.Lang.Runtime.RunFinalizersOnExit/) 메서드, 및 [java.lang.Object.getClass ](http://developer.android.com/reference/java/lang/Object.html#getClass) 메서드 자동으로 바인딩되기는 [Java.Lang.Object.Class](https://developer.xamarin.com/api/property/Java.Lang.Object.Class/) 속성입니다.

메서드 호출에는 두 단계로 이루어집니다.

1.  *메서드 id 가져오기* 호출할 메서드에 대 한 합니다. *메서드 id 가져오기* 메서드는 메서드 호출 메서드에서 사용 하는 메서드 핸들을 반환 합니다. 메서드 id를 얻으려면 알면 선언 하는 메서드의 이름을 입력 해야 하 고 [JNI 형식 시그니처](#_JNI_Type_Signatures) 메서드의 합니다.

1.  메서드를 호출합니다.

필드의 경우와 마찬가지로 메서드 id를 가져오고 메서드를 호출 하는 데 메서드는 정적 메서드와 인스턴스 메서드 간에 서로 다릅니다.

[정적 메서드](#_Static_Methods_1) 사용 [JNIEnv.GetStaticMethodID()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/) 메서드 id를 조회 하 여 사용할는 `JNIEnv.CallStatic*Method` 호출에 대 한 메서드의 제품군입니다.

[인스턴스 메서드](#_Instance_Methods) 사용 [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/) 메서드 id를 조회 하 고 사용 하는 `JNIEnv.Call*Method` 및 `JNIEnv.CallNonvirtual*Method` 호출에 대 한 메서드의 제품군입니다.

바인딩 메서드는 둘 이상의 잠재적으로 메서드 호출 합니다. 또한에 포함 되어 있으므로 (추상 및 메서드에 대 한 최종이 아닌) 재정의 하는 방법 또는 (인터페이스 메서드)에 대 한 구현 된 메서드 바인딩. [상속을 지 원하는 인터페이스](#_Supporting_Inheritance,_Interfaces_1) 섹션에서는 가상 메서드 및 인터페이스 메서드를 지 원하는의 복잡성에 설명 합니다.

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>정적 메서드

사용 하 여 정적 메서드를 바인딩에서는 `JNIEnv.GetStaticMethodID` 적절 한 사용 하는 메서드 핸들을 얻기 위해 `JNIEnv.CallStatic*Method` 메서드의 반환 형식에 따라 메서드. 다음은에 대 한 바인딩의 예는 [Runtime.getRuntime](http://developer.android.com/reference/java/lang/Runtime.html#getRuntime()) 메서드:

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

메서드 핸들 정적 필드에 저장 된 참고 `id_getRuntime`합니다. 이 소프트웨어는 성능 최적화를 호출할 때마다 조회 메서드 핸들 필요 하지 않습니다. 이러한 방식으로 메서드 핸들을 캐시 하려면 필요는 없습니다. 메서드 핸들을 받은 후 [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) 메서드를 호출 하는 데 사용 됩니다. `JNIEnv.CallStaticObjectMethod` 반환 된 `IntPtr` 반환 된 Java 인스턴스 핸들을 포함 하 합니다.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Java 핸들을 강력한 형식의 개체 인스턴스를 변환 하는 데 사용 됩니다.



#### <a name="non-virtual-instance-method-binding"></a>가상이 아닌 인스턴스 메서드 바인딩

바인딩는 `final` 사용 하 여 인스턴스 메서드 또는 인스턴스 메서드를 재정의 하 고, 필요 하지 않은 `JNIEnv.GetMethodID` 적절 한 사용 하는 메서드 핸들을 얻기 위해 `JNIEnv.Call*Method` 메서드의 반환 형식에 따라 메서드. 다음은에 대 한 바인딩의 예는 `Object.Class` 속성:

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

메서드 핸들 정적 필드에 저장 된 참고 `id_getClass`합니다.
이 소프트웨어는 성능 최적화를 호출할 때마다 조회 메서드 핸들 필요 하지 않습니다. 이러한 방식으로 메서드 핸들을 캐시 하려면 필요는 없습니다. 메서드 핸들을 받은 후 [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) 메서드를 호출 하는 데 사용 됩니다. `JNIEnv.CallStaticObjectMethod` 반환 된 `IntPtr` 반환 된 Java 인스턴스 핸들을 포함 하 합니다.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Java 핸들을 강력한 형식의 개체 인스턴스를 변환 하는 데 사용 됩니다.


### <a name="binding-constructors"></a>바인딩 생성자

생성자는 Java 메서드 이름의 `"<init>"`합니다. Java 인스턴스 메서드와 마찬가지로 `JNIEnv.GetMethodID` 생성자 핸들을 조회 하는 데 사용 됩니다. Java 메서드와 달리는 [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/) 메서드는 생성자 메서드 핸들을 호출 하는 데 사용 됩니다. 반환 값 `JNIEnv.NewObject` JNI 로컬 참조:


```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

클래스 바인딩의 하위 클래스는 일반적으로 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)합니다.
서브클래싱 할 경우 `Java.Lang.Object`에 추가 의미 체계 발생:는 `Java.Lang.Object` 인스턴스를 통해 Java 인스턴스에 대 한 전역 참조를 유지 관리는 `Java.Lang.Object.Handle` 속성입니다.

1.  `Java.Lang.Object` 기본 생성자는 Java 인스턴스를 할당 합니다.

1.  형식에 있는 경우는 `RegisterAttribute` , 및 `RegisterAttribute.DoNotGenerateAcw` 은 `true` , 다음의 인스턴스는 `RegisterAttribute.Name` 유형이 기본 생성자를 통해 만들어집니다.

1.  그렇지 않은 경우는 [Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md) (ACW)에 해당 하 `this.GetType` 기본 생성자를 통해 인스턴스화됩니다. Android 호출 가능 래퍼에 대 한 패키지를 만드는 동안 생성 된 모든 `Java.Lang.Object` 하위 클래스를 `RegisterAttribute.DoNotGenerateAcw` 로 설정 되지 않은 `true`합니다.

이 예상 되는 바인딩 하지 클래스에 대 한 의미 체계: 인스턴스화는 `Mono.Samples.HelloWorld.HelloAndroid` C# 인스턴스 Java를 구성 해야 `mono.samples.helloworld.HelloAndroid` 인스턴스는 생성 된 Android 호출 가능 래퍼는 합니다.

클래스 바인딩에 대 한 기본 생성자를 포함 하는 Java 형식 및/또는 다른 생성자를 호출 해야 하는 경우 올바른 동작 수 있습니다. 다음 작업을 수행 하는 생성자를 제공, 해야 합니다.

1.  호출 하 여 [Java.Lang.Object (IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) 기본값 대신 `Java.Lang.Object` 생성자입니다. 새 Java 인스턴스 않도록 하기 위해 필요 합니다.

1.  값을 확인 [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) 모든 Java 인스턴스를 만들기 전에 합니다. `Object.Handle` 속성이 갖는 값 이외의 다른 `IntPtr.Zero` Android 호출 가능 래퍼는 Java 코드에서 생성 된 클래스 바인딩이 만들어진된 Android 호출 가능 래퍼 인스턴스에 포함 하기 위해 생성 되 고 있습니다. 예를 들어, Android를 만들면는 `mono.samples.helloworld.HelloAndroid` 인스턴스를는 첫 번째, 및 Java는 Android 호출 가능 래퍼가 생성 됩니다 `HelloAndroid` 생성자는 해당 인스턴스를 만듭니다. `Mono.Samples.HelloWorld.HelloAndroid` 유형와 `Object.Handle` 속성 생성자를 실행 하기 전에 Java 인스턴스를 설정 합니다.

1.  현재 런타임 형식을 선언 하는 경우 형식, 해당 Android 호출 가능 래퍼의 인스턴스를 만들어야 하 고 사용 하 여 [Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) 에서 반환 된 핸들을 저장 하 [ JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/)합니다.

1.  현재 런타임 형식을 선언 형식으로 동일한 경우 다음 Java 생성자를 호출 하 고 사용 하 여 [Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) 에서 반환 된 핸들을 저장 하 `JNIEnv.NewInstance` 합니다.


예를 들어는 [java.lang.Integer(int)](http://developer.android.com/reference/java/lang/Integer.html#Integer(int)) 생성자입니다. 로 바인딩된:

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

[JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/) 메서드는 수행 하는 도우미는 `JNIEnv.FindClass`, `JNIEnv.GetMethodID`, `JNIEnv.NewObject`, 및 `JNIEnv.DeleteGlobalReference` 에서 반환 된 값에 `JNIEnv.FindClass`합니다. 자세한 내용은 다음 섹션을 참조하십시오.

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>인터페이스 상속 지원

Java 형식 서브클래싱 또는 Java 인터페이스를 구현 생성 해야 합니다. [Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md) (ACWs)에 대해 생성 되는 모든 `Java.Lang.Object` 패키지를 만드는 동안 하위 클래스입니다. ACW 생성을 통해 제어 되는 [Android.Runtime.RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) 사용자 지정 특성입니다.

C#의 형식에 대 한는 `[Register]` 사용자 지정 특성 생성자에는 하나의 인수가 필요:는 [JNI 간체 형식 참조](#_Simplified_Type_References_1) 해당 Java 입력 합니다. 이렇게 하면 Java와 C# 간의 서로 다른 이름을 제공 합니다.

Xamarin.Android 4.0 이전는 `[Register]` 사용자 지정 특성 "별칭" 기존 Java 형식에 사용할 수 없습니다. ACW 생성 프로세스에 대 한 ACWs 생성 하기 때문에이 모든 `Java.Lang.Object` 발생 하는 하위 클래스입니다.

Xamarin.Android 도입 4.0는 [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) 속성입니다. 이 속성을 ACW 생성 프로세스 지시 *건너뛸* 허용 새 관리 되는 호출 가능 래퍼 패키지를 만들 때 생성 되 고 ACWs 발생 하지 것입니다는 선언의 주석이 추가 된 형식입니다. 이렇게 하면 바인딩 기존 Java 형식입니다. 예를 들어 다음과 같은 간단한 Java 클래스 `Adder`, 메서드 하나가 포함 된 `add`, 정수에 추가 하 고 결과 반환 하는 합니다.

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

`Adder` 형식으로 바인딩할 수 없습니다.

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

여기서는 `Adder` C# 형식은 *별칭* 는 `Adder` Java 형식입니다. `[Register]` 특성의 JNI 이름을 지정 하는 데 사용 됩니다는 `mono.android.test.Adder` Java 형식 및 `DoNotGenerateAcw` 속성 ACW 생성을 금지 하는 데 사용 됩니다. 에 대 한 ACW 생성 이렇게 하면는 `ManagedAdder` 형식가 제대로 하위 클래스는 `mono.android.test.Adder` 유형입니다. 경우는 `RegisterAttribute.DoNotGenerateAcw` 속성을 사용 하지 않은 다음 Xamarin.Android 빌드 프로세스를 생성 하는 새 `mono.android.test.Adder` Java 형식입니다. 이렇게 하면 컴파일 오류가로 `mono.android.test.Adder` 형식은 두 개의 별도 파일을 두 번에 표시 됩니다.



### <a name="binding-virtual-methods"></a>가상 메서드 바인딩

`ManagedAdder` 하위 클래스는 Java `Adder` 종류와 특히 흥미롭습니다 없는: C# `Adder` 형식 이므로 모든 가상 메서드를 정의 하지 않습니다 `ManagedAdder` 아무 것도 재정의할 수 없습니다.

바인딩 `virtual` 서브 클래스에서 재정의 허용 하는 메서드를 다음 두 가지 범주로 나뉩니다를 수행 해야 할 몇 가지 필요:

1.  **메서드 바인딩**

1.  **메서드 등록**


#### <a name="method-binding"></a>메서드 바인딩

메서드 바인딩 C# 지원 멤버 두 명의 추가 해야 합니다. `Adder` 정의: `ThresholdType`, 및 `ThresholdClass`합니다.

##### <a name="thresholdtype"></a>ThresholdType

`ThresholdType` 바인딩의 현재 형식을 반환 합니다.

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType` 가상 메서드 디스패치에 대 가상 수행 해야 하는 시기를 결정 하 메서드 바인딩에 사용 됩니다. 항상 반환 된 `System.Type` 선언 C# 형식에 해당 하는 인스턴스.

##### <a name="thresholdclass"></a>ThresholdClass

`ThresholdClass` 속성 바인딩된 형식에 대 한 JNI 클래스 참조를 반환 합니다.

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` 비가상 메서드를 호출할 때 메서드에 바인딩에 사용 됩니다.

#### <a name="binding-implementation"></a>바인딩 구현

메서드 바인딩 구현을 Java 메서드의 런타임 호출을 담당 합니다. 또한 포함 되어는 `[Register]` 메서드 등록의 일부 이며 메서드 등록 섹션에서 설명 하는 사용자 지정 특성 선언의:

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

`id_add` 필드 호출할 Java 메서드에 대 한 메서드 ID를 포함 합니다. `id_add` 값에서 가져온 `JNIEnv.GetMethodID`를 선언 하는 클래스를 요구 하 (`class_ref`), Java 메서드 이름 (`"add"`), 및 메서드의 JNI 서명 (`"(II)I"`).

메서드 ID를 받은 후 `GetType` 에 비해 `ThresholdType` 가상 또는 비가상 디스패치가 필요한 지 여부를 확인 하려면. 가상 디스패치가 필요 `GetType` 일치 `ThresholdType`,으로 `Handle` 메서드를 재정의 하는 Java 할당 하위 클래스를 참조할 수 있습니다.

때 `GetType` 일치 하지 않으면 `ThresholdType`, `Adder` 서브클래싱된 (예: 여 `ManagedAdder`), 및 `Adder.Add` 구현 하위 클래스를 호출 하는 경우에 호출할 수 `base.Add`합니다. 에는 비가상 디스패치 좋다고 `ThresholdClass` 제공 합니다. `ThresholdClass` Java 클래스에는 호출할 메서드의 구현을 제공 합니다를 지정 합니다.



#### <a name="method-registration"></a>메서드 등록

업데이트 된 있다고 가정 `ManagedAdder` 정의 재정의 `Adder.Add` 메서드:

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

이전에 설명한 대로 `Adder.Add` 했습니다는 `[Register]` 사용자 지정 특성:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

`[Register]` 사용자 지정 특성 생성자 3 개 값을 사용할 수 있습니다.

1.  Java 메서드의 이름을 `"add"` 이 경우.

1.  메서드의 JNI 형식 시그니처 `"(II)I"` 이 경우.

1.  *커넥터 메서드* , `GetAddHandler` 이 경우.
    커넥터 메서드는 나중에 설명 합니다.


처음 두 매개 변수는 메서드를 재정의 하는 메서드 선언을 생성 하는 ACW 생성 프로세스를 사용 합니다. 다음 코드의 일부 결과 ACW이 포함 됩니다.

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

`@Override` 메서드 선언에 위임 하는 `n_`-접두사가 동일한 이름의 메서드. 이렇게 하는 Java 코드를 호출 하는 경우 `ManagedAdder.add`, `ManagedAdder.n_add` 호출 되는 사용 하면 재정의 C# `ManagedAdder.Add` 메서드를 실행할 수 있습니다.

따라서 가장 중요 한 질문: 어떻게는 `ManagedAdder.n_add` 로 후크 `ManagedAdder.Add`?

Java `native` 메서드를 통해 Java (Android 런타임) 런타임에 등록 되는 [JNI RegisterNatives 함수](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734)합니다.
`RegisterNatives` 다음에 나오는를 호출 하는 Java 메서드 이름, JNI 형식 시그니처 및 함수 포인터를 포함 하는 구조체의 배열을 사용 [호출 규칙 JNI](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715)합니다.
함수 포인터 메서드 매개 변수가 두 개의 포인터 인수를 사용 하는 함수 여야 합니다. 이 Java `ManagedAdder.n_add` 다음 C 프로토타입을 가진 함수를 통해 구현 해야 합니다.

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.Android 노출 하지 않습니다는 `RegisterNatives` 메서드. 대신는 ACW 및은 MCW 함께 제공를 호출 하는 데 필요한 정보 `RegisterNatives`: 함수 포인터를 연결 하는 유일한 검는 ACW 메서드 이름 및 JNI 형식 시그니처를 포함 합니다.

여기에는 *커넥터 메서드* 제공 합니다. 세 번째 `[Register]` 사용자 지정 특성 매개 변수는 등록 된 형식 또는 매개 변수를 허용 하 고 반환 하는 등록 된 형식의 기본 클래스에 정의 된 메서드의 이름을 `System.Delegate`합니다. 반환 된 `System.Delegate` 올바른 JNI 함수 서명이 있는 메서드를 참조 합니다. 커넥터 메서드가 반환 하는 대리자에 마지막으로, *해야* 루트 GC를 수집 하지 않도록 java 대리자를 제공 하는 것입니다.

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

`GetAddHandler` 메서드 만듭니다는 `Func<IntPtr, IntPtr, int, int,
int>` 을 참조 하는 대리자는 `n_Add` 메서드를 다음 호출 [JNINativeWrapper.CreateDelegate](https://developer.xamarin.com/api/member/Android.Runtime.JNINativeWrapper.CreateDelegate/)합니다.
`JNINativeWrapper.CreateDelegate` 모든 처리 되지 않은 예외가 처리 되 고 발생 하면 되도록 try/catch 블록에 제공 된 메서드를 래핑합니다는 [AndroidEvent.UnhandledExceptionRaiser](https://developer.xamarin.com/api/event/Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser/) 이벤트입니다. 결과로 생성 된 대리자는 정적에 저장 된 `cb_add` 변수를 GC 대리자를 해제 하지 것입니다.

마지막으로 `n_Add` 메서드는 다음 호출 메서드에 위임 JNI 매개 변수는 해당 하는 관리 되는 형식과를 마샬링합니다.

참고: 항상 사용 하 여 `JniHandleOwnership.DoNotTransfer` Java 인스턴스를 덮어써는 MCW을 가져올 때. 로컬 참조로 처리 하는 방법 (및 따라서 호출 `JNIEnv.DeleteLocalRef`)의 연결이 끊어집니다 관리-&gt; Java-&gt; 스택 전환을 관리 하는 합니다.



### <a name="complete-adder-binding"></a>Adder 바인딩 완료

관리 되는 전체에 대 한 바인딩을 `mono.android.tests.Adder` 유형입니다.

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

형식을 쓸 때 다음 조건에 일치 하는:

1.  서브 클래스 `Java.Lang.Object`

1.  에 `[Register]` 사용자 지정 특성

1.  `RegisterAttribute.DoNotGenerateAcw`가 `true`인 경우


GC 상호 작용 형식에 대 한 다음 *하지 않아야* 을 참조할 수 있는 필드가 `Java.Lang.Object` 또는 `Java.Lang.Object` 런타임에 하위 클래스입니다. 예를 들어 필드 형식의 `System.Object` 임의의 인터페이스 형식에 사용할 수 없습니다. 참조할 수 있는 유형 `Java.Lang.Object` 인스턴스를 사용할 수와 같은 `System.String` 및 `List<int>`합니다. 이 제한은 GC가 예기치 않은 개체 컬렉션을 방지 하기 위해입니다.

형식을를 참조할 수 있는 인스턴스 필드를 포함 해야 하는 경우는 `Java.Lang.Object` 인스턴스 필드 형식 이어야 합니다 `System.WeakReference` 또는 `GCHandle`합니다.



## <a name="binding-abstract-methods"></a>추상 메서드 바인딩

바인딩 `abstract` 메서드는 가상 메서드를 바인딩할 거의 동일 합니다. 다음과 같은 두 가지 차이점이 있습니다.

1.  추상 메서드는 추상 클래스입니다. 계속 유지는 `[Register]` 특성 및 관련된 메서드 등록 메서드 바인딩은으로 이동 하는 `Invoker` 유형입니다.

1.  비- `abstract` `Invoker` 형식을 만들 서브클래싱하는 추상 형식입니다. `Invoker` 형식을 기본 클래스에 선언 된 모든 추상 메서드를 재정의 해야 및 재정의 된 구현은 비가상 디스패치 대/소문자를 무시할 수 있지만 메서드 바인딩 구현 하는 합니다.


예를 들어 위의 `mono.android.test.Adder.add` 메서드가 `abstract`합니다. C# 바인딩 변경 되도록 `Adder.Add` abstract, 하 고 새 `AdderInvoker` 구현을 하는 형식 정의 됩니다 `Adder.Add`:

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

`Invoker` 형식은 JNI 참조 Java 만든 인스턴스를 가져올 경우에 필요한입니다.


## <a name="binding-interfaces"></a>바인딩 인터페이스

바인딩 인터페이스는 개념적으로 바인딩 가상 메서드를 포함 하는 클래스와 유사 하지만 다양 한 구체적인 미묘한 (및 없는 경우) 같은 차이점이 있습니다. 다음 사항을 고려 [Java 인터페이스 선언](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14):

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

인터페이스 바인딩에 두 부분으로 구성: C# 인터페이스 정의 및 인터페이스에 대 한 호출자 정의 합니다.



### <a name="interface-definition"></a>인터페이스 정의

C# 인터페이스 정의는 다음 요구 사항을 충족 해야 합니다.

-   인터페이스 정의 있어야는 `[Register]` 사용자 지정 특성입니다.

-   인터페이스 정의 확장 해야 합니다는 `IJavaObject interface`합니다.
    이렇게 하지 않으면은 ACWs Java 인터페이스에서 상속 되지 것입니다.

-   각 인터페이스 메서드를 포함 해야 합니다는 `[Register]` 해당 Java 메서드 이름, JNI 서명 및 커넥터 메서드를 지정 하는 특성입니다.

-   커넥터 메서드 커넥터 메서드에 있을 수 있는 형식을 지정 해야 합니다.

바인딩할 때 `abstract` 및 `virtual` 메서드, 커넥터 메서드 등록 되는 형식의 상속 계층 구조 내에서 검색 합니다. 인터페이스는 작동 하지 않을, 따라서 요구 사항은 본문을 포함 하는 메서드가 없으므로 가질 수 있습니다 커넥터 메서드가 있는 위치를 나타내는 형식을 지정할 수는 있습니다. 콜론 커넥터 메서드 문자열 내에서 지정 된 형식 `':'`, 호출자를 포함 하는 형식의 어셈블리 정규화 된 형식 이름 이어야 합니다.

인터페이스 메서드 선언 된 한 번역을 사용 하 여 해당 Java 메서드 *호환* 형식입니다. Java 기본 제공 형식에 대 한 호환 되는 유형 유형은 해당 C#, 예를 들어 Java `int` 는 C# `int`합니다. 참조 형식, 호환 되는 형식은 적절 한 Java 형식의 JNI 핸들을 제공할 수 있는 형식입니다.

Java에서 인터페이스 멤버는 직접 호출 되지 것입니다 &ndash; 호출 호출자 형식을 통해 조정 됩니다 &ndash; 약간의 유연성이 될 수 있도록 합니다.

Java 진행률 인터페이스 수 [에 C#으로 선언 된](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83):

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Java를 매핑할 있습니다 위의 공지 `int[]` 매개 변수를 한 [JavaArray&lt;int&gt;](https://developer.xamarin.com/api/type/Android.Runtime.JavaArray%601/)합니다.
이 필요 하지 않습니다: म 수 바인딩한 하 고 C# `int[]`, 또는 `IList<int>`, 어떤 것 완전히 합니다. 어떤 형식이 선택 됩니다는 `Invoker` Java 번역할 수 있어야 `int[]` 호출에 대 한 유형입니다.


### <a name="invoker-definition"></a>호출자 정의

`Invoker` 형식 정의 상속 해야 합니다. `Java.Lang.Object`적절 한 인터페이스를 구현 하 고 인터페이스 정의에서 참조 하는 모든 연결 메서드를 제공 합니다. 다른 클래스 바인딩 더 적절 한:는 `class_ref` Id 필드와 메서드가 인스턴스 멤버를 static이 아닌 멤버 여야 합니다.

인스턴스 멤버를 선호 하는 이유와 관련이 있습니다 `JNIEnv.GetMethodID` Android 런타임에 동작 합니다. (이 Java 동작에도 발생할 수 있습니다; 테스트 되지 않은 요소가) `JNIEnv.GetMethodID` 구현된 된 인터페이스와 하지 선언 된 인터페이스에서 제공 하는 메서드를 조회할 때 null을 반환 합니다. 고려는 [java.util.SortedMap&lt;K, V&gt; ](http://developer.android.com/reference/java/util/SortedMap.html) Java 인터페이스를 구현 하는 [java.util.Map&lt;K, V&gt; ](http://developer.android.com/reference/java/util/Map.html) 인터페이스입니다. 맵은 한 [선택을 취소](http://developer.android.com/reference/java/util/Map.html#clear()) 메서드, 따라서 겉보기 바람직합니다 `Invoker` SortedMap에 대 한 정의 것:

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

위의 실패 하 게 `JNIEnv.GetMethodID` 반환 됩니다 `null` 을 조회할 때는 `Map.clear` 통해 메서드는 `SortedMap` 클래스 인스턴스.

이 두 가지 솔루션: 모든 메서드에서 제공 하는 인터페이스를 추적 하 고 있는 `class_ref` 각각에 대 한 인터페이스를 또는 인스턴스 멤버로 모든 것이 고 가장 많이 파생 된 클래스 형식, 인터페이스 형식이 아닌에서 메서드 조회를 수행 합니다. 후자 이루어집니다 **Mono.Android.dll**합니다.

호출자 정의 6 개의 섹션이: 생성자는 `Dispose` 메서드를는 `ThresholdType` 및 `ThresholdClass` 멤버는 `GetObject` 인터페이스 메서드 구현 방법과 커넥터 메서드 구현 합니다.



#### <a name="constructor"></a>생성자

생성자 호출 되는 인스턴스의 런타임 클래스를 조회 하 고 인스턴스에 런타임 클래스를 저장 하는 데 필요한 `class_ref` 필드:

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

참고:는 `Handle` 생성자 본문 내에서 속성을 사용 해야 및 not는 `handle` 매개 변수를 Android v 4.0에서와 같이 `handle` 실행이 끝나면 기본 생성자는 매개 변수 유효할 수 있습니다.


#### <a name="dispose-method"></a>Dispose 메서드

`Dispose` 메서드를 생성자에 할당 된 전역 참조를 해제 해야 합니다.

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

`ThresholdType` 및 `ThresholdClass` 멤버 클래스 바인딩에 있는 내용에 동일 합니다.

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

정적 `GetObject` 메서드를 지원 하기 위해 필요한 [Extensions.JavaCast&lt;T&gt;()](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/):

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```


#### <a name="interface-methods"></a>인터페이스 메서드

인터페이스의 모든 메서드에 구현이 JNI 통해 해당 Java 메서드를 호출 해야 합니다.

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

커넥터 메서드 및 지원 인프라는 마샬링 JNI 매개 변수를 적절 한 C# 형식에 대 한 합니다. 이 Java `int[]` 매개 변수는 JNI로 전달 됩니다 `jintArray`, 즉는 `IntPtr` 내에서 C#입니다. `IntPtr` 에 마샬링되어야는 `JavaArray<int>` C# 인터페이스가 호출을 지원 하려면:

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

경우 `int[]` 선호 것 `JavaList<int>`, 다음 [JNIEnv.GetArray()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetArray/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership%2cSystem.Type)) 대신 사용할 수 있습니다.

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

그러나는 `JNIEnv.GetArray` Vm, 큰 배열에 대 한이 인해 많은 추가 된 GC 압력에 있으므로 간의 전체 배열에 복사 합니다.


### <a name="complete-invoker-definition"></a>호출자 정의 완성

[IAdderProgressInvoker 정의 완성](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88):

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

대부분의 JNIEnv 메서드 반환 *JNI* *개체 참조*는 비슷합니다는 `GCHandle`s입니다. 세 가지 유형의 개체 참조를 제공 하는 JNI: 로컬 참조, 전역 참조 및 글로벌 약한 참조 합니다. 세 가지 모두로 표현 됩니다 `System.IntPtr`, *하지만* (JNI 함수 유형 섹션)에 따라 일부 `IntPtr`에서 반환 된 `JNIEnv` 메서드는 참조 합니다. 예를 들어 [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/) 반환는 `IntPtr`, 반환 되지 않는 개체 참조를 반환 하지만 `jmethodID`합니다. 참조는 [JNI 함수 설명서](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html) 대 한 자세한 내용은 합니다.

로컬 참조 하 여 생성 *대부분* 메서드 참조 만들기.
Android 제한 된 수에 대 한 로컬 참조 512 일반적으로 모든 지정된 시간에 존재 하는 것을 허용 합니다. 로컬 참조를 통해 삭제할 수 있습니다 [JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/)합니다.
JNIEnv 메서드를 반환 개체를 참조 하는 로컬 참조;를 반환 합니다. 참조 일부만 JNI 달리 [JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/) 반환는 *글로벌* 참조 합니다. 로컬 참조를 눌러도 수를 구성 하 여 신속 하 게 삭제 하는 것이 좋습니다는 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) 개체 주위를 지정 하 고 `JniHandleOwnership.TransferLocalRef` 에 [(IntPtr Java.Lang.Object 처리, JniHandleOwnership 전송)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) 생성자입니다.

전역 참조 하 여 생성 [JNIEnv.NewGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewGlobalRef/) 및 [JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/)합니다.
제거할 수 있습니다. [JNIEnv.DeleteGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteGlobalRef/)합니다.
에뮬레이터는 해결 되지 않은 전역 참조가 2, 000 개로 제한 있고 하드웨어 장치는 약 52,000 전역 참조 개로 제한 합니다.

전역 약한 참조는 Android v2.2 (Froyo) 이상에 사용할 수 있습니다. 와 함께 전역 약한 참조를 삭제할 수 [JNIEnv.DeleteWeakGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteWeakGlobalRef/(System.IntPtr))합니다.


### <a name="dealing-with-jni-local-references"></a>JNI 로컬 참조 처리와

[JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/), [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/), [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/), [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/)및 [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) 메서드 반환는 `IntPtr` Java 개체로 JNI 로컬 참조를 포함 하 또는 `IntPtr.Zero` Java 반환 되 면 `null`합니다. 이 참조가 있는지 확인 하는 것이 바람직 (512 항목) 되 면에서 처리 되지 않은 로컬 참조의 수를 제한으로 인해 적절 한 시간 내에 삭제 됩니다. 으로 로컬 참조를 처리할 수 있는 세 가지: 명시적으로 삭제를 만드는 `Java.Lang.Object` , 및 사용 하 여 저장 하는 인스턴스 `Java.Lang.Object.GetObject<T>()` 이들을 관리 되는 호출 가능 래퍼를 만들려고 합니다.



### <a name="explicitly-deleting-local-references"></a>로컬 참조를 명시적으로 삭제

[JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) 로컬 참조를 삭제 하는 데 사용 됩니다. 로컬 참조를 삭제 한 후 사용할 수 없습니다, 더 이상 충족 되도록 주의 해야 `JNIEnv.DeleteLocalRef` 로컬 참조를 사용 하 여 수행 될 마지막 항목입니다.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```



### <a name="wrapping-with-javalangobject"></a>Java.Lang.Object와 래핑

`Java.Lang.Object` 제공 된 [Java.Lang.Object (IntPtr 핸들, JniHandleOwnership 전송)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) 를 래핑하는 기존 JNI 참조를 사용할 수 있는 생성자입니다. [JniHandleOwnership](https://developer.xamarin.com/api/type/Android.Runtime.JniHandleOwnership/) 매개 변수는 확인 방법을 `IntPtr` 매개 변수를 처리 해야 합니다.

-   [JniHandleOwnership.DoNotTransfer](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.DoNotTransfer/) &ndash; 만든 `Java.Lang.Object` 인스턴스에서 새 전역에 대 한 참조를 만듭니다는 `handle` 매개 변수 및 `handle` 변경 되지 않습니다.
    호출자가 해제 해야 `handle` 필요한 경우.

-   [JniHandleOwnership.TransferLocalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; 만든 `Java.Lang.Object` 인스턴스에서 새 전역 참조 만들어집니다는 `handle` 매개 변수 및 `handle` 함께 삭제 됩니다 [JNIEnv.DeleteLocalRef ](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) . 호출자에 게 해제 하지 해야 `handle` , 사용 하지 않아야 하 고 `handle` 실행이 끝나면 생성자입니다.

-   [JniHandleOwnership.TransferGlobalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; 만든 `Java.Lang.Object` 인스턴스 소유권을 인계 합니다는 `handle` 매개 변수입니다. 호출자에 게 해제 하지 해야 `handle` 합니다.


JNI 메서드 호출 메서드에서 로컬 refs를 반환 하므로 `JniHandleOwnership.TransferLocalRef` 일반적으로 사용 됩니다.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

만들어진된 전역 참조 될 때까지 해제 되지 것입니다는 `Java.Lang.Object` 인스턴스가 가비지 수집 합니다. 가비지 수집 속도 전역 참조 수 있다면을 절약할 수 인스턴스의 삭제:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```



### <a name="using-javalangobjectgetobjectlttgt"></a>Using Java.Lang.Object.GetObject&lt;T&gt;()

`Java.Lang.Object` 제공 된 [Java.Lang.Object.GetObject&lt;T&gt;(IntPtr 핸들, JniHandleOwnership 전송)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) 메서드를 지정 된 형식의 관리 되는 호출 가능 래퍼를 만드는 데 사용할 수 있습니다.

형식 `T` 다음 요구 사항을 충족 해야 합니다.

1.  `T` 참조 형식 이어야 합니다.

1.  `T` 구현 해야 합니다는 `IJavaObject` 인터페이스입니다.

1.  경우 `T` 추상 클래스 또는 인터페이스를 다음은 `T` 매개 변수 형식 생성자를 제공 해야 `(IntPtr,
    JniHandleOwnership)` 합니다.

1.  경우 `T` 추상 클래스 또는 인터페이스에 있는 *해야* 수는 *호출자* 에 사용할 수 있는 `T` 합니다. 호출자가 상속 되는 추상이 아닌 형식을 `T` 구현 또는 `T` 와 동일한 이름을 가진 및 `T` 호출자 접미사를 사용 합니다. 예를 들어, T는 인터페이스 `Java.Lang.IRunnable` , 형식은 `Java.Lang.IRunnableInvoker` 존재 해야 하며 필요한 있어야 `(IntPtr,
    JniHandleOwnership)` 생성자입니다.


JNI 메서드 호출 메서드에서 로컬 refs를 반환 하므로 `JniHandleOwnership.TransferLocalRef` 일반적으로 사용 됩니다.

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Java 형식 조회

필드 또는 JNI에서 메서드를 조회, 필드 또는 메서드 선언 형식이 첫 번째 조회할 수 해야 합니다. [Android.Runtime.JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String)) 메서드 Java 형식을 조회 하는 데 사용 됩니다. 문자열 매개 변수는는 *형식 참조 간체* 또는 *전체 형식 참조* Java 형식에 대 한 합니다. 참조는 [JNI 형식 참조 섹션](#_JNI_Type_References) 단순화 되 고 전체 형식 참조에 대 한 세부 정보에 대 한 합니다.

참고: 달리 다른 모든 `JNIEnv` 개체 인스턴스를 반환 하는 메서드 `FindClass` 로컬 참조가 아닌 전역 참조를 반환 합니다.

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>인스턴스 필드

필드를 통해 조작 *Id 필드*합니다. 통해 필드 Id를 가져오는 [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/), 클래스를 요구 하는 필드의 이름에 정의 된 필드 및 [JNI 형식 시그니처](#JNI_Type_Signatures) 필드의 합니다.

필드 Id 해제 될 필요가 없습니다 되며으로 해당 Java 형식이 로드 되는 유효 합니다. (Android 지원 하지 않습니다 현재 클래스 언로드.)

인스턴스 필드를 조작 하기 위한 메서드의 두 집합이: 인스턴스 필드 및 인스턴스 필드를 쓰기 위한 읽기에 대 한 합니다. 메서드의 모든 집합 읽기 또는 쓰기 필드 값 필드 ID가 필요 합니다.


### <a name="reading-instance-field-values"></a>인스턴스 필드 값 읽기

인스턴스 필드 값을 읽기 위한 메서드 집합 명명 패턴을 따릅니다.

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```
여기서 `*` 유형 필드입니다.

-   [JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/) &ndash; 와 같은 기본 제공 형식이 없는 모든 인스턴스 필드의 값을 읽을 `java.lang.Object` , 배열 및 인터페이스 형식입니다. 반환 값은 JNI에 로컬 참조 합니다.

-   [JNIEnv.GetBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetBooleanField/) &ndash; 값을 읽을 `bool` 인스턴스 필드입니다.

-   [JNIEnv.GetByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetByteField/) &ndash; 값을 읽을 `sbyte` 인스턴스 필드입니다.

-   [JNIEnv.GetCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetCharField/) &ndash; 값을 읽을 `char` 인스턴스 필드입니다.

-   [JNIEnv.GetShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetShortField/) &ndash; 값을 읽을 `short` 인스턴스 필드입니다.

-   [JNIEnv.GetIntField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetIntField/) &ndash; 값을 읽을 `int` 인스턴스 필드입니다.

-   [JNIEnv.GetLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetLongField/) &ndash; 값을 읽을 `long` 인스턴스 필드입니다.

-   [JNIEnv.GetFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFloatField/) &ndash; 값을 읽을 `float` 인스턴스 필드입니다.

-   [JNIEnv.GetDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetDoubleField/) &ndash; 값을 읽을 `double` 인스턴스 필드입니다.





### <a name="writing-instance-field-values"></a>인스턴스 필드 값을 쓰는 중

인스턴스 필드 값을 쓰기 위한 메서드 집합 명명 패턴을 따릅니다.

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

여기서 *형식* 필드의 형식입니다.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash; 와 같은 기본 제공 형식이 없는 모든 필드의 값을 쓰려면 `java.lang.Object` , 배열 및 인터페이스 형식입니다. `IntPtr` JNI 로컬 참조, JNI 전역 참조, JNI 약한 전역 참조 값일 수 있습니다 또는 `IntPtr.Zero` (에 대 한 `null` ).

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash; 변수의 값을 쓸 `bool` 인스턴스 필드입니다.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash; 변수의 값을 쓸 `sbyte` 인스턴스 필드입니다.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Char)) &ndash; 변수의 값을 쓸 `char` 인스턴스 필드입니다.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash; 변수의 값을 쓸 `short` 인스턴스 필드입니다.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash; 변수의 값을 쓸 `int` 인스턴스 필드입니다.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash; 변수의 값을 쓸 `long` 인스턴스 필드입니다.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Single)) &ndash; 변수의 값을 쓸 `float` 인스턴스 필드입니다.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Double)) &ndash; 변수의 값을 쓸 `double` 인스턴스 필드입니다.


<a name="_Static_Fields" />

## <a name="static-fields"></a>정적 필드

정적 필드를 통해 조작 *Id 필드*합니다. 통해 필드 Id를 가져오는 [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFieldID/), 클래스를 요구 하는 필드의 이름에 정의 된 필드 및 [JNI 형식 시그니처](#JNI_Type_Signatures) 필드의 합니다.

필드 Id 해제 될 필요가 없습니다 되며으로 해당 Java 형식이 로드 되는 유효 합니다. (Android 지원 하지 않습니다 현재 클래스 언로드.)

두 가지 방법으로 정적 필드를 조작 하기 위한 메서드 종류가: 인스턴스 필드 및 인스턴스 필드를 쓰기 위한 읽기에 대 한 합니다. 메서드의 모든 집합 읽기 또는 쓰기 필드 값 필드 ID가 필요 합니다.


### <a name="reading-static-field-values"></a>정적 필드 값을 읽기

정적 필드 값을 읽기 위한 메서드 집합 명명 패턴을 따릅니다.

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

여기서 `*` 유형 필드입니다.

-   [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) &ndash; 와 같은 기본 제공 형식이 없는 모든 정적 필드의 값을 읽을 `java.lang.Object` , 배열 및 인터페이스 형식입니다. 반환 값은 JNI에 로컬 참조 합니다.

-   [JNIEnv.GetStaticBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticBooleanField/) &ndash; 값을 읽을 `bool` 정적 필드입니다.

-   [JNIEnv.GetStaticByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticByteField/) &ndash; 값을 읽을 `sbyte` 정적 필드입니다.

-   [JNIEnv.GetStaticCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticCharField/) &ndash; 값을 읽을 `char` 정적 필드입니다.

-   [JNIEnv.GetStaticShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticShortField/) &ndash; 값을 읽을 `short` 정적 필드입니다.

-   [JNIEnv.GetStaticLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticLongField/) &ndash; 값을 읽을 `long` 정적 필드입니다.

-   [JNIEnv.GetStaticFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFloatField/) &ndash; 값을 읽을 `float` 정적 필드입니다.

-   [JNIEnv.GetStaticDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticDoubleField/) &ndash; 값을 읽을 `double` 정적 필드입니다.



### <a name="writing-static-field-values"></a>정적 필드 값을 쓰는 중

정적 필드 값을 쓰기 위한 메서드 집합 명명 패턴을 따릅니다.

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

여기서 *형식* 필드의 형식입니다.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash; 와 같은 기본 제공 형식이 없는 모든 정적 필드의 값을 쓰려면 `java.lang.Object` , 배열 및 인터페이스 형식입니다. `IntPtr` JNI 로컬 참조, JNI 전역 참조, JNI 약한 전역 참조 값일 수 있습니다 또는 `IntPtr.Zero` (에 대 한 `null` ).

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash; 변수의 값을 쓸 `bool` 정적 필드입니다.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash; 변수의 값을 쓸 `sbyte` 정적 필드입니다.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Char)) &ndash; 변수의 값을 쓸 `char` 정적 필드입니다.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash; 변수의 값을 쓸 `short` 정적 필드입니다.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash; 변수의 값을 쓸 `int` 정적 필드입니다.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash; 변수의 값을 쓸 `long` 정적 필드입니다.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Single)) &ndash; 변수의 값을 쓸 `float` 정적 필드입니다.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Double)) &ndash; 변수의 값을 쓸 `double` 정적 필드입니다.


<a name="_Instance_Methods" />

## <a name="instance-methods"></a>인스턴스 메서드

인스턴스 메서드를 통해 호출 된 *메서드 Id*합니다. 통해 메서드 Id를 가져오는 [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/)는 형식이 필요로 하는 메서드의 이름에는 메서드가 정의 되어 있는 및 [JNI 형식 시그니처](#JNI_Type_Signatures) 메서드의 합니다.

메서드 Id 해제 될 필요가 없습니다 되며으로 해당 Java 형식이 로드 되는 유효 합니다. (Android 지원 하지 않습니다 현재 클래스 언로드.)

메서드를 호출 하는 방법의 두 집합이: 가상 메서드를 호출 하는 데 및 비가상 메서드를 호출 합니다. 두 가지 메서드는 메서드를 호출 하는 메서드 ID가 필요 하 고 가상 호출 또한 하려면을 지정 해야 어떤 클래스 구현을 호출 해야 합니다.

인터페이스 메서드만 찾을 수 있습니다.에서 선언 형식입니다. 확장/상속 된 인터페이스에서 제공 하는 방법은 찾을 수 없습니다. 나머지 바인딩 인터페이스를 참조 하십시오. 호출자 구현에 대 한 자세한 내용은 섹션 / 합니다.

클래스에서 메서드 선언 또는 모든 기본 클래스 또는 구현 된 인터페이스 찾을 수 있습니다.


### <a name="virtual-method-invocation"></a>가상 메서드 호출

메서드를 호출 하기 위한 메서드 집합은 거의 명명 패턴을 따릅니다.

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

여기서 `*` 메서드의 반환 형식입니다.

-   [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/) &ndash; 와 같은 비 내장 형식을 반환 하는 메서드를 호출 `java.lang.Object` , 배열 및 인터페이스입니다. 반환 값은 JNI에 로컬 참조 합니다.

-   [JNIEnv.CallBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallBooleanMethod/) &ndash; 반환 하는 메서드 호출을 `bool` 값입니다.

-   [JNIEnv.CallByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallByteMethod/) &ndash; 반환 하는 메서드 호출을 `sbyte` 값입니다.

-   [JNIEnv.CallCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallCharMethod/) &ndash; 반환 하는 메서드 호출을 `char` 값입니다.

-   [JNIEnv.CallShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallShortMethod/) &ndash; 반환 하는 메서드 호출을 `short` 값입니다.

-   [JNIEnv.CallLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash; 반환 하는 메서드 호출을 `long` 값입니다.

-   [JNIEnv.CallFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallFloatMethod/) &ndash; 반환 하는 메서드 호출을 `float` 값입니다.

-   [JNIEnv.CallDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallDoubleMethod/) &ndash; 반환 하는 메서드 호출을 `double` 값입니다.



### <a name="non-virtual-method-invocation"></a>비가상 메서드 호출

비가상 메서드를 호출 하기 위한 메서드 집합 명명 패턴을 따릅니다.

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

여기서 `*` 메서드의 반환 형식입니다. 비가상 메서드 호출이 가상 메서드의 기본 메서드를 호출 하려면 일반적으로 사용 됩니다.

-   [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/) &ndash; 비 거의 같은 비 내장 형식을 반환 하는 메서드를 호출할 `java.lang.Object` , 배열 및 인터페이스입니다. 반환 값은 JNI에 로컬 참조 합니다.

-   [JNIEnv.CallNonvirtualBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod/) &ndash; 비 거의 반환 하는 메서드를 호출는 `bool` 값입니다.

-   [JNIEnv.CallNonvirtualByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualByteMethod/) &ndash; 비 거의 반환 하는 메서드를 호출는 `sbyte` 값입니다.

-   [JNIEnv.CallNonvirtualCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualCharMethod/) &ndash; 비 거의 반환 하는 메서드를 호출는 `char` 값입니다.

-   [JNIEnv.CallNonvirtualShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualShortMethod/) &ndash; 비 거의 반환 하는 메서드를 호출는 `short` 값입니다.

-   [JNIEnv.CallNonvirtualLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualLongMethod/) &ndash; 비 거의 반환 하는 메서드를 호출는 `long` 값입니다.

-   [JNIEnv.CallNonvirtualFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualFloatMethod/) &ndash; 비 거의 반환 하는 메서드를 호출는 `float` 값입니다.

-   [JNIEnv.CallNonvirtualDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod/) &ndash; 비 거의 반환 하는 메서드를 호출는 `double` 값입니다.


<a name="_Static_Methods" />

## <a name="static-methods"></a>정적 메서드

정적 메서드를 통해 호출 된 *메서드 Id*합니다. 통해 메서드 Id를 가져오는 [JNIEnv.GetStaticMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/)는 형식이 필요로 하는 메서드의 이름에는 메서드가 정의 되어 있는 및 [JNI 형식 시그니처](#JNI_Type_Signatures) 메서드의 합니다.

메서드 Id 해제 될 필요가 없습니다 되며으로 해당 Java 형식이 로드 되는 유효 합니다. (Android 지원 하지 않습니다 현재 클래스 언로드.)



### <a name="static-method-invocation"></a>정적 메서드 호출

메서드를 호출 하기 위한 메서드 집합은 거의 명명 패턴을 따릅니다.

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

여기서 `*` 메서드의 반환 형식입니다.

-   [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) &ndash; 와 같은 비 내장 형식을 반환 하는 정적 메서드를 호출 `java.lang.Object` , 배열 및 인터페이스입니다. 반환 값은 JNI에 로컬 참조 합니다.

-   [JNIEnv.CallStaticBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticBooleanMethod/) &ndash; 반환 하는 정적 메서드를 호출는 `bool` 값입니다.

-   [JNIEnv.CallStaticByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticByteMethod/) &ndash; 반환 하는 정적 메서드를 호출는 `sbyte` 값입니다.

-   [JNIEnv.CallStaticCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticCharMethod/) &ndash; 반환 하는 정적 메서드를 호출는 `char` 값입니다.

-   [JNIEnv.CallStaticShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticShortMethod/) &ndash; 반환 하는 정적 메서드를 호출는 `short` 값입니다.

-   [JNIEnv.CallStaticLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash; 반환 하는 정적 메서드를 호출는 `long` 값입니다.

-   [JNIEnv.CallStaticFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticFloatMethod/) &ndash; 반환 하는 정적 메서드를 호출는 `float` 값입니다.

-   [JNIEnv.CallStaticDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticDoubleMethod/) &ndash; 반환 하는 정적 메서드를 호출는 `double` 값입니다.


<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>JNI 형식 시그니처

[JNI 형식 시그니처](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432) 는 [JNI 형식 참조](#_JNI_Type_References) (그러나 하지 간소화 된 유형 참조) 메서드를 제외 하 고 있습니다. 메서드를 사용 하 여 JNI 형식 시그니처는 여는 괄호 `'('`, 닫는 괄호 뒤에 쉼표가 없는 구분 또는 기타), (으로 함께 연결 된 형식 매개 변수 모두에 대 한 형식 참조 `')'`, 다음 메서드 반환 형식의 JNI 유형 참조 합니다.

예를 들어 다음과 같습니다. Java 메서드

```java
long f(int n, String s, int[] array);
```

JNI 형식 시그니처 것입니다.

```csharp
(ILjava/lang/String;[I)J
```

일반적으로 *강력한* 사용 하도록 권장는 `javap` JNI 서명을 확인 하는 명령입니다. 예를 들어의 JNI 형식 시그니처는 [java.lang.Thread.State.valueOf(String)](http://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) 메서드는 "(Ljava/언어/문자열;) Ljava/언어/스레드$ 상태 이면"는 JNI의 시그니처를 입력 하는 반면는 [ java.lang.Thread.State.values](http://developer.android.com/reference/java/lang/Thread.State.html#values) 메서드는 "() [Ljava/언어/스레드$ 상태 이면"입니다. 후행 세미콜론;에 대 한 주의 이러한 *는* JNI 형식 서명을 포함 합니다.

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>JNI 형식 참조

JNI 형식 참조는 Java 형식 참조와에서 다릅니다. 와 같은 정규화 된 Java 형식 이름을 사용할 수 없습니다 `java.lang.String` JNI를 대신 사용 해야 JNI 변형 `"java/lang/String"` 또는 `"Ljava/lang/String;"`, 컨텍스트에 따라 내용은 아래 참조 합니다.
네 가지 유형의 JNI 형식 참조가 있습니다.

-  **built-in**
-  **simplified**
-  **type**
-  **array**


### <a name="built-in-type-references"></a>기본 제공 형식 참조

기본 제공 형식 참조는 기본 제공 값 형식을 참조 하는 데 사용 하는 단일 문자입니다. 매핑에 다음과 같습니다.

-  `"B"` 에 대 한 `sbyte` 합니다.
-  `"S"` 에 대 한 `short` 합니다.
-  `"I"` 에 대 한 `int` 합니다.
-  `"J"` 에 대 한 `long` 합니다.
-  `"F"` 에 대 한 `float` 합니다.
-  `"D"` 에 대 한 `double` 합니다.
-  `"C"` 에 대 한 `char` 합니다.
-  `"Z"` 에 대 한 `bool` 합니다.
-  `"V"` 에 대 한 `void` 메서드 형식을 반환 합니다.


<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>간소화 된 형식 참조

간소화 된 형식 참조 에서만 사용할 수 있습니다 [JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String))합니다.
간소화 된 유형 참조를 파생 하는 방법은 두 가지가 있습니다.

1.  Java 정규화 된 이름으로 대체 모든 `'.'` 패키지 이름 및 형식 이름으로 앞 `'/'` , 매 `'.'` 형식 이름을 내 `'$'` 합니다.

1.  출력을 읽을 `'unzip -l android.jar | grep JavaName'` 합니다.


Java 형식 발생 합니다 두 [java.lang.Thread.State](http://developer.android.com/reference/java/lang/Thread.State.html) 간소화 된 유형 참조에 매핑되고 있습니다. `java/lang/Thread$State`합니다.


### <a name="type-references"></a>형식 참조

형식 참조는 기본 제공 형식 참조 또는 사용 하 여 단순화 된 유형 참조는 `'L'` 접두사와 `';'` 접미사입니다. Java 형식에 대 한 [java.lang.String](http://developer.android.com/reference/java/lang/String.html), 간소화 된 형식 참조가 `"java/lang/String"`형식 참조는 `"Ljava/lang/String;"`합니다.

형식 참조 JNI 서명을 사용 하 여 배열 형식 참조로 사용 됩니다.

형식 참조를 얻으려고 하는 또 다른 방법의 출력을 읽는 `'javap -s -classpath android.jar fully.qualified.Java.Name'`합니다.
형식에 따라 관련 있습니다 사용할 수는 생성자 선언 또는 메서드 JNI 이름을 결정 하는 형식을 반환 합니다. 예:

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

`Thread.State` Java 열거형 형식 이므로의 서명을 사용 하는 `valueOf` 메서드 형식 참조 Ljava/언어/스레드$ 상태 인지 확인할 수 있습니다.



### <a name="array-type-references"></a>배열 형식 참조

배열 형식 참조는 `'['` 맨 앞에 JNI 형식 참조를 추가 합니다.
배열을 지정할 때 간소화 된 형식 참조를 사용할 수 없습니다.

예를 들어 `int[]` 은 `"[I"`, `int[][]` 은 `"[[I"`, 및 `java.lang.Object[]` 은 `"[Ljava/lang/Object;"`합니다.



## <a name="java-generics-and-type-erasure"></a>Java 제네릭 및 형식 삭제

*대부분* 제네릭 Java JNI 차례로 살펴 보았을 때 시간의 *존재 하지 않는*합니다.
몇 가지 "주름" 하는데 이러한 주름 Java JNI을 조회 하 고 제네릭 멤버를 호출 하는 방법을와 제네릭와 상호 작용 하는 방법입니다.

JNI 통해 상호 작용할 때 제네릭 형식 또는 멤버 및 제네릭이 아닌 형식 또는 멤버 사이는 차이점이 있습니다. 예를 들어 제네릭 형식 [java.lang.Class&lt;T&gt; ](http://developer.android.com/reference/java/lang/Class.html) "원시" 제네릭 형식의 `java.lang.Class`, 한 동일한 간소화 된 유형 참조 `"java/lang/Class"`합니다.


## <a name="java-native-interface-support"></a>Java 기본 인터페이스를 지원

[Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) 은 관리 되는 래퍼에 대 한는 Jave 네이티브 인터페이스 (JNI). JNI 함수 내에서 선언 된 [Java 기본 인터페이스 사양](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)메서드는 명시적으로 제거 하도록 변경 된 경우에 `JNIEnv*` 매개 변수 및 `IntPtr` 대신 사용 됩니다 `jobject`, `jclass`, `jmethodID`등입니다. 예를 들어는 [JNI NewObject 함수](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517):

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

이 기능은로 표시 된 [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/) 메서드:

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

두 호출 간에 변환 하는 것은 비교적 간단 합니다. C는에 있습니다.

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

C#에 해당 하는 것입니다.

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

IntPtr에 보관 하는 Java 개체 인스턴스를 만든 후 합니다를 싶이 있는 것입니다. 와 같은 JNIEnv 방법을 사용할 수 있습니다 [ <span class="external">JNIEnv.CallVoidMethod()</span> ](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallVoidMethod/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/) 그러려면 되지만 경우 있는 이미 아날로그 C# 래퍼 JNI 참조에 대해 래퍼를 생성 합니다. 통해 수행할 수 있습니다는 [Extensions.JavaCast <t>()</t> ](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/) 확장 메서드:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

사용할 수도 있습니다는 [Java.Lang.Object.GetObject <t>()</t> ](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) 메서드:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

또한 모든 JNI 함수에 의해 수정 된 제거는 `JNIEnv*` 모든 JNI 함수에 매개 변수입니다.


## <a name="summary"></a>요약

JNI 직접 처리 해서든 피해 야 하는 테러 경험 하는 합니다. 그러나 항상 것은 아닙니다 피할 수 있습니다. 다행히이 가이드 Android 용 모노도 언바운드 Java 사례에 도달 하면 필요한 일부 정보가 제공 합니다.


## <a name="related-links"></a>관련 링크

- [SanityTests (샘플)](https://developer.xamarin.com/samples/SanityTests/)
- [Java 기본 인터페이스 사양](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java 기본 인터페이스 함수](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
