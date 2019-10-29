---
title: 가비지 컬렉션
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: 62560d97a2e85a6045e419f0c0602a375f5a2a75
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027880"
---
# <a name="garbage-collection"></a>가비지 컬렉션

Xamarin.ios는 Mono의 [Simple 세대 가비지 수집기](https://www.mono-project.com/docs/advanced/garbage-collector/sgen/)를 사용 합니다. 다음 두 가지 종류의 컬렉션을 포함 하는 두 세대 및 *큰 개체 공간*을 포함 하는 표시 및 스윕 가비지 수집기입니다. 

- 보조 컬렉션 (Gen0 힙 수집) 
- 주요 컬렉션 (Gen1 및 큰 개체 공간 힙을 수집) 

> [!NOTE]
> GC를 통한 명시적 컬렉션이 없으면입니다 [. 수집 ()](xref:System.GC.Collect) 컬렉션은 힙 할당을 기반으로 하는 *요청*시입니다. *참조 횟수 시스템이 아닙니다*. *보류 중인 참조가*없거나 범위가 종료 되는 즉시 개체가 수집 되지 않습니다. 보조 힙에서 새 할당을 위한 메모리가 부족 한 경우 GC가 실행 됩니다. 할당이 없으면 실행 되지 않습니다.

사소한 컬렉션은 저렴 하 고 자주 수행 되며 최근에 할당 된 개체와 데드 개체를 수집 하는 데 사용 됩니다. 몇 MB의 할당 된 개체를 실행 한 후에 보조 컬렉션이 수행 됩니다. 보조 컬렉션은 GC를 호출 하 여 수동으로 수행할 수 있습니다 [. Collect (0)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

주요 컬렉션은 비용이 많이 들고 빈도가 낮지만 모든 비활성 개체를 회수 하는 데 사용 됩니다. 주 컬렉션은 메모리가 현재 힙 크기에 대해 사용 된 후 (힙 크기를 조정 하기 전) 수행 됩니다. 주 컬렉션은 GC를 호출 하 여 수동으로 수행할 수 있습니다 [. ()](xref:System.GC.Collect) 또는 GC를 호출 하 여 수집 [합니다. ](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_)인수 GC를 사용 하 여 (int)를 수집 [합니다. MaxGeneration](xref:System.GC.MaxGeneration). 

## <a name="cross-vm-object-collections"></a>VM 간 개체 컬렉션

개체 형식에는 세 가지 범주가 있습니다.

- **관리 되는 개체**: system.object에서 상속 *되지* 않는 형식 [입니다. 예를 들어](xref:Java.Lang.Object) [system.string](xref:System.String)입니다. 
    이러한 설정은 일반적으로 GC에 의해 수집 됩니다. 

- **Java 개체**: ANDROID 런타임 VM 내에 있지만 Mono vm에 노출 되지 않는 java 유형입니다. 이러한 내용은 보링 이며 더 이상 설명 하지 않습니다. 이러한 기능은 Android 런타임 VM에서 정상적으로 수집 됩니다. 

- **피어 개체**: [IJavaObject](xref:Android.Runtime.IJavaObject) 을 구현 하는 형식 (예 [: 모든](xref:Java.Lang.Object) [java.lang.throwable](xref:Java.Lang.Throwable) 하위 클래스)입니다. 이러한 형식의 인스턴스에는 *관리 되는 피어* 와 *네이티브 피어*라는 두 개의 "halfs"가 있습니다. 관리 되는 피어는 C# 클래스의 인스턴스입니다. 네이티브 피어는 Android 런타임 VM 내에서 Java 클래스의 인스턴스이고 C# [IJavaObject](xref:Android.Runtime.IJavaObject.Handle) 속성은 네이티브 피어에 대 한 JNI 전역 참조를 포함 합니다. 

Native 피어에는 다음과 같은 두 가지 유형이 있습니다.

- **프레임 워크 피어** : Xamarin이 없는 "일반" Java 형식입니다 (예:).   [android. 내용. 컨텍스트](xref:Android.Content.Context).

- **사용자 피어** : 응용 프로그램 내에 있는 각 Java. Lang 서브 클래스에 대해 빌드 시 생성 되는 [Android 호출 가능 래퍼입니다](~/android/platform/java-integration/working-with-jni.md) .

Xamarin Android 프로세스 내에는 두 개의 Vm이 있으므로 다음과 같은 두 가지 유형의 가비지 컬렉션이 있습니다.

- Android 런타임 컬렉션 
- Mono 컬렉션 

Android 런타임 컬렉션은 정상적으로 작동 하지만 주의 해야 합니다. JNI 전역 참조는 GC 루트로 처리 됩니다. 따라서 JNI 전역 참조가 Android runtime VM 개체를 보유 하는 경우 컬렉션에 적합 한 경우에도 개체를 수집할 *수 없습니다* .

Mono 컬렉션은 흥미로운 상황을 말합니다. 관리 되는 개체는 정상적으로 수집 됩니다. 피어 개체는 다음 프로세스를 수행 하 여 수집 됩니다.

1. Mono 컬렉션에 적합 한 모든 피어 개체는 JNI 전역 참조가 JNI weak global 참조로 대체 되었습니다. 

2. Android 런타임 VM GC가 호출 됩니다. 모든 네이티브 피어 인스턴스가 수집 될 수 있습니다. 

3. (1)에서 만든 JNI weak global 참조가 선택 됩니다. Weak 참조가 수집 된 경우 피어 개체가 수집 됩니다. Weak 참조가 수집 *되지 않은* 경우 WEAK 참조가 JNI 전역 참조로 바뀌고 피어 개체가 수집 되지 않습니다. 참고: API 14 이상에서는 `IJavaObject.Handle`에서 반환 된 값이 GC 후에 변경 될 수 있습니다. 

모든 this의 최종 결과는 피어 개체의 인스턴스가 관리 코드 (예: `static` 변수에 저장 됨)에서 참조 되거나 Java 코드에서 참조 되는 한 활성 상태로 유지 되는 것입니다. 또한 네이티브 피어와 관리 되는 피어를 모두 수집 가능 하 게 될 때까지 네이티브 피어를 수집 하지 않기 때문에 네이티브 피어의 수명은 그에 따라 살고 있는 것 이상으로 확장 됩니다.

## <a name="object-cycles"></a>개체 주기

피어 개체는 Android 런타임 및 Mono VM 모두에 논리적으로 나타납니다. 예를 들어, [android. 작업](xref:Android.App.Activity) 의 관리 되는 피어 인스턴스는 해당 하는 [android. Activity](https://developer.android.com/reference/android/app/Activity.html) framework 피어 Java 인스턴스를 가집니다. [Java. 개체](xref:Java.Lang.Object) 에서 상속 되는 모든 개체에는 두 vm 내에 표현이 있을 수 있습니다. 

두 Vm 모두에 표시 되는 모든 개체에는 단일 VM 내에만 있는 개체 (예: [`System.Collections.Generic.List<int>`](xref:System.Collections.Generic.List%601))에 비해 확장 되는 수명이 있습니다. [GC를 호출 합니다. ](xref:System.GC.Collect)Xamarin. ANDROID GC는 개체를 수집 하기 전에 VM에서 참조 하지 않도록 해야 하므로 이러한 개체를 수집 하지 않아도 됩니다. 

개체 수명을 줄이려면, Java. a d. [Dispose ()](xref:Java.Lang.Object.Dispose) 를 호출 해야 합니다. 이렇게 하면 전역 참조를 해제 하 여 두 Vm 사이에서 개체에 대 한 연결을 "서버"에 수동으로 "연결" 하 여 개체를 더 빠르게 수집할 수 있습니다. 

## <a name="automatic-collections"></a>자동 컬렉션

[Release 4.1.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/mono_for_android_4/mono_for_android_4.1.0/index.md)부터는 gref 임계값에 도달 하면 xamarin.ios가 자동으로 전체 GC를 수행 합니다. 이 임계값은 에뮬레이터에서 1800 grefs (2000 max) 및 46800 grefs on 하드웨어 (최대 52000)의 플랫폼에 대해 알려진 최대 grefs의 90%입니다. *참고:* Xamarin.ios는 [JNIEnv](xref:Android.Runtime.JNIEnv)에서 만든 grefs만 계산 하 고 프로세스에서 만든 다른 grefs는 인식 하지 못합니다. 이는 추론 *전용*입니다. 

자동 컬렉션을 수행 하는 경우 다음과 유사한 메시지가 디버그 로그에 출력 됩니다.

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

이 항목의 발생은 비결 정적 이며, 리디렉션하거나 번 (예: 그래픽 렌더링 중)에 발생할 수 있습니다. 이 메시지가 표시 되 면 다른 곳에서 명시적 컬렉션을 수행 하거나 [피어 개체의 수명을 줄이려고](#Helping_the_GC)할 수 있습니다. 

## <a name="gc-bridge-options"></a>GC 브리지 옵션

Xamarin android는 Android 및 Android 런타임으로 투명 한 메모리 관리를 제공 합니다. 이는 *GC 브리지*라는 Mono 가비지 수집기에 대 한 확장으로 구현 됩니다. 

GC 브리지는 Mono 가비지 수집 중에 작동 하며, Android 런타임 힙을 사용 하 여 "선거의"이 확인 되어야 하는 피어 개체를 파악 합니다. 다음 단계를 순서 대로 수행 하 여 GC 브리지가 이러한 결정을 합니다.

1. 연결할 수 없는 피어 개체의 mono 참조 그래프를 표시 하는 Java 개체로 유도 합니다. 

2. Java GC를 수행 합니다.

3. 정말 데드 된 개체를 확인 합니다. 

이 복잡 한 프로세스는 `Java.Lang.Object`의 서브 클래스가 개체를 자유롭게 참조할 수 있도록 하는 것입니다. 이를 통해 C#바인딩할 수 있는 Java 개체에 대 한 제한 사항이 제거 됩니다. 이러한 복잡성 때문에 브리지 프로세스는 매우 비용이 많이 들고 응용 프로그램에서 일시 중지가 발생할 수 있습니다. 응용 프로그램에 상당한 일시 중지가 발생 하는 경우 다음 3 개의 GC 브리지 구현 중 하나를 조사할 가치가 있습니다. 

- **Tarjan** - [Robert Tarjan의 알고리즘과 역방향 참조 전파](https://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm)를 기반으로 하는 GC 브리지의 완전히 새로운 디자인입니다.
    시뮬레이션 된 워크 로드에서 최상의 성능을 갖지만 실험적 코드를 더 많이 공유 하 고 있습니다. 

- **New** -원래 코드의 주요 재정비입니다 .이는 정방형 동작의 두 인스턴스를 수정 하지만 핵심 알고리즘은 유지 합니다 (강력한 연결 구성 요소를 찾는 [Kosaraju 알고리즘](https://en.wikipedia.org/wiki/Kosaraju's_algorithm) 기반). 

- **이전** -원래 구현 (가장 안정적으로 3 개) `GC_BRIDGE` 일시 중지가 허용 되는 경우 응용 프로그램에서 사용 해야 하는 브리지입니다. 

가장 잘 작동 하는 GC 브리지를 파악 하는 유일한 방법은 응용 프로그램에서 실험 하 고 출력을 분석 하는 것입니다. 벤치마킹을 위해 데이터를 수집 하는 방법에는 두 가지가 있습니다. 

- **로깅 사용** -각 GC 브리지 옵션에 대 한 로깅 ( [구성](~/android/internals/garbage-collection.md) 섹션에서 설명)을 사용 하도록 설정한 다음 각 설정에서 로그 출력을 캡처하고 비교 합니다. 각 옵션에 대 한 `GC` 메시지를 검사 합니다. 특히 `GC_BRIDGE` 메시지입니다. 비 대화형 응용 프로그램에 대해 최대 150ms을 일시 중지 하지만 매우 대화형 응용 프로그램 (예: 게임)의 경우 60ms 이상으로 일시 중지 하는 것은 문제입니다. 

- 브리지 **계정 사용** -브리지 계정에 브리지 프로세스와 관련 된 각 개체가 가리키는 개체의 평균 비용을 표시 합니다. 이 정보를 크기 별로 정렬 하면 많은 양의 추가 개체를 보유 하 고 있는 것에 대 한 힌트를 제공 합니다. 

응용 프로그램에서 `GC_BRIDGE` 옵션을 지정 하려면 `MONO_GC_PARAMS` 환경 변수에 `bridge-implementation=old`, `bridge-implementation=new` 또는 `bridge-implementation=tarjan`를 전달 합니다. 예를 들면 다음과 같습니다. 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

기본 설정은 **Tarjan**입니다. 회귀를 발견 한 경우이 옵션을 **이전**으로 설정 해야 할 수 있습니다. 또한 **Tarjan** 가 성능 향상을 생성 하지 않는 경우 더 안정적인 **이전** 옵션을 사용 하도록 선택할 수 있습니다. 

<a name="Helping_the_GC" />

## <a name="helping-the-gc"></a>GC 지원

GC에서 메모리 사용 및 수집 시간을 줄이는 데 도움이 되는 여러 가지 방법이 있습니다.

### <a name="disposing-of-peer-instances"></a>피어 인스턴스 삭제

Gc는 프로세스를 불완전 하 게 표시 하며, GC가 메모리가 부족 하다는 것을 알지 못하기 때문에 메모리가 부족 한 경우에는 실행 되지 않을 수 있습니다. 

예를 들어, [Java Lang. 개체](xref:Java.Lang.Object) 형식 또는 파생 형식의 인스턴스는 크기가 20 바이트 이상 (예: 표시 하지 않고 변경 될 수 있습니다. 등)입니다. 
[관리 되는 호출 가능 래퍼](~/android/internals/architecture.md) 는 추가 인스턴스 멤버를 추가 하지 않습니다. 따라서 10mb의 메모리를 참조 하는 [Android.](xref:Android.Graphics.Bitmap) x s s. i n s i &ndash; s. 10MB의 메모리를 유지 하는 Android 런타임에 할당 된 개체에 연결 되어 있는지 확인할 수 없습니다. 

GC를 지원 해야 하는 경우가 많습니다. 아쉽게도 *GC. AddMemoryPressure ()* 및 *GC. RemoveMemoryPressure ()* 은 지원 되지 않으므로 매우 많은 Java 할당 개체 그래프를 *해제 한 경우* 수동으로 GC를 호출 해야 할 수 있습니다 [. 을 수집 ()](xref:System.GC.Collect) 하 여 java 쪽 메모리를 해제 하도록 GC를 요청 하거나, 관리 되는 호출 가능 래퍼와 java 인스턴스 간의 매핑이 손상 될 수 *있습니다.* 예를 들어 [버그 1084](https://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6)을 참조 하세요. 

> [!NOTE]
> `Java.Lang.Object` 하위 클래스 인스턴스를 삭제 하는 경우 *매우* 주의 해야 합니다.

메모리 손상 가능성을 최소화 하려면 `Dispose()`를 호출할 때 다음 지침을 준수 합니다.

#### <a name="sharing-between-multiple-threads"></a>여러 스레드 간에 공유

*Java 또는 관리 되* 는 인스턴스를 여러 스레드 간에 공유할 수 있는 **경우에는** *`Dispose()`d가 되지 않아야*합니다. 예를 들어 [`Typeface.Create()`](xref:Android.Graphics.Typeface.Create*) 
*캐시 된 인스턴스*를 반환할 수 있습니다. 여러 스레드가 동일한 인수를 제공 하는 경우 *동일한* 인스턴스를 가져옵니다. 따라서 한 스레드에서 `Typeface` 인스턴스를 `Dispose()`하는 경우 다른 스레드가 무효화 될 수 있습니다 .이 경우 다른 스레드에서 인스턴스가 삭제 되었기 때문에 `JNIEnv.CallVoidMethod()`에서 `ArgumentException`을 발생 시킬 수 있습니다. 

#### <a name="disposing-bound-java-types"></a>바인딩된 Java 형식 삭제

인스턴스가 바인딩된 Java 형식이 면 *인스턴스는 관리* 코드에서 다시 사용 되지 않는 한 인스턴스를 삭제할 수 *있으며* , Java 인스턴스는 스레드 간에 공유할 수 없습니다 (이전 `Typeface.Create()` 토론 참조). (이러한 결정을 내리는 것이 어려울 수 있습니다.) 다음 번에 Java 인스턴스가 관리 코드에 들어가면 *새* 래퍼가 생성 됩니다. 

이는 Drawables 및 기타 리소스를 많이 사용할 수 있는 경우에 유용 합니다.

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

위 내용은 그릴 수 있는 [CreateFromPath ()](xref:Android.Graphics.Drawables.Drawable.CreateFromPath*) 가 반환 하는 피어는 사용자 피어가 *아닌* 프레임 워크 피어를 참조 하기 때문에 안전 합니다. `using` 블록의 끝에 있는 `Dispose()` 호출은 관리 되는 [그릴](xref:Android.Graphics.Drawables.Drawable) 수 있는 인스턴스 및 프레임 워크에서 [그릴](https://developer.android.com/reference/android/graphics/drawable/Drawable.html) 수 있는 인스턴스 간의 관계를 해제 하므로 Android 런타임이 필요한 즉시 Java 인스턴스를 수집할 수 있습니다. 이는 피어 인스턴스가 사용자 피어를 참조 하는 경우 안전 *하지 않습니다* . 여기서는 "외부" 정보를 사용 하 여 `Drawable` 사용자 피어를 참조할 수 없다는 것을 *알* 수 있으므로 `Dispose()` 호출은 안전 합니다. 

#### <a name="disposing-other-types"></a>기타 형식 삭제 

인스턴스가 Java 유형 (예: 사용자 지정 `Activity`)의 바인딩이 아닌 유형을 참조 *하는 경우* 에는 java 코드가 해당 인스턴스에서 재정의 된 메서드를 호출 하지 않는 한 `Dispose()`를 호출 **하지 마세요** . 이렇게 하지 않으면 [`NotSupportedException`s](~/android/internals/architecture.md#Premature_Dispose_Calls)가 발생 합니다. 

예를 들어 사용자 지정 클릭 수신기가 있는 경우 다음을 수행 합니다.

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

Java에서 나중에 메서드를 호출 하기 때문에이 인스턴스를 삭제 *해서는 안* 됩니다.

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```

#### <a name="using-explicit-checks-to-avoid-exceptions"></a>명시적 검사를 사용 하 여 예외 방지

JNI [오버 로드 메서드를 구현한](xref:Java.Lang.Object.Dispose*) 경우에는이를 포함 하는 개체를 터치 하지 않습니다. 이렇게 하면 코드가 이미 가비지 수집 된 기본 Java 개체에 액세스할 수 있도록 하는 *이중 삭제* 상황이 발생할 수 있습니다 (그다지). 이렇게 하면 다음과 유사한 예외가 생성 됩니다. 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

이 상황은 일반적으로 개체의 첫 번째 dispose로 인해 멤버가 null이 될 때 발생 합니다. 그러면이 null 멤버에 대 한 후속 액세스 시도로 인해 예외가 throw 됩니다. 특히 관리 되는 인스턴스를 기본 Java 인스턴스에 연결 하는 개체의 `Handle`는 첫 번째 dispose에서 무효화 되지만 관리 코드는 더 이상 사용할 수 없는 경우에도이 기본 Java 인스턴스에 계속 액세스 하려고 시도 합니다 (참조 [). ](~/android/internals/architecture.md#Managed_Callable_Wrappers)Java 인스턴스와 관리 되는 인스턴스 간의 매핑에 대 한 자세한 내용은 관리 되는 호출 가능 래퍼입니다. 

이 예외를 방지 하는 좋은 방법은 `Dispose` 메서드에서 관리 되는 인스턴스와 기본 Java 인스턴스 간의 매핑이 여전히 유효함을 명시적으로 확인 하는 것입니다. 즉, 해당 멤버에 액세스 하기 전에 개체의 `Handle` null 인지 (`IntPtr.Zero`) 확인 합니다. 예를 들어 다음 `Dispose` 메서드는 `childViews` 개체에 액세스 합니다. 

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

초기 dispose 전달으로 인해 `childViews`에 잘못 된 `Handle`있는 경우 `for` 루프 액세스에서 `ArgumentException`을 throw 합니다. 첫 번째 `childViews` 액세스 전에 명시적 `Handle` null 검사를 추가 하 여 다음 `Dispose` 메서드에서 예외가 발생 하지 않도록 합니다. 

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

### <a name="reduce-referenced-instances"></a>참조 된 인스턴스 줄이기

GC 중에 `Java.Lang.Object` 유형 또는 하위 클래스의 인스턴스를 검색할 때마다 인스턴스가 참조 하는 전체 *개체 그래프가* 검색 되어야 합니다. 개체 그래프는 "루트 인스턴스"가 참조 하는 개체 인스턴스 집합과 루트 인스턴스가 참조 하 *는 항목에서* 참조 하는 모든 항목을 재귀적으로 포함 합니다. 

다음 클래스를 살펴보세요.

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

`BadActivity` 생성 될 때 개체 그래프에는 10004 인스턴스 (1x `BadActivity`, 1x `strings`, `strings`에서 보유 한 1x `string[]`, 10000x 문자열 인스턴스가 포함 됨)가 포함 됩니다 .이 *모든* 항목은 `BadActivity` 인스턴스를 검색할 때마다 검색 해야 합니다. 

이로 인해 컬렉션 시간에 부정적인 영향을 줄 수 있으므로 GC 일시 중지 시간이 늘어납니다. 

사용자 피어 인스턴스를 기반으로 하는 개체 그래프의 크기를 *줄여* GC를 지원할 수 있습니다. 위의 예제에서이 작업을 수행 하려면 `BadActivity.strings`을 다른 클래스로 이동 하 여이 작업을 수행 해야 합니다. 

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

## <a name="minor-collections"></a>보조 컬렉션

보조 컬렉션은 GC를 호출 하 여 수동으로 수행할 수 있습니다 [. (0)을 수집](xref:System.GC.Collect)합니다. 사소한 컬렉션은 저렴 하지만 (주 컬렉션에 비해) 상당한 고정 비용이 발생 하므로 너무 자주 트리거하지 않고 일시 중지 시간 (밀리초)이 있어야 합니다. 

응용 프로그램에 동일한 작업을 수행 하는 "업무 주기"가 있는 경우 업무 주기가 종료 된 후에는 보조 컬렉션을 수동으로 수행 하는 것이 좋습니다. 예제 작업 주기는 다음과 같습니다. 

- 단일 게임 프레임의 렌더링 주기입니다.
- 지정 된 앱 대화 상자와의 전체 상호 작용 (열기, 채우기, 닫기) 
- 앱 데이터를 새로 고치거 나 동기화 할 네트워크 요청 그룹입니다.

## <a name="major-collections"></a>주요 컬렉션

주 컬렉션은 GC를 호출 하 여 수동으로 수행할 수 있습니다 [. ()](xref:System.GC.Collect) 또는 `GC.Collect(GC.MaxGeneration)`을 수집 합니다. 

이러한 장치는 거의 수행 되지 않으며, 512MB 힙을 수집할 때 Android 스타일 장치에 일시 중지 시간이 있을 수 있습니다. 

주 컬렉션은 다음과 같은 경우에만 수동으로 호출 해야 합니다. 

- 시간이 오래 걸리는 경우와 긴 일시 중지로 인해 사용자에 게 문제가 발생 하지 않을 수 있습니다. 

- 재정의 된 [Android. app.config. OnLowMemory ()](xref:Android.App.Activity.OnLowMemory) 메서드 내에서 

## <a name="diagnostics"></a>진단

전역 참조가 생성 되 고 삭제 되는 시기를 추적 하기 위해 [*gref*](~/android/troubleshooting/index.md) 및/또는 [*gc*](~/android/troubleshooting/index.md)를 포함 하도록 [debug. mono](~/android/troubleshooting/index.md) 시스템 속성을 설정할 수 있습니다. 

## <a name="configuration"></a>Configuration

Xamarin Android 가비지 수집기는 `MONO_GC_PARAMS` 환경 변수를 설정 하 여 구성할 수 있습니다. 환경 변수는 [Androidenvironment](~/android/deploy-test/environment.md)의 빌드 작업을 사용 하 여 설정할 수 있습니다.

`MONO_GC_PARAMS` 환경 변수는 다음 매개 변수를 쉼표로 구분한 목록입니다. 

- `nursery-size` = *크기* : nursery의 크기를 설정 합니다. 크기는 바이트로 지정 되며 2의 거듭제곱 이어야 합니다. 접미사 `k`, `m` 및 `g`를 사용 하 여 각각 메가 gb와 기가바이트를 지정할 수 있습니다. Nursery은 첫 번째 세대 (2)입니다. Nursery 더 큰 경우 일반적으로 프로그램 속도는 향상 되지만 메모리를 더 많이 사용 합니다. 기본 nursery 크기는 512 kb입니다. 

- `soft-heap-limit` = *크기* : 앱에 대 한 대상 최대 관리 되는 메모리 사용량입니다. 메모리 사용이 지정 된 값 보다 적으면 GC는 실행 시간 (컬렉션 수)에 최적화 됩니다. 
    이 한도를 초과 하면 메모리 사용에 대해 GC가 최적화 됩니다 (더 많은 컬렉션). 

- `evacuation-threshold` = *threshold* : 비울 임계값을 백분율로 설정 합니다. 값은 0에서 100 사이의 정수 여야 합니다. 기본값은 66입니다. 컬렉션의 스윕 단계가 특정 힙 블록 형식의 선점이이 백분율 보다 작은 것을 발견 하면 다음 주요 컬렉션에서 해당 블록 형식에 대 한 복사 컬렉션을 수행 하 여이를 100% 가까이 복원 합니다. 값이 0 이면 비울 꺼집니다. 

- `bridge-implementation` = *브리지 구현* : gc의 성능 문제를 해결 하는 데 도움이 되도록 gc 브리지 옵션을 설정 합니다. 사용할 수 있는 값에는 *old* , *new* , *tarjan*의 세 가지가 있습니다.

- `bridge-require-precise-merge`: Tarjan 브리지에는 최적화가 포함 되어 있으며,이는 드문 경우 이지만 개체가 가비지로 전환 된 후 하나의 GC를 수집 하 게 될 수 있습니다. 이 옵션을 포함 하면 해당 최적화를 사용 하지 않도록 설정 하 여 Gc를 보다 예측 가능한 상태로 만들 수 있습니다.

예를 들어 힙 크기 제한인 128MB를 갖도록 GC를 구성 하려면 `AndroidEnvironment` **빌드 작업** 을 사용 하 여 프로젝트에 새 파일을 추가 합니다. 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
