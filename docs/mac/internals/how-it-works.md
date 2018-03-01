---
title: "Xamarin.Mac의 작동 원리"
description: "이 문서에서는 Xamarin.Mac의 내부 작업에 설명 합니다. 특히, 생성자, 미리 컴파일, 메모리 관리 및 등록자에 살펴봅니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/25/2017
ms.openlocfilehash: 7329e8ddb5b86adcf6e1efaa805149012be8853c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="how-xamarinmac-works"></a>Xamarin.Mac의 작동 원리

그러나 대부분의 개발자는 내부 "매직" Xamarin.Mac의 걱정할 필요가 없습니다 것 대 한 대략적인 이해 어떻게 내부적인 작업 works는 데 도움이 됩니다 모두 C# 렌즈를 사용한 기존 문서를 해석 하 고 문제를 디버깅할 때 발생 합니다.

응용 프로그램의 장점을 모두 두 개의 연결 Xamarin.Mac에서: Objective-c 기반 런타임 네이티브 클래스의 인스턴스를 포함 하는 (`NSString`, `NSApplication`등) 관리 되는 클래스의 인스턴스를 포함 하는 C# 런타임 되며 (`System.String`, `HttpClient`등). 이 두 환경을 사이 Xamarin.Mac 브리지를 만듭니다. 양방향 Objective C의 응용 프로그램 (선택기) 메서드를 호출할 수 있습니다 (같은 `NSApplication.Init`) Objective-c 다시 (예: 응용 프로그램 대리자에서 메서드) 응용 프로그램의 C# 메서드를 호출할 수 있습니다. Objective C에는 호출을 통해 투명 하 게 처리 일반적으로 **P/Invoke** 와 Xamarin을 제공 하는 런타임 코드입니다.

<a name="exposing-classes" />

## <a name="exposing-c-classes--methods-to-objective-c"></a>C# 클래스를 노출 / Objective-c 하는 메서드

그러나 Objective-c 용 앱의 C# 개체에 콜백 하, 필요한 Objective-c 이해할 수 있는 방식으로 노출 될 수 있습니다. 통해 이렇게는 `Register` 및 `Export` 특성입니다. 다음 예제를 참조하세요.

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

이 예제에서는 Objective C 런타임 이제 것 이라고 하는 클래스에 대 한 `MyClass` 호출 선택기와 `init` 및 `run`합니다.

대부분의 경우에서이 개발자 무시할 수 있는 구현 정보 대부분 콜백을 수신 응용 프로그램에 포함 될 재정의 된 메서드를 통해 `base` 클래스 (예: `AppDelegate`, `Delegates`, `DataSources`)에서 또는  **작업** Api에 전달 합니다. 이 경우 모든 `Export` 특성 C# 코드에 필요 하지 않습니다.

## <a name="constructor-runthrough"></a>생성자 실행

대부분의 경우에서 개발자 노출 해야 합니다는 앱의 C# 클래스 생성 API Objective-c 런타임에 위치에서 같은 인스턴스화될 수 있도록 스토리 보드 또는 XIB 파일에서 호출 된 경우. 다음은 Xamarin.Mac 앱에서 사용 하는 가장 일반적인 5 명의 생성자입니다.

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

개발자 유지 해야 합니다. 일반적으로 `IntPtr` 및 `NSCoder` 일부 형식 예: 사용자 지정을 만들 때 생성 되는 생성자 `NSViews` 만 합니다. Xamarin.Mac Objective C 런타임 요청을 받아에서 이러한 생성자 중 하나를 호출 해야 하는 경우 제거한 후 응용 프로그램은 네이티브 코드 내부 작동이 중단 되 고 정확 하 게 문제를 파악 하기 어려울 수도 있습니다.

## <a name="memory-management-and-cycles"></a>메모리 관리 및 주기

Xamarin.Mac에서 메모리 관리 xamarin.ios 매우 비슷한 여러 가지 방법으로는입니다. 또한이 문서의 범위를 벗어나는 하나는 복잡 한 주제는. 읽으십시오는 [메모리 및 성능 모범 사례](~/cross-platform/deploy-test/memory-perf-best-practices.md)합니다.

## <a name="ahead-of-time-compilation"></a>컴파일 앞의

일반적으로.NET 응용 프로그램을 컴파일하지 않거나 기계어 코드 아래쪽으로 작성 될 때, 가져오는 IL 코드 라고 하는 중간 계층에 컴파일됩니다 대신 _Just In Time_ 앱을 실행 하는 경우 (JIT) 기계어 코드로 컴파일된 합니다.

모노 런타임 JIT 컴파일 하는 데 걸리는 시간 기계어 코드에이 느려질 수 있습니다 Xamarin.Mac 앱의 시작을 최대 20%, 대로를 생성 하는 데 필요한 시스템 코드에 대 한 시간이 걸립니다.

Apple iOS에서 설정한 제한으로 인해 IL 코드의 JIT 컴파일 xamarin.ios ´ ù. 결과적으로, 모든 Xamarin.iOS 앱은 전체 _타임 Ahead_ 빌드 주기 동안 기계어 코드로 컴파일된 (AOT).

새 Xamarin.Mac를 수 있다는 AOT IL 코드가 응용 프로그램 빌드 주기 동안 Xamarin.iOS 같이 합니다. 사용 하 여 Xamarin.Mac는 _하이브리드_ 필요한 기계어 코드의 대부분 컴파일되지 않는 AOT 접근 방식을 사용 하 고 계속 Reflection.Emit를 지원 하기 위한 유연성이 필요한 trampolines를 런타임에 있습니다 (및 기타 사용 경우까지 이렇게 하는 현재 작동 하므로 Xamarin.Mac에).

두 가지 주요 영역을 여기서 AOT Xamarin.Mac 앱 수 있습니다:

- **향상 된 "네이티브" 크래시 로그** -Xamarin.Mac 응용 프로그램은 흔히 Cocoa Api를 잘못 된 호출을 수행 하는 경우 네이티브 코드에서 충돌 하는 경우 (보내는 것과 같은 `null` 수용 하지 않는 메서드에) 네이티브 충돌 JIT와 함께 로그 프레임은 분석 하기 어렵습니다. 않으므로 JIT 프레임이 없는 디버그 정보에는 16 진수 오프셋으로 여러 줄 및 알 수 있는 진행 된 상황이 단서가 없게 됩니다. AOT "real" 명명 된 프레임을 생성 되며 추적 훨씬 더 쉽게 읽을 수 있습니다. 즉, Xamarin.Mac 앱이와 상호 작용 더 잘 네이티브 도구와 같은 **lldb** 및 **기기**합니다.
- **시간 성능을 더 잘 실행** -여러 개의 두 번째 시작 시간, JIT 컴파일 코드를 모두 큰 Xamarin.Mac 응용 프로그램에는 상당한 시간이 걸릴 수 있습니다. AOT 이러한 작업을 수행합니다.

### <a name="enabling-aot-compilation"></a>AOT 컴파일을 사용 하도록 설정

AOT 두 번 클릭 하 여 Xamarin.Mac에서 설정 됩니다는 **프로젝트 이름** 에 **솔루션 탐색기**탐색 하려면, **Mac 빌드** 추가 `--aot:[options]` 는 를 **추가 mmp 인수:** 필드 (여기서 `[options]` AOT 유형을 제어, 아래를 참조 하는 하나 이상의 옵션은). 예:

![추가 mmp 인수에 AOT 추가](how-it-works-images/aot01.png "추가 mmp 인수에 AOT 추가")

> [!IMPORTANT]
> 경고! 사용 하면 AOT 컴파일 크게 빌드 시간 경우에 따라 몇 분까지 걸릴 늘어나지만 앱 시작 시간 20%의 평균으로 높일 수 있습니다. 결과적으로, AOT 컴파일 수에 활성화 해야 **릴리스** Xamarin.Mac 앱의 작성 합니다.

### <a name="aot-compilation-options"></a>Aot 컴파일 옵션

AOT 컴파일 Xamarin.Mac는 앱에서 사용 하도록 설정할 때 조정 될 수 있는 여러 옵션이 있습니다.

- `none` -AOT 컴파일입니다. 이것이 기본 설정입니다.
- `all` -AOT는 MonoBundle에서 모든 어셈블리를 컴파일합니다.
- `core` -AOT 컴파일하는 `Xamarin.Mac`, `System` 및 `mscorlib` 어셈블리입니다.
- `sdk` -AOT 컴파일하는 `Xamarin.Mac` 및 클래스 라이브러리 BCL (기본) 어셈블리입니다.
- `|hybrid` -하이브리드 AOT IL 제거 수 있지만 결과를 더 이상 컴파일되지 번 위의 옵션 중 하나에이 추가 합니다.
- `+` -AOT 컴파일에 대 한 단일 포함 되어 있습니다.
- `-` -AOT 컴파일에서 단일 파일을 제거합니다.

예를 들어 `--aot:all,-MyAssembly.dll` 를 사용 하면는 MonoBundle에 있는 어셈블리의 모든 AOT 컴파일 _제외 하 고_ `MyAssembly.dll` 및 `--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll` 하이브리드를 사용 하면, AOT 코드에 포함 되어는 `MyOtherAssembly.dll` 는 를제외하고`mscorlib.dll`.

## <a name="partial-static-registrar"></a>부분 정적 등록 기관

Xamarin.Mac 앱을 개발 하는 경우에 변경을 완료 하 고 테스트 사이의 시간을 최소화 개발 마감일을 충족 하는 것이 중요 될 수 있습니다. 전략의 모듈화 코드 베이스 및 단위 테스트 도움이 되는지 컴파일 시간을 줄일 수 있는 앱은 비용이 많이 드는 전체 다시 빌드는 횟수를 줄일 때와 같은 합니다.

또한 및 Xamarin.Mac, 새 _부분 정적 등록 기관_ (Xamarin.iOS에 의해 개발 되었으며)으로 상당히 줄일 수 있습니다에 Xamarin.Mac 응용 프로그램의 시작 시간에서 **디버그** 구성 합니다. 압축 되어 어떻게 수 부분 정적 등록자를 사용 하 여 이해는 디버그 시작의 5 배 향상 거의 무엇 등록자, 정적 및 동적, 간의 차이점은 무엇 이며이 "부분 정적" 버전에서 수행 하는 작업에 대 한 배경 약간 걸립니다.

### <a name="about-the-registrar"></a>등록 기관에 대 한

모든 Xamarin.Mac 내부에서 응용 프로그램에서 Apple 및 Objective-c 런타임 Cocoa 프레임 워크를 표시 됩니다. 이 "네이티브 world"와 "관리 되는 world"의 C# 간의 다리 구축은 Xamarin.Mac 주 책임은입니다. 이 작업의 일부는 내부에서 실행 되는 등록 기관에서 처리 `NSApplication.Init ()` 메서드. 이 Xamarin.Mac Cocoa Api를 사용 해야 하는 한 가지 이유 `NSApplication.Init` 를 먼저 호출 해야 합니다.

등록자 일 Objective C와 같은 클래스에서 파생 되는 앱의 C# 클래스의 존재를 런타임에 알려야 하는 것 `NSApplicationDelegate`, `NSView`, `NSWindow`, 및 `NSObject`합니다. 이 등록 하는 것이 사항 및 보고서에는 각 유형에 대해 요소를 확인 하려면 응용 프로그램에 있는 모든 형식의 검색만 필요 합니다.

이 검사를 수행할 수 중 하나 **동적으로**, 리플렉션 사용 하 여 응용 프로그램의 시작 시 또는 **정적으로**, 빌드 시간 단계 합니다. 등록 유형을 선택할 때 개발자는 다음의 알고 있어야 합니다.

- 정적 등록 시작 시간을 획기적으로 줄일 수 있지만 속도가 느려집니다 빌드 시간 크게 (double 이상의 일반적으로 디버그 빌드 시간). 이 대 한 기본 됩니다 **릴리스** 구성을 빌드합니다.
- 동적 등록 본이 저작물 때까지 지연이 추가 작업 현저한 일시 중지가 (적어도 2 초)을 만들 수 있지만 응용 프로그램 실행 및 코드 생성을 건너뜁니다에서 응용 프로그램을 시작 합니다. 더욱 그렇습니다 디버그 구성 빌드에서 데이터베이스가 기본 인 동적 등록 하 고 해당 리플렉션 느립니다.

Xamarin.iOS 8.13에 처음 도입 된 부분 정적 등록, 두 옵션의 개발자를 제공 합니다. 사전에 있는 모든 요소의 등록 정보를 계산 하 여 `Xamarin.Mac.dll` Xamarin.Mac (하는 빌드 시에 연결할 수)의 정적 라이브러리를 사용 하 여이 정보를 전달, Microsoft 제거 대부분의 동적 리플렉션 시간 빌드 시간에 영향을 주지 하는 동안 등록 합니다.

### <a name="enabling-the-partial-static-registrar"></a>부분 정적 등록자를 사용 하도록 설정

부분 정적 등록 기관 두 번 클릭 하 여 Xamarin.Mac에서 설정 됩니다는 **프로젝트 이름** 에 **솔루션 탐색기**탐색 하려면, **Mac 빌드** 추가`--registrar:static` 에 **추가 mmp 인수:** 필드입니다. 예:

![추가 mmp 인수를 부분 정적 등록자 추가](how-it-works-images/psr01.png "추가 mmp 인수에는 부분 정적 등록 기관 추가")

## <a name="additional-resources"></a>추가 리소스

보다 자세한 설명을 작동 원리 내부적으로 다음과 같습니다.

- [Objective C 선택기](~/ios/internals/objective-c-selectors.md)
- [Registrar](~/ios/internals/registrar.md)
- [IOS 및 OS X 용 Xamarin 통합 API](~/cross-platform/macios/unified/index.md)
- [스레딩 기본 사항](~/ios/app-fundamentals/threading.md)
- [대리자, 프로토콜 및 이벤트](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [에 대 한 `newrefcount`](~/ios/internals/newrefcount.md)

