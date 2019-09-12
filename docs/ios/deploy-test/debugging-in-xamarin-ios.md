---
title: Xamarin.iOS 앱 디버깅
description: 이 문서에서는 Mac 또는 Visual Studio 2019용 Visual Studio에서 디버거를 사용하여 중단점 설정, 무선 디버깅 등을 포함한 Xamarin.iOS 애플리케이션을 디버깅하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 05460010-99E1-DC38-F855-2D691EF54484
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: 8a1a110bf1ff021c3280e19dea777180d71dba1a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70763354"
---
# <a name="debugging-xamarinios-apps"></a>Xamarin.iOS 앱 디버깅

_Mac용 Visual Studio 또는 Visual Studio에서 기본 제공 디버거를 사용하여 Xamarin.iOS 애플리케이션을 디버그할 수 있습니다._

C# 및 기타 관리되는 언어 코드를 디버그하는 경우 Mac용 Visual Studio의 네이티브 디버깅 지원을 사용하고, Xamarin.iOS 프로젝트와 연결할 수 있는 C, C++ 또는 Objective C 코드를 디버그해야 하는 경우 [LLDB](http://lldb.llvm.org/tutorial.html)를 사용합니다.

> [!NOTE]
> 디버그 모드에서 애플리케이션을 컴파일할 때 Xamarin.iOS는 모든 코드 줄을 계측해야 하므로 더 느리고 더 큰 애플리케이션을 생성합니다. 먼저 릴리스 빌드를 수행한 후에 릴리스해야 합니다.

Xamarin.iOS 디버거는 IDE에 통합되어 있으므로 개발자는 시뮬레이터와 디바이스에서 Xamarin.iOS를 통해 지원되는 모든 관리되는 언어로 빌드한 Xamarin.iOS 애플리케이션을 디버그할 수 있습니다.

Xamarin.iOS 디버거는 [Mono 소프트 디버거](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/)를 사용합니다. 즉, 생성된 코드와 Mono 런타임이 IDE와 협력하여 디버깅 환경을 제공합니다. 이는 디버그된 프로그램의 지식이나 협력 없이 프로그램을 제어하는 LLDB 또는 MDB와 같은 하드 디버거와 다릅니다.

## <a name="setting-breakpoints"></a>중단점 설정

애플리케이션 디버깅을 시작할 준비가 되면 첫 번째 단계는 [애플리케이션에 중단점을 설정](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)하는 것입니다. 이 작업은 중단하려는 코드 번호 옆에 있는 편집기의 여백 영역에서 클릭하여 수행됩니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](debugging-in-xamarin-ios-images/debugging1.png "중단점 설정")](debugging-in-xamarin-ios-images/debugging1.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](debugging-in-xamarin-ios-images/debugging1a.png "중단점 설정")](debugging-in-xamarin-ios-images/debugging1a.png#lightbox)

-----

**중단점 패드**로 이동하여 코드에 설정된 모든 중단점을 볼 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](debugging-in-xamarin-ios-images/image0a.png "중단점 패드")](debugging-in-xamarin-ios-images/image0a.png#lightbox)

 중단점 패드가 자동으로 표시되지 않으면 _보기 > 디버그 창 > 중단점_을 선택하여 중단점 패드를 표시할 수 있습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](debugging-in-xamarin-ios-images/image0.png "중단점 패드")](debugging-in-xamarin-ios-images/image0.png#lightbox)

 중단점 패드가 자동으로 표시되지 않으면 _디버그 > 창 > 중단점_을 선택하여 중단점 패드를 표시할 수 있습니다.

-----

애플리케이션 디버깅을 시작하기 전에 중단점, 데이터 시각화 도우미 사용, 호출 스택 보기와 같은 디버깅을 지원하는 유용한 도구 모음이 포함되어 있으므로 구성이 **디버그**로 설정되어 있는지 항상 확인해야 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](debugging-in-xamarin-ios-images/debugging7.png "시뮬레이터에서 디버깅")](debugging-in-xamarin-ios-images/debugging7.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7a.png "물리적 디바이스에서 디버깅")](debugging-in-xamarin-ios-images/debugging7a.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](debugging-in-xamarin-ios-images/debugging7c.png "시뮬레이터에서 디버깅")](debugging-in-xamarin-ios-images/debugging7c.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7d.png "물리적 디바이스에서 디버깅")](debugging-in-xamarin-ios-images/debugging7d.png#lightbox)

-----

## <a name="start-debugging"></a>디버깅 시작
디버깅을 시작하려면 IDE에서 대상 디바이스 또는 유사한 디바이스를 선택합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](debugging-in-xamarin-ios-images/debugging7b.png "대상 디바이스 선택")](debugging-in-xamarin-ios-images/debugging7b.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](debugging-in-xamarin-ios-images/debugging7e.png "대상 디바이스 선택")](debugging-in-xamarin-ios-images/debugging7e.png#lightbox)

-----

그런 다음, **재생** 단추를 눌러 애플리케이션을 배포합니다.

중단점을 적중하면 코드가 노란색으로 강조 표시됩니다.

[![](debugging-in-xamarin-ios-images/image2.png "노란색으로 강조 표시된 코드")](debugging-in-xamarin-ios-images/image2.png#lightbox)

개체의 값을 검사하는 것과 같은 디버깅 도구를 현재 시점에서 사용하여 코드에서 발생하는 상황에 대한 자세한 정보를 얻을 수 있습니다.

[![](debugging-in-xamarin-ios-images/image3.png "색상 값 표시")](debugging-in-xamarin-ios-images/image3.png#lightbox)

## <a name="conditional-breakpoints"></a>조건부 중단점

중단점이 발생해야 하는 상황을 결정하는 규칙을 설정할 수도 있습니다. 이를 *조건부 중단점* 추가라고 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

조건부 중단점을 설정하려면 **중단점 속성 창**에 액세스합니다. 다음 두 가지 방법으로 액세스할 수 있습니다.

- 새 조건부 중단점을 추가하려면 편집기 여백에서 중단점을 설정하려는 코드의 줄 번호 왼쪽을 마우스 오른쪽 단추로 클릭하고 새 중단점을 선택합니다.

  [![](debugging-in-xamarin-ios-images/image4.png "새 중단점 선택")](debugging-in-xamarin-ios-images/image4.png#lightbox)

- 기존 중단점에 조건을 추가하려면, 중단점을 마우스 오른쪽 단추로 클릭하고 **중단점 속성**을 선택하거나 **중단점 패드**에서 아래 그림과 같은 속성 단추를 선택합니다.

  [![](debugging-in-xamarin-ios-images/image5.png "중단점 패드")](debugging-in-xamarin-ios-images/image5.png#lightbox)

그런 다음, 중단점이 발생되게 하는 조건을 입력할 수 있습니다.

[![](debugging-in-xamarin-ios-images/image6.png "발생할 중단점의 조건 입력")](debugging-in-xamarin-ios-images/image6.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 조건부 중단점을 설정하려면 먼저 [일반 중단점을 설정](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)합니다. 중단점을 마우스 오른쪽 단추로 클릭하여 팝업 메뉴를 표시합니다.

 [![](debugging-in-xamarin-ios-images/image4vs.png "중단점 팝업 메뉴")](debugging-in-xamarin-ios-images/image4vs.png#lightbox)

**조건...** 을 선택하여 _중단점 설정_ 메뉴를 표시합니다.

 [![](debugging-in-xamarin-ios-images/image6vs.png "중단점 설정 메뉴")](debugging-in-xamarin-ios-images/image6vs.png#lightbox)

여기서 중단점이 발생되게 하는 조건을 입력할 수 있습니다.

이전 버전의 Visual Studio에서 중단점 조건을 사용하는 방법에 대한 자세한 내용은 이 항목에 대한 [Visual Studio 설명서](https://docs.microsoft.com/visualstudio/debugger/using-breakpoints)를 참조하세요.

-----

## <a name="navigating-through-code"></a>코드 탐색

중단점에 도달하면 디버그 도구를 사용하여 프로그램 실행을 제어할 수 있습니다. IDE에서 4개의 단추를 표시하여 코드를 실행하고 단계별로 실행할 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac용 Visual Studio에서는 다음과 같이 표시됩니다.

 [![](debugging-in-xamarin-ios-images/image7.png "개발자가 프로그램 실행을 제어하는 데 사용할 수 있는 디버그 도구")](debugging-in-xamarin-ios-images/image7.png#lightbox)

이러한 항목은 다음과 같습니다.

- **재생/중지** – 다음 중단점까지 코드 실행을 시작/중지합니다.
- **프로시저 단위 실행** - 다음 코드 줄을 실행합니다. 다음 줄이 함수 호출인 경우 프로시저 단위 실행은 함수를 실행하고, 함수 _뒤_의 다음 코드 줄에서 중지합니다.
- **한 단계씩 코드 실행** - 다음 코드 줄을 실행합니다. 다음 줄이 함수 호출인 경우 한 단계씩 코드 실행은 함수의 첫 번째 줄에서 중지되며, 함수 디버깅을 줄 단위로 계속할 수 있도록 합니다. 다음 줄이 함수가 아닌 경우 프로시저 단위 실행과 동일하게 동작합니다.
- **프로시저 나가기** - 현재 함수가 호출된 줄로 돌아갑니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서는 다음과 같이 표시됩니다.

[![](debugging-in-xamarin-ios-images/image7vs.png "개발자가 프로그램 실행을 제어하는 데 사용할 수 있는 디버그 도구")](debugging-in-xamarin-ios-images/image7vs.png#lightbox)

이러한 항목은 다음과 같습니다.

- **재생/중지** – 다음 중단점까지 코드 실행을 시작/중지합니다.
- **프로시저 단위 실행(F11)** - 다음 코드 줄을 실행합니다. 다음 줄이 함수 호출인 경우 프로시저 단위 실행은 함수를 실행하고, 함수 _뒤_의 다음 코드 줄에서 중지합니다.
- **한 단계씩 코드 실행(F10)** - 다음 코드 줄을 실행합니다. 다음 줄이 함수 호출인 경우 한 단계씩 코드 실행은 함수의 첫 번째 줄에서 중지되며, 함수 디버깅을 줄 단위로 계속할 수 있도록 합니다. 다음 줄이 함수가 아닌 경우 프로시저 단위 실행과 동일하게 동작합니다.
- **프로시저 나가기(Shift+F11)** - 현재 함수가 호출된 줄로 돌아갑니다.

디버깅에 대한 자세한 설명은 [Visual Studio 디버거를 사용하여 코드 탐색](https://docs.microsoft.com/visualstudio/debugger/navigating-through-code-with-the-debugger)을 참조하세요.

-----

### <a name="breakpoints"></a>중단점

iOS가 애플리케이션 대리자에서 `FinishedLaunching` 메서드를 시작하고 완료하는 데 몇 초(10)만 애플리케이션에 제공한다는 점을 지적하는 것이 중요합니다. 애플리케이션에서 이 메서드가 10초 내에 완료되지 않으면 iOS에서 해당 프로세스를 종료합니다.

즉 프로그램의 시작 코드에 중단점을 설정하는 것이 거의 불가능합니다. 시작 코드를 디버그하려면 초기화 중 일부를 지연시키고 타이머 호출 메서드 또는 FinishedLaunching이 종료된 후 실행되는 다른 형식의 콜백 메서드에 배치해야 합니다.

## <a name="device-diagnostics"></a>디바이스 진단

디버거를 설정하는 동안 오류가 발생하면 [프로젝트 옵션]에서 "-v -v -v"를 추가 mtouch 인수에 추가하여 자세한 진단을 사용하도록 설정할 수 있습니다. 이렇게 하면 자세한 오류 정보가 디바이스 콘솔에 출력됩니다.

 <a name="WiFi_Debugging" />

## <a name="wireless-debugging"></a>무선 디버깅

Xamarin.iOS는 기본적으로 USB 연결을 통해 디바이스에서 애플리케이션을 디버그합니다. 때로는 ExternalAccessory 지원 애플리케이션을 개발하기 위해 USB 디바이스에서 케이블의 연결/분리를 테스트해야 할 수도 있습니다. 이러한 경우 무선 네트워크를 통해 디버깅을 사용할 수 있습니다.

무선 배포 및 디버깅에 대한 자세한 내용은 [무선 배포](~/ios/deploy-test/wireless-deployment.md) 가이드를 참조하세요.

<a name="Technical_Details" />

## <a name="technical-details"></a>기술 세부 정보

Xamarin.iOS는 새로운 Mono 소프트 디버거를 사용합니다. 별도의 프로세스를 제어하기 위해 운영 체제 인터페이스를 사용하여 별도의 프로세스를 제어하는 프로그램인 표준 Mono 디버거와 달리, 소프트 디버거는 Mono 런타임에서 유선 프로토콜을 통해 디버깅 기능이 노출되도록 함으로써 작동합니다.

시작 시 디버그할 애플리케이션이 디버거에 연결되어 디버거가 작동하기 시작합니다. Visual Studio용 Xamarin.iOS에서 Xamarin Mac 에이전트는 Visual Studio에 있는 애플리케이션과 디버거 사이의 중개자 역할을 합니다.

이 소프트 디버거에는 디바이스에서 실행할 때 공동 작업 디버깅 구성표가 필요합니다. 즉 디버깅을 지원하기 위해 모든 시퀀스 점에서 추가 코드를 포함하도록 코드가 계측되므로 디버깅이 커질 때 바이너리가 빌드됩니다.

<a name="Accessing_the_Console" />

## <a name="accessing-the-console"></a>콘솔 액세스

콘솔 클래스의 크래시 로그 및 출력은 iPhone 콘솔로 전송됩니다. "구성 도우미"를 사용하고 구성 도우미에서 디바이스를 선택하여 Xcode를 통해 이 콘솔에 액세스할 수 있습니다.

또는 Xcode를 시작하지 않으려면 Apple의 [iPhone 구성 유틸리티](http://www.apple.com/support/iphone/enterprise/)를 사용하여 콘솔에 직접 액세스할 수 있습니다. 이 필드에서 문제를 디버그하는 경우 Windows 시스템에서 콘솔 로그에 액세스할 수 있는 기회가 추가로 있습니다.

Visual Studio 사용자의 경우 [출력] 창에서 사용할 수 있는 몇 가지 로그가 있지만, Mac으로 전환해야 더 철저하고 자세한 로그를 볼 수 있습니다.

-----

<a name="Debugging_Mono's_Class_Libraries" />

## <a name="debugging-monos-class-libraries"></a>Mono의 클래스 라이브러리 디버깅

Xamarin.iOS는 Mono의 클래스 라이브러리에 대한 소스 코드와 함께 제공되며, 이 소스 코드를 사용하여 디버거에서 한 단계씩 실행함으로써 작업의 동작 방식을 확인할 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

이 기능은 디버그하는 동안 더 많은 메모리를 사용하므로 기본적으로 꺼져 있습니다.

이 기능을 사용하도록 설정하려면 아래 그림과 같이 _Mac용 Visual Studio > 기본 설정> 디버거_ 메뉴에서 **프로젝트 코드만 디버깅합니다. 프레임워크 코드는 한 단계씩 실행하지 마세요.** 옵션이 선택 취소되어 있는지 확인합니다.

[![](debugging-in-xamarin-ios-images/debugging6.png "Mono의 클래스 라이브러리 디버깅")](debugging-in-xamarin-ios-images/debugging6.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 클래스 라이브러리를 디버그하려면 _디버그 > 옵션_ 메뉴에서 **내 코드만**을 사용하지 않도록 설정해야 합니다. _디버깅 > 일반_ 노드에서 **내 코드만 사용** 확인란을 선택 취소합니다.

[![](debugging-in-xamarin-ios-images/debugging6vs.png "Mono의 클래스 라이브러리 디버깅")](debugging-in-xamarin-ios-images/debugging6vs.png#lightbox)

-----

이렇게 하면 애플리케이션을 시작하고 Mono의 핵심 클래스 라이브러리 중 하나에서 코드를 한 단계씩 실행할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Xamarin을 사용한 디버깅](/visualstudio/mac/debugging/)
- [데이터 시각화](/visualstudio/mac/data-visualizations/)
- [중단점 설정](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)
- [단계별 코드 실행](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)
- [로그 창에 정보 출력](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)
