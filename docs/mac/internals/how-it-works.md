---
title: Xamarin.Mac 작동 방법
description: 이 문서에서는 Xamarin.ios의 내부 작동에 대해 설명 합니다. 특히 생성자, 메모리 관리, 미리 컴파일 및 등록자를 확인 합니다.
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 05/25/2017
ms.openlocfilehash: 24ddd71fe1468edc70ec4d487dc2cb2dbd4da1b6
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769818"
---
# <a name="how-xamarinmac-works"></a>Xamarin.Mac 작동 방법

대부분의 경우 개발자는 Xamarin.ios의 내부 "매직"에 대해 걱정 하지 않아도 되지만, 내부적으로 작동 하는 방식을 대략적으로 이해 하는 것은 기존 설명서를 사용 하 여 C# 렌즈 및 디버깅 하는 데 도움이 됩니다. 문제가 발생할 때 발생 합니다.

Xamarin.ios에서 응용 프로그램은 두 가지 환경을 연결 합니다. 네이티브 클래스의 인스턴스를 포함 하는 목표 C 기반`NSString`런타임 ( `NSApplication`, 등)이 있으며 관리 되는 클래스 C# (`System.String`, `HttpClient`등)의 인스턴스를 포함 하는 런타임이 있습니다. 이러한 두 가지 환경 사이에서 xamarin.ios는 두 가지 방식으로 연결 되므로 앱에서 목표-c (예: `NSApplication.Init`)의 메서드 (선택기)를 호출 하 고, 목표-c는 앱의 C# 메서드 (예: 앱 대리자의 메서드)를 호출할 수 있습니다. 일반적으로 목표 C에 대 한 호출은 **P/invoke** 를 통해 투명 하 게 처리 되 고 일부 런타임 코드는 Xamarin에서 제공 합니다.

<a name="exposing-classes" />

## <a name="exposing-c-classes--methods-to-objective-c"></a>클래스 C# /메서드를 목표에 노출-C

그러나 목표 C가 앱의 C# 개체로 다시 호출 되 게 하려면 목표에서 이해할 수 있는 방식으로 노출 해야 합니다. 이 작업은 `Register` 및 `Export` 특성을 통해 수행 됩니다. 다음 예제를 참조하세요.

```csharp
[Register ("MyClass")]
public class MyClass : NSObject
{
   [Export ("init")]
   public MyClass ()
   {
   }

   [Export ("run")]
   public void Run ()
   {
   }
}
```

이 예제에서 목표-C 런타임은 이제 및 `MyClass` `run`라는 `init` 선택기를 사용 하 여 호출 된 클래스에 대해 알 수 있습니다.

대부분의 경우, 앱이 수신 하는 `base` 대부분의 콜백은 클래스 (예 `AppDelegate`:,, `DataSources`) 또는 **동작** 에서 재정의 된 메서드를 `Delegates`통해 수신 되므로 개발자는 무시할 수 있는 구현 세부 정보입니다. Api에 전달 됩니다. 이러한 모든 경우 `Export` 에는 C# 코드에 특성이 필요 하지 않습니다.

## <a name="constructor-runthrough"></a>실행 생성자

대부분의 경우 개발자는 응용 프로그램의 C# 클래스 생성 API를 목표-C 런타임에 노출 하 여 STORYBOARD 또는 XIB 파일에서 호출 되는 경우와 같은 위치에서 인스턴스화할 수 있도록 해야 합니다. 다음은 Xamarin.ios 앱에서 사용 되는 가장 일반적인 5 개의 생성자입니다.

```csharp
// Called when created from unmanaged code
public CustomView (IntPtr handle) : base (handle)
{
   Initialize ();
}

// Called when created directly from a XIB file
[Export ("initWithCoder:")]
public CustomView (NSCoder coder) : base (coder)
{
   Initialize ();
}

// Called from C# to instance NSView with a Frame (initWithFrame)
public CustomView (CGRect frame) : base (frame)
{
}

// Called from C# to instance NSView without setting the frame (init)
public CustomView () : base ()
{
}

// This is a special case constructor that you call on a derived class when the derived called has an [Export] constructor.
// For example, if you call init on NSString then you don’t want to call init on NSObject.
public CustomView () : base (NSObjectFlag.Empty)
{
}
```

일반적으로 개발자는 사용자 지정 `IntPtr` `NSViews` 만 같은 일부 `NSCoder` 형식을 만들 때 생성 되는 및 생성자를 그대로 두어야 합니다. Xamarin.ios가 목표-C 런타임 요청에 대 한 응답으로 이러한 생성자 중 하나를 호출 해야 하 고 제거 하면 앱이 네이티브 코드 내에서 충돌 하 고 문제를 정확 하 게 파악 하기가 어려울 수 있습니다.

## <a name="memory-management-and-cycles"></a>메모리 관리 및 주기

Xamarin.ios의 메모리 관리는 Xamarin.ios와 매우 비슷한 여러 가지 방식으로 진행 됩니다. 이 문서의 범위를 벗어나는 복잡 한 토픽 이기도 합니다. [메모리 및 성능 모범 사례](~/cross-platform/deploy-test/memory-perf-best-practices.md)를 참조 하세요.

## <a name="ahead-of-time-compilation"></a>컴파일 시간 미리

일반적으로 .NET 응용 프로그램은 빌드될 때 기계어 코드로 컴파일되지 않으며, 대신 앱이 시작 될 때 기계어 코드로 컴파일된 JIT ( _just-in-time_ )를 가져오는 IL 코드 라는 중간 계층으로 컴파일됩니다.

이 기계어 코드를 JIT 컴파일하는 데 사용 하는 시간에는 필요한 기계어 코드를 생성 하는 데 시간이 걸리기 때문에 최대 20%까지 Xamarin.ios 앱의 시작 속도를 저하 시킬 수 있습니다.

IOS에서 Apple에 의해 적용 되는 제한 사항으로 인해 Xamarin.ios에서는 IL 코드의 JIT 컴파일을 사용할 수 없습니다. 결과적으로 모든 Xamarin.ios 앱은 빌드 주기 중에 기계어 코드로 컴파일된 AOT (전체 _시간_ )입니다.

Xamarin.ios를 처음 접하는 경우 Xamarin.ios와 마찬가지로 앱 빌드 주기 중에 IL 코드를 AOT 수 있습니다. Xamarin.ios는 필요한 대부분의 기계어 코드를 컴파일하는 _하이브리드_ AOT 접근 방식을 사용 하지만, 런타임이 필요한 trampolines를 컴파일할 수 있도록 하 고, 현재 작업 중인 다른 사용 사례를 계속 지원 합니다. Xamarin.ios).

AOT에서 Xamarin.ios 앱을 지원할 수 있는 두 가지 주요 영역은 다음과 같습니다.

- **"기본" 크래시 로그 개선** -xamarin.ios 응용 프로그램이 네이티브 코드에서 충돌 하는 경우 (예:를 `null` 수락 하지 않는 메서드로 전송 하는 등) cocoa api에 대 한 잘못 된 호출을 수행할 때 일반적으로 발생 합니다. 분석 하기 어렵습니다. JIT 프레임은 디버그 정보를 포함 하지 않으므로 16 진수 오프셋을 포함 하는 여러 줄이 있고 무슨 일이 발생 했는지 알 수 없습니다. AOT은 "실제" 이름의 프레임을 생성 하 고 추적을 훨씬 더 쉽게 읽을 수 있습니다. 즉, Xamarin.ios 앱은 **lldb** 및 **기기와**같은 네이티브 도구를 사용 하 여 더 효율적으로 상호 작용 합니다.
- **더 나은 시작 시간 성능** -큰 xamarin.ios 응용 프로그램의 경우 두 번째 시작 시간을 사용 하면 모든 코드를 JIT 컴파일하는 데 상당한 시간이 걸릴 수 있습니다. AOT는이 작업을 앞으로 진행 합니다.

### <a name="enabling-aot-compilation"></a>AOT 컴파일 사용

AOT는 **솔루션 탐색기**에서 **프로젝트 이름을** 두 번 클릭 하 고, **mac 빌드** 로 이동 하 고, `--aot:[options]` **추가 mmp 인수:** 필드 (여기서 `[options]` 는 하나 이상)에 추가 하 여 xamarin.ios에서 사용 하도록 설정 됩니다. AOT 유형을 제어 하는 옵션은 아래 참조). 예:

![추가 mmp 인수에 AOT 추가](how-it-works-images/aot01.png "추가 mmp 인수에 AOT 추가")

> [!IMPORTANT]
> AOT 컴파일을 사용 하도록 설정 하면 빌드 시간이 크게 증가 하 고, 경우에 따라 최대 몇 분이 걸릴 수 있지만 응용 프로그램 시작 시간을 평균 20%까지 향상 시킬 수 있습니다. 따라서 AOT 컴파일은 Xamarin.ios 앱의 **릴리스** 빌드에서만 사용 하도록 설정 해야 합니다.

### <a name="aot-compilation-options"></a>Aot 컴파일 옵션

Xamarin.ios 앱에서 AOT 컴파일을 사용 하도록 설정할 때 조정할 수 있는 몇 가지 옵션이 있습니다.

- `none`-AOT 컴파일이 없습니다. 이것이 기본 설정입니다.
- `all`-AOT는 MonoBundle의 모든 어셈블리를 컴파일합니다.
- `core`-AOT는 `Xamarin.Mac`, `System` 및 `mscorlib` 어셈블리를 컴파일합니다.
- `sdk`-AOT는 및 `Xamarin.Mac` BCL (기본 클래스 라이브러리) 어셈블리를 컴파일합니다.
- `|hybrid`-위의 옵션 중 하나에이를 추가 하면 IL 제거를 허용 하지만 컴파일 시간이 길어질 수 있는 하이브리드 AOT 활성화 됩니다.
- `+`-AOT 컴파일을 위한 단일 파일을 포함 합니다.
- `-`-AOT 컴파일에서 단일 파일을 제거 합니다.

예를 들어 `--aot:all,-MyAssembly.dll` , _는_ `MyAssembly.dll` MonoBundle의 모든 어셈블리에 대해 AOT 컴파일을 사용 하도록 설정 하 `--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll` 고, 하이브리드을 `mscorlib.dll`사용 하도록 설정 하 `MyOtherAssembly.dll` 고, 코드 AOT는를 포함 하 고를 제외 합니다.

## <a name="partial-static-registrar"></a>부분 정적 등록자

Xamarin.ios 앱을 개발할 때 변경 완료와 테스트 간의 시간을 최소화 하는 것이 개발 최종 기한에 중요 한 것이 될 수 있습니다. 코드 베이스 및 단위 테스트의 모듈화와 같은 전략을 통해 응용 프로그램에서 비용이 많이 드는 전체 다시 빌드를 필요로 하는 횟수를 줄임으로써 컴파일 시간을 줄일 수 있습니다.

또한 xamarin.ios를 처음 접하는 경우 최초로에서 비롯 한 _부분 정적 등록자_ 는 **디버그** 구성에서 xamarin.ios 앱의 시작 시간을 크게 줄일 수 있습니다. 부분 정적 등록자를 사용 하는 방법을 이해 하는 것은 디버그 시작 시 거의 5 배 향상 된 기능을 squeezed 수 있습니다. 즉, 등록자의 정의, 정적 및 동적 간의 차이 및이 "부분 정적" 버전에서 수행 하는 작업에 대해 약간의 배경 지식이 소요 됩니다.

### <a name="about-the-registrar"></a>등록자 정보

Xamarin.ios 응용 프로그램은 Apple의 공동 Coa 프레임 워크 및 목표-C 런타임의 공동 coa 프레임 워크입니다. 이 "기본 세계"와의 C# "관리 세계" 사이에 브리지를 구축 하는 것은 xamarin.ios의 주요 책임입니다. 이 작업의 일부는 메서드 내 `NSApplication.Init ()` 에서 실행 되는 등록자에 의해 처리 됩니다. 이는 xamarin.ios `NSApplication.Init` 에서 cocoa api를 먼저 호출 해야 하는 한 가지 이유입니다.

등록자 C# 의 작업은 `NSApplicationDelegate`, `NSView` `NSWindow`, 및`NSObject`과 같은 클래스에서 파생 되는 앱 클래스의 존재를 목표 C 런타임에 알려 주는 것입니다. 이렇게 하려면 앱에서 모든 형식을 검색 하 여 등록 해야 하는 항목 및 보고할 각 형식의 요소를 확인 해야 합니다.

이 검색은 리플렉션을 사용 하 여 응용 프로그램을 시작할 때 **동적**으로 수행 하거나 빌드 시간 단계로 **정적**으로 수행할 수 있습니다. 등록 유형을 선택 하는 경우 개발자는 다음 사항을 알고 있어야 합니다.

- 정적 등록은 시작 시간을 크게 줄일 수 있지만 빌드 시간을 상당히 느리게 할 수 있습니다 (일반적으로 이중 디버그 빌드 시간 이상). 이는 **릴리스** 구성 빌드에 대 한 기본값입니다.
- 동적 등록은 응용 프로그램이 시작 되 고 코드 생성을 건너뛸 때까지이 작업을 지연 하지만, 이러한 추가 작업은 응용 프로그램 시작 시 눈에 띄는 일시 중지 (2 초 이상)를 만들 수 있습니다. 이는 기본적으로 동적 등록으로 설정 되 고 리플렉션의 속도가 느린 디버그 구성 빌드에서 특히 그렇습니다.

Xamarin.ios 8.13에 처음 도입 된 부분 정적 등록은 개발자에 게 두 옵션 중에서 가장 적합 한 기능을 제공 합니다. 의 `Xamarin.Mac.dll` 모든 요소에 대 한 등록 정보를 미리 계산 하 고이 정보를 정적 라이브러리의 xamarin.ios로 전달 하면 (빌드 시에만 연결 해야 함) Microsoft에서 대부분의 리플렉션 시간을 동적으로 제거 했습니다. 등록자는 빌드 시간에 영향을 주지 않습니다.

### <a name="enabling-the-partial-static-registrar"></a>부분 정적 등록자 사용

부분 정적 등록자는 **솔루션 탐색기**에서 **프로젝트 이름을** 두 번 클릭 하 고 **mac 빌드** 로 이동한 다음 `--registrar:static` **추가 mmp arguments:** 필드에 추가 하 여 xamarin.ios에서 사용 하도록 설정 됩니다. 예:

![추가 mmp 인수에 부분 정적 등록자 추가](how-it-works-images/psr01.png "추가 mmp 인수에 부분 정적 등록자 추가")

## <a name="additional-resources"></a>추가 자료

다음은 내부적으로 작업을 수행 하는 방법에 대 한 자세한 설명입니다.

- [목표-C 선택기](~/ios/internals/objective-c-selectors.md)
- [Registrar](~/ios/internals/registrar.md)
- [IOS 및 OS X 용 Xamarin Unified API](~/cross-platform/macios/unified/index.md)
- [Theading 기본 사항](~/ios/app-fundamentals/threading.md)
- [대리자, 프로토콜 및 이벤트](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [에 대 한`newrefcount`](~/ios/internals/newrefcount.md)
