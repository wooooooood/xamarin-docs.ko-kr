---
title: 목표 Sharpie 시작
description: 이 문서에서는의 생성을 자동화 하는 데 사용 하는 도구, 목표 Sharpie의 대략적인 개요를 제공 C# Objective-c 코드에 대 한 바인딩을 합니다.
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 4fb5e503a82a2b666bf6f8d7d7166475e94546e7
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978410"
---
# <a name="getting-started-with-objective-sharpie"></a>목표 Sharpie 시작

> [!IMPORTANT]
> 목표 Sharpie는 고급 기술 자료의 Objective C (및 확장명, C)를 사용 하 여 숙련 된 Xamarin 개발자를 위한 도구. Objective-c 라이브러리 바인딩 하기 전에 명령줄 (및 네이티브 라이브러리의 작동 방식을 이해)에서 네이티브 라이브러리를 빌드하는 방법 확실히 알고가 있어야 합니다.

<a name="installing" />

## <a name="installing-objective-sharpie"></a>목표 Sharpie 설치

목표 Sharpie 현재 Mac OS X 10.10 및 이상에서 독립 실행형 명령줄 도구 이며 _완벽 하 게 지원 되는 Xamarin 제품이 아니라_합니다. 만 사용할 고급 개발자가 타사 Objective-c 라이브러리 바인딩 프로젝트를 만드는 지원 하기 위해.

목표 Sharpie 표준 OS X 패키지 설치 프로그램으로 다운로드할 수 있습니다.
설치 관리자를 실행 하 고 모든 따릅니다는 설치 마법사에서 화면의 프롬프트:

- **현재 버전: 3.4**
  - [최신 릴리스 다운로드](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [포럼 알림](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> 사용 된 `sharpie update` 최신 버전으로 업데이트 하는 명령입니다.

## <a name="basic-walkthrough"></a>기본 연습

목표 Sharpie는 타사 Objective-c 라이브러리를 바인딩하는 데 필요한 정의 만드는 데 도움이 되는 Xamarin에서 제공 하는 명령줄 도구는 C#입니다.
목표 Sharpie, 개발자를 사용 하는 경우에 *는* 목표 Sharpie 도구에서 자동으로 처리할 수 없는 문제가 해결 하기 위해 완료 된 후 생성된 된 파일을 수정 해야 합니다.

목표 Sharpie Api는 올바르게 바인딩하는 방법에 몇 가지 확실 하지 않은 주석을 달아서 한 가능할 경우 (네이티브 코드의 많은 구문을 모호한).
이러한 주석으로 표시 됩니다 [ `[Verify]` 특성](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)합니다.

목표 Sharpie의 출력은 파일-쌍 [ `ApiDefinition.cs` 하 고 `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) -Xamarin 앱에서 사용할 수는 라이브러리로 컴파일하 바인딩 프로젝트를 만드는 데 사용할 수 있는 합니다.

> [!IMPORTANT]
> 목표 Sharpie 하나가 함께 제공 됩니다 **주요** 적절 한 사용에 대 한 규칙: 반드시 전달 해야 올바른 clang 컴파일러 명령줄 인수를 적절 한 구문 분석 되도록 합니다. 단계를 구문 분석 목표 Sharpie는 단순히 도구 이므로 이것이 [clang libtooling API에 대 한 구현](http://clang.llvm.org/docs/LibTooling.html)합니다.

즉, 목표 Sharpie Clang의 모든 기능 (C/Objective C /C++ 바인딩하는 실제로 네이티브 라이브러리를 컴파일되지 않는 컴파일러) 및 모든 바인딩에 대 한 헤더 파일의 내부 정보입니다.
구문 분석 된 변환 하는 대신 [AST](https://en.wikipedia.org/wiki/Abstract_syntax_tree) 개체 코드를 목표 Sharpie 변환에 대 한 AST를 C# "스 캐 폴드" 입력에 대 한 적합 한에 바인딩를 `bmac` 및 `btouch` Xamarin 바인딩 도구입니다.

목표 Sharpie 오류가 출력을 구문 분석 하는 동안 의미 하는 동안 오류 발생 하는 clang AST를 생성 하는 동안 단계는 구문 분석 하 고 이유를 파악 해야 합니다.

**새로운!** 버전 3.0 시도 Xcode 프로젝트를 직접 지원 함으로써 이러한 복잡성 중 일부를 해결 합니다. 네이티브 라이브러리의 유효한 Xcode 프로젝트에 있으면 목표 Sharpie 지정 된 대상 및 필요한 입력된 헤더 파일 및 컴파일러 플래그를 추론 하는 구성에 대 한 프로젝트를 평가할 수 있습니다.

Xcode 프로젝트가 없습니다. 사용 가능한 경우 올바른 입력된 헤더 파일, 헤더 파일 검색 경로 및 기타 필요한 컴파일러 플래그를 추론 하 여 프로젝트에 더 익숙합니다 되도록 해야 합니다. 네이티브 라이브러리를 빌드하는 데 사용 된 컴파일러 플래그 목표 Sharpie를 전달 해야 하는 동일한 지를 실현 하는 것이 반드시 합니다. 수동 프로세스와 Clang 도구 체인을 사용 하 여 명령줄에서 네이티브 코드 컴파일 사용 경험 많은 필요가 하는 것입니다.

**새로운!** 버전 3.0에도 쉽게 바인딩에 대 한 도구 소개 [CocoaPods](https://cocoapods.org) 를 통해를 `sharpie pod` 명령입니다.
관심이 있다면 라이브러리는 CocoaPod로 사용 가능한 경우 원본에 대해 직접 바인딩을 시도) (대비 목표 Sharpie를 사용 하 여 CocoaPod를 바인딩할 시도 하 여 시작 하는 것이 좋습니다.
