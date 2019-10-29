---
title: Xamarin android 용 android 호출 가능 래퍼
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 7278fd624bb3147c2e1a1a1a79adde68813a9888
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020161"
---
# <a name="android-callable-wrappers-for-xamarinandroid"></a>Xamarin android 용 android 호출 가능 래퍼

Android 런타임에서 관리 코드를 호출할 때마다 ACWs (android 호출 가능 래퍼)가 필요 합니다. 이러한 래퍼는 런타임에 클래스 (Android 런타임)를 사용 하 여 클래스를 등록할 방법이 없기 때문에 필요 합니다. 특히 [JNI 정의 eclass () 함수](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986) 는 Android 런타임에서 지원 되지 않습니다.} Android 호출 가능 래퍼를 통해 런타임 형식 등록을 지원 하지 않습니다. 

*매번* Android 코드는 관리 코드에서 `overridden` 또는 구현 되는 `virtual` 또는 인터페이스 메서드를 실행 해야 합니다. Xamarin. Android는이 메서드가 적절 한 관리 되는 형식으로 디스패치 되도록 Java 프록시를 제공 해야 합니다. 이러한 Java 프록시 형식은 동일한 생성자를 구현 하 고 재정의 된 기본 클래스 및 인터페이스 메서드를 선언 하는 "동일한" 기본 클래스 및 Java 인터페이스 목록을 관리 되는 형식으로 포함 하는 Java 코드입니다. 

Android 호출 가능 래퍼는 [빌드 프로세스](~/android/deploy-test/building-apps/build-process.md)중에 **monodroid** 프로그램에 의해 생성 됩니다. (직접 또는 간접적으로)이 [를 상속 하](xref:Java.Lang.Object)는 모든 형식에 대해 생성 됩니다. 

## <a name="android-callable-wrapper-naming"></a>Android 호출 가능 래퍼 이름 지정

Android 호출 가능 래퍼의 패키지 이름은 내보내는 형식의 어셈블리 정규화 된 이름에 대 한 MD5SUM을 기반으로 합니다. 이 명명 기법을 사용 하면 패키징 오류를 일으키지 않고 다른 어셈블리에서 동일한 정규화 된 형식 이름을 사용할 수 있습니다. 

이 MD5SUM 명명 스키마 때문에 이름으로 형식에 직접 액세스할 수 없습니다. 예를 들어 형식 이름 `my.ActivityType` 기본적으로 생성 되지 않으므로 다음 `adb` 명령은 작동 하지 않습니다. 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

또한 이름을 기준으로 형식을 참조 하려고 하면 다음과 같은 오류가 표시 될 수 있습니다.

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

이름으로 형식에 액세스 해야 하는 경우 특성 선언에서 해당 형식의 이름을 선언할 *수 있습니다.* 예를 들어 다음은 `My.ActivityType`정규화 된 이름을 사용 하 여 작업을 선언 하는 코드입니다.

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

`ActivityAttribute.Name` 속성을 설정 하 여이 활동의 이름을 명시적으로 선언할 수 있습니다. 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

이 속성 설정이 추가 된 후에는 외부 코드 및 `adb` 스크립트에서 이름으로 `my.ActivityType` 액세스할 수 있습니다. `Name` 특성은 `Activity`, `Application`, `Service`, `BroadcastReceiver`및 `ContentProvider`를 비롯 한 다양 한 형식에 대해 설정할 수 있습니다. 

- [ActivityAttribute.Name](xref:Android.App.ActivityAttribute.Name)
- [ApplicationAttribute.Name](xref:Android.App.ApplicationAttribute.Name)
- [ServiceAttribute.Name](xref:Android.App.ServiceAttribute.Name)
- [BroadcastReceiverAttribute.Name](xref:Android.Content.BroadcastReceiverAttribute.Name)
- [ContentProviderAttribute.Name](xref:Android.Content.ContentProviderAttribute.Name)

MD5SUM 기반 ACW 이름은 Xamarin. Android 5.0에서 도입 되었습니다. 특성 명명에 대 한 자세한 내용은 [Registerattribute](xref:Android.Runtime.RegisterAttribute)를 참조 하십시오. 

## <a name="implementing-interfaces"></a>인터페이스 구현

Android 인터페이스 (예: [IComponentCallbacks](xref:Android.Content.IComponentCallbacks))를 구현 해야 하는 경우가 있습니다. 모든 Android 클래스와 인터페이스가 [IJavaObject](xref:Android.Runtime.IJavaObject) 인터페이스를 확장 하므로 질문은 어떻게 `IJavaObject`구현 하나요? 

위의 질문에 답변 했습니다. 모든 Android 형식이 `IJavaObject`를 구현 해야 하는 이유는 android에 제공할 Android 호출 가능 래퍼 (즉, 지정 된 형식에 대 한 Java 프록시)가 있는 것입니다. **Monodroid** 는 `Java.Lang.Object` 하위 클래스를 검색 하 고 `Java.Lang.Object` 구현 `IJavaObject,`를 구현 하므로, 하위 클래스 `Java.Lang.Object`입니다. 

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

*이 페이지의 나머지 부분에서는 예 고 없이 변경 될 수 있는 구현 세부 정보를 제공* 합니다. 개발자는이에 대 한 자세한 정보를 제공 합니다. 

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

기본 클래스는 유지 되 고 관리 코드 내에서 재정의 되는 각 메서드에 대해 `native` 메서드 선언이 제공 됩니다. 
