---
title: "가비지 컬렉션"
ms.topic: article
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: db277f20e63a59690ffaa8a8544ff9540578d3f5
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/16/2018
---
# <a name="garbage-collection"></a>가비지 컬렉션

Xamarin.Android 모노의를 사용 하 여 [간단한 세대 가비지 수집기](http://www.mono-project.com/Compacting_GC)합니다. 이 두 세대와 표시 스윕 가비지 수집기와 *큰 개체 공간*, 두 종류의 컬렉션: 

-   작은 컬렉션 (수집 Gen0 힙) 
-   주요 컬렉션 (Gen1 수집 하 고 큰 개체 공백 힙)입니다. 

> [!NOTE]
> 통해 명시적 컬렉션의 지정 하지 않을 경우에서 [GC 합니다. Collect ()](xref:System.GC.Collect) 컬렉션은 *주문형*, 힙 할당에 기반 합니다. *이 참조 횟수 시스템*; 개체 *미해결 참조가 없으면으로 수집 되지 것입니다*, 또는 범위를가 종료 합니다. GC는 부 힙 새 할당에 대 한 메모리 부족 때 실행 됩니다. 할당이 없는 경우 실행 되지 않습니다.


보조 컬렉션 저렴 하 고, 자주 수행 되며 최근에 할당 되 고 비활성 개체를 수집 하는 데 사용 됩니다. 보조 컬렉션 할당 된 개체의 모든 몇 MB 후 수행 됩니다. 호출 하 여 보조 컬렉션을 수동으로 수행 될 수 [GC 합니다. (0)를 수집 합니다.](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

주요 컬렉션은 비용이 많이 덜 자주 수행 하 고 모든 비활성 개체를 회수 하는 데 사용 됩니다. 주요 컬렉션 메모리가 부족 한 상태에 대 한 현재 힙 크기 (힙 크기 조정) 이전 되 면 수행 됩니다. 호출 하 여 주요 컬렉션을 수동으로 수행 될 수 [GC 합니다. ()를 수집](xref:System.GC.Collect) 또는 호출 하 여 [GC 합니다. (Int)를 수집](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 인수와 함께 [GC 합니다. MaxGeneration](xref:System.GC.MaxGeneration)합니다. 



## <a name="cross-vm-object-collections"></a>교차 VM 개체 컬렉션입니다.

개체 유형의 세 가지 종류가 있습니다.

-   **관리 되는 개체**: 라도 형식 *하지* 에서 상속 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) , 예: [System.String](xref:System.String)합니다. 
    이러한 GC가 정상적으로 수집 됩니다. 

-   **Java 개체**: Java 형식을 Android 런타임 VM 내에 있는 있지만 모노 VM에 노출 되지 않습니다. 자세히 설명 하는 수 고 지루한 작업, 됩니다. 이러한 Android 런타임 VM에서 일반적으로 수집 됩니다. 

-   **개체를 피어 투 피어**: 어떤 구현 형식은 [IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) , 예를 들어 모든 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) 및 [Java.Lang.Throwable](https://developer.xamarin.com/api/type/Java.Lang.Throwable/) 하위 클래스입니다. 이러한 형식의 인스턴스는 두 개의 "halfs"는 *관리 되는 피어* 및 *네이티브 피어*합니다. 관리 되는 피어는 C# 클래스의 인스턴스입니다. 네이티브 피어 Android 런타임 VM 내에서 Java 클래스 및 C#의 인스턴스가 [IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) 속성 네이티브 피어에 JNI 전역 참조를 포함 합니다. 


네이티브 피어는 다음과 같은 두 종류가 있습니다.

-   **프레임 워크 피어** : 예: 알고 있음 아무 Xamarin.Android, "Normal" Java 형식 [android.content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)합니다.

-   **사용자 피어** : [Android 호출 가능 래퍼](~/android/platform/java-integration/working-with-jni.md) 응용 프로그램 내에 있는 각 Java.Lang.Object 하위 클래스에 대 한 빌드 시 생성 되는 합니다.


Xamarin.Android 프로세스 내에서 두 개의 Vm으로는 두 가지 유형의 가비지 컬렉션

-   Android 런타임 컬렉션 
-   모노 컬렉션 

Android 런타임 컬렉션 일반적으로 하지만 다는 경고와 함께 작동: JNI 전역 참조 GC 루트로 처리 됩니다. 따라서 전역는 JNI 없을 경우 참조 Android 런타임 VM 개체를 개체 점유 *없습니다* 수집을 수행할 수는 그렇지 않은 경우에 수집할 수 있습니다.

모노 컬렉션은 fun 상황이 발생 합니다. 관리 되는 개체는 일반적으로 수집 됩니다. 다음 프로세스를 수행 하 여 피어 개체를 수집 합니다.

1.  모노 수집을 수행할 수 있는 모든 피어 개체 자신의 JNI 전역 대 한 참조가 있어야 JNI 약한 전역 참조로 대체 합니다. 

2.  기존 Android 런타임이 VM GC가 호출 됩니다. 모든 네이티브 피어 인스턴스 수집 될 수 있습니다. 

3.  JNI 약한 전역 참조 (1)에서 만든 확인 됩니다. 약한 참조를 수집 하는 경우 피어 개체 수집 됩니다. 약한 참조에 있으면 *하지* 수집 된, 다음 약한 참조는 JNI 전역 참조 바뀌고 피어 개체는 수집 되지 않습니다. 참고: API 14+에 즉에서 반환 된 값 `IJavaObject.Handle` GC 후 변경 될 수 있습니다. 

이것은 피어 개체의 인스턴스 중 하나에서 참조 되 고으로 만들겠습니다는 모두의 최종 결과 관리 코드 (예:에 저장 된는 `static` 변수) 또는 Java 코드에서 참조 합니다. 어떤 게 초과 네이티브 피어 수명 확장할 수는 또한 라이브, 네이티브 피어 네이티브 피어와 관리 되는 피어가 수집 될 때까지 수집 되지 않습니다.


## <a name="object-cycles"></a>개체 주기

피어 개체 Android 런타임 및 모노 VM 내에서 논리적으로 제공 됩니다. 예를 들어 한 [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) 해당 관리 되는 피어 인스턴스가 갖습니다 [android.app.Activity](http://developer.android.com/reference/android/app/Activity.html) 프레임 워크 피어 Java 인스턴스. 상속 하는 모든 개체 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) 두 Vm 내에서 표현이을 사용할 수 있습니다. 

두 Vm에서 하 게 표현 하는 모든 개체는 단일 VM 내 에서만 제공 되는 개체를 비교 하 여 확장 수명을 갖습니다 (예: 한 [ `System.Collections.Generic.List<int>` ](xref:System.Collections.Generic.List%601)). 호출 [GC 합니다. 수집](xref:System.GC.Collect) Xamarin.Android GC를 수집 하기 전에 개체가 VM 중 하나에 의해 참조 되지 않으면 확인 해야 합니다. 이러한 개체를 반드시 수집 하지 않습니다. 

개체 수명 간단히 줄이거나 [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) 를 호출 해야 합니다. 이 직접 "서버" 개체를 더 빠르게 수집할 수 있도록 전역 참조를 해제 하 여 두 개의 Vm 간에 개체에 대 한 연결. 


## <a name="automatic-collections"></a>자동 컬렉션

부터는 [릴리스 4.1.0](https://developer.xamarin.com/releases/android/mono_for_android_4/mono_for_android_4.1.0), gref 임계값이 초과 하는 경우 Xamarin.Android 전체 GC를 자동으로 수행 합니다. 이 임계값은 90%는 플랫폼에 대 한 알려진된 최대 grefs: 에뮬레이터 (최대 2000)에서 1800 grefs 및 46800 grefs 하드웨어 (최대 52000). *참고:* Xamarin.Android 계산 하 여 만든 grefs [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/), 및 프로세스에서 만든 다른 grefs에 대해 알지 못합니다. 이것은 경험적 접근 *만*합니다. 

자동 컬렉션을 수행 하면 디버그 로그에 다음과 비슷한 메시지가 표시 됩니다.

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

이 발생은 임의로 이루어집니다 및 (예: 그래픽 렌더링) 중에 컴퓨터가 부적절 한 시간에 발생할 수 있습니다. 이 메시지가 표시, 다른 곳에서 명시적 컬렉션을 수행 해야 할 수 있습니다 또는 하려고 할 수 있습니다 [피어 개체의 유효 기간을 단축](#Helping_the_GC)합니다. 

## <a name="gc-bridge-options"></a>GC 브리지 옵션

Xamarin.Android는 Android 및 Android 런타임을 사용한 투명 한 메모리 관리를 제공합니다. 호출 모노 가비지 수집기에 대 한 확장으로 구현 됩니다는 *GC 브리지*합니다. 

GC 브리지의 모노 가비지 수집 및 수치 한 피어 아웃 개체 필요한 해당 "liveness"가 포함 된 Android 런타임 힙에 사용 하 여 확인 하는 동안 작동 합니다. GC 브리지 순서에 따라 다음 단계를 수행 하 여이 결정을 내립니다.

1.  나타나는 Java 개체에 연결할 수 없습니다. 피어 개체의 모노 참조 그래프를 유도 합니다. 

2.  Java GC를 수행 합니다.

3.  어떤 개체가 실제로 소멸 되었는지 확인 합니다. 

복잡 한이 프로세스는 버퍼의 서브 클래스 덕분에 `Java.Lang.Object` 개체를 자유롭게 참조; Java에 개체에 바인딩될 수 C# 제한 사항을 제거 합니다. 이러한 복잡성 때문에 브리지 프로세스 비용이 매우 많이 들 수 있습니다 및 응용 프로그램의 띄는 일시 중지 될 수 있습니다. 응용 프로그램이 중요 한 일시 중지를 발생 하는 경우에 다음 세 가지 GC 브리지 구현 중 하나를 조사 하 고 이므로: 

-   **Tarjan** -GC 브리지의 완전히 새로운 디자인에 따라 [Robert Tarjan 알고리즘 및 이전 버전과 전파 참조](http://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm)합니다.
    시뮬레이션 된 작업 부하에서 최상의 성능을 갖지만 실험적 코드의 더 큰 공유를 포함 합니다. 

-   **새** -정방형 동작의 두 인스턴스를 수정 하지만 핵심 알고리즘 유지 원래 코드의 주요 정비 (기반 [Kosaraju의 알고리즘](http://en.wikipedia.org/wiki/Kosaraju's_algorithm) 구성 요소를 연결 강력 하 게 찾기). 

-   **이전** -원래 구현 (세 가지 가장 안정적인 것으로 간주). 이 응용 프로그램은 경우에 사용 해야 하는 브리지는 `GC_BRIDGE` 일시 중지가 허용 됩니다. 


어떤 GC 브리지의 작동을 가장 잘 파악 하려면 유일한 방법은 응용 프로그램에서 실험 하 고 출력을 분석 하 여입니다. 벤치 마크에 대 한 데이터를 수집 하는 방법은 두 가지가 있습니다. 

-   **로깅을 사용 하도록 설정** -로깅을 사용 하도록 설정 (의 설명 대로 [구성](~/android/internals/garbage-collection.md) 섹션)를 각 GC 브리지 옵션에 대 한 캡처 및 각 설정으로 인해 로그 출력을 비교 합니다. 검사는 `GC` ; 특히, 각 옵션에 대 한 메시지는 `GC_BRIDGE` 메시지입니다. 비 대화형 응용 프로그램, 있고 일시 중지 (예: 게임) 매우 대화형 응용 프로그램에 대 한 60ms 위에 있는 문제에 대 한 최대 150ms 일시 중지 합니다. 

-   **브리지 계정을 활성화** -브리지 회계 브리지 프로세스에 관련 된 각 개체를 가리키는 개체의 평균 운송 비용을 표시 합니다. 이 정보를 크기 별로 정렬 추가 개체를 가장 많이 보유 기능에 대 한 힌트를 제공 합니다. 


지정할 수 `GC_BRIDGE` 옵션 응용 프로그램 사용 해야 합니다, 전달 `bridge-implementation=old`, `bridge-implementation=new` 또는 `bridge-implementation=tarjan` 에 `MONO_GC_PARAMS` 예를 들어 환경 변수: 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

기본 설정은 **Tarjan**합니다. 한 회귀를 찾을 경우는 것이 옵션을 설정 하는 데 필요한 **이전**합니다. 또한, 더 안정적을 사용 하도록 선택할 수 있습니다 **이전** 옵션 **Tarjan** 성능 향상을 생성 하지 않습니다. 

<a name="Helping_the_GC" />

## <a name="helping-the-gc"></a>GC 수 있도록 지원

여러 가지 방법으로 메모리 사용 및 수집 시간을 줄이기 위한 GC 수 있도록 합니다.



### <a name="disposing-of-peer-instances"></a>피어 인스턴스 삭제

GC 불완전 한 뷰를 포함 하는 프로세스와 실행 되지 년 5 월의 메모리가 부족 GC 해당 메모리를 인식 하지 때문에 낮습니다. 

인스턴스의 예를 들어 한 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) 형식 또는 파생된 형식인은 크기가 20 바이트인 이상 (공지 등 없이 변경 될 수 등.). 
[관리 되는 호출 가능 래퍼](~/android/internals/architecture.md) 하므로 추가 인스턴스 멤버를 추가 하지 마십시오 보유 하는 경우는 [Android.Graphics.Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) 는 메모리의 10MB blob을 참조 하는 인스턴스 Xamarin.Android의 GC 사실을 &ndash; GC 20 바이트 개체 표시 되 고 10 MB의 메모리만 활성화 하는 유지 하는 Android 런타임 할당 된 개체에 연결 되어 있음을 확인할 수 없습니다. 

GC를 자주 해야만 됩니다. 그러나 *GC 합니다. AddMemoryPressure()* 및 *GC 합니다. RemoveMemoryPressure()* 지원 되지 않으며, 하므로 있습니다 *알고* 큰 Java에서 할당 된 개체 그래프를 직접 호출 하도록 할 수 방금 해제 하는 [GC 합니다. Collect ()](xref:System.GC.Collect) GC Java 측을 해제 하기 위해이 메시지에 메모리 또는 있습니다 수를 명시적으로 삭제 *Java.Lang.Object* 관리 되는 호출 가능 래퍼 및 Java 인스턴스 간의 매핑을 주요 하위 클래스입니다. 예를 들어 참조 [버그 1084](http://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6)합니다. 


> [!NOTE]
> 여야 *매우* 삭제할 때 신중 하 게 `Java.Lang.Object` 하위 클래스 인스턴스.

호출할 때 메모리 손상 가능성을 최소화 하려면 다음 지침을 준수 `Dispose()`합니다.


#### <a name="sharing-between-multiple-threads"></a>여러 스레드 간에 공유합니다.

경우는 *Java 또는 관리 되는* 인스턴스가 여러 스레드 간에 공유할 수 *수 없도록 `Dispose()`d*, **적이**합니다. 예를 들어 [ `Typeface.Create()` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/(System.String%2cAndroid.Graphics.TypefaceStyle)) 반환할 수 있습니다는 *캐시 된 인스턴스*합니다. 프로그램이 가져올 여러 스레드가 동일한 인수를 제공 하는 경우는 *동일한* 인스턴스. 따라서 `Dispose()`의 ing는 `Typeface` 인스턴스 한 스레드에서 발생할 수 있는 다른 스레드에 무효화 될 수 있습니다 `ArgumentException`에서 s `JNIEnv.CallVoidMethod()` 등 다른 스레드에서 인스턴스가 삭제 되었습니다. 


#### <a name="disposing-bound-java-types"></a>바인딩된 Java 형식 삭제

인스턴스를 삭제할 수 인스턴스가 바인딩된 Java 형식의 경우 *으로* 관리 코드에서 인스턴스를 다시 사용할 수 없습니다 *및* Java 인스턴스 (참조 이전 스레드간에공유될수없습니다`Typeface.Create()` 토론). (이러한 결정 어려울 수 있습니다.) 관리 코드, Java 인스턴스 입력 다음에 *새* 래퍼에 대 한 생성 됩니다. 

Drawables 및 다른 리소스 집약적 인스턴스에 관한 자주 유용 합니다.

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

위 그림은 안전 하 게 보호 하기 때문에 피어 하는 [Drawable.CreateFromPath()](https://developer.xamarin.com/api/member/Android.Graphics.Drawables.Drawable.CreateFromPath/) 프레임 워크 피어 반환은 참조 *하지* 사용자 피어가 아닙니다. `Dispose()` 끝날 때 호출 된 `using` 블록은 관리 되는 관계의 연결이 끊어집니다 [Drawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.Drawable/) 및 프레임 워크 [Drawable](http://developer.android.com/reference/android/graphics/drawable/Drawable.html) Java 인스턴스가 될 수 있도록 인스턴스 Android 런타임 해야 하는 즉시 수집 합니다. 이 문자로 *하지* 사용자 피어에 피어 인스턴스 참조 한 경우 안전; "외부" 정보를 사용 하는지 여기 *알고* 하는 `Drawable` 사용자 피어를 참조할 수 없습니다 이므로 `Dispose()` 호출 이 안전 합니다. 


#### <a name="disposing-other-types"></a>다른 형식 삭제 

인스턴스 형식을 참조 하는 Java 유형의 바인딩을 없는 경우 (사용자 지정 등 `Activity`), **하지 않으면** 호출 `Dispose()` 하지 않는 한 있습니다 *알고* Java 코드 없이 그에 재정의 된 메서드를 호출 합니다 인스턴스입니다. 그렇게 하지 못하면 [ `NotSupportedException`s](~/android/internals/architecture.md#Premature_Dispose_Calls)합니다. 

예를 들어 있는 경우 사용자 지정 수신기 클릭 합니다.

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

하면 *하지 않아야* Java는 나중에 메서드를 호출 하려고 하는 대로이 인스턴스를 삭제 합니다.

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```


#### <a name="using-explicit-checks-to-avoid-exceptions"></a>명시적 검사를 사용 하 여 예외를 방지 하기

구현한 경우는 [Java.Lang.Object.Dispose](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose(System.Boolean)/) 메서드를 오버 로드, JNI와 관련 된 개체를 변경 하지 마십시오. 발생할 수 있으므로 한 *더블 dispose* 상황 수 있는 코드의 (열리지)가 이미 가비지 수집 하는 기본 Java 개체에 액세스 하려고 합니다. 이렇게 다음과 유사한 예외가 생성 합니다. 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

이러한 상황을 종종 개체의 첫 번째 dispose null 되도록 멤버 시키고 유효성 검사 다음 null이 멤버에서 후속 액세스 시도 하면 예외가 throw 될 때 발생 합니다. 특히, 개체의 `Handle` 에 첫 번째 dispose (에 기본 Java 인스턴스는 연결 관리 되는 인스턴스)가 무효화 하지 않지만 관리 코드를 더 이상 사용할 수 있는 (참조 는것이기본Java인스턴스에액세스하려면시도[ 관리 되는 호출 가능 래퍼](~/android/internals/architecture.md#Managed_Callable_Wrappers) Java 인스턴스와 관리 되는 인스턴스 간 매핑에 대 한 자세한 정보에 대 한). 

이 예외를 방지 하는 좋은 방법에서 명시적으로 확인 하는 프로그램 `Dispose` 관리 되는 인스턴스 및 기본 Java 인스턴스 간의 매핑을 여전히 유효한; 즉 메서드, 있는지 확인할 개체의 `Handle` null (`IntPtr.Zero`) 해당 멤버에 액세스 합니다. 예를 들어, 다음 `Dispose` 메서드 액세스는 `childViews` 개체: 

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

초기 dispose 전달 원인을 `childViews` 잘못 된 있어야 `Handle`, `for` 루프 액세스를 throw 합니다는 `ArgumentException`합니다. 명시적으로 추가 하 여 `Handle` 첫 번째 확인 null `childViews` 에 액세스 하 고 다음 `Dispose` 메서드 않도록 예외 발생에서: 

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

때마다의 인스턴스는 `Java.Lang.Object` 형식이 나 하위 클래스는 전체 GC 중 검색 된 *개체 그래프* 인스턴스 가리키는 검사 해야 합니다. 개체 그래프는 "루트 인스턴스" 가리키는 개체 인스턴스 집합이 *플러스* 를 재귀적으로 루트 인스턴스에 의해 참조 된 모든 항목을 참조 합니다. 

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

때 `BadActivity` 가 생성 되 면 개체 그래프 인스턴스가 포함 될 10004 (1 x `BadActivity`, 1 x `strings`, 1 x `string[]` 소유한 `strings`, 문자열 인스턴스의 x 10000), *모든* 되도록 해야 합니다는의 검색 될 때마다는 `BadActivity` 인스턴스를 검색 합니다. 

좋지 않은 영향을 줄 수 있습니다 프로그램 컬렉션 시간에 향상 GC 일시 중지 시간. 

GC를 도울 수 있습니다 *감소* 사용자 피어 인스턴스에서 루트는 개체 그래프의 크기입니다. 위의 예제에서는 이렇게 이동 하 여 `BadActivity.strings` Java.Lang.Object에서 상속 하지 않습니다는 별도 클래스에: 

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

호출 하 여 보조 컬렉션을 수동으로 수행 될 수 [GC 합니다. Collect(0)](xref:System.GC.Collect)합니다. 보조 컬렉션은 저렴 (주요 컬렉션 비교) 하는 경우, 하지만 권한이 큰 고정 비용를 자주 트리거하 않도록 해야 하며, 몇 밀리초의 일시 중지 시간이 있습니다. 

응용 프로그램에 "duty 주기" 동일한 작업은 완료 하 반복 해 경우 duty 주기가 종료 된 후 수동으로 약간 컬렉션을 수행 하는 것이 좋습니다 수 있습니다. Duty 주기 예제는 다음과 같습니다. 

-  단일 게임 프레임의 렌더링 하는 주기 합니다.
-  지정 된 응용 프로그램 대화 상자 (열기, 닫기을 채운)와 전체 상호 작용 
-  그룹 새로 고침/앱 데이터 동기화에 대 한 네트워크 요청입니다.



## <a name="major-collections"></a>주요 컬렉션

호출 하 여 주요 컬렉션을 수동으로 수행 될 수 [GC 합니다. Collect ()](xref:System.GC.Collect) 또는 `GC.Collect(GC.MaxGeneration)`합니다. 

매우 드문 경우에 수행 해야 하며 512MB 힙 수집 하는 경우 1 초 일시 중지 시간이 스타일 Android 장치에 있을 수 있습니다. 

주요 컬렉션만 수동으로 호출할지를 요구 하는 경우: 

-   긴 duty 끝날 때 주기 및 long 일시 중지 하는 경우 사용자에 게 문제를 나타내지 않습니다. 

-   내에서 재정의 된 [Android.App.Activity.OnLowMemory()](https://developer.xamarin.com/api/member/Android.App.Activity.OnLowMemory/) 메서드. 



## <a name="diagnostics"></a>진단

전역 참조 생성 되 고 삭제 하는 경우를 추적 하려면 설정할 수 있습니다는 [debug.mono.log](~/android/troubleshooting/index.md) 포함 하는 시스템 속성 [ *gref* ](~/android/troubleshooting/index.md) 및/또는 [ *gc*](~/android/troubleshooting/index.md)합니다. 



## <a name="configuration"></a>구성

Xamarin.Android 가비지 수집기를 설정 하 여 구성할 수 있습니다는 `MONO_GC_PARAMS` 환경 변수입니다. 빌드 작업으로 환경 변수를 설정할 수 있습니다 [AndroidEnvironment](~/android/deploy-test/environment.md)합니다.

`MONO_GC_PARAMS` 환경 변수는 쉼표로 구분 된 목록이 다음 매개 변수: 

-   `nursery-size` = *크기* :는 학교의 크기를 설정 합니다. 바이트 단위로 지정 되며 2의 거듭제곱 이어야 합니다. 접미사 `k` , `m` 및 `g` kilo-, 메가-및 기가바이트, 각각 지정 데 사용할 수 있습니다. 학교 1 세대 (2)입니다. 더 큰 학교 일반적으로 프로그램을 신속 하 게 하지만 더 많은 메모리를 사용 하 여 분명 합니다. 기본 학교 512kb를 조정 합니다. 

-   `soft-heap-limit` = *크기* : 대상 최대 응용 프로그램에 대 한 메모리 소비량을 관리 합니다. 메모리 사용이 지정된 된 값 보다 낮으면 GC 실행 시간 (더 적은 컬렉션)에 대해 최적화 되어 있습니다. 
    이 제한을 초과 GC 메모리 사용량 (더 많은 컬렉션)에 대해 최적화 되어 있습니다. 

-   `evacuation-threshold` = *임계값* : 백분율에서 철수 임계값을 설정 합니다. 값에 0에서 100 사이의 정수 여야 합니다. 기본값은 66 합니다. 컬렉션의 스윕 단계는 특정 힙 블록 형식의 점유율은이 비율 보다 작으면를 발견 하는 경우 100% 가까이 점유율 복원 되는 다음 주요 컬렉션에 해당 블록 형식에 대 한 복사 컬렉션을 수행 합니다. 값이 0 철수를 해제합니다. 

-   `bridge-implementation` = *구현 브리지* : GC 브리지 옵션 성능 문제를 해결 GC 설정 합니다. 세 가지 가능한 값: *이전* , *새* , *tarjan*합니다.

-   `bridge-require-precise-merge`가비지 있게 되 면 먼저: Tarjan 브리지는 최적화 수 있는, 경우에 따라 개체를 포함 한 GC를 수집 합니다. 이 옵션을 포함 하 여 Gc를 잠재적으로 느린 하지만 예측 가능성이 더욱 뛰어난 만드는 최적화를 사용할 수 없습니다.

예를 들어 128MB의 힙 크기 한도를 갖도록 GC를 구성 하려면 사용 하 여 프로젝트에 새 파일 추가 **빌드 작업** 의 `AndroidEnvironment` 내용: 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
