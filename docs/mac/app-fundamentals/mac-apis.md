---
title: macOS Xamarin.Mac 개발자를 위한 Api
description: 이 문서에서는 Objective-c 선택기를 읽는 방법 및 Xamarin.Mac 앱에서 해당 C# 메서드를 찾는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/02/2017
ms.openlocfilehash: 6dfaa3c7bf988228bfbacefe7c8e7268edc8117a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994314"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>macOS Xamarin.Mac 개발자를 위한 Api

## <a name="overview"></a>개요

대부분의 Xamarin.Mac을 사용 하 여 개발 시간에 대 한 인지, 읽기 및 기본 Objective C Api를 사용 하 여 관계 없이 C#에서 작성 수 있습니다. 그러나 경우에 따라 Apple API 설명서를 읽을 Stack Overflow에서 답변을 솔루션에 문제에 대 한 변환 옮기거나 해야 기존 샘플 비교할 합니다.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>위험한 것으로 충분 한 Objective C 읽기

경우에 따라 읽기는 Objective-c로 정의 해야 할 수 있습니다 또는 메서드 호출 및 해당 하는 C# 메서드를 변환 합니다. Objective C 함수 정의 확인 하 고 부분 세분화 보겠습니다. 이 메서드 (한 *선택기* Objective C에서)에서 확인할 수 있습니다 `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

선언은 오른쪽에 왼쪽 읽을 수 있습니다.

- `-` 접두사 인스턴스 (비정적) 메서드인 것을 의미 합니다. + (정적) 클래스 메서드를 않음을 의미
- `(BOOL)` 반환 형식 (C#에서 bool)
- `canDragRowsWithIndexes` 이름의 첫 번째 부분이입니다.
- `(NSIndexSet *)rowIndexes` 첫 번째 매개 변수 이며 함께 입력 합니다. 첫 번째 매개 변수 형식입니다. `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` 두 번째 매개 변수 및 해당 형식이 됩니다. 첫 번째 형식은 후 모든 매개 변수: `selectorPart:(Type) pararmName`
- 이 메시지 선택기의 전체 이름은: `canDragRowsWithIndexes:atPoint:`합니다. 참고는 `:` 끝-것이 중요 합니다.
- 실제 Xamarin.Mac C# 바인딩은 다음과 같습니다. `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

이 선택기 호출에는 동일한 방식으로 읽을 수 있습니다.

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- 인스턴스 `v` 는 해당 `canDragRowsWithIndexes:atPoint` 두 매개 변수를 사용 하 여 호출 하는 선택기 `set` 및 `point`, 전달 된입니다.
- C#에서 메서드 호출은 다음과 같습니다. `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>지정 된 선택기에 대 한 C# 멤버 찾기

호출 해야 하는 Objective-c 선택기를 찾았으면 했으므로 다음 단계는 매핑는 해당 하는 C# 멤버에입니다. 네 가지 방법으로 시도할 수 있습니다 (사용 하 여 계속 합니다 `NSTableView CanDragRows` 예제):

1. 자동 완성 목록을 사용 하 여 동일한 이름의 항목을 빠르게 검사할 합니다. 알 수 있기 때문에 인스턴스에 `NSTableView` 입력할 수 있습니다.

    - `NSTableView x;`
    - `x.` [ctrl + 스페이스바 목록에 나타나지 않으면).
    - `CanDrag` [입력]
    - 메서드를 마우스 오른쪽 단추로 클릭, 어셈블리 브라우저 비교할 수 있습니다를 열려면 선언으로 이동 합니다 `Export` 문제의 선택기를 특성

2. 전체 클래스 바인딩을 검색 합니다. 알 수 있기 때문에 인스턴스에 `NSTableView` 입력할 수 있습니다.

    - `NSTableView x;`
    - 마우스 오른쪽 단추로 클릭 `NSTableView`, 어셈블리 브라우저 선언으로 이동
    - 문제의 선택기에 대 한 검색

3. 사용할 수는 [온라인 설명서 Xamarin.Mac API](https://docs.microsoft.com/dotnet/api/?view=xamarinmac-3.0) 합니다.

4. Miguel Xamarin.Mac api "전설의 Stone" 보기가 [여기](http://tirania.org/tmp/rosetta.html) 는 지정된 된 API를 통해 검색할 수 있습니다. API AppKit 또는 macOS 관련 없는 경우 있습니다 알 수 있습니다.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
