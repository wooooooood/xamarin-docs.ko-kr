---
title: 바인딩 문제 해결
description: 이 가이드는 Objective-c 라이브러리 바인딩 문제가 있을 경우 수행할 작업을 설명 합니다. 특히, 누락 된 바인딩, 바인딩 및 버그를 보고 하려면 null을 전달 하는 경우 인수 예외를 설명 합니다.
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: aaceada84b151856506ede66907274e2457c23d4
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854807"
---
# <a name="binding-troubleshooting"></a>바인딩 문제 해결

MacOS (OS X 이전의 함)에 대 한 바인딩 문제 해결에 대 한 몇 가지 팁 Xamarin.Mac의 Api입니다.

## <a name="missing-bindings"></a>누락 된 바인딩

Xamarin.Mac을 사용 하면 Apple Api의 대부분을 하는 동안 때로는 해야 할 수 있습니다 바인딩이 없는 일부 Apple API를 호출 하려면 아직 있습니다. 다른 경우에 타사 C/Objective-c로 호출 해야 한다는 Xamarin.Mac 바인딩의 범위 외부입니다.

Apple API를 사용 하 여 처리 하는 경우 Xamarin는 적중할 API의 섹션을 검사 한 아직 없는 것을 알 수 있도록 하는 첫 번째 단계가입니다. [버그를 파일링](#reporting-bugs) 누락 API 유의 해야 합니다. 고객의 보고서에서는 다음에 작동 하는 Api를 우선 순위를 사용 합니다. 또한 Business 또는 Enterprise 라이선스가 있는 경우 진행률을 차단 하는 바인딩의이 부족도의 지침을 따르세요 [지원](http://xamarin.com/support) 티켓을 제출 합니다. 바인딩 설계 수 없습니다. 하지만 경우에 따라 해결할 수 있습니다 작업.

에 알립니다 Xamarin (있는 경우)에 누락 된 바인딩, 직접 바인딩 고려해 야 할 다음 단계는입니다. 전체 가이드를 있다고 [여기](~/cross-platform/macios/binding/overview.md) 및 일부 비공식 설명서 [여기](http://brendanzagaeski.appspot.com/xamarin/0002.html) 손으로 Objective-c 바인딩 배치에 대 한 합니다. C API를 호출 하는 경우 C#의 P/Invoke 메커니즘을 사용 하면, 문서가 [여기](http://www.mono-project.com/docs/advanced/pinvoke/)합니다.

작업 하려는 경우 바인딩에서 직접 주의 바인딩에 실수가 모든 종류의 흥미로운 네이티브 런타임 충돌을 생성할 수 있습니다. C# 서명을 인수의 수와 각 인수의 크기에 기본 시그니처와 일치 하는 주의 해야 특히 합니다. 이렇게 하지 않으면 메모리 및/또는 스택 손상 될 수 있습니다 하 고 나중에 즉시 또는 임의의 시점에 충돌 하거나 데이터를 손상 시킬 수 있습니다.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>인수 예외를 바인딩에 null을 전달 하는 경우

Xamarin은 고가용성을 제공 하 게 작동 하는 동안 품질 및 Apple Api에 대 한 거친된 바인딩을 경우에 따라 실수를 쪽지의 버그입니다. 실행할 수 있는 가장 일반적인 문제는 API를 throw `ArgumentNullException` 전달 하는 경우 null에서 기본 API를 사용 하는 경우 `nil`합니다. 기본 헤더 파일 API를 자주 정의 Api nil 적용 하 고에 전달 하는 경우 충돌 하는 데 충분 한 정보를 제공 하지 않습니다.

경우에 실행 하는 경우 전달 하는 경우 `null` throw는 `ArgumentNullException` , 다음이 단계를 수행 해야 하는 것이 생각 하지만:

1. Apple 설명서 및/또는 허용 하는 개념 증명을 찾을 수 있는지 확인 하려면 예제 확인 `nil`합니다. Objective-c에 익숙한 경우 확인을 위해 작은 테스트 프로그램을 작성할 수 있습니다.
2. [버그를 파일링](#reporting-bugs)합니다.
3. 버그를 해결할 수 있습니다? API를 호출 하지 않아도 경우 `null`, 한 호출에 따라 간단한 null 검사를 쉽게 해결할 수 있습니다.
4. 그러나 일부 Api off로 설정 하거나 일부 기능을 사용 하지 않도록 설정 하려면 null 전달을 해야 합니다. 이러한 경우에 해결할 수 있습니다 문제 어셈블리 브라우저를 가져와서 (참조 [지정 된 선택기에 대 한 C# 멤버 찾기](~/mac/app-fundamentals/mac-apis.md#finding_selector))를 복사 하는 바인딩 및 null 검사를 제거 합니다. 이렇게 하면 복사 바인딩을 업데이트 되 고 수정 Xamarin.Mac에서 수신 하지 않습니다 하 고이 짧은 기간 동안 해결 간주 해야 하는 경우 버그 (2 단계)를 파일에 있는지 확인 하십시오.

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>버그 보고

여러분의 의견은 소중 합니다. Xamarin.Mac 사용 하 여 문제가 경우:

- [Xamarin.Mac 포럼](https://forums.xamarin.com/categories/mac) 확인
- [문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues) 검색 
- GitHub 문제로 전환하기 전에 Xamarin 문제가 [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi)에서 추적되었습니다. 여기서 일치하는 문제를 검색해 보세요.
- 일치하는 문제를 찾을 수 없는 경우 [GitHub 문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 제출하세요.

GitHub 문제는 모두 공용입니다. 설명 또는 첨부 파일을 숨길 수 없습니다. 

다음 정보를 가능한 많이 포함하세요.

- 문제를 재현하는 간단한 예제. 문제를 재현할 수 있다면 **매우 유용합니다**. 
- 크래시의 전체 스택 추적.
- 크래시 주변의 C# 코드. 

## <a name="related-links"></a>관련 링크

- [Objective-c 바인딩 라이브러리를 빌드할 Xamarin University 과정:](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University 과정: 목표 Sharpie 사용 하 여 Objective-c 바인딩 라이브러리를 빌드](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
