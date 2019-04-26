---
title: Xamarin.Android vs. Desktop-Mono 런타임에서 차이점
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 115d715214d7af3174c41d9d82e894ce429dab42
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60953346"
---
# <a name="limitations"></a>제한 사항

Android에서 응용 프로그램 Java 프록시 형식을 생성 하는 빌드 프로세스 중에 해야 하므로 런타임 시 모든 코드를 생성 하는 것이 불가능 합니다.

다음은 Mono 데스크톱에 비해 Xamarin.Android 제한 사항입니다.


## <a name="limited-dynamic-language-support"></a>제한 된 동적 언어 지원

 [Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md) Android 런타임에서 관리 코드를 호출 해야 할 때마다 필요 합니다. Android 호출 가능 래퍼 IL의 정적 분석을 기반으로 하는 컴파일 타임에 생성 됩니다. 이 결과: 있습니다 *없습니다* 사용 하 여 동적 언어 (IronPython, IronRuby 등) 모든 시나리오에서 Java 형식 하위 클래스 (간접 서브클래싱)을 포함 하 여 필요한 경우 이러한 동적 형식을 추출 방법이 없기 때문에 컴파일 타임에 필요한 Android 호출 가능 래퍼를 생성 합니다.


## <a name="limited-java-generation-support"></a>제한 된 Java 생성이 지원

[Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md) 관리 코드를 호출 하는 Java 코드에서 생성 해야 합니다. *기본적으로*, Android 호출 가능 래퍼 (특정) 선언 된 생성자 및 가상 Java 메서드를 재정의 하는 메서드가 포함 됩니다 (즉, 있기 [ `RegisterAttribute` ](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)) 구현 Java 인터페이스 메서드 (또는 인터페이스에는 마찬가지로 `Attribute`).
  
4.1 릴리스 전에 추가 메서드가 없는 선언할 수 없습니다. 4.1 릴리스에 [는 `Export` 하 고 `ExportField` Java 메서드 및 Android 호출 가능 래퍼를 내의 필드를 선언 하려면 사용자 지정 특성을 사용할 수](~/android/platform/java-integration/working-with-jni.md).

### <a name="missing-constructors"></a>누락 된 생성자

생성자는 작업은 복잡할 상태로 유지 하지 않는 한 [ `ExportAttribute` ](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute) 사용 됩니다. Android callable wrapper 생성자 생성 알고리즘은 경우 Java 생성자를 내보냅니다.

1. 모든 매개 변수 형식에 대 한 Java 매핑이 있는
2. 동일한 생성자를 선언 하는 기본 클래스 &ndash; 때문에 이것이 필요 Android 호출 가능 래퍼 *해야* 해당 기본 클래스 생성자를 호출, (쉽게 수 없기 때문에 기본 인수를 사용할 수 값을 결정 내 Java에서 사용).

예를 들어 다음 클래스를 예로 들어 볼 수 있습니다.

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

하지만이 같습니다 완벽 하 게 논리 결과 Android 호출 가능 래퍼 *릴리스 빌드에서* 기본 생성자가 포함 되지 것입니다. 따라서이 서비스를 시작 하려는 경우 (예: [ `Context.StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)), 실패:

```shell
E/AndroidRuntime(31766): FATAL EXCEPTION: main
E/AndroidRuntime(31766): java.lang.RuntimeException: Unable to instantiate service example.MyIntentService: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2347)
E/AndroidRuntime(31766):        at android.app.ActivityThread.access$1600(ActivityThread.java:130)
E/AndroidRuntime(31766):        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1277)
E/AndroidRuntime(31766):        at android.os.Handler.dispatchMessage(Handler.java:99)
E/AndroidRuntime(31766):        at android.os.Looper.loop(Looper.java:137)
E/AndroidRuntime(31766):        at android.app.ActivityThread.main(ActivityThread.java:4745)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invokeNative(Native Method)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invoke(Method.java:511)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:786)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:553)
E/AndroidRuntime(31766):        at dalvik.system.NativeStart.main(Native Method)
E/AndroidRuntime(31766): Caused by: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at java.lang.Class.newInstanceImpl(Native Method)
E/AndroidRuntime(31766):        at java.lang.Class.newInstance(Class.java:1319)
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2344)
E/AndroidRuntime(31766):        ... 10 more
```

이 기본 생성자를 선언, 사용 하 여 표시 하는 `ExportAttribute`, 설정 및 합니다 [ `ExportAttribute.SuperStringArgument` ](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/): 

```csharp
[Service]
class MyIntentService : IntentService {
    [Export (SuperArgumentsString = "\"value\"")]
    public MyIntentService (): base("value")
    {
    }

    // ...
}
```


### <a name="generic-c-classes"></a>제네릭 C# 클래스

제네릭 C# 클래스 부분적 으로만 지원 됩니다. 다음 제한 사항이 있습니다.


-   제네릭 형식을 사용할 수 없습니다 `[Export]` 또는 `[ExportField`]. 생성 작업을 수행 하는 동안는 `XA4207` 오류입니다.

    ```csharp
    public abstract class Parcelable<T> : Java.Lang.Object, IParcelable
    {
        // Invalid; generates XA4207
        [ExportField ("CREATOR")]
        public static IParcelableCreator CreateCreator ()
        {
            ...
    }
    ```

-   제네릭 메서드를 사용할 수 없습니다 `[Export]` 또는 `[ExportField]`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        
        // Invalid; generates XA4207
        [Export]
        public static void Method<T>(T value)
        {
            ...
        }
    }
    ```

-   `[ExportField]` 반환 하는 메서드에서만 사용할 수 없습니다 `void`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        // Invalid; generates XA4208
        [ExportField ("CREATOR")]
        public static void CreateSomething ()
        {
        }
    }
    ```

-   제네릭 형식의 인스턴스 _안_ Java 코드에서 만들 수 있습니다.
    만 안전 하 게 관리 되는 코드에서 만들 수 있습니다.

    ```csharp
    [Activity (Label="Die!", MainLauncher=true)]
    public class BadGenericActivity<T> : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);
        }
    }
    ```


## <a name="partial-java-generics-support"></a>부분 Java 제네릭 지원

Java 바인딩 제네릭을 지원 제한 됩니다. 다른 제네릭 (비 인스턴스화된) 클래스에서 파생 되는 제네릭 인스턴스 클래스의 멤버는 왼쪽 특히 Java.Lang.Object으로 노출 합니다. 예를 들어 [Android.Content.Intent.GetParcelableExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.GetParcelableExtra/p/System.String/) Java.Lang.Object 메서드를 반환 합니다. 지워진된 Java 제네릭 때문입니다.
이 제한이 적용 되지 않는 일부 클래스 있지만 수동으로 조정 됩니다.


## <a name="related-links"></a>관련 링크

- [Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md)
- [Jni 작업](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)
- [SuperString](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/)
- [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)
