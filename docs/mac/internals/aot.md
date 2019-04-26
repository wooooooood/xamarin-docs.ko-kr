---
title: Xamarin.Mac 미리 컴파일
description: 이 문서에서는 xamarin.mac에서 컴파일 미리 설명 합니다. JIT 컴파일으로 AOT 컴파일 비교, AOT를 사용 하도록 설정 하는 방법에 설명 하 고 하이브리드 AOT 살펴봅니다.
ms.prod: xamarin
ms.assetid: 38B8A017-5A58-429C-A6E9-9860A1DCEF63
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: e155a394afd68d9970ee32785f6d0aeda6e2d129
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61034206"
---
# <a name="xamarinmac-ahead-of-time-compilation"></a>Xamarin.Mac 미리 컴파일

## <a name="overview"></a>개요

계속 해 서 (AOT) 시간의 컴파일이 시작 성능 향상을 위한 강력한 최적화 기술입니다. 그러나이 영향을 미칩니다에 빌드 시간, 응용 프로그램 크기 및 프로그램 실행 심오한 방식입니다. 부과 장단점을 이해 하려면 컴파일 및 응용 프로그램의 실행은 잠시 살펴보도록 하겠습니다.

작성 된 코드와 같은 언어를 관리 합니다 C# 및 F#를 IL 호출 중간 표현으로 컴파일됩니다. 라이브러리 및 프로그램 어셈블리에 저장 된이 IL이 비교적 간단 하 고 프로세서 아키텍처 간의 이식 가능한 경우 IL 것 인데, 지침 및 IL 프로세서별 기계어 코드로 변환 해야 하는 특정 시점을 설정 하는 중간만 합니다.

다음 두 가지 사항을이 처리를 수행할 수 있습니다.

- **(JIT) just-in-time에서** – 시작 및 IL 메모리 기계어 코드로 컴파일된 응용 프로그램 실행 중입니다.
- **AOT ()을 미리** – IL 컴파일 및 네이티브 라이브러리를 작성 및 사용자 응용 프로그램 번들 내에 저장 하는 빌드 중입니다.

각 옵션에는 다양 한 장단점에 있습니다.

- **JIT**
  - **시작 시간** – JIT 컴파일 시 수행 해야 합니다. 대부분의 응용 프로그램에 대 한이 시간은 100ms, 있지만 대규모 응용 프로그램에 대 한이 이번 훨씬 더 많은 수 있습니다.
  - **실행** – 사용 중인 특정 프로세서에 대 한 JIT로 코드를 최적화할 수 있습니다, 약간 더 나은 코드를 생성할 수 있습니다. 대부분의 응용 프로그램에서 이것이 몇 퍼센트 더 빨리 최대입니다.
- **AOT**
  - **시작 시간** -미리 컴파일된 dylibs를 로드 하는 것은 JIT 어셈블리 보다 훨씬 빠릅니다.
  - **하지만 디스크 공간** – 해당 dylibs 상당한 양의 디스크 공간을 걸릴 수 있습니다. 두 수에 따라 어셈블리 AOTed, 또는 응용 프로그램의 코드 부분의 크기를 더 합니다.
  - **빌드 시간** – AOT 컴파일 훨씬 느립니다. 해당 JIT 및이 사용 하 여 빌드 속도가 느려집니다. 이 속도가 느려지는 최대 1 분 또는 컴파일된 어셈블리의 수와 크기에 따라 자세한 시간 (초)에서 까지입니다.
  - **난독 처리** – IL를 리버스 엔지니어링 하 기계어 코드 보다 훨씬 쉽습니다는 반드시 필요 하지 않은 중요 한 코드를 난독 처리 하는 데 제거 될 수 있습니다. 이렇게 하려면 "하이브리드" 아래 옵션에 설명 합니다.

## <a name="enabling-aot"></a>AOT를 사용 하도록 설정

AOT 옵션 향후 업데이트에서 Mac 빌드 창에 추가 됩니다. 그때 까지는 사용 AOT 하려면 Mac 빌드에 "추가 mmp 인수" 필드를 통해 명령줄 인수를 전달 합니다. 다음과 같은 위치 지정 옵션을 사용할 수 있습니다.


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

MacOS 런타임에서 응용 프로그램을 실행 하는 동안 기계어 코드를 사용 하 여 기본값으로 AOT 컴파일으로 생성 된 네이티브 라이브러리에서 로드 합니다. 그러나가 trampolines, 같은 코드의 JIT 컴파일 크게 보다 최적화 된 결과 생성할 수 있는 몇 가지 영역입니다. 관리 되는 어셈블리 IL를 사용할 수 있어야 합니다. JIT 컴파일 사용에서 제한할 응용 프로그램을 ios의 경우 해당 코드 섹션에는 AOT 컴파일되어야도 합니다.

하이브리드 옵션을 컴파일러에 지시 합니다 두 컴파일 (예: iOS) 이러한 섹션에 가정 IL 런타임에 사용할 수는 있지만 합니다. 이 IL 제거 될 수 있습니다 다음 빌드를 게시 합니다. 위에서 언급 했 듯이 런타임에서 해야만 일부 위치에서는 덜 최적화 된 루틴을 사용 합니다.

## <a name="further-considerations"></a>추가 고려 사항

처리 하는 어셈블리의 수와 크기를 사용 하 여 AOT 범위의 부정적인 결과가 발생 합니다. 전체 [대상 프레임 워크](~/mac/platform/target-framework.md) 예제에는 훨씬 큰 클래스 라이브러리 (BCL (기본) 보다 최신이 고 따라서 AOT 시간이 많이 소요 되며 큰 번들을 생성 합니다. 더 심해 집니다이 사용 되지 않는 코드 제거 하는 링크를 사용 하 여 전체 대상 프레임 워크의 비 호환성으로 합니다. 최상의 결과 사용 하도록 설정 하 고 최신 연결 응용 프로그램을 이동 하는 것이 좋습니다.

AOT의 추가 이점 중 하나는 네이티브 디버깅 및 도구 체인을 프로 파일링을 사용 하 여 향상 된 상호 작용을 사용 하 여 제공 됩니다. 미리 컴파일할 코드 베이스의 대부분 이므로 함수 이름과 네이티브 크래시 보고서를 프로 파일링 및 디버깅을 읽어서 쉽게 않은 기호가 생성 됩니다. 생성 된 JIT 함수 이러한 이름은 없고 종종 해결 하는 것이 어려울 수 있는 명명 되지 않은 16 진수 오프셋으로 표시 합니다.
