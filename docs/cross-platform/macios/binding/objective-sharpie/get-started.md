---
title: 목표 Sharpie 시작 하기
description: 이 문서에서는 Sharpie 코드에 대 한 C# 바인딩 만들기를 자동화 하는 데 사용 되는 도구인 목표에 대 한 개략적인 개요를 제공 합니다.
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: conceptdev
ms.author: crdun
ms.date: 10/11/2017
ms.openlocfilehash: c34a6c09bf1298fd710e3e39a244294821a714ae
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290726"
---
# <a name="getting-started-with-objective-sharpie"></a>목표 Sharpie 시작 하기

> [!IMPORTANT]
> 목표 Sharpie는 목표에 대 한 고급 지식을 갖춘 숙련 된 Xamarin 개발자를 위한 도구입니다 (및 확장, C). 목표-C 라이브러리를 바인딩하려면 먼저 명령줄에서 네이티브 라이브러리를 빌드하는 방법에 대 한 확실 한 지식이 있어야 합니다 (그리고 네이티브 라이브러리가 작동 하는 방식을 이해 하는 것이 좋습니다).

<a name="installing" />

## <a name="installing-objective-sharpie"></a>목표 Sharpie 설치

목표 Sharpie 현재 Mac OS X 10.10 이상에 대 한 독립 실행형 명령줄 도구 이며 _완전히 지원 되는 Xamarin 제품이 아닙니다_. 고급 개발자만이를 사용 하 여 타사 목표-C 라이브러리에 대 한 바인딩 프로젝트를 만드는 데 도움을 줍니다.

목표 Sharpie 표준 OS X 패키지 설치 관리자로 다운로드할 수 있습니다.
설치 관리자를 실행 하 고 설치 마법사의 모든 화면 프롬프트를 따릅니다.

- **현재 버전: 3.4**
  - [최신 릴리스 다운로드](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [포럼 공지](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> `sharpie update` 명령을 사용 하 여 최신 버전으로 업데이트 합니다.

## <a name="basic-walkthrough"></a>기본 연습

목표 Sharpie는 타사 목표-C 라이브러리를에 C#바인딩하는 데 필요한 정의를 만드는 데 도움이 되는 Xamarin에서 제공 하는 명령줄 도구입니다.
목표 Sharpie를 사용 하는 경우에 *도 개발자는* Sharpie 완료 된 후에 생성 된 파일을 수정 하 여 도구에서 자동으로 처리할 수 없는 문제를 해결 해야 합니다.

가능한 경우 목표 Sharpie 적절 하 게 바인딩하는 방법에 대해 잘 모르는 Api에 주석을 추가 합니다 (네이티브 코드의 많은 구문이 모호).
이러한 주석은 [ `[Verify]` 특성](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)으로 표시 됩니다.

목표 Sharpie의 출력은 파일 [ `ApiDefinition.cs` `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) 쌍으로, Xamarin 앱에서 사용할 수 있는 라이브러리로 컴파일되는 바인딩 프로젝트를 만드는 데 사용할 수 있습니다.

> [!IMPORTANT]
> 목표 Sharpie 적절 한 사용을 위해 하나의 **주요** 규칙을 제공 합니다. 올바른 구문 분석을 위해 올바른 clang 컴파일러 명령줄 인수를 반드시 전달 해야 합니다. 이는 목표 Sharpie 구문 분석 단계가 단순히 clang 된 도구 [API에 대해 구현](http://clang.llvm.org/docs/LibTooling.html)된 도구 이기 때문입니다.

즉, 목표 Sharpie에는 Clang (실제로 바인딩하려는 네이티브 라이브러리를 컴파일하는 c/목표C++ -c/컴파일러)와 바인딩할 헤더 파일에 대 한 모든 내부 지식이 있습니다.
구문 분석 된 [AST](https://en.wikipedia.org/wiki/Abstract_syntax_tree) 를 개체 코드로 변환 하는 대신, 목표 Sharpie는 ast를 C# `bmac` 및 `btouch` Xamarin 바인딩 도구를 입력 하는 데 적합 한 바인딩 "스 캐 폴드"로 변환 합니다.

구문 분석 중에 Sharpie 오류가 발생 하는 경우 clang는 AST를 구성 하는 동안 구문 분석 단계를 오류 발생 하 고 이유를 파악 해야 합니다.

**새로운!** 버전 3.0 시도는 Xcode 프로젝트를 직접 지원 하 여 이러한 복잡성의 일부를 해결 합니다. 네이티브 라이브러리에 유효한 Xcode 프로젝트가 있으면 목표 Sharpie는 지정 된 대상 및 구성에 대 한 프로젝트를 평가 하 여 필요한 입력 헤더 파일 및 컴파일러 플래그를 추론할 수 있습니다.

Xcode 프로젝트를 사용할 수 없는 경우 올바른 입력 헤더 파일, 헤더 파일 검색 경로 및 기타 필요한 컴파일러 플래그를 추론 하 여 프로젝트에 좀 더 친숙 해야 합니다. 네이티브 라이브러리를 빌드하는 데 사용 되는 컴파일러 플래그는 목표 Sharpie 전달 해야 한다는 것을 알아야 합니다. 이 프로세스는 더 수동 프로세스 이며, Clang 도구 체인를 사용 하 여 명령줄에서 네이티브 코드를 컴파일하는 것에 대해 약간의 지식이 필요 합니다.

**새로운!** 버전 3.0에서는 `sharpie pod` 명령을 통해 [CocoaPods](https://cocoapods.org) 을 쉽게 바인딩할 수 있는 도구도 제공 합니다.
관심이 있는 라이브러리를 CocoaPod 사용할 수 있는 경우에는 CocoaPod를 목표로 Sharpie (원본에 직접 바인딩하려는 시도)를 시도 하 여 시작 하는 것이 좋습니다.
