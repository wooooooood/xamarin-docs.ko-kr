---
title: 목표 Sharpie 시작
description: 이 문서에서는 목표 Sharpie, Objective C 코드에 대 한 바인딩 C#의 만들기를 자동화 하는 데 사용 하는 도구에 대 한 고급 개요를 제공 합니다.
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: a0431f8952055a55be24ae5f85381a4295206a40
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781818"
---
# <a name="getting-started-with-objective-sharpie"></a>목표 Sharpie 시작

> [!IMPORTANT]
> 목표 Sharpie는 고급 정보 Objective C의 (및 확장명, C)로 숙련 된 Xamarin 개발자를 위한 도구입니다. Objective C 라이브러리를 바인딩하는 명령줄 (및 네이티브 라이브러리의 작동 방식을 이해)에서 네이티브 라이브러리를 작성 하는 방법 확실히 알고가 있어야 합니다.

<a name="installing" />

## <a name="installing-objective-sharpie"></a>목표 Sharpie 설치

목표 Sharpie 현재 Mac OS X 10.10 이상 버전에서는 독립 실행형 명령줄 도구 이며 _완벽 하 게 지원 되는 Xamarin 제품과_합니다. 만 바인딩 프로젝트를 만드는 타사 Objective C 라이브러리를 지원 하기 위해 고급 개발자가 사용 해야 합니다.

목표 Sharpie 표준 OS X 패키지 설치 프로그램으로 다운로드할 수 있습니다.
설치 프로그램을 실행 하 고 모든 수행 된 설치 마법사에서 화면의 프롬프트:

- **현재 버전: 3.4**
  - [최신 릴리스를 다운로드 합니다.](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [포럼 알림](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> 사용 하 여는 `sharpie update` 최신 버전으로 업데이트 하는 명령입니다.

## <a name="basic-walkthrough"></a>기본 연습

목표 Sharpie는 C# 3rd 파티 Objective C 라이브러리를 바인딩하는 데 필요한 정의 만드는 데 도움이 되는 Xamarin으로 제공 된 명령줄 도구입니다.
목표 Sharpie, 개발자를 사용 하는 경우에 *됩니다* 목표 Sharpie 도구에 의해 자동으로 처리할 수 없는 모든 문제를 해결 하기 위해 완료 된 후 생성 된 파일을 수정 해야 합니다.

목표 Sharpie Api는 제대로 바인딩하는 방법에 확신이 아무런 주석 달기는 가능한 경우 (많은 구문을 네이티브 코드에는 모호).
이러한 주석으로 표시 됩니다 [ `[Verify]` 특성](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)합니다.

목표 Sharpie의 출력은 한 쌍의 파일- [ `ApiDefinition.cs` 및 `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) -프로젝트를 만드는 바인딩을 라이브러리로 컴파일하고 어떤 Xamarin 앱에서 사용할 수 있습니다 사용할 수 있는 합니다.

> [!IMPORTANT]
> 목표 Sharpie 하나와 함께 제공 **주요** 적절 한 사용에 대 한 규칙: 절대적으로 전달 해야 올바른 clang 컴파일러 명령줄 인수 적절 한 구문 분석 되도록 합니다. 이 단계를 구문 분석 목표 Sharpie는 단순히 도구는 [clang libtooling API에 대 한 구현](http://clang.llvm.org/docs/LibTooling.html)합니다.

즉, 목표 Sharpie Clang (실제로 바인딩하는 것으로 네이티브 라이브러리를 컴파일하는 C/Objective C/c + + 컴파일러) 및 모든 바인딩에 대 한 헤더 파일에 대 한 내부 정보를 최대한 활용 합니다.
구문 분석 된 변환 하는 대신 [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree) 개체 코드를 C# 바인딩 "스 캐 폴드" 입력에 대 한 적합 한에 대 한 AST를 변환 하는 목표 Sharpie는 `bmac` 및 `btouch` Xamarin 바인딩 도구입니다.

오류를 구문 분석 하는 동안 목표 Sharpie, 의미 하는 동안 오류가 해당 clang/의 구문 분석 단계는 AST 생성 하려고 하 고 이유가 무엇 인지 생각해 야 합니다.

**새로운!** 버전 3.0 시도 Xcode 프로젝트를 직접 지원 하 여 이러한 복잡성의 일부를 해결 합니다. 네이티브 라이브러리 유효한 Xcode 프로젝트 있으면 목표 Sharpie 지정 된 대상 및 필요한 입력된 헤더 파일 및 컴파일러 플래그를 추론 하는 구성에 대 한 프로젝트를 평가할 수 있습니다.

사용할 수 있는 Xcode 프로젝트가 없으면으로 자리를 잡게 프로젝트에 대해 올바른 입력된 헤더 파일, 헤더 파일 검색 경로 및 기타 필요한 컴파일러 플래그를 추론 해야 합니다. 네이티브 라이브러리를 빌드하는 데 사용 된 컴파일러 플래그 목표 Sharpie에 전달 해야 하는 동일한 지를 나타내려고 하는 것이 유용 합니다. 이것이 수동 프로세스 및 1 Clang 도구 체인을 사용 하 여 명령줄에서 네이티브 코드 컴파일 익숙하다고 다소 필요 합니다.

**새로운!** 버전 3.0도 쉽게 바인딩에 대 한 도구를 소개 [CocoaPods](https://cocoapods.org) 통해는 `sharpie pod` 명령입니다.
에 관심이 라이브러리는 CocoaPod로 사용할 수 있는 경우에 목표 Sharpie와 CocoaPod (아닌 소스에 대해 직접 바인딩하는) 바인딩할 시도 하 여 시작 하는 것이 좋습니다.
