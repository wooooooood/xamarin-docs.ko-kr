---
title: Mac API
description: 이 문서를 Objective-c 선택기를 읽는 방법 및 해당 C# 메서드를 찾는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/02/2017
ms.openlocfilehash: 0344fecb9a8d64a680bb11689f56cf074d952f4e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="mac-apis"></a>Mac API

## <a name="overview"></a>개요

많은 Xamarin.Mac를 사용 하 여 개발 시간이 대 한 생각, 읽기 및 기본 Objective-c Api와 관계 없이 C#에서 작성 수 있습니다. 그러나 때로는 Apple에서 API 설명서를 읽을 귀하의 문제에 대 한 솔루션에 스택 오버플로에서 답변을 변환 옮기거나 해야 기존 샘플을 비교 합니다.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>위험한 것으로 충분 한 Objective C 읽기

경우에 따라 Objective-c 정의 읽을 해야 할 수 있습니다 또는 메서드를 호출 하 고 해당 하는 C# 메서드로 변환 합니다. Objective C 함수 정의에서 참조 하 고 분리 하는 조각을 살펴보겠습니다. 이 메서드 (한 *선택기* Objective C의)에서 확인할 수 있습니다 `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

선언에서 오른쪽 왼쪽 읽을 수 있습니다.

- `-` 접두사 인스턴스 (비정적) 메서드는 것을 의미 합니다. + (정적) 클래스 메서드가
- `(BOOL)` 반환 형식 (C#에서 bool)
- `canDragRowsWithIndexes` 이름의 첫 번째 부분이입니다.
- `(NSIndexSet *)rowIndexes` 첫 번째 매개 변수 이며 함께 입력 했습니다. 첫 번째 매개 변수는 형식: `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` 두 번째 매개 변수 및 해당 형식이 됩니다. 모든 매개 변수는 첫 번째 이후의 형식은 다음과 같습니다. `selectorPart:(Type) pararmName`
- 이 메시지 선택기의 전체 이름은: `canDragRowsWithIndexes:atPoint:`합니다. 참고는 `:` 끝-것이 중요 합니다.
- 실제 Xamarin.Mac C# 바인딩이입니다. `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

이 선택기 호출에는 동일한 방식으로 읽을 수 있습니다.

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- 인스턴스 `v` 은 하는 데 해당 `canDragRowsWithIndexes:atPoint` 두 개의 매개 변수를 사용 하 여 호출 하는 선택기 `set` 및 `point`, 전달 합니다.
- C#에서 메서드 호출 다음과 같이 보입니다. `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>지정 된 선택기에 대 한 C# 멤버 찾기

호출 해야 Objective-c 선택기를 찾았으면 했으므로 다음 단계는에 매핑된다 C#의 동등한 멤버입니다. 네 가지 방법으로 시도할 수 있습니다 (계속는 `NSTableView CanDragRows` 예제):

1. 자동 완성 목록 신속 하 게 검색 이름이 동일한 용도로 사용 합니다. 인스턴스는 우리가 알고 `NSTableView` 입력할 수 있습니다.

    - `NSTableView x;`
    - `x.` [ctrl + 공간 목록이 표시 되지 않는 경우).
    - `CanDrag` [입력]
    - 메서드를 마우스 오른쪽 단추로 클릭, 비교할 수 있습니다 어셈블리 브라우저를 열려면 선언으로 이동 된 `Export` 문제의 선택기에 특성

2. 전체 클래스 바인딩을 검색 합니다. 인스턴스는 우리가 알고 `NSTableView` 입력할 수 있습니다.

    - `NSTableView x;`
    - 마우스 오른쪽 단추로 클릭 `NSTableView`, 어셈블리 브라우저에 선언으로 이동
    - 문제의 선택기에 대 한 검색

3. 사용할 수는 [Xamarin.Mac API 온라인 설명서](https://developer.xamarin.com/api/root/monomac-lib/) 합니다.

4. Miguel Xamarin.Mac Api "Rosetta 돌" 보기를 제공 [여기](http://tirania.org/tmp/rosetta.html) 는 지정 된 API에 대 한 검색할 수 있습니다. API AppKit 또는 macOS 특정 아닌 경우 있습니다 알 수 있습니다.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
