---
title: Xamarin.ios 및 데스크톱-Mono 런타임의 차이점
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 57d9d6a91f88d117f0889a8dba9e6198ec6b7f62
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524775"
---
# <a name="limitations"></a>제한 사항

Android의 응용 프로그램은 빌드 프로세스 중에 Java 프록시 형식을 생성 해야 하므로 런타임에 모든 코드를 생성할 수는 없습니다.

다음은 데스크톱 Mono와 비교 했을 때 Xamarin.ios의 제한 사항입니다.

## <a name="limited-dynamic-language-support"></a>제한 된 동적 언어 지원

 Android 런타임에 관리 코드를 호출 해야 하는 경우 언제 든 지 [android 호출 가능 래퍼가](~/android/platform/java-integration/android-callable-wrappers.md) 필요 합니다. Android 호출 가능 래퍼는 IL의 정적 분석을 기반으로 컴파일 타임에 생성 됩니다. 이에 대 한 순 결과: 컴파일 타임에 이러한 동적 형식을 추출할 수 있는 방법이 없기 때문에 Java 형식의 하위 클래스를 포함 하는 모든 시나리오에서 동적 언어 (IronPython, IronRuby 등)를 사용할 *수 없습니다* . 필요한 Android 호출 가능 래퍼를 생성 합니다.

## <a name="limited-java-generation-support"></a>제한 된 Java Generation 지원

Java 코드에서 관리 코드를 호출 하려면 [Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md) 를 생성 해야 합니다. *기본적으로*Android 호출 가능 래퍼는 (특정) 선언 된 생성자와 메서드만 포함 합니다. [`RegisterAttribute`](xref:Android.Runtime.RegisterAttribute)이 메서드는 가상 java 메서드를 재정의 하거나 (예:), Java 인터페이스 메서드를 구현 합니다. `Attribute`(인터페이스와 마찬가지로)
  
4\.1 릴리스 전에는 추가 메서드를 선언할 수 없습니다. 4\.1 릴리스를 [ `Export` 사용 하 여 및 `ExportField` 사용자 지정 특성을 사용 하 여 Android 호출 가능 래퍼 내에서 Java 메서드 및 필드를 선언할 수 있습니다](~/android/platform/java-integration/working-with-jni.md).

### <a name="missing-constructors"></a>누락 된 생성자

생성자는를 사용 하지 [`ExportAttribute`](xref:Java.Interop.ExportAttribute) 않는 한 복잡 하 게 유지 됩니다. Android 호출 가능 래퍼 생성자를 생성 하는 알고리즘은 다음과 같은 경우에 Java 생성자를 내보내는 것입니다.

1. 모든 매개 변수 형식에 대 한 Java 매핑이 있습니다.
2. Android 호출 가능 래퍼가 해당 하는 &ndash; 기본 클래스 생성자를 호출 *해야* 하기 때문에 기본 클래스는 동일한 생성자를 선언 합니다. 기본 인수를 사용할 수 없습니다 .이 경우에는 값을 쉽게 확인할 수 있습니다. Java 내에서 사용 해야 함).

예를 들어 다음 클래스를 예로 들어 볼 수 있습니다.

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

이는 완벽 하 게 논리적으로 보이지만 *릴리스 빌드에서* 생성 된 Android 호출 가능 래퍼는 기본 생성자를 포함 하지 않습니다. 따라서이 서비스 [`Context.StartService`](xref:Android.Content.Context.StartService*)를 시작 하려고 하면 실패 하 게 됩니다.

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

해결 방법은 기본 생성자를 선언 하 고 `ExportAttribute`, 장식할로 [`ExportAttribute.SuperStringArgument`](xref:Java.Interop.ExportAttribute.SuperArgumentsString)설정 하 고,를 설정 하는 것입니다. 

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

제네릭 C# 클래스는 부분적 으로만 지원 됩니다. 다음과 같은 제한 사항이 있습니다.


- 제네릭 형식은 또는를 `[Export]` `[ExportField`사용할 수 없습니다. 이렇게 하려고 하면 `XA4207` 오류가 생성 됩니다.

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

- 제네릭 메서드는 또는 `[Export]` `[ExportField]`를 사용할 수 없습니다.

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

- `[ExportField]`다음을 반환 `void`하는 메서드에서는 사용할 수 없습니다.

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

- Java 코드에서 제네릭 형식의 인스턴스를 만들지 _않아야_ 합니다.
    관리 코드 에서만 안전 하 게 만들 수 있습니다.

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

Java 제네릭 바인딩 지원은 제한 됩니다. 특히, 다른 제네릭 (인스턴스화되지 않은) 클래스에서 파생 된 제네릭 인스턴스 클래스의 멤버는 Java. Lang로 노출 됩니다. 예를 들어 [GetParcelableExtra](xref:Android.Content.Intent.GetParcelableExtra*) 메서드는 Java. Lang을 반환 합니다. 이는 삭제 된 Java 제네릭을 발생 하기 때문입니다.
이러한 제한을 적용 하지 않는 일부 클래스가 있지만 수동으로 조정 됩니다.

## <a name="related-links"></a>관련 링크

- [Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md)
- [JNI 사용](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](xref:Java.Interop.ExportAttribute)
- [SuperString](xref:Java.Interop.ExportAttribute.SuperArgumentsString)
- [RegisterAttribute](xref:Android.Runtime.RegisterAttribute)
