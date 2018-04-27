---
title: Objective-C 개발자용 C# 입문서
description: Xamarin.iOS를 사용하면 C#에서 작성된 플랫폼 제약 없는 코드를 플랫폼 간에 공유할 수 있습니다. 그러나 기존 iOS 응용 프로그램은 이미 만들어진 Objective-C 코드를 활용하려 할 수 있습니다. 이 문서는 Xamarin 및 C# 언어로 이동하려는 Objective-C 개발자를 위한 간단한 입문서입니다.
ms.prod: xamarin
ms.assetid: 00285CBD-AE5E-4126-8F22-6B231B9467EA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c4c8e7246b4414fb4153f0dd9eb812ddff1e7b07
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2018
---
# <a name="c-primer-for-objective-c-developers"></a>Objective-C 개발자용 C# 입문서

_Xamarin.iOS를 사용하면 C#에서 작성된 플랫폼 제약 없는 코드를 플랫폼 간에 공유할 수 있습니다. 그러나 기존 iOS 응용 프로그램은 이미 만들어진 Objective-C 코드를 활용하려 할 수 있습니다. 이 문서는 Xamarin 및 C# 언어로 이동하려는 Objective-C 개발자를 위한 간단한 입문서입니다._

Objective-C에서 개발된 iOS 및 OS X 응용 프로그램은 플랫폼별 코드가 필요 없는 곳에서 C#을 활용하여 Xamarin의 장점을 얻을 수 있으며, 이러한 코드를 Apple 이외의 장치에서 사용할 수 있습니다. 그리고 웹 서비스, JSON, XML 구문 분석 같은 것들과 사용자 지정 알고리즘을 플랫폼 간 방식으로 사용할 수 있습니다.

Objective-C 자산을 유지하면서도 Xamarin을 활용하려면 바인딩이라고 하는 Xamarin의 기술로 Xamarin을 C#에 노출하면 됩니다. 그러면 Objective-C 코드가 관리 C# 세계에 표시됩니다. 또한 원한다면 코드를 한 줄씩 C#으로 이식할 수 있습니다. 하지만 바인딩을 사용하든 아니면 이식을 사용하든, 기존 Objective-C 코드를 Xamarin.iOS에 효율적으로 활용하려면 Objective-C 및 C#에 대한 약간의 지식이 필요합니다.

## <a name="objective-c-interop"></a>Objective-C 상호 운용성

현재 Xamarin.iOS를 사용하여 Objective-C에서 호출 가능한 라이브러리를 C#으로 만드는 메커니즘은 지원되지 않습니다. 주요 이유는 바인딩 외에도 Mono 런타임이 필요하기 때문입니다. 하지만 사용자 인터페이스를 포함한 대부분의 논리는 여전히 Objective-C로 만들 수 있습니다. 이렇게 하려면 라이브러리에 Objective-C 코드를 래핑하고 그에 대한 바인딩을 만듭니다. 응용 프로그램을 부트스트랩하려면 Xamarin.iOS가 필요합니다(`Main` 진입점을 만들어야 한다는 의미). 그 후, 그 외의 다른 논리는 Objective-C에서 바인딩을 통해(또는 P/Invoke를 통해) C#에 노출할 수 있습니다. 이러한 방식으로 플랫폼 관련 논리는 Objective-C로 유지하고 플랫폼 중립적 부분은 C#으로 개발할 수 있습니다.

이 문서는 기존 Objective-C 코드로 바인딩하든 아니면 C#으로 이식하든, C# 및 Xamarin.iOS로 전환하는 개발자를 위한 입문서로써 두 언어의 비슷한 점과 차이점을 설명합니다.

바인딩을 만드는 방법에 대한 자세한 내용은 [Objective-C 바인딩](~/ios/platform/binding-objective-c/index.md)의 다른 문서를 참조하세요.

## <a name="language-comparison"></a>언어 비교

Objective-C와 C#은 구문 및 런타임의 관점에서 매우 다른 언어입니다. Objective-C는 동적 언어로 메시지 전달 체계를 사용하는 반면, C#은 형식은 정적 형식의 언어입니다. 구문 수준에서 볼 때 Objective-C는 Smalltalk와 비슷한 반면, C#은 최근 몇 년 사이에 Java에 없는 여러 기능이 새로 추가되기는 했지만 기본 구문의 상당수가 Java에서 파생됩니다.

따라서 Objective-C와 C#의 여러 언어 기능이 비슷한 방식으로 작동합니다. C#에서 Objective-C 코드에 대한 바인딩을 만들 때 또는 Objective-C를 C#으로 이식하는 경우 이러한 유사성을 알고 있으면 많은 도움이 됩니다.

### <a name="protocols-vs-interfaces"></a>프로토콜 vs. 인터페이스

Objective-C와 C# 둘 다 단일 상속 언어입니다. 그러나 두 언어는 여러 인터페이스를 특정 클래스로 구현하는 기능을 지원합니다. Objective-C에서는 이러한 논리 인터페이스를 *프로토콜*이라고 하고 C#에서는 *인터페이스*라고 합니다. 구현 수준에서 볼 때 C# 인터페이스와 Objective-C 프로토콜의 주요 차이점은 후자의 경우 선택적 메서드를 사용할 수 있다는 것입니다. 자세한 내용은 [이벤트, 대리자 및 프로토콜](~/ios/app-fundamentals/delegates-protocols-and-events.md) 문서를 참조하세요.

### <a name="categories-vs-extension-methods"></a>범주 vs. 확장 메서드

Objective-C는 *범주*를 사용하는 구현 코드가 없을 수도 있는 클래스에 메서드를 추가할 수 있습니다. C#에서 이와 비슷한 개념으로는 *확장 메서드*가 있습니다.

확장 메서드를 사용하면 클래스에 정적 메서드를 추가할 수 있으며, C#의 정적 메서드는 Objective-C의 class 메서드와 비슷합니다. 예를 들어 다음 코드는 `UITextView` 클래스에 `ScrollToBottom` 메서드를 추가하며, 이 클래스는 UIKit의 Objective-C `UITextView` 클래스에 바인딩되는 관리 클래스가 됩니다.

```csharp
public static class UITextViewExtensions
{
    public static void ScrollToBottom (this UITextView textView)
    {
        // code to scroll textView
    }
}
```

그 후 코드에 `UITextView` 인스턴스가 생성되면 아래와 같이 자동 완성 목록에서 메서드를 사용할 수 있습니다.

 ![](primer-images/01-extensionmethodintellisense.png "자동 완성 기능에서 사용할 수 있는 메서드")

확장 메서드가 호출되면 이 예제의 `textView`처럼 인스턴스가 인수로 전달됩니다.

### <a name="frameworks-vs-assemblies"></a>프레임워크 vs. 어셈블리

Objective-C는 관련 클래스를 프레임워크라고 하는 특수 디렉터리에 패키징합니다. 그러나 C# 및 .NET에서는 어셈블리를 사용하여 미리 컴파일된 코드의 재사용 가능한 비트를 제공합니다. iOS 외부 환경에서 어셈블리에는 런타임에 JIT(Just-In-Time) 컴파일되는 IL(중간 언어) 코드가 포함됩니다. 그러나 Apple은 iOS 응용 프로그램에서 JIT를 허용하지 않습니다. 따라서 Xamarin을 사용하는 iOS를 대상으로 하는 C# 코드는 AOT(Ahead Of Time) 컴파일되고, 응용 프로그램 번들에 포함된 메타데이터 파일과 함께 단일 Unix 실행 파일을 생성합니다.

### <a name="selectors-vs-named-parameters"></a>선택기 vs. 명명된 매개 변수

Objective-C 메서드는 기본적으로 매개 변수 이름을 특성에 따라 선택기에 포함합니다. 예를 들어 `AddCrayon:WithColor:` 같은 선택기는 각 매개 변수가 코드에 사용될 때 그 의미를 명확히 합니다. C#은 필요에 따라 명명된 인수도 지원합니다.

예를 들어 명명된 인수를 사용하는 C#의 비슷한 코드는 다음과 같습니다.

```csharp
AddCrayon (crayon: myCrayon, color: UIColor.Blue);
```

C#은 언어의 4.0 버전에서 이 지원을 추가했지만, 실제로는 그렇게 자주 사용되지 않습니다. 그러나 코드를 명확하게 하기 위해 이에 대한 지원이 제공됩니다.

### <a name="headers-and-namespaces"></a>헤더 및 네임스페이스

C의 상위 집합인 Objective-C는 구현 파일에서 구분되는 공용 선언에 대한 헤더를 사용합니다. C#은 헤더 파일을 사용하지 않습니다. Objective-C와는 달리, C# 코드는 네임스페이스에 포함됩니다. 일부 네임스페이스에서 사용할 수 있는 코드를 포함하려면 구현 파일의 맨 위에 using 지시문을 추가하거나 전체 네임스페이스가 있는 형식으로 한정해야 합니다.

예를 들어 다음 코드에는 `UIKit` 네임스페이스가 포함되어 있어서 해당 네임스페이스의 모든 클래스를 구현에 사용할 수 있습니다.

```csharp
using UIKit
namespace MyAppNamespace
{
    // implementation of classes
}
```

또한 위의 코드에 나온 네임스페이스 키워드는 구현 파일 자체에 사용되는 네임스페이스를 설정합니다. 여러 구현 파일이 동일한 네임스페이스를 공유하는 경우 네임스페이스를 using 지시문에 포함할 필요가 없습니다. 그렇게 하라는 암시적 의미가 있기 때문입니다.

### <a name="properties"></a>속성

Objective-C와 C# 모두 접근자 메서드에 대한 높은 수준의 추상화를 제공하는 속성 개념을 갖고 있습니다. Objective-C에서 @property 컴파일러 지시문은 효과적으로 접근자 메서드를 생성하는 데 사용됩니다. 반면, C#은 언어 자체에 속성 지원이 포함되어 있습니다. C# 속성은 다음 예제에 나와 있는 것처럼 지원 필드에 액세스하는 긴 스타일 또는 짧은 자동 속성 구문을 사용하여 구현할 수 있습니다.

```csharp
// automatic property syntax
public string Name { get; set; }

// property implemented with a backing field
string address;
public string Address {
    get {
        // could add additional code here
        return address;
    }
    set {
        address = value;
    }
}
```

### <a name="static-keyword"></a>Static 키워드

*static* 키워드의 의미는 Objective-C와 C#에서 매우 다릅니다. Objective-C에서 정적 함수는 함수의 범위를 현재 파일로 제한하는 데 사용됩니다. 하지만 C#에서는 *공용*, *개인* 및 *내부* 키워드를 통해 범위가 유지됩니다.

Objective-C의 변수에 static 키워드가 적용되면 변수는 함수 호출에서 해당 값을 유지합니다.

C#에도 static 키워드가 있습니다. static 키워드를 메서드에 적용하면 Objective-C에서 `+` 한정자가 하는 것과 똑같은 일을 효과적으로 수행합니다. 즉, 클래스 메서드를 만듭니다. 마찬가지로, static 키워드를 필드, 속성, 이벤트 등의 다른 구문에 적용하면 해당 형식의 인스턴스 대신 선언되는 형식의 해당 부분을 만듭니다. 또한 클래스에 정의된 모든 메서드가 정적이어야 하는 static 클래스를 만들 수 있습니다.

### <a name="nsarray-vs-list-initialization"></a>NSArray vs. 목록 초기화

이제 Objective-C에는 `NSArray`에 사용되는 리터럴 구문이 포함되어 있으므로 좀 더 쉽게 초기화할 수 있습니다. 하지만 C#에는 *제네릭*인 `List`라고 하는 풍부한 형식이 포함되어 있는데, 이는 목록을 만드는 코드를 사용하여 목록에서 보관할 형식을 제공할 수 있다는 의미입니다(예: C++의 템플릿). 또한 목록은 아래와 같이 자동 초기화 구문을 지원합니다.

```csharp
MyClass object1 = new MyClass ();
MyClass object2 = new MyClass ();
List<MyClass> myList = new List<MyClass>{ object1, object2 };
```

### <a name="blocks-vs-lambda-expressions"></a>블록 vs. 람다 식

Objective-C는 *블록*을 사용하여 클로저를 만들며, 여기서 사용자는 블록이 묶여 있는 상태를 사용할 수 있는 함수를 인라인으로 만들 수 있습니다. C#에도 람다 식을 사용하는 비슷한 개념이 있습니다. C#에서 람다 식은 아래와 같이 `=>` 연산자를 사용하여 만듭니다.

```csharp
(args) => {
    //  implementation code
};
```

람다 식에 대한 자세한 내용은 Microsoft의 [C# 프로그래밍 가이드](http://msdn.microsoft.com/library/vstudio/bb397687.aspx)를 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 Objective-C와 C#의 다양한 언어 기능을 비교하여 살펴보았습니다. 블록과 람다 식, 범주와 확장 메서드처럼 두 언어의 기능이 서로 비슷한 점도 있고, C#의 네임스페이스와 static 키워드의 의미처럼 두 언어가 서로 다른 점도 있습니다.
