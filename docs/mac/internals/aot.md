---
title: "미리 컴파일 Xamarin.Mac"
description: "앞의 시간 (AOT) 컴파일 및 고려 사항"
ms.topic: article
ms.prod: xamarin
ms.assetid: 38B8A017-5A58-429C-A6E9-9860A1DCEF63
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: f9ace41380987472b6957c94719e6ae9b6f7995d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinmac-ahead-of-time-compilation"></a>미리 컴파일 Xamarin.Mac

## <a name="overview"></a>개요

미리 (AOT) 시간의 컴파일은 시작 성능을 향상 시키기 위한는 강력한 최적화 기술입니다. 그러나 것도 영향을 줍니다 빌드 시간, 응용 프로그램 크기 및 프로그램 실행도 많은 방식입니다. 부과 장단점을 이해 하려면 컴파일 및 응용 프로그램의 실행 약간 살펴보도록 하겠습니다.

C# 및 F # 등의 관리 되는 언어로 작성 된 코드는 IL 라는 중간 표현으로 컴파일됩니다. 라이브러리 및 프로그램 어셈블리에 저장 된이 IL 상대적으로 간단 하 고 프로세서 아키텍처 간의 휴대용 됩니다. 그러나 IL 명령의 및 IL 프로세서별 컴퓨터 코드로 변환 해야 하는 특정 시점에 설정 하는 중간만을입니다.

다음 두 가지 사항을 이러한 처리를 수행할 수 있습니다.

- **(JIT) just-in-time에서** – 시작 및 IL 메모리 기계어 코드로 컴파일된 응용 프로그램 실행 중입니다.
- **(AOT)을 미리** – 빌드 IL의 컴파일된 네이티브 라이브러리에 기록 및 응용 프로그램 번들 내에 저장 하는 동안 합니다.

각 옵션에는 다양 한 장점 및 단점에 있습니다.

- **JIT**
  - **시작 시간은** – JIT 컴파일 시작 시에 수행 해야 합니다. 대부분의 응용 프로그램에 대 한이 시간은 100ms, 이지만 대규모 응용 프로그램에 대 한이 이번 훨씬 더 많은 수 있습니다.
  - **실행** – 사용 중인 특정 프로세서에 대 한 JIT로 코드를 최적화할 수 있습니다, 약간 더 나은 코드를 생성할 수 있습니다. 대부분의 응용 프로그램에서 이것이 하면 몇 퍼센트 더 빠르게 최대입니다.
- **AOT**
  - **시작 시간은** – 미리 컴파일된 dylibs를 로드 하는 것은 JIT 어셈블리 보다 훨씬 빠릅니다.
  - **하지만 디스크 공간** – 해당 dylibs 많은 양의 디스크 공간을 걸릴 수 있습니다. 두 수에 따라 어떤 어셈블리가 AOTed, 또는 응용 프로그램의 코드 부분의 크기에 더 합니다.
  - **빌드 시간** – AOT 컴파일 성능이 크게 저하 됩니다. 해당 JIT 및이 사용 하 여 빌드 속도가 느려집니다. 컴파일된 어셈블리의 수와 크기에 따라 더 또는 1 분까지 초에서이 일어난이 까지입니다.
  - **난독 처리** – 해당 하는 작업이 훨씬 수월해 리버스 엔지니어링 기계어 코드 보다 반드시 필요 하지 않은 IL로 중요 한 코드를 난독 처리를 제거할 수 있습니다. 이 옵션을 설명 아래 "하이브리드" 해야 합니다.

## <a name="enabling-aot"></a>AOT를 사용 하도록 설정

AOT 옵션 향후 업데이트에서 Mac에서 빌드 창에 추가 됩니다. 그때 까지는 사용 AOT 하려면 Mac에서 빌드에서 "추가 mmp 인수" 필드를 통해 명령줄 인수를 전달 합니다. 다음과 같은 위치 지정 옵션을 사용할 수 있습니다.


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



## <a name="hybrid-aot"></a>하이브리드 AOT

런타임에 macOS 응용 프로그램을 실행 하는 동안 기계어 코드를 사용 하 여 기본값으로 AOT 컴파일에서 생성 된 네이티브 라이브러리에서 로드 되었습니다. 그러나 가지, trampolines, 같은 코드의 JIT 컴파일 크게 보다 최적화 된 결과 생성할 수 있는 일부 영역입니다. 관리 되는 어셈블리 IL을 사용할 수 있어야 합니다. Ios에서 응용 프로그램에서 사용 되는 JIT 컴파일;의 모든 제한 코드의 해당 섹션도 컴파일된 AOT 됩니다.

하이브리드 옵션 모두 컴파일하도록 컴파일러 (예: iOS) 이러한 섹션도 하기 위해 IL에서 런타임에 사용할 수 없음을 가정 하지만 지시 합니다. 이 IL에 수 하이픈이 제거 되어 다음 빌드를 게시 합니다. 위에서 언급 한 대로 런타임 루틴을 사용 하도록 덜 최적화 된 일부 상황에서는 정해 집니다.

## <a name="further-considerations"></a>추가 고려 사항

AOT 눈금 크기 및 처리 하는 어셈블리의 수는 부정적인 결과가 발생 합니다. 전체 [대상 프레임 워크](~/mac/platform/target-framework.md) 에 대 한 예는 훨씬 큰 클래스 라이브러리 BCL (기본) 보다 최신, 포함 되어 있습니다. 따라서 AOT 시간이 많이 소요 되며 더 큰 번들을 생성 합니다. 더 복잡해 집니다이 전체 대상 프레임 워크의 비 호환성으로 연결을 여는 제거 사용 되지 않는 코드입니다. 최상의 결과 현대적이 고 사용할 수 있도록 연결 하도록 응용 프로그램을 이동 하는 것이 좋습니다.

AOT의 추가적인 이점 중 하나는 네이티브 디버깅 및 toolchains 프로 파일링의 향상 된 상호 함께 제공 됩니다. 대부분 코드 베이스의 미리 컴파일할 수 있으므로 함수 이름 및 기호를 보다 쉽게 읽을 네이티브 충돌 보고서, 프로 파일링 및 디버깅 내부 가집니다. 생성 된 JIT 함수 이름이이 해결 하기 매우 어려운 명명 되지 않은 16 진수 오프셋으로 나타나는 경우가 마십시오.
