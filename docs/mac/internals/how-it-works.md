---
title: Xamarin.Mac의 작동 원리
description: 이 문서에서는 Xamarin.Mac의 내부 작업을 설명 합니다. 특히, 생성자, 미리 컴파일, 메모리 관리 및 등록 기관에 살펴봅니다.
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 05/25/2017
ms.openlocfilehash: cd5371cde1dfcbe3cb1aea5dbdf8439816d66d95
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111319"
---
# <a name="how-xamarinmac-works"></a>Xamarin.Mac의 작동 원리

그러나 대부분의 개발자는 걱정할 필요가 없습니다를 내부 "매직" xamarin.mac,에 대 한 것으로 해석 모두 기존 문서의 내부적인 작업 작동 방법을 대 한 대략적인 이해는 C# 렌즈 및 디버깅 문제가 발생 하는 경우입니다.

Xamarin.Mac에서 응용 프로그램 연결 두 세계: 네이티브 클래스의 인스턴스를 포함 하는 기반 Objective-c 런타임에서 (`NSString`, `NSApplication`등) 한지를 C# 관리 되는 클래스의 인스턴스를 포함 하는 런타임 (`System.String` 를 `HttpClient`등). 이 두 환경을 사이 Xamarin.Mac 만들므로 양방향 브리지 앱 Objective C에서 (선택기) 메서드를 호출할 수 있습니다 (같은 `NSApplication.Init`) Objective-c로 앱을 호출할 수 있습니다 C# 메서드 (예: 메서드를 사용 하는 앱 대리자를) 다시 합니다. Objective C에 대 한 호출을 통해 투명 하 게 처리는 일반적으로 **P/Invoke** 와 Xamarin에서는 런타임 코드입니다.

<a name="exposing-classes" />

## <a name="exposing-c-classes--methods-to-objective-c"></a>표시 C# 클래스 / objective-c 메서드

그러나 앱의 콜백하지 Objective-c 용 C# 개체, Objective-c 이해할 수 있는 방식으로 노출 해야 합니다. 통해 이렇게 합니다 `Register` 및 `Export` 특성입니다. 다음 예제를 참조하세요.

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

이 예제에서는 Objective-c 런타임에서 이제 것 이라고 하는 클래스에 대 한 `MyClass` 호출 선택기를 사용 하 여 `init` 고 `run`입니다.

대부분의 경우, 즉 개발자 무시할 수 있는 구현 세부 정보는 앱이 수신 하는 대부분의 콜백 중 하나가 됩니다 재정의 된 메서드를 통해 온 `base` 클래스 (같은 `AppDelegate`하십시오 `Delegates`를 `DataSources`) 또는  **작업** Api에 전달 합니다. 이러한 경우 모든 `Export` 특성에 필요 하지 않은 C# 코드입니다.

## <a name="constructor-runthrough"></a>생성자 실행

대부분의 경우에서 개발자는 노출 해야 앱의 C# 클래스 생성 API는 Objective-c 런타임에서 경우와 같이 위치에서 인스턴스화될 수 있도록 호출 XIB 또는 스토리 보드 파일입니다. Xamarin.Mac 앱에서 사용 하는 가장 일반적인 5 명의 생성자는 다음과 같습니다.

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

일반적으로 개발자 유지 해야 합니다 `IntPtr` 및 `NSCoder` 일부 유형을 사용자 지정 등을 만들 때 생성 되는 생성자 `NSViews` 만 합니다. Xamarin.Mac Objective-c 런타임에서 요청에 대 한 응답으로 이러한 생성자 중 하나를 호출 해야 하는 경우 제거한 후 앱을 네이티브 코드 내에서 충돌 하 고 정확 하 게 문제를 파악 하기 어려울 수 있습니다.

## <a name="memory-management-and-cycles"></a>메모리 관리 및 주기

Xamarin.Mac의 메모리 관리는 다양 한 방식 Xamarin.iOS와 매우 비슷합니다. 이 문서에서 다루지 하나는 복잡 한 주제 이기도합니다. 읽어보세요 합니다 [메모리 및 성능 모범 사례](~/cross-platform/deploy-test/memory-perf-best-practices.md)합니다.

## <a name="ahead-of-time-compilation"></a>계속 해 서 of time 컴파일이

일반적으로.NET 응용 프로그램은 빌드 시 컴퓨터 코드 컴파일되지 않습니다, 가져오는 IL 코드를 호출 하는 중간 계층에 컴파일하는 대신 _Just In Time_ 앱이 시작 되 면 기계어 코드로 컴파일 (JIT).

Mono 런타임 JIT 컴파일에 소요 되는 시간 기계어 코드로이 느려질 수 있습니다 Xamarin.Mac 앱의 시작 최대 20%를 생성 하는 데 필요한 컴퓨터 코드에 대 한 시간이 소요 되므로.

Apple iOS에 따른 제한으로 인해 IL 코드의 JIT 컴파일 xamarin.ios 제공 되지 않습니다. 결과적으로, 모든 Xamarin.iOS 앱은 전체 _Ahead Of Time_ 빌드 주기 동안 기계어 코드로 컴파일된 (AOT).

새 Xamarin.Mac로 기능은 AOT IL 코드는 앱 빌드 주기 동안 Xamarin.iOS 마찬가지로입니다. Xamarin.Mac을 사용 하는 _하이브리드_ 되지만 사용 하면 필요한 trampolines 컴파일하고 계속 Reflection.Emit을 지원할 수 있는 유연성 런타임이 필요한 컴퓨터 코드의 대부분을 컴파일되지 않는 AOT 접근 방식 (및 다른 사용 사례에 현재 작업할 Xamarin.Mac).

두 가지 주요 영역을 여기서 AOT Xamarin.Mac 앱을 도움이:

- **"네이티브" 크래시 로그를 더 나은** -Cocoa Api를 잘못 된 호출을 수행할 때 자주 발생 되는 네이티브 코드에서 Xamarin.Mac 응용 프로그램 충돌 하는 경우 (보내는 등을 `null` 수락 하지 않습니다 하는 메서드를) 네이티브 JIT 사용 하 여 로그를 충돌 프레임은 분석 하기가 어렵습니다. JIT 프레임에 디버그 정보가 없는, 때문에 16 진수 오프셋을 사용 하 여 여러 줄 고 어떤 알 수 없습니다 수 됩니다. AOT "real" 명명 된 프레임을 생성 하 고 추적이 훨씬 쉽게 읽을 수 있습니다. 즉, Xamarin.Mac 앱은 더 잘 도구와 상호 작용 네이티브와 같은 **lldb** 하 고 **계측**합니다.
- **시간 성능을 더 잘 실행** -여러 두 번째 시작 시간, JIT 컴파일 코드의 모든 큰 Xamarin.Mac 응용 프로그램에서 시간이 많이 걸릴 수 있습니다. AOT 이러한 작업을 수행합니다.

### <a name="enabling-aot-compilation"></a>AOT 컴파일 사용

AOT를 두 번 클릭 하 여 Xamarin.Mac에서 활성화 되어는 **프로젝트 이름** 에 **솔루션 탐색기**로 이동 하면 **Mac 빌드** 추가 하 고 `--aot:[options]` 하는  **추가 mmp 인수:** 필드 (여기서 `[options]` 하나 이상의 옵션 AOT 유형을 제어 하려면 아래를 참조 하세요). 예를 들어:

![AOT 추가 mmp 인수 추가](how-it-works-images/aot01.png "추가 mmp 인수에 AOT 추가")

> [!IMPORTANT]
> 사용 하면 AOT 컴파일 크게 경우에 따라 몇 분까지 빌드할 때 늘어나지만 20%의 평균으로 앱 시작 시간을 향상 시킬 수 있습니다 것입니다. 결과적으로, AOT 컴파일 수에 활성화 해야 **릴리스** Xamarin.Mac 앱의 빌드입니다.

### <a name="aot-compilation-options"></a>Aot 컴파일 옵션

AOT 컴파일 Xamarin.Mac 앱에서 사용 하도록 설정할 때 조정할 수 있는 몇 가지 다른 옵션에는

- `none` -AOT 컴파일 없습니다. 이것이 기본 설정입니다.
- `all` -AOT를 MonoBundle에서 모든 어셈블리를 컴파일합니다.
- `core` -AOT 컴파일 합니다 `Xamarin.Mac`하십시오 `System` 및 `mscorlib` 어셈블리.
- `sdk` -AOT 컴파일는 `Xamarin.Mac` 및 클래스 라이브러리 (BCL (기본) 어셈블리입니다.
- `|hybrid` -하이브리드 AOT IL 제거를 허용 하는 하지만 결과에서 더 이상 컴파일되지 번이 위의 옵션 중 하나를 추가 합니다.
- `+` -AOT 컴파일으로에 대 한 단일을 포함 되어 있습니다.
- `-` -AOT 컴파일에서 단일 파일을 제거합니다.

예를 들어 `--aot:all,-MyAssembly.dll` AOT 컴파일 어셈블리를 MonoBundle에 모든 사용 하도록 설정 됩니다 _제외 하 고_ `MyAssembly.dll` 및 `--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll` 하이브리드를 사용 하는, 코드 AOT 포함는 `MyOtherAssembly.dll` 를제외하고`mscorlib.dll`.

## <a name="partial-static-registrar"></a>부분 정적 등록 기관

Xamarin.Mac 앱을 개발 하는 경우에 변경을 완료 하 고 테스트 하는 간격을 최소화 개발 마감일을 충족 하는 데 중요 한 될 수 있습니다. 전략 코드 베이스를 모듈화 하 고 앱은 비용이 많이 드는 전체 다시 작성 해야 하는 횟수를 줄여으로 단위 테스트 컴파일 시간이 감소 하는 데 도움이 됩니다.

또한 및 Xamarin.Mac, 새로운 _부분 정적 등록자_ (Xamarin.iOS를 통해 개발 되었으며) 하는 대로 수에서 Xamarin.Mac 앱의 시작 시간을 크게 줄일 합니다 **디버그** 구성 합니다. 들어가야 부분 정적 등록자를 사용 하는 방법을 이해는 디버그 시작을 5 배 개선 거의 등록자 란, 정적 및 동적 간의 차이점은,이 "부분 정적" 버전의 용도에 배경의 약간 걸립니다.

### <a name="about-the-registrar"></a>등록 기관에 대 한

모든 Xamarin.Mac 내부에서 응용 프로그램에는 Apple 및 Objective-c 런타임에서 Cocoa framework 있다는 점입니다. 이 "네이티브 world"의 "관리 되는 world" 사이의 연결을 구축 C# Xamarin.Mac은 기본 담당 합니다. 이 작업의 일부로 내에서 실행 되는 등록 기관에 의해 처리 됩니다 `NSApplication.Init ()` 메서드. Xamarin.mac에서 Cocoa Api를 사용 해야 하는 한 가지 이유 `NSApplication.Init` 를 먼저 호출 해야 합니다.

앱의 존재 여부 Objective-c 런타임에 알리는 데 기관의 작업은 C# 와 같은 클래스에서 파생 된 클래스 `NSApplicationDelegate`, `NSView`, `NSWindow`, 및 `NSObject`합니다. 이 등록 한 각 형식 보고서에 대해 요소를 확인 하려면 앱에서 모든 종류의 검색을 요구 합니다.

중이 검색을 수행할 수 있습니다 **동적**, 리플렉션 사용 하 여 응용 프로그램 시작 시 또는 **정적으로**, 빌드 시간 단계입니다. 등록 종류를 선택할 때 개발자는 다음 중 알고 있어야 합니다.

- 정적 등록 시작 시간을 감소 시킬 수 있지만 속도가 느려집니다 빌드 시간이 크게 (2 배 이상의 일반적으로 디버그 빌드 시간). 이 대 한 기본 됩니다 **릴리스** 구성을 빌드합니다.
- 동적 등록이 작업 때까지 지연이 추가 작업을 눈에 띄는 일시 중지 (최소: 2 초)을 만들 수 있지만 응용 프로그램 시작 및 코드 생성을 건너뛰고 응용 프로그램 시작에. 이 동적 등록 및 해당 리플렉션 느립니다. 기본값은 디버그 구성 빌드에서 특히 뚜렷하게 나타납니다.

Xamarin.iOS 8.13에서 처음 도입 된 일부 정적 등록 옵션을 모두의 개발자를 제공 합니다. 모든 요소에서 등록 정보를 미리 계산 하 여 `Xamarin.Mac.dll` (하는 빌드 시 연결할) 정적 라이브러리에서 Xamarin.Mac 사용 하 여이 정보를 전달, Microsoft 제거 대부분의 동적 리플렉션 시간 빌드 시간에 영향을 주지 하는 동안 등록 기관입니다.

### <a name="enabling-the-partial-static-registrar"></a>부분 정적 등록자를 사용 하도록 설정

부분 정적 등록 기관에 두 번 클릭 하 여 Xamarin.Mac에서 활성화 되어는 **프로젝트 이름** 에 **솔루션 탐색기**로 이동 하면 **Mac 빌드** 추가`--registrar:static` 에 **추가 mmp 인수:** 필드입니다. 예를 들어:

![추가 mmp 인수 부분 정적 등록 기관에 추가할](how-it-works-images/psr01.png "부분 정적 등록자 추가 mmp 인수 추가")

## <a name="additional-resources"></a>추가 자료

작업 내부적으로 작업 하는 방법에 대 한 보다 자세한 설명을 다음과 같습니다.

- [Objective-c 선택기](~/ios/internals/objective-c-selectors.md)
- [Registrar](~/ios/internals/registrar.md)
- [IOS 및 OS X 용 Xamarin 통합 API](~/cross-platform/macios/unified/index.md)
- [스레딩 기본 사항](~/ios/app-fundamentals/threading.md)
- [대리자, 프로토콜 및 이벤트](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [에 대 한 `newrefcount`](~/ios/internals/newrefcount.md)

