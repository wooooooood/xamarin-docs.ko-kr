---
title: 바인딩 문제 해결
description: 이 가이드에서는 목표-C 라이브러리를 바인딩하는 데 어려움이 있는 경우 수행할 작업에 대해 설명 합니다. 특히 바인딩에 null을 전달 하는 경우의 인수 예외 및 버그를 보고 하는 경우 누락 된 바인딩에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
author: davidortinau
ms.author: daortin
ms.date: 10/19/2016
ms.openlocfilehash: e43e32b2ad598a7c80e04d8e28d67e85d5a0f9f5
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84570949"
---
# <a name="binding-troubleshooting"></a>바인딩 문제 해결

Xamarin.ios에서 macOS (이전의 OS X) Api에 대 한 바인딩 문제 해결에 대 한 몇 가지 팁입니다.

## <a name="missing-bindings"></a>누락 된 바인딩

Xamarin.ios는 많은 Apple Api를 다루지만, 아직 바인딩이 없는 일부 Apple API를 호출 해야 하는 경우가 있습니다. 다른 경우에는 Xamarin.ios 바인딩 범위 밖에 서 타사 C/목표 C를 호출 해야 합니다.

Apple API를 사용 하는 경우 첫 번째 단계는 아직 적용 되지 않은 API의 섹션에 대해 Xamarin을 인식 하도록 하는 것입니다. 누락 된 API에 대 [한 버그를 파일에](#reporting-bugs) 기록 합니다. 고객의 보고서를 사용 하 여 다음에 작업할 Api의 우선 순위를 지정 합니다. 또한 비즈니스 또는 기업 라이선스를 보유 하 고 있으며,이로 인해 진행 상황이 차단 되는 경우에는 [지원](https://visualstudio.microsoft.com/vs/support/) 의 지침에 따라 티켓을 제출 해야 합니다. 바인딩을 사용할 수 없지만 경우에 따라 해결할 수 있습니다.

누락 된 바인딩의 Xamarin (해당 하는 경우)에 게 알리려면 다음 단계는 직접 바인딩을 고려 하는 것입니다. [여기](~/cross-platform/macios/binding/overview.md) 에서 전체 가이드를 제공 하 고, [여기](https://brendanzagaeski.appspot.com/xamarin/0002.html) 에는 목표-C 바인딩을 직접 래핑하는 몇 가지 비공식 설명서가 있습니다. C API를 호출 하는 경우 c #의 P/Invoke 메커니즘을 사용할 수 있습니다. 설명서는 [여기](https://www.mono-project.com/docs/advanced/pinvoke/)에 있습니다.

바인딩에 대해 작업을 수행 하기로 결정 한 경우 바인딩의 실수는 네이티브 런타임에서 모든 종류의 흥미로운 충돌을 생성할 수 있습니다. 특히 c #의 시그니처는 인수 개수 및 각 인수의 크기에 해당 하는 네이티브 시그니처와 일치 해야 합니다. 이렇게 하지 않으면 메모리 및/또는 스택이 손상 될 수 있으며 즉시 또는 나중에 또는 손상 된 데이터의 임의 지점에서 충돌을 발생 시킬 수 있습니다.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>바인딩에 null을 전달할 때 인수 예외

Xamarin은 Apple Api에 대 한 고품질 및 잘 테스트 된 바인딩을 제공 하는 데 사용할 수 있지만 실수 및 버그가 지연 되는 경우도 있습니다. 지금까지 실행할 수 있는 가장 일반적인 문제는 `ArgumentNullException` 기본 api가 허용 하는 경우 null을 전달할 때 발생 하는 api입니다 `nil` . API를 정의 하는 네이티브 헤더 파일은 종종 nil을 허용 하는 Api에 대 한 충분 한 정보를 제공 하지 않으며,이를 전달 하는 경우 충돌이 발생 합니다.

를 전달 하는 경우를 throw 하는 경우를 `null` throw `ArgumentNullException` 하지만 작동 해야 한다고 생각 하는 경우 다음 단계를 수행 합니다.

1. Apple 설명서 및/또는 예제를 확인 하 여 허용 되는 증명을 찾을 수 있는지 확인 `nil` 합니다. 목표-C에 익숙해지면 작은 테스트 프로그램을 작성 하 여 확인할 수 있습니다.
2. [버그를 파일로 만듭니다](#reporting-bugs).
3. 버그를 해결할 수 있나요? 를 사용 하 여 API 호출을 방지할 수 있는 경우 `null` 호출에 대 한 간단한 null 검사를 쉽게 해결할 수 있습니다.
4. 그러나 일부 Api를 해제 하거나 비활성화 하려면 null을 전달 해야 합니다. 이러한 경우에는 어셈블리 브라우저를 열고 ( [지정 된 선택기에 대 한 c # 멤버 찾기](~/mac/app-fundamentals/mac-apis.md#finding_selector)참조) 바인딩을 복사 하 고 null 검사를 제거 하 여 문제를 해결할 수 있습니다. 복사 된 바인딩이 Xamarin.ios에서 만든 업데이트 및 픽스를 수신 하지 않으므로이 작업을 수행 하는 경우 버그를 파일에 입력 해야 합니다 (2 단계).

<a name="reporting-bugs"></a>

## <a name="reporting-bugs"></a>버그 보고

Microsoft는 사용자의 의견을 소중하게 생각합니다. Xamarin.ios에서 문제가 발견 되 면 다음을 수행 합니다.

- [Xamarin.Mac 포럼](https://forums.xamarin.com/categories/xamarin-mac) 확인
- [문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues) 검색
- GitHub 문제로 전환하기 전에 Xamarin 문제가 [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi)에서 추적되었습니다. 여기서 일치하는 문제를 검색해 보세요.
- 일치하는 문제를 찾을 수 없는 경우 [GitHub 문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 제출하세요.

GitHub 문제는 모두 공용입니다. 설명 또는 첨부 파일을 숨길 수 없습니다.

다음 정보를 가능한 많이 포함하세요.

- 문제를 재현하는 간단한 예제. 문제를 재현할 수 있다면 **매우 유용합니다**.
- 크래시의 전체 스택 추적.
- 크래시 주변의 C# 코드.
