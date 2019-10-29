---
title: 버그 보고서를 언제 어떻게 제출해야 하나요?
description: 이 문서에서는 버그 보고서를 파일에 저장 하는 시기, 위치 및 방법을 설명 합니다. 또한 엔지니어가 문제를 가장 잘 진단할 수 있도록 버그 보고서 모범 사례를 제공 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: davidortinau
ms.author: daortin
ms.date: 08/01/2018
ms.openlocfilehash: df00eebe682d2d06b99721a2d3c3b90d13454c75
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014003"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>버그 보고서를 언제 어떻게 제출해야 하나요?

> [!TIP]
> Visual &ndash; Studio의 **문제 보고** 메뉴 항목을 사용 하 여 문제를 해결 하는 데 도움이 되는 버그 보고서와 함께 진단 정보를 보냅니다.
>
> [Visual studio 2019 또는 Visual studio 2017](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio) 및 [Mac용 Visual Studio](https://docs.microsoft.com/visualstudio/mac/report-a-problem)에 대 한 자세한 지침이 있습니다.
>
> [Visual Studio 개발자 커뮤니티](https://developercommunity.visualstudio.com/) 웹 사이트에서 기존 보고서를 검색할 수 있습니다.

## <a name="file-a-bug-if"></a>버그를 파일에

엔지니어가 문제를 재현 하는 데 사용할 수 있다고 생각 하는 일련의 단계가 있습니다.

또는

문제에 대 한 눈에 띄는 증상을 신중 하 게 설명할 수 있습니다. 특히 문제와 관련 된 몇 가지 정확한 상황을 설명할 수도 있습니다. <sup> [[1]](#note-1)</sup>

## <a name="best-practices-to-help-address-bugs-quickly-and-efficiently"></a>버그를 신속 하 고 효율적으로 해결 하는 데 유용한 모범 사례

1. 문제를 직접 해결할 수 있는 기존 버그 보고서 또는 사용 제안에 대 한 [Visual Studio 개발자 커뮤니티](https://developercommunity.visualstudio.com/) 및 웹 검색을 <a name="ref-1" />합니다. <sup>[[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. 발생 한 상황에 대 한 설명과 수행할 것으로 예상 되는 문제를 포함 하 여 문제를 명확 하 고 간결 하 게 설명 <a name="ref-2" />.

1. 관련 스택 추적, 오류 메시지 텍스트 또는 크래시 로그를 포함 하는 <a name="ref-3" />( **문제 보고** 기능을 사용 하는 경우 자동으로 포함 될 수 있음) <sup>[3-4](#note-4)</sup>

1. 스크린샷 첨부 파일에 표시 되는 중요 한 오류 메시지를 일반 텍스트로 기록 <a name="ref-4" />합니다.

1. <a name="ref-5" />는 버그를 가능한 한 적은 코드로 재현 하는 작은 자체 포함 테스트 사례를 포함 합니다.  기본 제공 템플릿 중 하나를 사용 하 여 만든 새 프로젝트를 사용 하 여 문제를 재현할 수 없는 경우 문제를 설명 하는 프로젝트를 압축 하 고 버그 보고서에 연결 하세요.  예제 프로젝트를 연결 하기 전에 최대한 단순하게 만듭니다. <sup>[[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. 운영 체제 및 [버전의 Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md) 및 모든 종속성을 포함 하 여 버그가 발생 한 환경을 설명 하는 <a name="ref-6" />.

## <a name="additional-details"></a>추가 정보

1. <a name="note-1" />[ *^* ](#ref-1) 하는 것이 좋습니다. "표시 되는 증상"에는 다른 고객이 동일한 문제 (동일한 오류 메시지, 동일한 성능 저하, 동일한 스택 추적)를 확인할 수 있도록 충분 한 정보가 포함 되어야 합니다. 충돌 등 _)._ "정확한 상황"의 경우 한 가지 좋은 예는 다음과 같습니다. "일반적으로 75%의 시간 동안 문제가 발생 하지만,이 항목을 변경 하면 문제를 완전히 방지할 수 있습니다." "정확한 상황"의 또 다른 유사한 예는 이전 버전의 Xamarin으로 다운 그레이드 하는 경우 문제를 중지 하는 경우입니다.

1. <a name="note-2" />[ *^* ](#ref-2) , 오류 텍스트 조각 (또는 고유 설명 텍스트)은 일반적으로 최상의 검색 용어입니다. 기존 버그 보고서가 불완전 한 경우 세부 정보를 추가 하거나 더 나은 새 버그 보고서를 파일에 추가 하는 것을 환영 합니다.

1. [ *^* ](#ref-3) <a name="note-3" />다른 좋은 질문은 Java, 객관적인 C 또는 Swift apps에 대해 동일한 문제가 보고 되었는지 여부입니다. 그렇다면이 문제는 Xamarin의 일부가 아닌 Android 또는 iOS 자체의 일부일 가능성이 높습니다.

1. <a name="note-4" />는 다음과 같은 몇 가지 정보의 예를 [ *^* ](#ref-4) 합니다.

    1. 프로젝트를 빌드할 때 발생 하는 오류에 대 한 자세한 내용은 버그 보고서에 전체 [진단 빌드 출력](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) 을 포함 하세요.

    1. Visual Studio에서 iOS 프로젝트를 빌드하거나 디버그할 때 발생 하는 오류의 경우 오류에 도달 하 고 결과 .zip 파일을 버그 보고서에 포함 하 여 **Xamarin > Zip 로그 > 도움말** 을 실행 하세요.

    1. Android 또는 iOS 앱의 예외 또는 충돌에 대해 [Xamarin android 및 xamarin.ios 앱에 대 한 관련 디버그 로그](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)를 포함 하세요.

1. <a name="note-5" />[ *^* ](#ref-5) 특정 문제에 대해 가능한 한 한 가지 방법은 원래 솔루션의 적은 수의 파일을 새로운 솔루션에 추가 하 여 문제를 다시 만드는 것입니다. Xamarin 팀은 종종 큰 테스트 사례 (재현 단계를 명확 하 게 설명 하는 것으로 가정) 에서도 문제를 조사할 수 있지만 더 간단한 테스트 사례를 통해 버그가 신속 하 게 해결 될 가능성이 있습니다.

1. <a name="note-6" />[ *^* ](#ref-6) 작은 수의 파일을 새로운 솔루션에 추가 하 여 문제를 재현할 수 _없는_ 경우 전체 앱에 대 한 전체 솔루션 폴더를 압축 하 고 연결할 수 있습니다. Zip 파일의 크기를 더 작게 만들려면 `bin`, `obj`, `Components`및 `packages` 폴더를 삭제 하십시오. IDE 및 빌드 프로세스는 일반적으로 필요에 따라 이러한 폴더의 콘텐츠를 복원 하거나 다시 만듭니다. 결과 솔루션이 원래 문제를 여전히 보여 주는 한 프로젝트에서 원하는 수 만큼 코드 및 리소스 파일을 삭제할 수도 있습니다.
