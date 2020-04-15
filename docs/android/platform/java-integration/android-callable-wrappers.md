---
title: Xamarin.Android용 Android 호출 가능 래퍼
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 7278fd624bb3147c2e1a1a1a79adde68813a9888
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020161"
---
# <a name="android-callable-wrappers-for-xamarinandroid"></a>Xamarin.Android용 Android 호출 가능 래퍼

Android 런타임에서 관리 코드를 호출할 때마다 ACW(Android 호출 가능 래퍼)가 필요합니다. 이러한 래퍼는 런타임 시 클래스를 ART(Android 런타임)로 등록할 방법이 없기 때문에 필요합니다. (특히 [JNI DefineClass() 함수](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986)는 Android 런타임에서 지원되지 않습니다.} 따라서 Android 호출 가능 래퍼는 런타임 형식 등록 지원이 부족합니다. 

Android 코드에서 관리 코드에 `overridden`되거나 구현되는 `virtual` 또는 인터페이스 메서드를 실행해야 할 *때마다* Xamarin.Android는 이 메서드가 적절한 관리 형식으로 디스패치되도록 Java 프록시를 제공해야 합니다. 이러한 Java 프록시 형식은 관리 형식과 "동일한" 기본 클래스 및 Java 인터페이스 목록이 있는 Java 코드로, 동일한 생성자를 구현하고, 재정의된 기본 클래스 및 인터페이스 메서드를 선언합니다. 

Android 호출 가능 래퍼는 [빌드 프로세스](~/android/deploy-test/building-apps/build-process.md) 중에 **monodroid.exe** 프로그램에서 생성되며, (직접 또는 간접적으로) [Java.Lang.Object](xref:Java.Lang.Object)를 상속하는 모든 형식에 대해 생성됩니다. 

## <a name="android-callable-wrapper-naming"></a>Android 호출 가능 래퍼 명명

Android 호출 가능 래퍼의 패키지 이름은 내보낼 형식의 정규화된 어셈블리 이름의 MD5SUM을 기반으로 합니다. 이 명명 기법을 사용하면 패키징 오류를 일으키지 않고 다른 어셈블리에서 동일한 정규화된 형식 이름을 사용할 수 있습니다. 

이 MD5SUM 명명 스키마 때문에 이름으로 형식에 직접 액세스할 수 없습니다. 예를 들어 형식 이름 `my.ActivityType`은 기본적으로 생성되지 않으므로 다음 `adb` 명령은 작동하지 않습니다. 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

또한 이름을 기준으로 형식을 참조하려고 하면 다음과 같은 오류가 표시될 수 있습니다.

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

이름으로 형식에 액세스 해야 *하는* 경우 특성 선언에서 해당 형식의 이름을 선언할 수 있습니다. 예를 들어 다음은 정규화된 이름을 사용하여 작업을 선언하는 코드입니다. `My.ActivityType`:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

`ActivityAttribute.Name` 속성을 설정하여 이 작업의 이름을 명시적으로 선언할 수 있습니다. 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

이 속성 설정이 추가된 후에는 외부 코드 및 `adb` 스크립트에서 이름으로 `my.ActivityType`에 액세스할 수 있습니다. `Name` 특성은 `Activity`, `Application`, `Service`, `BroadcastReceiver` 및 `ContentProvider`를 비롯한 다양한 형식에 대해 설정할 수 있습니다. 

- [ActivityAttribute.Name](xref:Android.App.ActivityAttribute.Name)
- [ApplicationAttribute.Name](xref:Android.App.ApplicationAttribute.Name)
- [ServiceAttribute.Name](xref:Android.App.ServiceAttribute.Name)
- [BroadcastReceiverAttribute.Name](xref:Android.Content.BroadcastReceiverAttribute.Name)
- [ContentProviderAttribute.Name](xref:Android.Content.ContentProviderAttribute.Name)

MD5SUM 기반 ACW 명명은 Xamarin.Android 5.0에서 도입되었습니다. 특성 명명에 대한 자세한 내용은 [RegisterAttribute](xref:Android.Runtime.RegisterAttribute)를 참조하세요. 

## <a name="implementing-interfaces"></a>인터페이스 구현

Android 인터페이스(예제: [Android.Content.IComponentCallbacks](xref:Android.Content.IComponentCallbacks))를 구현해야 하는 경우가 있습니다. 모든 Android 클래스 및 인터페이스는 [Android.Runtime.IJavaObject](xref:Android.Runtime.IJavaObject) 인터페이스를 확장하므로 `IJavaObject`를 구현하는 방법에 대한 질문이 발생합니다. 

위의 질문에 답변했습니다. 모든 Android 형식이 `IJavaObject`를 구현해야 하는 이유는 Xamarin.Android에는 Android에 제공할 Android 호출 가능 래퍼(즉, 지정된 형식에 대한 Java 프록시)가 있기 때문입니다. **monodroid.exe**는 `Java.Lang.Object` 서브클래스만 찾고 `Java.Lang.Object`는 `IJavaObject,`를 구현하므로 대답은 명백하게 서브클래스 `Java.Lang.Object`입니다. 

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {

    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
    {
        // implementation goes here...
    } 

    public void OnLowMemory ()
    {
        // implementation goes here...
    }
}
```

## <a name="implementation-details"></a>구현 세부 정보

*이 페이지의 나머지 부분에서는 예고 없이 변경될 수 있는 구현 세부 정보를 제공하며* 개발자는 진행 상황에 대해 궁금해 하므로 여기에서만 제공됩니다. 

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

public class HelloAndroid
    extends android.app.Activity
{
    static final String __md_methods;
    static {
        __md_methods = "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" + "";
        mono.android.Runtime.register (
            "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
            Culture=neutral, PublicKeyToken=null", HelloAndroid.class, __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
                Culture=neutral, PublicKeyToken=null", "", this, new java.lang.Object[] {  });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

기본 클래스는 유지되고, 관리 코드 내에서 재정의되는 각 메서드에 대해 `native` 메서드 선언이 제공됩니다. 
