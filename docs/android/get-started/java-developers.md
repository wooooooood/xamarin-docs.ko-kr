---
title: "Java 개발자를 위한 Xamarin"
description: "Java 개발자인 경우 Xamarin 플랫폼에서 기술과 기존 코드를 활용하는 동시에 C# 코드도 다시 사용할 수 있습니다. C# 구문은 Java 구문과 매우 비슷하며, 두 언어 모두에서 매우 비슷한 기능을 제공합니다. 또한 C# 고유의 기능을 통해 개발 작업을 더 쉽게 수행할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: A3B6C041-4052-4E7D-999C-C4FA10BE3D67
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 7abcaa218c6755a58e6f35e982a1144060df0b3b
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="xamarin-for-java-developers"></a>Java 개발자를 위한 Xamarin

_Java 개발자인 경우 Xamarin 플랫폼에서 기술과 기존 코드를 활용하는 동시에 C# 코드도 다시 사용할 수 있습니다. C# 구문은 Java 구문과 매우 비슷하며, 두 언어 모두에서 매우 비슷한 기능을 제공합니다. 또한 C# 고유의 기능을 통해 개발 작업을 더 쉽게 수행할 수 있습니다._


## <a name="overview"></a>개요

이 문서에서는 기본적으로 Xamarin.Android 응용 프로그램을 개발하는 동안 발생할 수 있는 C# 언어 기능에 집중하여 Java 개발자를 위한 C# 프로그래밍에 대해 소개합니다. 또한 이러한 기능과 해당 Java 기능의 차이점을 설명하고, Java에서 사용할 수 없는 중요한 C# 기능(Xamarin.Android와 관련됨)을 소개합니다. 추가 참조 자료에 대한 링크가 포함되어 있으므로 이 문서를 C# 및 .NET에 대한 추가 학습을 위한 "시작" 지점으로 사용할 수 있습니다.

Java에 익숙한 경우 집에서 C# 구문과 더불어 즉시 경험할 수 있습니다. C# 구문은 Java 구문과 매우 비슷하며, C#은 Java, C 및 C++와 같은 "중괄호" 언어입니다. C# 구문은 여러 가지 방식에서 Java 구문의 상위 집합처럼 읽지만, 몇 가지 키워드가 추가되고 이름이 바뀌었습니다.

C#에서 찾을 수 있는 Java의 많은 주요 특징은 다음과 같습니다.

-   클래스 기반 개체 지향 프로그래밍

-   강력한 형식화

-   인터페이스 지원

-   제네릭

-   가비지 수집

-   런타임 컴파일

Java 및 C#는 모두 관리되는 실행 환경에서 실행되는 중간 언어로 컴파일됩니다. 두 언어는 모두 형식을 정적으로 지정하며, 문자열을 변경할 수 없는 형식으로 처리합니다.
그리고 단일 루트 클래스 계층 구조를 사용합니다. Java와 마찬가지로, C#은 단일 상속만 지원하며 전역 메서드는 허용하지 않습니다.
두 언어 모두에서 개체는 `new` 키워드를 사용하여 힙에 만들어지고, 더 이상 사용되지 않으면 가비지 수집됩니다. 두 언어는 모두 `try`/`catch` 의미 체계가 포함된 공식적인 예외 처리 지원을 제공합니다. 그리고 스레드 관리 및 동기화 지원도 제공합니다.

하지만 Java와 C# 사이에는 많은 차이점이 있습니다. 예:

-   Java는 암시적으로 형식화된 지역 변수를 지원하지 않습니다(C#은 `var` 키워드를 지원함).

-   Java에서는 매개 변수를 값으로 전달할 수 있지만, C#에서는 값뿐만 아니라 참조로도 전달할 수 있습니다. (C#은 `ref` 및 `out` 키워드를 제공하여 매개 변수를 참조로 전달하지만, Java에는 이에 해당하는 키워드가 없습니다).

-   Java는 `#define`과 같은 전처리기 지시문을 지원하지 않습니다.

-   Java는 부호 없는 정수 형식을 지원하지 않지만, C는 `ulong`, `uint`, `ushort` 및 `byte`와 같은 부호 없는 정수 형식을 제공합니다.

-   Java는 연산자 오버로딩을 지원하지 않지만, C#에서는 연산자와 변환을 오버로드할 수 있습니다.

-   Java `switch` 문에서 코드는 다음 switch 섹션으로 진행할 수 있지만, C#에서는 각 `switch` 섹션의 끝에서 switch를 종료해야 합니다(즉, 각 섹션의 끝을 `break` 문으로 닫아야 함).

-   Java에서는 `throws` 키워드를 사용하여 메서드에서 throw되는 예외를 지정하지만, C#에는 확인된 예외에 대한 개념이 없습니다. 따라서 C#에서는 `throws` 키워드를 지원하지 않습니다.

-   C#은 데이터베이스 쿼리와 비슷한 방식으로 `from`, `select` 및 `where` 예약어를 사용하여 컬렉션에 대한 쿼리를 작성할 수 있는 LINQ(Language-Integrated Query)를 지원합니다.


물론 C#과 Java 사이에는 이 문서에서 설명하는 것보다 더 많은 차이점이 있습니다. 또한 Android 도구 체인에 아직 없는 Java 8에서 C# 스타일의 람다 식을 지원하는 것처럼 Java와 C# 모두는 계속 진화하고 있으므로 시간이 지남에 따라 이러한 차이점은 변합니다. 현재 Xamarin.Android를 처음 사용하는 Java 개발자가 직면하는 가장 중요한 차이점만 여기서 설명합니다.

-   [Java에서 C#으로 개발 이동](#fundamentals)에서는 C#과 Java의 근본적인 차이점을 소개합니다.

-   [개체 지향 프로그래밍 기능](#oopfeatures)에서는 두 언어 사이의 가장 중요한 개체 지향 기능에 대한 차이점을 설명합니다.

-   [키워드 차이점](#keywords)에서는 유용한 키워드 대조 표, C# 전용 키워드 및 C# 키워드 정의에 대한 링크를 제공합니다.

C#은 Java 개발자가 현재 Android에서 사용할 수 없는 Xamarin.Android에 많은 주요 기능을 제공합니다. 다음과 같은 기능을 사용하면 더 짧은 시간에 더 효율적인 코드를 작성할 수 있습니다.

-   [속성](#properties) &ndash; C#의 속성 시스템을 사용하면 setter 및 getter 메서드를 작성하지 않고도 멤버 변수에 안전하게 직접 액세스할 수 있습니다.

-   [람다 식](#lambdas) &ndash; C#에서는 무명 메서드(*람다*라고도 함)를 사용하여 기능을 더 간결하고 더 효율적으로 표현할 수 있습니다. 일회용 개체를 작성해야 하는 오버헤드를 방지할 수 있으며, 매개 변수를 추가할 필요 없이 로컬 상태를 메서드에 전달할 수 있습니다.

-   [이벤트 처리](#events) &ndash; C#은 *이벤트 구동 프로그래밍*에 대한 언어 수준 지원을 제공합니다. 이 경우 개체는 관심 있는 이벤트가 발생할 때 알림을 받도록 등록할 수 있습니다. `event` 키워드는 게시자 클래스에서 이벤트 구독자에게 알리는 데 사용할 수 있는 멀티캐스트 브로드캐스트 메커니즘을 정의합니다.

-   [비동기 프로그래밍](#async) &ndash; C#의 비동기 프로그래밍 기능(`async`/`await`)은 응용 프로그램이 즉시 응답할 수 있도록 합니다.
    이 기능의 언어 수준 지원을 통해 비동기 프로그래밍을 쉽게 구현할 수 있으며 오류가 발생할 가능성이 적습니다.


마지막으로, Xamarin에서는 *바인딩*으로 알려진 기술을 통해 [기존 Java 자산을 활용할 수 있습니다](#interop). Xamarin의 자동 바인딩 생성기를 사용하여 C#에서 기존 Java 코드, 프레임워크 및 라이브러리를 호출할 수 있습니다. 이렇게 하려면 Java에서 정적 라이브러리를 만들고 바인딩을 통해 C#에 노출하면 됩니다.

<a name="fundamentals" />

## <a name="going-from-java-to-c-development"></a>Java에서 C#으로 개발 이동

다음 섹션에서는 C#과 Java의 기본적인 "시작" 차이점에 대해 설명하고, 이후 섹션에서는 이러한 언어 간의 개체 지향 차이점에 대해 설명합니다.



### <a name="libraries-vs-assemblies"></a>라이브러리 및 어셈블리

Java는 일반적으로 **.jar** 파일에 관련 클래스를 패키지합니다. 그러나 C# 및 .NET에서는 미리 컴파일된 코드의 재사용 가능한 비트가 일반적으로 *.dll* 파일이라는 *어셈블리*에 패키지됩니다. 어셈블리는 C#/.NET 코드의 배포 단위이며, 각 어셈블리는 일반적으로 C# 프로젝트와 연결됩니다. 어셈블리에는 런타임에 컴파일된 JIT(Just-In-Time)인 IL(중간 코드)이 포함되어 있습니다.

어셈블리에 대한 자세한 내용은 MSDN [어셈블리와 전역 어셈블리 캐시](https://msdn.microsoft.com/en-us/library/ms173099.aspx) 항목을 참조하세요.


### <a name="packages-vs-namespaces"></a>패키지 대 네임스페이스

C#은 Java의 `package` 키워드와 비슷한 `namespace` 키워드를 사용하여 관련 형식을 그룹화합니다. 일반적으로 Xamarin.Android 앱은 해당 앱용으로 만든 네임스페이스에 상주합니다. 예를 들어 다음 C# 코드에서는 날씨 보고 앱에 대한 `WeatherApp` 네임스페이스 래퍼를 선언합니다.

```csharp
namespace WeatherApp
{
    ...
```


### <a name="importing-types"></a>형식 가져오기

외부 네임스페이스에 정의된 형식을 사용하는 경우 `using` 문(Java `import` 문과 매우 비슷함)을 사용하여 이러한 형식을 가져옵니다. Java에서는 다음 명령문을 사용하여 단일 형식을 가져올 수 있습니다.

```java
import javax.swing.JButton
```

다음 명령문을 사용하여 Java 패키지 전체를 가져올 수 있습니다.

```java
import javax.swing.*
```

C# `using` 문은 매우 비슷한 방식으로 작동하지만, 와일드카드를 지정하지 않고 패키지 전체를 가져올 수 있습니다. 예를 들어 Xamarin.Android 원본 파일의 시작 부분에 다음과 같은 일련의 `using` 문이 자주 표시됩니다.

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Net;
using System.IO;
using System.Json;
using System.Threading.Tasks;
```

이러한 명령문은 `System`, `Android.App`, `Android.Content` 등의 네임스페이스에서 기능을 가져옵니다.



### <a name="generics"></a>제네릭

Java와 C#은 모두 *제네릭*을 지원하며, 이는 컴파일 시간에 다른 형식을 플러그 인할 수 있는 자리 표시자입니다. 그러나 C#에서는 제네릭이 약간 다르게 작동합니다. Java에서 [형식 지우기](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html)는 컴파일 시간에만 형식 정보를 사용할 수 있게 하지만, 런타임에는 그렇지 않습니다. 반대로 .NET CLR(공용 언어 런타임)은 제네릭 형식을 명시적으로 지원하며, 이에 따라 C#에서 런타임에 형식 정보에 액세스할 수 있습니다. 일상적인 Xamarin.Android 개발에서 이 구분의 중요성은 명확하지 않지만, [리플렉션](https://msdn.microsoft.com/en-us/library/ms173183.aspx)을 사용하는 경우 이 기능을 사용하여 런타임에 형식 정보에 액세스할 수 있게 됩니다.

Xamarin.Android에서는 레이아웃 컨트롤에 대한 참조를 가져오는 데 사용되는 `FindViewById` 제네릭 메서드를 자주 볼 수 있습니다. 이 메서드는 조회하는 컨트롤의 형식을 지정하는 제네릭 형식 매개 변수를 허용합니다. 예:

```csharp
TextView label = FindViewById<TextView> (Resource.Id.Label);
```

이 코드 예제에서 `FindViewById`는 레이아웃에서 **label**로 정의된 `TextView` 컨트롤에 대한 참조를 가져온 다음, `TextView` 형식으로 반환합니다.

제네릭에 대한 자세한 내용은 MSDN [제네릭](https://msdn.microsoft.com/en-us/library/512aeb7t.aspx) 항목을 참조하세요.
제네릭 C# 클래스에 대한 Xamarin.Android 지원에는 몇 가지 제한 사항이 있습니다. 자세한 내용은 [제한 사항](~/android/internals/limitations.md)을 참조하세요.


<a name="oopfeatures" />

## <a name="object-oriented-programming-features"></a>개체 지향 프로그래밍 기능

Java와 C# 모두는 다음과 같이 매우 비슷한 개체 지향 프로그래밍 관용구를 사용합니다.

-   모든 클래스는 궁극적으로 단일 루트 개체에서 파생됩니다. 즉 모든 Java 개체는 `java.lang.Object`에서 파생되지만, 모든 C# 개체는 `System.Object`에서 파생됩니다.

-   클래스의 인스턴스는 참조 형식입니다.

-   인스턴스의 속성 및 메서드에 액세스하는 경우 "<code>.</code>" 연산자를 사용합니다.

-   모든 클래스 인스턴스는 `new` 연산자를 통해 힙에 만들어집니다.

-   두 언어 모두에서 가비지 수집을 사용하므로 사용되지 않는 개체를 명시적으로 해제하는 방법이 있습니다(즉, C++에 있는 것과 같은 `delete` 키워드가 없음).

-   상속을 통해 클래스를 확장할 수 있으며, 두 언어는 모두 형식별로 하나의 기본 클래스만 허용합니다.

-   인터페이스를 정의할 수 있으며, 클래스는 여러 인터페이스 정의로부터 상속(즉, 구현)할 수 있습니다.

그러나 다음과 같이 몇 가지 중요한 차이점도 있습니다.

-   Java에는 C#에서 지원하지 않는 강력한 두 가지 기능인 익명 클래스와 내부 클래스가 있습니다. 그러나 C#은 클래스 정의의 중첩을 허용하며, C#의 중첩 클래스는 Java의 정적 중첩 클래스와 같습니다.

-   C#은 C 스타일 구조체 형식(`struct`)을 지원하지만, Java는 그렇지 않습니다.

-   C#에서는 `partial` 키워드를 사용하여 별도의 원본 파일에 클래스 정의를 구현할 수 있습니다.

-   C# 인터페이스는 필드를 선언할 수 없습니다.

-   C#은 C++ 스타일 소멸자 구문을 사용하여 종료자를 표현합니다. 이 구문은 Java의 `finalize` 메서드와 다르지만 의미는 거의 같습니다. (`super.finalize`에 대한 명시적 호출이 사용되는 Java와 달리, C#에서는 소멸자가 기본 클래스 소멸자를 자동으로 호출합니다.)



### <a name="class-inheritance"></a>클래스 상속

Java에서 클래스를 확장하려면 `extends` 키워드를 사용합니다. C#에서 클래스를 확장하려면 콜론(`:`)을 사용하여 파생을 나타냅니다. 예를 들어 Xamarin.Android 앱에는 종종 다음 코드 조각과 비슷한 클래스 파생이 표시됩니다.

```csharp
public class MainActivity : Activity
{
    ...
```

이 예제에서 `MainActivity`는 `Activity` 클래스에서 상속받습니다.

Java에서 인터페이스에 대한 지원을 선언하려면 `implements` 키워드를 사용합니다. 그러나 C#에서는 다음 코드 조각과 같이 상속받을 클래스 목록에 인터페이스 이름을 추가하기만 하면 됩니다.

```csharp
public class SensorsActivity : Activity, ISensorEventListener
{
    ...
```

이 예제에서 `SensorsActivity`는 `Activity`에서 상속받고 `ISensorEventListener` 인터페이스에 선언된 기능을 구현합니다. 인터페이스 목록은 기본 클래스 뒤에 나와야 합니다(그렇지 않으면 컴파일 시간 오류가 발생함). 규칙에 따라 C# 인터페이스 이름 앞에는 "I" 대문자가 붙습니다. 이렇게 하면 `implements` 키워드가 없어도 인터페이스의 클래스를 확인할 수 있습니다.

C#에서 클래스가 더 이상 서브클래싱되지 않도록 하려면, 클래스 이름 앞에 `sealed`를 붙입니다. Java에서는 클래스 이름 앞에 `final`을 붙입니다.

C# 클래스 정의에 대한 자세한 내용은 MSDN [클래스 및 구조체](https://msdn.microsoft.com/en-us/library/x9afc042.aspx) 및 [상속](https://msdn.microsoft.com/en-us/library/x9afc042.aspx) 항목을 참조하세요.


<a name="properties" />

### <a name="properties"></a>속성

Java에서 변경자(mutator) 메서드(setter)와 검사기(inspector) 메서드(getter)는 외부 코드에서 클래스 멤버를 숨기고 보호하면서 이러한 클래스 멤버를 변경하는 방식을 제어하는 데 자주 사용됩니다. 예를 들어 Android `TextView` 클래스는 `getText` 및 `setText` 메서드를 제공합니다. C#은 *속성*으로 알려진 비슷하지만 더 직접적인 메커니즘을 제공합니다.
C# 클래스의 사용자는 필드에 액세스하는 것과 같은 방식으로 속성에 액세스할 수 있지만, 각 액세스는 실제로 호출자에게 투명한 메서드 호출을 발생시킵니다. 이 "숨겨진(under the covers)" 메서드는 다른 값 설정, 변환 수행 또는 개체 상태 변경과 같은 부작용을 구현할 수 있습니다.

속성은 UI(사용자 인터페이스) 개체 멤버를 액세스하고 수정하는 데 자주 사용됩니다. 예:

```csharp
int width = rulerView.MeasuredWidth;
int height = rulerView.MeasuredHeight;
...
rulerView.DrawingCacheEnabled = true;
```

이 예제에서 너비와 높이 값은 해당 `MeasuredWidth` 및 `MeasuredHeight` 속성에 액세스하여 `rulerView` 개체에서 읽습니다. 이러한 속성을 읽으면 연결된(그러나 숨겨진) 필드 값의 값이 장면 뒤에서 가져와 호출자에게 반환됩니다. `rulerView` 개체는 너비와 높이 값을 한 측정 단위(예: 픽셀)로 저장하고, `MeasuredWidth`와 `MeasuredHeight` 속성에 액세스할 때 이러한 값을 다른 측정 단위(예: 밀리미터)로 변환합니다.

또한 `rulerView` 개체는 `DrawingCacheEnabled`라는 속성을 가지고 있으며, 예제 코드에서는 이 속성을 `true`로 설정하여 `rulerView`에서 그리기 캐시를 사용할 수 있도록 합니다. 장면 뒤에서 연결되었지만 숨겨진 필드가 새 값으로 업데이트되고, `rulerView` 상태의 다른 측면이 수정됩니다. 예를 들어 `DrawingCacheEnabled`가 `false`로 설정되면 `rulerView`는 개체에 이미 누적된 그리기 캐시 정보를 모두 지울 수도 있습니다.

속성에 대한 액세스는 읽기/쓰기, 읽기 전용 또는 쓰기 전용일 수 있습니다. 또한 읽기 및 쓰기에 대해 다른 액세스 한정자를 사용할 수 있습니다. 예를 들어 공용 읽기 액세스 권한이 있지만 개인 쓰기 액세스 권한이 있는 속성을 정의할 수 있습니다.

C# 속성에 대한 자세한 내용은 MSDN [속성](https://msdn.microsoft.com/en-us/library/x9fsa0sw.aspx) 항목을 참조하세요.



### <a name="calling-base-class-methods"></a>기본 클래스 메서드 호출

C#에서 기본 클래스 생성자를 호출하려면 콜론(`:`) 뒤에 `base` 키워드와 이니셜라이저 목록을 사용합니다. 이 `base` 생성자 호출은 파생 생성자 매개 변수 목록 바로 뒤에 배치됩니다. 기본 클래스 생성자는 파생 생성자에 대한 항목에서 호출됩니다. 컴파일러는 메서드 본문의 시작 부분에 기본 생성자에 대한 호출을 삽입합니다. 다음 코드 조각에서는 Xamarin.Android 앱의 파생 생성자에서 호출되는 기본 생성자를 보여 줍니다.

```csharp
public class PictureLayout : ViewGroup
{
    ...
    public class PictureLayout (Context context)
           : base (context)
    {
        ...
    }
    ...
}

```

이 예제에서 `PictureLayout` 클래스는 `ViewGroup` 클래스에서 파생됩니다. 이 예제에 표시된 `PictureLayout` 생성자는 `context` 인수를 수락하고 `base(context)` 호출을 통해 `ViewGroup` 생성자에 전달합니다.

C#에서 기본 클래스 메서드를 호출하려면 `base` 키워드를 사용합니다. 예를 들어 Xamarin.Android 앱은 다음과 같이 기본 메서드를 호출하는 경우가 많습니다.

```csharp
public class MainActivity : Activity
{
    ...
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
```

이 경우 파생 클래스(`MainActivity`)에서 정의된 `OnCreate` 메서드는 기본 클래스(`Activity`)의 `OnCreate` 메서드를 호출합니다.



### <a name="access-modifiers"></a>액세스 한정자

Java 및 C#은 모두 `public`, `private` 및 `protected` 액세스 한정자를 지원합니다. 그러나 C#은 다음 두 가지 액세스 한정자를 추가로 지원합니다.

-   **`internal`** &ndash; 클래스 멤버는 현재 어셈블리 내에서만 액세스할 수 있습니다.

-   **`protected internal`** &ndash; 클래스 멤버는 정의 어셈블리, 정의 클래스 및 파생 클래스(어셈블리 내부와 외부 모두에서 파생된 클래스에 액세스할 수 있음) 내에서 액세스할 수 있습니다.

C# 액세스 한정자에 대한 자세한 내용은 MSDN [액세스 한정자](https://msdn.microsoft.com/en-us/library/ms173121.aspx) 항목을 참조하세요.



### <a name="virtual-and-override-methods"></a>가상 및 재정의 메서드

Java와 C#은 모두 *다형성*을 지원합니다. 이는 비슷한 방식으로 관련 개체를 처리하는 기능입니다. 두 언어 모두에서 기본 클래스 참조를 사용하여 파생 클래스 개체를 참조할 수 있으며, 파생 클래스의 메서드는 기본 클래스의 메서드를 재정의할 수 있습니다. 두 언어에는 모두 파생 클래스의 메서드로 교체되도록 설계된 기본 클래스의 메서드인 *가상* 메서드에 대한 개념이 있습니다.
Java와 마찬가지로 C#은 `abstract` 클래스와 메서드를 지원합니다.

그러나 가상 메서드를 선언하고 이를 재정의하는 방법에 있어 Java와 C# 사이에는 몇 가지 차이점이 있습니다.

-   C#에서는 메서드가 기본적으로 가상이 아닙니다. 부모 클래스는 `virtual` 키워드를 사용하여 재정의할 메서드에 대한 레이블을 명시적으로 지정해야 합니다. 반대로, Java의 모든 메서드는 기본적으로 가상 메서드입니다.

-   C#에서 메서드가 재정의되지 않도록 하려면 `virtual` 키워드를 사용하지 않습니다. 반대로, Java는 `final` 키워드를 사용하여 메서드를 "재정의가 허용되지 않습니다."로 표시합니다.

-   C# 파생 클래스는 `override` 키워드를 사용하여 가상 기본 클래스 메서드가 재정의되고 있음을 명시적으로 나타내야 합니다.

C#의 다형성 지원에 대한 자세한 내용은 MSDN [다형성](https://msdn.microsoft.com/en-us/library/ms173152.aspx) 항목을 참조하세요.


<a name="lambdas" />

## <a name="lambda-expressions"></a>람다 식

C#을 사용하면 인라인 무명 메서드가 포함된 메서드의 상태에 액세스할 수 있는 *클로저*를 만들 수 있습니다.
람다 식을 사용하면 더 적은 코드 줄을 작성하여 Java에서 더 많은 코드 줄로 구현한 것과 동일한 기능을 구현할 수 있습니다.

람다 식을 사용하면 Java에서와 같이 일회용 클래스 또는 익명 클래스를 만드는 데 필요한 추가 절차를 건너뛸 수 있습니다. 대신, 메서드 코드의 비즈니스 논리를 인라인으로 작성할 수 있습니다. 또한 람다는 주변 메서드의 변수에 액세스할 수 있으므로 메서드 코드에 상태를 전달할 긴 매개 변수 목록을 만들 필요가 없습니다.

C#에서 람다 식은 다음과 같이 `=>` 연산자를 사용하여 만듭니다.

```csharp
(arg1, arg2, ...) => {
    // implementation code
};
```

Xamarin.Android에서 람다 식은 이벤트 처리기를 정의하는 데 자주 사용됩니다. 예:

```csharp
button.Click += (sender, args) => {
    clickCount += 1;    // access variable in surrounding code
    button.Text = string.Format ("Clicked {0} times.", clickCount);
};
```

이 예제에서 람다 식 코드(중괄호 안의 코드)는 클릭 수를 늘리고 `button` 텍스트를 업데이트하여 클릭 수를 표시합니다. 이 람다 식은 단추를 탭할 때마다 호출되는 클릭 이벤트 처리기로 `button` 개체에 등록됩니다. (이벤트 처리기에 대한 자세한 내용은 아래에서 설명합니다.) 이 간단한 예제에서 `sender` 및 `args` 매개 변수는 람다 식 코드에서 사용하지 않지만, 이러한 매개 변수는 이벤트 등록에 대한 메서드 시그니처 요구 사항을 충족하기 위해 람다 식에 필요합니다. 내부적으로 C# 컴파일러는 람다 식을 단추 클릭 이벤트가 발생할 때마다 호출되는 무명 메서드로 변환합니다.

C# 및 람다 식에 대한 자세한 내용은 MSDN [람다 식](https://msdn.microsoft.com/en-us/library/bb397687.aspx) 항목을 참조하세요.


<a name="events" />

## <a name="event-handling"></a>이벤트 처리

*이벤트*는 개체에서 해당 개체에 흥미로운 일이 발생했을 때 등록된 구독자에게 알리는 방법입니다. 일반적으로 구독자가 콜백 메서드가 포함된 `Listener` 인터페이스를 구현하는 Java와 달리, C#은 *대리자*를 통한 이벤트 처리에 대한 언어 수준 지원을 제공합니다. *대리자*는 개체 지향 형식 안전 함수 포인터와 같으며, 개체 참조 및 메서드 토큰을 캡슐화합니다. 클라이언트 개체가 이벤트에 가입되도록 하려면 대리자를 만들고 알림 개체에 대리자를 전달합니다.
이벤트가 발생할 때 알림 개체는 대리자 개체에서 나타내는 메서드를 호출하여 가입 클라이언트 개체에 이벤트를 알립니다. C#에서 이벤트 처리기는 기본적으로 대리자를 통해 호출되는 메서드일 뿐입니다.

대리자에 대한 자세한 내용은 MSDN [대리자](https://msdn.microsoft.com/en-us/library/ms173171.aspx) 항목을 참조하세요.

C#에서 이벤트는 *멀티캐스트*입니다. 즉 이벤트가 발생할 때 둘 이상의 수신기에서 알림을 받을 수 있습니다. 이 차이점은 Java와 C# 이벤트 등록 간의 구문상 차이를 고려할 때 알 수 있습니다. Java에서는 `SetXXXListener`를 호출하여 이벤트 알림을 등록하지만, C#에서는 `+=` 연산자를 사용하여 대리자를 이벤트 수신기 목록에 "추가"하여 이벤트 알림을 등록합니다.
Java에서는 `SetXXXListener`를 호출하여 등록을 취소하지만, C#에서는 `-=`를 사용하여 대리자를 수신기 목록에서 "제거"합니다.

Xamarin.Android에서 이벤트는 사용자가 UI 컨트롤에 특정 작업을 수행하는 경우 개체에 알리는 데 자주 사용됩니다. 일반적으로 UI 컨트롤에는 `event` 키워드를 사용하여 정의된 멤버가 있습니다. 해당 UI 컨트롤에서 이벤트를 구독하려면 대리자를 이러한 멤버에 연결합니다.

이벤트에 가입하려면 다음을 수행합니다.

1.  이벤트가 발생할 때 호출하려는 메서드를 참조하는 대리자 개체를 만듭니다.

2.  `+=` 연산자를 사용하여 가입하려는 이벤트에 대리자를 연결합니다.

다음 예제에서는 단추 클릭에 가입할 대리자를 정의합니다(`delegate` 키워드를 명시적으로 사용).
이 단추 클릭 처리기는 새 작업을 시작합니다.

```csharp
startActivityButton.Click += delegate {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

그러나 람다 식을 사용하여 `delegate` 키워드를 모두 건너뛰는 이벤트를 등록할 수도 있습니다. 예:

```csharp
startActivityButton.Click += (sender, e) => {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

이 예제에서 `startActivityButton` 개체에는 보낸 사람 및 이벤트 인수를 수락하고 void를 반환하는 특정 메서드 시그니처가 있는 대리자가 필요한 이벤트가 있습니다. 그러나 이러한 대리자 또는 해당 메서드를 명시적으로 정의하는 데 문제가 없기를 원하므로, `(sender, e)`을 사용하여 메서드의 시그니처를 선언하고 람다 식을 사용하여 이벤트 처리기의 본문을 구현합니다.
`sender` 및 `e` 매개 변수를 사용하지 않는 경우에도 이 매개 변수 목록을 선언해야 합니다.

대리자의 가입을 취소할 수는 있지만(`-=` 연산자를 통해) 람다 식의 가입은 취소할 수 없습니다. 이렇게 하려고 하면 메모리 누수가 발생할 수 있습니다. 처리기에서 이벤트 구독을 취소하지 않을 경우에만 람다 형태의 이벤트 등록을 사용합니다.

일반적으로 람다 식은 Xamarin.Android 코드에서 이벤트 처리기를 선언하는 데 사용됩니다. 이벤트 처리기를 선언하는 이 간단한 방법은 처음에는 신비롭게 보일 수 있지만, 코드를 작성하고 읽을 때 엄청난 시간을 절약합니다. 숙련도가 높아짐에 따라 Xamarin.Android 코드에서 자주 발생하는 이 패턴을 인식하는 데 익숙해지고, 응용 프로그램의 비즈니스 논리를 고려하는 데 더 많은 시간을 들이고 구문 오버헤드에 걸리는 시간을 줄일 수 있습니다.


<a name="async" />

## <a name="asynchronous-programming"></a>비동기 프로그래밍

*비동기 프로그래밍*은 응용 프로그램의 전반적인 응답성을 향상시키는 방법입니다. 비동기 프로그래밍 기능을 사용하면 시간이 오래 걸리는 작업에서 앱의 일부를 차단하는 한편 앱 코드의 나머지 부분은 계속 실행할 수 있습니다.
웹에 액세스하고, 이미지를 처리하고, 파일을 읽고 쓰는 작업은 비동기적으로 작성되지 않은 경우 전체 앱이 정지된 것처럼 보일 수 있는 작업의 예입니다.

C#에는 `async` 및 `await` 키워드를 통한 비동기 프로그래밍에 대한 언어 수준 지원이 포함되어 있습니다. 이러한 언어 기능을 사용하면 응용 프로그램의 기본 스레드를 차단하지 않고 장기 실행 작업을 수행하는 코드를 매우 쉽게 작성할 수 있습니다. 간단히 말해, 메서드에 대해 `async` 키워드를 사용하여 메서드의 코드가 비동기적으로 실행되고 호출자의 스레드를 차단하지 않음을 나타냅니다. `async`로 표시된 메서드를 호출하는 경우 `await` 키워드를 사용합니다. 컴파일러는 `await`를 메서드 실행이 백그라운드 스레드로 이동되는 지점으로 해석합니다(작업이 호출자에게 반환됨). 이 작업이 완료되면 코드의 `await` 지점에 있는 호출자의 스레드에서 코드 실행이 다시 시작되어 `async` 호출의 결과를 반환합니다. 규칙에 따라 비동기적으로 실행되는 메서드의 이름 뒤에는 `Async`가 붙습니다.

Xamarin.Android 응용 프로그램에서 `async`과 `await`는 일반적으로 UI 스레드를 해제하여 백그라운드 작업에서 장시간 실행되는 동안 사용자 입력(예: **취소** 단추 태핑)에 응답할 수 있도록 하는 데 사용됩니다.

다음 예제에서는 단추 클릭 이벤트 처리기에서 비동기 작업으로 인해 웹에서 이미지를 다운로드합니다.


```csharp
downloadButton.Click += downloadAsync;
...
async void downloadAsync(object sender, System.EventArgs e)
{
    webClient = new WebClient ();
    var url = new Uri ("http://photojournal.jpl.nasa.gov/jpeg/PIA15416.jpg");
    byte[] bytes = null;

    bytes = await webClient.DownloadDataTaskAsync(url);

    // display the downloaded image ...
```

이 예제에서 사용자가 `downloadButton` 컨트롤을 클릭하면 `downloadAsync` 이벤트 처리기에서 `WebClient` 개체 및 `Uri` 개체를 만들어 지정된 URL에서 이미지를 가져옵니다. 그런 다음, 이 URL을 사용하여 `WebClient` 개체의 `DownloadDataTaskAsync` 메서드를 호출하여 이미지를 검색합니다.

`downloadAsync`의 메서드 선언은 `async` 키워드 앞에 붙어 비동기적으로 실행되고 작업을 반환한다는 것을 나타냅니다. 또한 `DownloadDataTaskAsync`에 대한 호출 앞에는 `await` 키워드가 붙습니다. 앱은 `DownloadDataTaskAsync`가 완료되어 반환될 때까지 `await`가 표시된 지점에서 시작하는 이벤트 처리기의 실행을 백그라운드 스레드로 이동합니다.
한편 앱의 UI 스레드는 다른 컨트롤에 대한 사용자 입력 및 이벤트 처리기에 계속 응답할 수 있습니다. `DownloadDataTaskAsync`가 완료되면(몇 초가 걸릴 수 있음) `bytes` 변수가 `DownloadDataTaskAsync`에 대한 호출의 결과로 설정된 위치에서 실행이 다시 시작되고, 이벤트 처리기 코드의 나머지 부분에서 다운로드한 이미지를 호출자의 스레드(UI)에 표시합니다.

C#의 `async`/`await`에 대한 소개는 MSDN [Async 및 Await를 사용한 비동기 프로그래밍](https://msdn.microsoft.com/en-us/library/hh191443.aspx) 항목을 참조하세요.
Xamarin의 비동기 프로그래밍 기능 지원에 대한 자세한 내용은 [비동기 지원 개요](~/cross-platform/platform/async.md)를 참조하세요.


<a name="keywords" />

## <a name="keyword-differences"></a>키워드 차이점

Java에서 사용되는 많은 언어 키워드가 C#에서도 사용됩니다. 또한 이 표에 나열된 것과 같이 C#에서 동등하지만 다른 이름이 지정된 Java 키워드가 많이 있습니다.

|Java|C#|설명|
|---|---|---|
|`boolean`|[bool](https://msdn.microsoft.com/en-us/library/c8f5xwh7.aspx)|true 및 false 부울 값을 선언하는 데 사용됩니다.|
|`extends`|`:`|상속받을 클래스 및 인터페이스 앞에 나옵니다.|
|`implements`|`:`|상속받을 클래스 및 인터페이스 앞에 나옵니다.|
|`import`|[using](https://msdn.microsoft.com/en-us/library/zhdeatwt.aspx)|네임스페이스에서 형식을 가져오며, 네임스페이스 별칭을 만드는 데도 사용됩니다.|
|`final`|[sealed](https://msdn.microsoft.com/en-us/library/88c54tsw.aspx)|클래스 파생을 방지합니다. 메서드 및 속성이 파생 클래스에서 재정의되지 않도록 합니다.|
|`instanceof`|[is](https://msdn.microsoft.com/en-us/library/scekt9xw.aspx)|개체가 지정된 형식과 호환되는지 여부를 평가합니다.|
|`native`|[extern](https://msdn.microsoft.com/en-us/library/e59b22c5.aspx)|외부적으로 구현된 메서드를 선언합니다.|
|`package`|[namespace](https://msdn.microsoft.com/en-us/library/z2kcy19k.aspx)|관련 개체 집합의 범위를 선언합니다.|
|`T...`|[params T](https://msdn.microsoft.com/en-us/library/w5zay9db.aspx)|가변 개수의 인수를 사용하는 메서드 매개 변수를 지정합니다.|
|`super`|[base](https://msdn.microsoft.com/en-us/library/hfw7t1ce.aspx)|파생 클래스 내에서 부모 클래스의 멤버에 액세스하는 데 사용됩니다.|
|`synchronized`|[lock](https://msdn.microsoft.com/en-us/library/c5kehkcz.aspx)|잠금 획득 및 해제를 사용하여 중요한 코드 섹션을 래핑합니다.|


또한 C#에 고유하고 Java에 해당하는 키워드가 없는 키워드가 많이 있습니다. Xamarin.Android 코드는 다음 C# 키워드를 자주 사용합니다. 다음 표는 Xamarin.Android [샘플 코드](https://developer.xamarin.com/samples/android/all/)를 읽을 때 유용하게 참조할 수 있습니다.

|C#|설명|
|---|---|
|[as](https://msdn.microsoft.com/en-us/library/cscsdfbt.aspx)|호환되는 참조 형식 또는 null 사용 가능 형식 간의 변환을 수행합니다.|
|[async](https://msdn.microsoft.com/en-us/library/hh156513.aspx)|메서드 또는 람다 식이 비동기임을 지정합니다.|
|[await](https://msdn.microsoft.com/en-us/library/hh156528.aspx)|작업이 완료될 때까지 메서드 실행을 일시 중단합니다.|
|[byte](https://msdn.microsoft.com/en-us/library/5bdb6693.aspx)|부호 없는 8비트 정수 형식입니다.|
|[delegate](https://msdn.microsoft.com/en-us/library/900fyy8e.aspx)|메서드 또는 무명 메서드를 캡슐화하는 데 사용됩니다.|
|[enum](https://msdn.microsoft.com/en-us/library/sbbt4032.aspx)|명명된 상수 집합인 열거형을 선언합니다.|
|[event](https://msdn.microsoft.com/en-us/library/8627sbea.aspx)|게시자 클래스에서 이벤트를 선언합니다.|
|[fixed](https://msdn.microsoft.com/en-us/library/f58wzh21.aspx)|변수가 다시 할당되지 않도록 방지합니다.|
|`get`|속성 값을 검색하는 접근자 메서드를 정의합니다.|
|[in](https://msdn.microsoft.com/en-us/library/dd469484.aspx)|제네릭 인터페이스에서 더 적게 파생된 형식을 수락하는 매개 변수를 사용하도록 설정합니다.|
|[object](https://msdn.microsoft.com/en-us/library/9kkx3h3c.aspx)|.NET Framework의 개체 형식에 대한 별칭입니다.|
|[out](https://msdn.microsoft.com/en-us/library/t3c3bfhx.aspx)|매개 변수 한정자 또는 제네릭 형식 매개 변수 선언입니다.|
|[override](https://msdn.microsoft.com/en-us/library/ebca9ah3.aspx)|상속된 멤버의 구현을 확장하거나 수정합니다.|
|[partial](https://msdn.microsoft.com/en-us/library/6b0scde8.aspx)|정의를 선언하여 여러 파일로 분할하거나 메서드 정의를 해당 구현에서 분할합니다.|
|[readonly](https://msdn.microsoft.com/en-us/library/acdd6hb7.aspx)|클래스 멤버가 선언 시에만 또는 클래스 생성자에서 할당될 수 있음을 선언합니다.|
|[ref](https://msdn.microsoft.com/en-us/library/14akc2c7.aspx)|인수를 값이 아닌 참조로 전달하도록 합니다.|
|[set](https://msdn.microsoft.com/en-us/library/ms228368.aspx)|속성 값을 설정하는 접근자 메서드를 정의합니다.|
|[string](https://msdn.microsoft.com/en-us/library/362314fe.aspx)|.NET Framework의 문자열 형식에 대한 별칭입니다.|
|[struct](https://msdn.microsoft.com/en-us/library/ah19swz4.aspx)|관련 변수 그룹을 캡슐화하는 값 형식입니다.|
|[typeof](https://msdn.microsoft.com/en-us/library/58918ffs.aspx)|개체의 형식을 가져옵니다.|
|[var](https://msdn.microsoft.com/en-us/library/bb383973.aspx)|암시적으로 형식화된 지역 변수를 선언합니다.|
|[value](https://msdn.microsoft.com/en-us/library/a1khb4f8.aspx)|클라이언트 코드에서 속성에 할당하려는 값을 참조합니다.|
|[virtual](https://msdn.microsoft.com/en-us/library/9fkccyh4.aspx)|파생 클래스에서 메서드를 재정의할 수 있도록 합니다.|


<a name="interop" />

## <a name="interoperating-with-existing-java-code"></a>기존 Java 코드와의 상호 운용

C#으로 변환하지 않으려는 기존 Java 기능이 있는 경우 Xamarin.Android 응용 프로그램에서 두 가지 방법을 통해 기존 Java 라이브러리를 다시 사용할 수 있습니다.

-  **Java 바인딩 라이브러리 만들기** &ndash; 이 방법을 사용하면 Xamarin 도구를 사용하여 Java 형식에 대한 C# 래퍼를 생성할 수 있습니다. 이러한 래퍼를 *바인딩*이라고 합니다. 따라서 Xamarin.Android 응용 프로그램은 이러한 래퍼로 호출하여 *.jar* 파일을 사용할 수 있습니다.

-  **Java 기본 인터페이스** &ndash; *JNI(Java 기본 인터페이스)*는 C# 앱에서 Java 코드로 호출하거나 호출할 수 있게 하는 프레임워크입니다.

이러한 방법에 대한 자세한 내용은 [Java 통합 개요](~/android/platform/java-integration/index.md)를 참조하세요.



## <a name="for-further-reading"></a>추가 정보

MSDN [C# 프로그래밍 가이드](https://msdn.microsoft.com/en-us/library/67ef8sbd.aspx)는 C# 프로그래밍 언어 학습을 시작할 수 있는 좋은 방법이며, [C# 참조](https://msdn.microsoft.com/en-us/library/618ayhy6.aspx)를 사용하여 특정 C# 언어 기능을 조회할 수 있습니다.

Java 지식에서 최소한 Java 언어를 알고 있는 만큼 Java 클래스 라이브러리에 대해 익숙해야 한다는 점과 마찬가지로, C#에 대한 실질적인 지식에서는 .NET 프레임워크에 익숙해야 합니다. Microsoft의 [Java 개발자를 위한 C# 및 .NET Framework로의 이동](https://www.microsoft.com/en-us/download/details.aspx?id=6073) 학습 패킷은 Java 관점에서 .NET 프레임워크에 대해 자세히 알아볼 수 있는 좋은 방법입니다(C#에 대한 깊은 이해를 얻을 수 있음).

C#에서 첫 번째 Xamarin.Android 프로젝트를 시작할 준비가 되면, [Hello, Android](~/android/get-started/hello-android/index.md) 시리즈를 통해 첫 번째 Xamarin.Android 응용 프로그램을 빌드하고 Xamarin을 사용하여 Android 응용 프로그램 개발의 기본 사항에 대한 이해를 높일 수 있습니다.



## <a name="summary"></a>요약

이 문서에서는 Java 개발자의 관점에서 Xamarin.Android C# 프로그래밍 환경을 소개했습니다. 실용적인 차이점을 설명하면서 C#과 Java 사이의 유사성을 지적했습니다. 어셈블리와 네임스페이스를 소개하고, 외부 형식을 가져오는 방법을 설명하고, 액세스 한정자, 제네릭, 클래스 파생, 호출 기본 클래스 메서드, 메서드 재정의 및 이벤트 처리의 차이점에 대한 개요를 제공했습니다. Java에서 사용할 수 없는 C# 기능(예: 속성, `async`/`await` 비동기 프로그래밍, 람다, C# 대리자 및 C# 이벤트 처리 시스템)을 소개했습니다. 중요한 C# 키워드 표가 있으며, 기존 Java 라이브러리와 상호 운용하는 방법에 대해 설명하고, 추가 정보와 관련된 설명서에 대한 링크를 제공했습니다.


## <a name="related-links"></a>관련 링크

- [Java 통합 개요](~/android/platform/java-integration/index.md)
- [C# 프로그래밍 가이드](https://msdn.microsoft.com/en-us/library/67ef8sbd.aspx)
- [C# 참조](https://msdn.microsoft.com/en-us/library/618ayhy6.aspx)
- [Java 개발자를 위한 C# 및 .NET Framework로의 이동](https://www.microsoft.com/en-us/download/details.aspx?id=6073)
