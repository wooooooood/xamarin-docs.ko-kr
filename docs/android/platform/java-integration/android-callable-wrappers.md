---
title: Android 호출 가능 래퍼
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: 7edbdaa5a690a641523cb5baad7909ed01992aa5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121817"
---
# <a name="android-callable-wrappers"></a>Android 호출 가능 래퍼

Android 호출 가능 래퍼 (ACWs)는 관리 되는 코드를 호출 하는 Android 런타임 때마다 필요 합니다. 이러한 래퍼는 런타임에 아트 (Android 런타임)을 사용 하 여 클래스를 등록할 수 없기 때문에 필요 합니다. (특히 합니다 [JNI DefineClass() 함수](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986) Android 런타임에서 지원 되지 않습니다.} Android 호출 가능 래퍼는 따라서 런타임 형식 등록 지원 부족을 확인합니다. 

*때마다* Android 코드를 실행 해야 하는 `virtual` 인터페이스는 메서드 또는 `overridden` 관리 되는 코드에서 구현 Xamarin.Android 프록시를 제공 해야 Java이이 메서드는 적절 한 관리 되는 형식에 디스패치 됩니다 있도록 또는 합니다. 이러한 Java 프록시 형식은 관리 되는 유형 같은 생성자를 구현 하 고 재정의 된 기본 클래스 및 인터페이스 메서드 선언으로 "동일" 기본 클래스 및 Java 인터페이스 목록에 있는 Java 코드입니다. 

Android 호출 가능 래퍼에서 생성 되는 **monodroid.exe** 하는 동안 프로그램 합니다 [빌드 프로세스](~/android/deploy-test/building-apps/build-process.md): 상속 (직접 또는 간접적으로) 하는 모든 형식에 대해 생성 됩니다 [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/)합니다. 



## <a name="android-callable-wrapper-naming"></a>Android 호출 가능 래퍼 이름 지정

Android 호출 가능 래퍼에 대 한 패키지 이름은 내보내는 형식의 어셈블리 정규화 된 이름의 MD5SUM를 기반으로 합니다. 이 명명 기법을 사용 하면 패키징 오류를 유발 하지 않고도 다른 어셈블리에서 사용할 수 있게 하려면 동일한 정규화 된 형식 이름에 대 한 수 있습니다. 

MD5SUM 명명 체계가이 인해 형식 이름으로 직접 액세스할 수 없습니다. 예를 들어, 다음 `adb` 때문에 명령이 작동 형식 이름을 `my.ActivityType` 기본적으로 생성 되지 않습니다. 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

또한 형식 이름별로 참조 하려고 하면 다음과 같은 오류가 나타날 수 있습니다.

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

경우 있습니다 *수행* 액세스가 필요한 이름별 형식에 특성 선언에 해당 형식에 대 한 이름을 선언할 수 있습니다. 예를 들어, 다음은 정규화 된 이름 사용 하 여 작업을 선언 하는 코드 `My.ActivityType`:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

`ActivityAttribute.Name` 이 활동의 이름을 명시적으로 선언 하려면 속성을 설정할 수 있습니다. 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

이 속성 설정은 추가한 후 `my.ActivityType` 외부 코드에서 이름으로 액세스할 수 있습니다 `adb` 스크립트입니다. 합니다 `Name` 비롯 한 많은 다른 형식에 특성을 설정할 수 있습니다 `Activity`를 `Application`를 `Service`를 `BroadcastReceiver`, 및 `ContentProvider`: 

-   [ActivityAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/)
-   [ApplicationAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Name/)
-   [ServiceAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ServiceAttribute.Name/)
-   [BroadcastReceiverAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.BroadcastReceiverAttribute.Name/)
-   [ContentProviderAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.ContentProviderAttribute.Name/)

ACW 명명 MD5SUM 기반 Xamarin.Android 5.0에서 도입 되었습니다. 특성 이름을 지정 하는 방법에 대 한 자세한 내용은 참조 하세요. [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)합니다. 



## <a name="implementing-interfaces"></a>인터페이스 구현

와 같은 Android 인터페이스를 구현 해야 하는 경우가 있습니다 [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/)합니다. 모든 Android 클래스 및 인터페이스를 확장 하므로 합니다 [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) 인터페이스를 질문 발생:에서는 구현지 않습니다 `IJavaObject`? 

위의 질문에 답변은: 이유 모든 Android 형식을 구현 해야 `IJavaObject` Xamarin.Android에는 Android, 즉, 지정된 된 형식에 대 한 Java 프록시를 제공 하는 Android 호출 가능 래퍼에 있도록 합니다. 이후 **monodroid.exe** 만 찾습니다 `Java.Lang.Object` 하위 클래스와 `Java.Lang.Object` 구현 `IJavaObject,` 대답이 명확: 하위 클래스입니다 `Java.Lang.Object`: 

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

*이 페이지의 나머지 부분에서는 예 고 없이 변경 될 수 있습니다 구현 세부 정보를 제공* (및 개발자는 것에 대해 궁금한 수 때문에 여기 표시 됩니다). 

예를 들어, 다음 지정 된 C# 원본:

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

합니다 **mandroid.exe** 프로그램 다음 Android 호출 가능 래퍼를 생성 합니다. 

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

기본 클래스 유지 되 면 확인 및 `native` 관리 코드 내에서 재정의 되는 각 메서드에 대 한 메서드 선언을 제공 됩니다. 
