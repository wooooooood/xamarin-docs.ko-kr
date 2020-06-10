---
title: Xamarin.ios 개발자를 위한 macOS Api
description: '이 문서에서는 목적-C 선택기를 읽고 Xamarin.ios 앱에서 해당 c # 메서드를 찾는 방법을 설명 합니다.'
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/02/2017
ms.openlocfilehash: c8fd877c6addac7dd865d8464e24a455b2f1aa88
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573939"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>Xamarin.ios 개발자를 위한 macOS Api

## <a name="overview"></a>개요

Xamarin.ios를 사용 하 여 개발 하는 데 많은 시간이 소요 되는 경우 기본 목표-C Api에 대 한 많은 걱정 없이 c #으로 생각 하 고 읽고 쓸 수 있습니다. 그러나 경우에 따라 Apple에서 API 설명서를 읽고 문제에 대 한 Stack Overflow의 답변을 솔루션으로 변환 하거나 기존 샘플과 비교 해야 합니다.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>위험에 대 한 충분 한 목표를 읽도록 합니다.

경우에 따라 목표 C 정의 또는 메서드 호출을 읽고 해당 하는 c # 메서드로 변환 해야 합니다. 목표-C 함수 정의를 살펴보고 부분을 분할 하는 방법을 살펴보겠습니다. 이 메서드 (목표-C의 *선택기* )는 다음 위치에서 찾을 수 있습니다 `NSTableView` .

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

선언은 왼쪽에서 오른쪽으로 읽을 수 있습니다.

- `-`접두사는 인스턴스 (비정적) 메서드 임을 의미 합니다. +는 클래스 (정적) 메서드 임을 의미 합니다.
- `(BOOL)`반환 형식 (c #의 경우 bool)입니다.
- `canDragRowsWithIndexes`이름의 첫 번째 부분입니다.
- `(NSIndexSet *)rowIndexes`는 첫 번째 매개 변수이 고이 매개 변수의 형식은입니다. 첫 번째 매개 변수는 다음과 같은 형식입니다.`(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint`는 두 번째 매개 변수 및 해당 형식입니다. 첫 번째 뒤의 모든 매개 변수는 형식입니다.`selectorPart:(Type) pararmName`
- 이 메시지 선택기의 전체 이름은 `canDragRowsWithIndexes:atPoint:` 입니다. 끝에는 `:` 이 중요 합니다.
- 실제 Xamarin.ios c # 바인딩은 다음과 같습니다.`bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

이 선택기 호출은 다음과 같은 방식으로 읽을 수 있습니다.

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- 인스턴스는 `v` `canDragRowsWithIndexes:atPoint` 두 매개 변수 및가 전달 된 라는 선택기를 사용 `set` `point` 합니다.
- C #에서 메서드 호출은 다음과 같습니다.`x.CanDragRows (set, point);`

<a name="finding_selector"></a>

## <a name="finding-the-c-member-for-a-given-selector"></a>지정 된 선택기에 대 한 c # 멤버 찾기

호출 해야 하는 목표-C 선택기를 찾았지만 다음 단계에서는 해당 c # 멤버에 매핑합니다. 다음 네 가지 방법으로 시도할 수 있습니다. 예를 계속 합니다 `NSTableView CanDragRows` .

1. 자동 완성 목록을 사용 하 여 같은 이름의 항목을 빠르게 검색할 수 있습니다. 이 인스턴스는의 인스턴스 이므로 다음을 `NSTableView` 입력할 수 있습니다.

    - `NSTableView x;`
    - `x.`[ctrl + space (목록이 표시 되지 않는 경우).
    - `CanDrag`들어가서
    - 메서드를 마우스 오른쪽 단추로 클릭 하 고 선언으로 이동 하 여 해당 `Export` 특성을 문제의 선택기와 비교할 수 있는 어셈블리 브라우저를 엽니다.

2. 전체 클래스 바인딩을 검색 합니다. 이 인스턴스는의 인스턴스 이므로 다음을 `NSTableView` 입력할 수 있습니다.

    - `NSTableView x;`
    - 마우스 오른쪽 단추 `NSTableView` 를 클릭 하 고 선언으로 이동 어셈블리 브라우저
    - 문제의 선택기 검색

3. [XAMARIN.IOS API 온라인 설명서](https://docs.microsoft.com/dotnet/api/?view=xamarinmac-3.0) 를 사용할 수 있습니다.

4. Miel el은 지정 된 API를 검색할 수 있는 [Xamarin.ios api의](https://tirania.org/tmp/rosetta.html) "Rosetta 석재" 보기를 제공 합니다. API가 AppKit 또는 macOS와 관련 되지 않은 경우 찾을 수 있습니다.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
