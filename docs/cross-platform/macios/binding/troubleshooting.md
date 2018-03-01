---
title: "바인딩 문제 해결"
description: "이 가이드는 Objective C 라이브러리 바인딩 문제가 있을 경우 수행할 작업을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: 2db7fe30f05224f6b74b4d2189606da59946bda0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="binding-troubleshooting"></a>바인딩 문제 해결

MacOS (이전의 OS X)에 대 한 바인딩을 문제 해결을 위한 몇 가지 팁 Xamarin.Mac의 Api입니다.

## <a name="missing-bindings"></a>바인딩 누락

Xamarin.Mac 대부분의 Apple Api를 다루지만 경우도 있습니다 할 수 없는 경우 바인딩을 일부 Apple API를 호출 하 아직 합니다. 제 3 자 C/Objective-c 호출 해야 경우에 따라 Xamarin.Mac 바인딩의 범위를 벗어나는 것입니다.

Apple API와 함께 처리 하는 경우 Xamarin을 적중할 API의 섹션 있습니까 아직에 있는 알 수 있도록는 첫 번째 단계가입니다. [버그를 보고할](#reporting-bugs) 누락 된 API를 기록 합니다. 고객 으로부터 보고서에서는 다음에 파악 하는 Api의 우선 순위를 사용 합니다. 또한 비즈니스 또는 엔터프라이즈 라이선스가 있는 경우 진행률을 차단 하는 바인딩의이 처럼도의 지침에 따라 [지원](http://xamarin.com/support) 티켓을 파일에 있습니다. 바인딩 promise 없습니다 것 이지만 경우에 따라 해결할 수 있습니다는 작업.

하면 알림 Xamarin (있는 경우)의 누락 된 바인딩을, 다음 단계는 사용자가 직접 바인딩 하는 것이 좋습니다.를 합니다. 전체 가이드는 [여기](~/cross-platform/macios/binding/overview.md) 및 일부 비공식 설명서 [여기](http://brendanzagaeski.appspot.com/xamarin/0002.html) Objective-c 바인딩을 직접 줄 바꿈 합니다. C#의 P/Invoke 메커니즘을 사용할 수 있는데, 설명서는 C API를 호출 하는 경우 [여기](http://www.mono-project.com/docs/advanced/pinvoke/)합니다.

작동 하도록 결정 한 경우의 바인딩에 직접 유의 실수 바인딩에 모든 종류의 흥미로운 네이티브 런타임 작동 중단을 생성할 수 있습니다. 특히, 기해야는 C#의 서명을 시그니처와 일치 하는 기본 인수의 수와 각 인수의 크기입니다. 이렇게 하지 않으면 메모리 및/또는 스택 손상 될 수 있습니다 및 즉시 또는 특정 임의의 시점에 나중에 충돌 하거나 데이터를 손상 시킬 수 있습니다.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>인수 예외를 바인딩에 null을 전달 하는 경우

Xamarin 작동 높은 수 있도록 하는 동안 품질 및 Apple Api에 대 한 잘 테스트 된 바인딩을 경우가 실수를 버그에 지연 합니다. 충돌할 수 있는 가장 일반적인 문제점은 API를 throw `ArgumentNullException` 전달 하는 경우 null에서 기본 API에서 승인 `nil`합니다. 종종 API를 정의 하는 기본 헤더 파일에서 Api nil 적용 하 고에 전달 하는 경우 충돌 하는 충분 한 정보를 제공 하지 않습니다.

경우에 실행 하는 경우를 전달 하는 경우 `null` throw 한 `ArgumentNullException` 작업, 다음이 단계를 수행 해야 것 같지만:

1. Apple 설명서 및/또는 증명을 허용 하는 경우 찾을 수 있습니다를 참조 하는 예제를 확인 `nil`합니다. Objective C에 익숙한 경우 확인 하는 작은 테스트 프로그램을 작성할 수 있습니다.
2. [버그를 보고할](#reporting-bugs)합니다.
3. 버그를 해결할 수 있습니다. 사용 하 여 API를 호출 하지 않는 경우 `null`, 호출 주위 간단한 null 검사를 쉽게 해결할 수 있습니다.
4. 그러나 일부 Api 해제 하거나 일부 기능을 사용 하지 않도록 설정 하는 null 전달을 해야 합니다. 이러한 경우 해결할 수 있습니다 문제 어셈블리 브라우저를 전환 하 여 (참조 [지정 된 선택기에 대 한 C# 멤버를 찾기](~/mac/app-fundamentals/mac-apis.md#finding_selector)), 바인딩 및 null 검사 제거를 복사 합니다. 이렇게 하면 복사 된 바인딩을 업데이트 및 Xamarin.Mac에서 수행 된 수정 사항 알림과 처럼 이므로 라이선스 전용 클러스터이 잠시 동안 해결 하는 경우 버그 (2 단계)를 파일에 있는지 확인 하십시오.

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>버그를 보고

여러분의 의견은 소중 합니다. Xamarin.Mac 문제가 있는지 확인 하는 경우:

- 확인 된 [Xamarin.Mac 포럼](https://forums.xamarin.com/categories/mac)
- 검색 된 [문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues) 
- GitHub 문제를 전환 하기 전에 Xamarin 문제에서 추적 된 [: Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi)합니다. 문제 일치에 대 한 있습니다를 검색 해 보십시오.
- 일치 하는 문제를 찾을 수 없는 경우에 새 문제를 제출 하세요는 [GitHub 문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues/new)합니다.

GitHub 문제는 모든 공용입니다. 설명 또는 첨부 파일을 숨기려고 하는 것이 불가능 합니다. 

다음을 가능한 많이 포함 하세요.

- 이 문제를 재현 하는 간단한 예제입니다. 이 **매우 유용한** 가능 합니다. 
- 크래시의 전체 스택 추적입니다.
- C# 코드 주변 충돌 합니다. 
