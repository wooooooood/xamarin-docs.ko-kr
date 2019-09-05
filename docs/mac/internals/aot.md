---
title: 이전 컴파일에서 xamarin.ios
description: 이 문서에서는 Xamarin.ios에서 시간 컴파일을 미리 설명 합니다. AOT 컴파일을 JIT 컴파일과 비교 하 고, AOT를 사용 하도록 설정 하는 방법에 대해 설명 하 고, 하이브리드 AOT를 살펴보겠습니다.
ms.prod: xamarin
ms.assetid: 38B8A017-5A58-429C-A6E9-9860A1DCEF63
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 11/10/2017
ms.openlocfilehash: e1f1e2e1e5dbec7dc8f2310b3f9565d0bc209c00
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283688"
---
# <a name="xamarinmac-ahead-of-time-compilation"></a>이전 컴파일에서 xamarin.ios

## <a name="overview"></a>개요

AOT (사전) 컴파일은 시작 성능을 향상 시키기 위한 강력한 최적화 기술입니다. 그러나 빌드 시간, 응용 프로그램 크기 및 프로그램 실행에는 다양 한 방식으로 영향을 줍니다. 이에 의해 적용 되는 장단점을 이해 하기 위해 응용 프로그램의 컴파일 및 실행에 대해 알아보겠습니다.

및 C# F#와 같이 관리 되는 언어로 작성 된 코드는 IL 이라는 중간 표현으로 컴파일됩니다. 라이브러리 및 프로그램 어셈블리에 저장 된이 IL은 프로세서 아키텍처 간에 비교적 간단 하 고 이식할 수 있습니다. 그러나 IL은 단지 일련의 지침을 가지 며 특정 시점에서 IL은 프로세서와 관련 된 기계어 코드로 변환 해야 합니다.

이 처리를 수행할 수 있는 두 가지 지점이 있습니다.

- **JIT (just-in-time)** – 응용 프로그램을 시작 하 고 실행 하는 동안 IL은 메모리에서 기계어 코드로 컴파일됩니다.
- **사전 (AOT)** – 빌드하는 동안 IL이 컴파일되고 응용 프로그램 번들 내에 저장 됩니다.

각 옵션에는 다음과 같은 다양 한 장단점이 있습니다.

- **JIT**
  - **시작 시간** – 시작 시 JIT 컴파일이 수행 되어야 합니다. 대부분의 응용 프로그램의 경우이는 100ms의 순서에 따라 다르지만 큰 응용 프로그램의 경우이 시간이 훨씬 더 많을 수 있습니다.
  - **실행** – 사용 중인 특정 프로세서에 대해 JIT 코드를 최적화할 수 있으므로 약간 더 나은 코드가 생성 될 수 있습니다. 대부분의 응용 프로그램에서이는 몇 가지 비율을 가장 빠르게 처리 합니다.
- **AOT**
  - **시작 시간** – JIT 어셈블리 보다 미리 컴파일된 dylibs를 로드 하는 속도가 훨씬 빠릅니다.
  - **디스크 공간** – 이러한 dylibs는 상당한 양의 디스크 공간을 사용할 수 있습니다. AOTed 된 어셈블리에 따라 응용 프로그램의 코드 부분 크기를 두 배 이상 지정할 수 있습니다.
  - **빌드 시간** – AOT 컴파일을 사용 하면 JIT가이를 사용 하 여 빌드 속도가 느려질 수 있습니다. 이 속도는 컴파일되는 어셈블리의 크기와 수에 따라 몇 분 이상 걸릴 수 있습니다.
  - **난독 처리** -시스템 코드 보다 훨씬 더 쉽게 리버스 엔지니어링할 수 있는 IL로, 중요 한 코드를 난독 처리 하는 데 도움이 되도록 제거할 수 있습니다. 여기에는 아래에 설명 된 "하이브리드" 옵션이 필요 합니다.

## <a name="enabling-aot"></a>AOT 사용

AOT 옵션은 향후 업데이트에서 Mac 빌드 창에 추가 됩니다. 그때까지 AOT를 사용 하도록 설정 하려면 Mac 빌드에서 "추가 mmp 인수" 필드를 통해 명령줄 인수를 전달 해야 합니다. 다음과 같은 위치 지정 옵션을 사용할 수 있습니다.

```
--aot[=VALUE]          Specify assemblies that should be AOT compiled
                          - none - No AOT (default)
                          - all - Every assembly in MonoBundle
                          - core - Xamarin.Mac, System, mscorlib
                          - sdk - Xamarin.Mac.dll and BCL assemblies
                          - |hybrid after option enables hybrid AOT which
                          allows IL stripping but is slower (only valid
                          for 'all')
                          - Individual files can be included for AOT via +
                          FileName.dll and excluded via -FileName.dll

                          Examples:
                            --aot:all,-MyAssembly.dll
                            --aot:core,+MyOtherAssembly.dll,-mscorlib.dll
```


## <a name="hybrid-aot"></a>하이브리드 AOT

MacOS 응용 프로그램을 실행 하는 동안 런타임의 기본값은 AOT 컴파일에 의해 생성 된 네이티브 라이브러리에서 로드 된 기계어 코드를 사용 하는 것입니다. 그러나 JIT 컴파일이 훨씬 더 최적화 된 결과를 생성할 수 있는 trampolines와 같은 일부 코드 영역이 있습니다. 이렇게 하려면 관리 되는 어셈블리를 사용할 수 있어야 합니다. IOS에서 응용 프로그램은 JIT 컴파일을 사용 하는 것으로 제한 됩니다. 이러한 코드 섹션은 AOT 컴파일됩니다.

하이브리드 옵션은 이러한 섹션 (예: iOS)을 컴파일하도록 컴파일러에 지시 하 고 런타임에 IL을 사용할 수 없다고 가정 합니다. 그런 다음이 IL을 빌드 후 제거할 수 있습니다. 위에서 언급 한 것 처럼 런타임은 최적화 되지 않은 루틴을 특정 위치에서 사용 하도록 강제 합니다.

## <a name="further-considerations"></a>추가 고려 사항

처리 된 어셈블리의 크기 및 수와 함께 AOT 크기를 음수로 표시 합니다. 예를 들어 전체 [대상 프레임 워크](~/mac/platform/target-framework.md) 에는 현대 보다 훨씬 더 큰 BCL (기본 클래스 라이브러리)이 포함 되어 있으므로 AOT이 훨씬 더 오래 걸리고 더 큰 번들이 생성 됩니다. 이는 사용 되지 않는 코드를 제거 하는 연결과의 전체 대상 프레임 워크의 비호환에 의해 복잡해 집니다. 응용 프로그램을 최신으로 전환 하 고 최상의 결과를 위해 링크를 사용 하도록 설정 하는 것이 좋습니다.

AOT의 추가 혜택 중 하나는 네이티브 디버깅 및 프로 파일링 도구 체인을 사용 하 여 향상 된 상호 작용을 제공 합니다. 코드 베이스의 대부분은 미리 컴파일되므로 네이티브 충돌 보고서, 프로 파일링 및 디버깅 내에서 읽기가 더 쉬운 함수 이름 및 기호가 있습니다. JIT 생성 함수는 이러한 이름을 포함 하지 않으며 해결 하기 매우 어려운 명명 되지 않은 16 진수 오프셋으로 표시 되는 경우가 많습니다.
