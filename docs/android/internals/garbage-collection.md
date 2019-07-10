---
title: 가비지 컬렉션
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: 09466cc9eed4899ef0aa1198ff0aee5cd420e110
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67674629"
---
# <a name="garbage-collection"></a>가비지 컬렉션

Mono를 사용 하는 Xamarin.Android [간단한 세대 가비지 수집기](https://www.mono-project.com/docs/advanced/garbage-collector/sgen/)합니다. 이 두 세대를 사용 하 여 표시 및 스윕 가비지 수집기와 *큰 개체 공간*, 두 종류의 컬렉션을 사용 하 여: 

-   부 컬렉션 (Gen0 수집 힙) 
-   주요 컬렉션 (Gen1 수집 하 고 큰 개체 공간 힙)입니다. 

> [!NOTE]
> 통해 명시적 컬렉션을 없으면 [GC 합니다. Collect ()](xref:System.GC.Collect) 컬렉션이 *주문형*힙 할당에 따라, 합니다. *시스템을 계산 하는 참조가 아닙니다.* ; 개체 *미해결 참조가 없으면 즉시 수집 되지 것입니다.* , 범위 않았거나 종료 하는 경우. GC는 부 힙이 새 할당을 위해 메모리에서 실행 될 때 실행 됩니다. 할당이 없는 경우 실행 되지 않습니다.


부 컬렉션 저렴 하 고, 자주 수행 되며 최근에 할당 되 고 비활성 개체를 수집 하는 데 사용 됩니다. 부 컬렉션은 할당 된 개체의 모든 몇 MB 후 수행 됩니다. 부 컬렉션을 호출 하 여 수동으로 수행할 수 있습니다 [GC 합니다. (0)를 수집 합니다.](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

주요 비용이 높고 덜 빈번 컬렉션과 모든 비활성 개체를 회수 하는 데 사용 됩니다. 주요 컬렉션 (힙 크기 조정) 이전 현재 힙 크기에 대 한 메모리 소모 되 면 수행 됩니다. 주요 컬렉션을 호출 하 여 수동으로 수행할 수 있습니다 [GC 합니다. ()를 수집](xref:System.GC.Collect) 를 호출 하거나 [GC 합니다. (Int)를 수집](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 인수를 사용 하 여 [GC 합니다. MaxGeneration](xref:System.GC.MaxGeneration)합니다. 



## <a name="cross-vm-object-collections"></a>VM 간 개체 컬렉션

개체 유형의 세 가지 범주가 있습니다.

-   **관리 되는 개체**: 형식 개라면 *하지* 에서 상속 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) 예 [System.String](xref:System.String)합니다. 
    이러한 작업은 GC가 정상적으로 수집 됩니다. 

-   **Java 개체**: Android 런타임 VM 내에서 있지만 Mono VM에 노출 되지 않습니다는 Java 형식입니다. 이러한 따분한 되며 더 이상 설명 하지 않습니다. 이러한 작업은 Android 런타임 VM에서 일반적으로 수집 됩니다. 

-   **개체를 피어 링**: 구현 하는 형식 [IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) , 모든 예를 들어 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) 및 [Java.Lang.Throwable](https://developer.xamarin.com/api/type/Java.Lang.Throwable/) 하위 클래스입니다. 이러한 형식의 인스턴스는 두 개의 "halfs"를 *관리 되는 피어* 와 *네이티브 피어*합니다. 관리 되는 피어가의 인스턴스는 C# 클래스입니다. 기본 피어가 Android 런타임 VM 내에서 Java 클래스의 인스턴스 및 C# [IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) 속성에는 기본 피어 JNI 전역 참조를 포함 합니다. 


기본 피어는 다음과 같은 두 종류가 있습니다.

-   **Framework 피어** : 예를 들어 Xamarin.Android의 아무 것도 알고 있는 "일반" Java 형식 [android.content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)합니다.

-   **사용자 피어** : [Android 호출 가능 래퍼](~/android/platform/java-integration/working-with-jni.md) 응용 프로그램 내에 있는 각 Java.Lang.Object 하위 클래스에 대 한 빌드 시 생성 되는 합니다.


Xamarin.Android 프로세스 내에서 두 개의 Vm으로는 두 가지 유형의 가비지 컬렉션

-   Android 런타임 컬렉션 
-   Mono 컬렉션 

Android 런타임 컬렉션에 주의 해야 하지만 일반적으로 작동: JNI 전역 참조 GC 루트로 처리 됩니다. 따라서 경우는 JNI 전역 참조는 Android 런타임 VM 개체를 개체 점유 *없습니다* 컬렉션에 대 한 적격 라인인 경우에 수집 합니다.

Mono 컬렉션은 재미 이뤄지는 위치입니다. 관리 되는 개체는 일반적으로 수집 됩니다. 피어 개체는 다음 프로세스를 수행 하 여 수집 됩니다.

1.  Mono 컬렉션에 대 한 적합 한 모든 피어 개체는 JNI 약한 전역 참조로 대체 하는 JNI 전역 참조를 가집니다. 

2.  Android 런타임 VM GC가 호출 됩니다. 기본 피어 인스턴스에도 수집 될 수 있습니다. 

3.  JNI 약한 전역 참조 (1)에서 만든 확인 됩니다. 약한 참조를 수집 하는 경우 피어 개체 수집 됩니다. 약한 참조에 있으면 *되지* 수집 된 다음 weak reference는 JNI 전역 참조를 사용 하 여 바뀌고 피어 개체 수집 되지 않습니다. 참고: API 14 +에서 즉에서 반환 되는 값 `IJavaObject.Handle` GC 후 변경 될 수 있습니다. 

이 피어 개체의 인스턴스 중 하나에서 참조 되 고으로 유지 되는 모든의 최종 결과 관리 코드 (에 저장 예:는 `static` 변수) 또는 Java 코드에서 참조 합니다. 네이티브 피어의 수명을 초과 어떤 것이 고, 그렇지 확장 될 것 또한 라이브, 네이티브 피어와 관리 되는 피어 수집 될 때까지 기본 피어 수집 가능한 수 없습니다.


## <a name="object-cycles"></a>개체 주기

피어 개체는 Android 런타임 및 Mono VM 내에 논리적으로 존재 합니다. 예를 들어를 [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) 관리 되는 피어 인스턴스가 해당 갖습니다 [android.app.Activity](https://developer.android.com/reference/android/app/Activity.html) framework 피어 Java 인스턴스. 상속 되는 모든 개체 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) 두 Vm 내에서 표현이를 예상할 수 있습니다. 

두 Vm에서 표현이 있는 모든 개체는 수명이 단일 VM 내 에서만 표시 되는 개체를 비교 하 여 확장 (같은 [ `System.Collections.Generic.List<int>` ](xref:System.Collections.Generic.List%601)). 호출 [GC 합니다. 수집](xref:System.GC.Collect) Xamarin.Android GC 수집 하기 전에 VM 중 하나에서 개체 참조 되지 않으면 확인 하기 때문에 반드시 이러한 개체를 수집 하지 않습니다. 

개체 수명 줄이려면 [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) 호출 해야 합니다. 이 수동으로 "서버" 개체를 더 빠르게 수집할 수 있으므로 전역 참조를 해제 하 여 두 Vm 간에 개체에 연결 합니다. 


## <a name="automatic-collections"></a>자동 컬렉션

부터는 [릴리스 4.1.0](https://developer.xamarin.com/releases/android/mono_for_android_4/mono_for_android_4.1.0), gref 임계값이 초과 하는 경우 자동으로 Xamarin.Android 전체 GC를 수행 합니다. 이 임계값은 90%를 플랫폼에 대해 알려진된 최대 grefs: (최대 2000) 에뮬레이터에서 1800 grefs 및 46800 grefs 하드웨어 (최대 52000). *참고:* Xamarin.Android에서 만든 grefs 계산 [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/), 및 프로세스에서 만든 다른 grefs에 대 한 알 수 없습니다. 이 경험적 접근 *만*합니다. 

자동 컬렉션을 수행 하는 경우 디버그 로그에 다음과 비슷한 메시지가 표시 됩니다.

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

이 발생 비 결정적인 이며 방법임 시간 (예: 그래픽 렌더링) 중에 발생할 수 있습니다. 이 메시지를 표시, 명시적 컬렉션을 다른 곳에서 수행 하려는 경우 또는 하고자 하는 것이 좋습니다 [피어 개체의 수명을 줄일](#Helping_the_GC)합니다. 

## <a name="gc-bridge-options"></a>GC 연결 옵션

Xamarin.Android는 Android 및 Android 런타임 투명 한 메모리 관리를 제공합니다. 호출 Mono 가비지 수집기에 대 한 확장으로 구현 되는 *GC 브리지*합니다. 

GC 브리지의 Mono 가비지 컬렉션 및 수치는 피어 아웃 개체는 해당 끊김을 Android 런타임 힙을 사용 하 여 확인 해야 하는 동안 작동 합니다. GC 브리지는 순서 대로 다음 단계를 수행 하 여이 결정을이 내립니다.

1.  나타내는 Java 개체에 연결할 수 없는 피어 개체의 mono 참조 그래프를 유도 합니다. 

2.  Java GC를 수행 합니다.

3.  어떤 개체가 실제로 소멸 되었는지 확인 합니다. 

이 복잡 한 프로세스는의 서브 클래스를 사용 하면 어떤 `Java.Lang.Object` 개체를 자유롭게 참조;는 Java에서 개체에 바인딩될 수 제한이 제거 C#합니다. 이러한 복잡성 때문에 브리지 프로세스는 매우 많은 비용이 소요 될 수 있습니다 하 고 응용 프로그램에 눈에 띄는 일시 중지 될 수 있습니다. 응용 프로그램은 중요 한 일시 중지에 발생 하는 경우 다음 세 가지 GC 브리지 구현 중 하나를 조사할 가치가: 

-   **Tarjan** -GC 브리지의 완전히 새로운 디자인을 기반으로 [Robert Tarjan 알고리즘 및 이전 버전과 전파를 참조](https://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm)합니다.
    이 시뮬레이트된 워크 로드에서 최상의 성능을 있으며 실험적 코드의 더 큰 공유에 있습니다. 

-   **새** -원래 코드 정방형 동작의 두 인스턴스를 수정 하는 핵심 알고리즘 유지의 크게 변화 (기준 [Kosaraju의 알고리즘](https://en.wikipedia.org/wiki/Kosaraju's_algorithm) 구성 요소를 연결 된 강력한 찾기에 대 한). 

-   **이전** -1962 (세 가지 가장 안정적인 것으로 간주). 이 응용 프로그램 사용 해야 하는 다리를 `GC_BRIDGE` 일시 중지가 허용 합니다. 


GC 브리지의 작동을 가장 잘 파악 하는 유일한 방법은 응용 프로그램에서 실험 하 고 출력을 분석 하 여 됩니다. 벤치 마크에 대 한 데이터를 수집 하는 방법은 두 가지 있습니다. 

-   **로깅을 사용 하도록 설정** -로깅을 사용 하도록 설정 (에서 설명한 것 처럼 합니다 [구성](~/android/internals/garbage-collection.md) 섹션) 각 GC 연결 옵션을 캡처한 다음 각 설정의 로그 출력을 비교 합니다. 검사는 `GC` 특히, 각 옵션에 대 한 메시지는 `GC_BRIDGE` 메시지입니다. 비 대화형 응용 프로그램은 적절 한 경우 되지만 (예: 게임) 매우 대화형 응용 프로그램에 대 한 60ms 위에서 일시 중지 문제에 대 한 최대 150ms 일시 중지 합니다. 

-   **연결 계정 사용** -연결 계정에 연결 프로세스에 관련 된 각 개체를 가리키는 개체의 평균 비용이 표시 됩니다. 크기에 따라 정렬 되는이 정보는 추가 개체를 가장 많이 보유 하 고 항목에 대 한 힌트를 제공 합니다. 


지정 하 `GC_BRIDGE` 옵션 응용 프로그램 사용 해야 합니다, 전달 `bridge-implementation=old`를 `bridge-implementation=new` 또는 `bridge-implementation=tarjan` 에 `MONO_GC_PARAMS` 예를 들어 환경 변수: 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

기본 설정은 **Tarjan**합니다. 회귀를 찾을 경우는 것이 옵션을 설정 하는 데 필요한 **이전**합니다. 또한, 더 안정적을 사용 하도록 선택할 수 있습니다 **이전** 옵션 **Tarjan** 성능 향상을 생성 하지 않습니다. 

<a name="Helping_the_GC" />

## <a name="helping-the-gc"></a>GC 지원

여러 가지 방법으로 GC에서 메모리 사용 및 수집 시간을 줄이는 데 있습니다.



### <a name="disposing-of-peer-instances"></a>피어 인스턴스 삭제

GC에는 불완전 한 보기가 작동 하지 않을 수도 확인 하 고 프로세스의 메모리 사용량이 적을 때 없으므로 GC는 메모리가 부족 합니다. 

인스턴스의 예를 들어를 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) 형식 또는 파생된 형식의 크기가 20 바이트인 이상 (알림 등 없이 변경 될 수 있습니다 등.). 
[관리 되는 호출 가능 래퍼](~/android/internals/architecture.md) 하므로 추가 인스턴스 멤버를 추가 하지 경우는 [Android.Graphics.Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) 메모리의 10MB blob 참조는 인스턴스의 Xamarin.Android의 GC는 알 수 없습니다 &ndash; GC 20 바이트 개체를 표시 하 고 10 MB의 메모리만 활성화 된 상태로 유지 되는 Android 런타임 할당 개체에 연결 되어 있는지 확인할 수 없습니다. 

GC는 데 자주 필요한 경우 아쉽게도 *GC 합니다. AddMemoryPressure()* 및 *GC 합니다. RemoveMemoryPressure()* 지원 되지 않으며, 따라서 있습니다 *알고* 큰 Java에서 할당 된 개체 그래프를 직접 호출 하도록 해야 할 수 있습니다만 해제 하는 [GC 합니다. Collect ()](xref:System.GC.Collect) 프롬프트는 Java 쪽을 해제 하려면 GC에 메모리 또는 사용자 수를 명시적으로 삭제 *Java.Lang.Object* 관리 되는 호출 가능 래퍼 및 Java 인스턴스 간의 매핑을 주요 하위 클래스입니다. 예를 들어, 참조 [버그 1084](http://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6)합니다. 


> [!NOTE]
> 해야 *매우* 삭제 하는 경우 주의 `Java.Lang.Object` 인스턴스 하위 클래스입니다.

호출할 때 메모리 손상의 가능성을 최소화 하려면 다음 지침을 준수 `Dispose()`합니다.


#### <a name="sharing-between-multiple-threads"></a>여러 스레드 간에 공유

경우는 *Java 또는 관리 되는* 인스턴스를 여러 스레드 간에 공유할 수 있습니다 *안 `Dispose()`d*를 **적이**합니다. 예를 들어 [`Typeface.Create()`](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/(System.String%2cAndroid.Graphics.TypefaceStyle)) 
반환할 수 있습니다는 *캐시 된 인스턴스*합니다. 가져오기는 여러 스레드가 동일한 인수를 제공 합니다 *동일한* 인스턴스. 따라서 `Dispose()`의 연산 합니다 `Typeface` 인스턴스 한 스레드에서 다른 스레드를 개선할 수 있는 무효화 될 수 있습니다 `ArgumentException`의 `JNIEnv.CallVoidMethod()` 등 인스턴스를 다른 스레드에서 삭제 된 합니다. 


#### <a name="disposing-bound-java-types"></a>바인딩된 Java 형식 삭제

인스턴스에 바인딩된 Java 형식의 경우의 인스턴스를 삭제할 수 있습니다 *하기만* 관리 되는 코드에서 인스턴스를 재사용할 수 없는 *및* Java 인스턴스 (참조 이전 스레드간에공유될수없습니다.`Typeface.Create()` 토론). (이 결정을 내릴 어려울 수 있습니다.) Java 인스턴스 입력 다음에 관리 코드를 *새* 래퍼에 대 한 생성 됩니다. 

드로어 블 및 다른 리소스 집약적 인스턴스에 연결할 때 자주 유용 합니다.

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

위 내용은 안전 하기 때문에 피어는 [Drawable.CreateFromPath()](https://developer.xamarin.com/api/member/Android.Graphics.Drawables.Drawable.CreateFromPath/) Framework 피어 이며 참조 반환 *하지* 사용자 피어. 합니다 `Dispose()` 끝날 때 호출 합니다 `using` 블록 관리 되는 관계의 연결이 끊어집니다 [Drawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.Drawable/) 및 프레임 워크 [Drawable](https://developer.android.com/reference/android/graphics/drawable/Drawable.html) Java 인스턴스가 있는 인스턴스 Android 런타임 해야 하는 즉시 수집 합니다. 결과 *되지* 사용자 피어에게 피어 인스턴스가 참조 하는 경우 안전 하 게 일 "external" 정보를 사용 하 고 여기 *알고* 는 합니다 `Drawable` 사용자 피어를 참조할 수 없습니다 이므로 `Dispose()` 호출 이 안전 합니다. 


#### <a name="disposing-other-types"></a>다른 형식 삭제 

인스턴스는 Java 형식 바인딩을 없는 형식을 참조 하는 경우 (사용자 지정 등 `Activity`), **하지** 호출 `Dispose()` 경우가 아니면 있습니다 *알고* 는 Java 코드 없이에 재정의 된 메서드를 호출 합니다 인스턴스입니다. 이렇게 하려면 오류로 인해 [ `NotSupportedException`s](~/android/internals/architecture.md#Premature_Dispose_Calls)입니다. 

예를 들어 있는 경우 사용자 지정 수신기를 클릭 합니다.

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

있습니다 *안* Java는 나중에 메서드를 호출 하려고 하는 대로이 인스턴스를 삭제 합니다.

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```


#### <a name="using-explicit-checks-to-avoid-exceptions"></a>예외를 방지 하려면 명시적 검사를 사용 하 여

구현한 경우는 [Java.Lang.Object.Dispose](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose(System.Boolean)/) 메서드를 오버 로드, JNI를 포함 하는 개체를 변경 하지 않도록 합니다. 발생할 수 있으므로 한 *이중 dispose* (심각 하 게) 코드를 수 있도록 하는 경우 이미 가비지 수집 된 하는 기본 Java 개체에 액세스 하려고 합니다. 그러면 다음과 유사한 예외가 생성 됩니다. 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

이 상황을 자주 개체의 첫 번째 dispose 하면 null이 되도록 멤버 및 null이 멤버에 대해 후속 액세스로 인해 예외가 throw 될 때 발생 합니다. 특히, 개체의 `Handle` 에서 첫 번째 dispose (기본 Java 인스턴스에 해당 하는 연결 관리 되는 인스턴스)가 무효화 됩니다 하지만 관리 코드는 여전히 경우에 더 이상 사용할 수 없습니다 (참조 기본Java인스턴스에이액세스하기위해시도[ 관리 되는 호출 가능 래퍼](~/android/internals/architecture.md#Managed_Callable_Wrappers) Java 인스턴스 및 관리 되는 인스턴스 간 매핑에 대 한 자세한 내용은). 

이 예외를 방지 하는 좋은 방법은 명시적으로 확인 하는 것에 `Dispose` 관리 되는 인스턴스 및 기본 Java 인스턴스 간의 매핑은 여전히 유효;는 하는 메서드를 확인 하는 경우 개체의 `Handle` 가 null (`IntPtr.Zero`) 해당 멤버에 액세스 합니다. 다음 예를 들어 `Dispose` 메서드 액세스를 `childViews` 개체: 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);
        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```

초기 dispose를 전달 하면 `childViews` 잘못 된 있어야 `Handle`의 `for` 루프 액세스를 발생 시킵니다는 `ArgumentException`합니다. 명시적인 추가 하 여 `Handle` 첫 번째 검사 null `childViews` 에 액세스 하 고 다음 `Dispose` 메서드 예외 발생을 방지 합니다. 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);

        // Check for a null handle:
        if (this.childViews.Handle == IntPtr.Zero)
            return;

        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```


### <a name="reduce-referenced-instances"></a>참조 된 인스턴스를 줄이기

때마다 인스턴스를 `Java.Lang.Object` 형식 또는 서브 클래스 전체 GC를 하는 동안 검색 되 *개체 그래프* 인스턴스 가리킵니다도 검사 해야 합니다. 개체 그래프는 "루트 인스턴스"를 참조 하는 개체 인스턴스 집합 *plus* 를 재귀적으로 루트 인스턴스에서 참조 하 모든 항목을 참조 합니다. 

다음 클래스를 고려 합니다.

```csharp
class BadActivity : Activity {

    private List<string> strings;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```

때 `BadActivity` 는 생성 된 개체 그래프 인스턴스가 포함 될 10004 (1 x `BadActivity`, 1 x `strings`, 1 x `string[]` 소유 `strings`, 문자열 인스턴스 x 10000), *모든* 되도록 해야 하는 중 검색 될 때마다는 `BadActivity` 인스턴스를 검색 합니다. 

이 나쁜 영향을 미칠 수 있습니다 프로그램 컬렉션 시간 향상 GC 일시 중지 시간입니다. 

GC를 도움이 *줄이는* 사용자 피어 인스턴스에서 루트는 개체 그래프의 크기입니다. 위의 예제에서는이 가능 이동 하 여 `BadActivity.strings` 별도 클래스로 만든 Java.Lang.Object에서 상속 하지 않습니다. 

```csharp
class HiddenReference<T> {

    static Dictionary<int, T> table = new Dictionary<int, T> ();
    static int idgen = 0;

    int id;

    public HiddenReference ()
    {
        lock (table) {
            id = idgen ++;
        }
    }

    ~HiddenReference ()
    {
        lock (table) {
            table.Remove (id);
        }
    }

    public T Value {
        get { lock (table) { return table [id]; } }
        set { lock (table) { table [id] = value; } }
    }
}

class BetterActivity : Activity {

    HiddenReference<List<string>> strings = new HiddenReference<List<string>>();

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```


## <a name="minor-collections"></a>부 컬렉션

부 컬렉션을 호출 하 여 수동으로 수행할 수 있습니다 [GC 합니다. Collect(0)](xref:System.GC.Collect)합니다. 부 컬렉션은 (주요 컬렉션에 비해) 하는 경우 저렴 한, 하지만 권한이 상당한 많습니다 트리거할 않으려는 하므로 고정 비용에 있고 몇 시간 (밀리초)을 일시 중지 시간을 있어야 합니다. 

응용 프로그램에는 동일한 작업 수행 됩니다 계속 해 서 "근무 주기가" 근무 주기가 종료 된 후 수동으로 약간 수집을 수행 하는 것이 좋습니다 수도 있습니다. Duty 주기 예제는 다음과 같습니다. 

-  단일 게임 프레임의 렌더링 하는 주기입니다.
-  지정 된 앱 대화 상자 (열, 입력, 닫기)를 사용 하 여 전체 상호 작용 
-  그룹 새로 고침/앱 데이터 동기화에 대 한 네트워크 요청입니다.



## <a name="major-collections"></a>주요 컬렉션

주요 컬렉션을 호출 하 여 수동으로 수행할 수 있습니다 [GC 합니다. Collect ()](xref:System.GC.Collect) 또는 `GC.Collect(GC.MaxGeneration)`합니다. 

드물게 수행 해야 하며 512MB 힙을 수집 하는 경우 스타일 Android 장치에서 1 초 일시 중지 시간이 있을 수 있습니다. 

주요 컬렉션만 수동으로 호출 될 경우: 

-   긴 근무 끝날 때 주기 및 long 일시 중지 하는 경우 사용자에 게 문제를 일으킵니다 없습니다. 

-   재정의 된 내 [Android.App.Activity.OnLowMemory()](https://developer.xamarin.com/api/member/Android.App.Activity.OnLowMemory/) 메서드. 



## <a name="diagnostics"></a>진단

전역 참조 생성 및 제거 하는 경우 추적을 설정할 수 있습니다 합니다 [debug.mono.log](~/android/troubleshooting/index.md) 포함 하는 시스템 속성 [ *gref* ](~/android/troubleshooting/index.md) 및/또는 [ *gc*](~/android/troubleshooting/index.md)합니다. 



## <a name="configuration"></a>구성

Xamarin.Android 가비지 수집기를 설정 하 여 구성할 수 있습니다는 `MONO_GC_PARAMS` 환경 변수입니다. 빌드 작업을 사용 하 여 환경 변수를 설정할 수 있습니다 [AndroidEnvironment](~/android/deploy-test/environment.md)합니다.

`MONO_GC_PARAMS` 환경 변수는 다음 매개 변수를 쉼표로 구분 된 목록: 

-   `nursery-size` = *size* : Nursery의 크기를 설정합니다. 크기 (바이트)에 지정 된 및 2의 거듭제곱 이어야 합니다. 접미사 `k` 하십시오 `m` 및 `g` kilo-, mega-및 기가바이트, 각각 지정 데 사용할 수 있습니다. Nursery는 1 세대 (2 개). 큰 nursery 프로그램 속도가 일반적으로 하지만 더 많은 메모리를 사용할지 분명 합니다. 기본 nursery 크기가 512kb입니다. 

-   `soft-heap-limit` = *size* : 대상 최대 앱에 대 한 메모리 소비량을 관리 합니다. 지정된 된 값 보다 작은 메모리 사용 되 면 GC는 실행 시간 (적은 컬렉션)에 대 한 최적화 됩니다. 
    이 제한을 초과 GC는 메모리 사용량 (자세한 컬렉션)에 대 한 최적화 됩니다. 

-   `evacuation-threshold` = *threshold* : 백분율에서 철수 임계값을 설정합니다. 값은 0에서 100 사이의 정수 여야 합니다. 기본값은 66 합니다. 컬렉션의 스윕 단계는 특정 힙 블록 형식의 점유율은이 백분율 보다 작아야 합니다을 찾으면, 100%에 가까운를 선점 복원 다음 주요 컬렉션의 해당 블록 형식에 대 한 복사 컬렉션을 수행 합니다. 값이 0 철수를 해제합니다. 

-   `bridge-implementation` = *구현 브리지* : GC 성능 문제를 해결 하는 데 GC 연결 옵션을 설정 합니다. 세 가지 가능한 값: *이전* 를 *새* 하십시오 *tarjan*합니다.

-   `bridge-require-precise-merge`: 브리지 포함, 가끔 않을 개체 수를 최적화 Tarjan 가비지 먼저 되기 전에 하나의 GC를 수집 합니다. 이 옵션을 포함 하 여 해당 최적화를 잠재적으로 저하 되지만 예측 가능성이 더욱 뛰어난 Gc를 수행을 사용 하지 않도록 설정 합니다.

예를 들어, 128MB의 힙 크기 제한이 GC를 구성 하려면 사용 하 여 프로젝트에 새 파일이 추가 된 **빌드 작업** 의 `AndroidEnvironment` 내용으로: 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
